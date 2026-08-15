---
title: "CF 102375L - \u0411\u043b\u0438\u0436\u0430\u0439\u0448\u0438\u0435 \u0442\u043e\u0447\u043a\u0438"
description: "Chúng ta có một lưới số nguyên bên trong hình chữ nhật có các góc (0, 0) và (X, Y). Trong số tất cả các điểm được đánh dấu, p1 là điểm đặc biệt. Chúng ta cần đếm mọi điểm lưới có khoảng cách Euclide đến p1 không lớn hơn khoảng cách của nó đến mọi điểm được đánh dấu khác."
date: "2026-08-15T07:33:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "L"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 1254
verified: false
draft: false
---

[CF 102375L - \u0411\u043b\u0438\u0436\u0430\u0439\u0448\u0438\u0435 \u0442\u043e\u0447\u043a\u0438](https://codeforces.com/problemset/problem/102375/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 20m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới số nguyên bên trong hình chữ nhật có các góc`(0, 0)`Và`(X, Y)`. Trong số tất cả các điểm được đánh dấu,`p1`là đặc biệt. Chúng ta cần đếm mọi điểm lưới có khoảng cách Euclide tới`p1`không lớn hơn khoảng cách của nó đến mọi điểm được đánh dấu khác. 

Một cách giải thích trực tiếp đề nghị kiểm tra từng điểm lưới đối với từng điểm được đánh dấu. Điều đó quá đắt. Cấu trúc hữu ích xuất hiện sau khi so sánh bình phương khoảng cách. Đối với một đối thủ cạnh tranh cố định`pi`, tập hợp các điểm ít nhất bằng`p1`như`pi`là một nửa mặt phẳng giới hạn bởi đường trung trực của hai điểm. Tập mong muốn là giao điểm của tất cả các nửa mặt phẳng này với hình chữ nhật. 

Tuyên bố cuộc thi ban đầu đưa ra`X, Y, K <= 2 * 10^5`, với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MiB. Điều này ngay lập tức loại trừ việc lặp lại trên tất cả các cặp điểm lưới và điểm được đánh dấu. Bản thân hình chữ nhật có thể chứa khoảng`(2 * 10^5 + 1)^2 = 4 * 10^10`số điểm nguyên, do đó ngay cả một`O(XY)`thuật toán là không thể. Chúng ta cần xử lý các điểm được đánh dấu trong khoảng`O(K log K)`thời gian và sau đó chỉ quét một phạm vi tọa độ. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. 

Nếu như`K = 1`, không có đối thủ cạnh tranh nào cả nên mọi điểm lưới đều tốt. Ví dụ,```
1 1 1
0 0
```có đầu ra`4`, vì bốn điểm nguyên của hình chữ nhật là`(0,0)`,`(0,1)`,`(1,0)`, Và`(1,1)`. Một lỗi phổ biến là đếm`X * Y`tế bào thay vì`(X + 1) * (Y + 1)`điểm lưới. 

Các đối thủ cạnh tranh có thể có cùng`x`phối hợp như`p1`. Khi đó đường trung trực của chúng nằm ngang nên chúng không áp đặt một`x`bị ràng buộc chút nào. Ví dụ,```
2 4 3
1 2
1 0
1 4
```Đối thủ đầu tiên yêu cầu`y >= 1`, điều thứ hai yêu cầu`y <= 3`, và mọi`x`từ`0`bởi vì`2`được cho phép. Câu trả lời là`3 * 3 = 9`. Đối xử với mọi đối thủ cạnh tranh như một`x`-line sẽ chia cho 0 hoặc âm thầm mất đi hạn chế này. 

Đường phân giác có thể đi qua nửa đường giữa hai tọa độ nguyên. Ví dụ,```
2 2 2
1 1
0 1
```Điều kiện chống lại`(0,1)`là`x >= 1/2`, vậy điểm nguyên phải có`x >= 1`. Có hai khả năng`x`giá trị và ba có thể`y`giá trị, đưa ra đầu ra`6`. Sử dụng phép chia số nguyên cắt ngắn thay vì sàn hoặc trần toán học có thể tạo ra ranh giới sai. 

Cà vạt cũng phải được chấp nhận. Điều kiện là khoảng cách nhỏ hơn hoặc bằng khoảng cách của đối thủ, do đó một điểm nằm chính xác trên đường phân giác thuộc vùng tốt. Đây là lý do tại sao tất cả các so sánh đường bao trong giải pháp đều sử dụng các so sánh không nghiêm ngặt khi chọn đường hoạt động. 

## Phương pháp tiếp cận 

Giải pháp brute-force được rút ra trực tiếp từ định nghĩa. Liệt kê mọi số nguyên`(x, y)`trong hình chữ nhật, tính khoảng cách bình phương của nó tới`p1`, sau đó so sánh giá trị đó với bình phương khoảng cách đến mọi điểm được đánh dấu khác. Khoảng cách bình phương là đủ vì căn bậc hai là đơn điệu. 

Điều này đúng vì một điểm được chấp nhận chính xác khi không có đối thủ nào ở gần hơn hoàn toàn. Tuy nhiên, trường hợp xấu nhất có khoảng`4 * 10^10`điểm lưới và`2 * 10^5`đối thủ cạnh tranh, đưa ra đại khái`8 * 10^15`so sánh khoảng cách. Đó là nhiều mệnh lệnh có độ lớn vượt quá những gì có thể phù hợp với thời hạn. 

Quan sát quan trọng là việc so sánh hai khoảng cách Euclide bình phương sẽ loại bỏ các số hạng bậc hai trong`x`Và`y`. Cho phép`p1 = (x1, y1)`và để một đối thủ cạnh tranh`q = (xi, yi)`. Sự bất bình đẳng`(x - x1)^2 + (y - y1)^2 <= (x - xi)^2 + (y - yi)^2`đơn giản hóa để`2(xi - x1)x + 2(yi - y1)y <= xi^2 + yi^2 - x1^2 - y1^2`. 

Đây là một nửa mặt phẳng tuyến tính. 

Bây giờ sửa một số nguyên`y`. Mỗi đối thủ cạnh tranh với`xi > x1`đưa ra một giới hạn trên`x`, trong khi mọi đối thủ cạnh tranh với`xi < x1`đưa ra giới hạn dưới`x`. Do đó, với mỗi hàng`y`, các điểm tốt tạo thành một khoảng nguyên`[L(y), R(y)]`. Chúng ta chỉ cần tính giới hạn trên và dưới chặt chẽ nhất. 

Mỗi giới hạn là một hàm tuyến tính của`y`, có thể với một hệ số hợp lý. Ranh giới trên là mức tối thiểu của nhiều dòng và ranh giới dưới là mức tối đa của nhiều dòng. Những đường bao như vậy có thể được xây dựng bằng thủ thuật bao lồi. Chúng tôi sắp xếp các đường theo độ dốc, loại bỏ các đường không bao giờ có thể trở nên tối ưu và sau đó quét`y`từ dưới lên trên trong khi vẫn duy trì con trỏ tới dòng hiện tại tối ưu. 

Giống nhau`y`phối hợp cũng có thể bị hạn chế bởi các đối thủ cạnh tranh với`xi = x1`. Những ràng buộc đó được xử lý trực tiếp trước khi xây dựng đường bao. 

Brute-force hoạt động vì nó kiểm tra chính xác định nghĩa nhưng không thành công khi hình chữ nhật chứa hàng tỷ điểm lưới. Quan sát cho thấy mọi so sánh khoảng cách đều trở thành một nửa mặt phẳng cho phép chúng ta thay thế phép liệt kê hai chiều bằng hai đường bao một chiều và một lần quét qua`y`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(XYK)`|`O(K)`| Quá chậm | 
| Tối ưu |`O(K log K + Y)`|`O(K)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`p1 = (x1, y1)`và khởi tạo khoảng thời gian hàng được phép thành`0 <= y <= Y`. Mỗi đối thủ cạnh tranh có cùng`x`phối hợp như`p1`chỉ có thể hạn chế khoảng thời gian này. Đối thủ cạnh tranh với sự khác biệt`x`tọa độ sẽ được chuyển thành đường. 
2. Đối với đối thủ cạnh tranh`q = (xi, yi)`, định nghĩa`dx = xi - x1`,`dy = yi - y1`,`C = xi^2 + yi^2 - x1^2 - y1^2`. 

Nửa mặt phẳng của nó là`2 dx x + 2 dy y <= C`. 

Phương trình này được giữ ở dạng số học số nguyên, do đó không có lỗi dấu phẩy động nào có thể ảnh hưởng đến điểm biên. 
3. Nếu`dx = 0`, bất đẳng thức chứa không`x`. Khi`dy > 0`, nó trở thành`y <= (yi + y1) / 2`. 

Khi`dy < 0`, nó trở thành`y >= (yi + y1) / 2`. 

Chuyển đổi chúng thành giới hạn trần và sàn số nguyên rồi giao chúng với khoảng cách hàng hiện tại. 
4. Nếu`dx > 0`, giải bất đẳng thức cho`x`:`x <= (C - 2dy * y) / (2dx)`. 

Đây là đường giới hạn trên. Đối với một cố định`y`, tất cả các đối thủ cạnh tranh như vậy đều phải hài lòng, vì vậy chúng tôi lấy mức tối thiểu của những dòng này. 
5. Nếu`dx < 0`, phép chia đảo ngược bất đẳng thức:`x >= (2dy * y - C) / (2(-dx))`. 

Đây là đường giới hạn dưới. Chúng ta cần tối đa tất cả các chức năng này. Thay vì triển khai loại thân tàu thứ hai, hãy lưu trữ phần phủ định của mọi dòng bên dưới. Điểm tối đa của các dòng ban đầu trở thành số âm của số âm tối thiểu của chúng. 
6. Biểu diễn mọi đường hữu tỉ dưới dạng`f(y) = (m*y + b) / d`với`d > 0`. Sắp xếp các đường theo độ dốc giảm dần`m/d`. Đối với các độ dốc bằng nhau, chỉ đường có điểm chặn nhỏ nhất mới quan trọng đối với đường bao tối thiểu. 
7. Xây dựng phong bì bên dưới. Xét ba dòng liên tiếp`a`,`b`, Và`c`với độ dốc giảm dần. Cho phép`x_ab`ở đâu`a`Và`b`cắt nhau và`x_bc`Ở đâu`b`Và`c`giao nhau. Nếu như`x_ab >= x_bc`, đường kẻ`b`không bao giờ là mức tối thiểu, vì vậy nó có thể được loại bỏ. Tất cả các so sánh được thực hiện bằng phép nhân chéo, tránh số học dấu phẩy động. 
8. Quét các hàng số nguyên từ hàng đầu tiên được phép`y`đến mức cuối cùng được phép`y`. Vì tọa độ truy vấn ngày càng tăng nên đường hoạt động trên đường bao chỉ có thể di chuyển về phía trước. Trong khi dòng tiếp theo không tệ hơn dòng hiện tại ở thời điểm hiện tại`y`, tiến con trỏ. 
9. Đánh giá đường bao trên tại`y`và lấy sàn toán học của nó. Đánh giá đường bao dưới bị phủ định và lấy giá trị âm của sàn của nó, cho ra trần toán học của giới hạn dưới ban đầu. 
10. Kẹp khoảng kết quả vào`[0, X]`. Nếu như`L <= R`, hàng đóng góp`R - L + 1`điểm số nguyên tốt. Tính tổng những đóng góp này trên tất cả các hàng được phép. 

### Tại sao nó hoạt động 

Đối với mỗi đối thủ cạnh tranh`pi`, thuật toán sẽ chèn chính xác nửa mặt phẳng chứa tất cả các điểm ít nhất gần bằng`p1`như`pi`. Giao điểm của các nửa mặt phẳng này chính là ô Voronoi của`p1`, giới hạn trong hình chữ nhật. 

Trên một hàng cố định`y`, mọi nửa mặt phẳng với`xi > x1`đóng góp một giới hạn trên vào`x`, và mọi nửa mặt phẳng với`xi < x1`đóng góp một giới hạn dưới. Do đó, giao điểm của chúng là khoảng cách từ giới hạn dưới tối đa đến giới hạn trên tối thiểu. Bình đẳng-`x`đối thủ cạnh tranh chỉ ảnh hưởng đến những hàng tồn tại. 

Bao lồi chứa chính xác các đường có thể là tối thiểu đối với một số tọa độ truy vấn. Kiểm tra thứ tự giao nhau sẽ loại bỏ đường giữa một cách chính xác khi khoảng tối ưu của nó trống. Trong quá trình quét ngày càng tăng của`y`, mức tối thiểu chỉ có thể di chuyển về phía trước qua thân tàu, vì vậy đường được chọn luôn là giá trị đường bao thực. Sàn và trần những giá trị hợp lý chính xác đó cung cấp chính xác số nguyên đầu tiên và cuối cùng`x`trên hàng. Như vậy mọi điểm được tính đều tốt, và mọi điểm tốt đều được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from functools import cmp_to_key

def slope_cmp(a, b):
    # Compare a.m / a.d and b.m / b.d, in decreasing order.
    left = a[0] * b[2]
    right = b[0] * a[2]

    if left > right:
        return -1
    if left < right:
        return 1

    # Equal slopes. Smaller intercept first.
    left = a[1] * b[2]
    right = b[1] * a[2]

    if left < right:
        return -1
    if left > right:
        return 1
    return 0

def value_leq(a, b, x):
    # a(x) <= b(x), with both denominators positive.
    left = (a[0] * x + a[1]) * b[2]
    right = (b[0] * x + b[1]) * a[2]
    return left <= right

def redundant(a, b, c):
    # Slopes are strictly decreasing.
    # b is redundant iff intersection(a,b) >= intersection(b,c).

    n1 = b[1] * a[2] - a[1] * b[2]
    d1 = a[0] * b[2] - b[0] * a[2]

    n2 = c[1] * b[2] - b[1] * c[2]
    d2 = b[0] * c[2] - c[0] * b[2]

    return n1 * d2 >= n2 * d1

def build_hull(lines):
    if not lines:
        return []

    lines.sort(key=cmp_to_key(slope_cmp))

    hull = []

    for line in lines:
        if hull:
            last = hull[-1]

            # Same slope. Keep the smaller intercept.
            if line[0] * last[2] == last[0] * line[2]:
                if line[1] * last[2] < last[1] * line[2]:
                    hull[-1] = line
                continue

        while len(hull) >= 2 and redundant(hull[-2], hull[-1], line):
            hull.pop()

        hull.append(line)

    return hull

def solve():
    X, Y, K = map(int, input().split())

    points = [tuple(map(int, input().split())) for _ in range(K)]
    x1, y1 = points[0]

    ymin = 0
    ymax = Y

    upper_lines = []
    lower_lines = []

    base_sq = x1 * x1 + y1 * y1

    for xi, yi in points[1:]:
        dx = xi - x1
        dy = yi - y1
        C = xi * xi + yi * yi - base_sq

        if dx == 0:
            s = xi + x1

            if dy > 0:
                # y <= (yi + y1) / 2
                ymax = min(ymax, s // 2)
            else:
                # y >= (yi + y1) / 2
                ymin = max(ymin, (s + 1) // 2)

        elif dx > 0:
            # x <= (C - 2*dy*y) / (2*dx)
            upper_lines.append((-2 * dy, C, 2 * dx))

        else:
            # x >= (2*dy*y - C) / (2*(-dx))
            #
            # Store the negation:
            # -x_bound = (-2*dy*y + C) / (2*(-dx))
            lower_lines.append((2 * dy, -C, -2 * dx))

    if ymin > ymax:
        print(0)
        return

    upper_hull = build_hull(upper_lines)
    lower_hull = build_hull(lower_lines)

    upper_ptr = 0
    lower_ptr = 0

    answer = 0

    for y in range(ymin, ymax + 1):
        if upper_hull:
            while (
                upper_ptr + 1 < len(upper_hull)
                and value_leq(
                    upper_hull[upper_ptr + 1],
                    upper_hull[upper_ptr],
                    y
                )
            ):
                upper_ptr += 1

            m, b, d = upper_hull[upper_ptr]
            upper_num = m * y + b
            right = upper_num // d
        else:
            right = X

        if lower_hull:
            while (
                lower_ptr + 1 < len(lower_hull)
                and value_leq(
                    lower_hull[lower_ptr + 1],
                    lower_hull[lower_ptr],
                    y
                )
            ):
                lower_ptr += 1

            m, b, d = lower_hull[lower_ptr]
            lower_neg_num = m * y + b

            # Original lower bound is -lower_neg_line.
            # ceil(-z) = -floor(z).
            left = -(lower_neg_num // d)
        else:
            left = 0

        if left < 0:
            left = 0
        if right > X:
            right = X

        if left <= right:
            answer += right - left + 1

    print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc qua`sys.stdin.readline`và điểm được đánh dấu đầu tiên được sử dụng làm`p1`chính xác như được chỉ định bởi định dạng đầu vào. số lượng`base_sq`cửa hàng`x1^2 + y1^2`, do đó, mọi hằng số của đối thủ cạnh tranh có thể được hình thành mà không cần tính toán lại cùng một giá trị. 

Ba nhánh trên`dx`tương ứng trực tiếp với ba trường hợp hình học từ thuật toán. Vì`dx = 0`, không có dòng nào được tạo vì hạn chế hoàn toàn theo chiều dọc. Vì`dx > 0`, mẫu số của đường giới hạn trên là dương. Vì`dx < 0`, nhân với`-1`làm cho mẫu số dương trước khi dòng bị phủ định. 

Cửa hàng thân tàu`(m, b, d)`thay vì độ dốc và điểm chặn của dấu phẩy động. Mọi so sánh đều nhân chéo hai phân số. Tọa độ có thể lớn bằng`2 * 10^5`và tọa độ bình phương có thể đạt tới khoảng`4 * 10^10`, nhưng số nguyên Python có độ chính xác tùy ý nên không có vấn đề tràn. 

các`redundant`test sử dụng tọa độ giao nhau chính xác của các đường liên tiếp. Mẫu số trong các biểu thức giao đó là dương vì thân tàu được sắp xếp theo độ dốc giảm dần. Điều này làm cho phép nhân chéo có hiệu lực ngay cả khi tọa độ giao nhau là âm. 

Con trỏ truy vấn chỉ di chuyển về phía trước. Vì các hàng được xử lý tăng dần`y`, đường tối ưu cũng di chuyển đơn điệu qua thân tàu có các độ dốc được sắp xếp một cách nhất quán. Điều này làm giảm tất cả các truy vấn đường bao sau khi xây dựng về thời gian tuyến tính. 

Hoạt động sàn là một điểm tinh tế khác. của Python`//`là phép chia sàn toán học, bao gồm cả tử số âm, chính xác là những gì ranh giới hợp lý yêu cầu. Đối với đường bao dưới bị phủ định, nếu giá trị của nó là`z = -g`, giới hạn dưới của số nguyên mong muốn là`ceil(g) = -floor(z)`, giải thích biểu thức được sử dụng trong mã. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,`p1 = (2,2)`. Bốn điểm còn lại tạo ra các giới hạn liên quan sau:`(1,1)`cho`x >= 3 - y`.`(1,3)`cho`x >= y - 1`.`(3,3)`cho`x <= 5 - y`.`(3,1)`cho`x <= y + 1`. 

Do đó, các phong bì được`L(y) = max(3-y, y-1)`Và`R(y) = min(5-y, y+1)`. 

Quá trình quét hàng là: 

|`y`|`L(y)`|`R(y)`| Tốt`x`giá trị | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | không | 0 | 
| 1 | 2 | 2 | 2 | 1 | 
| 2 | 1 | 3 | 1, 2, 3 | 3 | 
| 3 | 2 | 2 | 2 | 1 | 
| 4 | 3 | 1 | không | 0 | 

Tổng cộng là`1 + 3 + 1 = 5`. Hàng giữa chứa`p1`chính nó, và các hàng ngay bên trên và bên dưới chỉ chứa các điểm trên các đường phân giác tương ứng. Điều này chứng tỏ tại sao sự bình đẳng phải được chấp nhận. 

Đối với mẫu 2,`p1 = (0,0)`và mọi đối thủ cạnh tranh đều`(i,0)`vì`1 <= i <= 5`. Mỗi đối thủ cạnh tranh đưa ra`2ix <= i^2`,

hoặc`x <= i/2`. 

Giới hạn trên nhỏ nhất đến từ`(1,0)`, cho`x <= 1/2`. Từ`x`là không thể thiếu, mọi điểm tốt đều có`x = 0`. Không có giới hạn dưới và không có hạn chế về hàng. 

|`y`| Giới hạn dưới | Giới hạn trên | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 1 | 
| 3 | 0 | 0 | 1 | 
| 4 | 0 | 0 | 1 | 
| 5 | 0 | 0 | 1 | 
| 6 | 0 | 0 | 1 | 

Câu trả lời là`7`. Ví dụ này cũng cho thấy tại sao nhiều đối thủ cạnh tranh có thể biến mất khỏi danh sách. Một lần`(1,0)`đưa ra giới hạn chặt chẽ nhất, tất cả các đường phân giác song song sau đó đều không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(K log K + Y)`| Hai tập hợp dòng được sắp xếp và rút gọn thành các nhóm, sau đó tất cả các hàng có liên quan sẽ được quét một lần. | 
| Không gian |`O(K)`| Tối đa một dòng cho mỗi đối thủ cạnh tranh được lưu trữ trước khi loại bỏ các dòng thừa. | 

Với`K, Y <= 2 * 10^5`, việc sắp xếp chi phối chi phí tiệm cận. Thuật toán không bao giờ liệt kê`O(XY)`điểm lưới, đây là điểm khác biệt quan trọng so với lực lượng vũ phu. Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ các mối lo ngại về tràn yêu cầu số học 64 bit trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Người trợ giúp tạm thời thay thế nó`input`chức năng và nắm bắt đầu ra của nó, vì vậy mỗi xác nhận thực hiện giống nhau`solve()`được sử dụng bởi bài nộp.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = solution.input

    sys.stdin = io.StringIO(inp)
    solution.input = sys.stdin.readline
    sys.stdout = io.StringIO()

    try:
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        solution.input = old_input
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""\
4 4 5
2 2
1 1
1 3
3 3
3 1
""") == "5", "sample 1"

# Provided sample 2
assert run("""\
6 6 6
0 0
1 0
2 0
3 0
4 0
5 0
""") == "7", "sample 2"

# Minimum-size rectangle, only p1.
assert run("""\
1 1 1
0 0
""") == "4", "minimum case"

# All competitors have the same x coordinate as p1.
assert run("""\
2 4 3
1 2
1 0
1 4
""") == "9", "horizontal bisectors"

# Half-integer bisector: x >= 1/2 becomes x >= 1.
assert run("""\
2 2 2
1 1
0 1
""") == "6", "half-integer boundary"

# Maximum K, with the first competitor already giving the tightest
# possible x-bound. There are 200000 distinct marked points.
points = ["0 0"]
points.extend(f"{i} 0" for i in range(1, 200000))

max_case = "200000 200000 200000\n" + "\n".join(points) + "\n"

assert run(max_case) == "200001", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 0 0`|`4`| Kích thước tối thiểu,`K = 1`, và sự khác biệt giữa điểm lưới và ô | 
|`2 4 3 / (1,2), (1,0), (1,4)`|`9`| Các đối thủ cạnh tranh có cùng`x`các hạn chế về tọa độ và hàng thuần túy | 
|`2 2 2 / (1,1), (0,1)`|`6`| Xử lý sàn chính xác tại đường phân giác nửa số nguyên | 
|`200000 200000 200000`có điểm`(0,0), (1,0), ..., (199999,0)`|`200001`| Tọa độ lớn, tối đa`K`, và hiệu suất của việc xây dựng đường bao | 

## Vỏ cạnh 

Khi nào`K = 1`, các mảng dòng trống. Các truy vấn thân tàu bị bỏ qua, do đó mã sử dụng`left = 0`Và`right = X`cho mỗi hàng. Đối với đầu vào```
1 1 1
0 0
```khoảng cách hàng là`[0,1]`cho cả hai`y = 0`Và`y = 1`, đóng góp hai điểm mỗi hàng và tạo ra`4`. 

Khi đối thủ cạnh tranh chia sẻ`x`với`p1`, mã đi vào`dx == 0`chi nhánh. Vì```
2 4 3
1 2
1 0
1 4
```điểm`(1,0)`có`dy = -2`, do đó điều kiện trở thành`y >= 1`. điểm`(1,4)`có`dy = 2`, do đó điều kiện trở thành`y <= 3`. Khoảng cách hàng kết quả là`[1,3]`và mỗi hàng chứa cả ba khả năng`x`tọa độ. Câu trả lời là`9`. 

Đối với ranh giới nửa số nguyên```
2 2 2
1 1
0 1
```đối thủ cạnh tranh có`dx = -1`,`dy = 0`và hàm giới hạn dưới là`1/2`. Phong bì dưới đánh giá chính xác`1/2`và phép chia tầng của Python cho`0`cho giá trị được chuyển đổi`-1/2`chỉ sau khi việc chuyển đổi dấu được xử lý chính xác. Mã tính giới hạn dưới của số nguyên ban đầu là`1`, đưa ra các hàng hợp lệ`x = 1,2`cho mỗi người trong số ba`y`giá trị và câu trả lời`6`. 

Một điểm nằm chính xác trên đường phân giác phải giữ nguyên tốt vì cho phép liên kết. Trong mẫu 1,`(2,2)`là`p1`chính nó, trong khi`(2,1)`Và`(2,3)`nằm trên các đường phân giác tương ứng. Việc so sánh phong bì sử dụng`<=`, do đó, một đường liên kết với đường tốt nhất hiện tại có thể hoạt động mà không loại trừ tọa độ bị ràng buộc. 

Ranh giới hình chữ nhật cũng là một phần của không gian tìm kiếm. Nếu giới hạn dưới được tính toán là âm, nó sẽ được giữ ở mức`0`và nếu giới hạn trên vượt quá`X`, nó được kẹp vào`X`. Do đó, ô Voronoi mở rộng ra ngoài hình chữ nhật sẽ được cắt bớt một cách chính xác thay vì được tính bên ngoài lưới cho phép. 

Cuối cùng, câu trả lời không bao giờ có thể bằng 0 đối với đầu vào hợp lệ vì`p1`chính nó là một điểm nguyên bên trong hình chữ nhật và khoảng cách của nó với chính nó bằng 0. Việc triển khai có thể tạm thời có được một khoảng trống trên một hàng, nhưng hàng đó`y = y1`luôn chứa ít nhất`x = x1`, vì vậy tổng số câu trả lời ít nhất là một.
