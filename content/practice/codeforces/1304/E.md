---
title: "CF 1304E - 1-Cây và Truy vấn"
description: "Chúng ta có một cây có $n$ đỉnh, trong đó mỗi đỉnh được kết nối sao cho có chính xác một đường đi giữa hai đỉnh bất kỳ. Sau đó chúng tôi được hỏi nhiều truy vấn."
date: "2026-06-11T17:56:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1304
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 620 (Div. 2)"
rating: 2000
weight: 1304
solve_time_s: 74
verified: true
draft: false
---

[CF 1304E - 1-Cây và Truy vấn](https://codeforces.com/problemset/problem/1304/E) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, dfs và các đường dẫn ngắn nhất, tương tự, cây 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh, trong đó mỗi đỉnh được nối sao cho có đúng một đường đi giữa hai đỉnh bất kỳ. Sau đó chúng tôi được hỏi nhiều truy vấn. Mỗi truy vấn chỉ định hai đỉnh$x$Và$y$nơi chúng ta tưởng tượng việc thêm một cạnh hai chiều mới và hai đỉnh$a$Và$b$giữa đó chúng ta muốn tìm ra một con đường. Truy vấn cũng cung cấp một số nguyên$k$, và chúng ta phải xác định liệu có tồn tại một đường đi từ$a$ĐẾN$b$sử dụng chính xác$k$các cạnh. Đường đi có thể đi lại các đỉnh hoặc các cạnh nhưng phải đi qua một cách chính xác$k$các cạnh. 

Kích thước cây$n$có thể lên đến$10^5$và có thể có tới$10^5$truy vấn. Điều này có nghĩa là bất kỳ thuật toán nào kiểm tra các đường dẫn một cách đơn giản hoặc mô phỏng các bước đi trên cây cho mỗi truy vấn đều quá chậm, vì ngay cả việc quét tuyến tính cho mỗi truy vấn cũng sẽ dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất. chiều dài$k$của con đường có thể đi lên đến$10^9$, vì vậy việc tạo đường dẫn rõ ràng là không thể. 

Chi tiết không rõ ràng là đường dẫn có thể truy cập lại các nút và cạnh, nghĩa là chúng ta có thể đi qua các chu trình được tạo bằng cách thêm cạnh bổ sung nhiều lần. Điều này cho phép các đường đi dài hơn đường đi ngắn nhất đơn giản, miễn là chúng ta đi qua chu trình một số lần thích hợp để đạt được độ dài chính xác$k$. Một cách tiếp cận bất cẩn chỉ xem xét độ dài đường đi ngắn nhất giữa$a$Và$b$sẽ thất bại đối với các truy vấn trong đó$k$lớn hơn nhưng có thể đạt được bằng cách lặp qua cạnh được thêm vào. 

Một trường hợp tế nhị khác là khi$a$Và$b$là cùng một nút. Độ dài đường đi có thể vẫn khác 0 nếu chúng ta đi qua các chu trình, vì vậy chúng ta không thể giả sử rằng độ dài bằng 0 là bắt buộc. Những cây nhỏ có 3 nút có thể tạo ra nhiều đường đi khi thêm cạnh phụ. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ là xây dựng cây, thêm cạnh truy vấn và thực hiện BFS hoặc DFS để liệt kê tất cả các đường dẫn từ$a$ĐẾN$b$cho đến khi độ dài đường đi đạt tới$k$. Điều này đúng vì nó kiểm tra mọi khả năng nhưng không khả thi. Đối với mỗi truy vấn, thậm chí việc khám phá tất cả các đường dẫn ngắn nhất cũng$O(n)$, và với$q = 10^5$, chúng tôi đang ở$10^{10}$hoạt động. Xử lý rõ ràng các đường dẫn có độ dài tối đa$10^9$là không thể. 

Cái nhìn sâu sắc quan trọng là một cái cây có một đường đi đơn giản duy nhất giữa hai đỉnh bất kỳ. Thêm một cạnh sẽ tạo ra đúng một chu trình đơn giản. Bất kỳ con đường nào giữa$a$Và$b$có thể được biểu diễn dưới dạng kết hợp của đường đi cây ban đầu và không hoặc nhiều lần đi qua của chu kỳ đơn này. Điều này làm giảm vấn đề tính toán khoảng cách ngắn nhất giữa$a$Và$b$trên cây và kiểm tra xem$k$có thể đạt được bằng cách sử dụng chu trình. Cụ thể, nếu đường đi ngắn nhất của cây là$d$, và chu trình có độ dài$c$, thì bất kỳ độ dài đường dẫn nào có thể truy cập phải đáp ứng$k \ge d$Và$(k - d) \mod 2 = 0$, bởi vì mỗi lần đi qua một cạnh chu trình sẽ thêm hai cạnh vào một đường đi đơn giản từ$a$ĐẾN$b$. 

Để chính thức hóa, độ dài chu kỳ là tổng khoảng cách từ$x$ĐẾN$y$qua cây cộng với cạnh được thêm vào. Sau đó, đối với mỗi truy vấn, chúng tôi kiểm tra ba độ dài đường dẫn ứng cử viên: đường dẫn trực tiếp từ$a$ĐẾN$b$qua cái cây, con đường$a \to x \to y \to b$, và đường đi$a \to y \to x \to b$. Nếu như$k$ít nhất là độ dài ứng cử viên và có cùng tính chẵn lẻ, câu trả lời là CÓ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây dưới dạng danh sách kề. Điều này cho phép tính toán di chuyển và khoảng cách nhanh chóng. Lưu trữ các cạnh theo cả hai hướng vì cây không có hướng. 
2. Thực hiện một DFS hoặc BFS duy nhất từ ​​một gốc tùy ý (đỉnh 1) để tính toán trước độ sâu của mỗi nút và nút cha của mỗi nút. Lưu trữ khoảng cách giữa nút gốc và mọi nút. Những khoảng cách này sẽ cho phép chúng ta tính khoảng cách cây giữa hai nút bất kỳ trong thời gian không đổi bằng công thức$\text{dist}(u,v) = \text{depth}[u] + \text{depth}[v] - 2 \cdot \text{depth}[\text{lca}(u,v)]$, trong đó LCA là tổ tiên chung thấp nhất. 
3. Thực hiện chức năng tính khoảng cách giữa hai nút bất kỳ. Sử dụng nâng cấp nhị phân cho LCA, chúng ta có thể nhận được khoảng cách của cây trong$O(\log n)$mỗi truy vấn. Từ$n \le 10^5$, điều này hiệu quả. 
4. Đối với mỗi truy vấn$(x, y, a, b, k)$, tính khoảng cách cây ngắn nhất giữa$a$Và$b$, mà chúng tôi gọi là$d_0$. Sau đó tính khoảng cách của các đường đi sử dụng cạnh phụ:$d_1 = \text{dist}(a,x) + 1 + \text{dist}(y,b)$Và$d_2 = \text{dist}(a,y) + 1 + \text{dist}(x,b)$. 
5. Đối với mỗi khoảng cách ứng viên$d$TRONG$[d_0, d_1, d_2]$, kiểm tra xem$k \ge d$Và$(k - d) \mod 2 = 0$. Nếu bất kỳ ứng viên nào thỏa mãn các điều kiện này, hãy in CÓ. Ngược lại in ra NO. Điều này đảm bảo rằng các đường đi dài hơn đường đi ngắn nhất chỉ được xem xét nếu chúng có thể được kéo dài theo chu kỳ đầy đủ để đạt được chính xác$k$. 

Tại sao nó hoạt động: Mọi đường dẫn trong cây 1 có thể được phân tách thành đường dẫn cây cơ sở và không có hoặc nhiều lần truyền tải đầy đủ trong một chu kỳ. Điều kiện chẵn lẻ đảm bảo rằng tổng số cạnh có thể đạt chính xác$k$không có sự truyền tải phân số. Việc xem xét tất cả ba đường dẫn ứng cử viên đảm bảo chúng ta tính đến cả hai hướng thông qua cạnh được thêm vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n = int(input())
adj = [[] for _ in range(n + 1)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

LOG = 17
parent = [[-1] * (n + 1) for _ in range(LOG)]
depth = [0] * (n + 1)

def dfs(u, p):
    parent[0][u] = p
    for v in adj[u]:
        if v != p:
            depth[v] = depth[u] + 1
            dfs(v, u)

dfs(1, -1)

for k in range(1, LOG):
    for v in range(1, n + 1):
        if parent[k - 1][v] != -1:
            parent[k][v] = parent[k - 1][parent[k - 1][v]]

def lca(u, v):
    if depth[u] < depth[v]:
        u, v = v, u
    for k in range(LOG - 1, -1, -1):
        if parent[k][u] != -1 and depth[parent[k][u]] >= depth[v]:
            u = parent[k][u]
    if u == v:
        return u
    for k in range(LOG - 1, -1, -1):
        if parent[k][u] != -1 and parent[k][u] != parent[k][v]:
            u = parent[k][u]
            v = parent[k][v]
    return parent[0][u]

def dist(u, v):
    return depth[u] + depth[v] - 2 * depth[lca(u, v)]

q = int(input())
for _ in range(q):
    x, y, a, b, k = map(int, input().split())
    d0 = dist(a, b)
    d1 = dist(a, x) + 1 + dist(y, b)
    d2 = dist(a, y) + 1 + dist(x, b)
    ok = False
    for d in [d0, d1, d2]:
        if k >= d and (k - d) % 2 == 0:
            ok = True
            break
    print("YES" if ok else "NO")
```DFS tính toán trước độ sâu và mảng gốc cho tất cả các nút. Nâng cấp nhị phân cho phép truy vấn LCA trong$O(\log n)$. Hàm khoảng cách tính toán khoảng cách cây một cách nhanh chóng. Ba đường dẫn ứng cử viên nắm bắt đường đi ngắn nhất và hai tùy chọn đi qua cạnh được thêm vào theo một trong hai hướng. Kiểm tra tính chẵn lẻ đảm bảo chúng ta có thể mở rộng đường dẫn bằng cách sử dụng chu trình để đạt được chính xác$k$các cạnh. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1** 

| Bước | d0 | d1 | d2 | k | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Truy vấn 1: 1 3 1 2 2 | 1 | 2 | 4 | 2 | CÓ | 
| Truy vấn 2: 1 4 1 3 2 | 3 | 2 | 4 | 2 | CÓ | 
| Câu 3: 1 4 | | | | | |
