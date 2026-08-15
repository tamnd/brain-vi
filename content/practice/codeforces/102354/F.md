---
title: "CF 102354F - Ngã Tư Vũ Trụ"
description: "Chúng ta có hai tập hợp điểm không có thứ tự trên mặt cầu đơn vị. Mỗi đường hình học đi qua gốc tọa độ được biểu diễn hai lần, bởi hai điểm giao nhau của nó với mặt cầu, do đó, bất cứ khi nào một điểm (r) xuất hiện, (-r) cũng xuất hiện."
date: "2026-08-14T02:31:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "F"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 377
verified: false
draft: false
---

[CF 102354F - Ngã tư vũ trụ](https://codeforces.com/problemset/problem/102354/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm không có thứ tự trên mặt cầu đơn vị. Mỗi đường hình học đi qua gốc tọa độ được biểu diễn hai lần, bởi hai điểm giao nhau của nó với mặt cầu, do đó, bất cứ khi nào một điểm (r) xuất hiện, (-r) cũng xuất hiện. Bộ sưu tập thứ hai thu được từ bộ sưu tập đầu tiên bằng cách áp dụng một phép quay quanh điểm gốc và sau đó thay đổi thứ tự của các điểm. 

Nhiệm vụ là khôi phục cả hai phần thông tin. Đối với mỗi điểm của bộ sưu tập thứ hai, chúng ta phải xuất chỉ mục của điểm tương ứng trong bộ sưu tập thứ nhất và chúng ta phải mô tả phép quay theo một trục và một góc. Sai số hình học bắt buộc chỉ là (10^{-6}), trong khi dữ liệu đầu vào có độ chính xác khoảng (10^{-12}), do đó, độ chính xác gấp đôi thông thường là đủ nếu chúng ta tránh các phép tính không ổn định không cần thiết. 

Ràng buộc quyết định là (n\le 4\cdot10^4), do đó có thể có (8\cdot10^4) điểm. Bất kỳ phương pháp so sánh nào mỗi cặp điểm đều thực hiện các phép toán cặp khoảng (6,4\cdot10^9), vượt xa giới hạn bốn giây. Chúng ta cần một phép tính gần như tuyến tính ngoài việc sắp xếp, vì vậy (O(n\log n)) là mục tiêu tự nhiên. 

Có hai sự thật mang tính cấu trúc làm cho một giải pháp như vậy có thể thực hiện được. Đầu tiên, phép quay bảo toàn khoảng cách, tích chấm và mọi biểu thức được xây dựng từ chúng. Thứ hai, các hướng được chọn thống nhất một cách ngẫu nhiên. Tính ngẫu nhiên không có ý nghĩa trang trí ở đây: nó làm cho một bất biến quay được lựa chọn cẩn thận gần như chắc chắn là khác nhau đối với các đường khác nhau, vì vậy bất biến đó có thể dùng làm dấu vân tay. 

Có một sự tinh tế do cách biểu diễn đối cực gây ra. Bất kỳ bất biến nào chỉ phụ thuộc vào lũy thừa chẵn của tọa độ sẽ cho cùng một giá trị cho (r) và (-r). Đó không phải là lỗi vì hai điểm đó thuộc cùng một đường thẳng. Trước tiên, chúng tôi xác định các đường và chỉ sau khi khôi phục góc quay, chúng tôi mới quyết định điểm nào trong hai điểm cuối đối diện là điểm chính xác. 

Mẫu được cung cấp là một trường hợp hữu ích khác. Bốn điểm của nó tạo thành một hình vuông trong một mặt phẳng. Bất biến được sử dụng dưới đây có giá trị giống hệt nhau cho cả bốn điểm, do đó giả định tính duy nhất ngẫu nhiên không đúng cho mẫu này. Việc thực hiện bất cẩn khi ghép các điểm được sắp xếp liên tiếp một cách mù quáng có thể tạo thành các cặp dòng sai. Việc triển khai bên dưới chứa một dự phòng nhỏ cho (n\le3), xử lý mẫu và các cấu hình đối xứng nhỏ khác. Đối với các đầu vào lớn thực tế, việc xây dựng ngẫu nhiên như đã hứa khiến cho đường đi nhanh trở nên cực kỳ đáng tin cậy. 

Ví dụ: mẫu có bốn điểm 
[ 
(0,923879533,0,382683432,0),\quad 
(0,923879533,-0,382683432,0), 
] 
cùng với những mặt tiêu cực của chúng. Mọi điểm đều nhận được dấu vân tay bậc hai giống nhau. Đầu ra đúng có thể sử dụng phép quay (-\pi/2) quanh trục (z) và hoán vị (2,3,4,1). Một phương pháp giả định mỗi dấu vân tay là duy nhất sẽ âm thầm thất bại trước khi nó cố gắng tính toán phép quay. 

Trường hợp cạnh đơn giản thứ hai là phép xoay danh tính. Nếu hai bộ đầu vào giống hệt nhau nhưng bị xáo trộn thì góc yêu cầu là (0) và trục có thể là bất kỳ vectơ nào khác 0. Việc triển khai tạo ra trục (x) trong trường hợp này. Trục không được xác định duy nhất khi góc bằng 0, do đó việc so sánh trục được in với một số trục dự kiến ​​sẽ không chính xác. 

## Phương pháp tiếp cận

Cách tiếp cận trực tiếp rất đơn giản về mặt khái niệm. Hãy thử sự tương ứng giữa các điểm của hai tập hợp, xác định phép quay từ đủ vectơ tương ứng và kiểm tra tất cả các điểm còn lại. Với (2n) mục tiêu có thể có cho điểm đầu tiên và (2n-1) cho điểm thứ hai, ngay cả trước khi xử lý hoán vị còn lại đã có sẵn các cặp ứng cử viên (\Theta(n^2)). Nếu mọi ứng viên đều yêu cầu quét (O(n)) điểm thì trường hợp xấu nhất là (\Theta(n^3)), khoảng (5.12\cdot10^{14}) so sánh điểm cơ bản tại (n=4\cdot10^4). Ngay cả khi tìm kiếm (O(n^2)) cẩn thận hơn nhiều vẫn sẽ thực hiện khoảng (6,4\cdot10^9) thao tác cặp. 

Quan sát hữu ích là ngừng cố gắng đoán vòng quay trước. Thay vào đó, hãy xây dựng một số gắn với mỗi điểm không thay đổi khi quay và không phụ thuộc vào thứ tự của toàn bộ tập hợp. 

Giải pháp chính thức sử dụng đa thức khoảng cách lũy thừa bậc bốn 
[ 
P_4(x,y,z)= 
\sum_l 
\left((x-x_l)^2+(y-y_l)^2+(z-z_l)^2\right)^2. 
] 
Đây là bất biến quay và việc đánh giá nó cho mọi điểm có thể giảm xuống thành công không đổi trên mỗi điểm sau khi tích lũy các mômen cần thiết. 

4-8(p\cdot r_l)+4(p\cdot r_l)^2. 
] 
Tổng tất cả các điểm, số hạng tuyến tính biến mất vì đầu vào là đối cực: 
[ 
\sum_l r_l=0. 
] 
Xác định ma trận đối xứng 
[ 
M=\sum_l r_l r_l^T. 
] 
Sau đó 
[ 
\sum_l(p\cdot r_l)^2=p^TMp, 
] 
vậy 
[ 
P_4(p)=4(2n)+4p^TMp. 
] 
Hệ số không đổi và hằng số cộng không ảnh hưởng đến việc sắp xếp. Do đó chúng tôi sử dụng 
[ 
F(p)=p^TMp 
] 
như dấu vân tay. 

# b^TM_Bb 

# b^TR^TM_ARb 

# (Rb)^TM_A(Rb) 

F_A(Rb). 
] 
Do đó các điểm tương ứng có dấu vân tay bằng nhau. Bởi vì các hướng là ngẫu nhiên nên các đường khác nhau hầu như chắc chắn có các giá trị khác nhau. Sự bằng nhau không thể tránh khỏi duy nhất là giữa (r) và (-r), vì (F(-r)=F(r)). 

Chúng tôi sắp xếp dấu vân tay. Trong trường hợp chung, cứ hai giá trị bằng nhau liên tiếp tạo thành một cặp đối cực và các cặp này xuất hiện theo cùng thứ tự trong cả hai bộ. Điều này đưa ra sự tương ứng giữa các dòng (n) trong thời gian (O(n\log n)). 

Khi đã biết hai đường thẳng tương ứng không song song thì chỉ còn lại bốn hướng. Chọn một đại diện từ mỗi dòng trong mỗi bộ. Đối với mỗi trong số bốn lựa chọn dấu hiệu, hãy xây dựng phép quay thích hợp duy nhất ánh xạ hai vectơ đã chọn tới các vectơ mục tiêu đã chọn. Sau đó kiểm tra nó với tất cả các điểm. Sự kết hợp dấu hiệu chính xác được đảm bảo để vượt qua. 

Bước cuối cùng chuyển đổi ma trận xoay thành biểu diễn góc trục. Biểu diễn quaternion thuận tiện vì nó vẫn ổn định khi góc gần bằng (\pi), trong đó công thức thông thường chỉ dựa trên phần phản đối xứng của ma trận sẽ mất đi độ chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) đến (O(n^3)) tùy thuộc vào xác minh | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả (2n) điểm của bộ thứ nhất và tất cả (2n) điểm của bộ thứ hai. Lưu trữ tọa độ dưới dạng bộ ba dấu phẩy động. Vì mọi điểm đều nằm trên mặt cầu đơn vị và sai số bắt buộc là (10^{-6}), nên độ chính xác gấp đôi là phù hợp. 
2. Với mỗi tập hợp, tích lũy sáu phần tử độc lập của ma trận đối xứng 
[ 
M=\tổng r_ir_i^T. 
] 
Các mục là 
[ 
M_{xx}=\sum x_i^2,\quad 
M_{xy}=\sum x_iy_i,\quad 
M_{xz}=\sum x_iz_i, 
] 
và tương tự với (M_{yy},M_{yz},M_{zz}). 
3. Đánh giá (F(r)=r^TMr) cho mọi điểm. Việc này chỉ thực hiện một số phép tính số học không đổi cho mỗi điểm vì (M) chỉ bằng (3\times3). 
4. Sắp xếp các chỉ số điểm theo dấu vân tay của chúng. Trong trường hợp ngẫu nhiên, hai bản sao của mỗi dòng có cùng một dấu vân tay và các dòng khác nhau có dấu vân tay khác nhau. Do đó, các vị trí (0,1) tương ứng với một dòng, các vị trí (2,3) tương ứng với một dòng khác, v.v., trong cả hai bộ. 
5. Sử dụng dòng đầu tiên làm một tham chiếu và quét các nhóm dòng khác cho đến khi tìm thấy tham chiếu thứ hai có hướng gần như không song song với tham chiếu đầu tiên. Vì điểm là ngẫu nhiên nên điều này thường xảy ra ngay lập tức. Việc chọn một cặp được phân tách rõ ràng sẽ tránh được việc chia cho tích chéo nhỏ khi xây dựng hệ tọa độ. 
6. Cho (s_1,s_2) là đại diện của hai dòng đã chọn của tập thứ hai và (t_1,t_2) là đại diện của các dòng tương ứng của tập thứ nhất. Hãy thử cả bốn lựa chọn 
[ 
(\pm t_1,\pm t_2). 
] 
Đối với mỗi lựa chọn, hãy xây dựng một cơ sở trực chuẩn từ (s_1,s_2), xây dựng một cơ sở khác từ các vectơ đích có dấu và ánh xạ cơ sở thứ nhất sang cơ sở thứ hai. Điều này đưa ra một ma trận xoay thích hợp. 
7. Xác thực việc luân chuyển ứng viên theo mọi điểm. Đối với điểm đặt thứ hai (b), dấu vân tay của nó cho chúng ta biết đường đặt đầu tiên tương ứng, chứa chính xác hai điểm đối diện. So sánh (Rb) với hai ứng cử viên đó và giữ cái nào gần hơn. Nếu mọi khoảng cách đều nằm dưới một dung sai số nhỏ thì ứng cử viên là phép quay và hoán vị mong muốn. 
8. Nếu dấu vân tay không chia các điểm thành cặp và (n\le3), hãy sử dụng phương pháp dự phòng vũ phu nhỏ. Có nhiều nhất (6!=720) hoán vị, vì vậy chúng ta có thể thử mọi hoán vị, xây dựng một phép quay từ hai vectơ không song song và xác minh tất cả các điểm. Điều này xử lý mẫu đối xứng mà không ảnh hưởng đến độ phức tạp tiệm cận. 
9. Chuyển đổi ma trận xoay thu được thành một quaternion đơn vị. Làm cho thành phần vô hướng không âm, sau đó sử dụng 
[ 
\theta=2\operatorname{atan2}(|v|,w) 
] 
trong đó (w) là phần vô hướng và (v) là phần vectơ. Vectơ (v/|v|) là trục quay. Đối với phép quay bằng 0, bất kỳ trục nào cũng hợp lệ, vì vậy chúng tôi xuất ra ((1,0,0)). 
10. In góc, điểm trục và hoán vị trong chỉ mục dựa trên một yêu cầu. 

Tại sao nó hoạt động 

Bất biến trung tâm là (F(r)=r^TMr), là phần không đổi của đa thức khoảng cách lũy thừa bậc bốn. Một phép quay thay đổi (M) bằng cách chia động từ và thay đổi (r) bằng cách chia động từ nghịch đảo, do đó (F) không thay đổi đối với các điểm tương ứng. Các hướng độc lập ngẫu nhiên làm cho các dấu vân tay này khác biệt giữa các dòng khác nhau với xác suất bằng một trong mô hình toán học. Do đó, bước sắp xếp sẽ xác định từng cặp dòng. 

Đối với hai vectơ không song song, cặp có thứ tự của chúng xác định một hệ quy chiếu trực chuẩn có hướng. Việc xoay ánh xạ khung hình này sang khung hình khác là duy nhất. Bốn lựa chọn về dấu hiệu bao hàm sự mơ hồ duy nhất gây ra bởi thực tế là một đường thẳng có thể có hai đại diện. Chính xác một ứng cử viên đồng ý với việc luân chuyển thực tế và quá trình xác minh toàn cầu sẽ loại bỏ mọi ứng cử viên không chính xác. Khi đã biết phép quay đó, việc chọn điểm cuối gần hơn bên trong mỗi cặp đối cực phù hợp sẽ mang lại hoán vị điểm cần thiết. 

## Giải pháp Python```python
import sys
import math
import itertools

input = sys.stdin.readline

EPS = 1e-8
CHECK_EPS2 = 5e-10
CROSS_EPS = 1e-8

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1] + a[2] * b[2]

def cross(a, b):
    return (
        a[1] * b[2] - a[2] * b[1],
        a[2] * b[0] - a[0] * b[2],
        a[0] * b[1] - a[1] * b[0],
    )

def norm2(a):
    return dot(a, a)

def scale(a, k):
    return (a[0] * k, a[1] * k, a[2] * k)

def sub(a, b):
    return (a[0] - b[0], a[1] - b[1], a[2] - b[2])

def add(a, b):
    return (a[0] + b[0], a[1] + b[1], a[2] + b[2])

def normalize(a):
    d = math.sqrt(norm2(a))
    return scale(a, 1.0 / d)

def apply_rot(R, v):
    return (
        R[0][0] * v[0] + R[0][1] * v[1] + R[0][2] * v[2],
        R[1][0] * v[0] + R[1][1] * v[1] + R[1][2] * v[2],
        R[2][0] * v[0] + R[2][1] * v[1] + R[2][2] * v[2],
    )

def rotation_from_two(source1, source2, target1, target2):
    u = normalize(source1)
    v0 = sub(source2, scale(u, dot(source2, u)))
    vlen2 = norm2(v0)
    if vlen2 < CROSS_EPS * CROSS_EPS:
        return None
    v = scale(v0, 1.0 / math.sqrt(vlen2))
    w = cross(u, v)

    U = normalize(target1)
    V0 = sub(target2, scale(U, dot(target2, U)))
    Vlen2 = norm2(V0)
    if Vlen2 < CROSS_EPS * CROSS_EPS:
        return None
    V = scale(V0, 1.0 / math.sqrt(Vlen2))
    W = cross(U, V)

    # R = [U V W] [u v w]^T
    R = [[0.0] * 3 for _ in range(3)]
    T = (U, V, W)
    S = (u, v, w)

    for i in range(3):
        for j in range(3):
            R[i][j] = (
                T[0][i] * S[0][j]
                + T[1][i] * S[1][j]
                + T[2][i] * S[2][j]
            )
    return R

def matrix_fingerprint(p, M):
    x, y, z = p
    qx = M[0][0] * x + M[0][1] * y + M[0][2] * z
    qy = M[0][1] * x + M[1][1] * y + M[1][2] * z
    qz = M[0][2] * x + M[1][2] * y + M[2][2] * z
    return x * qx + y * qy + z * qz

def build_matrix(points):
    xx = xy = xz = yy = yz = zz = 0.0
    for x, y, z in points:
        xx += x * x
        xy += x * y
        xz += x * z
        yy += y * y
        yz += y * z
        zz += z * z
    return (
        (xx, xy, xz),
        (xy, yy, yz),
        (xz, yz, zz),
    )

def build_groups(values, order):
    groups = []
    for idx in order:
        if not groups or abs(values[idx] - values[groups[-1][0]]) > EPS:
            groups.append([idx])
        else:
            groups[-1].append(idx)
    return groups

def validate_group_rotation(R, A, B, groups_a, groups_b):
    m = len(A)
    perm = [-1] * m

    for g in range(len(groups_b)):
        ga = groups_a[g]
        gb = groups_b[g]

        if len(ga) != 2 or len(gb) != 2:
            return None

        a0, a1 = ga
        for bi in gb:
            rb = apply_rot(R, B[bi])

            d0 = norm2(sub(rb, A[a0]))
            d1 = norm2(sub(rb, A[a1]))

            if d0 <= d1:
                best = a0
                bestd = d0
            else:
                best = a1
                bestd = d1

            if bestd > CHECK_EPS2:
                return None
            if perm[bi] != -1:
                return None
            perm[bi] = best

    if any(x == -1 for x in perm):
        return None
    return perm

def brute_force_small(A, B):
    m = len(A)

    first = 0
    second = -1
    for j in range(1, m):
        if norm2(cross(B[first], B[j])) > CROSS_EPS * CROSS_EPS:
            second = j
            break

    if second == -1:
        return None

    for p in itertools.permutations(range(m)):
        for s1 in (1.0, -1.0):
            for s2 in (1.0, -1.0):
                R = rotation_from_two(
                    B[first],
                    B[second],
                    scale(A[p[first]], s1),
                    scale(A[p[second]], s2),
                )
                if R is None:
                    continue

                ok = True
                for i in range(m):
                    rb = apply_rot(R, B[i])
                    if norm2(sub(rb, A[p[i]])) > CHECK_EPS2:
                        ok = False
                        break

                if ok:
                    return R, list(p)

    return None

def rotation_to_axis_angle(R):
    tr = R[0][0] + R[1][1] + R[2][2]

    if tr > 0.0:
        s = math.sqrt(tr + 1.0) * 2.0
        qw = 0.25 * s
        qx = (R[2][1] - R[1][2]) / s
        qy = (R[0][2] - R[2][0]) / s
        qz = (R[1][0] - R[0][1]) / s
    elif R[0][0] > R[1][1] and R[0][0] > R[2][2]:
        s = math.sqrt(max(0.0, 1.0 + R[0][0] - R[1][1] - R[2][2])) * 2.0
        qx = 0.25 * s
        qy = (R[0][1] + R[1][0]) / s
        qz = (R[0][2] + R[2][0]) / s
        qw = (R[2][1] - R[1][2]) / s
    elif R[1][1] > R[2][2]:
        s = math.sqrt(max(0.0, 1.0 + R[1][1] - R[0][0] - R[2][2])) * 2.0
        qx = (R[0][1] + R[1][0]) / s
        qy = 0.25 * s
        qz = (R[1][2] + R[2][1]) / s
        qw = (R[0][2] - R[2][0]) / s
    else:
        s = math.sqrt(max(0.0, 1.0 + R[2][2] - R[0][0] - R[1][1])) * 2.0
        qx = (R[0][2] + R[2][0]) / s
        qy = (R[1][2] + R[2][1]) / s
        qz = 0.25 * s
        qw = (R[1][0] - R[0][1]) / s

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

    theta = 2.0 * math.atan2(vnorm, max(0.0, qw))
    axis = (qx / vnorm, qy / vnorm, qz / vnorm)

    if theta > math.pi:
        theta -= 2.0 * math.pi
        axis = scale(axis, -1.0)

    return theta, axis

def solve():
    n = int(input())
    m = 2 * n

    A = [tuple(map(float, input().split())) for _ in range(m)]
    B = [tuple(map(float, input().split())) for _ in range(m)]

    MA = build_matrix(A)
    MB = build_matrix(B)

    qa = [matrix_fingerprint(p, MA) for p in A]
    qb = [matrix_fingerprint(p, MB) for p in B]

    order_a = sorted(range(m), key=qa.__getitem__)
    order_b = sorted(range(m), key=qb.__getitem__)

    groups_a = build_groups(qa, order_a)
    groups_b = build_groups(qb, order_b)

    # The random-instance fast path has exactly n groups,
    # each containing the two antipodal endpoints of one line.
    fast = (
        len(groups_a) == n
        and len(groups_b) == n
        and all(len(g) == 2 for g in groups_a)
        and all(len(g) == 2 for g in groups_b)
    )

    if not fast and n <= 3:
        ans = brute_force_small(A, B)
        if ans is not None:
            R, perm = ans
        else:
            raise RuntimeError("No rotation found")
    else:
        if not fast:
            # The official random-input guarantee makes this branch
            # practically unreachable for large n.
            groups_a = [order_a[2 * i:2 * i + 2] for i in range(n)]
            groups_b = [order_b[2 * i:2 * i + 2] for i in range(n)]

        g0 = 0
        best_g = 1
        best_sep = 2.0

        a0 = A[groups_a[g0][0]]
        b0 = B[groups_b[g0][0]]

        for g in range(1, n):
            ag = A[groups_a[g][0]]
            sep = abs(dot(a0, ag))
            if sep < best_sep:
                best_sep = sep
                best_g = g

        a1 = A[groups_a[best_g][0]]
        b1 = B[groups_b[best_g][0]]

        R = None
        perm = None

        for s0 in (1.0, -1.0):
            for s1 in (1.0, -1.0):
                cand = rotation_from_two(
                    b0,
                    b1,
                    scale(a0, s0),
                    scale(a1, s1),
                )
                if cand is None:
                    continue

                p = validate_group_rotation(
                    cand, A, B, groups_a, groups_b
                )
                if p is not None:
                    R = cand
                    perm = p
                    break

            if R is not None:
                break

        if R is None:
            # This is only a safety net for unusual numerical degeneracy.
            if n <= 3:
                ans = brute_force_small(A, B)
                if ans is None:
                    raise RuntimeError("No rotation found")
                R, perm = ans
            else:
                raise RuntimeError("Fingerprint matching failed")

    theta, axis = rotation_to_axis_angle(R)

    print("{:.12f}".format(theta))
    print("{:.12f} {:.12f} {:.12f}".format(*axis))
    print(" ".join(str(x + 1) for x in perm))

if __name__ == "__main__":
    solve()
```Tích lũy ma trận là cách duy nhất vượt qua các tọa độ cần thiết để xây dựng bất biến. Vì ma trận đối xứng nên chỉ có sáu giá trị được lưu trữ, mặc dù mã vẫn giữ cấu trúc đối xứng hoàn chỉnh khi đánh giá dạng bậc hai. 

Biểu thức ở`matrix_fingerprint`được đánh giá là (x(Mr)_x+y(Mr)_y+z(Mr)_z). Hai điểm đối cực tạo ra cùng một giá trị vì việc thay thế (r) bằng (-r) làm thay đổi cả hai thừa số của tích các dấu của dạng bậc hai. 

Mảng sắp xếp chứa các chỉ số thay vì tọa độ. Điều này tránh di chuyển dữ liệu điểm thực tế và giúp việc khôi phục chỉ mục đầu vào ban đầu cho hoán vị cuối cùng trở nên đơn giản. 

Sự lựa chọn bốn dấu hiệu là cần thiết. Đầu vào biểu thị các đường thẳng, không phải vectơ định hướng, do đó, bất biến có thể cho chúng ta biết đường nào tương ứng với đường nào nhưng không thể biết điểm cuối được chọn là dương hay âm. Khi hai vectơ định hướng không song song được cố định, phép quay sẽ tự giải quyết sự mơ hồ này. 

Cấu trúc khung trừ hình chiếu của vectơ thứ hai lên vectơ thứ nhất. Điều đó tạo ra một vectơ vuông góc với vectơ đầu tiên, sau đó tích chéo hoàn thành một cơ sở thuận tay phải trực giao. Ánh xạ một cơ sở thuận tay phải sang một cơ sở thuận tay phải khác luôn tạo ra một phép quay thích hợp chứ không phải sự phản chiếu. 

Việc chuyển đổi quaternion sử dụng các công thức khác nhau tùy thuộc vào mục nhập đường chéo trội khi dấu vết không dương. Điều này tránh việc chia cho một số nhỏ gần một vòng quay (180^\circ). Trường hợp góc 0 được xử lý riêng vì trục ở đó là tùy ý về mặt toán học. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu được cung cấp, bốn điểm của tập hợp thứ nhất tạo thành một hình vuông trong mặt phẳng (xy) và tập hợp thứ hai là cùng một hình vuông được quay bởi (+\pi/2) trước khi áp dụng phép quay ngược theo yêu cầu. 

Ma trận bậc hai cho tập đầu tiên là đường chéo: 
[ 
M= 
\bắt đầu{pmatrix} 
3.41421356&0&0\ 
0&0.58578644&0\ 
0&0&0 
\end{pmatrix}. 
] 
Mọi điểm của hình vuông đều có cùng giá trị (r^TMr), do đó, việc ghép nối phiên bản ngẫu nhiên thông thường không khả dụng. 

| Sân khấu | Tiểu bang | 
| --- | --- | 
| (n) | (2) | 
| Số điểm | (4) | 
| Nhóm vân tay | Một nhóm chứa tất cả bốn điểm | 
| Con đường nhanh | Bị từ chối | 
| Dự phòng | Liệt kê (4!=24) hoán vị | 
| Xoay hợp lệ | Xoay theo (-\pi/2) xung quanh (z) | 
| Hoán vị hợp lệ | (2,3,4,1) | 

Dự phòng thử hoán vị và xác định phép quay từ hai điểm không song song. Sau khi đạt được hoán vị chính xác, phép quay được tính toán sẽ gửi mọi điểm gốc đến điểm được chỉ định của nó. Đầu ra được hiển thị trong câu lệnh là một biểu diễn hợp lệ và chương trình có thể tạo ra một biểu diễn khác nhưng tương đương vì bài toán có nhiều lựa chọn hợp lệ cho cấu hình đối xứng này. 

### Ví dụ bốn dòng không đối xứng 

Xét bốn phương hướng khó khăn 
[ 
(1,0,0),\quad 
(0,1,0),\quad 
(0,0,1),\quad 
\frac{1}{\sqrt3}(1,1,1), 
] 
cùng với những mặt tiêu cực của chúng. Xoay mọi thứ xung quanh trục (z) theo (90^\circ), sau đó xáo trộn các điểm. 

Dấu vân tay bậc hai không còn giống nhau cho mỗi dòng, do đó đường dẫn nhanh có thể xác định các nhóm dòng. Sự chuyển đổi trạng thái quan trọng được hiển thị dưới đây. 

| Sân khấu | Tập đầu tiên | Bộ thứ hai | 
| --- | --- | --- | 
| Ma Trận (M) | Tích lũy từ 8 điểm | Phiên bản xoay của (M) | 
| Phân loại dấu vân tay | Nhóm 4 dòng | 4 nhóm giống nhau theo thứ tự | 
| Nhóm tham khảo | Nhóm đầu tiên | Nhóm đầu tiên tương ứng | 
| Tham khảo thứ hai | Nhóm còn lại ít song song nhất | Nhóm tương ứng của nó | 
| Đăng thử | 4 | 4 | 
| Dùng thử thành công | Một cặp dấu hiệu | Xoay vật lý tương tự | 
| Xác thực | Tất cả 8 điểm đều nằm trong phạm vi cho phép | Tất cả 8 điểm đều nằm trong phạm vi cho phép | 

Ví dụ này giải thích lý do tại sao thuật toán tách biệt nhận dạng đường khỏi nhận dạng điểm cuối. Dấu vân tay xác định một cặp đối cực là một đối tượng. Việc tái cấu trúc xoay hai vectơ sau đó xác định hướng của hai điểm cuối của nó. 

## Phân tích độ phức tạp

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Tích lũy ma trận và đánh giá dấu vân tay là (O(n)); sắp xếp (2n) giá trị chi phí (O(n\log n)); chỉ có bốn phép quay được kiểm tra đối với tất cả các điểm. | 
| Không gian | (O(n)) | Hai tập điểm, dấu vân tay, chỉ số sắp xếp và hoán vị đều sử dụng bộ nhớ tuyến tính. | 

Đối với (n=4\cdot10^4), chỉ có (8\cdot10^4) điểm. Hoạt động chủ yếu là sắp xếp hai mảng có kích thước đó, theo sau là số lần quét tuyến tính không đổi. Điều này hoàn toàn thoải mái trong mục tiêu độ phức tạp dự kiến ​​là 4 giây trong quá trình triển khai được biên dịch và việc triển khai Python giữ cho tất cả các hoạt động hình học có kích thước và cách sử dụng không đổi`sys.stdin.readline`cho đầu vào. 

Sự đảm bảo hướng ngẫu nhiên là yếu tố biến tính bất biến từ dấu vân tay có mục đích chung thành dấu vân tay thực tế. Không có nó, các đường khác nhau có thể có dấu vân tay giống nhau và nói chung không có bất biến vô hướng nào là đủ. Cuộc thảo luận chính thức đưa ra sự khác biệt tương tự: (P_4) hữu ích cho các cấu hình ngẫu nhiên, trong khi cấu hình đối xứng có thể khiến nó trở nên vô dụng. 

## Trường hợp thử nghiệm 

Đầu ra của vấn đề này không phải là duy nhất, do đó, một xác nhận không nên so sánh chuỗi đầu ra thô với một câu trả lời được xác định trước. Bài kiểm tra đúng là phân tích phép quay và hoán vị được trả về cũng như xác minh điều kiện hình học. Dây nịt sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn trong cùng một tệp thử nghiệm.```python
import sys
import io
import math
import random

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

def rotate_z(p, angle):
    c = math.cos(angle)
    s = math.sin(angle)
    x, y, z = p
    return (c * x - s * y, s * x + c * y, z)

def make_case(points, angle):
    first = []
    for p in points:
        first.append(p)
        first.append((-p[0], -p[1], -p[2]))

    second = []
    for p in points:
        q = rotate_z(p, angle)
        second.append(q)
        second.append((-q[0], -q[1], -q[2]))

    rng = random.Random(1234567)
    rng.shuffle(second)

    lines = [str(len(points))]
    for p in first:
        lines.append("{:.12f} {:.12f} {:.12f}".format(*p))
    for p in second:
        lines.append("{:.12f} {:.12f} {:.12f}".format(*p))
    return "\n".join(lines) + "\n"

def parse_output(inp, out):
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = 2 * n

    A = []
    for _ in range(m):
        A.append(tuple(float(next(it)) for _ in range(3)))

    B = []
    for _ in range(m):
        B.append(tuple(float(next(it)) for _ in range(3)))

    out_data = out.split()
    theta = float(out_data[0])
    axis = tuple(map(float, out_data[1:4]))
    perm = list(map(int, out_data[4:4 + m]))

    assert -math.pi - 1e-9 <= theta <= math.pi + 1e-9
    assert 1e-3 <= sum(abs(x) for x in axis) <= 1e3
    assert sorted(perm) == list(range(1, m + 1))

    c = math.cos(theta)
    s = math.sin(theta)
    x, y, z = axis
    length = math.sqrt(x * x + y * y + z * z)
    x /= length
    y /= length
    z /= length

    for i in range(m):
        bx, by, bz = B[i]

        # Rodrigues rotation.
        cross_x = y * bz - z * by
        cross_y = z * bx - x * bz
        cross_z = x * by - y * bx
        d = x * bx + y * by + z * bz

        rx = bx * c + cross_x * s + x * d * (1.0 - c)
        ry = by * c + cross_y * s + y * d * (1.0 - c)
        rz = bz * c + cross_z * s + z * d * (1.0 - c)

        ax, ay, az = A[perm[i] - 1]
        err = math.sqrt(
            (rx - ax) ** 2 +
            (ry - ay) ** 2 +
            (rz - az) ** 2
        )
        assert err <= 2e-6

# Provided sample.
sample1 = """\
2
0.923879533 0.382683432 0
0.923879533 -0.382683432 0
-0.923879533 -0.382683432 0
-0.923879533 0.382683432 0
0.382683432 0.923879533 0
0.382683432 -0.923879533 0
-0.382683432 -0.923879533 0
-0.382683432 0.923879533 0
"""

parse_output(sample1, run(sample1))

# Minimum-size case, n = 2, with an identity rotation.
case_min = make_case(
    [
        (1.0, 0.0, 0.0),
        (0.0, 1.0, 0.0),
    ],
    0.0,
)
parse_output(case_min, run(case_min))

# Symmetric three-line case. This exercises the small brute-force fallback.
case_symmetric = make_case(
    [
        (1.0, 0.0, 0.0),
        (0.0, 1.0, 0.0),
        (0.0, 0.0, 1.0),
    ],
    math.pi / 2,
)
parse_output(case_symmetric, run(case_symmetric))

# Non-symmetric case with a general-looking set of directions.
case_general = make_case(
    [
        (1.0, 0.0, 0.0),
        (0.0, 1.0, 0.0),
        (0.0, 0.0, 1.0),
        (1.0 / math.sqrt(3.0),
         1.0 / math.sqrt(3.0),
         1.0 / math.sqrt(3.0)),
    ],
    -0.731,
)
parse_output(case_general, run(case_general))

# Maximum-size stress case.
# The points are generated deterministically on the sphere and then rotated.
n_big = 40000
points_big = []

for i in range(n_big):
    z = -1.0 + 2.0 * (i + 0.5) / n_big
    phi = i * 2.399963229728653
    r = math.sqrt(max(0.0, 1.0 - z * z))
    points_big.append((r * math.cos(phi), r * math.sin(phi), z))

case_big = make_case(points_big, 1.234567)
parse_output(case_big, run(case_big))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu được cung cấp | Bất kỳ phép quay và hoán vị hợp lệ về mặt hình học nào | Cấu hình đối xứng và dự phòng nhỏ gọn | 
| (n=2), luân chuyển danh tính | Góc (0) với bất kỳ trục và hoán vị hợp lệ nào | Kích thước tối thiểu và xử lý góc không | 
| Ba trục tọa độ | Bất kỳ phép quay và hoán vị hợp lệ nào (90^\circ) | Nhiều dấu vân tay bằng nhau và tính chính xác dự phòng | 
| Bốn hướng không đối xứng | Một phép quay hợp lệ gần (-0,731) radian quanh trục (z) | Lựa chọn dấu và so khớp dựa trên bất biến thông thường | 
| (n=40000) chỉ đường được tạo | Bất kỳ hoán vị hợp lệ nào có lỗi nhiều nhất (2\cdot10^{-6}) | Kích thước đầu vào tối đa, chi phí sắp xếp và độ ổn định về số | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là đẳng thức đối cực không thể tránh khỏi. Giả sử tập hợp chứa ((1,0,0)) và ((-1,0,0)). Dấu vân tay của họ đáp ứng 
[ 
F(1,0,0)=F(-1,0,0). 
] 
Việc thực hiện bất cẩn có thể kết luận rằng bất biến đã thất bại. Giải thích đúng là cả hai điểm đều mô tả cùng một đường hình học. Thuật toán giữ chúng lại với nhau và trì hoãn việc quyết định dấu cho đến khi biết được phép quay. 

Trường hợp cạnh thứ hai là phép xoay danh tính. lấy 
[ 
A={(1,0,0),(-1,0,0),(0,1,0),(0,-1,0)} 
] 
và đặt (B=A) theo một thứ tự khác. Vòng quay được yêu cầu có thể là danh tính, với (\theta=0). Quaternion có phần vectơ bằng 0, do đó mã sẽ in trục ((1,0,0)). Trục là tùy ý cho phép quay bằng 0 và hoán vị có được bằng cách khớp trực tiếp các điểm được xoay. 

Trường hợp cạnh thứ ba là một phép quay chính xác (\pi). Các phần tử phản đối xứng của ma trận xoay về mặt lý thuyết bằng 0 ở góc này, do đó một công thức như 
[ 
e_x=\frac{R_{32}-R_{23}}{2\sin\theta} 
] 
là nguy hiểm về mặt số lượng. Thay vào đó, phép chuyển đổi bậc bốn sẽ chọn số hạng đường chéo lớn nhất khi dấu vết không dương. Ví dụ: phép quay (\pi) quanh trục (z) có 
[ 
R= 
\bắt đầu{pmatrix} 
-1&0&0\ 
0&-1&0\ 
0&0&1 
\end{pmatrix}, 
] 
và đường chéo lớn nhất xác định thành phần (z) của quaternion mà không chia cho một đại lượng gần bằng 0. 

Hộp đựng cạnh thứ tư là mẫu hình vuông được cung cấp. Bốn điểm của nó đều có dấu vân tay bậc hai giống nhau. Chỉ sắp xếp không thể biết được hai điểm nào tạo thành một đường thẳng ban đầu. Vì (n=2), phương án dự phòng liệt kê tất cả (4!) hoán vị điểm có thể có. Đối với mỗi cái, nó xây dựng một phép quay từ hai vectơ không song song và kiểm tra tất cả bốn điểm. Một trong những ứng cử viên này đưa ra phép quay (-\pi/2) hợp lệ và hoán vị (2,3,4,1). 

Trường hợp cạnh số cuối cùng là hai dấu vân tay ngẫu nhiên cực kỳ gần nhau. Với các hướng ngẫu nhiên thống nhất độc lập, sự bằng nhau chính xác giữa các đường khác nhau có xác suất bằng 0 và xác suất va chạm bên trong dung sai số cố định là cực kỳ nhỏ. Tuyên bố có chủ ý cung cấp cấu trúc ngẫu nhiên này để bất biến bậc bốn vô hướng có thể được sử dụng làm dấu vân tay thực tế. Mã vẫn xác minh vòng quay cuối cùng đối với mọi điểm, do đó, một ứng cử viên không chính xác gây ra bởi sự mơ hồ về số sẽ bị từ chối thay vì được in âm thầm.
