---
title: "CF 104386B - Mảng ngẫu nhiên"
description: "Chúng tôi muốn phần tử nhỏ thứ k trong một tập hợp được hình thành bởi: - X: các giá trị xi mỗi lần lặp lại si lần - Y: các giá trị được chuyển đổi trên mỗi truy vấn dưới dạng alpha yj + beta, mỗi lần lặp lại tj Chúng tôi không bao giờ mở rộng mảng. Thay vào đó, chúng ta trả lời: có bao nhiêu phần tử ≤ v?"
date: "2026-07-01T02:49:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104386
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #14 (Cool-Forces)"
rating: 0
weight: 104386
solve_time_s: 178
verified: true
draft: false
---

[CF 104386B - Mảng ngẫu nhiên](https://codeforces.com/problemset/problem/104386/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

##Giải pháp 
## Ý chính (không thay đổi) 

Chúng tôi muốn phần tử nhỏ thứ k trong nhiều tập hợp được hình thành bởi: 

- X: giá trị`x_i`mỗi lần lặp lại`s_i`lần 
- Y: các giá trị được chuyển đổi trên mỗi truy vấn dưới dạng`alpha * y_j + beta`, mỗi lần lặp lại`t_j`lần 

Chúng tôi không bao giờ mở rộng mảng. Thay vào đó, chúng tôi trả lời: 

> có bao nhiêu phần tử ≤ v? 

Đây là đơn điệu trong v, vì vậy chúng tôi tìm kiếm câu trả lời nhị phân. 

## Sửa lỗi quan trọng 

Đối với mỗi truy vấn, giới hạn tìm kiếm nhị phân phải bao gồm: 

- tất cả các giá trị x 
- tất cả các giá trị y được chuyển đổi 

Vì vậy, chúng tôi tính toán: 

giá trị tối thiểu có thể:```
min(x[0], alpha*y[0] + beta)
```giá trị tối đa có thể:```
max(x[-1], alpha*y[-1] + beta)
```Điều này đảm bảo tìm kiếm nhị phân luôn hội tụ chính xác. 

## Giải pháp Python 3 đã được sửa```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

def build_prefix(w):
    pref = [0] * (len(w) + 1)
    for i, val in enumerate(w):
        pref[i + 1] = pref[i] + val
    return pref

def count_leq(arr, pref, x):
    return pref[bisect_right(arr, x)]

def solve():
    N, M, Q = map(int, input().split())

    x = list(map(int, input().split()))
    sx = list(map(int, input().split()))

    y = list(map(int, input().split()))
    ty = list(map(int, input().split()))

    px = build_prefix(sx)
    py = build_prefix(ty)

    for _ in range(Q):
        a, b, k = map(int, input().split())

        def count(v):
            # X contribution
            cx = count_leq(x, px, v)

            # Y contribution (invert transform)
            limit = (v - b) // a
            cy = count_leq(y, py, limit)

            return cx + cy

        # compute safe bounds for this query
        low = min(x[0], a * y[0] + b)
        high = max(x[-1], a * y[-1] + b)

        # expand bounds slightly to avoid edge misses
        lo = low - 1
        hi = high + 1

        while lo + 1 < hi:
            mid = (lo + hi) // 2
            if count(mid) >= k:
                hi = mid
            else:
                lo = mid

        print(hi)

if __name__ == "__main__":
    solve()
```## Điều gì thực sự đã xảy ra 

1. **Không có đường dẫn đầu ra được đảm bảo** 

Phiên bản trước phụ thuộc vào cấu trúc chức năng chưa bao giờ được kết nối đầy đủ với việc thực thi trọng tài, dẫn đến đầu ra trống. 
2. **Giới hạn tìm kiếm nhị phân không an toàn** 

Sử dụng phạm vi toàn cầu cố định như`[-1e12, 1e12]`không đáng tin cậy dưới nhiều phép biến đổi. Một số truy vấn dịch chuyển tất cả các giá trị ngoài phạm vi này trong thực tế, đặc biệt khi`beta`là lớn. 
3. **Bất biến đúng (được giữ)** 

chức năng`count(v)`là đơn điệu trong`v`, vì vậy tìm kiếm nhị phân vẫn hợp lệ. Chỉ có ranh giới thực hiện là sai. 

Phiên bản đã sửa này giữ nguyên ý tưởng thuật toán, sửa luồng thực thi và đảm bảo không gian tìm kiếm luôn chứa câu trả lời đúng cho mọi truy vấn.
