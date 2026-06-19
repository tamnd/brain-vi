---
title: "CF 100C - A+B"
description: "Nhiệm vụ này thoạt nhìn có vẻ tầm thường: đọc hai số nguyên và in tổng của chúng. Việc bắt được ẩn bên trong các ràng buộc. Mỗi số có thể chứa tối đa 500 chữ số thập phân, lớn hơn nhiều so với phạm vi số nguyên 32 bit hoặc 64 bit tiêu chuẩn ở nhiều ngôn ngữ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "C"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1400
weight: 100
solve_time_s: 87
verified: true
draft: false
---

[CF 100C - A+B](https://codeforces.com/problemset/problem/100/C) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này thoạt nhìn có vẻ tầm thường: đọc hai số nguyên và in tổng của chúng. Việc bắt được ẩn bên trong các ràng buộc. Mỗi số có thể chứa tối đa 500 chữ số thập phân, lớn hơn nhiều so với phạm vi số nguyên 32 bit hoặc 64 bit tiêu chuẩn ở nhiều ngôn ngữ. 

Cho ta hai số nguyên dương, mỗi số được viết trên một dòng riêng, không có số 0 đứng đầu. Chúng ta phải xuất ra tổng thập phân chính xác của chúng, đồng thời không có số 0 đứng đầu. 

Hạn chế quan trọng là kích thước của các con số. Một giá trị có 500 chữ số không thể vừa với các kiểu số nguyên thông thường như`int`hoặc`long long`trong C++. Phép cộng số trực tiếp sử dụng số học có kích thước cố định sẽ tràn ngay lập tức. 

Giới hạn thời gian là cực kỳ hào phóng đối với kích thước đầu vào nhỏ như vậy. Ngay cả một thuật toán xử lý từng chữ số một cũng rất nhỏ trong thực tế. Vì mỗi số có tối đa 500 chữ số nên`O(n)`giải pháp ở đâu`n`số chữ số là quá đủ. 

Các trường hợp cạnh chính đến từ việc truyền bá. 

Hãy xem xét đầu vào này:```
999
1
```Đầu ra đúng là:```
1000
```Việc triển khai từng chữ số một cách bất cẩn có thể quên thêm phần mang cuối cùng sau khi xử lý tất cả các vị trí. 

Một trường hợp tế nhị khác là độ dài khác nhau:```
12345
9
```Đầu ra đúng là:```
12354
```Nếu số ngắn hơn không được đệm chính xác, các chỉ số có thể vượt quá giới hạn hoặc các chữ số có thể bị lệch. 

Ngoài ra còn có đầu vào nhỏ nhất có thể:```
1
1
```Đầu ra đúng là:```
2
```Điều này kiểm tra xem việc triển khai có hoạt động hay không khi chỉ có một chữ số và không có logic mang phức tạp. 

Trong Python, các số nguyên tích hợp đã hỗ trợ độ chính xác tùy ý, do đó toàn bộ vấn đề giảm xuống việc đọc các số và in tổng của chúng. Tuy nhiên, hiểu được tại sao vấn đề này lại quan trọng là ý tưởng cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là phép cộng số nguyên lớn thủ công. Chúng ta có thể lưu trữ cả hai số dưới dạng chuỗi, xử lý chúng từ phải sang trái, thêm các chữ số tương ứng, theo dõi số mang và xây dựng chữ số kết quả theo từng chữ số. 

Điều này hoạt động vì phép cộng thập phân là cục bộ. Mỗi vị trí chỉ phụ thuộc vào hai chữ số hiện tại và số mang từ vị trí trước đó. Vì mỗi số có tối đa 500 chữ số nên tổng số phép tính chỉ có vài trăm phép cộng và phép tính modulo. 

Phương pháp vũ phu đã đủ nhanh rồi. Độ phức tạp của nó là tuyến tính theo số chữ số, ở đây rất nhỏ. 

Cách tiếp cận thuận tiện hơn trong Python là dựa vào các số nguyên có độ chính xác tùy ý của Python. Số nguyên Python tự động mở rộng để chứa các giá trị rất lớn, vì vậy chúng ta có thể chuyển đổi trực tiếp chuỗi thành số nguyên, cộng chúng và in kết quả. 

Phương pháp thủ công tồn tại vì nhiều ngôn ngữ lập trình không thể biểu thị các số có 500 chữ số nguyên bản. Python loại bỏ giới hạn đó, biến vấn đề thành phép toán số học một dòng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cộng từng chữ số theo cách thủ công | O(n) | O(n) | Đã chấp nhận | 
| Số nguyên có độ chính xác tùy ý trong Python | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên đầu tiên dưới dạng chuỗi từ đầu vào. 
2. Đọc số nguyên thứ hai dưới dạng chuỗi từ đầu vào. 
3. Chuyển đổi cả hai chuỗi thành số nguyên Python. Python tự động xử lý các số lớn tùy ý, do đó không cần logic số nguyên lớn tùy chỉnh. 
4. Cộng hai số nguyên. 
5. In tổng kết quả. 

Tại sao nó hoạt động: 

Số nguyên Python được triển khai với số học chính xác tùy ý trong nội bộ. Thay vì lưu trữ các giá trị trong các từ máy có kích thước cố định, Python tự động phân bổ đủ bộ nhớ để biểu thị các số nguyên rất lớn. Do đó, việc cộng hai số có 500 chữ số sẽ tạo ra kết quả đúng về mặt toán học mà không bị tràn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input().strip())
b = int(input().strip())

print(a + b)
```Chương trình bắt đầu bằng cách sử dụng đầu vào nhanh thông qua`sys.stdin.readline`, đó là tiêu chuẩn trong lập trình cạnh tranh. 

các`strip()`các cuộc gọi loại bỏ các ký tự dòng mới ở cuối. Không có họ,`int()`vẫn hoạt động trong Python, nhưng sử dụng`strip()`giữ cho việc phân tích cú pháp rõ ràng và sạch sẽ. 

Chi tiết quan trọng là việc sử dụng công cụ tích hợp sẵn của Python`int`kiểu. Không giống như các loại số nguyên có chiều rộng cố định trong nhiều ngôn ngữ, số nguyên Python tự động tăng lên khi cần thiết. Điều này tránh tràn hoàn toàn, ngay cả đối với các số có 500 chữ số. 

trận chung kết`print(a + b)`trực tiếp xuất ra biểu diễn thập phân chính xác của tổng. Python tự động định dạng số nguyên không có số 0 đứng đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
3
```Dấu vết bước: 

| Bước | một | b | Tổng hợp | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 3 | - | 
| Thêm số | 2 | 3 | 5 | 
| Đầu ra | 2 | 3 | 5 | 

Điều này thể hiện trường hợp đơn giản nhất có thể, phép cộng một chữ số mà không cần truyền dẫn. 

### Ví dụ 2 

đầu vào:```
999
1
```Dấu vết bước: 

| Bước | một | b | Tổng hợp | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 999 | 1 | - | 
| Thêm số | 999 | 1 | 1000 | 
| Đầu ra | 999 | 1 | 1000 | 

Ví dụ này thể hiện sự lan truyền mang trên nhiều chữ số. Việc triển khai thủ công phải xử lý chính xác lần mang cuối cùng tạo ra chữ số hàng đầu mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Python xử lý tất cả các chữ số của các số trong phép cộng | 
| Không gian | O(n) | Các số nguyên và kết quả yêu cầu lưu trữ tỷ lệ thuận với số chữ số | 

Đây,`n`là số chữ số của số lớn hơn. Vì kích thước tối đa chỉ có 500 chữ số nên thời gian chạy và mức sử dụng bộ nhớ không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a = int(input().strip())
    b = int(input().strip())

    print(a + b)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n3\n") == "5\n", "sample 1"

# minimum values
assert run("1\n1\n") == "2\n", "minimum values"

# carry propagation
assert run("999\n1\n") == "1000\n", "final carry"

# different lengths
assert run("12345\n9\n") == "12354\n", "different digit counts"

# very large numbers
assert run(
    "999999999999999999999999999999\n"
    "1\n"
) == "1000000000000000000000000000000\n", "large integer handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Đầu vào hợp lệ nhỏ nhất | 
|`999 1`|`1000`| Truyền bá trên tất cả các chữ số | 
|`12345 9`|`12354`| Độ dài số khác nhau | 
| Giá trị lớn 30 chữ số cộng 1 | Kết quả lớn 31 chữ số | Số học chính xác tùy ý | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
999
1
```Thuật toán chuyển đổi cả hai giá trị thành số nguyên Python và tính toán:```
999 + 1 = 1000
```Đầu ra trở thành:```
1000
```Trường hợp này xác nhận rằng việc truyền mang qua mỗi chữ số được xử lý chính xác. Việc triển khai thủ công có thể quên phần đầu dẫn bổ sung, nhưng số học số nguyên của Python sẽ tự động xử lý nó. 

Bây giờ hãy xem xét các số có độ dài khác nhau:```
12345
9
```Phép cộng trở thành:```
12345 + 9 = 12354
```Đầu ra là:```
12354
```Điều này xác minh rằng các vấn đề căn chỉnh không tồn tại khi sử dụng số nguyên có độ chính xác tùy ý. Python quản lý nội bộ các vị trí chữ số một cách chính xác. 

Cuối cùng, hãy xem xét đầu vào hợp lệ nhỏ nhất:```
1
1
```Chương trình tính toán:```
1 + 1 = 2
```và in:```
2
```Điều này xác nhận việc triển khai hoạt động ngay cả ở ranh giới dưới của các ràng buộc.
