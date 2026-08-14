---
title: "CF 102341H - Thôi miên"
description: "Chúng tôi có một đồ thị vô hướng được kết nối với tối đa 200.000 giao lộ và 200.000 con đường. Chúng tôi bắt đầu từ đỉnh 1 và muốn đạt đến đỉnh n. Đi qua một con đường mất một phút, nhưng mỗi con đường đều ẩn chứa một trong hai loại Hypno."
date: "2026-08-14T05:10:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "H"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 158
verified: true
draft: false
---

[CF 102341H - Hypno](https://codeforces.com/problemset/problem/102341/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đồ thị vô hướng được kết nối với tối đa 200.000 giao lộ và 200.000 con đường. Chúng tôi bắt đầu từ đỉnh 1 và muốn đạt đến đỉnh n. Đi qua một con đường mất một phút, nhưng mỗi con đường đều ẩn chứa một trong hai loại Hypno. Với xác suất 1/2, Hypno vô hại đối với chúng ta, trong trường hợp đó mọi lần truyền tải đều thành công. Với xác suất 1/2 thì nó có hại, trong trường hợp đó mọi lần truyền tải đều thành công độc lập với xác suất 1/2. 

Chúng tôi không biết loại đường trước khi sử dụng nó. Việc đi qua không thành công là thông tin đặc biệt có giá trị: nó chứng tỏ rằng con đường đó có hại. Một lần truyền tải thành công không hoàn toàn xác định được Hypno, nhưng khi chúng ta tiếp tục từ đỉnh mới, thông tin thu được cho đến nay đủ để làm cho các chiến lược thích ứng trở nên hữu ích. 

Nhiệm vụ là tính thời gian di chuyển dự kiến ​​tối thiểu có thể từ đỉnh 1 đến đỉnh n. Sai số tuyệt đối hoặc tương đối bắt buộc tối đa là 10^-9. 

Các ràng buộc loại trừ các thuật toán liên tục kiểm tra các phần lớn của biểu đồ. Với 200.000 đỉnh và 200.000 cạnh, phương pháp O(nm) có thể thực hiện khoảng 4 * 10^10 phép toán kề trong trường hợp xấu nhất, vượt xa giới hạn hai giây. Về cơ bản chúng ta cần công việc tuyến tính hoặc O((n + m) log n). 

Trường hợp cạnh nhỏ đầu tiên là đồ thị chỉ có hai đỉnh.```
2 1
1 2
```Câu trả lời là 1,5. Một giải pháp bất cẩn coi mọi con đường đều có thời gian di chuyển dự kiến ​​là 2 sẽ tạo ra 2, quên đi xác suất miễn nhiễm 1/2. 

Trường hợp cạnh thứ hai là con đường trực tiếp đến đích cùng với các lựa chọn thay thế không liên quan.```
3 2
1 3
1 2
```Câu trả lời lại là 1,5. Có thể đến đích bằng con đường đầu tiên, vì vậy thông tin về đỉnh 2 là không liên quan. Một giải pháp tính trung bình trên tất cả các con đường đi thay vì tối ưu hóa có thể tạo ra giá trị kém hơn. 

Trường hợp thú vị là một số lựa chọn thay thế tốt như nhau. Trong biểu đồ```
4 4
1 2
2 4
1 3
3 4
```mỗi tuyến đường hai cạnh có giá kỳ vọng là 3, nhưng câu trả lời tối ưu là 2,875. Sau lần thử đầu tiên không thành công ở một tuyến đường, việc thử tuyến đường khác có thể tốt hơn là thử lại ngay con đường xấu đã biết. Một giải pháp gán cho mỗi con đường một chi phí cố định là 1,5 và chỉ chạy con đường ngắn nhất thông thường, do đó sẽ bỏ qua quan sát trung tâm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ giữ khoảng cách của tất cả các đỉnh lân cận đã được phát hiện đối với mỗi đỉnh và tính toán lại chiến lược cục bộ tốt nhất mỗi khi có một đỉnh lân cận khác. Để đánh giá chiến lược cục bộ, chúng tôi sắp xếp các giá trị lân cận đã biết và kiểm tra tiền tố có thể hữu ích. Điều này đúng vì các hàng xóm được xem xét theo thứ tự tăng dần của các giá trị tiếp tục tối ưu của chúng. 

Vấn đề là việc quét toàn bộ sau mỗi lần cập nhật có thể kiểm tra cùng một danh sách lân cận nhiều lần. Đối với đỉnh có độ d, giá trị này có thể lấy O(d^2) và trên toàn bộ đồ thị, trường hợp xấu nhất là O(m^2). Với m = 200.000, có thể đạt khoảng 4 * 10^10 lần kiểm tra lân cận. 

Quan sát quan trọng là các hàng xóm được biết theo thứ tự tăng dần của các giá trị tối ưu nếu chúng ta xử lý biểu đồ bằng thuật toán Dijkstra bắt đầu từ đích. Khi đã biết được láng giềng hữu ích đầu tiên của một đỉnh, mọi láng giềng sau đó sẽ đến theo thứ tự khoảng cách không giảm. Chúng ta có thể cập nhật dần dần giá trị mong đợi thay vì tính toán lại toàn bộ tiền tố. 

Xét đỉnh u có đỉnh lân cận tốt nhất có giá trị tiếp tục tối ưu x. Giả sử chúng ta đã thất bại trên k con đường mới từ bạn. Con đường thất bại đầu tiên là con đường dự phòng được biết đến nhiều nhất và hiện được biết là có hại. Việc băng qua con đường đó từ điểm này dự kiến ​​sẽ mất thêm hai phút nữa, sau đó chi phí đi tiếp là x. 

Trước khi xem xét một con đường mới khác có giá trị tiếp tục y, chi phí dự phòng là 2 + x. Việc thử con đường mới tốn một phút ngay lập tức. Với xác suất 3/4 nó thành công, tiếp tục y. Với xác suất 1/4 nó thất bại, sau đó chi phí dự phòng là 2 + x. Vì vậy, chi phí dự kiến để thử con đường bổ sung này là 

1 + 3/4 y + 1/4(2 + x). 

Dùng thử nó rất hữu ích khi 

1 + 3/4 y + 1/4(2 + x) < 2 + x, 

đơn giản hóa để 

y < x + 2/3. 

Ngưỡng này là lý do thuật toán có thể ngừng xử lý các hàng xóm vĩnh viễn khi khoảng cách của chúng trở nên quá lớn. 

Đối với con đường đầu tiên, xác suất đi qua thành công là 3/4, bởi vì chúng ta miễn dịch hoặc chúng ta dễ bị tổn thương và điểm cuối ngẫu nhiên là điểm đối lập. Nếu thất bại, xảy ra với xác suất 1/4, con đường được coi là có hại. 

Nếu những người hàng xóm hữu ích có các giá trị x1, x2, ..., xk theo thứ tự tăng dần thì giá trị mong đợi là 

E_k = Σ từ i=1 đến k của (1/4)^(i-1) * (3/4) * (i + x_i) 
+ (1/4)^k * (k + 2 + x_1). 

Phần quan trọng là E_k có thể được cập nhật liên tục. Bắt đầu với E_1 = 1,5 + x_1, việc thêm x_k sẽ thay đổi câu trả lời bằng 

(1/4)^(k-1) * (3/4 * (x_k - x_1) - 1/2). 

Vì vậy, mỗi cạnh chỉ cần được xử lý một lần, khi điểm cuối của nó được Dijkstra hoàn thiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đảo ngược quan điểm và chạy Dijkstra từ đỉnh n. Giá trị được lưu trữ cho một đỉnh sẽ biểu thị thời gian còn lại dự kiến ​​tối thiểu từ đỉnh đó đến n. 

Đồ thị là vô hướng nên điều này không đòi hỏi phải thay đổi các cạnh về mặt vật lý. Chúng ta chỉ cần khởi tạo n với khoảng cách bằng 0 và truyền thông tin đến các hàng xóm của nó. 

1. Duy trì, với mỗi đỉnh u, khoảng cách lân cận cuối cùng nhỏ nhất`first[u]`. Trước khi người hàng xóm đầu tiên được hoàn thiện, bạn không có chiến lược ứng cử viên nào. 

Khi hàng xóm đầu tiên v với khoảng cách d được hoàn tất, chiến lược tốt nhất hiện được biết là sử dụng con đường đó cho đến khi thành công. Một con đường mới dự kiến ​​mất 1,5 phút, vì vậy chúng tôi đặt`value[u] = d + 1.5`. 

1. Sau khi hàng xóm đầu tiên được xử lý, duy trì xác suất`p`đến được con đường mới tiếp theo sau khi tất cả những con đường mới hữu ích trước đó đều không thành công. 

Mỗi con đường mới bị lỗi với xác suất 1/4, vì vậy sau khi xử lý k đường hữu ích, xác suất này là`(1/4)^k`. 

1. Khi một hàng xóm khác có khoảng cách d được hoàn tất, hãy so sánh nó với`first[u]`. 

Nếu như`d >= first[u] + 2/3`, nó không bao giờ có thể cải thiện chiến lược. Tất cả những người hàng xóm được hoàn thiện sau đó đều có khoảng cách ít nhất là d, vì vậy họ cũng không thể cải thiện khoảng cách đó. Chúng tôi có thể ngừng xử lý hàng xóm của bạn. 

Nếu như`d < first[u] + 2/3`, hãy đưa nó vào chiến lược. Giá trị kỳ vọng hiện tại thay đổi theo`p * (0.75 * (d - first[u]) - 0.5)`. 

Sau đó nhân p với 1/4, vì để đến được con đường mới tiếp theo cần phải có một lần thất bại nữa. 

1. Mỗi khi giá trị hiện tại của u được cải thiện, hãy chèn cặp mới`(value[u], u)`vào hàng đợi ưu tiên. 

Một số mục cho cùng một đỉnh có thể tồn tại. Khi một mục được bật lên, hãy loại bỏ nó nếu nó không còn bằng giá trị tốt nhất hiện tại. Đây là kỹ thuật xóa lười tiêu chuẩn được sử dụng trong triển khai Dijkstra. 

1. Khi đỉnh 1 được hoàn thiện, giá trị của nó chính là câu trả lời. 

Thứ tự Dijkstra hợp lệ vì các láng giềng hữu ích của một đỉnh thỏa mãn`d < first[u] + 2/3`. 

Giá trị hiện tại của u luôn lớn hơn`first[u] + 2/3`, vì vậy mọi hàng xóm hữu ích phải được hoàn thiện trước khi chính u có thể được hoàn thiện. Do đó, khi bạn rời khỏi hàng ưu tiên, mọi hàng xóm có thể cải thiện câu trả lời của mình đều đã được đưa vào. 

### Tại sao nó hoạt động 

Đối với đỉnh u cố định, chiến lược tối ưu xem xét các ứng viên lân cận theo thứ tự không giảm của các giá trị tiếp tục tối ưu của chúng. Sau con đường đầu tiên bị hỏng, con đường đó trở thành một tuyến đường dự phòng có hại với thời gian băng qua dự kiến ​​còn lại 2. Đường mới hữu ích chính xác khi giá trị tiếp tục của nó nhỏ hơn`first[u] + 2/3`, do đó các hàng xóm hữu ích tạo thành tiền tố của thứ tự Dijkstra. 

Giá trị được duy trì chính xác là chi phí dự kiến ​​khi thử tiền tố đó và sau đó liên tục sử dụng đường bị lỗi đầu tiên nếu mọi lần thử mới đều không thành công. Công thức tăng dần tương đương về mặt đại số với công thức kỳ vọng đầy đủ, do đó, mọi cập nhật đều bảo toàn giá trị tối ưu chính xác trong số tất cả các lân cận hữu ích hiện có. 

Vì mọi hàng xóm có khả năng hữu ích đều có khoảng cách nhỏ hơn giá trị tại u, nên tất cả những hàng xóm như vậy đều được hoàn thiện trước u. Do đó, khi Dijkstra hoàn thiện u, không hàng xóm tương lai nào có thể cải thiện giá trị của nó. Điều này mang lại tính bất biến về độ chính xác giống như Dijkstra thông thường: giá trị chưa hoàn thiện nhỏ nhất đã là tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    INF = float("inf")

    dist = [INF] * n

    # first[u] is the smallest finalized neighbor distance.
    first = [INF] * n

    # value[u] is the best expected value currently known for u.
    value = [INF] * n

    # prob[u] is the probability of reaching the next fresh edge
    # after all currently useful edges have failed.
    prob = [0.0] * n

    # Whether we have already found the first neighbor.
    seen = [False] * n

    pq = [(0.0, n - 1)]
    dist[n - 1] = 0.0

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        if u == 0:
            print(f"{cur:.12f}")
            return

        du = cur

        for v in graph[u]:
            d = du

            if not seen[v]:
                seen[v] = True
                first[v] = d

                # One fresh edge has expected traversal cost 3/2.
                value[v] = d + 1.5

                # To reach the second fresh edge, the first one
                # must have failed, which happens with probability 1/4.
                prob[v] = 0.25

                if value[v] < dist[v]:
                    dist[v] = value[v]
                    heapq.heappush(pq, (dist[v], v))

            else:
                # Later neighbors are processed in nondecreasing d.
                # Once this threshold fails, no later neighbor can help.
                if d >= first[v] + 2.0 / 3.0:
                    continue

                # Add this neighbor to the useful prefix.
                value[v] += prob[v] * (
                    0.75 * (d - first[v]) - 0.5
                )

                prob[v] *= 0.25

                if value[v] < dist[v]:
                    dist[v] = value[v]
                    heapq.heappush(pq, (dist[v], v))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ đồ thị vô hướng theo cách thông thường. Không cần thiết phải xây dựng một biểu đồ đảo ngược riêng biệt vì mọi con đường đều có hai chiều.`dist`là giá trị Dijkstra.`value`lưu trữ biểu thức cục bộ gia tăng, trong khi`first`ghi nhớ giá trị tiếp tục tốt nhất trong số các hàng xóm được xử lý cho đến nay.`prob`lưu trữ xác suất đến được con đường chưa sử dụng tiếp theo. 

Người hàng xóm đầu tiên cần được xử lý đặc biệt. Đường của nó còn mới nên thời gian đi qua dự kiến ​​vô điều kiện là 3/2. Xác suất thất bại trong lần thử đầu tiên là 1/4, trở thành xác suất đi đến con đường mới thứ hai. 

Đối với mỗi người hàng xóm sau này, biểu thức```
prob[v] * (0.75 * (d - first[v]) - 0.5)
```chính xác là sự thay đổi trong giá trị mong đợi gây ra bằng cách thêm hàng xóm đó vào tiền tố hữu ích. 

Ngưỡng sử dụng`2.0 / 3.0`, không`1.0 / 2.0`. Đây là một nơi phổ biến cho việc thực hiện không chính xác. Bản thân nỗ lực làm đường mới luôn tốn một phút, sau đó thành công với xác suất 3/4 và lùi lại với xác suất 1/4. So sánh hai chiến lược đó cho ra ngưỡng 2/3. 

Điểm nổi là đủ ở đây. số nhân`prob`giảm theo hệ số 4 đối với mỗi hàng xóm hữu ích bổ sung, do đó chỉ sau vài chục hàng xóm hữu ích, đóng góp của nó thấp hơn nhiều so với độ chính xác yêu cầu 10^-9. Số nguyên Python không tham gia vào quá trình lặp lại số, do đó không có vấn đề tràn số nguyên. 

Hàng đợi ưu tiên sử dụng tính năng xóa lười. Một đỉnh có thể nhận được nhiều ước lượng tốt hơn dần dần trước khi nó được hoàn thiện. Chỉ mục phù hợp với hiện tại`dist[u]`được xử lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác và đỉnh 3 là đích đến.```
3 3
1 2
1 3
2 3
```Từ đỉnh 3, cả hai đỉnh 1 và 2 ban đầu đều có giá trị tiếp tục bằng 0. 

| Đỉnh hoàn thiện | Hàng xóm cập nhật | Giá trị đầu tiên | Giá trị hiện tại | 
| --- | --- | --- | --- | 
| 3 | 1 | 0 | 1,500000 | 
| 3 | 2 | 0 | 1,500000 | 

Đỉnh 1 nhận được cạnh trực tiếp tới đích, cho 1,5. Cạnh kia không thể cải thiện nó vì giá trị tiếp tục của nó cũng bằng 0, nhưng đỉnh đó đã được hoàn thiện trước khi một bản cập nhật hữu ích khác có thể quan trọng. 

Do đó, câu trả lời là 1,500000000000. 

Dấu vết cho thấy tại sao một con đường mới có chi phí dự kiến ​​là 1,5. Lần thử đầu tiên thành công với xác suất 3/4. Nếu thất bại, con đường được xác định là có hại và số lần thử dự kiến ​​còn lại là hai, đưa ra tổng thời gian dự kiến ​​là 2 điều kiện trên con đường có hại. 

### Mẫu 2 

Đồ thị là```
4 4
1 2
2 4
4 3
3 1
```với đích đến 4. 

Đỉnh 2 và 3 đều có một đường mới tới đích nên cả hai đều có giá trị 1,5. 

| Đỉnh hoàn thiện | Đỉnh cập nhật | Giá trị đầu tiên | Đã thêm hàng xóm | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| 4 | 2 | 1,500000 | không | 1,500000 | 
| 4 | 3 | 1,500000 | không | 1,500000 | 
| 2 | 1 | 1,500000 | không | 3.000000 | 
| 3 | 1 | 1,500000 | 1,500000 | 2.875000 | 

Đối với đỉnh 1, chỉ sử dụng đỉnh 2 sẽ cho kết quả 3. Tuy nhiên, sau khi đường tới đỉnh 2 thất bại, tuyến đường khác qua đỉnh 3 đáng để thử vì giá trị tiếp tục của nó bằng với giá trị của tuyến đường đầu tiên và nằm dưới giá trị của tuyến đường đầu tiên.`first + 2/3`ngưỡng. 

Lần thử thứ hai đạt được với xác suất 1/4. Việc đưa nó vào làm giảm giá trị từ 3 xuống 2,875, đưa ra câu trả lời mẫu được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi vùng kề được xử lý một lần và mọi cập nhật giá trị hữu ích sẽ được chèn vào vùng nhị phân | 
| Không gian | O(n + m) | Danh sách kề, mảng khoảng cách và hàng đợi ưu tiên yêu cầu không gian tuyến tính | 

Biểu đồ chứa tối đa 200.000 cạnh, do đó thuật toán chỉ thực hiện xử lý cạnh cục bộ O(m) và chèn vùng heap O(m). Độ phức tạp O((n + m) log n) dễ dàng phù hợp với các ràng buộc đã nêu, trong khi mức sử dụng bộ nhớ là tuyến tính theo kích thước biểu đồ. 

## Trường hợp thử nghiệm```python
import io
import heapq
import math

def run(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    graph = [[] for _ in range(n)]

    for _ in range(m):
        a = int(next(it)) - 1
        b = int(next(it)) - 1
        graph[a].append(b)
        graph[b].append(a)

    INF = float("inf")
    dist = [INF] * n
    first = [INF] * n
    value = [INF] * n
    prob = [0.0] * n
    seen = [False] * n

    pq = [(0.0, n - 1)]
    dist[n - 1] = 0.0

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        if u == 0:
            return f"{cur:.12f}"

        for v in graph[u]:
            d = cur

            if not seen[v]:
                seen[v] = True
                first[v] = d
                value[v] = d + 1.5
                prob[v] = 0.25

                if value[v] < dist[v]:
                    dist[v] = value[v]
                    heapq.heappush(pq, (value[v], v))
            else:
                if d >= first[v] + 2.0 / 3.0:
                    continue

                value[v] += prob[v] * (
                    0.75 * (d - first[v]) - 0.5
                )
                prob[v] *= 0.25

                if value[v] < dist[v]:
                    dist[v] = value[v]
                    heapq.heappush(pq, (value[v], v))

    return ""

def check(inp: str, expected: float, message: str):
    actual = float(run(inp))
    assert math.isclose(actual, expected, rel_tol=1e-10, abs_tol=1e-10), (
        message, actual, expected
    )

# Sample 1
check(
    """\
3 3
1 2
1 3
2 3
""",
    1.5,
    "sample 1",
)

# Sample 2
check(
    """\
4 4
1 2
2 4
4 3
3 1
""",
    2.875,
    "sample 2",
)

# Minimum-size graph
check(
    """\
2 1
1 2
""",
    1.5,
    "minimum graph",
)

# A simple chain of three vertices.
# Each of the two roads has expected cost 1.5.
check(
    """\
3 2
1 2
2 3
""",
    3.0,
    "simple chain",
)

# Three equally good two-edge routes.
# The useful values at vertex 1 are 1.5, 1.5, 1.5.
# E1 = 3
# E2 = 2.875
# E3 = 2.84375
check(
    """\
5 6
1 2
2 5
1 3
3 5
1 4
4 5
""",
    2.84375,
    "three equal alternatives",
)

# Large boundary case: a chain with 200000 vertices.
# There are 199999 roads, each contributing 1.5 in expectation.
n = 200000
parts = [f"{n} {n - 1}"]
for i in range(1, n):
    parts.append(f"{i} {i + 1}")

large_input = "\n".join(parts) + "\n"
check(
    large_input,
    1.5 * (n - 1),
    "maximum-size chain",
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`|`1.500000000000`| Đồ thị tối thiểu và xác suất thành công cơ bản 3/4 | 
|`3 2 / 1 2 / 2 3`|`3.000000000000`| Một biểu đồ không có tuyến đường thay thế | 
| Ba đường hai cạnh song song qua các đỉnh 2, 3, 4 |`2.843750000000`| Một số hàng xóm hữu ích như nhau và cập nhật gia tăng lặp đi lặp lại | 
| Chuỗi có 200.000 đỉnh |`299998.500000000000`| Kích thước đầu vào tối đa và cấu trúc đồ thị tuyến tính | 

## Vỏ cạnh 

Đối với đồ thị hai đỉnh```
2 1
1 2
```Dijkstra bắt đầu ở đỉnh 2 với giá trị bằng 0. Khi đỉnh 2 được hoàn thành, đỉnh 1 nhận được hàng xóm đầu tiên với`first[1] = 0`. Giá trị ứng viên của nó trở thành`0 + 1.5 = 1.5`. Không có hàng xóm thứ hai tồn tại, vì vậy câu trả lời chính xác là 1,5. 

Đối với đồ thị chứa cạnh đích trực tiếp,```
3 2
1 3
1 2
```đỉnh 3 được hoàn thiện đầu tiên. Cạnh trực tiếp cho đỉnh 1 có giá trị là 1,5. Mặc dù đỉnh 2 cuối cùng cũng sẽ được xử lý, nhưng khoảng cách của nó không thể cung cấp tuyến đường tốt hơn cạnh đích trực tiếp đã được hoàn thiện. Câu trả lời vẫn là 1,5. 

Đối với đồ thị có ba phương án bằng nhau,```
5 6
1 2
2 5
1 3
3 5
1 4
4 5
```các đỉnh 2, 3 và 4 đều nhận giá trị 1,5 từ đỉnh 5. Hàng xóm đầu tiên như vậy cho đỉnh 1 giá trị 3. Hàng xóm thứ hai thay đổi nó bằng cách`(1/4) * (-1/2) = -1/8`, 

cho 2,875. Thứ ba thay đổi nó bằng cách`(1/16) * (-1/2) = -1/32`, 

cho 2,84375. Phương án thay thế tương đương thứ tư sẽ chỉ cải thiện nó thêm 1/128, v.v. Hệ số xác suất hình học chính xác là yếu tố làm cho việc biểu diễn gia tăng trở nên hiệu quả. 

Đối với một đỉnh có đỉnh lân cận có khoảng cách ít nhất`first + 2/3`, thuật toán sẽ bỏ qua nó. Giả sử hàng xóm được biết đến nhiều nhất có giá trị x và hàng xóm mới có giá trị y. Nếu như`y >= x + 2/3`, việc thử con đường mới đó ít nhất cũng tốn kém bằng việc sử dụng ngay con đường có hại đã biết. Vì tất cả các hàng xóm trong tương lai đều có giá trị cuối cùng lớn hơn nên không giá trị nào trong số chúng có thể trở nên hữu ích. Đây là điều kiện dừng ngăn chặn việc quét lặp lại cùng một danh sách kề. 

Đối với chuỗi có kích thước tối đa, mỗi đỉnh chỉ có một lân cận hữu ích hướng tới đích. Không có con đường thay thế nào được xem xét, vì vậy mỗi cạnh đóng góp chính xác 1,5 vào thời gian dự kiến. Với 199.999 con đường, kết quả là`199999 * 1.5 = 299998.5`. Thuật toán xử lý mọi cạnh một lần và duy trì trong thời gian O((n + m) log n).
