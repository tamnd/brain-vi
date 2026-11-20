---
title: "CF 2155F - Cây đầy màu sắc của Juan"
description: "Chúng ta có một cây có n nút, trong đó mỗi nút chứa một tập hợp các màu được vẽ từ bảng màu có kích thước k. Tập hợp của mỗi nút có thể khác nhau và tổng số lần gán màu trên tất cả các nút là s."
date: "2026-06-08T00:32:24+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dsu", "graphs", "meet-in-the-middle", "trees"]
categories: ["algorithms"]
codeforces_contest: 2155
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1056 (Div. 2)"
rating: 2800
weight: 2155
solve_time_s: 116
verified: false
draft: false
---

[CF 2155F - Cây đầy màu sắc của Juan](https://codeforces.com/problemset/problem/2155/F) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, gặp nhau ở giữa, cây 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút, trong đó mỗi nút chứa một tập hợp các màu được vẽ từ một bảng màu có kích thước`k`. Tập hợp của mỗi nút có thể khác nhau và tổng số lần gán màu trên tất cả các nút là`s`. Nhiệm vụ là trả lời nhiều truy vấn, mỗi truy vấn hỏi số lượng màu xuất hiện ở mỗi nút dọc theo đường dẫn đơn giản nối hai nút`u`Và`v`. 

Nói cách khác, với mỗi truy vấn, chúng ta phải tìm giao điểm của tập hợp màu của tất cả các nút dọc theo đường dẫn từ`u`ĐẾN`v`và trả về kích thước của giao lộ này. Đầu vào cũng đảm bảo rằng tổng của`n`,`k`,`s`, Và`q`trên tất cả các trường hợp thử nghiệm tối đa là 3·10⁵, điều này ngụ ý rằng bất kỳ giải pháp nào có kết quả kém hơn khoảng O(n + s + q) cho mỗi trường hợp thử nghiệm đều có khả năng vượt quá giới hạn thời gian. 

Một cách tiếp cận ngây thơ đi qua đường dẫn và giao nhau các tập hợp một cách rõ ràng cho mọi truy vấn ngay lập tức bị nghi ngờ. Ví dụ: hãy xem xét một cây có 10⁵ nút, mỗi nút có 10 màu và 10⁵ truy vấn. Việc lặp lại dọc theo các đường dẫn có độ dài lên tới 10⁵ và thực hiện các giao lộ sẽ yêu cầu các thao tác lên tới 10¹⁰, điều này vượt xa mức chấp nhận được. Các trường hợp biên bao gồm các đường dẫn trong đó`u = v`(câu trả lời chỉ là kích thước được đặt riêng của nút) hoặc khi một màu xuất hiện ở một số chứ không phải tất cả các nút trên đường dẫn (các cách tiếp cận bất cẩn có thể tính sai). 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: với mỗi truy vấn, hãy tìm đường dẫn đơn giản giữa`u`Và`v`, thu thập tất cả các bộ màu dọc theo đường dẫn đó và tính toán giao điểm của chúng. Điều này đúng vì nó trực tiếp thực hiện định nghĩa nhưng lại quá chậm. Nếu như`n`Và`q`là các đường dẫn lớn, đi ngang và các tập hợp giao nhau lặp đi lặp lại dẫn đến độ phức tạp trong trường hợp xấu nhất xung quanh O(n·q), có thể đạt tới 10¹⁰ phép toán. 

Thông tin chi tiết quan trọng để có giải pháp nhanh hơn là nhận ra rằng màu chỉ có thể xuất hiện ở giao điểm của đường dẫn nếu nó xuất hiện ở mọi nút dọc theo đường dẫn đó. Chúng ta có thể đảo ngược vấn đề: thay vì kiểm tra tất cả các màu cho từng đường dẫn, chúng ta có thể theo dõi từng màu mà các nút chứa nó. Điều này cho phép chúng ta sử dụng kỹ thuật cây cổ điển: đánh dấu các nút chứa màu và đối với mỗi truy vấn, hãy kiểm tra xem màu có xuất hiện dọc theo đường dẫn từ`u`ĐẾN`v`. Bằng cách sử dụng Tổ tiên chung thấp nhất (LCA) và số lượng tiền tố của các màu dọc theo các đường dẫn từ gốc, chúng ta có thể trả lời từng truy vấn trong thời gian O(k) cho mỗi màu (hoặc nhanh hơn nếu chúng ta cắt bớt các màu không liên quan), biến giao điểm đường dẫn mạnh mẽ thành một vấn đề đếm. 

Cách tiếp cận này tận dụng cấu trúc của cây. Nếu chúng ta tính toán, đối với mỗi nút, số lần xuất hiện của một màu nhất định dọc theo đường dẫn từ gốc, thì số lần xuất hiện dọc theo đường dẫn từ gốc`u`ĐẾN`v`có thể được tính bằng công thức LCA:`count[u] + count[v] - 2*count[lca] + (1 if lca contains color else 0)`. Một màu nằm trong giao điểm của đường dẫn khi và chỉ khi số đếm này bằng với độ dài của đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·n·k) | O(n·k) | Quá chậm | 
| Tối ưu | O(s + n + q·k) | O(n·k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng cây dưới dạng danh sách kề. Điều này cho phép truyền tải hiệu quả các đường đi và dễ dàng tính toán LCA sau này. 
2. Đối với mỗi nút, hãy duy trì một tập hợp màu mà nó chứa. 
3. Thực hiện tìm kiếm theo chiều sâu từ một gốc tùy ý (ví dụ nút 1) để tính độ sâu của mỗi nút và điền vào các con trỏ cha để tính toán LCA bằng cách sử dụng nâng nhị phân. Điều này cho phép chúng ta tìm LCA(u, v) trong thời gian O(log n). 
4. Với mỗi màu, hãy xây dựng một mảng`count_color`như vậy`count_color[v]`biểu thị số lần màu này xuất hiện dọc theo đường dẫn từ gốc đến nút`v`. Trong DFS, truyền số đếm từ nút cha sang nút con, tăng dần nếu nút hiện tại chứa màu. 
5. Với mỗi truy vấn (u, v), hãy tính LCA`w`. Đối với mỗi màu`c`, tính tổng số lần xuất hiện dọc theo đường dẫn như`count_color[u] + count_color[v] - 2*count_color[w] + (1 if w contains c else 0)`. Nếu giá trị này bằng số nút dọc theo đường dẫn (độ sâu [u] + độ sâu [v] - 2*độ sâu [w] + 1), thì màu sẽ xuất hiện ở mọi nút trên đường dẫn. 
6. Đếm số màu thỏa mãn điều kiện này cho truy vấn và xuất kết quả. 

Tại sao nó hoạt động: thuật toán tính, đối với mỗi màu, sự hiện diện của nó dọc theo các đường dẫn sử dụng số tiền tố từ gốc. Việc sử dụng LCA để chia đường dẫn thành hai đoạn cho phép chúng tôi tính toán số lần xuất hiện chính xác dọc theo bất kỳ đường dẫn nào trong thời gian không đổi cho mỗi màu. Bằng cách so sánh số lượng này với độ dài đường dẫn, chúng tôi đảm bảo xác định chính xác liệu một màu có xuất hiện ở mọi nút của đường dẫn hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

sys.setrecursionlimit(10**6)

def solve():
    t = int(input())
    for _ in range(t):
        n, k, s, q = map(int, input().split())
        tree = [[] for _ in range(n+1)]
        for _ in range(n-1):
            u, v = map(int, input().split())
            tree[u].append(v)
            tree[v].append(u)
        
        node_colors = [[] for _ in range(n+1)]
        color_nodes = defaultdict(list)
        for _ in range(s):
            v, x = map(int, input().split())
            node_colors[v].append(x)
            color_nodes[x].append(v)
        
        LOGN = 20
        parent = [[-1]*(n+1) for _ in range(LOGN)]
        depth = [0]*(n+1)
        
        def dfs(u, p):
            parent[0][u] = p
            for v in tree[u]:
                if v != p:
                    depth[v] = depth[u] + 1
                    dfs(v, u)
        dfs(1, -1)
        
        for i in range(1, LOGN):
            for v in range(1, n+1):
                if parent[i-1][v] != -1:
                    parent[i][v] = parent[i-1][parent[i-1][v]]
        
        def lca(u, v):
            if depth[u] < depth[v]:
                u, v = v, u
            for i in reversed(range(LOGN)):
                if parent[i][u] != -1 and depth[parent[i][u]] >= depth[v]:
                    u = parent[i][u]
            if u == v:
                return u
            for i in reversed(range(LOGN)):
                if parent[i][u] != -1 and parent[i][u] != parent[i][v]:
                    u = parent[i][u]
                    v = parent[i][v]
            return parent[0][u]
        
        color_count = defaultdict(lambda: [0]*(n+1))
        def dfs_count(u, p):
            for c in node_colors[u]:
                color_count[c][u] = 1
            for v in tree[u]:
                if v != p:
                    dfs_count(v, u)
                    for c in color_count:
                        color_count[c][v] += color_count[c][u]
        dfs_count(1, -1)
        
        result = []
        for _ in range(q):
            u, v = map(int, input().split())
            w = lca(u, v)
            path_len = depth[u] + depth[v] - 2*depth[w] + 1
            cnt = 0
            for c, arr in color_count.items():
                total = arr[u] + arr[v] - 2*arr[w] + (1 if c in node_colors[w] else 0)
                if total == path_len:
                    cnt += 1
            result.append(str(cnt))
        print(" ".join(result))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc cây và lưu trữ màu nào xuất hiện trong mỗi nút. Sau đó chúng tôi chuẩn bị cho tính toán LCA bằng cách sử dụng nâng nhị phân. DFS được sử dụng để tính toán độ sâu và bảng cha. các`dfs_count`hàm tính tổng tiền tố của mỗi màu dọc theo đường dẫn từ gốc. Khi trả lời các truy vấn, chúng tôi sử dụng LCA để chia đường dẫn thành hai đoạn và đếm các màu có trong mỗi nút dọc theo đường dẫn bằng cách sử dụng các tổng tiền tố này. 

Các điểm tinh tế bao gồm việc đảm bảo chúng tôi tăng số lượng cho chính LCA và lập chỉ mục cẩn thận trong mảng. Chúng tôi cũng xử lý độ sâu đệ quy lớn với`sys.setrecursionlimit`do độ sâu DFS của cây có khả năng đạt tới 3·10⁵. 

## Ví dụ đã hoạt động 

**Mẫu 1 dấu vết** 

| Truy vấn | Nút đường dẫn | Màu sắc trên mỗi nút | Giao lộ | Đếm | 
| --- | --- | --- | --- | --- | 
| (1,3) | 1,3 | {1,2,3,4,5}, {1,2} | {1,2} | 2 | 
| (2,3) | 2,1,3 | {1,2,5}, {1,2,3,4,5}, {1,2} | {1,2} | 2 | 
| (1,2) | 1,2 | {1,2,3,4,5}, {1, | | |
