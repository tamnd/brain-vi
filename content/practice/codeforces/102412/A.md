---
title: "CF 102412A - Người đa thức"
description: "Chúng ta có một mô đun nguyên tố (p), một tập hợp con (S) của dư lượng modulo (p) và một tập hợp con khác (V). Đối với mọi cặp có thứ tự ((a,b)) có cả hai giá trị trong (S), chúng ta đánh giá [ F(a,b)= frac{(2a+3b)^2+5a^2}{(3a+b)^2} + frac{(2a+5b)^2+3b^2}{(3a+2b)^2} pmod p."
date: "2026-08-11T08:27:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "A"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 500
verified: true
draft: false
---

[CF 102412A - Người đa thức duy nhất](https://codeforces.com/problemset/problem/102412/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mô đun nguyên tố (p), một tập hợp con (S) của dư lượng modulo (p) và một tập hợp con khác (V). Với mỗi cặp có thứ tự ((a,b)) có cả hai giá trị trong (S), chúng ta đánh giá 

[ 
F(a,b)= 
\frac{(2a+3b)^2+5a^2}{(3a+b)^2} 
+ 
\frac{(2a+5b)^2+3b^2}{(3a+2b)^2} 
\pmod p. 
] 

Cặp được tính chính xác khi mọi mẫu số xuất hiện đều khác 0 và (F(a,b)) thuộc về (V). Tích trong công thức ban đầu chỉ là một cách nói ngắn gọn rằng ít nhất một thừa số (F(a,b)-z) bằng 0 đối với một số (z\in V). 

Số nguyên tố (p) có thể lớn bằng (10^6), trong khi (S) và (V) mỗi số có thể chứa tất cả (p) dư lượng. Do đó, việc quét trực tiếp trên tất cả các cặp có thứ tự có thể thực hiện kiểm tra cặp khoảng (p^2) hoặc tối đa (10^{12}). Giới hạn bốn giây loại bỏ hoàn toàn điều đó. Mục tiêu hữu ích là khoảng (O(p\log p)), là tỷ lệ của FFT hoặc NTT trên một mảng có kích thước tỷ lệ với (p). Giải pháp cuộc thi ban đầu sử dụng chính xác mức giảm này. 

Có ba trường hợp đặc biệt cần được xử lý riêng biệt. 

Đầu tiên, cặp ((0,0)) không bao giờ được tính. Ví dụ,```
2
1
0
1
0
```chỉ có cặp ((0,0)), nhưng cả hai mẫu số đều bằng 0, vì vậy câu trả lời là`0`. Việc triển khai bất cẩn thay thế (a/b) mà không loại bỏ các giá trị 0 trước tiên sẽ không có tỷ lệ hợp lệ cho cặp này và có thể vô tình coi nó là một tỷ lệ đặc biệt. 

Thứ hai, các cặp có đúng một số 0 có mẫu số hoàn toàn hợp lệ và không được loại bỏ. Với (a=0,b\ne0), biểu thức là (16). Với (a\ne0,b=0), đó là (13/9). Ví dụ,```
7
2
0 1
2
2 3
```chứa cặp ((0,1)), có giá trị là (16\equiv2\pmod7) và cặp ((1,0)), có giá trị là (13/9\equiv3\pmod7). Cặp ((1,1)) có giá trị (0), không nằm trong (V) và ((0,0)) không hợp lệ. Đầu ra đúng là`2`. Việc triển khai chỉ đơn giản loại bỏ số 0 khỏi (S) sẽ làm mất hai cặp thứ tự hợp lệ này. 

Thứ ba, ngay cả đối với (a,b) khác 0, một tỷ lệ có thể làm cho mẫu số biến mất. Ví dụ: với (p=7), (3a+b=0) có nghĩa là (a/b=2), trong khi (3a+2b=0) có nghĩa là (a/b=4). Như vậy đối với```
7
2
1 2
1
0
```các tỷ số (1) và (1) từ các cặp ((1,1)) và ((2,2)) là hợp lệ và cho giá trị (0), trong khi các cặp ((1,2)) và ((2,1)) có tỷ lệ (4) và (2), do đó mẫu số bằng 0. Đầu ra đúng là`2`. Chỉ cần đánh giá biểu thức hữu tỉ sau khi đảo ngược mô-đun mà không kiểm tra mẫu số của nó có thể tạo ra một giá trị không liên quan. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Với mọi (a\in S) và mọi (b\in S), hãy kiểm tra hai mẫu số, tính hai phân số mô đun, thu được (F(a,b)) và kiểm tra xem giá trị đó có thuộc (V) hay không. Điều này đúng vì mỗi cặp có thứ tự đều được kiểm tra đúng một lần. Vấn đề là số lượng cặp (p^2). Tại (p=10^6), có thể có (10^{12}) cặp, thậm chí trước khi tính đến nghịch đảo mô-đun hoặc kiểm tra thành viên. Điều đó vượt xa những gì thời hạn cho phép. 

Cấu trúc cứu chúng ta là sự đồng nhất. Mọi tử số và mẫu số trong biểu thức đều có bậc hai trong (a,b). Do đó, khi (a\ne0) và (b\ne0), nhân cả hai biến với cùng một giá trị khác 0 sẽ không làm thay đổi kết quả. Giá trị chỉ phụ thuộc vào tỷ lệ 

[ 
t=\frac{a}{b}\pmod p. 
] 

Quan sát này làm giảm phần đại số của tất cả các cặp (p^2) xuống chỉ còn (p-1) các tỷ lệ khác 0 có thể. Giải pháp dự định trước tiên sẽ đánh giá biểu thức cho mọi tỷ lệ như vậy và ghi lại tỷ lệ nào tạo ra giá trị thuộc về (V). 

Khi đó chúng ta có thể quên đi hàm hữu tỷ phức tạp. Bài toán còn lại là đếm các cặp (a,b\in S) thỏa mãn 

[ 
a b^{-1}\in L, 
] 

trong đó (L) là tập hợp các tỷ lệ được chấp nhận. 

Phép tích chập bình thường xử lý phép cộng chứ không phải phép nhân. Tuy nhiên, nhóm nhân của các số dư khác 0 modulo một số nguyên tố có tính tuần hoàn. Chọn một gốc nguyên thủy (g). Mọi phần dư khác 0 có thể được viết duy nhất dưới dạng (g^x), trong đó (0\le x<p-1). Nếu (a=g^x) và (b=g^y), thì 

[ 
\frac ab=g^{x-y}. 
] 

Tỉ số nhân đã trở thành hiệu của số mũ. Chúng ta có thể đếm những khác biệt này bằng phép tích chập tuần hoàn. Một công thức thuận tiện là kết hợp chỉ báo (S) với chỉ báo (L). Tại số mũ (x), phép tích chập đếm các lựa chọn của (y) và (r) với (y+r=x), tương ứng chính xác với (r=x-y), do đó thành (a/b). Đây là phép biến đổi trung tâm được mô tả bởi nghiệm chuẩn. 

Vì hệ số chập tối đa là (p), nên chúng ta có thể sử dụng mô đun NTT (998244353) mà không có sự mơ hồ. Hệ số tích chập lớn nhất tối đa là (10^6), nhỏ hơn nhiều so với mô đun NTT. Chúng tôi sử dụng độ dài NTT là lũy thừa tiếp theo của hai ở trên (2(p-1)-1), tối đa là (2^{21}). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(p^2\log p)) với đảo ngược mô-đun trực tiếp | (O(p)) | Quá chậm | 
| Tỷ lệ giảm + NTT | (O(p\log p)) | (O(p)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (p), (S) và (V) và xây dựng các mảng thành viên có thời gian không đổi cho cả hai tập hợp. Chúng ta cũng tách số 0 khỏi các phần tử khác 0 của (S), vì tỷ số (a/b) chỉ tồn tại khi cả hai giá trị đều khác 0. 
2. Xử lý các cặp có đúng một số 0. Nếu (0\in S), mọi khác 0 (x\in S) tạo ra hai cặp có thứ tự, ((0,x)) và ((x,0)). Cái đầu tiên có giá trị (16), trong khi cái thứ hai có giá trị (13/9). Thêm số lượng tương ứng bất cứ khi nào các giá trị đó thuộc về (V). Cặp ((0,0)) luôn không hợp lệ. 
3. Tìm nghiệm nguyên thủy (g) modulo (p). Thừa số (p-1), sau đó kiểm tra các ứng cử viên (g) cho đến khi (g^{(p-1)/q}\ne1\pmod p) cho mọi thừa số nguyên tố riêng biệt (q) của (p-1). Điều này đảm bảo rằng lũy ​​thừa của (g) liệt kê mọi số dư khác 0 chính xác một lần. 
4. Xây dựng mảng logarit rời rạc. Bắt đầu với (x=1), nhân liên tục với (g). Tại số mũ (e), ghi lại số dư hiện tại có logarit (e). Sau (p-1) lần lặp, mọi số dư khác 0 đều có số mũ đã biết. 
5. Tính toán trước nghịch đảo mô-đun của tất cả các thặng dư khác 0 bằng cách sử dụng phép truy hồi tuyến tính tiêu chuẩn 

p-\left\lfloor\frac pi\right\rfloor 
\operatorname{inv[p\bmod i]\pmod p. 
] 

Điều này tránh việc thực hiện phép lũy thừa (O(\log p)) riêng biệt cho mọi tỷ lệ. 

1. Liệt kê mọi tỉ số khác 0 (t). Mẫu số là 

[ 
(3t+1)^2 
\quad\text{và}\quad 
(3t+2)^2. 
]

Nếu (3t+1) hoặc (3t+2) bằng 0 modulo (p), hãy bỏ qua tỷ lệ này vì biểu thức ban đầu cấm chia cho 0 một cách rõ ràng. 

1. Với mọi (t) còn lại, thay thế (a=t,b=1). Giá trị kết quả là 

[ 
G(t)= 
\frac{(2t+3)^2+5t^2}{(3t+1)^2} 
+ 
\frac{(2t+5)^2+3}{(3t+2)^2}. 
] 

Nếu (G(t)\in V), đánh dấu logarit rời rạc của (t) trong mảng tích chập thứ hai. 

1. Xây dựng mảng tích chập đầu tiên từ (S). Nếu (x\in S) và (x\ne0), hãy đặt một số ở chỉ mục (\log_g x). Xây dựng mảng thứ hai từ các tỷ lệ được chấp nhận, đặt một tỷ lệ ở chỉ mục (\log_g t). 
2. Tính tích chập thông thường của chúng với NTT. Nếu mảng số mũ chứa (y) và (r), hệ số tại (y+r) sẽ tính một cặp thỏa mãn 

[ 
\log_g(a)+\log_g(t)=\log_g(a), 
] 

sau khi diễn giải số mũ đầu tiên là số mũ của (b). Trực tiếp hơn, đối với số mũ tử số mong muốn (x), đẳng thức (y+r=x) chính xác là (r=x-y), có nghĩa là (t=g^{x-y}=a/b). 

1. Gấp nửa sau của phần tích chập lại (p-1). Số mũ sống theo modulo (p-1), do đó hệ số (k+(p-1)) đại diện cho số mũ nhóm nhân giống như hệ số (k). 
2. Với mọi số khác 0 (a\in S), cộng hệ số tích chập tuần hoàn tại (\log_g a). Hệ số đó tính chính xác các lựa chọn của (b\in S) mà (a/b) là tỷ lệ được chấp nhận. Thêm phần đóng góp không có trường hợp nào từ bước 2 để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Với mọi cặp khác 0 ((a,b)), hãy viết (a=g^x) và (b=g^y). Biểu thức đồng nhất bậc 0 nên giá trị của nó chỉ phụ thuộc vào (a/b=g^{x-y}). Quá trình xử lý trước tỷ lệ đánh dấu chính xác các số mũ (r=x-y) mà biểu thức được xác định và thuộc về (V). Tích chập tuần hoàn đếm chính xác các cặp số mũ thỏa mãn (x=y+r), tương đương với (r=x-y). Do đó, mọi cặp có thứ tự khác 0 hợp lệ đều đóng góp một lần và không có cặp không hợp lệ nào đóng góp. Các trường hợp 0 ​​được xử lý riêng biệt bao gồm mọi cặp mà biểu diễn tỷ lệ không có sẵn. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

NTT_MOD = 998244353
NTT_ROOT = 3

def factorize(n):
    factors = []
    d = 2
    while d * d <= n:
        if n % d == 0:
            factors.append(d)
            while n % d == 0:
                n //= d
        d += 1 if d == 2 else 2
    if n > 1:
        factors.append(n)
    return factors

def primitive_root(p):
    if p == 2:
        return 1

    factors = factorize(p - 1)
    for g in range(2, p):
        ok = True
        for q in factors:
            if pow(g, (p - 1) // q, p) == 1:
                ok = False
                break
        if ok:
            return g
    return -1

def ntt(a, invert, rev):
    n = len(a)

    for i in range(n):
        j = rev[i]
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        wlen = pow(
            NTT_ROOT,
            (NTT_MOD - 1) // length,
            NTT_MOD
        )
        if invert:
            wlen = pow(wlen, NTT_MOD - 2, NTT_MOD)

        half = length >> 1

        for start in range(0, n, length):
            w = 1
            end = start + half
            j = start

            while j < end:
                u = a[j]
                v = a[j + half] * w % NTT_MOD

                x = u + v
                if x >= NTT_MOD:
                    x -= NTT_MOD

                y = u - v
                if y < 0:
                    y += NTT_MOD

                a[j] = x
                a[j + half] = y

                w = w * wlen % NTT_MOD
                j += 1

        length <<= 1

    if invert:
        inv_n = pow(n, NTT_MOD - 2, NTT_MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % NTT_MOD

def convolution(a, b):
    need = len(a) + len(b) - 1
    n = 1
    while n < need:
        n <<= 1

    fa = a + [0] * (n - len(a))
    fb = b + [0] * (n - len(b))

    rev = array('I', [0]) * n
    half = n >> 1
    for i in range(1, n):
        rev[i] = (rev[i >> 1] >> 1) | ((i & 1) * half)

    ntt(fa, False, rev)
    ntt(fb, False, rev)

    for i in range(n):
        fa[i] = fa[i] * fb[i] % NTT_MOD

    del fb

    ntt(fa, True, rev)
    return fa[:need]

def solve():
    p = int(input())

    n = int(input())
    s = list(map(int, input().split())) if n else []

    m = int(input())
    v = list(map(int, input().split())) if m else []

    in_s = bytearray(p)
    for x in s:
        in_s[x] = 1

    in_v = bytearray(p)
    for x in v:
        in_v[x] = 1

    ans = 0

    has_zero = in_s[0]

    # Handle (0, b), b != 0.
    # The value is 16.
    if has_zero and in_v[16 % p]:
        ans += n - 1

    # Handle (a, 0), a != 0.
    # The value is 13 / 9.
    # If 9 == 0 mod p, the denominator is zero and no such pair is valid.
    if has_zero and 9 % p != 0:
        value = 13 * pow(9, p - 2, p) % p
        if in_v[value]:
            ans += n - 1

    nonzero_count = n - (1 if has_zero else 0)

    if nonzero_count == 0 or m == 0:
        print(ans)
        return

    q = p - 1

    # Find a primitive root and construct discrete logarithms.
    g = primitive_root(p)

    log = array('i', [-1]) * p
    cur = 1
    for e in range(q):
        log[cur] = e
        cur = cur * g % p

    # Linear-time modular inverses.
    inv = array('I', [0]) * p
    if p > 1:
        inv[1] = 1
        for i in range(2, p):
            inv[i] = (
                p - (p // i) * inv[p % i] % p
            )

    # A[x] = 1 iff g^x is in S.
    a_poly = [0] * q
    for x in range(1, p):
        if in_s[x]:
            a_poly[log[x]] = 1

    # B[r] = 1 iff g^r is an accepted ratio.
    b_poly = [0] * q

    for t in range(1, p):
        d1 = (3 * t + 1) % p
        d2 = (3 * t + 2) % p

        if d1 == 0 or d2 == 0:
            continue

        u = (2 * t + 3) % p
        w = (2 * t + 5) % p

        num1 = (u * u + 5 * t * t) % p
        num2 = (w * w + 3) % p

        inv_d1 = inv[d1]
        inv_d2 = inv[d2]

        term1 = num1 * inv_d1 % p * inv_d1 % p
        term2 = num2 * inv_d2 % p * inv_d2 % p
        value = (term1 + term2) % p

        if in_v[value]:
            b_poly[log[t]] = 1

    c = convolution(a_poly, b_poly)

    # Convert ordinary convolution into cyclic convolution modulo p - 1.
    for i in range(q, len(c)):
        c[i - q] += c[i]

    # For a = g^x, c[x] counts b = g^y with
    # x = y + log(a / b).
    for x in range(1, p):
        if in_s[x]:
            ans += c[log[x]]

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc với`input = sys.stdin.readline`, theo yêu cầu. Các nhóm thành viên sử dụng`bytearray`, giúp duy trì dung lượng lưu trữ trên mỗi phần dư ở mức nhỏ ngay cả khi (p=10^6). 

Việc xử lý bằng 0 xảy ra trước khi bất kỳ logarit rời rạc nào được sử dụng. Giá trị của ((0,b)) là (16), trong khi giá trị của ((a,0)) là (13/9). Séc`9 % p != 0`là cần thiết vì (p=3) làm cho mẫu số của phân số thứ hai bằng 0 khi (b=0). 

Bảng nghịch đảo tránh thực hiện hai phép lũy thừa mô-đun cho mỗi tỷ lệ (p-1). Một lần`inv[d1]`Và`inv[d2]`đã biết, các mẫu số bình phương được nghịch đảo bằng cách nhân nghịch đảo hai lần. Vòng lặp tỷ lệ cũng kiểm tra mẫu số tuyến tính không bình phương trước, điều này hoàn toàn tương đương với việc kiểm tra xem mẫu số bình phương ban đầu có bằng 0 hay không. 

Mảng tích chập sử dụng các chỉ số số mũ từ`0`bởi vì`p-2`. Mảng đầu tiên biểu thị các phần tử khác 0 của (S), trong khi mảng thứ hai biểu thị các tỷ lệ được chấp nhận. Vì thứ tự nhóm là (p-1), số mũ quấn quanh nhau. Tích chập thông thường chứa các khoản tiền từ`0`bởi vì`2(p-2)`, do đó phần thứ hai được gấp lại bởi`p-1`. 

Mô-đun NTT (998244353) đủ lớn để các hệ số tích chập số nguyên chính xác không thể bao bọc mô-đun mô-đun NTT. Một hệ số đếm một tập hợp các cặp dư lượng, vì vậy nó lớn nhất là (p\le10^6). 

Việc triển khai Python sử dụng thuật toán tương tự như phương pháp C++ được chấp nhận, nhưng giới hạn cuộc thi 4 giây ban đầu được thiết kế dựa trên mã NTT được biên dịch được tối ưu hóa cao. Một NTT Python thuần túy có kích thước (2^{21}) có chi phí thông dịch đáng kể, do đó, đối với giới hạn thời gian ban đầu của Codeforce, C++ là ngôn ngữ triển khai thực tế. Toán học và số học số nguyên trong phiên bản Python là chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
7
4
0 4 5 6
2
2 3
```Các phần tử khác 0 là (4,5,6). Các trường hợp 0 ​​đóng góp hai cặp vì (16\equiv2\pmod7), do đó mỗi phần tử khác 0 tạo ra một cặp hợp lệ ((0,b)). Giá trị (13/9\equiv3\pmod7) cũng thuộc về (V), cho cùng số cặp hợp lệ theo hướng ngược lại. 

Đối với phần khác 0, tập hợp tỷ lệ được chấp nhận chứa các tỷ lệ có giá trị hữu tỷ là (2) hoặc (3). Sau khi chuyển đổi các số dư có liên quan thành số mũ gốc nguyên thủy, tích chập sẽ đếm các cặp có thứ tự khác 0 hợp lệ. 

Một dấu vết nhỏ gọn là: 

| Sân khấu | Tiểu bang | Đóng góp | 
| --- | --- | --- | 
| Đầu vào | (S={0,4,5,6}), (V={2,3}) | 0 | 
| Không có trường hợp | (F(0,b)=2), (F(a,0)=3) | 6 | 
| Tỷ lệ khác không | Kiểm tra (t=1,\ldots,6), loại trừ tỷ lệ mẫu số bằng 0 | 2 | 
| Cuối cùng | Tất cả các cặp đặt hàng hợp lệ | 8 | 

Đầu ra cuối cùng là```
8
```Dấu vết cho thấy tại sao không thể loại bỏ các giá trị 0 một cách đơn giản. Chúng chiếm phần lớn câu trả lời trong mẫu này. 

### Mẫu 2 

Đầu vào là```
19
10
0 3 4 5 8 9 13 14 15 18
10
2 3 5 9 10 11 12 13 14 15
```Có chín phần tử khác 0 trong (S). Thuật toán trước tiên xử lý hai hướng có thể có liên quan đến số 0. Sau đó, nó đánh giá biểu thức hữu tỉ cho mọi modulo tỷ lệ khác 0 (19), tỷ lệ bỏ qua (t) thỏa mãn (3t+1=0) hoặc (3t+2=0). 

Tập hợp tỷ lệ kết quả được chuyển đổi thành số mũ của căn nguyên thủy. Tích chập sau đó đếm xem có bao nhiêu cặp số mũ có thứ tự khác nhau theo từng tỷ lệ được chấp nhận. 

| Sân khấu | Trạng thái chính | Chạy câu trả lời | 
| --- | --- | --- | 
| Đầu vào | ( | S | =10, | V | =10) | 0 | 
| Không có trường hợp | (9) ứng viên khác 0 ở mỗi hướng | một phần | 
| Quét tỷ lệ | (18) tỷ lệ khác 0, với mẫu số không hợp lệ bị bỏ qua | một phần | 
| NTT | Tích chập theo modulo số mũ (18) | một phần | 
| Tổng cuối cùng | Thêm (c[\log_g a]) cho mỗi khác không (a\in S) | 42 | 

Đầu ra cuối cùng là```
42
```Phần quan trọng của ví dụ này là phép chập đếm các cặp có thứ tự. Không có phép chia cho hai, bởi vì ((a,b)) và ((b,a)) tương ứng với hiệu số mũ đối diện và là các cặp có thứ tự riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(p\log p)) | Quá trình tiền xử lý gốc nguyên thủy, đánh giá tỷ lệ và tích chập NTT đều nằm trong giới hạn này | 
| Không gian | (O(p)) | Mảng thành viên, logarit, bảng nghịch đảo và bộ đệm NTT là tuyến tính trong (p) | 

Trường hợp lớn nhất có (p=10^6). Quá trình quét tỷ lệ là tuyến tính, trong khi tích chập sử dụng NTT có kích thước tối đa (2^{21}), mang lại độ phức tạp dự kiến ​​(O(p\log p)). Đây là thang tiệm cận chính xác cho giới hạn 4 giây và 256 MiB ban đầu. Cách tiếp cận được chấp nhận ban đầu cũng sử dụng NTT với tập dư lượng có kích thước tuyến tính và báo cáo cùng một ý tưởng (O(p\log p)). 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """7
4
0 4 5 6
2
2 3
"""
) == "8\n", "sample 1"

# Sample 2
assert run(
    """19
10
0 3 4 5 8 9 13 14 15 18
10
2 3 5 9 10 11 12 13 14 15
"""
) == "42\n", "sample 2"

# Minimum-size input.
# Only (0, 0) exists, and its denominators are zero.
assert run(
    """2
1
0
1
0
"""
) == "0\n", "minimum size and invalid zero pair"

# All-equal nonzero values.
# For p = 7 and a = b = 1, the expression is 0.
assert run(
    """7
1
1
1
0
"""
) == "1\n", "all-equal nonzero values"

# Boundary denominator cases.
# For p = 7, ratios 2 and 4 make one denominator zero.
# S = {1, 2}, V = {0}; only (1,1) and (2,2) are valid.
assert run(
    """7
2
1 2
1
0
"""
) == "2\n", "denominator-zero ratios"

# Maximum-size style case.
# S and V contain every residue modulo 101.
# Every pair with valid denominators is accepted.
# The two invalid denominator lines contain 101 pairs each,
# and intersect only at (0,0), so 101^2 - (2*101 - 1) = 10000.
p = 101
all_values = " ".join(map(str, range(p)))
assert run(
    f"""{p}
{p}
{all_values}
{p}
{all_values}
"""
) == "10000\n", "full sets and large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`p=2, S={0}, V={0}`|`0`| Đầu vào có kích thước tối thiểu và lỗi mẫu số ((0,0)) | 
|`p=7, S={1}, V={0}`|`1`| Các giá trị khác 0 hoàn toàn bằng nhau và tỷ lệ (a/b=1) | 
|`p=7, S={1,2}, V={0}`|`2`| Tỷ lệ mẫu số bằng 0 (2) và (4) | 
|`p=101, S=V=\{0,\ldots,100\}`|`10000`| Kích thước bộ lớn và bao phủ hoàn toàn cặn | 

## Vỏ cạnh 

Đối với trường hợp ((0,0)), lấy```
2
1
0
1
0
```Thuật toán thấy rằng số 0 thuộc về (S), nhưng không có phần tử nào khác 0, do đó phần đóng góp của trường hợp 0 ​​bằng 0. Nó ngay lập tức kết thúc mà không xây dựng một phép tích chập tỷ lệ có ý nghĩa. Điều này đúng vì cả hai mẫu số ban đầu đều bằng 0. 

Đối với một cặp ((0,b)) với (b\ne0), phân số thứ nhất trở thành 

[ 
\frac{9b^2}{b^2}=9, 
] 

và thứ hai trở thành 

[ 
\frac{28b^2}{(2b)^2}=7, 
] 

vậy tổng số là (16). Với (p=7), đây là (2). Thuật toán thêm một cặp như vậy cho mọi giá trị khác 0 (b\in S) khi (2\in V). 

Đối với một cặp ((a,0)) với (a\ne0), phân số thứ nhất là (1), trong khi phân số thứ hai là (4/9), cho ra (13/9). Thuật toán sẽ kiểm tra (9\ne0\pmod p) trước khi sử dụng giá trị này. Việc kiểm tra này quan trọng ở (p=3), trong đó mẫu số sẽ bằng 0. 

Đối với tỷ lệ mẫu số bằng 0, giả sử (p=7) và (t=a/b=2). Sau đó 

[ 
3t+1=7\equiv0\pmod7. 
] 

Thuật toán loại bỏ tỷ lệ này trước khi tính toán nghịch đảo. Tương tự (t=4) cho (3t+2=14\equiv0\pmod7). Điều này khớp chính xác với quy tắc chia ban đầu thay vì coi biểu thức hữu tỷ có mẫu số không xác định dưới dạng một giá trị mô-đun nào đó. 

Đối với trường hợp toàn dư có (p=101), (S=V=\mathbb F_{101}). Mỗi cặp có mẫu số khác 0 đều có một số giá trị trong (V), do đó chỉ có mẫu số bị loại trừ. Mỗi phương trình (3a+b=0) và (3a+2b=0) mô tả các cặp (101) và chúng chỉ giao nhau tại ((0,0)). Do đó có (2\cdot101-1=201) cặp không hợp lệ, để lại 

[ 
101^2-201=10000. 
] 

Phép tích chập xử lý tất cả các cặp khác 0, trong khi phép xử lý số 0 rõ ràng xử lý các cặp hợp lệ còn lại mà không bao giờ gán logarit rời rạc cho 0.
