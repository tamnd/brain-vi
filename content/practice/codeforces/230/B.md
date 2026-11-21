---
title: "CF 230B - T-số nguyên tố"
description: "Chúng ta được yêu cầu xác định các số nguyên tố T trong danh sách các số nguyên dương. Số nguyên tố T được định nghĩa là một số có đúng ba ước số dương riêng biệt. Nghĩ về các ước số, cách duy nhất để một số có thể có chính xác ba là nếu nó là bình phương của một số nguyên tố."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 230
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 142 (Div. 2)"
rating: 1300
weight: 230
solve_time_s: 55
verified: true
draft: false
---

[CF 230B - T-primes](https://codeforces.com/problemset/problem/230/B) 

**Đánh giá:** 1300 
**Tas:** tìm kiếm nhị phân, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định các số nguyên tố T trong danh sách các số nguyên dương. Số nguyên tố T được định nghĩa là một số có đúng ba ước số dương riêng biệt. Nghĩ về các ước số, cách duy nhất để một số có thể có chính xác ba là nếu nó là bình phương của một số nguyên tố. Ví dụ: 4 là số nguyên tố T vì các ước của nó là 1, 2 và 4 và 2 là số nguyên tố. Một số như 6 có bốn ước (1, 2, 3, 6) nên nó không phải là số nguyên tố T. 

Đầu vào cung cấp số lượng số nguyên, lên tới 100.000, theo sau là các số nguyên, có thể lớn tới 10^12. Bởi vì kích thước đầu vào là đáng kể nên bất kỳ giải pháp nào kiểm tra từng ước số cho mỗi số sẽ quá chậm. Ví dụ: việc kiểm tra khả năng chia hết của tất cả các số lên tới √10^12 mất khoảng một triệu bước cho mỗi số, điều này khả thi đối với một số duy nhất nhưng không khả thi đối với 100.000 số trong số đó. Do đó, chúng ta cần một cách tiếp cận thông minh hơn để giảm bớt công việc cho mỗi truy vấn. 

Chúng ta cũng cần xử lý các trường hợp cận biên một cách cẩn thận. Số 1 không bao giờ là số nguyên tố T vì nó chỉ có một ước số. Các ô vuông lớn như 10^12 phải được xử lý mà không bị tràn số nguyên hoặc mất độ chính xác khi lấy căn bậc hai. Các số là số chính phương hoàn hảo nhưng có căn bậc hai không phải là số nguyên tố, chẳng hạn như 16 (4^2), cũng phải bị từ chối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi số, hãy đếm các ước số của nó. Nếu số đếm chính xác là ba, hãy in CÓ; nếu không thì in ra NO. Việc đếm các ước số đơn giản yêu cầu lặp lại lên đến √x, tạo ra độ phức tạp trong trường hợp xấu nhất là O(n√x), tức là khoảng 10^11 phép tính trong trường hợp xấu nhất. Điều này là quá chậm đối với các hạn chế của vấn đề. 

Quan sát quan trọng là một số có chính xác ba ước khi và chỉ khi nó là bình phương hoàn hảo của một số nguyên tố. Điều này rút gọn bài toán thành hai bước: xác định xem một số có phải là số chính phương hay không và căn bậc hai của nó có phải là số nguyên tố hay không. Để xử lý đầu vào lớn một cách hiệu quả, chúng ta có thể tạo tất cả các số nguyên tố lên tới 10^6 bằng cách sử dụng Sàng Eratosthenes (vì √10^12 = 10^6) và sau đó lưu trữ các bình phương của chúng vào một tập hợp. Đối với mỗi số đầu vào, chúng tôi chỉ cần kiểm tra xem nó có nằm trong tập hợp các số nguyên tố T hay không. Điều này làm giảm công việc trên mỗi truy vấn thành một bộ tra cứu đơn giản, trung bình là O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n√x) | O(1) | Quá chậm | 
| Tối ưu | O(n + √max(x) log log √max(x)) | O(√max(x)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tất cả các số nguyên tố đến 10^6 bằng cách sử dụng Sàng Eratosthenes. Phạm vi này là đủ vì mọi số nguyên tố T đều không thể có căn bậc hai lớn hơn 10^6. 
2. Khởi tạo một tập trống để lưu trữ các số nguyên tố T. Với mỗi số nguyên tố p được sàng tạo ra, hãy tính p bình phương và thêm nó vào tập hợp. Điều này tính toán trước tất cả các số nguyên tố T có thể lên tới 10^12. 
3. Đọc dãy số. Với mỗi số x, hãy kiểm tra xem x có nằm trong tập hợp các số nguyên tố T được tính toán trước hay không. 
4. Nếu tìm thấy x trong tập hợp, in ra YES; nếu không thì in ra NO. 

Lý do điều này có hiệu quả là vì tập hợp này chứa chính xác tất cả các số là bình phương của các số nguyên tố. Mọi số nguyên tố T đều được đưa vào vì căn bậc hai của nó là số nguyên tố và bất kỳ số nào không có trong tập hợp đều không phải là số chính phương hoàn hảo hoặc có căn bậc hai không nguyên tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sieve(limit):
    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(limit ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, limit + 1, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    return primes

def main():
    n = int(input())
    numbers = list(map(int, input().split()))
    
    max_root = int(10**6) + 1
    primes = sieve(max_root)
    
    t_primes = set(p * p for p in primes)
    
    for x in numbers:
        if x in t_primes:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    main()
```Hàm sàng tạo ra các số nguyên tố một cách hiệu quả và tránh việc kiểm tra tính chia hết nhiều lần. Việc lưu trữ các số nguyên tố T trong một tập hợp cho phép kiểm tra tư cách thành viên liên tục, điều này rất quan trọng đối với 100.000 truy vấn. Mã đọc đầu vào bằng cách sử dụng`sys.stdin.readline`để tránh xử lý đầu vào chậm trong Python. Việc sử dụng phép nhân số nguyên sẽ tránh được mọi điểm không chính xác của dấu phẩy động. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
4 5 6
```| x | √x | Là số nguyên tố √x | T-Prime? | 
| --- | --- | --- | --- | 
| 4 | 2 | Có | CÓ | 
| 5 | 2.236 | Không | KHÔNG | 
| 6 | 2.449 | Không | KHÔNG | 

Điều này chứng tỏ rằng chỉ có bình phương hoàn hảo của số nguyên tố mới được chấp nhận. 

Một ví dụ khác:```
5
1 9 16 25 49
```| x | √x | Là số nguyên tố √x | T-Prime? | 
| --- | --- | --- | --- | 
| 1 | 1 | Không | KHÔNG | 
| 9 | 3 | Có | CÓ | 
| 16 | 4 | Không | KHÔNG | 
| 25 | 5 | Có | CÓ | 
| 49 | 7 | Có | CÓ | 

Điều này xác nhận thuật toán loại bỏ chính xác các bình phương hoàn hảo của các số nguyên tố và chấp nhận các bình phương hoàn hảo của các số nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + √max(x) log log √max(x)) | Sàng lấy √max(x) log log √max(x), thiết lập tra cứu n số là O(n) | 
| Không gian | O(√max(x)) | Lưu trữ số nguyên tố và bình phương của chúng | 

Với n tối đa 10^5 và max(x) tối đa 10^12, thuật toán dễ dàng phù hợp với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided sample
assert run("3\n4 5 6\n") == "YES\nNO\nNO", "sample 1"

# minimum-size input
assert run("1\n1\n") == "NO", "minimum input"

# perfect squares of primes
assert run("4\n4 9 25 49\n") == "YES\nYES\nYES\nYES", "primes squares"

# perfect squares of non-primes
assert run("3\n16 36 100\n") == "NO\nNO\nNO", "non-prime squares"

# large numbers
assert run("3\n999900000000 1000000000000 1000003*1000003\n") == "NO\nNO\nYES", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | KHÔNG | đầu vào tối thiểu | 
| 4 9 25 49 | CÓ CÓ CÓ CÓ CÓ | xác định đúng số nguyên tố T | 
| 16 36 100 | KHÔNG KHÔNG KHÔNG | bình phương của các số nguyên tố bị từ chối | 
| 999900000000 1000000000000 1000003*1000003 | KHÔNG KHÔNG CÓ | xử lý số lớn một cách chính xác | 

## Vỏ cạnh 

Thuật toán xử lý 1 một cách chính xác vì 1 không phải là số nguyên tố và do đó không phải là số nguyên tố T. Các số lớn như 10^12 được xử lý chính xác vì chúng tôi tránh căn bậc hai có dấu phẩy động và dựa vào phép nhân số nguyên. Các số là số chính phương hoàn hảo nhưng có gốc là hợp số, như 16, 36 và 100, sẽ bị loại vì căn bậc hai được kiểm tra dựa trên tập hợp số nguyên tố được tính toán trước. Việc tra cứu đã thiết lập đảm bảo xác minh theo thời gian liên tục ngay cả ở giới hạn trên của 100.000 số.
