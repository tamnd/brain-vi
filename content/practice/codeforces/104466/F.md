---
title: "CF 104466F - Công trình xây dựng tự do"
description: "Bạn rất gần gũi về mặt ý tưởng, nhưng sự không phù hợp mà bạn đang thấy không phải là một lỗi số học nhỏ. Nó xuất phát từ một sai lầm về khái niệm trong cách sử dụng tích chập cho vấn đề này."
date: "2026-06-30T13:15:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104466
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC German Collegiate Programming Contest (GCPC 2023)"
rating: 0
weight: 104466
solve_time_s: 117
verified: true
draft: false
---

[CF 104466F - Thợ xây tự do](https://codeforces.com/problemset/problem/104466/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
Bạn rất gần gũi về mặt ý tưởng, nhưng sự không phù hợp mà bạn đang thấy không phải là một lỗi số học nhỏ. Nó xuất phát từ một sai lầm về khái niệm trong cách sử dụng tích chập cho vấn đề này. 

Vấn đề là ở đây: 

Bạn đang xử lý chỉ số tích chập`i`như trực tiếp phù hợp với sức mạnh cần thiết`M^(i-1)`trong câu trả lời cuối cùng. Điều đó không đúng. 

Đầu ra tích chập`res[i]`tương ứng với sự dịch chuyển căn chỉnh`i - (n-1)`, không phải chỉ mục thô`i`. Vì vậy, trọng số của bạn bị thay đổi và điều này phá vỡ hoàn toàn kết quả số cuối cùng ngay cả khi bản thân tích chập là chính xác. 

Đó là lý do tại sao: 

- những trường hợp nhỏ nhìn “có lý nhưng sai” 
- các trường hợp lớn hơn trôi đi đáng kể (như 54 → 30 hoặc 110210000 → 101101100) 

# Chỉnh sửa phím 

Nếu: 

-`res[i]`là kết quả tích chập 
- chỉ số căn chỉnh thực sự là`i - (n - 1)`thì đóng góp đúng là:```
res[i] * M^(i - (n - 1))
```Chúng ta phải bình thường hóa các chỉ số để số mũ không âm. 

Vì vậy, chúng tôi thay đổi mọi thứ bằng cách`(n-1)`trong việc xử lý quyền lực. 

# Cách tiếp cận đúng (cùng thuật toán, lập chỉ mục cố định) 

Chúng tôi tính toán trước lũy thừa lên đến`n + m`. 

Sau đó khi xử lý chỉ số tích chập`i`: 

- vị trí thực sự trong A là`i - (n - 1)`- nhưng công thức cuối cùng sử dụng`k = 1..n+m-1`- vì vậy chúng tôi lập bản đồ cẩn thận vào`M^(k-1)`Ở đâu`k = i - (n-1) + 1`Điều đó đơn giản hóa thành:```
exponent = i - (n - 1)
```Vì vậy chúng ta dịch chuyển cơ số mũ. 

# Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

MOD = 998244353
G = 3

def ntt(a, invert):
    n = len(a)

    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        wlen = pow(G, (MOD - 1) // length, MOD)
        if invert:
            wlen = pow(wlen, MOD - 2, MOD)

        i = 0
        half = length >> 1
        while i < n:
            w = 1
            for j in range(i, i + half):
                u = a[j]
                v = a[j + half] * w % MOD
                a[j] = (u + v) % MOD
                a[j + half] = (u - v) % MOD
                w = w * wlen % MOD
            i += length

        length <<= 1

    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def convolution(a, b):
    n = 1
    while n < len(a) + len(b):
        n <<= 1

    fa = a + [0] * (n - len(a))
    fb = b + [0] * (n - len(b))

    ntt(fa, False)
    ntt(fb, False)

    for i in range(n):
        fa[i] = fa[i] * fb[i] % MOD

    ntt(fa, True)
    return fa

def solve():
    s = input().strip()
    t = input().strip()
    m = int(input())

    n = len(s)
    nt = len(t)

    maxlen = n + nt + 5
    powm = [1] * maxlen
    for i in range(1, maxlen):
        powm[i] = powm[i - 1] * m % MOD

    ans = 0

    for c in range(26):
        a = [0] * n
        b = [0] * nt

        for i in range(n):
            if ord(s[i]) - 97 == c:
                a[i] = 1

        for i in range(nt):
            if ord(t[i]) - 97 == c:
                b[nt - 1 - i] = 1

        res = convolution(a, b)

        shift = n - 1

        for i in range(n + nt - 1):
            if res[i]:
                ans = (ans + res[i] * powm[i - shift]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```# Tại sao điều này khắc phục được các mẫu bị lỗi của bạn 

-`puila / tiu`thất bại vì tất cả các trận đấu đều được tính quá xa về bên phải. 
-`fft / justforfun`không thành công vì lỗi căn chỉnh số mũ kết hợp với nhiều số hạng, làm sai lệch tổng đa thức. 

Sau khi dịch chuyển bằng`(n-1)`, các chỉ số tích chập tương ứng chính xác với các vị trí căn chỉnh trong định nghĩa của$A_i$. 

Nếu bạn muốn, tôi cũng có thể chỉ ra một dẫn xuất rõ ràng hơn để tránh hoàn toàn FFT và xây dựng đa thức cuối cùng một cách trực tiếp trong một lần chuyển (thật là tao nhã một cách đáng ngạc nhiên đối với vấn đề này).
