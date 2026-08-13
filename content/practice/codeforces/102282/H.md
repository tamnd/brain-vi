---
title: "CF 102282H - \u0411\u0435\u0437 \u0438\u043c\u0435\u043d\u0438"
description: "Chúng ta có hai đường tròn trong mặt phẳng Descartes. Mỗi đường tròn được mô tả bởi tọa độ nguyên của tâm và bán kính nguyên dương của nó. Nhiệm vụ là xác định có bao nhiêu điểm thuộc về cả hai vòng tròn và in những điểm đó với độ chính xác bằng số vừa đủ."
date: "2026-08-13T09:12:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "H"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 73
verified: true
draft: false
---

[CF 102282H - \u0411\u0435\u0437 \u0438\u043c\u0435\u043d\u0438](https://codeforces.com/problemset/problem/102282/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đường tròn trong mặt phẳng Descartes. Mỗi đường tròn được mô tả bởi tọa độ nguyên của tâm và bán kính nguyên dương của nó. Nhiệm vụ là xác định có bao nhiêu điểm thuộc về cả hai vòng tròn và in những điểm đó với độ chính xác bằng số vừa đủ. Hai đường tròn phân biệt có thể có 0, 1 hoặc 2 điểm chung. Nếu hai đường tròn thực sự là một đường tròn thì mọi điểm trên đường tròn đều chung nhau nên đáp án cần có là`MANY`. 

Giới hạn tọa độ và bán kính chỉ là (10^3), do đó, đầu vào rất nhỏ. Quan trọng hơn, chỉ có hai đối tượng hình học và đáp án chứa nhiều nhất hai điểm hữu hạn. Do đó, một giải pháp nên sử dụng một số lượng phép tính số học không đổi thay vì lặp lại trên mặt phẳng hoặc tìm kiếm các điểm giao nhau bằng số. Ngay cả một đạo hàm hình học đơn giản (O(1)) cũng nằm trong giới hạn một giây và sử dụng bộ nhớ không đáng kể. Các số nguyên như khoảng cách bình phương tối đa theo thứ tự (10^7), do đó số học số nguyên của Python hoàn toàn không có vấn đề tràn. 

Một số trường hợp rất dễ bị xử lý sai nếu việc triển khai nhảy thẳng vào công thức giao nhau. Nếu các đường tròn có cùng tâm và cùng bán kính thì có vô số điểm chung. Ví dụ,```
0 0 1
0 0 1
```phải sản xuất```
MANY
```Công thức dựa trên việc chia cho khoảng cách giữa các tâm sẽ chia cho 0 ở đây. 

Nếu các tâm trùng nhau nhưng bán kính khác nhau thì các đường tròn không có điểm chung. Ví dụ,```
0 0 1
0 0 2
```có đầu ra```
0
```Việc triển khai bất cẩn chỉ kiểm tra xem các tâm có trùng nhau hay không có thể phân loại không chính xác đây là vô số giao lộ. 

Hai đường tròn tiếp tuyến ngoài có đúng một giao điểm. Ví dụ,```
0 0 1
2 0 1
```phải tạo ra một điểm,`(1, 0)`. Phép so sánh dấu phẩy động dự kiến ​​có hai nghiệm phân biệt có thể vô tình in ra cùng một điểm tiếp tuyến hai lần. 

Tiếp tuyến nội bộ có cùng một vấn đề trong một cấu hình khác. Vì```
0 0 3
1 0 2
```vòng tròn nhỏ tiếp xúc với vòng tròn lớn hơn`(3, 0)`, vì vậy câu trả lời lại là một điểm. 

Cuối cùng, hai đường tròn có thể có tâm này ở trong tâm kia nhưng vẫn hoàn toàn tách rời nhau. Ví dụ,```
0 0 5
1 0 1
```không có giao điểm vì khoảng cách giữa các tâm nhỏ hơn hiệu bán kính. Việc chỉ kiểm tra xem khoảng cách trung tâm có nhỏ hơn tổng bán kính hay không sẽ coi đây là trường hợp giao nhau một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng tìm kiếm các điểm chung bằng cách quét vùng chứa cả hai vòng tròn và kiểm tra xem một điểm ứng viên có thỏa mãn cả hai phương trình vòng tròn hay không. Vì tọa độ giao điểm là các số thực tùy ý nên việc triển khai dựa trên lưới phải chọn một số độ phân giải số. Để đảm bảo tọa độ trong (10^{-6}), cần phải có lưới có khoảng cách xung quanh (10^{-6}). Phạm vi tọa độ có thể trải rộng khoảng 2000 đơn vị theo mỗi hướng, cung cấp khoảng (2\cdot10^9) vị trí dọc theo mỗi trục và khoảng (4\cdot10^{18}) điểm lưới trong trường hợp xấu nhất. Ngay cả việc kiểm tra một điểm trong thời gian không đổi cũng sẽ khiến phương pháp này hoàn toàn không khả thi và việc giảm độ phân giải lưới sẽ không còn đảm bảo độ chính xác cần thiết nữa. 

Ý tưởng brute-force hoạt động về mặt khái niệm vì giao điểm thực sự chính xác là một điểm thỏa mãn cả hai phương trình đường tròn, nhưng nó thất bại vì mặt phẳng liên tục và độ chính xác cần thiết khiến việc lấy mẫu toàn diện trở nên rất lớn. Quan sát hữu ích là hai phương trình đường tròn có thể được kết hợp về mặt đại số. Trừ chúng sẽ loại bỏ các số hạng bậc hai (x^2+y^2), để lại một đường thẳng. Mọi giao điểm phải nằm trên đường thẳng này, gọi là trục đẳng phương. Sau đó, chúng ta có thể giao đường tròn đầu tiên với đường thẳng đó, đường này cho trực tiếp nhiều nhất hai điểm. 

Có một cách thậm chí còn rõ ràng hơn để tính tọa độ. Gọi tâm là (C_1) và (C_2) và gọi (d) là khoảng cách giữa chúng. Đường thẳng đi qua hai tâm là trung điểm của dây chung. Nếu khoảng cách từ (C_1) đến dây đó là (a) thì định lý Pythagore cho 

[ 
a=\frac{r_1^2-r_2^2+d^2}{2d}. 
] 

Dây chung vuông góc với đường nối tâm. Nếu (h) là nửa độ dài của dây thì 

[ 
h^2=r_1^2-a^2. 
] 

Chân đường vuông góc từ (C_1) đến dây cung có được bằng cách di chuyển một khoảng (a) từ (C_1) về phía (C_2). Khi đó, vectơ đơn vị vuông góc cho phép chúng ta di chuyển theo (h) theo cả hai hướng để thu được hai điểm giao nhau có thể có. 

Trước khi sử dụng các công thức này, chúng tôi phân loại cấu hình bằng khoảng cách bình phương. Điều này tránh các căn bậc hai không cần thiết và làm cho tất cả các phép so sánh hình học trở nên chính xác vì các giá trị đầu vào là số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(4\cdot10^{18})) ứng viên kiểm tra ở độ phân giải (10^{-6}) | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tâm và bán kính của hai đường tròn. Tính toán`dx = x2 - x1`,`dy = y2 - y1`, và bình phương khoảng cách tâm`d2 = dx*dx + dy*dy`. Giữ`d2`thay vì lấy ngay căn bậc hai, chúng ta có thể phân loại tất cả các cấu hình suy biến và không giao nhau bằng cách sử dụng các phép so sánh số nguyên chính xác. 
2. Nếu`d2 == 0`và bán kính bằng nhau, các đường tròn trùng nhau. Mọi điểm của một đường tròn đều thuộc về đường tròn kia, vì vậy hãy in`MANY`và dừng lại. 
3. Nếu`d2 == 0`và bán kính khác nhau, các vòng tròn đồng tâm nhưng khác biệt. Họ không thể gặp nhau, vì vậy hãy in`0`và dừng lại. 
4. So sánh khoảng cách tâm với tổng và hiệu các bán kính bằng bình phương. Nếu (d > r_1+r_2), các vòng tròn được tách ra bên ngoài. Nếu (d < |r_1-r_2|), một vòng tròn nằm hoàn toàn bên trong vòng tròn kia. Trong cả hai trường hợp đều không có điểm giao nhau. 
5. Tính toán 

[ 
d=\sqrt{d^2} 
] 

và sau đó tính toán 

[ 
a=\frac{r_1^2-r_2^2+d^2}{2d}. 
] 

Đây là khoảng cách từ tâm đầu tiên đến dây chung dọc theo đường nối các tâm. Công thức tiếp theo bằng cách viết hai quan hệ tam giác vuông cho hai điểm giao nhau và trừ chúng. 

1. Tính toán 

[ 
h^2=r_1^2-a^2. 
] 

Đối với hai vòng tròn giao nhau, giá trị này không âm. Bởi vì`a`được tính toán bằng số học dấu phẩy động, cấu hình tiếp tuyến có thể tạo ra một giá trị âm nhỏ như`-1e-12`thay vì chính xác bằng 0. Kẹp giá trị đó về 0 trước khi lấy căn bậc hai. 

1. Tìm chân đường vuông góc kẻ từ tâm thứ nhất đến dây chung: 

[ 
p_x=x_1+a\frac{dx}{d},\qquad 
p_y=y_1+a\frac{dy}{d}. 
] 

Sau đó dựng vectơ đơn vị vuông góc với đường tâm, 

[ 
\left(-\frac{dy}{d},\frac{dx}{d}\right). 
] 

Nhân vectơ này với (h) sẽ có độ dịch chuyển từ điểm giữa dây tới điểm giao nhau. 

1. Nếu (h=0), các đường tròn tiếp tuyến nên hai điểm có thể trùng nhau. In một điểm. Ngược lại in cả hai 

[ 
(p_x-h\frac{dy}{d},\ p_y+h\frac{dx}{d}) 
] 

và 

[ 
(p_x+h\frac{dy}{d},\ p_y-h\frac{dx}{d}). 
] 

Thứ tự không quan trọng. 

Bất biến đằng sau cách xây dựng là mọi điểm được tạo đều nằm trên đường tròn đầu tiên theo quan hệ tam giác vuông (a^2+h^2=r_1^2), và nó cũng nằm trên đường tròn thứ hai vì vị trí dây cung đã chọn thỏa mãn phương trình bán kính tương ứng. Ngược lại, mọi điểm chung phải nằm trên dây chung, vị trí của nó được xác định duy nhất bởi hai phương trình đường tròn. Việc xây dựng vuông góc tìm mọi điểm trên dây cung nằm trên đường tròn đầu tiên, do đó nó tạo ra chính xác tất cả các giao điểm. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1
    d2 = dx * dx + dy * dy

    # Coincident centers.
    if d2 == 0:
        if r1 == r2:
            print("MANY")
        else:
            print(0)
        return

    # Disjoint or one circle strictly inside the other.
    sum_r = r1 + r2
    diff_r = abs(r1 - r2)

    if d2 > sum_r * sum_r or d2 < diff_r * diff_r:
        print(0)
        return

    d = math.sqrt(d2)

    # Distance from the first center to the common chord.
    a = (r1 * r1 - r2 * r2 + d2) / (2.0 * d)

    # Half of the common chord length, squared.
    h2 = r1 * r1 - a * a

    # Floating-point roundoff can make a tangent case slightly negative.
    if h2 < 0.0:
        h2 = 0.0

    h = math.sqrt(h2)

    # Midpoint of the common chord.
    px = x1 + a * dx / d
    py = y1 + a * dy / d

    # Unit vector perpendicular to the line between centers.
    ux = -dy / d
    uy = dx / d

    # First intersection point.
    x_a = px + h * ux
    y_a = py + h * uy

    # Tangency: both mathematical constructions give the same point.
    if h == 0.0:
        print(1)
        print(f"{x_a:.10f} {y_a:.10f}")
        return

    # Two distinct intersection points.
    x_b = px - h * ux
    y_b = py - h * uy

    print(2)
    print(f"{x_a:.10f} {y_a:.10f}")
    print(f"{x_b:.10f} {y_b:.10f}")

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán vectơ giữa tâm và chiều dài bình phương của nó. Trường hợp đặc biệt`d2 == 0`phải được xử lý trước công thức tổng quát vì công thức tính`a`chia cho khoảng cách trung tâm. 

Hai so sánh tiếp theo sử dụng bình phương số nguyên. điều kiện`d2 > (r1 + r2)^2`có nghĩa là các vòng tròn cách xa nhau hơn tổng bán kính của chúng. điều kiện`d2 < (r1 - r2)^2`có nghĩa là khoảng cách tâm nhỏ hơn chênh lệch bán kính tuyệt đối, do đó một vòng tròn nằm hoàn toàn bên trong vòng tròn kia. Sự bình đẳng trong cả hai sự so sánh đều không bị bác bỏ một cách có chủ ý bởi vì sự bình đẳng có nghĩa là tiếp tuyến, mang lại một điểm giao nhau hợp lệ. 

Sau khi biết cấu hình có một hoặc hai giao điểm, mã sẽ tính toán`d`, theo sau là`a`Và`h2`. Biểu thức cho`a`là công thức hình học trung tâm. giá trị`h2`xác định xem dây chung có độ dài dương hay đã co lại thành một điểm tiếp tuyến. 

các`if h2 < 0.0`việc chỉnh sửa chỉ xử lý làm tròn dấu phẩy động. Một cấu hình thực sự không giao nhau đã bị từ chối bằng cách sử dụng các so sánh số nguyên chính xác, do đó, một giá trị âm nhỏ ở đây chỉ có thể phát sinh từ lỗi số học gần tiếp tuyến. 

Trung điểm của dây chung được tính theo phương từ tâm tới tâm. Vectơ`(-dy / d, dx / d)`vuông góc với hướng đó và có chiều dài bằng 1, do đó nhân nó với`h`đưa ra chính xác độ lệch từ điểm giữa hợp âm đến giao điểm. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có loại số nguyên có chiều rộng cố định, các biểu thức bình phương vẫn phải được kiểm tra theo phạm vi số nguyên có sẵn, mặc dù các giới hạn đã cho đủ nhỏ để số nguyên 64 bit có dấu mà không gặp khó khăn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào mô tả các vòng tròn có tâm tại`(0, 0)`Và`(300, 0)`, cả hai đều có bán kính`200`. 

| Bước |`dx`|`dy`|`d2`|`d`|`a`|`h2`| 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 300 | 0 | 90000 | 300 | 150 | 17500 | 

Khoảng cách trung tâm là`300`. Từ`|200 - 200| < 300 < 200 + 200`, các vòng tròn cắt nhau hai lần. Trung điểm của dây cung là`(150, 0)`, và độ dài nửa hợp âm là (\sqrt{17500}), xấp xỉ`132.2875656`. Hướng vuông góc là hướng thẳng đứng nên hai điểm xấp xỉ nhau`(150, 132.2875656)`Và`(150, -132.2875656)`. 

Đầu ra theo định dạng đã nêu là:```
2
150.0000000 132.2875656
150.0000000 -132.2875656
```Việc xây dựng đối xứng xung quanh đường nối các tâm, chính xác như hình học dự đoán. 

### Mẫu 2 

Các vòng tròn có tâm tại`(0, 0)`Và`(0, 2)`, cả hai đều có bán kính`1`. 

| Bước |`dx`|`dy`|`d2`|`d`|`a`|`h2`| 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 2 | 4 | 2 | 1 | 0 | 

Ở đây khoảng cách tâm bằng tổng bán kính nên các đường tròn là tiếp tuyến ngoài. Dây chung có độ dài bằng 0 vì`h2 = 0`. Điểm duy nhất của nó là`(0, 1)`. 

Đầu ra là:```
1
0.0000000 1.0000000
```Đường vẽ này thể hiện ranh giới giữa một và hai giao điểm và cho thấy lý do tại sao trường hợp tiếp tuyến không được in cùng một điểm hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Một số lượng cố định các phép tính số học và so sánh được thực hiện. | 
| Không gian | (O(1)) | Chỉ một số lượng biến vô hướng và tọa độ đầu ra không đổi được lưu trữ. | 

Các ràng buộc nhỏ hơn nhiều so với những gì một giải pháp hình học (O(1)) cần. Thuật toán chỉ thực hiện một số phép tính số nguyên, căn bậc hai, phép chia và phép định dạng, do đó, nó dễ dàng phù hợp với giới hạn thời gian một giây và về cơ bản không sử dụng bộ nhớ. 

## Trường hợp thử nghiệm 

Phần khai thác bài kiểm tra bên dưới so sánh các câu trả lời hình học thay vì yêu cầu một thứ tự chính xác của hai điểm giao nhau. Nó cũng kiểm tra đặc biệt`MANY`trường hợp và số điểm yêu cầu.```python
import sys
import io
import math

input = sys.stdin.readline

def solve():
    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1
    d2 = dx * dx + dy * dy

    if d2 == 0:
        if r1 == r2:
            print("MANY")
        else:
            print(0)
        return

    sum_r = r1 + r2
    diff_r = abs(r1 - r2)

    if d2 > sum_r * sum_r or d2 < diff_r * diff_r:
        print(0)
        return

    d = math.sqrt(d2)
    a = (r1 * r1 - r2 * r2 + d2) / (2.0 * d)
    h2 = r1 * r1 - a * a

    if h2 < 0.0:
        h2 = 0.0

    h = math.sqrt(h2)

    px = x1 + a * dx / d
    py = y1 + a * dy / d

    ux = -dy / d
    uy = dx / d

    x_a = px + h * ux
    y_a = py + h * uy

    if h == 0.0:
        print(1)
        print(f"{x_a:.10f} {y_a:.10f}")
        return

    x_b = px - h * ux
    y_b = py - h * uy

    print(2)
    print(f"{x_a:.10f} {y_a:.10f}")
    print(f"{x_b:.10f} {y_b:.10f}")

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

def parse_output(out: str):
    lines = out.strip().splitlines()

    if lines == ["MANY"]:
        return "MANY", []

    n = int(lines[0])
    points = []

    for line in lines[1:]:
        x, y = map(float, line.split())
        points.append((x, y))

    assert len(points) == n
    return n, points

def assert_points_on_circles(inp: str, out: str):
    data = list(map(int, inp.split()))
    x1, y1, r1, x2, y2, r2 = data

    result, points = parse_output(out)

    if result == "MANY":
        assert x1 == x2 and y1 == y2 and r1 == r2
        return

    for x, y in points:
        d1 = (x - x1) ** 2 + (y - y1) ** 2
        d2 = (x - x2) ** 2 + (y - y2) ** 2

        assert abs(d1 - r1 * r1) <= 1e-5 * max(1.0, r1 * r1)
        assert abs(d2 - r2 * r2) <= 1e-5 * max(1.0, r2 * r2)

# Provided sample 1.
sample1 = """\
0 0 200
300 0 200
"""
out = run(sample1)
result, points = parse_output(out)
assert result == 2, "sample 1 count"
assert_points_on_circles(sample1, out)

# Provided sample 2.
sample2 = """\
0 0 1
0 2 1
"""
out = run(sample2)
result, points = parse_output(out)
assert result == 1, "sample 2 count"
assert_points_on_circles(sample2, out)
assert abs(points[0][0]) < 1e-9
assert abs(points[0][1] - 1.0) < 1e-9

# Custom case 1: coincident circles, the minimum radius.
case1 = """\
0 0 1
0 0 1
"""
assert run(case1).strip() == "MANY", "coincident circles"

# Custom case 2: concentric circles with different radii.
case2 = """\
0 0 1
0 0 2
"""
assert run(case2).strip() == "0", "concentric distinct circles"

# Custom case 3: internal tangency.
case3 = """\
0 0 3
1 0 2
"""
out = run(case3)
result, points = parse_output(out)
assert result == 1, "internal tangency count"
assert_points_on_circles(case3, out)
assert abs(points[0][0] - 3.0) < 1e-9
assert abs(points[0][1]) < 1e-9

# Custom case 4: maximum-size coordinates and radii, two intersections.
case4 = """\
-1000 -1000 1000
1000 -1000 1000
"""
out = run(case4)
result, points = parse_output(out)
assert result == 2, "maximum-size two-intersection case"
assert_points_on_circles(case4, out)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 200`/`300 0 200`| Hai điểm giao nhau | Xây dựng chung hai nút giao thông | 
|`0 0 1`/`0 2 1`| Một điểm tại`(0, 1)`| Tiếp tuyến ngoài | 
|`0 0 1`/`0 0 1`|`MANY`| Vòng tròn trùng khớp và ngăn chặn chia cho 0 | 
|`0 0 1`/`0 0 2`|`0`| Vòng tròn đồng tâm có bán kính khác nhau | 
|`0 0 3`/`1 0 2`| Một điểm tại`(3, 0)`| Tiếp tuyến nội bộ | 
|`-1000 -1000 1000`/`1000 -1000 1000`| Hai điểm giao nhau | Tọa độ và bán kính kích thước tối đa | 

## Vỏ cạnh 

Đối với các vòng tròn trùng nhau, hãy xem xét```
0 0 1
0 0 1
```Khoảng cách trung tâm bình phương bằng 0 và bán kính bằng nhau. Thuật toán in ngay lập tức`MANY`trước khi thử tính khoảng cách trung tâm làm ước số. Điều bất biến ở đây là cả hai phương trình đều mô tả chính xác cùng một tập hợp điểm. 

Đối với các đường tròn đồng tâm có bán kính khác nhau,```
0 0 1
0 0 2
```khoảng cách trung tâm bình phương lại bằng 0, nhưng bán kính thì khác. Thuật toán in`0`. Hai đường tròn có cùng tâm chỉ có thể chia sẻ điểm nếu bán kính của chúng bằng nhau. 

Đối với tiếp tuyến bên ngoài,```
0 0 1
2 0 1
```khoảng cách trung tâm bình phương là`4`, chính xác bằng`(1 + 1)^2`. Điều kiện rời rạc sử dụng`>`còn hơn là`>=`, do đó trường hợp này vẫn tồn tại trong phép tính giao điểm. Giá trị kết quả của`h2`bằng 0, cho điểm duy nhất`(1, 0)`. 

Đối với tiếp tuyến nội bộ,```
0 0 3
1 0 2
```khoảng cách trung tâm là chính xác`|3 - 2| = 1`. Một lần nữa, bất đẳng thức nghiêm ngặt không bác bỏ cấu hình. Công thức cho`a = 3`Và`h2 = 0`, sản xuất`(3, 0)`, điểm mà đường tròn nhỏ chạm vào đường tròn lớn. 

Để ngăn chặn nghiêm ngặt,```
0 0 5
1 0 1
```khoảng cách trung tâm là`1`, trong khi chênh lệch bán kính là`4`. Vì (1^2 < 4^2) nên thuật toán in ngay`0`. Không cần tính toán căn bậc hai hoặc dấu phẩy động cho phân loại này. 

Đối với hai nút giao thông thường,```
0 0 2
3 0 2
```khoảng cách trung tâm là`3`, nằm đúng giữa chênh lệch bán kính`0`và tổng bán kính`4`. Thuật toán đạt đến giai đoạn xây dựng, thu được`a = 1.5`, và thu được kết quả dương`h2`, do đó nó in ra hai điểm đối xứng. Tính đối xứng của chúng xung quanh đường nối tâm tuân theo trực tiếp từ việc sử dụng vectơ đơn vị vuông góc. 

Ranh giới số chính là tiếp tuyến. Giá trị hình học chính xác của`h2`ở đó bằng 0, nhưng đánh giá dấu phẩy động có thể tạo ra một số âm nhỏ. Kẹp`h2`về 0 ngăn cản`math.sqrt`khỏi thất bại trong khi vẫn giữ nguyên tất cả các trường hợp hai giao điểm hợp lệ thực sự.
