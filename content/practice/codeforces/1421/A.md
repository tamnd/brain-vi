---
title: "CF 1421A - XORwice"
description: "Bài toán yêu cầu tổng tối thiểu của hai phép toán XOR cho một cặp số nguyên a và b khi XOR có cùng số x. Cụ thể, cho trước a và b, chúng ta phải chọn x sao cho biểu thức (a XOR x) + (b XOR x) càng nhỏ càng tốt."
date: "2026-06-11T06:36:15+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1421
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 676 (Div. 2)"
rating: 800
weight: 1421
solve_time_s: 616
verified: true
draft: false
---

[CF 1421A - XORwice](https://codeforces.com/problemset/problem/1421/A) 

**Đánh giá:** 800 
**Tags:** bitmasks, tham lam, toán học 
**Thời gian giải:** 10 phút 16 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu tổng tối thiểu của hai phép toán XOR cho một cặp số nguyên`a`Và`b`khi XOR với cùng một số`x`. Cụ thể, đưa ra`a`Và`b`, chúng ta phải chọn`x`sao cho biểu thức`(a XOR x) + (b XOR x)`càng nhỏ càng tốt. Đầu vào cung cấp nhiều cặp số nguyên như vậy và đầu ra là tổng tối thiểu cho mỗi cặp. 

Với những hạn chế,`1 ≤ a, b ≤ 10^9`và lên đến`10^4`các trường hợp thử nghiệm, một cuộc tìm kiếm mạnh mẽ trên tất cả các trường hợp có thể`x`giá trị là không thể, bởi vì việc lặp qua các số lên tới`10^9`đối với mỗi trường hợp thử nghiệm sẽ yêu cầu hàng nghìn tỷ thao tác. Điều này buộc chúng ta phải suy luận về chính hoạt động XOR thay vì thử mọi cách`x`. 

Một trường hợp cạnh không rõ ràng phát sinh khi`a`bằng`b`. Lựa chọn`x = a`kết quả là cả hai`a XOR x`Và`b XOR x`trở thành số không. Một cách tiếp cận bất cẩn cố gắng`x = 0`hoặc một số số tùy ý khác có thể thất bại trong trường hợp này. Một sự tinh tế khác là mỗi bit của`x`có thể giảm thiểu một cách độc lập sự đóng góp của bit đó trong`a XOR x + b XOR x`. Quản lý sai logic bitwise có thể dẫn đến kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng tất cả`x`giá trị từ`0`đến một số giới hạn trên, hãy tính`(a XOR x) + (b XOR x)`cho mỗi cái và chọn mức tối thiểu. Điều này hoạt động về nguyên tắc, nhưng độ phức tạp là`O(max(a, b))`cho mỗi trường hợp thử nghiệm, quá chậm đối với các ràng buộc trên. 

Cái nhìn sâu sắc quan trọng là xem xét hoạt động XOR ở cấp độ bit. Đối với mỗi vị trí bit, chúng ta có thể xác định một cách độc lập liệu`x`nên có một`0`hoặc`1`để giảm thiểu tổng`a XOR x + b XOR x`. Sự đóng góp của một bit đơn lẻ như sau: nếu bit đó là`0`ở cả hai`a`Và`b`, bất kì`x`bit sẽ giữ nguyên tổng. Nếu một trong số họ là`1`, đang chọn`x`để phù hợp với`1`bit làm giảm tổng số tiền cho bit đó. Việc thực hiện điều này một cách cẩn thận sẽ dẫn đến một quan sát đơn giản: chọn`x = a AND b`giảm thiểu tổng cho tất cả các bit. Với sự lựa chọn này,`(a XOR x) + (b XOR x)`trở thành`a + b - 2 * (a & b)`. Công thức này bắt nguồn từ nhận dạng`a XOR b = a + b - 2*(a & b)`. 

Điều này chuyển đổi vấn đề từ việc thử tất cả`x`để tính toán một công thức duy nhất trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a, b)) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc số nguyên`a`Và`b`. 
3. Tính toán`x = a & b`. Lựa chọn này đảm bảo số tiền tối thiểu vì nó bảo toàn tất cả các thông tin chung`1`bit và tránh thêm những cái bổ sung. 
4. Tính số tiền tối thiểu bằng công thức`(a XOR x) + (b XOR x)`. Tương đương, đây là`a + b - 2 * x`. 
5. Xuất kết quả. 

Tại sao nó hoạt động: Lựa chọn`x = a & b`đảm bảo rằng mỗi bit đó là`1`ở cả hai`a`Và`b`đóng góp`0`thành tổng, trong khi mọi bit khác nhau đều được xử lý tối ưu. Điều bất biến là đối với mỗi vị trí bit, tổng XOR được giảm thiểu một cách độc lập, do đó tổng số tiền được giảm thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    x = a & b
    result = (a ^ x) + (b ^ x)
    print(result)
```Mã đọc đầu vào hiệu quả, tính toán`x`sử dụng toán tử AND theo bit và tính toán kết quả bằng XOR. sử dụng`a & b`trực tiếp tránh các thao tác lặp lại bit và ngăn chặn các lỗi xảy ra. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| một | b | x = a & b | a^x | b^x | (a^x)+(b^x) | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 12 | 4 | 2 | 8 | 10 | 

Bảng cho thấy việc chọn`x = 4`(đó là`6 & 12`) tối thiểu hóa tổng. Bất kỳ cái nào khác`x`sẽ tạo ra một khoản tiền lớn hơn. 

Mẫu 2: 

| một | b | x = a & b | a^x | b^x | (a^x)+(b^x) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 0 | 

Điều này chứng tỏ trường hợp Edge trong đó`a = b`. Số tiền tối thiểu là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm mất thời gian không đổi, chỉ có các phép toán bitwise và số học. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào. | 

Được cho`t ≤ 10^4`, giải pháp này dễ dàng phù hợp với giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        x = a & b
        output.append(str((a ^ x) + (b ^ x)))
    return "\n".join(output)

# provided samples
assert run("6\n6 12\n4 9\n59 832\n28 14\n4925 2912\n1 1\n") == "10\n13\n891\n18\n6237\n0", "sample 1"

# custom cases
assert run("3\n1 2\n7 7\n0 15\n") == "3\n0\n15", "custom cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 3 | tổng tối thiểu cho số nguyên nhỏ | 
| 7 7 | 0 | trường hợp cạnh số giống hệt nhau | 
| 0 15 | 15 | trường hợp cạnh số 0 và số lớn | 

## Vỏ cạnh 

Khi nào`a = b`, đang chọn`x = a & b`tạo ra số 0 một cách chính xác vì`(a XOR a) + (b XOR b) = 0`. Khi`a`hoặc`b`bằng 0, công thức vẫn hoạt động vì`(0 XOR x) = x`, Và`(b XOR x)`bổ sung cho nó, mang lại`b`là số tiền tối thiểu khi`x = 0`. 

Vì`a = 0, b = 15`: 

| một | b | x = a & b | a^x | b^x | tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 15 | 0 | 0 | 15 | 15 | 

Thuật toán xử lý chính xác tất cả các khác biệt một bit, thể hiện tính chính xác của nó trên toàn bộ miền.
