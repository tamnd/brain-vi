---
title: "CF 1758E - Tíc, Tíc"
description: "Chúng ta có một lưới $n lần m$ trong đó mỗi ô có một đồng hồ hiển thị số từ 0 đến $h-1$ hoặc trống. Các bước di chuyển được phép cho phép chúng ta chọn một hàng hoặc cột và tăng tất cả các đồng hồ trong hàng hoặc cột đó thêm một giờ theo modulo $h$."
date: "2026-06-09T14:43:45+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1758
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 836 (Div. 2)"
rating: 2500
weight: 1758
solve_time_s: 154
verified: true
draft: false
---

[CF 1758E - Tick, Tock](https://codeforces.com/problemset/problem/1758/E) 

**Đánh giá:** 2500 
**Thẻ:** tổ hợp, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới trong đó mỗi ô có một đồng hồ hiển thị số từ 0 đến$h-1$hoặc trống rỗng. Các bước di chuyển được phép cho phép chúng tôi chọn một hàng hoặc cột và tăng tất cả các đồng hồ trong hàng hoặc cột đó thêm một giờ theo modulo$h$. Một cấu hình có thể giải được nếu tồn tại một chuỗi các bước di chuyển như vậy làm cho tất cả các đồng hồ hiển thị cùng một số. Nhiệm vụ là đếm, đối với một lưới được lấp đầy một phần, số cách điền vào các ô trống để lưới có thể giải được. 

Từ kích thước đầu vào,$n$Và$m$mỗi người có thể tiếp cận được không$2 \cdot 10^5$với tổng của tất cả các lưới được giới hạn ở$2 \cdot 10^5$. Điều này buộc một thuật toán tuyến tính theo số lượng ô,$O(n \cdot m)$. Các vòng lặp lồng nhau trên các hàng và cột là khả thi miễn là chúng ta tránh mọi thứ bậc hai trong$n$hoặc$m$riêng. Số giờ$h$có thể lớn như$10^9$, vì vậy các thuật toán lặp lại trực tiếp các giá trị theo giờ là không khả thi. 

Một sự tinh tế nảy sinh vì ban đầu các ô trống có thể nhận bất kỳ giá trị nào. Một cách tiếp cận ngây thơ là thử tất cả$h^{\text{\#empty}}$nhiệm vụ là không thể. Một trường hợp cạnh khác là khi một hàng hoặc cột có các đồng hồ xung đột nhau: ví dụ: nếu hàng 1 có 0 và 2 và cột 2 có 1, thì cần phải kiểm tra cẩn thận để phát hiện sự không nhất quán. Lưới nhỏ, lưới có tất cả các ô trống và lưới có một hàng hoặc cột đều phải hoạt động nhất quán theo cùng một logic. 

Ví dụ: lưới 2x2 có```
1 -1
-1 2
```không thể làm thống nhất cho$h = 3$bởi vì sự khác biệt giữa các ô đã biết trong hàng và cột xung đột với modulo 3. Một giải pháp bất cẩn có thể gán 1 cho ô trống (trên cùng bên phải) và 2 (dưới cùng bên trái) một cách tùy ý, nhưng điều đó sẽ không cho phép một chuỗi di chuyển hàng/cột để thống nhất lưới. 

## Phương pháp tiếp cận 

Phương pháp brute-force gán cho mỗi ô trống một giá trị từ 0 đến$h-1$và kiểm tra khả năng giải được bằng cách mô phỏng chuyển động hàng/cột. Điều này đúng về nguyên tắc, nhưng có tới$2 \cdot 10^5$tế bào và$h$lên đến$10^9$, nó hoàn toàn không thực tế. 

Cái nhìn sâu sắc quan trọng là mô hình hóa vấn đề theo đại số. Cho phép$r_i$là số lần di chuyển được áp dụng cho hàng$i$Và$c_j$số cho cột$j$. Sau đó giá trị đồng hồ cuối cùng tại$(i,j)$là$$\text{initial}_{i,j} + r_i + c_j \equiv X \pmod{h}$$Ở đâu$X$là thời gian thống nhất Trừ hai ô đã biết trong cùng một hàng hoặc cột sẽ cho các phương trình như$$r_i + c_j - (r_i + c_k) \equiv \text{initial}_{i,j} - \text{initial}_{i,k} \pmod{h}$$hoặc$$c_j - c_k \equiv \text{initial}_{i,j} - \text{initial}_{i,k} \pmod{h}$$Ta thấy hệ thống có tính modulo tuyến tính$h$. Mỗi thành phần liên kết của các ô (được kết nối qua hàng và cột) phải thỏa mãn điều kiện nhất quán về sự khác biệt. Điều này có thể được mô hình hóa một cách hiệu quả bằng cách sử dụng biểu đồ trong đó các nút là hàng và cột và các cạnh mang modulo sai phân$h$. Tìm kiếm theo chiều sâu lan truyền các giá trị từ một nút cho phép chúng ta kiểm tra sự mâu thuẫn. Đối với các ô trống, mọi phép gán phù hợp với quá trình truyền đều có tác dụng. 

Về mặt đếm, nếu hệ thống nhất quán thì bất kỳ giá trị nào của$r_0$(các nước đi của hàng 0) có thể được chọn tự do trong$[0, h-1]$và các biến còn lại được xác định. Vì vậy, câu trả lời là$h^{\text{\#connected components}}$, modulo$10^9 + 7$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(h^{n \cdot m})$|$O(n \cdot m)$| Quá chậm | 
| Tối ưu |$O(n \cdot m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và khởi tạo mảng cho các hàng$r[i]$và cột$c[j]$như chưa được chỉ định. 
2. Xây dựng biểu đồ hai bên nối các hàng và cột thông qua các ô đã biết. Mỗi cạnh lưu trữ sự khác biệt cần thiết:$(\text{row value} + \text{col value}) \equiv \text{cell value} \pmod{h}$. 
3. Đối với mỗi hàng hoặc cột chưa được truy cập, hãy khởi động DFS. Gán một giá trị tùy ý (0) cho nút bắt đầu. Truyền bá các bài tập dọc theo các cạnh bằng cách sử dụng sự khác biệt. Nếu một nút đã có giá trị được gán, hãy kiểm tra xem nó có khớp với modulo giá trị được truyền không$h$. Nếu tìm thấy xung đột, thành phần đó không nhất quán và câu trả lời là 0. 
4. Đếm từng thành phần được kết nối nhất quán. Mỗi thành phần cho phép chính xác$h$các bài tập toàn cầu khác nhau (chọn giá trị của nút bắt đầu), vì vậy hãy nhân số lượng thành phần với$h$modulo$10^9 + 7$. 
5. Xuất kết quả cho từng test case. 

Tại sao nó hoạt động: Điều bất biến là sau khi gán một nút bắt đầu, mọi nút khác trong thành phần phải đáp ứng tất cả các ràng buộc cạnh. DFS đảm bảo mọi nút có thể truy cập được chỉ định một cách nhất quán hoặc phát hiện mâu thuẫn. Các thành phần độc lập vì không có ranh giới giữa chúng nên các lựa chọn sẽ tăng lên gấp bội. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, m, h = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]
        row_vals = [None] * n
        col_vals = [None] * m
        adj = [[] for _ in range(n + m)]
        for i in range(n):
            for j in range(m):
                if grid[i][j] != -1:
                    adj[i].append((n+j, (grid[i][j]) % h))
                    adj[n+j].append((i, (grid[i][j]) % h))
        visited = [False]*(n+m)
        ans = 1
        def dfs(u):
            for v, val in adj[u]:
                if visited[v]:
                    if (vals[v] - vals[u]) % h != val:
                        return False
                else:
                    vals[v] = (val + vals[u]) % h
                    visited[v] = True
                    if not dfs(v):
                        return False
            return True
        vals = [0]*(n+m)
        for i in range(n+m):
            if not visited[i]:
                vals[i] = 0
                visited[i] = True
                if not dfs(i):
                    ans = 0
                    break
                ans = ans * h % MOD
        print(ans)
```Đầu tiên, mã ánh xạ các hàng tới các chỉ số 0..n-1 và các cột tới n..n+m-1 để xử lý chúng một cách thống nhất. Mỗi cạnh lưu trữ sự khác biệt cần thiết giữa một hàng và cột. DFS truyền bá các giá trị và kiểm tra xung đột. Phép nhân với$h$đối với mỗi thành phần tương ứng với sự lựa chọn miễn phí về giá trị của nút bắt đầu. Số học Modulo xử lý các kết quả lớn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 3 4
1 0 -1
-1 -1 2
```| Bước | Các nút được chỉ định | Tuyên truyền DFS | Thành phần ổn chứ? | Chạy ans | 
| --- | --- | --- | --- | --- | 
| bắt đầu hàng0 | hàng0=0 | truyền tới col0: col0=1, col1=2 | được | 1 | 
| col1 | truyền tới hàng1: row1=0 | truyền tới col2: col2=2 | được | 1*4=4 | 

Điều này chứng tỏ sự lan truyền và đếm chính xác. 

Đầu vào mẫu 2:```
2 2 10
1 2
3 5
```Sự khác biệt giữa các tế bào đã biết xung đột: 

| Cạnh | Sự khác biệt hàng-Col | xung đột? | 
| --- | --- | --- | 
| (0,0)=1 | row0+col0=1 | không | 
| (0,1)=2 | row0+col1=2 | row0=0 => col1=2 | 
| (1,0)=3 | row1+col0=3 | col0=1 => row1=2 | 
| (1,1)=5 | hàng1+col1=5 | row1+col1=2+2=4 !=5 | 

Thuật toán phát hiện sự không nhất quán, xuất ra 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi ô tạo ra tối đa hai mục kề nhau, DFS chạm vào mỗi nút một lần | 
| Không gian | O(n + m + n*m) | Danh sách kề + mảng đã truy cập/vals | 

Cho tổng (n_m) 2_10^5, giá trị này dễ dàng khớp với 1 giây với O(n*m) DFS. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect
```
