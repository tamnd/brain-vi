---
title: "CF 103E - Bộ Mua"
description: "Chúng ta được cấp một số bộ số nguyên, mỗi bộ có một chi phí liên quan. Chúng tôi có thể chọn bất kỳ bộ sưu tập nào trong số các bộ này, kể cả bộ sưu tập trống. Đặt số tập hợp được chọn là $k$, và để hợp của tất cả các tập hợp được chọn chứa $u$ số nguyên riêng biệt."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings"]
categories: ["algorithms"]
codeforces_contest: 103
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 80 (Div. 1 Only)"
rating: 2900
weight: 103
solve_time_s: 190
verified: false
draft: false
---

[CF 103E - Bộ mua](https://codeforces.com/problemset/problem/103/E) 

**Xếp hạng:** 2900 
**Thẻ:** luồng, khớp biểu đồ 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số bộ số nguyên, mỗi bộ có một chi phí liên quan. Chúng tôi có thể chọn bất kỳ bộ sưu tập nào trong số các bộ này, kể cả bộ sưu tập trống. Gọi số tập hợp được chọn là$k$và để hợp của tất cả các tập hợp được chọn chứa$u$số nguyên phân biệt. 

Mục tiêu là giảm thiểu tổng chi phí trong điều kiện$u = k$. 

Phần bất thường của tuyên bố là sự đảm bảo về họ các tập hợp. Đối với mỗi bộ sưu tập của$k$các tập hợp thì hợp của chúng luôn chứa ít nhất$k$số riêng biệt. Đây chính xác là tình trạng của Hall. Điều đó có nghĩa là mọi tập hợp được chọn đều có thể được gán một phần tử đại diện riêng biệt từ bên trong tập hợp đó. 

Kích thước đầu vào đủ nhỏ để cho phép thuật toán đồ thị bậc ba, nhưng quá lớn để liệt kê tập hợp con. Từ$n \le 300$, vũ lực trên tất cả$2^n$bộ sưu tập là vô vọng. Thậm chí$2^{40}$sẽ quá lớn, và ở đây chúng ta có thể có$2^{300}$tập hợp con. 

Chi phí có thể âm, điều này làm thay đổi bản chất của việc tối ưu hóa. Một chiến lược tham lam ngây thơ như “lấy tất cả các tập hợp âm” có thể thất bại vì việc thêm một tập hợp có thể buộc quy mô hợp phải tăng lên, phá vỡ điều kiện bằng. 

Trường hợp cạnh tinh tế là bộ sưu tập trống. Nếu tất cả chi phí đều dương thì không mua gì là hợp lệ vì cả số lượng tập hợp được chọn và kích thước hợp đều bằng 0. 

Ví dụ:```
2
1 1
1 2
5 7
```Câu trả lời đúng là:```
0
```Một giải pháp bất cẩn nhất quyết chọn ít nhất một bộ sẽ xuất ra sai`5`. 

Một trường hợp khó khăn khác xuất hiện khi nhiều bộ chồng chéo lên nhau.```
3
2 1 2
2 1 2
1 2
-5 -4 100
```Câu trả lời đúng là:```
-9
```Hai bộ đầu tiên đều có thể được chọn vì tập hợp có kích thước 2 và có 2 bộ được chọn. Không nên thêm bộ thứ ba vì khi đó chúng ta sẽ có 3 bộ nhưng vẫn chỉ có 2 số phân biệt. 

Một sai lầm phổ biến là cho rằng điều kiện có nghĩa là các tập hợp được chọn phải rời rạc theo từng cặp. Điều đó là sai. Điều kiện chỉ hạn chế tổng kích thước liên kết. 

Một trường hợp nguy hiểm khác là khi sự bình đẳng đã tự động được duy trì.```
2
1 1
1 2
-3 -4
```Câu trả lời đúng là:```
-7
```Mỗi bộ đóng góp một số lượng riêng biệt, vì vậy lấy cả hai là tối ưu. 

Hiểu được khi nào sự bình đẳng xảy ra là toàn bộ chìa khóa của vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Liệt kê mọi tập hợp con của các tập hợp, tính kích thước của tập hợp và giữ tổng chi phí tối thiểu giữa các tập hợp con trong đó:$$|\text{chosen sets}| = |\text{union}|$$Điều kiện từ tuyên bố đảm bảo:$$|\text{union}| \ge |\text{chosen sets}|$$cho mỗi tập hợp con. Vì vậy, các tập hợp con khả thi chính xác là những tập hợp có sự bình đẳng. 

Phương pháp bạo lực này là chính xác vì nó kiểm tra mọi bộ sưu tập có thể một cách rõ ràng. Vấn đề là sự phức tạp. có$2^n$tập hợp con và$n$có thể là 300. Ngay cả việc lưu trữ tất cả các tập hợp con cũng không thể. 

Nhận xét quan trọng đến từ định lý Hall. 

Tuyên bố đảm bảo cho biết mọi tập hợp các tập hợp đều có ít nhất nhiều phần tử riêng biệt như tập hợp. Định lý Hall sau đó cho chúng ta biết rằng mọi tập hợp đều thừa nhận sự kết hợp hoàn hảo giữa các tập hợp được chọn và các phần tử riêng biệt. 

Bây giờ hãy xem xét khi sự bình đẳng giữ:$$|\text{union}| = |\text{chosen sets}|$$Giả sử chúng ta nhìn vào biểu đồ lưỡng cực giữa các tập hợp được chọn và các phần tử xuất hiện trong đó. Hall đảm bảo sự phù hợp bao gồm tất cả các bộ đã chọn. Vì phép hợp chứa chính xác số phần tử bằng tập hợp nên việc so khớp thực sự phải sử dụng mọi phần tử trong phép hợp. 

Điều này có nghĩa là mọi phần tử được chọn đều được khớp chính xác một lần. 

Điều đó ngay lập tức ngụ ý rằng đồ thị con được chọn tạo thành một cấu trúc thành phần cân bằng. Trong ngôn ngữ matroid, các tập hợp khả thi chính xác là các tập chặt chẽ của một matroid ngang. 

Việc tối ưu hóa sau đó có thể được chuyển thành bài toán đóng trọng số tối thiểu, giúp giảm bớt tính toán cắt nhỏ nhất. 

Thực tế cấu trúc quan trọng là thế này: 

Một tập hợp con của các tập hợp thỏa mãn đẳng thức khi và chỉ khi nó là tập hợp của các thành phần liên thông mạnh trong biểu đồ phụ thuộc có hướng dẫn xuất từ các đường xen kẽ trong một so khớp. 

Trước tiên, chúng tôi xây dựng một kết hợp hoàn hảo từ các bộ đến các phần tử riêng biệt. Sau đó, chúng tôi định hướng các cạnh: 

Từ một tập hợp đến tất cả các phần tử chưa từng có bên trong nó. 

Từ phần tử đã khớp trở lại tập hợp khớp của nó. 

Điều này tạo ra một đồ thị có hướng. Các tập hợp con chặt chẽ tương ứng chính xác với các tập hợp con đỉnh được đóng trong khả năng tiếp cận. 

Việc tìm tập hợp con đóng có chi phí tối thiểu là một cách giảm cổ điển đối với việc cắt giảm tối thiểu. 

Lực lượng vũ phu hoạt động vì điều kiện chỉ phụ thuộc vào tập hợp con và hợp, nhưng không thành công vì có nhiều tập hợp con theo cấp số nhân. Cấu trúc Hall cho phép chúng ta diễn giải lại các tập hợp con khả thi dưới dạng các tập hợp đóng trong biểu đồ có hướng, biến việc tìm kiếm hàm mũ thành tính toán luồng thời gian đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ lưỡng cực giữa tập hợp và số. 

Phía bên trái chứa$n$bộ. Phía bên phải chứa số$1 \ldots n$. Có một cạnh từ tập hợp$i$đánh số$x$nếu như$x$thuộc về tập hợp. 
2. Tính toán một kết hợp hoàn hảo bao gồm tất cả các bộ. 

Điều kiện của Hall đảm bảo sự trùng khớp như vậy tồn tại. Chúng ta có thể sử dụng thuật toán Kuhn vì$n \le 300$. 
3. Xây dựng đồ thị có hướng. 

Đối với mỗi bộ$i$:

Nếu như$i$chứa số$x$Và$x$không phù hợp với$i$, thêm một cạnh có hướng:$$i \to \text{owner}(x)$$Ở đâu`owner(x)`là tập hợp phù hợp với$x$. 

Cạnh này thể hiện sự phụ thuộc vào đường dẫn xen kẽ. Nếu chúng ta bao gồm bộ$i$, chúng tôi buộc phải bao gồm chủ sở hữu của$x$để bảo quản độ kín. 
4. Tính toán các thành phần liên thông mạnh. 

Bên trong một SCC, mọi đỉnh đều phụ thuộc vào mọi đỉnh khác. Hoặc là chúng tôi lấy toàn bộ SCC hoặc không lấy gì cả. 
5. Nén biểu đồ SCC thành DAG. 

Mỗi SCC trở thành một nút có trọng số bằng tổng chi phí của các bộ của nó. 
6. Tìm tập con đóng có trọng số nhỏ nhất. 

Một tập con đóng có nghĩa là: 

Nếu một nút được chọn thì tất cả các nút lân cận đi ra cũng phải được chọn. 

Điều này được giải quyết bằng cách sử dụng mức cắt tối thiểu. 
7. Xây dựng mạng lưới dòng chảy. 

Đối với mọi SCC có tổng trọng số âm, hãy kết nối:$$S \to \text{SCC}$$có công suất bằng$-w$. 

Với mỗi SCC có tổng trọng lượng dương, hãy kết nối:$$\text{SCC} \to T$$có công suất bằng$w$. 

Đối với mọi cạnh DAG:$$u \to v$$thêm công suất vô hạn. 
8. Chạy lưu lượng tối đa/cắt tối thiểu. 

Trọng số tập con đóng tối thiểu bằng:$$\text{maxflow} - \sum_{\text{negative } w} |w|$$9. So sánh với số 0. 

Bộ sưu tập trống luôn hợp lệ, vì vậy câu trả lời không thể vượt quá 0. 

### Tại sao nó hoạt động 

Việc khớp sẽ chuyển đổi điều kiện Hall thành biểu đồ phụ thuộc cấu trúc. Các tập hợp con chặt chẽ chính xác là những tập hợp mà mọi phần phụ thuộc có thể tiếp cận cũng được bao gồm. Đó chính xác là định nghĩa của tập đóng trong đồ thị có hướng. 

Các thành phần được kết nối chặt chẽ đại diện cho các nhóm không thể phân chia được vì mọi đỉnh bên trong SCC đều tác động lên mọi đỉnh khác. 

Mức giảm tối thiểu để đóng trọng lượng tối thiểu là tiêu chuẩn. Các cạnh có dung lượng vô hạn cấm vi phạm các ràng buộc đóng. Việc cắt chọn những SCC nào vẫn có thể truy cập được từ nguồn và dung lượng chính xác bằng với hình phạt đối với việc loại trừ SCC âm hoặc bao gồm SCC dương. 

Bởi vì mọi tập hợp khả thi đều tương ứng với một tập hợp con đóng và ngược lại, việc giảm thiểu trọng số đóng sẽ mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10**18

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.g[u].append([v, c, len(self.g[v])])
        self.g[v].append([u, 0, len(self.g[u]) - 1])

    def bfs(self, s, t):
        self.level = [-1] * self.n
        q = deque([s])
        self.level[s] = 0

        while q:
            u = q.popleft()
            for v, c, rev in self.g[u]:
                if c > 0 and self.level[v] == -1:
                    self.level[v] = self.level[u] + 1
                    q.append(v)

        return self.level[t] != -1

    def dfs(self, u, t, f):
        if u == t:
            return f

        for i in range(self.ptr[u], len(self.g[u])):
            self.ptr[u] = i

            v, c, rev = self.g[u][i]

            if c > 0 and self.level[v] == self.level[u] + 1:
                pushed = self.dfs(v, t, min(f, c))

                if pushed:
                    self.g[u][i][1] -= pushed
                    self.g[v][rev][1] += pushed
                    return pushed

        return 0

    def maxflow(self, s, t):
        flow = 0

        while self.bfs(s, t):
            self.ptr = [0] * self.n

            while True:
                pushed = self.dfs(s, t, INF)
                if not pushed:
                    break
                flow += pushed

        return flow

def solve():
    n = int(input())

    sets = []
    for _ in range(n):
        arr = list(map(int, input().split()))
        sets.append(arr[1:])

    cost = list(map(int, input().split()))

    match_num = [-1] * (n + 1)

    def kuhn(u, vis):
        if vis[u]:
            return False

        vis[u] = True

        for x in sets[u]:
            if match_num[x] == -1 or kuhn(match_num[x], vis):
                match_num[x] = u
                return True

        return False

    for i in range(n):
        vis = [False] * n
        kuhn(i, vis)

    owner = [-1] * (n + 1)

    for x in range(1, n + 1):
        if match_num[x] != -1:
            owner[x] = match_num[x]

    g = [[] for _ in range(n)]

    for i in range(n):
        matched_x = -1

        for x in sets[i]:
            if owner[x] == i:
                matched_x = x

        for x in sets[i]:
            if x != matched_x:
                g[i].append(owner[x])

    # Tarjan SCC
    sys.setrecursionlimit(10**6)

    tin = [-1] * n
    low = [0] * n
    stack = []
    in_stack = [False] * n

    comp = [-1] * n
    timer = 0
    comp_cnt = 0

    def dfs(u):
        nonlocal timer, comp_cnt

        tin[u] = low[u] = timer
        timer += 1

        stack.append(u)
        in_stack[u] = True

        for v in g[u]:
            if tin[v] == -1:
                dfs(v)
                low[u] = min(low[u], low[v])
            elif in_stack[v]:
                low[u] = min(low[u], tin[v])

        if low[u] == tin[u]:
            while True:
                x = stack.pop()
                in_stack[x] = False
                comp[x] = comp_cnt

                if x == u:
                    break

            comp_cnt += 1

    for i in range(n):
        if tin[i] == -1:
            dfs(i)

    comp_weight = [0] * comp_cnt

    for i in range(n):
        comp_weight[comp[i]] += cost[i]

    dag = set()

    for u in range(n):
        for v in g[u]:
            cu = comp[u]
            cv = comp[v]

            if cu != cv:
                dag.add((cu, cv))

    S = comp_cnt
    T = comp_cnt + 1

    dinic = Dinic(comp_cnt + 2)

    neg_sum = 0

    for i in range(comp_cnt):
        w = comp_weight[i]

        if w < 0:
            dinic.add_edge(S, i, -w)
            neg_sum += -w
        else:
            dinic.add_edge(i, T, w)

    for u, v in dag:
        dinic.add_edge(u, v, INF)

    flow = dinic.maxflow(S, T)

    ans = flow - neg_sum
    ans = min(ans, 0)

    print(ans)

solve()
```Phần đầu tiên tính toán sự khớp từ số đến tập hợp. Điều kiện Hall đảm bảo rằng mọi tập hợp đều có thể thu được một đại diện riêng biệt nên thuật toán Kuhn luôn thành công. 

Việc xây dựng đồ thị có hướng là phần tinh tế. Mỗi cạnh chưa được so sánh sẽ tạo ra một cạnh phụ thuộc. Nếu được đặt`i`cũng có thể sử dụng một số thuộc sở hữu của một bộ khác, sau đó bao gồm`i`không có chủ đó sẽ vi phạm chặt chẽ. 

Việc nén SCC là cần thiết vì sự phụ thuộc có thể tạo thành chu kỳ. Trong một chu trình, mỗi tập hợp sẽ tác động lên các tập hợp khác nên chúng phải được chọn cùng nhau. 

Cấu trúc min-cut mã hóa quy tắc đóng. Các cạnh vô hạn ngăn chặn các vết cắt vi phạm sự phụ thuộc. Trọng số SCC âm mang lại lợi nhuận và được kết nối từ nguồn. Trọng lượng SCC dương là hình phạt và được kết nối với bồn rửa. 

Công thức cuối cùng:```
flow - neg_sum
```là sự chuyển đổi tiêu chuẩn từ giá trị cắt tối thiểu trở lại trọng lượng đóng cửa tối thiểu. 

Một chi tiết triển khai tinh tế đang sử dụng đủ lớn`INF`. Nó chỉ cần vượt quá bất kỳ câu trả lời tuyệt đối có thể có. Vì chi phí bị giới hạn bởi$10^6$và có nhiều nhất là 300 bộ,$10^{18}$là hoàn toàn an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1
2 2 3
1 3
10 20 -3
```Một kết quả phù hợp có thể là: 

| Đặt | Số trùng khớp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 

Biểu đồ phụ thuộc: 

| Đặt | Số bổ sung | Cạnh | 
| --- | --- | --- | 
| 0 | không | không | 
| 1 | 3 | 1 → 2 | 
| 2 | không | không | 

SCC: 

| SCC | Bộ | Cân nặng | 
| --- | --- | --- | 
| A | {0} | 10 | 
| B | {1} | 20 | 
| C | {2} | -3 | 

SCC âm tính duy nhất là`{2}`và nó không có sự phụ thuộc đi ra nào, vì vậy chúng tôi chọn nó một mình. 

Kết quả:```
-3
```Ví dụ này cho thấy rằng một tập hợp đơn lẻ đã có thể thỏa mãn điều kiện bằng. Bộ`{3}`có một phần tử và đóng góp một tập hợp. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
2 1 2
2 1 2
1 2
-5 -4 100
```Giả sử sự phù hợp là: 

| Đặt | Số trùng khớp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 

Phụ thuộc: 

| Đặt | Số bổ sung | Cạnh | 
| --- | --- | --- | 
| 0 | 2 | 0 → 1 | 
| 1 | 1 | 1 → 0 | 
| 2 | 2 | 2 → 1 | 

SCC: 

| SCC | Bộ | Cân nặng | 
| --- | --- | --- | 
| A | {0,1} | -9 | 
| B | {2} | 100 | 

Vì tập 0 và tập 1 tạo thành một chu trình nên chúng phải được thực hiện cùng nhau. 

Câu trả lời tối ưu:```
-9
```Điều này chứng tỏ tại sao nén SCC là cần thiết. Chỉ lấy một trong hai bộ đầu tiên sẽ vi phạm việc kết thúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$| So khớp, tính toán SCC và luồng tối đa đều phù hợp với độ phức tạp bậc ba | 
| Không gian |$O(n^2)$| Đồ thị và mạng lưu trữ ở hầu hết các cạnh bậc hai | 

Với$n \le 300$, thuật toán bậc ba hoàn toàn thực tế. Ngay cả các đồ thị dày đặc vẫn thoải mái nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    from collections import deque

    input = sys.stdin.readline
    INF = 10**18

    class Dinic:
        def __init__(self, n):
            self.n = n
            self.g = [[] for _ in range(n)]

        def add_edge(self, u, v, c):
            self.g[u].append([v, c, len(self.g[v])])
            self.g[v].append([u, 0, len(self.g[u]) - 1])

        def bfs(self, s, t):
            self.level = [-1] * self.n
            q = deque([s])
            self.level[s] = 0

            while q:
                u = q.popleft()

                for v, c, rev in self.g[u]:
                    if c > 0 and self.level[v] == -1:
                        self.level[v] = self.level[u] + 1
                        q.append(v)

            return self.level[t] != -1

        def dfs(self, u, t, f):
            if u == t:
                return f

            for i in range(self.ptr[u], len(self.g[u])):
                self.ptr[u] = i

                v, c, rev = self.g[u][i]

                if c > 0 and self.level[v] == self.level[u] + 1:
                    pushed = self.dfs(v, t, min(f, c))

                    if pushed:
                        self.g[u][i][1] -= pushed
                        self.g[v][rev][1] += pushed
                        return pushed

            return 0

        def maxflow(self, s, t):
            flow = 0

            while self.bfs(s, t):
                self.ptr = [0] * self.n

                while True:
                    pushed = self.dfs(s, t, INF)

                    if not pushed:
                        break

                    flow += pushed

            return flow

    n = int(input())

    sets = []

    for _ in range(n):
        arr = list(map(int, input().split()))
        sets.append(arr[1:])

    cost = list(map(int, input().split()))

    match_num = [-1] * (n + 1)

    def kuhn(u, vis):
        if vis[u]:
            return False

        vis[u] = True

        for x in sets[u]:
            if match_num[x] == -1 or kuhn(match_num[x], vis):
                match_num[x] = u
                return True

        return False

    for i in range(n):
        vis = [False] * n
        kuhn(i, vis)

    owner = [-1] * (n + 1)

    for x in range(1, n + 1):
        if match_num[x] != -1:
            owner[x] = match_num[x]

    g = [[] for _ in range(n)]

    for i in range(n):
        matched_x = -1

        for x in sets[i]:
            if owner[x] == i:
                matched_x = x

        for x in sets[i]:
            if x != matched_x:
                g[i].append(owner[x])

    sys.setrecursionlimit(10**6)

    tin = [-1] * n
    low = [0] * n
    stack = []
    in_stack = [False] * n

    comp = [-1] * n
    timer = 0
    comp_cnt = 0

    def dfs(u):
        nonlocal timer, comp_cnt

        tin[u] = low[u] = timer
        timer += 1

        stack.append(u)
        in_stack[u] = True

        for v in g[u]:
            if tin[v] == -1:
                dfs(v)
                low[u] = min(low[u], low[v])
            elif in_stack[v]:
                low[u] = min(low[u], tin[v])

        if low[u] == tin[u]:
            while True:
                x = stack.pop()
                in_stack[x] = False
                comp[x] = comp_cnt

                if x == u:
                    break

            comp_cnt += 1

    for i in range(n):
        if tin[i] == -1:
            dfs(i)

    comp_weight = [0] * comp_cnt

    for i in range(n):
        comp_weight[comp[i]] += cost[i]

    dag = set()

    for u in range(n):
        for v in g[u]:
            cu = comp[u]
            cv = comp[v]

            if cu != cv:
                dag.add((cu, cv))

    S = comp_cnt
    T = comp_cnt + 1

    dinic = Dinic(comp_cnt + 2)

    neg_sum = 0

    for i in range(comp_cnt):
        w = comp_weight[i]

        if w < 0:
            dinic.add_edge(S, i, -w)
            neg_sum += -w
        else:
            dinic.add_edge(i, T, w)

    for u, v in dag:
        dinic.add_edge(u, v, INF)

    ans = dinic.maxflow(S, T) - neg_sum
    ans = min(ans, 0)

    return str(ans) + "\n"

# provided sample
assert run(
"""3
1 1
2 2 3
1 3
10 20 -3
"""
) == "-3\n"

# empty collection optimal
assert run(
"""2
1 1
1 2
5 7
"""
) == "0\n"

# SCC cycle
assert run(
"""3
2 1 2
2 1 2
1 2
-5 -4 100
"""
) == "-9\n"

# all negative independent sets
assert run(
"""3
1 1
1 2
1 3
-1 -2 -3
"""
) == "-6\n"

# minimum size
assert run(
"""1
1 1
5
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bộ dương đơn | 0 | Xử lý bộ sưu tập trống | 
| Hai bộ âm chồng lên nhau | -9 | Độ chính xác nén SCC | 
| Bộ đơn âm độc lập | -6 | Nhiều thành phần khả thi rời rạc | 
| Đầu vào kích thước tối thiểu | 0 | Điều kiện biên với$n=1$| 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi bộ có sẵn đều có chi phí dương.```
2
1 1
1 2
5 7
```Thuật toán xây dựng hai SCC riêng biệt có trọng số dương. Trong mạng luồng, cả hai chỉ kết nối với sink. Việc cắt giảm tối thiểu loại trừ cả hai SCC, tạo ra tổng chi phí bằng 0. Điều này tương ứng chính xác với việc chọn bộ sưu tập trống. 

Bây giờ hãy xem xét sự phụ thuộc theo chu kỳ.```
2
2 1 2
2 1 2
-3 -4
```Giả sử tập 0 sở hữu số 1 và tập 1 sở hữu số 2. Khi đó, mỗi tập hợp có một cạnh không trùng với số của chủ sở hữu kia, tạo ra các cạnh:```
0 → 1
1 → 0
```Cả hai bộ thu gọn thành một SCC với tổng trọng lượng`-7`. Việc cắt tối thiểu chọn cả hai hoặc không chọn. Vì trọng số SCC âm nên thuật toán chọn cả hai, tạo ra câu trả lời đúng`-7`. 

Cuối cùng, hãy xem xét sự chồng chéo gây hiểu lầm.```
3
2 1 2
1 1
1 2
-100 1 1
```Chỉ chọn bộ đầu tiên là hợp lệ vì một bộ và hai số không thỏa mãn đẳng thức. Biểu đồ phụ thuộc nắm bắt được điều này. Tập âm lớn phụ thuộc vào cả hai chủ sở hữu đơn lẻ, do đó việc đóng buộc phải bao gồm cả ba tập hợp. Tổng chi phí của họ là`-98`và kích thước hợp trở thành 2 trong khi số tập hợp trở thành 3, điều này là không thể. Các ràng buộc đóng sẽ tự động ngăn chặn lựa chọn không hợp lệ này.
