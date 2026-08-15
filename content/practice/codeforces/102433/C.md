---
title: "CF 102433C - Tranh chấp màu sắc"
description: "Chúng ta có một đồ thị vô hướng liên thông có các đỉnh được đánh số từ 1 đến (N). Alice gán một trong hai màu cho mỗi cạnh. Sau khi nhìn thấy màu, Bob chọn đường đi từ đỉnh 1 đến đỉnh (N)."
date: "2026-08-14T15:34:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 107
verified: true
draft: false
---

[CF 102433C - Tranh chấp màu sắc](https://codeforces.com/problemset/problem/102433/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông có các đỉnh được đánh số từ 1 đến (N). Alice gán một trong hai màu cho mỗi cạnh. Sau khi nhìn thấy màu, Bob chọn đường đi từ đỉnh 1 đến đỉnh (N). Bất cứ khi nào hai cạnh liên tiếp trên tuyến đường của anh ta có màu khác nhau thì một lần thay đổi màu sẽ được tính. Bob muốn có ít thay đổi nhất có thể, trong khi Alice chọn màu sao cho mức tối thiểu đó càng lớn càng tốt. 

Đầu ra được yêu cầu là số lần thay đổi màu lớn nhất mà Alice có thể đảm bảo trên mọi tuyến đường từ 1 đến (N). 

Số lượng chìa khóa hóa ra đơn giản hơn nhiều so với trò chơi tô màu gợi ý. Gọi (d) là khoảng cách đường đi ngắn nhất thông thường từ đỉnh 1 đến đỉnh (N). Câu trả lời là chính xác (d-1). Hai mẫu chính thức có câu trả lời lần lượt là 0 và 3. 

Với tối đa (100000) đỉnh và (100000) cạnh, một thuật toán khám phá nhiều màu hoặc đường dẫn có thể có là hoàn toàn không khả thi. Ngay cả (O(N^2)) cũng có nghĩa là khoảng (10^{10}) thao tác trên đầu vào lớn nhất, vượt xa giới hạn cuộc thi một giây. Chúng ta cần truyền tải đồ thị mà công việc của nó về cơ bản tỷ lệ thuận với số đỉnh và cạnh, chẳng hạn như BFS trong (O(N+M)). 

Có một số trường hợp khó khăn có thể dễ dàng gây ra câu trả lời sai nếu mối quan hệ với khoảng cách đường đi ngắn nhất bị bỏ qua. Đầu tiên, cạnh trực tiếp từ 1 đến (N) ngay lập tức cho kết quả 0. Ví dụ:```
2 1
1 2
```Tuyến đường duy nhất có một cạnh, do đó không có cạnh nào liên tiếp và do đó không thể thay đổi màu sắc. Một công thức như (d) sẽ trả về sai 1, trong khi câu trả lời đúng là 0. 

Trường hợp thứ hai là đồ thị có đường đi ngắn nhất có độ dài bằng 2:```
3 2
1 2
2 3
```Alice có thể tô màu cạnh đầu tiên là màu đỏ và cạnh thứ hai là màu xanh lam, buộc phải thay đổi một lần. Câu trả lời là 1. Việc triển khai bất cẩn mà quên rằng số lượng thay đổi ít hơn một số cạnh trên tuyến sẽ trả về 2. 

Trường hợp thứ ba là khi có một tuyến đường ngắn tồn tại bên cạnh một tuyến đường dài hơn nhiều:```
4 4
1 4
1 2
2 3
3 4
```Bob chỉ cần lấy cạnh trực tiếp (1)-(4), vì vậy Alice không thể ép bất kỳ sự thay đổi màu nào. Câu trả lời là 0 mặc dù một tuyến đường khác có ba cạnh và có thể có hai lần thay đổi màu sắc. Alice phải đánh bại tuyến đường tốt nhất của Bob chứ không phải tối đa hóa những thay đổi trên một số tuyến đường cụ thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ liệt kê mọi màu đỏ và xanh có thể có của các cạnh (M). Có (2^M) màu như vậy. Đối với mỗi màu, mức tối ưu của Bob có thể được tính bằng cách mở rộng trạng thái để bao gồm đỉnh hiện tại và màu của cạnh trước đó, sau đó sử dụng thuật toán đường đi ngắn nhất 0-1 trong đó việc tiếp tục sử dụng cùng một màu có chi phí là 0 và chuyển đổi màu có chi phí là 1. Việc đó mất (O(N+M)) thời gian cho một lần tô màu, vì vậy tổng số là (O(2^M(N+M))). Tại (M=100000), điều này thực sự là không thể, ngay cả trước khi xem xét các hệ số không đổi. 

Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi quyết định mà Alice có thể đưa ra và sau đó giải quyết chính xác sự tối ưu hóa của Bob. Nó thất bại vì số lượng màu sắc là theo cấp số nhân. Quan sát hữu ích là chúng ta thực sự không cần phải xây dựng cách tô màu của Alice. Câu trả lời chỉ được xác định bằng khoảng cách đường đi ngắn nhất thông thường giữa đỉnh 1 và (N). 

Gọi khoảng cách đó là (d). Cho dù Alice tô màu đồ thị như thế nào, Bob vẫn có thể chọn đường đi ngắn nhất chứa chính xác (d) cạnh. Một chuỗi các cạnh (d) chỉ có (d-1) cặp liền kề, do đó nó có thể chứa nhiều nhất (d-1) sự thay đổi màu sắc. Do đó Alice không bao giờ có thể ép nhiều hơn (d-1). 

Phần thú vị là cho thấy Alice luôn có thể đạt được (d-1). Chạy BFS từ đỉnh 1 và đặt (dist[v]) là khoảng cách ngắn nhất từ ​​1 đến (v). Tô màu mọi cạnh ((u,v)) theo tính chẵn lẻ của cạnh nhỏ hơn của (dist[u]) và (dist[v]). Nói cách khác, một cạnh có điểm cuối nằm trên các mức BFS (k) và (k+1) sẽ có màu được xác định bởi tính chẵn lẻ của (k). Các cạnh có điểm cuối có cùng khoảng cách BFS cũng có thể được chỉ định bằng cách sử dụng cùng quy tắc khoảng cách tối thiểu đó. 

Hãy xem xét bất kỳ tuyến đường nào từ 1 đến (N). Vì (dist[1]=0) và (dist[N]=d), tuyến đường cuối cùng phải đạt BFS cấp 1, sau đó là cấp 2, v.v. cho đến cấp (d). Cạnh đầu tiên mà tuyến đạt đến cấp (k+1) từ bên dưới phải kết nối các cấp (k) và (k+1), do đó màu của nó được xác định bởi tính chẵn lẻ của (k). Các cạnh tiếp cận đầu tiên liên tiếp như vậy tương ứng với (k) và (k+1), do đó màu sắc của chúng khác nhau. Do đó, có ít nhất một sự thay đổi màu sắc giữa mỗi cặp cấp độ liên tiếp. 

Có (d) cấp độ để vượt qua sau cấp độ 0, tạo ra ít nhất (d-1) thay đổi trên mọi tuyến đường có thể. Kết hợp với giới hạn trên, mức tối ưu chính xác là (d-1). 

Điều này làm giảm toàn bộ trò chơi thành một BFS duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^M(N+M))) | (O(N+M)) | Quá chậm | 
| Tối ưu | (O(N+M)) | (O(N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị vô hướng. Mỗi cạnh đầu vào được lưu trữ theo cả hai hướng vì Bob có thể duyệt đồ thị theo một trong hai hướng. 
2. Chạy BFS bắt đầu từ đỉnh 1. BFS là đường truyền đúng vì mọi cạnh đều có chi phí bằng nhau, do đó, khi đạt đến đỉnh đầu tiên, khoảng cách được ghi của nó là số cạnh tối thiểu có thể có từ đỉnh 1. 
3. Tiếp tục BFS cho đến khi đạt đến đỉnh (N). Khoảng cách kết quả (dist[N]) là độ dài của tuyến đường ngắn nhất từ ​​1 đến (N). 
4. Đầu ra (dist[N]-1). Tuyến đường ngắn nhất chứa các cạnh (dist[N]) và do đó chỉ có các cặp liền kề (dist[N]-1) có thể xảy ra sự thay đổi màu sắc. 

Phép trừ cũng có giá trị ở khoảng cách nhỏ nhất có thể. Vì đồ thị được kết nối và (N\neq1), nên chúng ta có (dist[N]\ge1), nên câu trả lời không bao giờ phủ định. 

### Tại sao nó hoạt động 

Đặt (d=dist[N]). Đối với giới hạn trên, Bob luôn có thể chọn tuyến đường ngắn nhất có các cạnh (d) và không có tuyến đường nào có các cạnh (d) có thể chứa nhiều hơn (d-1) thay đổi màu sắc. Do đó Alice không thể ép nhiều hơn (d-1).

Đối với giới hạn dưới, hãy xem xét các lớp BFS (0,1,\ldots,d), trong đó lớp (k) chứa các đỉnh ở khoảng cách (k) tính từ đỉnh 1. Alice có thể tô màu một cạnh nối các lớp (k) và (k+1) theo tính chẵn lẻ của (k). Bất kỳ tuyến đường nào từ lớp 0 đến lớp (d) đều phải vượt qua từng ranh giới giữa các lớp liên tiếp. Màu sắc được gán cho điểm giao nhau đầu tiên của các ranh giới đó sẽ xen kẽ nhau, vì vậy mỗi cặp ranh giới liên tiếp đều đóng góp ít nhất một sự thay đổi màu sắc. Có (d-1) cặp ranh giới nên mọi tuyến đường đều có ít nhất (d-1) thay đổi. 

Giới hạn trên và giới hạn dưới khớp nhau, chứng tỏ câu trả lời là chính xác (dist[N]-1). 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    dist = [-1] * n
    dist[0] = 0

    q = deque([0])

    while q:
        u = q.popleft()

        if u == n - 1:
            break

        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    print(dist[n - 1] - 1)

if __name__ == "__main__":
    solve()
```Danh sách kề sử dụng một danh sách cho mỗi đỉnh. Đối với mỗi cạnh đầu vào vô hướng ((a,b)), mã sẽ chèn (b) vào danh sách của (a) và (a) vào danh sách của (b). Điều này cung cấp bộ lưu trữ (O(N+M)) thay vì bộ lưu trữ (O(N^2)) mà ma trận kề sẽ yêu cầu. 

các`dist`mảng bắt đầu bằng (-1), có nghĩa là một đỉnh chưa được ghé thăm. Đỉnh 1 được gán khoảng cách 0 trước khi được đưa vào hàng đợi. Bất cứ khi nào BFS phát hiện ra một hàng xóm chưa được thăm của (u), khoảng cách của nó sẽ trở thành`dist[u] + 1`. 

Việc thoát sớm khi (N) bị xóa khỏi hàng đợi là an toàn vì BFS xử lý các đỉnh theo thứ tự khoảng cách không giảm. Tại thời điểm đó, khoảng cách được ghi của (N) đã là khoảng cách đường đi ngắn nhất của nó. 

Không cần phải lưu trữ màu của Alice. Bằng chứng chỉ sử dụng thực tế là việc tô màu như vậy có thể được xây dựng từ các mức BFS, trong khi giá trị mà Alice có thể buộc hoàn toàn được xác định bởi khoảng cách đường đi ngắn nhất. 

Số nguyên Python không tràn ở đây và khoảng cách tối đa tối đa là (N-1). Do đó, phép trừ cuối cùng nằm trong khoảng từ 0 đến (99999). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị chứa cạnh trực tiếp từ đỉnh 1 đến đỉnh 3 nên BFS tới đích ngay lập tức. 

| Đỉnh hiện tại | Khoảng cách | Đỉnh mới được phát hiện | Khoảng cách mới | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 1 | 
| 1 | 0 | 2 | 1 | 

Khoảng cách ngắn nhất là (d=1), vì vậy câu trả lời là (d-1=0). Điều này thể hiện trường hợp ranh giới trong đó tuyến đường của Bob chỉ có một cạnh và không có cạnh nào liên tiếp có thể xảy ra sự thay đổi màu sắc. Đầu ra mẫu chính thức là 0. 

### Mẫu 2 

Đồ thị có hai cách đối xứng để đạt đến đỉnh 4 và sau đó có hai cách đối xứng để đạt đến đỉnh 7. 

| Đỉnh hiện tại | Khoảng cách | Đỉnh mới được phát hiện | 
| --- | --- | --- | 
| 1 | 0 | 2, 3 | 
| 2 | 1 | 4 | 
| 3 | 1 | 4 | 
| 4 | 2 | 5, 6 | 
| 5 | 3 | 7 | 
| 6 | 3 | 7 | 

BFS tìm thấy (dist[7]=4). Mỗi tuyến từ 1 đến 7 cần ít nhất bốn cạnh, trong khi Alice có thể tô màu các lớp BFS để mỗi tuyến có ít nhất ba thay đổi. Bob có thể chọn tuyến đường bốn cạnh ngắn nhất, vì vậy ba cũng là con đường tối đa có thể. Kết quả đầu ra của thuật toán (4-1=3), khớp với mẫu chính thức. 

Ví dụ này cũng cho thấy tại sao sự tồn tại của nhiều đường đi ngắn nhất không quan trọng. Màu sắc dựa trên lớp của Alice xử lý tất cả chúng cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+M)) | BFS truy cập mỗi đỉnh nhiều nhất một lần và kiểm tra mọi cạnh vô hướng với số lần không đổi. | 
| Không gian | (O(N+M)) | Danh sách kề lưu trữ (2M) mục nhập kề được định hướng, mảng khoảng cách và hàng đợi sử dụng không gian bổ sung (O(N)). | 

Với (N,M\le100000), thuật toán chỉ thực hiện công việc tuyến tính ở kích thước đầu vào. Điều này nằm trong thang đo dự kiến ​​cho bài toán đồ thị một giây, trong khi việc liệt kê các màu theo cấp số nhân là không thể ở các giới hạn này. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    graph = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    dist = [-1] * n
    dist[0] = 0
    q = deque([0])

    while q:
        u = q.popleft()

        if u == n - 1:
            break

        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    print(dist[n - 1] - 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """3 3
1 3
1 2
2 3
"""
) == "0", "sample 1"

# Provided sample 2
assert run(
    """7 8
1 2
1 3
2 4
3 4
4 5
4 6
5 7
6 7
"""
) == "3", "sample 2"

# Minimum-size graph: one edge, so zero color changes are possible.
assert run(
    """2 1
1 2
"""
) == "0", "minimum-size graph"

# Shortest path has length two, so exactly one change can be forced.
assert run(
    """3 2
1 2
2 3
"""
) == "1", "distance-two path"

# A longer route exists, but Bob has a direct edge and therefore avoids
# every color change.
assert run(
    """4 4
1 4
1 2
2 3
3 4
"""
) == "0", "direct edge beats longer route"

# A chain of length four has answer three.
assert run(
    """5 4
1 2
2 3
3 4
4 5
"""
) == "3", "off-by-one boundary"

# Maximum-size linear graph. The answer is 99999.
n = 100000
large_input = str(n) + " " + str(n - 1) + "\n"
large_input += "".join(f"{i} {i + 1}\n" for i in range(1, n))
assert run(large_input) == "99999", "maximum-size graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`| 0 | Kích thước biểu đồ tối thiểu và ranh giới không thay đổi | 
|`3 2 / 1 2 / 2 3`| 1 | Phép trừ chính xác một từ độ dài đường đi ngắn nhất | 
|`4 4 / 1 4 / 1 2 / 2 3 / 3 4`| 0 | Bob chọn con đường ngắn nhất thay vì con đường dài hơn | 
|`5 4 / 1 2 / 2 3 / 3 4 / 4 5`| 3 | Đường dẫn ngắn nhất dài hơn và xử lý từng cái một | 
| Chuỗi có (100000) đỉnh | 99999 | Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Cạnh trực tiếp từ 1 đến (N) là trường hợp giới hạn dưới quan trọng nhất. Vì```
2 1
1 2
```BFS chỉ định`dist[1] = 0`Và`dist[2] = 1`. Thuật toán in`1 - 1 = 0`. Alice không thể buộc thay đổi vì Bob sử dụng một cạnh duy nhất và một cạnh không có cạnh lân cận để so sánh màu của nó. 

Đối với đường đi ngắn nhất có đúng hai cạnh,```
3 2
1 2
2 3
```BFS tạo ra khoảng cách 0, 1 và 2. Thuật toán trả về (2-1=1). Alice có thể tô màu (1)-(2) đỏ và (2)-(3) xanh lam, vì vậy Bob phải đổi màu một lần. Chỉ có một cặp cạnh liền kề, điều này cũng chứng tỏ hai sự thay đổi là không thể. 

Đối với một biểu đồ chứa cả tuyến đường ngắn và dài,```
4 4
1 4
1 2
2 3
3 4
```BFS phát hiện đỉnh 4 ngay tại khoảng cách 1. Việc đồ thị cũng chứa tuyến đường (1\rightarrow2\rightarrow3\rightarrow4) không thành vấn đề. Bob đang giảm thiểu sự thay đổi màu sắc và luôn có thể chọn cạnh trực tiếp, vì vậy đảm bảo của Alice là 0. Đây chính xác là lý do tại sao số lượng đồ thị có liên quan là khoảng cách ngắn nhất, không phải là tuyến đường dài nhất hoặc số đỉnh trong đồ thị. 

Đối với một chuỗi thuần túy,```
5 4
1 2
2 3
3 4
4 5
```tuyến đường duy nhất chứa bốn cạnh. BFS gán đỉnh 5 khoảng cách 4 và câu trả lời là 3. Alice có thể xen kẽ các màu dọc theo bốn cạnh, tạo ra ba thay đổi. Vì không có tuyến đường nào có ít hơn bốn cạnh nên đường này đạt đến giới hạn trên. 

Đối với chuỗi có kích thước tối đa có (100000) đỉnh, khoảng cách ngắn nhất là (99999), vì vậy câu trả lời là (99998). Tổng quát hơn, một biểu đồ được kết nối với các đỉnh (100000) không thể có đường đi ngắn nhất dài hơn (99999) và việc triển khai BFS xử lý toàn bộ biểu đồ trong thời gian (O(N+M)) mà không cố gắng liệt kê các tuyến đường hoặc màu sắc.
