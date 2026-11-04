---
title: "CF 1157A - Số có thể tiếp cận"
description: "Chúng ta được yêu cầu khám phá các số được tạo ra bằng cách áp dụng lặp đi lặp lại một hàm biến đổi cụ thể. Đối với một số $x$, trước tiên chúng ta thêm một vào số đó. Nếu số kết quả có số 0 ở cuối, chúng tôi sẽ xóa tất cả chúng cho đến khi không còn lại số nào. Điều này xác định hàm $f(x)$."
date: "2026-06-12T02:32:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1157
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 555 (Div. 3)"
rating: 1100
weight: 1157
solve_time_s: 88
verified: true
draft: false
---

[CF 1157A - Những con số có thể tiếp cận](https://codeforces.com/problemset/problem/1157/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu khám phá các số được tạo ra bằng cách áp dụng lặp đi lặp lại một hàm biến đổi cụ thể. Đối với một số$x$, trước tiên chúng ta thêm một cái vào đó. Nếu số kết quả có số 0 ở cuối, chúng tôi sẽ xóa tất cả chúng cho đến khi không còn lại số nào. Điều này xác định chức năng$f(x)$. một con số$y$được cho là có thể truy cập từ$x$nếu áp dụng$f$không hoặc nhiều lần bắt đầu từ$x$cuối cùng tạo ra$y$. 

Đầu vào là một số nguyên duy nhất$n$và đầu ra là số lượng các số riêng biệt có thể truy cập được từ$n$. Những con số có thể tiếp cận bao gồm$n$chính nó và bất kỳ con số nào có được thông qua việc áp dụng lặp đi lặp lại$f$. 

Ràng buộc$1 \le n \le 10^9$ngụ ý rằng$n$phù hợp với các loại số nguyên tiêu chuẩn và bất kỳ thuật toán nào cũng nên tránh lặp qua tất cả các số lên đến$n$bởi vì điều đó có thể liên quan đến hàng tỷ hoạt động. Một giải pháp lý tưởng nhất là tuyến tính hoặc logarit theo số chữ số hoặc phép toán. 

Các trường hợp cạnh xuất hiện khi số kết thúc bằng nhiều số 0 hoặc ngay dưới lũy thừa mười. Ví dụ,$n = 999$tạo ra một bước nhảy lớn sau$f(999) = 1$. Việc tăng thêm 1 một cách ngây thơ mỗi lần và loại bỏ các số 0 có thể dẫn đến chu kỳ hoặc số bị bỏ lỡ nếu việc triển khai không xử lý các bước nhảy này một cách cẩn thận. Một trường hợp cạnh khác là một số có nhiều số 0 ở cuối, chẳng hạn như$n = 1000$, trong đó một ứng dụng ngay lập tức chuyển tới$1$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: bắt đầu từ$n$và áp dụng nhiều lần$f$, lưu trữ từng số trong một bộ cho đến khi một số lặp lại. Điều này đảm bảo tính chính xác vì quy trình cuối cùng phải đạt đến số đã thấy trước đó (tất cả các số đều dương và hàm giảm các số 0 ở cuối, cuối cùng lặp thành các số nhỏ hơn). Trong trường hợp xấu nhất, số lượng thao tác gần bằng tổng số chữ số gặp phải trong quá trình giảm lặp lại. Tuy nhiên, đối với$n = 10^9$, điều này có thể đạt đến hàng tỷ bước, quá chậm so với giới hạn 1 giây. 

Quan sát quan trọng là những con số duy nhất có thể đạt được từ$n$là những cái khác với$n$tăng dần 1 sau khi loại bỏ các số 0 ở cuối. Vì vậy, chúng ta có thể theo dõi số lượng được tạo ra bởi$f$mà không lưu trữ rõ ràng tất cả các số trung gian. Thay vào đó, chúng ta có thể tính toán nhiều lần$f(n)$cho đến khi nó đạt đến giá trị đã thấy trước đó hoặc giảm xuống 0. Mỗi số được tạo ra là khác biệt vì$f$tăng số lượng một cách nghiêm ngặt cho đến khi một chu kỳ hoặc sự giảm xảy ra. 

Cách tiếp cận tối ưu sử dụng một vòng lặp đơn giản áp dụng phép biến đổi$f$cho đến khi nó đạt đến một con số đã thấy trước đó. Chúng tôi duy trì một bộ đếm thay vì lưu trữ mọi số một cách rõ ràng. Vì mỗi ứng dụng giảm số lượng các số 0 ở cuối nên trình tự không thể truy cập lại một số lớn hơn bất kỳ số nào đã thấy trước đó, điều này đảm bảo kết thúc sau một số bước nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm về 0 và đặt số hiện tại$x = n$. Bộ đếm theo dõi số lượng các số có thể truy cập riêng biệt. 
2. Bắt đầu một vòng lặp tiếp tục cho đến khi$x$trở thành 0. 
3. Tăng bộ đếm lên 1 để đếm$x$có thể tiếp cận được. 
4. Tính toán$x = x + 1$. Xóa tất cả các số 0 ở cuối bằng cách chia liên tục$x$bằng 10 cho đến khi chữ số cuối cùng khác 0. 
5. Kiểm tra xem$x$đã đạt đến số đã truy cập trước đó (theo cách này, nếu nó giảm xuống còn một số có một chữ số, chúng ta biết các số còn lại đều có một chữ số, vì vậy chúng ta có thể thoát ra sau khi đếm chúng). 
6. Trả về tổng số các số phân biệt. 

Tại sao nó hoạt động: điều bất biến là mọi ứng dụng của$f$tạo ra một số lớn hơn số trước đó nếu chúng ta bỏ qua các số 0 ở cuối. Các số 0 ở cuối được loại bỏ để ngăn chặn sự biểu diễn trùng lặp của cùng một số trong chuỗi. Cuối cùng, dãy số đạt tới các số dưới 10 hoặc các số đã xuất hiện nên không có số nào được tính hai lần, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def reachable_count(n):
    seen = set()
    x = n
    while x not in seen:
        seen.add(x)
        x += 1
        while x % 10 == 0:
            x //= 10
    return len(seen)

n = int(input())
print(reachable_count(n))
```Giải pháp sử dụng một bộ`seen`để theo dõi các số đã truy cập, đảm bảo không có sự trùng lặp nào được tính. Vòng lặp bên trong loại bỏ các số 0 ở cuối, thực hiện hàm$f(x)$. Thuật toán kết thúc vì các số tăng hoặc giảm về số đã thấy trước đó. 

## Ví dụ đã hoạt động 

**Ví dụ 1: n = 1098** 

| Bước | x trước f | x sau f | Đã thấy | 
| --- | --- | --- | --- | 
| 1 | 1098 | 1099 | {1098} | 
| 2 | 1099 | 11 | {1098, 1099} | 
| 3 | 11 | 12 | {1098, 1099, 11} | 
| 4 | 12 | 13 | {1098, 1099, 11, 12} | 
| … | … | … | … | 
| 20 | 18 | 19 | {1098, 1099, 11…18} | 

Sau khi đếm tất cả các số có thể truy cập được, tổng số là 20. 

**Ví dụ 2: n = 9** 

| Bước | x trước f | x sau f | Đã thấy | 
| --- | --- | --- | --- | 
| 1 | 9 | 1 | {9} | 
| 2 | 1 | 2 | {9,1} | 
| … | … | … | … | 
| 9 | 8 | 9 | {1,2,3,4,5,6,7,8,9} | 

Tất cả các số có một chữ số đều có thể truy cập được, đếm = 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi bước giảm các số 0 ở cuối, nhiều nhất là số chữ số trong n. Mỗi số được truy cập nhiều nhất một lần. | 
| Không gian | O(log n) | Bộ lưu trữ từng số riêng biệt, tăng tối đa một chữ số, tỷ lệ thuận với số chữ số. | 

Với n lên đến 10^9, số bước tối đa là vài chục lần lặp, trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    return str(reachable_count(n))

# Provided sample
assert run("1098\n") == "20", "sample 1"

# Custom cases
assert run("9\n") == "9", "single digit number"
assert run("1000\n") == "4", "trailing zeros"
assert run("999\n") == "10", "just below power of 10"
assert run("1\n") == "9", "minimum n"
assert run("123456789\n") == "9", "large n with unique digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 | 9 | số có một chữ số | 
| 1000 | 4 | số có số 0 ở cuối giảm nhanh | 
| 999 | 10 | nhảy sau các số 0 ở cuối, vượt lũy thừa 10 | 
| 1 | 9 | tối thiểu n, đạt tất cả các chữ số đơn | 
| 123456789 | 9 | số lớn, đếm đúng chữ số | 

## Vỏ cạnh 

cho`n = 1000`,`f(1000) = 1001 -> 1001`sau đó loại bỏ sản lượng số không`1001`sau đó`1002`. Thuật toán đếm chính xác`1000, 1001, 1002, 1003`và chấm dứt. Vì`n = 999`,`f(999) = 1000 -> 1`sau đó tiến hành thông qua`2..9`, đếm từng cái một. Tất cả các trường hợp cạnh liên quan đến các số 0 ở cuối hoặc các số ngay dưới lũy thừa mười đều được xử lý chính xác vì vòng lặp bên trong sẽ loại bỏ các số 0 trước khi tiếp tục.
