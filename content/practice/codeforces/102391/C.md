---
title: "CF 102391C - Vệ sinh"
description: "Hãy coi mỗi ô lưới là một đỉnh của đồ thị có hướng. Hai ô chia sẻ một cạnh là ứng cử viên cho một cạnh, nhưng một ô từ chối di chuyển theo hướng ghi trên đó."
date: "2026-08-12T02:01:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "C"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 644
verified: true
draft: false
---

[CF 102391C - Vệ sinh](https://codeforces.com/problemset/problem/102391/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 44 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi ô lưới là một đỉnh của đồ thị có hướng. Hai ô chia sẻ một cạnh là ứng cử viên cho một cạnh, nhưng một ô từ chối di chuyển theo hướng ghi trên đó. Do đó, mỗi ô có tối đa ba cạnh đi ra và đồ thị có cấu trúc cao mặc dù nó có thể chứa nhiều chu trình có hướng. 

Đối với một truy vấn, người chơi bắt đầu tại (các) ô và cuối cùng phải đứng ở một ô (t). Một ô phải được làm sạch chính xác khi tồn tại một số bước đi được chỉ dẫn từ (s) đến (t) ghé thăm ô đó. Tương tự, các ô được yêu cầu là các đỉnh có thể tới được từ (s) và bản thân chúng có thể tới tới (t). 

Lưới chứa tối đa (10^6) ô, trong khi có thể có (3\cdot10^5) truy vấn. Việc truyền tải biểu đồ riêng biệt cho mỗi truy vấn sẽ kiểm tra tối đa (10^6) ô cho mỗi truy vấn, đưa ra khoảng (3\cdot10^{11}) kiểm tra đỉnh trong trường hợp xấu nhất. Ngay cả một BFS được tối ưu hóa cực kỳ cao cũng sẽ vượt xa giới hạn hai giây. Quá trình tiền xử lý phải gần với tuyến tính trong kích thước lưới và mỗi truy vấn phải mất khoảng thời gian logarit. Các ràng buộc chính thức đưa ra giới hạn hai giây và bộ nhớ 1024 MB. 

Có một số trường hợp dễ xảy ra khi thực hiện bất cẩn sẽ đưa ra câu trả lời sai. Lưới (1\times1) là một trường hợp như vậy:```
1 1 1
U
1 1 1 1
```Câu trả lời là`1`, vì trình phát đã bắt đầu và kết thúc trên cùng một ô. Kiểm tra khả năng tiếp cận chỉ xem xét các bước di chuyển không trống có thể trả về 0 không chính xác. 

Bẫy thứ hai là một cặp ô chặn lẫn nhau:```
1 2 1
RL
1 1 1 2
```Ô bên trái từ chối di chuyển sang phải, trong khi ô bên phải từ chối di chuyển sang trái. Không có đường đi nên đáp án là`0`. Việc coi vùng lân cận như một kết nối vô hướng sẽ cho rằng mục tiêu có thể truy cập được một cách không chính xác. 

Hiện tượng ngược lại cũng có thể xảy ra. Coi như```
1 2 1
LR
1 1 1 2
```Ô bên trái có thể di chuyển sang phải và ô bên phải có thể di chuyển sang trái, vì vậy cả hai ô tạo thành một thành phần được kết nối chặt chẽ. Câu trả lời là`2`. Việc nén các thành phần được kết nối mạnh nhưng quên cung cấp cho một thành phần số ô ban đầu của nó sẽ tạo ra số đếm sai. 

Cuối cùng, mục tiêu có thể truy cập không có nghĩa là mọi ô có thể truy cập ngay từ đầu đều thuộc về câu trả lời. Tế bào phải nằm trên một số bước đi bắt đầu đến mục tiêu. Chỉ riêng BFS chuyển tiếp đã tính toán một tập hợp quá lớn. Sự khác biệt này là lý do giải pháp cần biểu diễn biểu đồ cụ thể hơn nhiều. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện tìm kiếm từ ô bắt đầu, giữ lại mọi ô có thể truy cập và xác định riêng biệt ô nào trong số đó cuối cùng có thể tiếp cận mục tiêu. Phần thứ hai có thể được thực hiện bằng cách duyệt qua biểu đồ đảo ngược từ mục tiêu. Giao điểm của chúng chính xác là tập hợp các ô xuất hiện trong ít nhất một bước đi (s)-tới-(t), vì vậy phương pháp này đúng. 

Vấn đề là công việc lặp đi lặp lại. Một truy vấn có thể yêu cầu (O(NM)) hoạt động và có các truy vấn (Q). Với (N,M\le1000) và (Q\le300000), đây là (O(QNM)), đạt khoảng (3\cdot10^{11}) lượt truy cập ô. Việc lưu trữ toàn bộ tập hợp có thể truy cập cho mọi truy vấn cũng là không thể. 

Quan sát quan trọng là biểu đồ lưới này không phải là biểu đồ có hướng tùy ý. Đầu tiên nén các thành phần được kết nối mạnh mẽ của nó. Bên trong một thành phần, mọi ô có thể tiếp cận mọi ô khác, do đó, với mục đích di chuyển giữa các thành phần, toàn bộ thành phần hoạt động như một đỉnh. Quan trọng hơn, nếu các thành phần này được xử lý theo thứ tự tôpô thì các ô đã được xử lý luôn tạo thành một tập hợp các hình chữ nhật riêng biệt. Đặc tính hình học này làm cho đồ thị có hướng khổng lồ có thể nén được. 

Giả sử thành phần liên thông mạnh tiếp theo là (C). Xét hình chữ nhật nhỏ nhất chứa (C). Các hình chữ nhật đã được xử lý trước đó nằm bên trong hình chữ nhật bao quanh đó có thể được hợp nhất thành (C) trong cấu trúc phụ trợ. Sau đó kiểm tra bốn cạnh của hình chữ nhật giới hạn. Các hình chữ nhật tiếp xúc trực tiếp với một cạnh được nhóm lại thông qua một đỉnh ảo. Thuộc tính định hướng quan trọng là, đối với một nhóm hình chữ nhật được đặt cạnh nhau, mọi ô trong nhóm đều có khả năng rời khỏi nhóm theo hướng vuông góc với cách sắp xếp cạnh nhau. Do đó, một kết nối ảo là đủ để biểu diễn tất cả các cạnh được định hướng ban đầu đó. 

Đồ thị phụ thu được là một cái cây. Mỗi thành phần liên kết mạnh ban đầu sẽ trở thành một đỉnh cây có trọng số, với trọng số bằng số ô của nó. Các đỉnh ảo có trọng số bằng 0. Các nút con của mỗi đỉnh cây được sắp xếp thành các chuỗi và mối quan hệ về khả năng tiếp cận ban đầu có thể được phục hồi từ các chuỗi đó. 

Khi cây này tồn tại, truy vấn sẽ trở thành truy vấn cây. Chúng tôi di chuyển thành phần bắt đầu lên trên cho đến khi nó có cùng độ sâu với thành phần đích. Nếu các đỉnh kết quả không có cùng đỉnh gốc thì không thể truy cập được mục tiêu. Mặt khác, hai đỉnh là anh em trong một chuỗi có thứ tự và một loạt các thành phần anh em góp phần tạo nên câu trả lời. Tổng tiền tố trên các phần tử con được sắp xếp làm cho phạm vi này được tính theo thời gian không đổi sau lần nhảy tổ tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(QNM)) | (O(NM)) | Quá chậm | 
| SCC + cây hình chữ nhật + nâng nhị phân | (O(NM\alpha(NM)+Q\log(NM))) | (O(NM\log(NM))) | Đã chấp nhận | 

Về cơ bản, cấu trúc này có ý tưởng cấu trúc giống như giải pháp chính thức, với cách triển khai bên dưới thay thế các lệnh gọi C++ DFS đệ quy bằng các phép duyệt Python lặp lại và sử dụng các mảng số nguyên được đóng gói để kiểm soát bộ nhớ. 

## Hướng dẫn thuật toán

1. Xây dựng biểu đồ lưới có hướng ẩn và tính toán tất cả các thành phần liên thông mạnh bằng thuật toán Tarjan. Một ô có một cạnh đi tới mọi ô lân cận hợp lệ ngoại trừ hướng được ghi trong ô đó. Chúng tôi không bao giờ lưu trữ rõ ràng tất cả các cạnh của đồ thị vì mọi cạnh đều có thể được tạo lại từ một ô trong thời gian không đổi. 
2. Xử lý các thành phần được kết nối mạnh theo thứ tự số thành phần đảo ngược. Tarjan chỉ định các thành phần theo thứ tự các gốc của chúng được hoàn thành, vì vậy thứ tự này tương ứng với quá trình xử lý tôpô cần thiết cho việc xây dựng. 
3. Đối với mọi thành phần (C), hãy lưu hình chữ nhật giới hạn của nó ([x_{\min},x_{\max}]\times[y_{\min},y_{\max}]) và số lượng ô ban đầu của nó. Trọng số của thành phần chính xác là con số đó, bởi vì mọi ô trong một thành phần được kết nối mạnh đều có thể sử dụng được bất cứ khi nào chính thành phần đó có thể sử dụng được. 
4. Duy trì cấu trúc liên kết được thiết lập rời rạc trên các thành phần và đỉnh ảo đã được xử lý. Khi xử lý (C), kiểm tra từng ô thuộc (C). Bất kỳ thành phần được xử lý nào gặp bên trong hình chữ nhật giới hạn của (C) đều được hợp nhất vào (C). Điều này thể hiện thực tế là các vùng đó có thể nhập (C) thông qua cấu trúc hình chữ nhật bên trong. 
5. Kiểm tra hàng ngay phía trên và ngay bên dưới hình chữ nhật bao quanh. Đối với mỗi bên hiện có, hãy quét các cột của nó từ trái sang phải. Các vùng được xử lý liên tiếp được nối thông qua một đỉnh ảo mới được tạo bất cứ khi nào có nhiều hơn một vùng xuất hiện. Thực hiện thao tác tương tự cho các cột ngay bên trái và bên phải của hình chữ nhật. 
6. Trong khi quét một bên, hãy kiểm tra xem mọi ô ranh giới có từ chối hướng hướng về phía (C) hay không. Nếu điều đó xảy ra, không có cạnh thực tế nào từ cạnh vào (C) có thể xảy ra, do đó không có kết nối không phải cây nào được ghi lại. Nếu không thì ghi lại một kết nối phụ giữa nhóm bên và (C). Đây là nơi thông tin định hướng của lưới ban đầu đi vào cấu trúc cây. 
7. Sau khi tất cả các thành phần đã được xử lý, mọi gốc DSU còn lại sẽ được gắn vào một gốc nhân tạo. Các mối quan hệ cha mẹ kết quả tạo thành một cây. Các đỉnh ảo có trọng số bằng 0, trong khi các đỉnh SCC ban đầu vẫn giữ nguyên kích thước thành phần của chúng. 
8. Xây dựng mảng con có thứ tự cho mỗi đỉnh cây. Các kết nối phụ trợ không phải cây cung cấp cho mỗi đứa trẻ một mối quan hệ dây chuyền với các anh chị em lân cận của nó. Việc tuân theo những mối quan hệ đó cho phép chúng ta chỉ định một vị trí cho mỗi đứa trẻ. Một cấp độ con bằng 0 bắt đầu chuỗi một đỉnh, trong khi cấp độ con một bắt đầu duyệt qua chuỗi của nó bằng cách sử dụng XOR của hai chuỗi lân cận. 
9. Đánh dấu mỗi em bằng một lá cờ chỉ đường để mô tả chuỗi của em tiếp tục sang phải hay sang trái. Với mỗi đỉnh cây, hãy tính tổng tiền tố của các trọng số con. Đồng thời tính toán`le[v]`Và`ri[v]`, điểm cuối của chuỗi chứa con (v). 
10. Tính toán`val[v]`, số lượng ô được biểu thị bằng phần của cây phụ từ gốc đến (v), bao gồm cả sự đóng góp của chuỗi anh chị em có liên quan. Các giá trị này cho phép truy vấn loại bỏ phần phía trên phần tổ tiên có độ sâu mục tiêu trong thời gian không đổi. 
11. Xây dựng bảng nâng nhị phân cho cây. Một truy vấn chỉ cần di chuyển thành phần bắt đầu lên trên cho đến khi độ sâu của nó bằng với độ sâu của thành phần đích, do đó, một bảng tổ tiên tiêu chuẩn là đủ. Không cần tính toán LCA chung. 
12. Đối với truy vấn ((s,t)), ánh xạ cả hai ô lưới tới SCC của chúng. Nếu điểm xuất phát nông hơn mục tiêu thì không thể đạt được mục tiêu. Nếu không, hãy bắt đầu nhảy lên độ sâu của mục tiêu. Nếu đỉnh kết quả và đích không có chung đỉnh cha thì không thể truy cập được đích. Nếu chúng là anh em ruột, hãy sử dụng điểm cuối chuỗi và tổng tiền tố của chúng để đếm chính xác các thành phần mà các ô của chúng có thể xuất hiện trên một bước đi (s)-to-(t). 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi xử lý bất kỳ tiền tố nào của trật tự tôpô SCC, vùng được xử lý có thể biểu diễn dưới dạng các hình chữ nhật rời rạc, có thể được nhóm thành các chuỗi cạnh nhau. Thuộc tính định hướng của các hình chữ nhật này đảm bảo rằng mọi ô trong một nhóm như vậy đều có khả năng đi theo hướng vuông góc như nhau. Do đó, việc thay thế tất cả các kết nối ranh giới bằng một kết nối ảo duy nhất không làm thay đổi khả năng tiếp cận. 

Sau khi tất cả SCC được xử lý, cấu trúc phụ trợ là một cây. Đường dẫn được định hướng từ SCC này đến SCC khác phải đi theo lộ trình cây tương ứng duy nhất, trong khi việc di chuyển giữa các anh chị em có thể thực hiện chính xác bên trong chuỗi được ghi lại. Quy trình truy vấn trước tiên sẽ loại bỏ phần cây phía trên độ sâu của mục tiêu, sau đó kiểm tra xem hai cây anh em kết quả có nằm trên cùng một chuỗi có thể truy cập hay không. Công thức tổng tiền tố đếm chính xác các SCC có trọng số trên chuỗi đó và`val`chiếm phần đã được cố định ở trên nó. Do đó, mỗi ô được đếm thuộc về một số bước đi bắt đầu đến mục tiêu hợp lệ và mọi ô thuộc về một bước đi như vậy đều được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array

def solve():
    read = sys.stdin.readline
    n, m, q = map(int, read().split())
    cells = n * m
    MAX = 2 * cells + 5

    # Directions:
    # 0 = L, 1 = R, 2 = U, 3 = D
    direction = bytearray(cells)

    for i in range(n):
        s = read().strip()
        base = i * m
        for j, ch in enumerate(s):
            if ch == 'L':
                direction[base + j] = 0
            elif ch == 'R':
                direction[base + j] = 1
            elif ch == 'U':
                direction[base + j] = 2
            else:
                direction[base + j] = 3

    dx = (-0, 0, -1, 1)
    dy = (-1, 1, 0, 0)

    def iarr(length, value=0):
        return array('i', [value]) * length

    # ------------------------------------------------------------
    # Iterative Tarjan SCC
    # ------------------------------------------------------------

    dfn = iarr(cells, -1)
    low = iarr(cells, -1)
    bel = iarr(cells, -1)
    nxt_cell = iarr(cells, -1)

    # SCC member linked lists.
    member_head = iarr(MAX, -1)
    sz = iarr(MAX, 0)
    xmi = iarr(MAX, n)
    xma = iarr(MAX, -1)
    ymi = iarr(MAX, m)
    yma = iarr(MAX, -1)

    tarjan_stack = []
    dfs_stack = []
    it = bytearray(cells)

    timer = 0
    cnt = 0

    for root in range(cells):
        if dfn[root] != -1:
            continue

        dfn[root] = timer
        low[root] = timer
        timer += 1
        tarjan_stack.append(root)
        dfs_stack.append(root)

        while dfs_stack:
            u = dfs_stack[-1]
            k = it[u]

            while k < 4:
                it[u] = k + 1

                if k == direction[u]:
                    k += 1
                    continue

                ux = u // m
                uy = u - ux * m
                vx = ux + dx[k]
                vy = uy + dy[k]

                if vx < 0 or vx >= n or vy < 0 or vy >= m:
                    k += 1
                    continue

                v = vx * m + vy

                if dfn[v] == -1:
                    dfn[v] = timer
                    low[v] = timer
                    timer += 1
                    tarjan_stack.append(v)
                    dfs_stack.append(v)
                    break

                if bel[v] == -1 and dfn[v] < low[u]:
                    low[u] = dfn[v]

                k = it[u]

            else:
                dfs_stack.pop()

                if dfs_stack:
                    p = dfs_stack[-1]
                    if low[u] < low[p]:
                        low[p] = low[u]

                if low[u] == dfn[u]:
                    while True:
                        v = tarjan_stack.pop()
                        bel[v] = cnt

                        x = v // m
                        y = v - x * m

                        nxt_cell[v] = member_head[cnt]
                        member_head[cnt] = v
                        sz[cnt] += 1

                        if x < xmi[cnt]:
                            xmi[cnt] = x
                        if x > xma[cnt]:
                            xma[cnt] = x
                        if y < ymi[cnt]:
                            ymi[cnt] = y
                        if y > yma[cnt]:
                            yma[cnt] = y

                        if v == u:
                            break

                    cnt += 1

    # ------------------------------------------------------------
    # Auxiliary tree construction
    # ------------------------------------------------------------

    parent = iarr(MAX, -1)
    dsu = array('i', range(MAX))

    deg = iarr(MAX, 0)
    chain_xor = iarr(MAX, 0)

    # Non-tree edges are kept as packed integer arrays.
    edge_a = array('i')
    edge_b = array('i')

    def find(x):
        while dsu[x] != x:
            dsu[x] = dsu[dsu[x]]
            x = dsu[x]
        return x

    for c in range(cnt - 1, -1, -1):
        # First merge processed components inside the bounding rectangle.
        u = member_head[c]
        while u != -1:
            ux = u // m
            uy = u - ux * m

            for k in range(4):
                vx = ux + dx[k]
                vy = uy + dy[k]

                if vx < xmi[c] or vx > xma[c] or vy < ymi[c] or vy > yma[c]:
                    continue

                v = vx * m + vy
                r = find(bel[v])

                if r != c:
                    parent[r] = c
                    dsu[r] = c

            u = nxt_cell[u]

        # Scan horizontal sides.
        for x in (xmi[c] - 1, xma[c] + 1):
            if x < 0 or x >= n:
                continue

            first_bel = bel[x * m + ymi[c]]
            if first_bel < c:
                continue

            all_blocked = True
            group = find(first_bel)
            first = True

            for y in range(ymi[c], yma[c] + 1):
                vcell = x * m + y

                forbidden = 3 if x < xmi[c] else 2
                if direction[vcell] != forbidden:
                    all_blocked = False

                r = find(bel[vcell])

                if r != group:
                    if first:
                        parent[group] = cnt
                        dsu[group] = cnt
                        group = cnt
                        cnt += 1
                        first = False

                    parent[r] = group
                    dsu[r] = group

            if not all_blocked:
                edge_a.append(group)
                edge_b.append(c)
                deg[group] += 1
                chain_xor[group] ^= c
                deg[c] += 1
                chain_xor[c] ^= group

        # Scan vertical sides.
        for y in (ymi[c] - 1, yma[c] + 1):
            if y < 0 or y >= m:
                continue

            first_bel = bel[xmi[c] * m + y]
            if first_bel < c:
                continue

            all_blocked = True
            group = find(first_bel)
            first = True

            for x in range(xmi[c], xma[c] + 1):
                vcell = x * m + y

                forbidden = 1 if y < ymi[c] else 0
                if direction[vcell] != forbidden:
                    all_blocked = False

                r = find(bel[vcell])

                if r != group:
                    if first:
                        parent[group] = cnt
                        dsu[group] = cnt
                        group = cnt
                        cnt += 1
                        first = False

                    parent[r] = group
                    dsu[r] = group

            if not all_blocked:
                edge_a.append(group)
                edge_b.append(c)
                deg[group] += 1
                chain_xor[group] ^= c
                deg[c] += 1
                chain_xor[c] ^= group

    # Add one root above all remaining DSU roots.
    root = cnt

    for i in range(cnt):
        if dsu[i] == i:
            parent[i] = root
            dsu[i] = root

    cnt += 1
    parent[root] = root

    nodes = cnt

    # ------------------------------------------------------------
    # Store children in contiguous ranges.
    # ------------------------------------------------------------

    child_count = iarr(nodes, 0)

    for i in range(nodes - 1):
        child_count[parent[i]] += 1

    start = iarr(nodes, 0)
    total = 0
    for u in range(nodes):
        start[u] = total
        total += child_count[u]

    ordered = iarr(nodes - 1, 0)
    used = iarr(nodes, 0)

    for i in range(nodes - 1):
        p = parent[i]
        idx = start[p] + used[p]
        ordered[idx] = i
        used[p] += 1

    # ------------------------------------------------------------
    # Depth and binary lifting.
    # ------------------------------------------------------------

    depth = iarr(nodes, 0)
    p0 = iarr(nodes, 0)
    p0[root] = root

    stack = [root]

    while stack:
        u = stack.pop()
        begin = start[u]
        end = begin + child_count[u]

        for idx in range(begin, end):
            v = ordered[idx]
            depth[v] = depth[u] + 1
            p0[v] = u
            stack.append(v)

    LOG = max(1, (nodes - 1).bit_length())
    up = [p0]

    for _ in range(1, LOG):
        prev = up[-1]
        cur = iarr(nodes, 0)
        for i in range(nodes):
            cur[i] = prev[prev[i]]
        up.append(cur)

    # ------------------------------------------------------------
    # Order children by chain structure.
    # ------------------------------------------------------------

    pos = iarr(nodes, -1)
    cp = iarr(nodes, 0)

    for i in range(nodes - 1):
        if pos[i] != -1:
            continue

        if deg[i] == 0:
            p = parent[i]
            pos[i] = cp[p]
            cp[p] += 1

        elif deg[i] == 1:
            u = i
            previous = 0
            p = parent[u]

            while True:
                pos[u] = cp[p]
                cp[p] += 1

                nxt = previous ^ chain_xor[u]
                previous, u = u, nxt

                if deg[u] != 2:
                    pos[u] = cp[p]
                    cp[p] += 1
                    break

    # Rebuild children according to their final positions.
    for i in range(nodes - 1):
        p = parent[i]
        ordered[start[p] + pos[i]] = i

    # Direction of the auxiliary chain edges.
    chain_dir = iarr(nodes, 0)

    for i in range(len(edge_a)):
        a = edge_a[i]
        b = edge_b[i]

        if pos[a] < pos[b]:
            chain_dir[a] = 1
        else:
            chain_dir[b] = -1

    # ------------------------------------------------------------
    # Prefix sums and val/le/ri.
    # ------------------------------------------------------------

    prefix = iarr(nodes, 0)
    le = iarr(nodes, 0)
    ri = iarr(nodes, 0)
    val = iarr(nodes, 0)

    # Process parents before children.
    stack = [root]

    while stack:
        u = stack.pop()
        begin = start[u]
        end = begin + child_count[u]

        if begin == end:
            continue

        running = 0
        for idx in range(begin, end):
            v = ordered[idx]
            running += sz[v]
            prefix[v] = running

        for idx in range(begin, end):
            v = ordered[idx]

            if idx == begin or chain_dir[ordered[idx - 1]] != -1:
                le[v] = v
            else:
                le[v] = le[ordered[idx - 1]]

        for idx in range(end - 1, begin - 1, -1):
            v = ordered[idx]

            if chain_dir[v] != 1:
                ri[v] = v
            else:
                ri[v] = ri[ordered[idx + 1]]

        for idx in range(begin, end):
            v = ordered[idx]
            val[v] = prefix[ri[v]] - prefix[le[v]] + sz[le[v]]
            val[v] += val[u]

        for idx in range(begin, end):
            stack.append(ordered[idx])

    def query(a, b):
        if depth[a] < depth[b]:
            return 0

        ret = val[a]

        diff = depth[a] - depth[b]

        bit = 0
        while diff:
            if diff & 1:
                a = up[bit][a]
            diff >>= 1
            bit += 1

        ret -= val[a]

        if parent[a] != parent[b]:
            return 0

        if pos[a] < pos[b]:
            if pos[ri[a]] >= pos[b]:
                return prefix[b] - prefix[a] + ret + sz[a]
            return 0

        if pos[le[a]] <= pos[b]:
            return prefix[a] - prefix[b] + ret + sz[b]

        return 0

    # ------------------------------------------------------------
    # Queries.
    # ------------------------------------------------------------

    out = []

    for _ in range(q):
        x1, y1, x2, y2 = map(int, read().split())
        x1 -= 1
        y1 -= 1
        x2 -= 1
        y2 -= 1

        a = bel[x1 * m + y1]
        b = bel[x2 * m + y2]

        out.append(str(query(a, b)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn SCC sử dụng ngăn xếp DFS rõ ràng vì giới hạn đệ quy của Python không thể xử lý một cách an toàn đường dẫn chứa tối đa (10^6) ô. Các mảng từ`array('i')`cũng là cố ý. Một danh sách Python bình thường gồm hàng triệu số nguyên Python sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể so với các mảng số nguyên C++ tương đương. 

Mã hóa hướng phải khớp chính xác với thứ tự lân cận.`0,1,2,3`nghĩa là trái, phải, lên và xuống tương ứng, do đó hướng cấm được bỏ qua trước khi kiểm tra ba hàng xóm có thể sử dụng được. 

Danh sách thành viên thành phần được đại diện bởi`member_head`Và`nxt_cell`. Điều này thay thế C++`vector<int> vr[N]`cấu trúc và tránh tạo tối đa (10^6) đối tượng danh sách Python riêng biệt. 

Trong quá trình xử lý hình chữ nhật, DSU lưu trữ đại diện hiện tại của mọi vùng đã được xử lý. điều kiện`bel[v] < c`trên một cạnh hình chữ nhật cũng là cố ý. Chỉ những thành phần đã xuất hiện theo thứ tự cấu trúc liên kết bắt buộc mới được phép tham gia vào quá trình quét đó. 

Rễ nhân tạo có chính nó như là gốc nâng của nó. Điều này tránh các chỉ số tiêu cực trong truy cập mảng của Python trong khi vẫn giữ được ý nghĩa logic của gốc C++ gốc, không có gốc gốc. 

Mảng con có thứ tự được xây dựng lại sau`pos`đã được giao. Điều này là cần thiết vì các công thức tổng tiền tố phụ thuộc vào thứ tự cuối cùng của chuỗi chứ không phụ thuộc vào thứ tự các nút được tạo ra. 

Truy vấn đầu tiên chỉ thay đổi đỉnh bắt đầu. Vì độ sâu cây của mục tiêu được bảo toàn nên tổ tiên đạt được bằng bước nhảy này là ứng cử viên duy nhất có thể tham gia vào cùng chuỗi anh chị em với mục tiêu. Nếu cha mẹ khác nhau, sẽ không có con đường định hướng nào đến mục tiêu và câu trả lời ngay lập tức là con số 0. 

Tất cả các giá trị câu trả lời nhiều nhất là (NM\le10^6), vì vậy số nguyên có dấu 32-bit là đủ cho số lượng biểu đồ được lưu trữ. Các số nguyên của Python được sử dụng tự động cho số học trung gian, do đó không có vấn đề tràn trong các phép tính cuối cùng. 

## Ví dụ đã hoạt động 

Chỉ có một mẫu chính thức được cung cấp nên dấu vết thứ hai sử dụng một lưới được xây dựng nhỏ. 

### Mẫu 1 

Lưới là```
DDDDD
RDDDL
RRDLL
RUUUL
UUUUU
```Năm câu hỏi đều có câu trả lời`0, 14, 20, 14, 5`. 

Dấu vết sau đây tóm tắt giai đoạn truy vấn sau quá trình tiền xử lý SCC và cây phụ trợ. 

| Truy vấn | Bắt đầu SCC | SCC mục tiêu | Mối quan hệ sâu sắc | Cùng cha mẹ sau khi nhảy | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`(1,1) -> (5,5)`| vùng nguồn | vùng mục tiêu | nhảy sâu hợp lệ | không |`0`| 
|`(2,2) -> (5,5)`| SCC A | SCC B | bắt đầu di chuyển lên trên | vâng |`14`| 
|`(3,3) -> (5,5)`| SCC C | SCC B | bắt đầu di chuyển lên trên | vâng |`20`| 
|`(4,4) -> (5,5)`| SCC D | SCC B | bắt đầu di chuyển lên trên | vâng |`14`| 
|`(5,5) -> (5,5)`| SCC B | SCC B | nhảy không | vâng |`5`| 

Truy vấn đầu tiên chứng minh rằng việc xây dựng không nhầm lẫn giữa khoảng cách hình học với khả năng tiếp cận. Truy vấn cuối cùng thể hiện tính bất biến trọng số SCC: khi cả hai điểm cuối đều nằm trong cùng một SCC, câu trả lời là kích thước của SCC đó, tức là`5`đây. 

### Ví dụ được xây dựng 

Hãy xem xét```
2 3 2
UUU
UUU
1 1 2 3
2 3 1 1
```Đối với một`U`ô, di chuyển lên trên bị cấm, trong khi di chuyển xuống dưới và theo chiều ngang được phép khi hàng xóm tương ứng tồn tại. Từ`(1,1)`ĐẾN`(2,3)`, mọi ô đều có thể xuất hiện trên một đường dẫn hợp lệ. 

| Truy vấn | Bắt đầu | Mục tiêu | Có thể truy cập được? | Các ô trên một số đường dẫn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`(1,1)`|`(2,3)`| vâng | cả 6 ô |`6`| 
| 2 |`(2,3)`|`(1,1)`| không | không |`0`| 

Truy vấn thứ hai thực hiện tính định hướng. Mục tiêu nằm phía trên điểm bắt đầu, nhưng không có bước di chuyển nào có thể đi lên, do đó việc kiểm tra độ sâu của cây phụ trợ cuối cùng sẽ từ chối truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NM\alpha(NM)+Q\log(NM))) | Tính toán SCC, xây dựng hình chữ nhật DSU và tiền xử lý cây gần như tuyến tính; mỗi truy vấn thực hiện một lần nhảy tổ tiên nâng nhị phân | 
| Không gian | (O(NM\log(NM))) | Chi phí phụ thêm chính là bàn nâng nhị phân; tất cả các mảng lưới và cây đều tuyến tính | 

Với (NM\le10^6), quá trình tiền xử lý chỉ chạm vào lưới một số lần không đổi ngoài các hoạt động DSU. Mỗi truy vấn (3\cdot10^5) chỉ cần (O(\log(NM))) hoạt động. Đây là độ phức tạp mà các ràng buộc yêu cầu và nó phù hợp với cấu trúc của phương pháp C++ được chấp nhận, có độ phức tạp được nêu là (O(NM\alpha(NM)+Q\log(NM))). 

Việc triển khai Python sử dụng các mảng số nguyên được đóng gói và các phép duyệt lặp lại vì chỉ riêng độ phức tạp tiệm cận là không đủ ở một triệu đỉnh. Một bản dịch Python đơn giản sử dụng các danh sách lồng nhau và DFS đệ quy sẽ tiêu tốn nhiều bộ nhớ hơn và cũng sẽ thất bại trên các cây DFS sâu. 

## Trường hợp thử nghiệm```python
# This test block assumes the solve() function from the solution above
# is available in the same file.

import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin

    return out.getvalue()

# Official sample.
sample1 = """\
5 5 5
DDDDD
RDDDL
RRDLL
RUUUL
UUUUU
1 1 5 5
2 2 5 5
3 3 5 5
4 4 5 5
5 5 5 5
"""

assert run(sample1) == "0\n14\n20\n14\n5", "sample 1"

# Minimum-size grid. The only cell is both the start and target.
case_min = """\
1 1 1
U
1 1 1 1
"""

assert run(case_min) == "1", "minimum-size grid"

# Two cells block each other.
case_blocked = """\
1 2 2
RL
1 1 1 2
1 2 1 2
"""

assert run(case_blocked) == "0\n1", "mutually blocked boundary cells"

# Two cells form one strongly connected component.
case_scc = """\
1 2 1
LR
1 1 1 2
"""

assert run(case_scc) == "2", "same SCC must count both cells"

# All equal directions. Every cell lies on some path from the
# upper-left corner to the lower-right corner.
case_all_equal = """\
2 2 1
UU
UU
1 1 2 2
"""

assert run(case_all_equal) == "4", "all-equal directions"

# Boundary and directionality.
case_direction = """\
2 3 2
UUU
UUU
1 1 2 3
2 3 1 1
"""

assert run(case_direction) == "6\n0", "boundary directionality"

# Maximum-size grid. All cells are reachable from the upper-left
# corner to the lower-right corner because downward and horizontal
# moves are available from every U cell.
n = 1000
m = 1000
grid = "\n".join(["U" * m for _ in range(n)])
case_max = f"""\
{n} {m} 1
{grid}
1 1 1000 1000
"""

assert run(case_max) == "1000000", "maximum-size grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / U / 1 1 1 1`|`1`| Lưới tối thiểu và bước đi có độ dài bằng không | 
|`1 2 2 / RL / ...`|`0`,`1`| Các ô bị chặn lẫn nhau và xử lý ranh giới | 
|`1 2 1 / LR / 1 1 1 2`|`2`| Trọng lượng thành phần được kết nối mạnh mẽ | 
|`2 2 1 / UU / 1 1 2 2`|`4`| Các hướng hoàn toàn bằng nhau và phạm vi bao phủ toàn bộ đường đi | 
|`2 3 2 / UUU / UUU / ...`|`6`,`0`| Tính định hướng và khả năng tiếp cận ranh giới | 
|`1000 1000 1 / all U`|`1000000`| Kích thước lưới tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Trường hợp (1\times1) được xử lý trước khi có bất kỳ chuyển động có ý nghĩa nào được yêu cầu. Vì```
1 1 1
U
1 1 1 1
```Tarjan sản xuất một SCC với kích thước`1`. Cả hai điểm cuối truy vấn đều ánh xạ tới SCC đó, do đó chênh lệch độ sâu bằng 0 và phép tính chuỗi trả về trọng số của nó,`1`. 

Đối với những người hàng xóm bị chặn lẫn nhau,```
1 2 1
RL
1 1 1 2
```ô đầu tiên không thể di chuyển được vì hàng xóm duy nhất của nó ở bên phải, điều này bị cấm. Do đó, hai ô trở thành các SCC riêng biệt không có chuỗi khả năng tiếp cận phụ trợ kết nối chúng theo hướng yêu cầu. Truy vấn không thành công trong thử nghiệm gốc hoặc chuỗi và trả về`0`. 

Đối với một cặp kết nối mạnh,```
1 2 1
LR
1 1 1 2
```ô bên trái có thể di chuyển sang phải và ô bên phải có thể di chuyển sang trái. Tarjan đặt cả hai ô vào một SCC, có trọng lượng là`2`. Vì thành phần bắt đầu và thành phần đích giống hệt nhau nên truy vấn trả về trọng số thành phần hoàn chỉnh thay vì chỉ đếm ô đích. 

Đối với lưới hoàn toàn bằng nhau (2\times2)```
2 2 1
UU
UU
1 1 2 2
```người chơi có thể di chuyển từ ô phía trên bên trái xuống hoặc sang phải và từ các ô kết quả tiếp tục di chuyển về ô phía dưới bên phải. Bốn ô đều nằm trên một đường dẫn hợp lệ nào đó, vì vậy câu trả lời là`4`. Điều này nắm bắt các giải pháp chỉ tính toán một đường đi ngắn nhất cụ thể thay vì kết hợp tất cả các đường đi có thể. 

Đối với ví dụ định hướng lớn hơn```
2 3 2
UUU
UUU
1 1 2 3
2 3 1 1
```truy vấn đầu tiên có thể truy cập mọi ô. Một đường dẫn có thể di chuyển xuống sớm hoặc muộn và có thể di chuyển theo chiều ngang ở một trong hai hàng, do đó, mỗi ô trong số sáu ô nằm trên ít nhất một đường dẫn bắt đầu đến mục tiêu. Truy vấn ngược lại không thể di chuyển lên trên, vì vậy câu trả lời của nó là`0`. Cây phụ trợ duy trì sự bất đối xứng này mặc dù lưới bên dưới trông hoàn toàn đồng nhất. 

Thử nghiệm kích thước tối đa sử dụng (10^6) ô. Quá trình xử lý trước vẫn xử lý mỗi ô với số lần không đổi, trong khi truy vấn đơn lẻ chỉ sử dụng cây được tính toán trước. Câu trả lời mong đợi chính xác là (10^6), chứng tỏ rằng việc triển khai xử lý cả lưới lớn nhất và câu trả lời lớn nhất có thể mà không cần dựa vào kích thước nhỏ. 

Bài xã luận ở trên bám sát giải pháp cấu trúc được chấp nhận, trong khi phiên bản Python thay thế vectơ đệ quy và vectơ C++ bằng các phép duyệt lặp và mảng đóng gói.
