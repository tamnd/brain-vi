---
title: "CF 347A - Hàng khác biệt"
description: "Cho ta một danh sách gồm n số nguyên và cần sắp xếp chúng thành một hàng sao cho tổng hiệu của các số liên tiếp là lớn nhất. Cụ thể, nếu cách sắp xếp là $x1, x2, dots, xn$ thì giá trị được tính là $(x1 - x2) + (x2 - x3) + dots + (x{n-1} - xn)$."
date: "2026-06-06T18:28:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 347
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 201 (Div. 2)"
rating: 1300
weight: 347
solve_time_s: 74
verified: true
draft: false
---

[CF 347A - Hàng khác biệt](https://codeforces.com/problemset/problem/347/A) 

**Đánh giá:** 1300 
**Tas:** thuật toán xây dựng, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một danh sách các số nguyên _n_ và cần sắp xếp chúng thành một hàng sao cho tổng hiệu của các số liên tiếp là lớn nhất. Cụ thể, nếu sự sắp xếp$x_1, x_2, \dots, x_n$, giá trị được tính như$(x_1 - x_2) + (x_2 - x_3) + \dots + (x_{n-1} - x_n)$. Quan sát công thức, các số hạng trung gian triệt tiêu, để lại giá trị tổng là$x_1 - x_n$. Điều này có nghĩa là giá trị lớn nhất đạt được bằng cách đặt số lớn nhất đầu tiên và số nhỏ nhất cuối cùng. Tuy nhiên, thách thức là bài toán cũng yêu cầu trình tự nhỏ nhất về mặt từ điển trong số tất cả các cách sắp xếp tạo ra giá trị tối đa này. 

Các hạn chế ở mức vừa phải:$n$lên tới 100 và các số nguyên nằm trong khoảng từ -1000 đến 1000. Điều này cho phép các giải pháp lên tới$O(n^2)$những hoán vị thoải mái nhưng ngây thơ, có quy mô như$O(n!)$, là không thực tế. Các trường hợp cạnh bao gồm các chuỗi trong đó nhiều phần tử có cùng mức tối đa hoặc tối thiểu, các chuỗi chỉ có hai phần tử và các chuỗi trong đó tất cả các số đều bằng nhau. Một cách tiếp cận ngây thơ có thể chọn bất kỳ thứ tự nào mà không đảm bảo tính tối thiểu về mặt từ điển, tạo ra giá trị tối đa chính xác nhưng chuỗi đầu ra sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tạo ra tất cả các hoán vị của mảng đầu vào, tính toán giá trị cho mỗi hoán vị và chọn giá trị có giá trị cao nhất, phá vỡ các ràng buộc bằng cách so sánh từ điển. Vì$n = 100$, cách tiếp cận này là không thể bởi vì$100!$hoán vị là lớn về mặt thiên văn. 

Thông tin chi tiết quan trọng đến từ việc quan sát việc hủy bỏ tổng chênh lệch. Tổng đơn giản hóa thành$x_1 - x_n$. Tối đa hóa điều này đòi hỏi phải đặt số lớn nhất đầu tiên và số nhỏ nhất cuối cùng. Khi vị trí đầu tiên và cuối cùng được cố định, các số còn lại không ảnh hưởng đến tổng. Để có được chuỗi nhỏ nhất về mặt từ điển, các số ở giữa phải được sắp xếp theo thứ tự tăng dần. Điều này đảm bảo chuỗi nhỏ nhất có thể cho số đầu tiên và số cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự tăng dần. Điều này cho phép dễ dàng xác định các số nhỏ nhất và lớn nhất, đồng thời đơn giản hóa việc xây dựng một chuỗi tối thiểu về mặt từ điển. 
2. Đặt số lớn nhất ở vị trí đầu tiên. Điều này tối đa hóa$x_1$trong tổng đơn giản$x_1 - x_n$. 
3. Đặt số nhỏ nhất ở vị trí cuối cùng. Điều này giảm thiểu$x_n$trong tổng số. 
4. Lấy các số còn lại (tất cả các số trừ số lớn nhất và nhỏ nhất) và sắp xếp chúng theo thứ tự tăng dần giữa vị trí đầu tiên và cuối cùng. Điều này đảm bảo trình tự tối thiểu về mặt từ điển trong khi vẫn duy trì giá trị tối đa. 
5. In chuỗi kết quả. 

Tại sao nó hoạt động: Bằng cách sửa các số lớn nhất và nhỏ nhất ở hai đầu, chúng tôi đảm bảo sự khác biệt tối đa. Thứ tự tăng dần của các số ở giữa đảm bảo chuỗi nhỏ nhất có thể về mặt từ điển. Bất kỳ thứ tự số ở giữa nào khác sẽ không làm thay đổi tổng nhưng sẽ dẫn đến một chuỗi từ điển lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()
largest = a[-1]
smallest = a[0]
middle = a[1:-1]

result = [largest] + middle + [smallest]
print(' '.join(map(str, result)))
```Mã đầu tiên sắp xếp mảng đầu vào.`largest`lấy phần tử cuối cùng,`smallest`lấy cái đầu tiên, và`middle`chứa tất cả các phần tử ở giữa. Nối chúng như`[largest] + middle + [smallest]`tạo ra đầu ra mong muốn. Việc sắp xếp ở giữa đảm bảo tính tối thiểu về mặt từ điển. Cách tiếp cận này cẩn thận tránh sửa đổi mảng ban đầu theo cách có thể phá vỡ trật tự. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`5\n100 -100 50 0 -50\n`| Bước | Mảng | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | [-100, -50, 0, 50, 100] | Đã sắp xếp | [-100, -50, 0, 50, 100] | 
| 2 | 100 | Chọn lớn nhất | lớn nhất = 100 | 
| 3 | -100 | Chọn nhỏ nhất | nhỏ nhất = -100 | 
| 4 | [-50, 0, 50] | Trung | giữa = [-50, 0, 50] | 
| 5 | Nối | [100] + [-50, 0, 50] + [-100] | [100, -50, 0, 50, -100] | 

Trình tự này tạo ra tổng`(100 - (-50)) + (-50 - 0) + (0 - 50) + (50 - (-100)) = 200`. 

**Mẫu 2** 

đầu vào:`4\n1 2 3 4\n`| Bước | Mảng | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | [1, 2, 3, 4] | Đã sắp xếp | [1, 2, 3, 4] | 
| 2 | 4 | Chọn lớn nhất | lớn nhất = 4 | 
| 3 | 1 | Chọn nhỏ nhất | nhỏ nhất = 1 | 
| 4 | [2, 3] | Trung | giữa = [2, 3] | 
| 5 | Nối | [4] + [2, 3] + [1] | [4, 2, 3, 1] | 

Tổng:`(4 - 2) + (2 - 3) + (3 - 1) = 3 + (-1) + 2 = 4`, đó là cực đại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong thời gian chạy, tất cả các hoạt động khác là O(n) | 
| Không gian | O(n) | Lưu trữ mảng và xây dựng chuỗi kết quả | 

Với$n \le 100$,$O(n \log n)$không đáng kể và phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    largest = a[-1]
    smallest = a[0]
    middle = a[1:-1]
    result = [largest] + middle + [smallest]
    return ' '.join(map(str, result))

# Provided samples
assert run("5\n100 -100 50 0 -50\n") == "100 -50 0 50 -100", "sample 1"
assert run("4\n1 2 3 4\n") == "4 2 3 1", "sample 2"

# Custom cases
assert run("2\n10 -10\n") == "10 -10", "two elements"
assert run("3\n5 5 5\n") == "5 5 5", "all equal"
assert run("5\n-1 -2 -3 -4 -5\n") == "-1 -4 -3 -2 -5", "negative numbers"
assert run("6\n0 100 -100 50 -50 25\n") == "100 -50 0 25 50 -100", "mixed numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n10 -10 | 10 -10 | Đầu vào kích thước tối thiểu | 
| 3\n5 5 5 | 5 5 5 | Tất cả các phần tử đều bằng nhau | 
| 5\n-1 -2 -3 -4 -5 | -1 -4 -3 -2 -5 | Xử lý số âm | 
| 6\n0 100 -100 50 -50 25 | 100 -50 0 25 50 -100 | Hỗn số, thứ tự từ điển | 

## Vỏ cạnh 

cho`n = 2`, mảng`[10, -10]`trực tiếp mang lại`[10, -10]`với tổng`20`. Không có yếu tố trung gian nên việc lựa chọn từ điển là chuyện nhỏ. Ví dụ: khi tất cả các số đều bằng nhau`[5, 5, 5]`, tổng tối đa bằng 0 và mọi hoán vị đều hợp lệ. Sắp xếp và làm theo thuật toán vẫn mang lại kết quả`[5, 5, 5]`, đó là mức tối thiểu về mặt từ điển. Đối với các chuỗi có số âm hoặc dấu hỗn hợp, logic tương tự được áp dụng và việc sắp xếp đảm bảo các số ở giữa duy trì tính tối thiểu về mặt từ điển trong khi vẫn giữ được tổng chênh lệch tối đa.
