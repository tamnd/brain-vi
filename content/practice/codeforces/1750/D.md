---
title: "CF 1750D - Đếm GCD"
description: "Chúng ta được yêu cầu đếm các mảng $b$ khớp với một chuỗi các ước chung lớn nhất có tiền tố. Cụ thể, với mỗi vị trí $i$, GCD của phần tử $i$ đầu tiên của $b$ phải bằng $ai$. Mỗi phần tử $bi$ phải là số nguyên từ 1 đến $m$."
date: "2026-06-09T15:06:18+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1750
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 3 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1800
weight: 1750
solve_time_s: 80
verified: true
draft: false
---

[CF 1750D - Đếm GCD](https://codeforces.com/problemset/problem/1750/D) 

**Đánh giá:** 1800 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm mảng$b$khớp với một chuỗi tiền tố ước chung lớn nhất. Cụ thể đối với từng vị trí$i$, GCD đầu tiên$i$các yếu tố của$b$phải bằng$a_i$. Mỗi phần tử$b_i$phải là số nguyên từ 1 đến$m$. Đầu ra là số lượng mảng như vậy theo modulo$998{,}244{,}353$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một số nguyên$n$cho độ dài của mảng và một số nguyên$m$đạt giá trị tối đa cho phép. Sau đó$n$số nguyên$a_1, \dots, a_n$chỉ định các GCD tiền tố được yêu cầu. 

Những ràng buộc cho chúng ta biết$n$có thể lên đến$2\cdot 10^5$Và$m$lên đến$10^9$. Vì tổng của$n$trên các trường hợp thử nghiệm được giới hạn bởi$2\cdot 10^5$, chúng ta cần một giải pháp gần như tuyến tính trong$n$mỗi trường hợp thử nghiệm. Một cách tiếp cận ngây thơ liệt kê tất cả các mảng có thể$b$là không thể bởi vì$m^n$có kích thước lớn về mặt thiên văn. Bất kỳ giải pháp nào lặp lại trên tất cả các giá trị của$b_i$rõ ràng là không thể thực hiện được. 

Trường hợp cạnh không rõ ràng xảy ra khi chuỗi$a$không phải là không tăng theo nghĩa chia hết. Ví dụ, nếu$a = [2,3]$, không thể nào$b$bởi vì$\gcd(b_1) = 2$Và$\gcd(b_1,b_2) = 3$không thể xảy ra-3 không chia hết 2. Một giải pháp bất cẩn bỏ qua việc kiểm tra tính chia hết sẽ cố gắng tính sai giá trị$b_2$và tạo ra số đếm khác 0. 

Một sự tinh tế khác là khi$a_i$bằng$a_{i-1}$. Trong trường hợp đó,$b_i$phải là bội số của$a_i$, nhưng không có ràng buộc bổ sung nào phát sinh từ GCD trước đó, do đó số đếm bao gồm tất cả bội số của$a_i$lên đến$m$bảo tồn GCD. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là liệt kê mọi khả năng$b_1$TRONG$[1,m]$, thì với mỗi$b_1$mọi khả năng có thể$b_2$TRONG$[1,m]$, v.v., tính toán tiền tố GCD ở mỗi bước. Điều này sẽ mất$O(m^n)$, điều này là không thể thực hiện được vì$m$có thể$10^9$Và$n$lên đến$2\cdot 10^5$. 

Quan sát quan trọng là tiền tố GCD áp đặt các ràng buộc về khả năng chia hết. Cụ thể, mỗi$b_i$phải là bội số của$a_i$và để GCD giảm từ$a_{i-1}$ĐẾN$a_i$,$b_i$phải thỏa mãn$\gcd(b_i, a_{i-1}) = a_i$. Điều này tương đương với việc đếm số nguyên$x$như vậy$1 \le x \le \frac{m}{a_i}$Và$\gcd(x, \frac{a_{i-1}}{a_i}) = 1$. Ở đây, chúng tôi thay đổi quy mô bằng cách$a_i$để rút gọn bài toán về việc đếm các số nguyên nguyên tố cùng nhau với một số cho trước. Sử dụng hàm tổng Euler$\phi(k)$, chúng ta có thể tính số lượng các số nguyên lên đến$t$đó là nguyên tố cùng nhau$k$một cách hiệu quả. 

Quan sát này cho phép quét tuyến tính qua mảng$a$, tính toán$\phi(a_{i-1}/a_i)$cho mỗi bước, đưa ra một$O(n \sqrt{a_{i-1}/a_i})$hoặc nhanh hơn cho mỗi giải pháp trường hợp thử nghiệm. Nếu có$a_i$không chia$a_{i-1}$, đáp án ngay lập tức là 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) khấu hao | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$t$, số lượng trường hợp thử nghiệm và lặp lại chúng. 
2. Với mỗi test case, hãy đọc$n$Và$m$, thì mảng$a$. 
3. Khởi tạo một biến`ans = 1`để giữ sản phẩm của các lựa chọn hợp lệ modulo$998244353$. 
4. Nếu$a_0 > m$, bộ`ans = 0`bởi vì phần tử đầu tiên không thể vượt quá$m$. 
5. Lặp lại$i$từ 1 đến$n-1$: 

1. Kiểm tra xem$a_{i-1}$chia hết cho$a_i$. Nếu không thì đặt`ans = 0`và phá vỡ. 
2. Tính toán$k = a_{i-1} / a_i$. Số số nguyên$b_i = a_i \cdot x$như vậy$1 \le b_i \le m$Và$\gcd(b_i, a_{i-1}) = a_i$bằng với số số nguyên$x \le m / a_i$đó là nguyên tố cùng nhau với$k$. 
3. Nhân tố hóa$k$vào các yếu tố chính của nó. 
4. Sử dụng phép loại trừ đối với thừa số nguyên tố để đếm số nguyên$x \le m / a_i$nguyên tố cùng nhau để$k$. 
5. Nhân số này với`ans`modulo$998244353$. 
6. Đầu ra`ans`. 

Tại sao nó hoạt động: bất biến là ở bước$i$, chúng tôi duy trì số lượng mảng$b_1 \dots b_i$thỏa mãn các ràng buộc tiền tố GCD. Tính chia hết đảm bảo GCD có thể giảm từ$a_{i-1}$ĐẾN$a_i$và chia tỷ lệ theo$a_i$giảm việc đếm thành các số nguyên nguyên tố cùng nhau. Loại trừ bao gồm đếm chính xác các số nguyên nguyên tố cùng nhau$k$, đảm bảo tính đúng đắn 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def prime_factors(x):
    i = 2
    factors = set()
    while i * i <= x:
        if x % i == 0:
            factors.add(i)
            while x % i == 0:
                x //= i
        i += 1
    if x > 1:
        factors.add(x)
    return list(factors)

def count_coprime(n, primes):
    # Inclusion-Exclusion to count numbers <= n coprime with product of primes
    res = 0
    m = len(primes)
    for mask in range(1, 1 << m):
        prod = 1
        bits = 0
        for i in range(m):
            if mask & (1 << i):
                prod *= primes[i]
                bits += 1
        cnt = n // prod
        if bits % 2 == 1:
            res += cnt
        else:
            res -= cnt
    return n - res

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    if a[0] > m:
        print(0)
        continue
    ans = 1
    for i in range(1, n):
        if a[i-1] % a[i] != 0:
            ans = 0
            break
        k = a[i-1] // a[i]
        x_max = m // a[i]
        if k == 1:
            cnt = x_max
        else:
            primes = prime_factors(k)
            cnt = count_coprime(x_max, primes)
        ans = ans * cnt % MOD
    print(ans)
```các`prime_factors`hàm phân tích một cách hiệu quả một số bằng cách chia thử.`count_coprime`sử dụng loại trừ bao gồm đối với các số nguyên tố này để đếm các số nguyên đến giới hạn cùng nguyên tố với hệ số hóa. Việc nhân các số đếm này tôn trọng tính độc lập của các lựa chọn tại mỗi vị trí, đảm bảo tổng số đếm chính xác theo modulo$998244353$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 5\n4 2 1`| tôi | một [tôi] | một[i-1] | k = a[i-1]/a[i] | x_max = m//a[i] | số nguyên tố | đếm_coprime | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | - | - | 5//4=1 | - | 1 | 1 | 
| 2 | 2 | 4 | 2 | 5//2=2 | [2] | 2-1=1 | 1 | 
| 3 | 1 | 2 | 2 | 5//1=5 | [2] | 5-2=3 | 3 | 

Điều này phù hợp với đầu ra mẫu 3. 

### Ví dụ 2 

đầu vào:`2 1\n1 1`| tôi | một [tôi] | một[i-1] | k | x_max | số nguyên tố | đếm_coprime | trả lời | 

|---|
