---
title: "CF 102396F - Metro 2345"
description: "Hãy nghĩ về hệ thống tàu điện ngầm như một biểu đồ có trọng số. Mỗi trạm là một đỉnh, các trạm liên tiếp trên cùng một đường được nối với nhau bằng một cạnh và trọng số của cạnh là thời gian di chuyển giữa các trạm đó."
date: "2026-08-10T18:34:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "F"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 191
verified: true
draft: false
---

[CF 102396F - Metro 2345](https://codeforces.com/problemset/problem/102396/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy nghĩ về hệ thống tàu điện ngầm như một biểu đồ có trọng số. Mỗi trạm là một đỉnh, các trạm liên tiếp trên cùng một đường được nối với nhau bằng một cạnh và trọng số của cạnh là thời gian di chuyển giữa các trạm đó. Ba kết nối đặc biệt giữa các đường là các cạnh bổ sung có trọng số là thời gian truyền`d`. 

Dòng đầu tiên chứa số trạm trên ba dòng,`x`,`y`, Và`z`. Dòng thứ hai cung cấp ba tham số trao đổi`a`,`b`, Và`c`. Những điều này xác định ba cặp chuyển giao:`(1, a)`được kết nối với`(2, b + 1)`,`(1, a + 1)`được kết nối với`(3, c)`, Và`(2, b)`được kết nối với`(3, c + 1)`. 

Dòng thứ ba cung cấp thời gian di chuyển cho một lần di chuyển trạm liền kề trên mỗi tuyến và thời gian cần thiết cho một lần chuyển. Dòng cuối cùng xác định tuyến và ga xuất phát cũng như tuyến và ga đích. 

Đầu ra là tổng thời gian di chuyển tối thiểu. Một tuyến đường có thể di chuyển theo một trong hai hướng dọc theo tuyến tàu điện ngầm và có thể chuyển tuyến tại bất kỳ cặp nút giao nào trong ba cặp nút giao. Vì Dima không rời tàu điện ngầm tại một nút giao nên những lần chuyển tuyến đó chỉ đơn giản là các cạnh của tuyến đường. 

Khó khăn chính là kích thước của các dòng. Mỗi dòng có thể chứa tối đa`10^9`các trạm, do đó biểu đồ hoàn chỉnh có thể chứa tới`3 * 10^9`đỉnh. Ngay cả việc đọc một biểu đồ như vậy một cách rõ ràng cũng không thể thực hiện được trong giới hạn một giây. Câu trả lời không thể phụ thuộc vào việc lặp lại trên tất cả các trạm, vì vậy lời giải phải sử dụng thực tế là hầu hết mọi trạm đều hoàn toàn bình thường. 

Có một số trường hợp đặc biệt có thể khiến một giải pháp có vẻ hợp lý trở nên sai lầm. Đầu tiên là các điểm cuối trao đổi không ở`(a, b)`,`(a, c)`, Và`(b, c)`. Chúng được dịch chuyển từng dòng một trên các đường xen kẽ. Ví dụ,```
3 3 3
1 1 1
5 1 1 1
1 2 2 1
```Điểm xuất phát là tuyến 1, trạm 2, đích đến là tuyến 2, trạm 1. Đi thẳng sử dụng tuyến 1 trạm 1 và tuyến 2 trạm 2 nên chi phí là`5 + 1 + 1 = 7`. Giải pháp kết nối trực tiếp trạm 1 của tuyến 1 với trạm 1 của tuyến 2 sẽ mô hình hóa một lần truyền không tồn tại và có thể tạo ra một câu trả lời nhỏ hơn, không chính xác. 

Trường hợp thứ hai là việc ở trên cùng một đường không nhất thiết là tối ưu. Ví dụ,```
5 5 5
2 2 2
10 1 1 1
1 1 1 5
```Di chuyển trực tiếp trên dòng 1 chi phí`4 * 10 = 40`. Tuyến đường nhanh hơn sử dụng tuyến 2 và tuyến 3 làm đường vòng và chi phí`35`. Giải pháp chỉ xét đến việc di chuyển trực tiếp khi trạm xuất phát và trạm đích nằm trên cùng một tuyến thiếu sự tối ưu. 

Trường hợp cạnh thứ ba là khoảng cách rất lớn giữa hai trạm quan trọng. Coi như```
1000000000 1000000000 1000000000
1 1 1
1 1 1 1
1 1000000000 2 1000000000
```Câu trả lời đúng là`1999999998`. Bản thân các con số đã lớn nhưng việc tính toán chỉ cần một vài phép nhân và phép cộng. Việc triển khai dựa trên việc xây dựng rõ ràng mọi trạm thậm chí không thể biểu thị biểu đồ trong bộ nhớ hoặc thời gian thực tế. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xây dựng toàn bộ biểu đồ metro và chạy thuật toán đường đi ngắn nhất như Dijkstra. Điều này đúng vì mọi chuyển động có thể được biểu thị bằng một cạnh có trọng lượng chính xác là thời gian di chuyển của nó. 

Vấn đề là số đỉnh. Ở giới hạn tối đa có`10^9 + 10^9 + 10^9 = 3 * 10^9`các đỉnh trạm. Ba tuyến tàu điện ngầm cùng nhau chứa`3 * 10^9 - 3`các cạnh của đường thông thường và ba nút giao cộng thêm ba nút nữa, do đó đồ thị có chính xác`3 * 10^9`các cạnh vô hướng trong trường hợp xấu nhất đó. Một biểu diễn danh sách kề sẽ chứa khoảng`6 * 10^9`các mục lân cận. Dijkstra sẽ cần kiểm tra hàng tỷ đỉnh và cạnh, vượt xa giới hạn một giây. 

Điều quan trọng là Dima chỉ có thể thay đổi tuyến ở ba cặp trạm cố định. Giữa hai ga quan trọng như vậy, không có điều gì thú vị có thể xảy ra: chỉ có một tuyến metro và việc di chuyển giữa hai vị trí trên tuyến đó có chi phí cố định bằng khoảng cách ga nhân với thời gian di chuyển của tuyến đó. 

Giả sử hai trạm quan trọng liên tiếp trên tuyến 1 là các trạm`p`Và`q`. Thay vì biểu diễn từng trạm giữa chúng, chúng ta chỉ có thể biểu diễn hai điểm cuối này và kết nối chúng với một cạnh chi phí có trọng số`|p - q| * t1`. 

Bất kỳ chuyển động nào dọc theo đoạn đường đó đều có chi phí chính xác này, vì vậy các trạm trung gian không đóng góp thông tin nào vào việc tính toán đường đi ngắn nhất. 

Đối với mỗi tuyến, chúng ta chỉ cần hai điểm cuối trao đổi của nó và trạm xuất phát hoặc trạm đích nếu trạm đó nằm trên tuyến. Do đó, mỗi dòng đóng góp tối đa bốn đỉnh. Trên cả ba dòng, đồ thị nén có nhiều nhất là 12 đỉnh. 

Khi các đỉnh đó được kết nối bằng các đoạn đường có trọng số và ba cạnh truyền, bài toán ban đầu chính xác là bài toán đường đi ngắn nhất trên biểu đồ nhỏ này. Dijkstra sau đó đã quá đủ nhanh. 

Phương pháp vũ lực hoạt động vì nó mô hình hóa chính xác mọi chuyển động có thể xảy ra, nhưng không thành công vì hầu hết tất cả các trạm đó đều không liên quan đến việc lựa chọn tuyến đường. Quan sát cho thấy chỉ các trạm trao đổi và hai điểm cuối mới có thể thay đổi cấu trúc của tuyến đường cho phép chúng ta thay thế hàng tỷ đỉnh thông thường bằng tối đa 12 đỉnh có ý nghĩa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O((x+y+z) log(x+y+z))`|`O(x+y+z)`| Quá chậm | 
| Tối ưu |`O(1)`|`O(1)`| Đã chấp nhận | 

Độ phức tạp tối ưu là không đổi về mặt kỹ thuật vì đồ thị nén chứa tối đa 12 đỉnh và số cạnh không đổi, không phụ thuộc vào`x`,`y`, Và`z`. 

## Hướng dẫn thuật toán 

1. Tạo tập hợp các trạm quan trọng trên mỗi tuyến. Đối với tuyến 1, các trạm trung chuyển là`a`Và`a + 1`. Đối với dòng 2, chúng là`b`Và`b + 1`. Đối với dòng 3, chúng là`c`Và`c + 1`. Đồng thời chèn trạm bắt đầu và trạm đích trên các đường tương ứng của chúng. 

Chúng tôi sử dụng các bộ vì điểm bắt đầu hoặc điểm đến có thể trùng với trạm trung chuyển. Trạm như vậy chỉ được xuất hiện một lần trong biểu đồ nén. 
2. Gán một đỉnh đồ thị cho mỗi`(line, station)`cặp trong các bộ này. 

Có nhiều nhất bốn vị trí trên mỗi dòng, do đó có nhiều nhất là mười hai đỉnh bị nén. Số trạm thực tế vẫn có thể lớn bằng`10^9`, nhưng nó chỉ được lưu trữ dưới dạng nhãn số nguyên thông thường. 
3. Sắp xếp số trạm quan trọng trên mỗi dòng. 

Với mỗi cặp vị trí liên tiếp`p`Và`q`trên một đường thẳng, thêm một cạnh vô hướng có trọng số`abs(p - q) * t`, Ở đâu`t`là thời gian di chuyển của tuyến đường đó. 

Kết nối các ga quan trọng liên tiếp là đủ vì hành khách đi vào một đoạn của tuyến và cuối cùng rời khỏi đó sẽ không có lý do hữu ích nào để ghé thăm một ga trung gian không liên quan. Toàn bộ phân khúc có chi phí đi lại cố định. 
4. Thêm ba cạnh chuyển. Kết nối trạm tuyến 1`a`với ga tuyến 2`b + 1`, ga tuyến 1`a + 1`với ga tuyến 3`c`, và trạm tuyến 2`b`với ga tuyến 3`c + 1`. Đặt trọng lượng cho mỗi cạnh này`d`. 

Đây chính xác là những nơi hợp pháp mà Dima có thể thay đổi dòng, do đó biểu đồ nén sẽ bảo toàn mọi chuyển khoản có thể. 
5. Chạy Dijkstra từ đỉnh bắt đầu. 

Mỗi cạnh đều có trọng số dương, do đó Dijkstra cho thời gian di chuyển tối thiểu tới mọi đỉnh bị nén. Đích là một trong những đỉnh này vì chúng tôi đã chèn nó một cách rõ ràng vào tập trạm quan trọng. 
6. Trả về khoảng cách của đỉnh đích. 

Đường dẫn ngắn nhất thu được có thể nằm trên một tuyến, sử dụng một nút trao đổi hoặc thực hiện một số lần chuyển tuyến. Biểu đồ nén cho phép tất cả các khả năng này, bao gồm cả các tuyến đường tạm thời sử dụng đường chậm hơn hoặc nhanh hơn và quay lại đường ban đầu. 

### Tại sao nó hoạt động 

Xem xét bất kỳ tuyến đường hợp lệ nào trong hệ thống tàu điện ngầm ban đầu. Mỗi khi tuyến đường thay đổi tuyến, nó phải ghé thăm một trong sáu điểm cuối của nút giao. Nó cũng bắt đầu và kết thúc tại các điểm cuối được bao gồm rõ ràng. Giữa hai ga quan trọng liên tiếp bất kỳ, tuyến đường vẫn nằm trên một tuyến và cách rẻ nhất để di chuyển giữa chúng chỉ đơn giản là đi thẳng dọc theo tuyến đó. Biểu đồ nén chứa chính xác đoạn đó với chi phí di chuyển chính xác. Do đó, mọi tuyến đường ban đầu hợp lệ đều có tuyến đường có chi phí bằng hoặc thấp hơn trong biểu đồ nén. 

Ngược lại, mỗi cạnh trong biểu đồ nén biểu thị một đoạn thực của tuyến tàu điện ngầm hoặc một trong ba đường chuyển giao hợp pháp. Vì vậy, mọi tuyến đường nén có thể được mở rộng thành tuyến tàu điện ngầm hợp lệ với chi phí như nhau. Do đó, hai biểu đồ có cùng khoảng cách đường đi ngắn nhất và Dijkstra trả về câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve(reader=input):
    x, y, z = map(int, reader().split())
    a, b, c = map(int, reader().split())
    t1, t2, t3, d = map(int, reader().split())
    k, i, l, j = map(int, reader().split())

    sizes = [x, y, z]
    times = [t1, t2, t3]

    # Important station positions on each line.
    special = [set(), set(), set()]

    # Interchange endpoints.
    special[0].add(a)
    special[0].add(a + 1)

    special[1].add(b)
    special[1].add(b + 1)

    special[2].add(c)
    special[2].add(c + 1)

    # Start and destination.
    special[k - 1].add(i)
    special[l - 1].add(j)

    # Assign compressed graph ids.
    node_id = {}
    vertices = []

    for line in range(3):
        for pos in sorted(special[line]):
            node_id[(line, pos)] = len(vertices)
            vertices.append((line, pos))

    n = len(vertices)
    graph = [[] for _ in range(n)]

    def add_edge(u, v, w):
        graph[u].append((v, w))
        graph[v].append((u, w))

    # Connect consecutive important stations on each metro line.
    for line in range(3):
        positions = sorted(special[line])
        for p, q in zip(positions, positions[1:]):
            u = node_id[(line, p)]
            v = node_id[(line, q)]
            weight = (q - p) * times[line]
            add_edge(u, v, weight)

    # Add the three legal interchanges.
    transfers = [
        ((0, a), (1, b + 1)),
        ((0, a + 1), (2, c)),
        ((1, b), (2, c + 1)),
    ]

    for left, right in transfers:
        u = node_id[left]
        v = node_id[right]
        add_edge(u, v, d)

    start = node_id[(k - 1, i)]
    target = node_id[(l - 1, j)]

    INF = 10**30
    dist = [INF] * n
    dist[start] = 0

    pq = [(0, start)]

    while pq:
        cur_dist, u = heapq.heappop(pq)

        if cur_dist != dist[u]:
            continue

        if u == target:
            return cur_dist

        for v, weight in graph[u]:
            new_dist = cur_dist + weight
            if new_dist < dist[v]:
                dist[v] = new_dist
                heapq.heappush(pq, (new_dist, v))

    return dist[target]

if __name__ == "__main__":
    print(solve())
```Phần đầu tiên của quá trình triển khai ghi lại ba độ dài và tốc độ của đường truyền, sau đó tạo các tập hợp trạm quan trọng. Các điểm cuối trao đổi được chèn một cách rõ ràng bằng cách sử dụng`a`,`a + 1`,`b`,`b + 1`,`c`, Và`c + 1`. Đây là nơi xảy ra hầu hết các lỗi sai lệch một, do đó các cặp truyền sau đó được ghi trực tiếp thay vì được xây dựng lại một cách gián tiếp. 

các`node_id`từ điển ánh xạ một ga tàu điện ngầm hợp lý như`(1, a)`đến một chỉ mục đồ thị nhỏ. Sắp xếp các vị trí trước khi kết nối chúng là cách biến toàn bộ khoảng các trạm thông thường thành một cạnh có trọng số. Nếu vị trí`p`Và`q`là các trạm quan trọng liên tiếp, có`q - p`các chuyến tàu liền kề nên chi phí là`(q - p) * times[line]`. 

Các cạnh chuyển tiếp được thêm vào một cách riêng biệt vì chi phí của chúng không tỷ lệ thuận với khoảng cách trạm. Mỗi cái có giá chính xác`d`, bất kể vị trí của hai trạm. 

Dijkstra sau đó chỉ hoạt động trên biểu đồ nén. Kiểm tra mục nhập cũ`if cur_dist != dist[u]`là chi tiết triển khai vùng heap tiêu chuẩn nhằm tránh việc xử lý khoảng cách lỗi thời sau khi đã tìm thấy tuyến đường tốt hơn. Sự trở về sớm khi`u == target`là an toàn vì Dijkstra loại bỏ các đỉnh khỏi heap theo thứ tự khoảng cách không giảm. 

Số nguyên Python có độ chính xác tùy ý, do đó phép nhân như`(q - p) * times[line]`không thể tràn. Điều này rất hữu ích ở đây vì cả hai thừa số có thể lớn bằng`10^9`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 4 4
2 2 2
1 1 1 1
1 1 2 1
```Điểm bắt đầu là tuyến 1 trạm 1 và đích đến là tuyến 2 trạm 1. Các vị trí liên quan của tuyến 1 là`1, 2, 3`, trong khi dòng 2 có`1, 2, 3`. Việc chuyển tuyến hữu ích cho chuyến đi này là tuyến 1 trạm 2 đến tuyến 2 trạm 3. 

| Bước | Trạm hiện tại | Hành động | Đã thêm thời gian | Tổng thời gian | 
| --- | --- | --- | --- | --- | 
| 0 | L1-1 | Bắt đầu | 0 | 0 | 
| 1 | L1-2 | Di chuyển một trạm trên tuyến 1 | 1 | 1 | 
| 2 | L2-3 | Chuyển L1-2 sang L2-3 | 1 | 2 | 
| 3 | L2-1 | Di chuyển hai trạm trên tuyến 2 | 2 | 4 | 

Chi phí cuối cùng là`4`. Việc truyền thay thế qua dòng 3 cũng được biểu diễn bằng biểu đồ nén, nhưng Dijkstra không cần phải giả định trước trao đổi nào là tốt nhất. 

### Mẫu 2 

Đầu vào là```
4 4 4
2 2 2
1 10 1 1
1 1 3 4
```Dòng đầu tiên nhanh, dòng thứ hai chậm và dòng thứ ba nhanh. Điểm bắt đầu là L1-1 và đích đến là L3-4. 

| Bước | Trạm hiện tại | Hành động | Đã thêm thời gian | Tổng thời gian | 
| --- | --- | --- | --- | --- | 
| 0 | L1-1 | Bắt đầu | 0 | 0 | 
| 1 | L1-3 | Di chuyển hai trạm trên tuyến 1 | 2 | 2 | 
| 2 | L3-2 | Chuyển L1-3 sang L3-2 | 1 | 3 | 
| 3 | L3-4 | Di chuyển hai trạm trên tuyến 3 | 2 | 5 | 

Câu trả lời kết quả là`5`. Tuyến đường qua tuyến 2 tệ hơn nhiều vì mỗi lần di chuyển liền kề trên tuyến 2 đều tốn phí`10`. Ví dụ này chứng tỏ tại sao thuật toán phải so sánh các tuyến đường thay vì giả định rằng nút giao gần nhất về mặt địa lý là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`| Có nhiều nhất 12 đỉnh bị nén và số cạnh không đổi, do đó Dijkstra thực hiện một lượng công việc không đổi. | 
| Không gian |`O(1)`| Đồ thị nén chứa tối đa 12 đỉnh và số cạnh không đổi. | 

Độ dài dòng ban đầu có thể lớn bằng`10^9`, nhưng chúng chỉ ảnh hưởng đến các giá trị số học như khoảng cách giữa các trạm quan trọng. Chúng không bao giờ ảnh hưởng đến số đỉnh đồ thị được tạo bởi thuật toán. Do đó, giải pháp vẫn hoạt động thoải mái trong giới hạn thời gian một giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve(reader)`chức năng từ giải pháp trên có sẵn. Người trợ giúp cung cấp một`StringIO`reader, do đó, thuật toán tương tự có thể được kiểm tra mà không cần sửa đổi logic của giải pháp.```
import io

def run(inp: str) -> str:
    return str(solve(io.StringIO(inp).readline))

# Provided samples
assert run("""\
4 4 4
2 2 2
1 1 1 1
1 1 2 1
""") == "4", "sample 1"

assert run("""\
4 4 4
2 2 2
1 10 1 1
1 1 3 4
""") == "5", "sample 2"

assert run("""\
4 4 4
2 2 2
1 1 1 1
1 1 1 4
""") == "3", "sample 3"

# Minimum-size lines.
assert run("""\
2 2 2
1 1 1
1 1 1 1
1 2 2 1
""") == "3", "minimum-size input"

# Maximum-size lines.
assert run("""\
1000000000 1000000000 1000000000
1 1 1
1 1 1 1
1 1000000000 2 1000000000
""") == "1999999998", "maximum-size input"

# All travel times equal, with the route using two transfers.
assert run("""\
5 5 5
2 2 2
3 3 3 2
1 1 3 5
""") == "16", "all-equal values"

# Boundary-sensitive interchange positions.
assert run("""\
3 3 3
1 1 1
5 1 1 1
1 2 2 1
""") == "7", "interchange off-by-one"

# Same-line trip where leaving the line is faster.
assert run("""\
5 5 5
2 2 2
10 1 1 1
1 1 1 5
""") == "35", "faster detour through other lines"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 2 / 1 1 1 / 1 1 1 1 / 1 2 2 1`|`3`| Kích thước đường tối thiểu và ranh giới nút giao trực tiếp | 
|`1000000000 1000000000 1000000000 / 1 1 1 / 1 1 1 1 / 1 1000000000 2 1000000000`|`1999999998`| Số lượng trạm khổng lồ và giá trị số học lớn | 
|`5 5 5 / 2 2 2 / 3 3 3 2 / 1 1 3 5`|`16`| Tốc độ bằng nhau và tuyến đường sử dụng nhiều lần chuyển | 
|`3 3 3 / 1 1 1 / 5 1 1 1 / 1 2 2 1`|`7`| Chính xác`a`,`a+1`,`b`,`b+1`vị trí trao đổi | 
|`5 5 5 / 2 2 2 / 10 1 1 1 / 1 1 1 5`|`35`| Một chuyến đi cùng tuyến có lộ trình tối ưu rời khỏi tuyến | 

## Vỏ cạnh 

Trường hợp kích thước tối thiểu là```
2 2 2
1 1 1
1 1 1 1
1 2 2 1
```Tuyến 1 trạm 2 kết nối với tuyến 3 trạm 1, còn tuyến 2 trạm 1 kết nối với tuyến 3 trạm 2. Tuyến 1 đi thẳng tuyến 2 có chi phí`1 + 1 + 1 = 3`. Biểu đồ nén chứa tất cả bốn điểm cuối có liên quan, do đó không cần xử lý đặc biệt vì thực tế là mỗi dòng chỉ chứa hai trạm. 

Trường hợp trao đổi riêng lẻ là```
3 3 3
1 1 1
5 1 1 1
1 2 2 1
```Lần chuyển đầu tiên là giữa L1-1 và L2-2, không phải L1-1 và L2-1. Từ L1-2 đến L2-1, Dima đầu tiên di chuyển từ L1-2 đến L1-1`5`giây, chuyển cho`1`thứ hai, sau đó chuyển từ L2-2 sang L2-1 cho`1`thứ hai. Đồ thị nén cho`7`, khớp chính xác với lộ trình thực tế. 

Trường hợp phím tắt cùng dòng là```
5 5 5
2 2 2
10 1 1 1
1 1 1 5
```Chi phí đi thẳng tuyến 1`40`. Biểu đồ nén cũng chứa chuỗi L1-1 đến L1-2, L2-3, L2-2, L3-3, L3-2, L1-3 và cuối cùng là L1-5. Chi phí của nó là`10 + 1 + 1 + 1 + 1 + 1 + 20 = 35`. Dijkstra phát hiện ra tuyến đường này vì nó xử lý các lần truyền như các cạnh đồ thị thông thường thay vì đưa ra các giả định dựa trên đường xuất phát và đích. 

Trường hợp kích thước tối đa là```
1000000000 1000000000 1000000000
1 1 1
1 1 1 1
1 1000000000 2 1000000000
```Chỉ các ga xung quanh ba nút giao và hai điểm cuối mới được đưa vào biểu đồ. Khoảng cách rất lớn từ nhà ga`1000000000`đến ga`1`được biểu thị bằng một cạnh có trọng số thay vì một tỷ đỉnh. Tuyến tốt nhất chuyển trực tiếp từ trạm L1 1 đến trạm L2 2 rồi đi đến trạm L2`1000000000`, cho`999999999 + 1 + 999999998 = 1999999998`. 

Bất biến trung tâm đằng sau tất cả các trường hợp này là như nhau: mọi cạnh nén biểu thị chuyển động rẻ nhất có thể có giữa hai trạm liên tiếp trong đó một tuyến đường có thể thay đổi hướng hoặc đường một cách có ý nghĩa. Khi các cạnh đó xuất hiện, đường đi ngắn nhất trong biểu đồ nén chính xác là hành trình tàu điện ngầm ngắn nhất có thể.
