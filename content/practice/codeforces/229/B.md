---
title: "CF 229B - Các hành tinh"
description: "Chúng ta được cung cấp một biểu đồ có trọng số vô hướng của các hành tinh được kết nối bằng các cổng sao. Jack bắt đầu ở hành tinh 1 vào thời điểm 0 và muốn đến hành tinh n càng sớm càng tốt. Di chuyển qua một cổng sao cần một khoảng thời gian cố định. Phần bất thường là quy tắc chờ đợi."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 229
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 142 (Div. 1)"
rating: 1700
weight: 229
solve_time_s: 97
verified: true
draft: false
---

[CF 229B - Các hành tinh](https://codeforces.com/problemset/problem/229/B) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số vô hướng của các hành tinh được kết nối bằng các cổng sao. Jack bắt đầu trên hành tinh`1`vào thời điểm đó`0`và muốn đến hành tinh`n`càng sớm càng tốt. 

Di chuyển qua một cổng sao cần một khoảng thời gian cố định. Phần bất thường là quy tắc chờ đợi. Mỗi hành tinh đều có một tập hợp các khoảnh khắc bị chặn. Nếu Jack đang đứng trên một hành tinh vào thời điểm đó`t`, Và`t`thuộc tập hợp bị chặn của hành tinh đó, anh ta bị cấm rời đi ngay lập tức. Anh phải đợi đến lần đầu tiên mà không bị chặn. 

Hạn chế chỉ áp dụng khi khởi hành từ một hành tinh. Được phép đến thời điểm bị chặn. Vấn đề yêu cầu thời gian đến hành tinh tối thiểu`n`, hoặc`-1`nếu không có tuyến đường tồn tại. 

Đồ thị có tới`10^5`đỉnh và`10^5`các cạnh, do đó, bất kỳ thuật toán nào tệ hơn đại khái`O((n + m) log n)`có nhiều rủi ro trong Python. Ở đây không thể thực hiện được thuật toán đường đi ngắn nhất cho tất cả các cặp tiêu chuẩn. Thậm chí`O(nm)`có nghĩa là xung quanh`10^10`hoạt động. 

Tổng số lần bị chặn trên tất cả các hành tinh cũng bị giới hạn bởi`10^5`. Chi tiết đó quan trọng vì nó có nghĩa là chúng tôi có thể xử lý trước hoặc tìm kiếm bên trong các danh sách này một cách hiệu quả. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Một trường hợp tế nhị xảy ra ở hành tinh khởi đầu. Jack bắt đầu vào lúc`0`, nhưng nếu hành tinh`1`bị chặn vào thời điểm đó`0`, anh ấy không thể rời đi ngay lập tức. 

Ví dụ:```
2 1
1 2 5
1 0
0
```Câu trả lời đúng là`6`, không`5`, vì Jack phải đợi một giây trước khi khởi hành. 

Một tình huống khó khăn khác là số lần bị chặn liên tiếp.```
2 1
1 2 3
3 0 1 2
0
```Jack bắt đầu vào lúc`0`, nhưng lần`0`,`1`, Và`2`tất cả đều bị chặn trên hành tinh`1`. Anh ấy chỉ có thể rời đi vào lúc`3`, vậy câu trả lời là`6`. 

Việc triển khai bất cẩn chỉ kiểm tra một thời điểm bị chặn sẽ tạo ra sai sót`4`. 

Đồ thị bị ngắt kết nối là một nguồn sai lầm khác.```
3 1
1 2 5
0
0
0
```Hành tinh`3`không thể truy cập được, vì vậy câu trả lời phải là`-1`. 

Cuối cùng, việc đến vào thời điểm bị chặn là hoàn toàn hợp pháp. Hạn chế chỉ áp dụng khi cố gắng khởi hành.```
2 1
1 2 3
0
1 3
```Câu trả lời vẫn là`3`. Jack đến hành tinh`2`chính xác khi nào một du khách khác đến đó, nhưng điều đó không thành vấn đề vì cuộc hành trình kết thúc. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy đối xử với mọi trạng thái như`(planet, current_time)`. Từ một trạng thái, hãy đợi một giây hoặc đi qua một cạnh nếu thời gian hiện tại không bị chặn. 

Điều này hiệu quả vì tất cả các chuyển đổi đều được mô hình hóa rõ ràng. Vấn đề là kích thước của chiều thời gian. Trọng lượng cạnh có thể tích lũy vượt xa`10^9`, và việc khám phá tất cả các thời gian trung gian là không thể. 

Một cách tiếp cận vũ phu tinh tế hơn sẽ vẫn sử dụng thuật toán của Dijkstra, nhưng bất cứ khi nào chúng ta đến một hành tinh vào thời điểm đó`t`, chúng tôi liên tục tăng`t`trong khi nó vẫn bị chặn. 

Điều đó đã tốt hơn nhiều vì chúng tôi không còn lập mô hình từng giây trên toàn cầu nữa. Vấn đề còn lại là hiệu quả. Giả sử một hành tinh có thời gian bị chặn:```
0 1 2 3 4 ... 100000
```Nếu nhiều lần thư giãn đường đi ngắn nhất liên tục quét từng giá trị này thì độ phức tạp tổng thể có thể trở thành bậc hai. 

Quan sát quan trọng là hành vi chờ đợi có tính quyết định. Một khi chúng ta biết thời gian đến`t`tại một hành tinh có đúng một thời điểm khởi hành hợp lệ sớm nhất. Vì các khoảnh khắc bị chặn đã được sắp xếp nên chúng tôi có thể chuyển thẳng đến thời gian khởi hành đó thay vì tăng dần một cách mù quáng. 

Điều này biến bài toán trở lại bài toán đường đi ngắn nhất tiêu chuẩn với độ giãn cạnh được sửa đổi. 

Đối với mọi hành tinh`u`, định nghĩa:```
next_free(u, t)
```như lần sớm nhất`>= t`cái đó không bị chặn`u`. 

Sau đó, mọi sự thư giãn của cạnh sẽ trở thành:```
departure = next_free(u, dist[u])
arrival = departure + weight
```Đồ thị vẫn có chi phí cạnh không âm, do đó Dijkstra vẫn hợp lệ. 

Để tính toán`next_free`một cách hiệu quả, chúng tôi lưu trữ từng danh sách bị chặn trong một tập hợp băm cho`O(1)`kiểm tra thành viên. Vì mỗi khoảnh khắc bị chặn được truy cập nhiều nhất một lần trong tất cả các lần tăng dần, nên tổng công việc vẫn tuyến tính trong tổng số lần bị chặn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo các trạng thái thời gian | Không thể | Không thể | Quá chậm | 
| Dijkstra với những lần quét ngây thơ chờ đợi | Có khả năng O(K²) | O(n + m + K) | Quá chậm | 
| Dijkstra được tối ưu hóa | O((n + m) log n + K) | O(n + m + K) | Đã chấp nhận | 

Đây`K`là tổng số khoảnh khắc bị chặn. 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. 

Mỗi cạnh lưu trữ hành tinh lân cận và thời gian di chuyển. 
2. Lưu trữ những khoảnh khắc bị chặn của mọi hành tinh trong bộ băm. 

Việc kiểm tra tư cách thành viên nhanh chóng là rất quan trọng vì chúng tôi liên tục hỏi liệu một thời gian nhất định có bị chặn hay không. 
3. Tính toán trước thời gian khởi hành hợp lệ sớm nhất cho mỗi thời điểm bị chặn. 

Giả sử một hành tinh có những khoảnh khắc bị chặn:```
3 4 5 8 9
```Nếu Jack đến đúng lúc`3`, anh ấy thực sự phải đợi cho đến khi`6`. 

Nếu anh ấy đến`4`, câu trả lời vẫn là`6`. 

Chúng tôi xử lý trước việc này một cách hiệu quả bằng cách quét ngược từng danh sách bị chặn. 
4. Chạy thuật toán Dijkstra từ hành tinh`1`. 

Các cửa hàng xếp hàng ưu tiên`(current_time, node)`. 
5. Khi xử lý một nút`u`vào thời điểm đó`t`, xác định thời gian khởi hành thực sự. 

Nếu như`t`không bị chặn, thời gian khởi hành vẫn giữ nguyên`t`. 

Nếu không, hãy chuyển thẳng đến thời điểm không bị chặn đầu tiên sau đoạn bị chặn liên tiếp. 
6. Thư giãn tất cả các cạnh hướng ra ngoài. 

Đối với một cạnh`(u, v, w)`:```
new_time = departure_time + w
```Nếu điều này được cải thiện`dist[v]`, đẩy trạng thái mới vào heap. 
7. Sau khi Dijkstra kết thúc, hãy in`dist[n]`. 

Nếu nó vẫn là vô hạn, hãy in`-1`. 

### Tại sao nó hoạt động 

Thuật toán của Dijkstra dựa trên một thuộc tính: khi một nút được trích xuất với khoảng cách tối thiểu, thì sau này không thể tồn tại tuyến đường ngắn hơn tới nút đó. 

Quy tắc chờ không vi phạm tính chất này vì việc chờ chỉ làm tăng thời gian chứ không bao giờ giảm thời gian. Mỗi lần truyền tải cạnh thực sự có chi phí động không âm:```
effective_cost = waiting_time + edge_weight
```Bước tiền xử lý đảm bảo rằng với mỗi thời gian đến, chúng tôi tính toán chính xác thời gian khởi hành hợp pháp sớm nhất. Vì mọi sự thư giãn đều sử dụng điểm khởi hành tối thiểu thực sự nên tất cả các đường dẫn ứng cử viên đều được đánh giá chính xác. 

Bởi vì tất cả các chi phí chuyển đổi hiệu quả đều không âm nên việc chứng minh tính đúng đắn của Dijkstra tiêu chuẩn vẫn được áp dụng. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b, c = map(int, input().split())
        graph[a].append((b, c))
        graph[b].append((a, c))

    nxt = [{} for _ in range(n + 1)]

    for i in range(1, n + 1):
        arr = list(map(int, input().split()))

        k = arr[0]
        times = arr[1:]

        if k == 0:
            continue

        jump = {}

        last = times[-1]
        jump[last] = last + 1

        for j in range(k - 2, -1, -1):
            cur = times[j]
            nxt_time = times[j + 1]

            if nxt_time == cur + 1:
                jump[cur] = jump[nxt_time]
            else:
                jump[cur] = cur + 1

        nxt[i] = jump

    dist = [INF] * (n + 1)
    dist[1] = 0

    pq = [(0, 1)]

    while pq:
        t, u = heapq.heappop(pq)

        if t != dist[u]:
            continue

        depart = nxt[u].get(t, t)

        for v, w in graph[u]:
            nt = depart + w

            if nt < dist[v]:
                dist[v] = nt
                heapq.heappush(pq, (nt, v))

    print(-1 if dist[n] == INF else dist[n])

solve()
```Danh sách lân cận là cơ sở hạ tầng Dijkstra tiêu chuẩn. Vì biểu đồ thưa thớt nên cách biểu diễn này giữ cho bộ nhớ và phép lặp hiệu quả. 

Bước tiền xử lý là thủ thuật cốt lõi. Đối với mỗi thời gian bị chặn`t`, chúng tôi lưu trữ thời gian hợp lệ đầu tiên một cách nghiêm ngặt sau phân đoạn bị chặn liên tiếp chứa`t`. 

Ví dụ:```
blocked = [3, 4, 5, 8]
```trở thành:```
3 -> 6
4 -> 6
5 -> 6
8 -> 9
```Điều này cho phép thuật toán tính toán thời gian chờ trong thời gian dự kiến ​​không đổi. 

Việc quét ngược có vấn đề. Khi xử lý`4`, chúng ta đã biết câu trả lời cho`5`. Nếu như`5`là liên tiếp thì`4`kế thừa thời gian thoát cuối cùng giống nhau. 

Bên trong Dijkstra, dòng:```
depart = nxt[u].get(t, t)
```có nghĩa:```
If t is blocked, jump forward.
Otherwise leave immediately.
```Một lỗi phổ biến là tính toán lại thời gian chờ sau khi đi qua một cạnh. Quy tắc chờ đợi áp dụng trước khi rời khỏi một hành tinh chứ không phải sau khi đến. 

Một điểm tinh tế khác là các mục nhập cũ. Dijkstra có thể đẩy nhiều phiên bản của cùng một nút vào hàng ưu tiên. Việc kiểm tra:```
if t != dist[u]:
    continue
```loại bỏ các trạng thái lỗi thời một cách an toàn. 

Số nguyên Python có độ chính xác tùy ý, vì vậy tổng đường dẫn lớn không phải là vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 6
1 2 2
1 3 3
1 4 8
2 3 4
2 4 5
3 4 3
0
1 3
2 3 4
0
```Khoảnh khắc bị chặn:```
Planet 1: none
Planet 2: {3}
Planet 3: {3, 4}
Planet 4: none
```Các bước nhảy được xử lý trước:```
Planet 2: 3 -> 4
Planet 3: 3 -> 5, 4 -> 5
```| Bước | Đống Pop | Giờ khởi hành | Cạnh thư giãn | Khoảng Cách Mới | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | 0 | 1→2 | 2 | 
| 1 | (0,1) | 0 | 1→3 | 3 | 
| 1 | (0,1) | 0 | 1→4 | 8 | 
| 2 | (2,2) | 2 | 2→4 | 7 | 
| 3 | (3,3) | 5 | 3→4 | 8 | 
| 4 | (7,4) | 7 | không | cuối cùng | 

Con đường ngắn nhất là:```
1 → 2 → 4
```với tổng thời gian`7`. 

Dấu vết cho thấy tại sao phải chờ đợi trước khi rời khỏi nút. Tiếp cận hành tinh`3`vào thời điểm đó`3`buộc Jack phải đợi cho đến khi`5`. 

### Ví dụ về ngắt kết nối 

đầu vào:```
3 1
1 2 5
0
0
0
```| Bước | Đống Pop | Giờ khởi hành | Cạnh thư giãn | Khoảng Cách Mới | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | 0 | 1→2 | 5 | 
| 2 | (5,2) | 5 | không hữu ích | không | 

Hành tinh`3`không bao giờ đạt được. 

Câu trả lời cuối cùng:```
-1
```Điều này xác nhận rằng thuật toán xử lý các thành phần bị ngắt kết nối một cách tự nhiên thông qua mảng khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + K) | Dijkstra chiếm ưu thế, quét tiền xử lý bị chặn một lần | 
| Không gian | O(n + m + K) | Đồ thị, đống, mảng khoảng cách, bản đồ thời gian bị chặn |`n`,`m`và tổng số bị chặn`K`nhiều nhất là tất cả`10^5`, do đó thuật toán phù hợp thoải mái trong giới hạn. Các phép toán heap là logarit và mỗi thời gian bị chặn sẽ tham gia tiền xử lý chính xác một lần. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

INF = 10**30

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b, c = map(int, input().split())
        graph[a].append((b, c))
        graph[b].append((a, c))

    nxt = [{} for _ in range(n + 1)]

    for i in range(1, n + 1):
        arr = list(map(int, input().split()))

        k = arr[0]
        times = arr[1:]

        jump = {}

        if k:
            jump[times[-1]] = times[-1] + 1

            for j in range(k - 2, -1, -1):
                cur = times[j]

                if times[j + 1] == cur + 1:
                    jump[cur] = jump[times[j + 1]]
                else:
                    jump[cur] = cur + 1

        nxt[i] = jump

    dist = [INF] * (n + 1)
    dist[1] = 0

    pq = [(0, 1)]

    while pq:
        t, u = heapq.heappop(pq)

        if t != dist[u]:
            continue

        depart = nxt[u].get(t, t)

        for v, w in graph[u]:
            nt = depart + w

            if nt < dist[v]:
                dist[v] = nt
                heapq.heappush(pq, (nt, v))

    return str(-1 if dist[n] == INF else dist[n])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""4 6
1 2 2
1 3 3
1 4 8
2 3 4
2 4 5
3 4 3
0
1 3
2 3 4
0
"""
) == "7"

# disconnected graph
assert run(
"""3 1
1 2 5
0
0
0
"""
) == "-1"

# starting node blocked at time 0
assert run(
"""2 1
1 2 5
1 0
0
"""
) == "6"

# consecutive waiting times
assert run(
"""2 1
1 2 3
3 0 1 2
0
"""
) == "6"

# arrival at blocked destination is allowed
assert run(
"""2 1
1 2 3
0
1 3
"""
) == "3"

print("ok")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút khởi đầu bị chặn tại thời điểm 0 | 6 | Chờ áp dụng ngay tại nguồn | 
| Khoảnh khắc bị chặn liên tiếp | 6 | Phải tiếp tục chờ đợi qua toàn bộ đoạn đường bị chặn | 
| Đồ thị bị ngắt kết nối | -1 | Xử lý mục tiêu không thể tiếp cận | 
| Điểm đến bị chặn khi đến | 3 | Được phép đến ngay cả khi thời gian bị chặn | 

## Vỏ cạnh 

Hãy xem xét trường hợp hành tinh xuất phát bị chặn ngay lập tức.```
2 1
1 2 5
1 0
0
```Các cửa hàng tiền xử lý:```
0 -> 1
```Dijkstra bắt đầu từ`(0,1)`. Kể từ thời điểm`0`bị chặn, thuật toán chuyển sang thời gian khởi hành`1`. Đi qua mép mất`5`thêm giây nữa, vì vậy câu trả lời cuối cùng sẽ trở thành`6`. 

Bây giờ hãy kiểm tra thời gian bị chặn liên tiếp.```
2 1
1 2 3
3 0 1 2
0
```Quá trình tiền xử lý tạo ra:```
0 -> 3
1 -> 3
2 -> 3
```Khi Dijkstra xử lý hành tinh`1`vào thời điểm đó`0`, nó nhảy ngay đến giờ khởi hành`3`. Thời gian đến hành tinh`2`trở thành`6`. 

Việc triển khai ngây thơ tăng lên một lần sẽ khởi hành không chính xác vào thời điểm đó`1`, điều vẫn bị cấm. 

Cuối cùng, hãy xem xét việc đến đích bị chặn.```
2 1
1 2 3
0
1 3
```Jack đến hành tinh`2`vào thời điểm đó`3`. Thuật toán không bao giờ kiểm tra các quy tắc chờ sau khi đến trừ khi cố gắng truyền tải cạnh khác. Kể từ khi hành tinh`2`là đích đến, câu trả lời vẫn là`3`. 

Điều này khớp chính xác với tuyên bố vấn đề.
