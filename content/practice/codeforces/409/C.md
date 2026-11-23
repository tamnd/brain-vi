---
title: "CF 409C - Kiệt tác vĩ đại"
description: "Bài toán cho chúng ta một danh sách các số nguyên biểu thị số lượng của một số thành phần giả kim. Mục tiêu là xác định ước số chung lớn nhất (GCD) của các đại lượng này. Nói cách khác, chúng ta muốn tìm số nguyên lớn nhất $d$ sao cho mỗi số đã cho đều chia hết cho $d$."
date: "2026-06-07T01:55:34+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 409
codeforces_index: "C"
codeforces_contest_name: "April Fools Day Contest 2014"
rating: 1700
weight: 409
solve_time_s: 252
verified: true
draft: false
---

[CF 409C - Magnum Opus](https://codeforces.com/problemset/problem/409/C) 

**Đánh giá:** 1700 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho chúng ta một danh sách các số nguyên biểu thị số lượng của một số thành phần giả kim. Mục tiêu là xác định ước số chung lớn nhất (GCD) của các đại lượng này. Nói cách khác, chúng ta muốn tìm số nguyên lớn nhất$d$sao cho mỗi số đã cho đều chia hết cho$d$. 

Mảng đầu vào có thể có tới 100 phần tử, mỗi phần tử nằm trong khoảng từ 0 đến 100. Vì các số này nhỏ nên ngay cả các phép tính số học đơn giản cũng nhanh, nhưng chúng ta vẫn phải cẩn thận với các số 0: GCD của một số và số 0 chính là số đó, vì vậy việc bỏ qua các số 0 có thể dẫn đến các câu trả lời sai. 

Trường hợp cạnh tinh tế là khi tất cả các số đều bằng 0. Trong trường hợp này, về mặt toán học, GCD không được xác định, nhưng trong bối cảnh của vấn đề này, người ta thường trả về 0 hoặc xử lý nó một cách nhất quán. Một trường hợp khác là khi chỉ có một số khác 0: GCD chính là số đó. Việc lặp lại và kiểm tra khả năng chia hết cho tất cả các ứng cử viên từ 1 đến số nhỏ nhất có hiệu quả nhưng chậm một cách không cần thiết và quan trọng hơn là không mở rộng nếu phạm vi số lớn hơn. 

Đầu ra là một số nguyên duy nhất, GCD của tất cả các giá trị đầu vào. Điều này trực tiếp ánh xạ tới “kích thước lô” tối đa mà bạn có thể chia đều tất cả số lượng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp qua mọi số nguyên từ 1 đến số tối thiểu của các số đầu vào và kiểm tra xem nó có chia hết tất cả các phần tử hay không. Điều này có tác dụng vì nếu một số chia hết cho tất cả các phần tử thì nó không được lớn hơn phần tử nhỏ nhất. Đối với kích thước đầu vào của chúng tôi, số lượng tối thiểu có thể nhiều nhất là 100, vì vậy trong bài toán này, phương pháp này là khả thi. Tuy nhiên, nhìn chung, nếu con số có thể lên tới$10^9$, việc lặp lại đến mức tối thiểu sẽ là quá chậm, khiến$O(n \cdot \text{min}(a_i))$hoạt động. 

Thông tin quan trọng cho một giải pháp tinh tế hơn đến từ thuật toán Euclide: GCD của hai số$x$Và$y$có thể được tính toán đệ quy như$\gcd(y, x \% y)$. Phương pháp này nhanh vì mỗi phép toán modulo làm giảm đáng kể kích thước bài toán và độ phức tạp là logarit trong chính các con số. Việc mở rộng điều này thành nhiều số rất đơn giản: GCD của một mảng chỉ là$\gcd(\gcd(a_1, a_2), a_3, ..., a_n)$. 

Cách tiếp cận vũ phu có hiệu quả vì số lượng nhỏ nhưng sẽ thất bại khi số lượng ngày càng lớn. Cách tiếp cận Euclide làm giảm vấn đề thành một chuỗi các phép toán đơn giản với thời gian logarit được đảm bảo, làm cho nó trở nên mạnh mẽ đối với các đầu vào lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * phút(a_i)) | O(1) | Hoạt động với số lượng nhỏ, quá chậm đối với số lượng lớn | 
| GCD Euclide | O(n * log(max(a_i))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách các số nguyên từ đầu vào. Chúng đại diện cho số lượng của các thành phần. 
2. Khởi tạo một biến`current_gcd`với số đầu tiên trong mảng. Điều này phục vụ như GCD đang chạy của chúng tôi. 
3. Lặp lại phần còn lại của mảng. Với mỗi số, hãy cập nhật`current_gcd`là GCD của`current_gcd`và số hiện tại bằng thuật toán Euclide. Điều này đảm bảo rằng sau mỗi bước,`current_gcd`chia tất cả các số nhìn thấy cho đến nay. 
4. Sau khi quá trình lặp hoàn tất,`current_gcd`là GCD của toàn bộ mảng. In giá trị này. 

Tại sao nó hoạt động: Thuật toán Euclide đảm bảo rằng$\gcd(x, y)$là số nguyên lớn nhất chia hết cả x và y. Bằng cách áp dụng nó lặp đi lặp lại trên tất cả các số, bất biến mà`current_gcd`chia mọi số gặp phải được duy trì. Không có số nào lớn hơn có thể chia tất cả các phần tử hơn GCD đang chạy này, vì vậy giá trị cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd
from functools import reduce

# read input and convert to integers
arr = list(map(int, input().split()))

# compute gcd of the entire array using reduce
result = reduce(gcd, arr)

print(result)
```Chúng tôi sử dụng`reduce`để gấp`gcd`chức năng trên toàn mảng. sử dụng`math.gcd`tránh thực hiện thuật toán Euclide một cách thủ công và đảm bảo tính chính xác. Chuyển đổi chuỗi đầu vào thành số nguyên với`map(int, ...)`xử lý khoảng cách tùy ý trong đầu vào. Các trường hợp cạnh như số 0 được xử lý tự động:`gcd(x, 0)`trả lại`x`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
2 4 6 8 10
```| Bước | hiện tại_gcd | Số tiếp theo | gcd(current_gcd, số tiếp theo) | 
| --- | --- | --- | --- | 
| Ban đầu | 2 | 4 | 2 | 
| Bước 1 | 2 | 6 | 2 | 
| Bước 2 | 2 | 8 | 2 | 
| Bước 3 | 2 | 10 | 2 | 

Đầu ra:`2`Dấu vết này cho thấy GCD đang chạy không bao giờ vượt quá GCD thực và xác định chính xác ước số lớn nhất. 

**Đầu vào tùy chỉnh:**```
5 10 15 20
```| Bước | hiện tại_gcd | Số tiếp theo | gcd(current_gcd, số tiếp theo) | 
| --- | --- | --- | --- | 
| Ban đầu | 5 | 10 | 5 | 
| Bước 1 | 5 | 15 | 5 | 
| Bước 2 | 5 | 20 | 5 | 

Đầu ra:`5`Điều này chứng tỏ khi phần tử đầu tiên là GCD thì thuật toán ngay lập tức ổn định, khẳng định tính bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * log(max(a_i))) | Mỗi phép tính gcd là logarit theo độ lớn của các số, lặp lại n-1 lần | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng số nguyên | 

Ràng buộc đầu vào$a_i \le 100$làm cho thuật toán này cực kỳ nhanh trong thực tế. Dung lượng bộ nhớ tối thiểu và thuật toán chạy thoải mái dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd
from functools import reduce

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    arr = list(map(int, input().split()))
    return str(reduce(gcd, arr))

# provided sample
assert run("2 4 6 8 10\n") == "2", "sample 1"

# single element
assert run("42\n") == "42", "single element"

# all zeros
assert run("0 0 0\n") == "0", "all zeros"

# mixed zeros
assert run("0 6 12 18\n") == "6", "zeros and non-zeros"

# all equal
assert run("7 7 7 7 7\n") == "7", "all equal numbers"

# prime numbers
assert run("3 5 7 11\n") == "1", "coprime numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 42 | 42 | trường hợp cạnh phần tử đơn | 
| 0 0 0 | 0 | tất cả các số không được xử lý chính xác | 
| 0 6 12 18 | 6 | số không không phá vỡ GCD | 
| 7 7 7 7 7 | 7 | số giống nhau không làm giảm GCD | 
| 3 5 7 11 | 1 | số nguyên tố cùng nhau trả về 1 | 

## Vỏ cạnh 

Nếu đầu vào chỉ chứa số 0, như`0 0 0`,`reduce(gcd, arr)`trả về đúng 0. Nếu đầu vào là`[0, 6, 12]`, thuật toán coi số 0 là phần tử trung tính vì`gcd(x, 0) = x`, do đó GCD đang chạy ổn định ở mức 6. Đối với mảng một phần tử`[42]`, thuật toán trả về chính xác số đó. 

Mọi trường hợp biên đều được xử lý một cách tự nhiên bằng thuật toán Euclide và`reduce`cách tiếp cận này, do đó không cần thêm điều kiện nào nữa.
