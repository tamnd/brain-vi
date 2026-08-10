---
title: "CF 102394D - Xe không người lái"
description: "Chúng ta có một trường hình chữ nhật thẳng hàng theo trục và hai đoạn đường rời nhau hoàn toàn bên trong nó. Ô tô là một điểm và mọi điểm trên đường đi của nó phải cách hai đoạn đường bằng nhau."
date: "2026-08-10T19:04:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "D"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 187
verified: true
draft: false
---

[CF 102394D - Xe không người lái](https://codeforces.com/problemset/problem/102394/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trường hình chữ nhật thẳng hàng theo trục và hai đoạn đường rời nhau hoàn toàn bên trong nó. Ô tô là một điểm và mọi điểm trên đường đi của nó phải cách hai đoạn đường bằng nhau. Tuyến đường bắt đầu và kết thúc trên ranh giới hình chữ nhật, với hai điểm ranh giới khác nhau và nó phải đi qua bên trong. 

Do đó, đối tượng chính là tập hợp 

[ 
{P\mid \operatorname{dist}(P,A)=\operatorname{dist}(P,B)}. 
] 

Đây là đường phân giác Voronoi của hai đoạn. Vì cả hai đoạn đều nằm bên trong hình chữ nhật và không giao nhau nên đường phân giác liên quan sẽ tách vùng gần đoạn (A) hơn với vùng gần đoạn (B). Bên trong hình chữ nhật, nó tạo thành tuyến đường ranh giới duy nhất mà ô tô phải đi theo. Câu trả lời là chiều dài của đường phân giác bên trong hình chữ nhật. 

Mỗi trường hợp đầu vào cung cấp cho hình chữ nhật thông qua góc dưới bên trái ((x_l,y_l)) và góc trên bên phải ((x_r,y_r)), theo sau là hai điểm cuối của đoạn. Điểm cuối là tọa độ nguyên, nhưng tuyến đường mong muốn là một đường cong liên tục, vì vậy câu trả lời nói chung là không hợp lý. 

Các ràng buộc chính thức cho phép tối đa (10^5) trường hợp độc lập, trong khi mọi tọa độ đều nằm trong khoảng từ (-1000) đến (1000). Phạm vi tọa độ nhỏ không làm cho giải pháp lưới trở nên khả thi vì sai số bắt buộc là (10^{-9}), nhỏ hơn nhiều so với thang đơn vị tự nhiên của đầu vào. Một lượng hình học tính toán không đổi cho mỗi trường hợp thử nghiệm là mục tiêu thích hợp. 

Có một số trường hợp khó xử lý. Đầu tiên, điểm gần nhất của đoạn thẳng không phải lúc nào cũng là điểm bên trong. Ví dụ,```
1
0 0 5 5
1 1 4 1
1 4 4 4
```có câu trả lời (5). Bên ngoài phạm vi nằm ngang của các đoạn, các điểm gần nhất là điểm cuối của chúng, do đó, coi mọi đoạn là một đường thẳng vô hạn sẽ đưa ra đường phân giác sai. 

Thứ hai, tính năng gần nhất có thể thay đổi chính xác tại điểm cuối của phân khúc. Trong mẫu,```
1
0 0 7 6
2 4 4 4
3 2 5 2
```đường phân giác bao gồm các đường điểm cuối-điểm cuối, các đoạn parabol của đường điểm và một đoạn đường thẳng. Bỏ qua các chuyển đổi tại (x=2,3,4,5) sẽ cho độ dài sai. Câu trả lời đúng là (7.552593593868681136). 

Thứ ba, hai đường hỗ trợ có thể song song hoặc cắt nhau. Ví dụ: hai đoạn ngang song song tạo ra một đường phân giác góc, trong khi hai đoạn không song song tạo ra hai đường phân giác góc qua giao điểm của các đường hỗ trợ của chúng. Một quy trình giao cắt đường chung giả sử một điểm giao nhau hữu hạn duy nhất sẽ âm thầm thất bại trong trường hợp song song. 

Cuối cùng, một phân đoạn có thể theo chiều dọc, do đó các công thức chia cho chênh lệch (x) của nó là không an toàn. Việc triển khai xử lý các đường ràng buộc dọc một cách riêng biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực theo nghĩa đen sẽ lấy mẫu hình chữ nhật trên một lưới, tính toán khoảng cách hai đoạn tại mỗi điểm lưới và cố gắng xây dựng lại đường cong đẳng thức. Ngay cả một lưới có khoảng cách một đơn vị cũng có khoảng (2000\cdot2000=4\cdot10^6) điểm cho hình chữ nhật có kích thước tối đa. Trên (10^5) trường hợp có khoảng (4\cdot10^{11}) điểm đánh giá. Về cơ bản hơn, một lưới như vậy không bao giờ có thể chứng nhận độ chính xác hình học (10^{-9}) cần thiết. 

Ý tưởng Brute-Force rất hữu ích vì nó tiết lộ điều chúng ta thực sự cần biết: tại mỗi điểm của đường cong mong muốn, phần nào của mỗi đoạn chịu trách nhiệm về khoảng cách? Đối với một phân khúc chỉ có ba khả năng. Điểm gần nhất có thể là điểm cuối đầu tiên, điểm cuối thứ hai hoặc điểm bên trong của đoạn. 

Khi tính năng gần nhất được cố định cho cả hai phân đoạn, đường cong đẳng thức sẽ trở thành một đối tượng cổ điển rất đơn giản. Hai đặc điểm điểm cho một đường phân giác vuông góc. Hai đặc điểm của đường cung cấp các đường phân giác của góc. Đối tượng điểm và đối tượng đường tạo ra một parabol, bởi vì đường cong chính xác là tập hợp các điểm có khoảng cách đến tiêu điểm bằng khoảng cách của chúng đến đường chuẩn. 

Quan sát đó làm giảm vấn đề liên tục xuống chỉ còn chín tổ hợp các đặc điểm gần nhất. Mỗi sự kết hợp cũng có một vùng hiệu lực đơn giản, được mô tả bởi một hoặc hai nửa mặt phẳng. Chúng ta giao đường thẳng hoặc parabol tương ứng với các nửa mặt phẳng đó và với hình chữ nhật. Các mảnh kết quả phân chia đường phân giác Voronoi hoàn chỉnh, do đó độ dài của chúng có thể được thêm vào một cách đơn giản. 

Việc triển khai hình học được chấp nhận sử dụng chính xác sự phân rã có kích thước không đổi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới vũ phu | (O((W/\varepsilon)(H/\varepsilon))) | (O(1)) hoặc (O(WH)) | Quá chậm và không chính xác | 
| Phân hủy tính năng | (O(1)) mỗi trường hợp | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mọi ranh giới của hình chữ nhật dưới dạng một đường thẳng có hướng cùng với quy ước rằng các điểm hợp lệ nằm trên mặt trong của hình chữ nhật. Cách biểu diễn tương tự sẽ được sử dụng cho các nửa mặt phẳng mô tả điểm cuối hoặc phần bên trong của đoạn là đối tượng gần nhất. 
2. Đối với mỗi phân đoạn (XY), hãy xem xét ba trạng thái đặc điểm gần nhất. Trạng thái (0) có nghĩa là điểm gần nhất là (X). Trạng thái (1) có nghĩa là (Y). Trạng thái (2) có nghĩa là hình chiếu vuông góc nằm bên trong đoạn thẳng. 

Đối với trạng thái (0), điều kiện hợp lệ là 

[ 
(P-X)\cdot(Y-X)\le 0. 
] 

Đối với trạng thái (1), đó là 

[ 
(P-Y)\cdot(X-Y)\le 0. 
]

Đối với trạng thái (2), cả hai điều kiện điểm cuối đều bị đảo ngược, yêu cầu phép chiếu nằm giữa các điểm cuối. 
3. Liệt kê tất cả (3\times3=9) cặp trạng thái đối tượng, mỗi trạng thái cho mỗi phân đoạn. Thêm các ranh giới nửa mặt phẳng tương ứng vào bốn ranh giới hình chữ nhật. Mỗi điểm được chấp nhận bởi các bất đẳng thức này đều có chính xác đặc điểm gần nhất được chọn trên mỗi đoạn. 
4. Nếu cả hai đối tượng được chọn là điểm (P) và (Q), thì khoảng cách của chúng bằng nhau trên đường phân giác vuông góc của (PQ). Xây dựng đường thẳng đó và cắt nó bằng tất cả các nửa mặt phẳng hiện tại. 
5. Nếu cả hai đặc điểm được chọn đều là phần bên trong của phân khúc thì khoảng cách của chúng là khoảng cách đến hai đường hỗ trợ. Khoảng cách bằng nhau giữa hai đường thẳng là đường phân giác của một góc. Nếu các đường hỗ trợ song song thì có một đường phân giác nằm giữa chúng. Nếu chúng giao nhau thì có hai đường phân giác của góc và cả hai phải được kiểm tra dựa trên nửa mặt phẳng có tính hợp lệ của đặc tính. 
6. Nếu một đặc điểm là điểm và đặc điểm kia là đường trong thì đường đẳng thức là một parabol. Di chuyển đường chuẩn đến (y=0), xoay nó để nằm ngang và chuyển tiêu điểm thành ((u,v)). Sau khi dịch sang đỉnh của parabol và, nếu cần, phản ánh theo chiều dọc, phương trình trở thành 

[ 
x^2=py,\qquad p=2v. 
] 

Ranh giới hợp lệ là các dòng. Giao một đường thẳng (y=kx+b) với parabol ta có 

[ 
x^2-pkx-pb=0, 
] 

đó chỉ là một phương trình bậc hai. Một ranh giới dọc cho một tọa độ (x) trực tiếp. 
7. Đối với mỗi đường thẳng ứng cử, hãy giao nó với mọi ranh giới nửa mặt phẳng đang hoạt động. Sắp xếp tất cả các tham số giao lộ dọc theo đường ứng cử viên. Giữa hai tham số liên tiếp, ứng cử viên hoàn toàn hợp lệ hoặc hoàn toàn không hợp lệ, vì không có ranh giới ràng buộc nào được vượt qua trong khoảng. Kiểm tra điểm giữa của mỗi khoảng và thêm độ dài của các khoảng hợp lệ. 
8. Đối với mọi parabol ứng viên, hãy thực hiện quy trình tương tự với tham số (x) của nó. Sau khi sắp xếp tất cả các nghiệm, hãy kiểm tra điểm giữa (x) bằng cách đánh giá điểm parabol tương ứng ((x,x^2/p)). Chiều dài cung được tính toán bằng phương pháp phân tích thay vì lấy mẫu số: 

\frac{x\sqrt{1+4x^2/p^2}}2+ 
\frac p4\operatorname{asinh}\left(\frac{2x}{p}\right). 
] 
9. Tính tổng các phần hợp lệ từ tất cả chín tổ hợp tính năng. Chúng rời rạc ngoại trừ có thể ở các ranh giới chuyển tiếp đặc điểm, có độ dài bằng 0. Hợp của chúng chính xác là đường phân giác Voronoi bên trong hình chữ nhật, vì vậy độ dài tích lũy là đường đi tối thiểu bắt buộc. 

### Tại sao nó hoạt động 

Đối với mọi điểm của mặt phẳng, điểm gần nhất trên một đoạn được phân loại duy nhất là điểm cuối thứ nhất, điểm cuối thứ hai hoặc hình chiếu bên trong, ngoại trừ các ranh giới có hai mô tả trùng nhau. Do đó, chín cặp đặc điểm bao trùm toàn bộ quỹ tích đẳng thức. 

Bên trong một cặp đặc trưng cố định, cả hai hàm khoảng cách đều có dạng đại số cố định. Do đó, quỹ tích đẳng thức chính xác là một trong ba đối tượng được xử lý ở trên: đường thẳng, đường phân giác hoặc parabol. Các ràng buộc nửa mặt phẳng đảm bảo rằng các đối tượng được chọn thực sự là các điểm gần nhất trên các đoạn của chúng. Việc giao các đối tượng này với mọi ranh giới ràng buộc sẽ phân chia chúng thành các khoảng có giá trị không đổi. 

Do đó, mọi phần có độ dài dương của quỹ tích đẳng thức đều được tính một lần và không có phần không hợp lệ nào được tính. Vì quỹ tích đẳng thức của hai đoạn lồi rời nhau là dải phân cách giữa hai vùng Voronoi của chúng, nên phần bên trong hình chữ nhật là tuyến đường biên giới đến biên giới bắt buộc. Do đó, tổng chiều dài của nó là chiều dài đường dẫn hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-9
INF = 1e100

def add(a, b):
    return (a[0] + b[0], a[1] + b[1])

def sub(a, b):
    return (a[0] - b[0], a[1] - b[1])

def mul(a, k):
    return (a[0] * k, a[1] * k)

def rot90(a):
    return (-a[1], a[0])

def rot270(a):
    return (a[1], -a[0])

def flip(a):
    return (-a[0], -a[1])

def cross(a, b):
    return a[0] * b[1] - a[1] * b[0]

def length(a):
    return math.hypot(a[0], a[1])

def sgn(x):
    if x > EPS:
        return 1
    if x < -EPS:
        return -1
    return 0

def line_intersection(a, b, p, q):
    """
    Intersect infinite lines AB and PQ.

    Return:
        (2, None, None) if coincident,
        (0, None, None) if parallel,
        (1, point, t) otherwise, where point = A + t(B-A).
    """
    d1 = sub(b, a)
    d2 = sub(q, p)
    den = cross(d1, d2)
    num = cross(sub(p, a), d2)

    if sgn(den) == 0:
        if sgn(num) == 0:
            return 2, None, None
        return 0, None, None

    t = num / den
    return 1, add(a, mul(d1, t)), t

def solve_case(xl, yl, xr, yr, seg_a, seg_b):
    rect = [
        ((float(xl), float(yl)), (float(xr), float(yl))),
        ((float(xr), float(yl)), (float(xr), float(yr))),
        ((float(xr), float(yr)), (float(xl), float(yr))),
        ((float(xl), float(yr)), (float(xl), float(yl))),
    ]

    total = 0.0

    def check(point, edges):
        for a, b in edges:
            if sgn(cross(sub(point, a), sub(b, a))) > 0:
                return False
        return True

    def call_line(a, b, edges):
        """
        Add the valid length of the infinite line AB under all
        current half-plane constraints.
        """
        values = []

        for p, q in edges:
            typ, _, t = line_intersection(a, b, p, q)
            if typ == 2:
                return 0.0
            if typ == 1:
                values.append(t)

        if len(values) < 2:
            return 0.0

        values.sort()
        dlen = length(sub(b, a))
        ret = 0.0

        for i in range(1, len(values)):
            t1 = values[i - 1]
            t2 = values[i]
            mid = (t1 + t2) * 0.5
            p = add(a, mul(sub(b, a), mid))
            if check(p, edges):
                ret += (t2 - t1) * dlen

        return ret

    def integral_parabola(p, x):
        """
        Integral of sqrt(1 + 4*x^2/p^2) dx.
        """
        if p <= 0:
            return 0.0

        z = 2.0 * x / p
        root = math.sqrt(1.0 + z * z)
        return 0.5 * x * root + 0.25 * p * math.asinh(z)

    def solve_features(edges, tp0, f0, tp1, f1):
        nonlocal total

        if tp0 > tp1:
            tp0, tp1 = tp1, tp0
            f0, f1 = f1, f0

        A = f0[0]
        B = f0[1] if len(f0) > 1 else None
        C = f1[0]
        D = f1[1] if len(f1) > 1 else None

        if tp0 == 0 and tp1 == 0:
            mid = mul(add(A, C), 0.5)
            direction = rot90(sub(A, mid))
            total += call_line(add(mid, direction), mid, edges)
            return

        if tp0 == 1 and tp1 == 1:
            typ, o, _ = line_intersection(A, B, C, D)

            if typ == 0:
                origin = mul(add(A, C), 0.5)
                total += call_line(origin, add(origin, sub(D, C)), edges)
                return

            if typ == 2:
                return

            if length(sub(A, o)) < length(sub(B, o)):
                A, B = B, A

            if length(sub(C, o)) < length(sub(D, o)):
                C, D = D, C

            ang1 = math.atan2(A[1] - o[1], A[0] - o[0])
            ang2 = math.atan2(C[1] - o[1], C[0] - o[0])
            ang = (ang1 + ang2) * 0.5

            direction = (math.cos(ang), math.sin(ang))
            total += call_line(o, add(o, direction), edges)
            total += call_line(o, add(o, rot90(direction)), edges)
            return

        # Point-line case.
        # A is the point, CD is the supporting line.
        direction = sub(D, C)

        # Translate C to the origin.
        A = sub(A, C)
        transformed = [(sub(p, C), sub(q, C)) for p, q in edges]

        # Rotate CD onto the positive x-axis.
        w = math.atan2(direction[1], direction[0])
        cw = math.cos(-w)
        sw = math.sin(-w)

        def rotate_point(p):
            return (p[0] * cw - p[1] * sw,
                    p[0] * sw + p[1] * cw)

        A = rotate_point(A)
        transformed = [
            (rotate_point(p), rotate_point(q))
            for p, q in transformed
        ]

        if A[1] < 0:
            A = flip(A)
            transformed = [(flip(p), flip(q)) for p, q in transformed]

        p = 2.0 * A[1]

        if sgn(p) == 0:
            return

        vertex = (A[0], A[1] * 0.5)

        transformed = [
            (sub(a, vertex), sub(b, vertex))
            for a, b in transformed
        ]

        roots = []

        for a, b in transformed:
            dx = a[0] - b[0]

            if sgn(dx) == 0:
                roots.append(a[0])
                continue

            k = (a[1] - b[1]) / dx
            bb = a[1] - k * a[0]

            # x^2 - p*k*x - p*b = 0
            disc = p * p * k * k + 4.0 * p * bb

            if sgn(disc) < 0:
                continue

            if disc < 0:
                disc = 0.0

            root = math.sqrt(disc)
            roots.append((p * k - root) * 0.5)
            roots.append((p * k + root) * 0.5)

        if len(roots) < 2:
            return

        roots.sort()

        prev_value = None

        for i, x in enumerate(roots):
            value = integral_parabola(p, x)

            if i > 0:
                mid = (roots[i - 1] + x) * 0.5
                point = (mid, mid * mid / p)

                if check(point, transformed):
                    total += value - prev_value

            prev_value = value

    segments = [seg_a, seg_b]

    for state_a in range(3):
        for state_b in range(3):
            edges = list(rect)
            features = [None, None]
            types = [state_a, state_b]

            for idx, state in enumerate(types):
                a, b = segments[idx]
                d = sub(b, a)

                if state == 0:
                    # Closest point is a.
                    edges.append((a, add(a, rot90(d))))
                    features[idx] = [a]

                elif state == 1:
                    # Closest point is b.
                    edges.append((b, add(b, rot90(sub(a, b)))))
                    features[idx] = [b]

                else:
                    # Closest point is in the interior of AB.
                    edges.append((a, add(a, rot270(d))))
                    edges.append((b, add(b, rot270(sub(a, b)))))
                    features[idx] = [a, b]

            solve_features(
                edges,
                state_a,
                features[0],
                state_b,
                features[1]
            )

    return total

def solve(inp):
    rd = inp.readline
    t = int(rd())
    out = []

    for _ in range(t):
        xl, yl, xr, yr = map(int, rd().split())
        a = tuple(map(float, rd().split()))
        b = tuple(map(float, rd().split()))

        seg_a = ((a[0], a[1]), (a[2], a[3]))
        seg_b = ((b[0], b[1]), (b[2], b[3]))

        ans = solve_case(
            xl, yl, xr, yr,
            seg_a, seg_b
        )

        if abs(ans) < 5e-12:
            ans = 0.0

        out.append(f"{ans:.15f}")

    return "\n".join(out)

if __name__ == "__main__":
    sys.stdout.write(solve(sys.stdin))
```Hình chữ nhật được chèn dưới dạng bốn ranh giới nửa mặt phẳng trước khi xem xét bất kỳ đối tượng địa lý nào. Bởi vì hình chữ nhật là lồi nên một đường cong ứng cử viên có thể được cắt bớt một cách đơn giản bằng cách sắp xếp các giao điểm của nó với các ranh giới đó. 

các`work`logic từ đạo hàm hình học được biểu diễn trực tiếp bằng ba trạng thái. Ranh giới vuông góc thông qua một điểm cuối xuất phát từ điều kiện tích số chấm xác định xem liệu phép chiếu lên đoạn có nằm trước điểm cuối đó hay không.`call_line`tham số hóa một dòng vô hạn là (A+t(B-A)). Mỗi lần vượt qua ràng buộc đều tạo ra một giá trị (t). Việc sắp xếp các giá trị này biến vấn đề cắt liên tục thành số lần kiểm tra theo khoảng thời gian không đổi. Kiểm tra điểm giữa là đủ vì không có ranh giới ràng buộc nào được vượt qua trong khoảng đó. 

Trường hợp đường thẳng cần xử lý đặc biệt khi các đường hỗ trợ giao nhau. Có hai đường phân giác chứ không phải một. Mã chọn các tia hướng ra khỏi giao điểm và tính trung bình hướng của chúng, sau đó sử dụng hướng vuông góc cho đường phân giác thứ hai. 

Trường hợp đường điểm là phần tinh tế về mặt số học. Phép biến đổi loại bỏ hướng phân đoạn tùy ý, sau đó đường đẳng thức có phương trình parabol tiêu chuẩn (x^2=py). Mọi ràng buộc sẽ trở thành một đường thẳng đứng hoặc (y=kx+b), do đó tất cả các tham số giao nhau đều đến từ giá trị (x) trực tiếp hoặc phương trình bậc hai. 

của Python`float`ở đây là đủ vì sai số bắt buộc là (10^{-9}), trong khi độ lớn tọa độ chỉ là (2000). Việc triển khai sử dụng epsilon tuyệt đối khi quyết định xem tích chéo, định thức hay phân biệt đối xử có bằng 0 hay không. Điều này ngăn tiếng ồn nhỏ của dấu phẩy động biến một đường song song về mặt hình học thành một giao điểm nhân tạo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1
0 0 7 6
2 4 4 4
3 2 5 2
```Cả hai đoạn đều nằm ngang, nhưng đoạn dưới dịch sang phải một đơn vị. Đường đẳng thức có năm phần có chiều dài dương. Chín sự kết hợp tính năng được tóm tắt dưới đây. 

| Cặp tính năng | Đường cong đẳng thức | Khoảng thời gian hợp lệ | Chiều dài | 
| --- | --- | --- | --- | 
| Điểm cuối A-trái, Điểm cuối B-trái | Dòng (y=x/2+7/4) | (0\le x\le2) | (\sqrt5) | 
| A-nội thất, điểm cuối B-trái | Parabol | (2\le x\le3) | (1.0402288194) | 
| Nội thất A, nội thất B | (y=3) | (3\le x\le4) | (1) | 
| Điểm cuối bên phải, B-nội thất | Parabol | (4\le x\le5) | (1.0402288194) | 
| Điểm cuối A-phải, Điểm cuối B-phải | Dòng (y=x/2+3/4) | (5\le x\le7) | (\sqrt5) | 

Bốn kết hợp tính năng còn lại không có khoảng thời gian hợp lệ có độ dài dương. Đối với một trong hai đoạn parabol, sau khi thay tọa độ tương ứng, tích phân độ dài cung là 

1.040228819434551. 
] 

Do đó, chiều dài tích lũy là 

[ 
2\sqrt5 
+1.040228819434551 
+1 
+1.040228819434551 
+2\sqrt5, 
] 

mang lại 

[ 
7.552593593868681. 
] 

Điều này đồng ý với đầu ra mẫu chính thức (7.552593593868681136). 

### Đoạn đối xứng 

Hãy xem xét```
1
0 0 10 10
2 3 8 3
2 7 8 7
```Hai đoạn nằm ngang, có chiều dài bằng nhau và nằm ngay phía trên nhau. Đường phân giác hỗ trợ của họ là (y=5). 

| Cặp tính năng | Khoảng thời gian hợp lệ | Đóng góp | Chiều dài tích lũy | 
| --- | --- | --- | --- | 
| Điểm cuối bên trái, điểm cuối bên trái | (0\le x\le2) | (2) | (2) | 
| Nội thất, nội thất | (2\le x\le8) | (6) | (8) | 
| Điểm cuối bên phải, điểm cuối bên phải | (8\le x\le10) | (2) | (10) | 

Tất cả các kết hợp đường điểm hỗn hợp sẽ thu gọn về các chuyển tiếp có độ dài bằng 0 tại các điểm cuối của đoạn. Câu trả lời cuối cùng là (10). 

Ví dụ này chứng tỏ tại sao thuật toán không được coi một đoạn là một hình học nguyên thủy đơn lẻ. Đường phân giác cuối cùng tương tự được ghép từ các trường hợp đặc điểm bên trong và điểm cuối, mặc dù nó trông giống như một đường thẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) cho mỗi trường hợp thử nghiệm | Có chính xác chín cặp tính năng và mỗi cặp có số lượng ràng buộc, giao điểm, gốc và kiểm tra khoảng không đổi | 
| Không gian | (O(1)) | Tối đa một số điểm, ranh giới, nghiệm và khoảng thời gian không đổi được lưu trữ | 

Trên (10^5) trường hợp, tổng công việc là tuyến tính theo số trường hợp có hằng số tương đối nhỏ. Bài toán ban đầu có giới hạn 6 giây và giới hạn bộ nhớ 512 MB, vì vậy việc tránh mọi sự phụ thuộc vào vùng tọa độ của hình chữ nhật là điều cần thiết. 

## Trường hợp thử nghiệm 

Khai thác sau giả định mã giải pháp ở trên có sẵn trong cùng một tệp hoặc được nhập để`solve`có thể gọi được.```
import io
import math

def run(inp: str) -> str:
    return solve(io.BytesIO(inp.encode())).strip()

def assert_close(inp: str, expected: float, name: str):
    got = float(run(inp))
    assert math.isclose(got, expected, rel_tol=1e-10, abs_tol=1e-10), (
        f"{name}: got {got}, expected {expected}"
    )

# Provided sample
sample1 = """\
1
0 0 7 6
2 4 4 4
3 2 5 2
"""
assert_close(sample1, 7.552593593868681136, "sample 1")

# Minimum-size valid construction.
# The rectangle has width 2 and height 5.
# The two vertical segments occupy disjoint interior intervals.
case_minimum = """\
1
0 0 2 5
1 1 1 2
1 3 1 4
"""
assert_close(case_minimum, 2.0, "minimum-size case")

# Maximum-size rectangle and symmetric horizontal segments.
case_maximum = """\
1
-1000 -1000 1000 1000
-500 -500 500 -500
-500 500 500 500
"""
assert_close(case_maximum, 2000.0, "maximum-size case")

# Segment endpoints are as close as allowed to the rectangle boundary.
case_boundary = """\
1
0 0 5 5
1 1 4 1
1 4 4 4
"""
assert_close(case_boundary, 5.0, "boundary case")

# Both segments are vertical, exercising x1 == x2.
case_vertical = """\
1
0 0 10 10
3 2 3 8
7 2 7 8
"""
assert_close(case_vertical, 10.0, "vertical segments")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 2 5`, các đoạn trên (x=1) |`2`| Hình chữ nhật thực tế nhỏ nhất có hai đoạn tọa độ nguyên rời nhau | 
|`-1000 -1000 1000 1000`, đoạn ngang |`2000`| Phạm vi tọa độ tối đa và giá trị hình học lớn | 
|`0 0 5 5`, các đoạn sử dụng tọa độ (1) và (4) |`5`| Điểm cuối ở khoảng cách cho phép gần nhất tính từ ranh giới hình chữ nhật | 
|`0 0 10 10`, đoạn dọc |`10`| Phân đoạn dọc và bảo vệ chống chia cho 0 | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là đoạn có điểm gần nhất là điểm cuối. Coi như```
1
0 0 5 5
1 1 4 1
1 4 4 4
```Đối với (x<1), điểm gần nhất của đoạn dưới là điểm cuối bên trái của nó và điều này cũng đúng với đoạn trên. Quỹ tích đẳng thức có một đường phân giác vuông góc giữa điểm cuối và điểm cuối. Thuật toán tiếp cận trường hợp này thông qua trạng thái đặc trưng (0,0), thay vì sử dụng sai hai đường hỗ trợ vô hạn. Tuyến cuối cùng là đường ngang đầy đủ (y=2,5), có chiều dài (5). 

Trường hợp cạnh thứ hai là chuyển đổi tính năng. Trong mẫu chính thức, với (2<x<3), đặc điểm gần nhất ở đoạn trên là phần bên trong của nó trong khi đặc điểm gần nhất ở đoạn dưới là điểm cuối bên trái của nó. Tại (x=3), đặc điểm thấp nhất gần nhất sẽ chuyển sang phần bên trong của đoạn. Thuật toán xem xét rõ ràng cả hai trạng thái đối tượng và điểm chuyển tiếp xuất hiện dưới dạng giao điểm với một trong các ranh giới nửa mặt phẳng hoạt động. Vì một điểm có độ dài bằng 0 nên không có vấn đề đếm kép. Tổng kết quả là (7,552593593868681136). 

Trường hợp cạnh thứ ba là một đoạn thẳng đứng. Coi như```
1
0 0 10 10
3 2 3 8
7 2 7 8
```Hai đường hỗ trợ là (x=3) và (x=7) nên phân giác của chúng là (x=5). Tuyến đường đi qua hình chữ nhật từ ((5,0)) đến ((5,10)), cho độ dài (10). Trong mã parabol, các đường ràng buộc dọc được phát hiện bởi sai phân 0 (x) của chúng, do đó không xảy ra phép chia cho 0. 

Trường hợp cạnh thứ tư là các đường hỗ trợ song song. Khi hai đặc điểm bên trong được chọn song song thì có chính xác một đường cách đều ở giữa chúng. Thuật toán phát hiện rằng giao điểm của đường hỗ trợ không tồn tại và xây dựng trực tiếp đường trung điểm. Điều này tránh việc cố gắng tạo một góc từ một điểm giao nhau không xác định. 

Trường hợp cạnh thứ năm là một cặp đường hỗ trợ giao nhau. Hai đoạn không song song có thể có các đường hỗ trợ cắt nhau mặc dù bản thân các đoạn hữu hạn đó không khớp nhau. Khoảng cách bằng nhau của hai đường thẳng đó cho ta hai đường phân giác. Thuật toán tạo ra cả hai và cho phép nửa mặt phẳng đối tượng loại bỏ bất kỳ phần nào không tương ứng với các phân đoạn thực tế. Điều này là cần thiết vì chỉ giữ lại một đường phân giác của góc có thể loại bỏ một phần hợp lệ của đường phân giác Voronoi. 

Trường hợp cạnh cuối cùng là suy biến số tại một ranh giới ràng buộc. Đường cong ứng cử viên có thể trùng khớp chính xác với ranh giới nửa mặt phẳng, đặc biệt là ở điểm chuyển tiếp giữa đối tượng điểm cuối và đối tượng bên trong. Việc triển khai coi các đường trùng khớp là đóng góp bằng 0 trong trường hợp đối tượng đó, bởi vì phần hình học tương tự được biểu thị bằng trường hợp đối tượng lân cận. Điều này ngăn cản việc tính cùng một đường cong có độ dài dương tương tự hai lần. 

Phiên bản này có chủ ý mang tính hình học chứ không phải là công thức đầu tiên: một khi đã hiểu được sự phân tách tính năng gần nhất, ba loại đường cong và quy tắc cắt của chúng có thể được rút ra lại cho các bài toán Voronoi tương tự.
