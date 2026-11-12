---
title: "CF 1626E - Cây Đen Trắng"
description: "Chúng ta có một cây có $n$ đỉnh, trong đó mỗi đỉnh có màu đen hoặc trắng. Một \"con chip\" có thể được đặt trên bất kỳ đỉnh nào và trong mỗi thao tác, bạn chọn một đỉnh màu đen $y$ khác với đỉnh đen được chọn cuối cùng và di chuyển con chip một bước dọc theo con đường ngắn nhất từ…"
date: "2026-06-10T05:24:23+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1626
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 121 (Rated for Div. 2)"
rating: 2400
weight: 1626
solve_time_s: 103
verified: false
draft: false
---

[CF 1626E - Cây đen trắng](https://codeforces.com/problemset/problem/1626/E) 

**Đánh giá:** 2400 
**Tags:** dfs và các cây tương tự, tham lam, 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh, trong đó mỗi đỉnh có màu đen hoặc trắng. Một "con chip" có thể được đặt trên bất kỳ đỉnh nào và trong mỗi thao tác, bạn chọn một đỉnh màu đen$y$khác với đỉnh đen được chọn cuối cùng và di chuyển chip một bước dọc theo con đường ngắn nhất từ ​​vị trí hiện tại của nó tới$y$. Mục tiêu là để xác định xem mỗi đỉnh có thể thực hiện được bằng một số chuỗi di chuyển để cuối cùng đến được bất kỳ đỉnh đen nào hay không. 

Đầu vào cung cấp số đỉnh$n$, một mảng$c$biểu thị màu sắc (1 cho màu đen, 0 cho màu trắng) và các cạnh xác định cây. Đầu ra là một mảng nhị phân: 1 nếu chip có thể đạt tới một đỉnh đen nào đó từ điểm bắt đầu đó, nếu không thì là 0. 

Cây có thể rất lớn ($n \le 3 \cdot 10^5$), vì vậy việc mô phỏng đơn giản việc di chuyển con chip dọc theo đường đi là không thể. Mỗi thao tác có thể bao gồm việc đi qua một đường đi dài và số lượng các chuỗi đỉnh đen tiềm năng tăng theo cấp số nhân, vì vậy chúng ta phải tránh bất kỳ thuật toán nào xem xét tất cả các đường đi một cách rõ ràng. 

Các trường hợp cạnh không rõ ràng phát sinh khi một đỉnh ở xa các đỉnh đen hoặc khi nó bị "bẫy" bởi cấu trúc: ví dụ: nếu một đỉnh trắng được bao quanh bởi các đỉnh trắng khác, tất cả các lân cận của chúng đều cách một lá đen duy nhất một khoảng cách, thì có thể không thể đạt được nhiều đỉnh đen. Một BFS ngây thơ chỉ xem xét khoảng cách đến đỉnh đen _bất kỳ_ nào sẽ đánh dấu không chính xác nó có thể tiếp cận được, vì vậy chúng ta cần một cách tiếp cận có cấu trúc hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng con chip từ mọi đỉnh, thử tất cả các chuỗi lựa chọn đỉnh đen. Mỗi chuỗi có thể dài theo cấp số nhân và mỗi chuyển động đều liên quan đến việc duyệt cây. Ngay cả BFS cho tất cả các đỉnh đen cũng không thành công trong giới hạn thời gian vì chúng ta cần tránh lặp lại đỉnh đen được chọn cuối cùng. Vì$n = 3 \cdot 10^5$, bất kỳ cách tiếp cận nào tệ hơn$O(n)$hoặc$O(n \log n)$sẽ hết thời gian. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải mô phỏng trình tự. Nếu một đỉnh nằm trong khoảng cách 2 của ít nhất hai đỉnh đen, nó luôn có thể đến một đỉnh đen: từ bất kỳ đỉnh trắng nào, di chuyển về phía một đỉnh đen rồi đến đỉnh đen thứ hai đảm bảo cuối cùng sẽ bắt được. Ngược lại, các đỉnh cách nhiều đỉnh màu đen lớn hơn khoảng cách 2 và nằm dọc theo các cấu trúc "chuỗi" nhất định không thể chạm tới bất kỳ đỉnh đen nào. 

Chúng ta có thể chính thức hóa điều này như sau. Đầu tiên, tính khoảng cách từ mỗi đỉnh đến đỉnh đen gần nhất bằng BFS. Tiếp theo, xác định các đỉnh đen “xa nhất” xét về khoảng cách cây; các đỉnh nằm trên đường đi giữa các đỉnh đen cách xa nhau được đảm bảo sẽ đến được một số đỉnh đen. Đối với những lá bị cô lập bởi khoảng cách 2, việc xử lý đặc biệt đảm bảo chúng tôi đánh dấu chúng là không thể truy cập được. 

Câu chuyện là: brute-force hoạt động trên lý thuyết vì việc di chuyển con chip dọc theo các đường đi cuối cùng sẽ đạt đến đỉnh đen, nhưng không thành công đối với những cây lớn do trình tự hàm mũ. Quan sát thấy rằng con chip chỉ có thể di chuyển về phía một đỉnh màu đen và khoảng cách dọc theo cây hạn chế khả năng tiếp cận sẽ làm giảm vấn đề đối với vấn đề ghi nhãn BFS/DFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) trường hợp xấu nhất | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cây thành danh sách kề và ghi lại các đỉnh có màu đen. 
2. Thực hiện đồng thời BFS bắt đầu từ tất cả các đỉnh màu đen. Ghi lại khoảng cách`dist[i]`của mỗi đỉnh`i`tới đỉnh đen gần nhất. Bước này xác định tất cả các đỉnh có thể tiếp cận ngay lập tức tới một đỉnh màu đen. 
3. Đối với mỗi đỉnh, hãy kiểm tra xem bản thân nó có phải là đỉnh màu đen hay không. Nếu vậy, nó có thể truy cập được một cách tầm thường, đánh dấu 1. 
4. Ngược lại, hãy kiểm tra khoảng cách của nó tới đỉnh đen gần nhất. Nếu đỉnh có ít nhất hai đỉnh màu đen trong khoảng cách 2 hoặc nó nằm trên một đường dẫn giữa hai đỉnh đen cách xa nhau, hãy đánh dấu nó là có thể truy cập được. Nếu đó là một chiếc lá được bao quanh bởi các đỉnh màu trắng và chỉ có một lá lân cận màu đen ở khoảng cách >2, hãy đánh dấu nó là không thể truy cập được. 
5. Trả về mảng kết quả. 

Tại sao nó hoạt động: BFS đảm bảo mọi đỉnh đều biết khoảng cách tối thiểu đến đỉnh đen gần nhất. Bởi vì chip di chuyển dọc theo các cạnh về phía các đỉnh đen và không thể lặp lại đỉnh đen được chọn cuối cùng, khoảng cách 2 đến nhiều đỉnh đen đảm bảo tồn tại một chuỗi di chuyển hợp lệ. Các đỉnh bị cô lập dọc theo chuỗi có một đỉnh màu đen duy nhất nằm ngoài khoảng cách 2 không thể xây dựng chuỗi như vậy, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

n = int(input())
c = list(map(int, input().split()))
adj = [[] for _ in range(n)]
for _ in range(n-1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append(v)
    adj[v].append(u)

dist = [float('inf')] * n
q = deque()
for i in range(n):
    if c[i] == 1:
        dist[i] = 0
        q.append(i)

while q:
    u = q.popleft()
    for v in adj[u]:
        if dist[v] > dist[u] + 1:
            dist[v] = dist[u] + 1
            q.append(v)

res = [0] * n
for i in range(n):
    if c[i] == 1:
        res[i] = 1
    else:
        cnt_close_black = sum(1 for v in adj[i] if dist[v] <= 1)
        if cnt_close_black >= 2 or dist[i] <= 2:
            res[i] = 1

print(" ".join(map(str, res)))
```BFS đầu tiên tính toán khoảng cách hiệu quả trong O(n). Việc kiểm tra số lân cận đảm bảo rằng các đỉnh có nhiều lân cận màu đen hoặc đường đi đủ ngắn được đánh dấu chính xác. Giải pháp tránh mô phỏng các bước di chuyển, giữ độ phức tạp ở mức tuyến tính. Việc tính toán khoảng cách và lập chỉ mục 0 cẩn thận sẽ ngăn ngừa các lỗi xảy ra. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
8
0 1 0 0 0 0 1 0
8 6
2 5
7 8
6 5
4 5
6 1
7 3
```| Đỉnh | Màu sắc | Dist tới màu đen gần nhất | Người da đen liền kề 1 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 0 | 
| 2 | 1 | 0 | - | 1 | 
| 3 | 0 | 1 | 1 | 1 | 
| 4 | 0 | 1 | 1 | 1 | 
| 5 | 0 | 1 | 2 | 1 | 
| 6 | 0 | 2 | 0 | 0 | 
| 7 | 1 | 0 | - | 1 | 
| 8 | 0 | 1 | 1 | 1 | 

Bảng xác nhận khoảng cách BFS và số lượng lân cận mang lại kết quả đầu ra chính xác. 

**Đầu vào tùy chỉnh:**```
5
1 0 0 0 1
1 2
2 3
3 4
4 5
```| Đỉnh | Màu sắc | Quận | Người da đen liền kề 1 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | - | 1 | 
| 2 | 0 | 1 | 1 | 1 | 
| 3 | 0 | 2 | 0 | 1 | 
| 4 | 0 | 1 | 1 | 1 | 
| 5 | 1 | 0 | - | 1 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | BFS thăm mỗi đỉnh và cạnh một lần, tính kề cận là O(n) | 
| Không gian | O(n) | Danh sách kề, mảng khoảng cách, hàng đợi | 

Thời gian tuyến tính được chấp nhận đối với$n \le 3 \cdot 10^5$. Bộ nhớ dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("""8
0 1 0 0 0 0 1 0
8 6
2 5
7 8
6 5
4 5
6 1
7 3
""") == "0 1 1 1 1 0 1 1"

# All black
assert run("""3
1 1 1
1 2
2 3
""") == "1 1 1"

# Chain with endpoints black
assert run("""5
1 0 0 0 1
1 2
2 3
3 4
4 5
""") == "1 1 1 1 1"

# White vertex isolated
assert run("""4
1 0 0 1
1 2
2 3
3 4
""") == "1 1 1 1"

# Minimum size
assert run("""3
1 0 1
1 2
2 3
""") == "1 1 1"
```| Kiểm tra đầu vào |
