---
title: "CF 97E - Lãnh Đạo"
description: "Chúng ta có một đồ thị vô hướng trong đó các đỉnh biểu thị con người và các cạnh biểu thị các mối quan hệ. Với mỗi truy vấn (u, v), chúng ta phải quyết định xem có tồn tại một đường đi đơn từ u đến v có độ dài lẻ hay không. Từ khóa ở đây là \"đơn giản\". Chúng tôi không được phép xem lại các đỉnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 97
codeforces_index: "E"
codeforces_contest_name: "Yandex.Algorithm 2011: Finals"
rating: 2200
weight: 97
solve_time_s: 123
verified: true
draft: false
---

[CF 97E - Lãnh đạo](https://codeforces.com/problemset/problem/97/E) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự, dsu, đồ thị, cây 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó các đỉnh biểu thị con người và các cạnh biểu thị các mối quan hệ. Đối với mỗi truy vấn`(u, v)`, chúng ta phải quyết định xem có tồn tại một đường đi đơn giản từ`u`ĐẾN`v`có độ dài lẻ. 

Từ khóa ở đây là "đơn giản". Chúng tôi không được phép xem lại các đỉnh. Biểu đồ không nhất thiết phải được kết nối và nó có thể chứa các chu trình. 

BFS trực tiếp từ mọi đỉnh truy vấn sẽ không hoạt động. Cả số đỉnh và số cạnh đều có thể đạt tới`10^5`, và cũng có tới`10^5`truy vấn. Bất kỳ giải pháp nào dành thời gian tuyến tính cho mỗi truy vấn sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất, vượt xa giới hạn. 

Khó khăn thực sự là một cặp đỉnh có thể có cả đường đơn chẵn và lẻ giữa chúng. Câu trả lời không được xác định chỉ bằng tính chẵn lẻ của đường đi ngắn nhất. 

Hãy xem xét biểu đồ này:```
1 - 2
|   |
3 - 4
```Giữa`1`Và`2`, có một con đường kỳ lạ`1-2`, nhưng cũng là một con đường chẵn`1-3-4-2`. Vấn đề chỉ hỏi liệu có tồn tại ít nhất một đường dẫn đơn giản kỳ lạ hay không. 

Một sai lầm phổ biến là nghĩ rằng việc tô màu hai bên sẽ giải quyết được mọi thứ. Trong đồ thị hai phần, tất cả các đường đi giữa hai đỉnh đều có cùng tính chẵn lẻ, do đó việc tô màu có tác dụng ở đó. Nhưng một khi đồ thị chứa một chu trình lẻ thì cả hai số chẵn lẻ đều có thể xảy ra. 

Ví dụ:```
1 - 2
 \ /
  3
```Tam giác chứa một chu trình lẻ. Giữa`1`Và`2`, chúng ta có:```
1-2        length 1
1-3-2      length 2
```Cả hai tính chẵn lẻ đều tồn tại. Bất kỳ phương pháp nào chỉ lưu trữ một chẵn lẻ cho mỗi cặp sẽ thất bại. 

Một trường hợp tinh tế khác là truy vấn cùng một đỉnh. 

đầu vào:```
3 3
1 2
2 3
3 1
2
1 1
2 2
```Đầu ra:```
Yes
Yes
```Tam giác chứa chu trình lẻ nên đỉnh`1`có thể quay trở lại chính nó bằng cách sử dụng một chu kỳ độ dài lẻ đơn giản`3`. Việc thực hiện bất cẩn có thể trả lời sai`"No"`bởi vì đường dẫn trống có chiều dài`0`. 

Các thành phần bị ngắt kết nối là một nguồn sai lầm khác. 

đầu vào:```
4 1
1 2
2
1 3
3 4
```Đầu ra:```
No
No
```Không có con đường nào tồn tại cả, vì vậy câu trả lời phải là`"No"`bất kể tính chẵn lẻ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn`(u, v)`, chạy BFS hoặc DFS theo dõi tính chẵn lẻ. Chúng ta có thể nhân đôi mỗi đỉnh thành hai trạng thái:```
(vertex, parity)
```Khi đi qua một cạnh, tính chẵn lẻ sẽ bị đảo ngược. 

Nếu chúng ta có thể đạt được`(v, 1)`, thì tồn tại một đường đi lẻ. 

Cách tiếp cận này đúng cho việc đi bộ, nhưng không nhất thiết cho những con đường đơn giản. Việc ngăn chặn các đỉnh lặp lại làm cho việc tìm kiếm khó khăn hơn nhiều. Trong các biểu đồ chung, việc theo dõi tính chẵn lẻ của đường dẫn đơn giản trực tiếp cho mỗi truy vấn trở nên tốn kém. 

Ngay cả khi chúng tôi bỏ qua vấn đề đơn giản và chạy BFS cho mỗi truy vấn, độ phức tạp sẽ trở thành:```
O(q(n + m))
```Với tất cả các giá trị gần`10^5`, đây là đại khái`10^10`hoạt động. 

Quan sát chính là cấu trúc. 

Bên trong một thành phần lưỡng cực được kết nối, mọi đường đi giữa hai đỉnh đều có tính chẵn lẻ cố định. Nếu hai đỉnh có cùng lớp màu thì tất cả các đường đi giữa chúng đều là số chẵn. Nếu chúng ở các lớp đối lập nhau thì mọi đường đi đều là số lẻ. 

Bên trong một thành phần không lưỡng đảng được kết nối, tình hình thay đổi hoàn toàn. Một chu trình lẻ tồn tại và chu trình lẻ đó cho phép chúng ta đảo ngược tính chẵn lẻ của đường đi. Trên thực tế: 

Nếu một thành phần liên thông không phải là lưỡng cực thì mỗi cặp đỉnh có cả đường đơn chẵn và đường đơn lẻ giữa chúng. 

Thực tế duy nhất đó đã làm sụp đổ toàn bộ vấn đề. 

Vì vậy, giải pháp trở thành: 

1. Tìm các thành phần được kết nối. 
2. Xác định xem mỗi thành phần có phải là lưỡng cực hay không. 
3. Nếu thành phần là lưỡng cực, hãy sử dụng màu đỉnh để xác định tính chẵn lẻ. 
4. Nếu thành phần không phải là lưỡng cực thì mọi cặp bên trong nó đều trả lời`"Yes"`. 

Điều này làm giảm tất cả các truy vấn thành kiểm tra liên tục sau một lần xử lý trước DFS/BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(q(n + m)) | O(n + m) | Quá chậm | 
| Tiền xử lý đồ thị tối ưu | O(n + m + q) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị. 

Biểu đồ thưa thớt nên danh sách kề là cách biểu diễn tự nhiên. 
2. Duyệt qua mọi thành phần được kết nối bằng DFS hoặc BFS. 

Trong quá trình truyền tải, gán màu nhị phân cho mỗi đỉnh. 
3. Khi đi qua các cạnh`(u, v)`, kiểm tra tính nhất quán của màu sắc. 

Nếu như`u`Và`v`đã có cùng màu thì thành phần không phải là lưỡng cực. 
4. Lưu trữ cho mọi đỉnh: 

- id thành phần được kết nối của nó 
- màu sắc lưỡng cực của nó 
- thành phần của nó có phải là lưỡng cực hay không 
5. Đối với mỗi truy vấn`(u, v)`: 

1. Nếu`u`Và`v`thuộc về các thành phần được kết nối khác nhau, in`"No"`. 

Không có con đường nào tồn tại cả. 
2. Mặt khác, nếu thành phần không có hai bên, hãy in`"Yes"`. 

Một thành phần được kết nối không lưỡng cực chứa một chu trình lẻ, cho phép xây dựng một đường dẫn đơn giản lẻ giữa bất kỳ cặp nào. 
3. Nếu không thì thành phần này là lưỡng cực. 

Trong đồ thị hai phần, tất cả các đường đi giữa hai đỉnh đều có cùng tính chẵn lẻ. 

In`"Yes"`nếu màu sắc khác nhau, nếu không`"No"`. 

### Tại sao nó hoạt động 

Một đồ thị liên thông là đồ thị lưỡng cực khi và chỉ khi nó không chứa chu trình lẻ. 

Trong biểu đồ hai bên, mỗi cạnh kết nối các lớp màu đối diện. Bất kỳ đường đi nào cũng xen kẽ các màu ở mỗi bước, do đó tính chẵn lẻ của đường đi giữa hai đỉnh được xác định duy nhất bởi màu sắc của chúng. 

Nếu đồ thị không lưỡng cực thì tồn tại một chu trình lẻ. Bắt đầu từ bất kỳ con đường nào giữa`u`Và`v`, chúng ta có thể định tuyến qua chu trình lẻ để lật tính chẵn lẻ trong khi vẫn giữ đường đi đơn giản. Vì thành phần được kết nối nên mọi cặp đỉnh đều có thể đạt đến chu trình lẻ và sử dụng nó để thay đổi tính chẵn lẻ. 

Điều đó có nghĩa là: 

- thành phần lưỡng cực → cố định tính chẵn lẻ 
- thành phần không lưỡng cực → có sẵn cả hai chẵn lẻ 

Thuật toán kiểm tra chính xác các điều kiện này nên mọi câu trả lời đều đúng. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    comp = [-1] * (n + 1)
    color = [-1] * (n + 1)
    bipartite = []

    cid = 0

    for start in range(1, n + 1):
        if comp[start] != -1:
            continue

        queue = deque([start])
        comp[start] = cid
        color[start] = 0

        ok = True

        while queue:
            u = queue.popleft()

            for v in graph[u]:
                if comp[v] == -1:
                    comp[v] = cid
                    color[v] = color[u] ^ 1
                    queue.append(v)
                else:
                    if color[v] == color[u]:
                        ok = False

        bipartite.append(ok)
        cid += 1

    q = int(input())

    out = []

    for _ in range(q):
        u, v = map(int, input().split())

        if comp[u] != comp[v]:
            out.append("No")
        elif not bipartite[comp[u]]:
            out.append("Yes")
        else:
            out.append("Yes" if color[u] != color[v] else "No")

    print("\n".join(out))

solve()
```Giai đoạn tiền xử lý thực hiện BFS trên mọi thành phần được kết nối. Trong quá trình truyền tải, các đỉnh nhận được màu sắc xen kẽ. 

các`comp`mảng xác định thành phần được kết nối mà mỗi đỉnh thuộc về. Điều này ngay lập tức trả lời các truy vấn kết nối. 

các`color`mảng lưu trữ phân vùng lưỡng cực. Các đỉnh liền kề phải luôn nhận được màu đối lập. Nếu chúng ta gặp một cạnh nối các màu bằng nhau thì thành phần đó chứa một chu kỳ lẻ và được đánh dấu là không lưỡng cực. 

các`bipartite`list lưu trữ một boolean cho mỗi thành phần. Điều này cho phép mỗi truy vấn chạy trong thời gian không đổi. 

Một chi tiết triển khai tinh tế là chúng tôi tiếp tục duyệt ngay cả sau khi phát hiện xung đột màu. Việc dừng sớm sẽ khiến các phần của thành phần không được xem xét và làm hỏng các truy vấn sau này. 

Một chi tiết quan trọng khác là xử lý các truy vấn như`(u, u)`. Trong thành phần lưỡng đảng, câu trả lời là`"No"`bởi vì mọi chu kỳ đều chẵn. Trong một thành phần không có lưỡng đảng, câu trả lời sẽ trở thành`"Yes"`vì tồn tại một chu trình lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 7
1 3
1 4
2 3
2 4
5 6
6 7
7 5
8
1 2
1 3
1 4
2 4
1 5
5 6
5 7
6 7
```Đồ thị có hai thành phần được kết nối. 

| Đỉnh | Thành phần | Màu sắc | Thành phần lưỡng đảng | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | Có | 
| 2 | 0 | 0 | Có | 
| 3 | 0 | 1 | Có | 
| 4 | 0 | 1 | Có | 
| 5 | 1 | 0 | Không | 
| 6 | 1 | 1 | Không | 
| 7 | 1 | 1 | Không | 

Bây giờ xử lý các truy vấn. 

| Truy vấn | Thành phần tương tự | lưỡng đảng | Quan hệ màu sắc | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 2 | Có | Có | Tương tự | Không | 
| 1 3 | Có | Có | Khác nhau | Có | 
| 1 4 | Có | Có | Khác nhau | Có | 
| 2 4 | Có | Có | Khác nhau | Có | 
| 1 5 | Không | - | - | Không | 
| 5 6 | Có | Không | - | Có | 
| 5 7 | Có | Không | - | Có | 
| 6 7 | Có | Không | - | Có | 

Thành phần đầu tiên là lưỡng cực nên tính chẵn lẻ được xác định hoàn toàn bằng màu sắc. Thành phần thứ hai chứa một hình tam giác, vì vậy mỗi cặp đều có một đường đi đơn lẻ. 

### Ví dụ 2 

đầu vào:```
3 2
1 2
2 3
4
1 1
1 2
1 3
2 3
```| Đỉnh | Thành phần | Màu sắc | lưỡng đảng | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | Có | 
| 2 | 0 | 1 | Có | 
| 3 | 0 | 0 | Có | 

Truy vấn: 

| Truy vấn | Quan hệ màu sắc | Trả lời | 
| --- | --- | --- | 
| 1 1 | Tương tự | Không | 
| 1 2 | Khác nhau | Có | 
| 1 3 | Tương tự | Không | 
| 2 3 | Khác nhau | Có | 

Ví dụ này thể hiện tính chất chẵn lẻ cố định của đồ thị hai bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) | BFS xử lý mọi đỉnh và cạnh một lần, sau đó mỗi truy vấn là O(1) | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng phụ trợ | 

Với`10^5`đỉnh, cạnh và truy vấn, tiền xử lý tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng an toàn đối với các ràng buộc nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    comp = [-1] * (n + 1)
    color = [-1] * (n + 1)
    bipartite = []

    cid = 0

    for start in range(1, n + 1):
        if comp[start] != -1:
            continue

        q = deque([start])
        comp[start] = cid
        color[start] = 0

        ok = True

        while q:
            u = q.popleft()

            for v in graph[u]:
                if comp[v] == -1:
                    comp[v] = cid
                    color[v] = color[u] ^ 1
                    q.append(v)
                else:
                    if color[v] == color[u]:
                        ok = False

        bipartite.append(ok)
        cid += 1

    queries = int(input())

    out = []

    for _ in range(queries):
        u, v = map(int, input().split())

        if comp[u] != comp[v]:
            out.append("No")
        elif not bipartite[comp[u]]:
            out.append("Yes")
        else:
            out.append("Yes" if color[u] != color[v] else "No")

    return "\n".join(out)

# provided sample
assert run(
"""7 7
1 3
1 4
2 3
2 4
5 6
6 7
7 5
8
1 2
1 3
1 4
2 4
1 5
5 6
5 7
6 7
"""
) == "\n".join([
    "No",
    "Yes",
    "Yes",
    "Yes",
    "No",
    "Yes",
    "Yes",
    "Yes"
]), "sample 1"

# single isolated vertex
assert run(
"""1 0
3
1 1
1 1
1 1
"""
) == "\n".join([
    "No",
    "No",
    "No"
]), "isolated vertex"

# simple chain
assert run(
"""4 3
1 2
2 3
3 4
4
1 2
1 3
1 4
2 4
"""
) == "\n".join([
    "Yes",
    "No",
    "Yes",
    "No"
]), "bipartite parity"

# triangle graph
assert run(
"""3 3
1 2
2 3
3 1
4
1 1
1 2
1 3
2 2
"""
) == "\n".join([
    "Yes",
    "Yes",
    "Yes",
    "Yes"
]), "non-bipartite component"

# disconnected graph
assert run(
"""5 2
1 2
4 5
4
1 3
2 5
4 5
3 3
"""
) == "\n".join([
    "No",
    "No",
    "Yes",
    "No"
]), "disconnected components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh cô lập đơn | Tất cả`No`| Không có con đường lẻ tự không có chu trình | 
| Dây chuyền đơn giản | Chẵn lẻ xen kẽ | Logic chẵn lẻ lưỡng cực | 
| Đồ thị tam giác | Tất cả`Yes`| Chu kỳ lẻ cho phép tất cả các chẵn lẻ | 
| Đồ thị bị ngắt kết nối | Câu trả lời hỗn hợp | Xử lý kết nối | 

## Vỏ cạnh 

Hãy xem xét một biểu đồ có thành phần không lưỡng cực và tự truy vấn. 

đầu vào:```
3 3
1 2
2 3
3 1
1
1 1
```Trong BFS, các đỉnh nhận được màu:```
1 -> 0
2 -> 1
3 -> 1
```Bờ rìa`(2, 3)`kết nối các màu bằng nhau, do đó thành phần được đánh dấu là không lưỡng cực. 

Đối với truy vấn`(1, 1)`, cả hai đỉnh đều thuộc cùng một thành phần không lưỡng cực, do đó thuật toán in`"Yes"`. 

Điều đó đúng vì chu trình:```
1-2-3-1
```có chiều dài lẻ`3`. 

Bây giờ hãy xem xét các đỉnh bị ngắt kết nối. 

đầu vào:```
4 1
1 2
2
1 3
3 4
```Các thành phần trở thành:```
{1,2}
{3}
{4}
```Đối với cả hai truy vấn, id thành phần khác nhau nên thuật toán sẽ trả lời ngay lập tức`"No"`. 

Điều này tránh việc suy luận không chính xác về tính chẵn lẻ khi không có đường dẫn nào tồn tại. 

Cuối cùng, hãy xem xét một biểu đồ hai bên trong đó tồn tại nhiều đường dẫn. 

đầu vào:```
4 4
1 3
3 2
1 4
4 2
2
1 2
1 3
```Đồ thị là lưỡng cực với các màu sắc:```
1,2 -> 0
3,4 -> 1
```Dù có hai con đường khác nhau`1`ĐẾN`2`, cả hai đều có độ dài chẵn:```
1-3-2
1-4-2
```Thuật toán trả lời`"No"`vì màu sắc giống nhau. 

Vì`(1,3)`, màu sắc khác nhau nên câu trả lời là`"Yes"`. 

Điều này xác nhận tính bất biến rằng tất cả các đường đi trong đồ thị hai bên đều có cùng tính chẵn lẻ.
