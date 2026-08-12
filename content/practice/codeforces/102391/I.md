---
title: "CF 102391I - Cây bao trùm đường kính tối thiểu"
description: "Chúng ta có một đồ thị vô hướng liên thông có các cạnh có độ dài dương. Chúng ta cần giữ chính xác đủ số cạnh để tạo thành cây bao trùm, nhưng mục tiêu không phải là tổng trọng lượng của cây. Thay vào đó, chúng ta muốn đường đi dài nhất bên trong cây kết quả càng ngắn càng tốt."
date: "2026-08-12T05:26:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "I"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 782
verified: false
draft: false
---

[CF 102391I - Cây bao trùm đường kính tối thiểu](https://codeforces.com/problemset/problem/102391/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông có các cạnh có độ dài dương. Chúng ta cần giữ chính xác đủ số cạnh để tạo thành cây bao trùm, nhưng mục tiêu không phải là tổng trọng lượng của cây. Thay vào đó, chúng ta muốn đường đi dài nhất bên trong cây kết quả càng ngắn càng tốt. Đầu ra là đường kính tối thiểu có thể cùng với bất kỳ cây bao trùm nào đạt được nó. Bài toán chính thức cho phép bất kỳ cây tối ưu hợp lệ nào, do đó các cạnh cụ thể được in bằng cách triển khai đúng không nhất thiết phải khớp với đầu ra mẫu. 

Giới hạn (N\le 500) đủ nhỏ để cho phép các thuật toán xoay quanh thời gian bậc ba, nhưng (M) có thể lớn bằng (N(N-1)/2), do đó đồ thị có thể dày đặc. Ở kích thước tối đa có thể có khoảng (125000) cạnh. Việc liệt kê các cây bao trùm là hoàn toàn không thể, bởi vì một đồ thị hoàn chỉnh trên (500) đỉnh có (500^{498}) cây bao trùm khác nhau theo công thức Cayley. Ngay cả việc kiểm tra một cây cũng tốn ít nhất thời gian tuyến tính nếu chúng ta muốn đường kính của nó. Thay vào đó, chúng ta cần một thuật toán đa thức sử dụng thông tin đường đi ngắn nhất. 

Tất cả các độ dài cạnh đều dương và có thể đạt tới (10^9). Một đường dẫn có thể chứa (N-1) cạnh, do đó khoảng cách có thể đạt tới khoảng (5\cdot10^{11}). Số nguyên Python xử lý trực tiếp phạm vi này, nhưng sử dụng loại số nguyên 32 bit sẽ tràn. 

Có ba trường hợp đặc biệt dễ xử lý sai. 

Đầu tiên, tâm tối ưu có thể nằm bên trong một cạnh chứ không phải ở một đỉnh. Coi như```
4 3
1 2 1
2 3 1
3 4 1
```Cây bao trùm duy nhất là đường dẫn (1-2-3-4), có đường kính là (3). Nếu chỉ xét tâm đỉnh thì đỉnh tốt nhất có độ lệch tâm (2), dẫn đến giá trị sai (4). Tâm thật là trung điểm của cạnh (2-3), có bán kính (1,5) nên đường kính đúng là (3). 

Thứ hai, cạnh chứa tâm không nhất thiết phải là đường đi ngắn nhất giữa các điểm cuối của nó. Coi như```
3 3
1 2 1
2 3 1
1 3 100
```Cạnh đắt tiền (1-3) vẫn là một cạnh của biểu đồ và phải được coi là vị trí có thể có của tâm tuyệt đối. Khoảng cách điểm cuối của nó phải được tính bằng cách sử dụng các đường đi ngắn nhất của đồ thị, chứ không phải bằng cách giả sử rằng chính cạnh đã cho là tuyến đường ngắn nhất giữa các điểm cuối của nó. Cây tối ưu là đường dẫn (1-2-3), có đường kính (2). 

Thứ ba, khoảng cách đường đi ngắn nhất bằng nhau có thể xảy ra thường xuyên. TRONG```
3 3
1 2 1
2 3 1
1 3 1
```cả hai đỉnh (2) và (3) được buộc ở khoảng cách (1) tính từ đỉnh (1). Việc triển khai Dijkstra phải cho phép sắp xếp thứ tự ràng buộc tùy ý. Câu trả lời cuối cùng vẫn là đường kính (2) và thuật toán không được phụ thuộc vào một thứ tự cụ thể nào của các đỉnh có khoảng cách bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Liệt kê mọi tập hợp con của các cạnh, giữ lại các tập hợp con chứa chính xác (N-1) cạnh, kiểm tra xem mỗi tập hợp con đó có phải là một cây hay không, tính đường kính của nó và giữ lại giá trị nhỏ nhất. Điều này đúng vì mọi cây bao trùm đều xuất hiện trong bảng liệt kê. Vấn đề là số lượng ứng viên. Trong biểu đồ hoàn chỉnh (K_N), có (N^{N-2}) cây bao trùm, do đó, đối với (N=500), bảng liệt kê đã có (500^{498}) ứng cử viên. Việc tính toán đường kính cho mỗi ứng cử viên sẽ làm cho tổng công việc theo thứ tự (N\cdot N^{N-2}), vượt xa mọi giới hạn thực tế. 

Quan sát hữu ích là một cây có đường kính tối thiểu có tâm hình học rất cụ thể. Hãy tưởng tượng việc thay thế mọi cạnh của đồ thị bằng một đoạn thẳng liên tục có cùng độ dài. Một điểm trên mạng liên tục này có thể là một đỉnh hoặc một điểm trong của một cạnh. Đối với một điểm (x) như vậy, hãy xác định bán kính của nó là khoảng cách đường đi ngắn nhất tối đa từ (x) đến bất kỳ đỉnh nào của đồ thị. 

Lấy bất kỳ cây bao trùm (T) nào và nhìn vào điểm giữa của một trong các đường kính của nó. Điểm giữa đó là đỉnh cây hoặc nằm bên trong cạnh cây. Mỗi đỉnh của cây cách cây tối đa một nửa đường kính tính từ điểm giữa đó. Đường đi ngắn nhất của đồ thị chỉ có thể ngắn hơn đường đi của cây, do đó, cùng một điểm có khoảng cách đồ thị nhiều nhất là một nửa đường kính cây đến mọi đỉnh. Do đó, mỗi cây bao trùm có đường kính (D) cho một điểm mạng có bán kính nhiều nhất là (D/2). 

Điều ngược lại là chìa khóa. Nếu (x) là một điểm của mạng ban đầu có khoảng cách đồ thị tối đa đến một đỉnh là (R), hãy xây dựng cây đường đi ngắn nhất bắt nguồn từ (x), coi điểm cạnh trong là đỉnh chia nhỏ. Mỗi khoảng cách từ gốc đến đỉnh trong cây đó lớn nhất là (R), do đó mỗi cặp đỉnh có khoảng cách cây tối đa là (2R). Do đó, đường kính cây bao trùm tối ưu chính xác gấp đôi bán kính tối thiểu có thể có của một điểm mạng như vậy. 

Điều này chuyển đổi vấn đề tối ưu hóa cây ban đầu thành vấn đề 1 tâm tuyệt đối trên biểu đồ có trọng số. Sự tương đương này là đặc tính tiêu chuẩn của cây bao trùm có đường kính tối thiểu. 

Chỉ có hai loại trung tâm khả thi. Tâm có thể là một đỉnh ban đầu, trong trường hợp đó bán kính của nó đơn giản là độ lệch tâm của nó. Hoặc nó có thể nằm đâu đó bên trong một cạnh ban đầu. Chúng ta có thể kiểm tra trực tiếp tất cả các đỉnh, nhưng việc kiểm tra một cạnh đòi hỏi phải cẩn thận hơn. 

Đối với một cạnh (u-v) có chiều dài (w), gọi (x) là một điểm ở khoảng cách (\alpha) từ (u), trong đó (0\le\alpha\le w). Với mọi đỉnh (z), 

[ 
d(x,z)=\min(\alpha+d(u,z),,w-\alpha+d(v,z)). 
] 

Biểu thức đầu tiên mô tả các đường đi đến (z) qua (u), trong khi biểu thức thứ hai mô tả các đường đi đến nó qua (v). Bán kính của (x) là giá trị lớn nhất của các giá trị này trên tất cả (z). 

Mỗi đỉnh đóng góp một hàm hình chữ V ngược của (\alpha). Đường bao trên của tất cả các hàm này chính xác là hàm bán kính dọc theo cạnh. Chúng ta cần điểm thấp nhất của nó. Quá trình quét Kariv-Hakimi tìm thấy tất cả các thung lũng có liên quan của đường bao trên này theo thời gian tuyến tính sau khi khoảng cách tất cả các cặp và thứ tự Dijkstra đã được tính toán. 

Đối với cạnh cố định (u-v), sắp xếp các đỉnh theo mức tăng (d(u,z)). Bắt đầu từ đỉnh xa nhất (u). Khi chúng ta quét ngược các đỉnh còn lại, một đỉnh mới chỉ quan trọng khi nó ở xa (v) hơn đỉnh hiện đang hoạt động. Sự thay đổi như vậy tạo ra sự giao thoa mới giữa hàm hiện có liên quan ở phía (u) và hàm mới ở phía (v). Nếu hai đỉnh liên quan là (p) và (q) thì đường giao nhau có bán kính 

[ 
R=\frac{d(u,p)+w+d(v,q)}{2}. 
] 

Vì câu trả lời bắt buộc là đường kính nên sẽ thuận tiện khi lưu trữ bán kính gấp đôi: 

[ 
D= d(u,p)+w+d(v,q). 
] 

Điều này tránh mọi phép tính dấu phẩy động.

Do đó, thuật toán hoàn chỉnh là tính toán đường đi ngắn nhất cho tất cả các cặp, sau đó là kiểm tra tâm đỉnh và quét Kariv-Hakimi tuyến tính cho mọi cạnh của đồ thị. Cuối cùng, khi đã biết được trung tâm tốt nhất, một phép tính Dijkstra nữa sẽ xây dựng cây đường đi ngắn nhất có gốc tại trung tâm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N\cdot N^{N-2})) | (O(N^2)) | Quá chậm | 
| Tối ưu | (O(NM\log N + NM)) với heap Dijkstra | (O(N^2+M)) | Đã chấp nhận | 

Đối với (N\le500) đã nêu, đây là cách tiếp cận đa thức chính xác tiêu chuẩn. Công thức lý thuyết của phương pháp trung tâm tuyệt đối thường được đưa ra là (O(MN+N^2\log N)) khi có sẵn ma trận đường đi ngắn nhất, với các đường dẫn ngắn nhất tất cả các cặp cung cấp các phép tính nguồn đơn (N) bổ sung. 

## Hướng dẫn thuật toán 

1. Chạy Dijkstra từ mọi đỉnh. Lưu trữ cả khoảng cách ngắn nhất (d[s][v]) và thứ tự mà các đỉnh được Dijkstra hoàn thiện vĩnh viễn từ (s). 

Thứ tự hoàn thiện được sắp xếp theo khoảng cách không giảm từ (s). Đó chính xác là thứ tự cần thiết cho quá trình quét cạnh Kariv-Hakimi. 

1. Với mỗi đỉnh (c), hãy tính 

[ 
D_c=2\max_v d[c][v]. 
] 

Giữ giá trị nhỏ nhất và ghi nhớ (c) là trung tâm tốt nhất hiện tại. 

Nếu tâm tuyệt đối tối ưu là một đỉnh của đồ thị thì điều này đã tìm thấy nó. Hệ số hai là có chủ ý vì đường kính cây bao trùm cuối cùng gấp đôi bán kính tâm. 

1. Với mọi cạnh đồ thị (u-v) có độ dài (w), hãy sử dụng thứ tự Dijkstra từ (u). Hãy để trật tự đó được 

[ 
r_0,r_1,\ldots,r_{N-1}, 
] 

trong đó (r_{N-1}) cách xa (u nhất). 

Khởi tạo (a=N-1). Sau đó quét (b=N-2,N-3,\ldots,0). Bất cứ khi nào 

[ 
d[v][r_b] > d[v][r_a], 
] 

hai đường bao hiện có liên quan tạo thành một thung lũng ứng cử viên mới. 

Bán kính nhân đôi tương ứng là 

[ 
D=d[u][r_b]+w+d[v][r_a]. 
] 

Nếu giá trị này cải thiện câu trả lời hiện tại, hãy nhớ cạnh này và hai đỉnh (r_b) và (r_a). 

Lý do của sự so sánh chặt chẽ là chỉ có một giá trị mới, lớn hơn ở phía (v) mới thay đổi đường bao trên. Các giá trị bằng nhau không thể tạo đường giao nhau thấp hơn chưa được biểu thị bằng đường hiện hoạt. 

1. Sau khi kiểm tra tất cả các đỉnh và cạnh, chúng ta biết bán kính nhân đôi tối thiểu (D^*). 

Nếu tâm tốt nhất là đỉnh (c), chúng ta sẽ xây dựng cây đường đi ngắn nhất thông thường có gốc tại (c). 

Nếu tâm tốt nhất nằm trên cạnh (u-v), hãy đặt (p=r_b) và (q=r_a) là hai đỉnh được ghi lại khi tìm thấy ứng cử viên cạnh đó. Vị trí giao cắt thỏa mãn 

[ 
\alpha+d[u][p]=w-\alpha+d[v][q]. 
] 

Nhân với hai cho 

[ 
2\alpha=w+d[v][q]-d[u][p]. 
] 

Chúng tôi lưu trữ giá trị số nguyên này thay vì sử dụng dấu phẩy động. 

1. Để xây dựng cây cho tâm đỉnh, hãy chạy Dijkstra từ đỉnh đó và giữ lại đỉnh trước của mỗi đỉnh. Mỗi đỉnh không phải gốc đều đóng góp cạnh trước nó. 

Một cây có đường đi ngắn nhất chính xác là những gì chúng ta muốn bởi vì mỗi đỉnh đều có khoảng cách đồ thị tối đa bằng bán kính tâm tính từ gốc, do đó, bất kỳ hai đỉnh cây nào cũng được nối qua gốc bằng một đường đi có độ dài tối đa gấp đôi bán kính đó. 

1. Để xây dựng cây cho tâm cạnh, hãy chia nhỏ (u-v) bằng cách chèn tâm mới (x). Khoảng cách từ (x) đến (u) và (v) là (\alpha) và (w-\alpha). 

Trong quá trình triển khai, chúng tôi tránh tạo đỉnh mới. Chúng tôi khởi tạo Dijkstra với khoảng cách dự kiến ​​gấp đôi 

[ 
2\alpha 
] 

cho (u) và 

[ 
2w-2\alpha 
] 

cho (v). Khi đó các cạnh của đồ thị thông thường có chiều dài gấp đôi (2w_e). 

Hai đỉnh ban đầu được phép thả lỏng sau đó. Chi tiết này quan trọng vì cạnh được chọn (u-v) không nhất thiết phải là đường đi ngắn nhất giữa (u) và (v). Nếu có thể tiếp cận một điểm cuối với chi phí rẻ hơn thông qua phần còn lại của biểu đồ thì Dijkstra phải được phép khám phá điều đó.

1. Biểu đồ tiền thân do Dijkstra đa nguồn này tạo ra là một khu rừng bắt nguồn từ các đỉnh đạt trực tiếp từ trung tâm nhân tạo. Nếu cả hai (u) và (v) vẫn là gốc, hãy thêm cạnh ban đầu (u-v). Nếu chỉ còn lại một cạnh gốc thì cạnh ban đầu là không cần thiết. 

Đồ thị kết quả có chính xác (N-1) cạnh và là cây bao trùm. Khoảng cách từ gốc đến đỉnh của nó bằng khoảng cách ngắn nhất từ ​​tâm đã chọn, do đó đường kính của nó tối đa là (D^_). Vì không có cây bao trùm nào có thể có đường kính dưới (D^_), nên đường kính của nó chính xác là tối ưu. 

### Tại sao nó hoạt động 

Gọi (R^_) là khoảng cách tối đa tối thiểu từ một điểm của đồ thị liên tục đến tất cả các đỉnh ban đầu. Đối với mọi cây bao trùm (T) có đường kính (D), điểm giữa của đường kính tối đa ở khoảng cách cây (D/2) tính từ mọi đỉnh và khoảng cách đồ thị không được vượt quá khoảng cách cây. Do đó (R^_\le D/2), cho (D\ge2R^*). 

Ngược lại, lấy tâm tuyệt đối (x) có bán kính (R^_). Cây đường đi ngắn nhất có gốc tại (x) cho phép mỗi đỉnh có khoảng cách tối đa là (R^_) từ (x). Khoảng cách của cây giữa hai đỉnh bất kỳ nhiều nhất là tổng khoảng cách của chúng đến (x), do đó đường kính của nó nhiều nhất là (2R^_). Hai bất đẳng thức gặp nhau, chứng tỏ đường kính cây bao trùm tối ưu là (2R^_). 

Tâm tuyệt đối nằm ở một đỉnh hoặc bên trong một cạnh. Trung tâm Vertex được kiểm tra trực tiếp. Trên mỗi cạnh, bán kính là đường bao trên của các hàm khoảng cách đỉnh và phép quét Kariv-Hakimi kiểm tra chính xác các điểm mà đường bao này có thể đạt mức tối thiểu cục bộ. Do đó, ứng cử viên nhỏ nhất được tìm thấy trong quá trình quét là (R^_). Cây có đường đi ngắn nhất cuối cùng sẽ đạt được đường kính (2R^_), do đó cây được in là tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def dijkstra(source, graph, n):
    dist = [INF] * n
    parent = [-1] * n
    order = []

    dist[source] = 0
    pq = [(0, source)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        order.append(u)

        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                parent[v] = u
                heapq.heappush(pq, (nd, v))

    return dist, parent, order

def dijkstra_center(graph, n, u, v, alpha2, w):
    """
    Dijkstra from an artificial center x lying on edge u-v.

    All distances are doubled, so alpha2 = 2 * distance(x, u).
    The initial distances are:
        dist2[u] = alpha2
        dist2[v] = 2*w - alpha2

    Unlike ordinary multi-source Dijkstra, u and v are allowed to
    be relaxed later. This is necessary because u-v itself need not
    be a shortest path between u and v.
    """
    dist = [INF] * n
    parent = [-1] * n

    dv = 2 * w - alpha2
    dist[u] = alpha2
    dist[v] = dv

    pq = [(alpha2, u)]
    if v != u:
        heapq.heappush(pq, (dv, v))

    used = [False] * n

    while pq:
        d, x = heapq.heappop(pq)
        if used[x] or d != dist[x]:
            continue

        used[x] = True

        for y, ew in graph[x]:
            nd = d + 2 * ew
            if nd < dist[y]:
                dist[y] = nd
                parent[y] = x
                heapq.heappush(pq, (nd, y))

    tree = []

    for x in range(n):
        if parent[x] != -1:
            tree.append((x, parent[x]))

    # If both endpoints are roots of the shortest-path forest,
    # the artificial center connects to both, which corresponds
    # to using the original edge u-v.
    if parent[u] == -1 and parent[v] == -1 and u != v:
        tree.append((u, v))

    return tree

def dijkstra_tree(graph, n, source):
    dist = [INF] * n
    parent = [-1] * n

    dist[source] = 0
    pq = [(0, source)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                parent[v] = u
                heapq.heappush(pq, (nd, v))

    return [(v, parent[v]) for v in range(n) if parent[v] != -1]

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]
    edges = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, w))
        graph[v].append((u, w))
        edges.append((u, v, w))

    # All-pairs shortest paths and Dijkstra finalization orders.
    dist = [[0] * n for _ in range(n)]
    orders = [None] * n

    for s in range(n):
        ds, _, order = dijkstra(s, graph, n)
        dist[s] = ds
        orders[s] = order

    # Best center found so far.
    best2 = INF
    best_type = 0          # 0 = vertex, 1 = edge
    best_vertex = -1

    for s in range(n):
        cur = 2 * max(dist[s])
        if cur < best2:
            best2 = cur
            best_type = 0
            best_vertex = s

    best_edge = None

    # Kariv-Hakimi sweep on every edge.
    for u, v, w in edges:
        r = orders[u]

        a = n - 1

        for b in range(n - 2, -1, -1):
            x = r[b]
            y = r[a]

            if dist[v][x] > dist[v][y]:
                candidate2 = dist[u][x] + w + dist[v][y]

                if candidate2 < best2:
                    best2 = candidate2
                    best_type = 1
                    best_edge = (u, v, w, x, y)

                a = b

    # Construct an optimal shortest-path tree.
    if best_type == 0:
        tree = dijkstra_tree(graph, n, best_vertex)
    else:
        u, v, w, p, q = best_edge

        # 2 * alpha = w + d(v,q) - d(u,p)
        alpha2 = w + dist[v][q] - dist[u][p]

        tree = dijkstra_center(graph, n, u, v, alpha2, w)

    out = [str(best2)]

    for u, v in tree:
        out.append(f"{u + 1} {v + 1}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp Dijkstra đầu tiên tính toán đồng thời hai mẩu thông tin. Mảng khoảng cách cung cấp ma trận đường đi ngắn nhất tất cả các cặp, trong khi mảng thứ tự ghi lại các đỉnh trong khoảng cách không giảm từ nguồn. Vì mỗi trọng số của cạnh đều dương nên thứ tự mà Dijkstra trích xuất vĩnh viễn các đỉnh là thứ tự khoảng cách hợp lệ. 

Vòng lặp tâm đỉnh sử dụng (2\cdot\max d[c][v]) vì câu trả lời thực tế là đường kính chứ không phải bán kính. Giữ mọi thứ nhân đôi cũng làm cho tích phân số học cạnh-tâm sau này. 

Vòng lặp cạnh là phần nhỏ gọn của thuật toán Kariv-Hakimi. Đối với một cạnh (u-v),`r`được sắp xếp theo khoảng cách từ (u). Biến`a`xác định đường xa nhất hiện đang hoạt động ở phía đối diện, trong khi`b`quét các dòng có thể có thể thay thế nó. Khi`dist[v][r[b]]`trở nên lớn hơn nghiêm ngặt so với`dist[v][r[a]]`, hai đường tạo thành một giao điểm mới có liên quan, có chiều cao gấp đôi chính xác```
dist[u][r[b]] + w + dist[v][r[a]]
```Mã xây dựng lại sử dụng khoảng cách gấp đôi. Nếu tâm cách (\alpha) từ (u), hai khoảng cách ban đầu tới tâm nhân tạo là (\alpha) và (w-\alpha). Nhân tất cả khoảng cách với 2 sẽ cho giá trị ban đầu là số nguyên, do đó không cần so sánh dấu phẩy động. 

Việc tái thiết Dijkstra cố tình không đánh dấu vĩnh viễn (u) và (v) là nguồn bất biến. Một cạnh đắt tiền có thể có tuyến đường thay thế ngắn hơn ở những nơi khác trong biểu đồ, do đó, một trong những điểm cuối đó có thể không còn là con trực tiếp của trung tâm nhân tạo. Biểu đồ tiền thân vẫn là một khu rừng vì mọi đồ thị tiền nhiệm chỉ được chỉ định thông qua việc cải thiện khoảng cách nghiêm ngặt. Nếu cả hai điểm cuối vẫn là gốc thì tâm nhân tạo sẽ sử dụng cả hai nửa của cạnh được chọn, do đó hai gốc đó được nối với nhau bằng cạnh ban đầu. 

Số nguyên có độ chính xác tùy ý của Python loại bỏ mối lo ngại về tràn. Khoảng cách phù hợp lớn nhất nằm ở bên dưới (5\cdot10^{11}), trong khi khoảng cách nhân đôi vẫn ở mức thoải mái ở bên dưới (10^{12}). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác có cả ba cạnh dài (1). Mọi đỉnh đều có độ lệch tâm (1), do đó tâm của đỉnh đã có bán kính gấp đôi (2). 

| Ứng viên trung tâm | Bán kính | Bán kính nhân đôi | 
| --- | --- | --- | 
| Đỉnh 1 | 1 | 2 | 
| Đỉnh 2 | 1 | 2 | 
| Đỉnh 3 | 1 | 2 | 

Việc quét cạnh không thể cải thiện giá trị này. Thuật toán có thể chọn đỉnh (1), sau đó Dijkstra tạo ra cây đường đi ngắn nhất như (1-2) và (1-3). 

Cây kết quả có đường kính (2), phù hợp với mức tối ưu được hiển thị trong mẫu. 

### Mẫu 2 

Biểu đồ có cụm bên trái được kết nối qua cạnh dài (3-4) có chiều dài (1000) đến cụm bên phải. 

Ứng cử viên quan trọng là phần bên trong của cạnh (3-4). Phía bên trái đến đỉnh (1) qua khoảng cách (30) từ (3), trong khi phía bên phải đến đỉnh (6) qua khoảng cách (30) từ (4). 

Do đó, đường giao nhau có liên quan có bán kính gấp đôi 

[ 
30+1000+30=1060. 
] 

| Ứng viên | Đóng góp còn lại | Cạnh | Đóng góp đúng đắn | Đường kính | 
| --- | --- | --- | --- | --- | 
| Tâm trên cạnh (3-4) | 30 | 1000 | 30 | 1060 | 

Tâm là trung điểm của cạnh (3-4). Cây có đường đi ngắn nhất nối các đỉnh bên trái qua (3) và các đỉnh bên phải qua (4), cho ra một cây có đường kính là (1060). 

Đầu ra mẫu sử dụng chính xác cạnh trung tâm này và báo cáo đường kính (1060). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NM\log N + NM)) | (N) Dijkstra chạy cộng với một lần quét (O(N)) cho mọi cạnh | 
| Không gian | (O(N^2+M)) | Khoảng cách tất cả các cặp, thứ tự Dijkstra và danh sách kề | 

Với (N\le500), ma trận khoảng cách chỉ cần (250000) mục nhập. Biểu đồ có thể chứa khoảng (125000) cạnh, do đó, danh sách kề cũng có thể quản lý được trong giới hạn bộ nhớ (1024) MB. Bản thân quá trình quét cạnh thực hiện tối đa (N) thao tác đơn giản trên mỗi cạnh, tức là khoảng (6,25\cdot10^7) lần lặp ở mật độ tối đa. Giai đoạn đường đi ngắn nhất chiếm ưu thế trong thời gian chạy. 

Công thức trung tâm tuyệt đối chính xác tiêu chuẩn là đa thức và dựa trên các đường đi ngắn nhất của tất cả các cặp, sau đó xử lý mọi cạnh. 

## Trường hợp thử nghiệm 

Cây đầu ra không phải là duy nhất nên bộ khai thác kiểm tra không được so sánh toàn bộ chuỗi đầu ra với một câu trả lời cố định. Thay vào đó, nó kiểm tra đường kính được báo cáo, xác minh rằng mọi cạnh được in đều thuộc về biểu đồ đầu vào, xác minh rằng có chính xác (N-1) cạnh và kiểm tra xem các cạnh đó có tạo thành một cây có đường kính trọng số thực tế bằng với mức tối ưu dự kiến ​​hay không.```python
import sys
import io
from collections import deque
import heapq

# Put the submitted solution in the same file above this harness.
# The function solve() must be the solution entry point.

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
    it = iter(data)

    n = next(it)
    m = next(it)

    edge_weight = {}
    graph = [[] for _ in range(n)]

    for _ in range(m):
        u = next(it) - 1
        v = next(it) - 1
        w = next(it)

        edge_weight[frozenset((u, v))] = w
        graph[u].append((v, w))
        graph[v].append((u, w))

    lines = out.strip().splitlines()
    assert len(lines) == n

    diameter = int(lines[0])
    assert diameter == expected_diameter

    tree = []
    for line in lines[1:]:
        u, v = map(int, line.split())
        u -= 1
        v -= 1

        assert 0 <= u < n
        assert 0 <= v < n
        assert u != v

        key = frozenset((u, v))
        assert key in edge_weight

        tree.append((u, v, edge_weight[key]))

    assert len(tree) == n - 1

    # Check that the output is a tree.
    parent = list(range(n))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    for u, v, _ in tree:
        ru = find(u)
        rv = find(v)
        assert ru != rv
        parent[ru] = rv

    root = find(0)
    for v in range(n):
        assert find(v) == root

    # Compute the actual diameter of the printed tree.
    tg = [[] for _ in range(n)]
    for u, v, w in tree:
        tg[u].append((v, w))
        tg[v].append((u, w))

    actual = 0

    for s in range(n):
        dist = [-1] * n
        dist[s] = 0
        q = deque([s])

        while q:
            u = q.popleft()
            for v, w in tg[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + w
                    q.append(v)

        actual = max(actual, max(dist))

    assert actual == expected_diameter

# Sample 1
sample1 = """\
3 3
1 2 1
2 3 1
3 1 1
"""
validate(sample1, run(sample1), 2)

# Sample 2
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
validate(sample2, run(sample2), 1060)

# Minimum-size graph.
case_min = """\
2 1
1 2 7
"""
validate(case_min, run(case_min), 7)

# Four-vertex path. The optimum center is inside an edge,
# so a vertex-only solution would incorrectly report 4.
case_edge_center = """\
4 3
1 2 1
2 3 1
3 4 1
"""
validate(case_edge_center, run(case_edge_center), 3)

# All edge weights equal. The triangle has a vertex center,
# and every spanning tree has diameter 10.
case_equal = """\
3 3
1 2 5
2 3 5
1 3 5
"""
validate(case_equal, run(case_equal), 10)

# Maximum-size dense input, all weights equal.
# A star has diameter 2 and is optimal.
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
| Mẫu 1 | Đường kính (2) | Tâm đỉnh và các mối quan hệ có khoảng cách bằng nhau | 
| Mẫu 2 | Đường kính (1060) | Tâm cạnh bên trong trên một cạnh có trọng lượng dài | 
| (2) đỉnh, một cạnh có trọng số (7) | Đường kính (7) | Ranh giới kích thước tối thiểu | 
| Đường dẫn (1-2-3-4), trọng lượng đơn vị | Đường kính (3) | Nắm bắt các giải pháp chỉ kiểm tra tâm đỉnh | 
| Tam giác cân bằng có trọng lượng (5) | Đường kính (10) | Khoảng cách bằng nhau và xử lý đỉnh-tâm | 
| Đồ thị hoàn chỉnh trên (500) đỉnh, tất cả trọng số (1) | Đường kính (2) | Tối đa (N), tối đa (M), đồ thị dày đặc, trọng số bằng nhau | 

## Vỏ cạnh 

Trường hợp kích thước tối thiểu chỉ có hai đỉnh:```
2 1
1 2 7
```Có chính xác một cây khung bao gồm cạnh duy nhất nên đáp án phải là (7). Pha trung tâm đỉnh cung cấp độ lệch tâm (7) cho điểm cuối và bán kính nhân đôi (14), nhưng đường kính cây thực tế là (7), điều này cho thấy có vấn đề khi xử lý (2R) một cách mù quáng đối với (N=2). Bán kính tâm tuyệt đối thực tế là (3,5), đạt được tại điểm giữa của cạnh, do đó việc quét cạnh sẽ tìm thấy ứng cử viên (7). Đây là lý do tại sao việc kiểm tra tâm cạnh bên trong là cần thiết ngay cả đối với đồ thị nhỏ nhất. 

Đối với đường đi bốn đỉnh```
4 3
1 2 1
2 3 1
3 4 1
```độ lệch tâm của đỉnh là (3,2,2,3). Ứng cử viên đỉnh tốt nhất có bán kính gấp đôi (4). Trên cạnh (2-3), các đỉnh xa nhất có liên quan là (1) và (4), cho 

[ 
1+1+1=3. 
] 

Quét cạnh ghi lại đường kính (3) và việc tái thiết đặt tâm nhân tạo ở điểm giữa của (2-3). Cây kết quả nhất thiết phải là đường dẫn ban đầu, có đường kính là (3). 

Đối với biểu đồ chứa một cạnh không phải là đường đi ngắn nhất giữa các điểm cuối của nó,```
3 3
1 2 1
2 3 1
1 3 100
```ma trận đường đi ngắn nhất cho (d(1,3)=2), mặc dù cạnh trực tiếp có trọng số (100). Tính toán trung tâm sử dụng những khoảng cách đường đi ngắn nhất này ở mọi nơi. Cạnh đắt tiền vẫn được xem xét như một vị trí trung tâm có thể, nhưng nó không thể đánh bại tâm ở đỉnh (2). Đường kính cuối cùng là (2), có cạnh cây (1-2) và (2-3). 

Với khoảng cách bằng nhau,```
3 3
1 2 1
2 3 1
1 3 1
```Dijkstra có thể hoàn thiện các đỉnh theo các thứ tự khác nhau tùy thuộc vào hành vi liên kết đống. Việc quét cạnh chỉ sử dụng thứ tự khoảng cách không giảm và các cải tiến nghiêm ngặt ở phía đối diện. Các giá trị bằng nhau không yêu cầu quy tắc ràng buộc cụ thể. Các ứng viên có tâm đỉnh đã cho bán kính gấp đôi (2), do đó thuật toán trả về đường kính (2). 

Đối với trường hợp dày đặc tối đa, đồ thị có thể chứa tất cả (124750) cạnh có thể có khi (N=500). Nếu mọi cạnh đều có trọng số (1), việc chọn bất kỳ đỉnh nào làm tâm sẽ cho bán kính (1), do đó đường kính tối ưu là (2). Thuật toán vẫn xử lý tất cả các cạnh trong quá trình quét Kariv-Hakimi, nhưng mọi ứng cử viên đều không tốt hơn giá trị đỉnh-trung tâm. Trường hợp này thực hiện cả ranh giới đầu vào (M=\Theta(N^2)) và hành vi khoảng cách bằng nhau của thứ tự đường đi ngắn nhất. 

Cần lưu ý một điều chỉnh khi triển khai bài xã luận này: khai thác thử nghiệm có chủ ý xác thực _properties_ của đầu ra thay vì so sánh danh sách cạnh, bởi vì Codeforces chấp nhận bất kỳ cây bao trùm có đường kính tối thiểu nào.
