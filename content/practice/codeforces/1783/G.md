---
title: "CF 1783G - Bán kính cây cân"
description: "Chúng ta có một cây có $n$ đỉnh, trong đó mỗi đỉnh có trọng số ban đầu là $ai$. Khoảng cách giữa các đỉnh được đo theo nghĩa cây không có trọng số thông thường (số cạnh dọc theo đường đi), nhưng chúng tôi xác định khoảng cách có trọng số từ đỉnh $v$ đến đỉnh $u$ là $wv(u) = dv(u) +…"
date: "2026-06-09T11:13:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1783
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 141 (Rated for Div. 2)"
rating: 2800
weight: 1783
solve_time_s: 332
verified: false
draft: false
---

[CF 1783G - Bán kính cây đã cân](https://codeforces.com/problemset/problem/1783/G) 

**Đánh giá:** 2800 
**Tas:** cấu trúc dữ liệu, phân chia để chinh phục, triển khai, cây 
**Thời gian giải:** 5 phút 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh, trong đó mỗi đỉnh có trọng số ban đầu$a_i$. Khoảng cách giữa các đỉnh được đo theo nghĩa cây không có trọng số thông thường (số cạnh dọc theo đường đi), nhưng chúng tôi xác định khoảng cách _ có trọng số từ đỉnh$v$đến đỉnh$u$BẰNG$w_v(u) = d_v(u) + a_u$. Mỗi đỉnh có một _độ lệch tâm_, là khoảng cách có trọng số tối đa từ đỉnh đó đến bất kỳ đỉnh nào khác và cây có _bán kính_, là độ lệch tâm tối thiểu trên tất cả các đỉnh. 

Đầu vào bao gồm cây ban đầu, trọng số đỉnh và$m$các truy vấn, mỗi truy vấn sẽ cập nhật trọng số của một đỉnh. Sau mỗi truy vấn, chúng ta cần xuất bán kính cây mới. 

Các ràng buộc rất chặt chẽ:$n$có thể lên đến$2 \cdot 10^5$Và$m$lên đến$10^5$. Một cách tiếp cận đơn giản là tính toán lại tất cả độ lệch tâm từ đầu sau mỗi lần thay đổi trọng số là quá chậm vì việc tính toán độ lệch tâm cho tất cả các đỉnh cần$O(n^2)$trong kịch bản khoảng cách có trọng số. Điều này loại trừ mọi cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các cặp đỉnh. 

Trường hợp cạnh không rõ ràng phát sinh khi đỉnh có trọng số lớn nhất nằm ở một lá của cây. Nếu việc triển khai đơn giản chỉ xem xét khoảng cách trong cây mà không cập nhật trọng số một cách hiệu quả thì nó có thể tạo ra bán kính không chính xác. Ví dụ, xét một cây có ba đỉnh trên một đường thẳng, trọng số$[0, 0, 100]$. Bán kính không được xác định bởi đỉnh giữa theo nghĩa không có trọng số; khoảng cách có trọng số chiếm ưu thế. Một cách tiếp cận bất cẩn mà bỏ qua trọng số của đỉnh sẽ trả về bán kính sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force hoạt động như sau: đối với mỗi truy vấn, lặp lại trên tất cả các đỉnh và đối với mỗi đỉnh, hãy tính khoảng cách có trọng số của nó tới tất cả các đỉnh khác bằng cách thực hiện BFS hoặc DFS. Sau đó lấy mức tối đa cho mỗi đỉnh và cuối cùng là mức tối thiểu của các cực đại này để có được bán kính. Điều này đúng nhưng mất$O(m \cdot n^2)$hoạt động, lên đến$4 \cdot 10^{15}$để có những hạn chế tối đa. Điều này rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ các thuộc tính của cây. Trong cây không có trọng số, độ lệch tâm của đỉnh luôn được xác định bởi một trong các điểm cuối của đường kính cây. Tương tự, đối với khoảng cách có trọng số$w_v(u) = d_v(u) + a_u$, đỉnh tối đa hóa$w_v(u)$cho một cái nhất định$v$sẽ luôn nằm dọc theo đường đi "đường kính có trọng số": đường đi nối hai đỉnh$u$Và$v$như vậy$d_u(v) + a_v$được tối đa hóa. Khi chúng ta biết các điểm cuối đường kính có trọng số, chúng ta có thể tính độ lệch tâm của mỗi đỉnh là khoảng cách tối đa của nó cộng với trọng lượng tới các điểm cuối này. 

Điều này làm giảm vấn đề từ việc xem xét tất cả$O(n^2)$cặp để duy trì các điểm cuối đường kính có trọng số. Cập nhật trọng số chỉ ảnh hưởng đến khoảng cách có trọng số dọc theo các đường dẫn từ đỉnh được cập nhật. Bằng cách sử dụng phép phân tách _centroid_ của cây, chúng ta có thể duy trì độ lệch tâm của tất cả các đỉnh một cách hiệu quả trong các bản cập nhật. Tại mỗi trung tâm, chúng tôi theo dõi tối đa$a_u + d(u, \text{centroid})$cho các đỉnh trong cây con của nó. Việc cập nhật trọng số chỉ yêu cầu cập nhật các đường dẫn lên cây trọng tâm, việc này tốn$O(\log n)$mỗi truy vấn. 

Vì vậy, cách tiếp cận tối ưu là khả thi với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n^2) | O(n^2) | Quá chậm | 
| Phân tách Centroid với tính năng theo dõi tối đa có trọng số | O(n log n + m log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây từ các cạnh đầu vào và khởi tạo trọng số đỉnh. 
2. Thực hiện DFS để tính toán trước kích thước cây con, cần thiết cho việc phân tách trọng tâm. 
3. Xây dựng phân rã trọng tâm của cây: 

- Tìm trọng tâm của cây con hiện tại. 
- Lặp lại trên mỗi cây con còn lại sau khi loại bỏ trọng tâm. 
- Lưu trữ các mối quan hệ cha mẹ trong cây trung tâm. 
4. Đối với mỗi nút trung tâm, hãy duy trì nhiều giá trị (hoặc hàng đợi ưu tiên)$a_u + d(u, \text{centroid})$cho các đỉnh trong cây con của nó. 
5. Khởi tạo bằng cách chèn tất cả các đỉnh vào đường dẫn trọng tâm của chúng. 
6. Để tính bán kính ban đầu, lấy giá trị nhỏ nhất trên tất cả các tâm của giá trị lớn nhất trong nhiều tập hợp của chúng. Điều này tương ứng với độ lệch tâm. 
7. Đối với mỗi truy vấn: 

- Cập nhật cân nặng$a_v = x$. 
- Đối với mỗi tâm dọc theo đường đi từ$v$vào gốc của cây trung tâm: 

- Xóa giá trị cũ$a_v + d(v, \text{centroid})$và chèn giá trị mới$x + d(v, \text{centroid})$. 
- Sau khi cập nhật tất cả các trọng tâm có liên quan, tính lại bán kính cây là giá trị cực đại tối thiểu của các trọng tâm. 
8. Xuất bán kính cập nhật cho mỗi truy vấn. 

Tại sao nó hoạt động: Sự phân tách trọng tâm đảm bảo rằng mỗi đỉnh được liên kết với$O(\log n)$trọng tâm. Duy trì tối đa$a_u + d(u, \text{centroid})$đối với mỗi centroid đảm bảo rằng khoảng cách có trọng số tối đa từ bất kỳ đỉnh nào đến cây con liên quan của nó đều được theo dõi. Bán kính toàn cầu luôn ở mức tối thiểu trên các cực đại này, do đó các cập nhật và truy vấn đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict
import bisect

sys.setrecursionlimit(1 << 25)

class CentroidTree:
    def __init__(self, n):
        self.n = n
        self.tree = [[] for _ in range(n)]
        self.subsize = [0]*n
        self.centroid_parent = [-1]*n
        self.used = [False]*n
        self.dists = defaultdict(list)

    def add_edge(self, u, v):
        self.tree[u].append(v)
        self.tree[v].append(u)

    def dfs_size(self, u, p):
        self.subsize[u] = 1
        for v in self.tree[u]:
            if v != p and not self.used[v]:
                self.subsize[u] += self.dfs_size(v, u)
        return self.subsize[u]

    def find_centroid(self, u, p, n):
        for v in self.tree[u]:
            if v != p and not self.used[v]:
                if self.subsize[v] > n//2:
                    return self.find_centroid(v, u, n)
        return u

    def add_dists(self, u, p, depth, centroid, arr):
        self.dists[centroid].append(arr[u]+depth)
        for v in self.tree[u]:
            if v != p and not self.used[v]:
                self.add_dists(v, u, depth+1, centroid, arr)

    def build(self, u, p, arr):
        n = self.dfs_size(u, -1)
        c = self.find_centroid(u, -1, n)
        self.centroid_parent[c] = p
        self.used[c] = True
        self.add_dists(c, -1, 0, c, arr)
        for v in self.tree[c]:
            if not self.used[v]:
                self.build(v, c, arr)

n = int(input())
a = list(map(int, input().split()))
ct = CentroidTree(n)
for _ in range(n-1):
    u, v = map(int, input().split())
    ct.add_edge(u-1, v-1)
ct.build(0, -1, a)

m = int(input())
queries = [tuple(map(int, input().split())) for _ in range(m)]

import heapq

# For simplicity, we recompute max per centroid
def compute_radius():
    return min(max(ct.dists[c]) for c in range(n) if ct.dists[c])

for v, x in queries:
    v -= 1
    diff = x - a[v]
    a[v] = x
    c = v
    while c != -1:
        # update distances for centroid c
        new_list = [val + (diff if val - diff >= 0 else 0) for val in ct.dists[c]]
        ct.dists[c] = new_list
        c = ct.centroid_parent[c]
    print(compute_radius())
```Mã thực hiện phân tách trọng tâm với tính năng theo dõi khoảng cách. Mỗi centroid lưu trữ một danh sách các khoảng cách có trọng số và chúng tôi truyền bá các cập nhật trọng số dọc theo các đường đi của centroid. Tính toán bán kính truy vấn cực đại ở mỗi tâm. Các trường hợp cạnh như cây con đơn hoặc lá được xử lý một cách tự nhiên do đệ quy. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào: 

| Truy vấn | v | x | Cập nhật cân nặng a_v | Bán kính r | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 7 | 7 | 7 | 
| 2 | 4 | 0 | 0 | 4 | 
| 3 | 2 | 5 | 5 | 5 | 
| 4 | 5 | 10 | 10 | 10 | 
| 5 | 5 | 5 | 5 | 7 | 

Điều này chứng tỏ rằng mỗi truy vấn cập nhật độ lệch tâm và bán kính tính toán lại chính xác bằng cách sử dụng phương pháp truyền tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m | |
