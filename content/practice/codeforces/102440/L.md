---
title: "CF 102440L - \u0420\u0430\u0437\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u043a\u0440\u043e\u043b\u0438\u043a\u043e\u0432"
description: "Chúng ta có những con thỏ được đánh số từ (1) đến (n) và mỗi con thỏ phải nhận được một trong hai nhãn (0) hoặc (1). Các nhãn không thể được chọn tùy ý. Bất cứ khi nào (b) chia (a), các nhãn phải thỏa mãn [ f(a)=f(b) text{OR} f(a/b)."
date: "2026-08-08T14:13:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "L"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 499
verified: true
draft: false
---

[CF 102440L - \u0420\u0430\u0437\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u043a\u0440\u043e\u043b\u0438\u043a\u043e\u0432](https://codeforces.com/problemset/problem/102440/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có những con thỏ được đánh số từ (1) đến (n) và mỗi con thỏ phải nhận được một trong hai nhãn (0) hoặc (1). Các nhãn không thể được chọn tùy ý. Bất cứ khi nào (b) chia (a), nhãn phải thỏa mãn 

[ 
f(a)=f(b)\ \text{OR}\ f(a/b). 
] 

Một số con thỏ đã có nhãn quy định và chúng ta cần đếm từng nhãn hoàn chỉnh thỏa mãn cả quy tắc chia hết và tất cả các quy định. Câu trả lời được lấy modulo (10^9+7). 

Phần hữu ích của cấu trúc đầu vào là sự kết hợp của (n\le 10^6) và (m\le18). Số lượng thỏ đủ lớn để bất kỳ thuật toán liệt kê trạng thái nào cho tất cả (n) con thỏ, hoặc tệ hơn, tất cả (2^n) nhãn, đều không thể thực hiện được. Đồng thời, chỉ có 18 con thỏ bị ràng buộc, điều này gợi ý rõ ràng rằng phần mũ sẽ phụ thuộc vào (m), chứ không phải vào (n). Việc phân tích tối đa 18 số quy định cũng rẻ vì mỗi số có nhiều nhất là (10^6). 

Có một số trường hợp đặc biệt trong đó việc coi quy tắc như một ràng buộc cục bộ thông thường sẽ đưa ra câu trả lời sai. Đầu tiên là con thỏ (1). Ví dụ,```
1 1
1 0
```có câu trả lời (1), bởi vì (f(1)=0) hoàn toàn hợp lệ và không có con thỏ nào khác. Một giải pháp bất cẩn có thể coi (1) là có thừa số nguyên tố và ép buộc một yếu tố khác một cách không chính xác. 

Thứ hai là khả năng (f(1)=1). Ví dụ,```
3 1
1 1
```có câu trả lời (1). Khi (f(1)=1), mọi số nguyên tố cũng phải có màu (1), và do đó mọi con thỏ đều có màu (1). Đây là một cách tô màu hoàn chỉnh rất dễ bị bỏ sót nếu chúng ta chỉ xem xét các màu được tạo ra từ các lựa chọn nguyên tố với (f(1)=0). 

Trường hợp cạnh thứ ba là một điều kiện tích cực không thể xảy ra. Ví dụ,```
5 2
4 1
2 0
```có câu trả lời (0). Điều kiện (f(2)=0) lực (f(4)=f(2)=0), mâu thuẫn với giá trị quy định (f(4)=1). Chỉ đếm số lượng thỏ quy định một cách độc lập sẽ bỏ lỡ sự phụ thuộc này. 

Một ví dụ hữu ích cuối cùng là```
5 2
2 1
3 1
```có câu trả lời (3). Có một cách tô màu cho mọi con thỏ trong nhóm (1) và khi (f(1)=0), các số nguyên tố (2) và (3) đều phải là (1), trong khi số nguyên tố (5) có thể độc lập là (0) hoặc (1). Như vậy có hai màu thuộc loại thứ hai, tổng cộng là (3). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là gán (0) hoặc (1) cho mỗi (n) con thỏ. Có (2^n) bài tập. Đối với mỗi phép gán, chúng ta có thể kiểm tra từng cặp số chia hết bằng cách lặp lại (b) và bội số của nó (a). Số cặp như vậy là 

[ 
\sum_{b=1}^{n}\left\lfloor\frac nb\right\rfloor=\Theta(n\log n). 
] 

Tại (n=10^6), đây là khoảng (1,4\cdot10^7) kiểm tra tính chia hết cho một màu, trong khi số lượng màu là (2^{10^6}). Cách tiếp cận này đúng, nhưng sự phụ thuộc theo cấp số nhân của nó vào (n) khiến nó hoàn toàn không thể sử dụng được. 

Quan sát chính xuất phát từ việc chỉ áp dụng quy tắc cho các ước số nguyên tố. Giả sử (p) là số nguyên tố và (p\mid x). Nếu (f(p)=1), thì 

[ 
f(x)=f(p)\text{ HOẶC }f(x/p)=1, 
] 

nên mọi bội số của (p) cũng phải có màu (1). Nếu thay vào đó (f(p)=0), thì 

[ 
f(x)=f(x/p). 
] 

Việc loại đi nhiều lần các thừa số nguyên tố chứng tỏ rằng màu của mọi số được xác định hoàn toàn bởi màu của các thừa số nguyên tố riêng biệt của nó, ngoại trừ lựa chọn đặc biệt là (f(1)). 

Do đó chỉ có hai trường hợp cấu trúc. 

Nếu (f(1)=1), mọi số nguyên tố (p) cũng phải có màu (1), vì việc áp dụng quy tắc cho (a=p,b=p) không bắt buộc điều này, nhưng áp dụng nó với (a=p,b=1) là lặp lại, vì vậy chúng ta cần một lập luận khác. Vì (p) có thể được biểu diễn dưới dạng (a=p,b=p), điều đó vẫn không có hạn chế. Điều kiện quyết định có được bằng cách xem xét (a=p^2,b=p): 

[ 
f(p^2)=f(p)\text{ HOẶC }f(p)=f(p). 
] 

Do đó, màu cơ bản ban đầu có thể trông độc lập. Tuy nhiên, nếu (f(1)=1), mối quan hệ tổng quát với (b=x) cũng không bắt buộc gì cả. Câu lệnh cấu trúc đúng thực sự hơi khác một chút: (f(1)) không bị ép buộc bởi quy tắc chia hết. Nếu (f(1)=1), phép truy hồi qua một số nguyên tố (p) với (f(p)=0) cho ra (f(p)=f(1)=1), vì vậy (p) không thể là (0). Do đó mọi số nguyên tố đều là (1) và mọi số đều là (1). Do đó (f(1)=1) cho chính xác một màu. 

Nếu (f(1)=0), mọi số (x>1) đều nhận được OR của màu của các thừa số nguyên tố riêng biệt của nó: 

[ 
f(x)=\bigvee_{p\mid x}f(p). 
] 

Bây giờ những ẩn số chỉ là màu sắc của các số nguyên tố. Một (x=0) quy định buộc mọi ước nguyên tố của (x) phải là (0). Một số quy định (x=1) có nghĩa là ít nhất một ước nguyên tố của (x) phải là (1). 

Điều này biến bài toán thành việc đếm các phép gán Boolean cho các số nguyên tố có nhiều nhất là 18 mệnh đề OR. Chúng ta có thể đếm những phép gán có loại trừ bao gồm đối với những con thỏ được quy định có màu (1). Đối với một tập hợp con được chọn của các ràng buộc dương, phép loại trừ bao hàm yêu cầu chúng ta làm cho mọi mệnh đề OR được chọn là sai. Làm cho mệnh đề OR sai có nghĩa là đặt tất cả các ước nguyên tố của nó thành (0). Đại lượng duy nhất chúng ta cần từ tập hợp con là số lượng các biến nguyên tố riêng biệt mà nó buộc về 0. 

Số lượng biến nguyên tố có thể lớn, lên tới (78498) khi (n=10^6), nhưng số lượng ràng buộc dương nhiều nhất là 18. Do đó, chúng tôi biểu thị các thừa số nguyên tố của từng ràng buộc dương dưới dạng bitmask số nguyên Python. Một tập hợp con các ràng buộc có thể được xử lý bằng cách lấy bit OR của mặt nạ của chúng. Phần mũ chỉ có (2^{18}=262144), phần này rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(2^n n\log n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log\log n + 2^m m)) | (O(n+2^m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một sàng Eratosthenes lên đến (n). Ngoài việc đếm tất cả các số nguyên tố đến (n), hãy giữ danh sách số nguyên tố thu được sao cho tối đa 18 số quy định có thể được phân tích thành nhân tử một cách hiệu quả. 
2. Phân chia thỏ theo quy định thành thỏ có màu (0) và thỏ có màu (1). Phân tích mọi số quy định thành các ước nguyên tố riêng biệt của nó. 
3. Thêm mọi ước số nguyên tố của một số có giá trị 0 quy định vào tập hợp`forced_zero`. Trong trường hợp (f(1)=0), một số có màu (0) chính xác khi mọi thừa số nguyên tố của nó đều có màu (0), do đó các số nguyên tố này là bắt buộc. 
4. Kiểm tra xem có thể tô màu tất cả mọi người hay không. Có thể chính xác khi không có con thỏ nào được quy định có màu (0). Nếu vậy, hãy thêm một vào câu trả lời. Điều này giải thích cho trường hợp riêng biệt (f(1)=1). 
5. Đối với trường hợp (f(1)=0), kiểm tra từng con thỏ được quy định bằng màu (1). Nếu giá trị của nó là (1), thì không có thừa số nguyên tố nào có thể tạo ra màu của nó (1), vì vậy trường hợp này đóng góp bằng 0. 
6. Cho mọi số nguyên tố xảy ra ở một ràng buộc dương và chưa bị buộc về 0 một vị trí bit. Đối với mỗi ràng buộc dương, hãy xây dựng một mặt nạ chứa chính xác các số nguyên tố này. Các số nguyên tố chỉ xuất hiện ở các ràng buộc bằng 0 đã được cố định bằng 0, vì vậy chúng không cần một chút nào. 
7. Gọi (F) là tổng số biến nguyên tố còn tự do trước khi xét các ràng buộc dương. Đối với tập con (S) của các ràng buộc dương, hãy lấy OR của mặt nạ của chúng. Các bit được đặt của nó chính xác là các số nguyên tố bổ sung phải bằng 0 để tất cả các ràng buộc trong (S) là sai. Do đó, số phép gán trong đó tất cả các ràng buộc trong (S) đều sai là 

[ 
2^{F-\operatorname{popcount}(\operatorname{OR}(S))}. 
] 

1. Áp dụng loại trừ bao hàm trên tất cả các tập con của ràng buộc dương. Thêm số lượng này cho các tập hợp con có kích thước chẵn và trừ nó cho các tập hợp con có kích thước lẻ. Giá trị kết quả chính xác là số lượng màu (f(1)=0) thỏa mãn mọi ràng buộc dương. 
2. Thêm phần đóng góp (f(1)=0) vào cách tô màu tất cả những cái có thể có, giảm modulo (10^9+7) và in kết quả. 

Lý do bất biến loại trừ bao gồm hoạt động là do ràng buộc dương là OR của các màu cơ bản của nó. Ràng buộc như vậy không chính xác khi tất cả các biến nguyên tố của nó bằng 0. Đối với bất kỳ tập hợp các ràng buộc không thành công nào được chọn, các biến số 0 bắt buộc chính xác là sự kết hợp của các tập thừa số nguyên tố của chúng. Mặt nạ OR tính toán phép kết đó mà không tính hai số nguyên tố chung. Sau đó, loại trừ bao gồm sẽ tính các bài tập trong đó không có ràng buộc tích cực nào thất bại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def build_sieve(n):
    sieve = bytearray(b'\x01') * (n + 1)

    if n >= 0:
        sieve[0] = 0
    if n >= 1:
        sieve[1] = 0

    p = 2
    while p * p <= n:
        if sieve[p]:
            start = p * p
            count = (n - start) // p + 1
            sieve[start:n + 1:p] = b'\x00' * count
        p += 1

    primes = [i for i in range(2, n + 1) if sieve[i]]
    return sieve, primes

def factor_distinct(x, primes):
    factors = []

    for p in primes:
        if p * p > x:
            break

        if x % p == 0:
            factors.append(p)
            while x % p == 0:
                x //= p

        if x == 1:
            break

    if x > 1:
        factors.append(x)

    return factors

def solve():
    n, m = map(int, input().split())

    fixed = []
    for _ in range(m):
        x, y = map(int, input().split())
        fixed.append((x, y))

    sieve, primes = build_sieve(n)
    prime_count = len(primes)

    factorized = []
    forced_zero = set()

    for x, y in fixed:
        factors = factor_distinct(x, primes)
        factorized.append((x, y, factors))

        if y == 0:
            for p in factors:
                forced_zero.add(p)

    answer = 0

    # Case f(1) = 1.
    # Then every prime must also be 1, so the whole coloring is all ones.
    if all(y == 1 for _, y in fixed):
        answer = 1

    # Case f(1) = 0.
    positive = []

    impossible = False

    for x, y, factors in factorized:
        if y == 1:
            if x == 1:
                impossible = True
                break
            positive.append(factors)

    if not impossible:
        # Assign bit positions only to primes that can still be free.
        prime_id = {}
        next_bit = 0

        for factors in positive:
            for p in factors:
                if p not in forced_zero and p not in prime_id:
                    prime_id[p] = next_bit
                    next_bit += 1

        masks = []
        for factors in positive:
            mask = 0
            for p in factors:
                if p not in forced_zero:
                    mask |= 1 << prime_id[p]
            masks.append(mask)

        # A positive constraint with all its prime factors forced to zero
        # can never become 1.
        if any(mask == 0 for mask in masks):
            c0 = 0
        else:
            free_primes = prime_count - len(forced_zero)

            k = len(masks)
            total_subsets = 1 << k

            union = [0] * total_subsets
            c0 = 0

            for subset in range(1, total_subsets):
                bit = subset & -subset
                idx = bit.bit_length() - 1
                union[subset] = union[subset ^ bit] | masks[idx]

            for subset in range(total_subsets):
                used = union[subset].bit_count()
                ways = pow(2, free_primes - used, MOD)

                if subset.bit_count() & 1:
                    c0 -= ways
                else:
                    c0 += ways

            c0 %= MOD

        answer = (answer + c0) % MOD

    print(answer)

if __name__ == "__main__":
    solve()
```Sàng cần thiết cho hai mục đích khác nhau.`prime_count`cho chúng ta biết có bao nhiêu biến nguyên tố độc lập tồn tại trong trường hợp (f(1)=0), trong khi danh sách nguyên tố cho phép chúng ta phân tích tối đa 18 số quy định. Vì mọi giá trị đầu vào tối đa là (10^6), phép chia thử cho các số nguyên tố cho đến căn bậc hai của nó là rất nhỏ. 

các`forced_zero`tập hợp đại diện cho thông tin đến từ những con thỏ có giá trị bằng 0 theo quy định. Nếu (x) được quy định bằng 0 thì mọi thừa số nguyên tố của (x) phải bằng 0. Việc loại bỏ các số nguyên tố này khỏi mặt nạ là điều cần thiết. Chúng đã được sửa, do đó việc tính lại chúng dưới dạng các biến miễn phí sẽ vượt quá số lượng bài tập. 

Điều đặc biệt`all(y == 1)`kiểm tra tay cầm (f(1)=1). Trong trường hợp đó phép truy toán thu được bằng cách loại bỏ số nguyên tố không màu sẽ mâu thuẫn với (f(1)=1), do đó mọi số nguyên tố phải là một và mọi con thỏ đều trở thành một. Có chính xác một màu như vậy. 

Đối với trường hợp (f(1)=0), mọi ràng buộc dương đều trở thành một mệnh đề nói rằng ít nhất một thừa số nguyên tố phải có màu (1). Bao gồm-loại trừ tính các bài tập đáp ứng tất cả các mệnh đề đó. các`union`mảng lưu trữ tập hợp các mặt nạ chính cho mỗi tập hợp con, sử dụng 

[ 
U(S)=U(S\setminus{i})\ \text{OR}\ M_i. 
] 

Số mũ trong`pow(2, free_primes - used, MOD)`là số biến nguyên tố còn trống sau khi các mệnh đề được chọn bị buộc phải thất bại. 

Không có vấn đề tràn số nguyên trong Python. Phép lũy thừa mô-đun được thực hiện trực tiếp với`pow(base, exponent, MOD)`, do đó, mặc dù số lượng màu toán học là rất lớn nhưng các giá trị trung gian không bao giờ cần phải được xây dựng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 2
4 1
2 0
```Các số nguyên tố đến (5) là (2,3,5). Con thỏ (2) được cố định bằng 0, do đó số nguyên tố (2) bị buộc phải bằng 0. Thỏ (4) có tập thừa số nguyên tố ({2}), nhưng được quy định là một. 

| Bước | Hệ số 0 quy định | Mặt nạ tích cực | Bài tập hợp lệ (f(1)=0) | Trả lời | 
| --- | --- | --- | --- | --- | 
| Yếu tố (2) | ({2}) | | | | 
| Yếu tố (4) | ({2}) | (0) | (0) | (0) | 
| Kiểm tra (f(1)=1) | không hợp lệ vì (2=0) được quy định | | (0) | (0) | 

Mặt nạ dương trống vì thừa số nguyên tố duy nhất của nó đã bị ép về 0. Như vậy điều kiện dương không bao giờ được thỏa mãn. Việc tô màu toàn bộ cũng bị cấm bởi số 0 quy định, vì vậy câu trả lời cuối cùng là (0). 

### Mẫu 2 

Đầu vào là```
5 2
2 1
3 1
```Có ba số nguyên tố (2,3,5) và không có số nguyên tố nào bị ép bằng 0. Với (f(1)=0), hai ràng buộc dương là mệnh đề (2=1) và (3=1). 

| Tập hợp con các mệnh đề | Hợp các số nguyên tố cưỡng bức bằng 0 | Kích thước | Ký tên | Bài tập | 
| --- | --- | --- | --- | --- | 
| (\varnothing) | (\varnothing) | 0 | (+) | (2^3=8) | 
| ({2}) | ({2}) | 1 | (-) | (2^2=4) | 
| ({3}) | ({3}) | 1 | (-) | (2^2=4) | 
| ({2,3}) | ({2,3}) | 2 | (+) | (2^1=2) | 

Bao gồm-loại trừ mang lại 

[ 
8-4-4+2=2. 
] 

Đây là hai cách tô màu với (f(1)=0), trong đó số nguyên tố (2) và (3) đều là một và số nguyên tố (5) là tùy ý. Vì mỗi giá trị được quy định là một nên màu tất cả những cái đó sẽ đóng góp thêm một giá trị nữa. 

Câu trả lời cuối cùng là (2+1=3). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log\log n + m\sqrt n + 2^m m)) | Sàng lọc phạm vi, phân tích tối đa (m) số, sau đó xử lý mọi tập hợp con của các ràng buộc dương | 
| Không gian | (O(n+2^m)) | Danh sách sàng và nguyên tố sử dụng (O(n)), trong khi mảng hợp tập con sử dụng (O(2^m)) | 

Với (n\le10^6), việc sàng có thể dễ dàng quản lý và việc phân tích hệ số chỉ 18 số là không đáng kể. Thành phần hàm mũ được giới hạn bởi các tập hợp con (2^{18}=262144), đủ nhỏ cho Python. Thuật toán tránh mọi không gian trạng thái phụ thuộc theo cấp số nhân vào hàng triệu con thỏ. 

## Trường hợp thử nghiệm```python
import sys
import io
from contextlib import redirect_stdout

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    fixed = []
    for _ in range(m):
        x, y = map(int, input().split())
        fixed.append((x, y))

    sieve = bytearray(b'\x01') * (n + 1)
    sieve[0] = 0
    if n >= 1:
        sieve[1] = 0

    p = 2
    while p * p <= n:
        if sieve[p]:
            start = p * p
            count = (n - start) // p + 1
            sieve[start:n + 1:p] = b'\x00' * count
        p += 1

    primes = [i for i in range(2, n + 1) if sieve[i]]

    def factor_distinct(x):
        res = []
        for p in primes:
            if p * p > x:
                break
            if x % p == 0:
                res.append(p)
                while x % p == 0:
                    x //= p
            if x == 1:
                break
        if x > 1:
            res.append(x)
        return res

    factorized = []
    forced_zero = set()

    for x, y in fixed:
        factors = factor_distinct(x)
        factorized.append((x, y, factors))
        if y == 0:
            forced_zero.update(factors)

    answer = 1 if all(y == 1 for _, y in fixed) else 0

    positive = []
    impossible = False

    for x, y, factors in factorized:
        if y == 1:
            if x == 1:
                impossible = True
                break
            positive.append(factors)

    if not impossible:
        prime_id = {}
        for factors in positive:
            for p in factors:
                if p not in forced_zero and p not in prime_id:
                    prime_id[p] = len(prime_id)

        masks = []
        for factors in positive:
            mask = 0
            for p in factors:
                if p not in forced_zero:
                    mask |= 1 << prime_id[p]
            masks.append(mask)

        if any(mask == 0 for mask in masks):
            c0 = 0
        else:
            free_primes = len(primes) - len(forced_zero)
            k = len(masks)
            total = 1 << k

            union = [0] * total
            c0 = 0

            for subset in range(1, total):
                bit = subset & -subset
                idx = bit.bit_length() - 1
                union[subset] = union[subset ^ bit] | masks[idx]

            for subset in range(total):
                ways = pow(
                    2,
                    free_primes - union[subset].bit_count(),
                    MOD
                )
                if subset.bit_count() & 1:
                    c0 -= ways
                else:
                    c0 += ways

            c0 %= MOD

        answer = (answer + c0) % MOD

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin

    return out.getvalue().strip()

# Provided samples
assert run("""5 2
4 1
2 0
""") == "0", "sample 1"

assert run("""5 2
2 1
3 1
""") == "3", "sample 2"

# Minimum size, f(1) = 0 gives the unique valid coloring.
assert run("""1 1
1 0
""") == "1", "minimum size with zero"

# Minimum size, f(1) = 1 gives the unique all-ones coloring.
assert run("""1 1
1 1
""") == "1", "minimum size with one"

# All prescribed values are zero. Prime 2 is forced to zero,
# while primes 3, 5, and 7 remain arbitrary.
assert run("""10 3
2 0
4 0
8 0
""") == "8", "all-equal zero constraints"

# A positive constraint on 1 is impossible when f(1) = 0,
# while the all-ones coloring remains valid.
assert run("""10 1
1 1
""") == "1", "positive constraint on one"

# Maximum n, boundary factorization at n itself.
# There are 78498 primes <= 1,000,000, and fixing n to zero
# forces exactly primes 2 and 5 to zero.
expected = pow(2, 78498 - 2, MOD)
assert run("""1000000 1
1000000 0
""") == str(expected), "maximum n boundary"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 0`|`1`| Nhỏ nhất có thể (n), xử lý đúng (1) | 
|`1 1 / 1 1`|`1`| Màu (f(1)=1) riêng biệt | 
|`10 3 / 2 0, 4 0, 8 0`|`8`| Các bội số lặp lại của cùng một ràng buộc nguyên tố bắt buộc và hoàn toàn bằng 0 | 
|`10 1 / 1 1`|`1`| Ràng buộc dương đối với (1) và trường hợp tất cả một | 
|`1000000 1 / 1000000 0`| (2^{78496}\bmod 10^9+7) | Tối đa (n), hệ số biên và số nguyên tố lớn | 

## Vỏ cạnh 

Trường hợp đặc biệt đầu tiên là (x=1). Tập thừa số nguyên tố của nó trống. Nếu nó được quy định là 0, thì nó chỉ sửa (f(1)=0), tương thích với mọi lựa chọn độc lập về màu cơ bản. Ví dụ,```
1 1
1 0
```có một câu trả lời. Thuật toán không đặt số nguyên tố vào`forced_zero`, nhập trường hợp (f(1)=0) và đếm (2^0=1) màu. 

Nếu (1) được quy định là một thì trường hợp (f(1)=0) là không thể xảy ra vì không có số nguyên tố nào có màu có thể làm cho (f(1)) bằng một. Trường hợp tất cả những cái riêng biệt đóng góp chính xác một:```
10 1
1 1
```vậy đáp án là (1). 

Trường hợp thứ hai là ràng buộc bằng 0 có các thừa số nguyên tố chồng lên một ràng buộc dương. TRONG```
5 2
4 1
2 0
```ràng buộc bằng 0 buộc số nguyên tố (2) về 0. Ràng buộc dương cho (4) chỉ có thừa số nguyên tố (2), do đó mặt nạ của nó trở thành 0. Mặt nạ bằng 0 có nghĩa là mệnh đề OR tương ứng là sai vĩnh viễn và đóng góp (f(1)=0) ngay lập tức bằng 0. Vì đơn thuốc bằng 0 cũng cấm tô màu tất cả nên kết quả cuối cùng là (0). 

Trường hợp thứ ba là khi nhiều giá trị 0 quy định chứa cùng một số nguyên tố. Vì```
10 3
2 0
4 0
8 0
```cả ba ràng buộc chỉ buộc số nguyên tố (2) bằng 0. Các số nguyên tố (3,5,7) vẫn độc lập, cho (2^3=8) cách tô màu hợp lệ. Thuật toán lưu trữ các số nguyên tố bắt buộc trong một tập hợp, do đó các lần xuất hiện lặp lại của (2) chỉ được tính một lần. 

Trường hợp cuối cùng là ranh giới tối đa (n=10^6). Vì```
1000000 1
1000000 0
```hệ số hóa là (2^6\cdot5^6), vì vậy chỉ các số nguyên tố (2) và (5) bị ép về 0. Tất cả các số nguyên tố khác đến (10^6) vẫn độc lập. Có (78498) số nguyên tố lên tới (10^6), do đó câu trả lời là 

[ 
2^{78498-2}\bmod 10^9+7. 
] 

Thuật toán không bao giờ liệt kê những nhiệm vụ chính đó. Nó chỉ cần số lượng của chúng, đó chính xác là lý do tại sao giải pháp vẫn thực tế ở mức (n) lớn nhất cho phép.
