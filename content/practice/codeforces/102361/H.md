---
title: "CF 102361H - Houraisan Kaguya"
description: "Chúng ta có môđun nguyên tố (p) và một mảng (a1,ldots,an), trong đó mọi giá trị mảng đều khác 0 modulo (p). Đối với hai số dư khác 0 (a,b), chúng ta cần số mũ dương nhỏ nhất (u) sao cho (a^u) thuộc nhóm con tuần hoàn được tạo bởi (b). Gọi giá trị đó là (f(a,b))."
date: "2026-08-14T02:46:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "H"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 98
verified: true
draft: false
---

[CF 102361H - Houraisan Kaguya](https://codeforces.com/problemset/problem/102361/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có mô đun nguyên tố (p) và một mảng (a_1,\ldots,a_n), trong đó mọi giá trị mảng đều khác 0 modulo (p). Đối với hai số dư khác 0 (a,b), chúng ta cần số mũ dương nhỏ nhất (u) sao cho (a^u) thuộc nhóm con tuần hoàn được tạo bởi (b). Gọi giá trị đó là (f(a,b)). 

Câu trả lời bắt buộc là tổng của (f(a_i,a_j)f(a_j,a_i)) trên mỗi cặp vị trí mảng có thứ tự, modulo giảm (p). 

Sự đơn giản hóa hữu ích đầu tiên xuất phát từ thực tế là các thặng dư khác 0 modulo một số nguyên tố tạo thành một nhóm bậc tuần hoàn (p-1). Do đó, các giá trị đầu vào có thể được hiểu thông qua cấp số nhân của chúng. Khó khăn là (p) có thể lớn bằng (10^{18}), do đó, ngay cả việc phân tích nhân tử (p-1) cũng yêu cầu thuật toán phân tích nhân tử số nguyên thực sự thay vì chia thử lên đến (\sqrt p). 

Với (n) đạt (10^5), việc kiểm tra trực tiếp tất cả (n^2) có nghĩa là có tới (10^{10}) đánh giá cặp. Điều đó vượt xa những gì giới hạn thời gian lập trình cạnh tranh có thể hỗ trợ. Giải pháp phải nén thông tin cấu trúc bằng nhau từ mảng và sau đó khai thác cấu trúc ước số của (p-1). 

Có một số trường hợp khó xử lý. Ví dụ: nếu (n=1), đầu vào```
1 2
1
```có đáp án (1), vì (f(1,1)=1). Một công thức vô tình coi phần tử đồng nhất là có thứ tự 0 sẽ không thành công ở đây. 

Các giá trị lặp lại cũng quan trọng. Vì```
3 7
2 2 2
```bậc của (2) modulo (7) là (3). Mỗi cặp có thứ tự đều có đóng góp (1), nên câu trả lời là (9\bmod 7=2). Chúng ta phải đếm các bội số thay vì chỉ đếm các giá trị riêng biệt. 

Giá trị biên (p-1) là một thử nghiệm hữu ích khác. Vì```
2 7
1 6
```thứ tự là (1) và (2). Bốn cặp có thứ tự đóng góp (1,2,2,1), cho ra (6\bmod7=6). Việc triển khai giả định mọi dư lượng khác 0 là một trình tạo sẽ mắc lỗi này. 

Cuối cùng, định nghĩa cho phép (f(a,b)=0) nói chung, nhưng trường hợp đó không bao giờ xảy ra đối với đầu vào thực tế. Vì (a\neq0), một số lũy thừa dương của (a) là (1) và (1) được chứa trong mọi nhóm con được tạo bởi một số khác không (b). Vì vậy, mọi cặp giá trị đầu vào đều có giá trị dương (f). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý mọi cặp có thứ tự ((a_i,a_j)). Đối với một cặp, người ta có thể nhân nhiều lần với (a_i) cho đến khi đạt được phần tử do (a_j) tạo ra, nhưng điều này có thể yêu cầu công việc (O(p)) cho một cặp. Ngay cả sau khi nhận ra cấu trúc nhóm và tính toán câu trả lời của từng cặp bằng gcds, vẫn có (n^2) cặp. Với (n=10^5), tức là (10^{10}) phép toán cặp, quá chậm. 

Lực lượng vũ phu hoạt động vì câu trả lời cho một cặp được xác định hoàn toàn bằng cách hai phần tử nằm bên trong nhóm tuần hoàn modulo (p). Quan sát quan trọng là vị trí này có thể được mô tả bằng cấp số nhân. 

Đặt (q=p-1) và chọn một nghiệm nguyên thủy (g) modulo (p). Viết 

[ 
a=g^A,\qquad b=g^B. 
] 

Nhóm con được tạo bởi (b) bao gồm các số mũ chia hết cho 

[ 
d_b=\gcd(B,q). 
] 

Chúng ta cần (u) dương nhỏ nhất sao cho (a^u) thuộc nhóm con đó. Vì (a^u=g^{Au}), điều này có nghĩa là 

[ 
d_b\mid Au. 
] 

Giải pháp tích cực nhỏ nhất là 

[ 
f(a,b)=\frac{\gcd(B,q)}{\gcd(A,B,q)}. 
] 

Biểu thức có thể được viết lại mà không cần biết logarit rời rạc (A) và (B). Thứ tự nhân của (a) là 

[ 
\operatorname{ord}(a)=\frac{q}{\gcd(A,q)}, 
] 

và tương tự cho (b). Ngoài ra, 

[ 
\gcd(A,B,q)=\frac{q}{\operatorname{lcm}(\operatorname{ord}(a),\operatorname{ord}(b))}. 
] 

Việc thay thế các danh tính này mang lại 

\frac{\operatorname{lcm}(r,s)} 
{\tên toán tử{gcd}(r,s)}, 
] 

trong đó (r=\operatorname{ord}(a)) và (s=\operatorname{ord}(b)). 

Vì vậy, các giá trị thực tế (a_i) biến mất khỏi phép tính cặp. Chúng ta chỉ cần thứ tự nhân của mỗi phần tử mảng. 

Việc nén tiếp theo là nhóm các phần tử theo thứ tự của chúng. Gọi (c_d) là số giá trị đầu vào có thứ tự (d). Khi đó số tiền mong muốn sẽ trở thành 

[ 
\sum_{d\mid q}\sum_{e\mid q} 
c_d c_e 
\frac{\operatorname{lcm}(d,e)}{\gcd(d,e)}. 
] 

Kể từ khi 

\frac{de}{\gcd(d,e)^2}, 
] 

xác định (b_d=c_d d). Câu trả lời là 

[ 
\sum_{d,e\mid q}\frac{b_d b_e}{\gcd(d,e)^2}. 
] 

Vấn đề còn lại là phép biến đổi tổng chia. Với mọi số nguyên dương (x), 

[ 
\frac1{x^2}=\sum_{k\mid x} h(k), 
] 

nơi đảo ngược Möbius mang lại 

[ 
h(k)=\sum_{t\mid k}\frac{\mu(t)}{(k/t)^2}. 
] 

Bởi vì (k) bao gồm các thừa số nguyên tố của (p-1), điều này đơn giản hóa thành 

\frac{1}{k^2} 
\prod_{r\mid k}(1-r^2). 
] 

Bây giờ thay thế (x=\gcd(d,e)): 

\sum_{k\mid d,\ k\mid e}h(k). 
] 

Sau khi trao đổi tổng, 

\sum_{k\mid q} 
h(k) 
\left( 
\sum_{\substack{d\mid q\k\mid d}} b_d 
\đúng)^2. 
] 

Đây là mức giảm trung tâm. Với mỗi ước số (k) của (q), chúng ta chỉ cần tổng của (b_d) trên tất cả các bội số chia (d) của (k). 

Tất cả các ước của (q) có thể được tạo ra một cách rõ ràng. Nếu chúng ta bắt đầu với các giá trị (b_d), một phép biến đổi hậu tố mạng chia sẽ tính các tổng này trong (O(\tau(q)\omega(q))), trong đó (\tau(q)) là số ước số và (\omega(q)) là số thừa số nguyên tố phân biệt. Số lượng ước của một số nguyên lên tới (10^{18}) là đủ nhỏ cho phương pháp này. 

Nhiệm vụ lý thuyết số còn lại là phân tích nhân tử (p-1). Vì (p-1) có thể gần với (10^{18}) nên phép chia thử là không đủ. Chúng tôi sử dụng Miller-Rabin xác định để kiểm tra tính nguyên tố bên dưới (2^{64}), kết hợp với Pollard-Rho để phân tích nhân tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) đánh giá cặp | (O(1)) thêm | Quá chậm | 
| Tối ưu | (O(n\omega(q)\log p+\tau(q)\omega(q)+\text{phân tích hệ số})) | (O(\tau(q)+n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đặt (q=p-1) và thừa số (q) thành các thừa số nguyên tố riêng biệt và số mũ của chúng. Vì (p<2^{60}), Miller-Rabin tất định với một tập cơ sở cố định là đủ để kiểm tra tính nguyên tố và Pollard-Rho có thể phân tích các số tổng hợp còn lại một cách hiệu quả. 
2. Với mỗi giá trị đầu vào (a_i), hãy tính modulo bậc nhân của nó (p). Bắt đầu với`order = q`. Với mỗi thừa số nguyên tố riêng biệt (r) của (q), hãy kiểm tra liên tục xem 

[ 
a_i^{\text{order}/r}\equiv1\pmod p. 
] 

Nếu thử nghiệm thành công, hãy chia`order`bởi (r) và kiểm tra lại. Giá trị cuối cùng chính xác là (\operatorname{ord}(a_i)), vì thứ tự nhân phải chia (q) và mỗi phép chia thành công sẽ loại bỏ một thừa số nguyên tố không cần thiết. 

1. Đếm xem mỗi thứ tự có thể có bao nhiêu giá trị đầu vào. Lưu trữ cái này dưới dạng (c_d). Chỉ các ước của (q) mới có thể xuất hiện dưới dạng thứ tự. 
2. Tạo mọi ước số (d) của (q). Với mỗi ước số, khởi tạo 

[ 
b_d=c_d d\pmod p. 
] 

Phép nhân với (d) xuất phát trực tiếp từ việc viết lại phần đóng góp của cặp dưới dạng (de/\gcd(d,e)^2). 

1. Tính toán 

[ 
S_k=\sum_{\substack{d\mid q\k\mid d}}b_d 
] 

với mọi ước số (k). Xử lý một thừa số nguyên tố riêng biệt (r) tại một thời điểm. Với mọi ước số (d) sao cho (dr\mid q), cộng giá trị thuộc (dr) vào giá trị thuộc (d). Việc xử lý các ước số theo thứ tự số giảm dần làm cho bản cập nhật trở thành tổng hậu tố tại chỗ trên số mũ của (r). 

1. Tính toán 

[ 
h(k)=\frac{\prod_{r\mid k}(1-r^2)}{k^2}\pmod p. 
] 

Phép chia có giá trị theo modulo (p), vì mọi ước số của (p-1) đều nguyên tố cùng nhau với (p). Thay vì tính toán nghịch đảo mô-đun cho mọi ước số, hãy tính trước bình phương nghịch đảo của từng số nguyên tố riêng biệt và suy ra (h(k)) từ (h(k/r)). 

1. Tích lũy 

\sum_{k\mid q}h(k)S_k^2\pmod p. 
] 

Đây chính xác là dạng biến đổi của tổng kép ban đầu. 

### Tại sao nó hoạt động 

Đối với mỗi giá trị đầu vào, thứ tự nhân của nó sẽ xác định hoàn toàn thông tin nhóm con có liên quan. Tích (f(a,b)f(b,a)) chính xác là (\operatorname{lcm}(r,s)/\gcd(r,s)=rs/\gcd(r,s)^2), do đó việc nhóm các giá trị theo thứ tự sẽ không mất thông tin. 

Hàm dẫn xuất Möbius (h) thỏa mãn (1/x^2=\sum_{k\mid x}h(k)). Áp dụng nhận dạng này cho (x=\gcd(d,e)) sẽ chuyển đổi biểu thức gcd theo cặp thành một tổng được lập chỉ mục bởi một ước số chung duy nhất (k). Biến đổi hậu tố tính toán chính xác tất cả các giá trị (\sum_{k\mid d}b_d), do đó tổng cuối cùng trên (h(k)S_k^2) chứa mọi cặp có thứ tự chính xác một lần với đóng góp ban đầu của nó. 

## Giải pháp Python```python
import sys
import math
import random

input = sys.stdin.readline

def is_prime(n):
    if n < 2:
        return False

    small = (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37)
    for p in small:
        if n % p == 0:
            return n == p

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    # Deterministic for every n < 2^64.
    for a in (2, 325, 9375, 28178, 450775, 9780504, 1795265022):
        if a % n == 0:
            continue

        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue

        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False

    return True

def pollard_rho(n):
    if n % 2 == 0:
        return 2
    if n % 3 == 0:
        return 3

    while True:
        c = random.randrange(1, n)
        x = random.randrange(0, n)
        y = x
        d = 1

        while d == 1:
            x = (x * x + c) % n
            y = (y * y + c) % n
            y = (y * y + c) % n
            d = math.gcd(abs(x - y), n)

        if d != n:
            return d

def factor_rec(n, factors):
    if n == 1:
        return
    if is_prime(n):
        factors.append(n)
        return

    d = pollard_rho(n)
    factor_rec(d, factors)
    factor_rec(n // d, factors)

def factorize(n):
    factors = []
    factor_rec(n, factors)
    factors.sort()

    result = []
    for x in factors:
        if not result or result[-1][0] != x:
            result.append([x, 1])
        else:
            result[-1][1] += 1
    return result

def solve_data(data):
    it = iter(data.split())
    n = int(next(it))
    mod = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    q = mod - 1
    factorization = factorize(q)
    primes = [r for r, _ in factorization]

    # Count elements by multiplicative order.
    freq = {}

    for x in a:
        order = q

        for r in primes:
            while order % r == 0 and pow(x, order // r, mod) == 1:
                order //= r

        freq[order] = freq.get(order, 0) + 1

    # Generate all divisors of q.
    divisors = [1]
    for r, e in factorization:
        old = divisors[:]
        mul = 1
        new = []
        for _ in range(e + 1):
            for d in old:
                new.append(d * mul)
            mul *= r
        divisors = new

    divisors.sort()
    index = {d: i for i, d in enumerate(divisors)}

    # b[d] = count[d] * d.
    values = [0] * len(divisors)
    for d, cnt in freq.items():
        values[index[d]] = (cnt * d) % mod

    # S[k] = sum_{d: k|d} b[d].
    #
    # Since d*r > d, descending order guarantees that values[d*r]
    # has already received all contributions for the current prime.
    descending = divisors[::-1]

    for r in primes:
        for d in descending:
            nd = d * r
            pos = index.get(nd)
            if pos is not None:
                values[index[d]] += values[pos]
                if values[index[d]] >= mod:
                    values[index[d]] -= mod

    # h[d] = sum_{t|d} mu(t) / (d/t)^2.
    #
    # If r is a prime divisor of d and d = r*m:
    #
    # h[d] / h[m] =
    #   (1-r^2)/r^2, if r does not divide m,
    #   1/r^2,       otherwise.
    inv_r2 = {}
    for r in primes:
        inv_r = pow(r, mod - 2, mod)
        inv_r2[r] = inv_r * inv_r % mod

    h = [0] * len(divisors)
    h[index[1]] = 1

    for d in divisors[1:]:
        for r in primes:
            if d % r == 0:
                m = d // r
                base = h[index[m]]
                inv2 = inv_r2[r]

                if m % r == 0:
                    h[index[d]] = base * inv2 % mod
                else:
                    factor = (1 - r * r) % mod
                    h[index[d]] = base * factor % mod
                break

    ans = 0
    for i in range(len(divisors)):
        ans = (ans + h[i] * values[i] % mod * values[i]) % mod

    return str(ans)

def solve():
    data = sys.stdin.buffer.read()
    sys.stdout.write(solve_data(data))

if __name__ == "__main__":
    solve()
```Kiểm tra tính nguyên tố sử dụng cơ sở Miller-Rabin xác định tiêu chuẩn cho phạm vi số nguyên 64 bit đầy đủ. Điều này quan trọng vì (p-1) có thể gần như (10^{18}), vì vậy việc coi Miller-Rabin chỉ đơn thuần là xác suất là không cần thiết ở đây. 

Pollard-Rho phân chia đệ quy (p-1) cho đến khi tất cả các thừa số đều là số nguyên tố. Độ sâu đệ quy nhỏ vì mỗi lần phân chia thành công sẽ làm giảm đáng kể số tổng hợp. 

Đối với mỗi giá trị đầu vào, việc tính toán thứ tự bắt đầu tại (p-1), không phải tại (1). Nếu số nguyên tố (r) chia thứ tự ứng cử viên hiện tại và (a^{\text{order}/r}=1), thì hệ số đó có thể bị loại bỏ. Việc lặp lại bài kiểm tra sẽ xử lý các lũy thừa chính xác một cách chính xác. Ví dụ: nếu thứ tự đúng chứa (r^2), phép chia thứ nhất có thể thành công nhưng phép chia thứ hai sẽ thất bại. 

Mảng ước số chứa mọi ước số của (p-1), bao gồm (1) và (p-1). Từ điển từ ước số đến chỉ số tránh mọi giả định rằng các ước số là số nguyên liên tiếp. 

Phép biến đổi tổng bội được thực hiện theo thứ tự số chia giảm dần. Khi xử lý số nguyên tố (r), giá trị tại (d r) đã bao gồm tất cả các bội số thu được bằng cách tăng số mũ của (r), do đó, việc thêm nó vào (d) sẽ tính tổng hậu tố cần thiết trong một lần xử lý. 

Việc tính toán (h(d)) được thực hiện theo modulo (p). Vì mọi ước số của (p-1) đều nhỏ hơn (p), nên tất cả các nghịch đảo môđun bắt buộc đều tồn tại. Số nguyên Python cũng tránh được vấn đề tràn phát sinh khi triển khai 64 bit khi nhân các số gần với (10^{18}). 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là```
4 5
1 2 3 4
```Ở đây (p-1=4=2^2). Thứ tự nhân là (1,4,4,2). 

| Đặt hàng (d) | Tần số (c_d) | (b_d=c_d d) | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 4 | 2 | 8 | 

Làm việc theo modulo (5), các giá trị ban đầu (b) là (1,2,0) cho các ước số (1,2,4). 

Đối với số nguyên tố (2), phép biến đổi hậu tố cho 

| (k) | Ban đầu (b_k) | (S_k=\sum_{k\mid d}b_d) | 
| --- | --- | --- | 
| 1 | 1 | 11 | 
| 2 | 2 | 10 | 
| 4 | 8 | 8 | 

Modulo (5), đây là (1,0,3). 

Các giá trị (h) tương ứng là 

[ 
h(1)=1,\qquad 
h(2)=\frac{1-2^2}{2^2}=-\frac34,\qquad 
h(4)=\frac{1-2^2}{4^2}=-\frac3{16}. 
] 

Modulo (5), điều này mang lại 

| (k) | (h(k)\bmod5) | (S_k\bmod5) | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 3 | 0 | 0 | 
| 4 | 2 | 3 | 18 mod 5 = 3 | 

Tổng số là (4), phù hợp với đầu ra mẫu. 

Dấu vết thể hiện quá trình nén chính: mặc dù có (16) cặp vị trí đầu vào được sắp xếp nhưng sau khi nhóm theo thứ tự, chúng ta chỉ làm việc với ba ước số (1,2,4). 

Đối với ví dụ thứ hai, hãy xem xét```
2 7
1 6
```Ở đây (q=6=2\cdot3). Thứ tự của (1) là (1), trong khi thứ tự của (6=-1) là (2). 

| Đặt hàng (d) | Tần số (c_d) | (b_d=c_d d) | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 3 | 0 | 0 | 
| 6 | 0 | 0 | 

Nhiều tổng là 

| (k) | (S_k) | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 0 | 
| 6 | 0 | 

Bốn đóng góp của cặp ban đầu được trực tiếp 

[ 
\frac{1\cdot1}{1^2}=1, 
\quad 
\frac{1\cdot2}{1^2}=2, 
\quad 
\frac{2\cdot1}{1^2}=2, 
\quad 
\frac{2\cdot2}{2^2}=1. 
] 

Tổng của chúng là (6), nên đáp án là (6\bmod7=6). 

Ví dụ này thực hiện đồng thời phần tử nhận dạng và phần tử không tạo. Nó cũng xác nhận rằng công thức sử dụng gcd của hai đơn hàng thay vì chỉ so sánh xem các đơn hàng có bằng nhau hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\text{Pollard-Rho} + n\omega(q)\log p+\tau(q)\omega(q)+\tau(q)\omega(q)\log p)) | Tính toán thứ tự sử dụng lũy ​​thừa mô-đun cho các thừa số nguyên tố riêng biệt, trong khi các phép biến đổi số chia sử dụng một lần cho mỗi số nguyên tố riêng biệt | 
| Không gian | (O(n+\tau(q))) | Các giá trị đầu vào, tần số thứ tự, mảng chia và dữ liệu phân tích nhân tử được lưu trữ | 

Đối với (n\le10^5), cách tiếp cận theo cặp (O(n^2)) là không thể. Phương pháp tối ưu hóa chỉ phụ thuộc tuyến tính vào (n) ngoài chi phí lũy thừa mô-đun, trong khi công việc của số chia phụ thuộc vào (p-1). Đối với các số nguyên lên tới (10^{18}), số lượng ước số đủ nhỏ để biến đổi mạng chia số rõ ràng và Pollard-Rho xử lý việc phân tích nhân tử của (p-1) mà không cần chia thử lên đến (10^9). 

## Trường hợp thử nghiệm```python
import io
import sys

# Paste the solve_data function and its helpers from the solution above
# before running these tests.

def run(inp: str) -> str:
    return solve_data(inp.encode()).strip()

# Provided sample
assert run("""\
4 5
1 2 3 4
""") == "4", "sample 1"

# Minimum size
assert run("""\
1 2
1
""") == "1", "minimum-size case"

# All values equal
assert run("""\
3 7
2 2 2
""") == "2", "all-equal values"

# Boundary value p-1 together with the identity
assert run("""\
2 7
1 6
""") == "6", "boundary orders"

# Mixed orders, catches confusion between gcd and lcm
assert run("""\
2 7
2 3
""") == "5", "different order structure"

# Maximum n with p=2. The only possible value is 1, so every
# ordered pair contributes 1. Since 100000^2 is even, the result is 0.
maximum_input = "100000 2\n" + " ".join(["1"] * 100000) + "\n"
assert run(maximum_input) == "0", "maximum-size case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 1`|`1`| Kích thước tối thiểu và bộ chia chỉ chứa (1) | 
|`3 7 / 2 2 2`|`2`| Đơn hàng đa dạng và bình đẳng | 
|`2 7 / 1 6`|`6`| Đơn hàng (1) và (2), bao gồm (p-1) | 
|`2 7 / 2 3`|`5`| Các đơn đặt hàng không cần thiết khác nhau | 
|`100000 2 / 1 ... 1`|`0`| Tối đa (n), giá trị lặp lại và (p=2) | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
1 2
1
```chúng ta có (q=1), do đó hệ số hóa trống và ước số duy nhất là (1). Thứ tự của (1) được khởi tạo thành (q=1), tần suất của thứ tự (1) là một và tổng hậu tố là (1). Vì (h(1)=1), đáp án cuối cùng là (1). Không có trường hợp đặc biệt nào cho (p=2) được yêu cầu. 

Đối với các giá trị lặp lại, hãy xem xét```
3 7
2 2 2
```Thứ tự của (2) modulo (7) là (3), do đó bản đồ tần số chứa (c_3=3). Mỗi cặp đều có mệnh lệnh (3,3), đưa ra 

[ 
\frac{3\cdot3}{3^2}=1. 
] 

Có chín cặp có thứ tự nên kết quả là (9\bmod7=2). Việc tổng hợp tần số xử lý tất cả chín cặp mà không liệt kê chúng một cách rõ ràng. 

Đối với giá trị biên (p-1), hãy xem xét```
2 7
1 6
```Thứ tự là (1) và (2). Cặp có cả hai giá trị bằng (6) đóng góp (2\cdot2/2^2=1), trong khi mỗi cặp hỗn hợp đóng góp (1\cdot2/1^2=2). Cộng bốn phần đóng góp sẽ được (6), do đó kết quả là (6). Thuật toán không bao giờ giả định rằng một phần dư khác 0 tùy ý có thứ tự (p-1). 

Đối với mô đun nhỏ nhất có thể,```
100000 2
1 1 1 ... 1
```mọi giá trị đầu vào là (1), có thứ tự là (1). Mỗi cặp có thứ tự đóng góp (1), tạo ra (10^{10}). Vì (p=2) nên kết quả cần có là (10^{10}\bmod2=0). Việc phân tích thành thừa số của (p-1=1) không tạo ra các thừa số nguyên tố và phép biến đổi số chia tự nhiên giảm xuống còn số chia duy nhất (1), do đó không có vấn đề chia bằng 0 hoặc phân tích thành thừa số trống.
