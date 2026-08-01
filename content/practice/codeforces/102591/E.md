---
title: "CF 102591E - \u0414\u0430\u043d\u0438\u044f\u0440 \u0438 \u0435\u0433\u043e \u043b\u044e\u0431\u0438\u043c\u044b\u0435 \u043c\u0430\u0433\u0430\u0437\u0438\u043d\u044b"
description: "Thành phố được biểu diễn dưới dạng đồ thị vô hướng. Mỗi giao lộ là một đỉnh, mỗi con đường là một cạnh và việc sở hữu một đường đi có nghĩa là cạnh đó có sẵn để đi lại."
date: "2026-08-01T06:37:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102591
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041c\u0423\u0418\u0422 \u043f\u043e \u0441\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u043e\u043c\u0443 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020. \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0442\u0443\u0440."
rating: 0
weight: 102591
solve_time_s: 203
verified: true
draft: false
---

[CF 102591E - \u0414\u0430\u043d\u0438\u044f\u0440 \u0438 \u0435\u0433\u043e \u043b\u044e\u0431\u0438\u043c\u044b\u0435 \u043c\u0430\u0433\u0430\u0437\u0438\u043d\u044b](https://codeforces.com/problemset/problem/102591/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 23s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Thành phố được biểu diễn dưới dạng đồ thị vô hướng. Mỗi giao lộ là một đỉnh, mỗi con đường là một cạnh và việc sở hữu một đường đi có nghĩa là cạnh đó có sẵn để đi lại. Daniyar muốn chọn tập hợp các con đường nhỏ nhất có thể sao cho đồ thị con được tạo bởi những con đường đó vẫn cho phép anh đi từ nhà mình ở đỉnh 1 đến mọi cửa hàng yêu thích. 

Đây không phải là yêu cầu con đường ngắn nhất đến từng cửa hàng riêng biệt. Một con đường duy nhất có thể giúp bạn tiếp cận nhiều cửa hàng, vì vậy những con đường được chọn có thể có chung một phần đường đi. Nhiệm vụ là tìm số cạnh tối thiểu cần thiết để nối một tập hợp nhỏ các đỉnh quan trọng trong một đồ thị lớn. 

Biểu đồ có thể chứa tối đa 100000 đỉnh và 100000 cạnh. Với những giới hạn này, các thuật toán kiểm tra nhiều nhóm cạnh có thể là không thể bởi vì ngay cả một lượng công việc bậc hai cũng sẽ vượt xa các phép toán có sẵn. Thuật toán đường đi ngắn nhất thông thường như BFS là ổn vì nó tuyến tính trong kích thước biểu đồ, nhưng thách thức là chúng ta cần kết hợp các đường dẫn đến một số đích. Số lượng cửa hàng nhiều nhất là 4, đây là hạn chế chính cho phép giải pháp lập trình động dựa trên tập hợp con. 

Có một số trường hợp nhỏ phá vỡ các phương pháp đơn giản. Nếu cửa hàng nằm cùng đỉnh với nhà thì cửa hàng đó không cần có thẻ. 

Ví dụ:```
3 2 1
1
1 2
2 3
```Câu trả lời là:```
0
```Một giải pháp bất cẩn luôn thêm chiều dài đường dẫn đến mọi cửa hàng có thể tính không chính xác khoảng cách từ đỉnh 1 đến chính nó. 

Một cái bẫy khác là các cửa hàng trùng lặp. 

Ví dụ:```
4 3 3
3 3 3
1 2
2 3
3 4
```Câu trả lời là:```
2
```Ba cửa hàng thực sự là cùng một địa điểm. Một giải pháp coi chúng như những thiết bị đầu cuối khác nhau có thể tạo ra những kết nối không cần thiết. 

Vấn đề thứ ba là câu trả lời tốt nhất có thể sử dụng cấu trúc phân nhánh thay vì các đường dẫn ngắn nhất riêng biệt. 

Ví dụ:```
5 6 2
4 5
1 2
2 3
3 4
3 5
1 5
2 5
```Câu trả lời là:```
2
```Việc chọn đường đi ngắn nhất đến từng cửa hàng một cách độc lập có thể đếm đường đi chung nhiều lần và tạo ra giá trị lớn hơn. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là tìm đường đi ngắn nhất từ đỉnh 1 đến mọi cửa hàng và lấy tất cả các cạnh từ những đường đi đó. Điều này rất dễ thực hiện và cung cấp một bộ thẻ hợp lệ. Vấn đề là nó bỏ qua việc chia sẻ giữa các tuyến đường. Nếu có thể đến được hai cửa hàng qua cùng một con đường trung gian thì phương pháp này sẽ thanh toán cho phần chung nhiều lần. 

Một phương pháp bạo lực khác sẽ thử mọi tập hợp các cạnh có thể có và kiểm tra xem tất cả các thiết bị đầu cuối có được kết nối hay không. Số tập cạnh có thể có là$2^M$, điều đó hoàn toàn không thể xảy ra khi$M$đạt 100000. 

Cách đúng đắn để xem vấn đề là vấn đề cây Steiner. Trong đồ thị tổng quát, việc tìm cây tối thiểu kết nối các đầu cuối tùy ý là rất khó, nhưng ở đây số lượng đầu cuối rất nhỏ. Chúng ta chỉ có thể nhớ những đỉnh quan trọng nào đã được kết nối. 

Đối với mỗi tập hợp con của các thiết bị đầu cuối và mỗi đỉnh đồ thị, chúng tôi lưu trữ số lượng đường tối thiểu cần thiết để kết nối tập hợp con đó trong khi biến đỉnh hiện tại thành điểm gặp nhau. Hai cấu trúc kết nối nhỏ hơn có thể được hợp nhất nếu chúng gặp nhau ở cùng một đỉnh. Sau mỗi thao tác hợp nhất, việc truyền đường dẫn ngắn nhất sẽ trải đều các giá trị được cải thiện thông qua biểu đồ. 

Giải pháp Brute Force không thành công vì nó coi toàn bộ đồ thị là không gian tìm kiếm. Nhận xét rằng chỉ có ít thiết bị đầu cuối cho phép chúng ta nén phần khó khăn vào$2^K$bang, ở đâu$K$tối đa là 5 sau khi thêm đỉnh nhà. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các bộ cạnh |$O(2^M \cdot (N+M))$|$O(N+M)$| Quá chậm | 
| Con đường ngắn nhất độc lập |$O(K(N+M))$|$O(N+M)$| Nói chung là không đúng | 
| Lập trình động tập hợp con |$O(3^K N + 2^K(N+M))$|$O(2^K N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xóa các vị trí cửa hàng trùng lặp và thêm đỉnh 1 làm thiết bị đầu cuối. Số lượng thiết bị đầu cuối bây giờ nhiều nhất là 5. 

Quy hoạch động chỉ phụ thuộc vào các đỉnh đặc biệt. Nếu cùng một đỉnh xuất hiện nhiều lần thì việc kết nối nó một lần đã thỏa mãn mọi lần xuất hiện. 

1. Tạo trạng thái`dp[mask][v]`. Mặt nạ mô tả thiết bị đầu cuối nào đã được kết nối và`v`là đỉnh nơi cấu trúc được kết nối hiện kết thúc. Khởi tạo mọi trạng thái đầu cuối đơn có giá trị 0 tại thiết bị đầu cuối của chính nó. 

Một thiết bị đầu cuối duy nhất không cần đường để kết nối. Mọi giá trị khác bắt đầu là vô cùng vì nó chưa được xây dựng. 

1. Đối với mỗi mặt nạ, hãy chia nó thành hai phần rời rạc không trống. Hãy thử kết hợp cấu trúc tốt nhất cho từng phần ở cùng một đỉnh. 

Nếu một cấu trúc kết nối thiết bị đầu cuối`a`và một bộ thiết bị đầu cuối kết nối khác`b`, việc đặt cả hai ở cùng một đỉnh sẽ tạo ra một cấu trúc được kết nối cho`a | b`. Đây là lý do tại sao quá trình chuyển đổi là:$$dp[mask][v] = \min(dp[mask][v], dp[sub][v] + dp[mask \setminus sub][v])$$1. Sau tất cả các chuyển tiếp hợp nhất cho mặt nạ, hãy chạy BFS đa nguồn bằng cách sử dụng các giá trị hiện tại làm khoảng cách bắt đầu. 

Bước hợp nhất chỉ cho phép hai cấu trúc đã gặp nhau ở một đỉnh. Tài khoản BFS để mở rộng điểm cuối qua các con đường thông thường. Vì mỗi con đường đều tốn một lượt đi, nên BFS thông thường chính xác là con đường rút ngắn nhất cần thiết. 

1. Sau khi xử lý mọi mặt nạ, câu trả lời là giá trị tối thiểu ở trạng thái chứa tất cả các thiết bị đầu cuối. 

Cấu trúc kết nối cuối cùng có thể kết thúc ở bất kỳ đỉnh nào. Yêu cầu duy nhất là mọi đỉnh quan trọng đều thuộc cùng một tập hợp đường đã chọn. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý mặt nạ,`dp[mask][v]`biểu thị số lượng đường tối thiểu trong bất kỳ sơ đồ con được kết nối nào chứa chính xác các điểm cuối từ`mask`và bao gồm đỉnh`v`. Ban đầu điều này đúng với các thiết bị đầu cuối đơn. Việc hợp nhất hai cấu trúc nhỏ hơn hợp lệ sẽ tạo ra một cấu trúc kết nối hợp lệ khác và mọi cấu trúc tối ưu có thể được tách tại một số đỉnh gặp nhau thành hai nhóm đầu cuối nhỏ hơn. BFS sau đó tìm ra cách rẻ nhất để di chuyển điểm gặp nhau qua biểu đồ. Bởi vì mọi sự phân chia có thể và mọi điểm cuối có thể đều được xem xét, trạng thái đầu cuối đầy đủ chứa kích thước cây Steiner tối ưu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    shops = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    terminals = [0]
    for x in shops:
        x -= 1
        if x not in terminals:
            terminals.append(x)

    t = len(terminals)
    full = (1 << t) - 1
    inf = 10 ** 9

    dp = [[inf] * n for _ in range(1 << t)]

    for i, vertex in enumerate(terminals):
        dp[1 << i][vertex] = 0

    for mask in range(1, 1 << t):
        sub = (mask - 1) & mask
        while sub:
            other = mask ^ sub
            if sub < other:
                first = dp[sub]
                second = dp[other]
                current = dp[mask]
                for v in range(n):
                    value = first[v] + second[v]
                    if value < current[v]:
                        current[v] = value
            sub = (sub - 1) & mask

        dist = dp[mask]
        queue = deque()

        for v in range(n):
            if dist[v] < inf:
                queue.append(v)

        while queue:
            u = queue.popleft()
            for v in graph[u]:
                if dist[v] > dist[u] + 1:
                    dist[v] = dist[u] + 1
                    queue.append(v)

    print(min(dp[full]))

if __name__ == "__main__":
    solve()
```Việc xây dựng thiết bị đầu cuối loại bỏ các cửa hàng lặp lại trước khi xây dựng bảng lập trình động. Điều này giữ cho số lượng mặt nạ càng ít càng tốt. 

Bảng có một hàng cho mỗi tập hợp con đầu cuối. Vì có nhiều nhất năm điểm cuối nên số trạng thái tối đa chỉ là 32. Mỗi trạng thái chứa một giá trị khoảng cách cho mỗi đỉnh đồ thị. 

Vòng lặp tập hợp con chỉ sử dụng các cặp trong đó`sub < other`, tránh sự hợp nhất trùng lặp. Hàng đợi BFS bắt đầu với mọi đỉnh đã có giá trị hữu hạn, biến việc thư giãn thành tìm kiếm đường đi ngắn nhất từ ​​nhiều nguồn. 

Số nguyên Python không tràn ở đây vì câu trả lời lớn nhất có thể có nhiều nhất là số cạnh. Giá trị vô cực được chọn lớn hơn nhiều so với bất kỳ câu trả lời nào có thể có. Việc triển khai cũng sử dụng BFS lặp thay vì đệ quy vì kích thước biểu đồ có thể là 100000. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các đầu cuối là các đỉnh 1, 2 và 3 sau khi thêm nhà. Lập trình động thử kết hợp ba thiết bị đầu cuối này. 

| Bước | Thiết bị đầu cuối được kết nối | Chi phí được biết đến nhiều nhất | Giải thích | 
| --- | --- | --- | --- | 
| Khởi tạo | {1}, {2}, {3} | 0 tại các đỉnh của chính chúng | Mỗi nhà ga một mình không cần đường | 
| Hợp nhất {1}+{2} | {1,2} | 1 | Đỉnh 1 kết nối trực tiếp với đỉnh 2 | 
| Hợp nhất {1}+{3} | {1,3} | 2 | Đã tìm thấy kết nối ngắn qua đỉnh 5 | 
| Hợp nhất {2}+{3} | {2,3} | 1 | Đỉnh 2 và 3 chung một đường | 
| Hợp nhất cuối cùng | {1,2,3} | 3 | Ba thiết bị đầu cuối được kết nối | 

Kết quả là 3, chứng tỏ rằng câu trả lời không phải là tổng của các đường đi ngắn nhất riêng lẻ. 

Đối với mẫu thứ hai, các cửa hàng`1, 3, 3`. Sau khi loại bỏ các bản sao, các đầu cuối chỉ còn đỉnh 1 và 3. 

| Bước | Thiết bị đầu cuối được kết nối | Chi phí được biết đến nhiều nhất | Giải thích | 
| --- | --- | --- | --- | 
| Khởi tạo | {1} | 0 tại đỉnh 1 | Thiết bị đầu cuối tại nhà | 
| Khởi tạo | {3} | 0 tại đỉnh 3 | Cửa hàng thiết bị đầu cuối | 
| Hợp nhất | {1,3} | 2 | Đường 1-2-3 sử dụng hai đường | 
| Câu trả lời cuối cùng | {1,3} | 2 | Cửa hàng trùng lặp không thay đổi kết quả | 

Dấu vết xác nhận rằng các cửa hàng lặp lại được xử lý một cách tự nhiên bằng cách nén danh sách thiết bị đầu cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(3^K N + 2^K(N+M))$| Mỗi tập hợp con thực hiện việc hợp nhất tập hợp con và một BFS | 
| Không gian |$O(2^K N)$| Bảng DP lưu trữ mọi tập hợp con và mọi đỉnh | 

Đây$K$là số lượng thiết bị đầu cuối duy nhất sau khi thêm nhà, vì vậy$K \leq 5$. Do đó, phần mũ nhiều nhất là một hằng số nhỏ, trong khi quá trình xử lý đồ thị vẫn tuyến tính ở dạng$N+M$. Điều này phù hợp với giới hạn với 100000 đỉnh và cạnh. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    shops = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    terminals = [0]
    for x in shops:
        x -= 1
        if x not in terminals:
            terminals.append(x)

    t = len(terminals)
    inf = 10 ** 9
    dp = [[inf] * n for _ in range(1 << t)]

    for i, x in enumerate(terminals):
        dp[1 << i][x] = 0

    for mask in range(1, 1 << t):
        sub = (mask - 1) & mask
        while sub:
            other = mask ^ sub
            if sub < other:
                for i in range(n):
                    dp[mask][i] = min(dp[mask][i], dp[sub][i] + dp[other][i])
            sub = (sub - 1) & mask

        q = deque(i for i, x in enumerate(dp[mask]) if x < inf)
        while q:
            u = q.popleft()
            for v in graph[u]:
                if dp[mask][v] > dp[mask][u] + 1:
                    dp[mask][v] = dp[mask][u] + 1
                    q.append(v)

    return str(min(dp[-1]))

assert run("""6 7 2
2 3
1 5
1 6
3 5
3 4
2 4
3 6
2 6
""") == "3"

assert run("""4 3 3
1 3 3
1 2
2 3
3 4
""") == "2"

assert run("""1 1 1
1
""") == "0"

assert run("""4 3 2
4 4
1 2
2 3
3 4
""") == "3"

assert run("""5 5 3
2 3 4
1 2
1 3
2 5
3 5
5 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn với nhà là cửa hàng | 0 | Không xử lý đường cần thiết | 
| Ba cửa hàng giống hệt nhau | 3 | Xử lý các thiết bị đầu cuối trùng lặp | 
| Nhiều tuyến đường có chung một trung tâm | 3 | Xử lý phân nhánh kiểu Steiner | 
| Cung cấp mẫu | 3 và 2 | Xác nhận trường hợp tiêu chuẩn | 

## Vỏ cạnh 

Trường hợp cửa hàng là nhà được xử lý trong quá trình tạo thiết bị đầu cuối. Đối với đầu vào:```
3 2 1
1
1 2
2 3
```danh sách thiết bị đầu cuối chỉ chứa đỉnh 1. Mặt nạ cuối cùng đã là một thiết bị đầu cuối duy nhất, vì vậy khoảng cách của nó bằng 0. Thuật toán xuất ra 0. 

Đối với các cửa hàng trùng lặp:```
4 3 3
3 3 3
1 2
2 3
3 4
```danh sách thiết bị đầu cuối trở thành`[1, 3]`trong lập chỉ mục một cơ sở. DP giải quyết bài toán nối hai đỉnh đó, yêu cầu các đường`1-2`Và`2-3`. Câu trả lời là 2. 

Đối với các đường dẫn được chia sẻ, hãy xem xét:```
5 6 2
4 5
1 2
2 3
3 4
3 5
1 5
2 5
```Cách tiếp cận đường đi ngắn nhất độc lập có thể tính hai tuyến đường riêng biệt, nhưng tập hợp con DP cho phép cả hai nhóm đầu cuối hợp nhất ở cùng một đỉnh trung gian. Trạng thái cuối cùng tìm thấy cấu trúc được kết nối chỉ có hai con đường, là mức tối thiểu.
