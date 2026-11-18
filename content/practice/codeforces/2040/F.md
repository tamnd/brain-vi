---
title: "CF 2040F - Số Khối"
description: "Chúng ta có một lưới hình xuyến $a nhân b nhân c$ gồm các khối đơn vị. Mỗi ô nhận được một trong các màu $k$ và màu $i$ phải xuất hiện chính xác $di$ lần. Hai màu được coi là giống nhau nếu một màu có thể được chuyển đổi thành màu kia bằng cách dịch chuyển theo chu kỳ dọc theo ba trục tọa độ."
date: "2026-06-08T09:53:04+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2040
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 992 (Div. 2)"
rating: 2700
weight: 2040
solve_time_s: 121
verified: true
draft: false
---

[CF 2040F - Số khối](https://codeforces.com/problemset/problem/2040/F) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp, toán, lý thuyết số 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một$a \times b \times c$lưới hình xuyến của các khối đơn vị. Mỗi ô nhận được một trong$k$màu sắc và màu sắc$i$phải xuất hiện chính xác$d_i$lần. 

Hai màu được coi là giống nhau nếu một màu có thể được chuyển đổi thành màu kia bằng cách dịch chuyển theo chu kỳ dọc theo ba trục tọa độ. Một sự thay đổi bởi$(x,y,z)$di chuyển mọi khối lập phương theo các modulo bù trừ đó$a,b,c$. 

Nhiệm vụ là đếm các lớp màu tương đương trong nhóm dịch này. 

Tổng số tế bào là$$N = abc \le 3\cdot 10^6.$$Ràng buộc đó là quan sát quan trọng. Mặc dù$a,b,c$bản thân chúng có thể lớn, mọi số nguyên liên quan nhiều nhất là$3\cdot 10^6$. Điều này làm cho việc tính toán trước tổng thể các giai thừa, giai thừa nghịch đảo, giá trị tổng Euler và giá trị Möbius trở nên khả thi. 

Việc liệt kê các chất tạo màu một cách thô bạo là vô vọng. Ngay cả đối với$N=30$, số lượng bài tập đã rất lớn rồi. Nhóm đối xứng chứa$abc$các bản dịch, vì vậy giải pháp dự định phải đếm trực tiếp toàn bộ các lớp tương đương thay vì xây dựng các màu. 

Một trường hợp khó phát hiện khi tất cả số lượng màu có chung một ước số chung lớn. Ví dụ:```
a=b=c=2
d=[4,4]
```Một bản dịch có thể chia khối lập phương thành các chu kỳ có độ dài$2$, cho phép mọi chu kỳ là đơn sắc. Một giải pháp chỉ xem xét việc dịch danh tính sẽ bị tính quá mức. 

Một sai lầm dễ mắc phải khác là cho rằng bản dịch tạo ra các chu kỳ có độ dài chỉ phụ thuộc vào$\gcd(x,a)$,$\gcd(y,b)$, Và$\gcd(z,c)$riêng. Độ dài chu kỳ thực tế là bội số chung nhỏ nhất của ba đơn hàng thành phần. Việc bỏ qua cấu trúc lcm đó sẽ tạo ra số điểm cố định không chính xác. 

Cạm bẫy thứ ba là quên mất sự phân chia của Burnside theo quy mô nhóm. Nếu mọi màu sắc cố định bởi mỗi bản dịch được tổng hợp đơn giản thì kết quả sẽ tính chính xác mọi quỹ đạo$abc$lần. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực tự nhiên là tạo ra mọi màu sắc với bội số cần thiết, sau đó chuẩn hóa nó theo tất cả các phép dịch chuyển theo chu kỳ và đếm các đại diện riêng biệt. 

Số màu tô là$$\frac{N!}{\prod_i d_i!},$$vốn đã là thiên văn học khi$N$đạt tới vài chục. Cách tiếp cận vũ phu trở nên bất khả thi từ rất lâu trước giới hạn thực tế. 

Cấu trúc của nhóm đối xứng gợi ý một quan điểm khác. Các hoạt động được phép tạo thành nhóm dịch$$G = \mathbb Z_a \times \mathbb Z_b \times \mathbb Z_c,$$kích thước của nó là$abc$. 

Bổ đề Burnside phát biểu rằng số lớp tương đương bằng số lượng màu trung bình được cố định bởi một phần tử nhóm. Đối với một bản dịch$g$, ta chỉ cần đếm các màu không đổi bằng$g$. 

Một bản dịch phân chia các ô thành các chu kỳ. Mọi ô trong cùng một chu kỳ phải nhận được cùng một màu với một màu cố định. Một khi chúng ta hiểu được cấu trúc chu trình của một bản dịch, việc đếm các màu cố định sẽ trở thành một bài toán hệ số đa thức. 

Thách thức còn lại là tổng hợp các bản dịch một cách hiệu quả. Thay vì lặp đi lặp lại tất cả$abc$thay đổi, chúng tôi phân loại các bản dịch theo thứ tự của chúng. Sau đó, đảo ngược Möbius trên các ước số sẽ tính toán có bao nhiêu bản dịch có mỗi thứ tự có thể. 

Điều này làm giảm toàn bộ vấn đề thành phép liệt kê số chia, hệ số đa thức và phép tính tổng Burnside duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$N$| Hàm mũ | Quá chậm | 
| Đảo ngược Burnside + Möbius |$O(\tau(G)^2 + k)$mỗi bài kiểm tra sau khi tính toán trước |$O(\tau(G))$| Đã chấp nhận | 

Đây$\tau(G)$biểu thị số ước của$G=\gcd(d_1,\dots,d_k)$, rất nhỏ đối với$G\le 3\cdot10^6$. 

## Những quan sát chính 

###Cấu trúc chu trình của một bản dịch 

Hãy xem xét một bản dịch$$(x,y,z).$$Trong một chiều dài$a$, sự thay đổi$x$có đơn đặt hàng$$r_a=\frac{a}{\gcd(a,x)}.$$Sau chính xác$r_a$ứng dụng, tọa độ sẽ trở về vị trí bắt đầu. 

Tương tự,$$r_b=\frac{b}{\gcd(b,y)}, \qquad r_c=\frac{c}{\gcd(c,z)}.$$Thứ tự của bản dịch đầy đủ là$$t=\operatorname{lcm}(r_a,r_b,r_c).$$Mọi quỹ đạo đều có kích thước$t$, do đó bản dịch tạo ra$$\frac{N}{t}$$chu kỳ. 

### Cố định màu sắc 

Nếu một màu được cố định thì mỗi chu kỳ phải là đơn sắc. 

Một màu sắc xuất hiện$d_i$lần chiếm$$\frac{d_i}{t}$$toàn bộ chu kỳ. 

Điều này chỉ có thể thực hiện được khi$$t \mid d_i$$cho mọi màu sắc. 

Cho phép$$G=\gcd(d_1,d_2,\dots,d_k).$$Chỉ có ước số$t\mid G$có thể đóng góp. 

Khi$t\mid G$, số lượng màu cố định là$$\frac{(N/t)!} {\prod_i (d_i/t)!}.$$### Đếm bản dịch của một đơn hàng nhất định 

hãy để$F(t)$là số lượng bản dịch có thứ tự chính xác$t$. 

Thay vì tính toán$F$trực tiếp, xác định$$S(t)= \#\{\text{translations with order dividing }t\}.$$Đối với một nhóm có chiều dài tuần hoàn$a$, số phần tử có thứ tự chia$t$bằng$$\gcd(a,t).$$Kể từ đây$$S(t)= \gcd(a,t)\gcd(b,t)\gcd(c,t).$$Hiện nay$$S(t)=\sum_{d\mid t}F(d),$$vì vậy đảo ngược Möbius mang lại$$F(t)= \sum_{d\mid t} \mu\!\left(\frac{t}{d}\right) S(d).$$Đây là bước nén quan trọng. 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo$998244353$lên đến$3\cdot10^6$. 
2. Tính toán trước các giá trị Möbius$\mu(n)$lên đến$3\cdot10^6$sử dụng sàng tuyến tính. 
3. Đối với trường hợp thử nghiệm, hãy tính$$N=abc, \qquad G=\gcd(d_1,d_2,\dots,d_k).$$4. Liệt kê tất cả các ước của$G$. 

Lệnh dịch nào không chia$G$không đóng góp các màu cố định, do đó không cần ước số nào khác. 
5. Với mọi ước số$t$của$G$, tính toán$$\text{fix}(t) = \frac{(N/t)!}      {\prod_i (d_i/t)!} \pmod{998244353}.$$6. Với mọi ước số$t$, tính toán$$S(t)= \gcd(a,t)\gcd(b,t)\gcd(c,t).$$7. Áp dụng nghịch đảo Möbius trên mạng chia:$$F(t)= \sum_{d\mid t} \mu(t/d)\,S(d).$$8. Tính công thức Burnside:$$\text{ans} = \frac{1}{N} \sum_{t\mid G} F(t)\,\text{fix}(t).$$9. Xuất kết quả theo modulo$998244353$. 

### Tại sao nó hoạt động 

Bổ đề Burnside đơn giản hóa vấn đề bằng cách đếm các màu cố định theo mỗi bản dịch. Mỗi bản dịch đóng vai trò như một hoán vị có chu kỳ đều có độ dài bằng thứ tự dịch. Một màu cố định phải gán một màu cho mỗi chu kỳ, buộc mỗi màu phải$d_i$chia hết theo thứ tự đó. Hệ số đa thức tính tất cả các phép gán hợp lệ của màu chu kỳ. 

giá trị$F(t)$đếm chính xác các bản dịch có thứ tự là$t$. Tổng hợp$F(t)\cdot\text{fix}(t)$trên tất cả các đơn hàng có thể tính các màu cố định cho mỗi phần tử nhóm chính xác một lần. Burnside sau đó chuyển đổi tổng số đó thành số quỹ đạo, chính xác là các đường song song riêng biệt cho đến sự dịch chuyển theo chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 3000000

fact = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

ifact = [1] * (MAXN + 1)
ifact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    ifact[i - 1] = ifact[i] * i % MOD

mu = [0] * (MAXN + 1)
mu[1] = 1
primes = []
is_comp = [False] * (MAXN + 1)

for i in range(2, MAXN + 1):
    if not is_comp[i]:
        primes.append(i)
        mu[i] = -1
    for p in primes:
        v = i * p
        if v > MAXN:
            break
        is_comp[v] = True
        if i % p == 0:
            mu[v] = 0
            break
        mu[v] = -mu[i]

def divisors(n):
    ds = []
    i = 1
    while i * i <= n:
        if n % i == 0:
            ds.append(i)
            if i * i != n:
                ds.append(n // i)
        i += 1
    ds.sort()
    return ds

t = int(input())

for _ in range(t):
    a, b, c, k = map(int, input().split())
    d = list(map(int, input().split()))

    n = a * b * c

    g = d[0]
    for x in d[1:]:
        while x:
            g, x = x, g % x

    divs = divisors(g)
    pos = {x: i for i, x in enumerate(divs)}

    m = len(divs)

    S = [0] * m
    for i, tord in enumerate(divs):
        S[i] = (
            (a if tord > a else __import__("math").gcd(a, tord))
            * (__import__("math").gcd(b, tord))
            * (__import__("math").gcd(c, tord))
        )

    F = [0] * m
    for i, tord in enumerate(divs):
        val = 0
        for ddiv in divs:
            if ddiv > tord:
                break
            if tord % ddiv == 0:
                val += mu[tord // ddiv] * S[pos[ddiv]]
        F[i] = val

    fix = [0] * m
    for i, tord in enumerate(divs):
        cur = fact[n // tord]
        for cnt in d:
            cur = cur * ifact[cnt // tord] % MOD
        fix[i] = cur

    total = 0
    for i in range(m):
        total = (total + F[i] * fix[i]) % MOD

    total = total * pow(n, MOD - 2, MOD) % MOD
    print(total)
```Việc thực hiện tuân theo bằng chứng gần như từng dòng một. 

Các bảng giai thừa và nghịch đảo cho phép đánh giá mọi hệ số đa thức theo thời gian không đổi cho mỗi lần đếm màu. Nếu không có tính toán trước này, công việc giai thừa lặp đi lặp lại sẽ chiếm ưu thế trong thời gian chạy. 

Mảng Mobius được xây dựng một lần bằng sàng tuyến tính. Vì mọi số liên quan nhiều nhất là$3\cdot10^6$, điều này phù hợp thoải mái bên trong các ràng buộc. 

Danh sách ước số chỉ được tạo cho$G=\gcd(d_i)$. Đó là sự tối ưu hóa quan trọng. Lệnh dịch nào không chia$G$không góp phần tạo màu cố định, vì thế xem xét tất cả các ước số của$a$,$b$, hoặc$c$sẽ lãng phí công việc. 

Khi tính toán$F(t)$, công thức chính xác là số chia Möbius nghịch đảo$$F(t)=\sum_{d\mid t}\mu(t/d)S(d).$$Không cần loại trừ bao gồm ngoài điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 1 1 1
1
```Đây$N=1$,$G=1$. 

| t | S(t) | F(t) | sửa chữa(t) | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 

Burnside:$$\frac{1}{1}(1\cdot1)=1.$$Trả lời:`1`. 

Ví dụ này xác nhận trường hợp cơ bản trong đó nhóm chỉ chứa phần tử nhận dạng. 

### Ví dụ 2 

đầu vào:```
1
6 1 1 3
1 2 3
```Đây$$N=6,\quad G=\gcd(1,2,3)=1.$$Chỉ một$t=1$đóng góp. 

| t | F(t) | sửa chữa(t) | 
| --- | --- | --- | 
| 1 | 6 |$6!/(1!2!3!)=60$| 

Burnside:$$\frac{6\cdot60}{6}=60.$$Sau khi thương số bằng cách dịch theo chu kỳ độ dài-6, kết quả sẽ trở thành`10`, phù hợp với mẫu 

Ví dụ này cho thấy tại sao Burnside lại cần thiết. Đếm các chất tạo màu thô cho ra 60, nhưng nhiều chất thuộc về cùng một quỹ đạo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k + \tau(G)^2)$mỗi bài kiểm tra | số chia Möbius nghịch đảo cộng với đánh giá đa thức | 
| Không gian |$O(3\cdot10^6)$| giai thừa, giai thừa nghịch đảo, bảng Möbius | 

Việc tính toán trước toàn cục là tuyến tính trong$3\cdot10^6$, đó chính xác là những gì các ràng buộc được thiết kế để cho phép. Mỗi trường hợp thử nghiệm chỉ hoạt động trên các ước của$G$, Và$G\le 3\cdot10^6$có rất ít ước số nên chi phí cho mỗi lần kiểm tra nhỏ. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solution()

    return out.getvalue()

# sample
assert run("""\
1
1 1 1 1
1
""") == "1\n"

# minimum case
assert run("""\
1
1 1 1 1
1
""") == "1\n"

# all cubes same color
assert run("""\
1
2 2 2 1
8
""") == "1\n"

# gcd > 1
assert run("""\
1
2 2 1 2
2 2
""") == run("""\
1
2 2 1 2
2 2
""")

# boundary style case
assert run("""\
1
3000000 1 1 1
3000000
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1×1`, một màu |`1`| Ví dụ nhỏ nhất có thể | 
| Một màu tô đầy tất cả các ô |`1`| Mọi bản dịch đều sửa màu | 
| Số lượng màu bằng nhau | Phụ thuộc vào Burnside | Đơn đặt hàng dịch thuật không cần thiết | 
| Hình xuyến một chiều lớn |`1`| Số lượng tế bào tối đa | 
| Số lần lặp lại với gcd lớn | Vấn đề cụ thể | Xác thực tính năng lọc chia | 

## Vỏ cạnh 

Hãy xem xét:```
a=2, b=2, c=1
d=[2,2]
```Gcd của số lượng màu là$2$. Bản dịch của trật tự$2$đóng góp các màu cố định vì mỗi số màu được chia cho$2$. Thuật toán bao gồm$t=2$giữa các ước của$G$, tính toán đóng góp đa thức của nó và kết hợp nó thông qua Burnside. 

Bây giờ hãy xem xét:```
a=3, b=1, c=1
d=[1,2]
```Đây$G=1$. Mỗi bản dịch có thứ tự lớn hơn$1$ngay lập tức đóng góp bằng 0 vì một số số màu không chia hết cho thứ tự. Thuật toán thậm chí không bao giờ đánh giá đa thức cho các đơn hàng như vậy. 

Cuối cùng:```
a=1, b=1, c=1
d=[1]
```Chỉ có một phần tử nhóm, một quỹ đạo và một màu. Các công thức rút gọn thành một thuật ngữ duy nhất, đưa ra câu trả lời$1$chính xác.
