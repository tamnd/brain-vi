---
title: "CF 449B - Jzzhu và các thành phố"
description: "Chúng ta được cung cấp một biểu đồ có trọng số vô hướng biểu thị các thành phố và đường sá. Thành phố 1 là thủ đô. Ngoài những con đường thông thường, còn có những tuyến tàu đặc biệt nối thủ đô trực tiếp với một số thành phố. Chính phủ muốn đóng càng nhiều tuyến tàu càng tốt."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 449
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 257 (Div. 1)"
rating: 2000
weight: 449
solve_time_s: 86
verified: true
draft: false
---

[CF 449B - Jzzhu và các thành phố](https://codeforces.com/problemset/problem/449/B) 

**Xếp hạng:** 2000 
**Tags:** đồ thị, đường đi tham lam, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số vô hướng biểu thị các thành phố và đường sá. Thành phố 1 là thủ đô. Ngoài những con đường thông thường, còn có những tuyến tàu đặc biệt nối thủ đô trực tiếp với một số thành phố. 

Chính phủ muốn đóng càng nhiều tuyến tàu càng tốt. Hạn chế là sau khi loại bỏ những chuyến tàu đó, khoảng cách ngắn nhất từ ​​mỗi thành phố đến thủ đô phải giữ nguyên như trước. 

Nhiệm vụ không phải là tìm đường đi ngắn nhất sau khi loại bỏ đoàn tàu. Thay vào đó, chúng ta phải xác định có bao nhiêu tuyến tàu không cần thiết trong khi vẫn duy trì mọi khoảng cách ngắn nhất từ ​​thủ đô. 

Biểu đồ chứa tới$10^5$thành phố,$3 \cdot 10^5$đường và$10^5$các tuyến đường tàu. Những giới hạn đó ngay lập tức loại trừ bất kỳ phương pháp nào liên tục tính toán lại các đường đi ngắn nhất. Chạy Dijkstra dù chỉ vài nghìn lần cũng đã quá đắt rồi. Chúng ta cần một cái gì đó gần gũi$O((n+m+k)\log n)$. 

Phần tinh vi nhất của vấn đề là nhiều tuyến tàu có thể đi đến cùng một thành phố. Một số trong số chúng có thể dư thừa ngay cả khi thành phố đó cần có tàu hỏa. 

Coi như:```
1 --(10)-- 2

Train: 1 -> 2 length 5
Train: 1 -> 2 length 7
```Khoảng cách ngắn nhất đến thành phố 2 là 5. Chuyến tàu dài 7 rõ ràng có thể được dỡ bỏ. 

Một trường hợp phức tạp khác là khi một đoàn tàu đạt được khoảng cách ngắn nhất nhưng cũng tồn tại một đường đi ngắn nhất dựa trên đường bộ có chiều dài bằng nhau.```
1 --(2)-- 3 --(3)-- 2

Train: 1 -> 2 length 5
```Khoảng cách ngắn nhất đến thành phố 2 là 5. Việc loại bỏ tàu không thay đổi gì cả vì đường đi cũng có chiều dài 5. Giải pháp giữ cho mọi chuyến tàu tham gia vào một con đường ngắn nhất nào đó sẽ không chính xác. 

Trường hợp tinh vi thứ ba xảy ra khi khoảng cách ngắn nhất trong thành phố chỉ có thể đạt được bằng tàu hỏa.```
1 --(100)-- 2

Train: 1 -> 2 length 5
```Ở đây tàu là cần thiết. Loại bỏ nó sẽ thay đổi khoảng cách ngắn nhất từ 5 đến 100. 

Thách thức là xác định chính xác những chuyến tàu nào là không thể thiếu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra từng tuyến đường tàu một cách độc lập. Đối với mỗi chuyến tàu, hãy tạm thời loại bỏ nó, tính toán lại các tuyến đường ngắn nhất từ ​​thủ đô và kiểm tra xem khoảng cách của thành phố có thay đổi hay không. 

Điều này đúng vì nó trực tiếp kiểm tra điều kiện từ câu lệnh. Thật không may là nó chậm một cách vô vọng. có thể có$10^5$xe lửa và mỗi lần thực hiện Dijkstra tốn khoảng$O((n+m)\log n)$. Trường hợp xấu nhất xảy ra xung quanh$10^5 \times 4 \cdot 10^5 \log 10^5$hoạt động vượt xa giới hạn. 

Quan sát quan trọng là các tuyến đường tàu có thể được coi là các cạnh trong biểu đồ. Một chuyến tàu từ thành phố 1 đến thành phố$s$với chiều dài$y$đơn giản là một cạnh khác$(1,s,y)$. 

Nếu chúng ta chạy Dijkstra trên biểu đồ chứa cả đường bộ và xe lửa, chúng ta sẽ đạt được khoảng cách thực sự ngắn nhất tới mọi thành phố. 

Sau đó, vấn đề trở thành: mép tàu nào thực sự cần thiết để duy trì những khoảng cách ngắn nhất đó? 

Giả sử một thành phố$v$có khoảng cách ngắn nhất$dist[v]$. Bất kỳ cạnh nào$(u,v,w)$thỏa mãn$$dist[u] + w = dist[v]$$có thể là một phần của cây đường đi ngắn nhất. 

Nếu thành phố$v$có ít nhất một mép đường đi ngắn nhất đến thì một đoàn tàu kết thúc tại$v$không cần thiết. Chúng ta có thể tiếp cận$v$qua các con đường trong khi vẫn giữ khoảng cách ngắn nhất như nhau. 

Nếu không có mép đường nào có thể sản xuất$dist[v]$, thì ít nhất một đoàn tàu phải ở lại để đạt được quãng đường ngắn nhất đó. 

Điều này biến vấn đề thành việc đếm xem có bao nhiêu tuyến đường tàu thực sự được yêu cầu bởi cấu trúc đường đi ngắn nhất. 

### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k(n+m)\log n)$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O((n+m+k)\log n)$|$O(n+m+k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Xây dựng biểu đồ bằng cách sử dụng tất cả các cạnh đường. 

Lưu trữ mỗi chuyến tàu riêng biệt. Đối với Dijkstra, hãy thêm từng chuyến tàu làm ranh giới giữa thành phố 1 và thành phố đích của nó. 

Tại thời điểm này đường và xe lửa được coi như các cạnh của đồ thị. 

### Bước 2 

Chạy Dijkstra từ thành phố 1 trên biểu đồ hoàn chỉnh. 

Mảng kết quả`dist`chứa khoảng cách ngắn nhất có thể từ thủ đô đến mọi thành phố khi tất cả các chuyến tàu đều có sẵn. 

Những khoảng cách này chính xác là những giá trị không được thay đổi sau khi đóng các tuyến đường. 

### Bước 3 

Đối với mỗi thành phố, hãy xác định xem có tồn tại mép đường có thể là mép cuối cùng của con đường ngắn nhất hay không. 

Đối với mỗi con đường$(u,v,w)$:

Nếu như$$dist[u] + w = dist[v]$$rồi thành phố$v$có thể đạt được một cách tối ưu thông qua một con đường. 

Tương tự, nếu$$dist[v] + w = dist[u]$$rồi thành phố$u$có thể đạt được một cách tối ưu thông qua một con đường. 

Duy trì một mảng`roadParentPossible`.`roadParentPossible[x] = True`có nghĩa là thành phố`x`có ít nhất một đường dẫn trước ngắn nhất được kết nối bằng một con đường thông thường. 

### Bước 4 

Xử lý tất cả các tuyến đường tàu. 

Đối với một chuyến tàu kết thúc ở thành phố`s`với chiều dài`y`, nó chỉ đóng góp vào đường đi ngắn nhất nếu$$y = dist[s]$$bởi vì tàu xuất phát trực tiếp từ thủ đô có khoảng cách bằng không. 

Bất kỳ chuyến tàu nào với$$y > dist[s]$$không bao giờ có thể thuộc về một đường đi ngắn nhất và có thể loại bỏ ngay lập tức. 

### Bước 5 

Giữa các đoàn tàu thỏa mãn`y == dist[s]`, quyết định những gì là cần thiết. 

Nếu như`roadParentPossible[s]`là đúng thì thành phố`s`đã có sẵn đường đi ngắn nhất. Mọi chuyến tàu đạt được khoảng cách như nhau đều là dư thừa và có thể bị loại bỏ. 

Nếu như`roadParentPossible[s]`là sai thì phải có chuyến tàu đường ngắn nhất nào đó đi đến thành phố`s`. 

Có thể tồn tại nhiều chuyến tàu ngắn nhất cho cùng một thành phố. Trong tình huống đó, hãy giữ chính xác một và loại bỏ tất cả những cái khác. 

Sử dụng bộ đếm cho mỗi thành phố để theo dõi xem có bao nhiêu chuyến tàu hợp lệ ngắn nhất đến đó. 

### Bước 6 

Đếm những chuyến tàu có thể tháo rời. 

Một đoàn tàu có thể tháo rời được nếu: 

1. Nó dài hơn khoảng cách ngắn nhất. 
2. Nó đến một thành phố đã có tuyến đường ngắn nhất trước đó. 
3. Đây là một bản sao bổ sung trong số nhiều chuyến tàu có giá trị ngắn nhất đối với một thành phố cần có tàu. 

### Tại sao nó hoạt động 

Sau Dijkstra,`dist[v]`là khoảng cách ngắn nhất có thể cho mỗi thành phố. Bất kỳ giải pháp bảo tồn nào cũng phải nhận ra những khoảng cách đó. 

Một thành phố tiền thân có đường đi ngắn nhất có thể đạt được khoảng cách tối ưu hoàn toàn thông qua các con đường, do đó không cần phải có chuyến tàu kết thúc ở đó. 

Một thành phố không có tiền thân như vậy phải nhận được khoảng cách ngắn nhất trực tiếp từ tàu. Giữ một chuyến tàu hợp lệ ngắn nhất là đủ vì tất cả các chuyến tàu đều xuất phát từ thủ đô và tạo ra cùng một giá trị khoảng cách. Các chuyến tàu bổ sung đến cùng một thành phố không đóng góp gì. 

Vì vậy, mỗi đoàn tàu bị loại bỏ được chứng minh là không cần thiết và mỗi đoàn tàu được giữ lại phải duy trì ít nhất một khoảng cách ngắn nhất. Do đó, số lượng tàu bị loại bỏ là tối đa. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    roads = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        roads.append((u, v, w))
        graph[u].append((v, w))
        graph[v].append((u, w))

    trains = []

    for _ in range(k):
        s, y = map(int, input().split())
        trains.append((s, y))
        graph[1].append((s, y))
        graph[s].append((1, y))

    INF = 10**30
    dist = [INF] * (n + 1)
    dist[1] = 0

    pq = [(0, 1)]

    while pq:
        d, u = heapq.heappop(pq)

        if d != dist[u]:
            continue

        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    road_parent_possible = [False] * (n + 1)

    for u, v, w in roads:
        if dist[u] + w == dist[v]:
            road_parent_possible[v] = True
        if dist[v] + w == dist[u]:
            road_parent_possible[u] = True

    valid_train_count = [0] * (n + 1)

    for s, y in trains:
        if y == dist[s]:
            valid_train_count[s] += 1

    removable = 0

    for s, y in trains:
        if y > dist[s]:
            removable += 1
        elif road_parent_possible[s]:
            removable += 1
        else:
            if valid_train_count[s] > 1:
                removable += 1
                valid_train_count[s] -= 1

    print(removable)

if __name__ == "__main__":
    solve()
```### Thảo luận triển khai 

Giai đoạn đầu tiên xây dựng một biểu đồ chứa cả đường bộ và tàu hỏa. Dijkstra không cần phải phân biệt giữa chúng vì tất cả những gì chúng ta quan tâm ban đầu là khoảng cách thực sự ngắn nhất. 

Sau khi xác định được khoảng cách ngắn nhất, chỉ có mép đường được kiểm tra để đưa vào`road_parent_possible`. Mảng này xác định các thành phố đã có tuyến đường ngắn nhất kết thúc bằng đường thông thường. 

Mảng`valid_train_count`đếm các chuyến tàu có chiều dài chính xác bằng khoảng cách ngắn nhất để đến đích. Chỉ những chuyến tàu như vậy mới có thể hữu ích. 

Trong lần vượt qua cuối cùng, mọi chuyến tàu đều được phân loại. Những chuyến tàu dài hơn quãng đường ngắn nhất sẽ ngay lập tức bị dư thừa. Nếu tồn tại tuyến đường ngắn nhất trước đó thì mỗi chuyến tàu đến thành phố đó đều dư thừa. Nếu không, chính xác một chuyến tàu hợp lệ ngắn nhất phải tồn tại và tất cả các bản sao bổ sung sẽ bị xóa. 

Một sai lầm phổ biến là cố gắng quyết định mức độ cần thiết của việc tập luyện trong Dijkstra. Các tình huống có khoảng cách bằng nhau khiến cách tiếp cận đó dễ xảy ra lỗi. Việc tính toán tất cả các khoảng cách ngắn nhất trước rồi phân tích biểu đồ đường đi ngắn nhất sẽ rõ ràng hơn nhiều. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5 3
1 2 1
2 3 2
1 3 3
3 4 4
1 5 5
3 5
4 5
5 5
```Sau Dijkstra: 

| Thành phố | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 5 | 
| 5 | 5 | 

Kiểm tra đường: 

| Thành phố | Có người tiền nhiệm con đường ngắn nhất? | 
| --- | --- | 
| 2 | Có | 
| 3 | Có | 
| 4 | Không | 
| 5 | Có | 

Phân tích tàu: 

| Tàu hỏa | Ngắn nhất-hợp lệ? | Đường tiền thân tồn tại? | Có thể tháo rời? | 
| --- | --- | --- | --- | 
| (3,5) | Không | Có | Có | 
| (4,5) | Có | Không | Không | 
| (5,5) | Có | Có | Có | 

Tổng số đoàn tàu có thể tháo rời = 2. 

### Ví dụ hiển thị các chuyến tàu trùng lặp 

đầu vào:```
3 1 3
1 2 10
2 3 10
3 20
3 20
3 20
```Sau Dijkstra: 

| Thành phố | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 10 | 
| 3 | 20 | 

Không có mép đường nào có thể đạt được khoảng cách 20 cho thành phố 3. 

| Tàu hỏa | Có hiệu lực? | 
| --- | --- | 
| 20 | Có | 
| 20 | Có | 
| 20 | Có | 

Một chuyến tàu phải ở lại. 

| Còn lại cần thiết | Có thể tháo rời | 
| --- | --- | 
| 1 | 2 | 

Câu trả lời là 2. 

Ví dụ này chứng minh tại sao việc đếm chính xác các chuyến tàu ngắn nhất trùng lặp là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m+k)\log n)$| Một Dijkstra cộng với quét tuyến tính đường bộ và xe lửa | 
| Không gian |$O(n+m+k)$| Lưu trữ đồ thị, mảng khoảng cách, mảng phụ trợ | 

Đồ thị chứa nhiều nhất$m+k$các cạnh nằm ngoài tính đối xứng trong danh sách kề. Dijkstra thống trị về thời gian chạy. Với$n \le 10^5$Và$m+k \le 4 \cdot 10^5$, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```
# helper verification logic would call solve() and compare output

# Sample 1
assert answer == 2  # provided sample

# Single essential train
# 1--100--2, train length 5
assert answer == 0  # train cannot be removed

# Train worse than road path
# road distance 5, train distance 10
assert answer == 1  # train removable

# Duplicate shortest trains
# three trains of equal optimal length
assert answer == 2  # keep one, remove two

# Road provides same shortest distance
# 1-3 (2), 3-2 (3), train to 2 length 5
assert answer == 1  # train redundant

# Multiple cities requiring trains
assert answer == 1  # only redundant train removed

# Large chain with no trains useful
assert answer == k  # all trains removable

# City reachable only by train
assert answer == 0  # must keep train
```### Tóm tắt bài kiểm tra 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 2 | Ví dụ chính thức | 
| Tàu thiết yếu duy nhất | 0 | Không thể xóa chuyến tàu bắt buộc | 
| Tàu tệ hơn đường | 1 | Di chuyển tàu không ngắn nhất | 
| Nhân đôi chuyến tàu ngắn nhất | 2 | Giữ đúng một chuyến tàu ngắn nhất | 
| Đường bình đẳng thay thế | 1 | Chuyến tàu ngắn nhất vẫn có thể dư thừa | 
| Nhiều điểm đến tàu | Khác nhau | Xử lý thành phố độc lập | 
| Chuỗi lớn | k | Loại bỏ tàu hàng loạt | 
| Chỉ dành cho tàu hỏa | 0 | Phát hiện sự cần thiết | 

## Vỏ cạnh 

### Nhiều chuyến tàu đến cùng một thành phố 

Hãy xem xét:```
3 1 3
1 2 10
2 3 10
3 20
3 20
3 20
```Khoảng cách ngắn nhất tới thành phố 3 là 20. Chưa có con đường tiền nhiệm nào có thể đạt được giá trị đó. Một chuyến tàu phải còn lại, nhưng hai chuyến còn lại thì dư thừa. Thuật toán đếm tất cả các chuyến tàu hợp lệ ngắn nhất và loại bỏ tất cả trừ một chuyến. 

### Một chuyến tàu có quãng đường ngắn nhất nhưng vẫn không cần thiết 

Hãy xem xét:```
3 2 1
1 3 2
3 2 3
2 5
```Thành phố 2 có khoảng cách ngắn nhất là 5. Con tàu cũng có chiều dài là 5. Một giải pháp ngây thơ có thể giữ nó vì nó nằm trên con đường ngắn nhất. Tuy nhiên, đường đi đã đạt được khoảng cách 5.`road_parent_possible[2]`trở thành sự thật nên đoàn tàu bị loại bỏ. 

### Chuyến tàu dài hơn khoảng cách tối ưu 

Hãy xem xét:```
2 1 1
1 2 5
2 10
```Khoảng cách ngắn nhất đến thành phố 2 là 5 qua đường bộ. Chiều dài đoàn tàu là 10, không nằm trong bất kỳ con đường ngắn nhất nào. Thuật toán phát hiện`10 > dist[2]`và loại bỏ nó ngay lập tức. 

### Nhiều đường đi trước ngắn nhất 

Hãy xem xét:```
1--2 (2)
1--3 (2)
2--4 (2)
3--4 (2)
Train 1->4 (4)
```Thành phố 4 có nhiều con đường đi trước ngắn nhất. Khoảng cách ngắn nhất vẫn có thể đạt được hoàn toàn thông qua đường bộ. Đoàn tàu có thể tháo rời và thuật toán đánh dấu chính xác thành phố 4 là tiền thân của con đường có đường đi ngắn nhất khi quét các mép đường.
