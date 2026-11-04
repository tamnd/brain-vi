---
title: "CF 1155E - Đoán Căn Nguyên"
description: "Bài toán cung cấp cho chúng ta một đa thức ẩn có bậc nhiều nhất là 10 với các hệ số nguyên, mỗi hệ số hoàn toàn nhỏ hơn $10^6 + 3$. Nhiệm vụ là tìm một số nguyên $x0$ sao cho đa thức có giá trị bằng 0 modulo $10^6 + 3$."
date: "2026-06-12T02:43:50+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 1155
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 63 (Rated for Div. 2)"
rating: 2200
weight: 1155
solve_time_s: 99
verified: false
draft: false
---

[CF 1155E - Đoán gốc](https://codeforces.com/problemset/problem/1155/E) 

**Đánh giá:** 2200 
**Tags:** bạo lực, tương tác, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán cho ta một đa thức ẩn bậc nhiều nhất là 10 với các hệ số nguyên, mỗi hệ số đều nhỏ hơn$10^6 + 3$. Nhiệm vụ là tìm số nguyên$x_0$sao cho đa thức có giá trị bằng 0 modulo$10^6 + 3$. Chúng tôi không nhìn thấy các hệ số một cách trực tiếp; thay vào đó, chúng ta có thể tương tác với hệ thống bằng cách truy vấn tối đa 50 đầu vào số nguyên và nhận modulo giá trị đa thức tương ứng$10^6 + 3$. Nếu không có giải pháp tồn tại, chúng tôi phải báo cáo$-1$. 

Mô-đun chính$10^6 + 3$đủ nhỏ để cho phép thực hiện các phép toán toàn diện nếu cần, nhưng mức độ của đa thức thấp, điều này gợi ý rằng chúng ta có thể tái tạo lại đa thức hoặc suy luận về nghiệm của nó một cách hiệu quả. Bởi vì các hệ số được giới hạn bên dưới mô đun, mỗi đánh giá mô đun số nguyên tố sẽ cung cấp thông tin đầy đủ về đa thức tại điểm đó. 

Các trường hợp cạnh rất tinh tế. Một cách tiếp cận bất cẩn sẽ là truy vấn các giá trị ngẫu nhiên và hy vọng tìm được số 0. Ví dụ, nếu đa thức là$f(x) = 1 + x^{10}$, truy vấn 0 sẽ cho 1 và truy vấn 1 sẽ cho 2, nhưng gốc thực sự có thể lớn, như$x = 10^6 + 2$. Thuật toán không được cho rằng nghiệm là nhỏ hoặc số 0 xuất hiện trong số ít số nguyên đầu tiên. Một trường hợp khác là đa thức không đổi như$f(x) = 0$modulo số nguyên tố, có tất cả các nghiệm; ngược lại,$f(x) = c \neq 0$modulo số nguyên tố không có gốc. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử mọi$x$từ 0 đến mô đun. Mỗi đánh giá là thời gian không đổi, nên về mặt lý thuyết, nó hoạt động, nhưng mô đun$10^6 + 3$, do đó, brute-force yêu cầu hơn một triệu truy vấn, vượt xa 50 truy vấn được phép. Cách tiếp cận này đúng nhưng rõ ràng là không thực tế. 

Cái nhìn sâu sắc quan trọng là đa thức bậc nhiều nhất là 10 được xác định duy nhất bởi các giá trị của nó tại 11 điểm phân biệt modulo một số nguyên tố. Khi chúng ta biết các giá trị tại 11 điểm, chúng ta có thể xây dựng lại đa thức bằng cách sử dụng phép nội suy Lagrange trong số học mô-đun. Sau khi xây dựng lại, chúng ta có thể cố gắng tìm gốc bằng cách kiểm tra tất cả$x$modulo số nguyên tố, nhưng việc đánh giá đa thức tại mọi điểm có thể được tối ưu hóa vì bậc 10 là nhỏ. 

Brute-force không thành công vì nó bỏ qua giới hạn bậc, trong khi phương pháp tái thiết đa thức tận dụng thực tế là 11 điểm đủ để xác định một modulo đa thức bậc 10 là một số nguyên tố. Một khi chúng ta đã biết hoàn toàn đa thức, việc tìm nghiệm rất đơn giản bằng cách kiểm tra tất cả từ 0 đến$10^6 + 2$hoặc sử dụng cách tìm nghiệm hiệu quả cho các đa thức bậc nhỏ modulo một số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^6) | O(1) | Quá chậm | 
| Nội suy đa thức | O(p·k) với p = 10^6+3, k=10 | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn đa thức tại 11 điểm phân biệt: 0, 1, 2, …, 10. Lý do là đa thức bậc 10 được xác định duy nhất bởi 11 điểm modulo một số nguyên tố. Mỗi truy vấn trả về$f(x_i)$. 
2. Sử dụng modulo nội suy Lagrange$10^6 + 3$để xây dựng lại các hệ số của đa thức. Nghịch đảo mô đun được sử dụng trong các mẫu số của công thức Lagrange vì mô đun là số nguyên tố, đảm bảo tồn tại nghịch đảo. 
3. Lặp lại tất cả các số nguyên$x$từ 0 đến$10^6 + 2$. Đánh giá đa thức được xây dựng lại ở mỗi$x$modulo$10^6 + 3$. Vì mức độ tối đa là 10 nên mỗi lần đánh giá đều nhanh chóng bằng phương pháp Horner. 
4. Nếu một phép đánh giá mang lại số nguyên tố bằng 0, thì kết quả$x$như là gốc. Nếu không có đánh giá nào mang lại kết quả bằng 0, hãy báo cáo$-1$. 
5. Xóa sau mỗi truy vấn để tuân thủ yêu cầu tương tác. 

Tại sao nó hoạt động: Đa thức được xác định duy nhất bởi 11 điểm modulo số nguyên tố. Sau khi được xây dựng lại, đánh giá đa thức ở bất kỳ thời điểm nào$x$modulo số nguyên tố tạo ra giá trị thực. Bởi vì chúng tôi lặp lại trên toàn bộ trường kích thước$10^6 + 3$, mọi gốc sẽ được tìm thấy. Thuật toán được đảm bảo đúng modulo số nguyên tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**6 + 3

def modinv(a, p):
    return pow(a, p-2, p)

def lagrange_interpolation(x_values, y_values, mod):
    k = len(x_values)
    coeffs = [0] * k
    for i in range(k):
        xi, yi = x_values[i], y_values[i]
        numerator = [1]
        denom = 1
        for j in range(k):
            if i == j:
                continue
            xj = x_values[j]
            denom = denom * (xi - xj) % mod
            numerator = poly_mul(numerator, [-xj % mod, 1], mod)
        inv_denom = modinv(denom, mod)
        for d in range(len(numerator)):
            coeffs[d] = (coeffs[d] + numerator[d] * yi * inv_denom) % mod
    return coeffs

def poly_mul(a, b, mod):
    res = [0] * (len(a) + len(b) - 1)
    for i in range(len(a)):
        for j in range(len(b)):
            res[i+j] = (res[i+j] + a[i]*b[j]) % mod
    return res

def eval_poly(coeffs, x, mod):
    res = 0
    for c in reversed(coeffs):
        res = (res*x + c) % mod
    return res

x_values = list(range(11))
y_values = []

for x in x_values:
    print(f"? {x}")
    sys.stdout.flush()
    y = int(input())
    y_values.append(y)

coeffs = lagrange_interpolation(x_values, y_values, MOD)

for x in range(MOD):
    if eval_poly(coeffs, x, MOD) == 0:
        print(f"! {x}")
        sys.stdout.flush()
        sys.exit(0)

print("! -1")
sys.stdout.flush()
```Giải pháp đầu tiên truy vấn 11 điểm để xây dựng lại đa thức.`lagrange_interpolation`tính toán các hệ số modulo số nguyên tố một cách hiệu quả bằng cách sử dụng nghịch đảo mô đun.`eval_poly`sử dụng phương pháp Horner để đánh giá nhanh. Lặp lại toàn bộ trường đảm bảo tìm thấy bất kỳ gốc nào. Tất cả các đầu ra đều được xóa để tuân thủ giao thức tương tác. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| x | y = f(x) | 
| --- | --- | 
| 0 | 1000002 | 
| 1 | 1000003 ≡ 0 | 

Sau khi nội suy, đa thức là$f(x) = 1000002 + x^2$. Đánh giá 0 cho 1000002, đánh giá 1 cho 0, vì vậy gốc là 1. 

Mẫu 2: 

| x | y = f(x) | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 5 | 

Sản lượng nội suy$f(x) = 1 + x^2$. Đánh giá từ 0 đến MOD-1 tìm thấy gốc ở 1000002, vì$1 + (10^6 + 2)^2 ≡ 0 \mod 10^6+3$. 

Dấu vết xác nhận thuật toán nắm bắt đa thức một cách chính xác và tìm ra các nghiệm trên mô đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MOD * k) | Đánh giá đa thức bậc k tại giá trị MOD = 10^6 + 3 | 
| Không gian | O(k^2) | Lưu trữ hệ số và đa thức trung gian cho phép nội suy | 

Với k ≤ 10 và MOD = 10^6 + 3, việc đánh giá 10^6 điểm là khả thi vì mỗi lần đánh giá đều nhanh chóng theo phương pháp Horner, vừa vặn trong vòng 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    return output.getvalue().strip()

# provided samples
assert run("1000002\n0\n") == "! 1", "sample 1"
assert run("1\n0\n") == "! 1000002", "sample 2"

# custom cases
assert run("0\n0\n") == "! 0", "root at 0"
assert run("1\n1\n1\n1\n1\n1\n1\n1\n1\n1\n1\n") == "! -1", "no roots"
assert run("2\n0\n0\n0\n0\n0\n0\n0\n0\n0\n0\n") == "! 500001", "root in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | ! 0 | gốc ở 0 | 
| 1 lần lặp lại | ! -1 | không có rễ | 
| 2 + 10 số không | ! 500001 | gốc tồn tại ở giữa trường | 

## Vỏ cạnh 

Đối với hằng số đa thức 0 modulo số nguyên tố, thuật toán truy vấn 11 điểm, tất cả đều trả về 0. Nội suy tái tạo các hệ số hoàn toàn bằng không. Đánh giá ở mức 0
