---
title: "CF 102680G - Xe đạp đua"
description: "Sự cố mô tả mạng lưới đường vô hướng được kết nối. Mỗi giao lộ là một đỉnh và mỗi con đường là một cạnh. Ba tay đua muốn chọn giao lộ xuất phát và giao lộ về đích."
date: "2026-08-03T03:58:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "G"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 198
verified: true
draft: false
---

[CF 102680G - Cuộc đua xe đạp](https://codeforces.com/problemset/problem/102680/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả mạng lưới đường vô hướng được kết nối. Mỗi giao lộ là một đỉnh và mỗi con đường là một cạnh. Ba tay đua muốn chọn giao lộ xuất phát và giao lộ về đích. Vì mọi tay đua luôn chọn con đường ngắn nhất có thể giữa hai điểm đó nên đường đua buộc phải là một trong những con đường ngắn nhất cho cặp được chọn. Mục tiêu là chọn cặp giao lộ có đường đi ngắn nhất càng dài càng tốt. 

Theo thuật ngữ đồ thị, chúng ta cần tìm khoảng cách đường đi ngắn nhất tối đa giữa hai đỉnh bất kỳ. Giá trị này là đường kính của đồ thị. Câu trả lời tính số đường đã đi chứ không phải số giao lộ đã ghé thăm. Một con đường có năm giao lộ có chiều dài bằng bốn vì nó sử dụng bốn con đường. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số lượng giao lộ và đường, theo sau là các kết nối đường không có hướng. Đồ thị được đảm bảo có tính kết nối nên mọi giao lộ đều có thể tiếp cận mọi giao lộ khác. 

Các giới hạn đủ nhỏ để cho phép các thuật toán xoay quanh bậc hai hoặc cao hơn một chút về số đỉnh. Vì tổng số giao điểm trong tất cả các trường hợp thử nghiệm nhiều nhất là 1000 và mỗi biểu đồ riêng lẻ có tối đa 500 giao điểm, nên việc chạy tìm kiếm theo chiều rộng từ mọi đỉnh là điều thực tế. Một giải pháp cố gắng liệt kê tất cả các cặp đỉnh và đường tìm kiếm một cách độc lập sẽ lặp lại rất nhiều công việc và trở nên tốn kém một cách không cần thiết. 

Có một số trường hợp khó khăn trong đó việc triển khai không chính xác có thể thất bại. Một đồ thị có một giao điểm và một vòng tự lặp không cần chuyển động giữa các điểm khác nhau, vì vậy câu trả lời là 0.```
1
1 1
1 1
```Đầu ra đúng là:```
0
```Giải pháp khởi tạo câu trả lời cho một hoặc giả sử mọi đường dẫn đều chứa ít nhất một đường sẽ tạo ra kết quả sai. 

Biểu đồ trong đó đường đi ngắn nhất dài nhất không phải là một chuỗi trông đơn giản là một lỗi phổ biến khác. Ví dụ:```
1
4 4
1 2
2 3
3 4
1 4
```Đầu ra đúng là:```
2
```Đường thẳng từ 1 đến 4 ngăn không cho đường kính trở thành ba. Một cách tiếp cận bất cẩn khi tìm kiếm quãng đường đi bộ dài nhất có thể thay vì con đường ngắn nhất dài nhất sẽ chọn sai con đường 1-2-3-4. 

Đường song song và đường vòng cũng cần được xử lý không có trường hợp đặc biệt. Họ không thay đổi khoảng cách ngắn nhất giữa hai nút giao thông vì mọi con đường đều có chi phí như nhau. Việc triển khai BFS đương nhiên sẽ bỏ qua các cạnh và vòng tự lặp lại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ xem xét từng cặp giao lộ. Đối với mỗi cặp, nó sẽ chạy thuật toán đường đi ngắn nhất để tìm khoảng cách của chúng, sau đó giữ giá trị lớn nhất được tìm thấy. Vì biểu đồ không có trọng số nên BFS là đủ cho mỗi truy vấn đường dẫn ngắn nhất. Với$n$các đỉnh thì có$O(n^2)$cặp và mỗi chi phí BFS$O(n+m)$, mang lại tổng độ phức tạp của$O(n^2(n+m))$. Với$n=500$, con số này đã là khoảng 125 triệu đơn vị xử lý cạnh đỉnh cho mỗi trường hợp thử nghiệm và nó lặp lại công việc có thể tránh được. 

Quan sát quan trọng là khoảng cách ngắn nhất từ ​​một giao lộ bắt đầu đến mọi giao lộ khác có thể được tính toán cùng một lúc. BFS khám phá đồ thị theo từng lớp, vì vậy sau một BFS, chúng ta biết khoảng cách từ một đỉnh đến tất cả các đỉnh khác. Vì biểu đồ không có trọng số nên không cần thực hiện tìm kiếm Dijkstra hoặc lặp lại theo cặp. 

Phương pháp tối ưu là chạy BFS một lần từ mọi giao lộ. Mỗi BFS cung cấp tất cả khoảng cách từ nguồn đó và giá trị lớn nhất trong số chúng là ứng cử viên cho đường kính. Việc chọn ứng cử viên tối đa trên tất cả các nguồn sẽ đưa ra câu trả lời vì mọi cặp có thể xuất hiện khi một trong các điểm cuối của nó được sử dụng làm nguồn BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2(n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O(n(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách lân cận cho mỗi giao lộ. Mỗi con đường được thêm vào theo cả hai hướng vì được phép di chuyển theo một trong hai hướng. 
2. Chạy BFS từ mọi giao lộ. Trong BFS, lưu trữ khoảng cách ngắn nhất từ ​​nguồn đã chọn đến mọi giao lộ khác. 
3. Sau khi mỗi BFS kết thúc, hãy kiểm tra khoảng cách lớn nhất đạt được từ nguồn đó và cập nhật mức tối đa toàn cầu. Giá trị này thể hiện tuyến đường ngắn nhất dài nhất bắt đầu từ giao lộ hiện tại. 
4. Sau khi tất cả các giao lộ đã được sử dụng làm nguồn BFS, hãy xuất ra khoảng cách lớn nhất được ghi lại. Mọi cặp bắt đầu và kết thúc có thể đều đã được xem xét thông qua một trong các lần chạy BFS này. 

Tại sao nó hoạt động: 

Bất biến đằng sau thuật toán là sau BFS từ một đỉnh, mọi khoảng cách được lưu trữ là số lượng đường ngắn nhất có thể từ đỉnh đó đến đích tương ứng. BFS có đặc tính này vì nó truy cập các đỉnh theo thứ tự khoảng cách tăng dần. Vì mỗi đỉnh được sử dụng làm nguồn nên mỗi cặp giao điểm đều có khoảng cách ngắn nhất được tính toán ít nhất một lần. Khoảng cách tối đa trong số tất cả các khoảng cách ngắn nhất này chính xác là đường kính đồ thị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def bfs(start, graph):
    n = len(graph)
    dist = [-1] * n
    dist[start] = 0
    queue = [start]
    head = 0

    while head < len(queue):
        u = queue[head]
        head += 1
        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                queue.append(v)

    return max(dist)

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        graph = [[] for _ in range(n)]

        for _ in range(m):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            graph[a].append(b)
            graph[b].append(a)

        diameter = 0
        for i in range(n):
            diameter = max(diameter, bfs(i, graph))

        ans.append(str(diameter))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ biểu đồ một cách hiệu quả vì mỗi con đường được xử lý một lần theo mỗi hướng. Hàm BFS sử dụng danh sách làm hàng đợi cùng với chỉ mục thay vì liên tục loại bỏ phần tử đầu tiên, tránh chi phí tuyến tính của các hoạt động như`pop(0)`. 

Mảng khoảng cách bắt đầu bằng`-1`, đại diện cho một giao lộ chưa đến được. Khi BFS tìm thấy một hàng xóm chưa được ghé thăm, nó sẽ gán khoảng cách của nút cha cộng thêm một. Bởi vì BFS mở rộng theo lớp nên khoảng cách được chỉ định đầu tiên luôn là khoảng cách ngắn nhất. 

Vòng ngoài chạy BFS từ mọi giao lộ. Khoảng cách lớn nhất được trả về bởi bất kỳ BFS nào là đường kính cuối cùng. Biểu đồ được đảm bảo được kết nối, do đó mọi BFS đều đạt đến tất cả các đỉnh và`max(dist)`luôn luôn hợp lệ. 

Số nguyên Python không bị tràn và kích thước hàng đợi tối đa bị giới hạn bởi số lượng giao điểm. Việc triển khai cũng tránh đệ quy, giúp loại bỏ mọi rủi ro về vấn đề độ sâu đệ quy. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 3
1 2
3 1
2 3
```Đồ thị là một hình tam giác. Mỗi cặp giao lộ đều có một con đường đi thẳng. 

| Nguồn BFS | Khoảng cách | Khoảng cách tối đa | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | [0, 1, 1] | 1 | 1 | 
| 2 | [1, 0, 1] | 1 | 1 | 
| 3 | [1, 1, 0] | 1 | 1 | 

Dấu vết cho thấy tại sao câu trả lời không phải là số lượng giao lộ trên tuyến đường. Tuyến đường ngắn nhất dài nhất sử dụng một con đường nên đường kính là 1. 

Đối với mẫu thứ hai:```
5 4
1 2
3 2
3 4
5 4
```Biểu đồ tạo thành một chuỗi. 

| Nguồn BFS | Khoảng cách | Khoảng cách tối đa | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | [0, 1, 2, 3, 4] | 4 | 4 | 
| 2 | [1, 0, 1, 2, 3] | 3 | 4 | 
| 3 | [2, 1, 0, 1, 2] | 2 | 4 | 
| 4 | [3, 2, 1, 0, 1] | 3 | 4 | 
| 5 | [4, 3, 2, 1, 0] | 4 | 4 | 

Các điểm cuối của chuỗi cho khoảng cách tối đa. Chạy BFS từ tất cả các đỉnh xác nhận rằng không có cặp nào khác có thể cách xa nhau hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n(n+m)) | Một BFS được thực hiện từ mỗi n giao điểm và mỗi BFS sẽ truy cập mọi đỉnh và đường. | 
| Không gian | O(n+m) | Danh sách kề lưu trữ biểu đồ và BFS lưu trữ khoảng cách và hàng đợi. | 

Với tối đa 500 giao lộ cho mỗi trường hợp thử nghiệm và tổng số tối đa 1000 giao lộ trong các thử nghiệm, phương pháp BFS lặp lại vẫn nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""2
3 3
1 2
3 1
2 3
5 4
1 2
3 2
3 4
5 4
""") == "1\n4\n", "sample cases"

assert run("""1
1 1
1 1
""") == "0\n", "single intersection"

assert run("""1
4 4
1 2
2 3
3 4
1 4
""") == "2\n", "cycle shortcut"

assert run("""1
5 4
1 2
2 3
3 4
4 5
""") == "4\n", "long chain"

assert run("""1
4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "1\n", "complete graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn có vòng tự lặp | 0 | Xử lý đồ thị mà không cần di chuyển | 
| Chu kỳ với cạnh phím tắt | 2 | Xác nhận các đường đi ngắn nhất được sử dụng thay vì đi bộ dài tùy ý | 
| Chuỗi năm nút | 4 | Kiểm tra cấu trúc khoảng cách tối đa | 
| Đồ thị hoàn chỉnh | 1 | Kiểm tra các biểu đồ dày đặc trong đó mọi cặp đều liền kề nhau | 

## Vỏ cạnh 

Một giao lộ duy nhất chỉ có một vòng lặp tự được xử lý chính xác vì BFS bắt đầu với khoảng cách bằng 0 và không bao giờ phát hiện ra một đỉnh xa hơn. Khoảng cách tối đa vẫn bằng 0, phù hợp với thực tế là không được đi đường nào.```
1
1 1
1 1
```Hàng đợi BFS chỉ chứa giao điểm bắt đầu. Vòng lặp tự quay lại một đỉnh đã được truy cập, do đó mảng khoảng cách vẫn giữ nguyên`[0]`. 

Biểu đồ chứa các phím tắt yêu cầu sử dụng khoảng cách ngắn nhất chứ không phải tuyến đường dài nhất có thể. Trong ví dụ:```
1
4 4
1 2
2 3
3 4
1 4
```BFS từ giao lộ 1 cho biết khoảng cách`[0,1,2,1]`. Tuyến đường qua 2 và 3 tồn tại, nhưng nó không phải là tuyến đường ngắn nhất tới 4 vì cạnh trực tiếp 1-4 ngắn hơn. Thuật toán ghi lại khoảng cách 2 là mức tối đa, điều này đúng. 

Đường song song và đường vòng không yêu cầu xử lý bổ sung. BFS chỉ quan tâm liệu hàng xóm đã được ghé thăm hay chưa. Nhiều cạnh giống hệt nhau vẫn tạo ra khoảng cách ngắn nhất giống nhau và các vòng tự lặp không thể giảm khoảng cách đã biết.
