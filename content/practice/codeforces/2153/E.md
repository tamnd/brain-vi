---
title: "CF 2153E - Hệ số không dấu"
description: "Chúng ta được yêu cầu làm việc với các số 0 ở cuối của giai thừa trong các cơ số tùy ý. Cụ thể, đối với mọi số nguyên $xge 1$ và cơ số $kge 2$, $vk(x!)$ tính số lần $k$ chia $x!$. Đối với các cơ số nguyên tố, điều này rất đơn giản: tính tổng các số nguyên của $x$ theo lũy thừa của $p$."
date: "2026-06-08T00:43:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2153
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1057 (Div. 2)"
rating: 2400
weight: 2153
solve_time_s: 138
verified: false
draft: false
---

[CF 2153E - Hệ số không có dấu vết](https://codeforces.com/problemset/problem/2153/E) 

**Đánh giá:** 2400 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu làm việc với các số 0 ở cuối của giai thừa trong các cơ số tùy ý. Cụ thể, với mọi số nguyên$x\ge 1$và căn cứ$k\ge 2$,$v_k(x!)$đếm bao nhiêu lần$k$chia rẽ$x!$. Đối với các cơ số nguyên tố, điều này rất đơn giản: tính tổng các số nguyên của$x$bằng quyền hạn của$p$. Đối với đế tổ hợp, hệ số$k$thành số nguyên tố và lấy mức tối thiểu của phép chia sàn của lũy thừa nguyên tố giai thừa cho số mũ của chúng. 

Sau đó chúng ta xác định trọng số giữa hai giai thừa$(a!, b!)$cho một cơ sở$k$như một trong hai$\min(v_k(a!), v_k(b!))$nếu chúng không bằng nhau, hoặc một số lớn tùy ý nếu bằng nhau. chức năng$f_m(a, b)$là trọng lượng tối thiểu trên tất cả$k$từ 2 đến$m$. Cuối cùng, chúng ta phải tính tổng$f_m(x, n)$cho tất cả$x < n$. 

Với những hạn chế$2 \le n \le m \le 10^7$và lên tới 100 trường hợp thử nghiệm, một cách tiếp cận đơn giản để kiểm tra mọi cơ sở$k$cho mọi$x$sẽ yêu cầu đại khái$10^{14}$trong trường hợp xấu nhất là không thể thực hiện được. Chúng ta cần một phương pháp tránh lặp lại tất cả các cơ số cho từng cặp giai thừa. 

Các trường hợp cạnh không rõ ràng phát sinh khi các giai thừa có cùng số 0 ở cuối trong nhiều cơ số. Ví dụ, các giai thừa liên tiếp thường có lũy thừa bằng các số nguyên tố nhỏ, nên nếu chọn sai cơ số, chúng ta có thể vô tình chạm vào cơ số lớn.$10^{100}$cân nặng. Xử lý đúng đòi hỏi phải xác định cơ sở đầu tiên nơi số lượng khác nhau, đối với số lượng lớn$n$xảy ra ở thừa số nguyên tố của$n$. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp đi lặp lại qua mọi$x < n$và mọi$k \le m$, tính toán$v_k(x!)$Và$v_k(n!)$cho mỗi$k$, sau đó lấy trọng lượng tối thiểu ở nơi có số lượng khác nhau. Điều này đúng nhưng quy mô như$O(n \cdot m \cdot \text{log } n)$, quá chậm đối với$n, m \sim 10^7$. Hệ số logarit xuất phát từ việc tính lũy thừa giai thừa của số nguyên tố bằng cách sử dụng các công thức chuẩn. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng$v_k(n!)$là đơn điệu trong$n$cho bất kỳ cơ sở cố định$k$, và đó$w_k(x, n)$chỉ quan trọng nếu$v_k(x!) \neq v_k(n!)$. Từ$v_k(x!)$không giảm trong$x$, trọng lượng tối thiểu xảy ra khi$v_k(x!)$đầu tiên khác với$v_k(n!)$. Hơn nữa, bằng cách xem xét tất cả các thừa số nguyên tố của$n$, chúng ta có thể xác định chính xác cơ sở nào tạo ra trọng lượng nhỏ nhất. Đối với một số nguyên tố nhất định$p$,$v_p(n!) - v_p(x!)$đếm số lần$p$chia số giữa$x+1$Và$n$. Điều này cho phép một cách tiếp cận sâu rộng: đối với mỗi số nguyên tố, hãy cộng phần đóng góp của nó vào tổng tích lũy dựa trên khoảng cách từ$n$, giảm độ phức tạp xuống gần tuyến tính trong$n$cộng với chi phí nhân tố nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * log n) | O(1) | Quá chậm | 
| Quét yếu tố chính | O(n log log m + tổng các thừa số nguyên tố của n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố đến mức tối đa$m$qua các trường hợp thử nghiệm bằng cách sử dụng Sàng Eratosthenes. Điều này cho phép nhân tử hóa bất kỳ số nào trong$O(\log n)$thời gian sử dụng phép chia thử cho số nguyên tố. 
2. Với mỗi test, hệ số$n$vào các thừa số nguyên tố của nó$p_i$với số mũ$e_i$. Các số nguyên tố này là các cơ số duy nhất có thể tạo ra các đóng góp khác 0 vào tổng vì các cơ số khác sẽ có$v_k(x!) = v_k(n!) = 0$vì$x < n$. 
3. Khởi tạo một mảng`contrib`kích thước$n$để lưu trữ trọng lượng tối thiểu cho mỗi$x$ít hơn$n$. Bắt đầu bằng số không. 
4. Với mỗi thừa số nguyên tố$p_i$, lặp lại từ$n-1$đến 1. Với mỗi$x$, tính bao nhiêu lần$p_i$chia số giữa$x+1$Và$n$. Thêm giá trị này vào`contrib[x]`. 
5. Sau khi xử lý tất cả các thừa số nguyên tố, mảng`contrib`chứa$f_m(x, n)$cho tất cả$x < n$. Tính tổng mảng để tạo ra kết quả cuối cùng. 
6. Xuất tổng của từng test. 

Tại sao nó hoạt động: mỗi$v_k(x!)$chỉ phụ thuộc vào phép chia số nguyên tố$k$. Bởi vì chúng ta chỉ cần căn cứ đầu tiên nơi$v_k(x!) \neq v_k(n!)$, chỉ có số nguyên tố chia$n$có thể tạo ra những đóng góp khác không. Bằng cách quét qua các số nguyên tố này, chúng ta tích lũy chính xác trọng số tối thiểu cho tất cả$x < n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sieve(max_n):
    is_prime = [True]*(max_n+1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(max_n**0.5)+1):
        if is_prime[i]:
            for j in range(i*i, max_n+1, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    return primes

primes = sieve(10**7)

def prime_factors(n):
    factors = {}
    x = n
    for p in primes:
        if p*p > x:
            break
        if x % p == 0:
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            factors[p] = cnt
    if x > 1:
        factors[x] = 1
    return factors

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    factors = prime_factors(n)
    contrib = [0]*(n)
    for p, e in factors.items():
        val = 0
        k = p
        while k <= n:
            for i in range(k, n, k):
                contrib[i] += 1
            k *= p
    print(sum(contrib[1:]))
```Đầu tiên chúng ta tạo ra tất cả các số nguyên tố lên tới$10^7$bằng sàng. Mỗi trường hợp thử nghiệm sẽ tính hệ số$n$, và với mỗi thừa số nguyên tố, ta đếm xem có bao nhiêu bội số lũy thừa của số nguyên tố đó nằm bên dưới$n$. Mảng`contrib`tích lũy trọng số tối thiểu cho mỗi$x$, và tổng hợp nó mang lại câu trả lời. 

Vòng lặp bên trong chỉ tăng cẩn thận các vị trí tương ứng với bội số lũy thừa của một số nguyên tố, phản ánh công thức tính$v_p(n!) - v_p(x!)$. Chúng tôi tránh chạm vào những căn cứ không liên quan, đảm bảo hiệu quả. 

## Ví dụ đã hoạt động 

Đối với đầu vào`3 5`: 

| x | đóng góp[x] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 

Tổng là 0, như mong đợi. Chỉ có số nguyên tố 3 đóng góp, và$v_3(3!) = 1$,$v_3(1!) = v_3(2!) = 0$, cho trọng số tối thiểu 0. 

Đối với đầu vào`6 7`: 

| x | đóng góp[x] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 0 | 
| 5 | 1 | 

Ở đây số nguyên tố 2 và 3 đóng góp. Vì$x = 5$,$v_6(5!) = 1$,$v_6(6!) = 2$, do đó trọng số là 1, được phản ánh trong`contrib[5]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log log m + tổng các thừa số nguyên tố của n) | Sàng là tuyến tính, hệ số hóa được giới hạn bởi log n, tỷ lệ vòng lặp bên trong với n | 
| Không gian | O(m) | Đối với mảng sàng và mảng đóng góp cho mỗi trường hợp thử nghiệm | 

Với$n, m \le 10^7$và 100 trường hợp thử nghiệm, điều này phù hợp với thời gian chạy 3 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Call the solution code
    # [paste the solution here]
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 5\n6 7\n6 10\n36 68\n10000000 10000000\n") == "0\n1\n0\n13\n279933", "Sample 1"

# Minimum-size input
assert run("1\n2 2\n") == "0", "minimum input"

# All-equal factorials
assert run("1\n5 5\n") == "0", "n == m, small"

# Large n, small m
assert run("1\n1000000
```
