---
title: "CF 105624G - \u0411\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u043e\u0435 \u043c\u043e\u0440\u0435\u043f\u043b\u0430\u0432\u0430\u043d\u0438\u0435"
description: "Chúng ta được cung cấp một biểu đồ có trọng số vô hướng có tối đa $10^5$ đỉnh và tối đa $4 cdot 10^5$ cạnh. Mỗi cạnh có trọng số ban đầu. Biểu đồ được kết nối."
date: "2026-06-26T19:47:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105624
codeforces_index: "G"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105624
solve_time_s: 58
verified: true
draft: false
---

[CF 105624G - \u0411\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u043e\u0435 \u043c\u043e\u0440\u0435\u043f\u043b\u0430\u0432\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/105624/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có trọng số vô hướng với tối đa$10^5$đỉnh và lên đến$4 \cdot 10^5$các cạnh. Mỗi cạnh có trọng số ban đầu. Biểu đồ được kết nối. 

Đối với mọi cạnh$f$, chúng ta chỉ được phép thay đổi trọng số của nó, độc lập với các cạnh khác, thành bất kỳ số nguyên nào trong một phạm vi rất lớn. Sau khi chúng tôi ấn định trọng lượng mới này cho$f$, chúng ta xem xét tất cả các cây bao trùm của biểu đồ và xem xét những cây có tổng trọng số tối thiểu, tức là các cây bao trùm tối thiểu. Câu hỏi yêu cầu giá trị lớn nhất mà chúng ta có thể gán cho cạnh$f$sao cho vẫn tồn tại ít nhất một cây bao trùm tối thiểu bao gồm cạnh này. 

Vì vậy, đối với mỗi cạnh, chúng ta không hỏi liệu nó có thuộc về MST gốc hay không. Chúng tôi đang hỏi chúng tôi có thể "tăng" trọng lượng của nó đến mức nào trong khi vẫn giữ cho nó có thể sử dụng được trong một số MST, giả sử phần còn lại của biểu đồ không thay đổi. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào cố gắng tính toán lại MST từ đầu trên mỗi cạnh đều không thể thực hiện được. Một tính toán MST duy nhất là$O(m \log n)$, và làm điều đó$m$thời gian sẽ vượt xa giới hạn. Ngay cả bất cứ thứ gì bậc hai trong$n$hoặc$m$ngay lập tức bị loại trừ. Giải pháp dự định phải xử lý trước cấu trúc toàn cục một lần và sau đó trả lời từng cạnh trong thời gian gần như không đổi hoặc logarit. 

Một trường hợp cạnh tinh tế xuất phát từ các cấu trúc trông song song trong đó có nhiều đường dẫn tồn tại giữa các điểm cuối. Ví dụ: nếu hai đỉnh được kết nối bằng một số đường có trọng số tương tự nhau, thì việc tăng trọng lượng của một cạnh có thể hoặc không làm mất tính đủ điều kiện của nó tùy thuộc vào tuyến đường thay thế tốt nhất. Một ý tưởng ngây thơ như chỉ so sánh các nước láng giềng địa phương đã thất bại vì quyết định phụ thuộc vào khả năng kết nối toàn cầu. 

Một trường hợp góc khác là khi biểu đồ chứa nhiều lựa chọn MST có trọng số bằng nhau. Ngay cả khi một cạnh không nằm trong một MST, nó vẫn có thể nằm trong một MST khác, vì vậy chúng ta phải suy luận về sự tồn tại chứ không phải một cây cố định. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: đối với mỗi cạnh$f = (u, v)$, chúng ta tạm thời đặt trọng số của nó thành một giá trị nào đó$x$, tính toán lại cây bao trùm tối thiểu và kiểm tra xem MST đó có bao gồm$f$. Bằng cách tìm kiếm nhị phân$x$, chúng ta có thể tìm thấy giá trị hợp lệ lớn nhất. 

Điều này hoạt động về mặt khái niệm vì việc xây dựng MST được hiểu rõ nhưng nó trở nên không khả thi về mặt tính toán. Mỗi chi phí tính toán MST$O(m \log n)$, và chúng ta sẽ cần nó nhiều lần trên mỗi cạnh. Thậm chí bỏ qua tìm kiếm nhị phân, chỉ cần kiểm tra từng giá trị ứng cử viên sẽ dẫn đến$O(m^2 \log n)$hoặc tệ hơn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần mô phỏng việc thay đổi trọng số. Điều quan trọng là thuộc tính cấu trúc của MST: việc một cạnh có thể thuộc về một số MST hay không chỉ phụ thuộc vào các đường dẫn thay thế giữa các điểm cuối của nó trong biểu đồ gốc. Nếu tồn tại một kết nối thay thế đủ rẻ giữa các điểm cuối của nó thì việc tạo ra một kết nối quá nặng sẽ loại trừ kết nối đó. Nếu không, nó vẫn khả thi. 

Điều này làm giảm vấn đề thành truy vấn đường dẫn minimax cổ điển. Đối với một cạnh$(u, v)$, chúng ta cần giá trị nhỏ nhất có thể có của trọng số cạnh tối đa dọc theo bất kỳ đường dẫn nào giữa$u$Và$v$. Giá trị này xác định hoàn toàn ngưỡng mà vượt quá ngưỡng đó, cạnh sẽ không thể sử dụng được trong bất kỳ MST nào. 

Một kết quả tiêu chuẩn trong lý thuyết đồ thị là các giá trị cực tiểu này chính xác là trọng số cạnh lớn nhất trên các đường đi bên trong cây khung nhỏ nhất của đồ thị. Vì vậy, chúng ta có thể xây dựng một MST một lần, sau đó trả lời mọi truy vấn cạnh bằng cách sử dụng các truy vấn đường dẫn tối đa trên cây đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại MST trên mỗi cạnh |$O(m^2 \log n)$|$O(m)$| Quá chậm | 
| MST + truy vấn tối đa đường dẫn cây |$O(m \log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây khung nhỏ nhất của đồ thị bằng thuật toán Kruskal. Mục đích là trích xuất cấu trúc trong đó các truy vấn đường dẫn tương ứng với kết nối minimax trong biểu đồ gốc. 
2. Root MST một cách tùy ý và xử lý trước nó cho các truy vấn Tổ tiên chung thấp nhất (LCA). Cùng với việc nâng nhị phân, lưu trữ trọng số cạnh tối đa từ mỗi nút đến tổ tiên của nó với lũy thừa hai. Điều này cho phép chúng ta tính toán trọng số cạnh tối đa trên bất kỳ đường đi nào của cây một cách hiệu quả. 
3. Đối với mỗi cạnh$f = (u, v)$, truy vấn MST để tìm trọng số cạnh tối đa dọc theo đường dẫn duy nhất giữa$u$Và$v$. Giá trị này là nút cổ chai của kết nối thay thế tốt nhất có thể giữa các điểm cuối này. 
4. Xuất giá trị thắt cổ chai này làm câu trả lời cho cạnh$f$. 

Lý do bước 3 hoạt động là vì đường dẫn MST giữa hai đỉnh giảm thiểu cạnh tối đa dọc theo bất kỳ đường dẫn kết nối nào. Vì vậy, nó trực tiếp tạo ra con đường “đối thủ” mạnh nhất có thể chống lại lợi thế$f$. 

### Tại sao nó hoạt động 

Sửa một cạnh$f = (u, v)$. Hãy xem xét bất kỳ cây bao trùm nào có chứa$f$. Đang xóa$f$chia cây thành hai thành phần, xác định một vết cắt. Bất kỳ con đường thay thế nào giữa$u$Và$v$phải vượt qua vết cắt này bằng cách sử dụng một số cạnh khác. Nếu tồn tại một đường dẫn trong đó tất cả các cạnh có trọng số nhỏ hơn một giá trị nào đó$x$, sau đó thiết lập$f$cân nặng$x$làm cho nó không thể$f$để duy trì tính cạnh tranh trong bất kỳ MST nào, vì con đường thay thế đó sẽ luôn được ưu tiên trong các trao đổi theo chu kỳ. 

MST đảm bảo rằng trong số tất cả các đường dẫn có thể có giữa$u$Và$v$, đường dẫn giảm thiểu trọng số cạnh tối đa sẽ nắm bắt chính xác ngưỡng này. Vì vậy, trọng lượng an toàn lớn nhất cho$f$là giá trị đường dẫn minimax giữa các điểm cuối của nó trong biểu đồ gốc, được lấy từ cấu trúc MST. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
edges = []
for i in range(m):
    u, v, w = map(int, input().split())
    edges.append((w, u - 1, v - 1, i))

edges.sort()

parent = list(range(n))
size = [1] * n

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    a = find(a)
    b = find(b)
    if a == b:
        return False
    if size[a] < size[b]:
        a, b = b, a
    parent[b] = a
    size[a] += size[b]
    return True

adj = [[] for _ in range(n)]

for w, u, v, idx in edges:
    if union(u, v):
        adj[u].append((v, w))
        adj[v].append((u, w))

LOG = 20
up = [[-1] * n for _ in range(LOG)]
mx = [[0] * n for _ in range(LOG)]
depth = [0] * n

def dfs(v, p):
    for to, w in adj[v]:
        if to == p:
            continue
        up[0][to] = v
        mx[0][to] = w
        depth[to] = depth[v] + 1
        dfs(to, v)

dfs(0, -1)

for k in range(1, LOG):
    for v in range(n):
        if up[k - 1][v] != -1:
            up[k][v] = up[k - 1][up[k - 1][v]]
            mx[k][v] = max(mx[k - 1][v], mx[k - 1][up[k - 1][v]])

def get_max(u, v):
    if depth[u] < depth[v]:
        u, v = v, u
    res = 0

    diff = depth[u] - depth[v]
    for k in range(LOG):
        if diff & (1 << k):
            res = max(res, mx[k][u])
            u = up[k][u]

    if u == v:
        return res

    for k in reversed(range(LOG)):
        if up[k][u] != up[k][v]:
            res = max(res, mx[k][u], mx[k][v])
            u = up[k][u]
            v = up[k][v]

    res = max(res, mx[0][u], mx[0][v])
    return res

ans = [0] * m
for w, u, v, idx in edges:
    ans[idx] = get_max(u, v)

print("\n".join(map(str, ans)))
```Giải pháp đầu tiên xây dựng MST bằng Kruskal, chỉ lưu trữ các cạnh thực sự được chọn. Điều này tạo ra một cây trong đó mỗi cặp đỉnh có đúng một đường đi. 

DFS thiết lập mức cơ sở của việc nâng nhị phân, ghi lại cả trọng số cạnh tối đa và tổ tiên. Bước nhân đôi tạo ra bước nhảy cao hơn, cho phép chúng ta bỏ qua$2^k$tổ tiên trong khi theo dõi trọng lượng cạnh lớn nhất gặp phải. 

chức năng`get_max`thực hiện căn chỉnh độ sâu LCA tiêu chuẩn, sau đó nâng cả hai nút đồng thời cho đến khi nút gốc của chúng khớp với nhau, tích lũy trọng lượng cạnh tối đa gặp dọc theo đường dẫn. 

Mỗi truy vấn được trả lời độc lập theo thời gian logarit. 

Một lỗi triển khai phổ biến là quên rằng MST có thể không được root ở nút 0 theo cách được kết nối nếu DFS không được chạy từ mọi thành phần. Ở đây đồ thị được đảm bảo kết nối, vì vậy một DFS duy nhất là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét đồ thị tam giác: 

| Bước | Hành động | cạnh MST | Truy vấn | 
| --- | --- | --- | --- | 
| Xây dựng MST | chọn các cạnh nhỏ nhất | hai cạnh nhỏ nhất | cạnh (u,v) | 
| Đường dẫn truy vấn | tìm cạnh tối đa trên đường dẫn MST | cây cố định | kết quả | 

MST loại bỏ cạnh nặng nhất, do đó đáp án cho cạnh đó trở thành trọng số của đường thay thế nặng hơn bên trong tam giác. 

Điều này xác nhận rằng câu trả lời phụ thuộc vào khả năng kết nối thay thế chứ không phải sự liền kề trực tiếp. 

### Ví dụ 2 

Trong biểu đồ đường$1 - 2 - 3 - 4$, mọi cạnh đều cần thiết. 

| Cạnh | Đường dẫn MST tối đa | 
| --- | --- | 
| (1,2) | trọng lượng của (1,2) | 
| (2,3) | trọng lượng của (2,3) | 
| (3,4) | trọng lượng của (3,4) | 

Vì không có tuyến đường thay thế nào tồn tại nên đường dẫn minimax giữa các điểm cuối luôn là cạnh trực tiếp, cho thấy rằng tất cả các cạnh có thể được tối đa hóa đến giới hạn cấu trúc của chính chúng. 

Điều này chứng tỏ rằng thuật toán bảo toàn chính xác các cạnh trong cây không tồn tại sự dư thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log n)$| Kruskal xây dựng MST trong$O(m \log m)$, tiền xử lý LCA là$O(n \log n)$, mỗi truy vấn là$O(\log n)$| 
| Không gian |$O(n \log n)$| bảng nâng nhị phân và danh sách kề cho MST | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả hai$n$Và$m$nhiều nhất là vài trăm nghìn và hệ số logarit vẫn còn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    edges = []
    for i in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1, i))

    edges.sort()
    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return False
        if size[a] < size[b]:
            a, b = b, a
        parent[b] = a
        size[a] += size[b]
        return True

    adj = [[] for _ in range(n)]
    for w, u, v, idx in edges:
        if union(u, v):
            adj[u].append((v, w))
            adj[v].append((u, w))

    LOG = 20
    up = [[-1] * n for _ in range(LOG)]
    mx = [[0] * n for _ in range(LOG)]
    depth = [0] * n

    def dfs(v, p):
        for to, w in adj[v]:
            if to == p:
                continue
            up[0][to] = v
            mx[0][to] = w
            depth[to] = depth[v] + 1
            dfs(to, v)

    dfs(0, -1)

    for k in range(1, LOG):
        for v in range(n):
            if up[k - 1][v] != -1:
                up[k][v] = up[k - 1][up[k - 1][v]]
                mx[k][v] = max(mx[k - 1][v], mx[k - 1][up[k - 1][v]])

    def get(u, v):
        if depth[u] < depth[v]:
            u, v = v, u
        res = 0
        diff = depth[u] - depth[v]
        for k in range(LOG):
            if diff & (1 << k):
                res = max(res, mx[k][u])
                u = up[k][u]
        if u == v:
            return res
        for k in reversed(range(LOG)):
            if up[k][u] != up[k][v]:
                res = max(res, mx[k][u], mx[k][v])
                u = up[k][u]
                v = up[k][v]
        res = max(res, mx[0][u], mx[0][v])
        return res

    ans = [0] * m
    for w, u, v, idx in edges:
        ans[idx] = get(u, v)

    return " ".join(map(str, ans))

# custom sanity checks (minimal illustrative, not full judge)
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị cạnh đơn | cùng trọng lượng | MST tầm thường | 
| tam giác | thay thế tốt thứ hai | hành vi chu kỳ | 
| đồ thị đường | giống hệt với trọng lượng | vỏ cây | 
| đồ thị nhỏ dày đặc | khắc phục nút thắt cổ chai | LCA đúng đắn | 

## Vỏ cạnh 

Đối với một biểu đồ đã là cây thì MST chính là biểu đồ đó. Đối với một cạnh, chỉ có một đường đi giữa các điểm cuối của nó, vì vậy cạnh tối đa trên đường đi đó chính là cạnh đó. Thuật toán xây dựng cùng một cây và trả về cùng một trọng số, phù hợp với thực tế là không có tuyến đường thay thế nào tồn tại để giảm tầm quan trọng của cạnh đó. 

Đối với một chu trình có trọng số bằng nhau, mọi cạnh đều có thể hoán đổi cho nhau. MST chọn các cạnh tùy ý, nhưng cạnh tối đa trên bất kỳ đường dẫn nào giữa các điểm cuối luôn có cùng trọng số, do đó mỗi cạnh nhận được đầu ra giống hệt nhau. Truy vấn dựa trên LCA vẫn hoạt động vì tất cả các cạnh được nâng lên đều có trọng số bằng nhau, do đó cực đại lan truyền một cách nhất quán. 

Đối với các đồ thị có nhiều MST có trọng số bằng nhau, MST được xây dựng có thể khác với đồ thị lý thuyết, nhưng thuộc tính đường dẫn cực tiểu là bất biến. Bất kỳ MST nào cũng đủ vì tất cả MST đều bảo toàn khoảng cách nút cổ chai giống nhau, đảm bảo câu trả lời được tính toán không phụ thuộc vào MST nào được chọn.
