---
title: "CF 187C - Trí nhớ yếu"
description: "Chúng ta có một đồ thị vô hướng biểu thị các giao lộ và đường bên trong công viên. Một số giao lộ có tình nguyện viên. PMP bắt đầu tại ngã tư s và bến xe ở ngã tư t. PMP có bộ nhớ yếu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu"]
categories: ["algorithms"]
codeforces_contest: 187
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 119 (Div. 1)"
rating: 2000
weight: 187
solve_time_s: 132
verified: true
draft: false
---

[CF 187C - Bộ nhớ yếu](https://codeforces.com/problemset/problem/187/C) 

**Xếp hạng:** 2000 
**Thẻ:** dfs và tương tự, dsu 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng biểu thị các giao lộ và đường bên trong công viên. Một số giao lộ có tình nguyện viên. PMP bắt đầu ở ngã tư`s`, và bến xe buýt nằm ở ngã tư`t`. 

PMP có bộ nhớ yếu. Sau khi nói chuyện với một tình nguyện viên, anh ta chỉ có thể nhớ được một con đường chứa nhiều nhất`q`giao lộ không bao gồm giao lộ hiện tại. Vì chiều dài con đường được đo bằng đường, điều này có nghĩa là một tình nguyện viên chỉ có thể hướng dẫn anh ta dọc theo con đường nhiều nhất là`q`các cạnh. 

Một tình nguyện viên có thể trực tiếp hướng dẫn PMP đến`t`, hoặc hướng dẫn anh ta đến một tình nguyện viên khác. Các tình nguyện viên luôn hợp tác tối ưu. Chúng ta phải tìm mức tối thiểu`q`sao cho PMP được đảm bảo đạt được`t`. 

Đồ thị có tới`10^5`đỉnh và`2 * 10^5`các cạnh. Bất kỳ giải pháp nào chạy BFS từ mọi tình nguyện viên một cách độc lập đều trở nên nguy hiểm. Nếu như`k`cũng gần đây`10^5`, sau đó một`O(k * (n + m))`cách tiếp cận vượt xa giới hạn. 

Cấu trúc của vấn đề rất tinh tế. PMP không cần phải ghi nhớ toàn bộ lộ trình từ`s`ĐẾN`t`. Anh ta chỉ cần di chuyển giữa các tình nguyện viên, và cuối cùng từ tình nguyện viên này sang tình nguyện viên khác.`t`. Điều đó biến vấn đề thành khả năng kết nối giữa các nút đặc biệt dưới ngưỡng khoảng cách. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét biểu đồ này:```
1 -- 2 -- 3
```Chỉ tình nguyện tại`1`, bắt đầu`s = 1`, mục tiêu`t = 3`. 

Con đường ngắn nhất từ`1`ĐẾN`3`có chiều dài`2`, vậy câu trả lời là`2`. Một giải pháp bất cẩn chỉ kiểm tra quá trình chuyển đổi từ tình nguyện viên sang tình nguyện viên sẽ kết luận sai rằng việc đạt được`t`là không thể bởi vì`t`không phải là tình nguyện viên 

Một trường hợp phức tạp khác là đồ thị bị ngắt kết nối. 

đầu vào:```
4 2 1
1
1 2
3 4
1 4
```Đầu ra đúng:```
-1
```Thậm chí lớn tùy ý`q`không thể giúp được vì không có con đường từ`1`ĐẾN`4`. 

Một trường hợp tinh tế hơn xuất hiện khi sử dụng tình nguyện viên trung gian làm giảm bộ nhớ cần thiết. 

đầu vào:```
5 4 2
1 3
1 2
2 3
3 4
4 5
1 5
```Con đường ngắn nhất từ`1`ĐẾN`5`có chiều dài`4`, nhưng câu trả lời là`2`. 

PMP có thể di chuyển:`1 -> 3 -> 5`Đoạn đầu tiên có độ dài`2`, và đoạn thứ hai cũng có độ dài`2`. 

Bất kỳ giải pháp nào chỉ tính toán đường đi ngắn nhất từ`s`ĐẾN`t`bỏ lỡ cấu trúc thực sự của vấn đề. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Giả sử chúng ta sửa một số giá trị ứng viên`q`. Chúng ta có thể xây dựng một biểu đồ mới có các đỉnh là tình nguyện viên cộng với mục tiêu`t`. 

Hai đỉnh được kết nối nếu khoảng cách đường đi ngắn nhất của chúng trong đồ thị ban đầu lớn nhất là`q`. 

Thì PMP thành công đúng lúc`s`có thể đạt được`t`bên trong biểu đồ nén này. 

Để kiểm tra một đơn`q`, chúng tôi có thể chạy BFS từ mọi tình nguyện viên và từ`t`, tính toán tất cả các khoảng cách theo cặp lên đến`q`. Nếu có`k`tình nguyện viên, việc này tốn khoảng`O((k + 1)(n + m))`. 

Cố gắng hết sức có thể`q`các giá trị bằng tìm kiếm nhị phân còn khiến mọi việc trở nên tồi tệ hơn. Trong trường hợp xấu nhất:```
O(log n * k * (n + m))
```Với mọi giới hạn gần`10^5`, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào cách các thành phần đồ thị hợp nhất khi chúng ta dần dần cho phép khoảng cách lớn hơn. 

Giả sử chúng ta biết tất cả các cặp đỉnh có khoảng cách đường đi ngắn nhất tối đa là`d`. Sau đó, mọi cặp như vậy sẽ được kết nối hiệu quả với PMP khi`q = d`. 

Điều này gợi ý khoảng cách xử lý theo thứ tự tăng dần, hợp nhất các vùng biểu đồ khi chúng có thể tiếp cận được trong ngưỡng hiện tại. 

Thủ thuật kinh điển cho loại vấn đề này là BFS đa nguồn kết hợp với DSU. 

Hãy tưởng tượng tất cả các tình nguyện viên mở rộng đồng thời. Mỗi đỉnh đều ghi nhớ tình nguyện viên nào đến được đỉnh đó trước. Khi sóng BFS từ hai tình nguyện viên khác nhau gặp nhau ở một rìa, chúng tôi phát hiện ra khoảng cách ngắn nhất giữa hai tình nguyện viên đó. 

Chính xác hơn, nếu:```
dist[u] + dist[v] + 1 = D
```Và`u`Và`v`thuộc về các nguồn tình nguyện viên khác nhau thì khoảng cách ngắn nhất giữa các tình nguyện viên đó chính xác là`D`. 

Điều này tạo ra các lợi thế có trọng số giữa các thành phần tình nguyện. Sau khi sắp xếp các cạnh này theo khoảng cách, chúng ta có thể hợp chúng theo thứ tự tăng dần. Thời điểm đầu tiên khi thành phần chứa`s`được kết nối với thành phần có thể tiếp cận trực tiếp`t`đưa ra câu trả lời. 

Vấn đề trở nên rất giống với thuật toán Kruskal trên biểu đồ ẩn về các tình nguyện viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(log n * k * (n + m)) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log (n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS đa nguồn bắt đầu từ tất cả các đỉnh tình nguyện cùng một lúc. 

Mỗi đỉnh lưu trữ:`owner[v]`, nguồn tình nguyện đã đạt được nó đầu tiên.`dist[v]`, khoảng cách từ nguồn đó. 

Điều này phân chia biểu đồ thành các vùng giống như Voronoi xung quanh các tình nguyện viên. 
2. Trong BFS, bất cứ khi nào chúng ta kiểm tra một cạnh`(u, v)`có điểm cuối thuộc về các chủ sở hữu khác nhau, chúng tôi phát hiện ra mối liên hệ ứng cử viên giữa hai nguồn tình nguyện. 

Con đường ngắn nhất giữa hai nguồn đó là:```
dist[u] + dist[v] + 1
```bởi vì đường dẫn đi từ một nguồn tới`u`, vượt qua rìa, sau đó đến nguồn khác từ`v`. 
3. Lưu trữ tất cả các cạnh ứng viên như vậy giữa các nguồn tình nguyện. 

Nhiều cạnh có thể kết nối cùng một cặp nguồn. Điều đó không sao cả vì quá trình xử lý theo kiểu Kruskal tự nhiên sẽ bỏ qua những bản sao tồi tệ hơn. 
4. Chúng ta cũng cần kết nối tình nguyện viên với mục tiêu`t`. 

Chạy BFS bình thường từ`t`để tính khoảng cách ngắn nhất từ ​​mọi đỉnh tới`t`. 
5. Đối với mọi nguồn tình nguyện`x`, ghi`dt[x]`, khoảng cách ngắn nhất từ ​​tình nguyện viên đó đến`t`. 

Nếu PMP có bộ nhớ`q >= dt[x]`, sau đó tình nguyện`x`có thể trực tiếp hướng dẫn anh ta đến xe buýt. 
6. Sắp xếp tất cả các cạnh kết nối tình nguyện viên theo khoảng cách. 
7. Tạo DSU trên các nguồn tình nguyện cộng thêm một nút bổ sung đại diện cho mục tiêu. 
8. Xử lý các cạnh ứng cử viên theo thứ tự khoảng cách tăng dần. 

Khi xử lý một cạnh có trọng lượng`w`, hợp nhất hai thành phần tình nguyện. 
9. Sắp xếp riêng các tình nguyện viên theo khoảng cách của họ tới`t`. 

Bất cứ khi nào`dt[x] <= current_weight`, tình nguyện viên công đoàn`x`với nút mục tiêu đặc biệt. 
10. Trọng số đầu tiên mà tại đó nút tình nguyện bắt đầu và nút mục tiêu được kết nối là giá trị tối thiểu`q`. 
11. Nếu kết nối không bao giờ xảy ra, hãy xuất`-1`. 

### Tại sao nó hoạt động 

BFS đa nguồn đảm bảo rằng mọi cuộc họp của hai khu vực BFS đều tạo ra khoảng cách ngắn nhất thực sự giữa các nguồn tình nguyện tương ứng. Đây là thuộc tính tương tự được sử dụng trong phân rã Voronoi đường đi ngắn nhất. 

Xử lý các cạnh theo thứ tự tăng dần mô phỏng tăng dần giới hạn bộ nhớ cho phép`q`. 

Tại bất kỳ thời điểm nào trong quá trình xử lý, hai tình nguyện viên thuộc cùng một thành phần DSU chính xác khi PMP có thể di chuyển giữa họ bằng cách sử dụng các đoạn có độ dài tối đa ngưỡng hiện tại. 

Tương tự, khi một tình nguyện viên được kết nối với nút mục tiêu đặc biệt, PMP có thể di chuyển trực tiếp từ tình nguyện viên đó đến`t`. 

Ngưỡng đầu tiên nơi`s`trở nên kết nối với`t`chính xác là giới hạn bộ nhớ khả thi nhỏ nhất. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]

def solve():
    n, m, k = map(int, input().split())

    volunteers = list(map(int, input().split()))

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    s, t = map(int, input().split())

    volunteer_id = {}
    for i, v in enumerate(volunteers):
        volunteer_id[v] = i

    # multi-source BFS from volunteers
    owner = [-1] * (n + 1)
    dist = [-1] * (n + 1)

    q = deque()

    for i, v in enumerate(volunteers):
        owner[v] = i
        dist[v] = 0
        q.append(v)

    edges = []

    while q:
        u = q.popleft()

        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                owner[v] = owner[u]
                q.append(v)
            elif owner[v] != owner[u]:
                w = dist[u] + dist[v] + 1
                edges.append((w, owner[u], owner[v]))

    # BFS from target
    dt = [-1] * (n + 1)

    q = deque([t])
    dt[t] = 0

    while q:
        u = q.popleft()

        for v in graph[u]:
            if dt[v] == -1:
                dt[v] = dt[u] + 1
                q.append(v)

    start_id = volunteer_id[s]

    if dt[s] == -1:
        print(-1)
        return

    target_node = k

    volunteer_to_t = []

    for i, v in enumerate(volunteers):
        volunteer_to_t.append((dt[v], i))

    volunteer_to_t.sort()
    edges.sort()

    dsu = DSU(k + 1)

    ptr = 0
    ans = -1

    all_weights = []

    for w, _, _ in edges:
        all_weights.append(w)

    for w, _ in volunteer_to_t:
        all_weights.append(w)

    all_weights = sorted(set(all_weights))

    edge_ptr = 0

    for current in all_weights:
        while edge_ptr < len(edges) and edges[edge_ptr][0] <= current:
            _, a, b = edges[edge_ptr]
            dsu.union(a, b)
            edge_ptr += 1

        while ptr < len(volunteer_to_t) and volunteer_to_t[ptr][0] <= current:
            _, x = volunteer_to_t[ptr]
            dsu.union(x, target_node)
            ptr += 1

        if dsu.find(start_id) == dsu.find(target_node):
            ans = current
            break

    print(ans)

solve()
```BFS đầu tiên phát triển đồng thời từ mọi tình nguyện viên. Mỗi nút ghi lại tình nguyện viên nào đã đến đích đầu tiên và ở khoảng cách nào. Khi hai vùng chạm nhau trên một cạnh, chúng ta sẽ có được kết nối đường đi ngắn nhất giữa các tình nguyện viên tương ứng. 

BFS thứ hai tính toán khoảng cách ngắn nhất thông thường tới mục tiêu. Điều này cho chúng tôi biết khi nào mỗi tình nguyện viên có thể trực tiếp hướng dẫn PMP`t`. 

Giai đoạn DSU hoạt động giống như thuật toán Kruskal. Khi ngưỡng tăng lên, sẽ có nhiều cặp tình nguyện viên hơn có thể đi qua được. Khi tình nguyện viên bắt đầu được kết nối với nút mục tiêu đặc biệt, ngưỡng hiện tại là câu trả lời. 

Một chi tiết tinh tế là xử lý các mục tiêu không thể tiếp cận. Nếu như`dt[s] == -1`, thì ngay cả chuyển động không hạn chế cũng không thể đạt tới`t`, vì vậy chúng tôi in ngay`-1`. 

Một chi tiết triển khai quan trọng khác là sử dụng`<= current`trong khi xử lý các cạnh và các liên kết tình nguyện đến mục tiêu. Tất cả các kết nối có trọng số tối đa ngưỡng hiện tại phải được kích hoạt trước khi kiểm tra kết nối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 6 3
1 3 6
1 2
2 3
4 2
5 6
4 5
3 4
1 6
```Kết quả BFS đa nguồn: 

| Đỉnh | Chủ sở hữu tình nguyện viên | Khoảng cách | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 1 | 1 | 
| 3 | 3 | 0 | 
| 4 | 3 | 1 | 
| 5 | 6 | 1 | 
| 6 | 6 | 0 | 

Các cạnh tình nguyện được tạo ra: 

| Tình nguyện viên | Cân nặng | 
| --- | --- | 
| 1 và 3 | 2 | 
| 3 và 6 | 3 | 

Khoảng cách tới mục tiêu`6`: 

| Tình nguyện viên | Khoảng cách đến 6 | 
| --- | --- | 
| 1 | 5 | 
| 3 | 3 | 
| 6 | 0 | 

Xử lý DSU: 

| q hiện tại | Các cạnh tình nguyện tích cực | Tình nguyện viên đã kết nối với t | Can s Reach t | 
| --- | --- | --- | --- | 
| 0 | không | 6 | Không | 
| 2 | 1-3 | 6 | Không | 
| 3 | 3-6 | 3,6 | Có | 

Trả lời:```
3
```Ví dụ này chứng minh tại sao tình nguyện viên ở trình độ trung cấp lại quan trọng. PMP không bao giờ ghi nhớ toàn bộ đường dẫn có độ dài-5 từ`1`ĐẾN`6`. 

### Ví dụ tùy chỉnh 

đầu vào:```
5 4 2
1 3
1 2
2 3
3 4
4 5
1 5
```Các khu vực tình nguyện: 

| Đỉnh | Chủ sở hữu | Khoảng cách | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 1 | 1 | 
| 3 | 3 | 0 | 
| 4 | 3 | 1 | 
| 5 | 3 | 2 | 

Góc tình nguyện: 

| Tình nguyện viên | Cân nặng | 
| --- | --- | 
| 1 và 3 | 2 | 

Khoảng cách tới mục tiêu`5`: 

| Tình nguyện viên | Khoảng cách | 
| --- | --- | 
| 1 | 4 | 
| 3 | 2 | 

Sự phát triển của DSU: 

| q hiện tại | Các cạnh hoạt động | Kết nối mục tiêu | Có thể truy cập | 
| --- | --- | --- | --- | 
| 2 | 1-3 | 3 | Có | 

Trả lời:```
2
```Dấu vết này xác nhận rằng giải pháp tối ưu có thể sử dụng chuyển giao tự nguyện để giảm bộ nhớ cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log (n + m)) | BFS là tuyến tính, việc sắp xếp các cạnh ứng cử viên chiếm ưu thế | 
| Không gian | O(n + m) | Đồ thị, mảng BFS, DSU và lưu trữ cạnh | 

Kích thước đồ thị tối đa là`2 * 10^5`các cạnh, do đó việc truyền tải BFS tuyến tính dễ dàng đủ nhanh. Sắp xếp đại khái`O(m)`các cạnh ứng cử viên cũng thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)

            if a == b:
                return

            if self.size[a] < self.size[b]:
                a, b = b, a

            self.parent[b] = a
            self.size[a] += self.size[b]

    n, m, k = map(int, input().split())
    volunteers = list(map(int, input().split()))

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    s, t = map(int, input().split())

    volunteer_id = {}
    for i, v in enumerate(volunteers):
        volunteer_id[v] = i

    owner = [-1] * (n + 1)
    dist = [-1] * (n + 1)

    q = deque()

    for i, v in enumerate(volunteers):
        owner[v] = i
        dist[v] = 0
        q.append(v)

    edges = []

    while q:
        u = q.popleft()

        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                owner[v] = owner[u]
                q.append(v)
            elif owner[v] != owner[u]:
                edges.append((dist[u] + dist[v] + 1, owner[u], owner[v]))

    dt = [-1] * (n + 1)

    q = deque([t])
    dt[t] = 0

    while q:
        u = q.popleft()

        for v in graph[u]:
            if dt[v] == -1:
                dt[v] = dt[u] + 1
                q.append(v)

    if dt[s] == -1:
        return "-1"

    volunteer_to_t = []

    for i, v in enumerate(volunteers):
        volunteer_to_t.append((dt[v], i))

    volunteer_to_t.sort()
    edges.sort()

    dsu = DSU(k + 1)

    target = k
    start = volunteer_id[s]

    vals = []

    for w, _, _ in edges:
        vals.append(w)

    for w, _ in volunteer_to_t:
        vals.append(w)

    vals = sorted(set(vals))

    ep = 0
    tp = 0

    for cur in vals:
        while ep < len(edges) and edges[ep][0] <= cur:
            _, a, b = edges[ep]
            dsu.union(a, b)
            ep += 1

        while tp < len(volunteer_to_t) and volunteer_to_t[tp][0] <= cur:
            _, x = volunteer_to_t[tp]
            dsu.union(x, target)
            tp += 1

        if dsu.find(start) == dsu.find(target):
            return str(cur)

    return "-1"

# provided sample
assert run(
"""6 6 3
1 3 6
1 2
2 3
4 2
5 6
4 5
3 4
1 6
"""
) == "3", "sample"

# disconnected graph
assert run(
"""4 2 1
1
1 2
3 4
1 4
"""
) == "-1", "unreachable target"

# direct short path
assert run(
"""3 2 1
1
1 2
2 3
1 3
"""
) == "2", "single volunteer"

# intermediate volunteer reduces answer
assert run(
"""5 4 2
1 3
1 2
2 3
3 4
4 5
1 5
"""
) == "2", "handoff between volunteers"

# already adjacent to target
assert run(
"""2 1 1
1
1 2
1 2
"""
) == "1", "minimum nonzero answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị bị ngắt kết nối |`-1`| Xử lý đúng đắn các mục tiêu không thể tiếp cận | 
| Con đường tình nguyện đơn |`2`| Hướng dẫn trực tiếp tình nguyện viên đến mục tiêu | 
| Trường hợp tình nguyện viên trung cấp |`2`| Định tuyến tình nguyện viên nhiều bước | 
| Đồ thị hai nút |`1`| Câu trả lời hợp lệ nhỏ nhất có thể | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
4 2 1
1
1 2
3 4
1 4
```BFS từ`t = 4`không bao giờ đến được nút`1`, Vì thế`dt[1] = -1`. 

Thuật toán ngay lập tức trở lại`-1`. Không cần xử lý DSU vì ngay cả việc di chuyển không hạn chế cũng không thể kết nối các thành phần. 

Bây giờ hãy xem xét trường hợp mục tiêu không phải là tình nguyện viên:```
3 2 1
1
1 2
2 3
1 3
```Tình nguyện viên duy nhất đang ở nút`1`. BFS từ`t = 3`cho khoảng cách`2`dành cho tình nguyện viên`1`. 

Không tồn tại ranh giới giữa tình nguyện viên với tình nguyện viên, nhưng thuật toán kết nối chính xác tình nguyện viên`1`trực tiếp tới nút đích khi`q = 2`. 

Cuối cùng, hãy xem xét trường hợp chuyển tiếp quan trọng:```
5 4 2
1 3
1 2
2 3
3 4
4 5
1 5
```Con đường ngắn nhất từ`1`ĐẾN`5`có chiều dài`4`, nhưng tình nguyện`3`đóng vai trò là điểm kiểm tra trung gian. 

DSU lần đầu tiên kích hoạt lợi thế giữa các tình nguyện viên`1`Và`3`ở trọng lượng`2`. Tình nguyện viên`3`cũng kết nối trực tiếp với mục tiêu ở trọng lượng`2`. 

Vào lúc này,`s`trở nên kết nối với`t`, vậy câu trả lời là`2`.
