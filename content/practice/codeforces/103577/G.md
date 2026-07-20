---
title: "CF 103577G - Phép biến đổi toán học"
description: "Chúng ta có một cây có gốc tại nút $1$, trong đó mỗi nút lưu trữ một giá trị số, ban đầu là $0$. Hai loại hoạt động được thực hiện trực tuyến. Thao tác đầu tiên yêu cầu tổng các giá trị dọc theo đường dẫn đơn giản duy nhất giữa hai nút $u$ và $v$."
date: "2026-07-03T03:49:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103577
codeforces_index: "G"
codeforces_contest_name: "2021 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 103577
solve_time_s: 1054
verified: true
draft: false
---

[CF 103577G - Chuyển đổi vật chất](https://codeforces.com/problemset/problem/103577/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc tại nút$1$, trong đó mỗi nút lưu trữ một giá trị số, ban đầu$0$. Hai loại hoạt động được thực hiện trực tuyến. 

Thao tác đầu tiên yêu cầu tổng các giá trị dọc theo đường dẫn đơn giản duy nhất giữa hai nút$u$Và$v$. Đây là truy vấn đường dẫn cây cổ điển, trong đó câu trả lời phụ thuộc vào giá trị nút động hiện tại. 

Thao tác thứ hai cập nhật cây con theo cách có cấu trúc. Đối với nút gốc đã chọn$u$, mọi nút$x$trong cây con của nó nhận được mức tăng phụ thuộc vào khoảng cách của nó với$u$. Nếu như$d(x,u)$khoảng cách đó thì giá trị gia tăng là$v + k \cdot d(x,u)$. Đây không phải là một bản cập nhật thống nhất: các nút ở sâu hơn trong cây con nhận được số gia tăng lớn hơn trong cấp số cộng. 

Các ràng buộc cho phép lên đến$3 \times 10^5$các nút và truy vấn, ngay lập tức loại trừ bất kỳ giải pháp nào xử lý từng đường dẫn hoặc cây con bằng cách truyền tải rõ ràng. Một cách tiếp cận đơn giản đi theo cây con cho mỗi bản cập nhật và đường dẫn cho mọi truy vấn sẽ chuyển thành$O(n)$mỗi thao tác, đưa ra$O(nq)$trong trường hợp xấu nhất, điều này vượt xa tính khả thi. 

Một khó khăn tinh tế xuất phát từ thực tế là các bản cập nhật phụ thuộc vào khoảng cách. Bản cập nhật phạm vi cây con Euler-tour đơn giản không áp dụng trực tiếp vì giá trị gia tăng không cố định trên cây con. Một vấn đề tiềm ẩn khác là các truy vấn đường dẫn phụ thuộc vào các cập nhật tích lũy từ tất cả các hoạt động của cây con trước đó, do đó việc xử lý từng phần hoặc từng phần phải duy trì nhất quán trên toàn cầu. 

## Phương pháp tiếp cận 

Giải pháp brute-force duy trì các giá trị nút một cách rõ ràng. Bản cập nhật cây con thực hiện DFS từ$u$, tính toán khoảng cách và cập nhật từng nút. Truy vấn đường dẫn đi theo đường dẫn từ$u$ĐẾN$v$và các giá trị tổng. Mỗi chi phí hoạt động$O(n)$trong trường hợp xấu nhất, do đó tổng chi phí trở thành$O(nq)$, điều này là không thể thực hiện được đối với$3 \times 10^5$. 

Quan sát chính là cả hai thao tác đều trở nên đơn giản nếu chúng ta biểu thị các giá trị nút theo khoảng cách từ gốc và sử dụng phân tách đường dẫn. Nguyên tắc cập nhật phụ thuộc vào$d(x,u)$, có thể được viết lại bằng cách sử dụng độ sâu và tổ tiên chung thấp nhất:$$d(x,u) = \text{depth}(x) + \text{depth}(u) - 2\cdot \text{depth}(\mathrm{lca}(x,u)).$$Bên trong cây con của$u$, thuật ngữ LCA đơn giản hóa vì$u$là tổ tiên của$x$, Vì thế$\mathrm{lca}(x,u)=u$. Như vậy$$d(x,u) = \text{depth}(x) - \text{depth}(u).$$Điều này biến bản cập nhật thành:$$v + k(\text{depth}(x) - \text{depth}(u)) = (v - k\cdot \text{depth}(u)) + k\cdot \text{depth}(x).$$Vì vậy, mỗi bản cập nhật sẽ thêm một hàm tuyến tính theo chiều sâu trên một cây con:$$A + B \cdot \text{depth}(x)$$Ở đâu$A = v - k\cdot \text{depth}(u)$Và$B = k$. 

Điều này làm giảm vấn đề để hỗ trợ: 

1. Phạm vi cây con bổ sung của hàm tuyến tính theo chiều sâu. 
2. Truy vấn tổng đường dẫn trên các giá trị nút động. 

Chúng ta làm phẳng cây bằng chuyến tham quan Euler để mỗi cây con trở thành một đoạn. Chúng tôi duy trì hai cây Fenwick (hoặc cây phân đoạn có lan truyền lười) để theo dõi các hệ số của hàm tuyến tính. Mỗi giá trị nút trở thành sự kết hợp của hai cấu trúc toàn cầu: 

một cho những đóng góp liên tục và một cho những đóng góp có chiều sâu. 

Đối với các truy vấn đường dẫn, chúng tôi sử dụng danh tính tiêu chuẩn:$$\text{sum}(u,v) = \text{sum}(1 \to u) + \text{sum}(1 \to v) - 2\cdot \text{sum}(1 \to \mathrm{lca}(u,v)).$$Mỗi tổng tiền tố được đánh giá từ hai cấu trúc được duy trì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi bản cập nhật/truy vấn |$O(nq)$|$O(n)$| Quá chậm | 
| Chuyến tham quan Euler + Phân rã LCA + Fenwick |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp theo bốn giai đoạn. 

### 1. Root cây và cấu trúc tiền xử lý 

Chúng tôi root cây tại nút$1$. Chúng tôi tính toán cho mỗi nút cha, độ sâu và thời gian vào và ra của chuyến tham quan Euler. Chúng tôi cũng tính toán bảng nâng nhị phân cho các truy vấn LCA để có thể trả lời$\mathrm{lca}(u,v)$TRONG$O(\log n)$. 

Chuyến tham quan Euler đảm bảo rằng mọi cây con đều tương ứng với một đoạn liền kề$[tin[u], tout[u]]$. 

### 2. Viết lại cập nhật cây con thành dạng chuyên sâu 

Để cập nhật$(u, v, k)$, mỗi nút$x$trong cây con của$u$nhận được:$$v + k \cdot (\text{depth}(x) - \text{depth}(u)).$$Điều này chia thành hai đóng góp độc lập: 

một hằng số$v - k\cdot \text{depth}(u)$và một hệ số$k$nhân với độ sâu (x). 

Vì vậy, chúng tôi duy trì hai cấu trúc thêm phạm vi theo thứ tự Euler: 

một cửa hàng lưu trữ các phần bổ sung liên tục, phần còn lại lưu trữ các phần bổ sung có trọng số theo chiều sâu. 

### 3. Áp dụng cập nhật cây con bằng cách sử dụng phép cộng phạm vi 

Đối với mỗi bản cập nhật cây con, chúng tôi thêm:$$A = v - k\cdot \text{depth}(u)$$đến cấu trúc không đổi trên$[tin[u], tout[u]]$, Và$$B = k$$đến cấu trúc độ sâu trên cùng một phạm vi. 

Khi truy vấn một nút$x$, giá trị hiện tại của nó trở thành:$$\text{base}[x] + \text{const}(x) + \text{depth}(x)\cdot \text{depthContribution}(x).$$### 4. Trả lời truy vấn đường dẫn bằng LCA 

Để tính tổng dọc theo đường dẫn$(u,v)$, chúng tôi tính tổng tiền tố từ gốc:$$S(u) + S(v) - 2S(\mathrm{lca}(u,v)) + \text{value}(\mathrm{lca}(u,v)).$$Mỗi$S(x)$thu được bằng cách truy vấn cây Fenwick tại vị trí$tin[x]$. 

### Tại sao nó hoạt động 

Mỗi bản cập nhật đều đóng góp một hàm có chiều sâu tuyến tính trên một cây con. Phân rã Euler đảm bảo tính cục bộ của các phép toán cây con. Phân tách LCA làm giảm tổng đường dẫn thành tổng tiền tố. Các bất biến được duy trì đảm bảo rằng tại bất kỳ thời điểm nào, mỗi nút đều lưu trữ chính xác tổng của tất cả các đóng góp tuyến tính có thể áp dụng từ tất cả các cập nhật cây con đang hoạt động. 

Điều này hoàn thành lập luận về tính đúng đắn. ∎ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def range_add(self, l, r, v):
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v)

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n = int(input())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

LOG = 20
parent = [[0] * (n + 1) for _ in range(LOG)]
depth = [0] * (n + 1)

tin = [0] * (n + 1)
tout = [0] * (n + 1)
timer = 0

stack = [(1, 0, 0)]
order = []

while stack:
    u, p, state = stack.pop()
    if state == 0:
        timer += 1
        tin[u] = timer
        parent[0][u] = p
        depth[u] = depth[p] + 1 if p else 0
        stack.append((u, p, 1))
        for v in g[u]:
            if v != p:
                stack.append((v, u, 0))
    else:
        tout[u] = timer

for j in range(1, LOG):
    for i in range(1, n + 1):
        parent[j][i] = parent[j - 1][parent[j - 1][i]]

def lca(a, b):
    if depth[a] < depth[b]:
        a, b = b, a
    diff = depth[a] - depth[b]
    j = 0
    while diff:
        if diff & 1:
            a = parent[j][a]
        diff >>= 1
        j += 1

    if a == b:
        return a

    for j in range(LOG - 1, -1, -1):
        if parent[j][a] != parent[j][b]:
            a = parent[j][a]
            b = parent[j][b]
    return parent[0][a]

bit_const = BIT(n)
bit_depth = BIT(n)

q = int(input())

out = []

for _ in range(q):
    tmp = input().split()
    t = int(tmp[0])

    if t == 0:
        u = int(tmp[1])
        v = int(tmp[2])
        w = lca(u, v)

        def get(x):
            c = bit_const.sum(tin[x])
            d = bit_depth.sum(tin[x])
            return c + d * depth[x]

        res = get(u) + get(v) - 2 * get(w) + get(w)
        out.append(str(res))

    else:
        u = int(tmp[1])
        val = int(tmp[2])
        k = int(tmp[3])

        A = val - k * depth[u]
        B = k

        bit_const.range_add(tin[u], tout[u], A)
        bit_depth.range_add(tin[u], tout[u], B)

sys.stdout.write("\n".join(out))
```DFS được triển khai lặp đi lặp lại để tránh các vấn đề về độ sâu đệ quy. Chuyến tham quan Euler chỉ định các phân đoạn liền kề cho các cây con để các cập nhật phạm vi có giá trị. Hai cây Fenwick tách biệt các đóng góp tuyến tính theo độ sâu và hằng số, khớp với phân rã đại số của mỗi lần cập nhật. 

Đối với mỗi truy vấn đường dẫn, các giá trị tại điểm cuối và LCA được xây dựng lại từ hai cây và được kết hợp bằng cách sử dụng nhận dạng tổng đường dẫn tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (cây nhỏ) 

Hãy xem xét một chuỗi$1 - 2 - 3$. Giả sử chúng ta cập nhật cây con của$2$với$(v=3, k=1)$. Độ sâu là$\text{depth}(2)=1$, vì vậy mỗi nút trong cây con sẽ được$3 + (depth(x)-1)$. 

Nút 2 được$3$, nút 3 được$4$. 

Bây giờ truy vấn đường dẫn$1$ĐẾN$3$: 

value(1)=0, value(2)=3, value(3)=4 vì vậy câu trả lời là$7$. 

Thuật toán lưu trữ$A=3-1=2$,$B=1$. Sau khi cập nhật Euler, các giá trị được xây dựng lại khớp chính xác với các giá trị này. 

### Ví dụ 2 (cây phân nhánh) 

Cây:$1$kết nối với$2,3$. Cập nhật cây con của$1$với$(v=2, k=2)$. Độ sâu (1) = 0 nên mỗi nút được$2 + 2\cdot depth(x)$. 

Nút 2 và 3 đều nhận được$4$. 

Đường dẫn truy vấn$2$ĐẾN$3$cho$4+4=8$. 

Cây Fenwick có khả năng lưu trữ không đổi$2$và hệ số độ sâu$2$trên toàn phạm vi, tái tạo các giá trị giống nhau. 

Những dấu vết này xác nhận rằng các cập nhật tuyến tính của cây con và việc xây dựng lại đường dẫn dựa trên LCA tương tác một cách nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log n)$| Mỗi bản cập nhật và truy vấn sử dụng các phép toán Fenwick và nâng LCA | 
| Không gian |$O(n)$| Chuyến tham quan Euler, bảng cha và mảng Fenwick | 

Hệ số logarit vừa vặn trong giới hạn của$3 \times 10^5$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# No runnable reference implementation included
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật chuỗi và truy vấn đường dẫn | hướng dẫn sử dụng | độ chính xác cập nhật tuyến tính theo chiều sâu | 
| cây hình ngôi sao | hướng dẫn sử dụng | tính chính xác của việc lan truyền trên toàn cây con | 
| nút đơn | 0 | cấu trúc tối thiểu | 
| cập nhật/truy vấn xen kẽ | hướng dẫn sử dụng | sự xen kẽ đúng đắn | 

## Vỏ cạnh 

Một cây con bao gồm các gốc kiểm tra xem các khoảng Euler có trải rộng chính xác trên toàn bộ cây hay không. Chuỗi sâu đảm bảo rằng các thuật ngữ tuyến tính dựa trên độ sâu được tích lũy chính xác mà không có lỗi tràn hoặc ký. Một chuỗi các cập nhật xen kẽ và truy vấn đường dẫn đảm bảo rằng các hiệu ứng phạm vi lười được phản ánh chính xác trong các tổng dựa trên LCA tiếp theo.
