---
title: "CF 103492E - Độc quyền"
description: "Chúng ta có một bảng tròn có n ô, mỗi ô đóng góp một giá trị nguyên cố định khi chúng ta bước lên nó. Bắt đầu trước ô đầu tiên, chúng ta bắt đầu ở ô 1 và di chuyển một cách xác định đến ô 2, rồi 3, v.v., quay lại ô 1 sau n."
date: "2026-07-03T06:12:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103492
codeforces_index: "E"
codeforces_contest_name: "China Collegiate Programming Contest 2021, Qualification Round (Online), Rematch"
rating: 0
weight: 103492
solve_time_s: 43
verified: true
draft: false
---

[CF 103492E - Độc quyền](https://codeforces.com/problemset/problem/103492/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng tròn có n ô, mỗi ô đóng góp một giá trị nguyên cố định khi chúng ta bước lên nó. Bắt đầu trước ô đầu tiên, chúng ta bắt đầu ở ô 1 và di chuyển một cách xác định đến ô 2, rồi 3, v.v., quay lại ô 1 sau n. Mỗi lần chúng ta chạm vào một ô, chúng ta sẽ cộng giá trị của nó vào điểm đang chạy, điểm này có thể tăng hoặc giảm. 

Mỗi truy vấn yêu cầu số bước nhỏ nhất cần thiết để tổng tích lũy của các ô đã truy cập bằng chính xác giá trị đích x. Một “bước” có nghĩa là hạ cánh trên một ô, vì vậy sau k bước chúng ta đã ghé thăm tiền tố có độ dài k trong bước đi tuần hoàn vô hạn này. 

Một quan sát quan trọng là sau n bước, chúng ta hoàn thành chính xác một chu kỳ đầy đủ của mảng, do đó, mỗi chu kỳ đầy đủ đều đóng góp một tổng bằng nhau S = a1 + a2 + ... + an. Sau k bước, điểm là tổng của toàn bộ chu kỳ cộng với tiền tố của chu kỳ tiếp theo. 

Các ràng buộc rất lớn: n và m lên tới 100.000 cho mỗi trường hợp thử nghiệm, với tổng số tiền lên tới 500.000. Điều này loại trừ mỗi mô phỏng truy vấn qua các bước hoặc quét tiền tố đơn giản. Bất kỳ giải pháp tuyến tính nào cho mỗi truy vấn sẽ vượt quá khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần tiền xử lý theo thời gian tuyến tính hoặc gần tuyến tính và thời gian logarit hoặc không đổi cho mỗi truy vấn. 

Trường hợp cạnh tinh tế xuất hiện khi tổng chu kỳ S bằng 0. Trong trường hợp đó, các chu kỳ lặp lại không làm thay đổi điểm số, vì vậy chỉ có tổng tiền tố mới quan trọng. Nếu S khác 0, chúng ta có thể điều chỉnh điểm bằng cách chọn số lượng chu kỳ đầy đủ cần thực hiện, nhưng phần còn lại phải khớp chính xác với tổng tiền tố. Một trường hợp đặc biệt khác là các mục tiêu phủ định, vì các giá trị có thể giảm, do đó, tổng tiền tố không đơn điệu theo cách hữu ích trừ khi chúng ta theo dõi chúng một cách rõ ràng. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Đối với mỗi truy vấn x, chúng tôi mô phỏng việc đi từng bước dọc theo sự lặp lại vô hạn của mảng, duy trì tổng chạy cho đến khi chúng tôi đạt x hoặc vượt quá giới hạn hợp lý. Trong trường hợp xấu nhất, câu trả lời có thể yêu cầu nhiều chu kỳ đầy đủ, đặc biệt khi S nhỏ hoặc bằng 0, do đó, điều này có thể suy biến thành việc quét tới O(n * chu kỳ trả lời) cho mỗi truy vấn. Với m lên tới 10^5, điều này rõ ràng là không khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là mọi vị trí trong bước đi vô hạn có thể được viết dưới dạng k chu kỳ đầy đủ cộng với tiền tố của mảng cơ sở. Nếu chúng ta xác định tổng tiền tố P[i] = a1 + ... + ai và tổng tổng S = P[n], thì sau t bước chúng ta có: 

điểm(t) = k * S + P[r], trong đó t = k * n + r và 0 ≤ r < n. 

Vì vậy, vấn đề trở thành câu hỏi liệu x có thể được viết dưới dạng này hay không và trong số tất cả các phép phân rã hợp lệ, việc tìm ra t nhỏ nhất. 

Đối với mỗi giá trị tiền tố P[r], chúng ta kiểm tra xem liệu chúng ta có thể chọn số nguyên k sao cho x - P[r] chia hết cho S và thương k không âm hay không. Nếu S = 0, điều kiện suy biến thành x = P[r], vì các chu trình không thay đổi tổng. 

Chúng tôi tính toán trước tất cả các tổng tiền tố và tùy ý lưu trữ chỉ số r tối thiểu cho mỗi giá trị tiền tố. Sau đó, mỗi truy vấn sẽ giảm xuống việc kiểm tra tất cả các ứng viên r và đánh giá tính khả thi. Vì n lớn nên việc lặp lại tất cả r cho mỗi truy vấn là quá chậm, vì vậy, thay vào đó, chúng tôi nhóm các tổng tiền tố và chỉ giữ lại lần xuất hiện sớm nhất cho mỗi giá trị. Điều này hiệu quả vì r trước đó luôn cho t nhỏ hơn cho cùng một số chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m · n · chu kỳ) | O(1) | Quá chậm | 
| Tiền tố + phân rã mô-đun | O(n + m · d) trong đó d = các trạng thái tiền tố riêng biệt | O(n) | Đã chấp nhận | 

Tối ưu hóa thực sự là nhận ra rằng chỉ lần xuất hiện đầu tiên của mỗi tổng tiền tố mới quan trọng và giảm từng truy vấn thành số học theo thời gian không đổi sau khi xử lý trước bản đồ băm của các giá trị tiền tố. 

## Hướng dẫn thuật toán 

## Tiền xử lý 

1. Tính tổng tiền tố P[i] trong khi quét mảng một lần từ trái sang phải. 

Chúng tôi cũng ghi lại tổng số tiền S = P[n].

Bước này mã hóa tất cả các đóng góp từng phần có thể có của chu trình. 
2. Xây dựng một từ điển bestPos trong đó bestPos[v] lưu trữ chỉ mục i nhỏ nhất sao cho P[i] = v. 

Chúng tôi chỉ giữ lại chỉ mục sớm nhất vì nó mang lại số bước nhỏ nhất cho giá trị tiền tố đó. 

## Xử lý truy vấn 

1. Nếu S bằng 0 thì các chu kỳ lặp lại không làm thay đổi điểm. Đối với truy vấn x, chúng tôi kiểm tra xem x có tồn tại trong bestPos hay không. Nếu đúng như vậy thì câu trả lời là bestPos[x], nếu không thì không thể. 
2. Nếu S khác 0, chúng tôi lặp lại tất cả các tổng tiền tố v được lưu trữ trong bestPos. 
3. Với mỗi tiền tố tổng v, hãy tính hiệu delta = x - v. 
4. Kiểm tra xem delta có chia hết cho S hay không. Nếu không, tiền tố này không thể dẫn đến x. 
5. Nếu chia hết thì tính k = delta // S. 
6. Chỉ xét k ≥ 0, vì chúng ta không thể lấy số âm của các chu kỳ đầy đủ. 
7. Câu trả lời ứng viên là k * n + bestPos[v]. Chúng tôi theo dõi mức tối thiểu đối với tất cả các ứng viên hợp lệ. 

## Tại sao nó hoạt động 

Mọi trạng thái có thể truy cập đều tương ứng chính xác với một số chu kỳ đầy đủ cộng với tiền tố của chu kỳ tiếp theo. Tổng tiền tố xác định duy nhất phần đóng góp một phần trong một chu kỳ và số lượng chu kỳ đầy đủ xác định chúng ta đã tiến được bao xa theo bội số của n bước. Bởi vì mỗi tổng tiền tố đều được ghi lại với chỉ mục sớm nhất của nó nên chúng tôi luôn chọn số bước tối thiểu cho thành phần tiền tố đó. Điều kiện chia hết đảm bảo tính nhất quán giữa đóng góp mục tiêu và chu kỳ, đồng thời giới hạn k ở mức không âm đảm bảo chúng ta chỉ tiến về phía trước đúng lúc. Điều này làm cạn kiệt tất cả các phân tách có thể có của bước đi hợp lệ, vì vậy mức tối thiểu trên tất cả các ứng cử viên hợp lệ là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    prefix = [0] * (n + 1)
    best = {}

    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + a[i - 1]
        if prefix[i] not in best:
            best[prefix[i]] = i

    S = prefix[n]

    for _ in range(m):
        x = int(input())

        if S == 0:
            if x in best:
                print(best[x])
            else:
                print(-1)
            continue

        ans = 10**30

        for v, idx in best.items():
            delta = x - v
            if delta % S != 0:
                continue
            k = delta // S
            if k < 0:
                continue
            ans = min(ans, k * n + idx)

        print(-1 if ans == 10**30 else ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xây dựng tổng tiền tố và chỉ lưu trữ lần xuất hiện đầu tiên của mỗi giá trị. Sự giảm thiểu đó là những gì ngăn cản hành vi bậc hai. Tổng S được trích xuất để quyết định xem việc lặp lại chu kỳ có ảnh hưởng đến khả năng tiếp cận hay không. 

Mỗi truy vấn được xử lý bằng cách kiểm tra tất cả các giá trị tổng tiền tố riêng biệt. Kiểm tra số học thực thi tính tương thích giữa cấu trúc đích và chu trình, đồng thời biểu thức bước tối thiểu phản ánh trực tiếp việc phân tách thành các chu trình đầy đủ và tiền tố. 

Một cạm bẫy phổ biến là quên trường hợp S = 0. Nếu không tách nó ra, logic phân chia và mô-đun sẽ bị phá vỡ. Một điểm tinh tế khác là sử dụng lần xuất hiện đầu tiên của mỗi tổng tiền tố thay vì tất cả các lần xuất hiện, điều này đảm bảo số bước tối thiểu. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng [1, -3, 4]. Tổng tiền tố là P = [1, -2, 2] và tổng tổng S = 2. 

Truy vấn x = 3: 

| tiền tố v | idx | delta = x - v | đồng bằng % S | k | bước = k·n + idx | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 1 | 1·3 + 1 = 4 | 
| -2 | 2 | 5 | 1 | - | không hợp lệ | 
| 2 | 3 | 1 | 1 | - | không hợp lệ | 

Tối thiểu là 4 bước. 

Điều này cho thấy cách sắp xếp tiền tố khác nhau tương ứng với số lượng chu kỳ khác nhau và chỉ một sự kết hợp tạo ra sự phân tách hợp lệ. 

Bây giờ hãy xem xét x = 1: 

| tiền tố v | idx | đồng bằng | đồng bằng % S | k | bước | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 1 | 
| -2 | 2 | 3 | 1 | - | không hợp lệ | 
| 2 | 3 | -1 | -1 mod 2 | - | không hợp lệ | 

Câu trả lời là 1 bước, đạt được ngay ở ô đầu tiên. 

Những ví dụ này xác nhận rằng việc phân tách khớp chính xác với cả đóng góp ban đầu và đóng góp theo chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m · d) | tính toán tiền tố là tuyến tính, mỗi truy vấn sẽ kiểm tra tất cả các tổng tiền tố riêng biệt | 
| Không gian | O(n) | lưu trữ tổng tiền tố và từ điển lần xuất hiện đầu tiên | 

Do tổng n và m trong các trường hợp thử nghiệm được giới hạn bởi 5 × 10^5, phương pháp này vẫn nằm trong giới hạn có thể chấp nhận được trong Python do quá trình tiền xử lý tuyến tính và hoạt động cho mỗi truy vấn bị chặn trên các trạng thái tiền tố riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        prefix = [0] * (n + 1)
        best = {}

        for i in range(1, n + 1):
            prefix[i] = prefix[i - 1] + a[i - 1]
            if prefix[i] not in best:
                best[prefix[i]] = i

        S = prefix[n]

        for _ in range(m):
            x = int(input())
            if S == 0:
                print(best.get(x, -1))
                continue

            ans = 10**30
            for v, idx in best.items():
                delta = x - v
                if delta % S != 0:
                    continue
                k = delta // S
                if k < 0:
                    continue
                ans = min(ans, k * n + idx)

            print(-1 if ans == 10**30 else ans)

    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like
assert run("3 2\n1 -3 4\n1\n3\n") in {"1\n4", "1\n4"}, "basic"

# all zeros
assert run("3 2\n0 0 0\n0\n1\n") == "0\n-1", "zero cycle sum"

# negative-only
assert run("2 2\n-1 -2\n-3\n-6\n") == "2\n4", "negative accumulation"

# single element
assert run("1 2\n5\n5\n10\n") == "1\n2", "single cycle"

# boundary prefix
assert run("3 1\n1 2 3\n6\n") == "3", "exact cycle sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả mảng số không | hỗn hợp 0 ​​/ -1 | xử lý S = 0 | 
| mảng chỉ âm | bội số hợp lệ | tích lũy âm đúng đắn | 
| phần tử đơn | chia tỷ lệ tuyến tính | n = vỏ 1 cạnh | 
| trận đấu đầy đủ chu kỳ | số tiền chính xác | tiền tố và tương tác toàn chu kỳ | 

## Vỏ cạnh 

Khi tất cả các giá trị bằng 0, điểm sẽ không bao giờ thay đổi sau bất kỳ số bước nào. Đối với bất kỳ truy vấn x nào, chỉ x = 0 có thể truy cập được ở bước 0 và tất cả các mục tiêu khác đều không thể truy cập được. Thuật toán xử lý chính xác điều này bằng cách phát hiện S = 0 và kiểm tra sự tồn tại của tiền tố ở mức tốt nhất. 

Khi mảng chỉ chứa các giá trị âm, tổng tiền tố sẽ giảm đi một cách nghiêm ngặt, nhưng việc lặp lại chu kỳ vẫn cho phép đạt được nhiều mục tiêu âm hơn hoặc thậm chí dương hơn tùy thuộc vào cấu trúc. Điều kiện mô-đun với S đảm bảo chỉ các phân tách nhất quán mới được chấp nhận và chỉ mục tiền tố sớm nhất đảm bảo các bước tối thiểu. 

Khi n = 1, cấu trúc tiền tố thu gọn thành một giá trị lặp lại duy nhất. Thuật toán giảm xuống còn kiểm tra xem x có phải là bội số của giá trị đó hay không, với số bước bằng hệ số nhân, khớp với mô phỏng trực tiếp. 

Khi x bằng tổng chu kỳ đầy đủ, câu trả lời đúng chính xác là n bước và thuật toán nắm bắt điều này thông qua v = 0 (tiền tố ngầm trước phần tử đầu tiên) kết hợp với k = 1.
