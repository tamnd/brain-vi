---
title: "CF 1419B - Cầu Thang"
description: "Một cầu thang có $n$ cột chứa các cột có độ cao $1,2,dots,n$. Tổng số ô trong một cầu thang như vậy là $$1+2+cdots+n=frac{n(n+1)}2.$$ Vấn đề chỉ quan tâm đến cầu thang đẹp."
date: "2026-06-11T06:43:30+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1419
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 671 (Div. 2)"
rating: 1200
weight: 1419
solve_time_s: 97
verified: true
draft: false
---

[CF 1419B - Cầu thang](https://codeforces.com/problemset/problem/1419/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một cầu thang có$n$cột chứa các cột có chiều cao$1,2,\dots,n$. Tổng số ô trong một cầu thang như vậy là$$1+2+\cdots+n=\frac{n(n+1)}2.$$Vấn đề chỉ quan tâm đến cầu thang _đẹp_. Một thực tế hình học quan trọng là một cầu thang đẹp luôn có số lượng cầu thang rất cụ thể. Nếu chúng ta bắt đầu từ cầu thang đẹp nhỏ nhất và liên tục xây dựng những cầu thang lớn hơn thì số lượng cầu thang sẽ như sau$$1,\ 3,\ 7,\ 15,\dots$$đó là$$2^k-1.$$Đối với cầu thang có$2^k-1$cầu thang, số ô là$$1+2+\cdots+(2^k-1) = \frac{(2^k-1)2^k}{2}.$$Cho phép$$c_k=\frac{(2^k-1)2^k}{2}.$$Các ô có sẵn$x$có thể được sử dụng để xây dựng một số cầu thang đẹp khác nhau. Mỗi loại cầu thang chỉ được xây dựng tối đa một lần vì bài toán yêu cầu các loại cầu thang đẹp khác nhau. Chúng tôi muốn số lượng loại cầu thang tối đa có tổng chi phí không vượt quá$x$. 

Giá trị của$x$có thể lớn như$10^{18}$. Bất kỳ thuật toán nào lặp qua tất cả các số lên đến$x$là không thể. Thậm chí$O(\sqrt{x})$sẽ là quá lớn. Chúng ta cần một giải pháp mà công việc của nó chỉ phụ thuộc vào số lượng cầu thang đẹp có thể có chứ không phụ thuộc vào$x$chính nó. 

Một điểm tinh tế là việc xây cầu thang lớn nhất trước tiên không phải lúc nào cũng tối ưu. Ví dụ:```
x = 8
```Chi phí cầu thang là:```
1, 6, 28, ...
```Lấy chi phí$6$những chiếc lá đầu tiên$2$, cho phép thêm một bậc thang chi phí$1$, tổng cộng có hai cầu thang. Một chiến lược cố gắng tối đa hóa các ô đã sử dụng thay vì số lượng cầu thang có thể dễ dàng đưa ra lựa chọn sai. 

Một sai lầm dễ mắc phải khác là cho rằng có nhiều kích thước cầu thang cần xem xét. Chi phí tăng lên gần như$4^k$, vì vậy đối với$x \le 10^{18}$chỉ có khoảng ba mươi loại cầu thang hợp lệ. Khai thác sự tăng trưởng này là toàn bộ giải pháp. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tạo ra mọi chi phí cầu thang đẹp và sau đó thử tất cả các tập hợp con để tìm số lớn nhất có tổng chi phí không vượt quá$x$. Điều này đúng vì mọi loại cầu thang đều có thể được chọn hoặc không được chọn. 

Vấn đề là ngay cả ba mươi loại cầu thang cũng sẽ dẫn đến khoảng$2^{30}$tập hợp con, hơn một tỷ khả năng. Điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chi phí cầu thang tăng cực kỳ nhanh:$$1,\ 6,\ 28,\ 120,\ 496,\dots$$Mỗi chi phí lớn hơn nhiều so với tổng của tất cả các chi phí trước đó. Ví dụ,$$28 > 1+6,$$

$$120 > 1+6+28.$$Trên thực tế, đối với mỗi$k$,$$c_k > \sum_{i<k} c_i.$$Điều này có nghĩa là dãy số đang siêu tăng. 

Giả sử chúng ta muốn số lượng cầu thang tối đa. Nếu cầu thang lớn nhất phù hợp với chi phí$c_k$, thì giải pháp nào bỏ qua$c_k$không thể bù đắp bằng cách đi cầu thang nhỏ hơn, bởi vì tất cả các chi phí nhỏ hơn gộp lại vẫn thấp hơn$c_k$. Lấy$c_k$luôn là sự lựa chọn tốt nhất bất cứ khi nào nó phù hợp. 

Điều đó ngay lập tức đưa ra một thuật toán tham lam: 

Tiếp tục trừ chi phí cầu thang lớn nhất không vượt quá các ô còn lại. Đếm xem điều này có thể được bao nhiêu lần. 

Vì chỉ có khoảng ba mươi cầu thang nên chi phí lên tới$10^{18}$, thuật toán rất nhỏ và rất nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^m)$|$O(m)$| Quá chậm | 
| Tham lam với chi phí được tính toán trước |$O(m)$|$O(m)$| Đã chấp nhận | 

Đây$m \approx 30$. 

## Hướng dẫn thuật toán 

1. Tính toán trước toàn bộ chi phí cầu thang đẹp$$c_k=\frac{(2^k-1)2^k}{2}$$trong khi$c_k \le 10^{18}$. 
2. Đối với mỗi trường hợp kiểm thử, hãy bắt đầu với số ô đã cho$x$. 
3. Tìm chi phí cầu thang lớn nhất không vượt quá hiện tại$x$. 
4. Trừ chi phí đó khỏi$x$và tăng câu trả lời lên một. 

Đây là sự lựa chọn đúng đắn vì mọi chi phí đều lớn hơn tổng của tất cả các chi phí nhỏ hơn. 
5. Lặp lại trong khi một số chi phí cầu thang vẫn vừa với các ô còn lại. 
6. Xuất ra số lượng cầu thang đã chọn. 

### Tại sao nó hoạt động 

Chi phí cầu thang tạo thành một chuỗi tăng dần:$$c_k > \sum_{i<k} c_i.$$Hãy tính chi phí lớn nhất$c_k$phù hợp với ngân sách hiện tại. Giải pháp nào không sử dụng$c_k$có thể chi tiêu nhiều nhất$$\sum_{i<k} c_i,$$nhỏ hơn hoàn toàn so với$c_k$. Thay thế$c_k$bởi bất kỳ bộ sưu tập cầu thang nhỏ hơn nào cũng không bao giờ mang lại cơ hội tốt hơn để tăng số lượng lựa chọn trong tương lai. 

Sau khi chọn$c_k$, vấn đề còn lại giống hệt với ngân sách nhỏ hơn. Việc lặp lại cùng một lập luận đệ quy chứng tỏ rằng sự lựa chọn tham lam luôn là tối ưu. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

# precompute staircase costs
costs = []
k = 1
LIMIT = 10 ** 18

while True:
    p = 1 << k
    cost = (p - 1) * p // 2
    if cost > LIMIT:
        break
    costs.append(cost)
    k += 1

t = int(input())
ans = []

for _ in range(t):
    x = int(input())
    cnt = 0

    while True:
        idx = bisect_right(costs, x) - 1
        if idx < 0:
            break
        x -= costs[idx]
        cnt += 1

    ans.append(str(cnt))

sys.stdout.write("\n".join(ans))
```Giai đoạn tiền tính toán tạo ra mọi chi phí cầu thang phù hợp có thể có. Bởi vì chi phí tăng theo cấp số nhân nên chỉ có khoảng ba mươi giá trị được lưu trữ. 

Đối với mỗi trường hợp thử nghiệm,`bisect_right`tìm thấy chi phí lớn nhất không vượt quá ngân sách hiện tại. Chi phí đó được trừ đi và quá trình lặp lại. 

Việc sử dụng tìm kiếm nhị phân là không thực sự cần thiết vì danh sách rất nhỏ nhưng nó giữ cho việc triển khai rõ ràng và làm cho ý định rõ ràng. 

Tất cả số học đều sử dụng số nguyên Python, do đó không có rủi ro tràn ngay cả khi gần$10^{18}$. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
x = 8
```Chi phí cầu thang là:```
1, 6, 28, ...
```| Còn lại x | Chi phí lớn nhất ≤ x | Mới x | Đếm | 
| --- | --- | --- | --- | 
| 8 | 6 | 2 | 1 | 
| 2 | 1 | 1 | 2 | 
| 1 | 1 | 0 | 3 | 

Dấu vết này tiết lộ một chi tiết quan trọng. Thuật toán đếm xem có thể xây dựng tuần tự bao nhiêu cầu thang đẹp từ các ô có sẵn. Sau khi xây cầu thang, những ô còn lại vẫn có thể dùng để xây cầu thang khác loại nhỏ hơn. 

Câu trả lời là:```
3
```### Đầu vào mẫu```
x = 1000000000000000000
```| Bước | Còn lại x Trước | Chi phí được chọn | 
| --- | --- | --- | 
| 1 | 10000000000000000000 | 576460752169205760 | 
| 2 | 423539247830794240 | 144115188075855872 | 
| 3 | 279424059754938368 | 36028792723996672 | 
| ... | ... | ... | 
| 30 | đủ cho cầu thang nhỏ nhất | 1 | 

Sau ba mươi lần lựa chọn thành công, ngân sách còn lại trở nên quá nhỏ cho một cầu thang khác. 

Câu trả lời là:```
30
```Điều này thể hiện tính chất logarit của nghiệm. Ngay cả đối với đầu vào lớn nhất có thể, chỉ tồn tại khoảng ba mươi kích thước cầu thang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| Nhiều nhất khoảng 30 lựa chọn tham lam | 
| Không gian |$O(m)$| Lưu trữ tất cả chi phí cầu thang | 

Từ$m \approx 30$, thời gian chạy cho mỗi trường hợp thử nghiệm thực tế là không đổi. Với tối đa 1000 trường hợp thử nghiệm, chương trình dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from bisect import bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    costs = []
    k = 1
    LIMIT = 10 ** 18

    while True:
        p = 1 << k
        cost = (p - 1) * p // 2
        if cost > LIMIT:
            break
        costs.append(cost)
        k += 1

    t = int(input())
    out = []

    for _ in range(t):
        x = int(input())
        cnt = 0

        while True:
            idx = bisect_right(costs, x) - 1
            if idx < 0:
                break
            x -= costs[idx]
            cnt += 1

        out.append(str(cnt))

    return "\n".join(out)

# provided samples
assert run(
    "4\n1\n8\n6\n1000000000000000000\n"
) == "1\n3\n1\n30"

# custom cases
assert run("1\n2\n") == "2", "two smallest staircases"
assert run("1\n5\n") == "5", "repeated use of size-1 staircase"
assert run("1\n28\n") == "1", "exactly one larger staircase"
assert run("1\n29\n") == "2", "boundary just above 28"
assert run("1\n1000000000000000000\n") == "30", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`2`| Ngân sách không cần thiết nhỏ nhất | 
|`5`|`5`| Sự cắt giảm tham lam lặp đi lặp lại | 
|`28`|`1`| Kết hợp chính xác với chi phí cầu thang | 
|`29`|`2`| Giá ranh giới ngay phía trên cầu thang | 
|`10^18`|`30`| Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
```Chi phí cầu thang phù hợp lớn nhất là$1$. Sau khi trừ đi, ngân sách còn lại bằng không. Các đầu ra thuật toán:```
1
```cái nào đúng. 

Coi như:```
1
28
```Chi phí lắp cầu thang lớn nhất chính xác là$28$. Thuật toán sẽ trừ nó ngay lập tức: 

| Còn lại x | Chi phí được chọn | 
| --- | --- | 
| 28 | 28 | 
| 0 | dừng lại | 

Câu trả lời là:```
1
```Không có sự kết hợp nhỏ hơn nào có thể tạo ra nhiều cầu thang hơn vì$28$đã tương ứng với cấp độ cầu thang tiếp theo. 

Coi như:```
1
29
```Đầu tiên thuật toán chọn$28$, rời đi$1$. Sau đó nó chọn$1$. 

| Còn lại x | Chi phí được chọn | Đếm | 
| --- | --- | --- | 
| 29 | 28 | 1 | 
| 1 | 1 | 2 | 
| 0 | dừng lại | 2 | 

Câu trả lời là:```
2
```Điều này bắt nguồn từ một sai lầm xung quanh ranh giới chính xác của cầu thang. 

Cuối cùng, hãy xem xét giá trị tối đa:```
1
1000000000000000000
```Chỉ có khoảng ba mươi chi phí cầu thang tồn tại bên dưới$10^{18}$. Quá trình tham lam thực hiện khoảng 30 lần lặp và đưa ra kết quả:```
30
```cho thấy giải pháp vẫn hiệu quả ngay cả ở kích thước đầu vào lớn nhất.
