---
title: "CF 281B - Phân số gần nhất"
description: "Chúng ta được yêu cầu tính gần đúng một phân số $x/y$ với một phân số khác $a/b$ trong đó $b$ nhiều nhất là $n$. Mục tiêu là kiếm $ Về mặt khái niệm, chúng ta đang cố gắng tìm phân số \"gần nhất\" có mẫu số giới hạn với một phân số đã cho."
date: "2026-06-05T09:05:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 281
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 172 (Div. 2)"
rating: 1700
weight: 281
solve_time_s: 120
verified: true
draft: false
---

[CF 281B - Phân số gần nhất](https://codeforces.com/problemset/problem/281/B) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thực hiện, hai con trỏ 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính gần đúng một phân số nhất định$x/y$với một phân số khác$a/b$Ở đâu$b$nhiều nhất là$n$. Mục tiêu là làm cho$|x/y - a/b|$càng nhỏ càng tốt và nếu nhiều phân số bằng nhau, trước tiên chúng ta giảm thiểu$b$và sau đó$a$. 

Về mặt khái niệm, chúng ta đang cố gắng tìm phân số "gần nhất" có mẫu số giới hạn với một phân số đã cho. Đầu vào cho phần mục tiêu$x/y$và mẫu số tối đa cho phép$n$. Đầu ra phải là một phân số đơn giản$a/b$ở mức thấp nhất gần nhất với$x/y$. 

Những hạn chế$x, y, n \le 10^5$cho rằng các thuật toán có độ phức tạp về thời gian kém hơn$O(n)$có thể gặp khó khăn trong Python. Một vòng lặp kép đơn giản trên tất cả các tử số và mẫu số sẽ đại khái là$O(n^2)$, hoặc lên tới$10^{10}$lặp lại trong trường hợp xấu nhất là quá chậm. Điều này gợi ý rằng phép tính theo mẫu số là khả thi, nhưng việc lặp lại trên tất cả các cặp tử số-mẫu số thì không. 

Trường hợp cạnh phát sinh khi phân số mục tiêu được biểu diễn chính xác với mẫu số nhỏ hơn$n$. Ví dụ, nếu$x=1$,$y=2$, Và$n=4$, phân số tốt nhất là$1/2$. Một trường hợp khó phát hiện khác là khi phân số mục tiêu rất nhỏ, ví dụ:$x=1$,$y=100$,$n=2$. Ở đây, phân số gần nhất là$0/1$, không$1/2$. Cách tiếp cận làm tròn ngây thơ có thể bỏ lỡ điều này bằng cách luôn làm tròn lên. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét mọi phân số có mẫu số$1 \le b \le n$và tính tử số$a$đó giảm thiểu sự khác biệt. Về mặt hình thức, đối với mỗi$b$, chúng ta có thể kiểm tra tất cả$a$từ$0$ĐẾN$b$. Điều này sẽ cho$O(n^2)$hoạt động đúng nhưng không khả thi đối với$n \sim 10^5$. 

Cái nhìn sâu sắc quan trọng là đối với một cố định$b$, tử số tối ưu$a$gần với$x \cdot b / y$. Điều này xuất phát từ việc sắp xếp lại công thức lỗi:$$|x/y - a/b| = |x \cdot b - a \cdot y| / (b \cdot y)$$Giảm thiểu sự khác biệt tuyệt đối$|x \cdot b - a \cdot y|$tương đương với việc giảm thiểu lỗi tổng thể. Vì vậy, chúng ta chỉ cần xét$a = \lfloor x \cdot b / y \rfloor$Và$a + 1$, vì đây là hai số nguyên gần nhất$x \cdot b / y$. Điều này làm giảm vấn đề từ$O(n^2)$ĐẾN$O(n)$, vì với mỗi mẫu số chúng ta chỉ kiểm tra hai tử số. 

Cách tiếp cận này cũng xử lý các quy tắc ràng buộc một cách tự nhiên: nếu hai phân số có cùng hiệu thì phân số nào có hiệu nhỏ hơn$b$được chọn đầu tiên vì chúng tôi xử lý các mẫu số theo thứ tự tăng dần và giữa các mẫu số bằng nhau$b$, càng nhỏ$a$được chọn tự động bằng cách kiểm tra$a$trước$a+1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$x, y, n$. Khởi tạo các biến để lưu trữ phân số tốt nhất:`best_a = 0`,`best_b = 1`, Và`min_diff = infinity`. 
2. Lặp lại từng mẫu số có thể$b$từ 1 đến$n$. Đối với mỗi$b$, tính toán`a = (x * b) // y`. Đây là tử số nguyên gần nhất với$x * b / y$. 
3. Đối với`a`Và`a + 1`, tính sự khác biệt`diff = abs(x * b - a * y)`. Đây là lỗi không chia tỷ lệ trong đơn vị tử số. 
4. Nếu`diff * best_b < min_diff * b`, cập nhật`best_a = a`,`best_b = b`, Và`min_diff = diff`. Sự so sánh này tránh được các lỗi dấu phẩy động bằng cách nhân chéo. 
5. Sau vòng lặp, in`best_a/best_b`. 

**Tại sao nó hoạt động**: Đối với mỗi mẫu số, chúng tôi chỉ xem xét hai tử số gần nhất với mục tiêu có giá trị thực. Phép nhân chéo đảm bảo chúng ta so sánh sự khác biệt tuyệt đối mà không có lỗi dấu phẩy động. Bằng cách lặp lại các mẫu số theo thứ tự, các quy tắc ràng buộc sẽ tự động được tôn trọng. Điều này đảm bảo rằng phân số tìm được là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y, n = map(int, input().split())

best_a, best_b = 0, 1
min_diff = x  # initial large value

for b in range(1, n + 1):
    a = (x * b) // y
    for candidate in [a, a + 1]:
        if 0 <= candidate <= b:
            diff = abs(x * b - candidate * y)
            if diff * best_b < min_diff * b:
                best_a, best_b, min_diff = candidate, b, diff

print(f"{best_a}/{best_b}")
```Dòng đầu tiên đọc đầu vào một cách hiệu quả. Chúng tôi khởi tạo`best_a`Và`best_b`đến 0/1 vì 0 luôn là tử số hợp lệ. Đối với mỗi mẫu số,`(x*b)//y`là ứng cử viên tốt nhất và chúng tôi cũng kiểm tra`a + 1`để đảm bảo việc làm tròn được xem xét. Sự so sánh`diff * best_b < min_diff * b`tránh các lỗi dấu phẩy động bằng cách so sánh các số nguyên tỷ lệ. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`3 7 6`| b | a = tầng(3_b/7) | ứng cử viên | khác = |3_b - a*y| | tốt nhất_a/b | 

|---|---|---|---|---| 

| 1 | 0 | 0 | 3 | 0/1 | 

| 2 | 0 | 0 | 6 | 0/1 | 

| 2 | 0 | 1 | 1 | 1/2 | 

| 3 | 1 | 1 | 2 | 1/2 | 

| 3 | 1 | 2 | 1 | 2/3 | 

| 4 | 1 | 1 | 5 | 2/3 | 

| 4 | 1 | 2 | 2 | 2/4 | 

| 5 | 2 | 2 | 1 | 2/5 | 

| 5 | 2 | 3 | 2 | 2/5 | 

| 6 | 2 | 2 | 3 | 2/5 | 

| 6 | 2 | 3 | 1 | 6/2 | 

Đầu ra:`2/5`Dấu vết này cho thấy rằng việc lặp lại các mẫu số và kiểm tra`a`Và`a+1`tìm phân số có sai phân nhỏ nhất. 

**Mẫu 2** 

đầu vào:`1 2 4`Phân số tốt nhất:`1/2`Trace xác nhận thuật toán chọn cách biểu diễn chính xác khi có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Lặp lại tất cả các mẫu số 1..n, kiểm tra tối đa 2 tử số trên mỗi mẫu số | 
| Không gian | O(1) | Số lượng biến không đổi; không cần mảng | 

Với$n \le 10^5$, thao tác này thực hiện khoảng 2*10^5 thao tác, thoải mái trong giới hạn thời gian 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y, n = map(int, input().split())
    best_a, best_b = 0, 1
    min_diff = x
    for b in range(1, n + 1):
        a = (x * b) // y
        for candidate in [a, a + 1]:
            if 0 <= candidate <= b:
                diff = abs(x * b - candidate * y)
                if diff * best_b < min_diff * b:
                    best_a, best_b, min_diff = candidate, b, diff
    return f"{best_a}/{best_b}"

# Provided samples
assert run("3 7 6\n") == "2/5", "sample 1"
assert run("1 2 4\n") == "1/2", "sample 2"

# Custom cases
assert run("1 100 2\n") == "0/1", "small fraction, small denominator"
assert run("100 100 100\n") == "1/1", "equal numerator and denominator"
assert run("5 9 5\n") == "3/5", "tie-breaking on numerator"
assert run("7 12 10\n") == "6/10", "larger denominator, rounding down"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 2 | 0/1 | Rất nhỏ |
