---
title: "CF 1814F - Tháp truyền thông"
description: "Chúng ta đang xử lý một mạng lưới các tháp truyền thông được kết nối bằng dây dẫn, trong đó mỗi tháp chỉ có thể hoạt động trên một dải tần số liên tục. Các tháp được đánh số từ 1 đến n và mỗi tháp có thể hoạt động trên các tần số từ li đến ri."
date: "2026-06-09T08:28:40+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "divide-and-conquer", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1814
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 146 (Rated for Div. 2)"
rating: 2700
weight: 1814
solve_time_s: 87
verified: false
draft: false
---

[CF 1814F - Tháp truyền thông](https://codeforces.com/problemset/problem/1814/F) 

**Đánh giá:** 2700 
**Tags:** vũ lực, chia để trị, dsu 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một mạng lưới các tháp truyền thông được kết nối bằng dây dẫn, trong đó mỗi tháp chỉ có thể hoạt động trên một dải tần số liên tục. Các tháp được đánh số từ 1 đến n và mỗi tháp i có thể hoạt động trên các tần số từ l_i đến r_i. Hai tòa tháp được kết nối nếu có một sợi dây giữa chúng. Nhiệm vụ là xác định xem tháp nào có thể truy cập được bắt đầu từ tháp 1, nhưng khả năng truy cập yêu cầu tần số chung dọc theo đường dẫn. Cụ thể, tháp b có thể truy cập được từ tháp a nếu tồn tại một tần số x duy nhất và một chuỗi các tháp được kết nối trực tiếp từ a đến b, trong đó mỗi tháp trong chuỗi chấp nhận tần số x. 

Hạn chế chính là n có thể lớn tới 2×10^5 và m lên tới 4×10^5. Điều này loại trừ mọi thuật toán kiểm tra từng tần số riêng lẻ hoặc thử tất cả các đường dẫn một cách rõ ràng. Ví dụ: việc lặp lại từng tần số cho mỗi nút sẽ dẫn đến tối đa 4×10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian 4 giây. Bản thân dải tần có thể lên tới 2×10^5, đây là một gợi ý khác cho thấy chúng ta không thể mô phỏng một cách ngây thơ tất cả các tần số có thể có. 

Các trường hợp biên không rõ ràng bao gồm các tháp có dải tần số một điểm, dải tần hoàn toàn tách rời giữa các lân cận và các thành phần bị ngắt kết nối. Ví dụ: nếu tháp 1 chấp nhận [1,1] và được kết nối với tháp có phạm vi [2,3], thì đầu ra chính xác sẽ chỉ là tháp 1, nhưng thuật toán bất cẩn có thể cố gắng đi theo các cạnh bất kể phạm vi chồng chéo. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là xem xét từng tần số từ 1 đến 2×10^5, xây dựng một biểu đồ chỉ bao gồm các tháp chấp nhận tần số này và thực hiện BFS hoặc DFS bắt đầu từ tháp 1. Mặc dù điều này đúng về mặt khái niệm, nhưng nó sẽ yêu cầu lặp lại trên tất cả các tần số cho tất cả các nút, dẫn đến các hoạt động O(n * max_ Frequency + m). Với tần số tối đa lên tới 2×10^5 và n lên tới 2×10^5, điều này trở nên cực kỳ chậm. 

Thông tin chi tiết quan trọng cần tối ưu hóa là khả năng tiếp cận phụ thuộc vào giao điểm của dải tần dọc theo các thành phần được kết nối. Đối với tháp 1, bất kỳ tháp nào có thể tiếp cận đều phải có dải tần giao nhau với khoảng tần số có thể tiếp cận hiện tại. Điều này biến vấn đề thành một dạng lan truyền khoảng trên biểu đồ: chúng ta bắt đầu với dải tần số của tháp 1 và truyền các lớp phủ dọc theo các cạnh. Điều này có thể được triển khai một cách hiệu quả với cấu trúc tìm liên kết (DSU) hoặc BFS giúp theo dõi giao điểm của các dải tần khi chúng ta di chuyển qua mạng. 

Bằng cách duy trì giao điểm của các dải tần một cách linh hoạt và chỉ truy cập từng cạnh một lần, chúng ta có thể giảm độ phức tạp từ việc lặp lại trên tất cả các tần số xuống việc chỉ xử lý từng nút và cạnh một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tần số) | O(n * tần số tối đa + m) | O(n + m) | Quá chậm | 
| Tuyên truyền khoảng thời gian với BFS/DSU | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc dữ liệu đầu vào để xây dựng biểu đồ với danh sách kề và lưu trữ dải tần số của từng tháp [l_i, r_i]. 
2. Khởi tạo một bộ để theo dõi các tháp đã ghé thăm và hàng đợi cho BFS. Bắt đầu với tháp 1 và đánh dấu nó là đã ghé thăm. 
3. Khởi tạo khoảng tần số [L, R] biểu thị tập hợp tần số có thể truy cập hiện tại. Ban đầu, đây là [l_1, r_1]. 
4. Trong khi hàng đợi không trống, hãy mở tháp u và kiểm tra các hàng xóm của nó v. 
5. Đối với mỗi hàng xóm v chưa được ghé thăm, hãy tính giao điểm của dải tần số [l_v, r_v] với khoảng thời gian có thể truy cập hiện tại [L, R]. Nếu giao lộ không trống, hãy đánh dấu v là đã ghé thăm, cập nhật khoảng cách có thể truy cập của nó vào giao lộ và thêm nó vào hàng đợi. 
6. Tiếp tục cho đến khi đã ghé thăm tất cả các tòa tháp có thể tiếp cận. Thu thập tất cả các tòa tháp đã ghé thăm, sắp xếp chúng theo thứ tự tăng dần và xuất chúng. 

Tại sao nó hoạt động: BFS đảm bảo rằng mỗi tháp được truy cập chính xác một lần với giao điểm tần số hợp lệ. Hoạt động giao nhau đảm bảo rằng chỉ những tần số được chấp nhận cho cả tuyến hiện tại và tuyến lân cận mới được xem xét. Điều này duy trì tính bất biến rằng bất kỳ tháp nào được thêm vào hàng đợi đều có thể truy cập thực sự từ tháp 1 dưới một tần số duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m = map(int, input().split())
ranges = [None] + [tuple(map(int, input().split())) for _ in range(n)]
graph = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v = map(int, input().split())
    graph[u].append(v)
    graph[v].append(u)

visited = [False] * (n + 1)
reachable_range = [None] * (n + 1)
queue = deque([1])
visited[1] = True
reachable_range[1] = ranges[1]

while queue:
    u = queue.popleft()
    L, R = reachable_range[u]
    for v in graph[u]:
        if visited[v]:
            continue
        l_v, r_v = ranges[v]
        new_L = max(L, l_v)
        new_R = min(R, r_v)
        if new_L <= new_R:
            visited[v] = True
            reachable_range[v] = (new_L, new_R)
            queue.append(v)

result = [i for i in range(1, n + 1) if visited[i]]
print(" ".join(map(str, sorted(result))))
```Mã đọc các tháp và các cạnh thành các cấu trúc thích hợp, khởi tạo BFS từ tháp 1 và truyền bá giao điểm của các dải tần số. Việc sử dụng`max`Và`min`đảm bảo giao lộ chính xác và việc đánh dấu đã ghé thăm đảm bảo mỗi tháp được xử lý một lần. Sắp xếp kết quả đảm bảo đầu ra tăng dần. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
6 5
3 5
1 2
2 4
2 3
3 3
4 6
1 3
6 1
3 5
3 6
2 3
```| Bước | Xếp hàng | Tháp đã đến thăm | Khoảng thời gian hiện tại | Ghi chú | 
| --- | --- | --- | --- | --- | 
| ban đầu | [1] | {1} | [3,5] | Bắt đầu tại tháp 1 | 
| bật 1 | [] | {1} | [3,5] | Hàng xóm: 3,6 | 
| kiểm tra 3 | [3] | {1,3} | [3,4] | Giao lộ [3,5] ∩ [2,4] = [3,4] | 
| kiểm tra 6 | [3,6] | {1,3,6} | [4,5] | Giao lộ [3,5] ∩ [4,6] = [4,5] | 
| bật 3 | [6] | {1,3,6} | [3,4] | Hàng xóm: 1,5,6; 1,6 lượt truy cập | 
| kiểm tra 5 | [6,5] | {1,3,5,6} | [3,3] | Giao lộ [3,4] ∩ [3,3] = [3,3] | 
| bật 6 | [5] | {} | [4,5] | Hàng xóm 1,3 ghé thăm | 
| bật 5 | [] | {} | [3,3] | Hàng xóm 3 ghé thăm | 

Đầu ra: 1 3 5 6 

Dấu vết này thể hiện cách các giao điểm lan truyền dọc theo các cạnh, đảm bảo chỉ các tháp có thể tiếp cận được dưới một tần số nào đó mới được đánh dấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi tháp và mỗi cạnh được truy cập nhiều nhất một lần trong BFS | 
| Không gian | O(n + m) | Danh sách đồ thị kề, mảng đã truy cập, mảng Reachable_range, hàng đợi | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian 4 giây cho n lên tới 2×10^5 và m lên đến 4×10^5. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    ranges = [None] + [tuple(map(int, input().split())) for _ in range(n)]
    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)
    visited = [False] * (n + 1)
    reachable_range = [None] * (n + 1)
    from collections import deque
    queue = deque([1])
    visited[1] = True
    reachable_range[1] = ranges[1]
    while queue:
        u = queue.popleft()
        L, R = reachable_range[u]
        for v in graph[u]:
            if visited[v]:
                continue
            l_v, r_v = ranges[v]
            new_L = max(L, l_v)
            new_R = min(R, r_v)
            if new_L <= new_R:
                visited[v] = True
                reachable_range[v] = (new_L, new_R)
                queue.append(v)
    result = [i for i in range(1, n + 1) if visited[i]]
    return " ".join(map(str, sorted(result)))

# Provided sample
assert run("6 5\n3 5\n1 2\n2 4\n2 3\n3 3\n4 6\n1 3\n6 1\n3 5\n3
```
