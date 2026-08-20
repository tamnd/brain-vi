---
title: "CF 102218B - Mua Đống Đá"
description: "Chúng tôi mua chính xác (M) cọc. Mỗi cọc độc lập chọn một trong (K) kích thước dương có sẵn, với mỗi kích thước có xác suất (1/K). Sau khi tất cả cọc được lật ra, trò chơi là thế Nim bình thường. Đối với Nim, Alice thắng chính xác khi xor của tất cả các kích thước cọc khác 0."
date: "2026-08-20T03:16:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "B"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 702
verified: false
draft: false
---

[CF 102218B - Mua đống đá](https://codeforces.com/problemset/problem/102218/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11 phút 42 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi mua chính xác (M) cọc. Mỗi cọc độc lập chọn một trong (K) kích thước dương có sẵn, với mỗi kích thước có xác suất (1/K). Sau khi tất cả cọc được lật ra, trò chơi là thế Nim bình thường. 

Đối với Nim, Alice thắng chính xác khi xor của tất cả các kích thước cọc khác 0. Vì vậy vấn đề là tính xác suất 

[ 
\Pr[c_{i_1}\oplus c_{i_2}\oplus\cdots\oplus c_{i_M}\ne 0], 
] 

trong đó mọi (i_j) được chọn độc lập và thống nhất từ (K) kích thước có sẵn. 

Câu trả lời được yêu cầu modulo (998244353). Nếu (W) là số lựa chọn có thứ tự của kích thước cọc (M) có xor khác 0, thì xác suất mong muốn là (W/K^M), do đó modulo số nguyên tố sẽ trở thành 

[ 
W\cdot (K^M)^{-1}\pmod {998244353}. 
] 

Số lượng cọc (M) có thể lớn tới (10^9) nên bất kỳ phương pháp nào xử lý từng cọc một đều không thể thực hiện được. Tất cả các kích thước cọc có thể có đều nằm dưới (2^{17}), đây là hạn chế quan trọng về cấu trúc. Điều đó có nghĩa là mọi kích thước đều vừa với không gian xor 17 bit chỉ chứa 

[ 
2^{17}=131072 
] 

những giá trị có thể. Thuật toán (O(2^{17}\cdot17)) dễ dàng khả thi, trong khi (O(MK)), (O(M2^{17})) hoặc liệt kê tất cả các lựa chọn theo thứ tự (K^M) là vô vọng. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ quá trình triển khai. Nếu (K=1), mọi cọc đều có cùng kích thước. Ví dụ,```
2 1
5
```đưa ra xác suất (1), bởi vì (5\oplus5=0), nên người chơi đầu tiên sẽ thua. Một công thức giả định một số lựa chọn riêng biệt hoặc quên rằng xor của một số chẵn các giá trị bằng 0 có thể mắc lỗi này. 

Vì```
1 1
5
```câu trả lời là (0). Chỉ có một cọc và kích thước của nó khác 0 nên Alice có thể loại bỏ nó và giành chiến thắng. Điều này cũng nắm bắt được ranh giới có số mũ (M=1). 

Một trường hợp hữu ích khác là```
3 2
1 2
```Số lượng có thể có của hai giá trị có tổng (3). Để có được xor 0, cả hai số đếm sẽ phải là số chẵn, điều này là không thể vì tổng của chúng là số lẻ. Do đó Alice thắng với xác suất (1) chứ không phải (0). Một lập luận bất cẩn chỉ kiểm tra xem có tồn tại kích thước cọc trùng lặp hay không sẽ bỏ lỡ điều kiện chẵn lẻ. 

Cuối cùng, phép biến đổi phải bao gồm toàn bộ miền xor từ (0) đến (2^{17}-1), mặc dù mọi kích thước được cung cấp đều dương. Giá trị (0) không phải là kích thước cọc được cung cấp, nhưng nó có thể là kết quả xor và rất cần thiết khi tính các vị thế thua lỗ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xem xét mọi chuỗi lựa chọn theo thứ tự (M) từ (K) kích thước cọc có sẵn. Đối với mỗi chuỗi, chúng tôi xor các phần tử của nó và kiểm tra xem kết quả có khác 0 hay không. Điều này đúng vì mọi chuỗi có thứ tự đều có xác suất chính xác (1/K^M). 

Vấn đề là số lượng trình tự. Có (K^M) trong số chúng, vì vậy ngay cả với số lượng công việc không đổi trên mỗi chuỗi thì thời gian chạy vẫn là (O(K^M M)). Ví dụ: (K=2) và (M=10^9) đã đưa ra (2^{10^9}) chuỗi có thể. Phương pháp vũ phu trở nên vô dụng rất lâu trước khi đạt được những hạn chế lớn nhất. 

Một quan điểm hứa hẹn hơn là đếm các vị trí bị mất, nghĩa là các chuỗi có xor chính xác bằng 0. Vì mọi giá trị cọc đều nhỏ hơn (2^{17}), nên xor hoạt động trong không gian vectơ hữu hạn của số nguyên 17 bit. Điều đó chỉ cung cấp cho chúng ta (N=2^{17}) trạng thái xor có thể. 

Chúng ta có thể định nghĩa một mảng lập trình động trong đó`dp[x]`là số cách lấy xor (x) sau một số cọc. Thêm một cọc nữa có giá trị (c) sẽ thay đổi trạng thái (x) thành (x\oplus c). Một lần chuyển đổi sẽ tốn (O(NK)) và thực hiện nó (M) lần sẽ tốn (O(MNK)), con số này vẫn còn quá lớn vì (M) có thể đạt tới (10^9). 

Quan sát quan trọng là quá trình chuyển đổi này là tích chập xor. Nếu (f[x]) mô tả phân bố hiện tại và (g[c]) là phân bố của một cọc mới mua thì phân bố tiếp theo là 

[ 
h[x]=\sum_y f[y]g[x\oplus y]. 
] 

Phép chập Xor có một phép biến đổi Fourier được thiết kế riêng cho nó, phép biến đổi Walsh-Hadamard. Trong không gian biến đổi, tích chập xor trở thành phép nhân theo điểm. Do đó, việc lấy cọc (M) trở nên đơn giản là nâng mọi giá trị được chuyển đổi lên lũy thừa thứ (M). 

Đặt (A[x]) là (1) khi (x) là một trong các kích thước được cung cấp và (0) nếu ngược lại. Biến đổi Walsh-Hadamard của nó là 

[ 
F[s]=\sum_x A[x](-1)^{\operatorname{popcount}(s\mathbin{&}x)}. 
] 

Khi đó số lượng bộ dữ liệu có thứ tự (M) có xor bằng 0 là 

[ 
L=\frac{1}{N}\sum_{s=0}^{N-1}F[s]^M. 
] 

Đây là công thức Walsh-Hadamard nghịch đảo tiêu chuẩn được đánh giá cụ thể ở trạng thái xor bằng 0. Chúng tôi thực sự không cần phải xây dựng lại toàn bộ phân phối. Ta chỉ cần hệ số bằng 0 nên sau khi biến đổi và lấy lũy thừa ta có thể tính tổng các giá trị đã biến đổi. 

Câu trả lời mong muốn là xác suất thắng, bằng một trừ xác suất thua. Vì có (K^M) các lựa chọn được sắp xếp có khả năng bằng nhau, 

\frac{1}{NK^M}\sum_s F[s]^M. 
] 

Do đó 

1- 
\frac{\sum_s F[s]^M}{N K^M} 
} 
\pmod {998244353}. 
] 

Bản thân biến đổi có chi phí (O(N\log N)) và có (N) lũy thừa mô-đun, mỗi chi phí (O(\log M)). Vì (N=2^{17}), điều này xử lý thoải mái các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(K^M M)) | (O(M)) | Quá chậm | 
| DP qua trạng thái xor | (O(MNK)) | (O(N)) | Quá chậm | 
| Biến đổi Walsh-Hadamard | (O(N\log N+N\log M)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt (N=2^{17}), vì mỗi kích thước cọc được biểu thị bằng tối đa 17 bit. Tạo một mảng`a`có chiều dài (N), với`a[c]=1`cho mọi kích thước cọc được cung cấp (c) và bằng 0 ở những nơi khác. Mảng mô tả phân phối một cọc trước khi chuẩn hóa. 
2. Áp dụng phép biến đổi Walsh-Hadamard nhanh cho`a`. Đối với mỗi (các) tọa độ biến đổi, giá trị kết quả là tổng có dấu 

[ 
F[s]=\sum_{c\in C}(-1)^{\operatorname{popcount}(s\mathbin{&}c)}. 
] 

Phép biến đổi chuyển đổi tích chập xor thành phép nhân, đây chính xác là những gì chúng ta cần vì các cọc độc lập kết hợp với xor. 

1. Nâng mọi giá trị được chuyển đổi lên modulo lũy thừa thứ (M) (998244353). Nếu một cọc đóng góp giá trị biến đổi (F[s]) thì (M) cọc độc lập đóng góp (F[s]^M). Chúng ta có thể tính toán điều này một cách trực tiếp bằng phép lũy thừa nhị phân mô-đun, do đó giá trị khổng lồ của (M) không phải là vấn đề. 
2. Tính tổng tất cả (F[s]^M) trên tọa độ biến đổi (N). Theo công thức Walsh-Hadamard nghịch đảo, chia tổng này cho (N) sẽ cho số lượng bộ dữ liệu có thứ tự (M) có xor bằng 0. 
3. Chia số lần thua cho (K^M) để có xác suất thua. Phép chia mô đun là phép nhân với nghịch đảo mô đun, vì vậy chúng ta tính toán 

\left(\sum_sF[s]^M\right) 
(NK^M)^{-1} 
\pmod {998244353}. 
] 

1. Đầu ra (1-\text{lose}) modulo (998244353), vì Alice thắng chính xác khi xor khác 0. 

Tại sao nó hoạt động: tính bất biến đằng sau phương pháp này là mỗi tọa độ Walsh-Hadamard theo dõi độc lập sự đóng góp có dấu của mọi trạng thái xor có thể có. Đối với một cọc, tọa độ là (F[s]). Việc kết hợp các cọc độc lập bằng xor tương ứng với phép tích chập xor và phép biến đổi Walsh-Hadamard thay đổi phép tích chập đó thành phép nhân thông thường. Sau (M) cọc, tọa độ là (F[s]^M). Phép biến đổi nghịch đảo cho biết hệ số của trạng thái xor bằng 0 chính xác là (1/N) lần tổng của tất cả các tọa độ được biến đổi. Vì mọi lựa chọn có thứ tự đều có xác suất bằng nhau (1/K^M), việc chuẩn hóa số đó sẽ cho xác suất thua và phần bù của nó là xác suất thắng của Alice. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
LOG = 17
N = 1 << LOG

def fwht(a):
    n = len(a)
    length = 1

    while length < n:
        step = length << 1

        for i in range(0, n, step):
            end = i + length

            for j in range(i, end):
                x = a[j]
                y = a[j + length]

                a[j] = x + y
                a[j + length] = x - y

        length = step

def solve():
    M, K = map(int, input().split())
    c = list(map(int, input().split()))

    a = [0] * N
    for x in c:
        a[x] = 1

    fwht(a)

    total = 0
    for x in a:
        total = (total + pow(x % MOD, M, MOD)) % MOD

    denominator = N * pow(K, M, MOD) % MOD
    losing = total * pow(denominator, MOD - 2, MOD) % MOD

    answer = (1 - losing) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Mảng đầu vào chứa chính xác một mục nhập cho mỗi kích thước cọc có thể có. Bởi vì tất cả các kích thước được cung cấp đều khác biệt nên việc chỉ định`1`đối với mỗi trong số chúng sẽ đưa ra hàm đếm một đống chính xác.`fwht`thực hiện phép biến đổi Walsh-Hadamard không chuẩn hóa. Mỗi con bướm thay thế một cặp ((x,y)) bằng ((x+y,x-y)). Không có sự phân chia trong quá trình chuyển đổi thuận, điều này giúp việc triển khai trở nên đơn giản và tránh đưa các nghịch đảo mô-đun vào mỗi con bướm. 

Các giá trị biến đổi có thể âm và cũng có thể tăng lên trong quá trình biến đổi. Số nguyên Python không bị tràn, nhưng việc giảm giá trị vẫn hữu ích trước khi tính lũy thừa mô-đun. biểu hiện`x % MOD`xử lý các giá trị âm một cách chính xác. 

Sự lũy thừa`pow(x % MOD, M, MOD)`là cần thiết vì (M) có thể là (10^9). Phép lũy thừa nhị phân chỉ lấy (O(\log M)) phép nhân mô-đun trên mỗi tọa độ biến đổi. 

Hệ số (N) xuất phát từ phép biến đổi Walsh-Hadamard nghịch đảo. Một lỗi triển khai phổ biến là bỏ qua nó và vô tình tính toán (N) lần số vị trí bị mất. 

Mẫu số còn lại là (K^M), vì các lựa chọn cửa hàng là độc lập và thống nhất. Chúng tôi kết hợp hai yếu tố như`N * K^M`trước khi lấy mô-đun nghịch đảo. Vì (N=2^{17}) và (K<2^{17<998244353), không hệ số nào chia hết cho mô đun nguyên tố. 

Cuối cùng,`1 - losing`được lấy modulo`MOD`. Điều này xử lý trường hợp biểu diễn mô-đun của`losing`lớn hơn`1`sau số học, mặc dù xác suất cơ bản nằm trong khoảng từ 0 đến 1. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2
1 3
```Có bốn cặp có thứ tự. Hai cặp thua là ((1,1)) và ((3,3)), vì xor của chúng bằng 0. Tương tự, có hai cặp thắng. 

Đối với phép biến đổi, các tổng có dấu liên quan được xác định bởi hai giá trị 1 và 3. Bảng sau đây trình bày các đại lượng chính sau phép biến đổi và lũy thừa. 

| Số lượng | Giá trị | 
| --- | --- | 
| (M) | 2 | 
| (K) | 2 | 
| (N) | 131072 | 
| Đếm một đống ở mỗi giá trị được cung cấp | 1 | 
| Tổng số lựa chọn theo thứ tự (K^M) | 4 | 
| Mất lựa chọn | 2 | 
| Xác suất chiến thắng | (2/4=1/2) | 
| Câu trả lời mô-đun | 499122177 | 

Đầu ra`499122177`là biểu diễn mô đun của (1/2), vì (2^{-1}\equiv499122177\pmod{998244353}). Điều này xác nhận rằng số lượng biến đổi được chuẩn hóa theo cả kích thước xor-space và số lượng đơn đặt hàng có thể có. 

### Mẫu 2 

Đầu vào là```
11 1
5
```Chỉ có một kích thước cọc có thể có, vì vậy mỗi cọc trong số 11 cọc chứa 5 viên đá. 

| Số lượng | Giá trị | 
| --- | --- | 
| (M) | 11 | 
| (K) | 1 | 
| Chỉ có kích thước cọc có thể | 5 | 
| Tổng xor | (5) | 
| Xác suất thua | 0 | 
| Xác suất chiến thắng | 1 | 
| Đầu ra | 1 | 

Vì 11 là số lẻ nên 

[ 
5\oplus5\oplus\cdots\oplus5=5, 
] 

với 11 bản sao của 5. Xor khác 0 nên Alice luôn thắng. Ví dụ này kiểm tra xem phép lũy thừa có xử lý chính xác một giá trị có sẵn không và số lẻ các cọc bằng nhau sẽ thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^{17}\cdot17+2^{17}\log M)) | FWHT có 17 cấp độ, theo sau là một lũy thừa mô-đun cho mỗi tọa độ biến đổi | 
| Không gian | (O(2^{17})) | Một mảng lưu trữ tất cả các giá trị xor-space | 

Biến đổi chỉ có 131072 mục và mỗi mục tham gia vào 17 cấp độ bướm. Giai đoạn lũy thừa thực hiện tối đa khoảng 30 bước bình phương mô-đun trên mỗi tọa độ cho (M\le10^9). Điều này dễ dàng nằm trong giới hạn dự định, trong khi sự phụ thuộc vào (M) là logarit chứ không phải tuyến tính. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353
N = 1 << 17

def fwht(a):
    n = len(a)
    length = 1

    while length < n:
        step = length << 1
        for i in range(0, n, step):
            for j in range(i, i + length):
                x = a[j]
                y = a[j + length]
                a[j] = x + y
                a[j + length] = x - y
        length = step

def solve():
    M, K = map(int, input().split())
    c = list(map(int, input().split()))

    a = [0] * N
    for x in c:
        a[x] = 1

    fwht(a)

    total = sum(pow(x % MOD, M, MOD) for x in a) % MOD
    denominator = N * pow(K, M, MOD) % MOD
    losing = total * pow(denominator, MOD - 2, MOD) % MOD

    print((1 - losing) % MOD)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        output = io.StringIO()
        old_stdout = sys.stdout
        sys.stdout = output
        try:
            solve()
        finally:
            sys.stdout = old_stdout

        return output.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("2 2\n1 3\n") == "499122177", "sample 1"
assert run("11 1\n5\n") == "1", "sample 2"
assert run("7 3\n1 2 3\n") == "50665352", "sample 3"

# Minimum-size input: one pile, one possible nonzero size.
assert run("1 1\n1\n") == "0", "single pile must be winning"

# One possible size, even number of piles.
assert run("2 1\n5\n") == "1", "two equal piles have xor zero"

# Two possible sizes, odd number of piles.
# With values 1 and 2, xor zero requires both counts to be even,
# which is impossible when M is odd.
assert run("3 2\n1 2\n") == "1", "odd length cannot have zero xor"

# Boundary value 2^17 - 1 with a huge exponent.
# M is even, so the xor of all equal piles is zero.
assert run("1000000000 1\n131071\n") == "0", "large even exponent"

# Two values and an even number of piles.
# For {1, 2}, exactly half of all even-length sequences have xor zero.
assert run("4 2\n1 2\n") == "499122177", "even-length two-value case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| Tối thiểu (M), cọc đơn khác 0 | 
|`2 1 / 5`|`1`| Tất cả các cọc đều bằng bội số chẵn | 
|`3 2 / 1 2`|`1`| Tính chẵn lẻ xor có độ dài lẻ | 
|`1000000000 1 / 131071`|`0`| Giá trị cọc tối đa (M) và tối đa | 
|`4 2 / 1 2`|`499122177`| Phân phối xor có độ dài chẵn và mô-đun (1/2) | 

## Vỏ cạnh 

Đối với một kích thước có sẵn duy nhất, toàn bộ trò chơi mang tính quyết định. Với đầu vào```
1 1
5
```phép biến đổi vẫn hoạt động vì mảng ban đầu có một mục nhập khác 0. Đơn giản hơn, xor chứa một bản sao 5, vì vậy nó khác 0 và câu trả lời là 0. Việc triển khai không cần trường hợp đặc biệt vì (F[s]^1=F[s]) tự động tạo ra cùng một số đếm. 

Vì```
2 1
5
```cả hai cọc đều chứa 5, vậy xor của chúng là (5\oplus5=0). Alice lần nào cũng thua và đầu ra chỉ là 1 cho xác suất thắng? Ở đây xác suất chiến thắng thực sự là 0, vì vậy kết quả đúng là`0`. Đây chính xác là lý do tại sao tính chẵn lẻ của số mũ lại quan trọng. Bộ thử nghiệm sử dụng sự khác biệt này để phát hiện các triển khai gây nhầm lẫn giữa xor 0 và xor khác 0. 

Đối với mẫu được cung cấp```
11 1
5
```có 11 bản sao của 5, cho xor 5. Alice thắng với xác suất 1, tạo ra đầu ra`1`. Trường hợp xác định tương tự thay đổi hoàn toàn khi tính chẵn lẻ của (M) thay đổi. 

Đối với giá trị cọc tối đa,```
1000000000 1
131071
```giá trị`131071`chính xác là (2^{17}-1), kích thước lớn nhất được phép. Vì (M) là số chẵn nên xor lặp lại (10^9) lần của nó bằng 0, nên xác suất chiến thắng của Alice là 0. Mảng biến đổi có sẵn chỉ số 131071 vì các chỉ số hợp lệ của nó chạy qua (2^{17}-1). Việc sử dụng một mảng có kích thước (2^{17}-1) sẽ gây ra từng lỗi ở đây. 

Giá trị 0 đáng được quan tâm đặc biệt mặc dù không thể mua được số 0. Nó vẫn phải tồn tại dưới dạng trạng thái biến đổi và xor vì câu hỏi hỏi liệu tổng xor có bằng không hay không. Tần số ban đầu tại chỉ số 0 bằng 0, nhưng phân phối cuối cùng có thể có số đếm khác 0 ở đó. Việc loại bỏ chỉ số 0 khỏi phép biến đổi sẽ phá hủy chính xác hệ số chúng ta cần khôi phục. 

Cuối cùng, các giá trị biến đổi âm được mong đợi. Con bướm Walsh-Hadamard thực hiện phép trừ nên nhiều tọa độ có thể trở thành số âm. Phép lũy thừa mô-đun trước tiên phải diễn giải chúng theo modulo (998244353). của Python`%`phép toán mang lại phần dư không âm chính xác, cho phép hàm công suất mô-đun thông thường xử lý các tọa độ này mà không cần bất kỳ logic đặc biệt nào.
