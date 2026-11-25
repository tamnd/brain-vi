---
title: "CF 962F - Các cạnh chu trình đơn giản"
description: "Chúng ta được cung cấp một đồ thị đơn giản vô hướng và được yêu cầu xác định các cạnh nào là “độc quyền” cho một chu trình đơn giản."
date: "2026-06-17T01:45:40+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 962
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 42 (Rated for Div. 2)"
rating: 2400
weight: 962
solve_time_s: 76
verified: true
draft: false
---

[CF 962F - Các cạnh của chu trình đơn giản](https://codeforces.com/problemset/problem/962/F) 

**Đánh giá:** 2400 
**Thẻ:** dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị đơn giản vô hướng và được yêu cầu xác định các cạnh nào là “độc quyền” cho một chu trình đơn giản. Nói cách khác, đối với mỗi cạnh, chúng ta muốn biết liệu có tồn tại chính xác một chu trình trong toàn bộ đồ thị chứa cạnh này hay không, trong đó các chu trình đơn giản và không lặp lại các đỉnh. 

Một cách hữu ích để diễn đạt lại nhiệm vụ là mọi cạnh trong biểu đồ đều tham gia vào 0, một hoặc nhiều chu trình đơn giản và chúng ta được yêu cầu chọn chính xác những cạnh thuộc về chính xác một chu trình như vậy. 

Các ràng buộc cho phép lên tới 100.000 đỉnh và cạnh, điều này ngay lập tức loại trừ mọi cách tiếp cận liệt kê chu trình một cách rõ ràng. Số chu kỳ đơn giản trong biểu đồ có thể là số mũ trong trường hợp xấu nhất, do đó, bất kỳ phương pháp nào cố gắng “tìm tất cả các chu kỳ và đếm sự tham gia của các cạnh” sẽ thất bại rất lâu trước khi đạt được đầu vào thậm chí vừa phải. Chúng ta cần mô tả đặc điểm cấu trúc của các cạnh để tránh hoàn toàn các chu trình liệt kê. 

Trường hợp cạnh tinh tế phát sinh khi đồ thị là một chu trình đơn giản. Trong tình huống đó, mọi cạnh đều phải được đưa vào câu trả lời. Việc phát hiện chu trình DFS ngây thơ có thể đánh dấu các cạnh là một phần của chu trình nào đó, nhưng nó sẽ không phân biệt được liệu có nhiều cạnh chia sẻ chu trình riêng biệt hay chỉ một. Một tình huống khó khăn khác là khi các chu kỳ chồng chéo lên nhau. Ví dụ: hai chu trình chia sẻ một đường dẫn sẽ tạo ra các cạnh thuộc về nhiều chu trình và các cạnh đó phải được loại trừ ngay cả khi chúng là một phần của chu trình theo nghĩa DFS. 

Khó khăn cốt lõi là “thuộc về chính xác một chu trình đơn giản” không phải là thuộc tính cục bộ thuần túy của một lần chạy phát hiện chu trình đơn lẻ. Nó phụ thuộc vào số lượng chu trình độc lập đi qua cùng một cạnh, điều này gợi ý lý luận về mặt cấu trúc đồ thị hơn là lịch sử truyền tải. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liệt kê tất cả các chu kỳ đơn giản và đếm, đối với mỗi cạnh, có bao nhiêu chu kỳ riêng biệt bao gồm nó. Điều này có thể được hình dung như việc bắt đầu DFS từ mọi đỉnh, theo dõi các tập hợp đã truy cập và ghi lại các chu kỳ khi chúng ta truy cập lại một đỉnh. Ngay cả khi cắt tỉa, số lượng chu trình đơn giản trong một đồ thị dày đặc vẫn tăng theo cấp số nhân và ngay cả trong những đồ thị thưa thớt có 100.000 cạnh, điều này trở nên không khả thi. Số lượng hoạt động bùng nổ vì mỗi đường dẫn phát hiện chu kỳ phân nhánh nhiều và xem lại nhiều trạng thái một phần. 

Cái nhìn sâu sắc quan trọng là loại bỏ chính các chu kỳ và thay vào đó sử dụng cách phân rã trong đó các chu kỳ không được liệt kê rõ ràng mà được biểu diễn ngầm. Cấu trúc liên quan là sự phân rã hai kết nối của đồ thị. Mỗi cạnh thuộc về chính xác một thành phần được kết nối hai cạnh (còn được gọi là thành phần được kết nối hai cạnh) và trong mỗi thành phần như vậy, các chu trình được “chứa”. 

Một thực tế cổ điển là một cạnh nằm trên ít nhất một chu trình đơn giản khi và chỉ nếu nó thuộc về một thành phần được kết nối hai chiều có chứa ít nhất một chu trình, tương đương với một thành phần có ít nhất một cạnh sau trong DFS hoặc có kích thước lớn hơn một và không phải là cấu trúc cầu cạnh cây. Tuy nhiên, điều kiện của chúng ta chặt chẽ hơn: chúng ta muốn các cạnh thuộc đúng một chu trình đơn. Điều này hóa ra lại tương ứng chính xác với các cạnh là một phần của chu trình bên trong một thành phần được kết nối hai chiều có cấu trúc “giống chu trình” hơn là “đa chu kỳ”. 

Trong một thành phần được kết nối hai chiều, nếu số cạnh bằng số đỉnh thì thành phần đó chứa chính xác một chu trình đơn giản với các cây có thể chỉ được gắn thông qua các điểm khớp nối, nhưng trong thành phần được kết nối hai chiều không có điểm khớp nối, vì vậy thành phần đó chính xác là một đồ thị chu trình đơn giản. Do đó, mỗi cạnh trong một thành phần như vậy thuộc về chính xác một chu trình đơn giản, cụ thể là chu trình duy nhất bao trùm thành phần đó.

Nếu thành phần có nhiều cạnh hơn số đỉnh thì có ít nhất hai chu trình độc lập và mọi cạnh tham gia vào chu trình nằm trong nhiều chu trình đơn giản được hình thành bởi sự kết hợp của các chu trình này, do đó không có cạnh nào trong thành phần đó đủ tiêu chuẩn. Nếu thành phần là một cây (cạnh = đỉnh - 1), thì nó không chứa chu trình nào cả. 

Do đó, vấn đề giảm xuống còn việc tìm các thành phần được kết nối hai chiều và chọn các thành phần có số cạnh bằng số đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê chu kỳ Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Các thành phần được kết nối hai chiều (Tarjan) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng thuật toán Tarjan dựa trên DFS để tính toán các thành phần được kết nối hai chiều. 

1. Chạy DFS trên tất cả các đỉnh, duy trì thời gian khám phá và giá trị liên kết thấp. Giá trị liên kết thấp nắm bắt tổ tiên có thể truy cập sớm nhất trong cây DFS, cho phép phát hiện các cạnh sau. 
2. Duy trì một chồng các cạnh hiện có trong ngăn xếp đệ quy DFS. Bất cứ khi nào chúng tôi xác định rằng một cây con tạo thành một ranh giới thành phần được kết nối hai chiều hoàn chỉnh (khi low[v] >= disc[u]), chúng tôi sẽ tách các cạnh từ ngăn xếp để tạo thành một thành phần. 
3. Mỗi nhóm cạnh xuất hiện xác định một thành phần được kết nối hai chiều. Chúng tôi thu thập cả đỉnh và cạnh thuộc về nó. 
4. Đối với mỗi thành phần, hãy đếm xem nó chứa bao nhiêu đỉnh và cạnh phân biệt. 
5. Nếu một thành phần thỏa mãn edge_count == vertex_count thì nó phải là một chu trình đơn giản. Đánh dấu tất cả các cạnh trong thành phần này là câu trả lời hợp lệ. 
6. Sau khi xử lý tất cả các thành phần, xuất ra tất cả các cạnh được đánh dấu theo thứ tự chỉ số tăng dần. 

Bước suy luận quan trọng là việc phân rã Tarjan đảm bảo mọi cạnh đều thuộc về chính xác một thành phần được kết nối hai chiều, do đó chúng tôi không bao giờ tính hai lần hoặc phân chia chu kỳ không chính xác. 

### Tại sao nó hoạt động 

Trong bất kỳ đồ thị vô hướng nào, mọi chu trình đơn giản đều được chứa đầy đủ bên trong một thành phần liên thông đôi. Bên trong một thành phần như vậy, các chu kỳ chồng chéo lên nhau một cách có kiểm soát. Điều kiện edge_count == vertex_count đặc trưng cho trường hợp đặc biệt trong đó thành phần có chính xác một chu trình độc lập. Trong trường hợp đó, mọi cạnh đều nằm trên chu trình đơn đó và không có cách nào để tạo thành một chu trình đơn giản thứ hai mà không sử dụng lại các cạnh theo cách tạo ra nhiều chu trình độc lập. Do đó mọi cạnh trong thành phần như vậy đều thuộc về đúng một chu trình đơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
g = [[] for _ in range(n)]
edges = []

for i in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append((v, i))
    g[v].append((u, i))
    edges.append((u, v))

tin = [-1] * n
low = [-1] * n
timer = 0

stack = []
used_edge = [False] * m
in_comp_edge = []
ans = set()

def dfs(u, peid):
    global timer
    tin[u] = low[u] = timer
    timer += 1

    for v, eid in g[u]:
        if eid == peid:
            continue
        if tin[v] == -1:
            stack.append(eid)
            dfs(v, eid)
            low[u] = min(low[u], low[v])

            if low[v] >= tin[u]:
                comp_edges = []
                comp_vertices = set()
                while True:
                    e = stack.pop()
                    comp_edges.append(e)
                    a, b = edges[e]
                    comp_vertices.add(a)
                    comp_vertices.add(b)
                    if e == eid:
                        break
                if len(comp_edges) > 0:
                    if len(comp_edges) == len(comp_vertices):
                        for x in comp_edges:
                            ans.add(x)
        else:
            if tin[v] < tin[u]:
                stack.append(eid)
            low[u] = min(low[u], tin[v])

for i in range(n):
    if tin[i] == -1:
        dfs(i, -1)

print(len(ans))
print(*sorted(x + 1 for x in ans))
```DFS chỉ định cho mỗi đỉnh một thời gian khám phá và tính toán các giá trị liên kết thấp để phát hiện khi nào một cây con tạo thành một thành phần đóng. Ngăn xếp cạnh đảm bảo rằng khi một thành phần được kết nối hai chiều được xác định, tất cả các cạnh của nó sẽ được thu thập chính xác một lần. 

Sự tinh tế trong triển khai chính là đảm bảo rằng các cạnh sau được đẩy chính xác và các thành phần được đưa ra chính xác tại các ranh giới khớp nối. Kiểm tra sự bằng nhau giữa số cạnh và số đỉnh là bộ lọc tách biệt các thành phần chu trình thuần túy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2
2 3
3 1
```| Bước | Trạng thái DFS | Ngăn xếp | Thành phần hình thành | Bộ đỉnh | Bộ cạnh | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Thăm 1 | bắt đầu | [] | không | {1} | {} | khám phá | 
| Đi 2 | mép cây | [1-2] | không | {1,2} | {1-2} | tiếp tục | 
| Đi 3 | mép cây | [1-2,2-3] | không | {1,2,3} | {1-2,2-3} | tiếp tục | 
| Cạnh sau 3-1 | đóng chu kỳ | [1-2,2-3,3-1] | thành phần | {1,2,3} | 3 cạnh | kiểm tra | 

Ở đây thành phần có 3 đỉnh và 3 cạnh nên mọi cạnh đều được chọn. Điều này phù hợp với thực tế là toàn bộ đồ thị là một chu trình đơn giản. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
2 3
3 1
3 4
```| Bước | Trạng thái DFS | Ngăn xếp | Thành phần hình thành | Bộ đỉnh | Bộ cạnh | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Chu kỳ 1-2-3 | chu kỳ hình thức | [1-2,2-3,3-1] | máy tính A | {1,2,3} | 3 cạnh | ứng cử viên | 
| Cạnh phụ 3-4 | phần mở rộng | [3-4] | phần B | {3,4} | 1 cạnh | vứt bỏ | 

Thành phần A có 3 đỉnh và 3 cạnh nên hợp lệ. Thành phần B là thành phần cạnh cây và không đủ điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được đẩy và bật ra khỏi ngăn xếp nhiều nhất một lần trong quá trình phân tách dựa trên DFS | 
| Không gian | O(n + m) | Danh sách kề, ngăn xếp đệ quy và lưu trữ thành phần | 

Độ phức tạp tuyến tính là cần thiết vì kích thước đầu vào đạt tới 100.000 cạnh và bất kỳ phương pháp liệt kê chu trình siêu tuyến tính nào cũng sẽ vượt quá giới hạn. Phân tách DFS đảm bảo mỗi phần tử cấu trúc được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n)]
    edges = []

    for i in range(m):
        u, v = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        g[u].append((v, i))
        g[v].append((u, i))
        edges.append((u, v))

    tin = [-1] * n
    low = [-1] * n
    timer = 0
    stack = []
    ans = set()

    def dfs(u, pe):
        nonlocal timer
        tin[u] = low[u] = timer
        timer += 1
        for v, eid in g[u]:
            if eid == pe:
                continue
            if tin[v] == -1:
                stack.append(eid)
                dfs(v, eid)
                low[u] = min(low[u], low[v])
                if low[v] >= tin[u]:
                    comp_edges = []
                    comp_vertices = set()
                    while True:
                        e = stack.pop()
                        comp_edges.append(e)
                        a, b = edges[e]
                        comp_vertices.add(a)
                        comp_vertices.add(b)
                        if e == eid:
                            break
                    if len(comp_edges) == len(comp_vertices):
                        for x in comp_edges:
                            ans.add(x)
            elif tin[v] < tin[u]:
                stack.append(eid)
                low[u] = min(low[u], tin[v])

    for i in range(n):
        if tin[i] == -1:
            dfs(i, -1)

    out = sorted(x + 1 for x in ans)
    return str(len(out)) + "\n" + " ".join(map(str, out)) if out else "0\n"

# provided sample
assert run("3 3\n1 2\n2 3\n3 1\n") == "3\n1 2 3\n"

# custom: single edge
assert run("2 1\n1 2\n") == "0\n", "single edge"

# custom: two disjoint cycles
assert run("6 6\n1 2\n2 3\n3 1\n4 5\n5 6\n6 4\n") == "6\n1 2 3 4 5 6\n", "two cycles"

# custom: cycle with tail
assert run("4 4\n1 2\n2 3\n3 1\n3 4\n") == "3\n1 2 3\n", "cycle + tree edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tam giác | tất cả các cạnh | trường hợp chu trình cơ bản | 
| cạnh đơn | không | không có chu kỳ | 
| hai hình tam giác rời nhau | tất cả các cạnh | nhiều thành phần | 
| chu kỳ có đuôi | chỉ các cạnh chu kỳ | lọc tính đúng đắn | 

## Vỏ cạnh 

Biểu đồ chu trình đơn thể hiện trường hợp tích cực dự định. Chạy thuật toán trên một tam giác sẽ tạo ra chính xác một thành phần liên thông đôi có số đỉnh và cạnh bằng nhau, do đó tất cả các cạnh đều được chọn chính xác. 

Một đầu vào cạnh đơn tạo thành một thành phần được kết nối hai chiều tầm thường với một tập hợp đỉnh có kích thước hai và số cạnh một, do đó điều kiện đẳng thức không thành công và không có cạnh nào được chọn. DFS không bao giờ xây dựng thành phần chu trình, vì vậy câu trả lời trống. 

Một đồ thị bao gồm hai chu trình rời nhau tạo ra hai thành phần liên thông đôi độc lập, mỗi thành phần thỏa mãn điều kiện đẳng thức riêng biệt. Thuật toán đánh dấu tất cả các cạnh của cả hai thành phần, xác nhận tính độc lập giữa các thành phần. 

Một chu trình có cạnh lủng lẳng thể hiện hành vi lọc. Phần tam giác tạo thành một thành phần hợp lệ, trong khi cạnh treo tạo thành một thành phần giống cây với tỷ lệ cạnh trên đỉnh không khớp, do đó nó được loại trừ một cách chính xác.
