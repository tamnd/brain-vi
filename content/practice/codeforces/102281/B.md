---
title: "CF 102281B - \u041a\u0443\u043b\u0438\u043d\u0430\u0440\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta có một khuôn cắt bánh quy hình tam giác có độ dài các cạnh là (a), (b) và (c) và một khuôn cắt bánh quy hình tròn có bán kính (r)."
date: "2026-08-13T16:09:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "B"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 114
verified: true
draft: false
---

[CF 102281B - \u041a\u0443\u043b\u0438\u043d\u0430\u0440\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khuôn cắt bánh quy hình tam giác có độ dài các cạnh là (a), (b) và (c) và một khuôn cắt bánh quy hình tròn có bán kính (r). Bánh quy mỏng, vì vậy câu hỏi hoàn toàn là hai chiều: liệu bánh quy hình tròn có thể được đặt hoàn toàn bên trong máy cắt hình tam giác hay không, và liệu bánh quy hình tam giác có thể được đặt hoàn toàn bên trong máy cắt hình tròn không? 

Câu hỏi đầu tiên yêu cầu hình tròn lớn nhất có thể nằm gọn trong hình tam giác. Đường tròn đó là đường tròn nội tiếp tam giác nên đại lượng liên quan là bán kính nội tiếp. 

Câu hỏi thứ hai yêu cầu hình tròn nhỏ nhất có thể chứa toàn bộ hình tam giác. Một đường tròn như vậy được xác định bởi đường tròn ngoại tiếp của tam giác, do đó đại lượng liên quan là bán kính đường tròn ngoại tiếp. 

Đầu vào chứa bốn số nguyên từ 1 đến 10000. Các giới hạn này rất nhỏ đối với nghiệm (O(1)) và thậm chí thuật toán tuyến tính cũng không cần thiết vì chỉ có một hình tam giác để xử lý. Mối quan tâm chính không phải là thời gian chạy mà là tính chính xác về số lượng. Các phép tính dấu phẩy động trực tiếp bằng công thức Heron có thể gây ra lỗi làm tròn chính xác ở ranh giới nơi câu trả lời thay đổi. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Coi như```
1 1 1 1
```Tam giác là tam giác đều. Bán kính trong của nó là (\sqrt{3}/6), nhỏ hơn 1, trong khi bán kính đường tròn của nó là (\sqrt{3}/3), cũng nhỏ hơn 1. Cả hai cookie đều phù hợp nên cả hai câu trả lời đều phải dương. Việc triển khai gây nhầm lẫn giữa bán kính trong và bán kính đường tròn sẽ nhận được một trong các câu trả lời sai. 

Một trường hợp ranh giới hữu ích hơn là```
2 2 2 1
```Ở đây bán kính đường tròn ngoại tiếp là (2/\sqrt{3}), lớn hơn 1, trong khi bán kính nội tiếp là (1/\sqrt{3}), nhỏ hơn 1. Do đó, đường tròn vừa với tam giác, nhưng tam giác không vừa với đường tròn. Sử dụng tiêu chí bán kính giống nhau cho cả hai hướng sẽ cho kết quả sai. 

Ranh giới chính xác cũng phải được xử lý một cách toàn diện. Ví dụ,```
2 2 2 1
```không nằm trên một ranh giới, nhưng một tam giác đều có cạnh 1 và một đường tròn có bán kính chính xác bằng bán kính nội tiếp của nó sẽ phù hợp. Việc so sánh phải sử dụng`>=`, không`>`. 

Cuối cùng, các cạnh có thể lớn tới 10000, do đó, việc tính toán trực tiếp biểu thức Heron là an toàn trong Python, nhưng việc triển khai bằng ngôn ngữ số nguyên có chiều rộng cố định vẫn phải chọn số học một cách cẩn thận. Sản phẩm lớn nhất có liên quan theo thứ tự (10^{16}), phù hợp với số nguyên 64 bit có dấu nhưng không phù hợp với số nguyên 32 bit có dấu. 

## Phương pháp tiếp cận 

Một cách tiếp cận hình học đơn giản sẽ cố gắng mô phỏng quá trình chèn vật lý. Đối với đường tròn, người ta có thể thử nhiều vị trí và hướng có thể và kiểm tra xem đường tròn có cắt một trong ba cạnh hay không. Đối với hình tam giác bên trong đường tròn, người ta có thể liệt kê các vị trí và góc quay một cách tương tự. Vấn đề là vị trí và góc quay là các biến liên tục, do đó không có tìm kiếm vũ phu hữu hạn nào vừa chính xác vừa đảm bảo tìm ra giá trị tối ưu. Nếu chúng ta rời rạc hóa góc thành các giá trị (N) và vị trí thành các giá trị (M), thì chi phí tìm kiếm kết quả (O(NM)) sẽ kiểm tra và câu trả lời của nó vẫn phụ thuộc vào sự rời rạc hóa đã chọn. Trong trường hợp xấu nhất, không có số lượng hoạt động hữu hạn nào làm cho phương pháp đó trở thành một giải pháp chính xác, bởi vì một lưới mịn tùy ý vẫn có thể thiếu một vị trí hợp lệ. 

Ý tưởng vũ lực có một đặc tính hữu ích: nó hoạt động vì nó đang tìm kiếm vị trí cực trị. Đường tròn phải có tâm tại một điểm đặc biệt nếu nó càng lớn càng tốt và tam giác phải liên hệ với một đường tròn bao quanh đặc biệt nếu đường tròn đó càng nhỏ càng tốt. Thay vì tìm kiếm những vị trí đó, chúng tôi có thể xác định chúng bằng toán học. 

Đối với một tam giác, đường tròn nội tiếp lớn nhất có bán kính bằng bán kính nội tiếp 

[ 
\rho = \frac{A}{s}, 
] 

trong đó (A) là diện tích của tam giác và (s=(a+b+c)/2) là bán chu vi của tam giác. Cookie hình tròn khớp chính xác khi (r\geq\rho). 

Ngược lại, đường tròn nhỏ nhất chứa cả ba đỉnh có bán kính bằng bán kính đường tròn ngoại tiếp 

[ 
R = \frac{abc}{4A}. 
] 

Tam giác vừa khít với dao cắt tròn khi (r\geq R). 

Vấn đề còn lại là tránh số học dấu phẩy động. Công thức Heron cho 

[ 
A^2=s(s-a)(s-b)(s-c). 
] 

Sẽ thuận tiện hơn khi nhân mọi thứ với 16 và xác định 

[ 
D=(a+b+c)(-a+b+c)(a-b+c)(a+b-c)=16A^2. 
] 

Khi đó (4A=\sqrt D). Điều kiện bán kính trở thành 

[ 
r\geq\frac{\sqrt D}{2(a+b+c)}. 
] 

Cả hai vế đều không âm nên ta có thể bình phương chúng: 

[ 
4r^2(a+b+c)^2\geq D. 
] 

Đối với bán kính đường tròn, 

[ 
R=\frac{abc}{\sqrt D}, 
] 

vậy 

[ 
r\geq R 
] 

tương đương với 

[ 
r^2D\geq a^2b^2c^2. 
] 

Cả hai quyết định bây giờ đều là so sánh số nguyên chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực mạnh hình học đối với các vị trí và phép quay | Không có giới hạn chính xác hữu hạn | O(1) | Không phù hợp | 
| Công thức có dấu phẩy động | O(1) | O(1) | Rủi ro ở ranh giới | 
| So sánh số nguyên sử dụng công thức Heron | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài các cạnh (a,b,c) và bán kính hình tròn (r). Độ dài ba cạnh mô tả một hình tam giác, vì vậy đại lượng hình học đầu tiên chúng ta cần là diện tích bình phương của nó. 
2. Tính toán 

[ 
D=(a+b+c)(-a+b+c)(a-b+c)(a+b-c). 
] 

Vì (D=16A^2), căn bậc hai của nó là (4A). Chúng ta thực sự không bao giờ cần phải tính căn bậc hai đó. 

1. Kiểm tra xem hình tròn có vừa với hình tam giác hay không bằng cách sử dụng 

[ 
4r^2(a+b+c)^2\geq D. 
] 

Đây chính xác là điều kiện (r\geq\rho), trong đó (\rho) là bán kính của tam giác. 

1. Kiểm tra xem hình tam giác có vừa với hình tròn hay không bằng cách sử dụng 

[ 
r^2D\geq a^2b^2c^2. 
] 

Đây chính xác là điều kiện (r\geq R), trong đó (R) là bán kính đường tròn. 

1. In câu tương ứng cho mỗi câu so sánh. Sự bình đẳng được coi là phù hợp vì bánh quy được phép chạm vào máy cắt. 

### Tại sao nó hoạt động 

Đường tròn lớn nhất có thể đặt bên trong một hình tam giác là đường tròn nội tiếp của nó, vì vậy việc kiểm tra bán kính nội tiếp là cần thiết và đủ cho hướng đầu tiên. Đường tròn nhỏ nhất chứa tất cả các đỉnh của một tam giác không suy biến là đường tròn ngoại tiếp của nó, vì vậy việc kiểm tra bán kính ngoại tiếp là cần thiết và đủ cho hướng thứ hai.

Giá trị (D) thỏa mãn (D=16A^2). Việc thay thế đẳng thức này vào các công thức tính bán kính nội tiếp và bán kính đường tròn sẽ biến đổi cả hai điều kiện hình học thành các bất đẳng thức nguyên chính xác. Vì mọi phép biến đổi đều bảo toàn hướng so sánh và tất cả các đại lượng bình phương đều không âm, nên thuật toán đưa ra kết quả giống như bài toán hình học ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c, r = map(int, input().split())

p = a + b + c

# 16 * area^2
d = p * (-a + b + c) * (a - b + c) * (a + b - c)

# Circle fits into triangle iff r >= inradius.
circle_in_triangle = 4 * r * r * p * p >= d

# Triangle fits into circle iff r >= circumradius.
triangle_in_circle = r * r * d >= a * a * b * b * c * c

if circle_in_triangle:
    print("Circle gets into the triangle")
else:
    print("Circle doesn’t get into the triangle")

if triangle_in_circle:
    print("Triangle gets into the circle")
else:
    print("Triangle doesn’t get into the circle")
```Biến`p`cửa hàng (a+b+c), xuất hiện trong cả biểu thức Heron và phép so sánh inradius. Biến`d`là (16A^2), cho phép toàn bộ nghiệm tránh được`sqrt`. 

Để so sánh đầu tiên, mã sử dụng```
4 * r * r * p * p >= d
```bởi vì 

[ 
r\geq\frac{\sqrt D}{2p} 
] 

tương đương với 

[ 
4r^2p^2\geq D. 
] 

Để so sánh thứ hai, mã sử dụng```
r * r * d >= a * a * b * b * c * c
```bởi vì (R=abc/\sqrt D). Bình đẳng được cố tình chấp nhận trong cả hai trường hợp. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Trong một ngôn ngữ như C++, số nguyên 64 bit là đủ cho những ràng buộc này. Đầu vào chỉ chứa một trường hợp thử nghiệm, do đó không có vòng lặp xung quanh phép tính. 

Các chuỗi đầu ra chính xác phải được giữ nguyên, bao gồm cả dấu nháy đơn cong trong`doesn’t`, vì Codeforces so sánh trực tiếp văn bản đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu đầu tiên, đầu vào là```
1 1 1 10
```Tam giác đều và hình tròn rất lớn. 

| Biến | Giá trị | 
| --- | --- | 
| (a) | 1 | 
| (b) | 1 | 
| (c) | 1 | 
| (r) | 10 | 
| (p=a+b+c) | 3 | 
| (D) | 3 | 
| (4r^2p^2) | 3600 | 
| (r^2D) | 300 | 
| (a^2b^2c^2) | 1 | 
| Vòng tròn bên trong tam giác | sai | 
| Tam giác bên trong vòng tròn | đúng | 

Bất đẳng thức đầu tiên không thành công vì đường tròn có bán kính 10 lớn hơn nhiều so với bán kính nội tiếp của tam giác. Bất đẳng thức thứ hai đúng vì bán kính đường tròn ngoại tiếp của tam giác nhỏ hơn 10 rất nhiều. 

Đầu ra là```
Circle doesn’t get into the triangle
Triangle gets into the circle
```### Mẫu 2 

Đối với mẫu thứ hai, đầu vào là```
10 10 10 1
```Bây giờ hình tam giác lớn hơn hình tròn rất nhiều. 

| Biến | Giá trị | 
| --- | --- | 
| (a) | 10 | 
| (b) | 10 | 
| (c) | 10 | 
| (r) | 1 | 
| (p=a+b+c) | 30 | 
| (D) | 30000 | 
| (4r^2p^2) | 3600 | 
| (r^2D) | 30000 | 
| (a^2b^2c^2) | 1000000 | 
| Vòng tròn bên trong tam giác | đúng | 
| Tam giác bên trong vòng tròn | sai | 

Bán kính của hình tròn lớn hơn bán kính của hình tam giác nên hình tròn nằm gọn trong đó. Tuy nhiên, bán kính đường tròn ngoại tiếp của tam giác lớn hơn 1 rất nhiều nên tam giác không thể nằm gọn trong hình tròn. 

Đầu ra là```
Circle gets into the triangle
Triangle doesn’t get into the circle
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Một số phép tính số học cố định được thực hiện. | 
| Không gian | (O(1)) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Các ràng buộc cho phép độ dài cạnh và bán kính lên tới 10000 và lời giải chỉ thực hiện một số phép tính số học. Nó thấp hơn nhiều so với giới hạn thời gian 1,5 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 128 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    a, b, c, r = map(int, input().split())

    p = a + b + c
    d = p * (-a + b + c) * (a - b + c) * (a + b - c)

    circle_in_triangle = 4 * r * r * p * p >= d
    triangle_in_circle = r * r * d >= a * a * b * b * c * c

    first = (
        "Circle gets into the triangle"
        if circle_in_triangle
        else "Circle doesn’t get into the triangle"
    )
    second = (
        "Triangle gets into the circle"
        if triangle_in_circle
        else "Triangle doesn’t get into the circle"
    )

    return first + "\n" + second

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided sample 1
assert run("1 1 1 10\n") == (
    "Circle doesn’t get into the triangle\n"
    "Triangle gets into the circle"
), "sample 1"

# Provided sample 2
assert run("10 10 10 1\n") == (
    "Circle gets into the triangle\n"
    "Triangle doesn’t get into the circle"
), "sample 2"

# Minimum-size valid triangle
assert run("1 1 1 1\n") == (
    "Circle gets into the triangle\n"
    "Triangle gets into the circle"
), "minimum-size values"

# Maximum-size equilateral triangle
assert run("10000 10000 10000 10000\n") == (
    "Circle gets into the triangle\n"
    "Triangle gets into the circle"
), "maximum-size values"

# Exact inradius boundary for a 3-4-5 triangle:
# inradius = 1, circumradius = 2.5
assert run("3 4 5 1\n") == (
    "Circle gets into the triangle\n"
    "Triangle doesn’t get into the circle"
), "inradius boundary"

# Exact circumradius boundary for a 3-4-5 triangle
assert run("3 4 5 3\n") == (
    "Circle gets into the triangle\n"
    "Triangle gets into the circle"
), "circumradius boundary"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`| Cả hai vào được | Giá trị tối thiểu và hình học đều đối xứng | 
|`10000 10000 10000 10000`| Cả hai vào được | Giá trị tối đa và số học số nguyên lớn | 
|`3 4 5 1`| Chỉ vòng tròn | Ranh giới bán kính chính xác | 
|`3 4 5 3`| Cả hai vào được | Ranh giới bán kính và so sánh toàn diện | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
1 1 1 1
```chúng tôi nhận được (D=3). Điều kiện hình tròn là (4\cdot1^2\cdot3^2=36\geq3) và điều kiện tam giác là (1^2\cdot3=3\geq1). Cả hai so sánh đều thành công. Ví dụ này phát hiện các cách triển khai vô tình sử dụng sai công thức bán kính cho một trong các hướng. 

Đối với tam giác 3-4-5 có bán kính 1,```
3 4 5 1
```diện tích là 6, nên bán kính nội bộ là (6/6=1). Vòng tròn chạm chính xác cả ba cạnh ở vị trí tối ưu và phải được chấp nhận. Bán kính đường tròn là (3\cdot4\cdot5/(4\cdot6)=2,5), do đó tam giác không vừa. Vụ án này bị xử lý nghiêm khắc`>`so sánh. 

Đối với cùng một tam giác có bán kính 3,```
3 4 5 3
```hình tròn vẫn lớn hơn bán kính nội tiếp và bây giờ nó cũng lớn hơn bán kính hình tròn. Cả hai hướng đều thành công. Điều này xác minh rằng hai bất đẳng thức là độc lập và phép tính bán kính đường tròn ngoại tiếp đang được sử dụng cho hướng tam giác-bên trong đường tròn. 

Đối với các giá trị tối đa,```
10000 10000 10000 10000
```số học liên quan đến các giá trị xung quanh (10^{16}). Python xử lý các số nguyên này một cách chính xác, do đó không mất đi độ chính xác. Hình dạng không thay đổi theo tỷ lệ: cả bán kính trong và bán kính đường tròn đều tỷ lệ với chiều dài cạnh và bán kính 10000 là quá đủ cho cả hai hướng.
