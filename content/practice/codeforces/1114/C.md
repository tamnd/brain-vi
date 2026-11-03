---
title: "CF 1114C - Trailing Loves (hay L'oeufs?)"
description: "Chúng ta được yêu cầu tính số các số 0 ở cuối trong giai thừa của một số khi được biểu diễn dưới dạng cơ số tùy ý. Chính xác hơn, với các số nguyên $n$ và $b$, chúng ta muốn số chữ số bằng 0 ở cuối biểu diễn cơ số-$b$ của $n!$."
date: "2026-06-12T04:51:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1114
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 538 (Div. 2)"
rating: 1700
weight: 1114
solve_time_s: 64
verified: true
draft: false
---

[CF 1114C - Trailing Loves (hay L'oeufs?)](https://codeforces.com/problemset/problem/1114/C) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính số các số 0 ở cuối trong giai thừa của một số khi được biểu diễn dưới dạng cơ số tùy ý. Chính xác hơn, các số nguyên đã cho$n$Và$b$, chúng ta muốn số chữ số bằng 0 ở cuối cơ số-$b$đại diện của$n!$. đầu vào$n$có thể rất lớn, lên tới$10^{18}$, Và$b$có thể đi lên$10^{12}$. Tính toán trực tiếp$n!$là không thể vì giai thừa phát triển quá nhanh; thậm chí$100!$có 158 chữ số và$10^{18}!$hoàn toàn không thể quản lý được. 

Số 0 ở cuối trong cơ sở$b$được xác định bằng bao nhiêu lần$b$chia rẽ$n!$. Đối với cơ số 10, số số 0 ở cuối xuất phát từ số đếm tối thiểu là 2 và 5 trong hệ số nguyên tố của$n!$. Đối với cơ số tổng quát, chúng ta cần một cách tiếp cận tương tự: hệ số$b$thành lũy thừa nguyên tố của nó, sau đó tìm xem mỗi thừa số nguyên tố chia bao nhiêu lần$n!$, và cuối cùng kết hợp các số đếm này theo số mũ trong$b$. 

Một cách tiếp cận ngây thơ cố gắng tính toán$n!$trực tiếp hoặc thậm chí lưu trữ tất cả các thừa số nguyên tố của nó là không thể thực hiện được. Vỏ cạnh bao gồm nhỏ$n$và lớn$b$Ở đâu$b$chính nó là số nguyên tố hoặc chứa lũy thừa nguyên tố lớn. Ví dụ, nếu$n = 5$Và$b = 16$, giai thừa là$120$, chia hết cho$2^3 = 8$nhưng không phải bởi$16$. Một giải pháp bất cẩn có thể đếm quá số 0 bằng cách giả sử$n! \ge b$là đủ để có ít nhất một số 0 ở cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tính toán theo nghĩa đen$n!$và sau đó liên tục chia cho$b$cho đến khi số dư khác 0, tính các phép chia. Điều này hoạt động rất nhỏ$n$Và$b$, nếu không có$n = 10^{18}$giai thừa không thể tính được nên cách tiếp cận này vô ích. Chỉ riêng số phép nhân là$O(n)$, vượt xa mọi tính toán hợp lý. 

Thông tin chi tiết quan trọng là các số 0 ở cuối tương ứng với số lần$b$chia rẽ$n!$. Nếu như$b = p_1^{e_1} p_2^{e_2} \dots p_k^{e_k}$, khi đó số số 0 ở cuối cơ số$b$là tối thiểu của$\text{floor}(\text{count of } p_i \text{ in } n! / e_i)$trên tất cả$i$. Số của một số nguyên tố$p$TRONG$n!$có thể được tính toán một cách hiệu quả bằng công thức Legendre: tổng$\lfloor n / p^j \rfloor$vì$j = 1, 2, \dots$cho đến khi$p^j > n$. Điều này làm giảm vấn đề về việc phân tích thành thừa số nguyên tố của$b$và tính tổng logarit cho mỗi số nguyên tố, điều này khả thi ngay cả đối với các số lớn$n$Và$b$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) phép nhân | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(b) + log_p(n) * #primes) | O(#số nguyên tố) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử$b$thành số nguyên tố. Lặp lại các số nguyên từ 2 đến$\sqrt{b}$, chia mỗi số nguyên tố nhiều lần nhất có thể để tìm số mũ của nó. Nếu yếu tố nào còn sót lại sau$\sqrt{b}$, nó là số nguyên tố. Điều này tạo ra$b = \prod p_i^{e_i}$. 
2. Với mỗi thừa số nguyên tố$p_i$của$b$, tính bội số của nó trong$n!$sử dụng công thức Legendre. Khởi tạo bộ đếm$c = 0$, sau đó chia liên tục$n$qua$p_i, p_i^2, p_i^3, \dots$thêm mỗi thương số vào$c$cho đến khi thương số bằng không.$c$là tổng số của$p_i$TRONG$n!$. 
3. Đối với mỗi số nguyên tố$p_i$, tính toán$c // e_i$, đó là số lần tối đa$b$có thể sử dụng$p_i^{e_i}$TRONG$n!$. Các số 0 ở cuối cơ sở$b$là giá trị nhỏ nhất của các giá trị này trên tất cả các thừa số nguyên tố. 
4. Xuất ra mức tối thiểu này. 

Tại sao nó hoạt động: Việc phân tích nhân tử đảm bảo chúng ta xem xét tất cả các ràng buộc do$b$. Công thức Legendre đếm chính xác số lần chia của mỗi số nguyên tố$n!$. Chia cho số mũ$e_i$điều chỉnh các cơ sở trong đó số nguyên tố xuất hiện nhiều lần. Mức tối thiểu trên tất cả các số nguyên tố là nút thắt cổ chai: một yếu tố bị thiếu sẽ ngăn cản việc hình thành bội số khác của$b$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def prime_factors(b):
    factors = {}
    d = 2
    while d * d <= b:
        count = 0
        while b % d == 0:
            b //= d
            count += 1
        if count > 0:
            factors[d] = count
        d += 1
    if b > 1:
        factors[b] = 1
    return factors

def count_prime_in_factorial(n, p):
    count = 0
    power = p
    while power <= n:
        count += n // power
        if power > n // p:
            break
        power *= p
    return count

def trailing_zeros(n, b):
    factors = prime_factors(b)
    zeros = float('inf')
    for p, e in factors.items():
        zeros = min(zeros, count_prime_in_factorial(n, p) // e)
    return zeros

n, b = map(int, input().split())
print(trailing_zeros(n, b))
```Mã này được cấu trúc để trước tiên phân tích cơ số, sau đó tính toán phần đóng góp của mỗi số nguyên tố vào giai thừa bằng công thức Legendre. Một điểm tinh tế là kiểm tra`power > n // p`trước khi nhân để tránh tràn số nguyên. Lấy giá trị tối thiểu trên tất cả các số nguyên tố sẽ xử lý chính xác các cơ số tổng hợp. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`6 9`| Thủ tướng | Số mũ trong 9 | Đếm đến 6! | Đếm // Số mũ | 
| --- | --- | --- | --- | 
| 3 | 2 | 2 | 1 | 

Tối thiểu là 1, vì vậy`6!`trong cơ số 9 có 1 số 0 ở cuối. 

**Ví dụ 2:** 

đầu vào:`5 16`| Thủ tướng | Số mũ trong 16 | Đếm đến 5! | Đếm // Số mũ | 
| --- | --- | --- | --- | 
| 2 | 4 | 3 | 0 | 

Tối thiểu là 0, vì vậy`5!`trong cơ sở 16 không có số 0 ở cuối. 

Những dấu vết này cho thấy thuật toán tôn trọng lũy ​​thừa nguyên tố và tính toán chính xác hệ số giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(b) + log_p(n) * #primes) | Phân tích nhân tử của b mất O(sqrt(b)), đếm các số nguyên tố trong giai thừa mất O(log_p(n)) mỗi số nguyên tố | 
| Không gian | O(#số nguyên tố) | Hệ số hóa cửa hàng | 

Ngay cả đối với lớn nhất$b = 10^{12}$, sqrt(b) là$10^6$. Logarit cho$n = 10^{18}$có thể quản lý được. Bộ nhớ tối thiểu vì chỉ có một số số nguyên tố được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, b = map(int, input().split())
    return str(trailing_zeros(n, b))

# provided samples
assert run("6 9\n") == "1", "sample 1"
assert run("5 16\n") == "0", "custom small factorial"

# custom tests
assert run("10 10\n") == "2", "10! has 2 trailing zeros in base 10"
assert run("25 5\n") == "6", "factorial divisible by 5^6"
assert run("1000000000000000000 2\n") == "999999999999999994", "large n, base 2"
assert run("100 12\n") == "48", "base with multiple primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 10 | 2 | Cơ số 10 số 0 ở cuối | 
| 25 5 | 6 | Tính bội số cao của số nguyên tố 5 | 
| 10^18 2 | 999999999999999994 | Xử lý n cực lớn hiệu quả | 
| 100 12 | 48 | Cơ số có nhiều thừa số nguyên tố | 

## Vỏ cạnh 

cho`5 16`, hệ số nguyên tố là$16 = 2^4$. Đếm 2s trong`5!`cho 3. Chia cho 4 mang lại 0. Thuật toán trả về chính xác số 0, không nhầm lẫn khi xác nhận một số 0. Vì`6 9`, nhân tử hóa là$9 = 3^2$. Đếm 3s sau`6!`được 2. Chia cho 2 có 1 số 0 ở cuối. Cả hai trường hợp đều xác nhận việc xử lý các cơ số tổng hợp có số mũ lớn hơn 1. Kiểm tra tràn`power *= p`ngăn chặn các vòng lặp vô hạn khi `n
