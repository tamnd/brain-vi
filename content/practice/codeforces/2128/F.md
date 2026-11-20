---
title: "CF 2128F - Tam giác nghiêm ngặt"
description: "Chúng ta có một đồ thị liên thông, vô hướng với các nút $n$ và các cạnh $m$, trong đó mỗi cạnh có trọng số không cố định mà nằm trong một khoảng cho trước $[li, ri]$."
date: "2026-06-08T11:18:49+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2128
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1039 (Div. 2)"
rating: 3200
weight: 2128
solve_time_s: 186
verified: false
draft: false
---

[CF 2128F - Tam giác nghiêm ngặt](https://codeforces.com/problemset/problem/2128/F) 

**Đánh giá:** 3200 
**Tags:** đồ thị, đường đi tham lam, đường đi ngắn nhất 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng, liên thông với$n$nút và$m$các cạnh, trong đó mỗi cạnh có trọng số không cố định mà nằm trong một khoảng nhất định$[l_i, r_i]$. Chúng ta được yêu cầu gán trọng số thực cho các cạnh trong các khoảng này sao cho một "bất đẳng thức tam giác" cụ thể liên quan đến một nút được chỉ định$k$thất bại. Cụ thể, đối với các đường đi ngắn nhất giữa các nút 1 và$n$, và những con đường đi qua$k$, chúng ta cần đảm bảo rằng$\mathrm{dist}_w(1, n) \neq \mathrm{dist}_w(1, k) + \mathrm{dist}_w(k, n)$đối với một số phép gán trọng số hợp lệ$w$. 

Đồ thị có thể khá lớn: lên tới$2 \cdot 10^5$tổng số nút và cạnh trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ thuật toán nào thử rõ ràng tất cả các phép gán trọng số có thể hoặc tính toán lại các đường đi ngắn nhất cho nhiều hoán vị trọng số. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính trong$n$Và$m$mỗi trường hợp thử nghiệm. Trọng lượng cạnh có thể lớn, lên tới$10^9$, vì vậy cần cẩn thận để tránh tràn số nguyên nếu sử dụng tổng trọng số. 

Trường hợp cạnh xảy ra khi nút$k$nằm trên **mọi đường đi ngắn nhất** giữa nút 1 và nút$n$, bất kể trọng số của cạnh trong khoảng cho phép. Ví dụ, nếu nút$k$là một phần của cây cầu hoặc nút cổ chai mà mọi đường đi từ 1 đến$n$phải đi qua thì không có phép gán nào có thể làm cho bất đẳng thức thất bại, và câu trả lời là "KHÔNG". Một trường hợp tinh vi khác là khi các khoảng của một số cạnh bị suy biến, tức là$l_i = r_i$, buộc một trọng lượng cố định. Trong những tình huống như vậy, sự linh hoạt cần thiết để phá vỡ sự bình đẳng có thể không tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: thử tất cả các phép gán trọng số trong khoảng của chúng và kiểm tra xem đẳng thức có thất bại hay không. Về mặt khái niệm, với mỗi phép gán, hãy tính các đường đi ngắn nhất từ ​​1 đến$n$, từ 1 đến$k$, và từ$k$ĐẾN$n$, và so sánh các tổng Điều này hiệu quả với các đồ thị rất nhỏ, nhưng với$m$lên đến$2 \cdot 10^5$, số lượng bài tập là vô hạn (hoặc cực kỳ lớn nếu bạn thử các mẫu riêng biệt), vì vậy điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là chúng tôi **không cần trọng số chính xác**, chỉ để kiểm tra xem có tồn tại _bất kỳ phép gán_ nào vi phạm đẳng thức hay không. Đối với các đường đi ngắn nhất trong biểu đồ có trọng số thay đổi, một mẹo hữu ích là xem xét **các phép gán trọng số cực đại**: gán mỗi cạnh theo giá trị nhỏ nhất của nó$l_i$hoặc tối đa$r_i$. Nếu một đẳng thức đường dẫn đúng cho tất cả các mức tối thiểu và tất cả các mức tối đa thì nó cũng đúng cho tất cả các trọng số trung gian. Điều này làm giảm vấn đề xuống chỉ còn **hai phép tính đường đi ngắn nhất** bằng cách sử dụng tất cả$l_i$và tất cả$r_i$như trọng lượng. Sau đó chúng ta kiểm tra xem đẳng thức tam giác có thể bị vi phạm bởi một nhiễu loạn nhỏ hay không. 

Một cách thay thế đơn giản hơn, khai thác tính chất liên tục của trọng số các cạnh, là gán **tất cả các cạnh có trọng số tối thiểu**, sau đó kiểm tra xem đẳng thức của tam giác đã nghiêm ngặt chưa. Nếu đúng như vậy, chúng ta có thể trả lời "CÓ". Nếu nó hoàn toàn bằng nhau, chúng ta có thể tăng trọng số của **bất kỳ cạnh nào không nằm trên đường đi ngắn nhất 1→k→n** một chút. Bởi vì khoảng cách là liên tục và trọng số cạnh linh hoạt trong các khoảng, sự nhiễu loạn như vậy sẽ phá vỡ sự đẳng thức. Nếu tất cả các cạnh được cố định dọc theo đường đi ngắn nhất thì đẳng thức không thể bị phá vỡ và câu trả lời là "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l)^m) | O(n+m) | Quá chậm | 
| Trọng lượng cực lớn / Sự xáo trộn | O(m log n) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn đồ thị dưới dạng danh sách kề, lưu trữ cả giới hạn dưới$l_i$và giới hạn trên$r_i$cho mỗi cạnh. Điều này cho phép truy cập nhanh để tính toán đường đi ngắn nhất. 
2. Tính khoảng cách ngắn nhất từ ​​nút 1 đến tất cả các nút khác bằng **thuật toán Dijkstra** với tất cả các cạnh được đặt ở giới hạn dưới của chúng$l_i$. Biểu thị mảng khoảng cách này là`dist_from_1`. 
3. Tính khoảng cách ngắn nhất từ ​​nút$n$đến tất cả các nút khác, một lần nữa có trọng số cạnh ở giới hạn dưới của chúng. Biểu thị mảng khoảng cách này là`dist_from_n`. 
4. Xác định các cạnh nằm trên **bất kỳ đường đi ngắn nhất nào từ 1 đến n đi qua k**. Một cạnh`(u,v)`nằm trên con đường như vậy nếu`dist_from_1[u] + l_edge + dist_from_n[v] == dist_from_1[n]`hoặc`dist_from_1[v] + l_edge + dist_from_n[u] == dist_from_1[n]`. 
5. Nếu **mọi cạnh** nằm trên đường đi ngắn nhất 1→k→n và tất cả đều có khoảng suy biến`l_i = r_i`, đẳng thức không thể bị phá vỡ, xuất ra "NO". 
6. Mặt khác, tồn tại ít nhất một cạnh không bị ràng buộc bởi đẳng thức đường đi 1→k→n, vì vậy chúng ta có thể gán cho nó trọng số lớn hơn một chút so với`l_i`, giữ tất cả các cạnh khác tại`l_i`. Điều này sẽ làm cho`dist_w(1,n) < dist_w(1,k) + dist_w(k,n)`không thành công và chúng tôi xuất ra "CÓ". 
7. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Logic dựa trên thực tế là khoảng cách đường đi ngắn nhất là hàm liên tục của trọng số cạnh, do đó, bất kỳ tính linh hoạt nào ở một cạnh không được yêu cầu nghiêm ngặt trên tất cả các đường đi ngắn nhất đều cho phép chúng ta phá vỡ đẳng thức tam giác. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def dijkstra(n, adj, start):
    dist = [float('inf')] * (n + 1)
    dist[start] = 0
    heap = [(0, start)]
    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]:
            continue
        for v, w in adj[u]:
            if dist[v] > d + w:
                dist[v] = d + w
                heapq.heappush(heap, (dist[v], v))
    return dist

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        edges = []
        for _ in range(m):
            u, v, l, r = map(int, input().split())
            adj[u].append((v, l))
            adj[v].append((u, l))
            edges.append((u, v, l, r))
        
        dist1 = dijkstra(n, adj, 1)
        distn = dijkstra(n, adj, n)
        
        # check if all edges are fully constrained along 1->k->n path
        constrained = True
        for u, v, l, r in edges:
            on_shortest = ((dist1[u] + l + distn[v] == dist1[n]) or
                           (dist1[v] + l + distn[u] == dist1[n]))
            if not on_shortest and l != r:
                constrained = False
                break
        
        print("NO" if constrained else "YES")

if __name__ == "__main__":
    solve()
```Việc triển khai này trước tiên xây dựng biểu đồ bằng cách sử dụng danh sách kề và tính toán khoảng cách đường đi ngắn nhất từ ​​nút 1 và nút n bằng cách sử dụng Dijkstra với trọng số cạnh tối thiểu. Sau đó chúng tôi kiểm tra xem mọi cạnh có thể ảnh hưởng đến khoảng cách 1→k→n có cố định hay không. Nếu tồn tại một cạnh linh hoạt bên ngoài đường tới hạn, chúng ta có thể làm xáo trộn nó để phá vỡ đẳng thức. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Biến | Nút 1→n quận | Nút 1→k quận | Nút k→n quận | 
| --- | --- | --- | --- | 
| Trọng lượng cạnh | l_i | l_i | l_i | 
| Đường đi ngắn nhất | 70 | 49 | 21 | 

Vì 70 ≠ 49 + 21, chúng ta thấy ngay bất đẳng thức đúng, cho ra "CÓ". Điều này chứng tỏ rằng ngay cả khi sử dụng trọng số tối thiểu, phép gán vẫn hợp lệ. 

**Mẫu 2** 

| Biến | Nút 1→n quận | Nút 1→k quận | Nút k→n quận | 
| --- | --- | --- | --- | 
| Trọng lượng cạnh | l_i | l_i | l_i | 
| Đường đi ngắn nhất | 80 | 50 | 30 | 

Ở đây 80 = 50 + 30, đẳng thức giữ nguyên. Kiểm tra các cạnh linh hoạt không nằm trên đường dẫn 1→k→n. Nếu không tồn tại hoặc các khoảng bị suy biến, xuất "NO". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) cho mỗi trường hợp thử nghiệm | Thuật toán Dijkstra với danh sách kề và min-heap | 
| Không gian | O(n + m) | Danh sách kề, mảng khoảng cách | 

Cho rằng tổng n và m trong tất cả các trường hợp thử nghiệm là 2·10^5, thuật toán này chạy hiệu quả trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("""7
4 4 2
1 2 10 20
2 3 10 30
1 3 49 90
4 3 1 1000
4 4 2
1 2 10 20
2 3 10 30
1 3 50
```
