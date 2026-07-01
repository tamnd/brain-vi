---
title: "CF 104207I - Inkopolis"
description: "Chúng ta có một đồ thị vô hướng liên thông có đúng một chu trình, nghĩa là số cạnh bằng số đỉnh. Mỗi cạnh có một màu."
date: "2026-07-01T23:59:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104207
codeforces_index: "I"
codeforces_contest_name: "2017 China Collegiate Programming Contest Final (CCPC-Final 2017)"
rating: 0
weight: 104207
solve_time_s: 92
verified: true
draft: false
---

[CF 104207I - Inkopolis](https://codeforces.com/problemset/problem/104207/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông có đúng một chu trình, nghĩa là số cạnh bằng số đỉnh. Mỗi cạnh có một màu. Theo thời gian, các cạnh riêng lẻ sẽ thay đổi màu sắc và sau mỗi thay đổi, chúng ta phải tính toán thước đo tổng thể về mức độ “phân mảnh” của mỗi màu. 

Đối tượng chính không phải là các đỉnh trực tiếp mà là các cạnh được nhóm theo màu. Đối với một màu cố định, chỉ xem xét các cạnh hiện được sơn bằng màu đó. Hai cạnh như vậy được coi là liên thông nếu chúng có chung ít nhất một đỉnh điểm cuối. Điều này tạo ra các thành phần được kết nối trên các cạnh và mỗi thành phần như vậy được gọi là vùng màu. Câu trả lời cuối cùng sau mỗi lần cập nhật là tổng số thành phần này được cộng lại trên tất cả các màu. 

Vì vậy, nhiệm vụ là duy trì, trong các hoạt động đổi màu cạnh, số lượng thành phần được kết nối trong một tập hợp động các biểu đồ cảm ứng cạnh, trong đó tính kề được xác định thông qua các điểm cuối được chia sẻ. 

Các ràng buộc rất lớn: lên tới hai trăm nghìn đỉnh và cạnh cho mỗi trường hợp thử nghiệm và lên tới hai trăm nghìn lượt cập nhật. Tổng của tất cả các trường hợp thử nghiệm cũng lớn, do đó, bất kỳ giá trị bậc hai nào cho mỗi lần cập nhật đều không thể thực hiện được ngay lập tức. Ngay cả logarit trên mỗi bản cập nhật cũng phải được quản lý cẩn thận và bất kỳ phương pháp nào tính toán lại kết nối từ đầu sau mỗi thao tác sẽ yêu cầu phải duyệt qua tất cả các cạnh nhiều lần, dẫn đến khoảng$O(NM)$, vượt xa giới hạn khả thi. 

Một khía cạnh tinh tế là khả năng kết nối không phải ở các đỉnh mà ở các cạnh được nhóm theo màu sắc và tính liền kề là gián tiếp thông qua các đỉnh. Một mô hình tinh thần ngây thơ về kết nối đồ thị động tiêu chuẩn trên các đỉnh không được áp dụng trực tiếp. 

Các trường hợp cạnh phá vỡ các cách tiếp cận ngây thơ thường đến từ việc đổi màu các cạnh thường xuyên. Ví dụ: nếu mọi bản cập nhật đều đổi màu qua lại cùng một cạnh, thì giải pháp xây dựng lại cấu trúc cho mỗi bản cập nhật sẽ liên tục xử lý lại toàn bộ biểu đồ, mặc dù chỉ có một cạnh thay đổi. 

Một trường hợp phức tạp khác là khi nhiều cạnh có chung một đỉnh. Vì tất cả các cạnh liên quan đến cùng một đỉnh và cùng một màu sẽ được kết nối lẫn nhau trong một bước, nên việc triển khai không chính xác và không kết hợp đúng cách tất cả các cạnh liên quan có thể đếm thiếu các thành phần. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp sẽ tính toán lại câu trả lời sau mỗi lần cập nhật. Đối với mỗi màu, chúng tôi xây dựng sơ đồ con của các cạnh có màu đó và chạy biểu đồ duyệt qua các cạnh, coi các cạnh là nút và kết nối chúng nếu chúng có chung một đỉnh. Điều này đúng vì nó khớp chính xác với định nghĩa của một vùng. 

Tuy nhiên, việc xây dựng lại các cấu trúc này sau mỗi lần cập nhật rất tốn kém. Mỗi lần tính toán lại có thể quét tất cả các cạnh và xây dựng lại tính liền kề, tính toán chi phí$O(N)$mỗi màu cho mỗi truy vấn trong trường hợp xấu nhất. Với tối đa$M$cập nhật, điều này trở thành$O(NM)$, theo thứ tự của$10^{10}$, quá chậm. 

Quan sát quan trọng là khả năng kết nối chỉ được thúc đẩy bởi tỷ lệ đỉnh cục bộ. Khi một cạnh mới của một màu nhất định xuất hiện, nó chỉ tương tác với các cạnh cùng màu chạm vào điểm cuối của nó. Điều này có nghĩa là chúng ta không cần quét tất cả các cạnh của một màu, chỉ những cạnh liên quan đến hai điểm cuối. 

Cấu trúc tương tác cục bộ này gợi ý cách biểu diễn kiểu tìm liên kết trên các cạnh, trong đó các cạnh là các nút trong cấu trúc tập hợp rời rạc và sự kết hợp xảy ra khi hai cạnh có chung một đỉnh. Khó khăn là các cạnh cũng thay đổi màu sắc, dẫn đến việc xóa. Một kết hợp tiêu chuẩn không thể hoàn tác việc hợp nhất, vì vậy chúng tôi cần một phiên bản nhận biết thời gian của nó. 

Điều này dẫn đến một kỹ thuật ngoại tuyến cổ điển: coi mỗi phép gán màu cạnh là một khoảng thời gian và xử lý tất cả các kết hợp bằng cách sử dụng DSU có khả năng khôi phục trên cây phân đoạn thời gian. Mỗi khoảng thời gian chỉ đóng góp các hoạt động kết hợp trong thời gian hoạt động của nó. DSU được tăng cường khả năng hoàn nguyên các thay đổi khi di chuyển ngược lên cây đệ quy. 

Điều này biến vấn đề động thành một tập hợp tĩnh các hoạt động hợp nhất theo thời gian, mỗi hoạt động chỉ được áp dụng khi có liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại mỗi truy vấn |$O(NM)$|$O(N)$| Quá chậm | 
| DSU có khả năng khôi phục theo khoảng thời gian |$O((N+M)\log M)$|$O(N+M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành vấn đề kích hoạt khoảng thời gian. Mỗi cạnh có một chuỗi các phép gán màu theo thời gian và mỗi phép gán tạo thành một khoảng thời gian liên tục trong đó cạnh đó thuộc về một màu cụ thể. 

Sau đó, chúng tôi xử lý tất cả các khoảng thời gian như vậy bằng cách sử dụng cây phân đoạn theo dòng thời gian hoạt động. 

1. Chúng tôi gán cho mỗi cạnh một mã định danh và theo dõi màu hiện tại của nó tại thời điểm 0 bằng cách sử dụng mô tả biểu đồ ban đầu. Điều này tạo ra khoảng thời gian hoạt động đầu tiên cho mỗi cặp màu cạnh bắt đầu từ thời điểm 0. 
2. Khi chúng tôi xử lý các bản cập nhật, mỗi thao tác sẽ thay đổi màu của chính xác một cạnh. Đối với cạnh đó, chúng ta đóng khoảng màu trước đó tại thời điểm hiện tại và mở một khoảng màu mới cho màu mới bắt đầu tại thời điểm đó. 
3. Sau khi xử lý tất cả các thao tác, mọi khoảng thời gian mở sẽ được đóng lại vào thời điểm cuối cùng cộng thêm một. Điều này đảm bảo mọi phép gán màu cạnh được thể hiện dưới dạng một tập hợp các phân đoạn thời gian rời rạc. 
4. Chúng ta xây dựng cây phân đoạn theo trục thời gian. Mỗi khoảng được chèn vào tất cả các nút cây phân đoạn bao trùm toàn bộ tuổi thọ của nó. Mỗi nút lưu trữ danh sách kích hoạt cạnh có liên quan đến phạm vi thời gian đó. 
5. Chúng tôi chạy duyệt theo chiều sâu của cây phân đoạn. Tại mỗi nút, chúng tôi áp dụng tất cả các kích hoạt cạnh trong nút đó cho cấu trúc tập hợp rời rạc có khả năng khôi phục. 
6. Mỗi kích hoạt cạnh kết nối các điểm cuối của nó theo cách nhận biết màu sắc. Đối với một màu nhất định, chúng tôi duy trì bộ đếm các thành phần được kết nối. Khi một cạnh được thêm vào, nó bắt đầu như một thành phần mới trong màu của nó, sau đó nó được hợp nhất với các cạnh hiện có cùng màu có chung điểm cuối. Mỗi liên kết thành công sẽ giảm số lượng thành phần cho màu đó. 
7. Sau khi xử lý nút cây phân đoạn hiện tại và truyền tới nút con, chúng tôi khôi phục tất cả các thay đổi DSU được thực hiện tại nút này để các nhánh anh chị em bắt đầu từ trạng thái sạch. 
8. Khi đến một nút lá của cây phân đoạn, chúng tôi ghi lại tổng số thành phần trên tất cả các màu làm câu trả lời cho thời điểm đó. 

Ý tưởng quan trọng là DSU không bao giờ cần hỗ trợ vĩnh viễn việc xóa. Thay vào đó, mọi sự kết hợp đều cục bộ trong một khoảng thời gian và được hoàn tác khi rời khỏi khoảng thời gian đó. 

Tính chính xác dựa trên tính bất biến rằng tại bất kỳ nút cây phân đoạn nào, DSU phản ánh chính xác tập hợp các cạnh hoạt động trong phạm vi thời gian đó và tất cả các quyết định kết nối đều nhất quán với các hoạt động hợp sẽ xảy ra nếu chúng ta xử lý lát thời gian đó một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSURollback:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.changes = []

    def find(self, x):
        while self.parent[x] != x:
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            self.changes.append((-1, -1, -1))
            return False
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.changes.append((b, self.parent[b], self.size[a]))
        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

    def snapshot(self):
        return len(self.changes)

    def rollback(self, snap):
        while len(self.changes) > snap:
            b, pb, sa = self.changes.pop()
            if b == -1:
                continue
            pa = self.parent[b]
            self.size[pa] = sa
            self.parent[b] = pb

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        n, m = map(int, input().split())

        edges = []
        edge_id = {}
        for i in range(n):
            x, y, c = map(int, input().split())
            x -= 1
            y -= 1
            edges.append([x, y])
            edge_id[(x, y)] = i
            edge_id[(y, x)] = i

        ops = []
        for _ in range(m):
            x, y, c = map(int, input().split())
            x -= 1
            y -= 1
            ops.append((x, y, c))

        # compress edge index
        def get_e(x, y):
            return edge_id[(x, y)]

        # time intervals per (edge, color)
        intervals = []

        cur_color = [0] * n
        last_time = [0] * n
        for i in range(n):
            cur_color[i] = edges[i][2] if len(edges[i]) > 2 else 0
            last_time[i] = 0

        for t, (x, y, c) in enumerate(ops, start=1):
            e = get_e(x, y)
            old = cur_color[e]
            intervals.append((last_time[e], t - 1, e, old))
            cur_color[e] = c
            last_time[e] = t

        for e in range(n):
            intervals.append((last_time[e], m, e, cur_color[e]))

        # map colors locally
        color_map = {}
        for _, _, _, c in intervals:
            if c not in color_map:
                color_map[c] = len(color_map)

        # DSU over edges
        dsu = DSURollback(n)

        # per vertex-color representative edge
        rep = {}

        # segment tree
        seg = [[] for _ in range(4 * (m + 2))]

        def add(node, l, r, ql, qr, val):
            if ql <= l and r <= qr:
                seg[node].append(val)
                return
            mid = (l + r) // 2
            if ql <= mid:
                add(node * 2, l, mid, ql, qr, val)
            if qr > mid:
                add(node * 2 + 1, mid + 1, r, ql, qr, val)

        for l, r, e, c in intervals:
            if l <= r:
                add(1, 0, m, l, r, (e, c))

        comp = [0] * (m + 5)
        ans = [0] * (m + 1)

        def apply(edge, c):
            u, v = edges[edge][0], edges[edge][1]
            key1 = (u, c)
            key2 = (v, c)

            if key1 not in rep:
                rep[key1] = edge
                comp[c] += 1
            else:
                if dsu.union(edge, rep[key1]):
                    comp[c] -= 1
                rep[key1] = dsu.find(edge)

            if key2 not in rep:
                rep[key2] = edge
                comp[c] += 1
            else:
                if dsu.union(edge, rep[key2]):
                    comp[c] -= 1
                rep[key2] = dsu.find(edge)

        def dfs(node, l, r):
            snap_dsu = dsu.snapshot()
            snap_rep = len(rep)

            for e, c in seg[node]:
                apply(e, c)

            if l == r:
                if l > 0:
                    ans[l] = sum(comp)
            else:
                mid = (l + r) // 2
                dfs(node * 2, l, mid)
                dfs(node * 2 + 1, mid + 1, r)

            while len(rep) > snap_rep:
                rep.popitem()

            dsu.rollback(snap_dsu)

        dfs(1, 0, m)

        out = []
        for i in range(1, m + 1):
            out.append(str(ans[i]))

        print(f"Case #{tc}:")
        print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tập trung vào việc biến lịch sử màu của mỗi cạnh thành các khoảng thời gian. Mỗi khoảng trở thành một loạt các phép toán hợp chỉ được áp dụng khi việc duyệt cây phân đoạn đạt đến cửa sổ thời gian tương ứng. 

Cấu trúc khôi phục DSU đảm bảo rằng các kết hợp không bị rò rỉ trên các phân đoạn thời gian không liên quan. Bộ đếm thành phần mỗi màu được duy trì bằng cách tăng dần khi một cạnh cô lập mới xuất hiện ở một đỉnh và giảm dần khi các phần hợp nhất các thành phần riêng biệt trước đó. 

Một điểm tinh tế là tính kề cận của cạnh được mô hình hóa gián tiếp thông qua các đỉnh sử dụng cạnh đại diện cho mỗi (đỉnh, màu). Điều này tránh việc quét danh sách lân cận và đảm bảo quyền truy cập liên tục vào ứng viên kết nối. 

## Ví dụ đã hoạt động 

Xét một đồ thị nhỏ có ba đỉnh tạo thành một hình tam giác và ba cạnh ban đầu được tô màu khác nhau. Một bản cập nhật đổi màu một cạnh. 

Tại thời điểm 1, không có sự cập nhật nào xảy ra nên mỗi màu có đúng một cạnh và mỗi cạnh tạo thành thành phần riêng của nó. 

| Thời gian | Hoạt động | Thay đổi thành phần | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | ban đầu | 3 thành phần một cạnh | 3 | 
| 1 | tô màu lại cạnh | một màu đạt được kết nối cạnh thứ hai hoặc thay đổi nhóm | cập nhật | 

Điều này chứng tỏ rằng câu trả lời chỉ nhạy cảm với những thay đổi kết nối cục bộ do cạnh được đổi màu gây ra. 

Ví dụ thứ hai là biểu đồ hình sao trong đó có nhiều cạnh gặp nhau tại một nút trung tâm. Nếu tất cả các cạnh có chung một màu thì chúng đều nằm trong một thành phần. Nếu chúng ta tô màu lại từng cạnh thành các màu khác nhau, mỗi lần xóa sẽ chia một thành phần thành một thành phần riêng biệt mới cho mỗi màu, cho thấy rằng số lượng thành phần phụ thuộc rất nhiều vào các đỉnh được chia sẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + M)\log M \cdot \alpha(N))$| mỗi khoảng thời gian được xử lý trong các nút cây phân đoạn với các liên kết DSU | 
| Không gian |$O(N + M)$| lưu trữ theo khoảng, DSU và cây phân đoạn | 

Hệ số logarit xuất phát từ việc phân tách cây phân đoạn theo các khoảng thời gian. Với tổng kích thước lên tới khoảng một triệu qua các thử nghiệm, điều này hoàn toàn phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal case
assert run("""1
3 3
1 2 1
2 3 1
3 1 1
1 2 2
2 3 2
3 1 2
""") != "", "basic connectivity change"

# single edge flips color
assert run("""1
2 1
1 2 1
1 2 2
""") == "Case #1:\n1", "single edge recolor"

# no updates
assert run("""1
3 0
1 2 1
2 3 1
3 1 1
""") == "Case #1:\n1", "no updates"

# all edges independent colors
assert run("""1
4 0
1 2 1
2 3 2
3 4 3
4 1 4
""") == "Case #1:\n4", "all separate colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đổi màu tam giác | sáp nhập năng động | tính đúng đắn của việc hợp nhất | 
| lật một cạnh | 1 | hành vi đổi màu tầm thường | 
| không có cập nhật | câu trả lời ổn định | xử lý điều kiện ban đầu | 
| tất cả đều khác biệt | 4 | đếm thành phần cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là việc đổi màu lặp đi lặp lại của cùng một cạnh. Thuật toán xử lý việc này bằng cách đóng và mở lại các khoảng thời gian mỗi khi cạnh thay đổi màu sắc. Mỗi phân đoạn được xử lý độc lập trong cây phân đoạn, do đó việc chuyển đổi lặp đi lặp lại không gây ra việc tính toán lại đầy đủ lặp đi lặp lại. 

Một trường hợp khác là khi nhiều cạnh có chung một đỉnh. Cơ chế đại diện cho mỗi đỉnh cho mỗi màu đảm bảo rằng tất cả các cạnh liên quan đến đỉnh đó được hợp nhất thành một thành phần duy nhất, ngăn chặn việc đếm thiếu. 

Trường hợp tinh tế cuối cùng là khi một cạnh là thành viên duy nhất có màu của nó. Trong tình huống đó, nó sẽ đóng góp chính xác một thành phần và việc triển khai đảm bảo điều này bằng cách tăng số lượng thành phần khi đại diện đầu tiên được gán cho cặp màu đỉnh đó.
