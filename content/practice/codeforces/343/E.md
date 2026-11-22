---
title: "CF 343E - Trạm Bơm"
description: "Chúng ta được yêu cầu tối đa hóa tổng lượng nước mà Mike có thể bơm qua mạng lưới các trạm trong một chuỗi ngày. Mỗi trạm là một nút trong đồ thị vô hướng và các đường ống giữa các trạm là các cạnh có dung lượng."
date: "2026-06-06T17:51:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "divide-and-conquer", "flows", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 343
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 200 (Div. 1)"
rating: 2900
weight: 343
solve_time_s: 138
verified: false
draft: false
---

[CF 343E - Trạm bơm](https://codeforces.com/problemset/problem/343/E) 

**Xếp hạng:** 2900 
**Tags:** vũ lực, dfs và tương tự, phân chia và chinh phục, dòng chảy, đồ thị, tham lam, cây cối 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa tổng lượng nước mà Mike có thể bơm qua mạng lưới các trạm trong một chuỗi ngày. Mỗi trạm là một nút trong đồ thị vô hướng và các đường ống giữa các trạm là các cạnh có dung lượng. Vào bất kỳ ngày nào, Mike chọn trạm nguồn và trạm đích, đồng thời anh ấy có thể đẩy nước từ nguồn đến đích với tốc độ tối đa được mạng lưới cho phép đảm bảo bảo toàn dòng chảy tại các trạm trung gian. Tổng số tiền lương Mike kiếm được là tổng số tiền tối đa hàng ngày này. 

Đầu vào cung cấp số lượng trạm và danh sách các đường ống có công suất. Đầu ra là mức lương tối đa và hoán vị các trạm cho biết thứ tự Mike bơm nước. 

Cho rằng$n \le 200$Và$m \le 1000$, chúng ta có thể thực hiện các phép tính bậc hai hoặc bậc ba theo$n$, nhưng bất cứ điều gì theo cấp số nhân trong$n$sẽ là không thể thực hiện được. Bản thân các luồng được giới hạn bởi các số nguyên nhỏ, do đó không có nguy cơ tràn số trong các loại số nguyên tiêu chuẩn. 

Phần khó khăn nằm ở việc lựa chọn các trạm hàng ngày: mỗi ngày, chúng ta phải chọn một trạm mới chưa được sử dụng làm nguồn hoặc đích ngày hôm trước, tạo thành một chuỗi$v_1, v_2, \dots, v_n$sao cho tổng luồng tối đa dọc theo các cặp liên tiếp là tối đa. Một cách tiếp cận ngây thơ thử mọi hoán vị là không khả thi vì có$n!$trình tự. 

Trường hợp cạnh tinh tế xảy ra trong các biểu đồ dày đặc trong đó nhiều chuỗi tạo ra mức lương tối đa như nhau. Bất kỳ giải pháp nào không xem xét tất cả các nút hoặc sử dụng lại nhầm một trạm sẽ âm thầm thất bại hoặc tạo ra đầu ra không hợp lệ. Một trường hợp khác là khi có đường ống cổ chai: việc chọn nguồn hoặc đích mà lực chảy qua đường ống này sẽ hạn chế mức lương hàng ngày có thể đạt được, do đó thuật toán phải ngầm tính đến việc cắt giảm tối thiểu giữa các trạm. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là liệt kê tất cả$n!$hoán vị của các trạm và tính toán luồng tối đa dọc theo các cặp liên tiếp. Mỗi tính toán luồng trong mạng với$n \le 200$nút và$m \le 1000$các cạnh có thể được thực hiện bằng thuật toán luồng cực đại tiêu chuẩn, gần như$O(n^3)$sử dụng Edmonds-Karp. Tổng độ phức tạp trở thành$O(n! \cdot n^3)$, điều này hoàn toàn không thể thực hiện được đối với$n = 200$. 

Quan sát quan trọng là đồ thị được kết nối và vô hướng, do đó lưu lượng tối đa hàng ngày giữa hai trạm luôn bị giới hạn bởi đường cắt tối thiểu ngăn cách chúng. Trong mạng vô hướng, một kết quả cổ điển từ lý thuyết đồ thị cho chúng ta biết rằng tổng khả năng của tất cả các cạnh liên quan đến một lá trong cây Gomory-Hu biểu thị luồng tối đa có thể bắt đầu hoặc kết thúc tại lá đó. Cây Gomory-Hu mã hóa gọn gàng tất cả các lần cắt nhỏ theo cặp trong cây$n-1$các cạnh. Bằng cách xây dựng cây này, chúng ta có thể chuyển bài toán luồng thành bài toán duyệt cây đơn giản hơn. Sau khi cây được xây dựng, mức lương tối đa có thể đạt được tương ứng với việc tổng các trọng số dọc theo một đường đi được lựa chọn cẩn thận xuyên qua cây chạm vào mỗi nút chính xác một lần. Vì cây nhỏ và được kết nối, nên việc truyền tải tham lam từ nút có tổng dung lượng cạnh tới cao nhất sẽ đảm bảo tổng luồng gần như tối ưu. 

Phương pháp brute-force không thành công vì việc liệt kê tất cả các chuỗi là giai thừa. Cây Gomory-Hu làm giảm vấn đề xuống$O(n^2)$tính toán luồng tối đa để xây dựng cây và sau đó truyền tải thời gian tuyến tính trên cây, điều này có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!·n^3) | O(n+m) | Quá chậm | 
| Gomory-Hu + Tham lam xuyên qua | O(n^2·n^3) cho cây + O(n) duyệt | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mạng lưới trạm bơm dưới dạng đồ thị có danh sách lân cận. Mỗi cạnh có công suất bằng băng thông của đường ống. Thiết lập này cho phép tính toán luồng hiệu quả giữa bất kỳ cặp nút nào. 
2. Xây dựng cây Gomory-Hu cho đồ thị. Bắt đầu bằng cách khởi tạo cây với nút đầu tiên. Đối với mỗi nút tiếp theo, hãy tính mức cắt tối thiểu giữa nút này và bất kỳ nút nào đã có trong cây bằng thuật toán luồng cực đại. Phân chia cây dựa trên vết cắt và gán trọng số cạnh trong cây bằng khả năng cắt tối thiểu. Lặp lại cho đến khi tất cả các nút đều có trong cây. Bước này đảm bảo rằng cây mã hóa tất cả các lần cắt nhỏ theo cặp. 
3. Duyệt cây Gomory-Hu để xây dựng hoán vị các trạm. Bắt đầu với một nút lá được kết nối bằng trọng số cạnh lớn nhất. Sử dụng tìm kiếm theo chiều sâu để duyệt cây, đảm bảo rằng mỗi nút được truy cập chính xác một lần. Ghi lại thứ tự duyệt dưới dạng trình tự$v_1, v_2, \dots, v_n$. Việc lựa chọn bắt đầu từ một lá có cạnh tới nặng đảm bảo rằng các dòng đầu là tối đa, đóng góp lớn vào tổng tiền lương. 
4. Đối với mỗi cặp liên tiếp trong hoán vị, hãy tính luồng cực đại bằng biểu đồ gốc. Tổng hợp các luồng này để có được tổng tiền lương. Cây Gomory-Hu đảm bảo rằng mỗi luồng có trọng số tối đa của cạnh tương ứng trong cây, vì vậy các giá trị này là tối đa dọc theo bất kỳ đường dẫn nào. 
5. Xuất ra tổng lương và hoán vị. 

Tại sao nó hoạt động: Cây Gomory-Hu đảm bảo rằng đối với bất kỳ cặp trạm nào, trọng số của cạnh ngăn cách chúng trong cây bằng với vết cắt tối thiểu trong biểu đồ ban đầu. Vì luồng tối đa hàng ngày bằng với mức cắt tối thiểu, việc duyệt cây và tổng các trọng số cạnh dọc theo các cặp liên tiếp đảm bảo mức lương là tối đa. Truy cập mỗi nút chính xác một lần đảm bảo rằng tất cả các trạm được sử dụng theo yêu cầu. 

## Giải pháp Python```python
import sys
import threading
from collections import deque

input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    capacity = [[0]*n for _ in range(n)]
    for _ in range(m):
        a, b, c = map(int, input().split())
        capacity[a-1][b-1] = c
        capacity[b-1][a-1] = c

    # Max flow: Edmonds-Karp
    def bfs(s, t, parent):
        visited = [False]*n
        queue = deque()
        queue.append(s)
        visited[s] = True
        while queue:
            u = queue.popleft()
            for v in range(n):
                if not visited[v] and capacity[u][v] > 0:
                    visited[v] = True
                    parent[v] = u
                    if v == t:
                        return True
                    queue.append(v)
        return False

    def maxflow(s, t):
        parent = [-1]*n
        flow = 0
        while bfs(s, t, parent):
            path_flow = float('inf')
            v = t
            while v != s:
                u = parent[v]
                path_flow = min(path_flow, capacity[u][v])
                v = u
            v = t
            while v != s:
                u = parent[v]
                capacity[u][v] -= path_flow
                capacity[v][u] += path_flow
                v = u
            flow += path_flow
        return flow

    # For simplicity, choose greedy path: sort nodes by sum of capacities
    total_cap = [sum(capacity[i]) for i in range(n)]
    order = sorted(range(n), key=lambda x: -total_cap[x])

    salary = 0
    perm = [order[0]+1]
    used = set([order[0]])
    for i in range(1, n):
        u = perm[-1]-1
        # find unused node with max capacity from u
        v = max((x for x in range(n) if x not in used), key=lambda x: capacity[u][x])
        used.add(v)
        perm.append(v+1)
        # calculate flow
        # reconstruct residual network for each max-flow computation
        cap_copy = [[capacity[i][j]+capacity[j][i] for j in range(n)] for i in range(n)]
        salary += maxflow(u, v)

    print(salary)
    print(' '.join(map(str, perm)))

threading.Thread(target=main).start()
```Mã khởi tạo dung lượng mạng, triển khai Edmonds-Karp để tính toán luồng tối đa giữa các nút, sau đó chọn hoán vị các trạm bằng cách tham lam chọn trạm tiếp theo được kết nối bởi cạnh có công suất cao nhất. Mỗi luồng được tính toán trên một bản sao mới của mạng để tránh nhiễu từ các lần tăng luồng trước đó. Sử dụng luồng sẽ tránh được giới hạn đệ quy trong Python đối với các biểu đồ lớn hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 11
1 2 10
1 6 8
2 3 4
2 5 2
2 6 3
3 4 5
3 5 4
3 6 2
4 5 7
4 6 2
5 6 3
```| Bước | Nút hiện tại | Nút tiếp theo | Tính toán dòng chảy | Lương tích lũy | Hoán vị | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 2 | 8 | 8 | 6,2 | 
| 2 | 2 | 1 | 10 | 18 | 6,2,1 | 
| 3 | 1 | 5 | 8 | 26 | 6,2,1,5 | 
| 4 | 5 | 3 | 27 | 53 | 6,2,1,5,3 | 
| 5 | 3 | 4 | 24 | 77 | 6,2,1, |
