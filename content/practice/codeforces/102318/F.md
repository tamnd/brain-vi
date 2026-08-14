---
title: "CF 102318F - Vận tải đa phương thức"
description: "Chúng tôi có mạng lưới giao thông lên tới 400 thành phố. Mỗi thành phố đều có chi phí chuyển đổi liên quan. Một gói hàng có thể di chuyển giữa các thành phố bằng một trong bốn phương thức vận chuyển: HÀNG KHÔNG, ĐƯỜNG BIỂN, ĐƯỜNG SẮT hoặc XE TẢI. Mỗi đoạn tuyến là vô hướng và thuộc về chính xác một phương thức vận chuyển."
date: "2026-08-14T04:42:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "F"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 60
verified: true
draft: false
---

[CF 102318F - Vận tải đa phương thức](https://codeforces.com/problemset/problem/102318/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có mạng lưới giao thông lên tới 400 thành phố. Mỗi thành phố đều có chi phí chuyển đổi liên quan. Một gói hàng có thể di chuyển giữa các thành phố bằng một trong bốn phương thức vận chuyển:`AIR`,`SEA`,`RAIL`, hoặc`TRUCK`. 

Mỗi đoạn tuyến là vô hướng và thuộc về chính xác một phương thức vận chuyển. Chi phí của nó là giá di chuyển gói hàng dọc theo phân khúc đó khi sử dụng chế độ đó. Tại thành phố trung gian, gói cước có thể tiếp tục sử dụng miễn phí chế độ cũ hoặc chuyển sang chế độ khác và thanh toán cước chuyển đổi của thành phố đó. Thành phố xuất phát có thể sử dụng bất kỳ phương thức nào và có thể đến đích bằng bất kỳ phương thức nào. Nhiệm vụ là tìm tổng chi phí tối thiểu có thể từ điểm xuất phát nhất định đến điểm đến. Đầu vào chứa một số trường hợp thử nghiệm độc lập và mỗi trường hợp thử nghiệm yêu cầu một chi phí tối thiểu. 

Khó khăn cốt yếu là chi phí di chuyển từ thành phố này sang thành phố khác không chỉ do hai thành phố quyết định. Nó cũng phụ thuộc vào phương thức vận chuyển hiện đang được sử dụng. Tiếp cận thành phố`A`bằng đường hàng không và đến thành phố`A`bằng đường sắt ở các trạng thái khác nhau vì những lựa chọn trong tương lai của họ có thể có chi phí chuyển đổi khác nhau. 

Có nhiều nhất 400 thành phố, vì vậy sau khi biểu diễn rõ ràng bốn phương thức vận tải, biểu đồ có nhiều nhất`4 * 400 = 1600`tiểu bang. Có thể có tới 40000 đoạn tuyến đường, cộng với chỉ có sáu kết nối chuyển mạch có thể có cho mỗi thành phố. Điều đó đưa ra một biểu đồ thưa thớt với tối đa khoảng 42400 cạnh vô hướng. Thuật toán bậc hai trên biểu đồ mở rộng đã có khoảng 2,56 triệu so sánh trạng thái mỗi lần vượt qua, trong khi thuật toán bậc ba tất cả các cặp sẽ yêu cầu khoảng`1600^3 = 4.096 * 10^9`lặp đi lặp lại, vượt xa giới hạn bốn giây. Biểu đồ cũng được tính trọng số với chi phí dương hoàn toàn, vì vậy thuật toán của Dijkstra là phù hợp một cách tự nhiên. 

Trường hợp cạnh đầu tiên là con đường trực tiếp trong đó việc thay đổi chế độ sẽ tốn kém hơn so với việc duy trì một chế độ. Ví dụ,```
1
2
A 100
B 100
1
A B AIR 7
A B
```Câu trả lời là`7`. Việc triển khai bất cẩn giả định mỗi chuyến đi phải trả chi phí chuyển đổi ở cả hai điểm cuối có thể cộng thêm không chính xác`100`hoặc`200`. Điểm gốc không yêu cầu chuyển đổi chế độ và việc đến đích cũng không yêu cầu chuyển đổi chế độ. 

Trường hợp thứ hai là cần phải chuyển đổi tại một thành phố trung gian để có được tuyến đường rẻ nhất.```
1
3
A 5
B 2
C 5
2
A B AIR 4
B C RAIL 3
A C
```Câu trả lời là`9`, bởi vì gói hàng đi`A -> B`bằng đường hàng không, trả tiền`2`Tại`B`để chuyển sang đường sắt, sau đó đi du lịch`B -> C`bằng đường sắt. Việc triển khai chỉ giữ một khoảng cách ngắn nhất cho mỗi thành phố sẽ làm mất thông tin mà gói hàng đã đến`B`bằng đường hàng không và có thể bỏ sót chi phí chuyển đổi hoặc áp dụng sai thời điểm. 

Trường hợp thứ ba là nhiều chế độ có thể kết nối cùng một cặp thành phố. Ví dụ,```
1
2
A 10
B 10
2
A B AIR 100
A B TRUCK 3
A B
```Câu trả lời là`3`. Việc biểu diễn biểu đồ bất cẩn chỉ lưu trữ một cạnh giữa hai tên thành phố mà không bao gồm phương thức vận chuyển có thể ghi đè lên trạng thái rẻ hơn hoặc phù hợp hơn. 

Trường hợp cạnh thứ tư là đường đi rẻ nhất có thể bắt đầu ở một chế độ và kết thúc ở một chế độ khác. Ví dụ,```
1
2
A 50
B 50
2
A B AIR 10
A B TRUCK 3
A B
```Câu trả lời là`3`, bởi vì gói hàng có thể chỉ cần chọn xe tải tại điểm xuất phát. Tổng quát hơn, đích đến phải được coi là thành công ở cả bốn trạng thái chế độ, không chỉ ở chế độ được sử dụng bởi đoạn tuyến cuối cùng của một số chế độ được chọn ban đầu. 

## Phương pháp tiếp cận 

Công thức đường đi ngắn nhất theo kiểu bạo lực trực tiếp là mở rộng mỗi thành phố thành bốn trạng thái, một trạng thái cho mỗi phương thức vận chuyển, sau đó chạy thuật toán đường đi ngắn nhất dày đặc như Floyd-Warshall trên tất cả các trạng thái được mở rộng. Nhà nước`(city, mode)`có nghĩa là gói hàng hiện đang ở thành phố đó và phương thức vận chuyển hiện tại của nó là`mode`. Một đoạn tuyến đường trở thành một cạnh giữa các quốc gia có cùng phương thức, trong khi việc thay đổi từ phương thức này sang phương thức khác tại cùng một thành phố sẽ trở thành một cạnh có trọng số là chi phí chuyển đổi của thành phố đó. Vì mọi hành trình hợp pháp đều tương ứng với một đường đi trong biểu đồ mở rộng này nên thuật toán đường đi ngắn nhất tất cả các cặp là chính xác. 

Vấn đề là thời gian chạy khối. Với 400 thành phố có 1600 bang mở rộng, vì vậy Floyd-Warshall thực hiện khoảng`1600^3 = 4.096 * 10^9`lặp lại thư giãn cho một trường hợp thử nghiệm. Biểu đồ chỉ chứa khoảng 40000 đoạn tuyến đường, do đó, việc coi nó là dày đặc sẽ loại bỏ chính xác độ thưa thớt mà đầu vào mang lại cho chúng ta. 

Quan sát mở ra giải pháp nhanh hơn là chúng ta không cần đường đi ngắn nhất giữa mỗi cặp trạng thái. Chỉ có một điểm xuất phát và một điểm đến. Tất cả các trọng số của cạnh đều dương, do đó Dijkstra có thể tìm trực tiếp các đường đi ngắn nhất từ ​​trạng thái gốc. 

Chúng tôi giữ nguyên biểu đồ mở rộng vì nó nắm bắt chính xác chế độ vận chuyển. Mỗi thành phố đóng góp bốn tiểu bang. Đối với mỗi thành phố, tất cả sáu cặp phương thức riêng biệt đều nhận được lợi thế về chi phí chuyển đổi của thành phố. Đối với mỗi đoạn tuyến, chúng tôi kết nối trạng thái tương ứng ở cùng một chế độ ở cả hai điểm cuối. Cuối cùng, thay vì chạy Dijkstra bốn lần, chúng tôi giới thiệu một cách khái niệm một siêu nguồn được kết nối với các cạnh có chi phí bằng 0 cho bốn chế độ của nguồn gốc. Tương tự, chúng ta có thể khởi tạo bốn khoảng cách ở chế độ gốc về 0. Câu trả lời là khoảng cách tối thiểu giữa bốn trạng thái chế độ của đích. 

Đồ thị kết quả có nhiều nhất là 1600 đỉnh và khoảng`40000 + 6 * 400 = 42400`các cạnh vô hướng. Với một đống nhị phân, Dijkstra lấy`O((V + E) log V)`, ở đây dễ dàng đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu, Floyd-Warshall | O((4c)^3) | O((4c)^2) | Quá chậm | 
| Tối ưu, Dijkstra trên biểu đồ mở rộng | O((4c + r) log c) | O(c + r) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các thành phố và gán cho mỗi thành phố bốn ID trạng thái nguyên, một ID cho mỗi phương thức vận tải. Nhà nước cho`city + mode`trình bày chính xác thông tin cần thiết để đưa ra các quyết định trong tương lai. 
2. Đối với mỗi thành phố, hãy kết nối từng cặp phương thức vận tải khác nhau với một cạnh vô hướng có trọng số là chi phí chuyển đổi của thành phố đó. Có sáu cặp như vậy vì bốn chế độ cho`4 choose 2 = 6`các công tắc có thể có. Tiếp tục trong cùng một chế độ không cần chuyển đổi cạnh, vì các cạnh của tuyến đường đã kết nối các thành phố liên tiếp bằng chế độ đó. 
3. Đối với từng đoạn tuyến`(u, v, mode, cost)`, kết nối`(u, mode)`Và`(v, mode)`với cạnh vô hướng của chi phí đã cho. Chế độ này là một phần của tiểu bang, vì vậy tuyến đường có thể sử dụng được bằng đường hàng không không được vô tình sử dụng được bằng đường sắt hoặc xe tải. 
4. Khởi tạo khoảng cách Dijkstra của cả bốn tiểu bang thuộc thành phố gốc về 0. Điều này tương đương với việc thêm một siêu nguồn mới với chi phí cạnh bằng 0 vào bốn trạng thái đó. Không tính phí chuyển đổi khi chọn phương thức vận chuyển ban đầu vì gói hàng bắt đầu mà không có chế độ hiện có. 
5. Chạy Dijkstra từ quá trình khởi tạo đa nguồn này. Bất cứ khi nào một trạng thái bị xóa khỏi hàng ưu tiên, hãy thử nới lỏng tất cả các cạnh biểu đồ của nó. Vì mỗi trọng số cạnh đều dương nên khoảng cách cuối cùng đầu tiên cho một trạng thái là khoảng cách ngắn nhất thực sự của nó. 
6. Sau khi Dijkstra kết thúc, hãy kiểm tra bốn bang thuộc thành phố đích và đo khoảng cách tối thiểu của chúng. Gói hàng được phép đến bằng bất kỳ phương thức vận chuyển nào, vì vậy không loại trừ trạng thái nào trong bốn trạng thái. 

Tại sao nó hoạt động: mọi hành trình vận chuyển thực tế đều có thể được chuyển thành một đường đi trong biểu đồ mở rộng. Một đoạn tuyến đường giữ nguyên chế độ và được biểu thị bằng một cạnh tuyến, trong khi mọi thay đổi chế độ được biểu thị bằng chính xác một cạnh chuyển mạch mang chi phí chuyển đổi của thành phố. Điều ngược lại cũng đúng vì mọi cạnh trong biểu đồ mở rộng đều tương ứng với một hành động vận chuyển hợp pháp. Do đó, chi phí đường đi trong biểu đồ mở rộng chính xác là chi phí vận chuyển trong bài toán ban đầu. Bốn trạng thái gốc có khoảng cách bằng 0 đại diện cho tất cả các chế độ ban đầu hợp pháp và việc lấy tối thiểu bốn trạng thái đích đại diện cho tất cả các chế độ cuối cùng hợp pháp. Dijkstra sau đó trả về chi phí tối thiểu trong số tất cả các đường đi như vậy. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

MODES = {
    "AIR": 0,
    "SEA": 1,
    "RAIL": 2,
    "TRUCK": 3,
}

INF = 10**30

def solve_case():
    city_count = int(input())

    city_id = {}
    switch_cost = [0] * city_count

    for i in range(city_count):
        name, cost = input().split()
        city_id[name] = i
        switch_cost[i] = int(cost)

    state_count = city_count * 4
    graph = [[] for _ in range(state_count)]

    def state(city, mode):
        return city * 4 + mode

    # Six mode-switch edges inside every city.
    for city in range(city_count):
        base = city * 4
        cost = switch_cost[city]

        for a in range(4):
            for b in range(a + 1, 4):
                u = base + a
                v = base + b
                graph[u].append((v, cost))
                graph[v].append((u, cost))

    route_count = int(input())

    for _ in range(route_count):
        u_name, v_name, mode_name, cost = input().split()
        u = city_id[u_name]
        v = city_id[v_name]
        mode = MODES[mode_name]
        cost = int(cost)

        a = state(u, mode)
        b = state(v, mode)

        graph[a].append((b, cost))
        graph[b].append((a, cost))

    origin_name, destination_name = input().split()
    origin = city_id[origin_name]
    destination = city_id[destination_name]

    dist = [INF] * state_count
    heap = []

    # Any transport mode can be chosen at the origin for free.
    for mode in range(4):
        s = state(origin, mode)
        dist[s] = 0
        heapq.heappush(heap, (0, s))

    while heap:
        current_dist, u = heapq.heappop(heap)

        if current_dist != dist[u]:
            continue

        for v, weight in graph[u]:
            new_dist = current_dist + weight
            if new_dist < dist[v]:
                dist[v] = new_dist
                heapq.heappush(heap, (new_dist, v))

    return min(dist[state(destination, mode)] for mode in range(4))

def main():
    test_cases = int(input())
    answers = []

    for _ in range(test_cases):
        answers.append(str(solve_case()))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    main()
```các`city_id`từ điển chuyển đổi tên thành phố đầu vào thành các chỉ số số nguyên nhỏ gọn. Vì tên thành phố chỉ được sử dụng khi đọc dữ liệu đầu vào nên không có lý do gì để giữ các chuỗi trong biểu đồ thực tế. 

các`state`bản đồ chức năng`(city, mode)`ĐẾN`city * 4 + mode`. Điều này mang lại cho mỗi tiểu bang một chỉ mục duy nhất từ`0`bởi vì`4 * city_count - 1`. Hệ số cố định bằng 4 cũng làm cho việc chuyển đổi chế độ trở nên dễ dàng thực hiện. 

Sáu cạnh chuyển mạch được tạo trước các cạnh của tuyến đường. Đối với một thành phố có chi phí chuyển đổi`c`, mỗi cặp chế độ khác nhau đều có lợi thế về trọng số`c`. Đồ thị này là vô hướng vì việc thay đổi từ đường hàng không sang đường sắt cũng giống như việc chuyển từ đường sắt sang đường hàng không theo quy luật của bài toán. 

Các cạnh của tuyến chỉ kết nối các trạng thái có cùng chế độ. Nếu một tuyến đầu vào nói`A B AIR 7`, cạnh vận chuyển tương ứng duy nhất là`(A, AIR) <-> (B, AIR)`. Gói hàng chỉ có thể thay đổi chế độ tại một thành phố bằng cách đi qua một trong sáu cạnh chuyển mạch. 

Bốn trạng thái gốc được khởi tạo ở khoảng cách bằng 0. Điều này rõ ràng hơn việc thêm một đỉnh siêu nguồn thực tế và tránh được một nút biểu đồ bổ sung. Nó cũng ngăn chặn việc sạc chuyển đổi không chính xác tại điểm gốc. 

Hàng đợi ưu tiên có thể chứa nhiều mục cho cùng một trạng thái. Khi một mục cũ xuất hiện,`current_dist != dist[u]`xác định nó là cũ và bỏ qua nó. Đây là mẫu Dijkstra dựa trên heap tiêu chuẩn và tránh cần một mảng được truy cập riêng. 

Số nguyên Python không bị tràn, do đó việc tính toán khoảng cách vẫn an toàn ngay cả khi một đường dẫn chứa nhiều cạnh.`INF`chỉ cần lớn hơn mọi chi phí đường dẫn hợp lệ có thể có và`10**30`là đủ thoải mái. 

Điểm đích được xử lý đối xứng với điểm gốc. Chúng tôi lấy mức tối thiểu trên cả bốn trạng thái đích vì gói hàng có thể hoàn tất bằng bất kỳ phương thức vận chuyển nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có bốn thành phố và bảy đoạn tuyến đường. Con đường rẻ nhất từ`JACKSONVILLE`ĐẾN`TAMPA`không chỉ đơn giản là đoạn đường đơn rẻ nhất. Gói hàng có thể đi qua`MIAMI`, thay đổi chế độ trên đường đi. 

Sự tiến triển trạng thái có liên quan là: 

| Bước | Thành phố | Chế độ hiện tại | Khoảng cách | 
| --- | --- | --- | --- | 
| 0 | JACKSONVILLE | BIỂN | 0 | 
| 1 | MIAMI | BIỂN | 15 | 
| 2 | MIAMI | ĐƯỜNG SẮT | 20 | 
| 3 | JACKSONVILLE | ĐƯỜNG SẮT | 65 | 
| 4 | TAMPA | ĐƯỜNG SẮT | 75 | 

Tuyến đường cụ thể này không tối ưu vì việc chuyển đổi ở`MIAMI`là không cần thiết cho con đường tốt nhất. Chi phí tối ưu thực tế có được bằng cách đi qua`MIAMI`Và`SEA`tiếp theo là phần tiếp theo rẻ hơn thích hợp, tạo ra câu trả lời mẫu`55`. Điểm mấu chốt được thể hiện qua mô hình đồ thị là việc đến một thành phố với các phương thức khác nhau sẽ tạo ra các trạng thái thực sự khác nhau, vì vậy Dijkstra phải phân biệt chúng. 

Đầu ra mẫu là:```
55
```### Mẫu 2 

Chỉ có hai thành phố. Các tuyến đường có sẵn là một tuyến đường hàng không có giá`7`, một tuyến đường xe tải có giá`3`, và chi phí của một tuyến đường sắt`19`. 

| Bước | Tiểu bang | Khoảng cách | 
| --- | --- | --- | 
| 0 | ORLANDO, KHÔNG | 0 | 
| 0 | ORLANDO, BIỂN | 0 | 
| 0 | ORLANDO, ĐƯỜNG SẮT | 0 | 
| 0 | ORLANDO, XE TẢI | 0 | 
| 1 | TAMPA, XE TẢI | 3 | 
| 1 | TAMPA, AIR | 7 | 
| 1 | TAMPA, ĐƯỜNG SẮT | 19 | 

Khoảng cách đích tối thiểu là`3`, vậy đáp án là:```
3
```Mẫu này kiểm tra xem thuật toán có được phép chọn chế độ bắt đầu một cách tự do hay không. Nó cũng xác nhận rằng tuyến đường rẻ hơn ở một chế độ không được ẩn chỉ vì chế độ khác được liệt kê đầu tiên trong đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((c + r) log c) | có`4c`tiểu bang và`O(c + r)`các cạnh và Dijkstra dựa trên heap xử lý chúng theo thời gian logarit | 
| Không gian | O(c + r) | Danh sách lân cận mở rộng lưu trữ bốn tiểu bang cho mỗi thành phố, sáu cạnh chuyển đổi cho mỗi thành phố và hai mục nhập được chỉ dẫn cho mỗi đoạn tuyến đường | 

Với`c <= 400`, đồ thị mở rộng có tối đa 1600 đỉnh. Thậm chí tại`r = 40000`, đồ thị còn thưa thớt, chỉ có vài chục nghìn cạnh. Do đó, việc triển khai dựa trên heap vẫn thoải mái trong giới hạn 4 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

MODES = {
    "AIR": 0,
    "SEA": 1,
    "RAIL": 2,
    "TRUCK": 3,
}

INF = 10**30

def solve_case():
    city_count = int(input())
    city_id = {}
    switch_cost = [0] * city_count

    for i in range(city_count):
        name, cost = input().split()
        city_id[name] = i
        switch_cost[i] = int(cost)

    state_count = city_count * 4
    graph = [[] for _ in range(state_count)]

    def state(city, mode):
        return city * 4 + mode

    for city in range(city_count):
        base = city * 4
        cost = switch_cost[city]
        for a in range(4):
            for b in range(a + 1, 4):
                u = base + a
                v = base + b
                graph[u].append((v, cost))
                graph[v].append((u, cost))

    route_count = int(input())

    for _ in range(route_count):
        u_name, v_name, mode_name, cost = input().split()
        u = city_id[u_name]
        v = city_id[v_name]
        mode = MODES[mode_name]
        cost = int(cost)

        a = state(u, mode)
        b = state(v, mode)

        graph[a].append((b, cost))
        graph[b].append((a, cost))

    origin_name, destination_name = input().split()
    origin = city_id[origin_name]
    destination = city_id[destination_name]

    dist = [INF] * state_count
    heap = []

    for mode in range(4):
        s = state(origin, mode)
        dist[s] = 0
        heapq.heappush(heap, (0, s))

    while heap:
        current_dist, u = heapq.heappop(heap)

        if current_dist != dist[u]:
            continue

        for v, weight in graph[u]:
            nd = current_dist + weight
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(heap, (nd, v))

    return min(dist[state(destination, mode)] for mode in range(4))

def solve_all(data: str) -> str:
    global input
    old_input = input
    input = io.StringIO(data).readline

    test_cases = int(input())
    result = []

    for _ in range(test_cases):
        result.append(str(solve_case()))

    input = old_input
    return "\n".join(result)

# Provided samples.
sample = """\
2
4
ORLANDO 10
TAMPA 15
MIAMI 5
JACKSONVILLE 10
7
TAMPA JACKSONVILLE AIR 100
MIAMI TAMPA SEA 70
JACKSONVILLE MIAMI RAIL 45
ORLANDO JACKSONVILLE TRUCK 85
TAMPA ORLANDO RAIL 10
MIAMI JACKSONVILLE SEA 15
ORLANDO MIAMI TRUCK 15
JACKSONVILLE TAMPA
2
ORLANDO 15
TAMPA 10
3
ORLANDO TAMPA AIR 7
TAMPA ORLANDO TRUCK 3
ORLANDO TAMPA RAIL 19
ORLANDO TAMPA
"""
assert solve_all(sample) == "55\n3", "provided samples"

# Minimum-size graph, direct route.
case_min = """\
1
2
A 100
B 100
1
A B AIR 7
A B
"""
assert solve_all(case_min) == "7", "minimum-size case"

# All route modes between the same two cities.
case_all_modes = """\
1
2
A 5
B 5
4
A B AIR 10
A B SEA 20
A B RAIL 30
A B TRUCK 4
A B
"""
assert solve_all(case_all_modes) == "4", "all modes between one pair"

# Switching at an intermediate city is necessary for the best route.
case_switch = """\
1
3
A 5
B 2
C 5
2
A B AIR 4
B C RAIL 3
A C
"""
assert solve_all(case_switch) == "9", "intermediate mode switch"

# Boundary-style case with many route edges.
# 10 cities, all four modes on every consecutive pair and both directions.
# The cheapest route is the chain using TRUCK throughout.
def build_dense_case():
    n = 10
    lines = ["1", str(n)]

    for i in range(n):
        lines.append(f"C{i} 1000")

    routes = []
    for i in range(n - 1):
        for mode, cost in [
            ("AIR", 100),
            ("SEA", 80),
            ("RAIL", 60),
            ("TRUCK", 1),
        ]:
            routes.append(f"C{i} C{i+1} {mode} {cost}")

    lines.append(str(len(routes)))
    lines.extend(routes)
    lines.append("C0 C9")

    return "\n".join(lines) + "\n"

assert solve_all(build_dense_case()) == "9", "dense route case"

# Large-state construction with 400 cities.
# Only 399 route segments are needed, so this also checks that the
# four-state expansion scales to the maximum city count.
def build_max_city_case():
    n = 400
    lines = ["1", str(n)]

    for i in range(n):
        lines.append(f"C{i} 1")

    lines.append(str(n - 1))

    for i in range(n - 1):
        lines.append(f"C{i} C{i+1} TRUCK 2")

    lines.append("C0 C399")
    return "\n".join(lines) + "\n"

assert solve_all(build_max_city_case()) == str(399 * 2), "maximum city count"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp hai mẫu |`55`,`3`| Ví dụ chính thức và cách xây dựng đồ thị cơ bản | 
|`A -> B`bằng đường hàng không cho chi phí`7`|`7`| Mạng có kích thước tối thiểu và không có chi phí chuyển đổi tại các điểm cuối | 
| Bốn chế độ giữa`A`Và`B`|`4`| Nhiều phương thức vận chuyển giữa cùng một cặp thành phố | 
|`A -> B`bằng đường hàng không,`B -> C`bằng đường sắt |`9`| Sạc đúng công tắc chế độ trung gian | 
| Mười thành phố với tất cả bốn chế độ theo cặp liên tiếp |`9`| Dữ liệu tuyến đường dày đặc và truyền tải cùng chế độ lặp đi lặp lại | 
| Chuỗi 400 thành phố |`798`| Số lượng thành phố tối đa và mở rộng trạng thái chính xác | 

## Vỏ cạnh 

Đối với chuyến đi thẳng không thay đổi phương thức, hãy xem xét:```
1
2
A 100
B 100
1
A B AIR 7
A B
```Bốn trạng thái của`A`bắt đầu ở khoảng cách bằng không. Từ`(A, AIR)`, cạnh của tuyến đường đạt tới`(B, AIR)`với chi phí`7`. Do đó, khoảng cách đích tối thiểu là`7`. Các cạnh chuyển mạch tại`A`Và`B`không bao giờ cần thiết nên chi phí chuyển đổi cũng không bị tính phí sai. 

Để thay đổi chế độ trung gian, hãy xem xét:```
1
3
A 5
B 2
C 5
2
A B AIR 4
B C RAIL 3
A C
```Các trạng thái ban đầu của`A`tất cả đều có khoảng cách bằng không. Rìa không khí cho`(B, AIR)`khoảng cách`4`. Tại`B`, cạnh chuyển mạch từ`(B, AIR)`ĐẾN`(B, RAIL)`chi phí`2`, tạo ra khoảng cách`6`. Mép đường ray sau đó đạt đến`(C, RAIL)`ở khoảng cách`9`. Khoảng cách đích tối thiểu là`9`. Biểu diễn trạng thái là yếu tố làm cho điện tích chuyển mạch xuất hiện ở đúng điểm. 

Đối với nhiều phương thức giữa cùng một thành phố, hãy xem xét:```
1
2
A 10
B 10
2
A B AIR 100
A B TRUCK 3
A B
```Các tiểu bang`(A, AIR)`Và`(A, TRUCK)`cả hai đều bắt đầu từ số 0. Các cạnh tuyến đường tương ứng của chúng dẫn đến khoảng cách`100`Và`3`. Câu trả lời là`3`. Vì mỗi chế độ có trạng thái riêng nên việc đọc tuyến thứ hai không thể hủy tuyến đầu tiên hoặc ngược lại. 

Đối với đường dẫn thay đổi chế độ nhiều lần, việc xây dựng tương tự sẽ được áp dụng nhiều lần. Giả định`A -> B`là không khí,`B -> C`là đường sắt, và`C -> D`là xe tải. Đường đi qua đồ thị mở rộng có dạng`(A, AIR)`,`(B, AIR)`,`(B, RAIL)`,`(C, RAIL)`,`(C, TRUCK)`,`(D, TRUCK)`. Tổng chi phí chính xác bằng ba chi phí tuyến đường cộng với chi phí chuyển đổi tại`B`Và`C`. Mỗi công tắc được biểu thị bằng một cạnh biểu đồ, do đó không có sự mơ hồ về thời điểm tính phí chuyển đổi. 

Cuối cùng, điểm xuất phát và điểm đến yêu cầu xử lý đặc biệt vì chúng không có chế độ đến hoặc đi bắt buộc. Việc khởi tạo tất cả bốn trạng thái gốc về 0 sẽ tạo ra sự tự do lựa chọn bất kỳ chế độ đầu tiên nào. Lấy mức tối thiểu trên cả bốn trạng thái đích sẽ mô hình hóa quyền tự do hoàn thành ở bất kỳ chế độ nào. Việc hạn chế một trong hai bên vào một chế độ tùy ý sẽ giải quyết được một vấn đề khác và có thể tạo ra câu trả lời lớn hơn.
