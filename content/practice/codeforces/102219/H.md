---
title: "CF 102219H - Bạn có an toàn không?"
description: "Tọa độ cảm biến mô tả một tập hợp các điểm trong mặt phẳng. Vùng bị ảnh hưởng là đa giác khép kín ngắn nhất chứa mọi cảm biến, chính xác là bao lồi của các điểm cảm biến."
date: "2026-08-20T04:14:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "H"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 1178
verified: false
draft: false
---

[CF 102219H - Bạn có an toàn không?](https://codeforces.com/problemset/problem/102219/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 19 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Tọa độ cảm biến mô tả một tập hợp các điểm trong mặt phẳng. Vùng bị ảnh hưởng là đa giác khép kín ngắn nhất chứa mọi cảm biến, chính xác là bao lồi của các điểm cảm biến. Chúng ta phải in các đỉnh của thân tàu đó theo thứ tự ngược chiều kim đồng hồ, lặp lại đỉnh đầu tiên ở cuối. 

Sau khi thi công xong khu vực bị ảnh hưởng, các tọa độ còn lại là những vị trí cần xác định độ an toàn. Vị trí nằm hoàn toàn bên trong thân lồi là không an toàn. Vị trí bên ngoài nó là an toàn và vị trí nằm ngay trên ranh giới của nó cũng an toàn. 

Số lượng cảm biến và số lượng vị trí đều tối đa là 50 và có thể có tối đa 50 trường hợp thử nghiệm. Tọa độ là các số nguyên trong phạm vi nhỏ từ -500 đến 500. Các giới hạn này làm cho việc kiểm tra điểm trong đa giác O(CP) trở nên tầm thường, nhưng chúng cũng làm cho bao lồi O(C log C) tiêu chuẩn trở nên đặc biệt thuận tiện. Không cần hình học dấu phẩy động hoặc cấu trúc dữ liệu không gian phức tạp. Tích chéo số nguyên là đủ và độ lớn của chúng đủ nhỏ để vừa vặn thoải mái với số nguyên Python. 

Trường hợp tinh tế đầu tiên là một vị trí trên ranh giới. Hãy xem xét ba cảm biến tạo thành một hình tam giác:```
3 1
0 0
4 0
0 4
2 0
```Vị trí`(2, 0)`nằm chính xác trên mép thân tàu, do đó đầu ra phải là`2 0 is safe!`. Việc triển khai điểm trong đa giác bất cẩn coi các điểm ranh giới là bên trong sẽ đánh dấu không chính xác là nó không an toàn. 

Trường hợp thứ hai là một cảm biến nằm trên cạnh giữa hai đỉnh thân tàu thực tế:```
4 1
0 0
4 0
4 4
0 4
2 0
```điểm`(2, 0)`là một cảm biến, nhưng nó không phải là đỉnh của đa giác có chu vi tối thiểu. Thân tàu chỉ được chứa`(0,0)`,`(4,0)`,`(4,4)`, Và`(0,4)`. Giữ mọi cảm biến thẳng hàng sẽ tạo ra các điểm bổ sung trên chu vi được in mặc dù chúng không phải là các đỉnh đa giác. 

Trường hợp thứ ba là khi tất cả các cảm biến đều thẳng hàng:```
3 2
0 0
2 0
4 0
1 0
5 0
```Bao lồi suy biến về hai điểm cuối`(0,0)`Và`(4,0)`. Không có nội thất hai chiều nên cả hai điểm truy vấn đều an toàn. Một giải pháp mù quáng giả định thân tàu có ít nhất ba đỉnh có thể thất bại ở đây. 

Tọa độ cảm biến trùng lặp tạo ra một suy biến tương tự khác:```
3 1
1 1
1 1
1 1
1 1
```Chỉ có một điểm phân biệt nên vùng bị ảnh hưởng không có phần bên trong. Điểm truy vấn nằm trên thân tàu thoái hóa và an toàn. Việc loại bỏ các bản sao trước khi đóng thân tàu sẽ xử lý việc này một cách tự nhiên. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực trực tiếp có thể thử mọi thứ tự có thể có của các điểm cảm biến, đóng thứ tự thành đa giác, kiểm tra xem đa giác kết quả có chứa mọi cảm biến hay không và giữ đa giác hợp lệ với chu vi tối thiểu. Điều này đúng vì đa giác mong muốn phải có các đỉnh nằm trong tọa độ cảm biến và việc kiểm tra mọi thứ tự cuối cùng sẽ xem xét thứ tự tối ưu. 

Vấn đề là số lượng đặt hàng. Với C = 50 thì có 50! xấp xỉ 3,04 × 10^64 hoán vị có thể có. Ngay cả khi việc kiểm tra một hoán vị chỉ mất O(C), công việc sẽ ở mức 50 × 50!, khoảng 1,52 × 10^66 phép toán hình học cơ bản. Giới hạn 15 giây không thể cho phép điều này. 

Lực lượng vũ phu hoạt động vì câu trả lời là một đa giác có ranh giới được xác định bởi các điểm cảm biến cực trị. Quan sát quan trọng là bất kỳ cảm biến nào nằm hoàn toàn bên trong một đa giác lồi không bao giờ có thể cần thiết như một đỉnh của đa giác bao quanh có chu vi tối thiểu. Tương tự như vậy, nếu một số cảm biến thẳng hàng dọc theo một cạnh thân tàu thì chỉ có hai điểm cuối là quan trọng. Những gì còn lại chính xác là thân tàu lồi. 

Sau khi các cảm biến được thu gọn về phần bao lồi của chúng, thuật toán chuỗi đơn điệu của Andrew sẽ xây dựng nó trong thời gian O(C log C). Việc sắp xếp mang lại cho các điểm một thứ tự xác định và tích chéo cho chúng ta biết liệu điểm được thêm gần đây nhất sẽ rẽ trái hay rẽ phải. Rẽ không trái ở chuỗi dưới hoặc chuỗi trên có nghĩa là điểm giữa không thể là đỉnh thân tàu nên có thể loại bỏ. 

Sau khi xác định được thân tàu, mỗi vị trí có thể được phân loại bằng cách kiểm tra hướng của nó so với từng cạnh của thân tàu. Vì thân tàu ngược chiều kim đồng hồ nên một điểm nằm bên trong nó có tích chéo dương với mọi cạnh có hướng. Tích chéo bằng 0 có nghĩa là điểm nằm trên một cạnh và phải được phân loại là an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C · C!) | O(C) | Quá chậm | 
| Tối ưu | O(C log C + CP) | O(C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ cảm biến và loại bỏ các điểm trùng lặp. Các tọa độ trùng lặp không tạo ra thông tin hình học bổ sung, vì vậy việc giữ lại chúng sẽ chỉ làm phức tạp việc xây dựng thân tàu. 
2. Sắp xếp các điểm cảm biến riêng biệt theo thứ tự từ điển theo`(x, y)`. Điều này mang lại cho thuật toán của Andrew thứ tự cần thiết và cũng làm cho các điểm ngoài cùng bên trái và ngoài cùng bên phải trở nên xác định. 
3. Xây dựng phần thân dưới từ trái sang phải. Đối với mỗi điểm mới, hãy nối thêm điểm đó và kiểm tra ba điểm cuối cùng. Nếu tích chéo của chúng nhỏ hơn hoặc bằng 0, hãy xóa điểm ở giữa. Tích chéo âm có nghĩa là quay theo chiều kim đồng hồ, do đó điểm giữa nằm bên trong đường biên. Tích chéo bằng 0 có nghĩa là ba điểm thẳng hàng và điểm giữa không cần thiết làm đỉnh thân. 
4. Xây dựng phần thân trên bằng cách sử dụng quy tắc tương tự nhưng xử lý các điểm được sắp xếp theo thứ tự ngược lại. Việc nối chuỗi dưới và chuỗi trên sẽ tạo thành thân lồi hoàn chỉnh theo thứ tự ngược chiều kim đồng hồ. 
5. Loại bỏ các điểm cuối trùng lặp được tạo khi nối hai chuỗi. Danh sách kết quả chứa mỗi đỉnh thân thực tế đúng một lần. 
6. In mỗi đỉnh thân trên dòng riêng của nó và sau đó in lại đỉnh đầu tiên. Cấu trúc chuỗi đơn điệu cho phép định hướng ngược chiều kim đồng hồ, do đó không cần phải sắp xếp hoặc xoay thêm. 
7. Đối với mỗi vị trí truy vấn, trước tiên hãy xử lý các thân suy biến. Nếu có ít hơn ba đỉnh thân tàu riêng biệt thì thân tàu không có phần bên trong hai chiều nên mọi vị trí đều an toàn. 
8. Để có một đa giác thích hợp, hãy tính tích chéo của mỗi cạnh thân có hướng với vectơ từ điểm bắt đầu của cạnh đó đến điểm truy vấn. Nếu bất kỳ tích chéo nào đều âm thì vị trí đó nằm ở bên ngoài. Nếu bất kỳ tích chéo nào bằng 0 thì nó nằm trên ranh giới và an toàn. Chỉ khi mọi sản phẩm chéo đều dương tính nghiêm ngặt thì vị trí đó hoàn toàn nằm bên trong và do đó không an toàn. 

### Tại sao nó hoạt động 

Bao lồi là tập lồi nhỏ nhất chứa tất cả các cảm biến. Bất kỳ đa giác bao quanh nào có chu vi tối thiểu đều có thể được thay thế bằng ranh giới của bao lồi này mà không làm tăng chu vi, trong khi mọi cảm biến vẫn được bao bọc. Do đó, chu vi cần thiết bao gồm chính xác các đỉnh bao lồi, bỏ qua các điểm thẳng hàng giữa hai đỉnh. 

Thuật toán của Andrew duy trì tính bất biến rằng mỗi chuỗi hiện tại là một ranh giới lồi hợp lệ cho các điểm được xử lý. Bất cứ khi nào ba điểm cuối cùng rẽ trái, điểm giữa không thể là điểm cực trị của bao lồi, do đó việc loại bỏ nó sẽ bảo toàn ranh giới đúng. Sau khi xử lý mọi điểm theo cả hai hướng, hai chuỗi tạo thành bao lồi hoàn chỉnh. 

Đối với đa giác lồi ngược chiều kim đồng hồ, mọi điểm trong đều nằm hoàn toàn bên trái của mọi cạnh có hướng. Do đó tất cả các tích chéo cạnh đều dương chính xác đối với các điểm ở phần trong nghiêm ngặt. Tích chéo bằng 0 xác định ranh giới mà vấn đề phân loại rõ ràng là an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - \
           (b[1] - a[1]) * (c[0] - a[0])

def convex_hull(points):
    points = sorted(set(points))

    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]

def strictly_inside(hull, p):
    n = len(hull)

    if n < 3:
        return False

    for i in range(n):
        a = hull[i]
        b = hull[(i + 1) % n]
        value = cross(a, b, p)

        if value <= 0:
            return False

    return True

def solve():
    t = int(input())
    output = []

    for case_no in range(1, t + 1):
        c, p = map(int, input().split())

        sensors = [tuple(map(int, input().split())) for _ in range(c)]
        locations = [tuple(map(int, input().split())) for _ in range(p)]

        hull = convex_hull(sensors)

        output.append(f"Case {case_no}")

        if hull:
            for x, y in hull:
                output.append(f"{x} {y}")
            output.append(f"{hull[0][0]} {hull[0][1]}")

        for x, y in locations:
            if strictly_inside(hull, (x, y)):
                status = "unsafe!"
            else:
                status = "safe!"
            output.append(f"{x} {y} is {status}")

        if case_no != t:
            output.append("")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`cross`hàm tính diện tích có dấu của tam giác tạo thành bởi ba điểm, nhân với hai. Dấu hiệu của nó là những gì kết cấu thân tàu cần. Dương có nghĩa là quay ngược chiều kim đồng hồ, 0 có nghĩa là cộng tuyến và âm có nghĩa là quay theo chiều kim đồng hồ. 

các`convex_hull`chức năng sử dụng đầu tiên`set`để loại bỏ tọa độ trùng lặp, sau đó sắp xếp các điểm. các`<= 0`điều kiện là có chủ ý. sử dụng`< 0`sẽ giữ lại các điểm thẳng hàng trên các cạnh thân tàu, nhưng chu vi yêu cầu bao gồm các đỉnh đa giác thực tế, do đó các cảm biến thẳng hàng trung gian phải được loại bỏ. 

Hai chuỗi chia sẻ điểm đầu tiên và điểm cuối cùng, đó là lý do tại sao`lower[:-1]`Và`upper[:-1]`được nối với nhau. Điều này tránh trùng lặp các điểm cuối đó bên trong biểu diễn thân tàu. Điểm thân đầu tiên được in riêng biệt một lần nữa vì đầu ra được yêu cầu đóng đa giác một cách rõ ràng. 

Thử nghiệm ngăn chặn chỉ sử dụng số học số nguyên. Truy vấn chỉ không an toàn nếu mọi tích chéo đều hoàn toàn dương. Tích chéo của 0 ngay lập tức trả về`False`, xử lý chính xác một cạnh hoặc đỉnh là an toàn. Số nguyên Python không bị tràn, mặc dù với giới hạn tọa độ, tích thực tế đã rất nhỏ. 

Việc thực hiện không sử dụng dấu phẩy động ở bất cứ đâu. Điều đó rất hữu ích cho vấn đề này vì một điểm có thể nằm chính xác trên một cạnh và việc so sánh dấu phẩy động có thể biến trường hợp ranh giới chính xác thành phân loại bên trong hoặc bên ngoài không chính xác. 

## Ví dụ đã hoạt động 

### Trường hợp mẫu 1 

Sáu cảm biến là:```
(-477,-180)
(31,-266)
(-474,28)
(147,49)
(323,-53)
(277,-79)
```Sau khi sắp xếp, thuật toán Andrew loại bỏ`(277,-79)`vì nó nằm trong khúc cua được hình thành bởi các đỉnh thân tàu xung quanh. Thân tàu kết quả có năm đỉnh. 

| Sân khấu | Hoạt động hiện tại | Trạng thái thân tàu | 
| --- | --- | --- | 
| Sắp xếp điểm | Xử lý từ trái sang phải |`(-477,-180), (-474,28), (31,-266), (147,49), (277,-79), (323,-53)`| 
| Thân dưới | Loại bỏ các vòng quay theo chiều kim đồng hồ/cực tuyến |`(-477,-180), (31,-266), (323,-53)`| 
| Thân trên | Quy trình ngược lại |`(-477,-180), (-474,28), (147,49), (323,-53)`| 
| Thân tàu kết hợp | Tham gia cả hai chuỗi |`(-477,-180), (31,-266), (323,-53), (147,49), (-474,28)`| 
| Đóng đa giác | Lặp lại điểm đầu tiên |`(-477,-180)`| 

Đối với năm địa điểm,`(-139,-183)`tạo ra tích chéo dương đối với mọi mép thân tàu nên hoàn toàn nằm bên trong và không an toàn. Bốn địa điểm còn lại đều ở bên ngoài hoặc trên ranh giới nên rất an toàn. Đầu ra bắt đầu lúc`(-477,-180)`, đi theo thân tàu ngược chiều kim đồng hồ và kết thúc bằng cách lặp lại điểm đó. 

### Trường hợp mẫu 2 

Năm cảm biến là:```
(-52,-325)
(104,420)
(315,356)
(-192,8)
(493,146)
```Tất cả năm điểm đều là đỉnh thân. 

| Sân khấu | Hoạt động hiện tại | Trạng thái thân tàu | 
| --- | --- | --- | 
| Sắp xếp điểm | Xử lý từ trái sang phải |`(-192,8), (-52,-325), (104,420), (315,356), (493,146)`| 
| Thân dưới | Loại bỏ các lối rẽ không trái |`(-192,8), (-52,-325), (493,146)`| 
| Thân trên | Quy trình ngược lại |`(-192,8), (104,420), (315,356), (493,146)`| 
| Thân tàu kết hợp | Tham gia cả hai chuỗi |`(-192,8), (-52,-325), (493,146), (315,356), (104,420)`| 
| Đóng đa giác | Lặp lại điểm đầu tiên |`(-192,8)`| 

Truy vấn`(404,228)`nằm hoàn toàn bên trong vì mọi cạnh đều nhìn thấy nó ở phía bên trái của nó. Do đó, nó không an toàn. Truy vấn`(-239,484)`nằm ngoài đa giác và an toàn. 

Hai dấu vết cũng cho thấy lý do tại sao thân tàu được chế tạo từ các ngã rẽ thay vì thử nghiệm mọi đa giác có thể. Các điểm không thể tồn tại trên ranh giới lồi sẽ biến mất ngay khi lượt cục bộ chứng tỏ chúng không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C log C + CP) | Việc sắp xếp chiếm ưu thế trong kết cấu thân tàu, sau đó mọi vị trí đều kiểm tra tối đa các cạnh thân tàu C | 
| Không gian | O(C) | Các điểm được sắp xếp và hai chuỗi thân chứa các điểm O(C) | 

Với C và P nhiều nhất là 50, trường hợp xấu nhất đối với một trường hợp thử nghiệm chỉ thực hiện vài nghìn phép tính tích chéo sau khi sắp xếp O(C log C). Ngay cả với 50 trường hợp thử nghiệm, điều này vẫn nằm trong giới hạn 15 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

input = sys.stdin.readline

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - \
           (b[1] - a[1]) * (c[0] - a[0])

def convex_hull(points):
    points = sorted(set(points))

    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]

def strictly_inside(hull, p):
    if len(hull) < 3:
        return False

    for i in range(len(hull)):
        if cross(hull[i], hull[(i + 1) % len(hull)], p) <= 0:
            return False

    return True

def solve():
    t = int(input())
    output = []

    for case_no in range(1, t + 1):
        c, p = map(int, input().split())
        sensors = [tuple(map(int, input().split())) for _ in range(c)]
        locations = [tuple(map(int, input().split())) for _ in range(p)]

        hull = convex_hull(sensors)

        output.append(f"Case {case_no}")

        for x, y in hull:
            output.append(f"{x} {y}")
        if hull:
            output.append(f"{hull[0][0]} {hull[0][1]}")

        for x, y in locations:
            status = "unsafe!" if strictly_inside(hull, (x, y)) else "safe!"
            output.append(f"{x} {y} is {status}")

        if case_no != t:
            output.append("")

    return "\n".join(output)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample = """\
2
6 5
-477 -180
31 -266
-474 28
147 49
323 -53
277 -79
346 488
-139 -183
-427 129
386 -222
-408 -315
5 2
-52 -325
104 420
315 356
-192 8
493 146
404 228
-239 484
"""

expected_sample = """\
Case 1
-477 -180
31 -266
323 -53
147 49
-474 28
-477 -180
346 488 is safe!
-139 -183 is unsafe!
-427 129 is safe!
386 -222 is safe!
-408 -315 is safe!

Case 2
-192 8
-52 -325
493 146
315 356
104 420
-192 8
404 228 is unsafe!
-239 484 is safe!"""

assert run(sample) == expected_sample, "provided sample"

minimum_triangle = """\
1
3 4
0 0
4 0
0 4
2 2
2 0
5 5
0 4
"""

expected_minimum_triangle = """\
Case 1
0 0
4 0
0 4
0 0
2 2 is unsafe!
2 0 is safe!
5 5 is safe!
0 4 is safe!"""

assert run(minimum_triangle) == expected_minimum_triangle, \
    "minimum-size triangle and boundary cases"

collinear = """\
1
3 3
0 0
2 0
4 0
1 0
4 0
5 0
"""

expected_collinear = """\
Case 1
0 0
4 0
0 0
1 0 is safe!
4 0 is safe!
5 0 is safe!"""

assert run(collinear) == expected_collinear, \
    "collinear sensors"

duplicates = """\
1
5 3
1 1
1 1
1 1
2 2
2 2
1 1
2 2
0 0
"""

expected_duplicates = """\
Case 1
1 1
2 2
1 1
1 1 is safe!
2 2 is safe!
0 0 is safe!"""

assert run(duplicates) == expected_duplicates, \
    "duplicate and collinear sensors"

boundary_square = """\
1
8 5
0 0
10 0
10 10
0 10
5 0
10 5
5 10
0 5
5 5
0 0
10 10
11 5
5 10
"""

expected_boundary_square = """\
Case 1
0 0
10 0
10 10
0 10
0 0
5 5 is unsafe!
0 0 is safe!
10 10 is safe!
11 5 is safe!
5 10 is safe!"""

assert run(boundary_square) == expected_boundary_square, \
    "collinear edge points and boundary queries"

all_equal = """\
1
3 3
7 7
7 7
7 7
7 7
8 8
6 7
"""

expected_all_equal = """\
Case 1
7 7
7 7
7 7 is safe!
8 8 is safe!
6 7 is safe!"""

assert run(all_equal) == expected_all_equal, \
    "all sensors equal"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình tam giác có kích thước tối thiểu | Đỉnh tam giác, bên trong không an toàn, cạnh an toàn | Số lượng cảm biến hợp lệ tối thiểu và xử lý ranh giới nghiêm ngặt | 
| Ba cảm biến cộng tuyến | Hai điểm cuối và không có vị trí không an toàn | Thân tàu thoái hóa không có nội thất | 
| Cảm biến trùng lặp | Hai điểm cuối riêng biệt | Loại bỏ trùng lặp và suy biến hình học | 
| Hình vuông có cảm biến ở mọi cạnh | Chỉ bốn đỉnh góc | Không được in các điểm thẳng hàng trên mép thân tàu | 
| Tất cả các cảm biến đều bằng nhau | Một điểm lặp lại để đóng chu vi | Suy thoái hoàn toàn và tọa độ trùng lặp | 

## Vỏ cạnh 

Quy tắc biên được xử lý trực tiếp bởi điều kiện`cross <= 0`. Đối với tam giác có cảm biến`(0,0)`,`(4,0)`, Và`(0,4)`, vị trí`(2,0)`có tích chéo bằng 0 so với cạnh đầu tiên. Hàm ngay lập tức trả về`False`, sản xuất`2 0 is safe!`. Vị trí nội địa`(2,2)`có tích chéo dương trên cả ba cạnh và do đó không an toàn. 

Đối với cảm biến cộng tuyến`(0,0)`,`(2,0)`, Và`(4,0)`, chuỗi dưới và chuỗi trên sụp đổ thành hai điểm cuối. Thân tàu cuối cùng là`[(0,0), (4,0)]`. Vì chiều dài của nó nhỏ hơn ba,`strictly_inside`trả lại`False`mà không cần thử logic cạnh đa giác. Như vậy`(1,0)`,`(4,0)`, và thậm chí`(5,0)`đều an toàn vì một đoạn thẳng không có phần bên trong hai chiều. 

Khi có nhiều cảm biến nằm trên cùng một mép thân tàu,`<= 0`điều kiện loại bỏ loại bỏ các điểm trung gian. Đối với hình vuông chứa`(5,0)`,`(10,5)`,`(5,10)`, Và`(0,5)`, mỗi điểm đó sẽ bị xóa trong khi xử lý chuỗi tương ứng. Đa giác được in chỉ chứa`(0,0)`,`(10,0)`,`(10,10)`, Và`(0,10)`, là các đỉnh thực sự của đa giác bao quanh tối thiểu. 

Đối với tọa độ trùng lặp,`sorted(set(points))`giảm các cảm biến lặp lại thành một điểm hình học. Với ba bản sao của`(7,7)`, thân tàu trở thành`[(7,7)]`. Mã được in`(7,7)`hai lần để danh sách chu vi bắt đầu và kết thúc tại cùng một điểm và mọi truy vấn đều an toàn vì không có phần bên trong bị ảnh hưởng hai chiều. 

Việc sử dụng tích chéo số nguyên cũng xử lý các đỉnh một cách chính xác. Một truy vấn bằng một đỉnh thân làm cho tích chéo có một hoặc nhiều cạnh liên quan bằng 0, do đó nó được phân loại là an toàn thay vì không an toàn. Không cần epsilon vì mọi tọa độ và mọi thao tác đều chính xác.
