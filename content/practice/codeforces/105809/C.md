---
title: "CF 105809C - Cờ vua 3D"
description: "Chúng ta có một bàn cờ ba chiều có kích thước $A nhân B nhân C$. Một số ô bị chặn và không thể chứa hiệp sĩ. Mỗi ô còn lại đều là vị trí tiềm năng cho hiệp sĩ 3D."
date: "2026-06-25T15:28:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "C"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 54
verified: true
draft: false
---

[CF 105809C - Cờ vua 3D](https://codeforces.com/problemset/problem/105809/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ ba chiều với các kích thước$A \times B \times C$. Một số ô bị chặn và không thể chứa hiệp sĩ. Mỗi ô còn lại đều là vị trí tiềm năng cho hiệp sĩ 3D. 

Một hiệp sĩ di chuyển chính xác như được mô tả bởi mô hình cờ vua thông thường được mở rộng thành ba chiều. Nó chọn hai trục, di chuyển 2 dọc theo một trong số chúng và 1 dọc theo trục kia, trong khi tọa độ thứ ba không thay đổi. Bất kỳ động thái nào rời khỏi bảng đều không hợp lệ. Mục tiêu là đặt càng nhiều hiệp sĩ càng tốt để không có hai hiệp sĩ nào tấn công lẫn nhau trong một nước đi. 

Kích thước của bảng nhiều nhất là 10 ở mỗi hướng, do đó toàn bộ bảng chứa nhiều nhất$10^3 = 1000$tế bào. Ngay cả trước khi loại bỏ các ô bị chặn, số lượng vị trí có thể sử dụng vẫn đủ nhỏ để chúng ta có thể nghĩ dưới dạng các đỉnh của đồ thị. Thách thức không phải là số lượng ô mà là bản chất tổ hợp của việc chọn tập hợp con không tấn công lớn nhất. 

Một tìm kiếm đơn giản trên tất cả các tập hợp con sẽ yêu cầu kiểm tra tới$2^{1000}$cấu hình, điều này là hoàn toàn không thể. Chúng ta cần khai thác cấu trúc trong đồ thị nước đi hiệp sĩ. 

Một trường hợp tinh vi xuất hiện khi bàn cờ quá nhỏ đến mức không có nước đi hiệp sĩ nào tồn tại. 

Ví dụ:```
1 1 1
1
1 1 1
```Không có ô nào có thể sử dụng được nên câu trả lời là 0. Giải pháp giả định rằng có thể đặt ít nhất một hiệp sĩ sẽ không thành công. 

Một trường hợp thú vị khác là một bảng có sẵn các ô nhưng không có cặp tấn công.```
1 1 2
1
1 1 1
```Chỉ còn lại một ô có thể sử dụng được. Câu trả lời là 1. Mọi giải pháp dựa trên đồ thị đều phải xử lý chính xác các đỉnh bị cô lập. 

Một sai lầm nguy hiểm hơn là quên xóa các ô bị chặn khỏi biểu đồ. Coi như:```
3 3 3
1
2 2 2
```Ô trung tâm không có sẵn. Nếu chúng ta vẫn tạo các cạnh thông qua đỉnh đó hoặc tính nó là vị trí ứng cử viên, câu trả lời cuối cùng sẽ trở nên quá lớn hoặc quá nhỏ tùy thuộc vào cách triển khai. Câu trả lời đúng là 14. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp nhất là xây dựng một biểu đồ trong đó mỗi ô có thể sử dụng được là một đỉnh và một cạnh nối hai ô nếu các hiệp sĩ được đặt ở đó tấn công lẫn nhau. Nhiệm vụ sau đó trở thành tìm tập hợp đỉnh lớn nhất không có cạnh giữa bất kỳ cặp nào. Trong lý thuyết đồ thị, đó là tập độc lập tối đa. 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của các ô có thể sử dụng được và kiểm tra xem có cặp tấn công nào tồn tại hay không. Nếu có$N$các tế bào có thể sử dụng được, điều này đòi hỏi$2^N$tập hợp con. Ngay cả đối với$N = 50$, điều này đã là vô vọng rồi, trong khi giới hạn thực tế lên tới 1000 ô. 

Quan sát chính xuất phát từ tính chẵn lẻ của tọa độ. 

Một nước đi của hiệp sĩ thay đổi tọa độ bằng cách$(\pm2,\pm1,0)$theo một thứ tự nào đó. Tổng số thay đổi trong$x+y+z$luôn luôn kỳ lạ bởi vì$2+1=3$. Điều đó có nghĩa là mỗi bước đi của hiệp sĩ sẽ lật đổ tính chẵn lẻ của$x+y+z$. 

Nếu chúng ta tô màu từng ô theo tính chẵn lẻ của$x+y+z$, mọi cạnh đều kết nối các màu đối diện. Biểu đồ tấn công là lưỡng cực. 

Bây giờ vấn đề trở nên dễ dàng hơn nhiều nhờ một định lý cổ điển. Trong bất kỳ biểu đồ nào:$$\text{Maximum Independent Set}
=
|V|
-
\text{Minimum Vertex Cover}$$Đối với đồ thị lưỡng cực, định lý König phát biểu:$$\text{Minimum Vertex Cover}
=
\text{Maximum Matching}$$Kết hợp cả hai sự thật:$$\text{Answer}
=
|V|
-
\text{Maximum Matching}$$Vì vậy, thay vì tìm kiếm trực tiếp một tập hợp độc lập lớn nhất, chúng tôi xây dựng biểu đồ lưỡng cực về các cuộc tấn công hiệp sĩ, tính toán kết quả khớp tối đa và trừ kích thước của nó khỏi số lượng ô có thể sử dụng. 

Bảng có tối đa 1000 ô có thể sử dụng được. Mỗi ô chỉ có một số nước đi hiệp sĩ không đổi, do đó đồ thị vẫn còn thưa thớt. Hopcroft-Karp dễ dàng xử lý kích thước này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(E\sqrt{V})$|$O(V+E)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng và đánh dấu tất cả các ô bị chặn. 
2. Gán một id số nguyên cho mỗi ô có thể sử dụng được. Các id này trở thành các đỉnh đồ thị. 
3. Chia các đỉnh thành hai phân vùng theo tính chẵn lẻ của$x+y+z$. 
4. Đối với mỗi ô có thể sử dụng được trong phân vùng chẵn, hãy tạo tất cả các nước đi hiệp sĩ có thể có. 
5. Nếu ô đích nằm bên trong bảng và không bị chặn, hãy thêm một cạnh vào giữa hai đỉnh tương ứng. 
6. Chạy Hopcroft-Karp trên biểu đồ hai bên thu được để tính toán mức khớp tối đa. 
7. Hãy để$N$là số lượng ô có thể sử dụng được và$M$là kích thước phù hợp tối đa. 
8. Đầu ra$N-M$. 

Lý do bước 8 đúng là chuỗi đẳng thức:$$\text{Maximum Independent Set}
=
N-\text{Minimum Vertex Cover}
=
N-\text{Maximum Matching}$$### Tại sao nó hoạt động 

Mỗi bước di chuyển của hiệp sĩ đều thay đổi tính chẵn lẻ, do đó biểu đồ tấn công là lưỡng cực. Bất kỳ vị trí hiệp sĩ hợp lệ nào đều tương ứng chính xác với một tập hợp độc lập trong biểu đồ này vì không có hai đỉnh được chọn nào có thể chia sẻ một cạnh tấn công. 

Đối với đồ thị hai bên, định lý König chuyển đổi bài toán che đỉnh tối thiểu thành bài toán so khớp tối đa. Vì phần bù của lớp phủ đỉnh tối thiểu là một tập hợp độc lập tối đa, nên việc trừ đi kích thước phù hợp từ tổng số đỉnh sẽ tạo ra tập hợp lớn nhất có thể gồm các hiệp sĩ không tấn công lẫn nhau. Không có vị trí hợp lệ nào có thể chứa nhiều đỉnh hơn giá trị này và định lý đảm bảo rằng vị trí đó tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    A, B, C = map(int, input().split())
    K = int(input())

    blocked = set()
    for _ in range(K):
        x, y, z = map(int, input().split())
        blocked.add((x - 1, y - 1, z - 1))

    cell_id = {}
    cells = []

    for x in range(A):
        for y in range(B):
            for z in range(C):
                if (x, y, z) not in blocked:
                    cell_id[(x, y, z)] = len(cells)
                    cells.append((x, y, z))

    n = len(cells)

    moves = []
    for axis2 in [2, -2]:
        for axis1 in [1, -1]:
            moves.extend([
                (axis2, axis1, 0),
                (axis2, 0, axis1),
                (axis1, axis2, 0),
                (0, axis2, axis1),
                (axis1, 0, axis2),
                (0, axis1, axis2),
            ])

    left_vertices = []
    for idx, (x, y, z) in enumerate(cells):
        if (x + y + z) % 2 == 0:
            left_vertices.append(idx)

    adj = [[] for _ in range(n)]

    for idx in left_vertices:
        x, y, z = cells[idx]

        for dx, dy, dz in moves:
            nx, ny, nz = x + dx, y + dy, z + dz

            if not (0 <= nx < A and 0 <= ny < B and 0 <= nz < C):
                continue

            if (nx, ny, nz) not in cell_id:
                continue

            adj[idx].append(cell_id[(nx, ny, nz)])

    INF = 10 ** 18

    pair_u = [-1] * n
    pair_v = [-1] * n
    dist = [0] * n

    def bfs():
        q = deque()

        for u in left_vertices:
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
        for u in left_vertices:
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    print(n - matching)

solve()
```Phần đầu tiên xây dựng tập hợp các ô có thể sử dụng và gán cho mỗi ô một id đỉnh nhỏ gọn. Làm việc với các id số nguyên giúp việc triển khai đối sánh trở nên đơn giản hơn nhiều. 

Thế hệ nước đi rõ ràng tạo ra tất cả 24 nước đi hiệp sĩ trong ba chiều. Một số cách triển khai vô tình bỏ lỡ một số hoán vị tọa độ, tạo ra một biểu đồ bị thiếu các cạnh và kết quả quá lớn. 

Chỉ các đỉnh chẵn lẻ mới tạo ra các cạnh đi ra. Điều này tránh việc lưu trữ mỗi cạnh hai lần và tạo thành phía bên trái của đồ thị hai bên một cách tự nhiên. 

Việc triển khai Hopcroft-Karp sử dụng giai đoạn phân lớp BFS tiêu chuẩn, sau đó là tăng cường DFS. Biểu đồ thưa thớt nên điều này diễn ra thoải mái trong giới hạn. 

Một nguồn lỗi phổ biến là quên rằng các ô bị chặn không tồn tại dưới dạng đỉnh. Mọi điểm đến đều phải được kiểm tra`cell_id`trước khi thêm một cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
1 1 2
1
1 1 1
```Các tế bào có thể sử dụng: 

| Tế bào | Chẵn lẻ | 
| --- | --- | 
| (1,1,2) | lẻ | 

Không có bước di chuyển hiệp sĩ. 

| Số lượng | Giá trị | 
| --- | --- | 
| Các đỉnh có thể sử dụng | 1 | 
| Kích thước phù hợp | 0 | 
| Trả lời | 1 | 

Điều này chứng tỏ rằng các đỉnh cô lập tự động thuộc về tập độc lập lớn nhất. 

### Ví dụ 2```
3 3 3
1
2 2 2
```Có tổng cộng 26 ô và một ô bị chặn. 

| Số lượng | Giá trị | 
| --- | --- | 
| Tổng số ô | 27 | 
| Ô bị chặn | 1 | 
| Các đỉnh có thể sử dụng | 26 | 

Đang chạy Hopcroft-Karp tìm thấy: 

| Số lượng | Giá trị | 
| --- | --- | 
| Kết hợp tối đa | 12 | 
| Trả lời | 14 | 

Kết quả phù hợp với đầu ra mẫu. 

Ví dụ này thể hiện sự giảm thiểu hoàn toàn từ vị trí hiệp sĩ sang đối sánh hai bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(E\sqrt{V})$| Kết hợp tối đa Hopcroft-Karp | 
| Không gian |$O(V+E)$| Lưu trữ đồ thị và mảng khớp | 

Bảng chứa tối đa 1000 ô có thể sử dụng được. Mỗi ô có tối đa 24 nước đi hiệp sĩ, vì vậy$E$chỉ có vài chục nghìn. Hopcroft-Karp trên biểu đồ có kích thước này dễ dàng đủ nhanh để đạt giới hạn một giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from collections import deque

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    A, B, C = map(int, input().split())
    K = int(input())

    blocked = set()
    for _ in range(K):
        x, y, z = map(int, input().split())
        blocked.add((x - 1, y - 1, z - 1))

    cell_id = {}
    cells = []

    for x in range(A):
        for y in range(B):
            for z in range(C):
                if (x, y, z) not in blocked:
                    cell_id[(x, y, z)] = len(cells)
                    cells.append((x, y, z))

    n = len(cells)

    moves = []
    for a in [2, -2]:
        for b in [1, -1]:
            moves.extend([
                (a, b, 0),
                (a, 0, b),
                (b, a, 0),
                (0, a, b),
                (b, 0, a),
                (0, b, a),
            ])

    left = [i for i, (x, y, z) in enumerate(cells)
            if (x + y + z) % 2 == 0]

    adj = [[] for _ in range(n)]

    for u in left:
        x, y, z = cells[u]
        for dx, dy, dz in moves:
            nx, ny, nz = x + dx, y + dy, z + dz
            if (nx, ny, nz) in cell_id:
                adj[u].append(cell_id[(nx, ny, nz)])

    INF = 10**18
    pair_u = [-1] * n
    pair_v = [-1] * n
    dist = [0] * n

    def bfs():
        q = deque()
        found = False

        for u in left:
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = INF

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
        for u in left:
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    return str(n - matching) + "\n"

# sample
assert run("3 3 3\n1\n2 2 2\n") == "14\n"

# minimum usable board
assert run("1 1 1\n1\n1 1 1\n") == "0\n"

# single available cell
assert run("1 1 2\n1\n1 1 1\n") == "1\n"

# no knight moves anywhere
assert run("2 2 2\n1\n1 1 1\n") == "7\n"

# one-dimensional line
assert run("1 1 3\n1\n1 1 1\n") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1×1 bị chặn hoàn toàn | 0 | Biểu đồ trống | 
| 1×1×2 với một ô bị chặn | 1 | Đỉnh cô lập đơn | 
| 2×2×2 | 7 | Không có nước đi hiệp sĩ hợp lệ nào tồn tại | 
| 1×1×3 | 2 | Xử lý kích thước suy biến | 
| Trường hợp mẫu | 14 | Logic phù hợp đầy đủ | 

## Vỏ cạnh 

Hãy xem xét bảng bị chặn hoàn toàn:```
1 1 1
1
1 1 1
```Không có đỉnh nào được tạo ra. Kích thước phù hợp là 0 và đầu ra thuật toán$0 - 0 = 0$. Vị trí trống là vị trí hợp lệ duy nhất. 

Hãy xem xét một bảng có một ô có sẵn:```
1 1 2
1
1 1 1
```Đồ thị có một đỉnh và không có cạnh. Hopcroft-Karp không tìm thấy kết quả phù hợp. Câu trả lời trở thành$1 - 0 = 1$, điều này là tối ưu vì ô đơn độc luôn có thể chứa một hiệp sĩ. 

Hãy xem xét mẫu:```
3 3 3
1
2 2 2
```Ô trung tâm bị chặn không bao giờ được chèn vào biểu đồ. Mọi cạnh tấn công chỉ được tạo giữa các ô có thể sử dụng được. Kích thước phù hợp là 12, do đó kích thước bộ độc lập tối đa là$26 - 12 = 14$. Điều này xác nhận rằng các ô bị chặn được xử lý chính xác và không tham gia vào biểu đồ hoặc số lượng cuối cùng.
