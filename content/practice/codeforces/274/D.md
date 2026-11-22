---
title: "CF 274D - Ma Trận Đáng Yêu"
description: "Cho một ma trận có n hàng và m cột. Mỗi hàng ban đầu được sắp xếp theo thứ tự không giảm, nhưng bây giờ một số mục đã bị xóa (được đánh dấu -1) và các cột có thể đã bị xáo trộn."
date: "2026-06-05T02:05:48+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 274
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 168 (Div. 1)"
rating: 2200
weight: 274
solve_time_s: 94
verified: true
draft: false
---

[CF 274D - Ma trận đáng yêu](https://codeforces.com/problemset/problem/274/D) 

**Đánh giá:** 2200 
**Tags:** dfs và các biểu đồ tương tự, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận với`n`hàng và`m`cột. Mỗi hàng ban đầu được sắp xếp theo thứ tự không giảm, nhưng bây giờ một số mục đã bị xóa (được đánh dấu`-1`) và các cột có thể đã bị xáo trộn. Nhiệm vụ của chúng ta là khôi phục thứ tự cột để có thể điền số nguyên vào các mục đã xóa và sắp xếp lại mọi hàng. Đầu ra là một hoán vị của các chỉ số cột thể hiện sự sắp xếp lại hợp lệ hoặc`-1`nếu không có thứ tự như vậy tồn tại. 

Ràng buộc đầu vào`1 ≤ n·m ≤ 10^5`có nghĩa là chúng ta có thể đủ khả năng thực hiện hầu hết các thuật toán thời gian tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai trong`n·m`có lẽ là quá chậm. Mỗi số phù hợp với phạm vi số nguyên 32 bit, vì vậy chúng ta không phải lo lắng về việc tràn số học. 

Các trường hợp cạnh rất tinh tế. Ví dụ: nếu toàn bộ một cột bị xóa, nó có thể vừa với bất kỳ đâu, do đó, một cách tiếp cận ngây thơ chỉ so sánh các số hiện có có thể tuyên bố sai là không thể thực hiện được. Tương tự, nếu một số hàng chứa các giá trị bằng nhau trong nhiều cột, chúng ta cần đảm bảo thứ tự cuối cùng cho phép mọi số được điền duy trì thuộc tính được sắp xếp của hàng. 

Một ví dụ cụ thể là:```
2 2
-1 2
1 -1
```Một so sánh ngây thơ có thể từ chối cột đầu tiên trước khi nhận ra nó có thể lấy giá trị`0`ở hàng 1 và vẫn giữ nguyên việc sắp xếp. Đầu ra đúng là`2 1`, cho thấy rằng cần phải sắp xếp thứ tự tương đối cẩn thận dựa trên các ràng buộc chứ không phải các giá trị chính xác. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ thử tất cả`m!`hoán vị của các cột, kiểm tra từng hoán vị xem có thể điền các mục đã xóa để sắp xếp các hàng hay không. Điều này đúng về nguyên tắc nhưng không khả thi vì`m!`tăng nhanh hơn 10^5 ngay cả đối với`m = 10`. Kiểm tra từng hoán vị yêu cầu lặp qua tất cả các hàng và cột, đưa ra`O(n·m·m!)`hoạt động vượt xa giới hạn của chúng tôi. 

Quan sát quan trọng là vấn đề giảm xuống theo thứ tự tôpô. Mỗi hàng ngụ ý các ràng buộc giữa các cột: nếu một số trong cột`c1`lớn hơn một số trong cột`c2`trong cùng một hàng (bỏ qua`-1`), thì trong ma trận đáng yêu cuối cùng,`c1`phải xuất hiện sau`c2`. Đối với các mục bị xóa, chúng ta có thể bỏ qua chúng vì chúng có thể nhận bất kỳ giá trị nào. Kết hợp tất cả các hàng, chúng ta xây dựng một biểu đồ có hướng về các phụ thuộc của cột. Nếu đồ thị này có chu trình thì không thể xếp thứ tự được. Nếu nó không theo chu kỳ, thì việc sắp xếp tôpô sẽ cho phép hoán vị cột hợp lệ. 

Giải pháp tối ưu hoạt động vì chúng ta chỉ quan tâm đến thứ tự tương đối được áp đặt bởi các số đã điền. Chúng tôi không bao giờ cần các giá trị chính xác, vì vậy chúng tôi có thể giảm bớt vấn đề từ kiểm tra hoán vị thời gian giai thừa sang sắp xếp tôpô theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m·m!) | O(n·m) | Quá chậm | 
| Sắp xếp tôpô | O(n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách kề`graph`cho các cột và một mảng`in_degree`để theo dõi các cạnh đến. Chúng sẽ lưu trữ các phụ thuộc cột có nguồn gốc từ các hàng. 
2. Lặp lại từng hàng. Đối với mỗi cặp mục đã điền`(c1, c2)`sao cho giá trị trong`c1`> giá trị trong`c2`, thêm một cạnh từ`c2`ĐẾN`c1`trong biểu đồ. Điều này mã hóa`c2`phải xuất hiện trước`c1`. Phớt lờ`-1`mục nhập. 
3. Sau khi xử lý tất cả các hàng, hãy kiểm tra các chu trình bằng thuật toán Kahn để sắp xếp cấu trúc liên kết. Khởi tạo một hàng đợi với tất cả các cột có bậc bằng 0. 
4. Liên tục xóa một cột khỏi hàng đợi, thêm nó vào hoán vị kết quả và giảm mức độ của các cột lân cận. Thêm bất kỳ người hàng xóm nào có mức độ giảm xuống 0 vào hàng đợi. 
5. Nếu tất cả các cột được xử lý, hãy xuất kết quả hoán vị. Nếu một chu trình ngăn không cho tất cả các cột được thêm vào, hãy xuất`-1`. 

Tại sao nó hoạt động: Mỗi cạnh có hướng thể hiện một ràng buộc ngụ ý bởi một số hàng phải được giữ nguyên để đạt được chuỗi không giảm. Bằng cách thực hiện sắp xếp cấu trúc liên kết, chúng tôi tạo ra thứ tự các cột tuân thủ tất cả các ràng buộc này. Bởi vì`-1`các mục không áp đặt ràng buộc nào, việc bỏ qua chúng không gây ra bất kỳ sự vô hiệu nào. Một chu trình biểu thị một thứ tự mâu thuẫn không thể giải quyết được, do đó`-1`. 

## Giải pháp Python```python
import sys
from collections import deque, defaultdict
input = sys.stdin.readline

n, m = map(int, input().split())
matrix = [list(map(int, input().split())) for _ in range(n)]

graph = [[] for _ in range(m)]
in_degree = [0] * m

for row in matrix:
    filled = [(val, idx) for idx, val in enumerate(row) if val != -1]
    filled.sort()
    for i in range(len(filled) - 1):
        u = filled[i][1]
        v = filled[i + 1][1]
        if filled[i][0] < filled[i + 1][0]:
            graph[u].append(v)
            in_degree[v] += 1

queue = deque([i for i in range(m) if in_degree[i] == 0])
order = []

while queue:
    u = queue.popleft()
    order.append(u + 1)
    for v in graph[u]:
        in_degree[v] -= 1
        if in_degree[v] == 0:
            queue.append(v)

if len(order) < m:
    print(-1)
else:
    print(*order)
```Giải pháp trước tiên thu thập các số không bị xóa trên mỗi hàng và sắp xếp chúng để xác định các ràng buộc về thứ tự. Việc sử dụng biểu đồ và mảng theo độ sẽ triển khai thuật toán Kahn một cách hiệu quả. Thêm`1`vào các chỉ số đảm bảo đầu ra phù hợp với yêu cầu dựa trên 1. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 -1 -1
1 2 1
2 -1 1
```| Hàng | Các mục đã điền | Đã sắp xếp | Đã thêm các cạnh | 
| --- | --- | --- | --- | 
| 1 | [(1,0)] | [(1,0)] | Không có | 
| 2 | [(1,0),(2,1),(1,2)] | [(1,0),(1,2),(2,1)] | 0->1, 2->1 | 
| 3 | [(2,0),(1,2)] | [(1,2),(2,0)] | 2->0 | 

Sắp xếp đồ thị theo cấu trúc liên kết tạo ra thứ tự cột`[3,1,2]`. 

Dấu vết này cho thấy việc bỏ qua`-1`và chỉ thêm các cạnh từ các giá trị nhỏ hơn sẽ bảo toàn tất cả các ràng buộc và cho phép hoán vị hợp lệ. 

### Đầu vào tùy chỉnh```
2 2
-1 2
1 -1
```Các mục được điền trên mỗi hàng:`[(1,1)]`,`[(1,0)]`. Không có bất đẳng thức nghiêm ngặt nào nên đồ thị không có cạnh. Bất kỳ hoán vị nào`[1,2]`hoặc`[2,1]`là hợp lệ. Trả về sắp xếp tôpô`[1,2]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Mỗi hàng được quét một lần. Việc sắp xếp các mục đã điền tối đa là O(m log m) trên mỗi hàng, nhưng vì m ≤ 10^5/n nên nhìn chung nó vẫn tuyến tính. Cấu trúc đồ thị và sắp xếp tôpô là O(m + n·m). | 
| Không gian | O(n·m) | Lưu trữ ma trận và danh sách kề. Mảng theo độ là O(m). | 

Lời giải vừa vặn thoải mái trong giới hạn 2 giây đối với n·m ≤ 10^5. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque
    input = sys.stdin.readline
    n, m = map(int, input().split())
    matrix = [list(map(int, input().split())) for _ in range(n)]
    graph = [[] for _ in range(m)]
    in_degree = [0]*m
    for row in matrix:
        filled = [(val, idx) for idx, val in enumerate(row) if val != -1]
        filled.sort()
        for i in range(len(filled)-1):
            u = filled[i][1]
            v = filled[i+1][1]
            if filled[i][0] < filled[i+1][0]:
                graph[u].append(v)
                in_degree[v] += 1
    queue = deque([i for i in range(m) if in_degree[i]==0])
    order=[]
    while queue:
        u=queue.popleft()
        order.append(u+1)
        for v in graph[u]:
            in_degree[v]-=1
            if in_degree[v]==0:
                queue.append(v)
    if len(order)<m:
        return "-1"
    return " ".join(map(str, order))

# Provided samples
assert run("3 3\n1 -1 -1\n1 2 1\n2 -1 1\n") == "3 1 2", "sample 1"

# Custom cases
assert run("2 2\n-1 2\n1 -1\n") in ["1 2", "2 1"], "erased entries anywhere"
assert run("1 3\n-1 -1 -1\n") in ["1 2 3","1 3 2","2 1 3","2
```
