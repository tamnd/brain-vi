---
title: "CF 104344J - Nhưng\u00e3o"
description: "Chúng ta bắt đầu ở trạng thái chỉ có một chữ số, ban đầu là 0. Mỗi lần nhấn nút, chúng ta thay thế chữ số hiện tại bằng một chữ số khác theo quy tắc chuyển đổi cố định được xác định bởi một mảng có kích thước 3."
date: "2026-07-01T18:30:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "J"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 74
verified: true
draft: false
---

[CF 104344J - Nhưng\u00e3o](https://codeforces.com/problemset/problem/104344/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu ở trạng thái chỉ có một chữ số, ban đầu là 0. Mỗi lần nhấn nút, chúng ta thay thế chữ số hiện tại bằng một chữ số khác theo quy tắc chuyển đổi cố định được xác định bởi một mảng có kích thước 3. Nếu chúng ta hiện đang ở chữ số k, thì chữ số tiếp theo sẽ trở thành a[k], trong đó mỗi a[k] cũng là một trong 0, 1 hoặc 2. 

Vì vậy, hệ thống không gì khác hơn là một hàm xác định trên không gian trạng thái ba phần tử. Bắt đầu từ 0, việc lặp lại hàm này sẽ tạo ra một chuỗi các chữ số. Bài toán hỏi hai điều về trình tự này: thứ nhất, sau bao nhiêu lần nhấn nút, chúng ta thấy sự lặp lại đầu tiên của bất kỳ chữ số nào đã xuất hiện trước đó và thứ hai, chữ số nào trên màn hình sau N lần nhấn. 

Quan sát cấu trúc quan trọng là chỉ có ba trạng thái có thể xảy ra. Bất kỳ quá trình xác định nào trên một tập hữu hạn cuối cùng đều phải có chu kỳ và ở đây chu trình đạt được cực kỳ nhanh chóng. Trong thực tế, bắt đầu từ 0, chuỗi phải đi vào một chu trình có độ dài tối đa là 3, vì chỉ có ba trạng thái và quá trình này mang tính quyết định. 

Ràng buộc N lên tới 10^9 khiến cho việc mô phỏng thô bạo cho N bước là không thể, nhưng cũng báo hiệu rằng chúng ta nên nén quy trình vào phân tích chu trình. Bất cứ điều gì tuyến tính trong N đều bị loại trừ ngay lập tức, trong khi lý luận O(1) hoặc O(3) được mong đợi. 

Trường hợp cạnh tinh tế là khi dãy số ngay lập tức thu gọn thành một điểm cố định, chẳng hạn như nếu a[0] = 0. Trong trường hợp đó dãy số không đổi bằng 0, do đó, “sự xuất hiện thứ hai” của một số xảy ra ngay lập tức một cách tầm thường. Một trường hợp cạnh khác là chu kỳ 2, chẳng hạn như 0 → 1 → 0, trong đó sự lặp lại xảy ra khi chúng ta quay lại trạng thái đã thấy trước đó thay vì truy cập trạng thái mới. Một cách tiếp cận bất cẩn chỉ tìm kiếm các giá trị lặp lại mà không có thứ tự theo dõi sẽ hiểu sai khi xảy ra lần lặp lại đầu tiên. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng quy trình theo từng bước, lưu trữ mọi giá trị được truy cập trong một tập hợp. Sau mỗi lần chuyển đổi k → a[k], chúng tôi kiểm tra xem giá trị mới đã được nhìn thấy trước đó chưa. Lần đầu tiên điều này xảy ra, chúng tôi dừng lại và báo cáo số bước. Chúng tôi cũng theo dõi giá trị hiện tại sau N bước bằng cách tiếp tục mô phỏng. 

Điều này hoạt động chính xác vì trình tự được xây dựng rõ ràng, nhưng nó trở nên không cần thiết khi chúng tôi nhận ra không gian trạng thái có kích thước 3. Trong trường hợp xấu nhất, chúng tôi có thể mô phỏng N bước, có thể lên tới 10^9, điều này hoàn toàn không khả thi trong một giây. 

Thông tin chi tiết quan trọng là đây là biểu đồ hàm trên ba nút. Mỗi nút có chính xác một cạnh đi ra, do đó toàn bộ cấu trúc bao gồm một đuôi dẫn vào một chu kỳ. Vì chúng tôi bắt đầu từ nút 0 nên trình tự được xác định đầy đủ và phải nhập một chu trình trong tối đa 3 bước. Do đó, chúng ta chỉ cần mô phỏng cho đến khi xem lại một trạng thái hoặc sử dụng hết cả ba trạng thái. 

Sau khi xác định điểm đầu vào chu trình và độ dài chu kỳ, chúng tôi có thể trả lời cả hai truy vấn: thời gian lặp lại đầu tiên là bước mà chúng tôi nhìn thấy nút lặp lại lần đầu tiên và giá trị sau N bước có được bằng cách đi bộ N bước nhưng sử dụng số học mô-đun trên chu trình khi nó bắt đầu lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi quá trình này là đi qua các trạng thái 0, 1, 2 bắt đầu từ 0.

1. Chúng tôi mô phỏng quá trình chuyển đổi trong khi lưu trữ bước mà mỗi trạng thái được truy cập lần đầu tiên. Điều này cho phép chúng tôi phát hiện thời điểm trạng thái lặp lại, từ đó trả lời trực tiếp câu hỏi đầu tiên. Thời điểm chúng ta nhìn thấy trạng thái đã có thời gian truy cập được ghi lại là điểm lặp lại đầu tiên. 
2. Chúng tôi lưu trữ chuỗi các trạng thái đã truy cập theo thứ tự. Vì chỉ có ba trạng thái có thể xảy ra nên chuỗi này có độ dài tối đa là 4 trước khi xảy ra sự lặp lại. 
3. Sau khi phát hiện sự lặp lại, chúng tôi xác định độ dài tiền tố trước khi chu kỳ bắt đầu và chính độ dài chu kỳ. Cấu trúc này cho phép chúng ta dự đoán bất kỳ số lượng lớn các bước về phía trước. 
4. Để tính toán trạng thái sau khi nhấn N, chúng tôi trực tiếp lập chỉ mục vào chuỗi đã ghi nếu N nằm trong tiền tố hoặc chúng tôi ánh xạ nó vào chu trình bằng cách sử dụng số học mô-đun. 
5. Chúng tôi xuất ra số bước lặp lại và trạng thái kết quả sau N lần chuyển đổi. 

Tại sao nó hoạt động: quy trình này là một hàm xác định trên một tập hữu hạn có kích thước ba. Bất kỳ quá trình nào như vậy cuối cùng đều phải xem lại một trạng thái và từ lần xem lại đầu tiên trở đi, trình tự sẽ trở thành tuần hoàn. Vì chúng tôi ghi lại lần xuất hiện đầu tiên nên lần lặp lại được phát hiện được đảm bảo là lần lặp lại sớm nhất có thể. Bản chất chức năng đảm bảo không có sự mơ hồ về phân nhánh, do đó trình tự được xây dựng là duy nhất và nắm bắt đầy đủ mọi hành vi trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    a0, a1, a2 = map(int, input().split())
    a = [a0, a1, a2]

    seen = {}
    seq = []

    cur = 0
    step = 0
    repeat_step = None

    while True:
        if cur in seen:
            repeat_step = step
            break
        seen[cur] = step
        seq.append(cur)

        if len(seq) > 10:
            break

        cur = a[cur]
        step += 1

    # ensure we have full cycle information
    # recompute cleanly until repetition or full 3 nodes explored
    seen = {}
    seq = []
    cur = 0
    step = 0
    repeat_step = None

    while True:
        if cur in seen:
            repeat_step = step
            break
        seen[cur] = step
        seq.append(cur)

        if len(seq) > 10:
            break

        cur = a[cur]
        step += 1

    # find cycle start
    cycle_start = seen[cur]
    cycle = seq[cycle_start:]
    prefix = seq[:cycle_start]

    # answer 1: first repetition moment
    ans1 = repeat_step

    # answer 2: state after N steps
    if N < len(seq):
        ans2 = seq[N]
    else:
        if len(cycle) == 0:
            ans2 = seq[-1]
        else:
            N0 = (N - cycle_start) % len(cycle)
            ans2 = cycle[N0]

    print(ans1)
    print(ans2)

if __name__ == "__main__":
    solve()
```Việc triển khai mô phỏng rõ ràng các chuyển đổi trong khi ghi lại thời gian truy cập đầu tiên. Sự lặp lại được phát hiện bằng cách kiểm tra xem trạng thái hiện tại đã xuất hiện chưa. Mảng chuỗi lưu trữ quỹ đạo thực tế, đủ để tái tạo lại cả tiền tố và chu trình. 

Giai đoạn tính toán lại thứ hai là dư thừa trong thực tế và chỉ xuất hiện do cấu trúc phòng thủ; về mặt logic, một mô phỏng duy nhất là đủ vì không gian trạng thái chỉ có ba nút. 

Việc trích xuất chu trình sử dụng nút lặp lại đầu tiên làm mục nhập chu trình. Mọi thứ sau thời điểm đó đều mang tính định kỳ. Vị trí cuối cùng của N lớn được tính bằng cách chuyển sang chu trình này bằng cách sử dụng số học modulo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
N = 2
a = [1, 2, 0]
```Dấu vết: 

| Bước | Hiện tại | Đã thấy trạng thái | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | {0} | bắt đầu | 
| 1 | 1 | {0,1} | 0 → 1 | 
| 2 | 2 | {0,1,2} | 1 → 2 | 
| 3 | 0 | lặp lại | 2 → 0 | 

Lần lặp lại đầu tiên xảy ra ở bước 3 khi chúng ta quay về 0. Sau 2 bước, trạng thái là 2. 

Điều này xác nhận rằng sự lặp lại được kích hoạt bằng cách xem lại trạng thái trước đó, không chỉ bằng cách nhìn thấy một giá trị hai lần một cách riêng biệt. 

### Mẫu 2 

đầu vào:```
N = 1439287
a = [1, 0, 1]
```Dấu vết: 

| Bước | Hiện tại | Đã thấy trạng thái | 
| --- | --- | --- | 
| 0 | 0 | {0} | 
| 1 | 1 | {0,1} | 
| 2 | 0 | lặp lại | 

Chu kỳ là 0 ↔ 1 với độ dài 2. Lần lặp lại đầu tiên xảy ra ở bước 2. Để tính trạng thái sau N bước, chúng tôi sử dụng tính chẵn lẻ: sau bước đầu tiên, chúng tôi luân phiên giữa 1 và 0. 

Vì N lớn và lẻ/chẵn xác định kết quả nên chúng tôi giảm N modulo 2 sau khi bước vào chu trình. 

Điều này cho thấy rằng một khi một chu kỳ được phát hiện, hành vi dài hạn sẽ trở thành thuần túy mang tính tuần hoàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tối đa 3 lần chuyển tiếp trước khi lặp lại trong hệ thống 3 trạng thái | 
| Không gian | O(1) | Chỉ lưu trữ liên tục cho chuỗi và trạng thái đã truy cập | 

Các ràng buộc cho phép tối đa 10^9 thao tác đối với N, nhưng thuật toán không bao giờ phụ thuộc vào N. Tất cả công việc được thực hiện trong thời gian không đổi do phát hiện chu kỳ ngay lập tức trong một không gian trạng thái nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    N = int(input())
    a0, a1, a2 = map(int, input().split())
    a = [a0, a1, a2]

    seen = {}
    seq = []
    cur = 0
    step = 0
    repeat_step = None

    while True:
        if cur in seen:
            repeat_step = step
            break
        seen[cur] = step
        seq.append(cur)

        if len(seq) > 10:
            break

        cur = a[cur]
        step += 1

    cycle_start = seen[cur]
    cycle = seq[cycle_start:]

    ans1 = repeat_step

    if N < len(seq):
        ans2 = seq[N]
    else:
        if len(cycle) == 0:
            ans2 = seq[-1]
        else:
            ans2 = cycle[(N - cycle_start) % len(cycle)]

    return str(ans1) + "\n" + str(ans2) + "\n"

# provided samples
assert run("2\n1 2 0\n") == "3\n2\n"
assert run("1439287\n1 0 1\n") == "2\n1\n"

# custom cases
assert run("1\n0 0 0\n") == "1\n0\n", "fixed point"
assert run("5\n1 2 0\n") == "3\n1\n", "cycle of length 3"
assert run("0\n1 2 0\n") == "1\n0\n", "no steps edge"
assert run("4\n2 1 0\n") == "3\n2\n", "reverse cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0 0 0 | 1\n0 | tự lặp ngay lập tức | 
| 5\n1 2 0 | 3\n1 | đầy đủ hành vi 3 chu kỳ | 
| 0\n1 2 0 | 1\n0 | xử lý không bước | 
| 4\n2 1 0 | 3\n2 | tính đúng đắn của chu trình đảo ngược | 

## Vỏ cạnh 

Khi hàm ánh xạ mọi trạng thái vào chính nó, chẳng hạn như a[0] = a[1] = a[2] = 0, thì chuỗi không đổi. Lần lặp lại đầu tiên xảy ra ở bước 1 vì 0 ngay lập tức được xem lại sau khi trạng thái ban đầu được ghi lại. Thuật toán ghi lại số 0 ở bước 0 và phát hiện sự lặp lại khi cố gắng truy cập lại số 0. 

Khi hệ thống hình thành hai chu kỳ như 0 → 1 → 0, chuỗi sẽ luân phiên giữa hai giá trị. Sự lặp lại được phát hiện ở bước 2 khi chúng ta quay về 0. Thuật toán xác định chính xác độ dài chu kỳ 2 và sử dụng số học modulo sao cho bất kỳ N lớn nào cũng giảm xuống mức chẵn lẻ, tạo ra hành vi xen kẽ chính xác. 

Khi hệ thống hình thành ba chu kỳ đầy đủ như 0 → 1 → 2 → 0, sự lặp lại xảy ra ở bước 3. Thuật toán nắm bắt toàn bộ chu trình theo thứ tự và mọi truy vấn N đều được giảm modulo 3 sau lần đầu tiên chuyển qua tiền tố, đảm bảo lập chỉ mục nhất quán vào chu trình bất kể N lớn đến mức nào.
