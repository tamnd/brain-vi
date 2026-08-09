---
title: "CF 102460M - DivModulo"
description: "Chúng ta cần tính phần dư đặc biệt của hệ số nhị thức. Cho (M), (N), và (D), với (0 le N le M), xét [ C(M,N)=frac{M!}{N!(M-N)!}. ] Hoạt động DivModulo không chỉ đơn giản lấy số modulo này (D)."
date: "2026-08-08T10:26:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "M"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 524
verified: true
draft: false
---

[CF 102460M - DivModulo](https://codeforces.com/problemset/problem/102460/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tính phần dư đặc biệt của hệ số nhị thức. Cho (M), (N), và (D), với (0 \le N \le M), xét 

[ 
C(M,N)=\frac{M!}{N!(M-N)!}. 
] 

Hoạt động DivModulo không chỉ đơn giản lấy số modulo (D) này. Thay vào đó, chúng tôi liên tục loại bỏ hệ số hoàn chỉnh của (D) khi có thể. Nếu 

[ 
C(M,N)=X D^h 
] 

trong đó (D\nmid X), câu trả lời bắt buộc là (X\bmod D). 

Sự khác biệt này có ý nghĩa bất cứ khi nào hệ số nhị thức chia hết cho (D). Ví dụ: (C(5,2)=10=2\cdot5), do đó modulo thông thường cho (0), trong khi DivModulo cho (2). 

Kích thước đầu vào khiến cho việc tính toán giai thừa trực tiếp là không thể. (M) có thể đạt tới (4\cdot10^{18}), do đó, ngay cả thuật toán (O(M)) cũng sẽ yêu cầu khoảng (4\cdot10^{18}) lần lặp. Giai thừa chính xác cũng quá lớn để xây dựng. Thay vào đó, kích thước hữu ích là (D), tối đa là (1.6\cdot10^7). Điều này gợi ý rõ ràng về một thuật toán có quá trình tiền xử lý tốn kém phụ thuộc vào (D), trong khi các giá trị lớn (M) và (N) được xử lý thông qua các phép truy toán logarit. 

Có một số trường hợp đặc biệt bộc lộ những cách tiếp cận không chính xác. 

Đối với đầu vào`5 2 5`, hệ số nhị thức là (10). Số học mô-đun thông thường cho (10\bmod5=0), nhưng trước tiên phải loại bỏ một thừa số của (5), vì vậy câu trả lời đúng là (2). 

Đối với đầu vào`6 2 6`, hệ số nhị thức là (15). Cả (6) và (6^2) đều không chia hết (15), nên câu trả lời là (15\bmod6=3). Việc triển khai bất cẩn loại bỏ các thừa số nguyên tố của (D) một cách độc lập sẽ loại bỏ thừa số (2) hoặc (3) mặc dù thao tác DivModulo chỉ loại bỏ các thừa số nguyên tố của (6). 

Đối với đầu vào`1 0 2`, hệ số nhị thức là (1), vì (0!=1). Không có thừa số nào của (2) cần loại bỏ nên đáp án là (1). Việc triển khai giả định (N>0) hoặc xử lý một giai thừa trống không chính xác có thể thất bại ở đây. 

Đối với đầu vào`6 3 6`, hệ số nhị thức là (20). Vì (6\nmid20), không có gì bị loại bỏ và câu trả lời là (20\bmod6=2). Đây là một trường hợp biên hữu ích vì giá trị nguyên tố của (20) là khác nhau: có hai thừa số của (2), nhưng không có thừa số của (3). 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng ta có thể tính các giai thừa, lập hệ số nhị thức, chia liên tục cho (D) và cuối cùng lấy phần còn lại. Một phiên bản ít cực đoan hơn có thể tạo ra hệ số nhị thức theo cấp số nhân, nhưng nó vẫn cần các bước số học (\Theta(M)) trong trường hợp xấu nhất. Tại (M=4\cdot10^{18}), điều đó có nghĩa là khoảng bốn triệu triệu lần lặp trước khi xem xét chi phí của các số nguyên khổng lồ bị thao túng. Tam giác Pascal thậm chí còn kém thực tế hơn. Phương pháp brute-force là đúng vì nó tuân theo định nghĩa trực tiếp, nhưng giới hạn đầu vào khiến nó không thể sử dụng được. 

Quan sát quan trọng là việc loại bỏ các yếu tố hoàn chỉnh của (D) là một vấn đề về định giá. Hệ số (D) là 

[ 
D=\prod_{i=1}^s p_i^{a_i}, 
] 

trong đó (p_i) là các số nguyên tố phân biệt. 

hãy để 

[ 
e_i=v_{p_i}(C(M,N)). 
] 

Một bản sao hoàn chỉnh của (D) sử dụng (a_i) bản sao của (p_i) cho mỗi (i). Do đó số bản sao hoàn chỉnh của (D) chứa trong hệ số nhị thức là 

[ 
K=\min_i\left\lfloor\frac{e_i}{a_i}\right\rfloor. 
] 

Do đó, số nguyên cần tìm là 

[ 
R=\frac{C(M,N)}{D^K}. 
] 

Chúng ta không thể tự tính toán (C(M,N)) mà chỉ cần (R) modulo (D). Vì các lũy thừa nguyên tố (p_i^{a_i}) ​​là hai số nguyên tố cùng nhau nên chúng ta có thể tính toán (R) modulo riêng từng lũy ​​thừa nguyên tố và kết hợp các kết quả với Định lý số dư Trung Hoa. 

Sửa một nguồn điện chính 

[ 
q=p^a. 
] 

Giả sử hệ số nhị thức có định giá (p)-adic (e=v_p(C(M,N))). Sau khi loại bỏ (D^K), số mũ (p)-adic còn lại của nó là 

[ 
k=e-aK. 
] 

Viết hệ số nhị thức dưới dạng 

[ 
C(M,N)=p^e U, 
] 

trong đó (U) không chia hết cho (p). Sau đó 

p^k U\left(\frac{D}{q}\right)^{-K}. 
] 

Modulo (q), mẫu số khả nghịch vì (D/q) không chứa thừa số (p). Do đó nhiệm vụ còn lại là tính phần không có (p) (U) của hệ số nhị thức modulo (q). 

Đối với một giai thừa, xác định 

[ 
F_p(n)=\frac{n!}{p^{v_p(n!)}}. 
] 

Giá trị này luôn nguyên tố cùng nhau với (p), vì vậy nó có modulo nghịch đảo (q). Phần không có (p) của nhị thức là 

[ 
U\tương đương 
F_p(M)F_p(N)^{-1}F_p(M-N)^{-1}\pmod q. 
] 

Sự tái diễn quan trọng đến từ việc tách bội số của (p) bên trong (n!). Mọi bội số của (p) có thể loại bỏ một thừa số (p), để lại chính xác phần không có (p) của số tương ứng sau khi chia cho (p). Vì vậy, 

F_p\left(\left\lfloor\frac np\right\rfloor\right) 
\prod_{\substack{1\le i\le n\p\nmid i}}i 
\pmod q. 
] 

Sản phẩm còn lại có tính tuần hoàn theo khối có chiều dài (q=p^a). Nếu 

[ 
G(x)=\prod_{\substack{1\le i\le x\p\nmid i}}i\pmod q, 
] 

sau đó 

G(q-1)^{\lfloor n/q\rfloor}G(n\bmod q) 
\pmod q. 
] 

Chúng ta có thể tính toán trước (G(x)) cho mỗi (0\le x<q) trong (O(q)) thời gian. Mọi phép tính giai thừa tiếp theo sẽ liên tục thay thế (n) bằng (\lfloor n/p\rfloor), do đó chỉ cần mức (O(\log_p M)). 

Phương pháp brute-force hoạt động vì nó đánh giá định nghĩa giai thừa theo đúng nghĩa đen, nhưng không thành công vì (M) rất lớn. Nhận xét rằng chỉ các giá trị (p)-adic và các phần không có (p) mới quan trọng cho phép chúng ta thay thế phép tính giai thừa thiên văn bằng tiền xử lý (O(D)) và rút gọn theo logarit của (M) và (N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(M)) hoặc tệ hơn | (O(M)) hoặc tệ hơn | Quá chậm | 
| Tối ưu | (O(D+\omega(D)\log^2 M)) | (O(\max p_i^{a_i})) | Đã chấp nhận | 

Ở đây (\omega(D)) là số các thừa số nguyên tố phân biệt của (D). Thuật ngữ (D) xuất phát từ các bảng tiền tố lũy thừa nguyên tố. Số hạng logarit dùng cho phép chia lặp lại cho (p) và lũy thừa mô đun. 

## Hướng dẫn thuật toán

1. Phân tích (D) thành lũy thừa nguyên tố riêng biệt (q_i=p_i^{a_i}). Phép chia thử là đủ vì (D\le1.6\cdot10^7), do đó, việc kiểm tra các ước số tối đa (\sqrt D) chỉ chiếm tối đa khoảng (4000) giá trị thử nghiệm. 
2. Với mọi số nguyên tố (p_i), hãy tính 

[ 
e_i=v_{p_i}(M!)-v_{p_i}(N!)-v_{p_i}((M-N)!). 
] 

Việc định giá giai thừa được đưa ra theo công thức Legendre, 

[ 
v_p(n!)=\left\lfloor\frac np\right\rfloor+ 
\left\lfloor\frac n{p^2}\right\rfloor+ 
\left\lfloor\frac n{p^3}\right\rfloor+\cdots. 
] 

Chỉ các số hạng (O(\log_p M)) là khác 0. 

1. Tính toán 

[ 
K=\min_i\left\lfloor\frac{e_i}{a_i}\right\rfloor. 
] 

Đây chính xác là số bản sao hoàn chỉnh của (D) có thể được loại bỏ khỏi hệ số nhị thức. Mức tối thiểu là cần thiết vì một bản sao của (D) đồng thời tiêu thụ (a_i) bản sao của mọi số nguyên tố (p_i). 

1. Xử lý độc lập từng lũy ​​thừa nguyên tố (q=p^a). Xây dựng một mảng tiền tố 

[ 
G(x)=\prod_{\substack{1\le i\le x\p\nmid i}}i\bmod q. 
] 

Chỉ những số không chia hết cho (p) mới được nhân vào tiền tố. Mọi giá trị trong bảng này đều nguyên tố cùng nhau với (p), điều này sau này làm cho nghịch đảo mô-đun hợp lệ. 

1. Thực hiện hàm giai thừa-đơn vị (F_p(n)). Ở mỗi lần lặp, nhân phần đóng góp từ khối hiện tại, 

[ 
G(q-1)^{\lfloor n/q\rfloor}G(n\bmod q), 
] 

sau đó thay thế (n) bằng (\lfloor n/p\rfloor). Vòng lặp kết thúc sau lần lặp (O(\log_p M)) . 

1. Sử dụng ba giá trị đơn vị giai thừa để thu được phần không có (p) của hệ số nhị thức, 

[ 
U= 
F_p(M)F_p(N)^{-1}F_p(M-N)^{-1}\pmod q. 
] 

Cả ba đơn vị giai thừa đều nguyên tố cùng nhau với (q), do đó tồn tại nghịch đảo. 

1. Hãy để 

[ 
k=e-aK. 
] 

Giá trị DivModulo mong muốn vẫn chứa (p^k) sau khi tất cả các yếu tố (D) hoàn chỉnh đã bị xóa. Do đó dư lượng modulo (q) của nó là 

[ 
r= 
U,p^k 
\left(\frac Dq\right)^{-K} 
\bmod q. 
] 

Nghịch đảo của (D/q) tồn tại modulo (q), bởi vì (q) chứa toàn bộ lũy thừa (p) của (D). 

1. Kết hợp tất cả các đồng dư (R\equiv r_i\pmod{q_i}) bằng Định lý số dư Trung Hoa. Các lũy thừa nguyên tố là hai số nguyên tố cùng nhau và tích của chúng là (D), do đó có chính xác một câu trả lời modulo (D). 

Tại sao nó hoạt động: đối với mỗi số nguyên tố (p_i), thuật toán tách hệ số nhị thức thành lũy thừa chính xác (p_i) và một phần nguyên tố cùng nhau với (p_i). Việc tính toán định giá xác định chính xác có bao nhiêu bản sao hoàn chỉnh của (D) có thể bị xóa trên toàn cầu. Sau khi loại bỏ các bản sao đó, số mũ còn lại của mỗi số nguyên tố sẽ được biết và phần không có (p) còn lại được tính bằng phép truy hồi giai thừa. Do đó, modulo dư mọi thành phần lũy thừa nguyên tố của (D) là chính xác. Sau đó, Định lý số dư Trung Hoa sẽ xây dựng lại modulo số nguyên duy nhất (D), chính xác là kết quả DivModulo. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def factorize(n):
    factors = []
    e = 0
    while n % 2 == 0:
        n //= 2
        e += 1
    if e:
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

def vp_factorial(n, p):
    ans = 0
    while n:
        n //= p
        ans += n
    return ans

def build_prefix(p, q):
    # pref[x] = product of all 1 <= i <= x with p not dividing i, modulo q.
    pref = array('I', [0]) * q
    pref[0] = 1

    cur = 1
    for i in range(1, q):
        if i % p:
            cur = (cur * i) % q
        pref[i] = cur

    return pref

def factorial_unit(n, p, q, pref):
    # n! with every factor p removed, modulo q.
    block = pref[q - 1]
    res = 1

    while n:
        res = res * pow(block, n // q, q) % q
        res = res * pref[n % q] % q
        n //= p

    return res

def solve_case(M, N, D):
    factors = factorize(D)

    valuations = []
    K = 10**100

    for p, a in factors:
        e = (
            vp_factorial(M, p)
            - vp_factorial(N, p)
            - vp_factorial(M - N, p)
        )
        valuations.append(e)
        K = min(K, e // a)

    # CRT state: answer == x (mod mod)
    x = 0
    mod = 1

    for (p, a), e in zip(factors, valuations):
        q = p ** a

        pref = build_prefix(p, q)

        fm = factorial_unit(M, p, q, pref)
        fn = factorial_unit(N, p, q, pref)
        fr = factorial_unit(M - N, p, q, pref)

        unit = fm
        unit = unit * pow(fn, -1, q) % q
        unit = unit * pow(fr, -1, q) % q

        remaining_p = e - a * K

        residue = unit * pow(p, remaining_p, q) % q

        other = D // q
        residue = residue * pow(pow(other, K, q), -1, q) % q

        # Combine:
        # x + mod * t == residue (mod q)
        t = (residue - x) % q
        t = t * pow(mod, -1, q) % q

        x += mod * t
        mod *= q
        x %= mod

    return x

def solve():
    M, N, D = map(int, input().split())
    print(solve_case(M, N, D))

if __name__ == "__main__":
    solve()
```Thủ tục phân tích nhân tử sử dụng phép chia thử. Vì (D) tối đa là (1.6\cdot10^7), căn bậc hai nằm dưới (4000), nên chỉ cần thực hiện đơn giản là đủ.`vp_factorial`thực hiện công thức Legendre. Nó không bao giờ xây dựng một giai thừa và chỉ thực hiện phép chia lặp đi lặp lại cho số nguyên tố có liên quan.`build_prefix`lưu trữ tích của tất cả các số nguyên cho đến từng vị trí không chia hết cho (p). các`array('I')`sự đại diện là có chủ ý. Một danh sách Python chứa hàng triệu số nguyên Python sẽ tiêu tốn vài trăm megabyte, trong khi một mảng số nguyên không dấu lưu trữ mỗi mục trong bốn byte. Mỗi lần chỉ có một bảng lũy ​​thừa được giữ.`factorial_unit`thực hiện phép truy hồi từ hướng dẫn thuật toán. Đệ quy được viết lặp đi lặp lại để tránh chi phí đệ quy Python. Ở mỗi cấp độ, các khối có độ dài đầy đủ (q) đóng góp`pref[q - 1]`, trong khi khối không đầy đủ đóng góp`pref[n % q]`. Sau đó`n`được chia cho (p). 

Ba đơn vị giai thừa được kết hợp bằng cách sử dụng nghịch đảo mô-đun. Chúng được đảm bảo là nguyên tố cùng nhau với (q), không giống như các giai thừa ban đầu, đó chính là lý do tại sao việc phân tách (p)-adic lại cần thiết. 

Số mũ`remaining_p = e - a * K`không bao giờ là tiêu cực. Theo định nghĩa của (K), ta có (K\le e/a) với mọi thành phần lũy thừa nguyên tố. 

yếu tố`(D // q)^K`được đảo ngược modulo`q`. Nghịch đảo này luôn tồn tại vì (D/q) không chứa thừa số (p). Việc tính toán công suất trước khi lấy nghịch đảo của nó sẽ tránh được việc cố gắng chia cho một modulo không phải đơn vị (D). 

Cuối cùng, bản cập nhật CRT sử dụng phương trình 

[ 
x+\text{mod}\cdot t\equiv r\pmod q. 
]

Từ`mod`Và`q`là nguyên tố cùng nhau,`pow(mod, -1, q)`tồn tại. Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn ngay cả khi (M) đạt đến (4\cdot10^{18}). 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`9 2 3`, chúng tôi có 

[ 
C(9,2)=36=4\cdot3^2. 
] 

Chỉ có một thành phần lũy thừa nguyên tố (q=3). 

| Bước | (p) | (a) | (e=v_p(C)) | (K) | Còn lại (p)-quyền lực | Dư lượng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Nhân tố hóa | 3 | 1 | 2 | 2 | 0 | | 
| Sau khi xóa (3^2) | 3 | 1 | 2 | 2 | (2-2=0) | 1 | 

Phần (3)-tự do của (36) là (4) và (4\bmod3=1). Vì (36=4\cdot3^2) nên đáp án là`1`. Dấu vết cũng cho thấy tại sao modulo thông thường là không đủ: (36\bmod3=0), trong khi DivModulo trước tiên loại bỏ cả hai thừa số của (3). 

### Mẫu 2 

cho`5 2 5`, chúng tôi có 

[ 
C(5,2)=10=2\cdot5. 
] 

Ở đây (p=5), (a=1) và (e=1), do đó, chính xác một thừa số hoàn chỉnh của (5) bị loại bỏ. 

| Bước | (p) | (a) | (e=v_p(C)) | (K) | Còn lại (p)-quyền lực | Dư lượng cuối cùng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Nhân tố hóa | 5 | 1 | 1 | 1 | 0 | | 
| Xóa (5^1) | 5 | 1 | 1 | 1 | (1-1=0) | 2 | 

Phần không có (5) là (2), do đó kết quả DivModulo là`2`. Một trực tiếp`10 % 5`sẽ tạo ra số không không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(D+\omega(D)\log^2 M+\sqrt D)) | Các bảng tiền tố chứa tổng cộng nhiều nhất (D), trong khi đánh giá đơn vị giai thừa và lũy thừa mô đun có dạng logarit theo (M). | 
| Không gian | (O(\max_i p_i^{a_i})) | Mỗi lần chỉ có một bảng tiền tố lũy thừa nguyên tố được lưu trữ. | 

Tổng các thành phần lũy thừa nguyên tố riêng biệt của (D) nhiều nhất là (D), do đó tổng tiền xử lý tiền tố là (O(D)). Với (D\le1.6\cdot10^7), đây là quy mô dự kiến ​​của vấn đề. Giá trị khổng lồ của (M) chỉ xuất hiện bên trong các phép tính logarit, do đó giới hạn (4\cdot10^{18}) không buộc phải lặp lại chính giai thừa. 

các`array('I')`bộ lưu trữ giữ bảng tiền tố lớn nhất có thể khoảng hàng chục megabyte thay vì hàng trăm megabyte chi phí đối tượng Python, đặc biệt phù hợp với ranh giới (D=1.6\cdot10^7). 

## Trường hợp thử nghiệm```
# This test block assumes the solution above is saved as solution.py
# and exposes solve_case(M, N, D).

from solution import solve_case

# Provided samples
assert solve_case(9, 2, 3) == 1, "sample 1"
assert solve_case(5, 2, 5) == 2, "sample 2"
assert solve_case(6, 3, 6) == 2, "sample 3"
assert solve_case(7654321, 1234567, 1050) == 210, "sample 4"

# Minimum-size input: C(1, 0) = 1
assert solve_case(1, 0, 2) == 1, "minimum input"

# N = M: C(M, M) = 1 even for enormous M
assert solve_case(4_000_000_000_000_000_000,
                 4_000_000_000_000_000_000,
                 16_000_000) == 1, "maximum M and D"

# D does not divide the binomial coefficient, even though
# D has several prime factors.
# C(6, 2) = 15, and 6 does not divide 15.
assert solve_case(6, 2, 6) == 3, "composite D without complete factor"

# Multiple complete D factors must all be removed.
# C(10, 5) = 252 = 7 * 6^2, so the answer is 7 mod 6 = 1.
assert solve_case(10, 5, 6) == 1, "multiple D factors"

# Dividing by D is required before taking the remainder.
# C(5, 2) = 10 = 2 * 5.
assert solve_case(5, 2, 5) == 2, "remove one complete D factor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 2`|`1`| Tối thiểu (M), (N=0) và (0!=1) | 
|`4000000000000000000 4000000000000000000 16000000`|`1`| Tối đa (M), tối đa (D) và (C(M,M)=1) | 
|`6 2 6`|`3`| Tổ hợp (D) không tồn tại thừa số đầy đủ của (D) | 
|`10 5 6`|`1`| Phải loại bỏ nhiều hơn một thừa số đầy đủ của (D) | 
|`5 2 5`|`2`| DivModulo khác với modulo thông thường | 

## Vỏ cạnh 

cho`5 2 5`, thuật toán tìm thấy (v_5(5!)=1), (v_5(2!)=0) và (v_5(3!)=0), cho ra (e=1). Vì (D=5^1) nên ta nhận được (K=1). Số mũ còn lại là (e-K=0), nên chỉ còn lại phần nhị thức không có (5). Phần đó là (2), đưa ra đáp án đúng`2`. 

Vì`6 2 6`, phân tích nhân tử sẽ cho (D=2\cdot3). Nhị thức là (15), do đó giá trị của nó là (v_2(15)=0) và (v_3(15)=1). Số bản sao hoàn chỉnh của (6) là 

[ 
K=\min(0,1)=0. 
] 

Không có gì bị xóa và (15\bmod6=3). Điều này chứng tỏ tại sao việc định giá công suất cơ bản tối thiểu là cần thiết. Việc loại bỏ các thừa số nguyên tố một cách độc lập sẽ làm thay đổi phép toán đang được tính toán. 

Vì`6 3 6`, hệ số nhị thức là (20=2^2\cdot5). Giá trị (2) của nó là (2), trong khi giá trị (3) của nó là (0). Do đó 

[ 
K=\min(2,0)=0. 
] 

Thuật toán giữ nguyên (20) và xây dựng lại (20\bmod6=2). 

Vì`1 0 2`, cả (0!) và (1!) đều có phần không có (p) (1) và mọi định giá giai thừa đều bằng 0. Do đó (K=0), phần dư được tái tạo là (1) và câu trả lời là`1`. 

Vì`10 5 6`, hệ số nhị thức là (252=7\cdot6^2). Các giá trị ban đầu là (v_2(252)=2) và (v_3(252)=2), do đó (K=2). Sau khi loại bỏ (6^2), giá trị còn lại là (7), có modulo dư (6) là`1`. Thuật toán đạt được kết quả tương tự mà không cần xây dựng (252) và cơ chế tương tự tiếp tục hoạt động khi hệ số nhị thức có hàng nghìn hoặc triệu tỷ chữ số.
