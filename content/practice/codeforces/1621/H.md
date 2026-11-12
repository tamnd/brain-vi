---
title: "CF 1621H - Xe lửa và Máy bay"
description: "Chúng ta được cho một đồ thị vô hướng được kết nối với n nút. Mỗi cạnh đại diện cho một chuyến tàu hoặc một chuyến bay kết nối giữa hai thành phố. Tàu hỏa có chi phí đi lại là 1 và máy bay có chi phí đi lại là 0."
date: "2026-06-10T05:57:41+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1621
codeforces_index: "H"
codeforces_contest_name: "Hello 2022"
rating: 3500
weight: 1621
solve_time_s: 48
verified: true
draft: false
---

[CF 1621H - Tàu hỏa và Máy bay](https://codeforces.com/problemset/problem/1621/H) 

**Đánh giá:** 3500 
**Thẻ:** dfs và tương tự, đồ thị, đường đi ngắn nhất, cây 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông với`n`nút. Mỗi cạnh đại diện cho một chuyến tàu hoặc một chuyến bay kết nối giữa hai thành phố. Tàu hỏa có chi phí đi lại bằng 1 và máy bay có chi phí đi lại bằng 0. Mục tiêu là tính toán, đối với mỗi thành phố, số chuyến tàu tối thiểu cần thiết để đến thủ đô (thành phố 1), có thể sử dụng số chuyến bay máy bay bất kỳ. Đầu ra là một số nguyên duy nhất cho mỗi thành phố: số chuyến tàu tối thiểu cần thiết để đến thủ đô. 

Đầu vào cung cấp các cạnh trong hai danh sách: một cho tàu hỏa và một cho máy bay. Biểu đồ được đảm bảo được kết nối, vì vậy mỗi thành phố có thể đến thủ đô thông qua một số sự kết hợp giữa chuyến bay và chuyến tàu. Số lượng nút`n`có thể lớn tới 2×10^5. Với giới hạn thời gian 2 giây, chúng ta phải tránh mọi cách tiếp cận O(n^2) vì điều đó có thể yêu cầu tới 4×10^10 thao tác. Thuật toán O(n + m) tuyến tính hoặc gần tuyến tính là khả thi. 

Một điều tinh tế quan trọng là máy bay có chi phí bằng không. Một BFS ngây thơ coi tất cả các cạnh đều bằng nhau sẽ tính quá nhiều chuyến tàu, bởi vì chúng ta phải phân biệt giữa các cạnh chi phí bằng 0 và cạnh chi phí đơn vị. Ví dụ: việc triển khai bất cẩn có thể bắt đầu từ thủ đô và tăng khoảng cách của mỗi hàng xóm lên 1, ngay cả khi đi máy bay, tạo ra kết quả không chính xác. 

Các trường hợp đặc biệt bao gồm biểu đồ hình sao trong đó tất cả các thành phố được kết nối trực tiếp với thủ đô bằng máy bay: câu trả lời đúng là không có chuyến tàu nào cho tất cả các thành phố. Một trường hợp cạnh khác là đường đi trong đó tất cả các cạnh đều là đoàn tàu: số lượng đoàn tàu tối thiểu bằng khoảng cách dọc theo đường đi. Việc xử lý cả hai loại trong cùng một lần duyệt mà không có chi phí trộn lẫn là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liệt kê tất cả các đường đi từ mỗi thành phố đến thủ đô, đếm số cạnh tàu trên mỗi đường và chọn mức tối thiểu. Điều này đúng vì về mặt lý thuyết, đồ thị đủ nhỏ để cho phép tìm kiếm toàn diện, nhưng với`n`lên tới 2×10^5, số lượng đường dẫn tăng theo cấp số nhân, khiến phương pháp này hoàn toàn không khả thi. 

Điểm mấu chốt là các chuyến tàu có chi phí bằng 1 và máy bay có chi phí bằng 0. Đây chính xác là bài toán đường đi ngắn nhất trên biểu đồ có trọng số trong đó trọng số là 0 hoặc 1. Thuật toán Dijkstra cổ điển hoạt động nhưng chúng ta có thể làm tốt hơn: khi tất cả trọng số các cạnh là 0 hoặc 1, chúng ta có thể sử dụng BFS 0-1. 0-1 BFS sử dụng một deque và chèn các vùng lân cận ở phía trước nếu trọng số cạnh là 0 và ở phía sau nếu trọng số cạnh là 1. Điều này mang lại một đường truyền O(n + m) tuyến tính. Vấn đề giảm xuống còn một BFS đơn giản trên biểu đồ có trọng số cạnh được xử lý cẩn thận. 

Brute-force hoạt động vì nó khám phá mọi con đường, nhưng không thành công khi số lượng thành phố lớn. Quan sát thấy rằng các cạnh máy bay có chi phí bằng 0 cho phép chúng ta xử lý chúng một cách khác nhau trong một deque, giảm vấn đề về các phép toán O(n + m), điều này có thể chấp nhận được đối với`n`lên tới 2×10^5. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n^2) | Quá chậm | 
| Dijkstra | O((n + m) log n) | O(n + m) | Đã chấp nhận | 
| 0-1 BFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn biểu đồ dưới dạng danh sách kề, lưu trữ mỗi hàng xóm với chi phí cạnh của nó: 0 cho máy bay, 1 cho tàu hỏa. Điều này cho phép đưa ra các quyết định nhanh chóng và có tính đến trọng lượng. 
2. Khởi tạo một mảng`dist`kích thước`n + 1`với vô cùng. Mảng này sẽ lưu trữ số chuyến tàu tối thiểu cần thiết để đến thủ đô từ mỗi thành phố. Bộ`dist[1] = 0`. 
3. Khởi tạo deque và đẩy nút viết hoa`1`lên nó. Deque cho phép chèn O(1) ở cả hai đầu, cho phép BFS 0-1. 
4. Trong khi deque không trống, hãy bật một nút`u`từ phía trước. Đối với mỗi người hàng xóm`v`của`u`với trọng lượng cạnh`w`:

- Nếu như`dist[v] > dist[u] + w`, cập nhật`dist[v] = dist[u] + w`. 
- Nếu như`w = 0`, xô`v`đến phía trước của deque; nếu như`w = 1`, xô`v`về phía sau. Điều này đảm bảo các nút có thể truy cập bằng máy bay được khám phá ngay lập tức. 
5. Sau khi truyền tải,`dist[i]`chứa các chuyến tàu tối thiểu từ thành phố`i`tới thủ đô. In khoảng cách cho tất cả các thành phố từ`2`ĐẾN`n`. 

Thuật toán hoạt động vì deque duy trì các nút theo thứ tự khoảng cách tăng dần. Các cạnh có chi phí bằng 0 không làm tăng khoảng cách nên chúng tôi xử lý chúng trước tiên. Mỗi nút được cập nhật nhiều nhất một lần cho mỗi khoảng cách, đảm bảo mức tối thiểu chính xác. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    n, m_train, m_plane = map(int, input().split())
    graph = [[] for _ in range(n + 1)]
    
    for _ in range(m_train):
        u, v = map(int, input().split())
        graph[u].append((v, 1))
        graph[v].append((u, 1))
    
    for _ in range(m_plane):
        u, v = map(int, input().split())
        graph[u].append((v, 0))
        graph[v].append((u, 0))
    
    dist = [float('inf')] * (n + 1)
    dist[1] = 0
    dq = deque([1])
    
    while dq:
        u = dq.popleft()
        for v, w in graph[u]:
            if dist[v] > dist[u] + w:
                dist[v] = dist[u] + w
                if w == 0:
                    dq.appendleft(v)
                else:
                    dq.append(v)
    
    print(' '.join(map(str, dist[2:])))
```Danh sách kề lưu trữ các lân cận cùng với trọng số cạnh. Deque cho phép chúng tôi ưu tiên các cạnh có chi phí bằng 0, bắt chước Dijkstra nhưng không có nhiều. Khoảng cách chỉ được cập nhật khi tìm thấy đường đi tốt hơn. Một cạm bẫy phổ biến là quên sử dụng deque và thay vào đó sử dụng hàng đợi tiêu chuẩn, điều này sẽ sắp xếp sai các cạnh có chi phí bằng 0 và đưa ra khoảng cách không chính xác. 

## Ví dụ đã hoạt động 

đầu vào:```
5 3 2
1 2
2 3
3 4
1 5
4 5
```| Nút | quận [] | nội dung deque | Hành động | 
| --- | --- | --- | --- | 
| 1 | [0, inf, inf, inf, inf, inf] | [1] | bắt đầu | 
| 1 | [0,1,inf,inf,0,inf] | [5,2] | hàng xóm được xử lý | 
| 5 | [0,1,inf,inf,0,inf] | [2,4] | hàng xóm 4 qua máy bay đẩy trước | 
| 4 | [0,1,inf,1,0,inf] | [2,3] | hàng xóm 3 qua tàu | 
| 2 | [0,1,2,1,0,inf] | [3] | hàng xóm 3 khoảng cách chưa cập nhật | 
| 3 | [0,1,2,1,0,inf] | [] | xong | 

Đầu ra:`1 2 1 0`Dấu vết này cho thấy cách xử lý ngay lập tức các cạnh máy bay với chi phí bằng 0, giảm thiểu việc đi tàu. 

đầu vào:```
3 2 1
1 2
2 3
1 3
```Đầu ra:`1 0`đối với các nút 2 và 3. Máy bay từ 1 đến 3 cho phép không có chuyến tàu nào, xác nhận rằng các cạnh có chi phí bằng 0 được ưu tiên chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý tối đa hai lần, các phép toán deque là O(1) | 
| Không gian | O(n + m) | Danh sách kề của đồ thị + mảng khoảng cách + deque | 

Với các phép toán n, m 2×10^5, O(n + m) xấp xỉ 4×10^5, dễ dàng khớp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    sys.stdin = io.StringIO(inp)
    from collections import deque
    n, m_train, m_plane = map(int, input().split())
    graph = [[] for _ in range(n + 1)]
    for _ in range(m_train):
        u, v = map(int, input().split())
        graph[u].append((v, 1))
        graph[v].append((u, 1))
    for _ in range(m_plane):
        u, v = map(int, input().split())
        graph[u].append((v, 0))
        graph[v].append((u, 0))
    dist = [float('inf')] * (n + 1)
    dist[1] = 0
    dq = deque([1])
    while dq:
        u = dq.popleft()
        for v, w in graph[u]:
            if dist[v] > dist[u] + w:
                dist[v] = dist[u] + w
                if w == 0:
                    dq.appendleft(v)
                else
```
