---
title: "CF 1416F - Hiển Thị"
description: "Hãy coi mỗi ô như một đỉnh của đồ thị có hướng. Mỗi đỉnh chọn chính xác một đỉnh liền kề làm cạnh đi ra của nó. Đồ thị trong đó mọi đỉnh đều có bậc bằng chính xác một đỉnh là đồ thị hàm số."
date: "2026-06-11T07:04:54+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1416
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 673 (Div. 1)"
rating: 3300
weight: 1416
solve_time_s: 137
verified: false
draft: false
---

[CF 1416F - Hiển thị](https://codeforces.com/problemset/problem/1416/F) 

**Đánh giá:** 3300 
**Tags:** luồng, khớp biểu đồ, tham lam, triển khai 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi ô như một đỉnh của đồ thị có hướng. Mỗi đỉnh chọn chính xác một đỉnh liền kề làm cạnh đi ra của nó. 

Đồ thị trong đó mọi đỉnh đều có bậc bằng chính xác một đỉnh là đồ thị hàm số. Mỗi thành phần kết nối chứa chính xác một chu trình có hướng và tất cả các đỉnh khác tạo thành cây có hướng chỉ vào chu trình đó. 

Đặt ma trận đã cho là$b$. Đối với một tế bào$u$,$b_u$là tổng chi phí của mỗi đỉnh có thể đạt được từ$u$. 

Chúng ta phải xây dựng lại: 

1. Chi phí dương cho mỗi tế bào. 
2. Một hướng hợp lệ cho mọi ô. 

sao cho việc tính toán lại tất cả các tổng có thể truy cập sẽ tạo ra chính xác ma trận đã cho. 

Tổng số ô trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, vì vậy bất cứ điều gì bậc hai về số ô ngay lập tức là không thể. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(N\sqrt N)$, Ở đâu$N=nm$. 

Quan sát tinh tế đầu tiên là những gì xảy ra dọc theo một cạnh có hướng$u\to v$. 

Nếu như$u$không nằm trên một chu kỳ thì$$b_u = \text{cost}_u + b_v,$$Vì thế$b_u>b_v$. 

Nếu như$u$Và$v$thuộc về cùng một chu kỳ, chúng có cùng một tập hợp có thể truy cập được, do đó$$b_u=b_v.$$Không có cạnh nào có thể đi từ giá trị nhỏ hơn đến giá trị lớn hơn. 

Một sai lầm phổ biến là cho rằng mọi thành phần kết nối có giá trị bằng nhau đều có thể đơn giản được chuyển thành một chu trình. Tính tích cực của chi phí rất quan trọng. Một chu trình gồm các đỉnh có giá trị bằng nhau phải có tổng chi phí chu trình bằng giá trị đó, vì vậy chúng ta cần một công trình luôn giữ mọi chi phí đều dương. 

Coi như```
4 4
```Hai ô liền kề có giá trị 4 có thể tạo thành 2 chu kỳ và nhận chi phí$1$Và$3$. Điều này luôn hợp lệ vì tất cả các giá trị đã cho ít nhất là 2. 

Một trường hợp dễ bỏ sót khác là mức tối thiểu cục bộ.```
5
```trong một$1\times1$lưới. 

Ô duy nhất không có hàng xóm. Không có cạnh đi ra nào là có thể, vì vậy câu trả lời là không thể. Đây chính xác là mẫu có đầu ra`NO`. 

Cạm bẫy thứ ba là một ô có các ô lân cận đều lớn hơn.```
7 8
9 10
```Giá trị 7 không thể trỏ tới bất kỳ đâu vì mọi cạnh đi ra đều phải hướng tới một giá trị$\le 7$. Một trường hợp như vậy là không thể. 

## Phương pháp tiếp cận 

Một ý tưởng xây dựng lại bằng vũ lực là đoán đồ thị hàm số và sau đó giải quyết chi phí. Biểu đồ có nhiều khả năng theo cấp số nhân, bởi vì mỗi ô có thể chọn một trong tối đa bốn ô lân cận. Ngay cả đối với vài chục tế bào thì điều này cũng trở nên vô vọng. 

Quan sát cấu trúc quan trọng là các giá trị ma trận đã cho chúng ta biết hầu hết mọi thứ. 

Đối với một tế bào$u$: 

Nếu tồn tại một ô lân cận có giá trị nhỏ hơn rất nhiều thì$u$có thể được tạo thành một đỉnh cây một cách an toàn. Chúng tôi chỉ đơn giản là trỏ nó tới một người hàng xóm như vậy$v$và chỉ định$$\text{cost}_u=b_u-b_v.$$Các ô khó khăn duy nhất là những ô không có hàng xóm nào nhỏ hơn**. 

Một ô như vậy không thể là một đỉnh của cây. Vì mỗi cạnh đi ra phải đi đến một giá trị không vượt quá giá trị của nó nên nó phải trỏ đến một cạnh có giá trị bằng nhau. Do đó nó phải thuộc về một chu kỳ gồm các ô có giá trị bằng nhau. Quan sát này xuất hiện trong các giải pháp tiêu chuẩn cho vấn đề. 

Biểu đồ lưới là lưỡng cực dưới màu bàn cờ. Mọi chu trình trong đồ thị hai bên đều có độ dài chẵn. Do đó, chỉ cần xây dựng 2 chu kỳ là đủ. Chúng ta chỉ cần ghép các ô có giá trị bằng nhau liền kề. 

Bây giờ vấn đề trở thành: 

Xây dựng sự so khớp giữa các ô liền kề có giá trị bằng nhau sao cho mọi ô bắt buộc (một ô không có ô lân cận nhỏ hơn) đều được khớp. 

Một số ô có thể khớp hoặc không khớp. Một số phải được kết hợp. 

Đây là vấn đề đối sánh hai bên với giới hạn dưới. Giải pháp cổ điển sử dụng tuần hoàn khả thi với giới hạn thấp hơn, chính xác như các giải pháp được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Luồng có giới hạn dưới + tái thiết |$O(N\sqrt N)$trong thực tế |$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận$b$. 
2. Đối với mỗi ô, hãy kiểm tra bốn ô lân cận của nó. 

Nếu không có hàng xóm nào có giá trị$\le b_u$, câu trả lời là không thể. 
3. Đánh dấu một ô là **linh hoạt** nếu nó có ít nhất một ô lân cận nhỏ hơn hoàn toàn. 

Các ô linh hoạt có thể trở thành các đỉnh cây bình thường. 
4. Các ô không có ô lân cận nhỏ hơn hoàn toàn là **ô chu trình bắt buộc**. 

Chúng phải thuộc về một chu trình có giá trị bằng nhau. 
5. Bàn cờ tô màu lưới. 

Xây dựng đồ thị hai bên có các đỉnh là các ô và các cạnh của nó nối các ô liền kề có giá trị bằng nhau. 
6. Xây dựng một thể hiện khớp giới hạn dưới. 

Một đỉnh bắt buộc phải được khớp chính xác một lần. 

Một đỉnh linh hoạt có thể được so khớp nhiều nhất một lần và cũng có thể vẫn chưa được so khớp. 
7. Giải quyết việc so khớp giới hạn dưới bằng cách sử dụng tuần hoàn khả thi. 

Nếu không có sự lưu hành khả thi, hãy in`NO`. 
8. Mỗi cạnh có giá trị bằng nhau phù hợp sẽ trở thành 2 chu kỳ. 

Giả sử các đỉnh trùng khớp là$u$Và$v$. 

Bộ:$$\text{cost}_u=1,\qquad \text{cost}_v=b_u-1.$$Từ$b_u=b_v\ge 2$, cả hai chi phí đều dương. 
9. Đối với mọi đỉnh linh hoạt chưa từng có$u$, chọn bất kỳ ô lân cận nào$v$với$b_v<b_u$. 

Trực tiếp$u\to v$và chỉ định$$\text{cost}_u=b_u-b_v.$$10. Xuất ra tất cả chi phí và phương hướng. 

### Tại sao nó hoạt động 

Đối với mỗi đỉnh linh hoạt chưa từng có, việc xây dựng mang lại$$b_u=\text{cost}_u+b_v.$$Do đó, tổng có thể truy cập được của nó là chính xác. 

Mỗi cặp trùng khớp tạo thành 2 chu kỳ gồm các đỉnh có giá trị bằng nhau. Tập hợp có thể truy cập của cả hai đỉnh chính xác là cặp đó. Tổng chi phí được giao của họ là$$1+(b_u-1)=b_u,$$vì vậy cả hai đỉnh đều đạt được giá trị cần thiết. 

Các đỉnh bắt buộc chính xác là các đỉnh không thể gắn được với một đỉnh lân cận nhỏ hơn. Công thức dòng chảy đảm bảo rằng mọi đỉnh như vậy đều khớp. Do đó, mọi đỉnh bắt buộc đều được đặt bên trong một số 2 chu trình. 

Mỗi đỉnh được khớp thành 2 chu trình hoặc được gắn với một giá trị nhỏ hơn. Do đó, mỗi đỉnh có chính xác một cạnh đi ra và đồ thị thu được là hợp lệ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10 ** 18

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, v, u, c):
        a = [u, c, None]
        b = [v, 0, a]
        a[2] = b
        self.g[v].append(a)
        self.g[u].append(b)
        return a

    def bfs(self, s, t):
        self.level = [-1] * self.n
        q = deque([s])
        self.level[s] = 0

        while q:
            v = q.popleft()
            for e in self.g[v]:
                if e[1] > 0 and self.level[e[0]] == -1:
                    self.level[e[0]] = self.level[v] + 1
                    q.append(e[0])

        return self.level[t] != -1

    def dfs(self, v, t, f):
        if v == t:
            return f

        g = self.g[v]
        while self.it[v] < len(g):
            e = g[self.it[v]]

            if e[1] > 0 and self.level[e[0]] == self.level[v] + 1:
                pushed = self.dfs(e[0], t, min(f, e[1]))
                if pushed:
                    e[1] -= pushed
                    e[2][1] += pushed
                    return pushed

            self.it[v] += 1

        return 0

    def maxflow(self, s, t):
        flow = 0

        while self.bfs(s, t):
            self.it = [0] * self.n

            while True:
                pushed = self.dfs(s, t, INF)
                if not pushed:
                    break
                flow += pushed

        return flow

def solve():
    t = int(input())
    out = []

    dirs = [(0, 1, 'R'), (1, 0, 'D'), (0, -1, 'L'), (-1, 0, 'U')]

    for _ in range(t):
        n, m = map(int, input().split())
        b = [list(map(int, input().split())) for _ in range(n)]

        N = n * m

        def id_of(r, c):
            return r * m + c

        flexible = [False] * N
        equal_edges = [[] for _ in range(N)]

        ok = True

        for r in range(n):
            for c in range(m):
                cur = b[r][c]

                has_le = False
                has_smaller = False

                for dr, dc, _ in dirs:
                    nr, nc = r + dr, c + dc

                    if 0 <= nr < n and 0 <= nc < m:
                        if b[nr][nc] <= cur:
                            has_le = True

                        if b[nr][nc] < cur:
                            has_smaller = True

                if not has_le:
                    ok = False

                flexible[id_of(r, c)] = has_smaller

        if not ok:
            out.append("NO")
            continue

        SS = N
        TT = N + 1
        S = N + 2
        T = N + 3

        dinic = Dinic(N + 4)

        demand = [0] * (N + 4)
        match_edges = []

        def add_lb(u, v, low, high):
            demand[u] -= low
            demand[v] += low
            return dinic.add_edge(u, v, high - low)

        for r in range(n):
            for c in range(m):
                v = id_of(r, c)

                if (r + c) & 1:
                    low = 0 if flexible[v] else 1
                    add_lb(SS, v, low, 1)

                    for dr, dc, _ in dirs:
                        nr, nc = r + dr, c + dc

                        if (
                            0 <= nr < n and 0 <= nc < m
                            and b[nr][nc] == b[r][c]
                        ):
                            u = id_of(nr, nc)
                            e = add_lb(v, u, 0, 1)
                            match_edges.append((v, u, e))
                else:
                    low = 0 if flexible[v] else 1
                    add_lb(v, TT, low, 1)

        add_lb(TT, SS, 0, INF)

        need = 0

        for i in range(N + 2):
            if demand[i] > 0:
                dinic.add_edge(S, i, demand[i])
                need += demand[i]
            elif demand[i] < 0:
                dinic.add_edge(i, T, -demand[i])

        flow = dinic.maxflow(S, T)

        if flow != need:
            out.append("NO")
            continue

        cost = [[0] * m for _ in range(n)]
        direction = [['?'] * m for _ in range(n)]

        matched = [False] * N

        for a, bto, e in match_edges:
            if e[1] == 0:
                matched[a] = matched[bto] = True

                ar, ac = divmod(a, m)
                br, bc = divmod(bto, m)

                cost[ar][ac] = 1
                cost[br][bc] = b[ar][ac] - 1

                for dr, dc, ch in dirs:
                    if ar + dr == br and ac + dc == bc:
                        direction[ar][ac] = ch
                        rev = {'L': 'R', 'R': 'L', 'U': 'D', 'D': 'U'}[ch]
                        direction[br][bc] = rev
                        break

        for r in range(n):
            for c in range(m):
                v = id_of(r, c)

                if matched[v]:
                    continue

                for dr, dc, ch in dirs:
                    nr, nc = r + dr, c + dc

                    if (
                        0 <= nr < n and 0 <= nc < m
                        and b[nr][nc] < b[r][c]
                    ):
                        cost[r][c] = b[r][c] - b[nr][nc]
                        direction[r][c] = ch
                        break

        out.append("YES")

        for row in cost:
            out.append(" ".join(map(str, row)))

        for row in direction:
            out.append("".join(row))

    sys.stdout.write("\n".join(out))

solve()
```Sau khi luồng tìm thấy sự phù hợp khả thi, mọi cạnh có giá trị bằng nhau bão hòa sẽ trở thành 2 chu kỳ. Việc triển khai sẽ phát hiện các cạnh đó bằng cách kiểm tra xem dung lượng dư có bằng 0 hay không. 

Việc xây dựng giới hạn dưới là phần tinh tế nhất. Các đỉnh bắt buộc nhận được giới hạn dưới 1, điều này buộc chúng phải tham gia vào việc so khớp. Các đỉnh linh hoạt nhận được giới hạn dưới 0 và có thể vẫn chưa khớp. 

Giai đoạn tái thiết rất đơn giản vì ma trận giá trị đã xác định chi phí cần thiết. Đối với cạnh cây$u\to v$, chi phí dương duy nhất có thể có là$b_u-b_v$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 6 7 8
5 5 4 4
5 7 4 4
```Các ô bắt buộc chính xác là các ô có giá trị bằng nhau và không có ô lân cận nào nhỏ hơn. 

Sự kết hợp khả thi có thể chọn: 

| Cặp | 
| --- | 
| (2,1) ↔ (2,2) | 
| (2,3) ↔ (2,4) | 

Tất cả các ô còn lại có hàng xóm nhỏ hơn và trở thành đỉnh cây. 

Đồ thị kết quả thỏa mãn mọi tổng yêu cầu. 

Ví dụ này thể hiện ý tưởng chính: chỉ có cực tiểu cục bộ trong số các vùng có giá trị bằng nhau mới cần khớp. 

### Ví dụ 2 

đầu vào:```
5
```| Tế bào | Có giá trị hàng xóm ≤ không? | 
| --- | --- | 
| ô duy nhất | Không | 

Điều kiện cần thiết không thành công ngay lập tức. 

Đầu ra:```
NO
```Điều này chứng tỏ trường hợp nhỏ nhất không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N\sqrt N)$| Đối sánh hai bên thông qua luồng trên biểu đồ với$O(N)$đỉnh và cạnh | 
| Không gian |$O(N)$| Lưới, biểu đồ luồng, dữ liệu tái thiết | 

Tổng số ô trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, vì vậy một$O(N\sqrt N)$giải pháp dựa trên dòng chảy thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm```
# The official checker accepts many valid outputs.
# For this problem, exact-output asserts are generally not suitable,
# because multiple different reconstructions may be correct.

# Sample 1
inp = """\
2
3 4
7 6 7 8
5 5 4 4
5 7 4 4
1 1
5
"""

# Expected:
# first test -> YES
# second test -> NO

# Minimum impossible case
inp = """\
1
1 1
2
"""

# Expected: NO

# Two equal adjacent cells
inp = """\
1
1 2
4 4
"""

# Expected: YES

# All equal 2x2 block
inp = """\
1
2 2
5 5
5 5
"""

# Expected: YES

# Strict gradient
inp = """\
1
2 2
10 9
8 7
"""

# Expected: YES
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$1\times1$| KHÔNG | Không có cạnh đi nào tồn tại | 
| Hai ô bằng nhau | CÓ | 2 chu kỳ hợp lệ nhỏ nhất | 
| Tất cả đều bình đẳng$2\times2$| CÓ | Khớp bên trong một thành phần có giá trị bằng nhau | 
| Độ dốc nghiêm ngặt | CÓ | Xây dựng cây thuần túy không có chu kỳ | 

## Vỏ cạnh 

Hãy xem xét một ô có các ô lân cận đều lớn hơn:```
1 2
3 4
```Giá trị 1 không có hàng xóm nào có giá trị$\le 1$. Thuật toán từ chối phiên bản trong lần quét đầu tiên. Bất kỳ đồ thị hàm số hợp lệ nào cũng sẽ yêu cầu một cạnh đi ra từ ô đó, điều này là không thể. 

Hãy xem xét một cao nguyên:```
4 4
```Không có ô nào có hàng xóm nhỏ hơn. Cả hai đều trở thành đỉnh bắt buộc. Việc so khớp chứa cạnh có giá trị bằng nhau duy nhất của chúng, tạo ra 2 chu kỳ hợp lệ. 

Hãy xem xét một tình huống hỗn hợp:```
7 6
5 5
```Hai ô có giá trị 5 là bắt buộc và phải khớp nhau. Các ô 7 và 6 đều có các ô lân cận nhỏ hơn nên chúng trở thành các đỉnh cây. Việc xây dựng lại kết hợp một 2 chu kỳ với một cây nhỏ, khớp chính xác với tổng số tiền có thể truy cập được yêu cầu.
