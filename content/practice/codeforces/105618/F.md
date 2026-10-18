---
title: "CF 105618F - \u041a\u0442\u043e \u0445\u043e\u0447\u0435\u0442 \u0441\u0442\u0430\u0442\u044c \u043d\u0435\u043c\u0430\u043b\u043e\u043d\u044c\u0435\u0440\u043e\u043c?"
description: "Chúng ta được cung cấp một đồ thị vô hướng có trọng số với các phòng $n$ và các đường hầm hai chiều $m$. Mỗi đường hầm có một chi phí đi lại. Chúng tôi luôn bắt đầu từ phòng $1$ và với mỗi phòng $v$, chúng tôi muốn đạt được chi phí tối thiểu."
date: "2026-06-26T18:19:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105618
codeforces_index: "F"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2024-2025. \u0422\u0440\u0435\u0442\u0438\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 105618
solve_time_s: 68
verified: true
draft: false
---

[CF 105618F - \u041a\u0442\u043e \u0445\u043e\u0447\u0435\u0442 \u0441\u0442\u0430\u0442\u044c \u043d\u0435\u043c\u0430\u043b\u043e\u043d\u044c\u0435\u0440\u043e\u043c?](https://codeforces.com/problemset/problem/105618/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số với$n$phòng và$m$đường hầm hai chiều. Mỗi đường hầm có một chi phí đi lại. Chúng tôi luôn bắt đầu từ phòng$1$, và cho mỗi phòng$v$chúng tôi muốn chi phí tối thiểu để đạt được nó. 

Twist là một khả năng đặc biệt duy nhất gắn liền với mỗi phòng$a$: nếu chúng ta hiện đang ở trong phòng$a$, chúng ta có thể ngay lập tức tạo một đường hầm mới từ$a$đến bất kỳ phòng nào khác$v$với chi phí$c_a$. Đường hầm nhân tạo này chỉ có thể được sử dụng một lần dọc theo bất kỳ tuyến đường nào. Nó hoạt động giống như một “cạnh dịch chuyển” mà chi phí chỉ phụ thuộc vào căn phòng nơi nó được kích hoạt. 

Điều quan trọng là khả năng này không mang tính toàn cầu. Đối với mỗi điểm đến$v$, chúng ta được phép tưởng tượng việc sử dụng dịch chuyển tức thời này nhiều nhất một lần ở đâu đó dọc theo con đường từ$1$ĐẾN$v$, nhưng các điểm đến khác nhau có thể chọn kích hoạt dịch chuyển tức thời khác nhau. 

Vì vậy, đối với một mục tiêu cố định$v$, một tuyến đường hợp lệ là một đường đi ngắn nhất thông thường trong biểu đồ gốc hoặc một đường dẫn mà tại một thời điểm nào đó sẽ ghé thăm một nút$a$, sau đó nhảy trực tiếp đến$v$trả chi phí$c_a$. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số đỉnh và cạnh trong tất cả các trường hợp thử nghiệm, điều này đẩy chúng ta tới$O((n+m)\log n)$hoặc các giải pháp tuyến tính. Bất kỳ ý tưởng nào cố gắng tính toán lại các đường đi ngắn nhất từ ​​đầu cho mỗi nút sẽ là quá chậm, vì ngay cả một Dijkstra trên mỗi nút cũng sẽ bùng nổ thành$O(nm \log n)$. 

Trường hợp cạnh tinh tế xuất hiện khi biểu đồ bị ngắt kết nối. Nếu không dịch chuyển tức thời, một số nút sẽ không thể truy cập được. Với dịch chuyển tức thời, họ vẫn có thể truy cập được nếu bất kỳ nút nào có thể truy cập được$a$cung cấp một giới hạn$c_a$, bởi vì chúng ta có thể nhảy trực tiếp từ$a$tới bất kỳ đỉnh nào. 

Một trường hợp khác là khi dịch chuyển tức thời tệ hơn các đường dẫn thông thường ở mọi nơi. Sau đó, nó phải được bỏ qua hoàn toàn và các câu trả lời giảm xuống các đường dẫn ngắn nhất tiêu chuẩn. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải bài toán là tính đường đi ngắn nhất từ$1$sử dụng thuật toán của Dijkstra và sau đó thử độc lập tất cả các cách sử dụng dịch chuyển tức thời có thể. Đối với một cặp cố định$(a, v)$, chúng ta có thể xem xét một đường đi từ$1$ĐẾN$a$, trả tiền$c_a$, và kết thúc tại$v$. Điều đó gợi ý nên kiểm tra tất cả các lựa chọn trung gian$a$cho mọi$v$, dẫn đến$O(n^2)$sự kết hợp trên tính toán đường đi ngắn nhất. Ngay cả khi bỏ qua các cạnh của đồ thị, nó vẫn trở nên quá lớn. 

Sự đơn giản hóa chính là tách biệt cấu trúc của dịch chuyển tức thời. Khi chúng tôi đến một nút nào đó$a$, việc dịch chuyển không còn phụ thuộc vào điểm đến nữa. Nó luôn cho phép chúng ta nhảy trực tiếp đến nút cuối cùng. Điều này có nghĩa là toàn bộ “giai đoạn dịch chuyển tức thời” thu gọn thành một giá trị ứng cử viên toàn cầu duy nhất:$$\min_a (\text{dist}[a] + c_a)$$Ở đâu$\text{dist}[a]$là khoảng cách ngắn nhất từ$1$ĐẾN$a$trong biểu đồ gốc. 

Giá trị này thể hiện cách rẻ nhất có thể để tiếp cận bất kỳ nút nào bằng cách sử dụng chính xác một dịch chuyển. Vì dịch chuyển tức thời đưa chúng ta trực tiếp đến đích nên ứng cử viên này áp dụng thống nhất cho tất cả các nút. 

Do đó, vấn đề giảm xuống còn việc tính toán đường đi ngắn nhất một nguồn từ nút$1$, sau đó tính toán mức tối thiểu toàn cầu trên tất cả các nút và cuối cùng so sánh giá trị đó với từng nút$\text{dist}[v]$. 

Brute-force không thành công vì nó cố gắng suy luận về các điểm đến dịch chuyển tức thời riêng lẻ. Quan sát đúng là lựa chọn dịch chuyển tức thời chỉ phụ thuộc vào nút kích hoạt chứ không phải mục tiêu, do đó tác dụng của nó có thể được tóm tắt bằng một đại lượng vô hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại hoặc thử tất cả các cặp dịch chuyển |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Dijkstra + hợp nhất tối thiểu toàn cầu |$O((n+m)\log n)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp theo hai giai đoạn độc lập. 

1. Chạy Dijkstra từ nút$1$trên đồ thị ban đầu và tính toán$\text{dist}[v]$cho tất cả các đỉnh. Điều này mang lại chi phí tốt nhất mà không cần sử dụng dịch chuyển tức thời. Lý do điều này hợp lệ là vì tất cả các đường hầm ban đầu đều có các cạnh có trọng số tiêu chuẩn, do đó Dijkstra nắm bắt đầy đủ chuyển động tối ưu trước bất kỳ hoạt động đặc biệt nào. 
2. Trong khi xử lý các nút trong hoặc sau Dijkstra, hãy tính giá trị$\text{dist}[a] + c_a$cho mỗi nút$a$. Duy trì mức tối thiểu của biểu thức này trên tất cả các nút. Điều này thể hiện “chi phí kích hoạt dịch chuyển tức thời” tốt nhất có thể. 
3. Đối với mọi nút$v$, tính kết quả cuối cùng là:$$\min(\text{dist}[v], \text{bestTeleport})$$bởi vì hoặc là chúng tôi không bao giờ sử dụng dịch chuyển tức thời, hoặc chúng tôi sử dụng nó một lần và nó ngay lập tức kết thúc hành trình ở$v$. 
4. Xuất các giá trị này cho tất cả các nút. 

Điểm cấu trúc quan trọng là dịch chuyển tức thời không tạo ra một đường truyền đồ thị phân lớp mới. Nó chỉ giới thiệu một tùy chọn phím tắt chung duy nhất. 

### Tại sao nó hoạt động 

Bất kỳ đường dẫn hợp lệ nào từ$1$ĐẾN$v$sử dụng dịch chuyển tức thời trước tiên phải đi theo các cạnh bình thường từ$1$đến một nút nào đó$a$, sau đó áp dụng dịch chuyển tức thời và dừng lại. Chi phí của một con đường như vậy chính xác là$\text{dist}[a] + c_a$. Vì chúng tôi đã tính toán con đường ngắn nhất có thể để tiếp cận mọi$a$, việc thay thế tiền tố bằng bất cứ điều gì tệ hơn sẽ không thể cải thiện kết quả. Do đó, mọi đường dẫn dựa trên dịch chuyển được biểu thị bằng chính xác một trong các giá trị$\text{dist}[a] + c_a$, và lấy giá trị nhỏ nhất trên tất cả$a$nắm bắt mọi khả năng. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def dijkstra(n, adj):
    dist = [INF] * (n + 1)
    dist[1] = 0
    pq = [(0, 1)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    c = [0] + list(map(int, input().split()))

    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))

    dist = dijkstra(n, adj)

    best_teleport = INF
    for i in range(1, n + 1):
        if dist[i] < INF:
            best_teleport = min(best_teleport, dist[i] + c[i])

    res = []
    for v in range(1, n + 1):
        if v == 1:
            res.append("0")
        else:
            ans = min(dist[v], best_teleport)
            res.append(str(ans))

    print(" ".join(res))
```Đoạn mã đầu tiên xây dựng biểu đồ và tính toán các đường đi ngắn nhất từ ​​nút$1$. Dijkstra dựa trên heap đảm bảo xử lý chính xác trọng lượng cạnh lớn lên đến$10^9$. 

Lượt thứ hai tính toán điểm neo dịch chuyển tốt nhất bằng cách quét tất cả các nút một lần. Một sai lầm phổ biến là cố gắng áp dụng dịch chuyển tức thời trong quá trình thư giãn Dijkstra; điều đó là không cần thiết vì dịch chuyển tức thời không phụ thuộc vào trạng thái đích. 

Cuối cùng, mỗi câu trả lời là một so sánh đơn giản giữa đường đi ngắn nhất trực tiếp và đường tắt dịch chuyển tức thời toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một biểu đồ nhỏ trong đó nút$1$kết nối với nút$2$và nút$2$nhánh ra. 

Chúng tôi tính toán các đường đi ngắn nhất: 

| Nút | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 6 | 

Bây giờ tính toán các ứng cử viên dịch chuyển tức thời$dist[a] + c[a]$: 

| một | quận [a] | c[a] | tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 0 | 100 | 100 | 
| 2 | 1 | 4 | 5 | 
| 3 | 3 | 3 | 6 | 
| 4 | 6 | 2 | 8 | 

Vì vậy, dịch chuyển tức thời tốt nhất là$5$. 

Câu trả lời cuối cùng: 

| Nút | trực tiếp | dịch chuyển tức thời | kết quả | 
| --- | --- | --- | --- | 
| 1 | 0 | 5 | 0 | 
| 2 | 1 | 5 | 1 | 
| 3 | 3 | 5 | 3 | 
| 4 | 6 | 5 | 5 | 

Điều này cho thấy dịch chuyển tức thời chỉ quan trọng đối với các nút có đường dẫn bình thường tệ hơn lối tắt chung. 

### Ví dụ 2 

Biểu đồ bị ngắt kết nối: 

Nút$1$bị cô lập ngoại trừ không có cạnh nào, nhưng vẫn tồn tại các giá trị dịch chuyển. 

| Nút | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | INF | 
| 3 | INF | 

Giá trị dịch chuyển: 

| một | dist[a] + c[a] | 
| --- | --- | 
| 1 | 10 | 
| 2 | INF | 
| 3 | INF | 

Vì vậy, dịch chuyển tức thời tốt nhất là$10$. Mọi nút đều có thể truy cập được với chi phí$10$, mặc dù đồ thị bị ngắt kết nối. 

Điều này xác nhận dịch chuyển tức thời hoạt động như một công cụ tăng cường kết nối toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log n)$| Dijkstra thống trị; mỗi cạnh được nới lỏng một lần bằng thao tác heap | 
| Không gian |$O(n+m)$| danh sách kề cộng với khoảng cách và lưu trữ đống | 

Giới hạn cho phép lên tới$2 \cdot 10^5$tổng số cạnh, do đó, một Dijkstra duy nhất cho mỗi bộ thử nghiệm sẽ phù hợp một cách thoải mái trong giới hạn về thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**30

    def dijkstra(n, adj):
        dist = [INF] * (n + 1)
        dist[1] = 0
        pq = [(0, 1)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            for v, w in adj[u]:
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))
        return dist

    t = int(input())
    out_lines = []
    for _ in range(t):
        n, m = map(int, input().split())
        c = [0] + list(map(int, input().split()))
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v, w = map(int, input().split())
            adj[u].append((v, w))
            adj[v].append((u, w))

        dist = dijkstra(n, adj)

        best = INF
        for i in range(1, n + 1):
            if dist[i] < INF:
                best = min(best, dist[i] + c[i])

        res = []
        for v in range(1, n + 1):
            if v == 1:
                res.append("0")
            else:
                res.append(str(min(dist[v], best)))
        out_lines.append(" ".join(res))

    return "\n".join(out_lines)

# sample-like test
assert run("""1
3 2
5 1 10
1 2 1
2 3 1
""") == "0 1 2"

# disconnected graph
assert run("""1
3 0
10 100 100
""") == "0 10 10"

# teleport worse than edges
assert run("""1
3 2
100 100 100
1 2 1
2 3 1
""") == "0 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tuyến tính |`0 1 2`| độ chính xác Dijkstra bình thường | 
| không có cạnh |`0 10 10`| kết nối dịch chuyển tức thời | 
| giá trị c lớn |`0 1 2`| dịch chuyển bị bỏ qua khi dưới mức tối ưu | 

## Vỏ cạnh 

Biểu đồ bị ngắt kết nối nêu bật vai trò chính của dịch chuyển tức thời: nó thay thế những khoảng cách không thể tiếp cận bằng mức tối thiểu toàn cầu hữu hạn. Trong trường hợp như vậy, Dijkstra tạo ra vô số nút, nhưng việc tổng hợp dịch chuyển tức thời vẫn mang lại giá trị có thể sử dụng được vì nó chỉ phụ thuộc vào các nút có thể truy cập được$a$. 

Khi tất cả$c_a$là cực kỳ lớn, thuật toán tự nhiên quay trở lại các đường đi ngắn nhất tiêu chuẩn kể từ$\min(\text{dist}[v], \text{bestTeleport})$không bao giờ chọn thời hạn dịch chuyển tức thời. 

Biểu đồ một nút không quan trọng nhưng quan trọng đối với tính chính xác của việc triển khai: nút$1$phải luôn xuất ra$0$, mặc dù tính toán dịch chuyển tức thời sẽ gợi ý một giá trị khác 0.
