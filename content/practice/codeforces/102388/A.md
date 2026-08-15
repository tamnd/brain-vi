---
title: "CF 102388A - Đế lạ"
description: "Chúng ta cần in biểu diễn chính tắc duy nhất của một số nguyên dương (n) bằng cách sử dụng lũy ​​thừa tỷ lệ vàng [ phi=frac{1+sqrt5}{2}. ] Mỗi vị trí chứa (0) hoặc (1) và hai vị trí lân cận không bao giờ có thể chứa cả (1)."
date: "2026-08-15T08:21:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "A"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 617
verified: true
draft: false
---

[CF 102388A - Căn cứ kỳ lạ](https://codeforces.com/problemset/problem/102388/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần in biểu diễn chính tắc duy nhất của một số nguyên dương (n) bằng cách sử dụng lũy thừa của tỷ lệ vàng 

[ 
\phi=\frac{1+\sqrt5}{2}. 
] 

Mỗi vị trí chứa (0) hoặc (1) và hai vị trí lân cận không bao giờ có thể chứa cả (1). Không giống như hệ thống vị trí thông thường, các vị trí hữu ích có thể có số mũ âm, do đó đầu ra có thể chứa một phần phân số. Ví dụ, 

[ 
2=\phi^1+\phi^{-2}, 
] 

được viết là`10.01`. 

Đầu vào chứa tối đa mười số nguyên độc lập, với mọi (n\le 100000). Một giải pháp bậc hai theo (n) sẽ thực hiện tối đa (10^{10}) phép tính cho một trường hợp thử nghiệm, vượt xa giới hạn một giây. Ngay cả một cách tiếp cận khám phá nhiều chuỗi chữ số theo cấp số nhân cũng hoàn toàn không khả thi. Chúng ta cần một thuật toán có công tăng dần theo số vị trí cơ sở-(\phi), thuật toán này chỉ có logarit trong (n). 

Có một số trường hợp khó xử lý. Đầu vào nhỏ nhất là (n=1), có câu trả lời đơn giản là`1`. Bộ định dạng luôn in một điểm cơ số có thể tạo ra không chính xác`1.`. 

Số đầu tiên cần lũy thừa âm là (n=2). Câu trả lời của nó là```
2
10.01
```Một phương pháp chỉ tìm kiếm lũy thừa (\phi^0,\phi^1,\phi^2,\ldots) không bao giờ có thể hoàn thành, vì (2) không phải là tổng của các lũy thừa dương cần thiết. 

Số học dấu phẩy động là một trường hợp nguy hiểm khác. Đối với (n=2), danh tính chính xác là (\phi+\phi^{-2}=2), nhưng việc đánh giá hai số hạng bằng dấu phẩy động thông thường có thể để lại một số dư nhỏ thay vì chính xác bằng 0. Một vòng lặp tiếp tục tạo ra lũy thừa âm cho đến khi số dư bằng 0 thì có thể tạo ra các chữ số phụ không chính xác. Giải pháp dưới đây không bao giờ đánh giá (\ phi) bằng số. 

Cuối cùng, những cái liền kề phải bị cấm. Ví dụ: (4) được biểu diễn dưới dạng`101.01`, không phải bởi sự kết hợp tùy tiện của các quyền lực lân cận. Quá trình tham lam tự động tạo ra sự phân tách cần thiết vì danh tính 

[ 
\phi^{k+1}-\phi^k=\phi^{k-1}. 
] 

Đặc tính tham lam được sử dụng ở đây là biểu diễn chuẩn, hay Bergman, base-(\phi). 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ chọn một phạm vi số mũ và liệt kê mọi chuỗi chữ số nhị phân có thể có, từ chối các chuỗi chứa các chuỗi liền kề và kiểm tra chuỗi còn lại nào đánh giá là (n). Về nguyên tắc, điều này đúng vì biểu diễn được yêu cầu là hữu hạn và duy nhất, nhưng không gian tìm kiếm tăng theo cấp số nhân. Đối với phạm vi 49 vị trí, có (F_{51}=20,365,011,074) chuỗi nhị phân không có chuỗi liền kề, do đó, ngay cả việc kiểm tra một ứng cử viên trong thời gian không đổi cũng sẽ vô vọng đối với đầu vào lớn nhất. 

Một cách tiếp cận tự nhiên hơn nhưng vẫn không phù hợp là cộng hoặc trừ nhiều lần lũy thừa của (\phi) trong khi vẫn duy trì biểu thức tượng trưng. Nó tránh liệt kê tất cả các chuỗi, nhưng nếu không có sự quan sát tham lam thì không có lý do gì để biết nên chọn lũy thừa nào tiếp theo, vì vậy thuật toán vẫn phải khám phá các lựa chọn thay thế. 

Quan sát quan trọng là biểu diễn kinh điển có thể được xây dựng một cách tham lam. Tại mọi điểm, gọi (r) là số dư dương còn biểu diễn. Chọn lũy thừa lớn nhất (\phi^k) thỏa mãn 

[ 
\phi^k\le r. 
] 

Đặt (1) vào vị trí (k), sau đó thay (r) bằng (r-\phi^k). Việc lặp lại điều này cuối cùng đạt tới số 0 và tạo ra biểu diễn chuẩn. Đây chính xác là đặc tính tham lam của biểu diễn cơ sở-(\phi) tối thiểu. 

Có khó khăn thứ hai: các phép so sánh liên quan đến (\phi) không thể sử dụng dấu phẩy động thông thường một cách an toàn. Một thực tế đại số hữu ích là mọi lũy thừa của (\phi) có thể được viết chính xác dưới dạng 

[ 
a+b\phi 
] 

đối với số nguyên (a, b). Kể từ khi 

[ 
\phi^2=\phi+1, 
] 

nhân một cặp như vậy với (\phi) chỉ là 

[ 
(a+b\phi)\phi=b+(a+b)\phi. 
] 

Tương tự, vì (1/\phi=\phi-1) nên chia cho (\phi) là 

[ 
(a+b\phi)/\phi=(b-a)+a\phi. 
] 

Vì vậy, chúng ta có thể tạo ra mọi công suất cần thiết chỉ bằng cách sử dụng số học số nguyên. 

Bản thân sự so sánh cũng có thể chính xác. cho 

[ 
x=a+b\phi, 
] 

viết 

[ 
x=\frac{(2a+b)+b\sqrt5}{2}. 
] 

Dấu của biểu thức này có thể được xác định chỉ bằng phép nhân và so sánh số nguyên. Không có con số gần đúng nào trong thuật toán. 

Do đó, cách tiếp cận brute-force không thành công vì số lượng chuỗi chữ số hợp lệ là theo cấp số nhân, trong khi quan sát thấy rằng biểu diễn chính tắc là tham lam sẽ làm giảm vấn đề khi quét lũy thừa một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(F_L)), trong đó (L) là số vị trí | (O(L)) | Quá chậm | 
| Tham lam với số học đại số chính xác | (O(L)) | (O(L)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biểu diễn mọi số đại số ở dạng ((a,b)), nghĩa là (a+b\phi). Số nguyên đầu vào (n) bắt đầu là ((n,0)) và phần còn lại hiện tại được lưu trữ ở dạng tương tự. 
2. Tìm số mũ (k) lớn nhất mà (\phi^k\le n). Bắt đầu bằng (\phi^0=1) và nhân liên tục công suất hiện tại với (\phi) trong khi nó không vượt quá (n). Vì (\phi>1), quá trình dừng sau (O(\log n)) lần lặp. 
3. Bắt đầu từ số mũ lớn nhất này, xử lý lũy thừa theo thứ tự giảm dần. Tại số mũ (k), so sánh số dư hiện tại với (\phi^k). Nếu nguồn phù hợp, đặt chữ số (1) ở vị trí này và trừ đi nguồn còn lại. Ngược lại đặt chữ số (0). 
4. Sau mỗi vị trí, chia công suất hiện tại cho (\phi) bằng cách sử dụng phép biến đổi cặp chính xác ((a,b)\mapsto(b-a,a)). Điều này chuyển từ số mũ (k) sang số mũ (k-1). 
5. Dừng lại ngay khi số dư bằng ((0,0)). Thuộc tính biểu diễn hữu hạn đảm bảo rằng quá trình tham lam đạt đến số không. 
6. Các chữ số đã được thu thập từ số mũ lớn nhất đến số mũ nhỏ nhất. Tách chúng ngay sau số mũ (0), loại bỏ các số 0 đứng đầu không cần thiết khỏi phần nguyên và các số 0 ở cuối khỏi phần phân số và chỉ chèn dấu thập phân nếu vẫn còn một chữ số phân số. 

Lý do những người bên cạnh không bao giờ xuất hiện trực tiếp từ sự lựa chọn tham lam. Giả sử (\phi^k) được chọn. Trước khi chọn nó, số dư nhỏ hơn (\phi^{k+1}), vì (k) là số mũ lớn nhất có thể có. Do đó, sau khi trừ, số dư mới nhỏ hơn 

[ 
\phi^{k+1}-\phi^k=\phi^{k-1}. 
] 

Do đó, vị trí tiếp theo (k-1) không thể được chọn. Quá trình tham lam tự động thỏa mãn quy tắc không có cái liền kề. 

**Tại sao nó hoạt động.** Tại mỗi lần lặp, phần còn lại (r) chính xác là giá trị được biểu thị bằng tất cả các chữ số chưa được chọn. Thuật toán chọn lũy thừa lớn nhất không vượt quá (r), đây chính xác là lựa chọn xác định của biểu diễn tham lam chính tắc. Trừ đi lũy thừa đó bảo toàn bất biến, và danh tính (\phi^{k+1}-\phi^k=\phi^{k-1}) chứng tỏ rằng hai vị trí được chọn liên tiếp là không thể. Khi phần còn lại bằng 0, các lũy thừa đã chọn tính tổng chính xác với (n) ban đầu và tính duy nhất của biểu diễn chuẩn có nghĩa là chuỗi chữ số kết quả là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mul_phi(p):
    a, b = p
    return (b, a + b)

def div_phi(p):
    a, b = p
    return (b - a, a)

def nonnegative(a, b):
    """
    Return whether a + b*phi >= 0 exactly.

    a + b*phi = (2*a + b + b*sqrt(5)) / 2.
    """
    c = 2 * a + b

    if c == 0:
        return b >= 0

    if b > 0 and c > 0:
        return True

    if b < 0 and c < 0:
        return False

    if c > 0:
        return c * c >= 5 * b * b

    return 5 * b * b >= c * c

def geq(x, y):
    """
    Return whether x >= y for two numbers represented as
    (a, b) = a + b*phi.
    """
    a = x[0] - y[0]
    b = x[1] - y[1]
    return nonnegative(a, b)

def solve_one(n):
    # Find the largest non-negative exponent whose power does not exceed n.
    power = (1, 0)  # phi^0
    k = 0

    while True:
        nxt = mul_phi(power)
        if not geq((n, 0), nxt):
            break
        power = nxt
        k += 1

    max_k = k
    remainder = (n, 0)
    digits = []

    # Greedily process phi^k, phi^(k-1), ...
    while remainder != (0, 0):
        if geq(remainder, power):
            digits.append('1')
            remainder = (
                remainder[0] - power[0],
                remainder[1] - power[1]
            )
        else:
            digits.append('0')

        power = div_phi(power)
        k -= 1

    s = ''.join(digits)

    # The digit corresponding to phi^0 is at index max_k.
    split = max_k + 1
    left = s[:split].lstrip('0')
    right = s[split:].rstrip('0')

    if not left:
        left = '0'

    if right:
        return left + '.' + right

    return left

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        out.append(solve_one(n))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    main()
```các`mul_phi`thực hiện chức năng 

[ 
(a+b\phi)\phi=b+(a+b)\phi, 
] 

vì vậy nó di chuyển một số mũ sang bên phải. các`div_phi`hàm thực hiện phép nhân với (\phi^{-1}=\phi-1), di chuyển một số mũ sang trái. 

các`nonnegative`chức năng là chi tiết chính xác quan trọng. Nó không bao giờ chuyển đổi (\phi) thành số dấu phẩy động. Với (a+b\phi), nhân với (2) sẽ được (2a+b+b\sqrt5). Nếu hai thành phần vô tỷ có cùng dấu thì câu trả lời là ngay lập tức. Nếu dấu của chúng khác nhau, việc bình phương hai đại lượng dương sẽ cho kết quả so sánh chính xác giữa (c^2) và (5b^2). 

Vòng lặp đầu tiên định vị số mũ không âm lớn nhất có thể sử dụng được. Vòng lặp thứ hai sau đó thực hiện việc xây dựng tham lam từ số mũ đó trở xuống. Vòng lặp được phép chuyển thành số mũ âm, điều này là cần thiết vì các số nguyên như (2) và (123) yêu cầu vị trí phân số. 

Định dạng đầu ra sử dụng`max_k + 1`làm vị trí phân chia vì chữ số ở số mũ (0) chính xác là chữ số cuối cùng của phần nguyên. Các số 0 đứng đầu trước số quan trọng nhất (1) và các số 0 ở cuối sau số ít quan trọng nhất (1) bị loại bỏ. Dấu thập phân chỉ được in khi có phần phân số không trống. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên mặc dù hệ số lũy thừa tăng lên như số Fibonacci. 

## Ví dụ đã hoạt động 

Đối với giá trị mẫu (n=2), lũy thừa lớn nhất không vượt quá (2) là (\phi^1). Quá trình tham lam sau đó đạt đến biểu diễn chính xác (\phi+\phi^{-2}). 

| Số mũ (k) | Công suất hiện tại | Phần còn lại trước bước | Hành động | Phần còn lại sau bước | 
| --- | --- | --- | --- | --- | 
| (1) | (\phi) | (2) | chọn (1) | (2-\phi) | 
| (0) | (1) | (2-\phi) | chọn (0) | (2-\phi) | 
| (-1) | (\phi^{-1}) | (2-\phi) | chọn (0) | (2-\phi) | 
| (-2) | (\phi^{-2}) | (2-\phi) | chọn (1) | (0) | 

Vì (\phi^{-2}=2-\phi), các chữ số cuối cùng là`10.01`. Dấu vết cũng cho thấy tại sao phần phân số không thể được xử lý đơn giản như phân số thập phân thông thường. 

Đối với ví dụ thứ hai được xây dựng (n=5), công suất khả dụng lớn nhất là (\phi^3). Phần dư chính xác dần dần trở nên đủ nhỏ để việc hiệu chỉnh cuối cùng được thực hiện với lũy thừa âm. 

| Số mũ (k) | Công suất hiện tại | Phần còn lại trước bước | Hành động | Phần còn lại sau bước | 
| --- | --- | --- | --- | --- | 
| (3) | (1+2\phi) | (5) | chọn (1) | (4-2\phi) | 
| (2) | (1+\phi) | (4-2\phi) | chọn (0) | (4-2\phi) | 
| (1) | (\phi) | (4-2\phi) | chọn (0) | (4-2\phi) | 
| (0) | (1) | (4-2\phi) | chọn (0) | (4-2\phi) | 
| (-1) | (-1+\phi) | (4-2\phi) | chọn (1) | (5-3\phi) | 
| (-2) | (2-\phi) | (5-3\phi) | chọn (0) | (5-3\phi) | 
| (-3) | (-3+2\phi) | (5-3\phi) | chọn (0) | (5-3\phi) | 
| (-4) | (5-3\phi) | (5-3\phi) | chọn (1) | (0) | 

Biểu diễn kết quả là`1000.1001`. Biểu diễn cặp chính xác làm cho phép trừ cuối cùng bằng 0 thay vì chỉ gần bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log n)) | Quét tham lam xử lý một số lũy thừa tỷ lệ thuận với độ dài biểu diễn, tăng theo logarit với (n). | 
| Không gian | (O(\log n)) | Các chữ số đầu ra và các hệ số đại số chính xác đều chỉ yêu cầu một mục nhập cho mỗi số mũ được xử lý. | 

Đối với (n\le100000), số lượng vị trí được xử lý chỉ có vài chục. Với tối đa mười trường hợp thử nghiệm, tổng lượng số học rất nhỏ so với giới hạn thời gian một giây và mức sử dụng bộ nhớ không đáng kể so với 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run the solution on an input string
import sys
import io

def mul_phi(p):
    a, b = p
    return (b, a + b)

def div_phi(p):
    a, b = p
    return (b - a, a)

def nonnegative(a, b):
    c = 2 * a + b

    if c == 0:
        return b >= 0

    if b > 0 and c > 0:
        return True

    if b < 0 and c < 0:
        return False

    if c > 0:
        return c * c >= 5 * b * b

    return 5 * b * b >= c * c

def geq(x, y):
    a = x[0] - y[0]
    b = x[1] - y[1]
    return nonnegative(a, b)

def solve_one(n):
    power = (1, 0)
    k = 0

    while True:
        nxt = mul_phi(power)
        if not geq((n, 0), nxt):
            break
        power = nxt
        k += 1

    max_k = k
    remainder = (n, 0)
    digits = []

    while remainder != (0, 0):
        if geq(remainder, power):
            digits.append('1')
            remainder = (
                remainder[0] - power[0],
                remainder[1] - power[1]
            )
        else:
            digits.append('0')

        power = div_phi(power)
        k -= 1

    s = ''.join(digits)
    split = max_k + 1

    left = s[:split].lstrip('0')
    right = s[split:].rstrip('0')

    if not left:
        left = '0'

    return left + ('.' + right if right else '')

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t = int(input())
    ans = [solve_one(int(input())) for _ in range(t)]

    sys.stdin = old_stdin
    return '\n'.join(ans)

# Provided sample
assert run(
    """5
1
2
3
100000
123
"""
) == (
    """1
10.01
100.01
101010001010100000100000.101000101000000010000001
10000000000.0000000001"""
), "sample 1"

# Minimum value and repeated equal values
assert run(
    """4
1
1
1
1
"""
) == (
    """1
1
1
1"""
), "minimum and repeated values"

# First values that require fractional powers
assert run(
    """2
2
3
"""
) == (
    """10.01
100.01"""
), "negative exponent boundary"

# Values with several separated one digits
assert run(
    """2
5
18
"""
) == (
    """1000.1001
1000000.000001"""
), "multiple separated digits"

# Maximum input value
assert run(
    """1
100000
"""
) == (
    """101010001010100000100000.101000101000000010000001"""
), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, 1, 1, 1`|`1, 1, 1, 1`| Giá trị tối thiểu và đầu vào bằng nhau lặp lại | 
|`2, 3`|`10.01, 100.01`| Trường hợp đầu tiên yêu cầu số mũ âm | 
|`5, 18`|`1000.1001, 1000000.000001`| Một số quyền hạn được lựa chọn riêng biệt | 
|`100000`|`101010001010100000100000.101000101000000010000001`| Đầu vào tối đa được phép và đầu ra phân đoạn dài | 

## Vỏ cạnh 

Với (n=1), công suất khả dụng lớn nhất là (\phi^0=1). Bước tham lam đầu tiên trừ chính xác (1), để lại cặp ((0,0)). Dãy số chỉ chứa`1`, do đó bộ định dạng sẽ in`1`không có dấu thập phân. 

Đối với (n=2), trước tiên thuật toán chọn (\phi^1), bỏ đi (2-\phi). Hai lũy thừa tiếp theo, (1) và (\phi^{-1}), đều quá lớn. Tại số mũ (-2), lũy thừa chính xác là (2-\phi), nên phần dư bằng 0. Đầu ra là`10.01`. Trường hợp này chứng minh tại sao số mũ âm là cần thiết. 

Với (n=3), lũy thừa được chọn đầu tiên là (\phi^2=1+\phi). Số dư là (2-\phi=\phi^{-2}), nên kết quả đầu ra là`100.01`. Lựa chọn tham lam cũng thể hiện tính bất biến kề cận, vì việc chọn số mũ (2) làm cho số mũ (1) không thể thực hiện được. 

Với (n=5), số mũ được chọn là (3,-1,-4). Giá trị tương ứng là 

[ 
\phi^3+\phi^{-1}+\phi^{-4}=5, 
] 

vì vậy đầu ra là`1000.1001`. Hai vị trí âm đã chọn được phân tách bằng hai số 0, cho thấy quy tắc không có số liền kề nào được áp dụng trên điểm cơ số giống như ở phía số nguyên. 

Với (n=123), câu trả lời là`10000000000.0000000001`. Điều này có nghĩa 

[ 
123=\phi^{10}+\phi^{-10}. 
] 

Hai số hạng đại số riêng lẻ đều vô tỷ, nhưng hệ số (\phi) của chúng triệt tiêu nhau một cách chính xác. Biểu diễn cặp số nguyên xử lý việc hủy bỏ này mà không bao giờ phụ thuộc vào độ chính xác của dấu phẩy động. 

Với (n=100000), thuật toán tiếp tục quá trình tương tự chỉ với vài chục vị trí và đạt chính xác về 0. Câu trả lời dài không phải là dấu hiệu của một thuật toán đắt tiền, bởi vì công việc tỷ lệ thuận với số chữ số thay vì giá trị số (100000).
