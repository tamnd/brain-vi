---
title: "CF 102441G - Tổng khoảng cách ở cây xương rồng"
description: "Chúng tôi có biểu đồ xương rồng được kết nối với tối đa (10^5) đỉnh. Cây xương rồng rất thưa thớt và có cấu trúc đặc biệt hữu ích: mọi thành phần được kết nối hai chiều đều là một cây cầu hoặc một chu trình đơn giản."
date: "2026-08-08T13:28:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "G"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 139
verified: true
draft: false
---

[CF 102441G - Tổng khoảng cách trong xương rồng](https://codeforces.com/problemset/problem/102441/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có biểu đồ xương rồng được kết nối với tối đa (10^5) đỉnh. Cây xương rồng rất thưa thớt và có cấu trúc đặc biệt hữu ích: mọi thành phần được kết nối hai chiều đều là một cây cầu hoặc một chu trình đơn giản. Đối với mỗi cặp đỉnh không có thứ tự (u,v), chúng ta cần khoảng cách đường đi ngắn nhất của nó, được đo bằng số cạnh và chúng ta cần tổng các khoảng cách này trên tất cả các cặp. 

Đồ thị chứa (n) đỉnh và (m) cạnh, theo sau là điểm cuối của mỗi cạnh. Câu trả lời là một số nguyên chứa tổng của tất cả (\binom n2) cặp không có thứ tự. Các ví dụ chính thức có câu trả lời`3`cho tam giác và`42`cho cây xương rồng bảy đỉnh. 

Giới hạn (n\le 10^5) loại trừ mọi thứ bậc hai. Chẵn (O(n^2)) đã có nghĩa là khoảng (10^{10}) phép toán cặp. Biểu đồ thưa thớt vì (m\le 2n), do đó, giải pháp (O(n+m)) hoặc (O((n+m)\log n)) là mục tiêu tự nhiên. Giới hạn một giây làm cho cách tiếp cận tuyến tính trở nên đặc biệt hấp dẫn. Bản thân câu trả lời có thể lớn hơn nhiều so với số nguyên 32 bit. Một đường đi trên (10^5) đỉnh đã có tổng khoảng cách 

[ 
\frac{n(n-1)(n+1)}6, 
] 

tức là khoảng (1,67\cdot10^{14}), vì vậy số học 64-bit là cần thiết. Số nguyên Python tự động xử lý việc này. 

Một số trường hợp nhỏ bộc lộ những sai sót rất dễ mắc phải. Với một đỉnh không có cặp:```
1 0
```và câu trả lời là`0`. Việc triển khai giả định mọi khối chứa ít nhất một cạnh có thể thất bại ở đây. 

Một cái cây phải cư xử giống hệt như một cái cây bình thường. Ví dụ,```
3 2
1 2
2 3
```có khoảng cách cặp (1,1,2), vì vậy câu trả lời là`4`. Việc coi mọi thành phần được kết nối hai chiều như một chu trình sẽ phân loại không chính xác hai khối cầu. 

Một chu trình không thể được xử lý như một cái cây. Đối với một hình tam giác,```
3 3
1 2
2 3
3 1
```mỗi cặp cách nhau một khoảng cách, nên câu trả lời là`3`. Phép tính cây bao trùm sẽ cho (4), vì nó đếm cặp (1,3) ở khoảng cách hai. 

Các chu kỳ chẵn có một trường hợp biên khác. Trong một hình vuông,```
4 4
1 2
2 3
3 4
4 1
```hai cặp đối diện có khoảng cách hai, trong khi bốn cặp liền kề có khoảng cách một. Câu trả lời là (8). Việc triển khai thay thế mọi khoảng cách chu kỳ bằng một trong hai độ dài cung có hướng phải xử lý chính xác trường hợp có độ dài bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện tìm kiếm theo chiều rộng từ mọi đỉnh. Một BFS cung cấp khoảng cách từ nguồn của nó đến mọi đỉnh khác, do đó việc tính tổng tất cả các kết quả BFS là chính xác. Tuy nhiên, mỗi BFS quét dữ liệu đồ thị (O(n+m)). Lặp lại nó (n) lần chi phí (O(n(n+m))). Ở giới hạn tối đa, danh sách lân cận chứa (2m\le4\cdot10^5) mục nhập, do đó, chỉ cần quét các mục nhập kề từ tất cả (10^5) nguồn có thể yêu cầu khoảng (4\cdot10^{10}) lần quét. Điều đó vượt xa giới hạn một giây. 

Quan sát quan trọng là cây xương rồng không có các thành phần liên kết đôi tùy ý. Mỗi khối là một cây cầu hoặc một chu trình đơn giản. Nếu chúng ta nén mọi khối thành một nút và kết nối nó với các đỉnh ban đầu chứa trong đó, chúng ta sẽ thu được cây cắt khối. Cây này mô tả cách các khối khác nhau được kết nối, trong khi chu trình ban đầu bên trong một khối cung cấp khoảng cách chính xác giữa các đỉnh đính kèm của nó. 

Root cây cắt khối này. Đối với khối (B), giả sử đỉnh đính kèm cha của nó là (p). Mọi đỉnh (v) khác thuộc (B) đều có vùng con chứa (v) và tất cả các khối bên dưới nó. Đặt kích thước của nó là (s_v). Từ quan điểm của (B), tất cả các đỉnh trong vùng đó gắn vào (B) chính xác tại (v). 

Ngoài ra còn có một vùng ở phía cha của (B). Kích thước của nó là 

[ 
n-\sum_{v\ne p}s_v. 
] 

Do đó, mỗi đỉnh ban đầu thuộc về chính xác một trong các vùng này và tất cả các cặp có khối cao nhất là (B) có thể được tính bằng cách nhân kích thước vùng. 

Đối với một cây cầu, chỉ có hai đỉnh đính kèm và khoảng cách của chúng bên trong khối là một. Đóng góp của nó chỉ đơn giản là 

[ 
s_1s_2. 
] 

Đối với một chu trình có các đỉnh được sắp xếp theo chu kỳ là (v_0,v_1,\ldots,v_{k-1}), khoảng cách giữa (v_i) và (v_j) bên trong khối là 

[ 
\min(|i-j|,\ k-|i-j|). 
] 

Vấn đề còn lại là tính toán 

[ 
\sum_{i<j}s_i s_j\min(j-i,k-(j-i)) 
] 

trong thời gian tuyến tính cho chu kỳ. Đầu tiên tính tổng khoảng cách dòng thông thường 

[ 
T=\sum_{i<j}s_i s_j(j-i). 
] 

Khi đó mọi cặp có (j-i>k/2) đều ở quá xa trên đường thẳng. Khoảng cách chính xác của nó là (k-(j-i)), nên số tiền cần trừ là 

[ 
(j-i)-(k-(j-i))=2(j-i)-k. 
] 

Một ranh giới chuyển động xác định tất cả các cặp như vậy theo thời gian tuyến tính và tổng tiền tố của (s_i) và (i s_i) đưa ra tổng số hiệu chỉnh của chúng. 

Phương pháp vũ phu trả tiền cho từng cặp riêng biệt. Cấu trúc xương rồng cho phép chúng ta thay thế tất cả các cặp đi qua một khối bằng tổng có trọng số trên các đỉnh đính kèm của khối. Cây cắt khối cho chúng ta biết nên sử dụng trọng số nào và công thức chu trình xử lý toàn bộ chu trình mà không liệt kê các cặp đỉnh của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n(n+m))) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS liên kết thấp lặp lại và chia biểu đồ thành các thành phần được kết nối hai chiều. Vì đầu vào là một cây xương rồng nên mọi thành phần thu được đều là một cạnh cầu hoặc một chu trình đơn giản. Một DFS lặp được sử dụng để đường dẫn chứa (10^5) đỉnh không phụ thuộc vào ngăn xếp đệ quy của Python. 
2. Xây dựng cây cắt khối ngầm. Đối với mọi thành phần được kết nối hai chiều, hãy lưu trữ các đỉnh của nó và thêm thành phần đó vào danh sách tỷ lệ của từng đỉnh đó. Đặc tính xương rồng đảm bảo rằng cấu trúc tỷ lệ này là một cây khi các khối và đỉnh khớp nối được xem như các loại nút xen kẽ. 
3. Root cấu trúc này tại đỉnh`0`. Trong quá trình truyền tải, ghi lại khối cha của mỗi đỉnh và đỉnh cha của mỗi khối. Cũng giữ các khối theo thứ tự khám phá. Việc đảo ngược thứ tự đó sau này sẽ đưa ra thứ tự xử lý từ dưới lên. 
4. Khởi tạo kích thước mỗi cây con của đỉnh thành một, đại diện cho chính đỉnh đó. Xử lý các khối từ dưới lên trên. Đối với một khối (B) có đỉnh cha (p), mọi đỉnh phụ khác (v) có trọng số nhánh bằng với kích thước cây con đã được tính toán của nó`sub[v]`. 
5. Gọi (S) là tổng kích thước của các nhánh con đó. Nhánh phía cha có trọng số (n-S). Điều này bao gồm đỉnh cha và mọi thứ bên ngoài khối hiện tại. Do đó, trọng số của nhánh có tổng chính xác là (n). 
6. Nếu khối là một cây cầu, hãy nhân trọng số hai nhánh của nó. Con đường duy nhất giữa hai vùng phải đi qua cây cầu này một lần, vì vậy mỗi cặp có điểm cuối ở các vùng khác nhau đều đóng góp một đường. 
7. Nếu khối là một chu trình, hãy xây dựng lại thứ tự đỉnh tuần hoàn của nó bằng cách đi theo hai đỉnh lân cận của nó. Liên kết trọng số nhánh với mỗi đỉnh chu kỳ. Sự đóng góp của khối này là tổng trọng số của khoảng cách hình tròn giữa mỗi hai đỉnh đính kèm. 
8. Đầu tiên hãy tính phần đóng góp của chu trình dưới dạng một đường. Với mọi (j), duy trì tổng tiền tố 

[ 
P=\sum_{i<j}s_i 
] 

và 

[ 
Q=\sum_{i<j}i s_i. 
] 

Khi đó tất cả các cặp kết thúc tại (j) đều đóng góp 

[ 
s_j(jP-Q). 
] 

Điều này tạo ra tổng sử dụng khoảng cách (j-i). 

1. Sửa các cặp tốt hơn qua phía bên kia của chu kỳ. Đối với mỗi điểm cuối bên trái (i), tất cả các điểm cuối bên phải có liên quan đều đáp ứng 

[ 
j-i>\left\lfloor\frac{k}{2}\right\rfloor. 
] 

Một con trỏ đơn điệu tìm thấy (j) đầu tiên. Tổng tiền tố trên hậu tố sau đó đưa ra 

[ 
s_i\sum_j s_j(2(j-i)-k) 
] 

trong thời gian không đổi cho điều đó (i). 

1. Thêm phần đóng góp khối vào câu trả lời và thêm tổng kích thước nhánh con vào`sub[p]`. Điều này làm cho toàn bộ khu vực con cháu của khối hiện tại có sẵn khi khối cha của nó được xử lý. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý một khối, đỉnh cha của nó nhận được chính xác số đỉnh ban đầu nằm bên dưới khối đó trong cây cắt khối có gốc. Đối với một khối cụ thể, các vùng nhánh tới của nó rời rạc và cùng chứa mọi đỉnh ban đầu. Bất kỳ cặp nào có tuyến đường sử dụng khối này đều có điểm cuối ở hai vùng khác nhau và khoảng cách của nó bên trong khối chính xác là khoảng cách giữa các đỉnh đính kèm tương ứng. Một cặp như vậy được tính một lần ở khối cao nhất ngăn cách hai điểm cuối của nó. Các cặp bên trong một khu vực được cố tình trì hoãn ở các khối thấp hơn. Do đó, mỗi cặp không có thứ tự được tính chính xác một lần tại mỗi đoạn khối xuất hiện trên đường đi ngắn nhất của nó và tổng của tất cả các đóng góp khối chính xác là tổng của tất cả các khoảng cách đường đi ngắn nhất. 

Đối với một chu trình, vấn đề đặc biệt duy nhất là chọn cung nào ngắn hơn trong hai cung của nó. Việc tính toán đường thẳng cho một độ dài cung và phép hiệu chỉnh thay thế chính xác các cặp mà cung kia ngắn hơn. Khi độ dài chu kỳ chẵn và hai cung bằng nhau thì cặp cung đó có hiệu chính xác (k/2) nên không đưa vào điều kiện hiệu chỉnh nghiêm ngặt và giữ đúng khoảng cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cycle_cost(order, weights):
    k = len(order)

    pref_w = [0] * (k + 1)
    pref_iw = [0] * (k + 1)

    line_cost = 0
    for i, w in enumerate(weights):
        line_cost += w * (i * pref_w[i] - pref_iw[i])
        pref_w[i + 1] = pref_w[i] + w
        pref_iw[i + 1] = pref_iw[i] + i * w

    half = k // 2
    left = half + 1
    correction = 0

    total_w = pref_w[k]
    total_iw = pref_iw[k]

    for i, w in enumerate(weights):
        need = i + half + 1
        if left < need:
            left = need

        if left < k:
            suffix_w = total_w - pref_w[left]
            suffix_iw = total_iw - pref_iw[left]

            correction += w * (
                2 * suffix_iw - (2 * i + k) * suffix_w
            )

    return line_cost - correction

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    eu = [0] * m
    ev = [0] * m
    adj = [[] for _ in range(n)]

    for eid in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        eu[eid] = u
        ev[eid] = v
        adj[u].append((v, eid))
        adj[v].append((u, eid))

    # Iterative Tarjan DFS.
    tin = [0] * n
    low = [0] * n
    parent_edge = [-1] * n
    it = [0] * n

    edge_stack = []
    components = []

    timer = 1
    tin[0] = low[0] = timer

    dfs_stack = [0]

    while dfs_stack:
        u = dfs_stack[-1]

        if it[u] < len(adj[u]):
            v, eid = adj[u][it[u]]
            it[u] += 1

            if eid == parent_edge[u]:
                continue

            if tin[v] == 0:
                parent_edge[v] = eid
                timer += 1
                tin[v] = low[v] = timer

                edge_stack.append(eid)
                dfs_stack.append(v)
            elif tin[v] < tin[u]:
                if tin[v] < low[u]:
                    low[u] = tin[v]
                edge_stack.append(eid)

        else:
            dfs_stack.pop()

            pe = parent_edge[u]
            if pe == -1:
                continue

            p = eu[pe] if ev[pe] == u else ev[pe]

            if low[u] < low[p]:
                low[p] = low[u]

            if low[u] >= tin[p]:
                comp = []

                while True:
                    eid = edge_stack.pop()
                    comp.append(eid)
                    if eid == pe:
                        break

                components.append(comp)

    # Store vertices of every block and its incidence list.
    block_vertices = []
    incidence = [[] for _ in range(n)]

    for b, comp in enumerate(components):
        seen = {}
        verts = []

        for eid in comp:
            a = eu[eid]
            c = ev[eid]

            if a not in seen:
                seen[a] = True
                verts.append(a)

            if c not in seen:
                seen[c] = True
                verts.append(c)

        block_vertices.append(verts)

        for v in verts:
            incidence[v].append(b)

    B = len(block_vertices)

    # Root the block-cut tree at vertex 0.
    parent_block = [-1] * n
    parent_vertex = [-1] * B

    parent_block[0] = -2

    block_order = []
    vertex_order = [0]
    stack = [0]

    while stack:
        v = stack.pop()

        for b in incidence[v]:
            if b == parent_block[v]:
                continue

            if parent_vertex[b] != -1:
                continue

            parent_vertex[b] = v
            block_order.append(b)

            for x in block_vertices[b]:
                if x == v:
                    continue

                if parent_block[x] == -1:
                    parent_block[x] = b
                    vertex_order.append(x)
                    stack.append(x)

    sub = [1] * n
    answer = 0

    # Process blocks bottom-up.
    for b in reversed(block_order):
        verts = block_vertices[b]
        p = parent_vertex[b]

        child_sum = 0
        for v in verts:
            if v != p:
                child_sum += sub[v]

        parent_weight = n - child_sum

        if len(verts) == 2:
            a, c = verts

            if a == p:
                wa = parent_weight
                wc = sub[c]
            else:
                wa = sub[a]
                wc = parent_weight

            answer += wa * wc

        else:
            # A cactus block with at least three vertices is a cycle.
            local = {}

            for v in verts:
                local[v] = []

            for eid in components[b]:
                a = eu[eid]
                c = ev[eid]
                local[a].append(c)
                local[c].append(a)

            start = verts[0]
            order = []
            prev = -1
            cur = start

            for _ in range(len(verts)):
                order.append(cur)

                x, y = local[cur]
                nxt = x if x != prev else y

                prev, cur = cur, nxt

            weights = []
            for v in order:
                if v == p:
                    weights.append(parent_weight)
                else:
                    weights.append(sub[v])

            answer += cycle_cost(order, weights)

        sub[p] += child_sum

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc biểu đồ trong khi gán cho mỗi cạnh một ID. Thuật toán của Tarjan cần có ID cạnh vì một cạnh vô hướng xuất hiện hai lần trong danh sách kề và DFS phải phân biệt cạnh gốc thực tế với cạnh khác dẫn trở lại đỉnh đã được truy cập. 

DFS liên kết thấp duy trì`tin`Và`low`. Khi còn là một đứa trẻ`u`thỏa mãn`low[u] >= tin[p]`, tất cả các cạnh từ đỉnh của ngăn xếp cạnh đến cạnh cha tạo thành một thành phần được kết nối hai chiều. Trong một cây xương rồng, những thành phần này chính xác là những cầu nối và chu trình cần thiết cho phần còn lại của thuật toán. 

Việc xây dựng thành phần chỉ sử dụng từ điển tạm thời cho một thành phần. Tổng số mục được xử lý là tuyến tính vì mỗi cạnh thuộc về chính xác một thành phần. Biểu diễn liên tục là danh sách các đỉnh trong mỗi khối cộng với danh sách tỷ lệ kết nối các đỉnh ban đầu với các khối. 

Việc duyệt gốc không bao giờ xây dựng một cây cắt khối riêng biệt.`parent_block`Và`parent_vertex`chứa chính xác thông tin cần thiết để điều hướng nó. Xử lý`block_order`ngược lại đảm bảo rằng mọi`sub[v]`được sử dụng bởi một khối đã kết hợp tất cả các khối bên dưới`v`. 

Trường hợp cầu cố tình ngắn gọn. Hai vùng của nó có một cạnh giữa các đỉnh đính kèm của chúng, do đó tích chéo của chúng chính xác là số cặp có đường đi sử dụng cây cầu đó. 

Trường hợp chu trình xây dựng lại thứ tự tuần hoàn thực tế từ thực tế là mỗi đỉnh chu trình có chính xác hai đỉnh lân cận bên trong khối. Thứ tự không phụ thuộc vào hướng được chọn, vì khoảng cách hình tròn là đối xứng. 

các`cycle_cost`chức năng là nơi phần bậc hai biến mất.`line_cost`đếm từng cặp bằng cách sử dụng khoảng cách về phía trước của nó. Hiệu chỉnh hai con trỏ xem xét chính xác các cặp có khoảng cách chuyển tiếp vượt quá nửa chu kỳ. Bất đẳng thức rất nghiêm ngặt, điều này rất cần thiết cho các chu trình chẵn vì các đỉnh đối diện đã có khoảng cách chính xác (k/2). 

Số nguyên trong Python tránh bị tràn, nhưng kết quả vẫn lớn, vì vậy việc giữ tất cả các phép tính ở dạng số nguyên là cần thiết. Không có phép toán modulo vì bài toán yêu cầu số tiền chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác duy nhất. Có một khối chu kỳ và mỗi đỉnh có trọng số nhánh là một. 

| Vị trí chu kỳ | Đỉnh | Trọng lượng cành | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 2 | 1 | 
| 2 | 3 | 1 | 

Tổng khoảng cách dòng là (1+2+1=4). Cặp giữa vị trí 0 và hai được phân tách bằng ba cạnh xung quanh chu kỳ, do đó khoảng cách đường hai của nó phải được điều chỉnh thành một. Sự điều chỉnh là (2\cdot2-3=1). Sự đóng góp của chu trình là (4-1=3). 

Câu trả lời cuối cùng là`3`, phù hợp với mẫu chính thức. 

### Mẫu 2 

Đồ thị gồm hai hình tam giác được nối qua các đỉnh`1`Và`3`, với một đường đi`1-5-7`gắn liền với đỉnh`1`. Root ở đỉnh`1`đưa ra các kích thước chi nhánh có liên quan sau đây. 

| Chặn | Đỉnh cha mẹ | Cân cành con | Cân nặng bên cha mẹ | Đóng góp | 
| --- | --- | --- | --- | --- | 
| Tam giác`1-2-3`| 1 | 1, 3 | 3 | 15 | 
| Bờ rìa`1-5`| 1 | 2 | 5 | 10 | 
| Bờ rìa`5-7`| 5 | 1 | 6 | 6 | 
| Tam giác`3-4-6`| 3 | 1, 1 | 5 | 11 | 

Đối với tam giác đầu tiên, trọng số của ba nhánh là (3,1,3). Mỗi cặp đỉnh gắn của nó đều kề nhau trên tam giác, do đó phần đóng góp có trọng số của nó là 

[ 
3\cdot1+1\cdot3+3\cdot3=15. 
] 

Tam giác thứ hai có trọng số (5,1,1), cho 

[ 
5\cdot1+5\cdot1+1\cdot1=11. 
] 

Hai đóng góp của cầu là (2\cdot5=10) và (1\cdot6=6). Tổng của họ là 

[ 
15+10+6+11=42. 
] 

Kết quả là`42`, một lần nữa phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Tarjan, xây dựng khối, truyền tải cắt khối và mọi phép tính chu trình đều tuyến tính trong tổng kích thước biểu đồ | 
| Không gian | (O(n+m)) | Danh sách kề, thành phần được kết nối hai chiều, danh sách tỷ lệ và mảng phụ đều là tuyến tính | 

Đồ thị có nhiều nhất (2n) cạnh, vì vậy (n+m=O(n)). Tại (n=10^5), thuật toán chỉ xử lý một số lượng tuyến tính các đối tượng đồ thị thay vì xem xét riêng lẻ các cặp đỉnh không có thứ tự (5\cdot10^9). Giới hạn chính thức là 1 giây và 256 MB, do đó, việc tránh cả kiểu liệt kê cặp bậc hai và DFS đệ quy sâu đặc biệt hữu ích ở đây. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io
from contextlib import redirect_stdout

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin

    return out.getvalue().strip()

# Provided sample 1
assert run(
    """3 3
1 2
2 3
3 1
"""
) == "3", "sample 1"

# Provided sample 2
assert run(
    """7 8
2 1
3 1
5 1
3 2
4 3
5 7
6 3
4 6
"""
) == "42", "sample 2"

# Minimum-size graph
assert run(
    """1 0
"""
) == "0", "single vertex"

# A path of length two
assert run(
    """3 2
1 2
2 3
"""
) == "4", "tree distances"

# Five-cycle, all branch weights equal to one
assert run(
    """5 5
1 2
2 3
3 4
4 5
5 1
"""
) == "15", "odd cycle"

# Four-cycle, catches the even-cycle midpoint case
assert run(
    """4 4
1 2
2 3
3 4
4 1
"""
) == "8", "even cycle"

# Maximum-size tree, a path with 100000 vertices.
n = 100000
max_case = str(n) + " " + str(n - 1) + "\n"
max_case += "\n".join(f"{i} {i + 1}" for i in range(1, n)) + "\n"

assert run(max_case) == "166666666650000", "maximum-size path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0`| Biểu đồ kích thước tối thiểu và bộ cặp trống | 
|`3 2`, cạnh`1-2`,`2-3`|`4`| Xử lý cầu và khoảng cách cây thông thường | 
| Năm chu kỳ |`15`| Chu kỳ lẻ và trọng lượng nhánh bằng nhau | 
| Bốn chu kỳ |`8`| Chu trình chẵn và các đỉnh đối diện | 
| Đường đi có 100000 đỉnh |`166666666650000`| Kích thước tối đa, câu trả lời lớn và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đồ thị một đỉnh```
1 0
```không có thành phần kết nối đôi. DFS thăm đỉnh`0`, thứ tự khối vẫn trống và câu trả lời ban đầu là`0`. Không cần khối giả đặc biệt. 

Đối với con đường```
3 2
1 2
2 3
```cả hai thành phần được kết nối đều là cầu nối. Cầu dưới có kích thước nhánh (1) và (2), góp phần (2). Cầu trên có kích thước nhánh (1) và (2), đóng góp một nhánh khác (2). Tổng cộng là`4`. Điều này cho thấy tại sao những cây cầu phải khác biệt với chu kỳ. 

Đối với hình tam giác```
3 3
1 2
2 3
3 1
```chu kỳ đơn có trọng số (1,1,1). Tổng dòng thông thường là`4`và cung dài cho một cặp được hiệu chỉnh bằng`1`, sản xuất`3`. Đây chính xác là trường hợp tổng khoảng cách của cây bao trùm bị sai. 

Đối với hình vuông```
4 4
1 2
2 3
3 4
4 1
```bốn nhánh cân đều là một. Tổng khoảng cách dòng là`10`. Chỉ những cặp có chênh lệch vị trí ba mới cần hiệu chỉnh và mỗi hiệu chỉnh là (2\cdot3-4=2). Kết quả là`8`. Cặp tại điểm chênh lệch hai không được hiệu chỉnh, vì cả hai cung của nó đều có chiều dài bằng hai. Ranh giới nghiêm ngặt này giúp ngăn chặn từng lỗi xảy ra trong các chu kỳ chẵn. 

Đối với mẫu thứ hai, các đỉnh khớp thuộc về nhiều khối. đỉnh`3`, chẳng hạn, được chia sẻ bởi cả hai tam giác. Kích thước cây con của nó bao gồm các đỉnh`3`,`4`, Và`6`khi tam giác đầu tiên được xử lý. Trọng số phía cha của tam giác đầu tiên đó sẽ bao gồm phần còn lại của biểu đồ. Đây chính xác là lý do tại sao quan điểm cây cắt khối lại hữu ích: các đỉnh khớp nối được chia sẻ về mặt cấu trúc, nhưng mọi cặp điểm cuối vẫn được gán cho các vùng khối chính xác mà không cần tính hai lần.
