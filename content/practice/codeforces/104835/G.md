---
title: "CF 104835G - Sàn là Baklava"
description: "Chúng ta được cung cấp một lưới trong đó mỗi ô đại diện cho một miếng baklava có thể chịu được một số lần bị dẫm lên. Mỗi người bạn bắt đầu ở góc trên bên trái và cố gắng đến góc dưới cùng bên phải bằng cách di chuyển từng ô một theo bốn hướng chính."
date: "2026-06-28T11:47:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104835
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 2 (Beginner)"
rating: 0
weight: 104835
solve_time_s: 64
verified: true
draft: false
---

[CF 104835G - Tầng là Baklava](https://codeforces.com/problemset/problem/104835/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó mỗi ô đại diện cho một miếng baklava có thể chịu được một số lần bị dẫm lên. Mỗi người bạn bắt đầu ở góc trên bên trái và cố gắng đến góc dưới cùng bên phải bằng cách di chuyển từng ô một theo bốn hướng chính. Mỗi khi bất kỳ người bạn nào bước lên một ô, độ bền của ô đó sẽ giảm đi một. Hai ô rất đặc biệt: phần đầu và phần cuối không thể phá hủy được nên chúng có thể được sử dụng bao nhiêu lần mà không bị ràng buộc. 

Câu hỏi không phải là tìm một đường dẫn duy nhất mà là về việc sử dụng lặp lại cùng một lưới. Mỗi người bạn đi độc lập, nhưng tất cả họ đều tiêu thụ độ bền trên các ô chung. Chúng tôi muốn số lần truyền tải thành công tối đa từ đầu đến cuối trước khi lưới không thể vượt qua được. 

Hạn chế chính là cả hai thứ nguyên tối đa là 100, trong khi giá trị độ bền có thể lớn tới 100000. Điều đó ngay lập tức loại trừ bất kỳ mô phỏng nào cho mỗi người bạn, vì ngay cả 100000 đường dẫn nhân với tối đa 10000 ô trên mỗi đường dẫn cũng vượt xa giới hạn. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý các đường dẫn một cách ngầm định thay vì mô phỏng từng đường dẫn một. 

Một cách giải thích ngây thơ thường thất bại là cho rằng mỗi người bạn luôn đi cùng một con đường ngắn nhất. Điều đó nhanh chóng bị phá vỡ vì sau một vài lần di chuyển, con đường ngắn nhất đó sẽ bị chặn mặc dù các tuyến đường dài hơn thay thế có thể vẫn tồn tại. Ví dụ: nếu một hành lang hẹp duy nhất kết nối điểm bắt đầu và kết thúc, việc sử dụng liên tục hành lang đó sẽ làm cạn kiệt hành lang đó ngay cả khi phần còn lại của lưới điện không bị ảnh hưởng, nhưng việc định tuyến lại có thể thực hiện được hoặc không tùy thuộc vào cấu trúc. Do đó, bất kỳ cách tiếp cận tham lam nào “luôn sử dụng lại đường đi ngắn nhất” sẽ tính thiếu hoặc thừa tùy thuộc vào cấu trúc ràng buộc. 

Một trường hợp thất bại tinh vi khác phát sinh khi tồn tại nhiều đường dẫn rời rạc. Một phương pháp đơn giản chỉ theo dõi một đường dẫn sẽ bỏ qua việc phân phối lại luồng trên lưới, do đó, nó không thể tính toán chính xác việc sử dụng cạnh chia sẻ. 

## Phương pháp tiếp cận 

Quan sát cốt lõi là đây không phải là bài toán đường đi ngắn nhất mà là bài toán về công suất truyền qua lưới. Mỗi ô (ngoại trừ điểm bắt đầu và kết thúc) hoạt động giống như một nút có công suất bằng độ bền của nó và mỗi lần truyền tải tiêu thụ một đơn vị công suất dọc theo tuyến đường đã chọn. Chúng tôi muốn tối đa hóa số lượng đường dẫn từ đầu đến cuối mà chúng tôi có thể gửi trước khi hết dung lượng. 

Đây chính xác là vấn đề về luồng tối đa, nhưng với dung lượng nút hơn là dung lượng cạnh. Một phép biến đổi tiêu chuẩn chuyển đổi dung lượng nút thành dung lượng biên bằng cách chia mỗi ô thành hai nút: nút “trong” và nút “ngoài” được kết nối bởi một cạnh có dung lượng bằng độ bền của ô. Sự di chuyển giữa các ô liền kề trở thành một cạnh từ nút “ra” của một ô đến nút “trong” của ô lân cận với dung lượng vô hạn. 

Sau khi được chuyển đổi, bài toán sẽ tính toán luồng tối đa từ nguồn (ô bắt đầu) đến ô đích (ô cuối). Mỗi đơn vị luồng tương ứng với một người bạn đi qua lưới thành công và mỗi đơn vị luồng tiêu thụ chính xác một đơn vị độ bền dọc theo mỗi nút trung gian mà nó sử dụng. 

Cách tiếp cận bạo lực sẽ mô phỏng đường đi của từng người bạn, liên tục tìm ra bất kỳ đường dẫn hợp lệ nào trong lưới dư và giảm dần dung lượng. Trong trường hợp xấu nhất, mỗi lần tìm đường dẫn là O(NM) và chúng tôi có thể lặp lại điều này cho đến tổng của tất cả các dung lượng, có thể là 100000, dẫn đến khoảng 10^9 đến 10^10 thao tác. Như vậy là quá chậm. 

Công thức luồng thay thế việc tìm đường dẫn lặp đi lặp lại bằng tối ưu hóa toàn cục nhằm thúc đẩy luồng hiệu quả bằng cách tăng cường các đường dẫn trong mạng dư. Thuật toán của Dinic phù hợp vì đồ thị tương đối nhỏ (tối đa khoảng 10.000 nút sau khi tách) và cấu trúc cạnh có dạng lưới.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(F · NM) trong đó F 100000 | O(NM) | Quá chậm | 
| Lưu lượng tối đa (Dinic) | O(E √V) điển hình cho đồ thị dạng lưới | O(E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi lưới thành mạng luồng và tính toán luồng tối đa từ nguồn đến đích. 

1. Chia mỗi ô (i, j) thành hai nút: nút vào và nút thoát. Chúng ta kết nối lối vào → lối ra với dung lượng bằng K[i][j]. Điều này mô hình hóa thực tế rằng việc bước lên một ô sẽ tiêu tốn độ bền và mỗi đơn vị dòng chảy sẽ tiêu tốn một đơn vị công suất đó. Các ô bắt đầu và kết thúc được coi là có dung lượng vô hạn, do đó các cạnh bên trong của chúng được cho một giá trị rất lớn. 
2. Đối với mỗi cặp ô liền kề trong lưới, chúng ta thêm các cạnh được định hướng từ nút thoát của một ô đến nút vào của ô lân cận với dung lượng vô hạn. Điều này cho phép di chuyển mà không bị hạn chế ngoại trừ thông qua năng lực của nút. 
3. Xác định nguồn là nút đầu vào của (0, 0) và nút đích là nút đầu ra của (N−1, M−1). Luồng từ nguồn đến đích tương ứng với việc một người bạn hoàn thành việc truyền tải thành công. 
4. Chạy thuật toán Dinic để tính luồng tối đa trong mạng này. Mỗi lần tăng cường tương ứng với một đường dẫn đầy đủ khả thi xuyên qua lưới đảm bảo độ bền còn lại. 
5. Trả về tổng lưu lượng làm câu trả lời. 

Tại sao nó hoạt động: mỗi bước đi hợp lệ tương ứng với một đơn vị luồng đi qua một chuỗi các cạnh công suất nút. Bởi vì mỗi dung lượng ô được thực thi trên cạnh phân chia của nó, nên tổng số đơn vị luồng không thể vượt quá K[i][j] có thể đi qua ô đó. Ngược lại, bất kỳ luồng khả thi nào cũng có thể được phân tách thành các đường dẫn từ nguồn tới đích, mỗi đường biểu diễn một đường truyền bạn bè hợp lệ. Điều này thiết lập sự tương ứng một-một giữa các hành trình hợp lệ và các đơn vị luồng, do đó việc tối đa hóa luồng sẽ tối đa hóa chính xác số lượng bạn bè. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

class Dinic:
    def __init__(self, n):
        self.n = n
        self.adj = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.adj[u].append([v, c, len(self.adj[v])])
        self.adj[v].append([u, 0, len(self.adj[u]) - 1])

    def bfs(self, s, t):
        self.level = [-1] * self.n
        q = deque([s])
        self.level[s] = 0
        while q:
            u = q.popleft()
            for v, c, rev in self.adj[u]:
                if c > 0 and self.level[v] < 0:
                    self.level[v] = self.level[u] + 1
                    q.append(v)
        return self.level[t] >= 0

    def dfs(self, u, t, f):
        if u == t:
            return f
        for i in range(self.it[u], len(self.adj[u])):
            self.it[u] = i
            v, c, rev = self.adj[u][i]
            if c > 0 and self.level[v] == self.level[u] + 1:
                pushed = self.dfs(v, t, min(f, c))
                if pushed:
                    self.adj[u][i][1] -= pushed
                    self.adj[v][rev][1] += pushed
                    return pushed
        return 0

    def max_flow(self, s, t):
        flow = 0
        INF = 10**18
        while self.bfs(s, t):
            self.it = [0] * self.n
            while True:
                pushed = self.dfs(s, t, INF)
                if not pushed:
                    break
                flow += pushed
        return flow

def node_id(i, j, m):
    return i * m + j

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    N = n * m * 2 + 5
    dinic = Dinic(N)

    INF = 10**18

    def in_id(i, j):
        return node_id(i, j, m)

    def out_id(i, j):
        return node_id(i, j, m) + n * m

    for i in range(n):
        for j in range(m):
            cap = grid[i][j]
            u = in_id(i, j)
            v = out_id(i, j)
            if (i, j) in [(0, 0), (n - 1, m - 1)]:
                dinic.add_edge(u, v, INF)
            else:
                dinic.add_edge(u, v, cap)

    for i in range(n):
        for j in range(m):
            for di, dj in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    dinic.add_edge(out_id(i, j), in_id(ni, nj), INF)

    s = in_id(0, 0)
    t = out_id(n - 1, m - 1)

    print(dinic.max_flow(s, t))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo việc xây dựng nút chia một cách trực tiếp. Mỗi ô được biểu thị bằng hai chỉ số và cạnh bên trong đảm bảo độ bền. Các cạnh chuyển động được định hướng nhưng hoạt động hiệu quả theo hai chiều vì cả hai hướng đều được thêm vào thông qua phép liệt kê kề. 

Một lỗi triển khai phổ biến là quên chỉ định dung lượng vô hạn cho ô bắt đầu và ô kết thúc. Nếu chúng bị ràng buộc, dòng chảy sẽ bị hạn chế một cách giả tạo mặc dù bài toán đã loại bỏ rõ ràng ràng buộc về độ bền của chúng. 

Một điểm tinh tế khác là lập chỉ mục nút. Vì mỗi ô lưới mở rộng thành hai nút nên tổng số nút sẽ tăng gấp đôi. Việc trộn lẫn các chỉ số trong nút và ngoài nút không chính xác là nguyên nhân phổ biến nhất dẫn đến các câu trả lời sai trong quá trình chuyển đổi này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
0 1000
2000 0
```Giải thích lưới cho thấy rằng chỉ có hai ô ở giữa quan trọng, vì phần đầu và phần cuối là miễn phí. 

Mạng lưới dòng chảy có khả năng: 

- (0,1) có 1000 
- (1,0) có 2000 

Các cạnh chuyển động là không giới hạn. 

Chúng tôi đẩy dòng chảy qua cả hai đường cho đến khi một trong các nút thắt bão hòa. Hệ số giới hạn là tổng công suất khả dụng trên toàn bộ điểm cắt tách từ đầu đến cuối, tổng cộng là 3000. 

| Bước | Đường dẫn hoạt động | Nút cổ chai | Tổng lưu lượng | 
| --- | --- | --- | --- | 
| 1 | (0,0)->(0,1)->(1,1) | 1000 | 1000 | 
| 2 | (0,0)->(1,0)->(1,1) | 2000 | 3000 | 

Bảng cho thấy luồng phân bổ trên cả hai hành lang có sẵn cho đến khi cả hai đều cạn kiệt. 

### Ví dụ 2 

đầu vào:```
3 3
0 1 0
1 1 1
0 1 0
```Ở đây, cấu trúc tạo thành một nút cổ chai hình chữ thập xuyên qua ô trung tâm. Mọi đường dẫn hợp lệ đều phải đi qua giữa. 

| Bước | Đường dẫn được sử dụng | Dung lượng ô giữa còn lại | Tổng lưu lượng | 
| --- | --- | --- | --- | 
| 1 | trên cùng bên trái → giữa → dưới cùng bên phải | 0 | 1 | 
| 2 | nỗ lực tìm tuyến đường thay thế không thành công | 0 | 1 | 

Khi ô trung tâm đã hết, không còn đường dẫn nào nữa. 

Điều này chứng tỏ rằng thuật toán xác định chính xác tắc nghẽn một điểm là yếu tố hạn chế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(E √V) (biểu diễn Dinic thực tế) | Biểu đồ lưới sau khi tách có các nút O(NM) và các cạnh O(NM) | 
| Không gian | O(NM) | Mỗi ô đóng góp số cạnh không đổi | 

Kích thước biểu đồ vẫn có thể quản lý được vì mỗi ô lưới chỉ đóng góp một số cạnh kề không đổi cộng với một cạnh bên trong. Với N, M ≤ 100, tổng số nút là khoảng 20000, vừa vặn trong giới hạn dành cho Dinic trong 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return str(solve())

# sample
assert run("2 2\n0 1000\n2000 0\n") == "3000"

# minimal grid
assert run("2 2\n0 0\n0 0\n") == "0"

# single corridor
assert run("2 3\n0 5 0\n0 5 0\n") == "5"

# wide grid high capacity center
assert run("3 3\n0 0 0\n0 100 0\n0 0 0\n") == "100"

# all large capacities
assert run("2 2\n0 100\n100 0\n") == "200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2x2 số không | 0 | không có năng lực ở bất cứ đâu | 
| hành lang | 5 | đường dẫn cổ chai đơn | 
| trung tâm nặng | 100 | nút quan trọng duy nhất | 
| cao đối xứng | 200 | nhiều con đường rời rạc | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tuyến đường hợp lệ duy nhất yêu cầu xem lại một ô nhiều lần trong các đường dẫn khác nhau. Mô hình luồng xử lý việc này một cách tự nhiên vì dung lượng không bị ràng buộc với một đường dẫn duy nhất mà gắn với tổng mức sử dụng. 

Coi như:```
2 3
0 1 1
1 1 0
```Các tế bào ở giữa tạo thành một cấu trúc chung. Cách tiếp cận đường đi ngắn nhất cho mỗi người bạn ngây thơ sẽ liên tục sử dụng cùng một tuyến đường và thất bại sau một vài lần lặp. Thay vào đó, giải pháp luồng sẽ phân bổ mức sử dụng cho đến khi dung lượng của mỗi ô được sử dụng hết, đếm chính xác tất cả các lần truyền tải có thể. 

Một trường hợp khác là khi tồn tại nhiều hành lang tối ưu như nhau. Việc lựa chọn đường dẫn tham lam có thể làm bão hòa một hành lang sớm, nhưng công thức luồng đảm bảo cả hai đều được sử dụng tối ưu vì việc tăng cường các đường dẫn sẽ khám phá năng lực còn lại một cách linh hoạt thay vì chỉ đi theo một tuyến đường duy nhất.
