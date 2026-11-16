---
title: "CF 19E - Tiên"
description: "Chúng ta được cho một đồ thị vô hướng. Mỗi cạnh đại diện cho một đoạn được vẽ giữa hai điểm. Chúng ta có thể xóa chính xác một cạnh và sau khi loại bỏ nó, chúng ta muốn đồ thị còn lại trở thành đồ thị lưỡng cực."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "divide-and-conquer", "dsu"]
categories: ["algorithms"]
codeforces_contest: 19
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 19"
rating: 2900
weight: 19
solve_time_s: 110
verified: true
draft: false
---
[CF 19E - Tiên](https://codeforces.com/problemset/problem/19/E) 

**Xếp hạng:** 2900 
**Tags:** dfs và tương tự, chia để trị, dsu 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng. Mỗi cạnh đại diện cho một đoạn được vẽ giữa hai điểm. Chúng ta có thể xóa chính xác một cạnh và sau khi loại bỏ nó, chúng ta muốn đồ thị còn lại trở thành đồ thị lưỡng cực. 

Một đồ thị là lưỡng cực nếu chúng ta có thể tô màu mọi đỉnh bằng hai màu sao cho mỗi cạnh nối các đỉnh có màu khác nhau. Một cách khác để nói điều tương tự là đồ thị không được chứa chu trình lẻ. 

Nhiệm vụ là xuất ra mọi cạnh mà việc loại bỏ chúng làm cho toàn bộ đồ thị trở thành hai phần. 

Biểu đồ có thể chứa tới$10^4$đỉnh và$10^4$các cạnh. Kích thước đó ngay lập tức loại trừ mọi thứ bậc hai ở cả đỉnh và cạnh nếu hệ số không đổi lớn. Một giải pháp đơn giản giúp loại bỏ mọi cạnh và kiểm tra tính lưỡng cực từ đầu sẽ thực hiện DFS hoặc BFS đầy đủ$m$lần. Mỗi chi phí kiểm tra$O(n+m)$, do đó tổng độ phức tạp trở thành$O(m(n+m))$. Với cả hai giá trị gần$10^4$, nó đạt tới khoảng$2 \cdot 10^8$hoạt động quá rủi ro trong Python. 

Phần khó khăn là biểu đồ có thể bị ngắt kết nối. Việc triển khai bất cẩn khiến chỉ các DFS từ đỉnh 1 bỏ lỡ các chu kỳ lẻ trong các thành phần khác. 

Một trường hợp tinh vi khác xảy ra khi đồ thị đã có hai phần. Sau đó, loại bỏ bất kỳ cạnh nào vẫn để lại biểu đồ lưỡng cực, vì vậy mọi cạnh đều phải được in. 

Ví dụ:```
3 2
1 2
2 3
```Biểu đồ đã có hai bên. Đầu ra đúng:```
2
1 2
```Một cách tiếp cận sai mà chỉ tìm kiếm các cạnh “xấu” tham gia vào các chu kỳ lẻ có thể không in được gì không chính xác. 

Ngoài ra còn có một thái cực ngược lại, trong đó nhiều chu kỳ lẻ tồn tại độc lập. Nếu việc loại bỏ một cạnh không thể phá hủy được tất cả các chu trình lẻ thì câu trả lời là trống. 

Ví dụ:```
6 6
1 2
2 3
3 1
4 5
5 6
6 4
```Có hai hình tam giác bị ngắt kết nối. Việc loại bỏ một cạnh chỉ phá hủy một hình tam giác, trong khi chu trình lẻ còn lại vẫn còn. Đầu ra đúng:```
0
```Tiêu chí ngây thơ “cạnh thuộc một chu trình lẻ nào đó” không thành công ở đây, bởi vì mọi cạnh đều nằm trên một chu trình lẻ, nhưng không có cạnh nào cố định toàn bộ đồ thị. 

Trường hợp khó khăn nhất là các chu kỳ lẻ chồng chéo. Đôi khi một số chu trình lẻ có chung các cạnh và chỉ các cạnh được chia sẻ mới hoạt động. 

Ví dụ:```
5 6
1 2
2 3
3 1
3 4
4 5
5 3
```Hai tam giác có chung đỉnh 3 nhưng không có cạnh. Không có việc loại bỏ cạnh đơn nào sẽ phá hủy cả hai chu trình lẻ, vì vậy câu trả lời vẫn trống. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi cạnh, hãy tạm thời loại bỏ nó và kiểm tra xem đồ thị còn lại có phải là lưỡng cực hay không bằng cách sử dụng màu BFS hoặc DFS. 

Bản thân việc kiểm tra lưỡng đảng là tiêu chuẩn. Chúng tôi chỉ định các màu xen kẽ trong quá trình truyền tải. Nếu chúng ta gặp một cạnh nối các đỉnh có cùng màu thì đồ thị chứa một chu trình lẻ và không phải là đồ thị lưỡng cực. 

Điều này có tác dụng vì việc loại bỏ một cạnh không phụ thuộc vào quá trình tô màu. Mọi cạnh ứng cử viên đều có thể được kiểm tra trực tiếp. 

Vấn đề là tổng chi phí. Chúng tôi lặp lại một$O(n+m)$truyền đồ thị cho mọi cạnh, tạo ra$O(m(n+m))$thời gian tổng thể. Với$m = 10^4$, cái này trở nên quá lớn. 

Quan sát quan trọng là các chu kỳ kỳ lạ là trở ngại duy nhất cho sự hợp tác hai bên. Việc loại bỏ một cạnh chỉ có thể hữu ích nếu cạnh đó thuộc về mọi chu kỳ lẻ trong biểu đồ. 

Điều đó thay đổi hoàn toàn vấn đề. Thay vì tính toán lại tính lưỡng cực sau mỗi lần loại bỏ, chúng tôi nghiên cứu cấu trúc của các chu kỳ lẻ bên trong một cây DFS. 

Giả sử chúng ta DFS đồ thị và các đỉnh màu theo độ tương đương độ sâu. Mép cây luôn kết nối các màu đối lập nhau. Một cạnh không phải cây hoạt động khác: 

Nếu nó kết nối các đỉnh có tính chẵn lẻ khác nhau thì nó tạo thành một chu trình chẵn. 

Nếu nó nối các đỉnh có cùng chẵn lẻ thì nó tạo thành một chu trình lẻ. 

Các cạnh sau “xấu” này hoàn toàn đặc trưng cho các chu kỳ lẻ trong biểu đồ. 

Bây giờ hãy xem xét việc loại bỏ một cạnh có thể loại bỏ tất cả các chu trình lẻ như thế nào. Mỗi chu kỳ lẻ tương ứng với một cạnh xấu cùng với đường đi của cây giữa các điểm cuối của nó. Một cạnh có giá trị chính xác khi nó thuộc về tất cả các chu kỳ lẻ như vậy. 

Điều này trở thành vấn đề đếm trên cây DFS. 

Chúng tôi chỉ định mỗi cạnh sau xấu một phần đóng góp dọc theo đường dẫn giữa các điểm cuối của nó. Sau khi tích lũy các đóng góp từ dưới lên, mỗi cạnh của cây sẽ biết có bao nhiêu chu kỳ lẻ đi qua nó. 

Nếu đồ thị chứa chính xác$k$chu kỳ lẻ gây ra bởi các cạnh sau xấu, thì: 

Nếu$k = 0$, mọi cạnh đều hoạt động. 

Nếu như$k > 0$, một cạnh hoạt động nếu nó thuộc về tất cả$k$chu kỳ lẻ. 

Bản thân các cạnh không phải là cây xấu cũng hoạt động nếu chúng là cạnh duy nhất chịu trách nhiệm đóng góp một chu kỳ lẻ. 

Điều này làm giảm toàn bộ vấn đề xuống còn một DFS cộng với một lần tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m(n+m))$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng và lưu trữ chỉ số của mọi cạnh. 
2. Chạy DFS trên mọi thành phần được kết nối. 

Trong DFS, lưu trữ: 

- độ sâu của mỗi đỉnh 
- cạnh cha trong cây DFS 
- màu chẵn lẻ sử dụng độ sâu modulo 2 
3. Phân loại các cạnh trong DFS. 

Nếu một người hàng xóm đã đến thăm không phải là cha mẹ, chúng tôi đã tìm thấy một cạnh sau. 

Nếu hai điểm cuối có cùng tính chẵn lẻ thì cạnh sau này sẽ tạo ra một chu kỳ lẻ. 
4. Đếm tất cả các cạnh sau lẻ trên toàn cầu. 

Đặt giá trị này là`odd_cnt`. 

Nếu như`odd_cnt == 0`, đồ thị đã có hai phần nên mọi cạnh đều hợp lệ. 
5. Đối với mọi cạnh sau lẻ$(u,v)$, thêm đóng góp đường dẫn. 

Cho rằng`depth[u] > depth[v]`. 

Tăng`mark[u] += 1`Giảm`mark[v] -= 1`Sau này, khi chúng ta tổng hợp các giá trị đi lên qua cây DFS, mọi cạnh của cây trên đường dẫn từ`u`ĐẾN`v`nhận được một đóng góp 
6. DFS một lần nữa để tích lũy số tiền cây con. 

Với mỗi đỉnh, tính tổng đóng góp từ nút con vào nút cha. 

Cho phép`sub[x]`biểu thị giá trị tích lũy cuối cùng. 

Nếu một cạnh của cây từ cây cha tới cây con có`sub[child] == odd_cnt`, thì mọi chu trình lẻ đều đi qua cạnh này, vì vậy việc loại bỏ nó sẽ sửa được đồ thị. 
7. Xử lý riêng các cạnh lẻ không phải cây. 

Mỗi cạnh sau lẻ riêng lẻ thuộc về chính xác một bộ mô tả chu kỳ lẻ. Một cạnh như vậy hoạt động nếu`odd_cnt == 1`. 
8. Thu thập tất cả các chỉ số cạnh hợp lệ và in chúng theo thứ tự. 

### Tại sao nó hoạt động 

Mỗi chu kỳ lẻ trong DFS vô hướng có thể được biểu diễn bằng một cạnh sau lẻ cộng với đường dẫn cây nối các điểm cuối của nó. 

Sự lan truyền đóng góp đánh dấu chính xác các cạnh của cây thuộc mỗi chu kỳ lẻ. Sau khi tích lũy,`sub[x]`bằng số chu kỳ lẻ đi qua cạnh cây nối`x`tới cha mẹ của nó. 

Nếu một cạnh thuộc về tất cả các chu trình lẻ, việc loại bỏ nó sẽ đồng thời phá hủy mọi chu trình lẻ, làm cho đồ thị có hai phần. 

Nếu một cạnh bỏ lỡ dù chỉ một chu kỳ lẻ, chu trình đó vẫn tồn tại sau khi bị loại bỏ, do đó đồ thị vẫn không có lưỡng phân. 

Thuật toán kiểm tra chính xác điều kiện này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n, m = map(int, input().split())

g = [[] for _ in range(n + 1)]
edges = [None]

for i in range(1, m + 1):
    u, v = map(int, input().split())
    edges.append((u, v))
    g[u].append((v, i))
    g[v].append((u, i))

depth = [0] * (n + 1)
visited = [False] * (n + 1)
parent = [0] * (n + 1)
parent_edge = [0] * (n + 1)

mark = [0] * (n + 1)

odd_edges = []
odd_cnt = 0

def dfs(u, pe):
    global odd_cnt

    visited[u] = True

    for v, idx in g[u]:
        if idx == pe:
            continue

        if not visited[v]:
            depth[v] = depth[u] + 1
            parent[v] = u
            parent_edge[v] = idx
            dfs(v, idx)
        elif depth[v] < depth[u]:
            if (depth[u] - depth[v]) % 2 == 0:
                odd_cnt += 1
                odd_edges.append(idx)

                mark[u] += 1
                mark[v] -= 1

for i in range(1, n + 1):
    if not visited[i]:
        dfs(i, -1)

if odd_cnt == 0:
    print(m)
    print(*range(1, m + 1))
    sys.exit()

ans = []

def collect(u, pe):
    subtotal = mark[u]

    for v, idx in g[u]:
        if idx == pe:
            continue

        if parent[v] == u:
            child_sum = collect(v, idx)

            if child_sum == odd_cnt:
                ans.append(idx)

            subtotal += child_sum

    return subtotal

for i in range(1, n + 1):
    if parent[i] == 0:
        collect(i, -1)

if odd_cnt == 1:
    ans.extend(odd_edges)

ans.sort()

print(len(ans))
print(*ans)
```DFS đầu tiên xây dựng cây DFS và phát hiện các cạnh sau lẻ. Kiểm tra tính chẵn lẻ đến từ độ sâu DFS. Nếu độ sâu chênh lệch giữa hai điểm cuối là chẵn thì độ dài chu kỳ sẽ trở thành số lẻ. 

các`mark`mảng thực hiện tích lũy đường dẫn. Đối với một cạnh sau lẻ$(u,v)$, tăng ở điểm cuối sâu hơn và giảm ở điểm tổ tiên khiến mọi cạnh của cây trên đường dẫn đó nhận được một đóng góp trong quá trình tổng hợp cây con. 

DFS thứ hai tính tổng cây con từ dưới lên. Nếu một cạnh cây mang chính xác`odd_cnt`đóng góp thì tất cả các chu kỳ lẻ đều đi qua nó. 

điều kiện`depth[v] < depth[u]`tránh xử lý cùng một cạnh sau hai lần. Nếu không có nó, các cạnh vô hướng sẽ được tính từ cả hai hướng. 

Việc xử lý riêng biệt các cạnh lẻ không phải cây rất tinh tế. Bản thân cạnh đó sẽ khép lại một chu kỳ lẻ. Việc xóa nó chỉ sửa biểu đồ khi đó là bộ mô tả chu kỳ lẻ duy nhất trong toàn bộ biểu đồ. 

Giới hạn đệ quy phải được tăng lên vì cây DFS có thể trở thành một chuỗi dài$10^4$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2
1 3
2 4
3 4
```Truyền tải DFS: 

| Bước | Cạnh | Độ sâu | Cạnh sau kỳ lạ? | lẻ_cnt | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 | 0,1 | Không | 0 | 
| 2 | 2-4 | 1,2 | Không | 0 | 
| 3 | 4-3 | 2,3 | Không | 0 | 
| 4 | 3-1 | 3,0 | Không | 0 | 

Không tồn tại chu kỳ lẻ. Biểu đồ đã có hai bên. 

Đầu ra:```
4
1 2 3 4
```Ví dụ này minh họa trường hợp đặc biệt`odd_cnt == 0`. Mọi cạnh đều hợp lệ vì việc loại bỏ các cạnh không thể phá hủy tính lưỡng cực. 

### Ví dụ 2 

đầu vào:```
3 3
1 2
2 3
3 1
```Truyền tải DFS: 

| Bước | Cạnh | Độ sâu | Cạnh sau kỳ lạ? | lẻ_cnt | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 | 0,1 | Không | 0 | 
| 2 | 2-3 | 1,2 | Không | 0 | 
| 3 | 3-1 | 2,0 | Có | 1 | 

Tuyên truyền đóng góp: 

| Đỉnh | đánh dấu trước khi thu thập | Tổng cây con cuối cùng | 
| --- | --- | --- | 
| 3 | +1 | 1 | 
| 2 | 0 | 1 | 
| 1 | -1 | 0 | 

Cả hai cạnh cây đều nhận giá trị 1, bằng`odd_cnt`. 

Bản thân cạnh sau cũng hoạt động vì`odd_cnt == 1`. 

Đầu ra:```
3
1 2 3
```Dấu vết này cho thấy mọi cạnh của một chu trình lẻ đều có thể tháo rời được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| Mỗi cạnh và đỉnh được xử lý một số lần không đổi | 
| Không gian |$O(n+m)$| Lưu trữ đồ thị cộng với mảng DFS | 

Với nhiều nhất$10^4$đỉnh và cạnh, độ phức tạp tuyến tính dễ dàng nằm gọn trong giới hạn. Thuật toán chỉ thực hiện hai lần truyền tải DFS và một số thao tác liên tục trên mỗi cạnh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n, m = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    edges = [None]

    for i in range(1, m + 1):
        u, v = map(int, input().split())
        edges.append((u, v))
        g[u].append((v, i))
        g[v].append((u, i))

    depth = [0] * (n + 1)
    visited = [False] * (n + 1)
    parent = [0] * (n + 1)

    mark = [0] * (n + 1)

    odd_edges = []
    odd_cnt = 0

    def dfs(u, pe):
        nonlocal odd_cnt

        visited[u] = True

        for v, idx in g[u]:
            if idx == pe:
                continue

            if not visited[v]:
                depth[v] = depth[u] + 1
                parent[v] = u
                dfs(v, idx)
            elif depth[v] < depth[u]:
                if (depth[u] - depth[v]) % 2 == 0:
                    odd_cnt += 1
                    odd_edges.append(idx)

                    mark[u] += 1
                    mark[v] -= 1

    for i in range(1, n + 1):
        if not visited[i]:
            dfs(i, -1)

    if odd_cnt == 0:
        print(m)
        print(*range(1, m + 1))
        return

    ans = []

    def collect(u, pe):
        subtotal = mark[u]

        for v, idx in g[u]:
            if idx == pe:
                continue

            if parent[v] == u:
                child = collect(v, idx)

                if child == odd_cnt:
                    ans.append(idx)

                subtotal += child

        return subtotal

    for i in range(1, n + 1):
        if parent[i] == 0:
            collect(i, -1)

    if odd_cnt == 1:
        ans.extend(odd_edges)

    ans.sort()

    print(len(ans))
    if ans:
        print(*ans)
    else:
        print()

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""4 4
1 2
1 3
2 4
3 4
"""
) == "4\n1 2 3 4\n"

# single triangle
assert run(
"""3 3
1 2
2 3
3 1
"""
) == "3\n1 2 3\n"

# two disconnected odd cycles
assert run(
"""6 6
1 2
2 3
3 1
4 5
5 6
6 4
"""
) == "0\n\n"

# no edges
assert run(
"""1 0
"""
) == "0\n\n"

# already bipartite chain
assert run(
"""5 4
1 2
2 3
3 4
4 5
"""
) == "4\n1 2 3 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác đơn | Tất cả các cạnh hợp lệ | Một chu kỳ lẻ | 
| Hai hình tam giác bị ngắt kết nối | Câu trả lời trống rỗng | Một cạnh không thể phá hủy nhiều chu kỳ lẻ | 
| Không có cạnh | Câu trả lời trống rỗng | Đồ thị tối thiểu | 
| Dây chuyền đơn giản | Tất cả các cạnh hợp lệ | Đã có biểu đồ lưỡng cực | 

## Vỏ cạnh 

Xét đồ thị không có chu trình lẻ:```
4 3
1 2
2 3
3 4
```DFS không bao giờ tìm thấy cạnh sau lẻ, vì vậy`odd_cnt = 0`. Thuật toán ngay lập tức xuất ra tất cả các cạnh. Điều này đúng vì việc xóa bất kỳ cạnh nào khỏi biểu đồ lưỡng cực sẽ duy trì tính lưỡng cực. 

Bây giờ hãy xem xét hai chu kỳ lẻ bị ngắt kết nối:```
6 6
1 2
2 3
3 1
4 5
5 6
6 4
```DFS phát hiện hai cạnh sau lẻ, vì vậy`odd_cnt = 2`. 

Mỗi cạnh của cây chỉ nhận được đóng góp 1 từ một tam giác, không bao giờ là 2. Không có cạnh nào thuộc cả hai chu kỳ lẻ cùng một lúc, vì vậy câu trả lời vẫn trống. 

Cuối cùng, hãy xem xét cấu trúc chồng chéo:```
5 6
1 2
2 3
3 1
3 4
4 5
5 3
```DFS tìm thấy hai chu kỳ lẻ. Các cạnh bên trong tam giác đầu tiên chỉ nhận được một đóng góp. Các cạnh bên trong tam giác thứ hai cũng nhận được một đóng góp. Không có cạnh nào đạt tới`odd_cnt = 2`. 

Thuật toán in chính xác:```
0
```bởi vì việc loại bỏ một cạnh vẫn để lại một chu kỳ kỳ lạ khác.
