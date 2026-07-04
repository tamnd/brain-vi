---
title: "CF 103081I - Email"
description: "Chúng ta có một tập hợp người được biểu diễn dưới dạng các nút trong biểu đồ vô hướng. Một cạnh giữa hai nút có nghĩa là ban đầu hai người đó biết địa chỉ email của nhau. Hệ thống phát triển theo các vòng đồng bộ."
date: "2026-07-04T00:24:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "I"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 57
verified: true
draft: false
---

[CF 103081I - Email](https://codeforces.com/problemset/problem/103081/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp người được biểu diễn dưới dạng các nút trong biểu đồ vô hướng. Một cạnh giữa hai nút có nghĩa là ban đầu hai người đó biết địa chỉ email của nhau. Hệ thống phát triển theo các vòng đồng bộ. Mỗi buổi sáng, mỗi người sẽ phát toàn bộ sổ địa chỉ hiện tại của mình cho tất cả hàng xóm. Mỗi tối, mỗi người gộp tất cả những gì nhận được trong ngày vào sổ địa chỉ của riêng mình. Khi một người không nhận được gì mới trong bản cập nhật buổi tối, họ sẽ ngừng tham gia. 

Câu hỏi không phải là mô phỏng trực tiếp quá trình mà là xác định xem cuối cùng mọi người có biết địa chỉ email của người khác hay không. Nếu có, chúng tôi cũng phải tính toán mất bao lâu cho đến khi quá trình lan truyền có ý nghĩa cuối cùng hoàn tất, được tính bằng ngày với quy ước cụ thể về việc chúng tôi tính lần cập nhật cuối cùng hay bước ổn định. 

Quy tắc giao tiếp về cơ bản là đóng cửa thông tin mang tính bắc cầu theo thời gian, nhưng chi tiết quan trọng là thông tin được truyền đi một bước nhảy mỗi ngày theo kiểu phân lớp, bởi vì các cập nhật chỉ được đưa vào buổi tối và chỉ được gửi vào sáng hôm sau. 

Biểu đồ đầu vào có tới 100.000 nút và 100.000 cạnh. Bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng việc truyền bá sổ địa chỉ đầy đủ đều không thể thực hiện được, bởi vì mỗi nút có thể tích lũy thông tin O(N) và mỗi ngày có thể tốn O(N + M) cho mỗi nút trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Điều này ngay lập tức buộc chúng ta phải suy luận ở cấp độ cấu trúc đồ thị và khoảng cách thay vì mô phỏng các tập hợp. 

Một quan sát quan trọng là quá trình này chỉ có ý nghĩa nếu đồ thị được kết nối. Nếu nó không được kết nối, sẽ không có đường truyền email giữa các thành phần, do đó, sẽ không có sự kết hợp lặp đi lặp lại nào có thể kết nối các thành phần được kết nối khác nhau. 

Trường hợp cạnh tinh tế xuất hiện khi biểu đồ đã hoàn thành hoặc gần hoàn thành. Ví dụ: nếu mọi nút được kết nối với mọi nút khác thì về cơ bản quá trình sẽ kết thúc sau một vòng. Một “lũ đa nguồn” kiểu BFS ngây thơ có thể giả định không chính xác về thời gian bằng 0 hoặc chênh lệch từng cái một tùy thuộc vào việc liệu ngày ổn định cuối cùng có được tính hay không. Một trường hợp khác là biểu đồ không kết nối trong đó chỉ tồn tại hai thành phần: mặc dù trong mỗi thành phần mọi thứ đều hội tụ, nhưng trên toàn cầu thì không thể tiếp cận được sổ địa chỉ toàn cầu đầy đủ. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của quy trình là mô phỏng rõ ràng mỗi ngày. Mỗi nút duy trì một tập hợp các email đã biết. Vào mỗi ngày, mỗi nút sẽ gửi tập hợp đầy đủ của nó tới các nút lân cận, liên kết tất cả các tập hợp đã nhận và chúng tôi lặp lại cho đến khi không có tập hợp nào thay đổi. Mỗi phép toán hợp có thể tốn O(N) trong trường hợp xấu nhất nếu tập hợp lớn và nó xảy ra trên các cạnh O(M) mỗi ngày. Trong trường hợp xấu nhất, điều này dẫn đến hành vi O(D · N · M) trong đó D có thể là O(N), điều này hoàn toàn không khả thi đối với 100.000 nút. 

Thông tin chi tiết về cấu trúc quan trọng là quy trình này tương đương với việc tính toán khoảng cách đường đi ngắn nhất trong biểu đồ không có trọng số, nhưng có sự thay đổi trong cách xác định các vòng. Nếu chúng ta suy nghĩ cẩn thận, một mẩu thông tin sẽ truyền đi một cạnh mỗi ngày. Điều đó có nghĩa là nếu chúng ta sửa một nút bắt đầu, thời gian để email của nút đó đến được nút khác chính xác là khoảng cách đường đi ngắn nhất giữa chúng tính theo các cạnh. Vì cuối cùng thì mọi người đều phải biết email của người khác, điều quan trọng là phải mất bao lâu để thông tin được truyền đi trên đường kính của từng thành phần được kết nối.

Chính xác hơn, khi chúng ta xem xét một thành phần được kết nối duy nhất, thông tin sẽ lan truyền đồng thời như một làn sóng từ mọi nút. Thời điểm cuối cùng khi bất kỳ thông tin mới nào xuất hiện đều tương ứng với cấu trúc lệch tâm của biểu đồ. Quá trình hoàn tất khi mọi nút đều đến được mọi nút khác, do đó thời gian bị chi phối bởi khoảng cách đường đi ngắn nhất tối đa giữa hai nút bất kỳ trong cùng một thành phần được kết nối, tức là đường kính. 

Do đó, vấn đề giảm xuống còn việc tìm xem đồ thị có được kết nối hay không và nếu có thì tính đường kính của đồ thị được kết nối. Câu trả lời cuối cùng được rút ra từ đường kính này với một sự điều chỉnh nhỏ tùy thuộc vào việc chúng tôi đo ngày cập nhật cuối cùng hay ngày ổn định. 

Để tính đường kính trong biểu đồ không có trọng số chung, chúng tôi sử dụng hai lượt BFS cho mỗi thành phần: chọn bất kỳ nút nào, BFS để tìm nút xa nhất, sau đó lại BFS từ nút đó để có khoảng cách xa nhất. Giá trị tối đa như vậy trên các thành phần là đường kính. Nếu có nhiều thành phần, quy trình không bao giờ kết nối đầy đủ tất cả mọi người, vì vậy câu trả lời là -1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(D · N · M) | O(N2) tập hợp trường hợp xấu nhất | Quá chậm | 
| Đường kính BFS trên mỗi thành phần | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi đầu vào thành biểu diễn danh sách kề của đồ thị vô hướng. 

Trước tiên, chúng tôi xác định kết nối và chuẩn bị đo khoảng cách. 

1. Xây dựng danh sách kề của đồ thị từ M cạnh. Điều này là bắt buộc vì BFS cần truy cập lân cận nhanh và việc truyền tải danh sách cạnh sẽ quá chậm đối với các tìm kiếm lặp lại. 
2. Kiểm tra xem tất cả các nút có thuộc về một thành phần được kết nối duy nhất bằng BFS hoặc DFS hay không. Nếu chúng tôi tìm thấy nhiều hơn một thành phần, chúng tôi ngay lập tức trả về -1 vì không có chuỗi trao đổi cục bộ nào có thể kết nối các phần bị ngắt kết nối. 
3. Đối với mỗi thành phần được kết nối, chúng tôi tính toán đường kính của nó bằng cách sử dụng hai lượt BFS. Chúng tôi bắt đầu từ một nút tùy ý trong thành phần và chạy BFS để tìm nút u xa nhất từ ​​​​nó. Bước này xác định một điểm cuối của đường dẫn dài nhất trong thành phần đó. 
4. Chúng tôi chạy lại BFS bắt đầu từ u để tính khoảng cách tối đa đến bất kỳ nút nào trong cùng thành phần. Khoảng cách lớn nhất gặp phải chính là đường kính của bộ phận đó. 
5. Theo dõi đường kính tối đa trên tất cả các bộ phận. Nếu đồ thị được kết nối, đây chỉ đơn giản là đường kính tổng thể. 
6. Chuyển đổi đường kính thành số ngày cần thiết theo quy tắc thời gian của quy trình. Vì mỗi ngày tương ứng với sự lan truyền đầy đủ của một lớp cạnh và quá trình bắt đầu với kiến ​​thức ban đầu đã có vào sáng ngày 0, số ngày cho đến lần cập nhật cuối cùng là đường kính. 

### Tại sao nó hoạt động 

Bất biến chính là sau k ngày đầy đủ, mọi nút đều biết chính xác thông tin bắt nguồn từ k cạnh khoảng cách trong biểu đồ. Điều này xảy ra bởi vì mỗi ngày đều mở rộng kiến ​​thức chính xác bằng một bước nhảy dọc theo tất cả các khía cạnh cùng một lúc. Do đó, tập hợp các email đã biết tại nút v sau k ngày tương ứng chính xác với tất cả các nút trong khoảng cách k của v. 

Khi k đạt đến độ lệch tâm của một nút, nút đó sẽ ngừng thay đổi. Sự thay đổi toàn cục cuối cùng xảy ra khi k đạt đến độ lệch tâm tối đa trên tất cả các nút, chính xác là đường kính đồ thị. Do đó, không nút nào có thể nhận thông tin mới sau các bước đường kính và một số nút vẫn phải nhận thông tin cho đến thời điểm đó để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start, adj):
    n = len(adj) - 1
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    far = start

    while q:
        v = q.popleft()
        for to in adj[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)
                if dist[to] > dist[far]:
                    far = to
    return far, dist

def bfs_dist(start, adj):
    n = len(adj) - 1
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    far = 0

    while q:
        v = q.popleft()
        for to in adj[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)
                if dist[to] > dist[far]:
                    far = to
    return dist

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    visited = [False] * (n + 1)
    components = 0
    diameter = 0

    for i in range(1, n + 1):
        if not visited[i]:
            components += 1
            q = deque([i])
            visited[i] = True
            comp_nodes = [i]

            while q:
                v = q.popleft()
                for to in adj[v]:
                    if not visited[to]:
                        visited[to] = True
                        q.append(to)
                        comp_nodes.append(to)

            if components > 1:
                print(-1)
                return

            start = i
            far, _ = bfs(start, adj)
            dist = bfs_dist(far, adj)
            diameter = max(diameter, max(dist))

    print(diameter)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng danh sách kề, hỗ trợ việc truyền tải tuyến tính của các lân cận theo thời gian tuyến tính. Kiểm tra thành phần dựa trên BFS đảm bảo chúng tôi phát hiện sớm tình trạng ngắt kết nối. Điều này là bắt buộc vì không thể tồn tại đường dẫn liên lạc giữa các thành phần. 

Đối với mỗi thành phần được kết nối, chúng tôi tính toán đường kính của nó bằng kỹ thuật hai BFS tiêu chuẩn. BFS đầu tiên xác định nút xa nhất từ ​​​​điểm bắt đầu tùy ý, đóng vai trò là điểm cuối của đường dẫn dài nhất trong thành phần đó. BFS thứ hai từ điểm cuối đó tính toán cấu trúc độ lệch tâm thực sự, đưa ra khoảng cách tối đa trong thành phần đó. 

Câu trả lời cuối cùng là đường kính vì mỗi đơn vị khoảng cách tương ứng với một ngày truyền sóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2
2 3
3 4
```Đây là một chuỗi duy nhất. 

Chúng tôi đầu tiên BFS từ nút 1: 

| Bước | Nút | Khoảng cách | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 1 | 
| 3 | 3 | 2 | 
| 4 | 4 | 3 | 

Nút xa nhất là 4. 

Bây giờ BFS từ 4: 

| Bước | Nút | Khoảng cách | 
| --- | --- | --- | 
| 1 | 4 | 0 | 
| 2 | 3 | 1 | 
| 3 | 2 | 2 | 
| 4 | 1 | 3 | 

Đường kính là 3 nên sau 3 ngày không có thông tin mới nào có thể lan truyền được. 

Điều này tương ứng với khoảng cách chuỗi dài nhất trong biểu đồ. 

### Ví dụ 2 

đầu vào:```
6 3
1 2
3 4
5 6
```Biểu đồ này có ba thành phần bị ngắt kết nối. 

Trong quá trình quét thành phần, chúng tôi gặp phải thành phần thứ hai sau khi hoàn thành thành phần đầu tiên. Vì có nhiều thành phần tồn tại nên quy trình không bao giờ có thể thống nhất tất cả các bộ email. 

Đầu ra là:```
-1
```Điều này khẳng định rằng khả năng kết nối là một yêu cầu nghiêm ngặt để hội tụ đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi BFS truy cập các nút và cạnh một lần và chúng tôi thực hiện số lần chạy BFS không đổi cho mỗi thành phần | 
| Không gian | O(N + M) | Danh sách kề cộng với hàng đợi BFS và mảng khoảng cách | 

Các ràng buộc cho phép truyền tải đồ thị theo thời gian tuyến tính, do đó phương pháp này phù hợp thoải mái trong giới hạn ngay cả đối với 100.000 nút và cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def bfs(start, adj):
        n = len(adj) - 1
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        far = start
        while q:
            v = q.popleft()
            for to in adj[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)
                    if dist[to] > dist[far]:
                        far = to
        return far

    def bfs_dist(start, adj):
        n = len(adj) - 1
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        while q:
            v = q.popleft()
            for to in adj[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)
        return dist

    def solve():
        n, m = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        visited = [False] * (n + 1)
        components = 0
        diameter = 0

        for i in range(1, n + 1):
            if not visited[i]:
                components += 1
                q = deque([i])
                visited[i] = True
                while q:
                    v = q.popleft()
                    for to in adj[v]:
                        if not visited[to]:
                            visited[to] = True
                            q.append(to)

                if components > 1:
                    return "-1"

                far = bfs(i, adj)
                dist = bfs_dist(far, adj)
                diameter = max(diameter, max(dist))

        return str(diameter)

    return solve()

# provided samples
assert run("4 3\n1 2\n2 3\n3 4\n") == "3"
assert run("6 3\n1 2\n3 4\n5 6\n") == "-1"

# custom cases
assert run("2 1\n1 2\n") == "1", "minimum chain"
assert run("5 4\n1 2\n2 3\n3 4\n4 5\n") == "4", "line graph"
assert run("3 3\n1 2\n2 3\n1 3\n") == "1", "complete graph"
assert run("4 2\n1 2\n3 4\n") == "-1", "two components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 2 nút | 1 | trường hợp kết nối nhỏ nhất | 
| Đường 5 nút | 4 | lan truyền đường dài | 
| tam giác | 1 | đồ thị dày đặc hội tụ sớm | 
| hai cặp | -1 | xử lý ngắt kết nối | 

## Vỏ cạnh 

Biểu đồ bị ngắt kết nối được xử lý ở giai đoạn phát hiện thành phần. Khi quét các nút, thuật toán sẽ tăng bộ đếm thành phần mỗi khi nó tìm thấy một nút chưa được truy cập. Nếu bộ đếm này vượt quá một, nó sẽ ngay lập tức trả về -1. Ví dụ, đầu vào`1-2`Và`3-4`tạo thành hai thành phần. BFS chỉ đánh dấu các nút trong mỗi thành phần và khám phá thứ hai sẽ kích hoạt việc chấm dứt trước khi thực hiện bất kỳ phép tính đường kính nào. 

Biểu đồ được kết nối đầy đủ có đường kính 1. BFS đầu tiên từ bất kỳ nút nào đến tất cả các nút khác trong một bước và BFS thứ hai xác nhận khoảng cách tối đa 1. Thuật toán trả về chính xác 1, khớp với trực giác rằng một vòng là đủ để trao đổi hoàn toàn. 

Một chuỗi tuyến tính là trường hợp xấu nhất cho việc lan truyền. Mỗi bước BFS mở rộng phạm vi tiếp cận thêm chính xác một nút, do đó đường kính bằng N-1. Thuật toán nắm bắt chính xác điều này vì điểm cuối xa nhất là điểm cuối của chuỗi và BFS thứ hai đo chiều dài đầy đủ.
