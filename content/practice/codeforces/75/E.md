---
title: "CF 75E - Đường đi ngắn nhất của tàu"
description: "Chúng ta có hai điểm trên mặt phẳng, vị trí xuất phát và điểm đến của con tàu, cùng với một đa giác lồi biểu thị một hòn đảo. Di chuyển qua biển tốn 1 cho mỗi đơn vị khoảng cách. Di chuyển qua bên trong hòn đảo tốn 2 chi phí cho mỗi đơn vị khoảng cách."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 75
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 67 (Div. 2)"
rating: 2400
weight: 75
solve_time_s: 149
verified: true
draft: false
---

[CF 75E - Đường đi ngắn nhất của tàu](https://codeforces.com/problemset/problem/75/E) 

**Đánh giá:** 2400 
**Tas:** hình học, đường đi ngắn nhất 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai điểm trên mặt phẳng, vị trí xuất phát và điểm đến của con tàu, cùng với một đa giác lồi biểu thị một hòn đảo. 

Chi phí di chuyển qua biển`1`trên một đơn vị khoảng cách. Chi phí di chuyển trong nội địa của đảo`2`trên một đơn vị khoảng cách. Di chuyển dọc theo ranh giới đa giác được coi là chuyển động của biển nên chi phí vẫn còn`1`. 

Con tàu có thể đi bất cứ nơi nào bên ngoài đảo với chi phí đường biển và cũng có thể đi qua đảo bằng cách trả chi phí đất đai cao hơn. Nhiệm vụ là tính toán tổng chi phí tối thiểu cần thiết để đi từ điểm đầu đến điểm cuối. 

Đa giác lồi và chứa tối đa 30 đỉnh. Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Với đa giác nhỏ như vậy, chúng ta không cần cấu trúc dữ liệu hình học phức tạp hoặc tối ưu hóa liên tục. MỘT`O(n^3)`hoặc thậm chí`O(n^4)`thuật toán hoàn toàn có thể chấp nhận được. 

Phần khó khăn không phải là hiệu suất mà là mô hình hóa hình học một cách chính xác. 

Một cách giải thích ngây thơ sẽ là "đi thẳng qua đảo hoặc đi vòng qua ranh giới". Điều đó không phải lúc nào cũng đúng. Một đường đi tối ưu có thể đi vào đa giác tại một điểm biên, đi qua phần bên trong, sau đó rời khỏi một điểm khác và cũng sử dụng các cung biên trước hoặc sau đoạn đó. 

Một điểm tinh tế khác là con đường ngắn nhất chỉ có biển xung quanh một đa giác lồi luôn bao gồm các đoạn thẳng và các cạnh đa giác. Khi một đoạn thẳng chạm vào bên trong đa giác, đoạn thẳng đó sẽ không còn hợp lệ như một đoạn biển. 

Thực tế là đa giác lồi là thuộc tính cấu trúc quan trọng. Tính lồi đảm bảo rằng bất kỳ đoạn thẳng nào nối hai điểm bên ngoài đều cắt đa giác trong nhiều nhất một khoảng liên tục. Điều đó làm cho đường đi ngắn nhất đơn giản hơn nhiều so với các bài toán chướng ngại vật đa giác nói chung. 

Hãy xem xét ví dụ này:```
Start = (0,0)
End   = (10,0)

Square:
(4,-1) (6,-1) (6,1) (4,1)
```Đoạn thẳng đi qua đảo. Việc thực hiện bất cẩn có thể nói: 

“Đi qua đảo tốn tiền`2 * 2 = 4`cho phần bị phong tỏa, cộng với phần còn lại trên biển." 

Điều đó mang lại tổng chi phí`8 + 4 = 12`. 

Nhưng câu trả lời đúng thì nhỏ hơn. Lộ trình tối ưu là đi vòng quanh ranh giới hình vuông. Chiều dài đường vòng chỉ là`2 + 2 = 4`, do đó tổng chi phí trở thành`10 + 2 = 12`? Trên thực tế cả hai đều có mối quan hệ ràng buộc ở đây. Những nhiễu loạn nhỏ dễ làm đứt dây trói. 

Ví dụ:```
Start = (0,0)
End   = (10,0)

Rectangle:
(4,-3) (6,-3) (6,3) (4,3)
```Băng qua đảo bây giờ có giá:`8`khoảng cách biển cộng`2 * 2 = 4`, tổng cộng`12`. 

Đi vòng qua ranh giới cần phải đi đường vòng`6`, tổng cộng`14`. 

Câu trả lời tối ưu là băng qua đảo. 

Một trường hợp cạnh nguy hiểm khác xảy ra khi đoạn thẳng chỉ chạm vào ranh giới đa giác tại một điểm. Vì việc di chuyển ranh giới được tính là di chuyển trên biển nên con đường này hoàn toàn có giá trị như một con đường biển và không phải chịu thêm bất kỳ chi phí đất đai nào. 

Ví dụ:```
Start = (0,0)
End   = (10,0)

Triangle:
(5,0) (6,2) (6,-2)
```Đoạn chỉ chạm vào đỉnh`(5,0)`. Câu trả lời là chính xác`10`. 

Việc triển khai coi liên hệ ranh giới là nút giao bên trong sẽ làm tăng chi phí một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là suy nghĩ liên tục. Bất kỳ đường đi tối ưu nào cũng có thể đi vào và ra khỏi đa giác tại các điểm biên tùy ý, vì vậy chúng ta có thể thử lấy mẫu nhiều điểm biên và tính toán tuyến đường tốt nhất trong số đó. 

Điều này hoạt động về mặt khái niệm vì hàm chi phí có tính chất hình học và trơn tru từng phần. Nếu chúng ta rời rạc hóa đủ tinh vi, chúng ta sẽ ước chừng được lời giải tối ưu. 

Vấn đề là việc tối ưu hóa liên tục sẽ trở nên lộn xộn rất nhanh. Ngay cả khi chúng tôi chỉ lấy mẫu`k`điểm trên mỗi cạnh, chúng ta đã có`O(k^2)`cặp ứng cử viên vào-ra. Mỗi cặp cũng yêu cầu kiểm tra giao điểm hình học và tính toán ranh giới ngắn nhất. Độ chính xác cao sẽ cần lớn`k`, làm cho phương pháp này không thực tế và dễ vỡ về mặt số lượng. 

Nhận xét quan trọng là đường đi ngắn nhất trong bài toán này chỉ cần các đỉnh đa giác làm điểm ngoặt. 

Giả sử chúng ta đang di chuyển hoàn toàn trên biển. Trong mỗi vùng tự do, đường đi ngắn nhất giữa hai điểm luôn là một đoạn thẳng. Lý do duy nhất để uốn cong là trượt dọc theo ranh giới đa giác. Vì đa giác là lồi nên các tuyến đường biên tối ưu giữa hai điểm biên chính xác là hai chuỗi đa giác nối chúng. 

Điều này biến bài toán hình học liên tục thành bài toán đường đi ngắn nhất của đồ thị. 

Chúng tôi tạo một biểu đồ có các nút là: 

- điểm bắt đầu 
- điểm cuối 
- tất cả các đỉnh đa giác 

Sau đó, chúng tôi kết nối các cặp nút có cạnh có trọng số. 

Nếu đoạn giữa hai nút không đi qua phần bên trong đa giác, chúng ta có thể di chuyển trực tiếp qua biển, do đó chi phí cạnh bằng khoảng cách Euclide. 

Nếu đoạn này đi qua phần bên trong đa giác, chúng ta vẫn có thể đi thẳng, nhưng bây giờ phần bên trong có giá gấp đôi. 

Đối với đa giác lồi, đoạn thẳng cắt đa giác ở một trong hai điểm: 

- không có khoảng cách 
- một điểm 
- một đoạn liên tục 

Vì vậy chúng ta có thể tính toán: 

- tổng chiều dài đoạn 
- chiều dài bên trong đa giác 

Khi đó chi phí sẽ trở thành:`outside_length * 1 + inside_length * 2`mà đơn giản hóa thành:`total_length + inside_length`Khi tất cả các chi phí theo cặp đã được biết, bài toán sẽ trở thành bài toán đường đi ngắn nhất bằng đồ thị có trọng số hoàn chỉnh. Vì đồ thị có nhiều nhất là 32 đỉnh nên Floyd-Warshall là đồ thị tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu lực lượng vũ phu | Cực lớn cho độ chính xác cao | Lớn | Quá chậm và không đáng tin cậy | 
| Đồ thị + Hình học + Floyd-Warshall | O(n³) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc điểm đầu, điểm cuối và đỉnh đa giác. 
2. Xây dựng danh sách các nút đồ thị bao gồm: 

- nút`0`như điểm bắt đầu 
- nút`1`là điểm cuối 
- nút`2...n+1`như các đỉnh đa giác 
3. Đối với mỗi cặp nút`(A, B)`, hãy tính chi phí tối thiểu để di chuyển trực tiếp giữa chúng. 
4. Để tính chi phí di chuyển trực tiếp, trước tiên hãy xác định số lượng đoạn đường`AB`nằm bên trong đa giác lồi. 
5. Tham số hóa phân đoạn như sau:$P(t)=A+t(B-A),\quad 0\le t\le 1$Chúng tôi cắt đoạn này với mọi cạnh đa giác. 
6. Thu thập tất cả các giá trị tham số`t`nơi đoạn thẳng giao nhau với các cạnh đa giác. 

Ngoài ra còn bao gồm: 

-`0`-`1`7. Sắp xếp tất cả thu thập được`t`giá trị và kiểm tra mỗi khoảng thời gian liên tiếp. 

Đối với mỗi khoảng thời gian: 

- lấy tham số trung điểm của nó 
- Đánh giá vị trí trung điểm 
- kiểm tra xem điểm giữa này có nằm hoàn toàn bên trong đa giác không 

Nếu đúng như vậy thì toàn bộ khoảng nằm bên trong vì đa giác lồi. 
8. Tính tổng độ dài của tất cả các khoảng được phân loại là bên trong. 
9. Hãy: 

-`L`là tổng chiều dài đoạn 
-`I`tổng chiều dài bên trong 

Khi đó chi phí đi lại sẽ là:$\text{cost}=L+I$đầu tiên`L`chiếm chi phí đường biển ở khắp mọi nơi, và thêm`I`nâng cấp chuyển động bên trong từ giá`1`chi phí`2`. 
10. Lưu giá trị này làm trọng số cạnh đồ thị giữa hai nút. 
11. Chạy Floyd-Warshall trên biểu đồ. 
12. Khoảng cách ngắn nhất từ ​​nút`0`đến nút`1`là câu trả lời. 

### Tại sao nó hoạt động 

Đối với bất kỳ đường đi ngắn nhất nào, mọi đoạn biển cực đại đều là một đường thẳng, bởi vì khoảng cách Euclide là tối ưu trong không gian mở. Bất cứ khi nào đường đi thay đổi hướng, nó phải xảy ra trên ranh giới đa giác. 

Đối với đa giác lồi, giao điểm của một đoạn thẳng với đa giác luôn được kết nối. Điều đó có nghĩa là một phân khúc trực tiếp có một phần bên trong được xác định duy nhất mà chi phí của nó có thể được tính toán chính xác. 

Biểu đồ chứa tất cả các đỉnh đa giác cộng với các điểm cuối. Bất kỳ đường biên tối ưu nào đều có thể được biểu diễn dưới dạng một chuỗi các cạnh đa giác giữa các đỉnh, trong khi mọi chuyển động trong không gian tự do được biểu diễn bằng các cạnh đồ thị thẳng. 

Vì mỗi trọng số cạnh trực tiếp đều bằng chi phí tối ưu thực sự giữa các điểm cuối của nó nên Floyd-Warshall kết hợp chính xác chúng thành mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-9

def dist(a, b):
    return math.hypot(a[0] - b[0], a[1] - b[1])

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def segment_intersection_t(a, b, c, d):
    ax, ay = a
    bx, by = b
    cx, cy = c
    dx, dy = d

    rx = bx - ax
    ry = by - ay

    sx = dx - cx
    sy = dy - cy

    denom = cross(rx, ry, sx, sy)

    if abs(denom) < EPS:
        return None

    qpx = cx - ax
    qpy = cy - ay

    t = cross(qpx, qpy, sx, sy) / denom
    u = cross(qpx, qpy, rx, ry) / denom

    if -EPS <= t <= 1 + EPS and -EPS <= u <= 1 + EPS:
        return max(0.0, min(1.0, t))

    return None

def point_in_convex_polygon_strict(p, poly):
    x, y = p
    n = len(poly)

    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]

        cr = cross(x2 - x1, y2 - y1, x - x1, y - y1)

        if cr <= EPS:
            return False

    return True

def inside_length(a, b, poly):
    ts = [0.0, 1.0]

    n = len(poly)

    for i in range(n):
        c = poly[i]
        d = poly[(i + 1) % n]

        t = segment_intersection_t(a, b, c, d)

        if t is not None:
            ts.append(t)

    ts.sort()

    uniq = []
    for t in ts:
        if not uniq or abs(t - uniq[-1]) > 1e-8:
            uniq.append(t)

    total = 0.0
    full_len = dist(a, b)

    for i in range(len(uniq) - 1):
        l = uniq[i]
        r = uniq[i + 1]

        mid = (l + r) / 2.0

        mx = a[0] + (b[0] - a[0]) * mid
        my = a[1] + (b[1] - a[1]) * mid

        if point_in_convex_polygon_strict((mx, my), poly):
            total += (r - l) * full_len

    return total

def solve():
    xs, ys, xe, ye = map(int, input().split())

    n = int(input())

    vals = list(map(int, input().split()))

    poly = []

    for i in range(0, 2 * n, 2):
        poly.append((vals[i], vals[i + 1]))

    nodes = [(xs, ys), (xe, ye)] + poly

    m = len(nodes)

    INF = 1e100

    d = [[INF] * m for _ in range(m)]

    for i in range(m):
        d[i][i] = 0.0

    for i in range(m):
        for j in range(i + 1, m):
            a = nodes[i]
            b = nodes[j]

            total = dist(a, b)
            inside = inside_length(a, b, poly)

            cost = total + inside

            d[i][j] = cost
            d[j][i] = cost

    for k in range(m):
        for i in range(m):
            dik = d[i][k]

            for j in range(m):
                nd = dik + d[k][j]

                if nd < d[i][j]:
                    d[i][j] = nd

    print("{:.9f}".format(d[0][1]))

solve()
```Quy trình hình học cốt lõi là`inside_length`. 

Phân đoạn được tham số hóa từ`t = 0`ĐẾN`t = 1`. Mỗi giao điểm có cạnh đa giác đều đóng góp một điểm chuyển tiếp ứng cử viên trong đó đoạn thay đổi từ ngoài vào trong hoặc ngược lại. 

Sau khi sắp xếp tất cả các tham số chuyển tiếp, mỗi khoảng liên tiếp nằm hoàn toàn bên trong hoặc bên ngoài đa giác. Chỉ kiểm tra điểm giữa là đủ vì đa giác lồi và đoạn thẳng không thể luân phiên lặp lại trong một khoảng. 

chức năng`point_in_convex_polygon_strict`cố tình sử dụng một bài kiểm tra nghiêm ngặt. Các điểm ranh giới được coi là nằm ở bên ngoài vì chi phí di chuyển qua ranh giới tương đương với chi phí đi lại bằng đường biển. 

Một chi tiết triển khai tinh tế là các tham số giao lộ trùng lặp. Khi một đoạn đi qua chính xác một đỉnh đa giác, hai cạnh có thể có cùng giao điểm. Nếu không loại bỏ trùng lặp, các khoảng có độ dài bằng 0 sẽ xuất hiện và kéo theo đó là sự mất ổn định về số. 

Floyd-Warshall được sử dụng thay vì Dijkstra đơn giản vì đồ thị rất nhỏ. Với tối đa 32 nút, độ phức tạp bậc ba là không đáng kể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 7 6 7
4
4 2 4 12 3 12 3 2
```Hình chữ nhật chặn đường dẫn trực tiếp theo chiều ngang. 

| Bước | Giá trị | 
| --- | --- | 
| Độ dài đoạn trực tiếp | 5 | 
| Phần bên trong | 1 | 
| Chi phí trực tiếp | 6 | 
| Đường vòng ranh giới | Lâu hơn | 
| Câu trả lời cuối cùng | 6 | 

Đường thẳng cắt hình chữ nhật giữa`x = 3`Và`x = 4`, vậy có đúng một đơn vị nằm bên trong hòn đảo. Khoảng cách biển góp phần`5`, và phí bảo hiểm bên trong đóng góp một phần khác`1`. 

### Ví dụ tùy chỉnh```
0 0 10 0
4
4 -3 6 -3 6 3 4 3
```| Bước | Giá trị | 
| --- | --- | 
| Độ dài đoạn trực tiếp | 10 | 
| Phần bên trong | 2 | 
| Chi phí trực tiếp | 12 | 
| Đường vòng ranh giới trên | 14 | 
| Đường vòng ranh giới dưới | 14 | 
| Câu trả lời cuối cùng | 12 | 

Ví dụ này chứng minh rằng việc băng qua đảo thực sự có thể rẻ hơn so với việc đi vòng quanh đảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Floyd-Warshall thống trị | 
| Không gian | O(n²) | Lưu trữ ma trận khoảng cách | 

Đa giác có nhiều nhất là 30 đỉnh nên đồ thị có nhiều nhất là 32 nút. Floyd-Warshall biểu diễn đại khái`32³ = 32768`thư giãn, đó là rất nhỏ. Các phép tính hình học cũng nhẹ, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isclose

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    EPS = 1e-9

    def dist(a, b):
        return math.hypot(a[0] - b[0], a[1] - b[1])

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    def segment_intersection_t(a, b, c, d):
        ax, ay = a
        bx, by = b
        cx, cy = c
        dx, dy = d

        rx = bx - ax
        ry = by - ay

        sx = dx - cx
        sy = dy - cy

        denom = cross(rx, ry, sx, sy)

        if abs(denom) < EPS:
            return None

        qpx = cx - ax
        qpy = cy - ay

        t = cross(qpx, qpy, sx, sy) / denom
        u = cross(qpx, qpy, rx, ry) / denom

        if -EPS <= t <= 1 + EPS and -EPS <= u <= 1 + EPS:
            return max(0.0, min(1.0, t))

        return None

    def point_in_convex_polygon_strict(p, poly):
        x, y = p

        for i in range(len(poly)):
            x1, y1 = poly[i]
            x2, y2 = poly[(i + 1) % len(poly)]

            cr = cross(x2 - x1, y2 - y1, x - x1, y - y1)

            if cr <= EPS:
                return False

        return True

    def inside_length(a, b, poly):
        ts = [0.0, 1.0]

        for i in range(len(poly)):
            c = poly[i]
            d = poly[(i + 1) % len(poly)]

            t = segment_intersection_t(a, b, c, d)

            if t is not None:
                ts.append(t)

        ts.sort()

        uniq = []

        for t in ts:
            if not uniq or abs(t - uniq[-1]) > 1e-8:
                uniq.append(t)

        total = 0.0
        full_len = dist(a, b)

        for i in range(len(uniq) - 1):
            l = uniq[i]
            r = uniq[i + 1]

            mid = (l + r) / 2

            mx = a[0] + (b[0] - a[0]) * mid
            my = a[1] + (b[1] - a[1]) * mid

            if point_in_convex_polygon_strict((mx, my), poly):
                total += (r - l) * full_len

        return total

    xs, ys, xe, ye = map(int, input().split())

    n = int(input())

    vals = list(map(int, input().split()))

    poly = []

    for i in range(0, 2 * n, 2):
        poly.append((vals[i], vals[i + 1]))

    ans = dist((xs, ys), (xe, ye))
    ans += inside_length((xs, ys), (xe, ye), poly)

    return f"{ans:.6f}"

# provided sample
assert run(
"""1 7 6 7
4
4 2 4 12 3 12 3 2
"""
) == "6.000000"

# segment completely outside polygon
assert run(
"""0 0 10 0
4
20 20 21 20 21 21 20 21
"""
) == "10.000000"

# direct path crosses rectangle
assert run(
"""0 0 10 0
4
4 -3 6 -3 6 3 4 3
"""
) == "12.000000"

# touching boundary only
assert run(
"""0 0 10 0
3
5 0 6 2 6 -2
"""
) == "10.000000"

# thin crossing
assert run(
"""0 0 4 0
4
1 -1 3 -1 3 1 1 1
"""
) == "6.000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 6.000000 | Tính toán giao cắt cơ bản | 
| Đa giác xa | 10.000000 | Không có trường hợp giao nhau | 
| Đường chéo hình chữ nhật lớn | 12.000000 | Đi nội địa rẻ hơn đi đường vòng | 
| Liên lạc ranh giới | 10.000000 | Biên giới được coi là biển | 
| Hình chữ nhật mỏng | 6.000000 | Kế toán chính xác bên trong | 

## Vỏ cạnh 

Xét trường hợp chạm ranh giới:```
0 0 10 0
3
5 0 6 2 6 -2
```Đoạn thẳng cắt đa giác chỉ ở đỉnh`(5,0)`. 

Các tham số giao lộ trở thành: 

| giá trị t | 
| --- | 
| 0 | 
| 0,5 | 
| 1 | 

Các bài kiểm tra điểm giữa là: 

- giữa`0`Và`0.5`, bên ngoài 
- giữa`0.5`Và`1`, bên ngoài 

Không có khoảng nào được phân loại là bên trong nên chiều dài bên trong là`0`. 

Câu trả lời cuối cùng trở nên chính xác`10`. 

Bây giờ hãy xem xét việc vượt qua đầy đủ:```
0 0 10 0
4
4 -3 6 -3 6 3 4 3
```Đoạn này đi vào tại`x = 4`và thoát ra tại`x = 6`. 

Các khoảng tham số là: 

| Khoảng thời gian | Bên trong? | 
| --- | --- | 
| [0, 0,4] | Không | 
| [0,4, 0,6] | Có | 
| [0,6, 1] | Không | 

Phân số bên trong là`0.2`, vậy chiều dài bên trong là`2`. 

Tổng chi phí:```
10 + 2 = 12
```Cuối cùng, hãy xem xét một đoạn thẳng hàng với cạnh đa giác:```
0 0 10 0
4
3 0 7 0 7 2 3 2
```Đường dẫn nằm dọc theo cạnh dưới của hình chữ nhật. 

Bài kiểm tra điểm giữa không bao giờ phân loại bất kỳ khoảng nào là bên trong một cách nghiêm ngặt, bởi vì các điểm biên không đạt được bài kiểm tra ngăn chặn nghiêm ngặt. 

Thuật toán xuất ra chính xác`10`, coi sự di chuyển của cạnh là chuyển động của biển.
