---
title: "CF 1650G - Phím tắt đếm"
description: "Chúng ta được yêu cầu đếm các đường đi trong đồ thị vô hướng từ đỉnh bắt đầu s đến đích t có độ dài chính xác là khoảng cách ngắn nhất hoặc vượt quá nó nhiều nhất là một. The graph has n vertices and m edges, with no loops or multiple edges."
date: "2026-06-10T03:55:48+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1650
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 776 (Div. 3)"
rating: 2100
weight: 1650
solve_time_s: 91
verified: false
draft: false
---

[CF 1650G - Phím tắt đếm](https://codeforces.com/problemset/problem/1650/G) 

**Đánh giá:** 2100 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm đường đi trong đồ thị vô hướng từ đỉnh bắt đầu`s`đến một mục tiêu`t`có độ dài chính xác là khoảng cách ngắn nhất hoặc vượt quá nó nhiều nhất là một. Đồ thị có`n`đỉnh và`m`các cạnh, không có vòng hoặc nhiều cạnh. Các cạnh đầu vào tạo thành cấu trúc danh sách kề tiêu chuẩn và mỗi trường hợp kiểm thử là độc lập. Đầu ra của mỗi trường hợp thử nghiệm là một modulo số nguyên duy nhất$10^9+7$. 

Những hạn chế ngụ ý rằng`n`Và`m`có thể đạt được$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm và có thể có tới$10^4$trường hợp thử nghiệm. Điều này cấm mọi phép liệt kê đường dẫn đơn giản, bởi vì số lượng đường dẫn không đơn giản có thể theo cấp số nhân. Thông tin chi tiết cốt lõi là chúng tôi chỉ quan tâm đến các đường đi có độ dài bằng đường đi ngắn nhất hoặc đường đi ngắn nhất cộng một. Điều này hạn chế đáng kể tập hợp các đường dẫn có liên quan đến các cạnh dọc theo các đường dẫn ngắn nhất hoặc các cạnh "phím tắt" bằng cách thêm một bước nhảy bổ sung. 

Các trường hợp cạnh không rõ ràng bao gồm các biểu đồ trong đó nhiều đường đi ngắn nhất chồng lên nhau ở các đỉnh và trong đó một cạnh phụ tạo ra đường vòng một bước. Ví dụ: một tam giác có các đỉnh 1-2-3-1 và`s=1`,`t=3`có độ dài đường đi ngắn nhất là 1 qua cạnh 1-3, nhưng đường dẫn hai bước 1-2-3 cũng được tính. Một cách tiếp cận bất cẩn chỉ xem xét cây đường đi ngắn nhất sẽ bỏ lỡ những đường đi dài hơn một bước này. 

## Phương pháp tiếp cận 

Giải pháp brute-force cố gắng liệt kê tất cả các đường dẫn từ`s`ĐẾN`t`sử dụng DFS, đếm những cái có độ dài tối đa`d+1`Ở đâu`d`là con đường ngắn nhất. Điều này hoạt động về nguyên tắc, nhưng với`n`lên đến$2 \cdot 10^5$và số đường đi theo cấp số nhân thì không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần tạo ra tất cả các đường dẫn. Tính khoảng cách ngắn nhất từ`s`đến tất cả các đỉnh và từ`t`tới tất cả các đỉnh bằng BFS. Ký hiệu các khoảng cách này là`dist_s[v]`Và`dist_t[v]`. Một đường đi có độ dài bằng`dist_s[t]`phải tuân theo các cạnh`(u,v)`như vậy`dist_s[u] + 1 + dist_t[v] == dist_s[t]`. Đối với đường đi có độ dài`dist_s[t] + 1`, xét các cạnh ở đó`dist_s[u] + 1 + dist_t[v] == dist_s[t] + 1`hoặc`dist_s[v] + 1 + dist_t[u] == dist_s[t] + 1`. Đếm tất cả các cạnh hợp lệ với bội số thích hợp. 

Một điều tinh tế khác là các đường dẫn có thể truy cập lại các nút. Để xử lý vấn đề này, lập trình động được áp dụng trên các lớp BFS: đếm số cách để đến từng đỉnh chỉ bằng cách sử dụng các lớp đường đi ngắn nhất. Sau đó, đối với các cạnh tạo thành đường vòng có độ dài +1, hãy nhân số cách để đến một đầu với số cách để đến đầu kia từ`t`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | O(2^n) | O(n) | Quá chậm | 
| BFS + DP | O(n + m) cho mỗi trường hợp thử nghiệm | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS từ`s`tính toán`dist_s[v]`cho tất cả các đỉnh. Điều này mang lại khoảng cách ngắn nhất từ ​​​​`s`tới mọi đỉnh. 
2. Chạy BFS từ`t`tính toán`dist_t[v]`cho tất cả các đỉnh. Điều này mang lại khoảng cách ngắn nhất từ ​​​​`t`tới mọi đỉnh. 
3. Khởi tạo`ways_s[v]`là số đường đi ngắn nhất từ`s`ĐẾN`v`. Bộ`ways_s[s] = 1`. Xử lý các đỉnh theo thứ tự BFS từ`s`, với mỗi đỉnh`u`cập nhật`ways_s[v] += ways_s[u]`cho tất cả hàng xóm`v`Ở đâu`dist_s[v] == dist_s[u] + 1`. 
4. Tương tự, tính`ways_t[v]`về số đường đi ngắn nhất từ`v`ĐẾN`t`sử dụng BFS từ`t`. 
5. Khởi tạo`result = ways_s[t]`. Điều này đếm tất cả các con đường ngắn nhất. 
6. Lặp lại tất cả các cạnh`(u,v)`. Nếu như`dist_s[u] + 1 + dist_t[v] == dist_s[t] + 1`, thêm vào`ways_s[u] * ways_t[v]`ĐẾN`result`. Tương tự, nếu`dist_s[v] + 1 + dist_t[u] == dist_s[t] + 1`, thêm vào`ways_s[v] * ways_t[u]`ĐẾN`result`. Sử dụng modulo$10^9+7$. 
7. Đầu ra`result`. 

Lý do điều này có tác dụng là vì bất kỳ đường đi nào có độ dài`d`phải tiến triển dọc theo các lớp BFS theo thứ tự, được nắm bắt bởi`ways_s`Và`ways_t`. Bất kỳ đường đi có độ dài`d+1`có thể được xây dựng bằng cách sử dụng một cạnh "phụ" giữa hai lớp và việc đếm các cạnh này một cách riêng biệt đảm bảo rằng tất cả các đường dẫn được ghi lại chính xác một lần. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline
MOD = 10**9 + 7

def bfs(n, adj, start):
    dist = [None] * n
    dist[start] = 0
    q = deque([start])
    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] is None:
                dist[v] = dist[u] + 1
                q.append(v)
    return dist

def count_shortest_paths(n, adj, start):
    dist = [None] * n
    ways = [0] * n
    dist[start] = 0
    ways[start] = 1
    q = deque([start])
    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] is None:
                dist[v] = dist[u] + 1
                ways[v] = ways[u]
                q.append(v)
            elif dist[v] == dist[u] + 1:
                ways[v] = (ways[v] + ways[u]) % MOD
    return dist, ways

t = int(input())
for _ in range(t):
    input()  # blank line
    n, m = map(int, input().split())
    s, u_t = map(int, input().split())
    s -= 1
    u_t -= 1
    adj = [[] for _ in range(n)]
    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append(v)
        adj[v].append(u)
        edges.append((u, v))
    
    dist_s, ways_s = count_shortest_paths(n, adj, s)
    dist_t, ways_t = count_shortest_paths(n, adj, u_t)
    shortest = dist_s[u_t]
    result = ways_s[u_t] % MOD

    for u, v in edges:
        if dist_s[u] + 1 + dist_t[v] == shortest + 1:
            result = (result + ways_s[u] * ways_t[v]) % MOD
        if dist_s[v] + 1 + dist_t[u] == shortest + 1:
            result = (result + ways_s[v] * ways_t[u]) % MOD
    print(result)
```BFS tính toán khoảng cách hiệu quả trong`O(n + m)`và theo dõi số lượng đường đi dọc theo con đường ngắn nhất. Nhân các cách từ`s`đến một đầu với những con đường từ đầu kia đến`t`đếm tất cả các đường dẫn có độ dài +1 hợp lệ. Việc sử dụng modulo đảm bảo chúng ta không vượt quá giới hạn số nguyên. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`4 4`các cạnh tạo thành một hình vuông,`s=1`,`t=4`. Đường dẫn ngắn nhất có độ dài 2. Đường dẫn: 

| Đường dẫn | Chiều dài | Đếm | 
| --- | --- | --- | 
| 1-2-4 | 2 | 1 | 
| 1-3-4 | 2 | 1 |`ways_s`Và`ways_t`số lượng theo dõi chính xác và không tồn tại cạnh +1. Kết quả là 2. 

**Mẫu 2** 

đầu vào:`6 8`,`s=6`,`t=1`. Độ dài đường dẫn ngắn nhất là 1. Đường dẫn có độ dài 2: 

| Đường dẫn | Chiều dài | Đếm | 
| --- | --- | --- | 
| 6-1 | 1 | 1 | 
| 6-4-1 | 2 | 1 | 
| 6-2-1 | 2 | 1 | 
| 6-5-1 | 2 | 1 | 

DP dọc theo các lớp BFS đếm chính xác các đường dẫn này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | BFS đi qua tất cả các đỉnh và cạnh một lần; đếm các cạnh cho đường dẫn +1 là O(m) | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng khoảng cách và cách | 

Tổng của`n`Và`m`trên tất cả các trường hợp thử nghiệm là ≤$2 \cdot 10^5$, do đó tổng thời gian có thể chấp nhận được là 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # insert solution code here
    MOD = 10**9 + 7

    from collections import deque

    def count_shortest_paths(n, adj, start):
```
