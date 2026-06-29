---
title: "CF 104614I - Con đường tiết kiệm"
description: "Thành phố được mô hình hóa dưới dạng đồ thị có trọng số vô hướng. Mỗi giao lộ là một đỉnh và mỗi con đường là một cạnh có trọng số là chiều dài của nó."
date: "2026-06-29T20:31:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104614
codeforces_index: "I"
codeforces_contest_name: "2022-2023 ICPC East Central North America Regional Contest (ECNA 2022)"
rating: 0
weight: 104614
solve_time_s: 70
verified: true
draft: false
---

[CF 104614I - Con đường tiết kiệm](https://codeforces.com/problemset/problem/104614/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố được mô hình hóa dưới dạng đồ thị có trọng số vô hướng. Mỗi giao lộ là một đỉnh và mỗi con đường là một cạnh có trọng số là chiều dài của nó. Thị trưởng luôn đi từ ngã tư`a`đến giao lộ`b`và Pat chỉ muốn mở những con đường thuộc ít nhất một con đường ngắn nhất giữa hai địa điểm đó. 

Nhiệm vụ không phải là tìm một con đường ngắn nhất. Đường phải được trải nhựa nếu có đường đi ngắn nhất sử dụng đường đó. Mọi con đường còn lại đều không cần thiết và chúng ta phải xuất ra tổng chiều dài của chúng. 

Đồ thị chứa tối đa 100 đỉnh, trong khi số lượng đường không bị giới hạn trong câu lệnh nhưng không bao giờ có thể vượt quá số cạnh trong một đồ thị vô hướng đơn giản, đó là`n(n-1)/2`. Ngay cả đồ thị dày đặc nhất cũng có ít hơn 5000 cạnh. Những giới hạn này đủ nhỏ để việc chạy thuật toán Dijkstra nhiều lần là hoàn toàn thực tế. Không cần cấu trúc dữ liệu đường dẫn ngắn nhất phức tạp hơn. 

Khó khăn chính là có thể tồn tại một số tuyến đường ngắn nhất khác nhau. Giải pháp chỉ xây dựng lại một con đường ngắn nhất sẽ bỏ lỡ những con đường thuộc một tuyến đường ngắn tương đương khác. 

Hãy xem xét biểu đồ sau.```
4 4 1 4
1 2 1
2 4 1
1 3 1
3 4 1
```Đầu ra đúng là```
0
```Cả hai tuyến đường từ`1`ĐẾN`4`có chiều dài`2`, do đó mọi con đường đều thuộc về một con đường ngắn nhất nào đó. Một giải pháp chỉ lưu trữ một cha mẹ trong Dijkstra sẽ chỉ đánh dấu sai một tuyến đường và đầu ra`2`. 

Một trường hợp khó phát hiện khác là khi một cạnh trực tiếp tồn tại cùng với một cạnh thay thế dài hơn.```
3 3 1 3
1 2 2
2 3 2
1 3 4
```Đầu ra đúng là```
0
```Cả hai con đường đều có tổng chiều dài`4`, vì vậy mọi cạnh đều phải được lát. Chỉ tìm kiếm các lựa chọn thay thế ngắn hơn sẽ loại trừ đường thẳng một cách không chính xác. 

Trường hợp cạnh cuối cùng xảy ra khi một con đường không thể xuất hiện trên bất kỳ con đường ngắn nhất nào.```
3 3 1 3
1 2 1
2 3 1
1 3 5
```Đầu ra đúng là```
5
```Cạnh trực tiếp dài không bao giờ hữu ích, do đó độ dài của nó góp phần vào câu trả lời. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là thử nghiệm từng con đường một cách độc lập. Xóa đường, tính toán đường đi ngắn nhất, sau đó buộc đường vào đường dẫn và kiểm tra xem khoảng cách thu được có bằng mức tối ưu toàn cục hay không. Điều này hiệu quả vì mọi cạnh đều được kiểm tra riêng biệt nhưng nó yêu cầu chạy thuật toán đường đi ngắn nhất cho mỗi con đường. Với khoảng 5000 cạnh và mỗi Dijkstra lấy`O(m log n)`, tổng công việc trở thành`O(m² log n)`, điều này là không cần thiết ngay cả đối với những ràng buộc này. 

Quan sát quan trọng là liệu một cạnh có thuộc đường đi ngắn nhất nào đó hay không chỉ phụ thuộc vào khoảng cách từ hai điểm cuối. 

Giả sử một cạnh kết nối`u`Và`v`với trọng lượng`w`. Nếu chúng ta đã biết khoảng cách ngắn nhất từ`a`tới mọi đỉnh và từ mọi đỉnh tới`b`, thì chúng ta có thể kiểm tra ngay cả hai hướng có thể. 

Cạnh xuất hiện trên đường đi ngắn nhất nếu một trong hai`distA[u] + w + distB[v] == distA[b]`hoặc`distA[v] + w + distB[u] == distA[b]`. 

Phương trình đầu tiên tương ứng với việc đạt`u`, đi qua cạnh để`v`, sau đó tiếp tục`b`. Cái thứ hai xử lý theo hướng ngược lại vì đồ thị không có hướng. 

Điều này làm giảm vấn đề xuống còn hai lần chạy Dijkstra, sau đó là một lần quét qua danh sách cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(m² log n)`|`O(n + m)`| Quá chậm | 
| Tối ưu |`O(m log n)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị có trọng số vô hướng đồng thời lưu trữ mọi cạnh trong một danh sách riêng. Danh sách kề được Dijkstra sử dụng, trong khi danh sách cạnh cần thiết cho lần quét cuối cùng. 
2. Chạy Dijkstra bắt đầu từ nhà thị trưởng`a`. Ghi lại khoảng cách ngắn nhất từ`a`đến mọi đỉnh trong mảng`distA`. 
3. Chạy lại Dijkstra bắt đầu từ văn phòng thị trưởng`b`. Ghi lại khoảng cách ngắn nhất từ`b`đến mọi đỉnh trong mảng`distB`. 
4. Hãy để`best = distA[b]`, là độ dài của mọi tuyến đường ngắn nhất giữa hai địa điểm. 
5. Kiểm tra mọi con đường`(u, v, w)`một lần. Kiểm tra xem có`distA[u] + w + distB[v]`hoặc`distA[v] + w + distB[u]`bằng`best`. 
6. Nếu không có đẳng thức nào xảy ra thì không có con đường ngắn nhất nào có thể chứa con đường này, vì vậy hãy cộng độ dài của nó vào câu trả lời. 
7. In số tiền tích lũy. 

### Tại sao nó hoạt động 

Dijkstra đầu tiên tính toán chi phí tối thiểu có thể để đạt được mọi đỉnh ngay từ đầu. Phần thứ hai tính toán chi phí còn lại tối thiểu có thể từ mọi đỉnh đến đích. 

Giả sử một cạnh thuộc về đường đi ngắn nhất. Đi qua đường dẫn đó sẽ đạt đến một điểm cuối một cách tối ưu, vượt qua cạnh, sau đó đến đích một cách tối ưu. Nếu không thì đường dẫn có thể được rút ngắn. Do đó, một trong hai đẳng thức phải được giữ nguyên. 

Ngược lại, nếu một trong các đẳng thức được giữ nguyên, việc ghép một đường đi ngắn nhất đến điểm cuối thứ nhất, chính cạnh đó và đường đi ngắn nhất từ ​​điểm cuối thứ hai sẽ tạo ra một đường dẫn có tổng chiều dài bằng mức tối ưu toàn cục. Do đó cạnh nằm trên ít nhất một đường đi ngắn nhất. 

Việc kiểm tra vừa cần vừa đủ nên đường nào cũng được phân loại chính xác. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def dijkstra(start, graph, n):
    INF = 10 ** 18
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

def solve():
    n, m, a, b = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    edges = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))
        edges.append((u, v, w))

    distA = dijkstra(a, graph, n)
    distB = dijkstra(b, graph, n)

    shortest = distA[b]
    ans = 0

    for u, v, w in edges:
        if (distA[u] + w + distB[v] == shortest or
                distA[v] + w + distB[u] == shortest):
            continue
        ans += w

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tuân theo thuật toán trực tiếp. Biểu đồ được lưu trữ hai lần, một lần dưới dạng danh sách kề để tính toán đường đi ngắn nhất hiệu quả và một lần dưới dạng danh sách cạnh đơn giản để mỗi con đường có thể được kiểm tra chính xác một lần. 

Việc triển khai Dijkstra sử dụng kỹ thuật xếp hàng ưu tiên lười tiêu chuẩn. Bất cứ khi nào một mục nhập lỗi thời bị xóa khỏi heap, nó sẽ bị bỏ qua bằng cách so sánh khoảng cách của nó với giá trị tốt nhất hiện tại. 

Lần quét cuối cùng thực hiện hai phép kiểm tra đẳng thức cho mọi cạnh vô hướng. Kiểm tra cả hai hướng là điều cần thiết vì đường đi ngắn nhất có thể đi qua cạnh theo một trong hai hướng. Vì số nguyên Python có độ chính xác tùy ý nên không có nguy cơ tràn ngay cả khi tính tổng nhiều khoảng cách. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
4 5 1 4
1 2 1
1 3 2
1 4 2
4 2 1
3 4 1
```Mảng khoảng cách là`distA = [0, 1, 2, 2]`

`distB = [2, 1, 1, 0]`Quãng đường ngắn nhất là`2`. 

| Cạnh | Biểu hiện hài lòng? | Trên con đường ngắn nhất? | Chạy câu trả lời | 
| --- | --- | --- | --- | 
| (1,2,1) | Có | Có | 0 | 
| (1,3,2) | Không | Không | 2 | 
| (1,4,2) | Có | Có | 2 | 
| (2,4,1) | Có | Có | 2 | 
| (3,4,1) | Không | Không | 3 | 

Câu trả lời là`3`. Dấu vết cho thấy một cạnh chỉ bị loại bỏ khi cả hai hướng đều không đạt được khoảng cách ngắn nhất toàn cầu. 

### Mẫu 2 

đầu vào```
4 5 1 4
1 2 1
1 3 2
1 4 1
4 2 1
3 4 1
```Mảng khoảng cách là`distA = [0, 1, 2, 1]`

`distB = [1, 1, 1, 0]`Quãng đường ngắn nhất là`1`. 

| Cạnh | Biểu hiện hài lòng? | Trên con đường ngắn nhất? | Chạy câu trả lời | 
| --- | --- | --- | --- | 
| (1,2,1) | Không | Không | 1 | 
| (1,3,2) | Không | Không | 3 | 
| (1,4,1) | Có | Có | 3 | 
| (2,4,1) | Không | Không | 4 | 
| (3,4,1) | Không | Không | 5 | 

Chỉ có con đường đi thẳng thuộc về con đường ngắn nhất nên mọi con đường khác đều đóng góp vào tổng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m log n)`| Hai lần chạy Dijkstra và một lần quét tuyến tính trên mọi con đường | 
| Không gian |`O(n + m)`| Mảng khoảng cách, danh sách kề và danh sách cạnh | 

Ngay cả đồ thị dày đặc nhất với 100 đỉnh cũng chứa ít hơn 5000 cạnh, do đó hai phép thực thi Dijkstra hoàn thành thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline

    n, m, a, b = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    edges = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))
        edges.append((u, v, w))

    def dijkstra(s):
        INF = 10 ** 18
        dist = [INF] * (n + 1)
        dist[s] = 0
        pq = [(0, s)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            for v, w in graph[u]:
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))
        return dist

    da = dijkstra(a)
    db = dijkstra(b)
    best = da[b]

    ans = 0
    for u, v, w in edges:
        if da[u] + w + db[v] == best or da[v] + w + db[u] == best:
            continue
        ans += w
    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

assert run("""4 5 1 4
1 2 1
1 3 2
1 4 2
4 2 1
3 4 1
""") == "3"

assert run("""4 5 1 4
1 2 1
1 3 2
1 4 1
4 2 1
3 4 1
""") == "5"

assert run("""2 1 1 2
1 2 7
""") == "0"

assert run("""4 4 1 4
1 2 1
2 4 1
1 3 1
3 4 1
""") == "0"

assert run("""3 3 1 3
1 2 1
2 3 1
1 3 5
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có một cạnh | 0 | Đồ thị tối thiểu | 
| Hai con đường ngắn nhất bằng nhau | 0 | Mọi cạnh đều phải được chấp nhận | 
| Cạnh thẳng dài cộng với đường vòng ngắn | 5 | Cạnh bị loại khỏi tất cả các đường đi ngắn nhất | 
| Mẫu 1 | 3 | Đường hỗn hợp được chấp nhận và bị từ chối | 
| Mẫu 2 | 5 | Chỉ có một con đường thuộc về con đường ngắn nhất | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là có nhiều đường đi ngắn nhất.```
4 4 1 4
1 2 1
2 4 1
1 3 1
3 4 1
```Quãng đường ngắn nhất là`2`. Đối với mỗi cạnh, một trong hai lần kiểm tra đẳng thức thành công, do đó mọi cạnh được phân loại là thuộc về một đường đi ngắn nhất. Đầu ra của thuật toán`0`, trong khi việc xây dựng lại dựa trên cha mẹ sẽ loại bỏ một nửa số cạnh một cách không chính xác. 

Trường hợp thứ hai là khi hai tuyến đường hoàn toàn khác nhau có tổng chiều dài bằng nhau.```
3 3 1 3
1 2 2
2 3 2
1 3 4
```Quãng đường ngắn nhất là`4`. Cả đường thẳng và đường hai cạnh đều thỏa mãn phép thử bằng nhau nên cả ba đường đều được trải nhựa và đáp án là`0`. 

Trường hợp cuối cùng có một con đường thực sự tồi tệ hơn mọi phương án khác.```
3 3 1 3
1 2 1
2 3 1
1 3 5
```Quãng đường ngắn nhất là`2`. Cạnh dài tạo ra`0 + 5 + 0 = 5`, lớn hơn mức tối ưu, do đó không có đẳng thức nào giữ được. Độ dài của nó được thêm vào câu trả lời, tạo ra kết quả chính xác`5`.
