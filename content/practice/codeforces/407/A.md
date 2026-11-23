---
title: "CF 407A - Tam giác"
description: "Cho hai số nguyên dương a và b là độ dài hai cạnh của một tam giác vuông. Nhiệm vụ không phải là kiểm tra xem một tam giác như vậy có tồn tại hay không, rõ ràng là có."
date: "2026-06-07T01:46:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 407
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 239 (Div. 1)"
rating: 1600
weight: 407
solve_time_s: 273
verified: true
draft: false
---

[CF 407A - Tam giác](https://codeforces.com/problemset/problem/407/A) 

**Đánh giá:** 1600 
**Tags:** sức mạnh vũ phu, hình học, cách thực hiện, toán học 
**Thời gian giải:** 4m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên dương`a`Và`b`, là độ dài các cạnh của một tam giác vuông. Nhiệm vụ không phải là kiểm tra xem một tam giác như vậy có tồn tại hay không, rõ ràng là có. Thay vào đó, chúng ta phải đặt tam giác đó trên mạng nguyên sao cho cả ba đỉnh đều có tọa độ nguyên và không có cạnh nào của nó song song với trục tọa độ. 

Một cách hữu ích để suy nghĩ về vấn đề này là xây dựng hai vectơ vuông góc có độ dài chính xác bằng`a`Và`b`. Nếu chúng ta đặt một đỉnh ở gốc tọa độ thì có thể thu được hai đỉnh còn lại bằng cách di chuyển dọc theo các vectơ này. Tam giác thu được sẽ tự động vuông góc. 

Các giới hạn là rất nhỏ. Cả hai`a`Và`b`nhiều nhất là 1000. Điều này có nghĩa là chúng ta có thể liệt kê tất cả các cặp tọa độ nguyên có tổng bình phương bằng`a²`hoặc`b²`. Thậm chí một`O(1000²)`tìm kiếm là hoàn toàn an toàn. 

Khó khăn chính không phải là hiệu suất mà là hình học. Chúng ta cần tọa độ nguyên, một góc vuông, độ dài cạnh quy định và ngoài ra không có cạnh nào có thể nằm ngang hoặc dọc. 

Một sai lầm dễ mắc phải là xây dựng tam giác hiển nhiên```
(0,0), (a,0), (0,b)
```Ví dụ, với đầu vào```
3 4
```cái này có độ dài cạnh chính xác, nhưng hai cạnh song song với trục tọa độ nên không hợp lệ. 

Một cái bẫy khác là tìm các vectơ nguyên có độ dài chính xác nhưng lại cho phép chúng nằm trên cùng một đường thẳng. Ví dụ: nếu cả hai vectơ cùng hướng thì ba điểm sẽ thẳng hàng và không tạo thành một tam giác. 

Một trường hợp tinh tế hơn là khi không tồn tại sự phân tách số nguyên. Đối với đầu vào```
1 1
```các vectơ nguyên duy nhất có độ dài 1 là`(±1,0)`Và`(0,±1)`. Bất kỳ công trình nào sử dụng chúng nhất thiết phải tạo ra một cạnh song song với một trục. Câu trả lời đúng là```
NO
```và giải pháp chỉ kiểm tra độ dài mà không thực thi điều kiện "không có cạnh song song" sẽ tạo ra một hình tam giác không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tìm kiếm tất cả tọa độ nguyên của ba đỉnh bên trong một hộp lớn nào đó và kiểm tra mọi khả năng. Cách tiếp cận như vậy rõ ràng là đúng vì mọi tam giác hợp lệ cuối cùng sẽ được kiểm tra. Vấn đề là ngay cả một hộp có chiều dài cạnh 2000 cũng chứa hàng triệu điểm mạng, làm cho số lượng bộ ba lớn về mặt thiên văn. 

Cấu trúc của bài toán cho chúng ta một cách biểu diễn tốt hơn nhiều. Vì tam giác là góc vuông nên chúng ta có thể đặt góc vuông ở gốc tọa độ. Cho một chân được biểu diễn bằng vector```
(x1, y1)
```với chiều dài`a`, và cái còn lại theo vectơ```
(x2, y2)
```với chiều dài`b`. 

Các vectơ phải thỏa mãn```
x1² + y1² = a²
x2² + y2² = b²
```và chúng phải vuông góc:```
x1*x2 + y1*y2 = 0
```Một quan sát quan trọng là nếu`(x1, y1)`đã biết thì vectơ vuông góc là```
(-y1, x1)
```hoặc```
(y1, -x1)
```Chúng có độ dài chính xác giống như`(x1, y1)`. 

Giả sử chúng ta tìm thấy số nguyên`(x, y)`như vậy```
x² + y² = a².
```Khi đó vectơ```
(-y, x)
```có chiều dài`a`. Để có được một vectơ vuông góc có chiều dài`b`, chúng ta có thể mở rộng quy mô bằng cách`b / a`:```
(-y*b/a, x*b/a)
```Để vectơ này có tọa độ nguyên, cả hai`y*b`Và`x*b`phải chia hết cho`a`. 

Thay vì lập luận bằng phân số, chúng ta có thể liệt kê trực tiếp tất cả các cặp số nguyên`(x, y)`thỏa mãn```
x² + y² = a²
```và tất cả các cặp số nguyên`(u, v)`thỏa mãn```
u² + v² = b².
```Sau đó chúng ta chỉ cần tìm một cặp trong đó```
x*u + y*v = 0.
```Giới hạn rất nhỏ nên việc tìm kiếm này rất dễ dàng. 

Có một quan sát thậm chí còn đơn giản hơn được sử dụng trong hầu hết các giải pháp được chấp nhận. Liệt kê tất cả các cặp số nguyên khác 0`(x, y)`với```
x² + y² = a².
```Sau đó lấy```
(u, v) = (-y, x).
```Điều này tự động vuông góc và có chiều dài`a`. 

Nếu chúng ta muốn chiều dài`b`, chúng ta có thể tìm hệ số tỷ lệ```
k = b / a
```giữ tọa độ nguyên. Tương tự, chúng ta tìm kiếm một phân tích khác của`b²`và kiểm tra xem nó có khớp với hướng vuông góc hay không. 

Việc triển khai tiêu chuẩn được chấp nhận liệt kê tất cả các phân rã của`a²`và trực tiếp kiểm tra xem tọa độ vuông góc tỷ lệ tương ứng có phải là số nguyên hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các vị trí đỉnh | Rất lớn | Rất lớn | Quá chậm | 
| Liệt kê các vectơ mạng trên vòng tròn | O(a + b) đến O(1000²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố định đỉnh vuông tại`(0,0)`. 
2. Liệt kê tất cả các số nguyên dương`x`từ`1`ĐẾN`a-1`. 
3. Tính toán```
y² = a² - x².
```Nếu như`y²`đây không phải là một hình vuông hoàn hảo, đây`x`không thể thuộc về một vectơ nguyên có độ dài`a`. 
4. Hãy để`y = sqrt(y²)`. Bỏ qua trường hợp`y = 0`bởi vì điều đó sẽ tạo ra một mặt trục song song. 
5. Xét phương vuông góc`(-y, x)`. chiều dài của nó cũng`a`. 
6. Chia tỷ lệ vectơ vuông góc này theo`b / a`. Tọa độ ứng cử viên trở thành```
X = -y*b/a
Y = x*b/a.
```7. Kiểm tra xem cả hai`X`Và`Y`là số nguyên. Điều này tương đương với việc kiểm tra```
(y*b) % a == 0
(x*b) % a == 0.
```8. Nếu cả hai điều kiện chia hết đều giữ nguyên và không tọa độ nào bằng 0, thì xuất ra```
(0,0)
(x,y)
(X,Y)
```và chấm dứt. 
9. Nếu tất cả các phân tách đã được sử dụng hết mà không thành công, thì xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Chân đầu tiên là vector`(x,y)`, có độ dài chính xác bằng`a`vì nó thỏa mãn`x²+y²=a²`. 

Chân thứ hai được lấy từ vectơ vuông góc`(-y,x)`và thu nhỏ bằng`b/a`. Chia tỷ lệ thay đổi độ dài của nó từ`a`ĐẾN`b`, trong khi vẫn giữ nguyên độ vuông góc. Do đó tích vô hướng của hai vectơ chân vẫn bằng 0, nên góc ở gốc tọa độ là góc vuông. 

Việc kiểm tra tính chia hết đảm bảo tọa độ nguyên. Việc loại trừ tọa độ 0 sẽ ngăn không cho bất kỳ cạnh nào trở thành ngang hoặc dọc. Mọi cách xây dựng hợp lệ với góc vuông ở gốc tọa độ đều tương ứng với một vectơ mạng có độ dài`a`, do đó việc liệt kê không thể bỏ sót một giải pháp nào. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

a, b = map(int, input().split())

for x in range(1, a):
    y2 = a * a - x * x
    y = isqrt(y2)

    if y * y != y2 or y == 0:
        continue

    if (y * b) % a != 0:
        continue
    if (x * b) % a != 0:
        continue

    X = -(y * b) // a
    Y = (x * b) // a

    if X == 0 or Y == 0:
        continue

    print("YES")
    print(0, 0)
    print(x, y)
    print(X, Y)
    sys.exit()

print("NO")
```Mã này tuân theo cấu trúc hình học trực tiếp. 

Vòng lặp liệt kê tất cả các vectơ mạng nguyên có độ dài`a`có tọa độ dương. Đối với mỗi ứng cử viên, biểu thức```
y2 = a * a - x * x
```tính số bình phương còn lại. sử dụng`isqrt`tránh các vấn đề về độ chính xác của dấu phẩy động khi kiểm tra xem`y2`là một hình vuông hoàn hảo 

Việc kiểm tra khả năng phân chia là cốt lõi của giải pháp. Họ xác minh rằng việc chia tỷ lệ vectơ vuông góc bằng`b/a`giữ nguyên cả hai tọa độ. Nếu không có những kiểm tra này, phép chia số nguyên sẽ âm thầm cắt bớt các giá trị và tạo ra một tam giác không chính xác. 

Người bảo vệ cuối cùng```
if X == 0 or Y == 0:
```loại bỏ các công trình có thể tạo ra một mặt trục song song. 

Khi tìm thấy cấu hình hợp lệ, chương trình sẽ in cấu hình đó ngay lập tức và thoát. Vì mọi tam giác hợp lệ đều được chấp nhận nên không cần tiếp tục tìm kiếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
```| x | y2 = 1 - x2 | Hình vuông hoàn hảo? | Hành động | 
| --- | --- | --- | --- | 
| không | không | không | vòng lặp không bao giờ chạy | 

Đầu ra:```
NO
```Từ`a = 1`, không có số nguyên`x`trong phạm vi`[1, a-1]`. Không tồn tại vectơ mạng không trục có độ dài 1, vì vậy câu trả lời bị từ chối chính xác. 

### Ví dụ 2 

đầu vào:```
5 10
```| x | y² | y | (y·b)%a | (x·b)%a | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 24 | không vuông | - | - | bỏ qua | 
| 2 | 21 | không vuông | - | - | bỏ qua | 
| 3 | 16 | 4 | 0 | 0 | tìm thấy | 

Tọa độ kết quả là```
(0,0)
(3,4)
(-8,6)
```Xác minh:```
3² + 4² = 25 = 5²
(-8)² + 6² = 100 = 10²
3*(-8) + 4*6 = 0
```Dấu vết thể hiện tính bất biến chính: vectơ thứ hai luôn vuông góc với vectơ thứ nhất và có độ dài cần thiết sau khi chia tỷ lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a) | Một lần quét qua các giá trị của`x`từ`1`ĐẾN`a-1`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`a ≤ 1000`, vòng lặp thực hiện tối đa 999 lần lặp. Thời gian chạy thực tế là tức thời và mức sử dụng bộ nhớ không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def solve():
    a, b = map(int, input().split())

    for x in range(1, a):
        y2 = a * a - x * x
        y = isqrt(y2)

        if y * y != y2 or y == 0:
            continue

        if (y * b) % a != 0:
            continue
        if (x * b) % a != 0:
            continue

        X = -(y * b) // a
        Y = (x * b) // a

        if X == 0 or Y == 0:
            continue

        print("YES")
        print(0, 0)
        print(x, y)
        print(X, Y)
        return

    print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out

# provided sample
assert run("1 1\n") == "NO\n"

# custom cases
assert run("5 10\n").startswith("YES")
assert run("2 2\n") == "NO\n"
assert run("3 4\n") == "NO\n"
assert run("1000 1000\n").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 10`| CÓ | Điển hình thi công thành công | 
|`2 2`| KHÔNG | Giá trị nhỏ không có phân tách mạng hợp lệ | 
|`3 4`| KHÔNG | Độ dài Pythagore cổ điển nhưng không thể sắp xếp dưới những ràng buộc | 
|`1000 1000`| CÓ | Giá trị biên lớn | 

## Vỏ cạnh 

Đối với đầu vào```
1 1
```phạm vi vòng lặp trống vì không có giá trị dương`x < 1`. Thuật toán in ngay lập tức`"NO"`. Điều này phù hợp với thực tế là mọi vectơ mạng có độ dài 1 đều được căn chỉnh theo trục. 

Đối với đầu vào```
2 2
```ứng cử viên duy nhất là`x = 1`, cho```
y² = 3
```đó không phải là một hình vuông hoàn hảo. Không tồn tại vectơ mạng có độ dài 2 với cả hai tọa độ khác 0, vì vậy câu trả lời là`"NO"`. 

Đối với đầu vào```
3 4
```vectơ ứng cử viên là`(x,y)=(?,?)`. Sự phân hủy duy nhất của`3²`có hiệu quả`(0,3)`Và`(3,0)`, cả hai đều chứa tọa độ bằng 0. Thuật toán sẽ tự động từ chối chúng và in`"NO"`. 

Đối với đầu vào```
5 5
```thuật toán tìm thấy`(3,4)`từ```
3² + 4² = 25.
```Vectơ vuông góc là```
(-4,3),
```đã có độ dài 5. Tất cả tọa độ đều khác 0, vì vậy nó xuất ra```
YES
0 0
3 4
-4 3
```Mọi cạnh đều tránh ngang hoặc dọc và tất cả tọa độ vẫn là số nguyên.
