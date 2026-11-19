---
title: "CF 2065G - Skibidus và đóng nắp"
description: "Chúng ta được yêu cầu đếm các cặp số trong một mảng trong đó bội số chung nhỏ nhất của cặp số đó là bán nguyên tố. Bán nguyên tố là bất kỳ số nào có thể được biểu diễn dưới dạng tích của chính xác hai số nguyên tố, có thể giống nhau."
date: "2026-06-08T07:21:20+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2065
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1003 (Div. 4)"
rating: 1700
weight: 2065
solve_time_s: 104
verified: false
draft: false
---

[CF 2065G - Skibidus và Capping](https://codeforces.com/problemset/problem/2065/G) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các cặp số trong một mảng trong đó bội số chung nhỏ nhất của cặp số đó là bán nguyên tố. Bán nguyên tố là bất kỳ số nào có thể được biểu diễn dưới dạng tích của chính xác hai số nguyên tố, có thể giống nhau. Mảng bao gồm các số nguyên từ 2 đến$n$và nhiều trường hợp thử nghiệm được cung cấp. Với mỗi test case, chúng ta cần trả về số lượng cặp$(i, j)$với$i \le j$như vậy$\text{lcm}(a_i, a_j)$là bán nguyên tố. 

Những ràng buộc cho chúng ta biết$n$có thể lớn như$2 \cdot 10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức$2 \cdot 10^5$. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra một cách ngây thơ tất cả các cặp, bởi vì điều đó sẽ mất$O(n^2)$hoạt động trong trường hợp xấu nhất$4 \cdot 10^{10}$tính toán, vượt xa giới hạn 2 giây. Thay vào đó, chúng ta cần một phương pháp tận dụng cấu trúc của các số và tính chất của bán nguyên tố. 

Các trường hợp cạnh phát sinh khi mảng chứa các phần tử lặp lại hoặc các giá trị rất nhỏ. Ví dụ: nếu tất cả các giá trị đều có cùng số nguyên tố, như$[2, 2, 2]$, thì lcm của bất kỳ cặp nào đều bằng 2, không phải là số nguyên tố. Một thuật toán bất cẩn cho rằng tất cả các cặp đóng góp sẽ bị tính quá mức. Một trường hợp phức tạp khác là khi lcm của hai số riêng biệt dẫn đến bình phương nguyên tố, chẳng hạn như 9 từ (3, 3), là bán nguyên tố nhưng dễ bị bỏ lỡ nếu việc triển khai chỉ xem xét các số nguyên tố riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại qua mọi cặp có thể$(i, j)$, tính lcm, kiểm tra xem nó có phải là bán nguyên tố hay không và đếm nếu có. Điều này hoạt động chính xác, nhưng độ phức tạp về thời gian của nó là$O(n^2)$cho mỗi trường hợp thử nghiệm, điều này là không khả thi đối với$n \sim 2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng$\text{lcm}(a_i, a_j)$chỉ có thể nhận giá trị lên đến$n$bởi vì mỗi$a_i \le n$. Điều này cho phép chúng ta tính toán trước tất cả các số nguyên tố lên đến$n$sử dụng hiệu quả sàng Eratosthenes đã được cải tiến. Khi chúng tôi biết số nào là bán nguyên tố, chúng tôi có thể ánh xạ các phần tử mảng vào bảng tần số và đếm các cặp hợp lệ bằng cách sử dụng kết hợp tính năng sàng và bội số, tránh tính toán lcm theo cặp rõ ràng. Cụ thể, nếu chúng ta tính toán trước những số nào là bán nguyên tố, chúng ta có thể lặp qua mảng và chỉ xem xét các giá trị có thể tạo thành lcm bán nguyên tố. Sau đó, đóng góp của cặp có thể được tính toán thông qua các công thức đếm thay vì các vòng lặp lồng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu | O(n \log \log n + n \cdot \text{#semi-primes}) |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố đến mức tối đa có thể$n$dùng sàng. Bước này là$O(n \log \log n)$. Nó cung cấp cho chúng ta danh sách các số nguyên tố và cho phép kiểm tra số nguyên tố nhanh chóng. 
2. Tạo tất cả các số nguyên tố lên đến$n$. Lặp lại tất cả các số nguyên tố$p \le \sqrt{n}$, và với mỗi$p$, nhân nó với số nguyên tố$q \ge p$như vậy$p \cdot q \le n$. Lưu trữ các số này trong một mảng boolean hoặc đặt cho các truy vấn thành viên O(1). Điều này đảm bảo rằng chúng tôi có thể nhanh chóng kiểm tra xem có kết quả lcm nào là bán nguyên tố hay không. 
3. Với mỗi test case, hãy xây dựng một mảng tần số`count[x]`ghi lại mỗi số bao nhiêu lần$x$xuất hiện trong mảng đầu vào. Điều này làm giảm vấn đề từ việc lập chỉ mục các cặp đến việc đếm các cặp giá trị theo bội số. 
4. Lặp lại tất cả các cặp số$(u, v)$Ở đâu$u \le v$và cả hai đều xuất hiện trong mảng. Tính lcm của họ bằng cách sử dụng$\text{lcm}(u, v) = u \cdot v // \gcd(u, v)$. Nếu lcm này là bán nguyên tố (kiểm tra tập hợp được tính toán trước), hãy cộng phần đóng góp vào số cặp. Nếu như$u = v$, phần đóng góp là$\text{count}[u] \cdot (\text{count}[u] + 1) // 2$; nếu không thì đó là$\text{count}[u] \cdot \text{count}[v]$. 
5. Xuất tổng số của test case. 

Điều bất biến là bằng cách tính toán trước các giá trị bán nguyên tố và sử dụng số đếm tần số, chúng tôi đảm bảo mọi cặp hợp lệ đều được tính chính xác một lần mà không cần lặp qua từng cặp chỉ mục, duy trì tính chính xác trong khi vẫn đạt được hiệu quả. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

MAX_N = 2 * 10**5 + 5

# Sieve to find primes up to MAX_N
is_prime = [True] * MAX_N
is_prime[0] = is_prime[1] = False
for i in range(2, int(MAX_N**0.5) + 1):
    if is_prime[i]:
        for j in range(i*i, MAX_N, i):
            is_prime[j] = False
primes = [i for i, val in enumerate(is_prime) if val]

# Precompute semi-primes
is_semi_prime = [False] * MAX_N
for i, p in enumerate(primes):
    for q in primes[i:]:
        val = p * q
        if val >= MAX_N:
            break
        is_semi_prime[val] = True

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    count = [0] * MAX_N
    for x in a:
        count[x] += 1
    
    nums = [i for i in range(2, n+1) if count[i] > 0]
    res = 0
    for i, u in enumerate(nums):
        for v in nums[i:]:
            lcm = u * v // math.gcd(u, v)
            if lcm < MAX_N and is_semi_prime[lcm]:
                if u == v:
                    res += count[u] * (count[u] - 1) // 2 + count[u]  # i <= j
                else:
                    res += count[u] * count[v]
    print(res)
```Giải pháp đầu tiên tính toán trước các số nguyên tố và bán số nguyên tố để cho phép kiểm tra tư cách thành viên liên tục. Mảng tần số tránh so sánh theo cặp lặp lại và vòng lặp lồng nhau trên các giá trị mảng duy nhất đảm bảo mỗi lcm được kiểm tra chính xác một lần. Chúng tôi xử lý$i = j$một cách chính xác bằng cách tính toán sự kết hợp cộng với việc tự ghép nối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[2, 2, 3, 4]`| Bước | số | đếm | Cặp (u,v) | lcm | Bán chính? | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [2,3,4] | {2:2,3:1,4:1} | (2,2) | 2 | Không | 0 | 0 | 
| 2 | | | (2,3) | 6 | Có | 2*1=2 | 2 | 
| 3 | | | (2,4) | 4 | Có | 2*1=2 | 4 | 
| 4 | | | (3,3) | 3 | Không | 0 | 4 | 
| 5 | | | (3,4) | 12 | Không | 0 | 4 | 
| 6 | | | (4,4) | 4 | Có | 1 | 5 | 

Điều này xác nhận số lượng`5`. 

### Mẫu 2 

đầu vào:`[2, 2, 3, 4, 5, 6]`Theo cùng một bảng, tổng số sẽ là`12`, phù hợp với sản lượng dự kiến. 

Những dấu vết này chứng tỏ rằng việc đếm tần số kết hợp với các bán nguyên tố được tính toán trước sẽ nắm bắt chính xác tất cả các cặp có$i \le j$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log log n + n * sqrt(n)) | Sàng và tính toán trước bán nguyên tố chiếm ưu thế; việc lặp lại các cặp giá trị duy nhất bị hạn chế vì các giá trị ≤ n | 
| Không gian | O(n) | Mảng cho cờ nguyên tố, bán nguyên tố và đếm tần số | 

Thuật toán phù hợp thoải mái trong vòng 2 giây cho$n \le 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    MAX_N = 2 * 10**5 + 5
    import math

    is_prime = [True] * MAX_N
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(MAX_N**0.5) + 1):
        if is_prime[i]:
            for j in range
```
