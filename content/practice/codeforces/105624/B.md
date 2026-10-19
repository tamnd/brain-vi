---
title: "CF 105624B - \u0414\u043e\u0433\u043e\u043d\u044f\u043b\u043a\u0438 \u043d\u0430 \u043e\u0441\u0442\u0440\u043e\u0432\u0430\u0445"
description: "Biểu đồ mô tả một nhóm đảo được kết nối bằng các tuyến phà hai chiều. Mỗi tuyến đường đều có chi phí đi lại và mỗi hòn đảo đều có một khoản thuế phải nộp nếu Moana chọn đến thăm nó."
date: "2026-06-26T18:12:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105624
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105624
solve_time_s: 48
verified: true
draft: false
---

[CF 105624B - \u0414\u043e\u0433\u043e\u043d\u044f\u043b\u043a\u0438 \u043d\u0430 \u043e\u0441\u0442\u0440\u043e\u0432\u0430\u0445](https://codeforces.com/problemset/problem/105624/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Biểu đồ mô tả một nhóm đảo được kết nối bằng các tuyến phà hai chiều. Mỗi tuyến đường đều có chi phí đi lại và mỗi hòn đảo đều có một khoản thuế phải nộp nếu Moana chọn đến thăm nó. Moana bắt đầu từ mọi hòn đảo có thể một cách riêng biệt và muốn đến thăm một hòn đảo nào đó, nộp thuế cho hòn đảo đó và sau đó quay trở lại nơi cô ấy bắt đầu. Nhiệm vụ là tìm tổng chi phí tối thiểu này cho mỗi hòn đảo ban đầu. Vấn đề ban đầu là từ Codeforces Gym 105624, vấn đề B. 

Đối với một hòn đảo`u`, nếu Moana chọn điểm đến`v`, chi phí chuyến đi`dist(u, v)`để đến đó và trả lại số tiền tương tự vì phà là hai chiều. Tổng số trở thành:```
2 * dist(u, v) + a[v]
```Chúng tôi cần giảm thiểu giá trị này trên tất cả các điểm đến có thể`v`. 

Kích thước đầu vào đủ lớn nên việc xử lý từng hòn đảo một cách độc lập là không thể. Với tối đa`2 * 10^5`đảo và tuyến đường, một cách tiếp cận chạy thuật toán đường đi ngắn nhất từ ​​mỗi hòn đảo sẽ cần khoảng`n * (m log n)`hoạt động, tức là khoảng hàng tỷ hoạt động trong trường hợp xấu nhất. Giải pháp dự định phải xử lý toàn bộ biểu đồ trong thời gian gần như tuyến tính. 

Phần khó khăn không phải là tự mình tìm ra những con đường ngắn nhất mà là kết hợp tất cả các điểm đến có thể có vào một phép tính. Một giải pháp bất cẩn có thể khiến Dijkstra phải chạy từ mỗi hòn đảo và tính thêm mức thuế rẻ nhất sau khi đến mọi hòn đảo khác. Đối với một biểu đồ có 200000 hòn đảo, công việc này gần như lặp lại nhiều lần. 

Một trường hợp khác là khi điểm đến rẻ nhất chính là hòn đảo xuất phát. Ví dụ:```
Input
2 1
1 2 5
3 10
```Đáp án đảo 1 là`3`, vì ở trên đảo 1 chỉ tốn tiền thuế. Một giải pháp chỉ xem xét việc di chuyển dọc theo ít nhất một cạnh sẽ xuất ra không chính xác`13`. 

Trường hợp thứ hai là khi một con đường dài với mức thuế rẻ sẽ tốt hơn một hòn đảo đắt đỏ gần đó. Ví dụ:```
Input
3 2
1 2 5
2 3 1
100 1 2
```Đối với đảo 1, đi đảo 3 chi phí`5 + 1 = 6`một chiều, vậy tổng số là`12 + 2 = 14`. Chọn đảo 2 mang lại`10 + 1 = 11`, vậy ra đảo 2 ở đây thực sự tốt hơn. Việc triển khai chỉ kiểm tra các hàng xóm trực tiếp sẽ bỏ lỡ các đích đến hợp lệ trong các biểu đồ lớn hơn. 

## Phương pháp tiếp cận 

Phương pháp vũ phu trực tiếp rất đơn giản. Đối với mỗi hòn đảo bắt đầu`u`, chạy Dijkstra để tính khoảng cách tới tất cả các đảo khác. Sau đó quét tất cả các đảo`v`và lấy giá trị nhỏ nhất của`2 * dist(u, v) + a[v]`. Điều này đúng vì Dijkstra đưa ra khoảng cách đường đi ngắn nhất chính xác cho điểm xuất phát đã chọn. Vấn đề là công việc lặp đi lặp lại. Với`n`chạy, sự phức tạp trở nên`O(nm log n)`, vượt xa những gì ràng buộc cho phép. 

Quan sát quan trọng là mọi hòn đảo bắt đầu đều hỏi cùng một loại câu hỏi. Thay vì chọn điểm xuất phát và tìm kiếm điểm đến, chúng ta có thể đảo ngược góc nhìn. Hãy tưởng tượng rằng mọi hòn đảo đều có chi phí ban đầu bằng thuế của nó. Chúng tôi truyền bá những giá trị này thông qua biểu đồ. Khi một giá trị từ đảo`v`đi qua một cạnh, nó tăng gấp đôi trọng số của cạnh vì câu trả lời cuối cùng bao gồm hành trình tới`v`và chuyến trở về. 

Điều này biến công thức thành bài toán đường đi ngắn nhất tiêu chuẩn. Nếu chúng ta tạo một biểu đồ trong đó mỗi trọng số của cạnh được nhân đôi và ban đầu đặt`a[v]`vào hàng ưu tiên cho mọi hòn đảo`v`, Dijkstra đa nguồn tính toán:```
min(a[v] + shortest doubled-edge distance from u to v)
```chính xác là:```
min(a[v] + 2 * dist(u, v))
```Cấu trúc biểu đồ giúp điều này trở nên khả thi vì tất cả các điểm đến đều là những nguồn câu trả lời tiềm năng độc lập và Dijkstra đương nhiên sẽ giữ điểm đến rẻ nhất đến từng hòn đảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm log n) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách lân cận cho các đảo. Đối với mỗi tuyến phà có chi phí`w`, lưu trữ một cạnh với chi phí`2 * w`. Việc nhân đôi được thực hiện ở đây vì mọi chuyển động hướng tới đích đều được ghép nối với cùng một chuyển động quay lại. 
2. Xếp từng hòn đảo vào hàng ưu tiên với giá trị thuế riêng`a[i]`bằng khoảng cách ban đầu. Mỗi hòn đảo đại diện cho một điểm đến có thể cung cấp câu trả lời cho mọi hòn đảo khác. 
3. Chạy thuật toán Dijkstra bình thường. Khi loại bỏ một hòn đảo khỏi hàng đợi, hãy thử thư giãn tất cả các hòn đảo lân cận bằng cách sử dụng chi phí cạnh gấp đôi. 
4. Nếu việc tiếp cận hàng xóm thông qua đảo hiện tại mang lại giá trị nhỏ hơn, hãy cập nhật giá trị đó và đẩy giá trị mới vào hàng đợi. 
5. Sau khi hàng đợi ưu tiên trống, khoảng cách được lưu trữ của mỗi hòn đảo là số tiền tối thiểu Moana phải chi khi bắt đầu ở đó. 

Tại sao nó hoạt động: Mỗi giá trị ban đầu`a[v]`đại diện cho việc chọn đảo`v`như là điểm đến. Di chuyển giá trị này qua biểu đồ sẽ thêm`2 * edge_cost`cho mỗi chuyến phà được sử dụng, phù hợp chính xác với chi phí đi tới`v`và quay trở lại. Dijkstra luôn giữ giá trị rẻ nhất có thể đạt được ở mỗi hòn đảo vì tất cả trọng số của các cạnh được chuyển đổi đều không âm. Vì mọi điểm đến có thể đều bắt đầu trong hàng đợi nên giá trị cuối cùng của mỗi hòn đảo là giá trị nhỏ nhất trên tất cả các điểm đến. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        w *= 2
        graph[u].append((v, w))
        graph[v].append((u, w))

    a = list(map(int, input().split()))

    dist = a[:]
    pq = [(a[i], i) for i in range(n)]
    heapq.heapify(pq)

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        for v, w in graph[u]:
            nxt = cur + w
            if nxt < dist[v]:
                dist[v] = nxt
                heapq.heappush(pq, (nxt, v))

    print(*dist)

if __name__ == "__main__":
    solve()
```Danh sách lân cận lưu trữ mạng lưới phà một cách hiệu quả vì số lượng tuyến đường lớn nhưng vẫn tuyến tính ở kích thước đầu vào. Mỗi cạnh được lưu trữ đã có trọng số nhân với hai, do đó, độ giãn Dijkstra khớp trực tiếp với công thức được yêu cầu. 

Mảng khoảng cách được khởi tạo bằng thuế đảo thay vì vô cùng. Đây là điểm khác biệt chính so với Dijkstra nguồn đơn thông thường. Mỗi hòn đảo đều có thể là một điểm đến, vì vậy mỗi hòn đảo đều bắt đầu như một nguồn câu trả lời ứng viên. 

Hàng đợi ưu tiên có thể chứa các giá trị lỗi thời sau khi tìm thấy tuyến đường tốt hơn. các`if cur != dist[u]`kiểm tra sẽ loại bỏ các mục đó và giữ cho thuật toán hoạt động hiệu quả. 

Số nguyên Python không bị tràn, điều này rất hữu ích vì chi phí đường dẫn tối đa có thể vượt quá giới hạn số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input
4 2
2 3 2
3 4 3
15 3 10 2
```Thuật toán bắt đầu với tất cả các loại thuế trong hàng đợi. 

| Đảo hiện tại | Giá trị hiện tại | Hàng xóm cập nhật | Trạng thái khoảng cách | 
| --- | --- | --- | --- | 
| Đảo 4 | 2 | Đảo 3 trở thành 8 | [15, 3, 8, 2] | 
| Đảo 2 | 3 | Thí sinh đảo 3 trở thành 7 | [15, 3, 7, 2] | 
| Đảo 3 | 7 | Ứng viên Đảo 4 lên 13, Đảo 2 lên 11 | [15, 3, 7, 2] | 
| Đảo 1 | 15 | Không có tuyến đường | [15, 3, 7, 2] | 

Các giá trị cuối cùng khớp với đầu ra được yêu cầu. Dấu vết này cho thấy điểm đến rẻ nhất có thể đến từ bất kỳ hòn đảo nào, không nhất thiết phải là hòn đảo gần đó. 

Đối với mẫu thứ hai:```
Input
3 3
1 2 1
1 3 1
2 3 1
10 20 30
```| Đảo hiện tại | Giá trị hiện tại | Hàng xóm cập nhật | Trạng thái khoảng cách | 
| --- | --- | --- | --- | 
| Đảo 1 | 10 | Đảo 2 thành 12, Đảo 3 thành 12 | [10, 12, 12] | 
| Đảo 2 | 12 | Không có bản cập nhật nào tốt hơn | [10, 12, 12] | 
| Đảo 3 | 12 | Không có bản cập nhật nào tốt hơn | [10, 12, 12] | 

Điều này thể hiện tính chất đa nguồn. Đảo 1 ngay lập tức cung cấp điểm đến tốt nhất cho các đảo khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi hòn đảo được xếp vào hàng ưu tiên và mỗi tuyến đường được nới lỏng một lần từ mỗi điểm cuối. | 
| Không gian | O(n + m) | Biểu đồ lưu trữ tất cả các tuyến phà và mảng lưu trữ dữ liệu khoảng cách và hàng đợi. | 

Các ràng buộc yêu cầu tránh việc tính toán đường đi ngắn nhất lặp đi lặp lại. Phương pháp Dijkstra đa nguồn xử lý toàn bộ biểu đồ một lần, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        w *= 2
        graph[u].append((v, w))
        graph[v].append((u, w))

    a = list(map(int, input().split()))

    dist = a[:]
    pq = [(dist[i], i) for i in range(n)]
    heapq.heapify(pq)

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return " ".join(map(str, dist)) + "\n"

assert solve_io(
    """4 2
2 3 2
3 4 3
15 3 10 2
"""
) == "15 3 7 2\n", "sample 1"

assert solve_io(
    """3 3
1 2 1
1 3 1
2 3 1
10 20 30
"""
) == "10 12 12\n", "sample 2"

assert solve_io(
    """2 1
1 2 5
3 10
"""
) == "3 13\n", "staying on the starting island"

assert solve_io(
    """3 2
1 2 5
2 3 1
100 1 2
"""
) == "11 1 2\n", "cheap middle destination"

assert solve_io(
    """5 4
1 2 1
2 3 1
3 4 1
4 5 1
7 7 7 7 7
"""
) == "7 9 11 9 7\n", "equal taxes and path boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai hòn đảo một cạnh |`3 13`| Thuật toán cho phép ở lại hòn đảo hiện tại. | 
| Ba hòn đảo với một chuỗi |`11 1 2`| Một điểm đến xa hơn vẫn có thể là tối ưu. | 
| Đường năm đảo thuế ngang nhau |`7 9 11 9 7`| Đường đối xứng và đảo ranh giới. | 

## Vỏ cạnh 

Đối với trường hợp ở lại là tối ưu:```
2 1
1 2 5
3 10
```Cả hai hòn đảo đều bắt đầu trong hàng đợi ưu tiên. Đảo 1 bắt đầu bằng giá trị`3`, vì vậy không có con đường nào đi qua hòn đảo khác có thể cải thiện được nó vì mỗi chuyến phà đi qua đều bổ sung thêm một lượng dương. Câu trả lời vẫn còn`3`. 

Đối với trường hợp điểm đến tốt hơn không ở gần nhau:```
3 2
1 2 5
2 3 1
100 1 2
```Hàng đợi ban đầu chứa các giá trị`100`,`1`, Và`2`. giá trị`1`từ đảo 2 lan sang đảo 1 với chi phí bổ sung`10`, sản xuất`11`. Giá trị từ đảo 3 đến đảo 1 với chi phí bổ sung`12`, sản xuất`14`. Thuật toán giữ giá trị nhỏ hơn, chứng tỏ rằng tất cả các đích đến có thể đều được xem xét thay vì chỉ các lân cận trực tiếp.
