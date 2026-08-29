---
title: "CF 104375K - Vương Quốc Quyền Lực C."
description: "Trò chơi có thể được mô hình hóa dưới dạng đồ thị có hướng trong đó mỗi cấp độ là một nút và mỗi mối quan hệ tiên quyết là một cạnh có hướng. Nếu có một cạnh từ cấp u đến cấp v, thì việc hoàn thành u sẽ mở khóa v, do đó v sẽ có thể chơi được sau khi u hoàn thành."
date: "2026-07-01T17:31:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104375
codeforces_index: "K"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 104375
solve_time_s: 84
verified: true
draft: false
---

[CF 104375K - Sức mạnh Vương quốc C.](https://codeforces.com/problemset/problem/104375/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được mô hình hóa dưới dạng đồ thị có hướng trong đó mỗi cấp độ là một nút và mỗi mối quan hệ tiên quyết là một cạnh có hướng. Nếu có một cạnh từ mức`u`để lên cấp`v`, sau đó kết thúc`u`mở khóa`v`, Vì thế`v`có thể chơi được sau`u`được hoàn thành. 

Một lần chơi duy nhất, được gọi là chạy, bắt đầu ở bất kỳ cấp độ bắt đầu được chỉ định nào và tiếp tục dọc theo các cạnh được định hướng, luôn chuyển sang cấp độ đã mở khóa, cho đến khi đạt đến cấp độ không có chuyển tiếp đi giữa các tùy chọn có thể chơi được, được đảm bảo là một trong các cấp độ kết thúc được chỉ định. Sau khi kết thúc một lượt chạy, người chơi sẽ bắt đầu một trò chơi mới từ đầu, nhưng với một hạn chế nghiêm ngặt: mọi cấp độ được sử dụng trong bất kỳ lượt chạy nào trước đó sẽ vĩnh viễn không có sẵn cho tất cả các lượt chạy trong tương lai. 

Mục tiêu là tối đa hóa số lần chạy hoàn chỉnh có thể được thực hiện trước khi không thể bắt đầu một lần chạy khác từ bất kỳ cấp độ bắt đầu nào và đạt đến bất kỳ cấp độ kết thúc nào mà không sử dụng lại các nút đã sử dụng. 

Sự tương tác chính là các lần chạy không độc lập. Mỗi lần chạy tiêu thụ tất cả các cấp độ trên đường dẫn đã chọn và các nút đó không thể được sử dụng lại sau này. Điều này biến vấn đề thành việc chọn càng nhiều đường dẫn hợp lệ càng tốt từ tập hợp các nút bắt đầu đến tập hợp các nút kết thúc, với ràng buộc là không nút nào có thể xuất hiện trong nhiều hơn một đường dẫn. 

Những hạn chế`N ≤ 100`gợi ý mạnh mẽ rằng việc liệt kê theo cấp số nhân các đường dẫn là không khả thi. Ngay cả một biểu đồ hệ số phân nhánh vừa phải cũng có thể chứa nhiều đường dẫn đơn giản giữa nguồn và đích theo cấp số nhân, do đó, bất kỳ phương pháp nào cố gắng liệt kê hoặc thử tất cả các lần chạy sẽ thất bại. Dự kiến ​​sẽ có một công thức dựa trên luồng đa thức hoặc dựa trên kết hợp. 

Trường hợp phức tạp xuất hiện khi nhiều cấp độ bắt đầu có thể đạt đến các cấu trúc trung gian chồng chéo. Ví dụ: nếu cả hai khởi đầu đều dẫn vào một chuỗi chung trước khi đạt đến mức chìm, việc lựa chọn tham lam ngây thơ một đường dẫn có thể chặn các kết hợp tối ưu hơn sau này. 

Một trường hợp khác xảy ra khi một cấp độ vừa có thể đạt được ngay từ đầu vừa nằm trên nhiều đường dẫn tiềm năng đến các đầu khác nhau. Chọn nó sớm trong việc lựa chọn đường đi tham lam có thể làm giảm tổng số lần chạy rời rạc có thể xảy ra. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp là mô phỏng các hoạt động chạy một cách tham lam. Người ta có thể liên tục chọn nút bắt đầu, tìm bất kỳ đường dẫn nào đến nút kết thúc bằng DFS hoặc BFS, xóa tất cả các nút trên đường dẫn đó và lặp lại cho đến khi không còn đường dẫn hợp lệ nào. Điều này rất đơn giản và mỗi lần chạy đều có giá trị riêng. 

Tuy nhiên, cách tiếp cận này phụ thuộc rất nhiều vào thứ tự các đường dẫn được chọn. Một đường dẫn hợp lệ cục bộ có thể sử dụng một nút quan trọng để kích hoạt hai đường dẫn rời rạc khác sau này. Trong các biểu đồ có nút thắt cổ chai được chia sẻ, các lựa chọn tham lam có thể làm giảm đáng kể tổng số lượng. 

Quan sát cốt lõi là mỗi lần chạy là một đường dẫn từ nút nguồn nào đó đến nút đích nào đó và không có nút nào có thể được sử dụng lại trong các lần chạy. Đây chính xác là số lượng tối đa các đường dẫn tách đỉnh trong đồ thị có hướng có nhiều nguồn và điểm chìm. 

Điều này có thể được chuyển đổi thành bài toán luồng cực đại bằng cách sử dụng phân tách nút. Mỗi nút được chia thành một “nút trong” và “nút ngoài” được kết nối bởi một cạnh có công suất một, đảm bảo rằng nút đó có thể được sử dụng trong tối đa một lần chạy. Mỗi cạnh có hướng ban đầu`u -> v`trở thành cạnh công suất vô hạn từ`u_out`ĐẾN`v_in`. Một siêu nguồn kết nối với tất cả các nút bắt đầu và tất cả các nút kết thúc kết nối với một siêu nguồn. 

Luồng tối đa trong mạng này bằng số lần chạy hợp lệ rời rạc tối đa. Mỗi đơn vị luồng tương ứng với một đường dẫn đầy đủ từ đầu đến cuối và ràng buộc dung lượng nút đảm bảo rằng không có cấp độ nào được sử dụng lại trong các lần chạy khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Loại bỏ con đường tham lam | Hàm mũ trong trường hợp xấu nhất | O(N + M) | Quá chậm / Không chính xác | 
| Luồng tối đa khi chia nút | O(F · E) với Dinic, F ≤ N | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành một mạng luồng trong đó các đường dẫn tương ứng với các lượt chạy và các đơn vị luồng tương ứng với các lượt phát đã hoàn thành. 

1. Chia từng nút`i`thành hai nút`i_in`Và`i_out`và kết nối chúng với cạnh có công suất 1. Điều này buộc rằng mỗi cấp độ có thể được sử dụng trong tối đa một lần chạy. 
2. Đối với mọi cạnh tiên quyết`u -> v`, thêm một cạnh có hướng từ`u_out`ĐẾN`v_in`với năng lực vô hạn. Điều này duy trì các quy tắc chuyển động ban đầu mà không hạn chế số lần chạy khác nhau có thể đi qua mép. 
3. Thêm một nút siêu nguồn. Kết nối nó với`s_in`cho mọi cấp độ bắt đầu`s`với công suất 1. Điều này đảm bảo mỗi cấp độ bắt đầu có thể bắt đầu tối đa một lần chạy. 
4. Thêm một nút siêu chìm. Kết nối`e_out`đến bồn rửa cho mọi cấp độ kết thúc`e`với dung lượng 1. Điều này đảm bảo mỗi cấp độ kết thúc có thể kết thúc tối đa một lần chạy. 
5. Chạy thuật toán luồng cực đại từ siêu nguồn đến siêu chìm. Giá trị luồng kết quả là số lần chạy rời rạc tối đa. 

Lý do cấu trúc này hoạt động là vì bất kỳ lần chạy hợp lệ nào đều tương ứng với một đường dẫn trong mạng luồng và bất kỳ đơn vị luồng nào đều tương ứng với việc chọn đường dẫn đó. Việc tách nút đảm bảo sự rời rạc trên tất cả các lần chạy. 

### Tại sao nó hoạt động 

Bất kỳ tập hợp chạy khả thi nào đều xác định một tập hợp các đường dẫn rời rạc từ nguồn đến đích và do đó có thể được ánh xạ trực tiếp tới luồng hợp lệ trong đó mỗi đường dẫn mang một đơn vị. Ngược lại, bất kỳ luồng số nguyên nào cũng có thể được phân tách thành các đường dẫn từ nguồn đến đích và ràng buộc dung lượng một trên mỗi nút phân chia đảm bảo không có nút nào xuất hiện trong nhiều hơn một đường dẫn. Điều này thiết lập sự tương ứng một-một giữa các giải pháp hợp lệ và các luồng khả thi, do đó việc tối đa hóa luồng sẽ tối đa hóa số lần chạy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Dinic:
    def __init__(self, n):
        self.n = n
        self.adj = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.adj[u].append([v, c, len(self.adj[v])])
        self.adj[v].append([u, 0, len(self.adj[u]) - 1])

    def bfs(self, s, t):
        self.level = [-1] * self.n
        q = [s]
        self.level[s] = 0
        for u in q:
            for v, c, _ in self.adj[u]:
                if c > 0 and self.level[v] == -1:
                    self.level[v] = self.level[u] + 1
                    q.append(v)
        return self.level[t] != -1

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
        INF = 10**9
        while self.bfs(s, t):
            self.it = [0] * self.n
            while True:
                pushed = self.dfs(s, t, INF)
                if not pushed:
                    break
                flow += pushed
        return flow

n, s, e = map(int, input().split())
starts = list(map(int, input().split()))
ends = set(map(int, input().split()))
m = int(input())

N = 2 * n + 2
SRC = 2 * n
SNK = 2 * n + 1

dinic = Dinic(N)

for i in range(1, n + 1):
    dinic.add_edge(2 * (i - 1), 2 * (i - 1) + 1, 1)

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    dinic.add_edge(2 * u + 1, 2 * v, 10**9)

for st in starts:
    st -= 1
    dinic.add_edge(SRC, 2 * st, 1)

for en in ends:
    en -= 1
    dinic.add_edge(2 * en + 1, SNK, 1)

print(dinic.max_flow(SRC, SNK))
```Việc triển khai bắt đầu bằng cách xây dựng cấu trúc phân chia nút trong đó mỗi cấp đóng góp một nút trong và nút ngoài được kết nối theo dung lượng một. Đây là cơ chế thực thi việc sử dụng một lần trong tất cả các lần chạy. 

Các cạnh tiên quyết được thêm từ nút ngoài vào nút trong với dung lượng lớn để chúng không hạn chế việc sử dụng lại ngoài cấu trúc. 

Bắt đầu kết nối từ siêu nguồn vào các nút trong và các đầu kết nối từ các nút ngoài vào siêu chìm, đảm bảo mỗi lần chạy đều bắt đầu và kết thúc đúng cách. 

Cuối cùng, thuật toán của Dinic tính toán số lượng tối đa các đường dẫn từ nguồn đến bồn chứa rời rạc, tương ứng trực tiếp với số lần chạy trò chơi tối đa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1 1
2
3
3
1 2
2 3
1 3
```Chúng tôi xây dựng các nút phân chia:`1,2,3`mỗi cái có dung lượng 1 thông qua các cạnh bên trong của chúng. Bắt đầu là nút 2, kết thúc là nút 3. 

| Bước | Hành động dòng chảy | Đường dẫn đã sử dụng | Tổng lưu lượng | 
| --- | --- | --- | --- | 
| 1 | Tìm đường dẫn tăng cường | 2 → 3 | 1 | 

Sau khi gửi một đơn vị luồng, nút 2 và nút 3 sẽ được sử dụng. Không có con đường nào khác có thể được hình thành bởi vì sự bắt đầu và kết thúc duy nhất đã cạn kiệt. 

Đầu ra là:```
1
```Điều này cho thấy một nút cổ chai trong đó một cấu trúc chia sẻ duy nhất giới hạn tất cả các lần chạy có thể. 

### Mẫu 2 

đầu vào:```
3 3 3
1 2 3
1 2 3
3
1 2
1 3
2 3
```Tất cả các nút đều là điểm bắt đầu và kết thúc tiềm năng và các cạnh cho phép kết nối đầy đủ. 

| Bước | Hành động dòng chảy | Đường dẫn đã sử dụng | Tổng lưu lượng | 
| --- | --- | --- | --- | 
| 1 | Chọn 1→2 | 1 → 2 | 1 | 
| 2 | Chọn 2→3 | 2 → 3 | 2 | 
| 3 | Chọn 3 | 3 → 3 | 3 | 

Mỗi nút có thể phục vụ chính xác một lần chạy do hạn chế về dung lượng và cấu trúc cho phép sử dụng toàn bộ. 

Đầu ra:```
3
```Điều này chứng tỏ rằng mô hình trích xuất nhiều đường dẫn từ nguồn đến đích rời rạc như cấu trúc biểu đồ cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(F · E √V) | Dinic chạy hiệu quả trên biểu đồ nhỏ này, với tối đa N đơn vị dòng chảy | 
| Không gian | O(N + M) | Mỗi nút được phân chia và mỗi cạnh được lưu trữ một lần trong danh sách kề | 

Những hạn chế`N ≤ 100`làm cho mạng luồng trở nên nhỏ bé, chỉ có tối đa vài trăm nút và cạnh. Ngay cả việc triển khai Dinic đơn giản cũng có thể diễn ra thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import check_output
    return check_output(["python3", "solution.py"], input=inp.encode()).decode().strip()

# provided samples
assert run("""3 1 1
2
3
3
1 2
2 3
1 3
""") == "1"

assert run("""3 3 3
1 2 3
1 2 3
3
1 2
1 3
2 3
""") == "3"

# custom cases
assert run("""1 1 1
1
1
0
""") == "1", "single node"

assert run("""4 1 1
1
4
3
1 2
2 3
3 4
""") == "1", "single chain"

assert run("""4 2 2
1 2
3 4
4
1 3
2 3
3 4
2 4
""") == "2", "two disjoint routes"

assert run("""5 1 1
1
5
6
1 2
2 5
1 3
3 5
2 4
4 5
""") == "1", "shared bottleneck"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | bắt đầu tầm thường bằng kết thúc | 
| chuỗi đơn | 1 | phụ thuộc tuyến tính | 
| hai tuyến đường rời nhau | 2 | sử dụng đường dẫn song song | 
| nút cổ chai chia sẻ | 1 | hạn chế công suất nút được thực thi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều đường dẫn hợp lệ cạnh tranh cho một nút trung gian. Ví dụ: nếu hai cấp độ bắt đầu đều dẫn vào một hành lang chung trước khi đến các đầu khác nhau thì chỉ một lượt chạy có thể đi qua hành lang đó vì sức chứa của nút là một. Mô hình luồng thực thi điều này một cách tự nhiên bằng cách bão hòa cạnh phân chia của nút chia sẻ sau khi đường dẫn đầu tiên được chọn. 

Một trường hợp khác là khi cấp độ bắt đầu cũng là cấp độ kết thúc. Cấu trúc cho phép kết nối trực tiếp từ nguồn đến chìm qua nút đó và giới hạn dung lượng của nó đảm bảo nó chỉ có thể được sử dụng một lần trong tất cả các lần chạy. Luồng sẽ sử dụng nó như một lần chạy độc lập hoặc dành nó cho một đường dẫn dài hơn, tùy thuộc vào mức tối ưu tổng thể. 

Trường hợp cuối cùng là một biểu đồ hoàn toàn bị ngắt kết nối trong đó không có mức bắt đầu nào có thể đạt đến bất kỳ mức kết thúc nào. Trong tình huống này, không có đường tăng cường nào tồn tại trong mạng luồng, do đó luồng tối đa bằng 0, phù hợp với thực tế là không thể chạy được.
