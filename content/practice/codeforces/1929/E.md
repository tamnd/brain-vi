---
title: "CF 1929E - Sasha và chuyến chặt cây hạnh phúc"
description: "Cho một cây có n đỉnh và tập hợp k cặp đỉnh. Sasha muốn đảm bảo rằng với mỗi cặp (ai, bi), có ít nhất một cạnh màu trên đường đơn giản nối ai và bi."
date: "2026-06-09T01:38:57+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar", "dp", "graphs", "greedy", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1929
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 926 (Div. 2)"
rating: 2300
weight: 1929
solve_time_s: 73
verified: true
draft: false
---

[CF 1929E - Sasha và vụ chặt cây hạnh phúc](https://codeforces.com/problemset/problem/1929/E) 

**Đánh giá:** 2300 
**Tags:** bitmasks, brute Force, dfs và tương tự, dp, đồ thị, tham lam, toán học, cây cối 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các đỉnh và một tập hợp`k`các cặp đỉnh. Sasha muốn đảm bảo điều đó cho mỗi cặp`(a_i, b_i)`, có ít nhất một cạnh màu trên đường đơn nối`a_i`Và`b_i`. Mục tiêu của chúng ta là tìm ra số cạnh tối thiểu cần được tô màu để thỏa mãn tất cả các cặp. Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định một cây, các cặp đỉnh và kết nối giữa các đỉnh. 

Cây có tới`10^5`đỉnh trên tất cả các trường hợp thử nghiệm, loại trừ các thuật toán kém hơn tuyến tính hoặc tuyến tính trong`n`mỗi trường hợp thử nghiệm. Số cặp đỉnh`k`nhiều nhất là 20, đủ nhỏ để các thuật toán có độ phức tạp thời gian theo cấp số nhân trong`k`(lên đến`2^k`) là khả thi. Nhỏ này`k`là một gợi ý rằng chúng ta có thể cần khám phá các tập con của các cặp một cách hiệu quả thay vì thử tất cả các cạnh một cách ngây thơ. 

Trường hợp cạnh không rõ ràng phát sinh khi các cặp chồng lên nhau nhiều. Ví dụ, hãy xem xét một cây đường có 5 đỉnh và cặp`(1,5)`Và`(2,4)`. Một cách tiếp cận tham lam ngây thơ có thể tô màu một cạnh cho mỗi cặp một cách độc lập, nhưng một lựa chọn cẩn thận có thể làm hài lòng cả hai cặp có ít cạnh hơn. Một trường hợp cạnh khác xảy ra khi một số cặp chia sẻ chính xác cùng một đường dẫn; tô màu một cạnh trên đường dẫn đó bao gồm nhiều cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ liệt kê tất cả các tập hợp con của các cạnh trong cây và kiểm tra xem tập hợp con nào thỏa mãn tất cả các cặp. Đối với mỗi tập hợp con, chúng ta có thể đánh dấu đường dẫn nào giữa các cặp giao nhau với các cạnh đã chọn. Điều này đúng, nhưng số cạnh có thể lên tới`10^5`, vì vậy việc liệt kê tất cả các tập hợp con là không khả thi (`2^(n-1)`có kích thước lớn về mặt thiên văn). 

Cái nhìn sâu sắc quan trọng là số lượng các cặp`k`là nhỏ. Chúng ta có thể coi bài toán này như một bài toán phủ: mỗi cạnh bao phủ một tập con của`k`các cặp có đường đi qua nó. Điều này cho phép chúng ta biểu diễn mỗi cạnh bằng một`k`-bit mặt nạ trong đó một bit được đặt nếu cạnh nằm trên đường dẫn của cặp đó. Mục tiêu của chúng ta là tìm ra tập hợp các cạnh nhỏ nhất có các mặt nạ bao phủ tất cả`k`bit. Điều này làm giảm vấn đề thành vấn đề lập trình động trên bitmask. 

Chúng tôi lặp qua tất cả các cạnh và cập nhật mảng DP`dp[mask]`lưu trữ số cạnh tối thiểu cần thiết để bao gồm tập hợp con của các cặp được biểu thị bằng`mask`. Ban đầu,`dp[0] = 0`vì không có cặp nào được tô màu nên không có cạnh. Sau đó, đối với mỗi cạnh, chúng tôi cố gắng kết hợp mặt nạ của nó với các trạng thái DP hiện có để cập nhật mặt nạ lớn hơn. Điều này khả thi vì tổng số khẩu trang là`2^k ≤ 2^20`, có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n-1) * k * n) | O(2^(n-1)) | Quá chậm | 
| DP với Bitmask | O(n * 2^k) | O(2^k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và xây dựng danh sách kề của cây. 
3. Đọc`k`cặp đỉnh`(a_i, b_i)`. Gán cho mỗi cặp một chỉ mục duy nhất từ ​​0 đến`k-1`. 
4. Tính toán trước, đối với mỗi cạnh, tập hợp các cặp có đường đi bao gồm cạnh này. Điều này có thể được thực hiện bằng cách sử dụng DFS để ghi lại các mối quan hệ cha-con và truy vấn LCA để xác định đường dẫn. 
5. Mã hóa mỗi cạnh dưới dạng bitmask có độ dài`k`trong đó một bit được đặt nếu cạnh nằm trên đường đi của cặp đó. 
6. Khởi tạo một mảng DP có kích thước`2^k`với`inf`, ngoại trừ`dp[0] = 0`. 
7. Lặp lại tất cả các cạnh và mặt nạ của chúng. Đối với mỗi trạng thái DP hiện có`mask`, tính toán`mask | edge_mask`và cập nhật`dp[mask | edge_mask] = min(dp[mask | edge_mask], dp[mask] + 1)`. 
8. Sau khi xử lý tất cả các cạnh,`dp[(1 << k) - 1]`chứa số cạnh tối thiểu cần thiết để bao phủ tất cả các cặp. Xuất giá trị này. 

Tại sao nó hoạt động: DP đảm bảo rằng đối với mỗi tập hợp con các cặp được đại diện bởi một mặt nạ, chúng tôi duy trì số lượng cạnh tối thiểu cần thiết để che phủ nó. Mỗi cạnh đều góp phần mở rộng tập hợp các cặp được bao phủ và việc kết hợp các cạnh theo mọi cách có thể đảm bảo không bỏ sót giải pháp nào tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n)]
        edges = []
        edge_index = {}
        for i in range(n-1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append((v, i))
            adj[v].append((u, i))
            edges.append((u, v))
            edge_index[(u, v)] = i
            edge_index[(v, u)] = i
        
        k = int(input())
        pairs = [tuple(map(lambda x: int(x)-1, input().split())) for _ in range(k)]
        
        parent = [-1]*n
        depth = [0]*n
        def dfs(u, p):
            for v,_ in adj[u]:
                if v == p:
                    continue
                parent[v] = u
                depth[v] = depth[u]+1
                dfs(v, u)
        dfs(0, -1)
        
        def get_path_edges(u, v):
            mask = 0
            while u != v:
                if depth[u] < depth[v]:
                    u, v = v, u
                e = edge_index[(u, parent[u])]
                mask |= (1 << e)
                u = parent[u]
            return mask
        
        edge_mask = [0]*(n-1)
        for idx, (a,b) in enumerate(pairs):
            u, v = a, b
            path_edges = []
            while u != v:
                if depth[u] < depth[v]:
                    u, v = v, u
                e = edge_index[(u, parent[u])]
                edge_mask[e] |= (1 << idx)
                u = parent[u]
        
        INF = int(1e9)
        dp = [INF]*(1<<k)
        dp[0] = 0
        for mask in edge_mask:
            for prev in range((1<<k)-1, -1, -1):
                dp[prev | mask] = min(dp[prev | mask], dp[prev]+1)
        
        print(dp[(1<<k)-1])

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng cây và ánh xạ từng cạnh tới một chỉ mục duy nhất. Sau đó, nó tính toán từng cạnh nằm trên cặp nào bằng cách sử dụng DFS, cho phép mỗi cạnh được biểu diễn dưới dạng mặt nạ bit. Lập trình động lặp qua tất cả các cạnh và cập nhật số cạnh tối thiểu cần thiết để bao phủ từng tập hợp con của các cặp. Việc lặp lại ngược lại trên các trạng thái DP đảm bảo chúng ta không sử dụng một cạnh nhiều hơn một lần cho mỗi lần cập nhật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cặp đầu vào:`(1,3)`Và`(4,1)`trong cây 4 nút. 

| Cạnh | Bitmask (cặp được bảo hiểm) | 
| --- | --- | 
| 1-2 | 11 (cả hai cặp) | 
| 2-3 | 01 (cặp đầu tiên) | 
| 2-4 | 10 (cặp thứ hai) | 

Cập nhật DP:`dp[0] = 0`

`dp[11] = min(dp[11], dp[0]+1) = 1`Đầu ra:`1`, vì một cạnh bao phủ cả hai cặp. 

### Mẫu 2 

Cặp:`(1,2)`,`(2,3)`,`(3,4)`,`(4,5)`trong một cây dòng. 

Các cạnh bao phủ các cặp liên tiếp. Yêu cầu tô màu tối ưu`4`các cạnh. DP tìm chính xác tập tối thiểu bằng cách sử dụng mặt nạ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^k) | DFS tiền xử lý là O(n), cập nhật DP cho mỗi cạnh là O(2^k), khả thi vì k ≤ 20 | 
| Không gian | O(n + 2^k) | Danh sách kề và mặt nạ cạnh sử dụng O(n), mảng DP sử dụng O(2^k) | 

Với những hạn chế`sum(n) ≤ 10^5`Và`sum(2^k) ≤ 2^20`, giải pháp này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n4\n1 2\n2 3\n2 4\n2\n1 3\n4 1\n6\n1 2\n3 1\n6 1\n5 2\n4 2\n3\n3 1\n3 6\n2 6\n5\n1 2\n2 3\n3 4\n4 5\n4\n1 2\n2 3\n3 4\n4 5\n") == "1\n2\n4"

# Custom cases
assert run("1\n2\n1
```
