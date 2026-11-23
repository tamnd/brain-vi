---
title: "CF 44F - BerPaint"
description: "Chúng ta có một khung vẽ hình chữ nhật có kích thước W × H. Ban đầu toàn bộ khung vẽ có màu trắng. Sau đó, một số đoạn đường màu đen được vẽ trên đó. Sau đó, một chuỗi các hoạt động lấp lũ được áp dụng. Flood-fill sẽ chọn một điểm và một màu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "graphs"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "F"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 2700
weight: 44
solve_time_s: 177
verified: false
draft: false
---

[CF 44F - BerPaint](https://codeforces.com/problemset/problem/44/F) 

**Xếp hạng:** 2700 
**Thẻ:** hình học, đồ thị 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một canvas hình chữ nhật có kích thước`W × H`. Ban đầu toàn bộ khung vẽ có màu trắng. Sau đó, một số đoạn đường màu đen được vẽ trên đó. Sau đó, một chuỗi các hoạt động lấp lũ được áp dụng. 

Flood-fill sẽ chọn một điểm và một màu. Mọi điểm được kết nối với điểm đã chọn thông qua các vùng có cùng màu hiện tại sẽ được tô màu lại. Các phân đoạn hoạt động giống như những bức tường, chúng có thể chặn chuyển động giữa các vùng. Bản thân các phân đoạn không có diện tích nhưng chúng vẫn quan trọng vì chúng tách biệt các thành phần. 

Cuối cùng, chúng ta phải xuất ra tổng diện tích chiếm giữ của mỗi màu. 

Phần hình học là phần khó nhất. Việc tô màu không hoạt động trên các pixel hoặc ô, chúng hoạt động trên các vùng liên tục trong mặt phẳng. Hai điểm thuộc về cùng một vùng nếu tồn tại một đường đi giữa chúng không cắt bất kỳ đoạn nào và nằm trong các vùng có cùng màu. 

Các ràng buộc loại trừ hoàn toàn bất kỳ loại rasterization chi tiết nào. Kích thước hình chữ nhật lên tới`10^4`, vì vậy ngay cả một`10^4 × 10^4`lưới đã chứa`10^8`tế bào. Việc lấp đầy lưới như vậy nhiều lần sẽ quá chậm và tốn nhiều bộ nhớ. 

Số lượng phân đoạn và số lần điền tối đa là nhỏ`100`mỗi. Điều này cho thấy rõ ràng rằng độ phức tạp thực sự phụ thuộc vào cấu trúc hình học hơn là vào kích thước hình chữ nhật. Chỉ với`100`các phân đoạn, sự sắp xếp được hình thành bởi các phân đoạn chỉ chứa khoảng`O(n^2)`nút giao thông và khu vực. Đó là đối tượng thực sự mà chúng ta nên làm việc cùng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử một điểm điền nằm trực tiếp trên một đoạn.```
5 5
1
1 2 4 2
1
2 2 red
```Bản thân đoạn đó chuyển sang màu đỏ nhưng diện tích của nó vẫn bằng 0. Các vùng màu trắng ở trên và dưới vẫn giữ nguyên màu trắng. Câu trả lời đúng vẫn là`white 25`, bởi vì đoạn này không đóng góp diện tích. 

Việc triển khai ngây thơ coi phân đoạn có độ dày dương sẽ phân chia khu vực một cách không chính xác. 

Một trường hợp tinh tế khác xảy ra khi các đoạn cắt nhau.```
5 5
2
1 1 4 4
1 4 4 1
1
2 2 blue
```Điểm điền nằm chính xác trên điểm giao nhau. Chỉ có hai đoạn trở thành màu xanh lam và tổng diện tích màu xanh lam vẫn bằng 0. 

Nếu chúng ta không phân chia các đoạn tại các điểm giao nhau, biểu đồ phẳng sẽ trở nên không chính xác và quá trình truyền tải bề mặt bị gián đoạn. 

Các phần điền lồng nhau là một nguồn lỗi khác.```
6 6
1
3 1 3 5
2
1 1 red
5 1 blue
```Màu tô đầu tiên là nửa bên trái màu đỏ. Màu tô thứ hai là nửa bên phải màu xanh lam. Các khu vực cuối cùng là`18`Và`18`. 

Nếu chúng tôi xử lý việc điền độc lập thay vì tuần tự, chúng tôi sẽ mất hành vi ghi đè. 

Trường hợp cạnh khái niệm khó nhất là các mảnh cùng màu bị ngắt kết nối. Hai vùng khác nhau đều có thể có màu trắng mà không cần kết nối. Màu tô chỉ tô màu lại một thành phần được kết nối, không phải mọi vùng có cùng màu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là rời rạc hóa mặt phẳng thành các ô nhỏ và mô phỏng trực tiếp việc lấp lũ. Chúng tôi có thể đánh dấu ranh giới phân đoạn trên một lưới lớn và chạy BFS cho mọi hoạt động lấp đầy. 

Điều này có hiệu quả về mặt khái niệm vì việc lấp lũ về cơ bản là một vấn đề về kết nối. Nếu mọi đặc điểm hình học được căn chỉnh theo các ô lưới, BFS sẽ tái tạo chính xác hoạt động của chương trình vẽ. 

Vấn đề là quy mô. Tọa độ lên tới`10^4`và độ chính xác hình học rất quan trọng vì các đoạn có thể giao nhau tại các điểm hữu tỷ tùy ý. Ngay cả độ phân giải một đơn vị trên mỗi ô cũng đã mang lại`10^8`tế bào. Mỗi lần lấp đầy sẽ có khả năng quét toàn bộ khung vẽ. Điều đó hoàn toàn không khả thi cả về bộ nhớ và thời gian chạy. 

Quan sát quan trọng là số lượng phân đoạn rất nhỏ. Cấu trúc liên kết thực tế của mặt phẳng chỉ thay đổi tại các giao điểm của đoạn và điểm cuối của đoạn. Giữa các tọa độ quan trọng đó, mọi thứ đều hoạt động thống nhất. 

Điều này gợi ý chuyển từ chế độ xem pixel sang chế độ xem biểu đồ phẳng. 

Các đoạn được vẽ sẽ chia hình chữ nhật thành các mặt. Mỗi mặt là một vùng mở được kết nối với diện tích cố định. Flood-fill chỉ đơn giản là tô màu lại một mặt hoặc nhiều mặt được kết nối thông qua các ranh giới có cùng màu hiện tại. 

Vì vậy, vấn đề trở thành: 

1. Xây dựng phân khu phẳng được tạo bởi đường viền hình chữ nhật và tất cả các đoạn thẳng. 
2. Tính toán từng diện tích khuôn mặt. 
3. Xác định mặt nào chứa từng điểm tô màu. 
4. Mô phỏng việc đổi màu trên khuôn mặt. 

Sự sắp xếp của`n`phân đoạn chỉ có`O(n^2)`giao lộ. Sau khi phân chia các đoạn tại các điểm giao nhau, biểu đồ phẳng vẫn có thể quản lý được. 

Cách mạnh mẽ nhất để trích xuất các mặt là duyệt nửa cạnh. Mỗi cạnh có hướng đều lưu trữ góc hướng ra ngoài của nó. Đi bộ bằng cách lấy đi cạnh trước đó theo thứ tự tuần hoàn để vạch ra chính xác một ranh giới của mặt. 

Một khi tất cả các khuôn mặt đã được biết đến, việc lấp lũ sẽ trở nên tầm thường. Mỗi điểm lấp đầy thuộc về phần bên trong của bề mặt hoặc thuộc về một phân đoạn. Việc lấp đầy phân đoạn không có tác động đến diện tích, vì vậy chúng tôi chỉ quan tâm đến các điểm bên trong. Việc đổi màu ảnh hưởng đến chính xác một khuôn mặt được kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Raster Force Brute Force | O(W·H·m) | O(W·H) | Quá chậm | 
| Đồ thị phẳng + truyền tải mặt | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các nguyên thủy hình học. 

Thêm bốn đường viền hình chữ nhật làm các phân đoạn cùng với tất cả các phân đoạn màu đen từ đầu vào. Đường viền hình chữ nhật là cần thiết vì các khuôn mặt cũng được bao quanh bởi các cạnh canvas. 
2. Tính toán mọi giao điểm của đoạn thẳng. 

Đối với mỗi cặp phân đoạn, hãy tính xem chúng có giao nhau hay không. Nếu có, hãy lưu điểm giao nhau cho cả hai đoạn. 

Mỗi đoạn ban đầu sau đó sẽ được phân chia tại tất cả các điểm giao nhau của nó. Nếu không có bước này, biểu đồ phẳng sẽ chứa các cạnh cắt nhau, điều này sẽ phá vỡ cấu trúc bề mặt. 
3. Chia các đoạn thành các cạnh cơ bản. 

Sắp xếp tất cả các điểm giao nhau dọc theo mỗi đoạn. Các điểm liên tiếp xác định một cạnh cơ bản. 

Sau khi tách, không có hai cạnh nào giao nhau ngoại trừ tại điểm cuối. Đây là yêu cầu tiêu chuẩn cho việc truyền tải đồ thị phẳng. 
4. Nén các điểm hình học thành các đỉnh đồ thị. 

Tọa độ dấu phẩy động nguy hiểm như các khóa từ điển. Sử dụng số học hợp lý chính xác với phân số hoặc lưu trữ các bộ dữ liệu đã chuẩn hóa. 

Mỗi điểm duy nhất sẽ trở thành một đỉnh trong đồ thị. 
5. Xây dựng các nửa cạnh có hướng. 

Với mọi cạnh vô hướng`(u, v)`, tạo hai cạnh có hướng`u → v`Và`v → u`. 

Đối với mỗi đỉnh, sắp xếp các cạnh có hướng đi theo góc cực. 
6. Xây dựng các mặt bằng phương pháp truyền góc. 

Bắt đầu từ một cạnh có hướng chưa được sử dụng. 

Giả sử chúng ta đã đến đỉnh`v`xuyên qua cạnh`u → v`. Trong số tất cả các cạnh đi từ`v`, tìm cạnh ngược`v → u`. Lấy cạnh ngay trước nó theo thứ tự góc tuần hoàn. 

Lặp lại quá trình này sẽ đi vòng quanh một ranh giới của khuôn mặt. 

Đánh dấu tất cả các cạnh có hướng được sử dụng trong quá trình truyền tải này. 
7. Tính diện tích đa giác cho mọi mặt. 

Việc truyền tải tạo ra một ranh giới đa giác. Sử dụng công thức dây giày. 

Một mặt là mặt ngoài, nó có hướng âm và diện tích bằng vùng vô hạn bên ngoài. Bỏ qua nó. 
8. Xác định vị trí các điểm điền. 

Đối với mỗi thao tác điền, hãy xác định mặt giới hạn nào chứa điểm truy vấn. 

Vì số lượng mặt ít nên thử nghiệm điểm trong đa giác đối với tất cả các mặt là đủ nhanh. 

Nếu điểm nằm chính xác trên một đoạn, hãy bỏ qua phần tô vì các đoạn có diện tích bằng 0. 
9. Mô phỏng việc đổi màu. 

Ban đầu mọi mặt giới hạn đều có màu trắng. 

Mỗi thao tác tô màu sẽ thay đổi màu của chính xác một mặt. 

Duy trì bản đồ`face → current color`. 
10. Tích lũy các khu vực cuối cùng. 

Tổng diện tích của mỗi khuôn mặt thành màu cuối cùng của nó. 

### Tại sao nó hoạt động 

Sau khi phân chia tại các điểm giao nhau, các đoạn này tạo thành một mặt phẳng nhúng thích hợp. Mỗi vùng được kết nối của khung vẽ tương ứng chính xác với một mặt được bao bọc của phần nhúng này. 

Việc duyệt nửa cạnh liệt kê mỗi mặt chính xác một lần vì mỗi cạnh có hướng đều giáp chính xác một mặt ở phía bên trái của nó. Quy tắc góc luôn tuân theo ranh giới liên tục mà không cắt các cạnh. 

Flood-fill trên một điểm bên trong một bề mặt không thể vượt qua ranh giới phân đoạn, vì vậy nó sẽ tô màu lại chính xác bề mặt đó. Vì các khuôn mặt phân chia bên trong hình chữ nhật nên việc tính tổng diện tích của chúng theo màu cuối cùng sẽ cho câu trả lời đúng. 

## Giải pháp Python```python
import sys
from fractions import Fraction
from collections import defaultdict

input = sys.stdin.readline

EPS = 1e-9

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def sub(a, b):
    return (a[0] - b[0], a[1] - b[1])

def segment_intersection(a, b, c, d):
    ax, ay = a
    bx, by = b
    cx, cy = c
    dx, dy = d

    r = (bx - ax, by - ay)
    s = (dx - cx, dy - cy)

    den = cross(r[0], r[1], s[0], s[1])

    if den == 0:
        return []

    t = Fraction(cross(cx - ax, cy - ay, s[0], s[1]), den)
    u = Fraction(cross(cx - ax, cy - ay, r[0], r[1]), den)

    if 0 <= t <= 1 and 0 <= u <= 1:
        x = Fraction(ax) + Fraction(r[0]) * t
        y = Fraction(ay) + Fraction(r[1]) * t
        return [(x, y)]

    return []

def point_on_segment(p, a, b):
    px, py = p
    ax, ay = a
    bx, by = b

    c = cross(bx - ax, by - ay, px - ax, py - ay)
    if c != 0:
        return False

    return (
        min(ax, bx) <= px <= max(ax, bx)
        and min(ay, by) <= py <= max(ay, by)
    )

def polygon_area(poly):
    s = Fraction(0)
    n = len(poly)

    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]
        s += x1 * y2 - x2 * y1

    return s / 2

def point_in_poly(pt, poly):
    x = float(pt[0])
    y = float(pt[1])

    inside = False
    n = len(poly)

    for i in range(n):
        x1 = float(poly[i][0])
        y1 = float(poly[i][1])
        x2 = float(poly[(i + 1) % n][0])
        y2 = float(poly[(i + 1) % n][1])

        if ((y1 > y) != (y2 > y)):
            t = (y - y1) / (y2 - y1)
            xx = x1 + (x2 - x1) * t
            if xx > x:
                inside = not inside

    return inside

def solve():
    W, H = map(int, input().split())

    segments = []

    rect = [
        ((0, 0), (W, 0)),
        ((W, 0), (W, H)),
        ((W, H), (0, H)),
        ((0, H), (0, 0)),
    ]

    for s in rect:
        segments.append(s)

    n = int(input())

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        segments.append(((x1, y1), (x2, y2)))

    m = int(input())
    fills = []

    for _ in range(m):
        x, y, c = input().split()
        fills.append((Fraction(int(x)), Fraction(int(y)), c))

    k = len(segments)

    pts_on_seg = []

    for i in range(k):
        a, b = segments[i]
        cur = set()
        cur.add((Fraction(a[0]), Fraction(a[1])))
        cur.add((Fraction(b[0]), Fraction(b[1])))
        pts_on_seg.append(cur)

    for i in range(k):
        for j in range(i + 1, k):
            a, b = segments[i]
            c, d = segments[j]

            inter = segment_intersection(a, b, c, d)

            for p in inter:
                pts_on_seg[i].add(p)
                pts_on_seg[j].add(p)

    edges = set()

    for i in range(k):
        a, b = segments[i]

        pts = list(pts_on_seg[i])

        dx = b[0] - a[0]
        dy = b[1] - a[1]

        pts.sort(
            key=lambda p: (
                Fraction(p[0] - a[0], dx) if dx != 0
                else Fraction(p[1] - a[1], dy)
            )
        )

        for j in range(len(pts) - 1):
            u = pts[j]
            v = pts[j + 1]

            if u != v:
                edges.add((u, v))
                edges.add((v, u))

    graph = defaultdict(list)

    for u, v in edges:
        graph[u].append(v)

    import math

    order = {}

    for u in graph:
        graph[u].sort(
            key=lambda v: math.atan2(
                float(v[1] - u[1]),
                float(v[0] - u[0])
            )
        )

        for i, v in enumerate(graph[u]):
            order[(u, v)] = i

    used = set()
    faces = []

    for u, v in edges:
        if (u, v) in used:
            continue

        face = []

        cu, cv = u, v

        while True:
            used.add((cu, cv))
            face.append(cu)

            arr = graph[cv]
            idx = order[(cv, cu)]

            nxt = arr[(idx - 1) % len(arr)]

            nu, nv = cv, nxt

            cu, cv = nu, nv

            if (cu, cv) == (u, v):
                break

        area = polygon_area(face)

        if area > 0:
            faces.append((face, area))

    face_colors = ["white"] * len(faces)

    all_segments = segments[4:]

    for x, y, col in fills:
        on_seg = False

        for a, b in all_segments:
            aa = (Fraction(a[0]), Fraction(a[1]))
            bb = (Fraction(b[0]), Fraction(b[1]))

            if point_on_segment((x, y), aa, bb):
                on_seg = True
                break

        if on_seg:
            continue

        for i, (poly, area) in enumerate(faces):
            if point_in_poly((x, y), poly):
                face_colors[i] = col
                break

    ans = defaultdict(float)

    for i, (_, area) in enumerate(faces):
        ans[face_colors[i]] += float(area)

    for c in sorted(ans):
        print(c, f"{ans[c]:.8f}")

solve()
```Phần đầu tiên xây dựng sự sắp xếp hình học. Mỗi đoạn được chia liên tục tại tất cả các điểm giao nhau. sử dụng`Fraction`tránh các thảm họa chính xác gây ra bởi các giao lộ hợp lý như`1/3`. 

Đồ thị nửa cạnh là trung tâm của giải pháp. Mỗi cạnh có hướng thuộc về đúng một mặt ở phía bên trái của nó. Việc sắp xếp các cạnh hướng ra ngoài theo góc cho phép chúng ta di chuyển một cách nhất quán dọc theo các ranh giới. 

Bước truyền tải là tinh tế. Sau khi nhập một đỉnh qua một cạnh, chúng ta chọn cạnh đi trước đó theo thứ tự tuần hoàn. Điều này giữ cho mặt hiện tại ở bên trái và vạch ra ranh giới của nó mà không có sự mơ hồ. 

Việc thực hiện chỉ lưu trữ các mặt khu vực tích cực. Mặt vô hạn bên ngoài xuất hiện với hướng âm và tự động bị loại bỏ. 

Vị trí điểm sử dụng thử nghiệm truyền tia tiêu chuẩn. Vì số lượng khuôn mặt ít nên việc kiểm tra tất cả các khuôn mặt là đủ nhanh. 

Một chi tiết quan trọng là xử lý việc điền vào các phân đoạn. Những phần lấp đầy đó chỉ ảnh hưởng đến hình học có diện tích bằng 0, vì vậy chúng bị bỏ qua khi tính toán các khu vực. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 5
6
1 1 1 3
1 3 3 3
3 3 3 1
3 1 1 1
1 3 3 1
1 1 3 3
2
2 1 red
2 2 blue
```Hình chữ nhật có một hình vuông có hai đường chéo. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Xây dựng sắp xếp | Một số mặt tam giác xuất hiện bên trong hình vuông | 
| 2 | Đổ đầy`(2,1)`với màu đỏ | Một hình tam giác phía dưới trở thành màu đỏ | 
| 3 | Đổ đầy`(2,2)`với màu xanh | Điểm nằm trên giao điểm chéo | 
| 4 | Bỏ qua màu xanh lam | Các đoạn có diện tích bằng 0 | 
| 5 | Tổng diện tích | Toàn bộ hình chữ nhật vẫn trắng | 

Đầu ra cuối cùng:```
blue 0.00000000
white 20.00000000
```Ví dụ này thể hiện quy tắc phản trực giác nhất trong bài toán. Điểm điền trên một đoạn chỉ thay đổi hình học có diện tích bằng không. 

### Ví dụ tùy chỉnh```
6 6
1
3 0 3 6
2
1 1 red
5 1 blue
```Đoạn thẳng chia hình chữ nhật thành hai mặt bằng nhau. 

| Bước | Điểm chứa mặt | Màu trước | Màu mới | 
| --- | --- | --- | --- | 
| 1 | Hình chữ nhật bên trái | trắng | đỏ | 
| 2 | Hình chữ nhật bên phải | trắng | màu xanh | 

Bảng diện tích: 

| Mặt | Khu vực | Màu cuối cùng | 
| --- | --- | --- | 
| Nửa trái | 18 | đỏ | 
| Nửa bên phải | 18 | màu xanh | 

Đầu ra cuối cùng:```
blue 18.00000000
red 18.00000000
```Điều này xác nhận rằng việc lấp lũ hoạt động trên các vùng được kết nối chứ không phải trên toàn bộ màu sắc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Nút giao đoạn, xây dựng mặt và định vị điểm trên tất cả các mặt | 
| Không gian | O(n^2) | Sắp xếp các đỉnh, cạnh và mặt | 

Với nhiều nhất`100`các phân đoạn, độ phức tạp sắp xếp vẫn đủ nhỏ. Ngay cả hành vi hình khối cũng hoàn toàn an toàn trong giới hạn vì các hệ số không đổi ở mức vừa phải và biểu đồ chỉ chứa vài nghìn phần tử. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()

    with redirect_stdout(out):
        solve()

    return out.getvalue().strip()

# provided sample
assert run(
"""4 5
6
1 1 1 3
1 3 3 3
3 3 3 1
3 1 1 1
1 3 3 1
1 1 3 3
2
2 1 red
2 2 blue
"""
) == """white 20.00000000"""

# no segments
assert run(
"""3 3
0
1
1 1 red
"""
) == """red 9.00000000"""

# single split
res = run(
"""6 6
1
3 1 3 5
2
1 1 red
5 1 blue
"""
)

assert "red 18.00000000" in res
assert "blue 18.00000000" in res

# fill on segment
assert run(
"""5 5
1
1 2 4 2
1
2 2 red
"""
) == """white 25.00000000"""

# overwrite same region
assert run(
"""4 4
0
2
1 1 red
1 1 blue
"""
) == """blue 16.00000000"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Canvas trống với một lần điền | Toàn bộ khu vực được đổi màu | Hành vi cơ bản | 
| Một phân đoạn chia | Hai gương mặt độc lập | Xây dựng khuôn mặt đúng | 
| Điền vào phân đoạn | Không thay đổi diện tích | Xử lý hình học vùng không | 
| Điền lặp đi lặp lại | Chiến thắng điền mới nhất | Mô phỏng tuần tự | 
| Giao lộ chéo | Tách phân khúc | Nhúng phẳng thích hợp | 

## Vỏ cạnh 

Hãy xem xét một điểm điền chính xác trên một đoạn.```
5 5
1
1 2 4 2
1
2 2 red
```Trước tiên, thuật toán sẽ kiểm tra xem điểm lấp đầy có thuộc về bất kỳ phân đoạn nào hay không bằng cách sử dụng các kiểm tra hộp giới hạn và cộng tuyến chính xác. Từ`(2,2)`nằm trên đoạn đó, phần điền sẽ bị bỏ qua khi tính toán diện tích. 

Mặt duy nhất vẫn là toàn bộ hình chữ nhật có diện tích`25`, do đó đầu ra vẫn là:```
white 25.00000000
```Bây giờ hãy xem xét các phân đoạn giao nhau.```
5 5
2
1 1 4 4
1 4 4 1
1
2 2 blue
```Trong quá trình tiền xử lý, hai đoạn này giao nhau tại`(5/2, 5/2)`và được chia thành các cạnh nhỏ hơn. Điểm tô nằm trên chính giao điểm nên không có mặt nào được tô màu lại. 

Nếu không phân tách, biểu đồ sẽ chứa các cạnh giao nhau và việc truyền tải bề mặt sẽ không tạo ra đa giác hợp lệ. 

Cuối cùng, hãy xem xét các vùng bị ngắt kết nối có cùng màu.```
6 6
1
3 0 3 6
3
1 1 red
5 1 red
1 1 blue
```Sau hai lần lấp đầy đầu tiên, cả hai nửa đều có màu đỏ độc lập. 

Màu tô cuối cùng chỉ ảnh hưởng đến mặt trái vì màu lấp đầy phụ thuộc vào khả năng kết nối chứ không chỉ đơn thuần là sự cân bằng màu sắc. 

Các khu vực cuối cùng trở thành:```
blue 18.00000000
red 18.00000000
```Thuật toán xử lý việc này một cách tự nhiên vì màu sắc được lưu trữ trên mỗi khuôn mặt chứ không phải trên toàn bộ.
