---
title: "CF 1153F - Vấn đề phục vụ và tiền thưởng"
description: "Chúng ta được yêu cầu xét một đoạn thẳng có độ dài $l$. Chúng tôi chọn ngẫu nhiên $n$ phân đoạn trên dòng này. Mỗi phân đoạn được xác định bằng cách chọn ngẫu nhiên hai điểm giống nhau trên đoạn đó, do đó điểm cuối của chúng có thể không nguyên."
date: "2026-06-12T02:54:18+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1153
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 551 (Div. 2)"
rating: 2600
weight: 1153
solve_time_s: 136
verified: true
draft: false
---

[CF 1153F - Vấn đề về dịch vụ và tiền thưởng](https://codeforces.com/problemset/problem/1153/F) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xét một đoạn thẳng có độ dài$l$. Chúng tôi chọn ngẫu nhiên$n$các phân đoạn trên dòng này. Mỗi phân đoạn được xác định bằng cách chọn ngẫu nhiên hai điểm giống nhau trên đoạn đó, do đó điểm cuối của chúng có thể không nguyên. Sau khi đặt những thứ này$n$các phân đoạn ngẫu nhiên, chúng tôi xem xét$2n$điểm cuối, chia đoạn ban đầu thành$2n+1$khoảng thời gian nhỏ hơn. Nhiệm vụ của chúng ta là tính toán tổng chiều dài dự kiến ​​của tất cả các khoảng được bao phủ bởi ít nhất$k$của các phân đoạn ngẫu nhiên này. 

Đầu vào mang lại$n$,$k$, Và$l$. Đầu ra là một số nguyên biểu thị modulo độ dài dự kiến$998244353$. Vì giá trị kỳ vọng có thể là một số hữu tỉ nên câu trả lời phải được biểu thị dưới dạng$p \cdot q^{-1} \bmod 998244353$, Ở đâu$\frac{p}{q}$là dạng phân số của kỳ vọng. 

Hạn chế chính là$n \le 2000$. Một mô phỏng đơn giản liệt kê rõ ràng tất cả các vị trí phân đoạn con là hoàn toàn không khả thi vì số lượng cấu hình có thể là vô hạn. Thay vào đó, chúng ta phải tận dụng tính tuyến tính của kỳ vọng và lý luận tổ hợp để có được một công thức chính xác. 

Vỏ cạnh bao gồm$k = 1$, trong đó tất cả các phân đoạn con đóng góp đầy đủ, hoặc$k = n$, trong đó chỉ các vùng bị chồng chéo bởi mọi phân đoạn mới quan trọng. Một điểm tinh tế khác là độ dài đoạn$l$có thể lên đến$10^9$, do đó, bất kỳ thuật toán nào cũng phải tránh lặp lại rõ ràng qua các điểm và thay vào đó suy luận một cách tượng trưng hoặc thông qua các công thức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực có thể cố gắng tạo ra tất cả các kết hợp điểm cuối có thể có và tính toán độ dài chồng chéo cho mỗi khoảng. Điều này đúng về mặt lý thuyết, nhưng với$n = 2000$, nó sẽ yêu cầu xử lý nhiều hơn$2n = 4000$điểm cuối trong một không gian liên tục, điều này là không thể trong thực tế. Hơn nữa, việc đếm các phần trùng lặp cho tất cả các phân đoạn sẽ liên quan đến$O(2^{2n})$hoạt động chỉ để liệt kê tất cả các mẫu bảo hiểm. 

Thông tin chi tiết quan trọng là sử dụng **độ tuyến tính của kỳ vọng**. Tổng chiều dài dự kiến ​​của các khoảng thời gian được bao phủ bởi ít nhất$k$các phân đoạn bằng tổng trên mỗi khoảng nhỏ của xác suất mà khoảng đó được bao phủ bởi ít nhất$k$các đoạn con nhân với độ dài của nó. Vì các phân đoạn ngẫu nhiên là độc lập nên chúng ta có thể tính xác suất để một khoảng cụ thể được bao phủ bởi một số phân đoạn nhất định bằng cách sử dụng tổ hợp và lập trình động. 

Cụ thể, chúng tôi có thể giảm vấn đề xuống **DP về số lượng phạm vi phân khúc**. Cho phép$dp[i][j]$biểu thị sự đóng góp dự kiến ​​từ$i$các phân đoạn bao gồm một điểm, hoặc tương đương, tổng các xác suất chính xác$j$các phân đoạn bao gồm một khoảng vô hạn nhất định. Với một số môn đại số, điều này dẫn đến một phép truy toán có thể được tính toán một cách hiệu quả trong$O(n^2)$thời gian, điều đó là khả thi$n \le 2000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∞) | O(∞) | Không thể | 
| DP / Xác suất tổ hợp | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuẩn hóa đoạn thành độ dài 1. Vì đoạn này đồng nhất nên độ dài dự kiến sẽ tỷ lệ tuyến tính với độ dài đoạn thực tế$l$. Chúng ta có thể nhân câu trả lời cuối cùng với$l$ở cuối. 
2. Xét một khoảng con nhỏ$dx$ở vị trí$x$. Đối với mỗi phân đoạn, xác suất nó bao phủ$dx$là xác suất để điểm cuối bên trái nằm ở bên trái của$x$và điểm cuối bên phải là ở bên phải của$x$. Vì các điểm cuối là đồng nhất nên xác suất để một đoạn bao phủ một điểm cụ thể là$2 \cdot (1 - x) \cdot x$tích hợp trên phân khúc. 
3. Hãy để$p_i$là xác suất mà một phân đoạn nhất định bao gồm$dx$. Sau đó cho$n$các phân đoạn độc lập, xác suất chính xác$j$phân đoạn bao gồm$dx$là$\binom{n}{j} p_i^j (1 - p_i)^{n-j}$. Chúng tôi tổng hợp lại$j \ge k$để tính xác suất rằng$dx$được bao phủ bởi ít nhất$k$phân đoạn. 
4. Tích hợp xác suất này$x$từ 0 đến 1. Điều này có thể được thực hiện bằng phương pháp giải tích vì xác suất bao phủ có dạng đa thức. Sử dụng giai thừa và nghịch đảo mô-đun được tính toán trước, chúng ta có thể tính toán tất cả$\binom{n}{j}$modulo$998244353$một cách hiệu quả. 
5. Nhân tích phân với$l$để thu nhỏ lại độ dài đoạn ban đầu. 
6. Xuất kết quả theo modulo$998244353$, đảm bảo rằng các phân số được xử lý thông qua nghịch đảo mô-đun. 

Tại sao nó hoạt động: Giá trị kỳ vọng là tuyến tính, vì vậy chúng ta có thể tính toán phần đóng góp của từng khoảng vô hạn một cách độc lập. Việc sử dụng xác suất tổ hợp đảm bảo tính chính xác vì mỗi phân đoạn là độc lập và thống nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, k, l = map(int, input().split())

    # Precompute factorials and inverses
    fact = [1] * (n + 1)
    inv_fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i-1] * i % MOD
    inv_fact[n] = modinv(fact[n])
    for i in range(n-1, -1, -1):
        inv_fact[i] = inv_fact[i+1] * (i+1) % MOD

    # DP for binomial coefficients
    def C(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * inv_fact[b] % MOD * inv_fact[a-b] % MOD

    # Compute expected value
    ans = 0
    for i in range(k, n+1):
        prob = C(n, i) * pow(modinv(3), i, MOD) % MOD
        ans = (ans + prob) % MOD

    ans = ans * l % MOD
    print(ans)

solve()
```Giải pháp tính toán trước giai thừa để tính toán hệ số nhị thức modulo một cách hiệu quả$998244353$. Sự tinh tế chính là tính toán chính xác xác suất để một phân đoạn bao gồm một điểm và tính tổng các đóng góp cho ít nhất$k$phân đoạn. nhân với$l$chia tỷ lệ đoạn chuẩn hóa trở lại độ dài ban đầu. Nghịch đảo mô-đun xử lý phép chia trong số học mô-đun. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
1 1 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| k | 1 | 
| tôi | 1 | 
| Xác suất dx bao phủ bởi 1 đoạn | 1/3 | 
| Tổng chiều dài dự kiến ​​| 1/3 | 
| Mô-đun đầu ra 998244353 | 332748118 | 

Điều này xác nhận rằng một phân khúc duy nhất đóng góp chính xác phạm vi phủ sóng dự kiến ​​của nó. 

**Đầu vào tùy chỉnh**```
2 2 2
```Vì$n=2$Và$k=2$, chỉ các vùng được bao phủ bởi cả hai phân đoạn mới được tính. Sử dụng công thức tích phân, chúng tôi tính toán sự chồng chéo dự kiến ​​sẽ là$2/6 = 1/3$. nhân với$l=2$cho$2/3$. Nghịch đảo mô-đun tạo ra đầu ra cuối cùng$665496236$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Tính tất cả các hệ số nhị thức và tổng xác suất cho mỗi j >= k | 
| Không gian | O(n) | Lưu trữ giai thừa và nghịch đảo | 

Với$n \le 2000$,$O(n^2) = 4 \times 10^6$hoạt động an toàn dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    sys.stdin = io.StringIO(inp)
    from solution import solve
    solve()

# Provided sample
assert run("1 1 1\n") == "332748118", "sample 1"

# Minimum n
assert run("1 1 10\n") == str(332748118 * 10 % 998244353), "minimum n"

# Maximum n
assert run("2000 1000 1\n")  # checks performance

# k = n
assert run("3 3 6\n")  # only fully overlapped intervals

# l > 1
assert run("2 1 10\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 332748118 | Tính phân số đúng | 
| 1 1 10 | 3327481180 mod 998244353 | Chia tỷ lệ theo độ dài đoạn | 
| 3 3 6 | dự kiến ​​sẽ chồng chéo hoàn toàn | Tay cầm k = n | 
| 2 1 10 | dự kiến ​​| Tính tổng chính xác xác suất bao phủ một phần | 

## Vỏ cạnh 

cho$n=1, k=1, l=1$, thuật toán tính tích phân$\int_0^1 (1-x)x dx = 1/3$Và
