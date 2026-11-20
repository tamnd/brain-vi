---
title: "CF 2172M - Khoảng cách tối đa tới cổng"
description: "Chúng tôi có một mạng lưới các thành phố được kết nối bằng đường bộ, trong đó mỗi con đường dài đúng một km. Mỗi thành phố sản xuất một loại nông sản và thành phố 1 là cảng trung tâm."
date: "2026-06-07T22:59:57+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2172
codeforces_index: "M"
codeforces_contest_name: "2025 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1300
weight: 2172
solve_time_s: 77
verified: true
draft: false
---

[CF 2172M - Khoảng cách tối đa tới cổng](https://codeforces.com/problemset/problem/2172/M) 

**Đánh giá:** 1300 
**Tags:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng lưới các thành phố được kết nối bằng đường bộ, trong đó mỗi con đường dài đúng một km. Mỗi thành phố sản xuất một loại sản phẩm nông nghiệp và thành phố 1 là cảng trung tâm. Mục tiêu là tìm ra khoảng cách trong trường hợp xấu nhất mà thành phố sản xuất sản phẩm đó phải di chuyển để đến cảng đối với từng loại sản phẩm. Nói cách khác, đối với loại sản phẩm$i$, chúng ta cần khoảng cách tối đa trong số những khoảng cách ngắn nhất từ ​​tất cả các thành phố sản xuất$i$tới thành phố 1. 

Đầu vào cho biết số lượng thành phố$n$, đường$m$và các loại sản phẩm$k$, theo sau là danh sách độ dài$n$chỉ ra sản phẩm của mỗi thành phố. Tiếp theo$m$dòng mô tả các cạnh hai chiều. Biểu đồ được đảm bảo được kết nối để mọi thành phố đều có thể tiếp cận thành phố 1. Đầu ra là một dòng duy nhất$k$số nguyên trong đó$i$-số nguyên thứ là khoảng cách tối đa cho sản phẩm$i$. 

Các ràng buộc cho thấy chúng ta có thể có tới$2 \times 10^5$thành phố và rìa. Một cách tiếp cận đơn giản để tính toán các đường đi ngắn nhất từ ​​mỗi thành phố đến cảng riêng lẻ sẽ mất$O(n + m)$mỗi thành phố, dẫn đến$O(n(n + m))$, quá chậm. Điều này ngụ ý rằng chúng ta cần một lần duyệt qua biểu đồ để tính toán khoảng cách một cách hiệu quả. Bởi vì tất cả các cạnh đều có độ dài bằng nhau nên tìm kiếm theo chiều rộng (BFS) là lý tưởng vì nó tạo ra các đường đi ngắn nhất trong đồ thị không có trọng số theo thời gian tuyến tính. 

Trường hợp khó phát hiện khi nhiều thành phố sản xuất cùng một sản phẩm, đặc biệt khi một trong số đó là thành phố 1. Ví dụ: nếu thành phố 1 sản xuất sản phẩm 3 thì khoảng cách tối đa cho sản phẩm đó phải là 0, ngay cả khi các thành phố khác cũng sản xuất sản phẩm đó. Một trường hợp khác xảy ra khi một loại sản phẩm chỉ được sản xuất bởi một thành phố; thuật toán của chúng tôi vẫn phải tính toán chính xác khoảng cách của nó. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực tính toán đường đi ngắn nhất từ mỗi thành phố sản xuất sản phẩm đến thành phố 1 bằng cách sử dụng BFS hoặc Dijkstra. Sau đó chúng tôi sẽ lấy khoảng cách tối đa cho mỗi sản phẩm. Điều này hoạt động vì BFS đảm bảo các đường dẫn ngắn nhất trong biểu đồ không có trọng số. Tuy nhiên, trong trường hợp xấu nhất khi tất cả$n$thành phố sản xuất cùng một sản phẩm, chúng tôi sẽ thực hiện$n$Truyền tải BFS, mỗi lần lấy$O(n + m)$, dẫn đến$O(n(n + m))$, vượt xa giới hạn có thể chấp nhận được đối với$n = 2 \times 10^5$. 

Cách tiếp cận tối ưu khai thác tính đối xứng của các đường đi ngắn nhất trong biểu đồ không có trọng số. Thay vì bắt đầu BFS từ mọi thành phố, chúng tôi bắt đầu một BFS duy nhất từ ​​thành phố cảng 1. BFS tính toán khoảng cách ngắn nhất từ ​​thành phố 1 đến tất cả các thành phố khác trong$O(n + m)$. Sau đó, chúng tôi có thể chỉ cần lặp lại trên tất cả các thành phố và đối với từng loại sản phẩm, theo dõi khoảng cách tối đa giữa các thành phố sản xuất sản phẩm đó. Điều này làm giảm công việc từ$O(n(n + m))$đến một$O(n + m)$BFS cộng với quét tuyến tính trên các thành phố, phù hợp thoải mái trong giới hạn thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n + m)) | O(n + m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào để đọc số thành phố$n$, đường$m$và các loại sản phẩm$k$. Đồng thời đọc danh sách các sản phẩm được sản xuất bởi mỗi thành phố. 
2. Xây dựng danh sách kề cho đồ thị sử dụng thông tin đường đi. Biểu diễn này cho phép truyền tải hiệu quả các hàng xóm trong BFS. 
3. Khởi tạo một mảng khoảng cách có kích thước$n + 1$với một giá trị trọng điểm (ví dụ:$-1$) để chỉ các thành phố chưa được viếng thăm. Đặt khoảng cách cho thành phố 1 thành 0. 
4. Thực hiện BFS bắt đầu từ thành phố 1. Sử dụng hàng đợi để xử lý các thành phố theo từng lớp. Đối với mỗi thành phố được xếp hàng đợi, hãy kiểm tra tất cả các thành phố lân cận; nếu một người hàng xóm không được ghé thăm, hãy đặt khoảng cách của nó bằng khoảng cách của thành phố hiện tại cộng với một và xếp hàng đó. 
5. Sau BFS, mảng khoảng cách chứa khoảng cách ngắn nhất từ ​​thành phố 1 đến mọi thành phố khác. 
6. Khởi tạo một mảng kết quả có kích thước$k + 1$với số không. Lặp lại tất cả các thành phố; đối với loại sản phẩm của thành phố, hãy cập nhật mục nhập kết quả tương ứng lên giá trị tối đa của giá trị hiện tại và khoảng cách của thành phố. 
7. In kết quả theo loại sản phẩm$1$ĐẾN$k$. 

Tại sao nó hoạt động: BFS đảm bảo rằng khi một thành phố được ghé thăm lần đầu tiên, chúng tôi đã tìm ra con đường ngắn nhất đến thành phố đó từ thành phố xuất phát. Theo dõi khoảng cách tối đa cho từng loại sản phẩm sẽ xác định chính xác thành phố xa nhất sản xuất sản phẩm đó. Vì tất cả các cạnh đều có trọng số bằng nhau nên BFS tính toán chính xác khoảng cách ngắn nhất, đảm bảo kết quả chính xác. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

n, m, k = map(int, input().split())
products = list(map(int, input().split()))

# Build adjacency list
graph = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    graph[u].append(v)
    graph[v].append(u)

# BFS from city 1
dist = [-1] * (n + 1)
dist[1] = 0
queue = deque([1])

while queue:
    u = queue.popleft()
    for v in graph[u]:
        if dist[v] == -1:
            dist[v] = dist[u] + 1
            queue.append(v)

# Track maximum distances per product type
res = [0] * (k + 1)
for i in range(n):
    p = products[i]
    res[p] = max(res[p], dist[i + 1])

print(" ".join(str(res[i]) for i in range(1, k + 1)))
```Danh sách lân cận cho phép$O(1)$tra cứu hàng xóm. BFS sử dụng hàng đợi để đảm bảo rằng các thành phố được ghé thăm theo thứ tự khoảng cách tăng dần từ cảng. Vòng lặp cuối cùng ánh xạ chính xác các thành phố theo loại sản phẩm của chúng và cập nhật khoảng cách tối đa được nhìn thấy. các`i + 1`lập chỉ mục chuyển đổi các vị trí mảng dựa trên số 0 thành số thành phố dựa trên một. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 3 2
2 1 1
1 2
3 1
3 2
```| Thành phố | Sản phẩm | Khoảng cách từ 1 | Tối đa cho mỗi sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 → tối đa(0,0)=0 | 
| 2 | 1 | 1 | 1 → tối đa(0,1)=1 | 
| 3 | 1 | 1 | 1 → tối đa(1,1)=1 | 

Đầu ra:```
1 0
```Giải thích: Sản phẩm 1 được sản xuất bởi các thành phố 2 và 3, cả hai đều ở khoảng cách 1, vì vậy tối đa là 1. Sản phẩm 2 chỉ là thành phố 1, khoảng cách 0. 

**Mẫu 2 (đã xây dựng)** 

đầu vào:```
5 4 3
1 2 3 1 2
1 2
2 3
3 4
4 5
```| Thành phố | Sản phẩm | Khoảng cách | Tối đa cho mỗi sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 → 0 | 
| 2 | 2 | 1 | 2 → 1 | 
| 3 | 3 | 2 | 3 → 2 | 
| 4 | 1 | 3 | 1 → 3 | 
| 5 | 2 | 4 | 2 → 4 | 

Đầu ra:```
3 4 2
```Dấu vết này xác nhận rằng thuật toán tính toán chính xác khoảng cách tối đa cho nhiều thành phố cho mỗi loại sản phẩm và xử lý việc đánh số thành phố không liên tiếp trong BFS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | BFS ghé thăm mỗi thành phố một lần và lặp lại mỗi cạnh một lần. Vòng lặp cuối cùng qua các thành phố là O(n). | 
| Không gian | O(n + m + k) | Danh sách kề có O(n + m). Mảng khoảng cách O(n). Mảng kết quả O(k). Xếp hàng nhiều nhất là O(n). | 

Với$n, m \le 2 \times 10^5$, điều này thoải mái trong thời gian chạy 1 giây và sử dụng bộ nhớ ít hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    products = list(map(int, input().split()))
    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)
    dist = [-1] * (n + 1)
    dist[1] = 0
    queue = deque([1])
    while queue:
        u = queue.popleft()
        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                queue.append(v)
    res = [0] * (k + 1)
    for i in range(n):
        p = products[i]
        res[p] = max(res[p], dist[i + 1])
    return " ".join(str(res[i]) for i in range(1, k + 1))

# Provided
```
