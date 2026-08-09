---
title: "CF 102448H - Hellcife đang bốc cháy"
description: "Hãy coi mỗi thành phố như một đỉnh và mỗi con đường là một cạnh có trọng số vô hướng. Thành phố (v) có thêm độ trễ (Tv): khi ngọn lửa lan đến (v), thành phố không bị cháy hoàn toàn ngay lập tức. Phải mất thêm (Tv) giây nữa. Ban đầu, một số thành phố bị đốt cháy ở thời điểm (0)."
date: "2026-08-09T18:22:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "H"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 635
verified: true
draft: false
---

[CF 102448H - Hellcife đang bốc cháy](https://codeforces.com/problemset/problem/102448/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi thành phố như một đỉnh và mỗi con đường là một cạnh có trọng số vô hướng. Thành phố (v) có thêm độ trễ (T_v): khi ngọn lửa lan tới (v), thành phố không bị cháy hoàn toàn ngay lập tức. Phải mất thêm (T_v) giây nữa. 

Ban đầu, một số thành phố bị đốt cháy ở thời điểm (0). Đối với mỗi thành phố, chúng ta cần thời gian chính xác khi thành phố đó bị cháy hoàn toàn. 

Giả sử thành phố (u) bị cháy hoàn toàn vào thời điểm (D_u) và có một con đường dài (w) từ (u) đến (v). Lửa có thể chạm tới (v) tại thời điểm (D_u+w), và sau đó (v) cần thêm (T_v) giây nữa để cháy hoàn toàn. Như vậy tuyến đường này sẽ làm cho (v) cháy hoàn toàn tại 

[ 
D_u+w+T_v. 
] 

Đối với (các) thành phố bị đốt cháy ban đầu, không có đường đi qua trước khi ngọn lửa đến được, vì vậy thời gian cháy của nó chỉ đơn giản là 

[ 
D_s=T_s. 
] 

Biểu đồ có thể chứa tối đa (10^5) thành phố và (10^5) đường. Với quy mô này, bất cứ thứ gì tính theo số lượng thành phố đều đã quá đắt. Ngay cả việc quét tất cả các con đường (10^5) lần cũng sẽ yêu cầu khoảng (10^{10}) lần kiểm tra lân cận, vượt xa giới hạn một giây. Chúng ta cần một thuật toán gần với (O((N+M)\log N)). 

Độ dài đường và tất cả các giá trị (T_i) đều dương, do đó mọi thời gian đều không âm. Biểu đồ được kết nối, vì vậy mọi thành phố cuối cùng sẽ bị đốt cháy. 

Có một số trường hợp đặc biệt có thể dễ dàng dẫn đến câu trả lời sai. 

Đầu tiên, một thành phố bị đốt cháy ban đầu không bị cháy hoàn toàn tại thời điểm (0). Ví dụ:```
2 1 1
1 2 5
7 3
1
```Đầu ra đúng là```
7
15
```Thành phố 1 bắt đầu cháy vào thời điểm (0), kết thúc vào thời điểm (7) và chỉ sau đó ngọn lửa mới di chuyển trong (5) giây đến thành phố 2. Khi đó, Thành phố 2 cần thêm (3) giây. Việc triển khai khởi tạo khoảng cách nguồn về 0 sẽ thu được (0) và (8) không chính xác. 

Thứ hai, độ trễ cháy của mọi thành phố trung gian đều quan trọng. Coi như:```
2 1 1
1 2 5
10 1
1
```Đầu ra đúng là```
10
16
```Ngọn lửa dành (10) giây để đốt cháy thành phố 1, di chuyển trong (5) giây và sau đó dành (1) giây để đốt thành phố 2. Hãy coi vấn đề là một con đường ngắn nhất thông thường và chỉ thêm (T_v) của đích sẽ cho (6), điều này sai vì thành phố 1 phải cháy xong trước khi nó có thể lan lửa. 

Thứ ba, có thể có một số thành phố bị đốt cháy ban đầu. Người đến sớm nhất sẽ quyết định câu trả lời của thành phố đó. Ví dụ:```
3 2 2
1 2 10
2 3 1
1 1 1
1 3
```Đầu ra đúng là```
1
3
1
```Thành phố 2 được tiếp cận từ thành phố 3 sau (1) giây và sau đó mất thêm (1) giây để đốt cháy, vì vậy câu trả lời của nó là (2), thực sự tạo ra đầu ra hoàn chỉnh```
1
2
1
```Giải pháp chỉ bắt đầu từ nguồn đầu tiên sẽ bỏ lỡ con đường nhanh hơn này. 

Cuối cùng, các đường song song phải được xử lý độc lập. Mẫu 2 chứa nhiều con đường nối cùng một cặp thành phố có độ dài khác nhau. Con đường ngắn nhất như vậy có thể thay đổi hoàn toàn câu trả lời, do đó việc triển khai không được thu gọn các cạnh song song mà không giữ lại cạnh tối thiểu. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xử lý riêng từng thành phố đang cháy ban đầu. Đối với một (các) nguồn, chúng ta có thể chạy thuật toán đường đi ngắn nhất có chi phí chuyển đổi từ (u) sang (v) 

[ 
w(u,v)+T_v. 
] 

Nguồn bắt đầu bằng khoảng cách (T_s). Điều này mang lại thời gian ghi chính xác từ nguồn cụ thể đó và sau khi xử lý tất cả các nguồn (K), chúng tôi đưa ra câu trả lời tối thiểu cho mọi thành phố. 

Cách tiếp cận này đúng nhưng nó lặp lại gần như toàn bộ biểu đồ tìm kiếm cho mọi nguồn. Một lần chạy Dijkstra mất (O((N+M)\log N)), do đó chi phí xử lý tất cả các nguồn 

[ 
O(K(N+M)\log N). 
] 

Trong trường hợp xấu nhất (K=N=M=10^5), thuật toán có thể kiểm tra khoảng (2KM=2\cdot10^{10}) các mục liền kề trước khi tính đến các hoạt động của vùng nhớ khối xếp. Đó là quá nhiều cho thời hạn. 

Quan sát chính là tất cả các nguồn đều sử dụng chính xác cùng một biểu đồ và cùng một quy tắc chuyển tiếp. Chúng tôi thực sự không quan tâm nguồn nào tạo ra đường dẫn tối ưu. Chúng tôi chỉ quan tâm đến thời gian cháy sớm nhất có thể cho mỗi thành phố. 

Đây chính xác là tình huống mà Dijkstra có thể bắt đầu từ nhiều nguồn cùng một lúc. Thay vì chạy một Dijkstra từ mọi nguồn, hãy khởi tạo mỗi thành phố đang cháy ban đầu với giá trị riêng (T_s), đặt tất cả chúng vào cùng một hàng đợi ưu tiên và để chúng cạnh tranh để tiếp cận các thành phố còn lại. 

Đối với thành phố hiện tại (u) có thời gian đốt cuối cùng (D_u), việc đi qua cạnh có độ dài (w) đến (v) sẽ tạo ra ứng cử viên 

[ 
D_u+w+T_v. 
] 

Đại lượng (w+T_v) luôn dương, do đó biểu đồ trạng thái kết quả có chi phí cạnh không âm và lựa chọn tham lam của Dijkstra vẫn hợp lệ. 

Phương pháp brute-force hoạt động vì mỗi nguồn riêng lẻ có thể được Dijkstra xử lý, nhưng nó không thành công khi có nhiều nguồn vì nó lặp lại cùng một công việc. Quan sát rằng tất cả các nguồn có thể tham gia vào một tính toán đường đi ngắn nhất cho phép chúng ta thay thế (K) các lần chạy Dijkstra bằng một Dijkstra đa nguồn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chạy Dijkstra một lần cho mỗi nguồn | (O(K(N+M)\log N)) | (O(N+M)) | Quá chậm | 
| Dijkstra đa nguồn | (O((N+M)\log N)) | (O(N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi con đường vào danh sách kề. Đối với đường nằm giữa (u) và (v) có chiều dài (w), hãy thêm ((v,w)) vào danh sách của (u) và ((u,w)) vào danh sách của (v) vì đường đó không có hướng. 
2. Tạo một mảng (dist), trong đó (dist[v]) biểu thị thời điểm sớm nhất mà thành phố (v) bị cháy hoàn toàn. Ban đầu mọi giá trị đều là vô cùng vì chúng ta chưa tìm được đường đến thành phố đó. 
3. Với mỗi (các) thành phố được đốt cháy ban đầu, hãy đặt 

[ 
dist[s]=T_s. 
] 

Đẩy mọi thành phố như vậy vào hàng ưu tiên bằng phím (T_s). 

Giá trị là (T_s), chứ không phải 0, vì thành phố bắt đầu cháy vào thời điểm 0 nhưng chỉ bị cháy hoàn toàn sau thời gian cháy trễ của chính nó. 

1. Liên tục loại bỏ thành phố (u) có thời gian ghi dự kiến ​​nhỏ nhất khỏi hàng đợi ưu tiên. 

Nếu giá trị được trích xuất lớn hơn giá trị hiện tại (dist[u]), hãy loại bỏ nó. Đây là một mục nhập cũ được tạo ra bởi một quá trình thư giãn tồi tệ hơn trước đó. 

1. Với mọi đường từ (u) đến (v) có chiều dài (w), hãy tính 

[ 
ứng cử viên=dist[u]+w+T_v. 
] 

Số hạng thứ nhất là khi (u) cháy xong, số hạng thứ hai là thời gian cần thiết để lửa lan qua đường, số hạng thứ ba là thời gian cần thiết để cháy (v). 

1. Nếu ứng viên nhỏ hơn (dist[v]), hãy thay thế (dist[v]) bằng ứng cử viên và đẩy cặp mới vào hàng ưu tiên. 
2. Khi hàng đợi ưu tiên trống, mọi thành phố đều có thời gian ghi tối thiểu có thể vì biểu đồ được kết nối. Xuất các giá trị theo thứ tự thành phố. 

### Tại sao nó hoạt động

Điều bất biến là bất cứ khi nào Dijkstra xử lý vĩnh viễn một thành phố (u), (dist[u]) là thời gian đốt cháy hoàn toàn tối thiểu có thể có đối với (u) từ bất kỳ thành phố nào được đốt cháy ban đầu. 

Xem xét mọi tuyến đường có thể từ nguồn ban đầu tới (u). Nguồn đóng góp giá trị (T) của nó, mỗi con đường đóng góp chiều dài của nó và mọi thành phố đi vào sau nguồn đóng góp giá trị (T) riêng của nó. Do đó, mọi tuyến đường đều tương ứng chính xác với một đường dẫn trong đồ thị có hướng phụ mà quá trình chuyển đổi từ (u) sang (v) có chi phí (w(u,v)+T_v), với mỗi nguồn được khởi tạo là (T_s). 

Tất cả các chi phí chuyển đổi này đều dương. Do đó, Dijkstra hoàn thiện các đỉnh theo thứ tự không giảm của chi phí đường đi ngắn nhất thực sự của chúng. Vì tất cả các nguồn đều được chèn ban đầu nên đường đi ngắn nhất có thể bắt đầu từ bất kỳ nguồn nào phù hợp nhất với thành phố đó. Do đó, (dist[v]) cuối cùng chính xác là thời điểm sớm nhất mà (v) có thể bị cháy hoàn toàn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append((b, c))
        graph[b].append((a, c))

    t = list(map(int, input().split()))

    sources = list(map(int, input().split()))

    INF = 10**30
    dist = [INF] * n
    pq = []

    for x in sources:
        x -= 1
        if t[x] < dist[x]:
            dist[x] = t[x]
            heapq.heappush(pq, (t[x], x))

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        for v, w in graph[u]:
            candidate = cur + w + t[v]

            if candidate < dist[v]:
                dist[v] = candidate
                heapq.heappush(pq, (candidate, v))

    sys.stdout.write("\n".join(map(str, dist)))

if __name__ == "__main__":
    solve()
```Danh sách kề đại diện cho mạng lưới đường ban đầu. Mỗi con đường được chèn vào cả hai hướng vì ngọn lửa có thể di chuyển dọc theo cả hai hướng. 

Việc khởi tạo là điểm khác biệt chính so với Dijkstra nguồn đơn thông thường. Mọi nguồn ban đầu đều đồng thời đi vào heap, với mức độ ưu tiên (T_s). Nếu cùng một thành phố xuất hiện nhiều lần trong số các nguồn, việc khởi tạo lặp lại không có tác hại gì vì tất cả các lần xuất hiện đều có cùng giá trị. 

Công thức thư giãn là phần trung tâm của việc thực hiện:```
candidate = cur + w + t[v]
```Đây`cur`là thời điểm thành phố hiện tại bị thiêu rụi hoàn toàn. Ngọn lửa sau đó tiêu tốn`w`giây băng qua đường và`t[v]`giây đốt cháy thành phố đích. 

Kiểm tra mục nhập cũ```
if cur != dist[u]:
    continue
```là cần thiết bởi vì Python`heapq`không cung cấp thao tác giảm phím. Khi phát hiện được khoảng cách tốt hơn, chúng tôi sẽ đẩy một mục khác thay vì sửa đổi mục cũ. Mục cũ cuối cùng sẽ thoát ra khỏi heap và phải được bỏ qua. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Dù sao thì câu trả lời hữu ích tối đa cũng nằm trong phạm vi số nguyên 64 bit thông thường, nhưng sử dụng`10**30`vì vô cực giúp việc thực hiện đơn giản. 

Biểu đồ có nhiều nhất (10^5) cạnh, vì vậy danh sách kề lưu trữ tối đa (2\cdot10^5) mục nhập kề có hướng. Vùng heap chứa nhiều nhất một số lượng tuyến tính các mục hữu ích hoặc cũ trong quá trình thực thi, phù hợp với giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào có một nguồn ban đầu, thành phố 1. Độ trễ ghi của nó là (T_1=1), do đó thành phố 1 bị cháy hoàn toàn tại thời điểm 1. 

Bảng sau đây cho thấy các hoạt động Dijkstra có ý nghĩa. 

| Thành phố nổi bật |`dist[u]`| Cạnh được xem xét | Ứng viên | Đã cập nhật`dist`| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (1\to2), độ dài 1 | (1+1+2=4) | (dist[2]=4) | 
| 1 | 1 | (1\to5), dài 13 | (1+13+5=19) | (dist[5]=19) | 
| 2 | 4 | (2\to3), dài 4 | (4+4+3=11) | (dist[3]=11) | 
| 3 | 11 | (3\to4), dài 5 | (11+5+4=20) | (dist[4]=20) | 
| 5 | 19 | (5\to4), dài 10 | (19+10+4=33) | không thay đổi | 
| 4 | 20 | các cạnh còn lại | không cải thiện | không thay đổi | 

Khoảng cách cuối cùng là```
1
4
11
20
19
```Phần thú vị là thành phố 5. Có thể đến trực tiếp từ thành phố 1, cho (1+13+5=19), nhanh hơn so với việc đến đó qua các thành phố 2, 3 và 4. Dijkstra không cần biết trước tuyến đường này. Nó phát hiện ra cả hai khả năng và giữ giá trị nhỏ hơn. 

### Mẫu 2 

Có một nguồn ban đầu, thành phố 2, với (T_2=94560). Do đó, đống ban đầu chứa 

[ 
(94560,2). 
] 

Thành phố 2 có một số con đường song song với các thành phố khác. Ứng cử viên tốt nhất cho mỗi thành phố lân cận được tính toán độc lập. 

| Thành phố nổi bật |`dist[u]`| Chuyển tiếp tốt nhất | Ứng viên | Thành phố cập nhật | 
| --- | --- | --- | --- | --- | 
| 2 | 94560 | (2\to1), chiều dài 25722 | (94560+25722+31551=151833) | 1 | 
| 2 | 94560 | (2\to3), dài 3043 | (94560+3043+84171=181774) | 3 | 
| 2 | 94560 | (2\to4), chiều dài 49102 | (94560+49102+16742=160404) | 4 | 
| 2 | 94560 | (2\to5), chiều dài 41563 | (94560+41563+55756=191879) | 5 | 
| 1 | 151833 | các cạnh từ 1 | tất cả các ứng cử viên lớn hơn | không | 
| 4 | 160404 | cạnh từ 4 | tất cả các ứng cử viên lớn hơn | không | 
| 3 | 181774 | cạnh (3\to5), chiều dài 32836 | (270366) | không | 
| 5 | 191879 | các cạnh còn lại | tất cả các ứng cử viên lớn hơn | không | 

Các giá trị cuối cùng là```
151833
94560
181774
160404
191879
```Ví dụ này cũng chứng minh tại sao các cạnh song song không thể bị bỏ qua một cách đơn giản. Thành phố 2 có ba con đường khác nhau đến thành phố 1 và con đường có chiều dài (25722) mang lại ứng cử viên tốt hơn đáng kể so với những con đường có chiều dài (50743) và (81271). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((N+M)\log N)) | Mỗi lần thư giãn được xử lý thông qua một đống nhị phân và biểu đồ có (O(M)) mục kề nhau | 
| Không gian | (O(N+M)) | Danh sách kề, mảng khoảng cách và hàng đợi ưu tiên đều tuyến tính trong kích thước biểu đồ | 

Với (N,M\le 10^5), thuật toán thực hiện một lần duyệt đồ thị với các phép toán vùng heap thay vì tối đa (10^5) lần duyệt riêng biệt. Giới hạn (O((N+M)\log N)) phù hợp với giới hạn một giây và mức sử dụng bộ nhớ (O(N+M)) vừa vặn thoải mái trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    graph = [[] for _ in range(n)]

    for _ in range(m):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append((b, c))
        graph[b].append((a, c))

    t = list(map(int, input().split()))
    sources = list(map(int, input().split()))

    INF = 10**30
    dist = [INF] * n
    pq = []

    for x in sources:
        x -= 1
        if t[x] < dist[x]:
            dist[x] = t[x]
            heapq.heappush(pq, (t[x], x))

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        for v, w in graph[u]:
            candidate = cur + w + t[v]

            if candidate < dist[v]:
                dist[v] = candidate
                heapq.heappush(pq, (candidate, v))

    sys.stdout.write("\n".join(map(str, dist)))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1 = """\
5 5 1
1 2 1
2 3 4
3 4 5
4 5 10
1 5 13
1 2 3 4 5
1
"""

assert run(sample1) == """\
1
4
11
20
19
""", "sample 1"

sample2 = """\
5 18 1
1 3 14877
2 1 81271
1 2 50743
5 1 46485
2 5 41563
5 4 72606
1 2 88401
5 3 56633
2 1 25722
3 1 78857
2 3 95527
5 4 66046
1 4 87400
4 2 49102
3 2 3043
5 3 32836
3 2 13703
4 1 86008
31551 94560 84171 16742 55756
2
"""

assert run(sample2) == """\
151833
94560
181774
160404
191879
""", "sample 2"

sample3 = """\
7 12 2
6 3 61451
3 5 48225
3 6 18732
5 3 86896
1 5 73979
4 3 49294
3 1 2794
1 5 3449
7 2 86351
4 6 59862
2 1 38972
3 7 20293
36685 6614 81280 91835 68491 81662 10505
2 1
"""

assert run(sample3) == """\
36685
6614
120759
261888
108625
221153
103470
""", "sample 3"

# Minimum valid connected graph, source is city 1.
custom1 = """\
2 1 1
1 2 1
1 1
1
"""

assert run(custom1) == """\
1
3
""", "minimum-size case"

# Source has a large burning time. The destination cannot start
# spreading before the source has completely burnt.
custom2 = """\
2 1 1
1 2 5
10 1
1
"""

assert run(custom2) == """\
10
16
""", "intermediate burning time"

# Multiple sources. City 3 is much closer to city 2 than city 1.
custom3 = """\
3 2 2
1 2 10
2 3 1
1 1 1
1 3
"""

assert run(custom3) == """\
1
2
1
""", "multiple sources"

# Parallel edges with the same endpoints. The shorter road must win.
custom4 = """\
3 3 1
1 2 100
1 2 1
2 3 1
1 1 1
1
"""

assert run(custom4) == """\
1
3
5
""", "parallel edges"

# Large boundary case generated programmatically.
n = 100000
parts = [f"{n} {n - 1} 1"]
for i in range(1, n):
    parts.append(f"{i} {i + 1} 1")
parts.append(" ".join(["1"] * n))
parts.append("1")

large_input = "\n".join(parts) + "\n"
large_output = run(large_input)

expected_large = "\n".join(str(2 * i - 1) for i in range(1, n + 1)) + "\n"
assert large_output == expected_large, "maximum-size case"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`, một cạnh có độ dài 1 |`1, 3`| Khởi tạo nguồn và biểu đồ được kết nối hợp lệ tối thiểu | 
|`2 1 1`, cạnh 5, (T=[10,1]) |`10, 16`| Đốt cháy chậm trễ của một thành phố trung gian | 
| Ba thành phố với hai nguồn ban đầu |`1, 2, 1`| Cạnh tranh giữa nhiều nguồn | 
| Ba thành phố có cạnh song song |`1, 3, 5`| Chọn đường song song ngắn nhất | 
| Chuỗi (100000) thành phố |`1, 3, 5, ..., 199999`| Biểu đồ kích thước tối đa, trọng lượng đơn vị và hiệu suất | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một thành phố được đốt cháy ban đầu. Coi như```
2 1 1
1 2 1
1 1
1
```Thuật toán khởi tạo`dist[1]`đến (T_1=1), không phải bằng không. Nó bật lên thành phố 1 với thời gian 1 và thu được 

[ 
1+1+1=3 
] 

cho thành phố 2. Do đó, đầu ra là```
1
3
```Việc khởi tạo nguồn trực tiếp nắm bắt được thực tế rằng việc đánh lửa và đốt cháy hoàn toàn là các sự kiện khác nhau. 

Trường hợp cạnh thứ hai là một thành phố trung gian có độ trễ cháy lớn:```
2 1 1
1 2 5
10 1
1
```Khoảng cách ban đầu duy nhất là (10). Khi thành phố 1 được xử lý, việc chuyển đổi sang thành phố 2 tốn kém (5+1=6), cho ra (10+6=16). Đầu ra là```
10
16
```Công thức đường đi ngắn nhất chỉ tính chiều dài đường sẽ cho phép thành phố 1 truyền lửa ngay lập tức vào thời điểm 0. 

Trường hợp cạnh thứ ba có nhiều nguồn:```
3 2 2
1 2 10
2 3 1
1 1 1
1 3
```Đống bắt đầu bằng`(1, city 1)`Và`(1, city 3)`. Thành phố 3 đến thành phố 2 sau một giây, sau đó thành phố 2 cháy trong một giây, mang lại`dist[2]=2`. Thành phố 1 sẽ đến thành phố 2 muộn hơn nhiều, với (1+10+1=12), vì vậy nguồn thứ hai sẽ thắng. Kết quả cuối cùng là```
1
2
1
```Việc khởi tạo nhiều nguồn chính xác là điều cho phép Dijkstra chọn nguồn tốt hơn này mà không cần thực hiện tìm kiếm riêng. 

Trường hợp cạnh thứ tư chứa các đường song song:```
3 3 1
1 2 100
1 2 1
2 3 1
1 1 1
1
```Từ thành phố 1, đoạn đường dài 1 cho thành phố 2 thời gian cháy là (1+1+1=3). Con đường dài 100 không bao giờ có cạnh tranh. Thành phố 3 sau đó nhận được hỏa lực từ thành phố 2 và kết thúc ở (3+1+1=5). Đầu ra là```
1
3
5
```Danh sách kề giữ lại cả hai con đường, do đó, con đường ngắn hơn đương nhiên sẽ thắng trong quá trình thư giãn. 

Trường hợp ranh giới cuối cùng là chuỗi (100000) thành phố với mọi chiều dài đường và mọi (T_i) bằng 1. Bắt đầu từ thành phố 1 cho 

[ 
khoảng cách [1]=1, 
] 

và mọi thành phố tiếp theo sẽ cộng chính xác (1+1=2). Như vậy thành phố (i) có câu trả lời 

[ 
2i-1. 
] 

Thành phố cuối cùng có câu trả lời (199999). Thuật toán xử lý toàn bộ biểu đồ này bằng một lần chạy Dijkstra, chứng tỏ rằng độ phức tạp dự định vẫn thực tế ngay cả ở kích thước đầu vào lớn nhất. 

Nếu bạn muốn, tôi cũng có thể biến điều này thành phong cách biên tập Codeforces điển hình hơn với bằng chứng ngắn hơn và nguồn gốc trực quan hơn của trạng thái Dijkstra đã sửa đổi.
