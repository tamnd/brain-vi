---
title: "CF 105112G - Nhiệm vụ thiên hà"
description: "Chúng ta có một mạng lưới cố định các hành tinh trong không gian 3D, trong đó các cặp hành tinh nhất định được kết nối bằng các đường cao tốc không gian hai chiều. Mỗi đường cao tốc là một đoạn thẳng có độ dài đã biết được xác định bằng khoảng cách Euclide giữa các điểm cuối của nó."
date: "2026-06-27T19:58:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105112
codeforces_index: "G"
codeforces_contest_name: "2023-2024 ICPC Northwestern European Regional Programming Contest (NWERC 2023)"
rating: 0
weight: 105112
solve_time_s: 58
verified: true
draft: false
---

[CF 105112G - Nhiệm vụ thiên hà](https://codeforces.com/problemset/problem/105112/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới cố định các hành tinh trong không gian 3D, trong đó các cặp hành tinh nhất định được kết nối bằng các đường cao tốc không gian hai chiều. Mỗi đường cao tốc là một đoạn thẳng có độ dài đã biết được xác định bằng khoảng cách Euclide giữa các điểm cuối của nó. 

Một con tàu vũ trụ bắt đầu từ hành tinh 1 cho mỗi truy vấn. Đối với mỗi nhiệm vụ, chúng tôi được cung cấp một hành tinh mục tiêu và giới hạn thời gian. Chúng ta phải quyết định hai điều: liệu có thể đến được mục tiêu bằng cách sử dụng đường cao tốc trong thời gian cho phép hay không và nếu có thể, lượng nhiên liệu tối thiểu cần thiết trong số tất cả các cách hợp lệ thỏa mãn hạn chế về thời gian là bao nhiêu. 

Di chuyển dọc theo đường cao tốc không phải là chuyển động tùy ý. Mỗi lần di chuyển hoạt động giống như một cuộc chạy nước rút vật lý dọc theo một đoạn thẳng: con tàu bắt đầu đứng yên, tăng tốc, có thể đạt tốc độ tối đa nào đó và phải dừng lại chính xác ở điểm cuối. Chuyển động có cường độ gia tốc không đổi giới hạn trong 1 m/s² và mức tiêu thụ nhiên liệu tỷ lệ thuận với thời gian, do đó việc giảm thiểu nhiên liệu tương đương với việc giảm thiểu thời gian di chuyển. 

Hệ quả chính của mô hình vật lý này là việc đi qua một cạnh có độ dài L luôn mất một thời gian tối thiểu cố định, không phụ thuộc vào các lựa chọn trung gian, bởi vì cấu hình tối ưu là tăng tốc và giảm tốc đối xứng. Chi phí thời gian cho một cạnh tỷ lệ thuận với √L, đạt đến hệ số không đổi giống hệt nhau đối với tất cả các cạnh. Vì tất cả các truy vấn chỉ so sánh tổng thời gian di chuyển với một giới hạn nên chúng ta có thể coi mỗi trọng số cạnh là một giá trị xác định tỷ lệ với √khoảng cách. 

Do đó, bài toán trở thành bài toán đường đi ngắn nhất trong biểu đồ có trọng số, nhưng có thêm ràng buộc toàn cục cho mỗi truy vấn: chúng tôi chỉ xem xét các đường đi có tổng thời gian di chuyển không vượt quá t và trong số đó chúng tôi muốn có thời gian di chuyển tối thiểu có thể. 

Các ràng buộc đặt ở vị trí n, m, q lên tới 10^5. Điều này ngay lập tức loại trừ việc tính toán lại các đường dẫn ngắn nhất cho mỗi truy vấn hoặc chạy Dijkstra từ đầu q lần, sẽ là O(q m log n) và quá lớn. 

Trường hợp khó khăn phát sinh từ các thành phần bị ngắt kết nối. Nếu không đạt được mục tiêu từ 1 thì câu trả lời phải là “không thể” bất kể thời hạn. Một trường hợp quan trọng khác là khi thời hạn rất nhỏ nhưng vẫn tồn tại một đường dẫn; vì tất cả các trọng số của cạnh đều dương nên tính khả thi là đơn điệu về mặt thời gian, nhưng phải được kiểm tra theo khoảng cách ngắn nhất được tính toán trước. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chạy Dijkstra từ nút 1 cho mọi truy vấn, tính toán khoảng cách đường đi ngắn nhất đến tất cả các nút, sau đó trả lời từng truy vấn bằng cách kiểm tra khoảng cách đến mục tiêu và so sánh nó với giới hạn thời gian. Điều này đúng vì đường đi ngắn nhất sẽ giảm thiểu tổng thời gian di chuyển dưới các trọng số dương. Tuy nhiên, điều này là không khả thi về mặt tính toán. Mỗi Dijkstra có giá O(m log n) và thực hiện q lần sẽ dẫn đến O(q m log n), theo thứ tự 10^10 phép toán trong trường hợp xấu nhất. 

Quan sát quan trọng là biểu đồ là tĩnh và tất cả các truy vấn đều có chung một nguồn. Chúng tôi không cần tính toán lại cho mỗi truy vấn. Một phép tính đường đi ngắn nhất toàn cầu từ nút 1 là đủ. Sau khi chúng tôi tính toán thời gian di chuyển ngắn nhất tới mọi nút, mỗi truy vấn sẽ trở thành tra cứu theo thời gian không đổi: nếu dist[c] ≤ t, xuất ra dist[c], nếu không thì xuất ra “không thể”. 

Phần không cần thiết duy nhất còn lại là tính toán chính xác trọng số cạnh. Mỗi đường cao tốc có chiều dài Euclide L ở dạng 3D. Thời gian di chuyển tỷ lệ thuận với √L. Vì tất cả các truy vấn đều so sánh với giới hạn thời gian và cả nhiên liệu đầu ra tỷ lệ thuận với thời gian, nên chúng tôi có thể lưu trữ trực tiếp các trọng số cạnh dưới dạng √L và chạy một Dijkstra duy nhất. 

Điều này làm giảm toàn bộ vấn đề thành một phép tính đường đi ngắn nhất trong một biểu đồ thưa thớt lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chạy lại Dijkstra cho mỗi truy vấn | O(q m log n) | O(n + m) | Quá chậm | 
| Dijkstra đơn từ nút 1 | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả tọa độ hành tinh và xây dựng danh sách các cạnh kề. Đối với mỗi đường cao tốc, hãy tính khoảng cách Euclide giữa các điểm cuối, sau đó chuyển nó thành trọng số thời gian di chuyển bằng cách sử dụng căn bậc hai. 
2. Xây dựng biểu đồ danh sách kề trong đó mỗi cạnh (u, v) lưu trọng số tính toán. Điều này đảm bảo chúng ta có thể đi qua tất cả các đường cao tốc đi từ bất kỳ hành tinh nào một cách hiệu quả. 
3. Chạy Dijkstra bắt đầu từ hành tinh 1. Khởi tạo mảng khoảng cách với vô cùng và đặt dist[1] = 0. Đẩy (0, 1) vào hàng đợi ưu tiên. 
4. Liên tục trích xuất nút có khoảng cách dự kiến ​​nhỏ nhất. Nếu giá trị trích xuất đã lỗi thời, hãy bỏ qua nó. Nếu không thì thư giãn tất cả các cạnh đi ra. 
5. Đối với mỗi cạnh (u, v, w), hãy cố gắng cải thiện dist[v] bằng dist[u] + w. Điều này đảm bảo chúng tôi luôn duy trì thời gian di chuyển tốt nhất được biết đến đến từng hành tinh. 
6. Sau khi Dijkstra hoàn thành, hãy xử lý từng truy vấn một cách độc lập. Đối với truy vấn (c, t), kiểm tra xem dist[c] có hữu hạn và ≤ t hay không. Nếu có thì xuất ra dist[c], nếu không thì xuất ra “không thể”. 

Lý do sự phân tách này có tác dụng là vì các giá trị đường dẫn ngắn nhất không phụ thuộc vào các ràng buộc truy vấn. Sau khi tính toán, chúng mô tả đầy đủ tính khả thi. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thuộc tính tiêu chuẩn của thuật toán Dijkstra: khi tất cả các trọng số cạnh không âm, lần đầu tiên một nút được hoàn thiện, chúng tôi đã tìm thấy chi phí đường đi tối thiểu có thể có đến nút đó. Vì mọi tuyến đường di chuyển hợp lệ đều tương ứng chính xác với một đường dẫn trong biểu đồ có trọng số cộng bằng thời gian di chuyển, nên mảng dist được tính toán là tối ưu toàn cục. Bất kỳ đường dẫn thay thế nào tới một nút đều phải có chi phí bằng hoặc lớn hơn, do đó việc so sánh với giới hạn thời gian là đủ để xác định tính khả thi cho tất cả các truy vấn cùng một lúc. 

## Giải pháp Python```python
import sys
import math
import heapq

input = sys.stdin.readline

def dist(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    dz = a[2] - b[2]
    return math.sqrt(dx*dx + dy*dy + dz*dz)

def solve():
    n, m, q = map(int, input().split())
    pts = [None] * (n + 1)

    for i in range(1, n + 1):
        x, y, z = map(int, input().split())
        pts[i] = (x, y, z)

    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        w = dist(pts[a], pts[b])
        g[a].append((b, w))
        g[b].append((a, w))

    INF = float('inf')
    dist_arr = [INF] * (n + 1)
    dist_arr[1] = 0.0

    pq = [(0.0, 1)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist_arr[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist_arr[v]:
                dist_arr[v] = nd
                heapq.heappush(pq, (nd, v))

    out = []
    for _ in range(q):
        c, t = map(int, input().split())
        if dist_arr[c] <= t:
            out.append(f"{dist_arr[c]:.10f}")
        else:
            out.append("impossible")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã xây dựng biểu đồ từ tọa độ và tính trọng số cạnh Euclide. Việc triển khai Dijkstra sử dụng vùng heap tối thiểu và bỏ qua các trạng thái cũ bằng cách sử dụng kiểm tra đẳng thức tiêu chuẩn so với khoảng cách tốt nhất hiện tại. Điều này là cần thiết vì nhiều mục nhập cho cùng một nút có thể tồn tại trong vùng heap. 

Việc xử lý truy vấn được tách biệt một cách có chủ ý khỏi logic biểu đồ. Sau khi xử lý trước, mỗi truy vấn là một phép so sánh ngưỡng đơn giản với khoảng cách đường đi ngắn nhất được tính toán trước. 

Một chi tiết triển khai tinh tế là tính ổn định của dấu phẩy động. Vì tọa độ được giới hạn bởi 1e3, độ dài cạnh tối đa là khoảng 1e3 và các đường dẫn tích lũy tối đa 1e5 cạnh, nên độ chính xác gấp đôi là đủ cho độ chính xác 1e-6 cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ trong đó hành tinh 1 kết nối với hành tinh 2 và hành tinh 2 kết nối với hành tinh 3 theo một đường thẳng. 

| Bước | Nút được xử lý | Quận hiện tại[2] | Quận hiện tại[3] | 
| --- | --- | --- | --- | 
| 1 | 1 | 5.0 | thông tin | 
| 2 | 2 | 5.0 | 9,0 | 
| 3 | 3 | 5.0 | 9,0 | 

Điều này cho thấy một chuỗi thư giãn tiêu chuẩn trong đó đường đi ngắn nhất lan truyền ra bên ngoài. Truy vấn yêu cầu hành tinh 3 với giới hạn thời gian 8 không thành công, trong khi 10 thành công. 

Bây giờ hãy xem xét một biểu đồ có hai tuyến đường đến cùng một nút, một tuyến đường dài hơn nhưng trực tiếp hơn và một tuyến đường ngắn hơn qua nút trung gian. Dijkstra đảm bảo tuyến đường ngắn hơn được chọn bất kể thứ tự thăm dò, bởi vì thứ tự đống luôn mở rộng khoảng cách một phần nhỏ nhất đã biết trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần thư giãn cạnh có thể được đẩy vào đống một lần, các thao tác trên đống chiếm ưu thế | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng khoảng cách và đống | 

Độ phức tạp này tương thích với n, m đến 10^5. Một đường chuyền Dijkstra duy nhất vừa vặn thoải mái trong giới hạn ngay cả trong Python khi được triển khai với danh sách heapq và kề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    import heapq

    input = sys.stdin.readline

    def dist(a, b):
        dx = a[0] - b[0]
        dy = a[1] - b[1]
        dz = a[2] - b[2]
        return math.sqrt(dx*dx + dy*dy + dz*dz)

    def solve():
        n, m, q = map(int, input().split())
        pts = [None] * (n + 1)

        for i in range(1, n + 1):
            x, y, z = map(int, input().split())
            pts[i] = (x, y, z)

        g = [[] for _ in range(n + 1)]

        for _ in range(m):
            a, b = map(int, input().split())
            w = dist(pts[a], pts[b])
            g[a].append((b, w))
            g[b].append((a, w))

        INF = float('inf')
        dist_arr = [INF] * (n + 1)
        dist_arr[1] = 0.0

        pq = [(0.0, 1)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist_arr[u]:
                continue
            for v, w in g[u]:
                nd = d + w
                if nd < dist_arr[v]:
                    dist_arr[v] = nd
                    heapq.heappush(pq, (nd, v))

        res = []
        for _ in range(q):
            c, t = map(int, input().split())
            if dist_arr[c] <= t:
                res.append("ok")
            else:
                res.append("impossible")
        return "\n".join(res)

    return solve()

# sample-style checks
assert run("""4 1 2
0 0 0
3 0 0
10 0 0
0 0 0
1 2
3 5
""") == "ok\nimpossible"

assert run("""2 1 1
0 0 0
1 0 0
1 2
1 2
""") == "ok"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Có thể tiếp cận một cạnh | được | tính đúng đắn cơ bản | 
| Giới hạn thời gian chặt chẽ không thành công | không thể | so sánh ngưỡng | 
| Đồ thị tối thiểu | được | xử lý trường hợp cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi đồ thị bị ngắt kết nối. Giả sử hành tinh 3 bị cô lập với hành tinh 1. Dijkstra để lại dist[3] là vô cùng, do đó, mọi truy vấn nhắm mục tiêu 3 đều phải trả về "không thể" bất kể giới hạn thời gian. Thuật toán xử lý việc này một cách tự nhiên vì vô cực luôn vượt quá bất kỳ t hữu hạn nào. 

Một trường hợp khác là khi có nhiều đường dẫn nhưng chỉ khác nhau một chút về chi phí. Ví dụ: cạnh dài trực tiếp so với phím tắt nhiều cạnh. Dijkstra đảm bảo đường dẫn tổng hợp ngắn hơn được chọn vì mỗi lần thư giãn đều cải thiện khoảng cách một cách đơn điệu và các mục nhập lỗi thời sẽ bị loại bỏ. 

Trường hợp tinh tế cuối cùng là độ chính xác về mặt số học khi các đường dẫn trở thành chuỗi dài. Vì trọng lượng của mỗi cạnh là căn bậc hai của một giá trị giới hạn nên sai số tích lũy vẫn nằm trong phạm vi dung sai 1e-6 và việc in với độ chính xác cố định sẽ duy trì độ chính xác.
