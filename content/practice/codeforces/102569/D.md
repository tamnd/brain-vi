---
title: "CF 102569D - Đường dẫn ngắn nhất tối thiểu về mặt từ điển"
description: "Biểu đồ mô tả một mạng lưới các đỉnh được kết nối bằng các cạnh hai chiều. Mỗi cạnh có một chữ cái viết thường gắn liền với nó. Di chuyển dọc theo đường đi từ đỉnh 1 đến đỉnh n sẽ tạo ra một chuỗi gồm các chữ cái gặp ở các cạnh."
date: "2026-07-31T07:47:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "D"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 178
verified: false
draft: false
---

[CF 102569D - Đường dẫn ngắn nhất tối thiểu về mặt từ điển](https://codeforces.com/problemset/problem/102569/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** không 

##Giải pháp 
#Hiểu vấn đề 

Biểu đồ mô tả một mạng lưới các đỉnh được kết nối bằng các cạnh hai chiều. Mỗi cạnh có một chữ cái viết thường gắn liền với nó. Di chuyển dọc theo đường đi từ đỉnh 1 đến đỉnh n sẽ tạo ra một chuỗi gồm các chữ cái gặp ở các cạnh. Trong số tất cả các đường dẫn có số cạnh nhỏ nhất có thể, chúng ta cần chọn đường dẫn có chuỗi được tạo ra nhỏ nhất về mặt từ điển, sau đó in cả đỉnh và chuỗi. 

Biểu đồ có thể chứa tới 200.000 đỉnh và 200.000 cạnh. Với giới hạn vài giây, cần phải có một thuật toán khám phá một lượng nhỏ công việc không đổi trên mỗi cạnh hoặc đỉnh. Các phương pháp liệt kê các đường dẫn, thử tất cả các kết hợp lựa chọn hoặc thực hiện tìm kiếm nhiều lần cho nhiều ứng viên sẽ tăng theo cấp số nhân hoặc ít nhất là vượt quá thời gian có sẵn. Giải pháp xung quanh O(n + m) là mục tiêu vì nó phù hợp với kích thước đầu vào. 

Khó khăn là điều kiện đường đi ngắn nhất và điều kiện từ điển tương tác với nhau. Đường đi có chữ cái đầu tiên nhỏ hơn sẽ vô dụng nếu nó dài hơn khoảng cách ngắn nhất. Đường dẫn có độ dài chính xác vẫn có thể bị mất vì ký tự sau lớn hơn. Thuật toán chỉ được giữ lại những đường dẫn có thể trở nên tối ưu. 

Một lỗi đơn giản là chọn ngay cạnh nhỏ nhất từ ​​đỉnh 1. Coi như:```
4 4
1 2 b
2 4 a
1 3 a
3 4 z
```Đường đi ngắn nhất có độ dài bằng 2. Việc chọn cạnh đầu tiên nhỏ nhất sẽ cho đường đi từ 1 đến 3 đến 4 và chuỗi`az`, điều đó đúng. Tuy nhiên, một phương pháp tham lam không giới hạn bản thân ở các lớp đường dẫn ngắn nhất có thể thất bại trong các biểu đồ khác bằng cách đi theo một chữ cái nhỏ vào ngõ cụt hoặc một tuyến đường dài hơn. 

Một lỗi phổ biến khác là chạy BFS và đi theo con đường ngắn nhất đầu tiên được tìm thấy. Coi như:```
3 3
1 2 b
2 3 a
1 3 z
```Đường đi ngắn nhất có độ dài 1, vì vậy câu trả lời là:```
1
1 3
z
```Việc tìm kiếm ưu tiên các chữ cái nhỏ trước khi kiểm tra khoảng cách có thể chọn đường dẫn`ba`, nhưng con đường đó không phải là con đường ngắn nhất chút nào. 

Vấn đề thứ ba xuất hiện khi một số đỉnh có chung tiền tố tốt nhất. Coi như:```
5 5
1 2 a
1 3 a
2 5 b
3 4 a
4 5 b
```Chuỗi tốt nhất là`ab`. Sau khi chọn chữ cái đầu tiên`a`, cả hai đỉnh 2 và 3 vẫn là ứng cử viên. Việc loại bỏ đỉnh 3 quá sớm có thể loại bỏ phần tiếp theo duy nhất mang lại câu trả lời tối ưu. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp sẽ tạo ra tất cả các đường dẫn từ đỉnh 1 đến đỉnh n, chỉ giữ lại những đường dẫn có độ dài tối thiểu và so sánh các chuỗi của chúng. Điều này đúng vì nó kiểm tra chính xác tập ứng viên được yêu cầu. Vấn đề là số lượng đường dẫn trong biểu đồ có thể rất lớn. Một biểu đồ có nhiều cạnh phân nhánh có thể có nhiều đường đi khác nhau theo cấp số nhân, do đó phương pháp này thậm chí không thể kết thúc với đầu vào vừa phải. 

Một nỗ lực tốt hơn là chạy BFS từ đỉnh 1 vì BFS cho khoảng cách ngắn nhất trong biểu đồ không có trọng số. Sau đó, chúng ta có thể thử chọn chữ cái nhỏ nhất trong khi duyệt qua biểu đồ. Phần còn thiếu là biết đỉnh nào vẫn có thể tham gia vào đường đi ngắn nhất. Nếu không có thông tin đó, một lá thư nhỏ có thể đi chệch khỏi mục tiêu. 

Quan sát quan trọng là khoảng cách chia tất cả các đường đi ngắn nhất hợp lệ thành các lớp. Gọi khoảng cách ngắn nhất từ ​​đỉnh 1 đến đỉnh n là L. Một đỉnh chỉ có thể xuất hiện ở vị trí i của đường đi ngắn nhất nếu khoảng cách của nó tới đỉnh 1 là i và khoảng cách của nó đến đỉnh n là L - i. Khi đã biết các lớp hợp lệ này, mọi bước di chuyển hợp lệ luôn đi từ lớp i đến lớp i + 1. 

Bây giờ sự lựa chọn từ điển trở thành địa phương. Ở mỗi lớp, chúng ta có một tập hợp các đỉnh có chung tiền tố tốt nhất được tìm thấy cho đến nay. Chúng tôi kiểm tra tất cả các cạnh đi tiếp tục đến lớp hợp lệ tiếp theo, chọn chữ cái nhỏ nhất trong số chúng và giữ cho mọi đích đến đều có thể truy cập bằng chữ cái đó. Việc giữ tất cả các đỉnh như vậy là cần thiết vì một số tiền tố có thể vẫn được liên kết cho đến khi ký tự sau tách chúng ra. 

Hai lần chạy BFS tìm cấu trúc đường đi ngắn nhất. Quá trình xử lý lớp tham lam sau đó sẽ xây dựng chuỗi nhỏ nhất bên trong cấu trúc đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số đường dẫn) | O(n) | Quá chậm | 
| Các lớp BFS với khả năng tái thiết tham lam | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS từ đỉnh 1 và lưu trữ`dist_start`, khoảng cách ngắn nhất từ ​​nguồn tới mọi đỉnh. Chạy một BFS khác từ đỉnh n và lưu trữ`dist_end`, khoảng cách ngắn nhất từ ​​mọi đỉnh tới đích. 
2. Hãy để`length = dist_start[n]`. Một đỉnh chỉ thuộc đường đi ngắn nhất khi`dist_start[v] + dist_end[v] == length`. Điều này loại bỏ mọi đỉnh không thể là một phần của tuyến đường tối ưu. 
3. Bắt đầu với tập hợp hiện tại chỉ chứa đỉnh 1. Đối với mọi lớp từ 0 đến`length - 1`, kiểm tra tất cả các cạnh rời khỏi tập hợp hiện tại. Chỉ xem xét một cạnh`(u, v)`khi`v`nằm trong lớp đường dẫn ngắn nhất tiếp theo. 
4. Tìm ký tự nhỏ nhất trong số tất cả các cạnh được xem xét. Ký tự này phải là ký tự tiếp theo của câu trả lời vì mọi ứng viên khác sẽ làm cho chuỗi lớn hơn ở vị trí đầu tiên nơi chúng khác nhau. 
5. Xây dựng tập tiếp theo bằng cách lấy mọi điểm đến có thể tiếp cận thông qua một cạnh có ký tự nhỏ nhất đó. Lưu trữ một đỉnh cha cho mỗi đỉnh mới được thêm vào để sau này có thể xây dựng lại đường dẫn hợp lệ. 
6. Sau khi xử lý tất cả các lớp, chọn bất kỳ đỉnh nào trong tập cuối cùng và theo ngược lại các đỉnh cha đã lưu cho đến đỉnh 1. Đảo ngược trình tự này để có được đường đi. 

Tại sao nó hoạt động: Ở mỗi lớp, tập hợp hiện tại chứa chính xác các đỉnh có thể xuất hiện sau tiền tố nhỏ nhất được tạo cho đến nay. Khi chúng ta chọn ký tự tiếp theo nhỏ nhất có thể, mọi đường dẫn sử dụng ký tự lớn hơn sẽ ngay lập tức trở nên kém hơn về mặt từ điển. Việc giữ mọi đích đến có ký tự đó sẽ bảo toàn tất cả các phần tiếp theo có thể có mà vẫn chia sẻ tiền tố tốt nhất. Vì mọi quá trình chuyển đổi đều nằm trong các lớp đường dẫn ngắn nhất nên đường dẫn cuối cùng có độ dài tối thiểu. Bất biến vẫn đúng sau mỗi lớp, vì vậy chuỗi hoàn chỉnh là chuỗi nhỏ nhất trong số tất cả các đường dẫn ngắn nhất. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def bfs(start, graph):
    n = len(graph) - 1
    dist = [-1] * (n + 1)
    dist[start] = 0
    q = deque([start])

    while q:
        u = q.popleft()
        for v, _ in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    return dist

def solve():
    n, m = map(int, input().split())
    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v, c = input().split()
        u = int(u)
        v = int(v)
        graph[u].append((v, c))
        graph[v].append((u, c))

    dist_start = bfs(1, graph)
    dist_end = bfs(n, graph)

    length = dist_start[n]

    parent = [0] * (n + 1)
    current = {1}
    answer_chars = []

    for layer in range(length):
        best = '{'
        for u in current:
            for v, c in graph[u]:
                if dist_start[v] == layer + 1 and dist_start[v] + dist_end[v] == length:
                    if c < best:
                        best = c

        answer_chars.append(best)

        nxt = set()
        for u in current:
            for v, c in graph[u]:
                if c == best and dist_start[v] == layer + 1 and dist_start[v] + dist_end[v] == length:
                    if v not in nxt:
                        parent[v] = u
                        nxt.add(v)

        current = nxt

    end = next(iter(current))
    path = []

    while end != 0:
        path.append(end)
        end = parent[end]

    path.reverse()

    out = []
    out.append(str(length))
    out.append(" ".join(map(str, path)))
    out.append("".join(answer_chars))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Hai cuộc gọi BFS là các phép tính khoảng cách ngắn nhất độc lập. Vì biểu đồ không có trọng số nên một hàng đợi là đủ và mỗi cạnh chỉ được kiểm tra một số lần không đổi. 

Vòng lặp chính đại diện cho việc xây dựng tham lam. Biến`current`là tập hợp các đỉnh có chung tiền tố tốt nhất đã được chọn. điều kiện`dist_start[v] + dist_end[v] == length`lọc ra các cạnh không thể thuộc về bất kỳ đường đi ngắn nhất nào. 

Mảng cha lưu trữ phần trước được chọn khi một đỉnh đầu tiên đi vào lớp tiếp theo. Một đỉnh chỉ xuất hiện trong một lớp khoảng cách, vì vậy giá trị gốc đơn này là đủ để tái cấu trúc. Việc truyền ngược cuối cùng tạo ra một trong các đường dẫn tối ưu, ngay cả khi một số đường dẫn có cùng một chuỗi tối thiểu. 

nhân vật`'{`được sử dụng làm giá trị ban đầu vì nó lớn hơn về mặt từ điển so với mọi chữ cái tiếng Anh viết thường. Chuỗi Python so sánh các ký tự theo giá trị ASCII của chúng, vì vậy chuỗi này hoạt động an toàn cho bảng chữ cái đầu vào. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 2
1 2 a
2 3 b
```Khoảng cách ngắn nhất là 2. Các lớp đường dẫn ngắn nhất hợp lệ là 1, 2, 3. 

| Lớp | Đỉnh hiện tại | Nhân vật được chọn | Đỉnh tiếp theo | 
| --- | --- | --- | --- | 
| 0 | {1} | một | {2} | 
| 1 | {2} | b | {3} | 

Con đường duy nhất có thể được theo sau. Bất biến giữ nguyên vì sau mỗi bước tập hợp chứa tất cả các đỉnh có thể tạo ra tiền tố tốt nhất. 

Đối với mẫu thứ hai:```
3 3
1 3 z
1 2 a
2 3 b
```Cạnh trực tiếp ngắn hơn đường đi qua đỉnh 2. 

| Lớp | Đỉnh hiện tại | Nhân vật được chọn | Đỉnh tiếp theo | 
| --- | --- | --- | --- | 
| 0 | {1} | z | {3} | 

Giới hạn lớp sẽ loại bỏ đường dẫn`1 -> 2 -> 3`bởi vì độ dài của nó là 2 trong khi khoảng cách ngắn nhất là 1. Điều này cho thấy tại sao thông tin về khoảng cách phải được kết hợp với các lựa chọn từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Cả quá trình truyền tải BFS và xử lý lớp tham lam đều kiểm tra đồ thị một cách tuyến tính. | 
| Không gian | O(n + m) | Danh sách kề lưu trữ tất cả các cạnh và mảng lưu trữ khoảng cách, cha mẹ và các tập hợp tạm thời. | 

Các giới hạn đầu vào yêu cầu một giải pháp tuyến tính. Với 200.000 đỉnh và cạnh, O(n + m) nằm trong giới hạn thời gian, trong khi mọi cách tiếp cận tùy thuộc vào việc liệt kê các đường dẫn hoặc tìm kiếm lặp đi lặp lại qua nhiều ứng cử viên thì không. 

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

assert run("""3 2
1 2 a
2 3 b
""") == """2
1 2 3
ab
""", "sample 1"

assert run("""3 3
1 3 z
1 2 a
2 3 b
""") == """1
1 3
z
""", "sample 2"

assert run("""4 4
1 2 b
2 4 a
1 3 a
3 4 z
""") == """2
1 3 4
az
""", "sample 3"

assert run("""2 1
1 2 c
""") == """1
1 2
c
""", "minimum size"

assert run("""5 5
1 2 a
1 3 a
2 5 b
3 4 a
4 5 b
""") == """2
1 2 5
ab
""", "multiple vertices with same prefix"

assert run("""6 6
1 2 a
2 6 a
1 3 a
3 4 a
4 6 a
1 5 b
""") == """2
1 2 6
aa
""", "equal characters and competing paths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh nối với nhau bằng một cạnh | Độ dài 1 đường dẫn | Xử lý biểu đồ nhỏ nhất. | 
| Một số đỉnh có chung tiền tố | Bất kỳ hợp lệ`ab`con đường ngắn nhất | Giữ lại tất cả các ứng cử viên bị ràng buộc thay vì loại bỏ chúng. | 
| Một số chữ cái giống hệt nhau | Ngắn nhất`aa`tuyến đường | Xử lý các ký tự lặp lại và ngắt kết nối. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là tình huống trong đó cạnh trực tiếp đánh bại một đường dẫn dài hơn nhỏ hơn về mặt từ điển. 

đầu vào:```
3 3
1 3 z
1 2 a
2 3 b
```Khoảng cách BFS từ 1 đến 3 là 1. Trong quá trình xây dựng tham lam, chỉ các đỉnh có khoảng cách 1 mới được coi là lớp tiếp theo. Đỉnh 2 bị loại trừ vì để đạt được nó cần có một cạnh khác. Thuật toán đưa ra đường đi ngắn nhất duy nhất:```
1
1 3
z
```Trường hợp cạnh thứ hai là khi một số đỉnh có chung tiền tố. 

đầu vào:```
5 5
1 2 a
1 3 a
2 5 b
3 4 a
4 5 b
```Độ dài ngắn nhất là 2. Sau khi chọn`a`, tập hợp hiện tại trở thành`{2, 3}`. Lớp tiếp theo chứa các cạnh`2 -> 5`Và`4 -> 5`, nhưng chỉ`2 -> 5`đến từ tập hợp hiện tại, vì vậy chuỗi cuối cùng là`ab`. Thuật toán không chuyển giao quá sớm cho một đỉnh. 

Trường hợp cạnh thứ ba là khi đồ thị chứa nhiều chữ cái bằng nhau. 

đầu vào:```
6 6
1 2 a
2 6 a
1 3 a
3 4 a
4 6 a
1 5 b
```Khoảng cách ngắn nhất là 2. Cả hai`1 -> 2 -> 6`Và`1 -> 3 -> 4 -> 6`bắt đầu với`a`, nhưng tuyến thứ hai dài hơn và bị loại bỏ bởi điều kiện lớp. Thuật toán chỉ xem xét các cạnh di chuyển chính xác một lớp đường đi ngắn nhất về phía trước, do đó, nó đưa ra:```
2
1 2 6
aa
```
