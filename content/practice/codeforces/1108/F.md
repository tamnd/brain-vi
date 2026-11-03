---
title: "CF 1108F - Thống nhất MST"
description: "Chúng ta có một đồ thị vô hướng liên thông có n đỉnh và m cạnh, mỗi cạnh có trọng số dương. Mục tiêu là điều chỉnh một số trọng số cạnh bằng cách tăng chúng, sao cho cây bao trùm tối thiểu (MST) của đồ thị vẫn có cùng chi phí như ban đầu nhưng trở nên duy nhất."
date: "2026-06-12T05:19:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dsu", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1108
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 535 (Div. 3)"
rating: 2100
weight: 1108
solve_time_s: 45
verified: true
draft: false
---

[CF 1108F - Hợp nhất MST](https://codeforces.com/problemset/problem/1108/F) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, dsu, đồ thị, tham lam 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông với`n`đỉnh và`m`các cạnh, mỗi cạnh có trọng số dương. Mục tiêu là điều chỉnh một số trọng số cạnh bằng cách tăng chúng, sao cho cây bao trùm tối thiểu (MST) của đồ thị vẫn có cùng chi phí như ban đầu nhưng trở nên duy nhất. Nói cách khác, ban đầu biểu đồ có thể có nhiều MST có cùng tổng chi phí và chúng tôi muốn phá vỡ các ràng buộc theo cách tối thiểu bằng cách tăng nhẹ trọng số cạnh mà không thay đổi tổng chi phí MST. 

Đầu vào cung cấp biểu đồ dưới dạng danh sách các cạnh có điểm cuối và trọng số. Đầu ra là một số nguyên duy nhất: số lượng thao tác "tăng thêm một" tối thiểu cần thiết để đạt được MST duy nhất. 

Các ràng buộc rất chặt chẽ:`n`Và`m`có thể lên tới 200.000 và trọng lượng cạnh có thể đạt tới`10^9`. Bất kỳ giải pháp nào cố gắng liệt kê tất cả MST hoặc kiểm tra tất cả các chu kỳ một cách rõ ràng sẽ quá chậm vì ngay cả`O(m^2)`hoạt động là không khả thi. Điều này cho thấy chúng ta cần một giải pháp hoạt động hiệu quả trong`O(m log n)`thời gian, lý tưởng nhất là kết hợp việc xây dựng MST với một cách hiệu quả để xác định các cạnh không rõ ràng. 

Trường hợp cạnh tinh tế là một biểu đồ trong đó có nhiều cạnh có cùng trọng số và tồn tại nhiều MST. Ví dụ, một hình tam giác có các cạnh`(1,2,1)`,`(2,3,1)`,`(1,3,1)`có ba MST. Bất kỳ thuật toán đơn giản nào chỉ chọn một MST và tăng các cạnh không có trong đó có thể vô tình thay đổi chi phí MST hoặc tăng nhiều cạnh hơn mức cần thiết. Cách tiếp cận đúng sẽ xác định cẩn thận các cạnh mà việc tăng trọng lượng của chúng sẽ phá vỡ các mối liên kết mà không ảnh hưởng đến chi phí MST ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả MST, tìm các cạnh có trong một số chứ không phải tất cả MST và tăng chúng. Điều này hoạt động vì tính duy nhất của MST chỉ thất bại khi có một cạnh bên ngoài MST hiện tại có thể thay thế cạnh MST mà không làm tăng tổng chi phí. Tuy nhiên, việc liệt kê tất cả MST là không thực tế; ngay cả việc sử dụng kiểm tra chu trình cho từng cạnh không phải MST cũng dẫn đến`O(m * n)`hoặc tệ hơn. Với`n`lên tới`2 * 10^5`, tốc độ này quá chậm. 

Cái nhìn sâu sắc quan trọng là sự mơ hồ của MST chỉ phát sinh từ các cạnh bên ngoài MST có trọng số bằng với trọng số cạnh tối đa trên chu trình mà chúng sẽ hình thành trong MST. Nếu chúng ta xem xét cạnh MST và cạnh không MST`(u, v, w)`, nó tạo thành một chu trình với MST. Nếu như`w`bằng trọng lượng tối đa dọc theo chu kỳ đó trong MST, chúng ta có thể hoán đổi các cạnh và nhận một MST khác. Để tránh sự mơ hồ, chúng ta phải tăng trọng lượng của một số cạnh không phải MST này ở mức tối thiểu để chúng không còn khớp với mức tối đa dọc theo chu kỳ của chúng. Điều này làm giảm vấn đề tính toán, đối với mỗi cạnh không phải MST, trọng số cạnh tối đa dọc theo đường dẫn duy nhất trong MST giữa các điểm cuối của nó. 

Việc tính toán trọng số cạnh tối đa dọc theo các đường đi có thể được thực hiện một cách hiệu quả bằng cách sử dụng nâng nhị phân với`O(n log n)`tiền xử lý, sau đó truy vấn từng cạnh không phải MST trong`O(log n)`. Tổng thời gian là`O(m log n)`, phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n) | O(n + m) | Quá chậm | 
| Tối ưu | O(m log n) | O(n log n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng MST bất kỳ của đồ thị bằng thuật toán Kruskal. Lưu trữ các cạnh thuộc về MST. Điều này hợp lệ vì tất cả MST đều có tổng trọng lượng như nhau và mọi MST đều có tác dụng để phân tích. 
2. Xây dựng MST dưới dạng cây và chuẩn bị các bảng nâng nhị phân cho các truy vấn LCA. Đối với mỗi nút, lưu trữ tổ tiên ở lũy thừa hai và trọng số cạnh tối đa trên đường dẫn tới mỗi tổ tiên. Điều này cho phép truy vấn trọng số tối đa trên đường đi giữa hai nút bất kỳ trong`O(log n)`. 
3. Đối với mỗi cạnh không phải MST`(u, v, w)`, tính trọng số cạnh tối đa dọc theo đường đi từ`u`ĐẾN`v`trong MST bằng cách sử dụng các bảng nâng nhị phân. Nếu như`w`bằng mức tối đa này thì cạnh này có thể được hoán đổi với cạnh MST để tạo ra một MST khác. 
4. Đếm từng cạnh như vậy. Việc tăng trọng lượng của nó lên một đơn vị sẽ phá vỡ sự mơ hồ vì nó sẽ vượt quá trọng lượng tối đa trong suốt chu kỳ của nó. Số lượng thao tác tối thiểu là tổng số cạnh tới hạn này. 
5. Xuất số đếm này. 

Tại sao nó hoạt động: Tính duy nhất của MST tương đương với việc đảm bảo không có cạnh nào không phải MST có trọng số bằng mức tối đa trên đường dẫn MST kết nối các điểm cuối của nó. Thuật toán xác định chính xác các cạnh này và tăng chúng. Nâng nhị phân đảm bảo truy vấn đường dẫn tối đa chính xác cho tất cả các cạnh không phải MST một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def main():
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1))
    edges.sort()
    
    parent = list(range(n))
    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x
    def union(x, y):
        x, y = find(x), find(y)
        if x == y: return False
        parent[y] = x
        return True

    mst_edges = []
    for w, u, v in edges:
        if union(u, v):
            mst_edges.append((u, v, w))
    
    # build tree for LCA
    from collections import defaultdict, deque
    adj = defaultdict(list)
    for u, v, w in mst_edges:
        adj[u].append((v, w))
        adj[v].append((u, w))
    
    LOG = 20
    up = [[-1]*LOG for _ in range(n)]
    max_edge = [[0]*LOG for _ in range(n)]
    depth = [0]*n
    
    def dfs(u, p):
        for v, w in adj[u]:
            if v == p: continue
            up[v][0] = u
            max_edge[v][0] = w
            depth[v] = depth[u] + 1
            for i in range(1, LOG):
                if up[v][i-1] != -1:
                    up[v][i] = up[up[v][i-1]][i-1]
                    max_edge[v][i] = max(max_edge[v][i-1], max_edge[up[v][i-1]][i-1])
            dfs(v, u)
    
    dfs(0, -1)
    
    def query(u, v):
        if depth[u] < depth[v]:
            u, v = v, u
        res = 0
        for i in reversed(range(LOG)):
            if up[u][i] != -1 and depth[up[u][i]] >= depth[v]:
                res = max(res, max_edge[u][i])
                u = up[u][i]
        if u == v:
            return res
        for i in reversed(range(LOG)):
            if up[u][i] != -1 and up[u][i] != up[v][i]:
                res = max(res, max_edge[u][i], max_edge[v][i])
                u, v = up[u][i], up[v][i]
        res = max(res, max_edge[u][0], max_edge[v][0])
        return res

    mst_set = set((min(u,v), max(u,v)) for u,v,w in mst_edges)
    ans = 0
    for w, u, v in edges:
        if (min(u,v), max(u,v)) in mst_set:
            continue
        if w == query(u,v):
            ans += 1
    print(ans)

if __name__ == "__main__":
    main()
```Mã đầu tiên xây dựng MST bằng Kruskal và đánh dấu các cạnh MST. Nó xây dựng cấu trúc cây với các danh sách kề để chạy DFS và điền vào các bảng nâng nhị phân cho các truy vấn biên tối đa. Sau đó, mỗi cạnh không phải MST sẽ được kiểm tra: nếu trọng số của nó bằng mức tối đa dọc theo đường đi của nó trong MST thì đó là một ứng cử viên để tăng. Đếm tất cả các cạnh như vậy sẽ có câu trả lời. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
n = 8, m = 10
edges = [
1 2 1, 2 3 2, 2 4 5, 1 4 2, 6 3 3,
6 1 3, 3 5 2, 3 7 1, 4 8 1, 6 2 4
]
```Sau Kruskal, các cạnh MST bao gồm các trọng số 1,1,1,2,2,2,3. Cạnh`(6,2,4)`nằm ngoài MST và tạo thành một chu trình có trọng số tối đa 4. Vì`w == max_cycle`, việc tăng nó sẽ giải quyết được sự mơ hồ. Chỉ có một cạnh như vậy tồn tại. Đầu ra là`1`. 

**Đầu vào tùy chỉnh:**```
n = 3, m = 3
edges = [
1 2 1, 2 3 1, 1 3 1
]
```Tất cả các cạnh đều có trọng số 1. MST chọn hai cạnh bất kỳ. Cạnh không MST`(1,3,1)`có`w == max_cycle`và phải được tăng lên. Đầu ra là`1`. 

| Bước | bạn | v | tối đa trên đường dẫn | là cạnh quan trọng? | đếm | 
| --- | --- | --- | --- | --- | --- | 
| kiểm tra (1, | | | | | |
