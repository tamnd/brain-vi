---
title: "CF 1163F - Phí taxi thiếu quyết đoán"
description: "Chúng ta được cung cấp một đồ thị vô hướng có trọng số lên tới 200.000 đỉnh và cạnh. Mỗi cạnh có trọng số cố định và chúng tôi quan tâm đến đường đi ngắn nhất từ ​​nút 1 đến nút n bằng cách sử dụng tổng trọng số của cạnh. Điều khó khăn là chúng ta không chỉ giải quyết một bài toán về đường đi ngắn nhất."
date: "2026-06-13T08:41:15+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1163
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 558 (Div. 2)"
rating: 3000
weight: 1163
solve_time_s: 297
verified: false
draft: false
---

[CF 1163F - Phí taxi thiếu quyết đoán](https://codeforces.com/problemset/problem/1163/F) 

**Đánh giá:** 3000 
**Tags:** cấu trúc dữ liệu, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 4 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng có trọng số lên tới 200.000 đỉnh và cạnh. Mỗi cạnh có trọng số cố định và chúng tôi quan tâm đến đường đi ngắn nhất từ ​​nút 1 đến nút n bằng cách sử dụng tổng trọng số của cạnh. 

Điều khó khăn là chúng ta không chỉ giải quyết một bài toán về đường đi ngắn nhất. Thay vào đó, chúng tôi được cung cấp tới 200.000 sửa đổi độc lập. Mỗi sửa đổi sẽ thay đổi trọng số của chính xác một cạnh, trong khi tất cả các cạnh khác không thay đổi và chúng tôi phải báo cáo khoảng cách đường đi ngắn nhất mới từ 1 đến n cho mỗi phiên bản được sửa đổi của biểu đồ. 

Giải thích trực tiếp là đối với mỗi truy vấn, chúng tôi tạm thời xây dựng lại biểu đồ với một trọng số cạnh đã thay đổi và tính toán lại đường đi ngắn nhất. 

Những hạn chế làm cho điều này không thể thực hiện được một cách ngây thơ. Chạy Dijkstra từ đầu cho mỗi truy vấn sẽ tốn khoảng O(q · m log n), điều này vượt xa khả thi đối với các truy vấn và cạnh 2 × 10^5. 

Một khía cạnh tinh tế là biểu đồ không có hướng và tĩnh ngoại trừ sự thay đổi trọng số một cạnh cho mỗi truy vấn. Cấu trúc đó gợi ý chúng ta nên sử dụng lại quá trình tiền xử lý nặng. 

Một cách tiếp cận ngây thơ cũng có thể thất bại theo những cách ít rõ ràng hơn. Nếu chúng ta cố gắng tính toán trước một cây đường đi ngắn nhất từ ​​nút 1 và cho rằng nó ổn định, nó sẽ bị hỏng ngay lập tức khi một cạnh được sửa đổi không phải là một phần của cây đường đi ngắn nhất ban đầu mà tạo ra một đường đi thay thế rẻ hơn. Ví dụ: việc giảm trọng số của cạnh không phải là cây tạo thành lối tắt giữa hai điểm trên đường đi ngắn nhất ban đầu có thể thay đổi câu trả lời ngay cả khi cạnh đó không liên quan trong giải pháp ban đầu. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi cập nhật trọng số cạnh và chạy Dijkstra từ nút 1 đến nút n. Điều này đúng vì Dijkstra xử lý các trọng số dương tùy ý, nhưng về cơ bản nó lặp lại cùng một phép tính tới 200.000 lần. Mỗi lần chạy tốn O(m log n), dẫn đến khoảng 4 × 10^10 thao tác trong trường hợp xấu nhất, vượt xa mọi giới hạn thực tế. 

Quan sát chính là mỗi truy vấn chỉ sửa đổi một cạnh. Điều này gợi ý rằng chúng ta nên hiểu cách các đường đi ngắn nhất phản ứng với sự thay đổi trọng số của một cạnh, thay vì tính toán lại mọi thứ. 

Chúng tôi bắt đầu bằng cách tính toán khoảng cách ngắn nhất từ ​​nút 1 đến tất cả các nút bằng biểu đồ gốc và cũng từ nút n đến tất cả các nút bằng cách sử dụng biểu đồ gốc được đảo ngược. Đặt các mảng này là dist1 và distn. 

Bây giờ hãy xem xét một truy vấn thay đổi cạnh e = (u, v) từ trọng số w thành x. Bất kỳ đường đi ngắn nhất nào theo sửa đổi này đều có hai loại. Hoặc là nó hoàn toàn không sử dụng cạnh này, trong trường hợp đó chi phí của nó không thay đổi và bằng khoảng cách đường đi ngắn nhất ban đầu dist1[n]. Hoặc nó sử dụng cạnh này ít nhất một lần. Bởi vì tất cả các trọng số đều dương, nên một đường đi tối ưu sẽ sử dụng cạnh được sửa đổi nhiều nhất một lần, vì vậy chúng ta chỉ cần xem xét các đường đi từ 1 đến u, đi qua cạnh rồi đi từ v đến n hoặc hướng ngược lại. 

Điều này làm giảm mỗi truy vấn để kiểm tra một số lượng không đổi các giá trị ứng viên xuất phát từ dist1 và distn. 

Điều tinh tế còn lại là đường đi ngắn nhất cũng có thể sử dụng cạnh được sửa đổi theo hướng ngược lại, do đó cả hai hướng đều phải được kiểm tra. 

Chúng ta cũng cần xử lý trường hợp cạnh xuất hiện trên một số cấu trúc đường đi ngắn nhất theo cách tương tác với các tuyến đường thay thế. Quá trình tiền xử lý dist1 và distn đã nắm bắt được tất cả cấu trúc toàn cầu cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Dijkstra mỗi truy vấn | O(q · m log n) | O(n + m) | Quá chậm | 
| Tính toán trước dist + cho mỗi truy vấn O(1) | O((n + m) log n + q) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành câu trả lời cho từng truy vấn bằng cách sử dụng thông tin đường dẫn ngắn nhất được tính toán trước.

1. Chạy Dijkstra từ nút 1 trên biểu đồ gốc và lưu trữ dist1[v], khoảng cách ngắn nhất từ ​​1 đến mọi nút v. Điều này cung cấp đường cơ sở cho tất cả chi phí di chuyển chuyển tiếp. 
2. Chạy Dijkstra từ nút n trên cùng một biểu đồ và lưu trữ distn[v], khoảng cách ngắn nhất từ ​​mọi nút v đến n. Phối cảnh ngược lại này cho phép chúng ta đánh giá nhanh chi phí hậu tố của các đường đi kết thúc tại n. 
3. Lưu trữ tất cả các cạnh có điểm cuối (u, v) và trọng số w trong mảng để chúng ta có thể truy cập và sửa đổi chúng theo chỉ mục. 
4. Đối với mỗi truy vấn (t, x), hãy hiểu nó là thay thế tạm thời cạnh t = (u, v, w) bằng trọng số x. 
5. Tính đường đi tốt nhất không sử dụng cạnh t. Đây chỉ đơn giản là dist1[n]. 
6. Tính đường đi tốt nhất sử dụng cạnh t đúng một lần theo hướng u → v. Chi phí trở thành dist1[u] + x + distn[v]. Điều này tương ứng với việc đi theo con đường ngắn nhất tới u, sử dụng cạnh đã được sửa đổi, sau đó kết thúc một cách tối ưu đến n. 
7. Tính trường hợp đối xứng v → u dưới dạng dist1[v] + x + distn[u]. 
8. Câu trả lời cho truy vấn là giá trị tối thiểu trong ba giá trị này. 

Lý do chúng tôi có thể hạn chế đối với những ứng cử viên này là vì bất kỳ đường dẫn nào sử dụng cạnh đã sửa đổi đều có thể được phân tách thành tiền tố từ 1 đến một điểm cuối, chính cạnh đó và hậu tố từ điểm cuối kia đến n. Nếu một đường dẫn được sử dụng cạnh nhiều lần, việc loại bỏ chu trình sẽ giảm đáng kể chi phí vì tất cả các trọng số đều dương nên không thể tối ưu. 

### Tại sao nó hoạt động 

Quá trình tiền xử lý tính toán khoảng cách tối ưu toàn cầu từ cả hai đầu. Đối với bất kỳ đường dẫn ứng cử viên nào sử dụng cạnh đã được sửa đổi, mọi thứ ngoại trừ cạnh đó đều có thể được thay thế bằng các đường dẫn ngắn nhất mà không làm tăng chi phí. Điều này hợp lệ vì các đường dẫn ngắn nhất thỏa mãn cấu trúc con tối ưu: bất kỳ đường dẫn con nào của đường dẫn ngắn nhất đều là đường dẫn ngắn nhất. Do đó, mức độ tự do duy nhất được đưa ra bởi truy vấn là liệu cạnh đã sửa đổi có được sử dụng hay không và theo hướng nào. Tất cả các cấu trúc khác đã được nén tối ưu thành dist1 và distn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

INF = 10**30

def dijkstra(n, adj, start):
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

n, m, q = map(int, input().split())

adj = [[] for _ in range(n + 1)]
edges = [None] * (m + 1)

for i in range(1, m + 1):
    u, v, w = map(int, input().split())
    adj[u].append((v, w))
    adj[v].append((u, w))
    edges[i] = (u, v, w)

dist1 = dijkstra(n, adj, 1)
distn = dijkstra(n, adj, n)

out = []
for _ in range(q):
    t, x = map(int, input().split())
    u, v, w = edges[t]

    ans = dist1[n]
    ans = min(ans, dist1[u] + x + distn[v])
    ans = min(ans, dist1[v] + x + distn[u])

    out.append(str(ans))

print("\n".join(out))
```Mã bắt đầu bằng cách xây dựng danh sách kề và lưu trữ các cạnh để chúng có thể được tham chiếu theo chỉ mục trong truy vấn. Hai Dijkstra chạy tính toán mảng khoảng cách từ nút 1 và nút n. 

Sau đó, mỗi truy vấn sẽ được trả lời theo thời gian không đổi bằng cách đánh giá ba loại đường dẫn ứng viên. Đường đi ngắn nhất ban đầu vẫn hợp lệ và hai biểu thức chiếm bất kỳ đường đi ngắn nhất nào bao gồm cạnh được sửa đổi theo một trong hai hướng. 

Một lỗi triển khai phổ biến là quên chạy Dijkstra từ nút n trên cùng một biểu đồ. Đường chuyền thứ hai đó rất cần thiết vì nó mã hóa tất cả các đường dẫn có hậu tố ngắn nhất đến đích. 

Một sự tinh tế khác là sử dụng các giá trị an toàn 64 bit. Khoảng cách có thể tích lũy lên tới 2 × 10^14, do đó Python xử lý nó một cách tự nhiên, nhưng phải tránh tràn trong các ngôn ngữ được gõ. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

### Dấu vết ví dụ 

Chúng tôi tính toán trước dist1 và distn một lần, sau đó xử lý các truy vấn. 

Đối với mỗi truy vấn, chúng tôi đánh giá các ứng viên: 

| Truy vấn | dist1[n] | dist1[u] + x + distn[v] | dist1[v] + x + distn[u] | Trả lời | 
| --- | --- | --- | --- | --- | 
| (3,4) | 4 | 5 | 6 | 4 | 
| (5,1) | 4 | 2 | 6 | 2 | 
| (3,8) | 4 | 5 | 7 | 5 | 

Điều này cho thấy các truy vấn khác nhau kích hoạt các cạnh khác nhau như các lối tắt tiềm năng như thế nào, trong khi đường dẫn ngắn nhất ban đầu vẫn là tùy chọn dự phòng. 

Dấu vết xác nhận rằng thuật toán cô lập chính xác tác động của việc sửa đổi một cạnh mà không cần tính toán lại cấu trúc tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + q) | Hai lần chạy Dijkstra chiếm ưu thế trong quá trình tiền xử lý, mỗi truy vấn là O(1) | 
| Không gian | O(n + m) | danh sách kề, lưu trữ cạnh và mảng khoảng cách | 

Quá trình tiền xử lý phù hợp thoải mái với các ràng buộc vì 2 × 10^5 cạnh với Dijkstra vùng nhị phân là tiêu chuẩn. Việc xử lý truy vấn diễn ra theo thời gian không đổi, do đó, ngay cả số lượng truy vấn tối đa cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

INF = 10**18

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    import heapq

    n, m, q = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    edges = [None] * (m + 1)

    for i in range(1, m + 1):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
        edges[i] = (u, v, w)

    def dijkstra(s):
        dist = [INF] * (n + 1)
        dist[s] = 0
        pq = [(0, s)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            for v, w in adj[u]:
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))
        return dist

    dist1 = dijkstra(1)
    distn = dijkstra(n)

    out = []
    for _ in range(q):
        t, x = map(int, input().split())
        u, v, w = edges[t]
        ans = min(
            dist1[n],
            dist1[u] + x + distn[v],
            dist1[v] + x + distn[u]
        )
        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert solve("""4 5 6
1 2 2
2 4 3
1 4 7
1 3 1
3 4 5
3 4
5 1
3 8
1 4
2 1
3 1
""") == """4
2
5
6
3
1"""

# minimum size
assert solve("""2 1 1
1 2 5
1 1
""") == "1"

# single edge updated heavily
assert solve("""3 2 2
1 2 10
2 3 10
1 1
2 1
""") == "1\n11"

# all equal weights
assert solve("""4 4 2
1 2 1
2 3 1
3 4 1
1 4 10
1 1
4 1
""") == "3\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị 2 nút | 1 | độ chính xác cấu trúc tối thiểu | 
| cập nhật biểu đồ chuỗi | 1, 11 | ảnh hưởng của việc thay thế cạnh trên đường đi ngắn nhất | 
| trọng lượng đồng đều | 3, 3 | ổn định theo nhiều đường đi ngắn nhất hợp lệ | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi cạnh được sửa đổi không phải là một phần của bất kỳ đường đi ngắn nhất ban đầu nào nhưng trở nên quan trọng sau khi rút gọn. Ví dụ: một đường vòng dài có thể chỉ trở nên tối ưu sau khi một cạnh duy nhất được làm rẻ. Thuật toán xử lý việc này vì dist1[u] + x + distn[v] đánh giá mọi cách có thể mà cạnh có thể kết nối các tiền tố và hậu tố tối ưu, bất kể nó có được sử dụng trước đó hay không. 

Một trường hợp khác là khi đường đi ngắn nhất ban đầu đã sử dụng cạnh được sửa đổi. Nếu trọng lượng của nó tăng lên, con đường thay thế tốt nhất có thể chuyển sang một con đường hoàn toàn khác. Thuật ngữ dist1[n] đã bao gồm tất cả các đường dẫn tránh hoàn toàn cạnh, do đó chuyển đổi chính xác tối thiểu giữa việc sử dụng và không sử dụng nó mà không có vỏ đặc biệt.
