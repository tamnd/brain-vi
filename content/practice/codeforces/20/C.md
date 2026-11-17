---
title: "CF 20C - Dijkstra?"
description: "Chúng ta được cho một đồ thị có trọng số vô hướng. Mỗi cạnh nối hai đỉnh và có chi phí dương. Nhiệm vụ là bắt đầu từ đỉnh 1, đến đỉnh n và in ra một đường đi ngắn nhất. Nếu không có tuyến đường nào tồn tại, chúng tôi in -1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 20
codeforces_index: "C"
codeforces_contest_name: "Codeforces Alpha Round 20 (Codeforces format)"
rating: 1900
weight: 20
solve_time_s: 96
verified: true
draft: false
---
[CF 20C - Dijkstra?](https://codeforces.com/problemset/problem/20/C) 

**Xếp hạng:** 1900 
**Tags:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có trọng số vô hướng. Mỗi cạnh nối hai đỉnh và có chi phí dương. Nhiệm vụ là bắt đầu ở đỉnh`1`, đạt tới đỉnh`n`và in ra một đường đi ngắn nhất. Nếu không có tuyến đường nào tồn tại, chúng tôi sẽ in`-1`. 

Đồ thị đủ lớn để chúng ta không thể khám phá tất cả các đường đi có thể. Với tối đa`10^5`đỉnh và`10^5`các cạnh, bất kỳ thuật toán nào gần`O(n^2)`Trong Python vốn đã có rủi ro và bất cứ điều gì theo cấp số nhân đều là không thể. Biểu đồ thưa thớt, vì số cạnh có cùng thứ tự với số đỉnh, điều này gợi ý rõ ràng về danh sách kề thay vì ma trận kề. 

Các trọng số của cạnh đều dương. Chi tiết đó thay đổi hoàn toàn vấn đề. Các trọng số dương cho phép thuật toán Dijkstra hoạt động, bởi vì một khi chúng ta đã xác định được khoảng cách ngắn nhất tới một nút thì không có đường dẫn nào trong tương lai có thể cải thiện được khoảng cách đó. 

Vấn đề cũng yêu cầu đường dẫn thực tế chứ không chỉ khoảng cách tối thiểu. Điều đó có nghĩa là chúng ta phải nhớ cách chúng ta đạt đến từng đỉnh trong quá trình thư giãn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Một biểu đồ có thể bị ngắt kết nối. 

đầu vào:```
3 1
1 2 5
```Không có con đường từ`1`ĐẾN`3`, vì vậy kết quả đúng là:```
-1
```Việc triển khai bất cẩn vẫn có thể cố gắng xây dựng lại đường dẫn và gặp sự cố vì không có nút cha nào tồn tại cho nút`3`. 

Đồ thị có thể chứa nhiều cạnh giữa cùng một cặp đỉnh. 

đầu vào:```
3 3
1 2 10
1 2 1
2 3 2
```Con đường ngắn nhất là:```
1 2 3
```với tổng chi phí`3`. Nếu chúng ta ghi đè lên các cạnh hoặc giả sử tính duy nhất, chúng ta có thể vô tình giữ lại cạnh nặng hơn. 

Biểu đồ có thể chứa các vòng lặp tự. 

đầu vào:```
2 2
1 1 5
1 2 3
```Vòng lặp trên nút`1`đơn giản là nên bị bỏ qua một cách tự nhiên bởi quá trình thư giãn. Việc triển khai có lỗi có thể vô tình tiếp tục làm thư giãn cùng một nút mãi mãi nếu trạng thái cũ không được xử lý chính xác. 

Con đường ngắn nhất có thể liên quan đến khoảng cách rất lớn. Trọng lượng cạnh lên tới`10^6`và các đường dẫn có thể chứa nhiều cạnh, do đó tổng khoảng cách có thể vượt quá phạm vi số nguyên 32 bit. Số nguyên Python xử lý việc này một cách tự động, nhưng trong các ngôn ngữ như C++, chúng ta sẽ cần`long long`. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liệt kê tất cả các con đường có thể từ`1`ĐẾN`n`, tính chi phí của chúng và giữ ở mức tối thiểu. Điều này hoạt động hợp lý vì mọi tuyến đường hợp lệ đều được kiểm tra, do đó không thể bỏ qua tuyến đường tối ưu. 

Vấn đề là số lượng đường dẫn đơn giản trong biểu đồ có thể tăng theo cấp số nhân. Ngay cả một biểu đồ được kết nối vừa phải cũng có thể chứa hàng triệu hoặc hàng tỷ đường dẫn. Với`10^5`đỉnh, vũ lực là hoàn toàn không khả thi. 

Một cách tiếp cận ngây thơ tốt hơn một chút là sử dụng liên tục sự thư giãn theo kiểu BFS, tương tự như Bellman-Ford. Bellman-Ford hoạt động chính xác ngay cả với trọng số âm, bởi vì nó liên tục cải thiện khoảng cách cho đến khi không còn đường đi ngắn hơn. Độ phức tạp của nó là`O(n * m)`. Ở đây điều đó trở nên đại khái`10^10`hoạt động trong trường hợp xấu nhất, vượt xa thời hạn. 

Quan sát quan trọng là mọi trọng số của cạnh đều dương. Các trọng số dương tạo cho các đường đi ngắn nhất một cấu trúc đơn điệu: một khi chúng ta rút ra khoảng cách dự kiến ​​nhỏ nhất hiện tại thì không có đường đi nào trong tương lai có thể cải thiện được khoảng cách đó. Thuộc tính đó chính xác là những gì thuật toán của Dijkstra dựa vào. 

Dijkstra duy trì khoảng cách được biết đến tốt nhất đến mọi nút và luôn mở rộng nút chưa được xử lý gần nhất trước tiên. Việc sử dụng hàng đợi ưu tiên sẽ giúp việc trích xuất được hiệu quả. Vì biểu đồ thưa thớt nên danh sách kề cộng với một đống sẽ phức tạp`O((n + m) log n)`, dễ dàng phù hợp với các ràng buộc. 

Chúng tôi cũng lưu trữ một`parent`mảng. Bất cứ khi nào chúng tôi cải thiện khoảng cách ngắn nhất tới một nút, chúng tôi sẽ ghi lại nút nào trước đó đã tạo ra sự cải thiện đó. Sau khi Dijkstra kết thúc, chúng ta xây dựng lại đường đi bằng cách đi lùi từ`n`ĐẾN`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê đường dẫn vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Bellman-Ford | O(nm) | O(n) | Quá chậm | 
| Dijkstra với Heap | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. 

Vì đồ thị là vô hướng nên mọi cạnh`(u, v, w)`phải được thêm vào theo cả hai hướng. 
2. Tạo một mảng khoảng cách được khởi tạo bằng vô cùng.`dist[x]`lưu trữ khoảng cách ngắn nhất hiện được biết từ nút`1`đến nút`x`. Ban đầu chỉ`dist[1] = 0`. 
3. Tạo một mảng cha được khởi tạo bằng`-1`.`parent[x]`nhớ nút trước đó trên đường đi ngắn nhất tới`x`. Điều này cho phép xây dựng lại đường dẫn sau này. 
4. Đẩy`(0, 1)`thành một đống tối thiểu. 

Heap luôn cho chúng ta nút có khoảng cách dự kiến ​​nhỏ nhất. 
5. Liên tục bật trạng thái nhỏ nhất ra khỏi heap. 

Giả sử chúng ta bật`(d, u)`. Nếu như`d`không bằng`dist[u]`, trạng thái này đã cũ và nên được bỏ qua. 

Nhiều bản sao của cùng một nút có thể tồn tại trong heap vì tuyến đường ngắn hơn có thể được phát hiện sau đó. 
6. Thư giãn mọi cạnh rời`u`. 

Đối với mỗi người hàng xóm`(v, w)`, kiểm tra xem:```
dist[u] + w < dist[v]
```Nếu đúng, chúng tôi đã tìm thấy một con đường ngắn hơn để`v`. 
7. Cập nhật khoảng cách và mảng cha. 

Bộ:```
dist[v] = dist[u] + w
parent[v] = u
```Sau đó đẩy trạng thái cập nhật vào heap. 
8. Sau khi heap trống, hãy kiểm tra`dist[n]`. 

Nếu nó vẫn là vô cùng, nút`n`không thể truy cập được và chúng tôi in`-1`. 
9. Xây dựng lại đường dẫn. 

Bắt đầu từ nút`n`và liên tục di chuyển đến`parent[cur]`cho đến khi đạt được`1`. Đảo ngược trình tự đã thu thập trước khi in. 

### Tại sao nó hoạt động 

Thuật toán Dijkstra dựa trên thực tế là tất cả các trọng số của cạnh đều dương. Khi heap trích xuất một nút`u`với khoảng cách dự kiến ​​​​nhỏ nhất, mọi đường đi chưa được xử lý khác ít nhất phải đắt bằng nhau. Bất kỳ tuyến đường thay thế nào đạt được`u`sau đó sẽ cần phải đi qua một nút khác có khoảng cách không nhỏ hơn và sau đó thêm trọng số cạnh dương nên không thể cải thiện`dist[u]`. 

Do thuộc tính đó, khi một nút được xử lý với khoảng cách tốt nhất hiện tại, giá trị đó là giá trị cuối cùng. 

Mảng cha luôn ghi lại cạnh tạo ra khoảng cách được biết rõ nhất. Vì khoảng cách chỉ cải thiện theo hướng có giá trị ngắn nhất thực sự nên chuỗi gốc cuối cùng sẽ xây dựng lại đường đi ngắn nhất hợp lệ. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

n, m = map(int, input().split())

graph = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v, w = map(int, input().split())
    graph[u].append((v, w))
    graph[v].append((u, w))

dist = [INF] * (n + 1)
parent = [-1] * (n + 1)

dist[1] = 0

pq = [(0, 1)]

while pq:
    d, u = heapq.heappop(pq)

    if d != dist[u]:
        continue

    for v, w in graph[u]:
        nd = d + w

        if nd < dist[v]:
            dist[v] = nd
            parent[v] = u
            heapq.heappush(pq, (nd, v))

if dist[n] == INF:
    print(-1)
else:
    path = []

    cur = n

    while cur != -1:
        path.append(cur)
        cur = parent[cur]

    path.reverse()

    print(*path)
```Danh sách kề giữ cho việc sử dụng bộ nhớ tuyến tính theo số cạnh. Với tối đa`10^5`các cạnh, điều này hiệu quả hơn nhiều so với ma trận, đòi hỏi`10^10`tế bào. 

Heap lưu trữ các cặp`(distance, node)`. của Python`heapq`thực hiện một vùng heap tối thiểu, do đó khoảng cách nhỏ nhất luôn được xử lý trước tiên. 

Kiểm tra trạng thái cũ:```
if d != dist[u]:
    continue
```là điều cần thiết. Giả sử nút`u`trước đây đã được đẩy với khoảng cách`10`, và sau đó chúng tôi tìm thấy một con đường tốt hơn với khoảng cách`5`. Cả hai trạng thái vẫn ở trong heap. Khi đã lỗi thời`(10, u)`cuối cùng xuất hiện, chúng ta phải bỏ qua nó. Nếu không có sự kiểm tra này, thuật toán vẫn đúng nhưng thực hiện các bước thư giãn không cần thiết và có thể trở nên quá chậm. 

Bản cập nhật gốc diễn ra chính xác khi một đường dẫn ngắn hơn được phát hiện. Điều đó đảm bảo chuỗi được xây dựng lại tương ứng với khoảng cách ngắn nhất cuối cùng. 

Quá trình xây dựng lại đường dẫn đi lùi từ`n`theo hướng`1`. Vì cha mẹ trỏ theo thứ tự ngược lại nên chúng tôi đảo ngược mảng đã thu thập trước khi in. 

Sử dụng một số nguyên lớn như`10**30`vì vô cực là an toàn vì khoảng cách ngắn nhất tối đa có thể nhỏ hơn thế nhiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
1 2 2
2 5 5
2 3 4
1 4 1
4 3 3
3 5 1
```| Đống Pop | Cạnh thư giãn | Khoảng cách được cập nhật | Thay đổi phụ huynh | 
| --- | --- | --- | --- | 
| (0, 1) | 1 → 2 (2) | dist[2] = 2 | cha mẹ[2] = 1 | 
| (0, 1) | 1 → 4 (1) | quận [4] = 1 | cha mẹ[4] = 1 | 
| (1, 4) | 4 → 3 (3) | phân phối [3] = 4 | cha mẹ[3] = 4 | 
| (2, 2) | 2 → 5 (5) | dist[5] = 7 | cha mẹ[5] = 2 | 
| (4, 3) | 3 → 5 (1) | phân phối [5] = 5 | cha mẹ[5] = 3 | 
| (5, 5) | không | cuối cùng | cuối cùng | 

Chuỗi cha mẹ cuối cùng:```
5 ← 3 ← 4 ← 1
```Đường đi ngược lại:```
1 4 3 5
```Dấu vết này cho thấy khoảng cách của một nút có thể được cải thiện nhiều lần như thế nào. nút`5`đầu tiên nhận được khoảng cách`7`, sau đó cải thiện thành`5`. 

### Ví dụ 2 

đầu vào:```
4 2
1 2 5
3 4 1
```| Đống Pop | Cạnh thư giãn | Khoảng cách được cập nhật | 
| --- | --- | --- | 
| (0, 1) | 1 → 2 (5) | quận [2] = 5 | 
| (5, 2) | không | không | 

Nút`3`Và`4`không bao giờ đạt được. 

Khoảng cách cuối cùng:```
dist = [∞, 0, 5, ∞, ∞]
```Từ`dist[4]`vẫn là vô cùng, thuật toán in:```
-1
```Ví dụ này thể hiện cách xử lý chính xác các biểu đồ bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi lần nới lỏng cạnh có thể bị đẩy vào vùng heap và các thao tác trên vùng heap tốn thời gian logarit | 
| Không gian | O(n + m) | Danh sách kề, mảng khoảng cách, mảng cha và bộ nhớ heap | 

Với`10^5`đỉnh và cạnh, độ phức tạp này hoàn toàn có thể chấp nhận được. Hoạt động của heap vẫn hiệu quả và mức sử dụng bộ nhớ tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    INF = 10**30

    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))

    dist = [INF] * (n + 1)
    parent = [-1] * (n + 1)

    dist[1] = 0

    pq = [(0, 1)]

    while pq:
        d, u = heapq.heappop(pq)

        if d != dist[u]:
            continue

        for v, w in graph[u]:
            nd = d + w

            if nd < dist[v]:
                dist[v] = nd
                parent[v] = u
                heapq.heappush(pq, (nd, v))

    if dist[n] == INF:
        return "-1"

    path = []

    cur = n

    while cur != -1:
        path.append(cur)
        cur = parent[cur]

    path.reverse()

    return " ".join(map(str, path))

# provided sample
assert run(
"""5 6
1 2 2
2 5 5
2 3 4
1 4 1
4 3 3
3 5 1
"""
) == "1 4 3 5", "sample 1"

# disconnected graph
assert run(
"""3 1
1 2 5
"""
) == "-1", "disconnected graph"

# multiple edges
assert run(
"""3 3
1 2 10
1 2 1
2 3 2
"""
) == "1 2 3", "multiple edges"

# self-loop
assert run(
"""2 2
1 1 5
1 2 3
"""
) == "1 2", "self loop handling"

# minimum graph
assert run(
"""2 1
1 2 1
"""
) == "1 2", "minimum valid graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị bị ngắt kết nối |`-1`| Xử lý đúng cách không thể truy cập | 
| Nhiều cạnh giữa các nút giống nhau |`1 2 3`| Chọn cạnh có trọng lượng tối thiểu | 
| Hiện tại tự lặp |`1 2`| Vòng lặp không phá vỡ sự thư giãn | 
| Biểu đồ hợp lệ tối thiểu |`1 2`| Độ chính xác kích thước ranh giới | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
3 1
1 2 5
```Thuật toán bắt đầu từ nút`1`, thư giãn nút`2`, và sau đó heap trở nên trống rỗng. nút`3`không bao giờ đạt được, vì vậy:```
dist[3] = INF
```Kiểm tra cuối cùng phát hiện điều này và in:```
-1
```Không có nỗ lực xây dựng lại đường dẫn nào, do đó không có quyền truy cập chính không hợp lệ. 

Bây giờ hãy xem xét nhiều cạnh:```
3 3
1 2 10
1 2 1
2 3 2
```Khi xử lý nút`1`, cả hai cạnh tới nút`2`được kiểm tra. Đầu tiên cho khoảng cách`10`, nhưng cái thứ hai cải thiện nó thành`1`. 

Khoảng cách cuối cùng trở thành:```
dist[2] = 1
dist[3] = 3
```Chuỗi gốc được xây dựng lại:```
1 → 2 → 3
```Thuật toán xử lý các cạnh song song một cách tự nhiên vì mọi cạnh đều được nới lỏng một cách độc lập. 

Cuối cùng, hãy xem xét một vòng lặp tự:```
2 2
1 1 5
1 2 3
```Khi nút`1`được xử lý, cạnh vòng lặp đề xuất:```
0 + 5 = 5
```cho nút`1`chính nó. Từ`dist[1]`đã rồi`0`, không có cập nhật nào xảy ra. 

Cạnh bình thường tới nút`2`mang lại:```
dist[2] = 3
```Đầu ra trở thành:```
1 2
```Vòng lặp tự không có tác dụng, chính xác như mong đợi.
