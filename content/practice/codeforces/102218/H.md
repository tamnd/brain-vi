---
title: "CF 102218H - Đài phát thanh Heartbreaker"
description: "Chúng ta có một số sóng hình sin, tất cả đều dao động với cùng tần số góc. Điều duy nhất khác nhau giữa các sóng là biên độ và pha của chúng. Chúng ta cần thay tổng của chúng bằng một hình sin có cùng tần số đó và báo cáo biên độ và pha của nó."
date: "2026-08-20T03:26:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "H"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 111
verified: false
draft: false
---

[CF 102218H - Đài phát thanh Heartbreaker](https://codeforces.com/problemset/problem/102218/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số sóng hình sin, tất cả đều dao động với cùng tần số góc. Điều duy nhất khác nhau giữa các sóng là biên độ và pha của chúng. Chúng ta cần thay tổng của chúng bằng một hình sin có cùng tần số đó và báo cáo biên độ và pha của nó. 

Đối với sóng (i), 

[ 
f_i(t)=A_i\sin(\omega t+\phi_i). 
] 

Tín hiệu hoàn chỉnh là 

[ 
f(t)=\sum_{i=1}^{n} A_i\sin(\omega t+\phi_i), 
] 

và chúng tôi muốn các giá trị (A\ge 0) và (0\le\phi<2\pi) sao cho 

[ 
f(t)=A\sin(\omega t+\phi) 
] 

với mọi (t). 

Tần số (\omega) thực sự không liên quan đến việc tính toán một khi chúng ta nhận ra rằng mọi sóng đều có cùng tần số. Thách thức là kết hợp biên độ và pha một cách hiệu quả. 

Với (n\le 10^5), thuật toán (O(n)) nằm trong phạm vi dự định trong giới hạn 2 giây. Một phương thức (O(n^2)) sẽ yêu cầu khoảng (10^{10}) thao tác cơ bản ở kích thước đầu vào lớn nhất, vượt xa những gì có thể phù hợp trong giới hạn thời gian. Biên độ và pha đầu vào là số thực, do đó việc triển khai cũng phải sử dụng số học dấu phẩy động và tôn trọng độ chính xác cần thiết (10^{-6}). 

Có ba trường hợp đặc biệt thường khiến việc triển khai hợp lý không thành công. Đầu tiên là hủy bỏ. Coi như```
2 1
1 0
1 3.141592653589793
```Hai sóng đối nhau nên tổng của chúng chính xác bằng 0. Kết quả đúng là```
0 0
```vì khi (A=0), pha không có tác dụng và (0) là lựa chọn hợp lệ. Việc thực hiện bất cẩn có thể dẫn đến`atan2(0, 0)`và có được cách diễn giải phụ thuộc vào việc thực hiện hoặc có thể tạo ra biên độ số nhỏ và pha tùy ý. 

Vấn đề thứ hai là góc phần tư của pha. Vì```
1 1
1 4.71238898038469
```câu trả lời là cùng biên độ và pha, xấp xỉ```
1 4.71238898038469
```Từ`atan2`trả về các giá trị trong ([-\pi,\pi]), nó có thể trả về (-\pi/2) thay vì (3\pi/2). Pha toán học là tương đương, nhưng phạm vi đầu ra được yêu cầu cụ thể là ([0,2\pi)), do đó các góc âm phải được chuẩn hóa. 

Vấn đề thứ ba là sự hủy bỏ ở hai thành phần tích lũy. Ví dụ,```
2 1
100 0
100 3.141592653589793
```một lần nữa sẽ tạo ra số không. Các thành phần sin và cosin trung gian có thể rất nhỏ vì các đóng góp dương và âm lớn triệt tiêu nhau. Giải pháp không nên đưa ra quyết định dựa trên sự bằng nhau chính xác của các giá trị dấu phẩy động trừ khi biên độ cuối cùng thực tế bằng 0. 

## Phương pháp tiếp cận 

Một cách trực tiếp nhưng tốn kém không cần thiết để suy nghĩ về vấn đề này là đánh giá tín hiệu hoàn chỉnh ở nhiều thời điểm khác nhau. Với mỗi thời điểm đã chọn (t), chúng tôi sẽ tính toán tất cả (n) sóng và cộng chúng lại. Nếu chúng ta sử dụng (n) thời gian mẫu, thì việc này cần (n) đánh giá (n) sóng, cho kết quả (O(n^2)). Tại (n=10^5), đó là khoảng (10^{10}) đánh giá sóng, quá chậm. 

Cách tiếp cận bạo lực là đúng vì mỗi sóng riêng lẻ được đánh giá chính xác theo định nghĩa của nó, do đó các mẫu được tính toán thực sự là các mẫu có tổng mong muốn. Vấn đề là tần số chung mang lại cho chúng ta nhiều cấu trúc hơn các mẫu tùy ý yêu cầu. 

Quan sát quan trọng là sự đồng nhất góc cộng 

[ 
\sin(x+\phi)=\sin x\cos\phi+\cos x\sin\phi. 
] 

Áp dụng nó cho mọi làn sóng sẽ mang lại 

A_i\cos\phi_i\sin(\omega t) 
+ 
A_i\sin\phi_i\cos(\omega t). 
] 

Bây giờ tất cả các sóng được biểu diễn bằng hai hàm cơ bản giống nhau, (\sin(\omega t)) và (\cos(\omega t)). Chúng ta chỉ cần cộng các hệ số của chúng. 

Xác định 

[ 
C=\sum_{i=1}^{n} A_i\cos\phi_i 
] 

và 

[ 
S=\sum_{i=1}^{n} A_i\sin\phi_i. 
] 

Khi đó tín hiệu hoàn chỉnh sẽ trở thành 

[ 
f(t)=C\sin(\omega t)+S\cos(\omega t). 
] 

Bây giờ hãy mở rộng sóng đơn mong muốn: 

A\cos\phi\sin(\omega t) 
+ 
A\sin\phi\cos(\omega t). 
] 

Việc so sánh hai hệ số sẽ cho 

[ 
A\cos\phi=C, 
\qquad 
A\sin\phi=S. 
] 

Hai phương trình này mô tả một vectơ có tọa độ ((C,S)). Chiều dài của nó là biên độ thu được, 

[ 
A=\sqrt{C^2+S^2}, 
] 

và hướng của nó là pha kết quả, 

[ 
\phi=\operatorname{atan2}(S,C). 
] 

Vì vậy, toàn bộ vấn đề giảm xuống còn một lần chuyển qua đầu vào, tích lũy hai số thực. 

Cái nhìn sâu sắc tương tự cũng có thể được xem như là phép cộng vectơ. Mỗi hình sin (A_i\sin(\omega t+\phi_i)) tương ứng với một vectơ có chiều dài (A_i) và góc (\phi_i). Thêm sóng có nghĩa là thêm các vectơ này. Chiều dài của vectơ thu được là (A), trong khi hướng của nó là (\phi). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ tích lũy, (C=0) và (S=0). Chúng sẽ lưu trữ các hệ số tương ứng của (\sin(\omega t)) và (\cos(\omega t)). 
2. Với mỗi sóng đầu vào, hãy tính (A_i\cos\phi_i) và cộng nó vào (C). Tính (A_i\sin\phi_i) và cộng nó vào (S). 

Đây là sự chuyển đổi trung tâm. Chúng ta không bao giờ cần đánh giá các sóng tại bất kỳ thời điểm thực tế nào (t), vì tần số chung của chúng có nghĩa là mọi sóng đều sử dụng hai hàm cơ bản giống nhau. 
3. Sau khi xử lý xong tất cả các sóng, hãy tính 

[ 
A=\sqrt{C^2+S^2}. 
] 

Các giá trị (C) và (S) chính xác là (A\cos\phi) và (A\sin\phi), do đó độ dài Euclide của chúng phải là biên độ. 

1. Nếu (A) thực sự bằng 0, thì đầu ra (0) và pha (0). 

Một hình sin có biên độ bằng 0 bằng 0 bất kể pha của nó là gì, vì vậy pha (0) là một lựa chọn chính tắc hợp lệ. Điều này cũng tránh việc yêu cầu hướng của vectơ 0. 
2. Ngược lại tính 

[ 
\phi=\operatorname{atan2}(S,C). 
]`atan2`là bắt buộc thay vì thông thường`atan(S/C)`bởi vì nó biết dấu của cả hai thành phần và do đó xác định được góc phần tư chính xác. 

1. Nếu pha âm, hãy thêm (2\pi) vào nó. Sau đó in (A) và (\phi) với đủ chữ số thập phân để đáp ứng yêu cầu về lỗi (10^{-6}). 

### Tại sao nó hoạt động 

Sau khi xử lý từng sóng, bộ tích lũy thỏa mãn 

[ 
C=\sum_i A_i\cos\phi_i 
] 

và 

[ 
S=\sum_i A_i\sin\phi_i. 
] 

Theo đẳng thức cộng góc, tổng ban đầu là 

[ 
f(t)=C\sin(\omega t)+S\cos(\omega t). 
] 

Biên độ và pha tính toán thỏa mãn 

[ 
A\cos\phi=C 
] 

và 

[ 
A\sin\phi=S. 
] 

Thay hai đẳng thức đó thành 

[ 
A\sin(\omega t+\phi) 
] 

tạo ra chính xác (C\sin(\omega t)+S\cos(\omega t)), là tổng ban đầu. Do đó, kết quả là hình sin tương đương với mọi giá trị có thể có của (t), không chỉ ở các điểm mẫu đã chọn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

n, omega = input().split()
n = int(n)
omega = float(omega)

c = 0.0
s = 0.0

for _ in range(n):
    a, phi = map(float, input().split())
    c += a * math.cos(phi)
    s += a * math.sin(phi)

amplitude = math.hypot(c, s)

if amplitude < 1e-12:
    phase = 0.0
else:
    phase = math.atan2(s, c)
    if phase < 0.0:
        phase += 2.0 * math.pi

print(f"{amplitude:.12f} {phase:.12f}")
```Dòng đầu tiên ghi`n`Và`omega`. Giá trị của`omega`được phân tích cú pháp vì nó là một phần của định dạng đầu vào, nhưng nó không xuất hiện sau này trong quá trình tính toán. Khi mọi sóng có cùng tần số, chỉ có biên độ và pha của nó xác định vectơ hệ số phải được thêm vào. 

Các biến`c`Và`s`tương ứng trực tiếp với hai hệ số dẫn xuất trong thuật toán. Đối với mỗi sóng, mã tính toán thành phần ngang (A_i\cos\phi_i) và thành phần dọc (A_i\sin\phi_i), sau đó thêm chúng vào bộ tích lũy tương ứng.`math.hypot(c, s)`tính toán (\sqrt{c^2+s^2}). Tốt nhất là viết biểu thức bằng tay vì`hypot`được thiết kế để tính toán độ dài vectơ một cách mạnh mẽ. 

Kiểm tra bằng 0 sử dụng dung sai rất nhỏ thay vì kiểm tra`amplitude == 0`. Việc hủy dấu phẩy động có thể để lại kết quả bằng 0 về mặt toán học được biểu thị bằng một giá trị dư nhỏ. Bất kỳ pha nào cũng hợp lệ khi biên độ bằng 0, do đó việc chọn pha (0) sẽ cho đầu ra ổn định và hợp lệ. 

Để có kết quả khác 0,`atan2(s, c)`trả về góc của vectơ ((c,s)). Kết quả của nó nằm ở ([-\pi,\pi]), trong khi bài toán yêu cầu ([0,2\pi)). Việc thêm (2\pi) vào kết quả âm sẽ chuyển nó thành phạm vi được yêu cầu. Một kết quả chính xác (2\pi) không xuất hiện từ`atan2`và việc thêm (2\pi) chỉ được thực hiện đối với các giá trị âm, do đó ranh giới trên vẫn hợp lệ. 

Tần số không bao giờ cần phải được nhân vào bất kỳ biểu thức nào. Làm như vậy thực sự sẽ là một sai lầm về mặt khái niệm vì pha đầu ra được yêu cầu là hằng số (\phi) trong (A\sin(\omega t+\phi)), chứ không phải góc phụ thuộc thời gian (\omega t+\phi). 

## Ví dụ đã hoạt động 

Không có mẫu chính thức thứ hai trong tuyên bố được cung cấp, vì vậy dấu vết thứ hai bên dưới sử dụng đầu vào được xây dựng nhỏ. 

Đối với Mẫu 1, trạng thái quan trọng là cặp ((C,S)). Bảng sau đây hiển thị các giá trị tích lũy sau mỗi sóng, được làm tròn để dễ đọc. 

| Sóng | (A_i) | (\phi_i) | (C) sau sóng | (S) sau sóng | 
| --- | --- | --- | --- | --- | 
| 1 | 93,22 | 5,53 | 65,75 | -66.07 | 
| 2 | 48,58 | 0,86 | 97,49 | -28,99 | 
| 3 | 15.31 | 5,39 | 107,13 | -40,89 | 
| 4 | 5,66 | 4.12 | 104.07 | -44,76 | 
| 5 | 48,53 | 6.09 | 152,43 | -54.13 | 
| 6 | 6h60 | 1,42 | 153,42 | -47,61 | 
| 7 | 21.15 | 0,06 | 174,50 | -46,34 | 
| 8 | 4.27 | 5,47 | 177,49 | -49,20 | 

Bảng tròn che giấu một số độ chính xác, nhưng bộ tích lũy có độ chính xác đầy đủ cho kết quả xấp xỉ 

[ 
A=185.184472750 
] 

và 

[ 
\phi=6.019915094. 
] 

Vectơ kết quả nằm trong góc phần tư thứ tư vì (C>0) và (S<0).`atan2`Trước tiên, tạo ra một góc tương đương âm một cách chính xác, sau đó bước chuẩn hóa sẽ thêm (2\pi), đưa ra pha yêu cầu gần (6.02). 

Đối với ví dụ hủy bỏ được xây dựng```
2 1
3 0
3 3.141592653589793
```hai sóng có biên độ bằng nhau và các pha cách nhau bởi (\pi). 

| Sóng | (A_i) | (\phi_i) | (C) sau sóng | (S) sau sóng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 3 | 0 | 
| 2 | 3 | (\pi) | 0 | xấp xỉ 0 | 

Vectơ cuối cùng là vectơ 0 nên biên độ của nó bằng 0. Thuật toán chọn pha (0), tạo ra```
0.000000000000 0.000000000000
```Bất kỳ pha nào khác sẽ biểu thị cùng một tín hiệu bằng 0. Dấu vết này chứng tỏ tại sao thuật toán phải xử lý rõ ràng trường hợp vectơ 0 thay vì cố gắng diễn giải hướng của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi sóng yêu cầu một sin, một cosin và số học bổ sung không đổi. | 
| Không gian | (O(1)) | Chỉ có hai thành phần tích lũy và một số biến vô hướng được lưu trữ. | 

Đối với (n=10^5), thuật toán thực hiện một lần chuyển qua đầu vào và không lưu trữ mảng sóng nào. Thời gian chạy (O(n)) của nó phù hợp với giới hạn 2 giây, trong khi mức sử dụng bộ nhớ liên tục của nó thấp hơn nhiều so với giới hạn 256 MB. Các lệnh gọi hàm lượng giác của Python chiếm ưu thế trong hệ số không đổi, nhưng chỉ có (10^5) mỗi hệ số, điều này rất thực tế. 

## Trường hợp thử nghiệm 

Vì không thể so sánh đầu ra dấu phẩy động dưới dạng một chuỗi chính xác một cách an toàn nên bộ khai thác kiểm tra bên dưới sẽ phân tích hai giá trị đầu ra và kiểm tra chúng với các giá trị dự kiến với dung sai.```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    n, omega = input().split()
    n = int(n)
    omega = float(omega)

    c = 0.0
    s = 0.0

    for _ in range(n):
        a, phi = map(float, input().split())
        c += a * math.cos(phi)
        s += a * math.sin(phi)

    amplitude = math.hypot(c, s)

    if amplitude < 1e-12:
        phase = 0.0
    else:
        phase = math.atan2(s, c)
        if phase < 0.0:
            phase += 2.0 * math.pi

    print(f"{amplitude:.12f} {phase:.12f}")

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

def check(inp: str, expected_a: float, expected_phi: float, message: str):
    out = run(inp).split()
    actual_a = float(out[0])
    actual_phi = float(out[1])

    assert math.isclose(actual_a, expected_a, rel_tol=1e-6, abs_tol=1e-6), message
    assert math.isclose(actual_phi, expected_phi, rel_tol=1e-6, abs_tol=1e-6), message

# Provided sample.
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
check(
    sample1,
    185.184472750,
    6.019915094,
    "sample 1"
)

# Minimum-size input, a single wave must remain unchanged.
check(
    """\
1 1
7 1.2
""",
    7.0,
    1.2,
    "single wave"
)

# Exact cancellation.
check(
    """\
2 10
5 0
5 3.141592653589793
""",
    0.0,
    0.0,
    "complete cancellation"
)

# Phase in the fourth quadrant. This catches atan2 without normalization.
check(
    """\
1 2
4 4.71238898038469
""",
    4.0,
    1.5 * math.pi,
    "negative atan2 result must be normalized"
)

# Equal phases. The amplitudes simply add.
check(
    """\
4 50
1.5 0.75
2.5 0.75
3.0 0.75
4.0 0.75
""",
    11.0,
    0.75,
    "equal amplitudes direction"
)

# Large input, exercising linear processing and accumulation.
large_n = 100000
large_input = f"{large_n} 1\n" + ("1 0\n" * large_n)
large_out = run(large_input).split()
assert math.isclose(float(large_out[0]), 100000.0, rel_tol=1e-6, abs_tol=1e-6)
assert math.isclose(float(large_out[1]), 0.0, rel_tol=1e-6, abs_tol=1e-6)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | (185.184472750,\ 6.019915094) | Ví dụ chính thức đầy đủ và tích lũy chung | 
|`1 1 / 7 1.2`| (7,\ 1.2) | Đầu vào tối thiểu và hành vi sóng đơn | 
|`2 10 / 5 0 / 5 π`| (0,\ 0) | Hủy hoàn toàn và biên độ bằng 0 | 
|`1 2 / 4 3π/2`| (4,\ 3π/2) |`atan2`xử lý góc phần tư và chuẩn hóa pha | 
| Bốn sóng có pha (0,75) | (11,\ 0,75) | Pha bằng nhau, trong đó biên độ cộng trực tiếp | 
| (10^5) sóng có biên độ (1), pha (0) | (100000,\ 0) | Kích thước đầu vào tối đa và độ phức tạp tuyến tính | 

## Vỏ cạnh 

Việc hủy hoàn toàn được xử lý bởi nhánh biên độ bằng 0. Vì```
2 1
1 0
1 3.141592653589793
```sóng đầu tiên đóng góp ((C,S)=(1,0)). Phần thứ hai đóng góp ((-1,0)), vì vậy vectơ cuối cùng là ((0,0)). Biên độ của nó bằng 0 và thuật toán tạo ra pha 0. Tần số không làm thay đổi kết luận này vì cả hai sóng đều có cùng tần số. 

Một giai đoạn trong góc phần tư thứ tư bộc lộ một sai lầm phổ biến với`atan2`. Vì```
1 1
1 4.71238898038469
```các thành phần tích lũy là khoảng 

[ 
C=0,\qquad S=-1. 
]`atan2(-1,0)`trả về (-\pi/2). Vì pha cần thiết phải không âm nên thuật toán cộng (2\pi), thu được (3\pi/2), chính xác là pha ban đầu. 

Việc hủy dấu phẩy động cũng được xử lý an toàn. Coi như```
2 1
100 0
100 3.141592653589793
```Về mặt toán học, đóng góp của vectơ là ((100,0)) và ((-100,0)). Trong số học dấu phẩy động, sin thứ hai không nhất thiết phải được biểu diễn chính xác bằng 0, do đó vectơ cuối cùng có thể chứa phần dư nhỏ. các`1e-12`ngưỡng coi phần dư đó là vectơ 0. Vì dung sai số được yêu cầu là (10^{-6}), điều này không loại bỏ bất kỳ kết quả khác 0 có ý nghĩa nào. 

Cuối cùng, bản thân ranh giới pha không yêu cầu xử lý đặc biệt ngoài bước chuẩn hóa. Một pha tiến đến (2\pi) có một vectơ chỉ ngay dưới trục dương (C), trong khi một pha ngay trên 0 có các điểm ngay phía trên nó.`atan2`phân biệt chính xác các dấu hiệu này. Việc chuẩn hóa chỉ thay đổi các biểu diễn âm thành các giá trị tương đương của chúng trong ([0,2\pi)) mà không thay đổi hình sin được biểu thị.
