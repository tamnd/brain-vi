---
title: "CF 1257G - Bộ chia số"
description: "Đầu vào không cung cấp số trực tiếp. Thay vào đó, nó đưa ra hệ số nguyên tố dưới dạng danh sách các thừa số nguyên tố, trong đó các số nguyên tố bằng nhau có thể xuất hiện nhiều lần. Giả sử một số nguyên tố $q$ xuất hiện $c$ lần. Sau đó, mỗi ước số sẽ chọn một số mũ trong khoảng từ $0$ đến $c$ cho số nguyên tố đó."
date: "2026-06-11T20:50:29+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "fft", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1257
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 76 (Rated for Div. 2)"
rating: 2600
weight: 1257
solve_time_s: 119
verified: true
draft: false
---

[CF 1257G - Bộ chia số](https://codeforces.com/problemset/problem/1257/G) 

**Đánh giá:** 2600 
**Tags:** chia để trị, fft, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào không cung cấp số trực tiếp. Thay vào đó, nó đưa ra hệ số nguyên tố dưới dạng danh sách các thừa số nguyên tố, trong đó các số nguyên tố bằng nhau có thể xuất hiện nhiều lần. 

Giả sử một số nguyên tố$q$xuất hiện$c$lần. Khi đó mọi ước số đều chọn một số mũ nằm giữa$0$Và$c$cho số nguyên tố đó. Nếu các số nguyên tố riêng biệt có bội số$$c_1,c_2,\dots,c_k,$$thì mọi ước số đều tương ứng với một vectơ$$(e_1,e_2,\dots,e_k), \qquad 0 \le e_i \le c_i.$$Mối quan hệ chia hết trở nên rất đơn giản:$$(e_1,\dots,e_k)\mid(f_1,\dots,f_k)$$nếu và chỉ khi$$e_i \le f_i$$cho mọi tọa độ. 

Chúng ta phải tìm tập hợp các ước số lớn nhất có thể sao cho không có hai ước số phân biệt nào có thể so sánh được khi chia hết. Theo thuật ngữ của poset, chúng tôi đang tìm kiếm kích thước của một antichain tối đa. 

Số lượng yếu tố đầu vào tối đa là$2\cdot10^5$. Việc liệt kê các ước số là không thể. Nếu tất cả các thừa số đều khác nhau thì số ước số là$2^n$, có giá trị lớn về mặt thiên văn. Bất kỳ giải pháp nào cũng phải hoạt động trực tiếp với bội số của các số nguyên tố bằng nhau. 

Một điểm tinh tế là câu trả lời không phải là số ước. Ví dụ:```
n = 3
primes = [2,3,5]
```Các ước số tạo thành mạng Boolean có chiều thứ ba. Antichain lớn nhất có kích thước$3$, không$8$. 

Một sai lầm dễ mắc phải khác là cho rằng tất cả các ước có cùng số thừa số nguyên tố luôn tối ưu mà không cần chứng minh. Coi như:```
x = 2^3
```Các ước số là$1,2,4,8$. Mỗi cặp đều có thể so sánh được, vì vậy bộ tốt nhất có kích thước$1$. Cấu trúc của mạng chia có vấn đề. 

Bẫy thứ ba là cố gắng đếm các ước số ở một thứ hạng nhất định bằng cách sử dụng các công thức giai thừa. Các bội số có thể lớn như$2\cdot10^5$và số mũ được giới hạn độc lập cho mỗi số nguyên tố. Đối tượng đếm chính xác là hàm sinh chứ không phải hệ số đa thức. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực rất hữu ích vì nó phơi bày cấu trúc tổ hợp cơ bản. 

Mỗi ước số tương ứng với một vectơ số mũ. Một bộ tốt là một antichain trong tư thế sản phẩm của chuỗi$$[0,c_1]\times[0,c_2]\times\cdots\times[0,c_k].$$Một giải pháp mạnh mẽ sẽ tạo ra tất cả các ước số, xây dựng mối quan hệ chia hết và tìm kiếm antichain lớn nhất. Điều này là vô vọng vì số lượng ước số có thể là số mũ trong$n$. 

Quan sát quan trọng là tư thế này được xếp hạng bởi$$r(e_1,\dots,e_k)=e_1+\cdots+e_k.$$Hai yếu tố riêng biệt có cùng cấp bậc không thể so sánh được. Nếu như$$e_i\le f_i$$với mọi tọa độ và tổng thứ hạng bằng nhau thì tất cả tọa độ phải bằng nhau. 

Vì vậy, mọi thứ hạng đều tự động là một antichain. 

Bây giờ một định lý sâu sắc đã đi vào bức tranh. Sản phẩm của chuỗi có tính chất đối xứng bậc và không đồng hình thức bậc, và chúng thỏa mãn tính chất Sperner. Đối với các poset như vậy, kích thước của antichain tối đa bằng kích thước của cấp bậc lớn nhất. 

Tổng thứ hạng dao động từ$0$ĐẾN$$c_1+\cdots+c_k=n.$$Bởi vì kích thước thứ hạng là đối xứng và không đồng nhất, nên thứ hạng lớn nhất là thứ hạng ở giữa:$$\left\lfloor \frac n2 \right\rfloor .$$Do đó, toàn bộ vấn đề quy về việc đếm các vectơ số mũ thỏa mãn$$0\le e_i\le c_i, \qquad e_1+\cdots+e_k=\left\lfloor\frac n2\right\rfloor.$$Đây chính xác là hệ số của$$\prod_{i=1}^{k}(1+x+x^2+\cdots+x^{c_i})$$ở mức độ$\lfloor n/2\rfloor$. 

Nhiệm vụ còn lại là nhân đa thức. 

Mỗi thừa số là một đa thức có dạng$$1+x+\cdots+x^{c_i}.$$Tổng của tất cả các độ là$$c_1+\cdots+c_k=n\le 2\cdot10^5.$$Nhân tất cả các đa thức với chiến lược chia để trị và NTT đưa ra nghiệm được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số ước | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n\log^2 n)$|$O(n\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi số nguyên tố xuất hiện bao nhiêu lần trong phép phân tích nhân tử. 

Nếu một số nguyên tố xuất hiện$c$lần, nó đóng góp đa thức$$1+x+\cdots+x^c.$$2. Tạo một đa thức cho mỗi bội số. 

Các hệ số đều là số 1 vì mọi số mũ từ$0$bởi vì$c$được phép đúng một lần. 
3. Nhân liên tục hai đa thức nhỏ nhất. 

Điều này tương tự với mẫu hợp nhất tối ưu. Nó giữ kích thước đa thức trung gian nhỏ và giảm tổng công việc NTT. 
4. Sử dụng phép biến đổi lý thuyết số theo mô đun$998244353$cho mỗi phép nhân. 

Mô-đun này được chọn đặc biệt vì nó hỗ trợ NTT hiệu quả. 
5. Sau khi hợp nhất tất cả các đa thức, thu được hàm sinh cuối cùng$$F(x)=\prod_i(1+x+\cdots+x^{c_i}).$$6. Xuất ra hệ số bậc$$\left\lfloor \frac n2 \right\rfloor.$$Theo tính chất Sperner của tích các chuỗi, hệ số này bằng kích thước lớn nhất của một tập con tốt. 

### Tại sao nó hoạt động 

Mạng chia là sản phẩm của chuỗi. Hàm xếp hạng của nó là tổng số mũ. Mỗi thứ hạng tạo thành một antichain vì các phần tử có thể so sánh được có thứ hạng bằng nhau phải giống hệt nhau. 

Sản phẩm của chuỗi là Sperner poset, do đó antichain lớn nhất chính xác là thứ hạng lớn nhất. Kích thước xếp hạng được tính bằng hàm tạo$$\prod_i(1+x+\cdots+x^{c_i}),$$và tính đối xứng cộng với tính đơn điệu đặt thứ hạng tối đa ở mức độ$\lfloor n/2\rfloor$. Thuật toán tính toán chính xác hệ số đó, vì vậy giá trị trả về là kích thước của tập hợp con tốt nhất. 

## Giải pháp Python```python
import sys
from collections import Counter
from heapq import heapify, heappop, heappush

input = sys.stdin.readline

MOD = 998244353
G = 3

def ntt(a, invert):
    n = len(a)

    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        wlen = pow(G, (MOD - 1) // length, MOD)
        if invert:
            wlen = pow(wlen, MOD - 2, MOD)

        for i in range(0, n, length):
            w = 1
            half = length // 2
            for j in range(i, i + half):
                u = a[j]
                v = a[j + half] * w % MOD

                a[j] = (u + v) % MOD
                a[j + half] = (u - v) % MOD

                w = w * wlen % MOD

        length <<= 1

    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def multiply(a, b):
    need = len(a) + len(b) - 1
    n = 1
    while n < need:
        n <<= 1

    fa = a[:] + [0] * (n - len(a))
    fb = b[:] + [0] * (n - len(b))

    ntt(fa, False)
    ntt(fb, False)

    for i in range(n):
        fa[i] = fa[i] * fb[i] % MOD

    ntt(fa, True)
    return fa[:need]

def solve():
    n = int(input())
    primes = list(map(int, input().split()))

    freq = Counter(primes)

    heap = []
    uid = 0

    for c in freq.values():
        poly = [1] * (c + 1)
        heap.append((len(poly), uid, poly))
        uid += 1

    heapify(heap)

    while len(heap) > 1:
        _, _, a = heappop(heap)
        _, _, b = heappop(heap)

        c = multiply(a, b)

        heappush(heap, (len(c), uid, c))
        uid += 1

    result = heap[0][2]
    print(result[n // 2] % MOD)

if __name__ == "__main__":
    solve()
```Bước đếm tần số chuyển hệ số nguyên tố thành bội số$c_i$. Mỗi bội số ngay lập tức trở thành một đa thức$1+x+\dots+x^{c_i}$. 

Hàng đợi ưu tiên không bắt buộc phải có để đảm bảo tính chính xác nhưng nó cải thiện đáng kể hiệu suất. Trước tiên, nhân các đa thức nhỏ sẽ giữ cho tổng lượng công việc NTT gần như tối ưu. 

Việc triển khai NTT thực hiện phép biến đổi lặp tiêu chuẩn theo mô đun$998244353$. Sau khi chuyển đổi cả hai toán hạng, các hệ số được nhân theo điểm và chuyển đổi ngược lại. 

Đa thức cuối cùng có bậc chính xác$n$. Bằng cấp$d$đếm các vectơ số mũ có tổng tọa độ bằng$d$. Vì antichain tối đa nằm ở cấp bậc$\lfloor n/2\rfloor$, hệ số đó chính là đáp án. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2999999 43 2999957
```Tất cả các số nguyên tố đều khác biệt. 

Đa thức: 

| Thủ tướng | Đếm | 
| --- | --- | 
| 2999999 | 1 | 
| 43 | 1 | 
| 2999957 | 1 | 

Đa thức: 

| Bước | Đa thức | 
| --- | --- | 
| Ban đầu | (1+x), (1+x), (1+x) | 
| Sau khi hợp nhất | (1+2x+x2) | 
| Cuối cùng | 1+3x+3x²+x³ | 

Mức độ mục tiêu là:$$\lfloor 3/2\rfloor=1.$$| Bằng cấp | Hệ số | 
| --- | --- | 
| 0 | 1 | 
| 1 | 3 | 
| 2 | 3 | 
| 3 | 1 | 

Trả lời:```
3
```Đây chính xác là lớp giữa của mạng Boolean. 

### Mẫu 2 

đầu vào:```
6
2 3 2 3 2 2
```Đây$$x=2^4\cdot3^2.$$Đa thức: 

| Thủ tướng | Đếm | 
| --- | --- | 
| 2 | 4 | 
| 3 | 2 | 

Đa thức: 

| Bước | Đa thức | 
| --- | --- | 
| Ban đầu | (1+x+x²+x³+x⁴), (1+x+x²) | 
| Cuối cùng | 1+2x+3x²+3x³+3x⁴+2x⁵+x⁶ | 

Mức độ mục tiêu:$$6/2=3.$$| Bằng cấp | Hệ số | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 3 | 
| 4 | 3 | 
| 5 | 2 | 
| 6 | 1 | 

Trả lời:```
3
```Dấu vết cho thấy tính đối xứng bậc làm cơ sở cho lập luận Sperner. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log^2 n)$| Phép nhân NTT trên tổng bậc$n$sử dụng sự hợp nhất chia để trị | 
| Không gian |$O(n\log n)$| Lưu trữ đa thức và bộ đệm NTT | 

Vì tổng mức độ không bao giờ vượt quá$2\cdot10^5$, cách tiếp cận dựa trên NTT dễ dàng phù hợp với giới hạn 5 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import Counter

def run(inp: str) -> str:
    MOD = 998244353

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))

    freq = Counter(p)

    dp = [1]
    for c in freq.values():
        ndp = [0] * (len(dp) + c)
        for i, v in enumerate(dp):
            for e in range(c + 1):
                ndp[i + e] += v
        dp = ndp

    return str(dp[n // 2] % MOD) + "\n"

# sample 1
assert run("3\n2999999 43 2999957\n") == "3\n"

# sample 2
assert run("6\n2 3 2 3 2 2\n") == "3\n"

# minimum size
assert run("1\n2\n") == "1\n"

# all equal
assert run("5\n2 2 2 2 2\n") == "1\n"

# two distinct primes
assert run("2\n2 3\n") == "2\n"

# multiplicities (2,2)
assert run("4\n2 2 3 3\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`1`| Đầu vào nhỏ nhất có thể | 
|`2 2 2 2 2`|`1`| Chuỗi đơn, mỗi cặp có thể so sánh được | 
|`2 3`|`2`| Mạng Boolean có chiều hai | 
|`2 2 3 3`|`3`| Sản phẩm của hai chuỗi không cần thiết | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
```Vectơ bội số là$(1)$. Hàm tạo là$$1+x.$$Mức độ mục tiêu là$0$. Hệ số là$1$, phù hợp với thực tế là một chuỗi có kích thước phản chuỗi tối đa$1$. 

Bây giờ hãy xem xét:```
5
2 2 2 2 2
```Vectơ bội số là$(5)$. Hàm tạo là$$1+x+x^2+x^3+x^4+x^5.$$Mức độ mục tiêu là$2$. hệ số của nó là$1$. Mỗi số chia nằm trên một chuỗi$$1<2<4<8<16<32,$$vì vậy không có antichain nào có thể chứa nhiều hơn một phần tử. 

Cuối cùng hãy xem xét:```
4
2 2 3 3
```Sự đa dạng là$(2,2)$. Hàm tạo là$$(1+x+x^2)^2 = 1+2x+3x^2+2x^3+x^4.$$Hệ số ở giữa là$3$. Ba vectơ số mũ của thứ hạng$2$là$$(0,2),\ (1,1),\ (2,0),$$và chúng tạo thành một antichain tối đa. Thuật toán tính toán chính xác hệ số này.
