---
title: "CF 105819H - Chuỗi Fibocchi"
description: "Chúng ta được cho một dãy có độ dài n giống Fibonacci. Hai số hạng đầu tiên là a và b, và mọi số hạng sau đều bằng tổng của hai số hạng trước. Đối với mỗi truy vấn, chúng tôi bắt đầu từ chuỗi đó và có thể thực hiện các thao tác."
date: "2026-06-25T15:07:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "H"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 77
verified: true
draft: false
---

[CF 105819H - Chuỗi Fibocchi](https://codeforces.com/problemset/problem/105819/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài giống Fibonacci`n`. 

Hai thuật ngữ đầu tiên là`a`Và`b`, và mọi số hạng sau đều bằng tổng của hai số hạng trước đó. Đối với mỗi truy vấn, chúng tôi bắt đầu từ chuỗi đó và có thể thực hiện các thao tác. Một thao tác chọn một chỉ mục`k`, tăng`f[k]`một, sau đó tính toán lại mọi số hạng sau đó bằng cách sử dụng phép truy toán Fibonacci. đầu tiên`k - 1`các giá trị không thay đổi. 

Nhiệm vụ là xác định số thao tác tối thiểu cần thiết để làm cho tổng của chuỗi bằng giá trị đích.`x`, hoặc báo cáo`-1`nếu nó không thể được thực hiện. Giá trị toàn cầu`n`được cố định cho tất cả các truy vấn, trong khi các truy vấn cung cấp các giá trị khác nhau của`a`,`b`, Và`x`. Những hạn chế là`n ≤ 30`,`q ≤ 5 · 10^5`, Và`x ≤ 5 · 10^6`. 

Số lượng truy vấn lớn bất thường ngay lập tức loại trừ mọi hoạt động mô phỏng hoạt động theo từng truy vấn. Thậm chí một`O(n^2)`giải pháp cho mỗi truy vấn sẽ quá chậm đối với nửa triệu truy vấn. Chúng ta cần một bước tiền xử lý chỉ phụ thuộc vào`n`, tiếp theo là xử lý truy vấn cực nhanh. 

Trường hợp cạnh tinh tế xuất hiện khi tổng ban đầu đã lớn hơn`x`.```
n = 4
a = 1
b = 1
x = 6
```Trình tự là`[1, 1, 2, 3]`, tổng của nó là`7`. Mọi thao tác đều làm tăng tổng, do đó đạt được`6`là không thể. Câu trả lời đúng là`-1`. 

Một sai lầm dễ mắc phải khác là giả định rằng việc tăng một số hạng sẽ làm tổng số thay đổi đúng một.```
[2, 1, 3, 4, 7]
```Tăng số hạng thứ ba mang lại```
[2, 1, 4, 5, 9]
```Tổng số tiền tăng thêm`4`, không phải bởi`1`. Sự tái diễn lan truyền sự thay đổi về phía trước. 

Cái bẫy thứ ba là quên rằng các vị trí khác nhau đóng góp số tiền khác nhau. Một thao tác trên chỉ mục đầu ảnh hưởng đến nhiều số hạng trong tương lai, trong khi thao tác ở gần cuối chỉ ảnh hưởng đến một số số hạng. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là xử lý từng hoạt động một cách độc lập. Đối với mỗi vị trí, chúng ta có thể tính toán tổng số tiền tăng lên bao nhiêu nếu chúng ta áp dụng một thao tác ở đó. Sau đó, bài toán trở thành bài toán đổi xu: đạt được mức tăng cần thiết bằng cách sử dụng ít thao tác nhất. 

Quan sát này đã đủ để giải quyết phần logic của vấn đề. 

Giả sử chúng ta tăng`f[k]`bởi một. Trình tự thay đổi cảm ứng là```
1, 1, 2, 3, 5, ...
```bắt đầu ở vị trí`k`. 

Tổng số tiền tăng lên là```
1 + 1 + 2 + 3 + ... + Fm
```Ở đâu`m = n - k + 1`. 

Sử dụng danh tính tổng tiền tố Fibonacci,```
F1 + F2 + ... + Fm = Fm+2 - 1
```lợi ích từ một hoạt động ở vị trí đó là```
cm = Fm+2 - 1
```Vì vậy, các giá trị hoạt động có sẵn là```
1, 2, 4, 7, 12, 20, ...
```chính xác là`F3 - 1, F4 - 1, F5 - 1, ...`. 

Bây giờ truy vấn trở thành:```
Let D = x - current_sum.
Find the minimum number of coins needed to form D
using coin values F3-1, F4-1, ..., Fn+2-1.
```Quan sát quan trọng là hệ thống tiền xu này là hợp quy. Sự lựa chọn tham lam về đồng tiền lớn nhất có thể sử dụng được luôn là tối ưu. Nguyên nhân là do cấu trúc Fibonacci:```
(Fi+2 - 1) = (Fi+1 - 1) + (Fi - 1) + 1
```cung cấp cùng một thuộc tính trao đổi làm cho biểu diễn Zeckendorf hoạt động. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành giải pháp lấy đồng xu lớn nhất có thể trước, sau đó giải quyết số tiền còn lại một cách tham lam. 

Từ`n ≤ 30`, có tối đa 30 giá trị đồng xu. Một truy vấn tham lam chỉ mất`O(n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Thay đổi đồng tiền DP Brute Force cho mỗi truy vấn | O(n · D) | O(D) | Quá chậm | 
| Hệ thống tiền xu Fibonacci tham lam | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các số Fibonacci theo chỉ số`32`. 
2. Xây dựng giá trị đồng xu```
coin[i] = Fi+2 - 1
```cho tất cả các vị trí hoạt động có thể sử dụng. 
3. Tính tổng chuỗi ban đầu mà không tạo ra toàn bộ chuỗi. 

Hệ số của`a`trong tổng số tiền là`Fn`, và hệ số của`b`là`Fn+1 - 1`, cho```
S = a · Fn + b · (Fn+1 - 1)
```4. Hãy để```
D = x - S
```Nếu như`D < 0`, in`-1`. 
5. Nếu không, liên tục lấy đồng xu lớn nhất không vượt quá giá trị còn lại. 
6. Đếm xem lấy được bao nhiêu đồng xu. 
7. Nếu phần dư bằng 0, xuất số đếm. Nếu không thì xuất ra`-1`. 

### Tại sao nó hoạt động 

Mỗi hoạt động đóng góp một trong các giá trị```
F3 - 1, F4 - 1, ..., Fn+2 - 1.
```Tổng mức tăng sau một số thao tác chính xác là tổng giá trị của đồng tiền đã chọn. 

Hệ thống tiền xu có nguồn gốc từ Fibonacci đáp ứng đặc tính trao đổi giống như biểu diễn Zeckendorf. Bất cứ khi nào một giải pháp không sử dụng đồng xu lớn nhất có thể, đóng góp của nó có thể được thay thế bằng mệnh giá tương đương hoặc lớn hơn mà không làm tăng số lượng đồng xu. Lặp lại phép biến đổi này tạo ra giải pháp tham lam. Vì mỗi bước tham lam bị ép buộc trong một số giải pháp tối ưu nên số lượng tham lam cuối cùng là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())

fib = [0] * 35
fib[1] = fib[2] = 1
for i in range(3, 35):
    fib[i] = fib[i - 1] + fib[i - 2]

coins = [fib[i + 2] - 1 for i in range(1, n + 1)]

coef_a = fib[n]
coef_b = fib[n + 1] - 1

out = []

for _ in range(q):
    a, b, x = map(int, input().split())

    s = a * coef_a + b * coef_b
    d = x - s

    if d < 0:
        out.append("-1")
        continue

    ans = 0
    rem = d

    for c in reversed(coins):
        if rem == 0:
            break
        take = rem // c
        ans += take
        rem -= take * c

    if rem == 0:
        out.append(str(ans))
    else:
        out.append("-1")

sys.stdout.write("\n".join(out))
```Khối đầu tiên tính toán các số Fibonacci một lần. Từ`n`không bao giờ vượt quá 30, quá trình tiền xử lý này rất nhỏ. 

Bước tiếp theo xây dựng lợi ích hoạt động. Giá trị được liên kết với một vị trí chỉ phụ thuộc vào số lượng thuật ngữ còn lại ở bên phải vị trí đó, do đó, cùng một danh sách tiền xu sẽ được sử dụng lại cho mọi truy vấn. 

Công thức```
S = a · Fn + b · (Fn+1 − 1)
```tránh tạo ra toàn bộ chuỗi. Điều này quan trọng vì có tới 500.000 truy vấn. 

Vòng lặp tham lam xử lý tối đa 30 mệnh giá, giúp giữ cho mọi truy vấn cực kỳ nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = 1
b = 1
x = 10
```Vì`n = 4`, giá trị của đồng xu là:```
1, 2, 4, 7
```Tổng dãy là```
1 + 1 + 2 + 3 = 7
```Vì thế```
D = 10 - 7 = 3
```| Bước | Còn lại | Đồng xu được sử dụng | Hoạt động | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 | - | 0 | 
| 1 | 1 | 2 | 1 | 
| 2 | 0 | 1 | 2 | 

Trả lời:```
2
```Dấu vết này cho thấy mức tăng mục tiêu được phân tách thành lợi nhuận hoạt động như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 4
a = 2
b = 3
x = 31
```Trình tự là```
[2, 3, 5, 8]
```với tổng```
18
```Kể từ đây```
D = 31 - 18 = 13
```| Bước | Còn lại | Đồng xu được sử dụng | Hoạt động | 
| --- | --- | --- | --- | 
| Bắt đầu | 13 | - | 0 | 
| 1 | 6 | 7 | 1 | 
| 2 | 2 | 4 | 2 | 
| 3 | 0 | 2 | 3 | 

Trả lời:```
3
```Sự lựa chọn tham lam ngay lập tức nhận được lợi ích lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Tối đa 30 giá trị xu được kiểm tra | 
| Không gian | O(n) | Bảng Fibonacci và danh sách tiền xu | 

Từ`n ≤ 30`, mỗi truy vấn chỉ thực hiện vài chục phép tính số học. Ngay cả với`5 · 10^5`truy vấn, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, q = map(int, input().split())

    fib = [0] * 35
    fib[1] = fib[2] = 1
    for i in range(3, 35):
        fib[i] = fib[i - 1] + fib[i - 2]

    coins = [fib[i + 2] - 1 for i in range(1, n + 1)]

    coef_a = fib[n]
    coef_b = fib[n + 1] - 1

    out = []

    for _ in range(q):
        a, b, x = map(int, input().split())

        s = a * coef_a + b * coef_b
        d = x - s

        if d < 0:
            out.append("-1")
            continue

        ans = 0
        rem = d

        for c in reversed(coins):
            take = rem // c
            ans += take
            rem %= c

        out.append(str(ans) if rem == 0 else "-1")

    return "\n".join(out)

# sample
assert run(
"""4 5
1 1 7
1 1 10
2 1 14
2 1 8
2 3 31
"""
) == "0\n2\n1\n-1\n3"

# minimum target already achieved
assert run(
"""3 1
1 1 4
"""
) == "0"

# impossible because target smaller than current sum
assert run(
"""3 1
1 1 3
"""
) == "-1"

# exact single operation
assert run(
"""4 1
2 1 14
"""
) == "1"

# larger increase
assert run(
"""4 1
1 1 17
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 4`trường hợp |`0`| Đã đạt mục tiêu | 
|`1 1 3`trường hợp |`-1`| Mục tiêu dưới số tiền hiện tại | 
|`2 1 14`trường hợp |`1`| Câu trả lời thao tác đơn | 
|`1 1 17`trường hợp |`3`| Nhiều lựa chọn tham lam | 

## Vỏ cạnh 

Hãy xem xét:```
n = 4
a = 1
b = 1
x = 6
```Tổng hiện tại là`7`. Vì mọi thao tác đều tăng tổng một cách nghiêm ngặt nên thuật toán ngay lập tức phát hiện```
D = -1
```và trả về`-1`. 

Bây giờ hãy xem xét:```
n = 4
a = 1
b = 1
x = 7
```Số tiền hiện tại đã bằng mục tiêu. Thuật toán được```
D = 0
```và trả về`0`. 

Cuối cùng:```
n = 4
a = 2
b = 1
x = 14
```Tổng hiện tại là`10`, Vì thế`D = 4`. Đồng xu`4`tồn tại trực tiếp, tương ứng với một thao tác ở vị trí sớm nhất tạo ra mức tăng bốn. Thuật toán tham lam chọn nó ngay lập tức và trả về`1`.
