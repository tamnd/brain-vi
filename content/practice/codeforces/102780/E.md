---
title: "CF 102780E - Bảng mạch in"
description: "Bảng là bản vẽ các dây được đặt trên một lưới hình chữ nhật. Mỗi ô không trống mô tả cạnh nào của ô đó chứa các đoạn dây."
date: "2026-07-27T20:09:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 93
verified: true
draft: false
---

[CF 102780E - Bảng mạch in](https://codeforces.com/problemset/problem/102780/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bảng là bản vẽ các dây được đặt trên một lưới hình chữ nhật. Mỗi ô không trống mô tả cạnh nào của ô đó chứa các đoạn dây. Lệnh dành cho máy vẽ bắt đầu tại một vị trí lưới và đưa ra một chuỗi các bước di chuyển, do đó, một lệnh tương ứng với việc đi dọc theo một đường liên tục của biểu đồ dây. 

Nhiệm vụ không phải là tạo lại bức tranh theo bất kỳ thứ tự cụ thể nào. Chúng ta chỉ cần chia dây hiện có thành số lượng vệt liên tục nhỏ nhất có thể và in những vệt đó dưới dạng lệnh. Một đường nhỏ có thể quay lại một ngã ba, nhưng nó có thể không bao giờ vẽ được một đoạn đã được vẽ sẵn. 

Mô hình tự nhiên là một đồ thị. Mỗi ô lưới bị chiếm đóng sẽ trở thành một đỉnh. Mọi kết nối giữa hai ô lân cận đều trở thành một cạnh vô hướng. Một lệnh chính xác là một bước đi qua các cạnh của biểu đồ này. Vấn đề trở thành việc tìm số lượng đường nhỏ nhất bao phủ mọi cạnh một lần. 

Kích thước lưới tối đa là 100 x 100, do đó có tối đa 10000 ô và khoảng 20000 kết nối có thể. Điều này loại trừ các cách tiếp cận liên tục tìm kiếm trên tất cả các phân tách có thể có hoặc thử hoán vị các cạnh. Một giải pháp truyền tải đồ thị với độ phức tạp tuyến tính có thể đủ nhanh. 

Một số chi tiết làm cho vấn đề này dễ mắc sai lầm hơn. Tọa độ ở đầu ra là cột đầu tiên và hàng thứ hai, trong khi đầu vào được đọc dưới dạng hàng. Việc triển khai bất cẩn có thể hoán đổi chúng và tạo ra các lệnh không hợp lệ. 

Ví dụ, hãy xem xét:```
1 3
r-*
```Đầu ra đúng có thể là:```
1
1 1 RRU
```Dây bắt đầu từ ô đầu tiên, đi thẳng qua ô giữa, đến ngôi sao và lệnh phải sử dụng tọa độ là xy. Một giải pháp xử lý tọa độ đầu vào là x y sẽ bắt đầu tại`(1, 3)`, nằm ngoài bảng. 

Một trường hợp cạnh khác là một chu trình khép kín:```
3 3
*-*
|.|
L-J
```Câu trả lời là một lệnh, vì tất cả các đỉnh đều có bậc chẵn và toàn bộ thành phần có chu trình Euler. Một giải pháp chỉ bắt đầu các đường đi từ các đỉnh bậc lẻ sẽ không tạo ra kết quả gì và bỏ lỡ chu trình. 

Trường hợp thứ ba là điểm giao nhau:```
5 5
..*..
..|..
*-*-*
..|..
..*..
```Ngôi sao trung tâm có độ bốn. Nó không phải là một điểm cuối. Câu trả lời đúng là hai lệnh, mỗi lệnh là một đường Euler xuyên qua một phần của đồ thị. Việc coi mọi ngôi sao là điểm cuối của dòng sẽ đưa ra quá nhiều lệnh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là liên tục chọn một cạnh không được sử dụng và tìm kiếm cách tốt nhất để tiếp tục vẽ. Điều này có tác dụng vì mọi lệnh hợp lệ đều là một đường nhỏ và cuối cùng mọi cạnh đều phải được đưa vào. Tuy nhiên, số lượng các lựa chọn có thể tăng lên một cách bùng nổ. Với khoảng 20000 cạnh, thậm chí chỉ xem xét các điểm bắt đầu khác nhau và việc tiếp tục cục bộ đã trở nên không thể. Vấn đề không phải là tìm ra một bản vẽ hợp lệ mà là giảm thiểu số lượng đường đi. 

Quan sát quan trọng là số lượng đường nhỏ nhất trong đồ thị vô hướng chỉ được xác định bằng tính chẵn lẻ của đỉnh. Mọi đường mòn đều thay đổi tính chẵn lẻ ở hai điểm cuối của nó. Một đỉnh có bậc lẻ phải là điểm cuối của một đường nhỏ nào đó. Vì một đường dẫn đóng góp hai điểm cuối nên một thành phần được kết nối với`odd`đỉnh bậc lẻ cần ít nhất`odd / 2`những con đường mòn. Nếu không có đỉnh bậc lẻ thì một chu trình Euler là đủ. 

Câu hỏi còn lại là làm thế nào để thực sự xây dựng những con đường này. Biểu đồ đủ nhỏ cho thuật toán Hierholzer, thuật toán này xây dựng đường dẫn Euler bằng cách đi theo các cạnh không được sử dụng cho đến khi quay lại điểm bắt đầu, sau đó hợp nhất các chu trình. Nếu một thành phần có các đỉnh lẻ, chúng ta bắt đầu từ chúng theo cặp. Nếu nó chỉ có các đỉnh chẵn thì chúng ta bắt đầu một lần từ bất kỳ đỉnh nào trong thành phần đó. 

Brute-force hoạt động vì nó cố gắng suy luận về thứ tự các cạnh. Việc quan sát tính chẵn lẻ sẽ loại bỏ hoàn toàn việc tìm kiếm đó và giảm bớt vấn đề tìm kiếm các đường Euler trong mỗi thành phần được kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số cạnh | O(V + E) | Quá chậm | 
| Tối ưu | O(V + E) | O(V + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích từng ô và tạo biểu đồ. Mỗi ô bị chiếm đóng là một đỉnh. Bất cứ khi nào hai ô lân cận đều yêu cầu kết nối giữa chúng, hãy thêm một cạnh vô hướng. 

Việc biểu diễn đồ thị rất hữu ích vì chuyển động của máy vẽ chính xác là chuyển động dọc theo các cạnh của đồ thị. Các ký hiệu bản vẽ ban đầu chỉ mô tả các kết nối cục bộ nên sau khi chuyển đổi, hình học không còn cần thiết nữa. 

1. Tìm mọi thành phần liên thông chứa ít nhất một cạnh. Trong quá trình duyệt một thành phần, thu thập tất cả các đỉnh có bậc lẻ. 

Chỉ các đỉnh có bậc lẻ mới ảnh hưởng đến số lượng lệnh. Các đỉnh độ chẵn có thể được nhập và rời mà không cần tạo một đường mới. 

1. Với mỗi thành phần, chọn điểm bắt đầu cho đường đi Euler. Nếu thành phần có các đỉnh lẻ, hãy sử dụng chúng làm điểm bắt đầu. Nếu nó không có đỉnh lẻ thì chọn bất kỳ đỉnh nào trong thành phần. 

Điều này cung cấp số lượng đường mòn tối thiểu. Các đỉnh lẻ phải xuất hiện dưới dạng điểm cuối, trong khi thành phần chẵn có một đường Euler khép kín. 

1. Chạy thuật toán Hierholzer từ mỗi lần bắt đầu đã chọn. Mỗi lần duyệt sẽ sử dụng các cạnh không được sử dụng và ghi lại chuỗi các đỉnh đã thăm. 

Mỗi cạnh được xóa chính xác một lần, do đó không có lệnh nào có thể vẽ lên đường hiện có. 

1. Chuyển đổi từng chuỗi đỉnh thành một lệnh. Đỉnh đầu tiên trở thành tọa độ đầu ra. Các đỉnh liên tiếp xác định các chữ cái chuyển động. 

Biểu đồ sử dụng hàng và cột bên trong, nhưng đầu ra yêu cầu cột rồi đến hàng, do đó việc chuyển đổi chỉ xảy ra khi in. 

Tại sao nó hoạt động: 

Đối với bất kỳ thành phần được kết nối nào, mọi đường dẫn đều có chính xác hai điểm cuối trừ khi đó là đường dẫn kín. Các đỉnh bậc lẻ không thể là điểm bên trong của tất cả các đường đi vì việc đi vào và đi ra đã sử dụng hai cạnh liên quan. Do đó, ít nhất một nửa số đỉnh lẻ được yêu cầu làm điểm cuối của đường mòn. Thuật toán tạo ra chính xác nhiều đường đi đó bằng cách bắt đầu từ các đỉnh lẻ hoặc chính xác một đường đi khi không có đỉnh lẻ nào. 

Thuật toán Hierholzer hợp lệ vì nó luôn đi theo các cạnh không được sử dụng và tạo ra đường Euler bất cứ khi nào các điều kiện chẵn lẻ được yêu cầu giữ nguyên. Vì mỗi thành phần được xử lý riêng biệt nên mỗi cạnh trong bản vẽ được bao gồm chính xác một lần và số lượng lệnh là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    board = [input().rstrip("\n") for _ in range(n)]

    dirs = {
        '-': [(0, -1), (0, 1)],
        '|': [(-1, 0), (1, 0)],
        'L': [(-1, 0), (0, 1)],
        'J': [(-1, 0), (0, -1)],
        'r': [(1, 0), (0, 1)],
        '7': [(1, 0), (0, -1)],
        '*': [(-1, 0), (1, 0), (0, -1), (0, 1)]
    }

    rev = {
        (-1, 0): (1, 0),
        (1, 0): (-1, 0),
        (0, -1): (0, 1),
        (0, 1): (0, -1)
    }

    adj = [[] for _ in range(n * m)]

    def node(r, c):
        return r * m + c

    for r in range(n):
        for c in range(m):
            if board[r][c] == '.':
                continue
            for dr, dc in dirs[board[r][c]]:
                nr, nc = r + dr, c + dc
                if 0 <= nr < n and 0 <= nc < m and board[nr][nc] != '.':
                    if rev[(dr, dc)] in dirs[board[nr][nc]]:
                        if node(r, c) < node(nr, nc):
                            a, b = node(r, c), node(nr, nc)
                            adj[a].append(b)
                            adj[b].append(a)

    used = [False] * n * m
    edge_used = [False] * sum(len(x) for x in adj) 
    edge_id = {}
    eid = 0
    for u in range(n * m):
        for v in adj[u]:
            if u < v:
                edge_id[(u, v)] = eid
                edge_id[(v, u)] = eid
                eid += 1

    for u in range(n * m):
        for i, v in enumerate(adj[u]):
            adj[u][i] = (v, edge_id[(u, v)])

    def hierholzer(start):
        stack = [(start, -1)]
        path = []
        while stack:
            u, incoming = stack[-1]
            while adj[u] and edge_used[adj[u][-1][1]]:
                adj[u].pop()
            if adj[u]:
                v, e = adj[u].pop()
                if edge_used[e]:
                    continue
                edge_used[e] = True
                stack.append((v, e))
            else:
                path.append(u)
                stack.pop()
        path.reverse()
        return path

    result = []
    visited = [False] * (n * m)

    for s in range(n * m):
        if board[s // m][s % m] != '.' and not visited[s]:
            stack = [s]
            comp = []
            visited[s] = True
            while stack:
                u = stack.pop()
                comp.append(u)
                for v, _ in adj[u]:
                    if not visited[v]:
                        visited[v] = True
                        stack.append(v)

            starts = [u for u in comp if len(adj[u]) % 2 == 1]
            if not starts:
                starts = [comp[0]]

            for start in starts:
                path = hierholzer(start)
                if len(path) > 1:
                    result.append(path)

    def direction(a, b):
        ar, ac = divmod(a, m)
        br, bc = divmod(b, m)
        if br == ar - 1:
            return 'U'
        if br == ar + 1:
            return 'D'
        if bc == ac - 1:
            return 'L'
        return 'R'

    out = [str(len(result))]
    for path in result:
        r, c = divmod(path[0], m)
        moves = ''.join(direction(path[i], path[i + 1])
                        for i in range(len(path) - 1))
        out.append(f"{c + 1} {r + 1} {moves}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên chuyển đổi hình ảnh thành danh sách kề. Việc xử lý đặc biệt của`*`là có chủ ý: một ngôi sao không cho chúng ta biết hướng nào được sử dụng, nhưng nó chỉ có thể được kết nối với các ô dây lân cận thực tế, do đó, việc cho phép tất cả bốn hướng và xác thực hướng lân cận sẽ mang lại các cạnh chính xác. 

ID cạnh cho phép thuật toán của Hierholzer đánh dấu một cạnh một lần mặc dù mỗi cạnh xuất hiện hai lần trong danh sách kề. Việc xóa các mục trong khi di chuyển ngang cũng tránh được việc quét liên tục các cạnh đã cạn kiệt. 

Việc tìm kiếm thành phần được thực hiện trước khi tạo các đường nhỏ vì các điều kiện Euler áp dụng riêng cho từng phần được kết nối của bản vẽ. Việc chuyển đổi đầu ra thêm một vào cả hai tọa độ vì mảng đầu vào và mảng bên trong được lập chỉ mục bằng 0 trong khi vấn đề sử dụng tọa độ được lập chỉ mục một. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 4
r--*
*...
L--*
```Biểu đồ có một thành phần được kết nối. Đỉnh lẻ của nó là ba ngôi sao? Trên thực tế, các ngôi sao phía dưới và phía trên cùng với ngôi sao trông có vẻ biệt lập ở giữa bên trái kết nối với nhau thành một thành phần, tạo ra bốn đỉnh lẻ. Thuật toán tạo ra hai đường mòn. 

| Bước | Đỉnh thành phần | Bắt đầu kỳ lạ | Đường dẫn được tạo | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị | Tất cả các ô không trống được kết nối | Bốn điểm cuối | Chưa | 
| Chọn bắt đầu | Tế bào sao bậc lẻ | Hai lần bắt đầu mỗi cặp | Ghép nối là tùy ý | 
| Truyền tải Euler | Tiêu thụ mọi cạnh một lần | Không còn lại | Hai đường dẫn lệnh | 

Điều này chứng tỏ thuật toán không cần giữ nguyên thứ tự bản vẽ ban đầu. Mọi tập lệnh tối thiểu đều hợp lệ. 

Đối với mẫu thứ hai:```
4 3
r--*
L--7
*--J
```Thành phần này tạo thành một vòng khép kín. Mỗi đỉnh đều có bậc chẵn nên thuật toán chọn một điểm bắt đầu tùy ý và tạo ra một chu trình Euler. 

| Bước | Đỉnh thành phần | Bắt đầu kỳ lạ | Đường dẫn được tạo | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị | Một chu kỳ | Trống | Cần một mạch | 
| Chọn bắt đầu | Ô chiếm đóng đầu tiên | Một sự khởi đầu | Quá trình truyền tải Euler bắt đầu | 
| Truyền tải Euler | Tất cả các cạnh được tiêu thụ | Không có | Một lệnh đóng | 

Điều này xác nhận việc xử lý đặc biệt các thành phần chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mọi ô và mọi cạnh đều được xử lý với số lần không đổi | 
| Không gian | O(nm) | Biểu đồ, mảng đã truy cập và trạng thái truyền tải là tuyến tính về số lượng ô | 

Bảng lớn nhất chỉ chứa 10000 ô, do đó việc truyền tải đồ thị tuyến tính dễ dàng phù hợp với giới hạn thời gian một giây và giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""3 4
r--*
*...
L--*
""").splitlines()[0] == "2"

assert run("""4 3
r--*
L--7
*--J
""").splitlines()[0] == "1"

assert run("""1 3
r-*
""").splitlines()[0] == "1"

assert run("""1 1
*
""") == "0"

assert run("""5 5
..*..
..|..
*-*-*
..|..
..*..
""").splitlines()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên | Hai lệnh | Nhiều điểm cuối mức độ lẻ | 
| Mẫu thứ hai | Một lệnh | Xử lý mạch Euler | 
| Đoạn ngang đơn | Một lệnh | Chuyển đổi tọa độ và hướng | 
| Sao đơn | Không có lệnh | Xử lý đồ thị trống | 
| Vẽ hình chữ thập | Hai lệnh | Ngã bốn độ | 

## Vỏ cạnh 

Trường hợp ngôi sao bị cô lập:```
1 1
*
```chứa một điểm được đánh dấu nhưng không có đoạn dây. Đồ thị không có cạnh nên không cần dùng lệnh. Thuật toán không bao giờ bắt đầu quá trình truyền tải Hierholzer vì không có thành phần nào có cạnh. 

Trường hợp vòng kín:```
3 3
*-*
|.|
L-J
```không có đỉnh bậc lẻ. Việc duyệt thành phần tìm thấy một danh sách lẻ trống và chọn một ô tùy ý làm điểm bắt đầu Euler. Hierholzer sử dụng toàn bộ vòng lặp và trả về một lệnh. 

Trường hợp vượt qua:```
5 5
..*..
..|..
*-*-*
..|..
..*..
```chứa đỉnh trung tâm bậc 4. Mức độ của nó là chẵn nên không làm tăng số lượng lệnh. Thành phần này có bốn điểm cuối lẻ, buộc hai đường dẫn. Thuật toán bắt đầu từ các điểm cuối đó và bao gồm mọi nhánh chính xác một lần. 

Trường hợp ranh giới tọa độ:```
1 3
r-*
```sử dụng các ô ở cạnh của bảng. Việc tạo hướng chỉ so sánh các chỉ số lân cận nên nó không bao giờ di chuyển ra ngoài mảng. Đầu ra cuối cùng hoán đổi chính xác thứ tự hàng và cột bên trong trước khi in.
