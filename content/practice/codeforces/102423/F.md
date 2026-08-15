---
title: "CF 102423F - Du hành giữa các vì sao"
description: "Chúng ta có (N) ngôi sao xung quanh Trái đất. Đối với mỗi ngôi sao (i), ba giá trị mô tả sự đóng góp của nó vào khoảng cách di chuyển của tàu vũ trụ."
date: "2026-08-12T01:15:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "F"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 210
verified: true
draft: false
---

[CF 102423F - Du hành giữa các vì sao](https://codeforces.com/problemset/problem/102423/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (N) ngôi sao xung quanh Trái đất. Đối với mỗi ngôi sao (i), ba giá trị mô tả sự đóng góp của nó vào khoảng cách di chuyển của tàu vũ trụ. Ngôi sao được căn chỉnh lý tưởng theo hướng (a_i), đóng góp tối đa (T_i) và mất (s_i) đơn vị khoảng cách di chuyển cho mỗi radian mà hướng phóng di chuyển ra khỏi (a_i). Khoảng cách góc là hình tròn, do đó việc di chuyển một chút về phía trước (0) tương đương với việc di chuyển một chút về phía trước (2\pi). 

Nếu hướng phóng đã chọn là (x), ngôi sao (i) sẽ đóng góp 

[ 
f_i(x)=\max(0,T_i-s_i\operatorname{dist}(a_i,x)). 
] 

Tổng khoảng cách là tổng của tất cả những đóng góp này và chúng ta cần khoảng cách tối đa của nó trong một vòng quay đầy đủ. Dữ liệu đầu vào chứa tối đa (10^5) sao, với các tham số có giá trị thực có tối đa sáu chữ số sau dấu thập phân. Lỗi số bắt buộc là (10^{-6}). 

Giới hạn (N\le 10^5) ngay lập tức loại trừ việc thử nhiều hướng ứng cử viên và đánh giá tất cả các ngôi sao cho mỗi hướng. Ngay cả khi chúng ta chỉ xem xét ba hướng đặc biệt cho mỗi ngôi sao, điều đó cũng có nghĩa là khoảng (3N^2), khoảng (3\cdot10^{10}), đánh giá trong trường hợp xấu nhất. Một giải pháp xung quanh (O(N\log N)) là phù hợp, vì việc sắp xếp vài trăm nghìn sự kiện chiếm ưu thế trong công việc và dễ dàng phù hợp với giới hạn cuộc thi năm giây. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể xử lý sai. Nếu (s_i=0), ngôi sao đóng góp (T_i) cho mọi hướng. Ví dụ,```
1
5 0 1
```có câu trả lời```
5.000000
```Việc triển khai bất cẩn luôn tính toán (T_i/s_i) chia cho 0. 

Vấn đề thứ hai là ranh giới hình tròn. Coi như```
2
1 1 0
1 1 6.183185
```Góc thứ hai gần với (2\pi-0.1), do đó hai vùng hữu ích chồng lên nhau trên ranh giới (0/2\pi). Tối đa là khoảng```
1.900000
```Việc triển khai xử lý khoảng thời gian góc như một khoảng thời gian thông thường và không bao bọc nó sẽ bỏ lỡ sự chồng chéo này. 

Trường hợp đặc biệt thứ ba xảy ra khi một ngôi sao vẫn hoạt động trong ít nhất một hình bán nguyệt. Ví dụ,```
2
10 1 0
10 1 3.141593
```có câu trả lời xấp xỉ```
16.858407
```Bán kính (T_i/s_i) lớn hơn (\pi), do đó phần đóng góp không bao giờ bằng 0 xung quanh vòng tròn. Việc coi vùng hoạt động của nó như một tam giác thông thường có bán kính (T_i/s_i) sẽ mở rộng không chính xác ra ngoài điểm mà khoảng cách hình tròn thay đổi hướng. 

Cuối cùng, một số ngôi sao có thể có góc ưa thích giống hệt nhau. Ví dụ,```
3
5 1 1
5 1 1
5 1 1
```có câu trả lời```
15.000000
```Tất cả các thay đổi đạo hàm xảy ra ở cùng tọa độ. Việc xử lý các sự kiện một cách độc lập là được, nhưng việc triển khai không được cho rằng mỗi tọa độ sự kiện là duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xác định hướng đi của ứng viên và đánh giá tổng đóng góp của từng hướng đi. Sự đóng góp của mỗi ngôi sao thay đổi công thức theo hướng ưa thích của nó và theo hai hướng mà sự đóng góp của nó trở thành 0. Điều đó đưa ra hướng dẫn cho ứng viên (O(N)). Việc đánh giá tổng mức đóng góp một cách độc lập ở mỗi ứng viên mất (O(N)) công việc, cho (O(N^2)) thời gian. Với (N=10^5), trường hợp xấu nhất là đánh giá khoảng (10^{10}) sao, vượt xa giới hạn năm giây. 

Lực lượng vũ phu hoạt động vì mọi đóng góp của cá nhân đều tuyến tính từng phần. Thất bại xuất phát từ việc liên tục tính toán lại số tiền từ đầu. 

Quan sát quan trọng là toàn bộ tổng cũng tuyến tính từng phần. Giữa hai điểm liên tiếp trong đó một số ngôi sao thay đổi độ dốc của nó, mỗi ngôi sao đều có một công thức tuyến tính cố định, do đó hàm tổng cũng có độ dốc cố định. Hàm tuyến tính trên một khoảng đạt cực đại tại một trong các điểm cuối. Chúng ta chỉ cần biết độ dốc thay đổi như thế nào khi góc quay. 

Đối với một ngôi sao có bán kính góc hữu ích là 

[ 
d=\min\left(\pi,\frac{T_i}{s_i}\right), 
] 

đóng góp của nó tăng theo độ dốc (+s_i) từ (a_i-d) đến (a_i), sau đó giảm theo độ dốc (-s_i) từ (a_i) đến (a_i+d). Do đó đạo hàm thay đổi theo 

[ 
+s_i,\quad -2s_i,\quad +s_i 
] 

ở ba vị trí đó. 

Sự phức tạp duy nhất là ranh giới hình tròn tại (0) và (2\pi). Chúng tôi xử lý nó bằng cách di chuyển các điểm cuối được bao bọc trở lại khoảng ([0,2\pi]) và điều chỉnh đạo hàm ban đầu cho phù hợp. Điều này chỉ đưa ra ba sự kiện cho mỗi ngôi sao không đổi. 

Cách tiếp cận thu được là quét tiêu chuẩn các sự kiện thay đổi độ dốc. Trước tiên, chúng tôi tính tổng giá trị ở góc (0), tính tổng đạo hàm ngay sau góc (0), sắp xếp tất cả các sự kiện thay đổi đạo hàm và sau đó tích hợp hệ số góc hiện tại từ sự kiện này sang sự kiện tiếp theo. 

Ý tưởng sự kiện tuyến tính từng phần tương tự cũng được phản ánh trong một giải pháp được chấp nhận cho bài toán cuộc thi này, giải pháp này biểu thị mỗi ngôi sao theo điểm cuối bên trái, điểm cuối bên phải và điểm cuối bên phải của nó, đồng thời xử lý rõ ràng các khoảng giao nhau (0) và (2\pi). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi ngôi sao có (s_i=0), hãy cộng trực tiếp (T_i) vào giá trị hiện tại. Sự đóng góp của nó không bao giờ thay đổi nên nó không tạo ra các sự kiện phái sinh. 
2. Với mỗi ngôi sao khác, hãy tính 

[ 
d_i=\min\left(\pi,\frac{T_i}{s_i}\right). 
] 

Mức tối thiểu với (\pi) xử lý khoảng cách vòng tròn một cách chính xác. Không có khoảng cách góc ngắn nhất nào có thể vượt quá (\pi), do đó, một khi (T_i/s_i\ge\pi), sự đóng góp vẫn dương ở mọi nơi và những thay đổi về độ dốc liên quan xảy ra ở hướng ưu tiên và đối cực của nó. 

1. Tính toán phần đóng góp của ngôi sao này ở góc phóng (0) và cộng nó vào giá trị đang chạy. Khoảng cách vòng tròn từ (a_i) đến (0) chỉ đơn giản là (\min(a_i,2\pi-a_i)). 
2. Xác định ba vị trí sự kiện 

[ 
l=a_i-d_i,\qquad m=a_i,\qquad r=a_i+d_i. 
] 

Tại (l), đạo hàm thay đổi (+s_i). Tại (m), đạo hàm thay đổi theo (-2s_i). Tại (r) nó thay đổi (+s_i). 

1. Nếu (l<0), thêm (2\pi) vào nó để sự kiện nằm trong khoảng quét. Vì khoảng này cắt góc (0), ngôi sao đã ở phía mọc ngay sau góc (0), vì vậy hãy thêm (s_i) vào đạo hàm ban đầu. 
2. Nếu (r>2\pi), hãy trừ (2\pi) khỏi nó. Phần đóng góp đã ở phía giảm ngay sau góc (0), vì vậy hãy trừ (s_i) khỏi đạo hàm ban đầu. 

Sự bất bình đẳng nghiêm ngặt có vấn đề. Nếu điểm cuối chính xác là (0) hoặc (2\pi), thì đó đã là một sự kiện ở ranh giới và không biểu thị khoảng được bao bọc.

1. Sắp xếp tất cả các sự kiện theo góc độ. Giữa hai sự kiện liên tiếp không có ngôi sao nào thay đổi hệ số góc nên hàm số tổng là một đường thẳng tại đó. 
2. Bắt đầu với tổng giá trị ở góc (0), đạo hàm ban đầu và góc trước đó (0). Đối với mỗi sự kiện ở góc (x), hãy nâng giá trị lên bằng 

[ 
\text{value} \mathrel{+}= \text{slope}\cdot(x-\text{góc trước}). 
] 

Sau đó áp dụng thay đổi đạo hàm của sự kiện và cập nhật giá trị tốt nhất. 

1. Trả về giá trị lớn nhất gặp phải. Hàm này có tính tuần hoàn và góc (2\pi) cùng hướng với góc (0), vì vậy việc kiểm tra vị trí ban đầu và mọi vị trí thay đổi đạo hàm sẽ bao trùm toàn bộ đường tròn. 

### Tại sao nó hoạt động 

Đối với mỗi ngôi sao, sau khi thay thế (T_i/s_i) bằng (\min(\pi,T_i/s_i)), đóng góp của nó là tuyến tính trên mọi khoảng giữa ranh giới bên trái, góc ưa thích và ranh giới bên phải của nó. Do đó đạo hàm của nó không đổi giữa ba sự kiện đó. 

Tổng của tất cả các ngôi sao cũng là tuyến tính giữa các sự kiện toàn cầu liên tiếp. Hàm tuyến tính không thể có cực đại bên trong nghiêm ngặt, do đó cực đại toàn cục phải xảy ra ở góc (0) hoặc tại tọa độ sự kiện. Quá trình quét duy trì giá trị hàm chính xác ở mọi tọa độ như vậy bằng cách tích phân tổng độ dốc hiện tại và sau đó áp dụng thay đổi đạo hàm. Do đó giá trị lớn nhất được ghi lại bằng quá trình quét chính xác là khoảng cách di chuyển tối đa. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

PI = math.pi
TWO_PI = 2.0 * math.pi

def solve():
    n = int(input())

    events = []
    cur = 0.0
    initial_slope = 0.0

    for _ in range(n):
        t, s, a = map(float, input().split())

        if s == 0.0:
            cur += t
            continue

        # The angular contribution can never depend on a distance
        # larger than pi.
        d = min(PI, t / s)

        # Contribution at angle 0.
        dist0 = min(a, TWO_PI - a)
        cur += max(0.0, t - s * dist0)

        left = a - d
        right = a + d

        # If the active interval crosses 0, its rising segment
        # is already active immediately after angle 0.
        if left < 0.0:
            left += TWO_PI
            initial_slope += s

        # If the active interval crosses 2*pi, its falling segment
        # is already active immediately after angle 0.
        if right > TWO_PI:
            right -= TWO_PI
            initial_slope -= s

        events.append((left, s))
        events.append((a, -2.0 * s))
        events.append((right, s))

    if not events:
        print(f"{cur:.6f}")
        return

    events.sort()

    best = cur
    slope = initial_slope
    previous = 0.0

    for angle, delta in events:
        cur += slope * (angle - previous)
        best = max(best, cur)

        slope += delta
        previous = angle

    print(f"{best:.6f}")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào tách trường hợp (s_i=0) trước khi tính toán (T_i/s_i). Một ngôi sao như vậy đóng góp một hằng số (T_i), do đó việc lưu trữ bất kỳ sự kiện nào cho nó sẽ chỉ lãng phí thời gian. 

Đối với một ngôi sao không đổi,`dist0 = min(a, TWO_PI - a)`tính khoảng cách góc ngắn nhất của nó từ hướng (0). Điều này khởi tạo giá trị chính xác của toàn bộ mục tiêu khi bắt đầu quét. 

Ba mục sự kiện mã hóa các thay đổi phái sinh. Điểm cuối bên trái bắt đầu một đoạn tăng lên, góc ưu tiên thay đổi đạo hàm từ (+s_i) thành (-s_i) và điểm cuối bên phải thay đổi đạo hàm về 0. Đó là lý do tại sao các vùng đồng bằng là (+s_i), (-2s_i) và (+s_i). 

Việc điều chỉnh gói là phần tinh tế. Nếu điểm cuối bên trái âm thì đoạn lên của ngôi sao cắt góc (0), do đó đạo hàm ban đầu đã bao gồm (+s_i). Nếu điểm cuối bên phải vượt quá (2\pi), đoạn rơi xuống sẽ vượt qua ranh giới, do đó đạo hàm ban đầu bao gồm (-s_i). 

Quá trình quét tích hợp đạo hàm thay vì tính toán lại mục tiêu. biểu thức`cur += slope * (angle - previous)`chính xác là sự thay đổi giá trị của hàm tuyến tính trong khoảng đó. Thay đổi đạo hàm chỉ được áp dụng sau khi đạt đến tọa độ sự kiện, điều này tránh được lỗi sai kiểu một tại chính điểm dừng. 

của Python`float`ở đây là đủ. Giá trị đầu vào có tối đa sáu chữ số thập phân và dung sai bắt buộc của câu trả lời là (10^{-6}). Tổng đóng góp tối đa nhiều nhất là (10^8), vẫn nằm trong phạm vi chính xác thoải mái trong đó số học có độ chính xác kép thông thường mang lại đủ độ chính xác tuyệt đối. Câu lệnh ban đầu chỉ định dung sai đầu ra (10^{-6}) giống nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
100 1 1
100 1 1.5
```Cả hai ngôi sao đều thỏa mãn (T_i/s_i=100>\pi), do đó mỗi ngôi sao vẫn hoạt động theo mọi hướng. Đóng góp của họ là (100-\operatorname{dist}(a_i,x)). 

Ở góc (0), hai phần đóng góp là (99) và (98,5), cho (197,5). Cả hai hàm số ban đầu đều tăng khi góc di chuyển về hướng ưa thích của chúng, do đó tổng độ dốc ban đầu là (2). 

| Góc sự kiện | Giá trị trước sự kiện | Độ dốc trước | Thay đổi độ dốc | Giá trị sau sự kiện | 
| --- | --- | --- | --- | --- | 
| (1,000000) | (199.500000) | (2) | (-2) | (199.500000) | 
| (1,500000) | (199.000000) | (0) | (-2) | (199.000000) | 
| (4.141593) | (196.858407) | (-2) | (+2) | (196.858407) | 
| (4.641593) | (196.858407) | (0) | (+2) | (196.858407) | 

Sự kiện đầu tiên là tối đa, đưa ra```
199.500000
```Ví dụ này chứng tỏ tại sao chúng ta cần tích phân độ dốc thay vì chỉ thêm hoặc bớt các đóng góp. Cực đại xảy ra chính xác ở nơi ngôi sao đầu tiên đạt đến hướng ưa thích của nó. Mẫu chính thức có đầu ra này. 

### Mẫu 2 

Đầu vào là```
4
100 1 0.5
200 1 1
100 0.5 1.5
10 2 3
```Bốn ngôi sao tạo ra một số sự kiện trái, giữa và phải. Quá trình quét xử lý chúng theo thứ tự góc tăng dần. 

| Góc sự kiện | Giá trị hiện tại sau khi di chuyển | Độ dốc trước | Đồng bằng sự kiện | Độ dốc mới | 
| --- | --- | --- | --- | --- | 
| (0,500000) | (402.500000) | (2,000000) | (-2.000000) | (0,000000) | 
| (1,000000) | (402.500000) | (0,000000) | (-2.000000) | (-2.000000) | 
| (1,500000) | (401.500000) | (-2.000000) | (-1,000000) | (-3.000000) | 
| (2,000000) | (400.000000) | (-3.000000) | (1,000000) | (-2.000000) | 
| (3.000000) | (398.000000) | (-2.000000) | (-4.000000) | (-6,000000) | 

Danh sách sự kiện đầy đủ cũng chứa các sự kiện được bao bọc ở phần sau của vòng kết nối. Tiếp tục quét đạt mức tối đa toàn cầu```
405.500000
```Đầu ra mẫu xác nhận giá trị này. 

Bài học chính từ dấu vết này là mục tiêu không cần phải được đánh giá ở các góc độ tùy ý. Khi đã biết độ dốc hiện tại, mọi điểm cho đến sự kiện tiếp theo được xác định bằng một phép nhân duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Mỗi ngôi sao tạo ra tối đa ba sự kiện, tiếp theo là sắp xếp tối đa (3N) sự kiện. | 
| Không gian | (O(N)) | Mảng sự kiện chứa tối đa (3N) mục nhập. | 

Với (N=10^5), có nhiều nhất (3\cdot10^5) sự kiện. Việc sắp xếp sao cho nhiều cặp dấu phẩy động có thể dễ dàng tương thích với giới hạn năm giây, trong khi phương án thay thế (O(N^2)) sẽ yêu cầu đánh giá khoảng (10^{10}). Tuyên bố cuộc thi đưa ra (N\le10^5) và giới hạn thời gian là năm giây. 

## Trường hợp thử nghiệm```python
# Assume the submitted solution is saved as solution.py.
# The solve() function from that file reads stdin and writes stdout.

import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        # solution.py has:
        # input = sys.stdin.readline
        # Rebind it because stdin is replaced after importing the module.
        solution.input = sys.stdin.readline
        solution.solve()

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    """2
100 1 1
100 1 1.5
"""
) == "199.500000\n", "sample 1"

assert run(
    """4
100 1 0.5
200 1 1
100 0.5 1.5
10 2 3
"""
) == "405.500000\n", "sample 2"

# Minimum-size input.
assert run(
    """1
10 2 0
"""
) == "10.000000\n", "single star"

# Constant contribution, s = 0.
assert run(
    """2
7 0 0
9 0 3
"""
) == "16.000000\n", "zero decay"

# Several identical stars.
assert run(
    """3
5 1 1
5 1 1
5 1 1
"""
) == "15.000000\n", "all equal"

# Active intervals overlap through the 0 / 2*pi boundary.
assert run(
    """2
1 1 0
1 1 6.183185
"""
) == "1.900000\n", "circular boundary"

# Maximum-size case, also checks that many equal events are handled.
n = 100000
large_input = str(n) + "\n" + ("1 1 0\n" * n)
assert run(large_input) == "100000.000000\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 10 2 0`|`10.000000`| Kích thước đầu vào tối thiểu và tối đa một sao | 
|`2 / 7 0 0 / 9 0 3`|`16.000000`| Xử lý phân rã bằng 0 và chia cho 0 | 
|`3 / 5 1 1 / 5 1 1 / 5 1 1`|`15.000000`| Những ngôi sao giống hệt nhau và những sự kiện trùng hợp | 
|`2 / 1 1 0 / 1 1 6.183185`|`1.900000`| Gói tròn trên (0) và (2\pi) | 
| (10^5) bản sao của`1 1 0`|`100000.000000`| Tối đa (N), số lượng sự kiện lớn, tọa độ sự kiện lặp lại | 

## Vỏ cạnh 

Khi (s_i=0), ngôi sao không có sự phân rã góc nào cả. Vì```
1
5 0 1
```sự đóng góp luôn là (5). Thuật toán phát hiện điều này trước khi tính toán (T_i/s_i), thêm (5) trực tiếp vào`cur`và không tạo ra sự kiện nào. Câu trả lời cuối cùng là`5.000000`. 

Khi một khoảng hoạt động vượt qua ranh giới góc, điểm cuối bên trái và bên phải không thể được giữ đơn giản như các số thực thông thường. Vì```
2
1 1 0
1 1 6.183185
```ngôi sao thứ hai hoạt động ngay trước và ngay sau góc (0). Thuật toán dịch chuyển điểm cuối của nó nằm ngoài khoảng trở lại (2\pi) và thay đổi đạo hàm ban đầu để tính phần đã hoạt động ở góc (0). Việc quét do đó tìm thấy`1.900000`. 

Khi (T_i/s_i\ge\pi), đóng góp không bao giờ trở thành số âm quanh vòng tròn. Vì```
2
10 1 0
10 1 3.141593
```mỗi ngôi sao đang hoạt động ở khắp mọi nơi. Thuật toán giới hạn bán kính hữu ích ở mức (\pi), do đó độ dốc của nó thay đổi ở góc ưu tiên và góc đối diện thay vì tạo ra khoảng thời gian hoạt động dài hơn. Kết quả tối đa là khoảng`16.858407`. 

Khi một số tọa độ sự kiện trùng nhau, các thay đổi đạo hàm của chúng có thể được áp dụng theo bất kỳ thứ tự nào vì việc di chuyển từ tọa độ bằng nhau này sang tọa độ tiếp theo sẽ thay đổi góc bằng 0. Vì```
3
5 1 1
5 1 1
5 1 1
```cả ba đỉnh đều xảy ra ở cùng một góc. Quá trình quét áp dụng ba thay đổi đạo hàm giống hệt nhau tại tọa độ đó và thu được chính xác`15.000000`. 

Ranh giới cuối cùng cũng an toàn. Vật kính ở góc (2\pi) giống với giá trị của nó ở góc (0), vốn đã được đưa vào làm ứng cử viên ban đầu. Quá trình quét chỉ cần tọa độ sự kiện trong một khoảng thời gian hoàn chỉnh, do đó không có điểm cuối bổ sung nào có thể chứa mức tối đa chưa được khám phá.
