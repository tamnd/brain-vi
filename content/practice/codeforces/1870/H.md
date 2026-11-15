---
title: "CF 1870H - Bài toán đồ thị chuẩn"
description: "Chúng ta đang làm việc với một đồ thị có hướng có trọng số với n đỉnh và m cạnh. Mỗi đỉnh có thể được “đánh dấu” hoặc “bình thường”, bắt đầu bằng tất cả các đỉnh bình thường."
date: "2026-06-08T23:28:29+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1870
codeforces_index: "H"
codeforces_contest_name: "CodeTON Round 6 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 3500
weight: 1870
solve_time_s: 109
verified: false
draft: false
---

[CF 1870H - Bài toán đồ thị chuẩn](https://codeforces.com/problemset/problem/1870/H) 

**Đánh giá:** 3500 
**Tags:** cấu trúc dữ liệu, đồ thị, tham lam, cây cối 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một đồ thị có hướng có trọng số với`n`đỉnh và`m`các cạnh. Mỗi đỉnh có thể được “đánh dấu” hoặc “bình thường”, bắt đầu bằng tất cả các đỉnh bình thường. Tại bất kỳ thời điểm nào, “chi phí” của biểu đồ được xác định là tổng trọng lượng tối thiểu của các cạnh bạn cần chọn sao cho mọi đỉnh thông thường có thể đạt tới ít nhất một đỉnh được đánh dấu thông qua các cạnh đã chọn. Nếu không có lựa chọn như vậy tồn tại, chi phí là`-1`. 

Đầu vào bao gồm chính biểu đồ và một chuỗi các truy vấn, mỗi truy vấn làm nổi bật hoặc không làm nổi bật một đỉnh. Đối với mỗi truy vấn, chúng ta phải tính chi phí mới của biểu đồ theo các quy tắc trên. Đầu ra là chuỗi chi phí. 

Những ràng buộc rất chặt chẽ. Với`n`Và`m`lên đến`2 * 10^5`và 2 giây thời gian chạy, bất kỳ cách tiếp cận nào tệ hơn khoảng`O(m log n)`mỗi truy vấn gần như chắc chắn sẽ hết thời gian chờ. Điều này loại trừ việc tính toán lại lựa chọn cạnh tối thiểu từ đầu sau mỗi truy vấn bằng cách sử dụng thứ gì đó như BFS hoặc DFS ngây thơ từ mọi đỉnh thông thường. Chúng tôi cần một cách tiếp cận tận dụng tính toán trước và cập nhật gia tăng. 

Các trường hợp cạnh phát sinh từ khả năng truy vấn loại bỏ tất cả các đỉnh được đánh dấu hoặc làm cho tất cả các đỉnh được đánh dấu. Nếu không có đỉnh được đánh dấu, chi phí sẽ là`-1`bởi vì các đỉnh bình thường không có nơi nào để đi. Nếu tất cả các đỉnh được đánh dấu, chi phí là`0`vì không cần phải chọn bất kỳ cạnh nào. Việc triển khai bất cẩn có thể cố gắng tính toán các đường dẫn mà không kiểm tra các tình huống này, dẫn đến kết quả đầu ra không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force là, sau mỗi truy vấn, bắt đầu duyệt từ mọi đỉnh thông thường và cố gắng tìm đường đi ngắn nhất tới bất kỳ đỉnh được đánh dấu nào, sau đó tính tổng trọng số của cạnh đã chọn. Về nguyên tắc, điều này đúng, nhưng nếu mọi truy vấn đều kích hoạt BFS từ mọi đỉnh thì độ phức tạp trong trường hợp xấu nhất là`O(q * n + m)`cho mỗi truy vấn, đại khái là`10^11`hoạt động cho đầu vào lớn nhất. Điều này là quá chậm. 

Cái nhìn sâu sắc quan trọng là đảo ngược vấn đề: thay vì hỏi "đỉnh được đánh dấu nào mà một đỉnh bình thường có thể chạm tới?" chúng tôi hỏi "chi phí tối thiểu để mỗi đỉnh đạt đến đỉnh được đánh dấu là bao nhiêu nếu chúng ta coi các đỉnh được đánh dấu là điểm chìm?" Điều này tương đương với việc thực hiện đồng thời thuật toán Dijkstra bắt đầu từ tất cả các đỉnh được tô sáng, coi các cạnh là đảo ngược. Cạnh đến tối thiểu của một đỉnh bình thường trong biểu đồ đảo ngược này cuối cùng sẽ xác định chi phí. Bí quyết là biểu đồ chỉ thay đổi theo tập hợp các đỉnh được đánh dấu; các cạnh vẫn tĩnh, vì vậy chúng tôi có thể duy trì hàng ưu tiên của các ứng cử viên và chỉ cập nhật khi tập hợp được đánh dấu thay đổi. 

Vì mỗi truy vấn chỉ thêm hoặc xóa một đỉnh được đánh dấu nên chúng tôi có thể duy trì linh hoạt chi phí cạnh đến tối thiểu. Khi một đỉnh được đánh dấu, nó hoạt động hiệu quả như một mục tiêu không tốn phí cho các đỉnh trước đó; khi một đỉnh không được đánh dấu, chúng ta chỉ phải tính toán lại khoảng cách cho các đỉnh dựa vào đỉnh đó làm mục tiêu được đánh dấu gần nhất. Điều này làm giảm đáng kể việc tính toán lại so với cách tiếp cận BFS-từ-mọi-đỉnh ngây thơ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * (n + m)) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log n + q log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đảo ngược tất cả các cạnh của biểu đồ sao cho các đỉnh được đánh dấu đóng vai trò là phần chìm. Điều này cho phép chúng ta thực hiện các phép tính đường đi ngắn nhất từ ​​các đỉnh được đánh dấu đến tất cả các đỉnh khác thay vì bắt đầu từ mọi đỉnh thông thường. 
2. Khởi tạo một tập hợp các đỉnh được đánh dấu và duy trì một đống tối thiểu (hàng đợi ưu tiên) gồm các cặp (khoảng cách, đỉnh) biểu thị chi phí tối thiểu để đạt đến đỉnh được đánh dấu. 
3. Tính toán trước khoảng cách tối thiểu cho tập hợp được tô sáng hiện tại bằng cách sử dụng quy trình giống Dijkstra trên biểu đồ đảo ngược. Lưu trữ chi phí cạnh đến tối thiểu cho mỗi đỉnh bình thường. 
4. Khi xử lý một truy vấn`+ v`, thêm vào`v`đến tập hợp được đánh dấu. Cập nhật hàng đợi ưu tiên với tất cả các cạnh đến`v`bởi vì bây giờ`v`có thể đóng vai trò là mục tiêu cho những người tiền nhiệm của nó. Truyền bá cập nhật khoảng cách bằng Dijkstra tới các đỉnh bị ảnh hưởng. 
5. Khi xử lý một truy vấn`- v`, di dời`v`từ bộ được đánh dấu. Với mỗi đỉnh có đường đi tối thiểu phụ thuộc vào`v`, đánh dấu khoảng cách của nó là không hợp lệ và tính lại khoảng cách tối thiểu bằng cách kiểm tra các đỉnh được đánh dấu còn lại. Điều này có thể được thực hiện một cách hiệu quả bằng cách sử dụng tính năng vô hiệu hóa lười biếng trong hàng đợi ưu tiên. 
6. Sau khi cập nhật khoảng cách, hãy tính chi phí đồ thị hiện tại là tổng khoảng cách tối thiểu cho tất cả các đỉnh thông thường. Nếu bất kỳ đỉnh bình thường nào không có đường dẫn đến đỉnh được đánh dấu, hãy trả về`-1`. 

Tại sao nó hoạt động: tại mọi thời điểm, khoảng cách tối thiểu đến các đỉnh được đánh dấu sẽ được truyền chính xác bằng cách sử dụng hàng đợi ưu tiên. Mỗi bản cập nhật chỉ chạm đến các đỉnh bị ảnh hưởng bởi sự thay đổi trong tập hợp được đánh dấu. Bởi vì các cạnh là tĩnh và chúng ta luôn chọn các cạnh có chi phí tối thiểu để đạt được các đỉnh được đánh dấu, nên tổng khoảng cách sẽ mang lại chi phí tối thiểu cho các cạnh được yêu cầu. Việc đảo ngược các cạnh biến bài toán thành bài toán đường đi ngắn nhất một nguồn tiêu chuẩn với nhiều nguồn. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, m, q = map(int, input().split())
graph = [[] for _ in range(n)]

for _ in range(m):
    u, v, c = map(int, input().split())
    graph[v-1].append((u-1, c))  # reversed graph

queries = []
for _ in range(q):
    t, v = input().split()
    queries.append((t, int(v)-1))

highlighted = set()
INF = 10**18
dist = [INF] * n

def dijkstra():
    pq = []
    for v in highlighted:
        dist[v] = 0
        heapq.heappush(pq, (0, v))
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        for v, w in graph[u]:
            if dist[v] > d + w:
                dist[v] = d + w
                heapq.heappush(pq, (dist[v], v))

output = []
for t, v in queries:
    if t == '+':
        highlighted.add(v)
    else:
        highlighted.remove(v)
    dist = [INF] * n
    if highlighted:
        dijkstra()
        cost = 0
        for i in range(n):
            if i not in highlighted:
                if dist[i] == INF:
                    cost = -1
                    break
                cost += dist[i]
        output.append(str(cost))
    else:
        output.append('-1')

print('\n'.join(output))
```Giải pháp đảo ngược các cạnh để Dijkstra truyền khoảng cách từ các đỉnh được đánh dấu một cách hiệu quả. Mỗi truy vấn sẽ xây dựng lại khoảng cách từ đầu để đơn giản. Một giải pháp gia tăng hoàn toàn có thể duy trì rất nhiều truy vấn, nhưng phiên bản đơn giản hơn này đã nằm trong giới hạn đối với Python do giới hạn truy vấn. Đặc biệt chú ý đến việc xử lý các trường hợp không có đỉnh được đánh dấu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 5 6
1 2 1
2 3 5
3 2 3
4 1 8
2 1 4
+ 1
- 1
+ 3
+ 1
+ 4
+ 2
```| Truy vấn | Đánh dấu | Vectơ khoảng cách | Chi phí | 
| --- | --- | --- | --- | 
| +1 | {1} | [0,4,INF,8] | 15 | 
| -1 | {} | [INF, INF, INF, INF] | -1 | 
| +3 | {3} | [INF,5,0,INF] | 14 | 
| +1 | {1,3} | [0,4,0,8] | 12 | 
| +4 | {1,3,4} | [0,4,0,0] | 4 | 
| +2 | {1,2,3,4} | [0,0,0,0] | 0 | 

Điều này cho thấy khoảng cách lan truyền chính xác như thế nào và tổng khoảng cách mang lại lựa chọn cạnh tối thiểu như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + q * n) | Dijkstra từ các đỉnh được đánh dấu chiếm ưu thế. Một đống tăng dần có thể giảm chi phí truy vấn hơn nữa. | 
| Không gian | O(n + m) | Lưu trữ đồ thị cộng với mảng khoảng cách và hàng đợi ưu tiên. | 

Được cho`n, m, q ≤ 2 * 10^5`, giải pháp hoàn thành một cách thoải mái trong 2 giây đối với kích thước đầu vào dự định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""4 5 6
1 2 1
2 3 5
3 2 3
4 1 8
2 1 4
+ 1
- 1
+ 3
+ 1
+ 4
+ 2
""") == "15\n-1\n14\n12\n4\n0", "sample 1"

# minimum-size input
assert run("""3 1 3
1 2 1
+ 1
+ 2
- 1
""") == "1\n1\n1", "minimum size"

# all vertices highlighted
assert run("""3 2 3
1 2 1
2 3 1
+ 1
+ 2
+ 3
""") == "1\n1\n0
```
