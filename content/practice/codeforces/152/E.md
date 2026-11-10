---
title: "CF 152E - Sân vườn"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đều có chi phí, số lượng hoa bị phá hủy nếu chúng ta lát ô đó bằng bê tông. Trong số tất cả các ô, có tới bảy ô đặc biệt chứa các tòa nhà quan trọng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 152
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 108 (Div. 2)"
rating: 2500
weight: 152
solve_time_s: 144
verified: true
draft: false
---

[CF 152E - Khu vườn](https://codeforces.com/problemset/problem/152/E) 

**Đánh giá:** 2500 
**Thẻ:** bitmask, dp, đồ thị, cây 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đều có chi phí, số lượng hoa bị phá hủy nếu chúng ta lát ô đó bằng bê tông. Trong số tất cả các ô, có tới bảy ô đặc biệt chứa các tòa nhà quan trọng. Chúng ta phải chọn một tập hợp các ô được lát đá được kết nối sao cho mọi ô quan trọng đều thuộc về vùng được lát gạch và mọi ô quan trọng có thể tiếp cận các ô khác bằng cách di chuyển 4 hướng qua các ô được lát gạch. 

Mục tiêu là giảm thiểu tổng chi phí của tất cả các ô được lát. 

Một cách hữu ích để suy nghĩ về lưới là sử dụng biểu đồ có trọng số. Mỗi ô là một đỉnh, các ô liền kề được kết nối bằng các cạnh và việc đi vào một ô sẽ tính số lượng hoa của ô đó. Chúng tôi đang tìm kiếm một đồ thị con được kết nối với chi phí tối thiểu chứa tất cả các thiết bị đầu cuối. Đây chính xác là bài toán Cây Steiner trên biểu đồ lưới. 

Lưới có thể chứa tối đa 200 ô, đủ nhỏ cho các thuật toán đồ thị trên tất cả các đỉnh. Số lượng thiết bị đầu cuối nhiều nhất là 7, đây là manh mối thực sự. Một bài toán chung về Cây Steiner là NP-hard, nhưng khi số lượng thiết bị đầu cuối nhỏ thì lập trình động tập hợp con trở nên khả thi. 

Một phép liệt kê tập hợp con đơn giản trên tất cả các tập hợp con lưới sẽ yêu cầu kiểm tra tới$2^{200}$trạng thái, hoàn toàn không thể. Ngay cả việc liệt kê tất cả các cây hoặc tất cả các đường dẫn giữa các thiết bị đầu cuối cũng bùng nổ theo kiểu tổ hợp. 

Số lượng thiết bị đầu cuối nhỏ gợi ý DP qua mặt nạ thiết bị đầu cuối. Từ$2^7 = 128$, chúng ta có thể duy trì trạng thái DP cho mọi tập hợp con của thiết bị đầu cuối và mọi ô. 

Có một số trường hợp góc dễ bỏ sót. 

Một trường hợp tinh tế là khi giải pháp tối ưu xem xét lại các ô được chia sẻ bởi nhiều kết nối đầu cuối. 

Ví dụ:```
2 3 3
1 100 1
1 1 1
1 1
1 3
2 2
```Cấu trúc rẻ nhất không phải là ba con đường ngắn nhất riêng biệt. Hàng ghế giữa phía dưới đóng vai trò là hành lang chung. Một chiến lược tham lam kết nối các thiết bị đầu cuối theo cặp độc lập sẽ làm tăng chi phí quá mức và tạo ra sự kết hợp không tối ưu. 

Một trường hợp phức tạp khác là khi hai thiết bị đầu cuối ở cạnh nhau.```
1 2 2
5 7
1 1
1 2
```Câu trả lời phải tạo ra cả hai ô với tổng chi phí là 12. Việc triển khai bất cẩn coi chi phí đường dẫn là chi phí cạnh thay vì chi phí đỉnh có thể quên bao gồm một điểm cuối. 

Một vấn đề tế nhị hơn xuất hiện trong quá trình tái thiết. Các tập hợp con khác nhau có thể hợp nhất tại cùng một ô.```
3 3 3
1 1 1
1 100 1
1 1 1
1 1
1 3
3 2
```Cây tối ưu hợp nhất xung quanh các ô bên ngoài rẻ tiền và tránh trung tâm. Nếu tính năng theo dõi gốc chỉ lưu trữ các phần nới lỏng đường dẫn ngắn nhất và không hợp nhất tập hợp con thì việc tái thiết sẽ không thành công ngay cả khi các giá trị DP là chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi tập hợp con của các ô lưới, kiểm tra xem nó có tạo thành một thành phần được kết nối chứa tất cả các thiết bị đầu cuối hay không và tính toán tổng chi phí của nó. Lưới có tối đa 200 ô, vì vậy điều này đòi hỏi phải kiểm tra$2^{200}$các tập hợp con, có kích thước lớn về mặt thiên văn. 

Một ý tưởng mạnh mẽ hơn một chút là thử tất cả các cây có thể kết nối các thiết bị đầu cuối. Vì bản thân đồ thị có tới 200 đỉnh và có thể có nhiều điểm Steiner nên số lượng cây ứng cử vẫn vượt xa khả năng thực hiện. 

Quan sát quan trọng là số lượng tế bào quan trọng là cực kỳ nhỏ. Khi chỉ tồn tại một số thiết bị đầu cuối, chúng ta có thể mô tả các yêu cầu kết nối bằng mặt nạ bit. 

Giả sử chúng ta xác định:$$dp[mask][v]$$là chi phí tối thiểu của một vùng trải nhựa được kết nối chứa chính xác các thiết bị đầu cuối trong`mask`và kết thúc tại ô`v`, nghĩa`v`thuộc về cấu trúc được kết nối. 

Bây giờ vấn đề bắt đầu có vẻ có thể quản lý được. chỉ có$2^k \le 128$mặt nạ và nhiều nhất là 200 ô, vì vậy tổng số trạng thái là khoảng 25.000. 

Quan sát tiếp theo là cách các trạng thái này kết hợp với nhau. 

Nếu hai cấu trúc được kết nối đều kết thúc ở cùng một ô`v`, một tập hợp con bao phủ`A`và tập hợp con bao phủ khác`B`, sau đó hợp nhất chúng tại`v`tạo ra một cấu trúc kết nối hợp lệ cho`A ∪ B`. 

Điều đó mang lại sự chuyển đổi:$$dp[A \cup B][v] = dp[A][v] + dp[B][v] - cost(v)$$Chúng tôi trừ chi phí ô một lần vì cả hai cấu trúc đều đã tính nó. 

Sau khi thực hiện hợp nhất tập hợp con, chúng ta vẫn cần di chuyển qua lưới. Từ mỗi ô, chúng ta có thể thư giãn các ô lân cận giống hệt như các đường đi ngắn nhất trên biểu đồ. Vì tất cả các bổ sung cạnh đều tương ứng với việc trả chi phí ô của hàng xóm, nên việc truyền đường dẫn ngắn nhất đa nguồn sẽ cập nhật tất cả các điểm cuối có thể tiếp cận một cách tối ưu. 

Sự kết hợp giữa tập hợp con DP và các đường đi ngắn nhất này là Steiner Tree DP theo phong cách Dreyfus-Wagner cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{nm})$|$O(2^{nm})$| Quá chậm | 
| Tối ưu |$O(3^k \cdot nm + 2^k \cdot (nm)^2)$|$O(2^k \cdot nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ô lưới thành một đỉnh đồ thị. 

Cung cấp cho mỗi ô một id số nguyên. Các ô liền kề theo bốn hướng chính được kết nối. 
2. Xác định trạng thái DP. 

Cho phép:$$dp[mask][v]$$là tổng chi phí lát đường tối thiểu cho một kết cấu được kết nối có chứa tất cả các thiết bị đầu cuối trong`mask`và kết thúc ở đỉnh`v`. 
3. Khởi tạo mặt nạ đơn. 

Nếu thiết bị đầu cuối`i`nằm ở đỉnh`v`, sau đó:$$dp[1 << i][v] = cost(v)$$bởi vì cấu trúc được kết nối nhỏ nhất chỉ chứa thiết bị đầu cuối đó là chính ô đơn lẻ. 
4. Hợp nhất các tập con nhỏ hơn thành tập con lớn hơn. 

Đối với mọi mặt nạ và mọi mặt nạ con thích hợp không trống:$$sub \subset mask$$cập nhật:$$dp[mask][v] = \min( dp[mask][v], dp[sub][v] + dp[mask \oplus sub][v] - cost(v) )$$Cả hai cấu trúc được kết nối đều gặp nhau tại cùng một ô`v`, tạo ra một cấu trúc kết nối lớn hơn. 
5. Chạy thư giãn theo con đường ngắn nhất cho mỗi mặt nạ. 

Sau khi hợp nhất tập hợp con, cấu trúc tốt nhất kết thúc tại một ô có thể được mở rộng qua các ô lân cận. 

Vì mọi chi phí đều dương nên Dijkstra hoạt động một cách tự nhiên. 

Đối với mỗi mặt nạ, chạy Dijkstra bắt đầu từ tất cả các đỉnh có khoảng cách ban đầu`dp[mask][v]`. 

Thư giãn từ`u`ĐẾN`to`thêm vào`cost(to)`. 
6. Theo dõi thông tin phụ huynh. 

Tái thiết cần có hai loại cha mẹ: 

Đầu tiên, con đường ngắn nhất dành cho cha mẹ từ Dijkstra. 

Thứ hai, phân chia tập hợp con cha mẹ cho biết hai mặt nạ con nào đã được hợp nhất. 
7. Tìm điểm cuối tốt nhất. 

Câu trả lời là:$$\min_v dp[(1<<k)-1][v]$$Bất kỳ điểm cuối nào cũng hoạt động vì bản thân cấu trúc được kết nối. 
8. Tái tạo lại các ô đã sử dụng. 

Đệ quy làm theo con trỏ cha mẹ. 

Bất cứ khi nào một trạng thái xuất phát từ việc hợp nhất tập hợp con, hãy lặp lại thành cả hai phần. 

Bất cứ khi nào một trạng thái xuất phát từ việc nới lỏng đường đi ngắn nhất, hãy lặp lại vào ô trước đó. 

Đánh dấu mọi ô đã ghé thăm là đã trải nhựa. 

### Tại sao nó hoạt động 

Bất biến DP là:$$dp[mask][v]$$luôn bằng chi phí tối thiểu trong số tất cả các đồ thị con được lát đá được kết nối chứa mọi thiết bị đầu cuối trong`mask`và chứa đỉnh`v`. 

Quá trình chuyển đổi tập hợp con là chính xác vì bất kỳ cây Steiner được kết nối nào cũng có thể được phân tách tại một đỉnh phân nhánh thành các cây được kết nối nhỏ hơn bao gồm các tập hợp con đầu cuối rời rạc. 

Việc nới lỏng đường đi ngắn nhất là đúng vì việc mở rộng cấu trúc được kết nối bằng một ô lân cận sẽ duy trì kết nối và cộng thêm chính xác chi phí lát đường của ô đó. 

Vì mọi cây Steiner được kết nối đều có thể được xây dựng bằng cách liên tục hợp nhất các nhóm thiết bị đầu cuối nhỏ hơn và mở rộng qua các ô liền kề, DP sẽ khám phá tất cả các khả năng hợp lệ. Mọi chuyển đổi đều duy trì tính tối ưu, do đó trạng thái cuối cùng của mặt nạ đầy đủ là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**18

def solve():
    n, m, k = map(int, input().split())

    grid = [list(map(int, input().split())) for _ in range(n)]

    terminals = []
    for _ in range(k):
        x, y = map(int, input().split())
        terminals.append((x - 1, y - 1))

    V = n * m

    def vid(x, y):
        return x * m + y

    cost = [0] * V

    for i in range(n):
        for j in range(m):
            cost[vid(i, j)] = grid[i][j]

    adj = [[] for _ in range(V)]

    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]

    for x in range(n):
        for y in range(m):
            v = vid(x, y)

            for d in range(4):
                nx = x + dx[d]
                ny = y + dy[d]

                if 0 <= nx < n and 0 <= ny < m:
                    adj[v].append(vid(nx, ny))

    FULL = 1 << k

    dp = [[INF] * V for _ in range(FULL)]

    parent = [[None] * V for _ in range(FULL)]

    for i, (x, y) in enumerate(terminals):
        v = vid(x, y)
        dp[1 << i][v] = cost[v]

    for mask in range(FULL):

        sub = (mask - 1) & mask

        while sub:
            other = mask ^ sub

            for v in range(V):
                val = dp[sub][v] + dp[other][v] - cost[v]

                if val < dp[mask][v]:
                    dp[mask][v] = val
                    parent[mask][v] = ("merge", sub, other)

            sub = (sub - 1) & mask

        pq = []

        for v in range(V):
            if dp[mask][v] < INF:
                heapq.heappush(pq, (dp[mask][v], v))

        while pq:
            dist, v = heapq.heappop(pq)

            if dist != dp[mask][v]:
                continue

            for to in adj[v]:
                nd = dist + cost[to]

                if nd < dp[mask][to]:
                    dp[mask][to] = nd
                    parent[mask][to] = ("move", v)

                    heapq.heappush(pq, (nd, to))

    full = FULL - 1

    best_v = min(range(V), key=lambda v: dp[full][v])

    ans = dp[full][best_v]

    used = [[False] * m for _ in range(n)]

    sys.setrecursionlimit(10**6)

    def build(mask, v):
        x = v // m
        y = v % m

        used[x][y] = True

        p = parent[mask][v]

        if p is None:
            return

        if p[0] == "move":
            build(mask, p[1])

        else:
            _, a, b = p
            build(a, v)
            build(b, v)

    build(full, best_v)

    print(ans)

    for i in range(n):
        row = []

        for j in range(m):
            row.append('X' if used[i][j] else '.')

        print("".join(row))

solve()
```Giải pháp có hai thành phần đan xen, tập hợp con DP và đường đi ngắn nhất. 

Bảng DP lưu trữ cấu trúc được kết nối tốt nhất cho mọi tập hợp con và điểm cuối đầu cuối. Vì tất cả số lượng thiết bị đầu cuối đều rất nhỏ nên việc lặp lại trên tất cả các mặt nạ là khả thi. 

Vòng lặp hợp nhất tập hợp con sử dụng mẫu lặp mặt nạ con tiêu chuẩn:```
sub = (mask - 1) & mask
```Điều này liệt kê tất cả các mặt nạ con không trống thích hợp một cách hiệu quả. 

Một lỗi phổ biến ở đây là tính hai lần ô cuộc họp. Cả hai cấu trúc một phần đã bao gồm đỉnh`v`, vì vậy chúng tôi trừ chi phí của nó một lần trong quá trình hợp nhất. 

Sau khi hợp nhất, Dijkstra truyền cấu trúc được kết nối qua lưới. Cập nhật khoảng cách sẽ thêm chi phí ô đích vì việc vào một ô được lát mới sẽ phá hủy hoa của nó. 

Tính năng theo dõi của phụ huynh lưu trữ hai loại chuyển tiếp.```
("move", prev_vertex)
```có nghĩa là thư giãn Dijkstra.```
("merge", left_mask, right_mask)
```có nghĩa là hai tập hợp con được hợp nhất ở cùng một đỉnh. 

DFS xây dựng lại tuân theo các chuyển tiếp này một cách đệ quy và đánh dấu tất cả các ô đã truy cập. 

Một chi tiết tinh tế là nhiều cuộc gọi đệ quy có thể truy cập lại cùng một trạng thái. Điều đó vô hại vì chúng ta chỉ cần tập hợp ô đã sử dụng cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
1 2 3
1 2 3
1 2 3
1 2
3 3
```Các thiết bị đầu cuối đang ở`(1,2)`Và`(3,3)`. 

| Bước | Mặt nạ | Điểm cuối | Chi phí | 
| --- | --- | --- | --- | 
| Khởi tạo | 01 | (1,2) | 2 | 
| Khởi tạo | 10 | (3,3) | 3 | 
| Dijkstra | 01 | các ô có thể truy cập | chi phí ngắn nhất | 
| Dijkstra | 10 | các ô có thể truy cập | chi phí ngắn nhất | 
| Hợp nhất | 11 | điểm cuối chung | kết hợp | 
| Cuối cùng | 11 | (3,2) | 9 | 

Cấu trúc được xây dựng lại trở thành:```
.X.
.X.
.XX
```Dấu vết này thể hiện tính bất biến chính: mọi trạng thái DP biểu thị một cấu trúc được kết nối. Việc hợp nhất chỉ xảy ra sau khi cả hai tập hợp con được kết nối độc lập. 

### Ví dụ 2 

đầu vào:```
2 3 3
1 100 1
1 1 1
1 1
1 3
2 2
```| Bước | Mặt nạ | Điểm cuối | Chi phí tốt nhất | 
| --- | --- | --- | --- | 
| Khởi tạo | 001 | (1,1) | 1 | 
| Khởi tạo | 010 | (1,3) | 1 | 
| Khởi tạo | 100 | (2,2) | 1 | 
| Hợp nhất | 101 | (2,1) | 3 | 
| Hợp nhất | 111 | (2,2) | 5 | 

Lát đường tối ưu:```
X.X
XXX
```Loại bỏ hoàn toàn ô ở giữa đắt tiền với giá 100. Ví dụ này cho thấy tại sao thuật toán phải xem xét các điểm Steiner thay vì kết nối các cặp đầu cuối độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(3^k \cdot nm + 2^k \cdot nm \log(nm))$| tập hợp con hợp nhất cộng với Dijkstra cho mỗi mặt nạ | 
| Không gian |$O(2^k \cdot nm)$| Bảng DP và theo dõi phụ huynh | 

Với tối đa 128 mặt nạ và 200 đỉnh, DP chứa khoảng 25.000 trạng thái. Dijkstra trên một biểu đồ nhỏ như vậy là cực kỳ nhanh, vì vậy lời giải nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import heapq

    input = sys.stdin.readline
    INF = 10**18

    out = io.StringIO()
    sys.stdout = out

    n, m, k = map(int, input().split())

    grid = [list(map(int, input().split())) for _ in range(n)]

    terminals = []
    for _ in range(k):
        x, y = map(int, input().split())
        terminals.append((x - 1, y - 1))

    V = n * m

    def vid(x, y):
        return x * m + y

    cost = [0] * V

    for i in range(n):
        for j in range(m):
            cost[vid(i, j)] = grid[i][j]

    adj = [[] for _ in range(V)]

    for x in range(n):
        for y in range(m):
            v = vid(x, y)

            for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
                nx = x + dx
                ny = y + dy

                if 0 <= nx < n and 0 <= ny < m:
                    adj[v].append(vid(nx, ny))

    FULL = 1 << k

    dp = [[INF] * V for _ in range(FULL)]

    for i, (x, y) in enumerate(terminals):
        dp[1 << i][vid(x, y)] = cost[vid(x, y)]

    for mask in range(FULL):

        sub = (mask - 1) & mask

        while sub:
            other = mask ^ sub

            for v in range(V):
                dp[mask][v] = min(
                    dp[mask][v],
                    dp[sub][v] + dp[other][v] - cost[v]
                )

            sub = (sub - 1) & mask

        pq = []

        for v in range(V):
            if dp[mask][v] < INF:
                heapq.heappush(pq, (dp[mask][v], v))

        while pq:
            dist, v = heapq.heappop(pq)

            if dist != dp[mask][v]:
                continue

            for to in adj[v]:
                nd = dist + cost[to]

                if nd < dp[mask][to]:
                    dp[mask][to] = nd
                    heapq.heappush(pq, (nd, to))

    ans = min(dp[FULL - 1])

    print(ans)

    sys.stdout = sys.__stdout__

    return out.getvalue().strip()

# provided sample
assert run(
"""3 3 2
1 2 3
1 2 3
1 2 3
1 2
3 3
"""
) == "9"

# minimum size
assert run(
"""1 1 1
5
1 1
"""
) == "5"

# adjacent terminals
assert run(
"""1 2 2
5 7
1 1
1 2
"""
) == "12"

# avoid expensive center
assert run(
"""2 3 3
1 100 1
1 1 1
1 1
1 3
2 2
"""
) == "5"

# all equal values
assert run(
"""2 2 2
1 1
1 1
1 1
2 2
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`lưới |`5`| không gian trạng thái nhỏ nhất có thể | 
| Nhà ga liền kề |`12`| chi phí điểm cuối được tính chính xác | 
| Trung tâm đắt giá |`5`| Định tuyến Steiner tránh được các tế bào tốn kém | 
| Tất cả các chi phí bằng nhau |`3`| nhiều đường dẫn tối ưu được xử lý chính xác | 

## Vỏ cạnh 

Hãy xem xét trường hợp thiết bị đầu cuối liền kề:```
1 2 2
5 7
1 1
1 2
```Việc khởi tạo tạo ra:```
dp[01][left] = 5
dp[10][right] = 7
```Sự lan truyền Dijkstra cho phép mỗi trạng thái tiếp cận được ô lân cận. Khi hai mặt nạ hợp nhất ở một trong hai điểm cuối, tổng số sẽ trở thành:```
5 + 7 = 12
```Phép trừ trong quá trình hợp nhất sẽ ngăn chặn việc tính hai lần. 

Bây giờ hãy xem xét ví dụ về trung tâm đắt tiền:```
2 3 3
1 100 1
1 1 1
1 1
1 3
2 2
```Một liên minh đường đi ngắn nhất ngây thơ sẽ sử dụng hàng trên cùng và trả 102. Thay vào đó, DP phát hiện ra rằng tất cả các tập hợp con có thể hợp nhất một cách rẻ tiền dọc theo hàng thứ hai:```
X.X
XXX
```với tổng chi phí là 5. 

Cuối cùng, hãy xem xét một cấu trúc nặng về hợp nhất:```
3 3 3
1 1 1
1 100 1
1 1 1
1 1
1 3
3 2
```Cây tối ưu uốn quanh tâm đắt tiền và hợp nhất ở các ô bên ngoài. Trong quá trình xây dựng lại, các lệnh gọi đệ quy tuân theo cả việc hợp nhất tập hợp con và chuyển tiếp chuyển động, khôi phục chính xác cấu trúc được kết nối đầy đủ mà không bao gồm ô trung tâm.
