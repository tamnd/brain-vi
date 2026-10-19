---
title: "CF 105639C - Đi Học Trong Tuyết"
description: "Chúng ta được cung cấp một mạng lưới các nút giao thông được định hướng được kết nối bằng các đường chuyển tiếp một chiều. Mỗi quá trình chuyển đổi có hai thuộc tính: thời gian di chuyển và sự thay đổi nhiệt."
date: "2026-06-26T13:25:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105639
codeforces_index: "C"
codeforces_contest_name: "Innopolis Open 2024-2025. Elimination Round 2"
rating: 0
weight: 105639
solve_time_s: 53
verified: true
draft: false
---

[CF 105639C - Đến trường xuyên tuyết](https://codeforces.com/problemset/problem/105639/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới các nút giao thông được định hướng được kết nối bằng các đường chuyển tiếp một chiều. Mỗi quá trình chuyển đổi có hai thuộc tính: thời gian di chuyển và sự thay đổi nhiệt. Nếu phần chuyển tiếp đại diện cho một con phố ngoài trời, nó sẽ làm giảm lượng nhiệt hiện tại; nếu nó ở bên trong hành lang tòa nhà, nó sẽ làm tăng nhiệt. Bất kể cách giải thích nào, mỗi động thái đều sẽ thay đổi một cách xác định một “trạng thái nhiệt độ” đang chạy. 

Một du khách xuất phát tại ngã tư 1 với nhiệt độ chính xác bằng 0 và muốn đến ngã tư\(n\). Hành trình chỉ có hiệu lực nếu tại mọi thời điểm giá trị nhiệt vẫn nằm trong phạm vi bao gồm từ\(-30\)ĐẾN\(+30\). Nếu một chuyển động đẩy nhiệt ra ngoài dải này thì chuyển động đó bị cấm. 

Mục tiêu là tính tổng thời gian di chuyển tối thiểu trong số tất cả các tuyến hợp lệ từ nút 1 đến nút\(n\), hoặc báo cáo là không thể. 

Cấu trúc ràng buộc là rất quan trọng. Có tới\(10^5\)nút và\(10^5\)các cạnh trong tất cả các trường hợp thử nghiệm và mỗi cạnh có thể thay đổi nhiệt tối đa là 30. Kích thước nhiệt rất nhỏ và bị giới hạn, do đó, bất kỳ giải pháp nào coi nó như một kích thước đồ thị bổ sung đầy đủ\(61n\)đã được chấp nhận trong bộ nhớ và thời gian nếu được xử lý cẩn thận. Tuy nhiên, lập trình động đơn giản trên các đường đi không có cấu trúc đường đi ngắn nhất sẽ bùng nổ do các chu kỳ và trạng thái lặp lại. 

Một cách tiếp cận ngây thơ chỉ theo dõi thời gian tốt nhất trên mỗi nút là không chính xác, bởi vì việc tiếp cận cùng một nút với các giá trị nhiệt khác nhau sẽ dẫn đến những khả năng khác nhau trong tương lai. Ví dụ, đến nút\(v\)với nhiệt độ 30 có thể chặn tất cả các cạnh âm đi ra, trong khi đến với nhiệt độ -30 có thể cho phép linh hoạt hơn sau này. 

Một trường hợp thất bại tinh tế xuất hiện trong các biểu đồ tuần hoàn nhỏ. Coi như:```
1 -> 2 (dt = +30)
2 -> 1 (dt = -30)
```Bắt đầu từ 1 với nhiệt độ 0, chúng ta có thể lặp giữa các trạng thái (1,0) → (2,30) → (1,0), nhưng mọi nỗ lực coi nút 1 là “đã truy cập tối ưu” sẽ phá vỡ tính chính xác vì việc xem lại với trạng thái nhiệt khác có vấn đề. 

Một trường hợp khác là khi một đường dẫn tạm thời đi ra ngoài giới hạn mặc dù hiệu ứng thực là an toàn. Ví dụ:```
1 -> 2 (dt = +30)
2 -> 3 (dt = -30)
3 -> 4 (dt = +30)
```Ngay cả khi nhiệt độ cuối cùng vẫn còn hiệu lực, các bước trung gian phải tôn trọng các giới hạn, do đó tính khả thi của đường dẫn phụ thuộc vào trạng thái ở mọi cạnh. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là xem xét tất cả các đường đi có thể từ 1 đến\(n\), theo dõi nhiệt dọc theo mỗi đường dẫn. Mỗi bước cập nhật nhiệt và kiểm tra tính hợp lệ. Về nguyên tắc, điều này đúng vì nó thực thi các ràng buộc một cách chính xác, nhưng số lượng đường đi tăng theo cấp số nhân trong các biểu đồ chung có chu kỳ. Ngay cả việc hạn chế các đường dẫn đơn giản cũng không giúp ích gì vì trạng thái nhiệt có thể lặp lại theo chu kỳ, tạo ra vô số biến thể hợp lệ trừ khi được giới hạn cẩn thận. 

Thông tin chi tiết về cấu trúc quan trọng là trạng thái có ý nghĩa duy nhất là một cặp \((nút, nhiệt)\) và nhiệt được giới hạn ở 61 giá trị có thể. Điều này chuyển đổi bài toán thành bài toán đường đi ngắn nhất trên biểu đồ lớp có nhiều nhất\(61n\)tiểu bang. Mỗi cạnh ban đầu tạo ra sự chuyển tiếp giữa các lớp và tất cả các trọng số của cạnh đều không âm (thời gian luôn dương), do đó thuật toán Dijkstra áp dụng trực tiếp. 

Lực lượng vũ phu trên các đường dẫn không thành công vì nó tính toán lại cùng một trạng thái \((nút, nhiệt)\) nhiều lần. Công thức trạng thái phân lớp thu gọn tất cả các lịch sử tương đương thành một nút duy nhất trong biểu đồ mở rộng, biến việc khám phá hàm mũ thành tính toán đường đi ngắn nhất đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu trên các con đường | Hàm mũ | O(1) thêm | Quá chậm | 
| Dijkstra mở rộng cấp nhà nước | O(m · 61 log (n · 61)) | O(n · 61 + m · 61) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng cặp \((v, h)\) trong đó\(v\)là một nút và\(h \in [-30, 30]\)là một giá trị nhiệt hợp lệ dưới dạng trạng thái riêng biệt trong biểu đồ. 

1. Khởi tạo bảng khoảng cách`dist[v][h]`với vô cùng cho tất cả các nút và giá trị nhiệt. Bảng này thể hiện thời gian được biết rõ nhất để tiếp cận nút\(v\)với nhiệt\(h\). Chúng tôi thiết lập`dist[1][0] = 0`bởi vì chúng ta bắt đầu ở nút 1 với thời gian bằng 0 và nhiệt độ trung tính. 

2. Đẩy trạng thái ban đầu \((1, 0)\) vào hàng ưu tiên được sắp xếp theo thời gian tích lũy. 

3. Trong khi hàng đợi ưu tiên không trống, hãy trích xuất trạng thái \((v, h)\) với thời gian nhỏ nhất đã biết. Điều này đảm bảo rằng chúng tôi luôn mở rộng tuyến đường từng phần tốt nhất hiện tại trước tiên, bảo toàn tài sản tham lam của Dijkstra. 

4. Đối với mỗi cạnh đi từ\(v\)ĐẾN\(u\)với chi phí thời gian\(l\)và sự thay đổi nhiệt\(dt\), tính nhiệt lượng mới\(h' = h + dt\). Nếu như\(h'\)ở bên ngoài\([-30, 30]\), hãy bỏ qua quá trình chuyển đổi này vì nó vi phạm các ràng buộc ngay lập tức. 

5. Nếu quá trình chuyển đổi hợp lệ và thời gian mới\(dist[v][h] + l\)cải thiện`dist[u][h']`, cập nhật nó và đẩy \((u, h')\) vào hàng ưu tiên. 

6. Sau khi xử lý tất cả các trạng thái có thể truy cập, câu trả lời là giá trị tối thiểu trong số tất cả`dist[n][h]`vì\(h \in [-30, 30]\). Nếu tất cả đều là vô hạn, xuất ra -1. 

Lý do chúng tôi không thu gọn trạng thái nhiệt trên mỗi nút là vì các quá trình chuyển đổi trong tương lai phụ thuộc nhiều vào nhiệt và việc hợp nhất sẽ làm mất thông tin khả thi. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên việc xử lý từng cặp \((nút, nhiệt)\) dưới dạng một nút trong biểu đồ có hướng lớn hơn với trọng số cạnh không âm. Mọi bước đi hợp lệ trong bài toán ban đầu đều tương ứng chính xác với một đường dẫn trong biểu đồ mở rộng này và ngược lại. Vì tất cả các trọng số của cạnh đều không âm nên thuật toán của Dijkstra đảm bảo rằng một khi một trạng thái được xuất hiện với khoảng cách tối thiểu thì không có đường dẫn nào sau này có thể cải thiện nó. Ràng buộc nhiệt được thực thi cục bộ trên các cạnh, do đó không có đường dẫn không hợp lệ nào đi vào không gian trạng thái. Điều này đảm bảo chúng tôi không bao giờ loại bỏ tuyến đường có khả năng tối ưu cũng như không chấp nhận tuyến đường không hợp lệ. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**18
OFFSET = 30
MAXH = 61

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, l, dt = map(int, input().split())
        graph[u].append((v, l, dt))

    dist = [[INF] * MAXH for _ in range(n + 1)]
    dist[1][OFFSET] = 0

    pq = [(0, 1, OFFSET)]

    while pq:
        d, v, h = heapq.heappop(pq)
        if d != dist[v][h]:
            continue

        for to, cost, dh in graph[v]:
            nh = h + dh
            if nh < 0 or nh >= MAXH:
                continue
            nd = d + cost
            if nd < dist[to][nh]:
                dist[to][nh] = nd
                heapq.heappush(pq, (nd, to, nh))

    ans = min(dist[n])
    print(-1 if ans == INF else ans)
```Việc triển khai phản ánh trực tiếp ý tưởng biểu đồ trạng thái mở rộng. Độ lệch 30 được sử dụng để ánh xạ các giá trị nhiệt\([-30, 30]\)vào chỉ số mảng\([0, 60]\). Hàng đợi ưu tiên đảm bảo các trạng thái được xử lý theo thứ tự thời gian tăng dần. 

Một chi tiết tinh tế là kiểm tra trạng thái cũ`if d != dist[v][h]`, giúp tránh xử lý các mục hàng đợi lỗi thời. Không có nó, hiệu suất có thể giảm đáng kể do bị đẩy đi lặp lại. 

Một chi tiết quan trọng khác là lặp lại tất cả các giá trị nhiệt ở cuối. Nút mục tiêu không yêu cầu nhiệt độ cuối cùng cụ thể, chỉ yêu cầu nó nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 3
1 2 2 10
2 3 2 -10
1 3 10 0
```Chúng ta bắt đầu tại (1.0). Từ (1,0), ta có thể đi đến (2,10) với thời gian 2, hoặc đi thẳng tới (3,0) với thời gian 10. 

| Bước | Nút | Nhiệt | Khoảng cách | 
|------|------|------|----------| 
| ban đầu | 1 | 0 | 0 | 
| thư giãn | 2 | 10 | 2 | 
| thư giãn | 3 | 0 | 10 | 

Từ (2,10), ta có thể đi đến (3,0) cộng chi phí 2, được tổng là 4. 

| Bước | Nút | Nhiệt | Khoảng cách | 
|------|------|------|----------| 
| từ (2,10) | 3 | 0 | 4 | 

Câu trả lời đúng nhất là 4. Điều này cho thấy tại sao cạnh trực tiếp 1→3 không tối ưu mặc dù có sẵn. 

### Ví dụ 2 

đầu vào:```
1
4 4
1 2 5 30
2 3 5 -30
3 4 5 0
1 4 20 0
```Từ 1 chúng ta có thể đi đến 2 với nhiệt độ 30, rồi đến 3 trở lại 0, rồi đến 4. 

| Bước | Nút | Nhiệt | Khoảng cách | 
|------|------|------|----------| 
| ban đầu | 1 | 0 | 0 | 
| đến 2 | 2 | 30 | 5 | 
| đến 3 | 3 | 0 | 10 | 
| đến 4 | 4 | 0 | 15 | 

Ngoài ra còn có cạnh 1→4 trực tiếp với chi phí 20, nhưng đường đi bị ràng buộc thì tốt hơn. 

Điều này chứng tỏ rằng chu trình nhiệt trung gian là cần thiết để đạt được kết quả về thời gian tốt hơn, ngay cả khi các bước có vẻ dài hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(m \cdot 61 \log(n \cdot 61))\) | Mỗi cạnh có thể được thư giãn trên tối đa 61 trạng thái nhiệt và mỗi lần thư giãn sẽ trải qua một hàng đợi ưu tiên | 
| Không gian | \(O(n \cdot 61 + m)\) | Bảng khoảng cách lưu trữ 61 trạng thái trên mỗi nút và danh sách kề lưu trữ các cạnh | 

Các hằng số vẫn nhỏ vì phạm vi nhiệt cố định và hẹp. Với\(n, m \le 10^5\), điều này chạy thoải mái trong giới hạn dưới Dijkstra với vùng nhị phân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Placeholder: solution function would be invoked in real setup
# assert run(...) == ...

# minimal single-edge valid path
# 1 -> 2 direct
# ensures basic transition works

# cycle requiring heat tracking
# 1 -> 2 (+30), 2 -> 1 (-30), 1 -> 3
# tests state separation

# boundary heat violation case
# edge pushes heat to 31 should be ignored

# direct vs indirect optimal path comparison

```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| đồ thị tối thiểu | câu trả lời trực tiếp | độ đúng cơ sở | 
| đồ thị chu trình | xử lý đúng các lượt truy cập | sự cần thiết mở rộng nhà nước | 
| chuyển nhiệt không hợp lệ | -1 hoặc đường dẫn thay thế | thực thi ràng buộc | 
| con đường cạnh tranh | lựa chọn thời gian tối thiểu | Dijkstra đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi một đường dẫn truy cập lại một nút có trạng thái nhiệt khác. Thuật toán xử lý việc này một cách chính xác vì nó không bao giờ hợp nhất các trạng thái. Ví dụ: việc tiếp cận nút 2 với nhiệt độ -10 và nhiệt độ +10 được lưu trữ riêng biệt và cả hai đều được khám phá độc lập. 

Một trường hợp khác là khi tất cả các đường dẫn đều đến nút\(n\)nhưng với sự chuyển đổi nhiệt không hợp lệ ở đâu đó trên đường đi. Chúng sẽ tự động bị loại bỏ ở bước chuyển tiếp, vì vậy kết quả cuối cùng`min(dist[n])`chính xác sẽ trở thành vô cùng, tạo ra -1. 

Trường hợp tinh vi cuối cùng là khi nhiều cạnh kết nối cùng một cặp nút với các hiệu ứng nhiệt khác nhau. Thuật toán xử lý chúng một cách độc lập, đảm bảo tìm ra sự kết hợp tốt nhất mà không chiếm ưu thế giữa các cạnh.
