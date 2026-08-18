---
title: "CF 102267H - Vòng tròn đa giác"
description: "Chúng ta có một đa giác đều có các đỉnh (V), trong đó mỗi cạnh đều có độ dài (S). Bởi vì đa giác là đều, tất cả các đỉnh của nó nằm trên một đường tròn có tâm tại tâm đa giác. Nhiệm vụ là tính diện tích hình tròn đó."
date: "2026-08-17T19:24:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "H"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 194
verified: false
draft: false
---

[CF 102267H - Vòng tròn đa giác](https://codeforces.com/problemset/problem/102267/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đều có các đỉnh (V), trong đó mỗi cạnh đều có độ dài (S). Bởi vì đa giác là đều, tất cả các đỉnh của nó nằm trên một đường tròn có tâm tại tâm đa giác. Nhiệm vụ là tính diện tích hình tròn đó. 

Đại lượng quan trọng là bán kính đường tròn (R), khoảng cách từ tâm của đa giác đến bất kỳ đỉnh nào. Khi đã biết (R), khu vực được yêu cầu chỉ đơn giản là 

[ 
A=\pi R^2. 
] 

Số đỉnh thỏa mãn (3\le V\le359), do đó, ngay cả thuật toán tuyến tính hoặc bậc hai cũng sẽ đủ nhỏ trong thực tế. Độ dài cạnh có thể lớn bằng (10^9), vì vậy các giá trị trung gian có thể vào khoảng (10^{18}). Các số nguyên Python xử lý các giá trị đó một cách an toàn và phép tính cuối cùng sử dụng các hàm lượng giác dấu phẩy động vì câu trả lời yêu cầu độ chính xác bằng số. 

Phần thú vị không phải là kích thước của đầu vào mà là nhận dạng hình học. Một đa giác đều có thể được chia thành (V) các tam giác cân bằng cách nối tâm của nó với mọi đỉnh. Mỗi tam giác có hai cạnh bằng (R), trong khi đáy của nó là cạnh đa giác (S). Góc ở tâm là (2\pi/V). 

Có một số trường hợp hình học bất cẩn có thể âm thầm đưa ra đáp án sai. Ví dụ, với đầu vào`3 2`, đa giác là một tam giác đều. Bán kính đường tròn của nó là (2/\sqrt3), nên diện tích của hình tròn là (4\pi/3), xấp xỉ (4.188790205). Sử dụng (S/\sin(\pi/V)) thay vì (S/(2\sin(\pi/V))) sẽ làm cho bán kính lớn gấp đôi và diện tích quá lớn gấp bốn lần. 

Một lỗi phổ biến khác là sử dụng độ với hàm lượng giác mong đợi radian. Vì`8 2`, góc liên quan là (\pi/8), không phải (22.5) được truyền trực tiếp cho`sin`. Câu trả lời đúng là khoảng`21.452136491`. 

Giới hạn trên (V=359) cũng có nghĩa là không cần xử lý đặc biệt các đa giác rất lớn. Công thức vẫn hoạt động tốt về mặt số trong phạm vi cho phép vì (\sin(\pi/V)) là dương với mọi (V) được cho phép. 

## Phương pháp tiếp cận 

Việc triển khai hình học trực tiếp có thể xây dựng rõ ràng các đỉnh (V) xung quanh một vòng tròn, tính toán tọa độ của chúng và sau đó khôi phục bán kính hoặc xác minh độ dài cạnh. Cách tiếp cận đó đúng vì một đa giác đều được xác định hoàn toàn bởi bán kính đường tròn và khoảng cách góc của nó. Phải mất (O(V)) công việc và (O(V)) bộ nhớ nếu tất cả các đỉnh được lưu trữ. Ở mức tối đa (V=359), điều đó có nghĩa là chỉ có 359 đỉnh, vì vậy cách tiếp cận này dễ dàng đủ nhanh theo các ràng buộc đã cho. 

Một cách tiếp cận vũ phu phức tạp hơn có thể tìm kiếm bán kính bằng số. Đối với mỗi bán kính ứng viên, chúng ta có thể tính độ dài cạnh tương ứng (2R\sin(\pi/V)), sau đó sử dụng tìm kiếm nhị phân cho đến khi bán kính đủ chính xác. Ví dụ: với 60 lần lặp, điều này đòi hỏi khoảng (60) đánh giá lượng giác hoặc (60) đánh giá nếu góc không đổi được tính toán trước. Ngay cả điều này cũng dễ dàng nằm trong giới hạn. 

Vì vậy, không giống như nhiều vấn đề của Codeforce, các ràng buộc không thực sự làm cho quá trình xây dựng thô bạo trở nên quá chậm. Giải pháp tối ưu đến từ việc loại bỏ các tính toán không cần thiết thay vì giải cứu một thuật toán sắp hết thời gian chờ. Cấu trúc của một đa giác đều đưa ra một công thức chính xác cho bán kính, do đó không có lý do gì để xây dựng các đỉnh hoặc thực hiện tìm kiếm số. 

Xét một trong các tam giác (V) được tạo bởi tâm và hai đỉnh lân cận. Hai cạnh bằng nhau của nó có chiều dài (R), đáy của nó có chiều dài (S) và góc đỉnh ở tâm là (2\pi/V). Việc chia tam giác đó xuống giữa sẽ tạo ra một tam giác vuông có cạnh huyền (R), cạnh đối diện (S/2) và góc (\pi/V). Như vậy 

[ 
\sin\left(\frac{\pi}{V}\right)=\frac{S/2}{R}. 
] 

Sắp xếp lại mang lại 

[ 
R=\frac{S}{2\sin(\pi/V)}. 
] 

Thay thế công thức này vào công thức diện tích hình tròn sẽ cho 

[ 
A=\pi\left(\frac{S}{2\sin(\pi/V)}\right)^2. 
] 

Do đó, toàn bộ vấn đề được rút gọn thành một phép tính sin và một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng rõ ràng đa giác | (O(V)) | (O(V)) | Được chấp nhận, nhưng không cần thiết | 
| Tìm kiếm bán kính số | (O(K)) | (O(1)) | Được chấp nhận, nhưng không cần thiết | 
| Công thức hình học | (O(1)) | (O(1)) | Đã chấp nhận | 

Ở đây (K) là số lần lặp được sử dụng bởi tìm kiếm số. Giải pháp dựa trên công thức được ưu tiên hơn vì nó chính xác đến độ chính xác của hàm lượng giác dấu phẩy động tiêu chuẩn và không có tham số hội tụ. 

## Hướng dẫn thuật toán 

1. Đọc (V), số đỉnh đa giác và (S), độ dài cạnh chung. 
2. Tính nửa góc 

[ 
\theta=\frac{\pi}{V}. 
] 

Mỗi góc ở tâm là (2\pi/V), và chia một trong các tam giác cân tương ứng làm đôi sẽ được góc (\pi/V). 

1. Tính bán kính đường tròn bằng cách sử dụng 

[ 
R=\frac{S}{2\sin\theta}. 
] 

Nửa cạnh (S/2) đối diện với (\theta) trong tam giác vuông thu được, trong khi (R) là cạnh huyền của nó. 

1. Tính diện tích hình tròn ngoại tiếp là 

[ 
A=\pi R^2. 
] 

1. In kết quả dưới dạng số dấu phẩy động. của Python`float`cung cấp độ chính xác cao hơn đáng kể so với dung sai (10^{-6}) yêu cầu. 

Tại sao nó hoạt động 

Bất biến đằng sau đạo hàm là mọi cặp đỉnh lân cận và tâm đa giác tạo thành một tam giác cân giống nhau. Góc ở tâm của nó chính xác là (2\pi/V), và việc giảm một nửa nó sẽ tạo ra một tam giác vuông có cạnh đối diện bằng đúng một nửa cạnh của đa giác. Do đó, mối quan hệ sin xác định cùng một bán kính cho mọi đỉnh: 

[ 
S/2=R\sin(\pi/V). 
] 

Do đó, (R) được tính toán là khoảng cách thực tế từ tâm đa giác đến mọi đỉnh, chính xác là bán kính của đường tròn ngoại tiếp. Bình phương bán kính đó và nhân với (\pi) sẽ được diện tích cần tìm. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

v, s = map(int, input().split())

angle = math.pi / v
radius = s / (2.0 * math.sin(angle))
area = math.pi * radius * radius

print("{:.10f}".format(area))
```Đầu vào được đọc một lần vì bài toán chứa một đa giác đơn.`math.pi / v`tính một nửa của một góc ở tâm, là góc mà đạo hàm tam giác vuông yêu cầu. 

Mẫu số là`2.0 * math.sin(angle)`. Hệ số hai xuất phát từ thực tế là tam giác vuông chứa một nửa cạnh đa giác, (S/2). Bỏ qua yếu tố này là lỗi công thức trực tiếp nhất cho vấn đề này. 

Bán kính được bình phương trước khi nhân với (\pi), khớp chính xác (A=\pi R^2). Python không bị tràn số nguyên ở đây vì phép tính được thực hiện với các giá trị dấu phẩy động và số nguyên Python cũng sẽ tự động tăng nếu sử dụng số nguyên trung gian. 

Việc in mười chữ số sau dấu thập phân chính xác hơn một cách thoải mái so với lỗi tuyệt đối hoặc tương đối (10^{-6}) bắt buộc. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, đầu vào là`8 2`. Nửa góc ở tâm là (\pi/8), nên bán kính là 

[ 
R=\frac{2}{2\sin(\pi/8)} 
=\frac{1}{\sin(\pi/8)} 
\ khoảng 2,6131259298. 
] 

Diện tích kết quả là khoảng (21,452136491). 

| Bước | (V) | (S) | (\theta=\pi/V) | (R=S/(2\sin\theta)) | Khu vực | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 8 | 2 | | | | 
| Góc tính toán | 8 | 2 | 0,3926990817 | | | 
| Tính bán kính | 8 | 2 | 0,3926990817 | 2.6131259298 | | 
| Khu vực tính toán | 8 | 2 | 0,3926990817 | 2.6131259298 | 21.452136491 | 

Dấu vết này cho thấy tại sao góc ở tâm phải chia cho hai trước khi áp dụng sin. Góc ở giữa đầy đủ là (\pi/4), nhưng tam giác vuông sử dụng (\pi/8). 

Đối với ví dụ thứ hai, hãy xem xét`3 2`. Đây là một tam giác đều. Nửa góc ở tâm là (\pi/3), có sin là (\sqrt3/2). Do đó 

[ 
R=\frac{2}{2(\sqrt3/2)} 
=\frac{2}{\sqrt3}, 
] 

và diện tích hình tròn là (4\pi/3), xấp xỉ (4.1887902048). 

| Bước | (V) | (S) | (\theta=\pi/V) | (R=S/(2\sin\theta)) | Khu vực | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 3 | 2 | | | | 
| Góc tính toán | 3 | 2 | 1.0471975512 | | | 
| Tính bán kính | 3 | 2 | 1.0471975512 | 1.1547005384 | | 
| Khu vực tính toán | 3 | 2 | 1.0471975512 | 1.1547005384 | 4.1887902048 | 

Ví dụ này thực hiện số đỉnh tối thiểu được phép. Nó cũng xác nhận rằng công thức hoạt động đối với đa giác đều nhỏ nhất được bài toán cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ có một đánh giá lượng giác và một số phép tính số học không đổi được thực hiện. | 
| Không gian | (O(1)) | Chỉ các giá trị đầu vào, góc, bán kính và diện tích được lưu trữ. | 

Các ràng buộc cho phép (O(V)) thậm chí hoạt động vì (V\le359), nhưng giải pháp dựa trên công thức là thời gian không đổi bất kể kích thước đa giác. Việc sử dụng bộ nhớ của nó cũng không đổi và độ chính xác bằng số của dấu phẩy động có độ chính xác kép của Python là đủ cho dung sai (10^{-6}) cần thiết. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra bên dưới so sánh các kết quả dấu phẩy động với dung sai thay vì yêu cầu các chuỗi thập phân giống hệt nhau. Đây là cách chính xác để kiểm tra một giải pháp hình học số vì các cách triển khai hợp lệ khác nhau có thể in các chữ số cuối hơi khác nhau.```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline
    v, s = map(int, input().split())

    angle = math.pi / v
    radius = s / (2.0 * math.sin(angle))
    area = math.pi * radius * radius

    print("{:.10f}".format(area))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def assert_close(inp: str, expected: float, message: str):
    actual = float(run(inp))
    assert abs(actual - expected) <= 1e-6 * max(1.0, abs(expected)), message

# Provided sample
assert_close(
    "8 2\n",
    21.452136491,
    "sample 1"
)

# Minimum number of vertices
assert_close(
    "3 2\n",
    4.1887902047863905,
    "minimum vertices"
)

# A square with side 2 has circumradius sqrt(2)
assert_close(
    "4 2\n",
    2.0 * math.pi,
    "square"
)

# Maximum number of vertices and maximum side length
v = 359
s = 10**9
expected = math.pi * (s / (2.0 * math.sin(math.pi / v))) ** 2
assert_close(
    f"{v} {s}\n",
    expected,
    "maximum constraints"
)

# Smallest side length
assert_close(
    "3 1\n",
    math.pi / 3.0,
    "minimum side length"
)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`8 2`|`21.452136491`| Cung cấp mẫu và công thức chung | 
|`3 2`|`4.188790205`| Số đỉnh tối thiểu | 
|`4 2`|`6.283185307`| Hình học vuông và xử lý góc trung tâm | 
|`359 1000000000`| Khoảng (4,09\times10^{19}) | Ràng buộc tối đa và giá trị số lớn | 
|`3 1`|`1.047197551`| Chiều dài cạnh tối thiểu | 

Trường hợp kích thước tối đa đặc biệt hữu ích để phát hiện các triển khai vô tình sử dụng số học số nguyên cho phần lượng giác hoặc mất quá nhiều độ chính xác trong khi bình phương bán kính lớn. 

## Vỏ cạnh 

Đối với đa giác tối thiểu, đầu vào`3 2`cho ra một tam giác đều. Thuật toán tính toán (\theta=\pi/3), do đó (\sin\theta=\sqrt3/2). Bán kính trở thành (2/(2\cdot\sqrt3/2)=2/\sqrt3) và diện tích là (4\pi/3\approx4.188790205). Một công thức thiếu hệ số hai sẽ trả về bốn lần diện tích này. 

Đối với hình vuông`4 2`, nửa góc ở tâm là (\pi/4). Vì (\sin(\pi/4)=\sqrt2/2), nên bán kính là (2/(2\cdot\sqrt2/2)=\sqrt2). Do đó, diện tích hình tròn là (2\pi\khoảng6,283185307). Đây là một kiểm tra hữu ích khi vô tình sử dụng toàn bộ góc ở tâm thay vì một nửa. 

Đối với đa giác được phép lớn nhất,`359 1000000000`, nửa góc ở tâm rất nhỏ nhưng vẫn dương an toàn. Hàm sin trả về một giá trị đủ chính xác và bán kính thu được lớn vì đa giác có độ dài cạnh rất lớn. Biểu diễn dấu phẩy động của Python có thể xử lý thoải mái vùng kết quả theo thứ tự (10^{19}). 

Với độ dài cạnh nhỏ nhất,`3 1`, phép tính tam giác tương tự sẽ cho (R=1/\sqrt3) và diện tích (\pi/3\approx1.047197551). Điều này khẳng định rằng không có trường hợp đặc biệt nào gắn với độ lớn của (S); công thức tỉ lệ bậc hai với độ dài cạnh. 

Thất bại phổ biến khác là đỗ bằng cấp`sin`. Vì`8 2`, đối số đúng là (\pi/8\approx0.392699) radian. Đi qua`22.5`trực tiếp đến Python`math.sin`sẽ hiểu nó là radian và tạo ra một giá trị không liên quan. Việc triển khai sẽ tránh được lỗi đó bằng cách lấy góc trực tiếp từ`math.pi`.
