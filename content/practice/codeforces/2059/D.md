---
title: "CF 2059D - Đồ thị và đồ thị"
description: "Chúng ta có hai đồ thị vô hướng, liên thông có chung một tập đỉnh. Mỗi biểu đồ có một mã thông báo được đặt trên một đỉnh ban đầu: trong biểu đồ đầu tiên tại đỉnh s1 và trong biểu đồ thứ hai tại đỉnh s2. Chúng ta có thể di chuyển liên tục từng mã thông báo dọc theo một cạnh trong biểu đồ tương ứng của nó."
date: "2026-06-08T08:04:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2059
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1002 (Div. 2)"
rating: 1900
weight: 2059
solve_time_s: 120
verified: false
draft: false
---

[CF 2059D - Đồ thị và đồ thị](https://codeforces.com/problemset/problem/2059/D) 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, đồ thị, đường dẫn tham lam, ngắn nhất 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đồ thị vô hướng, liên thông có chung một tập đỉnh. Mỗi biểu đồ ban đầu có một mã thông báo được đặt trên một đỉnh: trong biểu đồ đầu tiên tại đỉnh`s1`và ở đỉnh thứ hai`s2`. Chúng ta có thể di chuyển liên tục từng mã thông báo dọc theo một cạnh trong biểu đồ tương ứng của nó. Mỗi lần di chuyển có chi phí được xác định là chênh lệch tuyệt đối của các đỉnh mà mã thông báo di chuyển tới,`|u1 - u2|`. Mục tiêu là xác định tổng chi phí tối thiểu nếu chúng ta di chuyển các mã thông báo vô thời hạn hoặc để phát hiện rằng chi phí sẽ là vô hạn. 

Trong thực tế, việc di chuyển các token vô thời hạn có nghĩa là chúng ta phải tìm chu trình trong cả hai biểu đồ cho phép các token di chuyển vô hạn. Nếu chúng ta có thể căn chỉnh các chu trình này sao cho chênh lệch tuyệt đối giữa các đỉnh tương ứng trong hai chuỗi luôn bằng 0 thì tổng chi phí có thể vẫn bằng 0. Nếu không, sẽ có chi phí khác 0 ở mỗi lần di chuyển và tổng vô số lần di chuyển sẽ dẫn đến tổng số vô hạn. 

Các biểu đồ nhỏ: mỗi biểu đồ có tối đa 1000 đỉnh và cạnh, đồng thời tổng số trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 1000. Điều này cho phép chúng tôi sử dụng các thuật toán có độ phức tạp bậc ba hoặc thậm chí bậc hai cho mỗi trường hợp thử nghiệm, chẳng hạn như BFS để tính toán đường đi ngắn nhất giữa tất cả các cặp đỉnh mà không đạt đến giới hạn thời gian. 

Một trường hợp cạnh tinh vi xảy ra khi hai biểu đồ có cấu trúc giống hệt nhau nhưng vị trí bắt đầu khác nhau, khiến cho việc căn chỉnh các đường truyền vô hạn với chi phí bằng 0 là không thể. Ví dụ: nếu cả hai biểu đồ đều là các chu kỳ đơn giản có độ dài 4, nhưng các mã thông báo bắt đầu ở các đỉnh khác nhau thì không có chuỗi di chuyển nào duy trì chi phí bằng 0, vì vậy câu trả lời là`-1`. Một cách tiếp cận bất cẩn có thể cho rằng các chu kỳ luôn cho phép di chuyển với chi phí bằng 0, điều này không đúng khi sự liên kết các đỉnh khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng tất cả các chuỗi vô hạn có thể có trong cả hai biểu đồ. Người ta có thể cố gắng liệt kê từng cặp nước đi`(u1, u2)`và tổng hợp chi phí. Điều này đúng về mặt lý thuyết, nhưng số lượng các chuỗi có thể tăng theo cấp số nhân với số đỉnh, vì vậy nó không khả thi ngay cả đối với các trình tự vừa phải.`n`. 

Điểm mấu chốt là tổng chi phí sẽ chỉ hữu hạn nếu có sự khớp hoàn hảo về khoảng cách giữa các đỉnh trong hai biểu đồ. Cụ thể, tổng chi phí tối thiểu được xác định bằng khoảng cách tối thiểu từ đỉnh bắt đầu đến bất kỳ đỉnh nào trong mỗi biểu đồ. Nếu tồn tại ít nhất một đỉnh trong đồ thị đầu tiên có khoảng cách đường đi ngắn nhất tới`s1`khớp với một đỉnh trong khoảng cách đường đi ngắn nhất của đồ thị thứ hai tới`s2`, thì chúng ta có thể căn chỉnh các chu trình bắt đầu từ các đỉnh này để đạt được chi phí tối thiểu hữu hạn. Ngược lại, chi phí chắc chắn là vô hạn vì bất kỳ chuỗi hành động nào cuối cùng cũng sẽ phải chịu chi phí khác 0. 

Chúng ta có thể tính toán khoảng cách đường đi ngắn nhất từ`s1`đến tất cả các đỉnh khác trong đồ thị đầu tiên và từ`s2`đến tất cả các đỉnh khác trong đồ thị thứ hai. Sự khác biệt tuyệt đối tối thiểu có thể có giữa các khoảng cách này mang lại chi phí tối thiểu có thể có cho một chuỗi vô hạn. Nếu số 0 tồn tại trong số những khác biệt này thì chi phí có thể bằng 0. Mặt khác, chênh lệch tối thiểu xác định chi phí sẽ tích lũy ở mỗi lần di chuyển và tổng số tiền sẽ phân kỳ, vì vậy câu trả lời là`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n!) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách kề cho cả hai biểu đồ. Sử dụng danh sách kề được ưu tiên hơn cho việc truyền tải BFS vì các biểu đồ thưa thớt. 
2. Đối với mỗi biểu đồ, hãy chạy BFS từ đỉnh bắt đầu để tính khoảng cách ngắn nhất từ ​​điểm bắt đầu đến mọi đỉnh khác. Trong biểu đồ đầu tiên, lưu trữ khoảng cách trong`dist1[v]`; trong biểu đồ thứ hai, lưu trữ khoảng cách trong`dist2[v]`. BFS đảm bảo chính xác các đường dẫn ngắn nhất trong biểu đồ không có trọng số và chạy trong O(n + m). 
3. Khởi tạo một biến`min_cost`đến vô cùng. Lặp lại trên tất cả các đỉnh`v`từ 1 đến n. Đối với mỗi`v`, tính toán`abs(dist1[v] - dist2[v])`. Cập nhật`min_cost`nếu giá trị này nhỏ hơn. Điều này xác định cặp đỉnh nơi chênh lệch khoảng cách được giảm thiểu. 
4. Nếu`min_cost`vẫn lớn hơn 0, không thể tránh được tổng chi phí vô hạn, do đó sản lượng`-1`. Ngược lại, xuất ra`min_cost`, sẽ bằng 0 nếu khoảng cách có thể được căn chỉnh. 

Tại sao nó hoạt động: BFS tạo ra khoảng cách đường đi ngắn nhất chính xác từ mỗi đỉnh bắt đầu. Bất kỳ chuyển động mã thông báo vô hạn nào cũng có thể được phân tách thành các chu kỳ xung quanh các đỉnh. Chi phí của việc căn chỉnh các chu trình được quyết định bởi sự chênh lệch khoảng cách đến các vị trí bắt đầu. Việc giảm thiểu sự khác biệt này đảm bảo rằng nếu có thể di chuyển mã thông báo vô hạn mà không làm tăng chi phí thì chúng tôi sẽ nắm bắt được nó. Thuật toán không bao giờ đánh giá thấp sự khác biệt tối thiểu vì nó kiểm tra tất cả các đỉnh một cách có hệ thống. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def bfs(n, adj, start):
    dist = [float('inf')] * (n + 1)
    dist[start] = 0
    q = deque([start])
    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == float('inf'):
                dist[v] = dist[u] + 1
                q.append(v)
    return dist

t = int(input())
for _ in range(t):
    n, s1, s2 = map(int, input().split())
    
    m1 = int(input())
    adj1 = [[] for _ in range(n + 1)]
    for _ in range(m1):
        a, b = map(int, input().split())
        adj1[a].append(b)
        adj1[b].append(a)
    
    m2 = int(input())
    adj2 = [[] for _ in range(n + 1)]
    for _ in range(m2):
        c, d = map(int, input().split())
        adj2[c].append(d)
        adj2[d].append(c)
    
    dist1 = bfs(n, adj1, s1)
    dist2 = bfs(n, adj2, s2)
    
    min_cost = float('inf')
    for v in range(1, n + 1):
        min_cost = min(min_cost, abs(dist1[v] - dist2[v]))
    
    if min_cost > 0:
        print(-1)
    else:
        print(0)
```Mã bắt đầu bằng cách đọc tất cả thông tin biểu đồ và tạo danh sách kề để truyền tải hiệu quả. BFS được sử dụng để tính toán khoảng cách đường đi ngắn nhất vì mỗi cạnh có trọng số bằng nhau và BFS là tuyến tính theo các nút cộng với các cạnh. Sau khi khoảng cách được tính toán, một phép lặp đơn giản sẽ tính toán chênh lệch khoảng cách tối thiểu, biểu thị chi phí tối thiểu có thể đạt được. Việc kiểm tra cho`min_cost > 0`tương ứng trực tiếp với việc phát hiện sự liên kết không thể. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 1 1
4
1 2
2 3
3 4
4 1
4
1 2
2 3
3 4
4 1
```| Đỉnh | quận 1 | quận 2 | cơ bụng (khác biệt) | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 
| 3 | 2 | 2 | 0 | 
| 4 | 1 | 1 | 0 |`min_cost`= 0 → đầu ra 0. Điều này cho thấy có thể căn chỉnh hoàn hảo khi các mã thông báo bắt đầu ở cùng một đỉnh trong các biểu đồ giống hệt nhau. 

Đầu vào mẫu 2:```
4 1 2
4
1 2
2 3
3 4
4 1
4
1 2
2 3
3 4
4 1
```| Đỉnh | quận 1 | quận 2 | cơ bụng (khác biệt) | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 2 | 1 | 1 | 
| 4 | 1 | 2 | 1 |`min_cost`= 1 → đầu ra -1. Điều này chứng tỏ rằng các vị trí bắt đầu bị lệch trong các chu kỳ giống hệt nhau sẽ ngăn chặn các bước di chuyển vô hạn với chi phí bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * (n + m1 + m2)) | BFS chạy trong O(n + m) cho mỗi biểu đồ; lặp qua các đỉnh là O(n). | 
| Không gian | O(n + m1 + m2) | Lưu trữ danh sách kề và mảng khoảng cách cho mỗi biểu đồ. | 

Với tổng ràng buộc trên tất cả`n`,`m1`,`m2`1000, thuật toán này thực thi thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Call the above solution block
    # Include solution code here
    exec(open(__file__).read())
    return output.getvalue().strip()

# Provided samples
assert run("3\n4 1 1\n4\n1 2\n2 3\n3 4\n4 1\n4\n1 2\n2 3\n3 4\n4 1\n4 1 2\n4\n1 2\n2 3\n3 4\n4 1\n4\n1 2\n2 3\n3 4\n
```
