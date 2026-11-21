---
title: "CF 246D - Đồ thị đầy màu sắc"
description: "Chúng ta có một đồ thị vô hướng trong đó mỗi đỉnh có một màu, được biểu thị bằng một số nguyên. Mục đích là để xác định màu nào có tập hợp các màu lân cận đa dạng nhất."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 246
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 151 (Div. 2)"
rating: 1600
weight: 246
solve_time_s: 90
verified: true
draft: false
---

[CF 246D - Đồ thị đầy màu sắc](https://codeforces.com/problemset/problem/246/D) 

**Đánh giá:** 1600 
**Tags:** bạo lực, dfs và tương tự, đồ thị 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó mỗi đỉnh có một màu, được biểu thị bằng một số nguyên. Mục đích là để xác định màu nào có tập hợp các màu lân cận đa dạng nhất. Chính thức, cho mỗi màu$k$, chúng ta xét tất cả các đỉnh có màu đó và thu thập màu của tất cả các đỉnh liền kề khác với$k$. Màu có tập hợp các màu lân cận có kích thước lớn nhất chính là câu trả lời. Nếu có nhiều màu trùng nhau thì ta chọn số màu nhỏ nhất. 

Đồ thị có thể có tới$10^5$đỉnh và cạnh, do đó, một thuật toán có độ phức tạp cao hơn$O(n + m)$hoặc$O((n + m)\log n)$có thể sẽ hết thời gian. Chúng ta cũng cần quản lý màu sắc có thể cao tới mức$10^5$, do đó, việc tạo các mảng được lập chỉ mục trực tiếp theo màu sắc có thể tốn nhiều bộ nhớ nếu được thực hiện một cách đơn giản. 

Một vài trường hợp khó khăn không rõ ràng là rất quan trọng. Đầu tiên, một biểu đồ trong đó tất cả các đỉnh có cùng màu. Ví dụ: với 3 đỉnh đều được tô màu 1 và các cạnh 1-2, 2-3, 1-3, không có màu lân cận rõ ràng nên đầu ra vẫn phải là 1, màu duy nhất. Thứ hai, nếu nhiều màu có độ đa dạng tối đa như nhau, chúng ta phải chọn màu nhỏ nhất chứ không phải màu đầu tiên chúng ta gặp. Cuối cùng, các đỉnh hoặc thành phần bị ngắt kết nối có màu biệt lập không đóng góp vào các đỉnh lân cận, nhưng màu vẫn được tính để xem xét nếu có. 

## Phương pháp tiếp cận 

Phương pháp brute-force là xem xét từng màu$k$một cách độc lập. Đối với mỗi đỉnh màu$k$, quét danh sách kề của nó và thêm tất cả các màu lân cận khác nhau vào một tập hợp. Khi chúng tôi xử lý tất cả các đỉnh của$k$, chúng ta biết bản số của$Q(k)$. Việc lặp lại điều này cho tất cả các màu đảm bảo tính chính xác, nhưng quá chậm vì việc lặp lại tất cả các đỉnh cho mỗi màu có thể dẫn đến$O(n \cdot m)$hoạt động trong đồ thị dày đặc. 

Thông tin chi tiết quan trọng để tối ưu hóa là chúng ta không cần xử lý mọi đỉnh cho mọi màu. Mỗi cạnh chỉ kết nối hai đỉnh và nó góp phần vào tính đa dạng của tối đa hai màu: màu của điểm cuối nếu chúng khác nhau. Do đó, chúng ta có thể lặp lại tất cả các cạnh một lần và với mỗi cạnh nối các đỉnh$u$Và$v$với các màu sắc khác nhau, chúng tôi thêm$c[v]$vào tập hợp các màu lân cận của$c[u]$Và$c[u]$vào tập hợp các màu lân cận của$c[v]$. Điều này làm giảm vấn đề từ việc quét liên tục tất cả các danh sách kề để chỉ duyệt qua các cạnh một lần. Việc còn lại là ghi sổ để duy trì các bộ này và tìm ra màu có kích thước bộ lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n) mỗi màu | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các đỉnh, màu sắc và cạnh của đồ thị. Duy trì ánh xạ từ chỉ số đỉnh tới màu. 
2. Khởi tạo một từ điển trống để lưu trữ tập hợp các màu lân cận cho mỗi màu. 
3. Đối với mỗi cạnh$u, v$, kiểm tra xem màu sắc của$u$Và$v$khác nhau. Nếu vậy, hãy chèn màu của$v$vào tập lân cận của$c[u]$, và màu sắc của$u$vào tập lân cận của$c[v]$. Điều này đảm bảo tập hợp của mỗi màu chỉ tính các màu lân cận riêng biệt. 
4. Sau khi xử lý tất cả các cạnh, lặp lại tất cả các màu có trong biểu đồ. Theo dõi màu sắc với kích thước tối đa của tập hàng xóm. Nếu nhiều màu có cùng kích thước tối đa thì hãy chọn màu nhỏ nhất. 
5. Xuất màu đã chọn. 

Tại sao nó hoạt động: mỗi cạnh kết nối hai màu khác nhau đóng góp chính xác một lần vào số lượng đa dạng lân cận của mỗi màu. Điều bất biến là tập hợp các màu lân cận của mỗi màu chính xác là sự kết hợp của tất cả các màu của các đỉnh liền kề với bất kỳ đỉnh nào của màu đó. Bằng cách sử dụng các bộ, chúng tôi sẽ tự động tránh được sự trùng lặp. Do đó, số lượng thẻ tối đa được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

n, m = map(int, input().split())
colors = list(map(int, input().split()))
edges = [tuple(map(int, input().split())) for _ in range(m)]

# Map color to its set of neighboring colors
neighbor_colors = defaultdict(set)

for u, v in edges:
    u -= 1
    v -= 1
    cu, cv = colors[u], colors[v]
    if cu != cv:
        neighbor_colors[cu].add(cv)
        neighbor_colors[cv].add(cu)

# Ensure every color present in graph has an entry (even isolated)
unique_colors = set(colors)
for color in unique_colors:
    neighbor_colors[color]  # touch entry to ensure existence

# Find color with maximum neighbor diversity, tie-breaking by smallest color
max_diversity = -1
best_color = None
for color in neighbor_colors:
    diversity = len(neighbor_colors[color])
    if diversity > max_diversity or (diversity == max_diversity and color < best_color):
        max_diversity = diversity
        best_color = color

print(best_color)
```Đầu tiên, mã sẽ xây dựng các tập hợp lân cận, sau đó đảm bảo tất cả các màu hiện tại có mục nhập tương ứng để xử lý các màu riêng biệt. Cuối cùng, nó tìm kiếm sự đa dạng tối đa trong khi vẫn tôn trọng quy tắc ràng buộc. Cần thận trọng khi giảm các chỉ số cho đầu vào dựa trên 1. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
6 6
1 1 2 3 5 8
1 2
3 2
1 4
4 3
4 5
4 6
```| Đỉnh | Màu sắc | Các màu lân cận (sau khi xử lý tất cả các cạnh) | 
| --- | --- | --- | 
| 1 | 1 | {2, 3} | 
| 2 | 1 | {2, 3} | 
| 3 | 2 | {1, 3} | 
| 4 | 3 | {1, 2, 5, 8} | 
| 5 | 5 | {3} | 
| 6 | 8 | {3} | 

Kích thước thiết lập cho mỗi màu: 

| Màu sắc | Hàng xóm đa dạng | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 4 | 
| 5 | 1 | 
| 8 | 1 | 

Độ đa dạng tối đa là 4 cho màu 3 được in. 

**Ví dụ tùy chỉnh 2: Tất cả các đỉnh cùng màu** 

đầu vào:```
3 2
1 1 1
1 2
2 3
```Mỗi đỉnh có màu 1 và tất cả các cạnh đều nối các đỉnh cùng màu. Các tập hàng xóm trống. Độ đa dạng tối đa là 0, màu duy nhất hiện tại là 1. Đầu ra:```
1
```Điều này xác nhận các màu riêng biệt hoặc không đa dạng được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần chuyển để đọc đầu vào, một lần chuyển qua các cạnh để điền vào các tập hợp lân cận, một lần chuyển qua màu sắc để chọn mức tối đa. | 
| Không gian | O(n + m) | Bộ hàng xóm cho mỗi màu và lưu trữ cho các cạnh. | 

Với những hạn chế, với$n, m \le 10^5$, điều này chạy hiệu quả trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    colors = list(map(int, input().split()))
    edges = [tuple(map(int, input().split())) for _ in range(m)]
    neighbor_colors = defaultdict(set)
    for u, v in edges:
        u -= 1
        v -= 1
        cu, cv = colors[u], colors[v]
        if cu != cv:
            neighbor_colors[cu].add(cv)
            neighbor_colors[cv].add(cu)
    for color in set(colors):
        neighbor_colors[color]
    max_div = -1
    best = None
    for color in neighbor_colors:
        div = len(neighbor_colors[color])
        if div > max_div or (div == max_div and color < best):
            max_div = div
            best = color
    return str(best)

# provided sample
assert run("6 6\n1 1 2 3 5 8\n1 2\n3 2\n1 4\n4 3\n4 5\n4 6\n") == "3", "sample 1"

# custom cases
assert run("3 2\n1 1 1\n1 2\n2 3\n") == "1", "all same color"
assert run("4 2\n1 2 3 4\n1 2\n3 4\n") == "1", "tie between 1 and 3, pick smaller"
assert run("5 0\n10 20 30 40 50\n") == "10", "no edges, smallest color"
assert run("5 4\n1 2 3 2 1\n1 2\n2 3\n3 4\n4 5\n") == "2", "diversity across middle color"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2, tất cả 1 | 1 | xử lý tất cả các đỉnh cùng màu | 
| 4 2, màu 1-4 | 1 | tie-break chọn nhỏ nhất |
