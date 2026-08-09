---
title: "CF 102458A - Daniel và Perpendophobia"
description: "Câu chuyện hình học trở nên đơn giản hơn nhiều nếu chúng ta chỉ nhìn vào tọa độ. Giả sử Daniel đã ghi lại một điểm (u, v). Phải tránh khai thác mỏ (x, y) bất cứ khi nào x = u hoặc y = v, vì đoạn nối hai điểm khi đó là đoạn thẳng đứng hoặc nằm ngang."
date: "2026-08-09T02:37:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102458
codeforces_index: "A"
codeforces_contest_name: "Hanoi final contest"
rating: 0
weight: 102458
solve_time_s: 390
verified: true
draft: false
---

[CF 102458A - Daniel và chứng sợ Perpendophobia](https://codeforces.com/problemset/problem/102458/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Câu chuyện hình học trở nên đơn giản hơn nhiều nếu chúng ta chỉ nhìn vào tọa độ. Giả sử Daniel đã ghi được một điểm`(u, v)`. Một mỏ`(x, y)`phải tránh bất cứ khi nào`x = u`hoặc`y = v`, vì đoạn nối hai điểm khi đó là đoạn thẳng đứng hoặc nằm ngang. Nói cách khác, mỗi điểm được ghi lại sẽ làm cho toàn bộ đường thẳng đứng và đường ngang của nó không thể sử dụng được cho các mỏ trong tương lai. 

Nguồn gốc được ghi lại trước khi Daniel đến thăm bất cứ nơi nào, vì vậy mọi mỏ đều có`x = 0`hoặc`y = 0`ngay lập tức không thể sử dụng được. Bất cứ khi nào Daniel đến thăm mỏ`(x, y)`, giá trị tọa độ`x`trở nên không khả dụng mãi mãi và giá trị tọa độ`y`cũng trở nên không có sẵn mãi mãi. 

Điều này mang lại cấu trúc tổ hợp thực sự của vấn đề. Nếu chúng ta quyết định đi thăm nhiều mỏ thì không có hai mỏ nào được thăm quan giống nhau.`x`phối hợp và không có hai cái nào có thể giống nhau`y`điều phối. Ngược lại, bất kỳ tập hợp mỏ nào thỏa mãn hai điều kiện đó đều có thể được thăm quan theo bất kỳ thứ tự nào. Sau khi thăm quan một mỏ, chỉ có mỏ của riêng nó`x`Và`y`các giá trị bị cấm, vì vậy một mỏ khác có cả hai tọa độ vẫn chưa được sử dụng vẫn hợp pháp. 

Vì vậy, nhiệm vụ là chọn càng nhiều mỏ càng tốt sao cho tất cả các mỏ được chọn`x`tọa độ là khác biệt và tất cả được chọn`y`tọa độ khác biệt sau khi loại bỏ các mỏ nằm trên một trong hai trục. 

Có thể có tới`80,000`mỏ, trong khi tọa độ có thể lớn bằng`10^9`. Phạm vi tọa độ lớn có nghĩa là chúng ta không thể xây dựng một mảng được lập chỉ mục trực tiếp theo tọa độ. Quan trọng hơn,`N = 80,000`loại trừ các thuật toán bậc hai: thậm chí`O(N^2)`sẽ yêu cầu khoảng`6.4 * 10^9`kiểm tra cặp trong trường hợp xấu nhất. Chúng ta cần một thuật toán gần tuyến tính hoặc có lẽ`O(N sqrt N)`. 

Có một số trường hợp khó xử lý. Coi như```
1
0 7
```Câu trả lời là`0`, vì nguồn gốc đã được ghi lại`x = 0`, nên mỏ này bị cấm ngay lập tức. Giải pháp chỉ kiểm tra xem mỏ hiện tại có chưa được khám phá hay không sẽ tính sai. 

Bây giờ hãy xem xét```
3
1 1
1 2
2 3
```Câu trả lời là`2`. Chúng ta có thể ghé thăm`(1, 1)`và sau đó`(2, 3)`. Chúng tôi không thể đến thăm cả hai`(1, 1)`Và`(1, 2)`, bởi vì sau chuyến thăm đầu tiên, mọi mỏ đều có`x = 1`bị cấm. Một giải pháp bất cẩn chỉ kiểm tra xem`y`tọa độ khác nhau có thể tính cả ba. 

Một trường hợp tế nhị khác là```
4
0 1
1 2
2 3
3 0
```Câu trả lời là`2`. Hai quả mìn chạm vào một trục sẽ không thể sử dụng được ngay từ đầu, để lại`(1, 2)`Và`(2, 3)`. Họ có sự khác biệt`x`Và`y`tọa độ, vì vậy cả hai đều có thể được truy cập. Chỉ cần coi mọi mỏ đầu vào là một lợi thế có sẵn sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể mô phỏng mọi chuỗi truy cập có thể xảy ra. Tại mỗi tiểu bang, nó kiểm tra tất cả các mỏ và chọn đệ quy mọi mỏ hiện hợp pháp. Điều này đúng vì mọi trình tự pháp lý đều được xem xét, do đó số lượng mỏ đã ghé thăm lớn nhất phải xuất hiện trong tìm kiếm. 

Vấn đề là số lượng trình tự. Trong một cấu hình có thể chọn nhiều mỏ, việc tìm kiếm có thể khám phá các hoán vị của hầu hết`N`mỏ. Số tiền tố có thể có là theo thứ tự`1 + N + N(N-1) + ... + N!`, 

đó là`O(N!)`. Ngay cả khi việc kiểm tra một ứng viên diễn ra liên tục,`N!`đã là vô vọng rồi. Tại`N = 20`,`20!`là về`2.43 * 10^18`, vượt xa mọi thứ có thể thực hiện được. 

Lực lượng vũ phu hoạt động vì một lượt truy cập chỉ thay đổi các giá trị tọa độ có sẵn. Quan sát đó cho phép chúng ta loại bỏ hoàn toàn hình học. Đưa ra mọi sự khác biệt`x`tọa độ một đỉnh bên trái và mọi điểm khác biệt`y`tọa độ một đỉnh bên phải. Mỗi mỏ`(x, y)`trở thành một cạnh kết nối nó`x`đỉnh của nó`y`đỉnh. 

Bây giờ chọn mỏ có nghĩa là chọn cạnh tương ứng của nó. Cả hai mỏ đều có thể được truy cập chính xác khi các cạnh của chúng không có chung điểm cuối. Vì vậy, một tập hợp các mỏ hợp lệ chính xác là sự trùng khớp trong biểu đồ hai bên này. 

Do đó, số lượng mỏ tối đa mà Daniel có thể khám phá là kích thước của sự kết hợp lưỡng cực tối đa. Mỏ có`x = 0`hoặc`y = 0`bị bỏ qua vì những tọa độ đó đã bị chiếm giữ bởi điểm gốc được ghi. 

Một thuật toán so khớp đường dẫn tăng cường đơn giản sẽ liên tục tìm kiếm trong toàn bộ biểu đồ và có thể trở thành bậc hai. Với tối đa`80,000`các cạnh, cách triển khai thích hợp là Hopcroft-Karp, tìm các đường dẫn tăng cường theo đợt và chạy theo`O(E sqrt V)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) đệ quy cộng với trạng thái | Quá chậm | 
| Tối ưu | O(N sqrt N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mỏ và loại bỏ mọi mỏ có`x = 0`hoặc`y = 0`. Điểm gốc đã ghi cả hai giá trị tọa độ nên không bao giờ có thể chọn được những mỏ đó. 
2. Nén phần còn lại riêng biệt`x`tọa độ và phân biệt`y`tọa độ thành các ID số nguyên liên tiếp. Tọa độ ban đầu có thể lớn bằng`10^9`, nhưng chỉ có sự bằng nhau giữa các tọa độ mới quan trọng, nên độ lớn thực tế của chúng là không liên quan. 
3. Tạo biểu đồ lưỡng cực. Đối với mỗi mỏ còn lại`(x, y)`, thêm một cạnh từ đỉnh nén biểu thị`x`đến đỉnh đại diện`y`. Mỗi cạnh tương ứng với chính xác một mỏ. 
4. Duy trì`pair_left[x]`, đỉnh bên phải hiện khớp với đỉnh bên trái và`pair_right[y]`, đỉnh bên trái hiện được khớp với đỉnh bên phải. Một đỉnh chưa từng có được biểu diễn bằng`-1`. 
5. Chạy Hopcroft-Karp. Đầu tiên thực hiện BFS từ mọi đỉnh bên trái chưa từng có. BFS xây dựng các lớp thông qua các cạnh khớp và khớp xen kẽ nhau và ghi lại khoảng cách ngắn nhất mà tại đó có thể đạt tới đỉnh phải tự do. 
6. Chạy DFS từ mọi đỉnh bên trái hiện chưa được so sánh, chỉ theo các cạnh tôn trọng các lớp BFS. Bất cứ khi nào DFS đạt đến một đỉnh bên phải chưa từng có, một đường dẫn tăng cường đã được tìm thấy, do đó tất cả các cạnh dọc theo đường dẫn đó có thể bị lật và kích thước phù hợp sẽ tăng thêm một. 
7. Lặp lại các giai đoạn BFS và DFS cho đến khi không còn đường tăng cường nào tồn tại. Tại thời điểm đó, kết quả phù hợp hiện tại là tối đa, vì vậy kích thước của nó là câu trả lời. 

Tại sao nó hoạt động được ghi lại bởi bất biến phù hợp. Tại mọi thời điểm, các cạnh được chọn đại diện cho các mỏ có phân biệt theo cặp`x`Và`y`tọa độ, từ đó hình thành nên một kế hoạch thăm dò khả thi. Bất cứ khi nào Hopcroft-Karp tìm thấy một đường dẫn tăng cường, việc lật các cạnh của nó sẽ giữ nguyên thuộc tính phù hợp trong khi tăng kích thước của nó lên đúng một. Khi BFS không còn có thể tìm thấy đường dẫn tăng cường, định lý đường tăng cường cho biết rằng không tồn tại kết quả phù hợp nào lớn hơn. Vì các kết quả phù hợp và các tập hợp mỏ khả thi hoàn toàn giống nhau ở đây nên kích thước phù hợp cuối cùng chính xác là số lượng mỏ tối đa mà Daniel có thể khám phá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def maximum_mines(data):
    it = iter(map(int, data.split()))
    n = next(it)

    mines = []
    xs = {}
    ys = {}

    for _ in range(n):
        x = next(it)
        y = next(it)

        # The origin has already forbidden x = 0 and y = 0.
        if x == 0 or y == 0:
            continue

        if x not in xs:
            xs[x] = len(xs)
        if y not in ys:
            ys[y] = len(ys)

        mines.append((xs[x], ys[y]))

    nx = len(xs)
    ny = len(ys)

    graph = [[] for _ in range(nx)]
    for x, y in mines:
        graph[x].append(y)

    pair_left = [-1] * nx
    pair_right = [-1] * ny
    dist = [-1] * nx

    from collections import deque

    def bfs():
        q = deque()
        found = False

        for u in range(nx):
            if pair_left[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        while q:
            u = q.popleft()

            for v in graph[u]:
                matched_u = pair_right[v]

                if matched_u == -1:
                    found = True
                elif dist[matched_u] == -1:
                    dist[matched_u] = dist[u] + 1
                    q.append(matched_u)

        return found

    sys.setrecursionlimit(max(1_000_000, nx * 2 + 10))

    def dfs(u):
        for v in graph[u]:
            matched_u = pair_right[v]

            if matched_u == -1 or (
                dist[matched_u] == dist[u] + 1 and dfs(matched_u)
            ):
                pair_left[u] = v
                pair_right[v] = u
                return True

        dist[u] = -1
        return False

    matching = 0

    while bfs():
        for u in range(nx):
            if pair_left[u] == -1 and dfs(u):
                matching += 1

    return str(matching)

def main():
    data = sys.stdin.buffer.read()
    sys.stdout.write(maximum_mines(data))

if __name__ == "__main__":
    main()
```Lượt đầu tiên nén tọa độ trong khi đọc mỏ. Việc sử dụng từ điển là cần thiết vì tọa độ có phạm vi lên tới`10^9`; ID nén nhiều nhất là`N - 1`. 

Biểu đồ chứa một cạnh cho mỗi mỏ có thể sử dụng được. Không cần lưu trữ tọa độ ban đầu sau khi nén, vì thao tác duy nhất mà thuật toán cần là kiểm tra xem hai tọa độ có bằng nhau hay không.`pair_left`Và`pair_right`mô tả sự phù hợp hiện tại từ cả hai hướng. Việc có cả hai mảng giúp bạn có thể đi theo con đường xen kẽ một cách hiệu quả. Nếu một đỉnh bên phải đã được khớp,`pair_right[v]`ngay lập tức cho chúng ta biết đỉnh bên trái nào phải được thăm tiếp theo. 

BFS xây dựng các lớp được Hopcroft-Karp sử dụng. Một đỉnh bên phải chưa khớp có nghĩa là BFS hiện tại đã tìm thấy điểm cuối có thể có của đường dẫn tăng cường. Sau đó, DFS chỉ tìm kiếm thông qua cấu trúc phân lớp, tránh các đường dẫn tùy ý không thể thuộc về đường dẫn tăng cường ngắn nhất. 

DFS đệ quy an toàn sau khi tăng giới hạn đệ quy của Python. Một đồ thị với`80,000`các cạnh có thể tạo ra một đường dẫn xen kẽ dài, do đó việc dựa vào giới hạn đệ quy mặc định của Python sẽ không an toàn. 

Không có vấn đề tràn số nguyên trong Python và bản thân tọa độ không bao giờ được sử dụng trong số học. Câu trả lời nhiều nhất là`N`, do đó bộ đếm phù hợp cũng nhỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
5
100 400
200 200
200 300
300 400
400 100
```Đồ thị nén có các đỉnh còn lại biểu thị`100, 200, 300, 400`và các đỉnh bên phải biểu thị`100, 200, 300, 400`. 

| Bước | Đỉnh trái | Đỉnh được chọn đúng | Phù hợp | 
| --- | --- | --- | --- | 
| 1 | 100 | 400 |`(100,400)`| 
| 2 | 200 | 200 |`(100,400), (200,200)`| 
| 3 | 300 | 400 | không thể giữ được điều này,`400`bị chiếm đóng | 
| 3 | 300 | 400 thông qua việc phân công lại | sắp xếp lại nếu có thể | 
| 3 | 300 | không có quyền miễn phí ngay sau các lựa chọn hiện tại | thử một con đường tăng cường khác | 
| 3 | 400 | 100 |`(100,400), (200,200), (400,100)`| 

Sự phù hợp tối đa có kích thước`3`. Ví dụ, ba mỏ`(100,400)`,`(200,200)`, Và`(400,100)`có thể được truy cập. Đang cố gắng thêm`(300,400)`thất bại vì nó`y = 400`xung đột với mỏ đầu tiên. 

Dấu vết chứng minh tại sao đây không chỉ đơn giản là vấn đề đếm các phân biệt`x`Và`y`tọa độ. Các cạnh xác định những kết hợp nào thực sự có sẵn, do đó cần có một thuật toán phù hợp. 

Đối với mẫu thứ hai,```
4
0 1
1 2
2 3
3 0
```Hai mỏ trục biến mất ngay lập tức. 

| Bước | Của tôi | Hành động | Kích thước phù hợp | 
| --- | --- | --- | --- | 
| 1 |`(0,1)`| vứt bỏ vì`x = 0`| 0 | 
| 2 |`(1,2)`| thêm vào phù hợp | 1 | 
| 3 |`(2,3)`| thêm vào phù hợp | 2 | 
| 4 |`(3,0)`| vứt bỏ vì`y = 0`| 2 | 

Câu trả lời là`2`. Hai mỏ còn sót lại sử dụng những cách khác nhau`x`tọa độ và khác nhau`y`tọa độ, do đó chúng tạo thành một kết quả khớp hợp lệ và cả hai đều có thể được khám phá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N sqrt N) | Đồ thị có nhiều nhất`N`các cạnh và`O(N)`các đỉnh và Hopcroft-Karp lấy`O(E sqrt V)`| 
| Không gian | O(N) | Bản đồ tọa độ, cạnh đồ thị, mảng khớp, khoảng cách và hàng đợi BFS đều tuyến tính | 

Với`N <= 80,000`, đồ thị chứa nhiều nhất`80,000`các cạnh và nhiều nhất`160,000`các đỉnh bị nén. Giải pháp này tránh mọi lần quét bậc hai trên các cặp mỏ và chỉ sử dụng lưu trữ đồ thị tuyến tính, do đó nó phù hợp với nhiệm vụ con lớn dự định và`512 MB`giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
from collections import deque

def solve_string(data: str) -> str:
    it = iter(map(int, data.split()))
    n = next(it)

    mines = []
    xs = {}
    ys = {}

    for _ in range(n):
        x = next(it)
        y = next(it)

        if x == 0 or y == 0:
            continue

        if x not in xs:
            xs[x] = len(xs)
        if y not in ys:
            ys[y] = len(ys)

        mines.append((xs[x], ys[y]))

    nx = len(xs)
    ny = len(ys)

    graph = [[] for _ in range(nx)]
    for x, y in mines:
        graph[x].append(y)

    left = [-1] * nx
    right = [-1] * ny
    dist = [-1] * nx

    def bfs():
        q = deque()
        found = False

        for u in range(nx):
            if left[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        while q:
            u = q.popleft()

            for v in graph[u]:
                w = right[v]

                if w == -1:
                    found = True
                elif dist[w] == -1:
                    dist[w] = dist[u] + 1
                    q.append(w)

        return found

    sys.setrecursionlimit(max(1_000_000, nx * 2 + 10))

    def dfs(u):
        for v in graph[u]:
            w = right[v]

            if w == -1 or (
                dist[w] == dist[u] + 1 and dfs(w)
            ):
                left[u] = v
                right[v] = u
                return True

        dist[u] = -1
        return False

    ans = 0

    while bfs():
        for u in range(nx):
            if left[u] == -1 and dfs(u):
                ans += 1

    return str(ans)

def run(inp: str) -> str:
    return solve_string(inp)

# Provided sample 1
assert run(
    """5
100 400
200 200
200 300
300 400
400 100
"""
) == "3", "sample 1"

# Provided sample 2
assert run(
    """4
0 1
1 2
2 3
3 0
"""
) == "2", "sample 2"

# Minimum-size input
assert run(
    """1
7 9
"""
) == "1", "single usable mine"

# All mines share the same x coordinate
assert run(
    """5
1 1
1 2
1 3
1 4
1 5
"""
) == "1", "all equal x coordinates"

# Axis boundaries plus a valid pair
assert run(
    """5
0 10
10 0
1 2
2 3
3 4
"""
) == "3", "axis mines must be ignored"

# Alternating-path case
assert run(
    """4
1 1
1 2
2 1
3 3
"""
) == "3", "augmenting path must rearrange an earlier match"

# Maximum-size input: 80,000 distinct independent mines
large = ["80000"]
large.extend(f"{i} {i}" for i in range(1, 80001))
assert run("\n".join(large)) == "80000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7 9`|`1`| Đầu vào tối thiểu và khớp một cạnh cơ bản | 
| Năm mỏ với`x = 1`|`1`| Tọa độ lặp đi lặp lại không thể được truy cập hai lần | 
| Mỏ trên cả hai trục cộng`(1,2),(2,3),(3,4)`|`3`| Xử lý đúng tọa độ cấm của nguồn gốc | 
|`(1,1),(1,2),(2,1),(3,3)`|`3`| Đường dẫn tăng cường có thể thay thế lựa chọn khớp trước đó | 
|`80,000`mỏ chéo |`80,000`| Kích thước đầu vào tối đa và xây dựng đồ thị tuyến tính | 

## Vỏ cạnh 

Mỏ trên một trục được xử lý trước khi xây dựng biểu đồ. Vì```
1
0 7
```mỏ bị loại bỏ vì nó`x`tọa độ đã có sẵn trong nguồn gốc. Không còn cạnh đồ thị nào, vì vậy Hopcroft-Karp trả về`0`. Lý do tương tự cũng áp dụng cho một mỏ như`(8,0)`. 

Khi nhiều mỏ có cùng tọa độ thì chúng đều trở thành các cạnh liên tiếp với một đỉnh đồ thị. Vì```
5
1 1
1 2
1 3
1 4
1 5
```chỉ có một đỉnh trái và năm đỉnh phải. Một kết quả khớp chỉ có thể chứa một cạnh vì mọi cạnh đều chia sẻ điểm cuối bên trái. Thuật toán trả về`1`, hoàn toàn phù hợp với thực tế là việc ghé thăm bất kỳ mỏ nào trong số này sẽ cấm vĩnh viễn bốn mỏ còn lại. 

Bản thân nguồn gốc không phải là mỏ đầu vào, nhưng tọa độ của nó vẫn quan trọng. TRONG```
4
0 1
1 2
2 3
3 0
```mỏ đầu tiên và cuối cùng bị loại bỏ vì chúng có chung tọa độ với nguồn gốc được ghi. Hai cạnh còn lại rời nhau, cho kích thước phù hợp`2`. Điều này ngăn ngừa lỗi phổ biến khi xây dựng kết quả khớp trên tất cả các cạnh đầu vào mà không tính đến trạng thái ban đầu. 

Trường hợp đường dẫn tăng cường```
4
1 1
1 2
2 1
3 3
```có một cấu trúc đặc biệt hữu ích. Một thuật toán tham lam ngây thơ trước tiên có thể chọn`(1,1)`, chặn cả hai`(1,2)`Và`(2,1)`. Thay vào đó, một kết quả phù hợp tối đa sẽ chọn`(1,2)`Và`(2,1)`, sau đó`(3,3)`, đạt`3`. Hopcroft-Karp có thể khám phá điều này thông qua một con đường tăng cường thay đổi lựa chọn trước đó. Đây là lý do tại sao việc kết hợp tham lam tùy ý là không đủ. 

Cuối cùng, giới hạn tọa độ của`10^9`không tạo ra trường hợp đặc biệt trong quá trình thực hiện. Tọa độ được lưu trữ dưới dạng khóa từ điển và được nén ngay lập tức thành ID số nguyên nhỏ. Thuật toán không bao giờ phân bổ một mảng có kích thước tỷ lệ với giá trị tọa độ, do đó, một mỏ như`(1000000000, 999999999)`chi phí chính xác bằng dung lượng lưu trữ biểu đồ như một mỏ như`(1,2)`.
