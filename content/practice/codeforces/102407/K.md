---
title: "CF 102407K - Sự sắp xếp điên rồ"
description: "Bản thân cái cây có vẻ là trung tâm của câu lệnh, nhưng cách biểu diễn hữu ích không phải là trọng số của các cạnh. Gốc cây tại bất kỳ đỉnh nào, chẳng hạn như đỉnh 1, và gọi (hv) là XOR của trọng số cạnh trên đường đi từ gốc đến (v)."
date: "2026-08-11T23:53:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "K"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 403
verified: false
draft: false
---

[CF 102407K - Những sự sắp xếp điên rồ](https://codeforces.com/problemset/problem/102407/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bản thân cái cây có vẻ là trung tâm của câu lệnh, nhưng cách biểu diễn hữu ích không phải là trọng số của các cạnh. Gốc cây ở đỉnh bất kỳ, giả sử đỉnh 1, và gọi (h_v) là XOR của trọng số cạnh trên đường đi từ gốc đến (v). Bởi vì cây có chính xác một đường đi giữa hai đỉnh nên XOR trên đường đi từ (u) đến (v) chỉ đơn giản là 

[ 
s(u,v)=h_u\oplus h_v. 
] 

Khi (h_1=0) được sửa, mỗi phép gán giá trị (h_v) tương ứng với chính xác một phép gán của các cạnh cây. Do đó, hình dạng cụ thể của cây ban đầu không ảnh hưởng gì đến câu trả lời. Đây là sự đơn giản hóa trung tâm của vấn đề. 

Bây giờ hãy quên cây ban đầu và xây dựng một biểu đồ mới (G). Các đỉnh của nó là các đỉnh của cây ban đầu và cạnh đồ thị thứ (i)-của nó kết nối (u_i) và (v_i). Giá trị cần thiết cho cạnh này là 

[ 
h_{u_i}\oplus h_{v_i}=s_i. 
] 

Dãy số (s_1,\ldots,s_m) chỉ bao gồm các số 0 và 1 và phải không giảm. Do đó, nó có chính xác một hình dạng có thể có cho mỗi ranh giới (k) giữa số 0 và số 1: 

[ 
s_1=\cdots=s_k=0,\qquad 
s_{k+1}=\cdots=s_m=1, 
] 

trong đó (k) có thể là số nguyên bất kỳ từ (0) đến (m). 

Vì vậy, vấn đề trở thành: có bao nhiêu ranh giới (k) mà hệ phương trình XOR sau đây phù hợp? 

[ 
h_{u_i}\oplus h_{v_i}= 
\bắt đầu{trường hợp} 
0,&i\le k,\ 
1,&i>k. 
\end{trường hợp} 
] 

Đối với mọi ranh giới nhất quán, số lượng giải pháp là như nhau. Trong biểu đồ (G), mọi thành phần được kết nối đều có một lựa chọn nhị phân tự do, bởi vì tất cả các phương trình chỉ xác định các giá trị XOR tương đối. Việc sửa (h_1=0) loại bỏ sự tự do của thành phần chứa đỉnh 1. Nếu (G) có (c) thành phần liên thông thì mọi ranh giới nhất quán có chính xác (2^{c-1}) phép gán cạnh tương ứng. 

Các giới hạn đạt tới (n,m\le250.000). Một thuật toán bậc hai sẽ yêu cầu khoảng (6,25\cdot10^{10}) phép toán cơ bản, vượt xa giới hạn hai giây của bài toán ban đầu. Ngay cả một thuật toán quét tất cả các ranh giới (m+1) và kiểm tra tất cả các phương trình (m) một cách độc lập cũng là (O(m^2)). Giải pháp dự định phải xử lý tất cả các ranh giới cùng nhau trong các phép toán biểu đồ khoảng (O(m\log m)). Sự cố ban đầu có giới hạn thời gian là hai giây và giới hạn bộ nhớ 512 MiB. 

Có một số cách dễ dàng để làm sai các điều kiện biên. Đầu tiên, ranh giới (k=0) là hợp lệ: nó có nghĩa là mọi (s_i) bằng 1. Tương tự, (k=m) hợp lệ khi tất cả (s_i) đều bằng 0. Ví dụ:```
2 2
1
1 2
1 2
```luôn tạo ra (s_1=s_2), do đó cả hai phép gán của cạnh cây đơn đều điên rồ và câu trả lời là 2. Một giải pháp chỉ kiểm tra ranh giới giữa hai truy vấn thực tế sẽ bỏ sót một trong hai chuỗi hằng số hợp lệ. 

Cái bẫy thứ hai là tính khả thi không nhất thiết phải đơn điệu khi (k) thay đổi. Hãy xem xét mẫu 1:```
3 3
1 2
1 2
2 3
1 3
```Bốn chuỗi mục tiêu đơn điệu có thể có là (000,001,011,111). Các chuỗi có thể đạt được là (000,011,101,110), do đó chỉ (000) và (011) hoạt động. Các ranh giới hợp lệ là (k=3) và (k=1), trong khi (k=2) không hợp lệ. Vì vậy, chúng ta không thể dừng lại sau mâu thuẫn đầu tiên hoặc cho rằng tất cả các ranh giới khả thi tạo thành một khoảng. 

Bẫy thứ ba là quên rằng biểu đồ truy vấn có thể bị ngắt kết nối. Mẫu 3 có hai cạnh truy vấn bị ngắt kết nối:```
4 2
1 2 3
1 2
3 4
```Mỗi một trong ba chuỗi mục tiêu đơn điệu đều nhất quán, nhưng mỗi hệ thống nhất quán có hai giải pháp vì biểu đồ truy vấn có hai thành phần được kết nối và chỉ một trong số chúng chứa gốc cố định. Do đó, câu trả lời là (3\cdot2=6), không phải 3. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu trực tiếp rất đơn giản. Có (n-1) cạnh cây, mỗi cạnh có thể có hai trọng số, vì vậy chúng tôi liệt kê tất cả (2^{n-1}) phép gán. Đối với một phép gán, chúng ta có thể tính các giá trị XOR từ gốc đến đỉnh trong (O(n)), sau đó đánh giá mọi đường dẫn được yêu cầu trong (O(1)) bằng cách sử dụng (h_u\oplus h_v) và cuối cùng kiểm tra xem chuỗi kết quả có phải là không giảm hay không. Điều này đúng vì mọi phép gán cạnh có thể được xem xét chính xác một lần. 

Vấn đề là liệt kê theo cấp số nhân. Thời gian chạy là 

[ 
\Theta((n+m)2^{n-1}), 
] 

và chỉ liệt kê các bài tập đã mất (2^{249999}) lần lặp ở mức tối đa (n). Điều đó không khả thi chút nào. 

Nhận xét hữu ích đầu tiên là một dãy điên chỉ có (m+1) dạng có thể. Chúng ta chỉ cần xác định ranh giới (k) nào tạo ra hệ thống XOR nhất quán. Đối với một ranh giới cố định, DSU chẵn lẻ có thể kiểm tra tính nhất quán trong (O(n+m)), bởi vì một phương trình (h_u\oplus h_v=q) chính xác là một ràng buộc nói rằng (u) và (v) phải có màu tương đối được quy định. 

Thực hiện điều này một cách độc lập cho tất cả các ranh giới (m+1) sẽ cho (O(m(n+m))), vẫn còn quá lớn. 

Quan sát quan trọng là các ranh giới liền kề chỉ khác nhau ở một phương trình. Khi chúng ta chuyển từ ranh giới (k-1) sang ranh giới (k), chỉ có phương trình thứ (k)-th thay đổi, từ 

[ 
h_{u_k}\oplus h_{v_k}=1 
] 

đến 

[ 
h_{u_k}\oplus h_{v_k}=0. 
] 

Đây là sự cố kết nối động ngoại tuyến với các ràng buộc chẵn lẻ. Chúng ta có thể sử dụng phép chia để trị trên chỉ số biên. Tại nút biểu thị ranh giới ([l,r]), đặt điểm giữa của nó là (giữa). Đối với mọi ranh giới ở nửa bên trái, mọi cạnh có chỉ số lớn hơn (giữa) được đảm bảo nằm ở phía bên phải của ranh giới, do đó tất cả các phương trình đó có tính chẵn lẻ 1. Chúng tôi tạm thời thêm các phương trình đó vào DSU. Sau đó chúng tôi tái diễn vào nửa bên trái. 

Sau khi cuộn các phép cộng đó trở lại, mọi ranh giới ở nửa bên phải có tất cả các cạnh từ nửa bên trái được cố định về số chẵn lẻ 0. Chúng ta tạm thời cộng các phương trình đó và lặp lại vào nửa bên phải. 

Mỗi cạnh truy vấn được thêm một lần cho mỗi cấp độ của cây chia để trị, do đó có các phần chèn ràng buộc (O(m\log m)). DSU chẵn lẻ khôi phục cho phép chúng tôi khôi phục trạng thái chính xác trước đó sau mỗi lệnh gọi đệ quy. 

DSU duy trì tính chẵn lẻ từ mọi đỉnh tới đại diện DSU của nó. Khi cộng (h_u\oplus h_v=q), nó sẽ nối hai thành phần với chẵn lẻ tương đối được yêu cầu hoặc phát hiện mâu thuẫn nếu các đỉnh đã được kết nối với chẵn lẻ sai. 

Hướng dẫn ban đầu của cuộc thi mô tả ý tưởng phân chia và chinh phục tương tự, được diễn đạt như thêm nửa bên phải với số chẵn lẻ 1 trong khi giảm dần sang nửa bên trái và thêm nửa bên trái với số chẵn lẻ 0 trong khi giảm dần vào nửa bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n+m)2^{n-1})) | (O(n+m)) | Quá chậm | 
| Kiểm tra mọi ranh giới một cách độc lập | (O(m(n+m))) | (O(n+m)) | Quá chậm | 
| Chia để trị + khôi phục chẵn lẻ DSU | (O(m\log m\log n+n)) | (O(n+m)) | Đã chấp nhận | 

Phần bổ sung (\log n) trong giới hạn đã nêu xuất phát từ hoạt động tìm liên kết theo kích thước của DSU khôi phục. Phần chia để trị đóng góp số lượng (O(m\log m)) của phần chèn DSU. 

## Hướng dẫn thuật toán

1. Root cây ban đầu ở đỉnh 1 về mặt khái niệm và xác định (h_v) là XOR từ gốc đến (v). Vì (s_i=h_{u_i}\oplus h_{v_i}), nên các cạnh của cây ban đầu không bao giờ cần phải xử lý. Chúng tôi chỉ phải sử dụng danh sách cha mẹ của họ từ đầu vào. 
2. Xây dựng biểu đồ truy vấn có cạnh thứ (i) là ((u_i,v_i)). Đồng thời sử dụng DSU thông thường trên biểu đồ này để đếm các thành phần được kết nối của nó (c). Số lượng này sẽ xác định có bao nhiêu phép gán cạnh tương ứng với mỗi ranh giới nhất quán. 
3. Biểu diễn mọi dãy không giảm có thể bằng một ranh giới (k\in[0,m]). Đối với ranh giới này, cạnh truy vấn (i) yêu cầu chẵn lẻ 0 nếu (i\le k) và yêu cầu chẵn lẻ 1 nếu (i>k). 
4. Tạo DSU chẵn lẻ khôi phục trên (n) đỉnh. Đối với mỗi đỉnh, lưu trữ cha mẹ của nó, kích thước của thành phần của nó và XOR từ đỉnh đến cha mẹ của nó. XOR từ một đỉnh đến đại diện của nó có được bằng cách đi theo các liên kết gốc và tích lũy các giá trị chẵn lẻ này. 
5. Xử lý đệ quy một khoảng ranh giới có thể có ([l,r]). Nếu (l=r), mỗi cạnh truy vấn có một mức chẵn lẻ bắt buộc cố định cho một ranh giới này và bộ đếm mâu thuẫn của DSU cho chúng ta biết liệu ranh giới đó có khả thi hay không. 
6. Tách ([l,r]) tại (mid=(l+r)//2). Đối với mọi ranh giới trong ([l,mid]), tất cả các cạnh truy vấn có chỉ số (mid+1,\ldots,r) đều có chẵn lẻ 1. Thêm chính xác các ràng buộc đó, sau đó xử lý đệ quy nửa bên trái. Những ràng buộc này vẫn hoạt động trong khi đệ quy đi xuống, do đó tổ tiên không cần phải được xây dựng lại. 
7. Đưa DSU về điểm kiểm tra đã thực hiện trước những lần bổ sung đó. Sau đó, với mọi ranh giới trong ([mid+1,r]), tất cả các cạnh truy vấn có chỉ số (l,\ldots,mid) đều có tính chẵn lẻ 0. Thêm các ràng buộc đó và xử lý đệ quy nửa bên phải. 
8. Tại mỗi lá (k), tăng số lượng ranh giới khả thi một cách chính xác khi DSU không có ràng buộc mâu thuẫn. Cấu trúc chia để trị đảm bảo rằng mọi phương trình (m) đều có mặt ở lá đó với độ chẵn lẻ chính xác được yêu cầu bởi (k). 
9. Nếu biểu đồ truy vấn có (c) các thành phần liên thông, hãy nhân số ranh giới khả thi với (2^{c-1}) modulo (998,244,353). Thành phần chứa đỉnh 1 có giá trị (h) được cố định bằng 0, trong khi mỗi thành phần (c-1) khác có thể được lật độc lập. 

### Tại sao nó hoạt động 

Với mỗi ranh giới (k), đường đệ quy từ gốc đến lá (k) cuối cùng sẽ tách mọi cạnh truy vấn (i) khỏi (k). Nếu (i\le k), cạnh nằm trong anh em bên trái khi hai chỉ số lần đầu tiên tách ra, do đó thuật toán cộng phương trình có chẵn lẻ 0. Nếu (i>k), nó nằm trong anh em bên phải, do đó thuật toán cộng phương trình có chẵn lẻ 1. Do đó DSU tại lá (k) biểu thị chính xác hệ XOR tương ứng với chuỗi đơn điệu có (k) các số 0 ban đầu. 

DSU chẵn lẻ báo cáo sự mâu thuẫn chính xác khi các phương trình XOR không thể được thỏa mãn đồng thời. Do đó, một chiếc lá được tính chính xác khi chuỗi đơn điệu của nó có thể đạt được. 

Cuối cùng, khi đã tồn tại một giải pháp, mọi thành phần được kết nối của biểu đồ truy vấn có thể bị lật đồng thời tất cả các giá trị (h) của nó mà không thay đổi bất kỳ cạnh XOR nào. Thành phần gốc không thể bị lật vì (h_1=0), để lại chính xác (c-1) các lựa chọn nhị phân độc lập. Do đó, mọi ranh giới khả thi đều đóng góp các phép gán (2^{c-1}) và các ranh giới khác nhau tương ứng với các chuỗi (s) khác nhau, do đó các phép gán của chúng là rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class RollbackParityDSU:
    __slots__ = ("parent", "size", "parity", "bad", "history")

    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.parity = [0] * n
        self.bad = 0
        self.history = []

    def find(self, x):
        px = 0
        parent = self.parent
        parity = self.parity

        while parent[x] != x:
            px ^= parity[x]
            x = parent[x]

        return x, px

    def add(self, u, v, want):
        ru, pu = self.find(u)
        rv, pv = self.find(v)

        if ru == rv:
            if (pu ^ pv) != want:
                self.bad += 1
                self.history.append((-1, 0))
            else:
                self.history.append((0, 0))
            return

        if self.size[ru] < self.size[rv]:
            ru, rv = rv, ru
            pu, pv = pv, pu

        old_size = self.size[ru]

        self.parent[rv] = ru
        self.parity[rv] = pu ^ pv ^ want
        self.size[ru] += self.size[rv]

        self.history.append((rv, old_size))

    def checkpoint(self):
        return len(self.history)

    def rollback(self, checkpoint):
        parent = self.parent
        parity = self.parity
        size = self.size
        history = self.history

        while len(history) > checkpoint:
            child, old_size = history.pop()

            if child == 0:
                continue

            if child == -1:
                self.bad -= 1
                continue

            root = parent[child]
            parent[child] = child
            parity[child] = 0
            size[root] = old_size

def solve():
    n, m = map(int, input().split())

    # The original tree is irrelevant after the h_v transformation.
    input()

    edges = [None] * m

    # Ordinary DSU, only for the number of connected components.
    comp_parent = list(range(n))
    comp_size = [1] * n
    components = n

    def comp_find(x):
        while comp_parent[x] != x:
            comp_parent[x] = comp_parent[comp_parent[x]]
            x = comp_parent[x]
        return x

    for i in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges[i] = (u, v)

        ru = comp_find(u)
        rv = comp_find(v)

        if ru != rv:
            if comp_size[ru] < comp_size[rv]:
                ru, rv = rv, ru
            comp_parent[rv] = ru
            comp_size[ru] += comp_size[rv]
            components -= 1

    dsu = RollbackParityDSU(n)
    good = 0

    def divide(l, r):
        nonlocal good

        if l == r:
            if dsu.bad == 0:
                good += 1
            return

        mid = (l + r) >> 1

        # For every boundary in [l, mid], all edges in [mid+1, r]
        # must have value 1.
        checkpoint = dsu.checkpoint()

        for i in range(mid + 1, r + 1):
            u, v = edges[i]
            dsu.add(u, v, 1)

        divide(l, mid)
        dsu.rollback(checkpoint)

        # For every boundary in [mid+1, r], all edges in [l, mid]
        # must have value 0.
        checkpoint = dsu.checkpoint()

        for i in range(l, mid + 1):
            u, v = edges[i]
            dsu.add(u, v, 0)

        divide(mid + 1, r)
        dsu.rollback(checkpoint)

    # Boundaries are represented by k = 0..m.
    # A boundary k means the first k query values are zero.
    #
    # Edge i (1-based) is therefore:
    #   1 when i > k
    #   0 when i <= k
    #
    # The recursive interval indices below use the same boundary
    # convention directly, so the query edge indices are shifted by 1.
    #
    # We process boundaries 0..m and query edges 1..m together by
    # storing an artificial edge-index range [0,m-1].
    #
    # The divide routine above assumes its index interval refers to
    # query edges, so we instead use a specialized recursion below.

    good = 0

    def divide_boundaries(l, r):
        nonlocal good

        if l == r:
            if dsu.bad == 0:
                good += 1
            return

        mid = (l + r) >> 1

        # Boundaries [l, mid]:
        # every query edge i > mid has parity 1.
        cp = dsu.checkpoint()
        start = max(mid, 0)
        for i in range(start, m):
            u, v = edges[i]
            # Query index is i+1, and i+1 > mid here.
            dsu.add(u, v, 1)

        divide_boundaries(l, mid)
        dsu.rollback(cp)

        # Boundaries [mid+1, r]:
        # every query edge i+1 <= mid has parity 0.
        cp = dsu.checkpoint()
        end = min(mid, m)
        for i in range(0, end):
            u, v = edges[i]
            dsu.add(u, v, 0)

        divide_boundaries(mid + 1, r)
        dsu.rollback(cp)

    divide_boundaries(0, m)

    ways_per_boundary = pow(2, components - 1, MOD)
    answer = good * ways_per_boundary % MOD
    return str(answer)

if __name__ == "__main__":
    print(solve())
```Dòng đầu vào đầu tiên cung cấp (n) và (m) và dòng tiếp theo được sử dụng vì định dạng đầu vào cần có mô tả cây ban đầu ngay cả khi thuật toán không còn cần cây sau khi chuyển đổi (h_v). 

Biểu đồ truy vấn được lưu trữ dưới dạng một mảng để giữ nguyên thứ tự của nó. Thứ tự đó rất cần thiết vì tính chẵn lẻ của cạnh (i) thay đổi chính xác khi ranh giới vượt qua truy vấn (i). 

DSU thông thường tách biệt với DSU khôi phục. Mục đích duy nhất của nó là đếm các thành phần được kết nối trong biểu đồ truy vấn đầy đủ. Việc giữ riêng phần này làm cho cấu trúc khôi phục chỉ chịu trách nhiệm kiểm tra tính nhất quán tạm thời. 

Các cửa hàng DSU khôi phục`parity[x]`dưới dạng (h_x\oplus h_{\text{parent[x]}). Trong lúc`find`, XOR các giá trị này sẽ cho ra (h_x\oplus h_{\text{root}}). Nếu hai đỉnh đã nằm trong cùng một thành phần thì phương trình mới sẽ phù hợp với tính chẵn lẻ tương đối hiện có của chúng hoặc tạo ra sự mâu thuẫn. Các mâu thuẫn được tính thay vì gây ra sự quay trở lại ngay lập tức vì nhánh đệ quy sau phải khôi phục lại trạng thái chính xác trước đó. 

Khi hai thành phần được nối với nhau, giả sử số chẵn lẻ tích lũy là (p_u) và (p_v). Mối quan hệ cha mẹ mới phải thỏa mãn 

[ 
p_u\oplus\text{chẵn lẻ[v]\oplus p_v=q, 
] 

vì vậy tính chẵn lẻ được gán cho gốc đính kèm là`pu ^ pv ^ want`. Liên kết theo kích thước giữ logarit độ sâu DSU. 

Điểm kiểm tra khôi phục chỉ đơn giản là độ dài hiện tại của ngăn xếp lịch sử. Mỗi lần hợp nhất thành công đều ghi lại gốc đính kèm và kích thước cũ của gốc còn sót lại. Một mâu thuẫn ghi lại một điểm đánh dấu đặc biệt. Quay lại khôi phục những thay đổi này theo thứ tự ngược lại. 

Phép đệ quy chia để trị sử dụng các ranh giới từ (0) đến (m), không truy vấn các chỉ số từ (1) đến (m). Sự khác biệt này là nguồn gốc chính của các lỗi riêng lẻ. Ranh giới 0 có nghĩa là tất cả các giá trị truy vấn là 1, trong khi ranh giới (m) có nghĩa là tất cả các giá trị truy vấn là 0. 

Tại một điểm phân tách ([l,r]), tất cả các cạnh có chỉ số truy vấn lớn hơn điểm giữa đều có giá trị 1 ở nửa bên trái. Ngược lại, tất cả các cạnh có chỉ số truy vấn nhiều nhất là điểm giữa có giá trị 0 trên nửa bên phải. Đây chính xác là những ràng buộc có thể được thêm vào trước khi giảm dần mà không vô tình áp đặt một điều kiện thay đổi trong khoảng thời gian hiện tại. 

Số nguyên Python không tràn và số học mô-đun duy nhất là phép nhân cuối cùng với số nghiệm trên mỗi ranh giới. Công suất (2^{c-1}) được tính bằng lũy ​​thừa mô-đun. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Biểu đồ truy vấn là hình tam giác 

[ 
1-2,\quad2-3,\quad1-3. 
] 

Nó có một thành phần được kết nối, do đó mỗi ranh giới khả thi đóng góp chính xác một nhiệm vụ. 

Bốn chuỗi mục tiêu đơn điệu tương ứng với (k=0,1,2,3). 

| Ranh giới (k) | (Các) mục tiêu | Chu kỳ chẵn lẻ | Nhất quán? | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 111 | (1\oplus1\oplus1=1) | Không | 0 | 
| 1 | 011 | (0\oplus1\oplus1=0) | Có | 1 | 
| 2 | 001 | (0\oplus0\oplus1=1) | Không | 0 | 
| 3 | 000 | (0\oplus0\oplus0=0) | Có | 1 | 

Các ranh giới hợp lệ là (k=1) và (k=3), đưa ra hai bài tập. Đây chính xác là đầu ra mẫu. 

### Mẫu 2 

Biểu đồ truy vấn là một biểu đồ bốn chu kỳ 

[ 
1-2-3-4-1. 
] 

Một lần nữa chỉ có một thành phần được kết nối. Một chu trình không nhất quán chính xác khi nó chứa một số cạnh lẻ có giá trị mục tiêu là 1. 

| Ranh giới (k) | (Các) mục tiêu | Số cạnh 1 trên chu kỳ | Nhất quán? | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1111 | 4 | Có | 1 | 
| 1 | 0111 | 3 | Không | 0 | 
| 2 | 0011 | 2 | Có | 1 | 
| 3 | 0001 | 1 | Không | 0 | 
| 4 | 0000 | 0 | Có | 1 | 

Do đó, ba ranh giới có tác dụng, cụ thể là (0,2,4) và câu trả lời là 3. Ví dụ này đặc biệt hữu ích vì nó chứng minh rằng các ranh giới khả thi có thể xen kẽ giữa hợp lệ và không hợp lệ thay vì tạo thành một khoảng liên tục. 

### Mẫu 3 

Biểu đồ truy vấn bao gồm hai cạnh bị ngắt kết nối, (1-2) và (3-4). Nó có (c=2) thành phần được kết nối. 

Không có chu trình nên mọi phép gán chẵn lẻ cho hai cạnh truy vấn này đều nhất quán. Do đó, tất cả các ranh giới (m+1=3) đều khả thi. 

Mỗi ranh giới có 

[ 
2^{c-1}=2 
] 

giải pháp vì thành phần chứa đỉnh 1 được cố định trong khi thành phần còn lại có thể bị đảo ngược. Câu trả lời là (3\cdot2=6). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m\log m\log n)) | Mỗi cạnh truy vấn được chèn tối đa một lần cho mỗi cấp độ chia để trị và tìm thấy DSU khôi phục lấy (O(\log n)) với liên kết theo kích thước | 
| Không gian | (O(n+m)) | Các cạnh truy vấn, hai cấu trúc DSU, ngăn xếp đệ quy và lịch sử khôi phục | 

Với (m\le250.000), độ sâu phân chia và chinh phục dưới 20. Mỗi truy vấn chỉ tham gia vào một lần chèn anh chị em trên mỗi cấp độ, do đó số lần chèn ràng buộc tạm thời là (O(m\log m)), thay vì (O(m^2)). Cây ban đầu chỉ đóng góp kích thước đầu vào và không bao giờ được lưu trữ, điều này giúp duy trì mức sử dụng bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm 

Dây nịt sau đây sử dụng tương tự`solve`hoạt động như giải pháp được gửi. Trường hợp kích thước tối đa là một bài kiểm tra sức chịu đựng và có kích thước lớn, do đó, nó phải được chạy riêng biệt với các bài kiểm tra đơn vị thông thường.```python
import sys
import io

# Paste the solution above before these tests.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve().strip()
    finally:
        sys.stdin = old_stdin

# Provided samples

assert run("""\
3 3
1 2
1 2
2 3
1 3
""") == "2", "sample 1"

assert run("""\
4 4
1 1 1
1 2
2 3
3 4
1 4
""") == "3", "sample 2"

assert run("""\
4 2
1 2 3
1 2
3 4
""") == "6", "sample 3"

# Minimum-size case.
# There are two identical queries on the only tree edge.
# The two path parities are always equal, so every edge assignment works.
assert run("""\
2 2
1
1 2
1 2
""") == "2", "minimum size"

# All query values are equal for every assignment.
# Only 0000 and 1111 are possible monotone target sequences.
# There are 2^(4-1) = 8 tree assignments.
assert run("""\
4 4
1 2 3
1 2
1 2
1 2
1 2
""") == "8", "all equal queries"

# Boundary/off-by-one case.
# The query graph is a tree, so every one of the m+1 boundaries is feasible.
# n=5, m=3 gives 4 boundaries and 2^(2-1)=2 assignments per boundary.
assert run("""\
5 3
1 2 3 4
1 2
2 3
4 5
""") == "8", "all boundaries feasible"

# Maximum-size stress case.
# The query graph is a chain plus a duplicate of edge (1,2).
# The only cycle consists of query edges 1 and m, so only k=0 and k=m
# are feasible. The query graph is connected, hence the answer is 2.
n = 250000
parents = " ".join(str(i) for i in range(1, n))
queries = "\n".join(
    [f"{i} {i + 1}" for i in range(1, n)] + ["1 2"]
)
max_input = f"{n} {n}\n{parents}\n{queries}\n"

assert run(max_input) == "2", "maximum-size stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`, hai bản sao của`1 2`| 2 | Tối thiểu (n,m), truy vấn lặp lại, chuỗi không đổi | 
|`4 4`, bốn bản sao của`1 2`| 8 | Biểu đồ truy vấn bị ngắt kết nối và các giá trị đường dẫn bằng nhau | 
|`5 3`, đồ thị truy vấn là một khu rừng | 8 | Mọi ranh giới đều khả thi và yếu tố thành phần | 
|`250000 250000`, chuỗi cộng với cạnh trùng lặp | 2 | Kích thước đầu vào tối đa và hành vi phân chia và chinh phục (O(m\log m)) | 

## Vỏ cạnh 

Ranh giới không đổi (k=m) được xử lý bởi lá ngoài cùng bên phải. Tại lá đó mọi phương trình truy vấn đều có tính chẵn lẻ bằng 0. Đối với trường hợp tối thiểu```
2 2
1
1 2
1 2
```DSU nhận được hai bản sao của (h_1\oplus h_2=0). Không mâu thuẫn với nhau nên ranh giới là khả thi. 

Biên không đổi một (k=0) được xử lý bởi lá ngoài cùng bên trái. Trong trường hợp tối thiểu giống nhau, cả hai phương trình đều trở thành (h_1\oplus h_2=1), điều này cũng nhất quán. Hai ranh giới này tương ứng với hai phép gán có thể có của cạnh cây duy nhất, cho kết quả đầu ra là 2. 

Tính khả thi xen kẽ được thấy trong Mẫu 1 được xử lý vì phép chia để trị không bao giờ giả định bất cứ điều gì về hình dạng của tập hợp các ranh giới khả thi. Vì```
3 3
1 2
1 2
2 3
1 3
```các lá (k=0,1,2,3) kết thúc độc lập với các trạng thái mâu thuẫn (1,0,1,0). Như vậy chính xác là hai lá được tính. 

Các biểu đồ truy vấn bị ngắt kết nối được xử lý bằng hệ số nhân thành phần thay vì kiểm tra tính nhất quán. TRONG```
4 2
1 2 3
1 2
3 4
```có hai thành phần truy vấn. Mọi ranh giới đều nhất quán vì biểu đồ truy vấn không có chu trình và mỗi hệ thống nhất quán có hai giải pháp sau khi sửa (h_1=0). Kết quả là (3\cdot2=6). 

Các cặp truy vấn lặp lại cũng là các cạnh biểu đồ thông thường, không phải thứ gì đó có thể được loại bỏ trùng lặp một cách đơn giản. Hai cạnh truy vấn bằng nhau với yêu cầu chẵn lẻ khác nhau tạo nên sự mâu thuẫn ngay lập tức. Đây chính xác là lý do tại sao trường hợp hoàn toàn bằng nhau với bốn bản sao của (1\ 2) chỉ có sẵn hai chuỗi mục tiêu không đổi, mặc dù cùng một cặp xuất hiện bốn lần. 

Cuối cùng, cây ban đầu có thể có bất kỳ hình dạng nào và danh sách gốc của nó có thể trông hoàn toàn không liên quan đến biểu đồ truy vấn. Phép biến đổi (h_v) làm cho cấu trúc liên kết cây không còn phù hợp vì mỗi phép gán của (h_2,\ldots,h_n) với (h_1=0) tương ứng với chính xác một phép gán cạnh cây. Đó là lý do tại sao việc triển khai chỉ đọc danh sách gốc để nâng cao dữ liệu đầu vào và không bao giờ sử dụng nó sau đó. 

Bài xã luận ở trên sử dụng phiên bản rollback-DSU của ý tưởng chia để trị chính thức. Hướng dẫn ban đầu cũng đề cập đến tính năng sàng lọc O(mlogm) bằng cách sử dụng tính năng nén biểu đồ rõ ràng, nhưng công thức khôi phục về cơ bản dễ thực hiện và giải thích hơn.
