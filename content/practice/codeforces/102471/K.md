---
title: "CF 102471K - Lưu lượng tối đa cho tất cả các cặp"
description: "Đồ thị được vẽ bên trong một đa giác lồi có các đỉnh được đánh số theo chu kỳ. Mọi cạnh của đa giác đều có mặt và các đường chéo bổ sung có thể được thêm vào, nhưng các đường chéo không bao giờ cắt nhau ngoại trừ tại các điểm cuối chung. Mỗi cạnh có một khả năng không âm."
date: "2026-08-09T18:51:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "K"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 530
verified: true
draft: false
---

[CF 102471K - Lưu lượng tối đa cho tất cả các cặp](https://codeforces.com/problemset/problem/102471/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị được vẽ bên trong một đa giác lồi có các đỉnh được đánh số theo chu kỳ. Mọi cạnh của đa giác đều có mặt và các đường chéo bổ sung có thể được thêm vào, nhưng các đường chéo không bao giờ cắt nhau ngoại trừ tại các điểm cuối chung. Mỗi cạnh có một khả năng không âm. 

Đối với mỗi cặp đỉnh (s,t), chúng ta cần giá trị của luồng (s)-(t) tối đa, tương đương với công suất tối thiểu của một lần cắt (s)-(t). Đầu ra được yêu cầu là tổng của các giá trị này trên tất cả các cặp đỉnh không có thứ tự, modulo giảm (998244353). Vấn đề ban đầu có (n\le 200000) và (m\le 400000), với mọi cạnh đa giác đều có mặt. 

Các ràng buộc ngay lập tức loại trừ việc thực hiện bất kỳ điều gì một cách độc lập cho mỗi cặp. có 

[ 
\frac{n(n-1)}2 
] 

cặp, gần như là (2\cdot 10^{10}) khi (n=200000). Ngay cả một thao tác chỉ mất (O(m)) thời gian cho mỗi cặp cũng sẽ yêu cầu kiểm tra cạnh khoảng (8\cdot10^{15}). Thuật toán luồng cực đại thực sự đắt hơn đáng kể so với việc chỉ quét các cạnh, vì vậy giải pháp phải khai thác cấu trúc phẳng trên toàn cầu. 

Có một số trường hợp khó xử lý. Đồ thị nhỏ nhất là hình tam giác. Với ba cạnh của công suất một,```
3 3
1 2 1
2 3 1
3 1 1
```câu trả lời là`6`, không`3`. Mỗi cặp có hai đơn vị luồng vì cạnh trực tiếp và đường thay thế hai cạnh đều có thể mang luồng. 

Năng lực bằng không cũng là hợp pháp. Vì```
3 3
1 2 0
2 3 1
3 1 2
```câu trả lời là`4`. Cạnh công suất bằng 0 không làm cho mọi cặp bị ngắt kết nối. Ví dụ: luồng tối đa giữa các đỉnh (1) và (3) vẫn là (2), bởi vì phần cắt bao gồm hai cạnh liên quan đến đỉnh (1) có dung lượng (2). 

Dung lượng lớn yêu cầu số học 64 bit trước khi lấy mô đun cuối cùng. Vì```
3 3
1 2 1000000000
2 3 1000000000
3 1 1000000000
```mỗi cặp đều có luồng (2000000000), vì vậy câu trả lời chưa được sửa đổi là (6000000000). Đầu ra cần thiết là`10533882`. 

Cuối cùng, cạnh ((1,n)) cần được xử lý đặc biệt khi xây dựng cấu trúc phẳng. Nó vượt qua ranh giới tuần hoàn của việc đánh số đỉnh, do đó việc coi tất cả các cạnh chỉ là các khoảng ([u,v]) mà không tách rời cạnh này sẽ dẫn đến cấu trúc lồng không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Với mỗi cặp (s,t), hãy chạy thuật toán luồng cực đại và cộng kết quả của nó vào câu trả lời. Theo định lý cắt tối thiểu luồng cực đại thì điều này đúng, nhưng có (n(n-1)/2) lệnh gọi. Tại (n=200000), đó chính xác là (19999900000) tính toán luồng tối đa. Ngay cả khi mỗi phép tính chỉ mất (O(m)), tổng số sẽ là khoảng (8\cdot10^{15}) bài kiểm tra cạnh cơ bản. Việc tính toán lưu lượng cực đại thực sự khiến tình hình trở nên tồi tệ hơn nhiều. 

Quan sát hữu ích đến từ việc nhúng phẳng. Hãy xem xét một cạnh (e) hiện nằm trên ranh giới của vùng không bị chặn và giả sử nó có dung lượng tối thiểu trong số tất cả các cạnh biên đó. Gọi (C) là chu trình nhỏ nhất chứa (e). Chúng ta có thể loại bỏ (e), thêm dung lượng của nó vào mọi cạnh khác của (C) và bảo toàn mọi lần cắt tối thiểu theo cặp. Đây là mức giảm trung tâm được sử dụng cho vấn đề. 

Tại sao điều này làm việc? Trong đồ thị phẳng, đường cắt giữa hai đỉnh có thể được biểu diễn bằng một đường dẫn trong mặt phẳng kép sau khi mặt không giới hạn được phân chia ở hai đầu. Một chu trình (C) bao quanh một vùng giới hạn không thể đi qua một đường kép như vậy đúng một lần. Nếu một đường cắt tối ưu tương tác với (C), nó có thể được biểu diễn bằng (e) và một cạnh khác của (C). Vì (e) là cạnh ranh giới lộ ra hiện tại rẻ nhất nên việc thay thế đường giao nhau lộ ra bằng (e) không làm tăng giá trị cắt. 

Giả sử công suất hiện tại của (e) là (w). Việc xóa (e) sẽ thay đổi phần cắt sử dụng (e) bằng (-w). Việc thêm (w) vào mọi cạnh khác của (C) sẽ bù chính xác bất cứ khi nào vết cắt sử dụng cạnh khác của (C). Các vết cắt tránh (C) không thay đổi. Ngược lại, bất kỳ vết cắt nào trong biểu đồ đã sửa đổi đều có thể được mở rộng bằng (e) khi cần thiết để thu được vết cắt không có giá trị lớn hơn trong biểu đồ gốc. Do đó, mỗi lần cắt tối thiểu theo cặp vẫn không thay đổi. 

Chúng ta có thể lặp lại thao tác này cho đến khi chỉ còn lại (n-1) cạnh. Khi đó, đồ thị là một cây và luồng cực đại giữa hai đỉnh chỉ đơn giản là dung lượng cạnh tối thiểu trên đường đi duy nhất của chúng. Vấn đề tất cả các cặp ban đầu đã được giảm xuống thành vấn đề tắc nghẽn cây. 

Thách thức còn lại là tìm ra các chu trình thích hợp một cách hiệu quả. Mô phỏng trực tiếp sẽ yêu cầu duy trì ranh giới bên ngoài thay đổi của đồ thị phẳng. Một cách biểu diễn rõ ràng hơn sử dụng cây bắt nguồn từ các đường chéo không cắt nhau. Các đường chéo được sắp xếp bằng cách giảm điểm cuối bên trái và tăng điểm cuối bên phải. Một cấu trúc có thứ tự biểu diễn ranh giới hiện tại cho chúng ta biết cạnh nào trước đó thuộc về mặt mới được tạo bởi mỗi đường chéo. Điều này tạo ra một cây có (m+1) nút, trong đó mỗi cạnh của đồ thị ban đầu tương ứng với một cạnh của cây. 

Việc triển khai bên dưới sử dụng cây Fenwick thay vì bản đồ có thứ tự. Các mảnh ranh giới hoạt động được lưu trữ theo vị trí đa giác của chúng. Đối với đường chéo ((l,r)), tất cả các vị trí hoạt động trong ([l,r)) trở thành con của đường chéo đó và đường chéo sau đó được chèn vào vị trí (l). Vì mọi vị trí hoạt động chỉ bị xóa khi nó trở thành vị trí con nên tổng số lần xóa như vậy là tuyến tính. Mỗi chi phí hoạt động của Fenwick (O(\log n)). 

Khi cây phụ trợ này tồn tại, hãy xử lý các lá của nó từ dung lượng sẵn có nhỏ nhất trở lên. Khi một mặt bị lộ ra, cạnh được chọn của nó sẽ bị xóa và dung lượng của nó được truyền sang các cạnh tới khác. Hàng đợi ưu tiên duy trì cạnh ứng cử viên tiếp theo. Mỗi cạnh của cây phụ vào hàng đợi nhiều nhất một lần, nên tổng chi phí xử lý là (O(m\log m)). Đây là mức giảm tương tự được mô tả bằng cách tiếp cận giải pháp tiêu chuẩn cho vấn đề này.

Cuối cùng, các cạnh ban đầu (n-1) còn sót lại tạo thành một cây. Sắp xếp chúng theo công suất chuyển đổi theo thứ tự giảm dần. Ban đầu mỗi đỉnh ban đầu là một thành phần DSU riêng biệt. Khi một cạnh có trọng số (w) nối các thành phần có kích thước (a) và (b), chính xác các cặp đỉnh (ab) sẽ được kết nối lần đầu tiên ở ngưỡng (w), do đó (w\cdot a\cdot b) được thêm vào câu trả lời. Đây là phiên bản giảm dần của đối số đếm nút cổ chai Kruskal thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Kiểm tra cạnh ít nhất (O(n^2m)), nhiều hơn nữa với lưu lượng tối đa thực tế | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(m\log m+n\log n)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và tách các cạnh đa giác khỏi các đường chéo. Mỗi bên ((i,i+1)) được biểu thị bằng vị trí (i) của nó, trong khi bên đặc biệt ((1,n)) được biểu thị bằng vị trí (n). Mọi cạnh còn lại là một đường chéo ((l,r)) với (l<r). 
2. Sắp xếp tất cả các đường chéo theo thứ tự giảm dần (l), ngắt mối quan hệ bằng cách tăng dần (r). Thứ tự này tuân theo cấu trúc lồng nhau của các đường chéo không cắt nhau. Đường chéo sau này chỉ có thể tương tác với các phần ranh giới hiện đang lộ ra nằm trong khoảng của nó. 
3. Duy trì một cạnh hoạt động cho mỗi vị trí đa giác hiện đang lộ ra. Cây Fenwick lưu trữ các vị trí đang hoạt động, trong khi`edge_at[pos]`cho chúng ta biết cạnh gốc nào chiếm vị trí đó. Đối với đường chéo ((l,r)), liên tục tìm vị trí hoạt động đầu tiên trong ([l,r)). Gán cây phụ của cạnh đó cho đường chéo và xóa vị trí khỏi tập hiện hoạt. 
4. Sau khi tất cả các vị trí hoạt động trong ([l,r)) đã bị xóa, hãy chèn đường chéo vào vị trí (l). Điều này thể hiện bề mặt mới được tạo bằng một vị trí ranh giới đại diện duy nhất. Bởi vì đồ thị không giao nhau nên các mối quan hệ cha mẹ thu được sẽ tạo thành một cây. 
5. Sau khi tất cả các đường chéo đã được xử lý, hãy kết nối mọi cạnh ranh giới vẫn đang hoạt động với một nút gốc duy nhất (m+1). Mỗi cạnh gốc (i) bây giờ tương ứng với cạnh cây phụ giữa nút (i) và nút`fa[i]`. 
6. Ban đầu, mỗi lá của cây phụ biểu thị một mặt có đúng một cạnh biên dẫn đến cây mẹ của nó. Đặt tất cả các cạnh như vậy thành một đống tối thiểu và đánh dấu chiếc lá tương ứng là đã lộ ra. Khóa heap là dung lượng hiện được liên kết với cạnh ranh giới đó. 
7. Loại bỏ mục nhập heap nhỏ nhất. Nếu điểm cuối khác của cạnh phụ đó đã lộ ra thì cạnh đó đã trở thành một phần của ranh giới đã được xử lý, do đó, hãy thêm giá trị hiện tại vào trọng số được chuyển đổi của nó. 
8. Nếu không, hãy để lộ điểm cuối khác. Cạnh được sử dụng để hiển thị nó sẽ bị xóa khỏi biểu đồ đã chuyển đổi, vì vậy hãy đánh dấu trọng số đã chuyển đổi của nó bằng trọng số rất âm. Dung lượng được sử dụng để lộ mặt sau đó sẽ được thêm vào mọi cạnh của cây phụ trợ sự cố khác. Nếu mặt lân cận vẫn chưa lộ ra, hãy đẩy cạnh đó vào heap với dung lượng mới tăng của nó. 
9. Tiếp tục cho đến khi heap trống. Mỗi cạnh ban đầu được đánh dấu bằng trọng số biến đổi không âm là một trong các cạnh (n-1) của cây cuối cùng. Mỗi cạnh bị xóa đều có trọng điểm âm. 
10. Sắp xếp các cạnh còn lại theo trọng số biến đổi theo thứ tự giảm dần. Khởi tạo DSU với một thành phần cho mỗi đỉnh đồ thị gốc. Với mỗi cạnh cây ((u,v)) có trọng số (w), hãy tìm hai thành phần hiện tại chứa điểm cuối của nó. Nếu kích thước của chúng là (a) và (b), hãy thêm (wab) vào câu trả lời và hợp nhất các thành phần. 
11. Giảm modulo câu trả lời tích lũy (998244353) và in nó. 

Bất biến đằng sau toàn bộ thuật toán là mọi phép biến đổi đều bảo toàn mức cắt tối thiểu giữa mọi cặp đỉnh ban đầu. Hoạt động chu trình bảo toàn từng giá trị cắt, do đó, sau khi xóa tất cả, cây cuối cùng có chính xác các giá trị luồng cực đại theo cặp giống như biểu đồ ban đầu. Trên cây, giá trị của một cặp là cạnh tối thiểu trên đường đi duy nhất của nó. Xử lý DSU giảm dần nhóm chính xác các cặp có đường dẫn tối thiểu là trọng số cạnh hiện tại, do đó mỗi cặp đóng góp chính xác một lần. Điều này chứng tỏ rằng tổng cuối cùng là tổng cần thiết của tất cả các luồng tối đa. 

## Giải pháp Python```python
import sys
import heapq
from array import array

input = sys.stdin.readline

MOD = 998244353
NEG = -(10 ** 18)

def solve():
    n, m = map(int, input().split())

    # Original endpoints and capacities.
    eu = array('i', [0]) * (m + 1)
    ev = array('i', [0]) * (m + 1)
    cap = array('q', [0]) * (m + 1)

    # fa[i] is the parent of auxiliary-tree node i.
    fa = array('i', [0]) * (m + 2)

    # Active boundary position -> original edge id.
    edge_at = array('i', [0]) * (n + 1)

    diagonals = []

    for eid in range(1, m + 1):
        u, v, w = map(int, input().split())
        if u > v:
            u, v = v, u

        eu[eid] = u
        ev[eid] = v
        cap[eid] = w

        if v == u + 1:
            edge_at[u] = eid
        elif u == 1 and v == n:
            edge_at[n] = eid
        else:
            diagonals.append((u, v, eid))

    # Fenwick tree containing 1 exactly at active positions.
    bit = array('i', [0]) * (n + 1)
    for i in range(1, n + 1):
        bit[i] = i & -i

    def bit_add(i, delta):
        while i <= n:
            bit[i] += delta
            i += i & -i

    def bit_sum(i):
        s = 0
        while i > 0:
            s += bit[i]
            i -= i & -i
        return s

    def bit_kth(k):
        """Return the position of the k-th active point, 1-indexed."""
        pos = 0
        step = 1 << (n.bit_length() - 1)
        while step:
            nxt = pos + step
            if nxt <= n and bit[nxt] < k:
                k -= bit[nxt]
                pos = nxt
            step >>= 1
        return pos + 1

    # The nesting order of noncrossing diagonals.
    diagonals.sort(key=lambda x: (-x[0], x[1]))

    for l, r, eid in diagonals:
        before = bit_sum(l - 1)
        right = bit_sum(r - 1)

        # Every active position in [l, r) becomes a child of eid.
        while before < right:
            pos = bit_kth(before + 1)
            old_edge = edge_at[pos]

            fa[old_edge] = eid
            edge_at[pos] = 0
            bit_add(pos, -1)

            right -= 1

        # The new diagonal becomes the representative of this face.
        edge_at[l] = eid
        bit_add(l, 1)

    root = m + 1

    # Remaining active boundary pieces belong to the outer root.
    for pos in range(1, n + 1):
        eid = edge_at[pos]
        if eid:
            fa[eid] = root

    # The auxiliary tree is represented by parent -> children links.
    head = array('i', [-1]) * (m + 2)
    nxt = array('i', [-1]) * (m + 1)

    for eid in range(1, m + 1):
        p = fa[eid]
        nxt[eid] = head[p]
        head[p] = eid

    # nw[i] is the final transformed weight of original edge i.
    nw = array('q', [0]) * (m + 1)
    vis = bytearray(m + 2)

    heap = []

    # Every non-root leaf is initially an exposed face.
    for i in range(1, m + 1):
        if head[i] == -1:
            heapq.heappush(heap, (cap[i], i, fa[i]))
            vis[i] = 1

    while heap:
        cur, u, v = heapq.heappop(heap)

        if vis[v]:
            # Both sides are already exposed. The edge contributes
            # its current value to its transformed weight.
            nw[u] += cur
            continue

        # Expose face v through auxiliary edge u-v.
        vis[v] = 1
        nw[u] = NEG

        # The parent side of v is another incident edge.
        p = fa[v]
        if p != 0:
            if vis[p]:
                nw[v] += cur
            else:
                heapq.heappush(heap, (cur + cap[v], v, p))

        # Process all child sides of v except the edge we arrived through.
        c = head[v]
        while c != -1:
            if c != u:
                if vis[c]:
                    nw[c] += cur
                else:
                    heapq.heappush(heap, (cur + cap[c], v, c))
            c = nxt[c]

    # The non-deleted edges form the final tree.
    tree_edges = []
    for eid in range(1, m + 1):
        if nw[eid] >= 0:
            tree_edges.append((nw[eid], eu[eid], ev[eid]))

    tree_edges.sort(reverse=True)

    # Descending Kruskal on the final tree.
    parent = array('i', range(n + 1))
    size = array('i', [1]) * (n + 1)

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    ans = 0

    for w, u, v in tree_edges:
        ru = find(u)
        rv = find(v)

        if ru == rv:
            continue

        if size[ru] < size[rv]:
            ru, rv = rv, ru

        ans = (
            ans
            + (w % MOD) * (size[ru] % MOD) % MOD
            * (size[rv] % MOD)
        ) % MOD

        parent[rv] = ru
        size[ru] += size[rv]

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên lưu trữ mọi cạnh gốc trong mảng số nguyên nhỏ gọn. Danh sách hàng trăm nghìn số nguyên Python có thể tiêu tốn bộ nhớ đáng kể, vì vậy`array`Và`bytearray`được sử dụng cho các cấu trúc số lớn. 

Việc xây dựng đường chéo sử dụng cây Fenwick như một tập hợp có thứ tự.`bit_sum(r-1)-bit_sum(l-1)`cho chúng ta biết có bao nhiêu đại diện ranh giới đang hoạt động nằm trong ([l,r)).`bit_kth`xác định vị trí đầu tiên, sau đó vị trí đó sẽ bị xóa. Mỗi lần xóa chỉ xảy ra một lần, do đó, mặc dù chi phí của mỗi thao tác riêng lẻ (O(\log n)), tổng số lần xóa là tuyến tính. 

Cây phụ sử dụng`head`Và`nxt`thay vì danh sách các bộ dữ liệu Python. Nút (i) có nút cha`fa[i]`, Và`head[v]`sinh con đầu lòng của (v). Biểu diễn này là đủ vì cây được bắt rễ ở nút mặt ngoài nhân tạo. 

Các cửa hàng đống`(current_capacity, child, parent)`. Ứng cử viên chỉ được chèn khi một mặt của cạnh phụ lộ ra trong khi mặt còn lại thì không. Do đó, mỗi cạnh phụ tạo ra nhiều nhất một mục nhập heap. Khi một mục nhập được xử lý,`vis[v]`cho chúng ta biết liệu khuôn mặt kia đã bị lộ chưa. 

Trọng tâm tiêu cực được cố tình nhỏ hơn nhiều so với bất kỳ khả năng chuyển đổi nào có thể có. Mỗi dung lượng ban đầu tối đa là (10^9) và có nhiều nhất (4\cdot10^5) bổ sung, do đó mọi dung lượng được chuyển đổi hợp lệ đều ở dưới (4\cdot10^{14}).`-10**18`do đó được tách biệt một cách an toàn khỏi tất cả các giá trị hợp lệ. 

DSU cuối cùng xử lý các cạnh theo thứ tự giảm dần. Bởi vì đồ thị còn lại là một cây nên điểm cuối của mỗi cạnh còn lại nằm trong các thành phần DSU khác nhau khi cạnh đó được xử lý. Tích các kích thước thành phần của chúng đếm chính xác các cặp có đường đi đầu tiên được kết nối ở ngưỡng của cạnh này. 

Số nguyên Python không bị tràn, nhưng dung lượng lưu trữ của mảng sử dụng số nguyên 64 bit có dấu. Công suất chuyển đổi lớn nhất có thể nằm thoải mái trong phạm vi đó. Câu trả lời là giảm modulo (998244353) sau mỗi lần đóng góp DSU. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu có sáu đỉnh đa giác và hai đường chéo. Các khoảng đường chéo là ((1,4)) và ((1,5)), do đó việc xây dựng cây phụ sẽ đưa ra các mối quan hệ cha sau: 

[ 
1,2,3\rightarrow7,\qquad 
7,4\rightarrow8,\qquad 
8,5,6\phảiarrow9. 
] 

Ở đây các nút (1) đến (6) đại diện cho các cạnh đa giác, các nút (7) và (8) đại diện cho hai đường chéo và nút (9) là gốc nhân tạo bên ngoài. 

Các trạng thái xử lý quan trọng là: 

| Bước | Giá trị cạnh đống | Nút tiếp xúc | Trọng lượng chuyển đổi quan trọng | 
| --- | --- | --- | --- | 
| 1 | 1 | 7 | (w_2=1,\ w_3=1,\ w_1=-\infty) | 
| 2 | 10 | 7 đã được tiếp xúc | (w_2=11) | 
| 3 | 100 | 7 đã được tiếp xúc | (w_3=101) | 
| 4 | 1000 | 8 | (w_4=-\infty,\ w_7=1000) | 
| 5 | 10000 | 9 | (w_5=-\infty,\ w_6=10000,\ w_8=10000) | 
| 6 | 100000 | 9 đã lộ | (w_6=110000) | 
| 7 | 1000001 | 8 đã lộ | (w_7=1001001) | 
| 8 | 10001000 | 9 đã lộ | (w_8=10011000) | 

Do đó, các cạnh còn sót lại là: 

| Cạnh | Điểm cuối | Công suất chuyển đổi | 
| --- | --- | --- | 
| 2 | (2,3) | 11 | 
| 3 | (3,4) | 101 | 
| 6 | (6,1) | 110000 | 
| 7 | (1,4) | 1001001 | 
| 8 | (1,5) | 10011000 | 

Cách tính DSU giảm dần là: 

| Cân nặng | Kích thước thành phần | Đóng góp theo cặp | 
| --- | --- | --- | 
| 10011000 | (1\cdot1) | 10011000 | 
| 1001001 | (2\cdot1) | 2002002 | 
| 110000 | (3\cdot1) | 330000 | 
| 101 | (4\cdot1) | 404 | 
| 11 | (5\cdot1) | 55 | 

Tổng số tiền là 

[ 
10011000+2002002+330000+404+55 
=12343461. 
] 

Điều này thể hiện sự giảm thiểu hoàn toàn: đồ thị phẳng phức tạp trở thành một cây năm cạnh có các giá trị thắt cổ chai mã hóa mọi luồng cực đại theo cặp ban đầu. Mẫu chính thức có cùng đầu ra. 

### Tam giác tùy chỉnh 

Hãy xem xét```
3 3
1 2 1
2 3 1
3 1 1
```Không có đường chéo nên cây phụ chỉ đơn giản là một gốc nối với cả ba cạnh của đa giác. 

| Bước | Giá trị đống | Nút tiếp xúc | Trọng lượng chuyển đổi | 
| --- | --- | --- | --- | 
| 1 | 1 | gốc | (w_1=-\infty,\ w_2=1,\ w_3=1) | 
| 2 | 1 | root đã bị lộ | (w_2=2) | 
| 3 | 1 | root đã bị lộ | (w_3=2) | 

Cây sống sót chứa hai cạnh của dung lượng (2). Đóng góp của DSU là (2\cdot1\cdot1=2) và (2\cdot2\cdot1=4), mang lại`6`. 

Ví dụ này xác nhận rằng một chu trình có thể cung cấp nhiều luồng hơn bất kỳ cạnh đơn nào. Việc giảm thiểu sẽ duy trì khả năng kết nối bổ sung đó bằng cách tăng khả năng sống sót của cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\log m+n\log n)) | Sắp xếp theo đường chéo, hoạt động Fenwick, xử lý đống và sắp xếp cạnh cuối cùng | 
| Không gian | (O(n+m)) | Các cạnh gốc, cây phụ, cây Fenwick, đống và DSU | 

Đầu vào có (m\le400000), vì vậy (m) chỉ là hệ số không đổi lớn hơn (n). Do đó, thuật toán hoạt động như (O(m\log m)), phù hợp với các giới hạn dự định. Các cấu trúc lớn trong quá trình triển khai Python sử dụng các mảng nhỏ gọn khi thực tế, giữ cho bộ nhớ tỷ lệ thuận với kích thước biểu đồ. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve()`chức năng từ phần Giải pháp Python có sẵn.```python
import sys
import io

# helper: run solution on input string, return output string
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""\
6 8
1 2 1
2 3 10
3 4 100
4 5 1000
5 6 10000
6 1 100000
1 4 1000000
1 5 10000000
""") == "12343461", "sample"

# Minimum-size graph, all equal capacities.
# Every pair has flow 2, and there are 3 pairs.
assert run("""\
3 3
1 2 1
2 3 1
3 1 1
""") == "6", "minimum triangle"

# Zero capacity boundary edge.
assert run("""\
3 3
1 2 0
2 3 1
3 1 2
""") == "4", "zero capacity"

# A diagonal crossing the cyclic boundary of the numbering structure.
# The graph is a 4-cycle plus diagonal (1,3), all capacities 1.
assert run("""\
4 5
1 2 1
2 3 1
3 4 1
4 1 1
1 3 1
""") == "13", "diagonal and boundary handling"

# Maximum-size graph with all capacities equal to 1.
# It is just a cycle, so every pair has flow 2.
# The answer is n * (n - 1) modulo 998244353.
n = 200000
lines = [f"{n} {n}"]
for i in range(1, n):
    lines.append(f"{i} {i + 1} 1")
lines.append(f"{n} 1 1")

maximum_case = "\n".join(lines) + "\n"
assert run(maximum_case) == "70025880", "maximum-size cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác có ba cạnh đơn vị |`6`| Kích thước tối thiểu và dòng chu kỳ | 
| Tam giác có dung lượng (0,1,2) |`4`| Không có công suất | 
| Bốn chu kỳ cộng với đường chéo (1,3) |`13`| Lồng chéo và cạnh ranh giới tuần hoàn | 
| (n=m=200000) chu kỳ công suất đơn vị |`70025880`| Kích thước tối đa, giá trị bằng nhau và hiệu suất |

 ## Vỏ cạnh 

Hình tam giác có kích thước tối thiểu```
3 3
1 2 1
2 3 1
3 1 1
```bắt đầu bằng ba lá phụ gắn vào gốc. Lá đầu tiên bị xóa và hai lá còn lại nhận được dung lượng của nó, làm cho dung lượng chuyển đổi của chúng bằng (2). Cây thu được có hai cạnh có trọng số (2), do đó ba cặp đỉnh của nó đóng góp (2+2+2=6). Thuật toán không bao giờ giả định rằng đồ thị gốc đã là một cây. 

Trường hợp không có công suất```
3 3
1 2 0
2 3 1
3 1 2
```chọn cạnh có công suất bằng 0 trước tiên. Dung lượng của nó được thêm vào các cạnh khác nên không có gì thay đổi về mặt số lượng. Cây còn lại có dung lượng (1) và (2). Các giá trị thắt cổ chai theo cặp của nó là (1,1,2), cho`4`. Điều này cho thấy tại sao số 0 phải duy trì là giá trị heap hợp lệ và tại sao trọng điểm xóa phải là số âm thay vì sử dụng số 0 có nghĩa là "đã xóa". 

Trường hợp đường chéo```
4 5
1 2 1
2 3 1
3 4 1
4 1 1
1 3 1
```chứa cạnh ranh giới tuần hoàn ((1,4)) cùng với đường chéo ((1,3)). Việc xây dựng đường chéo coi ((1,4)) là vị trí biên (4), chứ không phải là một khoảng thông thường từ (1) đến (4). Cây biến đổi kết quả bảo toàn tất cả sáu luồng theo cặp, có tổng là`13`. Việc xử lý sai cạnh bao quanh đơn này sẽ làm thay đổi cây phụ trợ và có thể âm thầm tạo ra một câu trả lời khác. 

Tam giác công suất tối đa```
3 3
1 2 1000000000
2 3 1000000000
3 1 1000000000
```tạo ra một cây biến đổi có hai cạnh có dung lượng (2000000000). Tổng chưa sửa đổi là (6000000000) và kết quả được yêu cầu là`10533882`. Việc triển khai duy trì dung lượng trong mảng 64 bit đã ký và thực hiện modulo tổng hợp cuối cùng (998244353), do đó, cả dung lượng ban đầu lẫn giá trị được chuyển đổi trung gian đều không bị tràn.
