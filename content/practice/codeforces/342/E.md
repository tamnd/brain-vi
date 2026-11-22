---
title: "CF 342E - Xenia và Cây"
description: "Chúng tôi đang làm việc với một cây trong đó mỗi nút biểu thị một điểm trong biểu đồ chu kỳ được kết nối. Ban đầu chỉ có nút 1 có màu đỏ, trong khi mọi nút khác bắt đầu có màu xanh lam. Theo thời gian, chúng tôi thực hiện hai loại hoạt động."
date: "2026-06-06T17:38:06+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "trees"]
categories: ["algorithms"]
codeforces_contest: 342
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 199 (Div. 2)"
rating: 2400
weight: 342
solve_time_s: 82
verified: true
draft: false
---

[CF 342E - Xenia và Cây](https://codeforces.com/problemset/problem/342/E) 

**Đánh giá:** 2400 
**Tas:** cấu trúc dữ liệu, phân chia để chinh phục, cây cối 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một cây trong đó mỗi nút biểu thị một điểm trong biểu đồ chu kỳ được kết nối. Ban đầu chỉ có nút 1 có màu đỏ, trong khi mọi nút khác bắt đầu có màu xanh lam. Theo thời gian, chúng tôi thực hiện hai loại hoạt động. Một thao tác biến nút màu xanh đã chọn thành màu đỏ vĩnh viễn, làm tăng tập hợp các nút màu đỏ. Thao tác còn lại yêu cầu, đối với một nút nhất định, nó gần nút đỏ gần nhất như thế nào về khoảng cách cây. 

Mỗi truy vấn đều trực tuyến, vì vậy chúng tôi phải trả lời các truy vấn loại 2 ngay lập tức khi tập hợp màu đỏ phát triển. Khó khăn chính là các nút màu đỏ tích lũy theo thời gian và khoảng cách được đo trên cây, do đó, việc tính toán lại các đường đi ngắn nhất từ ​​đầu cho mỗi truy vấn sẽ quá chậm. 

Các ràng buộc đẩy chúng tôi tới một giải pháp gần với tuyến tính hoặc logarit cho mỗi truy vấn. Với tối đa 100.000 nút và 100.000 truy vấn, bất kỳ thao tác nào tính toán lại BFS hoặc DFS cho mỗi truy vấn sẽ ngay lập tức thoái hóa thành khoảng 10¹⁰ thao tác trong trường hợp xấu nhất, điều này là không khả thi. Ngay cả việc tính toán lại khoảng cách từ tất cả các nút đỏ cho mỗi truy vấn rõ ràng là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều truy vấn loại 2 liên tiếp được đưa ra trước khi bất kỳ nút màu đỏ mới nào được thêm vào. Trong trường hợp đó, một cách tiếp cận đơn giản có thể liên tục tính toán lại khoảng cách từ đầu mặc dù câu trả lời không thay đổi. Một góc khác là khi các nút đỏ trở nên dày đặc trong một vùng nhỏ của cây, điều này sẽ phá vỡ mọi giải pháp giả định một nguồn duy nhất hoặc cố gắng chỉ duy trì một nút đỏ “tốt nhất”. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ duy trì một tập hợp các nút màu đỏ và đối với mỗi truy vấn yêu cầu khoảng cách, hãy chạy BFS đa nguồn bắt đầu từ tất cả các nút màu đỏ. Điều này đúng vì BFS trên cây cho khoảng cách chính xác. Tuy nhiên, mỗi truy vấn như vậy sẽ có chi phí là O(n) và với truy vấn O(n), giá trị này sẽ trở thành O(n2), tốc độ này quá chậm. 

Quan sát chính là cấu trúc cây cho phép chúng ta tách các bản cập nhật khỏi các truy vấn bằng cách sử dụng phân tách trọng tâm. Thay vì cố gắng trả lời trực tiếp “khoảng cách đến bất kỳ nút đỏ nào”, chúng tôi điều chỉnh lại vấn đề bằng cách duy trì, đối với mỗi nút, thông tin về đường đi của nó thông qua cây phân rã trọng tâm. 

Mỗi nút thuộc về các thành phần trung tâm O(log n). Đối với mỗi centroid trong phân tách này, chúng ta duy trì khoảng cách tối thiểu từ centroid đó đến bất kỳ nút màu đỏ nào trong cây con thành phần của nó. Khi một nút chuyển sang màu đỏ, chúng tôi cập nhật tất cả các trọng tâm tổ tiên của nó. Khi chúng tôi truy vấn một nút, chúng tôi kết hợp các giá trị được lưu trữ này trên đường dẫn trung tâm của nó và khôi phục khoảng cách tối thiểu tới bất kỳ nút màu đỏ nào. 

Điều này có tác dụng vì mọi đường đi giữa hai nút đều đi qua một tập hợp các nút tổ tiên ở tâm được xác định rõ ràng và các khoảng cách sẽ phân tách rõ ràng dọc theo các nút tổ tiên đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn | O(nm) | O(n) | Quá chậm | 
| Phân hủy trung tâm | O(n log n + m log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một phân tách trung tâm của cây và duy trì, đối với mỗi trung tâm, một giá trị biểu thị khoảng cách tối thiểu từ trung tâm đó đến bất kỳ nút màu đỏ hiện tại nào.

1. Đầu tiên, chúng ta root phân rã centroid. Ở mỗi giai đoạn, chúng tôi tính toán kích thước cây con và chọn trọng tâm để chia thành phần hiện tại thành các phần cân bằng. Điều này đảm bảo độ sâu đệ quy là logarit. 
2. Đối với mỗi nút trong cây ban đầu, chúng tôi tính toán trước khoảng cách của nó đến từng tâm trên đường đi trọng tâm của nó. Điều này được thực hiện trong quá trình phân rã bằng cách sử dụng BFS hoặc DFS từ mỗi centroid. 
3. Chúng ta khởi tạo tất cả các giá trị centroid thành vô cùng, sau đó đặt nút 1 thành màu đỏ. Đối với nút 1, chúng tôi cập nhật tất cả các tâm trên đường đi của nó bằng cách lấy khoảng cách tối thiểu từ các tâm đó đến nút 1. 
4. Đối với truy vấn loại 1, khi một nút chuyển sang màu đỏ, chúng ta sẽ đi lên đường tâm của nó. Đối với mỗi tổ tiên trung tâm, chúng tôi cập nhật giá trị tối thiểu được lưu trữ của nó bằng cách sử dụng khoảng cách được tính toán trước từ trung tâm đó đến nút. 
5. Đối với truy vấn loại 2, chúng ta lại duyệt qua đường tâm của nút được truy vấn. Đối với mỗi tổ tiên trung tâm, chúng tôi kết hợp giá trị trung tâm được lưu trữ với khoảng cách từ trung tâm đó đến nút truy vấn. Mức tối thiểu trên tất cả các kết hợp như vậy là câu trả lời. 

Ý tưởng quan trọng là mọi đường dẫn từ nút truy vấn đến bất kỳ nút đỏ nào đều phải đi qua ít nhất một tổ tiên trung tâm trong đó cả hai khoảng cách đều được tính toán chính xác. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi tâm đều lưu trữ nút màu đỏ gần nhất trong thành phần của nó theo khoảng cách được đo qua tâm đó. Đối với bất kỳ nút truy vấn v và bất kỳ nút đỏ r nào, tồn tại một trọng tâm c trên các đường dẫn phân tách của cả v và r sao cho khoảng cách đường dẫn dist(v, r) bằng dist(v, c) + dist(c, r). Vì chúng tôi lưu trữ khoảng cách tối thiểu có thể có (c, r) trên tất cả các nút màu đỏ, nên việc kiểm tra tất cả các trọng tâm trên đường dẫn của v đảm bảo rằng chúng tôi nắm bắt được khoảng cách tối thiểu thực sự tới bất kỳ nút màu đỏ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    g[a].append(b)
    g[b].append(a)

parent_centroid = [-1] * n
sub_size = [0] * n
blocked = [False] * n

centroid_tree = [[] for _ in range(n)]
dist_to_centroid = [[] for _ in range(n)]
centroids_on_path = [[] for _ in range(n)]

INF = 10**18
best = [INF] * n

def dfs_size(v, p):
    sub_size[v] = 1
    for to in g[v]:
        if to != p and not blocked[to]:
            dfs_size(to, v)
            sub_size[v] += sub_size[to]

def dfs_centroid(v, p, total):
    for to in g[v]:
        if to != p and not blocked[to]:
            if sub_size[to] > total // 2:
                return dfs_centroid(to, v, total)
    return v

def add_dist(v, p, c, d):
    dist_to_centroid[v].append(d)
    centroids_on_path[v].append(c)
    for to in g[v]:
        if to != p and not blocked[to]:
            add_dist(to, v, c, d + 1)

def build(c_parent, entry):
    dfs_size(entry, -1)
    c = dfs_centroid(entry, -1, sub_size[entry])
    blocked[c] = True

    parent_centroid[c] = c_parent

    add_dist(c, -1, c, 0)

    for to in g[c]:
        if not blocked[to]:
            build(c, to)

def update(v):
    for i in range(len(centroids_on_path[v])):
        c = centroids_on_path[v][i]
        d = dist_to_centroid[v][i]
        best[c] = min(best[c], d)

def query(v):
    ans = INF
    for i in range(len(centroids_on_path[v])):
        c = centroids_on_path[v][i]
        d = dist_to_centroid[v][i]
        ans = min(ans, best[c] + d)
    return ans

build(-1, 0)

update(0)

for _ in range(q):
    t, v = map(int, input().split())
    v -= 1
    if t == 1:
        update(v)
    else:
        print(query(v))
```Sự phân rã được xây dựng một lần bằng cách sử dụng phương pháp phân tách trọng tâm đệ quy. Trong quá trình xây dựng, mỗi nút lưu trữ một danh sách các trọng tâm trên đường đi của nó cùng với khoảng cách đến chúng. Điều này cho phép cả cập nhật và truy vấn hoạt động mà không cần duyệt qua cây gốc. 

các`best`mảng là cấu trúc cốt lõi:`best[c]`luôn đại diện cho khoảng cách tối thiểu từ tâm`c`đến bất kỳ nút màu đỏ nào. Việc cập nhật một nút chỉ yêu cầu đi theo đường tâm của nó, có độ sâu logarit. 

Một chi tiết tinh vi là việc phân tách trung tâm phải đánh dấu các nút là bị chặn sau khi được chọn, nếu không phép đệ quy sẽ truy cập lại các thành phần đã được phân tách và phá vỡ các đảm bảo về kích thước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2
2 3
2 4
4 5
2 1
2 5
1 2
2 5
```Chúng tôi bắt đầu với nút 1 màu đỏ. 

| Bước | Truy vấn | Nút đỏ | Nút truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | truy vấn 1 | {1} | 1 | 0 | 
| 2 | truy vấn 5 | {1} | 5 | 3 | 
| 3 | sơn 2 | {1,2} | - | - | 
| 4 | truy vấn 5 | {1,2} | 5 | 2 | 

Truy vấn thứ hai được cải thiện sau khi nút 2 chuyển sang màu đỏ vì đường dẫn 5 → 4 → 2 ngắn hơn 5 → 4 → 2 → 1. 

Điều này xác nhận rằng nhiều nguồn màu đỏ được xử lý chính xác, không chỉ nguồn gốc ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi nút tham gia vào các cấp độ trung tâm O(log n) và mỗi bản cập nhật/truy vấn sẽ đi theo con đường đó | 
| Không gian | O(n log n) | Mỗi nút lưu trữ thông tin đường dẫn trung tâm qua các mức phân tách | 

Hệ số logarit xuất phát từ việc giảm một nửa các thành phần lặp đi lặp lại trong quá trình phân tách trọng tâm. Với 10⁵ nút và truy vấn, điều này hoàn toàn phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    # Paste solution here as functionally encapsulated version if needed
    # For illustration, assume solve() exists
    return solve()

# sample 1
assert run("""5 4
1 2
2 3
2 4
4 5
2 1
2 5
1 2
2 5
""").strip() == """0
3
2"""

# single node chain
assert run("""3 3
1 2
2 3
2 3
1 2
2 3
""").strip() == """2
1"""

# all nodes already red via updates
assert run("""4 3
1 2
2 3
3 4
2 4
1 3
2 4
""").strip() == """3
1"""

# star shaped tree
assert run("""5 5
1 2
1 3
1 4
1 5
2 5
1 5
2 5
2 2
""").strip() == """1
0
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật chuỗi | khoảng cách hỗn hợp | truyền dọc theo đường đi | 
| cây sao | sự thống trị trung tâm | độ đúng trọng tâm | 
| kích hoạt gia tăng | bộ phát triển | cập nhật trực tuyến | 

## Vỏ cạnh 

Một tình huống khó khăn là khi các bản cập nhật được phân cụm sâu trong một cây con. Trong cách tiếp cận trung tâm đơn giản, việc quên truyền bá các bản cập nhật tới tất cả tổ tiên của trung tâm sẽ dẫn đến khoảng cách không chính xác cho các nút bên ngoài cây con đó. Cấu trúc đường dẫn trọng tâm ngăn chặn điều này vì mỗi nút chia sẻ ít nhất một trọng tâm với mọi đường dẫn đi qua vùng của nó. 

Một trường hợp đặc biệt khác là khi nút truy vấn chuyển sang màu đỏ. Trong trường hợp đó, bản cập nhật centroid đảm bảo các mục centroid của chính nó trở thành nguồn có khoảng cách bằng 0, do đó việc truy vấn nó sẽ trả về 0 một cách chính xác mà không cần viết hoa đặc biệt. 

Đối với biểu đồ đường dẫn, quá trình phân tách trọng tâm suy biến thành chuỗi phân chia cân bằng, nhưng mỗi nút vẫn duy trì các mục nhập trung tâm O(log n), đảm bảo các truy vấn vẫn hiệu quả ngay cả trong các cấu trúc tuyến tính trong trường hợp xấu nhất.
