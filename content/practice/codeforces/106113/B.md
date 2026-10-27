---
title: "CF 106113B - La campa\u00f1a de Bob"
description: "Bob muốn truyền bá thông điệp qua mạng lưới các thành phố. Các thành phố là các đỉnh của đồ thị vô hướng và một cạnh có nghĩa là thông tin có thể di chuyển giữa hai thành phố lân cận trong một tuần. Bob không cần phải đến thăm mọi thành phố. Anh ta chỉ có thể có hai đơn đặt hàng thăm viếng."
date: "2026-06-25T11:37:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106113
codeforces_index: "B"
codeforces_contest_name: "Coding Cup TecNM 2025"
rating: 0
weight: 106113
solve_time_s: 39
verified: true
draft: false
---

[CF 106113B - La campa\u00f1a de Bob](https://codeforces.com/problemset/problem/106113/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bob muốn truyền bá thông điệp qua mạng lưới các thành phố. Các thành phố là các đỉnh của đồ thị vô hướng và một cạnh có nghĩa là thông tin có thể di chuyển giữa hai thành phố lân cận trong một tuần. 

Bob không cần phải đến thăm mọi thành phố. Anh ta chỉ có thể có hai đơn đặt hàng thăm viếng. Theo thứ tự đầu tiên, anh ta đến thăm các thành phố có chỉ số lẻ từ chỉ số nhỏ hơn đến chỉ số lớn hơn. Ở thứ tự thứ hai, anh ta đến thăm các thành phố có chỉ số chẵn từ chỉ số nhỏ hơn đến chỉ số lớn hơn. Một thành phố đã ghé thăm trong tuần`k`trở thành điểm khởi đầu bổ sung cho việc truyền bá thông tin trong tuần đó. Mỗi tuần tiếp theo, những cử tri có hiểu biết sẽ chuyển thông điệp này đến hàng xóm của họ. 

Nhiệm vụ là chọn ra chiến lược tốt hơn trong hai chiến lược và đưa ra số tuần cần thiết cho đến khi mọi thành phố đều biết được thông điệp. Nếu cuối cùng cả hai chiến lược đều không thể đến được mọi thành phố thì câu trả lời là`-1`. 

Nhận xét quan trọng là một thành phố được ghé thăm sau vẫn có thể hữu ích. Ví dụ: truy cập một thành phần bị ngắt kết nối sau một vài tuần sẽ cho phép thông báo bắt đầu ở đó, mặc dù các lần truy cập trước đó không thể truy cập được thành phần đó. 

Biểu đồ có thể chứa tới`100000`các thành phố, vì vậy cách tiếp cận mô phỏng hàng tuần và truyền bá thông điệp nhiều lần là quá tốn kém. Với kích thước này, chúng ta cần tuyến tính gần đúng hoặc`O((n+m) log n)`công việc. Chạy duyệt đồ thị một hoặc hai lần là có thể chấp nhận được, nhưng thực hiện duyệt đồ thị riêng biệt sau mỗi lần truy cập thì không. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ quá trình triển khai. Nếu chỉ có một thành phố thì câu trả lời là`1`vì Bob phải đến thăm nó một lần.```
Input:
1 0
```Đầu ra đúng là:```
1
```Một giải pháp chỉ khởi động BFS từ các cạnh hiện có hoặc giả sử ít nhất một hàng xóm sẽ không thành công. 

Một trường hợp khác là khi một chiến lược không bao giờ truy cập vào thành phần bị ngắt kết nối. Ví dụ:```
Input:
4 1
1 2
```Chỉ sử dụng các chuyến thăm thành phố lẻ`1`Và`3`, vậy các thành phố`3`Và`4`chỉ có thể được bảo hiểm nếu họ liên lạc được từ những chuyến thăm đó. Thành phố`4`bị cô lập và không bao giờ nhận được tin nhắn. Vấn đề tương tự có thể xảy ra đối với chiến lược chẵn. Câu trả lời là`-1`nếu không có chiến lược nào bao gồm tất cả các thành phố. 

Một trường hợp tế nhị cuối cùng là thời gian thăm quan rất quan trọng. Một thành phố đã ghé thăm trong tuần`3`không thể truyền bá thông tin trước tuần`3`. Đối xử với tất cả các thành phố đã ghé thăm như thể chúng có sẵn trong tuần`1`cho kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ mô phỏng chiến dịch theo từng tuần. Đối với một chiến lược đã chọn, chúng tôi biết thành phố nào Bob ghé thăm mỗi tuần. Sau mỗi lần truy cập, chúng tôi có thể chạy BFS từ tất cả các thành phố hiện có thông tin và kiểm tra xem toàn bộ biểu đồ có được bao phủ hay không. Điều này đúng vì nó tuân theo quy trình chính xác được mô tả trong tuyên bố. 

Vấn đề là các cạnh giống nhau sẽ được khám phá nhiều lần. Trong trường hợp xấu nhất, Bob ghé thăm khoảng`n/2`thành phố và chi phí BFS`O(n+m)`. Tổng công việc trở nên đại khái`O(n(n+m))`, vượt xa những gì có thể`n = 100000`. 

Quan sát quan trọng là toàn bộ quá trình có thể được xem là đường đi ngắn nhất với điểm bắt đầu bị trễ. 

Nếu Bob đến thăm thành phố`u`trong tuần`t`, thì thành phố đó có thể thông báo cho thành phố khác`v`TRONG:```
t + shortest_distance(u, v)
```tuần. 

Đối với chiến lược cố định, mỗi thành phố được ghé thăm là một nguồn có thời gian bắt đầu khác nhau. Chúng tôi cần thời gian đến tối thiểu trong số tất cả các nguồn. Đây chính xác là bài toán đường đi ngắn nhất trong đó tất cả các cạnh đều có trọng số`1`, nhưng mỗi nguồn bắt đầu với khoảng cách ban đầu của riêng nó. Chúng ta có thể giải quyết nó bằng cách truyền tải kiểu BFS đa nguồn bằng cách sử dụng hàng đợi ưu tiên. 

Đối với các thành phố lẻ, thành phố`i`được truy cập trong tuần`(i+1)/2`. Đối với các thành phố chẵn, thành phố`i`được truy cập trong tuần`i/2`. Chúng tôi khởi tạo hàng đợi ưu tiên với tất cả các thành phố từ chiến lược đã chọn và các tuần ghé thăm của chúng, sau đó phổ biến thời gian đến sớm nhất có thể. 

Thuật toán tương tự xử lý cả hai chiến lược. Chúng ta chỉ cần chạy nó hai lần và lấy câu trả lời hợp lệ nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O((n+m) log n) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị thành phố. Mỗi cạnh được lưu trữ theo cả hai hướng vì thông tin có thể truyền theo một trong hai hướng. 
2. Giải bài toán một lần cho chiến lược thành phố lẻ. Đặt mọi thành phố được đánh số lẻ vào hàng ưu tiên với tuần ghé thăm là thời gian đến đầu tiên. 
3. Chạy đường đi ngắn nhất. Bất cứ khi nào một thành phố có thời gian đến`t`bị xóa khỏi hàng đợi, hãy cố gắng cải thiện từng hàng xóm theo thời gian`t+1`. Nếu điều này mang lại thời gian tốt hơn, hãy cập nhật hàng xóm và đẩy nó vào hàng đợi. 
4. Sau khi quá trình truyền tải kết thúc, hãy tìm thời gian đến lớn nhất trong số tất cả các thành phố. Nếu thành phố nào đó vẫn có thời gian đến vô hạn thì chiến lược này không thể cung cấp thông tin cho toàn bộ biểu đồ. 
5. Lặp lại quy trình tương tự cho chiến lược thành phố chẵn. Sự khác biệt duy nhất là tuần đầu tiên được chỉ định cho mỗi thành phố nguồn. 
6. Trả về kết quả hợp lệ nhỏ hơn từ hai chiến lược. Nếu cả hai chiến lược đều thất bại, hãy quay lại`-1`. 

Tại sao nó hoạt động: 

Đối với một chiến lược cố định, mỗi thành phố được ghé thăm đều có thể là điểm khởi đầu cho thông điệp. Thuật toán xếp hàng ưu tiên luôn giữ tuần nhỏ nhất đã biết khi mỗi thành phố có thể nhận được thông tin. Vì mỗi cạnh cộng thêm đúng một tuần nên bước thư giãn sẽ xem xét mọi đường đi có thể từ mọi nguồn. Giá trị cuối cùng cho mỗi thành phố là giá trị tối thiểu trên tất cả các thành phố có thể ghé thăm của:```
visit_week + graph_distance
```Đây chính xác là tuần sớm nhất thành phố có thể nhận được tin nhắn. Việc lấy giá trị tối đa của các giá trị này sẽ cho biết tuần mà thành phố cuối cùng được thông báo. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve_strategy(n, graph, start_parity):
    inf = 10**18
    dist = [inf] * (n + 1)
    pq = []

    for city in range(1, n + 1):
        if city % 2 == start_parity:
            week = (city + 1) // 2 if city % 2 == 1 else city // 2
            dist[city] = week
            heapq.heappush(pq, (week, city))

    while pq:
        time, u = heapq.heappop(pq)

        if time != dist[u]:
            continue

        for v in graph[u]:
            if dist[v] > time + 1:
                dist[v] = time + 1
                heapq.heappush(pq, (dist[v], v))

    ans = max(dist[1:])
    if ans == inf:
        return -1
    return ans

def main():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    odd = solve_strategy(n, graph, 1)
    even = solve_strategy(n, graph, 0)

    if odd == -1:
        print(even)
    elif even == -1:
        print(odd)
    else:
        print(min(odd, even))

if __name__ == "__main__":
    main()
```chức năng`solve_strategy`chứa toàn bộ thuật toán đồ thị. Của nó`start_parity`đối số quyết định thành phố nào Bob ghé thăm. Mảng khoảng cách ban đầu không phải là khởi tạo BFS thông thường, vì mỗi nguồn có thời gian bắt đầu khác nhau. Một thành phố đã ghé thăm trong tuần`k`bắt đầu bằng khoảng cách`k`, không`0`. 

Hàng đợi ưu tiên được sử dụng thay cho hàng đợi thông thường vì các giá trị ban đầu khác nhau. BFS bình thường hoạt động khi tất cả các nguồn bắt đầu đồng thời, nhưng ở đây có một thành phố được ghé thăm trong tuần`1`phải luôn được xử lý trước một lần truy cập trong tuần`20`nếu cả hai đang chờ mở rộng. 

Kiểm tra mục nhập cũ:```
if time != dist[u]:
    continue
```là cần thiết vì một thành phố có thể được chèn vào hàng đợi nhiều lần sau khi tìm được đường đi tốt hơn. Chỉ giá trị ngắn nhất mới nhất sẽ mở rộng. 

Thời gian đến tối đa cuối cùng là câu trả lời cho một chiến lược. Nếu nó vẫn là vô hạn, thì bất kỳ thành phố nào đã ghé thăm trong chiến lược đó đều không thể tiếp cận được một số thành phố. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 6
1 4
1 2
2 3
2 5
2 4
3 6
```Để thực hiện chiến lược kỳ quặc, Bob ghé thăm`1`,`3`,`5`. 

| Thành phố | Tuần đầu tiên | Tuần đến cuối cùng | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | - | 2 | 
| 3 | 2 | 2 | 
| 4 | - | 3 | 
| 5 | 3 | 3 | 
| 6 | - | 3 | 

Thành phố cuối cùng nhận được tin nhắn trong tuần`3`. 

Đối với chiến lược đồng đều, thành phố cuối cùng cần nhiều thời gian hơn, vì vậy câu trả lời là`3`. 

Ví dụ này cho thấy tại sao các thứ tự lượt truy cập khác nhau lại quan trọng. Chiến lược đầu tiên đạt đến phần trung tâm của biểu đồ sớm hơn. 

### Mẫu 2 

đầu vào:```
6 6
1 4
1 2
2 3
2 4
2 5
2 6
```Để thực hiện chiến lược kỳ quặc, Bob ghé thăm`1`,`3`,`5`. 

| Thành phố | Tuần đầu tiên | Tuần đến cuối cùng | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | - | 2 | 
| 3 | 2 | 2 | 
| 4 | - | 3 | 
| 5 | 3 | 3 | 
| 6 | - | 3 | 

Chiến dịch kết thúc trong tuần`3`. 

Để có chiến lược đồng đều, Bob ghé thăm`2`,`4`,`6`. 

| Thành phố | Tuần đầu tiên | Tuần đến cuối cùng | 
| --- | --- | --- | 
| 1 | - | 2 | 
| 2 | 1 | 1 | 
| 3 | - | 2 | 
| 4 | 2 | 2 | 
| 5 | - | 2 | 
| 6 | 3 | 3 | 

Chiến lược thứ hai cũng kết thúc trong tuần`3`, vậy đáp án vẫn là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Mỗi thành phố và rìa được xử lý thông qua việc duyệt hàng đợi ưu tiên cho từng chiến lược trong số hai chiến lược. | 
| Không gian | O(n+m) | Danh sách kề lưu trữ biểu đồ và mảng lưu trữ thời gian đến. | 

Các ràng buộc cho phép sự phức tạp này vì đồ thị chỉ được duyệt hai lần. Thuật toán tránh lặp lại BFS sau mỗi lần Bob truy cập. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve_strategy(n, graph, parity):
        inf = 10**18
        dist = [inf] * (n + 1)
        pq = []

        for i in range(1, n + 1):
            if i % 2 == parity:
                t = (i + 1) // 2 if i % 2 else i // 2
                dist[i] = t
                heapq.heappush(pq, (t, i))

        while pq:
            t, u = heapq.heappop(pq)
            if t != dist[u]:
                continue
            for v in graph[u]:
                if dist[v] > t + 1:
                    dist[v] = t + 1
                    heapq.heappush(pq, (t + 1, v))

        return -1 if max(dist[1:]) == inf else max(dist[1:])

    n, m = map(int, input().split())
    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    a = solve_strategy(n, graph, 1)
    b = solve_strategy(n, graph, 0)

    if a == -1:
        ans = b
    elif b == -1:
        ans = a
    else:
        ans = min(a, b)

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert run("""6 6
1 4
1 2
2 3
2 5
2 4
3 6
""") == "3\n", "sample 1"

assert run("""6 6
1 4
1 2
2 3
2 4
2 5
2 6
""") == "3\n", "sample 2"

assert run("""1 0
""") == "1\n", "single city"

assert run("""4 1
1 2
""") == "-1\n", "disconnected graph"

assert run("""5 4
1 2
2 3
3 4
4 5
""") == "3\n", "path graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| Xử lý biểu đồ kích thước tối thiểu | 
|`4 1`có cạnh`1 2`|`-1`| Thông tin không thể lan truyền | 
| Con đường năm thành phố |`3`| Tuyên truyền qua nhiều cạnh | 
| Cung cấp mẫu |`3`| So sánh chiến lược | 

## Vỏ cạnh 

Đối với một thành phố:```
Input:
1 0
```Chiến lược kỳ quặc thăm thành phố`1`trong tuần`1`. Mảng đến trở thành`[1]`, vậy thời gian đến lớn nhất là`1`. Thuật toán không bao giờ phụ thuộc vào việc có các cạnh nên nó xử lý trực tiếp trường hợp này. 

Đối với đồ thị bị ngắt kết nối:```
Input:
4 1
1 2
```Chiến lược kỳ quặc bắt đầu từ các thành phố`1`Và`3`. Thành phố`4`bị cô lập và không phải là thành phố khởi đầu nên khoảng cách của nó là vô hạn. Chiến lược đồng đều bắt đầu từ thành phố`2`và chỉ đạt đến thành phần đầu tiên. Nó cũng rời thành phố`4`không thể truy cập được. Vì cả hai chiến lược đều có thời gian đến vô hạn nên câu trả lời là`-1`. 

Đối với các chuyến thăm bị trì hoãn:```
Input:
5 4
1 2
2 3
3 4
4 5
```Chiến lược đồng đều đến thăm thành phố`2`trong tuần`1`và thành phố`4`trong tuần`2`. Thành phố`5`có thể đến từ thành phố`4`trong tuần`3`, sớm hơn là chờ đợi một chiến lược kỳ quặc để ghé thăm thành phố`5`trong tuần`3`. Hàng đợi ưu tiên tách biệt các thời điểm bắt đầu khác nhau này, do đó, nó không cho rằng mọi nguồn đều tồn tại trong tuần một cách không chính xác.`1`.
