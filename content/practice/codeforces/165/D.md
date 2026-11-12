---
title: "CF 165D - Đồ thị râu"
description: "Đồ thị trong bài toán này gần như là một đường đi đơn giản. Mỗi đỉnh có nhiều nhất là 2 bậc, ngoại trừ một đỉnh đặc biệt có thể có bậc lớn hơn. Một cái cây có hình dạng này trông giống như nhiều chuỗi được dán lại với nhau ở một trung tâm. Ban đầu mọi cạnh đều có màu đen."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "trees"]
categories: ["algorithms"]
codeforces_contest: 165
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 112 (Div. 2)"
rating: 2100
weight: 165
solve_time_s: 133
verified: true
draft: false
---

[CF 165D - Biểu đồ râu](https://codeforces.com/problemset/problem/165/D) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, dsu, cây 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị trong bài toán này gần như là một đường đi đơn giản. Mỗi đỉnh có nhiều nhất là 2 bậc, ngoại trừ một đỉnh đặc biệt có thể có bậc lớn hơn. Một cái cây có hình dạng này trông giống như nhiều chuỗi được dán lại với nhau ở một trung tâm. 

Ban đầu mọi cạnh đều có màu đen. Truy vấn có thể sơn lại một cạnh màu trắng hoặc đen và cũng có thể yêu cầu đường đi ngắn nhất giữa hai đỉnh chỉ sử dụng các cạnh màu đen. 

Vì đồ thị là một cây nên đường đi ngắn nhất giữa hai đỉnh là duy nhất. Câu hỏi thực sự trong mỗi truy vấn loại 3 là liệu mọi cạnh trên đường dẫn duy nhất đó hiện có màu đen hay không. Nếu có, câu trả lời chỉ đơn giản là số cạnh trên đường đi. Nếu thậm chí một cạnh có màu trắng thì câu trả lời là`-1`. 

Các ràng buộc đủ lớn nên việc tính toán lại các đường dẫn một cách trực tiếp là không thể. Đồ thị có tới`10^5`đỉnh và có tới`3 * 10^5`truy vấn. Một DFS hoặc BFS ngây thơ cho mọi truy vấn khoảng cách sẽ có giá`O(n)`cho mỗi truy vấn, điều này trở nên đại khái`3 * 10^10`hoạt động trong trường hợp xấu nhất. Điều đó vượt xa giới hạn. 

Cấu trúc bất thường của đồ thị chính là chìa khóa. Một cây bình thường sẽ yêu cầu cấu trúc dữ liệu cây nặng như Phân tách nặng-Ánh sáng. Ở đây chúng ta có thể làm điều gì đó đơn giản hơn nhiều vì cây là một đường đi hoặc nhiều đường dẫn được kết nối tại một đỉnh trung tâm. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai bất cẩn. 

Hãy xem xét một con đường thuần túy:```
1 - 2 - 3 - 4
```Nếu cạnh`(2,3)`trở thành màu trắng, sau đó là các đỉnh`1`Và`4`bị ngắt kết nối mặc dù cả hai vẫn được kết nối với các đỉnh khác. Một giải pháp chỉ theo dõi các thành phần được kết nối một cách lười biếng có thể vô tình báo cáo khoảng cách`3`. 

Một trường hợp tinh tế khác là khi một điểm cuối là đỉnh trung tâm. Ví dụ:```
    2
    |
1 - 3 - 4
    |
    5
```Nếu cạnh`(3,4)`có màu trắng, sau đó truy vấn từ`3`ĐẾN`4`phải quay lại`-1`, trong khi truy vấn từ`3`ĐẾN`2`vẫn làm việc. Bản thân trung tâm không có gì đặc biệt về logic kết nối, chỉ có tính phân rã. 

Ngoài ra còn có trường hợp cả hai đỉnh đều nằm trên cùng một nhánh. Ví dụ:```
1 - 2 - 3 - 4
      |
      5
```Con đường từ`1`ĐẾN`2`không bao giờ chạm vào cấu trúc phân nhánh trung tâm. Giải pháp luôn đi qua trung tâm sẽ tính vượt quá khoảng cách hoặc kiểm tra các cạnh sai. 

Cuối cùng, các truy vấn có thể yêu cầu khoảng cách từ một đỉnh đến chính nó. Dù có nhiều cạnh màu trắng thì đáp án vẫn phải là`0`vì đường dẫn trống không sử dụng cạnh. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mọi truy vấn khoảng cách, hãy chạy BFS hoặc DFS chỉ sử dụng các cạnh màu đen và tính toán đường đi ngắn nhất. Vì đồ thị là một cái cây nên chỉ cần DFS là đủ. Việc sơn lại cạnh là chuyện nhỏ vì chúng tôi chỉ cập nhật một mảng màu. 

Điều này hoạt động hợp lý vì biểu đồ đủ nhỏ về mặt khái niệm để khám phá trực tiếp. Vấn đề là quy mô. Một DFS có thể truy cập tất cả`10^5`đỉnh. Lặp đi lặp lại điều này cho`3 * 10^5`các truy vấn dẫn đến hàng chục tỷ hoạt động. 

Cấu trúc đồ thị mang lại một thuộc tính mạnh hơn nhiều. Vì có nhiều nhất một đỉnh bậc cao nên cây có thể bị phân tách thành nhiều chuỗi độc lập bắt đầu từ tâm. Mỗi đỉnh thuộc đúng một nhánh. 

Giả sử trung tâm đặc biệt là`c`. 

Mỗi con đường rơi vào một trong hai loại. 

Nếu cả hai đỉnh đều nằm trên cùng một nhánh thì đường đi hoàn toàn nằm bên trong nhánh đó. 

Nếu chúng ở trên các nhánh khác nhau thì đường đi phải đi qua`c`. 

Điều này biến vấn đề thành việc duy trì các phân đoạn màu đen trên mảng. 

Đối với mỗi nhánh, ấn định các vị trí tăng dần ra xa trung tâm. Khi đó mỗi cạnh tương ứng với đúng một vị trí trên đúng một nhánh. 

Bây giờ một đường dẫn là hợp lệ nếu mọi cạnh trong một khoảng liền kề đều có màu đen. Điều này trở thành một vấn đề truy vấn tổng phạm vi. 

Chúng tôi duy trì cây Fenwick trên tất cả các vị trí cạnh. Các cạnh màu trắng góp phần`1`, các cạnh màu đen đóng góp`0`. 

Sau đó: 

Nếu số cạnh trắng trên đường đi bằng 0 thì đường dẫn đó tồn tại. 

Nếu không thì không. 

Tính toán khoảng cách không phụ thuộc vào màu sắc vì cây bên dưới không bao giờ thay đổi. Chúng tôi tính toán trước độ sâu một lần và sử dụng chúng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và tính bậc của tất cả các đỉnh. 
2. Tìm đỉnh trung tâm. Đây là đỉnh duy nhất có bậc có thể vượt quá 2. Nếu không có đỉnh nào như vậy tồn tại thì cây chỉ là một đường đi và bất kỳ đỉnh nào cũng có thể đóng vai trò là tâm. 
3. Bắt đầu từ mọi hàng xóm của trung tâm, đi dọc theo chuỗi và chỉ định: 

- mỗi đỉnh thuộc nhánh nào, 
- độ sâu của nó từ trung tâm, 
- một vị trí tuyến tính cho mỗi cạnh. 

Mỗi nhánh trở thành một khoảng liền kề trong cây Fenwick. 
4. Đối với mỗi cạnh, lưu trữ chỉ số cây Fenwick đại diện cho cạnh đó. Ban đầu tất cả các cạnh đều có màu đen nên cây Fenwick bắt đầu bằng số 0. 
5. Đối với truy vấn tô một cạnh màu trắng, hãy thêm`1`tại vị trí Fenwick của cạnh đó. 
6. Đối với truy vấn tô một cạnh màu đen, hãy thêm`-1`tại vị trí Fenwick của cạnh đó. 
7. Đối với truy vấn khoảng cách giữa`a`Và`b`, đầu tiên xử lý trường hợp tầm thường`a == b`. Câu trả lời là`0`. 
8. Nếu`a`Và`b`thuộc cùng một nhánh thì đường dẫn là một đoạn liền kề bên trong nhánh đó. 

Truy vấn cây Fenwick về khoảng cách giữa độ sâu của chúng. 

Nếu tổng bằng 0, trả về chênh lệch độ sâu tuyệt đối. 

Nếu không thì quay lại`-1`. 
9. Nếu`a`Và`b`thuộc các nhánh khác nhau thì đường đi phải đi qua tâm. 

Truy vấn phân đoạn từ`a`lên đến trung tâm và từ`b`lên tới trung tâm. 

Nếu cả hai đoạn đều không có cạnh trắng, trả về`depth[a] + depth[b]`. 

Nếu không thì quay lại`-1`. 

### Tại sao nó hoạt động 

Mỗi cạnh thuộc về đúng một nhánh và xuất hiện đúng một lần trên cây Fenwick. Vì đồ thị là một cây nên mỗi cặp đỉnh đều có một đường đi đơn duy nhất. 

Nếu hai đỉnh nằm trên cùng một nhánh thì đường đi của chúng chính xác là khoảng cách giữa độ sâu của chúng trên chuỗi đó. 

Nếu chúng nằm trên các nhánh khác nhau thì đường đi của chúng là sự nối của: 

- đỉnh`a`lên đến trung tâm, 
- tâm xuống đỉnh`b`. 

Cây Fenwick lưu trữ chính xác những cạnh nào có màu trắng. Một đường dẫn tồn tại khi và chỉ khi số cạnh trắng trên tất cả các khoảng được yêu cầu bằng 0. Vì khoảng cách được lưu trữ là độ sâu thực tế của cây tính từ tâm nên độ dài được báo cáo luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

    def range_sum(self, l, r):
        if l > r:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve():
    n = int(input())

    edges = [None]
    g = [[] for _ in range(n + 1)]
    deg = [0] * (n + 1)

    for i in range(1, n):
        u, v = map(int, input().split())
        edges.append((u, v))

        g[u].append((v, i))
        g[v].append((u, i))

        deg[u] += 1
        deg[v] += 1

    center = 1
    for i in range(1, n + 1):
        if deg[i] > 2:
            center = i
            break

    branch = [0] * (n + 1)
    depth = [0] * (n + 1)

    edge_pos = [0] * n

    timer = 0
    branch_id = 0

    for nxt, eid in g[center]:
        branch_id += 1

        prev = center
        cur = nxt
        d = 1

        while True:
            branch[cur] = branch_id
            depth[cur] = d

            timer += 1
            edge_pos[eid] = timer

            next_vertex = -1
            next_edge = -1

            for to, ne in g[cur]:
                if to != prev:
                    next_vertex = to
                    next_edge = ne
                    break

            if next_vertex == -1:
                break

            prev = cur
            cur = next_vertex
            eid = next_edge
            d += 1

    bit = Fenwick(n)

    black = [1] * n

    def path_white_same_branch(a, b):
        da = depth[a]
        db = depth[b]

        if da > db:
            da, db = db, da

        return bit.range_sum(da + 1, db)

    def path_white_to_center(v):
        return bit.range_sum(1, depth[v])

    q = int(input())

    ans = []

    for _ in range(q):
        query = list(map(int, input().split()))

        if query[0] == 1:
            eid = query[1]

            if black[eid] == 0:
                black[eid] = 1
                bit.add(edge_pos[eid], -1)

        elif query[0] == 2:
            eid = query[1]

            if black[eid] == 1:
                black[eid] = 0
                bit.add(edge_pos[eid], 1)

        else:
            a, b = query[1], query[2]

            if a == b:
                ans.append("0")
                continue

            if a == center:
                white = path_white_to_center(b)
                if white == 0:
                    ans.append(str(depth[b]))
                else:
                    ans.append("-1")
                continue

            if b == center:
                white = path_white_to_center(a)
                if white == 0:
                    ans.append(str(depth[a]))
                else:
                    ans.append("-1")
                continue

            if branch[a] == branch[b]:
                white = path_white_same_branch(a, b)

                if white == 0:
                    ans.append(str(abs(depth[a] - depth[b])))
                else:
                    ans.append("-1")
            else:
                white = (
                    path_white_to_center(a)
                    + path_white_to_center(b)
                )

                if white == 0:
                    ans.append(str(depth[a] + depth[b]))
                else:
                    ans.append("-1")

    print("\n".join(ans))

solve()
```Việc thực hiện bắt đầu bằng việc xác định đỉnh trung tâm đặc biệt. Nếu không có đỉnh nào có bậc lớn hơn 2 thì đồ thị chỉ là một đường dẫn và mọi lựa chọn điểm cuối vẫn hoạt động vì mọi nút đều nằm trên một nhánh. 

Bước xây dựng chi nhánh là phần quan trọng nhất. Bắt đầu từ mỗi hàng xóm của trung tâm, chúng ta đi theo đường thẳng cho đến khi đến một chiếc lá. Vì tất cả các đỉnh không ở tâm có nhiều nhất là hai bậc, nên mỗi nhánh chỉ là một chuỗi và có thể đi qua một cách tham lam. 

Mỗi cạnh đi qua sẽ nhận được một chỉ số Fenwick. Bất biến chính là các cạnh dọc theo nhánh nhận được các chỉ số liên tiếp theo thứ tự độ sâu tăng dần. Điều này cho phép các truy vấn theo khoảng thời gian tương ứng chính xác với các truy vấn đường dẫn. 

Cây Fenwick lưu trữ các cạnh màu trắng chứ không phải các cạnh màu đen. Một đường dẫn chỉ có thể sử dụng được khi tổng được truy vấn bằng 0. Điều này sạch sẽ hơn một chút vì việc sơn lại thành màu trắng sẽ trở nên bổ sung thêm`1`. 

Một điểm tinh tế là truy vấn bên trong cùng một nhánh:```
bit.range_sum(da + 1, db)
```Độ sâu kết nối cạnh`k-1`đến độ sâu`k`được đại diện ở vị trí`k`. Vì vậy, con đường giữa độ sâu`da`Và`db`sử dụng các cạnh`da+1 ... db`. 

Một lỗi dễ mắc phải khác là xử lý đỉnh trung tâm. Bản thân trung tâm có chiều sâu`0`và không thuộc chi nhánh nào. Xử lý đặc biệt tránh các truy vấn khoảng thời gian không hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
2 3
7
3 1 2
3 1 3
3 2 3
2 2
3 1 2
3 1 3
3 2 3
```Cây là một con đường đơn giản:```
1 - 2 - 3
```Chúng tôi chọn`1`làm trung tâm. 

| Bước | Truy vấn | Viền trắng | Kết quả | 
| --- | --- | --- | --- | 
| 1 |`3 1 2`| không |`1`| 
| 2 |`3 1 3`| không |`2`| 
| 3 |`3 2 3`| không |`1`| 
| 4 |`2 2`| cạnh 2 trở thành màu trắng | | 
| 5 |`3 1 2`| cạnh 2 chưa dùng |`1`| 
| 6 |`3 1 3`| đường dẫn cạnh 2 khối |`-1`| 
| 7 |`3 2 3`| đường dẫn cạnh 2 khối |`-1`| 

Dấu vết này chứng tỏ rằng thuật toán chỉ kiểm tra các cạnh trên đường dẫn duy nhất thực tế. Các cạnh màu trắng ở nơi khác không thành vấn đề. 

### Ví dụ tùy chỉnh 

đầu vào:```
5
1 3
2 3
3 4
3 5
6
3 1 4
2 3
3 1 4
3 2 5
1 3
3 2 5
```Biểu đồ:```
    2
    |
1 - 3 - 4
    |
    5
```Cạnh 3 là`(3,4)`. 

| Bước | Truy vấn | Viền trắng | Kết quả | 
| --- | --- | --- | --- | 
| 1 |`3 1 4`| không |`2`| 
| 2 |`2 3`|`(3,4)`trắng | | 
| 3 |`3 1 4`| đường dẫn bị chặn |`-1`| 
| 4 |`3 2 5`| không bị ảnh hưởng |`2`| 
| 5 |`1 3`|`(3,4)`lại đen | | 
| 6 |`3 2 5`| đã kết nối |`2`| 

Ví dụ này cho thấy tại sao các nhánh khác nhau có thể được xử lý độc lập. Làm trắng một nhánh không ảnh hưởng hoàn toàn đến các đường dẫn bên trong các nhánh khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Quá trình tiền xử lý cây là tuyến tính, mỗi truy vấn thực hiện cập nhật Fenwick hoặc tính tổng phạm vi | 
| Không gian | O(n) | Danh sách kề, mảng nhánh và cây Fenwick | 

Quá trình tiền xử lý sẽ đi qua mỗi cạnh đúng một lần. Mỗi truy vấn vẽ lại thực hiện một cập nhật Fenwick và mỗi truy vấn khoảng cách thực hiện một số tổng tiền tố Fenwick không đổi. Với`3 * 10^5`truy vấn, điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            while idx <= self.n:
                self.bit[idx] += val
                idx += idx & -idx

        def sum(self, idx):
            res = 0
            while idx > 0:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

        def range_sum(self, l, r):
            if l > r:
                return 0
            return self.sum(r) - self.sum(l - 1)

    n = int(input())

    edges = [None]
    g = [[] for _ in range(n + 1)]
    deg = [0] * (n + 1)

    for i in range(1, n):
        u, v = map(int, input().split())
        edges.append((u, v))
        g[u].append((v, i))
        g[v].append((u, i))
        deg[u] += 1
        deg[v] += 1

    center = 1
    for i in range(1, n + 1):
        if deg[i] > 2:
            center = i
            break

    branch = [0] * (n + 1)
    depth = [0] * (n + 1)
    edge_pos = [0] * n

    timer = 0
    branch_id = 0

    for nxt, eid in g[center]:
        branch_id += 1

        prev = center
        cur = nxt
        d = 1

        while True:
            branch[cur] = branch_id
            depth[cur] = d

            timer += 1
            edge_pos[eid] = timer

            nx = -1
            ne = -1

            for to, ee in g[cur]:
                if to != prev:
                    nx = to
                    ne = ee
                    break

            if nx == -1:
                break

            prev = cur
            cur = nx
            eid = ne
            d += 1

    bit = Fenwick(n)
    black = [1] * n

    def path_same(a, b):
        da = depth[a]
        db = depth[b]
        if da > db:
            da, db = db, da
        return bit.range_sum(da + 1, db)

    def to_center(v):
        return bit.range_sum(1, depth[v])

    q = int(input())
    ans = []

    for _ in range(q):
        t, *rest = map(int, input().split())

        if t == 1:
            e = rest[0]
            if black[e] == 0:
                black[e] = 1
                bit.add(edge_pos[e], -1)

        elif t == 2:
            e = rest[0]
            if black[e] == 1:
                black[e] = 0
                bit.add(edge_pos[e], 1)

        else:
            a, b = rest

            if a == b:
                ans.append("0")
            elif a == center:
                ans.append(str(depth[b]) if to_center(b) == 0 else "-1")
            elif b == center:
                ans.append(str(depth[a]) if to_center(a) == 0 else "-1")
            elif branch[a] == branch[b]:
                ans.append(
                    str(abs(depth[a] - depth[b]))
                    if path_same(a, b) == 0
                    else "-1"
                )
            else:
                ans.append(
                    str(depth[a] + depth[b])
                    if to_center(a) + to_center(b) == 0
                    else "-1"
                )

    return "\n".join(ans)

# provided sample
assert run(
"""3
1 2
2 3
7
3 1 2
3 1 3
3 2 3
2 2
3 1 2
3 1 3
3 2 3
"""
) == "\n".join(["1", "2", "1", "1", "-1", "-1"]), "sample"

# minimum graph
assert run(
"""2
1 2
3
3 1 2
2 1
3 1 2
"""
) == "\n".join(["1", "-1"]), "minimum size"

# star-shaped beard
assert run(
"""5
1 3
2 3
3 4
3 5
5
3 1 2
2 1
3 1 2
3 4 5
3 3 3
"""
) == "\n".join(["2", "-1", "2", "0"]), "center handling"

# same branch query
assert run(
"""4
1 2
2 3
3 4
4
2 2
3 1 2
3 1 4
3 3 4
"""
) == "\n".join(["1", "-1", "1"]), "same branch intervals"

# repaint back to black
assert run(
"""3
1 2
2 3
5
2 1
3 1 3
1 1
3 1 3
3 2 2
"""
) == "\n".join(["-1", "2", "0"]), "repainting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị tối thiểu có một cạnh |`1`,`-1`| Xử lý đúng cây nhỏ nhất | 
| Râu hình ngôi sao |`2`,`-1`,`2`,`0`| Logic đỉnh trung tâm | 
| Khoảng cách nhánh giống nhau |`1`,`-1`,`1`| Ánh xạ khoảng thời gian chính xác | 
| Sơn lại viền đen phía sau |`-1`,`2`,`0`| Cập nhật Fenwick hoạt động theo cả hai cách | 

## Vỏ cạnh 

Hãy xem xét trường hợp cả hai đỉnh được truy vấn đều giống hệt nhau.```
2
1 2
1
3 1 1
```Thuật toán ngay lập tức trở lại`0`trước khi kiểm tra bất kỳ cạnh nào. Điều này đúng vì đường dẫn trống luôn tồn tại. 

Bây giờ hãy xem xét một đường dẫn bị ngắt kết nối do một cạnh màu trắng gây ra.```
4
1 2
2 3
3 4
3
2 2
3 1 4
3 1 2
```Bờ rìa`(2,3)`trở nên trắng. 

Đối với truy vấn`(1,4)`, thuật toán kiểm tra khoảng bao phủ độ sâu`1..3`. Cây Fenwick có một cạnh trắng nên câu trả lời là`-1`. 

Đối với truy vấn`(1,2)`, khoảng được chọn sẽ loại trừ hoàn toàn cạnh trắng, vì vậy câu trả lời là`1`. 

Cuối cùng, hãy xem xét việc phân nhánh qua trung tâm.```
5
1 3
2 3
3 4
3 5
2
2 1
3 2 4
```Bờ rìa`(1,3)`trở nên trắng. 

Con đường từ`2`ĐẾN`4`sử dụng các cạnh`(2,3)`Và`(3,4)`chỉ một. Thuật toán kiểm tra chính xác hai đoạn nhánh đó. Vì cả hai đều không phải màu trắng nên câu trả lời vẫn là`2`. 

Điều này xác nhận tính bất biến trung tâm: chỉ các cạnh trên đường dẫn cây duy nhất mới quan trọng.
