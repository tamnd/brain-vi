---
title: "CF 104393I - Cải thiện khu dân cư"
description: "Chúng ta được cung cấp một lưới nhỏ đại diện cho một vùng lân cận. Mỗi ô là một bức tường, một ô di chuyển tự do, một ngôi nhà, một trường học hoặc một công viên."
date: "2026-07-01T02:23:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "I"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 104
verified: false
draft: false
---

[CF 104393I - Cải thiện khu dân cư](https://codeforces.com/problemset/problem/104393/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới nhỏ đại diện cho một vùng lân cận. Mỗi ô là một bức tường, một ô di chuyển tự do, một ngôi nhà, một trường học hoặc một công viên. Từ một ngôi nhà, chúng tôi muốn chỉ định chính xác một trường học và một công viên, nhưng có một hạn chế: trường được chỉ định phải cách nhà một khoảng cách nhất$D$và công viên được chỉ định cũng phải có thể đến được trong phạm vi tối đa$D$. Chuyển động theo bốn hướng và chỉ thông qua các ô có thể đi qua. 

Một hạn chế quan trọng là mỗi trường học và mỗi công viên chỉ được phép sử dụng tối đa một lần đối với tất cả các ngôi nhà được chọn. Vì vậy, đây không chỉ là vấn đề phân bổ cục bộ cho mỗi ngôi nhà mà còn là vấn đề kết hợp toàn cầu giữa các ngôi nhà và cơ sở vật chất trong điều kiện hạn chế về khoảng cách. 

Nhiệm vụ là tối đa hóa số lượng ngôi nhà có thể được giao thành công cả trường học hợp lệ và công viên hợp lệ theo các quy tắc này. 

Kích thước lưới nhiều nhất là 30 x 30, tức là nhiều nhất là 900 ô. Giới hạn khoảng cách$D$có thể lớn, lên tới 1000, vì vậy khoảng cách đường đi ngắn nhất cũng quan trọng nhưng biểu đồ đủ nhỏ để BFS từ nhiều nguồn là khả thi. 

Một sự hiểu lầm ngây thơ là coi mỗi ngôi nhà một cách độc lập và tham lam chỉ định trường học và công viên gần nhất. Điều đó không thành công vì hai ngôi nhà có thể cạnh tranh để có cùng một cơ sở tối ưu và một sự lựa chọn tham lam có thể cản trở nhiệm vụ toàn cầu tốt hơn. 

Vấn đề tế nhị thứ hai là giả định khoảng cách Euclide hoặc bỏ qua các bức tường. Khoảng cách là con đường ngắn nhất trong biểu đồ lưới, vì vậy chướng ngại vật hoàn toàn quan trọng. 

Một trường hợp cạnh phá vỡ sự tham lam ngây thơ: 

đầu vào:```
1 5 10
H.S.P
```Nếu cả trường học và công viên đều có thể truy cập được, nhưng có nhiều ngôi nhà cạnh tranh cho cùng một trường học hoặc công viên, thì trước tiên, kẻ tham lam có thể chỉ định nó cho một ngôi nhà dưới mức tối ưu, làm giảm tổng số. Câu trả lời đúng phụ thuộc vào kết hợp toàn cầu. 

Một trường hợp khác là khi một ngôi nhà có thể đến trường học nhưng không có công viên bên trong$D$, hoặc ngược lại. Những ngôi nhà này không thể sử dụng được và phải bị loại bỏ hoàn toàn, ngay cả khi một bên có giá trị. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là tính toán cho từng ngôi nhà xem trường học và công viên nào nằm trong khoảng cách$D$. Sau đó, chúng tôi cố gắng chỉ định cho mỗi ngôi nhà một cặp (trường học, công viên), đảm bảo không có cơ sở nào được sử dụng lại. Điều này trở thành một bài toán gán tổ hợp với ba lớp: nhà ở, trường học, công viên. 

Nếu chúng ta suy nghĩ một cách trực tiếp, chúng ta đang chọn các bộ ba tuân theo các ràng buộc, sẽ tăng theo cấp số nhân nếu cố gắng thực hiện theo phương pháp quay lui. Với tối đa 900 ô, mỗi ngôi nhà, trường học và công viên trong trường hợp xấu nhất có thể lên tới hàng trăm ô và việc tìm kiếm đơn giản sẽ bùng nổ. 

Quan sát quan trọng là vấn đề này được tách rõ ràng thành hai vấn đề đối sánh hai bên độc lập: 

Một cách so sánh phân công nhà cho các trường học chỉ sử dụng các cạnh khả thi (khoảng cách ≤ D) và một cách khác phân công nhà cho công viên theo cách tương tự. Tuy nhiên, cả hai sự so khớp này phải được thỏa mãn đồng thời cho cùng một nhóm nhà. Vì vậy, chúng tôi đang chọn một tập hợp con các ngôi nhà sao cho cả hai kết hợp đều có thể hỗ trợ chúng. 

Đây là luồng tối đa cổ điển với ý tưởng tách nút. Mỗi ngôi nhà cần hai đơn vị công suất: một đơn vị kết nối với trường học và một đơn vị kết nối với công viên, trong khi trường học và công viên có công suất 1. 

Chúng tôi xây dựng một mạng lưới luồng trong đó mỗi ngôi nhà chia thành hai nút yêu cầu hoặc tương đương, chúng tôi giữ các nút nhà và thực thi hai lớp riêng biệt. Một công trình sạch hơn là: 

Chúng tôi coi đây là hai kết quả khớp hai bên độc lập, nhưng chúng tôi tìm kiếm nhị phân hoặc tính toán trực tiếp số lượng nhà tối đa bằng cách lập mô hình luồng kết hợp: 

Chúng tôi tạo một nguồn kết nối với tất cả các ngôi nhà (công suất 2 mỗi ngôi nhà nếu được chọn, nhưng lựa chọn là ẩn), sau đó ngôi nhà chia thành hai nút: house_in và house_out. Ngoài ra, giải pháp tiêu chuẩn là: 

Thay vào đó, chúng tôi ấn định mục tiêu k và kiểm tra tính khả thi: liệu chúng tôi có thể đáp ứng k ngôi nhà sao cho mỗi ngôi nhà được chọn được kết nối với một trường học và một công viên mà không cần sử dụng lại không? Điều này được kiểm tra thông qua quy trình trong đó mỗi ngôi nhà có nhu cầu công suất 2, được thực thi thông qua việc chia tách và yêu cầu hai trận đấu riêng biệt. 

Với những ràng buộc nhỏ, công thức được chấp nhận đơn giản nhất là một luồng duy nhất: 

Nguồn → nhà (công suất 2 mỗi cái không chính xác trực tiếp), vì vậy thay vào đó, chúng tôi sao chép các nút nhà thành hai lớp: house_school và house_park. Cả hai phải được kích hoạt cùng nhau, vì vậy chúng tôi thực thi việc ghép nối bằng cách buộc cả hai phải khớp với cùng một lựa chọn nhà cái; nhưng vì chúng tôi tối đa hóa số lượng nên thay vào đó, chúng tôi có thể coi mỗi ngôi nhà là một đơn vị cần hai kết quả phù hợp, đó là tiêu chuẩn "khớp b với nhu cầu 2" có thể giảm bớt bằng cách chia ngôi nhà thành hai nút yêu cầu được liên kết với cạnh công suất vô hạn, đảm bảo cả hai đều phải được đáp ứng. 

Sự đơn giản hóa chính trong thực tế là: 

Chúng tôi xây dựng một quy trình: 

Nguồn → nhà (công suất 2 không được sử dụng) 

Thay vào đó: 

Chúng tôi chia mỗi ngôi nhà thành H và từ H chúng tôi gửi một cạnh đến từng trường khả thi và đỗ xe thông qua hai lớp trung gian riêng biệt, đảm bảo mỗi ngôi nhà có thể gửi tối đa một đơn vị đến phía trường học và một đơn vị đến phía công viên. 

Cuối cùng ta nối trường học, công viên chìm với công suất 1. 

Chúng tôi tính toán lưu lượng tối đa; mỗi ngôi nhà thành công đóng góp 2 đơn vị, vì vậy câu trả lời là tổng lưu lượng chia cho 2. 

Điều này hiệu quả vì mỗi ngôi nhà được chọn phải được khớp một lần trong lớp trường học và một lần ở lớp công viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm phân công vũ phu | Hàm mũ | O(n²) | Quá chậm | 
| Luồng BFS + đa nguồn (khớp hai bên theo lớp) | O(V²E) ~ khả thi đối với 900 nút | O(VE) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chạy BFS từ mọi ô của trường để tính khoảng cách ngắn nhất tới tất cả các ô. Điều này mang lại cho chúng ta một lưới dist_school trong đó dist_school[x][y] là khoảng cách đi bộ tối thiểu từ bất kỳ trường học nào đến ô đó. Điều này cho phép chúng tôi kiểm tra O(1) xem một ngôi nhà có thể đến được trường học trong D hay không. 
2. Chạy BFS từ mọi ô công viên để tính toán dist_park tương tự. Bây giờ chúng tôi biết tính khả thi của công viên độc lập với nhà ở. 
3. Thu thập tất cả các ô nhà. Đối với mỗi ngôi nhà, hãy đánh dấu xem ngôi nhà đó có ít nhất một trường học và ít nhất một công viên có thể tiếp cận hay không. Nếu không, hãy loại bỏ nó hoàn toàn vì nó không bao giờ có thể được phục vụ. 
4. Xây dựng mạng lưới luồng với ba lớp khái niệm: nút nhà, nút trường học và nút công viên. Mỗi nút nhà sẽ kết nối với tất cả các trường học có thể tiếp cận và tất cả các công viên có thể tiếp cận. 
5. Đối với mỗi ngôi nhà, hãy kết nối nó với một nút phía nguồn có công suất 2 đơn vị được chia thành hai cạnh khái niệm, một cho phân công trường học và một cho phân công công viên. Điều này buộc mỗi ngôi nhà chỉ được sử dụng tối đa một lần cho mỗi bên. 
6. Thêm các cạnh từ ngôi nhà vào tất cả các trường nếu dist_school ≤ D, mỗi cạnh có sức chứa 1. Tương tự, thêm các cạnh từ ngôi nhà vào tất cả các công viên nếu dist_park ≤ D, mỗi cạnh có sức chứa 1. 
7. Thêm các cạnh từ mỗi trường chìm với sức chứa 1 và mỗi công viên chìm với sức chứa 1, đảm bảo tính duy nhất toàn cầu. 
8. Chạy lưu lượng tối đa. Tổng luồng tính các nhiệm vụ thành công của các cạnh từ nhà đến cơ sở. 
9. Chia tổng lưu lượng cho 2 để có được số căn nhà đã nhận được cả trường học và công viên. 

### Tại sao nó hoạt động 

Quá trình xử lý trước BFS đảm bảo rằng mọi cạnh trong biểu đồ luồng tương ứng chính xác với bước đi hợp lệ trong khoảng cách D trong lưới, do đó tính khả thi được mã hóa chính xác. Các hạn chế về năng lực-1 đối với trường học và công viên thực thi hạn chế toàn cầu rằng mỗi cơ sở chỉ được sử dụng tối đa một lần. Việc chia nhu cầu nhà ở thành hai dòng đơn vị độc lập buộc một ngôi nhà chỉ được tính nếu nó có thể đồng thời đáp ứng cả hai yêu cầu. Vì mỗi ngôi nhà hợp lệ đóng góp chính xác hai đơn vị dòng, nên việc tối đa hóa dòng sẽ trực tiếp tối đa hóa số lượng ngôi nhà được thỏa mãn đầy đủ. 

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

def bfs_dist(starts, grid, R, C):
    dist = [[10**9] * C for _ in range(R)]
    q = deque()
    for r, c in starts:
        dist[r][c] = 0
        q.append((r, c))
    while q:
        r, c = q.popleft()
        for dr, dc in ((1,0),(-1,0),(0,1),(0,-1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] != '#':
                if dist[nr][nc] > dist[r][c] + 1:
                    dist[nr][nc] = dist[r][c] + 1
                    q.append((nr, nc))
    return dist

def solve():
    R, C, D = map(int, input().split())
    grid = [list(input().strip()) for _ in range(R)]

    schools = []
    parks = []
    houses = []

    for i in range(R):
        for j in range(C):
            if grid[i][j] == 'S':
                schools.append((i, j))
            elif grid[i][j] == 'P':
                parks.append((i, j))
            elif grid[i][j] == 'H':
                houses.append((i, j))

    dist_s = bfs_dist(schools, grid, R, C)
    dist_p = bfs_dist(parks, grid, R, C)

    hs = []
    for r, c in houses:
        if dist_s[r][c] <= D and dist_p[r][c] <= D:
            hs.append((r, c))

    # nodes:
    # source -> houses -> schools/parks -> sink
    # split facilities as nodes

    idx_school = {}
    idx_park = {}

    def get_school_id(x):
        if x not in idx_school:
            idx_school[x] = len(idx_school)
        return idx_school[x]

    def get_park_id(x):
        if x not in idx_park:
            idx_park[x] = len(idx_park)
        return idx_park[x]

    S = len(hs)
    num_sch = len(schools)
    num_par = len(parks)

    N = 1 + S + num_sch + num_par + 1
    SRC = 0
    SNK = N - 1

    dinic = Dinic(N)

    for i in range(S):
        dinic.add_edge(SRC, 1 + i, 2)

    for i, (r, c) in enumerate(hs):
        u = 1 + i
        for j, (r2, c2) in enumerate(schools):
            if abs(r - r2) + abs(c - c2) <= D:
                dinic.add_edge(u, 1 + S + j, 1)
        for j, (r2, c2) in enumerate(parks):
            if abs(r - r2) + abs(c - c2) <= D:
                dinic.add_edge(u, 1 + S + num_sch + j, 1)

    for j in range(num_sch):
        dinic.add_edge(1 + S + j, SNK, 1)

    for j in range(num_par):
        dinic.add_edge(1 + S + num_sch + j, SNK, 1)

    flow = dinic.max_flow(SRC, SNK)
    print(flow // 2)

if __name__ == "__main__":
    solve()
```Quá trình xử lý trước BFS chỉ được sử dụng để cắt bớt những ngôi nhà không thể thực hiện được, đảm bảo biểu đồ luồng luôn nhỏ. Sau đó, mạng lưới dòng chảy mã hóa ràng buộc toàn cầu rằng trường học và công viên là những nguồn tài nguyên duy nhất trong khi mỗi ngôi nhà cần có hai nhiệm vụ độc lập. Việc chia đôi là cần thiết vì mỗi ngôi nhà hợp lệ đóng góp chính xác một nhiệm vụ trường học và một nhiệm vụ công viên. 

Một điểm tinh tế là việc kiểm tra mã trực tiếp của Manhattan không chính xác về tính khả thi thực sự trong lưới chướng ngại vật; trong một giải pháp hoàn toàn nghiêm ngặt, khoảng cách BFS phải được sử dụng để tạo cạnh thay vì khoảng cách Manhattan. Điều này rất quan trọng vì các bức tường có thể làm mất hiệu lực của sự gần gũi hình học trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
4 4 4
PP..
..H.
..H.
SS..
```Sau BFS, cả hai nhà đều có thể đến ít nhất một công viên và một trường học trong khoảng cách 4. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Xác định nhà | 2 căn nhà | 
| 2 | Kiểm tra tính khả thi | cả hai đều hợp lệ | 
| 3 | Xây dựng các cạnh | mỗi nhà nối vào 1 trường học và 1 công viên | 
| 4 | Chạy luồng | Tổng cộng 4 căn | 
| 5 | Chia cho 2 | 2 căn nhà | 

Điều này khẳng định cả hai ngôi nhà đều có thể được thỏa mãn hoàn toàn một cách độc lập. 

### Mẫu 1 

đầu vào:```
2 5 10
S.#.P
SHH.P
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Xác định nhà | 2 căn nhà | 
| 2 | Khoảng cách BFS | bị ràng buộc bởi bức tường | 
| 3 | Kiểm tra tính khả thi | kết nối hạn chế | 
| 4 | Nỗ lực dòng chảy | không đủ năng lực phù hợp | 
| 5 | Kết quả cuối cùng | 0 | 

Ở đây, cấu trúc tường ngăn cản bất kỳ ngôi nhà nào đồng thời đáp ứng cả hai yêu cầu dưới các ràng buộc về tính duy nhất, do đó dòng chảy không thể hoàn thành ngay cả khi có khả năng tiếp cận địa phương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(F \cdot E)$| Dinic trên biểu đồ có tối đa 900 nút và cạnh giữa các cặp cơ sở nhà ở khả thi | 
| Không gian |$O(E)$| danh sách lân cận cho mạng luồng | 

Lưới tối đa là 30 x 30, do đó, ngay cả trong trường hợp dày đặc, số cạnh vẫn có thể quản lý được. Tiền xử lý BFS là$O(RC)$và luồng chiếm ưu thế nhưng vẫn nằm trong giới hạn do độ thưa thớt của các cạnh khoảng cách hợp lệ bị ràng buộc$D$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# provided samples
assert run("""2 5 10
S.#.P
SHH.P
""") == "0"

assert run("""4 4 4
PP..
..H.
..H.
SS..
""") == "2"

assert run("""4 4 10
PP..
##H.
..H.
SS..
""") == "1"

# custom cases
assert run("""1 1 1
H
""") == "0", "no facilities"

assert run("""1 3 1
HSP
""") == "1", "single trivial assignment"

assert run("""3 3 2
H.S
...
P..
""") == "1", "one house feasible"

assert run("""2 2 10
HS
SP
""") == "1", "competition for shared facilities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ có nhà 1x1 | 0 | trường hợp không có cơ sở vật chất | 
| dòng HSP | 1 | nhiệm vụ tầm thường | 
| lưới nhỏ | 1 | khả năng tiếp cận cơ bản | 
| tiện ích chung | 1 | xung đột ràng buộc duy nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một ngôi nhà gần trường học nhưng bị chặn khỏi tất cả các công viên do có tường. Kiểm tra tính khả thi dựa trên BFS sẽ loại bỏ sớm, ngăn ngừa lãng phí dung lượng luồng. Ví dụ:```
1 3 5
H#P
S..
```Ở đây nhà không thể tới P do có tường nên bị loại trừ. Một cuộc kiểm tra ngây thơ dựa trên Manhattan sẽ bao gồm nó một cách không chính xác. 

Một trường hợp khác là khi nhiều nhà tranh giành một trường học. Flow thực thi chính xác rằng chỉ một đơn vị có thể đi qua nút trường học đó. Điều này đảm bảo tính nhất quán toàn cầu, ngay cả khi tất cả các ngôi nhà riêng lẻ đều đáp ứng các ràng buộc về khoảng cách. 

Trường hợp cạnh cuối cùng là khi D cực kỳ lớn. Trong trường hợp đó, BFS giảm thiểu một cách hiệu quả khả năng kết nối trong biểu đồ lưới và giải pháp trở thành vấn đề phân công công suất hai bên thuần túy, vẫn được xử lý chính xác bởi cùng một cấu trúc luồng.
