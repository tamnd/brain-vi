---
title: "CF 102391E - Hội xương rồng chết"
description: "Chúng tôi bắt đầu với một cây xương rồng có trọng lượng được kết nối. Mỗi đỉnh ban đầu (v) có một giá trị chữa lành (RVv) và mỗi cạnh ban đầu (e) có độ dài (Le) và một giá trị chữa lành (REE). Đối với mỗi chu kỳ, phải loại bỏ chính xác một cạnh. Việc xóa một cạnh (e={u,v}) không chỉ xóa nó."
date: "2026-08-14T14:13:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "E"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 498
verified: false
draft: false
---

[CF 102391E - Hội xương rồng chết](https://codeforces.com/problemset/problem/102391/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 18 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một cây xương rồng có trọng lượng được kết nối. Mỗi đỉnh ban đầu (v) có một giá trị chữa lành (RV_v) và mỗi cạnh ban đầu (e) có độ dài (L_e) và một giá trị chữa lành (RE_e). 

Đối với mỗi chu kỳ, phải loại bỏ chính xác một cạnh. Việc xóa một cạnh (e={u,v}) không chỉ xóa nó. Thay vào đó, một lá mới được gắn vào (u) với độ dài cạnh (RV_u+RE_e) và một lá mới khác được gắn vào (v) với độ dài cạnh (RV_v+RE_e). Sau khi thực hiện điều này trong mỗi chu kỳ, đồ thị sẽ là một cái cây. Chúng tôi muốn đường kính nhỏ nhất có thể của cây cuối cùng đó. 

Đầu vào chứa tối đa (100.000) đỉnh gốc và (150.000) cạnh. Vì một đồ thị liên thông có (M-N+1) chu trình độc lập ở đây nên có thể có gần (50.000) chu trình. Độ dài cạnh và giá trị chữa lành đạt đến (10^9), vì vậy câu trả lời có thể nằm trong khoảng (10^{14}). Điều này loại trừ việc liệt kê tất cả các lựa chọn của các cạnh bị loại bỏ và nó cũng có nghĩa là mọi phép tính khoảng cách phải sử dụng số nguyên 64 bit. Giải pháp (O((N+M)\log V)) phù hợp với giới hạn (10) giây. 

Có một số cách mà một giải pháp bất cẩn có thể thất bại. 

Hãy xem xét cây xương rồng nhỏ nhất có thể, một hình tam giác có tất cả các chiều dài cạnh ban đầu bằng (1), tất cả các giá trị chữa lành đỉnh (0) và tất cả các giá trị chữa lành các cạnh (1).```
3 3
0 0 0
1 2 1 1
2 3 1 1
3 1 1 1
```Câu trả lời là (4). Sau khi cắt bất kỳ cạnh nào, hai cạnh ban đầu còn lại tạo thành một đường dẫn có chiều dài (2) và hai cạnh lành mới thêm một cạnh khác (2). Giải pháp chỉ tính đường kính giữa các đỉnh ban đầu sẽ báo cáo sai (2). 

Cái bẫy thứ hai là cạnh tốt nhất để cắt không nhất thiết phải là cạnh ngắn nhất. Mẫu 1 chứa một hình tam giác trong đó cạnh cắt (1\text{-}2), cạnh ngắn nhất, cho đường kính (12), trong khi cạnh cắt (2\text{-}3) cho đường kính tối ưu (10). Giá trị chữa lành của cạnh được chọn sẽ thay đổi cả hai lá mới, do đó chỉ chiều dài cạnh ban đầu là không đủ.```
3 3
1 2 3
3 1 2 3
1 2 1 2
2 3 3 1
```Sai lầm phổ biến thứ ba là quên cầu nối. Chúng không bao giờ bị cắt, nhưng chúng vẫn đóng góp chiều dài cạnh thông thường của chúng cho mọi đường đi qua chúng. Ví dụ,```
4 4
0 0 0 0
1 2 1 1
2 3 1 1
3 1 1 1
3 4 10 1
```có câu trả lời (12). Tam giác được chia thành một đường đi và cây cầu có chiều dài (10) vẫn gắn với đỉnh (3). Việc coi mọi thành phần được kết nối hai chiều như một chu trình có thể được sửa đổi tự do sẽ tạo ra trạng thái không hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là liệt kê cạnh bị loại bỏ khỏi mỗi chu kỳ. Nếu các chu trình có độ dài (c_1,c_2,\ldots,c_k), thì có 

[ 
\prod_{i=1}^{k}c_i 
] 

cây cuối cùng có thể. Đối với mỗi lựa chọn, chúng ta có thể xây dựng cây kết quả và chạy thuật toán đường kính cây trong (O(N+M)). 

Điều này đúng vì mọi cây cuối cùng hợp pháp đều tương ứng với chính xác một lựa chọn của một cạnh trong mỗi chu kỳ. Vấn đề là số lượng lựa chọn. Một cây xương rồng được làm từ khoảng (50.000) hình tam giác có chung một đỉnh khớp nối có khoảng (3^{50.000}) lựa chọn khác nhau. Ngay cả một thao tác cho mỗi lựa chọn cũng đã là vô vọng và việc tính toán lại đường kính sẽ tạo ra lực lượng mạnh mẽ (O(3^{50.000}(N+M))). 

Quan sát hữu ích là mục tiêu là mức tối đa tối thiểu có thể. Điều đó ngay lập tức gợi ý tìm kiếm nhị phân. Cố định đường kính dự kiến ​​(R) và hỏi xem liệu có cách nào để cắt cây xương rồng có đường kính cuối cùng lớn nhất là (R) hay không. Điều kiện khả thi này là đơn điệu: nếu (R) khả thi thì mọi giá trị lớn hơn cũng khả thi. 

Câu hỏi còn lại là làm thế nào để kiểm tra một giá trị của (R) một cách hiệu quả. Một cây xương rồng có khả năng phân hủy rất hữu ích. Mỗi thành phần được kết nối hai chiều là một cầu nối đơn hoặc một chu trình đơn giản. Việc thay thế mỗi chu trình bằng một nút khối riêng biệt sẽ tạo ra một cây cắt khối. Cây này chứa tất cả các lựa chọn cấu trúc trong khi mỗi chu trình riêng lẻ có thể được xử lý như một đối tượng cục bộ. Hướng dẫn cuộc thi chính thức sử dụng chính xác sự phân tách này cùng với chương trình động DFS đảo ngược và tìm kiếm nhị phân. 

Đối với mỗi nút (u) của phân tách này, hãy xác định (d_u) là khoảng cách tối thiểu có thể từ (u) đến đỉnh xa nhất của nó bên trong cây con đã được xử lý, với điều kiện mọi đường kính bên trong cây con đó lớn nhất là (R). Nếu không có lựa chọn hợp lệ thì (d_u) là vô cùng. 

Đối với một đỉnh ban đầu thông thường, quá trình chuyển đổi là quá trình chuyển đổi cây tiêu chuẩn. Mỗi đứa trẻ đóng góp khoảng cách xa nhất của nó và hai đóng góp lớn nhất phải có tổng nhiều nhất là (R). Đóng góp lớn nhất trở thành (d_u). 

Một chu trình phức tạp hơn vì chúng ta có thể cắt bất kỳ cạnh nào của nó. Khi một cạnh bị cắt, chu trình sẽ trở thành một đường dẫn, với một lá chữa lành ở mỗi điểm cuối. Nếu chu trình được xem từ đỉnh khớp nối gốc của nó, mọi đường cắt có thể sẽ tách nó thành một đường bên trái và một đường bên phải. Chúng tôi có thể quét cả hai mặt bằng lập trình động tiền tố và hậu tố, do đó, mọi lần cắt có thể được đánh giá trong thời gian không đổi sau quá trình tiền xử lý tuyến tính cho chu trình. 

Thực tế cấu trúc quan trọng là chỉ cần khoảng cách từ mỗi đỉnh chu kỳ đến cây con đính kèm của nó. Khi đã biết những giá trị đó, phần còn lại của chu trình chỉ là một đường dẫn có trọng số cộng với hai lá chữa lành ứng cử viên. Đó là cái làm giảm sự lựa chọn hàm mũ rõ ràng trên các cạnh của chu kỳ thành công tuyến tính trên mỗi chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^{\Theta(N)}(N+M))) trong trường hợp xấu nhất | (O(N+M)) | Quá chậm | 
| Tối ưu | (O((N+M)\log V)) | (O(N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tách cây xương rồng thành các cây cầu và các chu trình đơn giản bằng thuật toán hai thành phần của Tarjan. 

Đối với mỗi cây cầu, hãy kết nối trực tiếp hai đỉnh ban đầu của nó trong một cây cắt khối mới. Với mỗi chu trình, tạo thêm một nút chu trình và kết nối nó với mọi đỉnh thuộc chu trình đó. Biểu đồ kết quả là một cây vì mỗi chu kỳ được thay thế bằng một nút khối duy nhất. 

Đối với mỗi chu kỳ, chúng tôi cũng bảo toàn các đỉnh của nó theo thứ tự tuần hoàn và ID cạnh gốc tương ứng. Thứ tự này cần thiết khi đánh giá mọi cạnh có thể bị loại bỏ. 

1. Tìm kiếm nhị phân câu trả lời.

Gọi (R) là đường kính ứng cử viên hiện tại. Việc kiểm tra tính khả thi sẽ xác định liệu một số lần cắt hợp pháp có tạo ra một cây có đường kính tối đa (R) hay không. 

Giới hạn trên đạt được bằng cách lấy giá trị lớn nhất trong số tất cả các độ dài cạnh ban đầu và tất cả các độ dài cạnh lành có thể có. Đường kính cuối cùng sử dụng tối đa (N-1) cạnh cây thông thường và tối đa hai cạnh lành trong mỗi chu kỳ, do đó 

\maxWeight\left(N-1+2(M-N+1)\right). 
] 

1. Xử lý cây cắt khối theo thứ tự DFS ngược. 

Đối với mỗi đỉnh ban đầu (u), tất cả các cây con con đã tạo ra các giá trị (d) của chúng. Nếu một đỉnh con là một đỉnh ban đầu được nối với nhau bằng một cây cầu có chiều dài (w), thì phần đóng góp của nó là (d_v+w). Nếu khối con là một khối chu kỳ, thì (d_v) của nó đã bao gồm toàn bộ đường chu trình từ (u), do đó không có độ dài cạnh bổ sung nào được thêm vào. 

Giữ nguyên hai đóng góp lớn nhất (x) và (y). Nếu (x+y>R), đường kính dự kiến ​​là không thể. Nếu không thì đặt (d_u=x). 

1. Xử lý khối chu kỳ với đỉnh cha của nó làm trục xoay. 

Giả sử chu trình chứa các đỉnh 

[ 
v_0,v_1,\ldots,v_{L-1},v_L 
] 

trong đó (v_0=v_L) là trục xoay. Cho cạnh (e_i) kết nối (v_i) và (v_{i+1}). 

Đối với mọi đỉnh không xoay (v_i), (d_{v_i}) đều đã được biết. Xác định (l_i) là khoảng cách từ trục quay đến (v_i) dọc theo bên trái của chu trình: 

[ 
l_0=0,\qquad 
l_i=l_{i-1}+L_{e_{i-1}}. 
] 

Tương tự, xác định khoảng cách hậu tố (r_i) dọc theo hướng khác. 

Tập hợp giá trị tiền tố đầu tiên ghi lại khoảng cách xa nhất từ ​​trục quay: 

[ 
LD_i=\max_{1\le j\le i}(d_{v_j}+l_j). 
] 

Chúng tôi cũng cần 

[ 
LF_i=\max_{1\le j\le i}(d_{v_j}-l_j), 
] 

bởi vì đối với hai cây con (p<q), khoảng cách của chúng qua đường dẫn là 

[ 
(d_{v_p}-l_p)+(d_{v_q}+l_q). 
] 

Do đó, đường kính tốt nhất có điểm cuối ở bên trái có thể được cập nhật tăng dần bằng cách 

\max(LG_{i-1},LF_{i-1}+d_{v_i}+l_i). 
] 

Phía bên phải sử dụng mảng hậu tố đối xứng (RD,RF,RG). Giải pháp chính thức rút ra phép truy toán nửa bên trái tương tự từ hai biểu thức này cho khoảng cách cây con. 

1. Thêm lá chữa bệnh vào quá trình chuyển đổi chu kỳ. 

Giả sử cạnh (e_{i-1}) là cạnh chúng ta cắt. Điểm cuối của nó là (v_{i-1}) và (v_i), do đó chiều dài của hai lá mới là 

[ 
a=RV_{v_{i-1}}+RE_{e_{i-1}}, 
] 

và 

[ 
b=RV_{v_i}+RE_{e_{i-1}}. 
] 

Sau khi cắt, chu trình sẽ trở thành đường dẫn từ (v_{i-1}) đến (v_i). Mọi đường kính có thể có bên trong cấu trúc cục bộ này thuộc một trong sáu loại điểm cuối. 

Loại đầu tiên có cả hai điểm cuối ở phần bên trái. Giá trị của nó là đường kính tiền tố bên trái (LG_{i-1}). 

Loại thứ hai có cả hai điểm cuối ở phần bên phải. Giá trị của nó là (RG_i). 

Loại thứ ba có một điểm cuối ở cây con bên trái và một điểm cuối ở cây con bên phải. Giá trị của nó là (LD_{i-1}+RD_i). 

Loại thứ tư sử dụng lá chữa lành mới tại (v_{i-1}) và điểm cuối ở phần bên phải. Giá trị của nó là 

[ 
a+\max(LH_{i-1},l_{i-1}+RD_i). 
] 

Loại thứ năm là đối xứng, sử dụng lá chữa lành tại (v_i): 

[ 
b+\max(RH_i,r_{i+1}+LD_{i-1}). 
] 

Loại thứ sáu kết nối trực tiếp hai lá chữa bệnh mới thông qua đường vòng còn lại: 

[ 
a+l_{i-1}+r_{i+1}+b. 
] 

Lấy giá trị lớn nhất trong sáu giá trị này. Nếu mức tối đa đó nhiều nhất là (R), mức cắt cụ thể này là hợp lệ. 

Để cắt hợp lệ, khoảng cách từ trục quay đến đỉnh xa nhất trong chu trình được xử lý này là 

[ 
D= 
\max( 
LD_{i-1}, 
RD_i, 
a+l_{i-1}, 
b+r_{i+1} 
). 
] 

Khối chu kỳ lấy (D) mức tối thiểu như vậy trên tất cả các cạnh cắt có thể có của nó. 

1. Từ chối ứng viên ngay khi cây con không có trạng thái hợp lệ. 

Nếu một đỉnh ban đầu có hai nhánh con có khoảng cách tổng hợp vượt quá (R), thì không có lựa chọn nào ngoài cây con đó có thể sửa chữa vi phạm. Tương tự như vậy, nếu mọi vết cắt có thể có trong một chu trình đều vượt quá (R), thì chu trình đó không thể là một phần của nghiệm có đường kính nhiều nhất (R). 

1. Sau khi xử lý nghiệm, ứng viên (R) khả thi chính xác khi nghiệm có giá trị (d) hữu hạn. 

Tìm kiếm nhị phân sau đó trả về giá trị khả thi nhỏ nhất (R). 

### Tại sao nó hoạt động

Bất biến là (d_u) là khoảng cách xa nhất có thể nhỏ nhất đến (u) trong số tất cả các cấu hình hợp lệ của cây con được xử lý, với mọi đường kính trong nhiều nhất là (R). 

Đối với một đỉnh thông thường, chỉ có hai nhánh con sâu nhất mới có thể tạo thành đường kính đi qua đỉnh đó, do đó việc duy trì độ sâu tối đa nhỏ nhất có thể là đủ. Đối với khối chu kỳ, khi cạnh cắt của nó được cố định, khối sẽ trở thành một đường dẫn và mọi đường kính có thể có đều có điểm cuối thuộc chính xác một trong sáu loại trên. Tiền tố và hậu tố cực đại tính toán giá trị tốt nhất cho từng danh mục mà không cần liệt kê các cặp đỉnh. Lấy độ sâu tối thiểu trên mỗi lần cắt hợp lệ sẽ duy trì chính xác trạng thái tốt nhất có thể cho cha mẹ. 

Do đó, nghiệm khả thi chính xác khi một tập hợp các đường cắt chu trình hợp pháp nào đó tạo ra một cây có đường kính tối đa là (R). Vì tính khả thi là đơn điệu nên tìm kiếm nhị phân cho đường kính tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

INF = 10**30

def solve():
    n, m = map(int, input().split())
    rv = [0] + list(map(int, input().split()))

    U = [0] * (m + 1)
    V = [0] * (m + 1)
    W = [0] * (m + 1)
    RE = [0] * (m + 1)

    g = [[] for _ in range(n + 1)]

    max_base = 0

    for eid in range(1, m + 1):
        a, b, w, re = map(int, input().split())
        U[eid] = a
        V[eid] = b
        W[eid] = w
        RE[eid] = re
        g[a].append((b, eid))
        g[b].append((a, eid))
        max_base = max(max_base, w, rv[a] + re, rv[b] + re)

    # Tarjan decomposition.
    dfn = [0] * (n + 1)
    low = [0] * (n + 1)
    edge_stack = []

    # Block-cut tree.
    # Original vertices are 1..n.
    # Cycle block vertices are n+1..cnt.
    t = [[] for _ in range(n + 2)]

    # cycle_info[node] = (vertices, edge_ids)
    # vertices has length len(edge_ids)+1 and starts/ends at the pivot.
    cycle_info = [None] * (n + 1)

    timer = 0
    cnt = n
    max_cycle_len = 0

    def add_component(pivot, edges):
        nonlocal cnt, max_cycle_len

        if len(edges) == 1:
            eid = edges[0]
            a = U[eid]
            b = V[eid]
            t[a].append((b, eid))
            t[b].append((a, eid))
            return

        cnt += 1
        node = cnt

        # A cactus biconnected component with more than one edge
        # is a simple cycle. Build its cyclic order locally.
        local = {}
        for eid in edges:
            a = U[eid]
            b = V[eid]
            local.setdefault(a, []).append(eid)
            local.setdefault(b, []).append(eid)

        verts = [pivot]
        eids = []

        cur = pivot
        prev_eid = 0

        while True:
            choices = local[cur]
            if choices[0] != prev_eid:
                eid = choices[0]
            else:
                eid = choices[1]

            eids.append(eid)

            a = U[eid]
            b = V[eid]
            nxt = b if a == cur else a

            verts.append(nxt)

            prev_eid = eid
            cur = nxt

            if cur == pivot:
                break

        cycle_info.append((verts, eids))
        max_cycle_len = max(max_cycle_len, len(eids))

        # Connect every cycle vertex to the block node.
        for i in range(len(eids)):
            v = verts[i]
            eid = eids[i]
            t[node].append((v, eid))
            t[v].append((node, eid))

    def tarjan(u, parent_eid):
        nonlocal timer

        timer += 1
        dfn[u] = low[u] = timer

        for v, eid in g[u]:
            if eid == parent_eid:
                continue

            if dfn[v] == 0:
                edge_stack.append(eid)
                tarjan(v, eid)

                if low[v] < low[u]:
                    low[u] = low[v]

                if low[v] >= dfn[u]:
                    comp = []
                    while True:
                        x = edge_stack.pop()
                        comp.append(x)
                        if x == eid:
                            break
                    add_component(u, comp)

            elif dfn[v] < dfn[u]:
                edge_stack.append(eid)
                if dfn[v] < low[u]:
                    low[u] = dfn[v]

    tarjan(1, 0)

    # Root the block-cut tree once.
    parent = [0] * (cnt + 1)
    parent[1] = -1
    order = [1]

    for u in order:
        for v, _ in t[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    # Reusable cycle arrays. Reusing them is much cheaper than allocating
    # ten fresh lists for every cycle on every binary-search iteration.
    size = max_cycle_len + 3

    lp = [0] * size
    rp = [0] * size
    ld = [0] * size
    rd = [0] * size
    lf = [0] * size
    rf = [0] * size
    lg = [0] * size
    rg = [0] * size
    lh = [0] * size
    rh = [0] * size

    def check(limit):
        d = [INF] * (cnt + 1)

        for u in reversed(order):
            if u <= n:
                best1 = 0
                best2 = 0

                for v, eid in t[u]:
                    if parent[v] != u:
                        continue

                    if v > n:
                        cur = d[v]
                    else:
                        cur = d[v] + W[eid]

                    if cur > best1:
                        best2 = best1
                        best1 = cur
                    elif cur > best2:
                        best2 = cur

                if best1 + best2 > limit:
                    return False

                if best1 > limit:
                    return False

                d[u] = best1
                continue

            verts, eids = cycle_info[u]
            L = len(eids)

            lp[0] = 0
            ld[0] = 0
            lf[0] = 0
            lh[0] = 0
            lg[0] = 0

            for i in range(1, L):
                lp[i] = lp[i - 1] + W[eids[i - 1]]
                x = d[verts[i]]
                ld[i] = max(ld[i - 1], x + lp[i])
                lf[i] = max(lf[i - 1], x - lp[i])
                lh[i] = max(lh[i - 1] + W[eids[i - 1]], x)

            if L >= 2:
                lg[1] = 0

            for i in range(2, L):
                x = d[verts[i]]
                lg[i] = max(
                    lg[i - 1],
                    lf[i - 1] + x + lp[i]
                )

            rp[L + 1] = 0
            for i in range(L, 1, -1):
                rp[i] = rp[i + 1] + W[eids[i - 1]]

            rd[L] = 0
            rf[L] = 0
            rh[L] = 0
            rg[L] = 0

            for i in range(L - 1, 0, -1):
                x = d[verts[i]]
                rd[i] = max(rd[i + 1], x + rp[i + 1])
                rf[i] = max(rf[i + 1], x - rp[i + 1])
                rh[i] = max(rh[i + 1] + W[eids[i]], x)

            if L >= 2:
                rg[L - 1] = 0

            for i in range(L - 2, 0, -1):
                x = d[verts[i]]
                rg[i] = max(
                    rg[i + 1],
                    rf[i + 1] + x + rp[i + 1]
                )

            best_depth = INF

            for i in range(1, L + 1):
                eid = eids[i - 1]

                a = rv[verts[i - 1]] + RE[eid]
                b = rv[verts[i]] + RE[eid]

                r1 = lg[i - 1]
                r2 = rg[i]
                r3 = ld[i - 1] + rd[i]
                r4 = a + max(lh[i - 1], lp[i - 1] + rd[i])
                r5 = b + max(rh[i], rp[i + 1] + ld[i - 1])
                r6 = a + lp[i - 1] + rp[i + 1] + b

                diameter = max(r1, r2, r3, r4, r5, r6)

                if diameter <= limit:
                    depth = max(
                        ld[i - 1],
                        rd[i],
                        a + lp[i - 1],
                        b + rp[i + 1]
                    )
                    if depth < best_depth:
                        best_depth = depth

            if best_depth > limit:
                return False

            d[u] = best_depth

        return True

    cycles = m - n + 1
    lo = 0
    hi = max_base * (n - 1 + 2 * cycles)

    while lo < hi:
        mid = (lo + hi) // 2
        if check(mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Mảng đầu vào lưu trữ điểm cuối, độ dài ban đầu và giá trị chữa lành của từng cạnh một cách riêng biệt. Việc giữ ID cạnh trong suốt quá trình phân hủy xương rồng rất hữu ích vì quá trình chuyển đổi chu kỳ cần cả độ dài ban đầu và giá trị chữa lành của nó. 

Thuật toán Tarjan lưu trữ các cạnh trên một ngăn xếp. Bất cứ khi nào`low[v] >= dfn[u]`, các cạnh lên đến cạnh cây (u\text{-}v) tạo thành một thành phần được kết nối hai chiều. Trong cây xương rồng, bộ phận đó là một cây cầu đơn hoặc một chu trình đơn giản, do đó không cần đến máy móc hai thành phần có mục đích chung. 

Cây cắt khối được root một lần trước khi tìm kiếm nhị phân. Cấu trúc của nó không bao giờ thay đổi nên việc xây dựng lại nó trong mỗi lần kiểm tra tính khả thi sẽ lãng phí thời gian. 

các`check`Hàm xử lý cây này theo thứ tự ngược lại. Đối với một đỉnh ban đầu, hai độ sâu con lớn nhất là đủ vì mọi đường đi qua đỉnh đó có thể sử dụng tối đa hai nhánh con. Một khối chu trình được xử lý với mười mảng có thể tái sử dụng tương ứng với số lượng tiền tố và hậu tố được mô tả ở trên. 

Trục xoay trùng lặp ở cuối`verts`là cố ý. Nó đại diện cho điểm kết thúc của chu kỳ và làm cho việc lập chỉ mục của hai bên trở nên thống nhất. DP không bao giờ đọc giá trị cây con cho trục trùng lặp đó, do đó không cần giá trị từ trục gốc. 

Tất cả các khoảng cách đều sử dụng số nguyên Python, do đó không có vấn đề tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, cần có số học 64 bit có dấu. 

Tìm kiếm nhị phân sử dụng điều kiện nghiêm ngặt`lo < hi`và một điểm giữa khả thi sẽ di chuyển ranh giới trên tới`mid`. Điều này trả về trực tiếp giá trị khả thi đầu tiên mà không cần biến trả lời riêng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác duy nhất. Có ba cạnh có thể loại bỏ. Cây kết quả luôn bao gồm hai cạnh ban đầu còn sót lại cộng với hai lá chữa lành. 

| Cắt cạnh | Lá lành ở điểm cuối đầu tiên | Lá lành ở điểm cuối thứ hai | Đường kính | 
| --- | --- | --- | --- | 
| (1\text{-}2) | (RV_1+RE_2=3) | (RV_2+RE_2=4) | 12 | 
| (2\text{-}3) | (RV_2+RE_1=3) | (RV_3+RE_1=4) | 10 | 
| (3\text{-}1) | (RV_3+RE_3=6) | (RV_1+RE_3=4) | 14 | 

Lựa chọn thứ hai là tối ưu. Sau khi loại bỏ cạnh (2\text{-}3), đường dẫn còn lại là (2\text{-}1\text{-}3), có độ dài (1+2=3). Hai lá chữa bệnh cộng (3) và (4), cho đường kính (3+3+4=10). 

DP nhìn thấy lựa chọn tương tự khi xử lý chu trình duy nhất. Đối với một ứng cử viên (R=10), vết cắt (2\text{-}3) có tối đa tất cả sáu loại đường kính (10) và độ sâu thu được của nó tính từ trục quay là nhiều nhất (10). Hai vết cắt còn lại vi phạm giới hạn. 

### Mẫu 2 

Có hai tam giác có chung đỉnh (1). Vì các nhánh cây được cắt khối ở đỉnh (1) nên đường kính cuối cùng có thể là tổng độ sâu lớn nhất mà mỗi tam giác đóng góp. 

Đối với chu kỳ đầu tiên, độ sâu tốt nhất tính từ đỉnh (1) cho mỗi lần cắt có thể là: 

| Cắt theo chu kỳ (1\text{-}2\text{-}3) | Độ sâu tối đa từ đỉnh 1 | 
| --- | --- | 
| (1\text{-}2) | 12 | 
| (2\text{-}3) | 10 | 
| (3\text{-}1) | 17 | 

Đối với chu kỳ thứ hai, các giá trị tương ứng là: 

| Cắt theo chu kỳ (1\text{-}4\text{-}5) | Độ sâu tối đa từ đỉnh 1 | 
| --- | --- | 
| (1\text{-}4) | 13 | 
| (4\text{-}5) | 12 | 
| (5\text{-}1) | 12 | 

Vì hai chu trình gặp nhau ở đỉnh (1), nên đường kính giữa các nhánh xa nhất của chúng là tổng của hai độ sâu. 

| Cắt chu kỳ đầu tiên | Cắt chu kỳ thứ hai | Kết quả đường kính xuyên chu kỳ | 
| --- | --- | --- | 
| (1\text{-}2) | (1\text{-}4) | 25 | 
| (1\text{-}2) | (4\text{-}5) | 24 | 
| (1\text{-}2) | (5\text{-}1) | 24 | 
| (2\text{-}3) | (1\text{-}4) | 23 | 
| (2\text{-}3) | (4\text{-}5) | 22 | 
| (2\text{-}3) | (5\text{-}1) | 22 | 
| (3\text{-}1) | (1\text{-}4) | 30 | 
| (3\text{-}1) | (4\text{-}5) | 29 | 
| (3\text{-}1) | (5\text{-}1) | 29 | 

Mức tối thiểu là (22), thu được bằng cách cắt các cạnh (2\text{-}3) và (4\text{-}5). Ví dụ này chứng minh tại sao DP phải duy trì độ sâu xa nhất có thể tối thiểu thay vì chỉ duy trì đường kính của một chu trình riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((N+M)\log V)) | Tarjan và cấu trúc cắt khối là tuyến tính. Mỗi lần kiểm tra tính khả thi sẽ quét mọi khối và cạnh chu kỳ với số lần không đổi và tìm kiếm nhị phân thực hiện kiểm tra (O(\log V)). | 
| Không gian | (O(N+M)) | Biểu đồ gốc, cây cắt khối, dữ liệu phân rã, mảng DP và mảng chu trình tạm thời đều có kích thước tuyến tính ở kích thước đầu vào. | 

Có nhiều nhất (M-N+1) chu trình và tổng số cạnh trên tất cả các chu trình nhiều nhất là (M). Do đó, việc kiểm tra tính khả thi hoàn chỉnh là tuyến tính. Với tối đa khoảng (2\cdot10^{14}) giá trị câu trả lời có thể có, tìm kiếm nhị phân cần ít hơn (50) lần lặp. Điều này phù hợp với các giới hạn đã nêu, trong khi Python được hưởng lợi đáng kể từ việc sử dụng lại mảng chu trình thay vì phân bổ mảng mới cho mỗi chu kỳ trong mỗi lần kiểm tra. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`. Nó chuyển hướng đầu vào và đầu ra tiêu chuẩn và gọi`solve`hoạt động trực tiếp.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solution.input = sys.stdin.readline
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run("""\
3 3
1 2 3
3 1 2 3
1 2 1 2
2 3 3 1
""") == "10", "sample 1"

# Sample 2
assert run("""\
5 6
1 2 3 4 5
1 2 6 1
1 3 5 4
2 3 4 2
1 4 3 6
1 5 2 3
4 5 1 5
""") == "22", "sample 2"

# Minimum-size cactus, all equal values.
# Any cut produces a path with two original edges and two healing edges.
assert run("""\
3 3
0 0 0
1 2 1 1
2 3 1 1
3 1 1 1
""") == "4", "minimum size and all equal values"

# A bridge attached to one cycle vertex.
# The optimal cut avoids making the long bridge branch even longer.
assert run("""\
4 4
0 0 0 0
1 2 1 1
2 3 1 1
3 1 1 1
3 4 10 1
""") == "12", "bridge attachment"

# Large values, testing 64-bit-sized distances.
assert run("""\
3 3
1000000000 1000000000 1000000000
1 2 1000000000 1000000000
2 3 1000000000 1000000000
3 1 1000000000 1000000000
""") == "6000000000", "large weights"

# Maximum-size instance: one cycle containing all 100000 vertices.
# All original edges and healing edges have length 1.
n = 100000
parts = [
    f"{n} {n}",
    " ".join(["0"] * n)
]
for i in range(1, n):
    parts.append(f"{i} {i + 1} 1 1")
parts.append(f"{n} 1 1 1")

max_case = "\n".join(parts) + "\n"
assert run(max_case) == "100001", "maximum-size single cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác tối thiểu có tất cả các giá trị bằng nhau | 4 | Lá chữa bệnh phải tham gia vào đường kính. | 
| Tam giác có cầu dài 10 | 12 | Các cây cầu vẫn còn nguyên vẹn và chiều dài của chúng phải được tính vào độ sâu của cây con. | 
| Tam giác có giá trị (10^9) | 6000000000 | Giới hạn số học và tìm kiếm nhị phân số nguyên lớn. | 
| Chu kỳ 100000 đỉnh | 100001 | Xử lý chu trình tuyến tính, lập chỉ mục ranh giới và kích thước đầu vào tối đa. | 

## Vỏ cạnh 

Tam giác tối thiểu có tất cả các giá trị bằng nhau được xử lý bởi chính quá trình chuyển đổi chu trình. Vì```
3 3
0 0 0
1 2 1 1
2 3 1 1
3 1 1 1
```chu trình có ba vị trí cắt có thể. Mỗi vết cắt tạo ra hai cạnh lành có chiều dài (1), trong khi hai cạnh ban đầu còn sót lại cũng có tổng chiều dài (2). Loại thứ sáu của chu kỳ DP, con đường giữa hai lá chữa bệnh, đạt đến (4), do đó việc kiểm tra tính khả thi thành công chính xác ở (R=4). 

Trường hợp mẫu-1 cho thấy sự khác biệt giữa chiều dài cạnh ban đầu và chi phí chữa lành. Việc cắt (2\text{-}3) sử dụng giá trị chữa lành (RE=1), tạo ra chiều dài của lá (2+1=3) và (3+1=4). Đường còn lại có chiều dài (1+2=3), cho đường kính (10). Thay vào đó, việc cắt cạnh ban đầu ngắn nhất (1\text{-}2) sử dụng (RE=2), tạo ra các lá và đường kính chữa lành lớn hơn (12). Chu trình DP xem xét các giá trị chữa lành trực tiếp thông qua (a=RV_u+RE_e) và (b=RV_v+RE_e). 

Đối với trường hợp cầu,```
4 4
0 0 0 0
1 2 1 1
2 3 1 1
3 1 1 1
3 4 10 1
```Tarjan tạo một khối chu kỳ cho các đỉnh (1,2,3) và một cạnh cây thông thường (3\text{-}4). Cầu đóng góp (10+d_4) khi đỉnh (3) được xử lý. Sau đó, quá trình chuyển đổi chu trình sẽ so sánh tất cả ba lần cắt có thể có trong khi vẫn mang theo độ sâu nhánh đã được tính toán đó. Kết quả tốt nhất là (12). 

Đối với trường hợp có giá trị lớn,```
3 3
1000000000 1000000000 1000000000
1 2 1000000000 1000000000
2 3 1000000000 1000000000
3 1 1000000000 1000000000
```mọi cạnh ban đầu còn sót lại đều có chiều dài (10^9) và mọi cạnh chữa lành đều có chiều dài (2\cdot10^9). Cây cuối cùng có đường đi chứa hai cạnh ban đầu và hai cạnh chữa lành nên đường kính của nó là (6\cdot10^9). Các số nguyên Python xử lý việc này một cách trực tiếp, trong khi việc triển khai C++ cần`long long`. 

Cuối cùng, một chu trình có thể chứa gần như tất cả (100.000) đỉnh. Các phép tính tiền tố và hậu tố là tuyến tính theo độ dài chu kỳ và mỗi cạnh chỉ được chạm vào một số lần không đổi cho mỗi lần kiểm tra tính khả thi. Thuật toán không bao giờ liệt kê các cặp đỉnh chu kỳ hoặc cấu hình cắt hoàn chỉnh, đây là thuộc tính giúp giữ trường hợp kích thước tối đa trong độ phức tạp cần thiết.
