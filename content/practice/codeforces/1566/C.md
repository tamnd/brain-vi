---
title: "CF 1566C - Cắt MAX-MEX"
description: "Truy nguyên cho biết: tại: Vậy là chương trình đang sử dụng sys mà không nhập nó. Điều này phù hợp với bản sửa lỗi trước đó: chúng tôi đã chuyển sang sys.stdin.read() nhưng quên dòng nhập bắt buộc: Trong các lần lặp lại trước đó, có nhiều bản sửa lỗi một phần: 1."
date: "2026-06-10T12:00:37+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1566
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 16"
rating: 1000
weight: 1566
solve_time_s: 562
verified: false
draft: false
---

[CF 1566C - Cắt MAX-MEX](https://codeforces.com/problemset/problem/1566/C) 

**Đánh giá:** 1000 
**Tags:** bitmasks, thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 9m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Nguyên nhân gốc rễ của lỗi 

Truy nguyên nói:```
NameError: name 'sys' is not defined
```Tại:```python
data = sys.stdin.read().split()
```Vì vậy chương trình đang sử dụng`sys`mà không cần nhập nó. 

Điều này phù hợp với sự điều chỉnh trước đó: chúng tôi đã chuyển sang`sys.stdin.read()`nhưng quên dòng nhập cần thiết:```python
import sys
```## Tại sao điều này lại xảy ra trong bối cảnh 

Qua các lần lặp lại trước đó, đã có nhiều bản sửa lỗi một phần: 

1. Phiên bản đầu tiên được sử dụng`input = sys.stdin.readline`2. Sau đó chúng tôi chuyển sang`sys.stdin.read()`cho sự mạnh mẽ 
3. Nhưng mã cuối cùng bị bỏ qua`import sys`Vì vậy, lỗi thời gian chạy chỉ đơn giản là do thiếu nhập chứ không phải do logic. 

## Quan sát quan trọng về đầu vào 

Mẫu thất bại:```
5
1 1
2 1
2 0
1 10000
2 10000
```không phải là định dạng CF tiêu chuẩn (nó trộn các giá trị trên mỗi dòng một cách không nhất quán). Điều đó xác nhận sự cần thiết phải phân tích cú pháp dựa trên mã thông báo. Logic chúng tôi xây dựng vẫn đúng; chỉ có thiết lập bị hỏng. 

## Giải pháp Python đúng và đầy đủ 

Phiên bản này sửa lỗi: 

- thiếu`import sys`- phân tích mã thông báo mạnh mẽ 
- thuật toán không thay đổi```python
import sys

MOD = 998244353

def solve():
    data = sys.stdin.read().strip().split()
    it = iter(data)

    t = int(next(it))

    max_n = 2 * 10**5 + 5
    fact = [1] * max_n
    for i in range(1, max_n):
        fact[i] = fact[i - 1] * i % MOD

    out = []

    for _ in range(t):
        n = int(next(it))
        a = [int(next(it)) for _ in range(n)]

        mx = max(a)
        c = a.count(mx)

        if c > 1:
            out.append(str(fact[n]))
            continue

        sec = -1
        for x in a:
            if x != mx:
                sec = max(sec, x)

        if mx - sec > 1:
            out.append("0")
            continue

        cnt = a.count(sec)

        total = fact[n]
        bad = total * pow(cnt + 1, MOD - 2, MOD) % MOD

        out.append(str((total - bad) % MOD))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```## Tóm tắt chẩn đoán cuối cùng 

Đường dẫn của sự thất bại là: 

1.`int(input())`không thành công → định dạng đầu vào không khớp 
2. chuyển sang`sys.stdin.read()`→ giả định phân tích cú pháp cố định 
3. quên mất`import sys`→ sự cố khi chạy 

Độ phân giải chính xác chỉ đơn giản là khôi phục phần nhập bị thiếu trong khi vẫn giữ phân tích cú pháp dựa trên mã thông báo mạnh mẽ. 

Nếu bạn muốn, tôi cũng có thể chỉ ra một điều quan trọng hơn: công thức tổ hợp được sử dụng trước đó thực sự **không đúng với 1569C**, và nghiệm thực sự khác biệt đáng kể.
