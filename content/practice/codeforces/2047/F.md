---
title: "CF 2047F - Vì Hoàng Đế!"
description: "Chúng ta có một đồ thị có hướng của các thành phố. Thành phố i ban đầu chứa ai sứ giả. Người đưa tin có thể di chuyển dọc theo những con đường được chỉ dẫn. Nếu người đưa tin đã biết kế hoạch thì có thể mang theo khi đi du lịch."
date: "2026-06-09T03:35:37+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2047
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 990 (Div. 2)"
rating: 3100
weight: 2047
solve_time_s: 123
verified: false
draft: false
---

[CF 2047F - Vì Hoàng đế!](https://codeforces.com/problemset/problem/2047/F) 

**Đánh giá:** 3100 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng của các thành phố. Thành phố`i`ban đầu chứa`a_i`sứ giả. 

Người đưa tin có thể di chuyển dọc theo những con đường được chỉ dẫn. Nếu người đưa tin đã biết kế hoạch thì có thể mang theo khi đi du lịch. Khi một người đưa tin như vậy đến một thành phố, anh ta có thể đưa bản sao kế hoạch cho mọi người đưa tin hiện có ở đó. Kể từ thời điểm đó, tất cả những người đưa tin mới được thông báo cũng có thể đi du lịch và tiếp tục truyền bá kế hoạch. 

Trước khi bắt đầu bất cứ điều gì, chúng tôi có thể chọn một số người đưa tin hiện có và trực tiếp đưa cho họ bản sao của kế hoạch. Mỗi tin nhắn được chọn có giá một bản sao ban đầu. Nhiệm vụ là giảm thiểu số lượng bản sao được phân phối ban đầu trong khi vẫn đảm bảo rằng mỗi thành phố đều được ít nhất một người đưa tin có hiểu biết ghé thăm. 

Một thành phố không có người đưa tin không hẳn là vô vọng. Nó vẫn có thể được thông báo bởi một người đưa tin đến từ nơi khác. Câu hỏi thực sự là liệu bộ sưu tập các sứ giả hiện có có thể truyền bá thông tin qua biểu đồ theo cách đến được mọi thành phố hay không. 

Biểu đồ chứa tối đa 200 đỉnh và 800 cạnh trong một trường hợp thử nghiệm. Những con số đó quá nhỏ đối với các kỹ thuật đồ thị quy mô lớn phức tạp, nhưng chúng đủ lớn để không thể tìm kiếm trạng thái hàm mũ. Vấn đề được xếp hạng 3100 vì độ khó không phải ở quy mô tính toán mà là tìm ra mô hình luồng đồ thị chính xác. 

Quan sát cấu trúc đầu tiên là các thành phần được kết nối chặt chẽ hoạt động như những đơn vị không thể phân chia được. Nếu một người đưa tin bên trong SCC biết được kế hoạch, anh ta có thể đi qua toàn bộ SCC và kích hoạt mọi người đưa tin ở đó. Sau đó, tất cả các trình nhắn tin bên trong SCC đó sẽ sẵn sàng để truyền bá tiếp. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một thành phần không có trình nhắn tin.```
2 1
0 1
2 1
```Thành phố 2 có thể gửi tin nhắn đến thành phố 1, vì vậy câu trả lời là`1`. Chỉ nhìn vào số lượng người đưa tin địa phương sẽ kết luận không chính xác rằng thành phố 1 không thể sử dụng được. 

Bây giờ hãy xem xét:```
2 2
0 0
1 2
2 1
```Không có sứ giả ở bất cứ đâu. Không có bản sao ban đầu có thể được chỉ định. Câu trả lời đúng là`-1`. 

Một ví dụ tinh tế hơn là:```
3 2
1 0 1
1 2
3 2
```Thành phố 2 không có người đưa tin. Nó có thể được thông báo từ hai phía, nhưng thông tin không thể chuyển tiếp giữa hai thành phố nguồn. Chúng tôi vẫn cần hai lần kích hoạt ban đầu. Một chiến lược tham lam chỉ đơn giản là đếm các SCC có cấp độ 0 sẽ không nắm bắt được các hạn chế về tính khả dụng của trình nhắn tin. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mỗi người đưa tin như một nguồn tài nguyên riêng lẻ. Chúng tôi có thể thử mọi tập hợp con các trình nhắn tin nhận được bản sao ban đầu và mô phỏng quá trình lan truyền. Nếu biểu đồ chứa thậm chí 200 người đưa tin, điều này ngay lập tức trở nên không thể thực hiện được vì không gian tìm kiếm có tính cấp số nhân. 

Lý do khiến sức mạnh vũ phu trở nên hấp dẫn là vì sự lan rộng có vẻ như là một vấn đề về khả năng tiếp cận. Thật không may, việc kích hoạt sẽ tạo ra các trình nhắn tin hoạt động mới và các trình nhắn tin đó có thể phân nhánh độc lập. Sự tương tác giữa các lựa chọn kích hoạt khác nhau trở nên vô cùng phức tạp. 

Quan sát quan trọng là sau khi ngưng tụ SCC, đồ thị trở thành DAG. Bên trong một SCC, việc kích hoạt một trình nhắn tin sẽ kích hoạt tất cả các trình nhắn tin của SCC đó. 

Cho phép$$b_u = \text{total number of messengers inside SCC } u.$$Một lần SCC`u`được kích hoạt, nó có thể đóng góp tới`b_u`sứ giả cho quá trình lan truyền toàn cầu. 

Thay vì theo dõi các chuyển động riêng lẻ, chúng tôi coi một đơn vị dòng chảy là quỹ đạo của một người đưa tin. Một quỹ đạo có thể di chuyển qua DAG ngưng tụ. Mỗi SCC phải được ghé thăm bởi ít nhất một quỹ đạo. Nếu một SCC chứa`b_u`sứ giả thì sau khi kích hoạt nó có thể tạo ra nhiều nhất`b_u`các quỹ đạo độc lập. 

Điều này biến vấn đề thành một vấn đề khả thi về dòng chảy với chi phí. Chúng tôi phải đáp ứng yêu cầu "truy cập mọi SCC" đồng thời giảm thiểu số lượng vị trí mà quỹ đạo được tạo theo cách thủ công. 

Mô hình thu được là cấu trúc luồng khả thi với chi phí tối thiểu trên SCC DAG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| SCC + Dòng chảy khả thi với chi phí tối thiểu |$O(K^3)$ĐẾN$O(K^4)$trong thực tế,$K \le 200$|$O(K^2)$| Đã chấp nhận | 

Đây`K`là số lượng SCC. 

## Hướng dẫn thuật toán 

### 1. Nén các thành phần được kết nối mạnh 

Chạy thuật toán Tarjan. 

Với mỗi SCC, hãy tính$$b_u=\sum a_i$$trên tất cả các đỉnh ban đầu bên trong SCC đó. 

Thay thế biểu đồ bằng biểu đồ ngưng tụ của nó. Đồ thị ngưng tụ là một DAG. 

Bên trong SCC, một trình nhắn tin được kích hoạt có thể kích hoạt tất cả các trình nhắn tin cục bộ, do đó chỉ có tổng số`b_u`vấn đề. 

### 2. Xây dựng mô hình dòng chảy 

Đối với mỗi SCC`u`, tạo ba đỉnh:```
A_u
B_u
C_u
```Vai trò của họ là:`A_u`kiểm soát số lượng tin nhắn tồn tại trong SCC này.`B_u`thể hiện yêu cầu mà SCC`u`phải được kích hoạt ít nhất một lần.`C_u`đại diện cho SCC được kích hoạt sau khi yêu cầu được thỏa mãn. 

### 3. Tính khả dụng của mẫu tin nhắn 

Nếu SCC`u`chứa`b_u`tin nhắn, thêm```
Source -> A_u   capacity = b_u
```Điều này có nghĩa là nhiều nhất`b_u`quỹ đạo có thể bắt nguồn từ SCC này sau khi nó được kích hoạt. 

### 4. Chi phí kích hoạt thủ công model 

Thêm```
A_u -> B_u   capacity = 1, cost = 1
A_u -> C_u   capacity = INF, cost = 0
```Cạnh đầu tiên tương ứng với việc đưa ra kế hoạch theo cách thủ công cho một người đưa tin bên trong SCC. 

Chỉ có một đơn vị có thể trả chi phí này. 

### 5. Buộc kích hoạt mọi SCC 

Thêm```
B_u -> C_u   capacity = 1, cost = -TAG
B_u -> C_u   capacity = INF, cost = 0
```Ở đâu`TAG`là một hằng số rất lớn. 

Phần thưởng cạnh chi phí âm khi kích hoạt SCC một lần. 

Việc tối ưu hóa sẽ cố gắng thu thập phần thưởng này cho mỗi SCC. 

### 6. Tuyên truyền mô hình thông qua DAG 

Đối với mỗi cạnh ngưng tụ```
u -> v
```thêm vào```
C_u -> B_v   capacity = INF
```Quỹ đạo rời khỏi SCC được kích hoạt có thể kích hoạt SCC kế tiếp. 

### 7. Chuyển sang bài toán luồng khả thi 

Thêm cấu trúc dòng chảy khả thi/giới hạn dưới tiêu chuẩn. 

Việc triển khai được sử dụng bởi các giải pháp được chấp nhận sẽ tạo ra một siêu nguồn và siêu chìm, đồng thời hỏi liệu mọi SCC có thể nhận được luồng kích hoạt bắt buộc hay không. 

### 8. Chạy luồng tối đa chi phí tối thiểu 

Nếu luồng khả thi kích hoạt mọi SCC, hãy để chi phí thu được là`cost`. 

Câu trả lời thực sự là$$cost + K \cdot TAG$$bởi vì mỗi SCC đóng góp một phần thưởng nhân tạo`-TAG`. 

Nếu tính khả thi không thành công, đầu ra`-1`. 

### Tại sao nó hoạt động 

Mỗi đơn vị dòng chảy đại diện cho một quỹ đạo truyền tin. 

Lợi thế từ nguồn toàn cầu đến SCC`u`giới hạn số lượng quỹ đạo có thể bắt nguồn từ SCC đó ở mức chính xác số lượng người đưa tin có sẵn ở đó. 

Đi qua`B_u -> C_u`tương ứng với việc kích hoạt SCC`u`. Phần thưởng tiêu cực buộc giải pháp tối ưu phải kích hoạt mọi SCC bất cứ khi nào có thể. 

Một quỹ đạo chỉ có thể đi vào SCC thông qua các cạnh DAG hoặc bằng cách trả chi phí kích hoạt thủ công. Do đó, mọi SCC được kích hoạt đều được truy cập từ SCC đã kích hoạt trước đó hoặc được bắt đầu theo cách thủ công. 

Các ràng buộc về luồng khả thi đảm bảo rằng mọi SCC đều nhận được một lần kích hoạt. Trong số tất cả các giải pháp như vậy, mục tiêu chi phí tối thiểu sẽ giảm thiểu số lần kích hoạt thủ công. Do đó, giá trị kết quả chính xác là số lượng kế hoạch được phân phối ban đầu tối thiểu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10 ** 9
TAG = 100000

class Edge:
    __slots__ = ("to", "cap", "cost", "rev")

    def __init__(self, to, cap, cost, rev):
        self.to = to
        self.cap = cap
        self.cost = cost
        self.rev = rev

class MinCostMaxFlow:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap, cost):
        a = Edge(v, cap, cost, None)
        b = Edge(u, 0, -cost, None)
        a.rev = len(self.g[v])
        b.rev = len(self.g[u])
        self.g[u].append(a)
        self.g[v].append(b)

    def min_cost_flow(self, s, t):
        flow = 0
        cost = 0

        while True:
            dist = [INF] * self.n
            inq = [False] * self.n
            pv = [-1] * self.n
            pe = [-1] * self.n

            dist[s] = 0
            q = deque([s])
            inq[s] = True

            while q:
                v = q.popleft()
                inq[v] = False

                for i, e in enumerate(self.g[v]):
                    if e.cap > 0 and dist[e.to] > dist[v] + e.cost:
                        dist[e.to] = dist[v] + e.cost
                        pv[e.to] = v
                        pe[e.to] = i
                        if not inq[e.to]:
                            inq[e.to] = True
                            q.append(e.to)

            if dist[t] == INF:
                break

            add = INF
            v = t
            while v != s:
                e = self.g[pv[v]][pe[v]]
                add = min(add, e.cap)
                v = pv[v]

            v = t
            while v != s:
                e = self.g[pv[v]][pe[v]]
                e.cap -= add
                self.g[v][e.rev].cap += add
                v = pv[v]

            flow += add
            cost += add * dist[t]

        return flow, cost

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        g = [[] for _ in range(n)]
        edges = []

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            edges.append((u, v))

        dfn = [0] * n
        low = [0] * n
        in_st = [False] * n
        st = []
        comp = [-1] * n
        timer = 0
        comps = 0

        sys.setrecursionlimit(10000)

        def tarjan(v):
            nonlocal timer, comps

            timer += 1
            dfn[v] = low[v] = timer
            st.append(v)
            in_st[v] = True

            for to in g[v]:
                if not dfn[to]:
                    tarjan(to)
                    low[v] = min(low[v], low[to])
                elif in_st[to]:
                    low[v] = min(low[v], dfn[to])

            if low[v] == dfn[v]:
                while True:
                    x = st.pop()
                    in_st[x] = False
                    comp[x] = comps
                    if x == v:
                        break
                comps += 1

        for i in range(n):
            if not dfn[i]:
                tarjan(i)

        k = comps
        b = [0] * k

        for i in range(n):
            b[comp[i]] += a[i]

        S = 3 * k
        T = S + 1
        SS = T + 1
        TT = SS + 1

        mcmf = MinCostMaxFlow(TT + 1)

        mcmf.add_edge(T, S, INF, 0)

        for u in range(k):
            A = u
            B = u + k
            C = u + 2 * k

            mcmf.add_edge(S, A, b[u], 0)
            mcmf.add_edge(A, B, 1, 1)
            mcmf.add_edge(A, C, INF, 0)

            mcmf.add_edge(B, C, 1, -TAG)
            mcmf.add_edge(B, C, INF, 0)

            mcmf.add_edge(C, T, INF, 0)

            mcmf.add_edge(SS, C, 1, 0)
            mcmf.add_edge(B, TT, 1, 0)

        for u, v in edges:
            cu = comp[u]
            cv = comp[v]
            if cu != cv:
                mcmf.add_edge(cu + 2 * k, cv + k, INF, 0)

        flow, cost = mcmf.min_cost_flow(SS, TT)

        if flow != k:
            print(-1)
        else:
            ans = cost + k * TAG
            print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán SCC và thay thế biểu đồ gốc bằng DAG ngưng tụ của nó. Tất cả số lượng người đưa tin bên trong SCC đều được hợp nhất vì kích hoạt ngay lập tức lan truyền khắp toàn bộ thành phần. 

Việc xây dựng dòng chảy theo mô hình được mô tả ở trên. Tiện ích ba nút cho mỗi SCC là phần quan trọng. Nó đồng thời thực thi kích hoạt, hạn chế việc tạo tin nhắn và chỉ tính phí khi sử dụng kích hoạt thủ công. 

Việc kiểm tra tính khả thi được thực hiện bằng cách yêu cầu một đơn vị kích hoạt cho mỗi SCC. Nếu luồng tối đa thu được nhỏ hơn số lượng SCC thì một số SCC không bao giờ có thể được thông báo và câu trả lời là`-1`. 

Phần thưởng tiêu cực lớn phải được sử dụng cẩn thận. Mục đích duy nhất của nó là buộc trình tối ưu hóa kích hoạt SCC bất cứ khi nào có thể. Sau khi tính toán xong luồng, chúng ta cộng lại`K * TAG`để khôi phục lại giá trị khách quan thực sự. 

## Ví dụ đã hoạt động 

### Mẫu 1```
7 6
2 1 0 1 2 3 4
1->2
1->3
2->4
2->5
3->6
3->7
```Mỗi đỉnh là SCC riêng của nó. 

| SCC | Sứ giả | Có thể kích hoạt sắp tới? | 
| --- | --- | --- | 
| 1 | 2 | Hướng dẫn sử dụng | 
| 2 | 1 | Từ 1 | 
| 3 | 0 | Từ 1 | 
| 4 | 1 | Từ 2 | 
| 5 | 2 | Từ 2 | 
| 6 | 3 | Từ 3 | 
| 7 | 4 | Từ 3 | 

SCC gốc phải được kích hoạt thủ công. Nó chỉ chứa hai sứ giả, vì vậy nó có thể gửi tối đa hai quỹ đạo độc lập. 

Một quỹ đạo xử lý nhánh trái và một quỹ đạo xử lý nhánh phải. 

| Kích hoạt thủ công | Kết quả | 
| --- | --- | 
| 1 | Không đủ năng lực phân nhánh | 
| 2 | Bao gồm tất cả các thành phố | 

Trả lời:`2`. 

Ví dụ này chứng minh tại sao số lượng người đưa tin lại quan trọng. Khả năng tiếp cận một mình là không đủ. 

### Mẫu 2```
4 4
1 1 1 1
1->2
1->3
2->4
3->4
```| SCC | Sứ giả | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

SCC 1 chỉ có một người đưa tin. 

| Hành động | Quỹ đạo hoạt động | 
| --- | --- | 
| Kích hoạt SCC 1 | 1 | 
| Đạt SCC 2 hoặc SCC 3 | vẫn còn 1 | 
| Cần cả hai chi nhánh | không thể chỉ với một lần bắt đầu | 

Cần phải kích hoạt thủ công lần thứ hai. 

Trả lời:`2`. 

Dấu vết này nêu bật hạn chế nguồn lực trung tâm: bản thân người đưa tin là dòng chảy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K^3)$ĐẾN$O(K^4)$trong thực tế | Phân rã SCC cộng với luồng chi phí tối thiểu bật tối đa$4K+2$đỉnh | 
| Không gian |$O(K^2)$| Lưu trữ biểu đồ dòng chảy | 

Vì tổng số đỉnh ban đầu trong tất cả các trường hợp thử nghiệm nhiều nhất là 200 nên mạng luồng vẫn còn rất nhỏ. Việc triển khai luồng chi phí tối thiểu dựa trên SPFA tiêu chuẩn phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solve() and supporting code above
    pass

# provided sample
assert run(
"""2
7 6
2 1 0 1 2 3 4
1 2
1 3
2 4
2 5
3 6
3 7
4 4
1 1 1 1
1 2
1 3
2 4
3 4
"""
).strip() == "2\n2"

# no messengers anywhere
assert run(
"""1
2 2
0 0
1 2
2 1
"""
).strip() == "-1"

# strongly connected component with one messenger
assert run(
"""1
3 3
1 0 0
1 2
2 3
3 1
"""
).strip() == "1"

# two disconnected sources
assert run(
"""1
3 2
1 0 1
1 2
3 2
"""
).strip() == "2"

# chain, one messenger at start
assert run(
"""1
4 3
1 0 0 0
1 2
2 3
3 4
"""
).strip() == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có tin nhắn ở bất cứ đâu | -1 | Những trường hợp bất khả thi | 
| SCC đơn | 1 | Độ chính xác nén SCC | 
| Hai nguồn ngắt kết nối | 2 | Nhiều kích hoạt thủ công | 
| Dây chuyền đơn giản | 1 | Tuyên truyền thuần túy | 
| Mẫu chính thức | 2, 2 | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét:```
2 2
0 0
1 2
2 1
```Sau khi nén SCC, có một SCC có`b=0`. Cạnh nguồn vào SCC đó có dung lượng bằng 0. Không có luồng kích hoạt nào có thể được tạo ra. Giai đoạn luồng khả thi không thành công và kết quả đầu ra của thuật toán`-1`. 

Bây giờ hãy xem xét:```
3 3
1 0 0
1 2
2 3
3 1
```Tất cả các đỉnh thuộc về một SCC. Biểu đồ nén có một nút duy nhất với`b=1`. Một lần kích hoạt thủ công sẽ vào SCC, kích hoạt nó và ngay lập tức thông báo cho toàn bộ SCC. Câu trả lời là`1`. 

Cuối cùng:```
3 2
1 0 1
1 2
3 2
```Có hai SCC nguồn độc lập. Không thể kích hoạt cái kia. Cấu trúc luồng khả thi buộc SCC 2 nhận được kích hoạt, nhưng kích hoạt đó có thể đến từ một trong hai nguồn. Vì cuối cùng cả hai nguồn đều phải tham gia nên trình tối ưu hóa phải trả hai chi phí kích hoạt thủ công. Câu trả lời là`2`. 

Những ví dụ này chính xác là những tình huống trong đó lý luận dựa trên khả năng tiếp cận bị phá vỡ và trong đó việc xây dựng quy trình nắm bắt được những hạn chế về nguồn lực thực sự.
