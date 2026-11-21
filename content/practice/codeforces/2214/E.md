---
title: "CF 2214E - Đường đi ngắn nhất"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số lên đến một trăm đỉnh. Mỗi cạnh kết nối hai nút được đánh số và có chi phí không âm."
date: "2026-06-07T19:01:45+07:00"
tags: ["codeforces", "competitive-programming", "*special", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2214
codeforces_index: "E"
codeforces_contest_name: "April Fools Day Contest 2026"
rating: 0
weight: 2214
solve_time_s: 70
verified: true
draft: false
---

[CF 2214E - Đường dẫn ngắn nhất](https://codeforces.com/problemset/problem/2214/E) 

**Đánh giá:** - 
**Tags:** *đường đi đặc biệt, ngắn nhất 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số lên đến một trăm đỉnh. Mỗi cạnh kết nối hai nút được đánh số và có chi phí không âm. Nhiệm vụ là tính khoảng cách tối thiểu có thể từ nút 1 đến mọi nút khác, trong đó khoảng cách có nghĩa là tổng trọng số của các cạnh dọc theo đường dẫn đã chọn. 

Đầu vào có thể được hiểu là mạng lưới các thành phố được kết nối bằng đường bộ với chi phí đi lại. Bắt đầu từ thành phố 1, chúng tôi muốn biết cách rẻ nhất để đến mọi thành phố khác hoặc xác định rằng không thể đến được thành phố nào. 

Các ràng buộc đủ nhỏ để các phương pháp đường đi ngắn nhất cổ điển là đủ. Với tối đa 100 nút, ma trận kề hoặc danh sách kề đều phù hợp một cách thoải mái. Số cạnh có thể đạt tới khoảng 5000 trong trường hợp xấu nhất, do đó, bất kỳ giải pháp nào có hành vi gần như bậc hai hoặc siêu bậc hai vẫn sẽ chạy nhanh. Điều này ngay lập tức loại trừ bất kỳ tìm kiếm theo cấp số nhân nào trên các đường dẫn, nhưng quan trọng hơn là nó làm cho Dijkstra tiêu chuẩn với một đống nhị phân trở nên đủ nhanh. 

Trường hợp biên tinh tế xuất hiện khi một số nút bị ngắt kết nối khỏi nút 1. Trong những trường hợp như vậy, đầu ra chính xác là -1, không phải 0 hoặc vô cực hoặc giá trị trọng điểm lớn. 

Một trường hợp góc quan trọng khác là các cạnh có trọng lượng bằng không. Vì trọng số có thể bằng 0 nên mọi thuật toán dựa trên BFS đơn giản hoặc giả sử trọng số dương hoàn toàn sẽ thất bại. Ví dụ: nếu chúng ta có một chuỗi như 1-2 (0), 2-3 (5), thì có thể truy cập được nút 3 với chi phí là 5, nhưng BFS ngây thơ sẽ xử lý không chính xác tất cả các cạnh như nhau. 

Vấn đề thứ ba phát sinh từ nhiều cạnh giữa cùng một cặp nút. Do biểu đồ không được đảm bảo đơn giản trong việc diễn giải đầu vào nên việc triển khai bất cẩn sẽ ghi đè các mục liền kề thay vì giữ cạnh trọng số tối thiểu có thể làm tăng khoảng cách một cách không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các đường dẫn có thể từ nút 1 đến mọi nút khác, theo dõi chi phí tối thiểu gặp phải. Về mặt khái niệm, điều này đúng vì nó khám phá toàn bộ không gian tìm kiếm của các bước đi trong biểu đồ. Tuy nhiên, số lượng đường dẫn tăng theo cấp số nhân. Ngay cả trong một biểu đồ có hệ số phân nhánh khiêm tốn, số bước đi riêng biệt có độ dài lên tới n có thể vượt quá bất kỳ giới hạn khả thi nào. Điều này trở nên đặc biệt rõ ràng trong các biểu đồ dày đặc, trong đó mỗi nút kết nối với nhiều nút khác và việc xem lại các trạng thái sẽ tạo ra một sự mở rộng tổ hợp không giới hạn. 

Quan sát cấu trúc quan trọng là khi chúng ta biết khoảng cách ngắn nhất tới một nút, chúng ta không bao giờ cần phải xem xét lại cách dài hơn để tiếp cận nút đó. Thuộc tính “hoàn thiện” đơn điệu này đúng vì tất cả các trọng số của cạnh đều không âm. Nó cho phép chúng ta mở rộng một cách tham lam từ nút hiện chưa được tiếp cận gần nhất, luôn khóa khoảng cách tối ưu theo thứ tự tăng dần. Đây chính xác là cài đặt áp dụng thuật toán của Dijkstra. 

Sự cải thiện này đến từ việc thay thế việc liệt kê đường dẫn toàn cục bằng việc nới lỏng cục bộ. Thay vì khám phá các đường dẫn đầy đủ, chúng tôi liên tục cải thiện các khoảng cách đã biết rõ nhất bằng cách sử dụng các cạnh rời khỏi các nút đã tiếp cận, luôn ưu tiên khoảng cách dự kiến ​​nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Con đường vũ phu | Hàm mũ | O(n) đệ quy | Quá chậm | 
| Dijkstra (đống) | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề trong đó mỗi nút lưu trữ các cặp trọng số lân cận và cạnh. Việc biểu diễn này là cần thiết vì chúng ta liên tục đi qua các cạnh hướng ra ngoài một cách hiệu quả trong quá trình thư giãn. 
2. Khởi tạo một mảng khoảng cách có vô cực cho tất cả các nút ngoại trừ nút 1, được đặt thành 0. Điều này mã hóa thực tế là chúng ta bắt đầu ở nút 1 mà không mất phí. 
3. Đẩy cặp (0, 1) vào hàng ưu tiên. Hàng đợi luôn lưu trữ các trạng thái ứng cử viên được sắp xếp theo khoảng cách ngắn nhất đã biết hiện tại. 
4. Liên tục trích xuất nút có khoảng cách dự kiến ​​nhỏ nhất từ ​​hàng đợi. Nút này đại diện cho đỉnh chưa hoàn thiện gần nhất theo kiến ​​thức hiện tại. 
5. Nếu khoảng cách được trích xuất lớn hơn khoảng cách được lưu trữ cho nút đó, hãy bỏ qua nó. Điều này xảy ra vì một đường dẫn tốt hơn đã được tìm thấy và được đẩy vào hàng đợi sau đó. 
6. Đối với mỗi lân cận của nút hiện tại, hãy tính khoảng cách ứng viên qua nút này. Nếu nó cải thiện khoảng cách được lưu trữ, hãy cập nhật nó và đẩy cặp mới vào hàng ưu tiên. 
7. Tiếp tục cho đến khi hàng đợi ưu tiên trống, nghĩa là tất cả các nút có thể truy cập đã được xử lý theo thứ tự khoảng cách tăng dần. 
8. Xuất khoảng cách cuối cùng cho các nút từ 2 đến n, thay thế các nút không thể truy cập (vẫn là vô cực) bằng -1. 

Lý do bước 5 rất cần thiết là vì cùng một nút có thể xuất hiện nhiều lần trong hàng đợi với khoảng cách đã lỗi thời. Nếu không bỏ qua các mục nhập cũ, chúng tôi sẽ nới lỏng các cạnh một cách không chính xác bằng cách sử dụng các giá trị dưới mức tối ưu và tăng đáng kể thời gian chạy. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, khi một nút được đưa ra khỏi hàng đợi ưu tiên với khoảng cách d, chúng tôi cho rằng d là khoảng cách nhỏ nhất có thể từ nút 1 đến nút đó. Điều này đúng vì tất cả các trọng số của cạnh đều không âm, do đó, bất kỳ đường dẫn thay thế nào đến nút này sau đó đều phải tích lũy chi phí ít nhất bằng đường dẫn tốt nhất hiện được phát hiện. Do đó, một khi một nút được bật lên, khoảng cách của nó sẽ được xác định và sẽ không bao giờ được cải thiện. Bất biến này đảm bảo rằng tất cả các khoảng thư giãn đều truyền đúng khoảng cách tối thiểu ra bên ngoài. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
    
    INF = 10**18
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
    
    for i in range(2, n + 1):
        if dist[i] == INF:
            print(-1)
        else:
            print(dist[i])

if __name__ == "__main__":
    solve()
```Việc xây dựng danh sách kề đảm bảo mỗi cạnh vô hướng đều có thể sử dụng được theo cả hai hướng. Mảng khoảng cách được khởi tạo với một trọng điểm lớn để biểu thị các đường đi ngắn nhất chưa xác định. Hàng đợi ưu tiên luôn mở rộng nút biên giới được biết đến nhiều nhất hiện nay. 

Kiểm tra mục nhập cũ`if d != dist[u]`ngăn chặn các mục hàng đợi lỗi thời gây ra sự thư giãn không cần thiết. Đây là sự tối ưu hóa tiêu chuẩn giúp Dijkstra hoạt động hiệu quả trong các bản cập nhật lặp đi lặp lại. 

Cuối cùng, các nút không thể truy cập vẫn ở mức vô cùng và được chuyển đổi thành -1 ở đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2 1
```Chúng ta bắt đầu với dist[1] = 0 và dist[2] = INF. 

| Bước | Trạng thái PQ | Nút được chọn | quận [2] | 
| --- | --- | --- | --- | 
| ban đầu | (0,1) | - | INF | 
| bật 1 | trống | 1 | INF | 
| thư giãn (1→2) | (1,2) | - | 1 | 
| bật 2 | trống | 2 | 1 | 

Điều này cho thấy một lần thư giãn cập nhật trực tiếp nút 2 và không tồn tại đường dẫn thay thế nào. 

Đầu ra:```
1
```### Ví dụ 2 

đầu vào:```
4 4
1 2 1
2 3 2
1 3 10
3 4 1
```Chúng tôi theo dõi khoảng cách ngắn nhất khi chúng được cải thiện. 

| Bước | Trạng thái PQ | Nút được chọn | quận [2] | quận [3] | quận [4] | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | (0,1) | - | INF | INF | INF | 
| bật 1 | (1,2),(10,3) | 1 | 1 | 10 | INF | 
| bật 2 | (3,3),(10,3) | 2 | 1 | 3 | INF | 
| bật 3 | (4,4),(10,3) | 3 | 1 | 3 | 4 | 
| bật 4 | (10,3) | 4 | 1 | 3 | 4 | 

Cạnh trực tiếp 1→3 kém hơn đường dẫn 1→2→3 và Dijkstra đã sửa chính xác điều này khi xử lý nút 2 trước. 

Đầu ra:```
1
3
4
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần nới lỏng cạnh có thể đẩy một mục vào heap, mỗi thao tác trên heap tốn log n | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và đống | 

Với n ≤ 100 và m lên tới khoảng 5000, điều này nằm trong giới hạn thoải mái. Ngay cả trong trường hợp xấu nhất, các thao tác trên heap vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, sys.stdin.readline().split())
    adj = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v, w = map(int, sys.stdin.readline().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
    
    INF = 10**18
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
    
    out = []
    for i in range(2, n + 1):
        out.append(str(-1 if dist[i] == INF else dist[i]))
    
    return "\n".join(out)

# provided sample
assert run("2 1\n1 2 1\n") == "1"

# disconnected node
assert run("3 1\n1 2 5\n") == "5\n-1"

# zero-weight edges
assert run("3 2\n1 2 0\n2 3 5\n") == "0\n5"

# multiple edges
assert run("3 3\n1 2 10\n1 2 3\n2 3 1\n") == "3\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1, 1 2 5 | 5, -1 | xử lý nút không thể truy cập | 
| xích không trọng lượng | 0, 5 | tính đúng đắn với các cạnh bằng 0 | 
| cạnh trùng lặp | 3, 4 | xử lý cạnh tối thiểu | 

## Vỏ cạnh 

Kịch bản nút bị ngắt kết nối cho thấy mức độ vô cực được bảo toàn như thế nào. Coi như:```
3 1
1 2 5
```Nút 3 không bao giờ đạt được. Thuật toán không bao giờ đẩy nó vào hàng đợi, vì vậy khoảng cách của nó vẫn là INF và được in là -1. 

Trường hợp có cạnh không trọng lượng:```
3 2
1 2 0
2 3 5
```Ban đầu dist[2] trở thành 0. Khi xử lý nút 2, nó giảm cạnh xuống 3 với chi phí 5. Kết quả cuối cùng phản ánh chính xác rằng việc truyền tải chi phí bằng 0 được cho phép mà không vi phạm thứ tự Dijkstra. 

Trường hợp nhiều cạnh:```
3 3
1 2 10
1 2 1
2 3 1
```Cạnh thứ hai cải thiện dist[2] thành 1 và tất cả các lần nới lỏng sau này đều sử dụng giá trị được cải thiện này, đảm bảo tính chính xác ngay cả khi đầu vào chứa các cạnh dư thừa hoặc bị chi phối.
