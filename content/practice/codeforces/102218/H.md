---
title: "CF 102218H - Đài phát thanh Heartbreaker"
description: "Chúng ta có (n) sóng hình sin. Mỗi sóng dao động với cùng tần số góc (omega), nhưng mỗi sóng có biên độ (Ai) và pha (phii) riêng."
date: "2026-08-18T12:52:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "H"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 147
verified: false
draft: false
---

[CF 102218H - Đài phát thanh Heartbreaker](https://codeforces.com/problemset/problem/102218/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) sóng hình sin. Mỗi sóng dao động với cùng tần số góc (\omega), nhưng mỗi sóng có biên độ (A_i) và pha (\phi_i) riêng. Tổng của chúng được đảm bảo có thể biểu diễn dưới dạng một hình sin nữa có cùng tần số đó và chúng ta cần khôi phục biên độ (A) và pha (\phi) của nó. 

Đầu vào đưa ra (n), tần số chung và sau đó là (n) cặp ((A_i,\phi_i)). Đầu ra là một cặp ((A,\phi)) sao cho 

A\sin(\omega t+\phi) 
] 

với mọi (t), với (A\ge 0) và (0\le\phi<2\pi). 

Bản thân tần số không ảnh hưởng đến việc tính toán. Vì mọi số hạng đều có số hạng hoàn toàn giống nhau (\omega), nên chúng ta chỉ cần kết hợp biên độ và pha của chúng. 

Giá trị (n) có thể đạt tới (10^5). Một giải pháp so sánh mọi sóng với mọi sóng khác sẽ thực hiện khoảng (10^{10}) phép tính theo cặp trong trường hợp xấu nhất, vượt xa giới hạn thời gian hai giây. Chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính. Biên độ tối đa là (100), do đó hệ số tích lũy tối đa là khoảng (10^7), nằm thoải mái trong phạm vi dấu phẩy động thông thường. Các giá trị pha đã được tính bằng radian và nằm trong một vòng quay đầy đủ. 

Có một số trường hợp cạnh số có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Hãy xem xét một sóng duy nhất:```
1 1
5 0
```Kết quả đúng (5) với pha (0). Một giải pháp sửa đổi pha một cách không cần thiết hoặc sử dụng công thức liên quan đến phép chia cho một thành phần lượng giác có thể thất bại khi thành phần đó bằng 0. 

Trường hợp thứ hai là sóng hướng ngược lại:```
1 1
5 3.141592653589793
```Kết quả có biên độ (5) và pha (\pi). Tính toán pha với`atan(y / x)`không an toàn vì (x) có thể bằng 0 và quan trọng hơn là dấu của (x) và (y) xác định góc phần tư.`atan2`được thiết kế cho chính xác tình huống này. 

Cuối cùng, có thể hủy hoàn toàn:```
2 1
1 0
1 3.141592653589793
```Hai sóng âm của nhau nên kết quả là hàm số 0. Biên độ của nó là (0) và pha của nó không liên quan về mặt toán học vì (0\sin(\omega t+\phi)=0) với mọi (\phi). Việc triển khai dấu phẩy động có thể để lại một phần dư nhỏ thay vì số 0 chính xác, điều này vô hại trong phạm vi dung sai lỗi yêu cầu. 

Ngoài ra còn có trường hợp biên xung quanh (2\pi). Ví dụ,```
2 1
1 0
1 6.283185307079586
```có pha tổng cực gần (2\pi), không phải là góc âm. Từ`atan2`trả về các góc trong ([-\pi,\pi]), kết quả âm phải được dịch chuyển theo (2\pi). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng đánh giá tổng dưới dạng hàm của (t), có lẽ tại một số điểm, sau đó khôi phục biên độ và pha từ các giá trị đó. Điều đó là không cần thiết và việc đánh giá nhiều điểm cho mỗi đợt sẽ chỉ tốn thêm công sức. Một cách tiếp cận đại số mạnh mẽ hơn có thể kết hợp nhiều lần hai hình sin bằng cách sử dụng đồng nhất thức lượng giác. Mặc dù mỗi sự kết hợp riêng lẻ đều đúng, nhưng việc thao tác lặp đi lặp lại các biểu thức vẫn có thể gây ra công việc không cần thiết và độ phức tạp về số. Nếu mọi cặp sóng đều được xem xét thì trường hợp xấu nhất là theo thứ tự các phép toán (n^2=10^{10}). 

Quan sát hữu ích là độ lệch pha có thể được mở rộng trước khi thực hiện bất kỳ phép tính tổng nào. Đối với một làn sóng, 

A_i\sin(\omega t)\cos\phi_i 
+ 
A_i\cos(\omega t)\sin\phi_i. 
] 

Do đó, mọi sóng chỉ là sự kết hợp tuyến tính của hai hàm giống nhau, (\sin(\omega t)) và (\cos(\omega t)). Chúng ta có thể cộng các hệ số của chúng một cách độc lập. 

Xác định 

[ 
X=\sum_{i=1}^{n} A_i\cos\phi_i 
] 

và 

[ 
Y=\sum_{i=1}^{n} A_i\sin\phi_i. 
] 

Khi đó tổng đầy đủ sẽ trở thành 

[ 
f(t)=X\sin(\omega t)+Y\cos(\omega t). 
] 

Bây giờ hãy mở rộng sóng đơn mong muốn: 

A\cos\phi\sin(\omega t) 
+ 
A\sin\phi\cos(\omega t). 
] 

Hệ số phù hợp cho 

[ 
A\cos\phi=X, 
\qquad 
A\sin\phi=Y. 
] 

Cặp ((X,Y)) có thể được xem dưới dạng vectơ hai chiều. Chiều dài của nó là biên độ thu được, 

[ 
A=\sqrt{X^2+Y^2}, 
] 

và hướng của nó là pha kết quả, 

[ 
\phi=\operatorname{atan2}(Y,X). 
] 

Điều này làm giảm toàn bộ vấn đề xuống còn một lần thông qua đầu vào. Lực lượng vũ phu hoạt động vì sóng hình sin có thể được kết hợp theo đại số, nhưng nó không khai thác được thực tế là mọi sóng đều sử dụng hai hàm cơ bản giống nhau. Nhận xét rằng tất cả các số hạng đều quy về hệ số (\sin(\omega t)) và (\cos(\omega t)) cho phép chúng ta thay thế toàn bộ tập hợp sóng bằng một vectơ hai chiều tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (\omega). Giá trị của (\omega) sau đó không cần thiết vì nó giống hệt nhau đối với mọi sóng, vì vậy tất cả công việc có thể được thực hiện bằng cách sử dụng biên độ và pha. 
2. Khởi tạo hai bộ tích lũy, (X=0) và (Y=0). Họ sẽ lưu trữ các hệ số của (\sin(\omega t)) và (\cos(\omega t)) trong tổng đầy đủ. 
3. Với mỗi sóng ((A_i,\phi_i)), cộng (A_i\cos\phi_i) với (X) và (A_i\sin\phi_i) với (Y). Điều này diễn ra trực tiếp từ việc mở rộng (\sin(\omega t+\phi_i)). 
4. Tính biên độ thu được với 

[ 
A=\tên toán tử{hypot}(X,Y). 
] 

Đây là độ dài Euclide của vectơ hệ số và tốt hơn về mặt số học so với cách viết thủ công (\sqrt{X^2+Y^2}). 
5. Tính pha với 

[ 
\phi=\operatorname{atan2}(Y,X). 
]`atan2`sử dụng cả hai tọa độ, do đó nó chọn góc phần tư chính xác. của Python`math.atan2`trả về một góc trong phạm vi từ (-\pi) đến (\pi). 
6. Nếu pha âm thì cộng (2\pi). Khoảng đầu ra được yêu cầu là ([0,2\pi)), do đó, điều này sẽ chuyển đổi`atan2`quy ước theo quy ước mà bài toán yêu cầu. 
7. In (A) và (\phi) có đủ chữ số thập phân. In mười hai chữ số sau dấu thập phân mang lại độ chính xác cao hơn nhiều so với yêu cầu (10^{-6}). 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố sóng nào, (X) chính xác là hệ số do tiền tố đó đóng góp cho (\sin(\omega t)), trong khi (Y) chính xác là hệ số của nó cho (\cos(\omega t)). Việc thêm một sóng khác sẽ cập nhật các hệ số này một cách chính xác (A_i\cos\phi_i) và (A_i\sin\phi_i), do đó, bất biến vẫn đúng cho mọi dòng đầu vào. 

Sau khi tất cả các sóng đã được xử lý, hàm hoàn chỉnh là 

[ 
X\sin(\omega t)+Y\cos(\omega t). 
] 

Việc chọn (A=\sqrt{X^2+Y^2}) và (\phi=\operatorname{atan2}(Y,X)) sẽ có (A\cos\phi=X) và (A\sin\phi=Y). Việc thay thế các danh tính đó vào (A\sin(\omega t+\phi)) sẽ tái tạo chính xác hàm tích lũy, ngoài việc làm tròn dấu phẩy động. Việc chuẩn hóa pha chỉ thêm một vòng quay đầy đủ, không làm thay đổi hình sin. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, omega = input().split()
    n = int(n)

    x = 0.0
    y = 0.0

    for _ in range(n):
        a, phi = map(float, input().split())
        x += a * math.cos(phi)
        y += a * math.sin(phi)

    amplitude = math.hypot(x, y)
    phase = math.atan2(y, x)

    if phase < 0.0:
        phase += 2.0 * math.pi

    print(f"{amplitude:.12f} {phase:.12f}")

if __name__ == "__main__":
    solve()
```Dòng đầu tiên ghi`omega`, nhưng việc thực hiện có chủ ý không sử dụng nó. Tần số chung đã có sẵn trong mọi thuật ngữ và không bao giờ thay đổi trong quá trình khớp hệ số. 

Các biến`x`Và`y`tương ứng trực tiếp với hai hệ số dẫn xuất trong thuật toán. Mỗi sóng đầu vào đóng góp một vectơ có chiều dài (A_i) ở góc (\phi_i), do đó việc tích lũy hai thành phần này tương đương với việc cộng vectơ.`math.hypot(x, y)`tính độ dài của vectơ tích lũy đó. Tài liệu Python`hypot`như chuẩn Euclide cho các đối số của nó, đây chính xác là phép tính biên độ được yêu cầu ở đây.`math.atan2(y, x)`được sử dụng thay vì`math.atan(y / x)`. Ngoài việc tránh chia cho 0, nó còn giữ nguyên dấu của cả hai tọa độ và do đó chọn đúng góc phần tư. 

Việc điều chỉnh pha âm được thực hiện có chủ ý sau`atan2`. Chỉ thêm (2\pi) khi kết quả là âm sẽ ánh xạ góc trả về vào khoảng cần thiết mà không thay đổi sin hoặc cos của nó. 

Không có số học số nguyên nào được tham gia vào các phép tính lượng giác, do đó việc tràn số nguyên không phải là vấn đề. Tọa độ tích lũy lớn nhất có thể là khoảng (10^7), được biểu thị dễ dàng bằng số dấu phẩy động có độ chính xác kép. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu đầu tiên, mỗi sóng đóng góp một vectơ 

[ 
(A_i\cos\phi_i,\A_i\sin\phi_i). 
] 

Bảng sau đây hiển thị vectơ tích lũy sau mỗi dòng đầu vào. Các giá trị được làm tròn ở đây để dễ đọc; chương trình giữ độ chính xác đầy đủ của dấu phẩy động. 

| Sóng | (A_i) | (\phi_i) | (X) sau sóng | (Y) sau sóng | 
| --- | --- | --- | --- | --- | 
| 1 | 93,22 | 5,53 | 67,96 | -63,80 | 
| 2 | 48,58 | 0,86 | 99,65 | -26,99 | 
| 3 | 15.31 | 5,39 | 109,24 | -38,93 | 
| 4 | 5,66 | 4.12 | 106.08 | -43,63 | 
| 5 | 48,53 | 6.09 | 153,71 | -52,95 | 
| 6 | 6h60 | 1,42 | 154,70 | -46,43 | 
| 7 | 21.15 | 0,06 | 175,81 | -45,16 | 
| 8 | 4.27 | 5,47 | 178,74 | -48,26 | 

Vectơ cuối cùng hướng xuống phía dưới trục dương (X) một chút, do đó`atan2`trả về một góc âm nhỏ. Việc thêm (2\pi) sẽ di chuyển nó vào phạm vi được yêu cầu. Việc sử dụng các giá trị nội bộ không làm tròn sẽ cho kết quả xấp xỉ 

[ 
A=185,184472750, 
\qquad 
\phi=6.019915094, 
] 

phù hợp với đầu ra mẫu. 

Dấu vết thể hiện tính bất biến chính: bất kể có bao nhiêu sóng đã được xử lý, hai giá trị tích lũy đều mô tả đầy đủ tổng của chúng ở tần số chung. 

### Ví dụ được xây dựng 

Hãy xem xét```
2 1
1 0
1 1.5707963267948966
```Sóng đầu tiên là (\sin(t)). Thứ hai là (\sin(t+\pi/2)=\cos(t)). 

| Sóng | (A_i) | (\phi_i) | (X) sau sóng | (Y) sau sóng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1.000000 | 0,000000 | 
| 2 | 1 | (\pi/2) | 1.000000 | 1.000000 | 

Biên độ cuối cùng là 

[ 
A=\sqrt{1^2+1^2}=\sqrt2, 
] 

và pha là 

[ 
\phi=\operatorname{atan2}(1,1)=\frac{\pi}{4}. 
] 

Như vậy kết quả là```
1.414213562373 0.785398163397
```Ví dụ này làm cho việc khớp hệ số trở nên đặc biệt rõ ràng vì hai sóng ban đầu đóng góp trực tiếp vào các hàm cơ sở khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi sóng (n) yêu cầu một sin, một cosin và số học theo thời gian không đổi. | 
| Không gian | (O(1)) | Chỉ có hai hệ số tích lũy và một vài giá trị vô hướng được lưu trữ. | 

Với (n\le 10^5), thuật toán chỉ thực hiện một lần chuyển qua đầu vào và không bao giờ lưu trữ danh sách sóng. Việc sử dụng bộ nhớ của nó là không đổi và thời gian của nó tăng tuyến tính theo số lượng sóng, phù hợp một cách thoải mái với các giới hạn đã nêu. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra bên dưới kiểm tra các câu trả lời bằng số thay vì so sánh các chuỗi được định dạng. Điều này là cần thiết vì nhiều cách biểu diễn số thập phân khác nhau có thể thỏa mãn khả năng chịu lỗi của bài toán.```python
import math
import io
import sys

def solve_text(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    omega = float(next(it))

    x = 0.0
    y = 0.0

    for _ in range(n):
        a = float(next(it))
        phi = float(next(it))
        x += a * math.cos(phi)
        y += a * math.sin(phi)

    amplitude = math.hypot(x, y)
    phase = math.atan2(y, x)

    if phase < 0.0:
        phase += 2.0 * math.pi

    return f"{amplitude:.12f} {phase:.12f}"

def run(inp: str):
    out = solve_text(inp)
    a, p = map(float, out.split())
    return a, p

def phase_distance(a, b):
    d = abs(a - b) % (2.0 * math.pi)
    return min(d, 2.0 * math.pi - d)

# Provided sample
a, p = run(
    """8 66.82
93.22 5.53
48.58 0.86
15.31 5.39
5.66 4.12
48.53 6.09
6.60 1.42
21.15 0.06
4.27 5.47
"""
)
assert abs(a - 185.184472750) <= 1e-6
assert phase_distance(p, 6.019915094) <= 1e-6

# Minimum-size input
a, p = run(
    """1 0.1
5 0
"""
)
assert abs(a - 5.0) <= 1e-9
assert phase_distance(p, 0.0) <= 1e-9

# All waves identical
a, p = run(
    """3 2
2 2.0943951023931953
2 2.0943951023931953
2 2.0943951023931953
"""
)
assert abs(a - 6.0) <= 1e-9
assert phase_distance(p, 2.0943951023931953) <= 1e-9

# Exact cancellation
a, p = run(
    """2 1
1 0
1 3.141592653589793
"""
)
assert abs(a) <= 1e-9

# Phase near the 2*pi boundary
a, p = run(
    """2 1
1 0
1 6.283185207179586
"""
)
assert abs(a - 2.0) <= 1e-7
assert phase_distance(p, 2.0 * math.pi - 5e-8) <= 1e-7

# Maximum-size input
n = 100000
maximum_input = str(n) + " 100\n" + ("100 0\n" * n)
a, p = run(maximum_input)
assert abs(a - 10000000.0) <= 1e-5
assert phase_distance(p, 0.0) <= 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0.1 / 5 0`| (5,0) | Đầu vào tối thiểu và pha chính xác bằng 0 | 
| Ba sóng giống nhau có pha (2\pi/3) | (6,2\pi/3) | Tích lũy tuyến tính của các vectơ bằng nhau | 
|`1 0 / 1 π`| (0), pha tùy ý | Hủy bỏ hoàn toàn và phần dư dấu phẩy động | 
| Hai pha gần (0) và (2\pi) | Biên độ gần (2), pha gần (2\pi) | Xử lý pha tròn đúng cách | 
| (100000) sóng giống nhau | (10^7,0) | Tối đa (n), độ phức tạp tuyến tính và cường độ tích lũy | 

## Vỏ cạnh 

Một sóng đơn có pha 0,```
1 0.1
5 0
```tạo ra (X=5) và (Y=0). Biên độ là (5) và`atan2(0,5)`cho pha không. Không có sự phân chia cho (X) hoặc (Y), nên trường hợp trục được xử lý một cách tự nhiên. 

Một pha của (\pi),```
1 1
5 3.141592653589793
```tạo ra (X=-5) và (Y) rất gần bằng 0.`atan2`nhìn thấy tọa độ âm (X) và trả về một góc gần (\pi), trong khi một đơn vị`atan(Y/X)`Cách tiếp cận này có thể làm mất thông tin góc phần tư. 

Để hủy bỏ hoàn toàn,```
2 1
1 0
1 3.141592653589793
```tổng vectơ toán học là ((0,0)). Đánh giá dấu phẩy động của Python về (\sin(\pi)) có thể để lại một giá trị nhỏ thay vì chính xác bằng 0, nhưng`hypot`vẫn tạo ra biên độ theo thứ tự độ chính xác của máy. Tỷ lệ này thấp hơn nhiều so với sai số tuyệt đối bắt buộc (10^{-6}), vì vậy kết quả tính toán thể hiện chính xác hàm số 0. 

Đối với pha gần (2\pi),```
2 1
1 0
1 6.283185207179586
```vectơ thứ hai gần giống với vectơ thứ nhất, nhưng có điểm vô cùng nhỏ bên dưới trục dương (X).`atan2`do đó trả về một pha âm nhỏ. Việc bổ sung rõ ràng (2\pi) sẽ chuyển đổi nó thành một giá trị ngay bên dưới (2\pi), thỏa mãn phạm vi đầu ra được yêu cầu. 

Trường hợp kích thước tối đa bao gồm (100000) sóng giống hệt nhau có biên độ (100) và pha bằng 0. Mỗi vectơ đóng góp ((100,0)), do đó vectơ cuối cùng là ((10^7,0)), cho biên độ (10^7) và pha bằng 0. Thuật toán vẫn thực hiện chính xác một lượng công việc không đổi trên mỗi sóng, do đó kích thước đầu vào thay đổi thời gian chạy theo tuyến tính thay vì bậc hai.
