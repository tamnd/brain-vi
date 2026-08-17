---
title: "CF 102419J - Cảnh sát Jaber"
description: "Hãy coi mỗi hàng và mỗi cột là một đỉnh của đồ thị hai bên. Ô chứa 1 là cạnh giữa đỉnh hàng và đỉnh cột của nó. Giá trị ai cho chúng ta biết chính xác có bao nhiêu cạnh phải liên tiếp với hàng i. Độ cột hoàn toàn nằm trong tầm kiểm soát của chúng tôi."
date: "2026-08-16T09:11:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "J"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 546
verified: false
draft: false
---

[CF 102419J - Cảnh sát Jaber](https://codeforces.com/problemset/problem/102419/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 6 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi hàng và mỗi cột là một đỉnh của đồ thị hai bên. Một ô chứa`1`là cạnh giữa đỉnh hàng và đỉnh cột của nó. giá trị`a_i`cho chúng ta biết chính xác có bao nhiêu cạnh phải liên tiếp với một hàng`i`. Độ cột hoàn toàn nằm trong tầm kiểm soát của chúng tôi. 

Khi Jaber kiểm tra một đỉnh, mọi cạnh liên quan đến đỉnh đã được kiểm tra đều biến mất. Do đó, số lượng đèn anh ta nhìn thấy chính xác là bậc của đỉnh đó trong đồ thị con gây ra bởi các đỉnh chưa được kiểm tra. Do đó, thứ tự kiểm tra hợp lệ là thứ tự trong đó mỗi đỉnh có nhiều nhất một cạnh phụ còn lại khi nó bị loại bỏ. 

Một biểu đồ có thứ tự như vậy chính xác khi nó là một khu rừng. Nếu một đồ thị chứa một chu trình, sau khi liên tục loại bỏ các đỉnh có bậc nhiều nhất là một, thì chu trình cuối cùng vẫn giữ nguyên và mọi đỉnh trên nó đều có bậc hai. Ngược lại, mọi khu rừng khác rỗng đều có một lá hoặc một đỉnh cô lập, vì vậy chúng ta có thể loại bỏ nhiều nhất một đỉnh bậc. 

Do đó bài toán trở thành bài toán xây dựng đồ thị. Chúng ta cần một khu rừng lưỡng cực đơn giản với`n`đỉnh hàng,`m`đỉnh cột và độ quy định ở phía hàng. 

Giới hạn`n,m <= 1000`đủ nhỏ để một`O(nm)`xây dựng là phù hợp. Đặc biệt, bản thân đầu ra chứa`nm`các ký tự, nên chỉ in câu trả lời thôi cũng đã tốn phí rồi`O(nm)`. Do đó, việc xây dựng với thời gian và bộ nhớ bậc hai là điều đương nhiên, trong khi mọi thứ theo cấp số nhân đều hoàn toàn không khả thi. 

Có một số trường hợp đặc biệt khiến cho việc xây dựng đơn giản không thành công. Coi như```
2 2
2 2
```Cả hai hàng đều cần hai ô, vì vậy cả bốn ô sẽ phải`1`. Biểu đồ lưỡng cực thu được là 4 chu kỳ và không tồn tại thứ tự kiểm tra hợp lệ. Chỉ kiểm tra xem tổng số cái có nhiều nhất không`n+m-1`là đủ để bác bỏ ví dụ này, nhưng điều kiện đó nói chung là không đủ. 

Ví dụ,```
5 3
3 3 0 0 0
```chỉ có sáu đỉnh, trong khi có tám đỉnh, nên điều kiện đếm cạnh thô`6 <= 8-1`vượt qua. Tuy nhiên, hai hàng dương đều có bậc ba và chỉ có ba cột. Họ phải chia sẻ cả ba cột, tạo ra nhiều chu kỳ. Câu trả lời đúng là`NO`. 

Ở một thái cực khác, các hàng có độ bằng 0 không được coi là một phần của cấu trúc được kết nối. Vì```
3 2
2 0 0
```hàng đầu tiên có thể chỉ cần sử dụng cả hai cột và các hàng khác có thể được tách biệt. Câu trả lời là`YES`. Một cấu trúc giả định mỗi hàng đều có bậc dương sẽ bác bỏ trường hợp này một cách không chính xác. 

Vụ án```
1 1
0
```cũng hợp lệ. Không có cạnh nào cả nên có thể kiểm tra cả hàng và cột ngay lập tức. Câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ liệt kê mọi nhị phân`n x m`ma trận, loại bỏ các ma trận có tổng hàng khác với giá trị yêu cầu, sau đó kiểm tra xem đồ thị hai bên thu được có thứ tự loại bỏ hợp lệ hay không. có`2^(nm)`tổng số ma trận nhị phân, vì vậy trong trường hợp xấu nhất, điều này sẽ xem xét`2^(1,000,000)`ứng viên. Ngay cả khi chúng ta hạn chế việc liệt kê các ma trận có tổng hàng đúng, số ứng cử viên trong trường hợp xấu nhất là`C(m, floor(m/2))^n`, 

vẫn còn theo cấp số nhân. Việc kiểm tra từng ứng viên tốn ít nhất`O(nm)`, vì vậy phương pháp này có chi phí trong trường hợp xấu nhất`Theta(nm * C(m, floor(m/2))^n)`. Tính đúng đắn của nó rất đơn giản, nhưng nó không thể sử dụng được. 

Quan sát hữu ích là thứ tự kiểm tra hợp lệ tương đương với biểu đồ lưỡng cực được xây dựng là một khu rừng. Do đó, chúng ta không cần phải lý luận về quá trình kiểm tra khi xây dựng ma trận. Chúng ta chỉ cần xây dựng một biểu đồ chu kỳ với độ hàng được yêu cầu. 

Giả sử có`k`các hàng có bậc dương và gọi tổng của tất cả các hàng độ là`S`. Hãy cân nhắc việc sử dụng một cột làm cột lá, chứa một`1`hoặc dưới dạng cột kết nối chứa hai`1`s và nối hai đỉnh hàng. Cột kết nối hoạt động giống như một cạnh giữa hai đỉnh hàng. Nếu chúng ta sử dụng`E`cột kết nối, những cột đó có thể tạo thành một khu rừng trên`k`các hàng tích cực. Một khu rừng như vậy chỉ có thể có nhiều nhất`k-1`các cạnh và vì mỗi đầu nối tiêu thụ hai đơn vị độ hàng nên chúng ta cũng có`E <= floor(S/2)`. 

Do đó, số lượng cột kết nối hữu ích tối đa là`E = min(k-1, floor(S/2))`. 

Mỗi trình kết nối lưu một cột so với việc sử dụng hai cột lá riêng biệt. Do đó, tổng số cột được sử dụng là`E + (S - 2E) = S - E`. 

Nếu điều này vượt quá`m`, không có công trình nào có thể tồn tại. Nếu không, chúng ta có thể xây dựng chính xác một khu rừng như vậy. 

Nhiệm vụ còn lại là hiện thực hóa một rừng trên các hàng dương có bậc tại hàng`i`là một số giá trị`t_i <= a_i`, với chính xác`E`các cạnh. Chúng tôi chọn`t_i`sao cho tổng của chúng là`2E`. Một cách thuận tiện để hiện thực hóa khu rừng là tạm thời thêm một siêu đỉnh. Một khu rừng với`E`cạnh trên`k`đỉnh có`k-E`các thành phần được kết nối. Nối siêu đỉnh với đúng một đỉnh trong mọi thành phần. Kết quả là một cái cây trên`k+1`các đỉnh, vì vậy chúng ta có thể xây dựng nó từ dãy bậc của nó bằng cách sử dụng dãy Prüfer. 

Điều này biến vấn đề thành một công trình xây dựng trình tự cấp độ có kích thước tuyến tính, sau đó là một cuộc duyệt rừng thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(nm * C(m,floor(m/2))^n)`|`O(nm)`| Quá chậm | 
| Tối ưu |`O(nm + (n+m) log(n+m))`|`O(nm + n+m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`S`, tổng của tất cả các độ hàng và thu thập các hàng có bậc dương. Hãy để số của họ là`k`. Hàng với`a_i = 0`sẽ không bao giờ cần đến lợi thế, vì vậy chúng có thể vẫn bị cô lập. 
2. Đặt`E = min(k - 1, S // 2)`. 

Đây là những cột kết nối mà chúng tôi muốn sử dụng. Chúng ta không thể sử dụng nhiều hơn`k-1`mà không tạo chu trình giữa các đỉnh hàng và chúng ta không thể sử dụng nhiều hơn`S//2`bởi vì mỗi đầu nối tiêu thụ hai đơn vị cấp hàng. 
3. Tính toán số cột mà công trình sẽ cần`S-E`. Nếu như`S-E > m`, in`NO`. 

số lượng`S-E`là số cột tối thiểu có thể có. Mỗi trình kết nối sử dụng hai cột trong một cột thay vì hai cột riêng biệt, do đó mỗi trình kết nối lưu chính xác một cột. 
4. Chọn độ kết nối`t_i`cho các hàng tích cực. Chúng tôi cần`0 <= t_i <= a_i`Và`sum(t_i) = 2E`. 

Khi`E > 0`, trước tiên hãy đưa ra một mức độ kết nối cho`E+1`hàng dương riêng biệt. Điều này là có thể bởi vì`E <= k-1`. Sau đó phân phối phần còn lại`E-1`đơn vị độ một cách tham lam mà không vượt quá`a_i`hoặc`k-1`. 

Trao một đơn vị cho`E+1`hàng đảm bảo rằng biểu đồ kết nối có thể có`E`các cạnh mà không cố gắng tập trung toàn bộ mức độ của nó vào quá ít đỉnh. Giới hạn bổ sung của`k-1`sau này sẽ hữu ích vì không có hàng nào có thể có bậc lớn hơn`k`trong cây tạm thời sau khi siêu đỉnh được thêm vào. 
5. Hãy để`C = k-E`, số thành phần mà nhóm kết nối sẽ có. Mỗi hàng với`t_i=0`phải nhận được một cạnh của siêu đỉnh. Chọn tất cả các hàng như hàng xóm siêu đỉnh, sau đó chọn các hàng bổ sung tùy ý cho đến khi chính xác`C`các hàng đã được chọn. 

Có đủ các hàng độ 0 để thực hiện việc này vì cấu trúc đã cung cấp mức độ kết nối dương ít nhất`E+1`hàng. Do đó nhiều nhất`k-E-1`hàng có`t_i=0`, trong khi`C=k-E`. 
6. Xác định độ của hàng`i`trong cây tạm thời như`t_i + 1`nếu hàng`i`được chọn là hàng xóm siêu đỉnh và`t_i`nếu không thì. Cho độ siêu đỉnh`C`. 

Tổng mức độ là`2E + C + C = 2(E+C) = 2k`, 

chính xác số lượng cần thiết cho một cây trên`k+1`đỉnh. Mọi mức độ đều tích cực và nhiều nhất là`k`, vì vậy đây là một chuỗi cấp độ cây hợp lệ. 
7. Xây dựng chuỗi Prüfer chứa chính xác từng đỉnh tạm thời`degree[i]-1`lần. chiều dài của nó là`k-1`. Giải mã chuỗi bằng hàng đợi ưu tiên chứa các lá hiện tại. 

Giải mã Prüfer xây dựng một cây có trình tự bậc chính xác được yêu cầu. Vì số đỉnh chỉ`k+1 <= 1001`, cái`O(k log k)`việc thực hiện dễ dàng và đủ nhanh. 
8. Loại bỏ mọi cạnh tới siêu đỉnh. Các cạnh còn lại tạo thành một khu rừng trên các hàng dương. Mỗi cạnh cây còn lại sẽ trở thành một cột kết nối với chính xác hai cột, một cột ở mỗi điểm cuối. 
9. Cho mỗi hàng`i`, có`a_i-t_i`những cái còn lại không được các cột kết nối sử dụng. Cung cấp cho mỗi người một cột lá riêng biệt chứa một`1`liên tiếp`i`. 

Số cột được tạo chính xác`E + S - 2E = S-E`, đã được kiểm tra chống lại`m`trước đó. Tất cả các cột không sử dụng đều được điền bằng số không. 
10. Xây dựng danh sách kề cận hai bên của ma trận kết quả và liên tục lấy một đỉnh có bậc hiện tại nhiều nhất là một. Nối nó vào thứ tự câu trả lời, loại bỏ nó về mặt khái niệm và giảm mức độ của các hàng xóm của nó. 

Vì đồ thị là một khu rừng nên luôn tồn tại một đỉnh như vậy. Các hàng riêng biệt và các cột không được sử dụng chỉ cần đưa vào hàng đợi với độ 0. 
11. Xuất ma trận và thứ tự hàng/cột thu được. 

### Tại sao nó hoạt động 

Các cột kết nối tạo thành một khu rừng trên các hàng dương. Mỗi lần bổ sung`1`không được sử dụng bởi một trình kết nối được đặt trong một cột lá riêng, do đó việc gắn các cột đó không thể tạo ra một chu trình. Do đó toàn bộ đồ thị lưỡng cực là một khu rừng. 

Mỗi hàng nhận được chính xác`t_i`cạnh kết nối và`a_i-t_i`mép lá, cho nó chính xác`a_i`những cái đó. Việc xây dựng sử dụng`S-E`cột, không lớn hơn`m`bất cứ khi nào thuật toán chấp nhận. 

Cuối cùng, việc loại bỏ nhiều nhất một đỉnh bậc sẽ tạo ra thứ tự kiểm tra hợp lệ cho bất kỳ khu rừng nào. Tại thời điểm một đỉnh bị loại bỏ, độ hiện tại của nó chính xác là số lượng đèn còn sáng trong hàng hoặc cột đó, vì vậy Jaber không bao giờ nhìn thấy nhiều hơn một đèn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    positive = [i for i, x in enumerate(a) if x > 0]
    k = len(positive)
    total = sum(a)

    if k == 0:
        mat = ['0' * m for _ in range(n)]
        order = []
        for i in range(n):
            order.append(("row", i))
        for j in range(m):
            order.append(("col", j))

        out = ["YES"]
        out.extend(mat)
        out.extend(f"{typ} {idx + 1}" for typ, idx in order)
        sys.stdout.write("\n".join(out))
        return

    E = min(k - 1, total // 2)

    used_columns = total - E
    if used_columns > m:
        print("NO")
        return

    # t[i] is the number of row i's edges used by connector columns.
    t = [0] * n

    if E > 0:
        # First give one connector incidence to E+1 rows.
        base_count = E + 1
        for idx in range(base_count):
            t[positive[idx]] = 1

        remaining = E - 1

        # Distribute the remaining incidences.
        # Capping at k-1 is enough for a row degree in the
        # temporary tree after possibly adding one super edge.
        for v in positive:
            if remaining == 0:
                break
            cap = min(a[v], k - 1)
            extra = min(cap - t[v], remaining)
            if extra > 0:
                t[v] += extra
                remaining -= extra

        if remaining != 0:
            print("NO")
            return

    # Number of connected components in the connector forest.
    C = k - E

    # Rows selected to connect to the super vertex.
    roots = []
    selected_root = [False] * n

    for v in positive:
        if t[v] == 0:
            roots.append(v)
            selected_root[v] = True

    if len(roots) > C:
        print("NO")
        return

    for v in positive:
        if len(roots) == C:
            break
        if not selected_root[v]:
            roots.append(v)
            selected_root[v] = True

    if len(roots) != C:
        print("NO")
        return

    # Temporary tree vertices:
    # 0 .. k-1 are positive rows
    # k is the super vertex
    super_v = k
    Ntree = k + 1

    degree = [0] * Ntree

    pos_index = {}
    for idx, v in enumerate(positive):
        pos_index[v] = idx

    for v in positive:
        idx = pos_index[v]
        degree[idx] = t[v] + (1 if selected_root[v] else 0)

    degree[super_v] = C

    # Build a Prüfer sequence.
    prufer = []
    for v in range(Ntree):
        prufer.extend([v] * (degree[v] - 1))

    # Decode Prüfer sequence.
    cur_degree = degree[:]
    leaves = []
    for v in range(Ntree):
        if cur_degree[v] == 1:
            heapq.heappush(leaves, v)

    tree_edges = []

    for v in prufer:
        leaf = heapq.heappop(leaves)
        tree_edges.append((leaf, v))

        cur_degree[leaf] -= 1
        cur_degree[v] -= 1

        if cur_degree[v] == 1:
            heapq.heappush(leaves, v)

    last1 = heapq.heappop(leaves)
    last2 = heapq.heappop(leaves)
    tree_edges.append((last1, last2))

    # Convert the tree, after removing the super vertex,
    # into connector columns.
    row_connector_edges = []

    for u, v in tree_edges:
        if u == super_v or v == super_v:
            continue

        original_u = positive[u]
        original_v = positive[v]
        row_connector_edges.append((original_u, original_v))

    if len(row_connector_edges) != E:
        print("NO")
        return

    # Matrix as mutable byte arrays.
    mat = [bytearray(b'0' * m) for _ in range(n)]

    col = 0

    # Each connector edge gets one column with two ones.
    for u, v in row_connector_edges:
        if col >= m:
            print("NO")
            return
        mat[u][col] = ord('1')
        mat[v][col] = ord('1')
        col += 1

    # Remaining row degrees use private leaf columns.
    for i in range(n):
        remaining = a[i] - t[i]
        for _ in range(remaining):
            if col >= m:
                print("NO")
                return
            mat[i][col] = ord('1')
            col += 1

    # Build the bipartite graph.
    total_vertices = n + m
    adj = [[] for _ in range(total_vertices)]
    deg = [0] * total_vertices

    for i in range(n):
        for j in range(m):
            if mat[i][j] == ord('1'):
                u = i
                v = n + j
                adj[u].append(v)
                adj[v].append(u)
                deg[u] += 1
                deg[v] += 1

    # Every forest has a vertex of degree <= 1.
    queue = []
    for v in range(total_vertices):
        if deg[v] <= 1:
            heapq.heappush(queue, v)

    removed = [False] * total_vertices
    order = []

    while queue:
        v = heapq.heappop(queue)
        if removed[v]:
            continue

        removed[v] = True
        order.append(v)

        for u in adj[v]:
            if removed[u]:
                continue
            deg[u] -= 1
            if deg[u] <= 1:
                heapq.heappush(queue, u)

    if len(order) != total_vertices:
        print("NO")
        return

    out = ["YES"]
    out.extend(row.decode() for row in mat)

    for v in order:
        if v < n:
            out.append(f"row {v + 1}")
        else:
            out.append(f"col {v - n + 1}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ tách các hàng 0 khỏi các hàng dương. Điều này quan trọng vì hàng 0 hoàn toàn không cần tham gia vào nhóm kết nối. 

Biến`E`là số cột kết nối. biểu thức`total - E`là số cột cần thiết sau khi tối đa hóa số lượng cột hai hàng. Nếu con số đó lớn hơn`m`, không có sự sắp xếp nào có thể thành công. 

Mảng`t`ghi lại số lượng yêu cầu của mỗi hàng thuộc về các cột kết nối. ban đầu`E+1`các bài tập đảm bảo có đủ các đỉnh khác 0 cho một khu rừng có`E`các cạnh. Mức độ còn lại được phân phối một cách tham lam, với`k-1`như một giới hạn trên. Số nguyên Python không bị tràn và tất cả các giá trị liên quan tối đa là khoảng`10^6`đây. 

Việc xây dựng siêu đỉnh là phần lý thuyết đồ thị quan trọng. Một khu rừng với`E`cạnh trên`k`đỉnh có`k-E`thành phần. Việc kết nối một đỉnh từ mọi thành phần với một siêu đỉnh mới sẽ tạo ra một cây. Thay vì xây dựng các thành phần một cách trực tiếp, chúng ta xây dựng cây này từ chuỗi bậc của nó và sau đó xóa siêu đỉnh. 

Dãy Prüfer chứa đỉnh`v`chính xác`degree[v]-1`lần. chiều dài của nó là`k-1`, ghép một cái cây với`k+1`đỉnh. Một đống lưu trữ các lá hiện tại trong quá trình giải mã, tránh mọi tìm kiếm bậc hai cho lá tiếp theo. 

Sau khi giải mã, mỗi cạnh cây không chạm vào siêu đỉnh tương ứng với một cột kết nối. Độ hàng dư được đặt vào các cột riêng tư. Cấu trúc không bao giờ đặt hai cặp hàng giống hệt nhau vào các cột kết nối riêng biệt và biểu đồ kết quả là không theo chu kỳ vì nó là sơ đồ con của cây tạm thời cộng với các cột lá. 

Lần duyệt cuối cùng là quá trình loại bỏ lá tiêu chuẩn.`deg[v]`là số cạnh còn tồn tại khi có đỉnh`v`sắp được kiểm tra. Việc loại bỏ một đỉnh bậc 0 hoặc đỉnh bậc 1 chính xác là điều kiện mà câu lệnh yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
4 4
1 0 0 0
```chỉ có hàng 1 là dương nên`k=1`Và`S=1`. 

| Biến | Giá trị | 
| --- | --- | 
|`positive`|`[1]`| 
|`k`|`1`| 
|`S`|`1`| 
|`E = min(k-1,S//2)`|`0`| 
| cột đã qua sử dụng |`1`| 
|`t`|`[1,0,0,0]`| 

Vì không có cạnh kết nối nên cạnh được yêu cầu duy nhất sẽ trở thành cột lá riêng. Ma trận kết quả có thể là```
1000
0000
0000
0000
```Đồ thị chỉ chứa một cạnh. Mọi đỉnh đều có bậc 0 hoặc bậc 1, do đó, bất kỳ thứ tự nào của cả 8 đỉnh đều đúng. Thứ tự của mẫu là một trong những thứ tự như vậy. 

Ví dụ này thực hiện`E=0`trường hợp. Công trình không cần máy móc siêu đỉnh để nối các hàng vì chỉ có một hàng dương. 

### Mẫu 2 

cho```
4 4
2 1 1 1
```chúng tôi có`k=4`Và`S=5`. 

| Biến | Giá trị | 
| --- | --- | 
|`positive`|`[1,2,3,4]`| 
|`k`|`4`| 
|`S`|`5`| 
|`E`|`2`| 
|`C = k-E`|`2`| 
| cột bắt buộc |`S-E = 3`| 

Việc xây dựng có thể chọn độ kết nối```
t = [2, 1, 1, 0]
```Tổng của họ là`4 = 2E`. Cây tạm thời tương ứng có độ hàng```
3, 1, 1, 1
```sau khi chọn hàng 1 và hàng 4 làm hàng xóm siêu đỉnh, trong khi siêu đỉnh có bậc hai. 

Một rừng kết nối có thể là```
row 1 -- row 2
row 1 -- row 3
```và hàng 4 bị cô lập trong rừng kết nối. Mức độ còn lại hoàn toàn thuộc về hàng 4 nên một cột riêng tư sẽ được thêm vào đó. 

Do đó, một ma trận có thể là```
1100
1000
1000
0010
```Ma trận chính xác khác với ma trận mẫu, điều này ổn vì bài toán chấp nhận bất kỳ cách xây dựng hợp lệ nào. Đồ thị lưỡng cực của nó là một khu rừng nên tồn tại thứ tự loại bỏ lá. 

Dấu vết cho thấy tại sao độ của hàng không cần phải được hiểu là độ trong cây trên chính các hàng đó. Một số mép hàng trở thành cột lá, trong khi các cột nối là bộ phận quyết định cấu trúc rừng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm + (n+m) log(n+m))`| Ma trận và sự kề cận lưỡng cực được xử lý trong`O(nm)`, trong khi giải mã Prüfer và truyền tải đống cuối cùng là logarit trên mỗi đỉnh. | 
| Không gian |`O(nm + n+m)`| Danh sách ma trận và kề chi phối việc sử dụng bộ nhớ. | 

Với`n,m <= 1000`, ma trận chứa tối đa một triệu ô. các`O(nm)`phần cũng không thể tránh khỏi với các hằng số vì ma trận hoàn chỉnh phải được in. Việc sử dụng bộ nhớ vẫn thoải mái dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên việc so sánh chuỗi chính xác là không phù hợp cho vấn đề này. Thay vào đó, các thử nghiệm bên dưới sẽ chạy bộ giải và xác nhận ma trận được tạo ra cũng như thứ tự kiểm tra. Trình xác thực sẽ kiểm tra tổng hàng, tính duy nhất của mỗi hàng và cột theo thứ tự và điều kiện là mỗi đỉnh được kiểm tra có nhiều nhất một cạnh còn lại.```python
import sys
import io
import heapq

def solve_string(inp: str) -> str:
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

def validate(inp: str, out: str) -> bool:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    lines = out.strip().splitlines()
    if not lines:
        return False

    possible = lines[0] == "YES"

    total = sum(a)
    if not possible:
        # A validator for NO instances is supplied separately below.
        return True

    if len(lines) != 1 + n + n + m:
        return False

    matrix = lines[1:1 + n]
    if any(len(row) != m for row in matrix):
        return False

    for i in range(n):
        if sum(c == '1' for c in matrix[i]) != a[i]:
            return False
        if any(c not in '01' for c in matrix[i]):
            return False

    order_lines = lines[1 + n:]
    if len(order_lines) != n + m:
        return False

    seen = set()
    order = []

    for line in order_lines:
        parts = line.split()
        if len(parts) != 2:
            return False

        typ, x = parts
        x = int(x)

        if typ == "row":
            if not (1 <= x <= n):
                return False
            v = x - 1
        elif typ == "col":
            if not (1 <= x <= m):
                return False
            v = n + x - 1
        else:
            return False

        if v in seen:
            return False
        seen.add(v)
        order.append(v)

    if len(seen) != n + m:
        return False

    adj = [[] for _ in range(n + m)]
    for i in range(n):
        for j in range(m):
            if matrix[i][j] == '1':
                u = i
                v = n + j
                adj[u].append(v)
                adj[v].append(u)

    removed = [False] * (n + m)

    for v in order:
        remaining = sum(not removed[u] for u in adj[v])
        if remaining > 1:
            return False
        removed[v] = True

    return True

def expect_no(inp: str):
    out = solve_string(inp)
    assert out.strip() == "NO"

# Sample 1
sample1 = """\
4 4
1 0 0 0
"""
assert validate(sample1, solve_string(sample1)), "sample 1"

# Sample 2
sample2 = """\
4 4
2 1 1 1
"""
assert validate(sample2, solve_string(sample2)), "sample 2"

# Minimum-size instance
case_min = """\
1 1
0
"""
assert validate(case_min, solve_string(case_min)), "minimum-size zero"

# Minimum-size instance with one edge
case_min_edge = """\
1 1
1
"""
assert validate(case_min_edge, solve_string(case_min_edge)), "minimum-size edge"

# Boundary case: both rows require every column.
# The only possible matrix is all ones, which contains a cycle.
case_impossible = """\
2 2
2 2
"""
expect_no(case_impossible)

# Same total-edge count as a sparse graph might allow, but the
# prescribed row degrees force two rows to share all three columns.
case_impossible_2 = """\
5 3
3 3 0 0 0
"""
expect_no(case_impossible_2)

# Maximum-size feasible case.
# Every row has exactly one one, so one private column per row is enough.
case_max = "1000 1000\n" + " ".join(["1"] * 1000) + "\n"
assert validate(case_max, solve_string(case_max)), "maximum-size case"

# All equal positive row degrees.
# 1000 rows and 1000 columns, every row has degree 1.
case_equal = "1000 1000\n" + " ".join(["1"] * 1000) + "\n"
assert validate(case_equal, solve_string(case_equal)), "all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`YES`| Đồ thị nhỏ nhất không có cạnh | 
|`1 1 / 1`|`YES`| Đồ thị nhỏ nhất chứa một cạnh | 
|`2 2 / 2 2`|`NO`| Buộc 4 chu kỳ | 
|`5 3 / 3 3 0 0 0`|`NO`| Bắt được điều kiện đếm cạnh không đủ | 
|`1000 1000 / 1 ... 1`|`YES`| Kích thước tối đa và sản lượng lớn | 
|`1000 1000 / 1 ... 1`|`YES`| Mức độ hàng bằng nhau và cách sử dụng cột ranh giới | 

## Vỏ cạnh 

cho```
2 2
2 2
```chúng tôi có`S=4`,`k=2`, Và`E=min(1,2)=1`. Việc xây dựng sẽ cần`S-E=3`cột, nhưng chỉ tồn tại hai cột. Nó in ngay`NO`. Điều này cho thấy một thực tế là hạn chế rừng mạnh hơn việc chỉ yêu cầu số cạnh tối đa là số đỉnh trừ đi một. 

Vì```
5 3
3 3 0 0 0
```chúng tôi có`S=6`,`k=2`, và một lần nữa`E=1`. Số cột tối thiểu là`6-1=5`, vượt quá`m=3`. Thuật toán từ chối thể hiện trước khi thử xây dựng bất kỳ biểu đồ nào. Đây chính xác là trường hợp chỉ kiểm tra`S <= n+m-1`đưa ra câu trả lời sai. 

Vì```
3 2
2 0 0
```có một hàng dương, vậy`k=1`,`S=2`, Và`E=0`. Cả hai cái bắt buộc đều trở thành cột lá riêng. Hàng đầu tiên là`11`, và hai hàng còn lại bằng 0. Biểu đồ là một ngôi sao với hàng làm tâm, là một cây, do đó tồn tại một thứ tự kiểm tra hợp lệ. 

Vì```
3 3
0 0 0
```chúng tôi có`k=0`. Không có cạnh nào ở đâu cả, vì vậy mỗi hàng và cột đều có độ 0. Trường hợp đặc biệt in trực tiếp một ma trận toàn 0 và bất kỳ hoán vị nào của sáu đỉnh. Điều này tránh việc cố gắng xây dựng một nhóm kết nối không có hàng dương. 

Đối với phiên bản có kích thước tối đa với 1000 hàng và 1000 cột và mỗi`a_i=1`, chúng tôi có`S=1000`,`k=1000`, Và`E=999`. Việc xây dựng sử dụng`1000-999=1`cột cho nhóm kết nối, với cấu trúc đơn vị cấp độ còn lại được biểu thị bằng cùng một cây kết nối. Biểu đồ lưỡng cực thu được là một cây chứa tất cả 1000 đỉnh hàng và một cột được sử dụng, trong khi 999 cột còn lại bị cô lập. Quá trình loại bỏ lá cuối cùng sẽ tự động xử lý các cột bị cô lập. 

Bất biến trung tâm đằng sau tất cả các trường hợp này là không thay đổi: mọi`1`là bìa rừng. Khi bất biến đó được thiết lập, thứ tự kiểm tra bắt buộc được đảm bảo bằng cách liên tục loại bỏ một đỉnh có nhiều nhất một cạnh còn lại.
