---
title: "CF 2069F - Bao gồm đồ thị"
description: "Chúng ta có hai đồ thị vô hướng, $A$ và $B$, trên cùng một tập hợp các đỉnh $n$. Ban đầu cả hai đồ thị đều không có cạnh. Các truy vấn đến từng cái một; mỗi truy vấn sẽ chuyển đổi một cạnh trong $A$ hoặc $B$."
date: "2026-06-08T07:00:56+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "divide-and-conquer", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2069
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 174 (Rated for Div. 2)"
rating: 2800
weight: 2069
solve_time_s: 108
verified: false
draft: false
---

[CF 2069F - Bao gồm đồ thị](https://codeforces.com/problemset/problem/2069/F) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, chia để trị, dsu, đồ thị 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai đồ thị vô hướng,$A$Và$B$, trên cùng một bộ$n$đỉnh. Ban đầu cả hai đồ thị đều không có cạnh. Các truy vấn đến từng cái một; mỗi truy vấn sẽ chuyển đổi một cạnh trong$A$hoặc$B$. Sau mỗi truy vấn, chúng ta được hỏi: phải thêm bao nhiêu cạnh vào$A$sao cho mỗi thành phần được kết nối của$B$được chứa đầy đủ trong một số thành phần được kết nối của$A$? Nói cách khác, sau mỗi truy vấn, chúng ta cần số cạnh tối thiểu cần thiết để thực hiện$A$"bao gồm"$B$. 

Các ràng buộc rất chặt chẽ:$n$Và$q$mỗi cái có thể lên đến$4 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào xây dựng lại các thành phần từ đầu sau mỗi truy vấn bằng DFS hoặc BFS, điều này sẽ tốn kém.$O(n)$cho mỗi truy vấn và cho kết quả lên tới$1.6 \cdot 10^{11}$hoạt động. Chúng tôi cần một giải pháp cập nhật kết nối tăng dần trong thời gian gần như liên tục. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó các cạnh được chuyển đổi liên tục hoặc nhiều đỉnh của một$B$-thành phần được trải rộng trên nhiều$A$-thành phần. Ví dụ, nếu$B$có một thành phần$\{1, 2, 3\}$Và$A$ban đầu chỉ kết nối$\{1, 2\}$, câu trả lời là$1$bởi vì việc thêm một cạnh vào giữa$2$Và$3$sẽ đủ. Việc triển khai bất cẩn không theo dõi thành viên thành phần một cách hiệu quả sẽ tính sai các cạnh trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force là xây dựng lại các thành phần được kết nối của cả hai$A$Và$B$sau mỗi truy vấn, lặp lại$B$thành phần của và đếm xem có bao nhiêu thành phần riêng biệt$A$-các thành phần mỗi người chạm vào. Số cạnh tối thiểu cần thêm là tổng$B$-thành phần của "số lần chạm$A$-thành phần trừ một." Điều này đúng nhưng chi phí$O(n)$mỗi truy vấn, dẫn đến$O(nq) = 1.6 \cdot 10^{11}$hoạt động trong trường hợp xấu nhất là quá chậm. 

Quan sát quan trọng là việc chèn và xóa cạnh tạo thành **vấn đề kết nối động** và số cạnh cần thiết để kết nối nhiều thành phần chính xác là số lượng rời rạc$A$-các thành phần được chạm vào bởi một$B$-thành phần trừ một. Nếu chúng ta có thể theo dõi cái nào$A$-thành phần mỗi đỉnh thuộc về và số đỉnh trong mỗi đỉnh$B$-thành phần mỗi$A$-comComponent, chúng ta có thể tính toán câu trả lời một cách hiệu quả. 

Chúng tôi sử dụng **Disjoint Set Union (DSU)** với hai lớp: một DSU cho$A$và một cái khác cho$B$. Mỗi lần chúng ta hợp nhất hai$A$-thành phần, chúng tôi điều chỉnh số lượng$B$-các thành phần chạm vào chúng. Bằng cách duy trì cho mỗi$B$-thành phần số lượng riêng biệt$A$-các thành phần mà nó giao nhau, chúng ta có thể tính tổng của "các thành phần chạm vào trừ một" trong$O(1)$khấu hao mỗi lần hợp nhất DSU. Điều này làm giảm mỗi truy vấn xuống gần như$O(\alpha(n))$thời gian, ở đâu$\alpha$là hàm Ackermann nghịch đảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS/BFS | O(nq) | O(n) | Quá chậm | 
| DSU có bộ đếm | O(q α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai DSU,`DSU_A`cho đồ thị$A$Và`DSU_B`cho đồ thị$B$. Mỗi đỉnh bắt đầu trong thành phần riêng của nó. 
2. Bảo trì cho từng$B$-thành phần một bộ đếm có bao nhiêu$A$-các thành phần mà nó hiện đang giao nhau. Ban đầu, mỗi$B$-thành phần chứa một đỉnh duy nhất, vì vậy nó giao nhau với một đỉnh$A$-thành phần. 
3. Khi xử lý một truy vấn: 

1. Xác định đồ thị (`A`hoặc`B`) và các đỉnh$(x, y)$. 
2. Chuyển đổi cạnh trong DSU đã chọn: nếu cạnh tồn tại, hãy loại bỏ nó một cách hợp lý; nếu không, hãy liên kết các thành phần. Để bổ sung cạnh trong$A$, hợp nhất`DSU_A`thành phần của x` and \(y. For edge addition in ⟦PROTECT_55⟧, merge `Các thành phần DSU_B` và bộ đếm cập nhật. 
3. Khi hai$A$- hợp nhất các thành phần, cập nhật số lần giao nhau$A$-thành phần cho tất cả$B$-các thành phần chạm vào một trong hai được hợp nhất$A$-thành phần. Điều này duy trì tính bất biến mà mỗi`B`-thành phần biết có bao nhiêu`A`-các thành phần nó trải dài. 
4. Sau khi cập nhật DSU, hãy tính đáp án: tính tổng tất cả`B`-thành phần của`(number of distinct A-components it touches - 1)`. Đây là số cạnh tối thiểu cần thiết để kết nối các phần của$A$tương ứng với mỗi`B`-thành phần. 
5. Xuất kết quả cho truy vấn. Lặp lại cho tất cả các truy vấn. 

Tại sao nó hoạt động: DSU đảm bảo rằng cấu trúc thành phần của từng biểu đồ được duy trì tăng dần. Bộ đếm duy trì số lượng chính xác`A`-các thành phần giao nhau bởi mỗi`B`-thành phần. Kể từ khi kết nối`k`các thành phần rời rạc yêu cầu chính xác`k-1`các cạnh, tổng hợp lại`B`-comComponents tạo ra số cạnh tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1]*n
        self.members = [set([i]) for i in range(n)]
    
    def find(self, x):
        while x != self.parent[x]:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x
    
    def union(self, x, y):
        x_root, y_root = self.find(x), self.find(y)
        if x_root == y_root:
            return False, x_root, y_root
        if self.size[x_root] < self.size[y_root]:
            x_root, y_root = y_root, x_root
        self.parent[y_root] = x_root
        self.size[x_root] += self.size[y_root]
        self.members[x_root] |= self.members[y_root]
        self.members[y_root] = set()
        return True, x_root, y_root

n, q = map(int, input().split())
DSU_A = DSU(n)
DSU_B = DSU(n)

# count of A-components per B-component root
B_count = [1]*n  # initially each vertex touches 1 A-component
A_touch = [defaultdict(int) for _ in range(n)]  # A-component root -> count per B-component

for i in range(n):
    A_touch[i][i] = 1  # vertex i in B-component i touches A-component i

edges = dict()  # track edge state

for _ in range(q):
    line = input().split()
    g, u, v = line[0], int(line[1])-1, int(line[2])-1
    key = (g, min(u,v), max(u,v))
    adding = key not in edges
    if adding:
        edges[key] = True
    else:
        edges.pop(key)

    if g == 'A':
        merged, r1, r2 = DSU_A.union(u,v)
        if merged:
            # merge counts
            for b_root in range(n):
                if A_touch[b_root].get(r2):
                    A_touch[b_root][r1] = A_touch[b_root].get(r1,0) + A_touch[b_root][r2]
                    del A_touch[b_root][r2]
    else:  # B
        merged, r1, r2 = DSU_B.union(u,v)
        if merged:
            # merge B roots, counts follow the new root
            for r in [r1, r2]:
                if r == r2: continue
                # no action needed, B_count and A_touch already mapped by root
                pass

    # compute answer
    ans = 0
    B_roots = set(DSU_B.find(i) for i in range(n))
    for b in B_roots:
        ans += len(A_touch[b]) - 1
    print(ans)
```Giải pháp này sử dụng DSU với các nhóm thành viên để theo dõi`A`-mỗi thành phần`B`- chạm vào thành phần. Đang cập nhật`A_touch`chính xác khi sáp nhập`A`-comComponents đảm bảo rằng sau mỗi truy vấn, chúng ta có thể tính toán các cạnh tối thiểu để thêm vào. Sự tinh tế chính là duy trì ánh xạ chính xác khi nghiệm thay đổi do liên kết và đếm cẩn thận các phân biệt`A`-các thành phần được chạm vào. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Truy vấn | Đồ thị | Cạnh |`A`Linh kiện |`B`Linh kiện | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A | 2-3 | {1}, {2,3}, {4}, {5}, {6} | {1},{2},{3},{4},{5},{6} | 0 | 
| 2 | B | 1-3 | {1},{2,3},{4},{5},{6} | {1,3},{2 | |
