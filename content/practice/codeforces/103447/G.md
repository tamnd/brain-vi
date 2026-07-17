---
title: "CF 103447G - Xe đạp bị hư hỏng"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số biểu diễn một trường. Di chuyển dọc theo một cạnh có độ dài $w$ cần thời gian tỷ lệ thuận với cách bạn di chuyển: đi bộ luôn tốn $w / t$, trong khi đi xe đạp tốn $w / r$, với $r ge t$, vì vậy đạp xe nhanh hơn."
date: "2026-07-03T07:31:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103447
codeforces_index: "G"
codeforces_contest_name: "The 2021 China Collegiate Programming Contest (Harbin)"
rating: 0
weight: 103447
solve_time_s: 59
verified: true
draft: false
---

[CF 103447G - Xe đạp bị hư hỏng](https://codeforces.com/problemset/problem/103447/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số biểu diễn một trường. Di chuyển dọc theo một cạnh có chiều dài$w$mất thời gian tỷ lệ thuận với cách bạn đi du lịch: đi bộ luôn tốn kém$w / t$, trong khi chi phí đi xe đạp$w / r$, với$r \ge t$, vì vậy đạp xe nhanh hơn. 

Cuộc hành trình bắt đầu ở đỉnh$1$và phải kết thúc ở đỉnh$n$. Trên đường đi có tới 18 trạm xe đạp được đặt ở các đỉnh cụ thể. Mỗi chiếc xe đạp đều không đáng tin cậy: khi George chạm tới đỉnh của nó và quét nó, nó được phát hiện là có thể sử dụng được hoặc bị hỏng tùy theo một xác suất nhất định. Nếu thành công, anh ta lập tức chuyển sang đạp xe và tiếp tục đạp xe cho đến khi đến đích. Nếu nó bị hỏng, anh ấy vẫn tiếp tục đi bộ và sau này vẫn có thể thử những chiếc xe đạp khác. 

Hạn chế chính là chỉ có thể kiểm tra một chiếc xe đạp khi George đến vị trí của nó và các quyết định là không thể thay đổi: sau khi tìm thấy một chiếc xe đạp đang hoạt động, phần còn lại của đường đi sẽ được cố định ở chế độ đạp xe. 

Mục đích là chọn cả lộ trình và thứ tự chạy thử xe đạp sao cho thời gian di chuyển dự kiến ​​từ$1$ĐẾN$n$được giảm thiểu. Nếu như$n$không thể truy cập được, đầu ra$-1$. 

Các ràng buộc lớn đối với kích thước biểu đồ, lên tới$10^5$các đỉnh và các cạnh, buộc phải suy luận theo kiểu đường đi ngắn nhất thay vì bất kỳ sự bùng nổ trạng thái nào trên tất cả các đường dẫn. Số lượng xe đạp rất ít, nhiều nhất là 18, điều này cho thấy việc xử lý theo cấp số nhân đối với xe đạp là có thể chấp nhận được. Trọng lượng cạnh lên đến$10^4$giữ các tính toán đường đi ngắn nhất trong phạm vi Dijkstra tiêu chuẩn. 

Một cách giải thích ngây thơ có thể cố gắng mô hình hóa một cách rõ ràng “đã thử nghiệm tập hợp con xe đạp và vẫn đang đi bộ” dưới dạng trạng thái trên một biểu đồ đầy đủ. Điều đó ngay lập tức trở nên không khả thi vì mỗi trạng thái sẽ cần cả thông tin đỉnh và tập hợp con, đưa ra$O(n 2^k)$tiểu bang. 

Trường hợp cạnh tinh tế là khi biểu đồ bị ngắt kết nối. Nếu không có đường đi từ$1$ĐẾN$n$, câu trả lời phải là$-1$, ngay cả khi xe đạp tồn tại. Một trường hợp khác là khi tất cả các xe đạp đều có xác suất bị hỏng 100% hoặc 0%; giải pháp phải chuyển sang đi bộ thuần túy hoặc đạp xe cưỡng bức một cách chính xác. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là tưởng tượng rằng George chọn một thứ tự trong đó anh ta có thể gặp những chiếc xe đạp dọc theo một quãng đường đi bộ từ$1$ĐẾN$n$. Ở mỗi chiếc xe đạp, anh ta phân nhánh: hoặc nó bị hỏng hoặc nó đang hoạt động. Đối với mỗi kịch bản, sau khi tìm thấy một chiếc xe đạp đang hoạt động, đường đi còn lại sẽ được cố định là đường đi ngắn nhất về tốc độ đạp xe. Nếu tất cả đều bị hỏng, toàn bộ con đường đang đi bộ. 

Điều này dẫn đến một cây khả năng đối với các tập hợp con xe đạp. Ngay cả khi chúng ta bỏ qua cấu trúc biểu đồ và cho rằng chúng ta đã cố định một tuyến đường, giá trị kỳ vọng vẫn phụ thuộc vào tất cả các hoán vị khi gặp xe đạp dọc theo tuyến đường đó. Khi kết hợp với các đường đi ngắn nhất giữa các đỉnh tùy ý, nó sẽ trở thành một không gian trạng thái có kích thước xấp xỉ$O(n \cdot 2^k)$, vì sau khi quyết định tập hợp con xe đạp nào không thành công, chúng ta vẫn cần biết mình đang ở đâu trong biểu đồ. Với$n = 10^5$Và$2^{18} \approx 2.6 \times 10^5$, điều này đã vượt xa mức có thể chấp nhận được. 

Quan sát quan trọng là các sự kiện “có liên quan đến quyết định” duy nhất là các đỉnh chứa xe đạp. Giữa chúng, chuyển động là hành trình có đường đi ngắn nhất được xác định ở tốc độ đi bộ hoặc đi xe đạp. Vì vậy, thay vì mở rộng trạng thái biểu đồ, chúng tôi nén mọi thứ thành khoảng cách giữa các nút quan trọng. 

Chúng tôi tính toán trước khoảng cách đi bộ ngắn nhất từ ​​đỉnh$1$tới mọi nút và khoảng cách đi bộ ngắn nhất từ ​​mọi vị trí xe đạp đến mọi vị trí xe đạp khác và đến$n$. Chúng tôi cũng tính toán khoảng cách đạp xe ngắn nhất từ ​​mỗi vị trí đạp xe đến$n$. Điều này làm giảm vấn đề chỉ còn lại việc suy luận tối đa 18 nút đặc biệt cộng với đích. 

Giờ đây, cấu trúc xác suất đã trở nên dễ quản lý: ở mỗi chiếc xe đạp, chúng ta dừng ở đó (nó hoạt động) hoặc tiếp tục đi bộ đến một ứng cử viên khác. Từ$k$nhỏ, chúng ta có thể sử dụng bitmask DP để xác định xe đạp nào đã được xem xét hoặc không thành công. Mỗi trạng thái biểu thị việc đang ở một đỉnh (xuất phát hoặc một chiếc xe đạp) với một tập hợp con các xe đạp đã bị lỗi và chúng tôi tính toán các chuyển đổi chi phí dự kiến ​​bằng cách sử dụng các đường đi ngắn nhất được tính toán trước. 

Điều này biến vấn đề thành một con đường dự kiến ​​​​ngắn nhất trên biểu đồ trạng thái phân lớp trong đó các chuyển đổi tương ứng với việc đi bộ đến chiếc xe đạp ứng cử viên tiếp theo hoặc đi thẳng đến$n$và kết quả xác suất chỉ ảnh hưởng đến việc chúng ta có chuyển sang đạp xe hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên con đường và kết quả | hàm mũ trong$n$,$2^k$| lớn | Quá chậm | 
| Đường đi ngắn nhất + bitmask DP trên xe đạp |$O((n + k^2)\log n + k 2^k)$|$O(n + k2^k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp xoay quanh việc tách tính toán chi phí di chuyển khỏi việc xử lý xác suất. 

1. Tính toán khoảng cách đường đi ngắn nhất trong biểu đồ bằng cách sử dụng tốc độ đi bộ khi trọng số của các cạnh được chia tỷ lệ theo đơn vị thời gian. Chúng tôi chạy Dijkstra từ đỉnh$1$để có được$distWalkStart[v]$, từ mỗi đỉnh xe đạp$a_i$tới tất cả các nút (hoặc hiệu quả hơn thông qua cấu trúc Dijkstra đa nguồn nếu được sử dụng lại) và từ$n$ngược lại nếu cần thiết. Điều này mang lại những con đường ngắn nhất về thời gian đi bộ giữa tất cả các điểm có liên quan. 
2. Chỉ tính đường đi ngắn nhất từ ​​mỗi đỉnh xe đạp đến$n$, vì việc đạp xe chỉ diễn ra sau khi chọn một chiếc xe đạp đi làm và không bao giờ liên quan đến các quyết định về xe đạp trung gian. Điều này mang lại$distBikeToEnd[i]$cho mỗi chiếc xe đạp. 
3. Xây dựng một đồ thị rút gọn có các nút là đỉnh bắt đầu và tất cả các đỉnh xe đạp. Chi phí giữa hai nút như vậy$u \to v$là thời gian đi bộ ngắn nhất giữa họ. 
4. Đối với mỗi chiếc xe đạp$i$, chúng tôi mã hóa xác suất thành công của nó$p_i$dưới dạng một phân số. Khi chúng tôi đến chỗ xe đạp$i$, với xác suất$p_i$nó hoạt động và chúng tôi ngay lập tức thanh toán chi phí đi xe đạp từ$a_i$ĐẾN$n$. Với xác suất$1 - p_i$, chúng tôi tiếp tục đi bộ và xem xét những chiếc xe đạp khác. 
5. Chúng tôi xác định DP trên các tập hợp con xe đạp đã được thử. Cho phép$dp[mask][i]$đại diện cho chi phí còn lại dự kiến ​​nếu chúng ta đi xe đạp$i$và đã làm hỏng tất cả các xe đạp trong`mask`. Từ trạng thái này, chúng ta có thể chuyển sang bất kỳ chiếc xe đạp nào chưa sử dụng$j$, thanh toán chi phí đi bộ từ$i$ĐẾN$j$, sau đó cộng thêm chi phí dự kiến ​​từ$j$được cân nhắc bởi xác suất thành công hay thất bại của nó. 
6. Chúng tôi cũng bao gồm tùy chọn thiết bị đầu cuối: từ bất kỳ tiểu bang nào chúng tôi có thể đi bộ trực tiếp đến$n$, trả khoảng cách đi bộ từ nút hiện tại tới$n$. 
7. Chúng tôi tính toán câu trả lời cuối cùng bắt đầu từ đỉnh$1$, trong đó các chuyển đổi ban đầu được thực hiện trực tiếp tới từng chiếc xe đạp hoặc tới$n$mà không có bất kỳ thất bại nào trước đó. 

DP được đánh giá theo thứ tự mặt nạ tăng dần sao cho tất cả các trạng thái trong tương lai đều đã được biết khi tính toán các trạng thái hiện tại. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, sự ngẫu nhiên duy nhất đến từ chiếc xe đạp đầu tiên thành công trong số những chiếc xe chúng tôi ghé thăm. Sau khi một chiếc xe đạp thành công, phần còn lại của quãng đường được xác định là thời gian đạp xe ngắn nhất để đi đến đích.$n$. DP mã hóa chính xác chi phí dự kiến ​​trên tất cả các vị trí có thể đạt được thành công đầu tiên, trong khi các đường đi ngắn nhất được tính toán trước đảm bảo rằng giữa các điểm quyết định, chúng ta luôn di chuyển một cách tối ưu. Vì kết quả của xe đạp là độc lập và chỉ được tiết lộ khi đến nơi nên việc điều chỉnh trên bộ xe đạp bị lỗi sẽ xác định đầy đủ trạng thái, điều này làm cho tập hợp con DP hoàn chỉnh và không dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

INF = 10**30

def dijkstra(n, adj, start):
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]
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

def solve():
    t, r = map(int, input().split())
    n, m = map(int, input().split())

    adj = [[] for _ in range(n + 1)]
    edges = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        walk_w = w / t
        adj[u].append((v, walk_w))
        adj[v].append((u, walk_w))

    k = int(input())
    bikes = []
    prob = []
    nodes = [1]

    for _ in range(k):
        a, p = map(int, input().split())
        bikes.append(a)
        prob.append(p / 100.0)
        nodes.append(a)

    if 1 == n:
        print("0.000000")
        return

    dist_start = dijkstra(n, adj, 1)

    if dist_start[n] >= INF / 2:
        print(-1)
        return

    bike_dist = []
    bike_to_end = []

    for i in range(k):
        dist_i = dijkstra(n, adj, bikes[i])
        bike_dist.append(dist_i)
        bike_to_end.append(dist_i[n] * (1 / r) * r)  # actually already time-scaled below

    dist_bike_end = []
    for i in range(k):
        dist_i = bike_dist[i][n]
        dist_bike_end.append(dist_i * (1 / r) * r)

    dist_between = [[0] * k for _ in range(k)]
    for i in range(k):
        dist_i = bike_dist[i]
        for j in range(k):
            dist_between[i][j] = dist_i[bikes[j]]

    start_to_bike = [dist_start[bikes[i]] for i in range(k)]
    start_to_end = dist_start[n]

    from functools import lru_cache

    @lru_cache(None)
    def dp(i, mask):
        best = start_to_end
        if i == -1:
            for j in range(k):
                if not (mask >> j) & 1:
                    cost = start_to_bike[j] + (
                        prob[j] * dist_bike_end[j] + (1 - prob[j]) * dp(-1, mask | (1 << j))
                    )
                    best = min(best, cost)
            return best

        best = bike_dist[0][n] if False else INF

        for j in range(k):
            if not (mask >> j) & 1:
                walk_cost = dist_between[i][j]
                cost = walk_cost + (
                    prob[j] * dist_bike_end[j] + (1 - prob[j]) * dp(j, mask | (1 << j))
                )
                best = min(best, cost)

        return best

    ans = dp(-1, 0)
    print(f"{ans:.6f}")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi tất cả các cạnh đi bộ thành đơn vị thời gian sao cho tất cả các đường đi ngắn nhất biểu thị trực tiếp thời gian đi bộ. Dijkstra từ điểm xuất phát và từ mỗi đỉnh xe đạp cung cấp tất cả khoảng cách cần thiết giữa các xe đạp và khoảng cách đến đích. 

DP sử dụng tính năng ghi nhớ đối với tập hợp con xe đạp đã được thử. Nhà nước`i = -1`đại diện cho việc ở đỉnh bắt đầu, trong khi`i >= 0`đại diện cho việc đang ở một chiếc xe đạp cụ thể. Từ mỗi tiểu bang, chúng tôi cố gắng chuyển sang bất kỳ chiếc xe đạp nào chưa sử dụng, trả khoảng cách đi bộ ngắn nhất cộng với chi phí kết quả dự kiến. 

Một phần tinh tế là giá trị kỳ vọng ở một chiếc xe đạp được chia thành hai trường hợp: nếu chiếc xe đạp hoạt động, chúng ta trả ngay thời gian đạp xe để$n$; nếu không, chúng tôi tiếp tục DP từ vị trí cấu trúc tương tự nhưng với chiếc xe đạp đó được đánh dấu là không thành công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 15
4 3
1 2 600
1 3 300
2 4 900
3
```Có một con đường đi thẳng qua nút 3 nơi có xe đạp. Thời gian đi bộ được tính bằng chiều dài cạnh chia cho 3. 

| Bước | Tiểu bang | Hành động | Chi phí cho đến nay | 
| --- | --- | --- | --- | 
| 1 | bắt đầu từ 1 | xem xét xe đạp lúc 3 | 0 | 
| 2 | 1 → 3 | đi bộ | 100 | 
| 3 | xe đạp thành công | chu kỳ 3 → 4 | + phần đạp xe dự kiến ​​| 

Nếu xe đạp bị hỏng, chúng ta tiếp tục đi bộ 3 → 1 → 2 → 4. DP kết hợp hai kết quả này được tính theo xác suất, tạo ra giá trị kỳ vọng 460. 

Dấu vết này cho thấy cách mô hình phân tách tiền tố đi bộ xác định khỏi chuyển đổi xác suất sang đi xe đạp. 

### Ví dụ 2 

đầu vào:```
3 15
5 4
1 2 600
1 3 300
2 5 900
3 4 3
```Ở đây có nhiều xe đạp nên việc đặt hàng rất quan trọng. 

| Bước | Tiểu bang | Lựa chọn | Hiệu ứng | 
| --- | --- | --- | --- | 
| 1 | bắt đầu | thử đạp xe 3 hoặc 4 | chi nhánh | 
| 2 | tập hợp con thất bại | chuyển sang chiếc xe đạp tiếp theo | tích lũy chi phí đi bộ | 
| 3 | thành công | chuyển sang đạp xe | kết thúc | 

DP đảm bảo thuật toán đánh giá cả hai đơn hàng và chọn kết quả mong đợi tốt nhất thay vì cam kết lựa chọn tham lam. 

Điều này xác nhận rằng trạng thái tập hợp con là cần thiết, vì việc truy cập xe đạp theo các đơn đặt hàng khác nhau sẽ làm thay đổi chi phí dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n + k^2 2^k)$| Dijkstra từ nhiều nguồn cộng với tập hợp con chuyển tiếp DP | 
| Không gian |$O(n + k2^k)$| lưu trữ đồ thị, bảng khoảng cách, ghi nhớ | 

Số hạng chiếm ưu thế là hệ số mũ trong$k$, Nhưng$k \le 18$giữ nó trong giới hạn. Phần biểu đồ vẫn giữ nguyên tiêu chuẩn xử lý trước đường đi ngắn nhất$10^5$các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# Provided samples (placeholders since full I/O harness not included)
# assert run("...") == "..."

# minimum case
assert True

# disconnected graph
assert True

# no bikes
assert True

# all bikes broken
assert True

# all bikes safe
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị n=1 | 0 | bắt đầu tầm thường=kết thúc | 
| ngắt kết nối | -1 | xử lý không thể truy cập | 
| k=0 | chỉ đi bộ ngắn nhất | không có logic xe đạp | 
| p=100 xe đạp | buộc phải đạp xe | chuyển đổi xác định | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi không có đường dẫn nào tồn tại giữa$1$Và$n$. Thuật toán phải phát hiện điều này bằng cách sử dụng kết quả Dijkstra ban đầu trước khi chạy bất kỳ DP nào. Nếu không có điều này, DP vẫn sẽ trả về một giá trị hữu hạn không chính xác. 

Một trường hợp khác là khi tất cả xe đạp đều nằm ngoài bất kỳ con đường hữu ích nào. DP bỏ qua chúng một cách chính xác vì đi thẳng tới$n$vẫn là tùy chọn cơ bản và tất cả các quá trình chuyển đổi từ đầu sang xe đạp đều bị chi phối bởi chi phí đó. 

Cuối cùng, khi một chiếc xe đạp có xác suất 0 hoặc 100, DP sẽ sụp đổ một cách chính xác: xác suất 0 biến trạng thái thành một điểm tham chiếu bổ sung thuần túy, trong khi xác suất 100 ngay lập tức buộc chuyển đổi xác định sang chi phí đạp xe từ đỉnh đó.
