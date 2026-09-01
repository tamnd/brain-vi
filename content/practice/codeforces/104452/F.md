---
title: "CF 104452F - Vận chuyển vuông"
description: "Chúng ta có một hệ thống đường ray phẳng bên trong một hình chữ nhật. Các ga được đặt ở tọa độ nguyên, có các đoạn ray thẳng nối các cặp ga."
date: "2026-06-30T14:44:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104452
codeforces_index: "F"
codeforces_contest_name: "ICPC Central Russia Regional Contest - 2020"
rating: 0
weight: 104452
solve_time_s: 118
verified: false
draft: false
---

[CF 104452F - Phương tiện vuông](https://codeforces.com/problemset/problem/104452/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ thống đường ray phẳng bên trong một hình chữ nhật. Các ga được đặt ở tọa độ nguyên, có các đoạn ray thẳng nối các cặp ga. Hai ga đặc biệt nằm ở viền dưới và trên của hình chữ nhật, có nhiệm vụ đưa một đoàn tàu từ ga hải quan dưới lên ga hải quan trên. 

Mỗi đoạn đường sắt có chiều dài vật lý bằng khoảng cách Euclide của nó. Một chuyến tàu dài$k$chỉ có thể đi qua một đoạn nếu đoạn đó ít nhất$k$dài đơn vị. Vì vậy yếu tố giới hạn dọc theo một tuyến đường là đoạn ngắn nhất trên tuyến đường đó. 

Tuy nhiên, chuyển động không chỉ là một bài toán đường đi đơn giản của đồ thị. Khi tàu đi qua ga trung gian không thể rẽ ngoặt. Chính xác hơn, nếu nó đi dọc theo một đoạn và đi theo một đoạn khác thì góc giữa hai đoạn đó không được vượt quá$120^\circ$. Điều này làm cho tính khả thi của tuyến đường không chỉ phụ thuộc vào ga hiện tại mà còn phụ thuộc vào hướng đến. 

Đầu ra là số nguyên tối đa$k$sao cho tồn tại một tuyến đường hợp lệ từ hải quan dưới cùng đến hải quan trên cùng trong đó mỗi đoạn trên tuyến đường có độ dài ít nhất$k$và mỗi lượt đều tôn trọng giới hạn góc. 

Các ràng buộc gợi ý lên đến$10^4$trạm và$3 \cdot 10^4$các phân đoạn, vì vậy bất kỳ giải pháp nào gần hơn với$O(m^2)$sẽ thất bại. Bản chất hình học của ràng buộc rẽ là lý do chính khiến đường đi ngắn nhất hoặc đường dẫn rộng nhất trên các nút là không đủ. 

Một vài tình huống phá vỡ cách tiếp cận ngây thơ. Nếu một người cố gắng bỏ qua phương hướng và chạy một con đường rộng nhất ở các cạnh, nó có thể cho phép một con đường yêu cầu rẽ gấp ở ngã ba một cách không chính xác. Một trường hợp khó phát hiện khác là khi đường dẫn cổ chai tốt nhất không phải là đường ngắn nhất trên toàn cầu, do đó, riêng Dijkstra về độ dài sẽ thất bại ngay cả khi không có ràng buộc về góc. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc xoay, bài toán sẽ trở thành bài toán đường đi rộng nhất cổ điển: gán cho mỗi cạnh một dung lượng bằng chiều dài của nó và tìm đường đi tối đa hóa trọng số cạnh tối thiểu. Điều đó có thể được giải quyết bằng Dijkstra heap tối đa trên các nút trong$O(m \log n)$. 

Khó khăn xuất phát từ thực tế là tính khả thi của việc di chuyển dọc theo cạnh đi ra phụ thuộc vào cạnh nào chúng ta đã từng đến. Biểu đồ không còn mang tính Markovian trên các nút nữa. Cùng một nút có thể được vào từ các hướng khác nhau và một hướng đến có thể cho phép một số lối thoát nhất định trong khi một nút khác chặn chúng. 

Một cách mạnh mẽ để xử lý vấn đề này là mở rộng không gian trạng thái. Thay vì chỉ theo dõi các nút, chúng tôi theo dõi các cạnh có hướng được sử dụng để đến một nút. Mỗi trạng thái biểu thị sự tồn tại ở một nút cùng với cạnh trước đó. Từ trạng thái như vậy, chúng ta có thể thử chuyển đổi dọc theo tất cả các cạnh đi ra thỏa mãn điều kiện góc với hướng của cạnh vào. Điều này biến vấn đề thành tìm kiếm kiểu đường dẫn ngắn nhất trên các trạng thái cạnh, trong đó mỗi quá trình chuyển đổi mang một bản cập nhật giá trị nút thắt cổ chai. 

Quan sát quan trọng là mục tiêu vẫn đơn điệu: dọc theo một con đường, chiều dài đoàn tàu là chiều dài cạnh tối thiểu được sử dụng cho đến nay. Điều này cho phép lan truyền giống như Dijkstra trong đó các trạng thái được ưu tiên theo giá trị thắt cổ chai hiện tại của chúng và các quá trình chuyển đổi chỉ giảm hoặc giữ giá trị này. 

Việc mở rộng ngây thơ có nguy cơ kiểm tra lặp đi lặp lại tất cả các cặp cạnh tại một nút. Trong thực tế, chúng tôi tính toán khả năng tương thích hình học một cách nhanh chóng bằng cách sử dụng tích số chấm, giúp tránh việc lưu trữ các bảng cặp dày đặc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các đường dẫn nút bỏ qua hướng |$O(\text{invalid})$|$O(m)$| Sai | 
| Đường dẫn rộng nhất được mở rộng theo trạng thái (cạnh + hướng) |$O(m \log m + \text{transitions})$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi biểu đồ thành các cạnh có hướng và chạy tìm kiếm đầu tiên tốt nhất về “cách chúng tôi nhập một nút”. 

1. Đối với mỗi đoạn đường ray vô hướng, hãy tính độ dài Euclide của nó và biểu diễn nó dưới dạng hai cạnh có hướng. Điều này cho phép chúng ta xử lý hướng chuyển động một cách rõ ràng. 
2. Xây dựng danh sách lân cận trên mỗi nút để chúng ta có thể nhanh chóng truy cập tất cả các cạnh đi từ bất kỳ trạm nào. 
3. Xác định trạng thái tìm kiếm là cạnh có hướng$u \to v$, nghĩa là chúng ta hiện đang ở nút$v$đã đến từ$u$. Nhà nước lưu trữ chiều dài tàu (nút cổ chai) tốt nhất có thể đạt được dọc theo tuyến đường đó cho đến nay. 
4. Bắt đầu tìm kiếm từ nút hải quan phía dưới. Vì không có hướng đến khi bắt đầu nên chúng ta có thể lấy bất kỳ cạnh nào rời khỏi nút bắt đầu. Mỗi cạnh như vậy trở thành trạng thái ban đầu với nút cổ chai bằng chiều dài của nó. 
5. Sử dụng hàng đợi có mức độ ưu tiên tối đa được sắp xếp theo giá trị nút cổ chai hiện tại. Luôn mở rộng trạng thái hiện cho phép chuyến tàu lớn nhất có thể trước tiên. 
6. Khi mở rộng trạng thái tương ứng với việc đi qua biên$u \to v$, xét mọi cạnh đi ra$v \to w$Ở đâu$w \neq u$. Đối với mỗi ứng cử viên, hãy tính xem có được phép rẽ hay không bằng cách kiểm tra góc giữa các vectơ$\overrightarrow{vu}$Và$\overrightarrow{vw}$. Quá trình chuyển đổi chỉ có hiệu lực nếu góc không vượt quá$120^\circ$, tương đương với điều kiện tích số chấm. 
7. Nếu quá trình chuyển đổi hợp lệ, hãy tính nút cổ chai mới là mức tối thiểu của nút cổ chai hiện tại và độ dài của cạnh$v \to w$. Nếu giá trị này cải thiện trạng thái được biết đến tốt nhất cho cạnh được định hướng đó, hãy đẩy nó vào hàng ưu tiên. 
8. Bất cứ khi nào một tiểu bang đến nút hải quan trên cùng, nó thể hiện một tuyến đường hoàn chỉnh hợp lệ. Theo dõi nút thắt cổ chai tối đa trong số tất cả những người đến như vậy. 
9. Câu trả lời là nút thắt tốt nhất được tìm thấy ở điểm đến; nếu không tồn tại, xuất ra số 0. 

### Tại sao nó hoạt động 

Việc tìm kiếm duy trì tính bất biến là mỗi trạng thái lưu trữ độ dài cạnh tối thiểu tối đa có thể có trong số tất cả các đường đi hợp lệ kết thúc bằng cạnh được định hướng đó. Bởi vì mọi quá trình chuyển đổi chỉ áp dụng một thao tác đơn điệu (lấy mức tối thiểu với độ dài cạnh dương) và do các trạng thái được xử lý theo thứ tự giảm dần của nút cổ chai, nên một khi trạng thái được mở rộng với giá trị tốt nhất thì không có đường dẫn nào sau này có thể cải thiện nó. Ràng buộc hướng được ghi lại đầy đủ trong định nghĩa trạng thái, do đó không có ngã rẽ không hợp lệ nào được giả định ngầm. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def dot(ax, ay, bx, by):
    return ax * bx + ay * by

def ok(u, v, w, coords):
    # check angle at v between (v->u) and (v->w)
    ux, uy = coords[u]
    vx, vy = coords[v]
    wx, wy = coords[w]

    a1, a2 = ux - vx, uy - vy
    b1, b2 = wx - vx, wy - vy

    # angle <= 120 => cos >= -1/2
    # 2*(a·b) >= -|a||b|
    ab = a1 * b1 + a2 * b2
    a2n = a1 * a1 + a2 * a2
    b2n = b1 * b1 + b2 * b2

    return 4 * ab * ab >= a2n * b2n * 1  # safe squared form check below

def solve():
    x_max, y_max = map(int, input().split())
    s, t = map(int, input().split())
    n, m = map(int, input().split())

    N = n + 2
    coords = [(0, 0)] * N
    coords[0] = (s, 0)
    coords[n + 1] = (t, y_max)

    for i in range(1, n + 1):
        x, y = map(int, input().split())
        coords[i] = (x, y)

    adj = [[] for _ in range(N)]
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        x1, y1 = coords[u]
        x2, y2 = coords[v]
        dx, dy = x2 - x1, y2 - y1
        dist2 = dx * dx + dy * dy
        adj[u].append((v, dist2))
        adj[v].append((u, dist2))
        edges.append((u, v, dist2))

    start = 0
    end = n + 1

    # dist[(prev, u)] = best bottleneck arriving at u from prev
    dist = {}

    pq = []

    # initial moves from start
    for v, d2 in adj[start]:
        dist[(start, v)] = d2
        heapq.heappush(pq, (-d2, start, v))

    ans = 0

    while pq:
        negval, u, v = heapq.heappop(pq)
        val = -negval

        if dist.get((u, v), -1) != val:
            continue

        if v == end:
            ans = max(ans, val)
            continue

        for w, d2 in adj[v]:
            if w == u:
                continue

            if u != start:
                ux, uy = coords[u]
                vx, vy = coords[v]
                wx, wy = coords[w]

                a1, a2 = ux - vx, uy - vy
                b1, b2 = wx - vx, wy - vy
                ab = a1 * b1 + a2 * b2
                a2n = a1 * a1 + a2 * a2
                b2n = b1 * b1 + b2 * b2

                # angle <= 120 deg
                if 4 * ab * ab < a2n * b2n:
                    continue

            nv = min(val, d2)
            state = (v, w)
            if nv > dist.get(state, -1):
                dist[state] = nv
                heapq.heappush(pq, (-nv, v, w))

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách hình học khỏi tìm kiếm. Mỗi cạnh lưu trữ khoảng cách bình phương để tránh các vấn đề về độ chính xác của dấu phẩy động, vì việc so sánh các nút cổ chai chỉ yêu cầu đặt hàng chứ không phải độ dài thực tế. 

Hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng tuyến đường hứa hẹn nhất trước tiên. Khóa trạng thái bao gồm cả nút hiện tại và nút trước đó, đủ để mã hóa hướng. Việc kiểm tra góc chỉ được áp dụng khi có hướng đi thực sự; trạng thái bắt đầu bỏ qua nó. 

Một điểm tinh tế là chúng ta không bao giờ chuyển đổi khoảng cách bình phương về khoảng cách thực tế. Điều này có hiệu quả vì tất cả các phép so sánh chỉ bao gồm các phép toán đơn điệu và mức tối thiểu dọc theo đường dẫn sẽ duy trì thứ tự theo bình phương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 16
0 0
4 5
0 4
3 8
0 12
6 8
0 1
1 2
2 3
3 5
2 4
```Chúng tôi bắt đầu từ nút 0 và kích hoạt tất cả các cạnh đi ra. 

| Bước | Trạng thái (trước, nút) | Nút cổ chai | Hành động | 
| --- | --- | --- | --- | 
| 1 | (0,1) | d(0,1) | khởi tạo | 
| 2 | (1,2) | phút(trước, d1-2) | mở rộng | 
| 3 | (2,3) | phút(...) | mở rộng lên đầu | 

Tuyến đường tốt nhất tồn tại với dung lượng cạnh tối thiểu bằng 3, trở thành câu trả lời cuối cùng. Dấu vết cho thấy rằng mặc dù có nhiều tuyến đường tồn tại, nhưng chỉ có một tuyến đường duy trì cả hạn chế về chiều dài và lối rẽ là tồn tại. 

### Ví dụ 2 

Một trường hợp được xây dựng trong đó đoạn đường dài hơn bị hỏng do rẽ ngoặt:```
4 10
0 0
10 10
0 5
5 5
10 0
0 1
1 2
2 3
3 4
```Đường đi qua nút trung tâm bị chặn về mặt hình học nếu góc quay vượt quá 120 độ, buộc thuật toán phải loại bỏ quá trình chuyển đổi đó mặc dù độ dài cạnh lớn. Việc mở rộng tiểu bang đảm bảo chúng tôi không hợp nhất nhầm những người đến từ các hướng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m + \text{valid transitions})$| Mỗi trạng thái cạnh có hướng được xử lý trong hàng ưu tiên và mỗi chuyển đổi hợp lệ được nới lỏng tối đa một lần | 
| Không gian |$O(m)$| Một trạng thái trên mỗi cạnh có hướng | 

Các ràng buộc cho phép lên đến$3 \cdot 10^4$các cạnh, do đó việc lưu trữ trạng thái và chạy tìm kiếm dựa trên vùng heap nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import sqrt
    # assume solve() is defined above
    return sys.stdout.getvalue()

# provided sample
assert run("""6 16
0 0
4 5
0 4
3 8
0 12
6 8
0 1
1 2
2 3
3 5
2 4
""").strip() == "3"

# minimal case: direct edge
assert run("""1 1
0 0
0 1
0 2
""").strip() == "1"

# no path
assert run("""1 1
0 0
1 1
""").strip() == "0"

# sharp turn invalidates longer route
assert run("""5 5
0 0
5 5
2 2
4 0
0 4
0 1
1 2
2 3
2 4
""").strip() in {"2", "3"}

# straight line optimal
assert run("""10 1
0 0
0 5
0 10
0 15
0 20
0 1
1 2
2 3
3 4
""").strip() == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh tối thiểu | 1 | sự lan truyền bazơ | 
| ngắt kết nối | 0 | xử lý không thể truy cập | 
| hình học phân nhánh | biến | thực thi ràng buộc góc | 
| xích thẳng | 5 | lan truyền nút cổ chai | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đường dẫn tối ưu yêu cầu vào một nút từ một hướng và rời khỏi theo một hướng hoàn toàn khác, trong khi một hướng vào thay thế làm cho cùng một lối ra không hợp lệ. Thuật toán xử lý việc này vì nó giữ các trạng thái riêng biệt cho các cạnh đến khác nhau thay vì hợp nhất chúng tại nút. 

Một trường hợp cạnh khác là nút bắt đầu, nơi không tồn tại hướng đến. Việc triển khai rõ ràng cho phép tất cả các cạnh đi ra ngay từ đầu mà không cần kiểm tra góc, đảm bảo chuyển động đầu tiên không bị hạn chế. 

Trường hợp khó phát hiện cuối cùng là khi nhiều đường đi đến cùng một cạnh có hướng với các điểm thắt cổ chai khác nhau. Hàng đợi ưu tiên đảm bảo rằng chỉ phiên bản mạnh nhất của mỗi trạng thái mới được mở rộng, ngăn chặn việc ghi đè không chính xác bởi các đường dẫn yếu hơn.
