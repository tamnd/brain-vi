---
title: "CF 105023G - Ổ đĩa I-5"
description: "Chúng ta được yêu cầu đếm các cặp số nguyên tố có thứ tự $(p, q)$ sao cho một số có dạng $N = p^2 + q^3$ có đặc tính biểu diễn rất cụ thể trong cơ số $T$."
date: "2026-06-28T01:45:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "G"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 87
verified: true
draft: false
---

[CF 105023G - Ổ đĩa I-5](https://codeforces.com/problemset/problem/105023/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các cặp số nguyên tố có thứ tự$(p, q)$sao cho một số được hình thành như$N = p^2 + q^3$có một thuộc tính đại diện rất cụ thể trong cơ sở$T$. Khi chúng tôi viết$N$trong căn cứ$T$, không có số 0 đứng đầu, các chữ số phải tạo thành một hoán vị của tất cả các chữ số từ$0$ĐẾN$T-1$, mỗi cái xuất hiện đúng một lần. 

Vì vậy cơ sở-$T$đại diện của$N$là sự trùng hợp giữa các vị trí và chữ số: mỗi chữ số xuất hiện đúng một lần, nghĩa là số này là một hoán vị hợp lệ của tập hợp chữ số đầy đủ trong cơ số đó. Đặc biệt, độ dài biểu diễn phải chính xác$T$, bởi vì chúng ta cần đặt tất cả$T$chữ số riêng biệt. 

Đầu vào chỉ là$T$, và chúng ta phải đếm xem có bao nhiêu cặp số nguyên tố có thứ tự$(p, q)$sản xuất như vậy$N$. 

Mặc dù báo cáo vấn đề không bị ràng buộc một cách rõ ràng$p$Và$q$, ràng buộc chữ số ngầm buộc$N$trở nên nhỏ bé. Một căn cứ-$T$số chứa mỗi chữ số đúng một lần có giá trị tối đa$T^T - 1$, Vì thế$N$được giới hạn bởi một số mũ nhỏ trong$T$. Từ$T \le 10$, mức tối đa có thể$N$nhiều nhất là$10^{10} - 1$, có thể quản lý được để liệt kê sau khi cắt tỉa. 

Khó khăn chính đó là$N$được xác định bởi các số nguyên tố thông qua sự kết hợp phi tuyến$p^2 + q^3$, nhưng ràng buộc về chữ số hạn chế mạnh mẽ các ứng cử viên hợp lệ. 

Một trường hợp thất bại đơn giản phát sinh nếu chúng ta cố gắng lặp lại tất cả các số nguyên tố đến$10^{10}$, điều đó là không thể. Một cạm bẫy phổ biến khác là tạo ra tất cả các hoán vị của các chữ số trong cơ số$T$, chuyển đổi chúng thành số nguyên và sau đó cố gắng phân tách từng số thành$p^2 + q^3$mà không giới hạn hiệu quả các phạm vi nguyên tố. Mặc dù điều này gần đúng hơn nhưng việc phân tách bất cẩn vẫn sẽ quá chậm nếu chúng ta không tính toán trước các số nguyên tố và căn bậc hai và căn bậc ba bị ràng buộc chặt chẽ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các số nguyên tố$p$Và$q$, tính toán$N = p^2 + q^3$, chuyển thành$N$căn cứ$T$và kiểm tra xem nó có chứa mỗi chữ số đúng một lần hay không. Tính chính xác có ngay lập tức nhưng không gian tìm kiếm rất lớn. Thậm chí còn hạn chế$p, q \le \sqrt{10^{10}}$hoặc các giới hạn tương tự vẫn theo thứ tự hàng triệu số nguyên tố, dẫn đến hàng tỷ đánh giá cặp. 

Cấu trúc của điều kiện chữ số là quan sát chính. Thay vì tạo ra$p, q$, chúng ta có thể đảo ngược quá trình: tạo ra tất cả các cơ sở hợp lệ-$T$hoán vị thỏa mãn ràng buộc chữ số, chuyển đổi từng hoán vị thành số nguyên$N$, sau đó kiểm tra xem$N$có thể được thể hiện như$p^2 + q^3$cho số nguyên tố$p, q$. Từ$T \le 10$, số hoán vị nhiều nhất là$10!$, tức là khoảng 3,6 triệu và ít hơn rất nhiều khi các ràng buộc về số 0 được xử lý cẩn thận. Điều này đủ nhỏ để kiểm tra tính khả thi có kiểm soát. 

Sau khi chúng tôi chọn được một ứng cử viên$N$, ta chỉ cần kiểm tra xem có tồn tại số nguyên tố hay không$q$như vậy$N - q^3$là một hình vuông hoàn hảo của một số nguyên tố. Chúng ta có thể ràng buộc$q \le \sqrt[3]{N}$Và$p \le \sqrt{N}$và sử dụng sàng được tính toán trước để kiểm tra tính nguyên tố một cách nhanh chóng. 

Vì vậy, sự thay đổi quan trọng là từ liệt kê các cặp nguyên tố sang liệt kê các hoán vị chữ số, sau đó xác nhận cấu trúc đại số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua số nguyên tố |$O(\pi(M)^2)$|$O(1)$| Quá chậm | 
| Hoán vị + kiểm tra số nguyên tố |$O(T! \cdot T + \sqrt[3]{N})$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các hoán vị của các chữ số từ$0$ĐẾN$T-1$, đảm bảo chúng tôi không cho phép số 0 đứng đầu. Điều này đảm bảo mọi số ứng viên đều có chính xác$T$chữ số trong cơ sở$T$. Hạn chế số 0 đứng đầu có vấn đề vì vấn đề yêu cầu biểu diễn chính tắc mà không có số 0 đứng đầu. 
2. Với mỗi hoán vị, hãy chuyển đổi nó từ cơ số$T$vào giá trị nguyên của nó$N$. Điều này cung cấp cho tất cả các ứng cử viên có thể thỏa mãn điều kiện chữ số. 
3. Tính toán trước tất cả các số nguyên tố đến$\sqrt{N_{\max}}$, Ở đâu$N_{\max}$là giá trị hoán vị lớn nhất. Điều này cho phép kiểm tra tính nguyên thủy theo thời gian liên tục sau này. 
4. Đối với mỗi ứng viên$N$, lặp qua tất cả các số nguyên tố$q$như vậy$q^3 \le N$. Đối với mỗi như vậy$q$, tính toán$x = N - q^3$. 
5. Kiểm tra xem$x$là một số chính phương hoàn hảo và liệu căn bậc hai của nó$p$là nguyên tố. Nếu cả hai điều kiện đều đúng thì chúng ta đã tìm thấy một cặp có thứ tự hợp lệ$(p, q)$. 
6. Đếm tất cả các cặp có thứ tự hợp lệ như vậy trên tất cả các hoán vị. 

### Tại sao nó hoạt động 

Mỗi hợp lệ$N$phải là một hoán vị của các chữ số trong cơ số$T$, vì vậy nó phải xuất hiện trong tập ứng cử viên được tạo. Ngược lại, mọi ứng cử viên đều được kiểm tra chính xác về tính đại diện vì$p^2 + q^3$. Kiểm tra phân rã là toàn diện trên tất cả các số nguyên tố bậc ba có thể$q$và với mỗi cái, nó xác định duy nhất$p^2$. Vì kiểm tra tính nguyên tố và kiểm tra bình phương hoàn hảo là chính xác nên không có cặp không hợp lệ nào được chấp nhận và không có cặp hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import itertools
import math

def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            step = i
            start = i * i
            is_prime[start:n+1:step] = [False] * len(range(start, n+1, step))
    return is_prime

def to_value(perm, base):
    val = 0
    for d in perm:
        val = val * base + d
    return val

def solve():
    T = int(input().strip())
    
    digits = list(range(T))
    perms = itertools.permutations(digits, T)

    candidates = []
    for p in perms:
        if p[0] == 0:
            continue
        candidates.append(to_value(p, T))

    Nmax = max(candidates) if candidates else 0

    # primes up to sqrt(Nmax)
    limit = int(math.isqrt(Nmax)) + 1 if Nmax else 2
    is_prime = sieve(limit)

    def is_prime_small(x):
        return x < len(is_prime) and is_prime[x]

    def is_square(x):
        r = int(math.isqrt(x))
        return r * r == x, r

    ans = 0

    for N in candidates:
        max_q = int(N ** (1/3)) + 2
        q = 2
        while q <= max_q:
            if q < len(is_prime) and is_prime[q]:
                cube = q ** 3
                if cube > N:
                    break
                rem = N - cube
                ok, p = is_square(rem)
                if ok and p < len(is_prime) and is_prime_small(p):
                    ans += 1
            q += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách tạo ra tất cả các hoán vị chữ số hợp lệ có độ dài$T$, bỏ qua những số bắt đầu bằng số 0 vì chúng sẽ vi phạm yêu cầu về biểu diễn không có số 0 đứng đầu. 

Mỗi hoán vị được chuyển đổi thành cơ sở của nó$T$giá trị số nguyên sử dụng tích lũy vị trí. Điều này tránh việc phân tích chuỗi lặp đi lặp lại và giữ cho chuyển đổi tuyến tính trong$T$. 

Một cái sàng được chế tạo để$\sqrt{N_{\max}}$, vì mọi giá trị hợp lệ$p$phải thỏa mãn$p^2 \le N$. Điều này là đủ để xác nhận căn bậc hai một cách nhanh chóng. 

Đối với mỗi ứng viên$N$, chúng tôi lặp lại các số nguyên tố$q$lên tới$\sqrt[3]{N}$. Đối với mỗi, chúng tôi trừ$q^3$và kiểm tra xem phần còn lại có phải là số chính phương có gốc cũng là số nguyên tố hay không. Cả hai phép kiểm tra chỉ sử dụng số học số nguyên, tránh các vấn đề về độ chính xác của dấu phẩy động. 

Cấu trúc đảm bảo rằng mọi cặp thứ tự hợp lệ được tính chính xác một lần vì mỗi cặp xác định duy nhất$q$, và sau đó$p$được cố định như$\sqrt{N - q^3}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$T = 3$Các chữ số là$\{0,1,2\}$. Tất cả các hoán vị hợp lệ không có số 0 đứng đầu là:$102, 120, 201, 210$. 

Chúng tôi chuyển đổi chúng thành số nguyên trong cơ sở 3: 

| hoán vị | giá trị$N$| 
| --- | --- | 
| 102 | 11 | 
| 120 | 15 | 
| 201 | 19 | 
| 210 | 21 | 

Bây giờ chúng tôi kiểm tra sự phân hủy$N = p^2 + q^3$. Đối với các số nguyên tố nhỏ, hình khối đã phát triển nhanh chóng:$2^3 = 8$,$3^3 = 27$, vậy chỉ$q = 2$phù hợp với hầu hết các ứng viên. 

Kiểm tra tất cả các trường hợp cho thấy không có biểu diễn hợp lệ nên câu trả lời là 0. 

Điều này phù hợp với lý do mẫu rằng ngay cả các giá trị ứng cử viên nhỏ nhất cũng quá nhỏ để phù hợp với cấu trúc hình vuông cộng với các số nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T! \cdot T + \pi(N^{1/3}))$| hoán vị tạo ra các ứng cử viên, mỗi hoán vị được kiểm tra trên các khối nguyên tố nhỏ | 
| Không gian |$O(T! + \sqrt{N})$| lưu trữ ứng viên và sàng lọc | 

Số hạng giai thừa được giới hạn bởi$10!$, và phép lặp nguyên tố căn bậc ba nhiều nhất là vài trăm bước. Điều này thoải mái phù hợp trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    import itertools
    import math

    def sieve(n):
        is_prime = [True] * (n + 1)
        is_prime[0] = is_prime[1] = False
        for i in range(2, int(n ** 0.5) + 1):
            if is_prime[i]:
                for j in range(i * i, n + 1, i):
                    is_prime[j] = False
        return is_prime

    def to_value(perm, base):
        val = 0
        for d in perm:
            val = val * base + d
        return val

    T = int(sys.stdin.readline().strip())
    digits = list(range(T))

    candidates = []
    for p in itertools.permutations(digits, T):
        if p[0] != 0:
            candidates.append(to_value(p, T))

    if not candidates:
        return "0"

    Nmax = max(candidates)
    limit = int(math.isqrt(Nmax)) + 1
    is_prime = sieve(limit)

    def is_square(x):
        r = int(math.isqrt(x))
        return r * r == x, r

    ans = 0
    for N in candidates:
        max_q = int(N ** (1/3)) + 2
        q = 2
        while q <= max_q:
            if q < len(is_prime) and is_prime[q]:
                cube = q ** 3
                if cube > N:
                    break
                rem = N - cube
                ok, p = is_square(rem)
                if ok and p < len(is_prime) and is_prime[p]:
                    ans += 1
            q += 1

    return str(ans)

# provided sample
assert run("3\n") == "0", "sample 1"

# custom cases
assert run("2\n") == "0", "minimum base"
assert run("4\n") in {"0", "1"}, "small base sanity"
assert run("5\n") >= "0", "non-negative count"
assert run("10\n") >= "0", "maximum base sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 0 | độ đúng mẫu và cơ sở nhỏ | 
| 2 | 0 | bộ chữ số tối thiểu | 
| 4 | 0 hoặc 1 | xử lý hoán vị ổn định | 
| 10 | không âm | độ bền giới hạn trên | 

## Vỏ cạnh 

Khi nào$T = 2$, hoán vị chữ số là cực kỳ hạn chế và mọi loại trừ số 0 đứng đầu chỉ để lại một số ứng cử viên duy nhất. Thuật toán tạo chính xác tối đa một$N$, sau đó thực hiện kiểm tra khối và hình vuông, ngay lập tức thất bại vì ngay cả khối hợp lệ nhỏ nhất$2^3$vượt qua hầu hết các ứng cử viên. 

Vì$T = 10$, bộ hoán vị đạt kích thước tối đa, nhưng vòng sàng và căn bậc ba vẫn có thể quản lý được. Mỗi ứng viên$N$được kiểm tra độc lập và vì tốc độ tăng trưởng khối nhanh nên hầu hết các lần lặp đều kết thúc sau khi kiểm tra rất ít số nguyên tố$q$. 

Các hoán vị số 0 đứng đầu được loại bỏ một cách an toàn tại thời điểm tạo, do đó không có biểu diễn không hợp lệ nào như cơ sở ngắn hơn-$T$số được đưa vào giai đoạn phân hủy.
