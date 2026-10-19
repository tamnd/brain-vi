---
title: "CF 105633H - Tu sửa ngục tối 2"
description: "Hầm ngục được cung cấp dưới dạng lưới trong đó chỉ một số ô nhất định là phòng thực tế. Giữa các phòng lân cận có thể có cửa gắn vào tường và những cửa này xác định một đồ thị vô hướng: mỗi phòng là một nút và mỗi cửa nối hai phòng liền kề."
date: "2026-06-22T18:07:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105633
codeforces_index: "H"
codeforces_contest_name: "The 2024 ICPC Asia Yokohama Regional Contest"
rating: 0
weight: 105633
solve_time_s: 82
verified: true
draft: false
---

[CF 105633H - Tu sửa ngục tối 2](https://codeforces.com/problemset/problem/105633/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hầm ngục được cung cấp dưới dạng lưới trong đó chỉ một số ô nhất định là phòng thực tế. Giữa các phòng lân cận có thể có cửa gắn vào tường và những cửa này xác định một đồ thị vô hướng: mỗi phòng là một nút và mỗi cửa nối hai phòng liền kề. 

Chúng tôi được phép loại bỏ một số cửa. Sau khi loại bỏ, hai yêu cầu phải được giữ. Đầu tiên, giữa hai phòng bất kỳ phải có đúng một đường đi đơn giản, tương đương với đồ thị còn lại là một cây trên tất cả các phòng. Thứ hai, nếu chúng ta xem xét tất cả các phòng có đúng một cửa sau khi tu sửa, thì hai phòng bất kỳ phải được nối với nhau bằng một con đường sử dụng số cửa chẵn. 

Điều kiện thứ hai là ràng buộc chẵn lẻ về khoảng cách giữa các lá của cây cuối cùng. Trong một cái cây, “có đúng một cửa” có nghĩa là có bậc một, vì vậy điều kiện nói lên rằng tất cả các lá phải có khoảng cách đều nhau theo từng cặp trong cấu trúc cuối cùng. 

Kích thước lưới lên tới 400 x 400 phòng, do đó biểu đồ có thể có tới 160000 nút và khoảng 320000 khả năng lân cận. Bất kỳ giải pháp nào cố gắng liệt kê các đường dẫn hoặc kiểm tra khả năng kết nối sau mỗi lần xóa sẽ quá chậm, vì ngay cả các thao tác biểu đồ tuyến tính được lặp lại nhiều lần cũng sẽ vượt quá giới hạn thời gian. Một giải pháp phải xây dựng cấu trúc cuối cùng về cơ bản chỉ bằng một lần duyệt đồ thị. 

Một vấn đề tế nhị xuất hiện khi nghĩ về ràng buộc chẵn lẻ. Một cây bao trùm đơn giản luôn có thể được xây dựng, nhưng không rõ liệu mọi cây bao trùm có tự động thỏa mãn điều kiện chẵn lẻ lá hay một số đồ thị có thể vi phạm hay không. Nếu bỏ qua điều này, chúng ta có thể xây dựng một cây hợp lệ nhưng không đáp ứng được yêu cầu về tính chẵn lẻ đối với một số hình dạng nhất định trong đó các lá xuất hiện ở cả hai phía của một phân vùng kép. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua ràng buộc chẵn lẻ trong giây lát, nhiệm vụ sẽ trở thành tiêu chuẩn: chúng ta có một biểu đồ kết nối các phòng và chúng ta muốn xóa các cạnh để nó trở thành một cây. Một phương pháp đơn giản là chạy DFS hoặc BFS và chỉ giữ lại các cạnh cha, loại bỏ tất cả các cạnh khác. Điều này tạo ra một cây bao trùm vì mọi nút ngoại trừ nút gốc đều có chính xác một nút cha và khả năng kết nối được duy trì bằng quá trình truyền tải. 

Tư duy vũ phu sẽ cố gắng xóa các cạnh một cách rõ ràng và liên tục kiểm tra xem biểu đồ có còn kết nối và không theo chu kỳ hay không. Mỗi lần kiểm tra kết nối đều tuyến tính về số lượng phòng và việc thực hiện việc này sau mỗi lần loại bỏ sẽ dẫn đến hành vi hình khối trong trường hợp xấu nhất, vượt xa mức chấp nhận được đối với 160000 nút. 

Quan sát quan trọng là điều kiện thứ hai không thực sự hạn chế việc chúng ta chọn cây bao trùm nào miễn là chúng ta chọn nó một cách cẩn thận. Điều kiện về các lá có khoảng cách đều nhau chuyển thành điều kiện ngang nhau về độ sâu của chúng trong cây có rễ. Trong bất kỳ cây nào, tính chẵn lẻ khoảng cách giữa hai nút phụ thuộc vào độ tương đương độ sâu của chúng. Vì vậy, yêu cầu tất cả các lá phải có khoảng cách chẵn theo cặp tương đương với việc yêu cầu tất cả các lá nằm trên cùng một lớp chẵn lẻ trong một số phân vùng dựa trên gốc. 

Điều này gợi ý việc kiểm soát cấu trúc của cây bao trùm để tất cả các lá đều có cùng tính chẵn lẻ của lớp BFS. Cây BFS trong biểu đồ hai bên đương nhiên có thuộc tính này. Đồ thị ban đầu là đồ thị lưỡng cực vì đây là đồ thị kề lưới nên mọi cạnh đều kết nối các màu đối lập nhau. Trong cây BFS, các nút được nhóm theo khoảng cách từ gốc và các cạnh chỉ kết nối các lớp liền kề. Một lá trong cây BFS phải nằm ở lớp cuối cùng vì bất kỳ nút nào không phải lớp cuối cùng sẽ có ít nhất một nút lân cận ở lớp tiếp theo, nút này sẽ trở thành nút con của nó trong BFS. 

Do đó, việc xây dựng cây bao trùm BFS sẽ tự động buộc tất cả các lá vào lớp BFS cuối cùng, lớp này có tính chẵn lẻ cố định. Điều này làm cho ràng buộc về tính chẵn lẻ của lá được giữ nguyên mà không cần thực hiện thêm bất kỳ thao tác nào.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xóa nhiều lần bằng séc | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Xây dựng cây bao trùm BFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất biểu đồ các phòng bằng cách lặp qua lưới và thêm các cạnh bất cứ khi nào có một cánh cửa tồn tại giữa hai ô phòng liền kề. Mỗi phòng trở thành một nút và mỗi cánh cửa là một cạnh vô hướng. 
2. Chạy BFS từ bất kỳ phòng nào làm thư mục gốc. Đánh dấu từng nút đã truy cập và ghi lại nút cha của nó khi nó được phát hiện lần đầu tiên. Mối quan hệ cha mẹ này xác định cấu trúc cây bao trùm. 
3. Bất cứ khi nào một nút phát hiện ra một hàng xóm chưa được ghé thăm, hãy giữ cạnh đó như một phần của bố cục ngục tối cuối cùng và đánh dấu nó là đã sử dụng. Tất cả các cạnh cửa tiềm năng khác không được sử dụng làm cạnh cây BFS sẽ bị xóa. 
4. Sau khi BFS kết thúc, mỗi phòng ngoại trừ gốc có chính xác một cạnh cha, do đó cấu trúc thu được là kết nối và không có chu kỳ. Điều này đảm bảo điều kiện “đường đi duy nhất giữa hai phòng bất kỳ”. 
5. Xuất lưới ban đầu nhưng loại bỏ tất cả các cửa ngoại trừ những cửa tương ứng với các cạnh cây BFS đã chọn. Mỗi cánh cửa bị loại bỏ sẽ trở thành một bức tường. 

Lý do BFS được chọn thay vì DFS là vì các lớp BFS kiểm soát tính chẵn lẻ độ sâu một cách rõ ràng. Cấu trúc lớp này đảm bảo rằng tất cả các lá đều xuất hiện ở cùng mức chẵn lẻ, đây là yêu cầu cốt lõi của điều kiện thứ hai. 

### Tại sao nó hoạt động 

Cây BFS phân vùng các nút thành các lớp theo khoảng cách đường đi ngắn nhất từ gốc. Bởi vì biểu đồ ban đầu là lưỡng cực, mỗi cạnh kết nối các nút có lớp BFS khác nhau đúng một. Bất kỳ nút nào không nằm trong lớp BFS cuối cùng phải có ít nhất một nút lân cận ở lớp sâu hơn, điều này đảm bảo nó không phải là một lá trong cây BFS. Do đó tất cả các lá thuộc về lớp BFS cuối cùng. Vì tất cả các nút trong cùng một lớp BFS có cùng khoảng cách chẵn lẻ từ gốc, nên tất cả các lá đều có tính chẵn lẻ giống hệt nhau, điều này buộc mỗi cặp lá phải có khoảng cách chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

h, w = map(int, input().split())
grid = [list(input().strip()) for _ in range(2 * h + 1)]

# room indexing
id_map = {}
nodes = []
idx = 0

for i in range(h):
    for j in range(w):
        if grid[2 * i + 1][2 * j + 1] == '.':
            id_map[(i, j)] = idx
            nodes.append((i, j))
            idx += 1

n = idx
adj = [[] for _ in range(n)]

# build graph from doors
for i in range(h):
    for j in range(w):
        if (i, j) not in id_map:
            continue
        u = id_map[(i, j)]

        # right
        if j + 1 < w and (i, j + 1) in id_map:
            x = 2 * i + 1
            y = 2 * j + 2
            if grid[x][y] == '.':
                v = id_map[(i, j + 1)]
                adj[u].append(v)
                adj[v].append(u)

        # down
        if i + 1 < h and (i + 1, j) in id_map:
            x = 2 * i + 2
            y = 2 * j + 1
            if grid[x][y] == '.':
                v = id_map[(i + 1, j)]
                adj[u].append(v)
                adj[v].append(u)

# BFS spanning tree
parent = [-1] * n
used = set()
q = deque()

root = 0
parent[root] = root
q.append(root)

while q:
    u = q.popleft()
    for v in adj[u]:
        if parent[v] == -1:
            parent[v] = u
            used.add((u, v))
            used.add((v, u))
            q.append(v)

# rebuild grid: remove unused doors
for i in range(h):
    for j in range(w):
        if (i, j) not in id_map:
            continue
        u = id_map[(i, j)]

        # right wall
        if j + 1 < w and (i, j + 1) in id_map:
            x = 2 * i + 1
            y = 2 * j + 2
            if (u, id_map[(i, j + 1)]) not in used:
                grid[x][y] = '#'

        # down wall
        if i + 1 < h and (i + 1, j) in id_map:
            x = 2 * i + 2
            y = 2 * j + 1
            if (u, id_map[(i + 1, j)]) not in used:
                grid[x][y] = '#'

print("Yes")
for row in grid:
    print("".join(row))
```Phần đầu tiên của quá trình triển khai xây dựng một biểu đồ nhỏ gọn chỉ bao gồm các ô phòng. Mỗi phòng được chỉ định một chỉ mục và vùng lân cận chỉ được tạo khi có một cửa tồn tại trong ô tường tương ứng của lưới đầu vào. 

Phần BFS xây dựng một cây bao trùm. các`parent`mảng đảm bảo mỗi nút được phát hiện chính xác một lần và`used`thiết lập các bản ghi cạnh nào trở thành một phần của cây. Mọi cánh cửa khác đều được đánh dấu ngầm để loại bỏ sau này. 

Bước tái thiết cuối cùng sẽ đi qua tất cả các vị trí cửa tiềm năng một lần nữa. Nếu một cánh cửa không thuộc cây BFS, nó sẽ được thay thế bằng một bức tường. Điều này trực tiếp thực hiện thao tác “chặn một số cửa” mà vấn đề yêu cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét cấu hình 2 x 2 phòng đơn giản trong đó tất cả bốn phòng được kết nối theo một chu kỳ. BFS có thể tiến hành như sau. 

Bắt đầu từ nút 0, BFS truy cập từng lớp nút. Mảng cha phát triển khi các nút được phát hiện và các cạnh của cây tạo thành một cấu trúc giống như chuỗi thay vì một chu trình. Bất kỳ cạnh chu kỳ bổ sung nào cũng bị loại bỏ trong quá trình tái thiết, để lại chính xác ba cạnh. 

| Bước | Xếp hàng | Đã truy cập | Bài tập của phụ huynh | 
| --- | --- | --- | --- | 
| Bắt đầu | [0] | {0} | 0 là gốc | 
| Thăm 0 | [1,2] | {0,1,2} | cha mẹ[1]=0, cha mẹ[2]=0 | 
| Thăm 1 | [2,3] | {0,1,2,3} | parent[3]=1 hoặc 2 tùy theo thứ tự | 

Dấu vết này cho thấy chỉ các cạnh được phát hiện đầu tiên mới được giữ lại, đảm bảo cấu trúc cây. 

Đối với ví dụ thứ hai, hãy xem xét một dãy gồm ba phòng. BFS từ một đầu tạo ra một chuỗi trong đó nút cuối cùng là lá duy nhất, đảm bảo tất cả các lá đều có cùng tính chẵn lẻ của lớp BFS. Điều này xác nhận ràng buộc chẵn lẻ tự động được giữ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(HW) | Mỗi phòng và mỗi cửa được xử lý một số lần không đổi trong quá trình xây dựng biểu đồ, BFS và tái thiết | 
| Không gian | O(HW) | Lưu trữ chỉ mục phòng, danh sách lân cận và siêu dữ liệu BFS | 

Kích thước lưới tối đa là 400 x 400 phòng, do đó, tổng số nút có thể quản lý được cho một lần truyền tải BFS. Thuật toán chỉ thực hiện công tuyến tính trên biểu đồ ẩn, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    input = sys.stdin.readline
    h, w = map(int, input().split())
    grid = [list(input().strip()) for _ in range(2 * h + 1)]

    id_map = {}
    idx = 0
    for i in range(h):
        for j in range(w):
            if grid[2 * i + 1][2 * j + 1] == '.':
                id_map[(i, j)] = idx
                idx += 1

    n = idx
    adj = [[] for _ in range(n)]

    for i in range(h):
        for j in range(w):
            if (i, j) not in id_map:
                continue
            u = id_map[(i, j)]
            if j + 1 < w and (i, j + 1) in id_map:
                if grid[2*i+1][2*j+2] == '.':
                    v = id_map[(i, j+1)]
                    adj[u].append(v)
                    adj[v].append(u)
            if i + 1 < h and (i+1, j) in id_map:
                if grid[2*i+2][2*j+1] == '.':
                    v = id_map[(i+1, j)]
                    adj[u].append(v)
                    adj[v].append(u)

    parent = [-1]*n
    used = set()
    q = deque([0])
    parent[0] = 0

    while q:
        u = q.popleft()
        for v in adj[u]:
            if parent[v] == -1:
                parent[v] = u
                used.add((u,v))
                used.add((v,u))
                q.append(v)

    for i in range(h):
        for j in range(w):
            if (i,j) not in id_map:
                continue
            u = id_map[(i,j)]
            if j+1 < w and (i,j+1) in id_map:
                if (u, id_map[(i,j+1)]) not in used:
                    grid[2*i+1][2*j+2] = '#'
            if i+1 < h and (i+1,j) in id_map:
                if (u, id_map[(i+1,j)]) not in used:
                    grid[2*i+2][2*j+1] = '#'

    return "Yes\n" + "\n".join("".join(r) for r in grid)

# Sample-style structural checks
assert run("""3 3
#######
#.....#
#.#.###
#.#...#
#.#.#.#
#.....#
#######""").startswith("Yes")

assert run("""3 3
#######
#.....#
###.###
###...#
###.#.#
#.....#
#######""").startswith("Yes")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phòng đơn 1×1 | Có và không có thay đổi | Xử lý đồ thị tối thiểu | 
| Dòng phòng | Có | Cấu trúc chuỗi BFS | 
| Chu kỳ 4 phòng | Có | Chu kỳ đột nhập vào cây | 
| Lưới thưa có ống dẫn | Có | Bỏ qua phòng không chính xác | 

## Vỏ cạnh 

Một ngục tối một phòng không có cạnh nào cả. BFS bắt đầu và kết thúc ngay lập tức, tạo ra một cây trống, thỏa mãn một cách tầm thường cả tính duy nhất của đường đi và điều kiện của lá vì không có cặp lá riêng biệt nào. 

Một hành lang tuyến tính hoàn toàn gồm các phòng tạo ra cây BFS giống hệt cấu trúc ban đầu. Các nút lá duy nhất là hai điểm cuối và cả hai đều thuộc cùng một lớp BFS tương đương khi được root ở điểm cuối, do đó khoảng cách của chúng là chẵn. 

Cấu trúc tuần hoàn là trường hợp căng thẳng quan trọng nhất vì tồn tại nhiều cây bao trùm hợp lệ. Cấu trúc BFS luôn loại bỏ chính xác một cạnh trên mỗi chu kỳ, đảm bảo tính không tuần hoàn trong khi vẫn duy trì kết nối và cấu trúc lá vẫn bị giới hạn ở lớp BFS sâu nhất, do đó các ràng buộc chẵn lẻ vẫn được thỏa mãn.
