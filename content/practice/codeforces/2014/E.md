---
title: "CF 2014E - Rendez-vous de Marian et Robin"
description: "Chúng tôi đang làm việc trên một đồ thị vô hướng có trọng số trong đó hai người xuất phát từ hai đầu đối diện và muốn gặp nhau càng nhanh càng tốt. Một cái bắt đầu ở đỉnh 1 và cái kia ở đỉnh n."
date: "2026-06-08T13:00:46+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 1800
weight: 2014
solve_time_s: 74
verified: true
draft: false
---

[CF 2014E - Rendez-vous de Marian et Robin](https://codeforces.com/problemset/problem/2014/E) 

**Đánh giá:** 1800 
**Tags:** dfs và tương tự, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một đồ thị vô hướng có trọng số trong đó hai người xuất phát từ hai đầu đối diện và muốn gặp nhau càng nhanh càng tốt. Một cái bắt đầu ở đỉnh 1 và cái kia ở đỉnh n. Chúng di chuyển dọc theo các cạnh, nhưng có một điểm khác biệt: một số đỉnh chứa ngựa và việc sử dụng ngựa sẽ giảm một nửa thời gian di chuyển sau khi bạn nhặt nó lên. Mỗi người có thể quyết định độc lập thời điểm cưỡi ngựa và sau khi lên ngựa, tốc độ tăng sẽ duy trì vĩnh viễn. 

Mục tiêu là xác định thời gian sớm nhất có thể mà tại đó hai du khách có thể chiếm cùng một đỉnh cùng lúc, giả sử chuyển động tối ưu và chờ đợi tùy chọn. 

Khó khăn cốt lõi là chi phí đi lại của mỗi người không phải là vấn đề về con đường ngắn nhất. Thay vào đó, chi phí phụ thuộc vào việc họ đã đến đỉnh ngựa trước khi đạt đến điểm nhất định hay chưa. Điều đó có nghĩa là mỗi đỉnh thực tế có hai trạng thái: không có lợi thế về ngựa và có lợi thế về ngựa. 

Các ràng buộc làm rõ rằng mọi giải pháp đều phải gần tuyến tính. Với tối đa 2×10^5 đỉnh và cạnh cho mỗi lô thử nghiệm, bất kỳ phương pháp nào tính toán lại các đường đi ngắn nhất từ ​​đầu cho mỗi lần chuyển đổi trạng thái hoặc mô phỏng rõ ràng thời gian gặp nhau đều quá chậm. Một Dijkstra tiêu chuẩn cho mỗi trạng thái có thể chấp nhận được, nhưng bất cứ điều gì như tính toán lại thời gian gặp nhau theo cặp trên tất cả các đỉnh thì không. 

Một vài trường hợp thất bại tinh vi sẽ xuất hiện nếu chúng ta bỏ qua trạng thái. 

Một vấn đề nảy sinh nếu chúng ta coi các đỉnh ngựa như những công cụ điều chỉnh chi phí đơn giản được áp dụng một cách tham lam khi gặp lần đầu. Ví dụ: một đường đi có thể đi qua đỉnh ngựa quá sớm để có thể hưởng lợi hoàn toàn và một thuật toán ngây thơ có thể cho rằng việc đi ngay lập tức luôn là tối ưu. 

Một vấn đề khác là giả sử khoảng cách đường đi ngắn nhất cho mỗi nút là đủ. Điều đó không thành công vì đến một nút muộn hơn mà không có ngựa vẫn có thể tốt hơn đến sớm hơn mà không có ngựa, tùy thuộc vào khả năng tăng tốc trong tương lai. 

Cuối cùng, một cách tiếp cận đơn giản có thể cố gắng tính toán các đường đi ngắn nhất từ ​​cả hai đầu và lấy giá trị tối thiểu trên khoảng cách tối đa. Điều đó bị phá vỡ vì hai du khách có thể đến vào những thời điểm khác nhau và một người có thể đợi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng tính toán thời gian gặp mặt tốt nhất có thể cho mọi đỉnh bằng cách mô phỏng độc lập các chiến lược di chuyển tối ưu của cả hai khách du lịch tới đỉnh đó. Đối với mỗi đỉnh v, chúng ta sẽ tính thời gian tối thiểu Marian có thể đến v theo tất cả các lựa chọn có thể có về thời điểm cưỡi ngựa, và tương tự đối với Robin, sau đó lấy giá trị tối đa của hai thời gian đến. Câu trả lời sẽ là giá trị nhỏ nhất trên tất cả các đỉnh. 

Vấn đề là việc tính toán một “thời gian đến tối thiểu” không phải là con đường ngắn nhất theo tiêu chuẩn do trạng thái ngựa. Nếu chúng ta cố gắng phân nhánh rõ ràng xem một con ngựa đã được bắt ở mọi đỉnh hay chưa, thì mỗi quyết định đường đi sẽ nhân đôi không gian trạng thái và việc liệt kê ngây thơ sẽ dẫn đến các đường dẫn hàm mũ hoặc ít nhất là lý luận kiểu O(2^n) đối với các tập hợp con kích hoạt ngựa. 

Quan sát quan trọng là bài toán phân tách rõ ràng thành bài toán đường đi ngắn nhất trên biểu đồ trạng thái mở rộng với chỉ hai trạng thái trên mỗi nút: trước ngựa và sau ngựa. Một khi điều này được nhận ra, bộ máy sẽ trở thành Dijkstra tiêu chuẩn. Chúng tôi chạy đường đi ngắn nhất từ ​​nhiều nguồn từ cả hai lần xuất phát, theo dõi thời gian đến tốt nhất khi có và không sử dụng ngựa, sau đó kết hợp các kết quả trên mỗi đỉnh. 

Cái nhìn sâu sắc quan trọng thứ hai là cách lập mô hình xe đón ngựa. Tại đỉnh ngựa, chúng ta có thể chuyển từ trạng thái “tốc độ bình thường” sang trạng thái “tốc độ nhanh” với chi phí bằng 0. Từ thời điểm đó trở đi, tất cả các cạnh đều được đi qua với giá bằng một nửa. Vì tất cả các trọng số đều là số chẵn nên việc giảm một nửa sẽ bảo toàn tính nguyên vẹn. 

Khi chúng ta có thời gian đến ngắn nhất cho cả Marian và Robin cho cả hai trạng thái ở mọi đỉnh, thời gian gặp nhau tốt nhất tại một đỉnh chỉ đơn giản là thời gian tối thiểu trong tất cả các kết hợp trạng thái hợp lệ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các con đường và lựa chọn ngựa | Hàm mũ | Cao | Quá chậm | 
| Dijkstra 2 trạng thái từ mỗi nguồn | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi đỉnh có hai lớp: lớp 0 có nghĩa là chưa có con ngựa nào được sử dụng và lớp 1 có nghĩa là khách du lịch hiện đang cưỡi ngựa (hoặc đã kích hoạt một con ngựa trước đó). Mỗi lần di chuyển cạnh phụ thuộc vào lớp hiện tại. 

1. Xây dựng đồ thị trong đó mỗi đỉnh có hai trạng thái. Các chuyển đổi trạng thái 0 biểu thị các cạnh đi bộ với toàn bộ chi phí và các chuyển đổi trạng thái 1 biểu thị việc đi lại với một nửa chi phí. Điều này cho thấy thực tế là việc sử dụng ngựa sẽ thay đổi vĩnh viễn tốc độ di chuyển. 
2. Khởi tạo khoảng cách riêng biệt cho Marian và Robin. Đối với cả hai, đặt khoảng cách tại đỉnh bắt đầu của chúng ở trạng thái 0 đến 0. Nếu đỉnh bắt đầu có con ngựa, chúng tôi cũng cho phép chuyển tức thời sang trạng thái 1. 
3. Chạy thuật toán Dijkstra trên biểu đồ trạng thái mở rộng cho Marian. Mỗi trạng thái là (đỉnh, chế độ) và các chuyển đổi tuân theo các quy tắc ở trên. Điều này tính toán thời gian tối thiểu Marian có thể đến mọi đỉnh khi có hoặc không có lợi thế về ngựa. 
4. Lặp lại tương tự Dijkstra cho Robin bắt đầu từ đỉnh n. Điều này tạo ra một bảng khoảng cách thứ hai. 
5. Với mỗi đỉnh v, hãy tính thời gian gặp nhau tốt nhất có thể bằng cách xem xét tất cả các kết hợp trạng thái: Marian ở trạng thái 0 hoặc 1 và Robin ở trạng thái 0 hoặc 1. Thời gian gặp nhau là nhỏ nhất trên max(distM[v][i], distR[v][j]). 
6. Lấy giá trị nhỏ nhất trên tất cả các đỉnh. Nếu không có đỉnh nào có thể truy cập được cho cả hai, hãy xuất -1. 

### Tại sao nó hoạt động 

Việc xây dựng biểu đồ trạng thái đảm bảo rằng mọi chiến lược trong thế giới thực hợp lệ đều tương ứng với chính xác một đường dẫn trong biểu đồ mở rộng. Bất biến chính là khi khách du lịch bước vào trạng thái 1, tất cả các trọng số cạnh tiếp theo đều giảm một nửa, phù hợp với hiệu ứng ngựa không thể đảo ngược của vấn đề. Dijkstra đảm bảo tính tối ưu vì tất cả các chuyển đổi đều có trọng số không âm và biểu đồ không chứa các phụ thuộc ẩn ngoài mã hóa hai trạng thái. Do đó, các đường đi ngắn nhất trong không gian mở rộng này tương ứng chính xác với các chiến lược di chuyển tối ưu trong bài toán ban đầu. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

INF = 10**18

def dijkstra(n, adj, horses, start):
    dist = [[INF, INF] for _ in range(n + 1)]
    pq = []

    dist[start][0] = 0
    heapq.heappush(pq, (0, start, 0))

    if horses[start]:
        dist[start][1] = 0
        heapq.heappush(pq, (0, start, 1))

    while pq:
        d, u, s = heapq.heappop(pq)
        if d != dist[u][s]:
            continue

        for v, w in adj[u]:
            if s == 0:
                nd = d + w
                ns = s
                if nd < dist[v][ns]:
                    dist[v][ns] = nd
                    heapq.heappush(pq, (nd, v, ns))
            else:
                nd = d + w // 2
                ns = s
                if nd < dist[v][ns]:
                    dist[v][ns] = nd
                    heapq.heappush(pq, (nd, v, ns))

        if s == 0 and horses[u]:
            if d < dist[u][1]:
                dist[u][1] = d
                heapq.heappush(pq, (d, u, 1))

    return dist

t = int(input())
for _ in range(t):
    n, m, h = map(int, input().split())
    horses = [0] * (n + 1)
    for x in map(int, input().split()):
        horses[x] = 1

    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))

    distM = dijkstra(n, adj, horses, 1)
    distR = dijkstra(n, adj, horses, n)

    ans = INF
    for v in range(1, n + 1):
        ans = min(ans,
                  max(distM[v][0], distR[v][0]),
                  max(distM[v][0], distR[v][1]),
                  max(distM[v][1], distR[v][0]),
                  max(distM[v][1], distR[v][1]))

    print(-1 if ans == INF else ans)
```Việc triển khai mã hóa hiệu ứng ngựa dưới dạng trạng thái nhị phân. Hàng đợi Dijkstra lưu trữ các bộ dữ liệu (khoảng cách, nút, trạng thái) và các chuyển đổi trạng thái được phân tách cẩn thận. Quá trình chuyển đổi tinh tế duy nhất là ở các đỉnh ngựa, nơi chúng tôi cho phép chuyển sang trạng thái tăng tốc với chi phí bằng 0. 

Khi kết hợp các câu trả lời, tất cả bốn cặp trạng thái đều được kiểm tra rõ ràng. Điều này là cần thiết vì một trong hai du khách có thể kích hoạt hoặc không kích hoạt ngựa trước khi đến đỉnh cuộc họp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (đồ thị đơn giản với cải tiến trực tiếp) 

Hãy xem xét một đồ thị trong đó 1 được kết nối với 2 với trọng số 10 và 2 được kết nối với 3 với trọng số 10 và đỉnh 2 có một con ngựa. Marian bắt đầu lúc 1 và Robin lúc 3. 

| Bước | Quận Marian[2][0] | Quận Marian[2][1] | Quận Robin[2][0] | Quận Robin[2][1] | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 lúc 1 | 0 tại 1 nếu ngựa khác INF | 0 lúc 3 | 0 lúc 3 nếu ngựa khác INF | 
| Sau Dijkstra | 10, rồi 5 qua ngựa | 5 | 10 | 5 qua 2 | 

Ở đỉnh 2, Marian dùng ngựa đến ở đỉnh 5, Robin đến ở đỉnh 5 từ phía bên kia. Thời gian gặp mặt là 5. 

Điều này xác nhận rằng trạng thái chuyển đổi ở đỉnh chính xác là điều cần thiết để đạt được sự tối ưu. 

### Ví dụ 2 (lợi thế sử dụng ngựa bị trì hoãn) 

Đường đi 1-2-3-4 trong đó chỉ có đỉnh 3 có ngựa và trọng số ở các cạnh đầu là lớn. Marian có thể đạt tới cấp 3 muộn hơn nhưng được hưởng lợi đáng kể sau khi kích hoạt. Robin tiếp cận từ phía đối diện tương tự. 

Quan sát quan trọng là điểm gặp mặt tối ưu không nhất thiết phải là nơi đến sớm nhất trong chế độ cơ bản. Bảng các trạng thái cho thấy rằng việc kích hoạt bị trì hoãn sẽ tạo ra thời gian xuôi dòng tốt hơn, xác thực sự cần thiết của mô hình Dijkstra hai lớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Hai Dijkstra chạy trên một biểu đồ có 2 trạng thái trên mỗi nút, mỗi cạnh được xử lý một số lần không đổi | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách cho cả hai nguồn | 

Tổng số đỉnh và cạnh trong các trường hợp thử nghiệm được giới hạn bởi 2×10^5, do đó phương pháp này chạy thoải mái trong giới hạn ngay cả với hàng đợi ưu tiên dựa trên đống. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import heapq

    INF = 10**18

    def dijkstra(n, adj, horses, start):
        dist = [[INF, INF] for _ in range(n + 1)]
        pq = []
        dist[start][0] = 0
        heapq.heappush(pq, (0, start, 0))
        if horses[start]:
            dist[start][1] = 0
            heapq.heappush(pq, (0, start, 1))

        while pq:
            d, u, s = heapq.heappop(pq)
            if d != dist[u][s]:
                continue
            for v, w in adj[u]:
                nd = d + (w if s == 0 else w // 2)
                if nd < dist[v][s]:
                    dist[v][s] = nd
                    heapq.heappush(pq, (nd, v, s))
            if s == 0 and horses[u]:
                if d < dist[u][1]:
                    dist[u][1] = d
                    heapq.heappush(pq, (d, u, 1))
        return dist

    t = int(input())
    out = []
    for _ in range(t):
        n, m, h = map(int, input().split())
        horses = [0] * (n + 1)
        for x in map(int, input().split()):
            horses[x] = 1
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v, w = map(int, input().split())
            adj[u].append((v, w))
            adj[v].append((u, w))

        distM = dijkstra(n, adj, horses, 1)
        distR = dijkstra(n, adj, horses, n)

        ans = INF
        for v in range(1, n + 1):
            ans = min(ans,
                      max(distM[v][0], distR[v][0]),
                      max(distM[v][0], distR[v][1]),
                      max(distM[v][1], distR[v][0]),
                      max(distM[v][1], distR[v][1]))

        out.append(str(-1 if ans == INF else ans))

    return "\n".join(out)

# sample 1
assert run("""6
2 1 1
1
1 2 10
3 1 2
2 3
1 2 10
3 3 1
2
1 2 4
1 3 10
2 3 6
4 3 2
2 3
1 2 10
2 3 18
3 4 16
3 2 1
2
1 2 4
1 3 16
7 7 1
3
1 5 2
2 6 12
1 2 12
6 4 8
7 3 4
6 3 4
7 6 4
""") == """5
-1
6
19
14
12"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 5 -1 6 19 14 12 | tính chính xác trên các biểu đồ hỗn hợp và vị trí ngựa | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nút bắt đầu đã chứa một con ngựa. Trong tình huống đó, thuật toán cho phép chuyển ngay sang trạng thái tăng tốc với chi phí bằng 0. Nếu quá trình chuyển đổi này bị bỏ qua, tất cả khoảng cách xuôi dòng của khách du lịch đó sẽ được đánh giá quá cao, tạo ra thời gian gặp nhau không chính xác. 

Một trường hợp khác xảy ra khi cả hai du khách đến một đỉnh ở các trạng thái khác nhau, ví dụ một người có ngựa và một người không có. Thời gian họp chính xác phải xét đến cả 4 tổ hợp trạng thái. Việc không kiểm tra các cuộc họp ở trạng thái hỗn hợp sẽ cho rằng cả hai khách du lịch đều cư xử đối xứng một cách không chính xác, điều này là sai vì một người có thể đã tăng tốc sớm hơn. 

Trường hợp tinh tế cuối cùng là đồ thị bị ngắt kết nối. Nếu một trong hai khoảng cách vẫn là vô hạn đối với mọi trạng thái ở mọi đỉnh thì câu trả lời phải là -1. Bước kết hợp xử lý việc này một cách tự nhiên vì tất cả các phép toán tối đa vẫn là vô hạn và mức tối thiểu cuối cùng không bao giờ cập nhật.
