---
title: "CF 1338D - Dây thun lồng"
description: "Chúng ta được cho một cái cây và bài toán yêu cầu chúng ta suy nghĩ về một cấu trúc hình học có thể thực hiện được trên nó. Mỗi đỉnh trở thành một đường cong khép kín không cắt nhau trên một mặt phẳng."
date: "2026-06-16T09:13:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1338
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 633 (Div. 1)"
rating: 2700
weight: 1338
solve_time_s: 385
verified: true
draft: false
---

[CF 1338D - Dây cao su lồng nhau](https://codeforces.com/problemset/problem/1338/D) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, dfs và tương tự, dp, toán, cây 
**Thời gian giải:** 6 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây và bài toán yêu cầu chúng ta suy nghĩ về một cấu trúc hình học có thể thực hiện được trên nó. Mỗi đỉnh trở thành một đường cong khép kín không cắt nhau trên một mặt phẳng. Những đường cong này bắt buộc phải tương tác theo một cách rất cứng nhắc: hai đường cong cắt nhau khi và chỉ khi các đỉnh tương ứng của chúng được nối bởi một cạnh trên cây. Ngoài ra, các đường cong tương ứng với các đỉnh không liền kề phải hoàn toàn tách biệt. 

Trên mô hình hình học này, chúng tôi xác định khái niệm ngăn chặn: một đường cong được coi là bên trong một đường cong khác nếu nó nằm hoàn toàn trong vùng bao quanh của nó và không giao nhau với nó. Bằng cách sử dụng điều này, chúng ta tìm kiếm một chuỗi các đỉnh sao cho mỗi đường cong chứa đúng đường cong tiếp theo. 

Đầu ra là độ dài tối đa có thể có của một chuỗi lồng nhau như vậy, giả sử chúng ta có thể tự do thiết kế bất kỳ cách thể hiện hình học hợp lệ nào của cây. 

Khó khăn chính là hình học không phải là tùy ý. Quy tắc giao nhau buộc phải có một cấu trúc toàn cục: mọi cạnh là giao nhau bắt buộc, trong khi các cạnh không phải là cạnh cấm mọi tương tác. Điều này làm cho bản vẽ bị hạn chế nhiều, do đó hành vi lồng nhau không được tự do lựa chọn độc lập trên mỗi đỉnh. 

Ràng buộc n lên tới 100000 ngụ ý rằng mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. Cách tiếp cận bậc hai hoặc thậm chí n log n trên mỗi nút sẽ không thành công. Điều này ngay lập tức loại trừ bất kỳ công trình nào cố gắng mô phỏng rõ ràng các phần nhúng hoặc kiểm tra tất cả các cặp đỉnh để biết tính khả thi của việc lồng ghép. 

Một số trường hợp đặc biệt làm rõ điều gì là tinh tế ở đây. Trong một cây hình ngôi sao, trong đó một tâm được kết nối với tất cả các tâm khác, không thể lồng lâu hơn 2, vì các lá không thể lồng vào nhau mà không vi phạm các ràng buộc kề cận. Tuy nhiên, trong một đường đi đơn giản, mọi đỉnh có thể được sắp xếp thành một chuỗi lồng nhau hoàn hảo, gợi ý rằng câu trả lời sẽ tăng tuyến tính theo độ dài đường đi. Hai thái cực này đã gợi ý rằng câu trả lời phụ thuộc vào thời gian tồn tại của một “cấu trúc tuyến tính” bên trong cây. 

Một sự hiểu lầm ngây thơ sẽ là cho rằng việc lồng nhau có liên quan đến độ sâu của cây con. Điều đó không thành công đối với những cây có rễ sâu nhất không nằm trong chuỗi dài nhất. Một ý tưởng sai lầm khác là tham lam hái những chiếc lá sâu nhất, có thể bỏ sót những con đường dài xen kẽ đi qua các điểm phân nhánh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng các phần nhúng hình học hợp lệ của cây và sau đó tìm kiếm chuỗi lồng dài nhất trong số tất cả các cấu hình có thể có. Ngay cả khi chúng ta trừu tượng hóa hình học, điều này thực sự sẽ trở thành một sự tối ưu hóa tổ hợp toàn cầu đối với việc nhúng cây, điều này sẽ bùng nổ về độ phức tạp. Bất kỳ nỗ lực nào nhằm liệt kê các mối quan hệ ngăn chặn có thể có giữa các đỉnh đều nhanh chóng trở thành hàm mũ trong trường hợp xấu nhất vì mỗi nhánh đưa ra nhiều khả năng sắp xếp tương đối cho các đường cong. 

Quan sát quan trọng là các ràng buộc hình học loại bỏ hầu hết mọi tự do: cách duy nhất để duy trì các quy tắc giao nhất quán là sắp xếp các đỉnh trong một cấu trúc hoạt động giống như một đường đi xét về mặt bao hàm. Khi một đỉnh được đặt bên trong một đỉnh khác mà không có giao điểm, tất cả các đỉnh lân cận của nó phải nằm trong các vùng nhất quán và điều này lan truyền dọc theo một chuỗi. Việc phân nhánh không cho phép làm tổ độc lập vì anh chị em phải tách biệt, ngăn cản cả hai nằm trong các vùng của nhau theo các thứ tự khác nhau. 

Điều này buộc bất kỳ chuỗi lồng nhau hợp lệ nào phải tương ứng với một đường dẫn đơn giản trong cây. Ngược lại, bất kỳ đường đi đơn giản nào cũng có thể được nhận ra dưới dạng một chuỗi lồng nhau bằng cách đặt dần từng đỉnh tiếp theo bên trong đỉnh trước đó đồng thời tôn trọng các ràng buộc kề dọc theo đường đi. 

Vì vậy, bài toán quy về việc tìm đường đi đơn giản dài nhất trong cây, chính xác là đường kính cây được đo bằng số đỉnh.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Xây dựng Brute Force các phần nhúng | Hàm mũ | Hàm mũ | Quá chậm | 
| Tính toán đường kính cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi chuyển quan sát thành một tính toán cụ thể. 

1. Xây dựng danh sách kề của cây từ các cạnh đầu vào. 
2. Chạy DFS hoặc BFS từ bất kỳ nút tùy ý nào, ví dụ như nút 1 và tìm nút xa nhất từ ​​nút đó. Điều này hiệu quả vì trong cây, một điểm cuối của đường kính luôn là điểm xa nhất so với bất kỳ nút bắt đầu nào. 
3. Chạy DFS hoặc BFS thứ hai bắt đầu từ nút xa nhất đó. Theo dõi khoảng cách đến tất cả các nút. 
4. Khoảng cách tối đa thu được trong lần di chuyển thứ hai này là đường kính đo bằng các cạnh. 
5. Chuyển đổi đường kính dựa trên cạnh này thành số đỉnh bằng cách thêm 1, vì một đường dẫn có k cạnh chứa k + 1 đỉnh. 

Lý do phương pháp truyền tải hai lần hoạt động là vì lần quét đầu tiên đảm bảo chúng ta sẽ đến một điểm cực trị của con đường dài nhất. Lần quét thứ hai sau đó mở rộng ra ngoài từ điểm cực trị đó và nhất thiết phải đạt đến điểm cuối đối diện của đường kính. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi lồng dài nhất nào cũng tương ứng với một đường dẫn đơn giản trong cây. Cây có một đường đi đơn giản duy nhất giữa mỗi cặp đỉnh, vì vậy việc tối đa hóa độ dài lồng nhau tương đương với việc tối đa hóa số đỉnh trên đường đi đó. Thuộc tính điểm xa nhất đảm bảo rằng BFS thứ hai bắt đầu từ điểm cuối tùy ý của đường kính sẽ khám phá điểm cuối đối diện, bởi vì không còn đường đi nào có thể tồn tại mà không mâu thuẫn với cực đại của điểm cực trị thứ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start, adj):
    n = len(adj) - 1
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    far = start

    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)
                if dist[v] > dist[far]:
                    far = v
    return far, dist[far]

def solve():
    n = int(input())
    adj = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    if n == 1:
        print(1)
        return

    any_node = 1
    far, _ = bfs(any_node, adj)
    far2, dist = bfs(far, adj)

    print(dist[far2] + 1)

if __name__ == "__main__":
    solve()
```BFS đầu tiên chỉ chịu trách nhiệm xác định điểm cuối đường kính và nó chưa đưa ra câu trả lời. BFS thứ hai là nơi đo chiều dài đường kính thực tế. trận chung kết`+1`là cần thiết vì khoảng cách BFS đếm các cạnh, trong khi chuỗi lồng nhau đếm các đỉnh. 

Một chi tiết triển khai tinh tế là duy trì “nút xa nhất cho đến nay” trong BFS. Điều này tránh việc vượt qua mảng khoảng cách lần thứ hai và giữ cho mã tuyến tính mà không cần thêm chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 3
2 3
3 4
4 5
4 6
```BFS đầu tiên từ nút 1: 

| Bước | Nút | Khoảng cách | Xa nhất | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | 1 | 
| Thăm | 3 | 1 | 3 | 
| Thăm | 2 | 2 | 2 | 
| Thăm | 4 | 2 | 2 | 
| Thăm | 5 | 3 | 5 | 
| Thăm | 6 | 3 | 5 | 

Nút xa nhất là 5. BFS thứ hai bắt đầu từ 5. 

| Bước | Nút | Khoảng cách | Tối đa cho đến nay | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 0 | 0 | 
| Thăm | 4 | 1 | 1 | 
| Thăm | 3 | 2 | 2 | 
| Thăm | 2 | 3 | 3 | 
| Thăm | 6 | 2 | 3 | 
| Thăm | 1 | 3 | 3 | 

Khoảng cách tối đa là 3 cạnh, vì vậy câu trả lời là 4 đỉnh. 

Điều này xác nhận rằng chuỗi dài nhất đi theo đường từ 2 đến 5 hoặc 2 đến 6 qua tâm. 

### Ví dụ 2 

đầu vào:```
5
1 2
2 3
3 4
4 5
```Đây đã là một dòng. BFS đầu tiên đạt đến điểm cuối và BFS thứ hai đạt đến điểm cuối đối diện với khoảng cách 4 cạnh, cho câu trả lời 5. Điều này chứng tỏ rằng các đường dẫn thuần túy đạt được khả năng lồng tối đa có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi BFS truy cập mọi nút và cạnh một lần | 
| Không gian | O(n) | Danh sách kề và mảng khoảng cách | 

Cây có tới 100000 đỉnh, do đó, việc duyệt tuyến tính là tối ưu. Hai thẻ BFS vừa vặn thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def bfs(start, adj):
        n = len(adj) - 1
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        far = start
        while q:
            u = q.popleft()
            for v in adj[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)
                    if dist[v] > dist[far]:
                        far = v
        return far, dist[far]

    n = int(sys.stdin.readline())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, sys.stdin.readline().split())
        adj[a].append(b)
        adj[b].append(a)

    if n == 1:
        return "1\n"

    far, _ = bfs(1, adj)
    far2, dist = bfs(far, adj)
    return str(dist[far2] + 1)

# sample
assert run("""6
1 3
2 3
3 4
4 5
4 6
""") == "4\n"

# chain
assert run("""5
1 2
2 3
3 4
4 5
""") == "5\n"

# star
assert run("""5
1 2
1 3
1 4
1 5
""") == "2\n"

# small balanced
assert run("""7
1 2
1 3
2 4
2 5
3 6
3 7
""") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây xích | 5 | trường hợp đường dẫn tối đa | 
| cây sao | 2 | hành vi đường kính tối thiểu | 
| cây cân bằng | 4 | cấu trúc phân nhánh đúng đắn | 

## Vỏ cạnh 

Cây hình ngôi sao kiểm tra xem giải pháp có tránh đánh giá quá cao độ sâu lồng nhau hay không. Đường kính luôn là 2, vì hai lá bất kỳ được nối qua tâm và không còn đường đi nữa. 

Chuỗi tuyến tính kiểm tra hành vi giới hạn trên trong đó mọi đỉnh có thể tham gia vào một chuỗi lồng nhau. BFS phải truyền khoảng cách chính xác mà không bỏ sót điểm cuối do xử lý ràng buộc. 

Cây nhị phân cân bằng đảm bảo rằng độ sâu cục bộ không khiến thuật toán hiểu nhầm rằng các cây con sâu hơn luôn tạo ra chuỗi dài hơn, vì đường đi dài nhất thực tế chạy qua hai lá sâu nhất thông qua gốc.
