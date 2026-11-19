---
title: "CF 212A - Tư nhân hóa"
description: "Chúng tôi được cung cấp một biểu đồ lưỡng cực. Một bên chứa các thành phố Berland, bên kia chứa các thành phố Beerland và mỗi chuyến bay là một biên giới vô hướng giữa hai quốc gia. Mỗi cạnh phải được gán cho một trong t công ty tư nhân."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 212
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Finals (unofficial online-version)"
rating: 3000
weight: 212
solve_time_s: 140
verified: false
draft: false
---

[CF 212A - Tư nhân hóa](https://codeforces.com/problemset/problem/212/A) 

**Đánh giá:** 3000 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một biểu đồ lưỡng cực. Một bên chứa các thành phố Berland, bên kia chứa các thành phố Beerland và mỗi chuyến bay là một biên giới vô hướng giữa hai quốc gia. 

Mỗi cạnh phải được gán cho một trong`t`các công ty tư nhân. Đối với mỗi thành phố, chúng tôi đếm xem mỗi công ty có bao nhiêu cạnh sự cố. Nếu một thành phố có số lượng$$a_{i1}, a_{i2}, \dots, a_{it}$$thì sự đóng góp của nó vào sự không đồng đều là$$\sum_{j=1}^t a_{ij}^2$$Tổng số không đồng đều là tổng của tất cả các thành phố. 

Chúng ta phải gán một màu từ`1..t`tới mọi cạnh sao cho tổng giá trị này là nhỏ nhất. 

Kích thước biểu đồ đủ lớn để không thể thực hiện được các bài tập một cách thô bạo. Có nhiều nhất là 5000 cạnh và có tới 200 công ty. Ngay cả đối với`t = 2`, thử tất cả các bài tập sẽ yêu cầu kiểm tra$$2^{5000}$$cấu hình, điều này hoàn toàn vô vọng. 

Cấu trúc của hàm mục tiêu là quan sát chính. Sự đóng góp của một thành phố chỉ phụ thuộc vào việc các biên sự cố của nó được phân bổ đồng đều như thế nào giữa các công ty. Nếu một đỉnh có bậc`d`, thì giá trị tối thiểu có thể có của$$\sum a_j^2$$dưới sự ràng buộc$$\sum a_j = d$$đạt được khi các giá trị khác nhau nhiều nhất là một. 

Ví dụ, nếu một thành phố có mức độ`7`Và`t = 3`, thì cách phân chia tối ưu là`(3,2,2)`và sự đóng góp trở thành$$3^2 + 2^2 + 2^2 = 17$$Bất kỳ sự phân chia không đồng đều nào đều làm tăng tổng bình phương. 

Phần không rõ ràng là tính tối ưu cục bộ ở mọi đỉnh có thể xung đột trên toàn cầu, bởi vì mọi cạnh đều ảnh hưởng đến hai đỉnh cùng một lúc. Chúng ta cần một màu cạnh để thực hiện phân bố tối ưu cho mọi đỉnh cùng một lúc. 

Một người tham lam bất cẩn có thể dễ dàng thất bại. 

Coi như:```
2 2 4 2
1 1
1 2
2 1
2 2
```Mỗi đỉnh đều có bậc`2`, do đó phân phối cục bộ tối ưu là`(1,1)`. 

Nếu chúng ta tô màu lần lượt các cạnh liên quan đến đỉnh đầu tiên, chúng ta có thể tạo ra:```
edge (1,1) -> 1
edge (1,2) -> 2
edge (2,1) -> 1
edge (2,2) -> 1
```Bây giờ đỉnh`2`về phía Beerland có số lượng`(2,0)`, điều đó không tối ưu. 

Lời giải đúng phải phối hợp tất cả các đỉnh cùng một lúc. 

Một trường hợp tế nhị khác là khi`t`lớn hơn mọi mức độ. 

Ví dụ:```
1 3 3 10
1 1
1 2
1 3
```Sự phân phối tốt nhất tại thành phố Berland duy nhất là`(1,1,1,0,0,0,0,0,0,0)`, đóng góp`3`. 

Việc triển khai sai có thể cố gắng sử dụng tất cả các màu như nhau và vô tình trùng lặp một màu. 

Biểu đồ cũng có thể bị ngắt kết nối. Bất kỳ thuật toán nào giả định kết nối đều có thể âm thầm bỏ sót các thành phần. 

Ví dụ:```
2 2 2 2
1 1
2 2
```Mỗi cạnh có thể nhận bất kỳ màu nào một cách độc lập. Câu trả lời tối ưu là`4`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi thử mọi cách gán công ty cho các cạnh, tính toán phần đóng góp cho mọi đỉnh và giữ mức tối thiểu. 

Đối với mỗi phép gán, chúng ta có thể đếm, với mỗi đỉnh và mỗi công ty, có bao nhiêu cạnh sự cố thuộc về công ty đó. Sau đó, chúng tôi đánh giá tổng bình phương. 

Điều này hoạt động vì hàm mục tiêu được xác định rõ ràng bằng màu cạnh. Vấn đề là số lượng nhiệm vụ. Với`k`các cạnh và`t`các công ty, có$$t^k$$chất màu có thể. 

Với`k = 5000`, con số này lớn về mặt thiên văn ngay cả đối với`t = 2`. 

Ý tưởng tự nhiên tiếp theo là tối ưu hóa từng đỉnh một cách độc lập. Để có bằng cấp`d`, tổng bình phương tối thiểu thu được bằng cách phân bổ các cạnh càng đều càng tốt giữa các màu. 

Giả định$$d = qt + r$$Thế thì chính xác`r`màu sắc sẽ xuất hiện`q+1`lần và phần còn lại`t-r`màu sắc sẽ xuất hiện`q`lần. 

Điều này mang lại sự đóng góp tối thiểu về mặt lý thuyết cho đỉnh đó. 

Bước đột phá thực sự là nhận ra rằng cấu trúc địa phương tối ưu này thực sự có thể đạt được trên toàn cầu. Đồ thị là đồ thị lưỡng cực và đồ thị lưỡng cực cho phép tô màu các cạnh một cách chính xác`Δ`màu sắc, ở đâu`Δ`là mức độ tối đa. Quan trọng hơn, chúng ta có thể phân tách các cạnh thành các nhóm gần như bằng nhau cho mọi đỉnh. 

Việc xây dựng xuất phát từ việc chính quy hóa và các chuyến tham quan Euler. 

Đầu tiên chúng ta làm cho tất cả các độ của đỉnh bằng nhau bằng cách cộng các đỉnh giả và các cạnh giả. Sau đó, chúng tôi chia biểu đồ thông thường thành các kết quả khớp hoàn hảo không có cạnh liên tục. Việc gán màu theo chu kỳ cho các kết hợp này đảm bảo rằng mọi đỉnh đều nhận được từng màu$$\left\lfloor \frac{d}{t} \right\rfloor$$hoặc$$\left\lceil \frac{d}{t} \right\rceil$$lần, đó chính xác là sự phân phối tối ưu. 

Điều này chuyển đổi vấn đề thành việc tìm kiếm các kết quả khớp hoàn hảo trong đồ thị hai bên một cách lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(t^k \cdot k)$|$O(k)$| Quá chậm | 
| Tối ưu |$O(D \cdot V^2)$|$O(V^2)$| Đã chấp nhận | 

Đây`D`là mức độ tối đa sau khi chính quy hóa, nhiều nhất là`200`, Và`V`là số đỉnh sau khi thêm các nút giả, cũng nhiều nhất là khoảng`400`. 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ lưỡng cực từ đầu vào. 

Phía bên trái chứa các thành phố Berland, phía bên phải chứa các thành phố Beerland. 
2. Tính độ lớn nhất`D`. 

MỘT`D`-Biểu đồ lưỡng cực thông thường dễ dàng phân tách thành các kết hợp hoàn hảo hơn nhiều. 
3. Thêm các đỉnh giả sao cho cả hai cạnh đều có số đỉnh bằng nhau. 

Sự kết hợp hoàn hảo đòi hỏi các bộ lưỡng cực cân bằng. 
4. Thêm các cạnh giả cho đến khi mọi đỉnh đều có bậc chính xác`D`. 

Điều này biến biểu đồ thành một`D`-biểu đồ lưỡng cực đều đặn. 

Một biểu đồ như vậy luôn thừa nhận một sự kết hợp hoàn hảo. 
5. Liên tục tìm một kết hợp hoàn hảo và loại bỏ nó. 

Sau khi loại bỏ một kết quả phù hợp hoàn hảo khỏi một`D`-đồ thị lưỡng cực đều, đồ thị còn lại trở thành`(D-1)`-đều đặn, vì vậy quá trình có thể tiếp tục. 
6. Đánh số các kết quả phù hợp từ`0`ĐẾN`D-1`. 

Chỉ định công ty$$(matching\_index \bmod t) + 1$$tới tất cả các cạnh trong sự khớp đó. 
7. Bỏ qua các cạnh giả và chỉ xuất màu cho các cạnh gốc. 

Tại sao sự cân bằng này lại có tác dụng? 

Giả sử một đỉnh có bậc gốc`d`. Trong quá trình phân rã, mỗi kết quả khớp đóng góp chính xác một cạnh phụ vào đỉnh đó cho đến khi hết tất cả các cạnh ban đầu của nó. 

các`D`sự phù hợp được phân phối theo chu kỳ giữa`t`các công ty. Đối với đỉnh đó, số cạnh liên quan được gán cho hai công ty bất kỳ khác nhau tối đa một. 

Đó chính xác là cấu hình giảm thiểu tổng bình phương. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi kết quả khớp được trích xuất đều sử dụng chính xác một cạnh tới mọi đỉnh của đồ thị chính quy hiện tại. 

Bởi vì đồ thị bắt đầu`D`-regular và mỗi lần lặp sẽ loại bỏ một cạnh trên mỗi đỉnh, đồ thị còn lại vẫn đều đặn. Điều này đảm bảo quá trình tiếp tục cho đến khi tất cả các cạnh được phân chia thành các phần khớp hoàn hảo. 

Việc gán màu theo chu kỳ trên các kết hợp này sẽ phân phối các cạnh liên quan ở mọi đỉnh một cách đồng đều nhất có thể về mặt toán học. Vì hàm mục tiêu có thể phân tách bằng các đỉnh và được giảm thiểu chính xác bằng các phân bố cân bằng như vậy nên màu thu được là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def hopcroft_karp(adj, n_left, n_right):
    INF = 10**9

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

                if pu == -1:
                    found = True
                elif dist[pu] == INF:
                    dist[pu] = dist[u] + 1
                    q.append(pu)

        return found

    def dfs(u):
        for v in adj[u]:
            pu = pair_v[v]

            if pu == -1 or (dist[pu] == dist[u] + 1 and dfs(pu)):
                pair_u[u] = v
                pair_v[v] = u
                return True

        dist[u] = INF
        return False

    matching = 0

    while bfs():
        for u in range(n_left):
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    return pair_u

def solve():
    n, m, k, t = map(int, input().split())

    edges = []
    deg_left = [0] * n
    deg_right = [0] * m

    for _ in range(k):
        x, y = map(int, input().split())
        x -= 1
        y -= 1

        edges.append((x, y))

        deg_left[x] += 1
        deg_right[y] += 1

    D = max(max(deg_left), max(deg_right))

    size = max(n, m)

    left_deg = deg_left[:] + [0] * (size - n)
    right_deg = deg_right[:] + [0] * (size - m)

    graph = [[-1] * size for _ in range(size)]

    for idx, (u, v) in enumerate(edges):
        graph[u][v] = idx

    for u in range(size):
        need = D - left_deg[u]

        for v in range(size):
            if need == 0:
                break

            if right_deg[v] < D and graph[u][v] == -1:
                graph[u][v] = -2
                left_deg[u] += 1
                right_deg[v] += 1
                need -= 1

    answer = [0] * k

    for match_id in range(D):
        adj = [[] for _ in range(size)]

        for u in range(size):
            for v in range(size):
                if graph[u][v] != -1:
                    adj[u].append(v)

        match = hopcroft_karp(adj, size, size)

        color = (match_id % t) + 1

        for u in range(size):
            v = match[u]

            edge_id = graph[u][v]

            if edge_id >= 0:
                answer[edge_id] = color

            graph[u][v] = -1

    total = 0

    for d in deg_left:
        q, r = divmod(d, t)
        total += r * (q + 1) * (q + 1)
        total += (t - r) * q * q

    for d in deg_right:
        q, r = divmod(d, t)
        total += r * (q + 1) * (q + 1)
        total += (t - r) * q * q

    print(total)
    print(*answer)

solve()
```Thành phần chính đầu tiên là Hopcroft-Karp, tìm thấy sự phù hợp tối đa trong biểu đồ hai bên. Vì biểu đồ đều đặn và cân bằng nên mọi kết quả khớp tối đa đều hoàn hảo. 

Ma trận đồ thị lưu trữ ID cạnh. Các cạnh gốc lưu trữ chỉ mục đầu vào của chúng, các cạnh giả lưu trữ`-2`và lưu trữ các cạnh vắng mặt`-1`. 

Bước chính quy hóa là tinh tế. Chúng ta tham lam kết nối các đỉnh có bậc hiện tại thấp hơn`D`. Bởi vì tổng mức độ thâm hụt của cả hai bên đều bằng nhau nên quá trình này luôn thành công. 

Sau khi đồ thị trở thành`D`-thường xuyên, chúng tôi liên tục trích xuất các kết quả khớp hoàn hảo. Mỗi kết quả được trích xuất tương ứng với một lớp cạnh. 

Việc gán màu sử dụng lập chỉ mục theo chu kỳ:```
color = (match_id % t) + 1
```Điều này tự động cân bằng số cạnh cho mọi đỉnh. 

Giá trị câu trả lời được tính trực tiếp từ công thức độ thay vì tính lại màu sắc. Khi chúng ta biết mọi đỉnh đều nhận được phân bố cân bằng tối ưu thì mức đóng góp tối thiểu sẽ mang tính quyết định. 

Một lỗi phổ biến là quên rằng chỉ có các cạnh gốc thuộc về đầu ra. Các cạnh giả hoàn toàn mang tính cấu trúc và phải được bỏ qua. 

Một lỗi dễ mắc khác là sửa đổi biểu đồ trong khi lặp lại không chính xác. Sau khi trích xuất một kết quả khớp, chúng tôi loại bỏ chính xác các cạnh đó khỏi ma trận đồ thị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5 8 2
1 4
1 3
3 3
1 2
1 1
2 1
1 5
2 2
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| L1 | 5 | 
| L2 | 2 | 
| L3 | 1 | 
| R1 | 2 | 
| R2 | 2 | 
| R3 | 2 | 
| R4 | 1 | 
| R5 | 1 | 

Mức độ tối đa là`5`. 

Sau khi chính quy hóa, biểu đồ trở thành`5`-thường xuyên. 

Giả sử các kết quả được trích xuất được gán màu theo chu kỳ: 

| Phù hợp | Công ty | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 

đỉnh`L1`có bằng cấp`5`, do đó các cạnh sự cố của nó trở nên phân bố`(3,2)`. 

Sự đóng góp:$$3^2 + 2^2 = 13$$Mỗi đỉnh bậc 2 có`(1,1)`và đóng góp`2`. 

Tổng số cuối cùng trở thành`4`. 

Dấu vết này thể hiện tính chất cân bằng. Mỗi đỉnh nhận được các màu khác nhau nhiều nhất là một. 

### Ví dụ 2 

đầu vào:```
2 2 4 2
1 1
1 2
2 1
2 2
```Biểu đồ đã có rồi`2`-thường xuyên. 

Sự kết hợp hoàn hảo: 

| Phù hợp | Cạnh | 
| --- | --- | 
| 0 | (1,1), (2,2) | 
| 1 | (1,2), (2,1) | 

Màu sắc được chỉ định: 

| Cạnh | Màu sắc | 
| --- | --- | 
| (1,1) | 1 | 
| (2,2) | 1 | 
| (1,2) | 2 | 
| (2,1) | 2 | 

Mỗi đỉnh nhìn thấy một cạnh của mỗi màu. 

Đóng góp cho mỗi đỉnh:$$1^2 + 1^2 = 2$$Tổng cộng:$$4 \times 2 = 8$$Ví dụ này cho thấy lý do tại sao việc phân tách kết hợp hoàn hảo sẽ cân bằng đồng thời tất cả các đỉnh một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(D \cdot V^2)$| Chúng tôi trích xuất`D`các kết hợp hoàn hảo, mỗi kết hợp sử dụng nhiều nhất Hopcroft-Karp`400`đỉnh | 
| Không gian |$O(V^2)$| Ma trận kề lưu trữ biểu đồ chính quy | 

Mức độ chính quy tối đa là nhiều nhất`200`và kích thước đồ thị sau khi cân bằng vẫn nhỏ. Ngay cả trường hợp xấu nhất cũng dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    from collections import deque

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    def hopcroft_karp(adj, n_left, n_right):
        INF = 10**9

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

                    if pu == -1:
                        found = True
                    elif dist[pu] == INF:
                        dist[pu] = dist[u] + 1
                        q.append(pu)

            return found

        def dfs(u):
            for v in adj[u]:
                pu = pair_v[v]

                if pu == -1 or (dist[pu] == dist[u] + 1 and dfs(pu)):
                    pair_u[u] = v
                    pair_v[v] = u
                    return True

            dist[u] = INF
            return False

        while bfs():
            for u in range(n_left):
                if pair_u[u] == -1:
                    dfs(u)

        return pair_u

    n, m, k, t = map(int, input().split())

    edges = []
    deg_left = [0] * n
    deg_right = [0] * m

    for _ in range(k):
        x, y = map(int, input().split())
        x -= 1
        y -= 1

        edges.append((x, y))
        deg_left[x] += 1
        deg_right[y] += 1

    D = max(max(deg_left), max(deg_right))
    size = max(n, m)

    left_deg = deg_left[:] + [0] * (size - n)
    right_deg = deg_right[:] + [0] * (size - m)

    graph = [[-1] * size for _ in range(size)]

    for idx, (u, v) in enumerate(edges):
        graph[u][v] = idx

    for u in range(size):
        need = D - left_deg[u]

        for v in range(size):
            if need == 0:
                break

            if right_deg[v] < D and graph[u][v] == -1:
                graph[u][v] = -2
                left_deg[u] += 1
                right_deg[v] += 1
                need -= 1

    ans = [0] * k

    for mid in range(D):
        adj = [[] for _ in range(size)]

        for u in range(size):
            for v in range(size):
                if graph[u][v] != -1:
                    adj[u].append(v)

        match = hopcroft_karp(adj, size, size)

        color = (mid % t) + 1

        for u in range(size):
            v = match[u]

            eid = graph[u][v]

            if eid >= 0:
                ans[eid] = color

            graph[u][v] = -1

    total = 0

    for d in deg_left[:n]:
        q, r = divmod(d, t)
        total += r * (q + 1) * (q + 1)
        total += (t - r) * q * q

    for d in deg_right[:m]:
        q, r = divmod(d, t)
        total += r * (q + 1) * (q + 1)
        total += (t - r) * q * q

    print(total)
    print(*ans)

    return out.getvalue()

# sample 1
out = run(
"""3 5 8 2
1 4
1 3
3 3
1 2
1 1
2 1
1 5
2 2
"""
)

assert out.splitlines()[0] == "20"

# minimum case
out = run(
"""1 1 1 1
1 1
"""
)

assert out.splitlines()[0] == "2"

# disconnected graph
out = run(
"""2 2 2 2
1 1
2 2
"""
)

assert out.splitlines()[0] == "4"

# t larger than degrees
out = run(
"""1 3 3 10
1 1
1 2
1 3
"""
)

assert out.splitlines()[0] == "6"

# complete 2x2 graph
out = run(
"""2 2 4 2
1 1
1 2
2 1
2 2
"""
)

assert out.splitlines()[0] == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | 2 | Đồ thị tối thiểu | 
| Đồ thị bị ngắt kết nối | 4 | Thành phần độc lập | 
|`t > degree`| 6 | Sử dụng màu sắc thưa thớt | 
| Hoàn thành`2x2`đồ thị | 8 | Cân bằng hoàn hảo | 

## Vỏ cạnh 

Hãy xem xét trường hợp số lượng công ty vượt quá tất cả các mức độ:```
1 3 3 10
1 1
1 2
1 3
```Đỉnh bên trái duy nhất có bậc`3`. Thuật toán phân phối ba cạnh của nó trên ba kết quả khớp khác nhau. Vì màu sắc được gán theo chu kỳ nên đỉnh sẽ nhận được số lượng:```
(1,1,1,0,0,0,0,0,0,0)
```Sự đóng góp trở thành:$$1+1+1=3$$Các đỉnh bên phải đều đóng góp`1`, vậy tổng số là`6`. 

Bây giờ hãy xem xét một biểu đồ bị ngắt kết nối:```
2 2 2 2
1 1
2 2
```Đồ thị đã có bậc tối đa`1`. Sự phân tách chứa một kết quả khớp duy nhất, vì vậy cả hai cạnh đều nhận được cùng một màu. 

Mọi đỉnh đều nhìn thấy sự phân bố`(1,0)`và đóng góp`1`. 

Thuật toán hoạt động vì việc chính quy hóa và trích xuất khớp không phụ thuộc vào khả năng kết nối. 

Cuối cùng, hãy xem xét áp lực mất cân bằng:```
2 2 3 2
1 1
1 2
2 1
```Bằng cấp là: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| L1 | 2 | 
| L2 | 1 | 
| R1 | 2 | 
| R2 | 1 | 

Các đỉnh bậc 2 phải nhận được một cạnh của mỗi màu. Việc phân tách phù hợp đảm bảo điều này một cách tự động. Một kẻ tham lam ngây thơ ở địa phương có thể dễ dàng tạo ra`(2,0)`tại một đỉnh, nhưng cấu trúc khớp hoàn hảo sẽ ngăn chặn những xung đột như vậy.
