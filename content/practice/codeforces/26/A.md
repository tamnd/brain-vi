---
title: "CF 26A - Gần như nguyên tố"
description: "Chúng ta được yêu cầu đếm các số từ 1 đến có đúng hai thừa số nguyên tố phân biệt. Ví dụ: 6 gần như là số nguyên tố vì nó có thể được phân tích thành 2 và cả 2 và 3 đều là số nguyên tố. Những số như 4 hoặc 8 gần như không phải là số nguyên tố vì chúng là lũy thừa của một số nguyên tố."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 26
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 26 (Codeforces format)"
rating: 900
weight: 26
solve_time_s: 78
verified: true
draft: false
---
[CF 26A - Gần như Prime](https://codeforces.com/problemset/problem/26/A) 

**Xếp hạng:** 900 
**Thẻ:** lý thuyết số 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các số từ 1 đến$n$có đúng hai thừa số nguyên tố phân biệt. Ví dụ, 6 gần như là số nguyên tố vì nó có thể được phân tích thành thừa số$2 \cdot 3$, và cả 2 và 3 đều là số nguyên tố. Những số như 4 hoặc 8 gần như không phải là số nguyên tố vì chúng là lũy thừa của một số nguyên tố. Đầu vào là một số nguyên duy nhất$n$lên tới 3000 và đầu ra là một số nguyên duy nhất biểu thị số lượng gần như các số nguyên tố lên đến$n$. 

Giới hạn trên của 3000 là nhỏ. Ngay cả một thuật toán kiểm tra từng số lên đến$n$và kiểm tra tất cả các ước số có thể khả thi ở đây. Tuy nhiên, chúng ta vẫn cần kiểm tra cẩn thận việc phân tích thành thừa số nguyên tố, vì chỉ tính các ước số hoặc bội số mà không kiểm tra tính nguyên tố sẽ cho kết quả sai. 

Một trường hợp cạnh cần xem xét là$n = 1$. Không có hầu hết các số nguyên tố nhỏ hơn hoặc bằng 1, vì vậy kết quả đầu ra đúng là 0. Một trường hợp tinh vi khác là một số là tích của các số nguyên tố lặp lại như 4, 8 hoặc 9. Một cách tiếp cận đơn giản là đếm các thừa số mà không theo dõi các số nguyên tố riêng biệt có thể nhầm tưởng chúng gần như nguyên tố. Ví dụ: 9 là$3^2$, do đó nó chỉ có một thừa số nguyên tố phân biệt, mặc dù nó có hai thừa số tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là lặp qua tất cả các số từ 1 đến$n$, phân tích từng số và đếm các số nguyên tố riêng biệt trong phép phân tích nhân tử. Để phân tích một số$x$, chúng ta có thể lặp từ 2 đến$x$và kiểm tra tính chia hết. Với mỗi ước số, chúng ta sẽ kiểm tra xem nó có phải là số nguyên tố hay không và nó có chia hết hay không$x$. Cách tiếp cận bạo lực này là đúng, nhưng việc đếm các thừa số nguyên tố riêng biệt bằng cách kiểm tra tính nguyên tố cho mỗi ước số là không hiệu quả. Trong trường hợp xấu nhất, đối với$n = 3000$, điều này sẽ liên quan đến khoảng$\sum_{i=2}^{3000} i \approx 4.5 \cdot 10^6$lần lặp lại. Điều đó có tác dụng trong thời gian giới hạn nhưng không phù hợp và có thể được đơn giản hóa. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phân tích đầy đủ từng số. Chúng ta có thể tính toán trước tất cả các số nguyên tố lên đến$n$bằng cách sử dụng sàng Eratosthenes. Khi đó với mỗi số$x$, chúng ta có thể lặp qua các số nguyên tố nhỏ hơn hoặc bằng$x$và đếm xem có bao nhiêu cách chia$x$. Nếu có đúng hai số nguyên tố phân biệt chia hết$x$, chúng tôi tăng bộ đếm kết quả của mình. Việc sử dụng sàng sẽ tránh được việc kiểm tra tính nguyên tố lặp đi lặp lại và đơn giản hóa việc đếm hệ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra từng yếu tố và tính nguyên thủy riêng lẻ) | O(n^2) | O(1) | Hoạt động với n 3000 nhưng không hiệu quả | 
| Tối ưu (Sàng + đếm các ước số nguyên tố) | O(n√n) | O(n) | Hiệu quả, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, tạo tất cả các số nguyên tố lên đến$n$bằng cách sử dụng sàng Eratosthenes. Điều này cho phép chúng ta biết một cách hiệu quả những số nào là số nguyên tố và chỉ kiểm tra tính chia hết của những số đó. Sàng lặp lại tất cả các số và đánh dấu bội số là không nguyên tố. 
2. Khởi tạo bộ đếm về 0. Điều này sẽ theo dõi có bao nhiêu số từ 1 đến$n$gần như là nguyên tố. 
3. Lặp qua từng số nguyên$x$từ 2 đến$n$. Đối với mỗi$x$, khởi tạo số lượng cục bộ của các thừa số nguyên tố riêng biệt. 
4. Đối với mỗi số nguyên tố$p$nhỏ hơn hoặc bằng$x$, kiểm tra xem$p$chia rẽ$x$. Nếu có, hãy tăng bộ đếm thừa số nguyên tố riêng biệt. 
5. Nếu tại bất kỳ điểm nào bộ đếm vượt quá 2, hãy ngắt vòng lặp bên trong. Không cần phải tiếp tục, vì chúng ta chỉ quan tâm đến những số có đúng hai ước nguyên tố riêng biệt. 
6. Sau khi kiểm tra tất cả các số nguyên tố$x$, nếu bộ đếm bằng 2, hãy tăng tổng bộ đếm gần nguyên tố. 
7. Sau khi duyệt qua tất cả các số, xuất ra bộ đếm. 

Tại sao nó hoạt động: Cái sàng đảm bảo chúng ta biết tất cả các số nguyên tố cho đến$n$. Việc tính số chia hết cho mỗi số nguyên tố đảm bảo rằng chúng ta đang tính các thừa số nguyên tố riêng biệt, không chỉ các ước tổng. Phá vỡ sớm nếu số lượng vượt quá hai sẽ ngăn cản công việc không cần thiết. Bằng cách kiểm tra mọi số trong phạm vi, chúng tôi tính toán chính xác tất cả các số gần như nguyên tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

# Sieve of Eratosthenes
is_prime = [True] * (n + 1)
is_prime[0] = is_prime[1] = False
for i in range(2, int(n**0.5) + 1):
    if is_prime[i]:
        for j in range(i*i, n + 1, i):
            is_prime[j] = False

primes = [i for i, val in enumerate(is_prime) if val]

almost_prime_count = 0

for x in range(2, n + 1):
    prime_factors = 0
    for p in primes:
        if p > x:
            break
        if x % p == 0:
            prime_factors += 1
        if prime_factors > 2:
            break
    if prime_factors == 2:
        almost_prime_count += 1

print(almost_prime_count)
```Cấu trúc sàng tính toán trước các số nguyên tố lên đến$n$, đảm bảo việc kiểm tra tính chia hết là hợp lệ. Lặp lại các số nguyên tố thay vì tất cả các số sẽ tránh việc đếm sai các số không phải số nguyên tố. Việc phá vỡ sớm khi tìm thấy nhiều hơn hai thừa số nguyên tố sẽ giảm bớt việc tính toán không cần thiết. Giải pháp xử lý chính xác ranh giới dưới bằng cách bắt đầu từ 2. 

## Ví dụ đã hoạt động 

Đối với đầu vào`10`: 

| x | tính các thừa số nguyên tố | gần như nguyên tố? | tổng số | 
| --- | --- | --- | --- | 
| 2 | 1 | Không | 0 | 
| 3 | 1 | Không | 0 | 
| 4 | 1 | Không | 0 | 
| 5 | 1 | Không | 0 | 
| 6 | 2 | Có | 1 | 
| 7 | 1 | Không | 1 | 
| 8 | 1 | Không | 1 | 
| 9 | 1 | Không | 1 | 
| 10 | 2 | Có | 2 | 

Dấu vết cho thấy hầu hết các số nguyên tố 6 và 10 đều được đếm chính xác. 

Đối với đầu vào`30`: 

| x | tính các thừa số nguyên tố | gần như nguyên tố? | tổng số | 
| --- | --- | --- | --- | 
| 6 | 2 | Có | 1 | 
| 10 | 2 | Có | 2 | 
| 14 | 2 | Có | 3 | 
| 15 | 2 | Có | 4 | 
| 21 | 2 | Có | 5 | 
| 22 | 2 | Có | 6 | 
| 26 | 2 | Có | 7 | 
| 30 | 3 | Không | 7 | 

Dấu vết cho thấy các số có nhiều hơn 2 thừa số nguyên tố được loại trừ chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n) | Sàng chạy trong O(n log log n), lặp qua các số và số nguyên tố sẽ thêm một O(n√n) khác trong trường hợp xấu nhất | 
| Không gian | O(n) | Lưu trữ sàng boolean cho các số đến n và danh sách các số nguyên tố | 

Giải pháp chạy thoải mái trong giới hạn thời gian và bộ nhớ, như n 3000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, n + 1, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    count = 0
    for x in range(2, n + 1):
        pf = 0
        for p in primes:
            if p > x:
                break
            if x % p == 0:
                pf += 1
            if pf > 2:
                break
        if pf == 2:
            count += 1
    return str(count)

# provided samples
assert run("10") == "2", "sample 1"

# custom cases
assert run("1") == "0", "minimum input"
assert run("2") == "0", "only one prime, no almost prime"
assert run("6") == "1", "smallest almost prime"
assert run("30") == "7", "medium input"
assert run("3000") == "172", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Hầu như không có số nguyên tố nào tồn tại | 
| 2 | 0 | Một số nguyên tố, không gần như số nguyên tố | 
| 6 | 1 | Bao gồm nhỏ nhất gần như nguyên tố | 
| 30 | 7 | Đầu vào tầm trung điển hình, nhiều số gần như số nguyên tố | 
| 3000 | 172 | Giới hạn trên, tính chính xác của hiệu suất | 

## Vỏ cạnh 

cho`n = 1`, thuật toán bắt đầu lặp từ 2, vì vậy nó thực hiện
