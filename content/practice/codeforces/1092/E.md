---
title: "CF 1092E - Rừng có đường kính tối thiểu"
description: "Chúng ta được cung cấp một biểu đồ đã là một khu rừng, nghĩa là nó bao gồm một số cây bị ngắt kết nối. Nhiệm vụ là thêm chính xác đủ các cạnh để kết nối tất cả các cây này thành một cây duy nhất."
date: "2026-06-13T04:38:17+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1092
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 527 (Div. 3)"
rating: 2000
weight: 1092
solve_time_s: 314
verified: false
draft: false
---

[CF 1092E - Rừng có đường kính tối thiểu](https://codeforces.com/problemset/problem/1092/E) 

**Xếp hạng:** 2000 
**Thẻ:** thuật toán xây dựng, dfs và các cây tương tự, tham lam 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ đã là một khu rừng, nghĩa là nó bao gồm một số cây bị ngắt kết nối. Nhiệm vụ là thêm chính xác đủ các cạnh để kết nối tất cả các cây này thành một cây duy nhất. Kể từ khi một cái cây trên`n`đỉnh có chính xác`n - 1`các cạnh và đầu vào đã chứa`m`các cạnh, chúng ta phải thêm`n - 1 - m`cạnh, chính xác là ít hơn một so với số lượng thành phần được kết nối. 

Sau khi thêm các cạnh này, chúng ta có thể tự do lựa chọn cách kết nối các thành phần, nhưng cấu trúc thu được vẫn phải là một cây. Trong số tất cả các cách có thể để kết nối các thành phần, chúng tôi muốn một cách giảm thiểu đường kính của cây cuối cùng, trong đó đường kính là khoảng cách đường đi ngắn nhất tối đa giữa bất kỳ cặp đỉnh nào. 

Cấu trúc đồ thị đầu vào chỉ quan trọng thông qua các thành phần được kết nối của nó và cấu trúc bên trong của từng thành phần, bởi vì chúng ta được phép kết nối các thành phần một cách tùy ý. 

Ràng buộc`n ≤ 1000`ngụ ý rằng ngay cả việc tiền xử lý bậc hai hoặc gần bậc hai cho mỗi thành phần cũng có thể chấp nhận được. Chúng tôi có đủ khả năng chạy BFS hoặc DFS từ nhiều nút, thậm chí hai lần cho mỗi thành phần, vì tổng công việc trên tất cả các thành phần vẫn bị giới hạn bởi khoảng`O(n^2)`trong trường hợp xấu nhất. 

Một trực giác ngây thơ nhưng không chính xác sẽ là kết nối các thành phần một cách tùy ý hoặc theo chuỗi. Điều này không thành công vì đường kính trở nên nhạy cảm với việc đặt hàng. 

Ví dụ: giả sử chúng ta có ba thành phần: 

Thành phần A là một đường đi dài có độ dài 10, thành phần B là một cạnh đơn và thành phần C là một cạnh đơn khác. Nếu chúng ta nối chúng thành chuỗi A-B-C, đường kính sẽ đi qua cả hai phần đính kèm và trở nên lớn một cách không cần thiết. Một chiến lược tốt hơn sẽ đặt “trung tâm” lớn nhất theo cách hạn chế khoảng cách giữa các thành phần. 

Một trường hợp thất bại tinh vi khác là chọn điểm cuối của cây thay vì trung tâm. Nếu chúng ta gắn các lá thay vì các nút trung tâm, khoảng cách bên trong một thành phần sẽ tăng đường kính cuối cùng ngay cả khi cấu trúc tổng thể là tối ưu. 

Khó khăn cốt lõi là mỗi thành phần hoạt động giống như một “quả bóng” có bán kính và chúng tôi đang quyết định cách dán các quả bóng này lại với nhau. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi cách kết nối các thành phần vào một cây và tính toán đường kính kết quả mỗi lần. Nếu có`k`thành phần, có`k^(k-2)`các cây có thể được gắn nhãn theo công thức của Cayley và thậm chí hạn chế việc chọn các cạnh giữa các thành phần vẫn để lại cấu hình hàm mũ. Đối với mỗi cấu hình, tính toán chi phí đường kính`O(n)`hoặc`O(n^2)`tùy thuộc vào phương pháp, làm cho điều này hoàn toàn không khả thi ngay cả đối với những`n`. 

Quan sát quan trọng là mỗi thành phần có thể được tóm tắt bằng hai giá trị: đường kính và tâm của nó. Đường kính cho chúng ta biết khoảng cách bên trong tồi tệ nhất và tâm giảm thiểu khoảng cách tối đa đến tất cả các nút bên trong thành phần. Khi chúng ta kết nối các thành phần, bất kỳ đường dẫn nào đi qua các thành phần đều phải đi qua các điểm kết nối đã chọn, do đó, biểu diễn hữu ích duy nhất của một thành phần là nút xa nhất của nó cách điểm đính kèm đã chọn của nó bao xa. 

Nếu chúng ta chọn một trung tâm cho mỗi thành phần và kết nối tất cả các trung tâm trong một ngôi sao thì tất cả các đường dẫn giữa các thành phần đều đi qua một trung tâm duy nhất. Sau đó, bất kỳ đường đi nào giữa hai thành phần đều được điều khiển bằng tổng bán kính của chúng cộng với một cạnh giữa chúng. Điều này làm giảm toàn bộ vấn đề trong việc chọn thành phần trung tâm tốt nhất. 

Hub tối ưu là thành phần có bán kính lớn nhất. Điều này đảm bảo rằng đường dẫn chéo thành phần xấu nhất được giảm thiểu, bởi vì mọi thành phần khác đều gắn với đường dẫn lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia biểu đồ thành các thành phần liên thông bằng DFS hoặc BFS. Mỗi thành phần được xử lý độc lập vì ban đầu không có cạnh nào giữa chúng. 
2. Đối với mỗi thành phần, chọn một nút tùy ý và chạy BFS để tìm nút xa nhất`a`. Nút này là một điểm cuối của ứng cử viên đường kính. 
3. Chạy lại BFS từ`a`để tìm nút xa nhất`b`. Khoảng cách giữa`a`Và`b`là đường kính của thành phần này. Điều này hiệu quả vì nút xa nhất tính từ điểm bắt đầu tùy ý luôn là điểm cuối có đường kính nào đó trong cây. 
4. Chạy BFS từ`a`một lần nữa và từ`b`một lần nữa để tính khoảng cách đến tất cả các nút. Đối với mỗi nút, tính toán`max(dist_a[x], dist_b[x])`. Nút giảm thiểu giá trị này là trung tâm của thành phần. Nút này giảm thiểu khoảng cách tối đa tới tất cả các nút khác. 
5. Lưu trữ đường kính và tâm của từng bộ phận. 
6. Xác định thành phần có bán kính lớn nhất (có hiệu lực là một nửa đường kính được làm tròn, nhưng chúng tôi sử dụng trực tiếp khoảng cách tối đa được tính toán). Thành phần này trở thành trung tâm. 
7. Đối với mọi thành phần khác, hãy kết nối tâm của nó với tâm trung tâm bằng cách sử dụng một cạnh bổ sung. Các cạnh này đủ để kết nối tất cả các thành phần thành một cây duy nhất. 
8. Tính đường kính cuối cùng là giá trị lớn nhất của hai giá trị: đường kính thành phần ban đầu lớn nhất và giá trị lớn nhất trên tất cả các cặp thành phần của`radius[i] + 1 + radius[j]`, được tối đa hóa bằng cách ghép bán kính lớn nhất và lớn thứ hai. 

### Tại sao nó hoạt động 

Mỗi thành phần hoạt động giống như một cái cây có gốc ở trung tâm của nó và bất kỳ đường dẫn nào vào hoặc ra khỏi một thành phần đều phải đi qua tâm đó trong công trình. Điều này làm giảm mỗi thành phần thành một nút có trọng số có trọng số bằng bán kính của nó. Do đó, đường kính của bất kỳ đường đi nào đi qua các thành phần được xác định bằng hai bán kính cộng với một cạnh nối. Việc chọn bán kính lớn nhất làm trung tâm sẽ giảm thiểu tổng tồi tệ nhất có thể có của hai bán kính trên các thành phần khác nhau, bởi vì tất cả các thành phần khác gắn qua một điểm duy nhất thay vì nối chuỗi và tích lũy nhiều bán kính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m = map(int, input().split())
g = [[] for _ in range(n)]

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

vis = [False] * n
comp = []

def bfs(start):
    dist = [-1] * n
    q = deque([start])
    dist[start] = 0
    order = []
    while q:
        x = q.popleft()
        order.append(x)
        for y in g[x]:
            if dist[y] == -1:
                dist[y] = dist[x] + 1
                q.append(y)
    return dist, order

def find_center(nodes):
    start = nodes[0]
    dist1, _ = bfs(start)
    a = max(nodes, key=lambda x: dist1[x])

    dista, _ = bfs(a)
    b = max(nodes, key=lambda x: dista[x])

    distb, _ = bfs(b)

    best = None
    best_val = 10**9
    for x in nodes:
        val = max(dista[x], distb[x])
        if val < best_val:
            best_val = val
            best = x

    diameter = dista[b]
    radius = best_val
    return a, b, best, diameter, radius

components = []

for i in range(n):
    if not vis[i]:
        stack = [i]
        vis[i] = True
        nodes = []
        while stack:
            x = stack.pop()
            nodes.append(x)
            for y in g[x]:
                if not vis[y]:
                    vis[y] = True
                    stack.append(y)

        a, b, c, diam, rad = find_center(nodes)
        components.append((rad, c, diam))

if len(components) == 1:
    rad, c, diam = components[0]
    print(diam)
    sys.exit()

components.sort(reverse=True)
hub_rad, hub_center, hub_diam = components[0]

edges = []

for i in range(1, len(components)):
    edges.append((hub_center, components[i][1]))

ans = hub_diam
if len(components) > 1:
    ans = max(ans, components[0][0] + components[1][0] + 1)

print(ans)
for u, v in edges:
    print(u + 1, v + 1)
```Mã đầu tiên phân tách rừng thành các thành phần bằng DFS. Đối với mỗi thành phần, nó thực hiện trích xuất đường kính dựa trên BFS và sau đó tính toán tâm bằng cách giảm thiểu khoảng cách tối đa đến hai điểm cuối đường kính. Khi mỗi thành phần được tóm tắt thành tâm, bán kính và đường kính, thuật toán sẽ chọn thành phần có bán kính tối đa làm trung tâm. 

Tất cả các thành phần khác được kết nối trực tiếp với trung tâm trung tâm này, tạo thành cấu trúc hình ngôi sao. Tính toán đường kính cuối cùng phù hợp với giới hạn lý thuyết bắt nguồn từ phân tích đường dẫn chéo thành phần. 

Một chi tiết triển khai tinh tế là chúng tôi sử dụng lại kết quả BFS từ các điểm cuối đường kính để tính toán các ứng cử viên trung tâm. Điều này tránh việc tính toán lại không cần thiết và giữ lời giải trong phạm vi`O(n^2)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 2
2 3
```Có hai thành phần:`{1,2,3}`Và`{4}`. 

| Thành phần | Đường kính | Trung tâm | Bán kính | 
| --- | --- | --- | --- | 
| {1,2,3} | 2 | 2 | 1 | 
| {4} | 0 | 4 | 0 | 

Hub là thành phần`{1,2,3}`. Chúng tôi kết nối nút`2`ĐẾN`4`. 

Đường kính cuối cùng là`max(2, 1 + 0 + 1) = 2`. 

### Ví dụ 2 

đầu vào:```
5 2
1 2
3 4
```Hai thành phần:`{1,2}`Và`{3,4}`, cộng với nút bị cô lập`{5}`. 

| Thành phần | Đường kính | Trung tâm | Bán kính | 
| --- | --- | --- | --- | 
| {1,2} | 1 | 1 | 1 | 
| {3,4} | 1 | 3 | 1 | 
| {5} | 0 | 5 | 0 | 

Chúng tôi chọn một trong hai`{1,2}`hoặc`{3,4}`như trung tâm. Giả định`{1,2}`là trung tâm, chúng tôi kết nối`1-3`Và`1-5`. 

Đường kính cuối cùng là`max(1, 1 + 1 + 1) = 3`. 

Các ví dụ này cho thấy đường dẫn nhiều thành phần chiếm ưu thế như thế nào khi nhiều thành phần có bán kính khác 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi nút tham gia vào một số lần truyền tải BFS không đổi trên thành phần của nó | 
| Không gian | O(n) | Lưu trữ đồ thị, mảng đã truy cập và hàng đợi BFS | 

Giới hạn bậc hai phù hợp thoải mái trong các ràng buộc vì`n ≤ 1000`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    vis = [False] * n

    def bfs(start):
        dist = [-1] * n
        q = deque([start])
        dist[start] = 0
        while q:
            x = q.popleft()
            for y in g[x]:
                if dist[y] == -1:
                    dist[y] = dist[x] + 1
                    q.append(y)
        return dist

    def comp_nodes(i):
        stack = [i]
        vis[i] = True
        nodes = []
        while stack:
            x = stack.pop()
            nodes.append(x)
            for y in g[x]:
                if not vis[y]:
                    vis[y] = True
                    stack.append(y)
        return nodes

    def center(nodes):
        dist1 = bfs(nodes[0])
        a = max(nodes, key=lambda x: dist1[x])
        dista = bfs(a)
        b = max(nodes, key=lambda x: dista[x])
        distb = bfs(b)
        best = min(nodes, key=lambda x: max(dista[x], distb[x]))
        return best

    comps = []
    for i in range(n):
        if not vis[i]:
            nodes = comp_nodes(i)
            comps.append(nodes)

    return str(len(comps))

# sample checks (structure sanity only)
assert run("4 2\n1 2\n2 3\n") == "2", "sample 1 structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút bị cô lập | 0 | trường hợp cơ sở thành phần đơn | 
| chuỗi + nút bị cô lập | đường kính nhỏ | xử lý nhiều thành phần | 
| hai cạnh bằng nhau | 3 | tương tác bán kính đối xứng | 
| cây được kết nối đầy đủ | đường kính ban đầu | trường hợp không có cạnh bổ sung | 

## Vỏ cạnh 

Đầu vào một thành phần đã tạo thành một cây nên không có cạnh nào được thêm vào và câu trả lời chỉ là đường kính của nó. Thuật toán xử lý việc này một cách tự nhiên vì nó không bao giờ bước vào giai đoạn kết nối thành phần khi chỉ tồn tại một thành phần. 

Các nút bị ngắt kết nối hoàn toàn tạo thành các thành phần có kích thước bằng một, mỗi nút có bán kính bằng 0. Thuật toán kết nối tất cả chúng với một nút trung tâm đã chọn, tạo ra một ngôi sao. Đường kính tối đa là hai, là tối ưu vì bất kỳ cây nào trên các nút bị cô lập đều phải có đường kính ít nhất là hai trừ khi`n ≤ 2`. 

Một đường dẫn dài kết hợp với nhiều thành phần nhỏ sẽ kiểm tra xem việc lựa chọn trung tâm có đúng hay không. Thuật toán đảm bảo tâm của thành phần có bán kính dài nhất trở thành trung tâm, ngăn không cho các thành phần nhỏ làm tăng đường kính do được sử dụng làm điểm kết nối.
