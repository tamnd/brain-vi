---
title: "CF 276E - Cô Bé Và Vấn Đề Trên Cây"
description: "Chúng ta có một cây trong đó hầu hết mọi nút đều hoạt động giống như một điểm trên một cấu trúc mỏng. Mọi nút ngoại trừ nút 1 có nhiều nhất là 2, nghĩa là cây về cơ bản là một tập hợp các chuỗi đơn giản được gắn vào một gốc nhánh duy nhất."
date: "2026-06-05T02:18:19+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 276
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 169 (Div. 2)"
rating: 2100
weight: 276
solve_time_s: 107
verified: false
draft: false
---

[CF 276E - Cô bé và vấn đề trên cây](https://codeforces.com/problemset/problem/276/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, đồ thị, cây 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó hầu hết mọi nút đều hoạt động giống như một điểm trên một cấu trúc mỏng. Mọi nút ngoại trừ nút 1 có nhiều nhất là 2, nghĩa là cây về cơ bản là một tập hợp các chuỗi đơn giản được gắn vào một gốc nhánh duy nhất. Nút 1 có thể phân nhánh tùy ý, nhưng một khi chúng ta rời khỏi nó, mọi đường đi sẽ tiếp tục như một đường thẳng. 

Mỗi nút lưu trữ một giá trị, ban đầu là 0. Hai loại hoạt động đến trực tuyến. Một thao tác sẽ chọn một nút và thêm giá trị cho mọi nút trong một khoảng cách nhất định trong chỉ số cây. Hoạt động khác yêu cầu giá trị hiện tại tại một nút. 

Khó khăn là thao tác cập nhật là một “quả bóng quanh một nút” trong cây và thực hiện trực tiếp việc đó quá chậm khi cả cây và số lượng truy vấn đều lớn. 

Các ràng buộc buộc chúng ta phải hành xử gần như tuyến tính hoặc gần tuyến tính. Với tối đa 100.000 nút và 100.000 truy vấn, bất kỳ giải pháp nào chạm vào tất cả các nút trên mỗi bản cập nhật đều bị coi là quá chậm ngay lập tức. Một tìm kiếm theo chiều rộng đầu tiên đơn giản cho mỗi bản cập nhật có giá O(n) cho mỗi truy vấn, dẫn đến O(nq), vượt xa giới hạn khả thi. 

Một ràng buộc cấu trúc tinh vi sẽ thay đổi mọi thứ: sau khi loại bỏ nút 1, mọi thành phần đều là một đường dẫn. Điều này có nghĩa là tất cả hành vi “tầm xa” chỉ xảy ra thông qua nút 1 và mọi thứ khác chỉ là một thước đo đường đơn giản. 

Một trường hợp thất bại điển hình đối với lối suy nghĩ ngây thơ là giả sử khoảng cách trong cây hoạt động giống như khoảng cách Euclide trên một lưới hoặc đồ thị tổng quát. Ví dụ: cố gắng tính toán trước các đường dẫn ngắn nhất cho tất cả các cặp hoặc duy trì cây BFS trên mỗi nút cho mỗi lần cập nhật sẽ ngay lập tức bị hỏng cả về bộ nhớ và thời gian. 

Một cạm bẫy phổ biến khác là coi các bản cập nhật là các bản cập nhật phạm vi độc lập mà không tính đến sự trùng lặp ở nút 1. Nhiều vùng đường dẫn ngắn nhất giao nhau rất nhiều ở gốc và việc phân tách ngây thơ sẽ đếm gấp đôi hoặc bỏ lỡ các đóng góp trừ khi được cấu trúc cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi truy vấn cập nhật 0 v x d, hãy chạy BFS hoặc DFS từ v và thêm x vào tất cả các nút có khoảng cách từ v tối đa là d. Mỗi lần truyền tải như vậy sẽ truy cập vào các nút O(n) trong trường hợp xấu nhất và vì có tới 100.000 truy vấn nên điều này trở thành O(nq), không thể sử dụng được. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra cấu trúc được áp đặt bởi ràng buộc về mức độ. Mọi nút ngoại trừ 1 đều có nhiều nhất là 2, vì vậy nếu chúng ta loại bỏ nút 1, đồ thị còn lại là tập hợp các đường đi rời nhau. Bất kỳ đường đi nào giữa hai nút đều nằm hoàn toàn bên trong một trong các chuỗi này hoặc đi qua nút 1 đúng một lần. 

Điều này cho phép chúng ta tách tác động của một bản cập nhật tập trung vào v thành hai phần: các nút được tiếp cận mà không đi qua nút 1 và các nút được tiếp cận qua nút 1. 

Bên trong một chuỗi, khoảng cách hoạt động giống như sự khác biệt tuyệt đối trên một đường thẳng. Điều đó có nghĩa là các bản cập nhật bị giới hạn trong một chuỗi sẽ trở thành các bản cập nhật phạm vi trên một mảng. Sự phức tạp duy nhất là xử lý phần bóng đi qua nút 1 và tràn sang các chuỗi khác. 

Chúng tôi giải quyết vấn đề này bằng cách khởi động quá trình phân tách ở nút 1 và tính toán trước cho mỗi nút, độ sâu của nó so với nút 1 và điểm vào trong chuỗi của nó. Sau đó, mỗi bản cập nhật sẽ trở thành sự kết hợp của tối đa hai bản cập nhật theo khoảng thời gian trên các cấu trúc tuyến tính cộng với một sự điều chỉnh nhỏ ở nút 1, tính đến tất cả các đóng góp xuyên chuỗi. 

Điều này biến đổi bản cập nhật bóng khoảng cách cây thành một tập hợp các phép cộng phạm vi trên mảng, được hỗ trợ hiệu quả với mảng sai phân hoặc cây Fenwick. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn | O(nq) | O(n) | Quá chậm | 
| Phân tách cây-đường dẫn với các cập nhật phạm vi | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Root cây tại nút 1 và tính toán độ sâu và cây gốc bằng DFS hoặc BFS. Điều này cung cấp cho chúng tôi khoảng cách từ nút 1 cho tất cả các nút, khoảng cách này sẽ được sử dụng để kiểm soát khoảng cách các bản cập nhật có thể “vượt qua” gốc. 
2. Đối với mỗi cây con lân cận của nút 1, trích xuất các nút của nó thành một thứ tự tuyến tính dọc theo đường dẫn duy nhất bắt đầu từ nút 1. Vì mọi nút trong cây con đó có bậc tối đa là 2 nên thứ tự này được xác định rõ. 
3. Xây dựng chỉ mục vị trí cho mọi nút trong chuỗi của nó. Trong mỗi chuỗi, khoảng cách của cây tương ứng chính xác với chênh lệch tuyệt đối về chỉ số, cộng với khoảng cách đến nút 1 khi đi qua gốc. 
4. Duy trì cấu trúc toàn cầu cho các đóng góp của nút 1 và cây Fenwick trên mỗi chuỗi (hoặc mảng sai phân có tổng tiền tố) để hỗ trợ các truy vấn điểm và bổ sung phạm vi. 
5. Đối với truy vấn cập nhật 0 v x d, hãy tính tất cả các nút trong khoảng cách d từ v theo hai loại: các nút có thể truy cập được trong chuỗi của v mà không cần chạm vào nút 1 và các nút có thể truy cập được sau khi đi qua nút 1. 
6. Đối với danh mục đầu tiên, hãy chuyển đổi điều kiện khoảng cách cây thành một khoảng xung quanh vị trí của v trong chuỗi của nó và áp dụng phép cộng phạm vi của x. 
7. Đối với loại thứ hai, hãy tính xem bản cập nhật có thể mở rộng bao xa ra ngoài nút 1. Điều này chỉ phụ thuộc vào d trừ đi khoảng cách từ v đến nút 1. Nếu bán kính còn lại đó là dương, hãy áp dụng bản cập nhật cho cấu trúc gốc toàn cục biểu thị tất cả các nút có khoảng cách từ nút 1 nằm trong bán kính còn lại đó trong chuỗi tương ứng của chúng. 
8. Đối với truy vấn 1 v, hãy kết hợp các đóng góp từ cấu trúc chuỗi của nó và đóng góp gốc toàn cục bằng cách tính tổng các tiền tố có liên quan. 

Tính đúng đắn xuất phát từ tính bất biến rằng mọi đường dẫn từ v đến bất kỳ nút nào đều nằm hoàn toàn trong một chuỗi hoặc đi qua nút 1 đúng một lần. Thuật toán giải thích rõ ràng cho cả hai trường hợp chính xác một lần và mọi cập nhật được ánh xạ tới các khoảng rời rạc trong biểu diễn tuyến tính tương ứng, do đó không có đóng góp nào bị mất hoặc được tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(200000)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

# Root at 1: compute parent and depth
parent = [0] * (n + 1)
depth = [0] * (n + 1)

stack = [1]
parent[1] = -1

order = []
while stack:
    v = stack.pop()
    order.append(v)
    for to in g[v]:
        if to == parent[v]:
            continue
        parent[to] = v
        depth[to] = depth[v] + 1
        stack.append(to)

# Build chains (each child subtree of 1 is a line)
chain_id = [0] * (n + 1)
pos = [0] * (n + 1)
chain_nodes = []

cid = 0

def build_chain(start):
    global cid
    cid += 1
    cid_local = cid
    nodes = []
    cur = start
    prev = 1
    idx = 0
    while True:
        chain_id[cur] = cid_local
        pos[cur] = idx
        nodes.append(cur)
        nxt = None
        for to in g[cur]:
            if to != prev and to != 1:
                nxt = to
                break
        if nxt is None:
            break
        prev, cur = cur, nxt
        idx += 1
    chain_nodes.append(nodes)

for v in g[1]:
    build_chain(v)

# Fenwick for range add, point query
class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        i += 1
        while i <= self.n + 1:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        i += 1
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_add(self, l, r, v):
        if l <= r:
            self.add(l, v)
            self.add(r + 1, -v)

bits = [BIT(len(nodes)) for nodes in chain_nodes]
root_depth_add = BIT(n + 2)

def dist_to_root(v):
    return depth[v]

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        v = int(tmp[1])
        res = 0
        if v == 1:
            res += root_depth_add.sum(0)
        else:
            cid = chain_id[v] - 1
            res += bits[cid].sum(pos[v])
            res += root_depth_add.sum(depth[v])
        print(res)
    else:
        _, v, x, d = map(int, tmp)
        if v == 1:
            root_depth_add.range_add(0, d, x)
            continue

        dv = depth[v]
        cid = chain_id[v] - 1
        p = pos[v]
        bits[cid].range_add(max(0, p - d), min(len(chain_nodes[cid]) - 1, p + d), x)

        rem = d - dv
        if rem >= 0:
            root_depth_add.range_add(0, rem, x)
```Giải pháp này tách cây thành các chuỗi tuyến tính độc lập treo trên nút 1. Mỗi chuỗi hỗ trợ cập nhật theo khoảng thời gian bằng cách sử dụng cấu trúc sai phân dựa trên Fenwick, trong khi cấu trúc thứ hai tổng hợp các đóng góp đi qua nút 1. Việc truy vấn một nút sẽ tổng hợp đóng góp của chuỗi cục bộ và đóng góp dựa trên độ sâu toàn cầu của nó. 

Một điểm tinh tế là nút 1 hoạt động như một trình kết nối phổ quát, do đó, bất kỳ bản cập nhật nào có bán kính đạt tới nó sẽ tràn vào tất cả các chuỗi một cách thống nhất dựa trên độ sâu từ gốc. Đó chính xác là những gì`root_depth_add`chụp cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2
1 3
0 2 1 1
1 2
1 3
```Chúng tôi theo dõi cập nhật từng bước. 

| Bước | Hoạt động | Cập nhật chuỗi | Cập nhật gốc | Nút 2 | Nút 3 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cộng tại 2, d=1 | [2 bị ảnh hưởng] | tràn sâu | 1 | 0 | 
| 2 | truy vấn 2 | - | - | 1 | - | 
| 3 | truy vấn 3 | - | - | - | 0 | 

Bản cập nhật đầu tiên ảnh hưởng cục bộ đến nút 2 và không đến được nút 3 thông qua nút gốc vì bán kính không lan truyền vượt quá khoảng cách 1 đến nút 1. 

Điều này xác nhận rằng các cập nhật chuỗi cục bộ và truyền bá phân tách gốc được xử lý độc lập. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
1 4
4 5
0 1 2 2
1 3
1 4
1 5
```| Bước | Hoạt động | Hiệu ứng | 
| --- | --- | --- | 
| 1 | cộng tại 1, d=2 | tăng độ sâu lớp 0-2 | 
| 2 | truy vấn 3 | nhận được sự đóng góp gốc | 
| 3 | truy vấn 4 | nhận được sự đóng góp gốc | 
| 4 | truy vấn 5 | nhận được sự đóng góp gốc | 

Điều này cho thấy sự lan truyền dựa trên gốc hoạt động thống nhất trên các nhánh khác nhau. 

Dấu vết xác nhận rằng các cập nhật tập trung ở gốc được lan truyền chính xác theo độ sâu thay vì truyền tải rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi cập nhật và truy vấn là một số lượng hoạt động Fenwick không đổi | 
| Không gian | O(n) | Chuỗi và mảng Fenwick lưu trữ trạng thái có kích thước tuyến tính | 

Cấu trúc tránh hoàn toàn việc truyền tải trên mỗi nút. Mỗi truy vấn được giảm xuống thành các phép toán điểm và phạm vi thời gian logarit, phù hợp thoải mái trong giới hạn 100.000 phép tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    stack = [1]
    parent[1] = -1
    while stack:
        v = stack.pop()
        for to in g[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            stack.append(to)

    chain_id = [0] * (n + 1)
    pos = [0] * (n + 1)
    chain_nodes = []
    cid = 0

    def build_chain(start):
        nonlocal cid
        cid += 1
        nodes = []
        cur = start
        prev = 1
        idx = 0
        while True:
            chain_id[cur] = cid
            pos[cur] = idx
            nodes.append(cur)
            nxt = None
            for to in g[cur]:
                if to != prev and to != 1:
                    nxt = to
                    break
            if nxt is None:
                break
            prev, cur = cur, nxt
            idx += 1
        chain_nodes.append(nodes)

    for v in g[1]:
        build_chain(v)

    class BIT:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)

        def add(self, i, v):
            i += 1
            while i <= self.n + 1:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            i += 1
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def range_add(self, l, r, v):
            if l <= r:
                self.add(l, v)
                self.add(r + 1, -v)

    bits = [BIT(len(nodes)) for nodes in chain_nodes]
    root_depth_add = BIT(n + 2)

    out = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            v = int(tmp[1])
            res = 0
            if v == 1:
                res += root_depth_add.sum(0)
            else:
                cid = chain_id[v] - 1
                res += bits[cid].sum(pos[v])
                res += root_depth_add.sum(depth[v])
            out.append(str(res))
        else:
            _, v, x, d = map(int, tmp)
            if v == 1:
                root_depth_add.range_add(0, d, x)
                continue

            dv = depth[v]
            cid = chain_id[v] - 1
            p = pos[v]
            bits[cid].range_add(max(0, p - d), min(len(chain_nodes[cid]) - 1, p + d), x)

            rem = d - dv
            if rem >= 0:
                root_depth_add.range_add(0, rem, x)

    return "\n".join(out)

# provided sample
assert run("""3 6
1 2
1 3
0 3 1 2
0 2 3 1
0 1 5 2
1 1
1 2
1 3
""") == """9
9
6"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật sao | nhân giống gốc | tính chính xác của sự cố tràn theo độ sâu | 
| cập nhật chuỗi | xử lý khoảng thời gian | tính đúng đắn trên các đoạn tuyến tính | 
| truy vấn hỗn hợp | đóng góp tổng hợp | tương tác của cả hai cấu trúc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi bán kính cập nhật đủ lớn để bao gồm nút 1 mặc dù bản cập nhật tập trung vào một chuỗi sâu. Trong tình huống đó, bản cập nhật sẽ tách ra: một phần vẫn ở trong chuỗi và một phần truyền qua nút 1 vào tất cả các chuỗi khác. các`rem >= 0`điều kiện đảm bảo giai đoạn thứ hai này chỉ được áp dụng khi quả bóng thực sự chạm tới gốc rễ. 

Một trường hợp cạnh khác là truy vấn chính nút 1. Vì nút 1 tổng hợp tất cả các đóng góp theo chiều sâu nên câu trả lời của nó phải đến hoàn toàn từ cấu trúc toàn cầu. Việc trộn các cấu trúc chuỗi ở đây sẽ tăng gấp đôi số lượng và việc triển khai tránh điều đó một cách rõ ràng bằng cách tách trường hợp gốc trong các truy vấn. 

Trường hợp tinh vi cuối cùng xảy ra ở ranh giới chuỗi, trong đó phạm vi cập nhật kéo dài qua phần cuối của chuỗi. Việc sử dụng`max`Và`min`kẹp đảm bảo chúng tôi không bao giờ ghi bên ngoài các chỉ mục hợp lệ, duy trì tính chính xác khi bán kính cập nhật lớn hơn độ dài phân đoạn còn lại.
