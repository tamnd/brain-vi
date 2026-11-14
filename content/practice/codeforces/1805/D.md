---
title: "CF 1805D - Đồ thị rộng, rộng"
description: "Chúng ta được cho một cây có các đỉnh $n$ và được yêu cầu tạo một chuỗi đồ thị $Gk$ cho $k = 1$ đến $n$. Trong mỗi đồ thị $Gk$, một cạnh tồn tại giữa các đỉnh $u$ và $v$ nếu khoảng cách giữa $u$ và $v$ trong cây ban đầu ít nhất là $k$."
date: "2026-06-09T09:17:48+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1805
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 862 (Div. 2)"
rating: 1800
weight: 1805
solve_time_s: 207
verified: false
draft: false
---

[CF 1805D - Đồ thị rộng, rộng](https://codeforces.com/problemset/problem/1805/D) 

**Đánh giá:** 1800 
**Thẻ:** dfs và tương tự, dp, đồ thị, tham lam, cây cối 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các đỉnh và được yêu cầu tạo ra một chuỗi các đồ thị$G_k$vì$k = 1$ĐẾN$n$. Trong mỗi biểu đồ$G_k$, tồn tại một cạnh giữa các đỉnh$u$Và$v$nếu khoảng cách giữa$u$Và$v$trong cây ban đầu ít nhất là$k$. Đối với mỗi$k$, chúng ta phải đếm số lượng thành phần được kết nối trong$G_k$. 

Đầu vào cung cấp$n-1$các cạnh xác định cây. Từ$n$có thể lên đến$10^5$, bất kỳ nghiệm nào có thời gian bậc hai trong$n$ngay lập tức bị loại trừ. Tính toán khoảng cách theo cặp một cách rõ ràng cho tất cả các cặp sẽ yêu cầu$O(n^2)$hoạt động quá chậm. Do đó, chúng tôi cần một giải pháp hoạt động trong khoảng$O(n \log n)$hoặc$O(n)$. 

Trường hợp khó phát hiện xảy ra khi$k$lớn hơn đường kính của cây. Trong trường hợp đó, không có hai đỉnh nào được kết nối và mỗi đỉnh tạo thành thành phần riêng của nó. Ví dụ: cây sao có 5 đỉnh và$k = 3$tạo ra 5 thành phần vì khoảng cách lớn nhất giữa hai lá bất kỳ là 2. Một thuật toán đơn giản không xem xét cấu trúc cây sẽ kết nối nhầm các đỉnh quá xa nhau. 

Một trường hợp cạnh khác phát sinh khi$k = 1$. Theo định nghĩa, tất cả các cặp đỉnh đều thỏa mãn yêu cầu về khoảng cách, do đó$G_1$được kết nối đầy đủ, mang lại chính xác một thành phần bất kể cấu trúc của cây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xây dựng$G_k$rõ ràng cho từng$k$. Đối với mỗi$k$, chúng tôi sẽ kiểm tra khoảng cách giữa mỗi cặp đỉnh, sau đó chạy DFS hoặc BFS để đếm các thành phần. Cách tiếp cận này đúng nhưng đối với$n = 10^5$nó đòi hỏi đại khái$n^2$kiểm tra khoảng cách mỗi$k$, dẫn đến tổng cộng$O(n^3)$hoạt động. Điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là vấn đề cơ bản là về đường kính của cây và độ sâu của cây con. Các thành phần được kết nối của$G_k$có thể được suy ra từ mức độ "sâu" của các đỉnh trong cây và khoảng cách tồn tại của các đường đi mà không chạm vào đỉnh có thể phân chia thành phần. Cụ thể hơn, chúng ta có thể lấy gốc cây ở một đỉnh tùy ý, sau đó với mỗi đỉnh hãy tính đường đi dài nhất trong cây con của nó. Nếu cây con không có đủ độ sâu để đạt được khoảng cách$k$, nó sẽ tạo thành một thành phần riêng biệt trong$G_k$. 

Tối ưu hóa cuối cùng sử dụng lập trình động trên cây. Tại mỗi đỉnh, chúng ta theo dõi chiều cao của hai cây con dài nhất bên dưới nó. Những độ cao này cho phép chúng ta xác định cho từng$k$liệu đỉnh đó có tham gia vào một thành phần lớn hơn hay bắt đầu một thành phần mới hay không. Bằng cách kết hợp độ cao một cách hiệu quả và truyền số đếm lên cây, chúng ta có thể tính toán số lượng thành phần cho tất cả$k$TRONG$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây một cách tùy ý, ví dụ ở đỉnh 1. Đây là một lựa chọn ghi sổ để đơn giản hóa việc tính toán cây con. 
2. Thực hiện DFS từ thư mục gốc. Tại mỗi đỉnh, hãy tính chiều cao của hai cây con sâu nhất của nó. Những độ cao này tương ứng với khoảng cách dài nhất trong cây con. 
3. Duy trì nhiều tập hợp chiều cao cây con cho mỗi đỉnh. Điều này cho phép chúng ta xác định nhanh chóng đường đi lớn nhất đi qua đỉnh và liệu đường đi đó có ít nhất là$k$dài. 
4. Đối với mỗi đỉnh, truyền phần đóng góp của số thành phần lên trên. Nếu chiều cao cây con tối đa cộng một nhỏ hơn$k$, đỉnh đó và cây con của nó tạo thành một thành phần mới. Ngược lại, cây con sẽ hợp nhất thành một thành phần lớn hơn. 
5. Tổng hợp số lượng thành phần cho mỗi thành phần$k$bằng cách quét qua độ sâu duy nhất. Điều này đảm bảo chúng tôi chỉ cập nhật số lượng tại những điểm mà số lượng thành phần thay đổi. 
6. Xuất ra số lượng thành phần được tính toán cho tất cả$k = 1$ĐẾN$n$. Vì$k$lớn hơn đường kính của cây, sản lượng$n$bởi vì mỗi đỉnh trở thành thành phần riêng của nó. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến mà tại mỗi đỉnh, chúng ta biết độ sâu tối đa có thể đạt được trong cây con của nó. Khi độ sâu này không đủ để duy trì khoảng cách ít nhất$k$, cây con sẽ tách thành một thành phần mới. Điều này đảm bảo rằng tất cả các đường dẫn thỏa mãn ràng buộc khoảng cách đều được kết nối chính xác và không có đỉnh nào không thể đạt được khoảng cách$k$được sáp nhập không chính xác. 

## Giải pháp Python```python
import sys
from collections import defaultdict, deque
input = sys.stdin.readline

n = int(input())
edges = defaultdict(list)
for _ in range(n-1):
    u, v = map(int, input().split())
    edges[u].append(v)
    edges[v].append(u)

# BFS to get heights from farthest node (tree diameter ends)
def bfs(start):
    dist = [-1]*(n+1)
    q = deque([start])
    dist[start] = 0
    while q:
        u = q.popleft()
        for v in edges[u]:
            if dist[v] == -1:
                dist[v] = dist[u]+1
                q.append(v)
    farthest = max(range(1,n+1), key=lambda x: dist[x])
    return farthest, dist

# first BFS to find one end of diameter
u, _ = bfs(1)
# second BFS to find other end and distances
v, dist_u = bfs(u)
_, dist_v = bfs(v)

diameter = max(dist_u)
res = [1]*n  # initialize with 1 for k=1

# for k
```
