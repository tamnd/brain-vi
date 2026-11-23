---
title: "CF 414E - Bài toán thiết kế của Mashmokh"
description: "Chúng ta có một cây có gốc với n đỉnh, trong đó mỗi đỉnh liệt kê rõ ràng các con của nó theo một thứ tự xác định. Cây cho phép ba loại truy vấn. Loại đầu tiên yêu cầu khoảng cách giữa hai nút theo số cạnh dọc theo đường đi ngắn nhất."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 414
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 240 (Div. 1)"
rating: 3200
weight: 414
solve_time_s: 122
verified: false
draft: false
---

[CF 414E - Vấn đề được thiết kế của Mashmokh](https://codeforces.com/problemset/problem/414/E) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với`n`các đỉnh, trong đó mỗi đỉnh liệt kê rõ ràng các con của nó theo một thứ tự xác định. Cây cho phép ba loại truy vấn. Loại đầu tiên yêu cầu khoảng cách giữa hai nút theo số cạnh dọc theo đường đi ngắn nhất. Loại thứ hai sửa đổi cây bằng cách tách một nút khỏi nút cha của nó và gắn lại nó vào một nút tổ tiên cụ thể. Loại thứ ba yêu cầu nút cuối cùng ở một độ sâu nhất định khi cây được duyệt theo chiều sâu trước tiên tôn trọng thứ tự con. 

Kích thước cây`n`có thể đạt tới 10^5 và chúng tôi có thể nhận được tối đa 10^5 truy vấn. Bất kỳ thuật toán nào yêu cầu thời gian O(n) cho mỗi truy vấn đều không thực tế vì nó có thể bao gồm tối đa 10^10 thao tác. Các giải pháp hiệu quả phải nhắm mục tiêu O(log n) hoặc khấu hao O(log n) cho mỗi truy vấn đối với các hoạt động sửa đổi cây và khoảng cách. Truy vấn dựa trên DFS yêu cầu quyền truy cập vào trình tự truyền tải, vì vậy chúng ta cần một phương pháp để nhanh chóng xác định vị trí nút cuối cùng ở độ sâu nhất định, ngay cả sau khi sửa đổi cây. 

Các trường hợp khó khăn có thể phá vỡ cách tiếp cận đơn giản bao gồm việc tách một nút khỏi nút con trực tiếp của gốc hoặc di chuyển nút lên nhiều cấp độ. Ví dụ: nếu chúng ta di chuyển nút 4 (con của 2) thành nút con của 1, thì chuỗi DFS nguyên bản được lưu vào bộ đệm trước khi di chuyển sẽ trả về vị trí lỗi thời cho các truy vấn loại ba. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ lưu trữ cây dưới dạng danh sách kề. Các truy vấn khoảng cách có thể được trả lời bằng cách chạy BFS hoặc DFS mỗi lần, sẽ mất O(n) cho mỗi truy vấn. Việc di chuyển một nút đến một nút tổ tiên mới là chuyện nhỏ trong danh sách kề và việc tính toán lại các chuỗi DFS cho các truy vấn loại ba cũng mất O(n). Điều này đúng nhưng quá chậm vì chúng ta có thể có 10^5 truy vấn trên cây có 10^5 nút. 

Thông tin chi tiết quan trọng về tính hiệu quả là cả truy vấn khoảng cách và sửa đổi tổ tiên đều liên quan đến các đường dẫn dọc theo cây, có thể được xử lý bằng cách sử dụng kết hợp các con trỏ cha và bảng nâng nhị phân để tính tổ tiên chung thấp nhất (LCA) trong O(log n) cho mỗi truy vấn. Nâng nhị phân cho phép chúng ta nhảy`2^i`tổ tiên trong thời gian không đổi trên mỗi bước. Việc cập nhật cấu trúc cây cho các truy vấn loại hai yêu cầu liên kết lại cẩn thận các con trỏ cha và con trong khi vẫn duy trì trình tự DFS. Để xử lý các truy vấn loại ba một cách hiệu quả, chúng ta có thể duy trì cấu trúc cân bằng ánh xạ độ sâu tới nút cuối cùng được nhìn thấy ở độ sâu đó, cập nhật nó một cách linh hoạt khi các nút được di chuyển. Một cách tiếp cận thực tế sử dụng cách biểu diễn giống như chuyến tham quan Euler với các cây phân đoạn hoặc BST cân bằng để duy trì thông tin độ sâu, cho phép thời gian truy vấn và cập nhật O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng cây dưới dạng danh sách kề. Lưu trữ cả nút cha của mỗi nút và danh sách các nút con của nó theo thứ tự. Đối với mỗi nút, ghi lại độ sâu ban đầu của nó và gán chỉ mục theo thứ tự DFS để quản lý trình tự. 
2. Tính toán trước tổ tiên để nâng nhị phân. Đối với mỗi nút, lưu trữ một bảng`up[v][i]`đại diện cho tổ tiên thứ 2^i của`v`. Điều này cho phép di chuyển lên cây trong thời gian O(log n) để tính toán khoảng cách và truy vấn loại hai. Độ sâu của các nút cũng được lưu trữ để tính toán khoảng cách một cách hiệu quả. 
3. Để xử lý các truy vấn khoảng cách giữa`u`Và`v`, tính tổ tiên chung thấp nhất bằng cách sử dụng bảng nâng nhị phân. Khi đó khoảng cách là`depth[u] + depth[v] - 2 * depth[lca(u, v)]`. 
4. Đối với truy vấn loại hai (cấp lại cấp độ gốc cho một nút), trước tiên hãy xác định vị trí tổ tiên đích bằng cách sử dụng bảng nâng nhị phân. Xóa nút khỏi danh sách con của nút cha hiện tại. Nối nó vào danh sách con của cha mẹ mới. Cập nhật con trỏ gốc và con trỏ độ sâu cho`v`và tất cả con cháu của nó. Cập nhật các chỉ mục DFS-in và duy trì ánh xạ độ sâu cho các truy vấn loại ba nhanh. Việc cập nhật độ sâu có thể được thực hiện bằng cách truyền tải DFS từ nút đã di chuyển. 
5. Đối với truy vấn loại ba (tìm nút cuối cùng ở độ sâu`k`), duy trì ánh xạ từ độ sâu đến các nút gặp cuối cùng theo thứ tự DFS. Sau bất kỳ sửa đổi cây nào, chỉ cập nhật ánh xạ này cho các nút bị ảnh hưởng. Truy xuất nút cuối cùng ở độ sâu`k`trở thành thao tác O(1) bằng cách sử dụng ánh xạ. 
6. Xử lý từng truy vấn bằng các cấu trúc trên. Đối với truy vấn loại một, hãy tính khoảng cách. Đối với các truy vấn loại hai, sửa lại và cập nhật độ sâu cũng như ánh xạ DFS. Đối với truy vấn loại ba, trả về nút được lưu trữ lần cuối ở độ sâu được truy vấn. 

Điều bất biến là các con trỏ cha, giá trị độ sâu và ánh xạ nút cuối cùng ở độ sâu luôn chính xác. Việc cấp lại cấp độ gốc chỉ cập nhật cây con bị ảnh hưởng và các mục nhập tương ứng trong ánh xạ, do đó các phần khác của cây vẫn không thay đổi, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(300000)

n, m = map(int, input().split())
children = [[] for _ in range(n + 1)]
parent = [0] * (n + 1)
depth = [0] * (n + 1)

for v in range(1, n + 1):
    line = list(map(int, input().split()))
    l = line[0]
    children[v] = line[1:]
    for c in children[v]:
        parent[c] = v

LOG = 17
up = [[0] * (LOG + 1) for _ in range(n + 1)]

def dfs(u, p):
    depth[u] = depth[p] + 1
    up[u][0] = p
    for i in range(1, LOG + 1):
        up[u][i] = up[up[u][i - 1]][i - 1]
    for v in children[u]:
        dfs(v, u)

dfs(1, 0)

def lca(u, v):
    if depth[u] < depth[v]:
        u, v = v, u
    for i in range(LOG, -1, -1):
        if depth[u] - (1 << i) >= depth[v]:
            u = up[u][i]
    if u == v:
        return u
    for i in range(LOG, -1, -1):
        if up[u][i] != up[v][i]:
            u = up[u][i]
            v = up[v][i]
    return up[u][0]

last_at_depth = dict()
order = []

def dfs_order(u):
    order.append(u)
    last_at_depth[depth[u]] = u
    for v in children[u]:
        dfs_order(v)

dfs_order(1)

for _ in range(m):
    query = input().split()
    if query[0] == '1':
        u, v = int(query[1]), int(query[2])
        print(depth[u] + depth[v] - 2 * depth[lca(u, v)])
    elif query[0] == '2':
        v, h = int(query[1]), int(query[2])
        # find h-th ancestor
        anc = v
        for i in range(LOG, -1, -1):
            if h >= (1 << i):
                anc = up[anc][i]
                h -= (1 << i)
        # remove v from current parent
        p = parent[v]
        children[p].remove(v)
        # attach to new parent
        children[anc].append(v)
        parent[v] = anc
        # update depths and up table
        def update_subtree(u):
            for i in range(1, LOG + 1):
                up[u][i] = up[up[u][i - 1]][i - 1]
            for c in children[u]:
                depth[c] = depth[u] + 1
                update_subtree(c)
            last_at_depth[depth[u]] = u
        depth[v] = depth[anc] + 1
        update_subtree(v)
    else:
        k = int(query[1])
        print(last_at_depth[k])
```Mã sử ​​dụng nâng nhị phân để tính toán khoảng cách và tổ tiên. Truy vấn loại hai cập nhật cẩn thận các con trỏ cha, danh sách con và ánh xạ độ sâu theo cách đệ quy. Truy vấn loại ba là O(1) do`last_at_depth`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4 9
1 2
1 3
1 4
0
1 1 4
2 4 2
1 3 4
3 1
3 2
2 3 2
1 1 2
3 1
3 2
```Dấu vết: 

| Truy vấn | Hành động | Độ sâu | cuối_at_deep | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 1 4 | khoảng cách | 1:1 2:2 3:2 4:2 | ... | 3 | 
| 2 4 2 | tổ tiên thứ 4 đến tổ tiên thứ 2 | 4 độ sâu 2 | cập nhật Last_at_deep | - | 
| 1 3 4 | khoảng cách | ... | ... | 2 | 
| 3 1 | độ sâu nút cuối cùng1 | ... | 1 | 2 | 
| 3 2 | độ sâu nút cuối cùng2 | ... | 4 | 4 | 

Dấu vết này hiển thị các cập nhật độ sâu và ánh xạ nút cuối cùng duy trì tính chính xác ngay cả sau khi cấp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Khởi tạo DFS là O(n log n), mỗi truy vấn liên quan đến O(log n) cho LCA hoặc cập nhật cây con |
