---
title: "CF 104804G - \u041e, \u043d\u0435\u0442! \u0413\u0435\u043e\u043c\u0430!!!"
description: "Chúng ta có ba điểm trong mặt phẳng, mỗi điểm được mô tả bằng tọa độ nguyên. Các điểm được đảm bảo không nằm trên cùng một đường thẳng nên tạo thành một tam giác hợp lệ. Nhiệm vụ là dựng đường tròn duy nhất đi qua cả ba điểm và tính bán kính của nó."
date: "2026-06-28T16:52:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "G"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 77
verified: false
draft: false
---

[CF 104804G - \u041e, \u043d\u0435\u0442! \u0413\u0435\u043e\u043c\u0430!!!](https://codeforces.com/problemset/problem/104804/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba điểm trong mặt phẳng, mỗi điểm được mô tả bằng tọa độ nguyên. Các điểm được đảm bảo không nằm trên cùng một đường thẳng nên tạo thành một tam giác hợp lệ. Nhiệm vụ là dựng đường tròn duy nhất đi qua cả ba điểm và tính bán kính của nó. 

Về mặt hình học, đây là yêu cầu bán kính đường tròn ngoại tiếp của một tam giác được xác định bởi ba đỉnh. Đầu ra là một số thực duy nhất, bán kính của đường tròn ngoại tiếp đó, được in với độ chính xác vừa đủ. 

Các ràng buộc rất nhỏ: tọa độ là các số nguyên có giá trị tuyệt đối tối đa là 1000. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ số học nào liên quan đến khoảng cách hoặc diện tích theo cặp đều an toàn trong dấu phẩy động nếu được xử lý cẩn thận. Không cần phải lo lắng về vấn đề tràn số nguyên ngoài độ an toàn 64-bit tiêu chuẩn và không có áp lực về hiệu suất vì chúng tôi chỉ xử lý một hình tam giác. 

Các trường hợp lỗi khó phát hiện chính đến từ cấu hình suy biến hoặc không ổn định về mặt số lượng hơn là do hiệu suất. 

Trường hợp cạnh đầu tiên là khi tam giác gần như suy biến, nghĩa là các điểm rất gần với đường thẳng. Mặc dù bài toán đảm bảo tính không cộng tuyến, nhưng diện tích có thể rất nhỏ, điều này có thể làm tăng sai số dấu phẩy động trong các công thức chia theo diện tích. 

Ví dụ, hãy xem xét các điểm:```
(0, 0)
(1000, 1)
(2000, 2)
```Điều này không được cho phép nghiêm ngặt vì chúng thẳng hàng, nhưng trường hợp hợp lệ gần như cộng tuyến như:```
(0, 0)
(1000, 1)
(2000, 3)
```tạo ra bán kính rất lớn. Bất kỳ công thức chia cho diện tích tam giác đều phải xử lý mẫu số nhỏ một cách cẩn thận. 

Một vấn đề khác là sử dụng không chính xác khoảng cách bình phương so với khoảng cách thực tế. Nếu một giải pháp quên căn bậc hai ở các bước trung gian hoặc trộn lẫn số lượng bình phương và không bình phương, nó sẽ âm thầm tạo ra sai tỷ lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận hình học mạnh mẽ sẽ cố gắng xây dựng đường tròn ngoại tiếp một cách rõ ràng bằng cách tìm giao điểm của các đường phân giác vuông góc của hai cạnh. Mỗi đường phân giác là một đường thẳng nên chúng ta tính điểm giữa, hệ số góc rồi giải hệ tuyến tính 2x2. Điều này đúng và dễ hiểu về mặt khái niệm, nhưng nó đưa ra nhiều cách phân chia và trường hợp đặc biệt cho các đường thẳng đứng. Nó cũng có nguy cơ mất ổn định về số lượng khi độ dốc lớn hoặc gần bằng nhau. 

Cái nhìn sâu sắc hơn là tránh việc xây dựng hoàn toàn tâm vòng tròn một cách rõ ràng. Thay vào đó, chúng ta sử dụng công thức trực tiếp tính bán kính đường tròn ngoại tiếp một tam giác. Nếu tam giác có độ dài các cạnh$a$,$b$,$c$, và diện tích$S$, thì bán kính đường tròn là$$R = \frac{abc}{4S}.$$Đây là sự đơn giản hóa quan trọng. Cấu trúc tam giác cung cấp cho chúng ta mọi thứ chúng ta cần chỉ thông qua khoảng cách và diện tích, tránh các trường hợp cạnh hình học tọa độ như giao điểm đường phân giác vuông góc. 

Chúng tôi tính độ dài các cạnh bằng khoảng cách Euclide và tính diện tích bằng tích chéo của hai cạnh. Tích chéo cho diện tích trực tiếp gấp đôi, giúp loại bỏ sự cần thiết của công thức Heron và giảm sai số số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đường phân giác vuông góc | O(1) | O(1) | Rủi ro do độ chính xác và trường hợp đặc biệt | 
| Công thức khoảng cách + diện tích | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba điểm$A(x_1, y_1)$,$B(x_2, y_2)$, Và$C(x_3, y_3)$. Chúng xác định một hình tam giác trong mặt phẳng. 
2. Tính độ dài cạnh$a = BC$,$b = CA$, Và$c = AB$sử dụng khoảng cách Euclide. Mỗi giá trị được tính là căn bậc hai của chênh lệch tọa độ bình phương. Điều này là cần thiết vì công thức bán kính đường tròn phụ thuộc vào độ dài hình học thực tế chứ không phải giá trị bình phương. 
3. Tính diện tích tam giác$ABC$sử dụng tích chéo:$$S = \frac{1}{2} |(B - A) \times (C - A)|.$$Tích chéo cho diện tích gấp đôi và lấy giá trị tuyệt đối đảm bảo tính dương bất kể hướng. 
4. Tính bán kính đường tròn bằng cách sử dụng:$$R = \frac{a \cdot b \cdot c}{4S}.$$Công thức này xuất phát từ hình học tam giác tiêu chuẩn và tránh giải một cách rõ ràng cho tâm đường tròn. 
5. In$R$với độ chính xác đủ về dấu phẩy động, thường ít nhất từ ​​5 đến 10 chữ số thập phân để đáp ứng yêu cầu. 

### Tại sao nó hoạt động 

Đường tròn ngoại tiếp một tam giác được xác định duy nhất bởi ba đỉnh của nó. Công thức$R = \frac{abc}{4S}$được bắt nguồn từ mối quan hệ giữa diện tích của một tam giác và đường tròn ngoại tiếp của nó. Tích chéo tính toán chính xác diện tích hình học chính xác từ tọa độ và khoảng cách Euclide cho độ dài cạnh chính xác. Vì tất cả các phép tính đều được lấy trực tiếp từ các bất biến hình học của tam giác nên mọi cách triển khai nhất quán đều phải mang lại cùng một bán kính bất kể thứ tự hoặc hướng tọa độ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def dist(x1, y1, x2, y2):
    return math.hypot(x1 - x2, y1 - y2)

x1, y1 = map(int, input().split())
x2, y2 = map(int, input().split())
x3, y3 = map(int, input().split())

a = dist(x2, y2, x3, y3)
b = dist(x1, y1, x3, y3)
c = dist(x1, y1, x2, y2)

cross = abs((x2 - x1) * (y3 - y1) - (y2 - y1) * (x3 - x1))
area = cross / 2.0

R = (a * b * c) / (4.0 * area)

print(R)
```Việc thực hiện trực tiếp theo cấu trúc thuật toán. các`math.hypot`Hàm được sử dụng thay vì bình phương và căn bậc hai thủ công vì nó ổn định hơn về mặt số lượng và tránh tràn hoặc mất độ chính xác trong bình phương trung gian. 

Tích chéo được tính từ hai vectơ có nguồn gốc ở điểm đầu tiên. Điều này tránh mọi nhu cầu tính toán góc hoặc hàm lượng giác. Chia cho 2 chuyển đổi diện tích hình bình hành thành diện tích hình tam giác. 

Công thức cuối cùng được áp dụng chính xác như đã suy ra. Sự phân chia theo$4 \cdot area$là an toàn vì bài toán đảm bảo không cộng tuyến nên diện tích hoàn toàn dương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
0 0
2 1
-2 1
```Chúng ta tính toán từng bước: 

| Bước | Giá trị | 
| --- | --- | 
| A | (0, 0) | 
| B | (2, 1) | 
| C | (-2, 1) | 
| a = BC | 4.0 | 
| b = CA | sqrt( (0+2)^2 + (0-1)^2 ) = sqrt(5) | 
| c = AB | sqrt( (0-2)^2 + (0-1)^2 ) = sqrt(5) | 
| chéo | | 
| khu vực | 2 | 
| R | (4 * sqrt(5) * sqrt(5)) / (8) = 20 / 8 = 2,5 | 

Điều này xác nhận một tam giác đối xứng trong đó hai cạnh bằng nhau, tạo ra bán kính hợp lý rõ ràng. 

### Mẫu 2 

đầu vào:```
0 10
0 0
12 4
```| Bước | Giá trị | 
| --- | --- | 
| A | (0, 10) | 
| B | (0, 0) | 
| C | (12, 4) | 
| a = BC | sqrt(144 + 16) = sqrt(160) | 
| b = CA | sqrt(144 + 36) = sqrt(180) | 
| c = AB | 10 | 
| chéo | | 
| khu vực | 60 | 
| R | sqrt(160)*sqrt(180)*10 / 240 ≈ 7.07107 | 

Ví dụ này cho thấy một tam giác không đối xứng trong đó tính toán hình học trực tiếp là cần thiết. Sản phẩm chéo chụp chính xác khu vực không phụ thuộc vào hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học theo thời gian không đổi trên ba điểm | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Việc tính toán hoàn toàn mang tính hình học và không phụ thuộc vào kích thước đầu vào, do đó nó thỏa mãn mọi ràng buộc hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import hypot

    x1, y1 = map(int, sys.stdin.readline().split())
    x2, y2 = map(int, sys.stdin.readline().split())
    x3, y3 = map(int, sys.stdin.readline().split())

    def dist(a, b, c, d):
        return math.hypot(a - c, b - d)

    a = dist(x2, y2, x3, y3)
    b = dist(x1, y1, x3, y3)
    c = dist(x1, y1, x2, y2)

    cross = abs((x2 - x1) * (y3 - y1) - (y2 - y1) * (x3 - x1))
    area = cross / 2.0

    return str((a * b * c) / (4.0 * area))

# provided samples
assert abs(float(run("0 0\n2 1\n-2 1\n")) - 2.5) < 1e-6
assert abs(float(run("0 10\n0 0\n12 4\n")) - 7.07107) < 1e-3

# custom cases
assert abs(float(run("0 0\n1 0\n0 1\n")) - math.sqrt(2)/2) < 1e-6, "right triangle"
assert abs(float(run("0 0\n2 0\n1 2\n")) - (2*math.sqrt(5)*math.sqrt(5)) / (4*2)) < 1e-6, "simple triangle"
assert abs(float(run("0 0\n1000 0\n0 1000\n")) - (1000*math.sqrt(2)/2)) < 1e-6, "large coordinates"
assert abs(float(run("0 1\n2 3\n4 0\n")) > 0), "general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác vuông | √2/2 | độ đúng hình học cơ bản | 
| tam giác đơn giản | công thức tính toán | tính nhất quán của việc thực hiện công thức | 
| tọa độ lớn | bán kính lớn | ổn định số trong giới hạn | 
| trường hợp chung | giá trị dương | xử lý không suy biến | 

## Vỏ cạnh 

Tam giác gần suy biến là trường hợp ứng suất chính. Ví dụ:```
0 0
1000 1
2000 3
```Thuật toán tính tích chéo rất nhỏ, dẫn đến bán kính lớn. Các bước vẫn hoạt động chính xác vì diện tích được tính trực tiếp từ công thức xác định, công thức này vẫn ổn định trong phép tính số nguyên cho đến phép chia cuối cùng. 

Việc tính toán tiến hành như sau. Tích chéo khác 0 nên diện tích dương. Độ dài cạnh được tính toán thông thường thông qua khoảng cách Euclide. Phép chia cuối cùng khuếch đại kết quả nhưng không phá vỡ tính chính xác, chỉ có độ chính xác mới có thể yêu cầu xử lý dấu phẩy động cẩn thận. 

Điều này xác nhận rằng phương pháp này hiệu quả ngay cả khi tam giác trở nên cực kỳ mỏng, miễn là loại trừ hiện tượng cộng tuyến.
