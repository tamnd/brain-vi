---
title: "CF 104599I - Chủ nghĩa khủng bố giữa các thiên hà"
description: "Chúng ta có một cây có gốc với các nút $n$. Mỗi nút $i$ mang một giá trị dương $ai$. Cấu trúc này đã là một cái cây, do đó có chính xác các cạnh $n-1$, mỗi cạnh đều có trọng số $1$."
date: "2026-06-30T03:01:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "I"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 44
verified: true
draft: false
---

[CF 104599I - Chủ nghĩa khủng bố giữa các thiên hà](https://codeforces.com/problemset/problem/104599/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với$n$nút. Mỗi nút$i$mang giá trị dương$a_i$. Cấu trúc đã là một cái cây nên có chính xác$n-1$các cạnh, mỗi cạnh đều có trọng số$1$. Ngoài cấu trúc này, chúng ta được phép thêm chính xác một cạnh mới giữa hai nút riêng biệt bất kỳ$u$Và$v$. 

Việc thêm cạnh này sẽ tạo ra chính xác một chu trình đơn giản, vì trong cây có một đường dẫn duy nhất giữa hai nút bất kỳ. Chu trình đó bao gồm đường đi ban đầu giữa$u$Và$v$, cộng với cạnh mới$(u,v)$. Tổng “cường độ bùng nổ” của chu kỳ được định nghĩa là tổng của tất cả các trọng số cạnh trên chu kỳ đó. Mỗi cạnh gốc đều đóng góp$1$và cạnh được thêm vào góp phần$a_u + a_v$. 

Vì vậy nếu khoảng cách giữa$u$Và$v$trong cây là$\text{dist}(u,v)$, thì vụ nổ thu được là$$\text{dist}(u,v) + (a_u + a_v).$$Nhiệm vụ là chọn cặp tốt nhất$(u,v)$để tối đa hóa giá trị này. 

Các ràng buộc cho phép lên đến$10^5$các nút, loại trừ bất kỳ giải pháp nào kiểm tra tất cả các cặp một cách rõ ràng. Một sự ngây thơ$O(n^2)$quét qua các cặp sẽ cần khoảng$10^{10}$hoạt động vượt xa giới hạn 1 giây. Ngay cả những cách tiếp cận thực hiện BFS hoặc LCA cho mỗi cặp cũng quá chậm trừ khi được giảm bớt một cách cẩn thận. 

Một hạn chế quan trọng về cấu trúc là cây được đưa ra thông qua các con trỏ gốc, nghĩa là chúng ta có thể xử lý nó ở dạng gốc và tính toán độ sâu cũng như tập hợp cấu trúc một cách hiệu quả. 

Một số trường hợp đặc biệt bộc lộ những lỗi điển hình. Nếu tất cả$a_i$bằng nhau thì bài toán giảm đến mức tối đa hóa$a_u + a_v + \text{dist}(u,v)$, vì vậy cặp tốt nhất chỉ đơn giản là cặp điểm cuối đường kính. Ví dụ: trong một chuỗi có độ dài 3 với tất cả$a_i = 1$, câu trả lời là$2 + 2 = 4$cộng khoảng cách$2$, tổng cộng$6$. Một cách tiếp cận ngây thơ chỉ xem xét các nút lân cận sẽ bỏ lỡ điều này. 

Một trường hợp cạnh khác là khi lớn nhất$a_i$nằm trong một chiếc lá và ghép nối nó với một nút gần đó có vẻ hấp dẫn, nhưng thuật ngữ khoảng cách chiếm ưu thế và thay vào đó buộc phải ghép nối với nút xa nhất. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp$(u,v)$. Đối với mỗi cặp, hãy tính khoảng cách cây, ví dụ sử dụng LCA hoặc BFS. Mỗi truy vấn khoảng cách có thể được thực hiện trong$O(\log n)$với quá trình tiền xử lý, nhưng có$O(n^2)$cặp, dẫn đến$O(n^2 \log n)$. Điều này ngay lập tức là quá lớn đối với$n = 10^5$. 

Quan sát chính là mục tiêu được chia thành hai phần:$$a_u + a_v + \text{dist}(u,v).$$các$a$-terms chỉ phụ thuộc vào điểm cuối, trong khi khoảng cách chỉ phụ thuộc vào cấu trúc cây. Điều này gợi ý viết lại biểu thức ở dạng trong đó mỗi nút đóng góp độc lập dọc theo các đường dẫn. 

Sửa lỗi gốc tại nút$1$. Cho phép$d[u]$là độ sâu của$u$. Đối với bất kỳ cặp nào,$$\text{dist}(u,v) = d[u] + d[v] - 2d[\text{lca}(u,v)].$$Vì vậy mục tiêu trở thành$$(a_u + d[u]) + (a_v + d[v]) - 2d[\text{lca}(u,v)].$$Sự phức tạp là thuật ngữ LCA, ngăn cản sự phân tách hoàn toàn. Tuy nhiên, chúng ta có thể diễn giải lại cấu trúc: thay vì suy nghĩ một cách tổng thể, chúng ta có thể root cây và xử lý các đóng góp bằng cách sử dụng ý tưởng “hai ứng cử viên tốt nhất trong một cây con” kết hợp với lý luận kiểu rerooting. 

Đối với một nút cố định$x$được coi là LCA của cặp tối ưu, cả hai điểm cuối phải nằm trong các cây con khác nhau của$x$, hoặc một điểm cuối là$x$chính nó. Điều này làm giảm vấn đề cục bộ: đối với mỗi nút, chúng tôi muốn biết giá trị ứng cử viên tốt nhất đến từ mỗi cây con con, được đo bằng$$f[u] = a_u + d[u].$$Sau đó đối với một nút$x$, bất kỳ cặp nút nào trong các cây con con khác nhau đều cho ứng cử viên:$$f[u] + f[v] - 2d[x].$$Vì vậy, đối với mỗi nút, chúng tôi chỉ cần các giá trị cao nhất từ ​​mỗi cây con và chúng tôi duy trì các kết hợp tốt nhất giữa các nút con. 

Điều này biến vấn đề thành một cây DP trong đó mỗi nút tổng hợp hai điểm tốt nhất$f$-giá trị từ các cây con con của nó và lan truyền lên trên. 

Câu trả lời cuối cùng là tốt nhất: 

1. ghép nối hai nút trong các cây con khác nhau tại một số LCA 
2. việc ghép nối nút với chính nó là không hợp lệ vì$u \ne v$### Bảng độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút$1$và tính toán độ sâu$d[u]$. 

Độ sâu là cần thiết vì mọi cạnh đều có trọng lượng$1$, do đó nó trực tiếp đưa ra khoảng cách theo LCA. 
2. Xác định giá trị được chuyển đổi$f[u] = a_u + d[u]$. 

Điều này tách biệt sự đóng góp của nút khỏi chuyển động đi lên trong cây. 
3. Chạy DFS đặt hàng sau trên cây. 

Mỗi nút sẽ tính toán hai giá trị lớn nhất$f$-các giá trị nhìn thấy trong toàn bộ cây con của nó, được nhóm theo các nhánh con. 
4. Tại mỗi nút$x$, thu thập những gì tốt nhất$f$-giá trị đến từ mỗi cây con con. 

Điều này là cần thiết vì bất kỳ cặp dựa trên LCA hợp lệ nào cũng phải đến từ hai nhánh khác nhau. 
5. Kết hợp hai giá trị lớn nhất tại nút$x$. 

Nếu các giá trị tốt nhất là$f[u]$Và$f[v]$từ những đứa trẻ khác nhau, tính toán ứng cử viên:$$f[u] + f[v] - 2d[x].$$Điều này tương ứng chính xác với việc mở rộng$\text{dist}(u,v)$thông qua LCA. 
6. Theo dõi giá trị tối đa trên tất cả các nút. 

Điều này đảm bảo mọi LCA có thể được xem xét chính xác một lần như một điểm nối cấu trúc. 
7. Trả về giá trị lớn nhất tìm được. 

### Tại sao nó hoạt động 

Mỗi cặp nút có một tổ tiên chung thấp nhất duy nhất$x$. Sự đóng góp của cặp đó được xác định đầy đủ tại$x$khi chúng ta biết được đại diện tốt nhất từ ​​mỗi cây con con. Sự biến đổi$f[u] = a_u + d[u]$chuyển đổi công thức khoảng cách thành một tổng trong đó số hạng hiệu chỉnh duy nhất chỉ phụ thuộc vào độ sâu LCA, được cố định trên mỗi điểm tổng hợp. Vì mỗi cặp được xem xét chính xác tại LCA của nó nên không có cặp nào bị bỏ sót và không có cặp nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))
parent = [0] + list(map(int, input().split()))

g = [[] for _ in range(n)]
for i in range(1, n):
    p = parent[i]
    g[p-1].append(i)

depth = [0] * n

def dfs_depth(u, p):
    for v in g[u]:
        depth[v] = depth[u] + 1
        dfs_depth(v, u)

dfs_depth(0, -1)

ans = 0

def dfs(u):
    global ans
    best = []  # store f-values from different child subtrees

    f_u = a[u] + depth[u]

    for v in g[u]:
        child_best = dfs(v)
        best.append(child_best)

    # include node itself as candidate subtree
    best.append(f_u)

    # take top two
    best.sort(reverse=True)

    if len(best) >= 2:
        ans = max(ans, best[0] + best[1] - 2 * depth[u])

    return best[0]

dfs(0)

print(ans)
```Giải pháp trước tiên sẽ xây dựng cây gốc từ mảng gốc, sau đó tính toán độ sâu bằng DFS. Những độ sâu này được sử dụng để xác định
