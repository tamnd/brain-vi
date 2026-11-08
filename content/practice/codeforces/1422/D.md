---
title: "CF 1422D - Trở Về Nhà"
description: "Chúng tôi đang di chuyển trên một mạng lưới khổng lồ. Ô bắt đầu là $(sx,sy)$ và ô đích là $(fx,fy)$. Đi bộ hoạt động bình thường: di chuyển một ô theo chiều ngang hoặc chiều dọc tốn một phút, do đó chi phí đi bộ giữa hai vị trí là khoảng cách Manhattan."
date: "2026-06-11T06:22:12+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1422
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 675 (Div. 2)"
rating: 2300
weight: 1422
solve_time_s: 205
verified: true
draft: false
---

[CF 1422D - Trở về nhà](https://codeforces.com/problemset/problem/1422/D) 

**Đánh giá:** 2300 
**Thẻ:** đồ thị, đường đi ngắn nhất, sắp xếp 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang di chuyển trên một mạng lưới khổng lồ. Ô bắt đầu là$(s_x,s_y)$và đích đến là$(f_x,f_y)$. Đi bộ hoạt động bình thường: di chuyển một ô theo chiều ngang hoặc chiều dọc tốn một phút, do đó chi phí đi bộ giữa hai vị trí là khoảng cách Manhattan. 

Ngoài ra còn có những địa điểm đặc biệt. Một vị trí đặc biệt$(x_i,y_i)$có thể đạt được ngay lập tức từ bất kỳ vị trí nào chia sẻ nó$x$-tọa độ hoặc của nó$y$-điều phối. Tương tự, nếu chúng ta đang đứng ở$(a,b)$, chúng ta có thể chuyển đến$(x_i,y_i)$với chi phí$$\min(|a-x_i|,\;|b-y_i|).$$Nếu$x$-tọa độ đã khớp, số hạng đầu tiên bằng 0 và dịch chuyển tức thời là miễn phí. Điều tương tự cũng xảy ra khi$y$-tọa độ trùng khớp. 

Nhiệm vụ là tìm thời gian tối thiểu cần thiết để về đến nhà. 

Quy mô thành phố$n$có thể lớn như$10^9$, điều này ngay lập tức cho chúng ta biết rằng bản thân lưới không thể được biểu diễn. Số lượng địa điểm đặc biệt nhiều nhất$10^5$, do đó, bất kỳ giải pháp nào hoạt động trực tiếp trên tập hợp các vị trí đặc biệt đều khả thi. Một đồ thị với$10^5$các đỉnh có thể quản lý được, nhưng một biểu đồ có tất cả các cạnh theo cặp sẽ chứa khoảng$10^{10}$cạnh, điều đó hoàn toàn không thể xảy ra. 

Phần nguy hiểm nhất của vấn đề là hiểu cách dịch chuyển tương tác. 

Coi như:```
start = (1,1)
finish = (100,100)

special:
(1,50)
(50,50)
```Điểm đặc biệt đầu tiên có thể truy cập miễn phí vì$x$-cosort đã phù hợp. Sau khi đến$(1,50)$, điểm đặc biệt thứ hai có thể đạt được với chi phí$\min(49,0)=0$bởi vì$y$-tọa độ trùng khớp. Một giải pháp chỉ xem xét khoảng cách Manhattan thông thường giữa các điểm đặc biệt sẽ bỏ lỡ chuỗi chuyển động tự do này. 

Một trường hợp tế nhị khác là khi không sử dụng điểm đặc biệt nào là tối ưu.```
start = (1,1)
finish = (3,3)
```không có vị trí đặc biệt. 

Câu trả lời đơn giản là$4$. Bất kỳ cách xây dựng biểu đồ nào cũng phải bao gồm khả năng đi bộ trực tiếp đến đích. 

Cái bẫy thứ ba là cho rằng mọi cặp điểm đặc biệt đều cần có lợi thế.```
(1,100)
(2,100)
(1000000000,100)
```Các chuyển tiếp hữu ích đến từ các điểm lân cận sau khi sắp xếp theo tọa độ. Kết nối từng cặp sẽ đúng nhưng quá chậm. Quan sát quan trọng là chỉ những người hàng xóm gần nhất theo thứ tự được sắp xếp mới quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng tự nhiên đầu tiên là xây dựng một biểu đồ có các đỉnh là các vị trí đặc biệt cùng với vị trí bắt đầu và kết thúc. 

Giả sử chúng ta tạo một cạnh giữa mỗi cặp vị trí đặc biệt. Chi phí di chuyển từ điểm đặc biệt$i$đến điểm đặc biệt$j$là$$\min(|x_i-x_j|,\;|y_i-y_j|).$$Chúng ta cũng có thể kết nối điểm bắt đầu với mọi điểm đặc biệt với chi phí$$\min(|s_x-x_i|,\;|s_y-y_i|).$$Cuối cùng, mọi đỉnh đều có thể đi thẳng tới đích với khoảng cách Manhattan. 

Biểu đồ này mô hình hóa vấn đề một cách chính xác và thuật toán của Dijkstra sẽ tìm ra câu trả lời. 

Vấn đề là kích thước. Với$m=10^5$, một biểu đồ hoàn chỉnh chứa khoảng$$\frac{m(m-1)}2 \approx 5 \cdot 10^9$$các cạnh. Việc lưu trữ hoặc xử lý biểu đồ đó là không thể. 

Bước đột phá đến từ việc kiểm tra trọng lượng cạnh$$\min(|x_i-x_j|,\;|y_i-y_j|).$$Giả sử hai điểm được kết nối vì chúng$x$-tọa độ rất gần. Khi các điểm được sắp xếp theo$x$, kết nối hữu ích rẻ nhất thông qua$x$-thứ nguyên luôn xuất hiện giữa các điểm lân cận theo thứ tự đó. 

Đây là quan sát tương tự được sử dụng trong một số bài toán đường đi ngắn nhất hình học. Nếu điểm$A$và chỉ$C$được ngăn cách bởi một điểm khác$B$được sắp xếp$x$-đặt hàng rồi$$|x_A-x_C|$$không thể nhỏ hơn cả hai khoảng trống lân cận. Con đường nào cũng muốn khai thác nhỏ$x$-sự khác biệt có thể làm như vậy thông qua các điểm liền kề. 

Lý do tương tự cũng đúng đối với$y$-điều phối. 

Vì vậy, thay vì kết nối từng cặp, chúng ta sắp xếp các điểm đặc biệt theo$x$và kết nối các cặp lân cận. Sau đó chúng ta sắp xếp theo$y$và kết nối các cặp lân cận một lần nữa. Đồ thị kết quả chỉ có$O(m)$các cạnh. 

Sau lần giảm này, thuật toán Dijkstra chạy thoải mái trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Biểu đồ hoàn chỉnh Brute Force |$O(m^2 \log m)$|$O(m^2)$| Quá chậm | 
| Biểu đồ lân cận + Dijkstra |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Giải thích biểu đồ 

Mỗi vị trí đặc biệt sẽ trở thành một đỉnh đồ thị. 

Chúng tôi không tạo đỉnh cho vị trí bắt đầu. Thay vào đó, điểm xuất phát cung cấp khoảng cách ban đầu cho mọi vị trí đặc biệt. 

Chúng tôi cũng không tạo đỉnh cho đích. Về đến nhà được coi là bước đi bộ cuối cùng từ bất kỳ địa điểm nào đã ghé thăm. 

### bước 

1. Đọc tất cả các vị trí đặc biệt và gán cho mỗi vị trí một chỉ mục. 
2. Sắp xếp các vị trí đặc biệt theo$x$-điều phối. 
3. Với mỗi cặp liền kề theo thứ tự này, hãy thêm một cạnh vô hướng có trọng số bằng hiệu của$x$-tọa độ. 

Quá trình chuyển đổi ngắn nhất dựa trên$x$-thứ nguyên luôn có thể được biểu diễn thông qua các điểm lân cận theo thứ tự được sắp xếp. 
4. Sắp xếp các vị trí đặc biệt theo$y$-điều phối. 
5. Đối với mỗi cặp liền kề theo thứ tự này, hãy thêm một cạnh vô hướng có trọng số bằng hiệu của$y$-tọa độ. 

Điều này nắm bắt tất cả các chuyển tiếp hữu ích dựa trên$y$-kích thước. 
6. Khởi tạo Dijkstra. 

Đối với mỗi điểm đặc biệt$i$,$$dist[i] = \min(|s_x-x_i|,\;|s_y-y_i|).$$Đây chính xác là chi phí để đạt được vị trí đặc biệt đó ngay từ đầu. 
7. Khởi tạo đáp án với chi phí đi bộ trực tiếp từ đầu đến cuối:$$|s_x-f_x|+|s_y-f_y|.$$8. Chạy Dijkstra trên biểu đồ các vị trí đặc biệt. 
9. Bất cứ khi nào có một đỉnh$i$được trích xuất từ ​​hàng đợi ưu tiên, cập nhật câu trả lời bằng cách sử dụng$$dist[i] + |x_i-f_x| + |y_i-f_y|.$$Điều này thể hiện việc sử dụng dịch chuyển tức thời đến điểm$i$, rồi đi thẳng về nhà. 
10. Thư giãn tất cả các cạnh của đồ thị theo cách Dijkstra thông thường. 
11. Sau khi Dijkstra kết thúc, hãy đưa ra câu trả lời nhỏ nhất tìm được. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi chuyển đổi hữu ích giữa các vị trí đặc biệt đều có thể được phân tách thành các chuyển động giữa các điểm lân cận được sắp xếp.$x$-đặt hàng hoặc sắp xếp$y$-đặt hàng. 

Giả sử một đường đi tối ưu sử dụng một cạnh có chi phí được xác định bởi$x$-sự khác biệt. Di chuyển qua các điểm liên tiếp được sắp xếp$x$-order tái tạo chi phí đó mà không làm tăng nó, vì tổng các khoảng cách tọa độ liên tiếp bằng tổng khoảng cách. Lập luận tương tự cũng đúng đối với$y$-kích thước. 

Kết quả là mọi tuyến đường tối ưu trong biểu đồ hoàn chỉnh đều có một tuyến đường tương đương trong biểu đồ thưa thớt chỉ được xây dựng từ các cặp lân cận. Sau đó, Dijkstra tính toán khoảng cách ngắn nhất tới tất cả các địa điểm đặc biệt và kết hợp từng địa điểm đó với quãng đường đi bộ cuối cùng từ Manhattan về nhà để xem xét mọi chiến lược tối ưu có thể có. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    sx, sy, fx, fy = map(int, input().split())

    pts = []
    for i in range(m):
        x, y = map(int, input().split())
        pts.append((x, y, i))

    g = [[] for _ in range(m)]

    by_x = sorted(pts)
    for i in range(m - 1):
        x1, y1, id1 = by_x[i]
        x2, y2, id2 = by_x[i + 1]
        w = x2 - x1
        g[id1].append((id2, w))
        g[id2].append((id1, w))

    by_y = sorted(pts, key=lambda p: p[1])
    for i in range(m - 1):
        x1, y1, id1 = by_y[i]
        x2, y2, id2 = by_y[i + 1]
        w = y2 - y1
        g[id1].append((id2, w))
        g[id2].append((id1, w))

    INF = 10**30
    dist = [INF] * m
    pq = []

    for x, y, idx in pts:
        d = min(abs(sx - x), abs(sy - y))
        dist[idx] = d
        heapq.heappush(pq, (d, idx))

    ans = abs(sx - fx) + abs(sy - fy)

    while pq:
        curd, v = heapq.heappop(pq)
        if curd != dist[v]:
            continue

        x, y, _ = pts[v]

        ans = min(
            ans,
            curd + abs(x - fx) + abs(y - fy)
        )

        for to, w in g[v]:
            nd = curd + w
            if nd < dist[to]:
                dist[to] = nd
                heapq.heappush(pq, (nd, to))

    print(ans)

solve()
```Biểu đồ chỉ chứa các vị trí đặc biệt. Vị trí xuất phát xuất hiện thông qua khoảng cách Dijkstra ban đầu, khớp chính xác với chi phí để đạt được từng điểm đặc biệt ngay từ đầu. 

Hai bước sắp xếp tạo ra biểu đồ thưa thớt. Một lỗi phổ biến là sử dụng$$\min(|x_i-x_j|, |y_i-y_j|)$$là trọng số cạnh giữa các điểm lân cận. Giải pháp được chấp nhận chỉ sử dụng chênh lệch tọa độ tương ứng với thứ tự sắp xếp. Đây là đối số nén chính từ giải pháp chính thức. 

Một điểm tinh tế khác là cập nhật câu trả lời cuối cùng. Đạt đến một điểm đặc biệt không buộc phải sử dụng dịch chuyển tức thời thêm. Bất kỳ lúc nào chúng ta cũng có thể dừng lại và đi bộ thẳng về nhà, vì vậy mỗi đỉnh được trích xuất đều đóng góp một câu trả lời ứng cử viên. 

Tất cả các giá trị đều dễ dàng khớp với số nguyên 64 bit, nhưng số nguyên Python loại bỏ mọi lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
1 1 5 5
1 2
4 1
3 3
```Điểm đặc biệt: 

| Chỉ mục | Điểm | 
| --- | --- | 
| 0 | (1,2) | 
| 1 | (4,1) | 
| 2 | (3,3) | 

Khoảng cách ban đầu: 

| Đỉnh | Chi phí từ đầu | 
| --- | --- | 
| (1,2) | 0 | 
| (4,1) | 0 | 
| (3,3) | 2 | 

Tiến trình Dijkstra: 

| Đỉnh bật lên | Khoảng cách | Câu trả lời hay nhất cho đến nay | 
| --- | --- | --- | 
| (1,2) | 0 | 7 | 
| (4,1) | 0 | 5 | 
| (3,3) | 2 | 4+2=6 | 

Số tối thiểu được tìm thấy là 5. 

Dấu vết này cho thấy tại sao việc đi bộ trực tiếp không phải lúc nào cũng tối ưu. Dịch chuyển tức thời cho phép chúng tôi tiếp cận$(4,1)$miễn phí và lưu một phần của tuyến đường. 

### Ví dụ 2 

đầu vào:```
10 2
1 1 10 10
1 7
7 7
```Khoảng cách ban đầu: 

| Đỉnh | Chi phí | 
| --- | --- | 
| (1,7) | 0 | 
| (7,7) | 6 | 

Dijkstra: 

| Đỉnh bật lên | Khoảng cách | Câu trả lời của ứng viên | 
| --- | --- | --- | 
| (1,7) | 0 | 12 | 
| (7,7) | 6 | 12 | 

Chi phí đi bộ trực tiếp$18$, vì vậy câu trả lời trở thành$12$. 

Ví dụ này cho thấy việc đạt được điểm dịch chuyển miễn phí có thể rút ngắn đáng kể hành trình như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Hai loại và Dijkstra trên$O(m)$cạnh | 
| Không gian |$O(m)$| Biểu đồ, khoảng cách và hàng đợi ưu tiên | 

Với$m \le 10^5$, việc sắp xếp chiếm ưu thế trong thời gian chạy.$m \log m$là khoảng vài triệu thao tác, trong giới hạn 2 giây trong Python. Biểu đồ chỉ chứa nhiều cạnh tuyến tính, do đó mức sử dụng bộ nhớ vẫn ở dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    sx, sy, fx, fy = map(int, input().split()

)
    pts = []
    for i in range(m):
        x, y = map(int, input().split())
        pts.append((x, y, i))

    g = [[] for _ in range(m)]

    by_x = sorted(pts)
    for i in range(m - 1):
        x1, y1, a = by_x[i]
        x2, y2, b = by_x[i + 1]
        w = x2 - x1
        g[a].append((b, w))
        g[b].append((a, w))

    by_y = sorted(pts, key=lambda p: p[1])
    for i in range(m - 1):
        x1, y1, a = by_y[i]
        x2, y2, b = by_y[i + 1]
        w = y2 - y1
        g[a].append((b, w))
        g[b].append((a, w))

    INF = 10**30
    dist = [INF] * m
    pq = []

    for x, y, idx in pts:
        d = min(abs(sx - x), abs(sy - y))
        dist[idx] = d
        heapq.heappush(pq, (d, idx))

    ans = abs(sx - fx) + abs(sy - fy)

    while pq:
        d, v = heapq.heappop(pq)
        if d != dist[v]:
            continue

        x, y, _ = pts[v]
        ans = min(ans, d + abs(x - fx) + abs(y - fy))

        for to, w in g[v]:
            nd = d + w
            if nd < dist[to]:
                dist[to] = nd
                heapq.heappush(pq, (nd, to))

    return str(ans) + "\n"

# sample
assert run(
"""5 3
1 1 5 5
1 2
4 1
3 3
"""
) == "5\n"

# no special locations
assert run(
"""10 0
1 1 10 10
"""
) == "18\n"

# start equals finish
assert run(
"""100 0
5 5 5 5
"""
) == "0\n"

# teleport immediately reachable
assert run(
"""10 1
1 1 10 10
1 10
"""
) == "9\n"

# large coordinate boundary
assert run(
"""1000000000 1
1 1 1000000000 1000000000
1 1000000000
"""
) == "999999999\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có địa điểm đặc biệt | 18 | Con đường Manhattan trực tiếp | 
| Bắt đầu bằng kết thúc | 0 | Câu trả lời không khoảng cách | 
| Truy cập dịch chuyển ngay lập tức | 9 | Truy cập miễn phí thông qua tọa độ phù hợp | 
| Tọa độ lớn | 999999999 | Xử lý đúng$10^9$giá trị -scale | 

## Vỏ cạnh 

### Không có vị trí đặc biệt 

đầu vào:```
10 0
1 1 10 10
```Biểu đồ trống. Dijkstra không bao giờ xử lý bất kỳ đỉnh nào. Câu trả lời ban đầu là khoảng cách Manhattan$$18.$$Giá trị đó được in trực tiếp. 

### Đã đến đích 

đầu vào:```
100 0
5 5 5 5
```Câu trả lời đi bộ trực tiếp bắt đầu từ số 0. Không có tuyến đường nào có thể cải thiện điều đó, vì vậy thuật toán cho kết quả bằng 0. 

### Chuỗi miễn phí thông qua tọa độ trùng khớp 

đầu vào:```
100 2
1 1 100 100
1 50
50 50
```Điểm đặc biệt đầu tiên có khoảng cách ban đầu bằng 0 vì nó$x$-tọa độ phù hợp với sự bắt đầu. 

Điểm thứ hai được kết nối thông qua chi phí bằng 0$y$-chuyển tiếp. 

Dijkstra truyền bá chuyển động tự do này một cách chính xác và khám phá ra con đường tối ưu. Điều này xác nhận rằng các chuỗi dịch chuyển được xử lý một cách tự nhiên bằng cách xây dựng biểu đồ. 

### Lưới cực lớn 

đầu vào:```
1000000000 1
1 1 1000000000 1000000000
1 1000000000
```Bản thân lưới không bao giờ được đại diện. Chỉ có tọa độ được lưu trữ. Tất cả các tính toán đều sử dụng sự khác biệt giữa các tọa độ, vì vậy thuật toán vẫn giữ nguyên$O(m \log m)$bất kể kích thước của$n$.
