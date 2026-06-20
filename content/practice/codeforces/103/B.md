---
title: "CF 103B - Cthulhu"
description: "Chúng ta được cung cấp một đồ thị vô hướng và cần quyết định xem nó có khớp với một cấu trúc rất cụ thể hay không. Đồ thị phải chứa chính xác một chu trình đơn giản và mọi đỉnh khác phải thuộc về một cây gắn với một đỉnh nào đó trên chu trình đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 103
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 80 (Div. 1 Only)"
rating: 1500
weight: 103
solve_time_s: 103
verified: true
draft: false
---

[CF 103B - Cthulhu](https://codeforces.com/problemset/problem/103/B) 

**Đánh giá:** 1500 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng và cần quyết định xem nó có khớp với một cấu trúc rất cụ thể hay không. 

Đồ thị phải chứa chính xác một chu trình đơn giản và mọi đỉnh khác phải thuộc về một cây gắn với một đỉnh nào đó trên chu trình đó. Một cách khác để mô tả nó là đồ thị được kết nối và có đúng một chu trình. 

Tuyên bố này diễn đạt điều này như một số cây có rễ nằm trên một chu trình đơn giản. Khi chu trình được cố định, mỗi cạnh phụ phân nhánh ra khỏi nó sẽ tạo thành một cây gắn liền với một trong các đỉnh của chu trình. 

Đầu vào mô tả một đồ thị vô hướng có tối đa 100 đỉnh. Mỗi cạnh kết nối hai đỉnh khác nhau và không có cạnh trùng lặp hoặc vòng tự lặp. 

Các ràng buộc là nhỏ, do đó ngay cả một$O(n^2)$hoặc$O(n^3)$giải pháp sẽ trôi qua một cách thoải mái. Tuy nhiên, cấu trúc đồ thị mang lại đặc tính rõ ràng hơn nhiều dẫn đến một giải pháp tuyến tính đơn giản. 

Quan sát quan trọng là một đồ thị vô hướng liên thông chứa đúng một chu trình khi và chỉ khi:$$m = n$$Ở đâu$n$là số đỉnh và$m$là số cạnh. 

Đồ thị liên thông với$n$đỉnh và$n-1$các cạnh là một cái cây. Thêm một cạnh nữa sẽ tạo ra đúng một chu trình. Đó chính xác là cấu trúc được yêu cầu ở đây. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai bất cẩn. 

Hãy xem xét một biểu đồ bị ngắt kết nối trong đó một thành phần có tính tuần hoàn. 

đầu vào:```
4 4
1 2
2 3
3 1
4 4
```Ví dụ này thực sự không hợp lệ vì các vòng lặp tự bị cấm, nhưng ý tưởng quan trọng là một biểu đồ bị ngắt kết nối chứa một chu trình ở đâu đó. Một kiểm tra ngây thơ chỉ`m == n`sẽ chấp nhận không chính xác các đồ thị bị ngắt kết nối. 

Một ví dụ bị ngắt kết nối hợp lệ là:```
4 3
1 2
2 3
3 1
```Đầu ra đúng là:```
NO
```Đồ thị có chu trình nhưng đỉnh 4 bị cô lập nên đồ thị không liên thông. 

Một trường hợp phức tạp khác là đồ thị liên thông có nhiều hơn một chu trình. 

đầu vào:```
4 5
1 2
2 3
3 1
3 4
4 1
```Đầu ra đúng là:```
NO
```Đồ thị này được kết nối, nhưng nó chứa hai chu trình. Một DFS chỉ kiểm tra kết nối sẽ thất bại ở đây. 

Trường hợp quan trọng thứ ba là cây thuần chủng. 

đầu vào:```
5 4
1 2
2 3
3 4
4 5
```Đầu ra đúng là:```
NO
```Đồ thị được kết nối, nhưng nó không có chu trình nào cả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng phát hiện rõ ràng các chu kỳ và xác minh cấu trúc sau khi loại bỏ chúng. Một phương pháp khả thi là liệt kê các cạnh, tạm thời loại bỏ từng cạnh và kiểm tra xem biểu đồ có trở thành cây hay không. Vì các ràng buộc rất nhỏ nên điều này có hiệu quả. 

Đối với mỗi cạnh, chúng ta có thể: 

1. Loại bỏ cạnh. 
2. Chạy DFS hoặc BFS để kiểm tra kết nối. 
3. Xác minh rằng đồ thị còn lại là không có chu kỳ. 

Việc này tốn khoảng$O(m \cdot (n + m))$. Với$n \le 100$, thế này vẫn đủ nhanh. 

Vấn đề trở nên đơn giản hơn nhiều khi chúng ta nhớ đến thuộc tính đồ thị cổ điển. 

Đối với đồ thị vô hướng: 

- Một cái cây có chính xác$n-1$các cạnh. 
- Mỗi cạnh phụ thêm vào một chu trình bổ sung. 

Vậy một đồ thị liên thông có chính xác$n$các cạnh có đúng một chu trình. 

Điều đó hoàn toàn phù hợp với cấu trúc "chu kỳ có cây kèm theo" cần thiết. Mỗi đỉnh không nằm trên chu trình phải thuộc về một cây treo trên nó, vì chu trình thứ hai sẽ cần thêm một cạnh nữa. 

Điều này làm giảm toàn bộ vấn đề thành hai kiểm tra: 

1. Đồ thị phải được kết nối. 
2. Đồ thị phải thỏa mãn$m = n$. 

Kết nối được xác minh bằng một DFS hoặc BFS duy nhất. Số cạnh đã được đưa ra trong đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m(n + m)) | O(n + m) | Đã chấp nhận | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và xây dựng danh sách kề. 

Danh sách kề cho phép truyền tải DFS hiệu quả trong thời gian tuyến tính. 
2. Kiểm tra xem số cạnh có bằng số đỉnh hay không. 

Đồ thị vô hướng liên thông có đúng một chu trình phải thỏa mãn$m = n$. Nếu điều kiện này không thành công, chúng ta có thể in ngay`"NO"`. 
3. Chạy DFS từ bất kỳ đỉnh nào, ví dụ đỉnh 1. 

DFS đánh dấu mọi đỉnh có thể tiếp cận. Vì biểu đồ là vô hướng và tính kết nối đóng vai trò quan trọng trên toàn cầu nên chỉ cần duyệt một lần là đủ. 
4. Đếm xem có bao nhiêu đỉnh đã được thăm. 

Nếu một số đỉnh vẫn chưa được thăm, biểu đồ sẽ bị ngắt kết nối và không thể khớp với cấu trúc được yêu cầu. 
5. Nếu cả hai điều kiện đều đúng, hãy in`"FHTAGN!"`. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một định lý đồ thị tiêu chuẩn. 

Đồ thị vô hướng liên thông với$n$đỉnh và$n-1$các cạnh là một cái cây. Việc thêm chính xác một cạnh phụ sẽ tạo ra đúng một chu trình đơn giản. 

Vì vậy: 

- Nếu đồ thị liên thông và$m = n$, nó chứa đúng một chu trình. 
- Mọi cạnh còn lại đều thuộc về các cây gắn liền với chu trình đó. 
- Đây chính xác là định nghĩa cần có trong bài toán. 

Ngược lại, mọi đồ thị Cthulhu hợp lệ đều chứa một chu trình cộng với các nhánh cây, do đó nó phải được kết nối và phải chứa đúng một cạnh hơn một cây, nghĩa là$m = n$. 

Cả hai điều kiện đều cần và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dfs(node, graph, visited):
    visited[node] = True

    for nei in graph[node]:
        if not visited[nei]:
            dfs(nei, graph, visited)

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    if m != n:
        print("NO")
        return

    visited = [False] * (n + 1)

    dfs(1, graph, visited)

    for node in range(1, n + 1):
        if not visited[node]:
            print("NO")
            return

    print("FHTAGN!")

solve()
```Phần đầu tiên xây dựng danh sách kề. Vì đồ thị là vô hướng nên mọi cạnh đều được chèn theo cả hai hướng. 

điều kiện`m != n`được kiểm tra trước DFS vì nó loại bỏ ngay các trường hợp không thể thực hiện được. Điều này tránh được công việc truyền tải không cần thiết. 

DFS đánh dấu tất cả các đỉnh có thể truy cập được từ đỉnh 1. Nếu đồ thị được kết nối, mọi đỉnh cuối cùng đều phải được thăm. 

Vòng lặp trên tất cả các đỉnh xác minh kết nối một cách rõ ràng. Thiếu bước này là một lỗi phổ biến vì chỉ phát hiện một chu trình thôi là chưa đủ. 

Kích thước biểu đồ rất nhỏ nên độ sâu đệ quy ở đây hoàn toàn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 6
6 3
6 4
5 1
2 5
1 4
5 4
```Đồ thị có: 

-$n = 6$-$m = 6$Vì vậy, điều kiện đếm cạnh đã khớp. 

Truyền tải DFS: 

| Bước | Nút hiện tại | Mới Truy Cập | Đã truy cập Bộ | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | {1} | 
| 2 | 5 | 5 | {1,5} | 
| 3 | 2 | 2 | {1,2,5} | 
| 4 | 4 | 4 | {1,2,4,5} | 
| 5 | 6 | 6 | {1,2,4,5,6} | 
| 6 | 3 | 3 | {1,2,3,4,5,6} | 

Tất cả các đỉnh đều có thể truy cập được. 

Đầu ra:```
FHTAGN!
```Dấu vết này thể hiện tính bất biến trung tâm: tính kết nối cộng$m=n$đảm bảo chính xác một chu kỳ. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
3 4
4 5
```Đây:

-$n = 5$-$m = 4$Thuật toán dừng ngay lập tức vì`m != n`. 

| Biến | Giá trị | 
| --- | --- | 
| n | 5 | 
| m | 4 | 
| Điều kiện m == n | Sai | 

Đầu ra:```
NO
```Ví dụ này cho thấy tại sao việc kiểm tra kết nối đơn giản là không đủ. Đồ thị được kết nối nhưng nó chỉ là một cái cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | DFS thăm mọi đỉnh và cạnh một lần | 
| Không gian | O(n + m) | Danh sách kề và mảng đã thăm | 

Với tối đa 100 đỉnh, giải pháp này thấp hơn nhiều so với giới hạn. Các thuật toán thậm chí chậm hơn nhiều cũng có thể vượt qua, nhưng cách tiếp cận tuyến tính vừa đơn giản hơn vừa sạch hơn về mặt toán học. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def dfs(node, graph, visited):
        visited[node] = True

        for nei in graph[node]:
            if not visited[nei]:
                dfs(nei, graph, visited)

    def solve():
        n, m = map(int, input().split())

        graph = [[] for _ in range(n + 1)]

        for _ in range(m):
            u, v = map(int, input().split())
            graph[u].append(v)
            graph[v].append(u)

        if m != n:
            return "NO"

        visited = [False] * (n + 1)

        dfs(1, graph, visited)

        for node in range(1, n + 1):
            if not visited[node]:
                return "NO"

        return "FHTAGN!"

    return solve()

# provided sample
assert run(
"""6 6
6 3
6 4
5 1
2 5
1 4
5 4
""") == "FHTAGN!", "sample 1"

# tree, connected but no cycle
assert run(
"""5 4
1 2
2 3
3 4
4 5
""") == "NO", "tree should fail"

# disconnected graph with one cycle
assert run(
"""4 3
1 2
2 3
3 1
""") == "NO", "disconnected graph should fail"

# connected graph with multiple cycles
assert run(
"""4 5
1 2
2 3
3 1
3 4
4 1
""") == "NO", "multiple cycles should fail"

# smallest valid cycle
assert run(
"""3 3
1 2
2 3
3 1
""") == "FHTAGN!", "simple cycle should pass"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây 5 đỉnh | KHÔNG | Đồ thị được kết nối không có chu trình bị lỗi | 
| Tam giác ngắt kết nối | KHÔNG | Cần có kết nối | 
| Đồ thị hai chu kỳ | KHÔNG | Nhiều hơn một chu kỳ không hợp lệ | 
| Tam giác | FHTAGN! | Cấu trúc hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Một đồ thị bị ngắt kết nối có đúng một chu trình có thể đánh lừa các giải pháp chỉ kiểm tra`m == n`. 

đầu vào:```
6 6
1 2
2 3
3 1
4 5
5 6
6 4
```Đồ thị có hai thành phần bị ngắt kết nối. DFS bắt đầu từ đỉnh 1 chỉ truy cập các đỉnh`{1,2,3}`. 

Việc kiểm tra đã thăm không thành công vì các đỉnh 4, 5 và 6 vẫn chưa được thăm. 

Thuật toán in chính xác:```
NO
```Biểu đồ được kết nối với nhiều chu kỳ có thể đánh lừa các giải pháp chỉ kiểm tra kết nối. 

đầu vào:```
4 5
1 2
2 3
3 1
3 4
4 1
```Đồ thị được kết nối, nhưng:$$m = 5,\quad n = 4$$Từ`m != n`, thuật toán sẽ ngay lập tức bác bỏ nó. 

Đầu ra là:```
NO
```Một cái cây có thể đánh lừa các giải pháp chỉ kiểm tra khả năng kết nối. 

đầu vào:```
4 3
1 2
2 3
3 4
```DFS truy cập mọi nút, do đó biểu đồ được kết nối. Nhưng:$$m = 3,\quad n = 4$$Cạnh phụ bị thiếu có nghĩa là không có chu trình. 

Thuật toán xuất ra chính xác:```
NO
```
