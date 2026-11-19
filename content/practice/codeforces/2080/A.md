---
title: "CF 2080A - Khả năng kết nối mạnh mẽ trở lại"
description: "Cho một đồ thị có hướng có n đỉnh và m cạnh. Nhiệm vụ là xác định số cạnh tối thiểu cần thêm vào để đồ thị được liên thông chặt chẽ. Một đồ thị được liên thông chặt chẽ nếu có một đường đi có hướng từ mọi đỉnh đến mọi đỉnh khác."
date: "2026-06-08T06:25:39+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2080
codeforces_index: "A"
codeforces_contest_name: "XIX Open Olympiad in Informatics - Final Stage, Day 2 (Unrated, Online Mirror, IOI rules)"
rating: 3200
weight: 2080
solve_time_s: 56
verified: true
draft: false
---

[CF 2080A - Khả năng kết nối mạnh mẽ phản công](https://codeforces.com/problemset/problem/2080/A) 

**Đánh giá:** 3200 
**Tags:** *thuật toán đặc biệt, mang tính xây dựng, đồ thị 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng với`n`đỉnh và`m`các cạnh. Nhiệm vụ là xác định số cạnh tối thiểu cần thêm vào để làm cho đồ thị **liên thông mạnh**. Một đồ thị được liên thông chặt chẽ nếu có một đường đi có hướng từ mọi đỉnh đến mọi đỉnh khác. 

Đầu vào bao gồm`n`Và`m`, theo sau là`m`các cặp số nguyên biểu diễn các cạnh có hướng giữa các đỉnh. Đầu ra là một số nguyên duy nhất: số cạnh tối thiểu cần thiết để đạt được kết nối mạnh. 

Những ràng buộc ngụ ý rằng`n`có thể lên tới 10^5 và`m`lên đến 10^5. Điều này loại trừ các thuật toán có độ phức tạp bậc hai như kiểm tra liên tục khả năng tiếp cận từ mỗi đỉnh. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Các trường hợp cạnh bao gồm: 

- Đồ thị đã được kết nối mạnh mẽ. Ví dụ, một hình tam giác có các cạnh`1→2`,`2→3`,`3→1`. Câu trả lời nên là`0`. 
- Đồ thị có đỉnh cô lập. Ví dụ,`n=3`và không có cạnh. Mỗi đỉnh là thành phần liên thông mạnh của riêng nó, do đó phải thêm nhiều cạnh. 
- Đồ thị có một đỉnh. Câu trả lời luôn là`0`bất kể các cạnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét từng cặp đỉnh và kiểm tra xem liệu có tồn tại một đường dẫn giữa chúng hay không. Đối với đồ thị có`n`đỉnh, điều này có thể yêu cầu`O(n^3)`hoạt động sử dụng Floyd-Warshall hoặc BFS/DFS lặp lại từ mỗi đỉnh. Điều này là quá chậm đối với`n=10^5`. 

Thông tin chi tiết quan trọng đến từ **các thành phần được kết nối mạnh mẽ (SCC)**. Mỗi SCC được kết nối nội bộ mạnh mẽ. Biểu đồ của SCC tạo thành Biểu đồ chu kỳ có hướng (DAG). Để làm cho biểu đồ gốc được kết nối chặt chẽ, chúng ta chỉ cần kết nối các SCC trong DAG để nó trở thành một SCC duy nhất. 

Cho phép`sources`là số lượng SCC không có cạnh vào và`sinks`là số lượng SCC không có cạnh đi ra. Khi đó số cạnh tối thiểu cần thiết là`max(sources, sinks)`. Điều này hoạt động vì mỗi nguồn cần một cạnh vào, mỗi bồn cần một cạnh đi và DAG phải được kết nối theo một chu kỳ. 

Thuật toán tiêu chuẩn để tính toán SCC một cách hiệu quả là **Thuật toán Kosaraju** hoặc **Thuật toán Tarjan**, cả hai đều chạy trong`O(n + m)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Phân tích SCC + DAG | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh`n`và các cạnh`m`và xây dựng danh sách kề của đồ thị. 
2. Chạy thuật toán Kosaraju để tìm tất cả các thành phần liên thông mạnh. Điều này bao gồm một thẻ DFS để ghi lại thời gian hoàn thiện, chuyển vị biểu đồ và DFS theo thứ tự ngược lại của thời gian hoàn thiện. 
3. Xây dựng đồ thị thu gọn của SCC, trong đó mỗi SCC là một nút và tồn tại các cạnh nếu có bất kỳ cạnh nào giữa hai SCC trong đồ thị ban đầu. 
4. Đếm số lượng SCC không có cạnh vào (`sources`) và không có cạnh đi ra (`sinks`). 
5. Nếu số lượng SCC là 1 thì đồ thị đã được liên thông mạnh. Còn không thì trả lại`max(sources, sinks)`. 

**Tại sao nó hoạt động:** SCC phân chia biểu đồ thành các thành phần không thể chia nhỏ thêm. Việc kết nối các nguồn và bồn trong SCC DAG đảm bảo mọi thành phần đều có thể kết nối với nhau. Số lượng nguồn và bồn chứa tối đa đảm bảo rằng không có thành phần nào bị ngắt kết nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

def kosaraju_scc(n, adj):
    visited = [False] * n
    order = []

    def dfs(u):
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                dfs(v)
        order.append(u)

    for u in range(n):
        if not visited[u]:
            dfs(u)

    transpose = [[] for _ in range(n)]
    for u in range(n):
        for v in adj[u]:
            transpose[v].append(u)

    comp_id = [-1] * n
    def dfs_rev(u, label):
        comp_id[u] = label
        for v in transpose[u]:
            if comp_id[v] == -1:
                dfs_rev(v, label)

    label = 0
    for u in reversed(order):
        if comp_id[u] == -1:
            dfs_rev(u, label)
            label += 1

    return comp_id, label

def main():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        adj[u - 1].append(v - 1)

    comp_id, num_scc = kosaraju_scc(n, adj)
    if num_scc == 1:
        print(0)
        return

    in_deg = [0] * num_scc
    out_deg = [0] * num_scc
    for u in range(n):
        for v in adj[u]:
            if comp_id[u] != comp_id[v]:
                out_deg[comp_id[u]] += 1
                in_deg[comp_id[v]] += 1

    sources = sum(1 for deg in in_deg if deg == 0)
    sinks = sum(1 for deg in out_deg if deg == 0)
    print(max(sources, sinks))

if __name__ == "__main__":
    main()
```DFS đầu tiên tính toán thời gian hoàn thành, DFS thứ hai trên các nhãn đồ thị được chuyển đổi SCC. Đếm mức độ trong và ngoài cho mỗi SCC xác định nguồn và mức tiêu thụ. Hãy cẩn thận với đầu vào dựa trên 1; chúng tôi chuyển đổi nó thành lập chỉ mục dựa trên 0. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 2
1 2
2 3
```| Bước | đặt hàng | chuyển vị | comp_id | nguồn | bồn rửa | 
| --- | --- | --- | --- | --- | --- | 
| DFS | [0, 1, 2] | [[ ], [0], [1]] | [0,1,2] | 1 | 1 | 

Giải thích: Mỗi nút là SCC riêng. Nút 1 không có cạnh vào (nguồn), nút 3 không có cạnh ra (sink). Các cạnh tối thiểu = max(1, 1) = 1. 

**Đầu vào mẫu 2:**```
3 3
1 2
2 3
3 1
```| Bước | đặt hàng | chuyển vị | comp_id | nguồn | bồn rửa | 
| --- | --- | --- | --- | --- | --- | 
| DFS | [0,1,2] | [[2], [0], [1]] | [0,0,0] | 0 | 0 | 

Tất cả các nút đều nằm trong một SCC. Không cần cạnh. Đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Thuật toán Kosaraju duyệt tất cả các đỉnh và cạnh hai lần. | 
| Không gian | O(n + m) | Danh sách kề và mảng phụ cho lượt truy cập, thứ tự, comp_id. | 

Đối với n 10^5 và m ≤ 10^5, điều này phù hợp thoải mái trong 1-2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("3 2\n1 2\n2 3\n") == "1", "sample 1"
assert run("3 3\n1 2\n2 3\n3 1\n") == "0", "sample 2"

# custom cases
assert run("1 0\n") == "0", "single node"
assert run("4 0\n") == "4", "all isolated"
assert run("4 3\n1 2\n2 3\n3 4\n") == "1", "chain of nodes"
assert run("4 4\n1 2\n2 3\n3 4\n4 1\n") == "0", "already cyclic"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | Trường hợp đỉnh đơn | 
| 4 0 | 4 | Nhiều đỉnh bị cô lập | 
| chuỗi 4 3 | 1 | Các cạnh tối thiểu để kết nối chuỗi | 
| 4 4 chu kỳ | 0 | Đã kết nối mạnh mẽ | 

## Vỏ cạnh 

Đối với đồ thị có nhiều đỉnh cô lập,`n=4`,`m=0`, mỗi đỉnh là SCC của chính nó. Chúng ta có 4 nguồn và 4 bồn. Tối đa = 4, cung cấp chính xác số cạnh cần thiết để tạo thành một chu trình nối tất cả các đỉnh. 

Đối với một đỉnh đơn có vòng tự lặp,`n=1`,`m=1`, Kosaraju xác định 1 SCC. Cả nguồn và phần chìm đều bằng 0, trả về 0 như mong đợi. 

Giải pháp này xử lý chính xác tất cả các điều kiện biên và cấu trúc liên kết bất thường.
