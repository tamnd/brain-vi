---
title: "CF 102694A - Chu vi của cây"
description: "Cây được coi như một vật thể hình học trong đó đường kính là đường đi dài nhất giữa hai nút bất kỳ. Bài toán xác định một dạng chu vi khác thường: thay vì giá trị thực của pi, chúng ta sử dụng pi = 3."
date: "2026-08-01T23:22:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102694
codeforces_index: "A"
codeforces_contest_name: "AlgorithmsThread Tree Basics Contest"
rating: 0
weight: 102694
solve_time_s: 59
verified: true
draft: false
---

[CF 102694A - Chu vi của cây](https://codeforces.com/problemset/problem/102694/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cây được coi như một vật thể hình học trong đó đường kính là đường đi dài nhất giữa hai nút bất kỳ. Bài toán xác định một phiên bản bất thường của chu vi: thay vì giá trị thực của pi, chúng ta sử dụng pi = 3. Vì chu vi thường là đường kính nhân với pi nên câu trả lời bắt buộc là ba lần đường kính của cây. Đầu vào mô tả một cây vô hướng với`n`đỉnh và`n - 1`các cạnh và đầu ra là giá trị chu vi theo định nghĩa được sửa đổi này. 

Nhiệm vụ chính không phải là mô phỏng bất kỳ hình học nào. Đó là tìm khoảng cách dài nhất giữa hai đỉnh của cây. Ràng buộc cho phép tối đa`3 * 10^5`đỉnh, do đó cần có một thuật toán khám phá cây với số lần không đổi. Một cách tiếp cận thử mọi cặp đỉnh sẽ cần khoảng`n^2 / 2`cặp, trở thành xung quanh`4.5 * 10^10`kiểm tra đầu vào lớn nhất và vượt xa giới hạn một giây có thể hỗ trợ. 

Các trường hợp cạnh xuất phát từ cấu trúc của cây. Một đỉnh không có cạnh nên đường kính của nó bằng 0 và đáp án cũng phải bằng 0. Ví dụ:```
Input:
1

Output:
0
```Việc triển khai bất cẩn với giả định luôn có ít nhất một cạnh có thể thất bại trong khi xây dựng danh sách kề hoặc khi thực hiện tìm kiếm từ hàng xóm không tồn tại. 

Cây có hai đỉnh có đường kính bằng một chứ không phải hai. Ví dụ:```
Input:
2
1 2

Output:
3
```Đường đi dài nhất có một cạnh nên chu vi là`1 * 3`. Một lỗi phổ biến là đếm các đỉnh thay vì các cạnh và tạo ra`6`. 

Cây hình ngôi sao là một trường hợp hữu ích khác. Ví dụ:```
Input:
5
1 2
1 3
1 4
1 5

Output:
6
```Đường đi dài nhất đi từ lá này qua tâm đến lá khác, có hai cạnh. Việc triển khai chỉ kiểm tra phần tử con sâu nhất từ ​​một gốc tùy ý có thể trả về không chính xác một thay vì hai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ tính khoảng cách giữa mỗi cặp đỉnh và giữ mức tối đa. Đối với mỗi đỉnh bắt đầu, chúng ta có thể chạy DFS hoặc BFS để tìm khoảng cách đến mọi đỉnh khác. Điều này đúng vì đường kính chính xác là khoảng cách tối đa giữa tất cả các cặp. Tuy nhiên, thực hiện việc này từ mọi đỉnh sẽ tốn`O(n^2)`thời gian. Với`n = 300000`, điều này sẽ đòi hỏi hàng chục tỷ hoạt động. 

Đặc tính hữu ích của cây là đường kính có thể được tìm thấy chỉ bằng hai lần duyệt. Chọn bất kỳ đỉnh bắt đầu nào và tìm đỉnh xa nhất từ ​​nó. Đỉnh đó được đảm bảo là một điểm cuối của đường kính. Sau đó bắt đầu một quá trình truyền tải khác từ điểm cuối đó. Khoảng cách xa nhất được tìm thấy trong lần truyền thứ hai là chiều dài đường kính. 

Lý do tác phẩm này xuất phát từ hình dạng của những con đường trên cây. Mỗi cặp đỉnh có đúng một đường đi nối giữa chúng. Nếu chúng ta bắt đầu từ một điểm tùy ý, việc di chuyển càng xa càng tốt sẽ đưa chúng ta tới điểm cuối cùng của cây. Cuộc tìm kiếm thứ hai từ đầu cực đó sẽ đến cực đối diện, đưa ra con đường dài nhất có thể. 

Sau khi tìm đường kính theo số cạnh, câu trả lời cuối cùng chỉ đơn giản là`diameter * 3`bởi vì vấn đề định nghĩa pi là 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho cây. Mỗi cạnh nối hai đỉnh nên cả hai hướng phải được lưu trữ vì cây không có hướng. 
2. Chạy DFS hoặc BFS từ bất kỳ đỉnh nào, chẳng hạn như đỉnh`1`, và ghi lại khoảng cách từ đỉnh đó. Tìm đỉnh có khoảng cách lớn nhất. 

Lần duyệt đầu tiên không cần bắt đầu từ một đỉnh đặc biệt. Đỉnh xa nhất mà nó tìm thấy là đủ để bắt đầu tìm kiếm đường kính thực. 

1. Chạy DFS hoặc BFS thứ hai từ đỉnh được tìm thấy ở bước trước. Một lần nữa ghi lại khoảng cách và lấy khoảng cách tối đa đạt được. 

Khoảng cách tối đa này là đường kính của cây được đo bằng các cạnh. 

1. Nhân đường kính với`3`và in kết quả. 

Lý do thuật toán đúng là vì cây có một đường đi duy nhất giữa mỗi cặp đỉnh. Đường đi đầu tiên tới một chiếc lá ở một bên của con đường dài nhất. Bắt đầu từ điểm cuối đó, đường truyền thứ hai phải đến điểm cuối đối diện của đường đi dài nhất, do đó khoảng cách tối đa tìm được chính xác là đường kính. Vì mọi đường kính hợp lệ đã được xem xét thông qua điểm cuối này nên không thể tồn tại khoảng cách lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def farthest(start, graph):
    stack = [(start, -1, 0)]
    best_node = start
    best_dist = 0

    while stack:
        node, parent, dist = stack.pop()

        if dist > best_dist:
            best_dist = dist
            best_node = node

        for nxt in graph[node]:
            if nxt != parent:
                stack.append((nxt, node, dist + 1))

    return best_node, best_dist

def solve():
    n = int(input())

    graph = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        a, b = map(int, input().split())
        graph[a].append(b)
        graph[b].append(a)

    if n == 1:
        print(0)
        return

    endpoint, _ = farthest(1, graph)
    _, diameter = farthest(endpoint, graph)

    print(diameter * 3)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cây trong`O(n)`bộ nhớ, điều này cần thiết vì mọi cạnh đều phải được thăm. Chức năng trợ giúp`farthest`thực hiện một DFS lặp lại. Sử dụng ngăn xếp rõ ràng sẽ tránh được các vấn đề về độ sâu đệ quy Python trên cây có thể là một chuỗi có độ dài`300000`. 

Cuộc gọi đầu tiên tới`farthest`chỉ xác định điểm cuối khởi đầu tốt cho việc tìm kiếm đường kính. Khoảng cách trả về của nó bị bỏ qua vì nó không nhất thiết phải là đường kính cuối cùng. Cuộc gọi thứ hai bắt đầu từ điểm cuối đó và khoảng cách trả về của nó là đường kính thực tế. 

Phép nhân được thực hiện sau khi tìm thấy đường kính. Đường kính tính các cạnh, do đó một đường đi có`k`các cạnh có chu vi`3k`. Không có vấn đề tràn số nguyên trong Python vì số nguyên tự động tăng lên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Cây chỉ có một đỉnh. 

| Bước | Nút bắt đầu | Nút xa nhất | Đường kính | Trả lời | 
| --- | --- | --- | --- | --- | 
| Cây ban đầu | 1 | 1 | 0 | 0 | 

Ví dụ này xác nhận trường hợp đỉnh bị cô lập. Không có đường đi giữa các đỉnh khác nhau nên đường kính bằng 0. 

### Ví dụ 2 

đầu vào:```
5
4 2
1 4
5 4
3 4
```Cây là một ngôi sao có tâm ở đỉnh`4`. 

| Bước | Nút bắt đầu | Nút xa nhất | Khoảng cách tối đa | 
| --- | --- | --- | --- | 
| Truyền tải đầu tiên | 1 | 2 | 2 | 
| Truyền tải thứ hai | 2 | 1 | 2 | 

Lần duyệt thứ hai tìm thấy đường kính của hai cạnh. Nhân với ba sẽ có chu vi cần thiết là sáu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Cây được duyệt hai lần và mỗi cạnh được xử lý một số lần không đổi. | 
| Không gian | O(n) | Danh sách kề và ngăn xếp DFS chứa tối đa một số phần tử tuyến tính. | 

Kích thước đầu vào tối đa là`300000`đỉnh nên cần phải có thuật toán tuyến tính. Hai đường duyệt cây đủ nhỏ để vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n = int(input())
    graph = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        a, b = map(int, input().split())
        graph[a].append(b)
        graph[b].append(a)

    def farthest(start):
        stack = [(start, -1, 0)]
        node = start
        dist = 0

        while stack:
            cur, parent, d = stack.pop()
            if d > dist:
                node = cur
                dist = d
            for nxt in graph[cur]:
                if nxt != parent:
                    stack.append((nxt, cur, d + 1))
        return node, dist

    if n == 1:
        return "0"

    a, _ = farthest(1)
    _, d = farthest(a)
    return str(d * 3)

assert solve("1\n") == "0", "single vertex"

assert solve("""3
3 2
2 1
""") == "6", "sample 2"

assert solve("""5
4 2
1 4
5 4
3 4
""") == "6", "sample 3"

assert solve("""2
1 2
""") == "3", "two vertices"

assert solve("""5
1 2
1 3
1 4
1 5
""") == "6", "star tree"

assert solve("""6
1 2
2 3
3 4
4 5
5 6
""") == "15", "long chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 | Xử lý cây nhỏ nhất có thể | 
| Hai đỉnh | 3 | Xác nhận đường kính đếm các cạnh, không phải đỉnh | 
| Cây sao | 6 | Kiểm tra cấu trúc phân nhánh | 
| Chuỗi dài | 15 | Kiểm tra cây kiểu có độ sâu tối đa | 
| Trường hợp mẫu | 6 | Xác nhận ví dụ tiêu chuẩn | 

## Vỏ cạnh 

Đối với cây một đỉnh:```
Input:
1
```Thuật toán bỏ qua việc truyền tải và trả về trực tiếp số 0. Nếu không có điều kiện này, mã giả định điểm cuối thứ hai tồn tại có thể truy cập dữ liệu không hợp lệ. 

Đối với cây hai đỉnh:```
Input:
2
1 2
```Lần duyệt đầu tiên sẽ tìm thấy đỉnh còn lại ở khoảng cách một. Lần duyệt thứ hai cũng tìm thấy khoảng cách tối đa bằng 1, vì vậy câu trả lời là`1 * 3 = 3`. Điều này xác nhận rằng khoảng cách được đo bằng các cạnh. 

Đối với cây sao:```
Input:
5
1 2
1 3
1 4
1 5
```Bắt đầu từ đỉnh`1`, các nút xa nhất là các lá ở khoảng cách một. Bắt đầu từ một trong những lá đó, lần truyền thứ hai đến một lá khác ở khoảng cách hai. Thuật toán tìm chính xác đường đi qua tâm thay vì chỉ xét các con trực tiếp.
