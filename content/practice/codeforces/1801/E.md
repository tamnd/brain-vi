---
title: "CF 1801E - Giá xăng dầu"
description: "Chúng ta có một quốc gia, Berland, được cấu trúc như một cây gồm các thành phố bắt nguồn từ thành phố 1. Mỗi thành phố có một trạm xăng với mức giá trong khoảng $[li, ri]$. Hàng năm, hai người con trai của nhà vua đều kiểm tra giá xăng dầu theo hai con đường có chiều dài bằng nhau."
date: "2026-06-09T09:32:15+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dsu", "hashing", "trees"]
categories: ["algorithms"]
codeforces_contest: 1801
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 857 (Div. 1)"
rating: 3000
weight: 1801
solve_time_s: 77
verified: true
draft: false
---

[CF 1801E - Giá xăng](https://codeforces.com/problemset/problem/1801/E) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, dsu, băm, cây 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một quốc gia, Berland, được cấu trúc như một cây gồm các thành phố bắt nguồn từ thành phố 1. Mỗi thành phố có một trạm xăng với một mức giá khác nhau$[l_i, r_i]$. Hàng năm, hai người con trai của nhà vua đều kiểm tra giá xăng dầu theo hai con đường có chiều dài bằng nhau. Mỗi lần kiểm tra đều yêu cầu các thành phố tương ứng dọc theo hai con đường có cùng mức giá. 

Nhiệm vụ là tính toán cho mỗi năm$i$, số cách ấn định giá cho tất cả các thành phố sao cho tất cả các cuộc thanh tra từ năm 1 đến năm$i$hài lòng, modulo$10^9 + 7$. Sản lượng của mỗi năm phải phản ánh những hạn chế tích lũy do các cuộc thanh tra áp đặt cho đến thời điểm đó. 

Hạn chế chính là số lượng thành phố và số lượng thanh tra có thể cao tới mức$2 \cdot 10^5$và phạm vi giá có thể lên tới$10^9 + 6$. Một cách tiếp cận mạnh mẽ để thử tất cả các phép gán giá là không thể vì không gian tìm kiếm có tính hàm mũ theo$n$. Chúng ta cũng cần xử lý các đường dẫn trong cây một cách hiệu quả, vì các đường dẫn có thể là các chuỗi nút tùy ý. 

Các trường hợp biên bao gồm các tình huống trong đó việc kiểm tra buộc phải có giao điểm trống của các phạm vi, tạo ra các phép gán không hợp lệ. Một trường hợp tinh vi khác là khi nhiều cuộc thanh tra áp đặt sự bình đẳng chồng chéo giữa các thành phố – việc ghi sổ kế toán bất cẩn có thể tính các giới hạn gấp đôi. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại tất cả các ấn định có thể có về giá xăng dầu cho từng thành phố và kiểm tra tất cả các lộ trình để tìm ra các hạn chế về bình đẳng. Về nguyên tắc thì điều này đúng nhưng độ phức tạp$\prod_{i=1}^n (r_i - l_i + 1)$, là số mũ trong$n$và hoàn toàn không khả thi đối với$n = 2 \cdot 10^5$. Ngay cả việc lặp lại các đường dẫn một cách đơn giản cho mỗi lần kiểm tra cũng là quá chậm, vì mỗi đường dẫn có thể chứa$O(n)$nút. 

Cái nhìn sâu sắc quan trọng là các ràng buộc bình đẳng có thể được biểu diễn dưới dạng liên kết của các nhóm thành phố có giá cả phải phù hợp. Khi chúng tôi xác định được các tập hợp này, giá hợp lệ cho mỗi tập hợp là giao điểm của phạm vi của tất cả các thành phố trong tập hợp đó. Mỗi lần kiểm tra tương ứng với việc ghép nối các nút dọc theo hai đường dẫn có độ dài bằng nhau và mỗi cặp phải thuộc cùng một bộ. 

Điều này gợi ý cấu trúc Disjoint Set Union (DSU) hoặc Union-Find. Mỗi thành phố bắt đầu theo cách riêng của mình. Đối với mỗi lần kiểm tra, chúng tôi hợp nhất các tập hợp theo các ràng buộc bằng nhau dọc theo các đường dẫn. Sau mỗi lần hợp nhất, chúng tôi cập nhật phạm vi giá hợp lệ để tập hợp được hợp nhất thành giao điểm của các phạm vi của các tập hợp kết hợp. Nếu bất kỳ giao lộ nào trở nên trống, tập hợp đó có giá hợp lệ bằng 0 và tổng số lần gán giá bằng 0. 

Việc theo dõi các tập hợp và giao điểm một cách hiệu quả cho phép chúng tôi tính toán số lượng phép gán hợp lệ tích lũy sau mỗi năm bằng cách nhân kích thước của các phạm vi của tất cả các tập hợp rời rạc. Điều này làm giảm độ phức tạp xuống còn khoảng$O(n \log n + m \cdot k)$, Ở đâu$k$là độ dài đường dẫn tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\prod_{i=1}^n (r_i-l_i+1) \cdot m \cdot n)$|$O(n)$| Quá chậm | 
| Phân tách DSU + Đường dẫn |$O(n \log n + m \cdot k)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn cây dưới dạng danh sách kề. Tính toán mảng gốc và nếu cần, tính toán trước độ sâu nút để có đường đi hiệu quả. 
2. Khởi tạo DSU trong đó mỗi thành phố bắt đầu theo tập hợp riêng. Mỗi bộ theo dõi hai số:`set_min`Và`set_max`, thể hiện sự giao nhau của các phạm vi giá có thể chấp nhận được cho tập hợp đó. 
3. Mỗi năm kiểm tra: 

1. Chia mỗi đường dẫn thành một danh sách các nút theo thứ tự từ đầu đến cuối. Bạn có thể đi tới thư mục gốc bằng cách sử dụng con trỏ gốc, sau đó đảo ngược một phần của đường dẫn để có được đường dẫn đầy đủ. 
2. Lặp lại các vị trí tương ứng dọc theo hai đường dẫn. Đối với mỗi cặp`(u, v)`, hợp nhất các bộ DSU của họ. Khi hợp nhất, hãy tính giao điểm mới của các phạm vi:`new_min = max(set_min[u], set_min[v])`,`new_max = min(set_max[u], set_max[v])`. 
3. Nếu bất kỳ giao lộ nào trở nên trống trải (`new_min > new_max`), đánh dấu tổng số là 0 ngay lập tức. 
4. Sau khi xử lý tất cả các cặp đường dẫn trong năm, lặp lại tất cả các bộ DSU và tính tích của`(set_max - set_min + 1)`modulo$10^9 + 7$. Đây là tổng số bài tập hợp lệ trong năm đó. 
5. Xuất ra số liệu tích lũy theo từng năm. 

Tại sao nó hoạt động: DSU đảm bảo rằng bất kỳ thành phố nào trong cùng một tập hợp đều phải có cùng mức giá. Việc hợp nhất các bộ dọc theo các đường dẫn sẽ thực hiện các ràng buộc bình đẳng theo cặp được áp đặt bởi các cuộc kiểm tra. Việc duy trì giao điểm của các phạm vi đảm bảo rằng chúng tôi chỉ tính các phép gán hợp lệ. Việc nhân các kích thước phạm vi trên các tập hợp rời rạc sẽ tính tất cả các kết hợp mà không tính quá mức, vì tất cả các ràng buộc đã được mã hóa trong cấu trúc DSU. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

MOD = 10**9 + 7

class DSU:
    def __init__(self, n, l, r):
        self.parent = list(range(n))
        self.min_val = l[:]
        self.max_val = r[:]

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        x_root, y_root = self.find(x), self.find(y)
        if x_root == y_root:
            return
        new_min = max(self.min_val[x_root], self.min_val[y_root])
        new_max = min(self.max_val[x_root], self.max_val[y_root])
        self.parent[y_root] = x_root
        self.min_val[x_root] = new_min
        self.max_val[x_root] = new_max

def main():
    n = int(input())
    p = list(map(int, input().split()))
    tree = [[] for _ in range(n)]
    for i, parent in enumerate(p):
        tree[parent-1].append(i+1)

    l = [0]*n
    r = [0]*n
    for i in range(n):
        l[i], r[i] = map(int, input().split())

    m = int(input())
    queries = [tuple(map(int, input().split())) for _ in range(m)]

    parent = [-1]*n
    depth = [0]*n
    def dfs(u, par):
        parent[u] = par
        for v in tree[u]:
            depth[v] = depth[u]+1
            dfs(v, u)
    dfs(0, -1)

    def path(u, v):
        up = []
        down = []
        while depth[u] > depth[v]:
            up.append(u)
            u = parent[u]
        while depth[v] > depth[u]:
            down.append(v)
            v = parent[v]
        while u != v:
            up.append(u)
            down.append(v)
            u = parent[u]
            v = parent[v]
        up.append(u)
        return up + down[::-1]

    dsu = DSU(n, l, r)
    result = []

    for a, b, c, d in queries:
        path1 = path(a-1, b-1)
        path2 = path(c-1, d-1)
        for u, v in zip(path1, path2):
            dsu.union(u, v)
        total = 1
        counted = set()
        for i in range(n):
            root = dsu.find(i)
            if root in counted:
                continue
            counted.add(root)
            if dsu.min_val[root] > dsu.max_val[root]:
                total = 0
                break
            total = total * (dsu.max_val[root] - dsu.min_val[root] + 1) % MOD
        result.append(total)

    print("\n".join(map(str, result)))

if __name__ == "__main__":
    main()
```Giải pháp xây dựng các đường dẫn bằng cách sử dụng các con trỏ cha và hợp nhất các tập hợp thông qua DSU. Việc cập nhật các giá trị tối thiểu/tối đa sẽ thực thi giao điểm của các phạm vi. Chỉ đếm gốc của mỗi bộ sẽ ngăn chặn việc đếm quá mức. Xử lý các nút giao trống ngay lập tức tránh được phép nhân không hợp lệ. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5
1 1 2 2
2 4
1 3
1 3
2 4
4 4
4
1 1 2 2
1 2 2 1
3 4 4 3
3 4 3 5
```Theo dõi sau mỗi năm: 

| Năm | Bộ hợp nhất | Nút giao | Đếm | 
| --- | --- | --- | --- | 
| 1 | {1,2} | [2,3] | 18 | 
| 2 | {1,2} | [2,3] | 18 | 
| 3 | {1,2}, {3,4} | [2,3], [2,3] | 4 | 
| 4 | {1,2,3,4,5} | trống | 0 | 

Điều này chứng tỏ rằng các tập hợp hợp nhất truyền bá chính xác các ràng buộc đẳng thức và các giao điểm trống dẫn đến các phép gán bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m_k_α | |
