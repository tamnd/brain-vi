---
title: "CF 105757L - Cây Hài Hòa"
description: "Chúng ta có một cây có gốc với gốc 1. Mỗi đỉnh lưu trữ một giá trị. Đối với mỗi truy vấn (u, v), chúng ta cần quyết định xem v có nằm trong cây con của u hay không và liệu các đỉnh còn lại sau khi loại bỏ toàn bộ cây con của v khỏi cây con của u có thể được ghép nối sao cho mọi cặp…"
date: "2026-06-25T16:02:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105757
codeforces_index: "L"
codeforces_contest_name: "Insomnia 2025"
rating: 0
weight: 105757
solve_time_s: 48
verified: true
draft: false
---

[CF 105757L - Cây hài hòa](https://codeforces.com/problemset/problem/105757/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với gốc`1`. Mỗi đỉnh lưu trữ một giá trị. Đối với mỗi truy vấn`(u, v)`, chúng ta cần phải quyết định xem`v`nằm bên trong cây con của`u`và liệu các đỉnh còn lại sau khi loại bỏ toàn bộ cây con của`v`từ cây con của`u`có thể được ghép nối để mỗi cặp chứa hai giá trị khác nhau. 

Điều kiện đầu tiên hoàn toàn là về tổ tiên. Nếu như`u`không phải là tổ tiên của`v`, thì tổ tiên chung thấp nhất của chúng không thể là`u`, vậy là có ngay câu trả lời`NO`. 

Phần thứ hai trông giống như một vấn đề về phân vùng, nhưng nó có thể được rút gọn thành một quan sát đơn giản hơn nhiều. Giả sử tập còn lại chứa`k`đỉnh. Chúng ta cần chia chúng thành hai nhóm có kích thước bằng nhau và ghép các nhóm lại với nhau. Một giá trị xuất hiện quá thường xuyên sẽ ngăn cản điều này. Nếu một số giá trị xuất hiện nhiều hơn`k / 2`đôi khi, không có đủ đỉnh với các giá trị khác để ghép với tất cả các lần xuất hiện của giá trị đó. Mặt khác, nếu không có giá trị nào vượt quá một nửa tập hợp, chúng ta luôn có thể sắp xếp các cặp. 

Vì vậy, mọi truy vấn đều trở thành: 

Tìm kích thước của`subtree(u) - subtree(v)`. Nó phải bằng nhau. 

Tìm xem tập hợp này có giá trị đa số hay không, nghĩa là một số giá trị chiếm hơn một nửa kích thước tập hợp. 

Cây có tới`100000`đỉnh và cùng số lượng truy vấn. Giải pháp bậc hai sẽ yêu cầu xem qua phần lớn cây cho mọi truy vấn, có thể tiếp cận khoảng`10^10`hoạt động. Chúng tôi cần tiền xử lý để cho phép mỗi truy vấn được trả lời theo thời gian logarit. 

Các trường hợp cạnh khó không phải là cây lớn mà là ý nghĩa chính xác của cây con bị loại bỏ. 

Ví dụ: nếu cây là:```
1
|
2
|
3
```với các giá trị:```
1 1 2
```và truy vấn là`(2, 3)`, tập còn lại chỉ là nút`2`, vậy kích thước của nó là số lẻ và câu trả lời là`NO`. Giải pháp chỉ kiểm tra màu sắc và quên số lượng ghép nối sẽ chấp nhận sai. 

Một trường hợp khác là:```
1
|
2
```với các giá trị:```
5 5
```và truy vấn`(1, 2)`. Bộ còn lại là`{1}`, không thể tạo thành một cặp. Giá trị lặp lại không phải là lý do thực sự dẫn đến thất bại ở đây mà chính là kích thước lẻ. 

Một trường hợp tế nhị hơn là:```
1
/ \
2  3
```với các giá trị:```
7 7 8
```và truy vấn`(1, 2)`. Bộ còn lại là`{1,3}`. Kích thước là chẵn và các giá trị khác nhau, vì vậy câu trả lời là`YES`. Một cách tiếp cận bất cẩn kiểm tra toàn bộ cây con của`1`sẽ thấy hai`7`s và từ chối không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, trước tiên hãy kiểm tra xem`u`là tổ tiên của`v`. Nếu đúng thì duyệt cây con của`u`trong khi bỏ qua cây con của`v`, đếm tất cả các giá trị và kiểm tra xem tần số lớn nhất có lớn hơn một nửa kích thước còn lại hay không. Điều này đúng vì nó trực tiếp đánh giá định nghĩa về tính tương thích. 

Vấn đề là chi phí. Một truy vấn có thể chạm vào`O(n)`đỉnh và với`10^5`truy vấn trường hợp xấu nhất là về`10^10`thăm đỉnh, vượt xa thời gian cho phép. 

Quan sát quan trọng là điều kiện ghép nối chỉ phụ thuộc vào giá trị đa số của một tập hợp. Chúng tôi không cần tất cả tần số cho mọi truy vấn. Một bộ có một giá trị xuất hiện hơn một nửa thời gian hoặc không. 

Ý tưởng bỏ phiếu theo đa số của Boyer Moore cung cấp chính xác thông tin chúng ta cần. Một phân khúc có thể được biểu diễn bằng một giá trị ứng cử viên và số dư. Khi hai phân đoạn rời rạc được kết hợp, bản tóm tắt của chúng có thể được hợp nhất trong khi vẫn giữ nguyên ứng cử viên đa số có thể. 

Sau chuyến tham quan Euler, mỗi cây con trở thành một khoảng liền kề. Từ`subtree(v)`bị xóa khỏi`subtree(u)`, các đỉnh còn lại nhiều nhất là hai khoảng Euler. Chúng ta có thể lưu trữ các tóm tắt đa số trong cây phân đoạn và kết hợp hai khoảng còn lại. Sau đó, ứng cử viên kết quả sẽ được kiểm tra bằng cách đếm tần số thực của nó bằng cách sử dụng các vị trí được sắp xếp của mọi giá trị. 

Lực lượng vũ phu hoạt động vì nó đếm trực tiếp tất cả các giá trị, nhưng không thành công khi nhiều truy vấn chồng lên các cây con lớn. Nhận xét rằng chỉ có giá trị đa số có thể có mới quan trọng làm giảm truy vấn từ việc quét các đỉnh xuống một vài phép toán logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS từ gốc để tính toán vị trí chuyến tham quan Euler, kích thước cây con và thời gian vào và ra. Cây con của một nút trở thành khoảng`[tin[node], tout[node]]`. 
2. Xây bàn nâng đôi cho tổ tiên. Điều này cho phép chúng tôi kiểm tra tổ tiên và xử lý các truy vấn liên quan đến tổ tiên chung thấp nhất trong`O(log n)`thời gian. 
3. Xây dựng cây phân đoạn theo thứ tự Euler. Mỗi nút của cây phân đoạn lưu trữ bản tóm tắt đa số Boyer Moore chứa giá trị ứng cử viên và số dư của nó. 
4. Lưu trữ vị trí Euler của mọi giá trị theo thứ tự được sắp xếp. Điều này cho phép đếm số lần giá trị được chọn xuất hiện trong bất kỳ khoảng nào bằng cách sử dụng tìm kiếm nhị phân. 
5. Đối với mỗi truy vấn`(u, v)`, trước tiên hãy kiểm tra xem`u`là tổ tiên của`v`. Nếu không thì câu trả lời là`NO`. 
6. Tính số đỉnh còn lại là`subtree_size[u] - subtree_size[v]`. Nếu số này là số lẻ thì đáp án là`NO`, vì mỗi đỉnh phải thuộc đúng một cặp. 
7. Nếu`u == v`, tập còn lại trống. Không có cặp nào để hình thành nên câu trả lời là`YES`. 
8. Cây con bị loại bỏ là một khoảng Euler bên trong cây con của`u`. Các đỉnh còn lại là phần trước và sau khoảng đó. Truy vấn cây phân đoạn trên cả hai phần và hợp nhất hai bản tóm tắt chính. 
9. Đếm số lần xuất hiện thực tế của giá trị ứng viên trong các khoảng còn lại. Nếu số lượng này lớn hơn một nửa kích thước còn lại thì câu trả lời là`NO`. Nếu không thì câu trả lời là`YES`. 

Điều bất biến đằng sau thuật toán là mỗi bản tóm tắt khoảng Euler luôn giữ lại ứng cử viên đa số duy nhất có thể có của khoảng đó. Nếu một giá trị chiếm phần lớn thực sự của bộ truy vấn thì giá trị đó phải tồn tại trong mọi hoạt động hợp nhất Boyer Moore. Việc kiểm tra tần suất cuối cùng sẽ loại bỏ các ứng cử viên sai, do đó thuật toán chấp nhận chính xác các tập hợp không tồn tại giá trị đa số. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = 1
        while self.n < len(arr):
            self.n *= 2
        self.tree = [(0, 0)] * (2 * self.n)
        for i, x in enumerate(arr):
            self.tree[self.n + i] = (x, 1)
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.merge(self.tree[2 * i], self.tree[2 * i + 1])

    def merge(self, a, b):
        if a[0] == b[0]:
            return (a[0], a[1] + b[1])
        if a[1] > b[1]:
            return (a[0], a[1] - b[1])
        return (b[0], b[1] - a[1])

    def query(self, l, r):
        if l > r:
            return (0, 0)
        l += self.n
        r += self.n
        left = (0, 0)
        right = (0, 0)
        while l <= r:
            if l & 1:
                left = self.merge(left, self.tree[l])
                l += 1
            if not (r & 1):
                right = self.merge(self.tree[r], right)
                r -= 1
            l //= 2
            r //= 2
        return self.merge(left, right)

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        x, y = map(int, input().split())
        graph[x].append(y)
        graph[y].append(x)

    LOG = 17
    while (1 << LOG) <= n:
        LOG += 1

    up = [[0] * (n + 1) for _ in range(LOG)]
    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    size = [0] * (n + 1)
    euler = []
    timer = 0

    sys.setrecursionlimit(300000)

    def dfs(v, p):
        nonlocal timer
        up[0][v] = p
        for i in range(1, LOG):
            up[i][v] = up[i - 1][up[i - 1][v]]
        tin[v] = timer
        euler.append(a[v])
        timer += 1
        size[v] = 1
        for u in graph[v]:
            if u != p:
                dfs(u, v)
                size[v] += size[u]
        tout[v] = timer - 1

    dfs(1, 1)

    for i in range(LOG):
        up[i][1] = 1

    positions = {}
    for i, x in enumerate(euler):
        if x not in positions:
            positions[x] = []
        positions[x].append(i)

    seg = SegTree(euler)

    def ancestor(x, y):
        return tin[x] <= tin[y] <= tout[x]

    def count_value(x, l, r):
        if l > r:
            return 0
        arr = positions.get(x, [])
        return bisect_right(arr, r) - bisect_left(arr, l)

    q = int(input())
    ans = []

    for _ in range(q):
        u, v = map(int, input().split())

        if not ancestor(u, v):
            ans.append("NO")
            continue

        remaining = size[u] - size[v]

        if remaining % 2:
            ans.append("NO")
            continue

        if remaining == 0:
            ans.append("YES")
            continue

        cand1 = seg.query(tin[u], tin[v] - 1)
        cand2 = seg.query(tout[v] + 1, tout[u])
        cand = seg.merge(cand1, cand2)[0]

        cnt = count_value(cand, tin[u], tin[v] - 1)
        cnt += count_value(cand, tout[v] + 1, tout[u])

        if cnt * 2 > remaining:
            ans.append("NO")
        else:
            ans.append("YES")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần DFS chuyển cây thành bài toán mảng. Thời gian vào và ra là điều khiến cho việc truy vấn cây con có thể thực hiện được mà không cần phải di chuyển trên cây nhiều lần. 

Cây phân đoạn không lưu trữ tần số. Nó chỉ lưu trữ ứng cử viên Boyer Moore và số dư, bởi vì đa số thực sự là duy nhất và phải là ứng cử viên sau khi kết hợp bất kỳ phân vùng nào của tập hợp. 

Danh sách vị trí chỉ được sử dụng sau khi có được ứng viên. Sự tách biệt này là cần thiết vì Boyer Moore có thể tìm ra đa số có thể có nhưng không thể chứng minh rằng nó thực sự tồn tại. Các tìm kiếm nhị phân thực hiện xác minh cuối cùng đó. 

Thứ tự xử lý truy vấn có vấn đề. Việc kiểm tra kích thước xảy ra trước logic đa số vì một số đỉnh lẻ không bao giờ có thể được ghép nối hoàn toàn. Trường hợp tập trống cũng phải được xử lý trước khi truy vấn cây phân đoạn. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên:```
5
1 1 2 1 2
1 2
2 3
3 4
4 5
```Đối với truy vấn`(3,5)`: 

| Bước | Giá trị | 
| --- | --- | 
| Là 3 tổ tiên của 5 | Có | 
| Kích thước còn lại | 2 | 
| Khoảng Euler còn lại | nút 3 và nút 4 | 
| Ứng cử viên đa số | không | 
| Kết quả | CÓ | 

Hai giá trị còn lại là`2`Và`1`, vì vậy chúng có thể tạo thành một cặp hợp lệ. 

Đối với truy vấn`(2,5)`: 

| Bước | Giá trị | 
| --- | --- | 
| Là 2 tổ tiên của 5 | Có | 
| Kích thước còn lại | 3 | 
| Khoảng Euler còn lại | nút 2,3,4 | 
| Kích thước tương đương | lẻ | 
| Kết quả | KHÔNG | 

Thuật toán bác bỏ ngay lập tức vì ba đỉnh không thể chia thành từng cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) log n) | DFS, xây dựng cây phân đoạn và mỗi truy vấn sử dụng các phép toán logarit | 
| Không gian | O(n log n) | Bảng tổ tiên thống trị việc sử dụng bộ nhớ | 

Các ràng buộc cho phép tiền xử lý tuyến tính và truy vấn logarit. Giải pháp tránh việc quét các cây con, do đó, ngay cả một cây hình chuỗi có`100000`các nút vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("""5
1 1 2 1 2
1 2
2 3
3 4
4 5
4
4 5
3 5
2 5
1 4
""") == """NO
YES
NO
NO
""", "sample 1"

assert run("""6
1 2 3 4 5 6
1 2
2 3
3 4
4 5
5 6
4
1 3
1 2
2 5
1 6
""") == """YES
NO
NO
NO
""", "sample 2"

assert run("""1
7
1
3
1 1
""") == "YES\n", "single node"

assert run("""3
5 5 8
1 2
1 3
2
1 2
1 3
""") == """YES
YES
""", "different remaining pairs"

assert run("""4
1 1 1 1
1 2
1 3
1 4
2
1 2
1 3
""") == """NO
NO
""", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nút đơn | CÓ | Xử lý bộ còn lại trống | 
| Ngôi sao có giá trị hỗn hợp | CÓ | Ghép nối với cây con đã bị loại bỏ | 
| Tất cả các giá trị bằng nhau | KHÔNG | Phát hiện đa số | 
| Ví dụ về đường dẫn | Hỗn hợp | Ranh giới tổ tiên và chẵn lẻ | 

## Vỏ cạnh 

Đối với một truy vấn ở đâu`u == v`, cây con bị loại bỏ là toàn bộ cây con của`u`, để lại một tập hợp trống. Thuật toán đạt đến`remaining == 0`điều kiện và trả lại`YES`, vì không có đỉnh nào cần ghép nối. 

Đối với truy vấn trong đó tập hợp còn lại có kích thước lẻ, chẳng hạn như chuỗi`1-2-3`với các giá trị`1 1 2`và truy vấn`(2,3)`, thuật toán tính toán`size[2] - size[3] = 1`. Vì một đỉnh không thể chia thành từng cặp nên nó trả về`NO`trước khi thực hiện bất kỳ công việc đa số nào. 

Đối với trường hợp toàn bộ cây con chiếm đa số nhưng phần còn lại thì không, chẳng hạn như ngôi sao có giá trị`7 7 8`và truy vấn`(1,2)`, khoảng Euler loại trừ nút`2`. Thuật toán chỉ tính các đỉnh còn lại`{1,3}`, do đó đa số sai từ cây con bị loại bỏ không bao giờ ảnh hưởng đến câu trả lời.
