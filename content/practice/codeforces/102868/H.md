---
title: "CF 102868H - Vàng"
description: "Bài toán yêu cầu chúng ta ngăn chặn màu Vàng di chuyển qua một mạng lưới bằng cách đóng càng ít cửa càng tốt. Lưới bao gồm những bức tường luôn bị chặn, hành lang luôn mở và những cánh cửa có thể mở hoặc đóng."
date: "2026-07-25T13:32:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102868
codeforces_index: "H"
codeforces_contest_name: "2020 UTPC Fall Puzzle Contest"
rating: 0
weight: 102868
solve_time_s: 50
verified: true
draft: false
---

[CF 102868H - Vàng](https://codeforces.com/problemset/problem/102868/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta ngăn chặn màu Vàng di chuyển qua một mạng lưới bằng cách đóng càng ít cửa càng tốt. Lưới bao gồm những bức tường luôn bị chặn, hành lang luôn mở và những cánh cửa có thể mở hoặc đóng. Màu vàng bắt đầu ở ô trên cùng bên trái và muốn đến ô dưới cùng bên phải. Chúng ta cần tìm số lượng cửa nhỏ nhất mà việc đóng cửa khiến đích đến đó không thể truy cập được. Nếu chỉ riêng hành lang đã có đường đi thì những kẻ mạo danh không thể ngăn được Vàng, nên câu trả lời là`-1`. Bài toán chính thức mô tả nhiệm vụ tách lưới này với các hàng, cột, tường, cửa và hành lang. 

Lưới có thể có tới 1000 hàng và 1000 cột, nghĩa là có thể có tới một triệu ô. Bất kỳ thuật toán nào thử nhiều đường dẫn một cách rõ ràng sẽ quá chậm vì số lượng đường dẫn có thể tăng theo cấp số nhân. Ngay cả các thuật toán xử lý từng cặp ô cũng đã quá đắt. Chúng ta cần một phương pháp chỉ chạm vào mỗi ô một số lần nhỏ, hướng tới các thuật toán đồ thị có độ phức tạp gần như tuyến tính. 

Các trường hợp phức tạp chính đến từ các ô không phải là không gian mở thông thường. Ô bắt đầu và ô kết thúc có thể là cửa ra vào. Một giải pháp bất cẩn có thể cho rằng chúng luôn có thể truy cập được và bỏ lỡ việc đóng một trong hai cái đó ngay lập tức chặn Màu vàng. Ví dụ:```
2 2
..
.@
```Câu trả lời đúng là`1`. Đóng cửa bắt đầu là đủ, nhưng giải pháp chỉ xem xét các cửa giữa điểm bắt đầu và điểm kết thúc sẽ tạo ra giá trị lớn hơn một cách không chính xác. 

Một trường hợp khác là khi đường đi chỉ chứa các hành lang. Vì hành lang không thể đóng nên không có lựa chọn cửa nào có thể ngắt kết nối hai điểm cuối.```
2 2
@@
@@
```Câu trả lời đúng là`-1`. Tìm kiếm đường dẫn ngắn nhất chỉ tính các cửa trên một đường dẫn được phát hiện có thể xuất ra không chính xác`0`, mặc dù một lối đi hành lang khác vẫn tồn tại. 

Trường hợp khó phát hiện cuối cùng là khi câu trả lời bằng 0 vì các bức tường đã ngăn cách các điểm cuối.```
3 3
@#.
###
.@.
```Câu trả lời đúng là`0`. Thuật toán phải cho phép vết cắt tối thiểu không chứa cửa nào cả. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tự nhiên là xem xét mọi tập hợp con cửa có thể đóng lại. Đối với mỗi tập hợp con, chúng tôi có thể chạy lũ lụt ngay từ đầu và kiểm tra xem liệu đích có còn truy cập được hay không. Điều này đúng vì mọi cách chặn cửa có thể đều đã được thử nghiệm, nhưng số lượng tập hợp con là`2^D`, Ở đâu`D`là số cửa. Với hàng trăm ngàn cánh cửa có thể có, điều này là không thể. 

Điều quan trọng cần lưu ý là đây thực sự không phải là vấn đề tìm đường. Chúng tôi không tìm kiếm con đường cho Vàng. Chúng tôi đang tìm kiếm tập hợp chướng ngại vật có thể tháo rời nhỏ nhất mà việc loại bỏ sẽ ngăn cách hai vị trí. Đó chính xác là định nghĩa của mức cắt giảm tối thiểu. 

Lưới có thể được chuyển đổi thành mạng luồng. Mỗi ô sẽ trở thành một nút biểu đồ và việc di chuyển giữa các ô liền kề sẽ trở thành một cạnh có dung lượng vô hạn. Các bức tường chỉ đơn giản là bị bỏ qua. Hành lang có sức chứa vô hạn vì chúng không thể bị chặn. Cửa có sức chứa một vì cắt qua một ô như vậy đồng nghĩa với việc đóng chính xác một cửa. 

Thách thức còn lại là các thuật toán cắt tối thiểu tiêu chuẩn hoạt động trên các cạnh, trong khi chi phí thuộc về các ô. Điều này được giải quyết bằng cách chia nút. Mỗi ô trở thành hai nút: một nút vào và một nút thoát. Cạnh từ lối vào đến lối ra mang chi phí loại bỏ ô đó. Các cạnh di chuyển đi từ phía thoát của một ô đến phía vào của các ô lân cận với dung lượng vô hạn. Tối thiểu`s-t`cut sau đó chọn các cạnh ô cần cắt và tổng công suất của chúng chính xác là số cửa được đóng. 

Vì lưới chứa tới một triệu ô nên chúng ta cần triển khai luồng tối đa hiệu quả. Thuật toán của Dinic hoạt động tốt ở đây vì đồ thị thưa thớt. Mỗi ô chỉ tạo ra một số cạnh không đổi, do đó mạng vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^D * (R*C)) | O(R*C) | Quá chậm | 
| Tối ưu | Trường hợp xấu nhất O(V^2E) với Dinic, thực tế trên lưới thưa thớt này | O(R*C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ô không có tường thành hai nút biểu đồ, một nút vào và một nút thoát. Thêm một cạnh từ lối vào đến lối ra. Công suất của nó là`1`cho một cánh cửa và vô cực cho một hành lang. Nguồn và đích được chọn cẩn thận vì ô bắt đầu và ô kết thúc cũng có thể là cửa ra vào. 
2. Đối với mỗi cặp ô không có vách liền kề, hãy thêm một cạnh có hướng từ nút thoát của ô đầu tiên đến nút vào của ô thứ hai với dung lượng vô hạn. Thêm hướng ngược lại là tốt. Điều này thể hiện màu Vàng có thể di chuyển cả hai chiều qua các ô lân cận. 
3. Chạy thuật toán luồng tối đa từ phía bắt đầu đến phía đích. Theo định lý cắt cực tiểu luồng cực đại, giá trị của luồng này bằng dung lượng của tập cạnh rẻ nhất ngăn cách hai cạnh. 
4. Nếu giá trị luồng kết quả ít nhất là vô cùng, điều đó có nghĩa là mọi sự phân tách có thể xảy ra sẽ yêu cầu cắt một cạnh hành lang không thể thực hiện được. Trong trường hợp đó câu trả lời là`-1`. Ngược lại, giá trị luồng là số lượng cửa tối thiểu phải đóng. 

Tại sao nó hoạt động: các cạnh có công suất hữu hạn duy nhất trong biểu đồ là các cạnh vào ra của cửa. Bất kỳ đường cắt hữu hạn nào cũng phải chọn một số cạnh này và không thể chọn các cạnh hành lang. Cắt một mép cửa chính xác là đóng lại cánh cửa đó. Bất kỳ bộ cửa nào chặn Màu Vàng sẽ tạo ra một mức cắt hợp lệ có cùng chi phí và bất kỳ mức cắt hữu hạn nào sẽ tạo ra một bộ cửa đóng hợp lệ. Do đó, giá trị cắt tối thiểu chính xác là số lượng cửa tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.g[u].append([v, c, len(self.g[v])])
        self.g[v].append([u, 0, len(self.g[u]) - 1])

    def max_flow(self, s, t):
        flow = 0
        n = self.n
        INF = 10**9

        while True:
            level = [-1] * n
            level[s] = 0
            q = [s]
            for u in q:
                for v, c, _ in self.g[u]:
                    if c and level[v] == -1:
                        level[v] = level[u] + 1
                        q.append(v)

            if level[t] == -1:
                return flow

            it = [0] * n

            def dfs(u, pushed):
                if u == t:
                    return pushed
                while it[u] < len(self.g[u]):
                    e = self.g[u][it[u]]
                    v, c, rev = e
                    if c and level[v] == level[u] + 1:
                        tr = dfs(v, min(pushed, c))
                        if tr:
                            e[1] -= tr
                            self.g[v][rev][1] += tr
                            return tr
                    it[u] += 1
                return 0

            while True:
                pushed = dfs(s, INF)
                if not pushed:
                    break
                flow += pushed

def solve():
    r, c = map(int, input().split())
    grid = [input().strip() for _ in range(r)]

    total = r * c * 2
    dinic = Dinic(total)

    def inside(x, y):
        return 0 <= x < r and 0 <= y < c

    def node(x, y, out):
        return (x * c + y) * 2 + out

    INF = 10**8

    for i in range(r):
        for j in range(c):
            if grid[i][j] == '#':
                continue
            cap = 1 if grid[i][j] == '.' else INF
            dinic.add_edge(node(i, j, 0), node(i, j, 1), cap)

    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    for i in range(r):
        for j in range(c):
            if grid[i][j] == '#':
                continue
            for dx, dy in dirs:
                ni, nj = i + dx, j + dy
                if inside(ni, nj) and grid[ni][nj] != '#':
                    dinic.add_edge(node(i, j, 1), node(ni, nj, 0), INF)

    source = node(0, 0, 0)
    sink = node(r - 1, c - 1, 1)

    ans = dinic.max_flow(source, sink)
    print(-1 if ans >= INF else ans)

if __name__ == "__main__":
    solve()
```Việc xây dựng biểu đồ tuân theo ý tưởng phân chia nút một cách trực tiếp. Cạnh vào-ra lưu trữ chi phí loại bỏ một ô, vì vậy chỉ có cửa mới đóng góp vào giá trị cắt cuối cùng. Hành lang nhận được sức chứa rất lớn vì việc cắt giảm tối thiểu hợp lệ sẽ không bao giờ chọn loại bỏ chúng. 

Các cạnh chuyển động được thêm vào sau các cạnh của ô vì chúng kết nối mặt ra của một ô với mặt vào của một ô khác. Hướng này khớp với trạng thái sau khi Màu vàng đã vào và rời khỏi một ô. Cả hai hướng đều cần thiết vì chuyển động được phép theo cả bốn hướng. 

Nguồn sử dụng phía đầu vào của ô đầu tiên và phần chìm sử dụng phía đầu ra của ô cuối cùng. Điều này xử lý trường hợp đặc biệt trong đó điểm cuối là một cánh cửa. Nếu cửa nguồn hoặc cửa chìm là thứ ít tốn kém nhất để loại bỏ thì thuật toán luồng có thể chọn cạnh đó. 

Giá trị được sử dụng cho vô cùng chỉ cần lớn hơn câu trả lời tối đa có thể. Vì câu trả lời không thể vượt quá số ô,`10^8`lớn một cách an toàn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 4
@..#
....
....
#..@
```Các trạng thái quan trọng là: 

| Bước | Ý tưởng hiện tại | Kết quả dòng chảy | 
| --- | --- | --- | 
| 1 | Xây dựng mạng lưới trong đó mỗi cửa có giá 1 | Tất cả các đường dẫn có thể tồn tại | 
| 2 | Tìm vết cắt tối thiểu giữa các góc | Việc cắt phải loại bỏ ô cửa | 
| 3 | Đạt công suất tách tối thiểu | 2 | 

Dấu vết cho thấy tồn tại nhiều tuyến đường nên việc loại bỏ một cánh cửa duy nhất là không đủ. Việc cắt tối thiểu chọn hai ô cửa mà việc loại bỏ sẽ ngắt kết nối các điểm cuối. 

Đối với mẫu thứ hai:```
4 4
@..#
..#.
.#..
#..@
```| Bước | Ý tưởng hiện tại | Kết quả dòng chảy | 
| --- | --- | --- | 
| 1 | Chuyển đổi ô thành các nút chia | Mạng được tạo | 
| 2 | Chỉ tìm kiếm bất kỳ con đường nào qua các cạnh hành lang vô tận | Không có con đường như vậy tồn tại | 
| 3 | Cắt tối thiểu có công suất bằng 0 | 0 | 

Điều này chứng tỏ rằng câu trả lời có thể bằng không. Các bức tường đã ngăn cản Màu vàng chạm tới nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V^2E) trường hợp xấu nhất đối với Dinic | Lưới tạo ra các nút O(R_C) và các cạnh O(R_C), đồng thời cấu trúc thưa thớt hoạt động tốt trong thực tế | 
| Không gian | O(R*C) | Mỗi ô tạo ra hai nút và số cạnh không đổi | 

Với một triệu ô, biểu đồ lớn nhưng vẫn tuyến tính ở kích thước đầu vào. Việc triển khai tránh lưu trữ lưới dưới dạng biểu đồ với các kết nối không cần thiết và sử dụng danh sách kề, giúp duy trì mức sử dụng bộ nhớ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""4 4
@..#
....
....
#..@
""") == "2\n", "sample 1"

assert run("""4 4
@..#
..#.
.#..
#..@
""") == "0\n", "sample 2"

assert run("""4 4
@@@@
..#@
.#.@
#..@
""") == "-1\n", "sample 3"

assert run("""2 2
..
.@
""") == "1\n", "start door"

assert run("""2 2
@@
@@
""") == "-1\n", "hallway only"

assert run("""3 3
@#.
###
.@.
""") == "0\n", "already disconnected"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 2 | Nhiều tuyến đường yêu cầu mức cắt tối thiểu thực sự | 
| Mẫu 2 | 0 | Những bức tường có thể khiến bạn không thể đến đích nếu không đóng cửa | 
| Mẫu 3 | -1 | Không thể chặn lối đi chỉ ở hành lang | 
| Vỏ cửa khởi động | 1 | Cửa điểm cuối được xử lý chính xác | 
| Trường hợp chỉ hành lang | -1 | Đường dẫn có dung lượng vô hạn được phát hiện | 
| Trường hợp lưới riêng biệt | 0 | Được phép cắt trống | 

## Vỏ cạnh 

Khi ô khởi động là cửa thì nguồn phải kết nối qua mép công suất của cửa. Vì:```
2 2
..
.@
```cạnh nguồn-ra của ô đầu tiên có dung lượng`1`. Việc cắt tối thiểu có thể loại bỏ cạnh đó ngay lập tức, mang lại giá trị luồng là`1`. 

Khi các hành lang tạo thành một lối đi không thể tránh khỏi thì không có đường cắt hữu hạn nào tồn tại. Vì:```
2 2
@@
@@
```tất cả các tuyến đường chỉ bao gồm các cạnh có dung lượng vô hạn. Dinic không thể đẩy một khoảng cách hữu hạn qua các cạnh cửa vì không tồn tại nên luồng đạt đến ngưỡng vô cực và thuật toán xuất ra`-1`. 

Khi các bức tường đã ngắt kết nối lưới:```
3 3
@#.
###
.@.
```không có đường đi từ nguồn tới điểm chìm trong đồ thị được xây dựng. BFS ban đầu trong Dinic không thể chạm tới bồn chứa, do đó luồng tối đa vẫn bằng 0 và câu trả lời được báo cáo chính xác là`0`.
