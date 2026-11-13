---
title: "CF 176E - Khảo cổ học"
description: "Chúng tôi được tặng một cây có trọng lượng. Làng tương ứng với các đỉnh và đường tương ứng với các cạnh có trọng số. Tại bất kỳ thời điểm nào, chỉ có một số ngôi làng còn sống. Làng có thể được thêm vào và loại bỏ theo thời gian."
date: "2026-06-02T17:07:37+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 176
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2012 - Round 2"
rating: 3100
weight: 176
solve_time_s: 160
verified: false
draft: false
---

[CF 176E - Khảo cổ học](https://codeforces.com/problemset/problem/176/E) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 2 phút 40 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây có trọng lượng. Làng tương ứng với các đỉnh và đường tương ứng với các cạnh có trọng số. 

Tại bất kỳ thời điểm nào, chỉ có một số ngôi làng là "sống sót". Làng có thể được thêm vào và loại bỏ theo thời gian. Sau mỗi lần cập nhật, chúng tôi có thể được hỏi tổng chiều dài của những con đường được sử dụng thực tế. 

Cụm từ chính là mọi người sử dụng số lượng đường tối thiểu vẫn cho phép đi lại giữa mọi cặp làng hiện có. Vì đồ thị ban đầu là một cây nên câu trả lời đơn giản là tổng trọng số của cây con liên thông nhỏ nhất chứa tất cả các đỉnh hoạt động. 

Một cách khác để nói điều này là: trong số tất cả các cạnh của cây ban đầu, giữ chính xác những cạnh nằm trên đường đi giữa hai đỉnh hoạt động. Tổng trọng số của chúng là câu trả lời cần thiết. 

Cây chứa tới$10^5$đỉnh và có tới$10^5$cập nhật và truy vấn. Việc tính toán lại cây con tối thiểu từ đầu sau mỗi lần sửa đổi là không thể. Thậm chí một$O(n)$truyền tải mỗi bản cập nhật sẽ yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Vấn đề yêu cầu một tập hợp các đỉnh động hoàn toàn bên trong một cây tĩnh, với các thao tác chèn, xóa và truy vấn độ dài cây con. 

Một số trường hợp đặc biệt rất dễ bị hiểu lầm. 

Hãy xem xét một đỉnh hoạt động duy nhất:```
1
?
```Không cần cạnh nào để nối một đỉnh, vì vậy câu trả lời là 0. 

Việc triển khai bất cẩn luôn tính tổng khoảng cách giữa các đỉnh hoạt động có thể vô tình tạo ra giá trị dương. 

Xem xét hai đỉnh hoạt động được kết nối bằng một đường dẫn:```
1 --5-- 2 --7-- 3
active = {1,3}
```Câu trả lời là$5+7=12$. 

Cây con tối thiểu chính xác là đường đi duy nhất giữa chúng. 

Bây giờ hãy xem xét ba đỉnh hoạt động:```
    2
    |
1 --3-- 4
```Nếu tất cả các đỉnh đều hoạt động thì câu trả lời là tổng của cả ba cạnh. 

Một ý tưởng ngây thơ như "tổng khoảng cách theo cặp và chia cho một số" không thành công vì mỗi cạnh tham gia vào nhiều đường dẫn theo cặp. 

Phần khó khăn là hỗ trợ chèn và xóa trong khi vẫn duy trì kích thước của cây Steiner của các đỉnh hoạt động. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. 

Sau mỗi lần cập nhật, hãy thu thập tất cả các đỉnh hoạt động. Xây dựng cây ảo của các đỉnh đó hoặc đơn giản là đánh dấu mọi cạnh thuộc một đường dẫn giữa các đỉnh hoạt động và tính tổng trọng số của cạnh tương ứng. 

Điều này đúng vì câu trả lời chính xác là cây con được kết nối tối thiểu bao trùm tất cả các đỉnh hoạt động. 

Thật không may, nếu có$k$các đỉnh hoạt động, thậm chí việc xây dựng câu trả lời từ đầu cũng tốn ít nhất$O(k \log n)$, thường$O(n)$. Với$10^5$cập nhật, điều này trở nên quá lớn. 

Sự đột phá đến từ đặc tính cổ điển của cây cối. 

Hãy để các đỉnh hoạt động được sắp xếp theo thời gian vào Euler-tour. Nếu chúng ta kết nối các đỉnh hoạt động liên tiếp theo thứ tự tuần hoàn đó và tính tổng khoảng cách theo cặp của chúng thì mỗi cạnh của cây Steiner được tính chính xác hai lần. 

Như vậy:$$\text{SteinerWeight} = \frac{ \sum d(v_i,v_{i+1}) }{ 2 }$$trong đó thứ tự tuần hoàn theo thứ tự DFS. 

Điều này biến vấn đề từ việc duy trì một cây con thành duy trì một tập đỉnh tuần hoàn. 

Khi một đỉnh được chèn vào, chỉ có đỉnh trước và đỉnh kế tiếp của nó theo thứ tự Euler là quan trọng. Chu kỳ toàn cầu thay đổi cục bộ. 

Giả sử đỉnh được chèn là$x$, tiền thân là$p$, người kế vị là$s$. 

Trước đây sự đóng góp có$$d(p,s)$$Sau khi chèn nó sẽ trở thành$$d(p,x)+d(x,s)$$Vì vậy, tổng chu kỳ được duy trì thay đổi bởi$$d(p,x)+d(x,s)-d(p,s).$$Việc xóa thực hiện thao tác nghịch đảo. 

Nhiệm vụ còn lại là tính toán khoảng cách một cách nhanh chóng. Vì cây không bao giờ thay đổi nên chúng tôi xử lý trước độ sâu, khoảng cách gốc và LCA. 

Sau đó$$d(u,v) = distRoot[u] + distRoot[v] - 2\,distRoot[lca(u,v)].$$Mỗi bản cập nhật chỉ yêu cầu các truy vấn tiền nhiệm/kế nhiệm trong một tập hợp có thứ tự và số lượng tính toán khoảng cách không đổi. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi lần cập nhật/truy vấn |$O(n)$| Quá chậm | 
| Tối ưu |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý 

1. Gốc cây ở bất kỳ đỉnh nào, ví dụ đỉnh 1. 
2. Chạy DFS và tính thời gian vào Euler`tin[u]`. 
3. Tính toán độ sâu, giá trị khoảng cách gốc và tổ tiên nâng nhị phân cho LCA. 
4. Với mỗi đỉnh, lưu trữ cặp`(tin[u], u)`. Các đỉnh hoạt động sẽ được duy trì theo thứ tự`tin`. 

### Bảo trì động 

Duy trì: 

- Tập hợp các đỉnh hoạt động có thứ tự được sắp xếp theo thời gian Euler. 
- Một giá trị`cycle_sum`.`cycle_sum`là$$\sum d(v_i,v_{i+1})$$theo thứ tự DFS tuần hoàn. 

### Chèn một đỉnh 

1. Tìm đỉnh trước và đỉnh kế tiếp của đỉnh mới theo thứ tự Euler. 
2. Nếu đây là đỉnh hoạt động đầu tiên, chỉ cần chèn nó vào. 
3. Nếu không thì thêm$$d(pred,x)+d(x,succ)-d(pred,succ)$$ĐẾN`cycle_sum`. 

4. Chèn đỉnh vào tập có thứ tự. 

Cạnh cũ của chu kỳ được thay thế bằng hai cạnh mới. 

### Xóa một đỉnh 

1. Tìm đỉnh liền kề và liền kề của đỉnh. 
2. Nếu đó là đỉnh hoạt động duy nhất, hãy loại bỏ nó và đặt phần đóng góp của chu trình về 0. 
3. Nếu không thì thêm$$d(pred,succ)-d(pred,x)-d(x,succ)$$ĐẾN`cycle_sum`. 

4. Loại bỏ đỉnh. 

Điều này đảo ngược chính xác thao tác chèn. 

### Trả lời truy vấn 

1. Nếu có ít hơn hai đỉnh hoạt động, trả lời 0. 
2. Nếu không thì xuất ra$$\frac{\text{cycle\_sum}}{2}.$$### Tại sao nó hoạt động 

Hãy để các đỉnh hoạt động được liệt kê theo chu kỳ theo thứ tự DFS. 

Tận dụng mọi lợi thế$e$của cây con tối thiểu bao trùm tất cả các đỉnh hoạt động. Đang xóa$e$chia cây thành hai thành phần. Từ$e$thuộc cây Steiner, cả hai bên đều chứa các đỉnh tích cực. 

Theo thứ tự DFS tuần hoàn, có chính xác hai cặp liên tiếp chéo từ bên này sang bên kia. Mỗi lần vượt biển như vậy góp phần tạo nên sức nặng của$e$một lần vào tổng khoảng cách. 

Do đó, mỗi cạnh của cây Steiner đóng góp chính xác hai lần vào tổng khoảng cách tuần hoàn. Các cạnh bên ngoài cây Steiner đóng góp bằng 0 lần. 

Do đó số lượng duy trì luôn chính xác gấp đôi câu trả lời. Vì việc chèn và xóa cập nhật thứ tự tuần hoàn cục bộ nên bất biến vẫn đúng sau mỗi thao tác. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, insort

input = sys.stdin.readline

def solve():
    n = int(input())

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append((b, c))
        g[b].append((a, c))

    LOG = 17
    while (1 << LOG) <= n:
        LOG += 1

    tin = [0] * n
    depth = [0] * n
    root_dist = [0] * n
    up = [[0] * n for _ in range(LOG)]

    timer = 0

    stack = [(0, 0, 0, 0)]
    while stack:
        u, p, state, idx = stack.pop()

        if state == 0:
            tin[u] = timer
            timer += 1

            up[0][u] = p

            stack.append((u, p, 1, 0))

            for v, w in reversed(g[u]):
                if v == p:
                    continue
                depth[v] = depth[u] + 1
                root_dist[v] = root_dist[u] + w
                stack.append((v, u, 0, 0))

    for k in range(1, LOG):
        prev = up[k - 1]
        cur = up[k]
        for v in range(n):
            cur[v] = prev[prev[v]]

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

        for k in range(LOG - 1, -1, -1):
            if up[k][a] != up[k][b]:
                a = up[k][a]
                b = up[k][b]

        return up[0][a]

    def dist(a, b):
        c = lca(a, b)
        return root_dist[a] + root_dist[b] - 2 * root_dist[c]

    q = int(input())

    active = []
    cycle_sum = 0

    def add_vertex(x):
        nonlocal cycle_sum

        key = (tin[x], x)

        if not active:
            insort(active, key)
            return

        pos = bisect_left(active, key)

        succ = active[pos % len(active)][1]
        pred = active[(pos - 1) % len(active)][1]

        cycle_sum += (
            dist(pred, x)
            + dist(x, succ)
            - dist(pred, succ)
        )

        active.insert(pos, key)

    def remove_vertex(x):
        nonlocal cycle_sum

        key = (tin[x], x)

        pos = bisect_left(active, key)

        if len(active) == 1:
            active.pop()
            cycle_sum = 0
            return

        pred = active[(pos - 1) % len(active)][1]
        succ = active[(pos + 1) % len(active)][1]

        cycle_sum += (
            dist(pred, succ)
            - dist(pred, x)
            - dist(x, succ)
        )

        active.pop(pos)

    out = []

    for _ in range(q):
        s = input().split()

        if s[0] == '+':
            add_vertex(int(s[1]) - 1)
        elif s[0] == '-':
            remove_vertex(int(s[1]) - 1)
        else:
            out.append(str(cycle_sum // 2))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Sau khối mã, quan sát chính là tổng khoảng cách theo chu kỳ được duy trì. Các đỉnh hoạt động luôn được lưu trữ theo thứ tự DFS. Mỗi bản cập nhật chỉ sửa đổi một vị trí trong chu trình đó, vì vậy chúng ta không bao giờ cần phải xây dựng lại cây Steiner. 

Quá trình tiền xử lý LCA cung cấp các truy vấn khoảng cách trong$O(\log n)$. Mỗi lần chèn hoặc xóa sẽ thực hiện một số lượng không đổi các phép tính khoảng cách như vậy và tìm kiếm tiền thân/kế tiếp. 

Trong C++, giải pháp dự định sử dụng một tập hợp có thứ tự. Trong Python, giải pháp được chấp nhận ở chất lượng sản xuất thường sử dụng cách triển khai cây cân bằng. Đoạn mã biên tập ở trên minh họa rõ ràng công thức cập nhật toán học, trong khi việc triển khai Codeforces được chấp nhận sử dụng một tập hợp có thứ tự thực sự để duy trì$O(\log n)$cập nhật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các đỉnh hoạt động được biểu diễn theo thứ tự Euler. 

| Hoạt động | Bộ hoạt động | Tổng chu kỳ | Trả lời | 
| --- | --- | --- | --- | 
| +3 | {3} | 0 | | 
| +1 | {1,3} | 10 | | 
| ? | {1,3} | 10 | 5 | 
| +6 | {1,3,6} | 28 | | 
| ? | {1,3,6} | 28 | 14 | 
| +5 | {1,3,5,6} | 34 | | 
| ? | {1,3,5,6} | 34 | 17 | 
| -6 | {1,3,5} | 20 | | 
| -3 | {1,5} | 20 | | 
| ? | {1,5} | 20 | 10 | 

Các câu trả lời thu được như`cycle_sum / 2`. 

### Ví dụ tùy chỉnh 

Cây:```
1 --4-- 2 --6-- 3
```Hoạt động:```
+1
+3
?
+2
?
```| Hoạt động | Bộ hoạt động | Tổng chu kỳ | Trả lời | 
| --- | --- | --- | --- | 
| +1 | {1} | 0 | | 
| +3 | {1,3} | 20 | | 
| ? | {1,3} | 20 | 10 | 
| +2 | {1,2,3} | 20 | | 
| ? | {1,2,3} | 20 | 10 | 

Việc thêm đỉnh 2 không làm thay đổi cây Steiner vì nó đã nằm trên đường nối 1 và 3. 

Điều này xác nhận rằng số lượng được duy trì đo lường cây con kết nối tối thiểu chứ không phải số lượng đỉnh hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log n)$| Tiền xử lý LCA cộng với cập nhật logarit | 
| Không gian |$O(n\log n)$| Bàn nâng nhị phân và lưu trữ cây | 

Cây chứa nhiều nhất$10^5$đỉnh và có nhiều nhất$10^5$hoạt động. Chi phí cập nhật logarit giữ cho tổng công việc thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    return "<execute solve() here>"

# sample 1
assert run(
"""6
1 2 1
1 3 5
4 1 7
4 5 3
6 4 2
10
+ 3
+ 1
?
+ 6
?
+ 5
?
- 6
- 3
?
"""
) == "5\n14\n17\n10"

# single vertex
assert run(
"""1
3
+ 1
?
- 1
"""
) == "0"

# simple chain
assert run(
"""3
1 2 4
2 3 6
5
+ 1
+ 3
?
+ 2
?
"""
) == "10\n10"

# add then remove middle
assert run(
"""3
1 2 1
2 3 1
7
+ 1
+ 2
+ 3
?
- 2
?
?
"""
) == "2\n2\n2"

# star tree
assert run(
"""4
1 2 5
1 3 5
1 4 5
6
+ 2
+ 3
?
+ 4
?
?
"""
) == "10\n15\n15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây đỉnh đơn | 0 | Cây Steiner trống | 
| Chuỗi có kích hoạt điểm cuối | 10 | Tính toán khoảng cách đường đi | 
| Xóa đỉnh hoạt động ở giữa | 2 | Các đỉnh hoạt động không nhất thiết phải là đỉnh Steiner | 
| Cây sao | 10, 15 | Nhiều chi nhánh tham gia tại một trung tâm | 

## Vỏ cạnh 

### Chỉ có một đỉnh hoạt động 

đầu vào:```
1
3
+ 1
?
- 1
```Tập hoạt động chứa một đỉnh duy nhất. Tổng chu trình được duy trì vẫn bằng 0 vì không có cạnh nào trong chu trình. Câu trả lời là`0 / 2 = 0`. 

### Đỉnh hoạt động nằm trên đường dẫn Steiner hiện có 

đầu vào:```
3
1 2 4
2 3 6
5
+ 1
+ 3
?
+ 2
?
```Ban đầu câu trả lời là 10 vì đường đi từ 1 đến 3 sử dụng cả hai cạnh. 

Sau khi kích hoạt đỉnh 2, cây con kết nối tối thiểu không thay đổi. Bản cập nhật chèn bổ sung thêm$$d(1,2)+d(2,3)-d(1,3) = 4+6-10 = 0.$$Giá trị được duy trì không thay đổi, tạo ra câu trả lời đúng. 

### Bộ hoạt động trống 

đầu vào:```
2
1 2 7
4
+ 1
- 1
?
?
```Sau khi xóa, không còn đỉnh hoạt động nào. 

Cấu trúc dữ liệu trở nên trống rỗng và`cycle_sum`được đặt lại về 0. Mọi truy vấn đều xuất ra chính xác 0. 

### Hai đỉnh hoạt động trở thành một 

đầu vào:```
2
1 2 7
5
+ 1
+ 2
- 2
?
?
```Trước khi xóa tổng chu kỳ là$14$, tương ứng với câu trả lời$7$. 

Xóa đỉnh 2 chỉ còn lại một đỉnh hoạt động. Trường hợp đặc biệt cho kích thước một sẽ đặt lại giá trị được duy trì về 0, tránh các đóng góp cũ từ chu kỳ trước. Câu trả lời trở thành 0, chính xác là kích thước của cây Steiner chứa một đỉnh.
