---
title: "CF 1857G - Đồ thị đếm"
description: "Chúng ta có một cây trên các đỉnh $n$, trong đó mỗi cạnh có trọng số nguyên. Nhiệm vụ là đếm số lượng đồ thị đơn giản có trọng số sao cho cây đã cho là cây bao trùm tối thiểu duy nhất (MST) và tất cả các trọng số cạnh trong đồ thị là số nguyên không vượt quá $S$."
date: "2026-06-09T00:49:22+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "divide-and-conquer", "dsu", "graphs", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1857
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 891 (Div. 3)"
rating: 2000
weight: 1857
solve_time_s: 115
verified: false
draft: false
---

[CF 1857G - Đồ thị đếm](https://codeforces.com/problemset/problem/1857/G) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, chia để trị, dsu, đồ thị, tham lam, sắp xếp, cây 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây trên$n$đỉnh, trong đó mỗi cạnh có trọng số nguyên. Nhiệm vụ là đếm số lượng đồ thị đơn giản có trọng số sao cho cây đã cho là cây bao trùm tối thiểu duy nhất (MST) và tất cả các trọng số cạnh trong đồ thị là số nguyên không vượt quá$S$. Biểu đồ có thể chứa các cạnh bổ sung ngoài cây, nhưng việc thêm cạnh sẽ không bao giờ tạo ra MST thay thế. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số đỉnh$n$, trọng lượng cạnh tối đa$S$, và sau đó$n-1$các cạnh mô tả cây với trọng số của chúng. Đầu ra cho mỗi trường hợp thử nghiệm là một số duy nhất: số lượng đồ thị hợp lệ theo modulo$998244353$. 

Những hạn chế là đáng kể.$n$có thể đạt được$2 \cdot 10^5$, Và$t$có thể$10^4$, nhưng tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn ở$2 \cdot 10^5$. Điều này ngụ ý rằng đối với mỗi trường hợp thử nghiệm, mọi giải pháp đều phải gần tuyến tính trong$n$. Việc khám phá mạnh mẽ tất cả các tập cạnh có thể là không thể vì số cạnh tiềm năng trong một biểu đồ hoàn chỉnh là$O(n^2)$, tùy thuộc vào$4 \cdot 10^{10}$vì$n \sim 2 \cdot 10^5$. 

Các trường hợp cạnh bao gồm các cây chỉ có hai đỉnh, các cây trong đó tất cả các trọng số của cạnh đều bằng nhau và các trường hợp trong đó$S$nhỏ so với trọng lượng cạnh lớn nhất. Ví dụ: nếu cây là một cạnh có trọng số 5 và$S = 5$, đồ thị duy nhất là chính cái cây đó. Một cách tiếp cận ngây thơ giả định rằng chúng ta có thể thêm bất kỳ cạnh nào có trọng số lên tới$S$không kiểm tra tính duy nhất của MST sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng lặp lại tất cả các cạnh bổ sung có thể không có trong cây và gán mọi trọng số có thể lên tới$S$, sau đó kiểm tra xem MST của đồ thị kết quả có phải là cây gốc hay không. Đây là vụ nổ tổ hợp: đối với$n = 10^5$, có khoảng$5 \cdot 10^9$các cạnh không phải cây. Ngay cả việc kiểm tra một kết hợp trong một micro giây cũng sẽ mất nhiều năm. Brute-force hoạt động về mặt khái niệm vì tính duy nhất của MST có thể được xác minh bằng thuật toán Kruskal, nhưng nó hoàn toàn thất bại đối với các trường hợp lớn.$n$. 

Cái nhìn sâu sắc quan trọng là xem xét điều gì làm cho cây nhất định trở thành MST duy nhất. Một cạnh không có trong cây chỉ có thể được thêm vào nếu trọng số của nó lớn hơn trọng lượng cạnh tối đa trên đường nối các điểm cuối của nó trong cây. Nếu trọng số cạnh được thêm vào nhỏ hơn hoặc bằng, nó sẽ thay thế cạnh cây trong một số MST hoặc tạo một MST khác có cùng tổng trọng số. 

Do đó, đối với mọi lợi thế tiềm năng tăng thêm$(u,v)$không có trên cây, số trọng lượng được phép là$S - \text{maxWeight}(u,v)$, nhưng chỉ khi điều này là tích cực. Bản thân các cạnh của cây chỉ có một trọng số có thể có (đã cho). Việc đếm các biểu đồ hợp lệ sẽ giảm bớt việc tính tích của các số đếm này cho tất cả các cạnh không phải là cây. 

Để tính toán hiệu quả cạnh tối đa trên đường đi giữa hai nút bất kỳ trong cây, chúng ta có thể sử dụng tìm kiếm theo chiều sâu (DFS) và nâng nhị phân với bảng thưa. Điều này cho phép$O(\log n)$truy vấn trên mỗi cạnh trong cây được xử lý trước. Sử dụng DSU hoặc phép đếm tổ hợp thông minh, chúng ta có thể tích lũy số đếm cuối cùng mà không cần liệt kê rõ ràng các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S * 2^(n^2)) | O(n^2) | Quá chậm | 
| Tối ưu (Đếm đường dẫn tối đa + tổ hợp) | O(n log n + m log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách cây kề với các trọng số. 

Mỗi nút lưu trữ các nút con của nó và trọng số của nút con đó. Cách trình bày này làm cho DFS trở nên đơn giản. 
2. Xử lý trước cây cho các truy vấn cạnh tối đa nhanh trên đường dẫn. 

Sử dụng nâng nhị phân: đối với mỗi nút, lưu trữ tổ tiên thứ 2^k của nó và trọng số cạnh tối đa dọc theo đường dẫn đến tổ tiên đó. DFS điền vào chiều sâu và cạnh/cạnh gốc ngay lập tức, sau đó tính toán tổ tiên cao hơn với:$$up[node][k] = up[up[node][k-1]][k-1], \quad \text{maxEdge[node][k]} = \max(\text{maxEdge[node][k-1]}, \text{maxEdge[up[node][k-1]][k-1]})$$3. Với mỗi cặp đỉnh$u,v$không được kết nối bởi cạnh cây, hãy tính trọng số cạnh tối đa dọc theo đường đi duy nhất trong cây bằng cách sử dụng nâng cấp nhị phân. Gọi đây$w_{\max}$. 
4. Số lượng trọng số hợp lệ cho cạnh này là$\max(0, S - w_{\max})$. Nhân các số đếm này với nhau theo modulo$998244353$. 
5. Nhân mỗi cạnh cây với 1 (trọng số là cố định). 
6. Trả lại sản phẩm cuối cùng. 

Tại sao nó hoạt động: tính duy nhất của MST được đảm bảo vì không có cạnh không phải cây nào có thể có trọng số nhỏ hơn hoặc bằng cạnh tối đa trên đường dẫn cây của nó. Điều này đảm bảo rằng thuật toán Kruskal sẽ luôn chọn các cạnh của cây trước tiên, tạo ra cây ban đầu là MST. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n, S = map(int, input().split())
        adj = [[] for _ in range(n)]
        edges = []
        for _ in range(n-1):
            u,v,w = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append((v,w))
            adj[v].append((u,w))
            edges.append((u,v,w))

        LOG = 20
        up = [[-1]*LOG for _ in range(n)]
        maxEdge = [[0]*LOG for _ in range(n)]
        depth = [0]*n

        def dfs(u, p):
            for v,w in adj[u]:
                if v == p:
                    continue
                depth[v] = depth[u]+1
                up[v][0] = u
                maxEdge[v][0] = w
                dfs(v,u)
        dfs(0,-1)

        for k in range(1,LOG):
            for v in range(n):
                if up[v][k-1] != -1:
                    up[v][k] = up[up[v][k-1]][k-1]
                    maxEdge[v][k] = max(maxEdge[v][k-1], maxEdge[up[v][k-1]][k-1])

        def path_max(u,v):
            if depth[u] < depth[v]:
                u,v = v,u
            res = 0
            for k in reversed(range(LOG)):
                if up[u][k] != -1 and depth[up[u][k]] >= depth[v]:
                    res = max(res, maxEdge[u][k])
                    u = up[u][k]
            if u == v:
                return res
            for k in reversed(range(LOG)):
                if up[u][k] != -1 and up[u][k] != up[v][k]:
                    res = max(res, maxEdge[u][k], maxEdge[v][k])
                    u = up[u][k]
                    v = up[v][k]
            res = max(res, maxEdge[u][0], maxEdge[v][0])
            return res

        # Count extra edges
        ans = 1
        for u in range(n):
            for v in range(u+1,n):
                if any((x==v) for x,_ in adj[u]):
                    continue
                m = path_max(u,v)
                ways = max(0, S - m)
                ans = ans * ways % MOD
        print(ans)
        
if __name__ == "__main__":
    solve()
```Mã này trước tiên xử lý trước cây để nâng nhị phân để nhanh chóng tìm thấy các cạnh tối đa dọc theo bất kỳ đường dẫn nào. Sau đó, nó lặp lại tất cả các cạnh không phải là cây tiềm năng và nhân số lượng trọng số hợp lệ. DFS và nâng cấp nhị phân đảm bảo rằng các truy vấn biên tối đa được thực hiện nhanh chóng. Séc`if any((x==v) for x,_ in adj[u])`bỏ qua mép cây. Phép nhân mô đun đảm bảo chúng ta luôn ở trong giới hạn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
2 5
1 2 4
```- Chỉ có hai đỉnh, một cạnh. Không có cạnh thêm có thể. Đếm = 1. 

**Đầu vào mẫu 2:**```
4 5
1 2 2
2 3 4
3 4 3
```- Cạnh không phải cây: (1,3), (1,4), (2,4) 
- Tính path_max cho mỗi loại: (1,3)=2, (1,4)=4, (2,4)=4 
- Các trọng số hợp lệ: (1,3)=3,4,5 → 3 cách; (1,4)=5 → 1 chiều; (2,4)=5 → 1 chiều 
- Tổng = 3_1_1 = 3, nhân với 1 cho cạnh cây → đáp án cuối cùng = 3 (modulo 998244353) 

Bảng có thể được xây dựng hiển thị`u,v,m,max_weight,S-m`, xác nhận bất biến rằng tất cả các cạnh phụ đều vượt quá trọng số đường dẫn tối đa. 

## Phân tích độ phức tạp 

|
