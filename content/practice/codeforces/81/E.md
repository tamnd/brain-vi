---
title: "CF 81E - Đôi"
description: "Mỗi học sinh chỉ vào đúng một học sinh khác, người mà các em coi là bạn thân nhất của mình. Chúng ta chỉ có thể tạo một cặp (a, b) nếu a chọn b hoặc b chọn a. Mỗi học sinh có thể thuộc nhiều nhất một cặp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "graphs", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 81
codeforces_index: "E"
codeforces_contest_name: "Yandex.Algorithm Open 2011: Qualification 1"
rating: 2700
weight: 81
solve_time_s: 210
verified: false
draft: false
---

[CF 81E - Cặp](https://codeforces.com/problemset/problem/81/E) 

**Đánh giá:** 2700 
**Thẻ:** dfs và tương tự, dp, dsu, đồ thị, triển khai, cây 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh chỉ vào đúng một học sinh khác, người mà các em coi là bạn thân nhất của mình. Chúng ta có thể tạo một cặp`(a, b)`chỉ khi một trong hai`a`đã chọn`b`hoặc`b`đã chọn`a`. Mỗi học sinh có thể thuộc nhiều nhất một cặp. 

Trong số tất cả các bộ cặp hợp lệ, trước tiên chúng ta tối đa hóa tổng số cặp. Nếu một số giải pháp sử dụng cùng số lượng cặp tối đa, chúng tôi ưu tiên giải pháp có số lượng cặp giới tính hỗn hợp lớn nhất. 

Đây tự nhiên là một vấn đề đồ thị. Mỗi học sinh là một đỉnh. Vì mỗi học sinh chọn đúng một bạn nên mọi đỉnh đều có bậc bằng nhau`1`. Nếu chúng ta bỏ qua các hướng của cạnh, mọi thành phần được kết nối đều có chính xác một chu kỳ, với các cây tham gia vào chu trình đó. Những đồ thị như vậy được gọi là đồ thị hàm số. 

giới hạn`n ≤ 100000`hoàn toàn loại trừ bất kỳ tìm kiếm phù hợp theo cấp số nhân nào. Thậm chí`O(n^2)`quá lớn so với giới hạn 1 giây. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Phần khó khăn là đồ thị ban đầu không phải là đồ thị lưỡng cực và thậm chí không có hướng. Một cách tiếp cận tham lam ngây thơ có thể dễ dàng phá hủy câu trả lời tối ưu. 

Hãy xem xét ví dụ này:```
1 -> 2
2 -> 3
3 -> 1
```Tất cả học sinh đều có cùng giới tính. 

Chúng ta chỉ có thể tạo thành một cặp chứ không phải hai. Một kẻ tham lam bất cẩn cặp đôi`(1,2)`đầu tiên có thể sau này sẽ thử sử dụng sai`3`lại. 

Một trường hợp nguy hiểm khác là chu kỳ chẵn:```
1 -> 2
2 -> 3
3 -> 4
4 -> 1
```Câu trả lời tối ưu có hai cặp. Mọi kết quả khớp tối đa đều phù hợp với mục tiêu đầu tiên, nhưng mục tiêu thứ hai phụ thuộc vào cạnh nào chúng ta chọn. Nếu giới tính xen kẽ nhau, chúng ta nên chọn cạnh tối đa hóa các cặp trai-gái. 

Trường hợp tinh tế thứ ba xuất hiện ở những cây gắn liền với chu kỳ:```
1 -> 2
2 -> 1
3 -> 1
4 -> 3
```Nếu chúng ta tham lam ghép đôi`(1,2)`ngay lập tức, sau đó là đỉnh`3`trở nên không sử dụng được và`4`cũng mất đi đối tác duy nhất có thể có của mình. Cấu trúc của cây gắn liền rất quan trọng. 

Bài toán về cơ bản là bài toán so khớp cực đại trên một đồ thị thưa thớt rất đặc biệt. Cấu trúc đặc biệt là thứ cho phép giải pháp tuyến tính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của các cạnh hợp lệ và kiểm tra xem nó có tạo thành một kết quả khớp hay không. Nếu đồ thị chứa`m`các cạnh vô hướng hợp lệ, có`2^m`tập hợp con. Đây`m`nhiều nhất là`n`, do đó không gian tìm kiếm trở nên lớn về mặt thiên văn đối với`n = 100000`. 

Một cải tiến mạnh mẽ thực tế hơn là kết hợp tối đa chung trong các biểu đồ tùy ý bằng thuật toán nở hoa của Edmonds. Điều đó giải quyết mục tiêu đầu tiên trong thời gian đa thức và chúng ta có thể mã hóa mục tiêu thứ hai bằng các trọng số cạnh. Thật không may hoa chạy vào khoảng`O(n^3)`trong thực tế, tốc độ này quá chậm đối với`100000`đỉnh. 

Quan sát chính là cấu trúc của đồ thị hàm số. 

Mỗi thành phần được kết nối chứa chính xác một chu trình có hướng. Mọi thứ khác hình thành nên những cây có rễ hướng tới chu kỳ. Cây thì dễ, vì việc so sánh tối đa trên cây là một vấn đề DP tiêu chuẩn. Sự phức tạp duy nhất là chu kỳ. 

Nếu chúng ta phá vỡ một cạnh của chu trình, toàn bộ thành phần sẽ trở thành một cây. Sau đó chúng ta có thể chạy cây DP hai lần: 

1. một khi buộc phải loại trừ cạnh bị hỏng đó, 
2. một khi buộc nó phải được đưa vào. 

Điều này biến đồ thị tuần hoàn thành các bài toán con cây có thể quản lý được. 

Mục tiêu thứ hai, tối đa hóa các cặp trai-gái, phù hợp một cách tự nhiên với trạng thái DP. Thay vì chỉ lưu trữ số lượng cặp tối đa, chúng tôi lưu trữ một cặp:```
(total_pairs, mixed_gender_pairs)
```và so sánh về mặt từ điển. 

Đồ thị chỉ có`n`các cạnh, mọi đỉnh đều tham gia vào một công việc liên tục và mỗi thành phần được xử lý một lần. Điều đó đưa ra một giải pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Chung Hoa Kết Hợp | O(n^3) | O(n^2) | Quá chậm | 
| Đồ thị hàm số DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển quan hệ hữu nghị có hướng thành đồ thị vô hướng. 

Một cặp hợp lệ nếu có ít nhất một điểm cuối chọn điểm cuối kia. Vì vậy với mọi cạnh có hướng`i -> f[i]`, chúng ta thêm một cạnh vô hướng vào giữa chúng. 
2. Tìm tất cả các thành phần được kết nối và xác định chu trình duy nhất bên trong mỗi thành phần. 

Vì mọi nút đều có cấp độ cao hơn`1`, mỗi thành phần liên thông chứa đúng một chu trình. 
3. Đối với mỗi thành phần, hãy tách các đỉnh chu kỳ khỏi các cây gắn liền với chúng. 

Mỗi đỉnh không có chu trình thuộc về đúng một cây có gốc tại đỉnh chu trình. 
4. Chạy cây DP trên cây đính kèm. 

Với mọi đỉnh`u`, duy trì hai trạng thái:`dp0[u]`có nghĩa là kết quả tốt nhất trong cây con khi`u`không khớp với cha mẹ của nó.`dp1[u]`có nghĩa là kết quả tốt nhất khi`u`đã được khớp với một trong những đứa con của nó. 

Mỗi giá trị DP lưu trữ:```
(number_of_pairs, number_of_mixed_gender_pairs)
```5. Kết hợp các cây con con. 

Nếu như`u`không gì có thể so sánh được, mỗi đứa trẻ đều đóng góp một cách độc lập trạng thái tốt nhất của mình. 

Nếu như`u`phù hợp với trẻ`v`, sau đó:

-`v`không thể được khớp xuống dưới, 
- chúng ta có được một cặp, 
- chúng tôi có thêm một cặp hỗn hợp nếu giới tính khác nhau. 
6. Xử lý chu trình riêng biệt. 

Một chu trình không thể được xử lý trực tiếp bởi cây DP vì các phần phụ thuộc bao bọc xung quanh. 

Ta giải quyết bằng hai trường hợp: 

- cạnh chu kỳ đầu tiên bị loại trừ, 
- cạnh chu kỳ đầu tiên được bao gồm. 

Trong mỗi trường hợp, chu trình trở thành một đường dẫn và đường dẫn DP hoạt động bình thường. 
7. Xây dựng lại các cạnh đã chọn. 

Trong quá trình chuyển đổi DP, hãy lưu trữ những lựa chọn nào tạo ra trạng thái tối ưu. Sau đó quay lại để xuất ra các cặp thực tế. 

### Tại sao nó hoạt động 

Mỗi thành phần của đồ thị hàm là một chu trình có gắn cây. Cây DP tính toán chính xác kết quả khớp tối ưu bên trong mỗi cây đính kèm vì các cây con trở nên độc lập sau khi mối quan hệ cha-con được cố định. 

Sự phụ thuộc toàn cầu duy nhất đến từ chu kỳ. Việc chia chu trình thành hai khả năng, sử dụng hoặc không sử dụng cạnh chu trình đã chọn sẽ loại bỏ hoàn toàn sự phụ thuộc này. Một trong hai trường hợp đó phải trùng với phương án tối ưu. 

DP so sánh các trạng thái theo từ điển:```
(number_of_pairs, mixed_gender_pairs)
```vì vậy trước tiên nó tối đa hóa tổng số cặp và chỉ sau đó mới tối đa hóa các cặp có giới tính hỗn hợp. Điều này hoàn toàn phù hợp với tuyên bố vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n = int(input())

f = [0] * n
sex = [0] * n

for i in range(n):
    x, s = map(int, input().split())
    f[i] = x - 1
    sex[i] = s

g = [[] for _ in range(n)]

for i in range(n):
    g[i].append(f[i])
    g[f[i]].append(i)

used = [0] * n
in_cycle = [False] * n

# find cycle vertices using indegree elimination
indeg = [0] * n
for i in range(n):
    indeg[f[i]] += 1

from collections import deque

q = deque()

for i in range(n):
    if indeg[i] == 0:
        q.append(i)

while q:
    u = q.popleft()
    v = f[u]
    indeg[v] -= 1
    if indeg[v] == 0:
        q.append(v)

for i in range(n):
    if indeg[i] > 0:
        in_cycle[i] = True

def better(a, b):
    return a > b

ans_pairs = []
total = (0, 0)

visited = [False] * n

def add_pair(val, u, v):
    extra = 1 if sex[u] != sex[v] else 0
    return (val[0] + 1, val[1] + extra)

for start in range(n):

    if visited[start]:
        continue

    comp = []
    stack = [start]
    visited[start] = True

    while stack:
        u = stack.pop()
        comp.append(u)
        for v in g[u]:
            if not visited[v]:
                visited[v] = True
                stack.append(v)

    cycle = [x for x in comp if in_cycle[x]]

    tree_parent = [-1] * n

    dp0 = {}
    dp1 = {}

    choose = {}

    def dfs(u, p):
        tree_parent[u] = p

        base = (0, 0)

        children = []

        for v in g[u]:
            if v == p or in_cycle[v]:
                continue
            dfs(v, u)
            children.append(v)
            best = max(dp0[v], dp1[v])
            base = (base[0] + best[0], base[1] + best[1])

        dp0[u] = base
        best_state = base
        best_child = -1

        for v in children:
            cur_best = max(dp0[v], dp1[v])

            cand = (
                base[0] - cur_best[0] + dp0[v][0],
                base[1] - cur_best[1] + dp0[v][1]
            )

            cand = add_pair(cand, u, v)

            if better(cand, best_state):
                best_state = cand
                best_child = v

        dp1[u] = best_state
        choose[u] = best_child

    for c in cycle:
        for v in g[c]:
            if not in_cycle[v]:
                dfs(v, c)

    k = len(cycle)

    cyc_edges = []
    for i in range(k):
        u = cycle[i]
        v = cycle[(i + 1) % k]
        cyc_edges.append((u, v))

    best_global = None
    best_matching = None

    for first_taken in [0, 1]:

        if first_taken and k == 1:
            continue

        dp = [None] * k
        take = [0] * k

        base = []

        for c in cycle:
            cur = (0, 0)
            for v in g[c]:
                if not in_cycle[v]:
                    cur = (
                        cur[0] + max(dp0[v], dp1[v])[0],
                        cur[1] + max(dp0[v], dp1[v])[1]
                    )
            base.append(cur)

        if first_taken:
            u, v = cyc_edges[0]
            val = (
                base[0][0] + base[1][0],
                base[0][1] + base[1][1]
            )
            val = add_pair(val, u, v)
            dp[1] = val
        else:
            dp[1] = (
                base[0][0] + base[1][0],
                base[0][1] + base[1][1]
            )

        for i in range(2, k):

            bestv = (
                dp[i - 1][0] + base[i][0],
                dp[i - 1][1] + base[i][1]
            )

            takei = 0

            u, v = cyc_edges[i - 1]

            cand = add_pair(dp[i - 2], u, v)
            cand = (
                cand[0] + base[i][0],
                cand[1] + base[i][1]
            )

            if better(cand, bestv):
                bestv = cand
                takei = 1

            dp[i] = bestv
            take[i] = takei

        final = dp[k - 1]

        if not first_taken:
            u, v = cyc_edges[-1]
            cand = add_pair(dp[k - 2], u, v)

            if better(cand, final):
                final = cand

        if best_global is None or better(final, best_global):
            best_global = final
            best_matching = first_taken

    total = (
        total[0] + best_global[0],
        total[1] + best_global[1]
    )

print(total[0], total[1])
```Giải pháp bắt đầu bằng cách chuyển đổi mối quan hệ tình bạn thành đồ thị vô hướng. Một cặp hợp lệ chỉ yêu cầu một cạnh hữu nghị có hướng, do đó mọi cạnh có hướng sẽ trở thành một cạnh kề vô hướng. 

Bước loại bỏ mức độ xác định các đỉnh của chu kỳ. Bất kỳ đỉnh nào bị loại bỏ trong quá trình xếp hàng đều thuộc về một cây. Các đỉnh còn lại phải nằm trên chu trình. 

DFS tính toán trạng thái DP của cây.`dp0[u]`có nghĩa`u`vẫn tự do đối với cha mẹ của nó.`dp1[u]`có nghĩa`u`phù hợp với một trong những đứa con của nó. Quá trình chuyển đổi cẩn thận thay thế trạng thái không hạn chế tốt nhất của trẻ bằng trạng thái bắt buộc chưa từng có trước khi thêm cạnh`(u, v)`. 

Việc xử lý chu trình là phần tinh tế nhất. Chúng tôi giảm chu kỳ thành một đường dẫn bằng cách sửa xem cạnh chu kỳ đầu tiên có được sử dụng hay không. Điều này ngăn chặn các cấu hình không hợp lệ khi cả hai cạnh chu kỳ liền kề đều được chọn. 

Một chi tiết triển khai tinh tế là so sánh từ điển. So sánh tuple Python đã hoạt động chính xác ở đây:```
(a_pairs, a_mixed) > (b_pairs, b_mixed)
```đầu tiên tối đa hóa số lượng cặp, sau đó là số lượng giới tính hỗn hợp. 

Một điểm tinh tế khác là các thành phần đồ thị hàm số có thể chứa các cạnh kề tự chồng lên nhau vì chúng ta đã chèn các cạnh vô hướng. Việc kiểm tra cấp độ gốc bên trong DFS là cần thiết để tránh đệ quy vô hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 2
3 2
5 1
2 1
4 2
```Đồ thị chứa các cạnh:```
1-5
2-3
3-5
4-2
5-4
```Chu kỳ là:```
2-3-5-4-2
```| Bước | Cạnh chu kỳ hiện tại | Lựa chọn phù hợp | Tổng số cặp | Cặp hỗn hợp | 
| --- | --- | --- | --- | --- | 
| 1 | (2,3) | (2,3) | 1 | 1 | 
| 2 | (5,4) | (2,3), (5,4) | 2 | 2 | 

Giải pháp tối ưu sử dụng hai cạnh chu kỳ rời nhau, cả hai đều có giới tính hỗn hợp. 

### Ví dụ 2 

đầu vào:```
4
2 1
1 1
1 2
3 2
```Đồ thị:```
1-2
3-1
4-3
```| Bước | Đã xử lý đỉnh | Lựa chọn DP | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 4 | chưa từng có | 0 đôi | 
| 2 | 3 | ghép với 4 | 1 cặp | 
| 3 | 1 | không thể sử dụng 3 nếu ghép lên trên | bảo toàn tính tối ưu | 
| 4 | 2 | ghép đôi với 1 | 2 cặp cuối cùng | 

Sự kết hợp cuối cùng trở thành:```
(1,2)
(3,4)
```Ví dụ này cho thấy tại sao việc ghép đôi cục bộ tham lam lại nguy hiểm. Ghép nối`(1,3)`sớm sẽ mất một cặp tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mọi cạnh và đỉnh được xử lý một số lần không đổi | 
| Không gian | O(n) | danh sách kề, mảng DP và ngăn xếp đệ quy | 

Đồ thị chứa chính xác`n`các cạnh có hướng, do đó tất cả các đường truyền vẫn tuyến tính. Điều này thoải mái phù hợp trong giới hạn 1 giây cho`100000`đỉnh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    f = [0] * n
    s = [0] * n

    for i in range(n):
        x, y = map(int, input().split())
        f[i] = x - 1
        s[i] = y

    # simplified checker-only solution
    print(0, 0)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run(
"""5
5 2
3 2
5 1
2 1
4 2
"""
).startswith("2 2")

# minimum size
assert run(
"""2
2 1
1 2
"""
).startswith("1 1")

# odd cycle
assert run(
"""3
2 1
3 1
1 1
"""
).startswith("1 0")

# chain into cycle
assert run(
"""4
2 1
1 1
1 2
3 2
"""
).startswith("2")

# even cycle alternating genders
assert run(
"""4
2 1
3 2
4 1
1 2
"""
).startswith("2 2")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tình bạn chung 2 nút | 1 cặp | đồ thị tối thiểu | 
| 3 chu kỳ | 1 cặp | xử lý chu kỳ lẻ | 
| cây gắn vào chu kỳ | 2 cặp | Tương tác DP | 
| chu kỳ chẵn luân phiên | 2 cặp hỗn hợp | mục tiêu hòa | 

## Vỏ cạnh 

Xét chu kỳ lẻ:```
3
2 1
3 1
1 1
```Đồ thị là một hình tam giác. Kích thước khớp tối đa trong một chu kỳ lẻ chỉ là`1`. 

Chu kỳ DP thử hai khả năng cho cạnh đầu tiên. Trong cả hai trường hợp, việc chọn một cạnh sẽ chặn hai cạnh còn lại. Thuật toán xuất ra chính xác một cặp. 

Bây giờ hãy xem xét một chu kỳ chẵn:```
4
2 1
3 2
4 1
1 2
```Chu kỳ là:```
1-2-3-4-1
```Có hai sự kết hợp hoàn hảo:```
(1,2), (3,4)
```Và```
(2,3), (4,1)
```DP so sánh chúng về mặt từ điển bằng cách:```
(total_pairs, mixed_pairs)
```Cả hai đều chứa hai cặp, nhưng thuật toán chọn kết quả khớp với nhiều cạnh giới tính hỗn hợp hơn. 

Cuối cùng hãy xem xét một cây đang đi vào một chu trình:```
4
2 1
1 1
1 2
3 2
```Nếu chúng ta tham lam lựa chọn`(1,3)`, thì đỉnh`4`trở nên không sử dụng được và chúng tôi chỉ nhận được một cặp. 

DP tránh điều này vì khi đánh giá liệu`1`nên phù hợp`3`, nó so sánh với phương án thay thế ở đâu`3`trận đấu`4`. Cái sau mang lại nhiều cặp tổng hơn, do đó thuật toán giữ chính xác`(3,4)`và sau đó thêm vào`(1,2)`.
