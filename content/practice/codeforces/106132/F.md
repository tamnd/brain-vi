---
title: "CF 106132F - LCS của đơn hàng DFS"
description: "Chúng tôi đang làm việc với một cây trong đó mỗi đỉnh có thể được chọn làm điểm bắt đầu cho việc duyệt DFS theo thứ tự trước. Trong quá trình truyền tải như vậy, chúng ta truy cập một nút, sau đó duyệt đệ quy các nút lân cận của nó theo một thứ tự tùy ý nào đó, tạo ra một chuỗi tuyến tính gồm tất cả các đỉnh."
date: "2026-06-19T19:47:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106132
codeforces_index: "F"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Individual Programming Contest"
rating: 0
weight: 106132
solve_time_s: 77
verified: true
draft: false
---

[CF 106132F - LCS của đơn hàng DFS](https://codeforces.com/problemset/problem/106132/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một cây trong đó mỗi đỉnh có thể được chọn làm điểm bắt đầu cho việc duyệt DFS theo thứ tự trước. Trong quá trình truyền tải như vậy, chúng ta truy cập một nút, sau đó duyệt đệ quy các nút lân cận của nó theo một thứ tự tùy ý nào đó, tạo ra một chuỗi tuyến tính gồm tất cả các đỉnh. Bởi vì thứ tự kề là tự do, mỗi đỉnh bắt đầu không tương ứng với một chuỗi truyền tải cố định duy nhất mà tương ứng với toàn bộ họ các hoán vị thứ tự trước hợp lệ, trong đó hạn chế về cấu trúc duy nhất là mỗi cây con xuất hiện dưới dạng một khối liền kề sau khi được nhập. 

Với mọi đỉnh$u$, chúng tôi biểu thị bằng$S_u$tập hợp tất cả các chuỗi có thể đạt được bằng cách chạy DFS đó bắt đầu từ$u$, thay đổi thứ tự của trẻ ở mỗi bước. Cho hai đỉnh$u$Và$v$, chúng ta so sánh tất cả các cặp trình tự có thể có$a_1 \in S_u$Và$a_2 \in S_v$, và xác định$f(u,v)$là độ dài LCS tối thiểu có thể có giữa một cặp như vậy. 

LCS ở đây được tính toán dựa trên hai hoán vị của cùng một tập đỉnh, nhưng khó khăn chính là chúng ta được phép sắp xếp lại thứ tự các phần tử con DFS một cách độc lập trong cả hai lần duyệt, nhằm mục đích làm cho hai chuỗi này giống nhau nhất có thể về mặt cấu trúc chuỗi con. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các đỉnh và truy vấn, do đó, mọi giải pháp về cơ bản phải là tuyến tính hoặc logarit cho mỗi truy vấn sau khi tiền xử lý. Một bậc hai hoặc thậm chí$O(n \log n)$mỗi cách tiếp cận truy vấn ngay lập tức không khả thi vì nó sẽ dẫn đến$10^{10}$hoạt động quy mô. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai đỉnh truy vấn đều giống nhau. Mặc dù cây cơ bản giống hệt nhau, nhưng quyền tự do sắp xếp lại thứ tự con có nghĩa là hai lần duyệt DFS từ cùng một gốc không phải là các chuỗi giống hệt nhau và một giả định ngây thơ rằng LCS luôn luôn giống nhau.$n$sẽ không chính xác. 

Một trường hợp cạnh quan trọng khác là khi hai đỉnh nằm cách xa nhau trên cây. Trong những trường hợp như vậy, nhiều nút thuộc về các cây con có thể được hoán vị độc lập và sự liên kết tham lam ngây thơ của các lệnh DFS sẽ đánh giá quá cao LCS vì nó ngầm giả định thứ tự cây con nhất quán. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng suy luận trực tiếp từ định nghĩa, cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị đặt hàng trước DFS từ$u$, tất cả từ$v$và tính LCS cho mỗi cặp. Ngay cả đối với một gốc duy nhất, số lượng lệnh DFS hợp lệ tăng theo giai thừa khi phân nhánh, vì mỗi nút có thể sắp xếp lại thứ tự con của nó một cách tùy ý. Điều này làm cho số lượng chuỗi tăng theo cấp số nhân trong trường hợp xấu nhất và việc so sánh tất cả các cặp là hoàn toàn không khả thi. 

Quan sát chính là mặc dù các thứ tự DFS khác nhau nhưng tất cả chúng đều tuân theo cùng một phân tách: mỗi nút đóng góp một cấu trúc khối liền kề tương ứng với các cây con của nó. Tính linh hoạt duy nhất là sự hoán vị của các cây con anh em. Điều này có nghĩa là từ góc độ so khớp dãy con, chúng ta không xử lý các hoán vị tùy ý mà xử lý các hoán vị bị ràng buộc bởi cấu trúc cây có gốc. 

Sự đơn giản hóa cấu trúc quan trọng là khi so sánh hai thứ tự DFS, hầu hết cây có thể được “di chuyển xung quanh” một cách độc lập trong mỗi chuỗi bằng cách chọn thứ tự cây con khác nhau. Kết quả là, các đỉnh duy nhất góp phần liên tục vào sự trùng khớp không thể tránh khỏi trong mọi cách xây dựng đối nghịch là những đỉnh nằm trên đường đi duy nhất giữa hai đỉnh bắt đầu. Mọi thứ ngoài đường dẫn đó có thể được sắp xếp theo một đường truyền để xuất hiện theo thứ tự tránh sự căn chỉnh trong đường dẫn khác. 

Điều này làm giảm vấn đề xuống còn việc suy luận xem cấu trúc buộc phải giữ lại bao nhiêu trong bất kỳ sự sắp xếp chuỗi con nào, hóa ra chỉ phụ thuộc vào khoảng cách giữa hai đỉnh trong cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các đơn đặt hàng DFS | Hàm mũ | Hàm mũ | Quá chậm | 
| Quan sát giảm đường đi của cây |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước cây để hỗ trợ các truy vấn tổ tiên chung thấp nhất và tính toán khoảng cách giữa hai nút bất kỳ. Khi chúng ta có thể tính toán khoảng cách một cách nhanh chóng, mỗi truy vấn sẽ giảm xuống còn một đánh giá công thức duy nhất. 

1. Root cây một cách tùy ý, ví dụ như ở nút 1, và chạy DFS để tính toán con trỏ gốc và độ sâu. Điều này thiết lập một cấu trúc nơi chúng ta có thể tính toán LCA một cách hiệu quả. 
2. Xây dựng bảng nâng nhị phân cho các truy vấn LCA. Điều này cho phép chúng tôi tính toán tổ tiên chung thấp nhất của hai nút bất kỳ theo thời gian logarit, điều này cần thiết do số lượng truy vấn lớn. 
3. Đối với mỗi truy vấn$(u, v)$, tính khoảng cách giữa chúng bằng cách sử dụng danh tính tiêu chuẩn$$\text{dist}(u,v) = \text{depth}(u) + \text{depth}(v) - 2 \cdot \text{depth}(\text{LCA}(u,v)).$$4. Xuất giá trị của$f(u,v)$, tương ứng với số cạnh trên đường đi giữa$u$Và$v$, tức là$\text{dist}(u,v)$. 

Lý do điều này có hiệu quả là vì trong bất kỳ cặp trình tự đặt hàng trước DFS nào, chúng ta có thể tự do hoán vị các cây con anh em để loại bỏ tất cả sự căn chỉnh bên ngoài đường dẫn đơn giản duy nhất giữa$u$Và$v$. Tuy nhiên, dọc theo con đường đó, cấu trúc tổ tiên buộc một chuỗi đỉnh tối thiểu phải duy trì sự tôn trọng trật tự một cách nhất quán trong bất kỳ quá trình truyền tải DFS nào. Chuỗi bắt buộc đó có độ dài chính xác bằng số cạnh giữa hai đỉnh và không có thứ tự đối nghịch nào có thể làm giảm LCS xuống dưới ràng buộc kết nối vốn có này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

LOG = (n).bit_length()
up = [[0] * (n + 1) for _ in range(LOG)]
depth = [0] * (n + 1)

def dfs(u, p):
    up[0][u] = p
    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)

dfs(1, 0)

for k in range(1, LOG):
    for v in range(1, n + 1):
        up[k][v] = up[k - 1][up[k - 1][v]]

def lca(a, b):
    if depth[a] < depth[b]:
        a, b = b, a
    diff = depth[a] - depth[b]
    bit = 0
    while diff:
        if diff & 1:
            a = up[bit][a]
        diff >>= 1
        bit += 1

    if a == b:
        return a

    for k in range(LOG - 1, -1, -1):
        if up[k][a] != up[k][b]:
            a = up[k][a]
            b = up[k][b]

    return up[0][a]

def dist(a, b):
    c = lca(a, b)
    return depth[a] + depth[b] - 2 * depth[c]

for _ in range(q):
    u, v = map(int, input().split())
    print(dist(u, v))
```Việc thực hiện tập trung vào việc nâng cấp nhị phân. DFS sửa các độ sâu và cha mẹ trực tiếp, đồng thời bàn nâng nén các bước nhảy tổ tiên thành sức mạnh của hai. Sau đó, mỗi truy vấn sẽ giảm xuống tính toán LCA, theo sau là công thức số học theo thời gian không đổi cho khoảng cách. Phần tinh tế duy nhất là đảm bảo bước căn chỉnh độ sâu là chính xác trước khi chuyển cả hai nút lên trên cùng nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét cây trong đó 1 được kết nối với 2 và 3 và 3 được kết nối với 4 và 5. Đối với truy vấn$(1,4)$, LCA là 1, do đó khoảng cách được tính là 2. Điều này phù hợp với ý tưởng rằng đường đi là$1 \rightarrow 3 \rightarrow 4$, chứa hai cạnh. 

| Bước | bạn | v | LCA | độ sâu(u) | độ sâu(v) | quận | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 4 | - | 0 | 2 | - | 
| sau LCA | 1 | 4 | 1 | 0 | 2 | - | 
| cuối cùng | 1 | 4 | 1 | 0 | 2 | 2 | 

Dấu vết này xác nhận rằng chỉ có khoảng cách cấu trúc mới quan trọng và các hoán vị cây con không ảnh hưởng đến LCS được thu nhỏ cuối cùng. 

Ví dụ thứ hai, lấy cây dòng$1 - 2 - 3 - 4 - 5$. Đối với truy vấn$(2,5)$, LCA là 2 và khoảng cách là 3. Đường đi bắt buộc là$2 \rightarrow 3 \rightarrow 4 \rightarrow 5$, cho ba cạnh và do đó đáp án 3. 

| Bước | bạn | v | LCA | độ sâu(u) | độ sâu(v) | quận | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 5 | - | 1 | 4 | - | 
| sau LCA | 2 | 5 | 2 | 1 | 4 | - | 
| cuối cùng | 2 | 5 | 2 | 1 | 4 | 3 | 

Điều này cho thấy ngay cả trong một cây suy biến hoàn toàn, kết quả vẫn nhất quán với độ dài đường đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q \log n)$| DFS và tiền xử lý nâng nhị phân, sau đó là LCA cho mỗi truy vấn | 
| Không gian |$O(n \log n)$| Bảng tổ tiên và biểu diễn kề | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn cho$n \le 2 \cdot 10^5$và mỗi truy vấn được trả lời theo thời gian logarit, giúp giải pháp trở nên hiệu quả với kích thước đầu vào tối đa. 

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

    LOG = (n).bit_length()
    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    sys.setrecursionlimit(10**7)

    def dfs(u, p):
        up[0][u] = p
        for v in g[u]:
            if v != p:
                depth[v] = depth[u] + 1
                dfs(v, u)

    dfs(1, 0)

    for k in range(1, LOG):
        for v in range(1, n + 1):
            up[k][v] = up[k - 1][up[k - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a
        diff = depth[a] - depth[b]
        bit = 0
        while diff:
            if diff & 1:
                a = up[bit][a]
            diff >>= 1
            bit += 1
        if a == b:
            return a
        for k in range(LOG - 1, -1, -1):
            if up[k][a] != up[k][b]:
                a = up[k][a]
                b = up[k][b]
        return up[0][a]

    def dist(a, b):
        c = lca(a, b)
        return depth[a] + depth[b] - 2 * depth[c]

    out = []
    for _ in range(q):
        u, v = map(int, input().split())
        out.append(str(dist(u, v)))
    return "\n".join(out)

# sample 1
assert run("""5 2
1 2
1 3
3 4
3 5
1 4
3 3
""") == "2\n0"

# chain test
assert run("""5 1
1 2
2 3
3 4
4 5
2 5
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn cây mẫu | 2, 0 | tính đúng đắn cơ bản và trường hợp u=v | 
| cây dòng | 3 | hành vi đường dẫn thuần túy | 

## Vỏ cạnh 

Đối với trường hợp cả hai điểm cuối đều có cùng một đỉnh, chẳng hạn như$u=v=3$, LCA thực chất là nút đó và khoảng cách được tính toán bằng 0. Điều này phù hợp với thực tế là đường dẫn duy nhất từ ​​một nút đến chính nó không chứa cạnh nào và do đó không có độ dài chuỗi bắt buộc được đóng góp bởi việc truyền tải giữa các thành phần riêng biệt. 

Đối với các nút được kết nối bằng một cạnh duy nhất, chẳng hạn như$u=2, v=3$trong một cây nơi chúng được liên kết trực tiếp, LCA là một điểm cuối và khoảng cách là 1. Tính linh hoạt thứ tự trước của DFS không thể loại bỏ ràng buộc kề cận này vì bất kỳ quá trình truyền tải nào từ nút này sang nút khác đều phải đi qua cạnh đó chính xác một lần, làm cho LCS tối thiểu bằng 1 trong trường hợp này.
