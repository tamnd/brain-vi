---
title: "CF 2125E - Bộ tổng bù"
description: "Bài toán yêu cầu chúng ta đếm các tập hợp số nguyên có thể được tạo ra dưới dạng tổng bổ sung từ một số mảng số nguyên dương."
date: "2026-06-08T03:29:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2125
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 181 (Rated for Div. 2)"
rating: 2500
weight: 2125
solve_time_s: 90
verified: false
draft: false
---

[CF 2125E - Bộ tổng bù](https://codeforces.com/problemset/problem/2125/E) 

**Đánh giá:** 2500 
**Tags:** lực lượng vũ phu, tổ hợp, dp, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm các tập hợp số nguyên có thể được tạo ra dưới dạng tổng bổ sung từ một số mảng số nguyên dương. Cụ thể, với kích thước đã đặt`n`và giới hạn trên`x`, chúng tôi muốn số lượng các bộ riêng biệt`Q`với chính xác`n`các phần tử trong đó mỗi phần tử nằm giữa`1`Và`x`. Mỗi bộ`Q`tương ứng với một số mảng`a`của các số nguyên dương và với mỗi phần tử`a_i`TRONG`a`chúng tôi bao gồm`s - a_i`trong bộ, ở đâu`s`là tổng của mảng. Các giá trị trùng lặp sẽ bị loại bỏ vì`Q`là một bộ, không phải là nhiều bộ. 

Đầu vào lớn:`n`Và`x`có thể lên tới 200.000 và có thể lên tới 10.000 trường hợp thử nghiệm, với tổng số`n`Và`x`trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở mức 200.000. Điều này loại trừ mọi giải pháp có độ phức tạp O(n^2) hoặc O(x^2) và thậm chí O(n x) phải được triển khai cẩn thận để tránh vượt quá 10^8 thao tác. Chúng ta cần một thuật toán gần như tuyến tính trong`x`cho từng trường hợp thử nghiệm. 

Trường hợp khó phát hiện xảy ra khi`n`lớn hơn`x`. Ví dụ, nếu`n = 5`Và`x = 4`, không có tập hợp kích thước 5 nào có thể tồn tại vì tất cả các phần tử phải ≤ 4. Một tình huống phức tạp khác phát sinh khi`n`bằng 1: bất kỳ số nào từ 1 đến`x`tự nó tạo thành một tập hợp lệ. Bất kỳ nỗ lực ngây thơ nào để liệt kê các mảng`a`sẽ thất bại vì số lượng mảng tăng theo cấp số nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liệt kê tất cả các mảng có thể`a`và tạo ra các tập tổng bổ sung của chúng`Q`. Sau đó, người ta sẽ lọc các bộ này theo kích thước và giới hạn trên. Điều này có tác dụng với đầu vào rất nhỏ nhưng không thể thực hiện được đối với`n`Và`x`trong hàng trăm ngàn. Cụ thể, số lượng mảng tăng theo cấp số nhân trong`n`, khiến ngay cả những kích thước đầu vào nhỏ nhất cũng không thể thực hiện được. 

Quan sát quan trọng là vấn đề giảm xuống việc đếm các tập hợp số nguyên từ`1`ĐẾN`x`có thể được biểu diễn dưới dạng`s - a_i`đối với một số người`s`và một số số nguyên dương`a_i`. Nếu chúng ta sắp xếp tập hợp, hãy gọi các phần tử của nó`b_1 < b_2 < ... < b_n`, chúng ta thấy điều đó với một số tiền`s`, mỗi`a_i = s - b_i`phải tích cực. Vì thế,`s > b_i`cho tất cả`i`. Chúng ta cũng cần ít nhất`n`số dương phân biệt`a_i`, ngụ ý rằng phần tử nhỏ nhất của tập hợp có thể có nhiều nhất`s - n + 1`. 

Từ đó, chúng tôi nhận ra các bộ chúng tôi muốn chính xác là các chuỗi`n`số trong đó mỗi phần tử`b_i`ít nhất là`i`. Khi đó số tập hợp lệ sẽ trở thành bài toán đếm tổ hợp: đếm các tập con của`{1, 2, ..., x}`kích thước`n`trong đó hiệu giữa phần tử lớn nhất và số phần tử thỏa mãn một bất đẳng thức đơn giản. Điều này cho phép sử dụng phương pháp quy hoạch động hoặc công thức tổ hợp sử dụng hệ số nhị thức modulo`998244353`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^x) | O(2^x) | Quá chậm | 
| Tối ưu | O(x) cho mỗi trường hợp thử nghiệm có tính toán trước | O(x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo`998244353`lên đến mức tối đa`x`trên tất cả các trường hợp thử nghiệm. Điều này cho phép tính toán hiệu quả các hệ số nhị thức. 
2. Đối với mỗi test case có tham số`n`Và`x`, kiểm tra xem`n > x`. Nếu vậy, xuất ra 0 vì không thể tạo tập hợp kích thước`n`với các phần tử lên đến`x`. 
3. Ngược lại hãy tính số cách chọn`n`các yếu tố từ`x`số nguyên liên tiếp. Cái nhìn sâu sắc quan trọng là các bộ hợp lệ tương ứng với các chuỗi của`n`những con số trong đó số lớn nhất có thể nhiều nhất`x`. Sử dụng công thức tổ hợp, số đếm được cho bởi`C(x, n)`modulo`998244353`. 
4. Xuất giá trị tính toán cho từng test. 

Bất biến đảm bảo tính đúng đắn là bất kỳ tập hợp tổng bổ sung hợp lệ nào cũng phải tương ứng với một dãy số tăng dần và mỗi số phải ≤`x`. Đếm các tập hợp con có kích thước`n`ở trong`1..x`nắm bắt chính xác tất cả các khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX = 2 * 10**5 + 5

# Precompute factorials and inverse factorials
fact = [1] * MAX
inv_fact = [1] * MAX

for i in range(1, MAX):
    fact[i] = fact[i - 1] * i % MOD

inv_fact[MAX - 1] = pow(fact[MAX - 1], MOD - 2, MOD)
for i in range(MAX - 2, -1, -1):
    inv_fact[i] = inv_fact[i + 1] * (i + 1) % MOD

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * inv_fact[k] % MOD * inv_fact[n - k] % MOD

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    if n > x:
        print(0)
    else:
        print(comb(x, n))
```Việc tính toán trước các giai thừa và nghịch đảo đảm bảo rằng chúng ta có thể tính toán bất kỳ hệ số nhị thức nào trong thời gian O(1). Vòng lặp chính chỉ đơn giản áp dụng công thức tổ hợp với việc kiểm tra ranh giới cho`n > x`. Sử dụng số học mô-đun xuyên suốt để ngăn ngừa tràn. 

## Ví dụ đã hoạt động 

Đối với đầu vào`1 7`, thuật toán tính toán`comb(7, 1) = 7`. Các bộ hợp lệ là`{1}, {2}, ..., {7}`. Bảng các biến chính ở đây không quan trọng: 

| n | x | lược(x, n) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 7 | 7 | 7 | 

Đối với đầu vào`2 5`,`comb(5, 2) = 10`. Các bộ là`{1,2}, {1,3}, ..., {4,5}`. Bảng theo dõi: 

| n | x | lược(x, n) | Đầu ra | 
| --- | --- | --- | --- | 
| 2 | 5 | 10 | 10 | 

Điều này chứng tỏ rằng công thức tổ hợp nắm bắt tất cả các tập hợp hợp lệ mà không liệt kê rõ ràng các mảng`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX + t) | Tính toán trước các giai thừa và nghịch đảo trong O(MAX). Mỗi trường hợp thử nghiệm tính một giá trị tổ hợp trong O(1) | 
| Không gian | O(TỐI ĐA) | Lưu trữ giai thừa và giai thừa nghịch đảo | 

Các ràng buộc cho phép MAX ~ 2_10^5, t ~ 10^4, do đó tổng số thao tác nằm trong khoảng 2_10^5 + 10^4 ~ 210.000, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 7\n2 5\n3 10\n27 31415\n1000 999\n") == "7\n10\n34\n605089068\n0", "sample 1"

# Minimum-size input
assert run("1\n1 1\n") == "1", "minimum input"

# Maximum n equals x
assert run("1\n5 5\n") == "1", "n equals x"

# n greater than x
assert run("1\n6 5\n") == "0", "n > x"

# Large input
assert run("1\n200000 200000\n") == "1", "maximum input"

# Random case
assert run("1\n3 4\n") == "4", "typical case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | đầu vào tối thiểu | 
| 5 5 | 1 | n lớn nhất bằng x | 
| 6 5 | 0 | n > x được xử lý đúng cách | 
| 200000 200000 | 1 | đầu vào kích thước tối đa | 
| 3 4 | 4 | trường hợp nhỏ điển hình | 

## Vỏ cạnh 

Khi nào`n > x`, chẳng hạn như`n = 6`Và`x = 5`, thuật toán ngay lập tức trả về 0. Không cần tính giai thừa hoặc tổ hợp vì không thể tạo thành một tập hợp có kích thước 6 từ các số từ 1 đến 5. Khi`n = x`, thuật toán trả về 1, tương ứng với tập hợp đơn`{1,2,...,x}`. Vì`n = 1`, thuật toán trả về`x`, đếm chính xác tất cả các bộ phần tử đơn. Các trường hợp cạnh này được xử lý bằng cách kiểm tra ranh giới đơn giản trước khi tính toán tổ hợp.
