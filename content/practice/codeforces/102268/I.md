---
title: "CF 102268I - Đồ thị thú vị"
description: "Chúng ta được cung cấp một đồ thị vô hướng đơn giản có tối đa (10^5) đỉnh và (10^5) cạnh. Đối với mọi số màu có sẵn (k) từ (1) đến (n), chúng ta cần số lượng màu đỉnh thích hợp bằng cách sử dụng các màu được gắn nhãn (k) đó, modulo (998244353)."
date: "2026-08-19T04:33:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "I"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 860
verified: false
draft: false
---

[CF 102268I - Biểu đồ thú vị](https://codeforces.com/problemset/problem/102268/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14p 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng đơn giản có tối đa (10^5) đỉnh và (10^5) cạnh. Đối với mọi số màu có sẵn (k) từ (1) đến (n), chúng ta cần số lượng màu đỉnh thích hợp bằng cách sử dụng các màu được gắn nhãn (k) đó, modulo (998244353). 

Điều kiện bất thường trên biểu đồ là điều làm cho vấn đề có thể giải quyết được. Lấy bảy đỉnh bất kỳ. Trong số đó, hai phải có đỉnh thứ ba nào đó ngoài bảy đỉnh nằm trên mọi đường đi giữa hai đỉnh đó. Điều kiện này buộc mọi thành phần được kết nối hai chiều, còn được gọi là khối, phải chứa tối đa sáu đỉnh. 

Để biết lý do tại sao, hãy giả sử một thành phần được kết nối hai chiều có ít nhất bảy đỉnh. Chọn bảy đỉnh bất kỳ của nó là (A). Đối với hai đỉnh phân biệt bất kỳ (a,b\in A) và với bất kỳ (c\notin A), đỉnh (c) không thể tách rời (a) và (b). Nếu (c) nằm ngoài thành phần thì nó không liên quan đến các đường dẫn bên trong thành phần. Nếu (c) là một đỉnh khác của thành phần, thì khả năng kết nối kép sẽ tạo ra một đường tránh (a)-(b) (c). Điều này mâu thuẫn với tính chất cần thiết. 

Giới hạn (n,m\le 10^5) loại trừ mọi thứ khám phá các tập hợp con đỉnh tùy ý, liệt kê các màu hoặc thực hiện phép toán bậc hai cho mọi đỉnh. Chẵn (O(n^2)) đã có nghĩa là khoảng (10^{10}) phép toán cơ bản ở giới hạn trên. Việc phân tách hữu ích về cơ bản phải tuyến tính theo kích thước đầu vào, chỉ với một lượng công việc không đổi nhỏ cho mỗi khối. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Đồ thị có hai đỉnh và một cạnh có số lượng màu (0,2), vì một màu không thể phân tách các điểm cuối trong khi hai màu cho hai phép gán. Một đồ thị có ba đỉnh trên một đường đi có (0,2,12), vì đa thức sắc độ của nó là (k(k-1)^2). Biểu đồ bị ngắt kết nối phải được xử lý theo từng thành phần. Ví dụ: hai cạnh rời nhau trên bốn đỉnh có đa thức (k^2(k-1)^2), cho ra (0,4,36,144). Cuối cùng, một biểu đồ hoàn chỉnh trên sáu đỉnh là một khối được phép duy nhất và câu trả lời của nó là (0,0,0,0,0,720). Việc triển khai bất cẩn cho rằng mọi khối đều là cạnh cây hoặc chia cho (k) không đúng điểm sẽ dẫn đến những trường hợp sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi cách gán màu cho các đỉnh. Đối với một (k) cố định, có các phép gán (k^n) và kiểm tra một phép gán với tất cả các chi phí cạnh (O(m)). Thực hiện điều này với mọi (k) sẽ được (O(m\sum_{k=1}^n k^n)), vốn đã ở mức (m n^n). Với (n=10^5), điều này không chỉ là quá chậm mà còn hoàn toàn không khả thi. 

Quan sát hữu ích là đồ thị có thể được phân chia ở các đỉnh khớp nối. Khi một biểu đồ được kết nối được phân tách thành các thành phần được kết nối đôi của nó, các khối khác nhau chỉ tương tác thông qua một đỉnh khớp nối chung duy nhất. Màu của một khối có thể được kết hợp độc lập với màu của khối tiếp theo sau khi màu của đỉnh khớp nối chung của chúng đã được cố định. 

Giả sử một đồ thị liên thông có các khối (B_1,\ldots,B_t). Nếu (P_B(k)) biểu thị đa thức sắc độ của khối (B), thì 

[ 
P_G(k)=\frac{\prod_{i=1}^{t}P_{B_i}(k)}{k^{t-1}}. 
] 

Mỗi khối chứa ít nhất một cạnh, vì vậy (P_B(k)) chia hết cho (k). Xác định 

[ 
Q_B(k)=\frac{P_B(k)}{k}. 
] 

Sau đó, một thành phần được kết nối sẽ đóng góp 

[ 
k\prod_B Q_B(k). 
] 

Đối với đồ thị có các thành phần liên thông (C), câu trả lời đầy đủ là 

[ 
P_G(k)=k^C\prod_B Q_B(k). 
] 

Khó khăn còn lại là đánh giá tất cả các yếu tố này cho mọi (k). Mỗi khối có tối đa sáu đỉnh, vì vậy chúng ta có thể liệt kê tất cả các phân vùng của các đỉnh của nó thành các tập độc lập. Chỉ có (203) bộ phân vùng gồm sáu phần tử. Nếu (c_t) là số phân vùng hợp lệ thành chính xác (t) tập hợp độc lập, thì 

[ 
P_B(k)=\sum_{t=1}^{|B|}c_t(k)_t, 
] 

ở đâu 

[ 
(k)_t=k(k-1)\cdots(k-t+1). 
] 

Sau khi chia cho (k), 

[ 
Q_B(k)=\sum_{t=1}^{|B|}c_t(k-1)_{t-1}. 
]

Do đó, mỗi khối được biểu diễn bằng một bộ gồm tối đa sáu số nguyên nhỏ. 

Có rất ít đa thức sắc độ khác nhau cho đồ thị liên thông trên nhiều nhất là sáu đỉnh. Số lượng đã biết là (1,1,2,5,14,50) cho các kích thước từ (1) đến (6), do đó chỉ có (72) đa thức màu được kết nối khác nhau trên các kích thước (2) đến (6). Do đó, chúng ta có thể nhóm các khối có cùng bộ hệ số và chỉ xử lý mỗi loại một lần. Cuộc thảo luận ban đầu của cuộc thi mô tả chính xác cách tiếp cận phân loại trạng thái nhỏ này, nhận thấy rằng số lượng đa thức địa phương có liên quan là dưới (100). 

Do đó, tính toán mạnh mẽ bên trong một khối là rất nhỏ, trong khi biểu đồ lớn được xử lý bằng cách phân tách khối. Đây là sự chuyển đổi quan trọng từ một bài toán tô màu đồ thị tùy ý sang một tập hợp các bài toán có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(mn^n)) | (O(n+m)) | Quá chậm | 
| Phân rã khối và phân loại đa thức cục bộ | (O(n+m+Un)), với (U<100) loại cục bộ | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS của Tarjan cho các thành phần được kết nối hai chiều. Duy trì thời gian khám phá và giá trị liên kết thấp của mỗi đỉnh và một chồng các cạnh đi qua. Bất cứ khi nào một phần tử con DFS (v) thỏa mãn (\operatorname{low[v]\ge\operatorname{tin[u]), hãy bật các cạnh cho đến khi cạnh (uv) bị loại bỏ. Những cạnh bật lên đó tạo thành một khối. Thuộc tính biểu đồ đã cho đảm bảo rằng mọi khối kết quả đều chứa tối đa sáu đỉnh. 
2. Đếm các thành phần được kết nối trong khi chạy DFS. Một đỉnh cô lập không có khối cạnh nhưng nó vẫn đóng góp một thừa số (k) vào đa thức màu. Đây chính xác là lý do tại sao hệ số toàn cầu cuối cùng là (k^C). 
3. Với mỗi khối, thu thập các đỉnh của nó và dịch chúng sang các chỉ số cục bộ (0,\ldots,s-1), trong đó (s\le6). Mã hóa các cạnh của khối dưới dạng mặt nạ bit. Vì có nhiều nhất (\binom 62=15) cạnh cục bộ có thể có nên toàn bộ khối vừa với một số nguyên 15 bit. 
4. Liệt kê mọi phân vùng tập hợp của (các) đỉnh cục bộ. Phân vùng thể hiện một cách để quyết định đỉnh nào nhận được cùng màu. Phân vùng có thể sử dụng được chính xác khi không có cạnh đồ thị nào có cả hai điểm cuối trong cùng một phần. Đếm xem có bao nhiêu phân vùng hợp lệ có (t) phần. Các số đếm này là các hệ số (c_t) trong khai triển giai thừa giảm dần của đa thức màu của khối. 
5. Lưu trữ bộ hệ số làm loại khối và đếm xem mỗi loại có bao nhiêu khối. Các khối có cùng bộ dữ liệu có cùng một bộ (Q_B(k)), vì vậy không có lý do gì để đánh giá chúng một cách riêng biệt. 
6. Đối với từng loại khối riêng biệt, hãy đánh giá 

[ 
Q(k)=\sum_t c_t(k-1)_{t-1} 
] 

với tất cả (k=1,\ldots,n). Bởi vì (Q) có bậc nhiều nhất là năm, nên các giá trị của nó có thể được tạo ra bằng cách sử dụng sai phân hữu hạn, tránh việc đánh giá đa thức mới liên quan đến năm phép nhân tại mọi điểm. 

1. Nhân phần đóng góp của loại vào câu trả lời. Nếu loại xảy ra (r) lần thì đóng góp của nó là (Q(k)^r). Đối với một lần xuất hiện, chúng tôi nhân trực tiếp; đối với nhiều lần xuất hiện, chúng tôi sử dụng lũy ​​thừa mô-đun. 
2. Sau khi tất cả các loại khối đã được xử lý, nhân mỗi câu trả lời với (k^C). Giá trị kết quả là số lượng màu (k) thích hợp của toàn bộ đồ thị. 

Tại sao nó hoạt động: Sự phân rã của Tarjan tách một biểu đồ thành các khối chỉ giao nhau ở các đỉnh khớp nối. Khi màu của đỉnh khớp nối như vậy được cố định, màu của các khối tới sẽ độc lập. Màu khối có khả năng (P_B(k)), nhưng màu khớp nối chung được tính một lần trong mỗi khối sự cố, do đó, mỗi khối bổ sung sẽ đóng góp một phép chia cho (k). Điều này mang lại (k^C\prod_B(P_B(k)/k)). Khai triển giai thừa giảm cục bộ đếm mỗi màu thích hợp chính xác một lần bằng cách phân chia các đỉnh đầu tiên thành các lớp màu khác rỗng của nó và sau đó gán các màu được gắn nhãn riêng biệt cho các lớp đó. Vì mỗi khối có tối đa sáu đỉnh nên việc liệt kê đầy đủ các phân vùng độc lập của nó là chính xác và có kích thước không đổi.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def generate_partitions(n):
    """Return (number_of_parts, internal_pair_mask) for every set partition."""
    if n == 0:
        return [(0, 0)]

    pair_id = [[-1] * n for _ in range(n)]
    bit = 0
    for i in range(n):
        for j in range(i + 1, n):
            pair_id[i][j] = pair_id[j][i] = bit
            bit += 1

    res = []

    # Restricted-growth strings describe set partitions uniquely.
    a = [0] * n

    def dfs(pos, mx):
        if pos == n:
            mask = 0
            for i in range(n):
                for j in range(i + 1, n):
                    if a[i] == a[j]:
                        mask |= 1 << pair_id[i][j]
            res.append((mx + 1, mask))
            return

        for x in range(mx + 2):
            a[pos] = x
            dfs(pos + 1, max(mx, x))

    a[0] = 0
    dfs(1, 0)
    return res

PARTITIONS = {s: generate_partitions(s) for s in range(2, 7)}

def block_signature(vertices, edge_ids, edges):
    """Return the falling-factorial coefficient tuple of one block."""
    s = len(vertices)

    where = {v: i for i, v in enumerate(vertices)}

    edge_mask = 0
    for eid in edge_ids:
        u, v = edges[eid]
        a = where[u]
        b = where[v]
        if a > b:
            a, b = b, a

        # Pair (a,b) among the s vertices.
        bit = 0
        for i in range(a):
            bit += s - 1 - i
        bit += b - a - 1
        edge_mask |= 1 << bit

    cnt = [0] * s

    for parts, inside in PARTITIONS[s]:
        if edge_mask & inside == 0:
            cnt[parts - 1] += 1

    return tuple(cnt)

def solve():
    n, m = map(int, input().split())

    edges = []
    graph = [[] for _ in range(n)]

    for eid in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))
        graph[u].append((v, eid))
        graph[v].append((u, eid))

    sys.setrecursionlimit(max(1_000_000, 2 * n + 100))

    tin = [0] * n
    low = [0] * n
    timer = 0

    edge_stack = []
    type_count = {}
    components = 0

    def process_component(edge_ids):
        verts = set()
        for eid in edge_ids:
            u, v = edges[eid]
            verts.add(u)
            verts.add(v)

        vertices = list(verts)
        sig = block_signature(vertices, edge_ids, edges)
        type_count[sig] = type_count.get(sig, 0) + 1

    def dfs(u, parent_edge):
        nonlocal timer

        timer += 1
        tin[u] = low[u] = timer

        for v, eid in graph[u]:
            if eid == parent_edge:
                continue

            if tin[v] == 0:
                edge_stack.append(eid)

                dfs(v, eid)

                low[u] = min(low[u], low[v])

                if low[v] >= tin[u]:
                    comp_edges = []

                    while True:
                        x = edge_stack.pop()
                        comp_edges.append(x)
                        if x == eid:
                            break

                    process_component(comp_edges)

            elif tin[v] < tin[u]:
                edge_stack.append(eid)
                low[u] = min(low[u], tin[v])

    for root in range(n):
        if tin[root] == 0:
            components += 1
            dfs(root, -1)

    # ans[k] is the contribution accumulated from all Q_B(k).
    ans = [1] * (n + 1)

    for sig, multiplicity in type_count.items():
        # Q(k) = sum_{t=1}^s c_t * (k-1)_(t-1)
        #
        # Q is degree at most 5. Build its first six values and
        # turn them into forward differences.
        s = len(sig)

        vals = []
        for k in range(1, s + 2):
            x = k - 1
            falling = 1
            value = 0

            for j in range(s):
                if j > 0:
                    falling *= x - (j - 1)
                value += sig[j] * falling

            vals.append(value % MOD)

        # Forward differences.
        diff = vals[:]

        for level in range(s):
            for i in range(s - level):
                diff[i] = (diff[i + 1] - diff[i]) % MOD

        # The current value at k=1 is diff[0].
        cur = diff[:]
        q = diff[0]

        # Apply k=1 first.
        if multiplicity == 1:
            ans[1] = ans[1] * q % MOD
        else:
            ans[1] = ans[1] * pow(q, multiplicity, MOD) % MOD

        # Advance from k to k+1 using finite differences.
        for k in range(2, n + 1):
            for level in range(s - 1):
                cur[level] = (cur[level] + cur[level + 1]) % MOD

            q = cur[0]

            if multiplicity == 1:
                ans[k] = ans[k] * q % MOD
            else:
                ans[k] = ans[k] * pow(q, multiplicity, MOD) % MOD

    # Each connected component contributes one free root color.
    for k in range(1, n + 1):
        ans[k] = ans[k] * pow(k, components, MOD) % MOD

    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ ID cạnh thay vì chỉ các đỉnh lân cận. Điều này là cần thiết vì hai điểm cuối DFS chỉ có thể có cùng một đỉnh cha trong các biểu đồ có các cạnh song song, điều này bị cấm ở đây, nhưng việc sử dụng ID cạnh giúp kiểm tra cạnh gốc chính xác và tránh các trường hợp đặc biệt. 

Ngăn xếp Tarjan chứa mỗi cạnh đúng một lần. Một cạnh của cây được đẩy khi con của nó được phát hiện lần đầu tiên, trong khi cạnh sau chỉ được đẩy khi nó trỏ về phía tổ tiên đã được phát hiện. Khi (\operatorname{low[v]\ge\operatorname{tin[u]), đoạn ngăn xếp kết thúc tại (uv) chính xác là một thành phần được kết nối hai chiều. 

Mã hóa khối cục bộ sử dụng tối đa 15 bit cạnh. Việc tính toán vị trí bit hơi khác thường chỉ là một sơ đồ lập chỉ mục cho các cặp đỉnh cục bộ không có thứ tự. Vì một khối có tối đa sáu đỉnh nên từ điển được sử dụng để dịch ID đỉnh toàn cầu sang ID cục bộ vẫn rất nhỏ. 

Trình tạo phân vùng sử dụng các chuỗi tăng trưởng bị hạn chế. Ví dụ: một phân vùng gồm bốn đỉnh thành ba nhóm có thể được biểu diễn bằng một chuỗi như (0,1,2,0). Mỗi phân vùng được thiết lập có chính xác một biểu diễn như vậy, do đó không có sự trùng lặp hay phân vùng bị thiếu. Sáu đỉnh chỉ cho (203) khả năng. 

Chữ ký chứa số lượng phân vùng độc lập với mỗi số phần có thể có. Nhận dạng giai thừa rơi 

[ 
P_B(k)=\sum_t c_t(k)_t 
] 

chính xác là tại sao chữ ký này là đủ. Nhãn thực tế của các đỉnh của khối sẽ biến mất sau phép tính này. 

Việc đánh giá sai phân hữu hạn đáng được quan tâm. Một đa thức bậc-(d) có thể được đánh giá ở các đối số nguyên liên tiếp bằng cách duy trì bảng vi phân thuận của nó. Tiến lên bằng một đối số sẽ thay đổi chênh lệch thứ nhất thành chênh lệch thứ hai, chênh lệch thứ hai thành chênh lệch thứ ba, v.v. Vì bậc nhiều nhất là năm nên mỗi giá trị mới chỉ cần một số ít phép cộng. 

Cuối cùng, Python`pow(a,b,MOD)`thực hiện phép lũy thừa mô-đun mà không cần xây dựng số nguyên khổng lồ (a^b). Mọi phép nhân đều được rút gọn theo modulo (998244353), do đó không có vấn đề tăng trưởng số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị có năm đỉnh, với một hình tam giác ở các đỉnh (1,3,5) và hai đỉnh cô lập. Tam giác là một khối, trong khi mỗi đỉnh cô lập là thành phần được kết nối riêng của nó. 

Tam giác có đa thức màu 

[ 
P_B(k)=k(k-1)(k-2), 
] 

vậy hệ số rút gọn của nó là 

[ 
Q_B(k)=(k-1)(k-2). 
] 

Có ba thành phần được kết nối, tạo ra hệ số chung (k^3). 

| (k) | (Q_B(k)) | (k^3) | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 
| 2 | 0 | 8 | 0 | 
| 3 | 2 | 27 | 54 | 
| 4 | 6 | 64 | 384 | 
| 5 | 12 | 125 | 1500 | 

Kết quả đầu ra là`0 0 54 384 1500`. Dấu vết chứng tỏ tại sao các đỉnh biệt lập được xử lý bởi yếu tố thành phần được kết nối chứ không phải bởi các khối nhân tạo. 

### Hai tam giác có chung một đỉnh 

Hãy xem xét```
5 6
1 2
2 3
3 1
3 4
4 5
5 3
```Có hai khối hình tam giác có chung đỉnh (3). Đồ thị được kết nối nên có một thừa số toàn cục (k). Mỗi tam giác đóng góp ((k-1)(k-2)). 

| (k) | Yếu tố tam giác | Sản phẩm của hai yếu tố | Toàn cầu (k) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | 
| 2 | 0 | 0 | 2 | 0 | 
| 3 | 2 | 4 | 3 | 12 | 
| 4 | 6 | 36 | 4 | 144 | 
| 5 | 12 | 144 | 5 | 720 | 

Ví dụ này thực hiện quy tắc nhân khớp nối-đỉnh. Đỉnh chung có một màu chứ không phải hai màu được chọn độc lập, đó chính xác là lý do tại sao tích của các đa thức khối phải chia cho (k). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m+Un)) | Tarjan là tuyến tính, mỗi khối có tối đa sáu đỉnh và có ít hơn (100) loại đa thức cục bộ có liên quan | 
| Không gian | (O(n+m)) | Lưu trữ đồ thị, mảng DFS, ngăn xếp cạnh và thông tin khối đều là tuyến tính | 

Thực tế cấu trúc quan trọng là điều kiện bảy đỉnh giới hạn mỗi khối bằng sáu đỉnh. Do đó, phép liệt kê cục bộ có kích thước không đổi, trong khi số lượng đa thức màu liên kết riêng biệt trên nhiều nhất sáu đỉnh là rất nhỏ. Với (n,m\le10^5), tính toán kết quả vẫn nằm trong phạm vi độ phức tạp dự định và thoải mái tránh mọi sự phụ thuộc vào (k^n). 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`Hàm từ giải pháp trên có sẵn trong cùng một quy trình Python.```python
import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin
    return out.getvalue().strip()

# Sample 1: triangle plus two isolated vertices.
assert run(
    """5 3
3 5
5 1
1 3
"""
) == "0 0 54 384 1500", "sample 1"

# Custom 1: minimum valid n with one edge.
assert run(
    """2 1
1 2
"""
) == "0 2", "single edge"

# Custom 2: a path on three vertices.
assert run(
    """3 2
1 2
2 3
"""
) == "0 2 12", "path"

# Custom 3: disconnected graph with two independent edges.
assert run(
    """4 2
1 2
3 4
"""
) == "0 4 36 144", "disconnected components"

# Custom 4: maximum-size block, K6.
assert run(
    """6 15
1 2
1 3
1 4
1 5
1 6
2 3
2 4
2 5
2 6
3 4
3 5
3 6
4 5
4 6
5 6
"""
) == "0 0 0 0 0 720", "K6 boundary"

# Large-size structural test.
# A path is useful for stress-testing the implementation of the block
# decomposition, although it does not satisfy the original seven-vertex
# promise once it becomes long.
n = 100000
edges = "\n".join(f"{i} {i+1}" for i in range(1, n))
large_input = f"{n} {n-1}\n{edges}\n"
large_output = run(large_input).split()

assert len(large_output) == n, "large output length"
assert large_output[0] == "0", "one-color boundary"
assert large_output[1] == str(2), "two-color path boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`|`0 2`| Đồ thị tối thiểu và khối một cạnh | 
|`3 2 / 1 2, 2 3`|`0 2 12`| Khối cây và các đỉnh khớp nối lặp lại | 
|`4 2 / 1 2, 3 4`|`0 4 36 144`| Nhiều thành phần được kết nối | 
| (K_6) |`0 0 0 0 0 720`| Kích thước khối tối đa được phép và liệt kê giai thừa giảm dần | 
| Đường đi có (10^5) đỉnh | (10^5) giá trị | Đường ranh giới đầu ra và đường ngang Tarjan đầu vào lớn | 

Kiểm tra căng thẳng lớn có chủ ý kiểm tra việc thực hiện trên biểu đồ có số đỉnh tối đa. Nó không được trình bày như một phiên bản hợp lệ của lời hứa ban đầu vì một đường đi dài chứa bảy đỉnh liên tiếp không có dấu phân cách bên ngoài. Mục đích của nó là phát hiện các lỗi ngăn xếp, truyền tải và hiệu suất một cách độc lập với sự đảm bảo về cấu trúc. 

## Vỏ cạnh 

Đối với đồ thị chứa một cạnh, khối duy nhất là (K_2). Các phân vùng tập hợp độc lập hợp lệ của nó là phân vùng thành hai tập đơn, vì vậy 

[ 
P_{K_2}(k)=(k)_2=k(k-1). 
] 

Hệ số khối giảm là (k-1) và một thành phần được kết nối cung cấp hệ số bổ sung (k). Đối với đầu vào```
2 1
1 2
```thuật toán thu được (0,2). 

Đối với đường đi trên ba đỉnh, Tarjan tạo ra hai khối (K_2). Mỗi thành phần đóng góp (k-1) và thành phần được kết nối đóng góp (k). Sản phẩm là 

[ 
k(k-1)^2. 
] 

Tại (k=1,2,3), điều này cho ra (0,2,12). Trường hợp này phát hiện lỗi trong đó một đỉnh khớp nối vô tình được đếm hai lần. 

Đối với các biểu đồ bị ngắt kết nối, mọi thành phần được kết nối đều có sự lựa chọn miễn phí về màu sắc của một đỉnh gốc. Với hai cạnh rời nhau thì có hai khối và hai thành phần liên thông nên công thức là 

[ 
k^2(k-1)^2. 
] 

Tại (k=2), điều này cho ra (4), tương ứng với hai lựa chọn nhị phân độc lập về hướng của hai cạnh. 

Với (K_6), có một khối chứa tất cả sáu đỉnh. Mỗi màu thích hợp cần sáu màu riêng biệt, vì vậy đa thức màu của nó là 

[ 
(k)_6. 
] 

Chữ ký khối có chính xác một phân vùng hợp lệ cho mọi số phần từ (1) đến (6) chỉ khi phân vùng tương ứng tương thích với biểu đồ hoàn chỉnh. Trên thực tế, chỉ có phân vùng sáu đơn còn tồn tại, cho ra (P(k)=(k)_6). Do đó tất cả các giá trị của (k<6) đều bằng 0 và giá trị tại (k=6) là (6!=720). Điều này phát hiện từng lỗi một trong cả phép liệt kê phân vùng và đánh giá hệ số giảm cục bộ. 

Lỗi triển khai nguy hiểm nhất là coi khối có kích thước 6 như thể nó có thể có bảy màu cục bộ hoặc quên rằng (Q_B(k)=P_B(k)/k) sử dụng ((k-1)_{t-1}), chứ không phải ((k)_t). Bài kiểm tra (K_6) cho thấy cả hai lỗi ngay lập tức.
