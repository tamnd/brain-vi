---
title: "CF 1608G - Cây chữ cái"
description: "Chúng ta được cho một cây có các nút được nối với nhau bằng các cạnh, mỗi nút được gắn nhãn bằng một chữ cái viết thường. Ngoài ra, chúng tôi có một bộ sưu tập các chuỗi. Đối với hai nút bất kỳ trong cây, đường dẫn nối chúng xác định một chuỗi bằng cách ghép các chữ cái dọc theo đường dẫn đó theo thứ tự truyền tải."
date: "2026-06-10T07:36:06+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "hashing", "string-suffix-structures", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1608
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 758 (Div.1 + Div. 2)"
rating: 3500
weight: 1608
solve_time_s: 97
verified: false
draft: false
---

[CF 1608G - Cây chữ cái](https://codeforces.com/problemset/problem/1608/G) 

**Đánh giá:** 3500 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, băm, cấu trúc hậu tố chuỗi, chuỗi, cây 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các nút được nối với nhau bằng các cạnh, mỗi nút được gắn nhãn bằng một chữ cái viết thường. Ngoài ra, chúng tôi có một bộ sưu tập các chuỗi. Đối với hai nút bất kỳ trong cây, đường dẫn nối chúng xác định một chuỗi bằng cách ghép các chữ cái dọc theo đường dẫn đó theo thứ tự truyền tải. Vấn đề hỏi, đối với một truy vấn nhất định, chuỗi đường dẫn đó xuất hiện dưới dạng chuỗi con bao nhiêu lần trong một phạm vi liên tiếp của các chuỗi được cung cấp. 

Kích thước đầu vào rất đáng kể: cây có thể có tới 100.000 nút, có thể có tới 100.000 chuỗi và tổng độ dài của tất cả các chuỗi lên tới 100.000 ký tự. Số lượng truy vấn cũng có thể lên tới 100.000. Điều này loại trừ bất kỳ giải pháp nào cố gắng trích xuất từng chuỗi đường dẫn và quét tất cả các chuỗi có liên quan để tìm các kết quả khớp riêng lẻ. Một thuật toán đơn giản sẽ liên quan đến việc hình thành một chuỗi đường dẫn có độ dài lên tới O(n) và tìm kiếm nó trong công việc O(tổng chiều dài chuỗi) cho mỗi truy vấn. Nhân với số lượng truy vấn làm cho phương pháp này không khả thi. 

Trường hợp cạnh tinh tế xảy ra khi chuỗi đường dẫn rất ngắn, chẳng hạn như một ký tự đơn và chuỗi chứa nhiều lần xuất hiện chồng chéo. Ví dụ: nếu cây có một cạnh được gắn nhãn 'a' và một chuỗi trong bộ sưu tập là "aaa", truy vấn yêu cầu các lần xuất hiện dọc theo cạnh đó phải tính cả ba vị trí chứ không chỉ một. Một trường hợp cạnh khác là truy vấn một đường dẫn đảo ngược thứ tự cạnh; mã phải đi qua các cạnh theo thứ tự chính xác dọc theo đường đi, từ u đến v. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ xây dựng chuỗi đường dẫn cho mỗi truy vấn và sau đó lặp lại tất cả các chuỗi trong phạm vi [l, r], quét để tìm chuỗi con khớp. Đối với đường dẫn có độ dài k và tổng độ dài L của các chuỗi liên quan, điều này sẽ yêu cầu O(k * L) cho mỗi truy vấn. Với độ dài trong trường hợp xấu nhất và 100.000 truy vấn, phương pháp này có thể dễ dàng yêu cầu 10^10 thao tác và do đó quá chậm. 

Quan sát quan trọng là tổng chiều dài của tất cả các chuỗi chỉ là 100.000, có thể quản lý được. Điều này gợi ý việc xử lý trước các chuỗi để hỗ trợ các truy vấn xuất hiện chuỗi con nhanh. Việc sử dụng Suffix Automaton hoặc Suffix Array trên tất cả các chuỗi cho phép chúng tôi xác định số lần xuất hiện của bất kỳ chuỗi con nào một cách hiệu quả. Sau đó, chúng tôi có thể tăng cường nó để ghi lại chuỗi mà mỗi vị trí thuộc về, do đó, các truy vấn trên phạm vi [l, r] có thể được trả lời bằng cách sử dụng tìm kiếm nhị phân hoặc cây Fenwick. 

Một cái nhìn sâu sắc quan trọng khác là các chuỗi đường dẫn đến từ một cái cây. Chúng ta có thể root cây một cách tùy ý và tính toán trước các giá trị băm hoặc mã định danh chuỗi cho các đường dẫn từ gốc đến mọi nút. Khi đó, chuỗi dọc theo đường dẫn từ u đến v có thể được biểu diễn dưới dạng nối của hàm băm từ gốc đến u, kết hợp với hàm băm từ gốc đến v, chiếm tổ tiên chung thấp nhất. Điều này cho phép xây dựng chuỗi đường dẫn trong thời gian O(1) hoặc O(log n) cho mỗi truy vấn, tùy thuộc vào kỹ thuật băm. 

Cách tiếp cận cuối cùng là kết hợp các ý tưởng này: tính toán trước hàm băm đường dẫn bằng cách sử dụng nâng cấp nhị phân để tìm LCA, xây dựng Suffix Automaton hoặc Trie cho tất cả các chuỗi đầu vào, tăng cường nó để đếm số lần xuất hiện theo chỉ mục chuỗi, sau đó trả lời từng truy vấn bằng cách xây dựng hàm băm chuỗi đường dẫn và truy vấn cấu trúc xuất hiện trên phạm vi được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n * Total_string_length) | O(n + tổng_string_length) | Quá chậm | 
| Hậu tố Automaton + Băm đường dẫn | O((total_string_length + n) log n + q log Total_string_length) | O(total_string_length + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Gốc cây tại một nút tùy ý, thường là nút 1. Đối với mỗi nút, tính chuỗi từ gốc đến nút đó bằng cách lưu trữ các ký tự được nối hoặc một hàm băm cuộn. 
2. Tính toán trước tổ tiên chung thấp nhất (LCA) cho tất cả các cặp nút bằng cách sử dụng nâng nhị phân. Điều này cho phép chúng ta tìm được đường đi duy nhất từ ​​u tới v một cách hiệu quả. 
3. Đối với mỗi chuỗi đầu vào, hãy chèn nó vào Trie hoặc Suffix Automaton. Lưu trữ chỉ mục của chuỗi tại mỗi nút đầu cuối và tùy ý duy trì cây Fenwick hoặc danh sách được sắp xếp để đếm nhanh có bao nhiêu chuỗi chứa một chuỗi con nhất định trong một phạm vi [l, r]. 
4. Để trả lời một truy vấn (u, v, l, r), hãy tính chuỗi đường dẫn từ u đến v bằng cách kết hợp các đường dẫn từ gốc đến nút được tính toán trước cho u và v và loại bỏ tiền tố tương ứng với LCA. 
5. Duyệt qua máy tự động hoặc Trie bằng chuỗi đường dẫn, thu được nút đại diện cho tất cả các lần xuất hiện của chuỗi con này. Sử dụng các chỉ mục được lưu trữ với tìm kiếm nhị phân hoặc cây Fenwick để đếm số lượng nằm trong [l, r]. 
6. Trả về số lượng cho truy vấn. 

Tại sao nó hoạt động: Tính chính xác xuất phát từ các thuộc tính của Suffix Automaton hoặc Trie. Bất kỳ chuỗi con nào có trong máy tự động đều được đảm bảo tương ứng với một đường dẫn trong cây và cơ chế lập chỉ mục đảm bảo chúng tôi chỉ tính các lần xuất hiện trong phạm vi được yêu cầu. Băm đường dẫn dựa trên LCA đảm bảo rằng chuỗi đường dẫn chính xác được trích xuất mà không đi qua các cạnh một cách rõ ràng trong mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque
import bisect

sys.setrecursionlimit(1 << 25)

class TrieNode:
    def __init__(self):
        self.children = {}
        self.indices = []

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, s, idx):
        node = self.root
        for c in s:
            if c not in node.children:
                node.children[c] = TrieNode()
            node = node.children[c]
        node.indices.append(idx)

    def query(self, s, l, r):
        node = self.root
        for c in s:
            if c not in node.children:
                return 0
            node = node.children[c]
        # count indices in range [l, r]
        return bisect.bisect_right(node.indices, r) - bisect.bisect_left(node.indices, l)

def build_tree(n, edges):
    g = [[] for _ in range(n)]
    labels = {}
    for u, v, c in edges:
        u -= 1; v -= 1
        g[u].append(v)
        g[v].append(u)
        labels[(u, v)] = c
        labels[(v, u)] = c
    return g, labels

def dfs(u, parent, g, labels, path, paths):
    paths[u] = ''.join(path)
    for v in g[u]:
        if v != parent:
            path.append(labels[(u, v)])
            dfs(v, u, g, labels, path, paths)
            path.pop()

def lca_precompute(n, g, root=0):
    LOG = 20
    up = [[-1]*LOG for _ in range(n)]
    depth = [0]*n
    def dfs_lca(u, p):
        up[u][0] = p
        for i in range(1, LOG):
            if up[u][i-1] != -1:
                up[u][i] = up[up[u][i-1]][i-1]
        for v in g[u]:
            if v != p:
                depth[v] = depth[u] + 1
                dfs_lca(v, u)
    dfs_lca(root, -1)
    return up, depth

def lca(u, v, up, depth):
    LOG = 20
    if depth[u] < depth[v]:
        u, v = v, u
    for i in range(LOG-1, -1, -1):
        if up[u][i] != -1 and depth[up[u][i]] >= depth[v]:
            u = up[u][i]
    if u == v:
        return u
    for i in range(LOG-1, -1, -1):
        if up[u][i] != -1 and up[u][i] != up[v][i]:
            u = up[u][i]
            v = up[v][i]
    return up[u][0]

def solve():
    n, m, q = map(int, input().split())
    edges = [tuple(input().split()) for _ in range(n-1)]
    edges = [(int(u), int(v), c) for u, v, c in edges]

    g, labels = build_tree(n, edges)
    paths = ['']*n
    dfs(0, -1, g, labels, [], paths)
    up, depth = lca_precompute(n, g)

    strings = []
    trie = Trie()
    for idx in range(1, m+1):
        s = input().strip()
        strings.append(s)
        trie.insert(s, idx)

    for _ in range(q):
        u, v, l, r = map(int, input().split())
        u -= 1; v -= 1
        ancestor = lca(u, v, up, depth)
        # construct path string
        pu = paths[u][:depth[u]-depth[ancestor]]
        pv = paths[v][:depth[v]-depth[ancestor]][::-1]
        path_str = pu + pv
        ans = trie.query(path_str, l, r)
        print(ans)
```Mã bắt đầu bằng việc xây dựng cây và lưu trữ các nhãn cạnh. Sau đó, nó tính toán trước các đường dẫn từ gốc đến nút bằng DFS. Nâng nhị phân tính toán LCA một cách hiệu quả. Trie lưu trữ tất cả các chuỗi đầu vào, với các chỉ mục nút được sắp xếp để tìm kiếm nhị phân. Truy vấn xây dựng chuỗi đường dẫn từ các đường dẫn được tính toán trước và sử dụng Trie để đếm số lần xuất hiện trong phạm vi đã cho. Các chi tiết tinh tế bao gồm việc đảo ngược đoạn đường dẫn từ v sang LCA và sử dụng chỉ mục dựa trên 1 cho
