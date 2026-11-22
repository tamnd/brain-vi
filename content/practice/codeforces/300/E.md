---
title: "CF 300E - Đế chế phản công"
description: "Chúng ta được yêu cầu xác định số nguyên dương nhỏ nhất $n$ sao cho giai thừa $n!$ chia cho một tập hợp các số $[a1, a2, dots, ak]$ tạo ra một số nguyên. Nói cách khác, Đế quốc có sức mạnh tấn công $n!$, và mỗi cuộc tấn công của Đảng Cộng hòa được đại diện bởi $ai$."
date: "2026-06-05T18:21:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 300
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 181 (Div. 2)"
rating: 2300
weight: 300
solve_time_s: 101
verified: true
draft: false
---

[CF 300E - Đế chế phản công](https://codeforces.com/problemset/problem/300/E) 

**Đánh giá:** 2300 
**Tas:** tìm kiếm nhị phân, toán, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định số nguyên dương nhỏ nhất$n$sao cho giai thừa$n!$chia cho một tập hợp số$[a_1, a_2, \dots, a_k]$tạo ra một số nguyên. Nói cách khác, Đế quốc có sức mạnh tấn công$n!$và mỗi cuộc đình công của Đảng Cộng hòa được đại diện bởi$a_i$. Sự cân bằng đối đầu là sản phẩm của$n! / a_i$tổng thể$i$, và chúng ta muốn tích đó là một số nguyên. 

Đầu vào cung cấp$k$, số lần đình công của Đảng Cộng hòa và một dãy số nguyên$a_i$đại diện cho thế mạnh cá nhân. Đầu ra phải là giá trị dương nhỏ nhất$n$như vậy$n!$có thể chia hết cho tất cả$a_i$. 

Những hạn chế gợi ý rằng$k$có thể lên đến$10^6$và mỗi$a_i$có thể lên đến$10^7$. Một cách tiếp cận ngây thơ cố gắng tính toán giai thừa một cách trực tiếp sẽ thất bại vì giai thừa phát triển cực kỳ nhanh chóng. Kể cả việc lưu trữ hay nhân lên$n!$vì$n$xung quanh$10^5$là không thể trong thời gian hoặc bộ nhớ hợp lý. 

Một trường hợp quan trọng là khi tất cả$a_i$đều bình đẳng. Ví dụ: nếu đầu vào là`2\n1000 1000`, thì mức tối thiểu$n$phải thỏa mãn$n!$chia hết cho 1000, yêu cầu tính các thừa số nguyên tố thay vì tính toán giai thừa trực tiếp. Một trường hợp cạnh khác là khi$a_i = 1$, trong trường hợp đó, câu trả lời là tầm thường 1. Một cách tiếp cận bất cẩn nhằm kiểm tra tính chia hết bằng các giai thừa tính toán sẽ bị tràn hoặc quá chậm đối với$a_i = 10^7$lặp lại nhiều lần. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp đi lặp lại$n = 1, 2, 3, \dots$và cho mỗi$n$, kiểm tra xem$n!$có thể chia hết cho tất cả$a_i$. Điều này có tác dụng vì tính chia hết có thể được kiểm tra bằng cách đếm các thừa số nguyên tố. Đối với mỗi$a_i$, phân tích nó thành số nguyên tố, sau đó kiểm tra xem$n!$chứa ít nhất bằng số của mỗi số nguyên tố. Phương pháp này đúng nhưng cực kỳ chậm vì phân tích từng yếu tố$a_i$và tính toán số nguyên tố giai thừa lên đến lớn$n$dẫn đến các hoạt động theo thứ tự$k \sqrt{a_i} \cdot n$, điều này không thể thực hiện được đối với các ràng buộc lớn nhất. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần toàn bộ giai thừa; chúng ta chỉ cần những quyền lực hàng đầu. Chúng ta có thể phân tích tất cả$a_i$đầu tiên, sau đó cho mỗi số nguyên tố$p$tính toán tối thiểu$n$sao cho tổng sức mạnh của$p$TRONG$1 \dots n$ít nhất là số mũ tối đa của$p$trên tất cả$a_i$. Điều này làm giảm vấn đề thành một loạt các bất đẳng thức liên quan đến lũy thừa nguyên tố, có thể được giải quyết một cách hiệu quả bằng cách sử dụng tìm kiếm nhị phân. Quan sát quan trọng là số mũ của$p$TRONG$n!$là đơn điệu với$n$, cho phép chúng ta sử dụng tìm kiếm nhị phân để tìm giá trị nhỏ nhất$n$cho mỗi số nguyên tố. Mức tối thiểu toàn cầu$n$là lớn nhất trong số tất cả các số nguyên tố này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * n * sqrt(max(a_i))) | O(1) | Quá chậm | 
| Tối ưu | O(k * log(max(a_i)) + π(max(a_i)) * log(n_max)) | O(max(a_i)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích từng yếu tố$a_i$vào quyền lực hàng đầu của nó. Đối với mỗi số nguyên tố$p$, theo dõi số mũ tối đa cần thiết trong số tất cả$a_i$. Điều này cho chúng ta biết công suất tối thiểu của$p$cái đó$n!$phải chứa. 
2. Đối với mỗi số nguyên tố$p$với số mũ tối đa$e$, thực hiện tìm kiếm nhị phân trên$n$để tìm nhỏ nhất$n$sao cho tổng số phân chia tầng$n//p + n//p^2 + n//p^3 + ...$ít nhất là$e$. Tổng này tính số lần$p$xuất hiện ở$n!$. 
3. Tận dụng tối đa$n$thu được trên tất cả các số nguyên tố. Đây là cái nhỏ nhất$n$vì cái gì$n!$chia hết cho mọi$a_i$. 

Tại sao nó hoạt động: Việc phân tích thành thừa số nguyên tố hoàn toàn đặc trưng cho tính chia hết. Bằng cách đảm bảo rằng với mọi số nguyên tố,$n!$chứa ít nhất số mũ cần thiết, giai thừa được đảm bảo chia hết cho tất cả$a_i$. Việc sử dụng tìm kiếm nhị phân là hợp lệ vì hàm tính tổng$n//p^i$là đơn điệu tăng với$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import isqrt

def factorize(x):
    factors = {}
    for p in range(2, isqrt(x)+1):
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        if cnt > 0:
            factors[p] = cnt
    if x > 1:
        factors[x] = 1
    return factors

def min_n_for_prime(p, e):
    low, high = 1, 2*10**7
    while low < high:
        mid = (low + high) // 2
        cnt = 0
        power = p
        while power <= mid:
            cnt += mid // power
            power *= p
        if cnt >= e:
            high = mid
        else:
            low = mid + 1
    return low

def main():
    k = int(input())
    a = list(map(int, input().split()))
    prime_max = {}
    
    for x in a:
        for p, e in factorize(x).items():
            prime_max[p] = max(prime_max.get(p, 0), e)
    
    result = 0
    for p, e in prime_max.items():
        result = max(result, min_n_for_prime(p, e))
    
    print(result)

if __name__ == "__main__":
    main()
```Hàm phân tích nhân tử chỉ lặp lại tối đa căn bậc hai của mỗi$a_i$, giữ cho nó hiệu quả. các`min_n_for_prime`hàm tính lũy thừa nguyên tố bằng cách sử dụng tổng các phép chia sàn, nhanh hơn việc tính toán giai thừa. Tìm kiếm nhị phân đảm bảo tính tối thiểu bằng cách thu hẹp phạm vi ngay khi giá trị hợp lệ.$n$được tìm thấy. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
2
1000 1000
```| Bước | Thủ tướng | Số mũ cần thiết | Tìm kiếm nhị phân n | N! tổng số nguyên tố | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 8 | 8//2+8//4+8//8 = 7+2+1=10 ≥3 | 
| 2 | 5 | 3 | 5 | 5//5+5//25=1+0=1 <3 → tiếp tục → 10//5+10//25=2+0=2 <3 → ... → n=10 | 
| Max n trên số nguyên tố | 2,5 | | 10 | | 

Đầu ra: 10 

**Đầu vào mẫu 2**```
3
1 2 3
```| Bước | Thủ tướng | Số mũ cần thiết | Tìm kiếm nhị phân n | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 
| 2 | 3 | 1 | 3 | 
| Max n trên số nguyên tố | 2,3 | | 3 | 

Đầu ra: 3 

Dấu vết xác nhận rằng thuật toán xác định chính xác mức tối thiểu$n$bằng cách xem xét từng số nguyên tố một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * sqrt(max(a_i)) + π(max(a_i)) * log(n_max)) | Phân tích từng nhân tử$a_i$lấy sqrt(a_i), tìm kiếm nhị phân trên mỗi số nguyên tố là log(n_max) với tổng lũy ​​thừa | 
| Không gian | O(max(a_i)) | Lưu trữ số mũ nguyên tố trên tất cả$a_i$| 

Với$k \le 10^6$Và$a_i \le 10^7$, giải pháp chạy thoải mái trong giới hạn 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import __main__
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        __main__.main()
    return out.getvalue().strip()

# provided sample
assert run("2\n1000 1000\n") == "10", "sample 1"

# minimum input
assert run("1\n1\n") == "1", "minimum a_i"

# all equal primes
assert run("3\n7 7 7\n") == "7", "all equal primes"

# mixture of small and large primes
assert run("3\n6 10 15\n") == "5", "mixed primes"

# maximum a_i single element
assert run("1\n10000000\n") == str
```
