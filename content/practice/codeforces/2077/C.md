---
title: "CF 2077C - Tổng giá trị chuỗi nhị phân"
description: "Mỗi ký tự của chuỗi đóng góp +1 hoặc -1. Nếu chúng ta xác định $$w(c)=begin{cases} +1,&c='1' -1,&c='0' end{cases}$$ thì đối với bất kỳ phân đoạn nào của chuỗi nhị phân, hàm $F$ chỉ đơn giản là tổng của các giá trị này trên phân đoạn đó."
date: "2026-06-08T06:31:22+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "dp", "fft", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 2077
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1008 (Div. 1)"
rating: 2300
weight: 2077
solve_time_s: 118
verified: true
draft: false
---

[CF 2077C - Tổng giá trị chuỗi nhị phân](https://codeforces.com/problemset/problem/2077/C) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, cấu trúc dữ liệu, dp, fft, toán, ma trận 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ký tự của chuỗi đều đóng góp`+1`hoặc`-1`. 

Nếu chúng ta định nghĩa$$w(c)= \begin{cases} +1,&c='1'\\ -1,&c='0' \end{cases}$$thì đối với bất kỳ đoạn nào của chuỗi nhị phân, hàm$F$chỉ đơn giản là tổng của các giá trị này trên phân khúc đó. 

Đối với chuỗi nhị phân$v$, cho phép$$d=\#1-\#0$$là sự mất cân bằng tổng thể của nó. 

Điểm số của$v$thu được bằng cách chia chuỗi thành tiền tố và hậu tố và tối đa hóa$$F(\text{prefix})\cdot F(\text{suffix}).$$Chúng tôi được cung cấp một chuỗi nhị phân$s$. Sau mỗi thao tác lật, chúng ta phải tính tổng điểm trên tất cả các chuỗi con không trống của chuỗi hiện tại. 

Quan sát đầu tiên từ các ràng buộc là độ dài chuỗi và số lượng truy vấn có thể đạt tới$2\cdot 10^5$, và tổng số của chúng trên tất cả các trường hợp thử nghiệm cũng là$2\cdot 10^5$. Bất kỳ giải pháp nào kiểm tra các dãy con một cách rõ ràng đều không thể thực hiện được vì một chuỗi có độ dài$2\cdot 10^5$có$2^{200000}$các chuỗi tiếp theo. Thậm chí một$O(n)$việc tính toán lại cho mỗi truy vấn sẽ quá tốn kém vì điều đó sẽ dẫn đến$O(nq)$. 

Câu trả lời chỉ phải phụ thuộc vào một lượng thông tin rất nhỏ có thể được cập nhật sau một lần lật. 

Trường hợp cạnh tinh tế là một dãy con có độ dài bằng một. 

Ví dụ:```
v = "1"
```Cách phân chia duy nhất có thể sử dụng một mặt trống, vì vậy điểm số là$0$. Bất kỳ đạo hàm nào giả định cả hai vế đều không trống sẽ cho giá trị dương không chính xác. 

Một sai lầm dễ mắc phải khác là xử lý tính chẵn lẻ không chính xác. 

Vì```
v = "111"
```chúng tôi có$d=3$. Điểm số không$d^2/4=9/4$. Câu trả lời thực tế là$$\left\lfloor \frac d2\right\rfloor \left\lceil \frac d2\right\rceil = 1\cdot2 = 2.$$Việc hiệu chỉnh tính chẵn lẻ là bước quan trọng của toàn bộ giải pháp. 

Bẫy thứ ba là dãy con trống. Vấn đề yêu cầu các chuỗi con không trống, nhưng công thức cuối cùng sẽ tính tất cả các chuỗi con một cách tự nhiên. May mắn là dãy con trống có sự mất cân bằng$0$và ghi điểm$0$, vì vậy việc bao gồm nó không thay đổi gì cả. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi dãy con, tính toán sự mất cân bằng của nó, xác định điểm của nó và cộng kết quả. 

Đối với một chuỗi có độ dài$n$, điều này đòi hỏi phải kiểm tra$2^n$các chuỗi tiếp theo. Ngay cả đối với$n=60$, điều đó đã vượt xa tính khả thi. Với$n=2\cdot10^5$, điều đó là hoàn toàn không thể. 

Quan sát quan trọng là điểm của dãy con chỉ phụ thuộc vào sự mất cân bằng của nó$$d=\#1-\#0.$$Nó không phụ thuộc vào thứ tự các ký tự bên trong dãy con. 

Giả sử dãy con có sự mất cân bằng$d$. Đặt sự mất cân bằng của tiền tố đã chọn là$x$. Khi đó sự mất cân bằng hậu tố là$d-x$, do đó mỗi phép chia đều tạo ra$$x(d-x).$$Phương trình bậc hai này được tối đa hóa khi hai phần càng gần nhau càng tốt. Giá trị lớn nhất trở thành$$\left\lfloor \frac d2\right\rfloor \left\lceil \frac d2\right\rceil = \frac{d^2-(d\bmod 2)}4.$$Bây giờ vấn đề trở thành:$$\sum_{\text{subsequence}} \frac{d^2-(d\bmod2)}4.$$Thứ tự tiếp theo đã biến mất hoàn toàn. 

Cho phép$R$là số đơn vị trong chuỗi hiện tại. Sau đó mọi truy vấn chỉ thay đổi$R$qua$\pm1$. 

Nhiệm vụ còn lại là tính tổng$d^2$trên tất cả các chuỗi con và sau đó trừ đi hiệu chỉnh chẵn lẻ. Sử dụng cách giải thích xác suất của một dãy con ngẫu nhiên thống nhất, cả hai đại lượng đều thừa nhận dạng đóng. Sau khi đơn giản hóa, câu trả lời chỉ phụ thuộc vào$n$Và$R$:$$\boxed{ 2^{\,n-4} \left((2R-n)^2+n-2\right) } \pmod{998244353}.$$Từ$n$không bao giờ thay đổi và một cú lật chỉ thay đổi$R$, mỗi truy vấn có thể được trả lời trong$O(1)$. 

Đạo hàm của công thức này được đưa ra trong phần tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi truy vấn |$O(n)$cho chuỗi | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Suy ra công thức tính điểm 

hãy để$$d=\#1-\#0.$$Đối với phần tách có sự mất cân bằng tiền tố là$x$,$$\text{value}=x(d-x).$$Parabol này đạt cực đại khi$x$gần nhất với$d/2$. 

Kể từ đây$$\text{score}(v) = \left\lfloor\frac d2\right\rfloor \left\lceil\frac d2\right\rceil = \frac{d^2-(d\bmod2)}4.$$### Tính tổng tất cả các dãy con 

Giải thích mọi chuỗi con như sự lựa chọn độc lập xem mỗi vị trí có được đưa vào hay không. 

Xác định một biến ngẫu nhiên:$$X_i= \begin{cases} 1,&s_i=1\text{ and chosen}\\ -1,&s_i=0\text{ and chosen}\\ 0,&\text{otherwise} \end{cases}$$Sau đó$$d=\sum_i X_i.$$Cho phép$R$là số đơn vị và$Z=n-R$. 

Chúng tôi có$$E[d] = \frac{R-Z}{2} = \frac{2R-n}{2}.$$Cũng$$\operatorname{Var}(X_i)=\frac14,$$Vì thế$$\operatorname{Var}(d) = \frac n4.$$Vì thế$$E[d^2] = \operatorname{Var}(d)+E[d]^2 = \frac{(2R-n)^2+n}{4}.$$Vì có$2^n$dãy số,$$\sum d^2 = 2^nE[d^2] = 2^{n-2}\big((2R-n)^2+n\big).$$Điểm số có chứa$$\frac{d^2-(d\bmod2)}4.$$Sự điều chỉnh chẵn lẻ vẫn còn. 

Một dãy con có sự mất cân bằng lẻ chính xác khi độ dài của nó là số lẻ. Chính xác một nửa số dãy con có độ dài lẻ, cụ thể là$$2^{n-1}.$$Như vậy$$\sum (d\bmod2) = 2^{n-1}.$$Đặt mọi thứ lại với nhau:$$\text{Answer} = \frac14 \left( 2^{n-2}\big((2R-n)^2+n\big) - 2^{n-1} \right).$$Sau khi tính toán:$$\boxed{ 2^{n-4} \left( (2R-n)^2+n-2 \right) }.$$### Đang xử lý cập nhật 

1. Đếm số hàng đơn vị hiện tại$R$. 
2. Tính toán trước lũy thừa của hai modulo$998244353$. 
3. Với mỗi truy vấn, lật ký tự đã chọn. 
4. Cập nhật$R$qua$+1$hoặc$-1$. 
5. Tính toán$$x=2R-n.$$1. Đầu ra$$2^{n-4}(x^2+n-2) \pmod{998244353}.$$Vì$n<4$, hệ số$2^{n-4}$được giải thích modulo$998244353$sử dụng nghịch đảo mô đun của$2$. 

### Tại sao nó hoạt động 

Bất biến chính là mọi dãy con chỉ đóng góp thông qua sự mất cân bằng của nó$d$. Khi điểm số được viết lại thành$$\frac{d^2-(d\bmod2)}4,$$tất cả thông tin cấu trúc về dãy con đều biến mất. 

Tổng của$d^2$trên tất cả các chuỗi con được xác định hoàn toàn bởi hai thời điểm đầu tiên của biến mất cân bằng ngẫu nhiên. Việc hiệu chỉnh tính chẵn lẻ chỉ phụ thuộc vào số lượng chuỗi con có sự mất cân bằng lẻ, bằng với số lượng chuỗi con có độ dài lẻ. Cả hai đại lượng chỉ phụ thuộc vào$n$và số lượng hiện tại$R$. 

Mỗi truy vấn chỉ thay đổi$R$, vì vậy việc đánh giá biểu mẫu đã đóng sau khi cập nhật$R$tạo ra chính xác số tiền cần thiết. 

Đạo hàm rút ra trực tiếp từ định nghĩa điểm và đếm mọi dãy con chính xác một lần, điều này chứng tỏ tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 200000

pow2 = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    pow2[i] = pow2[i - 1] * 2 % MOD

inv2 = (MOD + 1) // 2

def power_two_shift(n):
    if n >= 4:
        return pow2[n - 4]
    return pow(inv2, 4 - n, MOD)

t = int(input())

for _ in range(t):
    n, q = map(int, input().split())
    s = list(input().strip())

    ones = s.count('1')
    coef = power_two_shift(n)

    for _ in range(q):
        pos = int(input()) - 1

        if s[pos] == '1':
            s[pos] = '0'
            ones -= 1
        else:
            s[pos] = '1'
            ones += 1

        x = 2 * ones - n
        ans = coef * ((x * x + n - 2) % MOD)
        ans %= MOD

        print(ans)
```Việc thực hiện phản ánh trực tiếp công thức toán học. 

giá trị$2^{n-4}$cần được chăm sóc đặc biệt khi$n<4$. Trong số học mô-đun, điều này có nghĩa là nhân với lũy thừa của$2^{-1}$, do đó mã sử dụng mô-đun nghịch đảo của hai. 

Hệ số chỉ phụ thuộc vào$n$, không bao giờ thay đổi trong trường hợp thử nghiệm, vì vậy nó được tính một lần. 

Mỗi lần lật cập nhật số lượng cái trong thời gian không đổi. Việc đếm lại toàn bộ chuỗi sau mỗi truy vấn sẽ làm tăng độ phức tạp lên$O(nq)$, điều đó là không cần thiết. 

biểu thức$(2R-n)^2$dễ dàng phù hợp với số nguyên Python, do đó không có mối lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
s = 010
queries = [1, 3]
```| Truy vấn | Chuỗi Sau Khi Lật | những cái$R$|$x=2R-n$| Kết quả Công thức | 
| --- | --- | --- | --- | --- | 
| 1 | 110 | 2 | 1 | 1 | 
| 3 | 111 | 3 | 3 | 5 | 

Vì$n=3$,$$2^{n-4}=2^{-1}.$$Sau lần lật đầu tiên,$$\frac{1^2+3-2}{2}=1.$$Sau lần lật thứ hai,$$\frac{3^2+3-2}{2}=5.$$Những cái này khớp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
n = 2
s = 00
query = [1]
```| Truy vấn | Chuỗi Sau Khi Lật | những cái$R$|$x$| Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 1 | 0 | 0 | 

Các dãy tiếp theo là:```
1
0
10
```Mọi điểm đều bằng 0 nên tổng điểm bằng 0. 

Công thức cho$$2^{-2}(0^2+2-2)=0.$$Dấu vết xác nhận rằng biểu mẫu đóng cũng xử lý các giá trị nhỏ của$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi truy vấn | Chỉ một lần lật, cập nhật số đếm và đánh giá một công thức | 
| Không gian |$O(n)$| Lưu trữ chuỗi có thể thay đổi | 

Tổng số truy vấn trên tất cả các trường hợp thử nghiệm nhiều nhất là$2\cdot10^5$. Công việc liên tục trên mỗi truy vấn dễ dàng phù hợp với giới hạn thời gian và việc lưu trữ chuỗi chỉ yêu cầu bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    MOD = 998244353
    input_data = io.StringIO(inp)

    def input():
        return input_data.readline()

    MAXN = 200000
    pow2 = [1] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        pow2[i] = pow2[i - 1] * 2 % MOD

    inv2 = (MOD + 1) // 2

    def coef(n):
        if n >= 4:
            return pow2[n - 4]
        return pow(inv2, 4 - n, MOD)

    out = []

    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = list(input().strip())

        ones = s.count('1')
        c = coef(n)

        for _ in range(q):
            p = int(input()) - 1

            if s[p] == '1':
                s[p] = '0'
                ones -= 1
            else:
                s[p] = '1'
                ones += 1

            x = 2 * ones - n
            ans = c * ((x * x + n - 2) % MOD) % MOD
            out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""3
3 2
010
1
3
10 3
0101000110
3
5
10
24 1
011001100110000101111000
24
"""
) == "\n".join([
"1",
"5",
"512",
"768",
"1536",
"23068672"
])

# n = 1
assert run(
"""1
1 1
0
1
"""
) == "0"

# all ones
assert run(
"""1
3 1
111
1
"""
) == "1"

# all zeros
assert run(
"""1
2 1
00
1
"""
) == "0"

# flip twice on same position
assert run(
"""1
2 2
01
1
1
"""
) == "0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`0`| Chuỗi ký tự đơn | 
|`111`rồi lật |`1`| Xử lý mất cân bằng lẻ | 
|`00`rồi lật |`0`| Chuỗi cân bằng | 
| Cùng một vị trí bị lật hai lần |`0,0`| Cập nhật liên tục | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
0
1
```Sau khi lật sợi dây trở thành`"1"`. Có chính xác một dãy con không trống và điểm của nó bằng 0. Công thức cho$$2^{-3}(1+1-2)=0.$$Thuật toán xử lý chính xác chuỗi nhỏ nhất có thể. 

Bây giờ hãy xem xét:```
1
1 1
1
1
```Sau khi lật sợi dây trở thành`"0"`. 

Sự mất cân bằng là$-1$, nhưng điểm vẫn bằng 0 vì mỗi phép chia đều sử dụng một cạnh trống. Công thức phụ thuộc vào$d^2$, không phải dấu của$d$, và một lần nữa tạo ra số không. 

Cuối cùng hãy xem xét:```
1
3 1
111
1
```Sau khi lật chúng ta có được`"011"`. 

Số cái trở thành$R=2$, cho$x=1$. Câu trả lời là$$2^{-1}(1^2+3-2)=1.$$Trường hợp này thực hiện việc hiệu chỉnh chẵn lẻ. Chỉ sử dụng$d^2/4$sẽ tạo ra một giá trị phân số và dẫn đến một dẫn xuất không chính xác. Thuật ngữ hiệu chỉnh sẽ loại bỏ chính xác việc đếm thừa đối với sự mất cân bằng lẻ.
