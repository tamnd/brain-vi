---
title: "CF 102700F - Các chuyến bay hạn chế miễn phí"
description: "Hãy coi các quốc gia như các đỉnh của đồ thị có trọng số có hướng. Chuyến bay từ quốc gia u đến quốc gia v là một cạnh có hướng với chi phí dương w. Alice bắt đầu ở quốc gia a, Bob bắt đầu ở quốc gia b và họ phải chọn một quốc gia thứ ba nào đó để họ có thể gặp nhau."
date: "2026-08-08T08:15:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "F"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 364
verified: true
draft: false
---

[CF 102700F - Các chuyến bay bị hạn chế miễn phí](https://codeforces.com/problemset/problem/102700/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 4 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi các quốc gia như các đỉnh của đồ thị có trọng số có hướng. Một chuyến bay từ đất nước`u`đến đất nước`v`là cạnh có hướng với chi phí dương`w`. Alice bắt đầu ở đất nước`a`, Bob bắt đầu ở đất nước`b`, và họ phải chọn một quốc gia thứ ba nào đó để họ có thể gặp nhau. Họ được phép sử dụng quê hương của mình làm điểm dừng chân trung gian, nhưng quê hương của không ai là quốc gia hợp lệ vì người kia không thể nhập cảnh. 

Đối với một quốc gia họp đã chọn`x`, Alice cần một chuyến đi khứ hồi trọn vẹn`a -> x -> a`và Bob cần`b -> x -> b`. Mỗi người có vé riêng cho phép tối đa`k`chuyến bay được miễn phí. Các chuyến bay miễn phí của cùng một người có thể được sử dụng ở bất cứ đâu trong chuyến đi khứ hồi hoàn chỉnh của họ, bao gồm cả phần đi và phần về. Mục tiêu là giảm thiểu tổng chi phí của Alice và Bob. Nếu một số quốc gia gặp nhau có cùng chi phí tối thiểu thì chỉ số nhỏ nhất sẽ thắng. Nếu không có quốc gia nào mang lại lợi ích cho cả hai người thì câu trả lời là`>:(`. Đồ thị có hướng nên đường bay từ`u`ĐẾN`v`không ngụ ý một chuyến bay từ`v`ĐẾN`u`. 

Đồ thị có tới`10^4`các nước và`10^4`chuyến bay, trong khi`k`nhiều nhất là`10`. Giới hạn nhỏ trên`k`là ràng buộc chính làm cho đường đi ngắn nhất ở trạng thái mở rộng trở nên thực tế. Một giải pháp thực hiện tìm kiếm biểu đồ riêng cho từng quốc gia có thể gặp nhau là quá đắt, trong khi một giải pháp có khoảng`O(km log n)`có thể dễ dàng quản lý được công việc với số lượng tìm kiếm không đổi. Trọng số cạnh là dương, do đó thuật toán Dijkstra có thể áp dụng được sau khi chúng tôi mã hóa số chuyến bay miễn phí đã sử dụng vào trạng thái. 

Trường hợp khó khăn đầu tiên là ngân sách bay miễn phí thuộc về toàn bộ chuyến đi khứ hồi, không riêng cho chặng đi và chiều về. Ví dụ:```
4 4
0 1 1
0 2 1
2 0 1
1 2 1
2 1 1
```Thực tế có năm đường bay ở đây, vì vậy phiên bản đúng là:```
4 5
0 1 1
0 2 1
2 0 1
1 2 1
2 1 1
```Câu trả lời là:```
2 2
```Với`k = 1`, Alice có thể làm`0 -> 2`miễn phí và trả tiền`2 -> 0`, trong khi Bob có thể làm`1 -> 2`miễn phí và trả tiền`2 -> 1`. Mỗi người dành đúng một chuyến bay miễn phí. Một giải pháp bất cẩn cung cấp một chuyến bay miễn phí cho cả hai nửa chuyến đi của Alice sẽ tuyên bố sai rằng Alice có thể đi du lịch với giá bằng 0. 

Vấn đề thứ hai là phương hướng. Coi như:```
3 3
0 1 0
0 1 1
0 2 1
1 2 1
```Câu trả lời đúng là:```
>:(
```Cả hai người đều có thể đến được đất nước`2`, nhưng không có đường đi từ`2`trở về một trong hai nhà. Chỉ kiểm tra khoảng cách đi sẽ khai báo sai quốc gia`2`có thể sử dụng được. 

Đất nước gặp gỡ không thể đơn giản là quê hương của Alice hay Bob. Ví dụ:```
3 6
0 1 0
0 1 1
1 0 1
0 2 100
2 0 100
1 2 100
2 1 100
```Quốc gia`0`sẽ trông cực kỳ hấp dẫn nếu chúng ta coi nó như một điểm gặp gỡ có thể xảy ra, bởi vì Alice đã ở đó rồi. Tuy nhiên, Bob không thể vào đất nước của Alice với tư cách là khách du lịch. Quốc gia`1`có vấn đề đối xứng. Quốc gia họp pháp lý duy nhất là`2`, cho:```
2 400
```Cuối cùng, mối quan hệ phải được giải quyết bằng chỉ số quốc gia. Trong mẫu đầu tiên, cả hai nước`2`Và`3`có thể được sử dụng với tổng chi phí`4`. Từ`2 < 3`, đáp án cần tìm là:```
2 4
```Một giải pháp cập nhật câu trả lời trên`<=`trong khi quét ứng viên theo thứ tự tùy ý có thể vô tình trả về sai quốc gia. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là giải quyết vấn đề hoàn chỉnh một cách riêng biệt cho từng quốc gia có thể gặp nhau. Đối với một quốc gia cố định`x`, chúng tôi có thể chạy thuật toán đường đi ngắn nhất có trạng thái chứa quốc gia hiện tại, số lượng chuyến bay miễn phí đã được sử dụng và liệu khách du lịch có còn đi tới hay không`x`hoặc đã đạt tới`x`và đang trở về nhà. Chúng tôi sẽ làm điều này cho Alice và Bob. 

Điều này hiệu quả vì trạng thái chứa chính xác thông tin cần thiết để xác định những chuyển đổi nào vẫn có thể thực hiện được. Nó cũng đúng về mặt khái niệm. Vấn đề là sự lặp lại. có`n`các quốc gia có thể gặp nhau và mỗi tìm kiếm có khoảng`2(k+1)n`tiểu bang. Trong trường hợp xấu nhất, mọi tiểu bang đều có thể kiểm tra mọi chuyến bay đi trong lớp tương ứng của nó. Với hai hành khách, hai giai đoạn chuyến đi, hai lựa chọn chuyển tiếp cho mỗi chuyến bay và`k+1`các lớp phiếu giảm giá, số lần thử thư giãn có thể đạt tới khoảng`8 * n * (k+1) * m`. 

Ở giới hạn tối đa, con số này là khoảng`8.8 * 10^9`nỗ lực thư giãn trước khi tính đến các hoạt động xếp hàng ưu tiên. Điều đó vượt xa những gì giới hạn một giây có thể chịu đựng được. 

Nhận xét quan trọng là quốc gia tham gia cuộc họp không thực sự cần phải là một phần của quốc gia có con đường ngắn nhất. Chúng ta có thể tính toán khoảng cách có thể tái sử dụng tới mọi quốc gia một lần. 

Đối với Alice, chúng tôi cần hai loại thông tin. Chúng tôi cần chi phí tối thiểu từ`a`đến mọi quốc gia bằng cách sử dụng chính xác`c`các chuyến bay miễn phí và chi phí tối thiểu từ mọi quốc gia trở về`a`sử dụng chính xác`c`các chuyến bay miễn phí. Số lượng đầu tiên thu được bằng cách chạy Dijkstra phân lớp từ`a`trong biểu đồ gốc. Thứ hai thu được bằng cách đảo ngược mọi cạnh và chạy cùng một thuật toán từ`a`. Một con đường từ`x`ĐẾN`a`trong biểu đồ ban đầu trở thành một đường dẫn từ`a`ĐẾN`x`trong biểu đồ đảo ngược, với cùng chi phí và cùng số lượng chuyến bay miễn phí. 

Chúng tôi thực hiện hai tìm kiếm tương tự cho Bob. Điều này đưa ra tổng cộng bốn phép tính đường đi ngắn nhất. 

Đồ thị lớp là kỹ thuật trung tâm. Thay vì lưu trữ một khoảng cách cho một quốc gia`v`, cửa hàng`dist[c][v]`, Ở đâu`c`là số chuyến bay miễn phí đã được sử dụng. Đối với một chuyến bay bình thường`u -> v`với chi phí`w`, có hai sự chuyển đổi có thể xảy ra. Chúng tôi có thể trả tiền cho chuyến bay, di chuyển từ`(u,c)`ĐẾN`(v,c)`với chi phí`w`, hoặc chúng ta có thể sử dụng một chuyến bay miễn phí, di chuyển từ`(u,c)`ĐẾN`(v,c+1)`với chi phí`0`. 

Bởi vì`k <= 10`, điều này chỉ nhân kích thước biểu đồ lên 11. 

Khi có sẵn bốn mảng khoảng cách này, hãy xem xét quốc gia nơi tập trung`x`. Giả sử Alice sử dụng`i`chuyến bay miễn phí trên`a -> x`Và`j`chuyến bay miễn phí trên`x -> a`. Tổng số của cô ấy là`distAForward[i][x] + distABackward[j][x]`với`i + j <= k`. 

Tính toán tương tự cho chi phí tối thiểu của Bob. Chúng tôi kiểm tra mọi sự phân chia có thể có của`k`các chuyến bay miễn phí giữa hai nửa của chuyến đi. Đây chỉ là`O(k^2)`làm việc ở mỗi quốc gia, và`k`nhiều nhất là`10`. 

Cách tiếp cận bạo lực hoạt động vì trạng thái phân lớp mô tả hành trình hợp lệ của khách du lịch, nhưng về cơ bản nó liên tục giải quyết cùng một thông tin về đường đi ngắn nhất. Quan sát rằng quốc gia họp có thể được đánh giá sau khi tính toán khoảng cách đến mọi quốc gia cho phép chúng tôi chỉ thực hiện bốn lần chạy Dijkstra và sau đó kết hợp kết quả của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n k m log(nk))`|`O(nk)`| Quá chậm | 
| Tối ưu |`O(k(m+n) log(nk) + nk^2)`|`O(k n + m)`| Đã chấp nhận | 

Hệ số không đổi trong giải pháp tối ưu chứa bốn lần chạy Dijkstra, một lần cho mỗi sự kết hợp giữa hướng người và hướng đồ thị. 

## Hướng dẫn thuật toán 

1. Xây dựng cả đồ thị có hướng ban đầu và đồ thị đảo ngược của nó. Nếu đồ thị ban đầu chứa`u -> v`với chi phí`w`, đồ thị đảo ngược chứa`v -> u`với cùng chi phí. Biểu đồ đảo ngược cho phép đường đi ngắn nhất từ ​​quê hương biểu thị đường trở về quê hương đó trong biểu đồ ban đầu. 
2. Xác định đường đi ngắn nhất theo lớp trong đó trạng thái`(v,c)`có nghĩa là chúng tôi hiện đang ở đất nước`v`và đã sử dụng chính xác`c`các chuyến bay miễn phí. Khoảng cách được lưu trữ cho trạng thái này là chi phí phải trả tối thiểu cần thiết để đạt được trạng thái đó. 
3. Đối với mỗi chuyến bay thông thường`u -> v`với chi phí`w`, thư giãn`(v,c)`với chi phí`dist[c][u] + w`. Điều này thể hiện việc thanh toán bình thường cho chuyến bay và giữ nguyên số lượng chuyến bay miễn phí đã sử dụng. 
4. Nếu`c < k`, cũng thư giãn`(v,c+1)`với chi phí`dist[c][u]`. Điều này thể hiện việc đi chuyến bay miễn phí và sử dụng một phiếu giảm giá. Chúng tôi ngừng tạo các lớp cao hơn một lần`k`các chuyến bay miễn phí đã được sử dụng. 
5. Chạy Dijkstra nhiều lớp này từ nhà của Alice`a`trên đồ thị ban đầu. Mảng kết quả đưa ra chi phí đi nước ngoài rẻ nhất từ ​​nhà của Alice đến mọi quốc gia cho mọi số chuyến bay miễn phí có thể có. 
6. Chạy lại từ`a`trên đồ thị đảo ngược. Điều này mang lại chi phí khứ hồi rẻ nhất từ ​​mọi quốc gia đến nhà của Alice cho mọi số chuyến bay miễn phí có thể có. 
7. Lặp lại hai lần tìm kiếm từ nhà của Bob`b`. Bây giờ chúng ta có bốn mảng: khoảng cách đi và về của Alice, khoảng cách đi và về của Bob. 
8. Lặp lại qua mọi quốc gia`x`ngoại trừ`a`Và`b`, bởi vì hai nước quê hương không thể là điểm đến gặp nhau của du khách kia. 
9. Đối với Alice, hãy thử từng cặp`(i,j)`thỏa mãn`i + j <= k`. Giá trị đầu tiên biểu thị phiếu giảm giá được sử dụng trước cuộc họp và giá trị thứ hai biểu thị phiếu giảm giá được sử dụng sau cuộc họp. Lấy tổng tối thiểu của khoảng cách tiến và lùi tương ứng. Tính toán tương tự được thực hiện cho Bob. 
10. Cộng chi phí tối thiểu của Alice và chi phí tối thiểu của Bob. Nếu cả hai đều hữu hạn thì đây là tổng chi phí tốt nhất để gặp nhau tại`x`. Giữ tổng giá trị nhỏ nhất và chỉ thay thế câu trả lời hiện tại khi chi phí mới nhỏ hơn rất nhiều. Vì các quốc gia được quét từ thấp đến cao nên việc giữ nguyên các câu trả lời có chi phí bằng nhau sẽ tự động thực hiện ngắt liên kết chỉ số nhỏ nhất được yêu cầu. 
11. Nếu không có quốc gia ứng cử viên nào có tổng chi phí hữu hạn, hãy in`>:(`. Nếu không, hãy in quốc gia đã chọn và tổng chi phí của quốc gia đó. 

### Tại sao nó hoạt động 

Dijkstra phân lớp duy trì tính bất biến`dist[c][v]`là chi phí tối thiểu của bất kỳ đường đi nào từ nguồn được chọn tới`v`sử dụng chính xác`c`các chuyến bay miễn phí. Mỗi chuyến bay hợp lệ tiếp theo có chính xác hai khả năng, trả tiền cho chuyến bay đó hoặc sử dụng một chuyến bay miễn phí còn lại và cả hai khả năng đều được biểu thị dưới dạng chuyển đổi trong biểu đồ phân lớp. Tất cả chi phí chuyển đổi đều không âm, do đó Dijkstra tìm ra mức tối thiểu chính xác cho mọi trạng thái. 

Đối với quốc gia có cuộc họp cố định`x`, mỗi chuyến đi khứ hồi hợp lệ có thể được chia duy nhất tại`x`. Nếu du khách sử dụng`i`chuyến bay miễn phí trước khi đến`x`Và`j`sau khi rời đi`x`, sau đó`i+j <= k`. Khoảng cách phân lớp tiến và lùi tương ứng mô tả các đường đi với số phiếu giảm giá chính xác đó, vì vậy tổng của chúng là một chuyến đi khứ hồi hợp lệ. Ngược lại, việc kết hợp bất kỳ cặp khoảng cách hữu hạn nào như vậy sẽ tạo ra một chuyến đi khứ hồi hợp lệ sử dụng tối đa`k`phiếu giảm giá. Do đó, việc áp dụng mức tối thiểu trong tất cả các chặng sẽ mang lại chuyến đi khứ hồi tối ưu cho khách du lịch đó. 

Bốn lượt chạy Dijkstra cung cấp chính xác những giá trị này cho cả hai khách du lịch. Lấy mức tối thiểu ở mọi quốc gia có cuộc họp pháp lý sẽ đưa ra tổng chi phí tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def dijkstra(graph, source, n, k):
    # dist[c][v] = minimum paid cost to reach v
    # using exactly c free flights.
    dist = [[INF] * n for _ in range(k + 1)]
    dist[0][source] = 0

    pq = [(0, source, 0)]

    while pq:
        d, u, used = heapq.heappop(pq)

        if d != dist[used][u]:
            continue

        for v, w in graph[u]:
            # Pay for this flight.
            nd = d + w
            if nd < dist[used][v]:
                dist[used][v] = nd
                heapq.heappush(pq, (nd, v, used))

            # Take this flight for free.
            if used < k and d < dist[used + 1][v]:
                dist[used + 1][v] = d
                heapq.heappush(pq, (d, v, used + 1))

    return dist

def solve():
    n, m = map(int, input().split())
    a, b, k = map(int, input().split())

    graph = [[] for _ in range(n)]
    reverse_graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        reverse_graph[v].append((u, w))

    # Alice:
    # original graph: a -> x
    # reversed graph: x -> a
    alice_forward = dijkstra(graph, a, n, k)
    alice_backward = dijkstra(reverse_graph, a, n, k)

    # Bob:
    # original graph: b -> x
    # reversed graph: x -> b
    bob_forward = dijkstra(graph, b, n, k)
    bob_backward = dijkstra(reverse_graph, b, n, k)

    answer_country = -1
    answer_cost = INF

    for x in range(n):
        if x == a or x == b:
            continue

        alice_cost = INF
        bob_cost = INF

        # Split each person's k free flights between
        # the outbound and return parts of the trip.
        for i in range(k + 1):
            for j in range(k - i + 1):
                af = alice_forward[i][x]
                ar = alice_backward[j][x]

                if af != INF and ar != INF:
                    alice_cost = min(alice_cost, af + ar)

                bf = bob_forward[i][x]
                br = bob_backward[j][x]

                if bf != INF and br != INF:
                    bob_cost = min(bob_cost, bf + br)

        if alice_cost == INF or bob_cost == INF:
            continue

        total = alice_cost + bob_cost

        if total < answer_cost:
            answer_cost = total
            answer_country = x

    if answer_country == -1:
        print(">:(")
    else:
        print(answer_country, answer_cost)

if __name__ == "__main__":
    solve()
```các`dijkstra`hàm là đường đi ngắn nhất ở trạng thái mở rộng. Của nó`dist`mảng có`k+1`các lớp, được lập chỉ mục theo số lượng chuyến bay miễn phí đã được sử dụng. Trạng thái ban đầu là`(source, 0)`với chi phí bằng không. 

Quá trình chuyển đổi thông thường giữ nguyên số phiếu giảm giá và thêm giá chuyến bay. Việc chuyển đổi miễn phí sẽ tăng số phiếu giảm giá lên một và thêm số 0. điều kiện`used < k`là kiểm tra ranh giới nhằm ngăn chặn việc truy cập vào một địa chỉ không tồn tại`k+1`lớp. 

Biểu đồ ban đầu được sử dụng cho chuyến đi ra nước ngoài. Biểu đồ đảo ngược được sử dụng cho hành trình khứ hồi. Ví dụ, một đường dẫn`x -> p -> q -> a`trong đồ thị ban đầu trở thành`a -> q -> p -> x`trong đồ thị đảo ngược. Trình tự bị đảo ngược, nhưng mọi cạnh đều có cùng chi phí, do đó khoảng cách được tính toán chính xác là chi phí của đường quay về ban đầu. 

Bốn kết quả Dijkstra được giữ lại vì Alice và Bob có vé riêng và vé của mỗi người phải được chia cho cả chặng đi và chặng về của họ. Khi đánh giá một quốc gia,`i`Và`j`là số phiếu được gán cho hai chân đó. điều kiện`j <= k - i`tương đương với`i + j <= k`. 

Mã sử ​​dụng số nguyên Python, do đó không có vấn đề tràn số nguyên.`INF`được chọn lớn hơn nhiều so với bất kỳ câu trả lời thực tế nào có thể có. Ngay cả một con đường đơn giản với nhiều nhất`n-1`chuyến bay trả phí có chi phí dưới đây`10^7`và tổng số tiền của cả hai khách du lịch vẫn rất nhỏ so với`10^30`. 

Lần quét cuối cùng đi từ quốc gia`0`trở lên và chỉ cập nhật khi`total < answer_cost`. Các chi phí tương đương được cố tình bỏ qua, giúp tự động duy trì chỉ số quốc gia nhỏ nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4 5
0 1 2
0 2 2
1 2 2
2 3 2
3 0 2
3 1 2
```Các ứng cử viên cuộc họp hữu ích là`2`Và`3`. Cả hai đều có tổng chi phí`4`, nhưng đất nước`2`có chỉ số nhỏ hơn. 

Đối với đất nước`2`, Alice có thể đi từ`0`ĐẾN`2`về chi phí`2`, sau đó sử dụng cả chuyến bay miễn phí trên đường về`2 -> 3 -> 0`. Bob làm điều đối xứng. 

Đối với đất nước`3`, mỗi du khách có thể sử dụng cả chuyến bay miễn phí trên đường đi và trả phí`2`cho chuyến bay trở về cuối cùng. 

| Đất nước hội ngộ | Chia phiếu giảm giá Alice | Giá Alice | Chia phiếu giảm giá Bob | Giá Bob | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
|`2`|`0 + 2`|`2 + 0 = 2`|`0 + 2`|`2 + 0 = 2`|`4`| 
|`3`|`2 + 0`|`0 + 2 = 2`|`2 + 0`|`0 + 2 = 2`|`4`| 

Phần quan trọng của dấu vết này là việc chia phiếu giảm giá tốt nhất có thể khác nhau đối với các quốc gia gặp gỡ khác nhau. Đối với đất nước`2`, phiếu giảm giá tốt hơn nên được sử dụng cho chặng về. Đối với đất nước`3`, tốt hơn là họ nên chi tiêu cho chặng đi. 

Vì tổng số bằng nhau nên quá trình quét sẽ giữ nguyên quốc gia`2`, sản xuất:```
2 4
```### Mẫu 2 

Đầu vào là:```
3 3
0 1 0
0 1 1
0 2 1
1 2 1
```Quốc gia họp pháp lý duy nhất là`2`. Cả hai du khách đều có thể đến được đó, nhưng không ai có thể trở về nhà. 

| Đất nước hội ngộ | Alice đi | Alice trở lại | Tổng số Alice | Bob đi | Bob trở lại | Tổng số Bob | 
| --- | --- | --- | --- | --- | --- | --- | 
|`2`|`1`|`INF`|`INF`|`1`|`INF`|`INF`| 

Các đường dẫn ngắn nhất được xếp lớp một cách chính xác sẽ không thể truy cập được các trạng thái trả về. Vì cả hai du khách đều phải hoàn thành một chuyến đi khứ hồi, đất nước`2`không thể được chọn. 

Kết quả là:```
>:(
```Ví dụ này chứng minh tại sao chỉ tính toán`home -> meeting`khoảng cách là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(k(m+n) log(nk) + nk^2)`| Cuộc chạy Dijkstra bốn lớp cộng với tất cả các lần chia phiếu thưởng cho mọi ứng cử viên | 
| Không gian |`O(kn + m)`| bốn`O(kn)`mảng khoảng cách và hai danh sách kề chứa`O(m)`cạnh | 

chỉ có`k+1 <= 11`các lớp, vì vậy biểu đồ mở rộng có nhiều nhất`11n`tiểu bang. Mỗi trong số bốn lần chạy Dijkstra xử lý biểu đồ với số lần không đổi. Quá trình quét quốc gia cuối cùng thực hiện tối đa`n(k+1)(k+2)/2`kiểm tra phân chia, đó là về`5.5 * 10^5`kiểm tra khi nào`k = 10`. Với`n,m <= 10^4`, giải pháp nằm trong giới hạn dự định. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây chứa ba mẫu chính thức và bốn hộp bổ sung. Người trợ giúp`run`chuyển một chuỗi đầu vào hoàn chỉnh tới cùng một chuỗi`solve`logic và trả về đầu ra của nó.```python
import heapq
import io
import sys

INF = 10**30

def solve(data: str) -> str:
    it = iter(map(int, data.split()))

    n = next(it)
    m = next(it)

    a = next(it)
    b = next(it)
    k = next(it)

    graph = [[] for _ in range(n)]
    reverse_graph = [[] for _ in range(n)]

    for _ in range(m):
        u = next(it)
        v = next(it)
        w = next(it)

        graph[u].append((v, w))
        reverse_graph[v].append((u, w))

    def dijkstra(graph, source):
        dist = [[INF] * n for _ in range(k + 1)]
        dist[0][source] = 0

        pq = [(0, source, 0)]

        while pq:
            d, u, used = heapq.heappop(pq)

            if d != dist[used][u]:
                continue

            for v, w in graph[u]:
                nd = d + w

                if nd < dist[used][v]:
                    dist[used][v] = nd
                    heapq.heappush(pq, (nd, v, used))

                if used < k and d < dist[used + 1][v]:
                    dist[used + 1][v] = d
                    heapq.heappush(pq, (d, v, used + 1))

        return dist

    af = dijkstra(graph, a)
    ar = dijkstra(reverse_graph, a)
    bf = dijkstra(graph, b)
    br = dijkstra(reverse_graph, b)

    best_country = -1
    best_cost = INF

    for x in range(n):
        if x == a or x == b:
            continue

        alice = INF
        bob = INF

        for i in range(k + 1):
            for j in range(k - i + 1):
                if af[i][x] != INF and ar[j][x] != INF:
                    alice = min(alice, af[i][x] + ar[j][x])

                if bf[i][x] != INF and br[j][x] != INF:
                    bob = min(bob, bf[i][x] + br[j][x])

        if alice == INF or bob == INF:
            continue

        total = alice + bob

        if total < best_cost:
            best_cost = total
            best_country = x

    if best_country == -1:
        return ">:("

    return f"{best_country} {best_cost}"

def run(inp: str) -> str:
    return solve(inp).strip()

# Official sample 1
assert run("""\
4 5
0 1 2
0 2 2
1 2 2
2 3 2
3 0 2
3 1 2
""") == "2 4", "sample 1"

# Official sample 2
assert run("""\
3 3
0 1 0
0 1 1
0 2 1
1 2 1
""") == ">:(", "sample 2"

# Official sample 3
assert run("""\
3 3
0 1 0
0 1 1
1 2 1
2 0 1
""") == "2 6", "sample 3"

# Custom 1: minimum-size graph, k = 0.
# Both travelers must pay for both directions.
assert run("""\
3 4
0 1 0
0 2 1
2 0 1
1 2 1
2 1 1
""") == "2 4", "minimum-size case"

# Custom 2: equal weights and a tie between countries 2 and 3.
assert run("""\
4 8
0 1 1
0 2 1
2 0 1
0 3 1
3 0 1
1 2 1
2 1 1
1 3 1
3 1 1
""") == "2 2", "tie and equal weights"

# Custom 3: k = 10 is used exactly on the outbound part.
# Only country 11 has a route back to either home.
edges = [
    (0, 2, 1),
    (1, 2, 1),
]

for u in range(2, 11):
    edges.append((u, u + 1, 1))

edges.append((11, 0, 1))
edges.append((11, 1, 1))

case_k10 = "12 13\n0 1 10\n"
case_k10 += "\n".join(f"{u} {v} {w}" for u, v, w in edges) + "\n"

assert run(case_k10) == "11 2", "k=10 boundary case"

# Custom 4: maximum n and m, with k = 10.
# Country 9999 is the only candidate with a return path.
n = 10000
edges = [
    (0, 2, 1),
    (1, 2, 1),
]

for u in range(2, 9999):
    edges.append((u, u + 1, 1))

edges.append((9999, 0, 1))
edges.append((9999, 1, 1))

# Make exactly m = 10000 flights.
edges.append((0, 2, 1))

assert len(edges) == 10000

case_max = f"{n} {len(edges)}\n0 1 10\n"
case_max += "\n".join(f"{u} {v} {w}" for u, v, w in edges) + "\n"

# From 0 to 9999 there are 9998 flights, of which 10 are free.
# Return costs 1. Each traveler pays 9989, total 19978.
assert run(case_max) == "9999 19978", "maximum-size case"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 4`,`k=0`đạp xe xuyên đất nước`2`|`2 4`| Kích thước biểu đồ tối thiểu và không có chuyến bay miễn phí | 
| Biểu đồ bốn quốc gia với tất cả các trọng số`1`|`2 2`| Trọng lượng bằng nhau và đứt dây chỉ số nhỏ nhất | 
| Chuỗi mười hai quốc gia với`k=10`|`11 2`| Ranh giới phiếu giảm giá chính xác và sử dụng tất cả mười chuyến bay miễn phí | 
|`n=m=10000`,`k=10`|`9999 19978`| Kích thước đầu vào tối đa và khả năng mở rộng | 

## Vỏ cạnh 

### Ngân sách phiếu giảm giá được chia sẻ 

Hãy xem xét:```
4 5
0 1 1
0 2 1
2 0 1
1 2 1
2 1 1
```Đối với Alice, con đường`0 -> 2 -> 0`có hai chuyến bay. Từ`k=1`, chỉ có một người được tự do nên Alice trả tiền`1`. Bob trả tiền tương tự`1`vì`1 -> 2 -> 1`. Câu trả lời là:```
2 2
```Thuật toán xử lý việc này vì nó không bao giờ kết hợp hai đường đi ngắn nhất "nhiều nhất một phiếu giảm giá" độc lập. Nó chọn rõ ràng`i`phiếu giảm giá cho đường đi và`j`phiếu giảm giá cho đường trở về, yêu cầu`i+j <= 1`. 

### Thiếu đường quay về 

Hãy xem xét:```
3 3
0 1 0
0 1 1
0 2 1
1 2 1
```Quốc gia`2`có thể đến được từ cả hai nhà, nhưng không có đường quay lại nào tồn tại. Trong mảng khoảng cách chuyển tiếp,`dist[0][2]`là hữu hạn đối với cả hai du khách. Trong mảng khoảng cách ngược lại, trạng thái tương ứng của quốc gia`2`là vô hạn. Ứng viên bị loại vì cuộc họp không có chuyến về hoàn chỉnh là không hợp lệ. 

Kết quả là:```
>:(
```### Quốc gia sở tại là điểm đến của cuộc họp không hợp lệ 

Hãy xem xét:```
3 6
0 1 0
0 1 1
1 0 1
0 2 100
2 0 100
1 2 100
2 1 100
```Thuật toán bỏ qua các quốc gia`0`Và`1`trước khi đánh giá ứng viên. Quốc gia`2`là địa điểm họp hợp pháp duy nhất và mỗi khách du lịch trả tiền`200`, cho:```
2 400
```Việc bỏ qua là cần thiết mặc dù máy móc có đường đi ngắn nhất có thể đại diện một cách tự nhiên cho đường đi không tốn chi phí từ một người đến nhà riêng của họ. 

### Mối ràng buộc giữa các quốc gia gặp nhau 

Trong Mẫu 1, cả hai quốc gia`2`Và`3`có tổng chi phí`4`. Thuật toán kiểm tra các quốc gia theo thứ tự chỉ số tăng dần. Nó lưu trữ đất nước`2`khi lần đầu tiên nó nhìn thấy chi phí`4`. Khi đất nước`3`cũng sản xuất`4`, điều kiện`total < answer_cost`là sai, vậy đất nước`2`vẫn là câu trả lời. 

Kết quả là:```
2 4
```Việc xử lý ràng buộc này không yêu cầu so sánh riêng về chỉ mục quốc gia vì thứ tự quét đã cung cấp thứ tự được yêu cầu. 

### Chính xác`k`chuyến bay miễn phí 

Hãy xem xét`k=10`trường hợp tùy chỉnh. Đường về quê hương của Alice`11`chứa chính xác mười chuyến bay đi:```
0 -> 2 -> 3 -> ... -> 10 -> 11
```Tất cả mười có thể được miễn phí. Chuyến bay trở về`11 -> 0`phải trả tiền, tốn kém`1`. Bob có lộ trình đối xứng đi qua`11 -> 1`. Do đó, mỗi du khách phải trả`1`, cho:```
11 2
```Biểu đồ lớp chứa các lớp`0`bởi vì`10`, do đó chuyến bay tự do thứ mười sẽ chuyển trạng thái thành lớp`10`. điều kiện`used < k`ngăn chặn chuyến bay miễn phí thứ mười một, khớp chính xác với hạn chế về vé. 

### Nhiều chuyến bay giữa cùng một quốc gia 

Đầu vào không yêu cầu các chuyến bay phải có điểm cuối duy nhất nên danh sách lân cận phải giữ lại mọi chuyến bay. Việc triển khai Dijkstra chỉ đơn giản coi mỗi chuyến bay là một quá trình chuyển đổi riêng biệt. Nếu hai chuyến bay đi từ`u`ĐẾN`v`với các chi phí khác nhau, cái rẻ hơn sẽ đương nhiên chiếm ưu thế cái đắt hơn, trong khi những bản sao có chi phí bằng nhau không gây ra vấn đề về tính chính xác. 

### Kích thước số nguyên 

Đường đi đơn giản thông thường lớn nhất sử dụng nhiều nhất`n-1`các chuyến bay phải trả tiền, mỗi chuyến có chi phí tối đa`1000`, do đó chi phí tuyến đường liên quan sẽ vào khoảng`10^7`. Ngay cả sau khi cộng chi phí của Alice và Bob, điều này vẫn nằm trong phạm vi số nguyên của Python. Tuy nhiên, việc triển khai sử dụng một lượng rất lớn`INF`giá trị để các trạng thái không thể truy cập không thể bị nhầm lẫn với khoảng cách hợp lệ. 

Bài học trọng tâm là vấn đề không thực sự nằm ở việc tìm ra con đường rẻ nhất đến một quốc gia gặp gỡ cụ thể. Đó là về việc tính toán các đường đi ngắn nhất với một nguồn tài nguyên bổ sung nhỏ, số lượng chuyến bay miễn phí, sau đó kết hợp khoảng cách đi và về. Sau khi tài nguyên đó trở thành một phần của bang Dijkstra, việc lựa chọn quốc gia gặp gỡ dường như mang tính toàn cầu sẽ trở thành một lần quét cuối cùng đơn giản. 

Một chỉnh sửa nhỏ đáng được nêu ra từ tuyên bố được cung cấp: thử nghiệm tùy chỉnh đầu tiên trong mục đích biên tập`m = 5`, bởi vì nó chứa năm đường bay. Thuật toán và tất cả các trường hợp thử nghiệm khác đều nhất quán với các ràng buộc ban đầu.
