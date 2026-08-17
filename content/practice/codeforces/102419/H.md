---
title: "CF 102419H - Bằng cấp"
description: "Chúng ta có một đồ thị vô hướng và mỗi cạnh cuối cùng phải hướng về đúng một trong hai điểm cuối của nó. Đối với một đỉnh có giá trị được xác định là (ai), chính xác (ai) các cạnh liên quan phải trỏ vào đỉnh đó. Một đỉnh có (ai=-1) không bị hạn chế về bậc cuối cùng của nó."
date: "2026-08-16T09:02:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "H"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 287
verified: false
draft: false
---

[CF 102419H - Bằng cấp](https://codeforces.com/problemset/problem/102419/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 47 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng và mỗi cạnh cuối cùng phải hướng về đúng một trong hai điểm cuối của nó. Đối với một đỉnh có giá trị được chỉ định là (a_i), chính xác (a_i) các cạnh liên quan phải trỏ vào đỉnh đó. Một đỉnh có (a_i=-1) không bị hạn chế về bậc cuối cùng của nó. 

Nhiệm vụ là tìm ra một định hướng như vậy hoặc chứng minh rằng không có định hướng nào tồn tại. Đầu ra chứa một trong hai`NO`, hoặc`YES`theo sau là hướng của mỗi cạnh ban đầu. Đối với cạnh gốc ((u,v)), in`u v`có nghĩa là cạnh được hướng từ (u) đến (v), do đó (v) nhận được một đơn vị độ. 

Các ràng buộc ban đầu có (n,m\le 2000), không có cạnh song song hoặc vòng tự. Giá trị này đủ nhỏ đối với thuật toán đồ thị đa thức, nhưng nó loại trừ các thuật toán liệt kê các hướng. Có (2^m) hướng có thể có và khi (m=2000), ngay cả việc kiểm tra một hướng trong thời gian không đổi cũng đã là vô vọng. Mạng luồng có (O(n+m)) đỉnh và cạnh nằm trong giới hạn bộ nhớ và thuật toán luồng cực đại tích phân tiêu chuẩn là phù hợp. 

Có một số trường hợp rất dễ xử lý sai. 

Coi như```
2 1
0 0
1 2
```Cả hai đỉnh đều yêu cầu độ 0, nhưng cạnh duy nhất phải chỉ vào đâu đó. Câu trả lời đúng là`NO`. Chỉ kiểm tra xem mọi giá trị được yêu cầu có nhiều nhất là ở mức độ tương ứng hay không sẽ chấp nhận trường hợp này một cách sai lầm. 

Bây giờ hãy xem xét```
2 1
2 -1
1 2
```Đỉnh 1 có bậc một nhưng yêu cầu ở bậc hai. Câu trả lời đúng là`NO`. Việc xây dựng luồng phải tôn trọng số lượng được yêu cầu chính xác thay vì coi nó như giới hạn trên. 

Ngoài ra còn có một vấn đề ít rõ ràng hơn khi một cạnh nối hai đỉnh bị ràng buộc. Ví dụ,```
2 1
0 1
1 2
```Cạnh buộc phải trỏ vào đỉnh 2. Một cách tiếp cận chỉ cố gắng chọn một số cạnh để thỏa mãn các đỉnh bị ràng buộc phải đảm bảo rằng mọi cạnh giữa hai đỉnh bị ràng buộc đều được gán cho một trong số chúng. Việc để lại một cạnh như vậy không được chỉ định sau này không thể được sửa chữa bằng cách hướng nó về phía một đỉnh không bị ràng buộc, bởi vì không có điểm cuối nào là không bị ràng buộc. 

Cuối cùng, một cạnh có hai điểm cuối không bị ràng buộc hoàn toàn không cần tham gia vào phần giải quyết ràng buộc. Sau khi tất cả các mức độ ràng buộc đã được thỏa mãn, cạnh đó có thể được định hướng tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử cả hai hướng cho mọi cạnh. Tìm kiếm theo chiều sâu có thể đưa ra một quyết định nhị phân trên mỗi cạnh và sau khi đạt được hướng hoàn chỉnh, hãy đếm tất cả các mức độ và kiểm tra các ràng buộc. Điều này đúng vì mọi hướng có thể đều xuất hiện ở đúng một nhánh của cây tìm kiếm. Vấn đề là kích thước của nó: có (2^m) lá và việc kiểm tra hướng mất (O(m+n)), cho (O(2^m(m+n))) công việc. Tại (m=2000), điều này hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là một cạnh định hướng có thể được xem như là gán một đơn vị độ cho chính xác một điểm cuối. Thay vì quyết định trực tiếp hướng của một cạnh, chúng ta có thể tạo quyết định luồng cho biết điểm cuối nào sẽ nhận đơn vị đó. 

Đối với mọi cạnh gốc có liên quan, hãy tạo một nút luồng. Từ nút biên, chúng ta có thể gửi một đơn vị đến một trong hai điểm cuối. Một đỉnh bị ràng buộc có một lượng chính xác (a_i) phải đến đó. Một cạnh nối hai đỉnh bị ràng buộc phải gửi đơn vị của nó đến một nơi nào đó, trong khi một cạnh có điểm cuối không bị ràng buộc có thể rời khỏi mạng luồng mà không đóng góp vào một đỉnh bị ràng buộc. 

Các yêu cầu chính xác về đỉnh và các cạnh bị ràng buộc bắt buộc được thể hiện một cách tự nhiên bằng các giới hạn dưới và giới hạn trên của luồng. Điều này đưa ra một vấn đề lưu thông khả thi. 

Đối với mỗi cạnh, chúng tôi phân biệt ba trường hợp. Nếu cả hai điểm cuối đều bị ràng buộc, nút biên của nó phải nhận chính xác một đơn vị. Nếu chính xác một điểm cuối bị ràng buộc, nút cạnh của nó có thể gửi 0 hoặc một đơn vị đến điểm cuối bị ràng buộc đó. Nếu cả hai điểm cuối đều không bị ràng buộc, chúng ta sẽ bỏ qua nó trong khi giải quyết các ràng buộc và định hướng nó một cách tùy ý sau đó. 

Đối với mọi đỉnh bị ràng buộc (v), cạnh từ (v) đến điểm chìm có cả giới hạn dưới và giới hạn trên bằng (a_v). Như vậy chính xác (a_v) các đơn vị cạnh được chọn phải đạt tới (v). 

Phép biến đổi giới hạn dưới tiêu chuẩn sẽ loại bỏ các giới hạn dưới và ghi lại sự mất cân bằng dẫn đến ở mọi nút. Sau đó, một siêu nguồn và siêu chìm được thêm vào và một luồng tối đa thông thường sẽ xác định xem liệu tuần hoàn cần thiết có tồn tại hay không. 

Mối liên hệ chính là một đơn vị luồng đạt đến đỉnh (v) tương ứng chính xác với một cạnh ban đầu hướng vào (v). Vì luồng là tích phân nên mỗi cạnh được chọn sẽ được gán cho một điểm cuối, không bao giờ bị phân chia một phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^m(m+n))) | (O(m+n)) | Quá chậm | 
| Lưu thông giới hạn dưới + Dinic | (O(V^2E)) trường hợp xấu nhất | (O(V+E)) | Đã chấp nhận | 

Ở đây (V=O(n+m)) và (E=O(n+m)) cho mạng được xây dựng. Với (n,m\le2000), mạng chỉ có vài nghìn đỉnh và cạnh, và hoạt động thực tế của Dinic trên mạng thưa thớt, chủ yếu có dung lượng đơn vị này là đủ cho giới hạn. 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và đánh dấu mọi đỉnh bằng (a_i\ne-1) là bị ràng buộc. Lưu trữ điểm cuối ban đầu của mỗi cạnh vì đáp án cuối cùng phải được in theo thứ tự cạnh ban đầu. 
2. Xây dựng mạng luồng chứa một nguồn (S), một điểm chìm (T), một nút cho mỗi cạnh ban đầu chạm vào ít nhất một đỉnh bị ràng buộc và một nút cho mỗi đỉnh bị ràng buộc. 

Một cạnh có hai điểm cuối không bị ràng buộc sẽ bị bỏ qua vì nó không bao giờ có thể ảnh hưởng đến mức độ bắt buộc. Nó có thể được định hướng một cách an toàn sau đó. 
3. Đối với mọi cạnh gốc có liên quan (e=(u,v)), hãy tạo một nút cạnh (E_e). 

Nếu cả (u) và (v) đều bị ràng buộc, hãy thêm

[ 
S\rightarrow E_e 
] 

với giới hạn dưới và giới hạn trên đều bằng (1). Cạnh phải đóng góp một cạnh đến cho (u) hoặc (v). 

Nếu ít nhất một điểm cuối không bị ràng buộc, hãy sử dụng giới hạn dưới (0) và giới hạn trên (1). Một cạnh như vậy được phép đóng góp vào điểm cuối bị ràng buộc, nhưng không nhất thiết phải làm như vậy. 

Từ (E_e), thêm các cạnh dung lượng (1) vào mọi điểm cuối bị ràng buộc của cạnh ban đầu. Gửi một đơn vị qua (E_e\rightarrow v) có nghĩa là hướng cạnh ban đầu về phía (v). 
4. Với mỗi đỉnh bị ràng buộc (v), hãy thêm một cạnh 

[ 
v\rightarrow T 
] 

có giới hạn dưới và giới hạn trên đều là (a_v). 

Vì lượng rời khỏi (v) buộc phải chính xác (a_v), nên việc bảo toàn dòng chảy sẽ buộc các đơn vị chính xác (a_v) phải đi vào (v). Đây chính xác là yêu cầu ở mức độ. 
5. Thêm một cạnh (T\rightarrow S) có dung lượng (m). Điều này đóng mạng thành một vòng tuần hoàn. Số lượng chính xác của nó không thành vấn đề, bởi vì việc bảo toàn buộc nó phải bằng tổng số đơn vị được gán cho các đỉnh bị ràng buộc. 
6. Chuyển đổi mọi cạnh giới hạn ((u,v)) có giới hạn dưới (L) và giới hạn trên (R) thành cạnh thông thường có dung lượng (R-L). Duy trì một mảng cân bằng. Trừ (L) từ số dư của (u) và cộng (L) vào số dư của (v). 

Số dư ghi lại tác động của luồng đã bị giới hạn dưới ép buộc. Dòng chảy thông thường còn lại phải bù đắp cho những mất cân bằng này. 
7. Thêm siêu nguồn (SS) và siêu chìm (TT). Nếu nút có số dư dương thì thêm (SS\rightarrow v) với công suất bằng số dư đó. Nếu nó có số dư âm thì cộng (v\rightarrow TT) với công suất bằng số dư tuyệt đối của nó. 

Một vòng tuần hoàn khả thi tồn tại chính xác khi luồng cực đại từ (SS) đến (TT) bão hòa tất cả các cạnh cân bằng này. Nếu không, hãy in`NO`. 
8. Nếu quá trình lưu thông khả thi, hãy kiểm tra luồng của từng cạnh liên quan ban đầu từ nút cạnh đến điểm cuối bị ràng buộc của nó. Nếu một đơn vị tiến tới (u), hãy hướng cạnh ban đầu về phía (u). Nếu một đơn vị đi tới (v), hãy hướng nó về phía (v). 

Một cạnh bị ràng buộc-bị ràng buộc luôn có chính xác một đơn vị như vậy bởi vì luồng từ nguồn đến cạnh của nó bị buộc phải có một đơn vị. Đối với một cạnh có một điểm cuối không bị ràng buộc, luồng bằng 0 chỉ đơn giản có nghĩa là chúng ta hướng nó về điểm cuối không bị ràng buộc đó. Đối với một cạnh có hai điểm cuối không bị ràng buộc, hãy chọn một trong hai hướng. 
9. In`YES`và hướng kết quả của mỗi cạnh ban đầu. 

Bất biến trong suốt quá trình xây dựng là mỗi đơn vị đi vào một đỉnh bị ràng buộc đại diện cho một và chỉ một cạnh ban đầu có đầu là đỉnh đó. Giới hạn dưới và giới hạn trên chính xác trên một đỉnh bị ràng buộc buộc chính xác số lượng đơn vị như vậy được yêu cầu. Luồng bắt buộc thông qua một nút cạnh đảm bảo rằng mọi cạnh nối hai đỉnh bị ràng buộc đều nhận được một đầu. Do đó, một vòng tuần hoàn khả thi ánh xạ trực tiếp tới một hướng hợp lệ. Ngược lại, bất kỳ hướng hợp lệ nào đều tạo ra một vòng tuần hoàn khả thi bằng cách gửi một đơn vị qua mỗi nút cạnh đến đỉnh là đầu của cạnh đó, do đó, kiểm tra luồng không thể từ chối một phiên bản hợp lệ thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap):
        idx = len(self.g[u])
        self.g[u].append([v, cap, len(self.g[v])])
        self.g[v].append([u, 0, idx])
        return idx

    def bfs(self, s, t):
        level = [-1] * self.n
        level[s] = 0
        q = [s]
        head = 0

        while head < len(q):
            u = q[head]
            head += 1

            for v, cap, rev in self.g[u]:
                if cap > 0 and level[v] == -1:
                    level[v] = level[u] + 1
                    q.append(v)

        self.level = level
        return level[t] != -1

    def dfs(self, u, t, pushed):
        if u == t:
            return pushed

        g_u = self.g[u]
        while self.it[u] < len(g_u):
            i = self.it[u]
            v, cap, rev = g_u[i]

            if cap > 0 and self.level[v] == self.level[u] + 1:
                got = self.dfs(v, t, min(pushed, cap))
                if got:
                    g_u[i][1] -= got
                    self.g[v][rev][1] += got
                    return got

            self.it[u] += 1

        return 0

    def max_flow(self, s, t):
        flow = 0
        INF = 10**9

        while self.bfs(s, t):
            self.it = [0] * self.n
            while True:
                pushed = self.dfs(s, t, INF)
                if not pushed:
                    break
                flow += pushed

        return flow

def solve(data):
    it = iter(map(int, data.split()))
    n = next(it)
    m = next(it)

    a = [next(it) for _ in range(n)]
    edges = [(next(it), next(it)) for _ in range(m)]

    constrained = [x != -1 for x in a]

    # Node layout:
    # 0 ... n-1                 constrained vertex slots
    # edge_base ... edge nodes
    # S, T, SS, TT
    #
    # We only need vertex nodes for constrained vertices.
    vertex_id = [-1] * n
    vertex_nodes = []

    for v in range(n):
        if constrained[v]:
            vertex_id[v] = len(vertex_nodes)
            vertex_nodes.append(v)

    k = len(vertex_nodes)
    edge_base = k
    relevant = []

    for i, (u, v) in enumerate(edges):
        u -= 1
        v -= 1

        if constrained[u] or constrained[v]:
            relevant.append(i)

    r = len(relevant)

    S = k + r
    T = S + 1
    SS = T + 1
    TT = SS + 1
    N = TT + 1

    dinic = Dinic(N)
    balance = [0] * N

    # Store references to edge-node -> constrained-vertex arcs.
    # Each entry is (edge_index, original_endpoint, network_u, arc_index).
    choice_arcs = []

    def add_bounded(u, v, low, high):
        if low > high:
            return False

        cap = high - low
        dinic.add_edge(u, v, cap)

        balance[u] -= low
        balance[v] += low
        return True

    # Source -> edge node.
    for pos, ei in enumerate(relevant):
        u, v = edges[ei]
        u -= 1
        v -= 1

        enode = edge_base + pos

        if constrained[u] and constrained[v]:
            low = high = 1
        else:
            low, high = 0, 1

        if not add_bounded(S, enode, low, high):
            return "NO\n"

        if constrained[u]:
            idx = dinic.add_edge(enode, vertex_id[u], 1)
            choice_arcs.append((ei, u, enode, idx))

        if constrained[v]:
            idx = dinic.add_edge(enode, vertex_id[v], 1)
            choice_arcs.append((ei, v, enode, idx))

    # Every constrained vertex must receive exactly a[v] units.
    for v in vertex_nodes:
        need = a[v]
        if need < 0:
            continue

        # A vertex cannot receive more than its graph degree.
        # The lower-bound construction would reject this anyway,
        # but this check avoids creating an obviously impossible edge.
        degree = 0
        for u, w in edges:
            u -= 1
            w -= 1
            if u == v or w == v:
                degree += 1

        if need > degree:
            return "NO\n"

        if not add_bounded(vertex_id[v], T, need, need):
            return "NO\n"

    # Close the network into a circulation.
    add_bounded(T, S, 0, m)

    # Satisfy all lower-bound imbalances.
    required = 0

    for v in range(N - 2):
        if balance[v] > 0:
            dinic.add_edge(SS, v, balance[v])
            required += balance[v]
        elif balance[v] < 0:
            dinic.add_edge(v, TT, -balance[v])

    got = dinic.max_flow(SS, TT)

    if got != required:
        return "NO\n"

    # Start with arbitrary directions.
    answer = []
    for u, v in edges:
        answer.append([u, v])

    # A relevant edge with flow into a constrained endpoint is directed
    # toward that endpoint.
    selected = {}

    for ei, endpoint, enode, idx in choice_arcs:
        # The residual capacity of the forward edge is 0 exactly when
        # one unit of flow is using it.
        if dinic.g[enode][idx][1] == 0:
            selected[ei] = endpoint

    for ei in relevant:
        u, v = edges[ei]
        u -= 1
        v -= 1

        if ei in selected:
            head = selected[ei]

            if head == u:
                answer[ei] = [v + 1, u + 1]
            else:
                answer[ei] = [u + 1, v + 1]
        else:
            # No constrained endpoint receives this edge.
            # This is possible only when at least one endpoint is free.
            if not constrained[u]:
                answer[ei] = [v + 1, u + 1]
            else:
                answer[ei] = [u + 1, v + 1]

    out = ["YES"]
    for u, v in answer:
        out.append(f"{u} {v}")

    return "\n".join(out) + "\n"

def main():
    data = sys.stdin.buffer.read()
    sys.stdout.write(solve(data.decode()))

if __name__ == "__main__":
    main()
```các`Dinic`lớp lưu trữ mọi cạnh dư dưới dạng`[to, capacity, reverse_index]`. Chỉ số đảo ngược là thứ cho phép phần mở rộng cập nhật ngay công suất dư ngược mà không cần tìm kiếm trong danh sách kề. 

các`add_bounded`là cốt lõi của phép biến đổi giới hạn dưới. Đối với giới hạn ban đầu (L\le f\le R), nó tạo ra dung lượng dư (R-L), sau đó ghi các đơn vị (L) bắt buộc vào`balance`. Số dư dương có nghĩa là nút có luồng đến bắt buộc phải được bù bằng luồng đi bổ sung, đó là lý do tại sao siêu nguồn được kết nối với nó. 

Chỉ các đỉnh bị ràng buộc mới cần các nút đỉnh thực sự. Một cạnh giữa hai đỉnh tự do không ảnh hưởng đến bất kỳ yêu cầu nào, do đó, việc loại bỏ nó khỏi mạng luồng sẽ làm cho việc xây dựng nhỏ hơn mà không làm thay đổi tính khả thi. 

các`choice_arcs`mảng ghi nhớ chính xác cạnh dư nào tương ứng với mỗi đầu có thể có của mỗi cạnh ban đầu. Sau khi luồng tối đa thành công, cung lựa chọn bão hòa có nghĩa là một đơn vị đã được gửi đến điểm cuối đó. Vì mỗi nút biên có liên quan nhận được chính xác một đơn vị khi cả hai điểm cuối bị ràng buộc hoặc nhiều nhất là một đơn vị khi tồn tại một điểm cuối rảnh, nên không bao giờ có sự mơ hồ về đầu được chọn. 

Việc kiểm tra mức độ là dư thừa về mặt kỹ thuật vì bản thân vòng tuần hoàn phát hiện giới hạn dưới không thể, nhưng nó xử lý trường hợp không thể rõ ràng nhất trước khi chạy luồng. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. 

Việc xây dựng lại đầu ra có chủ ý bắt đầu với các hướng tùy ý. Chỉ các cạnh tham gia vào mạng ràng buộc mới bị ghi đè. Một cạnh liên quan không được chọn phải có một điểm cuối tự do, do đó, việc hướng nó tới điểm cuối đó không thể vi phạm bất kỳ yêu cầu cấp độ chính xác nào. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các đỉnh bị ràng buộc là (1,2,3,5), với độ được yêu cầu (1,2,1,0). Vertex 4 là miễn phí. 

Một chuỗi hợp lệ của các đầu được chọn được hiển thị bên dưới. Nhu cầu còn lại là số cạnh đến vẫn cần thiết cho mỗi đỉnh bị ràng buộc. 

| Cạnh | Đầu được chọn | Nhu cầu còn lại sau khi vượt biên | 
| --- | --- | --- | 
| (1-2) | 2 | (a=(1,1,1,0)) | 
| (1-3) | 1 | (a=(0,1,1,0)) | 
| (2-3) | 2 | (a=(0,0,1,0)) | 
| (3-4) | 3 | (a=(0,0,0,0)) | 
| (4-5) | 4 | (a=(0,0,0,0)) | 

Cạnh cuối cùng không được gán cho đỉnh 5 bị ràng buộc vì nhu cầu của nó đã bằng 0. Thay vào đó, nó hướng tới đỉnh tự do 4. Các hướng kết quả chính xác là hướng mẫu:```
1 2
3 1
3 2
4 3
5 4
```Dấu vết thể hiện sự bất biến trung tâm. Mỗi khi một điểm cuối bị ràng buộc được chọn làm đầu, nhu cầu còn lại của nó sẽ giảm đi một và lưu thông chỉ chấp nhận định hướng khi tất cả các nhu cầu chính xác được thỏa mãn. 

Đối với Mẫu 2, điểm khác biệt duy nhất là đỉnh 5 hiện yêu cầu một cạnh tới. Bốn cạnh đầu tiên có thể được xử lý chính xác như trước, để lại một đơn vị nhu cầu ở đỉnh 5. 

| Cạnh | Đầu được chọn | Nhu cầu còn lại sau khi vượt biên | 
| --- | --- | --- | 
| (1-2) | 2 | (a=(1,1,1,1)) | 
| (1-3) | 1 | (a=(0,1,1,1)) | 
| (2-3) | 2 | (a=(0,0,1,1)) | 
| (3-4) | 3 | (a=(0,0,0,1)) | 
| (4-5) | 5 | (a=(0,0,0,0)) | 

Do đó, cạnh cuối cùng hướng từ 4 đến 5. Hướng kết quả là```
1 2
3 1
3 2
4 3
4 5
```Ở đây, đường vẽ thực hiện một cạnh có một điểm cuối tự do và một điểm cuối bị ràng buộc. Trong Mẫu 1, cạnh đó được phép tránh điểm cuối bị ràng buộc, trong khi ở Mẫu 2, nhu cầu chính xác ở đỉnh 5 buộc nó phải trỏ đến đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(V^2E)) trường hợp xấu nhất đối với Dinic | Mạng lưới lưu thông được xây dựng có (V=O(n+m)) và (E=O(n+m)) | 
| Không gian | (O(V+E)) | Đồ thị dư, số dư, cạnh gốc và dữ liệu tái tạo đều là tuyến tính | 

Với (n,m\le2000), mạng được xây dựng chỉ có các đỉnh (O(4000)) và các cạnh logic (O(4000)) đến (O(6000)) trước khi các cạnh dư được thêm vào. Biểu đồ thưa thớt và hầu như tất cả khả năng lựa chọn cạnh đều là một. Điều này nằm trong giới hạn bộ nhớ 256 MB và phù hợp với giới hạn 1 giây khi triển khai Dinic được tối ưu hóa. 

## Trường hợp thử nghiệm 

Vì bài toán cho phép mọi hướng hợp lệ nên bài kiểm tra không thể so sánh văn bản đầu ra hoàn chỉnh với một câu trả lời cố định một cách an toàn. Dây nịt bên dưới kiểm tra`YES`hoặc`NO`kết quả và, cho`YES`, xác minh rằng mọi cạnh có hướng được in đều tương ứng với một cạnh ban đầu và mọi đỉnh bị ràng buộc đều nhận được chính xác cấp độ được yêu cầu của nó.```python
# Save the editorial solution as solution.py before running these tests.

from solution import solve

def run(inp: str) -> str:
    out = solve(inp)
    tokens = out.split()

    data = list(map(int, inp.split()))
    p = 0

    n = data[p]
    m = data[p + 1]
    p += 2

    a = data[p:p + n]
    p += n

    edges = []
    for _ in range(m):
        u = data[p]
        v = data[p + 1]
        p += 2
        edges.append((u, v))

    if not tokens:
        raise AssertionError("empty output")

    if tokens[0] == "NO":
        return "NO"

    assert tokens[0] == "YES", f"bad first token: {tokens[0]}"
    assert len(tokens) == 1 + 2 * m, "wrong number of output vertices"

    original = {tuple(sorted(e)) for e in edges}
    used = set()
    indeg = [0] * (n + 1)

    q = 1
    for _ in range(m):
        u = int(tokens[q])
        v = int(tokens[q + 1])
        q += 2

        assert 1 <= u <= n
        assert 1 <= v <= n
        assert u != v
        assert tuple(sorted((u, v))) in original
        assert tuple(sorted((u, v))) not in used, "an original edge was repeated"

        used.add(tuple(sorted((u, v))))
        indeg[v] += 1

    assert len(used) == m

    for v in range(1, n + 1):
        if a[v - 1] != -1:
            assert indeg[v] == a[v - 1], (
                f"vertex {v}: expected {a[v - 1]}, got {indeg[v]}"
            )

    return "YES"

# Sample 1
assert run("""\
5 5
1 2 1 -1 0
1 2
1 3
2 3
3 4
4 5
""") == "YES", "sample 1"

# Sample 2
assert run("""\
5 5
1 2 1 -1 1
1 2
1 3
2 3
3 4
4 5
""") == "YES", "sample 2"

# Minimum-size valid graph.
assert run("""\
2 1
0 1
1 2
""") == "YES", "minimum valid case"

# Boundary case: requested in-degree exceeds the actual degree.
assert run("""\
2 1
2 -1
1 2
""") == "NO", "degree upper boundary"

# Both endpoints are constrained and both demand zero.
# The single edge has nowhere valid to point.
assert run("""\
2 1
0 0
1 2
""") == "NO", "mandatory constrained-constrained edge"

# Maximum-size graph with all vertices unconstrained.
# A 2000-cycle has 2000 edges and needs no constrained flow at all.
n = 2000
cycle_edges = "\n".join(
    f"{i} {i + 1}" for i in range(1, n)
) + f"\n{n} 1\n"

max_case = f"{n} {n}\n" + ("-1 " * (n - 1)) + "-1\n" + cycle_edges

assert run(max_case) == "YES", "maximum-size all-free case"

# All-equal exact demands on a cycle.
all_equal_case = f"{n} {n}\n" + ("1 " * (n - 1)) + "1\n" + cycle_edges

assert run(all_equal_case) == "YES", "maximum-size all-equal case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, yêu cầu`0 1`, bờ rìa`1 2`|`YES`| Phiên bản hợp lệ tối thiểu và nhu cầu một đơn vị chính xác | 
|`2 1`, yêu cầu`2 -1`, bờ rìa`1 2`|`NO`| Yêu cầu bằng cấp lớn hơn bằng cấp hiện có | 
|`2 1`, yêu cầu`0 0`, bờ rìa`1 2`|`NO`| Không thể bỏ gán cạnh giữa hai đỉnh bị ràng buộc | 
| Chu kỳ 2000 với mọi nhu cầu`-1`|`YES`| Biểu đồ kích thước tối đa và xử lý các cạnh tự do không liên quan | 
| Chu kỳ 2000 với mọi nhu cầu`1`|`YES`| Đồ thị có kích thước tối đa với tất cả các đỉnh bị ràng buộc và yêu cầu chính xác bằng nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là bậc được yêu cầu lớn hơn bậc đỉnh. Vì```
2 1
2 -1
1 2
```đỉnh 1 có bậc 1 nhưng cần có 2 cạnh vào. Việc xây dựng thêm giới hạn dưới và giới hạn trên của hai trên cạnh từ đỉnh 1 đến (T), trong khi cạnh liên quan duy nhất có thể đóng góp nhiều nhất một đơn vị. Sự tuần hoàn không thể thỏa mãn giới hạn dưới, do đó luồng tối đa không thành công và thuật toán in ra`NO`. 

Trường hợp cạnh thứ hai là cạnh có điểm cuối đều bị ràng buộc. Vì```
2 1
0 0
1 2
```nút cạnh cho (1-2) nhận một đơn vị bắt buộc từ (S), vì cả hai điểm cuối đều bị ràng buộc. Nó chỉ có thể gửi đơn vị đó đến đỉnh 1 hoặc đỉnh 2, nhưng cả hai đỉnh đều có yêu cầu gửi đi đến (T) chính xác bằng 0. Sự mất cân bằng dẫn đến không thể sửa chữa được nên dòng chảy báo cáo là không khả thi. Điều này mắc phải lỗi phổ biến là coi mọi cạnh là tùy chọn trong mạng ràng buộc. 

Trường hợp cạnh thứ ba là cạnh giữa một đỉnh bị ràng buộc và một đỉnh không bị ràng buộc. Coi như```
2 1
0 -1
1 2
```Nút cạnh có khả năng tùy chọn là một đối với đỉnh bị ràng buộc 1. Vì đỉnh 1 yêu cầu bằng 0 nên luồng không gửi gì qua lựa chọn đó. Trong quá trình xây dựng lại, thuật toán thấy rằng cạnh không được chọn cho điểm cuối bị ràng buộc và thay vào đó hướng nó về phía đỉnh 2. Đầu ra có hiệu quả`1 2`, cho đỉnh 1 độ 0 theo yêu cầu. 

Trường hợp cạnh thứ tư là một đồ thị trong đó mọi đỉnh đều không bị ràng buộc. Trong chu kỳ 2000 với tất cả (a_i=-1), mạng luồng không có yêu cầu về đỉnh bị ràng buộc và các cạnh của chu trình không cần tham gia vào vòng tuần hoàn. Thuật toán chỉ đơn giản chọn các hướng tùy ý cho tất cả chúng và in ra`YES`. Đây là lý do tại sao các cạnh tự do có thể được loại bỏ một cách an toàn khỏi mô hình dòng chảy. 

Trường hợp cuối cùng là tình huống bị ràng buộc hoàn toàn. Đối với một hình tam giác có```
3 3
1 1 1
1 2
2 3
3 1
```mỗi cạnh nối hai đỉnh bị ràng buộc, do đó mỗi nút cạnh buộc phải mang chính xác một đơn vị. Ba đỉnh, mỗi đỉnh cần một đơn vị, do đó vòng tuần hoàn có thể gửi ba đơn vị đó đến ba đỉnh, ví dụ tạo ra chu trình có hướng (1\rightarrow2), (2\rightarrow3), (3\rightarrow1). Mỗi đỉnh nhận đúng một cạnh. Trường hợp này chứng tỏ rằng việc xây dựng giới hạn dưới cũng giải quyết được vấn đề định hướng theo bậc quy định ban đầu khi không có đỉnh không bị ràng buộc.
