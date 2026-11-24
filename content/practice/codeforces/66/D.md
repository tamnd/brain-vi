---
title: "CF 66D - Petya và những người bạn"
description: "Chúng ta cần xây dựng n số nguyên dương riêng biệt có hai thuộc tính đồng thời. Đầu tiên, mọi cặp số phải có ước chung lớn hơn 1. Nói cách khác, với mọi cặp (ai, aj), gcd của chúng không thể bằng 1. Thứ hai, gcd của toàn bộ tập hợp phải bằng 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 66
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 61 (Div. 2)"
rating: 1700
weight: 66
solve_time_s: 117
verified: false
draft: false
---

[CF 66D - Petya và những người bạn của anh ấy](https://codeforces.com/problemset/problem/66/D) 

**Đánh giá:** 1700 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng`n`các số nguyên dương phân biệt có hai thuộc tính đồng thời. 

Đầu tiên, mỗi cặp số phải có ước chung lớn hơn`1`. Nói cách khác, với mỗi cặp`(ai, aj)`, gcd của họ không thể bằng`1`. 

Thứ hai, gcd của toàn bộ tập hợp phải bằng`1`. Không có số nguyên tố nào được phép chia tất cả các số lại với nhau. 

Thoạt nhìn những điều kiện này có vẻ mâu thuẫn. Các gcd theo cặp phải luôn không tầm thường, tuy nhiên trên toàn cầu, các con số không thể có chung một yếu tố. Nhiệm vụ hoàn toàn mang tính xây dựng: chúng ta chỉ cần in bất kỳ tập hợp lệ nào hoặc`-1`nếu tập hợp đó không tồn tại. 

Đầu vào chứa một số nguyên duy nhất`n`, số bạn bè và số số nguyên mà chúng ta phải xây dựng. Từ`n ≤ 50`, thách thức không phải là hiệu suất mà là tìm ra cấu trúc toán học phù hợp. Các số đầu ra có thể chứa tối đa 100 chữ số, vì vậy cho phép số nguyên rất lớn. 

Phần nguy hiểm nhất của vấn đề này là hiểu sai sự khác biệt giữa gcd theo cặp và gcd chung. 

Ví dụ: các số:```
6 10 15
```hoạt động hoàn hảo vì:```
gcd(6,10)=2
gcd(6,15)=3
gcd(10,15)=5
gcd(6,10,15)=1
```Một cách tiếp cận bất cẩn có thể cố gắng biến tất cả các số thành bội số của một số giá trị cố định như`2`. Điều đó thỏa mãn tất cả các điều kiện gcd theo cặp, nhưng sau đó gcd của toàn bộ mảng ít nhất cũng bằng`2`, vi phạm yêu cầu thứ hai. 

Một trường hợp cạnh tinh tế khác là`n = 2`. 

Giả sử chúng ta cố gắng xây dựng hai số`a`Và`b`. điều kiện`gcd(a,b) ≠ 1`đã có nghĩa là gcd chung của họ có giá trị lớn hơn`1`. Nhưng đối với hai số:```
gcd(a,b) = gcd(a,b)
```gcd theo cặp và gcd toàn cục có cùng số lượng. Vì vậy, nếu gcd theo cặp lớn hơn`1`, gcd của mọi số không thể bằng`1`. Không có giải pháp tồn tại. 

Việc triển khai ngây thơ có thể bỏ lỡ điều này và in một cái gì đó như:```
6
10
```nhưng sau đó:```
gcd(6,10)=2
gcd(6,10)=2
```Gcd toàn cầu thì không`1`. 

Một lỗi dễ mắc phải khác là tạo ra các số trùng lặp. Nếu chúng ta sử dụng một cách xây dựng đối xứng một cách bất cẩn, hai vị trí có thể bằng nhau, vi phạm tính phân biệt mặc dù các điều kiện gcd vẫn đúng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra các mảng ứng cử viên và kiểm tra xem chúng có đáp ứng mọi điều kiện hay không. 

Đối với mỗi tập ứng cử viên, chúng ta sẽ kiểm tra tất cả các gcd theo cặp và cũng tính gcd của toàn bộ tập hợp. Kiểm tra chi phí theo cặp`O(n²)`tính toán gcd. Từ`n`chỉ là`50`, bản thân việc xác nhận là rẻ. 

Vấn đề thực sự là không gian tìm kiếm. Ngay cả khi chúng ta chỉ thử những con số lên đến`10^6`, số lượng mảng riêng biệt có thể lớn về mặt thiên văn. Việc tạo ngẫu nhiên là không đáng tin cậy vì các ràng buộc kéo theo các hướng ngược nhau. Dễ dàng tạo ra các gcd theo cặp không tầm thường, làm cho tổng gcd bằng nhau`1`thì dễ dàng, nhưng việc thỏa mãn cả hai cùng một lúc lại bị hạn chế một cách đáng ngạc nhiên. 

Quan sát quan trọng là các số nguyên tố cho phép chúng ta kiểm soát các mối quan hệ gcd một cách chính xác. 

Giả sử chúng ta lấy một số số nguyên tố phân biệt:```
p1, p2, p3, ..., pn
```Bây giờ xác định:```
ai = product of all primes except pi
```Ví dụ với số nguyên tố`2,3,5`:```
a1 = 3*5 = 15
a2 = 2*5 = 10
a3 = 2*3 = 6
```Hai số bất kỳ có chung tất cả các số nguyên tố, ngoại trừ một số nguyên tố, vì vậy gcd của chúng chắc chắn lớn hơn`1`. 

Đồng thời, không có số nguyên tố nào xuất hiện trong mọi số. Xuất sắc`pi`bị thiếu từ`ai`. Vì mọi số nguyên tố đều vắng mặt ở đâu đó nên gcd của toàn bộ mảng sẽ trở thành`1`. 

Việc xây dựng này giải quyết vấn đề một cách rõ ràng. 

Vấn đề duy nhất còn lại là`n = 2`. 

Với hai số:```
a1 = p2
a2 = p1
```gcd của họ trở thành`1`, vi phạm điều kiện từng cặp. Về cơ bản hơn, không có giải pháp hợp lệ nào tồn tại cho`n = 2`, như đã thảo luận trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Nếu`n == 2`, in`-1`. 

Hai số không thể đồng thời có gcd theo cặp lớn hơn`1`và tổng gcd bằng`1`, vì cả hai điều kiện đều đề cập đến cùng một gcd. 
3. Tạo cái đầu tiên`n`số nguyên tố. 

Chúng ta cần các số nguyên tố riêng biệt để mỗi số có thể loại trừ chính xác một số nguyên tố. 
4. Tính tích của tất cả các số nguyên tố được tạo. 

Đặt giá trị này là:```
P = p1 * p2 * ... * pn
```5. Đối với mọi chỉ số`i`, xây dựng:```
ai = P / pi
```Mỗi số chứa mọi số nguyên tố ngoại trừ một số nguyên tố còn thiếu duy nhất. 
6. In tất cả các số đã xây dựng. 

Tại sao nó hoạt động: 

Mỗi cặp số ít nhất có chung`n - 2`số nguyên tố. Từ`n ≥ 3`, luôn luôn tồn tại ít nhất một số nguyên tố chung, vì vậy mọi gcd theo cặp đều lớn hơn`1`. 

Đối với gcd của tất cả các số, hãy xem xét bất kỳ số nguyên tố nào`pi`. Con số`ai`không chứa`pi`, Vì thế`pi`không thể chia gcd của toàn bộ mảng. Điều này đúng với mọi số nguyên tố trong cách xây dựng, nghĩa là không có số nguyên tố nào chia hết cho tất cả các số. Gcd của toàn bộ là chính xác`1`. 

Sự khác biệt cũng tự động theo sau. Mỗi số bỏ qua một số nguyên tố khác nhau nên không có hai tích nào bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate_primes(cnt):
    primes = []
    x = 2

    while len(primes) < cnt:
        ok = True

        for p in primes:
            if p * p > x:
                break
            if x % p == 0:
                ok = False
                break

        if ok:
            primes.append(x)

        x += 1

    return primes

def solve():
    n = int(input())

    if n == 2:
        print(-1)
        return

    primes = generate_primes(n)

    total = 1
    for p in primes:
        total *= p

    for p in primes:
        print(total // p)

solve()
```Phần đầu tiên xử lý trường hợp không thể`n = 2`. Điều này phải được kiểm tra trước khi xây dựng vì không có cặp hợp lệ nào tồn tại. 

các`generate_primes`hàm tạo ra cái đầu tiên`n`số nguyên tố sử dụng phép chia thử. Từ`n ≤ 50`, thế là quá đủ nhanh rồi. Số nguyên tố lớn nhất cần thiết là nhỏ, vì vậy ngay cả việc triển khai đơn giản cũng có thể hoạt động thoải mái trong giới hạn. 

Sau khi tạo các số nguyên tố, mã sẽ nhân chúng thành một sản phẩm duy nhất`total`. 

Mỗi giá trị đầu ra khi đó là:```
total // p
```loại bỏ chính xác một thừa số nguyên tố khỏi tích đầy đủ. 

Việc sử dụng số nguyên Python ở đây rất thuận tiện vì các giá trị được xây dựng có thể vượt quá giới hạn 64 bit. Python tự động xử lý độ chính xác tùy ý và vấn đề rõ ràng cho phép các số nguyên rất lớn. 

Thứ tự của các hoạt động có vấn đề một chút. Đầu tiên chúng ta tính tích đầy đủ một lần, sau đó chia cho từng số nguyên tố. Việc tính toán lại các sản phẩm riêng biệt cho từng chỉ mục cũng sẽ hiệu quả, nhưng cách tiếp cận này sạch hơn và tránh được phép nhân lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Số nguyên tố được tạo:```
2 3 5
```Tổng sản phẩm:```
30
```| Bước | Thủ tướng đã xóa | Số được xây dựng | 
| --- | --- | --- | 
| 1 | 2 | 15 | 
| 2 | 3 | 10 | 
| 3 | 5 | 6 | 

Gcds theo cặp:```
gcd(15,10)=5
gcd(15,6)=3
gcd(10,6)=2
```Gcd toàn cầu:```
gcd(15,10,6)=1
```Ví dụ này thể hiện tính bất biến trung tâm. Mỗi cặp đều có chung một số số nguyên tố, nhưng không có số nguyên tố nào xuất hiện trong mọi số. 

### Ví dụ 2 

đầu vào:```
4
```Số nguyên tố được tạo:```
2 3 5 7
```Tổng sản phẩm:```
210
```| Bước | Thủ tướng đã xóa | Số được xây dựng | 
| --- | --- | --- | 
| 1 | 2 | 105 | 
| 2 | 3 | 70 | 
| 3 | 5 | 42 | 
| 4 | 7 | 30 | 

Kiểm tra một vài gcd:```
gcd(105,70)=35
gcd(70,42)=14
gcd(42,30)=6
```Gcd toàn cầu:```
gcd(105,70,42,30)=1
```Dấu vết này cho thấy các gcd theo cặp có thể lớn hơn nhiều so với`1`. Chúng ta chỉ cần chúng không tầm thường, không nhất thiết phải là số nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Thế hệ số nguyên tố với phép chia thử nghiệm trên số nguyên tố nhỏ | 
| Không gian | O(n) | Lưu trữ danh sách chính | 

Những hạn chế là rất nhỏ. Với`n ≤ 50`, ngay cả việc tạo số nguyên tố đơn giản cũng có hiệu quả ngay lập tức. Các số nguyên được xây dựng vẫn ở dưới giới hạn 100 chữ số và các số nguyên lớn của Python dễ dàng xử lý chúng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd
from functools import reduce

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def generate_primes(cnt):
        primes = []
        x = 2

        while len(primes) < cnt:
            ok = True

            for p in primes:
                if p * p > x:
                    break
                if x % p == 0:
                    ok = False
                    break

            if ok:
                primes.append(x)

            x += 1

        return primes

    out = io.StringIO()

    n = int(input())

    if n == 2:
        out.write("-1\n")
        return out.getvalue()

    primes = generate_primes(n)

    total = 1
    for p in primes:
        total *= p

    arr = []

    for p in primes:
        arr.append(total // p)

    for x in arr:
        out.write(str(x) + "\n")

    return out.getvalue()

def validate(output: str, n: int):
    if n == 2:
        return output.strip() == "-1"

    arr = list(map(int, output.strip().split()))

    assert len(arr) == n
    assert len(set(arr)) == n

    for i in range(n):
        for j in range(i + 1, n):
            assert gcd(arr[i], arr[j]) != 1

    assert reduce(gcd, arr) == 1

    return True

# provided sample
assert validate(run("3\n"), 3)

# minimum impossible case
assert run("2\n").strip() == "-1", "n=2 impossible"

# small valid case
assert validate(run("4\n"), 4)

# larger case
assert validate(run("10\n"), 10)

# maximum constraint
assert validate(run("50\n"), 50)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`-1`| Trường hợp ranh giới bất khả thi | 
|`3`| Bất kỳ bộ ba hợp lệ nào | Đầu vào có thể giải được nhỏ nhất | 
|`4`| Bất kỳ bộ bốn hợp lệ nào | Xây dựng chung đúng đắn | 
|`10`| Bất kỳ bộ hợp lệ nào | Logic gcd theo cặp trên mảng lớn hơn | 
|`50`| Bất kỳ bộ hợp lệ nào | Xử lý hạn chế tối đa | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là:```
2
```Thuật toán in ngay lập tức:```
-1
```Điều này đúng vì chỉ có hai số:```
gcd(a1,a2)
```đồng thời là gcd theo cặp và gcd toàn cục. Nếu gcd theo cặp lớn hơn`1`, gcd toàn cầu không thể bằng`1`. 

Một trường hợp tinh tế khác là đầu vào hợp lệ nhỏ nhất:```
3
```Thuật toán tạo ra:```
15
10
6
```Mỗi số thiếu chính xác một số nguyên tố:```
15 misses 2
10 misses 3
6 misses 5
```Mỗi cặp vẫn có chung một số nguyên tố còn lại và không có số nguyên tố nào chia hết cả ba số. 

Trường hợp cạnh liên quan đến sự khác biệt xảy ra khi các cấu trúc vô tình lặp lại các giá trị. Ở đây điều đó không thể xảy ra. 

Vì`n = 4`:```
105
70
42
30
```Mỗi số được hình thành bằng cách loại trừ một số nguyên tố khác nhau. Vì các số nguyên tố khác nhau bị bỏ qua nên các kết quả không thể khớp. 

Trường hợp tối đa:```
50
```cũng hoạt động an toàn. Tích của 50 số nguyên tố đầu tiên có ít hơn 100 chữ số, do đó giới hạn kích thước đầu ra được tôn trọng. Số nguyên Python xử lý số học mà không bị tràn.
