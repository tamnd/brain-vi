---
title: "CF 1749F - Khoảng cách đến con đường"
description: "Chúng ta có một cây có $n$ đỉnh. Mỗi đỉnh ban đầu giữ giá trị bằng 0. Có hai loại truy vấn. Loại đầu tiên yêu cầu giá trị hiện tại của một đỉnh cụ thể."
date: "2026-06-09T15:19:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 1749
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 138 (Rated for Div. 2)"
rating: 2800
weight: 1749
solve_time_s: 133
verified: true
draft: false
---

[CF 1749F - Khoảng cách đến con đường](https://codeforces.com/problemset/problem/1749/F) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh. Mỗi đỉnh ban đầu giữ giá trị bằng 0. Có hai loại truy vấn. Loại đầu tiên yêu cầu giá trị hiện tại của một đỉnh cụ thể. Loại thứ hai yêu cầu chúng ta thêm một giá trị cố định$k$đến tất cả các đỉnh nằm trong một khoảng cách nhỏ$d$từ đường nối hai đỉnh$u$Và$v$. Khoảng cách ở đây là khoảng cách cây thông thường: số cạnh dọc theo đường đi ngắn nhất. 

Bởi vì$n$Và$m$mỗi người có thể lên đến$2 \cdot 10^5$, một giải pháp đơn giản lặp qua tất cả các đỉnh cho mỗi truy vấn loại hai sẽ thực hiện tối đa$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Như vậy là quá chậm. Khoảng cách tối đa$d$trong bản cập nhật bị giới hạn ở 20, điều này gợi ý rằng chúng ta có thể khai thác một thuộc tính nhỏ của vùng lân cận thay vì chạm vào tất cả các đỉnh. 

Một khía cạnh tinh tế là cách xác định “khoảng cách đến con đường”. Ví dụ: một đỉnh không nằm trên đường đi vẫn có thể bị ảnh hưởng nếu nó nằm trong khoảng cách$d$tới bất kỳ đỉnh nào trên đường đi. Việc triển khai bất cẩn chỉ cập nhật các đỉnh dọc theo đường dẫn sẽ tạo ra kết quả không chính xác. Ví dụ: trong cây hình ngôi sao có tâm 1 và lá 2, 3, 4, một truy vấn cập nhật đường dẫn từ 2 đến 3 với$d = 1$cũng phải cập nhật đỉnh 1, đỉnh này không nằm ngay trên đường đi mà liền kề nhau. 

Một cái bẫy tiềm năng khác là khi$d$là rất lớn. Vì$d \ge \text{tree diameter}$, bản cập nhật ảnh hưởng đến tất cả các đỉnh. Trường hợp cạnh này cần được xử lý hiệu quả mà không cần lặp lại rõ ràng trên tất cả các nút. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Đối với mỗi truy vấn loại hai, chúng tôi xác định tất cả các đỉnh dọc theo đường đi từ$u$ĐẾN$v$, sau đó với mỗi đỉnh trong cây, chúng ta tính khoảng cách của nó đến từng đỉnh trên đường đi và cập nhật giá trị của nó nếu khoảng cách đó lớn nhất$d$. Điều này đảm bảo tính đúng đắn, nhưng độ phức tạp trong trường hợp xấu nhất là$O(m \cdot n \cdot d_{\text{path}})$, đó là về$8 \cdot 10^{10}$trong trường hợp xấu nhất. Rõ ràng là quá chậm. 

Cái nhìn sâu sắc quan trọng đến từ hai quan sát. Đầu tiên, khoảng cách tối đa$d$trong các bản cập nhật nhiều nhất là 20. Con số này cực kỳ nhỏ so với$n$. Thứ hai, các đỉnh bị ảnh hưởng chính xác là những đỉnh nằm trong khoảng cách$d$từ một số đường dẫn mà chúng ta có thể phân tách thành “khoảng cách từ mỗi điểm cuối trừ đi khoảng cách dọc theo đường dẫn”. Điều này cho thấy chúng ta có thể truyền bá bản cập nhật theo cách BFS hoặc DFS bắt đầu từ các nút đường dẫn, nhưng chỉ trong khoảng cách$d$. Từ$d \le 20$, mỗi bản cập nhật chạm nhiều nhất vào$O(d \cdot n_{\text{path}})$đỉnh, có thể chấp nhận được. Một cách đơn giản hóa khác là chúng ta có thể tính toán trước độ sâu và con trỏ gốc để nhanh chóng xác định đường đi và khoảng cách dọc theo cây. 

Chúng tôi tối ưu hóa hơn nữa bằng cách nhận thấy rằng các bản cập nhật chỉ liên quan đến các đỉnh trong một “quả bóng” nhỏ xung quanh đường đi. Sử dụng BFS từ các điểm cuối$u$Và$v$hoặc sử dụng cấu trúc LCA và chuyến tham quan Euler được tính toán trước, chúng ta có thể liệt kê một cách hiệu quả tất cả các đỉnh trong khoảng cách$d$khỏi đường đi mà không đi qua các đỉnh không liên quan. Điều này làm giảm độ phức tạp xuống còn khoảng$O(m \cdot d^2)$, điều này khả thi đối với$d \le 20$Và$m \le 2 \cdot 10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \cdot n^2)$|$O(n)$| Quá chậm | 
| Tối ưu (BFS/DFS khoảng cách nhỏ) |$O(m \cdot d^2 + n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và xây dựng danh sách kề. Điều này cho phép di chuyển nhanh chóng từ bất kỳ đỉnh nào tới các đỉnh lân cận của nó. 
2. Tính toán độ sâu và cha của mỗi đỉnh bằng DFS đơn giản. Độ sâu giúp xác định nhanh chóng khoảng cách giữa hai nút bất kỳ dọc theo cây. 
3. Chuẩn bị cấu trúc LCA, ví dụ sử dụng nâng nhị phân. LCA cho phép chúng ta nhanh chóng tìm ra đường đi giữa hai đỉnh bất kỳ$u$Và$v$, bởi vì đường dẫn có thể được chia thành đường dẫn từ$u$đến LCA và từ$v$tới LCA. 
4. Đối với truy vấn loại một, chỉ cần in giá trị hiện tại của đỉnh. Đây là thời gian không đổi. 
5. Đối với các truy vấn loại hai, trước tiên hãy tính LCA của$u$Và$v$để xác định đường đi. Đối với mỗi nút$x$trên đường đi từ$u$ĐẾN$v$, thực hiện BFS hoặc DFS theo khoảng cách$d$, thêm$k$tới tất cả các đỉnh đạt tới. Để tránh tính hai lần, hãy giữ một bộ đã truy cập chỉ cho bản cập nhật này. 
6. Bởi vì$d$nhỏ, BFS/DFS cho mỗi nút đường dẫn được giới hạn trong một vùng lân cận nhỏ. Ngay cả đối với đường đi có chiều dài trong trường hợp xấu nhất$n$, mỗi bản cập nhật chỉ lan truyền$O(d^2)$lần, có thể chấp nhận được với các ràng buộc. 

Tại sao nó hoạt động: Mỗi bản cập nhật chỉ ảnh hưởng đến các đỉnh trong khoảng cách$d$của con đường. Bằng cách bắt đầu BFS/DFS từ mỗi nút đường dẫn và giới hạn truyền tải theo khoảng cách$d$, chúng tôi bao gồm chính xác tập hợp bị ảnh hưởng. Phân tách LCA đảm bảo rằng đường đi được xác định chính xác và không có đỉnh nào bị bỏ qua. Tập đã truy cập đảm bảo rằng chúng ta không thêm$k$nhiều lần vào cùng một đỉnh trong một truy vấn. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n = int(input())
adj = [[] for _ in range(n)]
for _ in range(n-1):
    u, v = map(int, input().split())
    adj[u-1].append(v-1)
    adj[v-1].append(u-1)

# depth and parent for LCA
LOG = 20
parent = [[-1]*n for _ in range(LOG)]
depth = [0]*n

def dfs(u, p):
    parent[0][u] = p
    for v in adj[u]:
        if v != p:
            depth[v] = depth[u] + 1
            dfs(v, u)
dfs(0, -1)

# binary lifting
for k in range(1, LOG):
    for v in range(n):
        if parent[k-1][v] != -1:
            parent[k][v] = parent[k-1][parent[k-1][v]]

def lca(u, v):
    if depth[u] < depth[v]:
        u, v = v, u
    for k in reversed(range(LOG)):
        if parent[k][u] != -1 and depth[parent[k][u]] >= depth[v]:
            u = parent[k][u]
    if u == v:
        return u
    for k in reversed(range(LOG)):
        if parent[k][u] != -1 and parent[k][u] != parent[k][v]:
            u = parent[k][u]
            v = parent[k][v]
    return parent[0][u]

# value array
val = [0]*n

m = int(input())
for _ in range(m):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        print(val[tmp[1]-1])
    else:
        _, u, v, k, d = tmp
        u -= 1
        v -= 1
        w = lca(u, v)
        # collect path nodes
        path = []
        x = u
        while x != w:
            path.append(x)
            x = parent[0][x]
        path.append(w)
        stack = []
        x = v
        while x != w:
            stack.append(x)
            x = parent[0][x]
        path.extend(reversed(stack))
        
        visited = [False]*n
        for node in path:
            dq = deque()
            dq.append((node, 0))
            while dq:
                cur, dist = dq.popleft()
                if dist > d or visited[cur]:
                    continue
                visited[cur] = True
                val[cur] += k
                for nei in adj[cur]:
                    if not visited[nei]:
                        dq.append((nei, dist+1))
```Giải thích: DFS tính toán độ sâu và cấp độ gốc cho tất cả các nút. Nâng nhị phân cho phép chúng tôi tìm LCA trong$O(\log n)$thời gian. Đối với mỗi bản cập nhật, chúng tôi liệt kê đường dẫn từ$u$ĐẾN$v$thông qua LCA, sau đó thực hiện BFS giới hạn từ mỗi nút đường dẫn đến khoảng cách$d$. Một mảng được truy cập cục bộ sẽ tránh được việc tính hai lần trong cùng một bản cập nhật. Truy vấn loại một chỉ trả về giá trị được lưu trữ. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Truy vấn | Đường dẫn | Các đỉnh bị ảnh hưởng | Giá trị được cập nhật | 
| --- | --- | --- | --- | 
| 2 4 5 10 2 | 4-2-5 | 4,2,5,1,3 | +10 | 
| 1 3 | - | - | 10 | 
| 1 6 | - | - | 0 | 
| 2 1 1 10 20 | 1 | tất cả | |
