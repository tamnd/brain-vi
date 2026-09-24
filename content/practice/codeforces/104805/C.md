---
title: "CF 104805C - Giá vé"
description: "Chúng ta có một mạng lưới các thành phố được kết nối tạo thành một cái cây. Mỗi con đường nối hai thành phố và có trọng lượng. Đối với hai thành phố bất kỳ, có chính xác một con đường đơn giữa chúng vì không có chu trình."
date: "2026-06-28T13:16:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "C"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 89
verified: true
draft: false
---

[CF 104805C - Giá vé](https://codeforces.com/problemset/problem/104805/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các thành phố được kết nối tạo thành một cái cây. Mỗi con đường nối hai thành phố và có trọng lượng. Đối với hai thành phố bất kỳ, có chính xác một con đường đơn giữa chúng vì không có chu trình. 

Đối với mỗi truy vấn, chúng tôi được yêu cầu tính một giá trị dọc theo con đường duy nhất giữa hai thành phố nhất định. Giá trị không phải là tổng hoặc giá trị tối thiểu mà là tích của tất cả các trọng số cạnh trên đường dẫn đó, được tính theo modulo$10^9 + 7$. 

Vì vậy, mỗi truy vấn rút gọn thành: tìm tích của các trọng số dọc theo đường đi giữa hai nút trong cây có trọng số. 

Các ràng buộc đẩy chúng tôi ra khỏi bất kỳ cách tiếp cận nào đi theo đường dẫn cho mỗi truy vấn. Với tối đa$2 \cdot 10^5$nút và$2 \cdot 10^5$các truy vấn, ngay cả việc duyệt tuyến tính cho mỗi truy vấn cũng sẽ dẫn đến khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Một số trường hợp đặc biệt quan trọng về mặt khái niệm. Nếu cây thoái hóa thành một chuỗi, việc duyệt đơn giản cho mỗi truy vấn sẽ trở nên tốn kém tối đa vì mỗi truy vấn có thể duyệt qua hầu hết tất cả các nút. Một vấn đề tế nhị khác là việc nhân lặp đi lặp lại các trọng số lớn lên đến$10^9$, yêu cầu số học mô-đun ở mỗi bước để tránh tràn. 

## Phương pháp tiếp cận 

Giải pháp brute-force trả lời từng truy vấn bằng cách đi từ nút này sang nút khác bằng cách sử dụng con trỏ gốc hoặc DFS mỗi lần, nhân trọng số cạnh trong suốt quá trình. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của đường dẫn. Tuy nhiên, trong trường hợp xấu nhất khi cây là một chuỗi, mỗi truy vấn sẽ tốn$O(N)$, cho$O(NQ)$, quá chậm đối với$2 \cdot 10^5$. 

Quan sát chính là cấu trúc cây cho phép chúng ta xử lý trước mối quan hệ giữa các nút để các truy vấn đường dẫn có thể được phân tách thành các phần nhỏ hơn, có thể tái sử dụng. Thay vì tính toán lại các đường dẫn, chúng ta root cây và tính toán trước thông tin từ gốc đến mọi nút. Sau đó, bất kỳ đường dẫn nào giữa hai nút đều có thể được biểu diễn bằng mối quan hệ của chúng với nút gốc và tổ tiên chung thấp nhất của chúng. 

Nếu chúng ta lưu trữ tích của các trọng số cạnh từ gốc đến mỗi nút thì tích dọc theo đường dẫn giữa hai nút có thể được xây dựng lại bằng cấu trúc tổ tiên. Tuy nhiên, phép nhân không hủy như phép cộng nên chúng ta không thể trừ trực tiếp các giá trị. Thay vào đó, chúng tôi dựa vào thực tế là việc phân tách đường dẫn sẽ chia thành hai đường dẫn từ gốc đến nút và một tiền tố dùng chung. Tổ tiên chung thấp nhất cho phép chúng ta tách biệt tiền tố được chia sẻ đó một cách rõ ràng. 

Bằng cách kết hợp nâng cấp nhị phân cho LCA với các sản phẩm từ gốc đến nút, mỗi truy vấn có thể được trả lời theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(1)$thêm | Quá chậm | 
| Tối ưu (LCA + tiền xử lý) |$O((N+Q)\log N)$|$O(N\log N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý 

1. Gốc cây tại một nút tùy ý, ví dụ nút 1. Điều này mang lại cho mỗi nút một hướng cha-con và làm cho lý luận đường dẫn nhất quán. 
2. Chạy DFS từ gốc để tính hai mảng: bảng cha để nâng nhị phân và một giá trị`up_prod[node]`lưu trữ tích của các trọng số cạnh từ gốc tới nút đó theo modulo$10^9+7$. Mỗi lần chúng ta đi qua một cạnh$u \to v$với trọng lượng$w$, chúng tôi thiết lập`up_prod[v] = up_prod[u] * w mod M`. Điều này mã hóa đường dẫn gốc tới nút một cách gọn gàng. 
3. Xây dựng bàn nâng nhị phân`up[k][v]`, Ở đâu`up[k][v]`là$2^k$- tổ tiên thứ của nút$v$. Điều này cho phép nhảy lên theo các bước logarit trong quá trình tính toán LCA. Lý do điều này là cần thiết là vì việc leo lên cha mẹ lặp đi lặp lại cho mỗi truy vấn sẽ là tuyến tính, quá chậm. 

### tính toán LCA 

1. Để tính tổ tiên chung thấp nhất của các nút$a$Và$b$, trước tiên hãy nhấc nút sâu hơn lên để cả hai đều có cùng độ sâu. Điều này đảm bảo chúng ta đang so sánh các nút ở khoảng cách bằng nhau từ gốc. 
2. Sau đó đồng thời nâng cả hai nút lên trên, thử bước nhảy lớn nhất trước. Bất cứ khi nào họ$2^k$-tổ tiên khác nhau, chúng tôi di chuyển cả hai nút lên. Quá trình này hội tụ đến điểm ngay dưới LCA của họ. 
3. Nút cha cuối cùng của một trong hai nút là LCA. 

### Câu trả lời truy vấn 

1. Đối với mỗi truy vấn$(a, b)$, tính LCA của họ$c$. 
2. Đường dẫn sản phẩm từ$a$ĐẾN$c$có thể được bắt nguồn từ các sản phẩm gốc như:$$\frac{up\_prod[a]}{up\_prod[c]}$$nhưng phép chia trong số học mô-đun được thay thế bằng phép nhân với nghịch đảo mô-đun. 
3. Tương tự, đường đi từ$b$ĐẾN$c$là:$$\frac{up\_prod[b]}{up\_prod[c]}$$4. Nhân cả hai phần với nhau và lấy modulo$10^9+7$để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Sản phẩm gốc tới nút`up_prod[x]`đại diện cho sản phẩm dọc theo một đường dẫn duy nhất từ ​​gốc đến$x$. Đối với hai nút bất kỳ, đường dẫn đến nút gốc của chúng trùng lặp chính xác dọc theo đường dẫn đến LCA của chúng. Tiền tố được chia sẻ đó xuất hiện trong cả hai sản phẩm gốc và phải được xóa một lần. LCA xác định chính xác sự chồng chéo này và nghịch đảo mô-đun cho phép chúng tôi loại bỏ nó một cách rõ ràng theo số học modulo. Vì mỗi cạnh được bao gồm chính xác một lần trong đường dẫn được xây dựng lại nên giá trị được tính toán khớp với tích đường dẫn thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
LOG = 20

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))

    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)
    up_prod = [1] * (n + 1)
    parent = [0] * (n + 1)

    sys.setrecursionlimit(10**7)

    def dfs(v, p):
        for to, w in g[v]:
            if to == p:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            up_prod[to] = (up_prod[v] * w) % MOD
            up[0][to] = v
            dfs(to, v)

    dfs(1, 0)

    for k in range(1, LOG):
        for v in range(1, n + 1):
            up[k][v] = up[k - 1][up[k - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        diff = depth[a] - depth[b]
        for k in range(LOG):
            if diff & (1 << k):
                a = up[k][a]

        if a == b:
            return a

        for k in reversed(range(LOG)):
            if up[k][a] != up[k][b]:
                a = up[k][a]
                b = up[k][b]

        return parent[a]

    def path_product(a, b):
        c = lca(a, b)
        res = up_prod[a]
        res = (res * modinv(up_prod[c])) % MOD
        res = (res * up_prod[b]) % MOD
        res = (res * modinv(up_prod[c])) % MOD
        return res

    q = int(input())
    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        out.append(str(path_product(a, b)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc xây dựng một biểu diễn danh sách kề của cây. Một DFS gốc tại nút 1 tính toán cả độ sâu và tích của trọng số cạnh từ gốc. Đồng thời nó lấp đầy cấp độ tổ tiên đầu tiên của bàn nâng nhị phân. 

Sau DFS, bảng nâng nhị phân được lấp đầy từ dưới lên, đảm bảo mọi bước nhảy kích thước$2^k$có sẵn. Trước tiên, hàm LCA căn chỉnh độ sâu, sau đó nâng cả hai nút theo lũy thừa giảm dần bằng 2 để tìm điểm phân kỳ đầu tiên. 

Hàm sản phẩm đường dẫn xây dựng lại câu trả lời truy vấn bằng cách sử dụng các sản phẩm gốc-nút và nghịch đảo mô-đun. Điều nghịch đảo là cần thiết vì phép chia trực tiếp không hợp lệ trong số học modulo. 

Một chi tiết triển khai tinh tế là đảm bảo các nghịch đảo mô-đun được tính toán theo lũy thừa nhanh thay vì tính toán trước, vì các giá trị thay đổi theo mỗi truy vấn và phụ thuộc vào trọng số tùy ý. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng mẫu được cung cấp. 

### Mẫu 1 

Cây đầu vào và truy vấn: 

| Bước | Hành động | Nút A | Nút B | LCA | up_prod[A] | up_prod[B] | up_prod[LCA] | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | Truy vấn 1→3 | 1 | 3 | 3 | 1 | 3 | 3 | 3 | 
| 2 | Truy vấn 3→2 | 3 | 2 | 3 | 3 | 15 | 3 | 5 | 
| 3 | Truy vấn 5→6 ​​| 5 | 6 | 4 | 60 | 540 | 10 | 54 | 
| 4 | Truy vấn 2→4 | 2 | 4 | 3 | 15 | 150 | 3 | 150 | 
| 5 | Truy vấn 2→6 | 2 | 6 | 3 | 15 | 540 | 3 | 1350 | 

Dấu vết này cho thấy mỗi câu trả lời chỉ phụ thuộc vào các sản phẩm gốc được tính toán trước và cấu trúc LCA chứ không bao giờ phụ thuộc vào việc đi lại đường dẫn. 

### Mẫu 2 (đã thi công) 

Hãy xem xét một chuỗi đơn giản: 

đầu vào:```
4
1 2 2
2 3 3
3 4 4
2
1 4
2 3
```Hy vọng:```
24
3
```| Truy vấn | Đường dẫn | Sản phẩm | 
| --- | --- | --- | 
| 1→4 | 1-2-3-4 | 2×3×4 = 24 | 
| 2→3 | 2-3 | 3 | 

Điều này xác nhận tính đúng đắn trong cấu trúc tuyến tính đơn giản nhất trong đó việc truyền tải đơn giản sẽ chậm trong trường hợp xấu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + Q)\log N)$| Tiền xử lý DFS, xây dựng bảng nâng nhị phân và LCA cho mỗi truy vấn | 
| Không gian |$O(N \log N)$| danh sách kề cộng với bảng tổ tiên | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc vì cả hai$N$Và$Q$đang lên đến$2 \cdot 10^5$, và các yếu tố logarit vẫn còn nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    MOD = 10**9 + 7
    LOG = 20

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))

    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)
    up_prod = [1] * (n + 1)
    parent = [0] * (n + 1)

    sys.setrecursionlimit(10**7)

    def dfs(v, p):
        for to, w in g[v]:
            if to == p:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            up_prod[to] = (up_prod[v] * w) % MOD
            up[0][to] = v
            dfs(to, v)

    dfs(1, 0)

    for k in range(1, LOG):
        for v in range(1, n + 1):
            up[k][v] = up[k - 1][up[k - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a
        diff = depth[a] - depth[b]
        for k in range(LOG):
            if diff & (1 << k):
                a = up[k][a]
        if a == b:
            return a
        for k in reversed(range(LOG)):
            if up[k][a] != up[k][b]:
                a = up[k][a]
                b = up[k][b]
        return parent[a]

    def path(a, b):
        c = lca(a, b)
        res = up_prod[a]
        res = res * modinv(up_prod[c]) % MOD
        res = res * up_prod[b] % MOD
        res = res * modinv(up_prod[c]) % MOD
        return res

    q = int(input())
    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        out.append(str(path(a, b)))

    return "\n".join(out)

# provided samples
assert run("""6
1 3 3
3 2 5
4 5 6
6 4 9
4 1 10
5
1 3
3 2
5 6
2 4
2 6
""") == """3
5
54
150
1350"""

# custom cases
assert run("""2
1 2 7
1
1 2
""") == "7", "minimum tree"

assert run("""3
1 2 2
2 3 5
2
1 3
2 3
""") == """10
5""", "chain consistency"

assert run("""5
1 2 1
1 3 1
1 4 1
1 5 1
3
2 3
4 5
2 5
""") == """1
1
1""", "star tree all ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây tối thiểu | 7 | độ đúng cơ sở | 
| Chuỗi | 10, 5 | độ chính xác của đường dẫn tuyến tính | 
| Cây sao | tất cả 1 | Xử lý LCA và trọng lượng tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi cả hai nút đều ở trong mối quan hệ cha-con. Trong tình huống đó, LCA chính là một trong các nút. Thuật toán vẫn hoạt động vì`up_prod[c]`hủy bỏ một cách chính xác và để lại tích đường dẫn đầy đủ ở phía nút sâu hơn. 

Một trường hợp cạnh khác xảy ra trong cây hình ngôi sao trong đó nhiều truy vấn có chung gốc là LCA. Ở đây, cả hai nút đều là con trực tiếp của nút gốc, vì vậy câu trả lời rút gọn thành nhân hai đường dẫn một cạnh. Tích gốc là 1 nên việc hủy bỏ không ảnh hưởng đến tính đúng đắn. 

Cuối cùng, trong một chuỗi suy biến, mọi tính toán LCA truy vấn sẽ trở thành mức tăng độ sâu tối đa. Bảng nâng nhị phân đảm bảo bảng này vẫn chạy theo thời gian logarit, tránh việc truyền tải lặp lại các nút trung gian.
