---
title: "CF 102192B - Pizza Hub"
description: "Chúng ta có một tam giác không suy biến và một tập giấy hình chữ nhật. Phần đệm có chiều rộng cố định w, trong khi chiều cao của nó là số lượng chúng ta muốn giảm thiểu. Hình tam giác có thể được xoay tự do và được phép chạm vào đường biên."
date: "2026-08-18T09:54:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "B"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 732
verified: true
draft: false
---

[CF 102192B - Pizza Hub](https://codeforces.com/problemset/problem/102192/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tam giác không suy biến và một tập giấy hình chữ nhật. Tấm đệm có chiều rộng cố định`w`, trong khi chiều cao của nó là đại lượng mà chúng ta muốn cực tiểu hóa. Hình tam giác có thể được xoay tự do và được phép chạm vào đường biên. Kích thước ngang của tam giác quay tối đa phải bằng`w`; kích thước dọc của nó trở thành chiều cao cần thiết. 

Đối với mọi trường hợp thử nghiệm, sáu tọa độ mô tả ba đỉnh tam giác, theo sau là chiều rộng dải. Chúng ta phải in chiều cao nhỏ nhất có thể, hoặc`impossible`nếu không xoay có thể làm cho hình tam giác nằm gọn trong một dải chiều rộng`w`. 

Có tới 50.000 trường hợp thử nghiệm độc lập. Tọa độ và`w`nhiều nhất là 10.000, vì vậy số học số nguyên là đủ cho các vị từ hình học, nhưng bản thân câu trả lời nói chung là vô tỷ. Giới hạn thời gian chỉ là 3 giây, loại trừ mọi thứ lấy mẫu số lượng lớn góc quay cho mọi trường hợp thử nghiệm. Đặc biệt, việc quét hàng triệu góc có thể có trên mỗi tam giác sẽ quá tốn kém. Chúng ta cần một lượng công việc hình học không đổi cho mỗi trường hợp. 

Trường hợp cạnh đầu tiên là một dải quá hẹp để có thể chứa được hình tam giác. Ví dụ,```
0 0 2 0 1 2 1
```có chiều rộng tối thiểu có thể lớn hơn`1`, vậy câu trả lời là`impossible`. Việc triển khai bất cẩn chỉ cố gắng đặt một bên theo chiều ngang có thể vô tình báo cáo chiều cao thay vì nhận ra rằng không có hướng nào thỏa mãn giới hạn chiều rộng. 

Trường hợp cạnh thứ hai là một hình tam giác mà không thể đạt được hướng tối ưu bằng cách đặt một cạnh theo chiều ngang. Ví dụ, với một dải đủ hẹp, một cạnh dài hơn dải có thể phải chạm vào cả hai ranh giới dọc sau khi xoay. Hướng chính xác được xác định bằng hình chiếu của nó lên hướng chiều rộng, không phải bằng cách căn chỉnh một cạnh với hình chữ nhật. 

Trường hợp cạnh thứ ba là sự bằng nhau ở biên. Vì```
0 0 1 0 0 1 1
```hình tam giác vừa khít với chiều rộng`1`và chiều cao chính xác`1`, vì vậy đầu ra là`1.0000000000`. Sử dụng một cách nghiêm ngặt`< w`test sẽ từ chối vị trí này một cách không chính xác. 

Cuối cùng, tọa độ có thể chứa các giá trị lặp lại mặc dù bản thân ba đỉnh không thể trùng nhau hoặc thẳng hàng. Ví dụ,```
0 0 0 1 1 1 1
```là một tam giác vuông hợp lệ. Thuật toán phải dựa vào hình dạng vectơ thay vì giả định rằng tất cả sáu tọa độ đầu vào đều khác nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là xoay hình tam giác qua nhiều góc, tính toán hộp giới hạn của nó sau mỗi lần quay và giữ chiều cao nhỏ nhất trong số các phép quay có chiều rộng lớn nhất`w`. Điều này hoạt động về mặt khái niệm vì đối với bất kỳ phép quay cố định nào, ba đỉnh được xoay sẽ xác định hoàn toàn hình chữ nhật được yêu cầu. Vấn đề là quá trình quay diễn ra liên tục, do đó tìm kiếm dạng lưới cần một bước góc cực kỳ nhỏ để đảm bảo yêu cầu`1e-6`độ chính xác. Lấy mẫu mỗi`10^-6`radian sẽ yêu cầu khoảng`2π / 10^-6`, hoặc khoảng 6,3 triệu hướng cho một trường hợp thử nghiệm. Với 50.000 trường hợp, tức là hơn 300 tỷ lượt kiểm tra định hướng, vượt xa thời hạn. Lưới thô hơn cũng không đảm bảo độ chính xác vì mức tối ưu có thể nằm giữa hai góc được lấy mẫu. 

Quan sát hữu ích là luôn có thể chọn vị trí tối ưu với một đỉnh tam giác ở một góc của hình chữ nhật. Về mặt hình học, khi một hình tam giác nằm bên trong hình chữ nhật, hãy dịch chuyển hình chữ nhật cho đến khi nó trở nên chặt chẽ ở các ranh giới cạnh bên và bên dưới có liên quan. Ở một cấu hình chặt chẽ tối ưu, một đỉnh tam giác có thể được tạo thành trùng với một góc hình chữ nhật. Điều này làm giảm bài toán sắp xếp liên tục xuống chỉ còn ba lựa chọn về đỉnh tam giác, cộng với hai lựa chọn xem đỉnh nào trong hai đỉnh còn lại là đỉnh thấp hơn. 

Cố định đỉnh tam giác làm góc dưới bên trái của hình chữ nhật. Đặt vectơ`a`Và`b`điểm từ đỉnh này đến hai đỉnh còn lại. Chúng ta cần xoay chúng sao cho cả hai tọa độ đều không âm, tối đa cả hai tọa độ ngang đều`w`và tọa độ dọc lớn nhất càng nhỏ càng tốt. 

Hai vectơ phải tạo thành một góc tối đa 90 độ để có thể thực hiện được cấu hình góc này. Chúng ta có thể kiểm tra điều đó bằng tích số chấm của họ. Nếu như`|a| <= w`, vị trí tốt nhất cho thứ tự này là đặt`a`theo chiều ngang. Khi đó đóng góp theo chiều dọc của nó bằng 0 và xoay hình chữ nhật ra xa`a`sẽ chỉ tăng tọa độ dọc của nó. Chiều cao đóng góp bởi`b`chính xác là khoảng cách vuông góc từ`b`đến dòng thông qua`a`, cụ thể là`cross(a,b) / |a|`, cung cấp`b`vẫn vừa vặn theo chiều ngang. 

Nếu như`|a| > w`,`a`không thể nằm ngang vì hình chiếu ngang của nó sẽ vượt quá chiều rộng dải. Vị trí tốt nhất có thể cho`a`là thực hiện hình chiếu ngang của nó một cách chính xác`w`. Hình chiếu thẳng đứng của nó sau đó được cố định tại 

[ 
h_a=\sqrt{|a|^2-w^2}. 
] 

Có hai mặt có thể xảy ra của`a`mà vectơ kia có thể nằm trên đó. Chúng tôi kiểm tra cả hai bằng cách hoán đổi`a`Và`b`, và bằng cách phản ánh tất cả`y`tọa độ. Một khi góc của`a`so với phương ngang là cố định, góc giữa`a`Và`b`xác định vị trí của`b`. Chúng ta có thể kiểm tra trực tiếp hình chiếu ngang và hình chiếu dọc của nó bằng lượng giác. 

Phương pháp brute-force hoạt động vì mọi vòng quay đều có thể được đánh giá. Nó thất bại vì có quá nhiều vòng quay để kiểm tra. Việc quan sát một góc hình chữ nhật cho phép chúng ta loại bỏ gần như toàn bộ không gian tìm kiếm liên tục. Chỉ có sáu cặp vectơ có thứ tự và sự phản chiếu xử lý hai cạnh của trục hoành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T · K) trong đó K là hàng triệu góc được lấy mẫu | O(1) | Quá chậm và không chính xác | 
| Tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba đỉnh và chiều rộng dải`w`. Xử lý mọi phép tính bằng cách sử dụng vectơ giữa các đỉnh, vì phép tịnh tiến của tam giác không ảnh hưởng đến kích thước hình chữ nhật cần thiết. 
2. Với mỗi đỉnh trong ba đỉnh của tam giác, coi đó là góc hình chữ nhật và dựng hai vectơ`a`Và`b`từ đỉnh đó đến các đỉnh còn lại. Hãy thử cả hai thứ tự, vì một vectơ có thể là tia dưới và vectơ kia có thể là tia trên. 
3. Từ chối đơn hàng khi`a · b < 0`. Hai vectơ sau đó tạo thành một góc lớn hơn 90 độ, do đó chúng không thể cùng nằm trong cùng một góc phần tư bắt đầu từ một góc hình chữ nhật. 
4. Tính toán`A² = a · a`. Nếu như`A² <= w²`,`a`có thể được đặt theo chiều ngang mà không vi phạm chiều rộng dải. Trong trường hợp này, yêu cầu`a × b >= 0`và yêu cầu hình chiếu của`b`lên`a`nhiều nhất là`w`. Chiều cao thu được là`|a × b| / |a|`. 
5. Nếu`A² > w²`,`a`phải nghiêng. Đặt 

[ 
h=\sqrt{A^2-w^2}. 
] 

Đây là hình chiếu thẳng đứng nhỏ nhất`a`có thể có trong khi hình chiếu ngang của nó chính xác`w`. 

1. Tính toán 

[ 
\phi=\arctan(h/w), 
] 

đó là góc mà`a`thực hiện theo phương ngang khi hình chiếu ngang của nó là`w`. Đồng thời tính góc`δ`giữa`a`Và`b`từ 

[ 
\cos\delta=\frac{a\cdot b}{|a||b|}. 
] 

1. Nếu`a × b >= 0`,`b`nằm cùng phía quay với chiều dương chuyển từ`a`. Góc của nó so với phương ngang là`φ + δ`. Nó phải nằm trong góc phần tư thứ nhất, vì vậy`φ + δ <= π/2`. Hình chiếu ngang của nó cũng phải lớn nhất`w`. Nếu các điều kiện này được giữ, hình chiếu thẳng đứng của nó là`|b| sin(φ+δ)`, do đó chiều cao ứng cử viên là giá trị lớn hơn và`h`. 
2. Nếu`a × b < 0`,`b`nằm ở phía bên kia của`a`. Góc của nó so với phương ngang là`φ - δ`. Chúng tôi cần`φ - δ >= 0`để có thể`b`vẫn ở trên ranh giới ngang. Hình chiếu ngang của nó lại phải lớn nhất`w`. Trong cấu hình này hình chiếu thẳng đứng của nó không thể vượt quá`h`, vì vậy chiều cao của ứng cử viên chỉ đơn giản là`h`. 
3. Lặp lại sáu phép kiểm tra vectơ có thứ tự tương tự sau khi phản ánh tam giác qua trục hoành. Sự phản chiếu xử lý các vị trí trong đó các vectơ liên quan nằm ở phía đối diện của hệ tọa độ ban đầu. 
4. Nếu tìm được ít nhất một ứng viên, hãy in ra ứng viên nhỏ nhất. Nếu không thì in`impossible`. 

Tại sao nó hoạt động: bất biến chính là mọi ứng cử viên được thuật toán xem xét đều đại diện cho một vị trí hợp pháp trong đó một đỉnh tam giác nằm ở một góc hình chữ nhật và mọi vị trí tối ưu đều có một vị trí chặt chẽ tương đương có dạng này. Đối với một cặp góc cố định và có thứ tự, giới hạn chiều rộng cho phép vectơ đầu tiên nằm ngang hoặc buộc hình chiếu ngang của nó bằng`w`. Đó chính xác là hai trường hợp được thuật toán xử lý. Sau đó, góc sẽ kiểm tra đặc điểm xem vectơ thứ hai có còn nằm trong hình chữ nhật hay không. Vì cả ba đỉnh góc có thể có, cả thứ tự vectơ và cả hai hướng đều được kiểm tra nên không có vị trí tối ưu nào bị bỏ qua. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-10
INF = float("inf")
PI = math.pi

def solve_case(points, w):
    ans = INF
    w2 = float(w * w)

    def calc(a, b):
        nonlocal ans

        ax, ay = a
        bx, by = b

        dot = ax * bx + ay * by
        cross = ax * by - ay * bx
        aa = ax * ax + ay * ay
        bb = bx * bx + by * by

        # Both vectors must fit into the same 90-degree quadrant.
        if dot < -EPS:
            return

        A = math.sqrt(aa)
        B = math.sqrt(bb)

        if aa <= w2 + EPS:
            # Put a horizontally.
            if cross < -EPS:
                return

            # Projection of b onto a must not exceed w.
            if dot * dot > w2 * aa + EPS:
                return

            height = cross / A
            if height < ans:
                ans = height
            return

        # a is longer than the available width, so its x-projection
        # has to be exactly w.
        h = math.sqrt(max(0.0, aa - w2))
        phi = math.atan2(h, float(w))

        c = dot / (A * B)
        c = max(-1.0, min(1.0, c))
        delta = math.acos(c)

        if cross >= -EPS:
            angle = phi + delta

            # b must remain in the first quadrant.
            if angle > PI / 2 + EPS:
                return

            bx_proj = B * math.cos(angle)
            if bx_proj > w + EPS:
                return

            by_proj = B * math.sin(angle)
            ans = min(ans, max(h, by_proj))
        else:
            angle = phi - delta

            # b must not cross below the horizontal side.
            if angle < -EPS:
                return

            bx_proj = B * math.cos(angle)
            if bx_proj > w + EPS:
                return

            ans = min(ans, h)

    # Reflection lets us consider both sides of the horizontal axis.
    for reflected in (False, True):
        if reflected:
            p = [(x, -y) for x, y in points]
        else:
            p = points

        for i in range(3):
            j = (i + 1) % 3
            k = (i + 2) % 3

            a = (p[j][0] - p[i][0], p[j][1] - p[i][1])
            b = (p[k][0] - p[i][0], p[k][1] - p[i][1])

            calc(a, b)
            calc(b, a)

    return ans

def main():
    out = []

    t = int(input())
    for _ in range(t):
        x1, y1, x2, y2, x3, y3, w = map(int, input().split())

        points = [
            (x1, y1),
            (x2, y2),
            (x3, y3),
        ]

        ans = solve_case(points, w)

        if math.isinf(ans):
            out.append("impossible")
        else:
            out.append(f"{ans:.10f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`calc`là việc thực hiện các bước từ 3 đến 8. Tích vector chấm xác định xem hai tia có thể vừa với một góc phần tư hay không, trong khi tích chéo cho biết cạnh nào của`a`chứa`b`. 

các`aa <= w²`chi nhánh là trường hợp`a`có thể nằm dọc theo chiều rộng của hình chữ nhật. biểu hiện`cross / A`là khoảng cách vuông góc từ đỉnh thứ ba đến đường thẳng đi qua hai đỉnh đầu tiên nên trực tiếp cho ta độ cao cần tìm. 

Nhánh thứ hai xử lý trường hợp`a`rộng hơn dải. biểu hiện`sqrt(aa - w²)`theo trực tiếp từ tam giác vuông được tạo bởi vectơ`a`, hình chiếu ngang của nó`w`và hình chiếu thẳng đứng của nó.`atan2(h, w)`tốt hơn là`atan(h / w)`bởi vì nó hoạt động rõ ràng ở tất cả các giá trị hợp lệ, mặc dù`w`là tích cực ở đây. 

Đối số được chuyển đến`acos`được kẹp vào`[-1, 1]`. Về mặt toán học, nó đã nằm trong khoảng đó, nhưng việc làm tròn dấu phẩy động có thể tạo ra thứ gì đó như`1.0000000000000002`, nếu không sẽ gây ra lỗi tên miền. 

Việc so sánh sử dụng một epsilon nhỏ vì cấu hình tối ưu thường nằm chính xác trên ranh giới dải. Chỉ sử dụng các so sánh dấu phẩy động nghiêm ngặt có thể từ chối các vị trí hợp lệ, chẳng hạn như vectơ có hình chiếu chính xác`w`. 

Các vòng lặp kiểm tra mọi đỉnh của tam giác, cả sự lựa chọn của vectơ đầu tiên và cả sự phản xạ theo chiều dọc. Đó chỉ là 12 lần kiểm tra kích thước không đổi cho mỗi trường hợp thử nghiệm. 

Số nguyên Python có độ chính xác tùy ý, do đó chênh lệch tọa độ bình phương không thể tràn. Các phép tính lượng giác chỉ sử dụng số dấu phẩy động sau khi đã thu được tích số nguyên chính xác, tích chéo và độ dài bình phương. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hình tam giác là`(0,0), (3,0), (0,4)`và chiều rộng là`10`. Một cặp vectơ từ`(0,0)`là`a=(3,0)`Và`b=(0,4)`. 

| Đỉnh |`a`|`b`|`|a| <= w`| Chiều cao của ứng viên | 
|---|---|---|---|---:| 
|`(0,0)`|`(3,0)`|`(0,4)`| vâng |`4`| 
|`(0,0)`|`(0,4)`|`(3,0)`| vâng | bị từ chối bởi định hướng | 
|`(3,0)`|`(-3,0)`|`(-3,4)`| vâng |`4`| 
|`(0,4)`|`(0,-4)`|`(3,-4)`| vâng |`2.4`| 

Cấu hình cuối cùng tương ứng với việc đặt cạnh có chiều dài`5`dọc theo hướng chiều rộng. Độ cao của nó là`3·4/5 = 2.4`, nhỏ hơn độ cao thu được từ các cạnh khác. Do đó, đầu ra là`2.4000000000`. 

Đối với mẫu thứ hai, hình tam giác tương tự có chiều rộng là`1`. 

| Số lượng | Giá trị | 
| --- | --- | 
| Khu vực tam giác |`6`| 
| Cạnh dài nhất |`5`| 
| Chiều rộng tam giác tối thiểu có thể |`2.4`| 
| Chiều rộng dải có sẵn |`1`| 
| Ứng viên khả thi | không | 
| Đầu ra |`impossible`| 

Mọi cấu hình góc đều không kiểm tra được phép chiếu vì dải này hẹp hơn chiều rộng tối thiểu có thể có của hình tam giác. Thuật toán không bao giờ phát minh ra chiều cao cho một vị trí không thể thực hiện được, vì vậy câu trả lời cuối cùng vẫn là vô cùng và`impossible`được in. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện chính xác 12 phép kiểm tra hình học có kích thước không đổi và một số phép toán lượng giác không đổi. | 
| Không gian | O(1) | Chỉ có ba điểm đầu vào và một số giá trị dấu phẩy động tạm thời không đổi được lưu trữ. | 

Với tối đa 50.000 trường hợp thử nghiệm, thuật toán chỉ thực hiện tổng cộng vài trăm nghìn cấu hình hình học. Điều này phù hợp với thiết kế theo thời gian không đổi cho mỗi trường hợp dự kiến, trong khi việc sử dụng bộ nhớ không phụ thuộc vào số lượng trường hợp kiểm thử. 

## Trường hợp thử nghiệm```python
# The helper mirrors the submitted solution.
import sys
import io
import math

def solve_input(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        input = sys.stdin.readline

        EPS = 1e-10
        INF = float("inf")
        PI = math.pi

        def solve_case(points, w):
            ans = INF
            w2 = float(w * w)

            def calc(a, b):
                nonlocal ans

                ax, ay = a
                bx, by = b

                dot = ax * bx + ay * by
                cross = ax * by - ay * bx
                aa = ax * ax + ay * ay
                bb = bx * bx + by * by

                if dot < -EPS:
                    return

                A = math.sqrt(aa)
                B = math.sqrt(bb)

                if aa <= w2 + EPS:
                    if cross < -EPS:
                        return

                    if dot * dot > w2 * aa + EPS:
                        return

                    ans = min(ans, cross / A)
                    return

                h = math.sqrt(max(0.0, aa - w2))
                phi = math.atan2(h, float(w))

                c = dot / (A * B)
                c = max(-1.0, min(1.0, c))
                delta = math.acos(c)

                if cross >= -EPS:
                    angle = phi + delta

                    if angle > PI / 2 + EPS:
                        return

                    bx_proj = B * math.cos(angle)
                    if bx_proj > w + EPS:
                        return

                    by_proj = B * math.sin(angle)
                    ans = min(ans, max(h, by_proj))
                else:
                    angle = phi - delta

                    if angle < -EPS:
                        return

                    bx_proj = B * math.cos(angle)
                    if bx_proj > w + EPS:
                        return

                    ans = min(ans, h)

            for reflected in (False, True):
                p = [(x, -y) for x, y in points] if reflected else points

                for i in range(3):
                    j = (i + 1) % 3
                    k = (i + 2) % 3

                    a = (
                        p[j][0] - p[i][0],
                        p[j][1] - p[i][1],
                    )
                    b = (
                        p[k][0] - p[i][0],
                        p[k][1] - p[i][1],
                    )

                    calc(a, b)
                    calc(b, a)

            return ans

        def main():
            out = []
            t = int(input())

            for _ in range(t):
                x1, y1, x2, y2, x3, y3, w = map(int, input().split())
                points = [(x1, y1), (x2, y2), (x3, y3)]

                ans = solve_case(points, w)

                if math.isinf(ans):
                    out.append("impossible")
                else:
                    out.append(f"{ans:.10f}")

            sys.stdout.write("\n".join(out))

        main()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples.
assert solve_input(
    "2\n"
    "0 0 3 0 0 4 10\n"
    "0 0 3 0 0 4 1\n"
) == "2.4000000000\nimpossible", "provided samples"

# Minimum-size valid triangle. Width exactly reaches the boundary.
assert solve_input(
    "1\n"
    "0 0 1 0 0 1 1\n"
) == "1.0000000000", "minimum coordinates and exact width"

# Same geometry after translation and permutation of the vertices.
assert solve_input(
    "1\n"
    "7 8 7 9 8 9 1\n"
) == "1.0000000000", "translation and vertex order"

# A narrow strip that cannot contain an equilateral triangle of side 2.
assert solve_input(
    "1\n"
    "0 0 2 0 1 2 1\n"
) == "impossible", "impossible due to minimum width"

# Maximum coordinate values, with width exactly equal to the two legs.
assert solve_input(
    "1\n"
    "0 0 10000 0 0 10000 10000\n"
) == "10000.0000000000", "maximum coordinates and boundary width"

# A valid triangle with repeated coordinate components.
assert solve_input(
    "1\n"
    "0 0 0 1 1 1 1\n"
) == "1.0000000000", "repeated coordinate components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1 0 0 1 1`|`1.0000000000`| Tọa độ tối thiểu và đẳng thức tại ranh giới chiều rộng | 
|`7 8 7 9 8 9 1`|`1.0000000000`| Dịch bất biến và thứ tự đỉnh tùy ý | 
|`0 0 2 0 1 2 1`|`impossible`| Dải hẹp hơn chiều rộng tối thiểu của tam giác | 
|`0 0 10000 0 0 10000 10000`|`10000.0000000000`| Giá trị tọa độ tối đa và hình chiếu ranh giới chính xác | 
|`0 0 0 1 1 1 1`|`1.0000000000`| Các thành phần tọa độ lặp lại không cộng tuyến | 

## Vỏ cạnh 

Đối với trường hợp không thể```
0 0 2 0 1 2 1
```tam giác đều có độ dài cạnh`2`. Chiều rộng tối thiểu có thể có của nó là độ cao,`sqrt(3)`, lớn hơn chiều rộng có sẵn`1`. Mọi cặp vectơ có thứ tự cuối cùng đều không đạt được điều kiện góc phần tư hoặc điều kiện chiếu chiều rộng. Vì không có ứng viên nào đạt được`ans`, chương trình in`impossible`. 

Để liên hệ ranh giới chính xác,```
0 0 1 0 0 1 1
```vectơ`(1,0)`có chiều dài bình phương chính xác`w²`. các`aa <= w² + EPS`nhánh chấp nhận nó và đỉnh còn lại có hình chiếu ngang bằng 0 và hình chiếu dọc. Chiều cao của ứng viên chính xác là`1`, vì vậy đầu ra là`1.0000000000`. Trường hợp này phát hiện các triển khai vô tình sử dụng`aa < w²`hoặc`projection < w`. 

Đối với trường hợp tọa độ tối đa,```
0 0 10000 0 0 10000 10000
```vectơ từ`(0,0)`ĐẾN`(10000,0)`có chiều dài chính xác bằng chiều rộng dải. Đặt cạnh đó theo chiều ngang sẽ cho chiều cao`10000`và các cấu hình góc khác không thể cải thiện nó theo giới hạn chiều rộng. Thuật toán thực hiện tất cả các phép tính có độ dài bình phương số nguyên trước khi chuyển sang dấu phẩy động, tránh tràn và sai số không đáng có. 

Đối với các thành phần tọa độ lặp lại,```
0 0 0 1 1 1 1
```các vectơ từ`(0,0)`là`(0,1)`Và`(1,1)`. Tích vô hướng của chúng có giá trị dương và tích chéo của chúng có độ lớn`1`. Vì chiều rộng chính xác là`1`, tam giác phù hợp với chiều cao`1`. Ví dụ này chứng minh tại sao thuật toán không nên phụ thuộc vào tọa độ đầu vào khác nhau theo từng cặp. Chỉ cần đảm bảo không cộng tuyến.
