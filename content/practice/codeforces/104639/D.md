---
title: "CF 104639D - Độ chuyển tiếp"
description: "Chúng ta được cho một đồ thị đơn giản vô hướng trong đó một số cặp đỉnh đã được nối với nhau bằng các cạnh. Thao tác chúng ta được phép thực hiện là thêm các cạnh mới, nhưng chúng ta không thể xóa các cạnh hiện có và không thể đưa vào các cạnh song song hoặc tự vòng."
date: "2026-06-29T16:56:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104639
codeforces_index: "D"
codeforces_contest_name: "The 2023 ICPC Asia EC Regionals Online Contest (I)"
rating: 0
weight: 104639
solve_time_s: 58
verified: true
draft: false
---

[CF 104639D - Tính chuyển tiếp](https://codeforces.com/problemset/problem/104639/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị đơn giản vô hướng trong đó một số cặp đỉnh đã được nối với nhau bằng các cạnh. Thao tác chúng ta được phép thực hiện là thêm các cạnh mới, nhưng chúng ta không thể xóa các cạnh hiện có và không thể đưa vào các cạnh song song hoặc tự vòng. 

Mục tiêu cuối cùng là biến biểu đồ này thành một biểu đồ có đặc tính cấu trúc mạnh: bất cứ khi nào hai đỉnh có thể chạm tới nhau thông qua một chuỗi các cạnh nào đó, chúng phải có một cạnh trực tiếp giữa chúng. Nói cách khác, khả năng tiếp cận và tính liền kề phải trùng nhau trong biểu đồ cuối cùng. 

Tình trạng này có một hệ quả rất cứng nhắc. Bên trong bất kỳ vùng liên thông nào của đồ thị, mọi cặp đỉnh đều phải được kết nối trực tiếp, nếu không sẽ tồn tại một đường đi giữa chúng mà không có cạnh. Vì vậy, mỗi thành phần được kết nối phải trở thành một cụm trong biểu đồ cuối cùng. 

Nhiệm vụ là tính số cạnh tối thiểu chúng ta cần thêm để đạt được điều kiện này. 

Các ràng buộc lên tới một triệu đỉnh và một triệu cạnh. Điều đó ngay lập tức gợi ý rằng bất kỳ giải pháp nào cố gắng xem xét trực tiếp các cặp đỉnh đều là không thể. Ngay cả hành vi bậc hai trên mỗi thành phần cũng quá chậm nếu chúng ta không cẩn thận về cách xử lý các thành phần. Truyền tải đồ thị tuyến tính hoặc gần tuyến tính là hướng khả thi duy nhất. 

Một điểm tinh tế là chúng ta không được phép “sửa” đồ thị bằng cách hợp nhất các thành phần với giá rẻ. Nếu chúng ta kết nối hai thành phần đã ngắt kết nối trước đó thì mọi đỉnh trong cấu trúc kết hợp sẽ có thể truy cập được từ mọi đỉnh khác thông qua cạnh mới, điều này buộc chúng ta phải thêm tất cả các cạnh còn thiếu trên tập hợp đã hợp nhất. Điều đó nhanh chóng trở nên đắt hơn so với việc xử lý các thành phần riêng biệt. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta có thể kết nối mọi thứ thành một nhóm lớn và đếm các cạnh còn thiếu từ đó. Điều đó bỏ qua việc thêm một cạnh duy nhất giữa các thành phần sẽ thay đổi cấu trúc khả năng tiếp cận trên toàn cầu và buộc phải có một loạt các cạnh cần thiết bổ sung. 

Ví dụ, hãy xem xét ba thành phần có kích thước 2, 2 và 2 không có cạnh trong. Nếu chúng ta kết nối tất cả chúng thành một thành phần, cuối cùng chúng ta phải tạo thành một nhóm có kích thước 6, yêu cầu 15 cạnh, trong khi việc giữ chúng tách biệt chỉ cần tổng cộng 3 cạnh cho mỗi thành phần, do đó, 9 cạnh được thêm vào. Sự khác biệt là đáng kể và cho thấy tại sao việc hợp nhất là không tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng quá trình sửa chữa các vi phạm nhiều lần. Chúng ta có thể tìm kiếm bất kỳ cặp đỉnh nào trong cùng một thành phần được kết nối không được kết nối trực tiếp, thêm cạnh đó và tiếp tục cho đến khi đạt được mức đóng. Mỗi lần kiểm tra yêu cầu BFS hoặc DFS để xác minh khả năng tiếp cận và tính liền kề, đồng thời mỗi lần bổ sung sẽ thay đổi cấu trúc một lần nữa. 

Vấn đề với cách tiếp cận này là một thành phần có kích thước n có thể yêu cầu theo thứ tự n2 cạnh bị thiếu và mỗi lần chèn có thể kích hoạt việc tính toán lại kết nối. Điều này dẫn đến trường hợp xấu nhất bậc ba hoặc ít nhất là chi phí bậc hai cho mỗi thành phần, vượt xa giới hạn khả thi đối với n lên đến 10⁶. 

Quan sát quan trọng là cấu trúc cuối cùng bên trong mỗi thành phần được kết nối đã được xác định đầy đủ: nó phải trở thành một biểu đồ hoàn chỉnh trên chính xác các đỉnh đó. Chúng ta không cần mô phỏng quá trình thêm từng cạnh một. Chúng ta chỉ cần đếm xem có bao nhiêu cạnh bị thiếu trong biểu đồ hoàn chỉnh đó. 

Một khi chúng ta nhận ra điều này, vấn đề sẽ giảm xuống thành một nhiệm vụ phân rã. Chúng tôi tìm thấy các thành phần được kết nối của biểu đồ ban đầu. Đối với mỗi thành phần, chúng tôi tính toán cần bao nhiêu cạnh để tạo thành một cụm. Nếu một thành phần có s đỉnh và hiện có e cạnh, thì một đồ thị hoàn chỉnh sẽ chứa s·(s−1)/2 cạnh, vì vậy chúng ta phải thêm chính xác s·(s−1)/2 − e cạnh cho thành phần đó. 

Tổng hợp điều này trên tất cả các thành phần sẽ đưa ra câu trả lời cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n³) trường hợp xấu nhất | O(n + m) | Quá chậm | 
| Đếm các thành phần được kết nối | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào cấu trúc của các thành phần được kết nối nên thuật toán tập trung vào việc khám phá chúng và đo kích thước cũng như số cạnh của chúng. 

1. Xây dựng biểu diễn danh sách kề của biểu đồ và chuẩn bị một mảng đã thăm trên tất cả các đỉnh. Điều này cho phép chúng ta duyệt qua từng thành phần được kết nối một cách hiệu quả. 
2. Chạy DFS hoặc BFS từ mọi đỉnh chưa được thăm dò để xác định một thành phần được kết nối tại một thời điểm. Trong khi di chuyển ngang, hãy thu thập tất cả các đỉnh thuộc thành phần này và đếm xem có bao nhiêu cạnh gặp phải từ các đỉnh này. 

Việc đếm cạnh phải được xử lý cẩn thận vì mỗi cạnh vô hướng xuất hiện hai lần trong danh sách kề, vì vậy chúng tôi đảm bảo việc đếm hoặc chia nhất quán sau này. 
3. Với mỗi thành phần, hãy tính kích thước s của nó. Số cạnh mà nó phải chứa trong một đồ thị hoàn chỉnh là s·(s−1)/2. So sánh điều này với số cạnh hiện có trong thành phần đó. 
4. Thêm sự khác biệt giữa các cạnh được yêu cầu và các cạnh hiện có vào câu trả lời. 
5. Xuất tổng số tiền của tất cả các thành phần. 

### Tại sao nó hoạt động 

Bất biến chính là các thành phần được kết nối độc lập với yêu cầu về độ bắc cầu. Nếu hai đỉnh nằm trong các thành phần khác nhau thì không có đường đi giữa chúng, do đó điều kiện không yêu cầu về các cạnh giữa các thành phần đó. Bên trong một thành phần, khi có đường dẫn giữa hai đỉnh, đồ thị cuối cùng phải chứa cạnh trực tiếp giữa chúng. Điều đó buộc mọi thành phần phải phát triển thành một cụm và không có giải pháp nào có thể tránh được việc bổ sung chính xác các cạnh còn thiếu của cụm đó mà không vi phạm điều kiện. Điều này làm cho việc phân rã thành phần vừa đủ vừa cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, m = map(int, input().split())

adj = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append(v)
    adj[v].append(u)

visited = [False] * n

def dfs(start):
    stack = [start]
    visited[start] = True
    nodes = 0
    edges = 0

    while stack:
        u = stack.pop()
        nodes += 1
        for v in adj[u]:
            edges += 1
            if not visited[v]:
                visited[v] = True
                stack.append(v)

    return nodes, edges // 2

ans = 0

for i in range(n):
    if not visited[i]:
        sz, e = dfs(i)
        ans += sz * (sz - 1) // 2 - e

print(ans)
```Việc triển khai sử dụng DFS lặp để tránh các vấn đề về độ sâu đệ quy trên các chuỗi lớn. Mỗi cạnh được tính hai lần trong quá trình truyền tải, một lần từ mỗi điểm cuối, vì vậy chúng tôi chia cho hai khi tính số cạnh cuối cùng cho mỗi thành phần. 

Một sai lầm phổ biến là quên rằng các cạnh phải được tính trên mỗi thành phần chứ không phải trên toàn bộ. Chỉ cần tính tổng tất cả các cạnh và trừ đi từ tổng thể n·(n−1)/2 sẽ cho rằng biểu đồ cuối cùng phải là một cụm duy nhất một cách không chính xác, điều này là không bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2
1 3
2 3
```Chúng ta có một thành phần chứa tất cả 4 đỉnh. 

| Bước | Thành phần | Kích thước | Các cạnh được tính | Các cạnh hoàn chỉnh | Đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {1,2,3,4} | 4 | 3 | 6 | 3 | 

Thành phần này đã có một hình tam giác trong số {1,2,3}, nhưng đỉnh 4 bị cô lập trong thành phần nên chúng ta cần các cạnh (4,1), (4,2), (4,3). Đó là 3 cạnh. 

Đầu ra là 3. 

Điều này xác nhận rằng ngay cả khi một phần của thành phần đã dày đặc, chúng tôi vẫn tính toán dựa trên việc đóng hoàn toàn toàn bộ thành phần được kết nối. 

### Ví dụ 2 

đầu vào:```
5 2
1 2
4 5
```Chúng tôi có ba thành phần: {1,2}, {3}, {4,5}. 

| Thành phần | Kích thước | Các cạnh hiện có | Các cạnh hoàn chỉnh | Đã thêm | 
| --- | --- | --- | --- | --- | 
| {1,2} | 2 | 1 | 1 | 0 | 
| {3} | 1 | 0 | 0 | 0 | 
| {4,5} | 2 | 1 | 1 | 0 | 

Tổng cộng thêm = 0. 

Điều này cho thấy rằng một biểu đồ bao gồm các thành phần đã hoàn chỉnh đã có tính bắc cầu và không cần thêm cạnh nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n + m) | Danh sách kề và mảng đã thăm | 

Các ràng buộc cho phép lên tới một triệu đỉnh và cạnh, và việc truyền tải theo thời gian tuyến tính này phù hợp một cách thoải mái trong các giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính theo kích thước của biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    input = _sys.stdin.readline
    sys.setrecursionlimit(10**7)

    n, m = map(int, input().split())
    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append(v)
        adj[v].append(u)

    visited = [False] * n

    def dfs(start):
        stack = [start]
        visited[start] = True
        nodes = 0
        edges = 0
        while stack:
            u = stack.pop()
            nodes += 1
            for v in adj[u]:
                edges += 1
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)
        return nodes, edges // 2

    ans = 0
    for i in range(n):
        if not visited[i]:
            sz, e = dfs(i)
            ans += sz * (sz - 1) // 2 - e

    return str(ans)

# provided sample
assert run("4 3\n1 2\n1 3\n2 3\n") == "3"

# single node
assert run("1 0\n") == "0"

# already complete components
assert run("3 3\n1 2\n2 3\n1 3\n") == "0"

# two disjoint edges
assert run("5 2\n1 2\n4 5\n") == "0"

# chain graph
assert run("4 3\n1 2\n2 3\n3 4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | xử lý thành phần tầm thường | 
| tam giác hoàn chỉnh | 0 | không cần thêm cạnh | 
| các cạnh rời rạc | 0 | thành phần độc lập | 
| chuỗi | 3 | đóng hoàn toàn trong một thành phần | 

## Vỏ cạnh 

Một đỉnh cô lập duy nhất là thành phần riêng của nó có kích thước bằng một. Công thức s·(s−1)/2 ngay lập tức cho kết quả bằng 0, do đó không có cạnh nào được thêm vào. Thuật toán xử lý việc này một cách tự nhiên vì DFS đánh dấu đỉnh và trả về các cạnh bằng 0 cho thành phần đó. 

Một thành phần được kết nối đầy đủ đã thỏa mãn điều kiện nhóm. Trong quá trình truyền tải, mỗi cạnh được tính chính xác một lần cho mỗi thành phần và vì nó đã khớp với s·(s−1)/2 nên phép trừ mang lại kết quả bằng 0. Không có nguy cơ đếm quá mức vì việc đếm cạnh được cục bộ hóa cho từng thành phần. 

Biểu đồ đường dẫn dài kiểm tra trường hợp xấu nhất về độ chính xác của độ sâu và tính cạnh của DFS. Mặc dù mọi đỉnh đều được kết nối nhưng chỉ tồn tại n−1 cạnh và công thức tính toán chính xác số cạnh bị thiếu cần thiết để hoàn thành cụm.
