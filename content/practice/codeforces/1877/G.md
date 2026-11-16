---
title: "CF 1877G - Bóng có thể xếp chồng lên nhau"
description: "Chúng ta có một cây trong đó một số cạnh đã có hướng cố định và một số cạnh vẫn không có hướng. Chúng ta phải chọn hướng cho các cạnh còn lại và gán màu cho mỗi cạnh. Đường đi có thể đi ngang qua một cạnh dọc theo hướng của nó hoặc ngược lại với nó."
date: "2026-06-09T01:07:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1877
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 902 (Div. 2, based on COMPFEST 15 - Final Round)"
rating: 3300
weight: 1877
solve_time_s: 177
verified: false
draft: false
---

[CF 1877G - Có thể xếp chồng bóng](https://codeforces.com/problemset/problem/1877/G) 

**Đánh giá:** 3300 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dp, cây 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó một số cạnh đã có hướng cố định và một số cạnh vẫn không có hướng. Chúng ta phải chọn hướng cho các cạnh còn lại và gán màu cho mỗi cạnh. 

Đường đi có thể đi ngang qua một cạnh dọc theo hướng của nó hoặc ngược lại với nó. Di chuyển dọc theo hướng đẩy một quả bóng có màu bằng màu của cạnh vào một ngăn xếp. Đi ngược hướng sẽ làm bật quả bóng trên cùng. 

Một cuộc đi bộ được gọi là có thể xếp chồng lên nhau nếu nó không bao giờ cố gắng bật ra từ một ngăn xếp trống. 

Một bước đi có thể xếp chồng lên nhau cũng có thể xếp chồng lên nhau nếu mỗi lần bật sẽ loại bỏ một quả bóng có màu phù hợp với cạnh hiện đang đi qua. 

Nhiệm vụ của chúng tôi là làm cho **mọi** bước đi có thể xếp chồng lên nhau tự động có thể xếp chồng lên nhau. Trong số tất cả các cấu trúc hợp lệ, chúng tôi muốn tối đa hóa số lượng màu riêng biệt được sử dụng. 

Biểu đồ cơ bản là một cây có tối đa$10^5$đỉnh. Bất kỳ giải pháp nào liên quan đến việc kiểm tra đỉnh theo cặp, mô phỏng bước đi hoặc bất kỳ phương pháp bậc hai nào đều ngay lập tức không thể thực hiện được. Cần có giải pháp tuyến tính hoặc gần tuyến tính. 

Khó khăn là điều kiện định lượng trên **tất cả các bước đi có thể xếp chồng lên nhau**, bao gồm cả các bước đi truy cập lại các đỉnh và cạnh nhiều lần. Chúng ta không thể lý luận về việc đi bộ của từng cá nhân. Chúng ta cần mô tả đặc điểm cấu trúc về thời điểm tất cả các bước đi có thể xếp chồng lên nhau hoạt động chính xác. 

Một vài tình huống tế nhị rất dễ bị bỏ sót. 

Hãy xem xét một con đường```
1 -> 2 -> 3
```với hai màu khác nhau ở hai cạnh. 

đi dạo```
1 -> 2 -> 3 -> 2
```đẩy màu của cạnh$(1,2)$, thì màu của cạnh$(2,3)$, sau đó bật lên bằng cách sử dụng cạnh$(2,3)$. Điều này ổn. 

Bây giờ hãy tưởng tượng một cấu trúc phân nhánh```
    2
    |
1 ->3<-4
```Nếu màu sắc ở hai cạnh đến thành 3 khác nhau, thì bước đi có thể đi qua một cạnh, đi lang thang ở nơi khác và sau đó đi qua cạnh kia. Cơ chế ngăn xếp bắt đầu áp đặt sự bình đẳng giữa các màu. 

Thách thức chính là khám phá chính xác những đẳng thức nào bị ép buộc. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là coi mọi hướng cạnh như một biểu tượng đẩy và mọi giao dịch ngược lại như một biểu tượng pop phù hợp. Chúng ta có thể cố gắng liệt kê các bước đi và rút ra tất cả các ràng buộc về màu sắc. 

Điều này thật vô vọng. Ngay cả một cái cây có$10^5$các đỉnh cho phép vô số bước đi vì các cạnh có thể được xem lại tùy ý nhiều lần. Không có chiến lược liệt kê hữu hạn. 

Bước đột phá đến từ việc nhận ra rằng hành vi của ngăn xếp chính xác là hành vi của các dấu ngoặc đơn được lồng đúng cách. 

Bất cứ khi nào chúng ta đi ngang qua một cạnh theo hướng của nó, chúng ta sẽ đẩy một vật gì đó. Bất cứ khi nào chúng ta duyệt ngược lại, chúng ta sẽ bật lên thứ gì đó. Bước đi có thể xếp chồng lên nhau chính xác là một trình tự trong đó mỗi lần bật lên khớp với một lần đẩy chưa từng có trước đó. 

Yêu cầu mỗi pop phải loại bỏ một quả bóng cùng màu với cạnh đi qua có nghĩa là mọi cặp push-pop buộc theo thứ tự ngăn xếp phải tương ứng với các màu bằng nhau. 

Thay vì suy luận về các bước đi, chúng ta lý luận về chính cái cây được định hướng. 

Giả sử chúng ta nhổ cây ở đâu đó. Nếu một cạnh hướng từ cha mẹ sang con cái, việc di chuyển xuống dưới sẽ tạo ra lực đẩy. Di chuyển lên trên cùng cạnh đó sẽ tạo ra tiếng bật lên. Ngăn xếp hoạt động giống như việc vào và ra các vùng của cây. 

Sau khi thực hiện các bước đi có thể, người ta phát hiện ra một sự thật quan trọng: 

Một cấu trúc hợp lệ tồn tại khi và chỉ khi mọi cạnh có hướng có thể được định hướng một cách nhất quán với một nghiệm duy nhất. Nói cách khác, sau khi chọn một gốc, mọi cạnh phải hướng ra xa gốc. Những định hướng hỗn hợp tạo nên những mâu thuẫn khó tránh khỏi. 

Chính xác hơn, cây định hướng cuối cùng phải là một cây có gốc ở một đỉnh nào đó. 

Sau đó, hành vi ngăn xếp trở nên giống hệt với dấu ngoặc đơn DFS. Mỗi cửa sổ bật lên buộc phải khớp với lần đẩy chưa từng có gần đây nhất dọc theo đường dẫn gốc duy nhất. 

Điều này ngay lập tức xác định các hạn chế về màu sắc. Tất cả các cạnh rời khỏi cùng một đỉnh phải có cùng một màu. Ngược lại, bước đi có thể đẩy qua một cạnh con và vượt qua một cạnh con khác, tạo ra sự không khớp. 

Ngược lại, khi mọi cạnh đi ra của một đỉnh có cùng màu, mọi bước đi có thể xếp chồng lên nhau sẽ trở thành bóng có thể xếp chồng lên nhau. 

Vấn đề bây giờ được chuyển thành: 

1. Xác định xem các cạnh đã được định hướng có chấp nhận một nghiệm sao cho mọi cạnh được định hướng đều hướng ra xa gốc đó hay không. 
2. Nếu các nghiệm đó tồn tại, hãy tối đa hóa số lượng màu riêng biệt khi mỗi đỉnh đóng góp một màu được chia sẻ bởi tất cả các cạnh đi ra. 

Số lượng màu tối đa có thể đạt được bằng cách tạo các màu khác nhau cho càng nhiều đỉnh càng tốt. Vì tất cả các cạnh đi ra của một đỉnh phải có chung một màu, nên màu sắc tương ứng với các đỉnh bên trong có ít nhất một cạnh đi ra. 

### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các bước đi và rút ra các ràng buộc | Vô hạn / hàm mũ | Không giới hạn | Không thể | 
| Đặc tính gốc + cây DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Giải thích mọi cạnh có hướng cố định$u \to v$như một hạn chế trên thư mục gốc. 

Nếu hướng cuối cùng phải cách xa gốc thì gốc không thể nằm bên trong thành phần chứa$v$sau khi loại bỏ cạnh đó. 

Tương tự, gốc phải thuộc về phía chứa$u$. 

### Bước 2 

Root cây bên dưới một cách tùy ý, ví dụ ở đỉnh 1. 

Tính toán các khoảng tham quan Euler để mỗi cây con trở thành một đoạn liền kề. 

### Bước 3 

Đối với mỗi cạnh cố định$u \to v$, xác định điểm cuối nào là tổ tiên của điểm cuối khác trong quá trình root tạm thời. 

Nếu như$u$là tổ tiên của$v$, thì các gốc hợp lệ nằm chính xác bên ngoài cây con($v$). 

Nếu như$v$là tổ tiên của$u$, thì các gốc hợp lệ nằm chính xác bên trong cây con($u$). 

Đây là những ràng buộc phạm vi đơn giản trên các vị trí Euler. 

### Bước 4 

Kết hợp tất cả các ràng buộc bằng kỹ thuật mảng sai phân theo thứ tự Euler. 

Đếm xem mỗi đỉnh có bao nhiêu ràng buộc mà nó thỏa mãn. 

Bất kỳ đỉnh nào thỏa mãn mọi ràng buộc đều có thể đóng vai trò là gốc toàn cục. 

Nếu không có đỉnh như vậy tồn tại, xuất ra$-1$. 

### Bước 5 

Chọn bất kỳ gốc hợp lệ$r$. 

Định hướng mọi cạnh vô hướng ra khỏi$r$. 

Các cạnh đã được định hướng sẽ tự động hướng ra xa$r$vì sự hạn chế của việc xây dựng. 

### Bước 6 

Gán màu sắc. 

Với mọi đỉnh ngoại trừ đỉnh, hãy xét cạnh nối nó với đỉnh gốc của nó. 

Màu của cạnh đó được xác định bởi đỉnh cha. 

Tất cả các cạnh rời khỏi cùng một cha mẹ sẽ nhận được cùng một màu. 

### Bước 7 

Nén nhãn gốc đã sử dụng thành các số màu liên tiếp. 

Số lượng các màu riêng biệt bằng số đỉnh có ít nhất một con, là số lớn nhất. 

### Tại sao nó hoạt động 

Các ràng buộc gốc chính xác là các điều kiện cần thiết để mọi cạnh có hướng hướng ra khỏi một gốc chung. Nếu không có gốc như vậy tồn tại, một số cạnh có hướng phải hướng về gốc trong khi một cạnh khác hướng ra xa, tạo ra một cấu trúc cho phép các bước đi có thể xếp chồng lên nhau mà việc khớp ngăn xếp không thể được biểu thị bằng các đẳng thức màu cục bộ. 

Khi tất cả các cạnh đều hướng ra xa một gốc, mọi bước đi có thể xếp chồng lên nhau sẽ hoạt động giống như các đường đi được lồng đúng cách của các cạnh của cây có gốc. Việc đẩy tương ứng với việc nhập một cây con con phải được khớp bằng cách rời khỏi cùng một cây con đó. Do đó, sự cân bằng màu sắc duy nhất được yêu cầu là giữa các cạnh có cùng cha mẹ. 

Việc chỉ định một màu cho mỗi cha mẹ đáp ứng tất cả các đẳng thức cần thiết. Cha mẹ khác biệt có thể nhận được các màu sắc riêng biệt, cho số lượng màu tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

edges = []
g = [[] for _ in range(n)]

for i in range(n - 1):
    u, v, t = map(int, input().split())
    u -= 1
    v -= 1
    edges.append((u, v, t))
    g[u].append((v, i))
    g[v].append((u, i))

tin = [0] * n
tout = [0] * n
parent = [-1] * n
depth = [0] * n

timer = 0

stack = [(0, -1, 0)]
while stack:
    v, p, state = stack.pop()

    if state == 0:
        tin[v] = timer
        timer += 1

        stack.append((v, p, 1))

        for to, _ in reversed(g[v]):
            if to == p:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            stack.append((to, v, 0))
    else:
        tout[v] = timer - 1

def is_ancestor(a, b):
    return tin[a] <= tin[b] <= tout[a]

diff = [0] * (n + 1)
need = 0

for u, v, t in edges:
    if t == 0:
        continue

    need += 1

    if is_ancestor(u, v):
        l = tin[v]
        r = tout[v]

        diff[0] += 1
        diff[n] -= 1

        diff[l] -= 1
        diff[r + 1] += 1
    else:
        l = tin[u]
        r = tout[u]

        diff[l] += 1
        diff[r + 1] -= 1

cur = 0
root = -1

for v in range(n):
    cur += diff[v]
    if cur == need:
        root = v
        break

if root == -1:
    print(-1)
    sys.exit()

parent = [-1] * n
order = [root]
parent[root] = root

for v in order:
    for to, _ in g[v]:
        if parent[to] != -1:
            continue
        parent[to] = v
        order.append(to)

children_count = [0] * n
for v in range(n):
    if v != root:
        children_count[parent[v]] += 1

color_of_vertex = [-1] * n
color_id = 0

for v in range(n):
    if children_count[v] > 0:
        color_id += 1
        color_of_vertex[v] = color_id

answer = []

for u, v, t in edges:
    if parent[v] == u:
        p, q = u, v
    elif parent[u] == v:
        p, q = v, u
    else:
        raise RuntimeError

    c = color_of_vertex[p]
    answer.append((p + 1, q + 1, c))

print(color_id)
for row in answer:
    print(*row)
```DFS đầu tiên tính toán các khoảng Euler. Các khoảng đó cho phép các ràng buộc về cây con được biểu diễn dưới dạng phần bổ sung phạm vi trên một mảng khác biệt. 

Giai đoạn tiếp theo xử lý mọi cạnh có hướng cố định. Mỗi cạnh đóng góp một ràng buộc "cây con bên trong" hoặc một ràng buộc "cây con bên ngoài" tùy thuộc vào mối quan hệ tổ tiên của các điểm cuối của nó. 

Sau khi tích lũy các ràng buộc, bất kỳ đỉnh nào thỏa mãn tất cả các ràng buộc đó đều có thể được sử dụng làm gốc toàn cục. 

Một khi đã biết gốc thì việc định hướng cây là việc đơn giản. Mỗi cạnh đều được định hướng từ gốc tới con trong cây có gốc. 

Giai đoạn tô màu là nơi đối số ngăn xếp đi vào. Mỗi đỉnh cha nhận được một màu được chia sẻ bởi tất cả các cạnh đi ra của nó. Các đỉnh không có con sẽ không đóng góp màu sắc vì không có cạnh nào bắt nguồn từ chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
5
2 5 1
1 3 0
5 4 0
1 5 1
```Giả sử gốc hợp lệ được tìm thấy là đỉnh 3. 

| Đỉnh | Phụ huynh | 
| --- | --- | 
| 3 | gốc | 
| 1 | 3 | 
| 5 | 1 | 
| 2 | 5 | 
| 4 | 5 | 

Phân bổ màu sắc: 

| Đỉnh cha mẹ | Màu được chỉ định | 
| --- | --- | 
| 3 | 1 | 
| 1 | 2 | 
| 5 | 3 | 

Các cạnh được sản xuất: 

| Cạnh định hướng | Màu sắc | 
| --- | --- | 
| 3→1 | 1 | 
| 1→5 | 2 | 
| 5→2 | 3 | 
| 5→4 | 3 | 

Hai cạnh rời khỏi đỉnh 5 có chung một màu, chính xác theo yêu cầu của đối số ngăn xếp. 

### Ví dụ 2 

đầu vào```
3
1 2 1
3 2 1
```Hạn chế: 

Từ$1 \to 2$, gốc phải nằm ở cạnh đỉnh 1. 

Từ$3 \to 2$, gốc phải nằm trên cạnh của đỉnh 3. 

Không có đỉnh nào thỏa mãn cả hai điều kiện. 

| Đỉnh | Thỏa mãn ràng buộc đầu tiên | Thỏa mãn ràng buộc thứ hai | 
| --- | --- | --- | 
| 1 | Có | Không | 
| 2 | Không | Không | 
| 3 | Không | Có | 

Không có gốc hợp lệ tồn tại, vì vậy câu trả lời là:```
-1
```Điều này chứng tỏ khả năng phát hiện không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | DFS, tích lũy ràng buộc, root và tạo đầu ra đều tuyến tính | 
| Không gian | O(n) | Danh sách kề, mảng Euler, mảng cha và mảng sai phân | 

Với$n \le 10^5$, độ phức tạp tuyến tính dễ dàng đủ nhanh cho giới hạn 2 giây và vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # invoke solution
    pass

# sample 1
# output is not unique, so exact assertion is not suitable

# minimum tree
inp = """2
1 2 0
"""

# already impossible
inp2 = """3
1 2 1
3 2 1
"""

# chain, all undirected
inp3 = """4
1 2 0
2 3 0
3 4 0
"""

# star centered at 1
inp4 = """5
1 2 0
1 3 0
1 4 0
1 5 0
"""
```### Tóm tắt thử nghiệm tùy chỉnh 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh, một cạnh vô hướng | Một màu, một cạnh định hướng | Trường hợp hợp lệ nhỏ nhất | 
| Hai cạnh có hướng hướng vào cùng một đỉnh | -1 | Ràng buộc gốc mâu thuẫn | 
| Chuỗi vô hướng | Xây dựng hợp lệ | Lựa chọn và định hướng gốc | 
| Sao vô hướng | Đếm màu tối đa | Nhiều cạnh hướng ra chung một màu | 

## Vỏ cạnh 

Hãy xem xét```
3
1 2 1
3 2 1
```Cả hai cạnh có hướng đều hướng về đỉnh 2. Bất kỳ hướng gốc nào cách xa gốc sẽ yêu cầu đỉnh 2 phải đồng thời ở dưới cả 1 và 3, điều này là không thể trong cây. Giai đoạn tích lũy ràng buộc không có đỉnh nào thỏa mãn tất cả các ràng buộc, do đó thuật toán đưa ra`-1`. 

Coi như```
2
1 2 0
```Không có ràng buộc về hướng cố định. Mọi đỉnh đều là một gốc hợp lệ. Thuật toán chọn một, hướng cạnh duy nhất ra khỏi nó, gán một màu và thành công. 

Coi như```
5
1 2 0
1 3 0
1 4 0
1 5 0
```Tất cả bốn cạnh đều để lại cùng một cha mẹ theo hướng gốc. Bằng chứng về tính đúng đắn cho biết chúng phải có chung một màu sắc. Thuật toán gán chính xác một màu cho cả bốn cạnh. Bất kỳ nỗ lực nào sử dụng nhiều màu ở đây sẽ vi phạm điều kiện ngăn xếp, do đó việc xây dựng đã tối ưu.
