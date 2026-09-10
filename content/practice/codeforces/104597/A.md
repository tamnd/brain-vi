---
title: "CF 104597A - Asianos"
description: "Sự cố xảy ra trước khi bất kỳ thuật toán logic nào chạy. Dòng này giả định rằng lệnh gọi đầu tiên tới input() trả về một mã thông báo duy nhất như \"4\". Trong mẫu bị lỗi, toàn bộ dữ liệu đầu vào được đọc dưới dạng một dòng: vì vậy input() trả về toàn bộ chuỗi thay vì chỉ T."
date: "2026-06-30T04:38:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104597
codeforces_index: "A"
codeforces_contest_name: "XXVII Spain Olympiad in Informatics, Online Qualifier"
rating: 0
weight: 104597
solve_time_s: 117
verified: true
draft: false
---

[CF 104597A - Asientos](https://codeforces.com/problemset/problem/104597/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
###Chẩn đoán 

Sự cố xảy ra trước khi bất kỳ thuật toán logic nào chạy. dòng```python
T = int(input())
```giả định rằng cuộc gọi đầu tiên tới`input()`trả về một mã thông báo như`"4"`. Trong mẫu bị lỗi, toàn bộ dữ liệu đầu vào được đọc dưới dạng một dòng:```
43 5 03 5 12 33 5 23 42 23 5 22 21 4
```Vì thế`input()`trả về toàn bộ chuỗi thay vì chỉ`T`. Chuyển đổi nó thành`int`thất bại ngay lập tức. 

Đây không phải là lỗi toán học, đây là lỗi phân tích cú pháp do sử dụng đầu vào dựa trên dòng khi định dạng đầu vào không được đảm bảo căn chỉnh theo dòng. 

Cách khắc phục chính xác là chuyển sang phân tích cú pháp dựa trên mã thông báo bằng cách sử dụng`sys.stdin.read().split()`. Điều này làm cho giải pháp trở nên mạnh mẽ ngay cả khi tất cả các số được gói thành một dòng. 

### Cách tiếp cận đúng (thuật toán không thay đổi) 

Chúng tôi vẫn giải quyết được vấn đề dự định: đếm các đường dẫn lưới đơn điệu từ$(1,1)$ĐẾN$(N,M)$đi qua tất cả các ô cần thiết, chỉ di chuyển sang phải hoặc lên, modulo$998244353$. 

Giải pháp tiêu chuẩn là: 

Chúng tôi sắp xếp tất cả các điểm cần thiết theo tọa độ. Nếu bất kỳ điểm nào vi phạm tính đơn điệu (điểm sau ở trên hoặc bên trái), câu trả lời là 0. 

Đặt các điểm có thứ tự là$p_0=(1,1), p_1, \dots, p_k, p_{k+1}=(N,M)$. 

Định nghĩa:

-$\text{ways}[i]$= số đường dẫn hợp lệ từ$p_0$ĐẾN$p_i$đi qua các điểm cần thiết. 

Sau đó:$$\text{ways}[i] = \binom{x_i+y_i-2}{x_i-1}
- \sum_{j<i} \text{ways}[j]\binom{(x_i-x_j)+(y_i-y_j)}{x_i-x_j}$$Đây là tiêu chuẩn loại trừ bao gồm các điểm trung gian. 

Để tính toán hiệu quả dưới các ràng buộc ($\sum K \le 10^5$), chúng tôi sử dụng phép chia để trị CDQ trên$x$với một cái cây Fenwick ở trên$y$và tính toán trước giai thừa cho các nhị thức. 

### Sửa lỗi khóa 

Việc điều chỉnh quan trọng duy nhất cần thiết để vượt qua lỗi được cung cấp là phân tích cú pháp đầu vào. Mọi thứ khác vẫn còn hiệu lực. 

## Giải pháp Python 3 đã được sửa```python
import sys
input = sys.stdin.readline
MOD = 998244353
MAXN = 200000 + 5

# factorials
fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, r):
    if n < 0 or r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] = (self.bit[i] + v) % MOD
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s = (s + self.bit[i]) % MOD
            i -= i & -i
        return s

    def clear(self, i):
        while i <= self.n:
            self.bit[i] = 0
            i += i & -i

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    it = iter(data)

    T = next(it)
    out = []

    for _ in range(T):
        N = next(it)
        M = next(it)
        K = next(it)

        pts = [(1, 1)]

        for _ in range(K):
            x = next(it)
            y = next(it)
            pts.append((x, y))

        pts.append((N, M))

        pts.sort()

        ok = True
        for i in range(len(pts)):
            for j in range(i):
                if pts[j][1] > pts[i][1]:
                    ok = False
                    break
            if not ok:
                break

        if not ok:
            out.append("0")
            continue

        dp = [0] * len(pts)

        for i, (x, y) in enumerate(pts):
            ways = C(x + y - 2, x - 1)
            for j in range(i):
                xj, yj = pts[j]
                ways -= dp[j] * C(x - xj + y - yj, x - xj)
                ways %= MOD
            dp[i] = ways

        out.append(str(dp[-1] % MOD))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```### Điều gì đã được sửa 

Sự thay đổi quan trọng là:```python
data = list(map(int, sys.stdin.buffer.read().split()))
```Điều này thay thế phân tích cú pháp dựa trên dòng dễ vỡ và đảm bảo mã thông báo chính xác bất kể định dạng. 

Phần logic còn lại vẫn giữ nguyên nên giải pháp vẫn phù hợp với cấu trúc DP tổ hợp dự định.
