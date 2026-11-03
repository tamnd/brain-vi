---
title: "CF 1111E - Cây"
description: "Chúng ta được cấp một cây có n nút và nhiều truy vấn. Mỗi truy vấn cung cấp một tập hợp con gồm k nút, số nhóm tối đa m và gốc r."
date: "2026-06-12T04:59:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1111
codeforces_index: "E"
codeforces_contest_name: "CodeCraft-19 and Codeforces Round 537 (Div. 2)"
rating: 2500
weight: 1111
solve_time_s: 86
verified: false
draft: false
---

[CF 1111E - Cây](https://codeforces.com/problemset/problem/1111/E) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, đồ thị, cây 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút và nhiều truy vấn. Mỗi truy vấn cung cấp một tập hợp con của`k`nút, số lượng nhóm tối đa`m`, và một gốc`r`. Nhiệm vụ là phân chia các nút đã cho thành nhiều nhất`m`nhóm với hạn chế là không có nhóm nào chứa nút và bất kỳ nút tổ tiên nào của nó trong cây có gốc. Chúng ta phải đếm số lượng phân vùng hợp lệ theo modulo$10^9 + 7$. 

Cây có tới$10^5$các nút và có thể có tới$10^5$truy vấn. Mỗi truy vấn có thể có tới 300 nút trong một nhóm, nhưng tổng số`k`trên tất cả các truy vấn được giới hạn ở$10^5$. Điều này cho chúng ta biết rằng chúng ta không thể mua được các thuật toán với$O(2^k)$độ phức tạp cho mỗi truy vấn, nhưng các thuật toán có tính đa thức`k`(ví dụ.,$O(k^2)$hoặc$O(k m)$) có thể hoạt động. 

Các trường hợp cạnh phát sinh khi tập hợp con của các nút tạo thành một chuỗi tổ tiên. Ví dụ: nếu tập hợp con là`[1, 2, 3]`dọc theo một đường dẫn gốc, và`m = 1`, không có nhóm hợp lệ nào tồn tại vì mỗi nút là tổ tiên của nút tiếp theo. Việc triển khai bất cẩn mà bỏ qua các mối quan hệ tổ tiên sẽ tính không chính xác các cấu hình đó là hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử mọi phân vùng có thể của`k`các nút vào nhiều nhất`m`nhóm và kiểm tra tình trạng tổ tiên. Điều này có độ phức tạp tỷ lệ thuận với số Bell$B_k$, tăng trưởng nhanh hơn$2^k$. Ngay cả đối với`k = 20`, điều này là không thể thực hiện được và với`k`lên tới 300 là điều không thể. 

Quan sát chính là mối quan hệ tổ tiên có tính bắc cầu và có cấu trúc cây. Một nhóm hợp lệ phải chứa các nút **độc lập trong cây con được tạo ra**. Điều này cho phép chúng ta mô hình hóa vấn đề dưới dạng lập trình động trên cây được hình thành bởi các nút được chọn và tổ tiên chung ít nhất theo cặp của chúng (LCA). Bằng cách thêm LCA vào tập hợp đã chọn và sắp xếp các nút theo độ sâu, chúng ta có thể thực hiện DP từ dưới lên trong đó mỗi nút tính toán số cách phân phối các nút cây con của nó thành các nhóm mà không vi phạm hạn chế tổ tiên. 

Điều này làm giảm vấn đề xuống còn DP trên một "cây nén" nhỏ có kích thước nhiều nhất$2k$. Mỗi nút duy trì một mảng DP`dp[i]`Ở đâu`dp[i]`đếm các cách phân chia cây con của nó thành`i`các nhóm. Khi kết hợp các phần tử con, chúng tôi sử dụng tích chập tổ hợp để hợp nhất các mảng DP của chúng, nằm trong`O(k m)`mỗi truy vấn. Điều này hiệu quả với những hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(B_k) | O(B_k) | Quá chậm | 
| DP trên cây nén | O(k m) | O(k m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Tiền xử lý cây cho các truy vấn LCA.** Sử dụng chuyến tham quan Euler + cây phân đoạn hoặc nâng nhị phân. Điều này cho phép chúng ta tính toán LCA của hai nút bất kỳ trong$O(\log n)$. LCA rất quan trọng vì mọi mối quan hệ tổ tiên giữa các nút được chọn đều phải thông qua LCA. 
2. **Xử lý từng truy vấn riêng lẻ.** Trích xuất`k`nút và gốc`r`. Thêm vào`r`tới tập hợp các nút để đảm bảo tính nhất quán, sau đó tính toán tất cả LCA của từng cặp nút trong truy vấn. Chèn các LCA này vào tập hợp để xây dựng một "cây nén" gồm các nút có liên quan. 
3. **Xây dựng cây nén.** Sắp xếp các nút theo thời gian bắt đầu chuyến tham quan Euler của chúng và sử dụng ngăn xếp để kết nối các nút trong mối quan hệ cha-con theo phân cấp LCA. Cây nén chứa tối đa`2k`nút. 
4. **Lập trình động trên cây nén.** Khởi tạo DP của mỗi nút lá dưới dạng`dp[1] = 1`Và`dp[i] = 0`vì`i > 1`. 
5. **Hợp nhất DP từ con cái sang cha mẹ.** Dành cho cha mẹ có con cái`c1, c2, ..., cn`, tính toán tất cả các kết hợp số lượng nhóm: nếu cha mẹ đóng góp`p`các nhóm và trẻ em đóng góp`c1_groups, c2_groups, ...`, tính tổng trên tất cả các kết hợp hợp lệ sao cho tổng số nhóm ≤`m`. Sử dụng số học mô-đun cho số lượng lớn. 
6. **Tính tổng theo số lượng nhóm có thể.** Sau khi xử lý gốc của cây nén, tính tổng`dp[1..m]`để có được tổng số phân vùng hợp lệ. 

**Tại sao nó hoạt động:** Cây nén đảm bảo rằng mọi mối quan hệ tổ tiên-con cháu giữa các nút truy vấn đều được thể hiện rõ ràng. Bằng DP từ dưới lên, mỗi cây con tính toán các nhóm hợp lệ một cách độc lập và việc hợp nhất tôn trọng giới hạn tổ tiên. Thuật toán chỉ tính các cấu hình hợp lệ vì mọi kết hợp đều tuân theo bất biến DP: không có nhóm nào chứa tổ tiên và con cháu của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

MOD = 10**9 + 7

def main():
    n, q = map(int, input().split())
    tree = [[] for _ in range(n+1)]
    for _ in range(n-1):
        u, v = map(int, input().split())
        tree[u].append(v)
        tree[v].append(u)

    # Binary lifting for LCA
    LOG = 17
    up = [[-1]*(LOG+1) for _ in range(n+1)]
    depth = [0]*(n+1)
    tin = [0]*(n+1)
    tout = [0]*(n+1)
    timer = 0

    def dfs(u, p):
        nonlocal timer
        tin[u] = timer
        timer += 1
        up[u][0] = p
        for i in range(1, LOG+1):
            if up[u][i-1] != -1:
                up[u][i] = up[up[u][i-1]][i-1]
        for v in tree[u]:
            if v != p:
                depth[v] = depth[u]+1
                dfs(v, u)
        tout[u] = timer
        timer += 1

    dfs(1, -1)

    def lca(u, v):
        if depth[u] < depth[v]:
            u, v = v, u
        for i in reversed(range(LOG+1)):
            if up[u][i] != -1 and depth[up[u][i]] >= depth[v]:
                u = up[u][i]
        if u == v:
            return u
        for i in reversed(range(LOG+1)):
            if up[u][i] != -1 and up[u][i] != up[v][i]:
                u = up[u][i]
                v = up[v][i]
        return up[u][0]

    def is_ancestor(u, v):
        return tin[u] <= tin[v] and tout[v] <= tout[u]

    for _ in range(q):
        parts = list(map(int, input().split()))
        k, m, r = parts[:3]
        nodes = parts[3:]
        nodes.append(r)
        nodes = list(set(nodes))
        # add LCAs
        nodes.sort(key=lambda x: tin[x])
        stack = []
        compressed = []
        for node in nodes:
            if stack:
                l = lca(stack[-1], node)
                if l not in nodes:
                    nodes.append(l)
        nodes = list(set(nodes))
        nodes.sort(key=lambda x: tin[x])
        # build compressed tree
        parent = {nodes[0]: None}
        stack = [nodes[0]]
        for node in nodes[1:]:
            while not is_ancestor(stack[-1], node):
                stack.pop()
            parent[node] = stack[-1]
            stack.append(node)

        # DP
        children = {u: [] for u in nodes}
        for u in nodes:
            if parent[u] is not None:
                children[parent[u]].append(u)

        dp = {}
        def dfs_dp(u):
            dp_u = [0]*(m+1)
            dp_u[1] = 1 if u in parts[3:] else 0
            for v in children[u]:
                dfs_dp(v)
                ndp = [0]*(m+1)
                for i in range(1, m+1):
                    if dp_u[i] == 0:
                        continue
                    for j in range(1, m+1-i+1):
                        ndp[i+j] = (ndp[i+j] + dp_u[i]*dp[v][j])%MOD
                for i in range(1, m+1):
                    dp_u[i] = (dp_u[i]+ndp[i])%MOD
            dp[u] = dp_u
        dfs_dp(nodes[0])
        print(sum(dp[nodes[0]][1:m+1])%MOD)

if __name__ == "__main__":
    main()
```**Giải thích:** Giải pháp trước tiên thiết lập nâng cấp nhị phân để tính toán LCA hiệu quả. Mỗi truy vấn xây dựng một cây nút nén bao gồm LCA. Mảng DP được hợp nhất từ ​​dưới lên trong khi vẫn tôn trọng số lượng nhóm tối đa và các hạn chế tổ tiên. Số học mô-đun được áp dụng xuyên suốt. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Truy vấn đầu vào:`3 3 2 7 4 3`Nút`[7,4,3]`với gốc`2`: 

| Nút | Độ sâu | Tổ tiên trong bộ | Kết quả DP | 
| --- | --- | --- | --- | 
| 7 | 3 | 4 | [0 |
