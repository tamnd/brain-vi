---
title: "CF 102394H - Xe buýt đường cao tốc"
description: "Có (n) trạm xe buýt được nối với nhau bằng đồ thị liên thông vô hướng. Mỗi đường cao tốc đều có đơn vị chiều dài, do đó khoảng cách giữa hai trạm là chiều dài đường đi ngắn nhất thông thường của chúng trong biểu đồ."
date: "2026-08-10T19:23:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "H"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 457
verified: true
draft: false
---

[CF 102394H - Xe buýt đường cao tốc](https://codeforces.com/problemset/problem/102394/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) trạm xe buýt được nối với nhau bằng đồ thị liên thông vô hướng. Mỗi đường cao tốc đều có đơn vị chiều dài, do đó khoảng cách giữa hai trạm là chiều dài đường đi ngắn nhất thông thường của chúng trong biểu đồ. 

Trạm (i) có thể bán vé xe buýt cho bất kỳ trạm nào có đồ thị khoảng cách từ (i) tối đa là (f_i). Đi xe buýt như vậy tốn kém 

[ 
c_i+(T-1)w_i 
] 

khi tất cả các xe buýt đều được đưa vào ngày (T). Giá chỉ phụ thuộc vào ga mua vé chứ không phụ thuộc vào điểm đến. 

Alice bắt đầu ở trạm (1). Đối với mọi điểm đến có thể (k), chúng tôi cần tổng chi phí vé tối thiểu cho tất cả các chuỗi chuyến đi xe buýt hợp lệ và tất cả các ngày (1\le T\le T_{\max}). 

Đồ thị có tới (200000) đỉnh nhưng chỉ có (n+50) cạnh. Giới hạn thứ hai đó là khóa cấu trúc. Một biểu đồ thưa thớt tổng quát vẫn có thể có các đường đi ngắn nhất phức tạp, nhưng biểu đồ này khác với biểu đồ cây nhiều nhất là (51) cạnh. 

Tham số thời gian lớn bằng (10^6) nên việc cố gắng hàng ngày là không thể. Số lượng đỉnh cũng đủ lớn để thậm chí (O(n^2)) hoạt động vượt xa phạm vi dự định. Giải pháp phải khai thác cả số lượng nhỏ các cạnh bổ sung và thực tế là mọi chuyển đổi xe buýt đi từ một trạm đều có cùng chi phí. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ quá trình triển khai. 

Đầu tiên là trạm khởi đầu. Alice không cần mua vé vào thăm ga (1) nên đáp án của cô luôn là 0. Ví dụ,```
1 0 3
1 10 -5
```có đầu ra```
0
```Việc triển khai đường đi ngắn nhất buộc phải đi ít nhất một chuyến xe buýt sẽ trả về giá trị dương không chính xác. 

Thứ hai là âm (w_i). Ngày sau có thể rẻ hơn ngày (1) và ngày tối ưu không thể đơn giản được coi là ngày đầu tiên. Ví dụ,```
2 1 3
1 10 -4
1 1 0
1 2
```có đầu ra```
0
2
```Giá vé của ga (1) là (10,6,2) trong ba ngày nên đến ga thứ hai với giá rẻ nhất vào ngày (3). 

Thứ ba là đường cao tốc không có cây xanh. Giả định```
3 3 1
1 3 0
1 3 0
1 3 0
1 2
2 3
1 3
```được đưa ra. Cây bao trùm có thể chứa (1-2) và (2-3), trong khi (1-3) trở thành cạnh phụ. Vì (f_1=1), trạm (3) có thể đến được từ trạm (1) trên một xe buýt do có cạnh phụ. Đầu ra đúng là```
0
3
3
```Một phương pháp chỉ xem xét khoảng cách trong cây bao trùm sẽ nghĩ sai rằng trạm (3) cách đó hai đường cao tốc. 

Thứ tư là điều kiện bán kính bao gồm. Nếu (f_i=2), đích đến ở khoảng cách chính xác (2) là hợp pháp. Ví dụ,```
3 2 1
2 7 0
1 100 0
1 100 0
1 2
2 3
```có đầu ra```
0
7
7
```Điểm đến ở khoảng cách chính xác (2) phải được bao gồm. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp cho một ngày cố định về mặt khái niệm là đơn giản. Cung cấp cho trạm (i) chi phí cạnh đi (c_i+(T-1)w_i) và kết nối nó với mọi trạm trong khoảng cách đồ thị (f_i). Sau đó chạy Dijkstra từ trạm (1). 

Vấn đề là đồ thị có hướng ẩn này có thể dày đặc. Một trạm có bán kính bao phủ toàn bộ biểu đồ có (n) chuyển tiếp xe buýt đi. Trong trường hợp xấu nhất có (\Theta(n^2)) sự chuyển đổi như vậy. Việc thử tất cả (T_{\max}) ngày sẽ dẫn đến khoảng 

[ 
O(T_{\max}n^2\log n) 
] 

làm việc. Với (n=200000) và (T_{\max}=10^6), chỉ riêng số lần thư giãn có thể đạt được là khoảng (4\cdot10^{16}). 

Quan sát chính đầu tiên loại bỏ hoàn toàn chiều ngày. Hãy xem xét một chuỗi cố định các chuyến đi xe buýt. Tổng giá của nó là 

[ 
\sum_i c_i+(T-1)\sum_i w_i, 
] 

đó là hàm tuyến tính của (T). Hàm tuyến tính trên khoảng nguyên ([1,T_{\max}]) đạt mức tối thiểu tại một trong hai điểm cuối. Do đó, mỗi tuyến đường cụ thể chỉ cần được xem xét vào ngày (1) hoặc ngày (T_{\max}). 

Lấy mức tối thiểu trên tất cả các tuyến đường sẽ bảo tồn thuộc tính này: 

\min\left( 
\min_{\text{route}}\text{cost(route},1), 
\min_{\text{route}}\text{cost(route},T_{\max}) 
\đúng). 
] 

Vì vậy chúng ta chỉ cần hai phép tính đường đi ngắn nhất. Việc giảm điểm cuối này cũng là điểm khởi đầu của cách tiếp cận giải pháp đã biết cho vấn đề. 

Bây giờ hãy sửa một trong hai ngày đó. hãy để 

[ 
a_i=c_i+(T-1)w_i. 
] 

Bất cứ khi nào Alice đến trạm (i), mỗi lần chuyển xe buýt hợp lệ từ (i) đều có chi phí chính xác là (a_i). Giả sử khoảng cách ngắn nhất hiện tại của nó là (d_i). Do đó, mọi trạm trong bán kính của nó có thể được gán giá trị ứng cử viên 

[ 
d_i+a_i. 
] 

Điều này mang lại một biến thể Dijkstra hữu ích. Thay vì lưu trữ rõ ràng mọi cạnh bus được định hướng, hãy đặt trạm (i) vào hàng đợi ưu tiên bằng khóa (d_i+a_i). Khi nó được xử lý, chúng ta cần tìm tất cả các đỉnh vẫn chưa được chạm tới trong khoảng cách (f_i). 

Nếu biểu đồ đường cao tốc chính xác là một cây thì đây sẽ trở thành truy vấn phân rã trọng tâm tiêu chuẩn. Đối với mỗi centroid (x), lưu trữ tất cả các đỉnh trong thành phần hiện tại của nó được sắp xếp theo khoảng cách cây của chúng với (x). Với nguồn (u), hãy duyệt qua trọng tâm tổ tiên của (u). Nếu (d(u,x)) đã biết và 

[ 
d(u,x)+d(x,v)\le f_u, 
] 

thì (v) là một đích đến hợp lệ. Chúng ta có thể sử dụng danh sách đã sắp xếp bằng một con trỏ. Khi Dijkstra đã tới một đỉnh, nó không bao giờ cần phải xem xét lại, vì vậy mọi con trỏ chỉ di chuyển về phía trước. 

Đồ thị thực tế có tối đa (51) cạnh nằm ngoài cây bao trùm. Hãy xem xét một cạnh không phải là cây như vậy và chọn một trong các điểm cuối của nó, ví dụ (x). Bất kỳ đường đi ngắn nhất nào sử dụng cạnh bổ sung này đều đi qua (x). Do đó, đích (v) có thể đến được từ (u) thông qua một số đường dẫn sử dụng cạnh này thỏa mãn 

[ 
\operatorname{dist}(u,x)+\operatorname{dist}(x,v)\le f_u. 
] 

Chúng ta có thể chạy một BFS thông thường từ mọi điểm cuối đã chọn (x), lưu trữ tất cả các đỉnh theo thứ tự không giảm dần về khoảng cách của chúng với (x) và sử dụng chính xác cùng một ý tưởng con trỏ. Có nhiều nhất (51) lần chạy BFS như vậy. Đây là phần mở rộng đồ thị thưa thớt của ý tưởng phân rã centroid được sử dụng trong giải pháp tham chiếu. 

Sự đơn giản hóa quan trọng là chúng ta không bao giờ xây dựng đồ thị bus dày đặc. Các cấu trúc trung tâm và một số cấu trúc BFS ngầm thể hiện tất cả các chuyển đổi hữu ích của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(T_{\max}n^2\log n)) | (O(n^2)) | Quá chậm | 
| Tối ưu | (O(n\log n+(m-n+1)n)) mỗi hai lần chạy điểm cuối | (O(n\log n+(m-n+1)n)) | Đã chấp nhận | 

Vì (m-n+1\le51), nên độ phức tạp tối ưu là hiệu quả (O(n\log n+51n)). 

## Hướng dẫn thuật toán

1. Xây dựng cây khung bất kỳ của đồ thị đường cao tốc. Mỗi cạnh không được chọn cho cây được gọi là cạnh phụ. Vì đồ thị được kết nối và (m\le n+50), nên có nhiều nhất (51) cạnh phụ. 
2. Đối với mỗi cạnh phụ ((u,v)), hãy chọn một điểm cuối, ví dụ (u). Có thể loại bỏ các điểm cuối được chọn trùng lặp vì một BFS từ cùng một điểm cuối đã xử lý mọi đường dẫn đi qua điểm cuối đó. 
3. Xây dựng phân rã trọng tâm của cây bao trùm. Tại mỗi centroid (x), thu thập tất cả các đỉnh trong thành phần hiện tại của nó và lưu trữ khoảng cách cây của chúng với (x) theo thứ tự không giảm. 
4. Trong khi xây dựng phân rã trọng tâm, lưu trữ cho mỗi đỉnh (u) trọng tâm của nó ở mọi mức phân rã và khoảng cách của nó tới trọng tâm đó. Điều này cho phép truy vấn Dijkstra thu được (\operatorname{dist__{tree}(u,x)) mà không cần tính toán LCA. 
5. Đối với mọi điểm cuối cạnh ngoài (x) đã chọn, hãy chạy BFS trong biểu đồ gốc. Lưu trữ thứ tự BFS và khoảng cách từ (x) đến mọi đỉnh. BFS tự nhiên tạo ra các đỉnh theo thứ tự khoảng cách không giảm. 
6. Cố định một ngày (T), ban đầu (T=1). Xác định giá vé ga (i) là 

[ 
a_i=c_i+(T-1)w_i. 
] 

Chạy Dijkstra ngầm được mô tả bên dưới. 
7. Đặt (d_1=0). Hàng đợi ưu tiên ban đầu chứa trạm (1) có khóa (a_1). Đối với mọi trạm khác, khoảng cách của nó ban đầu là không xác định. 
8. Khi trạm (u) được bật lên, phím ưu tiên của nó là 

[ 
p=d_u+a_u. 
] 

Mỗi trạm có thể đến được bằng một xe buýt từ (u) có thể nhận được khoảng cách (p). 
9. Xử lý mọi cấp độ trọng tâm của (u). Đặt (x) là trọng tâm ở mức đó và đặt (r=d_{tree}(u,x)). Mỗi đỉnh (v) trong danh sách centroid với 

[ 
d_{cây}(x,v)\le f_u-r 
] 

là một ứng cử viên khoảng cách cây hợp lệ. Sử dụng tiền tố của danh sách đã sắp xếp bằng con trỏ. 
10. Xử lý mọi điểm cuối cạnh ngoài đã chọn (x). Đặt (r=d_G(u,x)). Trong danh sách BFS của (x), sử dụng mọi đỉnh (v) vẫn chưa được xử lý thỏa mãn 

[ 
d_G(x,v)\le f_u-r. 
] 

Một đỉnh như vậy có thể đến được từ (u) trong phạm vi (f_u) đường cao tốc. 
11. Bất cứ khi nào tìm thấy một đỉnh chưa thăm (v), đặt 

[ 
d_v=p 
] 

và chèn ((d_v+a_v,v)) vào hàng ưu tiên. Một đỉnh chỉ được chèn một lần. 
12. Lặp lại cho đến khi hàng ưu tiên trống. Khoảng cách kết quả là chi phí tối thiểu cho ngày cố định này. 
13. Chạy quy trình tương tự cho (T=T_{\max}). Lấy kết quả nhỏ hơn trong hai kết quả một cách độc lập cho mỗi đích đến. 

### Tại sao nó hoạt động 

Điều bất biến là khi một trạm (v) được gán khoảng cách lần đầu tiên thì việc gán khoảng cách đó đã là tối ưu. Hàng đợi ưu tiên sắp xếp các trạm theo (d_u+a_u), chính xác là giá trị mà (u) sẽ cung cấp cho mọi điểm đến trong bán kính xe buýt của nó. Bất kỳ tuyến đường nào trong tương lai có thể cải thiện (v) sẽ phải xuất phát từ một trạm có giá trị riêng không lớn hơn phương án đó, do đó trạm đó sẽ được xử lý trước. Do đó, nhiệm vụ đầu tiên là khoảng cách cuối cùng theo kiểu Dijkstra. 

Đối với phần cây, mọi đường dẫn chỉ sử dụng các cạnh của cây bao trùm đều có khoảng cách cây chính xác. Phân rã centroid tìm mọi đỉnh có khoảng cách cây từ (u) tối đa là (f_u). Điều kiện sử dụng (d(u,x)+d(x,v)) đôi khi có thể mạnh hơn khoảng cách thực tế của cây, nhưng nó không bao giờ chấp nhận một đỉnh không hợp lệ vì nó chỉ giới hạn trên một tuyến cây hợp lệ. 

Đối với phần ngoài cạnh, hãy chọn bất kỳ đường đi ngắn nhất nào sử dụng cạnh không phải là cây. Đường dẫn chứa cả hai điểm cuối của cạnh đó, bao gồm cả điểm cuối được chọn để xử lý trước. Do đó đối với điểm cuối được chọn (x), 

[ 
d_G(u,x)+d_G(x,v)=d_G(u,v). 
] 

Cấu trúc BFS cho (x) do đó tìm thấy mọi đích có tuyến đường ngắn nhất sử dụng cạnh bổ sung đó. Vì mọi đường dẫn chỉ sử dụng các cạnh cây hoặc ít nhất một cạnh bổ sung nên mọi chuyển đổi xe buýt hợp lệ đều được biểu diễn. 

Cuối cùng, mọi tuyến đường đều có chi phí tuyến tính tính bằng (T), vì vậy ngày tốt nhất của tuyến đường đó là (1) hoặc (T_{\max}). Lấy kết quả tối thiểu của hai kết quả đường đi ngắn nhất có ngày cố định sẽ cho câu trả lời tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array
from collections import deque
import heapq

def solve():
    input = sys.stdin.readline

    n, m, tmax = map(int, input().split())

    f = [0] * (n + 1)
    c = [0] * (n + 1)
    w = [0] * (n + 1)

    for i in range(1, n + 1):
        f[i], c[i], w[i] = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    tree = [[] for _ in range(n + 1)]

    dsu = list(range(n + 1))
    dsu_size = [1] * (n + 1)

    def find(x):
        while dsu[x] != x:
            dsu[x] = dsu[dsu[x]]
            x = dsu[x]
        return x

    extra_sources = []
    seen_extra = bytearray(n + 1)

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

        ru = find(u)
        rv = find(v)

        if ru != rv:
            if dsu_size[ru] < dsu_size[rv]:
                ru, rv = rv, ru
            dsu[rv] = ru
            dsu_size[ru] += dsu_size[rv]

            tree[u].append(v)
            tree[v].append(u)
        else:
            if not seen_extra[u]:
                seen_extra[u] = 1
                extra_sources.append(u)

    del dsu
    del dsu_size
    del seen_extra

    # Centroid decomposition of the spanning tree.
    #
    # At every decomposition level, center[level][v] is the centroid
    # of v's current component, and cd_dist[level][v] is the tree
    # distance from v to that centroid.
    levels = n.bit_length() + 1

    center = [
        array('i', [0]) * (n + 1)
        for _ in range(levels)
    ]
    cd_dist = [
        array('i', [0]) * (n + 1)
        for _ in range(levels)
    ]

    # For every centroid x:
    # vec_v[x] is the vertices in its component in BFS order.
    # vec_d[x] contains their distances from x in the same order.
    vec_v = [None] * (n + 1)
    vec_d = [None] * (n + 1)

    removed = bytearray(n + 1)
    temp_parent = array('i', [0]) * (n + 1)
    subtree_size = array('i', [0]) * (n + 1)

    tasks = [(1, 0)]
    while tasks:
        start, level = tasks.pop()

        # Collect this component.
        order = []
        stack = [start]
        temp_parent[start] = 0

        while stack:
            u = stack.pop()
            order.append(u)

            pu = temp_parent[u]
            for v in tree[u]:
                if removed[v] or v == pu:
                    continue
                temp_parent[v] = u
                stack.append(v)

        total = len(order)

        # Compute subtree sizes with respect to the temporary root.
        for u in reversed(order):
            s = 1
            for v in tree[u]:
                if removed[v]:
                    continue
                if temp_parent[v] == u:
                    s += subtree_size[v]
            subtree_size[u] = s

        # Find a centroid.
        centroid = start
        best = total + 1

        for u in order:
            largest = total - subtree_size[u]

            for v in tree[u]:
                if removed[v]:
                    continue
                if temp_parent[v] == u and subtree_size[v] > largest:
                    largest = subtree_size[v]

            if largest < best:
                best = largest
                centroid = u

        # BFS from the centroid inside this component.
        vv = array('i')
        dd = array('i')

        q = deque([centroid])
        temp_parent[centroid] = 0
        center[level][centroid] = centroid
        cd_dist[level][centroid] = 0

        while q:
            u = q.popleft()
            du = cd_dist[level][u]

            vv.append(u)
            dd.append(du)

            for v in tree[u]:
                if removed[v] or v == temp_parent[u]:
                    continue

                temp_parent[v] = u
                center[level][v] = centroid
                cd_dist[level][v] = du + 1
                q.append(v)

        vec_v[centroid] = vv
        vec_d[centroid] = dd

        removed[centroid] = 1

        # After removing the centroid, each remaining neighbor starts
        # an independent component.
        next_level = level + 1
        for v in tree[centroid]:
            if not removed[v]:
                tasks.append((v, next_level))

    del removed
    del temp_parent
    del subtree_size

    # For each selected endpoint of an extra edge, run BFS in the
    # original graph. BFS order is already sorted by distance.
    key_vertices = []
    key_distances = []

    for source in extra_sources:
        dist = array('i', [-1]) * (n + 1)
        vertices = array('i')

        dist[source] = 0
        q = deque([source])

        while q:
            u = q.popleft()
            vertices.append(u)

            nd = dist[u] + 1
            for v in graph[u]:
                if dist[v] == -1:
                    dist[v] = nd
                    q.append(v)

        key_distances.append(dist)
        key_vertices.append(vertices)

    key_count = len(key_vertices)

    # Bits needed to encode a vertex are no longer needed here because
    # centroid vertices and distances are stored separately.
    def dijkstra(day_offset, best_answer):
        cost = [0] * (n + 1)
        for i in range(1, n + 1):
            cost[i] = c[i] + w[i] * day_offset

        dis = [-1] * (n + 1)
        dis[1] = 0

        # Each centroid list is consumed monotonically.
        ptr = [0] * (n + 1)

        # Each extra-edge BFS list is also consumed monotonically.
        ptr_key = [0] * key_count

        heap = [(cost[1], 1)]

        while heap:
            p, u = heapq.heappop(heap)

            # p = dis[u] + cost[u].
            if best_answer[u] > dis[u]:
                best_answer[u] = dis[u]

            fu = f[u]

            # Tree-distance transitions through centroid decomposition.
            for level in range(levels):
                x = center[level][u]
                if x == 0:
                    break

                remaining = fu - cd_dist[level][u]
                if remaining < 0:
                    continue

                vv = vec_v[x]
                dd = vec_d[x]

                j = ptr[x]
                size_v = len(vv)

                while j < size_v and dd[j] <= remaining:
                    v = vv[j]

                    if dis[v] == -1:
                        dis[v] = p
                        heapq.heappush(
                            heap,
                            (p + cost[v], v)
                        )

                    j += 1

                ptr[x] = j

            # Transitions whose route uses at least one non-tree edge.
            for z in range(key_count):
                kd = key_distances[z]
                kv = key_vertices[z]

                remaining = fu - kd[u]
                if remaining < 0:
                    continue

                j = ptr_key[z]
                size_v = len(kv)

                while j < size_v and kd[kv[j]] <= remaining:
                    v = kv[j]

                    if dis[v] == -1:
                        dis[v] = p
                        heapq.heappush(
                            heap,
                            (p + cost[v], v)
                        )

                    j += 1

                ptr_key[z] = j

        return best_answer

    INF = 10**30
    answer = [INF] * (n + 1)

    dijkstra(0, answer)

    if tmax > 1:
        dijkstra(tmax - 1, answer)

    sys.stdout.write(
        '\n'.join(str(answer[i]) for i in range(1, n + 1))
    )

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ biểu đồ hoàn chỉnh vì các hoạt động BFS ngoài biên cần các đường cao tốc ban đầu. Đồng thời, DSU chọn cây bao trùm. Mỗi cạnh bị loại bỏ là một cạnh phụ và có thể có nhiều nhất (m-n+1\le51) trong số chúng. 

Quá trình tiền xử lý trung tâm là lặp lại chứ không phải đệ quy. Điều này tránh giới hạn đệ quy của Python và cũng tránh duy trì một ngăn xếp cuộc gọi đệ quy lớn. Đối với mỗi thành phần trung tâm, DFS tạm thời xác định kích thước cây con và xác định trọng tâm, sau đó BFS ghi lại các đỉnh theo thứ tự khoảng cách không giảm. 

các`array`mô-đun được sử dụng có chủ ý. Danh sách Python chứa vài triệu số nguyên Python tiêu tốn nhiều bộ nhớ hơn một mảng số nguyên được đóng gói. Các cấu trúc khoảng cách trung tâm chứa các số nguyên (O(n\log n)), trong khi các cấu trúc BFS ngoài cạnh chứa các số nguyên (O(51n)). Các mảng được đóng gói giữ các cấu trúc này trong phạm vi bộ nhớ. 

Hai mảng trung tâm được lập chỉ mục theo mức độ phân tách. Đối với một đỉnh (u),`center[level][u]`cung cấp trọng tâm có liên quan và`cd_dist[level][u]`mang lại khoảng cách cho nó. Điều này thay thế LCA và bảng thưa được sử dụng bởi triển khai C++ điển hình trong khi vẫn giữ mọi truy vấn trung tâm (O(\log n)). 

Mảng con trỏ được đặt lại cho mỗi lần chạy Dijkstra. Một con trỏ không bao giờ được di chuyển lùi lại. Bán kính nhỏ hơn trong truy vấn sau này là vô hại vì mọi mục nhập trước con trỏ đã được kiểm tra và bất kỳ đỉnh nào vẫn chưa được kiểm tra khi kiểm tra sẽ nhận được khoảng cách Dijkstra tối ưu. 

Các cửa hàng xếp hàng ưu tiên`dis[u] + cost[u]`, thay vì chỉ`dis[u]`. Giá trị này chính xác là chi phí thu được sau khi mua thêm một vé tại (u). Vì tất cả các chuyến chuyển tiếp xe buýt đi từ (u) đều có cùng mức giá nên mọi điểm đến được phát hiện từ (u) đều có cùng một ứng viên. Quan trọng hơn, mỗi đỉnh chỉ được gán trong lần khám phá đầu tiên của nó, do đó chỉ xảy ra việc chèn (O(n)) đống. 

biểu thức`c[i] + w[i] * day_offset`sử dụng chênh lệch ngày dựa trên số không. Ngày (1) tương ứng với độ lệch (0) và ngày (T_{\max}) tương ứng với độ lệch (T_{\max}-1). Đây là chi tiết chính từng bước một trong quá trình triển khai. 

Số nguyên Python không bị tràn, do đó phép nhân trung gian liên quan đến (w_i) vẫn an toàn ngay cả khi giá trị toán học lớn hơn nhiều so với số nguyên 32 bit. Vấn đề đảm bảo rằng giá vé thực tế vẫn nằm trong giới hạn đã nêu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6 6 2
1 50 -40
1 2 100
2 1 100
2 4 100
3 1 100
1 1 100
1 2
2 3
3 4
4 2
2 5
6 1
```Đối với ngày (1), giá vé là (50,2,1,2,1,1). Các trạng thái Dijkstra quan trọng là: 

| Ngày | Trạm xuất hiện | Khóa hiện tại (p) | Trạm mới đến | Khoảng cách cuối cùng bị ảnh hưởng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 50 | 2, 6 | (d_2=50,\ d_6=50) | 
| 1 | 6 | 51 | không | không thay đổi | 
| 1 | 2 | 52 | 3, 5 | (d_3=52,\ d_5=52) | 
| 1 | 3 | 53 | 4 | (d_4=52) | 
| 1 | 5 | 53 | không | không thay đổi | 
| 1 | 4 | 54 | không | không thay đổi | 

Chi phí thu được là (0,50,52,52,52,50) cho các điểm đến. 

Đối với ngày (2), giá vé trở thành (10,102,101,102,101,101): 

| Ngày | Trạm xuất hiện | Khóa hiện tại (p) | Trạm mới đến | Khoảng cách cuối cùng bị ảnh hưởng | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 10 | 2, 6 | (d_2=10,\ d_6=10) | 
| 2 | 2 | 112 | 3, 5 | (d_3=112,\ d_5=112) | 
| 2 | 6 | 111 | không | không thay đổi | 
| 2 | 3 | 213 | 4 | (d_4=112) | 

Lấy kết quả nhỏ hơn trong hai ngày sẽ cho```
0
10
52
52
52
10
```Dấu vết cũng minh họa tại sao khóa hàng đợi ưu tiên lại được`distance + current ticket cost`. Ví dụ: trạm (2) đạt được khoảng cách (50) vào ngày (1) và phím chuyển tiếp đi của nó là (50+2=52). 

### Mẫu 2 

Hãy xem xét trường hợp được xây dựng sau đây:```
3 2 3
2 10 -4
1 100 0
1 100 0
1 2
2 3
```Trạm (1) có bán kính (2) nên có thể đến trực tiếp cả hai trạm còn lại. 

| Ngày | Giá vé tại 1 | Trạm xuất hiện | Chìa khóa (p) | Trạm mới đến | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 1 | 10 | 2, 3 | 
| 1 | 10 | 2 | 110 | không | 
| 1 | 10 | 3 | 110 | không | 
| 3 | 2 | 1 | 2 | 2, 3 | 
| 3 | 2 | 2 | 102 | không | 
| 3 | 2 | 3 | 102 | không | 

Các câu trả lời cuối cùng là```
0
2
2
```Dấu vết này xác nhận đối số điểm cuối. Ngày trung gian không bao giờ cần thiết, mặc dù giá vé thay đổi theo ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n + Kn)) | Chi phí tiền xử lý Centroid (O(n\log n)), chi phí chạy BFS ngoại biên (K\le51) (O(Kn)) và mỗi lần chạy Dijkstra đều thực hiện (O(n\log n+Kn)) hoạt động | 
| Không gian | (O(n\log n+Kn)) | Danh sách centroid chứa các mục nhập (O(n\log n)) và các cấu trúc khoảng cách ngoài cạnh chứa các mục nhập (O(Kn)) | 

Đây (K\le m-n+1\le51). Với (n\le200000), phần ngoài cạnh được giới hạn bởi khoảng (10,2) triệu mục nhập khoảng cách đỉnh, trong khi phân tách trọng tâm chỉ đóng góp (O(n\log n)) mục nhập. Hai lần chạy Dijkstra là những phần duy nhất phụ thuộc vào thông số ngày và chỉ có hai trong số đó. Đây là độ phức tạp của biểu đồ thưa thớt dự định cho vấn đề. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây gọi`solve()`hoạt động từ giải pháp trên. Trình trợ giúp thay thế đầu vào tiêu chuẩn và ghi lại đầu ra tiêu chuẩn.```python
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

# Provided sample
assert run("""\
6 6 2
1 50 -40
1 2 100
2 1 100
2 4 100
3 1 100
1 1 100
1 2
2 3
3 4
4 2
2 5
6 1
""") == """\
0
10
52
52
52
10""", "sample 1"

# Minimum-size graph.
assert run("""\
1 0 3
1 10 -5
""") == """\
0""", "single station"

# Negative w_i makes the last day optimal.
assert run("""\
2 1 3
1 10 -4
1 1 0
1 2
""") == """\
0
2""", "last-day optimum"

# All values equal, plus an extra edge creating a shortcut.
assert run("""\
3 3 1
1 3 0
1 3 0
1 3 0
1 2
2 3
1 3
""") == """\
0
3
3""", "extra-edge shortcut"

# Maximum n, a tree, f_i = 1 exactly at the radius boundary.
n = 200000
stations = "1 1 0\n" * n
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))

max_case = f"{n} {n - 1} 1\n" + stations + edges + "\n"
max_expected = "".join(f"{i}\n" for i in range(n))

assert run(max_case) == max_expected, "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=1,m=0) |`0`| Trạm xuất phát không cần vé | 
| Hai trạm với (w_1<0) |`0, 2`| Ngày tối ưu có thể là (T_{\max}) | 
| Tam giác có giá trị bằng nhau |`0, 3, 3`| Đường đi ngắn nhất cực kỳ và thông số vé bằng nhau | 
| đường dẫn (n=200000) với (f_i=1) |`0,1,2,...,199999`| Kích thước đầu vào tối đa và ranh giới bán kính chính xác | 

## Vỏ cạnh 

### Trạm xuất phát 

cho```
1 0 3
1 10 -5
```cấu trúc trung tâm chỉ chứa trạm (1). Dijkstra bắt đầu với`dis[1] = 0`, và đích đến đã được xác định. Đầu ra cuối cùng là`0`. Không cần chuyển đổi xe buýt. 

### Tối ưu ngày cuối cùng 

cho```
2 1 3
1 10 -4
1 1 0
1 2
```lượt chạy Dijkstra đầu tiên sử dụng giá vé (10) tại ga (1). Lượt chạy thứ 2 sử dụng giá vé 

[ 
10+(3-1)(-4)=2. 
] 

Đạt được đích với chi phí (2) trong lần chạy thứ hai, vì vậy đầu ra cuối cùng là`0, 2`. Đây chính xác là tình huống được xử lý bằng cách đánh giá cả hai ngày điểm cuối. 

### Phím tắt không phải cây 

cho```
3 3 1
1 3 0
1 3 0
1 3 0
1 2
2 3
1 3
```giả sử cây bao trùm chứa (1-2) và (2-3). Khoảng cách của cây từ (1) đến (3) là (2), nằm ngoài bán kính của trạm (1). Cạnh (1-3) là cạnh phụ nên trạm (1) trở thành một trong những nguồn BFS đặc biệt. Khoảng cách BFS của nó tới (3) là (1), thỏa mãn (f_1=1). Do đó, cấu trúc cạnh phụ gán khoảng cách (3) cho trạm (3), đưa ra`0, 3, 3`. 

### Bán kính chính xác 

cho```
3 2 1
2 7 0
1 100 0
1 100 0
1 2
2 3
```khoảng cách cây từ trạm (1) đến trạm (3) chính xác là (2). Trong truy vấn trung tâm, điều kiện là`distance <= remaining`, không phải là một bất đẳng thức chặt chẽ. Vì (2\le f_1=2), trạm (3) đạt được với chi phí (7). Đầu ra là`0, 7, 7`. 

Sự so sánh toàn diện tương tự được sử dụng trong cấu trúc BFS cho các cạnh phụ. Điểm đến chính xác nằm trên ranh giới bán kính xe buýt phải luôn được xử lý. 

Nếu bạn muốn, tôi cũng có thể cung cấp một phiên bản biên tập cuộc thi ngắn hơn tập trung vào hai ý tưởng chính hoặc bản dẫn xuất từng dòng của việc triển khai Python.
