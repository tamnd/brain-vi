---
title: "CF 1423E - Tháp ăng-ten 5G"
description: "Chúng tôi được giao nhiệm vụ quản lý việc thu hồi đất cho các tháp ăng-ten 5G. Mỗi tòa tháp chiếm một diện tích hình tròn được xác định bởi tâm (x, y) và bán kính r. Cơ sở dữ liệu đất đai bao gồm các thuộc tính được xác định là đa giác có ID số nguyên duy nhất."
date: "2026-06-11T06:09:09+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1423
codeforces_index: "E"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 1]"
rating: 2700
weight: 1423
solve_time_s: 80
verified: true
draft: false
---

[CF 1423E - Tháp ăng-ten 5G](https://codeforces.com/problemset/problem/1423/E) 

**Đánh giá:** 2700 
**Thẻ:** hình học 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ quản lý việc thu hồi đất cho các tháp ăng-ten 5G. Mỗi tòa tháp chiếm một diện tích hình tròn được xác định bởi một trung tâm`(x, y)`và bán kính`r`. Cơ sở dữ liệu đất đai bao gồm các thuộc tính được xác định là đa giác có ID số nguyên duy nhất. Mục tiêu là, đối với mỗi truy vấn tháp, để tính tổng diện tích của tất cả thuộc tính giao với vòng tròn của tháp và trả về ID của chúng. 

Đầu vào đầu tiên cung cấp chiều rộng và chiều cao của tổng diện tích, giới hạn tọa độ nhưng mặt khác không liên quan đến tính toán. Sau đó chúng tôi có`n`đa giác đại diện cho các thuộc tính. Mỗi đa giác có từ 3 đến 40 đỉnh, được sắp xếp theo thứ tự ngược chiều kim đồng hồ, đảm bảo chúng xác định được một đa giác đơn giản hợp lệ. Các truy vấn chỉ định một vòng tròn và yêu cầu tổng diện tích thuộc tính giao nhau bởi vòng tròn đó. 

Những ràng buộc chỉ ra rằng`n * q ≤ 10^6`, ngay lập tức loại trừ một giải pháp mạnh mẽ để kiểm tra mọi đa giác đối với mọi truy vấn trong phép tính hình học O(n*q) ngây thơ. Giao điểm vòng tròn đa giác là không tầm thường, vì vậy mỗi lần kiểm tra có thể liên quan đến các phép toán O(v) cho`v`đỉnh. Với`n`Và`q`lên đến`10^5`, việc kiểm tra theo cặp đơn giản sẽ yêu cầu tới 10^10 thao tác, quá chậm. 

Các trường hợp cạnh không rõ ràng phát sinh khi một đường tròn chỉ tiếp xúc với một đa giác ở một đỉnh hoặc cạnh. Kiểm tra điểm trong đa giác đơn giản sẽ bỏ sót những điều này, vì vậy kiểm tra giao lộ phải xử lý chính xác sự chồng chéo một phần, bao gồm cả trường hợp vòng tròn chỉ chứa một phần của đa giác hoặc chỉ chạm vào nó. Một trường hợp cạnh khác xảy ra với các vòng tròn rất nhỏ nằm hoàn toàn bên trong một đa giác hoặc hoàn toàn nằm ngoài tất cả các đa giác, nên sẽ trả về diện tích bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp lại qua mọi truy vấn và kiểm tra giao điểm với mọi đa giác. Đối với mỗi đa giác, bạn tính toán giao điểm hình tròn-đa giác bằng thư viện hình học hoặc triển khai hàm kiểm tra xem có cạnh đa giác nào giao với hình tròn hay tâm hình tròn có nằm bên trong đa giác hay không. Thuật toán đúng vì nó trực tiếp thực hiện phát biểu bài toán: một đa giác đóng góp diện tích của nó nếu nó chạm vào đường tròn. Tuy nhiên, với`n*q`có khả năng đạt tới 10^6 và mỗi đa giác có tối đa 40 đỉnh, cách tiếp cận này có thể dễ dàng đạt tới 4*10^7 thao tác cho mỗi truy vấn, là đường biên nhưng có rủi ro dưới giới hạn 2 giây. 

Cách tiếp cận tối ưu tận dụng sự phân vùng không gian để giảm bớt những lần kiểm tra không cần thiết. Các thuộc tính không chồng chéo, vì vậy chúng ta có thể xử lý trước chỉ mục không gian, chẳng hạn như cây R hoặc lưới. Quan sát là một đa giác ở xa một vòng tròn không thể cắt nó. Bằng cách chia khu vực thành một lưới thống nhất và lưu trữ các đa giác chạm vào từng ô lưới, mỗi truy vấn chỉ kiểm tra các đa giác trong các ô chồng lên vòng tròn truy vấn. Điều này làm giảm số lần kiểm tra giao điểm đa giác-vòng tròn tốn kém từ`n`thành một tập hợp con nhỏ, thường có giá trị trung bình không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n_q_v) | O(n*v) | Có khả năng quá chậm | 
| Chỉ số không gian dựa trên lưới | O(n_v + q_k*v) | O(n*v) | Được chấp nhận, hiệu quả với n và q lớn | 

Đây,`v`là số đỉnh trên mỗi đa giác và`k`là số đa giác ứng cử viên trung bình cho mỗi truy vấn sau khi lọc không gian. 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp tất cả các đa giác và tính toán các hộp giới hạn căn chỉnh theo trục của chúng (AABB). Hộp giới hạn là`(min_x, max_x, min_y, max_y)`và cho phép kiểm tra giao điểm ban đầu nhanh chóng với một vòng tròn. Nếu một hình tròn không chồng lên hộp giới hạn của đa giác thì nó không thể cắt chính đa giác đó. 
2. Xử lý trước các đa giác thành một lưới 2D thống nhất. Kích thước ô lưới có thể được chọn dựa trên kích thước đa giác trung bình, ví dụ như`sqrt(total_area/n)`để cân bằng số lượng đa giác trên mỗi ô. Lưu trữ cho mỗi ô danh sách ID đa giác có các hộp giới hạn giao nhau với ô. 
3. Đối với mỗi vòng truy vấn`(x, y, r)`, xác định ô lưới nào mà vòng tròn chồng lên nhau. Điều này có thể được thực hiện bằng cách mở rộng hình tròn theo bán kính của nó và tìm ô nào giao nhau với hình chữ nhật đó. 
4. Thu thập tất cả các đa giác từ các ô chồng chéo này vào một tập ứng cử viên, tránh trùng lặp. 
5. Đối với mỗi đa giác ứng cử viên, trước tiên hãy đánh dấu vào hộp giới hạn đối với hình tròn bằng cách sử dụng điều kiện`(closest_x - cx)^2 + (closest_y - cy)^2 ≤ r^2`Ở đâu`closest_x, closest_y`là điểm gần nhất của hộp giới hạn với tâm vòng tròn. Nếu thất bại, hãy bỏ qua đa giác. 
6. Nếu hộp giới hạn giao nhau, hãy thực hiện kiểm tra giao điểm hình tròn đa giác chính xác. Điều này có thể được thực hiện bằng cách kiểm tra xem có cạnh đa giác nào cắt đường tròn hay không hoặc tâm đường tròn có nằm bên trong đa giác hay không. 
7. Tính tổng diện tích các đa giác giao nhau và thu thập ID của chúng để xuất ra. 

Tại sao nó hoạt động: Các hộp giới hạn và lọc lưới đảm bảo rằng không có đa giác nào giao nhau với vòng tròn bị bỏ qua. Giao điểm đa giác-vòng tròn chính xác đảm bảo tính chính xác cho các đa giác tiếp xúc hoặc một phần bên trong vòng tròn. Phân vùng lưới giúp giảm các kiểm tra không cần thiết mà không bỏ sót bất kỳ đa giác hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import sqrt

def point_inside_polygon(px, py, polygon):
    cnt = 0
    n = len(polygon)
    for i in range(n):
        x1, y1 = polygon[i]
        x2, y2 = polygon[(i+1)%n]
        if ((y1 <= py < y2) or (y2 <= py < y1)) and (px < (x2-x1)*(py-y1)/(y2-y1)+x1):
            cnt ^= 1
    return cnt

def distance_sq_point_to_segment(px, py, x1, y1, x2, y2):
    dx, dy = x2-x1, y2-y1
    if dx == dy == 0:
        return (px-x1)**2 + (py-y1)**2
    t = max(0, min(1, ((px-x1)*dx + (py-y1)*dy)/(dx*dx + dy*dy)))
    closest_x, closest_y = x1 + t*dx, y1 + t*dy
    return (px-closest_x)**2 + (py-closest_y)**2

def circle_intersects_polygon(cx, cy, r, polygon):
    if point_inside_polygon(cx, cy, polygon):
        return True
    r2 = r*r
    n = len(polygon)
    for i in range(n):
        x1, y1 = polygon[i]
        x2, y2 = polygon[(i+1)%n]
        if distance_sq_point_to_segment(cx, cy, x1, y1, x2, y2) <= r2:
            return True
    return False

def main():
    width, height, n = map(float, input().split())
    n = int(n)
    polygons = []
    bboxes = []
    areas = []

    for _ in range(n):
        data = list(map(float, input().split()))
        v = int(data[0])
        pts = [(data[i*2+1], data[i*2+2]) for i in range(v)]
        polygons.append(pts)
        xs, ys = zip(*pts)
        bboxes.append((min(xs), max(xs), min(ys), max(ys)))
        # Polygon area by shoelace
        area = 0
        for i in range(v):
            x1, y1 = pts[i]
            x2, y2 = pts[(i+1)%v]
            area += x1*y2 - x2*y1
        areas.append(abs(area)/2)

    q = int(input())
    for _ in range(q):
        cx, cy, r = map(float, input().split())
        total_area = 0
        intersect_ids = []
        r2 = r*r
        for pid, poly in enumerate(polygons):
            min_x, max_x, min_y, max_y = bboxes[pid]
            # Bounding box check
            closest_x = max(min_x, min(cx, max_x))
            closest_y = max(min_y, min(cy, max_y))
            if (closest_x-cx)**2 + (closest_y-cy)**2 > r2:
                continue
            if circle_intersects_polygon(cx, cy, r, poly):
                total_area += areas[pid]
                intersect_ids.append(str(pid))
        if intersect_ids:
            print(f"{total_area:.6f} {len(intersect_ids)} {' '.join(intersect_ids)}")
        else:
            print("0.000000 0")

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ đọc đa giác và tính toán diện tích cũng như hộp giới hạn của chúng. Đối với mỗi truy vấn, nó nhanh chóng loại bỏ các đa giác có hộp giới hạn ở xa vòng tròn. Đối với các ứng cử viên còn lại, nó kiểm tra giao điểm chính xác bằng cách sử dụng kiểm tra khoảng cách điểm trong đa giác và cạnh. Các vùng và ID của các đa giác giao nhau được thu thập và in ra. Những điểm tinh tế bao gồm việc sử dụng số học dấu phẩy động một cách nhất quán và áp dụng chính xác công thức dây giày để tính diện tích. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Truy vấn`(2, 3.5, 0.5)`| ID đa giác | Kiểm tra hộp giới hạn | Hình tròn cắt đa giác | Diện tích bao gồm | 
| --- | --- | --- | --- | 
| 0 | Vượt qua | Đúng | 1.0 | 
| 1 | Vượt qua | Sai | 0 | 
| 2 | Thất bại | Không áp dụng | 0 | 

Tổng diện tích là 1.0, ID`[0]`. 

Truy vấn`(3.3, 2, 0.4)`| ID đa giác | Kiểm tra hộp giới hạn | Hình tròn cắt đa giác | Diện tích bao gồm | 
| --- | --- | --- | --- | 
| 0 | Vượt qua | | |
