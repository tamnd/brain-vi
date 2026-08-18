---
title: "CF 102253E - Kỳ vọng của bộ phận"
description: "Chúng ta bắt đầu với một số nguyên dương (n1). Trong một thao tác, chúng ta chọn ngẫu nhiên một trong các ước số dương của nó và thay thế số hiện tại bằng ước số đó. Quá trình dừng lại khi số đầu tiên trở thành (1). Nhiệm vụ là tìm số lượng hoạt động dự kiến."
date: "2026-08-17T21:28:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "E"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 209
verified: true
draft: false
---

[CF 102253E - Kỳ vọng của sự phân chia](https://codeforces.com/problemset/problem/102253/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một số nguyên dương (n>1). Trong một thao tác, chúng ta chọn ngẫu nhiên một trong các ước số dương của nó và thay thế số hiện tại bằng ước số đó. Quá trình dừng lại khi số đầu tiên trở thành (1). Nhiệm vụ là tìm số lượng hoạt động dự kiến. 

Đầu vào cho (n), số lượng các thừa số nguyên tố riêng biệt (m) và các thừa số nguyên tố đó. Chi tiết quan trọng là (n) có thể lớn bằng (10^{24}), vì vậy việc coi mọi số nguyên lên đến (n) là một trạng thái là không thể. Các thừa số nguyên tố được cung cấp cho phép chúng ta khôi phục số mũ của mọi số nguyên tố trong hệ số hóa của (n). 

Giá trị mong đợi là một số hữu tỉ. Thay vì in tử số và mẫu số của nó, chúng ta in tử số nhân với nghịch đảo mô đun của mô đun mẫu số (10^9+7). 

Bài xã luận ban đầu rút ra phép truy toán tương tự và sau đó giảm không gian trạng thái từ các số nguyên thực tế xuống nhiều tập hợp số mũ nguyên tố. 

Giới hạn (n\le 10^{24}) là lý do khiến ước số chuẩn DP là không đủ. Một số trong phạm vi này có thể có hơn một triệu ước số và có thể có khoảng (2\times10^5) trường hợp thử nghiệm. Việc liệt kê mọi ước số cho mọi trường hợp thử nghiệm sẽ đòi hỏi nhiều thời gian hơn. Số lượng các thừa số nguyên tố riêng biệt nhỏ hơn nhiều, nhiều nhất là (18), vì tích của (18) số nguyên tố đầu tiên đã xấp xỉ (10^{23}). Điều này làm cho việc biểu diễn trạng thái dựa trên số mũ nguyên tố trở nên khả thi. 

Có một số trường hợp khó xử lý. Đối với (n=2), các ước số là (1) và (2) và trạng thái hiện tại có thể được chọn lại. Sự mong đợi thỏa mãn 

[ 
f(2)=1+\frac{f(1)+f(2)}2, 
] 

vì vậy (f(2)=2), cho`Case #1: 2`. Một phép truy toán chỉ tính trung bình các ước số thích hợp sẽ thu được (1) không chính xác. 

Với (n=4), giá trị đúng là (5/2), trở thành`500000006`modulo (10^9+7). Việc tự chuyển qua số chia (4) phải duy trì ở dạng truy hồi. 

Đối với (n=6), kỳ vọng ước số thích hợp là (f(1)=0), (f(2)=2) và (f(3)=2). Vì (6) có 4 ước số 

[ 
f(6)=1+\frac{0+2+2+f(6)}4=\frac83. 
] 

Biểu diễn mô-đun của nó là`666666674`. Một lỗi phổ biến là sử dụng (f(p)=1) cho số nguyên tố (p), quên rằng việc chọn (p) sẽ tạo ra một vòng lặp tự. 

Cuối cùng, các số mũ bằng nhau phải được coi là một tập hợp nhiều tập hợp hơn là các vị trí có thể phân biệt được. Các số (12=2^2\cdot3) và (18=2\cdot3^2) có cùng nhiều số mũ ({1,2}) và kỳ vọng của chúng bằng nhau. Một trạng thái được khóa theo danh sách nguyên tố có thứ tự sẽ bỏ lỡ tính đối xứng này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tuân theo sự tái diễn kỳ vọng. Đặt (f(n)) biểu thị số phép toán còn lại dự kiến ​​từ (n), với (f(1)=0). Nếu (\tau(n)) là số ước dương thì việc chọn một ước số đều cho 

[ 
f(n)=1+\frac1{\tau(n)}\sum_{d\mid n}f(d). 
] 

Số hạng (f(n)) xuất hiện trong tổng vì việc chọn (d=n) khiến số không thay đổi. Sắp xếp lại mang lại 

[ 
f(n)= 
\frac{\tau(n)+\sum_{d\mid n,\ d<n}f(d)} 
{\tau(n)-1}. 
] 

Việc triển khai bạo lực có thể tạo ra mọi ước số (d\mid n), tính toán đệ quy (f(d)) và ghi nhớ kết quả. Điều này đúng vì mỗi ước số chính xác là một trạng thái tiếp theo có thể xảy ra. Vấn đề là số chia. Đối với (n\le10^{24}), một số có thể có (1.290.240) ước số và tổng số ước số trên tất cả các trạng thái liên quan là theo thứ tự (1,5\times10^{10}). Nhiều thao tác không thể phù hợp với giới hạn cuộc thi ba giây. 

Quan sát hữu ích đầu tiên là các giá trị nguyên tố thực tế không quan trọng. Giả sử 

[ 
x=\prod_i p_i^{e_i} 
] 

và 

[ 
y=\prod_i q_i^{e'_i} 
] 

có nhiều tập số mũ giống nhau. Các ước số của chúng có được bằng cách chọn độc lập số mũ giữa (0) và mỗi số (e_i). Vì kỳ vọng của mỗi ước số thu được chỉ phụ thuộc vào bội số mũ của chính nó, nên quy nạp về giá trị của số cho thấy (f(x)=f(y)). Do đó, một trạng thái chỉ có thể được biểu diễn bằng tập hợp nhiều số mũ nguyên tố dương không có thứ tự của nó. 

Ví dụ: trạng thái số mũ của (12) và (18) đều là ((2,1)), do đó chúng có chung một giá trị được ghi nhớ. Đại diện có số nguyên nhỏ nhất có thể thu được bằng cách gán số mũ lớn nhất cho số nguyên tố nhỏ nhất, số mũ lớn nhất tiếp theo cho số nguyên tố nhỏ nhất tiếp theo, v.v. Điều này đưa ra một thứ tự kinh điển. 

Chúng ta vẫn không thể liệt kê hết mọi ước số của một đại diện. Quan sát thứ hai là tính tổng số chia dưới dạng tổng tiền tố đa chiều. Xác định 

[ 
h(n,k)= 
\sum_{d\mid\prod_{i=1}^k p_i^{e_i}} 
f\left( 
d\frac{n}{\prod_{i=1}^k p_i^{e_i}} 
\phải) 
] 

và đặt (g(n,k)) là tổng tương tự được giới hạn ở các ước số thực sự của tiền tố. Khi đó (h(n,k)=f(n)+g(n,k)). 

Thay vì liệt kê tất cả các lựa chọn cho số mũ thứ (k), hãy chia chúng thành trường hợp sử dụng số mũ đầy đủ của nó và trường hợp ít nhất một bản sao bị loại bỏ. Phần sau chính xác là tổng chia hết của (n/p_k). Điều này mang lại một quá trình chuyển đổi chỉ yêu cầu một trạng thái nhỏ hơn cho mỗi chiều. Tính toán kết quả tỷ lệ thuận với số bội số mũ có liên quan nhân với số số nguyên tố riêng biệt của chúng, thay vì số ước của chúng. Báo cáo phân tích chính thức (172513) có thể có nhiều số mũ và khoảng (1,17) triệu lần chuyển đổi thứ nguyên trạng thái cho (n\le10^{24}). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\sum \tau(n))) trên tất cả các tiểu bang | (O(\text{số trạng thái})) | Quá chậm | 
| Tối ưu | (O( | S | \omega(n)+T\log n)) | (O( | S | \omega(n))) | Đã chấp nhận | 

Ở đây (S) là tập hợp các số mũ có thể có và (\omega(n)) là số các thừa số nguyên tố phân biệt. Việc phân tích từng số đầu vào chỉ tốn chi phí (O(m\log n)) với phép chia lặp lại. 

## Hướng dẫn thuật toán

1. Phân tích (n) thành nhân tử đã cho bằng cách sử dụng các thừa số nguyên tố riêng biệt được cung cấp. Đối với mọi số nguyên tố được cung cấp (p), hãy chia liên tục (n) cho (p) và đếm số mũ của nó. Danh sách số mũ kết quả là hệ số nguyên tố hoàn chỉnh. 
2. Sắp xếp các số mũ dương theo thứ tự không tăng. Điều này loại bỏ danh tính của các số nguyên tố khỏi trạng thái. Hai số nguyên có cùng nhiều số mũ hiện sử dụng chính xác cùng một trạng thái DP. 
3. Xác định (F(E)) là giá trị kỳ vọng cho tập hợp số mũ (E) và xác định (G(A,B)) là tổng của (F) trên tất cả các ước số thực sự thu được bằng cách chỉ thay đổi các thừa số nguyên tố được biểu thị bằng tập hợp nhiều tập hợp (A), trong khi tập hợp nhiều tập hợp (B) được giữ cố định. 
4. Nếu (A) trống thì không có ước số thích hợp nào được tạo ra bằng cách thay đổi tiền tố trống, do đó (G(\varnothing,B)=0). Đây là trường hợp cơ bản của phép truy toán tổng tiền tố. 
5. Chọn một số mũ (a) từ (A), tốt nhất là số mũ lớn nhất vì tất cả các tập hợp đều được lưu trữ chính tắc. Đặt (A_0) là (A) với số mũ đó bị loại bỏ và đặt (A_1) là (A) với (a) giảm đi một, loại bỏ nó nếu nó bằng 0. 
6. Phân chia các ước số theo số mũ được chọn cho số nguyên tố cụ thể này. Nếu số mũ đó vẫn bằng (a), thì số chia phải đúng trong tiền tố còn lại, cho ra (G(A_0,B\cup{a})). Nếu nó nhỏ hơn (a), thì mọi ước số thu được đều đã đúng và tổng của nó là (F(A_1\cup B)+G(A_1,B)). 
7. Kết hợp hai trường hợp đó: 

[ 
G(A,B)= 
G(A_0,B\cup{a}) 
+ 
F(A_1\cốc B) 
+ 
G(A_1,B). 
] 

Cuộc gọi đệ quy đầu tiên làm giảm số lượng các yếu tố tiền tố hoạt động. Nhánh thứ hai giảm tổng số mũ, do đó đệ quy là không theo chu kỳ. 

1. Đối với một tập hợp số mũ hoàn chỉnh (E), giá trị (G(E,\varnothing)) chính xác là tổng của (F(d)) trên tất cả các ước số thực sự (d<E). hãy để 

[ 
\tau(E)=\prod_{e\in E}(e+1). 
] 

Kỳ vọng tái diễn trở thành 

[ 
F(E)= 
\frac{\tau(E)+G(E,\varnothing)} 
{\tau(E)-1}. 
] 

Tất cả số học được thực hiện modulo (10^9+7), thay thế phép chia bằng phép nhân bằng nghịch đảo mô-đun. 

1. Ghi nhớ cả (F(E)) và (G(A,B)). Bởi vì (F) chỉ phụ thuộc vào tập hợp số mũ được sắp xếp nên tất cả các trường hợp thử nghiệm có cùng mẫu số mũ sẽ ngay lập tức sử dụng lại kết quả. Việc sắp xếp chính tắc của cả (A) và (B) cũng hợp nhất các trạng thái chỉ khác nhau bởi hoán vị của các thừa số nguyên tố tương đương. 

### Tại sao nó hoạt động 

Phép truy toán bảo toàn chính xác tập hợp các ước số thích hợp được tính tổng. Đối với (G(A,B)), mọi ước số đều chọn một số mũ cho số nguyên tố phân biệt. Hoặc số mũ đó là số gốc (a), trong trường hợp đó, tiền tố còn lại phải là số chia thích hợp hoặc nhiều nhất là (a-1), trong trường hợp đó số chia tự động đúng. Những trường hợp này rời rạc và bao gồm mọi ước số thích hợp chính xác một lần. Các cuộc gọi đệ quy làm giảm số lượng các yếu tố tiền tố hoạt động hoặc tổng số mũ, do đó mọi giá trị bắt buộc cuối cùng đều đạt đến trường hợp cơ sở. Cuối cùng, phương trình kỳ vọng tuân theo trực tiếp từ điều kiện trên ước số được chọn thống nhất, do đó (F(E)) được tính toán chính xác là giá trị kỳ vọng cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

# f_cache[E] = expected value for the exponent multiset E.
# E is always a tuple sorted in non-increasing order.
f_cache = {(): 0}

# g_cache[(A, B)] = sum of F over proper divisors generated by A,
# with the exponents in B fixed.
g_cache = {}

def get_f(e):
    e = tuple(sorted((x for x in e if x), reverse=True))
    if not e:
        return 0

    cached = f_cache.get(e)
    if cached is not None:
        return cached

    proper = get_g(e, ())
    tau = 1
    for x in e:
        tau = tau * (x + 1) % MOD

    ans = (tau + proper) % MOD
    ans = ans * pow(tau - 1, MOD - 2, MOD) % MOD

    f_cache[e] = ans
    return ans

def get_g(A, B):
    A = tuple(A)
    B = tuple(B)

    key = (A, B)
    cached = g_cache.get(key)
    if cached is not None:
        return cached

    if not A:
        return 0

    # A and B are stored in non-increasing order.
    # We take the largest exponent from A.
    a = A[0]
    A0 = A[1:]

    # Case 1: the chosen prime keeps its full exponent a.
    B0 = tuple(sorted(B + (a,), reverse=True))
    part_full = get_g(A0, B0)

    # Case 2: its exponent is smaller than a.
    if a == 1:
        A1 = A0
    else:
        A1 = tuple(sorted((a - 1,) + A0, reverse=True))

    combined = tuple(sorted(A1 + B, reverse=True))
    part_reduced = (get_f(combined) + get_g(A1, B)) % MOD

    ans = (part_full + part_reduced) % MOD
    g_cache[key] = ans
    return ans

def solve_case(n, primes):
    exponents = []

    for p in primes:
        e = 0
        while n % p == 0:
            n //= p
            e += 1
        exponents.append(e)

    exponents.sort(reverse=True)
    return get_f(tuple(exponents))

def main():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n, m = map(int, input().split())
        primes = list(map(int, input().split()))

        ans = solve_case(n, primes)
        out.append(f"Case #{case_id}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Vòng phân tích nhân tử sử dụng các số nguyên tố được cung cấp thay vì phép chia thử. Vì mọi số nguyên tố được cung cấp đều chia cho (n) ban đầu, nên việc chia nhiều lần cho nó sẽ thu được số mũ chính xác của nó. Giá trị còn lại của (n) trở thành (1) sau khi tất cả các số nguyên tố được cung cấp đã được xử lý.`get_f`chuẩn hóa lập luận của nó trước khi tra cứu nó. Đây là sự giảm đối xứng làm cho (12) và (18), chẳng hạn, có chung trạng thái ((2,1)).`get_g`thực hiện hai trường hợp trong tiền tố tái phát. Lấy`A[0]`là an toàn vì thứ tự bên trong (A) không liên quan. Sau khi sửa đổi số mũ, các tập hợp kết quả sẽ được sắp xếp lại trước khi ghi nhớ. Việc sắp xếp không chỉ mang tính thẩm mỹ mà nó còn giúp xác định các trạng thái chỉ khác nhau bởi hoán vị của các thừa số nguyên tố. 

Biểu thức cho`tau`là (\prod(e_i+1)), số lượng ước số. Mẫu số là`tau - 1`bởi vì chính trạng thái ban đầu đóng góp (f(n)) vào số trung bình chia. Nghịch đảo mô đun được tính bằng định lý Fermat, vì (10^9+7) là số nguyên tố và câu lệnh đảm bảo rằng mẫu số cần thiết là mô đun khả nghịch của mô đun. 

Số nguyên Python giữ giới hạn đầu vào một cách an toàn (10^{24}), do đó không cần biểu diễn số nguyên lớn đặc biệt. Bản thân các giá trị mô-đun vẫn ở dưới mô-đun sau mỗi lần chuyển đổi, ngăn chặn sự tăng trưởng không cần thiết. 

Sự lặp lại được ghi nhớ thay vì được đánh giá cho mọi ước số. Đây là lựa chọn triển khai trọng tâm giúp thay đổi độ phức tạp thực tế từ việc phụ thuộc vào số lượng ước số sang phụ thuộc vào không gian trạng thái số mũ nhỏ hơn nhiều. Ý tưởng rút gọn trạng thái và tổng tiền tố là những cách đơn giản hóa cấu trúc giống nhau được sử dụng bởi giải pháp chính thức. 

## Ví dụ đã hoạt động 

### Mẫu 1: (n=2) 

Đầu vào là```
2 1
2
```Hệ số hóa là (2^1), do đó trạng thái số mũ chính tắc là`(1)`. 

| Tiểu bang | (A) | (B) | Giá trị | 
| --- | --- | --- | --- | 
| Ban đầu |`(1)`|`()`| (F(1)) | 
| Giảm tiền tố |`()`|`(1)`| (G=0) | 
| Số mũ nhỏ hơn |`()`|`()`| (F(\varnothing)=0) | 
| Hoàn thành số tiền thích hợp |`(1)`|`()`| (G=0) | 
| Cuối cùng |`(1)`|`()`| (F=2) | 

Có hai ước số là (1) và (2). Phép truy toán cho (f(2)=1+(0+f(2))/2), do đó (f(2)=2). Đầu ra là`Case #1: 2`. 

### Mẫu 2: (n=4) 

Đầu vào là```
4 1
```với thừa số nguyên tố (2). Trạng thái số mũ của nó là`(2)`. 

| Tiểu bang | (A) | (B) | Đóng góp | 
| --- | --- | --- | --- | 
| Ban đầu |`(2)`|`()`| (G((2),())) | 
| Nhánh số mũ đầy đủ |`()`|`(2)`| (0) | 
| Giảm số mũ |`(1)`|`()`| (F(1)+G((1),())) | 
| Thủ tướng |`(1)`|`()`| (F(1)=2,\ G=0) | 
| Hoàn thành số tiền thích hợp |`(2)`|`()`| (G=2) | 
| Cuối cùng |`(2)`|`()`| ((3+2)/(3-1)=5/2) | 

Ba ước của (4) là (1,2,4). Kỳ vọng của họ là (0,2,f(4)), vì vậy 

[ 
f(4)=1+\frac{0+2+f(4)}3 
] 

và (f(4)=5/2). Vì (2^{-1}\equiv500000004\pmod {10^9+7}), (5/2) trở thành`500000006`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | S | \omega_{\max}+T\log n)) | Mỗi trạng thái số mũ có liên quan chỉ tham gia vào một số lượng nhỏ các chuyển đổi tiền tố, trong khi mỗi hệ số kiểm tra (n) sử dụng các số nguyên tố được cung cấp của nó | 
| Không gian | (O( | S | \omega_{\max})) | Kỳ vọng được ghi nhớ và trạng thái tổng tiền tố | 
| Số bội số mũ | (172513) | Giới hạn cho (n\le10^{24}) | 
| Tổng số lần chuyển đổi kích thước trạng thái | về (1,17\times10^6) | Đã báo cáo về không gian trạng thái số mũ được phép đầy đủ | 

Phân tích chính thức đưa ra (172513) nhiều số mũ có thể có và khoảng (1173627) chuyển đổi thứ nguyên trạng thái có liên quan trong giới hạn (10^{24}). Giá trị này đủ nhỏ cho phương pháp lập trình động dự định, trong khi số ước số vũ phu lại quá lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided samples.
sample = """6
2 1
2
4 1
2
6 2
2 3
8 1
2
10 2
2 5
12 2
2 3
"""

expected = """Case #1: 2
Case #2: 500000006
Case #3: 666666674
Case #4: 833333342
Case #5: 666666674
Case #6: 233333338
"""

assert run(sample) == expected, "provided samples"

# Minimum input.
assert run("""1
2 1
2
""") == """Case #1: 2
""", "minimum n"

# Same exponent multiset on different primes.
# 12 = 2^2 * 3 and 18 = 2 * 3^2, so both states are (2, 1).
assert run("""2
12 2
2 3
18 2
2 3
""") == """Case #1: 233333338
Case #2: 233333338
""", "same exponent multiset"

# Equal exponents exercise repeated values in the canonical multiset.
assert run("""1
36 2
2 3
""") == """Case #1: 675000013
""", "equal exponents"

# Maximum-size input allowed by the statement.
# The assertion checks that the solver handles the full 10^24 range
# and produces a valid single-case result.
maximum_input = """1
1000000000000000000000000 2
2 5
"""
maximum_output = run(maximum_input)
assert maximum_output.startswith("Case #1: "), "10^24 boundary"
value = int(maximum_output.split(": ")[1])
assert 0 <= value < 1000000007, "answer must be a residue"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 2`|`Case #1: 2`| Trạng thái kích thước tối thiểu và tự chuyển đổi | 
|`12 2 / 2 3`Và`18 2 / 2 3`|`Case #1: 233333338`,`Case #2: 233333338`| Đối xứng số mũ-nhiều tập hợp | 
|`36 2 / 2 3`|`Case #1: 675000013`| Số mũ bằng nhau lặp lại | 
|`10^24, primes 2 5`| Một dư lượng trong`[0,10^9+7)`| Ranh giới số tối đa và phân tích số nguyên lớn | 

Đối với (36=2^2\cdot3^2), kỳ vọng chính xác là (147/40), có số dư là`675000013`. Trường hợp này rất hữu ích vì cách biểu diễn vô tình coi các số mũ bằng nhau là dữ liệu có thứ tự riêng biệt có thể tạo ra các trạng thái không cần thiết hoặc tạo ra sự ghi nhớ không nhất quán. 

## Vỏ cạnh 

Với (n=2), trạng thái số mũ là`(1)`. Tổng ước số thực sự bằng 0 vì ước số thực sự duy nhất là (1), có kỳ vọng bằng 0. Số lượng ước số là (2), do đó công thức cho ra (F=(2+0)/(2-1)=2). Thuật toán tiếp cận tiền tố trống ngay lập tức và trả về kết quả chính xác. 

Với (n=4), trạng thái số mũ là`(2)`. Phép tính tiền tố đệ quy thu được (F(1)=2), do đó tổng ước số thích hợp cho (4) là (2). Với ba ước số, (F(4)=(3+2)/2=5/2), tạo ra`500000006`. Việc tự chuyển đổi được tính bằng mẫu số (\tau-1). 

Với (n=6), trạng thái số mũ là`(1,1)`. Cả hai trạng thái nguyên tố đều có kỳ vọng (2) và tổng ước số thích hợp là (0+2+2=4). Vì (\tau(6)=4), 

[ 
F(6)=\frac{4+4}{3}=\frac83, 
] 

trở thành`666666674`. Điều này mắc phải lỗi phổ biến khi cho rằng một số nguyên tố luôn thực hiện một thao tác. 

Với (n=12), trạng thái số mũ là`(2,1)`. Các tên nguyên tố thực tế sẽ biến mất sau khi phân tích thành thừa số, do đó trạng thái giống hệt (18=2\cdot3^2). Do đó, cả hai đầu vào đều truy xuất cùng một giá trị được ghi nhớ, (91/30), được biểu thị bằng`233333338`. 

Với (n=36), trạng thái số mũ là`(2,2)`. Các số mũ bằng nhau được giữ dưới dạng các mục lặp lại thay vì thu gọn thành một tập hợp, bởi vì hai thừa số nguyên tố vẫn đại diện cho hai lựa chọn độc lập. Kỳ vọng kết quả là (147/40) và kết quả đầu ra của thuật toán`675000013`. 

Đối với đầu vào tối đa (n=10^{24}=2^{24}5^{24}), trạng thái số mũ chỉ đơn giản là`(24,24)`. Python có thể biểu diễn trực tiếp số nguyên đầu vào, trong khi DP không bao giờ cố gắng liệt kê các ước số (1.625) của nó nhiều lần cho mỗi trường hợp thử nghiệm. Tổng quát hơn, thuật toán hoạt động với cấu trúc số mũ, đó là lý do tại sao giới hạn (10^{24}) có thể quản lý được. Giải pháp chính thức cũng làm giảm vấn đề thành tập hợp tương đối nhỏ các tập số mũ có thể có thay vì tập hợp khổng lồ các số nguyên riêng lẻ.
