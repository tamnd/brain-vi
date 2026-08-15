---
title: "CF 102354F - Ngã Tư Vũ Trụ"
description: "Chúng ta có hai tập hợp (n) đường thẳng không định hướng đi qua gốc tọa độ. Mỗi dòng được biểu thị bằng hai giao điểm của nó với hình cầu đơn vị, do đó mọi tập hợp đều chứa (2n) vectơ đơn vị và mọi vectơ xuất hiện cùng với phần phủ định của nó."
date: "2026-08-15T17:42:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "F"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 610
verified: false
draft: false
---

[CF 102354F - Ngã tư vũ trụ](https://codeforces.com/problemset/problem/102354/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp (n) đường thẳng không định hướng đi qua gốc tọa độ. Mỗi dòng được biểu thị bằng hai giao điểm của nó với hình cầu đơn vị, do đó mọi tập hợp đều chứa (2n) vectơ đơn vị và mọi vectơ xuất hiện cùng với phần phủ định của nó. 

Bộ sưu tập thứ hai thu được từ bộ sưu tập đầu tiên bằng cách áp dụng một phép quay quanh gốc tọa độ và sau đó hoán vị các điểm. Nhiệm vụ là khôi phục bất kỳ phép quay nào như vậy và hoán vị tương ứng. Vì một đường không có hướng ưu tiên nên điểm cuối có cùng đường kính là khớp có thể chấp nhận được sau khi xoay. 

Giới hạn trên (n=4\cdot 10^4) có nghĩa là có thể có (8\cdot 10^4) điểm trong mỗi bộ sưu tập. Thuật toán (O(n^2)) sẽ yêu cầu các phép toán cặp khoảng (6,4\cdot 10^9), vượt xa giới hạn bốn giây. Chúng ta cần một cái gì đó gần với (O(n\log n)), chỉ với một lượng nhỏ đại số tuyến tính có kích thước không đổi cho mỗi điểm. Tọa độ có tới mười hai chữ số thập phân, do đó việc triển khai phải sử dụng dấu phẩy động một cách cẩn thận, nhưng câu lệnh cung cấp đủ lề chính xác để hoạt động với độ chính xác gấp đôi thông thường. 

Sự tinh tế đầu tiên là sự biểu diễn đối cực. Nếu (p) đại diện cho một dòng thì (-p) đại diện chính xác cho cùng một dòng. Bất kỳ bất biến nào không thay đổi bởi (p\mapsto -p) đều không thể phân biệt được hai điểm đó. Điều này được mong đợi và vô hại vì sau khi xoay, chúng ta có thể chọn bất kỳ điểm cuối nào mang lại hoán vị cần thiết. 

Điều tinh tế thứ hai là bất biến khoảng cách bậc hai ở đây là vô ích. Đối với một vectơ đơn vị (p), tổng bình phương khoảng cách từ (p) đến tất cả các điểm đầu vào là không đổi vì đầu vào chứa mọi điểm cùng với điểm đối diện của nó. Ví dụ, với```
2
1 0 0
-1 0 0
0 1 0
0 -1 0
1 0 0
-1 0 0
0 1 0
0 -1 0
```phép quay và hoán vị danh tính (1\ 2\ 3\ 4) là hợp lệ, nhưng mọi điểm đều có tổng bình phương chính xác như nhau. Một phương pháp dựa trên đại lượng đó không thể phân biệt được điều gì. 

Điểm tinh tế thứ ba là ngay cả bất biến bậc bốn hữu ích cũng có thể có các giá trị bằng nhau cho các đường khác nhau trong một cấu hình đối xứng đặc biệt. Bản thân mẫu có tính đối xứng như vậy. Việc triển khai bất cẩn giả định hai điểm được sắp xếp đầu tiên luôn là các điểm cuối đối diện nhau có thể vô tình cố gắng tạo một khung từ hai vectơ song song. Việc triển khai đúng sẽ tìm kiếm rõ ràng hai điểm không song song. Đối với mẫu, hai điểm đầu tiên đã không song song nên có thể sử dụng được. 

Cuối cùng, điều kiện hướng ngẫu nhiên có ý nghĩa quan trọng. Bất biến bậc bốn không phải là một dấu vân tay hoàn chỉnh xác định cho các tập hợp điểm tùy ý. Đối với một tập hợp các hướng ngẫu nhiên thống nhất, hai đường thẳng khác nhau chỉ bất biến bằng nhau với xác suất bằng 0 trong số học chính xác và các va chạm số là hoàn toàn khó xảy ra. Đây chính là nguồn gốc của sự độc đáo. Cách tiếp cận bất biến cơ bản cũng là giải pháp tiêu chuẩn được mô tả cho vấn đề này. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là đoán xem hai điểm nào của bộ sưu tập thứ hai tương ứng với hai điểm không song song của bộ sưu tập thứ nhất. Hai vectơ không song song có hướng xác định một phép quay duy nhất, sau khi chọn dấu thích hợp cho cặp thứ hai. Sau đó, chúng ta có thể xoay mọi điểm và kiểm tra xem tập kết quả có khớp với tập đầu tiên hay không. 

Có (O(n^2)) lựa chọn cho cặp trong bộ sưu tập thứ hai và kiểm tra vòng quay một ứng cử viên so với tất cả (O(n)) chi phí điểm (O(n)). Điều đó mang lại công việc (O(n^3)). Tại (n=4\cdot10^4), đây là thứ tự kiểm tra điểm (6.4\cdot10^{13}), trước khi tính đến hệ số không đổi của hình học ba chiều. Việc thử mọi hoán vị hoàn chỉnh thậm chí còn tệ hơn, với các khả năng ((2n)!). 

Quan sát hữu ích là phép quay bảo toàn khoảng cách. Xác định 

[ 
P_4(p)=\sum_q |p-q|^4, 
] 

trong đó tổng chạy trên tất cả (2n) điểm trong một bộ sưu tập. Nếu toàn bộ bộ sưu tập được xoay, tập hợp khoảng cách từ một điểm đến tất cả các điểm khác không thay đổi, do đó (P_4) không thay đổi. Ý tưởng ban đầu của người biên tập là sử dụng bất biến xoay bậc bốn này và sắp xếp các điểm theo nó. 

Đối với vấn đề cụ thể này, chúng ta có thể đơn giản hóa việc tính toán một cách đáng kể. hãy để 

[ 
M=\sum_q qq^T. 
] 

Bởi vì mọi (q) là một vectơ đơn vị và tập hợp chứa cả (q) và (-q), nên ta có 

[ 
\sum_q q=0. 
] 

Đối với một vectơ đơn vị (p), 

[ 
|p-q|^2=2-2p\cdot q. 
] 

Do đó, 

[ 
\bắt đầu{căn chỉnh} 
P_4(p) 
&=\sum_q (2-2p\cdot q)^2\ 
&=4\sum_q\left(1-2p\cdot q+(p\cdot q)^2\right)\ 
&=4\left(2n+p^TMp\right). 
\end{căn chỉnh} 
] 

Hệ số (4) và hằng số (2n) không ảnh hưởng đến thứ tự. Vì vậy chúng ta chỉ cần vô hướng 

[ 
s(p)=p^TMp. 
] 

Ma trận (M) chỉ có sáu mục độc lập, do đó nó được xây dựng trong (O(n)) và mọi chữ ký được đánh giá trong (O(1)). Sau đó chúng tôi sắp xếp các chữ ký (2n), thu được sự tương ứng giữa hai bộ sưu tập. 

Phương pháp brute-force hoạt động vì hai vectơ tương ứng không song song xác định góc quay. Nó thất bại vì chúng ta không biết vectơ nào tương ứng. Bất biến mang lại cho chúng ta sự tương ứng đó mà không cần thử tất cả các cặp, làm giảm vấn đề so khớp hình học thành sắp xếp các giá trị vô hướng (O(n)). 

Vẫn còn một dấu hiệu mơ hồ. Khi hai dòng tương ứng đã được xác định, hãy chọn dấu của vectơ mục tiêu thứ hai sao cho tích số chấm của nó với vectơ mục tiêu đầu tiên phù hợp với tích số chấm tương ứng trong tập hợp đầu tiên. Hai vectơ không song song được định hướng sau đó xác định các khung tọa độ trực giao và phép quay chỉ đơn giản là ma trận ánh xạ khung này sang khung khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n)) | Quá chậm | 
| Bất biến bậc bốn + sắp xếp | (O(n\log n)) | (O(n)) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Đọc điểm (2n) của mỗi tập hợp. Mỗi điểm có độ dài đơn vị lên đến độ chính xác đầu vào nhất định và mọi điểm đều có điểm đối diện trong cùng một bộ sưu tập. 
2. Với mỗi tập hợp, xây dựng ma trận đối xứng 

[ 
M=\sum_i r_i r_i^T. 
] 

Đối với một điểm (r_i=(x_i,y_i,z_i)), đóng góp của nó là 

[ 
\bắt đầu{pmatrix} 
x_i^2 & x_iy_i & x_iz_i\ 
x_iy_i & y_i^2 & y_iz_i\ 
x_iz_i & y_iz_i & z_i^2 
\end{pmatrix}. 
] 

Chỉ có sáu giá trị cần được lưu trữ. 

1. Với mỗi điểm (p), hãy tính chữ số vô hướng của nó 

[ 
s(p)=p^TMp. 
] 

Giá trị này tỷ lệ thuận với bất biến khoảng cách bậc 4 (P_4(p)), do đó các điểm tương ứng có dấu hiệu bằng nhau về mặt số học chính xác. Các điểm đối cực của một đường cũng có dấu hiệu tương tự, đó chính xác là sự mơ hồ mà chúng ta mong đợi. 

1. Sắp xếp chỉ mục của cả hai bộ sưu tập theo chữ ký của chúng. Với các hướng độc lập ngẫu nhiên, các dòng khác nhau gần như chắc chắn có các dấu hiệu khác nhau, do đó các vị trí được sắp xếp sẽ xác định các dòng tương ứng. Nếu một số chữ ký trùng nhau do tính đối xứng thì mọi sự tương ứng tương thích với tính đối xứng đó đều có khả năng hợp lệ. Mẫu này là một trường hợp suy biến nhỏ nên việc triển khai không giả định rằng một vị trí được sắp xếp cụ thể nhất thiết phải là điểm cuối đối diện. 
2. Lấy điểm đầu tiên trong tập hợp thứ nhất được sắp xếp và điểm ở cùng vị trí được sắp xếp trong tập hợp thứ hai. Sau đó quét các vị trí đã sắp xếp còn lại cho đến khi tìm được một cặp vectơ không song song khác. Điều này xử lý cặp đối cực xuất hiện liên tiếp trong trường hợp chung và cũng xử lý mẫu, trong đó một số chữ ký trùng khớp. 
3. Đặt vectơ nguồn được chọn là (a_0,a_1) và vectơ đích tương ứng là (b_0,b_1). Chuẩn hóa (a_0) và (b_0). Đối với mỗi vectơ thứ hai, hãy loại bỏ thành phần của nó dọc theo vectơ đầu tiên: 

[ 
a_1^\perp=a_1-(a_1\cdot a_0)a_0. 
] 

Chuẩn hóa vectơ này và thực hiện tương tự cho (b_1). 

1. Hoàn thành cả hai cặp cho hệ quy chiếu thuận tay phải bằng tích chéo: 

[ 
a_2=a_0\times a_1^\perp,\qquad 
b_2=b_0\times b_1^\perp. 
] 

Nếu vectơ mục tiêu thứ hai có hướng sai, hãy thay thế (b_1) bằng (-b_1) trước khi xây dựng khung. Dấu được chọn bằng cách so sánh hai tích chấm tương ứng. 

1. Lập ma trận xoay 

[ 
R= 
\bắt đầu{bmatrix} 
b_0&b_1^\perp&b_2 
\end{bmatrix} 
\bắt đầu{bmatrix} 
a_0&a_1^\perp&a_2 
\end{bmatrix}^T. 
] 

Bằng cách xây dựng, (Ra_0=b_0) và (Ra_1=\pm b_1), với dấu được chọn nhất quán. Vì hai vectơ nguồn không song song nên điều này quyết định toàn bộ góc quay thích hợp. 

1. Chuyển đổi (R) thành đơn vị quaternion, sau đó thành trục và góc. Lấy phần vô hướng quaternion không âm sẽ cho một góc bằng ([0,\pi]), thỏa mãn khoảng yêu cầu. Đối với phép quay bằng 0, bất kỳ trục nào cũng hợp lệ, do đó việc triển khai sử dụng trục (x). 
2. Đối với mỗi điểm đầu vào (b_i) của bộ sưu tập thứ hai, hãy xoay nó bằng cách sử dụng (R). Dòng tương ứng của nó đã được biết từ vị trí được sắp xếp. Dòng đó có hai điểm cuối ứng cử viên, (a_j) và (-a_j). So sánh điểm xoay với cả hai và chọn điểm cuối gần hơn. Các chỉ số kết quả tạo thành hoán vị cần thiết. 

Tại sao nó hoạt động: ma trận (M) ghi lại tất cả các khoảnh khắc thứ hai của tập hợp điểm và dưới một góc quay (R), nó biến đổi thành (M'=RMR^T). Do đó đối với các điểm tương ứng (p) và (Rp), 

[ 
(Rp)^TM'(Rp)=p^TR^TRMR^TRp=p^TMp. 
] 

Vì vậy, chữ ký vô hướng được bảo tồn. Với các hướng ngẫu nhiên, nó xác định từng đường một cách độc lập ngoại trừ sự mơ hồ đối cực không thể tránh khỏi của nó. Khi hai đường tương ứng không song song được chọn, việc xây dựng khung sẽ tạo ra ánh xạ xoay chính xác cho các đường đó. Vì đầu vào đảm bảo rằng tồn tại một vòng quay chung, nên vòng quay đó sẽ ánh xạ mọi dòng còn lại vào dòng tương ứng của nó. Cuối cùng, việc so sánh hai điểm cuối của mỗi dòng sẽ giải quyết được sự mơ hồ về dấu hiệu duy nhất còn lại. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1] + a[2] * b[2]

def cross(a, b):
    return (
        a[1] * b[2] - a[2] * b[1],
        a[2] * b[0] - a[0] * b[2],
        a[0] * b[1] - a[1] * b[0],
    )

def norm(a):
    return math.sqrt(dot(a, a))

def normalize(a):
    d = norm(a)
    return (a[0] / d, a[1] / d, a[2] / d)

def mat_vec(r, v):
    return (
        r[0][0] * v[0] + r[0][1] * v[1] + r[0][2] * v[2],
        r[1][0] * v[0] + r[1][1] * v[1] + r[1][2] * v[2],
        r[2][0] * v[0] + r[2][1] * v[1] + r[2][2] * v[2],
    )

def dist2(a, b):
    x = a[0] - b[0]
    y = a[1] - b[1]
    z = a[2] - b[2]
    return x * x + y * y + z * z

def build_signatures(points):
    m00 = m01 = m02 = 0.0
    m11 = m12 = 0.0
    m22 = 0.0

    for x, y, z in points:
        m00 += x * x
        m01 += x * y
        m02 += x * z
        m11 += y * y
        m12 += y * z
        m22 += z * z

    sig = [0.0] * len(points)

    for i, (x, y, z) in enumerate(points):
        tx = m00 * x + m01 * y + m02 * z
        ty = m01 * x + m11 * y + m12 * z
        tz = m02 * x + m12 * y + m22 * z
        sig[i] = x * tx + y * ty + z * tz

    order = list(range(len(points)))
    order.sort(key=sig.__getitem__)
    return sig, order

def make_frame(a, b):
    a = normalize(a)
    d = dot(a, b)
    v = (
        b[0] - d * a[0],
        b[1] - d * a[1],
        b[2] - d * a[2],
    )
    v = normalize(v)
    w = cross(a, v)
    return (a, v, w)

def frame_rotation(source, target):
    # R = T * S^T, where S and T contain frame vectors as columns.
    r = [[0.0] * 3 for _ in range(3)]

    for i in range(3):
        for j in range(3):
            r[i][j] = (
                target[0][i] * source[0][j]
                + target[1][i] * source[1][j]
                + target[2][i] * source[2][j]
            )

    return r

def rotation_to_axis_angle(r):
    trace = r[0][0] + r[1][1] + r[2][2]

    if trace > 0.0:
        s = math.sqrt(trace + 1.0) * 2.0
        qw = 0.25 * s
        qx = (r[2][1] - r[1][2]) / s
        qy = (r[0][2] - r[2][0]) / s
        qz = (r[1][0] - r[0][1]) / s
    elif r[0][0] >= r[1][1] and r[0][0] >= r[2][2]:
        s = math.sqrt(max(0.0, 1.0 + r[0][0] - r[1][1] - r[2][2])) * 2.0
        qw = (r[2][1] - r[1][2]) / s
        qx = 0.25 * s
        qy = (r[0][1] + r[1][0]) / s
        qz = (r[0][2] + r[2][0]) / s
    elif r[1][1] >= r[2][2]:
        s = math.sqrt(max(0.0, 1.0 - r[0][0] + r[1][1] - r[2][2])) * 2.0
        qw = (r[0][2] - r[2][0]) / s
        qx = (r[0][1] + r[1][0]) / s
        qy = 0.25 * s
        qz = (r[1][2] + r[2][1]) / s
    else:
        s = math.sqrt(max(0.0, 1.0 - r[0][0] - r[1][1] + r[2][2])) * 2.0
        qw = (r[1][0] - r[0][1]) / s
        qx = (r[0][2] + r[2][0]) / s
        qy = (r[1][2] + r[2][1]) / s
        qz = 0.25 * s

    qn = math.sqrt(qw * qw + qx * qx + qy * qy + qz * qz)
    qw /= qn
    qx /= qn
    qy /= qn
    qz /= qn

    if qw < 0.0:
        qw = -qw
        qx = -qx
        qy = -qy
        qz = -qz

    vnorm = math.sqrt(qx * qx + qy * qy + qz * qz)

    if vnorm < 1e-12:
        return 0.0, (1.0, 0.0, 0.0)

    theta = 2.0 * math.atan2(vnorm, qw)
    axis = (qx / vnorm, qy / vnorm, qz / vnorm)

    if theta > math.pi:
        theta -= 2.0 * math.pi

    return theta, axis

def solve():
    n = int(input())
    total = 2 * n

    a = [tuple(map(float, input().split())) for _ in range(total)]
    b = [tuple(map(float, input().split())) for _ in range(total)]

    sig_a, order_a = build_signatures(a)
    sig_b, order_b = build_signatures(b)

    a0 = order_a[0]
    b0 = order_b[0]

    # Find two nonparallel pairs. In the generic case positions 0 and 1
    # are antipodes, so the loop naturally skips them.
    chosen = None
    for k in range(1, total):
        ia = order_a[k]
        ib = order_b[k]

        ca = cross(a[a0], a[ia])
        cb = cross(b[b0], b[ib])

        if dot(ca, ca) > 1e-14 and dot(cb, cb) > 1e-14:
            chosen = (ia, ib)
            break

    if chosen is None:
        # This is only relevant for extremely degenerate input.
        # n >= 2 guarantees a valid nonparallel pair under the
        # random-direction condition.
        for ia in range(total):
            if ia == a0:
                continue
            ca = cross(a[a0], a[ia])
            if dot(ca, ca) <= 1e-14:
                continue
            for ib in range(total):
                if ib == b0:
                    continue
                cb = cross(b[b0], b[ib])
                if dot(cb, cb) > 1e-14:
                    chosen = (ia, ib)
                    break
            if chosen is not None:
                break

    a1, b1 = chosen

    a0v = normalize(a[a0])
    b0v = normalize(b[b0])
    a1v = normalize(a[a1])
    b1v = normalize(b[b1])

    da = dot(a0v, a1v)
    db = dot(b0v, b1v)

    # The two corresponding unoriented lines have the same angle.
    # Choose the sign giving the matching oriented dot product.
    if abs(da - db) > abs(da + db):
        b1v = (-b1v[0], -b1v[1], -b1v[2])

    source_frame = make_frame(a0v, a1v)
    target_frame = make_frame(b0v, b1v)

    r = frame_rotation(source_frame, target_frame)

    theta, axis = rotation_to_axis_angle(r)

    # Locate the antipode of every point of A exactly as represented
    # in the input. Decimal parsing preserves the sign symmetry.
    lookup = {}
    for i, p in enumerate(a):
        lookup[p] = i

    opposite = [0] * total
    for i, (x, y, z) in enumerate(a):
        opposite[i] = lookup[(-x, -y, -z)]

    position_b = [0] * total
    for pos, idx in enumerate(order_b):
        position_b[idx] = pos

    permutation = [0] * total

    for j in range(total):
        pos = position_b[j]
        candidate = order_a[pos]
        other = opposite[candidate]

        rb = mat_vec(r, b[j])

        if dist2(rb, a[other]) < dist2(rb, a[candidate]):
            permutation[j] = other + 1
        else:
            permutation[j] = candidate + 1

    print("{:.12f}".format(theta))
    print("{:.12f} {:.12f} {:.12f}".format(axis[0], axis[1], axis[2]))
    print(" ".join(map(str, permutation)))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xây dựng ma trận thời điểm thứ hai (3\times3). Sáu mục được lưu trữ là đủ vì ma trận đối xứng. Việc tính toán chữ ký sau đó giảm mọi điểm thành một đánh giá dạng bậc hai. 

Bước sắp xếp là thao tác tốn kém tiệm cận duy nhất. Tính năng sắp xếp tích hợp của Python được triển khai bằng mã gốc được tối ưu hóa, do đó, việc sắp xếp các khóa dấu phẩy động (8\cdot10^4) là thoải mái trong mức độ phức tạp dự định. 

Vòng lặp chọn cặp cố tình kiểm tra các tích chéo thay vì giả định rằng một cặp vị trí cố định được sắp xếp là không song song. Đối với đầu vào chung, hai điểm được sắp xếp đầu tiên là hai điểm cuối của cùng một đường thẳng, do đó chúng không thể xác định khung. Trong mẫu, một số chữ ký trùng nhau, do đó, hai điểm được sắp xếp đầu tiên có thể thuộc các dòng khác nhau. Kiểm tra các sản phẩm chéo xử lý cả hai trường hợp. 

Việc điều chỉnh dấu hiệu sử dụng 

[ 
|d_a-d_b| \quad\text{versus}\quad |d_a+d_b|. 
] 

Điều này tốt hơn là chỉ kiểm tra dấu của tích, vì tích chấm có thể rất gần bằng 0. Dấu hiệu được chọn làm cho hai cặp định hướng có cùng một góc tương hỗ. 

Xoay khung được xây dựng dưới dạng (T S^T). Vì cả hai khung đều trực giao, ma trận này tự động là một phép quay thích hợp cho đến lỗi dấu phẩy động. Việc chuyển đổi quaternion tránh được sự mất ổn định về mặt số học khi trích xuất một trục trực tiếp từ ((R-R^T)/(2\sin\theta)) khi góc gần bằng (0) hoặc (\pi). 

Hoán vị cuối cùng không tin cậy vào dấu được chọn trong quá trình sắp xếp. Mỗi vị trí được sắp xếp xác định một dòng, do đó có chính xác hai điểm cuối ứng cử viên trong bộ sưu tập đầu tiên. Xoay điểm cuối thứ hai và so sánh khoảng cách của nó với cả hai ứng cử viên sẽ giải quyết dấu hiệu một cách độc lập cho mọi điểm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu có hai dòng, có điểm cuối 

[ 
(\cos22.5^\circ,\pm\sin22.5^\circ,0) 
] 

và những mặt đối lập của chúng. Bộ thứ hai là cặp đường thẳng quay trong mặt phẳng. 

Chữ ký bậc bốn không đủ để phân biệt hai dòng trong ví dụ đối xứng đặc biệt này, do đó thứ tự sắp xếp chứa một số giá trị bằng nhau. Thuật toán không cho rằng vị trí (0) và (2) là hai đường thẳng. Nó quét cho đến khi tìm thấy hai cặp không song song. 

| Biến thuật toán | Giá trị hoặc hành vi | 
| --- | --- | 
| (n) | (2) | 
| Số điểm | (4) | 
| Điểm được chọn đầu tiên | Điểm đầu tiên theo thứ tự sắp xếp | 
| Điểm được chọn thứ hai | Điểm đầu sau không song song với nó | 
| Nguồn chấm sản phẩm | Khoảng (0,70710678) | 
| Mục tiêu chấm sản phẩm trước dấu | Khoảng (-0,70710678) | 
| Dấu hiệu mục tiêu | Phủ định | 
| Ma trận xoay | Một phép quay phẳng tương đương với phép quay yêu cầu | 
| Góc đầu ra | Bất kỳ góc hợp lệ tương đương nào trong ([-\pi,\pi]) | 
| Hoán vị | Sự kết hợp hợp lệ của bốn điểm cuối | 

Mẫu chính thức sử dụng góc (-\pi/2), trục ((0,0,1)) và hoán vị (2,3,4,1). Chương trình được phép tạo ra một phép quay hợp lệ khác vì cấu hình hai dòng đối xứng thừa nhận nhiều mô tả về sự tương ứng của cùng một dòng. 

### Đã thi công mẫu 2 

Hãy xem xét ba dòng nguồn được đại diện bởi 

[ 
a=(1,0,0), 
] 

[ 
b=(0,1,0), 
] 

và 

[ 
c=(0,3,0,4,\sqrt{0,75}). 
] 

Bộ sưu tập thứ hai thu được bằng cách xoay mọi thứ theo (90^\circ) quanh trục (z). Các đại diện luân phiên là 

[ 
(0,1,0),\quad (-1,0,0),\quad 
(-0,4,0,3,\sqrt{0,75}). 
] 

Mỗi điểm đều đi kèm với điểm đối lập của nó. 

Đối với tập hợp đầu tiên, sau khi tính tổng cả hai điểm cuối của mỗi dòng, chữ ký dạng bậc hai của ba dòng tỷ lệ với (2.18), (2.32) và (2.50). Các giá trị chính xác là không cần thiết, chỉ cần thứ tự của chúng.

| Biến thuật toán | Trạng thái nguồn | Trạng thái mục tiêu | 
| --- | --- | --- | 
| Chữ ký dòng đầu tiên | (2.18) | (2.18) | 
| Chữ ký dòng thứ hai | (2.32) | (2.32) | 
| Chữ ký dòng thứ ba | (2,50) | (2,50) | 
| Vectơ khung đầu tiên | ((1,0,0)) | ((0,1,0)) | 
| Vectơ khung thứ hai | ((0,1,0)) | ((-1,0,0)) | 
| Vector khung thứ ba | ((0,0,1)) | ((0,0,1)) | 
| Góc quay | (90^\circ) | (90^\circ) | 
| Trục quay | ((0,0,1)) | ((0,0,1)) | 

Phần quan trọng của dấu vết này là chữ ký vô hướng giống nhau thu được trước và sau khi quay. Khi hai đường thẳng không song song được ghép nối, toàn bộ ma trận xoay sẽ diễn ra từ hai khung trực chuẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc xây dựng ma trận và chữ ký lấy các giá trị (O(n)), sắp xếp (2n) lấy (O(n\log n)) và tất cả hình học còn lại là tuyến tính | 
| Không gian | (O(n)) | Hai mảng điểm, chữ ký, chỉ mục được sắp xếp, ánh xạ đối cực và hoán vị đều sử dụng bộ nhớ tuyến tính | 

Đối với (n\le4\cdot10^4), có tối đa (8\cdot10^4) điểm trong mỗi bộ sưu tập. Thuật toán chỉ thực hiện một lượng số học không đổi trên mỗi điểm cộng với hai loại phần tử (8\cdot10^4), phù hợp với giới hạn bốn giây một cách thoải mái hơn nhiều so với bất kỳ phương pháp bậc hai nào. Việc sử dụng bộ nhớ cũng tuyến tính và vẫn nằm trong giới hạn 256 MiB đã nêu. 

## Trường hợp thử nghiệm 

Đầu ra của vấn đề này không phải là duy nhất, do đó, việc xác nhận so sánh chuỗi đầu ra với đầu ra mẫu chính thức là quá nghiêm ngặt. Thay vào đó, bộ khai thác kiểm tra bên dưới sẽ kiểm tra xem hoán vị được tạo ra có phải là hoán vị của tất cả các chỉ số hay không và việc xoay mọi điểm đặt thứ hai theo trục và góc được báo cáo sẽ đặt nó trong phạm vi dung sai của điểm đặt đầu tiên được báo cáo. Nó cũng tự kiểm tra đầu ra mẫu chính thức.```python
import sys
import io
import math
import random

# The following helpers assume that solve() from the solution above
# has been renamed solve_stream(inp) and returns its printed output.
# In a local test file, replace this wrapper with the submitted solution.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def rotate(v, axis, theta):
    x, y, z = v
    ax, ay, az = axis

    c = math.cos(theta)
    s = math.sin(theta)
    d = ax * x + ay * y + az * z

    return (
        x * c + (ay * z - az * y) * s + ax * d * (1.0 - c),
        y * c + (az * x - ax * z) * s + ay * d * (1.0 - c),
        z * c + (ax * y - ay * x) * s + az * d * (1.0 - c),
    )

def valid_output(inp: str, out: str, eps=3e-5) -> bool:
    data = inp.strip().splitlines()
    n = int(data[0])
    m = 2 * n

    first = [tuple(map(float, data[i + 1].split())) for i in range(m)]
    second = [tuple(map(float, data[i + 1 + m].split())) for i in range(m)]

    lines = out.strip().splitlines()
    if len(lines) != 3:
        return False

    theta = float(lines[0])
    axis = tuple(map(float, lines[1].split()))
    perm = list(map(int, lines[2].split()))

    if len(perm) != m:
        return False

    if sorted(perm) != list(range(1, m + 1)):
        return False

    an = math.sqrt(sum(x * x for x in axis))
    if an < 1e-12:
        return False

    axis = tuple(x / an for x in axis)

    for i in range(m):
        rotated = rotate(second[i], axis, theta)
        target = first[perm[i] - 1]

        d2 = sum(
            (rotated[k] - target[k]) ** 2
            for k in range(3)
        )

        if d2 > eps * eps:
            return False

    return True

sample1 = """\
2
0.923879533 0.382683432 0
0.923879533 -0.382683432 0
-0.923879533 -0.382683432 0
-0.923879533 0.382683432 0
0.382683432 0.923879533 0
0.382683432 -0.923879533 0
-0.382683432 -0.923879533 0
-0.382683432 0.923879533
"""

official_sample_output = """\
-1.570796327
0.000000000 0.000000000 1.000000000
2 3 4 1
"""

assert valid_output(sample1, official_sample_output), "official sample"
assert valid_output(sample1, run(sample1)), "sample 1 produced by solution"

def make_case(points, theta, axis, order):
    second = [rotate(p, axis, theta) for p in points]

    shuffled = [second[i] for i in order]

    lines = [str(len(points) // 2)]
    for p in points:
        lines.append("{:.12f} {:.12f} {:.12f}".format(*p))
    for p in shuffled:
        lines.append("{:.12f} {:.12f} {:.12f}".format(*p))

    return "\n".join(lines) + "\n"

# Minimum size, n = 2, and a nontrivial rotation.
r = math.sqrt(0.5)
points_min = [
    (1.0, 0.0, 0.0),
    (-1.0, 0.0, 0.0),
    (0.0, r, r),
    (0.0, -r, -r),
]
case_min = make_case(
    points_min,
    math.pi / 3.0,
    (1.0, 1.0, 1.0),
    [2, 0, 3, 1],
)
assert valid_output(case_min, run(case_min)), "minimum n"

# Identity rotation, with the input already shuffled.
points_identity = [
    (1.0, 0.0, 0.0),
    (-1.0, 0.0, 0.0),
    (0.0, 1.0, 0.0),
    (0.0, -1.0, 0.0),
]
case_identity = make_case(
    points_identity,
    0.0,
    (1.0, 0.0, 0.0),
    [2, 3, 0, 1],
)
assert valid_output(case_identity, run(case_identity)), "zero rotation"

# All invariant values coincide. This is deliberately symmetric.
# The second set has the same order, so the arbitrary tie order is valid.
points_equal = [
    (1.0, 0.0, 0.0),
    (-1.0, 0.0, 0.0),
    (0.0, 1.0, 0.0),
    (0.0, -1.0, 0.0),
    (0.0, 0.0, 1.0),
    (0.0, 0.0, -1.0),
]
case_equal = make_case(
    points_equal,
    math.pi / 2.0,
    (0.0, 0.0, 1.0),
    list(range(6)),
)
assert valid_output(case_equal, run(case_equal)), "equal invariant values"

# Boundary angle close to pi.
s = math.sqrt(3.0) / 2.0
points_pi = [
    (1.0, 0.0, 0.0),
    (-1.0, 0.0, 0.0),
    (0.0, s, 0.5),
    (0.0, -s, -0.5),
    (0.5, 0.5, math.sqrt(0.5)),
    (-0.5, -0.5, -math.sqrt(0.5)),
]
case_pi = make_case(
    points_pi,
    math.pi,
    (0.0, 1.0, 0.0),
    [4, 0, 5, 2, 1, 3],
)
assert valid_output(case_pi, run(case_pi)), "angle pi"

# Maximum-size structural test.
# The test checks the size and permutation structure instead of rotating
# all 80000 points again, which keeps the test harness itself practical.
random.seed(123456)
n = 40000
points_max = []

for _ in range(n):
    x = random.gauss(0.0, 1.0)
    y = random.gauss(0.0, 1.0)
    z = random.gauss(0.0, 1.0)
    q = math.sqrt(x * x + y * y + z * z)
    p = (x / q, y / q, z / q)
    points_max.append(p)
    points_max.append((-p[0], -p[1], -p[2]))

case_max = make_case(
    points_max,
    0.0,
    (1.0, 0.0, 0.0),
    list(range(2 * n)),
)

out_max = run(case_max)
lines_max = out_max.strip().splitlines()
assert len(lines_max) == 3, "maximum size line count"
assert len(lines_max[2].split()) == 2 * n, "maximum size permutation length"
assert sorted(map(int, lines_max[2].split())) == list(range(1, 2 * n + 1)), \
    "maximum size permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức | Bất kỳ đầu ra hợp lệ về mặt hình học nào | Trường hợp đối xứng (n=2) và các giá trị bất biến bằng nhau | 
| Tối thiểu (n=2) | Bất kỳ phép quay và hoán vị hợp lệ nào | Xử lý đầu vào và đối cực nhỏ nhất được phép | 
| Luân chuyển danh tính | Góc (0) với bất kỳ trục và hoán vị hợp lệ nào | Nhánh bậc bốn góc không | 
| Bộ chữ ký bằng nhau đối xứng | Bất kỳ vòng quay hợp lệ nào | Hành vi khi bất biến bậc bốn có quan hệ | 
| Xoay theo (\pi) | Bất kỳ phép quay hợp lệ nào có góc (\pi) hoặc biểu diễn tương đương | Xử lý ranh giới Quaternion | 
| (n=40000) | Một hoán vị hợp lệ của tất cả (80000) chỉ số | Kích thước đầu vào tối đa và hành vi (O(n\log n)) | 

## Vỏ cạnh 

Trường hợp đối cực có tính cơ bản hơn là bệnh lý. Vì```
2
1 0 0
-1 0 0
0 1 0
0 -1 0
1 0 0
-1 0 0
0 1 0
0 -1 0
```hai điểm cuối của mỗi dòng có chữ ký cấp bốn giống hệt nhau. Thuật toán không bao giờ cố gắng phân biệt chúng. Việc sắp xếp xác định một đường và so sánh khoảng cách cuối cùng sẽ quyết định xem điểm xoay có khớp với (p) hay (-p) hay không. Phép xoay danh tính với hoán vị (1,2,3,4) là hợp lệ. 

Trường hợp xoay không được xử lý riêng bên trong chuyển đổi góc trục. Nếu ma trận xoay không thể phân biệt được về mặt số lượng với danh tính thì bậc bốn của nó có phần vectơ gần như bằng không. Góc được báo cáo là 0 và trục được chọn là ((1,0,0)). Trục là tùy ý khi góc bằng 0, vì vậy đây là đầu ra hợp lệ. 

Mẫu minh họa các va chạm bất biến. Một số dòng khác nhau có cùng giá trị (P_4), do đó, việc triển khai giả định một cách mù quáng các vị trí được sắp xếp (0) và (2) đại diện cho các dòng khác nhau có thể chọn hai điểm đối diện nhau và không tạo được khung. Thay vào đó, việc triển khai sẽ kiểm tra các sản phẩm chéo trong khi quét các vị trí được sắp xếp. Trong mẫu, hai điểm đầu tiên không song song nên chúng cung cấp một khung hợp lệ. 

Một phép quay chính xác (\pi) là một ranh giới số khác. Việc tính toán trực tiếp trục bằng phép chia cho (\sin\theta) không ổn định vì (\sin\pi=0). Việc chuyển đổi quaternion tránh sự phân chia đó và trích xuất trục từ phần vectơ của quaternion, do đó phép thử xoay vòng (\pi) thực hiện nhánh ổn định dự kiến. 

Lựa chọn dấu hiệu cuối cùng cũng là một trường hợp có biên. Giả sử bất biến xác định chính xác hai dòng, nhưng bộ sưu tập thứ hai lại liệt kê điểm cuối đối diện. Thay vào đó, ánh xạ xoay (p) tới (q) có thể cần ánh xạ (p) tới (-q). Thuật toán so sánh (d_a=a_0\cdot a_1) với cả (d_b=b_0\cdot b_1) và (-d_b), chọn hướng bảo toàn góc. Các lựa chọn điểm cuối còn lại sau đó được giải quyết một cách độc lập khi xây dựng hoán vị.
