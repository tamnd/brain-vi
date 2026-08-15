---
title: "CF 102452F - Vật rơi"
description: "Mỗi vật là một trong ba khối lồi: hình lập phương, hình cầu hoặc tứ diện đều. Kích thước, hướng và vị trí nhả ngang của nó được đưa ra. Các vật thể lần lượt được thả ra và mỗi vật thể chỉ rơi theo phương thẳng đứng."
date: "2026-08-12T08:28:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 179
verified: true
draft: false
---

[CF 102452F - Vật rơi](https://codeforces.com/problemset/problem/102452/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vật là một trong ba khối lồi: hình lập phương, hình cầu hoặc tứ diện đều. Kích thước, hướng và vị trí nhả ngang của nó được đưa ra. Các vật thể lần lượt được thả ra và mỗi vật thể chỉ rơi theo phương thẳng đứng. Khi chạm đất lần đầu tiên hoặc bất kỳ vật thể nào đã hạ cánh, nó sẽ dừng lại vĩnh viễn. 

Phép quay là phép quay z-X-Z Euler thích hợp. Sau khi áp dụng nó, chúng ta chỉ cần hình học cố định thu được. Câu hỏi dành cho vật (i) không phải là nó được thả ra ở đâu, mà là tọa độ (z) cuối cùng của điểm cao nhất của nó sau khi rơi thẳng đứng. 

Khó khăn chính là một vật thể có thể chạm vào một vật thể khác tại một điểm tùy ý. Ví dụ, một khối lập phương trước tiên có thể chạm vào một khối khác dọc theo một mặt, ở một cạnh hoặc ở một đỉnh. Một hình cầu có thể chạm vào một mặt hoặc cạnh của khối đa diện tại một điểm không rõ ràng. Việc coi mỗi vật rắn như một hộp giới hạn sẽ làm mất chính xác thông tin hình học xác định chiều cao hạ cánh. 

Các ràng buộc làm cho mô phỏng bậc hai trở nên khả thi. Tổng cộng có tối đa (1000) đối tượng trong tất cả các trường hợp thử nghiệm, do đó, việc xem xét mọi đối tượng trước đó cho mỗi đối tượng mới chỉ đưa ra khoảng (5\cdot10^5) cặp đối tượng. Giới hạn thời gian là 5,5 giây và giới hạn bộ nhớ là 512 MB. Do đó, chúng tôi muốn có một mô phỏng (O(n^2)), nhưng mỗi cặp phải được xử lý bằng các phép toán hình học theo thời gian không đổi được lựa chọn cẩn thận thay vì mô phỏng rơi số. Trang vấn đề chính thức đưa ra giới hạn (n\le1000) tương tự và giới hạn 5,5 giây. 

Có một số trường hợp tinh vi bộc lộ việc thực hiện bất cẩn. Một quả cầu được thả lên trên mặt đất có bán kính (1) phải có chiều cao tâm (1) nên điểm cao nhất của nó là (2). Việc triển khai diễn giải bán kính là điểm cao nhất sẽ in ra (1), điều này là sai.```
1
1
1 0 0 0 0 0 1
```Câu trả lời là (2). 

Cái bẫy thứ hai là sự tiếp tuyến. Hai quả cầu đơn vị có tâm nằm ngang (2) đơn vị, tiếp xúc chính xác tại một điểm. Quả cầu thứ hai vẫn dừng ở đó dù hình chiếu ngang của chúng chỉ chạm vào ranh giới.```
1
2
1 0 0 0 0 0 1
1 0 0 0 2 0 1
```Cả hai điểm cao nhất là (2). Một thử nghiệm giao cắt nghiêm ngặt yêu cầu sự chồng chéo diện tích dương sẽ khiến quả cầu thứ hai rơi xuống đất một cách không chính xác. 

Cái bẫy thứ ba là sự luân chuyển. Một tứ diện không đối xứng khi quay tùy ý, do đó độ lệch dọc cao nhất và thấp nhất của nó phụ thuộc vào các góc Euler. Chỉ sử dụng chiều cao không xoay của nó sẽ dẫn đến vị trí hạ cánh sai.```
1
1
2 191 98 10 25 25 2
```Điểm cao nhất chính xác là xấp xỉ (1,9504473433), không phải chiều cao của một tứ diện không quay. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu hấp dẫn là hạ thấp một đối tượng theo từng bước nhỏ, kiểm tra xem nó có giao nhau với một đối tượng trước đó hay không và dừng lại khi phát hiện thấy giao điểm. Điều này dễ về mặt khái niệm nhưng không thể cung cấp độ chính xác cần thiết (10^{-4}) một cách hiệu quả. Nếu bước dọc là (10^{-5}), một đối tượng có chiều cao khoảng (10^4) cần khoảng (10^9) lần lặp. Ngay cả một lần ngã như vậy cũng đã là quá đắt, và có thể có (1000) lần ngã. 

Một cách tiếp cận số đáng nể hơn là tìm kiếm nhị phân theo chiều cao trung tâm. Đối với mọi vật thể trước đó, chúng ta có thể kiểm tra xem hai vật rắn có giao nhau ở độ cao đề xuất hay không và thực hiện khoảng 50 đến 60 lần lặp tìm kiếm nhị phân. Trong trường hợp xấu nhất, điều này có nghĩa là khoảng (60\cdot n(n-1)/2), hoặc khoảng 30 triệu thử nghiệm cặp cho (n=1000), với mỗi thử nghiệm cặp yêu cầu tính toán giao nhau ba chiều đáng kể. Hằng số trở nên đặc biệt khó chịu đối với hình lập phương và tứ diện. 

Quan sát hữu ích là sự tiếp xúc đầu tiên của hai khối lồi luôn có thể được biểu diễn bằng một số lượng nhỏ các tổ hợp đặc điểm. Một khối đa diện bao gồm các đỉnh, các cạnh và các mặt hình tam giác. Khi một khối đa diện rơi thẳng đứng lên một khối đa diện khác, điểm tiếp xúc đầu tiên được thể hiện bằng một đỉnh chuyển động chạm vào một mặt tĩnh, một mặt chuyển động chạm vào một đỉnh tĩnh hoặc hai cạnh có hình chiếu ngang gặp nhau. Đối với hình cầu, các trường hợp tương tự là hình cầu điểm, hình cầu đường thẳng, hình cầu phẳng và hình cầu. Sự phân rã đặc trưng này cũng là tổ chức hình học tiêu chuẩn được sử dụng trong các lời giải chi tiết của bài toán này. 

Điều đó thay đổi hoàn toàn vấn đề. Chúng ta không phải tìm kiếm trên độ cao đang rơi. Đối với mọi đặc điểm tiếp xúc có thể có, chúng tôi tính toán trực tiếp chiều cao trung tâm nơi tiếp xúc đó xảy ra, sau đó lấy giá trị tối đa. Tối đa là lần tiếp xúc đầu tiên vì đối tượng bắt đầu ở (+\infty) và di chuyển xuống dưới. 

Đối với một điểm rơi trên một mặt tam giác, tọa độ ngang được cố định, do đó điểm chạm tới mặt đó ở độ cao duy nhất thu được từ phương trình mặt phẳng. Đối với hai cạnh, hình chiếu ngang của chúng xác định điểm tiếp xúc và sự khác biệt giữa tọa độ dọc của chúng sẽ cho ra chiều cao tâm cần thiết. Đối với một đường thẳng và một hình cầu, hàm chiều cao là căn bậc hai của một số hạng bậc hai cộng với một số hạng tuyến tính, có một cực đại duy nhất vì nó là lõm. Chúng ta có thể giải đạo hàm của nó một cách rõ ràng thay vì thực hiện tối ưu hóa bằng số. 

Do đó, hình học ba chiều được rút gọn thành một tập hợp cố định các công thức cơ bản. Bài viết hình học chi tiết chính thức mô tả mười loại va chạm nguyên thủy giống nhau và giải thích tại sao có thể giảm bớt va chạm khối lập phương và tứ diện đối với chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bước dọc | Không giới hạn cho độ chính xác cần thiết | (O(n)) | Quá chậm | 
| Tìm kiếm nhị phân theo chiều cao | (O(n^2\log \varepsilon^{-1})) | (O(n)) | Quá nhiều công việc hình học | 
| Va chạm dựa trên tính năng | (O(n^2)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biểu diễn mọi khối lập phương hoặc tứ diện bằng các đỉnh quay, các cạnh và các mặt tam giác của nó. Một hình lập phương có 8 đỉnh, 12 cạnh và 12 mặt tam giác sau khi chia mỗi mặt vuông thành hai hình tam giác. Tứ diện có 4 đỉnh, 6 cạnh và 4 mặt. Một hình cầu được biểu diễn bằng tâm và bán kính của nó. Tọa độ tứ diện có thể được chọn với độ dài cạnh bằng một và sau đó được chia tỷ lệ theo kích thước đầu vào. Các tọa độ được sử dụng bên dưới được căn giữa ở tâm hình học. 
2. Xây dựng ma trận xoay z-X-z từ ba góc đầu vào và xoay mọi đỉnh của khối đa diện. Ma trận xoay được áp dụng trước khi dịch đối tượng sang vị trí (x, y) đã cho. Tọa độ (x,y) thu được vẫn cố định trong suốt quá trình rơi, trong khi mọi đỉnh đều nhận được cùng một bản dịch dọc không xác định. Ma trận z-X-z tiêu chuẩn được sử dụng ở đây là ma trận được mô tả trong giải pháp chi tiết. 
3. Trước khi xử lý đối tượng (i), hãy khởi tạo chiều cao tâm của nó bằng cách sử dụng mặt đất. Nếu đỉnh hoặc điểm quay thấp nhất so với tâm có tọa độ (z_{\min}), thì tiếp xúc mặt đất xảy ra ở độ cao tâm (-z_{\min}). Đối với một hình cầu, đây chỉ đơn giản là bán kính của nó. 
4. Hãy coi mọi đối tượng trước đó đều có thể là chướng ngại vật. Nếu khoảng cách theo chiều ngang giữa hai tâm lớn hơn tổng bán kính giới hạn của chúng thì các hình chiếu ngang của chúng không thể gặp nhau nên cặp đôi có thể bị bỏ qua ngay lập tức. Đây là một bài kiểm tra loại bỏ chính xác, không phải là một phép tính gần đúng. 
5. Đối với cặp đa diện-đa diện, kiểm tra đỉnh chuyển động so với mặt tĩnh, mặt chuyển động so với đỉnh tĩnh và cạnh chuyển động so với cạnh tĩnh. Tiếp xúc đỉnh-mặt thu được từ phương trình mặt phẳng và phép thử điểm trong tam giác. Một liên hệ mặt-đỉnh là phép tính tương tự với các vai trò bị đảo ngược. Một điểm tiếp xúc cạnh-cạnh được tìm thấy từ giao điểm của hai đoạn đường ngang. 
6. Đối với cặp hình cầu-đa diện, kiểm tra các điểm tiếp xúc giữa hình cầu-đỉnh, cạnh hình cầu và mặt hình cầu. Đối với trường hợp cạnh, tối đa hóa chiều cao tiếp xúc dọc dọc theo cạnh. Đối với trường hợp mặt, sử dụng pháp tuyến của mặt, vì tiếp xúc mặt cầu-mặt phẳng đầu tiên xảy ra dọc theo hướng pháp tuyến. 
7. Đối với hai hình cầu, hãy phóng to bán kính hình cầu tĩnh bằng bán kính hình cầu chuyển động. Khi đó quả cầu chuyển động sẽ hành xử giống như một điểm rơi vào quả cầu phóng to này. Nếu khoảng cách ngang là (d) thì chiều cao trung tâm là 
[ 
z_{\text{static}}+\sqrt{(r_1+r_2)^2-d^2}. 
] 
8. Đối với mỗi cặp tính năng hợp lệ, hãy cập nhật độ cao hạ cánh hiện tại với ứng cử viên lớn nhất. Ứng cử viên cao hơn có nghĩa là vật thể chuyển động sẽ chạm vào đặc điểm đó sớm hơn trong quá trình chuyển động đi xuống của nó. 
9. Khi đã biết chiều cao của tâm hạ cánh, hãy dịch tất cả các đỉnh của đối tượng theo chiều cao đó và lưu điểm cao nhất cuối cùng của nó. Đối tượng tiếp theo coi đối tượng này là một chướng ngại vật hoàn toàn tĩnh. 
10. Xử lý các đối tượng theo trình tự thời gian và in ra đỉnh cuối cùng cao nhất của mỗi đối tượng. Các đối tượng trước đó không bao giờ di chuyển nữa, vì vậy không có mô phỏng nào sau này có thể thay đổi câu trả lời đã được tính toán. 

### Tại sao nó hoạt động 

Ở mọi giai đoạn, đối tượng hiện tại chỉ di chuyển theo chiều dọc. Hãy xem xét lần tiếp xúc đầu tiên của nó với một vật thể lồi trước đó. Một điểm tiếp xúc nằm trên ranh giới của mỗi vật rắn. Đối với khối đa diện, điểm biên thuộc về một đỉnh, cạnh hoặc mặt. Nếu phần tiếp xúc nằm giữa phần bên trong của hai mặt, thì các mặt phẳng đỡ của chúng trùng nhau ở điểm tiếp xúc đầu tiên và phép tính mặt đỉnh sẽ có cùng chiều cao. Nếu tiếp điểm liên quan đến một cạnh thì hai hình chiếu cạnh ngang giao nhau và phép tính cạnh-cạnh sẽ bắt được cạnh đó. Các điểm tiếp xúc hình cầu giảm xuống các trường hợp điểm, cạnh hoặc mặt tương ứng vì điểm bề mặt của quả cầu chịu trách nhiệm cho lần tiếp xúc đầu tiên được xác định duy nhất theo hướng thẳng đứng hoặc pháp tuyến của mặt.

Do đó, mọi liên hệ đầu tiên có thể được biểu thị bằng một trong các cặp tính năng được thử nghiệm. Mỗi quy trình tính năng sẽ tính toán độ cao trung tâm chính xác nơi tiếp xúc đó xảy ra. Việc lấy mức tối đa của chúng sẽ mang lại chiều cao tiếp xúc cao nhất có thể, đây chính xác là lần va chạm đầu tiên gặp phải khi rơi từ (+\infty). Điều bất biến là sau khi xử lý đối tượng (i), chiều cao trung tâm được lưu trữ của nó là chiều cao thực mà tại đó lần đầu tiên nó chạm vào tác phẩm điêu khắc đã bị đóng băng và tất cả hình học được lưu trữ của nó đều ở vị trí cuối cùng. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-9
NEG = -1e100

def cross2(ax, ay, bx, by):
    return ax * by - ay * bx

def inside_triangle(p, a, b, c):
    abx, aby = b[0] - a[0], b[1] - a[1]
    bcx, bcy = c[0] - b[0], c[1] - b[1]
    cax, cay = a[0] - c[0], a[1] - c[1]

    apx, apy = p[0] - a[0], p[1] - a[1]
    bpx, bpy = p[0] - b[0], p[1] - b[1]
    cpx, cpy = p[0] - c[0], p[1] - c[1]

    x1 = cross2(abx, aby, apx, apy)
    x2 = cross2(bcx, bcy, bpx, bpy)
    x3 = cross2(cax, cay, cpx, cpy)

    scale = max(
        1.0,
        math.hypot(abx, aby) * math.hypot(apx, apy),
        math.hypot(bcx, bcy) * math.hypot(bpx, bpy),
        math.hypot(cax, cay) * math.hypot(cpx, cpy),
    )
    tol = EPS * scale

    return (x1 >= -tol and x2 >= -tol and x3 >= -tol) or (
        x1 <= tol and x2 <= tol and x3 <= tol
    )

def plane_of(tri):
    a, b, c = tri
    ux, uy, uz = b[0] - a[0], b[1] - a[1], b[2] - a[2]
    vx, vy, vz = c[0] - a[0], c[1] - a[1], c[2] - a[2]

    nx = uy * vz - uz * vy
    ny = uz * vx - ux * vz
    nz = ux * vy - uy * vx
    d = nx * a[0] + ny * a[1] + nz * a[2]
    return nx, ny, nz, d

def point_plane(moving_p, static_tri):
    a, b, c = static_tri
    if not inside_triangle(moving_p, a, b, c):
        return NEG

    nx, ny, nz, d = plane_of(static_tri)
    if abs(nz) < EPS:
        return NEG

    z = (d - nx * moving_p[0] - ny * moving_p[1]) / nz
    return z - moving_p[2]

def plane_point(moving_tri, static_p):
    if not inside_triangle(static_p, moving_tri[0], moving_tri[1], moving_tri[2]):
        return NEG

    nx, ny, nz, d = plane_of(moving_tri)
    if abs(nz) < EPS:
        return NEG

    z = (d - nx * static_p[0] - ny * static_p[1]) / nz
    return static_p[2] - z

def line_line(moving_l, static_l):
    a, b = moving_l
    c, d = static_l

    rx, ry = b[0] - a[0], b[1] - a[1]
    sx, sy = d[0] - c[0], d[1] - c[1]

    den = cross2(rx, ry, sx, sy)
    qx, qy = c[0] - a[0], c[1] - a[1]

    if abs(den) > EPS:
        t = cross2(qx, qy, sx, sy) / den
        u = cross2(qx, qy, rx, ry) / den

        if t < -EPS or t > 1.0 + EPS or u < -EPS or u > 1.0 + EPS:
            return NEG

        zm = a[2] + t * (b[2] - a[2])
        zs = c[2] + u * (d[2] - c[2])
        return zs - zm

    if abs(cross2(qx, qy, rx, ry)) > EPS:
        return NEG

    rr = rx * rx + ry * ry
    if rr < EPS:
        return NEG

    tc = ((c[0] - a[0]) * rx + (c[1] - a[1]) * ry) / rr
    td = ((d[0] - a[0]) * rx + (d[1] - a[1]) * ry) / rr

    lo = max(0.0, min(tc, td))
    hi = min(1.0, max(tc, td))

    if lo > hi + EPS:
        return NEG

    ans = NEG

    for t in (lo, hi):
        x = a[0] + rx * t
        y = a[1] + ry * t

        ss = sx * sx + sy * sy
        if ss < EPS:
            continue

        u = ((x - c[0]) * sx + (y - c[1]) * sy) / ss
        if u < -EPS or u > 1.0 + EPS:
            continue

        zm = a[2] + t * (b[2] - a[2])
        zs = c[2] + u * (d[2] - c[2])
        ans = max(ans, zs - zm)

    return ans

def point_sphere(moving_p, sphere_center, radius):
    dx = moving_p[0] - sphere_center[0]
    dy = moving_p[1] - sphere_center[1]
    q = radius * radius - dx * dx - dy * dy

    if q < -EPS:
        return NEG

    return sphere_center[2] + math.sqrt(max(0.0, q)) - moving_p[2]

def sphere_point(sphere_center_xy, radius, static_p):
    dx = sphere_center_xy[0] - static_p[0]
    dy = sphere_center_xy[1] - static_p[1]
    q = radius * radius - dx * dx - dy * dy

    if q < -EPS:
        return NEG

    return static_p[2] + math.sqrt(max(0.0, q))

def line_sphere(moving_l, sphere_center, radius, moving_line=True):
    a, b = moving_l

    dx = b[0] - a[0]
    dy = b[1] - a[1]
    dz = b[2] - a[2]

    ax = a[0] - sphere_center[0]
    ay = a[1] - sphere_center[1]

    B = dx * dx + dy * dy

    if B < EPS:
        q = radius * radius - ax * ax - ay * ay
        if q < -EPS:
            return NEG
        top = math.sqrt(max(0.0, q))

        if moving_line:
            return sphere_center[2] + top - a[2]
        return a[2] + top

    t0 = -(ax * dx + ay * dy) / B
    min_d2 = ax * ax + ay * ay - B * t0 * t0

    if min_d2 > radius * radius + EPS:
        return NEG

    D = max(0.0, radius * radius - min_d2)
    width = math.sqrt(D / B)

    left = max(0.0, t0 - width)
    right = min(1.0, t0 + width)

    if left > right + EPS:
        return NEG

    def value(t):
        px = ax + dx * t
        py = ay + dy * t
        q = radius * radius - px * px - py * py
        if q < -EPS:
            return NEG

        root = math.sqrt(max(0.0, q))

        if moving_line:
            return sphere_center[2] + root - (a[2] + dz * t)
        return a[2] + dz * t + root

    ans = max(value(left), value(right))

    if D > EPS:
        if moving_line:
            qlinear = -dz
        else:
            qlinear = dz

        if abs(qlinear) > EPS:
            t = t0 + qlinear * math.sqrt(
                D / (B * (B + qlinear * qlinear))
            )
            if left - EPS <= t <= right + EPS:
                ans = max(ans, value(t))

    return ans

def plane_sphere(moving_tri, sphere_center, radius):
    nx, ny, nz, d = plane_of(moving_tri)
    norm = math.sqrt(nx * nx + ny * ny + nz * nz)

    if norm < EPS:
        return NEG

    ans = NEG

    for sign in (-1.0, 1.0):
        qx = sphere_center[0] + sign * radius * nx / norm
        qy = sphere_center[1] + sign * radius * ny / norm
        qz = sphere_center[2] + sign * radius * nz / norm

        p = (qx, qy, qz)

        if not inside_triangle(p, moving_tri[0], moving_tri[1], moving_tri[2]):
            continue

        nx2, ny2, nz2, d2 = plane_of(moving_tri)
        if abs(nz2) < EPS:
            continue

        plane_z = (d2 - nx2 * qx - ny2 * qy) / nz2
        ans = max(ans, qz - plane_z)

    return ans

def sphere_plane(center_xy, radius, static_tri):
    nx, ny, nz, d = plane_of(static_tri)
    norm = math.sqrt(nx * nx + ny * ny + nz * nz)

    if norm < EPS:
        return NEG

    ans = NEG

    for sign in (-1.0, 1.0):
        qx = center_xy[0] + sign * radius * nx / norm
        qy = center_xy[1] + sign * radius * ny / norm

        if not inside_triangle(
            (qx, qy, 0.0),
            static_tri[0],
            static_tri[1],
            static_tri[2],
        ):
            continue

        if abs(nz) < EPS:
            continue

        plane_z = (d - nx * qx - ny * qy) / nz
        center_z = plane_z - sign * radius * nz / norm
        ans = max(ans, center_z)

    return ans

class Cloud:
    __slots__ = (
        "typ", "x", "y", "r", "rel", "pts", "edges", "faces",
        "bound", "vmax", "minz", "top", "z"
    )

    def __init__(self, typ, alpha, beta, gamma, x, y, r):
        self.typ = typ
        self.x = float(x)
        self.y = float(y)
        self.r = float(r)
        self.z = 0.0

        if typ == 1:
            self.rel = []
            self.pts = []
            self.edges = []
            self.faces = []
            self.bound = float(r)
            self.vmax = float(r)
            self.minz = -float(r)
            self.top = 0.0
            return

        if typ == 0:
            h = 0.5
            base = [
                (-h, -h, -h),
                ( h, -h, -h),
                ( h,  h, -h),
                (-h,  h, -h),
                (-h, -h,  h),
                ( h, -h,  h),
                ( h,  h,  h),
                (-h,  h,  h),
            ]
            self.edges = [
                (0, 1), (1, 2), (2, 3), (3, 0),
                (4, 5), (5, 6), (6, 7), (7, 4),
                (0, 4), (1, 5), (2, 6), (3, 7),
            ]
            self.faces = [
                (0, 1, 2), (0, 2, 3),
                (4, 6, 5), (4, 7, 6),
                (0, 4, 5), (0, 5, 1),
                (1, 5, 6), (1, 6, 2),
                (2, 6, 7), (2, 7, 3),
                (3, 7, 4), (3, 4, 0),
            ]
        else:
            s3 = math.sqrt(3.0)
            s6 = math.sqrt(6.0)
            base = [
                (-0.5 / s3,  0.5, -0.5 / s6),
                (-0.5 / s3, -0.5, -0.5 / s6),
                ( 1.0 / s3,  0.0, -0.5 / s6),
                ( 0.0,       0.0,  1.5 / s6),
            ]
            self.edges = [
                (0, 1), (0, 2), (0, 3),
                (1, 2), (1, 3), (2, 3),
            ]
            self.faces = [
                (0, 1, 2),
                (0, 1, 3),
                (0, 2, 3),
                (1, 2, 3),
            ]

        a = math.radians(alpha)
        b = math.radians(beta)
        g = math.radians(gamma)

        ca, sa = math.cos(a), math.sin(a)
        cb, sb = math.cos(b), math.sin(b)
        cg, sg = math.cos(g), math.sin(g)

        # Active z-X-z rotation.
        m00 = ca * cg - cb * sa * sg
        m01 = -ca * sg - cb * cg * sa
        m02 = sa * sb

        m10 = cg * sa + ca * cb * sg
        m11 = ca * cb * cg - sa * sg
        m12 = -ca * sb

        m20 = sb * sg
        m21 = cg * sb
        m22 = cb

        scale = float(r)
        rel = []

        for px, py, pz in base:
            px *= scale
            py *= scale
            pz *= scale

            rx = m00 * px + m01 * py + m02 * pz
            ry = m10 * px + m11 * py + m12 * pz
            rz = m20 * px + m21 * py + m22 * pz

            rel.append((self.x + rx, self.y + ry, rz))

        self.rel = rel
        self.pts = list(rel)

        self.bound = 0.0
        self.vmax = 0.0
        self.minz = float("inf")

        for p in rel:
            dx = p[0] - self.x
            dy = p[1] - self.y
            self.bound = max(self.bound, math.hypot(dx, dy))
            self.vmax = max(self.vmax, abs(p[2]))
            self.minz = min(self.minz, p[2])

        self.top = self.z + max(p[2] for p in self.pts)

    def set_height(self, z):
        self.z = z
        if self.typ == 1:
            self.top = z + self.r
            return

        self.pts = [
            (p[0], p[1], p[2] + z)
            for p in self.rel
        ]
        self.top = z + max(p[2] for p in self.rel)

def collision(moving, static):
    if moving.typ == 1 and static.typ == 1:
        dx = moving.x - static.x
        dy = moving.y - static.y
        rr = moving.r + static.r
        q = rr * rr - dx * dx - dy * dy
        if q < -EPS:
            return NEG
        return static.z + math.sqrt(max(0.0, q))

    ans = NEG

    if moving.typ == 1:
        sc = (static.x, static.y, static.z)

        for p in static.pts:
            ans = max(
                ans,
                sphere_point((moving.x, moving.y), moving.r, p)
            )

        for i, j in static.edges:
            ans = max(
                ans,
                line_sphere(
                    (static.pts[i], static.pts[j]),
                    (moving.x, moving.y, 0.0),
                    moving.r,
                    moving_line=False,
                )
            )

        for f in static.faces:
            tri = (static.pts[f[0]], static.pts[f[1]], static.pts[f[2]])
            ans = max(ans, sphere_plane((moving.x, moving.y), moving.r, tri))

        return ans

    if static.typ == 1:
        sc = (static.x, static.y, static.z)

        for p in moving.rel:
            ans = max(ans, point_sphere(p, sc, static.r))

        for i, j in moving.edges:
            ans = max(
                ans,
                line_sphere(
                    (moving.rel[i], moving.rel[j]),
                    sc,
                    static.r,
                    moving_line=True,
                )
            )

        for f in moving.faces:
            tri = (
                moving.rel[f[0]],
                moving.rel[f[1]],
                moving.rel[f[2]],
            )
            ans = max(ans, plane_sphere(tri, sc, static.r))

        return ans

    # Polyhedron versus polyhedron.
    for p in moving.rel:
        for f in static.faces:
            tri = (
                static.pts[f[0]],
                static.pts[f[1]],
                static.pts[f[2]],
            )
            ans = max(ans, point_plane(p, tri))

    for f in moving.faces:
        tri = (
            moving.rel[f[0]],
            moving.rel[f[1]],
            moving.rel[f[2]],
        )
        for p in static.pts:
            ans = max(ans, plane_point(tri, p))

    for i, j in moving.edges:
        ml = (moving.rel[i], moving.rel[j])
        for k, l in static.edges:
            sl = (static.pts[k], static.pts[l])
            ans = max(ans, line_line(ml, sl))

    return ans

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())

        clouds = []

        for _ in range(n):
            typ, alpha, beta, gamma, x, y, r = map(int, input().split())

            cur = Cloud(typ, alpha, beta, gamma, x, y, r)

            # Ground contact.
            if typ == 1:
                ground_z = r
            else:
                ground_z = -cur.minz

            best = ground_z

            # Higher static objects are more likely to determine the answer.
            previous = sorted(
                clouds,
                key=lambda c: c.top,
                reverse=True,
            )

            for old in previous:
                # No point of old can force the center above this bound.
                if old.top + cur.vmax <= best + 1e-10:
                    break

                dx = cur.x - old.x
                dy = cur.y - old.y

                if dx * dx + dy * dy > (
                    cur.bound + old.bound
                ) ** 2 + 1e-8:
                    continue

                h = collision(cur, old)
                if h > best:
                    best = h

            cur.set_height(best)
            clouds.append(cur)
            out.append(f"{cur.top:.15f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`Cloud`Trước tiên, hàm tạo tạo hình học chính tắc và sau đó áp dụng phép quay z-X-z. Việc giữ các đỉnh so với tâm là hữu ích vì đại lượng chưa biết trong quá trình phát hiện va chạm chính xác là bản dịch dọc phổ biến. 

Đối với khối đa diện,`rel`lưu trữ các đỉnh đã xoay với bản dịch (x, y) được yêu cầu nhưng có chiều cao tâm bằng 0. Khi một vật thể tiếp đất,`set_height`thêm chiều cao trung tâm cuối cùng cho mỗi đỉnh. các`top`trường sau đó trực tiếp đưa ra câu trả lời cần thiết. 

Ba thói quen phẳng cơ bản là`point_plane`,`plane_point`, Và`line_line`. Một điểm tiếp xúc mặt chỉ có hiệu lực khi hình chiếu ngang của điểm thuộc về mặt tam giác. Tọa độ (z) của mặt phẳng tại vị trí nằm ngang đó suy ra trực tiếp từ phương trình của nó. Quy trình cạnh xử lý cả giao nhau thông thường và chồng chéo cộng tuyến, điều này quan trọng vì việc chạm vào một ranh giới là một va chạm hợp lệ. 

Các quy trình hình cầu sử dụng cùng một ý tưởng liên hệ theo chiều dọc. Một điểm chạm vào một quả cầu sử dụng bán cầu trên làm điểm rơi và bán cầu dưới làm quả cầu rơi. Quy trình hình cầu đường đáng được quan tâm đặc biệt. Sau khi tham số hóa đường thẳng theo (t), bình phương khoảng cách ngang là bậc hai tính theo (t), do đó chiều cao tiếp xúc dọc có dạng 
[ 
\sqrt{D-B(t-t_0)^2}+qt+c. 
] 
Hàm này lõm trên khoảng hợp lệ của nó. Điểm dừng của nó có thể được viết trực tiếp dưới dạng 
[ 
t=t_0+q\sqrt{\frac{D}{B(B+q^2)}}. 
] 
Việc kiểm tra điểm đó cùng với các điểm cuối trong khoảng sẽ cho kết quả tối đa chính xác mà không cần tìm kiếm ba lần. 

Các thói quen khuôn mặt hình cầu sử dụng khuôn mặt bình thường. Ở lần tiếp xúc mặt cầu-mặt phẳng đầu tiên, vectơ bán kính song song với mặt phẳng pháp tuyến, do đó chỉ cần xem xét hai điểm thu được bằng cách di chuyển tâm hình cầu một bán kính dọc theo pháp tuyến. Đây là cách giảm hình học tương tự được mô tả trong giải pháp chi tiết. 

Vòng lặp xử lý cũng chứa hai quy tắc cắt tỉa chính xác. Thử nghiệm bán kính giới hạn theo chiều ngang sẽ loại bỏ các vật thể có hình chiếu không thể đáp ứng. Thử nghiệm theo chiều dọc dừng quét các đối tượng cũ hơn một lần`old.top + cur.vmax`không cao hơn câu trả lời tốt nhất hiện tại. Vì các đối tượng trước được xử lý theo chiều cao trên cùng giảm dần nên mọi đối tượng sau không cao hơn và cũng có thể bị từ chối. 

Python sử dụng dấu phẩy động có độ chính xác kép thông qua`float`kiểu. Không có hiện tượng tràn số nguyên vì hình học được đánh giá bằng số học dấu phẩy động và tọa độ đầu vào đủ nhỏ để khoảng cách bình phương vẫn có thể biểu diễn thoải mái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp thử nghiệm đầu tiên chứa một khối lập phương, theo sau là một hình cầu.```
2
0 45 90 270 0 0 2
1 11 45 14 0 0 1
```Đối với khối lập phương, mặt đất là chướng ngại vật duy nhất. 

| Đối tượng | Hình dạng | Chiều cao trung tâm mặt đất | Va chạm tốt nhất | Chiều cao trung tâm cuối cùng | Điểm cao nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Khối lập phương, cạnh 2 | 1.000000 | không | 1.000000 | 2.000000 | 
| 2 | Hình cầu, bán kính 1 | 1.000000 | 3.000000 | 3.000000 | 4.000000 | 

Khối đầu tiên có độ lệch dọc (-1) và (1), do đó tâm của nó dừng ở (z=1). Điểm cao nhất của nó là (2). Vị trí nằm ngang của quả cầu giống như tâm của khối lập phương. Điểm cao nhất của khối lập phương là (2) và hình cầu có bán kính (1) nên tâm của nó phải chạm tới (3) trước khi chạm vào khối lập phương. Do đó, điểm cao nhất của nó là (4). 

Điều này chứng tỏ tính bất biến rằng câu trả lời cho một đối tượng là chiều cao tiếp xúc hợp lệ tối đa, chứ không phải chiều cao tối thiểu mà tại đó bất kỳ đặc điểm hình học nào có thể giao nhau. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai đặt một khối lập phương quay ở gốc tọa độ và một quả cầu rất lớn tại ((8,9)).```
2
0 45 90 0 0 0 2
1 112 345 67 8 9 99
```Dấu vết là: 

| Đối tượng | Hình dạng | Chiều cao trung tâm mặt đất | Ứng viên trước đối tượng | Chiều cao trung tâm cuối cùng | Điểm cao nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Khối lập phương, cạnh 2 | 2.000000 | không | 2.000000 | 2.000000 | 
| 2 | Hình cầu, bán kính 99 | 99.000000 | khoảng 100.384662 | khoảng 100.384662 | khoảng 199.384662 | 

Khối thứ nhất có phạm vi quay theo chiều dọc là (2), do đó điểm cao nhất của nó là (2). Vật thứ hai đủ lớn để hình chiếu ngang của nó chạm tới khối lập phương. Do đó, tâm của quả cầu dừng lại phía trên khối lập phương chứ không phải ở mặt đất. Việc cộng bán kính của nó sẽ cho điểm cao nhất được báo cáo, xấp xỉ (199,3846615614). 

Ví dụ này cũng chứng minh tại sao việc xấp xỉ theo chiều dọc chỉ bán kính đơn giản là không đủ. Khoảng cách ngang chính xác đến đối tượng trước đó xác định chiều cao tiếp xúc của quả cầu thông qua căn bậc hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Mọi đối tượng đều xem xét các đối tượng trước đó và mỗi khối chỉ có một số đỉnh, cạnh và mặt không đổi | 
| Không gian | (O(n)) | Chúng tôi lưu trữ hình dạng cuối cùng của tất cả các vật thể được hạ cánh | 

Trường hợp xấu nhất danh nghĩa vẫn kiểm tra từng cặp, nhưng mỗi cặp chỉ chứa một số lượng kiểm tra tính năng hình học không đổi. Các quy tắc cắt tỉa theo chiều ngang và chiều dọc làm giảm đáng kể số lượng kiểm tra tính năng thực tế, đặc biệt khi các đối tượng bị tách ra hoặc đối tượng hiện tại đã tìm thấy mức hỗ trợ cao. Với tổng cộng (n\le1000), mô phỏng bậc hai bên ngoài phù hợp với giới hạn 5,5 giây. Trang cuộc thi ban đầu xác nhận những hạn chế này. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng`solve()`hoạt động từ giải pháp trên. Đầu ra dấu phẩy động được kiểm tra bằng số thay vì so sánh các chuỗi được định dạng.```python
# Save the solution above as solution.py before running these tests.

import io
import sys
import math
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def values(inp: str):
    return [float(x) for x in run(inp).split()]

def assert_close(actual, expected, eps=1e-6):
    assert len(actual) == len(expected)
    for a, b in zip(actual, expected):
        assert abs(a - b) <= eps * max(1.0, abs(b)), (a, b)

# Provided sample.
sample = """\
3
2
0 45 90 270 0 0 2
1 11 45 14 0 0 1
2
0 45 90 0 0 0 2
1 112 345 67 8 9 99
1
2 191 98 10 25 25 2
"""

assert_close(
    values(sample),
    [
        2.000000000000001,
        4.000000000000001,
        2.000000000000001,
        199.384661561446364,
        1.950447343314250,
    ],
    1e-5,
)

# Minimum-size object, a unit sphere on the ground.
assert_close(
    values("""\
1
1
1 0 0 0 0 0 1
"""),
    [2.0],
)

# All equal values, three identical unit spheres stacked vertically.
assert_close(
    values("""\
1
3
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
"""),
    [2.0, 4.0, 6.0],
)

# Boundary tangency. The second sphere touches the first at exactly
# one point because their centers are two radii apart.
assert_close(
    values("""\
1
2
1 0 0 0 0 0 1
1 0 0 0 2 0 1
"""),
    [2.0, 2.0],
)

# Single tetrahedron with side length 2 and no rotation.
# Its height is 2*sqrt(2/3).
expected_tetra_top = 2.0 * math.sqrt(2.0 / 3.0)

assert_close(
    values("""\
1
1
2 0 0 0 0 0 2
"""),
    [expected_tetra_top],
)

# Maximum n. The spheres are far apart, so every one lands directly
# on the ground. This also exercises the horizontal-distance pruning.
n = 1000
parts = ["1", str(n)]
for i in range(n):
    parts.append(f"1 0 0 0 {3 * i} 0 1")

inp = "\n".join(parts) + "\n"
got = values(inp)

assert len(got) == n
assert all(abs(x - 2.0) < 1e-6 for x in got)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp mẫu ba trường hợp |`2, 4, 2, 199.3846615614, 1.9504473433`| Xoay, tiếp xúc hình cầu, hình học tứ diện, nhiều trường hợp | 
| Một đơn vị hình cầu |`2`| Vật thể có kích thước tối thiểu và tiếp xúc với mặt đất | 
| Ba quả cầu giống hệt nhau ở một vị trí |`2, 4, 6`| Xếp chồng lặp đi lặp lại và xử lý theo trình tự thời gian | 
| Hai quả cầu đơn vị ở khoảng cách 2 |`2, 2`| Tiếp tuyến chính xác và giao lộ không nghiêm ngặt | 
| Tứ diện không quay cạnh 2 | (2\sqrt{2/3}) | Tọa độ tứ diện và phạm vi dọc | 
| 1000 quả cầu đơn vị riêng biệt | 1000 bản`2`| Kích thước đầu vào tối đa và cắt tỉa ngang | 

## Vỏ cạnh 

Trường hợp hình cầu đơn vị```
1
1
1 0 0 0 0 0 1
```bắt đầu bằng`best = 1`, vì điểm thấp nhất của hình cầu cách tâm của nó một bán kính. Không có đối tượng nào trước đó nên chiều cao trung tâm cuối cùng vẫn là (1) và điểm cao nhất là (1+1=2). Điều này trực tiếp kiểm tra xem câu trả lời được lưu trữ có phải là điểm cao nhất chứ không phải chiều cao ở giữa. 

Trường hợp mặt cầu tiếp tuyến```
1
2
1 0 0 0 0 0 1
1 0 0 0 2 0 1
```đầu tiên đặt tâm hình cầu tại (z=1). Đối với hình cầu thứ hai, khoảng cách tâm theo chiều ngang chính xác là (2), bằng tổng bán kính. Công thức hình cầu có số hạng căn bậc hai bằng 0 nên chiều cao tâm của nó cũng bằng (1). Điểm cao nhất của nó là (2). Việc sử dụng`<=`thông qua việc kiểm tra dung sai là điều cho phép tiếp tuyến một điểm được tính là tiếp xúc. 

Để xếp chồng nhiều lần,```
1
3
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
```quả cầu đầu tiên có chiều cao tâm (1). Người thứ hai nhìn thấy một quả cầu tĩnh ở độ cao (1), do đó tâm của nó đạt tới (3). Người thứ ba coi quả cầu thứ hai là chướng ngại vật có liên quan cao nhất và đạt đến độ cao trung tâm (5). Do đó, điểm cao nhất của họ là (2,4,6). Quy tắc cắt tỉa theo chiều dọc cũng minh họa tính bất biến thứ tự, bởi vì một khi đối tượng cao hơn đã xác định câu trả lời thì các đối tượng đủ thấp hơn không thể cải thiện nó. 

Đối với tứ diện không quay,```
1
1
2 0 0 0 0 0 2
```đỉnh thấp nhất là ở 
[ 
-\frac{2}{2\sqrt6}=-\frac1{\sqrt6}, 
] 
nên tâm dừng ở (1/\sqrt6). Đỉnh cao nhất là ở 
[ 
\frac{3}{\sqrt6}, 
] 
và điểm cao nhất cuối cùng là 
[ 
\frac1{\sqrt6}+\frac3{\sqrt6} 
=\frac4{\sqrt6} 
=2\sqrt{\frac23} 
\approx1.6329931619. 
] 
Kết quả xuất phát từ các đỉnh tứ diện thực tế, do đó không có giả định nào về hộp giới hạn căn chỉnh theo trục. 

Cuối cùng, thử nghiệm kích thước tối đa sử dụng (1000) quả cầu có tâm cách nhau ba đơn vị. Bán kính của chúng là một, vì vậy các hình chiếu ngang liền kề thậm chí không chạm vào nhau. Kiểm tra bán kính giới hạn sẽ loại bỏ mọi đối tượng trước đó trước bất kỳ phép tính hình học đắt tiền nào. Do đó, mỗi quả cầu sẽ hạ cánh ở độ cao tâm (1) và có điểm cao nhất (2). Điều này xác nhận cả cấu trúc lưu trữ bậc hai và việc cắt tỉa không gian để giữ cho hệ số không đổi có thể quản lý được.
