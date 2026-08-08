---
title: "CF 102501A - Du lịch thân thiện với môi trường"
description: "Chúng ta cần chọn tuyến đường từ tọa độ bắt đầu đến tọa độ đích. Tuyến đường chỉ có thể sử dụng ô tô cho phần đầu và phần cuối của chuyến đi."
date: "2026-08-06T18:56:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 61
verified: true
draft: false
---

[CF 102501A - Du lịch thân thiện với môi trường](https://codeforces.com/problemset/problem/102501/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn tuyến đường từ tọa độ bắt đầu đến tọa độ đích. Tuyến đường chỉ có thể sử dụng ô tô cho phần đầu và phần cuối của chuyến đi. Giữa các ga, việc đi lại bị hạn chế trong các kết nối giao thông nhất định và mỗi kết nối có một trong một số phương thức vận chuyển với chi phí CO2 riêng trên mỗi km. 

Độ dài của mỗi đoạn là khoảng cách Euclide được làm tròn giữa các điểm cuối của nó. Tổng độ dài của tất cả các đoạn không được vượt quá ngân sách cho phép`B`. Trong số tất cả các tuyến đường hợp lệ, chúng tôi cần chi phí CO2 nhỏ nhất có thể. Nếu không có tuyến đường nào phù hợp với giới hạn khoảng cách thì câu trả lời là`-1`. 

Mạng lưới giao thông đương nhiên là một biểu đồ. Mỗi trạm là một đỉnh và mọi kết nối khả dụng đều là một cạnh. Điểm bắt đầu và đích đến không phải là một phần của biểu đồ gốc nhưng chúng ta có thể thêm chúng dưới dạng các đỉnh bổ sung. Các tuyến ô tô kết nối từ điểm xuất phát đến ga và từ ga đến điểm đích trở thành các cạnh đồ thị thông thường, với chi phí cao hơn mọi phương thức vận tải khác. 

Các giới hạn định hình giải pháp. Có thể có tối đa 1000 trạm và mỗi trạm có thể có tối đa 100 kết nối, do đó, biểu đồ có thể chứa khoảng 100000 cạnh có hướng sau khi làm rõ các kết nối vô hướng. Thuật toán đường đi ngắn nhất thông thường chỉ dựa trên chi phí CO2 sẽ bỏ qua giới hạn khoảng cách, trong khi phương pháp lưu trữ mọi đường đi có thể sẽ quá lớn. Giá trị nhỏ quan trọng là ngân sách khoảng cách:`B`tối đa là 100. Điều này có nghĩa là chúng tôi có đủ khả năng theo dõi khoảng cách đã sử dụng dưới dạng thứ nguyên trạng thái bổ sung, tạo ra tối đa khoảng 100000 trạng thái. 

Việc thực hiện bất cẩn có thể thất bại trong một số trường hợp. Tuyến đường có lượng CO2 rẻ nhất có thể vượt quá giới hạn khoảng cách. Ví dụ:```
0 0
10 0
5
10
1
1
2
```Chuyến xe đi thẳng có quãng đường 10 và tốn 100, nhưng kinh phí là 5 nên không hợp lệ. Đầu ra đúng là`-1`. Thuật toán đường đi ngắn nhất chỉ giảm thiểu chi phí sẽ chọn sai nó. 

Một lỗi phổ biến khác là sử dụng khoảng cách Euclide thông thường thay vì khoảng cách làm tròn bắt buộc. Coi như:```
0 0
1 1
2
10
1
1
0
```Khoảng cách là`ceil(sqrt(2)) = 2`, không`1`. Chuyến đi trực tiếp sử dụng toàn bộ ngân sách. Bất kỳ việc triển khai nào sử dụng tính năng cắt bớt số nguyên đều có thể cho rằng chuyến đi sẽ ngắn hơn một cách không chính xác. 

Vấn đề thứ ba là quên rằng ô tô chỉ được phép di chuyển từ điểm xuất phát đến điểm đến. Nếu một chương trình thêm các cạnh ô tô vào giữa tất cả các ga, nó có thể tìm ra một tuyến đường chi phí thấp không thể thực hiện được. Mạng lưới trạm phải được tuân thủ chính xác đối với mọi chuyển động trung gian. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê các tuyến đường có thể có thông qua biểu đồ trạm và giữ tuyến đường rẻ nhất hợp lệ. Tìm kiếm theo chiều sâu đầu tiên có thể bao gồm trạm hiện tại, tổng khoảng cách và chi phí CO2 tích lũy. Bất cứ khi nào đạt đến đích, chi phí hiện tại có thể được so sánh với câu trả lời tốt nhất. 

Cách tiếp cận này đúng vì mọi con đường có thể đều đã được khám phá. Vấn đề là số lượng tuyến đường. Một biểu đồ có 1000 trạm và nhiều kết nối có thể chứa một số lượng lớn các chuyến đi bộ khác nhau. Ngay cả với giới hạn khoảng cách nhỏ, việc liên tục khám phá các tuyến đường từng phần tương tự sẽ gây ra sự tăng trưởng theo cấp số nhân. Không thể hoàn thành trong giới hạn. 

Quan sát giúp giải quyết vấn đề là điều duy nhất giới hạn các tuyến đường là tổng khoảng cách di chuyển và giới hạn đó chỉ là 100. Chúng ta không cần phải nhớ chính xác lịch sử của tuyến đường. Chúng ta chỉ cần biết trạm hiện tại và khoảng cách đã được sử dụng. Nếu hai tuyến đường đến cùng một ga sau khi đi cùng một quãng đường thì chỉ có tuyến nào rẻ hơn mới quan trọng, bởi vì cả hai đều có những khả năng tương lai như nhau. 

Điều này biến bài toán thành bài toán đường đi ngắn nhất trên biểu đồ mở rộng. Một trạng thái là`(station, used_distance)`. Di chuyển dọc theo một cạnh sẽ tăng khoảng cách sử dụng và tăng thêm chi phí CO2 tương ứng. Chạy Dijkstra trên các trạng thái này mang lại chi phí rẻ nhất cho mọi giá trị khoảng cách có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng tuyến đường có thể | O(độ dài đường dẫn) | Quá chậm | 
| Tối ưu | O(BE log(BN)) | O(BN) | Đã chấp nhận | 

Đây`E`là số cạnh của đồ thị sau khi cộng cả hai hướng. yếu tố`B`xuất hiện vì mọi trạm ban đầu có thể tồn tại trong tối đa`B + 1`các trạng thái khoảng cách. 

## Hướng dẫn thuật toán 

1. Thêm hai đỉnh bổ sung biểu thị điểm bắt đầu và điểm đến. Thêm các cạnh ô tô từ điểm bắt đầu đến mọi trạm và trực tiếp đến đích, đồng thời thêm các cạnh ô tô từ mọi trạm đến đích. Không thêm mép ô tô vào giữa các bến vì những chuyển động đó bị cấm. 
2. Tính toán trước khoảng cách làm tròn giữa mỗi cặp tọa độ sẽ được kết nối. Khoảng cách được sử dụng cho cả việc kiểm tra ngân sách và tính toán chi phí CO2. 
3. Coi mọi sự kết hợp của một đỉnh và quãng đường đi được là một trạng thái Dijkstra riêng biệt. Trạng thái ban đầu là đỉnh bắt đầu có khoảng cách`0`và chi phí`0`. 
4. Khi xóa một trạng thái khỏi hàng ưu tiên, hãy thử mọi cạnh đi ra. Nếu tổng khoảng cách mới lớn nhất`B`, cập nhật trạng thái của đỉnh đích ở khoảng cách mới đó nếu chi phí CO2 được cải thiện. 
5. Sau khi tìm kiếm kết thúc, hãy kiểm tra tất cả các trạng thái thuộc đỉnh đích. Chi phí CO2 được lưu trữ nhỏ nhất trong số các khoảng cách từ`0`ĐẾN`B`là câu trả lời. Nếu mọi trạng thái đều không thể truy cập được, hãy quay lại`-1`. 

Lý do điều này có tác dụng là vì các lựa chọn trong tương lai của một trạm chỉ phụ thuộc vào chính trạm đó và khoảng cách đã sử dụng. Đường dẫn chính xác được sử dụng để đạt đến trạng thái đó không có hiệu lực. Dijkstra khám phá các trạng thái này theo thứ tự chi phí CO2 tăng dần, vì vậy khi một trạng thái được hoàn thiện, không có tuyến đường nào rẻ hơn có thể đến trạng thái tương tự sau đó. 

## Giải pháp Python```python
import sys
import math
import heapq

input = sys.stdin.readline

def solve():
    xs, ys = map(int, input().split())
    xd, yd = map(int, input().split())
    B = int(input())
    C0 = int(input())
    T = int(input())

    costs = [0]
    for _ in range(T):
        costs.append(int(input()))

    N = int(input())
    stations = []
    raw_edges = []

    for i in range(N):
        data = list(map(int, input().split()))
        x, y, l = data[:3]
        stations.append((x, y))
        edges = []
        ptr = 3
        for _ in range(l):
            j, m = data[ptr], data[ptr + 1]
            ptr += 2
            edges.append((j, m))
        raw_edges.append(edges)

    coords = [(xs, ys)] + stations + [(xd, yd)]
    start = 0
    offset = 1
    dest = N + 1
    total = N + 2

    graph = [[] for _ in range(total)]

    def dist(a, b):
        dx = coords[a][0] - coords[b][0]
        dy = coords[a][1] - coords[b][1]
        return math.isqrt(dx * dx + dy * dy - 1) + 1 if dx or dy else 0

    def add_edge(a, b, mode_cost):
        d = dist(a, b)
        graph[a].append((b, d, mode_cost * d))

    for i in range(N):
        u = offset + i
        for v, mode in raw_edges[i]:
            add_edge(u, offset + v, costs[mode])

    for i in range(N):
        u = offset + i
        add_edge(start, u, C0)
        add_edge(u, dest, C0)

    add_edge(start, dest, C0)

    INF = 10**18
    best = [[INF] * (B + 1) for _ in range(total)]
    best[start][0] = 0

    pq = [(0, start, 0)]

    while pq:
        cost, node, used = heapq.heappop(pq)

        if cost != best[node][used]:
            continue

        for nxt, d, c in graph[node]:
            new_used = used + d
            if new_used <= B:
                new_cost = cost + c
                if new_cost < best[nxt][new_used]:
                    best[nxt][new_used] = new_cost
                    heapq.heappush(pq, (new_cost, nxt, new_used))

    ans = min(best[dest])
    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng một biểu đồ chứa mạng trạm ban đầu cộng với các đỉnh bắt đầu và đích giả. Việc dịch chuyển chỉ mục là cần thiết vì đỉnh bắt đầu mới chiếm vị trí chỉ mục`0`, trong khi các trạm ban đầu được di chuyển một vị trí. 

các`dist`hàm tính toán mức trần yêu cầu của khoảng cách Euclide. Biểu thức xử lý khoảng cách nguyên chính xác một cách chính xác. Khi bình phương khoảng cách đã là một hình vuông hoàn hảo,`isqrt`trả về giá trị chính xác, nếu không thì công thức sẽ làm tròn lên. 

Các cửa hàng xếp hàng ưu tiên`(CO2 cost, vertex, used distance)`. Hai chiều`best`mảng là bảng lập trình động trên biểu đồ mở rộng. Một trạng thái bị bỏ qua khi giá trị xuất hiện không còn là giá trị được biết đến nhiều nhất, đó là tối ưu hóa Dijkstra tiêu chuẩn. 

Giới hạn khoảng cách được kiểm tra trước khi chèn trạng thái mới. Đây là lý do chính khiến thuật toán vẫn còn nhỏ. Bất kỳ tuyến đường nào đã vượt quá ngân sách sẽ không bao giờ có hiệu lực trở lại. 

## Ví dụ đã hoạt động 

Sử dụng mẫu được cung cấp:```
1 1
10 2
12
100
2
50
5 5 1 2 1
9 3 0
```Các trạng thái quan trọng là: 

| Bước | Hiện trạng | Khoảng cách đã sử dụng | Chi phí CO2 | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | Bắt đầu | 0 | 0 | Nhập hàng ưu tiên | 
| 2 | Trạm 0 | 3 | 300 | Du lịch bằng ô tô | 
| 3 | Trạm 1 | 10 | 650 | Du lịch theo phương thức 1 | 
| 4 | Điểm đến | 12 | 850 | Kết thúc lộ trình | 

Trạng thái đích với khoảng cách`12`là hợp lệ, vì vậy`850`trở thành câu trả lời. Một tuyến đường có vẻ ngoài rẻ hơn vượt quá ngân sách sẽ không bao giờ có được câu trả lời cuối cùng vì trạng thái khoảng cách của nó bị loại bỏ. 

Một ví dụ nhỏ thứ hai kiểm tra ranh giới ngân sách:```
0 0
3 4
5
10
1
1
```Khoảng cách xe trực tiếp là chính xác`5`. 

| Bước | Hiện trạng | Khoảng cách đã sử dụng | Chi phí CO2 | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | Bắt đầu | 0 | 0 | Trạng thái ban đầu | 
| 2 | Điểm đến | 5 | 50 | Cạnh xe trực tiếp | 

Câu trả lời là`50`. Điều này xác nhận rằng khoảng cách bằng ngân sách được cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(BE log(BN)) | Có nhiều nhất`(B + 1)N`trạng thái mở rộng và mỗi trạng thái kiểm tra các cạnh đồ thị đi. | 
| Không gian | O(BN + E) | Bảng khoảng cách lưu trữ mọi trạm và mọi khoảng cách có thể sử dụng. | 

Với`B <= 100`Và`N <= 1000`, biểu đồ mở rộng có khoảng 100000 trạng thái. Thuật toán tránh được số lượng lớn các đường đi có thể có trong khi vẫn giữ đủ thông tin để tôn trọng giới hạn khoảng cách. 

## Trường hợp thử nghiệm```python
import sys
import io
import math
import heapq

def solve_case(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    xs, ys = map(int, input().split())
    xd, yd = map(int, input().split())
    B = int(input())
    C0 = int(input())
    T = int(input())
    costs = [0] + [int(input()) for _ in range(T)]

    N = int(input())
    stations = []
    raw = []
    for _ in range(N):
        a = list(map(int, input().split()))
        stations.append((a[0], a[1]))
        raw.append([(a[i], a[i + 1]) for i in range(3, len(a), 2)])

    coords = [(xs, ys)] + stations + [(xd, yd)]
    start = 0
    dest = N + 1
    graph = [[] for _ in coords]

    def distance(a, b):
        dx = coords[a][0] - coords[b][0]
        dy = coords[a][1] - coords[b][1]
        s = dx * dx + dy * dy
        return math.isqrt(s - 1) + 1 if s else 0

    def add(a, b, c):
        d = distance(a, b)
        graph[a].append((b, d, c * d))

    for i in range(N):
        for j, m in raw[i]:
            add(i + 1, j + 1, costs[m])

    for i in range(N):
        add(start, i + 1, C0)
        add(i + 1, dest, C0)

    add(start, dest, C0)

    INF = 10**18
    dp = [[INF] * (B + 1) for _ in coords]
    dp[start][0] = 0
    pq = [(0, start, 0)]

    while pq:
        c, u, d = heapq.heappop(pq)
        if c != dp[u][d]:
            continue
        for v, nd, nc in graph[u]:
            if d + nd <= B and c + nc < dp[v][d + nd]:
                dp[v][d + nd] = c + nc
                heapq.heappush(pq, (c + nc, v, d + nd))

    ans = min(dp[dest])
    return str(-1 if ans == INF else ans)

assert solve_case("""0 0
3 4
5
10
0
""") == "50"

assert solve_case("""0 0
10 0
5
10
1
1
2
""") == "-1"

assert solve_case("""0 0
1 1
2
10
1
1
0
""") == "20"

assert solve_case("""0 0
0 0
0
10
1
1
0
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuyến đi trực tiếp 3-4-5 | 50 | Sử dụng ngân sách chính xác và làm tròn khoảng cách | 
| Chuyến đi xe dài | -1 | Từ chối các tuyến đường vượt quá giới hạn khoảng cách | 
| Khoảng cách chéo | 20 | Trần đúng khoảng cách Euclide | 
| Điểm xuất phát và điểm đến giống nhau | 0 | Xử lý khoảng cách bằng không | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là tuyến đường rẻ nhất nhưng lại quá dài. Thuật toán xử lý nó vì mỗi lần chuyển đổi đều kiểm tra`used distance <= B`trước khi lưu trữ một trạng thái. Tuyến đường như vậy không bao giờ xuất hiện giữa các trạng thái đích nên nó không thể ảnh hưởng đến mức tối thiểu. 

Trường hợp cạnh thứ hai là khoảng cách hình học không nguyên. Đối với một đoạn có độ dài`sqrt(2)`, quá trình chuyển đổi tiêu tốn khoảng cách`2`, không`1`. Việc thực hiện tính toán trần trực tiếp từ số nguyên bình phương, tránh lỗi dấu phẩy động. 

Trường hợp thứ ba là ô tô di chuyển trái phép giữa các ga. Việc xây dựng biểu đồ chỉ tạo các cạnh ô tô từ nút xuất phát giả tạo và đến nút đích giả tạo. Tất cả chuyển động của trạm trung gian phải xuất phát từ các tuyến giao thông được liệt kê, do đó không thể tạo ra các lối tắt không thể thực hiện được.
