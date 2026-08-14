---
title: "CF 102331D - Yếu tố quyết định"
description: "Chúng ta có một đồ thị vô hướng liên thông và cần định thức của ma trận kề modulo (998244353). Đồ thị có tới (25.000) đỉnh và (500.000) cạnh, nhưng điều kiện bất thường liên quan đến (k+1) đỉnh là ràng buộc cấu trúc thực sự."
date: "2026-08-13T03:33:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "D"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 188
verified: true
draft: false
---

[CF 102331D - Yếu tố quyết định](https://codeforces.com/problemset/problem/102331/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông và cần định thức của ma trận kề modulo (998244353). Đồ thị có tới (25.000) đỉnh và (500.000) cạnh, nhưng điều kiện bất thường liên quan đến (k+1) đỉnh là ràng buộc cấu trúc thực sự. Tuyên bố chính thức đưa ra (k\le 25) và hướng dẫn cuộc thi xác định cấu trúc tương đương có các thành phần được kết nối hai cạnh có kích thước tối đa (k). 

Định thức của ma trận kề có thể được xem tổ hợp thông qua công thức Leibniz. Mọi số hạng khác 0 tương ứng với một hoán vị của các đỉnh trong đó mỗi đỉnh được gửi tới một đỉnh liền kề. Việc phân tách hoán vị đó thành các chu trình sẽ phân rã tất cả các đỉnh thành các chu trình có hướng và dấu của một chu trình có độ dài (l) là ((-1)^{l-1}). 

Quan sát biểu đồ chính là về những cây cầu. Nếu một cạnh là cầu thì không chu trình nào có thể chứa cạnh đó. Do đó, một cây cầu chỉ có thể xảy ra trong một chu kỳ hoán vị dưới dạng 2 chu kỳ bao gồm hai điểm cuối của nó. Sau khi loại bỏ tất cả các cầu nối, mọi thành phần được kết nối còn lại không có cầu nối bên trong, do đó các thành phần này tạo thành các phần nhỏ tự nhiên của biểu đồ. 

Tại sao những mảnh đó lại nhỏ? Giả sử đồ thị thu được sau khi xóa tất cả các cây cầu có thành phần chứa ít nhất (k+1) đỉnh. Chọn bất kỳ đỉnh (k+1) nào từ nó. Không có cây cầu nào ngăn cách hai đỉnh trong số này, bởi vì chúng vẫn được kết nối sau khi mọi cây cầu bị loại bỏ. Điều đó mâu thuẫn với điều kiện trong tuyên bố. Ngược lại, nếu mỗi thành phần như vậy có nhiều nhất (k) đỉnh thì bất kỳ (k+1) đỉnh nào được chọn phải nằm trong ít nhất hai thành phần khác nhau và một cây cầu trên đường đi của cây thành phần sẽ phân tách một cặp phù hợp. Do đó, điều kiện đã cho chính xác là phát biểu rằng mọi thành phần được kết nối hai cạnh chứa tối đa (k\le25) đỉnh. 

Điều này thay đổi hoàn toàn quy mô của vấn đề. Định thức chung (25.000\times25.000) là quá lớn. Việc loại bỏ Gaussian sẽ yêu cầu khoảng 

[ 
\frac{n^3}{3}\approx\frac{25000^3}{3}\approx5.2\cdot10^{12} 
] 

hoạt động hiện trường trong trường hợp xấu nhất dày đặc. Ngay cả việc lưu trữ một ma trận như vậy cũng cần hàng trăm triệu mục nhập. Thay vào đó, công việc hữu ích phải diễn ra bên trong các thành phần có kích thước tối đa là 25, với một cây DP kết nối chúng. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Đối với một đỉnh duy nhất,```
1 0 1
```ma trận kề là ([0]), nên đáp án là (0). Việc coi một đồ thị trống là có định thức (1) mà không xem xét rằng thành phần thực tế chứa một đỉnh không được che sẽ cho kết quả sai. 

Đối với hai đỉnh được nối bởi một cây cầu,```
2 1 1
1 2
```ma trận kề là 

[ 
\begin{pmatrix}0&1\1&0\end{pmatrix}, 
] 

có định thức là (-1), vì vậy câu trả lời mô đun bắt buộc là (998244352). Một DP đếm cầu 2 chu kỳ nhưng quên dấu hoán vị lẻ của nó sẽ tạo ra (1). 

Đối với đường đi trên ba đỉnh,```
3 2 1
1 2
2 3
```định thức là (0). Đỉnh giữa không thể được bao phủ bởi sự phân rã chu trình. Một cây DP chỉ theo dõi xem mọi cạnh có được sử dụng hay không có thể đếm không chính xác hai cạnh là một cấu trúc hợp lệ, mặc dù các thuật ngữ xác định là các bìa chu trình chứ không phải các tập hợp con cạnh tùy ý. 

Một thành phần không có cầu nối cũng phải được xử lý mà không giả định rằng mọi thành phần đều đóng góp một định thức khác 0. Ví dụ,```
3 3 3
1 2
2 3
3 1
```có một thành phần chứa cả ba đỉnh. Ma trận kề của nó có định thức (2) và không có sự chuyển tiếp cầu nào cả. Thành phần DP phải giảm về một định thức nhỏ thông thường trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tuân theo định nghĩa của định thức. Chúng ta có thể xây dựng toàn bộ ma trận kề và chạy modulo loại bỏ Gaussian (998244353). Điều này đúng vì các phép toán hàng cơ bản bảo toàn định thức khi hiệu ứng của chúng được theo dõi. Thật không may, việc loại bỏ dày đặc (25.000\times25.000) cần khoảng (5.2\cdot10^{12}) cập nhật loại bỏ trong trường hợp xấu nhất, không bằng 5 giây có sẵn. 

Bản thân việc khai triển định thức thậm chí còn tệ hơn. Công thức Leibniz có (n!) số hạng hoán vị, vì vậy nó chỉ hữu ích như một cách để hiểu định thức có giá trị như thế nào chứ không phải là một thuật toán. 

Chế độ xem bạo lực tiết lộ cấu trúc mà chúng ta cần. Thuật ngữ xác định là sự phân rã chu trình của các đỉnh. Một cây cầu không thể thuộc về một chu trình có độ dài ít nhất là ba, vì chu trình như vậy sẽ tạo ra một đường đi khác giữa các điểm cuối của cầu. Do đó mọi cầu được sử dụng bởi số hạng xác định đều là 2 chu trình. 

Loại bỏ tất cả các cầu nối và thu gọn mọi thành phần được kết nối còn lại vào một nút. Kết quả là một cái cây. Mỗi thành phần có nhiều nhất (k) đỉnh nên chúng ta có thể xử lý cây này từ các lá đến một gốc tùy ý. Đây chính xác là cách tiếp cận DP dạng cây được mô tả trong hướng dẫn cuộc thi. 

Xét một thành phần (B) và một trong các đỉnh của nó (v). Một số thành phần con có thể được gắn vào (v) bằng cầu nối. Trong một phân tách chu trình hợp lệ, có thể sử dụng nhiều nhất một trong các cầu nối con đó, bởi vì sử dụng cầu nối có nghĩa là (v) tham gia vào chu trình 2 với điểm cuối con. 

Đối với mỗi thành phần con (C), xác định hai giá trị DP. Giá trị (dp[C][0]) có nghĩa là cầu nối từ (C) tới cha của nó không được sử dụng. Giá trị (dp[C][1]) có nghĩa là cầu đã được sử dụng, do đó đỉnh biên của (C) được loại bỏ khỏi vỏ chu trình bên trong của (C). Dấu của 2 chu kỳ cha-con được xử lý có chủ ý bởi quá trình chuyển đổi cha mẹ, vì vậy bản thân (dp[C][1]) chỉ là đóng góp xác định sau khi xóa đỉnh biên đó. Quy ước này là điều làm cho ma trận cục bộ trở nên đặc biệt sạch sẽ. 

Đối với một đỉnh (v), hãy 

[ 
s_v=\prod_C dp[C][0], 
] 

trong đó (C) bao trùm tất cả các thành phần con gắn liền với (v). Nếu không có cầu con nào được sử dụng, (v) vẫn ở bên trong thành phần hiện tại và nhận trọng số (s_v). 

Nếu sử dụng chính xác một cầu con thì mức đóng góp là 

[ 
t_v=\sum_C dp[C][1]\prod_{D\ne C}dp[D][0]. 
] 

Chúng tôi tính toán kết quả này mà không cần chia, vì một số (dp[C][0]) có thể bằng 0. Bắt đầu với`prod = 1`Và`t = 0`, mỗi đứa trẻ cập nhật 

[ 
t\leftarrow t\cdot dp[C][0]+prod\cdot dp[C][1], 
] 

theo sau là 

[ 
prod\leftarrow prod\cdot dp[C][0]. 
] 

Bây giờ hãy để (A) là ma trận kề thông thường của thành phần kết nối hai cạnh hiện tại. Chúng tôi thay thế nó bằng một ma trận có trọng số nhỏ (B) có cùng kích thước: 

[ 
B_{vv}=-t_v, 
] 

và đối với cạnh trong (v\mathord{-}u), 

[ 
B_{vu}=s_v. 
] 

Hai hướng có thể có trọng số khác nhau, điều này tốt vì chỉ định thức được sử dụng. Mở rộng định thức của (B), chọn mục nhập chéo (-t_v) có nghĩa là (v) sử dụng một cầu con. Việc chọn các hàng còn lại thông qua các cạnh trong sẽ để lại định thức của ma trận kề trên các đỉnh không bị loại bỏ. Dấu trừ chính là dấu của cầu 2 chu kỳ. 

Do đó (dp[B][0]=\det B). 

Nếu (p) là đỉnh của (B) liên quan đến cầu mẹ thì (dp[B][1]) phải loại bỏ (p) khỏi thành phần bên trong. Tất cả các phần tử con gắn trực tiếp với (p) phải sử dụng loại 0, đóng góp (s_p). Do đó 

[ 
dp[B][1]=s_p\det B_{\setminus p}, 
] 

trong đó (B_{\setminus p}) thu được bằng cách xóa hàng và cột (p). 

Mỗi định thức bây giờ lớn nhất là (25\times25). Vì tổng kích thước thành phần là (n), nên tổng chi phí là (O(nk^2)), phù hợp với độ phức tạp dự định đã nêu trong hướng dẫn cuộc thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Yếu tố quyết định Brute Force | (O(n^3)) với việc loại bỏ Gaussian hoặc (O(n!)) từ khai triển Leibniz | (O(n^2)) cho ma trận dày đặc | Quá chậm | 
| DP thành phần cầu tối ưu | (O(m+n k^2)) | (O(m+n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm mọi cây cầu của biểu đồ gốc bằng DFS liên kết thấp. Một cạnh (u-v) là một cây cầu chính xác khi cây con DFS của (v) không có cạnh sau nào tiếp cận (u) hoặc tổ tiên của (u), tương đương`low[v] > tin[u]`. Việc triển khai sử dụng DFS lặp để đường dẫn (25.000) đỉnh không phụ thuộc vào độ sâu đệ quy của Python. 
2. Loại bỏ tất cả các cầu nối về mặt khái niệm và tìm các thành phần được kết nối còn lại. Đây là những thành phần được kết nối hai cạnh. Điều kiện đã cho đảm bảo rằng mọi thành phần đều có nhiều nhất (k) đỉnh. 
3. Xây dựng cây thành phần. Mỗi cây cầu ban đầu kết nối hai thành phần khác nhau và vì các cây cầu không thể tạo thành một chu trình nên việc thu gọn mọi thành phần sẽ tạo ra một cây. 
4. Root cây thành phần tùy ý. Đối với mọi thành phần không phải gốc, hãy nhớ điểm cuối của cầu mẹ nằm bên trong thành phần đó. Đỉnh này là đỉnh biên phải biến mất khi thành phần sử dụng cầu mẹ của nó. 
5. Xử lý các thành phần theo thứ tự cây đảo ngược. Đối với mỗi đỉnh (v) của thành phần hiện tại, hãy kết hợp tất cả các thành phần con đã được tính toán gắn với (v). Tính (s_v) là tích của tất cả các giá trị con loại 0. 
6. Tính (t_v) là tổng các cấu hình trong đó có đúng một con sử dụng cầu nối của nó. Sự tái diễn`t = t * dp0 + prod * dp1`thêm đứa trẻ mới vào đứa trẻ được chọn duy nhất hoặc giữ nguyên lựa chọn trước đó. Nó tránh sự chia cho số 0 (dp0). 
7. Xây dựng ma trận cục bộ (B). Đặt (-t_v) trên đường chéo của nó. Với mọi cạnh trong từ (v) đến (u), đặt (s_v) tại vị trí ((v,u)). Trọng số hàng (s_v) thể hiện sự đóng góp của tất cả các cây con khi (v) vẫn còn trong thành phần. 
8. Tính toán (\det B) bằng cách loại bỏ Gaussian mô-đun. Giá trị này là (dp[B][0]), bởi vì mọi thuật ngữ xác định đều chọn tùy chọn đường chéo cho một đỉnh đi qua cầu con hoặc một cạnh trong cho một đỉnh vẫn còn trong thành phần. 
9. Đối với mọi thành phần không phải gốc, hãy xóa đỉnh biên cha của nó khỏi (B), tính định thức thu được và nhân nó với (s_p). Điều này mang lại (dp[B][1]). Thành phần cha sẽ cung cấp dấu trừ khi nó sử dụng cầu làm chu kỳ 2. 
10. Sau khi thành phần gốc đã được xử lý, giá trị loại 0 của nó là định thức của toàn bộ ma trận kề ban đầu, do đó xuất ra nó theo modulo (998244353). 

Tại sao nó hoạt động: mọi số hạng xác định đều là một phép phân rã chu trình đỉnh. Bên trong một thành phần được kết nối hai cạnh, tất cả các chu trình đều là nội bộ trừ khi một đỉnh tham gia vào chu trình 2 cầu. Một cây cầu chỉ có thể được sử dụng trong 2 chu kỳ như vậy và một đỉnh có thể thuộc nhiều nhất một chu trình, do đó mỗi đỉnh có thể vẫn nằm trong thành phần của nó hoặc kết nối với chính xác một thành phần con. Các giá trị (s_v) và (t_v) liệt kê chính xác hai khả năng này. Định thức cục bộ liệt kê tất cả các phạm vi chu trình nội bộ tương thích, trong khi các giá trị DP đệ quy đã tính đến mọi thành phần con cháu. Vì đồ thị thành phần là một cây nên mọi phân rã chu trình xác định có chính xác một phân tách đệ quy và được tính một lần với dấu đúng của nó. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 998244353

def determinant(a):
    n = len(a)
    if n == 0:
        return 1

    ans = 1

    for col in range(n):
        pivot = col
        while pivot < n and a[pivot][col] == 0:
            pivot += 1

        if pivot == n:
            return 0

        if pivot != col:
            a[pivot], a[col] = a[col], a[pivot]
            ans = -ans

        p = a[col][col] % MOD
        ans = ans * p % MOD
        inv = pow(p, MOD - 2, MOD)

        row = a[col]

        for r in range(col + 1, n):
            x = a[r][col]
            if x == 0:
                continue

            factor = x * inv % MOD
            rr = a[r]
            rr[col] = 0

            for c in range(col + 1, n):
                rr[c] = (rr[c] - factor * row[c]) % MOD

    return ans % MOD

def solve():
    n, m, k = map(int, input().split())

    head = array('i', [-1]) * n
    to = array('i')
    nxt = array('i')
    eu = array('i')
    ev = array('i')

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        eid = len(eu)
        eu.append(u)
        ev.append(v)

        to.append(v)
        nxt.append(head[u])
        head[u] = len(to) - 1

        to.append(u)
        nxt.append(head[v])
        head[v] = len(to) - 1

    # Find bridges with iterative low-link DFS.
    tin = array('i', [0]) * n
    low = array('i', [0]) * n
    parent = array('i', [-1]) * n
    parent_edge = array('i', [-1]) * n
    it = array('i', head)
    bridge = array('b', [0]) * m

    timer = 0
    stack = [0]
    timer += 1
    tin[0] = low[0] = timer

    while stack:
        v = stack[-1]
        e = it[v]

        if e == -1:
            stack.pop()

            p = parent[v]
            if p != -1:
                pe = parent_edge[v]
                if low[v] > tin[p]:
                    bridge[pe >> 1] = 1
                if low[v] < low[p]:
                    low[p] = low[v]
            continue

        it[v] = nxt[e]

        if e == parent_edge[v]:
            continue

        u = to[e]

        if tin[u] == 0:
            parent[u] = v
            parent_edge[u] = e ^ 1
            timer += 1
            tin[u] = low[u] = timer
            stack.append(u)
        else:
            if tin[u] < low[v]:
                low[v] = tin[u]

    # Build edge-biconnected components by ignoring bridges.
    comp = array('i', [-1]) * n
    blocks = []
    pos = array('i', [0]) * n

    cid = 0
    for start in range(n):
        if comp[start] != -1:
            continue

        vertices = []
        stack = [start]
        comp[start] = cid

        while stack:
            v = stack.pop()
            pos[v] = len(vertices)
            vertices.append(v)

            e = head[v]
            while e != -1:
                if not bridge[e >> 1]:
                    u = to[e]
                    if comp[u] == -1:
                        comp[u] = cid
                        stack.append(u)
                e = nxt[e]

        blocks.append(vertices)
        cid += 1

    bc = cid

    # Component tree.
    tree = [[] for _ in range(bc)]

    for e in range(m):
        if not bridge[e]:
            continue

        u = eu[e]
        v = ev[e]
        cu = comp[u]
        cv = comp[v]

        tree[cu].append((cv, u, v))
        tree[cv].append((cu, v, u))

    # Root the component tree.
    parent_block = array('i', [-2]) * bc
    parent_block[0] = -1

    parent_vertex = array('i', [-1]) * bc
    order = [0]

    for c in order:
        for d, vc, vd in tree[c]:
            if parent_block[d] != -2:
                continue

            parent_block[d] = c
            parent_vertex[d] = vd
            order.append(d)

    # For every local vertex, store its child components.
    children_at = []
    for vertices in blocks:
        children_at.append([[] for _ in vertices])

    for c in range(1, bc):
        p = parent_block[c]

        # Find the edge p-c and its endpoint inside p.
        for d, vp, vc in tree[p]:
            if d == c:
                children_at[p][pos[vp]].append(c)
                break

    dp0 = array('i', [0]) * bc
    dp1 = array('i', [0]) * bc

    # Process bottom-up.
    for c in reversed(order):
        vertices = blocks[c]
        s = len(vertices)

        sv = [1] * s
        tv = [0] * s

        for i in range(s):
            prod = 1
            chosen = 0

            for child in children_at[c][i]:
                a = dp0[child]
                b = dp1[child]

                chosen = (chosen * a + prod * b) % MOD
                prod = prod * a % MOD

            sv[i] = prod
            tv[i] = chosen

        # B[i][i] = -t_i
        # B[i][j] = s_i for an internal edge i-j.
        mat = [[0] * s for _ in range(s)]

        for i in range(s):
            mat[i][i] = (-tv[i]) % MOD

        for i, v in enumerate(vertices):
            e = head[v]
            while e != -1:
                if comp[to[e]] == c:
                    j = pos[to[e]]
                    mat[i][j] = sv[i]
                e = nxt[e]

        dp0[c] = determinant([row[:] for row in mat])

        if c != 0:
            boundary = pos[parent_vertex[c]]
            reduced = []

            for i in range(s):
                if i == boundary:
                    continue

                row = []
                for j in range(s):
                    if j == boundary:
                        continue
                    row.append(mat[i][j])
                reduced.append(row)

            minor = determinant(reduced)
            dp1[c] = sv[boundary] * minor % MOD

    print(dp0[0] % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai lưu trữ biểu đồ trong mảng sao chuyển tiếp. các`array`mô-đun giữ cho biểu diễn kề cận nhỏ gọn, điều này quan trọng vì câu lệnh cho phép lên tới (500.000) cạnh đầu vào mặc dù lời hứa về cấu trúc khiến cho các đầu vào dày đặc như vậy không thể thực hiện được theo (k\le25) đã nêu. Việc biểu diễn vẫn mạnh mẽ so với giới hạn đầu vào thô. 

Cầu DFS giữ`tin[v]`Và`low[v]`. Đối với cạnh vô hướng, cạnh ngược của cạnh cây DFS bị bỏ qua một cách rõ ràng. Khi trẻ học xong,`low[child] > tin[parent]`xác định một cây cầu. 

Sau khi đã biết các cây cầu, một phép duyệt khác sẽ gán mỗi đỉnh cho thành phần được kết nối với cây cầu của nó. các`pos`mảng ánh xạ một đỉnh ban đầu tới chỉ mục cục bộ của nó bên trong thành phần của nó, do đó các ma trận xác định nhỏ có thể được xây dựng mà không cần từ điển. 

Cây thành phần được root lặp đi lặp lại. Đối với mỗi thành phần,`children_at[i]`chứa chính xác các thành phần con có điểm cuối cầu nằm ở đỉnh cục bộ`i`. Đây là thông tin cần thiết cho quá trình chuyển đổi (s_v,t_v). 

Sự tái diễn```
chosen = (chosen * a + prod * b) % MOD
prod = prod * a % MOD
```được viết có chủ ý mà không có sự phân chia theo mô-đun. Việc triển khai hấp dẫn sẽ tính toán (t_v=s_v\sum dp_1/dp_0), nhưng (dp_0) có thể bằng 0. Phép truy hồi tích tiền tố vẫn hợp lệ trong mọi trường hợp. 

Ma trận cục bộ chỉ là (s\times s), không phải (2s\times2s). Việc xây dựng đỉnh phụ từ bài xã luận ban đầu có thể được loại bỏ về mặt đại số. Hàng (v) được nhân với (s_v), trong khi việc chọn kết nối con sẽ cho giá trị đường chéo (-t_v). Điều này tạo ra ma trận compact (B) được mô tả ở trên và cắt giảm hệ số không đổi một cách đáng kể. Hướng dẫn ban đầu mô tả cấu trúc đỉnh giả tưởng tương đương và đưa ra công việc (O(k^3)) cho mỗi thành phần. 

Phép loại bỏ Gaussian tìm kiếm một trục quay khác 0 vì trục quay bằng 0 không nhất thiết có nghĩa là định thức bằng 0 cho đến khi mọi hàng bên dưới cột đó được kiểm tra. Hoán đổi hàng sẽ lật dấu định thức, trong khi việc cộng bội số của hàng này vào hàng khác sẽ không làm thay đổi dấu đó. Mọi phép toán số học đều được giảm modulo (998244353), do đó số nguyên Python không bao giờ đủ lớn để gây ra sự cố tràn. 

Ma trận trống xảy ra khi một thành phần có một đỉnh biên và đỉnh đó bị xóa. Định thức của nó là (1), đây là quy ước tích rỗng tiêu chuẩn và được yêu cầu cho thành phần lá bao gồm một đỉnh duy nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là đường dẫn```
1 - 2 - 3 - 4
```với (k=1). Mỗi cạnh là một cầu nối, vì vậy mỗi thành phần được kết nối hai cạnh đều chứa một đỉnh. 

Gốc cây thành phần ở đỉnh 1. 

| Thành phần | Đỉnh | Trẻ em (dp_0) | Trẻ em (dp_1) | (s_v) | (t_v) | (dp_0) | (dp_1) | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | 4 | không | không | 1 | 0 | 0 | 1 | 
| 3 | 3 | 0 | 1 | 0 | 1 | 1 | 0 | 
| 2 | 2 | 1 | 0 | 1 | 0 | 0 | 1 | 
| 1 | 1 | 0 | 1 | 0 | 1 | 1 | không cần thiết | 

Đối với đỉnh lá 4, ma trận cục bộ của nó chỉ đơn giản là ([0]), cho (dp_0=0). Việc xóa đỉnh biên của nó sẽ để lại ma trận trống, do đó (dp_1=1). Do đó, cha mẹ nhìn thấy cầu 2 chu kỳ có dấu âm thông qua mục nhập chéo (-t_v). 

Tại gốc, ma trận cục bộ thu được là ([0]) với kết nối con hiệu quả và định thức cuối cùng là (1), khớp với đầu ra mẫu. Ví dụ minh họa tại sao biển báo cầu 2 chu kỳ phải được đưa ra đúng một lần. 

### Mẫu 2 

Sáu đỉnh tạo thành hai vùng tuần hoàn nhỏ được kết nối qua các cây cầu. Sau khi loại bỏ các cây cầu, cây thành phần có các thành phần có kích thước tối đa là ba, thỏa mãn (k=3). 

Đối với mỗi thành phần, thành phần con được xử lý trước tiên. Tại mỗi đỉnh biên, DP kết hợp khả năng không có cầu con nào được sử dụng với khả năng sử dụng chính xác một cầu con. 

| Sân khấu | Thành phần hiện tại | Kích thước địa phương | (dp_0) | (dp_1) | 
| --- | --- | --- | --- | --- | 
| 1 | thành phần lá | nhỏ | được tính từ ma trận cục bộ của nó | tính toán sau khi xóa ranh giới | 
| 2 | thành phần giữa | nhỏ | kết hợp các giá trị con (s_v,t_v) | xóa ranh giới gốc của nó | 
| 3 | thành phần gốc | nhỏ | yếu tố quyết định cuối cùng | không cần thiết | 

Giá trị gốc kết quả là 

[ 
998244352\equiv -1\pmod{998244353}, 
] 

đó là đầu ra mẫu thứ hai. Dấu vết thực hiện trường hợp một thành phần có cả các cạnh bên trong và các chuyển tiếp cầu, do đó định thức cục bộ phải kết hợp các vỏ chu trình bên trong thành phần đó với 2 chu trình con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m+n k^2)) | Phát hiện cầu và xây dựng thành phần là tuyến tính. Một thành phần có kích thước (s\le k) cần (O(s^3)) công thức xác định và (\sum s=n), cho (O(nk^2)). | 
| Không gian | (O(m+n)) | Biểu đồ, thông tin cầu nối, cây thành phần và mảng DP có kích thước tuyến tính ở đầu vào. | 

Thành phần lớn nhất chỉ có 25 đỉnh, do đó đại số tuyến tính đắt tiền được thực hiện trên các ma trận cực nhỏ. Tổng kích thước thành phần là (n), đó là lý do tại sao tính toán khối cục bộ trở thành (O(nk^2)) trên toàn cầu. Hướng dẫn cuộc thi đưa ra giới hạn (O(nk^2)) tương tự cho cây DP dự định. 

DFS lặp lại tránh được các vấn đề về ngăn xếp đệ quy và các mảng kề cận nhỏ gọn giúp kiểm soát việc sử dụng bộ nhớ ngay cả khi đọc giới hạn đầu vào danh nghĩa (500.000). 

## Trường hợp thử nghiệm```python
# Assume the solution code above is saved as solution.py.
# This harness redirects stdin/stdout and calls solve() directly.

import io
import sys
from contextlib import redirect_stdout

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    try:
        with redirect_stdout(out):
            solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin

# Provided samples.
assert run("""\
4 3 1
1 2
2 3
3 4
""") == "1", "sample 1"

assert run("""\
6 6 3
2 3
5 6
2 5
1 2
3 4
6 2
""") == "998244352", "sample 2"

assert run("""\
10 15 10
1 8
1 7
6 7
2 8
6 9
1 2
4 9
4 10
4 6
5 6
3 8
9 10
8 10
3 5
2 7
""") == "35", "sample 3"

# Minimum-size graph.
assert run("""\
1 0 1
""") == "0", "single isolated vertex"

# One bridge. The adjacency matrix has determinant -1.
assert run("""\
2 1 1
1 2
""") == "998244352", "single edge"

# Three-vertex path. Its determinant is zero.
assert run("""\
3 2 1
1 2
2 3
""") == "0", "odd path"

# One bridge-free component. Its adjacency matrix is the triangle matrix,
# whose determinant is 2.
assert run("""\
3 3 3
1 2
2 3
3 1
""") == "2", "triangle"

# Maximum n with a path. For P_n, det(P_n) is 1 when n is divisible by 4.
n = 25000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
large_path = f"{n} {n - 1} 1\n{edges}\n"
assert run(large_path) == "1", "maximum-n path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1`|`0`| Mục nhập ma trận kề trống và trường hợp biên có kích thước tối thiểu | 
|`2 1 1`|`998244352`| Biển hiệu cầu 2 chu kỳ | 
|`3 2 1`|`0`| Vỏ chu trình không tồn tại đối với đường đi lẻ | 
| Tam giác có (k=3) |`2`| Thành phần không có cầu và định thức cục bộ thông thường | 
| Đường đi có (25.000) đỉnh |`1`| Tối đa (n), DFS lặp và cây thành phần có kích thước tuyến tính | 

## Vỏ cạnh 

Đối với một đỉnh duy nhất,```
1 0 1
```không có cạnh và không có cầu. Thành phần duy nhất chứa đỉnh 1 nên ma trận cục bộ của nó là ([0]). Việc loại bỏ Gaussian không tìm thấy điểm xoay và trả về (0). Do đó, gốc DP là (0), khớp chính xác với định thức kề. 

Đối với một cây cầu duy nhất,```
2 1 1
1 2
```cả hai đỉnh đều là thành phần đơn lẻ. Lá có (dp_0=0) và (dp_1=1), vì việc xóa ranh giới của nó sẽ để lại ma trận trống. Tại cha, con đóng góp (t=1), do đó ma trận cục bộ là ([-1]). Định thức của nó là (-1), được biểu diễn theo modulo (998244353) là (998244352). Do đó, biển báo 2 chu kỳ của cầu được tính đúng một lần. 

Đối với đường đi ba đỉnh,```
3 2 1
1 2
2 3
```lá ở đỉnh 3 cho (dp_0=0,dp_1=1). Tại đỉnh 2, kết nối con cho (t_2=1), do đó ma trận cục bộ trở thành ([-1]), cho (dp_0=-1) và (dp_1=0). Số 0 (dp_1) có nghĩa là đỉnh 2 không thể bị xóa đồng thời thông qua cầu mẹ của nó trong khi phía con của nó đã được cố định. Ở gốc, định thức thu được là (0), theo yêu cầu. 

Đối với tam giác,```
3 3 3
1 2
2 3
3 1
```không có cầu nối nên cây thành phần chứa một nút. Mọi (s_v) là (1) và mọi (t_v) là (0). Ma trận cục bộ chính là ma trận kề tam giác, 

[ 
\bắt đầu{pmatrix} 
0&1&1\ 
1&0&1\ 
1&1&0 
\end{pmatrix}, 
] 

định thức của nó là (2). Do đó, DP suy biến một cách tự nhiên về tính toán xác định thông thường đối với thành phần nhỏ không có cầu. 

Thử nghiệm đường dẫn lớn sử dụng (25.000) thành phần đơn lẻ. Bản thân cây thành phần của nó là một đường dẫn, do đó cầu DFS đạt đến độ sâu tối đa có thể. Bởi vì việc triển khai sử dụng một ngăn xếp rõ ràng thay vì các lệnh gọi Python đệ quy nên nó xử lý trường hợp này mà không gặp vấn đề về giới hạn đệ quy hoặc ngăn xếp C. Sự truy hồi xác định cho một đường đi là (D_n=-D_{n-2}), với (D_0=1) và (D_2=-1). Vì (25.000\equiv0\pmod4), câu trả lời là (1). Bài kiểm tra này thực hiện phần tuyến tính của thuật toán thay vì phần ma trận nhỏ.
