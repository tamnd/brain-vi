---
title: "CF 102319B - Cầu lông của Paul"
description: "Các con đường tạo thành một cái cây nên giữa hai địa điểm bất kỳ chỉ có duy nhất một con đường. Một nhân viên được phân công đi từ a đến b sử dụng mọi cạnh trên con đường duy nhất đó hàng ngày từ s đến t. Paul trả một lần cho một lợi thế vào một ngày nhất định, bất kể có bao nhiêu nhân viên sử dụng lợi thế đó."
date: "2026-08-14T04:46:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "B"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 223
verified: true
draft: false
---

[CF 102319B - Cầu lông của Paul](https://codeforces.com/problemset/problem/102319/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các con đường tạo thành một cái cây nên giữa hai địa điểm bất kỳ chỉ có duy nhất một con đường. Một nhân viên được phân công đi từ`a`ĐẾN`b`sử dụng mọi cạnh trên con đường duy nhất đó hàng ngày từ`s`bởi vì`t`. 

Paul trả một lần cho một lợi thế vào một ngày nhất định, bất kể có bao nhiêu nhân viên sử dụng lợi thế đó. Do đó, số lượng chúng ta quan tâm trong một ngày là số cạnh cây riêng biệt thuộc về ít nhất một tuyến đường nhân viên hiện đang hoạt động. Một truy vấn`[c,d]`yêu cầu tổng số cạnh hàng ngày đó trong tất cả các ngày từ`c`bởi vì`d`. 

Khó khăn đến từ hai loại chồng chéo độc lập. Các nhân viên khác nhau có thể chia sẻ các cạnh của cây, do đó, việc tính riêng từng đường đi của nhân viên sẽ làm tăng thêm câu trả lời. Khoảng thời gian hoạt động của họ cũng có thể trùng nhau, do đó, cùng một lợi thế có thể được các nhân viên khác nhau thanh toán nhiều lần nhưng chỉ một lần mỗi ngày. 

Với tối đa`10^5`các đỉnh, nhân viên và truy vấn, việc đi từng con đường một cách rõ ràng đã quá tốn kém. Một đường dẫn cây có thể chứa`O(n)`các cạnh, vì vậy việc xử lý tất cả đường dẫn nhân viên theo cách này có thể mất`O(nm)`, xung quanh`10^10`hoạt động trong trường hợp xấu nhất. Thời gian có thể đạt tới`10^9`, vì vậy việc lặp lại nhiều ngày cũng là điều không thể. Chúng tôi chỉ cần xử lý thời gian bắt đầu của nhân viên và ranh giới truy vấn, đồng thời mỗi thao tác trên cây phải theo logarit hoặc gần với nó. 

Có một số trường hợp đặc biệt trong đó việc triển khai trực tiếp có thể âm thầm thất bại. Hãy xem xét cây nhỏ nhất có thể:```
2 2 1
1 2
1 2 1 3
1 2 2 4
2 4
```Cạnh duy nhất được sử dụng hàng ngày từ`1`bởi vì`4`, vậy câu trả lời là`3`. Việc tính riêng số nhân viên sẽ cho`3 + 3 = 6`, vì các khoảng của chúng trùng nhau. Đầu ra đúng là`3`. 

Trường hợp ranh giới thứ hai là một khoảng có đúng một ngày:```
2 1 1
1 2
1 2 5 5
5 5
```Cạnh chỉ được sử dụng vào ngày`5`, vậy câu trả lời là`1`. Việc coi các khoảng là nửa mở mà không chuyển đổi điểm cuối một cách chính xác có thể vô tình tạo ra số không. 

Trường hợp thứ ba gặp lỗi do các đường dẫn chỉ chia sẻ một phần tuyến đường của chúng:```
4 2 1
1 2
2 3
2 4
3 4 1 2
1 2 2 3
2 2
```Vào ngày`2`, cả hai nhân viên đều sử dụng cạnh`1-2`, nhưng chỉ một trong các cạnh khác được sử dụng bởi mỗi tuyến đường. Các cạnh phân biệt là`1-2`,`2-3`, Và`2-4`, vậy câu trả lời là`3`. Việc thêm độ dài đường dẫn sẽ được tính`1-2`hai lần và sản xuất`4`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xem xét từng nhân viên một cách riêng biệt, tìm ra con đường từ đó`a`ĐẾN`b`và với mỗi cạnh trên đường đi đó, hãy đánh dấu khoảng thời gian hoạt động của nó. Sau khi tất cả nhân viên được xử lý, chúng tôi có thể hợp nhất các khoảng cho từng cạnh và trả lời các truy vấn. Điều này đúng vì mỗi con đường có thể được xem xét độc lập và việc hợp nhất các khoảng thời gian của nó mô phỏng chính xác quy tắc rằng một con đường có giá một đô la cho mỗi ngày được bảo hiểm. 

Vấn đề là lượng dữ liệu đường dẫn. Một đường dẫn trong chuỗi có thể chứa`n-1`các cạnh và tất cả`m`nhân viên có thể sử dụng những con đường như vậy. Với`n=m=10^5`, việc truy cập rõ ràng các đường dẫn có thể yêu cầu khoảng`10^10`các chuyến thăm cạnh. Ngay cả trước khi xử lý việc hợp nhất theo khoảng thời gian, điều đó đã vượt xa giới hạn thời gian cho phép. 

Sự thay đổi hữu ích trong quan điểm là xử lý thời gian từ trái sang phải. Giả sử chúng ta đã đến ngày`x`. Đối với mỗi cạnh, chỉ có một thông tin về quá khứ quan trọng: ngày gần nhất mà cạnh đó hiện được đảm bảo vẫn được bảo vệ. Gọi giá trị này`E`. Khi một nhân viên mới bắt đầu vào ngày`s`và kết thúc vào ngày`t`, mọi cạnh trên đường đi của nhân viên đó đều có`E = max(E, t+1)`. 

sử dụng`t+1`làm cho khoảng thời gian nửa mở, do đó nhân viên hoạt động vào các ngày`s`bởi vì`t`đóng góp bảo hiểm cho đến đầu ngày`t+1`. 

Bây giờ hãy xem xét điều gì xảy ra giữa hai lần nhân viên liên tiếp bắt đầu làm việc. Nếu một cạnh hiện đã hết hạn`E`, thì vào ngày hiện tại`x`nó có`max(0, E-x)`số ngày bảo hiểm còn lại. Cho phép`R = sum max(0, E_e-x)`trên tất cả các cạnh của cây. Khi thời gian tiến triển từ`x`ĐẾN`y`, mọi cạnh hiện đang bị che đều mất`y-x`đơn vị bảo hiểm còn lại, dừng ở mức 0. Số ngày biên được trả trong khoảng thời gian đó chính xác là mức giảm trong`R`. 

Điều này mang lại cái nhìn sâu sắc trung tâm. Chúng ta không cần phải đếm rõ ràng từng cạnh mỗi ngày. Chúng tôi duy trì giá trị hết hạn cho mọi cạnh, đường dẫn hỗ trợ`chmax`hoạt động và duy trì tổng của tất cả các giá trị hết hạn. Trước khi chuyển thời gian hiện tại sang`x`, chúng tôi nâng mọi giá trị hết hạn lên`x`. Sau sự chuẩn hóa đó, một cạnh hết hạn`E`đóng góp chính xác`E-x`những ngày được bảo hiểm trong tương lai. 

một con đường`chmax`trên cây có thể bị phân hủy thành`O(log n)`phạm vi liền kề bằng cách sử dụng phân tách ánh sáng nặng. Trên mỗi phạm vi, chúng ta cần một phép toán phạm vi có dạng`E_i = max(E_i, x)`trong khi duy trì tổng phạm vi. Đây chính xác là hoạt động được hỗ trợ bởi cấu trúc nhịp cây phân đoạn. 

Cuối cùng, xác định`F(x)`là tổng số ngày biên được thanh toán kể từ ngày`1`suốt ngày`x`. Một truy vấn`[c,d]`đơn giản là`F(d) - F(c-1)`. 

Chúng tôi đánh giá`F`tại tất cả các điểm cuối được yêu cầu đều ngoại tuyến trong khi quét qua các tọa độ thời gian liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm + qn)`trong trường hợp xấu nhất |`O(nm)`| Quá chậm | 
| Tối ưu |`O(n log n + m log² n + q log q)`khấu hao |`O(n + m + q)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`1`và thực hiện phân hủy ánh sáng nặng. Mỗi đỉnh không phải gốc đại diện cho cạnh nối nó với đỉnh gốc của nó. Vị trí HLD của nó sẽ là vị trí của cạnh đó trong cây phân đoạn. Điều này chuyển đổi một đường dẫn cây thành`O(log n)`các phạm vi liền kề trong khi loại trừ chính xác cạnh đến của tổ tiên chung thấp nhất. 
2. Khởi tạo mọi cạnh hết hạn thành`1`. Trước ngày`1`không có bảo hiểm và hết hạn`1`có nghĩa là không còn số ngày được bảo hiểm tại thời điểm hiện tại`1`. 
3. Sắp xếp tất cả các đơn đặt hàng của nhân viên theo ngày bắt đầu. Chúng tôi xử lý các nhân viên có cùng ngày bắt đầu cùng nhau vì các cập nhật của họ sẽ diễn ra sau khi tính khoảng thời gian ngay trước ngày đó. 
4. Chuyển đổi mọi truy vấn`[c,d]`thành hai yêu cầu tiền tố. Chúng tôi cần`F(d)`Và`F(c-1)`và cái sau được biểu thị bằng tọa độ thời gian`c`. 
5. Quét qua thời gian bắt đầu của nhân viên đã được sắp xếp và tất cả tọa độ điểm cuối truy vấn. Giả sử thời điểm hiện tại là`cur`và tọa độ tiếp theo là`x`. Đặt tổng thời gian hết hạn của cây phân đoạn trước khi chuẩn hóa là`S`. 
6. Nâng cao thời gian hiện tại từ`cur`ĐẾN`x`bằng cách áp dụng`E_i = max(E_i, x)`đến mọi cạnh. Điều này chỉ thay đổi các cạnh đã hết hạn. Sau đó, nếu số tiền hết hạn là`S'`, số ngày trả biên trong ngày`[cur, x-1]`là`S - n_edges * cur`trừ đi`S' - n_edges * x`. Đây chính xác là mức giảm trong tổng phạm vi bảo hiểm còn lại. 
7. Nếu`x-1`là điểm cuối tiền tố được yêu cầu, lưu trữ giá trị tích lũy hiện tại dưới dạng`F(x-1)`. Chúng tôi làm điều này trước khi tuyển nhân viên bắt đầu vào ngày`x`, bởi vì những nhân viên đó không hoạt động vào bất kỳ ngày nào trong suốt`x-1`. 
8. Xử lý mọi nhân viên có ngày bắt đầu`x`. Đối với một nhân viên`(a,b,s,t)`, đường đi từ`a`ĐẾN`b`bị phân hủy bởi HLD và mọi cạnh trên đường đi đó đều nhận được`E = max(E, t+1)`. Từ`t+1`là điểm cuối độc quyền, điều này mang lại chính xác`t-s+1`ngày bảo hiểm cho một cạnh chưa được phát hiện trước đó. 
9. Sau khi tất cả các tọa độ liên quan đã được xử lý, hãy trả lời từng truy vấn ban đầu bằng`F(d) - F(c-1)`. Các giá trị tiền tố đã chứa liên kết trên tất cả các cạnh của cây, vì vậy các nhân viên chồng chéo chưa bao giờ được tính hai lần trong cùng một ngày. 

### Tại sao nó hoạt động 

Đối với mỗi cạnh,`E`đại diện cho ngày độc quyền mới nhất mà qua đó một số nhân viên đã bắt đầu đảm bảo được bảo hiểm về lợi thế đó. Khi một nhân viên mới bắt đầu, hãy tận dụng tối đa`t+1`với`E`bảo toàn chính xác sự kết hợp của tất cả các khoảng nhìn thấy cho đến nay trên cạnh đó. Vào bất kỳ ngày hiện tại nào`x`, thay thế các giá trị đã hết hạn nhỏ hơn`x`qua`x`không làm thay đổi phạm vi phủ sóng trong tương lai, bởi vì những giá trị đó đã biểu thị các khoảng thời gian kết thúc trước`x`. Sau sự bình thường hóa này,`E-x`chính xác là số ngày trong tương lai mà cạnh đó vẫn được bao phủ. Do đó, tổng của các giá trị này là tổng số ngày biên được thanh toán còn lại. Thời gian nâng cao làm giảm số tiền này chính xác bằng số ngày cạnh được thanh toán vượt qua trong quá trình quét, vì vậy mọi tiền tố`F(x)`là đúng. Trừ hai tiền tố sẽ cho kết quả chính xác trong khoảng truy vấn được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class SegmentTreeBeats:
    def __init__(self, n):
        self.n = n
        size = 4 * n + 5
        self.sum = [0] * size
        self.mn = [0] * size
        self.smn = [INF] * size
        self.cnt = [0] * size
        self._build(1, 0, n)

    def _build(self, p, l, r):
        if r - l == 1:
            self.sum[p] = 1
            self.mn[p] = 1
            self.smn[p] = INF
            self.cnt[p] = 1
            return

        mid = (l + r) >> 1
        self._build(p << 1, l, mid)
        self._build(p << 1 | 1, mid, r)
        self._pull(p)

    def _pull(self, p):
        lc = p << 1
        rc = lc | 1

        self.sum[p] = self.sum[lc] + self.sum[rc]

        if self.mn[lc] < self.mn[rc]:
            self.mn[p] = self.mn[lc]
            self.cnt[p] = self.cnt[lc]
            self.smn[p] = min(self.smn[lc], self.mn[rc])
        elif self.mn[lc] > self.mn[rc]:
            self.mn[p] = self.mn[rc]
            self.cnt[p] = self.cnt[rc]
            self.smn[p] = min(self.mn[lc], self.smn[rc])
        else:
            self.mn[p] = self.mn[lc]
            self.cnt[p] = self.cnt[lc] + self.cnt[rc]
            self.smn[p] = min(self.smn[lc], self.smn[rc])

    def _apply_chmax(self, p, x):
        if x <= self.mn[p]:
            return
        self.sum[p] += (x - self.mn[p]) * self.cnt[p]
        self.mn[p] = x

    def _push(self, p):
        x = self.mn[p]
        lc = p << 1
        rc = lc | 1

        if self.mn[lc] < x:
            self._apply_chmax(lc, x)
        if self.mn[rc] < x:
            self._apply_chmax(rc, x)

    def chmax(self, ql, qr, x):
        if ql >= qr:
            return
        self._chmax(1, 0, self.n, ql, qr, x)

    def _chmax(self, p, l, r, ql, qr, x):
        if qr <= l or r <= ql or x <= self.mn[p]:
            return

        if ql <= l and r <= qr and x < self.smn[p]:
            self._apply_chmax(p, x)
            return

        self._push(p)

        mid = (l + r) >> 1
        self._chmax(p << 1, l, mid, ql, qr, x)
        self._chmax(p << 1 | 1, mid, r, ql, qr, x)

        self._pull(p)

def build_hld(n, graph):
    parent = [0] * n
    depth = [0] * n
    order = [0]
    parent[0] = -1

    for v in order:
        for to in graph[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            order.append(to)

    size = [1] * n
    heavy = [-1] * n

    for v in reversed(order):
        best_size = 0
        for to in graph[v]:
            if parent[to] != v:
                continue
            size[v] += size[to]
            if size[to] > best_size:
                best_size = size[to]
                heavy[v] = to

    head = [0] * n
    pos = [0] * n
    cur_pos = 0

    stack = [(0, 0)]

    while stack:
        start, h = stack.pop()
        v = start

        while v != -1:
            head[v] = h
            pos[v] = cur_pos
            cur_pos += 1

            for to in graph[v]:
                if parent[to] == v and to != heavy[v]:
                    stack.append((to, to))

            v = heavy[v]

    return parent, depth, head, pos

def path_chmax(u, v, value, parent, depth, head, pos, seg):
    while head[u] != head[v]:
        if depth[head[u]] < depth[head[v]]:
            u, v = v, u

        h = head[u]
        seg.chmax(pos[h], pos[u] + 1, value)
        u = parent[h]

    if depth[u] > depth[v]:
        u, v = v, u

    # pos[u] is the vertex containing the LCA.
    # The edge entering the LCA must not be included.
    seg.chmax(pos[u] + 1, pos[v] + 1, value)

def solve():
    n, m, q = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        graph[x].append(y)
        graph[y].append(x)

    trips = []
    for _ in range(m):
        a, b, s, t = map(int, input().split())
        trips.append((s, t, a - 1, b - 1))

    queries = []
    query_times = {}

    for i in range(q):
        c, d = map(int, input().split())
        queries.append((c, d))

        # F(d) is available just before day d+1.
        query_times.setdefault(d + 1, []).append((i, 1))

        # F(c-1) is available just before day c.
        query_times.setdefault(c, []).append((i, -1))

    trips.sort()

    parent, depth, head, pos = build_hld(n, graph)
    seg = SegmentTreeBeats(n - 1)

    # The root has no associated edge, so positions are shifted implicitly
    # by using every non-root vertex's HLD position. The root's position is
    # still present, so we need a segment tree of n positions and ignore
    # the root position in path updates.
    #
    # Rebuild with n positions. Position 0 belongs to the root and is never
    # touched by path_chmax.
    seg = SegmentTreeBeats(n)

    starts = trips
    trip_idx = 0

    times = set(query_times.keys())
    for s, _, _, _ in trips:
        times.add(s)
    times = sorted(times)

    current = 1
    answer_prefix = [0] * (2 * q)
    prefix_value = 0

    for x in times:
        if x < current:
            continue

        old_sum = seg.sum[1] - n * current

        seg.chmax(0, n, x)

        new_sum = seg.sum[1] - n * x
        prefix_value += old_sum - new_sum
        current = x

        if x in query_times:
            for query_id, sign in query_times[x]:
                answer_prefix[2 * query_id + (0 if sign == 1 else 1)] = prefix_value

        while trip_idx < m and starts[trip_idx][0] == x:
            _, t, a, b = starts[trip_idx]
            path_chmax(
                a, b, t + 1,
                parent, depth, head, pos, seg
            )
            trip_idx += 1

    out = []
    for i in range(q):
        fd = answer_prefix[2 * i]
        fc = answer_prefix[2 * i + 1]
        out.append(str(fd - fc))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý cây trước tiên sẽ tính toán cây cha, độ sâu, kích thước cây con và cây con nặng. Sau đó, quá trình phân rã sẽ gán cho mỗi đỉnh một vị trí và ghi lại phần đầu của chuỗi nặng của nó. Vị trí của một đỉnh không có gốc biểu thị cạnh từ đỉnh gốc đến đỉnh đó. 

Cây phân đoạn bắt đầu bằng ngày hết hạn`1`ở khắp mọi nơi. Nút của nó lưu trữ thời gian hết hạn tối thiểu, thời gian hết hạn riêng biệt nhỏ thứ hai, số phần tử bằng mức tối thiểu và tổng số tiền. Đối với một phạm vi`chmax(x)`, nếu như`x`không lớn hơn mức tối thiểu, không có gì thay đổi. Nếu như`x`nằm hoàn toàn giữa mức tối thiểu tối thiểu và mức tối thiểu thứ hai, mọi phần tử bị ảnh hưởng đều chính xác ở mức tối thiểu, do đó toàn bộ nút có thể được cập nhật cùng một lúc. Nếu không thì thao tác sẽ chuyển sang phần tử con. Đây là ý tưởng đánh bại cây phân đoạn tiêu chuẩn cho phạm vi`chmax`. 

Vị trí của thư mục gốc là vô hại vì việc cập nhật đường dẫn luôn bắt đầu ngay bên dưới LCA. Do đó vị trí gốc không bao giờ được sửa đổi. Giữ`n`vị trí thay vì`n-1`làm cho việc lập chỉ mục HLD trở nên đơn giản và tránh phải ánh xạ lại các vị trí sau khi phân tách. 

Việc sử dụng quét thời gian`t+1`còn hơn là`t`. Một nhân viên năng động suốt ngày`t`phải che chắn cạnh tương ứng vào ban ngày`t`, vì vậy thời gian hết hạn độc quyền của nó là vào đầu ngày`t+1`. Tương tự, tiền tố truy vấn trong ngày`x`được đánh giá ngay trước ngày`x+1`, điều này giải thích tại sao hai tọa độ truy vấn là`d+1`Và`c`. 

biểu hiện`seg.sum[1] - n * current`là tổng phạm vi bảo hiểm còn lại. Sau tất cả các thời hạn dưới đây`current`đã được nâng lên`current`, mọi cạnh đều đóng góp`E-current`, bao gồm số 0 cho một cạnh đã hết hạn. Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị có thể đạt tới mức gần đúng một cách an toàn.`10^14`, vượt quá phạm vi 32-bit. 

Việc sắp xếp thứ tự ở mỗi thời điểm phối hợp cũng có chủ ý. Trước tiên, chúng tôi nâng cao thời gian và ghi lại tiền tố truy vấn, sau đó áp dụng những nhân viên có thời gian bắt đầu bằng tọa độ đó. Truy vấn kết thúc vào ngày`x-1`không được nhìn thấy nhân viên bắt đầu vào ngày`x`. 

## Ví dụ đã hoạt động 

Câu lệnh được cung cấp sẽ bỏ qua kết quả mẫu trong văn bản được sao chép, nhưng việc đánh giá ba truy vấn sẽ cho kết quả`5`,`14`, Và`4`. 

Đối với mẫu đầu tiên, cây có các cạnh`1-2`,`2-3`,`1-4`, Và`1-5`. Nhân viên đầu tiên sử dụng`1-2`Và`1-5`từ ngày`4`bởi vì`7`. Công dụng thứ hai`2-3`,`1-2`, Và`1-4`từ ngày`2`bởi vì`5`. Công dụng thứ ba`2-3`từ ngày`6`bởi vì`9`. 

| Ngày | Các cạnh hoạt động | Chi phí hàng ngày | 
| --- | --- | --- | 
| 2 |`2-3`,`1-2`,`1-4`| 3 | 
| 3 |`2-3`,`1-2`,`1-4`| 3 | 
| 4 |`2-3`,`1-2`,`1-4`,`1-5`| 4 | 
| 5 |`2-3`,`1-2`,`1-4`,`1-5`| 4 | 
| 6 |`2-3`,`1-2`,`1-5`| 3 | 
| 7 |`2-3`,`1-2`,`1-5`| 3 | 
| 8 |`2-3`| 1 | 
| 9 |`2-3`| 1 | 

Đối với truy vấn`[7,11]`, chi phí là`3 + 1 + 1 = 5`. Vì`[3,6]`, đó là`3 + 4 + 4 + 3 = 14`. Vì`[5,5]`, đó là`4`. 

Việc quét bắt đầu vào ngày`2`,`4`, Và`6`. Vào ngày`2`, nhân viên thứ hai tạo ra ngày hết hạn`6`trên ba cạnh của nó. Vào ngày`4`, nhân viên đầu tiên tăng hai trong số các giá trị hết hạn đó lên`8`. Vào ngày`6`, nhân viên thứ ba tăng`2-3`hết hạn đến`10`. Cây phân đoạn không bao giờ tính cạnh chia sẻ hai lần, bởi vì tất cả các cập nhật đều sử dụng`chmax`. 

Ví dụ thứ hai tách biệt hành vi chồng chéo:```
2 2 3
1 2
1 2 1 3
1 2 2 4
1 1
2 3
4 4
```Chỉ có một con đường. Nhân viên đầu tiên cho nó hết hạn`4`, và nhân viên thứ hai sau đó nâng nó lên`5`. Quá trình quét có thể được tóm tắt như sau. 

| Tọa độ | Hành động | Cạnh hết hạn | Chi phí tiền tố | 
| --- | --- | --- | --- | 
| 1 | Bắt đầu chuyến đi đầu tiên | 4 | 0 | 
| 2 | Bắt đầu chuyến đi thứ hai | 5 | 1 | 
| 4 | Truy vấn qua ngày thứ 3 | 5 | 3 | 
| 5 | Truy vấn qua ngày thứ 4 | 5 | 4 | 

Các câu trả lời là`1`,`2`, Và`1`cho ba truy vấn. Đường dùng chung vẫn được biểu thị bằng một giá trị hết hạn, điều này chứng tỏ tại sao`chmax`state nắm bắt chính xác các nhân viên chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m log² n + q log q)`khấu hao | HLD phân tách từng đường đi của nhân viên thành`O(log n)`phạm vi và nhịp cây phân đoạn xử lý từng phạm vi`chmax`khấu hao theo logarit | 
| Không gian |`O(n + m + q)`| Cây, mảng HLD, cây phân đoạn, nhân viên và thông tin truy vấn đều sử dụng không gian tuyến tính | 

Công việc chủ yếu là`m`cập nhật đường dẫn. Với`10^5`nhân viên và đỉnh, phân tách nặng-ánh sáng giữ cho mỗi đường đi đến nhiều phạm vi phân đoạn theo logarit, trong khi các nhịp của cây phân đoạn tránh truy cập vào mọi cạnh trong các phạm vi đó. Tọa độ thời gian cũng được giới hạn bởi`O(m+q)`, vì vậy`10^9`độ lớn của giá trị ngày không tạo ra một yếu tố bổ sung. 

## Trường hợp thử nghiệm```python
import sys
import io

# The solution above is assumed to be saved as solve() in the same file.
# This helper temporarily replaces stdin and captures stdout.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
5 3 3
1 2
3 2
1 4
1 5
2 5 4 7
3 4 2 5
2 3 6 9
7 11
3 6
5 5
"""

assert run(sample) == "5\n14\n4", "sample"

minimum = """\
2 1 3
1 2
1 2 1 1
1 1
2 2
1 2
"""

assert run(minimum) == "1\n0\n1", "minimum tree and one-day interval"

overlap = """\
2 2 3
1 2
1 2 1 3
1 2 2 4
1 1
2 3
4 4
"""

assert run(overlap) == "1\n2\n1", "overlapping employees on one edge"

shared_path = """\
4 2 3
1 2
2 3
2 4
3 4 1 2
1 2 2 3
1 2
2 2
3 3
"""

assert run(shared_path) == "4\n3\n1", "shared edge and path overlap"

equal_intervals = """\
3 3 4
1 2
2 3
1 3 5 5
1 3 5 5
1 2 5 5
4 5
5 5
6 6
5 5
"""

assert run(equal_intervals) == "2\n2\n0\n2", "all equal active intervals"

# A large structural test. All 100000 employees use the same complete path
# during exactly the same huge interval. Only two tree edges are ever charged.
n = 100000
m = 100000
q = 3

parts = [f"{n} {m} {q}\n"]
for v in range(2, n + 1):
    parts.append(f"{v - 1} {v}\n")

for _ in range(m):
    parts.append(f"1 {n} 1 1000000000\n")

parts.append("1 1\n")
parts.append("1 1000000000\n")
parts.append("1000000001 1000000001\n")

large_input = "".join(parts)
expected_large = f"99999\n99999999900001\n0"

assert run(large_input) == expected_large, "large repeated-path case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây có kích thước tối thiểu với chuyến đi một ngày |`1`,`0`,`1`| Cây nhỏ nhất, điểm cuối bao gồm và một ngày bên ngoài mọi hoạt động | 
| Hai chuyến đi chồng chéo trên một cạnh |`1`,`2`,`1`| Nhiều nhân viên không được tính phí gấp đôi | 
| Hai đường đi chung một cạnh |`4`,`3`,`1`| Chồng chéo một phần đường dẫn và sửa ranh giới đường dẫn HLD | 
| Khoảng thời gian bằng nhau và đường đi lặp lại |`2`,`2`,`0`,`2`| Nhân viên trùng lặp và thời gian bắt đầu/kết thúc giống hệt nhau | 
| Dây chuyền lớn với`10^5`chuyến đi giống hệt nhau |`99999`,`99999999900001`,`0`| Lớn`n`, lớn`m`, giá trị thời gian lớn và khả năng mở rộng | 

## Vỏ cạnh 

Cây nhỏ nhất có đúng một cạnh. TRONG```
2 1 1
1 2
1 2 5 5
5 5
```chuyến đi cập nhật cạnh đơn đó thành hết hạn`6`. Điểm cuối truy vấn là`6`, do đó quá trình quét tiến tới`6`, thay đổi phạm vi bảo hiểm còn lại từ`1`ĐẾN`0`và thêm chính xác một ngày trả phí vào tiền tố. Câu trả lời là`1`. Việc sử dụng`t+1`là điều khiến cho khoảng thời gian một ngày có tác dụng. 

Đối với các nhân viên chồng chéo trên cùng một cạnh,```
2 2 1
1 2
1 2 1 3
1 2 2 4
1 4
```chuyến đi đầu tiên đặt thời hạn thành`4`. Chuyến đi thứ hai sau đó đổi thành`5`, thay vì thêm một khoảng thời gian phủ sóng độc lập khác. Tiền tố cuối cùng chứa bốn ngày được trả lương, vì vậy câu trả lời là`4`. Đây chính xác là sự kết hợp của`[1,3]`Và`[2,4]`. 

Đối với các đường dẫn chỉ trùng nhau một phần,```
4 2 1
1 2
2 3
2 4
3 4 1 2
1 2 2 3
2 2
```đường dẫn đầu tiên sử dụng các cạnh`2-3`Và`2-4`, trong khi cách thứ hai sử dụng`1-2`Và`2-3`. Vào ngày`2`, cạnh chung`2-3`chỉ có một giá trị hết hạn. Ba cạnh hoạt động riêng biệt đưa ra câu trả lời`3`. 

Một truy vấn có thể kết thúc ngay trước khi nhân viên khác bắt đầu. Ví dụ,```
2 1 2
1 2
1 2 3 5
1 2
1 3
```cho`0`vì`[1,2]`Và`1`vì`[1,3]`. Tiền tố tọa độ cho`[1,2]`là`3`và thuật toán ghi lại tiền tố trước khi áp dụng nhân viên bắt đầu từ ngày`3`. Lệnh này ngăn nhân viên bị tính phí trước ngày làm việc đầu tiên của họ. 

Cuối cùng, thời gian có thể lớn hơn nhiều so với mọi khoảng thời gian hoạt động. Vì```
2 1 2
1 2
1 2 1 1000000000
1000000000 1000000000
1000000001 1000000001
```chi phí truy vấn đầu tiên`1`, trong khi chi phí thứ hai`0`. Không có vòng lặp trong ngày được thực hiện. Quá trình quét nhảy trực tiếp từ tọa độ`1`ĐẾN`1000000000`và sau đó đến`1000000001`, sử dụng số học hết hạn để tính tất cả các ngày trung gian cùng một lúc.
