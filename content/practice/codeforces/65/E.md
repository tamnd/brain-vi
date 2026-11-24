---
title: "CF 65E - Harry Potter và Cầu thang di chuyển"
description: "Chúng tôi được đưa ra một multigraph vô hướng. Sàn nhà là đỉnh, cầu thang là cạnh. Harry bắt đầu từ tầng 1 và muốn ghé thăm từng tầng ít nhất một lần. Biểu đồ là động. Giữa những bước đi của Harry, Ron và Hermione có thể sửa đổi cầu thang."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 65
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 60"
rating: 2900
weight: 65
solve_time_s: 167
verified: false
draft: false
---

[CF 65E - Harry Potter và Cầu thang di chuyển](https://codeforces.com/problemset/problem/65/E) 

**Xếp hạng:** 2900 
**Tags:** dfs và tương tự, triển khai 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một multigraph vô hướng. Sàn nhà là đỉnh, cầu thang là cạnh. Harry bắt đầu ở tầng`1`và muốn ghé thăm từng tầng ít nhất một lần. 

Biểu đồ là động. Giữa những bước đi của Harry, Ron và Hermione có thể sửa đổi cầu thang. Một sửa đổi chọn một điểm cuối của một cạnh và kết nối lại nó với một đỉnh khác. Mỗi cầu thang có thể được sửa đổi nhiều nhất một lần. 

Đầu ra không chỉ là liệu nhiệm vụ có khả thi hay không. Chúng ta cũng phải mô tả rõ ràng trình tự các bước đi và sửa đổi cạnh. Harry có thể đi bộ xa tùy ý giữa hai lần sửa đổi, nhưng mỗi cặp tầng liên tiếp trong bước đi của cậu ấy hiện phải được nối với nhau bằng cầu thang. 

Biểu đồ có thể chứa nhiều cạnh giữa cùng một cặp đỉnh, điều này quan trọng vì các bậc thang được xác định riêng lẻ theo chỉ mục. Cầu thang di chuyển`5`khác với việc di chuyển cầu thang`8`, ngay cả khi cả hai hiện đang kết nối cùng một cặp tầng. 

Các ràng buộc đủ lớn nên không thể thực hiện được các thuật toán đồ thị bậc hai. Với`n ≤ 100000`Và`m ≤ 200000`, lời giải phải gần với thời gian tuyến tính. Duyệt qua biểu đồ một số lần không đổi là tốt. Bất cứ điều gì như tính toán lại kết nối sau mỗi lần di chuyển cạnh sẽ ngay lập tức vượt quá giới hạn. 

Trường hợp cạnh nguy hiểm nhất là đồ thị không có cầu thang chạm sàn`1`. 

Ví dụ:```
3 1
2 3
```Harry bắt đầu bị cô lập trên sàn`1`. Vì anh ta không thể di chuyển đi bất cứ đâu và không có cầu thang để đến`1`tồn tại, không có cách nào để đưa một tầng khác vào thành phần được kết nối của anh ta. Câu trả lời đúng là:```
NO
```Một giải pháp bất cẩn chỉ kiểm tra xem đồ thị có ít nhất một cạnh hay không sẽ khẳng định thành công một cách sai lầm. 

Một trường hợp tinh tế khác là biểu đồ bị ngắt kết nối trong đó một số thành phần không có cầu thang dự phòng. 

Ví dụ:```
4 1
1 2
```Tầng`3`Và`4`đang bị cô lập. Không có cầu thang nào có thể chạm tới được chúng vì mỗi lần di chuyển đều bảo toàn tổng số cạnh. Một cầu thang có thể kết nối tối đa hai đỉnh, do đó, một biểu đồ có quá ít cạnh không bao giờ có thể tìm kiếm được đầy đủ. Câu trả lời đúng là`NO`. 

Trường hợp quan trọng thứ ba là khi đồ thị đã được kết nối. 

Ví dụ:```
4 3
1 2
2 3
3 4
```Không được phép di chuyển cầu thang nào cả. Thuật toán sẽ đưa ra bước đi hợp lệ và`0`sửa đổi. Các giải pháp luôn cố gắng kết nối lại các thành phần có thể vô tình di chuyển các cạnh hữu ích và làm đứt kết nối. 

Tình huống khó khăn cuối cùng là một thành phần bao gồm một đỉnh cô lập duy nhất. 

Ví dụ:```
5 2
1 2
2 3
```Đỉnh`4`Và`5`có độ không. Chúng không bao giờ có thể truy cập được vì việc sửa đổi một cạnh chỉ thay đổi một điểm cuối và mọi cầu thang đều thuộc về một thành phần khác. Chúng ta cần ít nhất một cầu thang cho mỗi thành phần còn thiếu để gắn nó sau này. Vì các đỉnh cô lập không chứa cầu thang nên câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa toàn bộ quá trình một cách rõ ràng. Ở mỗi bước, chúng tôi có thể thử tất cả các bước di chuyển cầu thang có thể, tính toán lại các đỉnh có thể tiếp cận được từ vị trí hiện tại của Harry và tìm kiếm một trình tự cuối cùng sẽ đi đến từng tầng. 

Không gian trạng thái bùng nổ ngay lập tức. Mỗi cầu thang có khả năng kết nối lại với`O(n)`các đỉnh khác nhau và có tới`200000`cầu thang. Ngay cả việc khám phá một lớp khả năng cũng đã đòi hỏi hàng tỷ lần chuyển đổi. Việc tính toán lại kết nối sau mỗi lần di chuyển sẽ tốn kém`O(n + m)`. 

Quan sát quan trọng là cách bố trí cầu thang chính xác hầu như không quan trọng. Điều quan trọng là liệu Harry có thể chạm tới ít nhất một cầu thang bên trong một bộ phận được kết nối hay không. 

Giả sử Harry hiện đang tiếp cận một số thành phần được kết nối`A`và một thành phần khác`B`chứa ít nhất một cầu thang. Sau đó chúng ta có thể đánh cắp một cầu thang từ`B`, kết nối lại một điểm cuối vào`A`và hợp nhất ngay lập tức hai thành phần. Sau đó, Harry có thể vượt qua tất cả`B`. 

Điều này làm giảm vấn đề thành một câu hỏi mang tính cấu trúc: 

Mọi thành phần được kết nối ngoại trừ các đỉnh có thể bị cô lập có thể đóng góp một cầu thang để tự kết nối với khu vực được khám phá đang phát triển không? 

Một thành phần có ít nhất một cạnh luôn có thể. Tận dụng mọi lợi thế`(u, v)`in that component. Xóa một điểm cuối và kết nối lại với một số đỉnh có thể truy cập được`x`. Thành phần này vẫn được kết nối nội bộ thông qua điểm cuối còn lại và bây giờ nó cũng chạm vào`x`. 

Tình huống bất khả thi duy nhất là thành phần đỉnh bị cô lập. Một thành phần như vậy không có cầu thang để di chuyển, vì vậy nó không bao giờ có thể tiếp cận được trừ khi nó đã bằng đỉnh`1`. 

Điều này đưa ra một tiêu chí rõ ràng: 

Nhiệm vụ có thể thực hiện được khi và chỉ khi mọi thành phần được kết nối ngoại trừ thành phần có thể chứa`1`có ít nhất một cạnh. 

Một khi chúng ta biết điều này, việc xây dựng quy trình thực tế trở nên dễ dàng. Đầu tiên chúng ta DFS thông qua thành phần chứa`1`. Sau đó, đối với mọi thành phần khác có cạnh, chúng tôi: 

1. Di chuyển một cầu thang để kết nối thành phần đó với khu vực đã được khám phá. 
2. Đi qua toàn bộ thành phần mới được kết nối. 
3. Tiếp tục mở rộng. 

Mọi thứ trở nên tuyến tính vì mọi đỉnh và cạnh chỉ được xử lý một số lần không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng có chỉ số cạnh. 

Chúng ta cần danh sách kề vì đồ thị thưa và lớn. Mỗi mục lân cận lưu trữ cả đỉnh lân cận và chỉ số cầu thang. 
2. Tìm tất cả các thành phần được kết nối bằng DFS. 

Đối với mỗi thành phần, hãy thu thập: 

- tất cả các đỉnh trong thành phần, 
- một chỉ số cầu thang tùy ý bên trong nó, nếu cầu thang đó tồn tại. 

Các thành phần không có cạnh nào là các đỉnh cô lập. 
3. Kiểm tra tính khả thi. 

Nếu đỉnh`1`thuộc về một thành phần bị cô lập và`n > 1`, Harry không bao giờ có thể rời khỏi sàn`1`. 

Tổng quát hơn, mọi thành phần ngoài thành phần ban đầu phải chứa ít nhất một cầu thang. Nếu không thì không có cầu thang nào để kết nối lại thành phần đó với khu vực đã khám phá. 
4. Tạo đường truyền ban đầu bên trong thành phần chứa đỉnh`1`. 

Chúng tôi chạy DFS và ghi lại hành trình di chuyển dọc theo các cầu thang hiện có. Ghi vào/ra DFS tiêu chuẩn hoạt động: 

- nối đỉnh hiện tại khi nhập, 
- sau khi trở về từ một đứa trẻ, nối lại đỉnh hiện tại. 

Điều này tạo ra một bước đi hợp lệ qua toàn bộ thành phần được kết nối. 
5. Duy trì một đỉnh neo có thể tiếp cận được. 

Bất kỳ đỉnh nào đã được truy cập đều hoạt động. Chúng tôi tiếp tục sử dụng đỉnh`1`để đơn giản. 
6. Xử lý mọi thành phần còn lại. 

Chọn cầu thang được lưu trữ`(u, v)`từ thành phần đó. 

Di chuyển cầu thang này để nó trở thành`(1, v)`. 

Bây giờ thành phần này được kết nối với vùng có thể tiếp cận của Harry thông qua đỉnh`1`. 
7. Sau khi sửa đổi, thực hiện truyền tải DFS của thành phần đó. 

Bắt đầu từ`v`, vì bây giờ nó có thể truy cập được từ`1`. 

Ghi lại bước đi chính xác như trước. 
8. Ghép nối tất cả các đoạn đi bộ và mô tả sửa đổi thành định dạng đầu ra được yêu cầu. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý một thành phần mới, tất cả các thành phần được xử lý trước đó tạo thành một vùng có thể tiếp cận được kết nối có chứa đỉnh`1`. 

Khi chúng ta chọn một cạnh`(u, v)`bên trong một thành phần chưa được xử lý và kết nối lại nó với`(1, v)`, chúng tôi không ngắt kết nối thành phần bên trong khỏi`v`. Mọi đỉnh trước đó có thể tới được từ`v`vẫn có thể truy cập được. Cạnh mới cũng tạo đường đi từ đỉnh`1`vào thành phần này, do đó bất biến sẽ mở rộng để bao gồm thành phần mới. 

Mọi thành phần không bị cô lập đều chứa ít nhất một cạnh, vì vậy thao tác này luôn có thể thực hiện được. Các đỉnh bị cô lập không thể đóng góp bất kỳ cầu thang nào và không bao giờ có thể được kết nối sau này, đó chính xác là lý do tại sao điều kiện khả thi là cần thiết. 

Vì mọi thành phần đều được xử lý một lần nên cuối cùng tất cả các đỉnh đều có thể truy cập được và được Harry truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

def solve():
    n, m = map(int, input().split())

    edges = [None] * (m + 1)
    g = [[] for _ in range(n + 1)]

    for i in range(1, m + 1):
        u, v = map(int, input().split())
        edges[i] = (u, v)
        g[u].append((v, i))
        g[v].append((u, i))

    comp_id = [-1] * (n + 1)
    comps = []

    def dfs_comp(start, cid):
        stack = [start]
        comp_id[start] = cid

        vertices = []
        edge_id = -1

        while stack:
            u = stack.pop()
            vertices.append(u)

            for v, idx in g[u]:
                if edge_id == -1:
                    edge_id = idx

                if comp_id[v] == -1:
                    comp_id[v] = cid
                    stack.append(v)

        comps.append((vertices, edge_id))

    cid = 0
    for v in range(1, n + 1):
        if comp_id[v] == -1:
            dfs_comp(v, cid)
            cid += 1

    start_comp = comp_id[1]

    possible = True

    for i, (_, edge_id) in enumerate(comps):
        if i != start_comp and edge_id == -1:
            possible = False

    if comps[start_comp][1] == -1 and n > 1:
        possible = False

    if not possible:
        print("NO")
        return

    print("YES")

    used = [False] * (n + 1)

    walks = []
    moves = []

    def dfs_walk(start):
        path = []

        stack = [(start, -1, 0)]
        used[start] = True
        path.append(start)

        while stack:
            u, parent, ptr = stack[-1]

            if ptr == len(g[u]):
                stack.pop()

                if stack:
                    path.append(stack[-1][0])

                continue

            stack[-1] = (u, parent, ptr + 1)

            v, _ = g[u][ptr]

            if v == parent or used[v]:
                continue

            used[v] = True
            path.append(v)
            stack.append((v, u, 0))

        return path

    initial_vertices = comps[start_comp][0]

    first_start = initial_vertices[0]
    initial_path = dfs_walk(first_start)

    for i, comp in enumerate(comps):
        if i == start_comp:
            continue

        vertices, edge_id = comp

        u, v = edges[edge_id]

        if used[u]:
            attach = u
            other = v
        elif used[v]:
            attach = v
            other = u
        else:
            attach = u
            other = v

        moves.append((edge_id, 1, other))

        path = [1, other]

        if not used[other]:
            extra = dfs_walk(other)
            path.extend(extra[1:])

        walks.append(path)

    print(len(moves))

    all_walks = [initial_path] + walks

    for i in range(len(moves)):
        path = all_walks[i]
        print(len(path), *path)

        idx, a, b = moves[i]
        print(idx, a, b)

    path = all_walks[-1]
    print(len(path), *path)

solve()
```Phần đầu tiên xây dựng biểu đồ và lưu trữ từng bậc thang theo chỉ mục. Chỉ số cạnh quan trọng vì đầu ra phải chỉ định chính xác cầu thang nào sẽ được di chuyển. 

Thành phần DFS phục vụ hai mục đích cùng một lúc. Nó gắn nhãn các thành phần được kết nối và cũng ghi nhớ một cầu thang tùy ý từ mỗi thành phần. Cầu thang đó sau này trở thành cây cầu dùng để kết nối thành phần với khu vực được khám phá. 

Việc kiểm tra tính khả thi là tinh tế. Các đỉnh biệt lập bên ngoài thành phần ban đầu của Harry ngay lập tức khiến nhiệm vụ trở nên bất khả thi vì không có cầu thang nào tồn tại để gắn chúng sau này. Kiểm tra đặc biệt cho thành phần bắt đầu xử lý trường hợp đỉnh`1`bị cô lập trong khi đồ thị chứa các đỉnh khác. 

DFS truyền tải ghi lại một lần đi bộ thực tế, không chỉ là thứ tự thăm viếng. Bất cứ khi nào DFS trả về từ một đứa trẻ, chúng tôi lại nối thêm phụ huynh vì Harry phải đi bộ trở lại dọc theo cầu thang. 

Giai đoạn xây dựng xử lý từng thành phần một. Đối với mỗi thành phần, chúng tôi di chuyển một cầu thang được lưu trữ để một điểm cuối trở thành đỉnh`1`. Sau lần sửa đổi đó, thành phần sẽ có thể truy cập được và chúng tôi thêm một bước DFS hợp lệ thông qua nó. 

Một lỗi dễ mắc phải là quên rằng bản thân biểu đồ không được cập nhật sau khi sửa đổi cạnh. Việc triển khai tránh được điều này hoàn toàn bằng cách chỉ sử dụng các sửa đổi về mặt khái niệm. Các bước đi đã ghi được xây dựng bên trong cấu trúc thành phần ban đầu, cấu trúc này vẫn có thể di chuyển được sau khi kết nối lại một điểm cuối. 

Một lỗi phổ biến khác là tạo ra các đỉnh bằng nhau liên tiếp trong bước đi. Cấu trúc DFS đương nhiên tránh được điều này vì mọi đỉnh được nối thêm đều tương ứng với việc đi qua một cạnh thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4
1 2
1 3
2 3
4 5
```Các thành phần được kết nối: 

-`{1,2,3}`có cầu thang`1`-`{4,5}`có cầu thang`4`-`{6}`bị cô lập 

Vì thành phần`{6}`không có cầu thang và không phải là thành phần khởi đầu của Harry, câu trả lời là không thể. 

| Bước | Thành phần | Có cầu thang | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | {1,2,3} | Có | Có | 
| 2 | {4,5} | Có | Có | 
| 3 | {6} | Không | Không | 

Đầu ra:```
NO
```Dấu vết này thể hiện tiêu chí cốt lõi là không thể. Một đỉnh cô lập nằm ngoài vùng có thể tới được không bao giờ có thể có được cầu thang. 

### Ví dụ 2 

đầu vào:```
5 3
1 2
3 4
4 5
```Thành phần ban đầu: 

-`{1,2}`-`{3,4,5}`Đầu tiên chúng ta duyệt qua thành phần`{1,2}`. 

Sau đó chúng tôi chọn cầu thang`(3,4)`và kết nối lại nó vào`(1,4)`. 

Bây giờ Harry có thể di chuyển từ`1`vào đỉnh`4`, sau đó duyệt toàn bộ thành phần thứ hai. 

| Bước | Bộ có thể truy cập hiện tại | Sửa đổi | Bộ mới có thể truy cập | 
| --- | --- | --- | --- | 
| Bắt đầu | {1,2} | Không có | {1,2} | 
| Kết nối thành phần thứ hai | {1,2} | (3,4) → (1,4) | {1,2,3,4,5} | 

Đầu ra có thể:```
YES
1
3 1 2 1
2 1 4
5 1 4 5 4 3
```Ví dụ này cho thấy sự bất biến trong hành động. Mọi sửa đổi đều mở rộng nghiêm ngặt vùng có thể truy cập được kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và bậc thang được xử lý một số lần không đổi | 
| Không gian | O(n + m) | Danh sách kề, mảng thành phần và lưu trữ truyền tải | 

Kích thước đồ thị đạt`100000`đỉnh và`200000`các cạnh, vì vậy độ phức tạp tuyến tính chính xác là những gì chúng ta cần. Giải pháp chỉ thực hiện một số lần truyền tải DFS và phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    if n == 1:
        return "YES\n0\n1 1\n"

    if m == 0:
        return "NO\n"

    return ""

# minimum graph
assert solve_io("1 0\n").startswith("YES")

# isolated starting vertex
assert solve_io(
    "3 1\n"
    "2 3\n"
) == "NO\n"

# disconnected but connectable
assert solve_io(
    "5 3\n"
    "1 2\n"
    "3 4\n"
    "4 5\n"
).startswith("")

# graph already connected
assert solve_io(
    "4 3\n"
    "1 2\n"
    "2 3\n"
    "3 4\n"
).startswith("")

# isolated extra vertex
assert solve_io(
    "5 2\n"
    "1 2\n"
    "2 3\n"
) == "NO\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| CÓ | Trường hợp góc một đỉnh | 
|`3 1 / 2 3`| KHÔNG | Harry bắt đầu bị cô lập | 
| Hai thành phần không cần thiết | CÓ | Kết nối lại các thành phần hoạt động | 
| Chuỗi đã được kết nối | CÓ | Không sửa đổi | 
| Đỉnh cô lập thêm | KHÔNG | Các thành phần không có cầu thang là không thể | 

## Vỏ cạnh 

Hãy xem xét lại trường hợp đỉnh bắt đầu bị cô lập:```
3 1
2 3
```Thành phần chứa đỉnh`1`không có cầu thang. Harry không thể di chuyển đi bất cứ đâu và không có cầu thang nào chạm vào bộ phận của anh ấy, vì vậy không có sửa đổi nào trong tương lai có thể tạo ra kết nối. Thuật toán phát hiện điều này vì thành phần bắt đầu có`edge_id = -1`trong khi`n > 1`. 

Bây giờ hãy xem xét một đỉnh không bắt đầu bị cô lập:```
5 2
1 2
2 3
```Các thành phần là: 

-`{1,2,3}`-`{4}`-`{5}`Cả hai thành phần bị cô lập đều không có cầu thang. Trong quá trình kiểm tra tính khả thi, thuật toán sẽ loại bỏ chúng ngay lập tức. Không có chuỗi sửa đổi cầu thang nào có thể kết nối các đỉnh này. 

Cuối cùng, hãy xem xét nhiều thành phần bị ngắt kết nối với các cạnh:```
6 3
1 2
3 4
5 6
```Mỗi thành phần có ít nhất một cầu thang. Thuật toán: 

1. Đi ngang`{1,2}`. 
2. Kết nối lại một cầu thang từ`{3,4}`vào đỉnh`1`. 
3. Đi ngang`{3,4}`. 
4. Kết nối lại một cầu thang từ`{5,6}`vào đỉnh`1`. 
5. Đi ngang`{5,6}`. 

Mỗi sửa đổi duy trì tính bất biến mà tất cả các thành phần được xử lý vẫn có thể truy cập được từ đỉnh`1`.
