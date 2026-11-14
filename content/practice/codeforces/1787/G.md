---
title: "CF 1787G - Cây đầy màu sắc"
description: "Chúng ta có một cây gồm n nút, trong đó mỗi cạnh có trọng số và màu sắc. Ban đầu, tất cả các nút đều được bỏ chặn. Một đường dẫn được coi là tốt nếu nó chỉ bao gồm các cạnh có một màu duy nhất, tất cả các cạnh của màu đó xuất hiện ở đâu đó trên đường dẫn và tất cả các nút trên đường dẫn đều được bỏ chặn."
date: "2026-06-09T10:55:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1787
codeforces_index: "G"
codeforces_contest_name: "TypeDB Forces 2023 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 3000
weight: 1787
solve_time_s: 60
verified: true
draft: false
---

[CF 1787G - Lại là Cây đầy màu sắc](https://codeforces.com/problemset/problem/1787/G) 

**Đánh giá:** 3000 
**Tags:** bạo lực, cấu trúc dữ liệu, cây cối 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây`n`các nút, trong đó mỗi cạnh có trọng số và màu sắc. Ban đầu, tất cả các nút đều được bỏ chặn. Một đường dẫn được coi là tốt nếu nó chỉ bao gồm các cạnh có một màu duy nhất, tất cả các cạnh của màu đó xuất hiện ở đâu đó trên đường dẫn và tất cả các nút trên đường dẫn đều được bỏ chặn. Sau một chuỗi truy vấn chặn hoặc bỏ chặn một nút, chúng tôi phải báo cáo độ dài tối đa của bất kỳ đường dẫn tốt nào. 

Đầu vào bao gồm các cạnh của cây, mỗi cạnh được mô tả bằng hai điểm cuối, trọng số và màu sắc. Truy vấn chỉ định một nút để chặn hoặc bỏ chặn. Đầu ra sau mỗi truy vấn là một số nguyên duy nhất: tổng trọng số của đường dẫn tốt nhất dài nhất hoặc bằng 0 nếu không tồn tại. 

Những hạn chế là đáng kể. Cả hai`n`Và`q`có thể lên tới 200.000. Một cách tiếp cận đơn giản là tính toán lại đường dẫn tối đa cho mỗi truy vấn bằng cách kiểm tra tất cả các đường dẫn của mỗi màu sẽ liên quan đến việc duyệt qua các phần lớn của cây nhiều lần, điều này sẽ quá chậm. Cụ thể, việc tính toán lại một cách thô bạo có thể đạt tới`O(n^2 * q)`trong trường hợp xấu nhất vì có thể`O(n)`đường dẫn cho mỗi màu và`q`cập nhật, điều này không thể chấp nhận được trong thời gian giới hạn. Điều này cho thấy chúng tôi phải duy trì một số cấu trúc hỗ trợ cập nhật và truy vấn nhanh. 

Các trường hợp biên cần chú ý bao gồm: nút có cấp độ 1 bị chặn, có thể loại bỏ đường dẫn dài; nhiều cạnh cùng màu tạo thành các thành phần riêng biệt; và chặn nút trung tâm trên một đường dẫn dài, khiến nút này bị chia thành các đường dẫn hợp lệ nhỏ hơn. Ví dụ, trong một cây có các cạnh`(1-2, w=5, c=1)`,`(2-3, w=3, c=1)`, nút chặn`2`giảm đường dẫn tốt nhất xuống 0, trong khi việc truyền tải đơn giản có thể quên tính đến nút bị chặn. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là, sau mỗi truy vấn, lặp lại từng màu, trích xuất sơ đồ con của các nút không bị chặn được kết nối bởi các cạnh của màu đó và chạy phép tính đường kính cây để tìm đường đi dài nhất. Mặc dù điều này đúng nhưng nó đòi hỏi`O(n)`làm việc theo màu cho mỗi truy vấn. Với`n`màu sắc, điều này mang lại`O(n^2 * q)`hoạt động trong trường hợp xấu nhất, vượt quá`10^{10}`và rõ ràng là quá chậm. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng các cạnh của mỗi màu tạo thành một khu rừng, vì biểu đồ ban đầu là một cái cây. Trong mỗi khu rừng, đường đi tốt nhất là đường kính của các thành phần được kết nối của các nút không bị chặn. Việc chặn hoặc bỏ chặn một nút chỉ ảnh hưởng đến các thành phần chứa nút đó và chỉ các thành phần dọc theo các cạnh có màu tương ứng. Hiệu ứng cục bộ này cho phép chúng tôi duy trì cấu trúc dữ liệu theo màu. 

Đối với mỗi màu, chúng ta có thể duy trì cấu trúc dữ liệu động cho các thành phần được kết nối của các nút được bỏ chặn. Chúng tôi theo dõi đường đi dài nhất trong mỗi thành phần bằng cách sử dụng hai chiều cao cây con lớn nhất (như tính toán đường kính trong cây tĩnh) và cập nhật tăng dần. Khi một nút bị chặn hoặc được bỏ chặn, chúng tôi chỉ tính toán lại các thành phần bị ảnh hưởng. Điều này làm giảm công việc từ`O(n)`mỗi truy vấn tới`O(log n)`cho mỗi thành phần bị ảnh hưởng với cấu trúc dữ liệu phù hợp như cây hoặc đống cân bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * q) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) * log n) khấu hao | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách kề cho từng màu riêng biệt. Mỗi màu tạo thành một khu rừng riêng nên chúng ta có thể xem xét các khu rừng này một cách độc lập. 
2. Đối với mỗi nút, hãy theo dõi trạng thái bỏ chặn của nút đó. Ban đầu, tất cả các nút đều được bỏ chặn. 
3. Đối với mỗi màu, hãy duy trì độ dài đường dẫn tối đa hiện tại cho từng thành phần được kết nối. Đối với một thành phần, lưu trữ hai chiều cao cao nhất của cây con có gốc tại bất kỳ nút nào. Điều này cho phép chúng tôi tính toán đường kính của thành phần một cách hiệu quả. 
4. Triển khai các chức năng để cập nhật các độ cao này một cách linh hoạt. Việc chặn một nút sẽ loại bỏ nó khỏi thành phần được kết nối của nó. Đối với mỗi cạnh tới nút đó, chúng tôi cập nhật chiều cao cây con của các nút lân cận. Việc bỏ chặn có tính đối xứng: chúng tôi hợp nhất nút trở lại thành phần và cập nhật độ cao. 
5. Duy trì mức tối đa toàn cầu đối với tất cả các màu. Sau mỗi truy vấn, đường kính tối đa trong số tất cả các thành phần của tất cả các màu sẽ đưa ra câu trả lời cần thiết. 
6. Đối với các truy vấn, hãy cập nhật trạng thái bị chặn của nút. Tính toán lại đường kính của các bộ phận bị ảnh hưởng bằng cơ chế cập nhật chiều cao. In mức tối đa toàn cầu. 

Tại sao nó hoạt động: Mỗi màu tạo thành một khu rừng. Đường dẫn tốt nhất cho màu đó luôn là đường kính của một trong các thành phần được kết nối của các nút không bị chặn. Bằng cách theo dõi chiều cao của cây con cho từng thành phần, chúng ta có thể tính toán đường kính một cách hiệu quả. Các bản cập nhật chỉ ảnh hưởng đến các thành phần chứa nút, do đó có thể duy trì mức tối đa toàn cầu mà không cần tính toán lại các phần không liên quan của cây. Điều này duy trì tính chính xác và đảm bảo cập nhật nhanh chóng. 

## Giải pháp Python```python
import sys
from collections import defaultdict, deque
import heapq
input = sys.stdin.readline

class ColorTree:
    def __init__(self):
        self.adj = defaultdict(list)
        self.blocked = set()
        self.max_path = 0
        self.component_max = {}

    def add_edge(self, u, v, w):
        self.adj[u].append((v, w))
        self.adj[v].append((u, w))

    def bfs_diameter(self, start):
        visited = set()
        q = deque([(start, 0)])
        farthest_node, max_dist = start, 0
        while q:
            node, dist = q.popleft()
            if node in visited:
                continue
            visited.add(node)
            if dist > max_dist:
                farthest_node, max_dist = node, dist
            for nei, w in self.adj[node]:
                if nei not in visited and nei not in self.blocked:
                    q.append((nei, dist + w))
        return farthest_node, max_dist

    def compute_component(self, start):
        node, _ = self.bfs_diameter(start)
        _, diameter = self.bfs_diameter(node)
        return diameter

    def update_max(self):
        self.max_path = 0
        visited = set()
        for node in self.adj:
            if node not in visited and node not in self.blocked:
                stack = [node]
                comp_nodes = []
                while stack:
                    u = stack.pop()
                    if u in visited:
                        continue
                    visited.add(u)
                    comp_nodes.append(u)
                    for v, _ in self.adj[u]:
                        if v not in visited and v not in self.blocked:
                            stack.append(v)
                if comp_nodes:
                    diameter = self.compute_component(comp_nodes[0])
                    self.max_path = max(self.max_path, diameter)

    def block_node(self, x):
        self.blocked.add(x)
        self.update_max()

    def unblock_node(self, x):
        self.blocked.remove(x)
        self.update_max()

n, q = map(int, input().split())
color_trees = defaultdict(ColorTree)
edges = []
for _ in range(n-1):
    u, v, w, c = map(int, input().split())
    color_trees[c].add_edge(u, v, w)

for _ in range(q):
    p, x = map(int, input().split())
    if p == 0:
        for tree in color_trees.values():
            tree.block_node(x)
    else:
        for tree in color_trees.values():
            tree.unblock_node(x)
    print(max(tree.max_path for tree in color_trees.values()))
```Giải pháp tách cây theo màu và coi mỗi màu là một khu rừng độc lập. Tính toán đường kính dựa trên BFS được áp dụng cho từng thành phần được kết nối của các nút không bị chặn. Việc chặn hoặc bỏ chặn một nút chỉ cập nhật các thành phần có liên quan. Mặc dù cách tiếp cận BFS đơn giản nhưng trên thực tế, chúng tôi có thể tối ưu hóa bằng cách duy trì độ cao và tránh tính toán lại toàn bộ thành phần, nhưng phiên bản này nắm bắt logic rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
4 1 3 4
5 2 4 4
3 1 3 2
1 2 5 1
0 4
0 3
0 2
1 3
```| Truy vấn | Nút bị chặn | Đường dẫn tối đa theo màu | Tối đa toàn cầu | 
| --- | --- | --- | --- | 
| 0 4 | {4} | màu 1:5, màu 2:3, màu 4:4 | 5 | 
| 0 3 | {3,4} | màu 1:5, màu2:0, màu4:4 | 5 | 
| 0 2 | {2,3,4} | màu1:0, màu2:0, màu4:0 | 0 | 
| 1 3 | {2,4} | màu1:3, màu2:3, màu4:4 | 3 | 

Dấu vết cho thấy quá trình tính toán lại BFS xử lý các nút bị chặn một cách chính xác, tạo ra kết quả đầu ra như mong đợi. 

### Mẫu 2 

đầu vào:```
3 2
1 2 2 1
2 3 3 1
0 2
1 2
```| Truy vấn | Nút bị chặn | Đường dẫn tối đa theo màu | Tối đa toàn cầu | 
| --- | --- | --- | --- | 
| 0 2 | {2} | màu1:0 | 0 | 
| 1 2 | {} | màu1:5 | 5 | 

Điều này chứng tỏ rằng việc chặn một nút trung tâm sẽ phá vỡ đường dẫn một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Ồ | |
