---
title: "CF 102299F - Forbechenko v Rodvsky"
description: "Chúng ta được cho hai số nguyên dương (A) và (B), biểu thị phân số (A/B). Chúng ta có thể chọn bất kỳ cơ số nguyên nào (beta ge 2) và chúng ta muốn cơ số nhỏ nhất trong đó phân số này có biểu diễn hữu hạn sau điểm cơ số."
date: "2026-08-13T08:11:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "F"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 73
verified: true
draft: false
---

[CF 102299F - Forbechenko v Rodvsky](https://codeforces.com/problemset/problem/102299/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên dương (A) và (B), biểu thị phân số (A/B). Chúng ta có thể chọn bất kỳ cơ số nguyên nào (\beta \ge 2) và chúng ta muốn cơ số nhỏ nhất trong đó phân số này có biểu diễn hữu hạn sau điểm cơ số. Ví dụ: (1/3) lặp lại mãi mãi trong cơ số 10, nhưng nó chính xác là (0,1_3), vì vậy câu trả lời cho (A=1,B=3) là (3). Vấn đề ban đầu có (1 \le A,B \le 10^{18}), giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB. 

Bước hữu ích đầu tiên là giảm phân số. Đặt (g=\gcd(A,B)), (a=A/g) và (b=B/g). Chỉ có mẫu số rút gọn (b) mới quan trọng. Trong cơ số (\beta), một số hữu tỷ có biểu diễn phân số hữu hạn chính xác khi mẫu số rút gọn của nó chia một số lũy thừa của (\beta). Nếu (b) có hệ số nguyên tố 

[ 
b=p_1^{e_1}p_2^{e_2}\cdots p_k^{e_k}, 
] 

sau đó (b\mid\beta^t) đối với một số (t) chính xác khi mọi (p_i) chia (\beta). Số mũ (e_i) không quan trọng, bởi vì một khi (p_i\mid\beta), lũy thừa cao tùy ý của (p_i) chia lũy thừa đủ lớn của (\beta). 

Do đó, cơ sở nhỏ nhất có thể là 

[ 
\beta=p_1p_2\cdots p_k, 
] 

tích của các thừa số nguyên tố riêng biệt của mẫu số rút gọn. Nếu mẫu số trở thành (1), thì phân số là số nguyên và hữu hạn ở mọi cơ số, nên đáp án là (2). 

Giới hạn lớn của (10^{18}) là khó khăn thực sự. Việc lặp qua tất cả các cơ sở có thể có thể yêu cầu (10^{18}) ứng viên khi bản thân mẫu số rút gọn là số nguyên tố lớn. Ngay cả phép chia thử nghiệm thông thường của mẫu số cho đến căn bậc hai của nó cũng có thể yêu cầu khoảng (10^9) phép chia, vượt xa những gì giải pháp một giây có thể thực hiện được. Việc phân tích hệ số phải sử dụng thuật toán phân tích hệ số nguyên tuyến tính. 

Một số trường hợp cạnh rất dễ xử lý sai. Đối với đầu vào`1 1`, mẫu số rút gọn là (1), do đó kết quả đúng là`2`. Một giải pháp luôn trả về tích của các thừa số nguyên tố được phát hiện có thể vô tình trả về (1), đây không phải là cơ sở hợp lệ. 

Đối với đầu vào`2 4`, phân số giảm xuống còn (1/2), nên đáp án là`2`. Giải pháp lấy nhân tử (B) trước khi giảm phân số vẫn có thể áp dụng được ở đây, nhưng sự khác biệt trở nên có ý nghĩa đối với các đầu vào như`6 15`: phân số rút gọn là (2/5), nên đáp án là`5`, không phải là tích của các thừa số của mẫu số không rút gọn kết hợp với các thừa số không liên quan của tử số. 

Đối với đầu vào`1 12`, mẫu số là (2^2\cdot3). Câu trả lời là`6`, không`12`. Số mũ của số nguyên tố không cần phải xuất hiện ở cơ số. Một giải pháp bất cẩn xây dựng toàn bộ mẫu số thay vì căn thức của nó sẽ tạo ra câu trả lời sai. 

Đối với đầu vào`1 3`, câu trả lời là`3`. Điều này nắm bắt các triển khai chỉ kiểm tra các cơ sở quen thuộc như (2, 10) hoặc gây nhầm lẫn giữa biểu diễn thập phân hữu hạn với biểu diễn hữu hạn trong một số cơ sở tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử các căn cứ theo thứ tự tăng dần. Đối với mỗi ứng cử viên (\beta), hãy giảm mẫu số hiện tại theo mọi thừa số cũng chia hết (\beta). Nếu mẫu số cuối cùng có thể giảm xuống (1), thì phân số có biểu diễn hữu hạn trong cơ số đó. Điều này đúng vì quá trình đang kiểm tra chính xác xem mọi thừa số nguyên tố của mẫu số có xuất hiện trong cơ sở ứng cử viên hay không. 

Vấn đề là số lượng ứng viên. Xét (A=1) và (B=p), trong đó (p) là số nguyên tố lớn gần với (10^{18}). Mọi cơ sở từ (2) đến (p-1) đều thất bại, trong khi (p) thành công. Điều đó có nghĩa là lực lượng vũ phu có thể thực hiện khoảng (10^{18}) lượt kiểm tra ứng viên. Ngay cả khi mỗi lần kiểm tra chỉ mất vài thao tác máy thì điều này hoàn toàn không thể thực hiện được. 

Một giải pháp đơn giản hơn về mặt toán học là phân tích mẫu số rút gọn bằng cách thử mọi số nguyên từ (2) đến (\sqrt b). Điều này tốt hơn nhiều so với việc thử các cơ sở, nhưng đối với (b) gần (10^{18}), vòng lặp vẫn đạt khoảng (10^9) lần lặp. Giới hạn một giây cũng loại trừ điều này. 

Quan sát quan trọng là chúng ta không cần phân tích thành thừa số nguyên tố đầy đủ với số mũ. Chúng ta chỉ cần các thừa số nguyên tố riêng biệt. Tuy nhiên, việc tìm ra các thừa số riêng biệt đó cho một số nguyên 64 bit tùy ý vẫn là một vấn đề về hệ số nguyên. Công cụ thích hợp ở đây là hệ số hóa Rho của Pollard kết hợp với kiểm tra tính nguyên tố Miller-Rabin xác định. 

Miller-Rabin nhanh chóng cho chúng ta biết số còn lại có phải là số nguyên tố hay không. Nếu là hợp số, Rho của Pollard sẽ tìm ra thừa số không tầm thường nhanh hơn nhiều so với phép chia thử. Chúng ta phân tích đệ quy cả hai phần và nhân từng số nguyên tố riêng biệt chính xác một lần. 

Lực lượng vũ phu hoạt động vì cơ số có giá trị chính xác khi nó chứa mọi thừa số nguyên tố của mẫu số. Việc này không thành công vì việc khám phá cơ sở hợp lệ đầu tiên có thể yêu cầu kiểm tra về cơ bản toàn bộ phạm vi (10^{18}). Nhận xét rằng chỉ có các thừa số nguyên tố riêng biệt mới quan trọng cho phép chúng ta thay thế việc tìm kiếm trên các cơ số bằng một phép phân tích thành thừa số của một số nguyên 64 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(B\log B)) trong trường hợp xấu nhất | (O(1)) | Quá chậm | 
| Phòng xét xử | (O(\sqrt B)) phân chia | (O(1)) | Quá chậm | 
| Pollard Rho + Miller-Rabin | Công việc phân tích nhân tử đại khái (O(B^{1/4})) dự kiến ​​cho các bán nguyên tố cứng | (O(\log B)) đệ quy | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính (g=\gcd(A,B)) và thay (B) bằng (B/g). Điều này mang lại mẫu số của phân số ở dạng thấp nhất, đây là phần duy nhất liên quan đến việc biểu diễn có kết thúc hay không. 
2. Nếu mẫu số rút gọn là (1), in ra`2`. Phân số là một số nguyên nên nó có biểu diễn hữu hạn ở mọi cơ số và (2) là cơ số nhỏ nhất được phép. 
3. Phân tích mẫu số rút gọn bằng cách sử dụng Miller-Rabin và Pollard's Rho. Miller-Rabin xử lý các số nguyên tố ngay lập tức, trong khi Rho của Pollard cung cấp ước số thích hợp bất cứ khi nào số hiện tại là hợp số. 
4. Thu thập các thừa số nguyên tố phân biệt. Nếu một thừa số xuất hiện nhiều lần, chẳng hạn như (2^5), chỉ lưu trữ (2) một lần. Cơ số chỉ cần chứa chính số nguyên tố chứ không phải số mũ đầy đủ của nó. 
5. Nhân tất cả các thừa số nguyên tố phân biệt với nhau và in kết quả. Tích này chia hết cho mọi thừa số nguyên tố của mẫu số rút gọn và không có số nguyên dương nhỏ hơn nào có thể chia hết cho tất cả chúng cùng một lúc. 

### Tại sao nó hoạt động 

Gọi phân số rút gọn là (a/b). Biểu diễn phân số hữu hạn cơ sở-(\beta) với (k) chữ số có dạng (x/\beta^k) cho một số nguyên (x), do đó (a/b=x/\beta^k), ngụ ý (b\mid\beta^k). Ngược lại, nếu (b\mid\beta^k) thì (a/b) có thể được viết bằng mẫu số (\beta^k), cho một biểu diễn hữu hạn. Do đó, vấn đề chính xác là tìm ra (\beta) nhỏ nhất có thừa số nguyên tố bao gồm mọi thừa số nguyên tố của (b). Tích của các số nguyên tố riêng biệt đó là số nguyên nhỏ nhất như vậy, do đó kết quả của thuật toán là tối ưu. 

## Giải pháp Python```python
import sys
import math
import random

input = sys.stdin.readline

# Deterministic for every 64-bit integer.
MR_BASES = (2, 325, 9375, 28178, 450775, 9780504, 1795265022)

def is_prime(n: int) -> bool:
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

    for a in MR_BASES:
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

def pollard_rho(n: int) -> int:
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
                limit = min(m, r - k)

                for _ in range(limit):
                    y = (y * y + c) % n
                    q = q * abs(x - y) % n

                g = math.gcd(q, n)
                k += limit

            r <<= 1

        if g == n:
            while True:
                ys = (ys * ys + c) % n
                g = math.gcd(abs(x - ys), n)
                if g > 1:
                    break

        if g != n:
            return g

def factor(n: int, factors: list[int]) -> None:
    if n == 1:
        return

    if is_prime(n):
        factors.append(n)
        return

    d = pollard_rho(n)
    factor(d, factors)
    factor(n // d, factors)

def solve() -> None:
    A, B = map(int, input().split())

    B //= math.gcd(A, B)

    if B == 1:
        print(2)
        return

    factors = []
    factor(B, factors)

    answer = 1
    for p in set(factors):
        answer *= p

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ giảm phân số bằng`math.gcd`, phù hợp với bước thuật toán đầu tiên. Chỉ chia mẫu số là đủ vì chúng ta không cần tử số rút gọn cho phần còn lại của phép tính.`is_prime`sử dụng bộ cơ sở Miller-Rabin xác định đủ cho tất cả các số nguyên bên dưới (2^{64}), bao gồm toàn bộ phạm vi đầu vào. Các số nguyên có độ chính xác tùy ý của Python cũng giúp phép nhân mô-đun an toàn mà không cần xử lý 128 bit đặc biệt.`pollard_rho`sử dụng biến thể theo đợt kiểu Brent của Pollard's Rho. Chuỗi được tạo bởi (f(x)=x^2+c\pmod n). Khi hai giá trị được tạo ra bằng modulo và là thừa số nguyên tố không xác định, hiệu của chúng có gcd không tầm thường với (n). Việc tính toán gcd sau một loạt sai phân sẽ làm giảm số lượng các thao tác gcd tương đối tốn kém. 

Biến`q`lưu trữ một tích của một số sai phân tuyệt đối modulo (n). Nếu bất kỳ một trong những khác biệt đó chứa hệ số (n), thì gcd của toàn bộ sản phẩm có (n) có thể tiết lộ điều đó. Nếu lô vô tình đưa ra số đầy đủ (n), mã sẽ quay lại kiểm tra từng điểm khác biệt. 

Đệ quy`factor`chức năng dừng ngay lập tức đối với số nguyên tố. Mặt khác, nó sẽ chia số bằng cách sử dụng Pollard's Rho và xử lý đệ quy cả hai yếu tố. Các yếu tố lặp lại được cho phép trong danh sách tạm thời vì kết quả cuối cùng`set`loại bỏ tính đa dạng của chúng. 

Phép nhân cuối cùng có chủ ý xảy ra sau khi loại bỏ trùng lặp. Ví dụ: nếu mẫu số là (72=2^3\cdot3^2), danh sách thừa số có thể chứa một số bản sao của`2`Và`3`, nhưng câu trả lời phải là (2\cdot3=6), không phải (72). 

Không có vấn đề tràn số nguyên trong Python. Trong triển khai C++, phép nhân mô-đun bên trong Pollard's Rho cần loại trung gian 128 bit cho phạm vi đầu vào này. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`1 3`. Mẫu số đã được giảm và`3`là nguyên tố. 

| Bước | Mẫu số rút gọn | Các yếu tố chính được tìm thấy | Trả lời | 
| --- | --- | --- | --- | 
| Giảm phân số | 3 | không | 1 | 
| Kiểm tra tính nguyên thủy | 3 | 3 | 1 | 
| Xây dựng triệt để | 3 | {3} | 3 | 

Câu trả lời là`3`. Trong cơ số (3), phân số chính xác là (0,1_3), do đó việc biểu diễn kết thúc. 

Đối với Mẫu 2, đầu vào là`3 4`. Mẫu số là (4=2^2). 

| Bước | Mẫu số rút gọn | Các yếu tố chính được tìm thấy | Trả lời | 
| --- | --- | --- | --- | 
| Giảm phân số | 4 | không | 1 | 
| Yếu tố 4 | 4 | 2, 2 | 1 | 
| Xóa trùng lặp | 4 | {2} | 1 | 
| Xây dựng triệt để | 4 | {2} | 2 | 

Câu trả lời là`2`. Mặc dù mẫu số chứa (2^2) nhưng cơ số chỉ cần chứa số nguyên tố (2). Vì (4\mid2^2), phân số có biểu diễn nhị phân hữu hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​(O(B^{1/4}\operatorname{polylog}B)) cho các vật liệu tổng hợp 64-bit khó | Rho của Pollard tìm thấy các thừa số không tầm thường nhanh hơn đáng kể so với phép chia thử, trong khi Miller-Rabin xử lý các số nguyên tố nhanh chóng | 
| Không gian | (O(\log B)) | Độ sâu của hệ số đệ quy là logarit và chỉ danh sách hệ số được lưu trữ | 

Với (B\le10^{18}), phép chia thử nghiệm có thể yêu cầu khoảng (10^9) lần lặp, trong khi Pollard's Rho được thiết kế cho chính xác phạm vi phân tích hệ số 64-bit này. Thuật toán cũng chỉ thực hiện một lượng đệ quy logarit và sử dụng rất ít bộ nhớ, do đó, nó phù hợp với giới hạn bộ nhớ 256 MB đã nêu và phù hợp với mục tiêu một giây với cách triển khai được tối ưu hóa. 

## Trường hợp thử nghiệm```python
# This test harness contains the same algorithm as the submitted solution.
import sys
import io
import math
import random

MR_BASES = (2, 325, 9375, 28178, 450775, 9780504, 1795265022)

def is_prime(n):
    if n < 2:
        return False

    for p in (2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37):
        if n % p == 0:
            return n == p

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    for a in MR_BASES:
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
                limit = min(m, r - k)

                for _ in range(limit):
                    y = (y * y + c) % n
                    q = q * abs(x - y) % n

                g = math.gcd(q, n)
                k += limit

            r <<= 1

        if g == n:
            while True:
                ys = (ys * ys + c) % n
                g = math.gcd(abs(x - ys), n)
                if g > 1:
                    break

        if g != n:
            return g

def factor(n, factors):
    if n == 1:
        return

    if is_prime(n):
        factors.append(n)
        return

    d = pollard_rho(n)
    factor(d, factors)
    factor(n // d, factors)

def solve_data(inp):
    A, B = map(int, inp.split())

    B //= math.gcd(A, B)

    if B == 1:
        return "2\n"

    factors = []
    factor(B, factors)

    ans = 1
    for p in set(factors):
        ans *= p

    return str(ans) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided samples
assert run("1 3\n") == "3\n", "sample 1"
assert run("3 4\n") == "2\n", "sample 2"

# Minimum-size input
assert run("1 1\n") == "2\n", "integer fraction"

# All factors have powers, so only distinct primes matter
assert run("1 12\n") == "6\n", "radical of 12"

# Numerator and denominator must be reduced first
assert run("6 15\n") == "5\n", "reduction by gcd"

# Maximum-size denominator from the stated range
assert run("1 1000000000000000000\n") == "10\n", "maximum-size denominator"

# Large all-equal values, again producing an integer
assert run("1000000000000000000 1000000000000000000\n") == "2\n", "large equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Đầu vào và mẫu số tối thiểu bằng (1) | 
|`1 12`|`6`| Các thừa số nguyên tố lặp lại không được nhân nhiều lần | 
|`6 15`|`5`| Việc giảm gcd phải xảy ra trước khi nhân tử hóa | 
|`1 1000000000000000000`|`10`| Mẫu số có kích thước tối đa và hệ số tổng hợp lớn | 
|`1000000000000000000 1000000000000000000`|`2`| Tử số và mẫu số lớn bằng nhau | 

## Vỏ cạnh 

cho`1 1`, thuật toán sẽ tính (g=1), để lại mẫu số là (1). Nó lập tức quay lại`2`. Không thử phân tích thành thừa số vì không có thừa số nguyên tố nào được đưa vào cơ số. 

Vì`2 4`, gcd là (2), do đó mẫu số rút gọn trở thành (2). Miller-Rabin công nhận nó là số nguyên tố và căn thức là (2). Đầu ra là`2`. Điều này chứng tỏ tại sao việc phân tích nhân tử phải sử dụng mẫu số rút gọn thay vì sử dụng mẫu số ban đầu một cách mù quáng. 

Vì`6 15`, gcd là (3), cho phân số rút gọn (2/5). Thừa số nguyên tố duy nhất của mẫu số rút gọn là (5), do đó đầu ra là`5`. Các thừa số của tử số không ảnh hưởng đến câu trả lời. 

Vì`1 12`, mẫu số là (2^2\cdot3). Thuật toán có thể khám phá các yếu tố như`2, 2, 3`, Nhưng`set(factors)`thay đổi chúng thành`{2,3}`trước khi nhân. Câu trả lời là`6`. Cơ số của (6) có hiệu quả vì (12\mid6^2). 

Vì`1 3`, bản thân mẫu số là số nguyên tố. Miller-Rabin xác định`3`ngay lập tức nên Rho của Pollard là không cần thiết. Câu trả lời là`3`, điều này cũng cho thấy tại sao cơ số hợp lệ nhỏ nhất không bị giới hạn ở lũy thừa hai hoặc mười. 

Vì`1 1000000000000000000`, mẫu số là 

[ 
10^{18}=2^{18}\cdot5^{18}. 
] 

Chỉ có các số nguyên tố riêng biệt (2) và (5) quan trọng, vì vậy câu trả lời là`10`. Trường hợp này thực hiện cả phép tính số học số nguyên lớn và sự phân biệt giữa số mũ nguyên tố và thừa số nguyên tố riêng biệt. 

Đối với các giá trị lớn bằng nhau như`1000000000000000000 1000000000000000000`, phép khử tạo ra mẫu số (1), do đó thuật toán trả về`2`ngay lập tức. Điều này tránh việc phân tích thành thừa số không cần thiết của một số nguyên lớn và xác nhận rằng một phân số nguyên là hợp lệ trong cơ số nhỏ nhất có thể.
