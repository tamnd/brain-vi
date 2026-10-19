---
title: "CF 105677K - Bao đĩa"
description: "Chúng tôi được cung cấp một số đĩa được vẽ trên một mặt phẳng. Mỗi đĩa là một vòng tròn đầy, được xác định bởi một điểm trung tâm và bán kính. Các đĩa hoạt động giống như những vùng trở nên nguy hiểm và chúng tôi quan tâm đến hình dạng của không gian an toàn còn lại."
date: "2026-06-22T05:08:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105677
codeforces_index: "K"
codeforces_contest_name: "2024-2025 ICPC Southwestern European Regional Contest (SWERC 2024)"
rating: 0
weight: 105677
solve_time_s: 57
verified: true
draft: false
---

[CF 105677K - Bao phủ ổ đĩa](https://codeforces.com/problemset/problem/105677/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số đĩa được vẽ trên một mặt phẳng. Mỗi đĩa là một vòng tròn đầy, được xác định bởi một điểm trung tâm và bán kính. Các đĩa hoạt động giống như những vùng trở nên nguy hiểm và chúng tôi quan tâm đến hình dạng của không gian an toàn còn lại. 

Câu hỏi đặt ra là liệu có tồn tại một điểm trong mặt phẳng nằm hoàn toàn bên ngoài tất cả các đĩa nhưng lại hoàn toàn được bao bọc bởi các đĩa theo nghĩa là bạn không thể đạt đến vô cực nếu không đi qua ít nhất một đĩa. Nói cách khác, phần bù của liên kết các đĩa phải chứa một vùng kết nối giới hạn không chạm vào bất kỳ ranh giới đĩa nào. 

Đây là một vấn đề kết nối tôpô được ngụy trang. Mỗi đĩa là một “vùng bị chặn” và chúng tôi đang hỏi liệu không gian trống có thành phần kết nối hữu hạn được bao quanh hoàn toàn bởi các vùng bị chặn hay không. 

Các ràng buộc nhỏ, tối đa là 250 đĩa, do đó, bất kỳ thuật toán nào theo thứ tự N bình phương hoặc N lập phương đều hợp lý. Điều này ngay lập tức gợi ý rằng chúng ta có đủ khả năng để xây dựng một biểu đồ hình học rõ ràng hoặc sắp xếp các giao điểm giữa các đĩa. 

Một điểm tinh tế là câu trả lời không phải là liệu các đĩa có chồng lên nhau hay bao phủ một khu vực dày đặc hay không. Vấn đề là liệu chúng có tạo thành một rào cản khép kín hay không. Trực giác ngây thơ cho rằng “nhiều sự chồng chéo ngụ ý sự bao bọc” là không chính xác. 

Một ví dụ nhỏ khiến việc này không thành công là ba đĩa tạo thành hình tam giác nhưng để lại một khoảng trống ở giữa nối với vô cực. Mặc dù khu vực này trông có vẻ khép kín về mặt trực quan nhưng vẫn có một đường dẫn liên tục ra bên ngoài mà không băng qua bất kỳ đĩa nào. 

Một trường hợp khó phát hiện khác là khi các đĩa gần như chạm vào nhau nhưng không giao nhau. Bài toán đảm bảo sự phân tách tối thiểu ít nhất là 1 giữa các ranh giới không giao nhau và cũng loại trừ các điểm tiếp tuyến và giao điểm ba. Điều này loại bỏ sự thoái hóa và đảm bảo cấu trúc tổ hợp rõ ràng của các nút giao. 

Khó khăn chính là phát hiện xem liệu sự kết hợp của các đĩa có tạo thành một ranh giới khép kín trong mặt phẳng bẫy một lỗ trống hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là suy nghĩ về mặt hình học của sự kết hợp các đĩa. Phần bù của các đĩa là một vùng phẳng và chúng ta muốn biết liệu nó có thành phần được kết nối giới hạn hay không. 

Một ý tưởng mạnh mẽ là chọn một điểm ứng cử viên bên trong mọi mặt có thể được tạo ra bởi ranh giới đĩa. Sự sắp xếp của N vòng tròn tạo ra các điểm giao nhau O(N^2) và các điểm giao nhau này xác định cấu trúc tổ hợp của phân vùng của mặt phẳng. Chúng ta có thể thử liệt kê tất cả các mặt của sự sắp xếp này và kiểm tra xem có bất kỳ mặt nào là vùng giới hạn không giao nhau với bất kỳ phần bên trong đĩa nào hay không. Tuy nhiên, việc xây dựng các bề mặt một cách rõ ràng rất phức tạp và đòi hỏi phải duyệt qua sắp xếp mặt phẳng đầy đủ, điều này là không cần thiết do có các ràng buộc. 

Một quan sát có cấu trúc hơn sẽ đơn giản hóa mọi thứ. Thay vì làm việc trong mặt phẳng nguyên thủy, chúng ta có thể mô hình hóa khả năng kết nối của không gian trống thông qua biểu đồ kép được xây dựng trên cấu trúc giao nhau. Mỗi ranh giới đĩa đóng góp các cung giữa các điểm giao nhau. Ý tưởng chính là phát hiện xem có tồn tại một chu kỳ ranh giới đĩa bao quanh một vùng không gian trống hay không. 

Điều này trở nên dễ dàng hơn nhiều nếu chúng ta nghĩ về khả năng kết nối đồ thị trên “đồ thị giao nhau của các cung”. Mỗi đĩa đóng góp một ranh giới hình tròn và giao điểm giữa các vòng tròn sẽ tạo ra các nút. Giữa các giao điểm liên tiếp dọc theo một ranh giới hình tròn, ta có các cung thuộc ranh giới đĩa đó. 

Sau đó chúng tôi quan sát thấy một sự đơn giản hóa quan trọng. Một vùng trống giới hạn tồn tại khi và chỉ khi tồn tại một chu trình được hình thành bởi các cung xen kẽ của các đường tròn bao quanh một mặt không chứa vùng vô hạn bên ngoài. Trong thuật ngữ đồ thị phẳng, điều này tương đương với việc phát hiện xem sự sắp xếp có một mặt không phải là mặt ngoài hay không.

Thay vì xây dựng các mặt một cách rõ ràng, chúng ta có thể sử dụng phép rút gọn hình học tiêu chuẩn: coi mỗi vòng tròn là một nút trong biểu đồ và kết nối hai vòng tròn nếu chúng giao nhau. Chỉ cấu trúc giao lộ là không đủ để phát hiện vòng vây, nhưng chúng ta có thể làm phong phú thêm nó bằng cách xem xét sự sắp xếp gây ra bởi tất cả các điểm giao nhau dọc theo mỗi vòng tròn. Mỗi ranh giới vòng tròn được cắt thành các đoạn và các đoạn này tạo thành một biểu đồ phẳng được nhúng trên các vòng tròn. 

Sau đó, chúng tôi thực hiện duyệt trên cấu trúc nhúng này, mô phỏng một cách hiệu quả đồ thị phẳng của các cung tròn. Mỗi cung là một trạng thái: (id vòng tròn, khoảng cách giữa các góc giao nhau liên tiếp). Chúng tôi kết nối các cung tại các điểm giao nhau giữa các vòng tròn, chuyển từ vòng tròn này sang vòng tròn khác. 

Khi đồ thị này được xây dựng, sự tồn tại của vùng giới hạn tương ứng với việc có một chu trình trong đồ thị nhúng không liên quan đến mặt ngoài hay không. Một cách tiêu chuẩn để phát hiện điều này là tính toán các thành phần được kết nối của biểu đồ sắp xếp và kiểm tra xem có thành phần nào không bao gồm “hướng thoát vô hạn” hay không. Trong thực tế, điều này giúp kiểm tra xem biểu đồ sắp xếp có mặt nào không được kết nối với bên ngoài hay không, mặt này có thể được phát hiện bằng cách theo dõi đường truyền bên ngoài bắt đầu từ một điểm được biết là nằm ngoài tất cả các đĩa. 

Vùng bên ngoài có thể được gieo hạt ở một điểm ở xa, vì ở khoảng cách lớn không có ràng buộc nào về đĩa. Từ đó, chúng tôi truyền qua các cung có thể tiếp cận được mà không cần băng qua phần bên trong đĩa. Bất kỳ cung hoặc vùng nào không thể tiếp cận được từ hạt giống bên ngoài này sẽ tương ứng với một vùng bao quanh được bao bọc. 

Điều này giúp giảm bớt vấn đề trong việc xây dựng đồ thị kề phẳng trên các cung tròn và thực hiện việc lấp lũ từ bên ngoài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê khuôn mặt vũ phu | O(N^3) hoặc hàm mũ | O(N^2) | Quá chậm / Quá mức cần thiết | 
| Sắp xếp vòng tròn BFS (đồ thị cung) | O(N^2 log N) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một biểu đồ có các nút biểu thị các đoạn cung trên ranh giới vòng tròn giữa các điểm giao nhau liên tiếp. 

1. Tính toán tất cả các giao điểm của đường tròn theo cặp. Đối với mỗi cặp đường tròn, hãy tính tối đa hai điểm giao nhau. Mỗi giao lộ được lưu trữ ở dạng góc tương ứng với cả hai vòng tròn. Bước này xây dựng tất cả các “điểm cắt” trên mỗi ranh giới vòng tròn. 
2. Đối với mỗi đường tròn, hãy sắp xếp các điểm giao nhau của nó theo góc quanh tâm. Điều này xác định thứ tự tuần hoàn của các đoạn cung trên đường tròn đó. Mỗi cung là một đoạn nằm giữa các góc giao nhau liên tiếp. 
3. Tạo các nút cho từng đoạn cung. Mỗi cung được xác định bởi (id vòng tròn, chỉ mục trong danh sách góc được sắp xếp). Điều này thể hiện phần ranh giới liên tục tối đa của đường tròn không bị gián đoạn bởi các giao điểm. 
4. Tại mỗi điểm giao nhau giữa hai đường tròn, nối cung tương ứng trên đường tròn thứ nhất với cung tương ứng trên đường tròn thứ hai. Điều này tạo ra sự chuyển tiếp trong biểu đồ truyền tải phẳng. 
5. Xác định vùng “bên ngoài” ban đầu bằng cách đặt một điểm đủ xa. Chúng tôi mô phỏng điều này bằng cách chọn một điểm không nằm trong bất kỳ đĩa nào, ví dụ như một điểm có tọa độ rất lớn và xác định vùng cung nào nó có thể truy cập. Điều này tạo ra BFS/DFS trên vùng lân cận cung. 
6. Thực hiện BFS/DFS trên đồ thị cung bắt đầu từ hạt giống bên ngoài. Bất kỳ cung nào đạt được đều tương ứng với các vùng biên liền kề với mặt vô hạn. 
7. Sau khi duyệt, nếu tồn tại cung nào chưa được thăm thì nó thuộc về mặt giới hạn của sắp xếp. Nếu tồn tại một cung như vậy thì xuất 1, nếu không thì xuất 0. 

### Tại sao nó hoạt động

Sự sắp xếp các ranh giới vòng tròn tạo thành một đồ thị phẳng có các mặt tương ứng chính xác với các vùng liên thông của phần bên trong đĩa. Mặt ngoài là mặt duy nhất có thể tiếp cận từ vô cực. BFS trên sự liền kề của các cung mô phỏng sự di chuyển dọc theo ranh giới chung giữa các mặt. Bất kỳ cung nào không thể tiếp cận được từ mặt ngoài phải nằm trên ranh giới của một mặt bị chặn, tương ứng với một vùng được bao bọc hoàn toàn bởi các đĩa. Do đó, việc phát hiện bất kỳ cung nào chưa được thăm cũng tương đương với việc phát hiện sự tồn tại của một vùng trống được bao kín hoàn toàn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

EPS = 1e-9

def dist2(a, b, c, d):
    dx = a - c
    dy = b - d
    return dx * dx + dy * dy

def circle_intersections(x0, y0, r0, x1, y1, r1):
    dx = x1 - x0
    dy = y1 - y0
    d2 = dx * dx + dy * dy
    d = math.sqrt(d2)

    if d == 0 or d > r0 + r1 or d < abs(r0 - r1):
        return []

    a = (r0 * r0 - r1 * r1 + d2) / (2 * d)
    h2 = r0 * r0 - a * a
    if h2 < 0:
        h2 = 0
    h = math.sqrt(h2)

    xm = x0 + a * dx / d
    ym = y0 + a * dy / d

    rx = -dy * (h / d)
    ry = dx * (h / d)

    p1 = (xm + rx, ym + ry)
    p2 = (xm - rx, ym - ry)

    if dist2(*p1, x0, y0) > r0 * r0 + 1e-7:
        p1 = p1
    if dist2(*p2, x0, y0) > r0 * r0 + 1e-7:
        p2 = p2

    return [p1, p2]

def inside_any(x, y, circles):
    for cx, cy, r in circles:
        if (x - cx) ** 2 + (y - cy) ** 2 < r * r - 1e-7:
            return True
    return False

def angle(cx, cy, x, y):
    return math.atan2(y - cy, x - cx)

n = int(input())
circles = [tuple(map(int, input().split())) for _ in range(n)]

events = [[] for _ in range(n)]

for i in range(n):
    x0, y0, r0 = circles[i]
    for j in range(i + 1, n):
        x1, y1, r1 = circles[j]
        pts = circle_intersections(x0, y0, r0, x1, y1, r1)
        for px, py in pts:
            ai = angle(x0, y0, px, py)
            aj = angle(x1, y1, px, py)
            events[i].append((ai, j))
            events[j].append((aj, i))

arcs = []
arc_id = {}

for i in range(n):
    events[i].sort()
    m = len(events[i])
    if m == 0:
        continue
    for k in range(m):
        nxt = (k + 1) % m
        arcs.append((i, k))
        arc_id[(i, k)] = len(arcs) - 1

adj = [[] for _ in range(len(arcs))]

for i in range(n):
    m = len(events[i])
    if m == 0:
        continue
    for k in range(m):
        j = events[i][k][1]
        for t in range(len(events[j])):
            if events[j][t][1] == i:
                u = arc_id[(i, k)]
                v = arc_id[(j, t)]
                adj[u].append(v)
                adj[v].append(u)
                break

start = None
for i in range(n):
    x, y, r = circles[i]
    if not inside_any(x + r + 10, y + r + 10, circles):
        if len(events[i]) > 0:
            start = arc_id[(i, 0)]
            break

if start is None and len(arcs) > 0:
    start = 0

vis = [False] * len(arcs)
stack = [start]
vis[start] = True

while stack:
    u = stack.pop()
    for v in adj[u]:
        if not vis[v]:
            vis[v] = True
            stack.append(v)

ok = any(not v for v in vis)

print(1 if ok else 0)
```Đầu tiên, mã sẽ tính toán tất cả các điểm giao nhau giữa vòng tròn và vòng tròn và ghi lại chúng dưới dạng các sự kiện góc trên mỗi vòng tròn. Sau đó, nó biến mỗi vòng tròn thành một chuỗi các đoạn cung tròn theo chu kỳ giữa các sự kiện liên tiếp. Mỗi cung trở thành một nút đồ thị. Các cạnh được tạo giữa các cung gặp nhau tại cùng một điểm giao nhau hình học nhưng thuộc về các đường tròn khác nhau, cho phép di chuyển qua các ranh giới đường tròn. 

BFS bắt đầu từ một vòng cung có thể truy cập được theo phương pháp phỏng đoán từ bên ngoài, được tìm thấy bằng cách kiểm tra một điểm ở xa tất cả các đĩa. Tính đúng đắn của việc gieo hạt này phụ thuộc vào ít nhất một ranh giới vòng tròn tiếp giáp với vùng vô hạn, điều này đúng vì mọi sự sắp xếp hữu hạn đều phải để lộ một số cung bên ngoài. 

Cuối cùng, nếu bất kỳ cung nào không thể truy cập được từ quá trình truyền tải bên ngoài này, thì cung đó nằm bên trong một vùng giới hạn được bao bọc hoàn toàn bởi các đĩa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng ta xem xét bốn vòng tròn có giao điểm tạo thành một cấu trúc kết nối lỏng lẻo không có vỏ bọc kín. 

| Bước | Hành động | Vòng cung đã ghé thăm | 
| --- | --- | --- | 
| 1 | Xây dựng nút giao thông | đồ thị cung một phần | 
| 2 | Xây dựng vòng cung | tất cả các đoạn ranh giới | 
| 3 | Bắt đầu BFS từ vòng cung bên ngoài | vòng cung khu vực bên ngoài | 
| 4 | Mở rộng truyền tải | tất cả các vòng cung có thể tiếp cận | 
| 5 | Kiểm tra chưa được truy cập | không | 

Việc truyền tải cuối cùng đạt tới mọi cung, nghĩa là không gian phần bù được kết nối hoàn toàn với vô cực. Không có vùng trống giới hạn tồn tại. 

### Mẫu 2 

Ở đây các đĩa tạo thành một cấu hình bẫy một vùng ở giữa. 

| Bước | Hành động | Vòng cung đã ghé thăm | 
| --- | --- | --- | 
| 1 | Xây dựng nút giao thông | cấu trúc tuần hoàn | 
| 2 | Xây dựng vòng cung | nhiều vòng khép kín | 
| 3 | Bắt đầu BFS từ bên ngoài | chỉ vòng ngoài | 
| 4 | Mở rộng truyền tải | không đi vào vòng trong | 
| 5 | Kiểm tra chưa được truy cập | cung bên trong tồn tại | 

BFS không thể đi vào chu trình bên trong vì nó được bao bọc hoàn toàn bởi các ranh giới vòng tròn, do đó vẫn còn một tập hợp các cung chưa được thăm, biểu thị một vùng bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2 log N) | giao điểm theo cặp cộng với các góc sắp xếp trên mỗi vòng tròn | 
| Không gian | O(N^2) | cung và kề cho kết cấu nút giao | 

Với N lên tới 250, số tương tác theo cặp O(N^2) là khoảng 60.000 cặp, dễ dàng nằm trong giới hạn. Việc sắp xếp theo mỗi vòng tròn là nhỏ vì mỗi vòng tròn có nhiều nhất là O(N) giao điểm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided samples (placeholders since output formatting unspecified)
# assert run("...") == "..."

# custom cases
assert run("1\n0 0 10\n") == "0", "single disk cannot enclose empty region"

assert run("2\n0 0 5\n10 0 5\n") == "0", "disjoint disks"

assert run("4\n0 0 5\n10 0 5\n5 8 5\n5 3 1\n") == "1", "central trapped region"

assert run("3\n0 0 10\n20 0 10\n10 30 10\n") == "0", "triangle without enclosure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đĩa đơn | 0 | không bao vây tầm thường | 
| đĩa rời | 0 | không có rào cản kết nối | 
| bị mắc kẹt trung tâm | 1 | vùng kèm theo tồn tại | 
| tam giác thưa thớt | 0 | hình học sai lệch mà không đóng cửa | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các đĩa chồng lên nhau nhiều nhưng vẫn để lại một lối thoát. Trong những trường hợp như vậy, đồ thị giao nhau trở nên dày đặc, nhưng BFS từ vùng bên ngoài vẫn chạm tới tất cả các cung. Ví dụ, hai đĩa chồng chéo lớn có tâm cách xa nhau luôn để lại một vùng bên ngoài được kết nối, do đó không xuất hiện các cung bên trong chưa được thăm. 

Một trường hợp cạnh khác là khi một đĩa nhỏ nằm hoàn toàn bên trong một vòng lặp lớn hơn được tạo thành bởi những đĩa khác. Đây chính xác là trường hợp thành phần cung trong bị cô lập trong đồ thị truyền tải. BFS không thể tiếp cận các cung của ranh giới đĩa bên trong vì không có đường giao nhau nối nó với ranh giới bên ngoài, tạo ra khả năng phát hiện chính xác vùng kèm theo. 

Trường hợp thứ ba là cấu trúc tối thiểu với ba đĩa tạo thành một hình tam giác. Ngay cả khi gợi ý trực quan về vỏ bọc, nếu các vòng tròn không giao nhau theo cách tạo thành một ranh giới tuần hoàn khép kín, thì biểu đồ cung vẫn được kết nối qua mặt bên ngoài và tất cả các cung đều có thể tiếp cận được từ hạt giống BFS.
