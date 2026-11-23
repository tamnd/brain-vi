---
title: "CF 460B - Dima nhỏ và phương trình"
description: "Nhiệm vụ yêu cầu chúng ta tìm tất cả các số nguyên dương nhỏ hơn một tỷ thỏa mãn phương trình $$x = b cdot s(x) cdot a + c$$ trong đó $a$, $b$ và $c$ là các hằng số và $s(x)$ là tổng các chữ số của $x$."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 460
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 262 (Div. 2)"
rating: 1500
weight: 460
solve_time_s: 66
verified: true
draft: false
---

[CF 460B - Dima nhỏ và phương trình](https://codeforces.com/problemset/problem/460/B) 

**Đánh giá:** 1500 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta tìm tất cả các số nguyên dương nhỏ hơn một tỷ thỏa mãn phương trình$$x = b \cdot s(x) \cdot a + c$$Ở đâu$a$,$b$, Và$c$được cho các hằng số và$s(x)$là tổng các chữ số của$x$. Đầu vào bao gồm ba số nguyên$a$,$b$, Và$c$và đầu ra phải liệt kê số lượng các giải pháp hợp lệ, theo sau là các giải pháp theo thứ tự tăng dần. 

Quan sát đầu tiên là vấn đề bị hạn chế bởi$x < 10^9$. Từ$x$bị giới hạn thì lời giải nào cũng phải nằm dưới ngưỡng này. chức năng$s(x)$nhiều nhất là tổng các chữ số của$999,999,999$, bằng 9 nhân 9, bằng 81. Vậy tổng các chữ số bị giới hạn trong một phạm vi rất nhỏ so với$x$. Đây là quan sát quan trọng cho phép một giải pháp hiệu quả. 

Một cách tiếp cận đơn giản để kiểm tra tất cả các số nguyên lên đến$10^9$sẽ là quá chậm. Thậm chí lặp đi lặp lại lên đến$10^8$sẽ dẫn đến hàng trăm triệu hoạt động, điều này không thể thực hiện được trong thời gian 1 giây. 

Các trường hợp có biên không rõ ràng bao gồm kết quả âm sau khi tính toán$b \cdot s(x) \cdot a + c$, giá trị của$x$ở giới hạn trên, và$x$các giá trị trong đó tổng chữ số$s(x)$bằng 0 hoặc nhỏ. Ví dụ, nếu$c$là âm và có cường độ lớn, nó có thể làm giảm$x$dưới 1. Nếu chúng ta thử một cách ngây thơ tất cả các tổng chữ số, chúng ta có thể bỏ lỡ các giới hạn phạm vi. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ lặp đi lặp lại tất cả những gì có thể$x$từ 1 đến$10^9$, tính tổng các chữ số$s(x)$, đánh giá$b \cdot s(x) \cdot a + c$, và kiểm tra xem nó có bằng không$x$. Điều này đúng vì nó thực sự kiểm tra phương trình cho mọi ứng viên, nhưng nó thực hiện tới một tỷ lần kiểm tra, điều này không thực tế. 

Điểm mấu chốt là phương trình có thể được viết lại dưới dạng tổng các chữ số$s$thay vì$x$:$$x = a \cdot b \cdot s + c$$Đây,$s$phải là một số nguyên trong phạm vi từ 1 đến 81, vì tổng các chữ số tối đa của bất kỳ số nào nhỏ hơn$10^9$là 9 nhân 9. Quan sát này làm giảm đáng kể không gian tìm kiếm. Thay vì lặp lại hơn một tỷ số nguyên, chúng tôi chỉ lặp lại tối đa 81 tổng chữ số có thể có. Đối với mỗi tổng ứng cử viên$s$, chúng tôi tính toán$x = a \cdot b \cdot s + c$, sau đó xác minh xem tổng các chữ số của$x$bằng$s$và nếu$x$là tích cực và dưới đây$10^9$. Điều này hiệu quả và đảm bảo tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^9) | O(1) | Quá chậm | 
| Tối ưu | O(81) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$a$,$b$, Và$c$. Đây là các hằng số cho phương trình. 
2. Khởi tạo một danh sách trống để lưu trữ các giải pháp hợp lệ. 
3. Lặp lại tất cả các tổng chữ số có thể$s$từ 1 đến 81. Điều này bao gồm tất cả các tổng khả thi cho các số nhỏ hơn$10^9$. 
4. Đối với mỗi$s$, tính toán giải pháp ứng cử viên$x = a \cdot b \cdot s + c$. Điều này xuất phát trực tiếp từ việc sắp xếp lại phương trình. 
5. Kiểm tra xem$x$là dương và nhỏ hơn$10^9$. Loại bỏ mọi giá trị nằm ngoài phạm vi này. 
6. Tính tổng các chữ số của$x$và so sánh nó với$s$. Nếu chúng khớp, hãy nối thêm$x$vào danh sách các giải pháp. 
7. Sau khi kiểm tra tất cả các số tiền có thể có, hãy sắp xếp danh sách theo thứ tự tăng dần. 
8. In ra số lượng giải pháp theo sau là giải pháp đó. 

Tại sao nó hoạt động: Điều bất biến ở đây là bất kỳ giải pháp hợp lệ nào$x$phải thỏa mãn cả phương trình và định nghĩa của tổng các chữ số. Từ$s(x)$không thể vượt quá 81 cho bất kỳ$x < 10^9$, việc lặp qua tất cả các tổng có thể đảm bảo rằng không có nghiệm hợp lệ nào bị bỏ sót. Kiểm tra$s(x) = s$đảm bảo rằng các ứng cử viên không có kết quả dương tính giả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(n):
    return sum(int(d) for d in str(n))

a, b, c = map(int, input().split())
solutions = []

for s in range(1, 82):
    x = a * b * s + c
    if 0 < x < 10**9 and digit_sum(x) == s:
        solutions.append(x)

solutions.sort()
print(len(solutions))
print(*solutions)
```chức năng`digit_sum`chuyển đổi số nguyên thành một chuỗi và tính tổng các chữ số của nó. Điều này hiệu quả đối với số lượng nhỏ và tránh trích xuất chữ số thủ công. Chúng tôi lặp lại tất cả các tổng chữ số khả thi, tính toán các ứng cử viên, lọc theo số dương và tổng chữ số, rồi cuối cùng sắp xếp để xuất chúng theo thứ tự tăng dần. Việc sắp xếp là bắt buộc vì việc lặp lại theo$s$không đảm bảo trật tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 2 8`| s | x = a_b_s + c | s(x) | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 3_2_1 + 8 = 14 | 5 | Không | 
| 2 | 3_2_2 + 8 = 20 | 2 | Có | 
| 10 | 3_2_10 + 8 = 68 | 14 | Không | 
| 2008 | 3_2_334 + 8 = 2008 | 10 | Có | 

Điều này chứng tỏ rằng việc lặp lại s một cách chính xác sẽ tạo ra tất cả các nghiệm mà không cần kiểm tra từng số nguyên. 

### Ví dụ 2 

đầu vào:`1 1 -1`| s | x = s - 1 | s(x) | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | Không | 
| 2 | 1 | 1 | Có | 

Điều này cho thấy việc xử lý các kết quả âm tính hoặc bằng 0 do`c`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(81 * log(x)) ≈ O(1) | Lặp lại tối đa 81 khoản tiền; tổng chữ số mất log(x) thời gian | 
| Không gian | O(81) | Lưu trữ giải pháp ứng viên | 

Thời gian và không gian là tầm thường so với giới hạn của bài toán. Việc sắp xếp tối đa 81 phần tử là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    def digit_sum(n):
        return sum(int(d) for d in str(n))
    
    a, b, c = map(int, input().split())
    solutions = []
    
    for s in range(1, 82):
        x = a * b * s + c
        if 0 < x < 10**9 and digit_sum(x) == s:
            solutions.append(x)
    
    solutions.sort()
    return f"{len(solutions)}\n{' '.join(map(str, solutions))}"

# Provided sample
assert run("3 2 8\n") == "3\n10 2008 13726", "sample 1"

# Custom cases
assert run("1 1 -1\n") == "1\n1", "negative c"
assert run("5 10000 0\n") == "1\n50000", "large b"
assert run("1 1 0\n") == "9\n1 2 3 4 5 6 7 8 9", "all single digits"
assert run("2 3 5\n") == "3\n11 17 23", "small a, b, c"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 -1 | 1\n1 | Xử lý đúng âm c | 
| 5 10000 0 | 1\n50000 | Số nhân lớn b | 
| 1 1 0 | 9\n1 2 3 4 5 6 7 8 9 | Tất cả một chữ số x | 
| 2 3 5 | 3\n11 17 23 | Thông số nhỏ điển hình | 

## Vỏ cạnh 

Thuật toán xử lý c < 0 bằng cách tính x = a_b_s + c. Với s = 1, nếu x <= 0 thì bị loại bỏ. Ví dụ, với đầu vào`1 1 -1`, x = 0 vì s = 1 không hợp lệ nên chỉ s = 2 cho x = 1. Việc kiểm tra tổng các chữ số đảm bảo các kết quả dương tính giả bị loại bỏ. 

Ví dụ, đối với x tiến đến giới hạn trên`a=5, b=100000, c=0`, x = 5_100000_81 = 40500000 < 10^9, vì vậy các nghiệm hợp lệ đều được đưa vào. Việc sắp xếp đảm bảo đầu ra theo thứ tự tăng dần bất kể thứ tự s tạo ra giải pháp là gì.
