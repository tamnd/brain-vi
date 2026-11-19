---
title: "CF 2091E - Tỷ lệ thú vị"
description: "Chúng ta được yêu cầu đếm các cặp số nguyên $(a, b)$ trong đó $1 le a < b le n$ sao cho tỷ lệ $F(a, b) = frac{text{lcm}(a, b)}{gcd(a, b)}$ là số nguyên tố."
date: "2026-06-08T05:47:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2091
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1013 (Div. 3)"
rating: 1300
weight: 2091
solve_time_s: 72
verified: true
draft: false
---

[CF 2091E - Tỷ lệ thú vị](https://codeforces.com/problemset/problem/2091/E) 

**Đánh giá:** 1300 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số, hai con trỏ 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các cặp số nguyên$(a, b)$Ở đâu$1 \le a < b \le n$sao cho tỷ lệ$F(a, b) = \frac{\text{lcm}(a, b)}{\gcd(a, b)}$là một số nguyên tố. Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp xác định số lượng tối đa$n$và đối với mỗi trường hợp thử nghiệm, chúng ta cần xuất ra số cặp thỏa mãn điều kiện này. 

Cái nhìn sâu sắc chính là$F(a, b) = \frac{\text{lcm}(a, b)}{\gcd(a, b)} = \frac{a \cdot b / \gcd(a, b)}{\gcd(a, b)} = \frac{a \cdot b}{\gcd(a, b)^2}$. Vì thế$F(a, b)$luôn là số nguyên và chúng ta cần nó là số nguyên tố. Vì số nguyên tố lớn hơn 1 nên$F(a, b) \ge 2$. 

Những hạn chế là đáng kể.$n$có thể lớn như$10^7$, và tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá$10^7$. Một cách tiếp cận ngây thơ để kiểm tra tất cả$O(n^2)$các cặp cho mỗi trường hợp thử nghiệm sẽ yêu cầu tới$10^{14}$trong trường hợp xấu nhất, điều này vượt xa khả năng thực hiện. Điều này cho thấy chúng ta cần một giải pháp gần với thời gian tuyến tính hoặc hơi siêu tuyến tính đối với$n$. 

Các trường hợp nguy hiểm có thể phá vỡ cách tiếp cận bất cẩn bao gồm các trường hợp nhỏ$n$giống$n = 2$, trong đó chỉ tồn tại một cặp$(1, 2)$, hoặc những trường hợp$a$là 1, vì$F(1, p) = p$cho bất kỳ số nguyên tố nào$p$. Một trường hợp phức tạp khác là khi$b$là bội số của$a$;$F(a, b)$đơn giản hóa để$\frac{b}{a}$, vì vậy chúng ta có thể tạo ra tất cả các tỷ lệ thú vị bằng cách xem xét bội số của một số nguyên cơ sở. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force liệt kê tất cả các cặp$(a, b)$với$1 \le a < b \le n$, tính toán$\gcd(a, b)$, tính toán$F(a, b)$, và kiểm tra xem nó có phải là số nguyên tố hay không. Mặc dù điều này đúng về mặt nguyên tắc, nhưng đối với$n \sim 10^7$cách tiếp cận này đòi hỏi$O(n^2 \log n)$thời gian vì tính toán$\gcd$và việc kiểm tra các số nguyên tố không phải là thời gian cố định. Điều này rõ ràng là quá chậm. 

Nhận xét quan trọng là thể hiện$b$BẰNG$b = a \cdot k$, Ở đâu$k > 1$là một số nguyên. Sau đó$\gcd(a, b) = a$Và$\text{lcm}(a, b) = a \cdot k$, Vì thế$F(a, b) = \frac{a \cdot k}{a} / a = k$. Vì thế,$F(a, b)$là số nguyên tố chính xác khi$b = a \cdot p$cho một số nguyên tố$p$. Vấn đề giảm xuống việc đếm tất cả bội số của mỗi số nguyên$a$bởi một số nguyên tố$p$sao cho sản phẩm không vượt quá$n$. 

Chúng ta có thể tính toán trước các số nguyên tố lên tới$n$sử dụng một cái sàng và sau đó cho mỗi$a$lặp qua tất cả các số nguyên tố$p$với$a \cdot p \le n$. Điều này làm giảm độ phức tạp xuống còn khoảng$O(n \log \log n)$cho sàng cộng thêm$O(n \log n)$để tạo các cặp, điều này khả thi với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(1) | Quá chậm | 
| bội số nguyên tố | O(n log log n + n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố đến mức tối đa$n$trên tất cả các trường hợp thử nghiệm sử dụng Sàng Eratosthenes. Điều này cho chúng ta một cách nhanh chóng để kiểm tra các số nguyên tố và liệt kê chúng. 
2. Khởi tạo một mảng`counts`Ở đâu`counts[i]`sẽ lưu trữ số lượng tỷ lệ thú vị cho một$i$. 
3. Với mỗi số nguyên$a$từ 1 đến$n$, lặp qua các số nguyên tố$p$như vậy$a \cdot p \le n$. Mỗi sản phẩm hợp lệ$b = a \cdot p$tạo thành một cặp hợp lệ$(a, b)$, do đó tăng lên`counts[a * p]`. 
4. Để xử lý nhiều trường hợp thử nghiệm một cách hiệu quả, hãy tích lũy số lượng cho tất cả các số nguyên lên tới`max_n`và lưu trữ tổng tiền tố nếu cần. 
5. Với mỗi test case có$n$, tính tổng đóng góp từ tất cả các cặp hợp lệ lên tới$n$và in kết quả. 
Tại sao nó hoạt động: Bằng cách tính toán$b$như bội số của$a$, chúng tôi đảm bảo$\gcd(a, b) = a$, Vì thế$F(a, b) = b / a$. Điều này đảm bảo kết quả là số nguyên và cho phép kiểm tra trực tiếp tính nguyên tố. Sàng đảm bảo rằng tất cả các thừa số nguyên tố đều được xem xét và việc lặp qua bội số đảm bảo rằng tất cả các cặp hợp lệ đều được tính chính xác một lần. Những hạn chế$1 \le a < b \le n$được duy trì một cách tự nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# sieve of Eratosthenes
def sieve(max_n):
    is_prime = [True] * (max_n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(max_n**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, max_n + 1, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    return primes, is_prime

def solve():
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)
    primes, _ = sieve(max_n)
    
    # precompute number of interesting ratios
    counts = [0] * (max_n + 1)
    for p in primes:
        for a in range(1, max_n // p + 1):
            counts[a * p] += 1
    
    # prefix sums
    for i in range(1, max_n + 1):
        counts[i] += counts[i - 1]
    
    for n in ns:
        print(counts[n])

if __name__ == "__main__":
    solve()
```Sàng tạo ra tất cả các số nguyên tố một cách hiệu quả. Chúng tôi lặp lại từng số nguyên tố và sau đó trên tất cả các bội số không vượt quá`max_n`. Việc sử dụng mảng tổng tiền tố cho phép chúng ta trả lời từng trường hợp kiểm thử trong thời gian không đổi. Cần chú ý cẩn thận đến các ranh giới: vòng lặp`a in range(1, max_n // p + 1)`đảm bảo chúng tôi không vượt quá mức tối đa cho phép$b$. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`n = 5`: 

| một | số nguyên tố p | một * p <= 5 | tính[a*p] sau số nguyên tố này | 
| --- | --- | --- | --- | 
| 1 | 2,3,5 | 2,3,5 | đếm[2]=1, đếm[3]=1, đếm[5]=1 | 
| 2 | 2 | 4 | đếm[4]=1 | 
| 3 | 2 | 6 > 5 | bỏ qua | 
| 4 | 2 | 8 > 5 | bỏ qua | 
| 5 | 2 | 10 > 5 | bỏ qua | 

Tổng tiền tố mang lại tổng số`counts[5] = 4`. 

Điều này xác nhận thuật toán đếm cặp`(1,2)`,`(1,3)`,`(1,5)`,`(2,4)`một cách chính xác. 

Đầu vào mẫu`n = 10`: 

| một | p | a*p <= 10 | đếm cặp | 
| --- | --- | --- | --- | 
| 1 | 2,3,5,7 | 2,3,5,7 | 4 | 
| 2 | 2,3,5 | 4,6,10 | 3 nữa | 
| 3 | 2,3 | 6,9 | 2 nữa | 
| 4 | 2 | 8 | 1 nữa | 
| 5 | 2 | 10 | đã được tính bằng 2*5 chưa? tính riêng | 
| 6..10 | chỉ có sản phẩm >10 | bỏ qua | | 

Tổng số = 11, phù hợp với sản lượng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log log n + n log n) | sàng + lặp qua các số nguyên tố và bội số | 
| Không gian | O(n) | lưu trữ số đếm và mảng sàng | 

Điều này phù hợp thoải mái trong giới hạn thời gian 2 giây cho`n ≤ 10^7`và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n5\n10\n34\n10007\n") == "4\n11\n49\n24317"

# minimum-size input
assert run("1\n2\n") == "1"

# maximum-size input (just check it runs)
assert run("1\n10000000\n
```
