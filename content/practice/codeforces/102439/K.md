---
title: "CF 102439K - Đổi mới"
description: "Chúng ta có một cây thành phố có trọng số. Bởi vì đồ thị là một cái cây nên giữa mỗi cặp thành phố có chính xác một đường đi, do đó đường đi đó tự động là đường đi ngắn nhất."
date: "2026-08-12T08:17:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "K"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 155
verified: true
draft: false
---

[CF 102439K - Những đổi mới](https://codeforces.com/problemset/problem/102439/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây thành phố có trọng số. Bởi vì đồ thị là một cái cây nên giữa mỗi cặp thành phố có chính xác một đường đi, do đó đường đi đó tự động là đường đi ngắn nhất. Mỗi truy vấn chọn hai thành phố và thay thế mọi cạnh trên đường đi của chúng bằng một cạnh có trọng số là sàn của căn bậc hai của nó. Cùng một cạnh có thể được thay đổi nhiều lần. Sau trạng thái ban đầu và sau mỗi truy vấn, chúng ta cần tổng khoảng cách trên tất cả các cặp thành phố không có thứ tự, modulo (10^9+7). Tuyên bố chính thức đưa ra hoạt động tương tự và đầu ra mẫu (140,92,72,48). 

Quan sát hữu ích đầu tiên là chúng ta không bao giờ cần duy trì khoảng cách theo cặp riêng lẻ. Root cây tùy ý. Xét một cạnh có cây con phía con chứa (các) đỉnh. Việc loại bỏ cạnh này sẽ tách cây thành các phần có kích thước (s) và (n-s). Chính xác (s(n-s)) các cặp không có thứ tự có đường đi của chúng đi qua cạnh này. Nếu trọng số hiện tại của nó là (w), thì phần đóng góp của nó vào tổng tất cả các khoảng cách theo cặp là 

[ 
w \cdot s(n-s). 
] 

Vì vậy, toàn bộ câu trả lời chỉ đơn giản là 

[ 
\sum_{\text{edges }e} w_e \cdot s_e(n-s_e). 
] 

Hệ số (s_e(n-s_e)) không bao giờ thay đổi, vì cấu trúc cây không bao giờ thay đổi. Chỉ có trọng số cạnh thay đổi. 

Điều này biến vấn đề thành vấn đề mảng động. Chúng ta cần áp dụng (w \leftarrow \lfloor\sqrt w\rfloor) cho mọi cạnh trên đường đi của cây trong khi vẫn duy trì tổng trọng số của tất cả các giá trị cạnh. 

Các giới hạn khiến cho việc mô phỏng trực tiếp là không thể. Có thể có (2\cdot10^5) truy vấn và một đường dẫn có thể chứa (2\cdot10^5-1) cạnh, do đó, việc truy cập rõ ràng mọi cạnh của đường dẫn có thể yêu cầu cập nhật cạnh (4\cdot10^{10}). Việc tính toán lại tất cả các khoảng cách theo cặp sau mỗi truy vấn sẽ còn tệ hơn, ở mức (O(mn^2)). Chúng ta cần khai thác thực tế là căn bậc hai làm giảm trọng số rất nhanh. 

Chỉ có một vài trường hợp thực sự nguy hiểm. 

### Một thành phố duy nhất 

Đầu vào nhỏ nhất có thể là```
1 1
1 1
```Không có đường nên tổng khoảng cách theo cặp đều bằng 0. Đầu ra đúng là```
0
0
```Việc triển khai bất cẩn cho rằng mọi truy vấn đều chứa ít nhất một cạnh có thể cố gắng truy cập vào một cạnh không tồn tại hoặc tạo ra một phạm vi không hợp lệ. 

### Một cạnh có trọng số đã bằng 1 

Hãy xem xét```
2 1
1 2 1
1 2
```Tổng ban đầu là (1) và việc áp dụng phép toán sẽ cho (\lfloor\sqrt1\rfloor=1), vì vậy câu trả lời vẫn là (1):```
1
1
```Một lỗi phổ biến là cho rằng mọi cạnh được truy vấn đều thay đổi. Cây phân đoạn phải có khả năng dừng ngay lập tức khi tất cả các trọng số trong một phạm vi đều bằng 1. 

### Cập nhật lặp đi lặp lại trên cùng một đường dẫn 

Hãy xem xét```
3 3
1 2 16
2 3 9
1 3
1 3
1 3
```Mỗi cạnh đóng góp (2w), vì mỗi cạnh ngăn cách một đỉnh với hai đỉnh. Các đầu ra là```
50
14
6
4
```Trọng số của cạnh phát triển thành (16\to4\to2\to1) và (9\to3\to1\to1). Việc triển khai chỉ áp dụng căn bậc hai một lần cho mỗi cạnh trên toàn bộ đầu vào sẽ sai. 

### Gốc không có cạnh nào so với gốc của nó 

Khi một cây có gốc bị san phẳng, mỗi đỉnh không có gốc đại diện cho cạnh nối nó với cây mẹ của nó. Gốc đại diện cho không có cạnh. Đối với truy vấn đường dẫn, vị trí gốc không được vô tình được coi là đường thực. Điều này đặc biệt dễ xảy ra sai sót khi khoảng nặng-ánh sáng cuối cùng chứa tổ tiên chung thấp nhất. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất bắt đầu từ công thức đóng góp cạnh. Gốc cây, tính toán mọi kích thước cây con và gán cho mỗi cạnh hệ số cố định (s(n-s)). Sau đó, đối với mỗi truy vấn, hãy đi từ (u) đến (v), thay đổi mọi cạnh trên đường dẫn đó và điều chỉnh câu trả lời tổng thể theo chênh lệch đóng góp tương ứng. 

Cách tiếp cận này đúng vì mọi cạnh đều biết chính xác có bao nhiêu cặp thành phố sử dụng nó. Nếu một cạnh thay đổi từ (w) thành (w'), tổng câu trả lời sẽ thay đổi theo 

[ 
(w'-w)s(n-s). 
] 

Vấn đề là số cạnh được truy cập. Trong cây có hình đường dẫn, một truy vấn giữa hai điểm cuối chứa (n-1) cạnh. Với (m=2\cdot10^5), điều này mang lại khoảng (4\cdot10^{10}) lượt truy cập vào cạnh. Mặc dù mỗi căn bậc hai riêng lẻ đều rẻ nhưng nhiều phép tính đó không thể đáp ứng được giới hạn 1,5 giây. 

Quan sát quan trọng là một cạnh không thể thay đổi nhiều lần. Với (w\le10^6), dãy được giới hạn bởi 

[ 
10^6\to1000\to31\to5\to2\to1. 
] 

Do đó, mỗi cạnh thay đổi tối đa năm lần trong toàn bộ quá trình nhập. Khi một cạnh đạt tới 1, tất cả các cập nhật sau này liên quan đến nó sẽ không có tác dụng gì. 

Điều này làm cho cây phân đoạn trở nên đặc biệt phù hợp. Trước tiên, chúng tôi sử dụng phân tách nặng-ánh sáng để biến mọi đường dẫn của cây thành các khoảng tiếp giáp (O(\log n)) của một mảng. Cây phân đoạn sau đó lưu trữ trọng số cạnh hiện tại trên mảng phẳng này. 

Cây phân đoạn đơn giản có thể lưu trữ trọng lượng tối đa ở mỗi nút. Nếu tối đa là 1 thì toàn bộ phân đoạn được truy vấn có thể bị bỏ qua. Ngược lại, chúng ta đi xuống cho đến khi tìm được các cạnh bị ảnh hưởng. Điều đó đã đưa ra giải pháp khấu hao vì chỉ có (O(n)) thay đổi trọng lượng thực tế. 

Chúng ta có thể làm cho cây phân đoạn mạnh hơn bằng cách lưu trữ cả trọng lượng tối thiểu và tối đa. Hàm căn bậc hai là đơn điệu. Nếu một phân đoạn có tối thiểu (a) và tối đa (b) và 

[ 
\lfloor\sqrt a\rfloor=\lfloor\sqrt b\rfloor, 
] 

thì mọi giá trị giữa (a) và (b) đều có cùng một giá trị mới. Chúng ta có thể cập nhật toàn bộ phân đoạn một cách lười biếng bằng cách gán một giá trị đó. Điều này loại bỏ nhiều hậu duệ không cần thiết. 

Cây phân đoạn cũng lưu trữ tổng các hệ số trong mỗi nút và tổng trọng số hiện tại của nút đó. Khi toàn bộ một nút có cùng trọng lượng (x), sự đóng góp của nó ngay lập tức 

[ 
x\cdot\sum c_i. 
] 

Do đó, gốc của cây phân đoạn luôn chứa câu trả lời cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm)) sau khi tiền xử lý | (O(n)) | Quá chậm | 
| HLD + cây đoạn | (O(m\log^2 n+n\log n\log\log W)) khấu hao | (O(n)) | Đã chấp nhận | 

Ở đây (W\le10^6), vì vậy (\log\log W) thực chất là một hằng số nhỏ. Số lượng thay đổi thành công thực tế trên mỗi cạnh nhiều nhất là năm. 

## Hướng dẫn thuật toán 

1. Root cây tại thành phố 1 và tính toán`parent`,`depth`, Và`subtree_size`cho mỗi thành phố. Đối với mọi đỉnh không phải gốc (v), cạnh từ (v) tới đỉnh gốc của nó có hệ số 

[ 
c_v=\text{subtree_size[v]\cdot(n-\text{subtree_size[v]). 
] 

Hệ số này đếm chính xác có bao nhiêu cặp thành phố không có thứ tự sử dụng cạnh đó. 
2. Tính con nặng của mỗi đỉnh, tức là con có cây con lớn nhất. Phân rã ánh sáng nặng nhóm mọi chuỗi nặng thành một khoảng liền kề của một mảng. Cạnh tới đỉnh cha của đỉnh (v) được lưu trữ tại vị trí mảng của (v). 

Gốc có trọng số bằng 0 vì nó không tương ứng với một cạnh. 
3. Xây dựng cây phân đoạn trên mảng đã được làm phẳng. Mỗi nút cây phân đoạn lưu trữ trọng số cạnh tối thiểu, trọng lượng cạnh tối đa, tổng các hệ số cố định của nó, đóng góp có trọng số hiện tại và giá trị gán lười tùy chọn. 
4. Khi khoảng thời gian truy vấn được bao phủ đầy đủ, trước tiên hãy kiểm tra trọng số tối đa của nó. Nếu nó nhiều nhất là 1 thì khoảng đó không cần thực hiện. 

Mặt khác, tính căn bậc hai sàn của mức tối thiểu và tối đa. Nếu chúng bằng nhau, tính đơn điệu của hàm căn bậc hai có nghĩa là mọi giá trị trong khoảng đều trở thành cùng một số đó. Toàn bộ phân khúc có thể được chỉ định một cách lười biếng mà không cần truy cập vào các lá của nó. 
5. Nếu giá trị tối thiểu và tối đa tạo ra các căn bậc hai khác nhau, thì phân đoạn chứa ít nhất hai giá trị kết quả khác nhau, do đó nó không thể được biểu thị bằng một phép gán lười. Đẩy bất kỳ nhiệm vụ đang chờ xử lý nào cho trẻ và lặp lại thành hai nửa. 
6. Phân tách mọi đường dẫn truy vấn cây bằng cách sử dụng phân tách nặng-nhẹ. Trong khi hai điểm cuối thuộc về các chuỗi nặng khác nhau, hãy cập nhật toàn bộ đoạn chuỗi thuộc chuỗi sâu hơn. Khi cả hai đỉnh đều nằm trên cùng một chuỗi, hãy cập nhật khoảng bên dưới tổ tiên chung thấp nhất của chúng. 

Khoảng thời gian cuối cùng sử dụng`pos[lca] + 1`, không`pos[lca]`, bởi vì vị trí của đỉnh biểu thị cạnh dẫn vào đỉnh đó. Bản thân LCA không có cạnh trên đường đi bên dưới nó. 
7. Sau khi xử lý truy vấn, gốc của cây phân đoạn chứa tổng (w_e c_e) trên mỗi cạnh. In giá trị này theo modulo (10^9+7). 

### Tại sao nó hoạt động 

Đối với mỗi cạnh, số cặp thành phố có đường đi sử dụng cạnh đó là cố định (s(n-s)), do đó tổng toàn cục cần thiết chính xác là tổng trọng số của cạnh nhân với hệ số cố định của chúng. Cây phân đoạn duy trì chính xác những đóng góp của cạnh có trọng số này. 

Trong khi truy vấn, mọi cạnh trên đường dẫn cây được yêu cầu sẽ được chuyển đổi một lần bởi (w\mapsto\lfloor\sqrt w\rfloor). Sự phân hủy ánh sáng nặng bao gồm chính xác các cạnh đó và không có các cạnh khác. Cây phân đoạn áp dụng phép biến đổi tương tự cho mọi cạnh được bao phủ. Khi toàn bộ phân đoạn có cùng giá trị căn bậc hai, phép gán lười là hợp lệ vì hàm căn bậc hai là đơn điệu. Mặt khác, đệ quy cuối cùng sẽ đạt đến các phân đoạn nhỏ hơn nơi phép biến đổi có thể được biểu diễn chính xác. 

Do đó, sau mỗi truy vấn, mỗi cạnh đều có chính xác trọng số hiện tại được yêu cầu và gốc cây phân đoạn chính xác là tổng yêu cầu của tất cả các đường đi ngắn nhất theo cặp. 

## Giải pháp Python```python
import sys
from math import isqrt
from array import array

input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, m = map(int, input().split())

    # Forward-star adjacency representation.
    head = array('i', [-1]) * n
    to = array('i', [0]) * (2 * max(0, n - 1))
    nxt = array('i', [0]) * (2 * max(0, n - 1))
    ew = array('i', [0]) * (2 * max(0, n - 1))

    ptr = 0
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1

        to[ptr] = v
        ew[ptr] = w
        nxt[ptr] = head[u]
        head[u] = ptr
        ptr += 1

        to[ptr] = u
        ew[ptr] = w
        nxt[ptr] = head[v]
        head[v] = ptr
        ptr += 1

    # Root the tree and compute parent, depth, edge-to-parent weight.
    parent = array('i', [-1]) * n
    depth = array('i', [0]) * n
    weight_to_parent = array('i', [0]) * n
    order = array('i', [0]) * n

    parent[0] = 0
    stack = [0]
    order_len = 0

    while stack:
        v = stack.pop()
        order[order_len] = v
        order_len += 1

        e = head[v]
        while e != -1:
            u = to[e]
            if u != parent[v]:
                parent[u] = v
                depth[u] = depth[v] + 1
                weight_to_parent[u] = ew[e]
                stack.append(u)
            e = nxt[e]

    # Subtree sizes and heavy children.
    size = array('i', [1]) * n
    heavy = array('i', [-1]) * n

    for idx in range(n - 1, 0, -1):
        v = order[idx]
        p = parent[v]
        size[p] += size[v]

        h = heavy[p]
        if h == -1 or size[v] > size[h]:
            heavy[p] = v

    # Heavy-light decomposition.
    chain_head = array('i', [0]) * n
    pos = array('i', [0]) * n

    cur = 0
    chain_stack = [0]

    while chain_stack:
        h = chain_stack.pop()
        v = h

        while v != -1:
            chain_head[v] = h
            pos[v] = cur
            cur += 1

            e = head[v]
            hv = heavy[v]
            while e != -1:
                u = to[e]
                if parent[u] == v and u != hv:
                    chain_stack.append(u)
                e = nxt[e]

            v = hv

    # Flattened edge weights and fixed edge coefficients.
    weights = array('i', [0]) * n
    coeff = array('q', [0]) * n

    for v in range(1, n):
        p = pos[v]
        weights[p] = weight_to_parent[v]
        coeff[p] = size[v] * (n - size[v])

    # Segment tree arrays.
    #
    # mn[x], mx[x]      minimum/maximum weight in the node
    # sumc[x]           sum of fixed edge coefficients
    # sumw[x]           current weighted contribution
    # lazy[x]           >= 0 means all values in this node are assigned to it
    #
    # The root position has coefficient 0 and weight 0.
    S = 4 * n + 5

    mn = array('i', [0]) * S
    mx = array('i', [0]) * S
    lazy = array('i', [-1]) * S
    sumc = array('q', [0]) * S
    sumw = array('q', [0]) * S

    def apply(node, value):
        mn[node] = value
        mx[node] = value
        sumw[node] = value * sumc[node]
        lazy[node] = value

    def build(node, left, right):
        if left == right:
            w = weights[left]
            c = coeff[left]
            mn[node] = w
            mx[node] = w
            sumc[node] = c
            sumw[node] = w * c
            return

        mid = (left + right) >> 1
        lc = node << 1
        rc = lc | 1

        build(lc, left, mid)
        build(rc, mid + 1, right)

        mn[node] = min(mn[lc], mn[rc])
        mx[node] = max(mx[lc], mx[rc])
        sumc[node] = sumc[lc] + sumc[rc]
        sumw[node] = sumw[lc] + sumw[rc]

    def push(node):
        value = lazy[node]
        if value != -1:
            lc = node << 1
            rc = lc | 1
            apply(lc, value)
            apply(rc, value)
            lazy[node] = -1

    def pull(node):
        lc = node << 1
        rc = lc | 1
        mn[node] = min(mn[lc], mn[rc])
        mx[node] = max(mx[lc], mx[rc])
        sumw[node] = sumw[lc] + sumw[rc]

    def range_sqrt(node, left, right, ql, qr):
        if right < ql or qr < left or mx[node] <= 1:
            return

        if ql <= left and right <= qr:
            a = isqrt(mn[node])
            b = isqrt(mx[node])

            if a == b:
                apply(node, a)
                return

        if left == right:
            apply(node, isqrt(mx[node]))
            return

        push(node)

        mid = (left + right) >> 1
        lc = node << 1
        rc = lc | 1

        if ql <= mid:
            range_sqrt(lc, left, mid, ql, qr)
        if qr > mid:
            range_sqrt(rc, mid + 1, right, ql, qr)

        pull(node)

    build(1, 0, n - 1)

    def update_path(u, v):
        while chain_head[u] != chain_head[v]:
            hu = chain_head[u]
            hv = chain_head[v]

            if depth[hu] < depth[hv]:
                u, v = v, u
                hu, hv = hv, hu

            range_sqrt(1, 0, n - 1, pos[hu], pos[u])
            u = parent[hu]

        if u == v:
            return

        if depth[u] < depth[v]:
            u, v = v, u

        # u is deeper. The LCA itself is not an edge on the path.
        range_sqrt(1, 0, n - 1, pos[v] + 1, pos[u])

    out = [str(sumw[1] % MOD)]

    for _ in range(m):
        u, v = map(int, input().split())
        update_path(u - 1, v - 1)
        out.append(str(sumw[1] % MOD))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý đầu tiên sử dụng phương pháp truyền tải lặp lại vì độ sâu đệ quy mặc định của Python không phù hợp với cây có thể là một chuỗi gồm (2\cdot10^5) đỉnh. các`order`mảng ghi lại thứ tự duyệt từ gốc tới lá và xử lý ngược nó sẽ cho kích thước cây con mà không cần đệ quy. 

Cây con nặng được chọn sau khi biết kích thước cây con. Sau đó, quá trình phân hủy sẽ đi trực tiếp từng chuỗi nặng và đẩy các chuỗi con nhẹ vào một ngăn xếp. Điều này tạo ra các vị trí liền kề cho mỗi chuỗi nặng, đó chính xác là những gì cây phân đoạn cần. 

Mảng hệ số được lập chỉ mục theo vị trí của đỉnh con. Nếu đỉnh (v) không phải là gốc thì vị trí đó biểu thị cạnh từ`parent[v]`đến (v). hệ số của nó là`size[v] * (n - size[v])`. 

Cây phân đoạn không lưu trữ tổng số tiền giảm modulo bên trong. Tổng số lớn nhất có thể là dưới khoảng (n^2\cdot10^6), nằm trong phạm vi số nguyên Python và cả trong số học 64 bit đã ký. Việc tránh thao tác modulo trong mỗi lần hợp nhất và gán sẽ giúp việc triển khai nhanh hơn. Chỉ giá trị được in cho người dùng mới bị giảm modulo (10^9+7). 

các`lazy`giá trị đại diện cho một nhiệm vụ, không phải là một sự gia tăng. Một giá trị của`-1`có nghĩa là không có nhiệm vụ đang chờ xử lý. Vì trọng số của các cạnh luôn không âm nên`-1`là một dấu hiệu rõ ràng. 

Tối ưu hóa tối thiểu và tối đa là phần tinh tế của cây phân đoạn. Giả sử một nút chứa trọng số từ 16 đến 25. Cả hai điểm cuối đều có căn bậc hai là 4 và 5, do đó kết quả không đồng nhất. Nút phải được tách ra. Thay vào đó, nếu nút chứa các giá trị từ 16 đến 24 thì căn bậc hai nằm trong khoảng từ 4 đến 4, do đó mọi giá trị sẽ trở thành 4 và toàn bộ nút có thể được gán cùng một lúc. 

Khoảng HLD cuối cùng bắt đầu lúc`pos[v] + 1`khi cả hai điểm cuối đều nằm trên cùng một chuỗi. Đây là điểm khác thường nhất trong vấn đề này. Vị trí đỉnh đại diện cho các cạnh đến, vì vậy vị trí của LCA đại diện cho cạnh từ cha mẹ của nó vào LCA, không phải là một phần của đường dẫn được truy vấn. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
5 3
1 2 4
2 3 4
1 4 9
1 5 16
1 5
1 3
1 4
```Sau khi root ở thành phố 1, hệ số cạnh là 6 cho cạnh (1-2), 4 cho cạnh (2-3), 4 cho cạnh (1-4) và 4 cho cạnh (1-5). 

| Tiểu bang | Cạnh (1-2) | Cạnh (2-3) | Cạnh (1-4) | Cạnh (1-5) | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | (4\cdot6=24) | (4\cdot4=16) | (9\cdot4=36) | (16\cdot4=64) | 140 | 
| Truy vấn (1,5) | 24 | 16 | 36 | (4\cdot4=16) | 92 | 
| Truy vấn (1,3) | (2\cdot6=12) | (2\cdot4=8) | 36 | 16 | 72 | 
| Truy vấn (1,4) | 12 | 8 | (3\cdot4=12) | 16 | 48 | 

Truy vấn đầu tiên chỉ thay đổi cạnh (1-5), vì đây là cạnh duy nhất trên đường dẫn từ 1 đến 5. Truy vấn thứ hai thay đổi hai cạnh từ 1 thành 3. Truy vấn cuối cùng thay đổi cạnh (1-4). Do đó, đầu ra là```
140
92
72
48
```Ví dụ thứ hai nhấn mạnh các phép biến đổi lặp đi lặp lại.```
3 3
1 2 16
2 3 9
1 3
1 3
1 3
```Cả hai cạnh đều có hệ số 2. 

| Truy vấn | Cân nặng (1-2) | Cân nặng (2-3) | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | 16 | 9 | 50 | 
| (1,3) | 4 | 3 | 14 | 
| (1,3) | 2 | 1 | 6 | 
| (1,3) | 1 | 1 | 4 | 

Dấu vết chứng minh tại sao một cạnh không thể được đánh dấu đơn giản là "đã được xử lý" sau lần đổi mới đầu tiên của nó. Nó vẫn đủ điều kiện cho đến khi trọng lượng của nó đạt 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\log^2 n+n\log n\log\log W)) khấu hao | HLD tạo các khoảng phân đoạn (O(\log n)) cho mỗi truy vấn, trong khi mỗi cạnh chỉ thay đổi (O(\log\log W)), nhiều nhất là năm lần | 
| Không gian | (O(n)) | Cây, mảng HLD và cây phân đoạn đều sử dụng bộ nhớ tuyến tính | 

Đối với (n,m\le2\cdot10^5), thực tế quan trọng là phần tốn kém của việc cập nhật đường dẫn không thể diễn ra vô thời hạn. Mỗi trọng lượng đường chỉ trải qua một số cấp căn bậc hai trước khi đạt đến 1. Cây phân đoạn cũng thu gọn các phạm vi thống nhất thành các bài tập lười biếng. Việc triển khai sử dụng tính năng tiền xử lý cây lặp và mảng số nguyên nhỏ gọn để duy trì mức sử dụng bộ nhớ Python ở mức hợp lý dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định`solve()`chức năng từ giải pháp trên có mặt. Nó chuyển hướng đầu vào và đầu ra tiêu chuẩn để mỗi đầu vào hoàn chỉnh có thể được kiểm tra độc lập.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official sample
sample = """\
5 3
1 2 4
2 3 4
1 4 9
1 5 16
1 5
1 3
1 4
"""
assert run(sample) == "140\n92\n72\n48", "official sample"

# Minimum-size tree, no edges at all.
case_min = """\
1 1
1 1
"""
assert run(case_min) == "0\n0", "single city"

# Weight 1 must never change.
case_one = """\
2 1
1 2 1
1 2
"""
assert run(case_one) == "1\n1", "weight already one"

# All equal values and repeated full-path updates.
case_equal = """\
4 2
1 2 4
2 3 4
3 4 4
1 4
1 4
"""
assert run(case_equal) == "40\n20\n10", "all equal weights"

# Boundary sequence 16 -> 4 -> 2 -> 1 and 9 -> 3 -> 1.
case_repeated = """\
3 3
1 2 16
2 3 9
1 3
1 3
1 3
"""
assert run(case_repeated) == "50\n14\n6\n4", "repeated square roots"

# Maximum-size structural test.
# A path of 200000 vertices with every edge equal to 1.
# Every query is the whole path, so the answer never changes.
n = 200000
m = 200000

initial = n * (n - 1) * (n + 1) // 6
expected_line = str(initial % 1_000_000_007) + "\n"
expected = expected_line * (m + 1)

parts = [f"{n} {m}"]
for i in range(1, n):
    parts.append(f"{i} {i + 1} 1")
for _ in range(m):
    parts.append(f"1 {n}")

max_case = "\n".join(parts) + "\n"

assert run(max_case) == expected, "maximum-size all-one path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`, truy vấn`1 1`|`0, 0`| Không có cạnh và đường dẫn trống | 
| Hai thành phố có trọng số cạnh 1 |`1, 1`| Đã có trọng lượng tối thiểu | 
| Con đường bốn thành phố với mọi trọng lượng 4 |`40, 20, 10`| Giá trị bằng nhau và cập nhật toàn bộ đường dẫn lặp đi lặp lại | 
| Đường ba thành trọng số 16 và 9 |`50, 14, 6, 4`| Nhiều cấp căn bậc hai và truy vấn lặp đi lặp lại | 
| Đường đi 200000 thành phố với tất cả trọng số 1 | Cùng một giá trị trên tất cả 200001 dòng | Tối đa (n, m), đường dẫn dài và bỏ qua phạm vi không thay đổi | 

## Vỏ cạnh 

### Một thành phố duy nhất 

cho```
1 1
1 1
```cây phân đoạn chỉ chứa vị trí gốc nhân tạo. Hệ số của nó bằng 0, do đó đóng góp có trọng số của nó bằng 0. Cập nhật đường dẫn sẽ thấy rằng cả hai điểm cuối đều có cùng một đỉnh và trả về mà không chạm vào cây phân đoạn. Đầu ra là```
0
0
```Việc triển khai xử lý việc này vì tất cả các khoảng HLD đều trống khi`u == v`và cây phân đoạn vẫn có gốc một phần tử hợp lệ. 

### Trọng lượng đã bằng 1 

cho```
2 1
1 2 1
1 2
```cạnh duy nhất có hệ số (1). Đóng góp ban đầu của nó là (1). Trong quá trình truy vấn, cây phân đoạn sẽ thấy`mx == 1`và quay trở lại ngay lập tức. Không có sự phân công và thay đổi đóng góp nào xảy ra. Đầu ra là```
1
1
```Việc chấm dứt sớm này cũng là nguyên nhân khiến cho các truy vấn lặp lại trở nên rẻ sau khi tất cả các con đường trên một đường dẫn đã đạt tới 1. 

### Các phép biến đổi lặp đi lặp lại 

cho```
3 3
1 2 16
2 3 9
1 3
1 3
1 3
```truy vấn đầu tiên thay đổi (16\to4) và (9\to3). Thay đổi thứ hai (4\to2) và (3\to1). Thay đổi thứ ba (2\to1), trong khi cạnh còn lại đã là 1. Tổng số là (50,14,6,4). 

Giá trị tối thiểu và tối đa của cây phân đoạn làm cho truy vấn thứ ba trở nên hiệu quả. Khi một phạm vi chỉ chứa các trọng số 1, giá trị tối đa của nó là 1 và quá trình đệ quy dừng trước khi truy cập bất kỳ lá nào. 

### Tổ tiên chung thấp nhất 

Hãy xem xét```
3 1
1 2 4
2 3 4
1 3
```Đường đi chứa cả hai cạnh, vì vậy câu trả lời ban đầu là 

[ 
4\cdot2+4\cdot2=16. 
] 

Sau truy vấn cả hai trọng số trở thành 2, đưa ra 

[ 
2\cdot2+2\cdot2=8. 
] 

Đầu ra là```
16
8
```Khi các điểm cuối nằm trên cùng một chuỗi nặng, bản cập nhật phải bao gồm các vị trí từ`pos[1] + 1`bởi vì`pos[3]`. Bản thân vị trí của LCA đại diện cho cạnh đi vào của LCA và phải được loại trừ. Đây chính xác là lý do tại sao khoảng HLD cuối cùng sử dụng`pos[v] + 1`. 

### Một con đường không có những thay đổi hiệu quả 

Giả sử một cây lớn chứa nhiều cạnh có trọng số 1 và đường dẫn truy vấn bao gồm toàn bộ các cạnh đó. Câu trả lời trước và sau truy vấn giống hệt nhau. Cây phân đoạn nhận ra điều này bằng cách sử dụng giá trị tối đa. Truy vấn vẫn thực hiện phân tách HLD, nhưng mọi phân đoạn ngay lập tức trả về mà không giảm dần. Điều này ngăn hành vi trong trường hợp xấu nhất phụ thuộc vào độ dài vật lý của đường dẫn không thay đổi.
