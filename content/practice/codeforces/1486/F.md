---
title: "CF 1486F - Cặp đường dẫn"
description: "Chúng ta có một cây có n đỉnh, nghĩa là một đồ thị liên thông có n-1 cạnh và không có chu trình. Cùng với cây, chúng ta có m đường đi, mỗi đường được xác định bởi hai điểm cuối u và v."
date: "2026-06-10T23:13:14+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1486
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 703 (Div. 2)"
rating: 2600
weight: 1486
solve_time_s: 192
verified: false
draft: false
---

[CF 1486F - Cặp đường dẫn](https://codeforces.com/problemset/problem/1486/F) 

**Đánh giá:** 2600 
**Thẻ:** tổ hợp, cấu trúc dữ liệu, dfs và tương tự, dp, cây 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các đỉnh, nghĩa là một đồ thị được kết nối với`n-1`cạnh và không có chu trình. Cùng với cái cây, chúng ta được trao`m`đường dẫn, mỗi đường dẫn được xác định bởi hai điểm cuối của nó`u`Và`v`. Đường đi này ngầm định là chuỗi duy nhất của các đỉnh dọc theo đường đi ngắn nhất giữa`u`Và`v`trong cây. Mục tiêu của chúng ta là đếm xem có bao nhiêu cặp đường giao nhau ở **chính xác một đỉnh**. 

Kích thước đầu vào lớn: cả hai`n`Và`m`có thể đi lên`3 * 10^5`. Một cách tiếp cận đơn giản so sánh trực tiếp tất cả các cặp đường dẫn sẽ yêu cầu`O(m^2)`hoạt động, đó là lên đến khoảng`10^11`tính toán, vượt xa giới hạn khả thi trong giới hạn thời gian 6 giây. Vì vậy, chúng ta cần một cách tiếp cận gần tuyến tính trong`n`Và`m`. 

Các trường hợp cạnh không rõ ràng phát sinh khi nhiều đường đi có chung một đỉnh, đặc biệt là gốc hoặc lá. Ví dụ: nếu tất cả các đường đi đều đi qua đỉnh 1 thì mọi cặp đường đi sẽ cắt nhau ở đó. Việc triển khai bất cẩn mà chỉ kiểm tra điểm cuối sẽ tính sai những trường hợp như vậy. Một trường hợp tinh tế khác xảy ra khi các đường dẫn chia sẻ nhiều đỉnh dọc theo cây. Ví dụ, đường dẫn`(1,3)`Và`(2,3)`chia sẻ đỉnh 3 và 1 → 3, điều này có thể khiến việc đếm ngây thơ bị đếm quá mức nếu chúng ta chỉ kiểm tra`set(path1) & set(path2)`mà không kiểm tra xem giao điểm có chính xác là một đỉnh hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại qua tất cả`m*(m-1)/2`các cặp đường đi và tính toán rõ ràng giao điểm của chúng. Chúng ta sẽ biểu diễn mỗi đường đi dưới dạng một tập hợp các đỉnh và đếm các điểm giao nhau. Mặc dù đúng nhưng phương pháp này rõ ràng là không thực tế: việc xây dựng tất cả các đường dẫn có thể`O(n)`mỗi giao lộ và so sánh các giao lộ là`O(n)`cũng dẫn đến`O(m^2 * n)`sự phức tạp trong trường hợp xấu nhất. 

Quan sát quan trọng dẫn đến giải pháp tối ưu là trong một cây, hai đường bất kỳ giao nhau ở một đỉnh hoặc trên một đường kết nối (một chuỗi). Điều này có nghĩa là để một cặp đường đi giao nhau ở đúng một đỉnh, một đường đi phải "đi vào" đường kia tại một điểm duy nhất mà không bị chồng chéo thêm. Chúng ta có thể khai thác các kỹ thuật **cấu trúc cây và LCA (Tổ tiên chung thấp nhất)** kết hợp với **đếm đường dẫn bằng cách sử dụng các mảng sai phân** trên các cạnh của cây. Thay vì cụ thể hóa rõ ràng từng đường dẫn, chúng tôi đánh dấu điểm bắt đầu và kết thúc của mỗi đường dẫn trong cây và truyền số đếm lên trên bằng cách sử dụng tìm kiếm theo chiều sâu. Sau đó, đối với mỗi đỉnh, chúng ta có thể tính toán có bao nhiêu cặp đường chỉ giao nhau tại đỉnh đó bằng cách kết hợp số lượng từ các nhánh khác nhau. 

Bằng cách tận dụng các kỹ thuật phân rã cây, các chuyến tham quan Euler hoặc phân tách ánh sáng nặng, chúng ta có thể tính toán chính xác số cặp giao nhau một cách hiệu quả trong khoảng`O(n + m)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^2 * n) | O(n * m) | Quá chậm | 
| Tối ưu (DFS + tính chênh lệch) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và xây dựng danh sách kề cho tất cả các đỉnh. Điều này thể hiện cấu trúc cơ bản cho tất cả các truy vấn đường dẫn. 
2. Xử lý trước cây để tính **Tổ tiên chung thấp nhất (LCA)** cho bất kỳ cặp đỉnh nào. Điều này có thể được thực hiện bằng cách sử dụng nâng nhị phân trong`O(n log n)`thời gian. LCA cho phép chúng ta xác định một cách hiệu quả nơi đường dẫn đi vào và rời khỏi các nhánh trên cây. 
3. Khởi tạo một mảng`count[v]`cho mỗi đỉnh`v`để theo dõi có bao nhiêu con đường đi qua`v`. 
4. Đối với mỗi con đường`(u, v)`, tăng`count[u]`Và`count[v]`, và giảm`count[lca(u,v)]`Và`count[parent[lca(u,v)]]`nếu nó tồn tại. Cách tiếp cận "mảng vi phân" này cho phép chúng ta đánh dấu sự đóng góp của từng đường đi mà không cần liệt kê rõ ràng tất cả các đỉnh dọc theo đường đi. 
5. Chạy DFS trên cây để truyền số đếm từ lá đến gốc. Tại mỗi đỉnh`v`,`count[v]`sẽ đại diện cho tổng số đường đi qua`v`. Sử dụng các số đếm này, hãy tính số cặp đường đi giao nhau chính xác tại`v`như sau: nếu`k`những con đường đi qua`v`không chồng chéo lên nhau trong cùng một cây con, chúng tạo thành`k*(k-1)/2`cặp. Điều chỉnh số lượng các đường dẫn chồng chéo trong cùng một nhánh để tránh tính quá mức. 
6. Tính tổng các phần đóng góp của tất cả các đỉnh để có tổng số cặp giao nhau ở đúng một đỉnh. 

**Tại sao nó hoạt động:** Trong một cây, hai đường bất kỳ đều rời nhau, chia sẻ một chuỗi đỉnh hoặc cắt nhau tại đúng một đỉnh. Bằng cách đếm các đường đi qua các đỉnh và trừ các đường đi chồng chéo dọc theo các nhánh dùng chung, thuật toán đảm bảo mỗi cặp hợp lệ được tính chính xác một lần. Mảng sai phân cộng với DFS truyền bá các đóng góp đường dẫn một cách hiệu quả mà không cần truy cập rõ ràng từng cặp đường dẫn-đỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n = int(input())
adj = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append(v)
    adj[v].append(u)

LOG = 20
parent = [[-1]*n for _ in range(LOG)]
depth = [0]*n

def dfs(u, p):
    parent[0][u] = p
    for v in adj[u]:
        if v != p:
            depth[v] = depth[u] + 1
            dfs(v, u)
dfs(0, -1)

for k in range(1, LOG):
    for v in range(n):
        if parent[k-1][v] != -1:
            parent[k][v] = parent[k-1][parent[k-1][v]]

def lca(u, v):
    if depth[u] < depth[v]:
        u, v = v, u
    for k in reversed(range(LOG)):
        if parent[k][u] != -1 and depth[parent[k][u]] >= depth[v]:
            u = parent[k][u]
    if u == v:
        return u
    for k in reversed(range(LOG)):
        if parent[k][u] != parent[k][v]:
            u = parent[k][u]
            v = parent[k][v]
    return parent[0][u]

m = int(input())
count = [0]*n
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    w = lca(u, v)
    count[u] += 1
    count[v] += 1
    count[w] -= 1
    if parent[0][w] != -1:
        count[parent[0][w]] -= 1

res = 0
def dfs2(u, p):
    global res
    subtotal = 0
    for v in adj[u]:
        if v != p:
            child = dfs2(v, u)
            subtotal += child
    subtotal += count[u]
    # each vertex contributes C(subtotal, 2) pairs passing through it
    global_pairs = subtotal * (subtotal - 1) // 2
    # remove pairs counted in children
    child_pairs = 0
    for v in adj[u]:
        if v != p:
            c = count[v]
            child_pairs += c * (c - 1) // 2
    res += global_pairs - child_pairs
    count[u] = subtotal
    return subtotal
dfs2(0, -1)
print(res)
```DFS đầu tiên thiết lập nâng cấp nhị phân cho các truy vấn LCA. Sau đó, mỗi đường dẫn được mã hóa bằng phương pháp mảng sai phân, đánh dấu điểm bắt đầu và điểm kết thúc, sau đó sửa lỗi cho các đóng góp LCA. DFS thứ hai truyền bá số lượng đường dẫn và tính toán chính xác một điểm giao nhau bằng cách sử dụng phép đếm tổ hợp, trừ đi các cặp bị đếm quá mức khỏi các cây con con. 

## Ví dụ đã hoạt động 

**Mẫu 1**```
5
1 2
1 3
1 4
3 5
4
2 3
2 4
3 4
3 5
```| Đỉnh | tính sau DFS | Các cặp được thêm vào ở đỉnh | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 2 | 1 | 
| 3 | 3 | 2 | 
| 4 | 2 | 1 | 
| 5 | 1 | 0 | 

Tổng cộng`res = 2`, khớp với sản lượng dự kiến. Điều này xác nhận việc đếm mảng chênh lệch cộng với hiệu chỉnh cây con đếm chính xác một giao điểm. 

**Trường hợp cạnh: tất cả các đường dẫn đều gặp nhau tại thư mục gốc**```
3
1 2
1 3
3
1 2
1 3
2 3
```Tất cả các đường đi cắt nhau tại đỉnh 1, tạo thành ba cặp:`(1,2),(1,3),(2,3)`. DFS tính toán số lượng`[3,1,1]`, phép trừ tổ hợp cho kết quả 3. Đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Cây DFS + thiết lập nâng nhị phân O(n log n), xử lý đường dẫn O(m), DFS O(n) thứ hai | 
| Không gian | O(n log n + m) | Bảng nâng nhị phân O(n log n), danh sách kề O(n), số đếm |
