---
title: "CF 102503F - Ulam xoắn ốc"
description: "Lưới chứa các số nguyên dương được sắp xếp theo hình xoắn ốc vuông xung quanh 1. Tọa độ được căn giữa ở 1, với tọa độ thứ nhất tăng dần lên trên và tọa độ thứ hai tăng dần về bên phải. Do đó 2 tại (0,1), 3 tại (1,1), 4 tại (1,0), v.v."
date: "2026-08-09T05:40:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "F"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 515
verified: true
draft: false
---

[CF 102503F - Xoắn ốc Ulam](https://codeforces.com/problemset/problem/102503/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới chứa các số nguyên dương sắp xếp theo hình xoắn ốc vuông xung quanh`1`. Tọa độ được tập trung tại`1`, với tọa độ thứ nhất tăng dần lên trên và tọa độ thứ hai tăng dần về bên phải. Như vậy`2`đang ở`(0,1)`,`3`Tại`(1,1)`,`4`Tại`(1,0)`, vân vân. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp hai số ulam`i`Và`j`. Chúng tôi xác định vị trí của cả hai số trong hình xoắn ốc vô hạn, lấy hình chữ nhật thẳng hàng theo trục nhỏ nhất chứa hai ô của chúng và tính tổng các giá trị của mọi ulam bên trong hình chữ nhật đó. Câu trả lời bắt buộc là tổng modulo này`10^9 + 7`. Tuyên bố chính thức xác nhận rằng có thể có tới`20,000`trường hợp thử nghiệm và mỗi số có thể lớn bằng`10^18`. 

Sự ràng buộc của`10^18`ngay lập tức loại trừ việc xây dựng đường xoắn ốc lên đến giá trị đầu vào. Một số xung quanh`10^18`nói dối đại khái`5 * 10^8`các ô cách xa trung tâm, do đó, ngay cả một hình chữ nhật lớn cũng có thể chứa theo thứ tự`10^18`tế bào. Một giải pháp truy cập từng ô riêng lẻ là không khả thi từ xa. Với`20,000`các trường hợp thử nghiệm, giải pháp dự định về cơ bản cần công việc không đổi hoặc logarit cho mỗi trường hợp. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Khi hai đầu vào bằng nhau, hình chữ nhật là một ô. Ví dụ, đầu vào`1 1`có câu trả lời`1`, không`0`hoặc kích thước của một vòng xung quanh. Việc triển khai bất cẩn giả định hai tọa độ riêng biệt có thể mắc sai lầm này. 

Một lỗi phổ biến khác là đếm một góc xoắn ốc hai lần khi tính tổng bốn cạnh của nó. Ví dụ,`13`Và`25`cả hai đều nằm trên cùng một mặt thẳng đứng của vòng bán kính`2`. Hình chữ nhật của chúng chứa`25, 10, 11, 12, 13`, tổng của nó là`71`. Nếu mỗi cạnh của vòng được coi là bao hàm đầy đủ thì các giá trị góc có thể được cộng hai lần. 

Hướng của hệ tọa độ cũng có vấn đề. Ví dụ,`7`Và`9`nằm trên cùng một hàng, có tọa độ`(-1,-1)`Và`(-1,1)`. Hình chữ nhật chứa chính xác`7,8,9`, vậy câu trả lời cho`7 9`là`24`. Đảo ngược ý nghĩa của tọa độ đầu tiên sẽ thay đổi phía nào của đường xoắn ốc đang được xem xét và âm thầm tạo ra tọa độ không chính xác. 

Cuối cùng, các giá trị lớn không được chuyển đổi thông qua căn bậc hai dấu phẩy động. Đối với một đầu vào như`10^18`, một phép tính gần đúng dấu phẩy động có thể rơi vào sai vòng xoắn ốc gần một hình vuông hoàn hảo. Số nguyên Python và`math.isqrt`tránh toàn bộ loại lỗi này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản. Đầu tiên xác định vị trí`i`Và`j`trong đường xoắn ốc. Sau đó xác định hàng và cột tối thiểu và tối đa giữa hai vị trí. Cuối cùng, liệt kê từng ô trong hình chữ nhật đó, đánh giá số ulam của nó và thêm nó vào câu trả lời. 

Lực lượng vũ phu này là chính xác vì hình chữ nhật được yêu cầu là hữu hạn và mỗi ô được truy cập đúng một lần. Vấn đề là kích thước của nó. Với các giá trị gần`10^18`, hai điểm có thể cách nhau một khoảng`10^9`các ô theo mỗi hướng tọa độ, tạo ra một hình chữ nhật chứa khoảng`10^18`tế bào. Do đó, số lượng hoạt động trong trường hợp xấu nhất là theo thứ tự`10^18`đánh giá tế bào chỉ cho một trường hợp thử nghiệm. 

Quan sát hữu ích là đường xoắn ốc có cấu trúc cao. Mỗi ô thuộc về chính xác một vòng vuông, trong đó chỉ số vòng là 

[ 
k=\max(|a|,|b|). 
]

Nhẫn`k`chứa các giá trị từ hình vuông trước cộng một đến`(2k+1)^2`. Quan trọng hơn, mỗi cạnh trong bốn cạnh của nó là một cấp số cộng có giá trị là đa thức bậc hai trong`k`cộng với hàm tuyến tính của vị trí dọc theo cạnh đó. 

Ví dụ, mặt dưới của vòng`k`, Ở đâu`a=-k`, có 

[ 
giá trị = 4k^2+3k+1+b. 
] 

Ba cạnh còn lại có công thức đơn giản tương tự. Điều này thay đổi vấn đề hoàn toàn. Thay vì truy cập từng ô, chúng tôi tính tổng giao điểm của hình chữ nhật được yêu cầu với mỗi cạnh của vòng một cách tượng trưng. Đối với một cạnh cố định, tọa độ thuộc hình chữ nhật được giới hạn bởi các biểu thức có dạng`constant`,`k`, hoặc`-k`. Giữa các điểm liên tiếp nơi các biểu thức này giao nhau, điểm cuối là các hàm affine cố định của`k`. Phần đóng góp của một vành khi đó là đa thức bậc nhiều nhất là ba trong`k`, có thể được tính tổng bằng các công thức chuẩn cho lũy thừa của số nguyên. 

Phương pháp brute-force hoạt động vì nó truy cập vào các ô một cách rõ ràng. Nó thất bại vì có thể có tới hàng triệu tỷ trong số đó. Quan sát thấy rằng mỗi cạnh của vòng là một dãy affine xung quanh một cơ số bậc hai cho phép chúng ta thay thế tất cả các lần thăm đó bằng một số tổng đa thức không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(A)`mỗi trường hợp, ở đâu`A`là diện tích hình chữ nhật |`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`mỗi trường hợp |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng số ulam đầu vào thành tọa độ xoắn ốc của nó. 

Đối với một số`n`, cho phép`k`là số nguyên không âm nhỏ nhất thỏa mãn 

[ 
n\le(2k+1)^2. 
] 

Đây là chiếc nhẫn của nó. Giá trị lớn nhất trên vòng là`(2k+1)^2`, tọa lạc tại`(-k,k)`. Đi vòng quanh vòng từ đó sẽ có các công thức tọa độ được sử dụng sau này. 

Nếu 

[ 
d=(2k+1)^2-n, 
] 

sau đó bốn phần của chiếc nhẫn được lấy từ`d`: 

[ 
\bắt đầu{căn chỉnh} 
d<2k &: (a,b)=(-k,k-d),\ 
2k\le d<4k &: (a,b)=(-k+d-2k,-k),\ 
4k\le d<6k &: (a,b)=(k,-k+d-4k),\ 
6k\le d &: (a,b)=(k-(d-6k),k). 
\end{căn chỉnh} 
] 

Trung tâm`n=1`được xử lý một cách tự nhiên bởi`k=0`. 
2. Lấy hình chữ nhật giới hạn theo tọa độ của hai vị trí. 

Nếu hai vị trí đó là`(a1,b1)`Và`(a2,b2)`, xác định 

[ 
L_a=\min(a_1,a_2),\quad R_a=\max(a_1,a_2), 
] 

và 

[ 
L_b=\min(b_1,b_2),\quad R_b=\max(b_1,b_2). 
] 

Hình chữ nhật được yêu cầu chính xác là 

[ 
L_a\le a\le R_a,\qquad L_b\le b\le R_b. 
] 
3. Biểu thị giá trị trên mỗi cạnh vòng dưới dạng đa thức. 

Chúng tôi chỉ định mỗi góc cho đúng một bên để tránh tính hai lần. Các phạm vi bên và công thức giá trị kết quả là 

[ 
\begin{mảng}{c|c|c} 
Phạm vi Bên & Tọa độ & Giá trị\ 
\hline 
Dưới & a=-k,\ -k\le b\le k & 4k^2+3k+1+b\ 
Còn lại & b=-k,\ -k+1\le a\le k & 4k^2+k+1-a\ 
Top & a=k,\ -k+1\le b\le k & 4k^2-k+1-b\ 
Đúng & b=k,\ -k+1\le a\le k-1 & 4k^2-3k+1+a 
\end{mảng} 
] 

Các quy tắc điểm cuối không đối xứng là có chủ ý. Mặt dưới sở hữu cả hai góc dưới, mặt trái sở hữu góc trên bên trái, mặt trên sở hữu góc trên bên phải và mặt bên phải không sở hữu góc nào. 
4. Đối với mỗi bên, hãy xác định những vòng nào có thể cắt hình chữ nhật được yêu cầu. 

Ví dụ, ở phía dưới chúng ta có`a=-k`. Hình chữ nhật yêu cầu 

[ 
L_a\le-k\le R_a, 
] 

vậy 

[ 
-R_a\le k\le-L_a. 
] 

Bất đẳng thức tương tự cho kết quả đúng`k`phạm vi cho ba mặt còn lại. 
5. Đối với một vòng hợp lệ cố định, hãy giao khoảng tọa độ của cạnh với khoảng tọa độ của hình chữ nhật. 

Mỗi điểm cuối là một hàm affine của`k`. Ví dụ, ở phía dưới, 

[ 
-k\le b\le k 
] 

phải giao nhau với 

[ 
L_b\le b\le R_b. 
] 

Do đó, điểm cuối thực tế là 

[ 
l(k)=\max(L_b,-k), 
\qquad 
r(k)=\min(R_b,k). 
] 

Ở các khía cạnh khác, ý tưởng tương tự cũng được áp dụng, với các khoảng thời gian như`[-k+1,k]`hoặc`[-k+1,k-1]`. 
6. Chia phạm vi`k`bất cứ khi nào hai biểu thức điểm cuối affine giao nhau. 

Mỗi điểm cuối là một trong bốn hàm affine: giới hạn dưới cố định của hình chữ nhật, giới hạn dưới của cạnh, giới hạn trên cố định của hình chữ nhật hoặc giới hạn trên của cạnh. Hai hàm affine chỉ có thể thay đổi thứ tự một lần. Do đó, chúng tôi thu thập tất cả các điểm giao nhau số nguyên, chia`k`phạm vi ở đó và xử lý từng khoảng thời gian kết quả một cách riêng biệt. 

Bên trong một khoảng như vậy, chúng ta biết chính xác biểu thức affine nào là điểm cuối dưới và biểu thức nào là điểm cuối trên. Các hình thức của chúng là 

[ 
l(k)=pk+q,\qquad r(k)=sk+t. 
] 
7. Tính tổng một vế của một`k`khoảng dưới dạng đa thức. 

Giả sử công thức giá trị ở bên là 

[ 
Ak^2+Bk+C+Dx. 
] 

Số ô được chọn là 

[ 
r-l+1, 
] 

đó là tuyến tính trong`k`. Tổng tọa độ của chúng là 

[ 
\frac{r(r+1)-l(l-1)}2, 
] 

đó là bậc hai trong`k`. 

Do đó, tổng đóng góp của bên đó là đa thức bậc nhiều nhất là ba trong`k`. Chúng tôi đánh giá tổng của`1`,`k`,`k^2`, Và`k^3`trong khoảng thời gian bằng cách sử dụng các công thức đóng. 
8. Cộng phần đóng góp của modulo bốn phía`10^9+7`. 

Chỉ có bốn cạnh và mỗi cạnh chỉ tạo ra một số lượng không đổi`k`khoảng thời gian. Do đó, toàn bộ trường hợp thử nghiệm mất thời gian không đổi. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi ô của đường xoắn ốc thuộc về chính xác một trong bốn dãy cạnh thuộc sở hữu của đúng một vòng. Việc chuyển đổi tọa độ đặt mỗi ulam đầu vào trên vòng và cạnh duy nhất của nó, trong khi hình chữ nhật giới hạn chứa chính xác các ô có tọa độ nằm giữa hai cực trị. 

Đối với mỗi bên sở hữu, giao điểm với hình chữ nhật được biểu diễn chính xác bằng các hàm điểm cuối affine trên và dưới của nó. Việc phân chia ở mỗi điểm giao nhau làm cho các chức năng đó có các lựa chọn cố định trong mỗi khoảng thời gian được xử lý. Sau đó, phép tính đa thức sẽ tính tổng mọi ô đã chọn trên các vòng đó chính xác một lần. Vì phạm vi quyền sở hữu của bốn bên phân chia từng vòng mà không trùng nhau nên phần đóng góp của chúng bằng tổng của hình chữ nhật được yêu cầu. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

MOD = 10**9 + 7
INV2 = pow(2, MOD - 2, MOD)
INV6 = pow(6, MOD - 2, MOD)

def coord(n):
    # Smallest k such that n <= (2k + 1)^2.
    k = (isqrt(n - 1) + 1) // 2

    m = (2 * k + 1) ** 2
    d = m - n

    if d < 2 * k:
        # Bottom: a = -k
        return -k, k - d

    if d < 4 * k:
        # Left: b = -k
        d -= 2 * k
        return -k + d, -k

    if d < 6 * k:
        # Top: a = k
        d -= 4 * k
        return k, -k + d

    # Right: b = k
    d -= 6 * k
    return k - d, k

def powers_sum(l, r):
    if l > r:
        return (0, 0, 0, 0)

    n = r - l + 1

    def pref1(x):
        return x * (x + 1) * INV2 % MOD

    def pref2(x):
        return x * (x + 1) * (2 * x + 1) % MOD * INV6 % MOD

    def pref3(x):
        y = x * (x + 1) % MOD * INV2 % MOD
        return y * y % MOD

    return (
        n % MOD,
        (pref1(r) - pref1(l - 1)) % MOD,
        (pref2(r) - pref2(l - 1)) % MOD,
        (pref3(r) - pref3(l - 1)) % MOD,
    )

def add_side(ans, kl, kr, fixed_l, fixed_r,
             lp, lq, rp, rq, A, B, C, D):
    """
    Sum one spiral side.

    The side coordinate interval is
        [lp*k + lq, rp*k + rq]
    and the rectangle coordinate interval is
        [fixed_l, fixed_r].

    Value on the side is
        A*k^2 + B*k + C + D*x.
    """
    kl = max(kl, 0)
    if kl > kr:
        return ans

    # Four affine expressions determine the two endpoints:
    # fixed_l, geometric_l, fixed_r, geometric_r.
    expr = [
        (0, fixed_l),
        (lp, lq),
        (0, fixed_r),
        (rp, rq),
    ]

    cuts = {kl, kr + 1}

    # Within each interval between crossings, the ordering of
    # all endpoint expressions is fixed.
    for i in range(4):
        p1, q1 = expr[i]
        for j in range(i + 1, 4):
            p2, q2 = expr[j]
            den = p1 - p2
            num = q2 - q1

            if den != 0 and num % den == 0:
                x = num // den
                if kl <= x <= kr:
                    cuts.add(x)
                    if x + 1 <= kr:
                        cuts.add(x + 1)

    cuts = sorted(cuts)

    for idx in range(len(cuts) - 1):
        l = cuts[idx]
        r = cuts[idx + 1] - 1

        if l > r:
            continue

        mid = (l + r) // 2

        gl = lp * mid + lq
        gr = rp * mid + rq

        # Choose which affine expression realizes max(fixed_l, geometric_l).
        if fixed_l >= gl:
            Lp, Lq = 0, fixed_l
        else:
            Lp, Lq = lp, lq

        # Choose which affine expression realizes min(fixed_r, geometric_r).
        if fixed_r <= gr:
            Rp, Rq = 0, fixed_r
        else:
            Rp, Rq = rp, rq

        # If the interval is empty at the midpoint, it is empty
        # throughout this segment because all orderings are fixed.
        if Lp * mid + Lq > Rp * mid + Rq:
            continue

        # count = r(k) - l(k) + 1
        count0 = Rq - Lq + 1
        count1 = Rp - Lp

        # Base value polynomial is C + B*k + A*k^2.
        base = [C, B, A]

        # Multiply base by count.
        poly = [0, 0, 0, 0]
        count = [count0, count1]

        for i in range(3):
            for j in range(2):
                poly[i + j] += base[i] * count[j]

        # Sum of coordinates:
        # (r(r+1) - l(l-1)) / 2.
        # For x = p*k + q:
        # x(x+1) = p^2*k^2 + p*(2q+1)*k + q(q+1).
        r2 = Rp * Rp
        r1 = Rp * (2 * Rq + 1)
        r0 = Rq * (Rq + 1)

        l2 = Lp * Lp
        l1 = Lp * (2 * Lq - 1)
        l0 = Lq * (Lq - 1)

        poly[2] += D * (r2 - l2) * INV2
        poly[1] += D * (r1 - l1) * INV2
        poly[0] += D * (r0 - l0) * INV2

        s0, s1, s2, s3 = powers_sum(l, r)

        ans += poly[0] * s0
        ans += poly[1] * s1
        ans += poly[2] * s2
        ans += poly[3] * s3
        ans %= MOD

    return ans

def solve_case(i, j):
    a1, b1 = coord(i)
    a2, b2 = coord(j)

    la = min(a1, a2)
    ra = max(a1, a2)
    lb = min(b1, b2)
    rb = max(b1, b2)

    ans = 0

    # Bottom:
    # a = -k, b in [-k, k]
    # value = 4k^2 + 3k + 1 + b
    ans = add_side(
        ans,
        -ra, -la,
        lb, rb,
        -1, 0, 1, 0,
        4, 3, 1, 1
    )

    # Left:
    # b = -k, a in [-k+1, k]
    # value = 4k^2 + k + 1 - a
    ans = add_side(
        ans,
        -rb, -lb,
        la, ra,
        -1, 1, 1, 0,
        4, 1, 1, -1
    )

    # Top:
    # a = k, b in [-k+1, k]
    # value = 4k^2 - k + 1 - b
    ans = add_side(
        ans,
        la, ra,
        lb, rb,
        -1, 1, 1, 0,
        4, -1, 1, -1
    )

    # Right:
    # b = k, a in [-k+1, k-1]
    # value = 4k^2 - 3k + 1 + a
    ans = add_side(
        ans,
        lb, rb,
        la, ra,
        -1, 1, 1, -1,
        4, -3, 1, 1
    )

    return ans % MOD

def main():
    t = int(input())
    out = []

    for _ in range(t):
        i, j = map(int, input().split())
        out.append(str(solve_case(i, j)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`coord`đầu tiên hàm sẽ tìm vòng chứa số được yêu cầu. biểu thức`(isqrt(n - 1) + 1) // 2`đưa ra vành đúng mà không cần số học dấu phẩy động. Biến`d`đo khoảng cách của số đó với giá trị tối đa ở góc dưới bên phải, xác định trực tiếp cạnh và tọa độ của nó. 

các`powers_sum`hàm cung cấp bốn đại lượng cần thiết cho một đa thức bậc ba: tổng của`1`,`k`,`k^2`, Và`k^3`. Tất cả các phép chia được thực hiện theo modulo`10^9+7`sử dụng nghịch đảo mô-đun. 

Thói quen trung tâm là`add_side`. Bốn biểu thức điểm cuối affine của nó đủ để mô tả mọi giao điểm có thể có giữa một cạnh xoắn ốc và hình chữ nhật. Nó chèn mọi số nguyên vào phân vùng, do đó, bên trong mỗi khoảng kết quả, các biểu thức điểm cuối giống nhau vẫn hoạt động. Điểm giữa chỉ được dùng để xác định các biểu thức tích cực đó chứ không dùng để ước tính câu trả lời. 

Đa thức trong`add_side`đáng được quan tâm đặc biệt. Giá trị của một ô là bậc hai trong`k`, trong khi số lượng ô trên phần được chọn của một cạnh là tuyến tính theo`k`. Sản phẩm của họ là hình khối. Tổng tọa độ đóng góp một thuật ngữ bậc hai khác. Do đó, toàn bộ khoảng có thể rút gọn thành bốn tổng lũy ​​thừa được trả về bởi`powers_sum`. 

Các phạm vi bên cố tình sử dụng các điểm cuối khác nhau. Mặt dưới bao gồm cả hai góc dưới, mặt trái bắt đầu một vị trí sau góc dưới bên trái, mặt trên bắt đầu một vị trí sau góc trên cùng bên trái và mặt phải loại trừ cả hai góc còn lại. Điều này làm cho bốn phạm vi rời rạc và ngăn chặn việc tính hai lần. 

Các số nguyên Python có độ chính xác tùy ý, do đó các tích trung gian vẫn an toàn ngay cả khi các giá trị ban đầu lớn bằng`10^18`và tổng hình chữ nhật lớn hơn nhiều. 

## Ví dụ đã hoạt động 

### Mẫu 1,`2 12`Tọa độ là 

[ 
2=(0,1),\qquad 12=(1,2). 
] 

Do đó, hình chữ nhật giới hạn là 

[ 
0\le a\le1,\qquad1\le b\le2. 
] 

Các tế bào bên trong nó là`2, 11, 3, 12`. 

| Bước |`k`| Tọa độ đã chọn | Giá trị gia tăng | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| Mặt dưới |`1`|`(0,1)`|`2`|`2`| 
| Giao lộ bên phải/trên cùng |`2`|`(0,2)`|`11`|`13`| 
| Mặt trên |`1`|`(1,1)`|`3`|`16`| 
| Bên phải |`2`|`(1,2)`|`12`|`28`| 

Câu trả lời là`28`, phù hợp với mẫu Dấu vết cho thấy hình chữ nhật có thể giao nhau với nhiều vòng khác nhau, nhưng thuật toán không bao giờ lặp lại trên tất cả các vòng giữa chúng. Mỗi phạm vi áp dụng được tính tổng dưới dạng đa thức. 

### Mẫu 1,`9 7`Tọa độ là 

[ 
9=(-1,1),\qquad7=(-1,-1). 
] 

Cả hai giá trị đều nằm trên cùng một hàng, vì vậy hình chữ nhật bao quanh là 

[ 
a=-1,\qquad -1\le b\le1. 
] 

| Bước |`k`| Tọa độ đã chọn | Giá trị gia tăng | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| Mặt dưới |`1`|`(-1,-1)`|`7`|`7`| 
| Mặt dưới |`1`|`(-1,0)`|`8`|`15`| 
| Mặt dưới |`1`|`(-1,1)`|`9`|`24`| 

Câu trả lời là`24`. Trường hợp này thực hiện một hình chữ nhật hẹp có toàn bộ nội dung nằm trên một cạnh xoắn ốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`mỗi trường hợp thử nghiệm | Bốn cạnh, mỗi cạnh chỉ chia thành một số lượng khoảng cấp affine không đổi | 
| Không gian |`O(1)`| Chỉ một số tọa độ, hệ số đa thức và ranh giới khoảng không đổi được lưu trữ | 

Với nhiều nhất`20,000`trường hợp thử nghiệm và đầu vào lên đến`10^18`, giải pháp chỉ thực hiện một lượng nhỏ số học không đổi cho mỗi trường hợp. Nó không bao giờ xây dựng hình xoắn ốc, không bao giờ lặp trên hình chữ nhật và không bao giờ lặp trên tất cả các vòng giữa hai giá trị đầu vào, vì vậy nó vừa khít trong phạm vi`3`thứ hai và`512 MB`giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

# The production solution above can be placed in this function/module.
# For a standalone test file, assume solve_case is already defined.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        t = int(input())
        ans = []
        for _ in range(t):
            i, j = map(int, input().split())
            ans.append(str(solve_case(i, j)))
        return "\n".join(ans)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""\
3
2 12
9 7
7 9
""") == """\
28
24
24
""", "sample 1"

# Minimum input, same cell
assert run("""\
1
1 1
""") == """\
1
""", "single center cell"

# Adjacent cells
assert run("""\
1
1 2
""") == """\
3
""", "adjacent cells"

# Same row, exercises side traversal
assert run("""\
1
7 9
""") == """\
24
""", "same-row boundary case"

# Same column, includes two corners of one ring
assert run("""\
1
13 25
""") == """\
71
""", "same-column ring case"

# Maximum input value, equal endpoints
assert run("""\
1
1000000000000000000 1000000000000000000
""") == """\
49
""", "maximum value and modular reduction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1`|`1`| Đầu vào tối thiểu và hình chữ nhật một ô suy biến | 
|`1 / 1 2`|`3`| Các ô liền kề và bước xoắn ốc đầu tiên | 
|`1 / 7 9`|`24`| Hình chữ nhật cùng hàng và di chuyển ngang phía dưới | 
|`1 / 13 25`|`71`| Quyền sở hữu truyền tải cùng cột và góc vòng | 
|`1 / 10^18 10^18`|`49`| Đầu vào tối đa, tính toán vòng chính xác và số học modulo | 

## Vỏ cạnh 

Trường hợp đầu vào bằng nhau được xử lý trước khi có bất kỳ biến chứng hình học nào có thể phát sinh. Vì`1 1`, cả hai tọa độ đều là`(0,0)`, vậy hình chữ nhật là một ô. Tính toán phía dưới bao gồm vòng`k=0`, công thức của nó cho`1`, trong khi ba phạm vi bên được sở hữu còn lại trống. Đầu ra chính xác là`1`. 

Trường hợp cùng hàng`7 9`có tọa độ`(-1,-1)`Và`(-1,1)`. Chiếc nhẫn có liên quan duy nhất là`k=1`và cạnh dưới đóng góp khoảng`b=-1..1`. Công thức của nó mang lại`7,8,9`, sản xuất`24`. Không có bên nào thêm các ô này vào nên không có sự trùng lặp. 

Trường hợp góc`13 25`có tọa độ`(2,2)`Và`(-2,2)`. Hình chữ nhật là cột đơn`b=2`, với các hàng từ`-2`bởi vì`2`. Các giá trị là`25,10,11,12,13`, tổng hợp thành`71`. Phía dưới sở hữu`25`, bên phải sở hữu`10,11,12`, và phía trên sở hữu`13`. Các quy ước về điểm cuối chính xác là thứ ngăn cản việc tính các góc hai lần. 

Trường hợp giá trị tối đa sử dụng`10^18`cho cả hai đầu vào. Vì hai tọa độ giống hệt nhau nên chỉ cần một ô. Thuật toán định vị số bằng số học căn bậc hai số nguyên và trả về giá trị modulo của nó`10^9+7`. Bởi vì 

[ 
10^{18}=(10^9)^2\equiv(-7)^2\equiv49\pmod{10^9+7}, 
] 

sản lượng dự kiến ​​là`49`. Điều này cũng chứng tỏ tại sao không cần tính toán dấu phẩy động ở bất kỳ đâu trong giải pháp.
