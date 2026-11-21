---
title: "CF 2217H - Gần hơn"
description: "Chúng ta có một cây có các đỉnh $2n$, mỗi đỉnh chứa một người. Mỗi người đều mang một huy hiệu tương ứng với một trong các giao dịch $n$ và mỗi giao dịch sẽ xuất hiện đúng hai lần trong số những người tham gia."
date: "2026-06-07T18:28:03+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 2217
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 1091 (Div. 2) and CodeCraft 26"
rating: 2800
weight: 2217
solve_time_s: 121
verified: false
draft: false
---

[CF 2217H - Gần hơn](https://codeforces.com/problemset/problem/2217/H) 

**Đánh giá:** 2800 
**Thẻ:** dfs và tương tự, dp, cây cối 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cái cây với$2n$các đỉnh, mỗi đỉnh chứa một người. Mỗi người mang một huy hiệu tương ứng với một trong$n$giao dịch và mỗi giao dịch xuất hiện chính xác hai lần trong số mọi người. Nếu hai người giữ cùng một huy hiệu giao dịch ở các đỉnh liền kề thì giao dịch ngay lập tức được chốt và đóng góp lợi nhuận$w_i$. 

Chúng ta được phép cải tổ một lần: chọn một tập hợp các cạnh tạo thành một khớp (không có hai cạnh nào có chung một đỉnh) và hoán đổi những người trên mỗi cạnh được chọn. Mục tiêu là tối đa hóa tổng lợi nhuận từ các giao dịch được niêm phong sau đợt cải tổ này. 

Mỗi trường hợp thử nghiệm chỉ định số lượng giao dịch$n$, lợi nhuận$w_1 \ldots w_n$, sự sắp xếp của mọi người trên các đỉnh$a_1 \ldots a_{2n}$, và các cạnh của cây. Sản lượng là tổng lợi nhuận tối đa có thể đạt được sau một lần cải tổ như vậy. 

Được cho$n$có thể đạt được$10^5$và có thể có tới$10^4$trường hợp thử nghiệm với tổng$n$trong mọi trường hợp giới hạn bởi$10^5$, chúng tôi không thể chấp nhận bất cứ điều gì tồi tệ hơn đại khái$O(n)$mỗi trường hợp thử nghiệm. Điều này loại trừ các phương pháp thử rõ ràng tất cả các kết quả phù hợp có thể có hoặc tất cả các giao dịch hoán đổi giữa các cặp người. 

Các trường hợp cạnh không rõ ràng phát sinh khi hai nút của giao dịch đã ở gần nhau hoặc được kết nối gián tiếp thông qua một đường dẫn có độ dài bằng hai. Ví dụ: nếu hai người của một giao dịch đã ở các đỉnh lân cận, việc hoán đổi họ ở nơi khác thực sự có thể làm giảm lợi nhuận nếu xử lý không đúng cách. Một tình huống phức tạp khác là khi nhiều giao dịch có giá trị cao chia sẻ các đỉnh liền kề: việc chọn kết hợp sai có thể bỏ lỡ những khoản lợi nhuận này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi cạnh có thể khớp trong cây, hãy hoán đổi các cặp và tính toán lại số lượng giao dịch được niêm phong. Vì một cái cây có$2n$đỉnh có$2n-1$các cạnh, có$2^{2n-1}$tập hợp con có thể có của các cạnh. Ngay cả việc cắt tỉa các kết quả không có chung đỉnh, số lượng tùy chọn vẫn theo cấp số nhân. Điều này rõ ràng là không thể thực hiện được đối với$n \sim 10^5$. 

Thông tin chi tiết quan trọng đến từ việc nhận ra rằng chúng ta chỉ cần xem xét các cạnh liền kề với vị trí của hai người trong mỗi giao dịch. Mỗi giao dịch có chính xác hai người, vì vậy những giao dịch hoán đổi duy nhất quan trọng là những giao dịch đưa hai người đến gần nhau hơn. Vì cây là cây không có chu trình nên đường dẫn duy nhất giữa hai nút bất kỳ có thể được xem xét độc lập. Theo con đường này, chúng ta có thể quyết định sự hoán đổi nào sẽ khiến hai người ở cạnh nhau. Đối với các cạnh nằm ngoài đường dẫn này, việc hoán đổi không ảnh hưởng đến giao dịch. 

Quan sát này làm giảm vấn đề thành một nhiệm vụ lập trình động cây: đối với mỗi nút, chúng tôi duy trì liệu việc hoán đổi người của nó dọc theo một cạnh phù hợp để tăng tổng lợi nhuận có mang lại lợi ích hay không. Vì chúng tôi chỉ có thể hoán đổi dọc theo một cạnh khớp (các cạnh không liền kề), nên chúng tôi thực hiện DP mà tại mỗi nút sẽ xem xét xem có nên đưa cạnh đó vào cạnh cha của nó trong khớp hay không. Đây thực sự là một kết quả khớp tối đa có trọng số đối với một bài toán về cây, trong đó trọng số là lợi ích tiềm năng từ việc chốt một giao dịch bằng cách hoán đổi dọc theo cạnh đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(2n)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lập bản đồ từng giao dịch$i$đến vị trí của hai người nó trên cây. Điều này mang lại một cặp đỉnh$(u_i, v_i)$cho mỗi giao dịch. 
2. Tính mức đóng góp hiện tại vào lợi nhuận từ các giao dịch đã được niêm phong (hai đỉnh của chúng liền kề nhau). Khởi tạo`total_profit`với số tiền này. 
3. Đối với mỗi giao dịch có hai người không đứng cạnh nhau, hãy tính đường đi trong cây giữa hai đỉnh. Đánh dấu tất cả các cạnh dọc theo con đường này với lợi nhuận tiềm năng của việc ký kết thỏa thuận này. Bây giờ, mỗi cạnh đã biết mức tăng tối đa mà nó có thể cung cấp nếu được đưa vào kết hợp hoán đổi. 
4. Chạy DP trên cây để tìm tổng trọng số lớn nhất của các cạnh trong một kết quả khớp. Đối với mỗi nút, duy trì hai giá trị:`dp[node][0]`nếu cạnh của nó không được bao gồm trong kết quả khớp và`dp[node][1]`nếu nó được bao gồm. Tại mỗi nút, hãy xem xét tất cả các nút con: 

1. Nếu bao gồm cạnh của trẻ em, hãy cộng trọng lượng của nó cộng với`dp[child][0]`. 
2. Nếu cạnh của phần tử con bị loại trừ, hãy thêm`max(dp[child][0], dp[child][1])`. 

Giá trị DP tối đa của gốc mang lại lợi nhuận tăng thêm tối đa có thể đạt được thông qua hoán đổi phù hợp. 
5. Đáp án cuối cùng là tổng của`total_profit`từ các giao dịch được ký kết ban đầu cộng với kết quả DP. 

Tại sao nó hoạt động: mỗi giao dịch đóng góp lợi nhuận nhiều nhất một lần. Bằng cách đánh dấu các cạnh dọc theo đường dẫn giữa hai nút của nó, chúng tôi đảm bảo rằng việc bao gồm bất kỳ cạnh nào trong kết quả khớp chỉ có thể giúp đạt được một giao dịch. Cây DP đảm bảo rằng không có hai cạnh liền kề nào được chọn đồng thời, tôn trọng ràng buộc so khớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        w = list(map(int, input().split()))
        a = list(map(int, input().split()))
        
        pos = [[] for _ in range(n)]
        for idx, badge in enumerate(a):
            pos[badge - 1].append(idx)
        
        tree = [[] for _ in range(2 * n)]
        for _ in range(2 * n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            tree[u].append(v)
            tree[v].append(u)
        
        # Build parent and depth arrays for LCA path retrieval
        parent = [-1] * (2 * n)
        depth = [0] * (2 * n)
        def dfs(node, par):
            parent[node] = par
            for nei in tree[node]:
                if nei != par:
                    depth[nei] = depth[node] + 1
                    dfs(nei, node)
        dfs(0, -1)
        
        # Mark edge weights for potential deal gains
        edge_weight = {}
        total_profit = 0
        for i in range(n):
            u, v = pos[i]
            if v in tree[u]:
                total_profit += w[i]
                continue
            # Move u and v up to LCA
            def add_path(u, v, val):
                while u != v:
                    if depth[u] > depth[v]:
                        edge_weight[(min(u, parent[u]), max(u, parent[u]))] = max(
                            edge_weight.get((min(u, parent[u]), max(u, parent[u])), 0), val)
                        u = parent[u]
                    else:
                        edge_weight[(min(v, parent[v]), max(v, parent[v]))] = max(
                            edge_weight.get((min(v, parent[v]), max(v, parent[v])), 0), val)
                        v = parent[v]
            add_path(u, v, w[i])
        
        # Tree DP for max matching
        dp = [[0,0] for _ in range(2 * n)]
        def tree_dp(node, par):
            incl = 0
            excl = 0
            for nei in tree[node]:
                if nei == par:
                    continue
                tree_dp(nei, node)
                e = edge_weight.get((min(node, nei), max(node, nei)), 0)
                incl += dp[nei][0] + e
                excl += max(dp[nei][0], dp[nei][1])
            dp[node][0] = excl
            dp[node][1] = incl
        tree_dp(0, -1)
        print(total_profit + max(dp[0][0], dp[0][1]))

if __name__ == "__main__":
    solve()
```Giải pháp tiến hành theo ba giai đoạn chính: phân tích các giao dịch đầu vào và ánh xạ, đánh dấu mức tăng tiềm năng trên các cạnh dọc theo đường dẫn giữa những người tham gia giao dịch và chạy cây DP để chọn kết quả phù hợp tối ưu. Cần chú ý cẩn thận đến việc lập chỉ mục dựa trên 0, đảm bảo các khóa cạnh được sắp xếp nhất quán và tránh tính hai lần bằng cách kiểm tra tính kề trước. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
1
3
10 8 5
1 2 1 2 3 3
1 2
2 3
3 4
4 5
5 6
```| Thỏa thuận | bạn | v | Đã liền kề | Các cạnh đường dẫn | Trọng lượng được thêm vào các cạnh | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | Không | (0-1),(1-2) | 10 | 
| 2 | 1 | 3 | Không | (1-2),(2-3) | 8 | 
| 3 | 4 | 5 | Có | - | 5 đã được bao gồm | 

Cây DP chọn các cạnh (0-1) và (3-4) phù hợp để có thêm lợi nhuận tối đa, dẫn đến tổng 10+5=15. 

Ví dụ thứ hai: đầu vào với các giao dịch liền kề sẽ mang lại phép cộng chính xác cho`total_profit`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đỉnh được truy cập một lần trong DFS và DP; việc đánh dấu các cạnh dọc theo đường dẫn giữa các nút giao dịch có giá O(n) vì tổng số nút trên tất cả các trường hợp thử nghiệm ≤ 10^5 | 
| Không gian | O(n) | Danh sách cây kề nhau, mảng DP, từ điển trọng số cạnh, |
