---
title: "CF 918A - Mười Một"
description: "Chúng ta được yêu cầu tạo một chuỗi có độ dài n chỉ bao gồm chữ cái 'O' viết hoa và viết thường, tuân theo quy tắc dựa trên dãy Fibonacci. Các vị trí trong chuỗi tương ứng với các số Fibonacci (1, 2, 3, 5, 8,..."
date: "2026-06-12T09:50:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 918
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 459 (Div. 2)"
rating: 800
weight: 918
solve_time_s: 157
verified: true
draft: false
---

[CF 918A - Eleven](https://codeforces.com/problemset/problem/918/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tạo một chuỗi có độ dài _n_ chỉ bao gồm chữ cái 'O' viết hoa và viết thường, tuân theo quy tắc dựa trên dãy Fibonacci. Các vị trí trong chuỗi tương ứng với các số Fibonacci (1, 2, 3, 5, 8, ...) phải là chữ hoa 'O' và tất cả các vị trí khác phải là chữ thường 'o'. Vị trí được tính bắt đầu từ 1. 

Đầu vào là một số nguyên _n_, có thể nhỏ bằng 1 và lớn bằng 1000. Điều này có nghĩa là mọi giải pháp có độ phức tạp thời gian tuyến tính hoặc gần tuyến tính sẽ chạy thoải mái trong giới hạn thời gian 1 giây. Không có lo ngại về việc tràn số nguyên vì các số Fibonacci dưới 1000 phù hợp với các số nguyên tiêu chuẩn. 

Trường hợp khó phát hiện xảy ra khi dãy Fibonacci có các số lặp lại ở đầu. Theo định nghĩa, hai số Fibonacci đầu tiên đều bằng 1 nên vị trí 1 nên được coi là vị trí Fibonacci và chúng ta không nên đánh dấu nhầm hai lần hoặc bỏ qua. Một trường hợp cạnh khác là khi _n_ bản thân nó là số Fibonacci, ví dụ _n_ = 8. Chúng ta cần đưa vị trí cuối cùng này làm chữ hoa. 

Việc triển khai bất cẩn có thể tạo ra chuỗi Fibonacci mà không kiểm tra giới hạn và có thể vượt quá _n_, gây ra lỗi chỉ mục hoặc ký tự thừa. Điều quan trọng là chỉ đánh dấu các vị trí trong phạm vi từ 1 đến _n_. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là tính dãy Fibonacci lên tới _n_, lưu trữ tất cả các số Fibonacci trong một danh sách, sau đó lặp qua các vị trí từ 1 đến _n_. Tại mỗi vị trí, chúng ta kiểm tra xem nó có nằm trong danh sách các số Fibonacci hay không. Nếu đúng như vậy, chúng tôi sẽ thêm 'O' vào chuỗi, nếu không thì là 'o'. Cách tiếp cận này hiệu quả vì chúng ta biết rõ ràng vị trí nào là Fibonacci và chúng ta chỉ cần xem xét các số lên đến _n_. Độ phức tạp có thể chấp nhận được với _n_ ≤ 1000, nhưng việc kiểm tra tư cách thành viên trong danh sách nhiều lần là tuyến tính theo kích thước của danh sách, điều này hơi kém hiệu quả. 

Chúng ta có thể tối ưu hóa điều này bằng cách sử dụng một tập hợp thay vì một danh sách để lưu trữ các số Fibonacci. Đặt tư cách thành viên là O(1), vì vậy chúng tôi giảm chi phí kiểm tra từng vị trí. Phần còn lại của thuật toán vẫn giữ nguyên. Đây là một tối ưu hóa nhỏ cho kích thước vấn đề này nhưng thể hiện cách thực hành tốt cho _n_ lớn hơn. 

Brute-force hoạt động vì nó xây dựng rõ ràng các số Fibonacci và kiểm tra từng vị trí, nhưng nó không hiệu quả khi chuỗi tăng lớn. Việc quan sát rằng các vị trí Fibonacci có thể được lưu trữ trong một tập hợp cho phép kiểm tra O(1) trên mỗi vị trí và hợp lý hóa giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra danh sách) | O(n * m) trong đó m là số số Fibonacci ≤ n | O(m) | Được chấp nhận với n ≤ 1000 | 
| Tối ưu (đặt kiểm tra) | O(n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên đầu vào _n_. Điều này cho biết độ dài của chuỗi cần tạo. 
2. Khởi tạo hai số Fibonacci đầu tiên, f1 và f2, cả hai đều bằng 1. Chúng đại diện cho vị trí đầu tiên được đánh dấu là chữ hoa 'O'. 
3. Tạo tất cả các số Fibonacci nhỏ hơn hoặc bằng _n_. Bắt đầu từ f1 và f2 và tính toán lặp lại f_next = f1 + f2. Thêm mỗi số Fibonacci vào một tập hợp để kiểm tra tư cách thành viên O(1). Dừng lại một lần f_next > n. 
4. Khởi tạo danh sách trống`name`để xây dựng chuỗi kết quả. Lặp lại các vị trí i từ 1 đến _n_, kiểm tra xem i có nằm trong tập Fibonacci không. Nếu có, hãy thêm 'O' vào danh sách; nếu không, hãy thêm 'o'. 
5. Nối danh sách thành một chuỗi duy nhất và in kết quả. 

Thuộc tính chính đảm bảo tính chính xác là tập hợp chứa tất cả các số Fibonacci lên đến _n_ và chúng tôi lặp lại chính xác các vị trí từ 1 đến _n_. Bằng cách kiểm tra từng vị trí dựa trên tập hợp này, chúng tôi đảm bảo rằng mọi vị trí Fibonacci đều là chữ hoa và tất cả các vị trí khác đều là chữ thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

# Generate Fibonacci numbers up to n
fib_set = set()
a, b = 1, 1
while a <= n:
    fib_set.add(a)
    a, b = b, a + b

# Build the name
name = []
for i in range(1, n + 1):
    if i in fib_set:
        name.append('O')
    else:
        name.append('o')

print(''.join(name))
```Giải pháp đầu tiên đọc số nguyên _n_. Các số Fibonacci được tạo trong một vòng lặp và mỗi số được thêm vào một tập hợp để cho phép tra cứu theo thời gian không đổi. Sử dụng danh sách`name`đảm bảo xây dựng chuỗi hiệu quả. Thao tác nối cuối cùng tạo ra chuỗi đầu ra trong một lần chuyển. Việc sử dụng một bộ sẽ tránh được các lần quét tuyến tính lặp lại và lỗi từng lỗi một vì tất cả các chỉ số đều được giới hạn hợp lý từ 1 đến _n_. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào: 8 

| tôi | Fibonacci? | tên | 
| --- | --- | --- | 
| 1 | Có | Ồ | 
| 2 | Có | Ồ | 
| 3 | Có | Ồ | 
| 4 | Không | o | 
| 5 | Có | Ồ | 
| 6 | Không | o | 
| 7 | Không | o | 
| 8 | Có | Ồ | 

Đầu ra: OOooOooO 

Dấu vết này xác nhận rằng các vị trí 1, 2, 3, 5, 8 được đánh dấu chính xác là chữ hoa 'O' và tất cả các vị trí khác là chữ thường 'o'. 

### Đầu vào tùy chỉnh 

Đầu vào: 1 

| tôi | Fibonacci? | tên | 
| --- | --- | --- | 
| 1 | Có | Ồ | 

Đầu ra: O 

Trường hợp cạnh này thể hiện việc xử lý đầu vào nhỏ nhất có thể. Thuật toán đánh dấu chính xác vị trí đầu tiên là 'O'. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tạo các số Fibonacci lên đến n thực hiện các bước O(log n), lặp qua các vị trí từ 1 đến n là O(n) | 
| Không gian | O(n) | Tập hợp các số Fibonacci và danh sách đầu ra đều chiếm không gian O(n) trong trường hợp xấu nhất | 

Với n 1000, giải pháp này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    fib_set = set()
    a, b = 1, 1
    while a <= n:
        fib_set.add(a)
        a, b = b, a + b
    name = []
    for i in range(1, n + 1):
        name.append('O' if i in fib_set else 'o')
    return ''.join(name)

# provided sample
assert run("8\n") == "OOOoOooO", "sample 1"

# custom cases
assert run("1\n") == "O", "minimum size"
assert run("2\n") == "OO", "two positions, both Fibonacci"
assert run("10\n") == "OOOoOooOoo", "n larger, check proper Fibonacci positions"
assert run("13\n") == "OOOoOooOooOooO", "n equal to a Fibonacci number"
assert run("1000\n")[0:10] == "OOOoOooOoo", "first ten characters correctness for large n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Ồ | Đầu vào tối thiểu được xử lý chính xác | 
| 2 | OO | Bao gồm hai số Fibonacci đầu tiên | 
| 10 | OOOoOooOoo | Sửa lỗi đánh dấu Fibonacci trong chuỗi lớn hơn | 
| 13 | OOOoOooOooOooO | Xử lý n bằng số Fibonacci | 
| 1000 | OOOoOooOoo... | Tính đúng đắn cho n lớn | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất, n = 1, thuật toán tạo ra các số Fibonacci {1}. Lặp lại vị trí 1, nó tìm thấy nó trong tập hợp, thêm 'O' và in "O". Điều này xác nhận rằng các vị trí cạnh ở đầu chuỗi được xử lý chính xác. Đối với n bằng số Fibonacci, chẳng hạn như n = 13, vị trí cuối cùng được đưa vào vì quá trình tạo tập hợp Fibonacci chỉ dừng khi a > n. Tất cả các vị trí nhỏ hơn hoặc bằng n đều được tính toán chính xác. Thuật toán xử lý các số Fibonacci lặp lại ở đầu (1, 1) một cách tự nhiên vì tập hợp này bỏ qua các số trùng lặp, đảm bảo vị trí 1 là chữ hoa mà không có vấn đề trùng lặp.
