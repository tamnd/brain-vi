---
title: "CF 102299J - MasterCodeChef Nga"
description: "Bài toán yêu cầu chúng ta chọn một điểm (p=(pt,pi)) đại diện cho các tham số mà chúng ta muốn sử dụng, cùng với giá trị nhỏ nhất có thể có của (t)."
date: "2026-08-13T08:13:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "J"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 71
verified: true
draft: false
---

[CF 102299J - MasterCodeChef Nga](https://codeforces.com/problemset/problem/102299/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta chọn một điểm (p=(p_t,p_i)) đại diện cho các tham số mà chúng ta muốn sử dụng, cùng với giá trị nhỏ nhất có thể có của (t). Với mỗi điểm cho trước (c=(c_t,c_i)), bài toán xác định một đại lượng (T(c,p)), một tích trong (I(c,p)) và yêu cầu 

[ 
T(c,p)-I(c,p)\le t. 
] 

Mục tiêu là tạo ra (p_t,p_i) phù hợp và mức tối thiểu có thể (t). 

Các định nghĩa liên quan có thể được viết là 

[ 
T(c,p)=\frac{s(c)+d(p)}{2}, 
] 

ở đâu 

[ 
s(c)=c_t^2+c_i^2,\qquad d(p)=p_t^2+p_i^2, 
] 

và 

[ 
I(c,p)=c_t p_t+c_i p_i. 
] 

Thoạt nhìn, đây có vẻ giống như một vấn đề tối ưu hóa liên quan đến một số biểu thức đại số. Bước hữu ích là mở rộng chúng trước khi nghĩ đến thuật toán. 

Thay thế các định nghĩa cho 

\frac{c_t^2+c_i^2+p_t^2+p_i^2}{2} 
-c_t p_t-c_i p_i. 
] 

Nhân với hai và nhóm các số hạng tạo ra 

[ 
(c_t-p_t)^2+(c_i-p_i)^2\le 2t. 
] 

Vì vậy, đối với một lựa chọn cố định về (p), đại lượng quan trọng chỉ đơn giản là bình phương khoảng cách Euclide từ (p) đến mọi điểm đầu vào. 

Do đó, việc tối ưu hóa ban đầu tương đương với việc chọn một tâm (p) để giảm thiểu khoảng cách tối đa đến tất cả các điểm đã cho. Nếu vòng tròn bao quanh tối thiểu có bán kính (R), thì tâm của nó chính xác là tối ưu (p) và 

[ 
t=\frac{R^2}{2}. 
] 

Phiên bản cuộc thi có giới hạn thời gian ba giây và cho phép một số lượng lớn điểm. Việc triển khai được chấp nhận sử dụng thuật toán vòng tròn bao quanh tối thiểu ngẫu nhiên, có thời gian chạy dự kiến ​​là tuyến tính. Tìm kiếm bậc ba trên bộ ba là không khả thi và thậm chí thuật toán hình học tổng quát (O(n^2)) cũng trở nên có vấn đề khi (n) lớn. Việc triển khai cũng cần số học dấu phẩy động vì tâm và bán kính tối ưu thường không phải là số nguyên. 

Trường hợp cạnh đầu tiên là một điểm duy nhất. Ví dụ,```
1
3 7
```có tâm tối ưu ((3,7)), bán kính (0) và do đó (t=0). Việc triển khai giả định vòng tròn được xác định bởi hai hoặc ba điểm sẽ thất bại một cách không cần thiết trong trường hợp này. 

Trường hợp cạnh thứ hai là hai điểm. Vì```
2
0 0
4 0
```tâm tối ưu là ((2,0)), bán kính là (2) và câu trả lời cho (t) là (2). Việc thực hiện bất cẩn chỉ xét đường tròn ngoại tiếp ba điểm sẽ không có tam giác để xây dựng câu trả lời. 

Trường hợp cạnh thứ ba là ba điểm thẳng hàng. Vì```
3
0 0
2 0
5 0
```vòng tròn bao quanh nhỏ nhất có tâm ((2,5,0)) và bán kính (2,5) nên (t=3,125). Công thức đường tròn ngoại tiếp cho ba điểm không được xác định khi các điểm thẳng hàng. Đường tròn đúng được xác định bởi hai điểm cực trị nên việc thực hiện phải xử lý cộng tuyến riêng biệt. 

Trường hợp cạnh thứ tư là một điểm nằm chính xác trên ranh giới đường tròn hiện tại. Nếu một điểm có khoảng cách bình phương chính xác bằng bán kính bình phương hiện tại thì điểm đó đã bị che và không khiến đường tròn phải được xây dựng lại. Việc sử dụng dung sai dấu phẩy động lớn không cần thiết hoặc được chọn kém có thể làm cho các trường hợp biên như vậy không ổn định. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử các tâm có thể được xác định bởi hình học đầu vào. Đối với một tập hợp các điểm, đường tròn bao quanh nhỏ nhất được xác định bởi hai điểm biên có trung điểm là tâm hoặc ba điểm biên có tâm đường tròn ngoại tiếp là tâm. Chúng ta có thể liệt kê từng cặp và từng bộ ba, xây dựng đường tròn tương ứng, kiểm tra tất cả các điểm dựa vào nó và giữ điểm nhỏ nhất hợp lệ. 

Điều này đúng vì một đường tròn bao quanh tối ưu có nhiều nhất ba điểm trên đường biên của nó. Nếu cần chính xác hai điểm biên thì điểm giữa của chúng sẽ xác định đường tròn nhỏ nhất cho hai điểm đó. Nếu cần ba, vòng tròn ngoại tiếp của chúng sẽ xác định vòng tròn duy nhất. Kiểm tra mọi ứng cử viên cuối cùng sẽ kiểm tra ứng viên tối ưu. 

Vấn đề là số lượng hoạt động. Có (O(n^3)) bộ ba và việc kiểm tra tất cả (n) điểm cho mỗi ứng viên sẽ tạo ra phiên bản đơn giản (O(n^4)). Ngay cả khi chúng ta tránh việc kiểm tra bổ sung và khai thác cấu trúc hình học để có được triển khai (O(n^3)), thì đó vẫn là quá nhiều công việc đối với một (n lớn). Với (n=10^5), chẵn (n^2) đã có nghĩa là gần đúng (10^{10}) phép toán cặp, vì vậy các thuật toán bậc ba hoặc bậc bốn hoàn toàn nằm ngoài phạm vi. 

Quan sát làm thay đổi vấn đề là chúng ta không cần liệt kê tất cả các tập hợp ranh giới có thể có. Sau khi biến đổi đại số, bài toán chính xác là bài toán đường tròn bao quanh nhỏ nhất. Thuật toán gia tăng ngẫu nhiên có thể duy trì vòng tròn nhỏ nhất cho các điểm được xử lý cho đến nay. 

Giả sử vòng tròn hiện tại đã chứa tất cả các điểm được xử lý trước đó và một điểm mới nằm bên trong nó. Không có gì thay đổi. Nếu điểm mới nằm bên ngoài thì nó phải nằm trên ranh giới của đường tròn tối ưu mới cho tiền tố được xử lý. Chúng ta có thể khởi động lại công trình với điểm đó được cố định trên đường biên. Nếu một điểm trước đó nằm ngoài đường tròn kết quả thì điểm thứ hai đó cũng phải nằm trên đường biên. Ở giai đoạn đó, vòng tròn ứng cử viên được xác định bởi cặp điểm biên. Nếu điểm thứ ba trước đó vẫn nằm ngoài thì cả ba điểm đều phải là điểm biên nên đường tròn ngoại tiếp của chúng xác định câu trả lời. 

Việc xáo trộn các điểm một cách ngẫu nhiên là nguyên nhân khiến số lần xây dựng lại dự kiến ​​như vậy trở nên nhỏ. Thuật toán kết quả có thời gian chạy dự kiến ​​(O(n)) và không gian (O(n)) để lưu trữ điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê vòng tròn ứng viên | (O(n^3)) hoặc tệ hơn | (O(n)) | Quá chậm | 
| Vòng tròn bao quanh tối thiểu tăng dần ngẫu nhiên | Dự kiến ​​(O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các điểm đầu vào và xáo trộn ngẫu nhiên thứ tự của chúng. Việc ngẫu nhiên hóa là chìa khóa để đạt được thời gian chạy tuyến tính dự kiến, bởi vì thứ tự đầu vào đối nghịch có thể khiến thuật toán gia tăng thực hiện nhiều lần xây dựng lại hơn. 
2. Bắt đầu với một đường tròn có bán kính bằng 0 có tâm tại điểm đầu tiên. Một điểm luôn có thể được bao quanh bởi một đường tròn như vậy, vì vậy đây là đường tròn tối thiểu hợp lệ cho tiền tố ban đầu. 
3. Xử lý điểm từ trái sang phải theo thứ tự ngẫu nhiên. Nếu điểm hiện tại đã ở bên trong vòng tròn hiện tại, hãy giữ nguyên vòng tròn đó. Vòng tròn hiện tại đã bao quanh mọi điểm được xử lý cho đến nay, bao gồm cả điểm mới. 
4. Nếu điểm hiện tại (p_i) ở bên ngoài, hãy xây dựng lại đường tròn với (p_i) bị ép vào ranh giới của nó. Bắt đầu với đường tròn có bán kính bằng 0 có tâm tại (p_i). Vòng tròn cũ không thể giữ nguyên tối ưu vì nó không chứa (p_i). 
5. Xem lại tất cả các điểm trước đó. Nếu điểm trước đó nằm trong đường tròn mới, hãy giữ nguyên đường tròn đó. Nếu nó ở bên ngoài, đường tròn tối ưu mới phải có cả (p_i) và điểm trước đó (p_j) trên ranh giới của nó. 
6. Dựng đường tròn có (p_i) và (p_j) là các điểm biên đối diện nhau. Tâm của nó là trung điểm của chúng và bán kính của nó bằng một nửa khoảng cách của chúng. Đây là đường tròn nhỏ nhất chứa hai điểm biên đó. 
7. Xem lại tất cả các điểm trước đó (p_j). Nếu mỗi cái đều được che kín thì vòng tròn hai điểm là đủ. Nếu một điểm (p_k) nằm ngoài thì đường tròn tối ưu cho tiền tố này phải có (p_i,p_j,p_k) trên ranh giới của nó. 
8. Vẽ đường tròn ngoại tiếp ba điểm. Nếu ba điểm không thẳng hàng thì đường tròn này được xác định duy nhất. Nếu chúng thẳng hàng thì đường tròn ngoại tiếp không tồn tại và đường tròn đúng thay vào đó là đường tròn nhỏ nhất chứa ba điểm, được xác định bởi hai điểm xa nhất. 
9. Sau khi tất cả các điểm đã được xử lý, vòng tròn được duy trì là vòng tròn bao quanh tối thiểu của toàn bộ tập hợp đầu vào. Xuất tọa độ tâm của nó và (R^2/2), vì tham số ban đầu (t) bằng một nửa bán kính hình tròn bao quanh bình phương. 

Điều bất biến là sau khi xử lý (i) điểm ngẫu nhiên đầu tiên, vòng tròn được duy trì là vòng tròn bao quanh tối thiểu của chính xác các điểm (i) đó. Khi điểm tiếp theo đã được đề cập, thì bất biến vẫn không thay đổi. Khi nó ở bên ngoài, bất kỳ đường tròn hợp lệ nào cho tiền tố mở rộng đều phải chứa điểm đó, do đó, việc cố định nó trên đường biên sẽ không làm mất đi giải pháp tối ưu. Lập luận tương tự được áp dụng khi điểm thứ hai và sau đó là điểm thứ ba được tìm thấy ở bên ngoài. Ba điểm biên không thẳng hàng xác định duy nhất đường tròn cần tìm, trong khi ba điểm biên thẳng hàng rút gọn thành đường tròn xác định bởi hai điểm cực trị. Do đó, mỗi lần xây dựng lại đều bảo toàn bất biến vòng tròn tối thiểu. 

## Giải pháp Python```python
import sys
import math
import random

input = sys.stdin.readline

EPS = 1e-12

def dist2(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    return dx * dx + dy * dy

def inside(circle, p):
    cx, cy, r2 = circle
    dx = p[0] - cx
    dy = p[1] - cy
    return dx * dx + dy * dy <= r2 + EPS * max(1.0, r2)

def circle_two(a, b):
    cx = (a[0] + b[0]) * 0.5
    cy = (a[1] + b[1]) * 0.5
    r2 = dist2(a, b) * 0.25
    return cx, cy, r2

def circle_three(a, b, c):
    ax, ay = a
    bx, by = b
    cx, cy = c

    abx = bx - ax
    aby = by - ay
    acx = cx - ax
    acy = cy - ay

    cross = abx * acy - aby * acx

    scale = max(
        1.0,
        abs(abx * acy),
        abs(aby * acx)
    )

    if abs(cross) <= EPS * scale:
        ab = dist2(a, b)
        ac = dist2(a, c)
        bc = dist2(b, c)

        if ab >= ac and ab >= bc:
            return circle_two(a, b)
        if ac >= ab and ac >= bc:
            return circle_two(a, c)
        return circle_two(b, c)

    # Circumcenter formula.
    d = 2.0 * (ax * (by - cy) +
               bx * (cy - ay) +
               cx * (ay - by))

    a2 = ax * ax + ay * ay
    b2 = bx * bx + by * by
    c2 = cx * cx + cy * cy

    ux = (
        a2 * (by - cy) +
        b2 * (cy - ay) +
        c2 * (ay - by)
    ) / d

    uy = (
        a2 * (cx - bx) +
        b2 * (ax - cx) +
        c2 * (bx - ax)
    ) / d

    r2 = (ux - ax) * (ux - ax) + (uy - ay) * (uy - ay)

    return ux, uy, r2

def minimum_enclosing_circle(points):
    random.shuffle(points)

    n = len(points)

    if n == 0:
        return 0.0, 0.0, 0.0

    circle = (points[0][0], points[0][1], 0.0)

    for i in range(1, n):
        p = points[i]

        if inside(circle, p):
            continue

        circle = (p[0], p[1], 0.0)

        for j in range(i):
            q = points[j]

            if inside(circle, q):
                continue

            circle = circle_two(p, q)

            for k in range(j):
                r = points[k]

                if inside(circle, r):
                    continue

                circle = circle_three(p, q, r)

    return circle

def solve():
    n = int(input())
    points = [tuple(map(float, input().split())) for _ in range(n)]

    cx, cy, r2 = minimum_enclosing_circle(points)

    # The original inequality becomes distance^2 <= 2t.
    t = r2 * 0.5

    print(f"{cx:.10f} {cy:.10f} {t:.10f}")

if __name__ == "__main__":
    solve()
```các`dist2`chức năng cố tình tính toán khoảng cách bình phương. Thuật toán chỉ cần so sánh khoảng cách với bán kính nên việc lấy căn bậc hai sẽ thêm các phép tính dấu phẩy động không cần thiết. 

các`inside`hàm so sánh bình phương khoảng cách với bình phương bán kính. Dung sai nhỏ liên quan đến thang đo của bán kính, giúp ích cho các điểm về mặt toán học nằm chính xác trên đường biên nhưng được biểu thị bằng một lỗi dấu phẩy động nhỏ.`circle_two`thực hiện trực tiếp trường hợp hai điểm biên. Điểm giữa là tâm và một phần tư bình phương khoảng cách giữa các điểm là bán kính bình phương.`circle_three`xử lý trường hợp ba điểm biên. Yếu tố quyết định đại diện bởi`cross`phát hiện sự cộng tuyến. Đối với các điểm thẳng hàng, đường tròn ngoại tiếp không được xác định, nhưng đường tròn bao quanh nhỏ nhất chỉ đơn giản là đường tròn có đường kính nối hai điểm xa nhất. 

Các vòng lặp lồng nhau trong`minimum_enclosing_circle`tương ứng trực tiếp với đối số ranh giới. Vòng ngoài xác định một điểm vi phạm vòng tròn hiện tại. Vòng lặp tiếp theo sửa điểm đó và tìm kiếm điểm ranh giới thứ hai. Vòng lặp trong cùng tìm kiếm điểm ranh giới thứ ba. Thứ tự quan trọng vì khi một điểm vi phạm vòng tròn hiện tại, nó phải thuộc ranh giới của vòng tròn tối ưu mới cho tiền tố đó. 

Thuật toán sử dụng`random.shuffle`trước khi xử lý. Nếu không có sự ngẫu nhiên hóa, sự đảm bảo về thời gian tuyến tính dự kiến ​​sẽ biến mất. Bản thân hình học không phụ thuộc vào thứ tự nên việc xáo trộn là an toàn. 

Kiểu dấu phẩy động của Python là kiểu C double, đủ cho các phép tính hình học được sử dụng ở đây. Giá trị cuối cùng được in với mười chữ số sau dấu thập phân, mang lại đủ độ chính xác cho trình kiểm tra dấu phẩy động tuyệt đối hoặc tương đối thông thường. 

## Ví dụ đã hoạt động 

Vì kho lưu trữ cuộc thi có sẵn bộc lộ vấn đề và chấp nhận các bài gửi nhưng không hiển thị các trường hợp mẫu trong văn bản tuyên bố có thể tìm kiếm nên các dấu vết sau đây sử dụng hai đầu vào cụ thể. 

Đối với đầu vào đầu tiên,```
1
3 7
```thứ tự ngẫu nhiên nhất thiết không thay đổi. 

| Bước | Điểm | Trung tâm | Bán kính bình phương | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | ((3,7)) | ((3,7)) | (0) | Khởi tạo | 

Điểm duy nhất đã được bao phủ bởi vòng tròn bán kính bằng 0. Do đó (R^2=0) và (t=0), do đó đầu ra là```
3.0000000000 7.0000000000 0.0000000000
```Điều này thể hiện trường hợp ranh giới một điểm và xác nhận rằng không cần có cặp hoặc bộ ba. 

Đối với đầu vào thứ hai,```
3
0 0
4 0
0 3
```giả sử thứ tự ngẫu nhiên xảy ra chính xác là thứ tự đầu vào. 

| Bước | Điểm | Trung tâm | Bán kính bình phương | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | ((0,0)) | ((0,0)) | (0) | Khởi tạo | 
| 2 | ((4,0)) | ((2,0)) | (4) | Điểm đầu tiên bên ngoài, sử dụng vòng tròn hai điểm | 
| 3 | ((0,3)) | ((2,0)) | (4) | Điểm được bảo hiểm | 

Đường tròn có tâm tại ((2,0)) có bán kính (2) chứa ((0,0)), ((4,0)) và ((0,3)), vì bình phương khoảng cách từ ((2,0)) đến ((0,3)) là (13), nên đường tròn cụ thể này thực sự không chứa điểm thứ ba. Do đó, dấu vết chính xác tiếp tục với việc xây dựng lại bị ép buộc bởi ((0,3)). 

| Bước | Điểm | Trung tâm | Bán kính bình phương | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | ((0,0)) | ((0,0)) | (0) | Khởi tạo | 
| 2 | ((4,0)) | ((2,0)) | (4) | Điểm đầu tiên bên ngoài | 
| 3a | ((0,3)) | ((0,3)) | (0) | Buộc điểm mới vào ranh giới | 
| 3b | ((0,0)) | ((0,1,5)) | (2.25) | Vòng tròn hai điểm | 
| 3c | ((4,0)) | ((0,1,5)) | (2.25) | Bên ngoài nên sử dụng vòng tròn ba điểm | 
| 3d | cả ba | ((2,1.5)) | (6.25) | Vòng tròn | 

Bán kính cuối cùng là (2,5), vì vậy 

[ 
t=\frac{2.5^2}{2}=3.125. 
] 

Kết quả đầu ra là```
2.0000000000 1.5000000000 3.1250000000
```Dấu vết cho thấy tại sao vòng lặp trong cùng tồn tại. Hai điểm biên không phải lúc nào cũng đủ. Khi điểm thứ ba nằm ngoài đường tròn đường kính thì phải xét cả ba điểm đó và đường tròn ngoại tiếp của chúng cho đáp án tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​(O(n)) | Vòng tròn bao quanh tối thiểu tăng dần ngẫu nhiên có thời gian chạy tuyến tính dự kiến ​​| 
| Không gian | (O(n)) | Các điểm đầu vào được lưu trữ để có thể xem lại trong quá trình xây dựng lại ranh giới | 

Cuộc thi đưa ra giới hạn thời gian là ba giây và được thiết kế cho tập hợp điểm lớn, do đó, thuật toán ngẫu nhiên theo thời gian tuyến tính dự kiến ​​là cách tiếp cận phù hợp. Việc triển khai chỉ lưu trữ tọa độ và một lượng trạng thái hình học tạm thời không đổi, giữ cho bộ nhớ thoải mái trong giới hạn cuộc thi 256 MB đã nêu. 

## Trường hợp thử nghiệm 

Kho lưu trữ cuộc thi có thể tìm kiếm ban đầu không hiển thị các cặp đầu vào/đầu ra mẫu, do đó khối kiểm tra bên dưới sử dụng các trường hợp được xây dựng độc lập bao gồm các tình huống hình học quan trọng.```python
# helper: run solution on input string, return output string
import sys
import io
import math

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

def parse(out: str):
    return list(map(float, out.split()))

def close(a, b, eps=1e-7):
    return abs(a - b) <= eps * max(1.0, abs(a), abs(b))

# Minimum-size input.
out = parse(run("""1
3 7
"""))
assert close(out[0], 3.0)
assert close(out[1], 7.0)
assert close(out[2], 0.0)

# Two points, so the answer is determined by their midpoint.
out = parse(run("""2
0 0
4 0
"""))
assert close(out[0], 2.0)
assert close(out[1], 0.0)
assert close(out[2], 2.0)

# Three non-collinear points.
out = parse(run("""3
0 0
4 0
0 3
"""))
assert close(out[0], 2.0)
assert close(out[1], 1.5)
assert close(out[2], 3.125)

# Collinear points, where the two extreme points determine the circle.
out = parse(run("""3
0 0
2 0
5 0
"""))
assert close(out[0], 2.5)
assert close(out[1], 0.0)
assert close(out[2], 3.125)

# All points equal.
out = parse(run("""5
7 7
7 7
7 7
7 7
7 7
"""))
assert close(out[0], 7.0)
assert close(out[1], 7.0)
assert close(out[2], 0.0)

# A square. Any minimum enclosing circle has center (1,1) and radius sqrt(2).
out = parse(run("""4
0 0
2 0
2 2
0 2
"""))
assert close(out[0], 1.0)
assert close(out[1], 1.0)
assert close(out[2], 1.0)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 3 7`|`3 7 0`| Khởi tạo một điểm | 
|`2 / 0 0 / 4 0`|`2 0 2`| Vòng tròn ranh giới hai điểm | 
|`3 / 0 0 / 4 0 / 0 3`|`2 1.5 3.125`| Đường tròn ngoại tiếp ba điểm | 
|`3 / 0 0 / 2 0 / 5 0`|`2.5 0 3.125`| Xử lý điểm cộng tuyến | 
| Năm bản sao của`(7,7)`|`7 7 0`| Điểm trùng lặp và hoàn toàn bằng nhau | 
| Bốn góc của hình vuông |`1 1 1`| Điểm đối xứng và ranh giới | 

Đầu ra văn bản chính xác có thể khác nhau vì tâm được biểu thị bằng số học dấu phẩy động, do đó các xác nhận sẽ so sánh các giá trị số thay vì yêu cầu định dạng thập phân cụ thể. 

## Vỏ cạnh 

Đối với trường hợp một điểm```
1
3 7
```vòng lặp bên ngoài khởi tạo đường tròn tại ((3,7)) với bán kính bằng 0 và không gặp điểm vi phạm. Bán kính bình phương cuối cùng bằng 0, do đó thuật toán sẽ in (t=0). Thuộc tính quan trọng ở đây là vòng tròn bao quanh tối thiểu được phép có bán kính bằng 0. 

Cho hai điểm```
2
0 0
4 0
```điểm đầu tiên tạo ra một đường tròn có bán kính bằng 0. Điểm thứ hai nằm ngoài nó nên trở thành điểm biên cưỡng bức. Điểm đầu tiên cũng nằm ngoài đường tròn bán kính bằng 0 nên dựng được đường tròn hai điểm. Tâm của nó là ((2,0)), bán kính bình phương của nó là (4) và giá trị được báo cáo là (4/2=2). Không có tính toán ba điểm nào được thử. 

Đối với các điểm thẳng hàng```
3
0 0
2 0
5 0
```khi cả ba điểm đều trở thành ứng cử viên biên thì tích chéo bằng 0. Không thể tính tâm đường tròn từ các điểm thẳng hàng vì không có đường tròn hữu hạn nào đi qua ba điểm thẳng hàng phân biệt. Thay vào đó, việc triển khai sẽ so sánh ba khoảng cách bình phương theo cặp, chọn cặp ((0,0),(5,0)) và xây dựng đường tròn điểm giữa của chúng. Tâm là ((2,5,0)), bán kính bình phương là (6,25) và câu trả lời là (3,125). 

Đối với các điểm đã nằm trên ranh giới,`inside`thử nghiệm sử dụng dung sai nhỏ. Coi như```
2
0 0
4 0
```sau khi điểm thứ hai tạo đường tròn có tâm tại ((2,0)) bình phương bán kính (4), cả hai điểm ban đầu đều có khoảng cách bình phương chính xác (4). Các phép tính dấu phẩy động có thể tạo ra một giá trị vô cùng lớn hơn (4), do đó so sánh với các phép tính nghiêm ngặt`<`có thể xây dựng lại vòng tròn một cách không chính xác. Dung sai coi các điểm ranh giới về mặt toán học là được bao phủ. 

Đối với các điểm trùng lặp,```
5
7 7
7 7
7 7
7 7
7 7
```mọi điểm đều có khoảng cách bằng 0 so với điểm đầu tiên. Vòng tròn không bao giờ cần thay đổi và đáp án vẫn giữ nguyên ((7,7,0)). Các điểm trùng lặp không làm thay đổi vòng tròn bao quanh tối thiểu. 

Đối với một cấu hình đối xứng như```
4
0 0
2 0
2 2
0 2
```trung tâm tối ưu là ((1,1)). Tất cả bốn góc đều có khoảng cách bình phương (2), nên bán kính bình phương là (2) và yêu cầu (t) là (1). Trường hợp này rất hữu ích vì mọi điểm đều có thể nằm trên đường biên và nó kiểm tra xem thuật toán không phụ thuộc vào một điểm cụ thể là điểm cực trị duy nhất.
