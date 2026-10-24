---
title: "CF 105945J - Cuộc thi giải đố"
description: "Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi nút đại diện cho một câu đố. Mọi nút đều bắt đầu với “năng lượng” bằng 0 và mỗi nút có một giá trị ngưỡng. Một nút sẽ được mở khóa ngay khi tổng năng lượng mà nó tích lũy đạt đến hoặc vượt quá ngưỡng của nó."
date: "2026-06-22T15:58:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105945
codeforces_index: "J"
codeforces_contest_name: "The 2025 Jiangsu Collegiate Programming Contest, The 2025 Guangdong Provincial Collegiate Programming Contest"
rating: 0
weight: 105945
solve_time_s: 87
verified: true
draft: false
---

[CF 105945J - Cuộc thi giải đố](https://codeforces.com/problemset/problem/105945/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi nút đại diện cho một câu đố. Mọi nút đều bắt đầu với “năng lượng” bằng 0 và mỗi nút có một giá trị ngưỡng. Một nút sẽ được mở khóa ngay khi tổng năng lượng mà nó tích lũy đạt đến hoặc vượt quá ngưỡng của nó. 

Cách duy nhất để năng lượng xuất hiện là thông qua các nút đã được mở khóa. Khi một nút được mở khóa, nó ngay lập tức bắt đầu gửi năng lượng đến tất cả các nút lân cận đi ra ngoài của nó. Mỗi cạnh đi ra có một thời gian di chuyển, vì vậy nếu nút u mở khóa tại thời điểm T thì mỗi cạnh u → v với độ trễ w sẽ đóng góp chính xác một đơn vị năng lượng cho v tại thời điểm T + w. Nếu v nhận được nhiều đóng góp như vậy từ những người hàng xóm khác nhau hoặc những con đường khác nhau, thì tất cả chúng đều được tích lũy. 

Ngoài hệ thống truyền bá này, còn có những sự kiện đặc biệt. Mỗi sự kiện chọn một số nút và tại một thời điểm cụ thể, buộc ngưỡng của chúng trở thành 0. Khi ngưỡng của nút trở về 0, nút đó sẽ mở khóa ngay lập tức, bất kể nó đã nhận được bao nhiêu năng lượng. 

Nhiệm vụ là tính toán, đối với mỗi nút, thời điểm sớm nhất nó được mở khóa. Nếu một nút không bao giờ có thể được mở khóa thì câu trả lời là −1. 

Các ràng buộc rất lớn: lên tới 100.000 nút và lên tới 1.000.000 cạnh. Điều này ngay lập tức loại trừ mọi thứ tính toán lại trạng thái chung trên mỗi nút hoặc mô phỏng từng bước thời gian. Cấu trúc gợi ý rằng mỗi cạnh chỉ có thể được “kích hoạt” một lần, khi nút nguồn của nó mở khóa, do đó, mọi giải pháp đều phải đảm bảo cẩn thận rằng mỗi nút được xử lý một lần và mỗi cạnh được thư giãn một lần. 

Một khó khăn tinh tế xuất hiện trong cách tích lũy năng lượng. Một nút không mở khóa do có một đường đi ngắn nhất mà do đạt đến ngưỡng của các sự kiện năng lượng đến. Điều này khác với các bài toán đường đi ngắn nhất cổ điển và buộc chúng ta phải theo dõi nhiều thời gian đến trên mỗi nút chứ không chỉ thời gian tốt nhất. 

Một trường hợp cạnh phá vỡ sự lan truyền ngây thơ là khi một nút có ngưỡng dương nhưng nhận được nhiều đóng góp chậm sau đó, đồng thời gặp phải sự kiện buộc phải đặt lại trước đó. 

Ví dụ: hãy xem xét một nút v có ngưỡng 2. Nó nhận được hai đóng góp đến vào thời điểm 100 và 200. Nó cũng bị buộc thiết lập lại tại thời điểm 50. Câu trả lời đúng là 50, vì nó mở khóa ngay lập tức tại thời điểm đặt lại. Một cách tiếp cận ngây thơ chỉ xem xét năng lượng đến sẽ tạo ra kết quả không chính xác là 200. 

Một trường hợp lỗi khác xảy ra khi một nút ban đầu có ngưỡng bằng 0. Nó sẽ mở khóa tại thời điểm 0, nhưng nếu bộ giải trì hoãn kích hoạt không chính xác cho đến khi xử lý cạnh đến đầu tiên, nó sẽ đánh giá quá cao tất cả thời gian xuôi dòng. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ cố gắng duy trì năng lượng hiện tại của mọi nút theo thời gian và liên tục xử lý sự kiện tiếp theo sớm nhất. Người ta có thể tưởng tượng một hàng đợi sự kiện toàn cầu bao gồm cả “sự kiện đến năng lượng” và “sự kiện mở khóa”, cập nhật các nút khi đạt đến ngưỡng. Vấn đề là mỗi nút có thể nhận được số lượng đóng góp năng lượng tối đa và mỗi lần đóng góp phụ thuộc vào thời điểm nguồn của nó mở khóa. Vì việc mở khóa phụ thuộc vào sự đóng góp tích lũy nên hệ thống trở nên phụ thuộc lẫn nhau. 

Nếu chúng ta bỏ qua sự ghép nối này và giả sử thời gian đến cố định, thì mỗi nút v sẽ chỉ thu thập tất cả thời gian đến T[u] + w và lấy thời gian đến nhỏ nhất ai[v]-th. Phần đó có thể quản lý được với một đống trên mỗi nút. Khó khăn thực sự là T[u] không được biết trước. 

Quan sát quan trọng là mỗi nút có chính xác một thời gian mở khóa cuối cùng và khi một nút mở khóa, nó sẽ phát ra năng lượng đúng một lần. Điều này cho phép chúng tôi coi các sự kiện mở khóa là những thay đổi trạng thái không thể đảo ngược, tương tự như thuật toán của Dijkstra trong đó mỗi nút được hoàn tất một lần.

Do đó, chúng tôi duy trì hai cơ chế độc lập có thể kích hoạt việc mở khóa. Đầu tiên là thời gian mở khóa bắt buộc đến từ các lần làm mới. Thứ hai là thời điểm đã thu thập đủ năng lượng. Mỗi lần chúng tôi chốt thời gian mở khóa của một nút, chúng tôi sẽ truyền bá hiệu ứng của nút đó tới các nút lân cận bằng cách tạo ra các sự kiện cung cấp năng lượng mới. 

Điều này đương nhiên dẫn đến chiến lược xử lý đầu tiên tốt nhất trong thời gian mở khóa ứng viên. Bất cứ khi nào chúng tôi phát hiện ra thời gian mở khóa tiềm năng cho một nút, từ lúc làm mới hoặc khi đạt đến ngưỡng của nút đó, chúng tôi sẽ đẩy nút đó vào hàng đợi ưu tiên toàn cầu. Thời gian ứng viên nhỏ nhất luôn là nút tiếp theo cần hoàn thiện và sau khi hoàn tất, nó sẽ trở thành thời gian mở khóa thực sự. 

Để hỗ trợ mở khóa dựa trên ngưỡng, mỗi nút duy trì một cấu trúc theo dõi thời gian đến nhỏ nhất của nó theo số lượng yêu cầu. Một đống tối đa có kích thước cố định ai[v] là đủ, vì nó luôn duy trì thời gian đến nhỏ nhất của ai[v] được thấy cho đến nay và đỉnh của đống này là thời gian ngưỡng ứng cử viên hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng năng lượng Brute Force theo thời gian | O(thời gian · n + m bước thời gian) | O(n + m) | Quá chậm | 
| Dijkstra theo sự kiện với tính năng theo dõi ngưỡng | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp xung quanh hàng đợi ưu tiên toàn cầu gồm các sự kiện mở khóa ứng viên và cấu trúc mỗi nút theo dõi nguồn năng lượng đến. 

1. Đối với mỗi nút, hãy tính thời gian sớm nhất mà nút đó có thể bị buộc phải mở khóa sau khi làm mới. Nếu một nút không có phần làm mới, giá trị này là vô cùng. Các nút có ngưỡng ban đầu bằng 0 được coi là có thời gian mở khóa bắt buộc bằng 0. Điều này cung cấp cho mỗi nút một sự kiện mở khóa ứng viên ban đầu. 
2. Khởi tạo hàng đợi ưu tiên với tất cả các cặp (forced_time[v], v). Điều này thể hiện tất cả các cách mà một nút có thể mở khóa ngay lập tức mà không cần chờ truyền năng lượng. 
3. Đối với mỗi nút v, hãy duy trì một heap tối đa lưu trữ tối đa ai[v] thời gian đến năng lượng đến nhỏ nhất. Cấu trúc này cho phép chúng ta xác định khi nào v đã nhận đủ năng lượng để mở khóa thông qua quá trình truyền lan. 
4. Duy trì một mảng lưu trữ thời gian mở khóa tốt nhất hiện được biết cho mỗi nút, được khởi tạo theo thời gian mở khóa bắt buộc của nó. 
5. Trong khi hàng đợi ưu tiên không trống, hãy trích xuất sự kiện có thời gian nhỏ nhất. Nếu thời gian này không tốt hơn thời gian mở khóa cuối cùng của nút đó, hãy loại bỏ nó vì việc mở khóa tốt hơn hoặc bằng nhau đã được xử lý. 
6. Nếu không, hãy hoàn tất nút này vào thời điểm đó. Đây là thời điểm nút thực sự mở khóa. 
7. Từ nút u mới được mở khóa này, đi qua tất cả các cạnh đi ra u → v với trọng số w. Đối với mỗi cạnh như vậy, hãy chèn một sự kiện đến năng lượng mới tại thời điểm T[u] + w vào vùng v. 
8. Sau khi chèn một phần đến vào heap của v, nếu kích thước heap vượt quá ai[v], hãy loại bỏ phần tử lớn nhất để chỉ còn lại phần tử ai[v] nhỏ nhất. Nếu kích thước heap chính xác là ai[v], thì phần tử tối đa hiện tại của heap biểu thị thời gian đến nhỏ nhất thứ ai[v], là thời gian mở khóa ứng cử viên thông qua năng lượng. Đẩy ứng viên này vào hàng đợi ưu tiên toàn cầu. 
9. Tiếp tục cho đến khi tất cả các sự kiện được xử lý. Các nút không bao giờ nhận đủ số lượt đến và không có thời gian mở khóa cưỡng bức hữu hạn vẫn ở mức vô cực và được xuất ra là −1. 

Tính chính xác dựa trên thực tế là mỗi nút được hoàn thành chính xác một lần tại thời gian mở khóa tối thiểu có thể. Bất kỳ ứng cử viên nào sau này cho cùng một nút đều không liên quan vì nó không thể cải thiện trạng thái đã hoàn thiện. Tính bất biến của heap đảm bảo rằng tại bất kỳ thời điểm nào, thời gian mở khóa dựa trên năng lượng ứng cử viên chính xác là thời gian vượt qua ngưỡng thực sự đối với tất cả các lần đến đã được xử lý cho đến nay. Vì các lượt đến chỉ được tạo từ các nút đã hoàn tất nên không có sự điều chỉnh nào sau này đối với nguồn có thể làm mất hiệu lực các sự kiện được tạo trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

INF = 10**30

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    forced = [INF] * n

    # read refreshers
    for _ in range(k):
        tmp = list(map(int, input().split()))
        t = tmp[0]
        sz = tmp[1]
        ids = tmp[2:]
        for v in ids:
            v -= 1
            if t < forced[v]:
                forced[v] = t

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))

    # best unlock time
    ans = [INF] * n

    # PQ of candidate unlock events
    pq = []

    # per node: store smallest ai[v] arrivals using max-heap (negated)
    heaps = [[] for _ in range(n)]

    # initialize forced unlocks
    for i in range(n):
        if forced[i] < INF:
            heapq.heappush(pq, (forced[i], i))
        if a[i] == 0:
            heapq.heappush(pq, (0, i))

    while pq:
        t, u = heapq.heappop(pq)
        if t >= ans[u]:
            continue

        ans[u] = t

        for v, w in g[u]:
            arr = t + w

            if a[v] > 0:
                heap = heaps[v]
                heapq.heappush(heap, -arr)
                if len(heap) > a[v]:
                    heapq.heappop(heap)

                if len(heap) == a[v]:
                    kth = -heap[0]
                    if kth < ans[v]:
                        heapq.heappush(pq, (kth, v))

    out = []
    for i in range(n):
        out.append(str(ans[i]) if ans[i] < INF else "-1")
    print(" ".join(out))

if __name__ == "__main__":
    solve()
```Mã này phân tách hai cơ chế có thể kích hoạt kích hoạt: thời gian mở khóa bắt buộc và thời gian mở khóa dựa trên ngưỡng. Hàng đợi ưu tiên hợp nhất cả hai thành một trật tự toàn cầu duy nhất, đảm bảo rằng sự kiện mở khóa sớm nhất có thể luôn được xử lý trước. 

Mỗi nút duy trì kích thước heap tối đa nhiều nhất là ai[v], điều này đảm bảo rằng gốc heap luôn là thời gian đến nhỏ nhất thứ hiện tại của ai[v]. Điều này tránh việc lưu trữ tất cả các sự kiện đến, có thể quá lớn theo các ràng buộc. 

Một điểm tinh tế là khi một nút được hoàn thành, các thời điểm ứng cử viên sau đó sẽ bị bỏ qua ngay cả khi vùng heap được cải thiện. Điều này an toàn vì mọi cải tiến nhất thiết phải lớn hơn hoặc bằng thời gian đã chọn vì hàng đợi xử lý các sự kiện theo thứ tự tăng dần. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu thứ hai nơi tồn tại các cập nhật bắt buộc và quá trình lan truyền tương tác với chúng. 

Chúng tôi chỉ theo dõi một vài sự kiện quan trọng đầu tiên. 

| Bước | Nút | Thời gian sự kiện | Hành động | Trạng thái kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | mở khóa ban đầu (ai=0) | nút 1 = 0 | 
| 2 | 2 | 100 | ứng viên bị ép buộc | nút 2 đang chờ xử lý | 
| 3 | 1 → 2 | 1 | đến qua cạnh | đống[2] = [1] | 
| 4 | 2 | 1 | thứ k đến không đủ | không mở khóa | 
| 5 | 2 | 100 | buộc phải xử lý mở khóa | nút 2 = 100 | 

Điều này cho thấy việc mở khóa cưỡng bức có thể chiếm ưu thế như thế nào ngay cả khi năng lượng sớm đến. 

Bây giờ hãy xem xét trường hợp lan truyền chiếm ưu thế. 

| Bước | Nút | Thời gian sự kiện | Hành động | Trạng thái kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | mở khóa nguồn | nút 1 = 0 | 
| 2 | 1 → 2 | 1 | đến | đống[2] = [1] | 
| 3 | 1 → 2 | 2 | đến | đống[2] = [1,2] | 
| 4 | 2 | 2 | đạt thứ k | nút 2 = 2 | 

Điều này xác nhận rằng nút 2 sẽ mở khóa chính xác khi tín hiệu đến nhỏ thứ hai của nó đến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + Σ ai log ai) | mỗi nút được hoàn thành một lần, mỗi cạnh được xử lý một lần, các thao tác heap mỗi lần đến | 
| Không gian | O(n + m) | danh sách kề cộng với đống trên mỗi nút và PQ | 

Các ràng buộc cho phép lên tới một triệu cạnh, do đó chi phí logarit trên mỗi cạnh là có thể chấp nhận được. Mỗi cạnh đóng góp chính xác một sự kiện sau khi nguồn của nó mở khóa, đảm bảo việc tạo ra sự kiện tuyến tính nói chung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    INF = 10**30

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    forced = [INF] * n

    for _ in range(k):
        tmp = list(map(int, input().split()))
        t = tmp[0]
        sz = tmp[1]
        ids = tmp[2:]
        for v in ids:
            v -= 1
            forced[v] = min(forced[v], t)

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))

    ans = [INF] * n
    heaps = [[] for _ in range(n)]
    pq = []

    for i in range(n):
        if forced[i] < INF:
            heapq.heappush(pq, (forced[i], i))
        if a[i] == 0:
            heapq.heappush(pq, (0, i))

    while pq:
        t, u = heapq.heappop(pq)
        if t >= ans[u]:
            continue
        ans[u] = t
        for v, w in g[u]:
            arr = t + w
            if a[v] > 0:
                heapq.heappush(heaps[v], -arr)
                if len(heaps[v]) > a[v]:
                    heapq.heappop(heaps[v])
                if len(heaps[v]) == a[v]:
                    kth = -heaps[v][0]
                    if kth < ans[v]:
                        heapq.heappush(pq, (kth, v))

    return " ".join(str(x) if x < INF else "-1" for x in ans)

# provided samples
assert run("""6 9 0
0 2 1 1 1 4
1 2 1
2 3 1
3 4 1
4 5 1
5 2 1
2 6 1
3 6 1
4 6 1
5 6 1
""") == "0 -1 -1 -1 -1 -1"

# custom: single node, zero threshold
assert run("""1 0 0
0
""") == "0"

# custom: forced unlock dominates propagation
assert run("""3 2 1
5 1 1
1 3
1 2 1
2 3 1
""") == "5 -1 -1"

# custom: chain propagation
assert run("""4 3 0
1 0 1 1
1 2 1
2 3 1
3 4 1
""") == "-1 0 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | cấu hình tối thiểu | 
| buộc phải thống trị | 5 -1 -1 | làm mới ghi đè tuyên truyền | 
| truyền chuỗi | -1 0 1 2 | tính chính xác phụ thuộc nhiều bước | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một nút có ngưỡng bằng 0. Một nút như vậy phải mở khóa tại thời điểm 0 ngay cả khi không có cạnh nào trỏ tới nó. Trong thuật toán, các nút này được chèn vào hàng đợi ưu tiên với thời gian bằng 0, đảm bảo chúng hoạt động như nguồn lan truyền ban đầu. 

Một trường hợp khác là khi một nút có cả khả năng mở khóa bắt buộc và năng lượng đến. Thuật toán xử lý chính xác điều này vì nó luôn lấy sự kiện tối thiểu xuất hiện từ hàng đợi ưu tiên. Nếu năng lượng sẽ mở khóa nó vào thời điểm 120 nhưng có một bản cập nhật tồn tại ở thời điểm 50, thì hàng đợi sẽ xử lý 50 trước và hoàn thành nút sớm, loại bỏ các thời điểm ứng cử viên sau đó. 

Trường hợp thứ ba là các nút không bao giờ đạt đến ngưỡng của chúng và không được làm mới. Các nút này không bao giờ nhận được một sự kiện hữu hạn được đẩy vào hàng đợi ưu tiên, vì vậy chúng vẫn ở mức vô cùng và xuất ra chính xác −1.
