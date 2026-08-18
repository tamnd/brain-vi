---
title: "CF 102270C - Chia"
description: "Chúng ta cần đếm các số nguyên dương (N) trong phạm vi bao gồm ([A,B]) thỏa mãn ba hạn chế có vẻ độc lập. Đầu tiên, (N) phải chia hết cho (X). Thứ hai, mọi chữ số thập phân của (N) phải thuộc tập hợp được mô tả bởi chuỗi (S)."
date: "2026-08-17T18:24:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102270
codeforces_index: "C"
codeforces_contest_name: "HCW 19 Individual Day 2"
rating: 0
weight: 102270
solve_time_s: 319
verified: false
draft: false
---

[CF 102270C - Chia](https://codeforces.com/problemset/problem/102270/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các số nguyên dương (N) trong phạm vi bao gồm ([A,B]) thỏa mãn ba hạn chế có vẻ độc lập. 

Đầu tiên, (N) phải chia hết cho (X). Thứ hai, mọi chữ số thập phân của (N) phải thuộc tập hợp được mô tả bởi chuỗi (S). Thứ ba, nếu một chữ số ở vị trí (i), được tính từ bên phải bắt đầu từ (1), thì chữ số (d) đó phải thỏa mãn (\gcd(d,i)=1). 

Điều kiện cuối cùng là phần bất thường. Ví dụ: chữ số hàng đơn vị luôn không bị giới hạn bởi điều kiện gcd vì mọi chữ số đều nguyên tố cùng nhau với (1). Tại vị trí (2), chỉ có thể xuất hiện các chữ số lẻ. Tại vị trí (3), các chữ số chia hết cho (3) đều bị cấm. Tại vị trí (5), cả (0) và (5) đều bị cấm. Chữ số 0 đặc biệt dễ bị xử lý sai: số 0 hợp lệ ở vị trí (1), vì (\gcd(0,1)=1), nhưng nó không hợp lệ ở mọi vị trí lớn hơn (1). 

Giới hạn là lý do thực sự khiến việc liệt kê trực tiếp là không thể. (B) có thể chứa tối đa (101) chữ số thập phân vì giá trị lớn nhất được phép có thể là (10^{100}), trong khi (X) có thể lớn bằng (10^5). Chúng ta không thể lặp qua các số nguyên trong khoảng và thậm chí chúng ta không thể liệt kê tất cả các chuỗi chữ số hợp lệ. Giải pháp phải hoạt động với modulo còn lại (X), tạo ra một chữ số DP với trạng thái khoảng (O(100X)) trên mỗi lớp. 

Có một số trường hợp ranh giới rất dễ mắc sai lầm. Coi như```
1 20 2
02
```Ứng cử viên duy nhất sử dụng các chữ số có sẵn gần ranh giới trên là (20), nhưng nó không hợp lệ vì chữ số hàng chục của nó là (2) và (\gcd(2,2)=2). Câu trả lời đúng là (0). Việc triển khai chỉ kiểm tra xem các chữ số có thuộc về (S) hay không sẽ đếm sai. 

Bây giờ hãy xem xét```
10 10 2
01
```Câu trả lời là (1), vì (10) chia hết cho (2), chữ số hàng đơn vị của nó là (0) và (\gcd(0,1)=1), và chữ số hàng chục của nó là (1) và (\gcd(1,2)=1). Việc triển khai chỉ từ chối các chữ số 0 sẽ bỏ lỡ trường hợp này. 

Trường hợp tinh tế thứ hai là một chữ số:```
1 1 1
0
```Câu trả lời là (0), vì chữ số duy nhất được phép là 0, nhưng bài toán tính các số tự nhiên dương nên không được đưa vào chính (0). Đây là lý do tại sao DP phải phân biệt chữ số hàng đơn vị bằng 0 với số có một chữ số hoàn chỉnh bằng 0. 

Cuối cùng, giới hạn trên có thể có nhiều chữ số hơn nhiều số hợp lệ. Ví dụ: khi (B=10^{100}), mọi số hợp lệ có ít hơn (101) chữ số sẽ tự động nhỏ hơn (B). Việc coi các số 0 đứng đầu là các chữ số thực sẽ áp dụng sai điều kiện vị trí-gcd cho các vị trí đầu không tồn tại đó. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Lặp lại qua mọi số nguyên từ (A) đến (B), loại bỏ nó nếu nó không chia hết cho (X), kiểm tra các chữ số của nó và xác minh cả giới hạn tập hợp chữ số và điều kiện gcd cho mọi vị trí. Điều này đúng vì mọi ứng viên đều được kiểm tra trực tiếp. 

Vấn đề của nó là kích thước của khoảng. Khi (A=1) và (B) gần với (10^{100}), về cơ bản có (10^{100}) ứng cử viên. Ngay cả việc kiểm tra một chữ số của mỗi ứng cử viên cũng sẽ yêu cầu một số thao tác không thể thực hiện được. Việc chuyển đổi số nguyên thành chuỗi cũng không làm thay đổi độ phức tạp cơ bản. 

Quan sát hữu ích là chúng ta không bao giờ cần giá trị của một số được xây dựng một phần, chỉ cần modulo phần còn lại của nó (X). Tồn tại nhiều nhất (10^5) số dư khác nhau. Điều kiện gcd cũng chỉ phụ thuộc vào vị trí và chữ số được chọn, vì vậy đối với vị trí (i), chúng ta có thể tính trước chính xác chữ số nào là hợp lệ. 

Có một chi tiết cấu trúc khác giúp cho việc định hướng vị trí trở nên thuận tiện. Chúng tôi xử lý các chữ số từ phải sang trái. Sau khi xử lý các vị trí (1) đến (i), phần còn lại chỉ đơn giản là 

[ 
r=\sum_{j=1}^{i} d_j10^{j-1}\pmod X. 
] 

Khi một chữ số mới được đặt ở vị trí (i+1), phần đóng góp của nó được gọi ngay là (d10^i\pmod X). Quan trọng hơn, nếu chúng ta so sánh một số có độ dài cố định với một số bị giới hạn thì chữ số mới được thêm vào sẽ có ý nghĩa hơn mọi chữ số đã được xử lý. Nếu nó khác với chữ số giới hạn tương ứng, nó hoàn toàn xác định số đó nhỏ hơn hay lớn hơn. Nếu bằng nhau thì giữ lại kết quả so sánh của các vị trí thấp hơn. 

Điều này mang lại một chữ số DP với phần còn lại là trạng thái chính và trạng thái so sánh ba giá trị cho độ dài biên. 

Chúng tôi cũng tránh thực hiện DP chữ số riêng biệt cho mọi độ dài có thể. Chúng tôi duy trì số lượng chuỗi hợp lệ của từng độ dài tăng dần. Khi một chữ số mới có ý nghĩa nhất được thêm vào, tất cả các chữ số cũ giữ nguyên vị trí tính từ bên phải nên giá trị của chúng không thay đổi. Chữ số mới chỉ phải thỏa mãn điều kiện cho vị trí mới của nó. 

Hai cách tiếp cận có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((B-A+1)\log B)) | (O(\log B)) | Quá chậm | 
| Chữ số tối ưu DP | (O(LXD)) | (O(X)) | Đã chấp nhận | 

Ở đây (L\le101) là số chữ số có liên quan và (D\le10) là số chữ số thập phân có sẵn. Phương pháp tối ưu thực hiện một số lượng DP không đổi cho hai ranh giới phạm vi. 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi chữ số (S) thành mảng các chữ số cho phép. Với mọi vị trí (i) từ (1) đến độ dài yêu cầu tối đa, hãy xây dựng danh sách các chữ số (d) thỏa mãn cả (d\in S) và (\gcd(d,i)=1). Điều này di chuyển bài kiểm tra gcd ra khỏi vòng lặp DP chính. 
2. Duy trì mảng DP được lập chỉ mục theo modulo còn lại (X). Khi xây dựng các chữ số từ phải sang trái,`dp[r]`biểu thị số dãy chữ số thấp hơn có thể có có giá trị số modulo (X) là (r). 
3. Khởi tạo vị trí (1) bằng cách sử dụng mọi chữ số được phép, kể cả số 0. Số 0 phải duy trì ở trạng thái này vì nó là chữ số đơn vị hoàn toàn hợp lệ của một số lớn hơn, như trong (10). 
4. Mở rộng DP từ vị trí (i) đến vị trí (i+1). Chữ số mới là chữ số mới có ý nghĩa nhất nên nó phải khác 0. Đóng góp của nó vào phần còn lại là (d10^i\bmod X). Do đó, trạng thái có số dư (r) sẽ chuyển sang 

[ 
(r+d10^i)\bmod X. 
] 

Bởi vì chữ số mới được thêm vào là chữ số có ý nghĩa nhất nên việc yêu cầu nó phải khác 0 sẽ đảm bảo rằng chuỗi kết quả đại diện cho một số có chính xác (i+1) chữ số. 

1. Cửa hàng`dp[0]`sau mỗi phần mở rộng là số số nguyên dương hợp lệ có độ dài chính xác chia hết cho (X). Đối với độ dài (1), xử lý số 0 riêng biệt vì DP ban đầu chứa số 0 dưới dạng chữ số đơn vị có thể nhưng số 0 không phải là số nguyên dương. 
2. Để xử lý một giới hạn như (B), chỉ độ dài chữ số chính xác của nó mới cần được xử lý đặc biệt. Mọi số hợp lệ có ít chữ số hơn sẽ tự động nhỏ hơn (B). Số lượng độ dài chính xác được tính toán trước đó có thể xử lý tất cả các số ngắn hơn đó. 
3. Để biết độ dài chính xác của giới hạn, hãy thêm trạng thái so sánh với ba giá trị: nhỏ hơn, bằng nhau và lớn hơn. Xử lý số từ phải sang trái. Khi chữ số mới được thêm vào nhỏ hơn chữ số tương ứng của giới hạn, phép so sánh hoàn chỉnh sẽ nhỏ hơn. Khi nó lớn hơn, sự so sánh trở nên lớn hơn. Khi nó bằng nhau thì so sánh trước đó không thay đổi. 
4. Ở cuối ranh giới DP, tính tổng các trạng thái còn lại bằng 0 có so sánh nhỏ hơn hoặc bằng. Điều này đưa ra số lượng số nguyên hợp lệ có cùng độ dài với giới hạn không vượt quá nó. 
5. Xác định`count_leq(B)`là tổng của tất cả các số lượng độ dài chính xác bên dưới độ dài của (B), cộng với kết quả DP biên cho độ dài của (B). Tính toán tương tự`count_leq(A-1)`và trừ hai giá trị theo modulo (10^9+7). 
6. Phép trừ một từ chuỗi thập phân được thực hiện trực tiếp trên chuỗi. Điều này là cần thiết vì (A) và (B) có thể lớn hơn bất kỳ số nguyên 64 bit tích hợp nào. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý vị trí (i), mọi trạng thái DP thể hiện chính xác các lựa chọn hợp lệ cho các vị trí (1) đến (i), chỉ được nhóm theo modulo còn lại (X) của chúng và, đối với DP biên, bằng cách so sánh chúng với hậu tố tương ứng của giới hạn. 

Điều kiện vị trí được kiểm tra chính xác khi một chữ số được đưa vào, do đó mọi chuỗi được lưu trữ đều đáp ứng yêu cầu gcd. Quá trình chuyển đổi còn lại là công thức giá trị vị trí tiêu chuẩn, do đó trạng thái (0) chứa chính xác các số chia hết cho (X). Việc yêu cầu mọi chữ số có nghĩa nhất mới được thêm vào phải khác 0 sẽ mang lại cho mỗi số nguyên dương chính xác một biểu diễn, không có số 0 trùng lặp ở đầu. 

Đối với DP biên, việc thêm một chữ số mới có nghĩa hơn sẽ ghi đè chính xác việc so sánh tất cả các chữ số ít quan trọng hơn bất cứ khi nào nó khác với giới hạn. Do đó, trạng thái so sánh cuối cùng chính xác là so sánh số thông thường giữa số được xây dựng và giới hạn. Kết hợp tất cả các độ dài ngắn hơn với số lượng ranh giới có độ dài chính xác sẽ đếm mọi số hợp lệ đến giới hạn chính xác một lần. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

MOD = 1_000_000_007

def dec_string(s):
    """Return s - 1 for a non-negative decimal string."""
    s = s.lstrip('0') or '0'
    if s == '0':
        return '-1'

    a = list(s)
    i = len(a) - 1

    while a[i] == '0':
        a[i] = '9'
        i -= 1

    a[i] = chr(ord(a[i]) - 1)
    res = ''.join(a).lstrip('0')
    return res or '0'

def prepare_digits(s, max_len):
    digits = [ord(c) - 48 for c in s]

    valid = [[] for _ in range(max_len + 1)]
    for pos in range(1, max_len + 1):
        valid[pos] = [d for d in digits if gcd(d, pos) == 1]

    return valid

def exact_counts(max_len, x, valid):
    """
    counts[len] = number of valid positive numbers with exactly len digits
    that are divisible by x.
    """
    counts = [0] * (max_len + 1)

    # Position 1 is the units digit. Zero is allowed here because it can
    # be the last digit of a multi-digit number.
    dp = [0] * x
    for d in valid[1]:
        dp[d % x] += 1

    # A one-digit number itself cannot be zero.
    c = 0
    for d in valid[1]:
        if d != 0 and d % x == 0:
            c += 1
    counts[1] = c % MOD

    power = 10 % x

    for pos in range(2, max_len + 1):
        ndp = [0] * x

        # Position pos is the most significant digit, so it must be nonzero.
        shifts = []
        for d in valid[pos]:
            if d != 0:
                shifts.append((d * power) % x)

        for shift in shifts:
            if shift == 0:
                for r in range(x):
                    ndp[r] += dp[r]
            else:
                cut = x - shift
                for r in range(cut):
                    ndp[r + shift] += dp[r]
                for r in range(cut, x):
                    ndp[r - cut] += dp[r]

        for r in range(x):
            ndp[r] %= MOD

        dp = ndp
        counts[pos] = dp[0]

        power = (power * 10) % x

    return counts

def boundary_count(bound, x, valid):
    """
    Count valid positive numbers with exactly len(bound) digits,
    divisible by x, and <= bound.
    """
    if bound == '0':
        return 0

    n = len(bound)

    # Relation:
    # 0 = smaller, 1 = equal, 2 = larger.
    less = [0] * x
    equal = [0] * x
    greater = [0] * x

    bound_digit = ord(bound[-1]) - 48

    # Start with the units digit. Zero is allowed here because the final
    # number may have more digits.
    for d in valid[1]:
        r = d % x
        if d < bound_digit:
            less[r] += 1
        elif d == bound_digit:
            equal[r] += 1
        else:
            greater[r] += 1

    if n == 1:
        # Only nonzero digits represent positive one-digit numbers.
        ans = 0
        for d in valid[1]:
            if d == 0:
                continue
            r = d % x
            if d <= bound_digit and r == 0:
                ans += 1
        return ans % MOD

    power = 10 % x

    for pos in range(2, n + 1):
        ndp_less = [0] * x
        ndp_equal = [0] * x
        ndp_greater = [0] * x

        bd = ord(bound[n - pos]) - 48

        for d in valid[pos]:
            if d == 0:
                continue

            shift = (d * power) % x

            if d < bd:
                # The new, more significant digit makes the whole number
                # smaller, regardless of the old comparison.
                if shift == 0:
                    for r in range(x):
                        ndp_less[r] += less[r] + equal[r] + greater[r]
                else:
                    cut = x - shift
                    for r in range(cut):
                        ndp_less[r + shift] += less[r] + equal[r] + greater[r]
                    for r in range(cut, x):
                        ndp_less[r - cut] += less[r] + equal[r] + greater[r]

            elif d > bd:
                # The new digit makes the whole number larger.
                if shift == 0:
                    for r in range(x):
                        ndp_greater[r] += less[r] + equal[r] + greater[r]
                else:
                    cut = x - shift
                    for r in range(cut):
                        ndp_greater[r + shift] += less[r] + equal[r] + greater[r]
                    for r in range(cut, x):
                        ndp_greater[r - cut] += less[r] + equal[r] + greater[r]

            else:
                # Equal new digit preserves the previous comparison.
                if shift == 0:
                    for r in range(x):
                        ndp_less[r] += less[r]
                        ndp_equal[r] += equal[r]
                        ndp_greater[r] += greater[r]
                else:
                    cut = x - shift

                    for r in range(cut):
                        nr = r + shift
                        ndp_less[nr] += less[r]
                        ndp_equal[nr] += equal[r]
                        ndp_greater[nr] += greater[r]

                    for r in range(cut, x):
                        nr = r - cut
                        ndp_less[nr] += less[r]
                        ndp_equal[nr] += equal[r]
                        ndp_greater[nr] += greater[r]

        for r in range(x):
            ndp_less[r] %= MOD
            ndp_equal[r] %= MOD
            ndp_greater[r] %= MOD

        less = ndp_less
        equal = ndp_equal
        greater = ndp_greater

        power = (power * 10) % x

    return (less[0] + equal[0]) % MOD

def count_leq(bound, x, valid, counts):
    if bound == '-1' or bound == '0':
        return 0

    n = len(bound)

    ans = 0

    # Every positive number with fewer digits is automatically below bound.
    for length in range(1, n):
        ans += counts[length]
        if ans >= MOD:
            ans -= MOD

    ans += boundary_count(bound, x, valid)
    return ans % MOD

def solve():
    A, B, X = input().split()
    X = int(X)
    S = input().strip()

    max_len = max(len(A), len(B))

    valid = prepare_digits(S, max_len)
    counts = exact_counts(max_len, X, valid)

    A_minus_one = dec_string(A)

    right = count_leq(B, X, valid, counts)
    left = count_leq(A_minus_one, X, valid, counts)

    print((right - left) % MOD)

if __name__ == "__main__":
    solve()
```Trình trợ giúp đầu tiên thực hiện phép trừ thập phân mà không chuyển đổi giá trị thành số nguyên. Điều này quan trọng vì đầu vào có thể vượt quá phạm vi số nguyên của máy.`prepare_digits`tính toán trước các hạn chế chữ số phụ thuộc vào vị trí. Tại vị trí (1), số 0 vẫn còn trong danh sách vì nó có thể là chữ số hàng đơn vị của một số có nhiều chữ số. Yêu cầu khác 0 chỉ được áp dụng khi một chữ số trở thành chữ số có ý nghĩa nhất.`exact_counts`là chiều dài không giới hạn DP. Trạng thái của nó chỉ chứa modulo dư (X). Trạng thái ban đầu chứa số 0 vì một số như (10) cần số 0 ở vị trí (1). Từ vị trí (2) trở đi, chữ số hàng đầu mới được thêm vào buộc phải khác 0. Phần còn lại được cập nhật bằng cách cộng chữ số mới nhân lũy thừa mười. 

DP ranh giới sử dụng cùng cấu trúc từ phải sang trái nhưng thêm ba mảng cho quan hệ so sánh. Chi tiết triển khai chính là một chữ số mới có ý nghĩa hơn tất cả các chữ số được xử lý trước đó. Do đó, một chữ số nhỏ hơn chữ số giới hạn sẽ gửi mọi trạng thái so sánh trước đó tới`less`, trong khi chữ số lớn hơn sẽ gửi mọi trạng thái tới`greater`. Chỉ có một chữ số bằng nhau giữ nguyên so sánh cũ. 

Việc chuyển đổi sử dụng`shift`thay vì xây dựng một số nguyên. Vì DP được xử lý từ phải sang trái nên việc thêm một chữ số chỉ là sự dịch chuyển theo chu kỳ của mảng còn lại. Mã tránh`% X`bên trong quá trình chuyển đổi trong cùng bằng cách chia mảng tại`X - shift`. Điều này quan trọng vì các vòng lặp trong cùng thực thi hàng triệu lần. 

Các giá trị DP được giảm theo modulo (10^9+7) một lần sau mỗi vị trí thay vì sau mỗi lần thêm. Trong một vị trí, tối đa mười giá trị trước đó được thêm vào một ô, do đó các số nguyên Python tạm thời vẫn đủ nhỏ để tối ưu hóa này. 

Cuối cùng,`count_leq(B) - count_leq(A-1)`chuyển đổi hai số tiền tố thành phạm vi bao gồm được yêu cầu. Phép trừ được thực hiện theo modulo (10^9+7), do đó, kết quả trung gian âm được Python xử lý chính xác`%`nhà điều hành. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
1 20 2
1234789
```các chữ số có sẵn là (1,2,3,4,7,8,9). Các số hợp lệ trong khoảng là (2,4,8,12,14,18). 

Sự phát triển trạng thái quan trọng của ranh giới (20) được trình bày dưới đây. 

| Vị trí từ phải | Chữ số bị ràng buộc | Hành vi chữ số hàng đầu có thể xảy ra | Số dư 0 có liên quan | 
| --- | --- | --- | --- | 
| 1 | 0 | Chữ số hàng đơn vị có thể là (1,2,3,4,7,8,9), nhưng không có số 0 | (2,4,8) | 
| 2 | 2 | Chữ số hàng chục phải khác 0 và nguyên tố cùng nhau với (2) | (12,14,18) | 
| Cuối cùng | 2 | Cho phép trạng thái bằng nhau, cho phép trạng thái nhỏ hơn | (2,4,8,12,14,18) | 

Các số có một chữ số (2,4,8) chia hết cho (2). Đối với hai chữ số, chữ số hàng chục phải là số lẻ vì vị trí (2) yêu cầu nguyên tố cùng nhau với (2), chừa (12,14,18) trong các số không vượt quá (20). Do đó, câu trả lời là (6). 

Đối với mẫu thứ hai,```
1 20 3
0123678
```các chữ số có sẵn là (0,1,2,3,6,7,8). 

| Vị trí từ phải | Chữ số bị ràng buộc | Các chữ số có giá trị vị trí từ S | Những con số góp phần trả lời | 
| --- | --- | --- | --- | 
| 1 | 0 | (0,1,2,3,6,7,8) | (3,6) | 
| 2 | 2 | (1,2,7,8) | (12,18) | 
| Cuối cùng | 2 | Các số chia hết cho (3) và nhiều nhất (20) | (3,6,12,18) | 

Tại vị trí (2), các chữ số (3) và (6) bị loại vì chúng không nguyên tố cùng nhau với (2). Các bội số hợp lệ thu được của (3) là (3,6,12,18), đưa ra câu trả lời bắt buộc (4). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(LXD)) | Có nhiều nhất (L\le101) vị trí, (X\le10^5) số dư và nhiều nhất (D=10) chuyển tiếp chữ số. Một số lượng DP không đổi xử lý hai ranh giới. | 
| Không gian | (O(X)) | Mỗi DP chỉ giữ các mảng còn lại hiện tại và tiếp theo. Số lượng độ dài chính xác chỉ yêu cầu bộ nhớ bổ sung (O(L)). | 

Với (L) giới hạn bởi khoảng một trăm và (X) giới hạn bởi (10^5), không gian trạng thái được kiểm soát bởi khoảng (10^7) sự kết hợp vị trí còn lại. Việc triển khai chỉ giữ lại một vài mảng có độ dài (X), do đó mức sử dụng bộ nhớ vẫn ở mức thấp hơn giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

MOD = 1_000_000_007

def dec_string(s):
    s = s.lstrip('0') or '0'
    if s == '0':
        return '-1'

    a = list(s)
    i = len(a) - 1

    while a[i] == '0':
        a[i] = '9'
        i -= 1

    a[i] = chr(ord(a[i]) - 1)
    return ''.join(a).lstrip('0') or '0'

def solve_instance(inp: str) -> str:
    data = inp.split()
    A, B, Xs, S = data
    X = int(Xs)

    max_len = max(len(A), len(B))

    digits = [ord(c) - 48 for c in S]
    valid = [[] for _ in range(max_len + 1)]

    for p in range(1, max_len + 1):
        valid[p] = [d for d in digits if gcd(d, p) == 1]

    counts = [0] * (max_len + 1)

    dp = [0] * X
    for d in valid[1]:
        dp[d % X] += 1

    counts[1] = sum(
        1 for d in valid[1]
        if d != 0 and d % X == 0
    ) % MOD

    power = 10 % X

    for p in range(2, max_len + 1):
        ndp = [0] * X

        for d in valid[p]:
            if d == 0:
                continue

            shift = (d * power) % X

            if shift == 0:
                for r in range(X):
                    ndp[r] += dp[r]
            else:
                cut = X - shift
                for r in range(cut):
                    ndp[r + shift] += dp[r]
                for r in range(cut, X):
                    ndp[r - cut] += dp[r]

        for r in range(X):
            ndp[r] %= MOD

        dp = ndp
        counts[p] = dp[0]
        power = (power * 10) % X

    def boundary(bound):
        if bound in ('0', '-1'):
            return 0

        n = len(bound)

        less = [0] * X
        equal = [0] * X
        greater = [0] * X

        bd = int(bound[-1])

        for d in valid[1]:
            r = d % X
            if d < bd:
                less[r] += 1
            elif d == bd:
                equal[r] += 1
            else:
                greater[r] += 1

        if n == 1:
            return sum(
                1 for d in valid[1]
                if d != 0 and d <= bd and d % X == 0
            ) % MOD

        power = 10 % X

        for p in range(2, n + 1):
            nl = [0] * X
            ne = [0] * X
            ng = [0] * X

            bd = int(bound[n - p])

            for d in valid[p]:
                if d == 0:
                    continue

                shift = (d * power) % X

                if d < bd:
                    for r in range(X):
                        v = less[r] + equal[r] + greater[r]
                        nr = r + shift
                        if nr >= X:
                            nr -= X
                        nl[nr] += v

                elif d > bd:
                    for r in range(X):
                        v = less[r] + equal[r] + greater[r]
                        nr = r + shift
                        if nr >= X:
                            nr -= X
                        ng[nr] += v

                else:
                    for r in range(X):
                        nr = r + shift
                        if nr >= X:
                            nr -= X
                        nl[nr] += less[r]
                        ne[nr] += equal[r]
                        ng[nr] += greater[r]

            for r in range(X):
                nl[r] %= MOD
                ne[r] %= MOD
                ng[r] %= MOD

            less, equal, greater = nl, ne, ng
            power = (power * 10) % X

        return (less[0] + equal[0]) % MOD

    def count_leq(bound):
        if bound in ('0', '-1'):
            return 0

        n = len(bound)
        ans = sum(counts[1:n]) % MOD
        ans += boundary(bound)
        return ans % MOD

    left = count_leq(dec_string(A))
    right = count_leq(B)

    return str((right - left) % MOD)

assert solve_instance(
    "1 20 2\n1234789\n"
) == "6", "sample 1"

assert solve_instance(
    "1 20 3\n0123678\n"
) == "4", "sample 2"

assert solve_instance(
    "1 1 1\n1\n"
) == "1", "single valid number"

assert solve_instance(
    "10 10 2\n01\n"
) == "1", "zero is valid at position 1"

assert solve_instance(
    "20 20 2\n02\n"
) == "0", "digit 2 is invalid at position 2"

assert solve_instance(
    "111 111 3\n1\n"
) == "1", "equal boundaries and repeated digits"

assert solve_instance(
    "2 4 1\n1234\n"
) == "3", "inclusive boundary"

assert solve_instance(
    "1 " + "1" + "0" * 100 + " 1\n1\n"
) == "100", "maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`1`| Phạm vi kích thước tối thiểu và sự bình đẳng chính xác | 
|`10 10 2 / 01`|`1`| Không được phép ở vị trí (1) | 
|`20 20 2 / 02`|`0`| Hạn chế Gcd tại vị trí (2) | 
|`111 111 3 / 1`|`1`| Giới hạn dưới và trên bằng nhau | 
|`2 4 1 / 1234`|`3`| Ranh giới phạm vi bao gồm | 
|`1 10^100 1 / 1`|`100`| Độ dài thập phân tối đa | 

## Vỏ cạnh 

cho```
10 10 2
01
```DP bắt đầu bằng cả hai chữ số đơn vị (0) và (1). Trạng thái 0 được giữ lại vì vị trí (1) cho phép điều đó. Khi vị trí (2) được thêm vào, chữ số (1) được cho phép và trở thành chữ số có ý nghĩa nhất khác 0. Số kết quả là (10), có phần dư modulo (2) bằng 0. Câu trả lời là (1). 

Vì```
20 20 2
02
```chữ số hàng đơn vị (0) là hợp lệ, nhưng chữ số hàng chục duy nhất có thể là (2). Vị trí (2) yêu cầu (\gcd(2,2)=1), sai, do đó quá trình chuyển đổi tạo ra chữ số hàng chục (2) không bao giờ được chèn vào DP. Câu trả lời cuối cùng là (0). 

Vì```
1 1 1
1
```ranh giới DP có một vị trí. Chữ số (1) bằng chữ số giới hạn, có phần dư bằng 0 modulo (1) và khác 0. Trạng thái bằng nhau đóng góp một số, vì vậy câu trả lời là (1). 

Đối với bài kiểm tra có độ dài tối đa```
1 10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 1
1
```giới hạn trên là (10^{100}). Chữ số duy nhất được phép là (1), vì vậy các số hợp lệ chính xác là các chuỗi bao gồm toàn bộ (1), có độ dài từ (1) đến (100). Bản thân số (10^{100}) chứa số 0 và không thể biểu thị bằng (S). DP ghi lại một số chia hết cho mỗi độ dài trong số (100) đầu tiên, cho ra (100). 

## Vỏ cạnh 

Sự khác biệt giữa số 0 ở vị trí thấp hơn và số 0 đứng đầu là một nguồn khác của lỗi đếm trùng lặp. Đối với số (102), số 0 ở giữa là chữ số thực ở vị trí (2), nhưng nó không hợp lệ vì (\gcd(0,2)=2). Đối với số (10), số 0 nằm ở vị trí (1), nơi nó hợp lệ. DP xử lý cả hai tình huống vì tính hợp lệ của vị trí được kiểm tra bằng cách sử dụng vị trí thực tế từ bên phải, trong khi hạn chế khác 0 chỉ được áp dụng khi một chữ số có nghĩa mới nhất được đưa vào. 

Phép trừ phạm vi cũng xử lý chính xác giới hạn dưới nhỏ nhất có thể. Nếu (A=1), thì (A-1=0) và`count_leq(0)`trả về 0 vì DP chỉ đếm số nguyên dương. Do đó, chênh lệch tiền tố trở thành chính xác số số nguyên hợp lệ từ (1) đến (B), không cần điều chỉnh đặc biệt ở nơi khác.
