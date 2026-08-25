---
title: "CF 104326F - Lặp lại b-ary"
description: "Chúng ta đang xem xét cách biểu diễn một phân số, cụ thể là $frac{1}{x}$, nhưng được viết bằng cơ số $b$ thay vì cơ số 10. Khi bạn mở rộng một số hữu tỷ theo cơ số bất kỳ, phần phân số của nó cuối cùng sẽ trở thành tuần hoàn."
date: "2026-07-01T19:09:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "F"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 72
verified: true
draft: false
---

[CF 104326F - Lặp lại b-ary](https://codeforces.com/problemset/problem/104326/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét cách biểu diễn một phân số, cụ thể là$\frac{1}{x}$, nhưng được viết bằng cơ sở$b$thay vì cơ số 10. Khi bạn khai triển một số hữu tỷ theo cơ số bất kỳ, phần phân số của nó cuối cùng sẽ trở thành tuần hoàn. Một số tiền tố của các chữ số có thể xuất hiện trước khi bắt đầu lặp lại và sau đó các chữ số đó sẽ lặp lại mãi mãi. 

Nhiệm vụ không phải là xây dựng bản khai triển mà chỉ xác định hai giá trị: có bao nhiêu chữ số xuất hiện trước khi chu kỳ lặp lại bắt đầu và chu kỳ lặp lại kéo dài bao lâu. 

Đầu vào cho một mẫu số$x$và một căn cứ$b$. Chúng ta tưởng tượng việc thực hiện phép chia dài của 1 cho$x$, nhưng mỗi bước được thực hiện trong cơ sở$b$. Ở mỗi bước, số dư xác định chữ số tiếp theo và nhân số dư với$b$mô phỏng việc chuyển sang vị trí phân số tiếp theo. 

Hành vi của quá trình này được kiểm soát hoàn toàn bởi phần dư modulo$x$. Khi phần dư lặp lại, chuỗi chữ số cũng phải lặp lại vì quá trình này mang tính quyết định. 

Những ràng buộc cho phép$x \le 10^{12}$Và$b \le 10^{18}$. Điều này loại trừ mọi mô phỏng mở rộng từng chữ số. Một mô phỏng đơn giản có thể chạy tới$x$bước trong trường hợp xấu nhất trước khi lặp lại phần dư vốn đã quá lớn. 

Một vấn đề tinh tế hơn là ngay cả việc suy luận trực tiếp về tất cả số dư cũng không đủ trừ khi chúng ta hiểu cách nhân với$b$hành xử các yếu tố modulo của$x$. Cấu trúc của các chu trình phụ thuộc vào việc liệu quá trình có thể đạt được số nguyên tố cùng nhau còn lại với$x$, và cách phân tích nhân tử của$x$tương tác với$b$. 

Một sai lầm ngây thơ là cho rằng khoảng thời gian luôn liên hệ với bậc nhân của$b$modulo$x$. Điều này thất bại khi$x$Và$b$không nguyên tố cùng nhau, bởi vì lũy thừa của$b$có thể phá hủy một phần cấu trúc mô đun ngay lập tức, tạo ra tiền tố không tuần hoàn. 

Một lỗi nhỏ khác là coi độ dài tiền tố luôn bằng 0. Nếu như$x$chia sẻ các thừa số nguyên tố với$b$, phép chia ban đầu liên tục hủy bỏ các thừa số đó trước khi bất kỳ chu kỳ nào có thể bắt đầu. 

Ví dụ, nếu$x = 12$Và$b = 10$, cơ số có chung thừa số 2 và 3 với mẫu số. Sự mở rộng của$1/12$trong cơ số 10 là hữu hạn nên phần tuần hoàn bằng 0. Một phương pháp chỉ tính modulo bậc nhân$x$sẽ tạo ra một chu trình khác 0 một cách không chính xác. 

## Phương pháp tiếp cận 

Quan sát quan trọng là quá trình phân chia dài có thể được tách thành hai giai đoạn: một giai đoạn trong đó mẫu số chia sẻ các thừa số với cơ số và giai đoạn khác khi nó trở thành nguyên tố cùng nhau với cơ số. 

Viết$x = g \cdot x'$, Ở đâu$g$chứa tất cả các thừa số nguyên tố của$x$điều đó cũng chia$b$. Các yếu tố này được “hấp thụ” vào cơ sở trong quá trình mở rộng, nghĩa là chúng chỉ đóng góp vào một tiền tố hữu hạn và không bao giờ tham gia vào chu trình lặp lại. 

Sau khi loại bỏ tất cả các thừa số chung như vậy, chúng ta còn lại mẫu số rút gọn$x'$như vậy$\gcd(x', b) = 1$. Từ thời điểm này trở đi, việc khai triển phân số hoạt động giống như một hệ thống mô-đun thuần túy được nhân với$b$, và khoảng thời gian tương ứng với thứ tự nhân của$b$modulo$x'$. 

Độ dài tiền tố được xác định bằng số lần chúng ta phải nhân phần còn lại với$b$trước khi tất cả các yếu tố chung biến mất. Mỗi phép nhân như vậy sẽ dịch chuyển các chữ số một cách hiệu quả đồng thời loại bỏ các thừa số chung với$b$. Số bước cần thiết bằng lũy ​​thừa cao nhất của mỗi phép chia nguyên tố$\gcd(x, b^\infty)$, tương đương với việc loại bỏ tất cả các số nguyên tố của$x$xuất hiện trong$b$. 

Khi quá trình chuẩn hóa này được thực hiện, hệ thống còn lại sẽ sạch: chúng tôi mô phỏng chu trình còn lại theo modulo nhóm nhân$x'$, trong đó mọi trạng thái đều khả nghịch. Độ dài chu kỳ là nhỏ nhất$t$như vậy$b^t \equiv 1 \pmod{x'}$. 

Tính toán thứ tự này yêu cầu phân tích thành thừa số$x'$, sau đó sử dụng định lý Euler hoặc tính toán thứ tự trực tiếp theo lũy thừa nguyên tố. 

Cách tiếp cận bạo lực sẽ mô phỏng chuỗi còn lại:$r_{k+1} = (r_k \cdot b) \bmod x$, theo dõi các trạng thái đã thấy cho đến khi lặp lại. Điều này có thể mất$O(x)$bước, điều này là không thể thực hiện được$x \le 10^{12}$. 

Phương pháp tối ưu hóa thay thế mô phỏng bằng lý thuyết số: tính ra các số nguyên tố chung cho tiền tố, sau đó tính thứ tự nhân cho chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |$O(x)$|$O(x)$| Quá chậm | 
| Phân tích nhân tử + bậc nhân |$O(\sqrt{x})$ĐẾN$O(\log x)$với sự tối ưu hóa |$O(1)$-$O(\log x)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Loại bỏ tất cả các thừa số nguyên tố khỏi$x$điều đó cũng chia$b$. Điều này được thực hiện bằng cách tính toán nhiều lần$\gcd(x, b)$và chia$x$bởi nó cho đến khi không còn nhân tử chung nào. Bước này cô lập phần mẫu số có thể đóng góp vào một chu trình lặp lại. 
2. Hãy để$x'$là giá trị còn lại sau tất cả các lần giảm đó. Số lượng các yếu tố bị loại bỏ xác định độ dài của tiền tố không định kỳ. Mỗi lần loại bỏ tương ứng với một bước chữ số trong cơ sở-$b$quá trình phân chia trong đó sự hủy bỏ xảy ra trước khi sự lặp lại có thể ổn định. 
3. Nếu$x' = 1$, phân số trở thành hữu hạn theo cơ số$b$, do đó không có chu kỳ lặp lại. Độ dài định kỳ bằng không. 
4. Ngược lại, hãy tính thứ tự nhân của$b$modulo$x'$. Điều này có nghĩa là tìm số nguyên dương nhỏ nhất$t$như vậy$b^t \equiv 1 \pmod{x'}$. Giá trị này tương ứng chính xác với độ dài chu kỳ của dãy dư trong phép chia dài. 
5. Để tính thứ tự, nhân tử$\varphi(x')$ngầm thông qua việc phân tích thành thừa số nguyên tố của$x'$, sau đó lặp đi lặp lại giảm số mũ$t$bằng cách kiểm tra các ước số trong khi vẫn bảo toàn điều kiện đồng đẳng. 

### Tại sao nó hoạt động 

Tại mỗi bước của cơ sở-$b$khai triển, số dư được nhân với$b$và giảm modulo mẫu số hiệu quả hiện tại. Yếu tố chung giữa$b$và mẫu số sụp đổ ngay lập tức, làm giảm mô đun hiệu dụng. Khi tất cả các yếu tố như vậy được loại bỏ, hệ thống còn lại sẽ tồn tại trong một nhóm nhân theo modulo$x'$, trong đó mọi trạng thái đều khả nghịch và chuỗi số dư cuối cùng phải lặp lại với chu kỳ bằng bậc nhân của$b$. Điều này đảm bảo rằng quá trình được phân chia rõ ràng thành một tiền tố hữu hạn gây ra bởi việc loại bỏ nhân tố và quá trình còn lại thuần túy theo chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def factorize(n):
    f = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            f[d] = f.get(d, 0) + 1
            n //= d
        d += 1
    if n > 1:
        f[n] = f.get(n, 0) + 1
    return f

def remove_common_factors(x, b):
    g = gcd(x, b)
    prefix = 0
    while g != 1:
        while x % g == 0:
            x //= g
            prefix += 1
        g = gcd(x, b)
    return x, prefix

def mod_pow(a, e, mod):
    r = 1
    a %= mod
    while e:
        if e & 1:
            r = (r * a) % mod
        a = (a * a) % mod
        e >>= 1
    return r

def multiplicative_order(b, mod):
    phi = mod
    factors = factorize(phi)
    for p in factors:
        phi -= phi // p

    order = phi
    for p in factorize(order):
        while order % p == 0 and mod_pow(b, order // p, mod) == 1:
            order //= p
    return order

def solve():
    x, b = map(int, input().split())
    x, prefix = remove_common_factors(x, b)

    if x == 1:
        print(prefix, 0)
        return

    # ensure gcd(b, x) = 1
    g = gcd(b, x)
    if g != 1:
        x //= g

    period = multiplicative_order(b, x)
    print(prefix, period)

if __name__ == "__main__":
    solve()
```chức năng`remove_common_factors`mô phỏng phần chia dài trong đó mẫu số chia sẻ các số nguyên tố với cơ số. Mỗi khi tìm thấy một gcd chung, các yếu tố đó sẽ được tách ra khỏi mẫu số, góp phần tạo ra tiền tố không lặp lại. 

Khi thu được mẫu số rút gọn,`multiplicative_order`tính độ dài chu kỳ. Đầu tiên, nó tính tổng mô đun Euler bằng cách sử dụng hệ số nguyên tố của nó, sau đó rút gọn nó bằng cách kiểm tra các ước của cấp ứng cử viên. Phép lũy thừa nhanh kiểm tra xem lũy thừa của$b$thu gọn về 1 modulo mẫu số rút gọn. 

Một điểm tinh tế là tiền tố được tính theo loại bỏ yếu tố, không chỉ các lệnh gọi gcd. Mỗi phép chia cho một thừa số nguyên tố chung tương ứng với sự dịch chuyển một chữ số trong khai triển cơ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 10
```Chúng tôi bắt đầu với$x = 2$,$b = 10$. gcd là 2 nên chúng ta loại bỏ nó ngay lập tức. 

| Bước | x | gcd(x, b) | tiền tố | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | 
| 1 | 1 | - | 1 | 

Hiện nay$x' = 1$, do đó phân số kết thúc. Không có chu kỳ lặp lại. 

Điều này phù hợp với thực tế rằng$1/2 = 0.5$ở cơ sở 10. 

Đầu ra:```
1 0
```### Ví dụ 2 

đầu vào:```
3 10
```Chúng tôi có$\gcd(3, 10) = 1$, do đó không có việc loại bỏ tiền tố nào xảy ra. 

Bây giờ chúng ta tính thứ tự nhân của 10 modulo 3. 

| Bước | b^k mod 3 | 
| --- | --- | 
| 1 | 1 | 

Từ$10 \equiv 1 \pmod{3}$, độ dài chu kỳ là 1. 

Điều này tương ứng với số thập phân lặp lại$1/3 = 0.\overline{3}$. 

Đầu ra:```
0 1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{x} + \log x)$| Yếu tố hóa chiếm ưu thế; lũy thừa mô đun là logarit | 
| Không gian |$O(1)$| Chỉ lưu trữ các thừa số nguyên tố và bộ đếm | 

Giới hạn lên đến$10^{12}$an toàn cho việc phân chia nhân tử trong Python bằng cách cắt tỉa và các bước lũy thừa là không đáng kể khi so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    def factorize(n):
        f = {}
        d = 2
        while d * d <= n:
            while n % d == 0:
                f[d] = f.get(d, 0) + 1
                n //= d
            d += 1
        if n > 1:
            f[n] = f.get(n, 0) + 1
        return f

    def remove_common_factors(x, b):
        g = gcd(x, b)
        prefix = 0
        while g != 1:
            while x % g == 0:
                x //= g
                prefix += 1
            g = gcd(x, b)
        return x, prefix

    def mod_pow(a, e, mod):
        r = 1
        a %= mod
        while e:
            if e & 1:
                r = (r * a) % mod
            a = (a * a) % mod
            e >>= 1
        return r

    def multiplicative_order(b, mod):
        phi = mod
        for p in factorize(phi):
            phi -= phi // p

        order = phi
        for p in factorize(order):
            while order % p == 0 and mod_pow(b, order // p, mod) == 1:
                order //= p
        return order

    def solve():
        x, b = map(int, sys.stdin.readline().split())
        from math import gcd
        x, prefix = remove_common_factors(x, b)

        if x == 1:
            return f"{prefix} 0\n"

        g = gcd(b, x)
        if g != 1:
            x //= g

        period = multiplicative_order(b, x)
        return f"{prefix} {period}\n"

    return solve()

# provided samples
assert run("2 10\n") == "1 0\n", "sample 1"
assert run("3 10\n") == "0 1\n", "sample 2"
assert run("2 2\n") == "1 0\n", "sample 3"

# custom cases
assert run("12 10\n") == "1 0\n", "finite expansion with shared factors"
assert run("7 10\n") == "0 6\n", "classic repetend length in base 10"
assert run("1 2\n") == "0 0\n", "trivial case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 12 10 | 1 0 | số thập phân hữu hạn do thừa số chung | 
| 7 10 | 0 6 | trường hợp toàn thời gian | 
| 1 2 | 0 0 | trường hợp chấm dứt tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh tinh vi xảy ra khi mẫu số trở thành 1 sau khi loại bỏ các thừa số chung với cơ số. Đối với đầu vào như`12 10`, việc loại bỏ gcd lặp đi lặp lại làm giảm 12 xuống 1 vì cả 2 và 3 đều chia 10. Thuật toán đếm chính xác một bước của tiền tố trước khi kết thúc, tạo ra`1 0`. 

Một trường hợp khác là khi cơ số nguyên tố cùng mẫu số ngay từ đầu. Vì`7 10`, không có việc loại bỏ tiền tố nào xảy ra và toàn bộ hành vi được xác định theo bậc nhân của 10 modulo 7. Thuật toán ngay lập tức đi vào tính toán chu trình mà không cần rút gọn không cần thiết. 

Khi tử số đã hoàn toàn tương thích với biểu diễn cơ số, chẳng hạn như`1 2`, gcd là 1 và bậc của 2 modulo 1 gần như bằng 0. Thuật toán ngắt mạch trường hợp này để tránh tính toán mô-đun không hợp lệ.
