---
title: "CF 104536E - Tối đa hóa LIS"
description: "Lỗi không đến từ chính logic cây mà từ hành vi khai thác thử nghiệm trong giải pháp được cung cấp. Triệu chứng chính là dòng này: và thực tế là đầu ra là một chuỗi trống bên trong trình chạy thử chứ không phải là giá trị được tính toán."
date: "2026-06-30T09:43:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104536
codeforces_index: "E"
codeforces_contest_name: "SashaT9 Contest 1"
rating: 0
weight: 104536
solve_time_s: 198
verified: false
draft: false
---

[CF 104536E - Tối đa hóa LIS](https://codeforces.com/problemset/problem/104536/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Chẩn đoán 

Lỗi không đến từ chính logic cây mà từ hành vi khai thác thử nghiệm trong giải pháp được cung cấp. 

Triệu chứng chính là dòng này:```
AssertionError
```và thực tế là đầu ra là một chuỗi trống bên trong bộ chạy thử chứ không phải là giá trị được tính toán. 

Điều này xảy ra vì “giải pháp” trước đó đã xác định:```
return ""
```bên trong`run()`helper thay vì thực sự gọi logic của bộ giải. Vì vậy, bài kiểm tra không hề đánh giá thuật toán. Nó đang âm thầm trả về một chuỗi trống, điều này rõ ràng là không thành công`"5"`. 

Riêng biệt, ngay cả khi điều đó đã được khắc phục, vẫn có vấn đề về tính chính xác thứ hai trong tính toán bán kính: 

Công thức được sử dụng là:```
radius = min(max(dist_u[i], dist_v[i]))
```Điều này đúng cho các điểm cuối đường kính nhưng chỉ sau khi đảm bảo`dist_u`Và`dist_v`tương ứng với các điểm cuối đối diện thực sự của đường kính. Phần đó ổn. 

Tuy nhiên, vấn đề về độ bền thực sự nằm ở cấu trúc: việc triển khai giả định các nút được lập chỉ mục 1 cho đến`len(adj)-1`, sẽ bị hỏng nếu kích thước kề được sử dụng lại không chính xác hoặc nếu phân tích cú pháp đầu vào bị xử lý sai. 

Nhưng sự cố ngay lập tức hoàn toàn là do cuộc gọi nhỡ. 

## Ý tưởng giải pháp đúng 

Chúng ta phải: 

1. Xây dựng cả hai cây. 
2. Chạy BFS hai lần cho mỗi cây để lấy điểm cuối đường kính. 
3. Tính bán kính bằng cách sử dụng khoảng cách từ cả hai điểm cuối. 
4. Kết hợp sử dụng:```
answer = max(d1, d2, r1 + r2 + 1)
```Không có logic khai thác thử nghiệm nào sẽ cản trở việc thực thi. 

## Giải pháp Python đúng```python
import sys
from collections import deque

input = sys.stdin.readline

def bfs(start, adj):
    n = len(adj) - 1
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0

    far = start

    while q:
        v = q.popleft()
        for to in adj[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)
                if dist[to] > dist[far]:
                    far = to

    return far, dist

def tree_info(adj):
    u, _ = bfs(1, adj)
    v, dist_u = bfs(u, adj)
    _, dist_v = bfs(v, adj)

    diameter = dist_u[v]

    radius = 10**18
    for i in range(1, len(adj)):
        radius = min(radius, max(dist_u[i], dist_v[i]))

    return diameter, radius

def solve():
    n = int(input())
    adj1 = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj1[u].append(v)
        adj1[v].append(u)

    m = int(input())
    adj2 = [[] for _ in range(m + 1)]
    for _ in range(m - 1):
        u, v = map(int, input().split())
        adj2[u].append(v)
        adj2[v].append(u)

    d1, r1 = tree_info(adj1)
    d2, r2 = tree_info(adj2)

    print(max(d1, d2, r1 + r2 + 1))

if __name__ == "__main__":
    solve()
```## Điều gì thực sự đã xảy ra 

Thất bại ban đầu không phải là thuật toán. Hồi quy đến từ việc trộn “mã trình bao bọc thử nghiệm đơn vị” với giải pháp thực tế: 

Hàm dùng để kiểm tra trả về một chuỗi trống thay vì gọi bộ giải, do đó mọi xác nhận đều không thành công bất kể tính đúng đắn. 

Sau khi loại bỏ điều đó, phương pháp kết hợp đường kính + bán kính BFS hoàn toàn phù hợp với các ràng buộc của bài toán và chạy theo thời gian tuyến tính trên mỗi cây. 

Nếu bạn muốn, tôi cũng có thể hiển thị một mẫu rõ ràng hơn cho lập trình cạnh tranh trong đó bộ giải được tách biệt khỏi bất kỳ khai thác kiểm tra nào để loại lỗi này không thể xảy ra nữa.
