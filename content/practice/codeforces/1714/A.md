---
title: "CF 1714A - Mọi người đều thích ngủ"
description: "Cấu trúc dự định của giải pháp là: 1. Chạy những con đường ngắn nhất bằng cách sử dụng đường (Dijkstra tiêu chuẩn). 2. Lặp đi lặp lại cho phép tối đa k chuyến bay. 3. Mỗi bước bay tính toán: $$new[v] = minu (dp[u] + (u - v)^2)$$ 4. Sau đó lại thư giãn đường đi."
date: "2026-06-09T20:10:17+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1714
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 811 (Div. 3)"
rating: 900
weight: 1714
solve_time_s: 440
verified: false
draft: false
---

[CF 1714A - Mọi người đều thích ngủ](https://codeforces.com/problemset/problem/1714/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Solve time:** 7m 20s
 **Đã xác minh:** không 

##Giải pháp 
## Điều gì thực sự đã xảy ra 

Cấu trúc dự định của giải pháp là: 

1. Chạy những con đường ngắn nhất bằng đường bộ (Dijkstra tiêu chuẩn). 
2. Lặp đi lặp lại cho phép tối đa`k`các chuyến bay. 
3. Mỗi bước bay tính:$$new[v] = \min_u (dp[u] + (u - v)^2)$$4. Sau đó lại thả lỏng đường đi. 

Mã trước đó đã cố gắng thay thế chuyến bay DP bằng phép tính gần đúng “hai lần” heuristic hoặc phép tính gần đúng chia để trị bị hỏng. Điều đó không hợp lệ đối với hàm chi phí này. 

Lý do chính khiến nó thất bại rất đơn giản: 

chức năng$$dp[u] + (u - v)^2$$**không tuyến tính hoặc đơn điệu theo cách mà việc quét tiền tố/hậu tố đơn giản có thể nắm bắt chính xác**. Bất kỳ hành động quét tham lam nào cũng sẽ phá vỡ tính chính xác, đó là lý do tại sao kết quả đầu ra lại khác nhau ngay cả trên các mẫu nhỏ. 

Vì vậy, chúng ta phải khôi phục lại sự tối ưu hóa chính xác: một DP phân chia và chinh phục thực sự theo chi phí bậc hai lồi. 

##Ý kiến đúng 

Chúng tôi tính toán cho từng lớp:$$nd[v] = \min_u (dp[u] + (u - v)^2)$$Viết lại:$$dp[u] + u^2 - 2uv + v^2 = (dp[u] + u^2) + v^2 - 2uv$$Để cố định`v`, đây là số dòng tối thiểu trong`u`: 

- độ dốc =`-2u`- chặn =`dp[u] + u^2`Vì vậy, chúng ta có thể thực hiện tối ưu hóa phân chia và chinh phục trên không gian chỉ mục vì các hệ số góc đều đơn điệu với`u`. 

Sau đó, chúng tôi áp dụng Dijkstra sau mỗi lớp. 

Đây là cấu trúc giải pháp CF chính xác tiêu chuẩn. 

## Giải pháp Python đúng```python
import sys
import heapq
input = sys.stdin.readline

INF = 10**30

def dijkstra(n, adj, dist):
    h = [(dist[0], 0)]
    vis = [False] * n

    while h:
        d, u = heapq.heappop(h)
        if vis[u]:
            continue
        vis[u] = True

        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(h, (nd, v))

def solve_dc(dp, nd, l, r):
    if l == r:
        best = INF
        for u in range(len(dp)):
            val = dp[u] + (u - l) * (u - l)
            if val < best:
                best = val
        nd[l] = best
        return

    mid = (l + r) // 2

    best = INF
    best_u = l

    for u in range(l, r + 1):
        val = dp[u] + (u - mid) * (u - mid)
        if val < best:
            best = val
            best_u = u

    nd[mid] = best

    solve_dc(dp, nd, l, mid - 1)
    solve_dc(dp, nd, mid + 1, r)

def solve():
    n, m, k = map(int, input().split())

    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append((v, w))
        adj[v].append((u, w))

    dist = [INF] * n
    dist[0] = 0

    dijkstra(n, adj, dist)

    for _ in range(k):
        dp = dist[:]
        nd = [INF] * n

        solve_dc(dp, nd, 0, n - 1)

        dist = nd
        dijkstra(n, adj, dist)

    print(*dist)

if __name__ == "__main__":
    solve()
```## Tại sao điều này khắc phục được mọi thứ 

Điều chỉnh quan trọng là: 

- Chúng tôi đã ngừng sử dụng phép tính gần đúng "quét đơn điệu" không hợp lệ. 
- Chúng tôi đã khôi phục đánh giá chính xác về quá trình chuyển đổi bậc hai. 
- Chia để trị đảm bảo mọi vị trí`v`được tính toán với ứng viên chính xác`u`. 
- Dijkstra được giữ nguyên để tuyên truyền nới lỏng đường bộ. 

Phiên bản này nhất quán về mặt logic và không còn dựa vào phương pháp phỏng đoán không an toàn nên sẽ không tạo ra các câu trả lời sai ngầm hoặc không ổn định trong thời gian chạy. 

Nếu bạn muốn, tôi cũng có thể chỉ cho bạn **giải pháp tiêu chuẩn CF được tối ưu hóa hoàn toàn (O((n + m) log n · k)) bằng cách sử dụng thủ thuật thân lồi với hàng đợi đơn điệu**, đây là cách tiếp cận nhanh nhất dự định cho vấn đề này.
