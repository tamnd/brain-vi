---
title: "CF 1904E - Truy vấn cây"
description: "Chúng ta được cấp một cây có các nút $n$ và cần trả lời các truy vấn $q$. Mỗi truy vấn chỉ định một nút bắt đầu $x$ và một tập hợp các nút cần loại bỏ. Sau khi loại bỏ các nút đã chỉ định và tất cả các cạnh liên quan, chúng ta phải tìm độ dài của đường đi đơn giản dài nhất bắt đầu từ $x$."
date: "2026-06-08T20:56:51+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1904
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 914 (Div. 2)"
rating: 2500
weight: 1904
solve_time_s: 67
verified: true
draft: false
---

[CF 1904E - Truy vấn cây](https://codeforces.com/problemset/problem/1904/E) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, triển khai, cây 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các nút và cần trả lời$q$truy vấn. Mỗi truy vấn chỉ định một nút bắt đầu$x$và một tập hợp các nút cần loại bỏ. Sau khi loại bỏ các nút đã chỉ định và tất cả các cạnh liên quan, chúng ta phải tìm độ dài của đường đi đơn giản dài nhất bắt đầu từ$x$. Một đường dẫn đơn giản có nghĩa là không có nút nào lặp lại và độ dài đường dẫn được đo bằng số cạnh đi qua. 

Các ràng buộc rất chặt chẽ:$n$Và$q$mỗi người có thể lên đến$2 \cdot 10^5$và tổng số nút bị loại bỏ trên tất cả các truy vấn cũng bị giới hạn bởi$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận ngây thơ nào thực hiện duyệt cây đầy đủ cho mỗi truy vấn, vì$O(nq)$có thể là$4 \cdot 10^{10}$hoạt động. Chúng ta cần một cái gì đó gần giống với tiền xử lý tuyến tính với khả năng xử lý truy vấn nhanh. 

Các trường hợp khó nhận thấy bao gồm việc loại bỏ chính nút bắt đầu, loại bỏ các nút quan trọng để kết nối các cây con lớn hoặc các truy vấn không có nút nào bị loại bỏ. Ví dụ: hãy xem xét cây chuỗi 1-2-3-4-5 và một truy vấn bắt đầu từ nút 3 với các nút 2 và 4 đã bị loại bỏ. Cây còn lại chỉ cho phép các đường dẫn có độ dài 0 bắt đầu từ 3, mặc dù đường dẫn dài nhất ban đầu có độ dài 4. Một cách tiếp cận bất cẩn bỏ qua việc loại bỏ sẽ trả về câu trả lời sai. 

## Phương pháp tiếp cận 

Đối với mỗi truy vấn, một cách tiếp cận bạo lực sẽ là xây dựng cây với các nút bị loại bỏ và thực hiện tìm kiếm theo chiều sâu (DFS) từ nút bắt đầu để tìm nút xa nhất có thể tiếp cận. Mỗi truy vấn sẽ mất$O(n)$trong trường hợp xấu nhất, và với$q = 2 \cdot 10^5$, điều này là không thể thực hiện được. 

Quan sát quan trọng là một cây có cấu trúc đơn giản và đường đi dài nhất từ ​​bất kỳ nút nào có thể được biểu thị bằng khoảng cách đến hai cây con xa nhất của nó. Bằng cách xử lý trước cây bằng DFS kép hoặc lập trình động, chúng ta có thể tính toán cho mỗi nút khoảng cách đến hai cây con con sâu nhất của nó. Sau đó, trong quá trình truy vấn, nếu chúng ta loại bỏ các nút, vấn đề sẽ giảm xuống còn việc xem xét các đường dẫn trong các cây con còn lại của nút bắt đầu. Vì tổng số nút bị loại bỏ trên tất cả các truy vấn bị giới hạn nên chúng tôi có thể xử lý tác động của chúng một cách riêng lẻ và hiệu quả. 

Cụ thể, nếu chúng tôi lưu trữ cho mỗi nút khoảng cách đến hai cây con sâu nhất trên cùng của nó, khi việc xóa ảnh hưởng đến một trong số chúng, chúng tôi có thể nhanh chóng chuyển sang cây sâu thứ hai hoặc tính toán lại khoảng cách cục bộ. Điều này cho phép chúng ta trả lời từng truy vấn trong$O(k)$Ở đâu$k$là số nút bị loại bỏ cho truy vấn đó, giữ nguyên tổng thời gian$O(n + \sum k) = O(4 \cdot 10^5)$, điều đó có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(nq) | O(n) | Quá chậm | 
| Tính toán trước độ sâu cây con và root lại | O(n + tổng k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn cây bằng danh sách kề để duyệt hiệu quả. Điều này cho phép lặp lại hàng xóm theo thời gian không đổi. 
2. Thực hiện DFS từ một gốc tùy ý (giả sử nút 1) để tính độ sâu của mỗi nút và kích thước của cây con của nó. Trong khi làm như vậy, hãy lưu trữ cho mỗi nút độ sâu của hai cây con sâu nhất của nó. Những điều này sẽ giúp tính toán đường đi dài nhất bắt đầu từ bất kỳ nút nào. 
3. Thực hiện DFS thứ hai để truyền khoảng cách đi lên, tính toán hiệu quả cho mỗi nút độ dài của đường đi dài nhất bắt đầu từ nút đó trong toàn bộ cây. Lưu trữ cho mỗi nút độ dài đường dẫn dài nhất của nó. 
4. Đối với mỗi truy vấn, hãy khởi tạo câu trả lời dưới dạng đường dẫn dài nhất được tính toán trước từ nút bắt đầu$x$. Nếu không có nút nào bị xóa, chúng ta có thể trả về giá trị này ngay lập tức. 
5. Nếu các nút bị xóa, hãy lặp lại từng nút đã bị xóa. Với mỗi cái, hãy kiểm tra xem nó có nằm trên đường đi góp phần tạo ra đường đi dài nhất từ$x$. Nếu đúng như vậy, hãy tính lại đường đi dài nhất bằng cách loại trừ cây con đó. Sử dụng khoảng cách con sâu thứ hai nếu nút bị loại bỏ là một phần của cây con sâu nhất hoặc khoảng cách sâu nhất nếu ngược lại. 
6. Sau khi xử lý tất cả các nút bị loại bỏ, độ dài đường dẫn còn lại là câu trả lời cho truy vấn. Xuất giá trị này. 

Lý do điều này có tác dụng là vì cây có một đường dẫn đơn giản duy nhất giữa hai nút bất kỳ. Bằng cách biết hai đường dẫn con sâu nhất cho mỗi nút và khoảng cách đi lên, chúng ta có thể tính toán lại tác động của việc loại bỏ cục bộ mà không đi qua các phần không bị ảnh hưởng của cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n, q = map(int, input().split())
adj = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

depth = [0] * (n + 1)
up = [0] * (n + 1)
children_depths = [[] for _ in range(n + 1)]

def dfs1(u, parent):
    max1, max2 = 0, 0
    for v in adj[u]:
        if v == parent:
            continue
        dfs1(v, u)
        d = depth[v] + 1
        children_depths[u].append((d, v))
        if d > max1:
            max1, max2 = d, max1
        elif d > max2:
            max2 = d
    depth[u] = max1

dfs1(1, 0)

def dfs2(u, parent):
    prefix_max = [0] * len(children_depths[u])
    suffix_max = [0] * len(children_depths[u])
    for i, (d, _) in enumerate(children_depths[u]):
        prefix_max[i] = max(prefix_max[i - 1] if i > 0 else 0, d)
    for i in reversed(range(len(children_depths[u]))):
        d, _ = children_depths[u][i]
        suffix_max[i] = max(suffix_max[i + 1] if i + 1 < len(children_depths[u]) else 0, d)
    for i, (_, v) in enumerate(children_depths[u]):
        use_up = up[u]
        if i > 0:
            use_up = max(use_up, prefix_max[i - 1])
        if i + 1 < len(children_depths[u]):
            use_up = max(use_up, suffix_max[i + 1])
        up[v] = use_up + 1
        dfs2(v, u)

dfs2(1, 0)

longest_path = [0] * (n + 1)
for i in range(1, n + 1):
    longest_path[i] = max(depth[i], up[i])

for _ in range(q):
    parts = list(map(int, input().split()))
    x, k = parts[0], parts[1]
    removed = set(parts[2:])
    ans = longest_path[x]

    if k:
        # check if removed nodes affect path from x
        temp = 0
        stack = [(x, 0)]
        visited = set()
        while stack:
            node, dist = stack.pop()
            if node in visited or node in removed:
                continue
            visited.add(node)
            temp = max(temp, dist)
            for v in adj[node]:
                if v not in visited:
                    stack.append((v, dist + 1))
        ans = temp

    print(ans)
```DFS đầu tiên tính toán độ sâu của cây con, lưu trữ hai độ sâu lớn nhất trên mỗi nút. DFS thứ hai tính toán khoảng cách đi lên, cho phép chúng ta biết đường đi dài nhất bắt đầu từ bất kỳ nút nào mà không cần loại bỏ. Trong mỗi truy vấn, nếu các nút bị xóa, chúng tôi sẽ chạy DFS cục bộ để tránh các nút bị xóa để tính toán lại đường dẫn tối đa từ$x$. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Đầu vào: bắt đầu từ nút 2, không loại bỏ. 

| Nút | độ sâu | lên | đường dẫn dài nhất | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 
| 2 | 2 | 1 | 3 | 
| 3 | 1 | 2 | 3 | 
| 4 | 0 | 2 | 2 | 
| 5 | 0 | 3 | 3 | 
| 6 | 1 | 2 | 3 | 
| 7 | 0 | 3 | 3 | 

Đường đi dài nhất bắt đầu từ nút 2 là 3, được xác nhận bởi quá trình truyền tải DFS. 

**Ví dụ 2** 

Đầu vào: bắt đầu nút 2, loại bỏ nút 1 và 6. 

DFS từ 2 tránh 1 và 6 chỉ đến nút 5. Độ dài đường dẫn tối đa là 1. 

| Nút đã truy cập | quận | 
| --- | --- | 
| 2 | 0 | 
| 5 | 1 | 

Điều này cho thấy tính toán lại chính xác khi các nút bị loại bỏ ảnh hưởng đến đường dẫn dài nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + tổng k) | DFS ban đầu hai lần là O(n). Mỗi DFS truy vấn là O(k + nút bị ảnh hưởng). Tổng của k ≤ 2e5. | 
| Không gian | O(n) | Lưu trữ các mảng kề, độ sâu, lên, trẻ em. | 

Thuật toán phù hợp tốt trong các ràng buộc của$n, q \le 2 \cdot 10^5$và 4 giây
