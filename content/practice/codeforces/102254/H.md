---
title: "CF 102254H - Hy vọng cao"
description: "Đối với mỗi tin nhắn, chúng ta có một cơ số (n) và một mô đun (m), với (1 le n < m le 10^6). Chúng ta cần tìm một số mũ (x) mà việc nâng (n) lên số mũ đó để lại phần dư (1) sau khi chia cho (m). Nếu không tồn tại số mũ dương như vậy thì câu trả lời bắt buộc là (-1)."
date: "2026-08-17T21:13:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102254
codeforces_index: "H"
codeforces_contest_name: "IME++ Starters Try-outs 2019"
rating: 0
weight: 102254
solve_time_s: 141
verified: false
draft: false
---

[CF 102254H - Hy vọng cao](https://codeforces.com/problemset/problem/102254/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi tin nhắn, chúng ta được cung cấp một cơ số (n) và một mô đun (m), với (1 \le n < m \le 10^6). Chúng ta cần tìm một số mũ (x) mà việc nâng (n) lên số mũ đó để lại phần dư (1) sau khi chia cho (m). Nếu không tồn tại số mũ dương như vậy thì câu trả lời bắt buộc là (-1). 

Sự khác biệt chính về mặt toán học là liệu (n) có phải là modulo khả nghịch (m) hay không. Nếu (\gcd(n,m)>1), mọi lũy thừa dương của (n) vẫn chia hết cho thừa số nguyên tố nào đó chia sẻ với (m), do đó nó không bao giờ có thể đồng dạng với (1). Nếu (\gcd(n,m)=1), luôn tồn tại số mũ phù hợp, vì (n) thuộc nhóm nhân modulo (m). 

Có một sự mâu thuẫn nhỏ trong câu lệnh hiện đang hiển thị: nó ghi (0 \le x), trong khi mẫu thứ hai yêu cầu (-1) cho (2^x \pmod 4). Vì (2^0=1), điều kiện hiển thị bằng chữ sẽ làm cho (x=0) trở thành giải pháp cho mọi truy vấn. Các mẫu và bài toán rõ ràng có ý định dùng số mũ dương. Giải pháp dưới đây tuân theo cách giải thích dự định đó, đây cũng là cách giải thích duy nhất phù hợp với Mẫu 2. 

Giới hạn (m\le 10^6) là lý do có thể thực hiện được giải pháp tiền xử lý theo lý thuyết số. Có thể có (10^5) truy vấn, do đó, việc phân tích mọi mô đun bằng cách chia thử lên tới (\sqrt m), tiếp theo là các tìm kiếm có khả năng tốn kém cho số mũ, sẽ quá tốn kém trong Python. Về cơ bản, chúng ta cần thực hiện phân tích thành logarit và giữ số lượng số học mô-đun cho mỗi truy vấn ở mức nhỏ. Một lượt xử lý trước (O(m\log\log m)) theo sau là công việc logarit gần đúng cho mỗi truy vấn nằm trong phạm vi quy mô dự kiến. 

Một số trường hợp đặc biệt có thể đánh lừa việc triển khai trực tiếp. Ví dụ, với đầu vào`1 2`, câu trả lời đúng là`1`, bởi vì (2) không phải là môđun thích hợp ở đây: truy vấn là (n=1,m=2) và (1^1\equiv1\pmod2). Tìm kiếm bắt đầu từ số mũ (2) sẽ bỏ lỡ số mũ hợp lệ nhỏ nhất, mặc dù mọi số mũ hợp lệ vẫn được chấp nhận. 

Đối với đầu vào`2 4`, câu trả lời đúng là`-1`. Vì (\gcd(2,4)=2), mọi lũy thừa dương của (2) là số chẵn, trong khi một số đồng dạng với (1\pmod4) phải là số lẻ. Việc thực hiện bất cẩn áp dụng định lý Euler một cách mù quáng mà không kiểm tra tính nguyên tố cùng nhau trước tiên có thể cho rằng số mũ tồn tại một cách không chính xác. 

Đối với đầu vào`3 5`, câu trả lời là`4`. Ở đây (\varphi(5)=4) và (3^4\equiv1\pmod5), trong khi (3,3^2,3^3) không đồng dạng với (1). Đây cũng là mẫu và chứng minh rằng số mũ cần thiết không nhất thiết phải là (1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử từng số mũ dương. Bắt đầu với`value = n % m`, nhân liên tục với (n) modulo (m) và dừng khi giá trị trở thành (1). Điều này đúng vì khi phần dư chạy lần đầu tiên trở thành (1), số mũ của nó chính xác là nghiệm. 

Vấn đề là số lần lặp lại. Nếu một truy vấn có thứ tự nhân gần (10^6), thì việc tìm kiếm cần gần một triệu phép nhân mô-đun. Với (10^5) truy vấn, có thể đạt tới khoảng (10^{11}) phép nhân mô-đun. Điều đó vượt xa những gì giới hạn một giây có thể hỗ trợ. 

Quan sát mở ra lời giải nhanh hơn là khi (\gcd(n,m)=1), định lý Euler cho 

[ 
n^{\varphi(m)}\equiv1\pmod m. 
] 

Vì vậy, chúng ta đã có một ứng cử viên được đảm bảo, (\varphi(m)). Nhiệm vụ còn lại là loại bỏ các thừa số nguyên tố không cần thiết khỏi ứng viên này. 

Giả sử ứng cử viên hiện tại là (k) và số nguyên tố (p) chia hết cho (k). Nếu 

[ 
n^{k/p}\equiv1\pmod m, 
] 

thì (k/p) cũng là một số mũ hợp lệ, do đó thừa số (p) là không cần thiết và có thể được loại bỏ. Chúng tôi tiếp tục làm điều này với mọi thừa số nguyên tố của (\varphi(m)). Giá trị kết quả chính xác là cấp bậc nhân của (n) modulo (m). 

Vì mọi thứ tự hợp lệ đều chia hết (\varphi(m)) nên đáp án cuối cùng nhiều nhất là (\varphi(m)\le m\le10^6), nên nó cũng thỏa mãn giới hạn số mũ. 

Để thực hiện phân tích nhanh hệ số trên (10^5) truy vấn, chúng tôi tính toán trước hệ số nguyên tố nhỏ nhất hoặc SPF cho mọi số nguyên lên đến (10^6). Mỗi mô đun sau đó có thể được tính thành phân số (O(\log m)). Tổng số Euler của nó có thể được lấy trực tiếp từ phép nhân tử đó và bản thân (\varphi(m)) có thể được tính thành nhân tử bằng cách sử dụng cùng một mảng SPF. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qm)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(M\log\log M + q\log M\log M)) | (O(M)) | Đã chấp nhận | 

Đây (M=10^6). Hệ số logarit thứ hai tính lũy thừa mô-đun được thực hiện trong khi loại bỏ các hệ số nguyên tố khỏi (\varphi(m)). 

## Hướng dẫn thuật toán 

1. Tính trước thừa số nguyên tố nhỏ nhất`spf[v]`với mọi (2\le v\le10^6). Mảng SPF cho phép chúng tôi tính hệ số bất kỳ mô đun truy vấn nào và bất kỳ tổng số nào theo thời gian logarit. 
2. Với mỗi truy vấn ((n,m)), hãy tính (\gcd(n,m)). Nếu nó lớn hơn (1), xuất ra`-1`. lũy thừa dương của (n) vẫn chia hết cho mọi ước số nguyên tố chung của (n) và (m), vì vậy nó không thể bằng (1) modulo (m). 
3. Hệ số (m) sử dụng`spf`. Nếu 

[ 
m=p_1^{a_1}p_2^{a_2}\cdots p_k^{a_k}, 
] 

tính toán 

m\prod_{i=1}^{k}\left(1-\frac1{p_i}\right). 
] 

Chỉ có các thừa số nguyên tố riêng biệt được sử dụng trong công thức này. 

1. Đặt`order = phi(m)`. Định lý Euler đảm bảo rằng (n^{order}\equiv1\pmod m), bởi vì phép kiểm tra gcd đã chứng minh rằng (n) và (m) là nguyên tố cùng nhau. 
2. Yếu tố`order`thành các thừa số nguyên tố riêng biệt của nó. Với mỗi số nguyên tố (p) như vậy, hãy liên tục kiểm tra xem 

[ 
n^{order/p}\equiv1\pmod m. 
] 

Nếu thử nghiệm thành công, hãy chia`order`bằng (p) và kiểm tra lại. Nếu thất bại thì giữ nguyên yếu tố đó. 

1. Xuất kết quả`order`. Đây là dư lượng tạo ra số mũ dương nhỏ nhất (1), vì vậy đây là câu trả lời hợp lệ và tối đa là tự động (10^6). 

### Tại sao nó hoạt động 

Giữ nguyên bất biến đó`order`luôn là số mũ dương thỏa mãn (n^{order}\equiv1\pmod m). Ban đầu điều này đúng theo định lý Euler. Khi loại bỏ thừa số nguyên tố (p), chúng tôi chỉ thực hiện việc này sau khi đã xác minh (n^{order/p}\equiv1\pmod m), do đó, bất biến vẫn đúng. Khi thử nghiệm thất bại, việc loại bỏ (p) khác sẽ phá hủy thuộc tính. 

Cuối cùng, không có thừa số nguyên tố nào`order`có thể được loại bỏ trong khi vẫn giữ được sự đồng dạng. Vì bậc nhân của (n) modulo (m) chia mọi số mũ tạo ra (1) và đặc biệt là chia giá trị ban đầu (\varphi(m)), nên việc loại bỏ liên tục mọi thừa số nguyên tố di động sẽ để lại chính xác số mũ dương nhỏ nhất đó. 

Nếu (\gcd(n,m)>1), không có nghiệm dương nào tồn tại, do đó phương án sớm`-1`kết quả cũng đúng 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_M = 10**6

def build_spf(limit):
    spf = list(range(limit + 1))
    if limit >= 1:
        spf[1] = 1

    for i in range(2, int(limit ** 0.5) + 1):
        if spf[i] == i:
            start = i * i
            for j in range(start, limit + 1, i):
                if spf[j] == j:
                    spf[j] = i
    return spf

spf = build_spf(MAX_M)

def factor_distinct(x):
    """Return the distinct prime factors of x."""
    factors = []
    while x > 1:
        p = spf[x]
        factors.append(p)
        while x % p == 0:
            x //= p
    return factors

def phi_from_factorization(m):
    phi = m
    x = m

    while x > 1:
        p = spf[x]
        phi -= phi // p
        while x % p == 0:
            x //= p

    return phi

def multiplicative_order(n, m):
    if m == 1:
        return 1

    if __import__("math").gcd(n, m) != 1:
        return -1

    order = phi_from_factorization(m)

    for p in factor_distinct(order):
        while order % p == 0:
            candidate = order // p
            if pow(n, candidate, m) == 1:
                order = candidate
            else:
                break

    return order

def solve():
    q = int(input())
    out = []

    for _ in range(q):
        n, m = map(int, input().split())
        out.append(str(multiplicative_order(n, m)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc xây dựng SPF được thực hiện một lần trước khi xử lý các truy vấn. Với mọi hợp số,`spf[x]`lưu trữ một trong các ước số nguyên tố của nó và chia liên tục cho ước số đó sẽ cho ra hệ số hoàn chỉnh mà không cần chia thử.`phi_from_factorization`bắt đầu bằng (\varphi(m)=m). Đối với mọi số nguyên tố riêng biệt (p\mid m), nó áp dụng phép biến đổi`phi -= phi // p`, chính xác là dạng số nguyên của phép nhân với (1-1/p). Vòng lặp bên trong sẽ loại bỏ tất cả các bản sao của (p), do đó mỗi số nguyên tố riêng biệt sẽ được xử lý một lần. 

Kiểm tra gcd xuất hiện trước định lý Euler. Định lý Euler yêu cầu tính nguyên tố cùng nhau và việc bỏ qua bước kiểm tra này là sai sót nghiêm trọng nhất về tính đúng đắn trong lời giải này. Vì`n = 2, m = 4`, chẳng hạn, không có số mũ dương nào phù hợp. 

Việc giảm đơn hàng sử dụng tính năng tích hợp sẵn của Python`pow(n, exponent, m)`. Điều này tính toán lũy thừa mô-đun một cách trực tiếp mà không cần xây dựng số nguyên khổng lồ (n^{số mũ}). Các số nguyên có độ chính xác tùy ý của Python cũng có nghĩa là không có vấn đề tràn, nhưng ba đối số`pow`vẫn rất cần thiết vì phép lũy thừa thông thường sẽ chậm hơn rất nhiều và sử dụng các giá trị trung gian rất lớn. 

các`while`vòng quanh mỗi thừa số nguyên tố là cần thiết. Một số nguyên tố có thể xuất hiện nhiều lần trong (\varphi(m)) và nhiều bản sao có thể được gỡ bỏ. Ví dụ: nếu ứng cử viên hiện tại chứa (p^3), một phép chia thành công không chứng minh được rằng (p^2) bản sao còn lại là cần thiết. 

Việc thực hiện trở lại`1`khi (n=1), vì (1^1\equiv1\pmod m). Bài toán dự định yêu cầu số mũ dương, vì vậy đây là câu trả lời hợp lệ nhỏ nhất. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, truy vấn là (n=3,m=5). 

| n | m | gcd | phi(m) | Đơn hàng hiện tại | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 5 | 1 | 4 | 4 | (3^{4/2}\bmod5=3^2\bmod5=4) | Giữ yếu tố 2 | 
| 3 | 5 | 1 | 4 | 4 | Không còn yếu tố khác biệt | Đầu ra 4 | 

Ứng cử viên ban đầu là (\varphi(5)=4). Thừa số nguyên tố duy nhất của nó là (2), nhưng số mũ (2) không tạo ra số dư (1). Do đó không thể loại bỏ thừa số và đáp án vẫn là (4). Thật vậy, (3^4=81\equiv1\pmod5). 

Đối với Mẫu 2, truy vấn là (n=2,m=4). 

| n | m | gcd | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 2 | 4 | 2 | gcd lớn hơn 1 nên không tồn tại thứ tự dương | -1 | 

Thuật toán dừng trước khi tính toán thứ tự dựa trên tổng thể. Mọi lũy thừa dương của (2) đều là số chẵn, nên không có số nào có thể đồng dạng với (1\pmod4). Đây chính xác là trường hợp không nguyên tố cùng nhau mà định lý Euler không thể giải quyết được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M\log\log M + q\log M\log M)) | Chi phí tiền xử lý SPF (O(M\log\log M)); mỗi truy vấn phân tích các số theo (O(\log M)) và thực hiện logarit nhiều phép lũy thừa mô-đun | 
| Không gian | (O(M)) | Mảng SPF chứa (10^6+1) số nguyên | 

Với (M=10^6), quá trình xử lý trước chỉ tốn một lần, trong khi (q\le10^5) giúp quản lý công việc trên mỗi truy vấn. Thuật toán tránh khả năng nhân theo mô-đun (10^{11}) của lực lượng vũ phu và chỉ sử dụng một số lượng nhỏ lệnh gọi lũy thừa mô-đun cho mỗi truy vấn. Việc sử dụng bộ nhớ cũng ở mức an toàn dưới 256 MB cho mảng SPF và đầu ra truy vấn. 

## Trường hợp thử nghiệm```python
# helper: run the core solution on input string, return output string
import io
import math

def run(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)
    q = int(next(it))
    ans = []

    for _ in range(q):
        n = int(next(it))
        m = int(next(it))
        ans.append(str(multiplicative_order(n, m)))

    return "\n".join(ans)

# provided sample 1
assert run("""\
1
3 5
""") == "4", "sample 1"

# provided sample 2
assert run("""\
1
2 4
""") == "-1", "sample 2"

# Minimum-size modulus, n = 1
assert run("""\
1
1 2
""") == "1", "minimum-size valid query"

# Boundary case with n = m - 1. Since n == -1 (mod m), order is 2.
assert run("""\
1
999999 1000000
""") == "2", "maximum modulus boundary"

# Repeated identical queries, including a non-coprime case.
assert run("""\
4
5 8
5 8
6 9
6 9
""") == "2\n2\n-1\n-1", "repeated queries"

# Several small orders, including order 1 and order 2.
assert run("""\
4
1 7
2 7
3 7
6 7
""") == "1\n3\n6\n2", "small multiplicative orders"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 2`|`1`| Giá trị hợp lệ tối thiểu và trường hợp đơn hàng 1 | 
|`1 / 999999 1000000`|`2`| Mô đun cực đại và ranh giới (n\equiv-1\pmod m) | 
|`4 / 5 8 / 5 8 / 6 9 / 6 9`|`2 / 2 / -1 / -1`| Truy vấn lặp đi lặp lại và từ chối không đồng thời | 
|`4 / 1 7 / 2 7 / 3 7 / 6 7`|`1 / 3 / 6 / 2`| Thứ tự nhân chính xác khác nhau và giảm hệ số | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là mô đun nhỏ nhất,`1 2`. gcd là (1) và (\varphi(2)=1). Thứ tự ứng cử viên đã là (1) nên không còn gì để giảm. Đầu ra của thuật toán`1`và thực sự là (1^1\equiv1\pmod2). 

Trường hợp cạnh thứ hai là truy vấn không nguyên tố cùng nhau`2 4`. gcd là (2) nên thuật toán trả về ngay`-1`. Không cần lũy thừa mô-đun. Điều này ngăn cản việc áp dụng sai định lý Euler cho một giá trị không khả nghịch modulo (4). 

Trường hợp cạnh thứ ba là`999999 1000000`. Vì (999999\equiv-1\pmod{1000000}), số mũ (2) là đủ. Tổng của (1000000) là (400000), do đó thuật toán bắt đầu bằng (400000) và liên tục loại bỏ các thừa số nguyên tố bất cứ khi nào số mũ nhỏ hơn tương ứng vẫn tạo ra (1). Cuối cùng nó đạt tới`2`. Sự đồng đẳng cuối cùng là ((-1)^2\equiv1\pmod{1000000}). 

Trường hợp cạnh thứ tư là`1 7`. Ở đây mọi lũy thừa dương của (1) đều là (1), nên bậc nhân chính xác là`1`. Thuật toán bắt đầu bằng (\varphi(7)=6), kiểm tra các thừa số nguyên tố của (6) và liên tục giảm ứng viên cho đến khi đạt`1`. Kết quả cuối cùng là hợp lệ và cho thấy lý do tại sao vòng rút gọn phải cho phép một ứng viên rơi xuống mức một. 

Trường hợp cạnh thứ năm liên quan đến giới hạn dưới được hiển thị của câu lệnh`0 <= x`. Nếu từ ngữ đó được hiểu theo nghĩa đen thì mọi câu hỏi đều có câu trả lời`0`, bởi vì (n^0=1) với mọi số dương (n). Điều đó sẽ làm cho Mẫu 2 không chính xác. Thuật toán cố tình tuân theo cách giải thích số mũ dương được ngụ ý trong mẫu và theo công thức cấp nhân. Sự khác biệt này phải được kiểm tra trước khi đệ trình chống lại bất kỳ phiên bản nào của thẩm phán mà tuyên bố có thể đã được sửa đổi.
