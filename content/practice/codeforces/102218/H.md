---
title: "CF 102218H - Đài phát thanh Heartbreaker"
description: "Chúng ta có (n) tín hiệu hình sin. Mọi tín hiệu đều dao động với cùng tần số góc (omega), nhưng mỗi tín hiệu đều có biên độ (Ai) và pha (phii) riêng."
date: "2026-08-17T23:21:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "H"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 140
verified: false
draft: false
---

[CF 102218H - Đài phát thanh Heartbreaker](https://codeforces.com/problemset/problem/102218/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) tín hiệu hình sin. Mọi tín hiệu đều dao động với cùng tần số góc (\omega), nhưng mỗi tín hiệu có biên độ (A_i) và pha (\phi_i) riêng. Tổng của chúng được đảm bảo vẫn là hình sin có cùng tần số, do đó nhiệm vụ là khôi phục biên độ (A) và pha (\phi) của tín hiệu tương đương đơn. 

Đầu vào chứa (n), tần số chung và sau đó là (n) cặp ((A_i,\phi_i)). Bản thân tần số không cần xuất hiện ở đầu ra vì nó không thay đổi khi thêm tín hiệu. Đầu ra chỉ cần biên độ và pha của sóng thu được, với pha được chuẩn hóa thành khoảng ([0,2\pi)). 

Ràng buộc chính là (n\le 10^5). Với giới hạn 2 giây, giải pháp (O(n)) là mục tiêu tự nhiên. Một phương thức (O(n^2)) sẽ thực hiện khoảng (5\cdot10^9) cặp thao tác ở kích thước đầu vào tối đa, vượt xa những gì có thể phù hợp trong giới hạn thời gian. Biên độ và pha là số thực, do đó cần phải có số học dấu phẩy động và dung sai (10^{-6}) được yêu cầu có nghĩa là chúng ta nên tránh các phép tính không ổn định không cần thiết. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. 

Đầu tiên là giai đoạn gói. Coi như```
2 1
1 6.2
1 0.1
```Hai pha gần với (2\pi) và (0), do đó pha tổng của chúng gần với (0), không gần với (2\pi). Một nguyên`atan2`kết quả có thể âm tính và việc in nó trực tiếp sẽ vi phạm khoảng thời gian pha được yêu cầu. Pha phải được chuẩn hóa bằng modulo (2\pi). 

Thứ hai là hủy bỏ. Coi như```
2 1
1 0
1 3.141592653589793
```Hai sóng hoàn toàn đối lập nhau nên tổng của chúng bằng hàm số 0. Biên độ đúng là (0). Pha này không liên quan về mặt toán học vì (0\sin(x+\phi)=0) với mọi (\phi). Việc triển khai mạnh mẽ có thể trả về pha (0) khi biên độ tính toán bằng 0. 

Thứ ba là chỉ hủy trong một tọa độ. Ví dụ,```
2 1
1 0
1 1.5707963267948966
```Tổng là (\sin x+\cos x=\sqrt2\sin(x+\pi/4)), vì vậy câu trả lời là xấp xỉ```
1.4142135623730951 0.7853981633974483
```Công thức chỉ sử dụng`atan(y/x)`có thể làm mất góc phần tư và tạo ra pha sai.`atan2`là cần thiết vì nó xác định góc từ cả hai tọa độ. 

## Phương pháp tiếp cận 

Một cách đơn giản để suy nghĩ về vấn đề này là kết hợp hai sóng cùng một lúc. Giả sử chúng ta đã biết rằng hai sóng có cùng tần số có thể được biểu diễn bằng một sóng tương đương. Chúng ta có thể kết hợp hai kết quả đầu tiên, sau đó kết hợp kết quả đó với kết quả thứ ba, rồi với kết quả thứ tư, v.v. Phiên bản tuần tự đó đã là tuyến tính, nhưng việc triển khai bạo lực theo nghĩa đen có thể liên tục tính toán lại kết quả của tất cả các cặp sóng còn lại trước khi chọn kết hợp tiếp theo. Quy trình như vậy thực hiện một sự kết hợp cặp cho mỗi cặp sóng, tạo ra 

[ 
\frac{n(n-1)}2 
] 

sự kết hợp. Đối với (n=10^5), tức là (4.999.950.000) kết hợp, do đó cách tiếp cận quá chậm mặc dù mỗi kết hợp riêng lẻ đều có thời gian không đổi. Tính đúng đắn của nó xuất phát từ tính kết hợp của phép cộng, nhưng nó lãng phí công sức khi biểu diễn lặp đi lặp lại các tổng riêng phần giống nhau. 

Quan sát loại bỏ công việc lãng phí này là ngừng lưu trữ hoàn toàn biên độ và pha trung gian. Mở rộng một sóng bằng cách sử dụng đẳng thức cộng góc: 

A_i\sin x\cos\phi_i 
+ 
A_i\cos x\sin\phi_i, 
] 

ở đâu (x=\omega t). 

Do đó, mọi sóng chỉ là tổ hợp tuyến tính của hai hàm cơ bản giống nhau, (\sin x) và (\cos x). Chúng ta có thể thêm tất cả các hệ số một cách độc lập: 

[ 
S=\sum_i A_i\cos\phi_i, 
\qquad 
C=\sum_i A_i\sin\phi_i. 
] 

Tổng đầy đủ sẽ trở thành 

[ 
f(t)=S\sin(\omega t)+C\cos(\omega t). 
] 

Bây giờ hãy mở rộng sóng đơn mong muốn: 

A\cos\phi\sin x+A\sin\phi\cos x. 
] 

Việc so khớp các hệ số mang lại 

[ 
A\cos\phi=S, 
\qquad 
A\sin\phi=C. 
] 

Do đó, biên độ chính là độ dài của vectơ ((S,C)): 

[ 
A=\sqrt{S^2+C^2}. 
] 

Pha là hướng của vectơ đó: 

[ 
\phi=\operatorname{atan2}(C,S). 
] 

Đây là ý tưởng trung tâm. Mỗi sóng ban đầu đóng góp một vectơ hai chiều ((A_i\cos\phi_i,A_i\sin\phi_i)). Việc thêm các sóng cũng giống như việc thêm các vectơ này. Biên độ cuối cùng là độ dài của vectơ và pha cuối cùng là hướng của vectơ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp cặp vũ phu lặp đi lặp lại | (O(n^2)) | (O(1)) | Quá chậm | 
| Tích lũy hệ số | (O(n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và tần số chung (\omega). Giá trị của (\omega) không cần thiết sau đó vì mọi sóng đều có cùng tần số, do đó nó không thay đổi trong tín hiệu cuối cùng. 
2. Khởi tạo hai bộ tích lũy, (S=0) và (C=0). Đối với mỗi sóng, hãy thêm (A_i\cos\phi_i) vào (S) và (A_i\sin\phi_i) vào (C). 

Hai giá trị này chính xác là hệ số của (\sin(\omega t)) và (\cos(\omega t)) trong tổng đầy đủ, do đó không cần lưu trữ từng sóng riêng lẻ. 
3. Tính biên độ cuối cùng là 

[ 
A=\sqrt{S^2+C^2}. 
] 

Điều này suy ra từ các phương trình (S=A\cos\phi) và (C=A\sin\phi), vì bình phương và cộng chúng sẽ cho ra (S^2+C^2=A^2). 
4. Tính pha với 

[ 
\phi=\operatorname{atan2}(C,S). 
] 

Các lập luận được cố tình theo thứ tự này.`atan2(y, x)`trả về hướng của vectơ ((x,y)) và vectơ của chúng ta là ((S,C)). 
5. Nếu pha được trả về bởi`atan2`là âm thì cộng (2\pi). Điều này chuyển đổi góc thành khoảng cần thiết ([0,2\pi)). 
6. In biên độ và pha chuẩn hóa với độ chính xác vừa đủ. Số học dấu phẩy động có độ chính xác kép của Python là quá đủ cho lỗi (10^{-6}) bắt buộc. 

### Tại sao nó hoạt động 

Sau khi xử lý mọi sóng đầu vào, bộ tích lũy thỏa mãn 

[ 
S=\sum_i A_i\cos\phi_i 
] 

và 

[ 
C=\sum_i A_i\sin\phi_i. 
] 

Sử dụng đẳng thức cộng góc, tổng ban đầu bằng chính xác 

[ 
S\sin(\omega t)+C\cos(\omega t). 
] 

Thuật toán chọn (A=\sqrt{S^2+C^2}) và (\phi=\operatorname{atan2}(C,S)), thỏa mãn (A\cos\phi=S) và (A\sin\phi=C). Việc thay thế hai đẳng thức đó thành (A\sin(\omega t+\phi)) sẽ tạo ra biểu thức giống hệt như tổng của tất cả các sóng đầu vào. Do đó, biên độ và pha trả về mô tả tín hiệu tương đương cần thiết. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, omega = input().split()
    n = int(n)
    omega = float(omega)

    s = 0.0
    c = 0.0

    for _ in range(n):
        a, phi = map(float, input().split())
        s += a * math.cos(phi)
        c += a * math.sin(phi)

    amplitude = math.hypot(s, c)
    phase = math.atan2(c, s)

    if phase < 0.0:
        phase += 2.0 * math.pi

    print(f"{amplitude:.12f} {phase:.12f}")

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc số sóng và tần số chung. Mặc dù`omega`được phân tích cú pháp vì nó thuộc định dạng đầu vào, nó không bao giờ được đưa vào phép tính. Đây là hệ quả trực tiếp của việc tất cả các sóng có cùng tần số. 

Vòng lặp thực hiện việc tích lũy hệ số từ thuật toán.`math.cos(phi)`đóng góp vào hệ số của (\sin(\omega t)), trong khi`math.sin(phi)`đóng góp vào hệ số của (\cos(\omega t)). Các biến được đặt tên`s`Và`c`theo các hệ số tích lũy này.`math.hypot(s, c)`tính toán (\sqrt{s^2+c^2}) một cách mạnh mẽ về mặt số học. Nó cũng tránh viết biểu thức căn bậc hai theo cách thủ công và thích hợp hơn khi xử lý độ lớn dấu phẩy động. 

Việc tính toán pha sử dụng`atan2(c, s)`còn hơn là`atan(c / s)`. Cái sau mất góc phần tư bất cứ khi nào`s`là số âm và cũng có vấn đề chia cho 0 khi`s`là số không.`atan2`xử lý trực tiếp cả hai trường hợp. 

Việc chuẩn hóa chỉ kiểm tra xem kết quả có âm tính hay không.`atan2`trả về một góc trong ([-\pi,\pi]), vì vậy việc thêm (2\pi) một lần là đủ để đặt nó vào ([0,2\pi)). Làm tròn dấu phẩy động có thể làm cho biên độ bằng 0 về mặt lý thuyết cực kỳ nhỏ, nhưng điều này không tạo ra vấn đề về tính chính xác vì pha của sóng có biên độ bằng 0 là tùy ý. 

Không có phép nhân số nguyên hoặc mảng số nguyên lớn nào được tham gia, do đó việc tràn số nguyên là không liên quan. Tổng lớn nhất theo thứ tự (10^7), được biểu thị một cách thoải mái bằng loại dấu phẩy động có độ chính xác kép của Python. 

## Ví dụ đã hoạt động 

Vì câu lệnh chỉ cung cấp một mẫu nên dấu vết thứ hai bên dưới sử dụng đầu vào được xây dựng nhỏ để tách biệt việc xử lý phép cộng vectơ và góc phần tư. 

Đối với Mẫu 1, thuật toán tích lũy các thành phần cosin và sin của tất cả tám sóng. 

| Sóng | (A_i) | (\phi_i) | (S) sau sóng | (C) sau sóng | 
| --- | --- | --- | --- | --- | 
| 1 | 93,22 | 5,53 | 65,31 | -66,50 | 
| 2 | 48,58 | 0,86 | 96,98 | -29.40 | 
| 3 | 15.31 | 5,39 | 106,78 | -40,18 | 
| 4 | 5,66 | 4.12 | 103,63 | -43,42 | 
| 5 | 48,53 | 6.09 | 151,96 | -52,77 | 
| 6 | 6h60 | 1,42 | 152,95 | -46,24 | 
| 7 | 21.15 | 0,06 | 174.06 | -44,97 | 
| 8 | 4.27 | 5,47 | 177,00 | -49,99 | 

Các giá trị trung gian hiển thị được làm tròn để dễ đọc. Sử dụng các giá trị dấu phẩy động đầy đủ, vectơ cuối cùng có độ dài xấp xỉ (185.184472750) và hướng của nó xấp xỉ (6.019915094). Hệ số sin tích lũy âm đặt vectơ bên dưới trục dương (S), do đó`atan2`ban đầu trả về một góc âm. Việc thêm (2\pi) sẽ tạo ra pha của mẫu trong phạm vi yêu cầu. 

Để có một ví dụ đơn giản hơn, hãy xem xét```
2 1
1 0
1 1.5707963267948966
```Sóng đầu tiên đóng góp vectơ ((1,0)), trong khi sóng thứ hai đóng góp ((0,1)). 

| Sóng | (S) đóng góp | (C) đóng góp | (S) | (C) | 
| --- | --- | --- | --- | --- | 
| 1 | 1.000000 | 0,000000 | 1.000000 | 0,000000 | 
| 2 | 0,000000 | 1.000000 | 1.000000 | 1.000000 | 

Biên độ cuối cùng là 

[ 
\sqrt{1^2+1^2}=\sqrt2, 
] 

và`atan2(1,1)`cho (\pi/4). Như vậy kết quả là```
1.414213562373 0.785398163397
```Dấu vết này chứng tỏ tại sao cả hai tọa độ tích lũy đều cần thiết và tại sao`atan2`là cách chính xác để phục hồi giai đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi sóng yêu cầu một phép tính sin và một phép tính cosin cộng với số học theo thời gian không đổi. | 
| Không gian | (O(1)) | Chỉ có hai hệ số tích lũy và một vài giá trị vô hướng được lưu trữ. | 

Đối với (n=10^5), thuật toán chỉ thực hiện (10^5) lần lặp và sử dụng bộ nhớ bổ sung không đổi. Điều này thoải mái trong giới hạn 2 giây và 256 MB, trong khi giải pháp thay thế bậc hai sẽ yêu cầu gần năm tỷ kết hợp cặp. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra bên dưới so sánh kết quả dấu phẩy động với dung sai thay vì so sánh chính xác các chuỗi thập phân. Đó là cách thích hợp để kiểm tra vấn đề này vì nhiều câu trả lời tương đương về mặt số có thể khác nhau ở các chữ số in cuối cùng của chúng trong khi vẫn đáp ứng giới hạn lỗi yêu cầu.```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    n, omega = input().split()
    n = int(n)
    omega = float(omega)

    s = 0.0
    c = 0.0

    for _ in range(n):
        a, phi = map(float, input().split())
        s += a * math.cos(phi)
        c += a * math.sin(phi)

    amplitude = math.hypot(s, c)
    phase = math.atan2(c, s)

    if phase < 0.0:
        phase += 2.0 * math.pi

    print(f"{amplitude:.12f} {phase:.12f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        old_stdout = sys.stdout
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def parse_output(out: str):
    a, p = map(float, out.split())
    return a, p

def assert_close(actual: str, expected_a: float, expected_p: float,
                 message: str):
    a, p = parse_output(actual)

    assert math.isclose(a, expected_a, rel_tol=1e-9, abs_tol=1e-9), message
    assert math.isclose(p, expected_p, rel_tol=1e-9, abs_tol=1e-9), message

# Provided sample
sample1 = """\
8 66.82
93.22 5.53
48.58 0.86
15.31 5.39
5.66 4.12
48.53 6.09
6.60 1.42
21.15 0.06
4.27 5.47
"""

assert_close(
    run(sample1),
    185.184472750,
    6.019915094,
    "sample 1"
)

# Minimum-size input
assert_close(
    run("1 0.1\n5 1.2\n"),
    5.0,
    1.2,
    "single wave"
)

# Two perpendicular waves
assert_close(
    run("2 1\n1 0\n1 1.5707963267948966\n"),
    math.sqrt(2.0),
    math.pi / 4.0,
    "perpendicular waves"
)

# Exact cancellation
assert_close(
    run("2 10\n1 0\n1 3.141592653589793\n"),
    0.0,
    0.0,
    "exact cancellation"
)

# Boundary phases near 0 and 2*pi
eps = 1e-10
assert_close(
    run(f"2 100\n1 {2 * math.pi - eps}\n1 {eps}\n"),
    2.0,
    0.0,
    "phase wrapping"
)

# Maximum-size input with identical waves
n = 100000
large_input = f"{n} 50\n" + ("1 0\n" * n)
assert_close(
    run(large_input),
    100000.0,
    0.0,
    "maximum-size input"
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0.1 / 5 1.2`|`5 1.2`| Đầu vào kích thước tối thiểu và bảo toàn một sóng | 
|`2 1 / 1 0 / 1 π/2`|`√2 π/4`| Tích lũy sin và cosin độc lập | 
|`2 10 / 1 0 / 1 π`|`0 0`| Hủy hoàn toàn và biên độ bằng 0 | 
|`2 100 / 1 (2π-ε) / 1 ε`|`2 0`| Gói pha trên ranh giới (0) và (2\pi) | 
|`100000`sóng giống hệt nhau |`100000 0`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Để gói pha, sử dụng```
2 1
1 6.283185307079586
1 0.0000000001
```Giai đoạn đầu tiên là (2\pi-10^{-10}), trong khi giai đoạn thứ hai là (10^{-10}). Các vectơ của chúng gần như cùng hướng nên tổng của chúng có biên độ cực gần (2) và pha cực gần (0). Thành phần sin tích lũy xấp xỉ bằng 0 và thành phần cosin xấp xỉ (2).`atan2`có thể tạo ra một giá trị dương hoặc âm rất nhỏ do làm tròn dấu phẩy động. Nếu nó âm, thuật toán sẽ thêm (2\pi), tạo ra pha tương đương trong phạm vi yêu cầu. 

Để hủy chính xác, hãy sử dụng```
2 1
1 0
1 3.141592653589793
```Sóng đầu tiên đóng góp ((1,0)), trong khi sóng thứ hai đóng góp ((-1,0)). Vectơ tích lũy là ((0,0)), vì vậy`math.hypot`trả về số không.`atan2(0,0)`trả về số 0 trong Python, cho kết quả đầu ra hợp lệ```
0.000000000000 0.000000000000
```Pha này không có ý nghĩa vật lý khi biên độ bằng 0, vì vậy bất kỳ pha nào trong khoảng cho phép sẽ biểu thị cùng một sóng. Trả về số 0 là một lựa chọn kinh điển thuận tiện. 

Đối với vấn đề góc phần tư, sử dụng```
2 1
1 0
1 1.5707963267948966
```Vectơ tích lũy là ((1,1)), nên pha là (\pi/4). Một công thức dựa trên`atan(C/S)`tình cờ có tác dụng ở đây, nhưng thay vào đó hãy xem xét```
2 1
1 3.141592653589793
1 1.5707963267948966
```Vectơ tích lũy xấp xỉ ((-1,1)). Pha đúng là (3\pi/4). Một sự ngây thơ`atan(C/S)`phép tính nhìn thấy tỷ lệ xấp xỉ (-1) và trả về xấp xỉ (-\pi/4), đó là hướng sai.`atan2(C,S)`nhìn thấy cả hai dấu hiệu và trả về chính xác (3\pi/4). 

Cuối cùng, trường hợp kích thước tối đa có thể được thực hiện đặc biệt đơn giản:```
100000 50
1 0
1 0
...
1 0
```với (100000) sóng giống hệt nhau. Mọi đóng góp là ((1,0)), vì vậy vectơ cuối cùng là ((100000,0)). Kết quả là biên độ (100000) và pha (0). Thuật toán xử lý mọi đầu vào chính xác một lần, chứng minh lý do tại sao giới hạn (O(n)) vẫn thực tế ở kích thước đầu vào lớn nhất được phép.
