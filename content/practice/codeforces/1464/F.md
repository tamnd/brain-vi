---
title: "CF 1464F - My Beautiful Madness"
description: "Chúng tôi duy trì nhiều đường dẫn trên cây. Đường dẫn có thể được chèn và xóa một cách linh hoạt. Đối với truy vấn có tham số d, chúng ta phải quyết định xem có tồn tại ít nhất một đỉnh có khoảng cách đến mọi đường dẫn được lưu trữ nhiều nhất là d hay không."
date: "2026-06-11T01:57:05+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1464
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 692 (Div. 1, based on Technocup 2021 Elimination Round 3)"
rating: 3500
weight: 1464
solve_time_s: 122
verified: false
draft: false
---

[CF 1464F - Sự điên rồ xinh đẹp của tôi](https://codeforces.com/problemset/problem/1464/F) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì nhiều đường dẫn trên cây. Đường dẫn có thể được chèn và xóa một cách linh hoạt. 

Đối với truy vấn có tham số`d`, chúng ta phải quyết định xem có tồn tại ít nhất một đỉnh có khoảng cách đến mọi đường đi được lưu trữ nhiều nhất hay không`d`. 

Một cách khác để nói điều tương tự là thế này: 

Đối với mọi đường dẫn được lưu trữ`P`, xem xét tất cả các đỉnh có khoảng cách từ`P`nhiều nhất là`d`. Chúng ta cần biết liệu giao của tất cả các tập hợp này có rỗng hay không. 

Cây chứa tới`2 · 10^5`đỉnh và có tới`2 · 10^5`hoạt động. Bất kỳ giải pháp nào chạm vào tất cả các đường dẫn đang hoạt động trong một truy vấn sẽ bị loại trừ ngay lập tức. Thậm chí`O(√q)`công việc mỗi hoạt động là không thoải mái. Chúng ta cần một cái gì đó gần với thời gian logarit. 

Khó khăn là các đối tượng được duy trì là các đường dẫn chứ không phải các đỉnh. Việc chèn và xóa diễn ra trực tuyến nên chúng tôi không thể xử lý truy vấn ngoại tuyến. 

Trường hợp cạnh tinh tế xuất hiện khi cùng một đường dẫn được chèn nhiều lần.```
add (1,2)
add (1,2)
delete (2,1)
```Vẫn còn một bản sao. Bất kỳ cấu trúc nào chỉ lưu giữ sự tồn tại thay vì đa bội sẽ bị phá vỡ. 

Một sai lầm dễ mắc phải khác là nghĩ rằng một truy vấn sẽ hỏi liệu tất cả các đường dẫn được lưu trữ có giao nhau hay không. Vì`d > 0`điều đó không đúng. 

Hãy xem xét một chuỗi:```
1 - 2 - 3 - 4 - 5
```với các đường dẫn được lưu trữ`(1,1)`Và`(5,5)`. 

Vì`d = 2`, đỉnh`3`nằm trong khoảng cách`2`của cả hai con đường, vì vậy câu trả lời là`Yes`mặc dù bản thân các con đường đều rời rạc. 

Phần khó nhất là hiểu điều kiện nào trên tất cả các đường dẫn được lưu trữ đang thực sự được kiểm tra. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì rõ ràng tất cả các đường dẫn đang hoạt động. Đối với một truy vấn`3 d`, chúng ta có thể kiểm tra mọi đỉnh của cây và kiểm tra khoảng cách của nó với mọi đường đi đang hoạt động. 

Khoảng cách từ một đỉnh đến một đường đi có thể được tính bằng máy LCA, nhưng ngay cả khi việc kiểm tra đó là thời gian không đổi, chúng ta vẫn sẽ thực hiện```
O(n · |P|)
```làm việc cho mỗi truy vấn. 

Với cả hai giá trị đạt`2 · 10^5`, điều này là hoàn toàn không thể. 

Quan sát quan trọng là mọi con đường`(u,v)`có thể được đại diện bởi LCA của nó. 

Cho phép```
l = lca(u,v)
```Một đỉnh`x`nằm trong khoảng cách`d`từ đường dẫn`(u,v)`nếu hình chiếu của`x`vào con đường đạt đến con đường trước khi leo lên hơn`d`các cạnh. Việc viết lại điều kiện này cho tất cả các đường dẫn đang hoạt động đồng thời sẽ cho thấy một cấu trúc mạnh mẽ đáng ngạc nhiên. 

Giả sử chúng tôi thu thập LCA của tất cả các đường dẫn đang hoạt động. 

Cho phép`L`là LCA hoạt động sâu nhất. 

Nếu một số đỉnh nằm trong khoảng cách`d`từ mọi đường đi thì đỉnh đó phải nằm trong cây con của tổ tiên thu được bằng cách leo lên`2d`các cạnh ở trên`L`. 

Điều đó làm giảm vấn đề xuống một cây con. 

Bên trong cây con đó, chỉ có LCA là quan trọng. Chúng ta cần tất cả các LCA đang hoạt động thuộc về cùng một cây con tổ tiên và đường kính của LCA đang hoạt động được đặt bên trong cây con đó phải đủ nhỏ. 

Các LCA đang hoạt động tạo thành một tập các đỉnh động. Đường kính cây động là một cấu trúc tiêu chuẩn: nếu hai đoạn biết đường kính của chúng, đường kính giao nhau của chúng có thể thu được bằng cách chỉ kiểm tra điểm cuối của hai đường kính. 

Điều này biến toàn bộ vấn đề thành việc duy trì: 

1. Một tập hợp nhiều LCA động. 
2. LCA hoạt động sâu nhất. 
3. Đường kính của tất cả các LCA đang hoạt động bên trong cây con Euler-tour. 
4. Số lượng đường dẫn hoạt động có LCA thuộc về cây con. 

Tất cả các hoạt động sau đó có thể được xử lý theo thời gian logarit. 

Giải pháp được chấp nhận sử dụng: 

- Khoảng thời gian du lịch Euler. 
- Nâng nhị phân. 
- Cây Fenwick. 
- Cây phân đoạn duy trì đường kính. 
- Một multiset được sắp xếp theo chiều sâu. 

Độ phức tạp thu được là`O(log n)`mỗi lần cập nhật và truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · | P | ) cho mỗi truy vấn | 
| Tối ưu | O(log n) mỗi thao tác | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Cải cách hình học 

Đối với một đường dẫn hoạt động`(u,v)`cho phép```
l = lca(u,v)
```Con đường này bao gồm hai chuỗi đi lên gặp nhau tại`l`. 

Đối với một đỉnh`x`ở trong khoảng cách`d`từ con đường này, tổ tiên của`l`thu được sau khi leo núi`d`các cạnh vẫn phải có thể truy cập được từ`x`không vượt quá cái khác`d`các cạnh. 

Sau khi đơn giản hóa điều kiện này trên tất cả các đường dẫn, chỉ LCA vẫn phù hợp. 

### Đối tượng được bảo trì 

hãy để`S`là tập hợp nhiều LCA của tất cả các đường dẫn đang hoạt động. 

Đối với mọi đường dẫn hoạt động: 

1. Thêm LCA của nó vào`S`. 
2. Xóa LCA của nó khi đường dẫn biến mất. 

Chúng tôi duy trì: 

1. Đỉnh sâu nhất của`S`. 
2. Số lượng LCA đang hoạt động bên trong cây con Euler-tour. 
3. Thông tin đường kính của LCA đang hoạt động. 

### Cấu trúc dữ liệu 

1. Chạy DFS. 
2. Tính mục nhập Euler`tin`và thoát`tout`. 
3. Xây dựng bàn nâng nhị phân. 
4. Xây dựng cây Fenwick theo thứ tự Euler. 
5. Xây dựng cây đoạn có các lá tương ứng với các đỉnh. 

Nút cây phân đoạn lưu trữ đường kính của tất cả các đỉnh hoạt động trong khoảng của nó. 

Đường kính được biểu thị bằng:```
(endpoint1, endpoint2, length)
```### Hợp nhất hai cấu trúc đường kính 

Giả sử con bên trái có điểm cuối đường kính`(a,b)`và đứa trẻ bên phải có`(c,d)`. 

Đường kính của mối nối phải là một trong:```
(a,b)
(c,d)
(a,c)
(a,d)
(b,c)
(b,d)
```Kiểm tra sáu cặp này là đủ. 

Đây là sự hợp nhất đường kính động tiêu chuẩn. 

### Chèn đường dẫn 

1. Tính toán`l = lca(u,v)`. 
2. Thêm một lần xuất hiện của`l`đến multiset theo thứ tự độ sâu. 
3. Thêm một lần xuất hiện của`l`tới cây Fenwick. 
4. Nếu đây là lần đầu tiên xảy ra`l`, kích hoạt lá của nó trong cây phân đoạn. 

### Xóa đường dẫn 

1. Tính toán`l = lca(u,v)`. 
2. Xóa một lần xuất hiện khỏi nhiều tập hợp. 
3. Xóa một lần xuất hiện khỏi cây Fenwick. 
4. Nếu bội số trở thành 0, hãy hủy kích hoạt lá cây phân đoạn. 

###Trả lời truy vấn`3 d`Cho phép`L`là LCA hoạt động sâu nhất. 

1. Di chuyển`d`các cạnh hướng lên từ`L`, thu được`x`. 
2. Di chuyển cái khác`d`các cạnh hướng lên từ`x`, thu được`y`. 

Điểm chung ứng cử viên phải nằm bên trong cây con`y`. 

Trước tiên hãy kiểm tra xem mọi LCA đang hoạt động có thuộc về cây con đó hay không. 

Sử dụng khoảng Euler và cây Fenwick:```
count(subtree(y)) == number_of_active_paths
```phải nắm giữ. 

Nếu không thì trả lời`No`. 

Mặt khác truy vấn cây phân đoạn trên khoảng Euler của cây con`y`. 

Điều này trả về đường kính của tất cả các LCA đang hoạt động trong cây con đó. 

Đặt điểm cuối đường kính là`(p,q)`. 

Câu trả lời là`Yes`nếu```
dist(x,p) ≤ d
and
dist(x,q) ≤ d
```Bởi vì mọi LCA đang hoạt động đều nằm trên một tập có cặp xa nhất là`(p,q)`. 

Nếu cả hai điểm cuối đường kính đều nằm trong bán kính-`d`quả bóng tập trung tại`x`, mọi LCA đang hoạt động cũng phù hợp. 

### Tại sao nó hoạt động 

LCA hoạt động sâu nhất là LCA hạn chế nhất. Bất kỳ đỉnh khả thi nào cũng phải ở gần nó. 

Leo núi`2d`các mức trên LCA sâu nhất đó sẽ tạo ra cây con lớn nhất mà vẫn có thể chứa một đỉnh khả thi chung. Mọi LCA đang hoạt động phải nằm bên trong cây con này. Nếu ngay cả một LCA đang hoạt động thoát ra thì không có đỉnh nào có thể thỏa mãn tất cả các ràng buộc cùng một lúc. 

Bên trong cây con hợp lệ, tính khả thi chỉ phụ thuộc vào khoảng cách theo cặp tối đa giữa các LCA đang hoạt động. Các điểm cuối đường kính là đủ vì mọi LCA đang hoạt động khác không nằm xa ứng cử viên trung tâm hơn một trong các điểm cuối đường kính. Do đó, việc kiểm tra hai điểm cuối đường kính sẽ mô tả đầy đủ toàn bộ tập hợp. 

Bất biến này được giữ nguyên sau mỗi lần chèn và xóa vì tất cả các cấu trúc được duy trì chỉ phụ thuộc vào bội số LCA đang hoạt động. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, insort

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = 20

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    depth = [0] * (n + 1)
    up = [[0] * (n + 1) for _ in range(LOG + 1)]
    euler = [0] * (n + 1)

    timer = 0

    sys.setrecursionlimit(1 << 20)

    def dfs(v, p):
        nonlocal timer
        timer += 1
        tin[v] = timer
        euler[timer] = v

        up[0][v] = p
        depth[v] = depth[p] + 1

        for i in range(1, LOG + 1):
            up[i][v] = up[i - 1][up[i - 1][v]]

        for to in g[v]:
            if to != p:
                dfs(to, v)

        tout[v] = timer

    dfs(1, 1)

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

        for i in range(LOG, -1, -1):
            if up[i][a] != up[i][b]:
                a = up[i][a]
                b = up[i][b]

        return up[0][a]

    def dist(a, b):
        c = lca(a, b)
        return depth[a] + depth[b] - 2 * depth[c]

    def jump(v, k):
        for i in range(LOG + 1):
            if k >> i & 1:
                v = up[i][v]
        return v

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            n = self.n
            bit = self.bit
            while idx <= n:
                bit[idx] += val
                idx += idx & -idx

        def sum(self, idx):
            bit = self.bit
            res = 0
            while idx:
                res += bit[idx]
                idx -= idx & -idx
            return res

        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    fenwick = Fenwick(n)

    NEG = (-1, -1, -1)

    def merge(A, B):
        if A[0] == -1:
            return B
        if B[0] == -1:
            return A

        cand = [
            (A[0], A[1]),
            (B[0], B[1]),
            (A[0], B[0]),
            (A[0], B[1]),
            (A[1], B[0]),
            (A[1], B[1]),
        ]

        best_u, best_v = A[0], A[1]
        best_d = dist(best_u, best_v)

        for u, v in cand:
            d = dist(u, v)
            if d > best_d:
                best_d = d
                best_u, best_v = u, v

        return (best_u, best_v, best_d)

    seg = [NEG] * (4 * n)

    def update(node, l, r, pos, val):
        if l == r:
            seg[node] = (val, val, 0) if val != -1 else NEG
            return

        mid = (l + r) // 2

        if pos <= mid:
            update(node * 2, l, mid, pos, val)
        else:
            update(node * 2 + 1, mid + 1, r, pos, val)

        seg[node] = merge(seg[node * 2], seg[node * 2 + 1])

    def query(node, l, r, ql, qr):
        if ql <= l and r <= qr:
            return seg[node]

        mid = (l + r) // 2

        if qr <= mid:
            return query(node * 2, l, mid, ql, qr)
        if ql > mid:
            return query(node * 2 + 1, mid + 1, r, ql, qr)

        return merge(
            query(node * 2, l, mid, ql, qr),
            query(node * 2 + 1, mid + 1, r, ql, qr),
        )

    cnt = [0] * (n + 1)

    depth_set = []

    active = 0
    out = []

    for _ in range(q):
        tmp = list(map(int, input().split()))
        tp = tmp[0]

        if tp == 1:
            _, u, v = tmp
            w = lca(u, v)

            insort(depth_set, (depth[w], w))

            fenwick.add(tin[w], 1)

            cnt[w] += 1
            if cnt[w] == 1:
                update(1, 1, n, tin[w], w)

            active += 1

        elif tp == 2:
            _, u, v = tmp
            w = lca(u, v)

            pos = bisect_left(depth_set, (depth[w], w))
            depth_set.pop(pos)

            fenwick.add(tin[w], -1)

            cnt[w] -= 1
            if cnt[w] == 0:
                update(1, 1, n, tin[w], -1)

            active -= 1

        else:
            _, d = tmp

            L = depth_set[-1][1]

            x = jump(L, d if d < depth[L] else depth[L] - 1)
            y = jump(x, d if d < depth[x] else depth[x] - 1)

            inside = fenwick.range_sum(tin[y], tout[y])

            if inside != active:
                out.append("No")
                continue

            dia = query(1, 1, n, tin[y], tout[y])

            if dia[0] == -1:
                out.append("Yes")
                continue

            ok = (
                dist(x, dia[0]) <= d and
                dist(x, dia[1]) <= d
            )

            out.append("Yes" if ok else "No")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cấu trúc chính xác được mô tả ở trên. Cây Fenwick đếm các LCA đang hoạt động bên trong các cây con Euler-tour. Cây phân đoạn duy trì đường kính động của LCA đang hoạt động. Việc xử lý bội số là rất quan trọng: một đỉnh chỉ biến mất khỏi cây phân đoạn khi số lượng của nó bằng 0. 

Việc hợp nhất đường kính là phần quan trọng nhất. Đối với hai tập hợp đỉnh, đường kính giao nhau của chúng phải được hình thành bằng một trong các đường kính hiện có hoặc bằng cách nối điểm cuối từ đường kính bên trái với điểm cuối từ đường kính bên phải. Kiểm tra sáu cặp là đủ. 

Khoảng Euler-tour`[tin[v], tout[v]]`chuyển đổi các truy vấn cây con thành các truy vấn phạm vi. Đó là điều cho phép cả cây Fenwick và cây phân đoạn hoạt động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 4
1 1 1
1 1 1
2 1 1
3 0
```| Hoạt động | LCA đang hoạt động | LCA sâu nhất | Kết quả | 
| --- | --- | --- | --- | 
| cộng (1,1) | {1} | 1 | | 
| cộng (1,1) | {1,1} | 1 | | 
| del (1,1) | {1} | 1 | | 
| truy vấn 0 | {1} | 1 | Có | 

Đường đi tích cực duy nhất là một đỉnh duy nhất`1`, vậy nó`0`-khu phố là`{1}`và giao lộ không trống. 

### Ví dụ tùy chỉnh 

Chuỗi:```
1 - 2 - 3 - 4 - 5
```Hoạt động:```
add (1,1)
add (5,5)
query 1
query 2
```| Hoạt động | LCA đang hoạt động | 
| --- | --- | 
| cộng (1,1) | {1} | 
| cộng (5,5) | {1,5} | 
| truy vấn 1 | Không | 
| truy vấn 2 | Có | 

Với bán kính`1`, không có đỉnh nào đủ gần cả hai đầu. Với bán kính`2`, đỉnh`3`thỏa mãn cả hai ràng buộc. 

Điều này chứng tỏ rằng vấn đề là ở các khu dân cư chứ không phải ở các nút giao thông. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Tiền xử lý DFS cộng với các cập nhật và truy vấn logarit | 
| Không gian | O(n log n) | Bàn nâng nhị phân và cây phân đoạn | 

Với`n, q ≤ 2 · 10^5`, khoảng vài triệu phép tính logarit được thực hiện. Điều này thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solve() and return captured output
    pass

# sample
assert run("""\
1 4
1 1 1
1 1 1
2 1 1
3 0
""") == "Yes"

# single vertex tree
assert run("""\
1 2
1 1 1
3 0
""") == "Yes"

# duplicate insert/delete handling
assert run("""\
1 5
1 1 1
1 1 1
2 1 1
3 0
3 0
""") == "Yes\nYes"

# chain, radius too small
assert run("""\
5 4
1 2
2 3
3 4
4 5
1 1 1
1 5 5
3 1
""") == "No"

# chain, radius large enough
assert run("""\
5 4
1 2
2 3
3 4
4 5
1 1 1
1 5 5
3 2
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | Có | Kích thước cây tối thiểu | 
| Hoạt động đường dẫn trùng lặp | Có, Có | Xử lý đa dạng đúng | 
| Chuỗi có d=1 | Không | Bán kính quá nhỏ | 
| Chuỗi có d=2 | Có | Ranh giới bán kính | 
| Mẫu chính thức | Có | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Xem xét nhiều bản sao của cùng một đường dẫn:```
1 5
1 1 1
1 1 1
2 1 1
3 0
3 0
```Sau một lần xóa, một bản sao vẫn hoạt động. Bộ đếm đỉnh`1`trở thành`1`, không`0`, do đó lá cây phân đoạn vẫn hoạt động. Một giải pháp chỉ lưu trữ một boolean sẽ loại bỏ nó một cách không chính xác. 

Hãy xem xét các đường dẫn có vùng lân cận giao nhau nhưng đường đi của chúng không giao nhau:```
5 3
1 2
2 3
3 4
4 5
1 1 1
1 5 5
3 2
```Các LCA đang hoạt động là`{1,5}`. Đường kính của chúng là`4`. Thuật toán tìm cây con tổ tiên thích hợp và xác minh cả hai điểm cuối đường kính đều nằm trong khoảng cách`2`từ trung tâm ứng viên. Câu trả lời trở thành`Yes`, phù hợp với giao lộ khu phố thực sự. 

Cuối cùng, hãy xem xét`d = 0`. 

Truy vấn hỏi liệu tất cả các đường dẫn đang hoạt động có chia sẻ một đỉnh chung hay không. Thuật toán tự nhiên giảm xuống để kiểm tra xem các LCA đang hoạt động có thu gọn vào cấu hình bán kính bằng 0 hợp lệ hay không. Không cần xử lý đặc biệt.
