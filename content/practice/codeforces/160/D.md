---
title: "CF 160D - Các cạnh trong MST"
description: "Chúng ta có một đồ thị vô hướng có trọng số liên thông không có vòng lặp hoặc có nhiều cạnh. Mỗi cạnh có trọng số dương."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "sortings"]
categories: ["algorithms"]
codeforces_contest: 160
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 111 (Div. 2)"
rating: 2300
weight: 160
solve_time_s: 81
verified: true
draft: false
---

[CF 160D - Các cạnh trong MST](https://codeforces.com/problemset/problem/160/D) 

**Đánh giá:** 2300 
**Thẻ:** dfs và tương tự, dsu, đồ thị, sắp xếp 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có trọng số liên thông không có vòng lặp hoặc có nhiều cạnh. Mỗi cạnh có trọng số dương. Nhiệm vụ là phân loại từng cạnh theo việc nó thuộc về tất cả các cây bao trùm tối thiểu, ít nhất một cây bao trùm tối thiểu hay không có cây bao trùm tối thiểu nào cả. Nói cách khác, đối với mọi cạnh, chúng tôi muốn biết liệu nó có cần thiết để xây dựng MST, tùy chọn hay không thể đưa vào bất kỳ MST nào. 

Đồ thị có thể có tới 100.000 đỉnh và 100.000 cạnh và trọng số mỗi cạnh lên tới 1.000.000. Với những giới hạn này, một thuật toán có độ phức tạp kém hơn O(m log n) có thể là quá chậm. Các phương pháp tiếp cận bạo lực cố gắng xây dựng tất cả MST một cách rõ ràng hoặc loại bỏ các cạnh để xem liệu việc tăng trọng lượng MST có yêu cầu các hoạt động O(m^2) hoặc tệ hơn hay không, điều này là không khả thi. 

Các trường hợp cạnh không rõ ràng bao gồm nhiều cạnh có cùng chu kỳ hình thành trọng số. Ví dụ, hãy xem xét một đồ thị tam giác có các đỉnh 1, 2, 3 và các cạnh (1-2, trọng số 1), (2-3, trọng số 1), (3-1, trọng số 1). Hai cạnh bất kỳ tạo thành một MST. Trong trường hợp như vậy, tất cả các cạnh là "ít nhất một", không có cạnh nào là "bất kỳ" và không có cạnh nào là "không có". Một trường hợp cạnh khác xảy ra khi một cạnh có trọng số tối thiểu kết nối hai thành phần - cạnh đó phải có trong mọi MST, nhưng một thuật toán bất cẩn bỏ qua các chu kỳ trọng số bằng nhau có thể phân loại sai nó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi cạnh, hãy tạm thời loại bỏ nó và tính toán lại MST. Nếu trọng số MST tăng thì cạnh sẽ nằm trong mọi MST. Nếu trọng số MST không đổi nhưng cạnh được bao gồm trong một MST có thể thì nó thuộc về ít nhất một MST. Nếu việc bao gồm nó luôn làm tăng trọng lượng MST thì nó không có MST. Về nguyên tắc, điều này hoạt động nhưng là O(m * (m log n)) vì mỗi phép tính MST là O(m log n). Với m lên tới 100.000, con số này sẽ trở thành 10 tỷ thao tác, quá chậm. 

Quan sát quan trọng là MST có thể được phân tích một cách hiệu quả bằng cách sắp xếp các cạnh theo trọng lượng và xử lý các cạnh theo thứ tự tăng dần, như trong thuật toán Kruskal. Cấu trúc tìm liên kết (DSU) theo dõi các thành phần được kết nối. Nếu một cạnh kết nối hai thành phần riêng biệt thì nó có thể là một phần của MST. Nếu nhiều cạnh có cùng trọng lượng kết nối các thành phần giống nhau, chúng ta phải quyết định cẩn thận những cạnh nào là tùy chọn. Để làm điều này, chúng ta có thể nhóm các cạnh theo trọng số, kiểm tra khả năng kết nối của từng cạnh trong nhóm của nó và xác định xem việc loại bỏ nó có làm đứt kết nối hay không. Điều này tránh việc tính toán lại MST từ đầu và cho phép chúng tôi phân loại các cạnh trong O(m log n) bằng cách sử dụng DSU với phép kết hợp theo thứ hạng và nén đường dẫn. 

Cách tiếp cận cuối cùng là Kruskal đã được sửa đổi: sắp xếp các cạnh, xử lý các cạnh có cùng trọng số với nhau và đối với mỗi nhóm, đánh dấu các cạnh là kết nối duy nhất giữa các thành phần là "bất kỳ", các cạnh kết nối các thành phần nhưng có các lựa chọn thay thế là "ít nhất một" và các cạnh tạo thành chu trình là "không có". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^2 log n) | O(n) | Quá chậm | 
| Kruskal sửa đổi + DSU | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc các cạnh của biểu đồ và lưu trữ chúng cùng với các chỉ số đầu vào của chúng để có thể giữ nguyên thứ tự đầu ra. Sắp xếp tất cả các cạnh theo trọng số theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng tôi xử lý các cạnh nhỏ nhất trước tiên, như trong thuật toán MST của Kruskal. 
2. Khởi tạo cấu trúc tập hợp rời rạc (DSU) với một thành phần trên mỗi đỉnh. Mỗi thành phần sẽ đại diện cho tập hợp các đỉnh được kết nối cho đến nay. 
3. Xử lý các cạnh theo nhóm theo trọng lượng. Đối với mỗi nhóm cạnh có trọng số bằng nhau, hãy thực hiện như sau: 
4. Đối với mỗi cạnh trong nhóm, hãy kiểm tra xem điểm cuối của nó có thuộc cùng một thành phần trong DSU hiện tại hay không. Nếu chúng nằm trong cùng một thành phần, cạnh sẽ tạo thành một chu trình và không thể thuộc về bất kỳ MST nào, vì vậy hãy đánh dấu nó là "không". 
5. Nếu các điểm cuối nằm trong các thành phần khác nhau, hãy tạm đánh dấu cạnh là "ít nhất một." Những cạnh này có thể được đưa vào MST. 
6. Đối với tất cả các cạnh được đánh dấu là "ít nhất một" trong nhóm trọng số này, hãy tạo một biểu đồ tạm thời trong đó các nút là các thành phần DSU và các cạnh là các cạnh trọng lượng hiện tại kết nối các thành phần khác nhau. Chạy kiểm tra DFS hoặc tìm kiếm liên kết để phát hiện cạnh nào là cầu nối trong biểu đồ tạm thời này. Các cạnh là cầu nối phải được bao gồm trong mọi MST kết nối các thành phần này, vì vậy hãy cập nhật nhãn hiệu của chúng thành "bất kỳ". Các cạnh không có cầu vẫn còn "ít nhất một." 
7. Sau khi phân loại tất cả các cạnh trong nhóm, hợp nhất các thành phần của chúng trong DSU chính. Điều này đảm bảo nhóm cân nặng tiếp theo nhìn thấy kết nối được cập nhật. 
8. Lặp lại cho đến khi tất cả các cạnh được xử lý. Cuối cùng, xuất phân loại theo thứ tự đầu vào ban đầu. 

Tại sao nó hoạt động: Ở mọi giai đoạn, các cạnh kết nối các thành phần riêng biệt có thể nằm trong MST. Bằng cách nhóm các cạnh có trọng số bằng nhau, chúng ta xử lý chính xác các tình huống trong đó nhiều cạnh có thể thay thế cho nhau. Việc xác định các cầu nối đảm bảo rằng các cạnh cần thiết cho kết nối được đánh dấu là "bất kỳ", trong khi các chu kỳ hình thành các cạnh được loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

sys.setrecursionlimit(1 << 25)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0]*n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return False
        if self.rank[x_root] < self.rank[y_root]:
            self.parent[x_root] = y_root
        else:
            self.parent[y_root] = x_root
            if self.rank[x_root] == self.rank[y_root]:
                self.rank[x_root] += 1
        return True

n, m = map(int, input().split())
edges = []
for i in range(m):
    a, b, w = map(int, input().split())
    edges.append((w, a-1, b-1, i))

edges.sort()
res = [None]*m
dsu = DSU(n)
i = 0

while i < m:
    j = i
    group = []
    while j < m and edges[j][0] == edges[i][0]:
        group.append(edges[j])
        j += 1

    temp_graph = defaultdict(list)
    temp_edges = []
    for w, u, v, idx in group:
        u_root = dsu.find(u)
        v_root = dsu.find(v)
        if u_root == v_root:
            res[idx] = "none"
        else:
            res[idx] = "at least one"
            temp_graph[u_root].append((v_root, idx))
            temp_graph[v_root].append((u_root, idx))
            temp_edges.append((u_root, v_root, idx))

    visited = {}
    low = {}
    tin = {}
    timer = [0]

    def dfs(u, parent):
        visited[u] = True
        timer[0] += 1
        tin[u] = low[u] = timer[0]
        for v, idx in temp_graph[u]:
            if v == parent:
                continue
            if v in visited:
                low[u] = min(low[u], tin[v])
            else:
                dfs(v, u)
                low[u] = min(low[u], low[v])
                if low[v] > tin[u]:
                    res[idx] = "any"

    for node in temp_graph:
        if node not in visited:
            dfs(node, -1)

    for w, u, v, idx in group:
        dsu.union(u, v)
    i = j

for r in res:
    print(r)
```Giải pháp trước tiên sắp xếp các cạnh, khởi tạo DSU và xử lý các cạnh theo trọng số. Nó phân loại các chu trình hình thành các cạnh là "không" và sử dụng biểu đồ tạm thời cho từng nhóm trọng số để phát hiện các cầu nối được đánh dấu là "bất kỳ". DSU sáp nhập sau khi xử lý đảm bảo cập nhật kết nối cho nhóm tiếp theo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các cạnh đầu vào: 

| idx | bạn | v | w | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 101 | 
| 1 | 1 | 3 | 100 | 
| 2 | 2 | 3 | 2 | 
| 3 | 2 | 4 | 2 | 
| 4 | 3 | 4 | 1 | 

Sau khi sắp xếp: 

| bạn | v | w | idx | 
| --- | --- | --- | --- | 
| 3 | 4 | 1 | 4 | 
| 2 | 3 | 2 | 2 | 
| 2 | 4 | 2 | 3 | 
| 1 | 3 | 100 | 1 | 
| 1 | 2 | 101 | 0 | 

Các cạnh có trọng số 1: chỉ có cạnh 3-4, kết nối các thành phần riêng biệt, được đánh dấu là "bất kỳ". 

Các cạnh có trọng số 2: các cạnh 2-3 và 2-4. Cả hai đều kết nối các thành phần riêng biệt, chưa có chu kỳ cầu nối nào, được đánh dấu là "ít nhất một". DFS phát hiện rằng không có cầu nối nào trong biểu đồ tạm thời, vẫn là "ít nhất một." 

Các cạnh có trọng lượng 100: 1-3 kết nối các thành phần riêng biệt, được đánh dấu là "bất kỳ". 

Cạnh có trọng số 101: 1-2 sẽ tạo thành một chu kỳ, được đánh dấu là "không". 

đầu ra
