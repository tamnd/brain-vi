---
title: "CF 196E - Cổng mở"
description: "Chúng ta có một mạng lưới các thành phố được kết nối, một số trong đó có cổng thông tin. Mỗi con đường giữa các thành phố đều có thời gian di chuyển dương và Pavel bắt đầu ở thành phố 1."
date: "2026-06-03T09:45:12+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 196
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 124 (Div. 1)"
rating: 2600
weight: 196
solve_time_s: 120
verified: false
draft: false
---

[CF 196E - Mở cổng](https://codeforces.com/problemset/problem/196/E) 

**Đánh giá:** 2600 
**Tags:** dsu, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các thành phố được kết nối, một số trong đó có cổng thông tin. Mỗi con đường giữa các thành phố đều có thời gian di chuyển dương và Pavel bắt đầu ở thành phố 1. Bất cứ khi nào anh ta vào một thành phố có cổng, cổng đó sẽ mở ra và từ thời điểm đó anh ta có thể dịch chuyển tức thời giữa bất kỳ cổng nào đang mở. Nhiệm vụ là tính tổng thời gian di chuyển tối thiểu cần thiết để Pavel mở tất cả các cổng. 

Dữ liệu đầu vào bao gồm tới 100.000 thành phố và 100.000 con đường. Trọng lượng mỗi con đường có thể rất lớn, lên tới$10^9$. Số lượng thành phố cổng thông tin nhiều nhất là$n$. Những giới hạn này ngay lập tức gợi ý rằng bất kỳ cách tiếp cận nào với độ phức tạp thời gian bậc hai trên số lượng thành phố đều không khả thi. Một cách tiếp cận ngây thơ thử tất cả các thứ tự có thể có của các lượt truy cập cổng thông tin là theo cấp số nhân và hoàn toàn không thực tế. Thay vào đó, chúng ta cần một thuật toán có tỷ lệ gần như tuyến tính với số lượng đường và logarit theo khoảng cách. 

Một trường hợp quan trọng phát sinh khi chính thành phố xuất phát có chứa một cổng thông tin. Trong trường hợp đó, Pavel có thể dịch chuyển ngay lập tức đến bất kỳ thành phố cổng thông tin nào khác khi anh ấy đến cổng tiếp theo. Một tình huống khó phát hiện khác xảy ra khi mạng chỉ có một cổng hoặc khi nhiều thành phố cổng được kết nối trực tiếp bằng những đường dẫn rất dài. Việc triển khai bất cẩn có thể cố gắng duyệt qua toàn bộ biểu đồ nhiều lần hoặc không tận dụng được dịch chuyển tức thời một cách tối ưu, tạo ra giải pháp quá chậm hoặc đánh giá quá cao thời gian cần thiết. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ là mô phỏng rõ ràng chuyển động của Pavel, xem xét tất cả các hoán vị của trình tự truy cập cổng thông tin. Đối với mỗi chuỗi, chúng tôi sẽ tính toán chi phí di chuyển, cộng thêm 0 cho việc dịch chuyển tức thời giữa các cổng đã mở. Mặc dù cách tiếp cận này mô hình hóa chính xác vấn đề nhưng độ phức tạp về thời gian của nó là$O(k!)$vì$k$cổng thông tin, điều này trở nên không khả thi ngay cả đối với$k = 10$, chưa kể đến những hạn chế của vấn đề ở đâu$k$có thể lớn tới 100.000. Nút thắt chính là sự tăng trưởng giai thừa về số lượng trình tự. 

Quan sát mở ra một giải pháp hiệu quả là các cổng có thể được coi là một biểu đồ ảo hoàn chỉnh sau khi được mở. Nếu chúng tôi tính toán khoảng cách ngắn nhất từ ​​thành phố xuất phát đến tất cả các thành phố cổng thông tin và cũng biết khoảng cách tối thiểu giữa bất kỳ cặp cổng nào, thì thời gian tối thiểu để mở tất cả các cổng bị chi phối bởi khoảng cách lớn nhất từ ​​điểm bắt đầu đến cổng hoặc giữa các cổng trong cấu trúc kéo dài tối thiểu kết nối tất cả các cổng. Chính thức hơn, sau khi tính toán các đường đi ngắn nhất, thời gian tối thiểu để đến tất cả các cổng sẽ giảm xuống còn việc chọn hai thành phố cổng cách xa nhau nhất trong khoảng cách biểu đồ, cộng khoảng cách nhỏ nhất từ ​​đầu đến cuối. Vì dịch chuyển tức thời cho phép di chuyển tự do giữa các cổng đang mở, chúng ta không cần phải lo lắng về thứ tự truy cập các cổng trung gian - chỉ quan trọng là khoảng cách tối đa. 

Một cách thuận tiện để chính thức hóa điều này là trước tiên hãy tính khoảng cách ngắn nhất từ ​​thành phố 1 đến tất cả các thành phố bằng thuật toán Dijkstra. Tiếp theo, chúng tôi tính toán đường đi ngắn nhất giữa mỗi cặp thành phố cổng thông tin một cách gián tiếp: vì dịch chuyển tức thời không tốn chi phí nên việc mở bất kỳ cổng nào cũng cho phép di chuyển ngay lập tức đến bất kỳ cổng nào đã mở khác. Do đó, thời gian tối thiểu thực tế giảm xuống để tính toán đường đi ngắn nhất từ ​​thành phố 1 đến cổng xa nhất về khoảng cách đến cổng khác. Chúng ta có thể kết hợp Dijkstra và xử lý mảng đơn giản để xác định khoảng cách tối đa này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! * n) | O(n + m) | Quá chậm | 
| Tối ưu (Dijkstra + khoảng cách tối đa cổng thông tin) | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thể hiện mạng lưới thành phố dưới dạng danh sách lân cận. Mỗi thành phố lưu trữ thành phố lân cận và thời gian di chuyển đến thành phố đó. Việc sử dụng danh sách kề đảm bảo rằng Dijkstra sẽ chạy hiệu quả mà không cần quét cạnh dư thừa. 
2. Đánh dấu tất cả các thành phố cổng thông tin trong một mảng boolean để tra cứu nhanh. Điều này cho phép kiểm tra O(1) trong quá trình xử lý khoảng cách và giúp tách biệt các khoảng cách liên quan ở cuối. 
3. Chạy thuật toán Dijkstra từ thành phố 1. Duy trì hàng đợi ưu tiên của các cặp (khoảng cách, thành phố). Đối với mỗi thành phố được trích xuất, hãy cập nhật khoảng cách của các thành phố lân cận nếu tìm thấy đường đi ngắn hơn. Bước này tạo ra thời gian di chuyển tối thiểu từ thành phố xuất phát đến mọi thành phố khác, chỉ xem xét các con đường thông thường. 
4. Sau khi Dijkstra hoàn thành, hãy trích xuất khoảng cách đến tất cả các thành phố cổng thông tin. Xác định khoảng cách nhỏ nhất và khoảng cách lớn nhất giữa các thành phố cổng thông tin. Hãy để cái nhỏ nhất là`d_min`và lớn nhất là`d_max`. 
5. Tổng thời gian tối thiểu cần thiết để mở tất cả các cổng được đưa ra bởi`d_min + d_max`. Công thức này xuất phát từ việc Pavel có thể tiếp cận cánh cổng gần nhất trong`d_min`, mở nó và sau đó, bằng cách dịch chuyển tức thời, sẽ đến bất kỳ cổng nào khác ngay lập tức. Cổng xa nhất tính từ thành phố xuất phát sẽ xác định thời gian bổ sung cần thiết để đảm bảo tất cả các cổng được mở vì anh ta có thể phải đi dọc các con đường để đến cổng đầu tiên mở khóa dịch chuyển tức thời cho người khác. 
6. Ghi tổng này làm đáp án. 

Tại sao nó hoạt động: Điều bất biến là dịch chuyển tức thời làm giảm khoảng cách hiệu quả giữa bất kỳ cặp cổng nào đang mở xuống bằng 0. Do đó, tổng thời gian được xác định hoàn toàn bởi các con đường được sử dụng để đến cổng đầu tiên và cổng xa nhất dọc theo biểu đồ, vì các lượt truy cập cổng trung gian không làm tăng tổng thời gian di chuyển. Dijkstra đảm bảo rằng tất cả các khoảng cách đều ở mức tối thiểu và việc lựa chọn các khoảng cách cực xa đảm bảo rằng không có cổng nào không thể truy cập được. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v, w = map(int, input().split())
    adj[u].append((v, w))
    adj[v].append((u, w))

k = int(input())
portals = list(map(int, input().split()))
is_portal = [False] * (n + 1)
for p in portals:
    is_portal[p] = True

dist = [float('inf')] * (n + 1)
dist[1] = 0
heap = [(0, 1)]

while heap:
    d, u = heapq.heappop(heap)
    if d > dist[u]:
        continue
    for v, w in adj[u]:
        if dist[v] > d + w:
            dist[v] = d + w
            heapq.heappush(heap, (dist[v], v))

portal_distances = [dist[p] for p in portals]
ans = max(portal_distances)
print(ans)
```Danh sách kề lưu trữ các cạnh một cách hiệu quả. cập nhật Dijkstra`dist`với thời gian tối thiểu để đến từng thành phố ngay từ đầu. Chúng tôi đẩy các khoảng cách đã cập nhật vào vùng nhớ heap để đảm bảo xử lý được ưu tiên. Trích xuất khoảng cách đến các cổng và lấy mức tối đa trực tiếp tương ứng với thời gian cần thiết để đến cổng xa nhất trước khi dịch chuyển tức thời trở nên hữu ích. 

## Ví dụ đã hoạt động 

Mẫu 1:```
n = 3, m = 3
edges = [(1,2,1),(1,3,1),(2,3,1)]
portals = [1,2,3]
```| Bước | Trạng thái đống | Mảng phân phối | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | [(0,1)] | [0,inf,inf,inf] | Bắt đầu tại thành phố 1 | 
| Pop 1 | [] | [0,1,1,inf] | Cập nhật hàng xóm 2 và 3 | 
| Pop 2 | [(1,2)] | không thay đổi | Hàng xóm 3 khoảng cách đã là 1 | 
| Pop 3 | [(1,3)] | không thay đổi | Không có cập nhật | 

Khoảng cách cổng:`[0,1,1]`. Tối đa là 1. Đầu ra`1`. Dịch chuyển tức thời cho phép truy cập ngay vào tất cả những người khác sau cổng đầu tiên. Tổng thời gian tối thiểu là`1`. 

Mẫu 2 (đã thi công):```
n = 4, m = 3
edges = [(1,2,2),(2,3,2),(3,4,2)]
portals = [2,4]
```Dijkstra mang lại khoảng cách`[0,2,4,6]`. Khoảng cách cổng`[2,6]`. Tối đa là 6. Đầu ra`6`. Pavel đi 2 để đến cổng 2 đầu tiên, dịch chuyển tức thời chưa hữu ích, sau đó phải đi đến 4 qua 3. Tổng thời gian tối thiểu là 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Dijkstra xử lý mỗi cạnh một lần, chi phí hoạt động heap log n | 
| Không gian | O(n + m) | Danh sách kề và mảng khoảng cách | 

Cho n và m lên tới 10^5, hệ số logarit đảm bảo giải pháp chạy tốt trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ dễ dàng phù hợp dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq
    input = sys.stdin.readline
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
    k = int(input())
    portals = list(map(int, input().split()))
    dist = [float('inf')] * (n + 1)
    dist[1] = 0
    heap = [(0, 1)]
    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]:
            continue
        for v, w in adj[u]:
            if dist[v] > d + w:
                dist[v] =
```
