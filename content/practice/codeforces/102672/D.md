---
title: "CF 102672D - Tập hợp con tốt"
description: "Bài toán đưa ra một tập hợp các số nguyên dương được viết trên một ổ khóa. Chúng ta cần chọn càng nhiều số nguyên càng tốt để mọi số được chọn đều có ước chung lớn hơn một."
date: "2026-08-01T23:43:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102672
codeforces_index: "D"
codeforces_contest_name: "Selection of tasks from Internet olympiads season 2019-20"
rating: 0
weight: 102672
solve_time_s: 69
verified: true
draft: false
---

[CF 102672D - Tập hợp con tốt](https://codeforces.com/problemset/problem/102672/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một tập hợp các số nguyên dương được viết trên một ổ khóa. Chúng ta cần chọn càng nhiều số nguyên càng tốt để mọi số được chọn đều có ước chung lớn hơn một. Nói cách khác, trong số tất cả các tập con có thể có, chúng ta muốn tập con lớn nhất có ước chung lớn nhất không bằng 1. 

Dữ liệu đầu vào chứa tối đa 1000 số, nhưng mỗi số có thể lớn bằng$10^{18}$. Số lượng phần tử nhỏ loại trừ các thuật toán phụ thuộc vào việc thử nhiều tập hợp con, bởi vì ngay cả việc kiểm tra tất cả các tập hợp con cũng sẽ yêu cầu$2^{1000}$hoạt động. Phạm vi giá trị lớn cũng loại trừ phép chia thử theo giá trị của từng số, vì một số có thể yêu cầu hàng tỷ lần thử. Giải pháp dự định cần khai thác thực tế là chúng ta chỉ cần các ước số nguyên tố chứ không cần cấu trúc nhân tử hóa đầy đủ. 

Câu trả lời được kiểm soát bởi các thừa số nguyên tố. Nếu nhiều số có thừa số nguyên tố$p$, thì tất cả chúng có thể được đặt trong một tập hợp con hợp lệ vì gcd của chúng ít nhất là$p$. Tập hợp con hợp lệ lớn nhất chính xác là nhóm số lớn nhất có chung một ước số nguyên tố. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Nếu mọi số đều là số nguyên tố và tất cả các số nguyên tố đều khác nhau thì câu trả lời vẫn là một vì một số có gcd bằng chính nó. Ví dụ:```
Input:
3
2 3 5

Output:
1
```Giải pháp chỉ tính các yếu tố lặp lại sẽ in không chính xác số 0. 

Một trường hợp phức tạp khác là khi một số chứa cùng một số nguyên tố nhiều lần. Ví dụ:```
Input:
3
12 18 25

Output:
2
```Các yếu tố chính là$12 = 2^2 \cdot 3$,$18 = 2 \cdot 3^2$, Và$25 = 5^2$. Tập hợp con tốt nhất là$\{12,18\}$, vì chúng có chung cả 2 và 3. Việc đếm các số nguyên tố xuất hiện bên trong một số thay vì đếm các số chứa số nguyên tố sẽ đánh giá kết quả quá cao. 

Trường hợp cạnh cuối cùng là số nguyên tố lớn gần giới hạn trên. Ví dụ:```
Input:
1
1000000000000000000

Output:
1
```Phương pháp phân tích nhân tử phải xử lý các số tổng hợp lớn mà không giả sử rằng mọi giá trị đều có thể chia cho các số nguyên tố nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi tập hợp con, tính gcd của các phần tử của nó và giữ tập hợp con lớn nhất có gcd lớn hơn một. Điều này đúng vì mọi câu trả lời có thể đều được xem xét. Tuy nhiên, có$2^n$tập hợp con và với$n=1000$điều này là hoàn toàn không thể. 

Một ý tưởng tốt hơn một chút là nhìn vào các ước số. Với mỗi ước số có thể, hãy đếm xem có bao nhiêu số chia hết cho nó. Số lượng lớn nhất sẽ là câu trả lời. Vấn đề là con số lên đến$10^{18}$, do đó việc liệt kê các ước của mỗi số cũng quá chậm nếu thực hiện bằng phép chia thử. 

Quan sát quan trọng là một tập hợp con có gcd lớn hơn một khi và chỉ khi tất cả các số của nó có chung ít nhất một ước số nguyên tố. Chúng ta không cần biết chính xác gcd. Chúng ta chỉ cần biết số nguyên tố nào xuất hiện trong số nào. Sau khi phân tích mọi số thành thừa số nguyên tố riêng biệt, chúng ta có thể đếm xem có bao nhiêu số chứa mỗi số nguyên tố và lấy số đếm lớn nhất. 

Thách thức còn lại là tính toán các giá trị lên tới$10^{18}$. Hệ số Pollard Rho được thiết kế chính xác cho phạm vi này. Nó có thể tìm ra các thừa số không tầm thường của các số tổng hợp lớn một cách hiệu quả, trong khi phép kiểm tra tính nguyên tố Miller Rabin nhanh chóng xác định được các số nguyên tố. Sau khi có được các thừa số, chúng ta chỉ chèn các số nguyên tố riêng biệt cho mỗi số, vì một số chứa$2^5$chỉ đóng góp một phần tử vào số nguyên tố 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Đếm số chia | Quá lớn đối với$10^{18}$giá trị |$O(1)$| Quá chậm | 
| Pollard Rho + Đếm thừa số nguyên tố | Về$O(n \log a_i)$dự kiến ​​|$O(k)$, Ở đâu$k$là số thừa số nguyên tố đã tìm được | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích mọi số bằng cách sử dụng Miller Rabin và Pollard Rho. 

Miller Rabin cho chúng ta biết một số có phải là số nguyên tố hay không. Nếu một số là hợp số, Pollard Rho sẽ tìm một thừa số và chúng ta phân tích đệ quy cả hai phần. Điều này tránh việc quét tất cả các ước số có thể. 
2. Loại bỏ các thừa số nguyên tố trùng lặp bên trong mỗi số. 

Một số như 72 có thừa số nguyên tố là 2 và 3. Mặc dù 2 xuất hiện nhiều lần trong quá trình phân tích thành thừa số, nhưng nó chỉ tăng số nguyên tố 2 một lần. 
3. Đối với mỗi thừa số nguyên tố riêng biệt của số, hãy tăng tần số của nó trong bộ đếm tổng thể. 

Bộ đếm biểu thị có bao nhiêu số gốc chứa số nguyên tố đó. Nếu một số nguyên tố xuất hiện trong nhiều số thì những số đó tạo thành một tập hợp con hợp lệ. 
4. Xuất ra tần số lớn nhất trong số tất cả các số nguyên tố. 

Nếu mọi số không có thừa số nguyên tố chung thì mỗi số nguyên tố nhiều nhất là một và câu trả lời vẫn là một. 

Tại sao nó hoạt động: 

Hãy xem xét tập hợp con tối ưu. gcd của nó là một số nguyên lớn hơn một. Bất kỳ ước số nguyên tố nào của gcd này đều chia mọi phần tử trong tập hợp con. Do đó tập hợp con được xếp vào nhóm số chứa số nguyên tố đó. Thuật toán của chúng tôi kiểm tra mọi số nguyên tố xuất hiện trong đầu vào và tìm ra nhóm lớn nhất như vậy. Ngược lại, mọi nhóm được tính bởi một số nguyên tố đều có số nguyên tố đó là ước chung, vì vậy nó luôn là tập hợp con hợp lệ. Cả hai hướng đều chứng minh rằng số lượng tối đa chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
import random
import math
from collections import defaultdict

input = sys.stdin.readline

def is_prime(n):
    if n < 2:
        return False
    small = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]
    for p in small:
        if n == p:
            return True
        if n % p == 0:
            return False

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    for a in [2, 3, 5, 7, 11, 13]:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        good = False
        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                good = True
                break
        if not good:
            return False
    return True

def pollard(n):
    if n % 2 == 0:
        return 2
    if n % 3 == 0:
        return 3
    while True:
        c = random.randrange(1, n - 1)
        x = random.randrange(0, n - 1)
        y = x
        d = 1
        while d == 1:
            x = (x * x + c) % n
            y = (y * y + c) % n
            y = (y * y + c) % n
            d = math.gcd(abs(x - y), n)
        if d != n:
            return d

def factor(n, res):
    if n == 1:
        return
    if is_prime(n):
        res.append(n)
    else:
        d = pollard(n)
        factor(d, res)
        factor(n // d, res)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    count = defaultdict(int)

    for x in a:
        factors = []
        factor(x, factors)
        for p in set(factors):
            count[p] += 1

    print(max(count.values()))

if __name__ == "__main__":
    solve()
```Kiểm tra tính nguyên thủy sử dụng các cơ sở xác định đủ cho phạm vi 64 bit nhất định. Hàm nhân tố đệ quy chia các số tổng hợp cho đến khi mọi phần còn lại đều là số nguyên tố. 

các`set(factors)`việc chuyển đổi là cần thiết vì chúng ta đang đếm các số chứ không phải lũy thừa nguyên tố. Không có nó, một giá trị như$16$sẽ cộng sai bốn phần đóng góp vào số nguyên tố 2. 

Từ điển cuối cùng chỉ lưu trữ các số nguyên tố thực sự xảy ra. Vì mỗi số đầu vào có ít nhất là hai nên luôn có ít nhất một thừa số nguyên tố, do đó tồn tại số lớn nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
6 15 10 42
```Số lượng yếu tố phát triển như sau. 

| Số đã xử lý | Yếu tố khác biệt | Đếm 2 | Đếm 3 | Đếm 5 | Đếm 7 | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 2, 3 | 1 | 1 | 0 | 0 | 
| 15 | 3, 5 | 1 | 2 | 1 | 0 | 
| 10 | 2, 5 | 2 | 2 | 2 | 0 | 
| 42 | 2, 3, 7 | 3 | 3 | 2 | 1 | 

Tần số tối đa là 3, vì vậy ba số có thể có chung ước số nguyên tố. Điều này khớp với tập hợp con chứa 6, 15 và 42. 

Đối với mẫu thứ hai:```
3
2 2 2
```| Số đã xử lý | Yếu tố khác biệt | Đếm 2 | 
| --- | --- | --- | 
| 2 | 2 | 1 | 
| 2 | 2 | 2 | 
| 2 | 2 | 3 | 

Thuật toán xử lý chính xác mỗi lần xuất hiện dưới dạng một phần tử mảng riêng biệt, đưa ra câu trả lời 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Hy vọng$O(n \log a_i)$| Mỗi số được phân tích bằng cách sử dụng Pollard Rho, hiệu quả đối với số nguyên 64 bit | 
| Không gian |$O(k)$| Lưu trữ số nguyên tố được phát hiện | 

Kích thước đầu vào chỉ có 1000 số nên khó khăn chính là kích thước của các giá trị chứ không phải là số phần tử. Pollard Rho tránh được sự phân chia thử nghiệm bất khả thi và thoải mái nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    n = int(data[0])
    arr = list(map(int, data[1:]))

    from collections import defaultdict
    import math
    import random

    def is_prime(n):
        if n < 2:
            return False
        for p in [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]:
            if n == p:
                return True
            if n % p == 0:
                return False
        d = n - 1
        s = 0
        while d % 2 == 0:
            s += 1
            d //= 2
        for a in [2, 3, 5, 7, 11, 13]:
            if a >= n:
                continue
            x = pow(a, d, n)
            if x in (1, n - 1):
                continue
            ok = False
            for _ in range(s - 1):
                x = x * x % n
                if x == n - 1:
                    ok = True
                    break
            if not ok:
                return False
        return True

    def pollard(n):
        if n % 2 == 0:
            return 2
        while True:
            c = random.randrange(1, n - 1)
            x = random.randrange(0, n - 1)
            y = x
            d = 1
            while d == 1:
                x = (x * x + c) % n
                y = (y * y + c) % n
                y = (y * y + c) % n
                d = math.gcd(abs(x - y), n)
            if d != n:
                return d

    def factor(x, out):
        if x == 1:
            return
        if is_prime(x):
            out.append(x)
        else:
            d = pollard(x)
            factor(d, out)
            factor(x // d, out)

    cnt = defaultdict(int)
    for x in arr:
        f = []
        factor(x, f)
        for p in set(f):
            cnt[p] += 1

    return str(max(cnt.values())) + "\n"

assert run("4\n6 15 10 42\n") == "3\n"
assert run("3\n2 2 2\n") == "3\n"

assert run("1\n35\n") == "1\n"
assert run("3\n2 3 5\n") == "1\n"
assert run("3\n12 18 25\n") == "2\n"
assert run("2\n9999999967 9999999967\n") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 6 15 10 42`|`3`| Các yếu tố được chia sẻ giữa các số nguyên tố khác nhau | 
|`3 / 2 2 2`|`3`| Giá trị lặp lại | 
|`1 / 35`|`1`| Tập hợp con phần tử đơn | 
|`3 / 2 3 5`|`1`| Không có yếu tố chung | 
|`3 / 12 18 25`|`2`| Các lũy thừa nguyên tố trùng lặp bên trong một số | 
| Hai số nguyên tố lớn bằng nhau |`2`| Xử lý hệ số nguyên lớn | 

## Vỏ cạnh 

Đối với trường hợp mỗi số có một thừa số nguyên tố khác nhau:```
3
2 3 5
```Việc nhân tố hóa tạo ra số lượng`2 -> 1`,`3 -> 1`, Và`5 -> 1`. Số lượng lớn nhất là một, điều này đúng vì không có cặp nào có thể có gcd lớn hơn một. 

Đối với lũy thừa nguyên tố lặp đi lặp lại:```
3
12 18 25
```Số đầu tiên chỉ đóng góp`{2,3}`, không`{2,2,3}`. Thứ hai đóng góp`{2,3}`. Thứ ba đóng góp`{5}`. Số số nguyên tố 2 và 3 trở thành hai, đưa ra câu trả lời đúng. 

Đối với các giá trị tổng hợp rất lớn:```
1
1000000000000000000
```Pollard Rho chia số này thành thừa số nguyên tố, nhưng câu trả lời chỉ cần thực tế là có ít nhất một số nguyên tố tồn tại. Số lượng số nguyên tố đó là một, vì vậy đầu ra là một. Điều này tránh mọi nỗ lực lặp qua tất cả các ước số có thể có cho đến căn bậc hai.
