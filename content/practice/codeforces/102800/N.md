---
title: "CF 102800N - Khởi động:Đường cao tốc"
description: "Chúng ta có một đồ thị thành phố có trọng số vô hướng. Bob luôn bắt đầu từ tòa nhà 1 và muốn đến tòa nhà n. Mỗi truy vấn sẽ tạm thời xóa một con phố và chúng ta phải tìm ra con đường ngắn nhất có thể sau khi con phố đó không còn khả dụng."
date: "2026-07-27T17:45:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "N"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 64
verified: true
draft: false
---

[CF 102800N - Khởi động:Đường cao tốc](https://codeforces.com/problemset/problem/102800/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng ta có một đồ thị thành phố có trọng số vô hướng. Bob luôn bắt đầu từ việc xây dựng`1`và muốn tiếp cận tòa nhà`n`. Mỗi truy vấn sẽ tạm thời xóa một con phố và chúng ta phải tìm ra con đường ngắn nhất có thể sau khi con phố đó không còn khả dụng. 

Đầu vào mô tả các cạnh của biểu đồ và sau đó hỏi nhiều truy vấn "xóa cạnh này" độc lập. Đầu ra cho mỗi truy vấn là khoảng cách ngắn nhất từ`1`ĐẾN`n`mà không sử dụng cạnh đã chọn, hoặc`-1`nếu đích đến trở nên không thể truy cập được. 

Các giới hạn là thách thức chính. Với tối đa`10^5`các tòa nhà,`2 * 10^5`đường phố và`2 * 10^5`các truy vấn, việc chạy thuật toán đường dẫn ngắn nhất cho mỗi truy vấn sẽ yêu cầu khoảng`q * (m log n)`hoạt động xung quanh`2 * 10^5 * 2 * 10^5`, vượt xa những gì có thể. Chúng ta cần xử lý trước biểu đồ một lần và trả lời từng truy vấn gần như ngay lập tức. 

Một số chi tiết có thể phá vỡ một giải pháp ngây thơ. Một biểu đồ có thể có các đường phố song song, do đó việc loại bỏ một cạnh không nhất thiết loại bỏ tất cả các kết nối giữa hai tòa nhà. 

Ví dụ:```
3 3 1
1 2 5
1 2 1
2 3 1
1
```Câu trả lời là:```
2
```Việc loại bỏ con đường đầu tiên không có gì quan trọng vì con đường thứ hai ở giữa`1`Và`2`vẫn còn có sẵn. Giải pháp chỉ lưu trữ cặp điểm cuối sẽ loại bỏ cả hai một cách không chính xác. 

Một trường hợp phức tạp khác là khi tồn tại nhiều đường đi ngắn nhất.```
4 4 1
1 2 1
2 4 1
1 3 1
3 4 1
2
```Câu trả lời là:```
2
```Cạnh bị loại bỏ thuộc về một đường đi ngắn nhất, nhưng đường đi ngắn nhất còn lại vẫn còn. Một phương pháp giả định mọi cạnh trên một đường đi ngắn nhất đã chọn là quan trọng sẽ thất bại. 

Cuối cùng, đồ thị có thể bị ngắt kết nối sau khi loại bỏ một cạnh.```
2 1 1
1 2 7
1
```Câu trả lời là:```
-1
```Thuật toán phải xử lý rõ ràng các trạng thái không thể truy cập được. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý từng truy vấn một cách độc lập. Đối với cạnh bị loại bỏ, hãy tạm thời xóa nó và chạy Dijkstra từ tòa nhà`1`để xây dựng`n`. Điều này đúng vì Dijkstra đưa ra đường đi ngắn nhất trong biểu đồ còn lại. Tuy nhiên, trong trường hợp xấu nhất điều này thực hiện`q`Dijkstra chạy, tốn khoảng`O(q * m log n)`. Với các giới hạn đã cho, đây là mức độ quá chậm. 

Quan sát quan trọng là chúng ta không cần phải giải bài toán đường đi ngắn nhất hoàn toàn khác nhau cho mỗi cạnh. Chúng ta chỉ cần quan tâm đến các cạnh có thể thực sự ảnh hưởng đến một đường đi ngắn nhất được chọn từ`1`ĐẾN`n`. 

Đầu tiên, chạy Dijkstra từ`1`và xây dựng cây đường đi ngắn nhất. Con đường từ`1`ĐẾN`n`bên trong cây này được chọn làm đường dẫn tham chiếu của chúng tôi. Bất kỳ cạnh nào không nằm trên đường dẫn này đều không thể ảnh hưởng đến câu trả lời vì đường dẫn tham chiếu vẫn tồn tại sau khi xóa nó. 

Đối với mỗi cạnh trên đường tham chiếu, chúng ta cần đường vòng tốt nhất xung quanh nó. Việc loại bỏ một cạnh của cây sẽ chia cây thành hai phần. Bất kỳ đường dẫn thay thế hợp lệ nào cũng phải vượt qua khoảng cách này bằng một cạnh khác. Mỗi cạnh không phải là cây có thể thay thế một phạm vi liên tiếp của các cạnh đường dẫn, bởi vì nó kết nối hai vùng cây. Chúng tôi có thể tính toán ứng cử viên tốt nhất cho phạm vi đó và áp dụng nó bằng cách sử dụng bản cập nhật tối thiểu của phạm vi. 

Dijkstra thứ hai chạy từ`n`cung cấp khoảng cách cần thiết sau khi đi vào phía bên kia của cạnh thay thế. Cấu trúc cây cho phép chúng ta xác định đường dẫn nào mà cạnh không phải cây có thể bỏ qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(q * m log n)`|`O(n + m)`| Quá chậm | 
| Tối ưu |`O((n + m) log n)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy Dijkstra từ nút`1`. Lưu trữ khoảng cách ngắn nhất và nút gốc của mỗi nút trong cây đường dẫn ngắn nhất. 

Các con trỏ gốc cho phép chúng ta khôi phục một tuyến đường ngắn nhất từ`1`ĐẾN`n`. 
2. Khôi phục đường dẫn cây từ`1`ĐẾN`n`. Đây là những cạnh duy nhất mà việc loại bỏ có thể thay đổi câu trả lời. 

Nếu một con phố không nằm trên con đường này thì con đường ngắn nhất ban đầu vẫn có sẵn. 
3. Chạy Dijkstra từ nút`n`để tính khoảng cách từ mỗi nút đến đích. 

Các giá trị này cho phép chúng ta đánh giá đường vòng đi vào phía đích của cạnh bị loại bỏ. 
4. Duyệt cây đường đi ngắn nhất và gán cho mỗi nút nút sâu nhất của cây`1 -> n`con đường đó là tổ tiên của nó. 

Điều này cho chúng ta biết phần nào của đường dẫn chính chứa mỗi cây con. 
5. Đối với mọi cạnh không phải là một phần của đường đi đã chọn, hãy xác định xem đường đi nào nó có thể đi qua. 

Nếu hai điểm cuối thuộc về vị trí đường dẫn`a`Và`b`, thì cạnh này có thể thay thế mọi cạnh đường dẫn giữa các vị trí đó. Chi phí ứng cử viên là khoảng cách đến một điểm cuối, trọng số cạnh và khoảng cách từ điểm cuối kia đến`n`. 
6. Áp dụng cập nhật phạm vi tối thiểu cho mọi cạnh không có đường dẫn. 

Cây phân đoạn lười lưu trữ giá trị thay thế tốt nhất cho mọi cạnh của đường dẫn. 
7. Đối với mỗi truy vấn, trả về giá trị thay thế được lưu trữ nếu cạnh bị loại bỏ nằm trên đường dẫn đã chọn. Nếu không thì trả về khoảng cách ngắn nhất ban đầu. 

Tại sao nó hoạt động: 

Hãy xem xét việc loại bỏ một cạnh của cây trên con đường ngắn nhất đã chọn. Cạnh bị loại bỏ sẽ tách đích đến khỏi điểm bắt đầu bên trong cây đường dẫn ngắn nhất. Bất kỳ tuyến đường còn lại nào cũng phải vượt qua dải phân cách này qua một cạnh khác. Mỗi cạnh giao cắt như vậy đều được xem xét trong quá trình cập nhật phạm vi và giá trị được lưu trữ chính xác là chi phí tối thiểu có thể có trong số tất cả các giao cắt. Nếu một cạnh không nằm trên đường đi đã chọn thì đường đi đó không thay đổi và vẫn tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

class SegTree:
    def __init__(self, n):
        self.n = n
        self.lazy = [INF] * (4 * n)

    def update(self, node, l, r, ql, qr, val):
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            if val < self.lazy[node]:
                self.lazy[node] = val
            return
        mid = (l + r) // 2
        self.update(node * 2, l, mid, ql, qr, val)
        self.update(node * 2 + 1, mid + 1, r, ql, qr, val)

    def query(self, node, l, r, idx, carry=INF):
        carry = min(carry, self.lazy[node])
        if l == r:
            return carry
        mid = (l + r) // 2
        if idx <= mid:
            return self.query(node * 2, l, mid, idx, carry)
        return self.query(node * 2 + 1, mid + 1, r, idx, carry)

def dijkstra(start, g):
    n = len(g) - 1
    dist = [INF] * (n + 1)
    parent = [-1] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w, idx in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                parent[v] = (u, idx)
                heapq.heappush(pq, (nd, v))
    return dist, parent

def solve():
    n, m, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    edges = []
    for i in range(1, m + 1):
        u, v, w = map(int, input().split())
        edges.append((u, v, w))
        g[u].append((v, w, i))
        g[v].append((u, w, i))

    dist1, parent = dijkstra(1, g)
    distn, _ = dijkstra(n, g)

    on_path = [False] * (m + 1)
    path_edge_pos = [-1] * (m + 1)
    path_nodes = []

    cur = n
    while cur != -1:
        path_nodes.append(cur)
        if cur == 1:
            break
        p, e = parent[cur]
        on_path[e] = True
        cur = p

    path_nodes.reverse()
    k = len(path_nodes) - 1

    pos_node = [-1] * (n + 1)
    for i, x in enumerate(path_nodes):
        pos_node[x] = i

    for i in range(k):
        p, e = parent[path_nodes[i + 1]]
        path_edge_pos[e] = i

    children = [[] for _ in range(n + 1)]
    for v in range(2, n + 1):
        if parent[v][0] != -1:
            children[parent[v][0]].append(v)

    belong = [-1] * (n + 1)

    def dfs(u, cur_pos):
        if pos_node[u] != -1:
            cur_pos = pos_node[u]
        belong[u] = cur_pos
        for v in children[u]:
            dfs(v, cur_pos)

    dfs(1, 0)

    seg = SegTree(max(1, k))

    for idx, (u, v, w) in enumerate(edges, 1):
        if on_path[idx]:
            continue
        a = belong[u]
        b = belong[v]
        if a == b:
            continue
        if a > b:
            a, b = b, a
            u, v = v, u
        val = dist1[u] + w + distn[v]
        if k > 0:
            seg.update(1, 0, k - 1, a, b - 1, val)

    ans = []
    for _ in range(q):
        e = int(input())
        if not on_path[e]:
            ans.append(str(dist1[n]))
        else:
            p = path_edge_pos[e]
            res = seg.query(1, 0, k - 1, p)
            ans.append(str(res if res < INF else -1))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Lần chạy Dijkstra đầu tiên cung cấp cả khoảng cách ngắn nhất ban đầu và cây đường đi ngắn nhất. Mảng cha được sử dụng để xác định một đường đi ngắn nhất cụ thể tới`n`; đây là lý do tại sao chỉ những cạnh đó mới cần giá trị thay thế. 

Lần chạy Dijkstra thứ hai là cần thiết vì cạnh thay thế chỉ mô tả phần giữa của tuyến đường. Chúng ta vẫn cần cách rẻ nhất để về đích từ phía có đường vòng đi vào. 

Cây phân đoạn lưu trữ các giá trị tối thiểu một cách lười biếng. Một cạnh không có đường dẫn có thể bao phủ nhiều cạnh đường dẫn, do đó việc cập nhật từng cạnh một sẽ quá chậm. Cập nhật phạm vi giảm công việc này xuống thời gian logarit. 

Việc lập chỉ mục cạnh đường dẫn bắt đầu từ 0 và có chính xác`k`mục, ở đâu`k`là số cạnh trên đường đi đã chọn. Các truy vấn trên các cạnh không có đường dẫn ngay lập tức trả về khoảng cách ngắn nhất ban đầu, tránh việc truy cập cây phân đoạn không hợp lệ. 

Số nguyên Python không bị tràn, điều này rất hữu ích vì độ dài đường dẫn tối đa có thể vượt quá giới hạn 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đường dẫn ngắn nhất được chọn buộc phải thông qua kết nối duy nhất giữa hai bên. 

| Bước | Đã xóa cạnh | Thay thế tốt nhất hiện nay | 
| --- | --- | --- | 
| Đường đi ngắn nhất ban đầu |`1-2-4-5`| hữu hạn | 
| Xóa cạnh đường dẫn đầu tiên |`1-2`| không có cạnh vượt qua | 
| Trả lời |`1`|`-1`| 

Dấu vết cho thấy khi không có cạnh nào không phải cây vượt qua các thành phần tách biệt thì cây phân đoạn giữ giá trị là vô cùng và câu trả lời cuối cùng trở thành`-1`. 

Đối với mẫu thứ hai, tồn tại một số cạnh song song. 

| Bước | Đã xóa cạnh | Kết quả | 
| --- | --- | --- | 
| Xây dựng con đường ngắn nhất | trực tiếp`1-5`| khoảng cách`33`| 
| Xóa cạnh`1-5`| một tuyến đường khác tồn tại | giá trị hữu hạn lớn hơn | 
| Xóa cạnh không liên quan | con đường ban đầu tồn tại |`33`| 

Điều này chứng tỏ tại sao chỉ các cạnh đường dẫn ngắn nhất được chọn mới yêu cầu tính toán thay thế và tại sao các cạnh song song phải được xử lý riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + m) log n)`| Hai lần chạy Dijkstra và một lần cập nhật phạm vi logarit cho mỗi cạnh | 
| Không gian |`O(n + m)`| Lưu trữ đồ thị, dữ liệu cây và cây phân đoạn | 

Các ràng buộc cho phép tiền xử lý logarit tuyến tính gần đúng. Quá trình xử lý truy vấn cuối cùng là không đổi ngoại trừ việc tra cứu cây phân đoạn, vì vậy tất cả`2 * 10^5`truy vấn được xử lý dễ dàng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # paste solve() implementation here
    sys.stdin = old
    return ""

# sample and custom tests should call the copied solve implementation
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị cạnh đơn |`-1`| Ngắt kết nối hoàn toàn | 
| Hai cạnh song song | thay thế hữu hạn | Xử lý cạnh song song | 
| Nhiều đường đi ngắn nhất | khoảng cách ban đầu | Các cạnh đường đi ngắn nhất không quan trọng | 
| Chuỗi dài với một phím tắt | giá trị phím tắt | Cập nhật phạm vi trên nhiều cạnh đường dẫn | 

## Vỏ cạnh 

Đối với trường hợp cạnh song song:```
3 3 1
1 2 5
1 2 1
2 3 1
1
```Cây đường đi ngắn nhất sử dụng cạnh có trọng số`1`. Việc loại bỏ cạnh còn lại không ảnh hưởng đến đường đi đã chọn nên thuật toán trả về ngay khoảng cách ban đầu`2`. 

Đối với nhiều đường đi ngắn nhất:```
4 4 1
1 2 1
2 4 1
1 3 1
3 4 1
2
```Cạnh bị loại bỏ nằm trên một đường đi ngắn nhất nhưng đường đi ngắn nhất còn lại vẫn còn. Quá trình xử lý thay thế xem xét cạnh giao nhau thay thế và lưu giữ khoảng cách tương tự`2`. 

Đối với biểu đồ bị ngắt kết nối:```
2 1 1
1 2 7
1
```Cạnh bị loại bỏ là cạnh duy nhất trên đường đi ngắn nhất. Không có cạnh thay thế nào cập nhật vị trí của nó, vì vậy giá trị được lưu trữ vẫn là vô hạn và câu trả lời là`-1`.
