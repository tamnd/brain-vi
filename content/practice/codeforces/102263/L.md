---
title: "CF 102263L - Bánh mì kẹp thịt"
description: "Bảng vô hạn là tuần hoàn. Mỗi khối (n lần m) chứa các giá trị giống nhau, do đó chỉ có vị trí bên trong một khối là quan trọng. Đặt hàng và cột cục bộ dựa trên số 0 là (x) và (y). Giá trị bánh mì kẹp thịt ở vị trí đó là [ f(x,y)=xm+y+1."
date: "2026-08-17T20:10:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "L"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 137
verified: true
draft: false
---

[CF 102263L - Bánh mì kẹp thịt](https://codeforces.com/problemset/problem/102263/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bảng vô hạn là tuần hoàn. Mọi khối (n \times m) đều chứa các giá trị giống nhau, do đó chỉ có vị trí bên trong một khối là quan trọng. Đặt hàng và cột cục bộ dựa trên số 0 là (x) và (y). Giá trị burger ở vị trí đó là 

[ 
f(x,y)=xm+y+1. 
] 

Sau khi ăn xong, người chơi di chuyển theo (r) hàng và (c) cột. Vì chúng ta chỉ quan tâm đến vị trí bên trong khối lặp nên chuỗi các trạng thái là 

[ 
(x+kr)\bmod n,\qquad (y+kc)\bmod m. 
] 

Người chơi dừng lại ngay trước khi ăn một chiếc bánh mì kẹp thịt có giá trị đã xuất hiện. Bởi vì mọi giá trị từ (1) đến (nm) xảy ra ở chính xác một vị trí cục bộ, nên hai trạng thái có độ ngon như nhau chính xác khi cả dư lượng hàng và cột của chúng bằng nhau. Do đó quá trình dừng lại khi cặp dư lượng lặp lại. 

Đối với tọa độ hàng, số lượng vị trí riêng biệt được truy cập là 

[ 
p_n=\frac{n}{\gcd(n,r)}. 
] 

Đối với tọa độ cột nó là 

[ 
p_m=\frac{m}{\gcd(m,c)}. 
] 

Cặp hoàn chỉnh lặp lại sau 

[ 
L=\tên toán tử{lcm}(p_n,p_m) 
] 

di chuyển. Người chơi ăn chính xác (L) bánh mì kẹp thịt. 

Các ràng buộc quá lớn để mô phỏng. Cả hai thứ nguyên đều có thể đạt tới (2\cdot10^9), do đó có thể có tới (4\cdot10^{18}) vị trí trong một khoảng thời gian. Giới hạn bài toán chính thức chỉ là 0,5 giây, do đó, ngay cả phương pháp (O(nm)) cũng không thể thực hiện được, chứ chưa nói đến việc lặp qua một quỹ đạo hoàn chỉnh từ mọi ô bắt đầu. Lời giải phải giảm quỹ đạo xuống một vài phép tính gcd, lcm và tổng số học. 

Có một số bẫy dễ dàng. Vì`1 1 1 1`, chiếc burger duy nhất có giá trị (1), chiếc burger đầu tiên được ăn và trạng thái tiếp theo có cùng giá trị, vì vậy câu trả lời là`1`. Một giải pháp giả sử người chơi luôn ăn ít nhất hai chiếc bánh mì kẹp thịt sẽ chỉ ăn một chiếc. 

Vì`1 4 1 2`, các cột được truy cập từ bất kỳ cột bắt đầu nào đều có cùng tính chẵn lẻ. Quỹ đạo tốt nhất ghé thăm các cột có giá trị (2) và (4), cho kết quả (2+4=6). Một giải pháp bất cẩn giả sử bước (2) cuối cùng truy cập vào cả bốn cột sẽ sử dụng sai tổng (1+2+3+4=10). Đầu ra mẫu chính xác là`6`. 

Vì`4 3 2 1`, bước hàng có (\gcd(4,2)=2), do đó chỉ có hai hàng thuộc về bất kỳ chu kỳ hàng nào. Một giải pháp xử lý bước hàng như thể nó nguyên tố cùng nhau với (n) sẽ bao gồm các giá trị từ cả bốn hàng. Câu trả lời đúng là`48`. 

Cuối cùng, khi (r=n) hoặc (c=m), tọa độ tương ứng không thay đổi chút nào. Ví dụ, với`3 2 3 2`, bạn sẽ đạt được cùng một ô sau mỗi lần di chuyển, vì vậy ô bắt đầu tối ưu chỉ đơn giản là ô dưới cùng bên phải, có giá trị là (6). Câu trả lời là`6`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn mọi vị trí bắt đầu có thể, mô phỏng các bước nhảy cho đến khi xảy ra giá trị đã ăn trước đó và giữ số tiền lớn nhất. Đối với một vị trí bắt đầu, việc này cần lặp lại (L), trong đó 

[ 
L=\operatorname{lcm}\left(\frac n{\gcd(n,r)},\frac m{\gcd(m,c)}\right). 
] 

Có thể có (nm) vị trí bắt đầu cục bộ, do đó số lượng hoạt động mạnh mẽ là 

[ 
O(nmL). 
] 

Trong trường hợp xấu nhất, hai chu kỳ rút gọn có thể nguyên tố cùng nhau, cho (L) gần với (nm). Với cả hai chiều gần (2\cdot10^9), điều này có thể tiếp cận ((nm)^2), khoảng (1.6\cdot10^{37}) bước quỹ đạo. Phương pháp vũ phu là đúng vì nó tuân theo quy trình theo đúng nghĩa đen, nhưng sự phụ thuộc vào thời gian khiến nó không thể sử dụng được. 

Quan sát hữu ích là tọa độ hàng và cột tiến triển độc lập. Đối với một chiều có kích thước (q) với bước (d), hãy 

[ 
g=\gcd(q,d),\qquad p=\frac qg. 
] 

Bắt đầu từ (các) dư lượng, dư lượng đã truy cập chính xác là 

[ 
s,\ s+g,\ s+2g,\ldots,s+(p-1)g 
] 

sau khi giảm chúng theo modulo (q). Nói cách khác, một quỹ đạo đi qua một lớp dư lượng hoàn chỉnh modulo (g). Mọi điểm bắt đầu có cùng dư lượng modulo (g) tạo ra cùng một tập hợp các vị trí. 

Tổng của lớp dư lượng đó là 

[ 
p s+g\frac{p(p-1)}2. 
] 

Hệ số của (s) là dương nên tổng lớn nhất đến từ (s=g-1). Do đó, tổng tối đa trong một kỳ cho thứ nguyên này là 

[ 
S(q,d)=p(g-1)+g\frac{p(p-1)}2. 
] 

Chu kỳ hàng và chu kỳ cột có thể có các chu kỳ khác nhau, do đó, chu kỳ hàng được lặp lại (L/p_n) lần trong quỹ đạo hai chiều hoàn chỉnh và chu kỳ cột được lặp lại (L/p_m) lần. 

Giá trị bánh mì kẹp thịt là (xm+y+1), do đó tổng đóng góp được chia thành phần hàng, phần cột và một hằng số (1) cho mỗi chiếc bánh mì kẹp thịt được ăn. Sự tách biệt này cho phép chúng tôi tối đa hóa chu kỳ hàng tốt nhất và chu kỳ cột tốt nhất một cách độc lập. Hàng bắt đầu và cột bắt đầu tương ứng có thể được kết hợp đơn giản thành một ô bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nmL)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\log(\max(n,m)))) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính (g_n=\gcd(n,r)) và (p_n=n/g_n). Tọa độ hàng trở về giá trị bắt đầu sau khi nhảy chính xác (p_n), bởi vì (p_n r) là bội số dương nhỏ nhất của (n). 
2. Tính (g_m=\gcd(m,c)) và (p_m=m/g_m) theo cách tương tự cho cột. Ô cục bộ đầy đủ lặp lại sau 

[ 
L=\tên toán tử{lcm}(p_n,p_m). 
] 

Đây là số lượng bánh mì kẹp thịt được ăn trước khi chiếc bánh mì kẹp thịt tiếp theo có giá trị đã thấy trước đó. 

1. Đối với kích thước hàng, hãy tính 

[ 
S_n=p_n(g_n-1)+g_n\frac{p_n(p_n-1)}2. 
] 

Đây là tổng tối đa của các chỉ số hàng dựa trên số 0 trong một chu kỳ hàng. Hàng bắt đầu tốt nhất thuộc về lớp dư lượng (g_n-1\pmod {g_n}). 

1. Đối với kích thước cột, tính 

[ 
S_m=p_m(g_m-1)+g_m\frac{p_m(p_m-1)}2. 
] 

Đây là tổng tối đa tương ứng của các chỉ số cột dựa trên số 0 trong một chu kỳ cột. 

1. Một chu kỳ hàng có độ dài (p_n) xảy ra (L/p_n) lần trong suốt quỹ đạo hoàn chỉnh. Do đó tổng số chỉ số hàng là 

[ 
S_n\frac{L}{p_n}. 
] 

Tương tự, tổng chỉ số cột là 

[ 
S_m\frac{L}{p_m}. 
] 

1. Chuyển đổi số tiền tọa độ đó thành độ ngon của bánh mì kẹp thịt. Mỗi chỉ số hàng đóng góp một hệ số (m), mỗi chỉ số cột đóng góp trực tiếp và mỗi chiếc bánh mì kẹp thịt đã ăn đóng góp (1). Câu trả lời là 

[ 
mS_n\frac{L}{p_n} 
+ 
S_m\frac{L}{p_m} 
+ 
L. 
] 

Lấy biểu thức này theo modulo (10^9+7).

Tại sao nó hoạt động: mọi quỹ đạo là một quỹ đạo tịnh tiến ((x,y)\mapsto(x+r,y+c)) trên hình xuyến hữu hạn (\mathbb Z_n\times\mathbb Z_m). Chiều dài của nó là (L), do đó, chính xác một quỹ đạo hoàn chỉnh đã bị ăn hết. Trong mỗi tọa độ, quỹ đạo bao gồm một lớp dư lượng modulo gcd tương ứng và tổng tối đa của lớp đó thu được bằng cách chọn dư lượng lớn nhất của nó. Vì giá trị burger là tổng của một số hạng hàng độc lập, một số hạng cột độc lập và một hằng số, nên việc tối đa hóa hai tổng tọa độ một cách độc lập cũng tối đa hóa tổng quỹ đạo hoàn chỉnh. Do đó, công thức đánh giá ô khởi đầu tốt nhất có thể. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

MOD = 10**9 + 7

def cycle_info(q, d):
    g = gcd(q, d)
    p = q // g

    # Maximum sum of residues in one cycle.
    # The best residue class starts at g - 1:
    # (g - 1), (2g - 1), ..., (pg - 1)
    s = p * (g - 1) + g * p * (p - 1) // 2
    return g, p, s

def solve():
    n, m, r, c = map(int, input().split())

    _, pn, row_sum = cycle_info(n, r)
    _, pm, col_sum = cycle_info(m, c)

    # lcm(pn, pm)
    L = pn // gcd(pn, pm) * pm

    row_repetitions = L // pn
    col_repetitions = L // pm

    answer = 0
    answer += (m % MOD) * (row_sum % MOD) % MOD
    answer %= MOD
    answer = answer * (row_repetitions % MOD) % MOD

    answer += (col_sum % MOD) * (col_repetitions % MOD) % MOD
    answer += L % MOD

    print(answer % MOD)

if __name__ == "__main__":
    solve()
```các`cycle_info`hàm thực hiện phép giảm toán học một chiều.`g`xác định lớp dư lượng nào được bảo toàn bằng bước nhảy, trong khi`p`là số vị trí trong một lớp như vậy. Cấp số cộng bắt đầu từ (g-1) vì nó cho tổng lớn nhất có thể. 

Chu kỳ đầy đủ là lcm của chu kỳ hàng và cột. biểu thức`pn // gcd(pn, pm) * pm`tính toán nó mà không xây dựng bất kỳ quỹ đạo nào. 

Đóng góp hàng cần cả tổng chu kỳ hàng và số lần chu kỳ đó xảy ra trong khoảng thời gian kết hợp. yếu tố`m`chuyển đổi chỉ số hàng dựa trên số 0 thành phần đóng góp của nó vào giá trị bánh mì kẹp thịt. Sự đóng góp của cột không cần thêm yếu tố thứ nguyên. 

Số nguyên Python có độ chính xác tùy ý, do đó, các tích trung gian vẫn an toàn mặc dù chu kỳ hoàn chỉnh có thể ở khoảng (4\cdot10^{18}). Mã này vẫn giảm tích số modulo (10^9+7) trong khi thực hiện, điều này giữ cho số học nhỏ gọn và phản ánh trực tiếp kết quả đầu ra được yêu cầu. 

Việc sử dụng tọa độ dựa trên số 0 là có chủ ý. Với hàng (x) và cột (y), giá trị bánh mì kẹp thịt chính xác là (mx+y+1). Việc sử dụng tọa độ dựa trên một sẽ yêu cầu điều chỉnh bổ sung trong mỗi cấp số cộng và là nguồn dễ gây ra lỗi từng cái một. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`3 3 1 1`. Cả hai chiều đều có gcd (1), vì vậy mọi tọa độ đều được truy cập đầy đủ. 

| Kích thước | (g) | Thời kỳ (p) | Tổng chu kỳ tốt nhất | Lặp lại | 
| --- | --- | --- | --- | --- | 
| Hàng | 1 | 3 | 3 | 1 | 
| Cột | 1 | 3 | 3 | 1 | 

Khoảng thời gian hoàn chỉnh là (L=3). Đóng góp của hàng là (3\cdot3=9), đóng góp của cột là (3) và đóng góp không đổi là (3). Tổng số là (9+3+3=15), phù hợp với mẫu. 

Đối với Mẫu 2, đầu vào là`1 4 1 2`. Kích thước hàng chỉ có một vị trí. Đối với các cột, (\gcd(4,2)=2), do đó mỗi quỹ đạo sẽ truy cập vào một trong hai lớp chẵn lẻ. 

| Kích thước | (g) | Thời kỳ (p) | Tổng chu kỳ tốt nhất | Lặp lại | 
| --- | --- | --- | --- | --- | 
| Hàng | 1 | 1 | 0 | 2 | 
| Cột | 2 | 2 | 4 | 1 | 

Khoảng thời gian hoàn chỉnh là (L=2). Lớp cột tốt nhất chứa các cột dựa trên số 0 (1) và (3), tương ứng với các giá trị burger (2) và (4). Câu trả lời là (0+4+2=6). Điều này chứng tỏ tại sao gcd lại quan trọng: bước nhảy không truy cập vào mọi cột. 

Đối với Mẫu 3, đầu vào là`2000000000 1 1 1`. Đóng góp của cột bằng 0 vì chỉ có một cột. Khoảng thời gian hàng là (2\cdot10^9) và chu kỳ hàng tốt nhất chứa mọi hàng. Câu trả lời là 

[ 
\frac{2000000000\cdot1999999999}{2}+2000000000. 
] 

Modulo (10^9+7), đây là`91`, như trong mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log(\max(n,m)))) | Hai phép tính gcd và số học theo thời gian không đổi | 
| Không gian | (O(1)) | Chỉ một số biến số nguyên cố định được lưu trữ | 

Kích thước có thể là (2\cdot10^9), nhưng thuật toán không bao giờ lặp qua các hàng, cột, ô hoặc vị trí quỹ đạo. Thuật toán Euclid kết thúc theo thời gian logarit nên lời giải dễ dàng phù hợp với giới hạn 0,5 giây và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

MOD = 10**9 + 7

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, m, r, c = map(int, sys.stdin.readline().split())

        def cycle_sum(q, d):
            g = gcd(q, d)
            p = q // g
            s = p * (g - 1) + g * p * (p - 1) // 2
            return p, s

        pn, row_sum = cycle_sum(n, r)
        pm, col_sum = cycle_sum(m, c)

        L = pn // gcd(pn, pm) * pm

        ans = (
            (m % MOD) * (row_sum % MOD) % MOD
            * (L // pn)
            + (col_sum % MOD) * (L // pm)
            + L
        ) % MOD

        return str(ans)
    finally:
        sys.stdin = old_stdin
        output = sys.stdout.getvalue()
        sys.stdout = old_stdout

# Provided samples
assert solve_io("3 3 1 1\n") == "15", "sample 1"
assert solve_io("1 4 1 2\n") == "6", "sample 2"
assert solve_io("2000000000 1 1 1\n") == "91", "sample 3"

# Minimum size, only one burger and one-step repetition
assert solve_io("1 1 1 1\n") == "1", "minimum-size case"

# All burgers in the trajectory have the same local value
assert solve_io("1 2 1 2\n") == "2", "all-equal trajectory"

# Boundary case r = n and c = m, so the same cell is reached immediately
assert solve_io("3 2 3 2\n") == "6", "zero movement modulo dimensions"

# Partial gcd cycles in both dimensions
assert solve_io("4 3 2 1\n") == "48", "non-coprime row step"

# Maximum-size dimensions
assert solve_io("2000000000 2000000000 1 1\n") == "999998628", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Kích thước tối thiểu và giai đoạn một | 
|`1 2 1 2`|`2`| Tọa độ có bước nhảy chính xác là kích thước của nó | 
|`3 2 3 2`|`6`| Cả hai lần nhảy đều có kích thước tương ứng | 
|`4 3 2 1`|`48`| Bước hàng không nguyên tố cùng nhau và chu kỳ hàng lặp lại | 
|`2000000000 2000000000 1 1`|`999998628`| Kích thước tối đa và số học mô-đun lớn | 

## Vỏ cạnh 

cho`1 1 1 1`, chúng ta có (g_n=g_m=1), (p_n=p_m=1) và (L=1). Tổng hàng và cột đều bằng 0 vì tọa độ dựa trên 0 của chúng luôn bằng 0. Công thức cho (0+0+1=1), vậy có đúng một chiếc bánh mì kẹp thịt được ăn. 

Vì`1 4 1 2`, cột gcd là (2), cho ra (p_m=2). Lớp dư lượng tốt nhất là (1\pmod2), chứa các cột dựa trên 0 (1) và (3). Tổng của chúng là (4) và quỹ đạo có chiều dài (2). Kết quả là (4+2=6). Thuật toán không bao giờ giả định rằng một bước không nguyên tố cùng nhau truy cập vào toàn bộ chiều. 

Vì`3 2 3 2`, cả hai bước nhảy đều bằng kích thước của chúng. Chúng ta nhận được (p_n=p_m=1), do đó (L=1). Phần dư của hàng tốt nhất là (2), phần dư của cột tốt nhất là (1) và chiếc bánh mì kẹp thịt duy nhất được ăn có giá trị (2\cdot2+1+1=6). Vị trí tiếp theo có độ ngon giống hệt nhau nên người chơi tỉnh táo ngay lập tức. Công thức tạo ra (6). 

Vì`4 3 2 1`, chu kỳ hàng có (g_n=2) và (p_n=2). Các hàng dựa trên số 0 tốt nhất của nó là (1) và (3), có tổng là (4). Khoảng thời gian hoàn chỉnh là (L=\operatorname{lcm}(2,3)=6), do đó mỗi vị trí hàng xảy ra ba lần và mỗi vị trí cột xảy ra hai lần. Đóng góp của hàng là (3\cdot4\cdot3=36), đóng góp của cột là (3\cdot2=6) và đóng góp không đổi là (6). Câu trả lời cuối cùng là (36+6+6=48). 

Vì`2000000000 2000000000 1 1`, cả hai chiều đều có chu kỳ (2\cdot10^9), do đó quỹ đạo hoàn chỉnh chứa (2\cdot10^9) bánh mì kẹp thịt. Tổng tối đa một chiều là (2000000000\cdot1999999999/2) cho mỗi tọa độ. Việc tính toán được thực hiện bằng phép nhân mô-đun, do đó không cần lặp lại trên quỹ đạo khổng lồ và kết quả cuối cùng là`999998628`.
