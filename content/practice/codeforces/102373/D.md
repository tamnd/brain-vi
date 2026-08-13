---
title: "CF 102373D - Tập hợp con tốt"
description: "Chúng ta có một mảng gồm (n) số nguyên dương. Chúng ta có thể chọn bất kỳ tập con nào gồm các phần tử của nó và tập con đó được coi là tốt khi ước số chung lớn nhất của tất cả các giá trị được chọn lớn hơn (1). Nhiệm vụ là tìm số phần tử tối đa có thể có trong một tập hợp con như vậy."
date: "2026-08-12T22:55:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "D"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 206
verified: false
draft: false
---

[CF 102373D - Tập hợp con tốt](https://codeforces.com/problemset/problem/102373/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng gồm (n) số nguyên dương. Chúng ta có thể chọn bất kỳ tập con nào gồm các phần tử của nó và tập con đó được coi là tốt khi ước số chung lớn nhất của tất cả các giá trị được chọn lớn hơn (1). Nhiệm vụ là tìm số phần tử tối đa có thể có trong một tập hợp con như vậy. 

Thuộc tính chính của gcd mang lại cách giải thích hữu ích hơn nhiều. Một tập hợp các số có gcd lớn hơn (1) chính xác khi tất cả chúng đều có chung ít nhất một ước số nguyên tố. Vì vậy, thay vì nghĩ về các tập hợp con tùy ý, chúng ta có thể đặt một câu hỏi đơn giản hơn: với số nguyên tố (p) nào có số phần tử mảng lớn nhất có (p) là ước số? Câu trả lời là tần số tối đa như vậy. 

Giá trị của (n) chỉ là (1000), nhưng mọi (a_i) có thể lớn bằng (10^{18}). (n) nhỏ loại trừ việc liệt kê theo cấp số nhân các tập hợp con, nhưng các giá trị lớn là ràng buộc tinh tế hơn. Việc phân tích một số bằng cách thử mọi số nguyên đến căn bậc hai của nó sẽ yêu cầu tối đa (10^9) phép chia thử cho một giá trị đầu vào gần (10^{18}), điều này quá đắt trong Python. Chúng ta cần một phương pháp phân tích số nguyên hoạt động hiệu quả trên các số nguyên 64 bit, điều này dẫn đến việc kiểm tra tính nguyên tố Miller-Rabin kết hợp với phân tích hệ số Pollard-Rho một cách tự nhiên. 

Có một số trường hợp đặc biệt có thể phá vỡ việc triển khai đơn giản hơn. Nếu mọi giá trị đều bằng nhau, chẳng hạn như```
3
2 2 2
```câu trả lời là (3), vì cả ba giá trị đều có chung ước số nguyên tố (2). Việc triển khai chỉ tìm kiếm các cặp rồi đếm các giá trị gcd riêng biệt có thể dễ dàng xử lý sai các giá trị lặp lại. 

Một đầu vào một phần tử như```
1
35
```có câu trả lời (1). gcd của tập hợp con một phần tử là chính phần tử đó và mọi giá trị đầu vào ít nhất là (2). Việc triển khai khởi tạo câu trả lời về 0 và chỉ cập nhật nó sau khi tìm thấy hệ số chung giữa hai phần tử khác nhau sẽ trả về 0 không chính xác. 

Một trường hợp quan trọng khác là các gcd theo cặp không tự động xác định một thừa số chung cho cả nhóm. Vì```
3
6 10 15
```mọi cặp đều có gcd lớn hơn (1), nhưng gcd của cả ba số là (1). Câu trả lời đúng là (2), vì các nhóm lớn nhất có chung một số nguyên tố là ({6,10}), chia sẻ (2) hoặc ({6,15}), chia sẻ (3). Một giải pháp bất cẩn khi đếm thành phần được kết nối trong biểu đồ trong đó các cạnh biểu thị gcd lớn hơn (1) sẽ trả về sai (3). 

Cuối cùng, một số có thể có thừa số nguyên tố lớn. Ví dụ,```
2
1000000007 1000000009
```chứa các số nguyên tố lớn gần (10^9) và nói chung các ràng buộc cho phép các giá trị nguyên tố gần với (10^{18}). Việc triển khai phân chia thử nghiệm giả định các yếu tố nhỏ sẽ được tìm thấy nhanh chóng có thể hết thời gian chờ. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp nhất xem xét mọi tập hợp con của (n) phần tử mảng. Đối với mỗi tập hợp con, nó tính gcd của tất cả các phần tử được chọn và giữ tập hợp con lớn nhất có gcd lớn hơn (1). Điều này đúng vì mọi tập hợp con ứng cử viên có thể đều được kiểm tra rõ ràng. Tuy nhiên, có (2^n) tập hợp con và việc tính toán gcd trên tối đa (n) phần tử có giá (O(n\log A)), trong đó (A) là giá trị mảng lớn nhất. Đối với (n=1000), trường hợp xấu nhất là theo thứ tự (1000\cdot2^{1000}\log A) hoạt động gcd, điều này hoàn toàn không khả thi. 

Lực lượng vũ phu hoạt động vì định nghĩa về một tập hợp con tốt rất đơn giản, nhưng nó dành gần như toàn bộ nỗ lực để phân biệt giữa các tập hợp con thực sự có cùng lý do cơ bản để trở thành tốt. Nếu một số số có ước số nguyên tố (p) thì mọi tập hợp con được tạo từ các số đó đều có gcd chia hết cho (p). Chúng tôi không cần phải kiểm tra các tập hợp con đó một cách riêng biệt. 

Quan sát quan trọng là gcd lớn hơn (1) chính xác khi tồn tại một số nguyên tố chia cho mọi số trong tập hợp con đã chọn. Đối với bất kỳ số nguyên tố (p) nào, tập con lớn nhất có gcd chia hết cho (p) bao gồm mọi số đầu vào chia hết cho (p). Việc thêm một số khác chia hết cho (p) không bao giờ có thể làm cho gcd mất (p), vì vậy không có lý do gì để loại bỏ phần tử đó. 

Do đó, nếu chúng ta phân tích mọi (a_i) thành các ước số nguyên tố riêng biệt của nó, thì chúng ta có thể duy trì tần số cho mỗi số nguyên tố. Câu trả lời cuối cùng đơn giản là tần số lớn nhất. 

Thử thách còn lại là phân tích các số lớn bằng (10^{18}). Phép chia thử lên tới (\sqrt{a_i}) ​​quá chậm, do đó, cách triển khai tối ưu sử dụng Miller-Rabin xác định để kiểm tra tính nguyên tố trên số nguyên 64 bit và Pollard-Rho để phân chia số tổng hợp. Pollard-Rho phân tích đệ quy mọi giá trị đầu vào và chỉ các thừa số nguyên tố riêng biệt mới được tính cho từng phần tử mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n\log A)) | (O(n)) | Quá chậm | 
| Tối ưu | Dự kiến ​​(O(n\cdot C_{64})) cho hệ số 64-bit | (O(n+\log A)) | Đã chấp nhận | 

Ở đây (C_{64}) biểu thị chi phí dự kiến ​​của việc phân tích một số nguyên 64 bit với Pollard-Rho. Thời gian chạy của nó mang tính xác suất, nhưng Miller-Rabin sử dụng một tập cơ sở xác định đủ cho tất cả các số nguyên 64 bit. 

## Hướng dẫn thuật toán 

1. Đọc giá trị (n) và tạo bản đồ`count`từ thừa số nguyên tố đến số phần tử mảng chứa số nguyên tố đó. 
2. Đối với mỗi giá trị (x), hãy phân tích nó hoàn toàn thành các ước số nguyên tố bằng cách sử dụng Miller-Rabin và Pollard-Rho. Chúng tôi chỉ giữ lại các thừa số nguyên tố riêng biệt của (x), vì một phần tử chứa (p^2) sẽ đóng góp chính xác một lần vào số phần tử chia hết cho (p). 
3. Với mọi thừa số nguyên tố riêng biệt (p) của (x), số gia`count[p]`. Điều này thể hiện kích thước của tập hợp con lớn nhất có thể sử dụng (p) làm ước số chung giữa các phần tử được xử lý cho đến nay. 
4. Sau khi tất cả các số đã được xử lý, trả về giá trị lớn nhất trong`count`. Nếu một số nguyên tố (p) xuất hiện trong chính xác (k) phần tử đầu vào, thì các phần tử (k) đó có gcd chia hết cho (p), do đó chúng tạo thành một tập hợp con hợp lệ có kích thước (k). 
5. Nếu (n=1), việc phân tích thành nhân tử vẫn tìm thấy ít nhất một thừa số nguyên tố vì mọi giá trị đầu vào đều ít nhất là (2). Tần số của nó là (1), đưa ra câu trả lời đúng mà không cần trường hợp đặc biệt. 

### Tại sao nó hoạt động 

Với mọi tập con tốt, gcd (g) của nó lớn hơn (1), do đó (g) có ít nhất một ước số nguyên tố (p). Số nguyên tố đó chia mọi phần tử của tập hợp con. Do đó, mọi tập hợp con tốt có kích thước (k) đều được chứa trong tập hợp các phần tử đầu vào chia hết cho một số nguyên tố (p), nghĩa là tần số tối đa của thuật toán ít nhất là (k). 

Ngược lại, nếu một số nguyên tố (p) chia cho (k) phần tử đầu vào thì gcd của các phần tử (k) đó chia hết cho (p) nên lớn hơn (1). Chúng tạo thành một tập hợp con hợp lệ có kích thước (k). Do đó, tần số nguyên tố tối đa có thể đạt được. Cả hai hướng đều đưa ra chính xác mức tối ưu mong muốn. 

## Giải pháp Python```python
import sys
import random
import math

input = sys.stdin.readline

random.seed(0xC0FFEE)

SMALL_PRIMES = (
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37
)

MR_BASES = (
    2, 325, 9375, 28178, 450775, 9780504, 1795265022
)

def is_prime(n):
    if n < 2:
        return False

    for p in SMALL_PRIMES:
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
        c = random.randrange(1, n)
        x = random.randrange(0, n)
        y = x
        d = 1

        while d == 1:
            x = (x * x + c) % n
            y = (y * y + c) % n
            y = (y * y + c) % n
            d = math.gcd(abs(x - y), n)

        if d != n:
            return d

def factor(n, result):
    if n == 1:
        return

    if is_prime(n):
        result.add(n)
        return

    d = pollard_rho(n)
    factor(d, result)
    factor(n // d, result)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    count = {}

    for x in a:
        primes = set()
        factor(x, primes)

        for p in primes:
            count[p] = count.get(p, 0) + 1

    print(max(count.values()))

if __name__ == "__main__":
    solve()
```

`is_prime`đầu tiên loại bỏ các trường hợp có số nguyên tố nhỏ vì chúng rẻ và cũng giúp công việc của Pollard-Rho dễ dàng hơn. Đối với giá trị còn lại, nó ghi (n-1) dưới dạng (d\cdot2^s) và thực hiện Miller-Rabin với bảy cơ sở tiêu chuẩn cung cấp thử nghiệm tính nguyên tố xác định cho mọi số nguyên 64 bit không dấu.`pollard_rho`tìm kiếm một hệ số không tầm thường bằng cách sử dụng phép lặp giả ngẫu nhiên (f(x)=x^2+c\pmod n). Phát hiện chu kỳ của Floyd tăng một giá trị chuỗi một lần và một giá trị khác hai lần, sau đó lấy gcd của hiệu của chúng với (n). Khi gcd đó trở thành một ước số không tầm thường, đệ quy có thể chia số đó. 

Đệ quy`factor`hàm dừng ngay lập tức khi Miller-Rabin chứng minh rằng đối số của nó là số nguyên tố. Mặt khác, Pollard-Rho cung cấp một ước số và cả hai phần đều được phân tích đệ quy. Kết quả được lưu trữ trong một`set`, điều này rất cần thiết vì thống kê cuối cùng tính các phần tử mảng chứa một số nguyên tố chứ không phải tổng số mũ của số nguyên tố đó. 

Số nguyên Python có độ chính xác tùy ý, do đó, các giá trị lên tới (10^{18}) và tích số được sử dụng bởi số học mô-đun không bị tràn. biểu hiện`x * x % n`cũng an toàn vì lý do tương tự. 

Các cơ sở Miller-Rabin được cố định thay vì được tạo ngẫu nhiên. Điều này quan trọng vì nó làm cho việc kiểm tra tính nguyên thủy có tính xác định trên toàn bộ phạm vi đầu vào. Bản thân Pollard-Rho vẫn mang tính ngẫu nhiên, điều này mang lại hiệu suất tốt như mong đợi đối với các đầu vào tổng hợp khó. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`6 15 10 42`. Hệ số hóa và trạng thái tần số phát triển như sau. 

| Giá trị hiện tại | Thừa số nguyên tố riêng biệt | Trạng thái tần số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 6 | 2, 3 | 2:1, 3:1 | 1 | 
| 15 | 3, 5 | 2:1, 3:2, 5:1 | 2 | 
| 10 | 2, 5 | 2:2, 3:2, 5:2 | 2 | 
| 42 | 2, 3, 7 | 2:3, 3:3, 5:2, 7:1 | 3 | 

Tần số tối đa là (3). Ba phần tử chia hết cho (3) là (6,15,42), có gcd là (3), do đó đầu ra là`3`. 

Dấu vết này chứng tỏ tại sao thuật toán tính các thừa số nguyên tố riêng biệt cho mỗi phần tử. Số (42) chứa (2\cdot3\cdot7), nhưng nó chỉ đóng góp một số đếm cho mỗi tần số trong số ba tần số nguyên tố tương ứng. 

Đối với Mẫu 2, mọi giá trị đều là`2`. 

| Giá trị hiện tại | Thừa số nguyên tố riêng biệt | Trạng thái tần số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 2 | 2 | 2:1 | 1 | 
| 2 | 2 | 2:2 | 2 | 
| 2 | 2 | 2:3 | 3 | 

Tần số cuối cùng của số nguyên tố (2) là (3) nên có thể chọn cả 3 phần tử và đáp án là`3`. 

Trường hợp này xác nhận rằng các giá trị lặp lại được xử lý độc lập. Mỗi lần xuất hiện là một phần tử mảng riêng biệt và sẽ tăng tần suất của các thừa số nguyên tố của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​(O(n\cdot C_{64})) | Mỗi giá trị (n) được phân tích thành thừa số Miller-Rabin và Pollard-Rho | 
| Không gian | (O(n+\log A)) | Bản đồ tần số lưu trữ các số nguyên tố gặp phải và hệ số đệ quy có độ sâu logarit | 

Ở đây (A\le 10^{18}) và (C_{64}) biểu thị chi phí phân tích nhân tử Pollard-Rho dự kiến ​​cho số nguyên 64 bit. Vì (n\le1000), số lượng giá trị yêu cầu phân tích nhân tử là khiêm tốn, trong khi việc chia thử lên tới (10^9) cho mỗi giá trị sẽ quá đắt. Phương pháp phân tích nhân tố đã chọn được thiết kế đặc biệt cho các giá trị 64-bit lớn mà bài toán cho phép. 

## Trường hợp thử nghiệm```python
import sys
import io
import contextlib
import random
import math

SMALL_PRIMES = (
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37
)

MR_BASES = (
    2, 325, 9375, 28178, 450775, 9780504, 1795265022
)

random.seed(0xC0FFEE)

def is_prime(n):
    if n < 2:
        return False

    for p in SMALL_PRIMES:
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
        c = random.randrange(1, n)
        x = random.randrange(0, n)
        y = x
        d = 1

        while d == 1:
            x = (x * x + c) % n
            y = (y * y + c) % n
            y = (y * y + c) % n
            d = math.gcd(abs(x - y), n)

        if d != n:
            return d

def factor(n, result):
    if n == 1:
        return
    if is_prime(n):
        result.add(n)
        return

    d = pollard_rho(n)
    factor(d, result)
    factor(n // d, result)

def solve_text(inp):
    data = inp.split()
    n = int(data[0])
    a = list(map(int, data[1:n + 1]))

    count = {}

    for x in a:
        primes = set()
        factor(x, primes)

        for p in primes:
            count[p] = count.get(p, 0) + 1

    return str(max(count.values())) + "\n"

# Provided samples
assert solve_text("""4
6 15 10 42
""") == "3\n", "sample 1"

assert solve_text("""3
2 2 2
""") == "3\n", "sample 2"

assert solve_text("""1
35
""") == "1\n", "sample 3"

# Minimum-size input
assert solve_text("""1
2
""") == "1\n", "single element"

# All elements share a large prime factor
assert solve_text("""4
1000000007 2000000014 3000000021 4000000028
""") == "4\n", "large common prime factor"

# Pairwise gcds can be greater than 1 without one common divisor
assert solve_text("""3
6 10 15
""") == "2\n", "pairwise gcd trap"

# Boundary near 10^18, with no common prime factor
assert solve_text("""3
999999999999999989 999999999999999937 1000000000000000000
""") == "1\n", "large boundary values"

# Maximum n, all equal
values = " ".join(["2"] * 1000)
assert solve_text("1000\n" + values + "\n") == "1000\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`1`| Tối thiểu (n) và trường hợp đơn lẻ | 
|`4 / 1000000007 2000000014 3000000021 4000000028`|`4`| Phân tích thừa số nguyên tố lớn và thừa số chung lặp | 
|`3 / 6 10 15`|`2`| Ngăn chặn việc xử lý kết nối gcd theo cặp như một gcd chung | 
|`3 / 999999999999999989 999999999999999937 1000000000000000000`|`1`| Giá trị ranh giới 64-bit lớn | 
|`1000`bản sao của`2`|`1000`| Giá trị tối đa (n) và lặp lại giống hệt nhau | 

## Vỏ cạnh 

Trường hợp đơn lẻ được xử lý trực tiếp bởi cùng một bất biến đếm thừa số. Đối với đầu vào```
1
35
```Pollard-Rho không bao giờ cần thiết vì Miller-Rabin xác định (35) là hợp số và phân tích nó thành (5) và (7). Cả hai số nguyên tố đều nhận được tần số (1), vì vậy mức tối đa là (1). Thuật toán không cần so sánh phần tử này với bất kỳ phần tử nào khác. 

Đối với các giá trị lặp lại,```
3
2 2 2
```mỗi lần xuất hiện được tính riêng. Tập hợp các thừa số nguyên tố riêng biệt cho mỗi lần xuất hiện là`{2}`, do đó tần số của (2) tăng dần từ (1) đến (2) đến (3). Câu trả lời cuối cùng là (3). Điều này tránh được lỗi phổ biến là sao chép các giá trị đầu vào trước khi đếm. 

Đối với bẫy gcd theo cặp,```
3
6 10 15
```các bộ nhân tố là`{2,3}`,`{2,5}`, Và`{3,5}`. Số nguyên tố (2) xảy ra hai lần, số nguyên tố (3) xảy ra hai lần và số nguyên tố (5) xảy ra hai lần. Không có số nguyên tố nào xuất hiện ba lần nên câu trả lời là (2), mặc dù mỗi cặp đều có gcd lớn hơn (1). Thuật toán tính số nguyên tố chung thực tế thay vì suy ra một nhóm từ các mối quan hệ từng cặp. 

Đối với các giá trị rất lớn, quy trình phân tích nhân tử hoạt động hoàn toàn với số nguyên modulo số hiện tại. Ví dụ: khi một đầu vào chứa số nguyên tố gần (10^{18}), Miller-Rabin có thể nhận ra nó mà không cần chia thử cho mọi số nguyên nhỏ hơn. Khi giá trị là tổng hợp, Pollard-Rho tìm kiếm một hệ số không tầm thường thay vì quét tất cả các ứng cử viên cho đến căn bậc hai của nó. Đây là một phần của quá trình triển khai giúp cho ràng buộc (10^{18}) trở nên thiết thực. 

Trường hợp giá trị lặp lại kích thước tối đa có (1000) bản sao của (2). Công việc phân tích nhân tử cho mỗi giá trị là rất nhỏ và bản đồ tần số chỉ chứa một khóa. Số đếm cuối cùng đạt tới (1000), cho thấy rằng câu trả lời được phép bằng kích thước đầy đủ của mảng và không có hạn chế nào loại trừ toàn bộ mảng.
