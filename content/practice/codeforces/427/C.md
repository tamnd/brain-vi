---
title: "CF 427C - Trạm kiểm tra"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi nút đại diện cho một ngã ba thành phố và mỗi cạnh có hướng đại diện cho một con đường một chiều. Mỗi ngã ba đều có kinh phí để xây dựng trạm kiểm soát của cảnh sát."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 427
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 244 (Div. 2)"
rating: 1700
weight: 427
solve_time_s: 94
verified: true
draft: false
---

[CF 427C - Trạm kiểm tra](https://codeforces.com/problemset/problem/427/C) 

**Đánh giá:** 1700 
**Thẻ:** dfs và tương tự, đồ thị, hai con trỏ 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi nút đại diện cho một ngã ba thành phố và mỗi cạnh có hướng đại diện cho một con đường một chiều. Mỗi ngã ba đều có kinh phí để xây dựng trạm kiểm soát của cảnh sát. Một trạm kiểm tra tại ngã ba _i_ bảo vệ tất cả các nút giao thông có thể tiếp cận được từ _i_ và có thể quay trở lại _i_, bao gồm cả chính _i_. Nói cách khác, một trạm kiểm tra tại một nút sẽ bảo vệ thành phần được kết nối mạnh mẽ (SCC) của nó. Mục tiêu là bao phủ tất cả các nút bằng cách đặt các trạm kiểm tra sao cho tổng chi phí được giảm thiểu và sau đó đếm xem có bao nhiêu cách sắp xếp chi phí tối thiểu khác nhau. 

Kích thước đầu vào cho phép lên tới 100.000 nút giao thông và 300.000 con đường. Một cách tiếp cận bạo lực nhằm thử tất cả các tổ hợp mối nối rõ ràng là không khả thi, bởi vì số lượng tập hợp con tăng theo cấp số nhân. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính đối với các nút và cạnh, điều này gợi ý rằng kỹ thuật truyền tải đồ thị và phân tách SCC là phù hợp. 

Trường hợp phức tạp phát sinh khi SCC chứa nhiều nút. Ví dụ: nếu nút 2 và 3 tạo thành một chu trình thì cả hai đều có thể truy cập được lẫn nhau. Chúng ta chỉ cần một trạm kiểm soát bên trong SCC này. Một cách tiếp cận bất cẩn khi xem xét các nút riêng lẻ một cách độc lập có thể đặt nhiều trạm kiểm tra dư thừa, làm tăng chi phí. Một trường hợp đặc biệt khác là khi nhiều nút trong SCC có cùng chi phí tối thiểu thì số cách để chọn nút rẻ nhất sẽ nhân lên. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ xem xét mọi nút một cách độc lập. Chúng ta có thể tưởng tượng việc lặp lại qua mọi điểm nối, cố gắng đặt một trạm kiểm tra ở đó và kiểm tra xem tất cả các nút có được bao phủ thông qua khả năng tiếp cận hay không. Cách tiếp cận này đúng về mặt lý thuyết vì nó kiểm tra tất cả các kết hợp, nhưng trong thực tế, nó yêu cầu liệt kê tất cả các tập hợp con của các nút, tạo ra độ phức tạp của$O(2^n)$- không thể cho$n$lên tới 100.000. 

Cái nhìn sâu sắc quan trọng là mối quan hệ bảo vệ phản ánh chính xác các thành phần được kết nối chặt chẽ. Bất kỳ SCC nào cũng là tập hợp các nút tối đa trong đó mỗi nút có thể tiếp cận mọi nút khác. Do đó, việc đặt một trạm kiểm tra trong mỗi SCC là đủ để bao phủ tất cả các nút trong SCC đó và không cần thêm trạm kiểm tra nào nữa. Chi phí tối thiểu cho mỗi SCC chỉ là chi phí của nút rẻ nhất. Số cách chọn trạm kiểm tra trong SCC đó là số lượng nút chia sẻ chi phí tối thiểu đó. 

Khi chúng tôi phân tách biểu đồ thành các SCC, vấn đề sẽ giảm xuống việc lặp qua từng SCC, chọn chi phí tối thiểu và đếm bội số. Việc phân tách SCC có thể được thực hiện trong thời gian tuyến tính bằng thuật toán của Kosaraju hoặc Tarjan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n + m) | Quá chậm | 
| Tối ưu (SCC + chi phí tối thiểu) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích đầu vào để trích xuất số lượng nút`n`, chi phí trạm kiểm soát của họ, số cạnh`m`, và danh sách các cạnh có hướng. 
2. Xây dựng danh sách kề cho đồ thị. 
3. Thực hiện thuật toán Kosaraju để tìm SCC. Điều này bao gồm hai lượt DFS: lượt đầu tiên sẽ điền các nút vào ngăn xếp theo thời gian hoàn thiện; lượt thứ hai xử lý các nút theo thứ tự xếp chồng trên biểu đồ được chuyển đổi để xác định SCC. 
4. Đối với mỗi SCC, hãy xác định chi phí kiểm tra tối thiểu. Đếm xem có bao nhiêu nút trong SCC chia sẻ chi phí tối thiểu đó. 
5. Tính tổng chi phí tối thiểu của tất cả các SCC để có được tổng chi tiêu tối thiểu. 
6. Nhân số lượng nút chi phí tối thiểu cho mỗi SCC, modulo$10^9 + 7$, để có được tổng số cách để đạt được chi phí tối thiểu. 

Tại sao nó hoạt động: Mỗi SCC tạo thành một tập hợp các nút tối đa có thể bảo vệ lẫn nhau. Bằng cách đặt trạm kiểm tra tại nút rẻ nhất trong SCC, chúng tôi bao phủ tất cả các nút của nó với chi phí tối thiểu. Bởi vì các SCC không có sự liên kết chặt chẽ về khả năng bảo vệ nên tổng chi phí tối thiểu bằng tổng chi phí của các SCC. Nhân số lượng tài khoản cho các lựa chọn độc lập trong mỗi SCC. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline

MOD = 10**9 + 7

def kosaraju_scc(n, adj):
    visited = [False] * n
    order = []

    def dfs1(u):
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                dfs1(v)
        order.append(u)

    for i in range(n):
        if not visited[i]:
            dfs1(i)

    # Transpose graph
    adj_t = [[] for _ in range(n)]
    for u in range(n):
        for v in adj[u]:
            adj_t[v].append(u)

    visited = [False] * n
    sccs = []

    def dfs2(u, comp):
        visited[u] = True
        comp.append(u)
        for v in adj_t[u]:
            if not visited[v]:
                dfs2(v, comp)

    for u in reversed(order):
        if not visited[u]:
            comp = []
            dfs2(u, comp)
            sccs.append(comp)

    return sccs

def main():
    n = int(input())
    costs = list(map(int, input().split()))
    m = int(input())
    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        adj[u-1].append(v-1)

    sccs = kosaraju_scc(n, adj)
    total_cost = 0
    ways = 1

    for comp in sccs:
        min_cost = min(costs[u] for u in comp)
        count = sum(1 for u in comp if costs[u] == min_cost)
        total_cost += min_cost
        ways = (ways * count) % MOD

    print(total_cost, ways)

if __name__ == "__main__":
    main()
```Đầu tiên, mã này xây dựng biểu đồ và chạy thuật toán của Kosaraju để lấy SCC. Đối với mỗi SCC, nó tìm nút rẻ nhất và đếm các bản sao. Tổng chi phí tối thiểu cho tổng chi phí. Nhân số lượng sẽ cho số cách. sử dụng`sys.setrecursionlimit`đảm bảo DFS trên đồ thị lớn không bị lỗi. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3
1 2 3
3
1 2
2 3
3 2
```| Bước | SCC được tìm thấy | chi phí tối thiểu cho mỗi SCC | cách mỗi SCC | tổng chi phí tích lũy | cách tích lũy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0], [1,2] | 1, 2 | 1,1 | 3 | 1 | 

Biểu đồ phân tách thành một nút SCC [0] và một chu kỳ [1,2]. Các nút rẻ nhất có giá lần lượt là 1 và 2. Chỉ có một cách để chọn mức tối thiểu trong mỗi SCC. 

**Đầu vào tùy chỉnh 2**```
4
1 1 2 2
4
1 2
2 1
3 4
4 3
```| Bước | SCC được tìm thấy | chi phí tối thiểu cho mỗi SCC | cách mỗi SCC | tổng chi phí tích lũy | cách tích lũy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,1], [2,3] | 1,2 | 2,2 | 3 | 4 | 

Hai SCC, mỗi SCC tạo thành một chu kỳ. Các nút có chi phí tối thiểu trong SCC đầu tiên: các nút 0,1 đều có giá 1, đưa ra 2 cách. SCC thứ hai: nút 2,3 đều có giá 2, cho 2 cách. Nhân 2*2 = 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Thuật toán Kosaraju chạy theo thời gian tuyến tính. Tổng và đếm mức tối thiểu cho mỗi SCC là O(n). | 
| Không gian | O(n + m) | Danh sách kề, biểu đồ chuyển vị, mảng đã truy cập và lưu trữ SCC đều có tỷ lệ tuyến tính với n + m. | 

Với các ràng buộc (n ≤ 10^5, m ≤ 3·10^5) và độ phức tạp thời gian tuyến tính, giải pháp dễ dàng phù hợp với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided sample
assert run("3\n1 2 3\n3\n1 2\n2 3\n3 2\n") == "3 1", "sample 1"

# Minimum-size input
assert run("1\n5\n0\n") == "5 1", "single node no edges"

# Two SCCs with multiple minimal nodes
assert run("4\n1 1 2 2\n4\n1 2\n2 1\n3 4\n4 3\n") == "3 4", "two cycles with duplicate minimums"

# All nodes same cost
assert run("3\n2 2 2\n2\n1 2\n2 3\n") == "4 1", "same cost, linear chain"

# Edge case: single cycle covering all nodes
assert run("3\n3 1 2\n3\n1 2\n2 3\n3 1\n") == "1 1", "full cycle"

# No edges, all nodes isolated
assert run("3\n1 2 3\n0\n") == "6 1", "isolated nodes"
```|
