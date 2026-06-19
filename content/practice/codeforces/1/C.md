---
title: "CF 1C - Xiếc Berland cổ đại"
description: "Chúng ta được cho tọa độ ba đỉnh của một số đa giác đều. Bản thân đa giác là không xác định: chúng ta không biết nó có bao nhiêu cạnh, tâm của nó ở đâu hoặc ba điểm tương ứng với đỉnh nào."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 1"
rating: 2100
weight: 1
solve_time_s: 100
verified: true
draft: false
---
## Hiểu vấn đề 

Chúng ta được cho tọa độ ba đỉnh của một số đa giác đều. Bản thân đa giác là không xác định: chúng ta không biết nó có bao nhiêu cạnh, tâm của nó ở đâu hoặc ba điểm tương ứng với đỉnh nào. Trong số tất cả các đa giác đều có thể chứa ba điểm này làm đỉnh, chúng ta phải tìm điểm có diện tích nhỏ nhất. 

Thực tế hình học quan trọng là mọi đa giác đều có một đường tròn ngoại tiếp. Tất cả các đỉnh nằm trên cùng một đường tròn, cách đều nhau một góc. Vì ba điểm không thẳng hàng xác định duy nhất một đường tròn nên ba trụ cột đã cố định hoàn toàn đường tròn ngoại tiếp. Câu hỏi duy nhất còn lại là có bao nhiêu đỉnh cách đều nhau tồn tại trên đường tròn đó sao cho cả ba điểm đã cho đều nằm ở vị trí đỉnh. 

Các ràng buộc rất nhỏ về kích thước đầu vào, chỉ có ba điểm. Thử thách này hoàn toàn mang tính toán học. Việc tìm kiếm mạnh mẽ trên tất cả các đa giác lên tới 100 cạnh là hoàn toàn khả thi, nhưng chúng ta vẫn cần một mô tả hình học đáng tin cậy về thời điểm ba điểm có thể thuộc cùng một đa giác đều. 

Độ chính xác của dấu phẩy động là phần nguy hiểm của vấn đề này. Đa giác đúng có thể phụ thuộc vào các góc như π/7 hoặc π/13 và việc kiểm tra đẳng thức trực tiếp sẽ không thành công do lỗi làm tròn. Một vấn đề tế nhị khác là ba điểm không được đảm bảo là các đỉnh liền kề nhau. Một lời giải ngây thơ giả định các đỉnh liên tiếp sẽ ngay lập tức đưa ra câu trả lời sai. 

Hãy xem xét ví dụ này:```
0 0
1 0
0 1
```Ba điểm này tạo thành một tam giác vuông. Một cách tiếp cận bất cẩn có thể cho rằng đa giác là một hình tam giác vì đã biết ba đỉnh. Nhưng những điểm này thực chất là các đỉnh của một hình vuông, có diện tích nhỏ hơn tam giác đều ngoại tiếp đi qua cùng một điểm. 

Một tình huống phức tạp khác xuất hiện khi đa giác có nhiều cạnh và các góc ở tâm trở nên rất nhỏ. Ví dụ: nếu đa giác thực có 97 cạnh, lỗi dấu phẩy động tích lũy trong các phép tính góc lặp đi lặp lại có thể dễ dàng làm cho phép giảm góc kiểu gcd không ổn định trừ khi chúng ta sử dụng epsilon một cách cẩn thận. 

Một lỗi phổ biến thứ ba là tính toán tâm đường tròn không chính xác cho các tam giác gần suy biến. Mặc dù dữ liệu đầu vào đảm bảo một câu trả lời hợp lệ nhưng các công thức không ổn định có thể gây ra sự mất mát lớn về độ chính xác nếu thực hiện một cách bất cẩn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu bắt đầu từ việc quan sát rằng số cạnh đa giác nhiều nhất là 100. Chúng ta có thể thử mọi cách có thể`n`từ 3 đến 100. 

Đối với một cố định`n`, thường xuyên`n`-gon chia đường tròn thành các cung góc`2π/n`. Nếu ba điểm đã cho là đỉnh của đa giác đó thì góc ở tâm giữa mỗi cặp điểm phải là bội số nguyên của`2π/n`. 

Vì vậy, thuật toán brute-force trông như thế này: 

1. Tính tâm đường tròn và bán kính đường tròn ngoại tiếp ba điểm. 
2. Tính góc cực của mỗi điểm xung quanh tâm. 
3. Đối với mọi`n`từ 3 đến 100: 

- Kiểm tra xem tất cả các hiệu số góc theo cặp có phải là bội số của`2π/n`. 
- Nếu có hãy tính diện tích đa giác. 
4. Xuất ra diện tích tối thiểu. 

Điều này đã hoạt động trong giới hạn. Chúng tôi chỉ kiểm tra 98 kích thước đa giác và mỗi thử nghiệm sử dụng hình học theo thời gian không đổi. Tổng công việc là không đáng kể. 

Thử thách thực sự không phải là tốc độ mà là sự chính xác. Việc so sánh các góc của dấu phẩy động với bội số chính xác là điều mong manh. Nếu chúng tôi kiểm tra:```
diff % step == 0
```giải pháp thất bại ngay lập tức vì độ nhiễu chính xác. 

Cái nhìn sâu sắc quan trọng là cấu trúc đa giác có thể được mô tả thông qua ước số chung lớn nhất của các góc ở tâm. 

Giả sử ba điểm tương ứng với các đỉnh cách nhau bởi`a`,`b`, Và`c`bước trên đa giác. Góc ở tâm của chúng là:```
a * 2π/n
b * 2π/n
c * 2π/n
```Điều đó có nghĩa là mọi độ lệch góc phải có cùng một đơn vị góc cơ bản. Đa giác hợp lệ nhỏ nhất chính xác là đa giác có góc đơn vị là gcd của tất cả các hiệu của góc ở tâm. 

Trong số học số nguyên, chúng tôi sử dụng thuật toán Euclid cho gcd. Đối với các góc có dấu phẩy động, chúng tôi mô phỏng ý tưởng tương tự bằng số. Từ`n ≤ 100`, một chiến lược thậm chí còn đơn giản hơn là đủ: thử mọi`n`và xác minh xem tất cả các góc có phải là bội số nguyên của`2π/n`trong epsilon. 

Một khi đúng`n`đã biết, diện tích đa giác sẽ được phân tích theo tiêu chuẩn thành các tam giác cân. Một thường xuyên`n`-gon với bán kính đường tròn`R`có diện tích:$A = \frac{nR^2\sin\left(\frac{2\pi}{n}\right)}{2}$Các phương pháp tiếp cận vũ phu và tối ưu thực sự giống nhau về mặt tiệm cận vì giới hạn cạnh chỉ là 100. Sự tối ưu đến từ hiểu biết sâu sắc về hình học giúp giảm xác thực của đa giác thành khả năng chia góc trên đường tròn ngoại tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các đa giác với các kiểm tra góc ngây thơ | O(100) | O(1) | Rủi ro do độ chính xác | 
| Chia góc hình học với epsilon | O(100) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba điểm. 

Ba điểm xác định duy nhất một đường tròn ngoại tiếp vì chúng đảm bảo không thẳng hàng. 
2. Tính độ dài các cạnh của tam giác tạo bởi các điểm đó. 

Chúng ta cần những độ dài này để tính bán kính đường tròn bằng công thức diện tích tam giác. 
3. Tính diện tích tam giác bằng tích chéo. 

Nếu các đỉnh của tam giác là`A`,`B`, Và`C`, sau đó:$\text{Area} = \frac{|(B-A) \times (C-A)|}{2}$4. Tính bán kính đường tròn`R`. 

Sử dụng quan hệ chuẩn:$R = \frac{abc}{4\Delta}$Ở đâu`a`,`b`, Và`c`là độ dài cạnh và`Δ`là diện tích tam giác. 
5. Tính ba góc ở tâm. 

Mỗi cạnh của tam giác đều tạo một góc nào đó ở tâm đường tròn. Theo định luật sin mở rộng:$\theta = 2\arcsin\left(\frac{a}{2R}\right)$Chúng tôi tính toán điều này cho cả ba cạnh tam giác. 
6. Thử mọi kích thước đa giác`n`từ 3 đến 100. 

Bước góc cơ bản của một thông thường`n`-gon là:$\frac{2\pi}{n}$7. Đối với mỗi góc ở tâm, hãy kiểm tra xem nó có phải là bội số nguyên của góc bước hay không. 

Chúng tôi tính toán:```
ratio = angle / step
```và xác minh rằng`ratio`đủ gần với một số nguyên. 
8. Hợp lệ đầu tiên`n`cho diện tích nhỏ nhất. 

Một cái lớn hơn`n`có nghĩa là có nhiều cạnh hơn trên cùng một đường tròn ngoại tiếp, điều này luôn làm tăng diện tích đa giác về phía diện tích hình tròn. 
9. Tính diện tích đa giác đều bằng công thức bán kính đường tròn. 
10. In câu trả lời với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Ba điểm nằm trên một đường tròn ngoại tiếp duy nhất. Bất kỳ đa giác đều nào chứa chúng đều phải sử dụng đường tròn chính xác này làm đường tròn ngoại tiếp. 

Một thường xuyên`n`-gon chia đường tròn thành các góc bằng nhau`2π/n`. Vì vậy, mọi cung giữa hai đỉnh đa giác phải là bội số nguyên của bước đó. Ba điểm đã cho xác định ba góc ở tâm và các góc này đều phải chia hết cho cùng một góc bước cơ bản. 

Bằng cách kiểm tra mọi`n`từ nhỏ nhất đến lớn nhất, đa giác hợp lệ đầu tiên được đảm bảo có diện tích tối thiểu. Đối với bán kính đường tròn cố định, việc tăng số cạnh sẽ làm tăng diện tích đa giác đều. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-6

def dist(x1, y1, x2, y2):
    return math.hypot(x1 - x2, y1 - y2)

def is_multiple(angle, step):
    k = round(angle / step)
    return abs(angle - k * step) < EPS

def solve():
    points = [tuple(map(float, input().split())) for _ in range(3)]

    (x1, y1), (x2, y2), (x3, y3) = points

    a = dist(x2, y2, x3, y3)
    b = dist(x1, y1, x3, y3)
    c = dist(x1, y1, x2, y2)

    cross = abs((x2 - x1) * (y3 - y1) - (y2 - y1) * (x3 - x1))
    triangle_area = cross / 2.0

    R = (a * b * c) / (4.0 * triangle_area)

    angles = []
    for side in [a, b, c]:
        value = side / (2.0 * R)
        value = max(-1.0, min(1.0, value))
        angles.append(2.0 * math.asin(value))

    answer = 0.0

    for n in range(3, 101):
        step = 2.0 * math.pi / n

        ok = True

        for angle in angles:
            if not is_multiple(angle, step):
                ok = False
                break

        if ok:
            answer = 0.5 * n * R * R * math.sin(2.0 * math.pi / n)
            break

    print(f"{answer:.10f}")

solve()
```Phần đầu tiên tính độ dài và diện tích các cạnh của tam giác. Công thức tích số chéo ổn định về số lượng và tránh công thức Heron, công thức này có thể làm mất độ chính xác đối với các hình tam giác mỏng. 

Việc tính toán bán kính đường tròn sử dụng mối quan hệ cổ điển giữa độ dài các cạnh và diện tích tam giác. Vì tất cả các tọa độ đều nhỏ nên việc tràn dấu phẩy động không phải là vấn đề đáng lo ngại. 

Các góc trung tâm có nguồn gốc từ độ dài dây cung. Đối với một hợp âm dài`a`trong một vòng tròn bán kính`R`, góc ở tâm tương ứng thỏa mãn:```
a = 2R sin(theta / 2)
```Chúng tôi đảo ngược mối quan hệ này bằng cách sử dụng`asin`. Cái kẹp để`[-1, 1]`rất quan trọng vì lỗi dấu phẩy động có thể tạo ra các giá trị như`1.0000000002`, sẽ sụp đổ`asin`. 

Bước xác thực sẽ kiểm tra xem mỗi góc có phải là bội số của góc bậc đa giác hay không. Chúng tôi làm tròn tỷ lệ đến số nguyên gần nhất và so sánh lỗi tái tạo với epsilon. Các phép tính mô đun trực tiếp trên số dấu phẩy động kém tin cậy hơn nhiều. 

Vòng lặp bắt đầu từ`n = 3`, do đó đa giác hợp lệ đầu tiên tự động có diện tích tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
1 1
0 1
```Các điểm tạo thành ba đỉnh của một hình vuông. 

| Biến | Giá trị | 
| --- | --- | 
| một | 1 | 
| b | 1 | 
| c | √2 | 
| Khu vực tam giác | 0,5 | 
| Bán kính tròn R | 0,70710678 | 

Góc ở tâm trở thành: 

| Bên | Góc trung tâm | 
| --- | --- | 
| 1 | π/2 | 
| 1 | π/2 | 
| √2 | π | 

Bây giờ chúng tôi kiểm tra kích thước đa giác: 

| n | Góc bước | Có hiệu lực? | 
| --- | --- | --- | 
| 3 | 2π/3 | Không | 
| 4 | π/2 | Có | 

Khu vực: 

| Kết quả công thức | 
| --- | 
| 1.0 | 

Dấu vết này cho thấy tại sao đa giác không nhất thiết phải là hình tam giác. Ba điểm thẳng hàng hoàn hảo với khoảng cách đỉnh của hình vuông. 

### Ví dụ 2 

đầu vào:```
1 0
0 1
-1 0
```Đây là ba đỉnh của một hình lục giác đều trên đường tròn đơn vị. 

| Biến | Giá trị | 
| --- | --- | 
| một | √2 | 
| b | 2 | 
| c | √2 | 
| Khu vực tam giác | 1 | 
| Bán kính tròn R | 1 | 

Góc ở tâm: 

| Bên | Góc trung tâm | 
| --- | --- | 
| √2 | π/2 | 
| 2 | π | 
| √2 | π/2 | 

Tìm kiếm đa giác: 

| n | Góc bước | Có hiệu lực? | 
| --- | --- | --- | 
| 3 | 2π/3 | Không | 
| 4 | π/2 | Có | 

Khu vực: 

| Kết quả công thức | 
| --- | 
| 2.0 | 

Ví dụ này chứng tỏ rằng nhiều đa giác có thể chứa ba điểm giống nhau. Hình lục giác có tác dụng nhưng hình vuông xuất hiện sớm hơn và có diện tích nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100) | Chúng tôi kiểm tra tối đa 98 kích thước đa giác | 
| Không gian | O(1) | Chỉ có một số biến dấu phẩy động được lưu trữ | 

Thời gian chạy có hiệu quả là không đổi. Ngay cả Python cũng xử lý việc này ngay lập tức vì tất cả các phép tính đều là các phép toán lượng giác cơ bản trên một số giá trị cố định. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    EPS = 1e-6

    def dist(x1, y1, x2, y2):
        return math.hypot(x1 - x2, y1 - y2)

    def is_multiple(angle, step):
        k = round(angle / step)
        return abs(angle - k * step) < EPS

    points = [tuple(map(float, input().split())) for _ in range(3)]

    (x1, y1), (x2, y2), (x3, y3) = points

    a = dist(x2, y2, x3, y3)
    b = dist(x1, y1, x3, y3)
    c = dist(x1, y1, x2, y2)

    cross = abs((x2 - x1) * (y3 - y1) - (y2 - y1) * (x3 - x1))
    triangle_area = cross / 2.0

    R = (a * b * c) / (4.0 * triangle_area)

    angles = []

    for side in [a, b, c]:
        value = side / (2.0 * R)
        value = max(-1.0, min(1.0, value))
        angles.append(2.0 * math.asin(value))

    answer = 0.0

    for n in range(3, 101):
        step = 2.0 * math.pi / n

        ok = True

        for angle in angles:
            if not is_multiple(angle, step):
                ok = False
                break

        if ok:
            answer = 0.5 * n * R * R * math.sin(2.0 * math.pi / n)
            break

    return f"{answer:.8f}"

# provided sample
assert run(
"""0 0
1 1
0 1
"""
) == "1.00000000", "sample 1"

# equilateral triangle
assert run(
"""0 0
1 0
0.5 0.8660254038
"""
) == "0.43301270", "equilateral triangle"

# square on unit circle
assert run(
"""1 0
0 1
-1 0
"""
) == "2.00000000", "square"

# regular hexagon vertices
assert run(
"""1 0
0.5 0.8660254038
-0.5 0.8660254038
"""
) == "2.59807621", "hexagon"

print("All tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 1.00000000 | Tính đúng đắn cơ bản | 
| Tam giác đều | 0,43301270 | Đa giác nhỏ nhất có thể | 
| Đơn vị hình tròn vuông điểm | 2.00000000 | Các đỉnh không liền kề | 
| Đỉnh lục giác | 2.59807621 | Đa giác đều lớn hơn | 

## Vỏ cạnh 

Một trường hợp thất bại phổ biến là giả sử ba điểm là các đỉnh liên tiếp. 

đầu vào:```
0 0
1 1
0 1
```Tam giác hình thành bởi những điểm này không đều. Một thuật toán ngây thơ có thể dừng lại ở`n = 3`và tính diện tích hình tam giác. Thay vào đó, thuật toán của chúng tôi kiểm tra tính chia hết của các góc ở tâm. Tam giác thất bại vì các góc không phải là bội số của`2π/3`. Hình vuông thành công vì mọi góc đều là bội số của`π/2`. 

Một trường hợp tinh tế khác là khi làm tròn dấu phẩy động đẩy các giá trị ra ngoài phạm vi lượng giác hợp lệ một chút. 

đầu vào:```
1 0
-1 0
0 1
```Về mặt toán học, độ dài một dây bằng chính xác`2R`, do đó biểu thức bên trong`asin`nên chính xác`1`. Số học dấu phẩy động có thể tạo ra`1.0000000001`. Không cần kẹp,`asin`đưa ra một lỗi tên miền. Việc triển khai giới hạn rõ ràng giá trị đối với`[-1, 1]`. 

Tình huống nguy hiểm thứ ba là các đa giác có nhiều cạnh, trong đó các bước góc cạnh trở nên rất nhỏ. 

đầu vào:```
1 0
0.9980267284 0.0627905195
0.9921147013 0.1253332336
```Đây là các đỉnh liên tiếp của một 100-giác đều. Kiểm tra đẳng thức trực tiếp trên các góc dấu phẩy động không thành công vì các góc được tính toán chứa nhiễu số tích lũy. Kiểm tra bội số nguyên dựa trên epsilon nhận dạng chính xác cấu trúc đa giác.
