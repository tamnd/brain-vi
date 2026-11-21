---
title: "CF 221A - Chú Voi Nhỏ Và Chức Năng"
description: "Chúng ta được cung cấp một thủ tục đệ quy thực hiện phép hoán vị các số từ 1 đến n. Hàm này hoạt động như thế này: Đối với f(x), trước tiên, nó xử lý đệ quy các phần tử x - 1 đầu tiên, sau đó hoán đổi vị trí x - 1 và x."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 221
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 136 (Div. 2)"
rating: 1000
weight: 221
solve_time_s: 70
verified: true
draft: false
---

[CF 221A - Chú voi nhỏ và Chức năng](https://codeforces.com/problemset/problem/221/A) 

**Đánh giá:** 1000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một thủ tục đệ quy thực hiện phép hoán vị các số từ`1`ĐẾN`n`. 

Chức năng hoạt động như thế này: 

cho`f(x)`, đầu tiên nó xử lý đệ quy lần đầu tiên`x - 1`các phần tử, sau đó hoán đổi vị trí`x - 1`Và`x`. 

Nếu chúng ta gọi`f(n)`, đệ quy cuối cùng thực hiện các hoán đổi này theo thứ tự: 

- hoán đổi vị trí`(1, 2)`- hoán đổi vị trí`(2, 3)`- hoán đổi vị trí`(3, 4)`- ... 
- hoán đổi vị trí`(n - 1, n)`Nhiệm vụ không phải là mô phỏng việc sắp xếp. Thay vào đó, chúng ta phải xây dựng một hoán vị ban đầu sao cho sau khi thực hiện tất cả các phép hoán đổi này, mảng cuối cùng sẽ được sắp xếp theo thứ tự tăng dần. 

Ràng buộc`n ≤ 1000`là rất nhỏ. Ngay cả mô phỏng bậc hai cũng sẽ vừa vặn thoải mái trong giới hạn. Thử thách thực sự là nhận ra khuôn mẫu ẩn bên trong quá trình đệ quy. 

Một lỗi phổ biến là cho rằng câu trả lời bằng cách nào đó phải giống một mảng đảo ngược. Ví dụ, với`n = 4`, bắt đầu từ:```
4 3 2 1
```các giao dịch hoán đổi tạo ra:```
3 4 2 1
3 2 4 1
3 2 1 4
```không được sắp xếp. 

Một sai lầm dễ mắc phải khác là hiểu sai thứ tự hoán đổi. Đệ quy không lặp lại các phần tử bong bóng xung quanh. Mỗi cặp liền kề được hoán đổi đúng một lần, từ trái sang phải. 

Vì`n = 3`, các giao dịch hoán đổi là:```
swap(1,2)
swap(2,3)
```Nếu mảng ban đầu là:```
2 1 3
```sau đó sau khi hoán đổi:```
1 2 3
1 3 2
```kết quả là không chính xác. Việc triển khai bất cẩn không theo dõi thứ tự hoán đổi thực tế có thể dễ dàng tạo ra các cấu trúc không hợp lệ như vậy. 

Trường hợp cạnh nhỏ nhất là`n = 1`. Không có sự hoán đổi nào xảy ra cả, vì vậy câu trả lời hợp lệ duy nhất là:```
1
```Bất kỳ giải pháp nào dựa vào các phần tử dịch chuyển mà không xử lý riêng phần này có thể vô tình tạo ra một mảng trống hoặc lập chỉ mục không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là thử mọi hoán vị của các số từ`1`ĐẾN`n`, mô phỏng quá trình đệ quy và kiểm tra xem mảng cuối cùng có được sắp xếp hay không. 

Điều này hoạt động vì hàm đệ quy có tính xác định. Khi hoán vị bắt đầu được cố định, mảng kết quả sau tất cả các lần hoán đổi sẽ được xác định duy nhất. 

Đối với mỗi hoán vị, chúng tôi thực hiện`n - 1`hoán đổi, vì vậy việc kiểm tra một chi phí ứng cử viên`O(n)`. Vấn đề là số lượng hoán vị. có`n!`khả năng. 

Ngay cả đối với`n = 10`, điều này đã trở thành:```
10! = 3,628,800
```Việc thử tất cả các hoán vị một cách nhanh chóng trở nên không thể. 

Quan sát quan trọng là hàm đệ quy thực hiện một phép biến đổi rất đơn giản. Mỗi phần tử dịch chuyển sang trái một vị trí, ngoại trừ phần tử đầu tiên di chuyển về cuối. 

Ví dụ:```
[a1, a2, a3, a4]
```sau khi hoán đổi trở thành:```
[a2, a3, a4, a1]
```Chúng tôi có thể xác minh điều này trực tiếp:```
swap(1,2): [a2, a1, a3, a4]
swap(2,3): [a2, a3, a1, a4]
swap(3,4): [a2, a3, a4, a1]
```Bây giờ vấn đề trở nên dễ dàng. 

Chúng tôi muốn mảng cuối cùng là:```
1 2 3 ... n
```Vì thao tác xoay mảng sang trái một vị trí, hoán vị ban đầu phải là mảng được sắp xếp xoay sang phải một vị trí:```
n 1 2 3 ... n-1
```Sau khi hoán đổi đệ quy:```
1 2 3 ... n
```Việc xây dựng này là ngay lập tức và chạy theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Xây dựng hoán vị bắt đầu bằng`n`. 

Phần tử này phải di chuyển đến vị trí cuối cùng sau tất cả các lần hoán đổi, vì quá trình đệ quy sẽ xoay mảng sang trái một bước. 
3. Nối tất cả các số nguyên từ`1`ĐẾN`n - 1`theo thứ tự tăng dần. 

Sau khi xoay trái do hoán đổi gây ra, các phần tử này sẽ chuyển sang vị trí được sắp xếp chính xác của chúng. 
4. In hoán vị kết quả. 

### Tại sao nó hoạt động 

Hàm đệ quy thực hiện các phép hoán đổi liền kề từ trái sang phải đúng một lần. Trình tự này biến đổi:```
[a1, a2, a3, ..., an]
```vào trong:```
[a2, a3, ..., an, a1]
```đó là phép quay trái một vị trí. 

Nếu chúng ta muốn mảng cuối cùng trở thành:```
[1, 2, 3, ..., n]
```thì mảng ban đầu phải là phép biến đổi nghịch đảo, phép quay phải của mảng đã được sắp xếp:```
[n, 1, 2, ..., n-1]
```Áp dụng các hoán đổi đệ quy cho hoán vị này sẽ tạo ra thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

ans = [n] + list(range(1, n))

print(*ans)
```Việc thực hiện trực tiếp theo quan sát toán học. 

Mảng bắt đầu bằng`n`bởi vì quá trình đệ quy di chuyển phần tử đầu tiên về cuối. Tất cả các số còn lại đã được sắp xếp theo thứ tự nên sau khi xoay trái chúng sẽ trở thành:```
1 2 3 ... n
```Biểu thức:```
list(range(1, n))
```tạo chính xác các số từ`1`bởi vì`n - 1`. 

Một điều kiện biên tinh tế xuất hiện khi`n = 1`. 

Trong trường hợp đó:```
list(range(1, 1))
```tạo ra một danh sách trống, do đó kết quả cuối cùng sẽ trở thành:```
[1]
```điều đó hoàn toàn chính xác 

Không cần mô phỏng giao dịch hoán đổi. Hành vi đệ quy hoàn toàn được ghi lại bằng quan sát xoay. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Hoán vị được xây dựng: 

| Bước | Mảng | 
| --- | --- | 
| Xây dựng ban đầu | [1] | 

Không có giao dịch hoán đổi nào xảy ra vì quá trình đệ quy dừng ngay lập tức. 

Mảng cuối cùng:```
1
```Ví dụ này xác nhận trường hợp ranh giới nhỏ nhất hoạt động chính xác. 

### Ví dụ 2 

đầu vào:```
4
```Hoán vị được xây dựng:```
4 1 2 3
```Bây giờ hãy theo dõi các hoán đổi đệ quy. 

| Hoạt động | Trạng thái mảng | 
| --- | --- | 
| Ban đầu | [4, 1, 2, 3] | 
| trao đổi (1,2) | [1, 4, 2, 3] | 
| trao đổi (2,3) | [1, 2, 4, 3] | 
| trao đổi (3,4) | [1, 2, 3, 4] | 

Quá trình này thể hiện rõ ràng hành vi xoay trái. Phần tử đầu tiên dần dần di chuyển về cuối trong khi mọi phần tử khác dịch chuyển sang trái một vị trí. 

### Ví dụ 3 

đầu vào:```
5
```Hoán vị được xây dựng:```
5 1 2 3 4
```Dấu vết: 

| Hoạt động | Trạng thái mảng | 
| --- | --- | 
| Ban đầu | [5, 1, 2, 3, 4] | 
| trao đổi (1,2) | [1, 5, 2, 3, 4] | 
| trao đổi (2,3) | [1, 2, 5, 3, 4] | 
| trao đổi (3,4) | [1, 2, 3, 5, 4] | 
| trao đổi (4,5) | [1, 2, 3, 4, 5] | 

Dấu vết này xác nhận rằng việc xây dựng cũng hoạt động cho các mảng lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xây dựng và in một mảng có kích thước`n`| 
| Không gian | O(n) | Mảng đầu ra lưu trữ`n`số nguyên | 

Với`n ≤ 1000`, độ phức tạp tuyến tính là không đáng kể đối với các giới hạn đã cho. Giải pháp dễ dàng phù hợp với cả những hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    ans = [n] + list(range(1, n))

    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("1\n") == "1\n", "sample 1"

# custom cases
assert run("2\n") == "2 1\n", "smallest non-trivial case"

assert run("3\n") == "3 1 2\n", "checks left-rotation logic"

assert run("5\n") == "5 1 2 3 4\n", "general case"

expected = "1000 " + " ".join(str(i) for i in range(1, 1000)) + "\n"
assert run("1000\n") == expected, "maximum size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Ranh giới kích thước tối thiểu | 
|`2`|`2 1`| Hành vi trao đổi đơn | 
|`3`|`3 1 2`| Thi công xoay đúng | 
|`5`|`5 1 2 3 4`| Tính đúng đắn chung | 
|`1000`|`1000 1 2 ... 999`| Xử lý hạn chế tối đa | 

## Vỏ cạnh 

### Trường hợp cạnh 1:`n = 1`đầu vào:```
1
```Cấu trúc thuật toán:```
[1]
```Hàm đệ quy dừng ngay lập tức vì`x = 1`. 

Kết quả cuối cùng:```
1
```Trường hợp này xác nhận việc triển khai xử lý chính xác hậu tố trống được tạo bởi:```
range(1, 1)
```### Edge Case 2: Case nhỏ nhất không tầm thường 

đầu vào:```
2
```Hoán vị được xây dựng:```
2 1
```Dấu vết thực hiện: 

| Hoạt động | Mảng | 
| --- | --- | 
| Ban đầu | [2, 1] | 
| trao đổi (1,2) | [1, 2] | 

Mảng cuối cùng được sắp xếp. 

Trường hợp này xác minh logic lập chỉ mục cho lần hoán đổi liền kề đầu tiên. 

### Trường hợp Edge 3: Xác minh hành vi xoay 

đầu vào:```
4
```Hoán vị được xây dựng:```
4 1 2 3
```Dấu vết thực hiện: 

| Hoạt động | Mảng | 
| --- | --- | 
| Ban đầu | [4, 1, 2, 3] | 
| trao đổi (1,2) | [1, 4, 2, 3] | 
| trao đổi (2,3) | [1, 2, 4, 3] | 
| trao đổi (3,4) | [1, 2, 3, 4] | 

Điều này thể hiện tính bất biến cốt lõi: quá trình đệ quy chính xác là một phép quay trái.
