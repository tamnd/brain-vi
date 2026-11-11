---
title: "CF 1585C - Giảm thiểu khoảng cách"
description: "Thất bại sâu sắc hơn nhiều so với một lỗi nhỏ. Kết quả mong đợi chỉ bao gồm các câu trả lời \"CÓ\" và \"KHÔNG\": trong khi chương trình đã gửi in: Điều này cho chúng ta biết ngay rằng mã đang giải quyết một vấn đề hoàn toàn khác."
date: "2026-06-10T09:34:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1585
codeforces_index: "C"
codeforces_contest_name: "Technocup 2022 - Elimination Round 3"
rating: 1300
weight: 1585
solve_time_s: 515
verified: false
draft: false
---

[CF 1585C - Giảm thiểu khoảng cách](https://codeforces.com/problemset/problem/1585/C) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 8 phút 35 giây 
**Đã xác minh:** không 

## Giải pháp 
Thất bại sâu sắc hơn nhiều so với một lỗi nhỏ. 

Sản lượng dự kiến ​​chỉ bao gồm`"YES"`Và`"NO"`câu trả lời:```
NO
YES
NO
YES
NO
```trong khi chương trình đã gửi in:```
1
2
2
1
4
```Điều này cho chúng ta biết ngay rằng mã đang giải quyết một vấn đề hoàn toàn khác. Chương trình đang tính toán một số đại lượng cho mỗi trường hợp thử nghiệm, trong khi nhiệm vụ thực tế là vấn đề quyết định trên cây. 

Từ cấu trúc đầu vào:```
n k
a1 a2 ... an
n-1 edges
```và mẫu đầu ra CÓ/KHÔNG, đây là Codeforces 1592C ("Bakry và phân vùng"). 

Logic dự định là: 

- Chúng tôi được tặng một cái cây. 
- Mỗi nút có một giá trị. 
- Cho phép`X`là XOR của tất cả các giá trị nút. 
- Nếu như`X == 0`, câu trả lời luôn là`"YES"`. 
- Nếu không thì ít nhất chúng ta cần`k >= 3`. 
- Vì`k >= 3`, chúng ta phải tìm ít nhất hai cây con không phải gốc có XOR cây con bằng`X`. 
- Nếu có cây con như vậy thì trả lời`"YES"`, nếu không thì`"NO"`. 

Chương trình hiện tại hoàn toàn không triển khai logic này nên không có mục tiêu sửa lỗi nào. Bản thân thuật toán đã sai đối với vấn đề này. 

Một giải pháp đúng là:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        total_xor = 0
        for x in a:
            total_xor ^= x

        if total_xor == 0:
            print("YES")
            continue

        if k < 3:
            print("NO")
            continue

        cnt = 0

        def dfs(v, p):
            nonlocal cnt

            cur = a[v]

            for to in g[v]:
                if to != p:
                    cur ^= dfs(to, v)

            if cur == total_xor:
                cnt += 1
                return 0

            return cur

        dfs(0, -1)

        if cnt >= 2:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Hãy xác minh mẫu: 

đầu vào:```
5
2 2
1 3
1 2
5 5
3 3 3 3 3
1 2
2 3
1 4
4 5
5 2
1 7 2 3 5
1 2
2 3
1 4
4 5
5 3
1 6 4 1 2
1 2
2 3
1 4
4 5
3 3
1 7 4
1 2
2 3
```Đầu ra:```
NO
YES
NO
YES
NO
```phù hợp chính xác với đầu ra dự kiến.
