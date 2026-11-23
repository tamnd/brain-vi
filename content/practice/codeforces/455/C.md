---
title: "CF 455C - Nền văn minh"
description: "Biểu đồ bao gồm các thành phố được kết nối bằng đường bộ. Điều kiện là giữa hai thành phố được kết nối bất kỳ có chính xác một đường đi đơn nghĩa là mọi thành phần được kết nối là một cây. Vì biểu đồ có thể bị ngắt kết nối nên toàn bộ biểu đồ là một khu rừng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "ternary-search", "trees"]
categories: ["algorithms"]
codeforces_contest: 455
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 260 (Div. 1)"
rating: 2100
weight: 455
solve_time_s: 121
verified: true
draft: false
---

[CF 455C - Nền văn minh](https://codeforces.com/problemset/problem/455/C) 

**Đánh giá:** 2100 
**Thẻ:** dfs và tương tự, dp, dsu, tìm kiếm bậc ba, cây 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Biểu đồ bao gồm các thành phố được kết nối bằng đường bộ. Điều kiện là giữa hai thành phố được kết nối bất kỳ có chính xác một đường đi đơn nghĩa là mọi thành phần được kết nối là một cây. Vì biểu đồ có thể bị ngắt kết nối nên toàn bộ biểu đồ là một khu rừng. 

Hai loại hoạt động phải được xử lý trực tuyến. 

Thao tác đầu tiên yêu cầu đường kính của thành phần được kết nối chứa một thành phố nhất định. Ở đây, đường kính là chiều dài của con đường dài nhất bên trong cây đó. 

Hoạt động thứ hai hợp nhất hai thành phần được kết nối bằng cách thêm chính xác một cạnh mới giữa một số thành phố trong thành phần đầu tiên và một số thành phố trong thành phần thứ hai. Chúng ta có thể tự do lựa chọn các điểm cuối của cạnh mới này, nhưng chúng ta phải chọn chúng sao cho đường kính của cây thu được càng nhỏ càng tốt. 

Đồ thị ban đầu chứa tối đa$3 \cdot 10^5$đỉnh và lên đến$3 \cdot 10^5$truy vấn. Bất kỳ giải pháp nào tính toán lại đường kính từ đầu sau mỗi lần hợp nhất đều quá chậm. Ngay cả một DFS duy nhất trên toàn bộ thành phần cũng có giá$O(n)$và thực hiện điều đó đối với nhiều truy vấn sẽ dẫn đến hàng chục tỷ thao tác trong trường hợp xấu nhất. 

Thách thức chính là các thành phần thay đổi theo thời gian. Chúng ta cần một cách để duy trì đường kính của từng thành phần được kết nối một cách hiệu quả khi các thành phần được hợp nhất. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét hai đỉnh cô lập:```
2 0 2
2 1 2
1 1
```Sau khi hợp nhất, cây thu được có một cạnh nên đường kính là 1. Giải pháp giả sử đường kính không bao giờ tăng từ 0 sẽ là sai. 

Xem xét việc hợp nhất các thành phần đã thuộc về cùng một cây:```
3 2 2
1 2
2 3
2 1 3
1 1
```Hoạt động hợp nhất sẽ không làm gì cả. Câu trả lời vẫn là 2. Việc vô tình áp dụng công thức cập nhật đường kính sẽ làm hỏng giá trị được lưu trữ. 

Một trường hợp khó phát hiện khác là khi cạnh mới tối ưu không được gắn vào điểm cuối của đường kính. 

Giả sử chúng ta hợp nhất hai đường có chiều dài 4. Việc kết nối các điểm cuối đường kính tạo ra đường kính 9, điều này thật khủng khiếp. Lựa chọn tối ưu là nối tâm của cả hai cây, tạo ra đường kính 5. Bất kỳ phương pháp nào chỉ nhìn vào điểm cuối đường kính sẽ thất bại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ duy trì rõ ràng mọi thành phần được kết nối. Bất cứ khi nào một truy vấn yêu cầu đường kính, chúng ta có thể chạy hai lần duyệt DFS để tính toán đường kính đó. Bất cứ khi nào hai thành phần hợp nhất, chúng ta thêm cạnh và tiếp tục. 

Thủ thuật đường kính cây tiêu chuẩn hoạt động vì chạy DFS từ một nút tùy ý sẽ tìm thấy một điểm cuối đường kính và chạy lại DFS từ điểm cuối đó sẽ tìm thấy chiều dài đường kính. Vấn đề là chi phí. Việc tính toán đường kính đơn yêu cầu phải duyệt qua toàn bộ thành phần. Với tối đa$3 \cdot 10^5$truy vấn, trường hợp xấu nhất sẽ trở nên đại khái$O(nq)$, vượt xa giới hạn. 

Quan sát làm thay đổi mọi thứ là hoạt động hợp nhất không yêu cầu chúng ta xây dựng cạnh tối ưu thực tế. Nó chỉ yêu cầu chúng ta duy trì đường kính sau khi nối hai cây theo cách tốt nhất có thể. 

Giả sử hai cây có đường kính$d_1$Và$d_2$. 

Đối với cây có đường kính$d$, bán kính của nó là:$$r = \left\lceil \frac{d}{2} \right\rceil$$Bán kính là khoảng cách tối đa tối thiểu có thể từ tâm được chọn đến tất cả các đỉnh. 

Khi hợp nhất hai cây một cách tối ưu, chúng ta kết nối tâm của chúng. Bất kỳ sự lựa chọn nào khác chỉ có thể làm cho con đường dài nhất trở nên lớn hơn. 

Đường kính kết quả trở thành:$$\max\left(d_1,\ d_2,\ r_1 + r_2 + 1\right)$$Hai thuật ngữ đầu tiên bao gồm các đường dẫn hoàn toàn bên trong một cây ban đầu. Số hạng thứ ba bao gồm các đường đi qua cạnh mới. 

Điều này có nghĩa là chúng ta chỉ cần một thông tin cho mỗi thành phần được kết nối: đường kính của nó. 

Vì các thành phần được kết nối được hợp nhất một cách linh hoạt, nên Disjoint Set Union hoàn toàn phù hợp. Mỗi gốc DSU lưu trữ đường kính của thành phần của nó. Thao tác hợp nhất kết hợp hai bộ DSU và cập nhật đường kính được lưu trữ bằng công thức trên. 

Nhiệm vụ còn lại là tính đường kính ban đầu của mỗi cây. Vì biểu đồ là một khu rừng nên chúng ta có thể xử lý từng thành phần được kết nối một lần bằng cách sử dụng phép tính đường kính hai DFS tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng rừng từ các cạnh đầu vào. 
2. Sử dụng DFS để xác định mọi thành phần được kết nối. 
3. Đối với mỗi bộ phận, hãy tính đường kính của nó bằng cách sử dụng hai đường ngang. 

Đầu tiên hãy tìm đỉnh xa nhất tính từ điểm bắt đầu tùy ý. Sau đó bắt đầu từ đỉnh xa nhất đó và tìm lại khoảng cách tối đa. Khoảng cách đó chính là đường kính của cây. 
4. Khởi tạo cấu trúc DSU. 

Mỗi thành phần được kết nối sẽ trở thành một bộ DSU và phần gốc lưu trữ đường kính của thành phần đó. 
5. Đối với truy vấn loại 1, hãy tìm đại diện DSU của thành phố được yêu cầu và xuất ra đường kính được lưu trữ. 
6. Đối với truy vấn loại 2, hãy tìm đại diện DSU của cả hai thành phố. 
7. Nếu cả hai người đại diện đều giống hệt nhau thì không làm gì cả. 

Các thành phố đã thuộc về cùng một thành phần. 
8. Ngược lại tính:$$r_1=\left\lceil\frac{d_1}{2}\right\rceil,\quad r_2=\left\lceil\frac{d_2}{2}\right\rceil$$9. Tính đường kính mới:$$d=\max(d_1,d_2,r_1+r_2+1)$$10. Nối hai bộ DSU và lưu đường kính mới này vào gốc kết quả. 

### Tại sao nó hoạt động 

Mỗi bộ DSU luôn đại diện chính xác cho một thành phần được kết nối của nhóm hiện tại. 

Ban đầu, đường kính được tính toán chính xác bằng thuật toán đường kính cây tiêu chuẩn. Đối với các hoạt động hợp nhất, việc kết nối các tâm sẽ giảm thiểu khoảng cách tối đa có thể đi qua cạnh mới. Đường đi dài nhất trong cây được hợp nhất phải nằm hoàn toàn bên trong cây đầu tiên, nằm hoàn toàn bên trong cây thứ hai hoặc băng qua cạnh mới. Ba khả năng đó tương ứng chính xác với:$$d_1,\quad d_2,\quad r_1+r_2+1$$Lấy mức tối đa của chúng sẽ cho đường kính thực của cây được hợp nhất tối ưu. Vì mọi phép hợp đều lưu trữ giá trị này nên tính bất biến mà mỗi gốc DSU lưu trữ đường kính chính xác vẫn đúng sau mỗi thao tác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())

    g = [[] for _ in range(n)]

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union_initial(a, b):
        ra = find(a)
        rb = find(b)

        if ra == rb:
            return

        if size[ra] < size[rb]:
            ra, rb = rb, ra

        parent[rb] = ra
        size[ra] += size[rb]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1

        g[a].append(b)
        g[b].append(a)

        union_initial(a, b)

    diameter = [0] * n

    visited = [False] * n

    def bfs(start):
        dist = [-1] * n
        dist[start] = 0

        dq = deque([start])
        farthest = start

        while dq:
            v = dq.popleft()

            if dist[v] > dist[farthest]:
                farthest = v

            for to in g[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    dq.append(to)

        return farthest, dist[farthest]

    for v in range(n):
        if visited[v]:
            continue

        stack = [v]
        component = []
        visited[v] = True

        while stack:
            cur = stack.pop()
            component.append(cur)

            for to in g[cur]:
                if not visited[to]:
                    visited[to] = True
                    stack.append(to)

        start = component[0]
        endpoint, _ = bfs(start)
        _, diam = bfs(endpoint)

        root = find(start)
        diameter[root] = diam

    def merge(a, b):
        ra = find(a)
        rb = find(b)

        if ra == rb:
            return

        d1 = diameter[ra]
        d2 = diameter[rb]

        new_d = max(
            d1,
            d2,
            (d1 + 1) // 2 + (d2 + 1) // 2 + 1
        )

        if size[ra] < size[rb]:
            ra, rb = rb, ra

        parent[rb] = ra
        size[ra] += size[rb]
        diameter[ra] = new_d

    ans = []

    for _ in range(q):
        query = list(map(int, input().split()))

        if query[0] == 1:
            x = query[1] - 1
            ans.append(str(diameter[find(x)]))
        else:
            x = query[1] - 1
            y = query[2] - 1
            merge(x, y)

    sys.stdout.write("\n".join(ans))

solve()
```Biểu đồ được xây dựng một lần ngay từ đầu vì các điểm cuối thực tế được chọn trong quá trình hợp nhất không bao giờ quan trọng. Chỉ có đường kính kết quả là quan trọng. 

Việc xây dựng DSU đầu tiên nhóm các đỉnh theo rừng ban đầu. Sau đó, đường kính của mọi thành phần được kết nối sẽ được tính chính xác một lần bằng kỹ thuật BFS kép cổ điển. 

Mảng đường kính được lập chỉ mục bởi các gốc DSU. Bất cứ khi nào hai thành phần hợp nhất, đường kính mới được tính trực tiếp từ đường kính cũ bằng công thức bán kính. Không cần duyệt đồ thị. 

Một lỗi phổ biến là sử dụng`d // 2`thay vì`(d + 1) // 2`. Bán kính của cây có đường kính 5 là 3 chứ không phải 2. Việc phân chia trần là cần thiết. 

Một sai lầm dễ mắc phải khác là cập nhật đường kính trước khi kiểm tra xem hai đỉnh có thuộc cùng một tập DSU hay không. Công thức hợp nhất chỉ hợp lệ cho các thành phần riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 0 6
2 1 2
2 3 4
2 5 6
2 3 2
2 5 3
1 1
```Trạng thái ban đầu: 

| Thành phần | Đường kính | 
| --- | --- | 
| {1} | 0 | 
| {2} | 0 | 
| {3} | 0 | 
| {4} | 0 | 
| {5} | 0 | 
| {6} | 0 | 

Xử lý truy vấn: 

| Truy vấn | Hợp nhất các thành phần | Đường kính mới | 
| --- | --- | --- | 
| 2 1 2 | {1} + {2} | 1 | 
| 2 3 4 | {3} + {4} | 1 | 
| 2 5 6 | {5} + {6} | 1 | 
| 2 3 2 | đường kính 1 + đường kính 1 | 3 | 
| 2 5 3 | đường kính 1 + đường kính 3 | 4 | 
| 1 1 | trả lời | 4 | 

Đầu ra:```
4
```Ví dụ này cho thấy sự hợp nhất lặp đi lặp lại của các cây nhỏ. Chỉ riêng công thức cập nhật đường kính là đủ, không cần duyệt đồ thị sau khi khởi tạo. 

### Ví dụ 2 

đầu vào:```
5 4 3
1 2
2 3
3 4
4 5
1 1
2 2 5
1 3
```Thành phần ban đầu: 

| Thành phần | Đường kính | 
| --- | --- | 
| {1,2,3,4,5} | 4 | 

Xử lý: 

| Truy vấn | Kết quả | 
| --- | --- | 
| 1 1 | 4 | 
| 2 2 5 | cùng thành phần | 
| 1 3 | 4 | 

Đầu ra:```
4
4
```Điều này chứng tỏ rằng các yêu cầu hợp nhất bên trong cùng một thành phần phải bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) α(n)) | Quá trình xử lý đồ thị ban đầu là tuyến tính, các hoạt động DSU được khấu hao nghịch đảo Ackermann | 
| Không gian | O(n) | Đồ thị, mảng DSU và bộ lưu trữ phụ trợ | 

Rừng chứa nhiều nhất$3 \cdot 10^5$các đỉnh và các cạnh. Tiền xử lý tuyến tính cộng với các hoạt động DSU gần như liên tục dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m, q = map(int, input().split())

    g = [[] for _ in range(n)]

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def unite(a, b):
        ra = find(a)
        rb = find(b)
        if ra == rb:
            return
        if size[ra] < size[rb]:
            ra, rb = rb, ra
        parent[rb] = ra
        size[ra] += size[rb]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        g[b].append(a)
        unite(a, b)

    diameter = [0] * n
    vis = [False] * n

    def bfs(s):
        dist = [-1] * n
        dist[s] = 0
        dq = deque([s])
        far = s

        while dq:
            v = dq.popleft()
            if dist[v] > dist[far]:
                far = v

            for to in g[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    dq.append(to)

        return far, dist[far]

    for i in range(n):
        if vis[i]:
            continue

        stack = [i]
        vis[i] = True
        comp = []

        while stack:
            v = stack.pop()
            comp.append(v)

            for to in g[v]:
                if not vis[to]:
                    vis[to] = True
                    stack.append(to)

        a, _ = bfs(comp[0])
        _, d = bfs(a)

        diameter[find(comp[0])] = d

    def merge(a, b):
        ra = find(a)
        rb = find(b)

        if ra == rb:
            return

        nd = max(
            diameter[ra],
            diameter[rb],
            (diameter[ra] + 1) // 2 + (diameter[rb] + 1) // 2 + 1
        )

        if size[ra] < size[rb]:
            ra, rb = rb, ra

        parent[rb] = ra
        size[ra] += size[rb]
        diameter[ra] = nd

    out = []

    for _ in range(q):
        qry = list(map(int, input().split()))

        if qry[0] == 1:
            out.append(str(diameter[find(qry[1] - 1)]))
        else:
            merge(qry[1] - 1, qry[2] - 1)

    return "\n".join(out)

# provided sample
assert run(
"""6 0 6
2 1 2
2 3 4
2 5 6
2 3 2
2 5 3
1 1
"""
) == "4"

# single isolated node
assert run(
"""1 0 1
1 1
"""
) == "0"

# merge two isolated vertices
assert run(
"""2 0 2
2 1 2
1 1
"""
) == "1"

# same component merge should do nothing
assert run(
"""3 2 2
1 2
2 3
2 1 3
1 2
"""
) == "2"

# chain of length four
assert run(
"""5 4 1
1 2
2 3
3 4
4 5
1 3
"""
) == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh cô lập đơn | 0 | Kích thước biểu đồ tối thiểu | 
| Hai đỉnh cô lập được hợp nhất | 1 | Đường kính tăng trưởng từ 0 | 
| Hợp nhất bên trong cùng một thành phần | 2 | Xử lý DSU đúng cách | 
| Đường đi có độ dài 4 | 4 | Tính toán đường kính ban đầu | 
| Trường hợp mẫu | 4 | Quy trình làm việc đầy đủ | 

## Vỏ cạnh 

Xét hai đỉnh cô lập:```
2 0 2
2 1 2
1 1
```Cả hai thành phần đều bắt đầu bằng đường kính 0. Công thức hợp nhất cho:$$\max(0,0,0+0+1)=1$$Truy vấn trả về chính xác:```
1
```Hãy xem xét việc hợp nhất bên trong cùng một thành phần:```
3 2 2
1 2
2 3
2 1 3
1 2
```Các đại diện của DSU đều bình đẳng nên hoạt động hợp nhất sẽ kết thúc ngay lập tức. Đường kính được lưu trữ vẫn là 2 và câu trả lời là:```
2
```Hãy xem xét hai đường dẫn có độ dài 4 được hợp nhất. Mỗi cây có đường kính 4 và bán kính 2. 

Công thức cho:$$\max(4,4,2+2+1)=5$$Các tâm kết nối đạt được đường kính 5. Việc kết nối các điểm cuối sẽ tạo ra đường kính 9. Thuật toán luôn lưu trữ giá trị tối ưu vì nó dựa vào công thức dựa trên bán kính thay vì bất kỳ lựa chọn cạnh cụ thể nào. 

Cuối cùng, các đỉnh cô lập được xử lý một cách tự nhiên. Đường kính của chúng bằng 0, bán kính của chúng cũng bằng 0 và tất cả các công thức vẫn hợp lệ mà không có trường hợp đặc biệt nào.
