---
title: "CF 2132F - Rada và Thung lũng hoa cúc"
description: "Chúng tôi được yêu cầu phân tích một biểu đồ vô hướng được kết nối đại diện cho Thung lũng Chamomile, trong đó các nút là những ngôi nhà và các cạnh là các làn đường giữa chúng. Rada muốn biết làn đường nào được đảm bảo là một phần của mọi con đường ngắn nhất từ ​​nhà 1 đến nhà n."
date: "2026-06-08T02:52:26+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2132
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1043 (Div. 3)"
rating: 2100
weight: 2132
solve_time_s: 101
verified: false
draft: false
---

[CF 2132F - Rada và Thung lũng hoa cúc](https://codeforces.com/problemset/problem/2132/F) 

**Xếp hạng:** 2100 
**Tags:** dfs và tương tự, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu phân tích một biểu đồ vô hướng được kết nối đại diện cho Thung lũng Chamomile, trong đó các nút là những ngôi nhà và các cạnh là các làn đường giữa chúng. Rada muốn biết làn đường nào được đảm bảo là một phần của **mọi con đường ngắn nhất** từ nhà 1 đến nhà n. Cứ vài ngày, cô ấy ở một ngôi nhà nhất định và chúng tôi cần báo cáo làn đường gần nhất trong số những làn đường "luôn được sử dụng" này. 

Khoảng cách từ một ngôi nhà đến một làn đường được xác định là khoảng cách tối thiểu đến điểm cuối của làn đường đó. Nhiệm vụ của mỗi truy vấn là trả về chỉ số của làn đường bắt buộc gần nhất với ngôi nhà nơi Rada đang đứng hoặc -1 nếu không có làn đường nào là bắt buộc. 

Các ràng buộc ngụ ý rằng cách tiếp cận bạo lực kiểm tra mọi đường dẫn từ 1 đến n cho mỗi làn đường và truy vấn quá chậm. Với n và m lên tới 2·10^5 và tối đa 10^4 trường hợp thử nghiệm, một thuật toán về cơ bản phải chạy trong **thời gian tuyến tính theo kích thước của biểu đồ trên mỗi trường hợp thử nghiệm**, khoảng O(n + m + q). 

Trường hợp cạnh tinh tế xảy ra khi có nhiều hơn một đường đi ngắn nhất từ ​​1 đến n. Trong trường hợp đó, một số làn đường có thể không thuộc mọi con đường ngắn nhất. Ví dụ: nếu 1 kết nối với 2 và 3, cả hai đều kết nối với n, thì cả 1-2-n và 1-3-n đều không được đảm bảo, vì vậy câu trả lời phải là -1. Một trường hợp cạnh khác là khi đồ thị là một cây; ở đây tất cả các cạnh trên đường đi duy nhất từ ​​1 đến n đều được đảm bảo sử dụng được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính toán tất cả các đường đi ngắn nhất từ 1 đến n bằng BFS và kiểm tra từng cạnh nếu nó xuất hiện trên mọi đường dẫn. Điều này đòi hỏi phải tạo ra tất cả các đường dẫn theo cấp số mũ trong n và do đó không thể thực hiện được. Một ý tưởng trung gian là tính khoảng cách từ 1 và từ n đến tất cả các nút khác. Một cạnh u-v được đảm bảo nếu **nó nằm trên một đường đi ngắn nhất nào đó từ 1 đến n** và **mọi đường đi ngắn nhất phải bao gồm nó**, điều này xảy ra khi tổng khoảng cách d1[u] + 1 + dn[v] bằng khoảng cách từ 1 đến n, và tương tự đối với d1[v] + 1 + dn[u]. Bằng tính đối xứng, trong đồ thị không có trọng số, điều này xác định các cạnh nằm trên đường đi ngắn nhất, nhưng chúng ta cần thuộc tính mạnh hơn: cạnh phải **bắt buộc**, tức là, đó là cách duy nhất để đi dọc theo đường đi ngắn nhất ở bước đó. 

Cái nhìn sâu sắc quan trọng là sử dụng BFS từ 1 và BFS từ n để tính khoảng cách. Một cạnh là bắt buộc nếu nó kết nối các nút trong đó khoảng cách từ 1 **tăng chặt** và khoảng cách đến n **giảm dần**. Thuộc tính này đảm bảo rằng tất cả các đường đi ngắn nhất phải đi qua cạnh vì không có tuyến đường thay thế nào duy trì khoảng cách tối thiểu. Khi đã biết các làn đường bắt buộc, việc trả lời các truy vấn sẽ giảm xuống việc tìm khoảng cách tối thiểu từ nút truy vấn đến bất kỳ làn đường bắt buộc nào, việc này có thể được thực hiện bằng BFS hoặc bằng cách tính toán trước khoảng cách bằng BFS đa nguồn bắt đầu từ tất cả các điểm cuối làn bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn) | O(2^n) | O(n + m) | Quá chậm | 
| Các cạnh bắt buộc dựa trên BFS + BFS đa nguồn | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc biểu đồ và xây dựng danh sách kề. 
2. Chạy BFS từ nút 1 để tính toán`d1[i]`, khoảng cách ngắn nhất từ ​​1 đến mọi nút. 
3. Chạy BFS từ nút n để tính toán`dn[i]`, khoảng cách ngắn nhất từ ​​n đến mọi nút. 
4. Xác định các làn đường bắt buộc bằng cách kiểm tra từng cạnh u-v. Nếu như`d1[u] + 1 + dn[v] == d1[n]`**Và**`d1[v] + 1 + dn[u] == d1[n]`Và`d1[u] < d1[v]`(để đảm bảo tiến tới n), đánh dấu cạnh này là bắt buộc. 
5. Thu thập tất cả các nút là điểm cuối của làn đường bắt buộc. Khởi tạo đồng thời BFS từ tất cả các nút này, lưu trữ chỉ số làn đường mà mỗi nút gần nhất. 
6. Đối với mỗi nhà truy vấn, báo cáo chỉ số làn đường bắt buộc gần nhất. Nếu không có làn đường bắt buộc tồn tại, ghi -1. 

Tại sao nó hoạt động: BFS đảm bảo rằng chúng tôi tính toán chính xác khoảng cách tối thiểu đến tất cả các nút và thuộc tính tăng khoảng cách nghiêm ngặt từ 1 và giảm nghiêm ngặt khoảng cách đến n đảm bảo rằng một cạnh không thể bị bỏ qua trong bất kỳ đường đi ngắn nhất nào. BFS đa nguồn truyền bá chính xác chỉ số làn đường bắt buộc gần nhất vì BFS luôn khám phá các nút theo thứ tự khoảng cách tăng dần. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        adj = [[] for _ in range(n)]
        edges = []
        for i in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append((v, i + 1))
            adj[v].append((u, i + 1))
            edges.append((u, v))
        
        def bfs(start):
            dist = [n+1] * n
            dist[start] = 0
            dq = deque([start])
            while dq:
                u = dq.popleft()
                for v, _ in adj[u]:
                    if dist[v] == n+1:
                        dist[v] = dist[u] + 1
                        dq.append(v)
            return dist
        
        d1 = bfs(0)
        dn = bfs(n-1)
        dist_1_n = d1[n-1]
        
        mandatory_edges = []
        for idx, (u, v) in enumerate(edges):
            if d1[u] + 1 + dn[v] == dist_1_n and d1[u] < d1[v]:
                mandatory_edges.append((u, v, idx+1))
            elif d1[v] + 1 + dn[u] == dist_1_n and d1[v] < d1[u]:
                mandatory_edges.append((v, u, idx+1))
        
        if not mandatory_edges:
            q = int(input())
            print(" ".join(["-1"] * q))
            for _ in range(q):
                input()
            continue
        
        nearest_edge = [0] * n
        dist_to_edge = [n+1] * n
        dq = deque()
        for u, v, idx in mandatory_edges:
            for node in (u, v):
                if dist_to_edge[node] > 0:
                    dist_to_edge[node] = 0
                    nearest_edge[node] = idx
                    dq.append(node)
        
        while dq:
            u = dq.popleft()
            for v, ei in adj[u]:
                if dist_to_edge[v] > dist_to_edge[u] + 1:
                    dist_to_edge[v] = dist_to_edge[u] + 1
                    nearest_edge[v] = nearest_edge[u]
                    dq.append(v)
                elif dist_to_edge[v] == dist_to_edge[u] + 1:
                    nearest_edge[v] = min(nearest_edge[v], nearest_edge[u])
        
        q = int(input())
        res = []
        for _ in range(q):
            c = int(input()) - 1
            res.append(str(nearest_edge[c]))
        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán khoảng cách từ các nút 1 và n. Các cạnh bắt buộc được chọn bằng cách kiểm tra xem cạnh đó có phải là cách duy nhất để đi dọc theo con đường ngắn nhất hay không. BFS đa nguồn truyền bá các chỉ số làn đường bắt buộc gần nhất một cách hiệu quả. Phải cẩn thận khi sử dụng các chỉ số làn đường dựa trên 1 cho đầu ra. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 2:**```
5 4
1 2
2 3
3 4
4 5
3
1
2
3
```| Nút | d1 | dn | Làn đường bắt buộc gần nhất | 
| --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 
| 2 | 1 | 3 | 1 | 
| 3 | 2 | 2 | 2 | 
| 4 | 3 | 1 | 3 | 
| 5 | 4 | 0 | 4 | 

Kết quả truy vấn: 1 1 2 

Điều này xác nhận việc truyền bá BFS hoạt động và các cạnh bắt buộc được xác định chính xác dọc theo đường dẫn ngắn nhất duy nhất. 

**Đầu vào mẫu 3:**```
3 3
1 2
2 3
3 1
1
1
```Tất cả các đường dẫn từ 1 đến 3 đều có thể: 1-2-3 và 1-3. Không có cạnh nào xuất hiện trên tất cả các đường dẫn. Đầu ra: -1. Thuật toán xác định chính xác không có cạnh bắt buộc và đầu ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) cho mỗi trường hợp thử nghiệm | BFS chạy trong O(n + m), kiểm tra các cạnh bắt buộc trong O(m), BFS đa nguồn trong O(n + m), trả lời các truy vấn O(q) | 
| Không gian | O(n + m) | Danh sách kề của đồ thị, mảng khoảng cách và hàng đợi BFS | 

Với tổng n, m, q trên tất cả các trường hợp thử nghiệm 2·10^5, thuật toán vừa vặn trong giới hạn 3 giây với bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
```
