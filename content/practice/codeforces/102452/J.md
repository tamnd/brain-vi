---
title: "CF 102452J - Nhà toán học trẻ"
description: "Với mọi số nguyên dương x, hãy xét các chữ số thập phân của nó. Nếu các chữ số đó là d1, d2, ..., dk, hãy xác định f(x) là tổng của di dj trên mọi cặp vị trí khác nhau với i < j. Chúng ta cần đếm các số nguyên trong [L, R] sao cho x và f(x) có cùng phần dư theo modulo m."
date: "2026-08-10T06:35:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 236
verified: true
draft: false
---

[CF 102452J - Nhà toán học trẻ](https://codeforces.com/problemset/problem/102452/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Với mọi số nguyên dương`x`, hãy nhìn vào các chữ số thập phân của nó. Nếu những chữ số đó là`d1, d2, ..., dk`, định nghĩa`f(x)`như tổng của`di * dj`trên mỗi cặp vị trí khác nhau với`i < j`. Chúng ta cần đếm các số nguyên trong`[L, R]`vì cái gì`x`Và`f(x)`có cùng số dư theo modulo`m`. 

Đầu vào chứa một số trường hợp độc lập. Mỗi trường hợp cho hai số nguyên thập phân có khả năng rất lớn`L`Và`R`, theo sau là`m`. Từ`R`có thể chứa tối đa 5000 chữ số, nó không thể được chuyển đổi thành số nguyên máy bình thường. Câu trả lời bắt buộc là số số nguyên hợp lệ trong khoảng, modulo giảm`10^9 + 7`. 

Giới hạn về tổng số chữ số của tất cả`R`giá trị chỉ là 5000, điều này cho chúng ta biết rằng thuật toán dự kiến ​​phải gần như tuyến tính theo số chữ số, nhân với một đa thức theo`m`. Với`m <= 60`, một không gian trạng thái của`m²`có thể quản lý được, trong khi`m³`đã quá lớn khi nhân với 5000. Đặc biệt, một DP có vị trí 5000 chữ số và`60³`các tiểu bang sẽ có khoảng 1,08 tỷ lượt truy cập cấp tiểu bang trước khi xem xét chuyển đổi chữ số. 

Có một số chi tiết ranh giới có thể âm thầm gây ra những câu trả lời sai. Đầu tiên, khoảng thời gian là bao gồm. Vì`L = R = 10`Và`m = 2`, chúng tôi có`f(10) = 0`Và`10 ≡ 0 (mod 2)`, vậy câu trả lời là`1`. Một giải pháp tính toán`count(R) - count(L)`thay vì`count(R) - count(L-1)`mất số hợp lệ duy nhất. 

Thứ hai, chữ số tự nhiên DP sử dụng các số 0 đứng đầu khi đếm tất cả các số có giới hạn có độ dài cố định. Ví dụ,`10`có thể được biểu diễn dưới dạng`0010`. Những số 0 đứng đầu không thay đổi`f(x)`, vì mọi tích chứa số 0 đều bằng 0, nên coi chúng như một phần của dãy chữ số là an toàn. giá trị`0`bản thân nó cũng có thể được tính bằng tiền tố DP, nhưng nó biến mất khi chúng ta trừ đi`count(L-1)`bởi vì`L >= 10`. 

Thứ ba, một số có hai chữ số có`f(ab) = a*b`, không`a+b`. Ví dụ, đối với`22`,`f(22) = 4`. Với`m = 2`, cả hai`22`Và`4`đều đồng dạng với 0, vì vậy câu trả lời cho`22 22 2`là`1`. Một DP chỉ lưu trữ tổng các chữ số thì không thể phân biệt được tình trạng này. 

Cuối cùng, giới hạn trên có thể có hàng nghìn chữ số. Ví dụ,`10`theo sau là 4998 số 0 là giá trị hợp pháp có 4999 chữ số. Chúng ta phải xử lý nó dưới dạng một chuỗi và thực hiện mọi phép toán số học theo modulo`m`; việc chuyển đổi nó thành một số nguyên thông thường là không cần thiết và cũng không mong muốn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi số nguyên`x`từ`L`bởi vì`R`, trích xuất các chữ số của nó, tính toán`f(x)`, và kiểm tra sự phù hợp. Máy tính`f(x)`trực tiếp từ tất cả các cặp chữ số`Θ(k²)`cho một`k`-số có chữ số. Ngay cả khi chúng ta duy trì tổng cặp tăng dần, việc kiểm tra mọi số nguyên vẫn tốn kém`Θ(R-L+1)`hoạt động. Trong trường hợp xấu nhất khoảng chứa gần như`10^5000`số và mỗi số có thể có khoảng 5000 chữ số. Do đó, việc triển khai dựa trên cặp trực tiếp sẽ thực hiện theo thứ tự`10^5000 * 5000²`, điều đó hoàn toàn không thể thực hiện được. 

Bước tiếp theo tự nhiên là chữ số DP. Trong khi xây dựng số từ trái sang phải, giả sử các chữ số đã chọn có tổng`s`và đóng góp của họ cho`f`là`g`. Nếu chữ số tiếp theo là`d`, thì mọi sản phẩm giữa`d`và một chữ số trước đó góp phần vào`f`, vậy đóng góp mới là`g + d*s`. Đồng thời, chữ số mới thay đổi giá trị số của`x`qua`d * 10^p`, Ở đâu`p`là giá trị vị trí của nó. 

Một DP đơn giản sẽ lưu trữ giá trị hiện tại của`x`modulo`m`, giá trị hiện tại của`f(x)`modulo`m`, và tổng các chữ số modulo`m`. Điều đó mang lại`m³`tiểu bang cho mỗi vị trí. Với`m = 60`và 5000 chữ số, điều này là quá nhiều. 

Quan sát hữu ích là chúng ta không bao giờ cần hai giá trị`x mod m`Và`f(x) mod m`một cách độc lập. Điều kiện cuối cùng chính xác là`f(x) - x ≡ 0 (mod m)`. Vì vậy, chúng tôi có thể lưu trữ sự khác biệt của họ một cách trực tiếp. 

Cho phép`q = f(prefix) - value(prefix) (mod m)`, và để`s`là tổng chữ số của tiền tố modulo`m`. Khi chúng tôi nối thêm chữ số`d`tại giá trị vị trí`p`, sự đóng góp của cặp mới vào`f`là`d*s`, trong khi sự đóng góp mới cho`x`là`d*p`. Kể từ đây`q' = q + d*s - d*p (mod m)`. 

Tổng chữ số mới chỉ đơn giản là`s' = s + d (mod m)`. 

Điều đó chỉ để lại hai chiều trạng thái mô-đun,`s`Và`q`. Bài xã luận chính thức mô tả tương tự`O(|R|m²)`giảm thiểu nhà nước. 

Chúng ta có thể đếm các số hợp lệ đến giới hạn`N`, sau đó sử dụng`answer = count(R) - count(L-1)`. 

Cờ DP chữ số chặt chẽ tiêu chuẩn xử lý giới hạn trên. Việc triển khai bên dưới giữ tất cả các tiền tố vốn đã nhỏ hơn trong một mảng DP và giữ tiền tố duy nhất bằng với giới hạn riêng biệt. Điều này sẽ loại bỏ một chiều khỏi bộ nhớ thực tế và giúp quá trình chuyển đổi trở nên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Θ((R-L+1) · k²)`|`O(k)`| Quá chậm | 
| Chữ số ngây thơ DP |`O(k · m³ · 10)`|`O(m³)`| Quá chậm | 
| Chữ số tối ưu DP |`O(k · m² · 10)`|`O(m²)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`count(N)`là số số nguyên hợp lệ từ`0`bởi vì`N`. Khoảng thời gian được yêu cầu sau đó có thể được lấy dưới dạng`count(R) - count(L-1)`. Đếm từ số 0 rất thuận tiện vì chữ số DP đương nhiên cho phép các số 0 đứng đầu. 
2. Xử lý các chữ số của`N`từ trái sang phải. Đối với tiền tố, hãy lưu trữ`s`, tổng các chữ số của nó modulo`m`, Và`q`, giá trị`f(prefix) - prefix`modulo`m`. Hai giá trị này chứa chính xác thông tin cần thiết để cập nhật trạng thái khi một chữ số khác được thêm vào. 
3. Giả sử chữ số tiếp theo là`d`và giá trị vị trí thập phân của nó là`p = 10^remaining (mod m)`. Chữ số đóng góp`d*s`ĐẾN`f`, bởi vì nó được ghép với mọi chữ số trước đó có tổng bằng`s`. Nó góp phần`d*p`tới trị số. Như vậy sự khác biệt thay đổi theo`q' = q + d*s - d*p (mod m)`. 
4. Tổng các chữ số thay đổi độc lập như`s' = s + d (mod m)`. Đối với mọi trạng thái, hãy thử tất cả mười chữ số có thể. Tiền tố đã nhỏ hơn`N`có thể sử dụng tất cả mười chữ số, trong khi tiền tố duy nhất bằng`N`chỉ có thể sử dụng các chữ số cho đến chữ số giới hạn tương ứng. 
5. Giữ tiền tố chặt chẽ riêng biệt. Nếu chữ số tiếp theo của nó nhỏ hơn chữ số tương ứng của`N`, trạng thái mới của nó sẽ được chèn vào DP không hạn chế. Nếu chữ số được chọn bằng nhau thì tiền tố vẫn chặt chẽ. Cuối cùng có đúng một đường dẫn chặt chẽ, biểu thị`N`chính nó. 
6. Sau khi tất cả các chữ số đã được xử lý, mọi trạng thái hợp lệ phải có`q = 0`. Tính tổng các trạng thái đó trên mọi tổng chữ số có thể`s`, cả ở trạng thái DP không hạn chế và, nếu có, ở trạng thái chặt chẽ. 
7. Tính toán`count(R) - count(L-1)`modulo`10^9+7`. Từ`L`Và`R`là chuỗi, giảm`L`sử dụng số học chuỗi thập phân thay vì chuyển đổi nó thành số nguyên. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái DP`(s, q)`đại diện chính xác các tiền tố với tổng chữ số`s`và sự khác biệt`q = f(prefix) - prefix`modulo`m`. Khi một chữ số`d`được thêm vào, các điều khoản mới duy nhất trong`f`là các sản phẩm của nó có chữ số trước đó, có tổng là`d*s`. Số lượng tăng tương ứng là`d*p`. Do đó quá trình chuyển đổi tính toán giá trị mới chính xác của`f - x`modulo`m`. 

Ở chữ số cuối cùng,`q = 0`tương đương với`f(x) ≡ x (mod m)`. Quá trình chuyển đổi chặt chẽ xem xét chính xác các chuỗi chữ số không lớn hơn`N`, trong khi các trạng thái không hạn chế chứa chính xác các tiền tố đã trở nên nhỏ hơn. Như vậy`count(N)`đếm mọi số nguyên hợp lệ từ 0 đến`N`đúng một lần. Trừ`count(L-1)`để lại chính xác các số nguyên hợp lệ trong`[L, R]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def decrement_decimal(s):
    """Return s - 1 as a decimal string. s is positive."""
    a = list(s)
    i = len(a) - 1

    while a[i] == '0':
        a[i] = '9'
        i -= 1

    a[i] = chr(ord(a[i]) - 1)

    res = ''.join(a).lstrip('0')
    return res if res else '0'

def count_upto(bound, m):
    """Count x in [0, bound] with f(x) == x (mod m)."""
    if bound == '-1':
        return 0

    digits = [ord(c) - 48 for c in bound]
    n = len(digits)

    states = m * m

    # dp[s * m + q]:
    # number of prefixes already strictly smaller than bound,
    # with digit sum s and f(prefix)-prefix == q (mod m).
    dp = [0] * states
    dp[0] = 1

    # The unique prefix equal to the bound so far.
    tight_s = 0
    tight_q = 0

    # 10^i mod m, where i is the number of positions to the right.
    pow10 = [1] * n
    for i in range(1, n):
        pow10[i] = pow10[i - 1] * 10 % m

    for pos in range(n):
        place = pow10[n - pos - 1]
        limit = digits[pos]

        ndp = [0] * states

        # Precompute the transition for each digit for every digit sum.
        # For a state (s, q):
        #   s' = s + d
        #   q' = q + d * (s - place)
        moves = []
        for s in range(m):
            row = []
            for d in range(10):
                ns = s + d
                if ns >= m:
                    ns -= m
                delta = (d * (s - place)) % m
                row.append((ns, delta))
            moves.append(row)

        # Extend prefixes which are already smaller than bound.
        for s in range(m):
            base = s * m
            row_moves = moves[s]

            for q in range(m):
                v = dp[base + q]
                if v == 0:
                    continue

                for d in range(10):
                    ns, delta = row_moves[d]

                    nq = q + delta
                    if nq >= m:
                        nq -= m

                    idx = ns * m + nq
                    nv = ndp[idx] + v
                    if nv >= MOD:
                        nv -= MOD
                    ndp[idx] = nv

        # Extend the unique tight prefix.
        # Digits smaller than limit become unrestricted.
        for d in range(limit):
            ns = tight_s + d
            if ns >= m:
                ns -= m

            nq = tight_q + d * (tight_s - place)
            nq %= m

            idx = ns * m + nq
            nv = ndp[idx] + 1
            if nv >= MOD:
                nv -= MOD
            ndp[idx] = nv

        # Choosing the bound digit keeps the prefix tight.
        tight_q += limit * (tight_s - place)
        tight_q %= m

        tight_s += limit
        if tight_s >= m:
            tight_s -= m

        dp = ndp

    ans = 1 if tight_q == 0 else 0

    # q == 0, any digit sum is acceptable.
    for s in range(m):
        ans += dp[s * m]
        if ans >= MOD:
            ans -= MOD

    return ans

def solve_case(L, R, m):
    left = decrement_decimal(L)
    return (count_upto(R, m) - count_upto(left, m)) % MOD

def main():
    T = int(input())

    out = []
    for _ in range(T):
        L = input().strip()
        R = input().strip()
        m = int(input())

        out.append(str(solve_case(L, R, m)))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    main()
```Mảng trạng thái được làm phẳng thành một chiều bằng cách sử dụng`s * m + q`. Điều này tránh các danh sách Python lồng nhau trong quá trình chuyển đổi trong cùng và làm cho vòng lặp nóng rẻ hơn. Mảng chỉ có`m²`mục, vì vậy ở mức tối đa`m = 60`nó chứa 3600 trạng thái. 

các`place`giá trị luôn giảm modulo`m`. Giá trị vị trí thực tế có thể có hàng nghìn chữ số thập phân, nhưng chỉ có phần dư của nó theo modulo`m`ảnh hưởng đến sự tái phát. các`pow10`mảng lưu trữ những phần còn lại này từ vị trí ngoài cùng bên phải về phía bên trái. 

Việc chuyển đổi sử dụng`q + d * (s - place)`. Phép nhân được thực hiện bằng cách sử dụng tổng chữ số tiền tố hiện tại`s`, không phải tổng mới`s + d`. Chữ số mới chỉ được ghép với các chữ số đã được đặt ở bên trái của nó, đó chính xác là chữ số cũ`s`đại diện. 

Số lượng được lưu trữ trong mỗi ô DP luôn được giảm theo modulo`10^9+7`. Vì cả ô cũ và giá trị được thêm vào đều nằm dưới mô đun nên một phép trừ có điều kiện là đủ sau mỗi lần cộng. Điều này tránh được một chi phí tương đối đắt`% MOD`bên trong vòng lặp trong cùng. 

Bản thân giới hạn được thể hiện bằng một trạng thái chặt chẽ, thay vì một chiều bổ sung của DP. Mọi chuyển đổi sử dụng chữ số nhỏ hơn sẽ được chèn vào`ndp`, trong khi quá trình chuyển đổi sử dụng chính xác chữ số bị ràng buộc sẽ trở thành trạng thái chặt chẽ mới. Điều này tương đương với cờ chặt thông thường nhưng tiết kiệm bộ nhớ và một số chi phí vòng lặp. 

Số 0 đứng đầu được cho phép một cách có chủ ý. Ví dụ: khi đếm các số đến`999`, giá trị`23`được biểu diễn dưới dạng`023`. Số 0 đứng đầu không đóng góp gì cho`f`, không đóng góp gì vào tổng chữ số và không đóng góp gì vào giá trị số, vì vậy trạng thái của`023`giống hệt với trạng thái thu được từ biểu diễn hai chữ số thực tế`23`. 

Sự giảm của`L`được thực hiện trước khi gọi`count_upto`. Đây là cách rõ ràng nhất để xử lý khoảng bao gồm và nó cũng tránh được các trường hợp đặc biệt trong chính chữ số DP. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên là```
2
10
50
17
33
33
3
```Vì`m = 17`, xét số có hai chữ số`ab`. Giá trị của nó là`10a+b`, trong khi`f(x)=ab`. Hai số hợp lệ trong`[10,50]`là`23`Và`42`. 

Vì`23`, DP bắt đầu từ`(s,q)=(0,0)`. Sau khi chọn chữ số đầu tiên`2`, giá trị vị trí của nó là`10`, Vì thế`s=2`Và`q=0+2*0-2*10=-20 ≡ 14 (mod 17)`. Sau khi chọn`3`, giá trị vị trí còn lại là`1`, Vì thế`q=14+3*2-3*1=17 ≡ 0`. Trạng thái cuối cùng là hợp lệ. 

Vì`42`, các trạng thái tương ứng là: 

| Vị trí | Chữ số | Đặt modulo 17 | Tổng chữ số`s`| Sự khác biệt`q`| 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | 0 | 0 | 
| 1 | 4 | 10 | 4 | 11 | 
| 2 | 2 | 1 | 6 | 0 | 

Số lượng không hạn chế lên đến`50`cũng chứa`0`, bởi vì`f(0)=0`. Như vậy`count(50)=3`, đại diện`0`,`23`, Và`42`. Lên đến`9`, chỉ một`0`là hợp lệ, vì vậy`count(9)=1`. Kết quả cuối cùng là`3-1=2`, phù hợp với đầu ra mẫu. 

Mẫu thứ hai là`L=R=33`,`m=3`. Vì`33`, chữ số đầu tiên cho biết`s=0`Và`q=0`, bởi vì`3*0-3*10 ≡ 0 (mod 3)`. Chữ số thứ hai cũng rời đi`q=0`, từ`3*0-3*1 ≡ 0 (mod 3)`. 

| Vị trí | Chữ số | Đặt modulo 3 | Tổng chữ số`s`| Sự khác biệt`q`| 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | 0 | 0 | 
| 1 | 3 | 1 | 0 | 0 | 
| 2 | 3 | 1 | 0 | 0 | 

Như vậy`33`là hợp lệ. Phép trừ của`count(32)`từ`count(33)`cô lập chính xác giá trị biên này, tạo ra`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(D · m² · 10)`| Mỗi trong số`D`lượt truy cập vị trí chữ số`m²`tiểu bang và thử mười chữ số | 
| Không gian |`O(m² + D)`| Hai`m²`Mảng DP có kích thước không cần thiết vì mảng cũ được thay thế bằng mảng mới; bảng điện sử dụng`O(D)`không gian | 

Đây`D`là số chữ số của giới hạn. Hai người kêu gọi`R`Và`L-1`chỉ thêm một yếu tố không đổi. Vì tổng số chữ số của tất cả`R`giá trị tối đa là 5000 và`m <= 60`, số lượng trạng thái DP trên mỗi chữ số nhiều nhất là`3600`, với mười lần chuyển đổi có thể có cho mỗi trạng thái. Việc sử dụng bộ nhớ vẫn nhỏ vì chỉ có hiện tại và tiếp theo`m²`các trạng thái được lưu trữ. 

## Trường hợp thử nghiệm 

Mẫu được cung cấp và các trường hợp tùy chỉnh bên dưới thực hiện các ranh giới khoảng, các chữ số hoàn toàn bằng nhau, giá trị nhỏ nhất được phép và số thập phân có độ dài gần như tối đa.```python
# Save the submitted solution as solution.py before running this test file.

import io
import sys
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution.main()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
sample = """\
2
10
50
17
33
33
3
"""
assert run(sample) == "2\n1", "provided samples"

# Minimum-size input.
# f(10) = 0 and 10 is divisible by 2.
assert run("""\
1
10
10
2
""") == "1", "minimum value"

# All digits equal.
# f(22) = 2 * 2 = 4, and both 22 and 4 are 0 modulo 2.
assert run("""\
1
22
22
2
""") == "1", "all-equal digits"

# Boundary case with no valid number.
# f(10)=0, 10 mod 3 = 1.
# f(11)=1, 11 mod 3 = 2.
assert run("""\
1
10
11
3
""") == "0", "boundary with no valid values"

# Inclusive interval and multiple consecutive valid values.
# For m=2:
# 10 -> f=0, valid
# 11 -> f=1, valid
# 12 -> f=2, valid
assert run("""\
1
10
12
2
""") == "3", "inclusive endpoints"

# Maximum-length style test.
# 1 followed by 4998 zeroes is a legal 4999-digit value.
# Its f(x) is 0, and the number is even, so it is valid for m=2.
huge = "1" + "0" * 4998
assert run(f"""\
1
{huge}
{huge}
2
""") == "1", "large decimal bound"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 10 2`|`1`| Giá trị cho phép nhỏ nhất và`L = R`| 
|`22 22 2`|`1`| Tất cả các chữ số bằng nhau và tính toán tích cặp | 
|`10 11 3`|`0`| Một phạm vi mà cả hai điểm cuối đều không hợp lệ | 
|`10 12 2`|`3`| Xử lý khoảng thời gian bao gồm và các giá trị hợp lệ liên tiếp | 
| 4999 chữ số`10...0`với`m=2`|`1`| Chuỗi thập phân rất lớn và modulo số học giá trị vị trí`m`| 

## Vỏ cạnh 

Đối với trường hợp ranh giới bao gồm`10 10 2`, thuật toán tính toán`count(10)`Và`count(9)`. số`10`đạt đến trạng thái cuối cùng`q=0`, bởi vì`f(10)-10 = -10 ≡ 0 (mod 2)`. số`0`cũng được tính theo cả hai giới hạn, do đó nó bị hủy trong quá trình trừ. Kết quả là chính xác`1`. 

Đối với ví dụ hoàn toàn bằng nhau`22 22 2`, hai chữ số tạo thành một cặp, vì vậy`f(22)=4`. DP lần đầu tiên đọc`2`, sau đó đọc thứ hai`2`. Sau chữ số đầu tiên, sự khác biệt là`-20 ≡ 0 (mod 2)`. Sau chữ số thứ hai, số tiền đóng góp bổ sung là`2*2 - 2 = 2`, do đó hiệu vẫn bằng 0 modulo hai. Câu trả lời cuối cùng là`1`. 

Đối với trường hợp số 0 đứng đầu, hãy xem xét`23`trong khi đếm đến`50`. DP xử lý nó như`23`, nhưng nếu giới hạn có nhiều chữ số hơn thì nó có thể xử lý cùng một giá trị giống như`0023`. Mỗi số 0 đứng đầu được chèn vào có`d=0`, do đó nó không thay đổi tổng các chữ số cũng như`f-x`. Việc biểu diễn vẫn đạt đến trạng thái cuối cùng giống nhau, có nghĩa là chữ số DP có độ dài cố định sẽ đếm số một cách chính xác. 

Đối với ranh giới trên chính xác`33 33 3`, con đường chật hẹp phải chọn`3`ở cả hai vị trí. Sau chữ số đầu tiên, tổng các chữ số là`0 mod 3`và sự khác biệt cũng là`0 mod 3`. Sau chữ số thứ hai, sự khác biệt vẫn bằng không. Vì đường dẫn không bao giờ nhỏ hơn giới hạn nên nó vẫn ở trạng thái chặt chẽ riêng biệt và được đưa vào cuối. Trừ`count(32)`để lại đúng một số. 

Đối với trường hợp có chiều dài tối đa bao gồm`1`theo sau là 4998 số 0, mỗi cặp chữ số chứa ít nhất một số 0, vì vậy`f(x)=0`. Bản thân số này là lũy thừa của mười và chia hết cho hai, nên nó có giá trị`m=2`. Thuật toán không bao giờ xây dựng số nguyên khổng lồ. Nó chỉ xử lý 4999 chữ số và lưu trữ mỗi giá trị vị trí theo modulo hai, do đó kích thước của giá trị số không ảnh hưởng đến biểu diễn trạng thái.
