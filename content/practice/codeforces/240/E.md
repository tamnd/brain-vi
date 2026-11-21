---
title: "CF 240E - Sửa chữa đường bộ"
description: "Chúng tôi có một biểu đồ có hướng của các thành phố và đường. Thành phố 1 là thủ đô. Mọi con đường đều có thể sử dụng được hoặc bị hỏng. Đường bị hỏng có thể được sửa chữa, sau đó nó hoạt động giống như một cạnh có hướng bình thường. Chính phủ muốn mọi thành phố đều có thể tiếp cận được từ thủ đô."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 240
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 145 (Div. 1, ACM-ICPC Rules)"
rating: 2800
weight: 240
solve_time_s: 118
verified: true
draft: false
---

[CF 240E - Sửa chữa đường](https://codeforces.com/problemset/problem/240/E) 

**Đánh giá:** 2800 
**Tags:** dfs và tương tự, đồ thị, tham lam 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ có hướng của các thành phố và đường. Thành phố`1`là thủ đô. Mọi con đường đều có thể sử dụng được hoặc bị hỏng. Đường bị hỏng có thể được sửa chữa, sau đó nó hoạt động giống như một cạnh có hướng bình thường. 

Chính phủ muốn mọi thành phố đều có thể tiếp cận được từ thủ đô. Chúng tôi được phép sửa chữa bất kỳ tập hợp con đường bị hỏng nào và chúng tôi phải giảm thiểu số lần thực hiện sửa chữa. 

Đồ thị có hướng, làm thay đổi hoàn toàn vấn đề. Một con đường từ`u`ĐẾN`v`chỉ giúp theo hướng đó. Dù có đường về cũng không giúp được thủ đô vào được thành phố. 

Đầu vào cung cấp cho mọi cạnh cùng với một lá cờ: 

-`0`có nghĩa là con đường đã hoạt động. 
-`1`nghĩa là đường bị hỏng và chỉ có thể sử dụng được nếu được sửa chữa. 

Đầu ra là: 

-`-1`nếu một số thành phố không thể truy cập được ngay cả sau khi sửa chữa mọi con đường bị hỏng, hoặc 
- số lượng đường được sửa chữa tối thiểu cùng với một bộ chỉ số cạnh hợp lệ. 

Các ràng buộc rất lớn: cả hai`n`Và`m`đang lên đến`10^5`. Bất kỳ thuật toán nào liên tục chạy quá trình duyệt đồ thị hoặc tính toán lại khả năng tiếp cận từ đầu sẽ không thành công. Ví dụ: không thể thử mọi tập hợp con của những con đường bị hỏng và thậm chí chạy BFS một lần trên mỗi đỉnh cũng đã trở nên quá chậm. 

Cần có thuật toán đồ thị tuyến tính hoặc gần tuyến tính. Có gì đó xung quanh`O(n + m)`là lý tưởng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét biểu đồ này:```
1 -> 2 (broken)
2 -> 3 (good)
```đầu vào:```
3 2
1 2 1
2 3 0
```Chỉ sửa chữa cạnh đầu tiên là đủ. Một giải pháp bất cẩn nhằm giảm thiểu việc sửa chữa riêng biệt cho từng thành phố có thể tính nhầm cùng một cạnh đã được sửa chữa nhiều lần. 

Một trường hợp nguy hiểm khác là các thành phần không thể truy cập được:```
4 2
1 2 0
3 4 1
```Thành phố`3`Và`4`bị ngắt kết nối với thủ đô ngay cả khi mọi con đường đều được sửa chữa. Câu trả lời đúng là:```
-1
```Một số triển khai chỉ tìm kiếm bằng cách sử dụng những con đường hiện đang tốt và kết luận sai rằng việc sửa chữa một cạnh ở đâu đó sẽ hữu ích. 

Chu kỳ cũng quan trọng:```
3 3
1 2 1
2 3 0
3 2 0
```Sửa chữa cạnh`1 -> 2`mở khóa cả hai`2`Và`3`. Câu trả lời tối ưu sẽ sửa chữa một cạnh chứ không phải hai. 

Cấu trúc của vấn đề mang tính toàn cầu. Chúng tôi không kết nối các thành phố một cách độc lập. Chúng tôi đang xây dựng một khu vực có thể tiếp cận trực tiếp từ thủ đô. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy coi mọi cạnh bị hỏng là tùy chọn, thử tất cả các tập hợp con của các cạnh bị hỏng và kiểm tra xem liệu tất cả các đỉnh có thể truy cập được từ thủ đô hay không. 

Nếu có`k`các cạnh bị hỏng, điều này đòi hỏi`2^k`tập hợp con. Chi phí kiểm tra khả năng tiếp cận`O(n + m)`mỗi lần. Thậm chí chỉ với`40`các cạnh bị gãy, nó trở nên lớn về mặt thiên văn. 

Một lực lượng vũ phu thông minh hơn một chút sẽ thử những con đường ngắn nhất một cách độc lập cho mọi thành phố. Chỉ định chi phí`0`để có lợi thế tốt và chi phí`1`đến các cạnh bị gãy. Sau đó tính toán số lần sửa chữa tối thiểu cần thiết để đạt đến mọi đỉnh. 

Điều này có hiệu quả đối với từng thành phố riêng lẻ nhưng không giải quyết được mục tiêu tối ưu hóa thực sự. Các thành phố khác nhau có thể chia sẻ các cạnh đã được sửa chữa. Tổng hợp chi phí đường đi ngắn nhất độc lập sẽ vượt quá chi phí sửa chữa. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến việc liệu mọi đỉnh có thể tiếp cận được hay không chứ không phải việc giảm thiểu từng đường dẫn riêng biệt. 

Giả sử chúng ta gán trọng số: 

-`0`đến các cạnh tốt, 
-`1`đến các cạnh bị gãy. 

Bây giờ hãy tưởng tượng việc xây dựng một cây đường đi ngắn nhất có gốc ở thủ đô, trong đó khoảng cách của một nút có nghĩa là số cạnh được sửa chữa tối thiểu cần thiết để tiếp cận nó. 

Nếu mọi nút đều có thể truy cập được trong biểu đồ có trọng số này thì việc chọn cạnh gốc được sử dụng trong cây đường đi ngắn nhất sẽ mang lại một tập hợp các con đường được sửa chữa tối ưu trên toàn cầu. 

Tại sao điều này làm việc? 

Mọi cạnh được sửa chữa trong giải pháp cuối cùng phải xuất hiện ở đâu đó trên con đường từ thủ đô. Cây đường đi ngắn nhất đảm bảo rằng mỗi nút được tiếp cận với số cạnh được sửa chữa tối thiểu có thể. Quan trọng hơn, sự kết hợp của tất cả các cạnh gốc tạo thành một dạng cây có định hướng. Bất kỳ cạnh nào được sửa chữa trong cây đó đều góp phần đạt được ít nhất một đỉnh mới. Không có sửa chữa là lãng phí. 

Vì trọng lượng cạnh chỉ`0`Và`1`, chúng ta có thể tính toán các đường đi ngắn nhất bằng cách sử dụng 0-1 BFS trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con đã được sửa chữa |$O(2^k \cdot (n+m))$|$O(n+m)$| Quá chậm | 
| Con đường ngắn nhất độc lập cho mỗi thành phố |$O(n(n+m))$|$O(n+m)$| Ý tưởng sai lầm | 
| 0-1 BFS + cây đường đi ngắn nhất |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng. 

Đối với mọi cạnh, lưu trữ: 

- đỉnh đích, 
- Chi phí sửa chữa (`0`hoặc`1`), 
- chỉ số cạnh gốc. 
2. Chạy 0-1 BFS từ thành phố`1`. 

Di chuyển qua một chi phí cạnh tốt`0`. 

Di chuyển qua một chi phí cạnh bị hỏng`1`. 

Bởi vì tất cả các trọng lượng đều`0`hoặc`1`, chúng ta có thể sử dụng deque: 

- Đẩy mạnh chi phí`0`, 
- đẩy lùi chi phí`1`. 

Điều này tính toán số cạnh được sửa chữa tối thiểu cần thiết để tiếp cận mọi thành phố. 
3. Trong khi nới lỏng các cạnh, hãy lưu trữ cạnh gốc đã tạo ra khoảng cách tốt nhất. 

Đối với mỗi đỉnh`v`, giữ:

-`parent[v]`= chỉ số cạnh được sử dụng để đạt được`v`, 
-`parent_cost[v]`= liệu cạnh đó có bị hỏng không. 
4. Nếu vẫn không thể truy cập được thành phố nào đó, hãy in`-1`. 

Điều này có nghĩa là không có đường dẫn có hướng nào tồn tại ngay cả sau khi sửa chữa mọi cạnh bị hỏng. 
5. Xây dựng lại bộ sửa chữa đã chọn. 

Mỗi đỉnh ngoại trừ đỉnh có chính xác một cạnh cha trong cây đường đi ngắn nhất. 

Đi qua tất cả các đỉnh: 

- nếu cạnh gốc bị hỏng, hãy thêm chỉ mục của nó vào câu trả lời. 
6. Xuất ra các chỉ số cạnh đã thu thập. 

Số cạnh như vậy là tối thiểu. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`dist[v]`bằng số lượng cạnh được sửa chữa tối thiểu cần thiết để đạt được`v`từ thủ đô. 

Các con trỏ cha xác định cây đường đi ngắn nhất. Mỗi đỉnh được kết nối với thủ đô thông qua một đường dẫn đạt được số lần sửa chữa tối thiểu này. 

Giả sử một giải pháp khác sửa chữa tổng thể ít đường hơn. Khi đó, cần phải đạt được ít nhất một đỉnh trong cây của chúng ta bằng cách sử dụng ít cạnh được sửa chữa hơn khoảng cách đường đi ngắn nhất của nó, điều này là không thể. 

Các cạnh được sửa chữa được chọn chính xác là các cạnh bị hỏng xuất hiện trong cây đường đi ngắn nhất. Việc loại bỏ bất kỳ một trong số chúng sẽ ngắt kết nối ít nhất một cây con khỏi chữ hoa. Mỗi sửa chữa được lựa chọn là cần thiết cho cấu trúc cây. 

Bởi vì cây đạt đến tất cả các đỉnh và sử dụng số cạnh được sửa chữa tối thiểu có thể dọc theo mọi đường dẫn từ gốc đến đỉnh nên tổng số đường được sửa chữa là tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for idx in range(1, m + 1):
        a, b, c = map(int, input().split())
        graph[a].append((b, c, idx))

    INF = 10**18

    dist = [INF] * (n + 1)
    parent = [-1] * (n + 1)
    parent_cost = [0] * (n + 1)

    dist[1] = 0

    dq = deque([1])

    while dq:
        u = dq.popleft()

        for v, w, idx in graph[u]:
            nd = dist[u] + w

            if nd < dist[v]:
                dist[v] = nd
                parent[v] = idx
                parent_cost[v] = w

                if w == 0:
                    dq.appendleft(v)
                else:
                    dq.append(v)

    for v in range(1, n + 1):
        if dist[v] == INF:
            print(-1)
            return

    ans = []

    for v in range(2, n + 1):
        if parent_cost[v] == 1:
            ans.append(parent[v])

    if not ans:
        print(0)
    else:
        print(len(ans))
        print(*ans)

solve()
```Biểu đồ được lưu dưới dạng danh sách kề vì các ràng buộc quá lớn đối với ma trận kề. 

Phần trung tâm của việc triển khai là BFS 0-1. BFS thông thường chỉ hoạt động đối với các biểu đồ không có trọng số, trong khi Dijkstra sẽ hoạt động nhưng ở đây lại đắt đỏ một cách không cần thiết. Vì mỗi trọng lượng cạnh là`0`hoặc`1`, một deque mô phỏng Dijkstra theo thời gian tuyến tính. 

Khi sự thư giãn được cải thiện`dist[v]`, mã cũng cập nhật: 

- cạnh dùng để nhập`v`, 
- cạnh đó có cần sửa chữa hay không. 

Cây đường dẫn ngắn nhất được lưu trữ ngầm thông qua các con trỏ cha này. 

Một điểm tinh tế là chúng tôi chỉ cập nhật khi`nd < dist[v]`, không`<=`. Khi một đỉnh đã có khoảng cách tối ưu, việc thay thế nó bằng một đỉnh gốc khác có khoảng cách bằng nhau là không cần thiết. 

Một chi tiết quan trọng khác là phát hiện không thể truy cập được. Nếu một nút vẫn còn khoảng cách`INF`sau BFS, thì ngay cả việc sửa chữa tất cả các cạnh bị hỏng cũng không thể kết nối nó với thủ đô, vì không tồn tại đường dẫn có hướng nào cả. 

Cuối cùng, câu trả lời được xây dựng lại bằng cách quét các cạnh cha. Mỗi cạnh cha mẹ bị hỏng phải được sửa chữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 3 0
3 2 1
```| Bước | Nút hiện tại | Cạnh thư giãn | Khoảng Cách Mới | Phụ huynh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | - | dist[1] = 0 | - | 
| 1 | 1 | 1 → 3 (0) | dist[3] = 0 | cạnh 1 | 
| 2 | 3 | 3 → 2 (1) | phân phối [2] = 1 | cạnh 2 | 

Khoảng cách cuối cùng: 

| Đỉnh | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 0 | 

Cây đường đi ngắn nhất sử dụng cạnh`1`và cạnh`2`. Chỉ có cạnh`2`hỏng rồi nên phải sửa. 

Đầu ra:```
1
2
```Dấu vết này cho thấy các cạnh có chi phí bằng 0 lan truyền ngay lập tức qua deque trong khi những con đường bị hỏng đóng góp một chi phí sửa chữa. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 1
1 3 0
3 2 0
2 4 1
```| Bước | Nút hiện tại | Cạnh thư giãn | Khoảng Cách Mới | Phụ huynh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | - | dist[1] = 0 | - | 
| 1 | 1 | 1 → 2 (1) | phân phối [2] = 1 | cạnh 1 | 
| 2 | 1 | 1 → 3 (0) | dist[3] = 0 | cạnh 2 | 
| 3 | 3 | 3 → 2 (0) | dist[2] = 0 | cạnh 3 | 
| 4 | 2 | 2 → 4 (1) | quận [4] = 1 | cạnh 4 | 

Các cạnh cha cuối cùng: 

- đỉnh 2 sử dụng cạnh 3, 
- đỉnh 3 sử dụng cạnh 2, 
- đỉnh 4 sử dụng cạnh 4. 

Chỉ có cạnh`4`bị phá vỡ bên trong cây đường đi ngắn nhất. 

Đầu ra:```
1
4
```Ví dụ này chứng minh tại sao các đường đi ngắn nhất độc lập lại nguy hiểm. Một giải pháp ngây thơ có thể sửa chữa Edge`1 -> 2`, nhưng thuật toán đã phát hiện chính xác một con đường thay thế miễn phí xuyên qua thành phố`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| Mỗi cạnh được xử lý một số lần không đổi trong 0-1 BFS | 
| Không gian |$O(n+m)$| Danh sách kề, mảng khoảng cách và mảng cha mẹ | 

Với`10^5`đỉnh và cạnh, độ phức tạp tuyến tính dễ dàng nằm gọn trong giới hạn. Tất cả các hoạt động deque đều có thời gian không đổi và mức sử dụng bộ nhớ tỷ lệ thuận với kích thước biểu đồ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for idx in range(1, m + 1):
        a, b, c = map(int, input().split())
        graph[a].append((b, c, idx))

    INF = 10**18

    dist = [INF] * (n + 1)
    parent = [-1] * (n + 1)
    parent_cost = [0] * (n + 1)

    dist[1] = 0

    dq = deque([1])

    while dq:
        u = dq.popleft()

        for v, w, idx in graph[u]:
            nd = dist[u] + w

            if nd < dist[v]:
                dist[v] = nd
                parent[v] = idx
                parent_cost[v] = w

                if w == 0:
                    dq.appendleft(v)
                else:
                    dq.append(v)

    for v in range(1, n + 1):
        if dist[v] == INF:
            print(-1)
            return out.getvalue()

    ans = []

    for v in range(2, n + 1):
        if parent_cost[v] == 1:
            ans.append(parent[v])

    if not ans:
        print(0)
    else:
        print(len(ans))
        print(*ans)

    return out.getvalue()

# provided sample
assert run(
    "3 2\n"
    "1 3 0\n"
    "3 2 1\n"
) == "1\n2\n"

# minimum size
assert run(
    "1 1\n"
    "1 1 0\n"
) == "0\n"

# unreachable component
assert run(
    "4 2\n"
    "1 2 0\n"
    "3 4 1\n"
) == "-1\n"

# all good roads
assert run(
    "3 2\n"
    "1 2 0\n"
    "2 3 0\n"
) == "0\n"

# alternative cheaper path
assert run(
    "4 4\n"
    "1 2 1\n"
    "1 3 0\n"
    "3 2 0\n"
    "2 4 1\n"
) == "1\n4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị một đỉnh |`0`| Vốn đã đến tất cả các thành phố | 
| Đồ thị bị ngắt kết nối |`-1`| Phát hiện khả năng tiếp cận không thể | 
| Mọi con đường đều tốt |`0`| Không cần sửa chữa không cần thiết | 
| Con đường miễn phí thay thế | Chỉ sửa cạnh 4 | Xác nhận logic cây đường dẫn ngắn nhất | 
| Trường hợp mẫu | Sửa cạnh 2 | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
4 2
1 2 0
3 4 1
```BFS bắt đầu từ thành phố`1`và chỉ đến thành phố`2`. Thành phố`3`Và`4`vẫn ở khoảng cách`INF`. 

Thuật toán in ngay lập tức:```
-1
```Điều này đúng vì không tồn tại đường dẫn trực tiếp từ thủ đô, ngay cả khi mọi con đường bị hỏng đều được sửa chữa. 

Bây giờ hãy xem xét việc sửa chữa chung:```
3 2
1 2 1
2 3 0
```Khoảng cách trở thành: 

-`dist[2] = 1`-`dist[3] = 1`Cả hai thành phố đều phụ thuộc vào cùng một cạnh được sửa chữa`1 -> 2`. Cây đường đi ngắn nhất chỉ chứa một cạnh bị gãy nên câu trả lời là:```
1
1
```Cách tiếp cận ngây thơ theo từng thành phố có thể tính sai hai lần sửa chữa. 

Cuối cùng, hãy xem xét chu kỳ:```
3 3
1 2 1
2 3 0
3 2 0
```Cạnh sửa chữa BFS`1 -> 2`một lần, sau đó là chu kỳ giữa`2`Và`3`đã có thể sử dụng được qua những con đường tốt. 

Các đầu ra thuật toán:```
1
1
```Chu trình không gây nhầm lẫn cho việc xây dựng cây cha vì mỗi đỉnh chỉ giữ cạnh đầu vào được biết đến tốt nhất.
