---
title: "CF 102391I - Cây bao trùm đường kính tối thiểu"
description: "Chúng ta cần chọn chính xác (N-1) cạnh đầu vào kết nối tất cả (N) đỉnh mà không có chu trình và trong số tất cả các cây bao trùm như vậy sẽ giảm thiểu độ dài của đường đi dài nhất của cây. Độ dài cạnh là số nguyên dương nên mọi khoảng cách của cây và đường kính cuối cùng đều là số nguyên."
date: "2026-08-10T21:05:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "I"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 403
verified: false
draft: false
---

[CF 102391I - Cây bao trùm đường kính tối thiểu](https://codeforces.com/problemset/problem/102391/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn chính xác (N-1) cạnh đầu vào kết nối tất cả (N) đỉnh mà không có chu trình và trong số tất cả các cây bao trùm như vậy sẽ giảm thiểu độ dài của đường đi dài nhất của cây. Độ dài cạnh là số nguyên dương nên mọi khoảng cách của cây và đường kính cuối cùng đều là số nguyên. 

Đồ thị có thể có tối đa (500) đỉnh và khoảng (125000) cạnh. Giá trị này đủ nhỏ về mặt đỉnh đối với thuật toán (O(N^3)), nhưng quá lớn đối với bất kỳ số mũ nào trong (N). Trường hợp dày đặc đặc biệt phù hợp vì (M) có thể là (\Theta(N^2)). Tại (N=500), (N^3) là (125) triệu, vốn đã gần với giới hạn thực tế trong Python, trong khi việc liệt kê các cây bao trùm là hoàn toàn không thể. Giới hạn trọng số cạnh (10^9) có nghĩa là một đường dẫn có thể có độ dài khoảng (5\cdot10^{11}), do đó, số học 64 bit là bắt buộc trong các ngôn ngữ có số nguyên có kích thước cố định. Số nguyên Python không bị tràn. 

Khó khăn chính là việc giảm thiểu đường kính không giống như tính toán cây bao trùm tối thiểu. Cây bao trùm tối thiểu có thể có chuỗi rất dài, trong khi cây đắt tiền hơn một chút có thể có đường kính nhỏ hơn nhiều. 

Cũng có một số trường hợp phá vỡ việc triển khai ngây thơ. 

Đối với đồ thị nhỏ nhất có thể,```
2 1
1 2 7
```cây bao trùm duy nhất chứa cạnh duy nhất, vì vậy câu trả lời là```
7
1 2
```Việc triển khai giả định tâm phải là một đỉnh vẫn có thể tồn tại trong trường hợp này, nhưng việc triển khai dự kiến ​​có hai nhánh riêng biệt xung quanh tâm có thể thất bại vì tâm được phép nằm ở bất kỳ đâu trên cạnh duy nhất. 

Một trường hợp thú vị hơn là đường dẫn có trọng số:```
3 2
1 2 100
2 3 1
```Cây bao trùm duy nhất có đường kính (101), vì vậy câu trả lời là```
101
1 2
2 3
```Tâm của cây này nằm bên trong cạnh (1)-(2), không phải ở đỉnh đồ thị. Chỉ kiểm tra các cây có đường đi ngắn nhất có gốc tại các đỉnh sẽ bỏ sót tâm thực tế. 

Khoảng cách bằng nhau cũng quan trọng. Vì```
3 3
1 2 1
2 3 1
1 3 1
```câu trả lời là (2), và bất kỳ hai cạnh nào tạo thành cây đều hợp lệ. Khi một số đỉnh có khoảng cách bằng nhau tính từ điểm cuối của cạnh trung tâm ứng cử viên, quá trình quét phải sử dụng các so sánh chặt chẽ khi quyết định đỉnh nào thuộc về phía đối diện. Việc coi khoảng cách bằng nhau như một bên mới có thể tạo ra một ứng cử viên không hợp lệ. 

Cuối cùng, trọng số rất lớn không được cắt bớt. Ví dụ,```
2 1
1 2 1000000000
```có câu trả lời (1000000000). Trong C++, số nguyên 32 bit không đủ để tính tổng của một số cạnh như vậy, do đó ma trận khoảng cách phải sử dụng số nguyên 64 bit. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là liệt kê từng cây bao trùm, tính đường kính của nó và giữ lại cây tốt nhất. Nó đúng vì mọi câu trả lời có thể đều được xem xét rõ ràng. Vấn đề là số lượng cây. Một đồ thị hoàn chỉnh trên (N) đỉnh có (N^{N-2}) cây bao trùm theo công thức Cayley. Tại (N=500), đây là (500^{498}) cây và việc kiểm tra ngay cả các cạnh (N-1) của mỗi cây sẽ yêu cầu kiểm tra cạnh theo thứ tự (499\cdot500^{498}). Lực lượng vũ phu không thể khả thi từ xa. 

Một cách tiếp cận ít vô lý hơn nhưng vẫn không phù hợp là chọn mọi đỉnh làm tâm cây có thể, xây dựng cây có đường đi ngắn nhất từ ​​đỉnh đó và thu được kết quả tốt nhất. Điều này giải quyết được trường hợp tâm cây tối ưu là một đỉnh. Tuy nhiên, một cây có trọng số có thể có tâm của nó nằm hoàn toàn bên trong một cạnh, như đường đi ba đỉnh có độ dài cạnh (100) và (1) minh họa. Chúng ta cũng cần xem xét các điểm bên trong các cạnh. 

Quan sát quan trọng là ngừng nghĩ về bản thân cái cây mà thay vào đó hãy nghĩ về điểm giữa của đường kính của nó. Mỗi cây đều có tâm, có thể là một đỉnh hoặc một điểm nằm trong một cạnh. Nếu chúng ta biết tâm, cây đường đi ngắn nhất bắt nguồn từ tâm đó là đủ để có được cây bao trùm tối ưu. Đây là mối liên hệ giữa bài toán cây khung có đường kính nhỏ nhất và bài toán 1 tâm tuyệt đối của đồ thị. Tâm 1 tuyệt đối là điểm, có thể nằm bên trong một cạnh, giảm thiểu khoảng cách đường đi ngắn nhất tối đa của nó tới tất cả các đỉnh của đồ thị. Cây có đường đi ngắn nhất có gốc tại điểm đó sẽ cho ra cây bao trùm có đường kính tối thiểu. 

Chúng ta có thể tìm thấy trung tâm này mà không cần thử mọi tọa độ thực tế có thể. Đầu tiên tính toán khoảng cách đường đi ngắn nhất của tất cả các cặp. Sau đó xem xét trực tiếp một đỉnh trung tâm có thể. Đối với một cạnh ((u,v)), hãy viết độ dài gấp đôi của nó là (W) và đặt tâm cách xa gấp đôi (X) tính từ (u). Đối với một đỉnh (z), khoảng cách của nó tới tâm là 

[ 
\min(D[u][z]+X,\ D[v][z]+W-X), 
] 

trong đó mọi khoảng cách đồ thị cũng đã được nhân đôi. 

Khi tâm di chuyển dọc theo một cạnh, mỗi đỉnh tạo ra hàm mái hai đoạn. Điểm tối đa của tất cả các chức năng này là độ lệch tâm của trung tâm. Mức tối thiểu của nó xảy ra tại điểm giao nhau của hai đoạn mái liên quan. Chúng ta có thể tìm thấy tất cả các giao điểm có liên quan bằng cách quét sau khi sắp xếp các đỉnh theo khoảng cách của chúng với một điểm cuối. Đây là phép tính trung tâm tuyệt đối tiêu chuẩn (O(N^3)) được sử dụng cho bài toán này. 

Đối với một cạnh ((u,v)), giả sử (a) là đỉnh xa nhất được gán cho cạnh (u) và (b) là đỉnh xa nhất được gán cho cạnh (v). Tại điểm giao nhau tối ưu, 

[ 
D[u][a]+X=D[v][b]+W-X. 
] 

Bán kính nhân đôi thu được là 

[ 
D[u][a]+D[v][b]+W. 
] 

Chúng ta có thể liệt kê (a) theo thứ tự giảm dần là (D[u][a]). Khi (a) được cố định, (b) phải là đỉnh có (D[v][b]) lớn nhất trong số các đỉnh có khoảng cách đến (u) lớn hơn (a). Việc duy trì mức tối đa đó trong khi quét sẽ mang lại công việc liên tục trên mỗi đỉnh. Đây chính xác là mức giảm đưa tìm kiếm trung tâm về (O(N^3)) sau các đường đi ngắn nhất tất cả các cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^{N-1})) hoặc tệ hơn | (O(N^2)) | Quá chậm | 
| Chỉ trung tâm Vertex | (O(NM\log N)) với Dijkstra lặp lại | (O(N^2+M)) | Sai trên đồ thị có trọng số | 
| Tối ưu | (O(N^3+MN)) | (O(N^2+M)) | Đã chấp nhận | 

Vì (M\leq N(N-1)/2), giới hạn cuối cùng là (O(N^3)). 

## Hướng dẫn thuật toán 

1. Nhân đôi trọng lượng mỗi cạnh. 

Nhân đôi sẽ loại bỏ tất cả các phân số khỏi tính toán trung tâm. Đường kính thực tế vẫn là số nguyên, trong khi tâm có thể nằm ở khoảng cách nửa số nguyên tính từ điểm cuối. Sau khi nhân đôi, mọi tọa độ liên quan đều là tích phân. 

1. Tính toán khoảng cách đường đi ngắn nhất của tất cả các cặp.

Chúng ta cần (D[u][v]) cho mỗi cặp vì mỗi cạnh tâm ứng cử viên sẽ so sánh khoảng cách từ cả hai điểm cuối đến mọi đỉnh. Với (N\leq500), Floyd-Warshall đưa ra nghiệm (O(N^3)) trong trường hợp dày đặc. Đối với đầu vào thưa thớt, việc triển khai bên dưới sử dụng Dijkstra lặp lại thay vào đó, cách này nhanh hơn trong thực tế. 

1. Sắp xếp các đỉnh theo khoảng cách của chúng với mọi điểm cuối có thể. 

Với mỗi đỉnh (u), lưu trữ các đỉnh theo thứ tự không giảm là (D[u][z]). Điều này cho phép chúng ta quét các đỉnh xa nhất có thể theo thứ tự giảm dần. 

1. Kiểm tra tâm là đỉnh đồ thị. 

Nếu tâm là đỉnh (u) thì bán kính của nó là độ lệch tâm của nó, 

[ 
R(u)=\max_z D[u][z]. 
] 

Đường kính cây tương ứng là (2R(u)) trong biểu diễn nhân đôi. Giữ giá trị nhỏ nhất như vậy và ghi nhớ đỉnh. 

1. Kiểm tra tâm bên trong mỗi cạnh của đồ thị. 

Đối với cạnh ((u,v)) có chiều dài gấp đôi (W), quét các đỉnh từ xa nhất đến gần nhất theo khoảng cách từ (u). Duy trì một đỉnh (p) với (D[v][p]) tối đa trong số các đỉnh đã được xử lý, nghĩa là những đỉnh ở xa (u) hơn ứng cử viên hiện tại. 

Bất cứ khi nào đỉnh hiện tại (a) có 

[ 
D[v][a]>D[v][p], 
] 

cặp ((a,p)) xác định khả năng giao nhau. Ứng cử viên có đường kính gấp đôi của nó là 

[ 
C=D[u][a]+D[v][p]+W. 
] 

Tọa độ tâm được đo từ (u), vẫn được nhân đôi, là 

[ 
X=\frac{D[u][a]+W-D[v][p]}{2}. 
] 

Chỉ (0\leq X\leq W) đại diện cho một điểm thực sự nằm trên cạnh này. Nếu giao điểm nằm ngoài cạnh, thì mức tối ưu sẽ đạt được xa hơn dọc theo biểu đồ và sẽ được đại diện bởi một ứng cử viên khác, do đó giao điểm không hợp lệ sẽ bị loại bỏ. 

1. Ghi nhớ trung tâm tốt nhất. 

Giá trị được thuật toán lưu trữ đã là đường kính thực tế của cây, vì tất cả khoảng cách đã được nhân đôi và công thức trên tính toán gấp đôi bán kính tâm. Do đó, đường kính ban đầu có cùng giá trị nguyên. 

1. Xây dựng cây khung từ tâm đã chọn. 

Nếu tâm là một đỉnh, hãy chạy Dijkstra từ đỉnh đó và giữ nguyên tiền thân của mỗi đỉnh. Cây đường đi ngắn nhất thu được có đường kính tối thiểu được yêu cầu. 

Nếu tâm nằm bên trong một cạnh ((u,v)), hãy đặt khoảng cách nhân đôi của nó tới (u) và (v) là (X) và (W-X). Chạy Dijkstra đa nguồn được khởi tạo với 

[ 
dist[u]=X,\qquad dist[v]=W-X. 
] 

Đây chính xác là phép tính đường đi ngắn nhất từ tâm ảo thu được bằng cách chia nhỏ cạnh. Hai đỉnh ban đầu tạo thành hai khu rừng có đường đi ngắn nhất có gốc. Thêm cạnh ban đầu ((u,v)) để nối hai khu rừng đó. Kết quả có chính xác (N-1) cạnh đồ thị gốc. 

Tại sao nó hoạt động: cho (c) là tâm của cây bao trùm tối ưu. Mọi đường đi giữa hai đỉnh của cây đó đều đi qua tâm nên đường kính của nó gấp đôi khoảng cách tối đa từ tâm đến một đỉnh. Việc thay thế mỗi đường đi từ gốc tới đỉnh bằng đường đi ngắn nhất trong biểu đồ gốc không thể tăng khoảng cách tối đa đó. Do đó, cây đường đi ngắn nhất có gốc ở điểm 1 tuyệt đối là tối ưu. Tâm là một đỉnh của đồ thị hoặc nằm bên trong một số cạnh của đồ thị, vì vậy hai phần của bảng liệt kê bao gồm mọi mức tối ưu có thể. Đối với một cạnh cố định, quá trình quét xem xét chính xác các cặp đỉnh có thể là đỉnh xa nhất ở hai cạnh của tâm và phương trình giao nhau cho bán kính nhỏ nhất cho cặp đó. Do đó, ứng cử viên nhỏ nhất là bán kính tâm tuyệt đối và cây đường đi ngắn nhất được xây dựng có chính xác đường kính tối thiểu có thể. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def all_pairs_shortest(n, m, adj):
    # For sparse graphs, repeated Dijkstra is considerably cheaper.
    # For dense graphs, Floyd-Warshall avoids a large number of heap operations.
    if m * 4 <= n * n:
        dist = []
        for s in range(n):
            ds = [INF] * n
            ds[s] = 0
            pq = [(0, s)]

            while pq:
                d, u = heapq.heappop(pq)
                if d != ds[u]:
                    continue
                for v, w in adj[u]:
                    nd = d + w
                    if nd < ds[v]:
                        ds[v] = nd
                        heapq.heappush(pq, (nd, v))

            dist.append(ds)

        return dist

    dist = [[INF] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0

    for u in range(n):
        for v, w in adj[u]:
            if w < dist[u][v]:
                dist[u][v] = w

    for k in range(n):
        dk = dist[k]
        for i in range(n):
            di = dist[i]
            dik = di[k]
            if dik == INF:
                continue
            # The comprehension is noticeably faster in Python than
            # an explicit inner loop.
            di[:] = [
                x if x <= dik + y else dik + y
                for x, y in zip(di, dk)
            ]

    return dist

def build_tree_vertex(root, n, adj):
    dist = [INF] * n
    parent = [-1] * n
    dist[root] = 0
    pq = [(0, root)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                parent[v] = u
                heapq.heappush(pq, (nd, v))

    edges = []
    for v in range(n):
        if v != root:
            edges.append((parent[v], v))

    return edges

def build_tree_edge(u, v, x, w, n, adj):
    # x is the doubled distance from the virtual center to u.
    # w - x is the doubled distance from the virtual center to v.
    dist = [INF] * n
    parent = [-1] * n

    dist[u] = x
    dist[v] = w - x

    pq = [(x, u), (w - x, v)]

    while pq:
        d, cur = heapq.heappop(pq)
        if d != dist[cur]:
            continue

        for to, ew in adj[cur]:
            nd = d + ew
            if nd < dist[to]:
                dist[to] = nd
                parent[to] = cur
                heapq.heappush(pq, (nd, to))

    edges = [(u, v)]

    for z in range(n):
        if z == u or z == v:
            continue
        edges.append((parent[z], z))

    return edges

def solve():
    n, m = map(int, input().split())

    edges = []
    adj = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1

        # Work entirely in doubled lengths.
        w *= 2

        edges.append((u, v, w))
        adj[u].append((v, w))
        adj[v].append((u, w))

    dist = all_pairs_shortest(n, m, adj)

    # order[u] contains vertices sorted by distance from u.
    order = []
    for u in range(n):
        row = list(range(n))
        row.sort(key=dist[u].__getitem__)
        order.append(row)

    best = INF
    best_vertex = 0
    best_edge = None
    best_x = None

    # Center at a graph vertex.
    for u in range(n):
        ecc = dist[u][order[u][-1]]
        cand = 2 * ecc

        if cand < best:
            best = cand
            best_vertex = u
            best_edge = None
            best_x = None

    # Center inside an edge.
    for u, v, w in edges:
        row = order[u]

        # p is the best v-distance among vertices strictly farther
        # from u than the current vertex.
        p = row[-1]

        for idx in range(n - 2, -1, -1):
            a = row[idx]

            if dist[v][a] > dist[v][p]:
                cand = dist[u][a] + dist[v][p] + w

                numerator = dist[u][a] + w - dist[v][p]

                # All values are doubled, so numerator is even.
                x = numerator // 2

                if 0 <= x <= w and cand < best:
                    best = cand
                    best_edge = (u, v, w)
                    best_x = x
                    best_vertex = -1

                p = a

    # Construct an SPT from the chosen absolute center.
    if best_edge is None:
        tree_edges = build_tree_vertex(best_vertex, n, adj)
    else:
        u, v, w = best_edge
        if best_x == 0:
            tree_edges = build_tree_vertex(u, n, adj)
        elif best_x == w:
            tree_edges = build_tree_vertex(v, n, adj)
        else:
            tree_edges = build_tree_edge(u, v, best_x, w, n, adj)

    out = [str(best)]
    out.extend(f"{u + 1} {v + 1}" for u, v in tree_edges)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ cả danh sách kề và danh sách cạnh. Dijkstra cần danh sách kề sau này khi xây dựng cây cuối cùng, trong khi danh sách cạnh là những gì mà quét trung tâm liệt kê. 

Tất cả các trọng số được nhân hai ngay lập tức. Đây không chỉ là một thủ thuật thẩm mỹ. Tâm bên trong một cạnh có thể có tọa độ ban đầu chẳng hạn như (3,5) và việc nhân đôi sẽ tạo ra tọa độ đó (7), vì vậy mọi phép so sánh vẫn giữ nguyên số học số nguyên chính xác. 

Quy trình tất cả các cặp sẽ lựa chọn giữa Dijkstra và Floyd-Warshall lặp lại. Trường hợp dày đặc là bối cảnh tự nhiên cho Floyd-Warshall, trong khi đồ thị thưa thớt được hưởng lợi đáng kể từ Dijkstra. Dù bằng cách nào, ma trận trả về chứa khoảng cách đường đi ngắn nhất được nhân đôi chính xác. 

các`order`mảng thực hiện việc sắp xếp theo yêu cầu của việc quét cạnh. Phần tử cuối cùng là đỉnh xa nhất tính từ điểm cuối tương ứng. Trong quá trình quét cạnh,`p`đại diện cho ứng cử viên tốt nhất ở phía đối diện trong số tất cả các đỉnh đã được biết là ở xa hơn (u). Sự nghiêm khắc`>`việc so sánh là có chủ ý vì các đỉnh ở cùng khoảng cách với (u) có thể đã được bao phủ bởi cạnh (u). 

Vòng lặp đỉnh-tâm lưu trữ (2\cdot\operatorname{ecc}(u)). Giá trị này là bán kính nhân đôi, chính xác là đường kính cây ở tỷ lệ ban đầu. 

Đối với một ứng cử viên cạnh tranh,`cand`là 

[ 
D[u][a]+D[v][p]+W. 
] 

Tọa độ trung tâm được tính toán từ sự bằng nhau của hai đoạn mái chủ động. Việc kiểm tra phạm vi ngăn chặn việc giao cắt bên ngoài cạnh thực tế được coi là tâm hợp lệ. 

Việc xây dựng sử dụng Dijkstra thông thường thay vì ma trận tất cả các cặp. Điều này giữ cho việc xây dựng đơn giản và tránh lưu trữ thông tin trước đó cho mọi nguồn có thể. Khi tâm là điểm trong của một cạnh, hai trạng thái Dijkstra ban đầu biểu thị hai nửa của cạnh được chia. Các cạnh trước của chúng tạo thành hai cây có đường đi ngắn nhất rời nhau. Cạnh ban đầu nối chúng thành một cây. 

Trọng số cạnh dương làm cho mọi phiên bản tiền nhiệm gần với trung tâm ảo hơn. Do đó, các con trỏ trước không thể chứa chu trình có hướng. Trong trường hợp hai nguồn, (u) và (v) là gốc của các khu rừng tiền thân riêng biệt, do đó, việc thêm ((u,v)) sẽ tạo ra chính xác một kết nối giữa các khu rừng đó và tạo ra chính xác các cạnh (N-1). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác có mọi cạnh dài (1). Sau khi nhân đôi, mỗi cạnh đều có trọng số (2). 

Đối với đỉnh (1), khoảng cách nhân đôi là (0,2,2), do đó độ lệch tâm của nó là (2), cho ra đường kính ứng cử viên là (4) trong ký hiệu bán kính nhân đôi, tương ứng với đường kính thực tế là (2). 

Một dấu vết thuận tiện là: 

| Bước | Trung tâm ứng viên | Nhân đôi khoảng cách từ trung tâm | Đường kính ứng viên | 
| --- | --- | --- | --- | 
| 1 | Đỉnh 1 | (0,2,2) | 2 | 
| 2 | Đỉnh 2 | (2,0,2) | 2 | 
| 3 | Đỉnh 3 | (2,2,0) | 2 | 
| 4 | Trung tâm cạnh bất kỳ | đối xứng | 2 | 

Ứng viên đỉnh đầu tiên đã là tối ưu. Dijkstra có gốc tại đỉnh (1) chọn các cạnh (1)-(2) và (1)-(3), tạo ra một cây có đường đi từ lá này sang lá khác không tầm thường có độ dài (2). 

Ví dụ này chứng tỏ rằng nhiều trung tâm và nhiều cây tối ưu là vô hại. Thuật toán chỉ cần một trong số chúng. 

### Mẫu 2 

Biểu đồ chứa hai vùng compact được kết nối bởi cạnh nặng (3)-(4), có độ dài là (1000). Vùng bên trái có các cạnh có độ dài (10,20,30), trong khi vùng bên phải có các cạnh có độ dài (30,20,10). 

Cây tối ưu được hiển thị bởi mẫu là đường dẫn 

[ 
1\rightarrow2\rightarrow3\rightarrow4\rightarrow6\rightarrow5 
] 

với tổng chiều dài 

[ 
10+20+1000+10+20=1060. 
] 

Tâm hữu ích nằm bên trong cạnh nặng (3)-(4). Trung tâm cân bằng cành dài nhất bên trái với cành dài nhất bên phải. 

| Bước | Phần liên quan | Giá trị | 
| --- | --- | --- | 
| 1 | Nhánh trái (1\rightarrow2\rightarrow3) | 30 | 
| 2 | Cạnh nặng (3\rightarrow4) | 1000 | 
| 3 | Nhánh phải (4\rightarrow6\rightarrow5) | 30 | 
| 4 | Tổng đường kính | 1060 | 

Tìm kiếm ở giữa xác định cạnh nặng là ứng cử viên quan trọng vì đồ thị được chia một cách tự nhiên thành hai nhóm có các đỉnh xa nhất nằm ở các cạnh đối diện. Khi trung tâm đó được chọn, việc xây dựng đường đi ngắn nhất đa nguồn sẽ tạo ra hai khu rừng có đường đi ngắn nhất và nối chúng thông qua (3)-(4). 

Dấu vết cho thấy tại sao chỉ kiểm tra các đỉnh đồ thị là không đủ. Một đỉnh ở một trong hai điểm cuối của cạnh nặng sẽ để toàn bộ cạnh có chiều dài (1000) ở một bên của tâm, trong khi một điểm bên trong cạnh cân bằng hai cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3+MN)) | Đường đi ngắn nhất cho tất cả các cặp, sắp xếp, quét trung tâm cạnh và một Dijkstra cuối cùng | 
| Không gian | (O(N^2+M)) | Ma trận khoảng cách, mảng thứ tự, danh sách kề và cạnh đầu vào | 

Bởi vì (M\leq N(N-1)/2), độ quét tâm tối đa là (O(N^3)). Với (N\leq500), giới hạn tiệm cận phù hợp với nghiệm dự định. Việc triển khai cũng chuyển sang Dijkstra lặp lại trên các biểu đồ thưa thớt, tránh hoạt động toàn bộ khối Floyd-Warshall khi biểu đồ chứa tương đối ít cạnh. Các giá trị khoảng cách có thể đạt khoảng (10^{12}) sau khi nhân đôi, điều mà Python xử lý trực tiếp mà không bị tràn. 

## Trường hợp thử nghiệm

Các cạnh đầu ra không phải là duy nhất, do đó, bộ khai thác kiểm tra sẽ xác thực cây được tạo ra thay vì so sánh thứ tự cạnh chính xác. Các thử nghiệm sau đây kiểm tra các mẫu, biểu đồ nhỏ nhất, đường đi có trọng số có tâm nằm trong một cạnh, các trọng số bằng nhau, ranh giới trọng số lớn và biểu đồ dày đặc có kích thước tối đa được tạo.```python
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

def validate(inp: str, out: str, expected_diameter: int):
    data = list(map(int, inp.split()))
    n = data[0]
    m = data[1]

    original = set()
    pos = 2
    for _ in range(m):
        u, v, w = data[pos:pos + 3]
        pos += 3
        if u > v:
            u, v = v, u
        original.add((u, v, w))

    lines = out.strip().splitlines()
    assert len(lines) == n
    assert int(lines[0]) == expected_diameter

    chosen = []
    seen = set()

    for line in lines[1:]:
        u, v = map(int, line.split())
        assert 1 <= u <= n
        assert 1 <= v <= n
        assert u != v

        key_uv = (min(u, v), max(u, v))
        assert any(a == key_uv[0] and b == key_uv[1]
                   for a, b, _ in original)

        assert key_uv not in seen
        seen.add(key_uv)
        chosen.append(key_uv)

    assert len(chosen) == n - 1

    graph = [[] for _ in range(n)]
    for u, v in chosen:
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    visited = [False] * n
    stack = [0]
    visited[0] = True

    while stack:
        u = stack.pop()
        for v in graph[u]:
            if not visited[v]:
                visited[v] = True
                stack.append(v)

    assert all(visited)

sample1 = """\
3 3
1 2 1
2 3 1
3 1 1
"""

sample2 = """\
6 7
1 2 10
2 3 20
1 3 30
3 4 1000
4 5 30
5 6 20
4 6 10
"""

validate(sample1, run(sample1), 2)
validate(sample2, run(sample2), 1060)

case_min = """\
2 1
1 2 7
"""
validate(case_min, run(case_min), 7)

case_internal_center = """\
3 2
1 2 100
2 3 1
"""
validate(case_internal_center, run(case_internal_center), 101)

case_equal = """\
5 10
1 2 1
1 3 1
1 4 1
1 5 1
2 3 1
2 4 1
2 5 1
3 4 1
3 5 1
4 5 1
"""
validate(case_equal, run(case_equal), 2)

case_large_weight = """\
2 1
1 2 1000000000
"""
validate(case_large_weight, run(case_large_weight), 1000000000)

# Maximum-size style test: complete graph on 500 vertices.
# All edges have equal weight, so a star has optimal diameter 2.
n = 500
parts = [f"{n} {n * (n - 1) // 2}"]
for u in range(1, n + 1):
    for v in range(u + 1, n + 1):
        parts.append(f"{u} {v} 1")

case_max = "\n".join(parts) + "\n"
validate(case_max, run(case_max), 2)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Đường kính (2) và hai cạnh tam giác bất kỳ | Nhiều cây tối ưu và khoảng cách bằng nhau | 
| Mẫu 2 | Đường kính (1060) và bất kỳ cây tối ưu hợp lệ nào | Trung tâm bên trong một cạnh nặng | 
| (2) đỉnh, một cạnh có trọng số (7) | Đường kính (7) | Ranh giới kích thước tối thiểu | 
| Đường dẫn có trọng số (100,1) | Đường kính (101) | Trung tâm cạnh bên trong | 
| Biểu đồ hoàn chỉnh với tất cả các trọng số (1) | Đường kính (2) | Nhiều đường đi ngắn nhất bằng nhau và xử lý ràng buộc | 
| Một cạnh trọng lượng (10^9) | Đường kính (10^9) | Khoảng cách số nguyên lớn | 
| Đồ thị hoàn chỉnh với (500) đỉnh | Đường kính (2) | Đầu vào dày đặc kích thước tối đa và tiền xử lý ở quy mô khối | 

## Vỏ cạnh 

Đối với đồ thị hai đỉnh```
2 1
1 2 7
```pha tâm đỉnh cho độ lệch tâm (7), do đó đường kính (7). Pha trung tâm cạnh cũng có thể khám phá điểm giữa của cạnh duy nhất, nhưng ứng cử viên đỉnh đã là tối ưu. Dijkstra cuối cùng có gốc tại đỉnh (1) trả về cạnh duy nhất (1)-(2). 

Đối với đường dẫn có trọng số```
3 2
1 2 100
2 3 1
```cây bao trùm duy nhất có thể có đường kính (101). Đỉnh (2) có độ lệch tâm (100), cho đường kính ứng cử viên là (200), do đó thuật toán chỉ có đỉnh sẽ sai. Quét cạnh xem xét cạnh (1)-(2), tìm giao điểm giữa nhánh kết thúc ở đỉnh (1) và nhánh tới đỉnh (3) và thu được đường kính thực (101). Tâm nằm (49,5) đơn vị tính từ đỉnh (1), hoặc (50,5) đơn vị tính từ đỉnh (2). 

Đối với tam giác cân bằng```
3 3
1 2 1
2 3 1
1 3 1
```mỗi đỉnh có độ lệch tâm (1) nên bán kính nhân đôi là (2) và đường kính là (2). Thứ tự sắp xếp có thể chọn các đỉnh có khoảng cách bằng nhau một cách tùy ý, nhưng việc so sánh chặt chẽ trong quá trình quét cạnh sẽ ngăn các đỉnh có khoảng cách bằng nhau bị coi là thuộc về phía đối diện một cách không chính xác. Cây có đường đi ngắn nhất từ ​​bất kỳ đỉnh nào được chọn có hai cạnh và đường kính (2). 

Đối với trường hợp ranh giới trọng số tối đa```
2 1
1 2 1000000000
```tất cả các tính toán được thực hiện với trọng số gấp đôi (2000000000). Đường kính được lưu trữ cuối cùng là (1000000000) ở thang đo ban đầu. Các số nguyên có độ chính xác tùy ý của Python làm cho phép tính số học trở nên an toàn, trong khi việc triển khai`INF = 10**30`lớn hơn bất kỳ giá trị đường đi ngắn nhất có thể nào. 

Đối với đồ thị dày đặc có kích thước tối đa, đồ thị hoàn chỉnh trên (500) đỉnh có (124750) cạnh. Nếu mọi cạnh đều có trọng số (1) thì mọi đỉnh đều là tâm hợp lệ với độ lệch tâm (1) và cây có đường đi ngắn nhất từ ​​bất kỳ đỉnh nào chỉ đơn giản là một ngôi sao có đường kính (2). Bài kiểm tra thực hiện xử lý trước khoảng cách, cấu trúc sắp xếp (N^2) và quét cạnh dày đặc mà không dựa vào lối tắt biểu đồ thưa thớt.
