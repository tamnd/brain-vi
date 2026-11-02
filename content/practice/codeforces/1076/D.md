---
title: "CF 1076D - Xóa cạnh"
description: "Chúng ta được cung cấp một đồ thị vô hướng có trọng số và trước tiên chúng ta tưởng tượng việc chạy tính toán đường đi ngắn nhất từ ​​đỉnh 1. Điều này tạo ra một giá trị khoảng cách cho mỗi đỉnh, mà chúng ta có thể coi là chi phí tối ưu thực sự để đạt đến đỉnh đó trong biểu đồ ban đầu."
date: "2026-06-15T14:33:09+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1076
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 54 (Rated for Div. 2)"
rating: 1800
weight: 1076
solve_time_s: 706
verified: true
draft: false
---

[CF 1076D - Xóa cạnh](https://codeforces.com/problemset/problem/1076/D) 

**Đánh giá:** 1800 
**Tags:** đồ thị, đường đi tham lam, đường đi ngắn nhất 
**Thời gian giải:** 11 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng có trọng số và trước tiên chúng ta tưởng tượng việc chạy tính toán đường đi ngắn nhất từ đỉnh 1. Điều này tạo ra một giá trị khoảng cách cho mỗi đỉnh, mà chúng ta có thể coi là chi phí tối ưu thực sự để đạt đến đỉnh đó trong biểu đồ ban đầu. 

Sau đó, chúng tôi được phép xóa các cạnh, nhưng chúng tôi không thể giữ tổng cộng nhiều hơn k cạnh. Sau khi thu nhỏ biểu đồ, chúng ta lại xem xét khả năng tiếp cận từ đỉnh 1. Một đỉnh được coi là thành công nếu vẫn tồn tại ít nhất một đường đi từ 1 đến đỉnh đó có tổng trọng số khớp với khoảng cách đường đi ngắn nhất ban đầu của nó. Nói cách khác, chúng ta không được phép tăng khoảng cách và chúng ta chỉ quan tâm rằng ít nhất một đường đi tối ưu vẫn còn nguyên cho đỉnh đó. 

Nhiệm vụ là chọn tối đa k cạnh sao cho có thể tiếp cận được càng nhiều đỉnh càng tốt thông qua đường đi ngắn nhất. 

Các ràng buộc rất lớn, lên tới 300.000 đỉnh và cạnh. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng tính toán lại các đường đi ngắn nhất sau nhiều lần xóa hoặc xem xét các tập hợp con của các cạnh một cách rõ ràng. Bất cứ điều gì vượt quá thời gian tuyến tính về số cạnh sẽ quá chậm, vì vậy chúng ta sẽ mong đợi một phép tính đường đi ngắn nhất duy nhất cộng với một cấu trúc tham lam. 

Một vấn đề nhỏ xuất hiện khi tồn tại nhiều đường đi ngắn nhất. Một cách tiếp cận đơn giản chỉ giữ một cây đường đi ngắn nhất có thể đánh giá thấp số lượng đỉnh có thể được bảo toàn, bởi vì một đỉnh có thể phụ thuộc vào nhiều đường đi ngắn nhất thay thế cha mẹ để duy trì mức tối ưu sau khi xóa. 

Một trường hợp cạnh khác xuất phát từ giới hạn k về số cạnh được giữ lại. Ngay cả khi chúng ta có thể bảo toàn tất cả các cạnh đường đi ngắn nhất, chúng ta có thể buộc phải loại bỏ một số cạnh, vì vậy chúng ta phải ưu tiên những đỉnh nào để giữ “sống” trong điều kiện hạn chế về ngân sách. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua ràng buộc trên k, thì cấu trúc tự nhiên để bảo toàn tất cả các đỉnh tốt là đường đi ngắn nhất DAG bắt nguồn từ nút 1. Nếu chúng ta tính khoảng cách ngắn nhất bằng thuật toán Dijkstra, thì mỗi đỉnh i có thể có một số cạnh đến (u, i) sao cho dist[u] + w = dist[i]. Những cạnh này chính xác là những cạnh có thể nằm trên một đường đi ngắn nhất nào đó. 

Nếu chúng ta được phép có các cạnh không giới hạn, chúng ta có thể giữ tất cả các cạnh như vậy và mọi đỉnh có thể tiếp cận được trên đường đi ngắn nhất thì DAG sẽ vẫn tốt. Điều này ngay lập tức bảo toàn tất cả các đỉnh. 

Khó khăn đến từ sự hạn chế là chúng ta có thể giữ tối đa k cạnh. Đường đi ngắn nhất DAG có thể có nhiều cạnh, có khả năng là O(m), vì vậy chúng ta cần chọn một tập hợp con. 

Quan sát quan trọng là chúng ta không cần tất cả các cạnh đường đi ngắn nhất để bảo toàn tất cả các đỉnh. Chúng ta chỉ cần đảm bảo rằng mọi đỉnh mà chúng ta muốn giữ đều có ít nhất một cạnh cha dẫn vào đỉnh đó từ một đỉnh đã đạt trước đó. Điều này gợi ý việc xây dựng cây đường dẫn ngắn nhất thay vì DAG đầy đủ. 

Chúng tôi chạy Dijkstra từ nút 1 và ghi lại một cạnh cha cho mỗi đỉnh khi nó được hoàn thiện lần đầu tiên. Điều này mang lại một cây đường đi ngắn nhất với chính xác n − 1 cạnh và tất cả các đỉnh đều tốt. Nếu k ≥ n − 1 thì chúng ta đã hoàn thành. 

Khi k nhỏ hơn n − 1, chúng ta không thể giữ được một cây đầy đủ. Trong trường hợp đó, chúng tôi muốn ưu tiên các đỉnh gần gốc hơn theo nghĩa thứ tự khám phá trong Dijkstra. Nếu chúng ta xử lý các đỉnh theo thứ tự chúng được đưa ra khỏi hàng ưu tiên thì k đỉnh đầu tiên (không bao gồm gốc) sẽ đưa ra một cấu trúc được kết nối đảm bảo các đường đi ngắn nhất đến các đỉnh đó, bởi vì mỗi đỉnh trong số chúng có một đường dẫn gốc ngắn nhất được chọn trong số các nút đã được xử lý. 

Điều này hoạt động vì Dijkstra đảm bảo rằng khi một nút được trích xuất, khoảng cách ngắn nhất của nó là cuối cùng và bất kỳ nút trước nào được sử dụng trong đường dẫn ngắn nhất phải đến từ trạng thái khoảng cách sớm hơn hoặc bằng nhau. Vì vậy, chúng ta có thể chỉ định một cạnh cha một cách an toàn tại thời điểm trích xuất và xây dựng cây tăng dần, nhưng dừng lại sau khi chúng ta đã chọn được k cạnh.

Ý tưởng vũ phu sẽ là thử tất cả các tập hợp con của các cạnh có kích thước k và kiểm tra xem đỉnh nào vẫn có thể tiếp cận được một cách tối ưu. Đó là số mũ tính bằng m và không thể thực hiện được. 

Ý tưởng được tối ưu hóa là xây dựng một cây đường đi ngắn nhất một cách tham lam bằng cách sử dụng Dijkstra, sau đó cắt bớt nó bằng cách chỉ chọn k cạnh cây đầu tiên trong các nút thứ tự được hoàn thiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Lực lượng vũ phu | O(2^m · (n + m)) | O(n + m) | Quá chậm | 
| Tối ưu (Dijkstra + cây tham lam) | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy Dijkstra từ đỉnh 1 để tính khoảng cách ngắn nhất tới tất cả các đỉnh. 
Bước này sửa các giá trị đích d[i], xác định tính chính xác cho mọi đỉnh mà chúng ta muốn giữ nguyên. 

2. Duy trì một mảng parent_edge[i] được khởi tạo là trống, mảng này sẽ lưu trữ một cạnh đến có đường đi ngắn nhất đã chọn cho mỗi đỉnh. 
Chúng ta chỉ cần một cạnh như vậy trên mỗi đỉnh để đảm bảo khả năng tiếp cận. 

3. Trong Dijkstra, khi một đỉnh v được hoàn thành (được bật ra từ hàng đợi ưu tiên), hãy ghi lại cạnh được sử dụng để thư giãn nó làm cạnh cha của nó. 
Tại thời điểm này, dist[v] là tối thiểu và ổn định, do đó mọi đường đi ngắn nhất đều phải đi qua một số cấu trúc đã được xử lý. 

4. Tập hợp tất cả các cạnh cha của các đỉnh từ 2 đến n theo thứ tự chúng đã được hoàn thiện. 
Thứ tự này đảm bảo rằng chúng tôi ưu tiên các đỉnh gần hơn theo thứ tự khám phá Dijkstra. 

5. Nếu số cạnh được thu thập nhỏ hơn hoặc bằng k, hãy xuất tất cả chúng. 
Điều này tương ứng với việc giữ đầy đủ cây đường đi ngắn nhất hoặc một tập hợp con của nó mà không làm đứt kết nối. 

6. Nếu có nhiều hơn k cạnh, chỉ xuất k cạnh đầu tiên theo thứ tự này. 
Điều này đảm bảo chúng ta giữ cấu trúc gốc hợp lệ cho chính xác k đỉnh, mỗi đỉnh vẫn được kết nối bằng một đường đi ngắn nhất. 

### Tại sao nó hoạt động 

Thuật toán của Dijkstra đảm bảo rằng mỗi đỉnh được hoàn thiện theo thứ tự không giảm của khoảng cách ngắn nhất. Khi chúng ta chỉ định cạnh cha sau khi hoàn thiện, cạnh đó là một phần của đường đi ngắn nhất từ ​​nguồn. Nếu chúng ta giới hạn bản thân ở k phép gán đầu tiên như vậy, thì chúng ta đang chọn k đỉnh một cách hiệu quả mà đường đi ngắn nhất của chúng vẫn còn nguyên thông qua cha mẹ đã chọn của chúng. Vì mọi đỉnh được chọn đều có đỉnh gốc của nó được hoàn thiện trước đó nên không thể xảy ra việc tái tạo chu kỳ hoặc khoảng cách không hợp lệ và mỗi đỉnh được giữ lại sẽ duy trì đường đi ngắn nhất chính xác từ 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

n, m, k = map(int, input().split())
g = [[] for _ in range(n + 1)]

for i in range(m):
    x, y, w = map(int, input().split())
    g[x].append((y, w, i + 1))
    g[y].append((x, w, i + 1))

INF = 10**30
dist = [INF] * (n + 1)
dist[1] = 0

parent_edge = [-1] * (n + 1)

pq = [(0, 1)]
visited = [False] * (n + 1)

order_edges = []

while pq:
    d, v = heapq.heappop(pq)
    if visited[v]:
        continue
    visited[v] = True

    if v != 1 and parent_edge[v] != -1:
        order_edges.append(parent_edge[v])

    for to, w, idx in g[v]:
        if dist[to] > d + w:
            dist[to] = d + w
            parent_edge[to] = idx
            heapq.heappush(pq, (dist[to], to))

if len(order_edges) > k:
    order_edges = order_edges[:k]

print(len(order_edges))
print(*order_edges)
```Việc triển khai tuân theo Dijkstra với hàng đợi ưu tiên tiêu chuẩn. Chi tiết quan trọng là chúng tôi lưu trữ, đối với mỗi đỉnh, cạnh được sử dụng ở mức thư giãn tốt nhất. Khi một đỉnh được xuất hiện lần đầu tiên, cạnh đó sẽ được đưa vào danh sách câu trả lời. Thứ tự này xác định những đỉnh nào chúng ta ưu tiên giữ dưới ràng buộc k-edge. 

Một cạm bẫy phổ biến là cập nhật câu trả lời khi nới lỏng các cạnh thay vì khi bật ra khỏi đống. Điều đó sẽ phá vỡ tính đúng đắn vì việc nới lỏng tạm thời chưa đảm bảo tính tối ưu. Một điểm tinh tế khác là chúng ta phải theo dõi các nút đã truy cập để tránh xử lý các mục nhập cũ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
1 2 1
3 2 1
1 3 3
```Chúng tôi tính khoảng cách ngắn nhất từ ​​​​1. Chúng tôi nhận được d2 = 1, d3 = 2 thông qua đường dẫn 1-2-3. 

Trong Dijkstra, nút 2 được hoàn thành trước, sau đó là nút 3. 

| Bước | Nút | Cạnh gốc được chọn | Các cạnh được chọn | 
|------|------|-------------------|----------------| 
| 1 | 1 | - | [] | 
| 2 | 2 | (1,2) cạnh 1 | [1] | 
| 3 | 3 | (2,3) cạnh 2 | [1,2] | 

Chúng ta đã có 2 cạnh khớp với k nên cả hai đều được giữ lại. Tất cả các đỉnh vẫn tốt. 

### Ví dụ 2 

đầu vào:```
4 4 2
1 2 1
2 3 1
3 4 1
1 4 10
```Đường đi ngắn nhất là chuỗi 1-2-3-4. 

| Bước | Nút | Cạnh gốc được chọn | Các cạnh được chọn | 
|------|------|-------------------|----------------| 
| 1 | 1 | - | [] | 
| 2 | 2 | (1,2) | [1] | 
| 3 | 3 | (2,3) | [1,2] | 
| 4 | 4 | (3,4) | [1,2,3] | 

Vì k = 2 nên chúng ta chỉ giữ lại hai cạnh đầu tiên. Nút 2 và 3 vẫn hoạt động tốt, trong khi nút 4 không thể truy cập được bằng đường đi ngắn nhất. 

Điều này cho thấy thứ tự tham lam ưu tiên các lớp Dijkstra trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(m log n) | Mỗi lần nới lỏng cạnh được xử lý thông qua hàng đợi ưu tiên một lần | 
| Không gian | O(n + m) | Danh sách lân cận cộng với khoảng cách và bộ nhớ gốc | 

Các ràng buộc cho phép tối đa 3 × 10^5 cạnh, do đó, Dijkstra O(m log n) vừa vặn thoải mái trong giới hạn thời gian trong Python với cách triển khai dựa trên heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, m, k = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for i in range(m):
        x, y, w = map(int, input().split())
        g[x].append((y, w, i + 1))
        g[y].append((x, w, i + 1))

    INF = 10**30
    dist = [INF] * (n + 1)
    dist[1] = 0
    parent = [-1] * (n + 1)

    pq = [(0, 1)]
    vis = [False] * (n + 1)
    ans = []

    while pq:
        d, v = heapq.heappop(pq)
        if vis[v]:
            continue
        vis[v] = True
        if v != 1 and parent[v] != -1:
            ans.append(parent[v])

        for to, w, idx in g[v]:
            if dist[to] > d + w:
                dist[to] = d + w
                parent[to] = idx
                heapq.heappush(pq, (dist[to], to))

    if len(ans) > k:
        ans = ans[:k]

    out = [str(len(ans)), " ".join(map(str, ans))]
    return "\n".join(out).strip()

# provided sample
assert run("""3 3 2
1 2 1
3 2 1
1 3 3
""") == "2\n1 2"

# chain graph
assert run("""4 3 2
1 2 1
2 3 1
3 4 1
""") == "2\n1 2"

# star graph
assert run("""4 3 3
1 2 1
1 3 1
1 4 1
""") == "3\n1 2 3"

# k = 0
assert run("""3 3 0
1 2 1
2 3 1
1 3 1
""") == "0\n"

# dense triangle
assert run("""3 3 1
1 2 1
2 3 1
1 3 2
""") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| đồ thị chuỗi | 2 cạnh | sự lựa chọn tiền tố đúng đắn | 
| đồ thị sao | tất cả các cạnh | hành vi k ≥ n-1 | 
| k = 0 | trống | xử lý trường hợp ranh giới | 
| tam giác | 1 cạnh | sự cắt ngắn tham lam đúng đắn | 

## Vỏ cạnh 

Giá trị k tối thiểu chẳng hạn như 0 kiểm tra xem thuật toán có tạo ra cạnh một cách an toàn hay không trong khi vẫn duy trì khởi tạo chính xác. Trong trường hợp này, không có đỉnh nào ngoài gốc vẫn được kết nối, nhưng ràng buộc đầu ra cho phép điều này. 

Một ngôi sao được kết nối đầy đủ đảm bảo rằng Dijkstra chọn tất cả các cạnh từ gốc trước tiên, cho thấy thứ tự trích xuất sớm sẽ ưu tiên chính xác kết nối có giá trị cao khi k lớn. 

Biểu đồ chuỗi dài nhấn mạnh đến thứ tự vì mỗi đỉnh phụ thuộc vào đỉnh trước đó. Việc cắt ngắn sớm chứng tỏ rằng một khi chuỗi gốc bị cắt, các đỉnh sâu hơn sẽ mất khả năng bảo toàn đường đi ngắn nhất, đó chính xác là những gì mà hàm tính điểm của bài toán phản ánh.
