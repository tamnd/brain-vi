---
title: "CF 1766F - MCF"
description: "Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi cạnh có công suất và chi phí. Chúng ta muốn đẩy luồng từ đỉnh 1 đến đỉnh n, thỏa mãn sự bảo toàn luồng thông thường ở tất cả các đỉnh trung gian: tổng luồng vào một đỉnh bằng tổng luồng đi ra."
date: "2026-06-09T13:01:21+07:00"
tags: ["codeforces", "competitive-programming", "flows"]
categories: ["algorithms"]
codeforces_contest: 1766
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 139 (Rated for Div. 2)"
rating: 2800
weight: 1766
solve_time_s: 138
verified: false
draft: false
---

[CF 1766F - MCF](https://codeforces.com/problemset/problem/1766/F) 

**Đánh giá:** 2800 
**Thẻ:** dòng chảy 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi cạnh có công suất và chi phí. Chúng ta muốn đẩy luồng từ đỉnh 1 đến đỉnh n, thỏa mãn sự bảo toàn luồng thông thường ở tất cả các đỉnh trung gian: tổng luồng vào một đỉnh bằng tổng luồng đi ra. Mỗi cạnh có một ràng buộc chẵn lẻ bổ sung: nếu dung lượng của nó là chẵn thì luồng phải chẵn; nếu lẻ thì luồng phải lẻ. Mục tiêu là giảm thiểu tổng chi phí của luồng, được định nghĩa là tổng trên tất cả các cạnh của trọng số thời gian của luồng. 

Những hạn chế về`n`(lên tới 100) và`m`(lên tới 200) gợi ý rằng một thuật toán có độ phức tạp bậc ba hoặc bậc bốn về số đỉnh là có thể chấp nhận được. Dung lượng là các số nguyên nhỏ (lên tới 100), giúp mô hình hóa các luồng có ràng buộc số nguyên trở nên khả thi. Các trọng số âm ngụ ý rằng chúng ta phải xử lý cẩn thận các đường đi ngắn nhất trong biểu đồ dư; tuy nhiên, việc không có chu kỳ âm đảm bảo rằng tồn tại dòng chi phí tối thiểu cho các nhu cầu khả thi. 

Các trường hợp biên rất khó phát hiện do các ràng buộc về tính chẵn lẻ. Ví dụ, nếu một đỉnh có các cạnh vào có dung lượng chẵn và các cạnh ra có dung lượng lẻ thì có thể không thể đáp ứng được việc bảo toàn luồng trong khi vẫn đảm bảo tính chẵn lẻ. Một thuật toán luồng chi phí tối thiểu ngây thơ bỏ qua tính chẵn lẻ sẽ tạo ra một luồng "khả thi" không chính xác và thực sự vi phạm điều kiện chẵn lẻ. 

Hãy xem xét ví dụ nhỏ này:```
2 1
1 2 2 3
```Ở đây, cạnh đơn có công suất chẵn nên dòng chảy phải đều. Luồng tối thiểu để thỏa mãn sự bảo toàn là 2. Bất kỳ nỗ lực nào nhằm đẩy 1 sẽ vi phạm tính chẵn lẻ và thuật toán bỏ qua tính chẵn lẻ sẽ chấp nhận 1 một cách sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các luồng có thể có cho mỗi cạnh, bị hạn chế bởi năng lực và tính chẵn lẻ. Với tối đa 200 cạnh và dung lượng lên tới 100, điều này dẫn đến trung bình 50 lựa chọn trên mỗi cạnh, mang lại`50^200`khả năng, rõ ràng là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát thấy rằng các ràng buộc chẵn lẻ có thể được chuyển thành vấn đề về luồng chi phí tối thiểu tiêu chuẩn. Nếu chúng ta giảm mọi công suất theo yêu cầu chẵn lẻ của nó (1 cho số lẻ, 0 cho số chẵn), sau đó điều chỉnh cung/cầu ở mỗi đỉnh tương ứng, chúng ta có thể giải quyết vấn đề luồng chi phí tối thiểu theo số nguyên tiêu chuẩn mà không vi phạm tính chẵn lẻ. Về cơ bản, chúng tôi phân bổ trước tính chẵn lẻ được yêu cầu tối thiểu trên mỗi cạnh và sau đó tính toán luồng còn lại như một bài toán MCF tiêu chuẩn. Nếu một luồng khả thi tồn tại trong mạng được điều chỉnh này, chúng ta có thể kết hợp nó với tính chẵn lẻ được phân bổ trước để có được một luồng thỏa mãn tất cả các ràng buộc. 

Điều này hiệu quả vì luồng của mọi cạnh có thể được phân tách thành phần chẵn lẻ cố định và phần còn lại có thể thay đổi. Biểu đồ dư vẫn không có chu kỳ âm, vì vậy chúng ta có thể sử dụng các thuật toán tiêu chuẩn như đường đi ngắn nhất liên tiếp hoặc luồng tối đa chi phí tối thiểu với Bellman-Ford/Dijkstra cho các cạnh âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((C/2)^m) | O(m) | Quá chậm | 
| Luồng chi phí tối thiểu với việc giảm tính chẵn lẻ | O(n m^2 C) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách`f`của dòng chảy, tất cả đều bằng không. Đối với mỗi cạnh, hãy kiểm tra dung lượng của nó`c`. Nếu như`c`là số lẻ, cấp trước 1 đơn vị lưu lượng; nếu chẵn, phân bổ trước 0. Trừ số phân bổ trước này khỏi dung lượng. Điều này đảm bảo rằng các ràng buộc chẵn lẻ được tự động thỏa mãn cho luồng còn lại. 
2. Xây dựng đồ thị phần dư sử dụng các khả năng điều chỉnh này. Cung/cầu ròng của mỗi đỉnh ban đầu bằng 0. Đỉnh 1 có nguồn cung bằng tổng các luồng được phân bổ trước rời khỏi nó và đỉnh n có nhu cầu bằng tổng các luồng được phân bổ trước đi vào nó. Đối với tất cả các đỉnh khác, hãy điều chỉnh nhu cầu để tính đến các luồng được phân bổ trước để duy trì việc bảo toàn luồng. 
3. Chạy thuật toán luồng chi phí tối thiểu số nguyên tiêu chuẩn trên mạng dư này. Đường đi ngắn nhất liên tiếp với Bellman-Ford hoạt động vì dung lượng và số cạnh nhỏ và tồn tại trọng số âm nhưng không có chu kỳ âm. 
4. Nếu thuật toán tìm thấy một luồng khả thi thỏa mãn tất cả các nhu cầu còn lại, hãy thêm luồng chẵn lẻ được phân bổ trước trở lại mỗi cạnh để có được các luồng cuối cùng. Ngược lại thì báo cáo`Impossible`. 
5. Đầu ra`Possible`theo sau là danh sách các luồng. Các luồng cuối cùng tự động tôn trọng tính chẵn lẻ và bảo toàn luồng. 

Tại sao nó hoạt động: điều bất biến là mỗi cạnh đã có luồng tối thiểu cần thiết để đáp ứng tính chẵn lẻ. Bất kỳ luồng bổ sung nào trong mạng dư đều tôn trọng cả khả năng bảo toàn luồng và dung lượng. Do đó, nếu luồng dư là khả thi thì việc thêm nó vào luồng chẵn lẻ sẽ tạo ra một giải pháp hợp lệ. Nếu mạng dư không khả thi thì không có phép gán nào có thể đáp ứng đồng thời cả việc bảo toàn luồng và tính chẵn lẻ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

class Edge:
    def __init__(self, u, v, cap, cost, idx):
        self.u = u
        self.v = v
        self.cap = cap
        self.cost = cost
        self.idx = idx
        self.flow = 0

def min_cost_flow(n, edges, source, sink):
    INF = 10**18
    m = len(edges)
    adj = [[] for _ in range(n)]
    for i, e in enumerate(edges):
        adj[e.u].append((e.v, i))
        adj[e.v].append((e.u, i))

    potential = [0]*n
    def shortest_path():
        dist = [INF]*n
        prev = [None]*n
        inqueue = [False]*n
        dist[source] = 0
        q = deque([source])
        while q:
            u = q.popleft()
            inqueue[u] = False
            for v, i in adj[u]:
                e = edges[i]
                residual = e.cap - e.flow if e.u == u else e.flow
                if residual <= 0:
                    continue
                cost = e.cost if e.u == u else -e.cost
                if dist[v] > dist[u] + cost + potential[u] - potential[v]:
                    dist[v] = dist[u] + cost + potential[u] - potential[v]
                    prev[v] = (u, i)
                    if not inqueue[v]:
                        inqueue[v] = True
                        q.append(v)
        for i in range(n):
            if dist[i] < INF:
                potential[i] += dist[i]
        return prev, dist[sink] < INF

    total_flow = 0
    total_cost = 0
    while True:
        prev, found = shortest_path()
        if not found:
            break
        # find bottleneck
        v = sink
        delta = INF
        while v != source:
            u, i = prev[v]
            e = edges[i]
            if e.u == u:
                delta = min(delta, e.cap - e.flow)
            else:
                delta = min(delta, e.flow)
            v = u
        # push flow
        v = sink
        while v != source:
            u, i = prev[v]
            e = edges[i]
            if e.u == u:
                e.flow += delta
                total_cost += delta * e.cost
            else:
                e.flow -= delta
                total_cost -= delta * e.cost
            v = u
        total_flow += delta
    return total_flow, total_cost

def solve():
    n, m = map(int, input().split())
    edges = []
    pre_flow = []
    for idx in range(m):
        x, y, c, w = map(int, input().split())
        x -= 1
        y -= 1
        parity = c % 2
        pre_flow.append(parity)
        edges.append(Edge(x, y, c - parity, w, idx))
    # adjust source and sink demands
    demands = [0]*n
    for e, pf in zip(edges, pre_flow):
        demands[e.u] -= pf
        demands[e.v] += pf
    # add super-source and super-sink
    super_source = n
    super_sink = n+1
    adj_edges = edges[:]
    for i, d in enumerate(demands):
        if d > 0:
            adj_edges.append(Edge(super_source, i, d, 0, m + len(adj_edges)))
        elif d < 0:
            adj_edges.append(Edge(i, super_sink, -d, 0, m + len(adj_edges)))
    # run min-cost flow from super_source to super_sink
    total_flow, _ = min_cost_flow(n+2, adj_edges, super_source, super_sink)
    if total_flow != sum(d for d in demands if d > 0):
        print("Impossible")
        return
    final_flow = [pf for pf in pre_flow]
    for e in edges:
        final_flow[e.idx] += e.flow
    print("Possible")
    print(" ".join(map(str, final_flow)))

solve()
```Giải pháp đầu tiên là phân bổ trước tính chẵn lẻ trên mỗi cạnh. Sau đó, nó tính toán luồng khả thi trong mạng dư với công suất và nhu cầu được điều chỉnh. Chúng tôi sử dụng luồng chi phí tối thiểu dựa trên đường đi ngắn nhất để xử lý các trọng số âm. Cuối cùng, chúng ta kết hợp tính chẵn lẻ được cấp phát trước với luồng dư để tạo ra câu trả lời. Phần khó khăn là quản lý chính xác nhu cầu cho từng đỉnh dựa trên luồng trước. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 3
1 2 3 -10
1 2 3 -15
2 3 2 0
```Phân bổ trước luồng: các cạnh`[1,1,0]`bởi vì năng lực`[3,3,2]`→ lẻ, lẻ, chẵn. Công suất dư`[2,2,2]`. Giải quyết chi phí tối thiểu
