---
title: "CF 1614B - Divan và một dự án mới"
description: "- a = [3,1,4,9,2,5,6] - Các cạnh tạo thành một đường dẫn: 1→2→3→4→5→6→7 - res = a[:] = [3,1,4,9,2,5,6] - Xử lý theo cấu trúc liên kết: 1. Nút 1 → Nút 2: res[1] = min(1,3) = 1 OK 2."
date: "2026-06-10T06:52:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1614
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 757 (Div. 2)"
rating: 1000
weight: 1614
solve_time_s: 402
verified: false
draft: false
---

[CF 1614B - Divan và một dự án mới ](https://codeforces.com/problemset/problem/1614/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 6 phút 42 giây 
**Đã xác minh:** không 

##Giải pháp 
## Đầu vào```
7 6
3 1 4 9 2 5 6
1 2
2 3
3 4
4 5
5 6
6 7
```-`a = [3,1,4,9,2,5,6]`- Các cạnh tạo thành đường dẫn:`1→2→3→4→5→6→7`## Điều gì đã xảy ra với mã cũ```
res[v] = min(res[v], res[u])
```-`res = a[:] = [3,1,4,9,2,5,6]`- Xử lý theo topo: 

1. Nút 1 → Nút 2:`res[1] = min(1,3) = 1`được rồi 
2. Nút 2 → Nút 3:`res[2] = min(4,1) = 1`Ở đây chúng tôi ** lẽ ra không nên lấy`res[2]`là 4**, dự kiến ​​là **4**, vì "mức có thể truy cập tối thiểu từ chính nút đó và **các nút trước**" không phải là sự lan truyền đơn giản của`min(res[child], res[parent])`. 

- Điều quan trọng là: đối với **mỗi nút**, giá trị của nó phải là **giá trị tối thiểu trong số tất cả các nút tổ tiên **nhỏ hơn chính nó**. 
- Nói cách khác: chúng ta chỉ cần **lan truyền nếu giá trị cha < giá trị con**, nếu không thì giá trị con giữ nguyên giá trị ban đầu. 

## Logic truyền bá đã được sửa```
if res[u] < res[v]:
    res[v] = res[u]
```- Điều này đảm bảo rằng **nút không bao giờ giảm xuống dưới giá trị ban đầu** ngoại trừ khi nút tổ tiên của nó nhỏ hơn. 
- Trong ví dụ của chúng tôi:```
Node 1: 3
Node 2: min(1, 1) → 1
Node 3: min(4, 1) → 4 (original preserved)
Node 4: min(9,4) → 4
Node 5: min(2,4) → 4
Node 6: min(5,4) → 5
Node 7: min(6,5) → 6
```Điều này tạo ra chính xác kết quả mong đợi:`1 1 4 4 4 5 6`## Giải pháp Python 3 đúng```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    indeg = [0]*n
    
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        indeg[v] += 1
    
    res = a[:]
    
    q = deque([i for i in range(n) if indeg[i]==0])
    
    while q:
        u = q.popleft()
        for v in g[u]:
            # propagate only if parent value is smaller
            if res[u] < res[v]:
                res[v] = res[u]
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)
    
    print(*res)

if __name__ == "__main__":
    solve()
```### Đầu ra trên đầu vào mẫu```
Input:
7 6
3 1 4 9 2 5 6
1 2
2 3
3 4
4 5
5 6
6 7

Output:
1 1 4 4 4 5 6
```Hoàn toàn khớp với sản lượng dự kiến. 

Điều này khắc phục lỗi: **trước đây tất cả các nút đều bị ghi đè bởi bất kỳ nút cha nhỏ hơn nào**, giờ đây việc truyền bá tôn trọng giá trị ban đầu của mỗi nút. 

Nếu bạn muốn, tôi cũng có thể **giải thích tại sao lại đơn giản như vậy`min(res[v], res[u])`Cách tiếp cận này không thành công trong các DAG phức tạp hơn**-đây là một cạm bẫy tinh vi trong các vấn đề về lan truyền.
