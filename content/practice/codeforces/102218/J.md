---
title: "CF 102218J - Chỉ là một nhiệm vụ dễ dàng"
description: "Chúng ta cần xác định, với mỗi ngày k từ 0 đến n - 1, có bao nhiêu cặp thứ tự (i, j) thỏa mãn i⋅j≡k(modn). Mỗi cặp như vậy đóng góp chính xác một đơn vị cho ngày k, do đó đầu ra yêu cầu chỉ đơn giản là số cặp tạo ra mỗi dư lượng modulo n."
date: "2026-08-20T03:33:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "J"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 440
verified: false
draft: false
---

[CF 102218J - Chỉ là một nhiệm vụ dễ dàng](https://codeforces.com/problemset/problem/102218/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xác định, mỗi ngày`k`từ`0`ĐẾN`n - 1`, có bao nhiêu cặp được đặt hàng`(i, j)`thỏa mãn 

i⋅j≡k(modn). 

Mỗi cặp như vậy đóng góp chính xác một đơn vị mỗi ngày`k`, do đó đầu ra cần thiết chỉ đơn giản là số cặp tạo ra mỗi modulo dư`n`. 

Một sự giải thích trực tiếp đưa ra một`n × n`modulo bảng nhân`n`. Quan sát đó rất hữu ích cho việc hiểu vấn đề, nhưng hạn chế`n <= 2.2 × 10^6`khiến cho việc xây dựng bảng đó là không thể. Có tới 

(2,2×10 6 ) 2 =4,84×10 12 

cặp, trong khi thời gian của bài toán ban đầu chỉ là 2,5 giây. Chúng ta cần một giải pháp có công gần tuyến tính trong`n`. 

Những trường hợp tế nhị nhất đều xuất phát từ thực tế là`0`cũng là một phần dư và phép nhân theo modulo của một hợp số hoạt động khác với phép nhân theo modulo của một số nguyên tố. Vì`n = 1`, chỉ có cặp`(0,0)`, vậy câu trả lời là`1`. Việc thực hiện bất cẩn chỉ lặp lại các dư lượng tích cực sẽ không tạo ra kết quả gì. 

Vì`n = 2`, các cặp là`(0,0)`,`(0,1)`,`(1,0)`,`(1,1)`. Ba sản phẩm dư lượng`0`và một tạo ra dư lượng`1`, cho```
31
```Một công thức vô tình giả định mọi dư lượng khác 0 có cùng số cách biểu diễn sẽ thất bại ở đây. 

Vì`n = 6`, câu trả lời bắt đầu bằng`15`ở dư lượng`0`, không`6`. Giá trị bằng 0 đếm tất cả các cặp có tích chia hết cho`6`và các mô đun tổng hợp tạo ra nhiều cặp như vậy. Đây chính xác là tình huống mà việc xử lý bài toán như số học modulo một số nguyên tố cho kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Tạo một mảng`n`bộ đếm, lặp lại trên mọi`i`và mọi`j`, tính toán`(i * j) % n`và tăng bộ đếm tương ứng. Điều này đúng vì mỗi cặp có thứ tự được xem xét chính xác một lần và đóng góp vào chính xác phần dư được chỉ định bởi bài toán. 

Vấn đề là số lượng hoạt động. Trong trường hợp lớn nhất có`n² = 4.84 × 10^12`cặp. Ngay cả một hằng số rất nhỏ trên mỗi cặp cũng sẽ vượt quá giới hạn thời gian. 

Quan sát hữu ích là ngừng sửa cả hai`i`Và`j`. Sửa chữa`i`và hỏi khi nào 
in≡a(mod) 
có giải pháp. 
hãy để 
g=gcd(i,n). 
Một tính chất tiêu chuẩn của sự đồng dư tuyến tính nói rằng`ij ≡ k (mod n)`có giải pháp chính xác khi`g`chia rẽ`k`. Khi điều kiện này được thoả mãn thì có chính xác`g`các giá trị khác nhau của`j`modulo`n`thỏa mãn sự đồng nhất. 

Vì vậy, một`i`đóng góp`gcd(i,n)`cặp để dư lượng`k`chính xác khi nào`gcd(i,n)`chia rẽ`k`. 

Bây giờ nhóm tất cả`i`có cùng gcd với`n`. Nếu 

gcd(i,n)=d, 

viết`i = d x`. Sau đó 

gcd(x,n/d)=1. 

Có chính xác 

φ(n/d) 

những giá trị như vậy của`i`, Ở đâu`φ`là hàm tổng Euler. Mỗi người đóng góp`d`giải pháp cho`j`, vậy là tất cả`i`với gcd bằng`d`đóng góp 

dφ(n/d) 

đến từng dư lượng`k`chia hết cho`d`. 

Do đó, 

c k ​ = d∣n d∣k ​ ∑ ​ dφ(n/d)= d∣gcd(k,n) ∑ ​ dφ(n/d). 

Công thức này thay đổi hoàn toàn vấn đề. Ta chỉ cần xét các ước của`n`. Với mỗi ước số`d`, tính toán 

w d ​ =dφ(n/d) 

và thêm`w_d`tới mọi bội số của`d`giữa những tàn dư`1,2,\ldots,n-1`. Dư lượng`0`chia hết cho mọi ước số nên nó nhận được mọi`w_d`riêng. 

Tổng số lần cập nhật là 

d∣n ∑ ​ d n ​ =n d∣n ∑ ​ d 1 ​ , 

đó là`O(n log log n)`và rất gần với tuyến tính đối với giới hạn đã cho. Chúng tôi cũng tránh xây dựng một sàng đầy đủ lên đến`n`, bởi vì chỉ`φ(n/d)`cho các ước của`n`là cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n²)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Yếu tố`n`vào quyền lực hàng đầu của nó. Chúng ta cần phân tích nhân tử vì nó cho phép chúng ta liệt kê mọi ước của`n`và tính tổng Euler cho`n / d`mà không cần xây dựng một kích thước-`n`mảng tổng thể. 
2. Tạo tất cả các ước của`n`từ hệ số nguyên tố của nó. chỉ có`τ(n)`trong số đó, rất nhỏ so với`n`vì`n <= 2.2 × 10^6`. 
3. Với mọi ước số`d`, tính toán 

w=dφ(n/d). 

Đây là tổng đóng góp của tất cả mọi người`i`thỏa mãn`gcd(i,n) = d`với mọi dư lượng chia hết cho`d`. 

1. Thêm`w`với mọi bội số dương của`d`dưới`n`. Các lượt truy cập vòng lặp`d, 2d, 3d, ...`, và mỗi số dư này đều chia hết cho`d`, khớp chính xác với điều kiện trong công thức. 
2. Thêm`w`ĐẾN`answer[0]`cũng vậy. Số 0 chia hết cho mọi số nguyên dương, nhưng vòng lặp bội số thông thường bắt đầu từ`d`không ghé thăm số không. 
3. Xuất mảng kết quả. Các giá trị có thể lớn như`n²`, do đó số nguyên Python tự nhiên cung cấp đủ độ chính xác. 

### Tại sao nó hoạt động 

Đối với một cố định`i`, sự phù hợp 
in≡k(mod n) 
có`gcd(i,n)`giải pháp cho`j`khi`gcd(i,n)`chia rẽ`k`, và không có giải pháp nào khác. Nhóm các giá trị của`i`qua`d = gcd(i,n)`, có`φ(n/d)`giá trị trong nhóm và mỗi người đều đóng góp`d`giải pháp. Vì vậy nhóm đó góp phần`d φ(n/d)`chính xác với số dư chia hết cho`d`. Thuật toán thực hiện chính xác các phép cộng đó cho mọi ước số`d`, bao gồm cả dư lượng đặc biệt`0`, vì vậy mỗi cặp được tính đúng một lần. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def factorize(n):    factors = []    x = n    p = 2
    while p * p <= x:        if x % p == 0:            e = 0            while x % p == 0:                x //= p                e += 1            factors.append((p, e))        p += 1 if p == 2 else 2
    if x > 1:        factors.append((x, 1))
    return factors

def get_divisors(factors):    divisors = [1]
    for p, e in factors:        old = divisors[:]        power = 1
        for _ in range(e):            power *= p            for d in old:                divisors.append(d * power)
    return divisors

def phi_from_factorization(x, factors):    result = x
    for p, _ in factors:        if x % p == 0:            result -= result // p
    return result

def compute(n):    factors = factorize(n)    divisors = get_divisors(factors)
    ans = [0] * n
    for d in divisors:        w = d * phi_from_factorization(n // d, factors)
        ans[0] += w
        for k in range(d, n, d):            ans[k] += w
    return ans

def solve():    n = int(input())    ans = compute(n)
    out = sys.stdout.buffer
    # Avoid constructing one enormous output string at once.    chunk = []    for x in ans:        chunk.append(str(x))        if len(chunk) == 100000:            out.write(("\n".join(chunk) + "\n").encode())            chunk.clear()
    if chunk:        out.write(("\n".join(chunk) + "\n").encode())

if __name__ == "__main__":    solve()
```Việc nhân tố hóa bắt đầu bằng`2`và sau đó chỉ kiểm tra các ứng cử viên lẻ. Từ`n`nhiều nhất là`2.2 × 10^6`, phép chia thử lên tới`sqrt(n)`là không tốn kém. 

Trình tạo số chia bắt đầu bằng`{1}`. Đối với mỗi quyền lực chính`p^e`, mọi ước số hiện có được kết hợp với`p`,`p²`, ...,`p^e`, tạo ra chính xác mọi ước số của`n`một lần. 

Đối với một số chia cụ thể`d`,`n // d`là mô đun xuất hiện bên trong tổng thể. Vì các thừa số nguyên tố của`n // d`phải là một trong những yếu tố hàng đầu`n`,`phi_from_factorization`có thể tính toán tổng số bằng cách sử dụng 

φ(x)=x p∣x ∏ ​ (1− p 1 ​ ). 

Vòng lặp bên trong bắt đầu tại`d`, không phải ở mức 0, vì số 0 được xử lý rõ ràng bằng`ans[0] += w`. Bắt đầu từ số 0 cũng hợp lệ, nhưng nó sẽ yêu cầu cấu trúc vòng lặp hơi khác một chút. 

Mảng câu trả lời chứa các số nguyên Python thông thường. Không cần xử lý tràn và điều này quan trọng vì tổng số cặp là`n²`, có thể ở xung quanh`4.84 × 10^12`. 

Đầu ra được viết thành từng đoạn 100.000 dòng. Điều này giữ cho chuỗi đầu ra tạm thời bị giới hạn thay vì xây dựng một chuỗi lớn có khả năng chứa mọi câu trả lời cùng một lúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`n = 6`Các ước số của`6`là`1, 2, 3, 6`. Đóng góp của họ là: 

1φ(6)=2, 
2φ(3)=4, 
3φ(2)=3, 
6φ(1)=6. 

Thuật toán cộng từng đóng góp vào 0 và vào tất cả các bội số dương của ước số của nó. 

| Số chia`d`|`φ(6/d)`| Sự đóng góp`dφ(6/d)`| Dư lượng tích cực được cập nhật | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 1, 2, 3, 4, 5 | 
| 2 | 2 | 4 | 2, 4 | 
| 3 | 1 | 3 | 3 | 
| 6 | 1 | 6 | không | 

Dư lượng không nhận được`2 + 4 + 3 + 6 = 15`. 

Mảng kết quả là```
1526562
```Ví dụ, dư lượng`4`chia hết cho`1`Và`2`, vì vậy nó nhận được`2 + 4 = 6`. Nó không chia được cho`3`hoặc`6`. 

### Ví dụ 2:`n = 5`Từ`5`là số nguyên tố nên ước số duy nhất của nó là`1`Và`5`. 

| Số chia`d`|`φ(5/d)`| Sự đóng góp`dφ(5/d)`| Dư lượng tích cực được cập nhật | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 1, 2, 3, 4 | 
| 5 | 1 | 5 | không | 

Phần dư bằng 0 nhận được cả hai khoản đóng góp, mang lại`9`. Mọi số dư khác 0 chỉ chia hết cho`1`, vậy mọi câu trả lời khác 0 đều là`4`. 

Đầu ra là```
94444
```Điều này minh họa tại sao các mô đun nguyên tố có hình dạng đặc biệt đơn giản, trong khi các mô đun tổng hợp yêu cầu tổng ước số đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log log n)`| Với mỗi ước số`d`của`n`, chúng tôi cập nhật khoảng`n/d`các vị trí. | 
| Không gian |`O(n)`| Mảng câu trả lời chứa`n`các giá trị nguyên. | 

Tổng số chia thỏa mãn 

d∣n ∑ ​ d n ​ =n n σ(n) ​ =O(nloglogn), 

vì vậy số lượng cập nhật mảng vẫn gần với tuyến tính. Việc tạo hệ số và ước số mất thời gian không đáng kể so với các cập nhật đó để có đầu vào tối đa. các`O(n)`mảng câu trả lời nằm trong giới hạn bộ nhớ 256 MB cho ràng buộc này. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io
# The functions below are the same computational functions used by the solution.
def factorize(n):    factors = []    x = n    p = 2
    while p * p <= x:        if x % p == 0:            e = 0            while x % p == 0:                x //= p                e += 1            factors.append((p, e))        p += 1 if p == 2 else 2
    if x > 1:        factors.append((x, 1))
    return factors

def get_divisors(factors):    divisors = [1]
    for p, e in factors:        old = divisors[:]        power = 1
        for _ in range(e):            power *= p            for d in old:                divisors.append(d * power)
    return divisors

def phi_from_factorization(x, factors):    result = x
    for p, _ in factors:        if x % p == 0:            result -= result // p
    return result

def compute(n):    factors = factorize(n)    divisors = get_divisors(factors)    ans = [0] * n
    for d in divisors:        w = d * phi_from_factorization(n // d, factors)
        ans[0] += w
        for k in range(d, n, d):            ans[k] += w
    return ans

def run(inp: str) -> str:    n = int(inp.strip())    ans = compute(n)    return "\n".join(map(str, ans)) + "\n"

# Provided sampleassert run("6") == "15\n2\n6\n5\n6\n2\n", "sample 1"
# Minimum sizeassert run("1") == "1\n", "n = 1"
# Small composite numberassert run("4") == "8\n4\n4\n4\n", "n = 4"
# Prime modulus, all nonzero residues have equal valuesassert run("5") == "9\n4\n4\n4\n4\n", "n = 5"
# Another composite case, useful for catching divisor/multiple errorsassert run("8") == "20\n4\n8\n4\n12\n4\n8\n4\n", "n = 8"

# Maximum-size structural test.# We do not materialize a second expected 2.2-million-line string.n = 2_200_000ans = compute(n)
assert len(ans) == n, "maximum n output length"assert sum(ans) == n * n, "every ordered pair must be counted exactly once"assert ans[0] == sum(    d * phi_from_factorization(n // d, factorize(n))    for d in get_divisors(factorize(n))), "zero residue"
```Thử nghiệm kích thước tối đa có chủ ý kiểm tra các đặc tính cấu trúc thay vì nhúng hàng triệu dòng đầu ra dự kiến. Danh tính`sum(ans) = n²`đặc biệt hữu ích vì mọi`n²`các cặp có thứ tự phải đóng góp vào đúng một dư lượng. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Kích thước tối thiểu và xử lý dư lượng bằng không | 
|`4`|`8, 4, 4, 4`| Mô đun tổng hợp và đóng góp của số chia lặp lại | 
|`5`|`9, 4, 4, 4, 4`| Mô đun nguyên tố và các phần dư khác không bằng nhau | 
|`8`|`20, 4, 8, 4, 12, 4, 8, 4`| Một số ước số lũy thừa nguyên tố và nhiều ranh giới | 
|`2_200_000`| Kiểm tra kết cấu | Kích thước đầu vào tối đa, tổng số cặp và hiệu suất | 

## Vỏ cạnh 

cho`n = 1`, cặp duy nhất là`(0,0)`. Tập số chia chỉ chứa`1`, và sự đóng góp của nó là 

1⋅φ(1)=1. 

Vòng lặp tích cực-nhiều không thực hiện cập nhật, trong khi`ans[0]`nhận được`1`. Đầu ra chính xác là`1`. 

Vì`n = 2`, các ước số là`1`Và`2`. Những đóng góp của họ là`1·φ(2)=1`Và`2·φ(1)=2`. Không nhận được`3`, trong khi dư lượng`1`chỉ nhận được sự đóng góp từ số chia`1`, cho`1`. Đầu ra là`3,1`, tính đúng ba cặp có tích chẵn. 

Vì`n = 5`, số chia`1`đóng góp`φ(5)=4`đến mọi số dư, trong khi số chia`5`đóng góp`5`chỉ về không. Như vậy câu trả lời là`9,4,4,4,4`. Điều này dễ mắc phải một lỗi khi hành vi đặc biệt của dư lượng bằng 0 bị lãng quên. 

Vì`n = 6`, số chia`3`đóng góp`3`đến dư lượng`0`Và`3`, trong khi số chia`2`đóng góp`4`ĐẾN`0`,`2`, Và`4`. Dư lượng`4`do đó nhận được`2 + 4 = 6`, trong khi dư lượng`5`chỉ nhận được`2`. Điều này xác nhận rằng thuật toán kiểm tra khả năng chia hết cho số chia thay vì chỉ kiểm tra xem phần dư có chia sẻ thừa số nguyên tố với nó hay không. 

Để có giá trị lớn nhất`n = 2,200,000`, thuật toán không bao giờ xây dựng`n × n`bảng nhân. Nó chỉ xử lý các ước của`n`và bội số của chúng, do đó khối lượng công việc vẫn gần như tuyến tính trong`n`. Các giá trị đầu ra nhiều nhất vẫn là tổng số cặp có thứ tự,`n²`và số nguyên Python xử lý phạm vi đó mà không bị tràn.
