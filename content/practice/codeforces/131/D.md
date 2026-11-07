---
title: "CF 131D - Tàu điện ngầm"
description: "Chúng ta có một hệ thống tàu điện ngầm gồm n ga được nối với nhau bằng đúng n lối đi, mỗi lối đi nối hai ga riêng biệt. Hệ thống tạo thành một biểu đồ kết nối trong đó mỗi trạm có thể tiếp cận mọi trạm khác."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 131
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 95 (Div. 2)"
rating: 1600
weight: 131
solve_time_s: 74
verified: true
draft: false
---

[CF 131D - Tàu điện ngầm](https://codeforces.com/problemset/problem/131/D) 

**Đánh giá:** 1600 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ thống tàu điện ngầm gồm _n_ ga được kết nối bởi chính xác _n_ lối đi, mỗi lối đi nối hai ga riêng biệt. Hệ thống tạo thành một biểu đồ kết nối trong đó mỗi trạm có thể tiếp cận mọi trạm khác. Theo lý thuyết đồ thị, một đồ thị liên thông có _n_ nút và _n_ cạnh chứa đúng một chu trình. Chu kỳ này tương ứng với “đường vành đai” trong bài toán. Mục đích là tính toán khoảng cách từ mỗi trạm đến đường vành đai, là số đoạn đường tối thiểu phải đi qua để đến bất kỳ trạm nào trên đường vòng. Các trạm là một phần của chu trình có khoảng cách bằng không. 

Đầu vào bao gồm _n_ dòng sau dòng đầu tiên, mỗi dòng mô tả kết nối hai chiều giữa hai trạm. Đầu ra là một dãy _n_ số, trong đó số thứ i là khoảng cách từ ga _i_ đến đường vành đai. 

Ràng buộc _n 3000_ có nghĩa là các giải pháp có độ phức tạp về thời gian lên tới khoảng O(n²) sẽ chạy thoải mái trong giới hạn 2 giây. Một cách tiếp cận đơn giản liên tục tìm kiếm khoảng cách mà không khai thác cấu trúc biểu đồ vẫn có thể quá chậm nếu truy cập các nút một cách không cần thiết. 

Các trường hợp ở rìa cần cẩn thận bao gồm các chu kỳ nhỏ, trong đó hầu hết tất cả các trạm đều nằm trên đường vành đai và các chuỗi tuyến tính được gắn vào chu trình, trong đó các khoảng cách cần được truyền chính xác dọc theo các nhánh. Ví dụ: một biểu đồ có hình tam giác có đuôi dài gắn vào một nút yêu cầu các nút của đuôi đó được gán khoảng cách tăng dần chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trước tiên sẽ cố gắng tìm đường vành đai bằng cách kiểm tra tất cả các chu kỳ có thể. Điều này liên quan đến việc liệt kê các chu kỳ và xác minh tính duy nhất của chúng, điều này trở nên tốn kém về mặt tổ hợp, khoảng O(n³) để kiểm tra kề. Khi đã biết chu kỳ, BFS từ mỗi trạm đến chu trình có thể tính toán khoảng cách theo O(n²). Mặc dù đúng nhưng đây là công việc không cần thiết vì các thuộc tính của biểu đồ cho phép tiếp cận trực tiếp hơn. 

Quan sát quan trọng là đồ thị được kết nối và có đúng một chu trình. Điều này ngụ ý rằng bất kỳ trạm nào không có trong chu trình đều là một phần của cây có gốc tại một số nút chu trình. Do đó, bài toán rút gọn thành hai bước: đầu tiên xác định các nút trong chu trình, sau đó truyền khoảng cách đến chu trình dọc theo các nhánh cây. 

Việc phát hiện chu trình có thể được thực hiện một cách hiệu quả bằng tìm kiếm theo chiều sâu được sửa đổi. Khi phát hiện thấy cạnh sau trong DFS, nó đánh dấu sự hiện diện của chu trình. Theo dõi đường dẫn trong DFS cho phép chúng tôi xác định chính xác các nút nào đang trong chu trình. Khi chu trình được xác định, tìm kiếm theo chiều rộng bắt đầu từ tất cả các nút chu trình sẽ đồng thời truyền khoảng cách ra bên ngoài. Mỗi trạm sẽ được tiếp cận dọc theo con đường ngắn nhất đến chu trình, đảm bảo phân bổ khoảng cách chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các chu kỳ + BFS trên mỗi nút) | O(n³) | O(n²) | Quá chậm | 
| DFS cho chu trình + BFS cho khoảng cách | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Trình bày tàu điện ngầm dưới dạng danh sách lân cận trong đó mỗi ga ánh xạ tới danh sách các ga mà nó kết nối tới. Cấu trúc này cho phép lặp O(1) qua các hàng xóm. 
2. Chạy tìm kiếm theo chiều sâu để phát hiện chu trình duy nhất. Theo dõi nút gốc của mỗi nút trong DFS. Nếu gặp một nút đã truy cập không phải là nút cha thì một chu trình sẽ được tìm thấy. Ghi lại tất cả các nút dọc theo đường dẫn DFS giữa nút hiện tại và nút đã truy cập để xác định chu trình. 
3. Khởi tạo mảng khoảng cách với -1 cho tất cả các trạm, nghĩa là "không được ghé thăm". Đặt khoảng cách 0 cho tất cả các trạm được xác định trong chu trình. 
4. Sử dụng hàng đợi để thực hiện tìm kiếm theo chiều rộng bắt đầu từ tất cả các nút chu kỳ cùng một lúc. Đối với mỗi nút được loại bỏ hàng đợi, hãy lặp lại các nút lân cận của nó. Nếu khoảng cách của hàng xóm là -1, hãy đặt nó bằng khoảng cách của nút hiện tại cộng với 1 và xếp nó vào hàng đợi. 
5. Tiếp tục BFS cho đến khi tất cả các nút được chỉ định khoảng cách. Ở cuối, mảng khoảng cách chứa khoảng cách tối thiểu từ mỗi ga đến đường vành đai. 

Tại sao nó hoạt động: DFS đảm bảo nhận dạng chu trình duy nhất vì biểu đồ có chính xác một chu trình. BFS truyền khoảng cách từ chu trình ra ngoài theo lớp và vì tất cả các cạnh đều có trọng số bằng nhau nên lần đầu tiên một nút được truy cập là thông qua đường đi ngắn nhất đến chu trình. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline
sys.setrecursionlimit(10000)

n = int(input())
adj = [[] for _ in range(n)]
for _ in range(n):
    u, v = map(int, input().split())
    adj[u - 1].append(v - 1)
    adj[v - 1].append(u - 1)

visited = [False] * n
parent = [-1] * n
cycle_nodes = []

def dfs(u, p):
    visited[u] = True
    for v in adj[u]:
        if v == p:
            continue
        if visited[v]:
            # cycle detected, reconstruct path
            cycle_nodes.append(v)
            node = u
            while node != v:
                cycle_nodes.append(node)
                node = parent[node]
            return True
        parent[v] = u
        if dfs(v, u):
            return True
    return False

dfs(0, -1)

dist = [-1] * n
q = deque()
for node in cycle_nodes:
    dist[node] = 0
    q.append(node)

while q:
    u = q.popleft()
    for v in adj[u]:
        if dist[v] == -1:
            dist[v] = dist[u] + 1
            q.append(v)

print(' '.join(map(str, dist)))
```Danh sách lân cận đại diện cho tàu điện ngầm một cách hiệu quả. Hàm DFS theo dõi các nút cha để xây dựng lại chu trình khi gặp cạnh sau. BFS đảm bảo tất cả các khoảng cách đều được truyền chính xác, với các nút chu kỳ ban đầu ở khoảng cách bằng 0. Cần thận trọng để tránh xem lại các nút và xử lý đầu vào dựa trên 1 bằng cách chuyển đổi sang các chỉ mục dựa trên 0. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
1 3
4 3
4 2
1 2
```| Nút | DFS gốc | Nút chu kỳ | Hàng đợi BFS | Mảng khoảng cách | 
| --- | --- | --- | --- | --- | 
| 1 | - | 1,2,3,4 | 1,2,3,4 | 0,0,0,0 | 

Tất cả các nút là một phần của chu trình, vì vậy BFS không mở rộng thêm nữa. Mỗi khoảng cách là 0, như mong đợi. 

### Tùy chỉnh đầu vào 2```
6
1 2
2 3
3 1
3 4
4 5
5 6
```| Nút | DFS gốc | Nút chu kỳ | Hàng đợi BFS | Mảng khoảng cách | 
| --- | --- | --- | --- | --- | 
| 1 | - | 1,2,3 | 1,2,3 | 0,0,0,-1,-1,-1 | 
| 4 | 3 | | 4 | 1 | 
| 5 | 4 | | 5 | 2 | 
| 6 | 5 | | 6 | 3 | 

Điều này cho thấy BFS truyền khoảng cách dọc theo cây gắn liền với chu trình. Nút 4 cách đó 1 đoạn, nút 5 là 2 và nút 6 là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS và một lần trong BFS. Mỗi cạnh được đi qua nhiều nhất hai lần. | 
| Không gian | O(n) | Danh sách kề, mảng cha, mảng đã truy cập và hàng đợi BFS đều có tỷ lệ tuyến tính. | 

Với _n 3000_, điều này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline
    sys.setrecursionlimit(10000)

    n = int(input())
    adj = [[] for _ in range(n)]
    for _ in range(n):
        u, v = map(int, input().split())
        adj[u - 1].append(v - 1)
        adj[v - 1].append(u - 1)

    visited = [False] * n
    parent = [-1] * n
    cycle_nodes = []

    def dfs(u, p):
        visited[u] = True
        for v in adj[u]:
            if v == p:
                continue
            if visited[v]:
                cycle_nodes.append(v)
                node = u
                while node != v:
                    cycle_nodes.append(node)
                    node = parent[node]
                return True
            parent[v] = u
            if dfs(v, u):
                return True
        return False

    dfs(0, -1)

    dist = [-1] * n
    q = deque()
    for node in cycle_nodes:
        dist[node] = 0
        q.append(node)

    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    return ' '.join(map(str, dist))

# Provided sample
assert run("4\n1 3\n4 3\n4 2\n1 2\n") == "0 0 0 0", "sample 1"

# Custom tests
assert run("6\n1 2\n2 3\n3 1\n3 4\n4 5\n5 6\n") == "0 0 0 1 2 3", "triangle with tail"
assert run("3\n1 2\n2 3\n3 1\n") == "
```
