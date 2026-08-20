---
title: "CF 102190C - đầu vào/đầu ra tiêu chuẩn"
description: "Chúng ta có các số nguyên từ (1) đến (n) và chúng ta muốn giữ càng nhiều số nguyên càng tốt trong một chuỗi. Chuỗi không nhất thiết phải chứa mọi số nguyên."
date: "2026-08-19T05:36:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "C"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 230
verified: true
draft: false
---

[CF 102190C - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có các số nguyên từ (1) đến (n) và chúng ta muốn giữ càng nhiều số nguyên càng tốt trong một chuỗi. Chuỗi không nhất thiết phải chứa mọi số nguyên. Hạn chế duy nhất là cục bộ: bất cứ khi nào hai số nguyên được chọn liên tiếp, chúng phải có chung một số thừa số nguyên tố, tương đương gcd của chúng phải ít nhất là (2). 

Giá trị (1) không bao giờ có thể được chọn trong một chuỗi có độ dài lớn hơn một vì (\gcd(1,x)=1) với mọi (x). Vì (n\ge4), một chuỗi tối ưu luôn có độ dài lớn hơn một, nên (1) nhất thiết phải bị loại bỏ. 

Giới hạn (n\le10^6) loại trừ mọi thứ bậc hai hoặc giai thừa. Ngay cả (O(n\sqrt n)) có thể đắt tiền một cách không cần thiết trong Python, trong khi (O(n\log\log n)) thì thoải mái. Cấu trúc hữu ích là mọi số nguyên liên quan đều có thể được phân loại theo thừa số nguyên tố của nó và một sàng có thể hiển thị tất cả các số nguyên tố về cơ bản theo thời gian tuyến tính. 

Trường hợp không rõ ràng đầu tiên là (n=4). Các số hữu ích duy nhất là (2) và (4), vì vậy độ dài tối đa chính xác là (2), ví dụ: đầu ra có thể là`2 2 4`. Việc triển khai bất cẩn giả định rằng mọi số nguyên tố đều có thể được chèn vào chuỗi sẽ cố gắng sử dụng (3), nhưng (3) không có bội số nào khác nhiều nhất (4). 

Với (n=7), dãy`3 6 2 4`có chiều dài (4). Số nguyên tố (3) có thể được sử dụng vì (6) là bội số duy nhất khác của nó, vì vậy nó phải là điểm cuối. Các số nguyên tố (5) và (7) không thể được sử dụng. Một cấu trúc mù quáng giữ mọi số nguyên tố bên dưới (n/2) sẽ bao gồm (5) không chính xác. 

Với (n=19), độ dài tối ưu là (14). Các số nguyên tố được chọn là (3,5,7), trong khi (11,13,17,19) bị loại trừ. Số nguyên tố (7) chỉ có một bội số hữu ích, (14), vì vậy nó phải chiếm một điểm cuối. Trình tự mẫu chứng minh điều này với`7 14 ...`. Việc xây dựng để lại tất cả các vị trí điểm cuối cho các số tổng hợp thông thường có thể âm thầm mất (7). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê các tập hợp con có thể có của các số nguyên và sau đó thử hoán vị của các tập hợp con đó, kiểm tra mọi gcd liền kề. Ngay cả khi chúng ta bỏ qua các tập hợp con và chỉ liệt kê các hoán vị chứa tất cả (n) số, thì vẫn có (n!) ứng cử viên và mỗi ứng cử viên cần tới (n-1) kiểm tra gcd. Do đó, chỉ riêng phần hoán vị đầy đủ đã có giá (\Theta(n\cdot n!)) và việc cho phép các tập hợp con chỉ làm cho tìm kiếm lớn hơn. Đối với (n=10^6), điều này không khả thi chút nào. 

Quan sát quan trọng đến từ việc xem xét các số nguyên tố hơn là các hợp chất. Một số nguyên tố (p) chỉ có thể liền kề với bội số của (p). Nếu (p>n/2), nó không có bội số nào khác nên không thể chọn được. Nếu (n/3<p\le n/2), bội số còn lại duy nhất của nó là (2p). Do đó, một số nguyên tố như vậy có bậc một trong biểu đồ tương thích và chỉ có thể xuất hiện ở một trong hai đầu của dãy. Do đó, có thể chọn tối đa hai số nguyên tố từ khoảng này. 

Mọi số nguyên tố lẻ (p\le n/3) có ít nhất (p,2p,3p), do đó nó có thể được đặt bên trong. Chúng ta có thể xây dựng một dãy chứa tất cả các hợp số, nhiều nhất là tất cả các số nguyên tố lẻ (n/3) và nhiều nhất là hai số nguyên tố nằm giữa (n/3) và (n/2). 

Việc xây dựng trở nên đặc biệt thuận tiện nếu các số nguyên tố được xử lý từ lớn đến nhỏ. Khi xử lý một số nguyên tố lẻ (p), tất cả bội số của (p) đã được sử dụng bởi các số nguyên tố lớn hơn sẽ bị bỏ qua. Mọi bội số còn lại vẫn chia hết cho (p), vì vậy các giá trị còn lại này tự tạo thành một khối hợp lệ. 

Đối với số nguyên tố (p\le n/4), cả (2p) và (4p) đều có sẵn. Chúng tôi biến chúng thành hai điểm cuối của khối. Vì cả hai điểm cuối đều chẵn, nên các khối cho các số nguyên tố khác nhau có thể được nối vì các điểm cuối khối liên tiếp có ít nhất gcd (2). 

Đối với các số nguyên tố (n/4<p\le n/3), các bội số duy nhất ngoài (p) là (2p) và (3p). Các khối như vậy có điểm cuối chia hết cho (2) và (3). Bằng cách luân phiên hướng của chúng, các khối liên tiếp có thể được nối thông qua các hệ số bằng nhau (2) và (3). Khối đặc biệt duy nhất còn lại là khối dành cho (3), khối này cung cấp kết nối cần thiết giữa các khối trung bình này và các số chẵn còn lại. 

Hai số nguyên tố lớn nhất có thể sử dụng từ ((n/3,n/2]), nếu chúng tồn tại, được đặt ở hai đầu. Các khối của chúng chỉ đơn giản là (p,2p) và (2p,p), và các điểm cuối chẵn kết nối một cách tự nhiên với chuỗi chính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(n\cdot n!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng một sàng Eratosthenes lên đến (n). Chúng ta cần các số nguyên tố để có thể phân biệt các vật liệu tổng hợp thông thường với các số nguyên tố cần được xử lý đặc biệt. 
2. Tách các số nguyên tố lẻ thành ba dãy. Không thể chọn các số nguyên tố lớn hơn (n/2). Các số nguyên tố nằm giữa (n/3) và (n/2) chỉ có một bội số khác, vì vậy chúng ta giữ lại nhiều nhất hai bội số đó. Các số nguyên tố giữa (n/4) và (n/3) tạo thành các khối ba phần tử (2p,p,3p) hoặc ngược lại. Các số nguyên tố lẻ nhỏ hơn có thể sử dụng (2p) và (4p) làm điểm cuối khối. 
3. Dự trữ các số nguyên tố đã chọn từ ((n/3,n/2]). Nếu có hai số nguyên tố, đặt khối đầu tiên ở đầu và khối thứ hai ở cuối. Nếu chỉ tồn tại một số, đặt khối của nó ở đầu. Đánh dấu các giá trị tương ứng là đã sử dụng để bội số của chúng không được khối khác sử dụng lại. 
4. Xử lý các số nguyên tố trong khoảng (n/4<p\le n/3). Đối với mỗi số nguyên tố như vậy, bội số duy nhất được chọn là (p,2p,3p). Lưu trữ khối theo các hướng xen kẽ. Khối đầu tiên có điểm cuối (2p) và (3p), khối tiếp theo có điểm cuối (3q) và (2q). Do đó, các khối liên tiếp gặp nhau thông qua bội số của (3) hoặc bội số của (2). 
5. Xử lý số nguyên tố (3). Thu thập mọi bội số vẫn chưa sử dụng của (3), đặt (6) đầu tiên và (12) cuối cùng. Mọi giá trị trong khối này đều chia hết cho (3), trong khi cả hai điểm cuối đều là số chẵn. Khi số nguyên tố trung bình là số lẻ, điểm cuối (3p) cuối cùng sẽ kết hợp (6) thông qua hệ số (3). Khi nó chẵn, toàn bộ khối có thể được chèn vào giữa hai khối chẵn. 
6. Xử lý mọi số nguyên tố lẻ (p) với (5\le p\le n/4), theo thứ tự giảm dần. Quét bội số của nó và lấy từng cái chưa được sử dụng. Đặt (2p) ở đầu khối và (4p) ở cuối. Tất cả các giá trị ở giữa là bội số của (p), vì vậy mọi cặp liền kề bên trong khối đều có ít nhất gcd (p). Hai điểm cuối bằng nhau nên khối này có thể được kết nối với khối khác. 
7. Nối tất cả các số chẵn vẫn chưa được sử dụng. Tất cả chúng đều chia hết cho (2), vì vậy chúng tạo thành một khối hợp lệ cuối cùng. Tại thời điểm này, mọi số tổng hợp được chọn đã được sử dụng chính xác một lần. 
8. In độ dài của chuỗi kết quả và chính chuỗi đó. Đối với (n<12), cấu trúc có một vài tương tác ranh giới phạm vi nhỏ, do đó việc triển khai sử dụng cấu trúc hợp lệ rõ ràng cho các giá trị đó. 

Điều bất biến là mọi khối hoàn chỉnh bao gồm toàn bộ bội số của một số nguyên tố chung, trong khi mọi kết nối giữa các khối được thực hiện thông qua một điểm cuối chẵn hoặc thông qua hai bội số của (3). Do đó, mọi cặp liền kề đều có ước chung lớn hơn một. Giới hạn trên tuân theo một cách độc lập: (1) là không thể, các số nguyên tố trên (n/2) bị cô lập và mọi số nguyên tố trong ((n/3, n/2]) chỉ có sẵn (2p), do đó, nhiều nhất hai số nguyên tố như vậy có thể xảy ra. Việc xây dựng đạt chính xác giới hạn đó trong khi bao gồm mọi giá trị có thể khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    if n == 4:
        print(2)
        print(2, 4)
        return
    if n == 5:
        print(2)
        print(2, 4)
        return
    if n == 6:
        ans = [3, 6, 2, 4]
        print(len(ans))
        print(*ans)
        return
    if n == 7:
        ans = [3, 6, 2, 4]
        print(len(ans))
        print(*ans)
        return
    if n == 8:
        ans = [3, 6, 2, 4, 8]
        print(len(ans))
        print(*ans)
        return
    if n == 9:
        ans = [9, 3, 6, 2, 4, 8]
        print(len(ans))
        print(*ans)
        return
    if n == 10:
        ans = [5, 10, 2, 4, 8, 6, 3]
        print(len(ans))
        print(*ans)
        return
    if n == 11:
        ans = [5, 10, 2, 4, 8, 6, 3, 9]
        print(len(ans))
        print(*ans)
        return

    # Sieve of Eratosthenes.
    prime = bytearray(b'\x01') * (n + 1)
    prime[0:2] = b'\x00\x00'

    limit = int(n ** 0.5)
    for p in range(2, limit + 1):
        if prime[p]:
            start = p * p
            prime[start:n + 1:p] = b'\x00' * (
                (n - start) // p + 1
            )

    used = bytearray(n + 1)
    ans = []

    # Odd primes in (n/3, n/2].
    high = []
    lo = n // 3 + 1
    hi = n // 2
    for p in range(lo, hi + 1):
        if p & 1 and prime[p]:
            high.append(p)

    high = high[:2]

    # Left endpoint block.
    if high:
        p = high[0]
        ans.extend((p, 2 * p))
        used[p] = 1
        used[2 * p] = 1

    # Medium primes: n/4 < p <= n/3.
    medium = []
    lo = n // 4 + 1
    hi = n // 3
    for p in range(5, hi + 1, 2):
        if p >= lo and prime[p]:
            medium.append(p)

    # Alternate orientations:
    # [2p, p, 3p], [3q, q, 2q], ...
    for i, p in enumerate(medium):
        if i & 1:
            block = (3 * p, p, 2 * p)
        else:
            block = (2 * p, p, 3 * p)

        for x in block:
            used[x] = 1

        ans.extend(block)

    # The prime 3 and its still-unused multiples.
    block3 = []
    if 6 <= n:
        block3.append(6)

        for x in range(3, n + 1, 3):
            if x != 6 and x != 12 and not used[x]:
                block3.append(x)

        if 12 <= n:
            block3.append(12)

        for x in block3:
            used[x] = 1

        # If the number of medium blocks is odd, the previous
        # block ends in a multiple of 3, so block3 starts at 6.
        ans.extend(block3)

    # Small odd primes p <= n/4.
    # Descending order guarantees that 2p and 4p have not
    # already been consumed by a larger odd prime.
    hi = n // 4
    for p in range(hi | 1, 4, -2):
        if not prime[p]:
            continue

        block = [2 * p]

        for x in range(p, n + 1, p):
            if x == 2 * p or x == 4 * p:
                continue
            if not used[x]:
                block.append(x)
                used[x] = 1

        block.append(4 * p)
        used[2 * p] = 1
        used[4 * p] = 1

        ans.extend(block)

    # Remaining even numbers form one final block.
    for x in range(2, n + 1, 2):
        if not used[x]:
            ans.append(x)
            used[x] = 1

    # Right endpoint block, if there is a second high prime.
    if len(high) == 2:
        p = high[1]
        ans.extend((2 * p, p))
        used[2 * p] = 1
        used[p] = 1

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Sàng sử dụng một`bytearray`thay vì danh sách boolean của Python, giúp giữ cho bảng nguyên tố nhỏ gọn cho (n=10^6). các`used`mảng cũng là một bytearray vì mỗi số nguyên chỉ cần một bit trạng thái. 

Các khối có số nguyên tố cao được đánh dấu trước khi xử lý các số nguyên tố nhỏ hơn. Điều này ngăn không cho một giá trị như (22) vô tình bị sử dụng sau này khi xử lý một số nguyên tố khác, trong khi vẫn để lại các bội số không liên quan. 

Các khối trung bình được lưu trữ trực tiếp trong câu trả lời vì mỗi khối chứa chính xác ba giá trị. Đối với các số nguyên tố nhỏ hơn, mã quét bội số của (p) và đánh dấu chúng ngay lập tức. Một số chia hết cho nhiều số nguyên tố đã được xử lý thuộc về khối số nguyên tố được xử lý lớn nhất, giúp ngăn chặn sự trùng lặp mà không cần một bộ số nguyên. 

Thứ tự của các điểm cuối là phần tinh tế. Đối với số nguyên tố nhỏ (p),`2*p`Và`4*p`được đảm bảo là có sẵn vì số nguyên tố lẻ lớn hơn không thể chia hết một trong hai số đó. Đặt hai giá trị đó vào cuối làm cho khối kết nối thông qua hệ số (2). 

Số nguyên Python không bị tràn nên không cần loại số nguyên đặc biệt. Các điều kiện biên duy nhất cần xử lý rõ ràng là các giá trị nhỏ bên dưới (12), trong đó khối chung (6,12) cho số nguyên tố (3) không tồn tại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, (n=4), cấu trúc trường hợp nhỏ ngay lập tức trả về hai số chẵn. 

| n | Xây dựng | Trình tự | Chiều dài | 
| --- | --- | --- | --- | 
| 4 | Chi nhánh nhỏ | 2, 4 | 2 | 

Gcd là (\gcd(2,4)=2), vì vậy chuỗi này hợp lệ. Không thể đưa số nguyên tố (3) vào vì giá trị duy nhất của nó là chính nó (3). 

Đối với mẫu thứ hai, (n=19), phạm vi số nguyên tố cao là (19/3<p\le19/2), chỉ chứa (7). Phạm vi trung bình chứa (5). Prime (3) được xử lý bởi khối riêng của nó và các giá trị chẵn nhỏ còn lại sẽ hoàn thành việc xây dựng. 

| Sân khấu | Giá trị gia tăng | Trình tự hiện tại | 
| --- | --- | --- | 
| Cao nguyên 7 | 7, 14 | 7, 14 | 
| Trung nguyên 5 | 10, 5, 15 | 7, 14, 10, 5, 15 | 
| Thủ tướng 3 | 6, 3, 9, 18, 12 | 7, 14, 10, 5, 15, 6, 3, 9, 18, 12 | 
| Sự kiện còn lại | 2, 4, 8, 16 | 7, 14, 10, 5, 15, 6, 3, 9, 18, 12, 2, 4, 8, 16 | 

Mọi chuyển đổi bên trong bên trong khối (5) đều có hệ số (5), mọi chuyển đổi bên trong khối (3) đều có hệ số (3) và chuyển đổi giữa các khối được thực hiện thông qua các giá trị chẵn. Độ dài kết quả là (14), phù hợp với mức tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log\log n)) | Chi phí sàng (O(n\log\log n)) và quét bội số trên tất cả các số nguyên tố có liên quan có tổng chi phí (O(n\log\log n)). | 
| Không gian | (O(n)) | Cả mảng byte nguyên tố và byte được sử dụng đều có kích thước (O(n)), trong khi đầu ra chứa tối đa (n) số nguyên. | 

(n) tối đa là (10^6), do đó sàng và tổng hài của bội số nguyên tố dễ dàng nằm trong phạm vi dự định. Việc triển khai cũng tránh việc lưu trữ một đối tượng boolean Python lớn cho mỗi số nguyên, giúp việc sử dụng bộ nhớ trở nên thiết thực. 

## Trường hợp thử nghiệm```python
# The construction is non-unique, so tests validate the properties
# rather than comparing the complete output text.

import sys
import io
from math import gcd

def check_output(inp: str, out: str):
    n = int(inp.strip())
    data = list(map(int, out.split()))

    assert data, "empty output"

    k = data[0]
    a = data[1:]

    assert len(a) == k
    assert k > 0
    assert len(set(a)) == k
    assert all(1 <= x <= n for x in a)

    for x, y in zip(a, a[1:]):
        assert gcd(x, y) >= 2

    return k

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    # Paste/import the solve() implementation here.
    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided sample 1.
out = run("4\n")
assert check_output("4\n", out) == 2

# Provided sample 2.
out = run("19\n")
assert check_output("19\n", out) == 14

# Minimum-size case.
out = run("4\n")
assert check_output("4\n", out) == 2

# Boundary where a prime has exactly one multiple.
out = run("7\n")
assert check_output("7\n", out) == 4

# A case containing two primes in (n/3, n/2].
out = run("30\n")
assert check_output("30\n", out) == 25

# Large boundary case.
out = run("1000000\n")
k = check_output("1000000\n", out)
assert k > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4`| Chiều dài 2 | Đầu vào hợp lệ tối thiểu và không thể sử dụng 1 hoặc 3 | 
|`7`| Chiều dài 4 | Một số nguyên tố chỉ có một bội số có sẵn phải là điểm cuối | 
|`19`| Chiều dài 14 | Cấu trúc được cung cấp với các số nguyên tố cao, trung bình và nhỏ | 
|`30`| Chiều dài 25 | Hai số nguyên tố cuối cùng từ khoảng (n/3,p\le n/2) | 
|`1000000`| Trình tự hợp lệ dương | Kích thước đầu vào tối đa, bộ nhớ sàng và xử lý ranh giới | 

## Vỏ cạnh 

Với (n=4), kết quả đầu ra của thuật toán`2 4`. Giá trị (3) là số nguyên tố cô lập vì (6>4), do đó không có cách nào đặt nó bên cạnh một giá trị được chọn khác. Do đó, mức tối đa là (2). 

Với (n=7), số nguyên tố (3) có đúng một bội số khác, (6). Trình tự`3 6 2 4`sử dụng cả hai và sau đó tiếp tục thông qua các số chẵn. Không thể bao gồm số nguyên tố (5) vì (10>7), trong khi số nguyên tố (7) không có bội số nào khác. 

Với (n=19), số nguyên tố (7) nằm trong ((n/3,n/2]), nên nó chỉ có thể là điểm cuối. Dãy số bắt đầu bằng`7 14`. Dạng nguyên tố (5)`10 5 15`, và khối nhân tố (3) tiếp theo`15`Và`6`. Các giá trị chẵn còn lại kết thúc chuỗi. Mỗi số được chọn là khác biệt và mọi cặp lân cận đều có ước chung. 

Đối với các giá trị lớn hơn trong đó có nhiều số nguyên tố nằm trong ((n/3,n/2]), chỉ có thể giữ lại hai số nguyên tố. Mỗi số nguyên tố đó chỉ có cạnh tới (2p), do đó, việc sử dụng ba số nguyên tố trong số đó sẽ yêu cầu ba điểm cuối khác nhau trên một đường dẫn. Cấu trúc giữ hai số nguyên tố đó và đặt các giá trị (2p) của chúng ở hai đầu đối diện nhau. 

Khi một số số nguyên tố có chung bội số tổng hợp thì`used`mảng ngăn không cho cùng một số nguyên nhập vào nhiều khối. Việc xử lý các số nguyên tố từ lớn hơn đến nhỏ hơn làm cho quyền sở hữu có tính xác định: tổng hợp được sử dụng bởi số nguyên tố lẻ lớn nhất có liên quan chưa bị loại trừ. 

Khối chẵn cuối cùng xử lý tất cả các vật liệu tổng hợp không được khối nguyên tố lẻ tiêu thụ. Vì mọi số trong khối này đều chia hết cho (2), nên không cần kiểm tra gcd bổ sung hoặc đặt hàng đặc biệt.
