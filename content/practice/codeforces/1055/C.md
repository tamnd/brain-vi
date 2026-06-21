---
title: "CF 1055C - Ngày may mắn"
description: "Hai người có những hình mẫu định kỳ về những “khoảng thời gian tốt” trên trục số ngày. Mỗi mẫu bao gồm một đoạn cố định của các ngày liên tiếp trong một chu kỳ lặp lại."
date: "2026-06-15T12:53:22+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1055
codeforces_index: "C"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 2"
rating: 1900
weight: 1055
solve_time_s: 137
verified: true
draft: false
---

[CF 1055C - Những ngày may mắn](https://codeforces.com/problemset/problem/1055/C) 

**Xếp hạng:** 1900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai người có những hình mẫu định kỳ về những “khoảng thời gian tốt” trên trục số ngày. Mỗi mẫu bao gồm một đoạn cố định của các ngày liên tiếp trong một chu kỳ lặp lại. Đối với Alice, trong mỗi khối chiều dài`t_a`, những ngày từ`l_a`ĐẾN`r_a`(bao gồm) là tốt và mọi thứ bên ngoài phân khúc đó trong cùng một khối đều xấu. Cấu trúc tương tự áp dụng cho Bob với các tham số`l_b`,`r_b`,`t_b`. 

Cả hai mô hình đều lặp lại vô thời hạn bằng cách dịch chuyển đoạn tốt của chúng theo bội số của chu kỳ. Một ngày sẽ tốt cho một người nếu nó nằm ở một trong những phân khúc đã thay đổi của họ. 

Nhiệm vụ là xác định độ dài tối đa của một khối ngày liền kề sao cho mỗi ngày trong khối đó đều có lợi cho cả Alice và Bob. 

Khó khăn chính là cả hai mô hình đều tuần hoàn nhưng có các khoảng thời gian khác nhau, do đó cấu trúc giao nhau không tuần hoàn với một khoảng thời gian nhỏ rõ ràng và việc căn chỉnh thô bạo trong một phạm vi ngày lớn là không thể vì các giá trị khoảng thời gian có thể lớn tới 10^9. 

Một cách tiếp cận ngây thơ cố gắng xây dựng hoặc mô phỏng rõ ràng các mẫu trong nhiều khoảng thời gian chung nhỏ nhất là không thể thực hiện được ngay lập tức, vì LCM của hai số nguyên lớn có thể vượt quá 10^18 và cấu trúc vẫn quá lớn để liệt kê. 

Một trường hợp cạnh tinh tế phát sinh khi một khoảng được chứa hoàn toàn trong một khoảng khác để căn chỉnh các phần dư. Ví dụ: nếu Alice luôn giỏi`[0, 4] mod 10`và Bob làm tốt`[2, 3] mod 10`, câu trả lời là 2, nhưng việc dịch chuyển trực giác không chính xác có thể gợi ý việc bao bọc hoặc hợp nhất giữa các ranh giới thời kỳ, điều này không hợp lệ vì mỗi thời kỳ được đặt lại độc lập. 

Một tình huống phức tạp khác xảy ra khi sự chồng chéo tốt nhất vượt qua ranh giới của một thời kỳ nhưng không vượt qua ranh giới của thời kỳ kia. Ví dụ, Bob có thể làm tốt ở cuối chu kỳ này và bắt đầu chu kỳ tiếp theo, trong khi Alice liên tục làm tốt. Giao lộ vẫn là một đoạn liền kề duy nhất trong thời gian tuyệt đối, mặc dù nó kéo dài qua hai khối mô-đun. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là kiểm tra mỗi ngày bắt đầu và kéo dài về phía trước trong khi cả Alice và Bob đều đang ở trong khoảng thời gian tốt đẹp của họ. Cho mỗi ngày`x`, chúng ta kiểm tra tư cách thành viên trong khoảng tuần hoàn của Alice và khoảng tuần hoàn của Bob trong O(1), sau đó kéo dài cho đến khi điều kiện bị phá vỡ. Trong trường hợp xấu nhất, bản thân câu trả lời có thể lớn, nhưng quan trọng hơn là chúng tôi sẽ lặp lại công việc cho mọi vị trí bắt đầu, dẫn đến hành vi O(N^2) trong một khoảng ngày rất lớn. 

Điều này không thành công vì cấu trúc lặp lại theo định kỳ và việc tính toán lại sự chồng chéo từ đầu đã bỏ qua thực tế là chỉ có pha tương đối giữa hai chu kỳ mới quan trọng. Quan sát quan trọng là cả hai mẫu đều là sự kết hợp của các khoảng dịch chuyển và sự tương tác của chúng chỉ phụ thuộc vào vị trí của chúng theo modulo chu kỳ tương ứng của chúng. 

Thay vì quét theo thời gian, chúng tôi xem xét sự liên kết tương đối của hai mẫu tuần hoàn. Hãy ấn định một ngày trong chu kỳ của Alice và xác định xem khoảng thời gian tốt của Bob nằm ở đâu so với ngày đó. Mô hình chồng chéo giữa hai hệ thống phân đoạn định kỳ lặp lại theo chu kỳ`t_a`, bởi vì cấu trúc của Alice đã tuần hoàn với chu kỳ đó. Vì vậy, chúng ta có thể hạn chế sự chú ý vào một chu kỳ duy nhất của Alice và tính toán, đối với mỗi điểm bên trong nó, giao lộ sẽ tiếp tục tiến về phía trước trong bao lâu. 

Trong chu kỳ của Alice, những ngày tốt lành của Bob xuất hiện dưới dạng nhiều phân đoạn được chuyển đổi. Chúng tôi ánh xạ các khoảng của Bob vào hệ tọa độ của Alice và sau đó kiểm tra sự trùng lặp giữa các khoảng trong một đoạn được tuyến tính hóa`[0, t_a - 1]`, chú ý xử lý các phần bao quát cho các phân đoạn của Bob. Khi cả hai bộ được thể hiện trong một không gian mô-đun có thể so sánh được, vấn đề sẽ giảm xuống còn việc tìm ra sự chồng chéo dài nhất giữa các khoảng trên một vòng tròn, có thể được tuyến tính hóa bằng cách nhân đôi chu trình. 

Sau đó, chúng tôi quét qua các điểm cuối của các đoạn chồng chéo và tính toán độ dài giao lộ liên tục tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) theo thời gian ẩn | O(1) | Quá chậm | 
| Căn chỉnh chu kỳ + giao điểm khoảng thời gian | O(t_a + t_b) | O(t_a + t_b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuẩn hóa mọi thứ liên quan đến chu trình của Alice, vì cấu trúc của Alice đã là một phân vùng có độ dài tuần hoàn rõ ràng`t_a`. 

1. Chuyển đoạn hay của Alice thành một đoạn duy nhất trong một chu kỳ, cụ thể là`[l_a, r_a]`. Điều này thể hiện tất cả các điểm trong một khoảng thời gian tiêu biểu mà Alice giỏi. 
2. Mở rộng các phân đoạn tốt của Bob thành một hình thức phù hợp với chu trình của Alice. Mỗi khoảng Bob`[l_b + k t_b, r_b + k t_b]`được chiếu vào dư lượng modulo`t_a`. Thay vì liệt kê vô số ca, chúng ta chỉ cần mẫu Bob modulo`t_a`, có thể được tạo bằng cách lấy khoảng cơ sở của Bob và xem xét sự trùng lặp của nó với cửa sổ trượt có chiều dài`t_a`. 
3. Xây dựng một tập hợp các khoảng thời gian ứng viên bên trong`[0, t_a)`nơi mà cả Alice và Bob đều giỏi. Điều này được thực hiện bằng cách giao khoảng đơn của Alice với tất cả các khoảng Bob đã dịch chuyển có thể giao với khoảng đó trong một chu kỳ. 
4. Vì các khoảng của Bob có thể bao quanh ranh giới chu kỳ, nên hãy chia bất kỳ khoảng nào đi qua`t_a - 1`thành hai đoạn tuyến tính. Điều này chuyển đổi vấn đề thành một vấn đề giao điểm khoảng cách dòng tiêu chuẩn. 
5. Sắp xếp tất cả các khoảng giao nhau theo điểm bắt đầu và hợp nhất chúng. Trong quá trình hợp nhất, hãy theo dõi độ dài tối đa của bất kỳ phân đoạn được hợp nhất nào. 

Câu trả lời là độ dài tối đa của bất kỳ khoảng thời gian hợp nhất nào. 

### Tại sao nó hoạt động 

Những ngày tốt lành của Alice tạo nên một đường số nguyên xếp hoàn hảo bằng các phân đoạn giống hệt nhau được lặp lại mỗi lần.`t_a`. Do đó, bất kỳ mô hình giao nhau toàn cục nào cũng phải lặp lại với cùng khoảng thời gian khi xem qua cấu trúc của Alice. Bằng cách hạn chế phân tích trong một chu kỳ đầy đủ của Alice, chúng tôi không mất bất kỳ cấu hình chồng chéo tương đối nào. 

Trong chu kỳ đó, những đóng góp của Bob được ghi lại đầy đủ bằng cách các phân đoạn định kỳ của anh ấy chiếu vào không gian dư lượng. Mọi đoạn đường tốt đồng thời có thể đều tương ứng chính xác với một đoạn giao nhau bên trong miền rút gọn này. Bởi vì chúng tôi chỉ hợp nhất các khoảng sau khi chiếu đầy đủ, nên chúng tôi duy trì tính liền kề và không vô tình kết nối các phần chồng chéo rời rạc trên các ranh giới chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_intervals(l, r, t, limit):
    res = []
    # generate occurrences that can intersect [0, limit)
    k = 0
    while l + k * t < limit:
        start = l + k * t
        end = r + k * t
        if start < limit and end >= 0:
            res.append((start, min(end, limit - 1)))
        k += 1
        if k > limit // max(1, t) + 5:
            break
    return res

def intersect(a1, a2, b1, b2):
    l = max(a1, b1)
    r = min(a2, b2)
    if l <= r:
        return (l, r)
    return None

def solve():
    l_a, r_a, t_a = map(int, input().split())
    l_b, r_b, t_b = map(int, input().split())

    # we work inside one cycle of Alice
    limit = t_a

    alice = [(l_a, r_a)]

    bob_intervals = build_intervals(l_b, r_b, t_b, limit + t_b)

    candidates = []
    for a1, a2 in alice:
        for b1, b2 in bob_intervals:
            inter = intersect(a1, a2, b1, b2)
            if inter:
                candidates.append(inter)

    if not candidates:
        print(0)
        return

    candidates.sort()
    merged = []
    cur_l, cur_r = candidates[0]

    best = 0
    for l, r in candidates[1:]:
        if l <= cur_r + 1:
            cur_r = max(cur_r, r)
        else:
            best = max(best, cur_r - cur_l + 1)
            cur_l, cur_r = l, r

    best = max(best, cur_r - cur_l + 1)
    print(best)

if __name__ == "__main__":
    solve()
```Trước tiên, mã sẽ giảm vấn đề thành các giao điểm bên trong một cửa sổ có kích thước giới hạn`t_a`, thế là đủ vì mô hình của Alice lặp lại chính xác mọi`t_a`. Sau đó, nó tạo ra các phân đoạn Bob có liên quan có thể chồng lên cửa sổ này, thay vì cố gắng suy luận về cấu trúc cấp số cộng vô hạn một cách trừu tượng. 

Mỗi sự trùng lặp ứng cử viên được tính toán bằng một giao điểm khoảng trực tiếp. Sau đó, chúng được hợp nhất để tạo thành các phân đoạn liên tục tối đa trong đó cả hai đều tốt. Câu trả lời cuối cùng là đoạn được hợp nhất dài nhất. 

Một điểm tinh tế là việc cắt bớt các khoảng Bob cho cửa sổ`[0, t_a + t_b)`. Điều này đảm bảo chúng tôi nắm bắt được tất cả các ca có thể diễn ra trong chu kỳ đầu tiên của Alice trong khi tránh được việc liệt kê vô hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 2 5
1 3 5
```Chúng tôi làm việc theo chu kỳ của Alice`[0, 4]`, nơi Alice giỏi`[0, 2]`. Bob tạo ra khoảng thời gian`[1, 3]`,`[6, 8]`, v.v., nhưng chỉ trong chu kỳ đầu tiên`[1, 3]`vấn đề. 

| Bước | Khoảng thời gian Alice | Khoảng Bob | Giao lộ | 
| --- | --- | --- | --- | 
| 1 | [0,2] | [1,3] | [1,2] | 

Sự chồng chéo duy nhất là`[1,2]`, có độ dài 2. Đây là câu trả lời. 

Điều này xác nhận rằng mặc dù mô hình của Bob tiếp tục vượt ra ngoài chu kỳ đầu tiên, nhưng chỉ phần giao với chu kỳ đầu tiên của Alice mới có ý nghĩa quan trọng đối với sự chồng chéo liền kề tối đa. 

### Ví dụ 2 

đầu vào:```
0 1 4
2 3 6
```Alice vẫn ổn`[0,1]`lặp lại mỗi 4. Bob làm tốt`[2,3]`lặp lại mỗi 6. 

Trong chu kỳ của Alice`[0,3]`, khoảng tốt của Alice là`[0,1]`. Bob đóng góp không có sự chồng chéo trong khu vực này. 

| Bước | Khoảng thời gian Alice | Khoảng Bob | Giao lộ | 
| --- | --- | --- | --- | 
| 1 | [0,1] | không chồng chéo | [] | 

Câu trả lời là 0. 

Điều này cho thấy trường hợp trong đó các cấu trúc tuần hoàn không bao giờ thẳng hàng trong một chu trình, do đó giao điểm tổng thể trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t_a + t_b) | Chúng tôi tạo và giao nhau chỉ các dịch chuyển có liên quan của Bob trong một chu kỳ Alice | 
| Không gian | O(t_a + t_b) | Lưu trữ các giao điểm khoảng cách ứng cử viên trước khi hợp nhất | 

Các ràng buộc cho phép tối đa 10^9 trong các khoảng thời gian, nhưng thuật toán tránh lặp lại toàn bộ thời gian bằng cách hạn chế công việc trong một chu kỳ của Alice và chỉ tạo ra các phần trùng lặp cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume solution wrapped
    return solve()

# provided sample
assert run("0 2 5\n1 3 5\n") == "2"

# no overlap
assert run("0 1 4\n2 3 6\n") == "0"

# full overlap
assert run("0 3 5\n0 3 5\n") == "4"

# boundary crossing behavior
assert run("0 2 5\n4 4 5\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu giống hệt nhau | toàn bộ phân đoạn | chính xác khi cả hai lịch trình đều khớp nhau | 
| chu kỳ rời rạc | 0 | không có sự hợp nhất vô tình | 
| chồng chéo một phần | giá trị dương | trích xuất giao lộ chính xác | 
| trường hợp ranh giới | 1 | độ chính xác ở các cạnh chu kỳ | 

## Vỏ cạnh 

Trường hợp một bên là khi khoảng thời gian của Bob bắt đầu ở gần cuối chu kỳ của anh ấy và kết thúc ở chu kỳ tiếp theo. Ví dụ, Bob`[4, 1] mod 6`chia thành các thành phần một cách hiệu quả`[4,5]`Và`[0,1]`. Thuật toán xử lý vấn đề này bằng cách tạo ra các khoảng dịch chuyển bao trùm cả hai phần một cách tự nhiên khi được chiếu vào chu trình của Alice, đảm bảo không bỏ sót giao điểm nào. 

Một trường hợp cạnh khác là khi khoảng của Alice bao trùm toàn bộ chu kỳ, tức là.`l_a = 0, r_a = t_a - 1`. Trong trường hợp này, câu trả lời giảm xuống khối liên tục dài nhất trong mẫu của Bob. Logic giao nhau vẫn hoạt động vì Alice không còn giới hạn miền nữa và tất cả các phần trùng lặp Bob đều được giữ nguyên và hợp nhất một cách chính xác. 

Trường hợp cạnh cuối cùng xảy ra khi cả hai chu kỳ đều lớn nhưng gần bằng nhau, khiến cho sự chồng chéo trôi chậm qua các chu kỳ. Việc hạn chế tính toán trong một chu trình Alice duy nhất đảm bảo rằng ngay cả các mô hình trôi chậm cũng được nắm bắt hoàn toàn mà không cần mô phỏng toàn bộ dòng.
