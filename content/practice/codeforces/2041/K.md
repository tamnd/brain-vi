---
title: "CF 2041K - Loài cân bằng dinh dưỡng"
description: "Chúng ta có một hệ sinh thái được mô hình hóa dưới dạng biểu đồ có hướng, trong đó mỗi nút đại diện cho một loài và mỗi cạnh có hướng thể hiện mối quan hệ kiếm ăn từ con mồi đến động vật ăn thịt. Đối với mỗi loài, chúng tôi muốn xác định xem đó có phải là loài cân bằng dinh dưỡng hay không."
date: "2026-06-08T09:45:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2041
codeforces_index: "K"
codeforces_contest_name: "2024 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 3100
weight: 2041
solve_time_s: 64
verified: true
draft: false
---

[CF 2041K - Loài cân bằng dinh dưỡng](https://codeforces.com/problemset/problem/2041/K) 

**Đánh giá:** 3100 
**Thẻ:** tìm kiếm nhị phân, lực lượng vũ phu, dfs và tương tự, đồ thị 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ sinh thái được mô hình hóa dưới dạng biểu đồ có hướng, trong đó mỗi nút đại diện cho một loài và mỗi cạnh có hướng thể hiện mối quan hệ kiếm ăn từ con mồi đến động vật ăn thịt. Đối với mỗi loài, chúng tôi muốn xác định xem đó có phải là loài cân bằng dinh dưỡng hay không. Một loài được cân bằng dinh dưỡng nếu sự khác biệt tuyệt đối giữa số lượng loài mà nó có thể tiếp cận (động vật ăn thịt ở hạ lưu) và số lượng loài có thể tiếp cận nó (con mồi ở thượng nguồn) được giảm thiểu ở tất cả các loài. 

Đầu vào cung cấp số lượng loài`n`và số lượng mối quan hệ nuôi dưỡng`m`, theo sau là`m`các cạnh có hướng. Đầu ra phải liệt kê tất cả các loài đạt được sự khác biệt tuyệt đối tối thiểu giữa số lượng ở thượng lưu và hạ nguồn của chúng. 

Các ràng buộc rất chặt chẽ:`n`có thể lên tới 200.000 và`m`lên tới 400.000. Điều này loại trừ các giải pháp ngây thơ tính toán rõ ràng khả năng tiếp cận cho từng nút thông qua DFS hoặc BFS riêng lẻ, vì đó sẽ là`O(n*(n+m))`trong trường hợp xấu nhất, đại khái là`4 * 10^10`hoạt động và rõ ràng là không khả thi. 

Các trường hợp biên dễ bị bỏ sót bao gồm các nút không có cạnh ra hoặc không có cạnh vào. Ví dụ, nếu tất cả các loài đều độc lập thì mọi loài đều không có ở thượng nguồn và không có ở hạ nguồn, vì vậy tất cả nên được coi là cân bằng dinh dưỡng. Tương tự, các chu kỳ trong biểu đồ có thể tăng số lượng nếu không được xử lý cẩn thận, do đó, giải pháp phải tính đến các thành phần được kết nối mạnh hoặc khả năng tiếp cận bắc cầu tương đương. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi loài, hãy chạy DFS để đếm số lượng loài có thể tiếp cận được từ nó (xuôi dòng) và một DFS khác trên biểu đồ đảo ngược để đếm số lượng loài có thể tiếp cận nó (ngược dòng). Sau đó tính toán sự khác biệt tuyệt đối cho từng loài và chọn mức tối thiểu. Mặc dù đúng nhưng điều này quá chậm vì mỗi DFS`O(n+m)`và chúng tôi thực hiện nó hai lần cho`n`các nút, dẫn đến`O(n*(n+m))`. 

Quan sát quan trọng là cấu trúc biểu đồ cho phép chúng ta tính toán trước khả năng tiếp cận một cách hiệu quả bằng cách sử dụng các tập hợp bit. Mỗi nút có thể duy trì một bit kích thước`n`đại diện cho các nút mà nó có thể tiếp cận. Việc lặp qua các nút theo thứ tự tôpô (hoặc thứ tự tôpô đảo ngược cho biểu đồ đảo ngược) cho phép chúng ta truyền bá thông tin về khả năng tiếp cận trong`O(n*m / w)`Ở đâu`w`là kích thước từ của máy (64 hoặc 32). Việc tối ưu hóa này là khả thi vì`k`nhỏ (16), ngụ ý có độ sâu hạn chế đối với các tập hợp con được kết nối đầy đủ và các thao tác theo bit trên các khối có kích thước cố định rất nhanh. 

Một góc nhìn khác là sử dụng các thành phần được kết nối mạnh mẽ (SCC). Tất cả các nút trong cùng một SCC có thể liên lạc với nhau. Khi SCC được cô đọng, biểu đồ sẽ trở thành DAG. Sau đó, chúng tôi có thể truyền bá số lượng khả năng tiếp cận một cách hiệu quả dọc theo DAG thay vì biểu đồ ban đầu. BFS/DFS trên DAG với tính năng ghi nhớ cung cấp số lượng ngược dòng/hạ lưu chính xác cho mỗi SCC, số lượng này có thể được phân phối đến các nút riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*(n+m)) | O(n) | Quá chậm | 
| Tuyên truyền Bitset / SCC-DAG | O((n+m) * n / w) | O(n^2 / w) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách kề cho biểu đồ và mặt trái của nó. 
2. Xác định các thành phần liên thông mạnh bằng thuật toán Kosaraju hoặc thuật toán Tarjan. Mỗi SCC được coi là một siêu nút duy nhất vì tất cả các nút trong SCC đều có thể kết nối với nhau. 
3. Thu gọn biểu đồ gốc thành DAG của SCC. Thay thế các cạnh giữa các nút trong các SCC khác nhau bằng các cạnh giữa các SCC của chúng. 
4. Khởi tạo các tập hợp bit cho mỗi SCC đại diện cho các nút có thể truy cập từ SCC đó. Ban đầu, bitset bao gồm tất cả các nút trong SCC. 
5. Lặp lại SCC theo thứ tự tôpô. Đối với mỗi SCC, hãy truyền tập hợp bit của nó tới các lân cận của nó trong DAG bằng cách sử dụng OR theo bit. Điều này tích lũy tất cả các nút hạ lưu có thể truy cập được từ mỗi SCC. 
6. Lặp lại quy trình tương tự trên DAG bị đảo ngược để tính toán khả năng tiếp cận ngược dòng cho mỗi SCC. 
7. Đối với mỗi nút gốc, số đếm ngược dòng của nó là kích thước của bitset ngược dòng trừ đi một (không bao gồm chính nó) và số lượng xuôi dòng là kích thước của bitset xuôi dòng trừ đi một. 
8. Tính toán chênh lệch tuyệt đối cho mỗi nút và theo dõi chênh lệch tối thiểu. 
9. Xuất ra tất cả các nút có chênh lệch bằng mức tối thiểu, được sắp xếp theo thứ tự tăng dần. 

Tại sao nó hoạt động: Mỗi SCC nắm bắt tất cả các nút có thể truy cập lẫn nhau, đảm bảo quá trình truyền tải ngược dòng và xuôi dòng là chính xác. Bằng cách sử dụng DAG, chúng tôi truyền bá khả năng tiếp cận một cách hiệu quả mà không cần truy cập lại các nút, duy trì tính chính xác. Thứ tự cấu trúc liên kết đảm bảo rằng tất cả các phiên bản trước của SCC đều có khả năng tiếp cận được truyền bá đầy đủ trước khi xử lý nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

sys.setrecursionlimit(1 << 25)

def main():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n)]
    radj = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append(v)
        radj[v].append(u)

    # Step 1: SCC using Kosaraju
    visited = [False] * n
    order = []
    def dfs1(u):
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                dfs1(v)
        order.append(u)
    for i in range(n):
        if not visited[i]:
            dfs1(i)

    comp = [-1] * n
    def dfs2(u, label):
        comp[u] = label
        for v in radj[u]:
            if comp[v] == -1:
                dfs2(v, label)

    label = 0
    for u in reversed(order):
        if comp[u] == -1:
            dfs2(u, label)
            label += 1

    # Step 2: Build SCC DAG
    scc_adj = [set() for _ in range(label)]
    scc_nodes = [set() for _ in range(label)]
    for u in range(n):
        scc_nodes[comp[u]].add(u)
        for v in adj[u]:
            if comp[u] != comp[v]:
                scc_adj[comp[u]].add(comp[v])

    # Step 3: Downstream reachability
    downstream = [set(nodes) for nodes in scc_nodes]
    topo_order = []
    indegree = [0]*label
    for u in range(label):
        for v in scc_adj[u]:
            indegree[v] += 1
    queue = deque([i for i in range(label) if indegree[i]==0])
    while queue:
        u = queue.popleft()
        topo_order.append(u)
        for v in scc_adj[u]:
            downstream[v].update(downstream[u])
            indegree[v] -= 1
            if indegree[v] == 0:
                queue.append(v)

    # Step 4: Upstream reachability
    upstream = [set(nodes) for nodes in scc_nodes]
    indegree = [0]*label
    rev_adj = [set() for _ in range(label)]
    for u in range(label):
        for v in scc_adj[u]:
            rev_adj[v].add(u)
            indegree[u] += 1
    queue = deque([i for i in range(label) if indegree[i]==0])
    while queue:
        u = queue.popleft()
        for v in rev_adj[u]:
            upstream[v].update(upstream[u])
            indegree[v] -= 1
            if indegree[v]==0:
                queue.append(v)

    # Step 5: Compute differences
    min_diff = n+1
    diffs = [0]*n
    for i in range(n):
        down = len(downstream[comp[i]]) - 1
        up = len(upstream[comp[i]]) - 1
        diffs[i] = abs(down - up)
        if diffs[i] < min_diff:
            min_diff = diffs[i]

    result = [str(i+1) for i in range(n) if diffs[i]==min_diff]
    print(" ".join(result))

if __name__ == "__main__":
    main()
```Giải pháp trước tiên là tìm SCC để thu gọn chu kỳ, xây dựng DAG, truyền bá khả năng tiếp cận xuôi dòng và ngược dòng bằng cách sử dụng các bộ và cuối cùng tính toán sự khác biệt tuyệt đối cho mỗi nút. Việc sử dụng các bộ đảm bảo tính chính xác ngay cả khi các nút thuộc về chu kỳ và việc truyền dọc theo DAG đảm bảo không tính hai lần. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

đầu vào:```
4 3
1 2
2 3
2 4
```| Nút | SCC | Hạ lưu | Thượng nguồn | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | {2,3,4} | {} | 3 | 
| 2 | 1 | {3,4} | {1} | 1 | 
| 3 | 2 | {} | {1,2} | 2 | 
| 4 | 3 | {} | {1,2} | 2 | 

Chênh lệch tối thiểu là 1 cho nút 2, đầu ra`2`. 

**Mẫu tùy chỉnh 2**: 

đầu vào:```
3 3
1 2
2 3
3 1
```Tất cả các nút tạo thành một chu kỳ: 

| Nút | SCC | Hạ lưu | Thượng nguồn | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | {1,2,3} | { | |
