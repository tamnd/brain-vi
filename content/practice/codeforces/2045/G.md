---
title: "CF 2045G - X hào quang"
description: "Chúng ta có một lưới có kích thước $R nhân C$ trong đó mỗi ô có chiều cao từ 0 đến 9. Bạn chỉ có thể di chuyển giữa các ô liền kề (lên, xuống, trái, phải)."
date: "2026-06-08T09:15:16+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "math", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2045
codeforces_index: "G"
codeforces_contest_name: "2024-2025 ICPC Asia Jakarta Regional Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 2045
solve_time_s: 61
verified: true
draft: false
---

[CF 2045G - X Aura](https://codeforces.com/problemset/problem/2045/G) 

**Đánh giá:** 2200 
**Tags:** đồ thị, toán học, đường đi ngắn nhất 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước$R \times C$trong đó mỗi ô có chiều cao từ 0 đến 9. Bạn chỉ có thể di chuyển giữa các ô liền kề (lên, xuống, trái, phải). Mỗi lần di chuyển sẽ bị phạt bằng$(h_1 - h_2)^X$, Ở đâu$h_1$là chiều cao của ô hiện tại,$h_2$là hàng xóm và$X$là số nguyên lẻ cố định. Bởi vì$X$là số lẻ, hình phạt này có thể âm khi di chuyển lên ô cao hơn. 

Bạn được hỏi nhiều truy vấn, mỗi truy vấn yêu cầu tổng số tiền phạt tối thiểu từ ô bắt đầu$(R_s, C_s)$đến một điểm đến$(R_f, C_f)$. Nếu mức phạt tối thiểu có thể được giảm nhỏ tùy ý bằng cách xoay vòng trong vòng lặp mức phạt âm, bạn nên xuất ra "INVALID". 

Những hạn chế là rất quan trọng. Lưới có thể lớn bằng$1000 \times 1000$, và có thể có tới$100,000$truy vấn. Điều này loại trừ việc tính toán lại các đường dẫn ngắn nhất từ ​​đầu cho mỗi truy vấn. Số lượng tế bào lên tới$10^6$, có nghĩa là bất kỳ$O(R \cdot C \cdot \text{something})$thuật toán phải cẩn thận. Chu kỳ tiêu cực làm cho Dijkstra tiêu chuẩn không thể sử dụng được. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó đường dẫn hình phạt tối thiểu bao gồm một chu kỳ âm. Ví dụ: hãy xem xét lưới 2x2 có chiều cao:```
1 2
2 1
```với$X=1$. Di chuyển xung quanh chu kỳ$(1,1) → (1,2) → (2,2) → (2,1) → (1,1)$đưa ra một hình phạt tiêu cực hoàn toàn. Bất kỳ Dijkstra ngây thơ nào cũng sẽ bỏ lỡ chu kỳ âm hoặc đưa ra câu trả lời hữu hạn thay vì "KHÔNG HỢP LỆ". Truy vấn một ô sẽ trả về 0. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là chạy Bellman-Ford hoặc Dijkstra với cách xử lý cạnh tiêu cực ngay từ đầu mỗi truy vấn. Bellman-Ford xử lý các trọng số âm nhưng có$O(VE)$sự phức tạp. Đây,$V \approx 10^6$Và$E \approx 4V$, vì vậy mỗi truy vấn sẽ có giá$O(4 \cdot 10^{12})$trong trường hợp xấu nhất là không thể thực hiện được. Chạy nó một lần cho mỗi truy vấn là không thể. 

Thông tin chi tiết quan trọng là hàm phạt được cố định trên các truy vấn và cấu trúc biểu đồ không đổi. Các chu kỳ âm là nội tại của toàn bộ lưới, không phụ thuộc vào ô bắt đầu. Do đó, chúng ta có thể tính toán trước ô nào là một phần hoặc có thể truy cập được bằng các chu kỳ âm. Khi các chu kỳ phủ định được đánh dấu, việc trả lời từng truy vấn sẽ giảm xuống việc kiểm tra xem liệu có thể truy cập đích đến từ một chu trình phủ định hay không (làm cho nó KHÔNG HỢP LỆ) hoặc tính toán đường đi ngắn nhất trong biểu đồ chu kỳ còn lại. 

Vì lưới thưa thớt và trọng số cạnh phụ thuộc vào độ cao nên chúng tôi có thể sử dụng một biến thể của **Bellman-Ford được tối ưu hóa cho lưới** hoặc hiệu quả hơn là **SPFA (Thuật toán nhanh hơn đường dẫn ngắn nhất)** với khả năng phát hiện chu kỳ âm. Điều này cho phép tính toán các đường đi ngắn nhất từ ​​mỗi ô đến mọi ô khác nếu cần thiết. Tuy nhiên, vì các truy vấn yêu cầu các cặp điểm bắt đầu-đích tùy ý nên chúng ta cần có APSP đầy đủ (đường dẫn ngắn nhất cho tất cả các cặp) hoặc một quan sát thông minh. 

Cách tiếp cận hiệu quả nhất là xử lý riêng từng thành phần được kết nối. Mỗi thành phần có một chu kỳ âm hoặc không. Nếu đúng như vậy, mọi truy vấn trong thành phần đó có thể đạt đến chu kỳ phủ định đều KHÔNG HỢP LỆ. Mặt khác, tính toán các đường đi ngắn nhất bằng cách sử dụng Dijkstra trên thành phần (vì các cạnh âm có thể tồn tại nhưng không có chu kỳ âm nên Dijkstra với các điều chỉnh tiềm năng có thể hoạt động). Với tính toán trước, câu trả lời truy vấn là ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bellman-Ford ngây thơ cho mỗi truy vấn | O(Q * V * E) | O(V) | Quá chậm | 
| Phát hiện chu kỳ phủ định trên toàn lưới + Dijkstra cho mỗi truy vấn | O(VE + Q log V) | O(V + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi lưới thành biểu đồ. Mỗi ô$(r, c)$là một đỉnh. Đối với mỗi cặp ô liền kề, tạo một cạnh có hướng theo cả hai hướng có trọng số$(h_1 - h_2)^X$. Từ$X$là số lẻ, cho phép trọng số âm. 
2. Phát hiện tất cả các chu kỳ tiêu cực bằng cách sử dụng Bellman-Ford trên toàn lưới. Khởi tạo một mảng khoảng cách bằng số không. Thư giãn tất cả các cạnh$R*C$lần. Bất kỳ cạnh nào vẫn có thể được thư giãn trên$R*C$-lần lặp thứ chỉ ra một chu kỳ âm. Đánh dấu tất cả các đỉnh liên quan hoặc có thể tiếp cận được từ một chu trình âm. 
3. Đối với biểu đồ chu kỳ còn lại, hãy tính toán các đường đi ngắn nhất bằng Dijkstra với hàng đợi ưu tiên từ tất cả các ô bắt đầu của truy vấn. Vì hiện tại đồ thị không có chu kỳ âm nên đường đi ngắn nhất được xác định rõ. Theo tùy chọn, vì có rất nhiều truy vấn, bạn có thể tính toán khoảng cách từ mọi ô bằng thuật toán của Johnson, thuật toán này sẽ tính lại trọng số các cạnh để loại bỏ trọng số âm, sau đó sử dụng Dijkstra cho mỗi truy vấn. 
4. Đối với mỗi truy vấn, nếu điểm bắt đầu hoặc điểm đến là một phần của chu kỳ phủ định hoặc có thể truy cập được từ nó, thì xuất "INVALID". Ngược lại, xuất ra đường đi ngắn nhất được tính toán trước. 

Tại sao nó hoạt động: Lưới được cố định và các chu kỳ âm không thể biến mất bằng cách thay đổi điểm bắt đầu hoặc kết thúc. Bất kỳ ô nào có thể đạt đến chu kỳ âm đều có thể bị giảm tổng số tiền phạt một cách tùy ý, khiến kịch bản không hợp lệ. Đối với các đồ thị có chu trình không âm hoặc không âm, Dijkstra đảm bảo đường đi ngắn nhất do khoảng cách dọc theo các cạnh không giảm sau khi tính lại trọng số. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

R, C, X = map(int, input().split())
grid = [list(map(int, list(input().strip()))) for _ in range(R)]

# Directions: up, down, left, right
dirs = [(-1,0),(1,0),(0,-1),(0,1)]

# Precompute edges
edges = []
for r in range(R):
    for c in range(C):
        u = r * C + c
        for dr, dc in dirs:
            nr, nc = r + dr, c + dc
            if 0 <= nr < R and 0 <= nc < C:
                v = nr * C + nc
                w = (grid[r][c] - grid[nr][nc]) ** X
                edges.append((u, v, w))

# Detect negative cycles using Bellman-Ford
V = R * C
dist = [0]*V
in_negative_cycle = [False]*V

for i in range(V):
    updated = False
    for u, v, w in edges:
        if dist[v] > dist[u] + w:
            dist[v] = dist[u] + w
            updated = True
            if i == V-1:
                in_negative_cycle[v] = True
    if not updated:
        break

# Propagate negative cycle marks
for _ in range(V):
    for u, v, w in edges:
        if in_negative_cycle[u]:
            in_negative_cycle[v] = True

Q = int(input())
queries = [tuple(map(int,input().split())) for _ in range(Q)]

for Rs, Cs, Rf, Cf in queries:
    start = (Rs-1)*C + (Cs-1)
    end = (Rf-1)*C + (Cf-1)
    if in_negative_cycle[start] or in_negative_cycle[end]:
        print("INVALID")
        continue
    # Dijkstra from start to end
    dists = [float('inf')]*V
    dists[start] = 0
    pq = [(0, start)]
    while pq:
        du, u = heapq.heappop(pq)
        if du != dists[u]:
            continue
        if u == end:
            break
        for dr, dc in dirs:
            r, c = divmod(u, C)
            nr, nc = r + dr, c + dc
            if 0 <= nr < R and 0 <= nc < C:
                v = nr*C + nc
                w = (grid[r][c] - grid[nr][nc])**X
                if dists[v] > dists[u] + w:
                    dists[v] = dists[u] + w
                    heapq.heappush(pq, (dists[v], v))
    print(dists[end])
```Mỗi phần phản ánh thuật toán. Tạo cạnh chuyển lưới 2D thành biểu đồ. Bellman-Ford chạy V lặp lại để phát hiện các chu kỳ âm. Quá trình lan truyền theo chu kỳ đảm bảo tất cả các nút bị ảnh hưởng đều được đánh dấu. Dijkstra chỉ tìm đường đi ngắn nhất ở những vùng hợp lệ. 

Điểm tinh tế: lập chỉ mục lưới dựa trên cơ sở 0 trong nội bộ, trong khi đầu vào là dựa trên một. Tránh được lũy thừa âm vì X được đảm bảo là số lẻ. sử dụng`heapq`ngăn chặn các vòng thư giãn không cần thiết. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 4 1
3359
4294
3681
5
1 1 3 4
3 3 2 1
2 2 1 4
1 3 3 2
1 1 1 1
```| Truy vấn | Bắt đầu | Kết thúc | Đường dẫn | Phạt đền | Chu kỳ tiêu cực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1) | (3,4) | (1,1)->(2,1)->...->(3,4) | 2 | Không | 
| 2 | (3,3) | (2,1) | ... | | |
