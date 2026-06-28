---
title: "CF 105139C - Lili Thích Đa Giác"
description: "Đầu vào mô tả một tập hợp các hình chữ nhật thẳng hàng theo trục trên một lưới vô hạn. Sau khi áp dụng tất cả chúng, mọi ô lưới được bao phủ bởi ít nhất một hình chữ nhật sẽ trở thành “trần”."
date: "2026-06-27T18:46:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105139
codeforces_index: "C"
codeforces_contest_name: "The 2024 International Collegiate Programming Contest in Hubei Province, China"
rating: 0
weight: 105139
solve_time_s: 62
verified: true
draft: false
---

[CF 105139C - Lili thích đa giác](https://codeforces.com/problemset/problem/105139/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một tập hợp các hình chữ nhật thẳng hàng theo trục trên một lưới vô hạn. Sau khi áp dụng tất cả chúng, mọi ô lưới được bao phủ bởi ít nhất một hình chữ nhật sẽ trở thành “trần”. Hình dạng thu được là sự kết hợp của các hình vuông đơn vị tạo thành một vùng trực giao: các cạnh nằm ngang hoặc dọc và hình dạng có thể chứa các lỗ hoặc nhiều phần bị ngắt kết nối. 

Nhiệm vụ là thay thế vùng có thể phức tạp này bằng một phân vùng thành các hình chữ nhật thẳng hàng theo trục không chồng chéo sao cho mỗi ô đơn vị trống thuộc về chính xác một hình chữ nhật đã chọn. Mục đích là giảm thiểu số lượng hình chữ nhật được sử dụng. 

Một cách hữu ích để diễn đạt lại kết quả đầu ra là chúng ta được yêu cầu lấy một lưới nhị phân (các ô được che phủ hoặc không được che phủ sau khi kết hợp) và phân chia tất cả 1 ô thành số lượng hình chữ nhật con đầy đủ nhỏ nhất, trong đó mỗi hình chữ nhật con chỉ được bao gồm 1 ô và hình chữ nhật là rời rạc. 

Tọa độ lớn nhưng tổng độ phức tạp hình học của ranh giới hợp nhỏ, được giới hạn bởi khoảng 2000 điểm cuối. Điều này ngụ ý rằng sau khi nén tọa độ, lưới kết quả chỉ có vài nghìn ranh giới x và y riêng biệt, do đó có thể quản lý được tổng số ô riêng biệt. Bất kỳ giải pháp nào xây dựng một cấu trúc tỷ lệ với lưới nén đều khả thi, trong khi bất kỳ giải pháp nào phụ thuộc vào độ lớn tọa độ ban đầu đều không thể thực hiện được. 

Một ý tưởng ngây thơ là xử lý từng ô một cách độc lập và cố gắng hợp nhất chúng thành các hình chữ nhật một cách tham lam. Điều này không thành công trong các cấu hình đơn giản trong đó các lựa chọn cục bộ cản trở tính tối ưu toàn cục. Ví dụ: hãy xem xét một vùng hình dấu cộng được tạo thành từ năm ô đơn vị: bất kỳ sự hợp nhất theo chiều ngang hoặc chiều dọc tham lam nào cũng có thể tạo ra các hình chữ nhật bổ sung tùy theo thứ tự quét, mặc dù câu trả lời tối ưu rõ ràng là 5 hoặc ít hơn tùy thuộc vào cấu trúc. Khó khăn chính là hình chữ nhật phải luôn thẳng hàng theo trục và không thể chồng lên nhau, vì vậy các quyết định về việc mở rộng hình chữ nhật theo một hướng sẽ ảnh hưởng đến nhiều vị trí trong tương lai. 

Một trường hợp lỗi tinh vi khác xuất hiện khi một vùng trông giống như một đốm màu được kết nối duy nhất nhưng có những “cầu nối mỏng” buộc phải chia tách. Việc lấp đầy ngây thơ mà nhóm các ô được kết nối không giúp ích gì vì kết nối không liên quan: một hình chữ nhật có thể chỉ bao gồm các phần trông có vẻ rời rạc nếu chúng tạo thành một sản phẩm Descartes hoàn chỉnh chứ không chỉ bất kỳ hình dạng được kết nối nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng liệt kê tất cả các cách phân chia lưới thành hình chữ nhật. Ngay cả khi chúng ta hạn chế chỉ xem xét các hình chữ nhật tối đa bắt đầu từ mỗi ô, số lượng lựa chọn sẽ tăng vọt vì mỗi ô có thể bắt đầu một hình chữ nhật có nhiều chiều cao và chiều rộng có thể, và những lựa chọn này tương tác với nhau một cách kết hợp. Trong một lưới có N ô, điều này nhanh chóng trở thành cấp số nhân. 

Quan sát cấu trúc quan trọng là các hình chữ nhật tương ứng với “các dải ngang nhất quán” tồn tại trên nhiều lát dọc liền kề của lưới. Nếu chúng ta nén tọa độ x, chúng ta có thể xem lưới dưới dạng một chuỗi các tấm thẳng đứng. Bên trong mỗi tấm, các ô bị chiếm đóng sẽ chia thành các khoảng dọc liền kề nhau. Mỗi khoảng như vậy là một khối xây dựng ứng cử viên. 

Bây giờ sự đơn giản hóa quan trọng là mọi hình chữ nhật hợp lệ đều tương ứng với việc lấy một khoảng thẳng đứng như vậy trong một tấm và kéo dài nó qua một số tấm liên tiếp trong đó khoảng cách chính xác như vậy vẫn không thay đổi. Điều này chuyển đổi vấn đề thành kết nối các đoạn khoảng giống hệt nhau trên các cột liền kề.

Chúng ta có thể mô hình hóa điều này như một biểu đồ lớp. Mỗi nút là một đoạn khoảng dọc trong một tấm x cụ thể. Chúng tôi kết nối hai nút trong các tấm liên tiếp nếu chúng đại diện cho cùng một khoảng y. Khi đó, hình chữ nhật chính xác là một đường dẫn trong biểu đồ này di chuyển từ trái sang phải mà không thay đổi khoảng y của nó. Mỗi nút phải thuộc chính xác một đường dẫn như vậy vì mỗi ô đơn vị phải được bao phủ một lần. 

Điều này trở thành bài toán bao phủ đường đi tối thiểu cổ điển trong đồ thị chu kỳ có hướng. Vì các cạnh chỉ đi từ phiến i đến i+1, nên biểu đồ là lưỡng cực giữa các lớp liền kề và độ che phủ đường dẫn tối thiểu bằng số lượng nút trừ đi kích thước khớp tối đa giữa các nút khoảng tương thích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | Hàm mũ | Cao | Quá chậm | 
| Biểu đồ khoảng + khớp tối đa | O(M √M) | O(M) | Đã chấp nhận | 

Ở đây M là số đoạn khoảng sau khi nén, được giới hạn bởi tổng kích thước ranh giới. 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các tọa độ x và y xuất hiện dưới dạng ranh giới hình chữ nhật, bao gồm cả hai điểm cuối cũng như ranh giới “phải + 1” và “trên cùng + 1” để thể hiện chính xác phạm vi bao phủ trên lưới nén. Bước này đảm bảo mọi ô đơn vị đều trở thành ô sạch trong biểu diễn nén. 
2. Sắp xếp và nén tọa độ sao cho mỗi ô gốc ánh xạ tới một cặp chỉ mục trong một lưới nhỏ. Sau khi nén, mỗi hình chữ nhật ban đầu sẽ trở thành một khối ô được lấp đầy trong lưới thu nhỏ này. 
3. Xây dựng lưới nhị phân đánh dấu xem mỗi ô nén có được bao phủ bởi ít nhất một hình chữ nhật đầu vào hay không. Điều này được thực hiện bằng cách đánh dấu từng hình chữ nhật trong lưới sai phân hoặc bằng cách lặp trực tiếp trên các phạm vi được nén. 
4. Đối với mỗi tấm x giữa các tọa độ x được nén liên tiếp, quét theo chiều dọc dọc theo y và chia tấm thành các đoạn ô liền kề tối đa. Mỗi đoạn như vậy sẽ trở thành một nút đại diện cho một “sọc dọc” ứng cử viên. 
5. Gán cho mỗi nút một nhãn bao gồm chỉ số bản và khoảng y của nó. Các nút được nhóm lại một cách tự nhiên theo chỉ số phiến, tạo thành các lớp. 
6. Xây dựng các cạnh giữa các nút trong bản i và bản i+1 bất cứ khi nào hai nút có khoảng y giống hệt nhau. Điều này thể hiện khả năng mở rộng hình chữ nhật theo chiều ngang mà không thay đổi phạm vi bao phủ theo chiều dọc của nó. 
7. Chạy đối sánh lưỡng cực tối đa trên các cạnh này, xử lý các nút trong các bảng chẵn là một bên và các bảng lẻ là bên kia. Mỗi cạnh phù hợp sẽ hợp nhất hai nút vào cùng một đường dẫn hình chữ nhật. 
8. Tính toán câu trả lời cuối cùng bằng số nút trừ đi kích thước của kết quả khớp tối đa. Mỗi nút chưa khớp bắt đầu một đường dẫn hình chữ nhật mới và mỗi cạnh phù hợp sẽ giảm số lượng đường dẫn bằng cách hợp nhất tính liên tục. 

### Tại sao nó hoạt động 

Mỗi nút tương ứng với một đoạn dọc tối đa không thể mở rộng theo chiều dọc nếu không rời khỏi vùng được lấp đầy. Bất kỳ hình chữ nhật nào cũng phải tôn trọng các ranh giới dọc tối đa này trong mỗi tấm, vì vậy nó chỉ có thể di chuyển theo chiều ngang bằng cách nằm trong các phân đoạn giống hệt nhau. Do đó, hình chữ nhật tương ứng chính xác với các đường đi qua các đoạn giống hệt nhau trên các tấm. Số lượng đường dẫn tối thiểu cần thiết để bao phủ tất cả các nút chính xác là số lượng hình chữ nhật tối thiểu và việc giảm độ che phủ đường dẫn tiêu chuẩn đảm bảo tính chính xác thông qua kết hợp tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque

def hopcroft_karp(adj, n_left, n_right):
    INF = 10**18
    pair_u = [-1] * n_left
    pair_v = [-1] * n_right
    dist = [0] * n_left

    def bfs():
        q = deque()
        for u in range(n_left):
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = INF

        found = False

        while q:
            u = q.popleft()
            for v in adj[u]:
                pu = pair_v[v]
                if pu != -1 and dist[pu] == INF:
                    dist[pu] = dist[u] + 1
                    q.append(pu)
                elif pu == -1:
                    found = True

        return found

    def dfs(u):
        for v in adj[u]:
            pu = pair_v[v]
            if pu == -1 or (dist[pu] == dist[u] + 1 and dfs(pu)):
                pair_u[u] = v
                pair_v[v] = u
                return True
        dist[u] = float('inf')
        return False

    match = 0
    while bfs():
        for u in range(n_left):
            if pair_u[u] == -1:
                if dfs(u):
                    match += 1
    return match

n = int(input())
rects = []
xs, ys = set(), set()

for _ in range(n):
    l, b, r, t = map(int, input().split())
    rects.append((l, b, r, t))
    xs.add(l); xs.add(r + 1)
    ys.add(b); ys.add(t + 1)

xs = sorted(xs)
ys = sorted(ys)

x_id = {x:i for i, x in enumerate(xs)}
y_id = {y:i for i, y in enumerate(ys)}

H = len(ys)
W = len(xs)

grid = [[0] * (H - 1) for _ in range(W - 1)]

for l, b, r, t in rects:
    xl = x_id[l]
    xr = x_id[r + 1]
    yb = y_id[b]
    yt = y_id[t + 1]
    for i in range(xl, xr):
        for j in range(yb, yt):
            grid[i][j] = 1

nodes = []
node_id = {}
slab_nodes = [[] for _ in range(W - 1)]

for i in range(W - 1):
    j = 0
    while j < H - 1:
        if grid[i][j] == 0:
            j += 1
            continue
        start = j
        while j < H - 1 and grid[i][j] == 1:
            j += 1
        nodes.append((i, start, j - 1))
        node_id[(i, start, j - 1)] = len(nodes) - 1
        slab_nodes[i].append(len(nodes) - 1)

adj = defaultdict(list)

for i in range(W - 2):
    for u in slab_nodes[i]:
        x, y1, y2 = nodes[u]
        for v in slab_nodes[i + 1]:
            x2, z1, z2 = nodes[v]
            if y1 == z1 and y2 == z2:
                adj[u].append(v)

# bipartite: split by slab parity
left = [i for i, (x, _, _) in enumerate(nodes) if x % 2 == 0]
right = [i for i, (x, _, _) in enumerate(nodes) if x % 2 == 1]

right_index = {v:i for i, v in enumerate(right)}

adj_bip = [[] for _ in range(len(left))]

for i, u in enumerate(left):
    for v in adj[u]:
        if v in right_index:
            adj_bip[i].append(right_index[v])

match = hopcroft_karp(adj_bip, len(left), len(right))

print(len(nodes) - match)
```Việc triển khai trước tiên sẽ nén tọa độ để hình học trở thành một lưới hữu hạn. Sau đó, nó xây dựng các nút khoảng dọc bên trong mỗi tấm x. Việc xây dựng liền kề có chủ đích nghiêm ngặt: chỉ các khoảng y giống hệt nhau mới được kết nối, bởi vì bất kỳ sự không khớp nào cũng sẽ phá vỡ tính hợp lệ của hình chữ nhật. 

Việc so khớp được áp dụng trên sự phân chia hai bên theo tính chẵn lẻ của tấm, điều này hợp lệ vì các cạnh chỉ kết nối các tấm liền kề, đảm bảo không có xung đột giữa các phần. 

Cuối cùng, trừ đi kích thước phù hợp tối đa từ số nút sẽ tạo ra số lượng hình chữ nhật tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (khối đơn vị tạo thành hình chữ thập) 

| Bước | Các nút được tạo | Các cạnh phù hợp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Sau khi nén | mỗi ô là nút riêng của nó | không | 8 | 

Mỗi nút đều bị cô lập vì không có hai tấm liền kề nào có khoảng dọc giống hệt nhau. Thuật toán không tạo ra kết quả trùng khớp, do đó, mỗi nút sẽ trở thành hình chữ nhật của riêng nó, phù hợp với nhu cầu trực quan để bao phủ từng ô riêng biệt. 

Điều này xác nhận rằng các thành phần đơn vị bị ngắt kết nối không thể được hợp nhất thành các hình chữ nhật lớn hơn. 

### Ví dụ 2 (hai hình chữ nhật lớn cách nhau) 

| Bước | Các nút được tạo | Các cạnh phù hợp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Sau khi nén | hai chuỗi khoảng cách dài | các cạnh chuỗi trong mỗi hình chữ nhật | 2 | 

Mỗi hình chữ nhật tạo thành một chuỗi liên tục gồm các đoạn thẳng đứng giống hệt nhau trên các tấm. Mỗi nút bên trong chuỗi được khớp với nút lân cận của nó, thu gọn mỗi chuỗi thành một đường dẫn duy nhất. Số đường đi bằng số hình chữ nhật độc lập. 

Điều này cho thấy các vùng ổn định lâu dài thu gọn một cách tối ưu thành một hình chữ nhật duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M √M) | Hopcroft-Karp trên biểu đồ kề khoảng với M nút | 
| Không gian | O(M) | lưu trữ cho các ô lưới, nút và vùng lân cận | 

Tổng số nút M được giới hạn bởi kích thước ranh giới nén, nhiều nhất là vài nghìn, giúp cho việc xây dựng lưới và kết hợp dễ dàng đủ nhanh trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict, deque

    def hopcroft_karp(adj, n_left, n_right):
        INF = 10**18
        pair_u = [-1] * n_left
        pair_v = [-1] * n_right
        dist = [0] * n_left

        def bfs():
            q = deque()
            for u in range(n_left):
                if pair_u[u] == -1:
                    dist[u] = 0
                    q.append(u)
                else:
                    dist[u] = INF
            found = False
            while q:
                u = q.popleft()
                for v in adj[u]:
                    pu = pair_v[v]
                    if pu != -1 and dist[pu] == INF:
                        dist[pu] = dist[u] + 1
                        q.append(pu)
                    elif pu == -1:
                        found = True
            return found

        def dfs(u):
            for v in adj[u]:
                pu = pair_v[v]
                if pu == -1 or (dist[pu] == dist[u] + 1 and dfs(pu)):
                    pair_u[u] = v
                    pair_v[v] = u
                    return True
            dist[u] = float('inf')
            return False

        match = 0
        while bfs():
            for u in range(n_left):
                if pair_u[u] == -1:
                    if dfs(u):
                        match += 1
        return match

    n = int(input())
    rects = []
    xs, ys = set(), set()

    for _ in range(n):
        l, b, r, t = map(int, input().split())
        rects.append((l, b, r, t))
        xs.add(l); xs.add(r + 1)
        ys.add(b); ys.add(t + 1)

    xs = sorted(xs)
    ys = sorted(ys)

    x_id = {x:i for i, x in enumerate(xs)}
    y_id = {y:i for i, y in enumerate(ys)}

    W, H = len(xs), len(ys)

    grid = [[0] * (H - 1) for _ in range(W - 1)]

    for l, b, r, t in rects:
        xl, xr = x_id[l], x_id[r + 1]
        yb, yt = y_id[b], y_id[t + 1]
        for i in range(xl, xr):
            for j in range(yb, yt):
                grid[i][j] = 1

    nodes = []
    slab_nodes = [[] for _ in range(W - 1)]

    for i in range(W - 1):
        j = 0
        while j < H - 1:
            if grid[i][j] == 0:
                j += 1
                continue
            s = j
            while j < H - 1 and grid[i][j]:
                j += 1
            nodes.append((i, s, j - 1))
            slab_nodes[i].append(len(nodes) - 1)

    adj = defaultdict(list)
    for i in range(W - 2):
        for u in slab_nodes[i]:
            x, y1, y2 = nodes[u]
            for v in slab_nodes[i + 1]:
                x
```
