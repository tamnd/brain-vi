---
title: "CF 102218J - Chỉ là một nhiệm vụ dễ dàng"
description: "Chúng ta cần xác định, với mỗi ngày (k) từ (0) đến (n-1), có bao nhiêu cặp thứ tự ((i,j)) thỏa mãn [ icdot j Equiv k pmod n."
date: "2026-08-17T23:24:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "J"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 179
verified: false
draft: false
---

[CF 102218J - Chỉ là một nhiệm vụ dễ dàng](https://codeforces.com/problemset/problem/102218/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xác định, với mỗi ngày (k) từ (0) đến (n-1), có bao nhiêu cặp thứ tự ((i,j)) thỏa mãn 

[ 
i\cdot j \equiv k \pmod n. 
] 

Mỗi cặp như vậy đóng góp một đơn vị công suất cho ngày (k), vì vậy mảng cần có chính xác là phân bố tần số của các sản phẩm (i j \bmod n) trên tất cả (n^2) cặp được đặt hàng. Tuyên bố chính thức xác nhận rằng các ngày được lập chỉ mục từ (0) đến (n-1), với một đóng góp cho mỗi cặp có thứ tự trong phạm vi đó. 

Mô phỏng trực tiếp xem xét tất cả (n^2) cặp. Với (n) lớn bằng (2,2\times10^6), điều đó có nghĩa là phép nhân mô-đun lên tới (4,84\times10^{12}), vượt xa những gì việc triển khai trong hai giây có thể thực hiện. Ngay cả cách tiếp cận (O(n\sqrt n)) cũng sẽ quá lớn ở quy mô này. Lời giải cần khai thác cấu trúc số học của phép nhân modulo (n), thay vì liệt kê các cặp. 

Phần dư bằng 0 cần được quan tâm đặc biệt vì (i=0) đóng góp vào nó với mọi (j) và mọi phần dư khác không (i) cũng đóng góp bất cứ khi nào (ij) chia hết cho (n). Với (n=1) chỉ có cặp ((0,0)) nên đáp án đơn giản là`1`. Một giải pháp giả định mô đun dương có nhiều hơn một dư lượng có thể dễ dàng xử lý sai trường hợp này. 

Sai lầm phổ biến thứ hai là coi phép nhân theo modulo là hợp số như thể mọi số nhân khác 0 đều có thể nghịch đảo. Ví dụ: với (n=4), kết quả đúng là```
8
2
4
2
```Giá trị (2) xuất hiện bốn lần vì (0\cdot2), (2\cdot1), (2\cdot3) và (2\cdot2) không phải là lý do đúng cho dư lượng trực tiếp. Một cách hệ thống hơn, số lượng nghiệm phụ thuộc vào (\gcd(i,n)). Một cách tiếp cận bất cẩn chỉ dựa trên nghịch đảo mô-đun sẽ bỏ lỡ các giải pháp bổ sung do các bội số không nguyên tố gây ra. 

Đối với môđun nguyên tố như (n=5), mọi số nhân khác 0 đều có thể nghịch đảo. Câu trả lời là```
9
4
4
4
4
```Tất cả các dư lượng khác không có cùng tần số, trong khi số 0 có tần số lớn hơn. Việc triển khai giả định tất cả dư lượng phải có số lượng bằng nhau sẽ thất bại ngay cả trong trường hợp nhỏ này. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa chính xác. Tạo một mảng gồm (n) bộ đếm, lặp lại mọi (i) và mọi (j), tính toán ((i j)\bmod n) và tăng bộ đếm tương ứng. Mỗi cặp được xử lý một lần nên kết quả là chính xác. Vấn đề là số lượng cặp. Ở mức tối đa (n=2{,}200{,}000), có (2{,}200{,}000^2=4{,}840{,}000{,}000{,}000) cặp, khiến phương pháp này không thể sử dụng được. 

Điều quan trọng là ngừng hỏi cặp riêng lẻ nào tạo ra dư lượng mà thay vào đó hãy hỏi có bao nhiêu giá trị (j) tạo ra dư lượng cố định cho một (i) cụ thể. Hãy xem xét sự phù hợp 

[ 
ij\equiv k\pmod n. 
] 

Đặt (g=\gcd(i,n)). Một tính chất tiêu chuẩn của sự đồng đẳng tuyến tính nói rằng phương trình này có nghiệm chính xác khi (g\mid k), và khi nó giải được thì nó có chính xác (g) nghiệm modulo (n). 

Điều này ngay lập tức cho chúng ta biết một số nhân (i) đóng góp gì. Nếu (g=\gcd(i,n)), thì (i) đóng góp (g) vào mọi vị trí trả lời (k) chia hết cho (g) và đóng góp 0 cho tất cả các vị trí khác. 

Câu hỏi tiếp theo là có bao nhiêu giá trị của (i) có gcd cụ thể với (n). Giả sử (g\mid n). Viết 

[ 
i=gx,\qquad n=gm 
] 

cho 

[ 
\gcd(i,n)=g 
] 

chính xác khi (\gcd(x,m)=1). Vì (x) nằm trong phạm vi (0,\ldots,m-1), nên có (\varphi(m)) các giá trị như vậy. Trường hợp (i=0) được đưa vào đây vì (\gcd(0,n)=n), tương ứng với (m=1) và (\varphi(1)=1). 

Do đó, với mọi ước số (g) của (n), chính xác 

[ 
\varphi\left(\frac ng\right) 
] 

các giá trị của (i) có gcd (g). Mỗi (i) đó đóng góp (g) vào mọi (k) chia hết cho (g). Do đó, tổng đóng góp liên quan đến ước số (g) là 

[ 
g\varphi\left(\frac ng\right) 
] 

với mọi bội số của (g). 

Vậy công thức cuối cùng là 

[ 
\đóng hộp{ 
c_k= 
\sum_{\substack{g\mid n\g\mid k}} 
g\varphi\left(\frac ng\right) 
} 
] 

hoặc tương đương, 

[ 
c_k= 
\sum_{g\mid\gcd(k,n)} 
g\varphi\left(\frac ng\right). 
] 

Bây giờ chúng ta chỉ cần liệt kê các ước của (n). Với mỗi ước số (g), cộng trọng số của nó (g\varphi(n/g)) vào các vị trí (0,g,2g,\ldots). Tổng số lần cập nhật mảng là 

[ 
\sum_{g\mid n}\frac nd=\sum_{g\mid n}\frac ng=\sigma(n), 
] 

theo quy ước điểm cuối vô hại. Con số này nhỏ hơn đáng kể so với (n^2). Đối với đầu vào tối đa, (2{,}200{,}000=2^6\cdot5^5\cdot11), do đó, nó chỉ có 84 ước và tổng các ước của nó chỉ là (5{,}952{,}744). 

Trước tiên, chúng ta có thể phân tích (n) thành nhân tử, tạo ra tất cả các ước của nó và tính toán (\varphi(n/g)) trực tiếp từ việc phân tích thành thừa số nguyên tố. Không cần sàng lên tới (n), điều này giúp việc triển khai vừa đơn giản vừa tiết kiệm bộ nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(\sqrt n+\sigma(n))) | (O(n+\tau(n))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích (n) thành lũy thừa nguyên tố (n=\prod p^a). Phép chia thử là đủ vì (n\le2.2\times10^6), vì vậy chỉ cần kiểm tra các ước số ứng cử viên (O(\sqrt n)). 
2. Tạo mọi ước số (g) của (n). Trong thế hệ này, hãy tính toán (\varphi(n/g)). Nếu (p^b) là lũy thừa còn lại của một số nguyên tố trong (n/g), thì đóng góp của nó vào tổng là (1) khi (b=0) và (p^{b-1}(p-1)) trong trường hợp khác. 
3. Với mỗi ước số (g), hãy tính trọng số của nó 

[ 
w=g\varphi(n/g). 
] 

Các giá trị của (i) thỏa mãn (\gcd(i,n)=g) chính xác là (\varphi(n/g)) về số lượng, và mỗi giá trị (i) như vậy đóng góp (g) nghiệm cho mọi dư lượng chia hết cho (g).

1. Thêm (w) vào mỗi vị trí mảng chia hết cho (g). Các vị trí bị ảnh hưởng là (0,g,2g,\ldots,n-g). Vị trí số 0 được đưa vào một cách có chủ ý vì số 0 chia hết cho mọi ước số dương. 
2. Sau khi xử lý từng ước số, xuất ra mảng kết quả. Mỗi cặp có thứ tự đã được tính theo gcd của tọa độ đầu tiên của nó, do đó giá trị tích lũy tại vị trí (k) chính xác là số cặp có tích bằng (k) modulo (n). 

### Tại sao nó hoạt động 

Khắc phục dư lượng (k). Phân vùng tất cả các tọa độ đầu tiên có thể có (i) theo (g=\gcd(i,n)). Với một (i) như vậy, sự đồng dư (ij\equiv k\pmod n) có (g) nghiệm cho (j) khi (g\mid k), và không có nghiệm nào khác. Có chính xác (\varphi(n/g)) tọa độ đầu tiên với gcd (g). Do đó, tất cả tọa độ đầu tiên trong nhóm này đóng góp chính xác (g\varphi(n/g)) vào (c_k) khi (g\mid k). Thuật toán cộng chính xác số lượng đó với mọi bội số của (g), do đó, mỗi cặp hợp lệ đóng góp một lần và mọi cặp không hợp lệ đóng góp bằng 0. Tổng hợp tất cả các ước số sẽ cho công suất chính xác của mỗi ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factorize(n):
    factors = []

    if n % 2 == 0:
        e = 0
        while n % 2 == 0:
            n //= 2
            e += 1
        factors.append((2, e))

    p = 3
    while p * p <= n:
        if n % p == 0:
            e = 0
            while n % p == 0:
                n //= p
                e += 1
            factors.append((p, e))
        p += 2

    if n > 1:
        factors.append((n, 1))

    return factors

def generate_terms(factors):
    terms = []

    def dfs(pos, divisor, phi_quotient):
        if pos == len(factors):
            terms.append((divisor, phi_quotient))
            return

        p, a = factors[pos]

        p_powers = [1]
        for _ in range(a):
            p_powers.append(p_powers[-1] * p)

        for e in range(a + 1):
            remaining = a - e

            if remaining == 0:
                phi_part = 1
            else:
                phi_part = (p - 1) * p_powers[remaining - 1]

            dfs(
                pos + 1,
                divisor * p_powers[e],
                phi_quotient * phi_part
            )

    dfs(0, 1, 1)
    return terms

def solve():
    n = int(input())

    factors = factorize(n)
    terms = generate_terms(factors)

    ans = [0] * n

    for divisor, phi_quotient in terms:
        weight = divisor * phi_quotient

        for k in range(0, n, divisor):
            ans[k] += weight

    sys.stdout.write('\n'.join(map(str, ans)))

if __name__ == "__main__":
    solve()
```các`factorize`hàm trích xuất các lũy thừa nguyên tố của (n). Vì căn bậc hai của đầu vào lớn nhất có thể chỉ khoảng 1484 nên phép chia thử rất nhỏ so với công việc đầu ra chính. 

Đệ quy`generate_terms`hàm liệt kê các ước số bằng cách sử dụng hệ số nguyên tố. Nếu (n) chứa (p^a), chọn số mũ (e) cho (p) bên trong ước số (g) để số mũ (a-e) bên trong (n/g). Đoạn mã sẽ tính toán tổng hệ số tương ứng ngay lập tức, do đó mỗi cặp được tạo ra đều chính xác`(g, phi(n/g))`. 

Vòng lặp chính thực hiện đóng góp số chia một cách trực tiếp. Đối với một số chia`divisor`, giá trị`weight`là (g\varphi(n/g)). Phạm vi bắt đầu ở số 0 chứ không phải ở`divisor`, bởi vì số dư bằng 0 có thể chia hết cho mọi ước số và nhận được sự đóng góp từ mọi lớp gcd. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Trong ngôn ngữ có chiều rộng cố định, số nguyên 64 bit là loại thích hợp vì dung lượng riêng lẻ có thể lớn hơn nhiều so với (2^{31}-1). 

Mảng câu trả lời sử dụng cách biểu diễn danh sách của Python. Ở (2,2) triệu vị trí, điều này vẫn nằm trong giới hạn bộ nhớ 256 MB, đồng thời nhanh hơn đáng kể đối với các phép cộng số nguyên lặp lại so với cấu trúc ánh xạ cấp cao được đóng hộp. 

## Ví dụ đã hoạt động 

Với (n=6), hệ số nguyên tố là (2\cdot3). Dễ dàng rút ra các số hạng chia: 

[ 
\begin{mảng}{c|c|c} 
g & \varphi(6/g) & g\varphi(6/g)\ 
\hline 
1 & \varphi(6)=2 & 2\ 
2 & \varphi(3)=2 & 4\ 
3 & \varphi(2)=1 & 3\ 
6 & \varphi(1)=1 & 6 
\end{mảng} 
] 

Dấu vết của các bản cập nhật mảng là: 

| Số chia (g) | Cân nặng | Vị trí được cập nhật | Mảng sau khi cập nhật | 
| --- | --- | --- | --- | 
| 1 | 2 | 0, 1, 2, 3, 4, 5 | 2, 2, 2, 2, 2, 2 | 
| 2 | 4 | 0, 2, 4 | 6, 2, 6, 2, 6, 2 | 
| 3 | 3 | 0, 3 | 9, 2, 6, 5, 6, 2 | 
| 6 | 6 | 0 | 15, 2, 6, 5, 6, 2 | 

Mảng cuối cùng chính xác là đầu ra mẫu. Dấu vết cho thấy tại sao số 0 nhận được sự đóng góp từ mọi ước số, trong khi mỗi số dư khác 0 chỉ nhận được trọng số của các ước số của chính nó. 

Với (n=5), là số nguyên tố, ước số duy nhất là (1) và (5). 

| Số chia (g) | Cân nặng | Vị trí được cập nhật | Mảng sau khi cập nhật | 
| --- | --- | --- | --- | 
| 1 | (\varphi(5)=4) | 0, 1, 2, 3, 4 | 4, 4, 4, 4, 4 | 
| 5 | (5\varphi(1)=5) | 0 | 9, 4, 4, 4, 4 | 

Điều này thể hiện trường hợp đặc biệt của môđun nguyên tố. Mọi số dư khác 0 đều nhận được bốn phần đóng góp giống nhau, bởi vì mọi số nhân khác 0 đều có thể nghịch đảo modulo một số nguyên tố. Zero nhận được năm đóng góp bổ sung từ hệ số nhân (i=0). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\sqrt n+\sigma(n))) | Chi phí bao thanh toán (O(\sqrt n)) và các vòng lặp cập nhật số chia thực hiện các lần lặp (\sum_{g\mid n}n/g=\sigma(n)) | 
| Không gian | (O(n+\tau(n))) | Mảng câu trả lời có (n) mục và danh sách ước có (\tau(n)) mục | 

Sự khác biệt quan trọng so với bạo lực là số lần cập nhật số học được gắn với cấu trúc ước số của (n), chứ không phải (n^2). Ở đầu vào tối đa, (n) chỉ có 84 ước số và (\sigma(n)=5{,}952{,}744), do đó giai đoạn cập nhật vẫn nhỏ so với các phép toán (4,84\times10^{12}) được yêu cầu bởi phép liệt kê trực tiếp. Mức tiêu thụ bộ nhớ bị chi phối bởi mảng câu trả lời phần tử (n) và vừa vặn trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data: str) -> str:
    n = int(data.strip())

    def factorize(x):
        factors = []

        if x % 2 == 0:
            e = 0
            while x % 2 == 0:
                x //= 2
                e += 1
            factors.append((2, e))

        p = 3
        while p * p <= x:
            if x % p == 0:
                e = 0
                while x % p == 0:
                    x //= p
                    e += 1
                factors.append((p, e))
            p += 2

        if x > 1:
            factors.append((x, 1))

        return factors

    factors = factorize(n)
    terms = []

    def dfs(pos, divisor, phi_quotient):
        if pos == len(factors):
            terms.append((divisor, phi_quotient))
            return

        p, a = factors[pos]

        powers = [1]
        for _ in range(a):
            powers.append(powers[-1] * p)

        for e in range(a + 1):
            remaining = a - e

            if remaining == 0:
                phi_part = 1
            else:
                phi_part = (p - 1) * powers[remaining - 1]

            dfs(
                pos + 1,
                divisor * powers[e],
                phi_quotient * phi_part
            )

    dfs(0, 1, 1)

    ans = [0] * n

    for divisor, phi_quotient in terms:
        weight = divisor * phi_quotient
        for k in range(0, n, divisor):
            ans[k] += weight

    return '\n'.join(map(str, ans))

# Provided sample
assert solution("6") == "15\n2\n6\n5\n6\n2", "sample 1"

# Minimum input
assert solution("1") == "1", "n = 1"

# Prime n, all nonzero residues have equal capacities
assert solution("5") == "9\n4\n4\n4\n4", "prime modulus"

# Composite n with repeated prime factors
assert solution("4") == "8\n2\n4\n2", "composite modulus"

# Maximum-size input.
# Checking the complete 2.2-million-line string directly would waste memory,
# so verify its size and boundary values.
maximum = solution("2200000")
maximum_lines = maximum.splitlines()

assert len(maximum_lines) == 2200000, "maximum n output length"
assert maximum_lines[0] == "84000000", "maximum n c[0]"
assert maximum_lines[-1] == "800000", "maximum n c[n-1]"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Kích thước tối thiểu và vai trò đặc biệt của dư lượng bằng không | 
|`5`|`9, 4, 4, 4, 4`| Mô đun nguyên tố và công suất khác 0 bằng nhau | 
|`4`|`8, 2, 4, 2`| Mô đun tổng hợp và hệ số nhân không khả nghịch | 
|`2200000`| 2.200.000 dòng, đầu tiên`84000000`, cuối cùng`800000`| Kích thước đầu vào tối đa, ranh giới đầu ra và hiệu suất | 

## Vỏ cạnh 

Với (n=1), cặp duy nhất có thể là ((0,0)). Hệ số hóa không có thừa số nguyên tố, do đó bộ tạo số chia chỉ tạo ra (g=1), với (\varphi(1)=1). Vòng lặp cập nhật thêm (1) vào vị trí 0, tạo ra chính xác```
1
```Một giải pháp bắt đầu liệt kê số chia từ`2`sẽ âm thầm bỏ lỡ sự đóng góp duy nhất. 

Đối với (n=4), các đóng góp của số chia cho thấy lý do tại sao các mô đun tổng hợp cần đối số gcd. Các số hạng là (g=1) với trọng số (\varphi(4)=2), (g=2) với trọng số (2\varphi(2)=2) và (g=4) với trọng lượng (4\varphi(1)=4). Thuật ngữ đầu tiên cập nhật mọi vị trí, thuật ngữ thứ hai cập nhật vị trí 0 và 2, còn thuật ngữ thứ ba chỉ cập nhật 0. Kết quả là```
8
2
4
2
```Vị trí 0 nhận được (2+2+4=8), trong khi vị trí thứ hai nhận được (2+2=4). Điều này nắm bắt các triển khai giả định rằng mọi hệ số nhân khác 0 đều có chính xác một nghịch đảo mô-đun. 

Với (n=5), ước số duy nhất là (1) và (5). Số chia (1) đóng góp (\varphi(5)=4) cho mọi vị trí, trong khi số chia (5) chỉ đóng góp (5) cho số 0. Kết quả là```
9
4
4
4
4
```Điều này mắc phải sai lầm ngược lại, trong đó nghiệm coi số 0 như một số dư thông thường và quên rằng hệ số nhân (i=0) đóng góp vào số 0 đối với mọi (j). 

Đối với mức tối đa (n=2{,}200{,}000), hệ số nguyên tố là (2^6\cdot5^5\cdot11), cho 84 ước số. Các vòng cập nhật chỉ thực hiện phép cộng (5{,}952{,}744), trong khi đầu ra vẫn chứa tất cả (2,2) triệu dung lượng. Giá trị đầu tiên là (84{,}000{,}000), lấy từ 

[ 
\sum_{g\mid n}g\varphi(n/g), 
] 

và giá trị cuối cùng, tương ứng với phần dư (n-1), là (800{,}000=\varphi(n)), vì (\gcd(n-1,n)=1). Trường hợp này thực hiện cả hành vi tiệm cận dự kiến ​​và ranh giới mảng tại vị trí 0 và (n-1).
