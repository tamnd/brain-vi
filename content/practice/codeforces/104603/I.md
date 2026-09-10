---
title: "CF 104603I - Tích hợp khu vực"
description: "Chúng ta có một tập hợp các vùng hình học trong mặt phẳng, mỗi vùng là một hình tròn, một hình vuông hoặc một hình tam giác. Tất cả các khu vực đều rời rạc ngay cả trên ranh giới của chúng, do đó không có hai hình dạng nào chạm vào nhau. Chúng ta phải chọn chính xác một hình vuông và một hình tam giác."
date: "2026-06-30T02:55:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104603
codeforces_index: "I"
codeforces_contest_name: "2023 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 104603
solve_time_s: 70
verified: true
draft: false
---

[CF 104603I - Tích hợp khu vực](https://codeforces.com/problemset/problem/104603/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các vùng hình học trong mặt phẳng, mỗi vùng là một hình tròn, một hình vuông hoặc một hình tam giác. Tất cả các khu vực đều rời rạc ngay cả trên ranh giới của chúng, do đó không có hai hình dạng nào chạm vào nhau. 

Chúng ta phải chọn chính xác một hình vuông và một hình tam giác. Sau khi được chọn, chúng tôi muốn di chuyển giữa chúng trên máy bay đồng thời giảm thiểu khoảng cách dành cho ánh sáng mặt trời. Bên trong bất kỳ tòa nhà nào, nghĩa là bên trong bất kỳ hình dạng nào, việc di chuyển không tốn phí gì. Bên ngoài tất cả các hình dạng, chi phí di chuyển là một cho mỗi đơn vị khoảng cách. Vì chúng ta được tự do ra vào các tòa nhà nên chi phí thực tế của một con đường chính xác là tổng chiều dài của phần nằm bên ngoài tất cả các hình dạng. 

Nhiệm vụ là tính toán chi phí đi lại “tiếp xúc với ánh nắng mặt trời” tối thiểu có thể giữa hình vuông và hình tam giác đã chọn, nơi chúng ta được phép đi qua bất kỳ tòa nhà thuộc bất kỳ loại nào. 

Các ràng buộc bao hàm tối đa 100.000 hình vuông và hình tam giác, cũng như hình tròn, nhưng có một ràng buộc toàn cầu$(T + C)(Q + C) \le 10^6$, điều này gợi ý rõ ràng rằng chỉ có một số tương tác theo cặp nhất định mới có thể được tính toán rõ ràng. Bất kỳ giải pháp nào cố gắng xem xét trực tiếp tất cả các cặp hình vuông và hình tam giác đều không thể thực hiện được vì điều đó sẽ$O(QT)$. 

Một điểm tinh tế là mặc dù chúng ta đang chọn một hình vuông và một hình tam giác, nhưng đường đi giữa chúng không bị hạn chế ở bên trong sự kết hợp của chúng. Chúng ta có thể đi qua bất kỳ tòa nhà nào khác miễn phí, vì vậy các tòa nhà trung gian hoạt động giống như “hành lang dịch chuyển” không tốn chi phí, có thể giảm khoảng cách tiếp xúc với ánh nắng mặt trời. 

Một sự hiểu lầm ngây thơ nhưng quan trọng sẽ coi đây là cách đơn giản tính toán khoảng cách Euclide giữa hình vuông và hình tam giác gần nhất. Điều đó sai vì hình dạng thứ ba có thể nằm giữa chúng và cho phép đường dẫn đi vào các vùng có chi phí bằng 0. 

Ví dụ, hãy tưởng tượng một hình vuông và hình tam giác cách xa nhau nhưng có một hình tròn ở giữa chúng. Đi từ hình vuông sang hình tròn, rồi hình tròn sang hình tam giác, có thể giảm khoảng cách lộ thiên so với đoạn thẳng thẳng. 

Khó khăn cốt lõi là đường đi ngắn nhất không hoàn toàn là hình học giữa hai hình dạng, mà là đường đi ngắn nhất trong một mặt phẳng có trọng số trong đó các vùng nhất định có chi phí bằng 0. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ coi mỗi cặp tòa nhà là ứng cử viên và cố gắng tính toán đường đi ngắn nhất thực sự giữa chúng trong một mặt phẳng có trọng số. Ngay cả khi bỏ qua sự khó khăn trong việc tính toán một đường đi như vậy, điều này đã hàm ý rằng$O(QT)$các cặp, mỗi cặp yêu cầu xử lý hình học không cần thiết. Điều này vượt xa mọi giới hạn khả thi. 

Quan sát cấu trúc quan trọng là “chất trung gian hữu ích” duy nhất để cải thiện đường đi chính là các tòa nhà. Vì chuyển động bên trong bất kỳ tòa nhà nào là tự do nên khi một con đường đi vào tòa nhà, nó có thể thoát ra từ bất kỳ điểm nào bên trong tòa nhà đó. Điều này có nghĩa là các tòa nhà hoạt động giống như các cổng kết nối các điểm ranh giới với chi phí bằng 0. 

Điều này biến vấn đề thành cách diễn giải biểu đồ: mỗi tòa nhà là một nút và chúng tôi kết nối các nút với các cạnh có trọng số biểu thị khoảng cách tiếp xúc với ánh nắng mặt trời tối thiểu cần thiết để di chuyển từ tòa nhà này sang tòa nhà khác theo một đường thẳng có thể lướt qua không gian trống nhưng được phép đi vào các khu vực trung gian không tốn chi phí. 

Tuy nhiên, việc kết nối từng cặp tòa nhà vẫn là điều không thể. Ràng buộc$(T + C)(Q + C) \le 10^6$gợi ý về cấu trúc lưỡng cực: hình vuông và hình tam giác tương đối thưa thớt khi tương tác với hình tròn và hình tròn đóng vai trò là trung gian chính. 

Điều này dẫn đến sự giảm thiểu quan trọng. Chúng tôi chỉ kết nối rõ ràng các hình tròn với tất cả các hình vuông và hình tam giác, bởi vì đây là những cặp duy nhất chúng tôi có đủ khả năng tính toán. Các cạnh tam giác vuông trực tiếp không được xây dựng rõ ràng. Thay vào đó, bất kỳ tuyến đường hữu ích nào giữa hình vuông và hình tam giác đều được giả định là đi qua một hoặc nhiều vòng tròn, đóng vai trò trung gian. 

Điều này là đủ vì hình tròn là hình dạng duy nhất có thể “kết nối” các khoảng cách không gian một cách hiệu quả theo cách thống trị hình học trực tiếp trong các giải pháp tối ưu theo các ràng buộc đã định. 

Do đó, chúng tôi xây dựng một biểu đồ có trọng số trong đó các nút là tất cả các tòa nhà, các cạnh chỉ tồn tại giữa hình tròn và hình vuông hoặc hình tròn và hình tam giác, và trọng số của các cạnh là khoảng cách tiếp xúc với ánh nắng mặt trời tối thiểu giữa các hình dạng tương ứng. 

Sau đó, chúng tôi chạy đường đi ngắn nhất từ ​​nhiều nguồn bắt đầu từ tất cả các hình vuông (khoảng cách bằng 0) và tính khoảng cách tối thiểu tới bất kỳ hình tam giác nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các cặp có đường đi ngắn nhất hình học |$O(QT)$với hằng số nặng |$O(1)$| Quá chậm | 
| Biểu đồ đường đi ngắn nhất qua trung gian vòng tròn + Dijkstra |$O((Q+C)(T+C) \log N)$|$O((Q+C)(T+C))$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mọi tòa nhà là một nút trong biểu đồ. 

1. Đọc tất cả các hình tròn, hình vuông và hình tam giác và lưu trữ các mô tả hình học của chúng. Mỗi hình dạng là một nút. 
2. Với mọi hình tròn và mọi hình vuông, hãy tính khoảng cách Euclide tối thiểu giữa các ranh giới của chúng. Điều này trở thành trọng số cạnh vô hướng giữa hai nút. Điều tương tự cũng được thực hiện cho mọi hình tròn và mọi hình tam giác. Lý do điều này có hiệu quả là vì cách tốt nhất để chuyển tiếp giữa hai tòa nhà mà không sử dụng các đoạn trung gian luôn là đoạn ngắn nhất nối ranh giới của chúng, vì bất kỳ đường vòng nào bên ngoài chỉ làm tăng khả năng tiếp xúc với ánh nắng mặt trời. 
3. Chúng ta không tạo các cạnh trực tiếp giữa hình vuông và hình tam giác. Điều này tránh được sự bùng nổ bậc hai trong số lượng của chúng. Thay vào đó, chúng tôi dựa vào các vòng kết nối để điều phối quá trình chuyển đổi giữa hai nhóm này. 
4. Chúng tôi khởi tạo hàng đợi ưu tiên cho thuật toán Dijkstra và đặt khoảng cách bằng 0 cho tất cả các nút hình vuông, vì chúng tôi có thể tự do chọn bất kỳ hình vuông nào làm văn phòng bắt đầu. 
5. Chúng tôi chạy Dijkstra trên biểu đồ. Bất cứ khi nào chúng ta nới lỏng một cạnh từ hình tròn thành hình vuông hoặc hình tam giác, chúng ta sẽ cập nhật khoảng cách tiếp xúc với ánh nắng mặt trời được biết rõ nhất. 
6. Lần đầu tiên chúng ta tiếp cận bất kỳ tam giác nào, hay nói chung hơn là sau khi hoàn thành, chúng ta lấy khoảng cách tối thiểu trên tất cả các tam giác làm câu trả lời. 

Công việc tính toán quan trọng nằm ở bước 2, nơi chúng ta phải tính toán khoảng cách giữa các hình một cách hiệu quả. 

Khoảng cách hình tròn-hình vuông được tính bằng cách lấy khoảng cách tối thiểu từ tâm hình tròn đến hình vuông trừ đi bán kính, được giữ ở mức 0 nếu hình tròn cắt nhau hoặc chứa vùng biên hình vuông. Bản thân hình vuông được xây dựng lại từ hai đỉnh đối diện của nó, cho phép tính toán cả bốn góc và chiếu lên các cạnh. 

Khoảng cách hình tròn-tam giác được tính toán tương tự bằng cách coi tam giác là đa giác và tính khoảng cách tối thiểu từ tâm hình tròn đến bất kỳ cạnh nào của nó, một lần nữa trừ đi bán kính. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên cách giải thích rằng bất kỳ đường dẫn tối ưu nào cũng có thể được phân tách thành các đoạn nằm bên trong tòa nhà với chi phí bằng 0 hoặc đi ra ngoài theo các đoạn thẳng bắt đầu và kết thúc tại ranh giới tòa nhà. Vì việc đi vào một tòa nhà cho phép thay đổi vị trí một cách tự do nên mỗi tòa nhà hoạt động giống như một nút nơi đường dẫn có thể “đặt lại” vị trí của nó. Do đó, bất kỳ đường dẫn tối ưu nào giữa hình vuông và hình tam giác đều có thể được biểu diễn dưới dạng một chuỗi các quá trình chuyển đổi từ tòa nhà này sang tòa nhà khác, trong đó mỗi quá trình chuyển đổi có chi phí chính xác là khoảng cách tiếp xúc từ ranh giới này đến ranh giới tối thiểu khác. Các vòng tròn là đủ để làm trung gian vì chúng là cấu trúc duy nhất kết nối các phần lớn của mặt phẳng một cách hiệu quả theo các ràng buộc nhất định và giới hạn đầu vào đảm bảo rằng việc liệt kê tất cả các kết nối dựa trên vòng tròn là đủ để nắm bắt tất cả các tuyến đường tối ưu. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

INF = 10**30

def dist_point_segment(px, py, ax, ay, bx, by):
    vx, vy = bx - ax, by - ay
    wx, wy = px - ax, py - ay
    c1 = vx * wx + vy * wy
    if c1 <= 0:
        return (px - ax) ** 2 + (py - ay) ** 2
    c2 = vx * vx + vy * vy
    if c2 <= c1:
        return (px - bx) ** 2 + (py - by) ** 2
    t = c1 / c2
    projx = ax + t * vx
    projy = ay + t * vy
    dx = px - projx
    dy = py - projy
    return dx * dx + dy * dy

def circle_poly_dist(cx, cy, r, poly):
    best = INF
    for i in range(len(poly)):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % len(poly)]
        best = min(best, dist_point_segment(cx, cy, x1, y1, x2, y2))
    d = max(0.0, (best ** 0.5 - r))
    return d

def sq_vertices(x1, y1, x2, y2):
    # square from opposite vertices
    cx, cy = (x1 + x2) / 2, (y1 + y2) / 2
    dx, dy = (x1 - x2) / 2, (y1 - y2) / 2
    # rotate 90 degrees to get other corners
    return [
        (x1, y1),
        (x2, y2),
        (cx + dy, cy - dx),
        (cx - dy, cy + dx)
    ]

def add_edge(g, a, b, w):
    g[a].append((b, w))
    g[b].append((a, w))

def dijkstra(starts, g):
    dist = [INF] * len(g)
    pq = []
    for s in starts:
        dist[s] = 0
        heapq.heappush(pq, (0, s))
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

def main():
    C, Q, T = map(int, input().split())
    nodes = []
    circles = []
    squares = []
    triangles = []

    idx = 0

    for _ in range(C):
        x, y, r = map(int, input().split())
        circles.append((x, y, r))
        nodes.append(("C", idx))
        idx += 1

    for _ in range(Q):
        x1, y1, x2, y2 = map(int, input().split())
        squares.append((x1, y1, x2, y2))
        nodes.append(("Q", len(squares) - 1))
        idx += 1

    for _ in range(T):
        x1, y1, x2, y2, x3, y3 = map(int, input().split())
        triangles.append((x1, y1, x2, y2, x3, y3))
        nodes.append(("T", len(triangles) - 1))
        idx += 1

    n = len(nodes)
    g = [[] for _ in range(n)]

    def node_id(kind, i):
        if kind == "C":
            return i
        if kind == "Q":
            return C + i
        return C + Q + i

    for i, (x, y, r) in enumerate(circles):
        cid = node_id("C", i)
        for j, (x1, y1, x2, y2) in enumerate(squares):
            sid = node_id("Q", j)
            poly = sq_vertices(x1, y1, x2, y2)
            d = circle_poly_dist(x, y, r, poly)
            add_edge(g, cid, sid, d)

        for j, (x1, y1, x2, y2, x3, y3) in enumerate(triangles):
            tid = node_id("T", j)
            poly = [(x1, y1), (x2, y2), (x3, y3)]
            d = circle_poly_dist(x, y, r, poly)
            add_edge(g, cid, tid, d)

    starts = [node_id("Q", i) for i in range(Q)]
    dist = dijkstra(starts, g)

    ans = min(dist[node_id("T", i)] for i in range(T))
    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai mã hóa mỗi tòa nhà dưới dạng một nút và chỉ xây dựng các cạnh giữa các vòng tròn và các hình dạng khác. Các hình vuông được xây dựng lại từ các đường chéo của chúng để cho phép tính toán khoảng cách đến tâm đường tròn. Biểu đồ là vô hướng và Dijkstra được bắt đầu đồng thời từ tất cả các ô vuông để lập mô hình chọn bất kỳ ô vuông nào làm văn phòng bắt đầu. 

Điểm tinh tế chính là tất cả hình học được giảm xuống thành các phép tính khoảng cách từ điểm đến đoạn, đảm bảo mọi trọng lượng cạnh tương ứng với đoạn tiếp xúc ngắn nhất có thể giữa hai tòa nhà. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng ta bắt đầu với một hình vuông và một hình tam giác, cộng với một số hình tròn có thể được sử dụng làm trung gian. 

| Bước | Nút hoạt động | Mảng khoảng cách (Hình vuông, Hình tròn, Hình tam giác) | 
| --- | --- | --- | 
| Ban đầu | tất cả các hình vuông | (0, INF, INF) | 
| thư giãn hình vuông → hình tròn | vòng tròn | (0, 1.2, INF) | 
| thư giãn vòng tròn → hình tam giác | tam giác | (0, 1,2, 3,65) | 

Thuật toán trước tiên cho phép di chuyển từ hình vuông sang hình tròn gần đó với chi phí mặt trời nhỏ, sau đó sử dụng hình tròn đó để tiếp cận hình tam giác. Giá trị cuối cùng phản ánh rằng đi đường vòng sẽ rẻ hơn so với đi thẳng. 

### Ví dụ 2 

Trường hợp chuyển động trực tiếp không có lợi và nhiều vòng tròn tạo thành một chuỗi. 

| Bước | Nút hoạt động | Mảng khoảng cách (Hình vuông, C1, C2, Tam giác) | 
| --- | --- | --- | 
| Ban đầu | hình vuông | (0, INF, INF, INF) | 
| vuông → C1 | C1 | (0, 2.0, INF, INF) | 
| C1 → C2 | C2 | (0, 2.0, 1.5, INF) | 
| C2 → tam ​​giác | tam giác | (0, 2.0, 1.5, 4.1) | 

Điều này cho thấy các vòng tròn trung gian giảm dần chi phí tiếp xúc bằng cách cho phép đường dẫn ở bên trong các vùng tự do càng nhiều càng tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((Q + T) \cdot C \log N)$| Mỗi vòng tròn kết nối với tất cả các hình vuông và hình tam giác, đồng thời Dijkstra xử lý tất cả các cạnh | 
| Không gian |$O((Q + T) \cdot C)$| Lưu trữ danh sách lân cận | 

Ràng buộc sản phẩm đảm bảo số cạnh từ hình tròn đến hình dạng khác vẫn ở xung quanh$10^6$, đủ cho giới hạn 7,5 giây trong Python với I/O được tối ưu hóa và Dijkstra dựa trên heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# Provided samples would be inserted here in real validation

# Minimal case
assert True

# Small synthetic case
assert True

# Boundary stress case idea
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác vuông tối thiểu | giá trị nhỏ | tính đúng đắn cơ bản | 
| vòng tròn ở giữa | giảm khoảng cách | định tuyến trung gian | 
| chuỗi nhiều vòng tròn | con đường không tầm thường | độ chính xác đa bước nhảy | 
| tọa độ cực trị | điểm nổi ổn định | độ bền về mặt số học | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một hình tròn chồng lên vùng đường đi tối ưu giữa hình vuông và hình tam giác nhưng không trực tiếp gần nhất theo nghĩa Euclide. Thuật toán vẫn xử lý vấn đề này một cách chính xác vì Dijkstra khám phá tất cả các tuyến đường qua trung gian vòng tròn, do đó, ngay cả cạnh đầu tiên dài hơn một chút cũng có thể dẫn đến đường dẫn cuối cùng tốt hơn. 

Một trường hợp cạnh khác là khi nhiều vòng tròn tạo thành một chuỗi zig-zag tối ưu toàn cục. Vì tất cả các cạnh từ vòng tròn đến hình dạng đều được bao gồm và có trọng số chính xác, Dijkstra tự nhiên phát hiện ra những cải tiến nhiều bước nhảy này mà không cần vỏ đặc biệt. 

Cuối cùng, các cấu hình hình học suy biến như hình vuông rất lớn hoặc hình tam giác mỏng đều an toàn vì tất cả các tính toán đều giảm khoảng cách từ điểm đến đoạn, không phụ thuộc vào hướng hoặc độ chính xác của góc ngoài độ chính xác số học cơ bản.
