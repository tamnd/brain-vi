---
title: "CF 106347D - \u041c\u043e\u0434\u0443\u043b\u044c\u043d\u044b\u0439 \u0433\u0440\u0430\u0444"
description: "Chúng ta có một đồ thị vô hướng được kết nối. Mỗi đỉnh lưu trữ một số và việc di chuyển qua một cạnh sẽ tạo ra sự khác biệt tuyệt đối giữa các số được ghi ở hai điểm cuối của nó."
date: "2026-06-25T08:05:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106347
codeforces_index: "D"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2024. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 106347
solve_time_s: 53
verified: true
draft: false
---

[CF 106347D - \u041c\u043e\u0434\u0443\u043b\u044c\u043d\u044b\u0439 \u0433\u0440\u0430\u0444](https://codeforces.com/problemset/problem/106347/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng được kết nối. Mỗi đỉnh lưu trữ một số và việc di chuyển qua một cạnh sẽ tạo ra sự khác biệt tuyệt đối giữa các số được ghi ở hai điểm cuối của nó. Trước khi đi từ đỉnh`1`đến đỉnh`n`, chúng ta có thể thay thế các số nhiều nhất`k`các đỉnh với bất kỳ giá trị nào chúng ta chọn. Nhiệm vụ là tìm ra chi phí đi lại tối thiểu có thể sau khi thực hiện những thay thế tốt nhất có thể. 

Đầu vào cung cấp biểu đồ, giá trị ban đầu của mỗi đỉnh và số đỉnh tối đa có giá trị có thể được thay đổi. Đầu ra là chi phí nhỏ nhất có thể có của đường đi từ đỉnh đầu tiên đến đỉnh cuối cùng sau những thay đổi đó. 

Khó khăn chính xuất phát từ thực tế là một đỉnh bị thay đổi có thể nhận bất kỳ giá trị nào. Chúng ta không cần phải quyết định trực tiếp những con số thực tế mới. Chúng ta cần hiểu tác dụng của việc loại bỏ một số lượng nhỏ các đỉnh khỏi một đường dẫn. 

Các giới hạn nhỏ về kích thước biểu đồ nhưng các giá trị thì không. Có tới 2000 đỉnh và 2000 cạnh, trong khi`k`nhiều nhất là 10. Một giải pháp phụ thuộc vào các giá trị số là không thể vì các giá trị có thể đạt tới`10^9`. Một giải pháp xử lý đồ thị bậc hai có thể chấp nhận được, nhưng bất kỳ giải pháp nào liên quan đến tất cả các đường dẫn có thể thì không. 

Giá trị nhỏ của`k`là hạn chế chính. Chúng ta có thể sử dụng các thuật toán nhân kích thước đồ thị với một hệ số nhỏ, chẳng hạn như`O(n * k)`trạng thái hoặc khám phá những con đường có chiều sâu xung quanh`k`. Chúng ta không thể liệt kê tất cả các đường dẫn vì ngay cả một biểu đồ thưa thớt cũng có thể chứa nhiều đường dẫn theo cấp số nhân. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ quá trình triển khai. 

Nếu như`k = 0`, không có đỉnh nào có thể được sửa đổi, vì vậy câu trả lời phải là đường đi ngắn nhất thông thường có trọng số cạnh`|a_u-a_v|`. Ví dụ:```
2 1 0
5 20
1 2
```Con đường duy nhất có chi phí`15`, vậy đáp án là:```
15
```Một giải pháp luôn giả định ít nhất một sửa đổi miễn phí có thể tạo ra giá trị 0 không chính xác. 

Đỉnh bắt đầu hoặc đỉnh kết thúc có thể nằm trong số các đỉnh được thay đổi. Coi như:```
2 1 2
100 200
1 2
```Cả hai đỉnh đều có thể được thay đổi thành cùng một giá trị, vì vậy câu trả lời là:```
0
```Giải pháp chỉ cho phép thay đổi các đỉnh bên trong của đường đi sẽ bỏ sót trường hợp này. 

Một đường dẫn có thể chứa nhiều hơn`k`các đỉnh nhưng vẫn được hưởng lợi từ những thay đổi ở giữa. Ví dụ:```
4 3 1
1 100 5 10
1 2
2 3
3 4
```Thay đổi đỉnh`2`ĐẾN`5`đưa ra một chi phí đường đi của`9`. Đỉnh đã thay đổi không cần phải được gán một trong các giá trị ban đầu của đỉnh lân cận và thuật toán phải cho phép thay thế tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi đường đi có thể và quyết định nên thay đổi đỉnh nào trên đó. Điều này đúng vì khi một đường dẫn được cố định, lựa chọn duy nhất còn lại là các giá trị mới của nhiều nhất`k`đỉnh. Tuy nhiên, ngay cả một đồ thị chỉ có 2000 đỉnh cũng có thể có vô số đường đi khác nhau, do đó phương pháp này có đặc tính hàm mũ và không thể sử dụng được. 

Cách tốt hơn để xem xét vấn đề là quên các giá trị chính xác được gán cho các đỉnh đã thay đổi. Giả sử một đường dẫn chứa một số đỉnh mà chúng ta sửa đổi. Nếu chúng ta loại bỏ các đỉnh đã sửa đổi đó khỏi đường đi thì các đỉnh không thay đổi còn lại sẽ xuất hiện theo thứ tự ban đầu của chúng. Giữa hai đỉnh liên tiếp không thay đổi, tất cả các đỉnh bị loại bỏ có thể được gán các giá trị nằm giữa hai giá trị ban đầu, do đó toàn bộ đoạn có chi phí chính xác là chênh lệch tuyệt đối của hai điểm cuối không thay đổi. 

Ví dụ, đường dẫn```
A - X - Y - B
```Ở đâu`X`Và`Y`được thay đổi, có chi phí tối thiểu:```
|a[A] - a[B]|
```bởi vì`X`Và`Y`có thể được chọn giữa hai giá trị điểm cuối. 

Điều này chuyển vấn đề thành một vấn đề trong đó chúng ta di chuyển giữa các đỉnh không thay đổi và dành một đơn vị ngân sách sửa đổi cho mỗi đỉnh bị bỏ qua. Từ`k`chỉ là 10, chúng ta chỉ cần biết những con đường bỏ qua nhiều nhất`k`đỉnh. 

Chúng tôi tạo ra sự chuyển tiếp giữa các đỉnh có thể không thay đổi. Một sự chuyển tiếp từ`u`ĐẾN`v`tồn tại nếu có một đường dẫn đồ thị từ`u`ĐẾN`v`các đỉnh bên trong của nó chính xác là các đỉnh đã được sửa đổi. Nếu đường dẫn có`d`các cạnh, nó tiêu thụ`d-1`sửa đổi và chi phí`|a_u-a_v|`. 

Việc bắt đầu và kết thúc yêu cầu xử lý đặc biệt vì bản thân chúng có thể được sửa đổi. Chúng tôi thêm hai đỉnh ảo. Bắt đầu ảo thể hiện tình huống trong đó tiền tố của đường đi trước đỉnh không thay đổi đầu tiên đã bị thay đổi. Kết thúc ảo đại diện cho hậu tố sau khi đỉnh không thay đổi cuối cùng được thay đổi. Những chuyển đổi này có chi phí bằng 0 và tiêu tốn số đỉnh bị bỏ qua. 

Sau khi tạo biểu đồ mở rộng nhỏ này, vấn đề còn lại là vấn đề đường đi ngắn nhất với tài nguyên bổ sung: có bao nhiêu sửa đổi đã được sử dụng. Vì tài nguyên tối đa là 10 nên chúng tôi có thể chạy Dijkstra trên các trạng thái`(vertex, used_changes)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng đường dẫn | O(n) | Quá chậm | 
| Tối ưu | O(n(n+m) + nk^2) | O(nk + n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy tìm kiếm theo chiều rộng từ mọi đỉnh ban đầu nhưng dừng lại sau chiều sâu`k + 1`. 

Chúng ta chỉ quan tâm đến những đường đi chứa nhiều nhất`k`các đỉnh bên trong được sửa đổi. Đoạn đường đi ngắn nhất dài hơn không bao giờ có thể được sử dụng làm một quá trình chuyển đổi giữa các đỉnh không thay đổi. 
2. Với mọi cặp đỉnh ban đầu`u`Và`v`được BFS phát hiện từ`u`, tạo chuyển tiếp nếu khoảng cách ít nhất là một cạnh. 

Nếu khoảng cách là`d`, tất cả`d - 1`các đỉnh bên trong có thể được thay đổi. Việc chuyển đổi sử dụng`d - 1`thay đổi và có chi phí`|a_u - a_v|`. 
3. Thêm một đỉnh bắt đầu ảo. 

Từ điểm bắt đầu ảo, chúng ta có thể đạt tới bất kỳ đỉnh ban đầu nào`v`bằng cách thay đổi mọi đỉnh trước`v`trên một đường đi nào đó từ đỉnh`1`. Nếu khoảng cách từ đỉnh`1`ĐẾN`v`là`d`, quá trình chuyển đổi này tiêu tốn`d`thay đổi và chi phí bằng không. 
4. Thêm một đỉnh kết thúc ảo. 

Từ một đỉnh ban đầu`u`, chúng ta có thể kết thúc bằng cách thay đổi tất cả các đỉnh sau`u`trên đường đi tới đỉnh`n`. Nếu khoảng cách là`d`, quá trình chuyển đổi tiêu thụ`d`thay đổi và chi phí bằng không. 
5. Chạy Dijkstra trên các trạng thái`(vertex, number_of_changes_used)`. 

Một trạng thái lưu trữ chi phí di chuyển tối thiểu sau khi đạt đến một đỉnh trong khi chi tiêu chính xác số lần sửa đổi đó. Việc chuyển đổi chỉ được chấp nhận khi số lần sửa đổi mới không vượt quá`k`. 
6. Câu trả lời là khoảng cách tối thiểu để về đích ảo trên tất cả các trạng thái có nhiều nhất`k`các sửa đổi được sử dụng. 

Tại sao nó hoạt động: 

Điều bất biến là mọi trạng thái Dijkstra thể hiện một cách tối ưu để đạt tới một đỉnh sau khi chọn chính xác các đỉnh trước khi nó được sửa đổi. Bất cứ khi nào một phân đoạn đã sửa đổi được vượt qua, thông tin duy nhất quan trọng là hai điểm cuối không thay đổi, bởi vì tất cả các giá trị thay đổi nội bộ đều có thể được chọn để làm cho chi phí của phân đoạn đó chênh lệch tuyệt đối giữa các điểm cuối đó. Các chuyển đổi do BFS tạo đại diện cho mọi phân đoạn có thể sử dụng ngân sách sửa đổi có sẵn. Vì Dijkstra khám phá tất cả các chuyển đổi như vậy và giữ chi phí rẻ nhất cho mọi khoản ngân sách được sử dụng nên nó không thể loại bỏ giải pháp hợp lệ hơn. 

## Giải pháp Python```python
import sys
import heapq
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    INF = 10**30

    jumps = [[] for _ in range(n)]

    for s in range(n):
        dist = [-1] * n
        q = deque([s])
        dist[s] = 0

        while q:
            v = q.popleft()
            if dist[v] == k + 1:
                continue
            for u in g[v]:
                if dist[u] == -1:
                    dist[u] = dist[v] + 1
                    q.append(u)

        for v in range(n):
            if v != s and dist[v] != -1 and dist[v] <= k + 1:
                jumps[s].append((v, dist[v] - 1, abs(a[s] - a[v])))

    start = n
    finish = n + 1
    total = n + 2

    jumps.extend([[], []])

    # From virtual start to first unchanged vertex.
    dist = [-1] * n
    q = deque([0])
    dist[0] = 0
    while q:
        v = q.popleft()
        if dist[v] == k:
            continue
        for u in g[v]:
            if dist[u] == -1:
                dist[u] = dist[v] + 1
                q.append(u)

    for v in range(n):
        if dist[v] != -1 and dist[v] <= k:
            jumps[start].append((v, dist[v], 0))

    # From unchanged vertex to virtual finish.
    dist = [-1] * n
    q = deque([n - 1])
    dist[n - 1] = 0
    while q:
        v = q.popleft()
        if dist[v] == k:
            continue
        for u in g[v]:
            if dist[u] == -1:
                dist[u] = dist[v] + 1
                q.append(u)

    for v in range(n):
        if dist[v] != -1 and dist[v] <= k:
            jumps[v].append((finish, dist[v], 0))

    # The whole path can consist of changed vertices.
    if dist[0] != -1 and dist[0] + 1 <= k:
        jumps[start].append((finish, dist[0] + 1, 0))

    d = [[INF] * (k + 1) for _ in range(total)]
    d[start][0] = 0

    pq = [(0, start, 0)]

    while pq:
        cur, v, used = heapq.heappop(pq)
        if cur != d[v][used]:
            continue

        for u, add_used, cost in jumps[v]:
            nu = used + add_used
            if nu <= k and cur + cost < d[u][nu]:
                d[u][nu] = cur + cost
                heapq.heappush(pq, (d[u][nu], u, nu))

    print(min(d[finish]))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý BFS xây dựng mọi đoạn đường dẫn nén hữu ích. Giới hạn độ sâu là`k + 1`bởi vì sự chuyển tiếp giữa các đỉnh không thay đổi có thể chứa nhiều nhất`k`các đỉnh bên trong bị thay đổi. 

Biểu đồ mở rộng lưu trữ các chuyển đổi dưới dạng`(destination, extra changes, cost)`. Pha Dijkstra không cần biết giá trị thực tế được gán cho các đỉnh được sửa đổi. Nó chỉ theo dõi có bao nhiêu sửa đổi đã được thực hiện. 

Các đỉnh ảo là thứ xử lý việc thay đổi điểm cuối. Nếu không có chúng, lời giải sẽ giả định không chính xác rằng các đỉnh`1`Và`n`phải giữ nguyên giá trị ban đầu của chúng. 

Tất cả các chi phí có thể đạt khoảng`10^9 * n`, vì vậy số nguyên Python được sử dụng một cách tự nhiên. Thuật toán không bao giờ xây dựng một mật độ dày đặc`n x n`ma trận, giúp quản lý việc sử dụng bộ nhớ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6 7 1
1 15 5 10 4 10
1 2
2 3
3 6
1 4
4 5
5 6
2 5
```Một đường dẫn nén hữu ích là`1 -> 2 -> 5 -> 6`. 

| Bước | Phân khúc hiện tại | Những thay đổi được sử dụng | Chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu | tại đỉnh 1 | 0 | 0 | 
| Di chuyển đến đỉnh 2 | giữ 1 và 2 làm điểm cuối | 0 | 14 | 
| Thay đổi đỉnh 2 | thay thế giá trị 15 bằng 3 | 1 | 14 | 
| Di chuyển qua 5 | nén chuyển tiếp 2 đến 5 | 1 | 2 | 
| Kết thúc lúc 6 | đỉnh cuối cùng không thay đổi | 1 | 9 | 

Phần quan trọng là đỉnh đã thay đổi không được lưu trữ với giá trị đã chọn trong thuật toán. Quá trình chuyển đổi trực tiếp thu được kết quả tốt nhất có thể. 

Đối với mẫu thứ hai:```
6 7 3
1 15 5 10 4 10
1 2
2 3
3 6
1 4
4 5
5 6
2 5
```Toàn bộ tuyến đường có thể được thay đổi. 

| Bước | Tiểu bang | Những thay đổi được sử dụng | Chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu | khởi đầu ảo | 0 | 0 | 
| Thay đổi đỉnh trước khi kết thúc | đi theo một đường đi chỉ chứa các đỉnh đã thay đổi | 3 | 0 | 
| Kết thúc | kết thúc ảo | 3 | 0 | 

Dấu vết xác nhận rằng khởi đầu và kết thúc ảo xử lý chính xác các trường hợp trong đó tất cả các đỉnh hữu ích đều được sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n(n+m) + nk^2) | Chúng tôi chạy BFS giới hạn từ mọi đỉnh và sau đó chạy Dijkstra`O(nk)`tiểu bang. | 
| Không gian | O(nk + n + m) | Biểu đồ trạng thái mở rộng và dữ liệu BFS phù hợp trong giới hạn. | 

Biểu đồ chỉ có 2000 đỉnh và giới hạn sửa đổi là 10. Giới hạn độ sâu BFS ngăn quá trình tiền xử lý trở thành phép tính đường đi ngắn nhất đầy đủ cho tất cả các cặp. 

## Trường hợp thử nghiệm```python
import sys, io, heapq
from collections import deque

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        g = [[] for _ in range(n)]
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        INF = 10**30
        jumps = [[] for _ in range(n)]

        for s in range(n):
            dist = [-1] * n
            q = deque([s])
            dist[s] = 0
            while q:
                v = q.popleft()
                if dist[v] == k + 1:
                    continue
                for u in g[v]:
                    if dist[u] == -1:
                        dist[u] = dist[v] + 1
                        q.append(u)
            for v in range(n):
                if v != s and dist[v] != -1 and dist[v] <= k + 1:
                    jumps[s].append((v, dist[v] - 1, abs(a[s] - a[v])))

        start, finish = n, n + 1
        jumps.extend([[], []])

        dist = [-1] * n
        q = deque([0])
        dist[0] = 0
        while q:
            v = q.popleft()
            if dist[v] == k:
                continue
            for u in g[v]:
                if dist[u] == -1:
                    dist[u] = dist[v] + 1
                    q.append(u)
        for v in range(n):
            if dist[v] != -1 and dist[v] <= k:
                jumps[start].append((v, dist[v], 0))

        dist = [-1] * n
        q = deque([n - 1])
        dist[n - 1] = 0
        while q:
            v = q.popleft()
            if dist[v] == k:
                continue
            for u in g[v]:
                if dist[u] == -1:
                    dist[u] = dist[v] + 1
                    q.append(u)
        for v in range(n):
            if dist[v] != -1 and dist[v] <= k:
                jumps[v].append((finish, dist[v], 0))

        if dist[0] != -1 and dist[0] + 1 <= k:
            jumps[start].append((finish, dist[0] + 1, 0))

        d = [[INF] * (k + 1) for _ in range(n + 2)]
        d[start][0] = 0
        pq = [(0, start, 0)]

        while pq:
            cur, v, used = heapq.heappop(pq)
            if cur != d[v][used]:
                continue
            for u, add, cost in jumps[v]:
                if used + add <= k and cur + cost < d[u][used + add]:
                    d[u][used + add] = cur + cost
                    heapq.heappush(pq, (d[u][used + add], u, used + add))

        return str(min(d[finish])) + "\n"

    res = solve()
    sys.stdin = old
    return res

assert run("""6 7 1
1 15 5 10 4 10
1 2
2 3
3 6
1 4
4 5
5 6
2 5
""") == "9\n"

assert run("""6 7 3
1 15 5 10 4 10
1 2
2 3
3 6
1 4
4 5
5 6
2 5
""") == "0\n"

assert run("""2 1 0
5 20
1 2
""") == "15\n"

assert run("""2 1 2
100 200
1 2
""") == "0\n"

assert run("""3 2 1
1 100 10
1 2
2 3
""") == "9\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 9 | Chuyển đổi nén cơ bản với một sửa đổi | 
| Mẫu 2 | 0 | Toàn bộ đường dẫn có thể được sửa đổi | 
| Hai đỉnh,`k=0`| 15 | Trường hợp ranh giới không sửa đổi | 
| Hai đỉnh,`k=2`| 0 | Thay đổi điểm cuối | 
| Chuỗi ba đỉnh | 9 | Thay thế đỉnh bên trong | 

## Vỏ cạnh 

cho`k = 0`, quá trình tiền xử lý chỉ tạo ra các chuyển tiếp không có đỉnh bị bỏ qua. Các trạng thái Dijkstra sụp đổ thành các trạng thái đường đi ngắn nhất thông thường, vì vậy câu trả lời là đường đi ngắn nhất có trọng số ban đầu. 

Đối với đường đi mà điểm bắt đầu hoặc kết thúc bị thay đổi, các đỉnh ảo sẽ cung cấp các điểm cuối còn thiếu. Ví dụ:```
2 1 2
100 200
1 2
```Quá trình chuyển đổi trực tiếp từ khởi đầu ảo sang kết thúc ảo cần có hai thay đổi và không tốn phí. Điều này thể hiện việc thay đổi cả hai đỉnh thành cùng một giá trị. 

Đối với các đường dẫn có nhiều đỉnh thay đổi liên tiếp, quá trình chuyển đổi BFS sẽ xóa toàn bộ đoạn cùng một lúc. TRONG:```
4 3 2
1 100 200 10
1 2
2 3
3 4
```thay đổi đỉnh`2`Và`3`để lại các điểm cuối với chi phí:```
|1 - 10| = 9
```Thuật toán đạt được sự chuyển đổi nén tương tự từ đỉnh`1`đến đỉnh`4`sử dụng hai sửa đổi và không bao giờ cần đoán giá trị mới của các đỉnh`2`Và`3`.
