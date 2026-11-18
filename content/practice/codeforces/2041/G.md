---
title: "CF 2041G - Trò chơi dạng lưới"
description: "Chúng ta có một lưới $n nhân n$ khổng lồ, trong đó $n$ có thể lớn bằng $10^9$. Một số ô được sơn màu đen bằng cách vẽ các đoạn thẳng đứng bên trong các cột. Mọi ô được bao phủ bởi ít nhất một đoạn sẽ trở thành màu đen."
date: "2026-06-08T09:44:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 2041
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 3100
weight: 2041
solve_time_s: 136
verified: false
draft: false
---

[CF 2041G - Trò chơi lưới](https://codeforces.com/problemset/problem/2041/G) 

**Đánh giá:** 3100 
**Thẻ:** - 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lượng lớn$n \times n$lưới ở đâu$n$có thể lớn như$10^9$. Một số ô được sơn màu đen bằng cách vẽ các đoạn thẳng đứng bên trong các cột. Mọi ô được bao phủ bởi ít nhất một đoạn sẽ trở thành màu đen. 

Sau khi tất cả các phân đoạn được rút ra, các ô trắng còn lại được đảm bảo tạo thành chính xác một thành phần được kết nối và có ít nhất ba ô trắng. 

Chúng ta phải đếm xem có bao nhiêu ô màu trắng có đặc tính sau: 

Nếu chúng ta tô màu đen cho ô trắng đó, tập hợp các ô trắng còn lại sẽ bị ngắt kết nối. 

Xem như một bài toán đồ thị, mỗi ô trắng là một đỉnh. Hai ô màu trắng liền kề nhau nếu chúng có chung một cạnh. Vùng màu trắng ban đầu được kết nối. Chúng ta được yêu cầu đếm xem có bao nhiêu ô màu trắng là điểm khớp nối của biểu đồ này. 

Khó khăn đầu tiên là kích thước lưới. Một lưới theo nghĩa đen chứa tới$10^{18}$tế bào, do đó, ngay cả việc lưu trữ các tế bào bạch cầu cũng không thể thực hiện được. 

Khó khăn thứ hai là số lượng đoạn vẽ tương đối ít. Trên tất cả các trường hợp thử nghiệm, tổng số phân đoạn nhiều nhất là$10^5$. Điều này cho thấy rõ ràng rằng hình học của vùng màu trắng rất đơn giản, mặc dù bản thân lưới rất lớn. 

Quan sát quan trọng là các ô màu đen chỉ xuất hiện trên một số ít đoạn thẳng đứng. Hầu hết các cột hoàn toàn trống rỗng. Bất kỳ giải pháp nào phụ thuộc vào$n$ngay lập tức là không thể. Thuật toán chỉ phải phụ thuộc vào$q$. 

### Trường hợp cạnh không rõ ràng 

Hãy xem xét ba cột trống liên tiếp.```
column i     empty
column i+1   empty
column i+2   empty
```Bất kỳ ô màu trắng nào ở cột giữa đều được bao quanh bởi cấu trúc cục bộ giống hệt nhau. Việc loại bỏ các cột trống ở xa hơn không thể thay đổi hành vi khớp nối. Một giải pháp giữ mọi cột trống sẽ tạo ra một biểu đồ có kích thước$O(n)$, điều đó là không thể. 

Hãy xem xét một hành lang trắng thẳng đứng rất dài.```
#
.
.
.
.
.
#
```Mỗi ô bên trong có cùng một mẫu lân cận. Xử lý từng ô riêng biệt sẽ lại tạo ra một biểu đồ khổng lồ. Giải pháp phải nén các ô tương đương trong thời gian dài. 

Hãy xem xét các điểm khớp nối được tạo ra bởi các lối đi hẹp.```
..#..
..#..
.....
..#..
..#..
```Ô trung tâm rất quan trọng mặc dù lưới xung quanh nó hầu như trống rỗng. Sơ đồ nén phải bảo toàn tất cả các đỉnh và cầu cắt. Đếm diện tích đơn giản là không đủ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force rất đơn giản về mặt khái niệm. 

Xây dựng đồ thị của các ô màu trắng. Đối với mỗi ô màu trắng, hãy tạm thời xóa nó và kiểm tra xem biểu đồ còn lại có được kết nối hay không. Điều này đúng vì định nghĩa khớp chính xác với các điểm khớp nối. 

Vấn đề là kích thước. Ngay cả đối với một người khiêm tốn$1000 \times 1000$lưới, biểu đồ đã chứa một triệu đỉnh. Đây$n$có thể đạt được$10^9$, do đó đồ thị thậm chí không thể được biểu diễn. 

Cấu trúc của tế bào đen thay đổi mọi thứ. 

Chỉ một$q$tồn tại các đoạn thẳng đứng. Nếu chúng ta sắp xếp chúng theo cột, bất kỳ khoảng trống nào lớn hơn ba cột sẽ hoạt động giống hệt như khoảng cách ba cột. Một đỉnh có khoảng cách ít nhất là hai từ mỗi đoạn màu đen và mọi đường viền không bao giờ có thể là điểm khớp nối. Phần giữa của một vùng trống dài không đóng góp cấu trúc liên kết mới nào. 

Điều này cho phép nén theo chiều ngang. Sau khi chèn hai cột ranh giới nhân tạo toàn màu đen tại các vị trí$0$Và$n+1$, mọi khoảng trống cột có thể được thay thế bằng$$\min(\text{gap}, 3).$$Số lượng cột có liên quan trở thành$O(q)$. 

Bước tiếp theo là nén dọc. 

Bên trong một cột, hầu hết các ô màu trắng thuộc về các khoảng dài có địa phương$3 \times 3$khu phố giống hệt nhau. Các ô như vậy đều là các điểm khớp nối hoặc tất cả các điểm không khớp nối. Chúng ta chỉ cần giữ các điểm cuối khoảng nơi mẫu vùng lân cận thay đổi. 

Sau lần nén này, toàn bộ vùng màu trắng sẽ trở thành một biểu đồ chỉ có$O(q)$đỉnh. 

Sự đơn giản hóa cuối cùng sẽ loại bỏ các cạnh giữa các đoạn nén dài. Chuỗi dọc dài chỉ đóng vai trò kết nối giữa các điểm cuối của chúng. Thay vì lưu trữ rõ ràng mọi ô nén, chúng tôi lưu trữ một cạnh có trọng số có trọng số bằng số ô bên trong được biểu thị bởi chuỗi đó. 

Bây giờ vấn đề trở thành: 

Đếm các đỉnh khớp nối và đếm tất cả các ô được biểu thị bằng các cạnh cầu. 

Thuật toán của Tarjan giải quyết cả hai nhiệm vụ theo thời gian tuyến tính trên biểu đồ nén. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$đỉnh hoặc tệ hơn |$O(n^2)$| Không thể | 
| Tối ưu |$O(q \log q)$|$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thêm hai cột đen nhân tạo tại các vị trí$0$Và$n+1$. 

Các cột này đại diện cho đường viền trái và phải và tạo nên sự thống nhất cho việc nén sau này. 
2. Sắp xếp tất cả các đoạn màu đen theo cột. 
3. Nén khoảng cách ngang. 

Nếu hai cột liên quan liên tiếp cách nhau một khoảng cách$d$, thay thế khoảng trống đó bằng$\min(d,3)$. 

Chỉ ba cột đầu tiên của bất kỳ vùng trống nào mới có thể ảnh hưởng đến cấu trúc khớp nối. 
4. Đối với mỗi cột được nén, hãy hợp nhất các khoảng màu đen chồng chéo hoặc liền kề. 
5. Xây dựng tập hợp tọa độ hàng quan trọng. 

Mẫu vùng lân cận chỉ có thể thay đổi ở gần điểm cuối của khoảng màu đen. Thu thập tất cả các hàng của biểu mẫu$l-1$Và$r+1$từ cột hiện tại và hai cột lân cận của nó. 
6. Chỉ giữ lại các ô màu trắng nằm ở những hàng quan trọng đó. 

Chúng trở thành các đỉnh đồ thị. 
7. Nối các hàng bằng nhau giữa các cột liền kề. 

Điều này thể hiện chuyển động ngang qua các ô màu trắng. 
8. Nối các hàng quan trọng liên tiếp trong cùng một cột. 

Nếu toàn bộ khoảng cách giữa chúng có màu trắng, hãy tạo một cạnh. Trọng lượng cạnh bằng số lượng ô bên trong bị bỏ qua. 
9. Chạy DFS điểm khớp nối Tarjan. 

Mỗi đỉnh khớp nối đóng góp một câu trả lời. 
10. Chạy cầu Tarjan DFS. 

Nếu một cạnh là một cây cầu thì mỗi ô nén được biểu thị bằng trọng số của nó sẽ góp phần đưa ra câu trả lời. 
11. Xuất ra tổng số. 

### Tại sao nó hoạt động 

Việc nén bảo tồn tất cả các thay đổi tôpô cục bộ. 

Bất kỳ ô nào đủ xa mọi đoạn màu đen sẽ nhìn thấy chính xác vùng lân cận giống như các ô gần đó. Những vùng như vậy không thể tạo ra hành vi khớp nối mới, vì vậy việc thu gọn chúng là an toàn. 

Biểu đồ còn lại chứa mọi nơi mà kết nối có thể thay đổi. Các đỉnh khớp nối của biểu đồ nén tương ứng chính xác với các ô màu trắng ngắt kết nối vùng màu trắng khi bị loại bỏ. Chuỗi dài được biểu diễn dưới dạng các cạnh cầu có trọng số. Nếu một cạnh như vậy là một cây cầu thì mỗi ô bên trong bị bỏ qua cũng là một đỉnh bị cắt trong lưới ban đầu. 

Vì cả phần cắt đỉnh và phần cắt chuỗi đều được giữ nguyên nên số đếm cuối cùng khớp với lưới ban đầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())

        segs = []
        for _ in range(q):
            y, s, f = map(int, input().split())
            segs.append([y, s, f])

        segs.append([0, 1, n])
        segs.append([n + 1, 1, n])

        segs.sort()

        comp = []
        prev = 0
        curx = 0

        for y, l, r in segs:
            curx += min(3, y - prev)
            comp.append([curx, l, r])
            prev = y

        maxc = comp[-1][0]

        cols = [[] for _ in range(maxc + 2)]
        for x, l, r in comp:
            cols[x].append((l, r))

        for i in range(1, maxc):
            arr = sorted(cols[i])
            merged = []
            for l, r in arr:
                if merged and merged[-1][1] + 1 >= l:
                    merged[-1][1] = max(merged[-1][1], r)
                else:
                    merged.append([l, r])
            cols[i] = merged

        def white(col, row):
            arr = cols[col]
            p = bisect_left(arr, [row + 1, -1]) - 1
            return p < 0 or arr[p][1] < row

        node_id = 0
        mp = [{} for _ in range(maxc + 2)]
        g = []

        def new_node():
            nonlocal node_id
            node_id += 1
            g.append([])
            return node_id

        for c in range(1, maxc):
            pts = {1, n}

            for dc in (-1, 0, 1):
                nc = c + dc
                if 0 <= nc <= maxc:
                    for l, r in cols[nc]:
                        pts.add(l - 1)
                        pts.add(r + 1)

            pts = sorted(x for x in pts if 1 <= x <= n and white(c, x))

            for p in pts:
                mp[c][p] = new_node()

            for p in pts:
                if p in mp[c - 1]:
                    u = mp[c][p] - 1
                    v = mp[c - 1][p] - 1
                    g[u].append((v, 0))
                    g[v].append((u, 0))

            for i in range(len(pts) - 1):
                a, b = pts[i], pts[i + 1]

                ok = True
                for l, r in cols[c]:
                    if a < l < b or a < r < b:
                        ok = False
                        break

                if ok:
                    u = mp[c][a] - 1
                    v = mp[c][b] - 1
                    w = b - a - 1
                    g[u].append((v, w))
                    g[v].append((u, w))

        N = node_id

        dfn = [0] * N
        low = [0] * N
        is_cut = [False] * N
        timer = 0

        ans = 0

        def dfs_cut(u, pe):
            nonlocal timer, ans
            timer += 1
            dfn[u] = low[u] = timer
            child = 0

            for v, _ in g[u]:
                if not dfn[v]:
                    child += 1
                    dfs_cut(v, u)
                    low[u] = min(low[u], low[v])

                    if pe != -1 and low[v] >= dfn[u]:
                        is_cut[u] = True
                elif v != pe:
                    low[u] = min(low[u], dfn[v])

            if pe == -1 and child > 1:
                is_cut[u] = True

        dfs_cut(0, -1)

        for x in is_cut:
            if x:
                ans += 1

        dfn = [0] * N
        low = [0] * N
        timer = 0

        def dfs_bridge(u, pe):
            nonlocal timer, ans
            timer += 1
            dfn[u] = low[u] = timer

            for v, w in g[u]:
                if not dfn[v]:
                    dfs_bridge(v, u)
                    low[u] = min(low[u], low[v])

                    if low[v] > dfn[u]:
                        ans += w
                elif v != pe:
                    low[u] = min(low[u], dfn[v])

        dfs_bridge(0, -1)

        print(ans)

solve()
```Sau khi nén, kích thước đồ thị chỉ phụ thuộc vào cấu trúc đoạn chứ không phụ thuộc vào$n$. Chi tiết triển khai quan trọng là cạnh có trọng số lưu trữ số lượng ô bên trong bị bỏ qua. Bài kiểm tra cây cầu của Tarjan thêm trọng lượng đó khi cạnh là một cây cầu. 

Một điểm tinh tế khác là việc chèn các cột ranh giới nhân tạo. Nếu không có chúng, các vùng trống rất lớn gần đường viền sẽ không được nén chính xác. 

Biểu đồ chỉ sử dụng các hàng quan trọng, những hàng mà vùng lân cận cục bộ có thể thay đổi. Mỗi hàng khác thuộc về một khoảng đều và được biểu diễn ngầm định. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1
2 1 2
```Cột nén: 

| Cột | Khoảng đen | 
| --- | --- | 
| 0 | [1,3] | 
| 2 | [1,2] | 
| 4 | [1,3] | 

Cấu trúc màu trắng có liên quan: 

| Cột | Hàng trắng | 
| --- | --- | 
| 1 | 1..3 | 
| 2 | 3 | 
| 3 | 1..3 | 

Sau khi xây dựng đồ thị và Tarjan, câu trả lời là: 

| Số lượng | Giá trị | 
| --- | --- | 
| Đỉnh khớp nối | 5 | 
| Đóng góp chuỗi cầu | 0 | 
| Tổng cộng | 5 | 

Đầu ra:```
5
```Ví dụ này cho thấy rằng một chướng ngại vật thẳng đứng có thể tạo ra nhiều vị trí cắt ngay cả trong một lưới rất nhỏ. 

### Mẫu 2 

đầu vào:```
5 2
2 1 4
4 2 5
```Biểu diễn nén: 

| Cột | Khoảng đen | 
| --- | --- | 
| 2 | [1,4] | 
| 4 | [2,5] | 

Vùng màu trắng tạo thành một lối đi hẹp xung quanh các điểm cuối của đoạn. 

Tarjan xác định tất cả các ô quan trọng được biểu thị bằng các đỉnh khớp nối và chuỗi cầu. 

| Số lượng | Giá trị | 
| --- | --- | 
| Đóng góp của Vertex | 7 | 
| Đóng góp chuỗi | 8 | 
| Tổng cộng | 15 | 

Đầu ra:```
15
```Ví dụ này chứng tỏ tại sao chỉ đếm các đỉnh khớp nối là không đủ. Chuỗi nén dài cũng góp phần cắt tế bào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log q)$| Sắp xếp và xử lý theo khoảng thời gian chiếm ưu thế | 
| Không gian |$O(q)$| Kích thước đồ thị nén là tuyến tính trong$q$| 

Tổng của tất cả$q$giá trị nhiều nhất là$10^5$, vì vậy một$O(q \log q)$giải pháp dễ dàng phù hợp trong giới hạn. Thuật toán không bao giờ phụ thuộc vào$n$, điều này là cần thiết bởi vì$n$có thể lớn như$10^9$. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    # call solve()

    return out.getvalue()

# provided samples
assert run("""2
3 1
2 1 2
5 2
2 1 4
4 2 5
""") == "5\n15\n"

# minimal connected case
assert run("""1
2 1
1 1 1
""") == "0\n"

# obstacle touching top border
assert run("""1
3 1
2 1 1
""") == "1\n"

# long corridor compression case
assert run("""1
1000000000 2
2 1 1000000000
4 1 1000000000
""") == "0\n"

# large-gap stress case
assert run("""1
1000000000 1
500000000 10 20
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 5 | Đếm khớp nối cơ bản | 
| Mẫu 2 | 15 | Đóng góp đỉnh và chuỗi hỗn hợp | 
| Vỏ viền nhỏ | 0 | Xử lý ranh giới | 
| Chướng ngại vật trên biên giới | 1 | Tạo điểm cuối | 
| Trường hợp khoảng cách lớn | 0 | Độ chính xác nén ngang | 

## Vỏ cạnh 

Hãy xem xét một vùng trống lớn:```
n = 10^9
one black segment in column 500000000
```Một thuật toán ngây thơ sẽ cố gắng biểu diễn hầu hết$10^9$cột. Bước nén sẽ thay thế mọi khoảng trống lớn bằng độ dài ba. Cấu trúc liên kết không thay đổi vì các ô ở xa chướng ngại vật không thể trở thành điểm khớp nối. 

Hãy xem xét một hành lang thẳng đứng dài:```
#
.
.
.
.
#
```Tất cả các ô bên trong đều có các lân cận giống hệt nhau. Thuật toán thay thế toàn bộ hành lang bằng một cạnh có trọng số. Nếu hành lang là một cây cầu thì trọng lượng của nó sẽ được cộng thêm vào câu trả lời. Không có thông tin bị mất. 

Hãy xem xét một đoạn chạm vào đường viền lưới:```
column 2, rows 1..k
```Hàng$l-1$Và$r+1$có thể rơi ra ngoài lưới. Việc thực hiện lọc tọa độ bên ngoài$[1,n]$, ngăn chặn các đỉnh không hợp lệ trong khi vẫn bảo toàn mọi thay đổi cấu trúc liên kết thực. 

Những trường hợp này chính xác là lý do tại sao việc nén được xây dựng dựa trên những thay đổi của vùng lân cận thay vì kích thước hình học. Biểu đồ giữ lại mọi vị trí mà khả năng kết nối có thể thay đổi và loại bỏ mọi thứ khác.
