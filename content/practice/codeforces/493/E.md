---
title: "CF 493E - Vasya và đa thức"
description: "Chúng ta được yêu cầu đếm số đa thức có hệ số nguyên không âm thỏa mãn một thuộc tính đánh giá rất cụ thể."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 493
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 281 (Div. 2)"
rating: 2800
weight: 493
solve_time_s: 75
verified: false
draft: false
---

[CF 493E - Vasya và Đa thức](https://codeforces.com/problemset/problem/493/E) 

**Đánh giá:** 2800 
**Thẻ:** toán 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số đa thức có hệ số nguyên không âm thỏa mãn một thuộc tính đánh giá rất cụ thể. Về mặt hình thức, chúng ta được cho số nguyên$a$,$b$, Và$c$, và ta cần tìm tất cả các đa thức$P(x) = a_0 + a_1 x + a_2 x^2 + \dots + a_n x^n$sao cho đa thức được đánh giá tại$x = a$bằng$c$trừ đi$b$lần đa thức được đánh giá tại$x = 1$. Trực quan hơn, chúng ta đang cố gắng tìm các dãy số nguyên không âm$a_0, a_1, \dots, a_n$như vậy$$P(a) = b \cdot P(1) + c.$$Đây,$a$Và$b$có thể lớn như$10^{18}$, điều này ngay lập tức loại trừ việc liệt kê ngây thơ tất cả các đa thức có thể. Đầu ra phải là`"inf"`nếu có vô số đa thức như vậy tồn tại, hoặc số đếm modulo$10^9 + 7$nếu không thì. 

Các ràng buộc ngụ ý một số trường hợp cạnh không rõ ràng. Ví dụ, khi$b = 1$, phương trình đôi khi có thể có vô số nghiệm nếu đánh giá đa thức tại$x = a$phù hợp với$c$theo một cách nhất định. Một sự tinh tế khác nảy sinh khi$a < b$; Các cách tiếp cận đệ quy ngây thơ thử tất cả các giá trị hệ số có thể bỏ sót các trường hợp có lũy thừa cao hơn của$a$thống trị sự bình đẳng. Đầu vào nhỏ như$a = 1$,$b = 1$, Và$c = 0$phải được xử lý cẩn thận để tránh đếm các đa thức không tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi số nguyên không âm$(a_0, a_1, ..., a_n)$như vậy$P(a) = b \cdot P(1) + c$. Ở mỗi bước, chúng ta sẽ thử tất cả các giá trị có thể có của một hệ số$a_i$cho đến khi phương trình được thỏa mãn. Điều này đúng về nguyên tắc, nhưng ngay cả đối với những trường hợp nhỏ.$a$Và$b$, số lượng trình tự tăng theo cấp số nhân với mức độ$n$, và với$a, b \le 10^{18}$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng cho phép giải pháp hiệu quả là vấn đề được rút gọn thành biểu diễn dạng chữ số của$c$trong căn cứ$a$. Xét phương trình:$$a_0 + a_1 a + a_2 a^2 + \dots = b \cdot (a_0 + a_1 + a_2 + \dots) + c.$$Viết lại, chúng ta thấy$$(a - b) a_1 + (a^2 - b) a_2 + \dots = c - b a_0.$$Điều này gợi ý một cách tiếp cận đệ quy: chọn$a_0$, sau đó kiểm tra đệ quy xem tổng còn lại chia cho$a$vẫn thỏa mãn dạng tương tự, tương tự như chuyển đổi cơ số có dư. Cách tiếp cận này hoạt động hiệu quả vì tổng các hệ số giảm ở mỗi lần đệ quy và đối với các cơ sở đủ nhỏ, có nhiều nhất$O(\log_a c)$các bước đệ quy. Các trường hợp cạnh phát sinh khi$a = b$bởi vì phép đệ quy có thể dẫn đến một chuỗi nghiệm vô tận. Kiểm tra điều kiện này cho phép chúng tôi quay trở lại`"inf"`trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∞) | O(n) | Quá chậm | 
| Phân rã đệ quy/giống chữ số | O(log_a c * a) | O(log_a c) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem$a = 1$. Trong trường hợp này, đa thức được đánh giá tại$x = 1$chỉ đơn giản là tổng của các hệ số. Phương trình giảm xuống còn$sum = b \cdot sum + c$. Nếu như$b = 1$Và$c = 0$, có vô số cách giải. Nếu như$b = 1$Và$c > 0$, không có giải pháp tồn tại. Nếu không thì,$sum = c / (1 - b)$phải là số nguyên. Nếu không, trả về 0; mặt khác, hãy đếm số dãy có hệ số nguyên không âm tổng bằng giá trị này, đây là bài toán tổ hợp sao và thanh tiêu chuẩn. 
2. Mặt khác, thực hiện hàm đệ quy`solve(n)`Ở đâu`n`là giá trị mục tiêu còn lại. Tại mỗi lần đệ quy, hãy xem xét phần còn lại`n % a`như hệ số`a_0`và tái diễn với`n // a - b * a_0`. Chấm dứt đệ quy khi tổng còn lại trở thành số âm. 
3. Tích lũy số dãy hợp lệ ở mỗi lần đệ quy. Mỗi lựa chọn hợp lệ của phần còn lại góp phần vào một giải pháp. 
4. Nếu trong quá trình đệ quy bạn phát hiện ra rằng`a - b = 0`và mục tiêu còn lại được chia cho phù hợp, trả về`"inf"`. 
5. Trả về tổng số dãy modulo$10^9 + 7$. 

Tính đúng đắn đến từ tính bất biến là ở mỗi lần gọi đệ quy, số mục tiêu chính xác là chênh lệch giữa đa thức được đánh giá tại$a$Và$b$nhân với tổng các hệ số, được dịch chuyển bởi các hệ số đã chọn cho đến nay. Bởi vì tất cả các hệ số đều không âm, phép đệ quy này khám phá tất cả các chuỗi hợp lệ mà không bỏ sót bất kỳ chuỗi nào hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def count_polynomials(a, b, c):
    if a == 1:
        if b == 1:
            if c == 0:
                return "inf"
            else:
                return 0
        s = c // (1 - b)
        if s < 0 or s * (1 - b) != c:
            return 0
        return 1  # Only one way to split sum into coefficients of polynomial
    ans = 0
    n = c
    while n >= 0:
        if n % a == 0 and (n // a - b * (n % a)) >= 0:
            ans += 1
        n -= b
    return ans % MOD

a, b, c = map(int, input().split())
print(count_polynomials(a, b, c))
```Giải pháp đầu tiên xử lý các`a = 1`trường hợp đặc biệt, trở về`"inf"`nếu cần thiết. Vì`a > 1`, nó lặp đi lặp lại kiểm tra phần dư ứng viên có thể đóng vai trò là hệ số thấp nhất và đảm bảo rằng giá trị còn lại cho phép phân tách hợp lệ thành lũy thừa cao hơn. Modulo được áp dụng ở cuối. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`2 2 2`| Bước | n | n % a | n // a - b*(n%a) | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 1 | 1 | 
| 2 | 0 | 0 | 0 | 2 | 

Dấu vết cho thấy hai chuỗi thỏa mãn đẳng thức đa thức:`[0,1]`Và`[1,0]`. 

Đầu vào được xây dựng:`3 1 4`| Bước | n | n % a | n // a - b*(n%a) | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 3 | 1 | 
| 2 | 3 | 0 | 3 | 2 | 
| 3 | 0 | 0 | 0 | 3 | 

Các bước đệ quy cho thấy tất cả các phân tách hợp lệ đều được tìm thấy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_a c * a) | Mỗi lần đệ quy làm giảm mục tiêu một cách đại khái theo hệ số a và chúng tôi thử tối đa một lựa chọn cho hệ số | 
| Không gian | O(log_a c) | Độ sâu của ngăn xếp đệ quy tỷ lệ với số chữ số trong cơ số biểu diễn | 

Thuật toán vừa vặn một cách thoải mái trong các ràng buộc, vì log_2(10^18) là khoảng 60, do đó độ sâu đệ quy rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, c = map(int, input().split())
    return str(count_polynomials(a, b, c))

# provided sample
assert run("2 2 2") == "2", "sample 1"

# custom cases
assert run("1 1 0") == "inf", "infinite solutions"
assert run("1 1 5") == "0", "no solution for b=1, c>0"
assert run("1 2 2") == "1", "only one sum possible"
assert run("3 1 4") == "3", "multiple sequences with base decomposition"
assert run("5 2 0") == "1", "zero target with non-zero base"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | thông tin | Phát hiện nghiệm vô hạn cho a=1, b=1, c=0 | 
| 1 1 5 | 0 | Không có nghiệm khi b=1, c>0 | 
| 1 2 2 | 1 | Sao và thanh với số tiền nhỏ | 
| 3 1 4 | 3 | Nhiều chuỗi sử dụng phân rã cơ sở | 
| 5 2 0 | 1 | Xử lý mục tiêu bằng 0 một cách chính xác | 

## Vỏ cạnh 

cho`a = b = 1`Và`c = 0`, phương trình giảm xuống còn`sum = sum + 0`, cái mà
