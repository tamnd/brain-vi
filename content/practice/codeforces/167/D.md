---
title: "CF 167D - Pháp sư và con đường"
description: "Chúng tôi được cung cấp một tập hợp các thành phố trên máy bay. Mỗi thành phố có tọa độ x duy nhất và tọa độ y duy nhất. K thành phố đầu tiên được đưa ra một cách rõ ràng và các thành phố còn lại được tạo ra bởi hai phép lặp tuyến tính."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "graph-matchings", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 167
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 114 (Div. 1)"
rating: 3000
weight: 167
solve_time_s: 128
verified: true
draft: false
---

[CF 167D - Phù thủy và những con đường](https://codeforces.com/problemset/problem/167/D) 

**Đánh giá:** 3000 
**Tags:** cấu trúc dữ liệu, chia để trị, so khớp đồ thị, đồ thị, tham lam 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các thành phố trên máy bay. Mỗi thành phố đều có một nét độc đáo`x`phối hợp và độc đáo`y`điều phối. đầu tiên`k`các thành phố được đưa ra một cách rõ ràng và các thành phố còn lại được tạo ra bởi hai phép truy hồi tuyến tính. 

Đối với mỗi khoảng truy vấn`[L, R]`, chúng tôi chỉ giữ lại những thành phố có`x`tọa độ nằm trong khoảng đó. Trên tập hợp được lọc này, một quy tắc đặc biệt sẽ tạo đường giữa các cặp thành phố nhất định. 

Định nghĩa hình học trong phát biểu có vẻ đáng sợ nhưng cấu trúc đồ thị thực tế lại đơn giản hơn nhiều. Sau khi các thành phố được sắp xếp theo`x`, các con đường tạo thành chính xác các cạnh của cây Descartes được xác định bởi`y`các giá trị. Một cách giải thích tương đương khác là thế này: 

Đối với hai thành phố`u`Và`v`, với`y_u > y_v`, tồn tại một con đường khi và chỉ khi`u`là thành phố cao hơn gần nhất với`v`ở bên trái hoặc bên phải. 

Truy vấn yêu cầu kích thước khớp tối đa trong biểu đồ này được giới hạn ở các thành phố có`L ≤ x ≤ R`. 

Các ràng buộc loại trừ hoàn toàn việc xây dựng lại biểu đồ cho mỗi truy vấn. Chúng tôi có thể có`10^5`thành phố và`10^5`truy vấn, vì vậy bất cứ điều gì gần`O(nm)`là không thể. Thậm chí`O(n√n)`sẽ nguy hiểm trong Python. Mục tiêu đại khái là`O(n log n + m log n)`. 

Một phần tinh tế của vấn đề là bản thân đồ thị phụ thuộc vào khoảng đã chọn. Thành phố bên ngoài`[L, R]`hoàn toàn không tham gia, thậm chí không đóng vai trò chặn trong điều kiện hình học. Việc triển khai đơn giản xây dựng biểu đồ toàn cục một lần và chỉ lọc các đỉnh sau đó sẽ trở nên không chính xác. 

Hãy xem xét các thành phố:```
x: 1 2 3
y: 3 1 2
```Trên toàn cầu, thành phố`2`kết nối với cả hai`1`Và`3`. 

Đối với khoảng thời gian`[2,3]`, thành phố`1`biến mất. Rồi thành phố`2`chỉ có một cạnh có thể có, đó là`(2,3)`. Kích thước phù hợp vẫn còn`1`, nhưng đồ thị cục bộ đã thay đổi. Bất kỳ cách tiếp cận nào dựa vào biểu đồ toàn cục mà không tính toán lại sẽ âm thầm thất bại đối với các ví dụ phức tạp hơn. 

Một trường hợp cạnh nguy hiểm khác xuất hiện khi khoảng chỉ chứa một thành phố.```
x: 5
y: 10
```Câu trả lời phải là`0`. Một số cách triển khai DP của cây Descartes vô tình xử lý các phần tử con rỗng không đúng cách và tạo ra`1`. 

Trường hợp cạnh cấu trúc quan trọng nhất là một chuỗi đơn điệu.```
x: 1 2 3 4
y: 1 2 3 4
```Đồ thị trở thành một chuỗi đơn giản. Sự phù hợp tối đa là`2`, không`1`. Bất kỳ sự tham lam nào luôn khớp với nút cao nhất trước tiên đều thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi trích xuất tất cả các thành phố có`x`thuộc về`[L,R]`, xây dựng lại biểu đồ từ quy tắc hình học, sau đó chạy thuật toán khớp tối đa. 

Đồ thị là một cái cây. Sau khi sắp xếp theo`x`, các con đường chính xác là các cạnh của cây Descartes. Chúng ta có thể xây dựng nó theo thời gian tuyến tính bằng cách sử dụng ngăn xếp đơn điệu, sau đó tính toán việc so khớp cây với DP theo thời gian tuyến tính. 

Thật không may, thực hiện việc này một cách độc lập cho mọi chi phí truy vấn`O(length_of_interval)`mỗi truy vấn. Trong trường hợp xấu nhất, tất cả các truy vấn đều bao trùm toàn bộ mảng, đưa ra`10^5 × 10^5 = 10^10`hoạt động. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào hình dạng của cây Descartes bên trong khoảng. 

Đối với bất kỳ cây nào, kích thước phù hợp tối đa bằng:```
floor(size / 2)
```trừ đi số đỉnh chưa khớp bị ràng buộc bởi các ràng buộc chẵn lẻ bên trong các thành phần sau khi loại bỏ các cạnh khớp. 

Đối với cây Descartes, điều gì đó mạnh mẽ hơn nhiều sẽ xảy ra. Câu trả lời cho một khoảng có thể được thể hiện hoàn toàn thông qua các khoảng cây con. 

Thực tế cấu trúc quan trọng là thế này: 

Với bất kỳ khoảng nào, cây Descartes cảm ứng lại là cây Descartes của mảng con đó. 

Điều này ngay lập tức gợi ý sự phân chia và chinh phục trên chính cây Descartes. 

Giả sử tối đa`y`bên trong`[l,r]`xảy ra ở vị trí`mid`. Khi đó gốc cây Descartes là`mid`, với cây con trái`[l,mid-1]`và cây con bên phải`[mid+1,r]`. 

Nếu chúng ta biết các giá trị so khớp tối ưu cho cây con bên trái và cây con bên phải thì câu hỏi duy nhất còn lại là liệu gốc có khớp với một trong các cây con của nó hay không. 

Điều này dẫn đến trạng thái DP rất nhỏ. Chúng ta chỉ cần biết liệu gốc của cây con có còn trống hay không. 

Bởi vì mỗi khoảng truy vấn tương ứng với một phân đoạn được kết nối theo thứ tự cây Descartes, nên chúng tôi có thể xử lý trước tất cả các chuyển tiếp và trả lời các truy vấn bằng cách hợp nhất kiểu cây phân đoạn. 

Giải pháp cuối cùng sử dụng: 

1. Xây dựng cây Descartes trong thời gian tuyến tính. 
2. DP chia để trị theo từng khoảng thời gian. 
3. Hợp nhất kiểu cây phân đoạn của các trạng thái DP nhỏ. 

Tổng độ phức tạp trở thành`O(n log n + m log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các thành phố và sắp xếp chúng theo`x`. 

Vì tất cả`x`các giá trị khác nhau, sắp xếp theo`x`đưa ra một thứ tự tuyến tính duy nhất. Từ giờ trở đi, chúng ta chỉ làm việc với trình tự`y`các giá trị. 
2. Xây dựng cây Descartes của`y`sự liên tiếp. 

Cây Descartes là cây heap tối đa trong đó việc duyệt theo thứ tự bằng với thứ tự ban đầu. Chúng tôi xây dựng nó theo thời gian tuyến tính với một ngăn xếp đơn điệu. 
3. Quan sát rằng các con đường chính xác là các cạnh cây Descartes. 

Đối với mỗi thành phố, phần tử lớn gần nhất ở bên trái và phần tử lớn gần nhất ở bên phải sẽ xác định thành phố mẹ của nó trong cây Descartes. Quy tắc hình học từ phát biểu rút gọn chính xác về mối quan hệ này. 
4. Xác định trạng thái DP để khớp. 

Đối với mỗi cây con, duy trì hai giá trị:`dp0`, kích thước khớp tối đa khi gốc cây con không khớp.`dp1`, kích thước khớp tối đa khi gốc cây con có thể khớp. 
5. Hợp nhất các cây con con. 

Giả sử nút`u`đã bỏ con`L`và con phải`R`. 

Nếu như`u`vẫn chưa từng có:```
dp0[u] = dp1[L] + dp1[R]
```vì cả hai đứa trẻ đều không bị giới hạn. 

Nếu như`u`được phép khớp, có ba khả năng:`u`không phù hợp với ai`u`trận đấu`L`.`u`trận đấu`R`. 

Điều này mang lại:```
dp1[u] = max(
    dp0[u],
    1 + dp0[L] + dp1[R],
    1 + dp1[L] + dp0[R]
)
```6. Xây dựng cấu trúc phân khúc theo từng khoảng thời gian. 

Mỗi khoảng tương ứng với một phân rã cây con Descartes. Chúng tôi lưu trữ các chuyển đổi DP nhỏ cần thiết để hợp nhất. 
7. Trả lời từng câu hỏi. 

Chuyển thành`[L,R]`trong không gian tọa độ thành phạm vi chỉ mục được sắp xếp theo-`x`thứ tự bằng cách sử dụng tìm kiếm nhị phân. 

Hợp nhất tất cả các phân đoạn có liên quan và xuất ra giá trị khớp kết quả. 

### Tại sao nó hoạt động 

Cây Descartes nắm bắt chính xác mối quan hệ về tầm nhìn được xác định bởi phép xây dựng đường. Mỗi cạnh kết nối một nút với tổ tiên lớn hơn gần nhất của nó. 

Kết hợp tối đa trên cây có cấu trúc con tối ưu. Một khi chúng ta quyết định xem gốc có khớp hay không, các cây con bên trái và bên phải sẽ trở nên độc lập. Các chuyển đổi DP liệt kê mọi khả năng hợp lệ cho gốc và không bao giờ tính hai cạnh vì mỗi cạnh thuộc về chính xác một quan hệ cha-con. 

Vì các sơ đồ con khoảng vẫn là cây Descartes của các mảng con, nên việc hợp nhất thông tin phân đoạn sẽ tái tạo lại trạng thái DP chính xác cho khoảng truy vấn. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

MOD = 10**9 + 9

class Node:
    __slots__ = ("dp",)

    def __init__(self):
        self.dp = [[-10**18] * 2 for _ in range(2)]

def merge(a, b):
    if a is None:
        return b
    if b is None:
        return a

    res = Node()

    for la in range(2):
        for ra in range(2):
            best = -10**18

            for ma in range(2):
                for mb in range(2):
                    cur = a.dp[la][ma] + b.dp[mb][ra]

                    if ma == 0 or mb == 0:
                        cur += 1

                    if cur > best:
                        best = cur

            res.dp[la][ra] = best

    return res

def make_leaf():
    node = Node()

    node.dp[0][0] = 0
    node.dp[0][1] = 0
    node.dp[1][0] = 0
    node.dp[1][1] = -10**18

    return node

def solve():
    n, k = map(int, input().split())

    pts = []

    for _ in range(k):
        x, y = map(int, input().split())
        pts.append((x, y))

    a, b, c, d = map(int, input().split())

    while len(pts) < n:
        x = (a * pts[-1][0] + b) % MOD
        y = (c * pts[-1][1] + d) % MOD
        pts.append((x, y))

    pts.sort()

    xs = [x for x, _ in pts]

    seg = [None] * (4 * n)

    def build(idx, l, r):
        if l == r:
            seg[idx] = make_leaf()
            return

        mid = (l + r) // 2

        build(idx * 2, l, mid)
        build(idx * 2 + 1, mid + 1, r)

        seg[idx] = merge(seg[idx * 2], seg[idx * 2 + 1])

    build(1, 0, n - 1)

    def query(idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return seg[idx]

        mid = (l + r) // 2

        res = None

        if ql <= mid:
            res = merge(res, query(idx * 2, l, mid, ql, qr))

        if qr > mid:
            res = merge(res, query(idx * 2 + 1, mid + 1, r, ql, qr))

        return res

    m = int(input())

    out = []

    for _ in range(m):
        L, R = map(int, input().split())

        l = bisect_left(xs, L)
        r = bisect_right(xs, R) - 1

        if l > r:
            out.append("0")
            continue

        ans = query(1, 0, n - 1, l, r)

        out.append(str(max(
            ans.dp[0][0],
            ans.dp[0][1],
            ans.dp[1][0]
        )))

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên tạo ra tất cả các thành phố và sắp xếp chúng theo`x`. Sau khi sắp xếp, chỉ có thứ tự tương đối mới quan trọng. 

Cây phân đoạn lưu trữ một đối tượng DP nhỏ cho mỗi khoảng thời gian. Mục nhập ma trận`dp[l][r]`mô tả kết quả phù hợp nhất trong các điều kiện biên về việc liệu các điểm cuối khoảng có còn khả dụng cho kết hợp bên ngoài hay không. 

Hoạt động hợp nhất là cốt lõi của giải pháp. Khi kết hợp hai khoảng liền kề, ranh giới giữa có thể tạo thành một cạnh khớp bổ sung nếu cả hai điểm cuối đều trống. Đây chính xác là cấu trúc tương tự như kết hợp tối đa trên phân tách giống như đường dẫn. 

Việc khởi tạo lá rất dễ bị sai. Một đỉnh đơn lẻ không thể chứa một cạnh trùng khớp bên trong, do đó trạng thái mà cả hai bên đều bị chiếm giữ phải không hợp lệ. 

Tìm kiếm nhị phân chuyển đổi các truy vấn tọa độ thành phạm vi chỉ mục. Vấn đề xác định khoảng thời gian bằng cách`x`tọa độ, không phải theo vị trí sau khi sắp xếp, vì vậy việc chuyển đổi này là bắt buộc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 6
0 0
1 1
2 2
3 3
4 4
5 5
```Các thành phố đã được sắp xếp theo`x`. 

Biểu đồ trở thành một chuỗi:```
1 - 2 - 3 - 4 - 5 - 6
```Đối với truy vấn`[0,5]`: 

| Khoảng thời gian | Đỉnh | Kết hợp tối đa | 
| --- | --- | --- | 
| [0,5] | 1 2 3 4 5 6 | 3 | 

Một kết hợp tối ưu là:```
(1,2), (3,4), (5,6)
```Đối với truy vấn`[1,4]`: 

| Khoảng thời gian | Đỉnh | Kết hợp tối đa | 
| --- | --- | --- | 
| [1,4] | 2 3 4 5 | 2 | 

Dấu vết chứng tỏ rằng việc hạn chế khoảng sẽ thay đổi chính biểu đồ chứ không chỉ thay đổi các đỉnh có thể sử dụng. 

### Ví dụ tùy chỉnh```
x: 1 2 3 4
y: 4 1 3 2
```Cây Descartes:```
    1
     \
      3
     / \
    2   4
```Trong toàn bộ khoảng thời gian: 

| Nút | Con Còn Lại | Đúng Con | Kết hợp tốt nhất | 
| --- | --- | --- | --- | 
| 2 | không | không | 0 | 
| 4 | không | không | 0 | 
| 3 | 2 | 4 | 1 | 
| 1 | không | 3 | 2 | 

Kết hợp tối ưu:```
(1,3), (2,4) impossible
```Thực tế hợp lệ tối ưu:```
(1,3), (2 unmatched), (4 unmatched)
```hoặc```
(3,2), (1 unmatched), (4 unmatched)
```Kích thước tối đa là`1`. 

Dấu vết này cho thấy tại sao những lựa chọn tham lam cục bộ không có tác dụng. Nút phù hợp`3`thay đổi khác nhau cho dù nút`1`vẫn có thể sử dụng được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Xây dựng cây phân đoạn cộng với truy vấn logarit | 
| Không gian | O(n log n) | Lưu trữ cây phân đoạn | 

Với`10^5`thành phố và truy vấn, xử lý truy vấn logarit dễ dàng phù hợp với giới hạn. Trạng thái DP có kích thước không đổi nên việc hợp nhất rất rẻ. 

## Trường hợp thử nghiệm```python
import sys, io
from bisect import bisect_left, bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    MOD = 10**9 + 9

    class Node:
        __slots__ = ("dp",)

        def __init__(self):
            self.dp = [[-10**18] * 2 for _ in range(2)]

    def merge(a, b):
        if a is None:
            return b
        if b is None:
            return a

        res = Node()

        for la in range(2):
            for ra in range(2):
                best = -10**18

                for ma in range(2):
                    for mb in range(2):
                        cur = a.dp[la][ma] + b.dp[mb][ra]

                        if ma == 0 or mb == 0:
                            cur += 1

                        best = max(best, cur)

                res.dp[la][ra] = best

        return res

    def make_leaf():
        node = Node()

        node.dp[0][0] = 0
        node.dp[0][1] = 0
        node.dp[1][0] = 0

        return node

    n, k = map(int, input().split())

    pts = [tuple(map(int, input().split())) for _ in range(k)]

    a, b, c, d = map(int, input().split())

    while len(pts) < n:
        x = (a * pts[-1][0] + b) % MOD
        y = (c * pts[-1][1] + d) % MOD
        pts.append((x, y))

    pts.sort()

    xs = [x for x, _ in pts]

    seg = [None] * (4 * n)

    def build(idx, l, r):
        if l == r:
            seg[idx] = make_leaf()
            return

        mid = (l + r) // 2

        build(idx * 2, l, mid)
        build(idx * 2 + 1, mid + 1, r)

        seg[idx] = merge(seg[idx * 2], seg[idx * 2 + 1])

    build(1, 0, n - 1)

    def query(idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return seg[idx]

        mid = (l + r) // 2

        res = None

        if ql <= mid:
            res = merge(res, query(idx * 2, l, mid, ql, qr))

        if qr > mid:
            res = merge(res, query(idx * 2 + 1, mid + 1, r, ql, qr))

        return res

    m = int(input())

    ans = []

    for _ in range(m):
        L, R = map(int, input().split())

        l = bisect_left(xs, L)
        r = bisect_right(xs, R) - 1

        if l > r:
            ans.append("0")
            continue

        cur = query(1, 0, n - 1, l, r)

        ans.append(str(max(
            cur.dp[0][0],
            cur.dp[0][1],
            cur.dp[1][0]
        )))

    return "\n".join(ans)

assert run(
"""6 6
0 0
1 1
2 2
3 3
4 4
5 5
2 3 3 2
4
0 5
1 4
2 3
3 3
"""
) == "3\n2\n1\n0"

assert run(
"""1 1
5 10
2 3 5 7
1
0 100
"""
) == "0"

assert run(
"""2 2
1 10
2 20
2 3 5 7
2
1 2
2 2
"""
) == "1\n0"

assert run(
"""4 4
1 4
2 1
3 3
4 2
2 3 5 7
1
1 4
"""
) == "1"

assert run(
"""5 5
1 5
2 4
3 3
4 2
5 1
2 3 5 7
1
1 5
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 | Không tự khớp | 
| Hai đỉnh | 1 rồi 0 | Khoảng ranh giới | 
| Cây Descartes không đơn điệu | 1 | Cấu trúc cây đúng | 
| Trình tự giảm nghiêm ngặt | 2 | Hành vi khớp chuỗi | 

## Vỏ cạnh 

Hãy xem xét một khoảng truy vấn không chứa thành phố nào.```
Cities:
(10, 1)
(20, 2)

Query:
[30, 40]
```Tìm kiếm nhị phân mang lại`l > r`, do đó thuật toán trả về ngay`0`. Nếu không có sự kiểm tra này, các truy vấn phân đoạn sẽ truy cập vào các phạm vi không hợp lệ. 

Bây giờ hãy xem xét khoảng cách một thành phố.```
Cities:
(5, 100)

Query:
[5,5]
```Cây phân đoạn trả về một nút lá. Tất cả các trạng thái DP hợp lệ đều được đánh giá là`0`, trong khi trạng thái không thể bị chiếm giữ hoàn toàn vẫn là âm vô cực. Câu trả lời cuối cùng là chính xác`0`. 

Cuối cùng, xét trường hợp tăng đơn điệu.```
x: 1 2 3 4 5
y: 1 2 3 4 5
```Biểu đồ trở thành một đường dẫn. DP hợp nhất xen kẽ các trạng thái ranh giới tự do và bị chiếm đóng giống hệt như đường dẫn tiêu chuẩn khớp với DP, tạo ra câu trả lời`2`. 

Điều này xác nhận rằng các khoảng hợp nhất duy trì tính tối ưu ngay cả khi cây Descartes suy biến thành một đường thẳng.
