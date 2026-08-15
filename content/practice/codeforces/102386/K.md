---
title: "CF 102386K - \u041c\u0430\u043b\u044b\u0448 \u0438 \u041a\u0430\u0440\u043b\u0441\u043e\u043d"
description: "Chúng ta có một đa giác lồi hoàn toàn có các đỉnh là các điểm mạng nguyên và được liệt kê ngược chiều kim đồng hồ. Chúng ta cần một đường thẳng chia đa giác thành hai vùng có cùng diện tích."
date: "2026-08-14T13:42:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "K"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 246
verified: false
draft: false
---

[CF 102386K - \u041c\u0430\u043b\u044b\u0448 \u0438 \u041a\u0430\u0440\u043b\u0441\u043e\u043d](https://codeforces.com/problemset/problem/102386/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi hoàn toàn có các đỉnh là các điểm mạng nguyên và được liệt kê ngược chiều kim đồng hồ. Chúng ta cần một đường thẳng chia đa giác thành hai vùng có cùng diện tích. Bản thân đường thẳng phải chứa hai điểm mạng nguyên phân biệt, do đó, tương tự, chúng ta cần một đường chia đôi diện tích với độ dốc hợp lý và điểm giao nhau hợp lý. 

Sự tự do hình học quan trọng là chúng ta được phép chọn bất kỳ đường thẳng nào, không nhất thiết phải là đường đi qua các đỉnh đa giác. Tuy nhiên, việc chọn một đỉnh cụ thể sẽ mang lại cho chúng ta một cấu trúc mạnh mẽ hơn nhiều. Sửa đỉnh đầu tiên (V_0). Khi một điểm (P) di chuyển dọc theo đường biên đa giác từ (V_1) về phía (V_{n-1}), đường thẳng (V_0P) quét qua tất cả các vết cắt có thể có mà một điểm cuối trên đường biên là (V_0). Với mỗi vị trí biên (P), diện tích một cạnh của (V_0P) thay đổi liên tục từ 0 đến toàn bộ diện tích đa giác. Vì đa giác lồi nên chính xác một (P) như vậy sẽ cho một nửa diện tích. 

Đầu vào có tối đa (1000) đỉnh và mọi tọa độ có tối đa giá trị tuyệt đối (10^5). Một giải pháp trực tiếp (O(n^2)) đã nhỏ về mặt số lượng ở giới hạn này, khoảng một triệu phép tính hình học cơ bản, nhưng cấu trúc cho phép xây dựng (O(n)). Giới hạn tọa độ cũng làm cho số học số nguyên chính xác trở nên thực tế. Tích chéo của hai hiệu tọa độ tối đa là khoảng (8\cdot10^{10}) và tổng của nhiều nhất (1000) hình tam giác nhỏ hơn (10^{14}). Số nguyên Python không có vấn đề tràn ở đây, trong khi số học 64-bit có dấu cũng đủ để tính toán diện tích. 

Có một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai dấu phẩy động. Đầu tiên, đường cắt được yêu cầu có thể đi qua chính xác một đỉnh đa giác khác. Ví dụ,```
4
0 0
4 0
4 2
0 2
```có diện tích (8) và đường chéo từ ((0,0)) đến ((4,2)) chia nó thành hai diện tích (4). Việc so sánh liên quan đến các tham số dấu phẩy động có thể vô tình đặt điểm cuối vào cạnh tiếp theo. So sánh số nguyên chính xác tránh điều này. 

Một trường hợp khác là khi điểm nửa diện tích nằm hoàn toàn bên trong một cạnh. Mẫu có hành vi chính xác này đối với phần cắt (y=4). Đường thẳng không đi qua một đỉnh đa giác nhưng giao điểm với cạnh thẳng đứng bên phải là một điểm nguyên. Tổng quát hơn, giao điểm đó chỉ được đảm bảo là hợp lý, không phải là số nguyên. Chúng ta phải xây dựng một hướng nguyên cho toàn bộ đường thay vì yêu cầu giao điểm phải là một điểm nguyên. 

Cuối cùng, tổng diện tích nhân đôi có thể là số lẻ. Trong mẫu diện tích nhân đôi là (15), nên một nửa của nó là (15/2). Một giải pháp chỉ dựa trên diện tích số nguyên sẽ kết luận không chính xác rằng việc cắt chính xác là không thể. Điểm cắt có thể đơn giản có tọa độ hợp lý và đường kết quả vẫn có hướng nguyên sau khi chia tỷ lệ. 

## Phương pháp tiếp cận 

Việc xây dựng lực lượng vũ phu tự nhiên bắt đầu bằng cách chọn một đỉnh đa giác (V_i) làm điểm cuối cố định của đường cắt. Sau đó, chúng ta có thể đi vòng quanh ranh giới còn lại, xác định giao điểm của nửa khu vực nằm ở cạnh nào và giải quyết vị trí dọc theo cạnh đó. Nếu toàn bộ tìm kiếm này được lặp lại độc lập cho mỗi (V_i), thì sẽ mất (O(n^2)) thời gian. Đối với (n=1000), đây là khoảng kiểm tra cạnh (10^6), do đó, nó không thực sự bị loại trừ bởi các ràng buộc đã cho, nhưng nó lặp lại chính xác cùng một công việc hình học nhiều lần. 

Quan sát hữu ích là chúng ta không cần phải thử mọi đỉnh. Chọn (V_0) một lần. Đa giác có thể được chia thành các hình tam giác 

[ 
(V_0,V_1,V_2),\quad 
(V_0,V_2,V_3),\quad\ldots,\quad 
(V_0,V_{n-2},V_{n-1}). 
] 

Vì đa giác hoàn toàn lồi và ngược chiều kim đồng hồ nên tất cả các hình tam giác này đều có diện tích dương và diện tích của chúng cộng lại bằng diện tích của đa giác. 

Giả sử điểm nửa diện tích cần thiết nằm trên cạnh (V_iV_{i+1}). Vùng được giới hạn bởi (V_0), chuỗi ranh giới (V_0,V_1,\ldots,V_i,P) và đoạn (PV_0) bao gồm tất cả các tam giác quạt trước cộng với tam giác (V_0V_iP). Dọc theo cạnh (V_iV_{i+1}), diện tích của tam giác sau là tuyến tính ở vị trí (P). Do đó, một khi chúng ta biết cạnh chứa mục tiêu thì vị trí chính xác của (P) chỉ là một phần hữu tỉ của cạnh đó. 

Bước cuối cùng là phần làm cho yêu cầu về mạng trở nên dễ dàng. Viết 

[ 
P=V_i+\frac pq(V_{i+1}-V_i). 
] 

Sau đó 

(q-p)(V_i-V_0)+p(V_{i+1}-V_0). 
] 

Vectơ bên phải có tọa độ nguyên. Nó là vectơ chỉ phương của đường mong muốn, vì vậy (V_0) và (V_0+D) là hai điểm nguyên trên đường cắt. Không cần thiết phải xây dựng (P) bằng cách sử dụng tọa độ dấu phẩy động. 

Điều này cũng chứng tỏ rằng một câu trả lời hợp lệ luôn tồn tại dưới sự đảm bảo của bài toán. Trường hợp đầu ra (-1) không bao giờ đạt được đối với đầu vào hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử mọi đỉnh và quét ranh giới | (O(n^2)) | (O(n)) | Được chấp nhận cho (n\le1000), nhưng không cần thiết | 
| Sửa một đỉnh và quét quạt một lần | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn (V_0) làm đỉnh cố định của đường cắt. Vì đa giác là lồi nên mỗi đường từ (V_0) đến một điểm trên ranh giới đối diện sẽ cắt một vùng được xác định rõ có diện tích tăng liên tục từ 0 đến diện tích đa giác đầy đủ. 
2. Tính diện tích nhân đôi (S) của đa giác bằng cách sử dụng quạt từ (V_0). Với mỗi (i=1,\ldots,n-2), hãy tính 

[ 
T_i=\tên toán tử{cross}(V_i-V_0,V_{i+1}-V_0). 
] 

Tổng của tất cả (T_i) chính xác là diện tích đa giác nhân đôi (S). 

1. Đi qua các tam giác quạt trong khi duy trì`prefix`, diện tích nhân đôi của tất cả các tam giác hoàn chỉnh trước cạnh hiện tại (V_iV_{i+1}). Mục tiêu nửa diện tích là (S/2). Để tránh phân số, hãy kiểm tra 

[ 
2\cdot tiền tố\le S\le2(tiền tố+T_i). 
] 

Cạnh đầu tiên thỏa mãn điều kiện này chứa điểm biên mong muốn (P). Diện tích lồi và diện tích tam giác dương đảm bảo rằng cần có chính xác một cạnh, ngoài sự bình đẳng vô hại ở một đỉnh chung. 

1. Hãy để 

[ 
tiền tố r=S-2\cdot. 
] 

Đây là gấp đôi diện tích cần nhân đôi bên trong tam giác (V_0V_iV_{i+1}). Nếu (P) chia (V_iV_{i+1}) theo tỷ lệ (p), thì 

[ 
\frac pq=\frac{r}{2T_i}. 
] 

Giảm phân số này bằng ước chung lớn nhất của chúng. Chúng ta sử dụng số nguyên xuyên suốt nên ngay cả (S) lẻ cũng không gây ra trường hợp đặc biệt nào. 

1. Xác định 

[ 
a=V_i-V_0,\qquad b=V_{i+1}-V_0. 
] 

Vectơ từ (V_0) về phía (P), nhân với (q), là 

[ 
D=(q-p)a+pb. 
] 

Cả (a) và (b) đều là vectơ số nguyên, vì vậy (D) là vectơ số nguyên. Vì (P) nằm trên ranh giới đa giác và vết cắt có diện tích dương ở cả hai phía nên (D) không thể bằng 0. 

1. Đầu ra (V_0) và (V_0+D). Chúng là các điểm nguyên phân biệt trên cùng một đường chia đôi diện tích. 

Bất biến đằng sau việc xây dựng là`prefix`luôn bằng diện tích gấp đôi của phần đã bị đường quét đi (V_0P). Mỗi tam giác quạt tiếp theo cộng thêm một số dương, do đó cuối cùng mục tiêu (S/2) nằm bên trong đúng một tam giác. Bên trong tam giác đó, diện tích phụ thuộc tuyến tính vào vị trí của (P), cho biết tham số hữu tỷ chính xác được sử dụng để xây dựng vectơ chỉ hướng nguyên. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def solve():
    n = int(input())
    p = [tuple(map(int, input().split())) for _ in range(n)]

    x0, y0 = p[0]

    triangles = []
    total = 0

    for i in range(1, n - 1):
        xi, yi = p[i]
        xj, yj = p[i + 1]

        ax = xi - x0
        ay = yi - y0
        bx = xj - x0
        by = yj - y0

        t = cross(ax, ay, bx, by)
        triangles.append(t)
        total += t

    prefix = 0

    for i in range(1, n - 1):
        t = triangles[i - 1]

        if 2 * prefix <= total <= 2 * (prefix + t):
            r = total - 2 * prefix
            den = 2 * t

            g = gcd(r, den)
            num = r // g
            den //= g

            xi, yi = p[i]
            xj, yj = p[i + 1]

            ax = xi - x0
            ay = yi - y0
            bx = xj - x0
            by = yj - y0

            dx = (den - num) * ax + num * bx
            dy = (den - num) * ay + num * by

            print(x0, y0)
            print(x0 + dx, y0 + dy)
            return

        prefix += t

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán tất cả diện tích tam giác quạt. Việc sử dụng vectơ tương đối với (V_0) sẽ tránh phải viết công thức dây giày đầy đủ và làm cho việc xây dựng hướng sau này sử dụng đại lượng chính xác như nhau. 

Vòng lặp thứ hai tìm kiếm tam giác mục tiêu. Sự so sánh được cố tình viết là`2 * prefix <= total <= 2 * (prefix + t)`. Điều này xử lý cả tổng diện tích nhân đôi chẵn và lẻ và không bao giờ chuyển đổi bất cứ thứ gì thành`float`. 

Khi cạnh mục tiêu được tìm thấy,`r / den`là phân số (p/q) mô tả vị trí của nửa điểm dọc theo cạnh đó.`gcd`không cần thiết để đảm bảo tính chính xác, nhưng việc giảm phân số sẽ giữ cho vectơ chỉ phương thu được nhỏ hơn. 

biểu hiện```
dx = (den - num) * ax + num * bx
```là dạng số nguyên của 

[ 
q(P-V_0)=(q-p)(V_i-V_0)+p(V_{i+1}-V_0). 
] 

Điểm đầu ra có thể cách xa hơn nhiều so với đa giác ban đầu, điều này được cho phép. Kích thước của nó vẫn ở mức an toàn dưới đây (10^{18}). Mỗi chênh lệch tọa độ trong đa giác ban đầu nhiều nhất là (2\cdot10^5), mỗi tam giác quạt có diện tích tối đa gấp đôi (8\cdot10^{10}) và sau khi chia tỷ lệ, tọa độ hướng thu được vẫn ở dưới xa (10^{18}). 

Việc thực hiện không bao giờ in`-1`, bởi vì việc xây dựng chứng minh rằng tồn tại một đường lưới hợp lệ cho mọi đa giác thỏa mãn các đảm bảo đầu vào. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, đa giác là```
(0,3), (3,0), (3,6), (0,7)
```Chiếc quạt ở (V_0=(0,3)) có hai hình tam giác. 

| Cạnh |`prefix`|`t`|`total`| Điều kiện mục tiêu |`r / (2t)`| 
| --- | --- | --- | --- | --- | --- | 
| (V_1V_2) | 0 | 18 | 30 | (0\le30\le36) | (30/36=5/6) | 

Mục tiêu nằm trên (V_1V_2). Như vậy (p=5), (q=6). Liên quan đến (V_0), 

[ 
V_1-V_0=(3,-3), 
\qquad 
V_2-V_0=(3,3). 
] 

Hướng nguyên là 

[ 
(6-5)(3,-3)+5(3,3)=(18,12). 
] 

Do đó chương trình có thể xuất ra```
0 3
18 15
```Đường có độ dốc (12/18=2/3). Dòng của mẫu (y=4) là một câu trả lời hợp lệ khác, vì vậy dự kiến ​​sẽ có các kết quả đầu ra chính xác khác nhau. 

Ví dụ thứ hai, hãy xem xét một tam giác vuông.```
3
0 0
4 0
0 4
```Chỉ có một tam giác quạt. 

| Cạnh |`prefix`|`t`|`total`|`r`| Giảm phần | 
| --- | --- | --- | --- | --- | --- | 
| (V_1V_2) | 0 | 16 | 16 | 16 | (1/2) | 

Điểm nửa diện tích là trung điểm của (V_1V_2) nên đường cắt đi từ ((0,0)) đến ((2,2)). Công thức định hướng cho 

[ 
(2-1)(4,0)+1(0,4)=(4,4). 
] 

Vì vậy kết quả đầu ra của chương trình```
0 0
4 4
```Đường thẳng (y=x) chia tam giác thành hai tam giác bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Một lượt tính toán các vùng quạt và một lượt xác định cạnh mục tiêu | 
| Không gian | (O(n)) | Các đỉnh và diện tích tam giác quạt được lưu trữ | 

Giải pháp tuyến tính dễ dàng phù hợp (n\le1000). Các phép toán chủ yếu là cộng số nguyên, nhân, so sánh và tính toán một gcd. Mọi quyết định hình học đều chính xác nên lời giải không phụ thuộc vào độ chính xác về mặt số học. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm dưới đây kiểm tra dây chuyền sản xuất về mặt hình học một cách chính xác`Fraction`số học. Điều này rất hữu ích vì câu trả lời không phải là duy nhất, vì vậy việc so sánh kết quả đầu ra với một cặp điểm cụ thể sẽ loại bỏ nhiều giải pháp đúng một cách không chính xác.```python
import sys
import io
from fractions import Fraction
from math import gcd, atan2

def solve_text(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    p = [(int(next(it)), int(next(it))) for _ in range(n)]

    x0, y0 = p[0]

    triangles = []
    total = 0

    for i in range(1, n - 1):
        xi, yi = p[i]
        xj, yj = p[i + 1]

        ax = xi - x0
        ay = yi - y0
        bx = xj - x0
        by = yj - y0

        t = ax * by - ay * bx
        triangles.append(t)
        total += t

    prefix = 0

    for i in range(1, n - 1):
        t = triangles[i - 1]

        if 2 * prefix <= total <= 2 * (prefix + t):
            r = total - 2 * prefix
            den = 2 * t

            g = gcd(r, den)
            num = r // g
            den //= g

            xi, yi = p[i]
            xj, yj = p[i + 1]

            ax = xi - x0
            ay = yi - y0
            bx = xj - x0
            by = yj - y0

            dx = (den - num) * ax + num * bx
            dy = (den - num) * ay + num * by

            return f"{x0} {y0}\n{x0 + dx} {y0 + dy}\n"

        prefix += t

    return "-1\n"

def polygon_area2(poly):
    s = 0
    n = len(poly)
    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]
        s += x1 * y2 - y1 * x2
    return s

def clip_halfplane(poly, A, B, keep_positive):
    ax, ay = A
    bx, by = B
    dx = bx - ax
    dy = by - ay

    def side(P):
        px, py = P
        return dx * (py - ay) - dy * (px - ax)

    result = []

    for i in range(len(poly)):
        P = poly[i]
        Q = poly[(i + 1) % len(poly)]

        fP = side(P)
        fQ = side(Q)

        inP = fP >= 0 if keep_positive else fP <= 0
        inQ = fQ >= 0 if keep_positive else fQ <= 0

        if inP:
            result.append(P)

        if inP != inQ:
            t = Fraction(fP, fP - fQ)
            x = P[0] + t * (Q[0] - P[0])
            y = P[1] + t * (Q[1] - P[1])
            result.append((x, y))

    return result

def area2_fraction(poly):
    if len(poly) < 3:
        return Fraction(0)

    s = Fraction(0)
    for i in range(len(poly)):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % len(poly)]
        s += x1 * y2 - y1 * x2
    return abs(s)

def valid_answer(inp: str, out: str) -> bool:
    data = inp.strip().split()
    n = int(data[0])
    poly = []
    pos = 1

    for _ in range(n):
        poly.append((int(data[pos]), int(data[pos + 1])))
        pos += 2

    ans = out.strip().split()
    if len(ans) != 4:
        return False

    A = (int(ans[0]), int(ans[1]))
    B = (int(ans[2]), int(ans[3]))

    if A == B:
        return False

    if any(abs(v) > 10**18 for v in A + B):
        return False

    total = Fraction(abs(polygon_area2(poly)))

    positive = clip_halfplane(poly, A, B, True)
    negative = clip_halfplane(poly, A, B, False)

    ap = area2_fraction(positive)
    an = area2_fraction(negative)

    return ap * 2 == total or an * 2 == total

def make_max_polygon():
    vectors = []

    for x in range(1, 51):
        for y in range(0, 51):
            if x == 0 and y == 0:
                continue
            if gcd(x, y) == 1:
                vectors.append((x, y))

    vectors.sort(key=lambda v: atan2(v[1], v[0]))
    vectors = vectors[:500]

    edges = vectors[:]

    for x, y in vectors:
        edges.append((-x, -y))

    poly = []
    x = 0
    y = 0

    for dx, dy in edges:
        poly.append((x, y))
        x += dx
        y += dy

    min_x = min(x for x, y in poly)
    max_x = max(x for x, y in poly)
    min_y = min(y for x, y in poly)
    max_y = max(y for x, y in poly)

    shift_x = -(min_x + max_x) // 2
    shift_y = -(min_y + max_y) // 2

    return [(x + shift_x, y + shift_y) for x, y in poly]

sample1 = """\
4
0 3
3 0
3 6
0 7
"""

assert valid_answer(sample1, solve_text(sample1)), "sample 1"

triangle = """\
3
0 0
4 0
0 4
"""

assert valid_answer(triangle, solve_text(triangle)), "minimum-size triangle"

half_vertex = """\
4
0 0
4 0
4 2
0 2
"""

assert valid_answer(half_vertex, solve_text(half_vertex)), "half-area at a vertex"

boundary_coordinates = """\
3
-100000 -100000
100000 -100000
0 100000
"""

assert valid_answer(
    boundary_coordinates,
    solve_text(boundary_coordinates)
), "boundary coordinates"

max_poly = make_max_polygon()
assert len(max_poly) == 1000
assert max(abs(x) <= 10**5 and abs(y) <= 10**5 for x, y in max_poly)

max_input = str(len(max_poly)) + "\n"
max_input += "\n".join(f"{x} {y}" for x, y in max_poly) + "\n"

assert valid_answer(max_input, solve_text(max_input)), "maximum-size polygon"

# A polygon with all coordinates equal cannot satisfy the input guarantees:
# three distinct vertices would be impossible. Such a test is intentionally
# excluded because it is not a valid instance of the problem.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu được cung cấp | Bất kỳ đường nửa diện tích chính xác nào, chẳng hạn như đầu ra của chương trình | Giao lộ hợp lý ngay trong một cạnh | 
| (3)-đỉnh tam giác vuông | Một đường thẳng đi qua hai điểm nguyên chia đôi tam giác | Đa giác hợp lệ tối thiểu | 
| (4\times2) hình chữ nhật | Một đường chéo qua một đỉnh đối diện | Mục tiêu nửa diện tích chính xác tại ranh giới tam giác quạt | 
| Tam giác sử dụng tọa độ (\pm10^5) | Bất kỳ dòng số nguyên hợp lệ nào trong giới hạn đầu ra (10^{18}) | Số học tọa độ biên | 
| Đa giác được tạo (1000) -vertex | Bất kỳ dòng số nguyên hợp lệ nào | Quét tối đa (n) và tuyến tính | 
| Tất cả tọa độ đều bằng nhau | Không tồn tại đầu vào hợp lệ | Xác nhận tại sao đây không thể là một trường hợp thử nghiệm hợp pháp | 

## Vỏ cạnh 

Khi điểm nửa diện tích chính xác là một đỉnh đa giác, mục tiêu có thể được chia sẻ bởi hai hình tam giác quạt liên tiếp. Vì```
4
0 0
4 0
4 2
0 2
```tam giác quạt đầu tiên có diện tích gấp đôi (8), trong khi toàn bộ đa giác có diện tích gấp đôi (16). Mục tiêu đạt được chính xác tại (V_2=(4,2)). Phép so sánh số nguyên chấp nhận cạnh đầu tiên có đẳng thức, đưa ra hướng (V_2-V_0=(4,2)). Không có epsilon nào liên quan. 

Khi điểm nửa diện tích nằm hoàn toàn bên trong một cạnh, tham số là hợp lý. Trong mẫu, tam giác quạt thứ nhất có diện tích gấp đôi (18), trong khi tổng diện tích là (30). Phân số cần tìm là 

[ 
\frac{30}{36}=\frac56. 
] 

Do đó, điểm này là hợp lý, nhưng hướng được chia tỷ lệ 

[ 
(6-5)(3,-3)+5(3,3)=(18,12) 
] 

là không thể thiếu. Do đó, dòng đầu ra chứa hai điểm nguyên mặc dù giao điểm ranh giới của nó không cần phải là điểm nguyên. 

Khi tổng diện tích nhân đôi là số lẻ thì thuật toán vẫn hoạt động không thay đổi. Nếu (S=15), mục tiêu là (7,5) tính theo đơn vị diện tích gấp đôi. Phép so sánh nhân mọi thứ với hai, do đó nó tìm kiếm 

[ 
2\cdot tiền tố\le15\le2(tiền tố+T_i). 
] 

Tử số kết quả là số lẻ, nhưng`gcd`thường rút gọn phân số hữu tỉ. Không cần giả định tính chẵn lẻ về diện tích đa giác. 

Khi tọa độ gần giới hạn đầu vào, mọi tính toán vẫn chính xác. Các tọa độ đa giác góp phần tạo ra sự khác biệt tối đa là (2\cdot10^5) và một hình tam giác hình quạt có diện tích tối đa là gấp đôi (8\cdot10^{10}). Ngay cả sau khi chia tỷ lệ theo hướng hợp lý, tọa độ đầu ra vẫn ở mức thoải mái bên dưới (10^{18}). Các số nguyên có độ chính xác tùy ý của Python làm cho phép tính số học trở nên đơn giản. 

Đầu vào suy biến với tất cả các đỉnh bằng nhau hoặc có ba đỉnh thẳng hàng sẽ làm mất hiệu lực các giả định hình học được sử dụng trong công trình. Những đầu vào như vậy được loại trừ một cách rõ ràng bởi vấn đề, do đó thuật toán không cần xử lý phòng thủ đối với chúng.
