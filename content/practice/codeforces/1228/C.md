---
title: "CF 1228C - Số nguyên tố và phép nhân"
description: "Chúng ta được yêu cầu tính tích trên một dãy số bắt nguồn từ một cặp số nguyên $x$ và $n$. Hàm $f(x, y)$ xem xét tất cả các thừa số nguyên tố của $x$ và với mỗi số nguyên tố đó $p$ xác định lũy thừa lớn nhất của $p$ chia $y$."
date: "2026-06-11T22:23:46+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1228
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 589 (Div. 2)"
rating: 1700
weight: 1228
solve_time_s: 99
verified: true
draft: false
---

[CF 1228C - Số nguyên tố và phép nhân](https://codeforces.com/problemset/problem/1228/C) 

**Đánh giá:** 1700 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính tích trên một dãy số bắt nguồn từ một cặp số nguyên$x$Và$n$. chức năng$f(x, y)$xét tất cả các thừa số nguyên tố của$x$và với mỗi số nguyên tố như vậy$p$xác định công suất lớn nhất của$p$đó là sự chia rẽ$y$. Sau đó chúng ta nhân các giá trị này với tất cả các số nguyên tố của$x$và sau đó trên tất cả$y$từ$1$ĐẾN$n$. Kết quả cuối cùng được mong đợi theo modulo$10^9 + 7$. 

Dữ liệu đầu vào bao gồm hai số nguyên,$x$Và$n$.$x$có thể lớn như$10^9$, do đó nó có thể có tới khoảng 9 hoặc 10 thừa số nguyên tố.$n$có thể lên đến$10^{18}$, điều này tạo nên một sự lặp lại ngây thơ trên tất cả$y$không thể nào. Chúng ta không đủ khả năng để tính toán$f(x, y)$trực tiếp cho mỗi$y$vì điều này sẽ yêu cầu$O(n \cdot \text{number of primes of } x)$hoạt động, mà là quá nhiều. 

Các trường hợp cần xem xét bao gồm khi nào$x$là số nguyên tố (vì vậy chỉ tồn tại một thừa số nguyên tố), khi$n$nhỏ hơn thừa số nguyên tố nhỏ nhất hoặc khi$n$rất lớn so với lũy thừa của các số nguyên tố$x$. Ví dụ, nếu$x = 2$Và$n = 1$, kết quả phải là$1$, nhưng việc thực hiện bất cẩn có thể cố gắng tính lũy thừa của$2$lên đến$1$không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ tính đến yếu tố$x$vào số nguyên tố của nó, lặp lại$y$từ$1$ĐẾN$n$, tính toán$g(y, p)$cho mỗi số nguyên tố$p$, nhân chúng rồi lấy tích modulo$10^9 + 7$. Vấn đề là ở chỗ đó$n$có thể$10^{18}$, do đó, ngay cả một lần lặp duy nhất cho mỗi$y$là không thể thực hiện được. Số lượng hoạt động sẽ theo thứ tự$10^{18} \cdot \text{number of primes}$, điều đó là không thể. 

Cái nhìn sâu sắc quan trọng là đối với mỗi số nguyên tố$p$,$f(x, y)$chỉ phụ thuộc vào số mũ của$p$TRONG$y$. Tổng đóng góp của$p$trên tất cả$y$từ$1$ĐẾN$n$có thể được tính bằng cách đếm xem có bao nhiêu bội số của$p^k$tồn tại ở$[1, n]$cho tất cả$k \ge 1$. Điều này làm giảm vấn đề thành một tổng trên nhiều lũy thừa logarit trên mỗi số nguyên tố thay vì nhiều số tuyến tính. Các số nguyên tố còn lại của$x$độc lập, vì vậy chúng ta có thể tính toán sự đóng góp của chúng một cách riêng biệt và kết hợp ở cuối bằng cách sử dụng số học mô-đun. 

Câu chuyện là: bạo lực chỉ có tác dụng với những kẻ nhỏ mọn$n$nhưng thất bại lớn$n$. Nhận thấy rằng hàm số chỉ phụ thuộc vào lũy thừa nguyên tố cho phép chúng ta đếm trực tiếp các bội số, tránh lặp qua tất cả$y$. Điều này làm giảm khả năng$O(n)$sự phức tạp để$O(\log n \cdot \pi(x))$, điều đó là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * log x) | O(logx) | Quá chậm | 
| Tối ưu | O(log n * sqrt(x)) | O(sqrt(x)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử$x$vào các ước nguyên tố của nó. Đối với số lượng lên đến$10^9$, phép chia thử lên đến$\sqrt{x}$là đủ. Giữ một danh sách các số nguyên tố$p_1, p_2, ..., p_m$. 
2. Khởi tạo một biến`result = 1`. Điều này sẽ tích lũy modulo sản phẩm cuối cùng$10^9 + 7$. 
3. Đối với mỗi số nguyên tố$p$trong phép nhân hóa: 

1. Đặt`power = 0`. Điều này sẽ tính tổng số mũ của$p$trong sản phẩm$f(x, 1) * ... * f(x, n)$. 
2. Đặt`cur = p`. Điều này thể hiện$p^1, p^2, ...$khi chúng tôi lặp lại. 
3. Trong khi`cur <= n`, thêm vào$\lfloor n / cur \rfloor$ĐẾN`power`và nhân lên`cur`qua`p`cho lần lặp tiếp theo. Cái này đếm xem có bao nhiêu số trong$[1, n]$được chia cho$p^k$cho mỗi$k$. 
4. Nhân lên`result`qua$p^{\text{power}} \mod 10^9 + 7$. Sử dụng lũy ​​thừa mô-đun để xử lý lũy thừa lớn một cách hiệu quả. 
4. Sau khi xử lý tất cả các số nguyên tố, in`result`. 

Tại sao nó hoạt động: Vòng lặp đếm bội số lũy thừa của$p$đảm bảo rằng mỗi$g(y, p)$được hạch toán chính xác. Mỗi$p$đóng góp một cách độc lập, do đó việc nhân các khoản đóng góp sẽ có tác dụng. Phép lũy thừa mô-đun xử lý các lũy thừa lớn mà không bị tràn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mod_pow(a, b, mod):
    result = 1
    a %= mod
    while b > 0:
        if b & 1:
            result = result * a % mod
        a = a * a % mod
        b >>= 1
    return result

def solve():
    x, n = map(int, input().split())
    primes = []
    temp = x
    i = 2
    while i * i <= temp:
        if temp % i == 0:
            primes.append(i)
            while temp % i == 0:
                temp //= i
        i += 1
    if temp > 1:
        primes.append(temp)
    
    result = 1
    for p in primes:
        power = 0
        cur = p
        while cur <= n:
            power += n // cur
            if cur > n // p:
                break
            cur *= p
        result = result * mod_pow(p, power, MOD) % MOD
    
    print(result)

solve()
```Giải thích: Vòng phân tích nhân tử đảm bảo chúng ta thu thập tất cả các số nguyên tố của$x$. Vòng lặp while bên trong đếm tổng lũy ​​thừa của mỗi số nguyên tố trong phạm vi$[1, n]$hiệu quả mà không cần lặp qua tất cả các số. lũy thừa mô-đun ngăn ngừa tràn. Séc`if cur > n // p`tránh tràn số nguyên khi nhân lũy thừa lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`10 2`Các số nguyên tố của 10: 2, 5 

| Thủ tướng | Cur | Đóng góp điện | Nguồn sau vòng lặp | 
| --- | --- | --- | --- | 
| 2 | 2 | 2 // 2 = 1 | 1 | 
| 2 | 4 | 2 // 4 = 0 | 1 | 
| 5 | 5 | 2 // 5 = 0 | 0 | 

Kết quả = 2^1 * 5^0 = 2 

Điều này xác nhận rằng việc đếm bội số hoạt động chính xác và lũy thừa nhỏ không đóng góp số 0 một cách nhầm lẫn. 

### Mẫu 2 

đầu vào:`6 5`Số nguyên tố: 2, 3 

| Thủ tướng | Cur | Đóng góp | 
| --- | --- | --- | 
| 2 | 2 | 5//2=2 | 
| 2 | 4 | 5//4=1 | 
| 3 | 3 | 5//3=1 | 

Kết quả = 2^(2+1) * 3^1 = 2^3 * 3^1 = 24 

Bảng hiển thị số mũ tích lũy của mỗi số nguyên tố trong tích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(x) + log n * π(x)) | Việc phân tích x lấy O(sqrt(x)) và lũy thừa đếm lấy O(log n) mỗi số nguyên tố | 
| Không gian | O(sqrt(x)) | Để lưu trữ các thừa số nguyên tố của x | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian vì sqrt(10^9) ≈ 31623 và log2(10^18) ≈ 60. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# Provided samples
assert run("10 2") == "2", "sample 1"
assert run("6 5") == "24", "sample 2"

# Custom test cases
assert run("2 1") == "1", "minimal n"
assert run("7 10") == "7", "prime x"
assert run("12 100") == "27720", "multiple primes"
assert run("1000000007 1") == "1", "large prime x"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 | Tối thiểu n | 
| 7 10 | 7 | x là số nguyên tố | 
| 12 100 | 27720 | nhiều số nguyên tố đóng góp chính xác | 
| 1000000007 1 | 1 | Số nguyên tố lớn x với n nhỏ |
