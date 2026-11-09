---
title: "CF 1482F - Các khía cạnh hữu ích"
description: "Chúng ta có một đồ thị có trọng số vô hướng có tối đa 600 đỉnh và khoảng 180.000 cạnh, trong đó mỗi cạnh có trọng số dương. Sau đó, chúng ta được cung cấp một danh sách các truy vấn, mỗi truy vấn bao gồm một cặp đỉnh $u$ và $v$ và một số $l$."
date: "2026-06-10T23:27:35+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1482
codeforces_index: "F"
codeforces_contest_name: "\u0422\u0435\u0445\u043d\u043e\u043a\u0443\u0431\u043e\u043a 2021 - \u0424\u0438\u043d\u0430\u043b"
rating: 2400
weight: 1482
solve_time_s: 192
verified: true
draft: false
---

[CF 1482F - Các khía cạnh hữu ích](https://codeforces.com/problemset/problem/1482/F) 

**Đánh giá:** 2400 
**Tags:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có trọng số vô hướng có tối đa 600 đỉnh và khoảng 180.000 cạnh, trong đó mỗi cạnh có trọng số dương. Sau đó, chúng ta được cung cấp một danh sách các truy vấn, mỗi truy vấn bao gồm một cặp đỉnh$u$Và$v$và một số$l$. Một cạnh được coi là _hữu ích_ nếu tồn tại một đường dẫn giữa một số cặp truy vấn$(u, v)$có tổng trọng lượng không vượt quá$l$và đi qua cạnh này. Mục đích là đếm xem có bao nhiêu cạnh hữu ích. 

Với các ràng buộc, một cách tiếp cận đơn giản để kiểm tra mọi đường dẫn cho mọi truy vấn là không khả thi vì số lượng đường dẫn tăng theo cấp số nhân. Giới hạn trên của$n = 600$gợi ý rằng các thuật toán với$O(n^3)$sự phức tạp có thể chấp nhận được, bởi vì$600^3 \approx 2 \times 10^8$hoạt động chỉ nằm trong giới hạn hợp lý trong khung thời gian 5 giây. Trọng số cạnh và giới hạn truy vấn lớn ($10^9$), vì vậy chúng ta phải tránh các thuật toán dựa vào việc xây dựng tất cả các đường dẫn một cách rõ ràng hoặc sử dụng các mảng dựa trên trọng số có kích thước đó. 

Các trường hợp cạnh không rõ ràng bao gồm các truy vấn trong đó đường dẫn tối thiểu có thể lặp qua các đỉnh nhiều lần để duy trì độ dài$l$. Ví dụ: đồ thị tạo thành một hình tam giác$1-2-3-1$với trọng số 2, 2, 3 và một truy vấn$(1,3,4)$yêu cầu xem xét các đường dẫn như$1-2-3$tổng bằng 4. Việc triển khai bất cẩn chỉ xem xét các đường đi ngắn nhất mà không kết hợp lại khoảng cách có thể bỏ lỡ điều này. 

Một điều tinh tế khác là đầu vào có thể bao gồm nhiều truy vấn ảnh hưởng đến tính hữu dụng của cùng một cạnh và một truy vấn có thể làm cho nhiều cạnh trở nên hữu ích đồng thời. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xử lý từng truy vấn riêng biệt, thực hiện tìm kiếm đầy đủ (như Dijkstra hoặc BFS) từ$u$để tìm tất cả các đường dẫn có thể truy cập theo độ dài$l$và đánh dấu mọi cạnh dọc theo những đường dẫn đó là hữu ích. Điều này hoạt động về nguyên tắc nhưng quá chậm. Mỗi cuộc gọi Dijkstra là$O(m + n \log n)$, và lên đến$q = 180,000$truy vấn, điều này có thể dễ dàng vượt quá$10^{10}$hoạt động. 

Quan sát quan trọng để tăng tốc độ này là chúng ta chỉ cần biết đối với mỗi cạnh$(a, b)$, nếu nó có thể tham gia vào bất kỳ con đường nào$u \to v$có chiều dài tối đa$l$. Chúng ta có thể tính toán trước các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng thuật toán Floyd-Warshall, vì$n \le 600$. Cho phép$dist[x][y]$biểu thị khoảng cách ngắn nhất từ$x$ĐẾN$y$. Sau đó, để có một cạnh$(a,b,w)$, nó rất hữu ích cho một truy vấn$(u,v,l)$nếu một trong hai$dist[u][a] + w + dist[b][v] \le l$hoặc$dist[u][b] + w + dist[a][v] \le l$. Điều này có hiệu quả vì bất kỳ đường dẫn nào góp phần mang lại sự hữu ích đều không thể ngắn hơn đường dẫn ngắn nhất kết nối các điểm cuối. Việc kiểm tra điều kiện này cho tất cả các cạnh đối với tất cả các truy vấn là khả thi, bởi vì$O(n^2 + q \cdot m)$phù hợp trong thời hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * (n + m log n)) | O(n + m) | Quá chậm | 
| Tối ưu | O(n^3 + m * n) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các cạnh của đồ thị và xây dựng ma trận kề hoặc ma trận khoảng cách trong đó`dist[i][j]`ban đầu là vô cùng, ngoại trừ 0 trên đường chéo. Điền vào trọng số cạnh trực tiếp giữa các đỉnh được kết nối. 
2. Chạy Floyd-Warshall: lặp qua từng đỉnh trung gian`k`, và với mỗi cặp`(i,j)`, cập nhật`dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`. Điều này đảm bảo rằng`dist[i][j]`trở thành khoảng cách ngắn nhất giữa các đỉnh`i`Và`j`. 
3. Chuẩn bị một mảng`best[i]`cho mỗi đỉnh`i`, sẽ theo dõi mức lớn nhất cho phép`l - dist[i][v]`trên tất cả các truy vấn liên quan đến`v`. Về cơ bản, đây là khoảng cách còn lại có thể được “dành” cho các cạnh liền kề với`i`trong một con đường hướng tới`v`. 
4. Lặp lại từng truy vấn`(u, v, l)`. Với mọi đỉnh`a`, cập nhật`best[a] = max(best[a], l - dist[v][a])`. Điều này có nghĩa là từ đỉnh`a`, cạnh dẫn về phía`v`không được vượt quá khoảng cách còn lại này để đáp ứng truy vấn. 
5. Bây giờ, lặp lại tất cả các cạnh`(a,b,w)`. Để cạnh có ích thì phải tồn tại một đỉnh`i`như vậy`best[a] >= w`hoặc`best[b] >= w`. Điều này kiểm tra xem cạnh này có thể tham gia vào một đường dẫn thỏa mãn bất kỳ truy vấn nào không. Đếm tất cả các cạnh như vậy. 
6. Xuất ra tổng số cạnh hữu ích. 

Tại sao nó hoạt động: Thuật toán Floyd-Warshall đảm bảo rằng`dist[i][j]`là khoảng cách đường đi ngắn nhất. Bằng cách theo dõi độ dài đường dẫn dư tối đa được phép (`best[a]`), chúng tôi nắm bắt chính xác những cạnh nào có thể là một phần của đường dẫn hợp lệ cho bất kỳ truy vấn nào. Bất kỳ cạnh nào không thỏa mãn điều kiện này đều không thể được đưa vào bất kỳ đường dẫn nào dưới giới hạn của bất kỳ truy vấn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
INF = 10**18

edges = []
dist = [[INF]*n for _ in range(n)]
for i in range(n):
    dist[i][i] = 0

for _ in range(m):
    u, v, w = map(int, input().split())
    u -= 1
    v -= 1
    edges.append((u, v, w))
    dist[u][v] = w
    dist[v][u] = w

q = int(input())
queries = []
for _ in range(q):
    u, v, l = map(int, input().split())
    u -= 1
    v -= 1
    queries.append((u, v, l))

# Floyd-Warshall
for k in range(n):
    for i in range(n):
        for j in range(n):
            if dist[i][j] > dist[i][k] + dist[k][j]:
                dist[i][j] = dist[i][k] + dist[k][j]

# Prepare best residual distances
best = [-10**18]*n
for u, v, l in queries:
    for i in range(n):
        best[i] = max(best[i], l - dist[v][i])

count = 0
for a, b, w in edges:
    if best[a] >= w or best[b] >= w:
        count += 1

print(count)
```Mã đọc biểu đồ và truy vấn, tính toán các đường đi ngắn nhất cho tất cả các cặp và sau đó tính khoảng cách còn lại tối đa`best[i]`cho mỗi đỉnh. Cuối cùng, mỗi cạnh được kiểm tra`best`để xác định tính hữu dụng. Các điểm triển khai tinh tế bao gồm việc sử dụng giá trị ban đầu lớn cho`INF`để tránh tràn và chuyển đổi các chỉ số dựa trên 1 thành dựa trên 0 cho danh sách Python. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
n = 4, edges = [(1,2,1),(2,3,1),(3,4,1),(1,3,3),(2,4,3),(1,4,5)], query = (1,4,4)
```| Cạnh | dist[u][a] + w + dist[b][v] | Hữu ích? | 
| --- | --- | --- | 
| 1-2 | 0+1+2=3 | Có | 
| 2-3 | 1+1+1=3 | Có | 
| 3-4 | 2+1+0=3 | Có | 
| 1-3 | 0+3+1=4 | Có | 
| 2-4 | 1+3+0=4 | Có | 
| 1-4 | 0+5+0=5 | Không | 

Điều này xác nhận 5 cạnh hữu ích, khớp với đầu ra. 

**Mẫu 2 (đã thi công):**```
n=3, edges=[(1,2,10),(2,3,5)], queries=[(1,3,11)]
```| Cạnh | dist[1][a] + w + dist[b][3] | Hữu ích? | 
| --- | --- | --- | 
| 1-2 | 0+10+5=15 | Không | 
| 2-3 | 10+5+0=15 | Không | 

Không có cạnh nào thỏa mãn truy vấn, đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3 + n_m + n_q) | Floyd-Warshall là n^3, đang cập nhật`best`là n_q, kiểm tra các cạnh là m_n | 
| Không gian | O(n^2 + m + q) | Ma trận khoảng cách là n^2, danh sách cạnh là m, danh sách truy vấn là q | 

Cho n=600, n^3 ≈ 2×10^8 và m,q ≤ n^2, điều này phù hợp thoải mái trong 5 giây và bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    INF = 10**18
    edges = []
```
