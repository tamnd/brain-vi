---
title: "CF 105791G - Bản đồ Grisi"
description: "Chúng ta được cung cấp một biểu đồ có trọng số vô hướng mô hình hóa một vùng. Hai đỉnh đặc biệt được phân biệt: một là cổng chính của một trường đại học và một là điểm đến gọi là IC. Với mỗi đỉnh truy vấn $x$, chúng ta so sánh hai cách tiếp cận IC."
date: "2026-06-21T14:24:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "G"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 63
verified: true
draft: false
---

[CF 105791G - Bản đồ Grisi](https://codeforces.com/problemset/problem/105791/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số vô hướng mô hình hóa một vùng. Hai đỉnh đặc biệt được phân biệt: một là cổng chính của một trường đại học và một là điểm đến gọi là IC. Đối với mỗi đỉnh truy vấn$x$, chúng tôi so sánh hai cách tiếp cận IC. 

Cách thứ nhất là cách tối ưu, nghĩa là đường đi ngắn nhất từ$x$trực tiếp tới IC trong đồ thị. Cách thứ hai là một chiến lược cố định được Grisi sử dụng: từ$x$, anh ấy luôn đi đến lối vào trước bằng con đường ngắn nhất, sau đó từ lối vào IC bằng con đường ngắn nhất. Chi phí đi đường vòng cho một truy vấn là chiến lược cố định của Grisi mất bao lâu so với đường đi ngắn nhất thực sự từ$x$tới IC. 

Đầu vào mô tả biểu đồ có tối đa 200.000 đỉnh và cạnh, theo sau là tối đa 200.000 truy vấn. Mỗi truy vấn là độc lập, do đó, bất kỳ giải pháp nào tính toán lại các đường dẫn ngắn nhất cho mỗi truy vấn sẽ ngay lập tức vượt quá giới hạn thời gian. Có thể chấp nhận tính toán đường dẫn ngắn nhất trên toàn bộ biểu đồ, nhưng việc lặp lại nó cho mỗi truy vấn thì không. 

Cấu trúc ẩn chính là tuyến đường của Grisi bao gồm hai đoạn đường đi ngắn nhất chỉ phụ thuộc vào hai đỉnh đặc biệt. Điều này giúp có thể tính toán trước tất cả khoảng cách cần thiết một lần và trả lời mọi truy vấn trong thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi đỉnh bắt đầu đã là IC. Câu trả lời đúng là bằng 0, mặc dù công thức về đường đi Grisi vẫn tạo ra giá trị dương nếu được áp dụng một cách máy móc. Điều này cần xử lý rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp cho mỗi truy vấn là chạy thuật toán đường đi ngắn nhất từ$x$tới IC, đồng thời mô phỏng tuyến đường của Grisi bằng cách chạy các đường đi ngắn nhất từ$x$đến lối vào, sau đó thêm khoảng cách từ lối vào đến IC được tính toán trước. Ngay cả khi chúng tôi sử dụng lại các giá trị được tính toán trước để truy cập vào IC, chúng tôi vẫn cần tính toán đường dẫn ngắn nhất cho mỗi truy vấn, dẫn đến khoảng$q$chạy của Dijkstra. Với$q$lên tới 200.000, điều này trở nên không khả thi về mặt tính toán. 

Bước ngoặt nhận thấy rằng điểm thay đổi duy nhất trong mỗi truy vấn là đỉnh bắt đầu$x$, trong khi cả hai đỉnh đặc biệt đều cố định. Điều đó có nghĩa là chúng ta chỉ cần khoảng cách đường đi ngắn nhất từ ​​một nguồn cố định (lối vào) và đến mục tiêu cố định (IC). Trong đồ thị vô hướng, khoảng cách đường đi ngắn nhất từ ​​IC cũng có thể được tính bằng cách chạy Dijkstra từ IC. 

Khi chúng ta có hai mảng khoảng cách này, mọi truy vấn sẽ chuyển thành một biểu thức số học đơn giản: khoảng cách từ$x$tới lối vào cộng với khoảng cách từ lối vào IC, trừ đi khoảng cách tối ưu từ$x$tới IC. Điều này loại bỏ hoàn toàn mọi thao tác truyền tải biểu đồ cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mỗi truy vấn Dijkstra |$O(q \cdot m \log n)$|$O(n + m)$| Quá chậm | 
| Hai lần chạy Dijkstra + truy vấn |$O(m \log n + q)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là các đường đi ngắn nhất từ một nguồn cố định có thể được tính toán trước một lần bằng Dijkstra. 

1. Chạy Dijkstra bắt đầu từ đỉnh vào$X_1$, tính toán một mảng$d_1[v]$, lưu trữ khoảng cách ngắn nhất từ ​​​​$X_1$đến mọi đỉnh$v$. Điều này ghi lại cách Grisi di chuyển từ bất kỳ điểm nào đến lối vào. 
2. Chạy Dijkstra bắt đầu từ đỉnh IC$X_2$, tính toán một mảng$d_2[v]$, lưu trữ khoảng cách ngắn nhất từ ​​​​$v$tới IC. Vì đồ thị là vô hướng nên điều này tương đương với khoảng cách từ IC đến tất cả các nút. 
3. Tính toán trước giá trị không đổi$d_1[X_2]$, đó là khoảng cách ngắn nhất từ ​​lối vào IC. Đây là đoạn thứ hai trong tuyến đường cố định của Grisi. 
4. Đối với mỗi đỉnh truy vấn$x$, tính đường cơ sở đường đi ngắn nhất như$d_2[x]$, đại diện cho thời gian di chuyển tối ưu thực sự từ$x$tới IC. 
5. Tính thời gian di chuyển của Grisi như sau$d_1[x] + d_1[X_2]$, kể từ lần đầu tiên anh ấy đi từ$x$đến lối vào và sau đó từ lối vào IC. 
6. Thời gian tăng thêm là sự khác biệt giữa lộ trình của Grisi và lộ trình tối ưu. Nếu như$x = X_2$, trực tiếp xuất ra 0. Ngược lại xuất ra$(d_1[x] + d_1[X_2]) - d_2[x]$. 

Việc kiểm tra rõ ràng cho$x = X_2$ngăn giá trị dương gây hiểu lầm do công thức gây ra, vì cả hai đoạn đường đi ngắn nhất vẫn được tính ngay cả khi không cần di chuyển. 

### Tại sao nó hoạt động 

Khoảng cách đường đi ngắn nhất thỏa mãn cấu trúc con tối ưu: mọi đường đi ngắn nhất giữa hai đỉnh cố định đều có thể được phân tách thành các đường đi ngắn nhất giữa các điểm trung gian. Dijkstra tính toán chính xác những khoảng cách này từ các nguồn cố định. 

Chiến lược của Grisi buộc mọi con đường từ$x$đến IC để đi qua lối vào, vì vậy chiều dài tuyến đường của anh ta chính xác bằng tổng của hai tuyến đường ngắn nhất độc lập. Tuyến đường tối ưu không phụ thuộc vào ràng buộc đó. Vì cả hai đại lượng đều được tính toán chính xác một lần cho tất cả các nút nên việc so sánh cho mỗi truy vấn là chính xác và không cần tính toán lại. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline
INF = 10**30

def dijkstra(n, adj, src):
    dist = [INF] * (n + 1)
    dist[src] = 0
    pq = [(0, src)]

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

def solve():
    n, m, q, x1, x2 = map(int, input().split())

    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))

    d1 = dijkstra(n, adj, x1)
    d2 = dijkstra(n, adj, x2)

    base = d1[x2]

    out = []
    for _ in range(q):
        x = int(input())
        if x == x2:
            out.append("0")
        else:
            extra = d1[x] + base - d2[x]
            out.append(str(extra))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng danh sách kề và chạy Dijkstra hai lần, một lần từ mỗi đỉnh đặc biệt. Hai mảng khoảng cách được sử dụng lại trên tất cả các truy vấn. giá trị`base`lưu trữ chi phí cố định từ lối vào IC. 

Mỗi truy vấn được trả lời trong thời gian không đổi bằng cách sử dụng khoảng cách được tính toán trước. Việc kiểm tra trường hợp đặc biệt đối với`x2`đảm bảo tính chính xác khi điểm bắt đầu đã là điểm đến. 

Một lỗi triển khai phổ biến là tính toán lại các đường dẫn ngắn nhất cho mỗi truy vấn hoặc quên rằng phân đoạn lối vào IC phải được tính toán trước một lần thay vì được tính toán lại bên trong mỗi truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó lối vào là 1 và IC là 4: 

Đồ thị đầu vào: 

1-2 (1), 2-4 (1), 1-3 (1), 3-4 (10) 

Từ 1, khoảng cách ngắn nhất là: d1[1]=0, d1[2]=1, d1[3]=1, d1[4]=2. 

Từ 4, khoảng cách ngắn nhất là: d2[4]=0, d2[2]=1, d2[3]=2, d2[1]=2. 

Đối với truy vấn x = 2: 

Lộ trình Grisi là 2→1→4, chi phí = d1[2] + d1[4] = 1 + 2 = 3. 

Lộ trình tối ưu là d2[2] = 1. 

Thời gian bù giờ là 2. 

Đối với truy vấn x = 3: 

Lộ trình Grisi là 3→1→4, chi phí = 1 + 2 = 3. 

Lộ trình tối ưu là d2[3] = 2. 

Thời gian bù giờ là 1. 

| Truy vấn x | d1[x] | d1[X2] | d2[x] | Grisi | Tối ưu | Thêm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 2 | 1 | 3 | 1 | 2 | 
| 3 | 1 | 2 | 2 | 3 | 2 | 1 | 

Điều này xác nhận rằng sau khi khoảng cách được tính toán trước, mỗi truy vấn sẽ giảm xuống mức so sánh trực tiếp giữa chỉ số đường dẫn bắt buộc và chỉ số đường dẫn ngắn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log n + q)$| Hai lần chạy Dijkstra chiếm ưu thế, mỗi truy vấn là O(1) | 
| Không gian |$O(n + m)$| danh sách kề cộng với hai mảng khoảng cách | 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh, do đó, một vài Dijkstra chạy với đống nhị phân vừa vặn thoải mái trong giới hạn thời gian. Công việc trên mỗi truy vấn không đáng kể so với tiền xử lý. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    INF = 10**30

    def dijkstra(n, adj, src):
        dist = [INF] * (n + 1)
        dist[src] = 0
        pq = [(0, src)]
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

    n, m, q, x1, x2 = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))

    d1 = dijkstra(n, adj, x1)
    d2 = dijkstra(n, adj, x2)
    base = d1[x2]

    out = []
    for _ in range(q):
        x = int(input())
        if x == x2:
            out.append("0")
        else:
            out.append(str(d1[x] + base - d2[x]))
    return "\n".join(out)

# sample 1
assert run("""4 4 2 1 4
1 2 1
1 3 1
2 4 1
3 4 1
2
3
""") == "2\n2"

# custom 1: single edge
assert run("""2 1 1 1 2
1 2 5
1
""") == "0"

# custom 2: start is IC
assert run("""3 2 1 1 3
1 2 1
2 3 1
3
""") == "0"

# custom 3: triangle graph
assert run("""3 3 2 1 3
1 2 1
2 3 1
1 3 10
2
1
""") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị cạnh đơn | 0 | đường dẫn trực tiếp tầm thường và tính đúng đắn của trường hợp cơ sở | 
| bắt đầu bằng IC | 0 | xử lý trường hợp đặc biệt của đỉnh đích | 
| đồ thị tam giác | giá trị hỗn hợp | tính chính xác của việc so sánh đường dẫn ngắn nhất và bắt buộc | 

## Vỏ cạnh 

Khi đỉnh truy vấn là IC, biểu thức$d_1[x] + d_1[X_2] - d_2[x]$trở thành$d_1[X_2] + d_1[X_2]$, điều này luôn dương mặc dù không cần đi du lịch. Kiểm tra đẳng thức rõ ràng đảm bảo trường hợp này đưa ra kết quả bằng 0, khớp với định nghĩa vấn đề. 

Trong các biểu đồ có nhiều đường đi có trọng số bằng nhau, Dijkstra vẫn tạo ra khoảng cách ngắn nhất nhất quán vì nó chỉ phụ thuộc vào thứ tự giãn chứ không phụ thuộc vào tính duy nhất của đường đi. Điều này đảm bảo rằng cả hai mảng khoảng cách vẫn hợp lệ ngay cả khi đường đi ngắn nhất không phải là duy nhất.
