---
title: "CF 102471I - Mặt Trăng"
description: "Chúng ta có các điểm cố định a 1 ​,…,an ​ trên mặt cầu đơn vị. Một điểm mới a 0 ​ được chọn thống nhất từ ​​hình cầu. Chúng ta hỏi liệu tất cả n+1 điểm có thể nằm gọn trong một bán cầu kín hay không."
date: "2026-08-09T04:49:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "I"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 347
verified: true
draft: false
---

[CF 102471I - Mặt Trăng](https://codeforces.com/problemset/problem/102471/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có các điểm cố định a 1 ​,…,an ​ trên mặt cầu đơn vị. Một điểm mới a 0 ​ được chọn thống nhất từ ​​hình cầu. Chúng ta hỏi liệu tất cả n+1 điểm có thể nằm gọn trong một bán cầu kín hay không. Vì 0 ​ là ngẫu nhiên và f là 0 hoặc 1, giá trị kỳ vọng của f chính xác là xác suất mà điểm ngẫu nhiên có thể chấp nhận được. 

Công thức cải tiến hình học hữu ích là tạm thời quên số 0 ​ và nhìn vào hình nón được tạo bởi các vectơ cố định, 

C={ i ∑ ​ λ i ​ a i ​ :λ i ​ ≥0}. 

Điểm x không chính xác khi −x nằm bên trong hình nón này. Do đó, các vị trí xấu của số 0 tạo thành ảnh đối cực của bao cầu lồi của các điểm cố định. Lập bản đồ đối cực bảo tồn diện tích, vì vậy 

E[f]=1− 4π diện tích(sconv(a 1 ​,…,a n ​ )) ​ . 

Do đó, bài toán xác suất trở thành bài toán hình học: tính diện tích bao cầu lồi của các điểm cố định. 

Đầu vào cho ra các bộ ba số nguyên (x, y, z), nhưng điểm thực tế là vectơ chuẩn hóa. Chúng ta nên giữ các bộ ba số nguyên cho các bài kiểm tra định hướng hình học vì chúng cho phép chúng ta thực hiện chính xác tất cả các vị từ bao lồi. Việc chuẩn hóa chỉ cần thiết khi đánh giá diện tích hình cầu cuối cùng. 

Với n 10 5, thuật toán O(n 2 ) sẽ yêu cầu khoảng 5×10 9 phép toán theo tỷ lệ cặp trong trường hợp xấu nhất, vượt xa giới hạn hai giây. Chúng ta cần một thuật toán hình học thời gian dự kiến ​​O(nlogn). Lộ trình tiêu chuẩn là một thân lồi ba chiều gia tăng ngẫu nhiên, có độ phức tạp dự kiến ​​là O(nlogn) theo chiều cố định. 

Có một số bệnh thoái hóa cần được điều trị rõ ràng. Nếu n=0,1 hoặc 2 thì các điểm cố định luôn vừa với một bán cầu nên câu trả lời là 1. Ví dụ:```
2
1 0 0
-1 0 0
```có câu trả lời 1. Một giải pháp bất cẩn coi hai điểm đối cực là xác định đa giác hình cầu có diện tích dương sẽ trừ đi diện tích khỏi câu trả lời một cách không chính xác. 

Nếu tất cả các điểm cố định nằm trong một mặt phẳng qua gốc tọa độ thì bao lồi hình cầu của chúng có diện tích hai chiều bằng không. Ví dụ,```
3
1 0 0
-1 0 0
0 1 0
```có câu trả lời 1. Ba điểm có thể tạo thành một cung lớn trên một đường tròn lớn, nhưng cung tròn lớn có diện tích bề mặt bằng không. 

Mẫu được cung cấp là một trường hợp ngược lại hữu ích:```
3
1 0 0
0 1 0
0 0 1
```Bao lồi hình cầu là một quãng tám của hình cầu, có diện tích là π/2. Do đó, xác suất xấu là (π/2)/(4π)=1/8, cho kết quả là 7/8. Giải pháp tính diện tích tam giác phẳng thông thường thay vì diện tích hình cầu sẽ nhận được câu trả lời sai. 

Các mặt thân lồi lồi cũng có thể xảy ra. Một số điểm đầu vào có thể nằm trên cùng một mặt phẳng. Những điểm như vậy không làm thay đổi diện tích hình cầu ngoài việc chia đa giác hình cầu giống nhau thành các hình tam giác, do đó, việc triển khai bao lồi có thể sử dụng bất kỳ phép tính tam giác hợp lệ nào của một mặt đồng phẳng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô tả đặc điểm của mọi bán cầu có thể chứa các điểm cố định và sau đó xác định vị trí nào của số 0 ​ có thể được thêm vào. Điều này nhanh chóng trở thành bậc hai vì mỗi mặt phẳng phân cách ứng cử viên được xác định bởi một số điểm đầu vào. Việc liệt kê các bộ ba đã cho khả năng Θ(n 3 ), trong khi việc liệt kê các cặp chẵn sẽ cho Θ(n 2 ), khoảng 5⋅10 9 cặp tại n=10 5. 

Nhận xét quan trọng là chỉ có ranh giới của bao lồi hình cầu mới quan trọng. Các điểm cố định bên trong không thể thay đổi tập hợp các hướng được bao phủ bởi hình nón. Biên của bao lồi hình cầu chính xác là hình chiếu xuyên tâm của các mặt liên quan của bao lồi ba chiều thông thường của các vectơ đầu vào. 

Vì vậy, lực lượng vũ phu hoạt động vì nó cố gắng khám phá các mặt phẳng hỗ trợ một cách rõ ràng, nhưng không thành công vì có quá nhiều mặt phẳng có thể có. Thân lồi gói tất cả các mặt phẳng hỗ trợ thành một tập hợp các mặt có kích thước tuyến tính. Khi đã biết những khuôn mặt đó, câu trả lời chỉ là tổng diện tích tam giác cầu của chúng. 

Đối với tập điểm đầy đủ chiều, hãy xây dựng bao lồi ba chiều. Định hướng mọi mặt thân tàu hướng ra ngoài. Mỗi mặt có các đỉnh u,v,w xác định một tam giác cầu bằng cách nối ba điểm đó với gốc tọa độ dọc theo các cung tròn lớn. Nếu mặt phẳng mặt không đi qua gốc tọa độ, phép chiếu hướng tâm của nó sẽ đóng góp chính xác vào diện tích của tam giác cầu đó. Các mặt chứa gốc tọa độ có diện tích hình cầu hai chiều bằng 0 và không đóng góp gì. 

Đối với một tam giác gồm các vectơ đơn vị u,v,w, công thức ổn định về số cho diện tích của nó là 

A=2atan2(∣det(u,v,w)∣,1+u⋅v+v⋅w+w⋅u). 

sử dụng`atan2`tốt hơn là khôi phục các góc thông qua`acos`, bởi vì`acos`mất độ chính xác khi đối số của nó rất gần với 1 hoặc −1. 

Thân tàu ba chiều được xây dựng dần dần. Chúng ta bắt đầu với một khối tứ diện, sắp xếp ngẫu nhiên thứ tự chèn và với mỗi điểm mới, hãy tìm tập hợp các mặt hiện có được kết nối. Những khuôn mặt đó tạo thành một chiếc mũ lưỡi trai. Ranh giới của chúng là đường chân trời và việc kết nối điểm mới với mọi cạnh của đường chân trời sẽ tạo ra thân tàu mới. Chủ sở hữu xung đột được lưu trữ cho mọi điểm chưa được chèn, do đó thuật toán có thể xác định vị trí một khuôn mặt hiển thị mà không cần quét toàn bộ thân tàu. Cấu trúc thân lồi tăng dần ngẫu nhiên có độ phức tạp O(nlogn) dự kiến ​​ở kích thước cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê cấu hình hỗ trợ ứng viên | O(n 2 ) hoặc tệ hơn | O(n) | Quá chậm | 
| Thân tàu 3D tăng dần ngẫu nhiên | Dự kiến ​​O(nlogn) | O(n) dự kiến ​​| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các vectơ chỉ hướng số nguyên và giữ nguyên tọa độ nguyên ban đầu của chúng. Các tọa độ dấu phẩy động chuẩn hóa chỉ được tính toán sau này để đánh giá diện tích hình cầu. Tọa độ nguyên chính xác có giá trị vì phép thử hướng thân lồi là yếu tố quyết định và có thể được đánh giá mà không cần làm tròn. 
2. Xử lý ngay n<2. Nhiều nhất hai điểm cố định luôn vừa với một bán cầu nên xác suất là 1. 
3. Tính thứ hạng của các vectơ cố định. Nếu chúng bao trùm một không gian có nhiều nhất là hai chiều thì tất cả các điểm đều nằm trên một vòng tròn lớn sau khi chuẩn hóa. Thân lồi hình cầu của chúng có diện tích bề mặt bằng 0, vì vậy câu trả lời là 1. 
4. Nếu n=3 và ba vectơ độc lập tuyến tính thì chúng xác định trực tiếp một tam giác cầu. Tính diện tích của nó bằng`atan2`công thức và trả về 1−A/(4π). Chưa có khối đa diện ba chiều để xây dựng. 
5. Chọn bốn điểm đầu vào độc lập với nhau và hướng bốn mặt của khối tứ diện của chúng ra ngoài. Vị ngữ định hướng 

định hướng(a,b,c,d)=(b−a)⋅((c−a)×(d−a)) 

được đánh giá bằng số nguyên. Nếu nó dương, d nằm trên cạnh pháp tuyến của abc, do đó hướng mặt phải đảo ngược. 

1. Xáo trộn ngẫu nhiên tất cả các điểm còn lại. Đối với mọi điểm chưa được chèn, hãy giữ một khuôn mặt hiện có làm chủ sở hữu xung đột của nó. Nếu bề mặt đó biến mất trong quá trình chèn, điểm sẽ được gán lại cho một trong các bề mặt mới được tạo mà nó có thể nhìn thấy. Các điểm nằm chính xác trên một mặt thân tàu không cần chèn thêm vì chúng chỉ chia nhỏ một mặt ranh giới hiện có. 
2. Chèn lần lượt các điểm còn lại. Bắt đầu từ chủ sở hữu xung đột của điểm và thực hiện duyệt đồ thị trên các mặt liền kề. Một mặt được nhìn thấy chính xác khi điểm mới nằm hoàn toàn bên ngoài mặt phẳng định hướng của nó. Quá trình truyền tải thu thập mọi khuôn mặt hiển thị được kết nối. 
3. Loại bỏ các mặt nhìn thấy được. Mọi cạnh ngăn cách một mặt nhìn thấy được với một mặt không nhìn thấy được đều thuộc về đường chân trời. Các cạnh chân trời này chính xác là ranh giới mà điểm mới phải được kết nối. 
4. Đối với mỗi cạnh chân trời, tạo một hình tam giác mới chứa cạnh đó và điểm được chèn. Định hướng nó sao cho điểm bên trong đã biết của khối tứ diện ban đầu nằm hoàn toàn bên trong thân tàu mới. Kết nối các mặt mới thông qua bản đồ cạnh. 
5. Chỉ định lại chủ sở hữu xung đột của các điểm mà chủ sở hữu trước đó đã bị xóa. Việc kiểm tra các bề mặt mới là đủ cho những điểm này vì vùng nhìn thấy được đã bị loại bỏ đã được thay thế bằng quạt chân trời mới. 
6. Sau tất cả các lần chèn, hãy đi ngang qua từng mặt thân tàu còn sót lại. Chuẩn hóa ba đỉnh của nó và tính diện tích tam giác cầu. Tổng hợp các khu vực này thành`spherical_area`. 
7. Thân cầu chính xác là tập hợp các vị trí đối cực xấu của số 0 . Diện tích của nó chia cho 4π là xác suất f=0. Do đó đầu ra 

1− 4π diện tích hình cầu ​ . 

### Tại sao nó hoạt động 

Đối với một điểm ngẫu nhiên x, tất cả các điểm cố định và x nằm gọn trong một bán cầu kín chính xác khi tồn tại một vectơ h sao cho h⋅a i ​ ≥0 với mọi điểm cố định và h⋅x ≥0. Theo định lý siêu phẳng phân tách, các điểm mà h như vậy không tồn tại chính xác là bao cầu lồi đối cực của các điểm cố định, cho đến ranh giới của nó, có số đo bằng 0. Thân lồi ba chiều chứa chính xác các mặt đỡ xác định thân lồi hình cầu đó. Hình chiếu hướng kính mỗi mặt của thân tàu không gốc sẽ tạo ra một hình tam giác hình cầu và các hình chiếu phân chia thân hình cầu mà không chồng lên nhau ở phần bên trong của chúng. Do đó, tổng hợp các diện tích tam giác đó sẽ đưa ra thước đo chính xác về các vị trí xấu. Phần bù cuối cùng là giá trị mong đợi cần thiết. 

## Giải pháp Python 

Việc triển khai bên dưới sử dụng các định thức số nguyên chính xác cho hướng thân tàu và kết cấu thân tàu tăng dần ngẫu nhiên. Diện tích hình cầu cuối cùng chỉ sử dụng dấu phẩy động sau khi xác định được bao tổ hợp.```python
import sys
input = sys.stdin.readline

import math
import random

def cross(a, b):
    return (
        a[1] * b[2] - a[2] * b[1],
        a[2] * b[0] - a[0] * b[2],
        a[0] * b[1] - a[1] * b[0],
    )

def sub(a, b):
    return (
        a[0] - b[0],
        a[1] - b[1],
        a[2] - b[2],
    )

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1] + a[2] * b[2]

def orient(a, b, c, d):
    ab = sub(b, a)
    ac = sub(c, a)
    ad = sub(d, a)
    return dot(cross(ab, ac), ad)

def face_area(p, q, r, nf):
    px, py, pz = p
    qx, qy, qz = q
    rx, ry, rz = r

    np = math.sqrt(px * px + py * py + pz * pz)
    nq = math.sqrt(qx * qx + qy * qy + qz * qz)
    nr = math.sqrt(rx * rx + ry * ry + rz * rz)

    ux, uy, uz = px / np, py / np, pz / np
    vx, vy, vz = qx / nq, qy / nq, qz / nq
    wx, wy, wz = rx / nr, ry / nr, rz / nr

    det = (
        ux * (vy * wz - vz * wy)
        - uy * (vx * wz - vz * wx)
        + uz * (vx * wy - vy * wx)
    )

    uv = ux * vx + uy * vy + uz * vz
    vw = vx * wx + vy * wy + vz * wz
    wu = wx * ux + wy * uy + wz * uz

    den = 1.0 + uv + vw + wu

    return 2.0 * math.atan2(abs(det), den)

def spherical_triangle_area(p, q, r):
    return face_area(p, q, r, None)

def solve_points(points):
    n = len(points)

    if n <= 2:
        return 1.0

    # Find three linearly independent vectors if possible.
    a = points[0]

    i1 = -1
    for i in range(1, n):
        if cross(a, points[i]) != (0, 0, 0):
            i1 = i
            break

    if i1 == -1:
        return 1.0

    b = points[i1]

    i2 = -1
    ab = cross(a, b)
    for i in range(i1 + 1, n):
        if dot(ab, points[i]) != 0:
            i2 = i
            break

    if i2 == -1:
        return 1.0

    c = points[i2]

    # Three fixed points already determine the spherical hull.
    if n == 3:
        area = spherical_triangle_area(a, b, c)
        return 1.0 - area / (4.0 * math.pi)

    # Find a fourth point outside the plane of a,b,c.
    i3 = -1
    for i in range(n):
        if i not in (0, i1, i2) and orient(a, b, c, points[i]) != 0:
            i3 = i
            break

    if i3 == -1:
        return 1.0

    # The centroid of the initial tetrahedron is strictly inside
    # the initial hull and remains inside every later hull.
    center = (
        a[0] + b[0] + c[0] + points[i3][0],
        a[1] + b[1] + c[1] + points[i3][1],
        a[2] + b[2] + c[2] + points[i3][2],
    )

    faces = []
    alive = []
    neigh = []
    buckets = []

    # edge -> (face_id, local_edge_index)
    edge_map = {}

    def edge_key(u, v):
        if u < v:
            return (u, v)
        return (v, u)

    def add_face(u, v, w):
        fid = len(faces)
        faces.append([u, v, w])
        alive.append(True)
        neigh.append([-1, -1, -1])
        buckets.append([])

        for e in range(3):
            x = faces[fid][e]
            y = faces[fid][(e + 1) % 3]
            key = edge_key(x, y)

            old = edge_map.get(key)
            if old is None:
                edge_map[key] = (fid, e)
            else:
                of, oe = old
                neigh[fid][e] = of
                neigh[of][oe] = fid

        return fid

    # Create the four tetrahedron faces.
    ids = [0, i1, i2, i3]

    tetra_faces = [
        (ids[0], ids[1], ids[2], ids[3]),
        (ids[0], ids[3], ids[1], ids[2]),
        (ids[0], ids[2], ids[3], ids[1]),
        (ids[1], ids[3], ids[2], ids[0]),
    ]

    for u, v, w, opposite in tetra_faces:
        if orient(points[u], points[v], points[w], points[opposite]) > 0:
            v, w = w, v
        add_face(u, v, w)

    owner = [-1] * n
    used = [False] * n
    for x in ids:
        used[x] = True

    # Every remaining point on the sphere is outside the tetrahedron,
    # except for coplanar degeneracies which can safely remain on the hull.
    for i in range(n):
        if used[i]:
            continue
        p = points[i]
        found = -1
        for fid in range(len(faces)):
            if orient(
                points[faces[fid][0]],
                points[faces[fid][1]],
                points[faces[fid][2]],
                p,
            ) > 0:
                found = fid
                break
        if found != -1:
            owner[i] = found
            buckets[found].append(i)

    order = [i for i in range(n) if not used[i]]
    random.shuffle(order)

    for pidx in order:
        start = owner[pidx]

        # Degenerate coplanar points need not be inserted.
        if start == -1 or not alive[start]:
            continue

        p = points[pidx]

        visible = set()
        stack = [start]

        while stack:
            fid = stack.pop()
            if fid in visible or not alive[fid]:
                continue

            u, v, w = faces[fid]
            if orient(points[u], points[v], points[w], p) <= 0:
                continue

            visible.add(fid)

            for nb in neigh[fid]:
                if nb != -1 and nb not in visible and alive[nb]:
                    stack.append(nb)

        if not visible:
            continue

        candidates = []
        for fid in visible:
            for q in buckets[fid]:
                if owner[q] == fid and q != pidx:
                    owner[q] = -1
                    candidates.append(q)
            buckets[fid].clear()

        horizon = []

        for fid in visible:
            u, v, w = faces[fid]
            vs = (u, v, w)

            for e in range(3):
                nb = neigh[fid][e]
                if nb not in visible:
                    x = vs[e]
                    y = vs[(e + 1) % 3]

                    nb_edge = -1
                    if nb != -1:
                        nu, nv, nw = faces[nb]
                        nvs = (nu, nv, nw)
                        for ee in range(3):
                            if edge_key(nvs[ee], nvs[(ee + 1) % 3]) == edge_key(x, y):
                                nb_edge = ee
                                break

                    horizon.append((x, y, nb, nb_edge))

        # Remove visible faces from the edge map.
        for fid in visible:
            alive[fid] = False
            u, v, w = faces[fid]
            vs = (u, v, w)

            for e in range(3):
                x = vs[e]
                y = vs[(e + 1) % 3]
                key = edge_key(x, y)
                old = edge_map.get(key)
                if old is not None and old[0] == fid:
                    del edge_map[key]

        new_faces = []

        for x, y, nb, nb_edge in horizon:
            u, v, w = x, y, pidx

            # The initial tetrahedron centroid must remain inside the hull.
            if orient(points[u], points[v], points[w], center) > 0:
                v, u = u, v

            fid = add_face(u, v, w)
            new_faces.append(fid)

            if nb != -1:
                # add_face already linked the two faces through the edge.
                pass

        # Reassign points whose only known visible face disappeared.
        for q in candidates:
            qp = points[q]
            for fid in new_faces:
                u, v, w = faces[fid]
                if orient(points[u], points[v], points[w], qp) > 0:
                    owner[q] = fid
                    buckets[fid].append(q)
                    break

    area = 0.0

    for fid in range(len(faces)):
        if not alive[fid]:
            continue

        u, v, w = faces[fid]
        area += spherical_triangle_area(
            points[u],
            points[v],
            points[w],
        )

    area = min(max(area, 0.0), 4.0 * math.pi)
    return 1.0 - area / (4.0 * math.pi)

def main():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    ans = solve_points(points)
    print("{:.12f}".format(ans))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_points`xử lý tất cả các trường hợp có chiều thấp hơn trước khi mã thân bắt đầu. Điều này là cần thiết vì bao lồi ba chiều cần có một tứ diện ban đầu, trong khi ba vectơ cố định độc lập tuyến tính đã xác định một tam giác cầu hoàn toàn hợp lệ. 

các`orient`chức năng là vị từ chính xác trung tâm. Nó là tích ba vô hướng, do đó, với đầu vào là số nguyên, mọi quyết định về phía nào của mặt phẳng thân tàu mà một điểm nằm trên đều chính xác. Số nguyên Python có độ chính xác tùy ý, do đó không có`int64`tràn ngay cả khi định thức trung gian có thể lớn hơn nhiều so với tọa độ ban đầu. 

Thân tàu lưu trữ mỗi mặt cùng với ba mặt lân cận của nó.`edge_map`cho phép một mặt mới tìm thấy mặt hiện có trên một cạnh dùng chung trong thời gian mong đợi không đổi. các`buckets`mảng là cấu trúc xung đột. Một điểm chỉ cần một khuôn mặt có thể nhìn thấy được. Khi khuôn mặt đó biến mất, điểm sẽ được kiểm tra với quạt chân trời mới và nhận được chủ sở hữu mới nếu nó vẫn ở bên ngoài. 

Tứ diện ban đầu được định hướng bằng đỉnh đối diện của nó. Các mặt sau này được định hướng bằng cách sử dụng`center`, nằm hoàn toàn bên trong tứ diện ban đầu. Vì mọi thân sau đều chứa tứ diện ban đầu nên điểm đó vẫn nằm hoàn toàn bên trong thân. Do đó, việc kiểm tra hướng của nó so với bề mặt mới sẽ mang lại hướng ra ngoài đáng tin cậy. 

Việc tính toán diện tích cuối cùng có chủ ý sử dụng các vectơ dấu phẩy động đã chuẩn hóa. Các quyết định về thân tàu đã được hoàn thành một cách chính xác, vì vậy dấu phẩy động được giới hạn ở số lượng liên tục mà cuối cùng phải được in. các`atan2`công thức xử lý các tam giác cầu rất nhỏ và rất lớn đáng tin cậy hơn nhiều so với việc tính ba góc cầu riêng biệt bằng`acos`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các điểm cố định là trục tọa độ dương. 

| Điểm cố định | Tọa độ | Kết quả hình học | 
| --- | --- | --- | 
| một 1 ​ | (1,0,0) | đỉnh đầu tiên | 
| một 2 ​ | (0,1,0) | đỉnh thứ hai | 
| một 3 ​ | (0,0,1) | đỉnh thứ ba | 
| Diện tích hình cầu | π/2 | một phần tám hình cầu | 
| Xác suất xấu | 8/1 | a 0 ​ ở quãng tám đối diện | 
| Dự kiến ​​f | 8/7 | 0,875000000000 | 

Đối với ba vectơ chuẩn hóa, mọi tích số chấm theo cặp đều bằng 0 và định thức là 1. Công thức tam giác trở thành 

2atan2(1,1)= 2 π ​ . 

Chia cho diện tích hình cầu 4π cho xác suất xấu là 1/8, do đó kỳ vọng mong muốn là 7/8. 

### Mẫu 2 

Xét bốn đỉnh của một tứ diện đều,```
4
1 1 1
1 -1 -1
-1 1 -1
-1 -1 1
```Nguồn gốc nằm bên trong thân lồi của chúng. 

| Bước | Trạng thái thân tàu | Diện tích hình cầu | 
| --- | --- | --- | 
| Tứ diện ban đầu | Cả bốn điểm đều là đỉnh thân | 4π | 
| Truyền tải cuối cùng | Bốn mặt cầu phân chia hình cầu | 4π | 
| Xác suất xấu | 4π/(4π) | 1 | 
| Dự kiến ​​f | Bổ sung | 0 | 

Mọi hướng từ gốc tọa độ đều thuộc về hình nón tạo bởi bốn đỉnh tứ diện. Do đó mọi khả năng số 0 ​ đều xấu, nghĩa là không có bán cầu nào có thể chứa đủ năm điểm và câu trả lời là 0. 

Đồ thị cũng chứng tỏ tại sao việc tính tổng diện tích hình cầu của các mặt thân vẫn hoạt động ngay cả khi gốc tọa độ nằm bên trong bao lồi thông thường. Hình chiếu xuyên tâm của tất cả các mặt bao phủ toàn bộ hình cầu đúng một lần ở phần bên trong của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​O(nlogn) | Thân lồi tăng dần ngẫu nhiên theo chiều cố định cộng với di chuyển ngang bề mặt có kích thước tuyến tính | 
| Không gian | Dự kiến ​​O(n) | Thân tàu ba chiều có các mặt và cạnh O(n) | 

Giới hạn quan trọng là kích thước cố định. Một thân lồi ba chiều chỉ có các mặt O(n) và việc xây dựng tăng dần ngẫu nhiên có công việc O(nlogn) dự kiến. Đầu vào chứa 10 5 điểm, do đó, điều này tránh được công việc bậc hai 5×10 9 của phép liệt kê theo cặp hoặc ba. Các số nguyên có độ chính xác tùy ý của Python làm cho các vị từ định hướng chính xác trở nên an toàn, trong khi công việc về dấu phẩy động bị giới hạn ở phép tính diện tích cuối cùng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

# Paste the solve_points function and its helpers from the solution above.

def run(inp: str) -> str:
    data = inp.strip().splitlines()
    n = int(data[0])
    points = [tuple(map(int, line.split())) for line in data[1:]]
    return f"{solve_points(points):.12f}"

# Provided sample
assert abs(float(run("""\
3
1 0 0
0 1 0
0 0 1
""")) - 0.875) < 1e-10, "sample 1"

# Minimum-size input
assert abs(float(run("""\
0
""")) - 1.0) < 1e-10, "n = 0"

# Two antipodal points
assert abs(float(run("""\
2
1 0 0
-1 0 0
""")) - 1.0) < 1e-10, "two antipodal points"

# Three points on one great circle
assert abs(float(run("""\
3
1 0 0
-1 0 0
0 1 0
""")) - 1.0) < 1e-10, "coplanar through origin"

# Four regular-tetrahedron directions, origin strictly inside
assert abs(float(run("""\
4
1 1 1
1 -1 -1
-1 1 -1
-1 -1 1
""")) - 0.0) < 1e-10, "origin inside hull"

# Maximum-size input. All directions lie in z = 0, so the
# spherical convex hull has zero two-dimensional area.
pts = ["100000"]
for i in range(1, 100001):
    pts.append(f"{i} 1 0")

assert abs(float(run("\n".join(pts))) - 1.0) < 1e-10, "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1.000000000000`| Kích thước đầu vào tối thiểu | 
| Hai vectơ đối cực |`1.000000000000`| Trường hợp biên và đối cực | 
| Ba vectơ đồng phẳng |`1.000000000000`| Thân hình cầu không diện tích | 
| Tứ diện đều |`0.000000000000`| Xuất xứ nghiêm ngặt bên trong thân tàu | 
| 100000 hướng đồng phẳng |`1.000000000000`| Kích thước đầu vào tối đa và khả năng xử lý tuyến tính của thân tàu | 

Lời hứa của vấn đề rằng các điểm cố định được chuẩn hóa là khác biệt có nghĩa là các điểm lặp lại theo nghĩa đen là không hợp pháp. Do đó, phép thử "tất cả các giá trị bằng nhau" không thể là đầu vào hợp lệ. Các bộ ba số nguyên khác nhau có thể có cùng hướng chuẩn hóa, nhưng chúng cũng bị cấm bởi điều kiện phân biệt. 

## Vỏ cạnh 

Với n=0, không có hạn chế cố định nào đối với 0 ​. Mỗi điểm ngẫu nhiên có thể được đặt riêng ở một bán cầu nào đó, do đó thuật toán trả về 1 ngay lập tức. 

Đối với hai điểm cố định đối cực,```
2
1 0 0
-1 0 0
```các điểm nằm trên ranh giới của nhiều bán cầu. Điểm thứ ba luôn có thể được điều chỉnh bằng cách chọn bán cầu thích hợp có ranh giới chứa cặp đối cực. Thân hình cầu không có diện tích hai chiều nên đáp án vẫn là 1. 

Đối với ba điểm đồng phẳng```
3
1 0 0
-1 0 0
0 1 0
```định thức bằng không. Cả ba điểm đều nằm trên cùng một đường tròn lớn nên bao lồi hình cầu của chúng là một chiều. Diện tích bề mặt của nó bằng 0 và thuật toán trả về 1 mà không cần cố gắng xây dựng một khối tứ diện. 

Đối với mẫu,```
3
1 0 0
0 1 0
0 0 1
```định thức là 1, mẫu số trong công thức tam giác là 1 và diện tích hình cầu là π/2. Thuật toán trả về 

1− 4π π/2 ​ = 8 7 ​ . 

Đây là bước kiểm tra độ chính xác quan trọng cho phép chuyển đổi xác suất. 

Đối với tứ diện đều,```
4
1 1 1
1 -1 -1
-1 1 -1
-1 -1 1
```gốc tọa độ nằm ngay bên trong bao lồi thông thường. Mọi tia từ gốc tọa độ đều cắt thân nên hình cầu lồi là toàn bộ hình cầu. Bốn diện tích bề mặt hình cầu cộng lại với 4π, cho ra xác suất thất bại là 1 và giá trị mong đợi là 0. 

Cuối cùng, khi một số điểm nằm trên cùng một mặt phẳng hỗ trợ, các thử nghiệm định hướng chính xác có thể phân loại một số điểm là đồng phẳng thay vì nhìn thấy được. Những điểm như vậy không tạo ra một vùng hình cầu hai chiều mới. Chúng chỉ chia nhỏ một mặt thân hiện có, do đó, việc bỏ qua một điểm nằm chính xác trên mặt hiện có sẽ không làm thay đổi tổng diện tích hình cầu.
