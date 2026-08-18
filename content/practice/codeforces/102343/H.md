---
title: "CF 102343H - Hướng núi"
description: "Đường viền núi là một hàm tuyến tính từng phần. Đầu vào đưa ra các đỉnh của nó từ trái sang phải dưới dạng các điểm (xi, yi) và giữa các điểm liên tiếp, ngọn núi là đường thẳng nối chúng. Bên ngoài phạm vi nhất định, độ cao bằng không."
date: "2026-08-17T10:19:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "H"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 127
verified: true
draft: false
---

[CF 102343H - Chế độ xem núi](https://codeforces.com/problemset/problem/102343/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đường viền núi là một hàm tuyến tính từng phần. Đầu vào cho các đỉnh của nó từ trái sang phải dưới dạng điểm`(x_i, y_i)`, và giữa các điểm liên tiếp ngọn núi là đường thẳng nối chúng. Bên ngoài phạm vi nhất định, độ cao bằng không. Một máy ảnh chọn một khoảng ngang có chiều rộng cố định`W`, cụ thể là`[x, x + W]`và chúng tôi muốn độ cao trung bình lớn nhất có thể có trong khoảng đó. Vì mọi bức ảnh đều có chiều rộng như nhau nên việc tối đa hóa mức trung bình hoàn toàn giống với việc tối đa hóa diện tích dưới ngọn núi bên trong khoảng đó. Những ràng buộc chính thức là`n <= 10^5`,`x_i <= 10^9`,`y_i <= 10^4`, Và`W <= 10^9`. 

Giá trị lớn của`n`loại trừ các thuật toán so sánh rõ ràng tất cả các cặp đoạn núi. MỘT`O(n^2)`phương pháp sẽ thực hiện đại khái`n(n-1)/2`, hầu hết`5 * 10^9`, cặp kiểm tra tại`n = 10^5`, vượt xa những gì giới hạn bảy giây có thể hỗ trợ. Tọa độ cũng lớn nên việc rời rạc hóa trục x không phải là một lựa chọn. Chúng ta cần làm việc trực tiếp với hàm tuyến tính từng phần liên tục. 

Có ba tình huống ranh giới thường gây ra việc triển khai không chính xác. Đầu tiên, khoảng thời gian tốt nhất có thể bắt đầu hoặc kết thúc chính xác ở đỉnh núi. Ví dụ, với`W = 1`và điểm`(0, 10), (1, 20)`, khoảng`[0, 1]`có mức trung bình`15`, vì vậy đầu ra đúng là`15.000000000`. Việc triển khai chỉ tìm kiếm nghiêm ngặt bên trong các phân đoạn có thể bỏ lỡ mức tối ưu. 

Thứ hai, vị trí xuất phát tốt nhất không nhất thiết phải là đỉnh núi hoặc đỉnh dịch chuyển. Coi như```
3 2
0 0
2 10
4 0
```Khoảng thời gian`[1, 3]`có diện tích`15`, do đó trung bình`7.5`. Đầu ra đúng là`7.500000000`. Tìm kiếm chỉ kiểm tra vị trí`x_i`Và`x_i-W`sẽ kiểm tra`0`,`2`, Và`4`và bỏ lỡ sự tối ưu thực sự tại`x = 1`. 

Thứ ba, một phần khoảng thời gian của camera có thể nằm bên ngoài ngọn núi được cung cấp. Vì```
2 10
0 10
1 20
```toàn bộ ngọn núi được chứa trong`[0, 10]`. Diện tích của nó là diện tích tam giác/hình thang`15`, vậy câu trả lời là`1.500000000`. Xử lý điểm đầu vào cuối cùng như thể ngọn núi tiếp tục với độ dốc cuối cùng sẽ tạo ra một câu trả lời hoàn toàn khác. Tuyên bố xác định rõ ràng độ cao bên ngoài cảnh quan được cung cấp bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể xử lý mọi phân đoạn của`y(x)`và mọi phân đoạn của hàm dịch chuyển`y(x + W)`như một cặp có thể. Trong một khoảng mà cả hai đều là hàm tuyến tính cố định, hiệu của chúng là tuyến tính, do đó hàm diện tích có thể được xử lý chính xác bằng phép tính cơ bản. Khó khăn là một phân đoạn của hàm đầu tiên có thể chồng lên nhiều phân đoạn đã dịch chuyển của hàm thứ hai và trong trường hợp xấu nhất có`O(n^2)`những cặp như vậy. Với`n = 10^5`, điều đó có nghĩa là khoảng`5 * 10^9`các mối quan hệ cặp, thậm chí trước cả khi tính toán số học được thực hiện cho mỗi mối quan hệ. 

Quan sát quan trọng là chúng ta không bao giờ cần xem xét các cặp phân đoạn tùy ý. Định nghĩa`F(x) = integral from x to x+W of y(t) dt`. 

Giá trị trung bình mong muốn là`F(x) / W`, Và`W`là cố định, do đó tối đa hóa mức trung bình có nghĩa là tối đa hóa`F(x)`. 

Theo định lý cơ bản của giải tích,`F'(x) = y(x + W) - y(x)`. 

Điều này thay đổi vấn đề một cách đáng kể. Các vị trí duy nhất có công thức cho`F'(x)`có thể thay đổi là những vị trí trong đó`x`đi qua một đỉnh núi ban đầu hoặc`x + W`vượt qua một. Những vị trí đó chính xác`x_i`Và`x_i - W`. 

Sau khi sắp xếp các vị trí sự kiện này, mỗi khoảng thời gian giữa hai sự kiện liên tiếp có cấu trúc rất đơn giản. Cả hai`y(x)`Và`y(x+W)`là tuyến tính ở đó, vì vậy`F'(x)`là tuyến tính và`F(x)`là bậc hai. 

Một phương trình bậc hai trên một khoảng chỉ có hai cách để đạt cực đại. Nó có thể đạt được điều đó tại một điểm cuối, hoặc, khi phương trình bậc hai lõm, tại điểm đứng yên của nó. Do đó, mỗi khoảng thời gian sự kiện chỉ yêu cầu hai điểm cuối và nhiều nhất là một ứng cử viên bên trong. 

Bản thân diện tích có thể được đánh giá theo thời gian không đổi sau khi xây dựng tích phân tiền tố của ngọn núi. Các vị trí sự kiện có thể được sắp xếp trong`O(n log n)`và mỗi sự kiện có thể định vị đoạn núi có liên quan bằng tìm kiếm nhị phân. Điều này mang lại một`O(n log n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng dãy núi tuyến tính từng đoạn và mảng vùng tiền tố. Với mọi đỉnh`x_i`, lưu trữ diện tích dưới núi từ đỉnh đầu tiên đến`x_i`. Điều này cho phép chúng ta tính diện tích trong bất kỳ khoảng nào`[a,b]`BẰNG`A(b) - A(a)`. 
2. Xác định`F(x) = A(x + W) - A(x)`. Đây chính xác là khu vực được chụp bởi một camera có cạnh trái nằm ở vị trí`x`, do đó tối đa hóa`F`giải quyết vấn đề ban đầu. 
3. Tạo tất cả các vị trí sự kiện`0`, mọi`x_i`, và mọi không âm`x_i - W`. Nhóm đầu tiên đánh dấu vị trí`y(x)`thay đổi đoạn tuyến tính của nó. Nhóm dịch chuyển thứ hai đánh dấu vị trí`y(x + W)`thay đổi phân khúc của nó. vị trí`0`được bao gồm vì máy ảnh bắt đầu trên trục x không âm. 
4. Sắp xếp và sao chép các vị trí sự kiện. Giữa hai sự kiện liên tiếp`[L,R]`, không`y(x)`cũng không`y(x+W)`đi qua một đỉnh nên cả hai hàm đều tuyến tính trong khoảng đó. 
5. Đánh giá`F(L)`Và`F(R)`cho mỗi khoảng thời gian sự kiện. Mọi mức tối đa toàn cầu xảy ra tại một sự kiện hiện đều được bảo hiểm. 
6. Bên trong khoảng`[L,R]`, thu được độ dốc của`y(x)`và độ dốc của`y(x+W)`bằng cách đánh giá cả hai ở điểm giữa. Sự khác biệt của chúng là độ dốc của`F'(x)`. Từ`F'(x)`là tuyến tính, độ dốc của nó không đổi trong toàn bộ khoảng. 
7. Nếu độ dốc của`F'`là tiêu cực,`F`bị lõm trên`[L,R]`. Điểm dừng của nó có thể là điểm cực đại bên trong. Tính số 0 của`F'(x)`và đánh giá`F`ở đó nếu nó nằm trong khoảng. Nếu độ dốc không âm,`F`là tuyến tính hoặc lồi, vì vậy giá trị cực đại của nó trên khoảng đã ở điểm cuối. 
8. Giữ diện tích lớn nhất được tìm thấy và chia cho`W`khi in câu trả lời. In chín chữ số sau dấu thập phân là đủ cho yêu cầu`10^-6`sức chịu đựng. 

Bất biến đằng sau thuật toán là mọi bộ tối đa hóa có thể đều nằm ở một vị trí sự kiện hoặc bên trong chính xác một khoảng thời gian sự kiện. Trên mỗi khoảng như vậy,`F'(x)`là tuyến tính, vì vậy`F(x)`là bậc hai. Một phương trình bậc hai không có loại cực đại địa phương nào khác. Do đó, việc kiểm tra các điểm cuối và điểm dừng duy nhất khi phương trình bậc hai lõm sẽ bao trùm mọi mức tối đa toàn cục có thể có. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

def solve():
    input = sys.stdin.readline

    n, W = map(int, input().split())
    xs = [0] * n
    ys = [0] * n

    for i in range(n):
        xs[i], ys[i] = map(int, input().split())

    # pref[i] = area under the mountain from xs[0] to xs[i].
    pref = [0.0] * n
    for i in range(1, n):
        dx = xs[i] - xs[i - 1]
        pref[i] = pref[i - 1] + dx * (ys[i - 1] + ys[i]) * 0.5

    total_area = pref[-1]

    def area(t):
        """Integral of y from xs[0] to t."""
        if t <= xs[0]:
            return 0.0
        if t >= xs[-1]:
            return total_area

        i = bisect_right(xs, t) - 1
        dx = t - xs[i]
        slope = (ys[i + 1] - ys[i]) / (xs[i + 1] - xs[i])

        return pref[i] + ys[i] * dx + 0.5 * slope * dx * dx

    def value_slope(t):
        """Return y(t) and the slope of y at t."""
        if t < xs[0] or t > xs[-1]:
            return 0.0, 0.0

        if t == xs[-1]:
            return float(ys[-1]), 0.0

        i = bisect_right(xs, t) - 1
        dx = t - xs[i]
        slope = (ys[i + 1] - ys[i]) / (xs[i + 1] - xs[i])
        value = ys[i] + slope * dx

        return value, slope

    # The derivative can change only when x or x + W
    # reaches an input vertex.
    events = {0}
    for x in xs:
        events.add(x)
        if x >= W:
            events.add(x - W)

    events = sorted(events)

    def captured_area(x):
        return area(x + W) - area(x)

    best = captured_area(0)

    for k in range(len(events) - 1):
        L = events[k]
        R = events[k + 1]

        if L == R:
            continue

        best = max(best, captured_area(L), captured_area(R))

        mid = (L + R) * 0.5

        y1, s1 = value_slope(mid)
        y2, s2 = value_slope(mid + W)

        # F'(x) = y(x + W) - y(x)
        # Its slope is s2 - s1.
        derivative_slope = s2 - s1

        derivative_at_mid = y2 - y1
        derivative_at_left = (
            derivative_at_mid
            + derivative_slope * (L - mid)
        )

        # F is concave exactly when F' is decreasing.
        if derivative_slope < 0.0:
            root = L - derivative_at_left / derivative_slope

            if L < root < R:
                best = max(best, captured_area(root))

    print(f"{best / W:.9f}")

if __name__ == "__main__":
    solve()
```Mảng tiền tố lưu trữ các khu vực hình thang. Giữa các đỉnh liên tiếp, ngọn núi có phương trình`y(x) = y_i + s(x-x_i)`, do đó diện tích trên một phần đoạn là`y_i * dx + s * dx² / 2`. các`area`hàm kết hợp hình thang một phần đó với diện tích tiền tố đã tích lũy. 

các`value_slope`hàm trả về cả độ cao và độ dốc tại tọa độ. Bên ngoài ngọn núi được cung cấp, cả hai đều bằng 0, xử lý độ cao bằng 0 cần thiết ngoài các điểm cuối. Ở đỉnh cuối cùng, độ dốc bên phải cũng bằng 0 vì cảnh quan kết thúc ở đó. 

Tập sự kiện chứa`x_i - W`chỉ khi nó không âm vì cạnh trái của máy ảnh bị giới hạn ở trục x không âm. Bao gồm`x_i - W`là những gì ghi lại những thay đổi do cạnh phải của máy ảnh băng qua đỉnh núi. 

Điểm giữa được sử dụng để thu được hệ số góc trong khoảng thời gian sự kiện. Nó không thể trùng với một sự kiện, do đó không có sự mơ hồ về việc nên sử dụng đoạn tuyến tính nào. Đạo hàm tại điểm cuối bên trái sau đó thu được bằng cách di chuyển từ điểm giữa dọc theo hệ số góc đạo hàm không đổi. 

biểu hiện`root = L - derivative_at_left / derivative_slope`trực tiếp từ việc giải phương trình tuyến tính`F'(x) = 0`. Chúng tôi chỉ kiểm tra gốc này khi`F'`đang giảm, vì chỉ khi đó điểm đứng yên tương ứng mới là điểm cực đại chứ không phải điểm cực tiểu. 

Tất cả các phép tính tọa độ và diện tích đều phù hợp thoải mái trong các biểu diễn số nguyên và dấu phẩy động của Python. Diện tích lớn nhất có thể có là theo thứ tự`10^13`, trong khi độ chính xác kép cung cấp nhiều hơn đáng kể độ chính xác cần thiết cho sai số tuyệt đối hoặc tương đối của`10^-6`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
4 20
0 10
20 20
30 5
60 30
```các vị trí sự kiện là`0, 10, 20, 30, 40, 60`. Quá trình quét liên quan có thể được tóm tắt như sau. 

| Khoảng thời gian | Loại ứng viên | Khu vực bị bắt | Trung bình | 
| --- | --- | --- | --- | 
|`[0, 10]`| điểm cuối / điểm dừng | nhiều nhất là 400 | nhiều nhất là 20 | 
|`[10, 20]`| điểm cuối / điểm dừng | nhiều nhất là 400 | nhiều nhất là 20 | 
|`[20, 30]`| điểm cuối / điểm dừng | nhiều nhất là 400 | nhiều nhất là 20 | 
|`[30, 40]`| điểm cuối / điểm dừng | nhiều nhất là 400 | nhiều nhất là 20 | 
|`[40, 60]`| điểm cuối`x = 40`|`1300 / 3`|`65 / 3`| 

Khoảng cuối cùng chứa tối ưu. Máy ảnh bao gồm`[40,60]`, nơi ngọn núi tăng dần theo độ cao`13.333...`ĐẾN`30`. Diện tích của nó là`433.333...`và chia cho chiều rộng`20`cho`21.666666667`, phù hợp với mẫu chính thức. 

### Mẫu 2 

cho```
3 1
10 50
90 50
1000 49
```ngọn núi nằm ngang ở độ cao`50`cho gần như toàn bộ khu vực hữu ích. Các vị trí sự kiện có liên quan ở gần đầu và cuối bao gồm`0`,`10`,`89`,`90`,`999`, Và`1000`. 

| Khoảng thời gian | Vùng camera | Mức trung bình liên quan tối đa | 
| --- | --- | --- | 
|`[0, 10]`| một phần bên ngoài, sau đó là chiều cao 50 | dưới 50 | 
|`[10, 89]`| hoàn toàn ở độ cao 50 | 50 | 
|`[89, 90]`| hoàn toàn ở độ cao 50 | 50 | 
|`[90, 999]`| bắt đầu bao gồm phần giảm dần | nhiều nhất là 50 | 
|`[999, 1000]`| bao gồm chiều cao thấp hơn | dưới 50 | 

Bất kỳ khoảng chiều rộng một nằm hoàn toàn bên trong phần ngang có giá trị trung bình chính xác`50`, vậy câu trả lời là`50.000000000`. Ví dụ này cũng chứng minh tại sao mức cực đại không nhất thiết phải xảy ra ở đỉnh cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Có các sự kiện O(n), chi phí sắp xếp là O(n log n) và mỗi sự kiện sử dụng tìm kiếm nhị phân O(log n). | 
| Không gian | O(n) | Các đỉnh, vùng tiền tố và vị trí sự kiện đều sử dụng bộ nhớ O(n). | 

Với`n <= 10^5`, thuật toán chỉ thực hiện số phép tính logarit cho mỗi sự kiện thay vì hàng tỷ lần kiểm tra theo cặp. Giới hạn tọa độ không ảnh hưởng đến số lượng sự kiện, do đó thời gian chạy vẫn được kiểm soát ngay cả khi đạt đến tọa độ x`10^9`. Giới hạn thời gian chính thức là bảy giây và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run the submitted solution on an input string
import sys
import io
from bisect import bisect_right

def solve():
    input = sys.stdin.readline

    n, W = map(int, input().split())
    xs = [0] * n
    ys = [0] * n

    for i in range(n):
        xs[i], ys[i] = map(int, input().split())

    pref = [0.0] * n
    for i in range(1, n):
        dx = xs[i] - xs[i - 1]
        pref[i] = pref[i - 1] + dx * (ys[i - 1] + ys[i]) * 0.5

    total_area = pref[-1]

    def area(t):
        if t <= xs[0]:
            return 0.0
        if t >= xs[-1]:
            return total_area

        i = bisect_right(xs, t) - 1
        dx = t - xs[i]
        slope = (ys[i + 1] - ys[i]) / (xs[i + 1] - xs[i])
        return pref[i] + ys[i] * dx + 0.5 * slope * dx * dx

    def value_slope(t):
        if t < xs[0] or t > xs[-1]:
            return 0.0, 0.0
        if t == xs[-1]:
            return float(ys[-1]), 0.0

        i = bisect_right(xs, t) - 1
        dx = t - xs[i]
        slope = (ys[i + 1] - ys[i]) / (xs[i + 1] - xs[i])
        return ys[i] + slope * dx, slope

    events = {0}
    for x in xs:
        events.add(x)
        if x >= W:
            events.add(x - W)
    events = sorted(events)

    def captured(x):
        return area(x + W) - area(x)

    best = captured(0)

    for k in range(len(events) - 1):
        L, R = events[k], events[k + 1]

        best = max(best, captured(L), captured(R))

        mid = (L + R) * 0.5
        y1, s1 = value_slope(mid)
        y2, s2 = value_slope(mid + W)

        ds = s2 - s1
        dm = y2 - y1
        dL = dm + ds * (L - mid)

        if ds < 0.0:
            root = L - dL / ds
            if L < root < R:
                best = max(best, captured(root))

    print(f"{best / W:.9f}")

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

# Provided sample 1
assert run(
    """4 20
0 10
20 20
30 5
60 30
"""
) == "21.666666667\n"

# Provided sample 2
assert run(
    """3 1
10 50
90 50
1000 49
"""
) == "50.000000000\n"

# Minimum-size input.
assert run(
    """2 1
0 10
1 20
"""
) == "15.000000000\n"

# Interior stationary point, catches implementations that only inspect events.
assert run(
    """3 2
0 0
2 10
4 0
"""
) == "7.500000000\n"

# Camera width is much larger than the mountain.
assert run(
    """2 10
0 10
1 20
"""
) == "1.500000000\n"

# All equal values.
assert run(
    """4 3
0 7
5 7
10 7
15 7
"""
) == "7.000000000\n"

# Maximum-size input, generated compactly.
n = 100000
parts = [f"{n} 500"]
parts.extend(f"{i} 7" for i in range(n))
large_input = "\n".join(parts) + "\n"

assert run(large_input) == "7.000000000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 0 10 / 1 20`|`15.000000000`| Đầu vào tối thiểu và điểm cuối tối ưu | 
|`3 2 / 0 0 / 2 10 / 4 0`|`7.500000000`| Điểm cố định nội thất | 
|`2 10 / 0 10 / 1 20`|`1.500000000`| Cảnh kết thúc trước khoảng thời gian của camera | 
|`4 3 / 0 7 / 5 7 / 10 7 / 15 7`|`7.000000000`| Chiều cao hoàn toàn bằng nhau | 
|`100000`đỉnh có chiều cao bằng nhau |`7.000000000`| Tối đa`n`và hiệu suất tiệm cận | 

## Vỏ cạnh 

Đối với trường hợp điểm cuối```
2 1
0 10
1 20
```tập sự kiện là`{0, 1}`. Thuật toán đánh giá cả hai vị trí camera. Tại`x = 0`, diện tích được chụp là hình thang có chiều cao`10`Và`20`, cho diện tích`15`và trung bình`15`. Không có cực đại bên trong bị thiếu vì hàm diện tích chỉ là bậc hai trên các khoảng biến cố, và ở đây khoảng liên quan không có điểm dừng nào tốt hơn. 

Đối với nội thất tối đa```
3 2
0 0
2 10
4 0
```tập sự kiện là`{0, 2, 4}`. TRÊN`[0,2]`,`y(x)`tăng theo độ dốc`5`, trong khi`y(x+2)`rơi có độ dốc`-5`. Kể từ đây`F'(x)`có độ dốc`-10`. Tại`x = 0`,`F'(0) = 10`, và số 0 của nó là tại`x = 1`. Thuật toán đánh giá điểm đó và lấy diện tích`15`, cho kết quả trung bình`7.5`. Đây chính xác là trường hợp đánh bại một thuật toán chỉ kiểm tra các điểm dừng. 

Đối với máy ảnh rộng hơn phong cảnh,```
2 10
0 10
1 20
```tập sự kiện chứa`0`Và`1`. Sau đó`x = 1`, cạnh trái đã vượt ra ngoài ngọn núi nên diện tích chiếm được bằng không. Tại`x = 0`, camera bao phủ toàn bộ ngọn núi và chín đơn vị địa hình có độ cao bằng không. Diện tích là`15`, vậy trung bình là`1.5`. Phần mở rộng bằng 0 ngoài điểm cuối cùng được xử lý trực tiếp bởi`area(t)`. 

Để có độ cao bằng nhau,```
4 3
0 7
5 7
10 7
15 7
```cả hai`y(x)`Và`y(x+W)`có độ dốc bằng 0 ở bất cứ nơi nào máy ảnh nằm hoàn toàn trên núi. Do đó`F'(x) = 0`và mọi vị trí như vậy đều tối ưu. Thuật toán không cần trường hợp đặc biệt cho việc này. Vì độ dốc đạo hàm cũng bằng 0 nên nó chỉ giữ các giá trị điểm cuối, tất cả đều có cùng diện tích. 

Đối với trường hợp kích thước tối đa, đầu vào được tạo chứa`100000`đỉnh và độ cao không đổi của`7`. Mọi vị trí camera ở trong núi đều ghi lại chính xác`7 * W`khu vực, vì vậy câu trả lời là chính xác`7.000000000`. Việc xây dựng sự kiện và tìm kiếm nhị phân vẫn còn`O(n log n)`, đó là thang đo dự định cho chính thức`n <= 10^5`hạn chế.
