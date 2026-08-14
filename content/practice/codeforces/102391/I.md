---
title: "CF 102391I - Cây bao trùm đường kính tối thiểu"
description: "Chúng ta cần chọn chính xác (N-1) cạnh từ một đồ thị vô hướng có trọng số được kết nối sao cho mọi đỉnh đều có thể tiếp cận được và đồ thị thu được là một cây. Trong số tất cả các cây bao trùm như vậy, chúng ta muốn một cây có đường đi có trọng số dài nhất càng ngắn càng tốt."
date: "2026-08-14T14:05:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "I"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 413
verified: false
draft: false
---

[CF 102391I - Cây bao trùm đường kính tối thiểu](https://codeforces.com/problemset/problem/102391/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn chính xác (N-1) cạnh từ một đồ thị vô hướng có trọng số được kết nối sao cho mọi đỉnh đều có thể tiếp cận được và đồ thị thu được là một cây. Trong số tất cả các cây bao trùm như vậy, chúng ta muốn một cây có đường đi có trọng số dài nhất càng ngắn càng tốt. Đầu ra phải chứa đường kính tối thiểu có thể có đó và các cạnh của một cây đạt được đường kính đó. Câu lệnh cho phép bất kỳ cây tối ưu nào, vì vậy các kết quả đầu ra chính xác khác nhau có thể chứa các cạnh khác nhau. 

Khó khăn chính là việc giảm thiểu đường kính không giống như giảm thiểu tổng trọng lượng của cạnh. Cây bao trùm tối thiểu có thể có nhánh rất dài ngay cả khi biểu đồ gốc chứa các tuyến đường ngắn sẽ có đường kính nhỏ hơn nhiều. 

Với (N\le 500), thời gian khối là mục tiêu tự nhiên. Tính toán đường đi ngắn nhất đầy đủ cho tất cả các cặp mất (O(N^3)), tức là khoảng (125) triệu cập nhật khoảng cách cơ bản ở kích thước tối đa. Việc liệt kê các cây bao trùm là hoàn toàn không thể, vì một đồ thị hoàn chỉnh trên (N) đỉnh đã có sẵn (N^{N-2}) cây bao trùm. Giới hạn (M\le N(N-1)/2) cũng có nghĩa là chúng ta phải thoải mái với các đồ thị dày đặc, do đó, thuật toán có số hạng chính là (O(NM)) vẫn là (O(N^3)) trong trường hợp xấu nhất. 

Có ba trường hợp mà một giải pháp bất cẩn có thể bỏ sót. 

Đầu tiên, tâm tối ưu không nhất thiết phải là đỉnh ban đầu. Coi như```
3 2
1 2 2
2 3 4
```Cây bao trùm duy nhất là chính đồ thị, vì vậy đường kính chính xác là (6), sử dụng các cạnh (1\ 2) và (2\ 3). Tâm của nó là một đơn vị bên trong cạnh (2\ 3), không phải ở đỉnh (2) hoặc đỉnh (3). Chỉ nhìn vào tâm đỉnh sẽ thu được (8) không chính xác, vì độ lệch tâm của đỉnh (2) là (4). 

Thứ hai, cạnh của đồ thị được đồ thị gốc sử dụng không nhất thiết phải thuộc về cây bao trùm tối ưu. Ví dụ,```
4 6
1 2 1
2 3 1
3 4 1
1 4 100
1 3 10
2 4 10
```Cây (1-2-3-4) có đường kính (3), là tối ưu vì mọi cây khung phải nối các đỉnh (1) và (4), đồng thời đồ thị chứa ba cạnh đơn vị tạo thành đường dẫn này. Việc xây dựng bất cẩn thiên về cạnh (1-4) vì nó kết nối trực tiếp với các điểm cuối có thể tạo ra một cây tồi tệ hơn nhiều. 

Thứ ba, một số đường đi ngắn nhất có thể có cùng độ dài. Vì```
3 3
1 2 1
2 3 1
1 3 1
```đường kính đúng là (2) và cả hai cây ({1-2,2-3}), ({1-2,1-3}) và ({1-3,2-3}) đều là câu trả lời hợp lệ. Việc thực hiện không được phụ thuộc vào một trình tự ràng buộc cụ thể nào. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi tập hợp con của các cạnh đồ thị (N-1), kiểm tra xem tập hợp con đó có phải là cây bao trùm hay không, sau đó tính đường kính của nó. có 

[ 
\binom{M}{N-1} 
] 

các tập hợp con như vậy. Việc kiểm tra một tập hợp con đã tốn ít nhất (O(N+M)) nếu chúng ta sử dụng phương pháp truyền tải đồ thị để kiểm tra khả năng kết nối, vì vậy công việc trong trường hợp xấu nhất là 

[ 
O\left(\binom{M}{N-1}(N+M)\right). 
] 

Đối với (N=500) và một biểu đồ hoàn chỉnh, (M=124750), làm cho biểu đồ này lớn hơn về mặt thiên văn so với bất kỳ số lượng hoạt động khả thi nào. Lực lượng vũ phu là chính xác vì nó kiểm tra mọi cây bao trùm có thể theo đúng nghĩa đen, nhưng số lượng ứng cử viên là số lượng sai để liệt kê. 

Quan sát hữu ích là mỗi cây đều có một trung tâm. Đi theo con đường dài nhất của cây và nhìn vào điểm giữa của nó. Điểm giữa là một đỉnh hiện có hoặc nằm đâu đó bên trong một cạnh. Nếu điểm giữa là một đỉnh (c), thì mọi đỉnh đều cách cây tối đa một nửa đường kính tính từ (c). Vì đường đi ngắn nhất của đồ thị chỉ có thể ngắn hơn đường đi của cây nên độ lệch tâm của đồ thị (c) cũng nhiều nhất bằng một nửa đường kính cây. 

Điều này ngay lập tức kết nối vấn đề với cây đường đi ngắn nhất. Nếu chúng ta chọn một đỉnh (c) và xây dựng cây đường đi ngắn nhất bắt nguồn từ (c), mọi khoảng cách từ gốc đến đỉnh sẽ trở thành khoảng cách đồ thị. Đường kính của nó nhiều nhất là gấp đôi độ lệch tâm của (c). Nếu (c) là tâm của cây tối ưu thì giới hạn này đạt mức tối ưu. 

Sự phức tạp là trường hợp thứ hai, trong đó tâm nằm bên trong một cạnh. Giải pháp tiêu chuẩn xử lý vấn đề này bằng cách coi một điểm tùy ý trên một cạnh là một đỉnh giả tạm thời. Độ lệch tâm tối thiểu có thể có trên tất cả các điểm như vậy là tâm 1 tuyệt đối của đồ thị và cây đường đi ngắn nhất bắt nguồn từ tâm đó sẽ cho ra cây bao trùm có đường kính tối thiểu. Sự tương đương này là phép rút gọn cổ điển đằng sau nghiệm (O(NM+N^3)). 

Đối với cạnh (e=(u,v)) có trọng số (w), giả sử tâm giả cách (x) đơn vị tính từ (u). Đối với mọi đỉnh (z), khoảng cách của nó tới tâm là 

[ 
\min(x+d(u,z),\ w-x+d(v,z)). 
] 

Đối với (z cố định), đây là hàm hình chữ V của (x), có độ dốc (+1) và (-1). Giá trị lớn nhất trên tất cả các đỉnh là đường bao trên của các hàm hình chữ V này. Mức tối thiểu của đường bao đó xảy ra ở điểm cuối, điểm này đã được bao phủ bởi các trường hợp tâm đỉnh hoặc tại giao điểm giữa hai hàm liên quan liên tiếp. 

Đối với một cạnh, sắp xếp tất cả các đỉnh theo (d(u,z)) theo thứ tự giảm dần. Trong khi quét thứ tự này, chỉ giữ lại các đỉnh có (d(v,z)) tăng nghiêm ngặt. Đây chính xác là các đỉnh có thể xuất hiện ở đường bao trên. Nếu các đỉnh được giữ liên tiếp là (a) và (b) thì hai đường thẳng liên quan cắt nhau tại 

[ 
x=\frac{w-d(u,a)+d(v,b)}{2}, 
] 

và độ lệch tâm tương ứng là 

[ 
\frac{w+d(u,a)+d(v,b)}{2}. 
] 

Do đó, đường kính ứng cử viên chỉ đơn giản là 

[ 
w+d(u,a)+d(v,b). 
] 

Điều này biến việc tối ưu hóa liên tục dọc theo một cạnh thành quét tuyến tính các cặp khoảng cách (N). 

Floyd-Warshall có thể lấy được khoảng cách yêu cầu của tất cả các cặp. Chúng tôi cũng tính toán trước thứ tự khoảng cách cho mọi điểm cuối có thể một lần, thay vì sắp xếp riêng cho từng cạnh của biểu đồ. Tổng độ phức tạp trở thành (O(N^3+NM+N^2\log N)), tức là (O(N^3)) tại các giới hạn đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\binom{M}{N-1}(N+M))) | (O(N+M)) | Quá chậm | 
| Tối ưu | (O(N^3+NM+N^2\log N)) | (O(N^2+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lưu trữ biểu đồ dưới dạng danh sách kề và dưới dạng ma trận khoảng cách. Ma trận ban đầu chứa các trọng số cạnh cho trước và số 0 trên đường chéo. Vì tất cả các trọng số đều dương nên không có biến chứng của chu kỳ âm. 
2. Chạy Floyd-Warshall để tính (d(u,v)), khoảng cách ngắn nhất giữa mỗi cặp đỉnh ban đầu. Việc tính toán tâm sau này chỉ phụ thuộc vào khoảng cách đường đi ngắn nhất này chứ không phụ thuộc vào cấu trúc cạnh ban đầu. 
3. Với mỗi đỉnh (c), hãy tính độ lệch tâm của nó là giá trị lớn nhất ở hàng (c). Tâm nằm chính xác tại (c) cho ra đường kính ứng cử viên (2\operatorname{ecc}(c)). Lưu trữ ứng cử viên đỉnh tốt nhất. 
4. Với mỗi cạnh đồ thị (e=(u,v)) có trọng số (w), hãy xét một tâm khả dĩ ở đâu đó bên trong cạnh đó. Đối với cạnh này, sắp xếp các đỉnh theo (d(u,z)) theo thứ tự giảm dần. Thứ tự sắp xếp được sử dụng lại cho mọi cạnh tới cùng một (u), do đó nó chỉ được tính một lần cho mỗi đỉnh. 
5. Quét thứ tự đó từ xa nhất đến gần nhất. Giữ đỉnh cuối cùng có khoảng cách từ (v) lớn hơn giá trị được giữ trước đó. Nếu đỉnh hiện tại là (a) và đỉnh được giữ lại trước đó là (b), thì hai đoạn đường bao tương ứng gặp nhau ở mức tối thiểu cục bộ có thể. Đường kính ứng cử viên của nó là 

[ 
d(u,a)+w+d(v,b). 
] 

Nếu giá trị này cải thiện câu trả lời, hãy lưu trữ (e), (a) và (b). Vị trí trung tâm tương ứng, được đo hai lần để tránh phân số, là 

[ 
2x=w+d(v,b)-d(u,a). 
] 

1. Sau khi kiểm tra tất cả các đỉnh và cạnh, ứng cử viên được lưu trữ là tâm tuyệt đối của đồ thị. Cây bao trùm có đường kính tối thiểu có thể được lấy từ cây có đường đi ngắn nhất có gốc tại tâm này. Đây là thuộc tính cấu trúc trung tâm của vấn đề. 
2. Nếu tâm là đỉnh ban đầu, hãy chạy Dijkstra từ đỉnh đó và giữ cạnh trước của mọi đỉnh ngoại trừ gốc. Vì mỗi phần trước được chọn dọc theo đường đi ngắn nhất từ ​​tâm nên các cạnh (N-1) này tạo thành cây đường đi ngắn nhất cần thiết. 
3. Nếu tâm nằm trên một cạnh ((u,v)), hãy tạm thời loại bỏ cạnh đó và thay thế nó bằng một đỉnh giả (c). Hai cạnh giả có độ dài (x) và (w-x). Chúng tôi chạy Dijkstra đa nguồn bằng cách khởi tạo (u) với khoảng cách (x) và (v) với khoảng cách (w-x). Chúng ta không cần số học dấu phẩy động vì mọi khoảng cách đều có thể nhân đôi. Do đó khoảng cách ban đầu là (2x) và (2(w-x)), cả hai đều là số nguyên. 
4. Cấu trúc tiền thân thu được là cây đường đi ngắn nhất trong biểu đồ có tâm giả. Nếu chỉ một trong số (u) và (v) vẫn được gắn trực tiếp vào hình nộm thì việc loại bỏ phần đính kèm giả đó đã để lại một cây bao trùm trên các đỉnh ban đầu. Nếu cả hai vẫn được gắn, việc loại bỏ cả hai cạnh giả sẽ để lại hai thành phần, vì vậy chúng ta thêm cạnh ban đầu ((u,v)) để kết nối lại chúng. 
5. In đường kính tối thiểu được lưu trữ và các cạnh biểu đồ gốc (N-1) thu được từ cây trước đó. 

Bất biến đằng sau toàn bộ thuật toán là tâm được chọn có khoảng cách đường đi ngắn nhất tối thiểu có thể đến các đỉnh của đồ thị. Bất kỳ cây bao trùm nào có đường kính (D) đều có điểm giữa có độ lệch tâm tối đa là (D/2), do đó độ lệch tâm gấp đôi của tâm tuyệt đối là giới hạn dưới trên mọi đường kính có thể có của cây. Ngược lại, một cây đường đi ngắn nhất có gốc tại tâm đó có mọi đỉnh nằm trong độ lệch tâm của tâm, vì vậy mỗi đường đi của cây có nhiều nhất là gấp đôi giá trị đó. Giới hạn dưới và giới hạn trên trùng nhau, chứng tỏ sự tối ưu. Quét cạnh tìm ra độ lệch tâm tối thiểu chính xác trên mọi cạnh có thể, trong khi quét đỉnh xử lý các tâm ở điểm cuối. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def dijkstra_vertex(n, adj, root):
    dist = [INF] * n
    parent = [-1] * n
    dist[root] = 0
    pq = [(0, root)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        for v, w, eid in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                parent[v] = u
                heapq.heappush(pq, (nd, v))

    return parent

def dijkstra_edge_center(n, adj, u, v, banned_eid, x2, w):
    # All distances are doubled.
    # The dummy center has doubled distances x2 and 2*w-x2
    # to u and v respectively.
    dist = [INF] * n
    parent = [-2] * n  # -2 means directly attached to dummy
    pq = []

    dv = 2 * w - x2

    dist[u] = x2
    dist[v] = dv
    heapq.heappush(pq, (x2, u))
    if v != u:
        heapq.heappush(pq, (dv, v))

    while pq:
        d, cur = heapq.heappop(pq)
        if d != dist[cur]:
            continue

        for to, ew, eid in adj[cur]:
            if eid == banned_eid:
                continue

            nd = d + 2 * ew
            if nd < dist[to]:
                dist[to] = nd
                parent[to] = cur
                heapq.heappush(pq, (nd, to))

    result = []

    for node in range(n):
        if parent[node] >= 0:
            result.append((parent[node], node))

    # If both u and v are still attached to the dummy, the two
    # components must be joined by the original center edge.
    if parent[u] == -2 and parent[v] == -2:
        result.append((u, v))

    return result

def solve():
    n, m = map(int, input().split())

    adj = [[] for _ in range(n)]
    edges = []

    dist = [[INF] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0

    for eid in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v, w))
        adj[u].append((v, w, eid))
        adj[v].append((u, w, eid))

        if w < dist[u][v]:
            dist[u][v] = w
            dist[v][u] = w

    # Floyd-Warshall.
    for k in range(n):
        dk = dist[k]
        for i in range(n):
            di = dist[i]
            dik = di[k]
            if dik >= INF:
                continue

            # The explicit loop avoids creating a large temporary
            # matrix row and works well for n <= 500.
            for j in range(n):
                nd = dik + dk[j]
                if nd < di[j]:
                    di[j] = nd

    # Distance orders, reused for every edge.
    orders = []
    for u in range(n):
        row = dist[u]
        order = list(range(n))
        order.sort(key=row.__getitem__, reverse=True)
        orders.append(order)

    best_diameter = INF
    best_type = 0
    best_root = -1
    best_edge = -1
    best_a = -1
    best_b = -1

    # Centers at original vertices.
    for u in range(n):
        ecc = max(dist[u])
        candidate = 2 * ecc
        if candidate < best_diameter:
            best_diameter = candidate
            best_type = 0
            best_root = u

    # Centers inside graph edges.
    for eid, (u, v, w) in enumerate(edges):
        order = orders[u]

        last = order[0]
        b_last = dist[v][last]

        for now in order[1:]:
            b_now = dist[v][now]

            if b_now > b_last:
                candidate = dist[u][now] + w + b_last

                if candidate < best_diameter:
                    best_diameter = candidate
                    best_type = 1
                    best_edge = eid
                    best_a = now
                    best_b = last

                last = now
                b_last = b_now

    # Construct the shortest-path tree from the selected center.
    if best_type == 0:
        parent = dijkstra_vertex(n, adj, best_root)
        answer_edges = []

        for v in range(n):
            if v != best_root:
                answer_edges.append((parent[v], v))
    else:
        u, v, w = edges[best_edge]

        # If a is the vertex on the u-side and b is the vertex on
        # the v-side, the center position satisfies
        #
        # x = (w + d(v,b) - d(u,a)) / 2.
        #
        # We store 2*x.
        x2 = w + dist[v][best_b] - dist[u][best_a]

        answer_edges = dijkstra_edge_center(
            n, adj, u, v, best_edge, x2, w
        )

    out = [str(best_diameter)]
    out.extend(f"{u + 1} {v + 1}" for u, v in answer_edges)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Ma trận khoảng cách được khởi tạo với độ dài cạnh ban đầu và sau đó được đóng lại theo các đường đi ngắn nhất bởi Floyd-Warshall. Biểu đồ rất đơn giản, do đó mỗi cặp có nhiều nhất một cạnh đầu vào, nhưng việc lấy mức tối thiểu trong quá trình khởi tạo cũng làm cho việc xây dựng ma trận trở nên chắc chắn. 

các`orders`mảng là một chi tiết triển khai quan trọng. Đối với mỗi điểm cuối (u), nó lưu trữ tất cả các đỉnh theo thứ tự giảm dần (d(u,\cdot)). Một cạnh ((u,v)) sau đó có thể tái sử dụng`orders[u]`, tránh việc sắp xếp bên trong vòng lặp cạnh (M). 

Giai đoạn đỉnh-tâm được cố tình đơn giản. Đối với mỗi đỉnh, độ lệch tâm của nó chỉ là giá trị lớn nhất trong hàng khoảng cách của nó, do đó đường kính ứng cử viên sẽ gấp đôi giá trị đó. 

Pha trung tâm cạnh theo sau quá trình quét đường bao trên.`last`là đỉnh được giữ lại trước đó, trong khi`now`là đỉnh hiện tại theo thứ tự khoảng cách giảm dần từ (u). điều kiện`b_now > b_last`có nghĩa là tọa độ thứ hai đã di chuyển theo hướng cần thiết cho đoạn đường bao trên mới. Ứng viên`dist[u][now] + w + b_last`chính xác gấp đôi giá trị cực tiểu cục bộ của hai hàm hình chữ V tương ứng. 

Bản thân tâm có thể nằm ở vị trí nửa nguyên dọc theo cạnh. Số học dấu phẩy động sẽ không cần thiết và có khả năng gây nguy hiểm, vì vậy việc xây dựng sử dụng khoảng cách gấp đôi. Nếu đường kính là (D), khoảng cách gấp đôi từ tâm giả đến điểm cuối phía (u) là 

[ 
2x=D-2d(u,a), 
] 

đó là giá trị tương tự được tính toán như`w + dist[v][best_b] - dist[u][best_a]`. 

Đối với một ứng cử viên lấy cạnh làm trung tâm, cạnh trung tâm ban đầu tạm thời bị loại khỏi Dijkstra. Điều này là cần thiết vì tâm giả đại diện cho một điểm bên trong cạnh đó chứ không phải một tuyến đường riêng biệt được phép đi qua tâm. Hai điểm cuối được khởi tạo dưới dạng nguồn ở khoảng cách từ trung tâm giả. 

giá trị`parent[node] == -2`có nghĩa là đỉnh được kết nối trực tiếp với tâm giả trong cây đường đi ngắn nhất theo khái niệm. Nếu cả hai điểm cuối vẫn giữ trạng thái đó thì cạnh ban đầu phải được khôi phục sau khi loại bỏ hình nộm. Nếu chỉ còn một cạnh được gắn vào thì các cạnh trước đó đã tạo thành cây bao trùm trên biểu đồ ban đầu. 

Số nguyên Python có độ chính xác tùy ý, do đó trọng số cạnh lên tới (10^9), đường dẫn chứa tối đa (499) cạnh và khoảng cách nhân đôi đều phù hợp mà không cần xử lý tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một tam giác có độ dài các cạnh bằng (1). 

| Sân khấu | Trạng thái chính | Giá trị | 
| --- | --- | --- | 
| APSP | (d(1,2),d(1,3),d(2,3)) | (1,1,1) | 
| Đỉnh 1 | lệch tâm | (1) | 
| Đỉnh 2 | lệch tâm | (1) | 
| Đỉnh 3 | lệch tâm | (1) | 
| Ứng cử viên đỉnh tốt nhất | (2\cdot 1) | (2) | 
| Ứng viên cạnh | tối thiểu | (2) | 
| Trung tâm được chọn | một đỉnh có thể | (1) | 
| Cây xây dựng | hai cạnh | (1-2,\ 1-3) | 
| Đường kính | (1+1) | (2) | 

Mọi đỉnh đều có độ lệch tâm (1), nên mọi đỉnh đều là tâm tối ưu. Dijkstra từ bất kỳ ngôi sao nào trong số chúng tạo ra một ngôi sao hai cạnh, có đường kính là (2). Điều này chứng tỏ rằng những ràng buộc về con đường ngắn nhất và những ràng buộc giữa các trung tâm là vô hại. 

### Mẫu 2 

Biểu đồ có kết nối giống như cây cầu nặng nề giữa cụm bên trái và cụm bên phải. 

| Sân khấu | Trạng thái chính | Giá trị | 
| --- | --- | --- | 
| Cụm trái | khoảng cách ngắn | (10,20,30) thang đo | 
| Cụm bên phải | khoảng cách ngắn | (10,20,30) thang đo | 
| Cạnh kết nối | (3-4) | (1000) | 
| Trung tâm đỉnh tốt nhất | ứng cử viên đường kính | lớn hơn (1060) | 
| Trung tâm cạnh | cạnh | (3-4) | 
| Ứng viên trung tâm biên | (d(3,a)+1000+d(4,b)) | (1060) | 
| Đường kính đã chọn | tối thiểu | (1060) | 
| Cây xây dựng | cạnh cụm chéo | (3-4) | 
| Cây cuối cùng | năm cạnh | (3-4,4-6,6-5,3-2,2-1) | 
| Đường kính | con đường cây dài nhất | (1060) | 

Đặc điểm quan trọng là tâm tối ưu nằm ở rìa trọng lượng (1000). Hai bên có cấu trúc ngắn riêng nên việc đặt tâm bên trong cạnh nặng đó sẽ cân bằng hai bên tốt hơn so với việc chọn một trong hai điểm cuối làm tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3+NM+N^2\log N)) | Chi phí Floyd-Warshall (O(N^3)), sắp xếp tất cả chi phí đặt hàng khoảng cách (O(N^2\log N)) và quét chi phí đặt hàng của mọi cạnh (O(NM)) | 
| Không gian | (O(N^2+M)) | Ma trận khoảng cách sử dụng (O(N^2)), trong khi danh sách biểu đồ và cạnh sử dụng (O(M)) | 

Với (N\le500), cả (N^3) và (NM) đều có nhiều nhất là theo thứ tự các phép toán (10^8). Yêu cầu bộ nhớ rất khiêm tốn vì ma trận khoảng cách (500\times500) chỉ có (250000) mục nhập. Thuật toán là cách tiếp cận đa thức dự định cho các giới hạn này, mặc dù phần Floyd-Warshall là phần quan trọng về hiệu năng khi triển khai Python. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề chấp nhận bất kỳ cây bao trùm tối ưu nào, nên các thử nghiệm sẽ xác nhận đường kính được báo cáo và bản thân cây thay vì so sánh từng ký tự chuỗi đầu ra hoàn chỉnh.```python
# Paste the solve() implementation from the solution above before this test code.

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
    it = iter(data)

    n = next(it)
    m = next(it)

    weights = {}
    for _ in range(m):
        u = next(it)
        v = next(it)
        w = next(it)
        if u > v:
            u, v = v, u
        weights[(u, v)] = w

    lines = out.strip().splitlines()
    assert len(lines) == n, "wrong number of output lines"

    diameter = int(lines[0])
    assert diameter == expected_diameter, (
        f"wrong diameter: got {diameter}, expected {expected_diameter}"
    )

    tree = [[] for _ in range(n)]
    used = set()

    for line in lines[1:]:
        u, v = map(int, line.split())
        assert 1 <= u <= n and 1 <= v <= n and u != v

        key = (min(u, v), max(u, v))
        assert key in weights, "output contains an edge not in the graph"
        assert key not in used, "duplicate tree edge"

        used.add(key)
        w = weights[key]
        tree[u - 1].append((v - 1, w))
        tree[v - 1].append((u - 1, w))

    assert len(used) == n - 1

    parent = [-1] * n
    parent[0] = 0
    stack = [0]
    order = []

    while stack:
        u = stack.pop()
        order.append(u)
        for v, _ in tree[u]:
            if v == parent[u]:
                continue
            assert parent[v] == -1, "cycle in output"
            parent[v] = u
            stack.append(v)

    assert len(order) == n, "output edges do not connect all vertices"

    def farthest(start):
        dist = [-1] * n
        dist[start] = 0
        stack = [start]
        best = start

        while stack:
            u = stack.pop()
            if dist[u] > dist[best]:
                best = u

            for v, w in tree[u]:
                if dist[v] != -1:
                    continue
                dist[v] = dist[u] + w
                stack.append(v)

        return best, dist[best]

    a, _ = farthest(0)
    _, tree_diameter = farthest(a)

    assert tree_diameter == expected_diameter, (
        f"constructed tree has diameter {tree_diameter}"
    )

# Sample 1.
sample1 = """\
3 3
1 2 1
2 3 1
3 1 1
"""
validate(sample1, run(sample1), 2)

# Sample 2.
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

# Center lies inside an edge.
case_edge_center = """\
3 2
1 2 2
2 3 4
"""
validate(case_edge_center, run(case_edge_center), 6)

# All edge weights equal.
case_equal = """\
4 6
1 2 5
1 3 5
1 4 5
2 3 5
2 4 5
3 4 5
"""
validate(case_equal, run(case_equal), 10)

# A very long direct edge should not be forced into the tree.
case_long = """\
4 6
1 2 1
2 3 1
3 4 1
1 4 100
1 3 10
2 4 10
"""
validate(case_long, run(case_long), 3)

# Maximum-size style test, 500 vertices.
# A unit-weight star is already optimal and has diameter 2.
n = 500
parts = [f"{n} {n - 1}"]
for v in range(2, n + 1):
    parts.append(f"1 {v} 1")
case_max = "\n".join(parts) + "\n"

validate(case_max, run(case_max), 2)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2 7`| Đường kính (7) | Tối thiểu (N), cây một cạnh, xử lý ranh giới | 
|`3 2 / 1 2 2 / 2 3 4`| Đường kính (6) | Căn giữa hoàn toàn bên trong một cấu trúc an toàn cạnh và nửa số nguyên | 
| Đồ thị hoàn chỉnh trên 4 đỉnh, mọi trọng số (5) | Đường kính (10) | Khoảng cách bằng nhau, nhiều cây tối ưu, xử lý ràng buộc | 
| Đồ thị bốn đỉnh có cạnh trọng số (100) | Đường kính (3) | Tránh một cạnh dài hấp dẫn nhưng có hại | 
| Ngôi sao đơn vị có (500) đỉnh | Đường kính (2) | Tối đa (N), đồ thị thưa thớt, kích thước đầu vào lớn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là biểu đồ có kích thước tối thiểu:```
2 1
1 2 7
```Chỉ có một cây bao trùm, đó là cạnh đơn (1-2), nên đường kính của nó là (7). Quét tâm đỉnh cho độ lệch tâm (7) đối với đường kính đỉnh và ứng cử viên (14), trông quá lớn nếu được hiểu là đường kính cây. Đây là lý do tại sao luận cứ chung về tâm phải được hiểu cẩn thận: trung điểm của một cạnh là tâm giả. Quá trình quét cạnh xem xét cạnh (1-2), tìm tâm ở khoảng cách (3,5) từ một trong hai điểm cuối và tạo ra đường kính dự kiến ​​(7). Việc xây dựng sử dụng khoảng cách giả gấp đôi, do đó không có giá trị dấu phẩy động (3,5) nào được lưu trữ. 

Trường hợp cạnh thứ hai là đường dẫn có trọng số```
3 2
1 2 2
2 3 4
```Cây duy nhất có thể có đường kính (6). Đỉnh (2) có độ lệch tâm (4), do đó, thuật toán chỉ có đỉnh sẽ báo cáo (8). Đối với cạnh (2-3), các đỉnh liên quan là (1) và (3). Cặp khoảng cách của chúng với các điểm cuối là ((2,6)) và ((4,0)). Giao điểm đường bao của họ cho 

[ 
D=2+4+0=6, 
] 

và tọa độ tâm nhân đôi là (4+0-2=2), nghĩa là tâm cách đỉnh (2 một đơn vị). Cấu trúc Dijkstra lấy cạnh làm trung tâm phục hồi hai cạnh ban đầu. 

Trường hợp cạnh thứ ba là biểu đồ có trọng số bằng nhau hoàn chỉnh:```
4 6
1 2 5
1 3 5
1 4 5
2 3 5
2 4 5
3 4 5
```Một ngôi sao có tâm ở bất kỳ đỉnh nào có đường kính (10), là mức tối ưu vì mỗi cặp đỉnh riêng biệt cách nhau ít nhất một cạnh có trọng số (5) và một cây trên bốn đỉnh không thể có đường kính nhỏ hơn (10) trong biểu đồ này. Việc triển khai có thể chọn một ngôi sao khác với ngôi sao mà con người mong đợi vì tất cả các so sánh khoảng cách đều có thể ràng buộc. Trình xác thực sẽ kiểm tra thuộc tính và đường kính của cây thay vì yêu cầu thứ tự cạnh cụ thể. 

Trường hợp cạnh thứ tư có cạnh rất dài:```
4 6
1 2 1
2 3 1
3 4 1
1 4 100
1 3 10
2 4 10
```Floyd-Warshall lần đầu tiên phát hiện ra rằng khoảng cách ngắn nhất giữa các đỉnh bị chi phối bởi ba cạnh đơn vị. Do đó, tính toán trung tâm thiên về vùng trung tâm ngắn của biểu đồ. Cây cuối cùng có thể sử dụng (1-2), (2-3) và (3-4), cho đường kính (3). Cạnh trọng số (100) không bao giờ được yêu cầu chỉ vì nó tồn tại. 

Trường hợp cạnh thứ năm là một biểu đồ mà việc loại bỏ cạnh giữa đã chọn sẽ ngắt kết nối biểu đồ:```
3 2
1 2 2
2 3 4
```Khi cạnh (2-3) được coi là cạnh giữa, đồ thị tạm thời có hai thành phần, một thành phần chứa đỉnh (1,2) và thành phần kia chứa đỉnh (3). Dijkstra đa nguồn bắt đầu từ cả hai phía của trung tâm giả. Cả hai điểm cuối vẫn được gắn trực tiếp vào hình nộm khái niệm, do đó việc xây dựng sẽ loại bỏ hai kết nối giả đó và khôi phục cạnh ban đầu (2-3). Chính xác còn lại hai cạnh ban đầu, tạo thành một cây bao trùm. 

Trường hợp tinh tế cuối cùng là cạnh trung tâm không có cầu nối. Nếu một đường dẫn thay thế kết nối các điểm cuối của nó, Dijkstra đa nguồn có thể khám phá một điểm cuối rẻ hơn từ phía bên kia. Khi đó chỉ còn một điểm cuối được gắn trực tiếp vào trung tâm giả. Các cạnh trước đó đã kết nối tất cả các đỉnh ban đầu thành một cây, do đó cạnh giữa ban đầu không được thêm vào lần thứ hai. Đây là lý do tại sao việc xây dựng kiểm tra xem cả hai điểm cuối có còn là gốc giả hay không thay vì thêm cạnh trung tâm một cách mù quáng.
