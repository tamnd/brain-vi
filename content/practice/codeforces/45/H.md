---
title: "CF 45H - Sự cố đường bộ"
description: "Chúng ta được cung cấp một đồ thị vô hướng được kết nối đại diện cho một mạng lưới đường bộ. Mọi giao điểm đều là một đỉnh, mọi con đường đều là một cạnh và có nhiều nhất một cạnh nằm giữa bất kỳ cặp đỉnh nào."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "H"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 2100
weight: 45
solve_time_s: 118
verified: true
draft: false
---

[CF 45H - Sự cố trên đường](https://codeforces.com/problemset/problem/45/H) 

**Xếp hạng:** 2100 
**Thẻ:** đồ thị 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng được kết nối đại diện cho một mạng lưới đường bộ. Mọi giao điểm đều là một đỉnh, mọi con đường đều là một cạnh và có nhiều nhất một cạnh nằm giữa bất kỳ cặp đỉnh nào. 

Thành phố muốn đồ thị cuối cùng thỏa mãn điều kiện kết nối mạnh mẽ hơn: giữa mỗi cặp nút giao phải tồn tại ít nhất hai đường đi cách nhau. Trong lý thuyết đồ thị, điều này có nghĩa là đồ thị phải được kết nối 2 cạnh hoặc tương đương, đồ thị không được chứa cầu nối. 

Cầu là một cạnh mà việc loại bỏ nó sẽ làm mất kết nối của đồ thị. Bất kỳ đồ thị nào không có cầu đều tự động thỏa mãn yêu cầu, bởi vì mỗi cặp đỉnh khi đó có hai đường đi khác nhau giữa chúng. 

Chúng ta có thể thêm các cạnh mới nhưng không được phép tạo các cạnh song song. Nhiệm vụ là cộng số cạnh tối thiểu để đồ thị thu được không có cầu nối. Nếu điều này không thể thực hiện được vì mọi cạnh hữu ích có thể đã tồn tại, chúng ta sẽ in`-1`. 

Đồ thị có nhiều nhất là 900 đỉnh nhưng có tới 100000 cạnh. Số lượng cạnh lớn ngay lập tức loại trừ các thuật toán liên tục tính toán lại kết nối từ đầu sau mỗi lần bổ sung cạnh ứng cử viên. Một sự ngây thơ`O(m * (n + m))`việc tính toán lại cầu đã quá lớn trong các biểu đồ dày đặc. 

Giới hạn đỉnh nhỏ là ràng buộc thú vị hơn. Từ`n ≤ 900`, MỘT`O(n^2)`tìm kiếm trên các cặp đỉnh là hoàn toàn có thể chấp nhận được. Điều này trở nên hữu ích sau này khi chúng ta cần tìm các cạnh bị thiếu trong đồ thị phần bù. 

Có một số trường hợp khó xử lý. 

Hãy xem xét một biểu đồ đã được kết nối 2 cạnh:```
3 3
1 2
2 3
3 1
```Câu trả lời đúng là:```
0
```Một giải pháp bất cẩn vẫn có thể cố gắng kết nối các lá của một số cây DFS mặc dù không có cầu nối. 

Bây giờ hãy xem xét một cái cây:```
4 3
1 2
2 3
3 4
```Mỗi cạnh là một cây cầu. Câu trả lời tối ưu là một lợi thế mới:```
1
1 4
```Việc thêm cạnh đơn này sẽ tạo ra một chu trình chứa tất cả các cạnh ban đầu. 

Một trường hợp tinh tế hơn xuất hiện khi biểu đồ gần như hoàn chỉnh:```
4 5
1 2
1 3
1 4
2 3
2 4
```Cạnh còn thiếu duy nhất là`(3,4)`. Biểu đồ vẫn chứa các cây cầu? Không, không, vì vậy câu trả lời là`0`. Nhưng hãy tưởng tượng một biểu đồ trong đó việc sửa các cây cầu sẽ yêu cầu thêm một cạnh đã có sẵn. Vì các cạnh song song bị cấm nên một số đồ thị trở nên không thể thực hiện được. 

Ví dụ:```
2 1
1 2
```Cạnh duy nhất có thể đã tồn tại. Chúng tôi không thể thêm một bản sao khác của`(1,2)`, vậy đáp án là:```
-1
```Giải pháp bỏ qua điều kiện "không có nhiều cạnh" sẽ tạo ra một cạnh thừa không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liên tục tìm tất cả các cây cầu, sau đó thử mọi cạnh còn thiếu có thể và đánh giá xem có bao nhiêu cây cầu biến mất sau khi thêm nó. Thêm cạnh tốt nhất, cập nhật biểu đồ và tiếp tục cho đến khi không còn cầu nào. 

Điều này có hiệu quả vì việc thêm các cạnh chỉ có thể làm giảm số lượng cầu nối. Cuối cùng đồ thị trở thành liên 2 cạnh. 

Vấn đề là chi phí. Việc phát hiện cầu mất`O(n + m)`bằng thuật toán Tarjan. có`O(n^2)`các cạnh ứng cử viên và chúng ta có thể thêm nhiều cạnh. Trong các biểu đồ dày đặc, điều này dễ dàng phát triển vượt quá hàng trăm triệu thao tác. 

Quan sát quan trọng là những cây cầu tạo thành một cấu trúc cây. 

Nếu chúng ta nén mọi thành phần được kết nối 2 cạnh tối đa vào một nút duy nhất thì tất cả các cạnh cầu còn lại sẽ tạo thành một cây gọi là cây cầu. Mỗi lá trong cây này tương ứng với một thành phần được gắn qua đúng một cây cầu. 

Để loại bỏ tất cả các cây cầu, các lá phải được ghép lại với nhau bằng các mép mới. Mỗi cạnh được thêm vào có thể giảm số lượng lá nhiều nhất là hai. Điều này mang lại giới hạn dưới cổ điển:$$\left\lceil \frac{\text{number of leaves}}{2} \right\rceil$$Đối với các biểu đồ không bị giới hạn, giới hạn này luôn có thể đạt được. 

Điều phức tạp hơn ở đây là việc cấm các cạnh song song. Khi chúng ta kết nối hai lá cây cầu, chúng ta phải chọn các đỉnh thực tế từ những thành phần chưa liền kề. Từ`n ≤ 900`, chúng ta có thể tìm kiếm trực tiếp các cặp như vậy. 

Toàn bộ vấn đề trở thành: 

1. Tìm những cây cầu. 
2. Xây dựng các thành phần kết nối cầu. 
3. Xây dựng cây cầu. 
4. Ghép lá. 
5. Đối với mỗi cặp thành phần lá, hãy tìm một cạnh còn thiếu giữa chúng. 

Nếu tại một thời điểm nào đó không có cạnh nào bị thiếu thì câu trả lời là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2(n + m)) hoặc tệ hơn | O(n + m) | Quá chậm | 
| Tối ưu | O(n2 + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS tìm cầu của Tarjan. 

Đối với mỗi cạnh, hãy xác định xem đó có phải là cầu nối hay không bằng cách sử dụng thời gian khám phá và giá trị liên kết thấp. 
2. Loại bỏ tất cả các cầu nối về mặt khái niệm và tính toán các thành phần được kết nối. 

Các đỉnh nối không dùng cầu đều thuộc cùng một thành phần nối 2 cạnh. 
3. Nén từng thành phần vào một nút và xây dựng cây cầu. 

Mọi cây cầu ban đầu đều trở thành một cạnh giữa hai thành phần bị nén. Vì cầu không thể hình thành chu trình sau khi nén nên cấu trúc này là một cây. 
4. Đếm số lá cây cầu. 

Một thành phần có mức độ`1`trong cây cầu là thành phần lá. 
5. Ghép các lá từ hai đầu đối diện nhau. 

Nếu có`k`lá, số cạnh tối thiểu cần thiết là`(k + 1) // 2`. 

Một cách tiêu chuẩn là liệt kê tất cả các lá và cặp:`leaf[i]`với`leaf[i + k/2]`. 
6. Với mỗi cặp thành phần lá, hãy tìm hai đỉnh không có cạnh hiện có. 

Chúng tôi lặp lại tất cả các đỉnh trong thành phần đầu tiên và tất cả các đỉnh trong thành phần thứ hai cho đến khi tìm thấy một cặp`(u, v)`sao cho không có cạnh nào tồn tại. 
7. Thêm các cạnh đó vào câu trả lời. 

Mỗi cạnh được thêm vào sẽ tạo ra một chu trình đi qua các cây cầu trên đường đi giữa hai lá, loại bỏ những cây cầu đó. 
8. Nếu bất kỳ cặp được yêu cầu nào không có sẵn cạnh, hãy in`-1`. 

Vì các cạnh song song bị cấm nên thao tác sửa chữa đó không thể thực hiện được. 

### Tại sao nó hoạt động 

Sau khi nén các thành phần nối 2 cạnh, mỗi cạnh còn lại là một cây cầu nên đồ thị nén là một cây. 

Trong một cái cây, những cây cầu tồn tại chính xác là do những chiếc lá được gắn qua một cạnh duy nhất. Việc kết nối hai chiếc lá sẽ tạo ra một vòng tuần hoàn dọc theo con đường duy nhất giữa chúng, loại bỏ tất cả các cây cầu trên con đường đó. 

Mỗi cạnh được thêm vào có thể loại bỏ tối đa hai khiếm khuyết của lá, do đó ít nhất`ceil(leaves / 2)`các cạnh là cần thiết. Chiến lược ghép đôi đạt được chính xác giới hạn này, khiến nó trở nên tối ưu. 

Vấn đề duy nhất còn lại là tính sẵn có của cạnh. Vì biểu đồ cuối cùng không thể chứa nhiều cạnh nên chúng ta phải đảm bảo mọi cặp thành phần được chọn đều chứa ít nhất một cặp đỉnh không liền kề. Nếu một cặp như vậy không tồn tại thì không thể thêm cạnh sửa chữa hợp lệ nào vào giữa các thành phần đó và việc xây dựng trở nên không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

def solve():
    n, m = map(int, input().split())

    g = [[] for _ in range(n)]
    edges = []
    adj = [[False] * n for _ in range(n)]

    for idx in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        edges.append((u, v))

        g[u].append((v, idx))
        g[v].append((u, idx))

        adj[u][v] = True
        adj[v][u] = True

    tin = [-1] * n
    low = [0] * n
    is_bridge = [False] * m
    timer = 0

    def dfs(v, pe):
        nonlocal timer

        tin[v] = low[v] = timer
        timer += 1

        for to, eid in g[v]:
            if eid == pe:
                continue

            if tin[to] != -1:
                low[v] = min(low[v], tin[to])
            else:
                dfs(to, eid)
                low[v] = min(low[v], low[to])

                if low[to] > tin[v]:
                    is_bridge[eid] = True

    dfs(0, -1)

    comp = [-1] * n
    comps = []

    def dfs_comp(v, cid):
        comp[v] = cid
        comps[cid].append(v)

        for to, eid in g[v]:
            if is_bridge[eid]:
                continue
            if comp[to] == -1:
                dfs_comp(to, cid)

    cid = 0
    for i in range(n):
        if comp[i] == -1:
            comps.append([])
            dfs_comp(i, cid)
            cid += 1

    if cid == 1:
        print(0)
        return

    tree = [[] for _ in range(cid)]
    deg = [0] * cid

    for eid, (u, v) in enumerate(edges):
        cu = comp[u]
        cv = comp[v]

        if cu != cv:
            tree[cu].append(cv)
            tree[cv].append(cu)
            deg[cu] += 1
            deg[cv] += 1

    leaves = []

    for i in range(cid):
        if deg[i] == 1:
            leaves.append(i)

    k = len(leaves)
    need = (k + 1) // 2

    if k == 2:
        pairings = [(leaves[0], leaves[1])]
    else:
        pairings = []

        half = (k + 1) // 2

        for i in range(k // 2):
            pairings.append((leaves[i], leaves[i + half]))

        if k % 2 == 1:
            pairings.append((leaves[0], leaves[half - 1]))

    ans = []

    for a, b in pairings:
        found = False

        for u in comps[a]:
            for v in comps[b]:
                if not adj[u][v]:
                    ans.append((u + 1, v + 1))
                    adj[u][v] = adj[v][u] = True
                    found = True
                    break
            if found:
                break

        if not found:
            print(-1)
            return

    print(len(ans))
    for u, v in ans:
        print(u, v)

solve()
```Phần đầu tiên xây dựng biểu đồ và lưu trữ cả danh sách kề và ma trận kề. Danh sách kề được sử dụng để truyền tải DFS, trong khi ma trận kề cho phép chúng ta kiểm tra xem một cạnh đã tồn tại trong thời gian không đổi hay chưa. 

Bridge DFS là thuật toán Tarjan tiêu chuẩn.`tin[v]`lưu trữ thời gian nhập DFS và`low[v]`lưu trữ tổ tiên có thể truy cập sớm nhất thông qua các cạnh sau. Một cạnh`(v, to)`trở thành một cây cầu khi`low[to] > tin[v]`. 

Sau khi xác định các cầu nối, chúng tôi chạy một DFS khác bỏ qua các cạnh cầu nối. Mỗi lần duyệt đánh dấu một thành phần kết nối 2 cạnh tối đa. 

Cây cầu nén không được xây dựng rõ ràng như các đối tượng riêng biệt. Chúng ta chỉ cần độ thành phần và danh sách các đỉnh bên trong mỗi thành phần. 

Logic ghép lá là phần tinh tế nhất. Việc ghép các lá đối diện sẽ giúp cân bằng cây và đảm bảo số lượng cạnh được thêm vào tối ưu. Số lượng lá lẻ cần có thêm một cặp bao bọc xung quanh. 

Khi tìm kiếm một cạnh mới hợp lệ, chúng ta lặp qua các đỉnh thực tế của cả hai thành phần. Ma trận kề tránh các cạnh song song ngẫu nhiên. 

Việc triển khai cập nhật ma trận kề sau khi thêm từng cạnh. Nếu không có bản cập nhật này, các cặp sau này có thể vô tình sử dụng lại cùng một cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2
2 3
3 4
```Mỗi cạnh là một cây cầu, vì vậy mỗi đỉnh trở thành thành phần riêng của nó. 

| Bước | Linh kiện | Cây Cầu | Lá | Đã thêm cạnh | 
| --- | --- | --- | --- | --- | 
| Sau khi nén | {1}, {2}, {3}, {4} | 1-2-3-4 | 1, 4 | không | 
| Cặp lá | giống nhau | giống nhau | 1, 4 | (1,4) | 

Cạnh được thêm vào tạo ra chu trình`1-2-3-4-1`, vì vậy tất cả các cây cầu cũ đều biến mất. 

### Ví dụ 2 

đầu vào:```
6 6
1 2
2 3
3 1
3 4
4 5
5 6
```Tam giác`{1,2,3}`đã được kết nối 2 cạnh. Các cạnh`(3,4)`,`(4,5)`, Và`(5,6)`là những cây cầu. 

| Bước | Linh kiện | Cây Cầu | Lá | Đã thêm cạnh | 
| --- | --- | --- | --- | --- | 
| Sau khi nén | {1,2,3}, {4}, {5}, {6} | A-B-C-D | A, D | không | 
| Cặp lá | giống nhau | giống nhau | A, D | (1,6) | 

Cạnh mới kết nối hai thành phần lá và tạo ra một vòng tròn bao phủ mọi đường cầu. 

Dấu vết này chứng tỏ tại sao việc nén các vùng liên kết mạnh lại đơn giản hóa vấn đề một cách đáng kể. Thay vì suy luận về đồ thị ban đầu, chúng ta chỉ sửa chữa một cái cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + m) | Bridge DFS và DFS thành phần là các tìm kiếm tuyến tính, cặp đỉnh sử dụng tối đa O(n²) | 
| Không gian | O(n2 + m) | Ma trận kề cộng với lưu trữ đồ thị | 

các`n²`ma trận kề hoàn toàn an toàn cho`n = 900`, yêu cầu dưới một triệu mục nhập boolean. Việc truyền tải DFS là tuyến tính theo kích thước đồ thị, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n, m = map(int, input().split())

    g = [[] for _ in range(n)]
    edges = []
    adj = [[False] * n for _ in range(n)]

    for idx in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        edges.append((u, v))

        g[u].append((v, idx))
        g[v].append((u, idx))

        adj[u][v] = True
        adj[v][u] = True

    tin = [-1] * n
    low = [0] * n
    is_bridge = [False] * m
    timer = 0

    def dfs(v, pe):
        nonlocal timer

        tin[v] = low[v] = timer
        timer += 1

        for to, eid in g[v]:
            if eid == pe:
                continue

            if tin[to] != -1:
                low[v] = min(low[v], tin[to])
            else:
                dfs(to, eid)
                low[v] = min(low[v], low[to])

                if low[to] > tin[v]:
                    is_bridge[eid] = True

    dfs(0, -1)

    comp = [-1] * n
    comps = []

    def dfs_comp(v, cid):
        comp[v] = cid
        comps[cid].append(v)

        for to, eid in g[v]:
            if is_bridge[eid]:
                continue
            if comp[to] == -1:
                dfs_comp(to, cid)

    cid = 0
    for i in range(n):
        if comp[i] == -1:
            comps.append([])
            dfs_comp(i, cid)
            cid += 1

    if cid == 1:
        return "0"

    deg = [0] * cid

    for eid, (u, v) in enumerate(edges):
        cu = comp[u]
        cv = comp[v]

        if cu != cv:
            deg[cu] += 1
            deg[cv] += 1

    leaves = []

    for i in range(cid):
        if deg[i] == 1:
            leaves.append(i)

    k = len(leaves)

    if k == 2:
        pairings = [(leaves[0], leaves[1])]
    else:
        pairings = []

        half = (k + 1) // 2

        for i in range(k // 2):
            pairings.append((leaves[i], leaves[i + half]))

        if k % 2 == 1:
            pairings.append((leaves[0], leaves[half - 1]))

    ans = []

    for a, b in pairings:
        found = False

        for u in comps[a]:
            for v in comps[b]:
                if not adj[u][v]:
                    ans.append((u + 1, v + 1))
                    adj[u][v] = adj[v][u] = True
                    found = True
                    break
            if found:
                break

        if not found:
            return "-1"

    out = [str(len(ans))]
    for u, v in ans:
        out.append(f"{u} {v}")

    return "\n".join(out)

# provided sample
assert run(
"""4 3
1 2
2 3
3 4
"""
).startswith("1")

# already 2-edge-connected
assert run(
"""3 3
1 2
2 3
3 1
"""
) == "0"

# impossible case
assert run(
"""2 1
1 2
"""
) == "-1"

# simple chain
assert run(
"""5 4
1 2
2 3
3 4
4 5
"""
).startswith("1")

# graph with one cyclic component and tail
assert run(
"""6 6
1 2
2 3
3 1
3 4
4 5
5 6
"""
).startswith("1")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị tam giác |`0`| Đã kết nối 2 cạnh | 
| Hai đỉnh có một cạnh |`-1`| Các cạnh song song bị cấm | 
| Dây chuyền dài | Thêm một cạnh | Tất cả các cây cầu được sửa chữa trong một chu kỳ | 
| Chu kỳ có đường dẫn đính kèm | Thêm một cạnh | Công trình nén thành phần | 

## Vỏ cạnh 

Xét trường hợp không thể xảy ra:```
2 1
1 2
```Biểu đồ chứa một cây cầu. Cây cầu có hai lá nên về mặt lý thuyết, thêm một cạnh sẽ cố định được đồ thị. Nhưng lợi thế duy nhất có thể`(1,2)`đã tồn tại rồi. 

Thuật toán tạo thành hai thành phần`{1}`Và`{2}`. Trong quá trình tìm kiếm một điểm không cạnh giữa chúng, mọi cặp ứng cử viên đều đã liền kề nhau. Tìm kiếm không thành công và thuật toán in chính xác`-1`. 

Bây giờ hãy xem xét một biểu đồ đã hợp lệ:```
4 4
1 2
2 3
3 4
4 1
```Thuật toán của Tarjan không tìm thấy cầu nối nào. Tất cả các đỉnh vẫn còn trong một thành phần sau khi nén. Vì số lượng thành phần là một nên thuật toán sẽ in ngay lập tức`0`. 

Cuối cùng, hãy xem xét một biểu đồ có nhiều đường dẫn chia sẻ cầu nối:```
7 6
1 2
2 3
3 4
4 5
3 6
6 7
```Cây cầu đã có lá`{1,5,7}`. 

Thuật toán ghép cặp lá trên cây. Một cạnh được thêm vào sẽ loại bỏ tất cả các cầu trên một đường dẫn và một cạnh khác sẽ loại bỏ đường cầu còn lại. Điều này xác nhận tính bất biến rằng mỗi cạnh mới sẽ sửa chữa toàn bộ đường đi của cây giữa các điểm cuối của nó chứ không chỉ một cây cầu duy nhất.
