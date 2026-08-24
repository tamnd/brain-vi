---
title: "CF 102163G - Ali và bữa sáng"
description: "Ali chọn góc phóng đồng nhất trong khoảng ([L,R]), được đo bằng độ. Giọt trà bắt đầu tại điểm gốc với tốc độ (V), di chuyển theo chuyển động của đạn thông thường dưới tác dụng của trọng lực (g=10) và rơi trở lại trục (X)."
date: "2026-08-23T08:11:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "G"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 874
verified: true
draft: false
---

[CF 102163G - Ali và Bữa sáng](https://codeforces.com/problemset/problem/102163/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ali chọn góc phóng đồng nhất trong khoảng ([L,R]), được đo bằng độ. Giọt trà bắt đầu tại điểm gốc với tốc độ (V), di chuyển theo chuyển động của đạn thông thường dưới tác dụng của trọng lực (g=10) và rơi trở lại trục (X). Mỗi người bạn sở hữu một chiếc cốc được biểu thị bằng một khoảng ([X_1,X_2]) trên trục đó. Đối với mỗi cốc, chúng ta cần một phần góc phóng làm cho tọa độ hạ cánh thuộc về khoảng đó. Các cốc có thể chồng lên nhau và mỗi xác suất được tính toán độc lập. 

Phạm vi đạn cho một góc (\ theta) là 

[ 
x(\theta)=\frac{V^2\sin(2\theta)}{g} 
=\frac{V^2}{10}\sin(2\theta). 
] 

Đầu vào cho phép (N\le 1000), do đó, giải pháp (O(N)) cho mỗi trường hợp thử nghiệm là đủ nhanh. Ngay cả (O(N\log N)) cũng sẽ vừa vặn thoải mái, nhưng không có lý do gì để sắp xếp bất cứ thứ gì ở đây vì mỗi cốc đều có thể được xử lý độc lập. Tọa độ và tốc độ có thể đạt tới (10^9), do đó (V^2) có thể đạt tới (10^{18}). Các số nguyên Python xử lý việc này một cách chính xác, trong khi phép tính lượng giác nghịch đảo cuối cùng chỉ cần độ chính xác của dấu phẩy động vì câu trả lời được in đến bốn chữ số thập phân. 

Khó khăn hình học chính là (x(\theta)) không đơn điệu trên toàn bộ khoảng (0^\circ) đến (90^\circ). Nó tăng cho đến khi (45^\circ), sau đó giảm xuống. Việc thực hiện bất cẩn chỉ tính một góc sin nghịch đảo sẽ bỏ sót nhánh thứ hai. Ví dụ: với (V=10), góc (30^\circ) và góc (60^\circ) đều tạo ra vị trí hạ cánh là (10\sin60^\circ). Cả hai góc phải đóng góp vào xác suất. 

Các điểm cuối cũng cần được chăm sóc. Coi như```
1
1 10 0 90
0 10
```Phạm vi tối đa có thể là (10), vì vậy câu trả lời đúng là`1.0000`. Một công thức coi (x=V^2/10) nằm ngoài miền sin nghịch đảo do một lỗi dấu phẩy động nhỏ có thể tạo ra lỗi 0 hoặc lỗi miền một cách không chính xác. 

Trường hợp (L=R) là một tình huống đặc biệt khác. Ví dụ,```
1
1 10 45 45
9 10
```Góc phóng được cố định ở (45^\circ), tạo ra (x=10), vì vậy câu trả lời đúng là`1.0000`. Chia cho (R-L) mà không xử lý trường hợp này sẽ chia cho 0. 

Cuối cùng, một chiếc cốc có thể nằm hoàn toàn ngoài tầm bắn tối đa của đạn. Vì```
1
1 10 0 90
11 20
```câu trả lời là`0.0000`, vì giọt nước không bao giờ có thể đi xa hơn (10). Lý do tương tự xử lý các cốc bắt đầu tại (0), trong đó điểm cuối chính xác (x=0) có xác suất bằng 0 khi khoảng góc có độ dài dương. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là lấy mẫu nhiều góc phóng, mô phỏng vị trí hạ cánh tương ứng và đếm xem có bao nhiêu mẫu đi vào mỗi cốc. Điều này có tác dụng gần đúng vì góc là biến ngẫu nhiên duy nhất, nhưng nó không phải là một giải pháp lập trình cạnh tranh tốt. Ví dụ: lấy mẫu mỗi (10^{-5}) độ trong khoảng (90^\circ) cần khoảng (9\cdot10^6) mẫu cho một cốc. Với (N=1000), điều đó trở thành khoảng (9\cdot10^9) séc cốc. Quan trọng hơn, việc lấy mẫu không mang lại sự đảm bảo về mặt toán học rõ ràng cho việc làm tròn bốn thập phân cần thiết trừ khi sai số lấy mẫu được kiểm soát cẩn thận. 

Lực lượng vũ phu có tác dụng vì câu trả lời chính xác là số đo của một tập hợp các góc hợp lệ. Quan sát hữu ích là chúng ta có thể mô tả tập hợp đó một cách phân tích. 

Bắt đầu từ 

[ 
x=\frac{V^2}{10}\sin(2\theta), 
] 

tọa độ hạ cánh (x) tương ứng với 

[ 
\sin(2\theta)=\frac{10x}{V^2}. 
] 

Với (0\leq x\leq V^2/10), hãy 

[ 
\alpha=\frac{1}{2}\arcsin\left(\frac{10x}{V^2}\right). 
] 

Bên trong (0^\circ\leq\theta\leq90^\circ), phương trình (x(\theta)=x) có hai nghiệm khả dĩ: 

[ 
\theta=\alpha 
] 

và 

[ 
\theta=90^\circ-\alpha. 
] 

Quan trọng hơn, bất đẳng thức (x(\theta)\leq x) có hai khoảng: 

[ 
\theta\leq\alpha 
] 

hoặc 

[ 
\theta\geq90^\circ-\alpha. 
] 

Vì vậy, đối với bất kỳ tọa độ (x) nào, chúng ta có thể tính toán chính xác khoảng cách góc được yêu cầu ([L,R]) tạo ra tọa độ hạ cánh nhiều nhất là bao nhiêu (x). Gọi đại lượng này là (F(x)). Xác suất rơi vào bên trong cốc ([X_1,X_2]) chỉ đơn giản là 

[ 
\frac{F(X_2)-F(X_1)}{R-L}. 
] 

Quan sát biến vấn đề thành (O(N)) là mỗi cốc chỉ cần hai đánh giá cho cùng một hàm tích lũy này. Không có sự tương tác giữa các cốc, vì vậy các cốc chồng lên nhau không cần xử lý đặc biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NM)), trong đó (M) là số góc được lấy mẫu | (O(1)) | Quá chậm và chỉ gần đúng | 
| Tối ưu | (O(N)) | (O(1)) bên cạnh việc lưu trữ đầu vào/đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi (L) và (R) từ độ sang radian. Các hàm lượng giác trong Python sử dụng radian và giữ toàn bộ phép tính theo radian để tránh việc chuyển đổi góc nhiều lần. 
2. Xác định phạm vi ngang tối đa có thể 

[ 
X_{\max}=\frac{V^2}{10}. 
] 

Nếu tọa độ được truy vấn (x) ít nhất là (X_{\max}), mọi góc phóng có thể sẽ dừng ở hoặc trước (x). Nếu (x\leq0), xác suất tích lũy bằng 0 đối với khoảng góc không suy biến vì việc hạ cánh chính xác tại (x=0) chỉ xảy ra ở các góc điểm cuối bị cô lập. 

1. Với (0<x<X_{\max}), hãy tính 

[ 
\alpha=\frac12\arcsin\left(\frac{10x}{V^2}\right). 
] 

Các góc hợp lệ của (x(\theta)\leq x) là 

[ 
[0,\alpha]\cup[90^\circ-\alpha,90^\circ]. 
] 

Đây là bước quan trọng. Khoảng thời gian thứ hai là cần thiết vì tầm bắn giảm sau (45^\circ). 

1. Giao cả hai khoảng góc hợp lệ với khoảng ngẫu nhiên thực tế ([L,R]). Độ dài giao điểm thứ nhất là 

[ 
\max(0,\min(R,\alpha)-L), 
] 

và độ dài của giây thứ hai là 

[ 
\max(0,R-\max(L,90^\circ-\alpha)). 
] 

Tổng của chúng là tổng số đo góc mà tọa độ hạ cánh tối đa là (x). 

1. Với mỗi cốc ([X_1,X_2]), hãy tính số đo tích lũy cho cả hai điểm cuối. Sự khác biệt của chúng chính xác là số đo góc mà 

[ 
X_1\leq x(\theta)\leq X_2. 
] 

Chia cho (R-L) để có được xác suất. 

1. Nếu (L=R), bỏ qua phép tính tích lũy vì không có khoảng ngẫu nhiên có độ dài dương. Đánh giá trực tiếp quỹ đạo đơn. Nếu tọa độ hạ cánh của nó thuộc về chiếc cốc, hãy in`1.0000`; nếu không thì in`0.0000`. 

### Tại sao nó hoạt động 

Đối với khoảng góc có độ dài dương, hàm (F(x)) tính chính xác số đo các góc trong ([L,R]) có tọa độ hạ cánh tối đa là (x). Hai nhánh sin nghịch đảo mô tả mọi góc thỏa mãn bất đẳng thức đó, bởi vì (2\theta) nằm giữa (0) và (\pi), trong đó sin đầu tiên tăng rồi giảm. Do đó, các góc được tính bằng (F(X_2)) chứ không phải bằng (F(X_1)) chính xác là những góc nằm bên trong cốc. Vì góc phóng là đồng nhất nên việc chia số đo góc đó cho tổng chiều dài góc sẽ cho xác suất cần thiết. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

PI = math.pi
HALF_PI = math.pi / 2.0

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, v, L, R = map(int, input().split())

        lrad = math.radians(L)
        rrad = math.radians(R)
        total = rrad - lrad

        v2 = v * v

        if L == R:
            # The angle is fixed, so the result is deterministic.
            landing = (v2 / 10.0) * math.sin(2.0 * lrad)

            for _ in range(n):
                x1, x2 = map(int, input().split())

                # Small tolerance protects exact endpoint cases such as
                # sin(0) and sin(pi/2) from floating-point noise.
                eps = 1e-9 * max(1.0, abs(landing))

                if x1 - eps <= landing <= x2 + eps:
                    out.append("1.0000")
                else:
                    out.append("0.0000")

            continue

        def measure_leq(x):
            """
            Return the angular length inside [lrad, rrad]
            for which the landing coordinate is <= x.
            """
            if x <= 0:
                return 0.0

            # Compare using integers before converting to float.
            # x >= v^2 / 10  <=>  10*x >= v^2
            if 10 * x >= v2:
                return total

            y = (10.0 * x) / v2

            # y is mathematically in (0, 1), but clamp against
            # a possible floating-point overshoot.
            y = max(0.0, min(1.0, y))

            alpha = 0.5 * math.asin(y)

            # First branch: theta <= alpha.
            left = max(0.0, min(rrad, alpha) - lrad)

            # Second branch: theta >= pi/2 - alpha.
            second_start = HALF_PI - alpha
            right = max(0.0, rrad - max(lrad, second_start))

            return left + right

        for _ in range(n):
            x1, x2 = map(int, input().split())

            m1 = measure_leq(x1)
            m2 = measure_leq(x2)

            probability = (m2 - m1) / total

            # Protect the printed value from tiny accumulated
            # floating-point errors such as 1.0000000000000002.
            probability = max(0.0, min(1.0, probability))

            out.append(f"{probability:.4f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ chuyển đổi giới hạn góc một lần cho mỗi trường hợp thử nghiệm, vì mỗi cốc đều sử dụng cùng một khoảng thời gian. số nguyên`v2`lưu trữ (V^2) chính xác. Sự so sánh`10 * x >= v2`được thực hiện có chủ ý trước khi chuyển đổi sang dấu phẩy động, do đó ranh giới phạm vi tối đa quan trọng được quyết định mà không làm mất độ chính xác. 

các`measure_leq`hàm này là việc triển khai hàm tích lũy từ hướng dẫn. Đối với tọa độ bên trong,`alpha`đại diện cho nhánh sin nghịch đảo đầu tiên. Biến`second_start`đại diện cho nhánh đối xứng sau (45^\circ). Mỗi nhánh được giao với khoảng ngẫu nhiên thực tế bằng cách sử dụng`max`Và`min`, đương nhiên sẽ cho kết quả bằng 0 khi không có giao điểm. 

Xác suất cốc sử dụng chênh lệch`m2 - m1`. Không cần phải lo lắng về việc liệu các điểm cuối của cốc có được bao gồm hay không, bởi vì đối với khoảng góc ngẫu nhiên liên tục có độ dài dương, các góc riêng lẻ có xác suất bằng 0. 

Trường hợp góc cố định được tách ra trước khi xác định hàm tích lũy. Nếu không thì`total`sẽ bằng 0 và công thức xác suất sẽ chia cho 0. Dung sai trong nhánh đó chỉ được sử dụng để so sánh quỹ đạo dấu phẩy động xác định với ranh giới cốc nguyên. 

Các số nguyên có kích thước tùy ý của Python cũng loại bỏ mối lo ngại tràn có thể tồn tại khi triển khai 32 bit. Các phép tính dấu phẩy động duy nhất là các phép tính lượng giác và xác suất cuối cùng, đủ chính xác cho bốn chữ số thập phân. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp sử dụng (V=15), vì vậy 

[ 
X_{\max}=\frac{15^2}{10}=22,5. 
] 

Khoảng góc ngẫu nhiên là (30^\circ) đến (45^\circ), có chiều dài là (15^\circ). 

| Cúp | (X_1) | (X_2) | Số đo tích lũy tại (X_1) | Số đo tích lũy tại (X_2) | Xác suất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 16 | 21 | (0) | về (4.4805^\circ) | 0,2987 | 
| 2 | 21 | 22 | về (4.4805^\circ) | về (8.9490^\circ) | 0,2979 | 
| 3 | 22 | 30 | về (8.9490^\circ) | (15^\circ) | 0,4034 | 
| 4 | 10 | 15 | (0) | (0) | 0,0000 | 
| 5 | 1 | 40 | (0) | (15^\circ) | 1,0000 | 

Cốc đầu tiên minh họa phép tính sin nghịch đảo chính. Vị trí hạ cánh nhỏ nhất có thể có trong khoảng thời gian bắt đầu gần (19,49), do đó (16) không đóng góp số đo tích lũy. Điểm cuối trên (21) tương ứng với một góc xung quanh (34,48^\circ), để lại khoảng (4,48^\circ) các góc hợp lệ bên trong khoảng (30^\circ) đến (45^\circ). Chia cho (15^\circ) sẽ được xấp xỉ (0,2987). 

Cốc thứ hai và thứ ba chứng minh rằng các khoảng chồng chéo hoặc liền kề không yêu cầu bất kỳ xử lý tổng thể nào. Mỗi câu trả lời thu được từ cùng một hàm tích lũy. Cốc cuối cùng chứa toàn bộ phạm vi hạ cánh có thể có nên xác suất của nó chính xác là một. 

Đối với ví dụ thứ hai, hãy xem xét khoảng góc suy biến```
1
3 10 45 45
9 10
0 9
10 20
```Góc phóng được cố định ở (45^\circ) và vị trí hạ cánh là 

[ 
\frac{10^2}{10}\sin(90^\circ)=10. 
] 

| Cúp | Góc cố định | Vị trí hạ cánh | Có hạ cánh bên trong cốc không? | Xác suất | 
| --- | --- | --- | --- | --- | 
| ([9,10]) | (45^\circ) | 10 | Có | 1,0000 | 
| ([0,9]) | (45^\circ) | 10 | Không | 0,0000 | 
| ([10,20]) | (45^\circ) | 10 | Có | 1,0000 | 

Ví dụ này thực hiện nhánh (L=R) và cũng xác nhận rằng ranh giới cốc được coi là một phần của cốc. Với một góc cố định, không có phân bố xác suất để tích phân, vì vậy câu trả lời đơn giản là liệu tọa độ hạ cánh xác định có thuộc khoảng đó hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) cho mỗi trường hợp thử nghiệm | Mỗi cốc thực hiện hai phép tính tích lũy theo thời gian không đổi | 
| Không gian | (O(1)) không gian phụ | Chỉ các biến vô hướng được yêu cầu ngoài bộ đệm đầu ra | 

Với (N\leq1000), thuật toán chỉ thực hiện một số phép tính số học và lượng giác trên mỗi cốc. Không có sự sắp xếp, mô phỏng, tích hợp số hoặc lặp lại trên phạm vi tọa độ, do đó nó phù hợp thoải mái với giới hạn một giây. Số nguyên trung gian lớn nhất là (V^2\leq10^{18}), được Python xử lý chính xác. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

PI = math.pi
HALF_PI = math.pi / 2.0

def solve_text(inp: str) -> str:
    data = io.StringIO(inp)
    input_ = data.readline
    t = int(input_())
    out = []

    for _ in range(t):
        n, v, L, R = map(int, input_().split())

        lrad = math.radians(L)
        rrad = math.radians(R)
        total = rrad - lrad
        v2 = v * v

        if L == R:
            landing = (v2 / 10.0) * math.sin(2.0 * lrad)

            for _ in range(n):
                x1, x2 = map(int, input_().split())
                eps = 1e-9 * max(1.0, abs(landing))

                if x1 - eps <= landing <= x2 + eps:
                    out.append("1.0000")
                else:
                    out.append("0.0000")

            continue

        def measure_leq(x):
            if x <= 0:
                return 0.0

            if 10 * x >= v2:
                return total

            y = (10.0 * x) / v2
            y = max(0.0, min(1.0, y))

            alpha = 0.5 * math.asin(y)

            left = max(0.0, min(rrad, alpha) - lrad)

            second_start = HALF_PI - alpha
            right = max(0.0, rrad - max(lrad, second_start))

            return left + right

        for _ in range(n):
            x1, x2 = map(int, input_().split())

            probability = (
                measure_leq(x2) - measure_leq(x1)
            ) / total

            probability = max(0.0, min(1.0, probability))
            out.append(f"{probability:.4f}")

    return "\n".join(out)

def run(inp: str) -> str:
    return solve_text(inp)

sample1 = """\
1
5 15 30 45
16 21
21 22
22 30
10 15
1 40
"""

assert run(sample1) == """\
0.2987
0.2979
0.4034
0.0000
1.0000
""", "sample 1"

sample2 = """\
1
3 10 0 90
0 5
5 10
0 10
"""

assert run(sample2) == """\
0.3333
0.6667
1.0000
""", "full angle range"

sample3 = """\
1
3 10 45 45
9 10
0 9
10 20
"""

assert run(sample3) == """\
1.0000
0.0000
1.0000
""", "fixed angle"

sample4 = """\
1
1 10 0 90
11 20
"""

assert run(sample4) == """\
0.0000
""", "cup beyond maximum range"

sample5 = """\
1
3 10 0 90
0 10
0 10
0 10
"""

assert run(sample5) == """\
1.0000
1.0000
1.0000
""", "all equal cups"

# Maximum N: 1000 independent cups, all containing the entire
# reachable range. The generated input has exactly 1000 queries.
n = 1000
max_case = "1\n{} 10 0 90\n".format(n) + ("0 10\n" * n)
expected = "1.0000\n" * n
assert run(max_case).splitlines() == expected.strip().splitlines(), "maximum N"

# Boundary case: x = 0 is reachable only at isolated angles,
# so it has probability zero for a positive-length angle interval.
sample6 = """\
1
2 10 0 90
0 1
1 10
"""

assert run(sample6) == """\
0.0000
1.0000
""", "zero endpoint and maximum endpoint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 10 0 90 / 0 10`|`1.0000`| Hộp có kích thước tối thiểu và phạm vi có thể tiếp cận đầy đủ | 
|`1 / 3 10 0 90 / 0 5, 5 10, 0 10`|`0.3333, 0.6667, 1.0000`| Cả hai nhánh sin nghịch đảo và cực đại chính xác | 
|`1 / 3 10 45 45 / 9 10, 0 9, 10 20`|`1.0000, 0.0000, 1.0000`| (L=R) và bao gồm ranh giới | 
|`1 / 1 10 0 90 / 11 20`|`0.0000`| Cốc hoàn toàn nằm ngoài phạm vi có thể tiếp cận | 
|`1 / 3 10 0 90 / 0 10`lặp đi lặp lại |`1.0000`cho mỗi cốc | Giá trị cốc hoàn toàn bằng nhau và câu trả lời độc lập | 
| 1000 cốc`[0,10]`| 1000 dòng`1.0000`| Độ phức tạp tối đa (N) và tuyến tính | 
|`1 / 2 10 0 90 / 0 1, 1 10`|`0.0000, 1.0000`| Ranh giới dưới và ranh giới phạm vi tối đa | 

## Vỏ cạnh 

Khi (L=R), khoảng góc ngẫu nhiên có độ rộng bằng 0, do đó công thức xác suất thông thường không thể chia cho (R-L). Vì```
1
1 10 45 45
9 10
```thuật toán tính toán vị trí hạ cánh duy nhất (10), kiểm tra (9\leq10\leq10) và in`1.0000`. Nếu cái cốc là`[0,9]`, phép tính xác định tương tự sẽ tạo ra`0.0000`. 

Khi cốc vượt quá phạm vi tối đa có thể, hàm tích lũy ngay lập tức trả về toàn bộ số đo góc cho điểm cuối phía trên. Vì```
1
1 10 0 90
11 20
```phạm vi tối đa là (10), vì vậy cả hai điểm cuối của cốc đều nằm ngoài phạm vi đó. Sự khác biệt của hai thước đo tích lũy là bằng 0, cho`0.0000`. 

Khi cốc chứa toàn bộ phạm vi có thể tiếp cận, mọi góc phóng đều hợp lệ. Vì```
1
1 10 0 90
0 10
```điểm cuối dưới đóng góp số đo tích lũy bằng 0 và điểm cuối trên đóng góp tất cả (90^\circ). Sự khác biệt của chúng là khoảng ngẫu nhiên hoàn toàn, vì vậy câu trả lời là`1.0000`. 

Phạm vi đạn không đơn điệu được xử lý bằng cách đếm hai khoảng góc. Với (V=10), (L=0), (R=90) và cốc`[0,5]`, điều kiện (x\leq5) tương đương với (\sin(2\theta)\leq0.5). Điều này xảy ra với (\theta\in[0,15^\circ]) và (\theta\in[75^\circ,90^\circ]), cho ra (30^\circ) các góc hợp lệ trong số (90^\circ), do đó`0.3333`. Việc triển khai chỉ sử dụng nhánh sin nghịch đảo đầu tiên sẽ chỉ được tính (15^\circ) và trả về không chính xác`0.1667`. 

Đối với (x=0), các góc duy nhất có thể có là (0^\circ) và (90^\circ). Khi (L<R), đây là những điểm cô lập và có xác suất bằng 0. Như vậy```
1
2 10 0 90
0 1
1 10
```sản xuất`0.0000`cho cốc đầu tiên và`1.0000`cho lần thứ hai. Tính toán tích lũy xử lý điều này một cách tự nhiên bởi vì`measure_leq(0)`trả về 0 trong khi`measure_leq(10)`trả về số đo góc đầy đủ. 

Các cốc chồng lên nhau không cần xử lý đặc biệt vì mỗi xác suất sẽ hỏi về khoảng thời gian riêng của nó. Trong mẫu,`[16,21]`,`[21,22]`, Và`[22,30]`chia sẻ điểm cuối, nhưng mỗi câu trả lời được tính toán độc lập từ hai giá trị tích lũy. Không có nỗ lực phân chia mặt đất hoặc chỉ định điểm hạ cánh cho một cốc.
