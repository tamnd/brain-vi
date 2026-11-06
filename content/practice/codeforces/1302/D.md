---
problem: 1302D
contest_id: 1302
problem_index: D
name: "Dijkstra"
contest_name: "AIM Tech Poorly Prepared Contest (unrated, funny, Div. 1 preferred)"
rating: 0
tags: []
answer: passed_samples
verified: true
solve_time_s: 118
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dd221-335c-83ec-b708-83f1f83c1158
---

# CF 1302D - Dijkstra 

**Đánh giá:** ? 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dd221-335c-83ec-b708-83f1f83c1158 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó mỗi kết nối giữa hai đỉnh có trọng số không âm. Nhiệm vụ là xác định tổng trọng lượng tối thiểu có thể cần thiết để di chuyển từ đỉnh 1 đến đỉnh n hoặc báo cáo rằng điều đó là không thể nếu không có chuỗi cạnh nào kết nối chúng. 

Đây là truy vấn đường đi ngắn nhất cổ điển trên biểu đồ có trọng số lên tới 200.000 đỉnh và 200.000 cạnh. Mỗi chi phí cạnh có thể lớn tới 10^9, do đó, mọi giải pháp đều phải quản lý cẩn thận tổng tích lũy bằng cách sử dụng số nguyên 64 bit. 

Kích thước ràng buộc ngay lập tức loại trừ mọi cách tiếp cận cố gắng tính toán lại các đường dẫn một cách triệt để. Tìm kiếm theo chiều rộng đầu tiên đơn giản là không hợp lệ vì trọng số cạnh không đồng nhất. Việc tính toán tất cả các cặp theo kiểu Floyd-Warshall là không thể vì nó cần thời gian O(n^3). Ngay cả việc thư giãn lặp đi lặp lại mà không có cấu trúc cũng sẽ giảm xuống O(nm), tốc độ này quá chậm khi cả n và m đều lớn. 

Một trường hợp khó nhận thấy xuất phát từ các biểu đồ bị ngắt kết nối. Ví dụ: nếu đầu vào là:```
4 2
1 2 3
3 4 5
```thì không có đường dẫn nào từ 1 đến 4 và kết quả đầu ra đúng là -1. Bất kỳ triển khai nào khởi tạo khoảng cách không chính xác hoặc giả định khả năng tiếp cận sẽ trả về không chính xác một giá trị mặc định lớn thay vì nhận ra sự ngắt kết nối này. 

Một trường hợp quan trọng khác là có nhiều cạnh giữa cùng một cặp đỉnh. Vì biểu đồ không đơn giản nên có thể có một số chi phí khác nhau giữa hai nút. Một giải pháp đúng phải luôn xem xét cách chuyển đổi hiệu quả rẻ nhất, không ghi đè một cách mù quáng hoặc giả định tính duy nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp nhưng không chính xác là thử khám phá tất cả các đường đi có thể từ 1 đến n, tích lũy chi phí dọc theo mỗi đường đi và theo dõi mức tối thiểu. Điều này nhanh chóng trở thành hàm mũ theo số đỉnh, vì mỗi nhánh trong biểu đồ sẽ nhân đôi số đường đi từng phần đang được xem xét. Ngay cả việc xem xét lại việc cắt tỉa cũng chỉ cải thiện được tình hình một chút, nhưng trường hợp xấu nhất vẫn bùng nổ vì có thể đạt đến cùng một đỉnh thông qua nhiều bối cảnh chi phí khác nhau. 

Một nỗ lực có cấu trúc hơn là chạy truyền tải giống như tìm kiếm theo chiều rộng trong khi giảm khoảng cách bất cứ khi nào tìm thấy đường đi ngắn hơn. Điều này chỉ hoạt động nếu tất cả các cạnh có trọng số bằng nhau, điều này không xảy ra ở đây. Với trọng số tùy ý, BFS có thể hoàn thiện một nút quá sớm và bỏ lỡ đường đi rẻ hơn được phát hiện sau đó. 

Quan sát quan trọng là tất cả các trọng số của cạnh đều không âm, vì vậy khi chúng ta biết khoảng cách dự kiến ​​tốt nhất hiện tại đến một đỉnh, chúng ta có thể mở rộng các đỉnh một cách an toàn theo thứ tự khoảng cách tăng dần. Đây chính xác là điều kiện cần thiết cho thuật toán Dijkstra. Thay vì khám phá một cách mù quáng, chúng tôi luôn xử lý đỉnh tiếp theo với khoảng cách nhỏ nhất đã biết, đảm bảo rằng khi xử lý, khoảng cách ngắn nhất của nó là cuối cùng. 

Thứ tự này được duy trì một cách hiệu quả bằng cách sử dụng hàng đợi ưu tiên heap tối thiểu. Mỗi lần chúng tôi cải thiện khoảng cách đến một nút, chúng tôi sẽ đẩy nút đó vào vùng heap. Mỗi lần trích xuất cho chúng ta đỉnh hứa hẹn nhất tiếp theo để mở rộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê đường dẫn vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Dijkstra với Min Heap | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu diễn danh sách kề của đồ thị. Mỗi đỉnh lưu trữ tất cả các cạnh đi ra dưới dạng cặp (láng giềng, chi phí). Cấu trúc này cho phép lặp nhanh qua các hàng xóm trong quá trình thư giãn. 
2. Khởi tạo một mảng khoảng cách trong đó mọi đỉnh được đặt thành vô cùng, ngoại trừ đỉnh 1 được đặt thành 0. Điều này mã hóa thực tế là chúng ta bắt đầu ở nút 1 với chi phí bằng 0. 
3. Tạo hàng đợi ưu tiên heap tối thiểu và chèn cặp (0, 1). Heap được sử dụng để luôn mở rộng đỉnh được biết gần nhất hiện tại. 
4. Trong khi heap không trống, hãy trích xuất đỉnh u có khoảng cách d nhỏ nhất đã biết. Đỉnh này đại diện cho điểm biên giới hứa hẹn nhất. 
5. Nếu khoảng cách d được trích xuất lớn hơn khoảng cách được lưu trữ cho u, hãy bỏ qua nó. Điều này ngăn chặn việc xử lý các mục heap lỗi thời đã được đẩy trước khi phát hiện ra đường dẫn tốt hơn. 
6. Với mọi hàng xóm v của u có trọng số cạnh w, hãy kiểm tra xem việc tiếp cận v qua u có cải thiện khoảng cách được biết tốt nhất hiện tại của nó hay không. Nếu d + w nhỏ hơn dist[v], cập nhật dist[v] và đẩy (dist[v], v) vào heap. Bước này truyền bá những cải tiến ra bên ngoài từ những con đường ngắn nhất đã được xác nhận. 
7. Sau khi heap đã hết, hãy kiểm tra dist[n]. Nếu vẫn là vô cùng, xuất -1; nếu không thì xuất ra dist[n]. 

### Tại sao nó hoạt động 

Thuật toán dựa trên đặc tính là khi một nút bị xóa khỏi hàng đợi ưu tiên có khoảng cách nhỏ nhất trong số tất cả các ứng cử viên thì khoảng cách đó là cuối cùng. Bất kỳ đường dẫn thay thế nào đến nút đó sẽ phải đi qua một nút khác có khoảng cách dự kiến ​​bằng hoặc lớn hơn, điều này không thể tạo ra kết quả nhỏ hơn mà không mâu thuẫn với thứ tự được thực thi bởi vùng heap. Điều này đảm bảo rằng mọi nút đều được giải quyết theo thứ tự tăng dần của khoảng cách ngắn nhất thực sự, đảm bảo tính chính xác của tất cả các lần thư giãn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

n, m = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(m):
    a, b, c = map(int, input().split())
    g[a].append((b, c))
    g[b].append((a, c))

dist = [INF] * (n + 1)
dist[1] = 0

pq = [(0, 1)]

while pq:
    d, u = heapq.heappop(pq)

    if d != dist[u]:
        continue

    for v, w in g[u]:
        nd = d + w
        if nd < dist[v]:
            dist[v] = nd
            heapq.heappush(pq, (nd, v))

print(-1 if dist[n] == INF else dist[n])
```Việc xây dựng danh sách kề đảm bảo rằng mỗi cạnh được lưu trữ chính xác hai lần, một lần cho mỗi hướng, vì đồ thị là vô hướng. Mảng khoảng cách sử dụng một giá trị trọng điểm lớn để biểu thị các đỉnh chưa được chạm tới và các số nguyên chính xác tùy ý của Python xử lý một cách an toàn các tổng đường dẫn lớn mà không gặp sự cố tràn. 

Heap luôn lưu trữ các trạng thái ứng cử viên và việc kiểm tra`d != dist[u]`loại bỏ các mục cũ một cách hiệu quả. Nếu không có sự kiểm tra này, thuật toán vẫn đúng nhưng chậm hơn do mở rộng dư thừa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 5
2 3 1
1 3 7
```Chúng tôi theo dõi trạng thái thuật toán. 

| Bước | Nút xuất hiện | mảng dist (1,2,3) | Hành động | 
| --- | --- | --- | --- | 
| 1 | (0,1) | (0,∞,∞) | thư giãn 2→5, 3→7 | 
| 2 | (5,2) | (0,5,7) | thư giãn 3→6 | 
| 3 | (6,3) | (0,5,6) | xong | 

Heap xử lý nút 2 trước nút 3 vì 5 < 7, cho phép phát hiện tuyến ngắn hơn tới nút 3. Điều này xác nhận rằng việc mở rộng tham lam là cần thiết. 

Đầu ra là 6. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 4
2 3 5
1 4 20
```| Bước | Nút xuất hiện | mảng dist (1,2,3,4) | Hành động | 
| --- | --- | --- | --- | 
| 1 | (0,1) | (0,4,∞,20) | hàng xóm thư giãn | 
| 2 | (4,2) | (0,4,9,20) | thư giãn 3 | 
| 3 | (9,3) | (0,4,9,20) | không thay đổi | 
| 4 | (20,4) | (0,4,9,20) | xong | 

Điều này cho thấy các đường dẫn gián tiếp được phát hiện dần dần như thế nào và các cạnh trực tiếp nhưng tốn kém bị bỏ qua một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | mỗi cạnh có thể kích hoạt một bản cập nhật heap, mỗi bản cập nhật có chi phí log n | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và lưu trữ đống | 

Với tối đa 200.000 nút và cạnh, độ phức tạp này phù hợp một cách thoải mái trong giới hạn thời gian vì hệ số logarit vẫn nhỏ và mỗi thao tác đều tuyến tính trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    INF = 10**30

    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b, c = map(int, input().split())
        g[a].append((b, c))
        g[b].append((a, c))

    dist = [INF] * (n + 1)
    dist[1] = 0

    pq = [(0, 1)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return str(-1 if dist[n] == INF else dist[n])

# provided sample
assert run("""3 3
1 2 5
2 3 1
1 3 7
""") == "6"

# custom: disconnected graph
assert run("""4 2
1 2 3
3 4 5
""") == "-1"

# custom: multiple edges
assert run("""3 3
1 2 10
1 2 2
2 3 3
""") == "5"

# custom: direct vs indirect
assert run("""4 4
1 4 100
1 2 1
2 3 1
3 4 1
""") == "3"

# custom: single node
assert run("""1 0
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị bị ngắt kết nối | -1 | xử lý không thể truy cập | 
| nhiều cạnh | 5 | giảm thiểu cạnh chính xác | 
| đường dẫn gián tiếp tốt hơn đường dẫn trực tiếp | 3 | Dijkstra đúng đắn | 
| nút đơn | 0 | tính đúng đắn của trường hợp cơ sở | 

## Vỏ cạnh 

Một biểu đồ bị ngắt kết nối như`1-2`Và`3-4`kiểm tra xem các nút không thể truy cập có được phát hiện chính xác hay không. Khoảng cách đến nút n vẫn là vô cùng và thuật toán trả về -1 mà không thử các đường dẫn không hợp lệ. 

Nhiều cạnh giữa các nút giống nhau đảm bảo rằng logic thư giãn không có tính duy nhất. Trong trường hợp thử nghiệm với các cạnh`1-2 (10)`Và`1-2 (2)`, heap xử lý cả hai, nhưng chỉ có trọng số nhỏ hơn cập nhật khoảng cách và thuật toán truyền chính xác giá trị 2. 

Biểu đồ trong đó cạnh trực tiếp kém hơn đường dẫn gián tiếp làm nổi bật tầm quan trọng của việc sắp xếp theo khoảng cách. Cạnh trực tiếp tới nút 4 là 100, nhưng chuỗi 1→2→3→4 tạo ra 3. Vùng heap đảm bảo các nút được mở rộng với chi phí ngày càng tăng, cho phép phát hiện lộ trình nhiều bước ngắn hơn trước và ghi đè chính xác dự đoán trực tiếp ban đầu.
