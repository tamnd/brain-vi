---
title: "CF 102365H - Trí tuệ cổ xưa"
description: "Gọi tuổi của David là (d), tuổi của Aram là (a) và gọi số nguyên đã cho là (C). Cuộc trò chuyện cho chúng ta biết rằng lập phương tuổi của David và nhân với (C) sẽ tạo ra chính xác bình phương tuổi của Aram: [ C d^3 = a^2. ] Cả hai độ tuổi đều là số nguyên dương."
date: "2026-08-12T23:56:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "H"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 85
verified: true
draft: false
---

[CF 102365H - Trí tuệ cổ đại](https://codeforces.com/problemset/problem/102365/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Gọi tuổi của David là (d), tuổi của Aram là (a) và gọi số nguyên đã cho là (C). Cuộc trò chuyện cho chúng ta biết rằng lập phương tuổi của David và nhân với (C) sẽ tạo ra chính xác bình phương tuổi của Aram: 

[ 
Cd^3 = a^2. 
] 

Cả hai độ tuổi đều là số nguyên dương. Chúng ta cần giá trị nhỏ nhất có thể có của (d) cho (C) đã cho. 

Khó khăn chính là (C) có thể lớn bằng (2^{63}-1), xấp xỉ (9.2\cdot10^{18}). Việc tìm kiếm trực tiếp theo độ tuổi có thể là không thể, và ngay cả phép chia thử thông thường lên tới (\sqrt C) cũng có thể yêu cầu khoảng (2^{31}) phép chia trong trường hợp xấu nhất. Giới hạn một giây loại trừ mọi thứ từ xa gần một tỷ lần lặp, do đó, bản thân việc phân tích nhân tử phải nhanh hơn đáng kể so với phép chia thử. 

Phương trình được điều khiển hoàn toàn bởi số mũ nguyên tố. Giả sử 

[ 
C=\prod p_i^{e_i} 
] 

và 

[ 
d=\prod p_i^{f_i}. 
] 

Khi đó số mũ của (p_i) trong (Cd^3) là 

[ 
e_i+3f_i. 
] 

Để (Cd^3) là số chính phương thì mọi số mũ như vậy phải là số chẵn. Vì (3) là số lẻ nên điều này có nghĩa là 

[ 
e_i+f_i\equiv0\pmod2, 
] 

vì vậy (f_i) phải có cùng tính chẵn lẻ với (e_i). Do đó, lựa chọn nhỏ nhất có thể là (f_i=1) khi (e_i) là số lẻ và (f_i=0) khi (e_i) là số chẵn. 

Do đó, câu trả lời là tích của chính xác các thừa số nguyên tố của (C) có số mũ là số lẻ. Đây là hạt nhân bình phương của (C). 

Một số trường hợp cạnh rất dễ xử lý sai. Đối với đầu vào`1`, phép phân tích thành nhân tử không có thừa số nguyên tố có số mũ lẻ, nên đáp án là`1`. Việc triển khai bạo lực bắt đầu tìm kiếm từ độ tuổi`2`sẽ từ chối không chính xác mức tối thiểu hợp lệ. 

Đối với đầu vào`8`, ta có (8=2^3). Số mũ của (2) là số lẻ nên đáp án là`2`. Thật vậy, 

[ 
8\cdot2^3=64=8^2. 
] 

Việc triển khai chỉ đơn giản lấy các thừa số nguyên tố riêng biệt cũng sẽ trả về`2`ở đây, nhưng ý tưởng đó thất bại khi nhập liệu`12`. Vì (12=2^2\cdot3) nên chỉ có số mũ của (3) là số lẻ nên đáp án là`3`. Sử dụng tất cả các thừa số nguyên tố riêng biệt sẽ tạo ra sai số`6`. 

Trường hợp tinh vi thứ hai là số nguyên tố lớn. Nếu (C) là số nguyên tố thì số mũ duy nhất của nó là (1), nên đáp án là (C). Việc triển khai dựa trên phép chia thử nghiệm lên tới (\sqrt C) có thể mất hàng tỷ lần lặp để chứng minh rằng số đó là số nguyên tố. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử tuổi của David bắt đầu từ (1), tính (Cd^3) và kiểm tra xem kết quả có phải là số chính phương hoàn hảo hay không. Điều này đúng vì (d) thành công đầu tiên chính xác là độ tuổi tối thiểu. Tuy nhiên, không có giới hạn trên nhỏ hữu ích nào cho câu trả lời. Đặc biệt, khi (C) là số nguyên tố lớn thì bản thân câu trả lời là xấp xỉ (9\cdot10^{18}), nên việc tìm kiếm qua các độ tuổi ứng cử viên là vô vọng. 

Thay vào đó chúng ta có thể suy luận về số mũ nguyên tố. Với mọi số nguyên tố (p), số mũ trong (Cd^3) phải là số chẵn. Nếu (p) xuất hiện với số lần lẻ trong (C), chúng ta cần một bản sao của (p) trong (d). Nếu (p) xuất hiện số lần chẵn thì chúng ta không cần sao chép. Việc thêm nhiều bản sao chỉ làm cho (d) lớn hơn và không thể giúp giảm thiểu nó. 

Do đó, vấn đề đã được rút gọn thành việc tìm tính chẵn lẻ của số mũ của mọi phép chia số nguyên tố (C). Phân tích nhân tử (C) bằng phép chia thử vẫn còn quá chậm vì (C) có thể tiếp cận (2^{63}). Công cụ thích hợp cho các số nguyên 64 bit tùy ý là kiểm tra tính nguyên tố Miller-Rabin xác định kết hợp với hệ số Pollard-Rho. 

Miller-Rabin cho phép chúng ta nhanh chóng nhận ra thừa số nguyên tố, trong khi Pollard-Rho tìm ra thừa số không tầm thường của một hợp số mà không cần quét mọi ước số có thể có. Đối với đầu vào 64-bit, một tập hợp cố định các cơ sở Miller-Rabin làm cho việc kiểm tra tính nguyên tố trở nên xác định. Sau khi phân tích đệ quy (C), chúng ta đếm số lần xuất hiện của mỗi số nguyên tố và nhân số lần xuất hiện với số lẻ. 

Phương pháp vũ lực có hiệu quả vì mọi độ tuổi có thể đều có thể được kiểm tra trực tiếp, nhưng nó thất bại vì bản thân câu trả lời có thể rất lớn. Việc quan sát số mũ làm giảm vấn đề toán học thành phân tích nhân tử và Pollard-Rho giảm phân tích nhân tử đó thành một khối lượng công việc thực tế cho số nguyên 63 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua các lứa tuổi | (O(d)) | (O(1)) | Quá chậm | 
| Phòng thử nghiệm | (O(\sqrt C)) | (O(1)) | Quá chậm đối với (C\approx2^{63}) | 
| Miller-Rabin + Pollard-Rho | Sub-(\sqrt C) dự kiến, thực tế đối với số nguyên 63 bit | (O(\log C)) đệ quy | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (C). Nếu (C=1), quay lại ngay`1`, bởi vì (1\cdot1^3=1^2). 
2. Sử dụng xác định Miller-Rabin để xác định xem một số có phải là số nguyên tố hay không. Đối với các giá trị bên dưới (2^{64}), các cơ số (2,325,9375,28178,450775,9780504,1795265022) là đủ cho kiểm tra xác định. 
3. Sử dụng Pollard-Rho để tìm ước số không cần thiết của mọi hợp số. Nếu số hiện tại là số nguyên tố, hãy lưu nó. Ngược lại, chia nó thành ước số được trả về bởi Pollard-Rho và thương số tương ứng, sau đó phân tích cả hai theo cách đệ quy. 
4. Đếm xem mỗi số nguyên tố xuất hiện bao nhiêu lần trong phân tích nhân tử của (C). Sự bình đẳng là tất cả những gì quan trọng. Ví dụ: nếu phân tích nhân tử chứa (2^4), bốn bản sao của (2) sẽ bị hủy khỏi yêu cầu vì số mũ đã chẵn. Nếu nó chứa (7^3), thì một bản sao của (7) phải được đưa vào tuổi của David. 
5. Nhân mọi số nguyên tố có số mũ là số lẻ. Gọi sản phẩm này là (d). Đây là tuổi David nhỏ nhất có thể có vì mọi số nguyên tố có số mũ lẻ trong (C) phải xuất hiện ít nhất một lần trong (d), trong khi các số nguyên tố có số mũ chẵn hoàn toàn không cần phải xuất hiện. 
6. In (d). 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ số nguyên tố (p) nào, với số mũ (e) ở (C) và số mũ (f) ở thời đại David. Số mũ của nó trong (Cd^3) là (e+3f). Vì một hình vuông hoàn hảo chỉ có số mũ nguyên tố chẵn nên chúng ta cần (e+3f) là số chẵn. Vì (3) là số lẻ nên điều này tương đương với (e+f) là số chẵn. Do đó (f) phải có cùng tính chẵn lẻ như (e). 

Khi (e) chẵn thì (f) hợp lệ nhỏ nhất là (0). Khi (e) lẻ thì (f) hợp lệ nhỏ nhất là (1). Những lựa chọn này độc lập với mọi số nguyên tố, vì vậy tích của chúng có độ tuổi nhỏ nhất có thể trên toàn cầu. Giai đoạn nhân tử hóa tìm thấy chính xác các số mũ này và phép nhân cuối cùng xây dựng chính xác mức tối thiểu đó. 

## Giải pháp Python```python
import sys
import random
import math

input = sys.stdin.readline

def is_prime(n):
    if n < 2:
        return False

    small_primes = (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37)
    for p in small_primes:
        if n % p == 0:
            return n == p

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    bases = (
        2,
        325,
        9375,
        28178,
        450775,
        9780504,
        1795265022,
    )

    for a in bases:
        if a % n == 0:
            continue

        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue

        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False

    return True

def pollard_rho(n):
    if n % 2 == 0:
        return 2
    if n % 3 == 0:
        return 3

    while True:
        y = random.randrange(1, n - 1)
        c = random.randrange(1, n - 1)
        m = 128

        g = 1
        r = 1
        q = 1

        while g == 1:
            x = y

            for _ in range(r):
                y = (y * y + c) % n

            k = 0
            while k < r and g == 1:
                ys = y
                for _ in range(min(m, r - k)):
                    y = (y * y + c) % n
                    q = q * abs(x - y) % n

                g = math.gcd(q, n)
                k += m

            r <<= 1

        if g == n:
            while True:
                ys = (ys * ys + c) % n
                g = math.gcd(abs(x - ys), n)
                if g > 1:
                    break

        if g != n:
            return g

def factor(n, result):
    if n == 1:
        return

    if is_prime(n):
        result.append(n)
        return

    d = pollard_rho(n)
    factor(d, result)
    factor(n // d, result)

def solve():
    c = int(input())

    if c == 1:
        print(1)
        return

    factors = []
    factor(c, factors)
    factors.sort()

    answer = 1
    i = 0

    while i < len(factors):
        j = i + 1
        while j < len(factors) and factors[j] == factors[i]:
            j += 1

        if (j - i) & 1:
            answer *= factors[i]

        i = j

    print(answer)

if __name__ == "__main__":
    solve()
```các`is_prime`đầu tiên hàm sẽ loại bỏ một tập hợp các trường hợp nguyên tố nhỏ. Điều này làm cho việc xử lý các yếu tố nhỏ thông thường trở nên rẻ hơn và cũng tránh được công việc Miller-Rabin không cần thiết. 

Kiểm tra tính nguyên tố còn lại ghi (n-1) là (d2^s) với (d) lẻ. Mỗi cơ sở Miller-Rabin kiểm tra xem số này có hoạt động giống số nguyên tố theo lũy thừa mô-đun hay không. Bảy cơ số cố định được sử dụng ở đây là một tập hợp xác định cho tất cả các số nguyên bên dưới (2^{64}), bao gồm toàn bộ phạm vi đầu vào. 

các`pollard_rho`hàm tìm kiếm một yếu tố bằng cách sử dụng phép truy hồi giả ngẫu nhiên 

[ 
x_{i+1}=x_i^2+c\pmod n. 
] 

Gcd của sự khác biệt giữa hai chuỗi có thể tiết lộ thừa số của (n). Việc triển khai sử dụng biến thể theo đợt của Brent, giúp giảm số lượng lệnh gọi gcd đắt tiền so với vòng lặp Pollard-Rho đơn giản. 

Đệ quy`factor`Hàm có điều kiện dừng đơn giản. Một số nguyên tố được thêm vào ngay lập tức. Một số tổng hợp được chia thành hai số nhỏ hơn và cả hai đều được phân tích thành nhân tử đệ quy. Vì mỗi nhánh đệ quy đều giảm số lượng được phân tích thành nhân tử nên quá trình đệ quy kết thúc. 

Sau khi sắp xếp các thừa số, các số nguyên tố bằng nhau tạo thành các nhóm liền kề. Mã đếm từng nhóm và nhân số nguyên tố thành câu trả lời chính xác khi số của nó là số lẻ. Việc sắp xếp ở đây rất thuận tiện vì nó tránh được việc duy trì một từ điển riêng biệt và làm cho phép tính chẵn lẻ trở nên đơn giản. 

Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn khi tính toán câu trả lời cuối cùng hoặc các sản phẩm mô-đun trung gian. Phép nhân mô-đun bên trong Pollard-Rho cũng an toàn vì Python tự động mở rộng bộ nhớ số nguyên. 

## Ví dụ đã hoạt động 

Câu lệnh ban đầu được cung cấp không chứa các giá trị đầu vào và đầu ra mẫu cụ thể, vì vậy các dấu vết sau đây sử dụng hai đầu vào nhỏ thực hiện các mẫu số mũ khác nhau. 

Với (C=12), hệ số phân tích là (2^2\cdot3). Số mũ của (2) là số chẵn, trong khi số mũ của (3) là số lẻ. 

| Sân khấu | Các yếu tố được tìm thấy | Nguyên tố hiện tại | Đếm | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | không | không | 1 | 
| Nhân tố hóa | 2, 2, 3 | không | không | 1 | 
| Nhóm 2 | 2, 2, 3 | 2 | 2 | 1 | 
| Nhóm 3 | 2, 2, 3 | 3 | 1 | 3 | 
| Kết thúc | 2, 2, 3 | không | không | 3 | 

Tuổi kết quả là`3`. Kiểm tra phương trình ban đầu sẽ cho (12\cdot3^3=324=18^2). Dấu vết cho thấy tại sao việc sử dụng các thừa số nguyên tố riêng biệt sẽ không chính xác, vì thừa số lặp lại (2^2) không đóng góp gì cho câu trả lời. 

Với (C=72), ta có 

[ 
72=2^3\cdot3^2. 
] 

Chỉ có số mũ của (2) là số lẻ. 

| Sân khấu | Các yếu tố được tìm thấy | Nguyên tố hiện tại | Đếm | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | không | không | 1 | 
| Nhân tố hóa | 2, 2, 2, 3, 3 | không | không | 1 | 
| Nhóm 2 | 2, 2, 2, 3, 3 | 2 | 3 | 2 | 
| Nhóm 3 | 2, 2, 2, 3, 3 | 3 | 2 | 2 | 
| Kết thúc | 2, 2, 2, 3, 3 | không | không | 2 | 

Câu trả lời là`2`, bởi vì (72\cdot2^3=576=24^2). Dấu vết xác nhận rằng số mũ lẻ đóng góp chính xác một bản sao của số nguyên tố của nó, bất kể số mũ đó là (1), (3), (5) hay lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Sub-(\sqrt C) dự kiến, thực tế đối với số nguyên 63 bit | Miller-Rabin thực hiện một số lượng lũy ​​thừa mô-đun không đổi, trong khi Pollard-Rho tìm các thừa số bằng cách sử dụng chuỗi ngẫu nhiên | 
| Không gian | (O(\log C)) | Phép đệ quy phân tích nhân tử có độ sâu logarit và có tối đa (O(\log C)) thừa số nguyên tố được tính bằng bội số | 

Sự khác biệt quan trọng so với phép chia thử là thuật toán không bao giờ quét tất cả các số nguyên lên tới (\sqrt C). Vì (C<2^{63}), Miller-Rabin xác định xử lý tính nguyên tố một cách đáng tin cậy và Pollard-Rho được thiết kế chính xác để phân tích các số nguyên có kích thước này. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn 1024 MB. 

## Trường hợp thử nghiệm 

Câu lệnh không cung cấp các giá trị mẫu thực tế, vì vậy bộ thử nghiệm bên dưới sử dụng cùng hai ví dụ được xây dựng từ các dấu vết đã xử lý cộng với các trường hợp ranh giới và giá trị lớn.```python
# helper: run solution on input string, return output string
import sys
import io
import random
import math

def is_prime(n):
    if n < 2:
        return False

    small_primes = (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37)
    for p in small_primes:
        if n % p == 0:
            return n == p

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    bases = (
        2,
        325,
        9375,
        28178,
        450775,
        9780504,
        1795265022,
    )

    for a in bases:
        if a % n == 0:
            continue

        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue

        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False

    return True

def pollard_rho(n):
    if n % 2 == 0:
        return 2
    if n % 3 == 0:
        return 3

    while True:
        y = random.randrange(1, n - 1)
        c = random.randrange(1, n - 1)
        m = 128

        g = 1
        r = 1
        q = 1

        while g == 1:
            x = y

            for _ in range(r):
                y = (y * y + c) % n

            k = 0
            while k < r and g == 1:
                ys = y

                for _ in range(min(m, r - k)):
                    y = (y * y + c) % n
                    q = q * abs(x - y) % n

                g = math.gcd(q, n)
                k += m

            r <<= 1

        if g == n:
            while True:
                ys = (ys * ys + c) % n
                g = math.gcd(abs(x - ys), n)
                if g > 1:
                    break

        if g != n:
            return g

def factor(n, result):
    if n == 1:
        return

    if is_prime(n):
        result.append(n)
        return

    d = pollard_rho(n)
    factor(d, result)
    factor(n // d, result)

def solve_value(c):
    if c == 1:
        return 1

    factors = []
    factor(c, factors)
    factors.sort()

    answer = 1
    i = 0

    while i < len(factors):
        j = i + 1
        while j < len(factors) and factors[j] == factors[i]:
            j += 1

        if (j - i) & 1:
            answer *= factors[i]

        i = j

    return answer

def run(inp: str) -> str:
    return str(solve_value(int(inp.strip()))) + "\n"

# constructed samples
assert run("12") == "3\n", "sample-like case 1"
assert run("72") == "2\n", "sample-like case 2"

# minimum input
assert run("1") == "1\n", "C = 1"

# all prime exponents even
assert run("36") == "1\n", "36 = 2^2 * 3^2"

# all distinct prime exponents are odd
assert run("30") == "30\n", "30 = 2 * 3 * 5"

# boundary near 2^63
assert run("9223372036854775807") == "188232082384791343\n", "2^63 - 1"

# large power with an even exponent
assert run("4611686018427387904") == "1\n", "2^62"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào nhỏ nhất có thể và tập hệ số mũ lẻ trống | 
|`36`|`1`| Mọi số mũ nguyên tố đều là số chẵn | 
|`30`|`30`| Một số số nguyên tố khác nhau đều có số mũ lẻ | 
|`9223372036854775807`|`188232082384791343`| Giá trị ranh giới 63-bit lớn và hệ số hóa không tầm thường | 
|`4611686018427387904`|`1`| Công suất rất lớn với số mũ nguyên tố chẵn | 

## Vỏ cạnh 

Với (C=1), đầu vào là`1`. Không có thừa số nguyên tố nào cả, vì vậy tích của các số nguyên tố có số mũ lẻ là tích rỗng, tức là`1`. Thuật toán xử lý việc này trước khi gọi Pollard-Rho và in`1`. 

Với (C=12), hệ số phân tích là (2^2\cdot3). Giai đoạn nhân tố hóa tạo ra`2, 2, 3`. Nhóm chứa hai bản sao của`2`có kích thước chẵn, vì vậy`2`bị loại trừ. Nhóm chứa một bản sao của`3`có kích thước lẻ, vì vậy`3`được bao gồm. Đầu ra là`3`. 

Với (C=8), hệ số phân tích là (2^3). Nhóm duy nhất có kích thước lẻ, vì vậy câu trả lời trở thành`2`. Phương trình thu được là (8\cdot2^3=64=8^2). Điều này mắc phải sai lầm phổ biến khi coi câu trả lời là thứ gì đó dựa trên cỡ số của (C) thay vì tính chẵn lẻ của các số mũ nguyên tố của nó. 

Đối với đầu vào biên lớn`9223372036854775807`, tức là (2^{63}-1), việc phân tích thành nhân tử là 

[ 
7^2\cdot73\cdot127\cdot337\cdot92737\cdot649657. 
] 

Số mũ của`7`là số chẵn, trong khi mọi số mũ hiển thị khác đều là số lẻ. Do đó, thuật toán sẽ loại bỏ hệ số (7^2) và trả về 

[ 
73\cdot127\cdot337\cdot92737\cdot649657 
=188232082384791343. 
] 

Trường hợp này chứng tỏ tại sao việc chia thử là không phù hợp. Mặc dù câu trả lời cuối cùng thu được từ một lượng thông tin phân tích nhân tử tương đối nhỏ, việc chứng minh các thừa số bằng cách quét mọi ước số lên đến (\sqrt C) sẽ yêu cầu khoảng (3\cdot10^9) phép chia ứng viên, vượt xa ngân sách thời gian dự định.
