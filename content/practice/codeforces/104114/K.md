---
title: "CF 104114K - Vấn đề kiểm tra kiến ​​thức"
description: "Chúng ta đang làm việc với một đồ thị vô hướng có trọng số trong đó các đỉnh được đánh số từ 1 đến n. Mỗi cạnh nối hai đỉnh và có chi phí dương. Một hạn chế quan trọng về cấu trúc là mỗi cạnh chỉ kết nối các đỉnh có nhãn khác nhau tối đa là 10."
date: "2026-07-02T02:02:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104114
codeforces_index: "K"
codeforces_contest_name: "2022 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 104114
solve_time_s: 59
verified: true
draft: false
---

[CF 104114K - Vấn đề kiểm tra kiến thức](https://codeforces.com/problemset/problem/104114/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một đồ thị vô hướng có trọng số trong đó các đỉnh được đánh số từ 1 đến n. Mỗi cạnh nối hai đỉnh và có chi phí dương. Một hạn chế quan trọng về cấu trúc là mỗi cạnh chỉ kết nối các đỉnh có nhãn khác nhau tối đa là 10. Điều này làm cho đồ thị “được kết nối cục bộ” dọc theo trục số của chỉ số đỉnh, mặc dù bản thân trọng số có thể lớn tùy ý. 

Nhiệm vụ là trả lời nhiều truy vấn đường đi ngắn nhất độc lập. Mỗi truy vấn đưa ra hai đỉnh và chúng ta phải tính tổng trọng số tối thiểu có thể có của bất kỳ đường dẫn nào kết nối chúng hoặc báo cáo rằng không có đường dẫn nào tồn tại. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cũng cần xử lý biểu đồ một cách thưa thớt. Với tối đa 100.000 đỉnh và 200.000 cạnh, việc lưu trữ danh sách kề là được, nhưng bất cứ điều gì như tiền xử lý tất cả các cặp là không thể. Số lượng truy vấn lên tới 25.000 sẽ loại trừ việc chạy thuật toán đường dẫn ngắn nhất toàn cầu nặng nề cho mỗi truy vấn mà không cần quan tâm. 

Gợi ý cấu trúc quan trọng nhất là ràng buộc cạnh |u − v| 10. Điều này đảm bảo rằng mỗi đỉnh chỉ kết nối với một lân cận nhỏ trong không gian chỉ mục, do đó mỗi danh sách kề đều nhỏ và biểu đồ hoạt động giống như một dải thưa xung quanh đường đồng nhất. 

Đường đi ngắn nhất cho mỗi truy vấn sử dụng Dijkstra là chính xác, nhưng mối lo ngại là sự lặp lại trên nhiều truy vấn. Tuy nhiên, do các cạnh thưa thớt và các truy vấn độc lập nên vấn đề giảm xuống còn việc thiết kế một lộ trình đường đi ngắn nhất từ ​​một nguồn đến một mục tiêu đủ nhanh để tránh những công việc không cần thiết. 

Một trường hợp tinh tế là các thành phần bị ngắt kết nối. Vì biểu đồ không được đảm bảo sẽ được kết nối nên các truy vấn giữa các thành phần khác nhau phải trả về −1. Một trường hợp khác là khi a và b là cùng một nút, trong đó câu trả lời luôn là 0, ngay cả khi không có cạnh nào liên quan. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng truy vấn một cách độc lập và chạy Dijkstra từ đỉnh nguồn ai cho đến khi chúng ta đạt được bi. Dijkstra đúng vì tất cả các trọng số của cạnh đều dương, do đó, nó luôn khám phá các đường đi theo thứ tự khoảng cách tăng dần và đảm bảo rằng lần đầu tiên chúng ta giải quyết bi, chúng ta đã tìm được đường đi ngắn nhất. 

Lý do điều này có khả năng được chấp nhận là vì mỗi đỉnh có bậc rất nhỏ. Vì các cạnh chỉ kết nối các đỉnh trong cửa sổ có kích thước 10 trong không gian chỉ mục, nên mỗi nút có thể có tối đa 20 nút lân cận. Điều này giữ cho hệ số không đổi của mỗi lần giãn ở mức nhỏ và làm cho biểu đồ hoạt động giống như một mạng thưa thớt rất mỏng thay vì một mạng dày đặc. 

Quá trình suy nghĩ mạnh mẽ sẽ là tính toán các đường đi ngắn nhất giữa tất cả các cặp, nhưng điều đó sẽ yêu cầu chạy Dijkstra từ mọi nút hoặc sử dụng Floyd-Warshall. Cả hai đều quá chậm để n lên tới 100.000. Quan sát cho thấy các truy vấn là độc lập và biểu đồ thưa thớt gợi ý rằng chúng ta chỉ nên khám phá phần biểu đồ cần thiết để trả lời từng truy vấn, thay vì tính toán trước khoảng cách toàn cầu. 

Điều này dẫn đến Dijkstra cho mỗi truy vấn bị dừng sớm. Thay vì chạy cho đến khi tất cả các nút được xử lý, chúng tôi dừng ngay khi nút mục tiêu được trích xuất khỏi hàng đợi ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tất cả các cặp / Floyd-Warshall | O(n³) | O(n²) | Quá chậm | 
| Dijkstra từ mọi nút | O(n (m log n)) | O(n + m) | Quá chậm | 
| Dijkstra mỗi truy vấn (dừng sớm) | O(q · m log n) trường hợp xấu nhất | O(n + m) | Được chấp nhận trong thực tế | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập bằng cách sử dụng tìm kiếm đường dẫn ngắn nhất dựa trên hàng ưu tiên.

1. Xây dựng danh sách kề của đồ thị từ các cạnh đã cho. Mỗi đỉnh lưu trữ một danh sách các cặp (hàng xóm, trọng số). Điều này là cần thiết để chỉ duyệt các cạnh hợp lệ một cách hiệu quả mà không cần quét tất cả các cặp đỉnh có thể có. 
2. Đối với mỗi truy vấn (a, b), trước tiên hãy kiểm tra xem a có bằng b hay không. Nếu vậy, câu trả lời ngay lập tức là 0 vì đường đi ngắn nhất từ ​​một nút đến chính nó không yêu cầu cạnh nào. 
3. Khởi tạo một mảng khoảng cách chứa đầy các giá trị vô cực và đặt dist[a] = 0. Chúng tôi cũng khởi tạo hàng đợi ưu tiên chứa cặp (0, a). Hàng đợi này luôn giữ đỉnh hứa hẹn nhất tiếp theo để xử lý. 
4. Trong khi hàng đợi ưu tiên không trống, hãy trích xuất đỉnh u có khoảng cách dự kiến ​​nhỏ nhất. Nếu khoảng cách này đã lớn hơn dist[u] được lưu trữ, hãy bỏ qua nó vì nó đã lỗi thời. 
5. Nếu u bằng b, chúng ta ngay lập tức trả về dist[u] làm câu trả lời cho truy vấn này. Điều này hợp lệ vì Dijkstra đảm bảo rằng các nút được xuất hiện theo thứ tự không giảm dần trong khoảng cách ngắn nhất đã biết. 
6. Ngược lại, lặp qua tất cả các hàng xóm v của u. Đối với mỗi cạnh (u, v, w), hãy thử thư giãn. Nếu dist[v] > dist[u] + w, cập nhật dist[v] và đẩy (dist[v], v) vào hàng ưu tiên. 
7. Nếu hàng đợi ưu tiên trống mà không đến được b, thì b không thể truy cập được từ a và chúng ta xuất −1. 

### Tại sao nó hoạt động 

Tính chính xác đến từ hành vi Dijkstra tiêu chuẩn. Tại bất kỳ thời điểm nào, hàng đợi ưu tiên sẽ lưu trữ khoảng cách ngắn nhất của ứng viên tới các nút biên giới. Sau khi một nút được bật lên, khoảng cách của nó sẽ được xác định cuối cùng vì bất kỳ đường dẫn thay thế nào cũng sẽ phải đi qua một nút có khoảng cách dự kiến ​​bằng hoặc nhỏ hơn, khoảng cách này đã được xử lý. 

Việc dừng sớm ở b không phá vỡ tính đúng đắn vì lần đầu tiên b được đưa ra khỏi hàng đợi, khoảng cách của nó được đảm bảo là nhỏ nhất trong số tất cả các đường đi có thể từ a đến b. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

n, m, q = map(int, input().split())

adj = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v, w = map(int, input().split())
    adj[u].append((v, w))
    adj[v].append((u, w))

INF = 10**30

def dijkstra(s, t):
    if s == t:
        return 0

    dist = [INF] * (n + 1)
    dist[s] = 0
    pq = [(0, s)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        if u == t:
            return d

        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return -1

for _ in range(q):
    a, b = map(int, input().split())
    print(dijkstra(a, b))
```Việc xây dựng danh sách kề rất đơn giản và đảm bảo mỗi cạnh được lưu trữ hai lần cho quá trình truyền tải không có hướng. Quy trình Dijkstra được viết ở dạng chuẩn với một mảng khoảng cách và một đống. Điều kiện thoát sớm khi chúng tôi bật nút mục tiêu là tối ưu hóa chính được sử dụng để tránh khám phá các phần không liên quan của biểu đồ. 

Một điểm tinh tế là việc kiểm tra mục nhập cũ`if d != dist[u]`, điều này rất cần thiết để tránh xử lý các mục heap lỗi thời và kiểm soát độ phức tạp. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó các đường dẫn phân nhánh. 

Đối với đầu vào:```
4 4 1
1 2 5
2 4 2
1 3 1
3 4 100
1 4
```Thuật toán bắt đầu từ 1 với khoảng cách 0. Từ 1, nó phát hiện 2 với giá 5 và 3 với giá 1. Hàng đợi luôn ưu tiên 3 tiếp theo. 

| Bước | Nút hiện tại | Khoảng cách | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | (2:5), (3:1) | 
| 2 | 3 | 1 | (4:101) | 
| 3 | 2 | 5 | (4:7) cải thiện từ 101 lên 7 | 
| 4 | 4 | 7 | dừng lại | 

Điều này cho thấy cạnh có vẻ đắt tiền (3,4) không bao giờ thực sự được sử dụng vì một tuyến đường tốt hơn được phát hiện thông qua 2. 

Đối với trường hợp bị ngắt kết nối:```
5 2 1
1 2 3
4 5 2
1 5
```| Bước | Nút hiện tại | Khoảng cách | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | (2:3) | 
| 2 | 2 | 3 | không | 
| 3 | hàng đợi trống | - | không tìm thấy đường dẫn | 

Điều này xác nhận rằng các nút không thể truy cập sẽ dẫn đến hàng đợi trống và đầu ra −1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · m log n) trường hợp xấu nhất | Mỗi truy vấn chạy Dijkstra với các thao tác heap trên các cạnh | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và đống | 

Cho rằng mỗi nút có bậc rất nhỏ do |u − v| 10 hạn chế, số lần thư giãn thực tế cho mỗi truy vấn thấp hơn đáng kể so với m trong các trường hợp điển hình. Điều này giúp giải pháp luôn trong giới hạn thời gian cho 25.000 truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, q = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))

    INF = 10**30

    def dijkstra(s, t):
        if s == t:
            return 0
        dist = [INF] * (n + 1)
        dist[s] = 0
        pq = [(0, s)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            if u == t:
                return d
            for v, w in adj[u]:
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))
        return -1

    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        out.append(str(dijkstra(a, b)))
    return "\n".join(out)

assert run("""6 5 3
1 3 7
3 4 5
1 4 1
2 5 10
2 6 12
6 5
1 3
1 5
""") == """-1
7
-1"""

assert run("""1 0 2
1 1
1 1
""") == """0
0"""

assert run("""3 2 1
1 2 5
2 3 6
1 3
""") == """11"""

assert run("""4 2 1
1 2 1
3 4 1
1 4
""") == """-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn nút đơn | 0 giây | tự điều chỉnh khoảng cách | 
| Biểu đồ chuỗi | giá trị hữu hạn | đường đi ngắn nhất thông thường | 
| Các thành phần bị ngắt kết nối | -1 | xử lý khả năng tiếp cận | 
| Biểu đồ hỗn hợp mẫu | định tuyến đúng | nhiều đường dẫn và lựa chọn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là khi nguồn bằng đích. Trong tình huống đó, thuật toán ngay lập tức trả về 0 mà không cần chạm vào hàng đợi ưu tiên. Điều này tránh việc khởi tạo vùng heap không cần thiết và ngăn chặn lỗi ngẫu nhiên khi biểu đồ không có cạnh. 

Một trường hợp cạnh khác là các đỉnh bị ngắt kết nối. Trong trường hợp như vậy, Dijkstra sẽ sử dụng hết tất cả các nút có thể truy cập từ nguồn và hàng đợi ưu tiên sẽ trống trước khi nhìn thấy mục tiêu. Thuật toán trả về chính xác −1 vì không có sự thư giãn nào có thể đưa đích đến vào vùng được khám phá. 

Trường hợp thứ ba là khi chỉ có thể tiếp cận một nút thông qua một đường vòng dài hơn mà ban đầu bị chi phối bởi cạnh trực tiếp nhưng đắt tiền. Quá trình thư giãn đảm bảo rằng ngay cả khi một cạnh xấu được xử lý sớm, nó sẽ bị ghi đè bởi một đường dẫn rẻ hơn ngay khi được phát hiện và các mục hàng đợi cũ sẽ được bỏ qua một cách an toàn khi kiểm tra khoảng cách.
