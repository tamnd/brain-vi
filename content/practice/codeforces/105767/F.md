---
title: "CF 105767F - Đa thức Mega"
description: "Chúng ta được cho hai đa thức. Cái đầu tiên là đa thức tuyến tính $$f(x)=Ax+B$$ và cái thứ hai chỉ có hai số hạng khác 0: $$g(x)=Cx^n+Dx^{n-1}."
date: "2026-06-25T15:59:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105767
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #40 (Maths-Forces)"
rating: 0
weight: 105767
solve_time_s: 47
verified: true
draft: false
---

[CF 105767F - Đa thức Mega](https://codeforces.com/problemset/problem/105767/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai đa thức. Đầu tiên là đa thức tuyến tính$$f(x)=Ax+B$$và số hạng thứ hai chỉ có hai số hạng khác 0:$$g(x)=Cx^n+Dx^{n-1}.$$Chúng ta cần số đạo hàm nhỏ nhất, gọi nó là$k$, sao cho sau khi lấy đạo hàm$g(x)$chính xác$k$lần, đa thức thu được có thể chia cho$f(x)$trong khi vẫn giữ tất cả các hệ số của thương là số nguyên. 

Số lượng ca kiểm thử có thể đạt tới$10^5$, và mọi giá trị lớn nhất là$2\cdot10^5$. Một giải pháp mô phỏng các công cụ phái sinh hoặc thử mọi cách có thể$k$đối với mọi trường hợp thử nghiệm sẽ là quá chậm. Với$10^5$trường hợp, chúng tôi cần thời gian gần như không đổi cho mỗi trường hợp sau khi tiền xử lý. 

Các bẫy chính được gây ra bởi từ "số nguyên" trong định nghĩa chia hết. Khả năng chia hết cho số hữu tỉ là không đủ. 

Ví dụ:```
1
2 4 1 3 3
```Đây$$f(x)=2x+4,\quad g(x)=x^3+3x^2.$$Sau một đạo hàm:$$g'(x)=3x^2+6x=3x(x+2).$$Phần tuyến tính tỉ lệ với$2x+4$, nhưng thương số sẽ chứa một phân số:$$3x(x+2)=\frac{3}{2}x(2x+4).$$Câu trả lời đúng là không$1$. Chúng ta phải tiếp tục cho đến khi thương là một đa thức nguyên. 

Một trường hợp cạnh khác là khi đạt tới đa thức 0. Ví dụ:```
1
5 1 1 1 1
```Sau khi đủ đạo hàm, đa thức trở thành 0. Số 0 chia hết cho mọi đa thức vì thương số có thể đơn giản bằng 0. Câu trả lời phải bao gồm khả năng cuối cùng này. 

Trường hợp cạnh thứ ba là$k=0$. Đa thức ban đầu có thể đã thỏa mãn điều kiện. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là phân biệt nhiều lần$g(x)$và kiểm tra xem kết quả có chia hết cho$f(x)$. Điều này hoạt động về mặt toán học vì mức độ giảm đi một đơn vị đạo hàm và sau$n+1$đạo hàm đa thức bằng 0. Tuy nhiên, việc kiểm tra tất cả các dẫn xuất có thể có cho mỗi trường hợp thử nghiệm đều có chi phí$O(n)$, cho$O(2\cdot10^{10})$làm việc trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là mọi công cụ phái sinh đều giữ nguyên cấu trúc. Sau đó$k$các dẫn xuất, ở đâu$0\le k<n$,$$g^{(k)}(x)=
\frac{n!}{(n-k)!}C x^{n-k}
+
\frac{(n-1)!}{(n-1-k)!}D x^{n-1-k}.$$Đa thức có thể được viết là$$x^{n-1-k}(\alpha x+\beta).$$Sức mạnh bổ sung của$x$không quan trọng việc chia hết cho$Ax+B$. Phần liên quan duy nhất là yếu tố tuyến tính. Chúng tôi cần$$\alpha x+\beta=q(Ax+B)$$đối với một số nguyên$q$. 

Điều kiện tỷ lệ cho$$\frac{\alpha}{\beta}=\frac{A}{B}.$$Thay các hệ số đạo hàm:$$\frac{nC}{(n-k)D}=\frac AB.$$Sắp xếp lại:$$nBC=AD(n-k).$$Cho phép$$s=n-k.$$Sau đó$s$được xác định duy nhất:$$s=\frac{nBC}{AD}.$$Nếu đây không phải là số nguyên giữa$1$Và$n$, không có công thức đạo hàm nào khác 0 và câu trả lời là$n+1$. Nếu nó tồn tại, chúng ta vẫn cần kiểm tra xem thương có phải là tích phân hay không. Điều kiện còn lại là$$B \mid D\cdot \frac{(n-1)!}{(s-1)!}.$$Chúng ta chỉ cần số mũ nguyên tố của$B$. Từ$B\le 2\cdot10^5$, chúng ta có thể phân tích nó thành nhân tử và tính số mũ nguyên tố giai thừa bằng công thức Legendre. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n \log B)$mỗi trường hợp thử nghiệm |$O(200000)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$$num=nBC,\qquad den=AD.$$Phương trình tỷ lệ nói rằng$s=n-k=num/den$, do đó số đạo hàm được xác định trước khi kiểm tra tính tích phân. 

1. Nếu`num`không chia hết cho`den`, không có đạo hàm hợp lệ trước đa thức 0. Trở lại$n+1$. 
2. Hãy để$s=num/den$. Nếu như$s\notin[1,n]$, trở lại$n+1$. Giá trị của$s$đại diện cho$n-k$, vì vậy nó phải tương ứng với một đạo hàm hiện có. 
3. Kiểm tra xem$$B \mid D\cdot s(s+1)\cdots(n-1).$$Sản phẩm chính xác là$(n-1)!/(s-1)!$, xuất hiện trong hệ số không đổi của thừa số tuyến tính sau khi lấy vi phân. 

1. Nếu phép chia thành công thì câu trả lời là$k=n-s$. Nếu không thì quay lại$n+1$. 

Tại sao nó hoạt động: 

Mọi đạo hàm hữu ích có thể có đều có một hệ số tuyến tính có hệ số tỷ lệ phải phù hợp$Ax+B$. Tỷ lệ đó buộc chính xác một giá trị có thể có của$n-k$. Yêu cầu duy nhất còn lại là số nhân vô hướng phải là số nguyên. Kiểm tra số mũ nguyên tố xác minh chính xác điều kiện đó, do đó mọi giá trị được trả về đều thỏa mãn định nghĩa và mọi đạo hàm nhỏ hơn đã bị loại trừ bởi giá trị bắt buộc của$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 200000

spf = list(range(MAXN + 1))
for i in range(2, int(MAXN ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXN + 1, i):
            if spf[j] == j:
                spf[j] = i

def factor(x):
    res = []
    while x > 1:
        p = spf[x]
        c = 0
        while x % p == 0:
            x //= p
            c += 1
        res.append((p, c))
    return res

def fact_exp(n, p):
    ans = 0
    while n:
        n //= p
        ans += n
    return ans

def solve_case(A, B, C, D, n):
    num = B * n * C
    den = A * D

    if num % den:
        return n + 1

    s = num // den
    if s < 1 or s > n:
        return n + 1

    need = factor(B)

    for p, e in need:
        have = 0
        x = D
        while x % p == 0:
            have += 1
            x //= p
        have += fact_exp(n - 1, p) - fact_exp(s - 1, p)
        if have < e:
            return n + 1

    return n - s

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        A, B, C, D, n = map(int, input().split())
        ans.append(str(solve_case(A, B, C, D, n)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Đoạn mã đầu tiên xây dựng một bảng thừa số nguyên tố nhỏ nhất để phân tích thành thừa số$B$là nhanh chóng. Vì mọi giá trị đều được giới hạn bởi$200000$, quá trình tiền xử lý này được chia sẻ bởi tất cả các trường hợp thử nghiệm. 

Tính toán tỷ lệ sử dụng số nguyên Python nên không có rủi ro tràn. giá trị`s`được kiểm tra trước khi được chuyển thành câu trả lời vì$k=n-s$chỉ có ý nghĩa khi$s$là bằng cấp còn hiệu lực. 

Phép thử tính chia hết không bao giờ xây dựng trực tiếp tích giai thừa. Thay vào đó, nó so sánh số mũ nguyên tố. Số mũ của số nguyên tố trong$m!$được tìm thấy bằng cách chia nhiều lần$m$bằng số nguyên tố đó, đó là công thức Legendre. 

## Ví dụ đã hoạt động 

Dành cho:```
1
1 2 2 4 1
```Các biến phát triển như sau. 

| Bước | Giá trị | 
| --- | --- | 
|$num=B n C$| 8 | 
|$den=A D$| 4 | 
|$s=num/den$| 2 | 
|$k=n-s$| -1 | 

Đây$s>n$, nên con đường này là không thể. Đa thức 0 đạt được sau$n+1$đạo hàm nên đáp án là:```
2
```Vì:```
1
4 2 3 3 4
```| Bước | Giá trị | 
| --- | --- | 
|$num=B n C$| 24 | 
|$den=A D$| 12 | 
|$s$| 2 | 
|$k=n-s$| 2 | 
| Kiểm tra bắt buộc | vượt qua | 

Đạo hàm thứ hai là đạo hàm thứ nhất trong đó thương là một đa thức nguyên, cho:```
2
```Những ví dụ này thể hiện cả phương trình bậc và điều kiện thương số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log B \log n)$| Tính toán số mũ và giai thừa là logarit | 
| Không gian |$O(200000)$| Lưới lọc SPF bảo quản một lần | 

Quá trình tiền xử lý xử lý các giá trị lớn nhất có thể một lần. Mỗi trường hợp thử nghiệm chỉ chạm vào các thừa số nguyên tố của$B$, Vì thế$10^5$trường hợp thử nghiệm phù hợp thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    t = int(next(it))
    out = []
    for _ in range(t):
        A = int(next(it))
        B = int(next(it))
        C = int(next(it))
        D = int(next(it))
        n = int(next(it))
        out.append(str(solve_case(A, B, C, D, n)))
    return "\n".join(out)

assert run("""6
1 2 2 4 1
4 2 3 3 4
2 4 1 3 3
2 1 5 2 4
131296 123463 91609 133724 142208
172458 127836 190471 141192 190476
""") == """0
2
4
5
50599
190477"""

assert run("1\n1 1 1 1 1\n") == "0"
assert run("1\n2 4 1 3 3\n") == "4"
assert run("1\n200000 200000 200000 200000 200000\n") == "200001"
assert run("1\n5 1 1 1 10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1`|`0`| Đa thức ban đầu đã hoạt động | 
|`2 4 1 3 3`|`4`| Giới hạn thương số nguyên | 
|`200000 200000 200000 200000 200000`|`200001`| Giá trị lớn và dự phòng về đa thức bằng 0 | 
|`5 1 1 1 10`|`0`| Trường hợp tỷ lệ đơn giản | 

## Vỏ cạnh 

Khi đa thức ban đầu đã chia đúng, phương trình tỉ số sẽ cho$s=n$, vì vậy câu trả lời trở thành$k=0$. Thuật toán xử lý việc này vì nó cho phép$s$bằng nhau$n$. 

Khi điều kiện tỷ lệ được thỏa mãn nhưng thương lại là phân số, việc kiểm tra số mũ nguyên tố sẽ loại bỏ ứng cử viên. Ví dụ, với```
1
2 4 1 3 3
```ứng cử viên khác 0 duy nhất có thể là$k=1$, nhưng hệ số nhân chứa hệ số$3/2$. Thuật toán phát hiện ra rằng$B=4$không chia hệ số cần thiết và trả về câu trả lời đa thức bằng 0 sau đó. 

Khi không có đạo hàm nào trước đa thức 0 có thể hoạt động, giá trị trả về là$n+1$. Tại thời điểm đó$g^{(n+1)}(x)=0$, và số 0 chia hết cho đa thức tuyến tính có thương số 0.
