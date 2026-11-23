---
title: "CF 48E - Ivan Kẻ Ngốc VS Rồng Gorynych"
description: "Trạng thái trò chơi được mô tả hoàn toàn bằng hai con số: con rồng hiện có bao nhiêu đầu và bao nhiêu đuôi. Từ trạng thái (h, t) Ivan có thể chọn một trong hai kiểu nước đi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "graphs"]
categories: ["algorithms"]
codeforces_contest: 48
codeforces_index: "E"
codeforces_contest_name: "School Personal Contest #3 (Winter Computer School 2010/11) - Codeforces Beta Round 45 (ACM-ICPC Rules)"
rating: 2100
weight: 48
solve_time_s: 141
verified: true
draft: false
---

[CF 48E - Ivan the Fool VS Gorynych the Dragon](https://codeforces.com/problemset/problem/48/E) 

**Đánh giá:** 2100 
**Tags:** dp, trò chơi, đồ thị 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trạng thái trò chơi được mô tả hoàn toàn bằng hai con số: con rồng hiện có bao nhiêu đầu và bao nhiêu đuôi. Từ một tiểu bang`(h, t)`Ivan có thể chọn một trong hai kiểu di chuyển. 

Nếu anh ta cắt`i`đầu, ở đâu`1 ≤ i ≤ n`Và`i ≤ h`, sau đó con rồng sẽ mọc lại`headGrow[i]`đầu và`tailGrow[i]`đuôi. Trạng thái mới trở thành:`(h - i + headGrow[i], t + tailGrow[i])`Nếu anh ta cắt`i`đuôi, ở đâu`1 ≤ i ≤ m`Và`i ≤ t`, thì trạng thái mới trở thành:`(h + headGrowTail[i], t - i + tailGrowTail[i])`Cuộc chiến ngay lập tức kết thúc theo một trong ba cách. 

Ivan thắng nếu bang trở thành`(0, 0)`sau khi tái sinh. 

Rồng thắng nếu sau khi di chuyển tổng số phần vượt quá`R`. 

Trận đấu sẽ hòa nếu Ivan có thể tránh thua vĩnh viễn nhưng không bao giờ có thể giành được chiến thắng. 

Điều khó khăn là Ivan luôn chơi tối ưu theo mục tiêu ba cấp độ. Đầu tiên anh ta thích chiến thắng với số nước đi tối thiểu. Nếu chiến thắng là không thể, anh ta thích sống sót mãi mãi. Nếu điều đó là không thể, anh ấy muốn trì hoãn thất bại càng lâu càng tốt. 

Giới hạn đủ nhỏ để xử lý mọi giá trị hợp lệ`(h, t)`ghép nối như một nút đồ thị. Từ`h + t ≤ R ≤ 200`, tổng số trạng thái pháp lý có thể tiếp cận tối đa là khoảng`201 × 201`, nhưng chỉ trạng thái với`h + t ≤ R`vấn đề. Điều đó mang lại đại khái`O(R²)`tiểu bang, khoảng bốn mươi nghìn trong trường hợp xấu nhất. Mỗi tiểu bang có tối đa`n + m ≤ 400`chuyển tiếp đi. Một thuật toán đồ thị trên tất cả các trạng thái là hoàn toàn khả thi. 

Một mô phỏng đệ quy đơn giản không ghi nhớ sẽ thất bại vì biểu đồ trò chơi chứa các chu trình. Ví dụ: giả sử cắt một đầu sẽ tái tạo chính xác một đầu. Sau đó, trạng thái không bao giờ thay đổi và lặp lại đệ quy mãi mãi. 

Một trường hợp tế nhị xuất hiện khi Ivan có thể sống sót mãi mãi nhưng không thể chạm tới`(0,0)`. 

Ví dụ:```
1 0 5
1
1 0
1
0 0
```Từ`(1,0)`, cắt một đầu dẫn trở lại`(1,0)`. Ivan không bao giờ thua vì tổng số tiền không bao giờ vượt quá`R`, nhưng anh ấy cũng không bao giờ thắng. Đầu ra đúng là:```
Draw
```Tìm kiếm đường đi ngắn nhất chỉ tìm kiếm`(0,0)`sẽ kết luận sai rằng Ivan thua. 

Một trường hợp quan trọng khác là khi mọi nước đi có thể ngay lập tức vượt quá`R`. 

Ví dụ:```
1 0 1
1
2 0
1
0 0
```Cắt một đầu tạo ra`(2,0)`, tổng của nó vượt quá`R`. Ivan thua ngay lập tức:```
Zmey
1
```Việc triển khai bất cẩn đôi khi quên mất việc vượt quá`R`là tổn thất đầu cuối ngay lập tức và vô tình chèn các trạng thái đó vào biểu đồ. 

Một trường hợp tinh tế hơn là khi Ivan không thể tránh khỏi thất bại cuối cùng, nhưng những lựa chọn khác nhau sẽ trì hoãn thất bại đó ở mức độ khác nhau. 

Ví dụ:```
1 0 3
1
2 0
1
0 0
```Động thái duy nhất là:`(1,0) → (2,0) → (3,0) → lose`Câu trả lời là:```
Zmey
3
```Con số được in ra là số nước đi tối đa mà Ivan có thể sống sót trước thất bại khó tránh khỏi. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xem từng trạng thái`(h,t)`như một nút và khám phá đệ quy tất cả các bước di chuyển có thể. Nếu một động thái đạt tới`(0,0)`, Ivan thắng. Nếu mọi bước đi cuối cùng đều vượt quá`R`, con rồng thắng. Nếu một chu kỳ nào đó tồn tại, trò chơi có thể tiếp tục mãi mãi. 

Cách tiếp cận đệ quy này đúng về mặt logic vì trò chơi là hữu hạn trong vùng pháp lý`h + t ≤ R`. Vấn đề là chỉ đệ quy không thể phân biệt được giữa các lần truy cập hữu ích và các vòng lặp vô hạn. Một DFS truy cập lại cùng một trạng thái nhiều lần có thể không bao giờ chấm dứt. Ngay cả với việc ghi nhớ, việc xử lý các chu trình một cách chính xác cũng trở nên phức tạp vì trò chơi không phải là một trò chơi thắng-thua đơn giản, nó có ba kết quả với tiêu chí tối ưu hóa. 

Quan sát quan trọng là các trạng thái pháp lý tạo thành một biểu đồ có hướng với tối đa khoảng bốn mươi nghìn nút. Khi chúng ta nghĩ về các thuật ngữ đồ thị, bài toán sẽ tự nhiên được chia thành ba bài toán đồ thị riêng biệt. 

Đầu tiên, hãy xác định xem Ivan có thể tiếp cận được không`(0,0)`trước khi rời khỏi khu vực pháp lý. Vì mỗi bước di chuyển tốn một bước nên đây chỉ là đường đi ngắn nhất trong biểu đồ không có trọng số. 

Nếu không thể chiến thắng, hãy xác định xem Ivan có thể tồn tại mãi mãi hay không. Điều đó xảy ra chính xác khi có thể truy cập được một số chu kỳ từ trạng thái bắt đầu trong khi vẫn hoàn toàn nằm trong khu vực pháp lý. 

Nếu không thể chiến thắng hay sống sót vô hạn thì biểu đồ có thể truy cập ngay từ đầu là DAG kết thúc ở các lần thoát thua. Trong tình huống đó, Ivan muốn có thời gian tồn tại lâu nhất có thể, đây sẽ là con đường dài nhất trong DAG. 

Cấu trúc của biểu đồ trạng thái là yếu tố làm cho quá trình phân tách này hoạt động được. Mọi nước đi hợp pháp đều nằm trong không gian trạng thái giới hạn trừ khi rồng thắng ngay lập tức. Giới hạn đó cho phép các thuật toán đồ thị tiêu chuẩn mô tả hoàn toàn đặc điểm của trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | Hàm mũ | Cây đệ quy hàm mũ | Quá chậm | 
| Đồ thị tối ưu + BFS + Phát hiện chu kỳ + DP | O(R2(n+m)) | O(R²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ra mọi trạng thái pháp lý`(h,t)`như vậy`h + t ≤ R`. 

Đây là những bang duy nhất mà cuộc chiến có thể tiếp tục. Bất kỳ động thái nào tạo ra tổng số lớn hơn đều có nghĩa là thất bại ngay lập tức. 
2. Xây dựng sự chuyển đổi có định hướng giữa các trạng thái pháp lý. 

Đối với mọi tiểu bang, hãy thử tất cả các cách cắt đầu và cắt đuôi có thể. Nếu trạng thái kết quả vẫn thỏa mãn`h + t ≤ R`, thêm một cạnh vào nó. Nếu không, hành động đó được coi là một lệnh thoát thua ngay lập tức. 
3. Chạy BFS từ trạng thái ban đầu để tính khoảng cách ngắn nhất. 

Vì mỗi lần di chuyển tốn chính xác một đòn, BFS đưa ra số lần di chuyển tối thiểu cho mọi trạng thái có thể tiếp cận. 
4. Nếu`(0,0)`có thể truy cập được, in`"Ivan"`và khoảng cách BFS. 

BFS đảm bảo đây là số lần đánh nhỏ nhất có thể. 
5. Mặt khác, hãy kiểm tra xem đồ thị con có thể truy cập có chứa chu trình hay không. 

Chạy DFS với ba màu:`0 = unvisited`

`1 = currently in recursion stack`

`2 = fully processed`Gặp một cạnh của một nút có màu`1`có nghĩa là một chu kỳ có thể tiếp cận tồn tại. 
6. Nếu tồn tại một chu trình có thể truy cập được, hãy in`"Draw"`. 

Ivan có thể ở trong chu kỳ đó mãi mãi và tránh thất bại vô thời hạn. 
7. Mặt khác, đồ thị có thể truy cập là không có chu kỳ. 

Mọi đường đi cuối cùng phải kết thúc ở một bước đi vượt quá`R`. 
8. Tính thời gian tồn tại lâu nhất với DP trên DAG. 

Định nghĩa`dp[state]`là số bước đi tối đa trước khi bị đánh bại bắt đầu từ trạng thái đó. 

Đối với một tiểu bang không có động thái hợp pháp,`dp = 1`vì nước đi tiếp theo sẽ thua ngay. 

Nếu không thì:`dp[state] = 1 + max(dp[next_state])`9. In`"Zmey"`Và`dp[start]`. 

Đây là số chiêu thức lớn nhất mà Ivan có thể sống sót khi không thể tránh khỏi thất bại. 

### Tại sao nó hoạt động 

Thuật toán phân chia tất cả các trò chơi có thể có thành chính xác ba loại. 

Nếu như`(0,0)`có thể truy cập được, BFS sẽ tìm chuỗi chiến thắng ngắn nhất vì mọi cạnh đều có chi phí bằng nhau. 

Nếu như`(0,0)`không thể truy cập được nhưng tồn tại một chu trình có thể truy cập được, Ivan có thể ở lại trạng thái hợp pháp mãi mãi bằng cách lặp lại chu trình đó. Vì thất bại chỉ xảy ra khi vượt quá`R`, một chu kỳ như vậy đảm bảo sự tồn tại vô hạn. 

Nếu không có điều kiện nào xảy ra thì biểu đồ có thể truy cập là DAG hữu hạn. Mọi lượt chơi cuối cùng đều đi đến ngõ cụt, nơi mọi nước đi đều thua ngay lập tức. Lập trình động trên DAG tính toán thời gian tồn tại tối đa có thể vì mỗi trạng thái chọn cách tiếp tục tốt nhất trong số tất cả các động thái hợp pháp. 

Ba trường hợp này loại trừ lẫn nhau và bao trùm mọi diễn biến có thể có của trò chơi. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10**18

def solve():
    H, T, R = map(int, input().split())

    n = int(input())
    head = [(0, 0)] * (n + 1)
    for i in range(1, n + 1):
        head[i] = tuple(map(int, input().split()))

    m = int(input())
    tail = [(0, 0)] * (m + 1)
    for i in range(1, m + 1):
        tail[i] = tuple(map(int, input().split()))

    states = []
    for h in range(R + 1):
        for t in range(R + 1 - h):
            states.append((h, t))

    graph = {}
    for h, t in states:
        nxt = []

        for i in range(1, min(n, h) + 1):
            nh = h - i + head[i][0]
            nt = t + head[i][1]

            if nh + nt <= R:
                nxt.append((nh, nt))

        for i in range(1, min(m, t) + 1):
            nh = h + tail[i][0]
            nt = t - i + tail[i][1]

            if nh + nt <= R:
                nxt.append((nh, nt))

        graph[(h, t)] = nxt

    start = (H, T)
    target = (0, 0)

    dist = {start: 0}
    q = deque([start])

    while q:
        v = q.popleft()

        for to in graph[v]:
            if to not in dist:
                dist[to] = dist[v] + 1
                q.append(to)

    if target in dist:
        print("Ivan")
        print(dist[target])
        return

    color = {}
    has_cycle = False

    def dfs(v):
        nonlocal has_cycle

        color[v] = 1

        for to in graph[v]:
            if to not in dist:
                continue

            if color.get(to, 0) == 0:
                dfs(to)
            elif color[to] == 1:
                has_cycle = True

        color[v] = 2

    dfs(start)

    if has_cycle:
        print("Draw")
        return

    dp = {}

    def longest(v):
        if v in dp:
            return dp[v]

        if not graph[v]:
            dp[v] = 1
            return 1

        best = 0
        for to in graph[v]:
            best = max(best, longest(to))

        dp[v] = best + 1
        return dp[v]

    print("Zmey")
    print(longest(start))

solve()
```Việc xây dựng biểu đồ tuân theo các quy tắc trò chơi chính xác. Mọi trạng thái hợp pháp chỉ lưu trữ các chuyển đổi nằm trong tổng số được phép`R`. Di chuyển vượt quá`R`bị loại trừ một cách có chủ ý vì chúng thể hiện sự thất bại ngay lập tức chứ không phải một trạng thái có thể chơi được khác. 

Phần BFS giải quyết trường hợp thắng trước vì Ivan luôn thích thắng hơn hòa hoặc trì hoãn thất bại. Sử dụng BFS thay vì DFS rất quan trọng vì câu trả lời yêu cầu số lần đánh tối thiểu. 

DFS phát hiện chu trình chỉ đi qua các trạng thái có thể truy cập được ngay từ đầu. Một chu trình ở nơi khác trong biểu đồ sẽ không liên quan nếu Ivan không thể nhập nó. DFS ba màu là cách tiêu chuẩn để phát hiện các chu kỳ có hướng. Nhìn thấy một nút đã có trong ngăn xếp đệ quy có nghĩa là chúng tôi đã tìm thấy cạnh sau và do đó một vòng lặp vô hạn. 

DP đường đi dài nhất chỉ hoạt động vì biểu đồ được đảm bảo không có chu kỳ sau khi trường hợp hòa bị loại trừ. Nếu không có thuộc tính đó, đệ quy sẽ lặp mãi mãi. Các quốc gia không có lợi thế pháp lý sẽ quay trở lại`1`bởi vì Ivan vẫn còn đi được một nước đi cuối cùng trước khi thua ngay lập tức. 

Một chi tiết tinh tế là thứ tự kiểm tra. Chúng ta phải kiểm tra khả năng tiếp cận của`(0,0)`trước khi kiểm tra chu kỳ. Một chu kỳ có thể đạt được sẽ không thành vấn đề nếu Ivan đã có sẵn chiến lược chiến thắng, bởi vì anh ấy luôn thích chiến thắng hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2 4
2
1 0
0 1
3
0 1
0 1
0 0
```Các chuyển đổi trạng thái: 

| Trạng thái hiện tại | Di chuyển | Bang tiếp theo | 
| --- | --- | --- | 
| (2,2) | cắt 2 đuôi | (2,1) | 
| (2,1) | cắt 1 đầu | (2,1) | 
| (2,1) | cắt 1 đuôi | (2,1) | 
| (2,1) | cắt 3 đuôi không thể | - | 
| (2,1) | cắt 2 đầu | (0,2) | 
| (0,2) | cắt 2 đuôi | (0,1) | 
| (0,1) | cắt 1 đuôi | (0,0) | 

Chuỗi chiến thắng ngắn nhất: 

| Bước | Tiểu bang | 
| --- | --- | 
| 0 | (2,2) | 
| 1 | (0,2) | 
| 2 | (0,0) | 

Đầu ra:```
Ivan
2
```Dấu vết này cho thấy tại sao BFS lại cần thiết. Có nhiều vòng lặp tồn tại nhưng BFS vẫn tìm ra con đường ngắn nhất để chiến thắng. 

### Ví dụ 2 

đầu vào:```
1 0 5
1
1 0
1
0 0
```Các trạng thái có thể truy cập: 

| Bước | Tiểu bang | 
| --- | --- | 
| 0 | (1,0) | 
| 1 | (1,0) | 
| 2 | (1,0) | 
| ... | ... | 

Nước đi duy nhất sẽ trở lại trạng thái cũ mãi mãi. 

Đầu ra:```
Draw
```Điều này thể hiện điều kiện chu kỳ. Ivan không thể thắng, nhưng có thể tránh được thất bại vô thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R2(n+m)) | Mọi quốc gia pháp lý đều thử mọi hoạt động đầu đuôi | 
| Không gian | O(R²) | Thông tin lưu trữ đồ thị, BFS, DFS và DP cho từng trạng thái | 

Với`R ≤ 200`, số lượng bang nhiều nhất là khoảng bốn mươi nghìn. Mỗi trạng thái xử lý tối đa bốn trăm lần chuyển đổi. Tổng số công việc vừa vặn thoải mái trong giới hạn của Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    H, T, R = map(int, input().split())

    n = int(input())
    head = [(0, 0)] * (n + 1)
    for i in range(1, n + 1):
        head[i] = tuple(map(int, input().split()))

    m = int(input())
    tail = [(0, 0)] * (m + 1)
    for i in range(1, m + 1):
        tail[i] = tuple(map(int, input().split()))

    states = []
    for h in range(R + 1):
        for t in range(R + 1 - h):
            states.append((h, t))

    graph = {}
    for h, t in states:
        nxt = []

        for i in range(1, min(n, h) + 1):
            nh = h - i + head[i][0]
            nt = t + head[i][1]

            if nh + nt <= R:
                nxt.append((nh, nt))

        for i in range(1, min(m, t) + 1):
            nh = h + tail[i][0]
            nt = t - i + tail[i][1]

            if nh + nt <= R:
                nxt.append((nh, nt))

        graph[(h, t)] = nxt

    start = (H, T)
    target = (0, 0)

    dist = {start: 0}
    q = deque([start])

    while q:
        v = q.popleft()

        for to in graph[v]:
            if to not in dist:
                dist[to] = dist[v] + 1
                q.append(to)

    out = []

    if target in dist:
        out.append("Ivan")
        out.append(str(dist[target]))
        return "\n".join(out)

    color = {}
    has_cycle = False

    def dfs(v):
        nonlocal has_cycle

        color[v] = 1

        for to in graph[v]:
            if to not in dist:
                continue

            if color.get(to, 0) == 0:
                dfs(to)
            elif color[to] == 1:
                has_cycle = True

        color[v] = 2

    dfs(start)

    if has_cycle:
        return "Draw"

    dp = {}

    def longest(v):
        if v in dp:
            return dp[v]

        if not graph[v]:
            dp[v] = 1
            return 1

        best = 0
        for to in graph[v]:
            best = max(best, longest(to))

        dp[v] = best + 1
        return dp[v]

    out.append("Zmey")
    out.append(str(longest(start)))

    return "\n".join(out)

# provided sample
assert run(
"""2 2 4
2
1 0
0 1
3
0 1
0 1
0 0
"""
) == "Ivan\n2", "sample 1"

# draw loop
assert run(
"""1 0 5
1
1 0
1
0 0
"""
) == "Draw", "infinite cycle"

# immediate loss
assert run(
"""1 0 1
1
2 0
1
0 0
"""
) == "Zmey\n1", "forced immediate defeat"

# shortest win
assert run(
"""1 1 3
1
0 0
1
0 0
"""
) == "Ivan\n2", "minimal winning path"

# longer forced survival
assert run(
"""1 0 3
1
2 0
1
0 0
"""
) == "Zmey\n3", "maximize survival time"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tự lặp vô hạn | Vẽ | Phát hiện chu kỳ có thể tiếp cận | 
| Tràn ngay lập tức | Zmey 1 | Nước đi thua không được thêm vào dưới dạng trạng thái | 
| Thắng trực tiếp nhỏ | Ivan 2 | Độ chính xác của đường dẫn ngắn nhất BFS | 
| Chuỗi phát triển | Zmey 3 | DP tồn tại lâu nhất | 
| Mẫu chính thức | Ivan 2 | Tích hợp đầy đủ | 

## Vỏ cạnh 

Xét trường hợp tự lặp:```
1 0 5
1
1 0
1
0 0
```Từ`(1,0)`, cắt một đầu sẽ tái sinh một đầu. BFS không bao giờ đạt tới`(0,0)`. DFS sau đó truy cập`(1,0)`và ngay lập tức tìm thấy một cạnh quay lại nút đã có trong ngăn xếp đệ quy. Thuật toán in:```
Draw
```Điều này nắm bắt chính xác sự sống sót vô hạn. 

Bây giờ hãy xem xét thất bại ngay lập tức:```
1 0 1
1
2 0
1
0 0
```Sự chuyển đổi duy nhất là:`(1,0) → (2,0)`Từ`2 > R`, cạnh này bị loại khỏi biểu đồ. Trạng thái bắt đầu không có cạnh đi ra hợp pháp. BFS không thể tiếp cận`(0,0)`, DFS không tìm thấy chu trình nào và DP trả về`1`. Thuật toán in:```
Zmey
1
```Câu trả lời tính nước đi thua cuối cùng một cách chính xác. 

Cuối cùng, hãy xem xét thất bại không thể tránh khỏi nhưng bị trì hoãn:```
1 0 3
1
2 0
1
0 0
```Các trạng thái có thể truy cập là:`(1,0) → (2,0) → (3,0)`Từ`(3,0)`, mọi chuyển động đều vượt quá`R`. Đồ thị không có tính tuần hoàn nên DP tính: 

| Tiểu bang | dp | 
| --- | --- | 
| (3,0) | 1 | 
| (2,0) | 2 | 
| (1,0) | 3 | 

Đầu ra trở thành:```
Zmey
3
```Điều này xác nhận rằng thuật toán tối đa hóa thời gian sống sót khi không thể tránh khỏi thất bại.
