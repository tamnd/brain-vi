---
title: "CF 106189C - Và lại là mê cung"
description: "Chúng ta có một mê cung hình chữ nhật. Các ô trống có thể duyệt được, các ô được đánh dấu X bị chặn. Người chơi bắt đầu ở góc trên bên trái và muốn đến góc dưới bên phải bằng cách di chuyển bốn hướng. Chúng tôi muốn đặt chính xác một chướng ngại vật bổ sung vào một ô trống."
date: "2026-06-25T06:47:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "C"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 65
verified: true
draft: false
---

[CF 106189C - Và một lần nữa là mê cung](https://codeforces.com/problemset/problem/106189/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mê cung hình chữ nhật. Các ô trống có thể duyệt được, các ô được đánh dấu`X`bị chặn. Người chơi bắt đầu ở góc trên bên trái và muốn đến góc dưới bên phải bằng cách di chuyển bốn hướng. 

Chúng tôi muốn đặt chính xác một chướng ngại vật bổ sung vào một ô trống. Một ô hợp lệ nếu sau khi chặn nó, đường đi ngắn nhất từ ​​đầu đến cuối trở nên dài hơn hoàn toàn, trong khi đường dẫn vẫn tồn tại. 

Đầu ra là mê cung ban đầu, ngoại trừ mọi ô hợp lệ được thay thế bằng`*`. 

Lưới có thể lớn bằng`1000 × 1000`, có nghĩa là lên tới một triệu tế bào. Bất kỳ giải pháp nào tính toán lại đường đi ngắn nhất cho mỗi ô trống đều hoàn toàn không khả thi. Ngay cả một BFS cũng đã chạm tới khoảng một triệu đỉnh, vì vậy chúng ta cần thứ gì đó rất gần với độ phức tạp tuyến tính trong kích thước của lưới. 

Phần tinh tế là việc tăng độ dài đường đi ngắn nhất không giống như việc ngắt kết nối mê cung. 

Hãy xem xét mê cung này:```
...
...
...
```Mọi đường đi ngắn nhất từ ​​góc trên bên trái đến góc dưới bên phải đều có độ dài bằng 4, nhưng có nhiều đường đi như vậy. Việc chặn bất kỳ ô bên trong nào vẫn để lại một đường đi ngắn nhất khác có độ dài 4, do đó không có ô trả lời nào tồn tại. 

Bây giờ hãy xem xét:```
.....
.XXX.
.....
```Hành lang ở giữa là con đường ngắn nhất duy nhất. Việc chặn một trong các ô bên trong của nó sẽ phá hủy tất cả các con đường ngắn nhất, nhưng vẫn tồn tại một đường vòng dài hơn. Những ô đó là hợp lệ. 

Một cái bẫy khác chính là nút thắt cổ chai thực sự:```
...
.X.
...
```Nếu một số ô nằm trên mọi đường dẫn có thể, không chỉ mọi đường dẫn ngắn nhất, thì việc chặn ô đó sẽ ngắt kết nối bắt đầu và kết thúc. Ô như vậy không được đánh dấu vì câu lệnh yêu cầu đường dẫn vẫn tồn tại sau đó. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ô trống, hãy tạm thời chặn nó, chạy BFS từ đầu, tính khoảng cách ngắn nhất mới và kiểm tra xem nó có lớn hơn trong khi vẫn hữu hạn hay không. 

Nếu lưới chứa một triệu ô, điều đó có nghĩa là có tới một triệu lần chạy BFS. Mỗi BFS đã có sẵn`O(NM)`, do đó tổng công việc trở nên đại khái`O((NM)^2)`, vượt xa mọi giới hạn thực tế. 

Quan sát quan trọng là một ô chỉ có thể tăng độ dài đường đi ngắn nhất nếu nó thuộc về **mọi đường đi ngắn nhất**. 

Giả sử tồn tại một đường đi ngắn nhất tránh được một ô`v`. Sau khi chặn`v`, con đường ngắn nhất đó vẫn tồn tại nên khoảng cách ngắn nhất không thể tăng lên. 

Điều này biến vấn đề thành hai câu hỏi độc lập. 

Đầu tiên, ô nào thuộc về mọi đường đi ngắn nhất? 

Cho phép`distS[v]`là khoảng cách từ điểm xuất phát và`distT[v]`khoảng cách từ đích. Một ô thuộc về một đường đi ngắn nhất chính xác khi```
distS[v] + distT[v] = D
```Ở đâu`D`là khoảng cách ngắn nhất từ ​​đầu đến cuối. 

Tất cả các ô có đường đi ngắn nhất tạo thành một DAG phân lớp. Mọi đường đi ngắn nhất đều đi qua đúng một đỉnh ở khoảng cách`0`, một ở khoảng cách`1`, một ở khoảng cách`2`, vân vân. 

Nếu một lớp chứa chính xác một ô đường dẫn ngắn nhất thì mọi đường dẫn ngắn nhất buộc phải đi qua nó. Nếu một lớp chứa hai ô trở lên, đường dẫn ngắn nhất có thể chọn trong số chúng. 

Vì vậy, một ô có đường đi ngắn nhất thuộc về mọi đường đi ngắn nhất khi và chỉ nếu nó là ô có đường đi ngắn nhất duy nhất trong lớp khoảng cách của nó. 

Điều đó xác định tất cả các ô mà việc loại bỏ sẽ phá hủy mọi đường dẫn ngắn nhất. 

Câu hỏi thứ hai là liệu việc loại bỏ một ô như vậy có còn để lại đường dẫn nào đó hay không. 

Đó là một câu hỏi đồ thị cổ điển. Chúng ta cần biết liệu tế bào có phải là một`s-t`cắt đỉnh. Nếu việc loại bỏ nó sẽ ngắt kết nối từ đầu đến cuối thì đó không phải là câu trả lời hợp lệ. Nếu điểm bắt đầu và điểm kết thúc vẫn được kết nối thì tất cả các đường đi ngắn nhất sẽ biến mất và khoảng cách ngắn nhất mới sẽ trở nên lớn hơn rất nhiều. 

Điều này có thể được kiểm tra bằng một DFS liên kết thấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((NM)2) | O(NM) | Quá chậm | 
| Tối ưu | O(NM) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS từ ô bắt đầu và tính toán`distS`. 
2. Chạy BFS từ ô kết thúc và tính toán`distT`. 
3. Hãy để`D = distS[finish]`. 
4. Đối với mỗi ô trống`v`, kiểm tra xem```
distS[v] + distT[v] = D
```Nếu đúng thì ô nằm trên ít nhất một đường đi ngắn nhất. 
5. Đếm xem có bao nhiêu ô có đường đi ngắn nhất xuất hiện trong mỗi lớp khoảng cách`distS[v]`. 
6. Ô có đường đi ngắn nhất`v`thuộc về mọi đường đi ngắn nhất chính xác khi số lớp của nó bằng`1`. 

Lý do là mỗi đường đi ngắn nhất phải chứa một đỉnh của mỗi lớp. Nếu một lớp chỉ có một ứng cử viên thì mọi đường đi ngắn nhất đều phải đi qua lớp đó. 
7. Chạy DFS liên kết thấp ngay từ đầu trên toàn bộ biểu đồ ô tự do. 
8. Đối với mỗi đỉnh, hãy xác định xem việc loại bỏ nó có làm ngắt kết nối điểm bắt đầu với điểm kết thúc hay không. Đánh dấu các đỉnh như`st_cut`. 
9. Một ô là câu trả lời nếu tất cả những điều sau đây đều đúng:```
it is free
it is not start
it is not finish
it belongs to every shortest path
it is not an s-t cut vertex
```10. Xuất lưới, thay thế chính xác các ô đó bằng`*`. 

### Tại sao nó hoạt động 

Khoảng cách ngắn nhất chỉ tăng lên nếu mọi đường đi ngắn nhất ban đầu bị phá hủy. Một ô sẽ phá hủy mọi con đường ngắn nhất một cách chính xác khi nó thuộc về tất cả chúng. 

Đối số lớp khoảng cách xác định chính xác các ô đó. Mọi đường đi ngắn nhất sẽ tiến qua các lớp theo thứ tự và một lớp có một đỉnh có đường đi ngắn nhất sẽ buộc đỉnh đó đi vào mọi đường đi ngắn nhất. 

Sau khi loại bỏ một ô như vậy, tất cả các đường dẫn ngắn nhất sẽ biến mất. Nếu điểm bắt đầu và điểm kết thúc vẫn được kết nối thì vẫn tồn tại một số đường dẫn dài hơn, do đó khoảng cách ngắn nhất mới sẽ lớn hơn. Nếu bắt đầu và kết thúc bị ngắt kết nối, nước đi không hợp lệ. 

Thuật toán đánh dấu chính xác các ô thỏa mãn cả hai điều kiện, vì vậy mọi ô được báo cáo đều hợp lệ và mọi ô hợp lệ đều được báo cáo. 

## Giải pháp Python```python
import sys
from collections import deque
from array import array

input = sys.stdin.readline

n, m = map(int, input().split())
g = [input().strip() for _ in range(n)]

V = n * m

def idx(r, c):
    return r * m + c

start = 0
target = V - 1

free = bytearray(V)
for r in range(n):
    row = g[r]
    base = r * m
    for c in range(m):
        if row[c] != 'X':
            free[base + c] = 1

def bfs(src):
    dist = array('i', [-1]) * V
    q = deque([src])
    dist[src] = 0

    while q:
        v = q.popleft()
        r = v // m
        c = v - r * m

        nv = v - m
        if r and free[nv] and dist[nv] == -1:
            dist[nv] = dist[v] + 1
            q.append(nv)

        nv = v + m
        if r + 1 < n and free[nv] and dist[nv] == -1:
            dist[nv] = dist[v] + 1
            q.append(nv)

        nv = v - 1
        if c and free[nv] and dist[nv] == -1:
            dist[nv] = dist[v] + 1
            q.append(nv)

        nv = v + 1
        if c + 1 < m and free[nv] and dist[nv] == -1:
            dist[nv] = dist[v] + 1
            q.append(nv)

    return dist

distS = bfs(start)
distT = bfs(target)

D = distS[target]

cnt = array('i', [0]) * (D + 1)
on_shortest = bytearray(V)

for v in range(V):
    if free[v] and distS[v] != -1 and distS[v] + distT[v] == D:
        on_shortest[v] = 1
        cnt[distS[v]] += 1

mandatory = bytearray(V)
for v in range(V):
    if on_shortest[v] and cnt[distS[v]] == 1:
        mandatory[v] = 1

tin = array('i', [0]) * V
low = array('i', [0]) * V
parent = array('i', [-1]) * V
contains_t = bytearray(V)
st_cut = bytearray(V)

timer = 0

stack = [(start, 0)]
parent[start] = start

while stack:
    v, it = stack[-1]

    if tin[v] == 0:
        timer += 1
        tin[v] = low[v] = timer
        if v == target:
            contains_t[v] = 1

    r = v // m
    c = v - r * m

    neigh = []

    if r:
        neigh.append(v - m)
    if r + 1 < n:
        neigh.append(v + m)
    if c:
        neigh.append(v - 1)
    if c + 1 < m:
        neigh.append(v + 1)

    advanced = False

    while it < len(neigh):
        to = neigh[it]
        it += 1
        stack[-1] = (v, it)

        if not free[to]:
            continue

        if tin[to] == 0:
            parent[to] = v
            stack.append((to, 0))
            advanced = True
            break

        if to != parent[v]:
            if tin[to] < low[v]:
                low[v] = tin[to]

    if advanced:
        continue

    stack.pop()

    if v != start:
        p = parent[v]

        if contains_t[v] and low[v] >= tin[p]:
            st_cut[p] = 1

        if low[v] < low[p]:
            low[p] = low[v]

        if contains_t[v]:
            contains_t[p] = 1

ans = [list(row) for row in g]

for v in range(V):
    if v == start or v == target:
        continue

    if mandatory[v] and not st_cut[v]:
        r = v // m
        c = v - r * m
        ans[r][c] = '*'

print("\n".join("".join(row) for row in ans))
```Sau hai lần chạy BFS, mọi ô trống đều biết khoảng cách của nó từ cả hai đầu. Sự bình đẳng```
distS[v] + distT[v] = D
```lọc chính xác các ô tham gia vào ít nhất một đường dẫn ngắn nhất. 

Mảng đếm lớp lưu trữ số lượng ô có đường dẫn ngắn nhất xuất hiện ở mỗi khoảng cách kể từ đầu. Số lượng một có nghĩa là lớp đó bị ép buộc. 

DFS là một phương pháp truyền tải liên kết thấp tiêu chuẩn. phần bổ sung`contains_t`cờ theo dõi xem cây con DFS có chứa đỉnh kết thúc hay không. Khi```
low[child] >= tin[parent]
```và cây con con chứa phần kết thúc, loại bỏ phần bắt đầu và kết thúc phân tách phần gốc. Những đỉnh đó được loại trừ. 

Việc triển khai sử dụng DFS lặp để tránh các vấn đề về độ sâu đệ quy trên các lưới chứa tới một triệu ô. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
4 5
.....
.XX..
.X...
...X.
```Khoảng cách ngắn nhất là 7. 

| Lớp | Các ô có đường dẫn ngắn nhất | 
| --- | --- | 
| 0 | (0,0) | 
| 1 | (0,1) | 
| 2 | (0,2) | 
| 3 | (0,3) | 
| 4 | (0,4) | 
| 5 | (1,4) | 
| 6 | (2,4) | 
| 7 | (3,4) | 

Lớp 1, 2 và 3 chứa một ô đường dẫn ngắn nhất duy nhất. Không ai trong số họ ngắt kết nối mê cung hoàn toàn, vì vậy họ trở thành`*`. 

Đầu ra:```
.***.
.XX..
.X...
...X.
```Ví dụ này cho thấy ý tưởng cốt lõi: mọi con đường ngắn nhất đều phải đi qua hành lang trên cùng, nhưng vẫn tồn tại những đường vòng dài hơn. 

### Ví dụ 2```
3 3
...
...
...
```Khoảng cách ngắn nhất là 4. 

| Lớp | Số ô có đường dẫn ngắn nhất | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 2 | 
| 4 | 1 | 

Không có lớp bên trong nào chứa chính xác một ô. 

Không có ô nào thuộc về mọi đường dẫn ngắn nhất, vì vậy đầu ra không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Hai lần truyền tải BFS và một lần truyền tải DFS | 
| Không gian | O(NM) | Khoảng cách, mảng liên kết thấp và bộ nhớ phụ trợ | 

Lưới chứa tối đa một triệu ô. Mỗi ô và mỗi cạnh lưới chỉ được xử lý với số lần không đổi, phù hợp với giới hạn đã định. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string

# sample-like case
assert run("""4 5
.....
.XX..
.X...
...X.
""") == """.***.
.XX..
.X...
...X.
"""

# minimum grid
assert run("""1 1
.
""") == """.
"""

# completely open 3x3 grid
assert run("""3 3
...
...
...
""") == """...
...
...
"""

# single corridor, removing middle disconnects
assert run("""1 5
.....
""") == """.....
"""

# forced shortest path but longer detour exists
assert run("""3 5
.....
.XXX.
.....
""") == """.....
.XXX.
.....
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | không thay đổi | Bắt đầu bằng kết thúc | 
| Mở lưới 3×3 | không thay đổi | Nhiều con đường ngắn nhất | 
| Hành lang đơn | không thay đổi | Đường dẫn vẫn phải tồn tại sau khi chặn | 
| Hành lang có đường vòng | kiểm tra số sao hợp lệ | Tăng khoảng cách ngắn nhất mà không bị ngắt kết nối | 

## Vỏ cạnh 

Một ô có thể thuộc mọi đường dẫn ngắn nhất và vẫn không hợp lệ. 

Ví dụ:```
.....
XXXXX
.....
```Kết nối duy nhất giữa điểm bắt đầu và điểm kết thúc là không thể xuyên qua hàng rào tường. Bất kỳ ô thắt cổ chai nào cũng sẽ ngắt kết nối hoàn toàn biểu đồ khi bị xóa. Pha liên kết thấp phát hiện rằng đó là một`s-t`cắt đỉnh, vì vậy nó không được đánh dấu. 

Một trường hợp tinh tế khác là một mê cung có nhiều đường đi ngắn nhất có độ dài bằng nhau:```
...
...
...
```Ô trung tâm tham gia vào nhiều con đường ngắn nhất, nhưng không phải tất cả chúng. Lớp của nó chứa nhiều ô có đường dẫn ngắn nhất, vì vậy nó không được coi là bắt buộc và không được đánh dấu. 

Cuối cùng, hãy xem xét một đỉnh có đường đi ngắn nhất bắt buộc có đường vòng dài hơn xung quanh nó:```
.....
.XXX.
.....
```Lớp của nó chứa chính xác một ô đường dẫn ngắn nhất, vì vậy mọi đường dẫn ngắn nhất đều sử dụng nó. Việc loại bỏ nó sẽ để lại một tuyến đường dài hơn qua hàng dưới cùng. Khoảng cách ngắn nhất tăng lên và đường đi vẫn tồn tại, đó chính xác là tình huống mà thuật toán đánh dấu`*`.
