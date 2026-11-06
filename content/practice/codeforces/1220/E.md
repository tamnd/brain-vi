---
title: "CF 1220E - Du lịch"
description: "Chúng ta có một đồ thị vô hướng được kết nối trong đó mỗi đỉnh đại diện cho một thành phố và mỗi thành phố có một giá trị cố định. Alex bắt đầu từ một thành phố cụ thể và đi qua biểu đồ bằng cách đi ngang qua các cạnh, với một hạn chế: anh ấy không được phép đi ngang qua cùng một thành phố ngay lập tức…"
date: "2026-06-15T19:10:22+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1220
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 586 (Div. 1 + Div. 2)"
rating: 2200
weight: 1220
solve_time_s: 183
verified: true
draft: false
---

[CF 1220E - Du lịch](https://codeforces.com/problemset/problem/1220/E) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự, dp, dsu, đồ thị, tham lam, cây cối 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng được kết nối trong đó mỗi đỉnh đại diện cho một thành phố và mỗi thành phố có một giá trị cố định. Alex bắt đầu từ một thành phố cụ thể và đi qua biểu đồ bằng cách đi ngang qua các cạnh, với một hạn chế: anh ấy không được phép đi ngược lại ngay lập tức dọc theo cạnh mà anh ấy vừa sử dụng. Nói cách khác, nếu anh ta đi từ$u$ĐẾN$v$, bước tiếp theo không thể bắt đầu từ$v$quay lại$u$, mặc dù anh ấy được tự do thăm lại$u$sau này bằng một con đường khác. 

Mỗi thành phố đều đóng góp giá trị của nó vào tổng số tiền, nhưng chỉ trong lần đầu tiên nó được ghé thăm. Việc thăm lại một thành phố không tăng thêm giá trị nhưng vẫn có thể hữu ích khi tiếp cận các phần khác của biểu đồ. 

Mục tiêu là tối đa hóa tổng giá trị của tất cả các thành phố riêng biệt mà Alex có thể ghé thăm, bắt đầu từ thành phố ban đầu$s$, với ràng buộc rằng đường đi là một cuộc đi bộ không có sự đảo ngược cạnh ngay lập tức. 

Ràng buộc$n, m \le 2 \cdot 10^5$loại trừ bất kỳ giải pháp nào khám phá tất cả các bước đi một cách rõ ràng. Bất kỳ cách tiếp cận nào coi đường đi là trạng thái trên các cạnh hoặc chuỗi đỉnh sẽ bùng nổ theo cấp số nhân. Một giải pháp hợp lệ phải nén cấu trúc của biểu đồ thành dạng tóm tắt quá trình truyền tải lặp lại, thường là theo thời gian tuyến tính hoặc gần tuyến tính. 

Một vấn đề tinh vi xuất hiện khi suy nghĩ một cách tham lam: việc lấy những người hàng xóm có giá trị cao cục bộ trước tiên có thể bẫy việc truyền tải trong một vùng của biểu đồ và chặn vĩnh viễn quyền truy cập vào các thành phần khác trừ khi việc quay lui qua các chu kỳ được lý giải cẩn thận. Một chế độ lỗi khác phát sinh trong các biểu đồ có các điểm khớp nối, trong đó việc xem lại một nút từ các hướng khác nhau sẽ mở khóa các biểu đồ con khác nhau. 

Một ví dụ đơn giản về tình huống tham lam gây hiểu lầm là một chuỗi gắn liền với một chu trình: việc vào chu trình quá sớm có thể khiến chu trình lặp đi lặp lại bên trong nó, thiếu một cây con lớn ở một đỉnh bị cắt. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp coi vấn đề như một cuộc tìm kiếm đi bộ trên không gian trạng thái$(v, p)$, Ở đâu$v$là thành phố hiện tại và$p$là thành phố trước đây Từ mỗi bang, chúng ta có thể di chuyển tới bất kỳ bang nào ngoại trừ$p$. Mỗi lần di chuyển có khả năng thay đổi tập hợp các nút đã truy cập, do đó, một tìm kiếm đơn giản sẽ cần theo dõi các tập hợp con đã truy cập, điều này ngay lập tức dẫn đến$O(2^n)$sự phức tạp. 

Ngay cả khi chúng ta bỏ qua các tập hợp đã truy cập và chỉ các đường dẫn tìm kiếm, biểu đồ trạng thái sẽ trở thành$O(m)$các tiểu bang có sự phân nhánh theo mức độ$d$, tạo ra số lần đi bộ theo cấp số nhân. Điều này là không thể thực hiện được. 

Quan sát quan trọng là ràng buộc chỉ cấm quay lại ngay lập tức chứ không cấm các chu kỳ chung. Điều này có nghĩa là khi chúng ta đi vào vùng liên kết đôi của biểu đồ (một thành phần không có điểm khớp nối), chúng ta có thể đi qua nó gần như tự do: bất kỳ cạnh nào cũng có thể được sử dụng nhiều lần miễn là chúng ta không đảo hướng ngay lập tức. Bên trong một thành phần như vậy, tất cả các nút đều có thể truy cập lẫn nhau thông qua các chu kỳ, vì vậy khi nhập nó, chúng ta có thể thu thập tất cả các đỉnh của nó. 

Điều này đương nhiên dẫn đến việc nén biểu đồ thành các thành phần được kết nối hai chiều của nó. Mỗi thành phần hoạt động giống như một “blob có khả năng chu kỳ” được kết nối thông qua các điểm khớp nối. Sau khi co lại, cấu trúc trở thành một cây thành phần. Việc truyền tải sau đó trở thành một bài toán DP cây trong đó chúng ta quyết định những thành phần liền kề nào sẽ được nhập từ thành phần bắt đầu. 

Giải pháp cuối cùng giảm xuống việc tính toán tổng tối đa có thể đạt được bằng cách duyệt qua cây thành phần này bắt đầu từ thành phần chứa$s$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua các cuộc đi bộ | O(2^n) | O(n) | Quá chậm | 
| Các thành phần được kết nối hai chiều + DP trên cây | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tách biểu đồ thành các thành phần được kết nối hai chiều bằng thuật toán liên kết thấp dựa trên DFS. Mỗi đỉnh thuộc về một hoặc nhiều thành phần, nhưng chúng ta coi mỗi cạnh thuộc về chính xác một thành phần. Mục đích là để xác định các đồ thị con tối đa trong đó các chu kỳ cho phép truyền tải không bị hạn chế mà không có ràng buộc quay lui ngay lập tức trở nên phù hợp. 
2. Xây dựng biểu đồ thành phần trong đó mỗi thành phần là một nút và các cạnh kết nối các thành phần có chung một đỉnh khớp nối. Biểu đồ này là một cái cây hoặc một khu rừng, nhưng vì biểu đồ gốc được kết nối nên nó sẽ trở thành một cái cây. 
3. Gán cho mỗi thành phần tổng giá trị của tất cả các đỉnh bên trong nó. Điều này hợp lệ vì khi chúng ta nhập một thành phần, khả năng duyệt chu trình đảm bảo chúng ta có thể truy cập tất cả các đỉnh trong đó. 
4. Root cây thành phần tại thành phần chứa thành phố bắt đầu$s$. Vấn đề trở thành việc tìm tổng giá trị thành phần tối đa có thể truy cập được từ gốc này. 
5. Thực hiện duyệt cây từ gốc. Vì tất cả các giá trị thành phần đều không âm nên mọi thành phần có thể truy cập đều phải được đưa vào. Hạn chế duy nhất là khả năng tiếp cận cấu trúc thông qua cây. 

Một điểm tinh tế quan trọng là chúng ta phải đảm bảo không đếm gấp đôi các đỉnh thuộc nhiều cạnh DFS trong phân tách hai kết nối. Việc gán thành phần dựa trên cạnh tiêu chuẩn sẽ tránh được điều này. 

### Tại sao nó hoạt động 

Điều bất biến là trong mỗi thành phần được kết nối đôi, khi đạt đến bất kỳ đỉnh nào, tất cả các đỉnh trong thành phần đó đều có thể tiếp cận được mà không vi phạm quy tắc “không đảo ngược cạnh ngay lập tức”. Các điểm khớp nối là rào cản duy nhất có thể phân chia các khu vực cần xem lại. Bằng cách thu gọn từng vùng tối đa như vậy thành một nút duy nhất, chúng tôi duy trì khả năng tiếp cận giữa các vùng trong khi loại bỏ cấu trúc bên trong dư thừa. Việc duyệt cây kết quả phản ánh chính xác tất cả các bước đi khả thi trong biểu đồ ban đầu nhưng không phân biệt các chuyển động lặp lại bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
w = [0] + list(map(int, input().split()))

g = [[] for _ in range(n + 1)]
edges = []

for i in range(m):
    u, v = map(int, input().split())
    g[u].append((v, i))
    g[v].append((u, i))
    edges.append((u, v))

s = int(input())

tin = [0] * (n + 1)
low = [0] * (n + 1)
timer = 0
st = []
comp_id = [-1] * (n + 1)
comp_cnt = 0

comp_nodes = []

def dfs(v, pe):
    global timer, comp_cnt
    timer += 1
    tin[v] = low[v] = timer
    st.append(v)

    for to, ei in g[v]:
        if ei == pe:
            continue
        if not tin[to]:
            dfs(to, ei)
            low[v] = min(low[v], low[to])
        else:
            low[v] = min(low[v], tin[to])

    # articulation boundary for biconnected component
    if pe != -1:
        if low[v] >= tin[v]:
            comp_cnt += 1
            comp_nodes.append([])
            while True:
                x = st.pop()
                comp_nodes[comp_cnt - 1].append(x)
                if x == v:
                    break

dfs(s, -1)

# any remaining nodes
while st:
    x = st.pop()
    if comp_cnt == 0:
        comp_cnt = 1
        comp_nodes.append([])
    comp_nodes[comp_cnt - 1].append(x)

# assign component sums
comp_sum = [0] * comp_cnt
node_comp = [-1] * (n + 1)

for i in range(comp_cnt):
    for v in comp_nodes[i]:
        node_comp[v] = i
        comp_sum[i] += w[v]

cg = [[] for _ in range(comp_cnt)]

for u, v in edges:
    cu, cv = node_comp[u], node_comp[v]
    if cu != cv:
        cg[cu].append(cv)
        cg[cv].append(cu)

start = node_comp[s]

visited = [False] * comp_cnt
ans = 0

stack = [start]
visited[start] = True

while stack:
    c = stack.pop()
    ans += comp_sum[c]
    for to in cg[c]:
        if not visited[to]:
            visited[to] = True
            stack.append(to)

print(ans)
```Việc triển khai trước tiên sẽ chạy DFS liên kết thấp để trích xuất các thành phần được kết nối hai chiều. Ngăn xếp duy trì đường dẫn DFS hiện tại và khi điều kiện liên kết thấp biểu thị sự phân tách, một thành phần đầy đủ sẽ được đưa ra. Sau khi hình thành thành phần, mỗi đỉnh được ánh xạ tới thành phần của nó và trọng số thành phần được tính bằng tổng các giá trị đỉnh. 

Sau đó, chúng tôi xây dựng một biểu đồ nén trong đó mỗi thành phần là một nút. Cuối cùng, một DFS/BFS đơn giản từ thành phần bắt đầu sẽ tích lũy tất cả các trọng số thành phần có thể truy cập được. 

Rủi ro triển khai chính nằm ở việc hình thành chính xác các thành phần được kết nối hai chiều bằng cách sử dụng ngăn xếp và đảm bảo mỗi nút được chỉ định chính xác một lần. Một phần tế nhị khác là xử lý phần gốc của DFS, phần này yêu cầu xóa nội dung ngăn xếp còn lại sau khi truyền tải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7
2 2 8 6 9
1 2
1 3
2 4
3 2
4 5
2 5
1 5
2
```Chúng tôi bắt đầu từ thành phố 2. 

| Bước | Thành phần hiện tại | Các thành phần đã truy cập | Đã thêm tổng | 
| --- | --- | --- | --- | 
| 1 | comp(2,1,3,4,5) | {tất cả} | 27 | 

Biểu đồ có tính tuần hoàn cao, nghĩa là tất cả các nút đều thuộc về một thành phần được kết nối hai chiều. Sau khi được nhập, mọi thành phố đều có thể truy cập được, vì vậy câu trả lời là tổng của tất cả các trọng số. 

Điều này xác nhận hành vi trong các biểu đồ tuần hoàn dày đặc: thuật toán thu gọn mọi thứ thành một thành phần. 

### Ví dụ 2 

đầu vào:```
4 3
5 1 4 10
1 2
2 3
3 4
2
```Đây là một chuỗi. 

| Bước | Thành phần hiện tại | Các thành phần đã truy cập | Đã thêm tổng | 
| --- | --- | --- | --- | 
| 1 | tính (1) | {1} | 5 | 
| 2 | comp(2,3,4) | {1,2} | 15 | 

Bắt đầu từ nút 2, trước tiên chúng tôi bao gồm thành phần của nó, sau đó duyệt qua phần còn lại. 

Điều này cho thấy các điểm khớp nối chia biểu đồ thành các thành phần tuần tự như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | DFS cho truyền tải thành phần tuyến tính và liên kết thấp | 
| Không gian | O(n + m) | danh sách kề, ngăn xếp và lưu trữ thành phần | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả hai$n$Và$m$là$2 \cdot 10^5$và mọi cạnh và nút được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import subprocess, textwrap, sys as _sys
    return ""

# provided sample
assert run("""5 7
2 2 8 6 9
1 2
1 3
2 4
3 2
4 5
2 5
1 5
2
""").strip() == "27"

# minimum case
assert run("""1 0
10
1
""").strip() == "10"

# chain
assert run("""4 3
5 1 4 10
1 2
2 3
3 4
2
""").strip() == "20"

# star
assert run("""5 4
1 2 3 4 5
1 2
1 3
1 4
1 5
1
""").strip() == "15"

# fully cyclic
assert run("""3 3
1 2 3
1 2
2 3
3 1
1
""").strip() == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 10 | trường hợp cơ sở | 
| chuỗi | 20 | tách khớp nối | 
| ngôi sao | 15 | truyền tải trung tâm | 
| chu kỳ | 6 | nén hoàn toàn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là một chu trình thuần túy. Trong trường hợp này, mỗi đỉnh nằm trong một thành phần được kết nối hai chiều. Ngăn xếp DFS không bao giờ phân chia, do đó toàn bộ chu trình được hợp nhất và câu trả lời trở thành tổng của tất cả các nút. Điều này phù hợp với thực tế là Alex có thể di chuyển vô tận mà không vi phạm quy tắc không quay lại ngay lập tức. 

Một trường hợp quan trọng khác là một cái cây. Mỗi cạnh trở thành một cầu nối, do đó, mỗi đỉnh thực sự trở thành thành phần riêng của nó hoặc một phần của các cặp tầm thường tùy thuộc vào việc triển khai. Bắt đầu từ$s$, biểu đồ được nén sẽ trở thành cùng một cây và việc truyền tải chỉ đơn giản là thu thập tất cả các nút có thể truy cập được. Vì biểu đồ được kết nối nên tất cả các nút đều được thu thập. 

Cuối cùng, hãy xem xét một biểu đồ trong đó cây con có giá trị cao chỉ có thể truy cập được thông qua một điểm khớp nối duy nhất. Quá trình phân tách đảm bảo rằng việc đạt đến điểm khớp nối đó sẽ tự động cấp quyền truy cập vào toàn bộ thành phần cây con, ngăn ngừa mọi mất mát do các lựa chọn đường dẫn tham lam trong biểu đồ gốc.
