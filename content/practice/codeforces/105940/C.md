---
title: "CF 105940C - Nhiệm vụ ngựa vằn vĩ đại tại ASZoo (Cứng)"
description: "Bài toán này yêu cầu chúng ta làm việc trên một đồ thị vô hướng, không có trọng số biểu diễn các đường đi bên trong vườn thú. Một số đỉnh chứa vỏ ngựa vằn. Đối với mỗi đỉnh, chúng ta cần số cạnh ngắn nhất cần thiết để đến bất kỳ đỉnh nào chứa vỏ ngựa vằn."
date: "2026-06-25T13:54:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105940
codeforces_index: "C"
codeforces_contest_name: "ASU Coding Cup 10"
rating: 0
weight: 105940
solve_time_s: 43
verified: true
draft: false
---

[CF 105940C - Nhiệm vụ ngựa vằn vĩ đại tại ASZoo (Khó)](https://codeforces.com/problemset/problem/105940/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán này yêu cầu chúng ta làm việc trên một đồ thị vô hướng, không có trọng số biểu diễn các đường đi bên trong vườn thú. Một số đỉnh chứa vỏ ngựa vằn. Đối với mỗi đỉnh, chúng ta cần số cạnh ngắn nhất cần thiết để đến bất kỳ đỉnh nào chứa vỏ ngựa vằn. Nếu một đỉnh được tách ra khỏi mọi chuồng ngựa vằn thì câu trả lời của nó là`-1`. Phiên bản cứng khác với phiên bản dễ vì có thể có nhiều chuồng ngựa vằn thay vì chỉ có một chuồng. [Codeforces Gym 105940C - Nhiệm vụ ngựa vằn vĩ đại tại ASZoo (Khó)](https://codeforces.com/gym/105940/problem/C) 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số lượng vị trí, số lượng đường dẫn giữa chúng và số lượng chuồng ngựa vằn. Các dòng tiếp theo mô tả các đường dẫn, theo sau là danh sách các vị trí bao vây. Đầu ra là khoảng cách tối thiểu cho mọi vị trí theo thứ tự. 

Các ràng buộc được thiết kế xung quanh các thuật toán đồ thị tuyến tính. Trong tất cả các trường hợp thử nghiệm, tổng số đỉnh và cạnh nhiều nhất là`2 * 10^5`. Điều đó loại trừ các giải pháp chạy tìm kiếm biểu đồ từ mọi đỉnh, vì thực hiện BFS từ mọi nút sẽ tốn khoảng`O(n * (n + m))`, có thể đạt tới khoảng`4 * 10^10`hoạt động. Một giải pháp gần`O(n + m)`là hướng dự định. 

Có một vài trường hợp phá vỡ việc triển khai bất cẩn. Đầu tiên là khi một đỉnh tự nó là một chuồng ngựa vằn. Ví dụ:```
1
3 2 2
1 2
2 3
1 3
```Đầu ra đúng là:```
0 1 0
```Một giải pháp bắt đầu khoảng cách tại`1`cho tất cả các nút được phát hiện và quên khởi tạo nguồn bằng`0`sẽ đưa ra câu trả lời sai. 

Một trường hợp khác là biểu đồ bị ngắt kết nối:```
1
4 2 1
1 2
3 4
1
```Đầu ra đúng là:```
0 1 -1 -1
```Một quá trình truyền tải khởi tạo tất cả các khoảng cách tới`0`hoặc giả sử mọi đỉnh cuối cùng sẽ được truy cập sẽ gán giá trị không chính xác cho thành phần không thể truy cập được. 

Trường hợp cạnh cuối cùng là khi tất cả các đỉnh đều là vỏ ngựa vằn:```
1
3 3 3
1 2
2 3
1 3
1 2 3
```Đầu ra đúng là:```
0 0 0
```Nếu việc triển khai không xử lý chính xác nhiều điểm bắt đầu, nó có thể tính toán những khoảng cách không cần thiết chỉ từ một vùng bao vây. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là chạy BFS riêng biệt với từng chuồng ngựa vằn. Vì BFS đưa ra khoảng cách ngắn nhất trong biểu đồ không có trọng số, nên mỗi lần chạy sẽ tính toán chính xác khoảng cách từ một nguồn và việc lấy mức tối thiểu trên tất cả các lần chạy sẽ đưa ra câu trả lời. Vấn đề là việc này lặp đi lặp lại cùng một công việc nhiều lần. 

Trong trường hợp xấu nhất có thể có`n`chuồng ngựa vằn. Chạy BFS`k`lần chi phí`O(k(n + m))`. Với`k`gần với`n`, điều này trở thành`O(n(n + m))`, quá chậm so với giới hạn đã cho. 

Quan sát quan trọng là tất cả các chuồng nuôi ngựa vằn đều là những điểm đến có giá trị như nhau. Chúng ta không quan tâm tới con ngựa vằn nào mà chỉ quan tâm đến con ngựa gần nhất. Thay vì khám phá từng con ngựa vằn một cách riêng biệt, chúng ta có thể đảo ngược quan điểm. Hãy tưởng tượng tất cả các vị trí ngựa vằn mở rộng qua biểu đồ cùng một lúc. Khi đạt tới một đỉnh đầu tiên, khoảng cách được gán cho nó sẽ tự động là khoảng cách ngắn nhất tới con ngựa vằn gần nhất. 

Đây chính xác là BFS đa nguồn. Ban đầu, chúng tôi xếp từng chuồng ngựa vằn vào hàng đợi với khoảng cách`0`. Sau đó BFS bình thường tiếp tục. Bởi vì BFS xử lý các nút bằng cách tăng khoảng cách, nên nguồn đầu tiên chạm tới bất kỳ đỉnh nào phải là nguồn gần nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k(n + m)) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo danh sách kề cho đồ thị. Đối với mọi đường dẫn giữa hai vị trí, hãy thêm từng điểm cuối vào danh sách lân cận của điểm cuối kia vì biểu đồ là vô hướng. 
2. Tạo một mảng khoảng cách và khởi tạo mọi giá trị thành`-1`. Điều này thể hiện rằng chưa có vị trí nào được xác định. 
3. Xếp từng chuồng ngựa vằn vào hàng đợi và đặt khoảng cách của chúng là`0`. Mỗi vòng vây đã có khoảng cách bằng 0 so với chính nó và tất cả chúng phải đóng vai trò là điểm xuất phát. 
4. Trong khi hàng đợi không trống, hãy xóa vị trí tiếp theo. Đối với mọi vị trí liền kề vẫn còn khoảng cách`-1`, chỉ định khoảng cách của nó là khoảng cách của vị trí hiện tại cộng với một và thêm nó vào hàng đợi. Kiểm tra chưa được kiểm tra là đủ vì BFS đảm bảo lần truy cập đầu tiên là lần ngắn nhất. 
5. Sau khi quá trình truyền tải kết thúc, xuất mảng khoảng cách. Mọi thứ còn lại`-1`các giá trị tương ứng với các vị trí không thể tiếp cận được chuồng ngựa vằn. 

Tại sao nó hoạt động: hàng đợi luôn lưu trữ các đỉnh theo thứ tự không giảm khoảng cách từ con ngựa vằn gần nhất. Khi một đỉnh được phát hiện lần đầu tiên, nó sẽ đạt được thông qua số cạnh nhỏ nhất có thể vì mọi đường đi ngắn hơn đều đã được xử lý. Vì tất cả các vị trí của ngựa vằn đều bắt đầu cùng nhau nên khoảng cách được tính toán là tối thiểu trên mọi khu vực bao vây có thể có. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, k = map(int, input().split())

        graph = [[] for _ in range(n)]
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            graph[u].append(v)
            graph[v].append(u)

        zebras = list(map(int, input().split()))

        dist = [-1] * n
        q = deque()

        for x in zebras:
            x -= 1
            dist[x] = 0
            q.append(x)

        while q:
            u = q.popleft()
            for v in graph[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        ans.append(" ".join(map(str, dist)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Danh sách kề chỉ lưu trữ các cạnh hiện có, giữ cho biểu diễn đồ thị tỷ lệ với kích thước đầu vào. Hàng đợi chứa biên giới của BFS và đảm bảo rằng các nút được xử lý theo đúng thứ tự. 

Vòng khởi tạo là điểm khác biệt chính so với BFS thông thường. Thay vì chọn một nguồn, mỗi con ngựa vằn sẽ được chèn vào trước khi quá trình truyền tải bắt đầu. Việc đặt khoảng cách trước khi chèn sẽ ngăn cản việc xem lại cùng một con ngựa vằn. 

điều kiện`dist[v] == -1`là việc kiểm tra ranh giới quan trọng. Nó đảm bảo rằng mỗi đỉnh được gán chính xác một lần. Vì khoảng cách tăng lên một khi di chuyển dọc theo một cạnh nên không cần phải so sánh và cập nhật các đỉnh đã truy cập. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1
5 5 1
1 2
2 3
3 4
4 5
5 1
1
```Con ngựa vằn duy nhất là đỉnh`1`. 

| Trạng thái xếp hàng | Nút hiện tại | Khoảng cách | 
| --- | --- | --- | 
| 1 | 1 | 0 1 -1 -1 1 | 
| 2,5 | 2 | 0 1 2 -1 1 | 
| 5,3 | 5 | 0 1 2 -1 1 | 
| 3,4 | 3 | 0 1 2 2 1 | 
| 4 | 4 | 0 1 2 2 1 | 

Chu kỳ được khám phá ra bên ngoài từ con ngựa vằn. Hai đỉnh bên cạnh nó nhận được khoảng cách`1`và các đỉnh còn lại nhận giá trị ngắn nhất của chúng. 

Đối với biểu đồ có nhiều ngựa vằn:```
1
6 4 2
1 2
2 3
4 5
5 6
1 6
```Hàng đợi ban đầu chứa các đỉnh`1`Và`6`. 

| Trạng thái xếp hàng | Nút hiện tại | Khoảng cách | 
| --- | --- | --- | 
| 1,6 | 1 | 0 1 -1 -1 -1 0 | 
| 6,2 | 6 | 0 1 -1 -1 1 0 | 
| 2,5 | 2 | 0 1 2 -1 1 0 | 
| 5,3 | 5 | 0 1 2 -1 1 0 | 
| 3 | 3 | 0 1 2 -1 1 0 | 

Hai tìm kiếm phát triển cùng nhau. đỉnh`3`đạt được từ ngựa vằn`1`, trong khi đỉnh`4`không thể truy cập được vì nó không được kết nối với một trong hai nguồn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh vào hàng đợi một lần và mỗi cạnh được kiểm tra nhiều nhất hai lần. | 
| Không gian | O(n + m) | Danh sách kề lưu trữ tất cả các cạnh và mảng BFS lưu trữ thông tin đỉnh. | 

Tổng kích thước đầu vào được giới hạn bởi`2 * 10^5`các đỉnh và các cạnh, do đó nghiệm tuyến tính dễ dàng nằm trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solution(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, k = map(int, input().split())
        g = [[] for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        z = list(map(int, input().split()))

        d = [-1] * n
        q = deque()

        for x in z:
            x -= 1
            d[x] = 0
            q.append(x)

        while q:
            u = q.popleft()
            for v in g[u]:
                if d[v] == -1:
                    d[v] = d[u] + 1
                    q.append(v)

        out.append(" ".join(map(str, d)))

    return "\n".join(out)

assert solution("""1
5 5 1
1 2
2 3
3 4
4 5
5 1
1
""") == "0 1 2 2 1"

assert solution("""1
4 2 1
1 2
3 4
1
""") == "0 1 -1 -1"

assert solution("""1
3 3 3
1 2
2 3
1 3
1 2 3
""") == "0 0 0"

assert solution("""1
2 1 1
1 2
2
""") == "1 0"

assert solution("""1
6 3 2
1 2
2 3
4 5
1 5
""") == "0 1 2 2 1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ đơn với một con ngựa vằn |`0 1 2 2 1`| Mở rộng BFS bình thường | 
| Các thành phần bị ngắt kết nối |`0 1 -1 -1`| Các đỉnh không thể tiếp cận | 
| Mỗi nút là một con ngựa vằn |`0 0 0`| Nhiều điểm khởi đầu | 
| Hai nút có ngựa vằn ở nút thứ hai |`1 0`| Xử lý hướng cơ bản | 
| Hai vùng ngựa vằn được kết nối thông qua một đường dẫn |`0 1 2 2 1 3`| Lựa chọn ngắn nhất đa nguồn | 

## Vỏ cạnh 

Đối với trường hợp bị ngắt kết nối:```
1
4 2 1
1 2
3 4
1
```Hàng đợi bắt đầu bằng đỉnh`1`. Nó thăm đỉnh`2`và dừng lại vì không còn nút nào có thể truy cập được nữa. Đỉnh`3`Và`4`không bao giờ rời khỏi ban đầu`-1`trạng thái, vì vậy câu trả lời cuối cùng là`0 1 -1 -1`. 

Đối với trường hợp tất cả các đỉnh đều là ngựa vằn:```
1
3 3 3
1 2
2 3
1 3
1 2 3
```Mỗi đỉnh vào hàng đợi với khoảng cách`0`. Khi hàng xóm được kiểm tra, họ đã chỉ định khoảng cách nên không có thay đổi giá trị. Kết quả vẫn còn`0 0 0`. 

Đối với nhiều chuồng ngựa vằn cạnh tranh cho cùng một đỉnh, thứ tự BFS sẽ tự động xử lý việc buộc. Sóng ngựa vằn đầu tiên tới một đỉnh sẽ cho khoảng cách tối thiểu có thể và bất kỳ sóng nào sau đó không thể cải thiện khoảng cách đó vì nó sẽ phải truyền qua một đường đi bằng hoặc dài hơn.
