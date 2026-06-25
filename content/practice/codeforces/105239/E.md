---
title: "CF 105239E - Mưa"
description: "Chúng ta có một lưới hình chữ nhật biểu thị địa hình ngập nước, trong đó mỗi ô có giá trị độ sâu của nước. Một khách du lịch bắt đầu ở ô trên cùng bên trái và muốn đến ô dưới cùng bên phải."
date: "2026-06-24T11:12:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105239
codeforces_index: "E"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 1"
rating: 0
weight: 105239
solve_time_s: 62
verified: true
draft: false
---

[CF 105239E - Mưa](https://codeforces.com/problemset/problem/105239/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật biểu thị địa hình ngập nước, trong đó mỗi ô có giá trị độ sâu của nước. Một khách du lịch bắt đầu ở ô trên cùng bên trái và muốn đến ô dưới cùng bên phải. Chỉ được phép di chuyển sang trái, phải và xuống, vì vậy du khách có thể tự do di chuyển theo chiều ngang trong một hàng nhưng không bao giờ có thể đi lên trên. 

Đường đi là bất kỳ chuỗi các bước đi liền kề nào theo các quy tắc này. Chi phí của một đường dẫn được xác định là độ sâu nước tối đa trong số tất cả các ô được truy cập dọc theo đường dẫn đó, bao gồm cả hai điểm cuối. Nhiệm vụ là chọn một đường dẫn hợp lệ để giảm thiểu độ sâu tối đa này. 

Kích thước lưới có thể lớn tới 500 x 500, do đó có tới 250.000 ô. Mỗi giá trị ô có thể lớn tới 10^9, loại trừ mọi cách tiếp cận phụ thuộc vào việc lặp qua tất cả các ngưỡng có thể theo cách đơn giản hoặc khám phá tất cả các đường dẫn một cách rõ ràng. Bất kỳ thuật toán nào cố gắng liệt kê các đường dẫn đều không khả thi ngay lập tức vì ngay cả một lưới vừa phải cũng có nhiều tuyến đường có thể có theo cấp số nhân do chuyển động tự do từ trái sang phải trong các hàng. 

Một khía cạnh tinh tế là chuyển động ngang tạo ra các chu kỳ trong một hàng. Mặc dù chuyển động thẳng đứng hoàn toàn hướng xuống, nhưng bạn có thể đi sang trái và sang phải tùy ý nhiều lần trên cùng một hàng trước khi đi xuống, do đó đồ thị không có tính chu kỳ. 

Một trường hợp thất bại đối với lối suy nghĩ ngây thơ là cho rằng bạn có thể tham lam chuyển đến một người hàng xóm nhỏ nhất ở địa phương. Ví dụ: việc chọn độ sâu liền kề nhỏ nhất ở mỗi bước có thể khiến bạn mắc kẹt trong một hàng khiến sau đó phải giảm theo chiều dọc lớn. 

Một trường hợp thất bại khác là giả sử chỉ di chuyển sang phải và xuống là đủ. Vì được phép di chuyển sang trái nên đôi khi bạn cần di chuyển sang trái để đạt được điểm vào theo chiều dọc tốt hơn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là liệt kê tất cả các đường dẫn hợp lệ từ đầu đến cuối và tính giá trị lớn nhất dọc theo mỗi đường dẫn, sau đó lấy giá trị tối thiểu trong số các giá trị cực đại này. Mặc dù đúng về mặt khái niệm nhưng số lượng đường đi tăng cực kỳ nhanh vì mỗi hàng hoạt động giống như một hành lang được kết nối: bạn có thể đi qua nó theo nhiều cách khác nhau trước khi thả xuống. Trong lưới 500 x 500, điều này vượt xa mọi tính toán khả thi, ngay cả trước khi xem xét rằng mỗi lần đánh giá đường dẫn có chi phí lên tới O(nm). 

Quan sát quan trọng là chúng ta không quan tâm đến đường đi chính xác mà chỉ quan tâm đến việc liệu ngưỡng T có khả thi hay không. Nếu chúng ta cố định một giá trị T và chỉ cho phép bước lên các ô có độ sâu tối đa T, thì câu hỏi sẽ trở thành liệu (1,1) có thể đạt tới (n,m) trong biểu đồ cảm ứng hay không. Điều này biến vấn đề thành một kiểm tra tính khả thi đơn điệu: nếu một đường dẫn tồn tại cho T thì nó cũng tồn tại cho bất kỳ T lớn hơn nào. 

Tính đơn điệu này cho phép có hai nghiệm tiêu chuẩn. Một là tìm kiếm nhị phân trên T kết hợp với kiểm tra khả năng tiếp cận bằng BFS hoặc DFS. Cách thứ hai là một công thức trực tiếp hơn dưới dạng bài toán đường đi ngắn nhất minimax, trong đó mỗi nút có trọng số bằng giá trị ô của nó và chi phí đường đi là trọng số tối đa dọc theo đường đi. Phiên bản đó có thể được giải quyết bằng Dijkstra bằng cách coi khoảng cách đến một nút là giá trị tối đa tối thiểu có thể gặp phải cho đến nay. 

Cả hai phối cảnh đều tương đương nhau, nhưng Dijkstra tránh yếu tố log bổ sung từ tìm kiếm nhị phân và thể hiện cấu trúc một cách trực tiếp hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi con đường | Hàm mũ | Ngăn xếp đệ quy O(nm) | Quá chậm | 
| Tìm kiếm nhị phân + BFS | O(nm log 10^9) | O(nm) | Đã chấp nhận | 
| Dijkstra Minimax | O(nm log(nm)) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng Dijkstra trên lưới trong đó trạng thái là một ô và khoảng cách biểu thị độ sâu tối đa tối thiểu có thể gặp trên bất kỳ đường dẫn nào kể từ đầu.

1. Khởi tạo ma trận khoảng cách trong đó mỗi giá trị là vô cùng, ngoại trừ ô bắt đầu được đặt ở độ sâu riêng của nó. Điều này thể hiện rằng mọi đường dẫn hợp lệ đều phải bao gồm chi phí ban đầu. 
2. Đẩy ô bắt đầu vào hàng ưu tiên được sắp xếp theo chi phí đường dẫn hiện tại. Hàng đợi luôn mở rộng trạng thái có độ sâu tối đa nhỏ nhất được biết đến cho đến nay. 
3. Trích xuất ô có chi phí hiện tại nhỏ nhất từ ​​hàng đợi. Chi phí này thể hiện cách tốt nhất để tiếp cận ô này trong khi giảm thiểu độ sâu tối đa gặp phải. 
4. Đối với mỗi hàng xóm có thể truy cập được từ ô hiện tại (trái, phải hoặc dưới), hãy tính chi phí ứng cử viên bằng mức tối đa của chi phí đường dẫn hiện tại và độ sâu của ô lân cận. Điều này mô hình hóa thực tế rằng chi phí đường đi được xác định bởi ô tồi tệ nhất được thấy cho đến nay. 
5. Nếu chi phí ứng cử viên này cải thiện khoảng cách đã ghi trước đó cho hàng xóm đó, hãy cập nhật nó và đẩy hàng xóm đó vào hàng đợi ưu tiên. 
6. Tiếp tục cho đến khi tất cả các trạng thái có thể truy cập được xử lý. Câu trả lời là giá trị khoảng cách cuối cùng ở ô dưới cùng bên phải. 

Lý do chính cho việc sử dụng hàng đợi ưu tiên là khi một ô được xuất hiện với giá trị tối đa nhỏ nhất có thể thì không có đường dẫn nào sau đó có thể cải thiện nó, vì bất kỳ tuyến đường thay thế nào cũng sẽ có chi phí ít nhất là lớn. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng hàng đợi ưu tiên chứa các cách thức tốt nhất để tiếp cận các ô biên giới, được sắp xếp theo giá trị thắt cổ chai của chúng. Khi một ô được hoàn thiện (xuất hiện ở mức tối thiểu), giá trị của nó là độ sâu tối đa nhỏ nhất có thể có trong số tất cả các đường dẫn hợp lệ đến nó. Điều này đúng vì bất kỳ đường dẫn thay thế nào đến ô đó sẽ bao gồm giá trị trung gian lớn hơn hoặc đã được khám phá với chi phí tốt hơn hoặc bằng trước đó do thứ tự của hàng đợi. Cấu trúc đơn điệu của chi phí chuyển đổi (lấy mức tối đa) đảm bảo rằng việc mở rộng một đường dẫn không bao giờ có thể làm giảm chi phí của nó, đây chính xác là điều kiện cần thiết cho tính chính xác kiểu Dijkstra. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    INF = 10**18
    dist = [[INF] * m for _ in range(n)]

    dist[0][0] = grid[0][0]
    pq = [(dist[0][0], 0, 0)]

    dirs = [(0, 1), (0, -1), (1, 0)]

    while pq:
        cost, x, y = heapq.heappop(pq)

        if cost != dist[x][y]:
            continue

        if x == n - 1 and y == m - 1:
            print(cost)
            return

        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                ncost = max(cost, grid[nx][ny])
                if ncost < dist[nx][ny]:
                    dist[nx][ny] = ncost
                    heapq.heappush(pq, (ncost, nx, ny))

solve()
```Lưới được lưu trữ trực tiếp dưới dạng số nguyên và bảng khoảng cách theo dõi giá trị nút cổ chai được biết đến nhiều nhất cho mỗi ô. Hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng con đường hứa hẹn nhất hiện tại về mặt giảm thiểu độ sâu tối đa. 

Mảng hướng mã hóa chính xác các bước di chuyển được phép: phải, trái và xuống. Chuyển động đi lên bị loại trừ, điều này rất quan trọng vì nó duy trì tính khả thi của việc tiếp cận tất cả các trạng thái mà không cần xem lại các hàng theo hướng không hợp lệ. 

Việc thoát sớm khi đến ô dưới cùng bên phải là an toàn vì Dijkstra đảm bảo rằng lần đầu tiên chúng ta bật nó lên, chúng ta đã tìm thấy giá trị tối ưu của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
0 1 9
9 1 9
9 1 0
```Chúng tôi chỉ theo dõi một số tiểu bang quan trọng. 

| Bước | Tế bào bật lên | Chi phí | Cập nhật chính | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 | (0,1)=1 | 
| 2 | (0,1) | 1 | (0,2)=9, (1,1)=1 | 
| 3 | (1,1) | 1 | (2,1)=1 | 
| 4 | (2,1) | 1 | (2,2)=1 | 

Đến đích với chi phí 1. 

Điều này chứng tỏ rằng chuyển động ngang ở hàng trên cùng và dưới cùng cho phép bỏ qua hoàn toàn các ô có chi phí cao ở hàng giữa. 

### Ví dụ 2 

đầu vào:```
5 3
0 1 1
9 9 2
1 1 1
2 9 9
1 1 0
```| Bước | Tế bào bật lên | Chi phí | Cập nhật chính | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 | (0,1)=1 | 
| 2 | (0,1) | 1 | (0,2)=1 | 
| 3 | (0,2) | 1 | (1,2)=2 | 
| 4 | (1,2) | 2 | (2,2)=2 | 
| 5 | (2,2) | 2 | (3,0)=2 có thể truy cập qua hàng | 
| 6 | ... | 2 | đạt (4,2) | 

Câu trả lời cuối cùng là 2, cho thấy việc tránh tất cả các ô trên 2 là đủ nhưng tránh tất cả các ô trên 1 sẽ ngắt kết nối lưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log(nm)) | Mỗi ô được đẩy và bật ra khỏi hàng đợi ưu tiên nhiều nhất một lần bằng một bước thư giãn và mỗi thao tác tốn thời gian logarit | 
| Không gian | O(nm) | Mảng khoảng cách và hàng đợi ưu tiên lưu trữ giá trị cho tất cả các ô lưới | 

Kích thước lưới 250.000 vừa vặn thoải mái trong các giới hạn này và hệ số logarit vẫn đủ nhỏ cho giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    INF = 10**18
    dist = [[INF] * m for _ in range(n)]

    dist[0][0] = grid[0][0]
    pq = [(dist[0][0], 0, 0)]

    dirs = [(0, 1), (0, -1), (1, 0)]

    while pq:
        cost, x, y = heapq.heappop(pq)
        if cost != dist[x][y]:
            continue

        if x == n - 1 and y == m - 1:
            return str(cost)

        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                ncost = max(cost, grid[nx][ny])
                if ncost < dist[nx][ny]:
                    dist[nx][ny] = ncost
                    heapq.heappush(pq, (ncost, nx, ny))

    return ""

# provided samples
assert run("""3 3
0 1 9
9 1 9
9 1 0
""") == "1", "sample 1"

assert run("""5 3
0 1 1
9 9 2
1 1 1
2 9 9
1 1 0
""") == "2", "sample 2"

# custom cases
assert run("""2 2
0 1
1 0
""") == "1", "simple square"

assert run("""2 2
0 5
5 0
""") == "5", "forced high cell"

assert run("""3 3
0 0 0
0 0 0
0 0 0
""") == "0", "all zero"

assert run("""3 3
0 100 0
100 100 100
0 100 0
""") == "100", "bottleneck wall"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới nhỏ 2×2 | 1 | chuyển động và lựa chọn cơ bản | 
| rào cản cao chéo | 5 | sự cần thiết của việc vượt ngục cao | 
| tất cả số không | 0 | đường đi tối ưu tầm thường | 
| kết cấu tường | 100 | xử lý các đường dẫn tắc nghẽn bắt buộc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cần chuyển động ngang để vượt qua chướng ngại vật thẳng đứng. Ví dụ: nếu một cột chi phí thấp bị chặn ở hàng giữa, cách duy nhất để tiếp tục là đi vòng sang trái hoặc phải ở hàng trước đó. Thuật toán xử lý điều này một cách tự nhiên vì Dijkstra khám phá tất cả các chuyển đổi theo chiều ngang một cách đối xứng, do đó, nó sẽ truyền giá trị nút cổ chai tốt nhất trên toàn bộ các phân đoạn hàng có thể tiếp cận trước khi thử di chuyển xuống dưới. 

Một trường hợp khác là khi đường dẫn tối ưu yêu cầu xem lại vị trí hàng từ phía đối diện. Mặc dù tồn tại các chu kỳ do chuyển động từ trái sang phải, hàng đợi ưu tiên đảm bảo rằng mọi lượt truy cập lặp lại với chi phí thấp hơn sẽ bị bỏ qua. Điều này ngăn vòng lặp vô hạn trong khi vẫn cho phép khám phá theo chiều ngang cần thiết. 

Trường hợp cuối cùng là khi câu trả lời tối ưu nằm ở vùng chỉ có thể truy cập được sau khi đi qua một ô cổng có giá trị cao. Thuật toán chỉ định chính xác giá trị cổng đó làm nút thắt cổ chai cho tất cả các đường dẫn xuôi dòng, vì mọi đường dẫn ứng cử viên phải kết hợp nó tại một số điểm và thao tác tối đa sẽ duy trì giá trị đó trên tất cả các tiện ích mở rộng.
