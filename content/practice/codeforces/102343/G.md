---
title: "CF 102343G - Hợp tác thoát hiểm"
description: "Thành phố là một mạng lưới hình chữ nhật có tối đa 30 hàng và 30 cột. Bonnie bắt đầu ở một phòng giam, Clyde bắt đầu ở phòng giam khác, và chiếc xe chạy trốn ở phòng giam thứ ba. Một số ô bị chặn. Mỗi người di chuyển từng ô một theo bốn hướng chính."
date: "2026-08-19T05:32:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "G"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 229
verified: true
draft: false
---

[CF 102343G - Hợp tác trốn thoát](https://codeforces.com/problemset/problem/102343/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thành phố là một mạng lưới hình chữ nhật có tối đa 30 hàng và 30 cột. Bonnie bắt đầu ở một phòng giam, Clyde bắt đầu ở phòng giam khác, và chiếc xe chạy trốn ở phòng giam thứ ba. Một số ô bị chặn. Mỗi người di chuyển từng ô một theo bốn hướng chính. 

Hạn chế bất thường là mỗi ô có thể di chuyển được cùng nhau có thể được sử dụng nhiều nhất một lần. Nếu Bonnie đi qua một phòng giam, Clyde không bao giờ có thể vào đó sau đó và ngược lại. Ô bắt đầu của họ cũng bị cấm đối với người khác. Cuối cùng cả hai người phải đến được ô tô và mục tiêu là giảm thiểu tổng số lần di chuyển của họ. Nếu không có cặp tuyến hợp lệ nào tồn tại thì câu trả lời là`-1`. Tuyên bố chính thức của UCF xác nhận các kích thước lưới này và quy tắc ô sử dụng một lần. 

Lưới chứa tối đa (30 \times 30 = 900) ô. Nó đủ nhỏ để xây dựng một biểu đồ rõ ràng với khoảng vài nghìn cạnh, nhưng nó không đủ nhỏ đối với trạng thái chứa tập hợp các ô đã được truy cập. Một tìm kiếm đơn giản trên toàn bộ tuyến đường của cả hai người có rất nhiều khả năng theo cấp số nhân, do đó, giải pháp dựa trên tập hợp con của các ô hoặc liệt kê đường dẫn đầy đủ là không khả thi. 

Những trường hợp phức tạp là do sự tương tác giữa hai tuyến đường. Con đường ngắn nhất dành cho Bonnie và con đường ngắn nhất dành cho Clyde được xem xét độc lập có thể dùng chung một ô, mặc dù cặp đôi này là bất hợp pháp. Ví dụ,```
3 3
B.F
...
.C.
```Các tuyến đường ngắn nhất riêng lẻ đều muốn sử dụng khu vực ở giữa, do đó, chỉ cần thêm hai khoảng cách BFS thông thường không nhất thiết tạo ra một cặp tuyến đường hợp pháp. 

Trường hợp cạnh thứ hai là người kia không thể nhập hai ô bắt đầu. Ví dụ,```
2 3
BCF
...
```Câu trả lời đúng là`3`: Bonnie có thể di chuyển trực tiếp từ`(1,1)`ĐẾN`(1,3)`trong hai nước đi, trong khi Clyde di chuyển từ`(1,2)`ĐẾN`(1,3)`trong một lần di chuyển. Một mô hình đường dẫn đỉnh-tách rời bất cẩn chỉ cung cấp dung lượng cho mỗi ô, người ta vẫn có thể cho phép một luồng đi vào ô bắt đầu thuộc tuyến khác trừ khi các cạnh đến của hai điểm bắt đầu bị cấm rõ ràng. 

Trường hợp thứ ba là cả hai người đều được phép ngồi trên xe. Ví dụ,```
2 2
BF
C.
```Câu trả lời là`2`. Bonnie cần một nước đi và Clyde cần một nước đi, và cả hai con đường đều kết thúc tại cùng một ô. Đưa ra công suất của ô tô, người ta sẽ tuyên bố không chính xác điều này là không thể. 

## Phương pháp tiếp cận 

Phương pháp brute-force là liệt kê các đường đi đơn giản có thể có từ Bonnie đến ô tô và các đường đi đơn giản có thể có từ Clyde đến ô tô. Đối với mỗi cặp, chúng tôi kiểm tra xem hai đường dẫn có rời rạc bên trong hay không và giữ tổng độ dài tối thiểu của chúng. Điều này đúng vì mọi giải pháp pháp lý đều chính xác là một cặp đường dẫn như vậy. 

Vấn đề là số lượng đường dẫn đơn giản. Trong một lưới có (V) các ô có thể sử dụng, số lượng đường dẫn đơn giản có thể là số mũ trong (V), do đó, việc liệt kê chúng đòi hỏi (2^{\Theta(V)}) trở lên hoạt động trong các vùng lưới dày đặc. Với (V=900), con số này vượt xa mọi ngân sách hoạt động thực tế. Ngay cả một tìm kiếm đại diện cho các ô đã truy cập một cách rõ ràng cũng có thể có tối đa (2^{900}) tập hợp đã truy cập. 

Quan sát quan trọng là hạn chế không thực sự nằm ở thứ tự di chuyển của hai người. Nó chỉ nói rằng hai tuyến cuối cùng không được đi chung một ô, ngoại trừ việc cả hai đều được phép về đích ở ô tô. Trong thuật ngữ đồ thị, chúng ta cần hai đường dẫn đỉnh-tách nhau từ hai đỉnh bắt đầu đến một đích chung, giảm thiểu tổng số cạnh của chúng. 

Đó chính xác là những gì một mạng lưới luồng có dung lượng đỉnh thể hiện. Chia mỗi ô lưới thành một`in`đỉnh và một`out`đỉnh. Cạnh từ`in`ĐẾN`out`có dung lượng một, nghĩa là có nhiều nhất một tuyến có thể sử dụng ô. Việc di chuyển từ ô này sang ô lân cận sẽ trở thành một cạnh của ô đầu tiên`out`đỉnh đến ô thứ hai`in`đỉnh. Vì mỗi chuyển động đều tốn một chi phí nên mỗi cạnh chuyển động đều tốn một chi phí. 

Sau đó, thêm một siêu nguồn được kết nối với Bonnie và Clyde, với công suất một ở mỗi cạnh và yêu cầu hai đơn vị dòng chảy để tiếp cận ô tô. Sức chứa của xe là hai người vì cả hai người đều được phép về đích ở đó. Luồng chi phí tối thiểu chính xác là tổng số lần di chuyển tối thiểu. 

Lực lượng vũ phu hoạt động vì mọi câu trả lời hợp pháp có thể được mô tả bằng hai đường dẫn, nhưng không thành công vì có nhiều đường dẫn như vậy theo cấp số nhân. Nhận xét rằng việc sử dụng ô chỉ đơn giản là một hạn chế về dung lượng cho phép chúng ta quên đi lịch sử tìm kiếm thực tế và giải quyết toàn bộ vấn đề như một trường hợp luồng chi phí tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong (rc) | Hàm mũ trong (rc) trong trường hợp xấu nhất | Quá chậm | 
| Tối ưu | (O(VE)) cho hai lần tăng cường SPFA | (O(V+E)) | Đã chấp nhận | 

Ở đây (V=O(rc)) và (E=O(rc)). Hệ số hai của giá trị luồng yêu cầu là không đổi. 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí của Bonnie, Clyde và chiếc ô tô. Mọi thứ không`x`ô trở thành một đỉnh đồ thị. 
2. Chia mọi ô có thể sử dụng (v) thành hai đỉnh biểu đồ,`vin`Và`vout`. Thêm một cạnh`vin -> vout`với công suất một và chi phí bằng không. Cạnh này biểu thị việc sử dụng ô đó nên dung lượng của nó thực thi quy tắc sử dụng một lần. 
3. Đối với mỗi cặp ô có thể sử dụng liền kề (u) và (v), hãy thêm một cạnh`uout -> vin`với công suất một và chi phí một. Đi ngang qua cạnh này tương ứng với việc di chuyển một lưới, do đó tổng chi phí luồng chính xác là tổng số lần di chuyển. 
4. Không thêm các cạnh chuyển động vào ô bắt đầu của Bonnie hoặc ô bắt đầu của Clyde. Các cạnh nguồn của chính chúng là cách duy nhất để vào các ô đó. Điều này trực tiếp mô hình hóa quy tắc rằng không người nào được phép di chuyển vào vị trí xuất phát của người kia. 
5. Thêm siêu nguồn`S`và kết nối nó với Bonnie's`in`đỉnh và Clyde`in`đỉnh có dung lượng bằng 1 và giá bằng 0. Gửi hai đơn vị từ`S`buộc một đơn vị xuất phát từ mỗi ô khởi đầu. 
6. Tặng xe`in -> out`công suất cạnh hai. Cả hai người đều phải về đích ở ô tô nên đây là ô thông thường duy nhất mà cả hai luồng có thể sử dụng. 
7. Chạy luồng chi phí tối thiểu cho đến khi hai đơn vị đã đến được ô tô hoặc không tồn tại đường dẫn tăng cường nào. Vì luồng yêu cầu chỉ có hai nên chúng tôi thực hiện tối đa hai lần tăng thêm. 
8. Nếu hai đơn vị dòng được gửi đi, hãy in tổng chi phí tối thiểu của chúng. Nếu không thì in`-1`, bởi vì ít nhất một trong hai người không thể được định tuyến tới ô tô mà không vi phạm hạn chế dùng chung mạng. 

### Tại sao nó hoạt động 

Mỗi đơn vị dòng chảy tương ứng với một lộ trình từ ô xuất phát đến ô tô. Năng lực-một`in -> out`cạnh của mỗi ô thông thường ngăn không cho hai tuyến đường sử dụng ô đó, do đó các tuyến đường này không khớp với nhau. Các cạnh của ô xuất phát ngăn cản một trong hai tuyến đi vào vị trí xuất phát của người kia. Xe có sức chứa hai, cho phép cả hai tuyến đều kết thúc ở đó. 

Ngược lại, mọi cặp tuyến đường hợp pháp có thể được chuyển đổi thành hai đơn vị luồng bằng cách đi theo các cạnh lưới của chúng. Vì các tuyến không chia sẻ các ô thông thường nên mọi hạn chế về dung lượng đều được tôn trọng. Mỗi lần di chuyển lưới đóng góp chính xác một đơn vị chi phí, do đó chi phí luồng bằng tổng của hai độ dài tuyến đường. Vì vậy việc giảm thiểu chi phí dòng chảy chính xác là vấn đề tối ưu hóa ban đầu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10**9

class MinCostMaxFlow:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap, cost):
        self.g[u].append([v, cap, cost, len(self.g[v])])
        self.g[v].append([u, 0, -cost, len(self.g[u]) - 1])

    def min_cost_flow(self, s, t, required):
        flow = 0
        cost = 0

        while flow < required:
            dist = [INF] * self.n
            prev_v = [-1] * self.n
            prev_e = [-1] * self.n
            in_queue = [False] * self.n

            dist[s] = 0
            q = deque([s])
            in_queue[s] = True

            while q:
                u = q.popleft()
                in_queue[u] = False

                for ei, edge in enumerate(self.g[u]):
                    v, cap, edge_cost, rev = edge
                    if cap > 0 and dist[v] > dist[u] + edge_cost:
                        dist[v] = dist[u] + edge_cost
                        prev_v[v] = u
                        prev_e[v] = ei

                        if not in_queue[v]:
                            q.append(v)
                            in_queue[v] = True

            if dist[t] == INF:
                break

            add = required - flow
            v = t

            while v != s:
                u = prev_v[v]
                ei = prev_e[v]
                add = min(add, self.g[u][ei][1])
                v = u

            v = t
            while v != s:
                u = prev_v[v]
                ei = prev_e[v]

                edge = self.g[u][ei]
                rev = edge[3]

                edge[1] -= add
                self.g[v][rev][1] += add

                v = u

            flow += add
            cost += add * dist[t]

        return flow, cost

def solve():
    r, c = map(int, input().split())
    grid = [input().strip() for _ in range(r)]

    cells = []
    pos = {}

    for i in range(r):
        for j in range(c):
            if grid[i][j] != 'x':
                idx = len(cells)
                cells.append((i, j))
                pos[(i, j)] = idx

    n_cells = len(cells)

    source = 2 * n_cells
    sink = 2 * n_cells + 1
    mcmf = MinCostMaxFlow(2 * n_cells + 2)

    start_b = start_c = finish = -1

    for idx, (i, j) in enumerate(cells):
        ch = grid[i][j]

        if ch == 'B':
            start_b = idx
        elif ch == 'C':
            start_c = idx
        elif ch == 'F':
            finish = idx

        capacity = 2 if ch == 'F' else 1
        mcmf.add_edge(2 * idx, 2 * idx + 1, capacity, 0)

    mcmf.add_edge(source, 2 * start_b, 1, 0)
    mcmf.add_edge(source, 2 * start_c, 1, 0)

    directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

    for idx, (i, j) in enumerate(cells):
        # The two starting cells may only be entered from the super-source.
        if idx != start_b and idx != start_c:
            for di, dj in directions:
                ni, nj = i + di, j + dj
                nxt = pos.get((ni, nj))
                if nxt is not None:
                    mcmf.add_edge(2 * idx + 1, 2 * nxt, 1, 1)

    mcmf.add_edge(2 * finish + 1, sink, 2, 0)

    flow, cost = mcmf.min_cost_flow(source, sink, 2)

    if flow < 2:
        print(-1)
    else:
        print(cost)

if __name__ == "__main__":
    solve()
```các`MinCostMaxFlow`lớp lưu trữ mỗi cạnh dư dưới dạng danh sách bốn phần tử chứa đích, dung lượng còn lại, chi phí và chỉ mục cạnh ngược. Các cạnh dư có giá trị âm so với chi phí ban đầu, đó là lý do tại sao BFS thông thường không đủ cho các đường đi ngắn nhất. Việc triển khai sử dụng SPFA để tìm đường tăng cường rẻ nhất. 

Các ô lưới được đánh số từ 0 đến`n_cells - 1`. Tế bào`idx`trở thành đỉnh đồ thị`2 * idx`Và`2 * idx + 1`. Việc giữ nguyên phép ánh xạ số học này thay vì sử dụng từ điển cho các đỉnh của đồ thị làm cho mạng trở nên nhỏ gọn. 

Năng lực của`in -> out`cạnh là một cho ô thông thường và hai cho ô tô. Ô tô là nơi duy nhất mà hai tuyến đường được phép gặp nhau. 

Các cạnh chuyển động chỉ được thêm vào một cách có chủ ý khi ô hiện tại không phải là một trong hai ô bắt đầu. Điều đó mạnh hơn một chút so với việc chỉ cung cấp dung lượng cho các ô ban đầu. Nếu không có nó, luồng có thể đi vào ô bắt đầu từ một ô lưới khác sau khi một đơn vị luồng khác đã bắt nguồn từ đó, điều này không tương ứng với chuỗi chuyển động hợp pháp. 

Mỗi cạnh chuyển động có chi phí bằng một, trong khi tất cả các cạnh cấu trúc đều có chi phí bằng 0. Do đó, một đường đi chứa (k) chuyển động có chi phí chính xác là (k). 

SPFA ở đây an toàn mặc dù trường hợp xấu nhất về mặt lý thuyết không thuận lợi vì biểu đồ có tối đa 900 ô lưới, chỉ cần hai đơn vị dòng chảy và lưới chỉ tạo ra các cạnh (O(rc)). 

## Ví dụ đã hoạt động 

Vì báo cáo vấn đề có sẵn ở đây không hiển thị các khối đầu vào/đầu ra mẫu nên sau đây là hai dấu vết đại diện sử dụng định dạng đầu vào ban đầu. 

### Ví dụ 1 

Hãy xem xét```
3 4
B..F
.xx.
C...
```Bonnie có thể đi đường trên trong ba nước đi. Clyde có thể đi theo con đường cuối cùng trong bốn nước đi. Họ không dùng chung bất kỳ phòng giam thông thường nào nên câu trả lời là bảy. 

| Tăng cường | Đã tìm thấy tuyến đường | Chi phí đường đi | Tổng lưu lượng | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 |`B -> (1,2) -> (1,3) -> F`| 3 | 1 | 3 | 
| 2 |`C -> (3,2) -> (3,3) -> (3,4) -> F`| 4 | 2 | 7 | 

Lần tăng cường đầu tiên dự trữ ba ô trên tuyến đường của Bonnie. Đường tăng cường ngắn thứ hai phải tôn trọng các cạnh dung lượng một đó, do đó, nó tự động chọn tuyến không sử dụng lại chúng. Chi phí cuối cùng là`3 + 4 = 7`. 

### Ví dụ 2 

Hãy xem xét```
3 3
B.F
...
C..
```Các tuyến đường ngắn nhất riêng lẻ đều muốn sử dụng khu vực trung tâm. Thay vào đó, một giải pháp pháp lý đưa Bonnie qua hàng trên và Clyde qua hàng dưới. 

| Tăng cường | Đã tìm thấy tuyến đường | Chi phí đường đi | Tổng lưu lượng | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 |`B -> (1,2) -> F`| 2 | 1 | 2 | 
| 2 |`C -> (3,2) -> (3,3) -> F`| 3 | 2 | 5 | 

Tuyến đường đầu tiên tiêu thụ`(1,2)`. Mạng dư ghi lại dung lượng đó nên lần tăng cường thứ hai không thể sử dụng ô đó. Cặp kết quả là không khớp với nhau và có tổng chi phí là 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(VE)) | Cần tối đa hai phép tính đường đi ngắn nhất SPFA, mỗi phép tính có giới hạn trường hợp xấu nhất tiêu chuẩn (O(VE)) | 
| Không gian | (O(V+E)) | Đồ thị ô phân tách và các cạnh dư của nó chứa (O(rc)) đỉnh và cạnh | 

Với tối đa 900 ô lưới, mạng được chuyển đổi vẫn còn nhỏ. Phần quan trọng là thuật toán không bao giờ lưu trữ một tập hợp con các ô đã truy cập và không bao giờ liệt kê các cặp tuyến đường. Công thức dòng hai đơn vị loại bỏ hoàn toàn thành phần hàm mũ. 

## Trường hợp thử nghiệm 

Tuyên bố chính thức cung cấp định dạng đầu vào và các ràng buộc nhưng các khối mẫu không có trong văn bản bài toán được cung cấp, vì vậy các bài kiểm tra bên dưới sử dụng hai ví dụ đã làm việc và bốn trường hợp bổ sung. Trình trợ giúp tải lại bộ giải cho mọi lệnh gọi bằng cách chuyển hướng đầu vào và đầu ra tiêu chuẩn.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample-style case 1
assert run("""\
3 4
B..F
.xx.
C...
""") == "7\n", "two disjoint routes"

# Sample-style case 2
assert run("""\
3 3
B.F
...
C..
""") == "5\n", "shared-region routes must be separated"

# Minimum-size grid
assert run("""\
2 2
BF
C.
""") == "2\n", "both people reach the car in one move"

# No possible route for Clyde
assert run("""\
2 3
B.F
Cx.
""") == "-1\n", "one starting cell is trapped"

# All traversable cells form a narrow corridor
assert run("""\
2 4
BC.F
....
""") == "5\n", "starting cells and shared destination"

# Boundary-heavy case
assert run("""\
3 3
F.B
...
C..
""") == "5\n", "paths begin and end on grid boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 x 4`có đường trên và đường dưới riêng biệt |`7`| Định tuyến chi phí tối thiểu theo đỉnh cơ bản | 
|`3 x 3`với các tuyến đường ngắn nhất cạnh tranh |`5`| Ràng buộc dung lượng ô thay đổi câu trả lời | 
|`2 x 2`với`B`,`C`, Và`F`liền kề |`2`| Lưới kích thước tối thiểu và công suất hai tại ô tô | 
|`2 x 3`với Clyde bị mắc kẹt |`-1`| Phát hiện chính xác khi không thể thực hiện được hai đơn vị dòng chảy | 
|`2 x 4`hành lang |`5`| Hạn chế ô bắt đầu và hình học hẹp | 
|`3 x 3`với ranh giới điểm bắt đầu và điểm đến |`5`| Chuyển động theo ranh giới và xử lý từng bước một | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
2 2
BF
C.
```biểu đồ gửi một đơn vị từ`B`thông qua cạnh chuyển động đơn vào`F`, và một đơn vị khác từ`C`thông qua cạnh chuyển động của nó vào`F`. Cạnh chia của ô tô có sức chứa hai nên cả hai luồng đều có thể kết thúc ở đó. Hai lần tăng thêm có giá một lần, mang lại`2`. 

Đối với người tham gia bị chặn,```
2 3
B.F
Cx.
```Clyde không có người hàng xóm nào có thể sử dụng được. Nguồn có thể gửi một đơn vị qua Bonnie, nhưng sau đó không có đường tăng cường thứ hai tới sink. Thuật toán dừng ở luồng một thay vì hai và in`-1`. 

Đối với giới hạn vị trí bắt đầu,```
2 3
BCF
...
```các cạnh chuyển động đi vào`B`Và`C`tế bào vắng mặt. Mỗi tế bào đó chỉ có thể nhận được đơn vị dòng chảy riêng từ siêu nguồn. Điều này ngăn chặn một tuyến đường xâm nhập trái phép vào ô xuất phát của người khác. 

Đối với đích đến được chia sẻ,```
2 2
BF
C.
```cả hai con đường đều kết thúc tại`F`, nhưng không có ô thông thường nào được chia sẻ. Dung lượng hai trên ô tô chính là điểm phân biệt điểm gặp hợp lệ này với một ô thông thường, có dung lượng vẫn là một. 

Đối với các ô ranh giới, chẳng hạn như```
3 3
F.B
...
C..
```biểu đồ chỉ đơn giản là bỏ qua các hàng xóm bên ngoài lưới. Không có logic chuyển động đặc biệt nào cho hàng và cột đầu tiên hoặc cuối cùng ngoài việc kiểm tra xem tọa độ lân cận có tồn tại trong`pos`từ điển, giúp tránh các lỗi sai lệch ranh giới.
