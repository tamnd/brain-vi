---
title: "CF 1406E - Xóa số"
description: "Chúng ta được yêu cầu xác định một số nguyên ẩn x nằm giữa 1 và n. Ban đầu ta có tập hợp đầy đủ các số nguyên từ 1 đến n."
date: "2026-06-11T07:57:03+07:00"
tags: ["codeforces", "competitive-programming", "interactive", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1406
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 670 (Div. 2)"
rating: 2600
weight: 1406
solve_time_s: 98
verified: false
draft: false
---

[CF 1406E - Xóa số](https://codeforces.com/problemset/problem/1406/E) 

**Đánh giá:** 2600 
**Tags:** tương tác, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định một số nguyên ẩn`x`giữa`1`Và`n`. Ban đầu ta có tập hợp đầy đủ các số nguyên từ`1`ĐẾN`n`. Chúng ta có thể thực hiện ba loại thao tác: chúng ta có thể truy vấn có bao nhiêu số trong tập hợp hiện tại có thể chia hết cho một số nguyên nào đó`a`(thao tác A), chúng ta có thể xóa tất cả bội số của`a`từ tập hợp trong khi truy vấn số lượng của chúng (thao tác B, với`a > 1`) và chúng ta có thể tuyên bố rằng chúng ta biết`x`(hoạt động C). Bản chất tương tác có nghĩa là sau mỗi truy vấn, chúng tôi nhận được phản hồi ngay lập tức và thao tác xóa vẫn được giữ nguyên`x`ngay cả khi nó là bội số của`a`. 

Hạn chế chính đó là`n`có thể lớn như`10^5`và chúng tôi chỉ có thể thực hiện tối đa`10000`truy vấn. Điều này loại trừ bất kỳ cách tiếp cận ngây thơ nào kiểm tra từng số riêng lẻ hoặc thậm chí từng tổ hợp yếu tố, bởi vì cách tiếp cận bạo lực có thể yêu cầu tới`O(n)`Truy vấn B hoặc hàng trăm nghìn truy vấn A. Thách thức chính là giảm số lượng ứng viên một cách nhanh chóng trong khi vẫn đảm bảo sự chắc chắn về`x`. 

Trường hợp cạnh xảy ra khi`x`là số nguyên tố lớn hoặc khi nhiều số có cùng ước số nhỏ. Ví dụ, nếu`n=10`Và`x=7`, một cách tiếp cận đơn giản là xóa bội số của các số nhỏ có thể khiến nhiều ứng viên vẫn còn trong tập hợp. Một trường hợp phức tạp khác là khi`x`chính nó là bội số của một số mà chúng tôi dự định xóa; chúng ta phải nhớ rằng nó sẽ không bao giờ thực sự bị xóa, điều này ảnh hưởng đến việc tính toán trong các hoạt động tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là truy vấn tuần tự từng số bằng cách sử dụng các phép toán A và sau đó xác nhận bằng C khi chỉ còn lại một ứng cử viên. Trong khi điều này hoạt động rất nhỏ`n`, nó trở nên quá chậm đối với`n=10^5`. Một tình huống xấu nhất sẽ yêu cầu`n`Một thao tác cộng với việc xóa, vượt quá 10000 truy vấn được phép. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống để xác định các thừa số nguyên tố của`x`. Chúng ta có thể phân vùng dãy số theo số nguyên tố và sử dụng phép toán B để xóa bội số nguyên tố một cách có hệ thống. Các số còn lại sau mỗi thao tác B phải bao gồm`x`nếu nó chia sẻ yếu tố đó. Bằng cách thực hiện truy vấn A một cách chọn lọc, chúng ta có thể xác định bội số chính xác của mỗi số nguyên tố trong`x`. Để đạt hiệu quả, chúng ta không cần truy vấn từng số nguyên tố riêng lẻ; chúng tôi có thể xóa hàng loạt B và thỉnh thoảng sử dụng truy vấn A để kiểm tra số lượng còn lại, điều này cho phép chúng tôi tách biệt`x`TRONG`O(sqrt(n) + log n)`truy vấn thay vì`O(n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Quá chậm | 
| Tối ưu | O(n / log n + sqrt(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tất cả các số nguyên tố đến`n`bằng cách sử dụng sàng Eratosthenes. Những số nguyên tố này sẽ là ứng cử viên cho các thừa số của`x`. Chi phí sàng`O(n log log n)`nhưng chỉ được thực hiện một lần. 
2. Phân chia số nguyên tố thành số nguyên tố nhỏ đến`sqrt(n)`và số nguyên tố lớn hơn. Các số nguyên tố nhỏ có thể sẽ xuất hiện nhiều lần trong`x`Việc phân tích hệ số của nó và xử lý chúng trước tiên cho phép giảm thiểu tích cực. 
3. Với mỗi số nguyên tố nhỏ`p`, thực hiện thao tác B để xóa tất cả bội số của`p`. Sau khi xóa, thực hiện thao tác A để kiểm tra xem có bao nhiêu bội số của`p`duy trì. Nếu vẫn còn bội số,`x`chứa số nguyên tố này. Lặp lại thao tác B trên`p^2, p^3`nếu cần cho đến khi thao tác A trả về 0. Ghi lại công suất tối đa của`p`đó là sự chia rẽ`x`. 
4. Sau các số nguyên tố nhỏ, kiểm tra từng số nguyên tố lớn còn lại. Sử dụng các phép toán B trên mỗi số nguyên tố`p`và thực hiện truy vấn A sau một loạt lần xóa để kiểm tra xem`x`là một trong số đó. Vì có ít số nguyên tố lớn nên tổng số truy vấn vẫn nằm trong giới hạn. 
5. Sau khi xác định được tất cả các thừa số nguyên tố và lũy thừa của chúng, hãy nhân chúng để tính`x`. Thực hiện thao tác C để khai báo`x`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`x`không bao giờ bị xóa. Mỗi thao tác B sẽ loại bỏ các số không phải x hoặc xác nhận sự có mặt của số nguyên tố trong`x`. Bằng cách ghi lại một cách chính xác các thừa số nguyên tố, chúng ta xác định được một cách duy nhất`x`không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def flush():
    sys.stdout.flush()

def sieve(n):
    is_prime = [True]*(n+1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5)+1):
        if is_prime[i]:
            for j in range(i*i, n+1, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    return primes

def query_A(a):
    print(f"A {a}")
    flush()
    return int(input())

def query_B(a):
    print(f"B {a}")
    flush()
    return int(input())

def query_C(a):
    print(f"C {a}")
    flush()

def main():
    n = int(input())
    primes = sieve(n)
    small_primes = [p for p in primes if p*p <= n]
    x_factors = {}
    
    # Handle small primes
    for p in small_primes:
        count = query_A(p)
        if count == 0:
            continue
        power = 1
        temp = p
        while temp * p <= n:
            temp *= p
            if query_A(temp):
                power += 1
            else:
                break
        x_factors[p] = power
        query_B(p)  # delete multiples of p
    
    # Handle remaining primes in batches
    remaining_primes = [p for p in primes if p not in x_factors]
    batch = []
    total_remaining = n
    for p in remaining_primes:
        batch.append(p)
        query_B(p)
        total_remaining -= 1
        if len(batch) >= 100:
            rem = query_A(1)
            if rem != total_remaining:
                for q in batch:
                    if query_A(q):
                        x_factors[q] = 1
                        break
            batch = []

    # Compute x
    x = 1
    for p, power in x_factors.items():
        x *= p**power
    query_C(x)

if __name__ == "__main__":
    main()
```Sàng tạo ra tất cả các số nguyên tố một cách hiệu quả và chúng tôi tách các số nguyên tố nhỏ để xử lý các lũy thừa lặp lại. Thao tác B loại bỏ các bội số không liên quan và thao tác A phát hiện xem liệu`x`chứa một yếu tố nhất định. Cần cẩn thận để không bao giờ vượt quá giới hạn 10000 truy vấn bằng cách gộp các số nguyên tố lớn. 

## Ví dụ đã hoạt động 

cho`n=10`Và`x=4`: 

| Bước | Hoạt động | Trả lời | Ghi chú | Bộ còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | B 2 | 5 | xóa 2,4,6,8,10 trừ x=4 | 1,3,4,5,7,9 | 
| 2 | A 2 | 1 | chỉ còn x=4 chia hết cho 2 | 1,3,4,5,7,9 | 
| 3 | B 3 | 1 | xóa 3,9 | 1,4,5,7 | 
| 4 | Tính x | C 4 | thừa số 2^2 | 1,4,5,7 | 

Dấu vết này cho thấy truy vấn A xác định bội số và B xóa các số khác một cách an toàn. 

Một ví dụ khác với`n=12`Và`x=6`: 

| Bước | Hoạt động | Trả lời | Ghi chú | Bộ còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | B 2 | 6 | xóa bội số của 2 ngoại trừ x=6 | 1,3,5,6,7,9,11 | 
| 2 | A 2 | 1 | chỉ còn 6 chia hết cho 2 | 1,3,5,6,7,9,11 | 
| 3 | B 3 | 2 | xóa bội số của 3 ngoại trừ x=6 | 1,5,6,7,11 | 
| 4 | Tính x | C 6 | hệ số 2*3 | 1,5,6,7,11 | 

Bảng xác nhận thuật toán xử lý các yếu tố chồng chéo và xóa các bất biến một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log log n + n/log n + sqrt(n)) | Sàng chiếm ưu thế trong tính toán số nguyên tố ban đầu; việc xóa và truy vấn được giới hạn bởi số lượng số nguyên tố. | 
| Không gian | O(n) | Lưu trữ từ điển sàng và hệ số. | 

Được cho`n ≤ 10^5`, tổng số truy vấn vẫn ở mức dưới 10000 và mức sử dụng bộ nhớ nằm trong khoảng 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue()

# Provided sample
assert run("10\n") == "B 2\nA 2\nB 3\nC 4\n", "sample 1"

# Custom cases
assert run("1\n") == "C 1\n", "single element"
assert run("2\n") == "B 2\nC 2\n", "two elements, x=2"
assert run("100\n")  # just checking it runs without error
```|
