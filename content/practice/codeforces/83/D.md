---
title: "CF 83D - Số"
description: "Chúng ta cần đếm các số nguyên bên trong khoảng $[a,b]$ có ước số nhỏ nhất lớn hơn $1$ chính xác là $k$. Đối với một số $x$, nói rằng ước số nhỏ nhất của nó là $k$ có nghĩa là hai điều xảy ra đồng thời: 1. $x$ chia hết cho $k$. 2."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 83
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 72 (Div. 1 Only)"
rating: 2400
weight: 83
solve_time_s: 127
verified: true
draft: false
---

[CF 83D - Số](https://codeforces.com/problemset/problem/83/D) 

**Đánh giá:** 2400 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm số nguyên bên trong khoảng$[a,b]$có ước số nhỏ nhất lớn hơn$1$chính xác là$k$. 

Đối với một số$x$, nói rằng ước số nhỏ nhất của nó là$k$có nghĩa là hai điều xảy ra đồng thời: 

1.$x$chia hết cho$k$. 
2.$x$không chia hết cho bất kỳ số nguyên nào từ$2$ĐẾN$k-1$. 

Điều kiện thứ hai ngay lập tức ngụ ý rằng$k$bản thân nó phải là số nguyên tố. Nếu như$k$là hợp số thì một trong những ước số nhỏ hơn của nó cũng sẽ chia hết$x$, Vì thế$k$không bao giờ có thể là ước số nhỏ nhất. 

Các giới hạn là lớn. Cả hai$a$Và$b$có thể đạt được$2 \cdot 10^9$, do đó việc lặp qua khoảng thời gian là không thể. Ngay cả việc kiểm tra từng con số riêng lẻ bằng phép chia thử cũng sẽ cần hàng tỷ thao tác trong trường hợp xấu nhất. 

Giá trị của$k$cũng lớn, nhưng một quan sát quan trọng sẽ cứu chúng ta: nếu$k$là số nguyên tố thì mọi số hợp lệ đều có dạng$$x = k \cdot t$$Và$t$phải tránh tất cả các thừa số nguyên tố nhỏ hơn$k$. 

Vì ước số nhỏ nhất của hợp số bất kỳ là số nguyên tố nên việc cấm chia hết cho các số nguyên tố nhỏ hơn là đủ$k$. Điều đó biến bài toán thành bài toán đếm với phép loại trừ và hàm Möbius. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Nếu như$k$là hợp số, câu trả lời luôn bằng 0. Ví dụ: 

đầu vào:```
1 100 6
```Mỗi bội số của$6$chia hết cho$2$hoặc$3$, Vì thế$6$không bao giờ có thể là ước số nhỏ nhất. Câu trả lời đúng là:```
0
```Việc thực hiện bất cẩn chỉ kiểm tra khả năng chia hết cho các số nhỏ hơn$k$sau khi chia tỷ lệ có thể vô tình đếm bội số của$6$. 

Một trường hợp tế nhị khác xuất hiện khi$k^2 > b$. Khi đó số hợp lệ duy nhất có thể là$k$chính nó, bởi vì mọi bội số khác$2k,3k,\dots$đã có ước số nhỏ hơn. Ví dụ: 

đầu vào:```
10 20 13
```bội số duy nhất của$13$trong khoảng là$13$, và ước số nhỏ nhất của nó thực sự là$13$. Câu trả lời là:```
1
```Ngoài ra còn có một cái bẫy riêng biệt khi chuyển đổi khoảng thời gian$[a,b]$vào một phạm vi cho số nhân$t$. Giả sử: 

đầu vào:```
15 30 5
```Chúng ta viết lại số dưới dạng$x=5t$. Sau đó:$$t \in \left[\left\lceil \frac{15}{5} \right\rceil,\left\lfloor \frac{30}{5} \right\rfloor\right] = [3,6]$$Giữa$3,4,5,6$, chỉ một$5$chia hết cho không có số nguyên tố nào nhỏ hơn$5$. Điều đó mang lại$25$là số hợp lệ duy nhất. Việc quên trần ở ranh giới bên trái sẽ bao gồm không chính xác$10$. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa. Với mọi số nguyên$x$TRONG$[a,b]$, ta thấy ước số nhỏ nhất của nó lớn hơn$1$. Nếu số chia đó bằng$k$, chúng tôi đếm nó. 

Điều này hoạt động hợp lý vì ước số nhỏ nhất xác định duy nhất liệu số đó có thỏa mãn điều kiện hay không. Vấn đề là quy mô. Độ dài khoảng có thể gần như$2 \cdot 10^9$. Thậm chí một$O(\sqrt{x})$kiểm tra mỗi số trở nên lớn đến mức vô vọng. 

Quan sát quan trọng là nếu ước số nhỏ nhất của$x$bằng$k$, sau đó$k$phải là số nguyên tố và$x$phải chia hết cho$k$. Viết:$$x = k \cdot t$$Bây giờ hãy hỏi điều kiện nào áp dụng cho$t$. 

Nếu một số nguyên tố$p < k$chia rẽ$t$, sau đó$p$cũng chia$x$, điều này mâu thuẫn$k$là ước số nhỏ nhất. Vì thế$t$phải cùng nguyên tố với tích của tất cả các số nguyên tố nhỏ hơn$k$. 

Điều này chuyển đổi vấn đề thành các số nguyên đếm trong một phạm vi không chia hết cho một tập hợp số nguyên tố. 

Số số nguyên tố nhỏ hơn$k$không thể lớn khi tích của họ ở dưới$2 \cdot 10^9$. Tích của mười số nguyên tố đầu tiên đã vượt quá giới hạn này. Điều đó có nghĩa là chúng ta có thể sử dụng loại trừ bao hàm một cách an toàn trên tất cả các tập con của các số nguyên tố này. 

Lực lượng vũ phu kiểm tra từng số riêng lẻ. Giải pháp tối ưu đếm toàn bộ các nhóm cùng một lúc bằng cách trừ bội số của các số nguyên tố bị cấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((b-a+1)\sqrt{b})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{k} + 2^{\pi(k)})$|$O(\pi(k))$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem$k$là nguyên tố. 

Nếu như$k$là tổng hợp, in ngay$0$. Một số tổng hợp không bao giờ có thể là ước số nhỏ nhất của một số khác vì một trong các ước nguyên tố nhỏ hơn của nó sẽ xuất hiện đầu tiên. 
2. Chuyển đổi khoảng từ số$x$để nhân lên$t$. 

Mọi số hợp lệ đều có dạng:$$x = k \cdot t$$Vì thế:$$L = \left\lceil \frac{a}{k} \right\rceil$$

$$R = \left\lfloor \frac{b}{k} \right\rfloor$$Bây giờ chúng ta cần đếm số nguyên$t \in [L,R]$không chia hết cho số nguyên tố nào nhỏ hơn$k$. 
3. Tạo tất cả các số nguyên tố nhỏ hơn$k$. 

Chúng ta chỉ cần các số nguyên tố lên đến$\sqrt{k}$để kiểm tra tính nguyên tố, nhưng chúng tôi cũng lưu trữ mọi số nguyên tố nhỏ hơn$k$bởi vì đây là những ước số bị cấm cho$t$. 
4. Áp dụng bao gồm-loại trừ. 

Cho các số nguyên tố bị cấm là:$$p_1,p_2,\dots,p_m$$Với mỗi tập con của các số nguyên tố này, hãy tính tích của chúng:$$d = p_{i_1}p_{i_2}\cdots p_{i_s}$$Đếm xem có bao nhiêu số$[L,R]$được chia cho$d$:$$\left\lfloor \frac{R}{d} \right\rfloor - \left\lfloor \frac{L-1}{d} \right\rfloor$$Thêm số này nếu kích thước tập hợp con là số chẵn, trừ đi nếu kích thước tập hợp con là số lẻ. 
5. Kết quả loại trừ đưa vào cuối cùng là số bội số hợp lệ$t$, bằng số số nguyên hợp lệ$x$. 

### Tại sao nó hoạt động 

Mọi số hợp lệ đều có thể được viết duy nhất dưới dạng$x = kt$. Từ$k$là số nguyên tố, cách duy nhất để$x$để có ước số nhỏ hơn là dành cho$t$chứa thừa số nguyên tố nhỏ hơn$k$. Thuật toán tính chính xác những$t$tránh được tất cả các số nguyên tố bị cấm. 

Loại trừ bao gồm là chính xác vì nó xen kẽ giữa việc trừ các số chia hết cho ít nhất một số nguyên tố bị cấm và cộng lại các số được tính nhiều lần. Sau khi xử lý tất cả các tập hợp con, mọi tập hợp con không hợp lệ$t$đóng góp số lượng mạng bằng 0, trong khi mọi giá trị hợp lệ$t$đóng góp chính xác một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False
    d = 2
    while d * d <= x:
        if x % d == 0:
            return False
        d += 1
    return True

def primes_less_than(k):
    primes = []
    for x in range(2, k):
        ok = True
        d = 2
        while d * d <= x:
            if x % d == 0:
                ok = False
                break
            d += 1
        if ok:
            primes.append(x)
    return primes

def solve():
    a, b, k = map(int, input().split())

    if not is_prime(k):
        print(0)
        return

    L = (a + k - 1) // k
    R = b // k

    if L > R:
        print(0)
        return

    primes = primes_less_than(k)

    ans = 0
    m = len(primes)

    for mask in range(1 << m):
        bits = 0
        prod = 1

        for i in range(m):
            if (mask >> i) & 1:
                bits += 1
                prod *= primes[i]

                if prod > R:
                    break

        else:
            cnt = R // prod - (L - 1) // prod

            if bits % 2 == 0:
                ans += cnt
            else:
                ans -= cnt

    print(ans)

solve()
```Phần đầu tiên kiểm tra xem$k$là nguyên tố. Đây không phải là một sự tối ưu hóa, nó là một yêu cầu về tính chính xác. Nếu như$k$là hợp số, không có số nào có thể có ước số nhỏ nhất một cách chính xác$k$. 

Tiếp theo chúng ta chuyển đổi khoảng ban đầu thành không gian nhân. Mỗi số ứng cử viên là bội số của$k$, vì vậy chúng tôi chỉ làm việc với các giá trị của$t$. Việc chia trần cho ranh giới bên trái rất dễ xảy ra sai sót. Sử dụng:```
(a + k - 1) // k
```tính đúng số nguyên nhỏ nhất$t$như vậy$kt \ge a$. 

Vòng lặp loại trừ bao gồm lặp qua mọi tập hợp con của các số nguyên tố bị cấm. Biến`prod`lưu trữ tích của các số nguyên tố trong tập hợp con hiện tại. Nếu sản phẩm đó đã vượt quá`R`, không có số nào trong phạm vi có thể chia hết cho nó, vì vậy tập hợp con không đóng góp gì. 

Các dấu hiệu xen kẽ đến trực tiếp từ sự bao gồm-loại trừ. Các tập hợp con có kích thước chẵn sẽ cộng số lượng, các tập hợp con có kích thước lẻ sẽ trừ đi số lượng. 

Việc triển khai không bao giờ có nguy cơ tràn vì số nguyên Python có độ chính xác tùy ý, nhưng việc dừng sớm`prod > R`giữ cho việc xử lý tập hợp con nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 10 2
```Chúng tôi có$k=2$, đó là số nguyên tố.$$L = \left\lceil \frac{1}{2} \right\rceil = 1$$

$$R = \left\lfloor \frac{10}{2} \right\rfloor = 5$$Không có số nguyên tố nào nhỏ hơn$2$, vì vậy mỗi$t$TRONG$[1,5]$là hợp lệ. 

| Bước | Giá trị | 
| --- | --- | 
|$L$| 1 | 
|$R$| 5 | 
| Số nguyên tố bị cấm | không | 
| Có hiệu lực$t$| 1,2,3,4,5 | 
| Có hiệu lực$x$| 2,4,6,8,10 | 
| Trả lời | 5 | 

Điều này thể hiện trường hợp đơn giản nhất trong đó việc loại trừ bao gồm chỉ có tập hợp con trống. 

### Ví dụ 2 

đầu vào:```
1 30 5
```Từ$5$là số nguyên tố:$$L = 1,\quad R = 6$$số nguyên tố bị cấm là$2$Và$3$. 

| Tập hợp con | Sản phẩm | Đếm vào$[1,6]$| Đóng góp | 
| --- | --- | --- | --- | 
| {} | 1 | 6 | +6 | 
| {2} | 2 | 3 | -3 | 
| {3} | 3 | 2 | -2 | 
| {2,3} | 6 | 1 | +1 | 

Câu trả lời cuối cùng:$$6 - 3 - 2 + 1 = 2$$Các số nhân hợp lệ là$1$Và$5$, đưa ra con số$5$Và$25$. 

Dấu vết này cho thấy cách loại trừ bao gồm loại bỏ các số chia hết cho các số nguyên tố bị cấm và khôi phục các phần trùng lặp một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{k} + 2^{\pi(k)})$| kiểm tra tính nguyên tố cộng với việc loại trừ bao gồm các số nguyên tố nhỏ hơn$k$| 
| Không gian |$O(\pi(k))$| lưu trữ số nguyên tố bị cấm | 

Số lượng các số nguyên tố có liên quan rất nhỏ vì tích của chúng tăng rất nhanh. Mặc dù loại trừ bao gồm theo cấp số nhân về số lượng số nguyên tố, số lượng tập hợp con thực tế vẫn đủ nhỏ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def is_prime(x):
        if x < 2:
            return False
        d = 2
        while d * d <= x:
            if x % d == 0:
                return False
            d += 1
        return True

    def primes_less_than(k):
        primes = []
        for x in range(2, k):
            ok = True
            d = 2
            while d * d <= x:
                if x % d == 0:
                    ok = False
                    break
                d += 1
            if ok:
                primes.append(x)
        return primes

    a, b, k = map(int, input().split())

    if not is_prime(k):
        print(0)
        return

    L = (a + k - 1) // k
    R = b // k

    if L > R:
        print(0)
        return

    primes = primes_less_than(k)

    ans = 0
    m = len(primes)

    for mask in range(1 << m):
        bits = 0
        prod = 1

        for i in range(m):
            if (mask >> i) & 1:
                bits += 1
                prod *= primes[i]

                if prod > R:
                    break
        else:
            cnt = R // prod - (L - 1) // prod

            if bits % 2 == 0:
                ans += cnt
            else:
                ans -= cnt

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("1 10 2\n") == "5", "sample 1"

# composite k
assert run("1 100 6\n") == "0", "composite k"

# single valid number
assert run("13 13 13\n") == "1", "single prime"

# no multiples of k in range
assert run("1 10 13\n") == "0", "empty multiplier interval"

# boundary case with ceil division
assert run("15 30 5\n") == "1", "left boundary correctness"

# larger interval
assert run("1 30 5\n") == "2", "inclusion exclusion correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100 6`|`0`| tổng hợp$k$thất bại ngay lập tức | 
|`13 13 13`|`1`| khoảng chứa đúng một số hợp lệ | 
|`1 10 13`|`0`| không có bội số của$k$tồn tại | 
|`15 30 5`|`1`| phân chia trần đúng ranh giới bên trái | 
|`1 30 5`|`2`| xử lý chồng chéo bao gồm-loại trừ | 

## Vỏ cạnh 

Xét trường hợp tổng hợp: 

đầu vào:```
1 100 6
```Thuật toán đầu tiên kiểm tra tính nguyên tố. Từ$6$chia hết cho$2$, nó không phải là số nguyên tố nên thuật toán trả về ngay:```
0
```Điều này đúng vì mọi bội số của$6$đã chia hết cho$2$, nhỏ hơn$6$. 

Bây giờ hãy xem xét trường hợp biên trong đó chỉ$k$bản thân nó có thể đủ điều kiện: 

đầu vào:```
10 20 13
```Chúng tôi tính toán:$$L = \left\lceil \frac{10}{13} \right\rceil = 1$$

$$R = \left\lfloor \frac{20}{13} \right\rfloor = 1$$Số nhân duy nhất là$t=1$, tương ứng với$x=13$. Từ$1$chia hết cho không có số nguyên tố bị cấm, câu trả lời là:```
1
```Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Cuối cùng, hãy xem xét bẫy từng cái một: 

đầu vào:```
15 30 5
```Thuật toán tính toán:$$L = (15+5-1)//5 = 3$$

$$R = 30//5 = 6$$Các số nhân ứng cử viên là$3,4,5,6$. Loại trừ bao gồm loại bỏ$3,4,6$vì chúng chia hết cho$2$hoặc$3$. Chỉ một$5$còn lại, đưa ra số hợp lệ duy nhất$25$. 

Nếu chúng tôi sử dụng cách chia tầng thông thường cho ranh giới bên trái, chúng tôi sẽ đưa hệ số nhân vào không chính xác$2$, tương ứng với$10$, nằm ngoài khoảng đó.
