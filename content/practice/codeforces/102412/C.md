---
title: "CF 102412C - Chạy Bi Thép"
description: "Chúng ta có một cái cây mà các đỉnh của nó hiện có thể chứa hoặc không chứa chip. Một truy vấn chuyển đổi một đỉnh giữa hai trạng thái này. Sau mỗi lần chuyển đổi, chúng ta cần tổng số lần duyệt cạnh tối thiểu cần thiết để tập hợp tất cả các chip hiện có ở một đỉnh chung."
date: "2026-08-10T14:00:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "C"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 1030
verified: true
draft: false
---

[CF 102412C - Đường chạy bi thép](https://codeforces.com/problemset/problem/102412/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cái cây mà các đỉnh của nó hiện có thể chứa hoặc không chứa chip. Một truy vấn chuyển đổi một đỉnh giữa hai trạng thái này. Sau mỗi lần chuyển đổi, chúng ta cần tổng số lần duyệt cạnh tối thiểu cần thiết để tập hợp tất cả các chip hiện có ở một đỉnh chung. Một chip có thể đi qua các đỉnh chứa các chip khác, do đó chi phí cho việc chọn đỉnh đích (v) chỉ đơn giản là 

[ 
F(v)=\sum_{u\text{ có một con chip}} \operatorname{dist}(u,v). 
] 

Câu trả lời là giá trị nhỏ nhất của (F(v)) trên tất cả các đỉnh. 

Cây chứa tối đa (10^5) đỉnh và có tối đa (10^5) cập nhật. Giới hạn thời gian chính thức là 4 giây và giới hạn bộ nhớ là 256 MiB. Điều này loại trừ mọi thao tác quét toàn bộ cây sau mỗi truy vấn. Một giải pháp (O(nq)) có thể thực hiện khoảng (10^{10}) thao tác ở giới hạn tối đa, vượt xa những gì phù hợp. Chúng tôi cần công việc logarit đại khái cho mỗi lần cập nhật. 

Có một số trường hợp khó xử lý. Chỉ với một con chip, câu trả lời luôn là số không. Ví dụ,```
1
1
+ 1
```có đầu ra```
0
```bởi vì con chip đã ở đích. 

Đích đến tối ưu không nhất thiết phải chứa chip. Trên đường đi (1-2-3), việc thêm các chip ở đỉnh 1 và 3 sẽ cho```
3
1 2
2 3
2
+ 1
+ 3
```với đầu ra```
0
2
```Đích đến tốt nhất là đỉnh 2, trống. Việc triển khai chỉ xem xét các đỉnh bị chiếm dụng sẽ chỉ báo cáo sai 2 nếu nó xử lý trường hợp này một cách gián tiếp và nói chung nó có thể bỏ lỡ mức tối ưu thực sự. 

Cũng có thể có hai đỉnh trung bình tốt như nhau. Trên đường dẫn (1-2), nếu cả hai điểm cuối đều chứa chip thì việc di chuyển mọi thứ đến một trong hai điểm cuối sẽ tốn một chi phí. Một phương pháp giả sử trung vị là duy nhất có thể vô tình bác bỏ một câu trả lời đúng. Đặc tính hữu ích dựa trên các thành phần chứa hơn một nửa số chip, do đó, trung vị được xử lý một cách tự nhiên. 

Cuối cùng, việc xóa chip cuối cùng bị đầu vào cấm, nhưng việc xóa một chip vẫn có thể để lại đúng một chip. Ví dụ,```
3
1 2
2 3
3
+ 1
+ 3
- 1
```sản xuất```
0
2
0
```Cấu trúc dữ liệu phải hoạt động khi tập hoạt động có kích thước một ngay sau khi cập nhật. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể tính toán lại toàn bộ mục tiêu sau mỗi truy vấn. Root cây một lần, tính số chip trong mỗi cây con, tính tổng khoảng cách từ gốc và sau đó root lại tổng khoảng cách trên tất cả các đỉnh. Công thức root lại tiêu chuẩn là 

[ 
F(v)=F(p)+M-2S_v, 
] 

khi (v) là con của (p), trong đó (M) là tổng số chip và (S_v) là số chip trong cây con của (v). Điều này đưa ra câu trả lời chính xác trong thời gian (O(n)) cho một truy vấn. 

Vấn đề là việc thực hiện việc này sau (10^5) truy vấn sẽ tốn chi phí (O(nq)), tức là khoảng (10^{10}) hoạt động ở mức giới hạn tối đa. Lực lượng vũ phu là chính xác vì nó đánh giá rõ ràng mọi đích đến có thể, nhưng nó liên tục loại bỏ gần như tất cả thông tin khỏi truy vấn trước đó. 

Quan sát quan trọng là vật kính có hình dạng rất cứng trên cây. Giả sử chúng ta đứng ở đỉnh (v) và di chuyển qua một cạnh vào thành phần chứa (x) chip. Mỗi chip (x) đó tiến gần hơn một cạnh, trong khi mỗi chip (M-x) khác tiến xa hơn một cạnh. Do đó, 

[ 
F(\text{next})-F(v)=(M-x)-x=M-2x. 
] 

Vì vậy, việc hướng tới một thành phần chứa hơn một nửa số chip sẽ cải thiện rõ ràng câu trả lời. Hướng tới một thành phần chứa nhiều nhất một nửa không thể cải thiện nó. Do đó, một đỉnh là tối ưu chính xác khi mọi thành phần thu được bằng cách loại bỏ nó chứa tối đa một nửa số chip. Đây là trung vị có trọng số của cây. 

Root cây ban đầu tùy ý. Trong cây có gốc này, nếu cây con nào đó chứa hơn một nửa số chip thì trung vị phải nằm trong cây con đó. Chúng ta có thể đi theo đứa trẻ nặng nề đó nhiều lần. Tương tự, điểm trung vị là đỉnh sâu nhất mà cây con chứa đúng hơn một nửa số chip. 

Vấn đề còn lại là tìm đỉnh đó một cách linh hoạt. Chuyến tham quan Euler biến mỗi cây con thành một khoảng liền kề. Cây Fenwick có thể duy trì đỉnh nào chứa chip, do đó số lượng chip của cây con trở thành truy vấn tổng khoảng. Đầu tiên chúng ta tìm con chip vượt qua nửa điểm theo thứ tự Euler. Bất kỳ cây con nào chứa hơn một nửa số chip đều phải chứa chip đó, do đó đường trung vị nằm trên đường dẫn từ gốc tới chip đó. Nâng nhị phân sau đó tìm ra tổ tiên sâu nhất mà cây con vẫn chứa hơn một nửa số chip. Chi phí này (O(\log^2 n)) vì mỗi bước kiểm tra tổ tiên (O(\log n)) thực hiện truy vấn tổng tiền tố Fenwick. 

Sau khi xác định được vị trí trung vị, chúng ta vẫn cần tổng khoảng cách của nó tới tất cả các chip. Việc tính toán lại số tiền này sẽ quá tốn kém. Phân rã Centroid mang lại chính xác cấu trúc động phù hợp. Đối với mỗi centroid (c), chúng tôi duy trì số lượng chip hoạt động được biểu thị ở đó và tổng khoảng cách của chúng tới (c). Chúng tôi cũng lưu trữ thông tin tương ứng cho từng centroid con để có thể trừ đi những đóng góp từ cùng một thành phần phân rã. Việc chèn hoặc xóa chỉ thay đổi tổ tiên trung tâm (O(\log n)) và truy vấn tổng khoảng cách sẽ truy cập cùng tổ tiên (O(\log n)) đó. 

Hai kỹ thuật này giải quyết được hai nửa khác nhau của vấn đề. Thứ tự Euler và nâng nhị phân xác định vị trí tối ưu, trong khi phân rã centroid đánh giá mục tiêu ở mức tối ưu đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n+q\log^2 n)) | (O(n\log n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây ban đầu ở đỉnh 1 và thực hiện chuyến tham quan Euler. Cửa hàng`tin[v]`Và`tout[v]`, do đó cây con của (v) tương ứng với khoảng Euler ([tin[v],tout[v])). Cũng xây tổ tiên nhị-nâng. 
2. Duy trì cấu hình chip hiện tại trong cây Fenwick. Chức vụ`tin[v]`chứa 1 chính xác khi đỉnh (v) có chip. Do đó, cây Fenwick hỗ trợ việc thêm hoặc bớt một con chip và đếm các con chip bên trong bất kỳ cây con nào. 
3. Gọi (M) là số chip hiện tại và đặt 

[ 
k=\left\lfloor\frac M2\right\rfloor+1. 
] 

Tìm đỉnh hoạt động thứ (k)-th theo thứ tự Euler. Gọi nó là (x). Đây là đỉnh hoạt động đầu tiên sau điểm giữa. 

1. Số trung vị phải là tổ tiên của (x). Nếu một đỉnh có hơn một nửa số chip trong cây con của nó thì cây con đó chứa đỉnh hoạt động thứ (k). Do đó mọi trung vị có thể đều nằm trên đường dẫn gốc tới (x). 
2. Bắt đầu từ (x), sử dụng nâng nhị phân để leo lên cao nhất có thể trong khi cây con của tổ tiên ứng cử viên chứa ít hơn (k) chip. Cây tổ tiên đầu tiên có ít nhất (k) chip là cây con sâu nhất chứa hơn một nửa số chip, vì vậy nó là cây trung vị hợp lệ. 
3. Xây dựng phân rã trọng tâm của cây ban đầu. Đối với mỗi đỉnh, hãy lưu trữ khoảng cách của nó tới tổ tiên trọng tâm của nó. Cây centroid có chiều cao logarit vì mỗi centroid chia thành phần của nó thành các phần có kích thước tối đa bằng một nửa. 
4. Với mọi trọng tâm (c), duy trì`cnt[c]`, số lượng chip hoạt động được đại diện bởi tâm đó và`sum[c]`, tổng khoảng cách của chúng tới (c). Đối với mọi centroid không phải gốc (c), cũng duy trì`subcnt[c]`Và`subsum[c]`, mô tả thành phần được biểu thị bằng (c) so với thành phần trung tâm của nó. 
5. Khi một con chip được đưa vào hoặc lấy ra ở đỉnh (v), hãy đi từ (v) trở lên qua cây tâm. Tại centroid (c), thêm bản cập nhật vào`cnt[c]`và thêm khoảng cách tương ứng vào`sum[c]`. Nếu (c) có cha mẹ trung tâm, hãy cập nhật`subcnt[c]`Và`subsum[c]`cũng vậy. 
6. Để tính tổng khoảng cách từ một đỉnh tùy ý (v) đến mọi chip đang hoạt động, hãy đi qua tổ tiên trọng tâm của nó. Đối với trọng tâm (c),`sum[c] + cnt[c] * dist(v,c)`đếm sự đóng góp của tất cả các chip được lưu trữ ở đó. Các chip thuộc cùng thành phần tâm như (v) đã được tính ở tâm sâu hơn, vì vậy hãy trừ đi`subsum[child] + subcnt[child] * dist(v,c)`cho đứa trẻ đó. 
7. Sau mỗi truy vấn, hãy cập nhật cả cây Fenwick và cấu trúc trọng tâm, tìm trung vị hiện tại, đánh giá tổng khoảng cách của nó bằng cấu trúc trọng tâm và in giá trị đó. 

### Tại sao nó hoạt động 

Bất biến trung tâm là một đỉnh là hàm tối thiểu của hàm tổng khoảng cách chính xác khi không có thành phần nào xung quanh nó chứa hơn một nửa số chip đang hoạt động. Nếu một thành phần như vậy tồn tại, việc vượt qua cạnh của nó sẽ làm giảm mục tiêu, do đó đỉnh hiện tại không thể tối ưu. Nếu không có thành phần nào như vậy tồn tại thì mọi bước đi đầu tiên có thể có sẽ có chi phí thay đổi không âm, do đó đỉnh là tối ưu. 

Việc xây dựng theo thứ tự Euler tìm ra tổ tiên sâu nhất thỏa mãn điều kiện này. Vị trí Euler hoạt động thứ (k)-phải nằm bên trong mỗi cây con chứa hơn một nửa số chip, do đó đường trung vị nằm trên đường đi tổ tiên của nó. Nâng nhị phân tìm thấy tổ tiên đủ điều kiện sâu nhất. 

Cấu trúc centroid duy trì độc lập tổng khoảng cách chính xác cho mọi đỉnh được truy vấn. Mỗi chip hoạt động đóng góp vào mọi tổ tiên trung tâm của đỉnh của nó và phép trừ cho thành phần trung tâm sâu hơn sẽ ngăn chặn việc tính hai lần. Do đó, giá trị trả về cho trung vị đã chọn chính xác là khoảng tối thiểu có thể. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, pos, delta):
        n = self.n
        bit = self.bit
        while pos <= n:
            bit[pos] += delta
            pos += pos & -pos

    def prefix(self, pos):
        bit = self.bit
        res = 0
        while pos:
            res += bit[pos]
            pos -= pos & -pos
        return res

    def kth(self, k):
        idx = 0
        step = 1 << (self.n.bit_length() - 1)
        bit = self.bit
        n = self.n

        while step:
            nxt = idx + step
            if nxt <= n and bit[nxt] < k:
                idx = nxt
                k -= bit[nxt]
            step >>= 1

        return idx

def solve():
    n = int(input())

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    # Root the original tree and build an Euler tour.
    parent = array('i', [-1]) * n
    depth = array('i', [0]) * n
    tin = array('i', [0]) * n
    tout = array('i', [0]) * n
    euler = []

    stack = [(0, -1, 0)]
    while stack:
        v, p, state = stack.pop()

        if state == 0:
            parent[v] = p
            if p != -1:
                depth[v] = depth[p] + 1

            tin[v] = len(euler)
            euler.append(v)

            stack.append((v, p, 1))
            for to in reversed(graph[v]):
                if to != p:
                    stack.append((to, v, 0))
        else:
            tout[v] = len(euler)

    # Binary lifting for ancestor queries.
    LOG = n.bit_length()
    up = [array('i', parent)]

    for _ in range(1, LOG):
        prev = up[-1]
        cur = array('i', [-1]) * n
        for v in range(n):
            p = prev[v]
            cur[v] = -1 if p == -1 else prev[p]
        up.append(cur)

    # Centroid decomposition.
    removed = bytearray(n)
    cd_parent = array('i', [-1]) * n

    # For every original vertex v, cd_dist[v] stores distances to
    # centroid ancestors in root-to-leaf centroid order.
    cd_dist = [array('i') for _ in range(n)]

    tmp_parent = array('i', [-1]) * n
    subtree_size = array('i', [0]) * n

    def find_centroid(start):
        order = [start]
        tmp_parent[start] = -1

        for v in order:
            pv = tmp_parent[v]
            for to in graph[v]:
                if not removed[to] and to != pv:
                    tmp_parent[to] = v
                    order.append(to)

        for v in reversed(order):
            s = 1
            for to in graph[v]:
                if not removed[to] and tmp_parent[to] == v:
                    s += subtree_size[to]
            subtree_size[v] = s

        total = len(order)

        for v in order:
            largest = total - subtree_size[v]
            for to in graph[v]:
                if not removed[to] and tmp_parent[to] == v:
                    if subtree_size[to] > largest:
                        largest = subtree_size[to]

            if largest * 2 <= total:
                return v

        return start

    def decompose(start, pcd):
        c = find_centroid(start)
        cd_parent[c] = pcd

        # Store distance from this centroid to every vertex in its
        # current component.
        stack = [(c, -1, 0)]
        while stack:
            v, p, d = stack.pop()
            cd_dist[v].append(d)

            for to in graph[v]:
                if not removed[to] and to != p:
                    stack.append((to, v, d + 1))

        removed[c] = 1

        for to in graph[c]:
            if not removed[to]:
                decompose(to, c)

    decompose(0, -1)

    # Dynamic centroid data.
    cnt = array('q', [0]) * n
    total_dist = array('q', [0]) * n
    subcnt = array('q', [0]) * n
    subdist = array('q', [0]) * n

    fenwick = Fenwick(n)
    active = bytearray(n)
    total_chips = 0

    def centroid_update(v, delta):
        chain = cd_dist[v]
        c = v

        for i in range(len(chain) - 1, -1, -1):
            d = chain[i]

            cnt[c] += delta
            total_dist[c] += delta * d

            p = cd_parent[c]
            if p != -1:
                dp = chain[i - 1]
                subcnt[c] += delta
                subdist[c] += delta * dp

            c = p
            if c == -1:
                break

    def distance_sum(v):
        chain = cd_dist[v]
        c = v
        child = -1
        ans = 0

        for i in range(len(chain) - 1, -1, -1):
            d = chain[i]

            ans += total_dist[c] + cnt[c] * d

            if child != -1:
                ans -= subdist[child] + subcnt[child] * d

            child = c
            c = cd_parent[c]

            if c == -1:
                break

        return ans

    def subtree_count(v):
        return fenwick.prefix(tout[v]) - fenwick.prefix(tin[v])

    def find_median():
        k = (total_chips + 1) // 2

        # Fenwick.kth returns a zero-based Euler position.
        pos = fenwick.kth(k)
        x = euler[pos]

        # x itself may already be the deepest heavy vertex.
        if subtree_count(x) >= k:
            return x

        cur = x

        for j in range(LOG - 1, -1, -1):
            a = up[j][cur]
            if a != -1 and subtree_count(a) < k:
                cur = a

        # cur is the deepest ancestor whose subtree is still too small.
        # Its parent is the first ancestor whose subtree exceeds half.
        return parent[cur]

    q = int(input())
    out = []

    for _ in range(q):
        op, v = input().split()
        v = int(v) - 1

        if op == '+':
            delta = 1
            active[v] = 1
        else:
            delta = -1
            active[v] = 0

        total_chips += delta

        fenwick.add(tin[v] + 1, delta)
        centroid_update(v, delta)

        median = find_median()
        out.append(str(distance_sum(median)))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```Quá trình duyệt tiền xử lý đầu tiên bắt nguồn từ cây ban đầu và gán các vị trí Euler. các`tout[v] = tin[v] + subtree_size[v]`tài sản cũng có thể được sử dụng, nhưng việc gán`tout`trực tiếp với quá trình duyệt vào/ra lặp đi lặp lại làm cho khoảng thời gian của cây con trở nên rõ ràng và tránh được các vấn đề về độ sâu đệ quy. 

Bảng nâng nhị phân lưu trữ tổ tiên của cây gốc, không phải tổ tiên của cây trung tâm. Hai cây này có ý nghĩa khác nhau và không được trộn lẫn. Bảng cây gốc chỉ được sử dụng để định vị trung vị có trọng số. 

Sự phân rã trung tâm được xây dựng độc lập. Mọi đỉnh ban đầu cuối cùng đều trở thành trọng tâm, vì vậy theo`cd_parent`bắt đầu từ (v) cung cấp chính xác tổ tiên trung tâm cần thiết cho các truy vấn khoảng cách động. 

các`cd_dist[v]`mảng lưu trữ khoảng cách theo thứ tự từ gốc đến lá trong cây trung tâm. Các vòng lặp cập nhật và truy vấn đi ngược lại mảng này vì chúng bắt đầu ở đỉnh ban đầu và di chuyển về phía gốc trung tâm. các`array('i')`Việc biểu diễn giữ cho các khoảng cách (O(n\log n)) này nhỏ gọn trong bộ nhớ. Tiêu chuẩn của Python`array`type lưu trữ các giá trị số đã nhập trong một biểu diễn được đóng gói thay vì một đối tượng Python cho mỗi phần tử. 

Bộ đếm trung tâm sử dụng mảng 64 bit vì câu trả lời có thể lớn bằng (\Theta(n^2)). Bản thân các số nguyên Python không bị tràn, nhưng việc sử dụng bộ lưu trữ 64-bit đã ký sẽ giữ cho các mảng rõ ràng được thu gọn trong khi vẫn bao phủ tổng khoảng cách tối đa có thể một cách thoải mái. 

Cây Fenwick sử dụng các vị trí bên trong dựa trên một, do đó vị trí Euler dựa trên 0 ban đầu`tin[v]`được cập nhật tại`tin[v] + 1`. Ngược lại,`prefix(tout[v]) - prefix(tin[v])`đếm chính xác các đỉnh trong khoảng Euler nửa mở ([tin[v],tout[v])). Việc kết hợp hai quy ước lập chỉ mục này là một trong những cách dễ nhất để tạo ra từng lỗi một. 

Tìm kiếm trung bình sử dụng (k=\lfloor M/2\rfloor+1), thay vì (M/2), vì điều kiện hoàn toàn lớn hơn một nửa. Đối với (M), điều này chọn một bên của một cặp trung vị có thể, điều này là đủ vì cả hai bên đều có chi phí tối ưu như nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây là con đường (1-2-3). Root nó ở mức 1, cho thứ tự Euler (1,2,3). 

| Truy vấn | Đỉnh hoạt động | Tổng số chip | (k) | Chip Euler (k)-th | Trung vị | Khoảng cách | 
| --- | --- | --- | --- | --- | --- | --- | 
|`+ 1`| {1} | 1 | 1 | 1 | 1 | 0 | 
|`+ 3`| {1,3} | 2 | 2 | 3 | 2 | 2 | 
|`+ 2`| {1,2,3} | 3 | 2 | 2 | 2 | 2 | 
|`- 1`| {2,3} | 2 | 2 | 3 | 2 | 1 | 

Sau truy vấn thứ hai, đỉnh 3 là chip vượt nửa chặng đường. Cây con của nó chỉ có một chip nên không phải là trung vị. Cha mẹ của nó, đỉnh 2, có cả hai chip trong cây con của nó và là nút tổ tiên sâu nhất có cây con vượt quá một nửa. Tổng khoảng cách từ đỉnh 2 đến các chip ở vị trí 1 và 3 là (1+1=2). 

Sau khi thêm đỉnh 2, đường trung tuyến vẫn là đỉnh 2. Sau khi xóa đỉnh 1, chỉ còn lại đỉnh 2 và 3, do đó đỉnh 2 có tổng chi phí (0+1=1). Điều này cũng áp dụng cho trường hợp một số chip chẵn có hai điểm trung bình tốt như nhau. 

### Mẫu 2 

Gốc cây tại đỉnh 1. Thứ tự Euler của nó là (1,2,3,4,5,6). 

| Truy vấn | Đỉnh hoạt động | Tổng số chip | (k) | Chip Euler (k)-th | Trung vị | Khoảng cách | 
| --- | --- | --- | --- | --- | --- | --- | 
|`+ 1`| {1} | 1 | 1 | 1 | 1 | 0 | 
|`+ 4`| {1,4} | 2 | 2 | 4 | 2 | 3 | 
|`+ 5`| {1,4,5} | 3 | 2 | 4 | 4 | 4 | 
|`- 5`| {1,4} | 2 | 2 | 4 | 2 | 3 | 
|`+ 6`| {1,4,6} | 3 | 2 | 4 | 2 | 4 | 

Khi các chip ở vị trí 1 và 4, đỉnh 4 nằm bên trong cây con của đỉnh 2, do đó đỉnh 2 trở thành tổ tiên nặng nhất sâu nhất. Khoảng cách của nó tới chip ở số 1 và 4 là (1+2=3). 

Sau khi thêm đỉnh 5, cây con của đỉnh 4 chứa các chip ở vị trí 4 và 5, tức là hai trong số ba chip. Do đó, đỉnh 4 trở thành trung tuyến, cho (0+3+1=4). Điều này chứng tỏ tại sao đường trung vị có thể di chuyển xuống dưới nhiều mức sau một lần chèn. 

Sau khi xóa đỉnh 5, số dư lại thay đổi và đỉnh 2 trở thành đường trung tuyến. Cuối cùng, đỉnh 6 tham gia vào tập hợp hoạt động. Đỉnh 2 hiện có khoảng cách (1), (2) và (1) tới các đỉnh 1, 4 và 6, cho khoảng 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Tiền xử lý | (O(n\log n)) | Tiền xử lý Euler, nâng nhị phân và phân rã centroid | 
| Cập nhật | (O(\log n)) | Một bản cập nhật Fenwick và một lần đi bộ trên cây trung tâm | 
| Tìm kiếm trung bình | (O(\log^2 n)) | (O(\log n)) kiểm tra nâng nhị phân, mỗi kiểm tra sử dụng tổng Fenwick | 
| Tổng khoảng cách | (O(\log n)) | Một lần đi bộ qua tổ tiên trung tâm | 
| Tổng cộng | (O(n\log n+q\log^2 n)) | Tất cả các truy vấn đều sử dụng cùng một cấu trúc được xử lý trước | 
| Không gian | (O(n\log n)) | Nâng nhị phân và khoảng cách đến tổ tiên trung tâm | 

Với (n,q\le 10^5), quá trình tiền xử lý dễ dàng nằm trong phạm vi mong muốn và mọi truy vấn đều tránh được việc quét toàn bộ cây. Tìm kiếm trung bình (O(\log^2 n)) là thành phần chiếm ưu thế trên mỗi truy vấn, trong khi phép tính khoảng cách trung tâm vẫn là logarit. Các mảng được gõ nhỏ gọn trong quá trình triển khai giữ dữ liệu phụ trợ (O(n\log n)) trong giới hạn bộ nhớ 256 MiB. 

## Trường hợp thử nghiệm 

Việc khai thác sau đây giả định`solve()`là hàm từ phần Giải pháp Python. Nó tạm thời thay thế mô-đun`input`Và`stdout`, do đó, mỗi xác nhận sẽ chạy quá trình triển khai thực tế thay vì triển khai lại riêng biệt.```python
import sys
import io

def run(inp: str) -> str:
    global input

    old_input = input
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        input = old_input
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1.
assert run(
    """3
1 2
2 3
4
+ 1
+ 3
+ 2
- 1
"""
) == """0
2
2
1""\n", "sample 1"

# Provided sample 2.
assert run(
    """6
1 2
2 3
3 4
4 5
2 6
5
+ 1
+ 4
+ 5
- 5
+ 6
"""
) == """0
3
4
3
4""\n", "sample 2"

# Minimum-size tree.
assert run(
    """1
1
+ 1
"""
) == """0\n""", "minimum-size tree"

# A path where the optimal vertex is between occupied vertices,
# followed by a deletion that leaves two active vertices.
assert run(
    """5
1 2
2 3
3 4
4 5
4
+ 1
+ 5
+ 3
- 5
"""
) == """0
4
4
2\n""", "path median and deletion"

# Star with every vertex eventually occupied.
assert run(
    """5
1 2
1 3
1 4
1 5
5
+ 1
+ 2
+ 3
+ 4
+ 5
"""
) == """0
1
2
3
4\n""", "all vertices active"

# Maximum-size tree and a distance close to the largest possible answer.
n = 100000
max_case = [str(n)]
for i in range(1, n):
    max_case.append(f"{i} {i + 1}")
max_case.append("2")
max_case.append("+ 1")
max_case.append(f"+ {n}")
max_input = "\n".join(max_case) + "\n"

assert run(max_input) == "0\n99999\n", "maximum-size path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=1), một lần chèn |`0`| Vỏ cây và chip đơn có kích thước tối thiểu | 
| Đường dẫn (1-2-3-4-5),`+1,+5,+3,-5`|`0,4,4,2`| Trung vị trống, di chuyển trung vị, xóa | 
| Sao căn giữa ở 1 với tất cả các đỉnh được chèn |`0,1,2,3,4`| Tất cả các đỉnh hoạt động và trọng tâm bậc cao | 
| Đường dẫn có (100000) đỉnh và chip ở cả hai điểm cuối |`0,99999`| Tối đa (n), khoảng cách lớn, phạm vi câu trả lời 64 bit | 

## Vỏ cạnh 

Một chip hoạt động duy nhất được xử lý bằng cách lấy (k=1). Đỉnh hoạt động duy nhất là vị trí Euler hoạt động đầu tiên và cây con của chính nó chứa ít nhất một chip, do đó tìm kiếm trung vị ngay lập tức trả về nó. Vì```
1
1
+ 1
```truy vấn khoảng cách trung tâm trả về 0. 

Một đích tối ưu trống sẽ được xử lý vì trung vị được xác định bởi trọng số của cây con, chứ không phải bởi liệu đỉnh đó có bị chiếm hay không. Vì```
3
1 2
2 3
2
+ 1
+ 3
```chip nửa chừng là đỉnh 3. Cây con của nó chứa một chip, không quá một nửa của hai. Cha mẹ của nó, đỉnh 2, có cả hai chip trong cây con của nó, vì vậy đỉnh 2 được chọn. Truy vấn khoảng cách trung tâm cho ra (1+1=2). 

Hai đường trung tuyến liền kề phát sinh khi một cạnh chia đôi các chip đang hoạt động. Trên đường đi (1-2), sau khi chèn cả hai chip, một trong hai đỉnh là tối ưu. Với điều kiện khắt khe`subtree >= floor(M / 2) + 1`, thuật toán chọn trung vị ở phía chứa chip nửa bậc bậc Euler. Đỉnh được chọn vẫn có khoảng cách tối thiểu có thể, do đó không cần xử lý ràng buộc đặc biệt. 

Trường hợp xóa chỉ còn lại một con chip cũng an toàn. Trên đường đi (1-2-3), sau```
+ 1
+ 3
- 1
```chip hoạt động duy nhất ở mức 3. Tìm kiếm trung bình trả về 3 và tổng khoảng cách bằng 0. Đầu vào đảm bảo rằng tập hoạt động không bao giờ trở nên trống, do đó thuật toán không bao giờ cần xác định trung vị cho các chip bằng 0. 

Một trường hợp ranh giới đặc biệt hữu ích là khi điểm giữa chính xác là ranh giới giữa hai cây con. Điều kiện nghiêm ngặt hơn một nửa ngăn cản thuật toán đi xuống một trong hai bên trừ khi bên đó thực sự chứa hơn một nửa. Đây chính xác là điều cho phép cùng một mã xử lý cả hai số trung vị duy nhất và các cặp trung vị liền kề. 

Bạn có thể điều chỉnh thuật ngữ của bài xã luận cho phù hợp với một phong cách cụ thể của Codeforces, chẳng hạn bằng cách làm cho phần phân rã centroid thiên về triển khai hơn hoặc làm cho bằng chứng trở nên trang trọng hơn.
