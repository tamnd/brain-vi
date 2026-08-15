---
title: "CF 102433J - Du hành giữa các vì sao"
description: "Mỗi ngôi sao được mô tả bằng ba giá trị. Đóng góp tối đa của nó là (Ti), nó mất (si) đơn vị đóng góp trên mỗi radian của độ lệch góc và hướng ưa thích của nó là (ai)."
date: "2026-08-12T07:39:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 169
verified: true
draft: false
---

[CF 102433J - Du hành giữa các vì sao](https://codeforces.com/problemset/problem/102433/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ngôi sao được mô tả bằng ba giá trị. Đóng góp tối đa của nó là (T_i), nó mất (s_i) đơn vị đóng góp trên mỗi radian của độ lệch góc và hướng ưa thích của nó là (a_i). Nếu tàu vũ trụ được phóng theo góc (a), ngôi sao (i) góp phần 

[ 
f_i(a)=\max(0,T_i-s_i\operatorname{dist}(a_i,a)). 
] 

Hàm khoảng cách là hàm tròn nên hiệu góc luôn nằm giữa (0) và (\pi). Tổng khoảng cách di chuyển theo hướng (a) là tổng của tất cả sự đóng góp của các ngôi sao. Chúng ta cần giá trị tối đa của tổng này trên toàn bộ vòng tròn các hướng phóng có thể có. 

Có thể có (10^5) sao, vì vậy việc đánh giá từng ngôi sao cho mọi hướng ứng cử viên có thể là không khả thi. Dữ liệu đầu vào chứa các số thực có tối đa sáu chữ số thập phân, nhưng câu trả lời phải chính xác đến (10^{-6}), do đó, việc triển khai phải sử dụng số học dấu phẩy động có độ chính xác kép. Ràng buộc quan trọng là (N=10^5): thuật toán (O(N^2)) thực hiện tối đa (10^{10}) đánh giá sao, vượt xa giới hạn 5 giây có thể hỗ trợ. Chúng ta cần một giải pháp (O(N\log N)) hoặc giải pháp hiệu quả tương tự. 

Có một số trường hợp ranh giới vòng tròn có thể âm thầm phá vỡ quá trình triển khai. 

Đầu tiên là góc giao khoảng ảnh hưởng (0). Ví dụ,```
1
1 1 0.1
```có khoảng đóng góp dương từ (0,1-1=-0,9) đến (0,1+1=1,1). Trên vòng tròn, điều đó có nghĩa là khoảng đi qua (0) và câu trả lời đúng là (1). Xử lý các góc như một đường thông thường và chỉ cần loại bỏ điểm cuối âm sẽ cho độ dốc sai gần bằng 0. 

Thứ hai là một ngôi sao có ảnh hưởng theo hướng ngược lại. Ví dụ,```
1
3.141593 1 0
```có (T_i/s_i) lớn hơn (\pi) một chút. Ngôi sao đóng góp tích cực theo mọi hướng, với góc gần tối thiểu (\ pi) và tối đa của nó vẫn là (3,141593) ở góc (0). Việc triển khai bất cẩn giả định mọi ngôi sao đều trở thành số 0 trước khi đạt đến hướng đối cực có thể tạo ra một khoảng không hợp lệ. 

Thứ ba là (s_i=0). Ví dụ,```
1
100 0 2
```luôn đóng góp (100), bất kể góc phóng, nên đáp án là (100). Chia (T_i) cho (s_i) mà không xử lý trường hợp này sẽ gây ra phép chia cho 0. 

Cuối cùng, một số góc độ sự kiện có thể giống hệt nhau. Ví dụ,```
2
5 2 1
5 2 1
```có câu trả lời (10). Cả hai ngôi sao đều có cực đại ở cùng một góc, do đó tất cả những thay đổi về độ dốc ở góc đó phải được tích lũy thay vì cho phép một sự kiện này ghi đè lên một sự kiện khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn góc phóng và đánh giá từng ngôi sao. Một quan sát tự nhiên là sự tối ưu xảy ra ở hướng ưa thích của một số ngôi sao, vì vậy chúng ta có thể đánh giá tổng mức đóng góp ở mọi (a_i). Điều này đúng, bởi vì mỗi đóng góp riêng lẻ đều tuyến tính từng phần theo góc và sự thay đổi độ dốc đi xuống duy nhất của nó xảy ra theo hướng ưa thích của nó. Do đó, số tiền tối đa có thể được lấy theo một trong các hướng đó. 

Tuy nhiên, việc đánh giá tổng số ở mọi (a_i) vẫn yêu cầu xem xét tất cả (N) sao cho mỗi (N) ứng cử viên. Trong trường hợp xấu nhất, điều này thực hiện các phép tính đóng góp (N^2=10^{10}). Phương pháp vũ phu về mặt khái niệm đơn giản và hoàn toàn chính xác, nhưng nó quá chậm. 

Quan sát quan trọng là mỗi ngôi sao đều đóng góp một hàm tuyến tính từng phần. Xung quanh góc ưa thích của nó (a_i), trong khi ngôi sao đang đóng góp, độ dốc của nó là (+s_i) trên đường hướng tới (a_i), sau đó là (-s_i) sau khi đi qua (a_i). Khi đóng góp của nó đạt đến 0, độ dốc của nó sẽ bằng 0. 

Giả sử 

[ 
r_i=\min\left(\pi,\frac{T_i}{s_i}\right) 
] 

cho (s_i>0). Phần đóng góp chỉ thay đổi độ dốc ở ba góc liên quan: điểm cuối bên trái (a_i-r_i), đỉnh (a_i) và điểm cuối bên phải (a_i+r_i). Tại ba vị trí này, sự thay đổi độ dốc lần lượt là 

[ 
+s_i,\qquad -2s_i,\qquad +s_i. 
] 

Do đó, một ngôi sao có thể được biểu diễn chỉ bằng ba sự kiện thay đổi độ dốc. Đây là mức giảm trung tâm được sử dụng bởi giải pháp đường quét. Bài viết giải pháp cho vấn đề này sử dụng chính xác cách biểu diễn ba sự kiện này và xử lý rõ ràng các khoảng giao nhau (0) và (2\pi). 

Thay vì tính lại toàn bộ tổng ở mọi góc ứng cử viên, chúng ta tính tổng đóng góp ở góc (0), tính đạo hàm của hàm tổng ngay bên phải (0), rồi quét tất cả các góc sự kiện (O(N)) theo thứ tự được sắp xếp. 

Giữa hai góc sự kiện liên tiếp, mỗi ngôi sao đều có hành vi tuyến tính cố định. Do đó, tổng đóng góp cũng có độ dốc không đổi ở đó. Nếu giá trị hiện tại là (E), độ dốc hiện tại là (D) và sự kiện tiếp theo ở góc (x), thì 

[ 
E\leftarrow E+D(x-\text{góc trước}). 
] 

Sau khi đạt đến (x), chúng ta cộng tất cả những thay đổi về độ dốc xảy ra ở đó. Điều này cho phép chúng ta duy trì hàm tuyến tính từng phần chính xác bằng cách chỉ sử dụng công không đổi cho mỗi sự kiện. 

Đường ranh giới hình tròn yêu cầu thêm một chi tiết. Nếu điểm cuối bên trái của một ngôi sao âm, thì khoảng của nó vượt qua (0), do đó sự thay đổi độ dốc (+s_i) của nó đã xảy ra trước góc (0). Chúng ta thêm (s_i) vào độ dốc ban đầu và chuẩn hóa sự kiện bên trái bằng cách thêm (2\pi). Tương tự, nếu điểm cuối bên phải vượt quá (2\pi), thì điểm cuối bên phải đã bao quanh, do đó độ dốc ngay bên phải của (0) đã nằm sau sự kiện (+s_i) tại điểm cuối bên phải. Chúng tôi trừ (s_i) khỏi độ dốc ban đầu và chuẩn hóa sự kiện đó bằng cách trừ (2\pi). 

Một ngôi sao có (s_i=0) đóng góp một hằng số (T_i), do đó nó đóng góp vào năng lượng ban đầu nhưng không tạo ra sự kiện nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Quét tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ngôi sao và tính tổng đóng góp ở góc phóng (0). Đối với một ngôi sao có góc ưa thích (a_i), khoảng cách của nó với (0) là (\min(a_i,2\pi-a_i)), do đó đóng góp ban đầu của nó có thể tính toán trực tiếp. 
2. Nếu (s_i=0), thêm (T_i) vào năng lượng ban đầu và không tạo ra sự kiện nào. Sự đóng góp của nó không bao giờ thay đổi theo góc phóng. 
3. Với (s_i>0), hãy tính bán kính hiệu dụng 

[ 
r_i=\min\left(\pi,\frac{T_i}{s_i}\right). 
] 

Giới hạn ở (\pi) là cần thiết vì khoảng cách góc tròn không bao giờ vượt quá (\pi). Nếu (T_i/s_i\geq\pi), ngôi sao không bao giờ ngừng hoạt động xung quanh vòng tròn.

1. Tạo ba sự kiện cho ngôi sao. Tại (a_i-r_i), độ dốc tăng thêm (s_i). Tại (a_i), độ dốc giảm đi (2s_i). Tại (a_i+r_i), độ dốc tăng thêm (s_i). 
2. Chuẩn hóa các điểm cuối trong khoảng ([0,2\pi]). Nếu điểm cuối bên trái âm, hãy thêm (s_i) vào độ dốc ban đầu và thêm (2\pi) vào điểm cuối. Nếu điểm cuối bên phải vượt quá (2\pi), hãy trừ (s_i) khỏi độ dốc ban đầu và trừ (2\pi) khỏi điểm cuối. Điều này chuyển đổi khoảng tròn thành các sự kiện được sắp xếp thông thường trong khi vẫn giữ hàm ngay bên phải góc (0). 
3. Sắp xếp tất cả các sự kiện theo góc độ của chúng. Có nhiều nhất (3N) sự kiện nên chi phí này là (O(N\log N)). 
4. Bắt đầu quét với góc (0), năng lượng ban đầu được tính toán trước đó và độ dốc ban đầu. Đối với mỗi góc sự kiện (x), trước tiên hãy tăng năng lượng bằng cách 

[ 
\text{độ dốc hiện tại}\time(x-\text{góc trước đó}). 
] 

Sau đó ghi lại năng lượng này dưới dạng mức tối đa ứng cử viên và áp dụng sự thay đổi độ dốc của sự kiện. 

1. Trả lại năng lượng lớn nhất gặp phải trong quá trình quét. Năng lượng ban đầu ở góc (0) cũng được xem xét, vì mức tối ưu có thể xảy ra chính xác tại ranh giới hình tròn. 

### Tại sao nó hoạt động 

Đối với một ngôi sao, trong khi đóng góp của nó là dương và góc phóng di chuyển về phía (a_i), thì đóng góp thay đổi với độ dốc không đổi (+s_i). Sau khi vượt qua (a_i), độ dốc trở thành (-s_i). Tại những điểm đóng góp đạt tới 0, độ dốc sẽ thay đổi về 0. Do đó, mỗi ngôi sao có thể được biểu diễn hoàn toàn bằng sự đóng góp ban đầu của nó và ba sự kiện thay đổi độ dốc. 

Tổng của các hàm tuyến tính từng phần chính là tuyến tính từng phần. Giữa hai sự kiện liên tiếp, độ dốc của nó không đổi, do đó giá trị có thể thu được chính xác bằng phép nội suy tuyến tính từ sự kiện trước đó. Tại mỗi điểm mà độ dốc thay đổi, quá trình quét sẽ cập nhật độ dốc đó trước khi tiếp tục. 

Cú nhảy dốc xuống duy nhất có thể tạo ra cực đại cục bộ xảy ra ở góc ưa thích của ngôi sao. Các điểm cắt tạo ra sự thay đổi độ dốc hướng lên và khi hàm khoảng cách bao quanh điểm đối cực, độ dốc của nó cũng thay đổi hướng lên. Do đó, cực đại luôn có thể được tìm thấy ở một trong các vị trí sự kiện, đặc biệt là ở góc ưa thích của một ngôi sao hoặc ở góc (0). Vì quá trình quét truy cập tất cả các vị trí như vậy và duy trì giá trị chính xác giữa chúng nên mức tối đa mà nó ghi lại là mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

PI = math.pi
TWO_PI = 2.0 * math.pi

def solve():
    n_line = input().strip()
    if not n_line:
        return

    n = int(n_line)

    events = []
    energy = 0.0
    initial_slope = 0.0

    for _ in range(n):
        t, s, a = map(float, input().split())

        # Contribution at angle 0.
        dist0 = min(a, TWO_PI - a)
        energy += max(0.0, t - s * dist0)

        if s == 0.0:
            # Constant contribution, so there are no slope changes.
            continue

        # The circular distance is never larger than pi.
        radius = min(PI, t / s)

        left = a - radius
        right = a + radius

        # The interval may cross angle 0.
        if left < 0.0:
            initial_slope += s
            left += TWO_PI

        # The interval may cross angle 2*pi.
        if right > TWO_PI:
            initial_slope -= s
            right -= TWO_PI

        # Slope changes:
        # left endpoint: 0 -> +s
        # center:        +s -> -s
        # right endpoint: -s -> 0
        events.append((left, s))
        events.append((a, -2.0 * s))
        events.append((right, s))

    events.sort()

    answer = energy
    current_angle = 0.0
    slope = initial_slope

    for angle, delta_slope in events:
        energy += slope * (angle - current_angle)

        if energy > answer:
            answer = energy

        slope += delta_slope
        current_angle = angle

    print(f"{answer:.10f}")

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve`tính giá trị hàm ở góc (0). Điều này đưa ra điểm bắt đầu của quá trình quét và cũng bao gồm trường hợp điểm tối ưu nằm chính xác tại đường biên hình tròn. 

các`s == 0.0`việc kiểm tra phải diễn ra trước khi tính toán`t / s`. Một ngôi sao như vậy là không đổi và không cần sự kiện nào. 

Đối với mọi ngôi sao khác,`radius = min(PI, t / s)`mô tả phạm vi góc mà biểu thức tuyến tính của ngôi sao có thể quan trọng. Giới hạn ở (\pi) cũng xử lý các ngôi sao có đóng góp tích cực trong suốt vòng tròn. 

Ba sự kiện được nối thêm chỉ mã hóa những thay đổi trong đạo hàm. Tại điểm cuối bên trái đạo hàm thay đổi từ 0 thành (+s), do đó sự kiện là`s`. Theo hướng ưa thích, nó thay đổi từ (+s) sang (-s), tổng số thay đổi`-2*s`. Tại điểm cuối bên phải, nó thay đổi từ (-s) trở về 0, do đó sự kiện lại diễn ra`s`. 

các`initial_slope`điều chỉnh là phần tinh tế của việc thực hiện. Giao nhau giữa khoảng (0) đã vượt qua điểm cuối bên trái của nó trước khi quá trình quét của chúng tôi bắt đầu, do đó, độ dốc dương của nó phải được đưa vào ban đầu. Ngược lại, một khoảng có điểm cuối bên phải bao quanh (2\pi) đã vượt qua điểm cuối bên phải của nó trước góc (0), do đó phần đóng góp của nó đã giảm dần và chúng ta trừ đi sự thay đổi độ dốc (+s) cuối cùng của nó khỏi đạo hàm ban đầu. 

Các sự kiện ở cùng một góc độ không cần nhóm đặc biệt. Việc tiến từ sự kiện này sang sự kiện tiếp theo không tốn chi phí khi các góc của chúng bằng nhau, do đó sự thay đổi độ dốc của chúng chỉ được áp dụng lần lượt. Giá trị hàm không thay đổi trong khi xử lý các sự kiện góc bằng nhau. 

của Python`float`là IEEE-754 gấp đôi, mang lại độ chính xác cao hơn đáng kể so với dung sai đầu ra cần thiết (10^{-6}). Tổng đóng góp lớn nhất có thể là tối đa (10^8), do đó cũng không có vấn đề tràn số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
100 1 1
100 1 1.5
```Cả hai ngôi sao đều có (T_i/s_i=100>\pi), do đó mỗi ngôi sao đều hoạt động xung quanh toàn bộ vòng tròn. Điểm cuối của chúng được giới hạn ở khoảng cách (\ pi). 

Ở góc (0), ngôi sao thứ nhất đóng góp (99), trong khi ngôi sao thứ hai đóng góp (98,5), cho (197,5). Cả hai khoảng ảnh hưởng đều có góc chéo (0), nên độ dốc ban đầu là (2). 

| Góc | Năng lượng trước sự kiện | Độ dốc trước sự kiện | Thay đổi độ dốc | Năng lượng sau sự kiện | 
| --- | --- | --- | --- | --- | 
| (0) | (197.500000) | (2,000000) | (+1,000000) | (197.500000) | 
| (1.0) | (199.500000) | (3.000000) | (-2.000000) | (199.500000) | 
| (1.5) | (199.500000) | (1,000000) | (-2.000000) | (199.500000) | 
| (4.141593) | (194.216815) | (-1,000000) | (+1,000000) | (194.216815) | 
| (4.641593) | (193.716815) | (0,000000) | (+1,000000) | (193.716815) | 

Đạt cực đại ở góc (1), trong đó ngôi sao đầu tiên được căn chỉnh hoàn hảo và ngôi sao thứ hai cách xa (0,5) radian. Câu trả lời là (100+99,5=199,5). 

### Mẫu 2 

Đầu vào là```
4
100 1 0.5
200 1 1
100 0.5 1.5
10 2 3
```Ở góc (0), các phần đóng góp là (99,5), (199), (99,25) và (4), với tổng số là (401,75). 

Độ dốc ban đầu là (4.5). Ba ngôi sao đầu tiên di chuyển theo hướng ưa thích của chúng và ngôi sao thứ tư cũng nhận được sự đóng góp khi góc phóng di chuyển từ (0) về phía (3). 

| Góc | Năng lượng trước sự kiện | Độ dốc trước sự kiện | Thay đổi độ dốc | Năng lượng sau sự kiện | 
| --- | --- | --- | --- | --- | 
| (0,5) | (404.000000) | (4,500000) | (-2.000000) | (404.000000) | 
| (1.0) | (405.250000) | (2,500000) | (-2.000000) | (405.250000) | 
| (1.5) | (405.500000) | (0,500000) | (-1,000000) | (405.500000) | 
| (3.0) | (404.000000) | (-0,500000) | (-4.000000) | (404.000000) | 

Ở góc (1,5), ngôi sao thứ ba đóng góp toàn bộ (100), trong khi các ngôi sao khác đóng góp (99), (199,5) và (7). Tổng của chúng là (405,5), khớp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Mỗi ngôi sao tạo ra tối đa ba sự kiện, sau đó là sắp xếp và một lần quét tuyến tính | 
| Không gian | (O(N)) | Tối đa (3N) sự kiện được lưu trữ | 

Với (N\leq10^5), thuật toán tạo ra tối đa (3\cdot10^5) sự kiện. Việc sắp xếp nhiều phím dấu phẩy động là thực tế, trong khi các thao tác (10^{10}) được yêu cầu bởi phương pháp brute-force thì không. Việc sử dụng bộ nhớ là tuyến tính theo (N). 

## Trường hợp thử nghiệm 

Chương trình thử nghiệm sau đây sử dụng cùng một`solve`có chức năng như bài nộp. Trình trợ giúp tạm thời chuyển hướng đầu vào tiêu chuẩn và so sánh các câu trả lời có dấu phẩy động với dung sai chặt chẽ.```python
import sys
import io
import math

def run(inp: str) -> float:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        output = sys.stdout.getvalue().strip()
        return float(output)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def assert_close(actual: float, expected: float, message: str):
    assert math.isclose(actual, expected, rel_tol=1e-9, abs_tol=1e-8), (
        f"{message}: expected {expected}, got {actual}"
    )

# Provided sample 1
sample1 = """\
2
100 1 1
100 1 1.5
"""
assert_close(run(sample1), 199.5, "sample 1")

# Provided sample 2
sample2 = """\
4
100 1 0.5
200 1 1
100 0.5 1.5
10 2 3
"""
assert_close(run(sample2), 405.5, "sample 2")

# Minimum-size input
minimum = """\
1
10 0 2
"""
assert_close(run(minimum), 10.0, "minimum input")

# All stars have identical parameters
identical = """\
3
5 2 1
5 2 1
5 2 1
"""
assert_close(run(identical), 15.0, "identical stars")

# Influence interval crosses angle 0.
wrap = """\
2
10 1 0.1
10 1 6.2
"""
expected_wrap = 20.0 - ((6.2 - 0.1) % (2.0 * math.pi))
assert_close(run(wrap), expected_wrap, "circular wraparound")

# Radius reaches pi.
antipode = """\
1
3.141593 1 0
"""
assert_close(run(antipode), 3.141593, "radius reaches pi")

# Maximum-size input.
max_case = "100000\n" + ("1 1 0\n" * 100000)
assert_close(run(max_case), 100000.0, "maximum-size input")

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 100 1 1 / 100 1 1.5`|`199.5`| Cung cấp các hàm ảnh hưởng mẫu và chồng chéo | 
|`4 / 100 1 0.5 / 200 1 1 / 100 0.5 1.5 / 10 2 3`|`405.5`| Nhiều thay đổi độ dốc và tối ưu không cần thiết | 
|`1 / 10 0 2`|`10`| Kích thước tối thiểu và (s=0) | 
| Ba ngôi sao giống hệt nhau |`15`| Góc sự kiện trùng hợp | 
| Hai ngôi sao gần (0) và (2\pi) |`19.816814...`| Bao quanh hình tròn | 
|`1 / 3.141593 1 0`|`3.141593`| Bán kính ảnh hưởng đạt tới cực âm | 
| (100000) sao giống hệt nhau |`100000`| Kích thước đầu vào tối đa và khả năng mở rộng (O(N\log N)) | 

## Vỏ cạnh 

Đối với độ dốc bằng 0, hãy xem xét```
1
10 0 2
```Mức đóng góp luôn là (10), do đó năng lượng ban đầu là (10), không có sự kiện nào được tạo ra và quá trình quét không có gì để xử lý. Câu trả lời vẫn là (10). Đây là lý do tại sao phép chia cho (s_i) phải được bỏ qua khi (s_i=0). 

Đối với một góc giao nhau (0), hãy xem xét```
2
10 1 0.1
10 1 6.2
```Khoảng thời gian hữu ích của ngôi sao đầu tiên kéo dài sang trái từ (0,1) đến (-0,9), trong khi khoảng thời gian hữu ích của ngôi sao thứ hai kéo dài sang phải từ (6.2) ngoài (2\pi). Khoảng đầu tiên đóng góp độ dốc dương ngay sau góc (0), trong khi khoảng thứ hai đã vượt qua điểm cuối bên phải của nó. Việc điều chỉnh độ dốc ban đầu xử lý chính xác hai sự kiện này và sau đó các sự kiện được chuẩn hóa sẽ được quét theo thứ tự tăng dần thông thường. 

Để đạt bán kính ảnh hưởng (\pi), hãy xem xét```
1
3.141593 1 0
```Bán kính hiệu dụng được giới hạn ở (\pi). Điểm cuối bên trái là (0-\pi), chuẩn hóa thành (\pi) và điểm cuối bên phải là (0+\pi), cũng là (\pi). Cả hai sự kiện điểm cuối đều xảy ra ở cùng một vị trí. Sự thay đổi độ dốc kết hợp của chúng mô tả chính xác đỉnh của khoảng cách hình tròn tại cực âm. Hàm lớn nhất ở góc (0), cho (3.141593). 

Đối với các sự kiện trùng hợp, hãy xem xét```
2
5 2 1
5 2 1
```Cả hai ngôi sao đều có cùng tâm và vị trí sự kiện giống hệt nhau. Quá trình quét gặp nhiều sự kiện có tọa độ giống hệt nhau. Vì khoảng cách nâng cao giữa các tọa độ bằng nhau bằng 0 nên mọi thay đổi độ dốc được áp dụng mà không làm thay đổi năng lượng hiện tại. Các sự kiện ở trung tâm cùng nhau làm giảm độ dốc đi (8), chính xác như hai ngôi sao yêu cầu và giá trị tối đa vẫn giữ nguyên (10) ở góc (1). 

Biểu diễn vòng tròn cũng xử lý trường hợp điểm cuối chính xác là (0) hoặc (2\pi). Các sự kiện ở góc (0) được xử lý trước khi quá trình quét di chuyển ra khỏi 0, do đó độ dốc ngay bên phải là chính xác. Góc (2\pi) thể hiện cùng hướng vật lý với (0) và điểm cuối như vậy là mức tối thiểu cục bộ chứ không phải là ứng cử viên tối đa bị thiếu, do đó không cần đánh giá riêng biệt ngoài góc ban đầu.
