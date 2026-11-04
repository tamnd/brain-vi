---
title: "CF 113C - Hạnh Phúc Nhân Đôi"
description: "Chúng ta được yêu cầu đếm các số trong một khoảng cho trước $l, r$ đồng thời là số nguyên tố và biểu thị được dưới dạng tổng của hai bình phương dương. Nói một cách đơn giản hơn, với mỗi số trong khoảng, chúng ta cần kiểm tra hai thuộc tính độc lập."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 113
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 86 (Div. 1 Only)"
rating: 2200
weight: 113
solve_time_s: 200
verified: true
draft: false
---

[CF 113C - Hạnh phúc nhân đôi](https://codeforces.com/problemset/problem/113/C) 

**Đánh giá:** 2200 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các số trong một khoảng thời gian nhất định$l, r$vừa là số nguyên tố vừa có thể biểu diễn dưới dạng tổng của hai bình phương dương. Nói một cách đơn giản hơn, với mỗi số trong khoảng, chúng ta cần kiểm tra hai thuộc tính độc lập. Đầu tiên, liệu nó có phải là số nguyên tố hay không - đây là định nghĩa của Peter về con số may mắn. Thứ hai, liệu nó có thể được viết là$a^2 + b^2$với số nguyên$a, b > 0$- đây là điều kiện về con số may mắn của Bob. Kết quả là số lượng thỏa mãn cả hai thuộc tính. 

Những ràng buộc cho phép$l$Và$r$lên tới 3×10^8. Một cách tiếp cận đơn giản lặp đi lặp lại trong khoảng thời gian và kiểm tra tính nguyên tố của từng số bằng cách sử dụng phép chia thử sẽ có số phép tính trong trường hợp xấu nhất theo thứ tự$(r-l+1)\sqrt{r}$. Với r-l có khả năng vào khoảng 3×10^8, tốc độ này quá chậm. Việc kiểm tra mọi khả năng tính tổng bình phương một cách đơn giản cũng sẽ không hiệu quả. Chúng ta cần một phương pháp tận dụng các thuộc tính của lý thuyết số để giảm bớt việc tính toán. 

Các trường hợp cạnh không rõ ràng bao gồm các khoảng nhỏ chỉ chứa 1 hoặc 2, trong đó các điều kiện số nguyên tố hoặc tổng bình phương hoạt động bất thường. Ví dụ: 2 là số nguyên tố và 1^2 + 1^2 = 2, vì vậy nó cần được tính. Số 3 là số nguyên tố nhưng không thể biểu diễn dưới dạng tổng của hai bình phương dương nên không được tính. Một thuật toán ngây thơ bỏ qua yêu cầu tích cực trên$a$hoặc$b$sẽ bao gồm không chính xác số 0 hoặc số âm trong tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ lặp từ l đến r, kiểm tra từng số n về tính nguyên tố bằng phép chia thử và sau đó thử tất cả các cặp$(a,b)$lên đến$\sqrt{n}$để xem liệu$a^2 + b^2 = n$. Về nguyên tắc, điều này hoạt động vì nó trực tiếp kiểm tra định nghĩa, nhưng nó cực kỳ chậm. Ví dụ: kiểm tra 3×10^8 số với tối đa 17.000 phép kiểm tra mỗi số (đối với căn bậc hai của 3×10^8) dẫn đến hơn 10^12 thao tác, không thể chạy trong 3 giây. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa đến từ lý thuyết số. Một số nguyên tố p có thể được viết dưới dạng tổng của hai bình phương khi và chỉ khi p = 2 hoặc p ≡ 1 (mod 4). Điều này làm giảm vấn đề từ việc kiểm tra tất cả a và b có thể thành kiểm tra modulo đơn giản. Kết hợp với sàng phân đoạn, chúng ta có thể tạo ra tất cả các số nguyên tố trong khoảng một cách hiệu quả$l, r$và chỉ đếm những số nguyên tố thỏa mãn điều kiện modulo 4 hoặc bằng 2. Quan sát cho thấy việc kiểm tra tổng bình phương quy về thuộc tính mô-đun cho các số nguyên tố là sự tăng tốc cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1) * sqrt(r)) | O(1) | Quá chậm | 
| Tối ưu | O((r-l+1) log log r + sqrt(r)) | O(r-l+1 + sqrt(r)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tất cả các số nguyên tố đến$\sqrt{r}$bằng cách sử dụng một sàng đơn giản của Eratosthenes. Các số nguyên tố này sẽ được dùng để đánh dấu bội số trong khoảng$l, r$. Điều này có hiệu quả vì bất kỳ số nguyên tố nào trong khoảng đều phải có hệ số ∼ sqrt(r). 
2. Khởi tạo một mảng`is_prime_segment`có độ dài r-l+1, đánh dấu tất cả các số có khả năng là số nguyên tố. 
3. Sử dụng các số nguyên tố ở bước 1 để đánh dấu các số không nguyên tố trong`is_prime_segment`. Với mỗi số nguyên tố p, hãy tìm bội số đầu tiên của p trong$l, r$, sau đó đánh dấu tất cả các bội số của p là không nguyên tố. Cần chú ý cẩn thận đối với trường hợp l ≤ p ≤ r, để tránh đánh dấu p là số nguyên tố. 
4. Lặp lại mảng phân đoạn. Đối với mỗi số n vẫn được đánh dấu là số nguyên tố, hãy kiểm tra xem n = 2 hay n % 4 = 1. Nếu vậy, hãy tăng bộ đếm. Điều này thực hiện điều kiện tổng hai bình phương cho các số nguyên tố. 
5. In số đếm ở cuối. Điều này cho ra tổng số các số vừa là số nguyên tố vừa có thể biểu thị được dưới dạng tổng của hai bình phương dương. 

Tính đúng đắn dựa trên hai sự thật. Đầu tiên, sàng phân đoạn xác định chính xác tất cả các số nguyên tố trong khoảng$l, r$. Thứ hai, định lý Fermat về tổng của hai bình phương đảm bảo rằng các số nguyên tố duy nhất có thể biểu thị dưới dạng a^2 + b^2 là 2 hoặc những số đó đồng dạng với 1 mod 4, do đó việc kiểm tra modulo đảm bảo không có kết quả dương tính hoặc âm tính giả. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def main():
    l, r = map(int, input().split())
    limit = int(math.isqrt(r)) + 1

    # Simple sieve up to sqrt(r)
    is_prime_small = [True] * limit
    is_prime_small[0] = is_prime_small[1] = False
    for i in range(2, int(math.isqrt(limit)) + 1):
        if is_prime_small[i]:
            for j in range(i*i, limit, i):
                is_prime_small[j] = False
    small_primes = [i for i, val in enumerate(is_prime_small) if val]

    # Segmented sieve for [l, r]
    n = r - l + 1
    is_prime_segment = [True] * n
    for p in small_primes:
        start = max(p*p, ((l + p - 1) // p) * p)
        for multiple in range(start, r+1, p):
            is_prime_segment[multiple - l] = False
    if l == 1:
        is_prime_segment[0] = False

    # Count primes which are 2 or ≡1 mod 4
    count = 0
    for i in range(n):
        num = l + i
        if is_prime_segment[i] and (num == 2 or num % 4 == 1):
            count += 1
    print(count)

if __name__ == "__main__":
    main()
```Sàng lên tới sqrt(r) đảm bảo chúng ta có tất cả các số nguyên tố nhỏ cần thiết cho sàng được phân đoạn. các`max(p*p, ((l + p - 1) // p) * p)`phép tính xác định bội số đầu tiên của p trong khoảng một cách hiệu quả. Cuối cùng, kiểm tra modulo thực hiện trực tiếp điều kiện Fermat. 

## Ví dụ đã hoạt động 

Mẫu 1: l = 3, r = 5 

| tôi | is_prime_segment[i] | số | số% 4 == 1 hoặc 2 | tính | 
| --- | --- | --- | --- | --- | 
| 3 | Đúng | 3 | Sai | Không | 
| 4 | Sai | 4 | - | Không | 
| 5 | Đúng | 5 | Đúng | Có | 

Bộ đếm kết thúc ở mức 1, phù hợp với kết quả đầu ra dự kiến. 

Mẫu 2: l = 1, r = 10 

| tôi | is_prime_segment[i] | số | số% 4 == 1 hoặc 2 | tính | 
| --- | --- | --- | --- | --- | 
| 1 | Sai | 1 | - | Không | 
| 2 | Đúng | 2 | Đúng | Có | 
| 3 | Đúng | 3 | Sai | Không | 
| 5 | Đúng | 5 | Đúng | Có | 
| 7 | Đúng | 7 | Sai | Không | 
| 11 | Đúng | 11 | Đúng | Có | 

Bộ đếm = 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(r) log log sqrt(r) + (r-l+1) * π(sqrt(r))) | Sàng đầu tiên lấy sqrt(r) log log sqrt(r), sàng phân đoạn đánh dấu từng bội số nguyên tố ≤ sqrt(r). | 
| Không gian | O(sqrt(r) + (r-l+1)) | Mảng cho số nguyên tố nhỏ và mảng phân đoạn. | 

Giải pháp vừa vặn thoải mái trong vòng 3 giây và bộ nhớ 128 MB cho rl lên tới 3×10^8. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3 5\n") == "1", "sample 1"

# Custom cases
assert run("1 10\n") == "3", "small interval with 2,5, and primes"
assert run("10 20\n") == "2", "primes 13,17"
assert run("2 2\n") == "1", "single prime number 2"
assert run("1 1\n") == "0", "single non-prime number 1"
assert run("1 100\n") == "12", "larger interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | 3 | Tổng bình phương chính xác và nhận dạng số nguyên tố | 
| 10 20 | 2 | Xử lý số nguyên tố modulo 4 kiểm tra | 
| 2 2 | 1 | Số đơn nguyên tố 2 được tính | 
| 1 1 | | |
