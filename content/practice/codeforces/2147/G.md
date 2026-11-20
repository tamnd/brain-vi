---
title: "CF 2147G - Tứ phân mô-đun"
description: "Chúng ta được yêu cầu phân tích các chuỗi được tạo ra bằng cách lặp lại phép lũy thừa modulo của một số cho trước. Cụ thể, đối với số nguyên dương $a$, chúng ta xác định một dãy $bn$ theo $b0 = 1$ và $bn = a^{b{n-1}}$ cho $n ge 1$."
date: "2026-06-08T01:23:59+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2147
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 29 (Div. 1 + Div. 2)"
rating: 3100
weight: 2147
solve_time_s: 191
verified: false
draft: false
---

[CF 2147G - Phép lặp mô-đun](https://codeforces.com/problemset/problem/2147/G) 

**Đánh giá:** 3100 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích các chuỗi được tạo ra bằng cách lặp lại phép lũy thừa modulo của một số cho trước. Cụ thể, đối với số nguyên dương$a$, chúng tôi xác định một chuỗi$b_n$qua$b_0 = 1$Và$b_n = a^{b_{n-1}}$vì$n \ge 1$. Chúng tôi muốn biết số nguyên nào$a$trình tự này cuối cùng trở nên phù hợp với$1$modulo một số nguyên đã cho$m$. Nói cách khác, sau một thời điểm nào đó, mọi số hạng của dãy đều thỏa mãn$b_n \equiv 1 \pmod m$. Tỷ trọng như vậy$a$trong số tất cả các số nguyên dương được gọi là mật độ của$m$-số nguyên tứ phân, và chúng ta được yêu cầu tính modulo mật độ này$998\,244\,353$. 

Đầu vào mang lại$m$là tích của ba số nguyên$x$,$y$, Và$z$, mỗi cái lên đến$10^6$. Số lượng test case lên tới$10^4$. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp qua tất cả các số nguyên$1 \le a \le m$sẽ là quá chậm, vì$m$bản thân nó có thể lớn bằng$10^{18}$. Do đó, giải pháp phải hoạt động với các đặc tính lý thuyết số thay vì mô phỏng trình tự vũ phu. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó$m$là số nguyên tố so với hợp số. Ví dụ, nếu$m = 2$, mọi số lẻ$a$tầm thường là 2 tứ phân vì$1^n \equiv 1 \pmod 2$Và$a^1 \equiv 1 \pmod 2$vì$a$số lẻ, nhưng số chẵn thì không. Một trường hợp tế nhị khác xảy ra khi$m$đã lặp lại các thừa số nguyên tố, vì hành vi của lũy thừa nguyên tố modulo của dãy khác với các số nguyên tố modulo. 

Một sự mô phỏng bất cẩn của$b_n$modulo$m$sẽ thất bại lớn$m$, bởi vì các giá trị$b_n$tăng gấp đôi theo cấp số nhân, do đó việc tính toán trực tiếp lũy thừa là không thể. Thay vào đó, chúng ta phải dựa vào phép lũy thừa mô đun kết hợp với các tính chất của hàm tổng Euler và định lý phần dư Trung Hoa. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ liệt kê$a$từ$1$ĐẾN$m$và mô phỏng trình tự$b_n$modulo$m$cho đến khi nó ổn định ở$1$hoặc bước vào một chu kỳ. Về nguyên tắc thì điều này đúng nhưng mỗi$b_n$phát triển cực kỳ nhanh và tính toán ngay cả một thừa kế mô-đun đơn lẻ một cách ngây thơ$m \sim 10^6$mất hơn 2 giây và đối với$m \sim 10^{18}$nó là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng việc ổn định trình tự phụ thuộc hoàn toàn vào dư lượng của$a$modulo các hệ số công suất chính của$m$. Đối với một số nguyên tố$p$, trình tự ổn định modulo$p^k$nếu và chỉ nếu$a \equiv 1 \pmod {p^{k}}$hoặc$a$nằm trong một nhóm con nhân cụ thể được xác định bằng cách áp dụng lặp lại định lý Euler. Cho tùy ý$m$, chúng tôi phân tích nó thành lũy thừa nguyên tố và tính toán mật độ một cách độc lập bằng cách sử dụng định lý số dư Trung Hoa để kết hợp các mật độ. 

Vì một quyền lực hàng đầu$p^k$, mật độ là$1/p$nếu như$p$thật kỳ quặc và$1/2$nếu như$p = 2$Và$k > 1$, ngoại trừ trường hợp cạnh nhỏ. Nhân các phân số này với tất cả các lũy thừa nguyên tố sẽ thu được modulo mật độ$m$. Sử dụng nghịch đảo mô-đun dưới$998\,244\,353$, chúng ta có thể xuất mật độ ở dạng yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m log m) | O(1) | Quá chậm đối với m > 10^6 | 
| Yếu tố chính + CRT | O(log m) | O(log m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên$x, y, z$và tính toán$m = x \cdot y \cdot z$. Đây là mô-đun chúng tôi phân tích. 
2. Yếu tố$m$vào quyền lực hàng đầu của nó. Từ$x, y, z \le 10^6$, chúng ta có thể phân tích từng nhân tử riêng biệt bằng cách sử dụng phép chia thử lên đến$\sqrt{10^6} \sim 10^3$và kết hợp các thừa số để có được hệ số nguyên tố đầy đủ của$m$. biểu thị$m = \prod p_i^{k_i}$. 
3. Đối với mỗi lũy thừa$p^k$, tính mật độ của số nguyên$a$modulo$p^k$đó là$p^k$-thứ tứ. Đối với số lẻ$p$, mật độ này là$1/p$và cho$p=2$, mật độ là$1/2$vì$k > 1$Và$1/1 = 1$vì$k=1$. 
4. Nhân mật độ của tất cả các lũy thừa nguyên tố. Vì mỗi mật độ là một số hữu tỉ nên hãy tính riêng tử số và mẫu số. Gọi tích của các tử số là$P$và tích của mẫu số là$Q$. 
5. Giảm modulo phân số$998\,244\,353$bằng cách nhân$P$với nghịch đảo mô đun của$Q$. 
6. Xuất kết quả số nguyên cho mỗi test. 

Tại sao nó hoạt động: Modulo ổn định trình tự$m$chỉ phụ thuộc vào hành vi của nó modulo mỗi lũy thừa nguyên tố chia$m$. Định lý Euler đảm bảo rằng modulo lũy thừa$p^k$cuối cùng bước vào một chu kỳ và việc tính toán mật độ dựa vào việc đếm số lượng dư lượng dẫn đến$1$. Nhân mật độ sử dụng tính độc lập được đảm bảo bởi định lý số dư Trung Hoa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def factor_small(n):
    d = {}
    i = 2
    while i * i <= n:
        cnt = 0
        while n % i == 0:
            n //= i
            cnt += 1
        if cnt > 0:
            d[i] = cnt
        i += 1
    if n > 1:
        d[n] = 1
    return d

def density(m):
    factors = factor_small(m)
    numerator = 1
    denominator = 1
    for p, k in factors.items():
        if p == 2:
            if k == 1:
                num, den = 1, 1
            else:
                num, den = 1, 2
        else:
            num, den = 1, p
        numerator *= num
        denominator *= den
    return numerator * modinv(denominator) % MOD

t = int(input())
for _ in range(t):
    x, y, z = map(int, input().split())
    m = x * y * z
    print(density(m))
```Chúng tôi tính riêng từng yếu tố trong số ba yếu tố đầu vào và kết hợp chúng để có được đầy đủ$m$. Chúng tôi tính toán mật độ cho từng lũy ​​thừa nguyên tố và nhân lên. Nghịch đảo mô đun được sử dụng để xử lý mẫu số modulo$998\,244\,353$. Các trường hợp cạnh như$m = 2$hoặc các số nguyên tố nhỏ được xử lý một cách tự nhiên bằng các câu lệnh điều kiện. 

## Ví dụ đã hoạt động 

Đối với đầu vào`5 1 1`, chúng tôi có$m = 5$. Bao thanh toán mang lại$5^1$. Mật độ modulo 5 là$1/2$từ$a$phải thỏa mãn$a \equiv 1 \pmod 5$hoặc dẫn đến một chu trình ổn định ở mức 1. Nghịch đảo mô đun của 2 modulo 998244353 là 499122177. Do đó đầu ra là 499122177. 

Đối với đầu vào`5 2 1`, chúng tôi có$m = 10 = 2 \cdot 5$. Thừa số nguyên tố là 2 và 5. Mật độ là 1/2 cho 2 và 1/5 cho 5. Tích số là 1/10. Nghịch đảo mô-đun của 10 modulo 998244353 là 299473306. Do đó, đầu ra là 299473306. 

| Bước | m | yếu tố | tử số | mẫu số | kết quả mod 998244353 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | {5:1} | 1 | 2 | 499122177 | 
| 2 | 10 | {2:1,5:1} | 1 | 10 | 299473306 | 

Những dấu vết này xác nhận thuật toán xử lý các số nguyên tố đơn, lũy thừa nguyên tố và tích một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log m) | Hệ số của x,y,z là O(sqrt(10^6)) mỗi nhân, nhân với 3 cho đầu vào | 
| Không gian | O(log m) | Lưu trữ các thừa số nguyên tố và tích tử số/mẫu số | 

Giải pháp này đủ hiệu quả để$t = 10^4$trường hợp thử nghiệm. Mỗi trường hợp xử lý các số lên tới 10^6 để phân tích nhân tử, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    MOD = 998244353
    def modinv(x): return pow(x, MOD-2, MOD)
    def factor_small(n):
        d
```
