---
title: "CF 2125C - Đếm Số Tốt"
description: "Một số được gọi là tốt nếu không có số nguyên tố nào trong hệ số nguyên tố của nó là số nguyên tố có một chữ số. Các số nguyên tố duy nhất có một chữ số là 2, 3, 5 và 7, vì vậy một số được coi là tốt khi nó không chia hết cho bất kỳ số nào trong bốn số nguyên tố này."
date: "2026-06-08T03:27:37+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2125
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 181 (Rated for Div. 2)"
rating: 1100
weight: 2125
solve_time_s: 120
verified: true
draft: false
---

[CF 2125C - Đếm số tốt](https://codeforces.com/problemset/problem/2125/C) 

**Đánh giá:** 1100 
**Tags:** bitmasks, tổ hợp, toán học, lý thuyết số 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một số được gọi là tốt nếu không có số nguyên tố nào trong hệ số nguyên tố của nó là số nguyên tố có một chữ số. Các số nguyên tố duy nhất có một chữ số là 2, 3, 5 và 7, vì vậy một số được coi là tốt khi nó không chia hết cho bất kỳ số nào trong bốn số nguyên tố này. 

Nhiệm vụ là đếm có bao nhiêu số nguyên trong một phạm vi$[l,r]$thỏa mãn tính chất này. 

Các giới hạn hoàn toàn thay đổi cách chúng ta nghĩ về vấn đề. Các điểm cuối của khoảng có thể lớn bằng$10^{18}$, nên việc phân tích từng số một là không thể. Ngay cả việc lặp qua khoảng thời gian cũng không thể thực hiện được vì bản thân phạm vi đó có thể chứa tới$10^{18}$những con số. Chúng ta cần một công thức đếm hoạt động trực tiếp trên các điểm cuối. 

Quan sát quan trọng là định nghĩa chỉ phụ thuộc vào khả năng chia hết cho bốn số nguyên tố cụ thể. Chúng tôi không quan tâm liệu một số có chứa các số nguyên tố lớn hơn hay không, bởi vì mọi số nguyên tố có ít nhất hai chữ số đều được phép. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Hãy xem xét phạm vi:```
13 37
```Câu trả lời là 7, không phải 25. Hầu hết các số trong khoảng này đều chia hết cho 2, 3, 5 hoặc 7 và phải được loại trừ. 

Coi như:```
11 11
```Câu trả lời là 1. Bản thân số 11 là số nguyên tố có hai chữ số nên số đó rất tốt. 

Coi như:```
14 14
```Câu trả lời là 0. Mặc dù 14 chỉ chứa số nguyên tố 7 một lần nhưng điều đó cũng đủ khiến nó trở nên tồi tệ. Sự đa dạng không thành vấn đề. 

Coi như:```
49 49
```Câu trả lời là 0. Một số giải pháp kiểm tra nhầm chỉ các thừa số nguyên tố nhỏ hơn 7 xuất hiện một lần. Từ$49=7^2$, nó vẫn chia hết cho 7 và vẫn xấu. 

Vấn đề thực sự là yêu cầu đếm các số nguyên không chia hết cho bất kỳ số nào trong số 2, 3, 5 hoặc 7. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp qua mọi số trong khoảng, phân tích nó và xác minh xem tất cả các thừa số nguyên tố có ít nhất là 10 hay không. Điều này đúng vì nó trực tiếp thực hiện định nghĩa. 

Thật không may, ngay cả việc kiểm tra một khoảng thời gian$10^{18}$là không thể. Phân tích một số yêu cầu khoảng$O(\sqrt n)$làm việc và có thể có$10^{18}$số để kiểm tra. 

Quan sát quan trọng là một số là tốt khi và chỉ khi nó không chia hết cho 2, 3, 5 hoặc 7. 

Điều đó chuyển đổi vấn đề thành một vấn đề đếm cổ điển. Thay vì xác định trực tiếp các số tốt, hãy đếm tất cả các số chia hết cho ít nhất một trong bốn số nguyên tố này và trừ chúng khỏi tổng số. 

Bất cứ khi nào chúng ta cần đếm các số chia hết cho nhiều bộ cùng một lúc, loại trừ bao gồm là công cụ tự nhiên. Chỉ có bốn số nguyên tố liên quan nên số lượng tập hợp con rất nhỏ:$$2^4=16.$$Đối với bất kỳ$x$, cho phép$F(x)$biểu thị số số nguyên tốt trong$[1,x]$. 

Số số nguyên chia hết cho một tập con số nguyên tố cụ thể bằng$$\left\lfloor \frac{x}{\operatorname{lcm}(\text{subset})} \right\rfloor.$$Vì các số nguyên tố là khác nhau nên LCM đơn giản là tích của chúng. 

Việc áp dụng loại trừ bao gồm sẽ cho ra số số nguyên chia hết cho ít nhất một số nguyên tố bị cấm. Trừ từ$x$sản lượng$F(x)$. Câu trả lời cho một truy vấn trở thành$$F(r)-F(l-1).$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)\sqrt r)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(2^4)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Cho các số nguyên tố bị cấm là:$$P=\{2,3,5,7\}.$$Đối với một giá trị nhất định$x$, định nghĩa$F(x)$là số số nguyên tốt trong$[1,x]$. 

1. Bắt đầu với`bad = 0`. 
2. Liệt kê mọi tập con khác rỗng của$\{2,3,5,7\}$. 
3. Tính tích các số nguyên tố trong tập con. Vì các số nguyên tố là khác nhau nên tích này cũng là bội số chung nhỏ nhất của chúng. 
4. Tính xem có bao nhiêu số nguyên$[1,x]$đều chia hết cho tích này:$$\left\lfloor \frac{x}{\text{product}} \right\rfloor.$$5. Nếu kích thước tập hợp con là số lẻ, hãy thêm số lượng này vào`bad`. 
6. Nếu kích thước tập hợp con là chẵn, hãy trừ số lượng này khỏi`bad`. 

Đây chính xác là nguyên tắc bao gồm-loại trừ. 
7. Sau khi xử lý tất cả các tập hợp con,`bad`bằng số số nguyên trong$[1,x]$chia hết cho ít nhất một số nguyên tố bị cấm. 
8. Tính toán$$F(x)=x-bad.$$9. Đối với mỗi truy vấn$[l,r]$, đầu ra$$F(r)-F(l-1).$$### Tại sao nó hoạt động 

Một số xấu chính xác khi nó thuộc về ít nhất một trong các bộ$$A_2,A_3,A_5,A_7,$$Ở đâu$A_p$chứa các số chia hết cho$p$. 

Loại trừ bao gồm tính toán kích thước của công đoàn$$|A_2\cup A_3\cup A_5\cup A_7|$$chính xác. Mỗi số chia hết cho một số nguyên tố bị cấm đều đóng góp một lần. Mọi số chia hết cho nhiều số nguyên tố bị cấm đều được hiệu chỉnh bằng các dấu xen kẽ của sự bao gồm-loại trừ. 

Trừ kích thước liên minh khỏi tổng số$x$để lại chính xác những số đó không chia hết cho số nguyên tố bị cấm. Đó chính xác là những con số tốt, vì vậy$F(x)$là đúng. Lấy$F(r)-F(l-1)$hạn chế số lượng trong khoảng thời gian được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

PRIMES = [2, 3, 5, 7]

def count_good(x):
    if x <= 0:
        return 0

    bad = 0

    for mask in range(1, 16):
        prod = 1
        bits = 0

        for i in range(4):
            if mask & (1 << i):
                prod *= PRIMES[i]
                bits += 1

        cnt = x // prod

        if bits & 1:
            bad += cnt
        else:
            bad -= cnt

    return x - bad

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        l, r = map(int, input().split())
        ans.append(str(count_good(r) - count_good(l - 1)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```chức năng`count_good(x)`tính số số nguyên tốt trong tiền tố$[1,x]$. 

Mặt nạ lặp đại diện cho tất cả các tập con không trống của`{2,3,5,7}`. Vì chỉ có bốn số nguyên tố nên tồn tại chính xác 15 tập con không rỗng. 

Đối với mỗi tập hợp con, chúng tôi tính toán tích của nó và số bội số bên trong tiền tố. Các tập hợp con có kích thước lẻ đóng góp tích cực và các tập hợp con có kích thước chẵn đóng góp tiêu cực, phù hợp với việc loại trừ bao gồm. 

biểu thức`x - bad`chuyển đổi số lượng số xấu thành số lượng số tốt. 

Câu trả lời cuối cùng sử dụng kỹ thuật đếm tiền tố tiêu chuẩn:$$\text{answer}=F(r)-F(l-1).$$Tất cả số học phù hợp thoải mái với số nguyên 64 bit vì tích trung gian lớn nhất là$$2\cdot3\cdot5\cdot7=210,$$trong khi số nguyên Python vẫn hỗ trợ độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 100
```Chúng tôi tính toán$F(100)$. 

| Tập hợp con | Sản phẩm | Bội số ≤ 100 | Ký tên | Đóng góp | 
| --- | --- | --- | --- | --- | 
| {2} | 2 | 50 | + | 50 | 
| {3} | 3 | 33 | + | 33 | 
| {5} | 5 | 20 | + | 20 | 
| {7} | 7 | 14 | + | 14 | 
| {2,3} | 6 | 16 | - | -16 | 
| {2,5} | 10 | 10 | - | -10 | 
| {2,7} | 14 | 7 | - | -7 | 
| {3,5} | 15 | 6 | - | -6 | 
| {3,7} | 21 | 4 | - | -4 | 
| {5,7} | 35 | 2 | - | -2 | 
| {2,3,5} | 30 | 3 | + | 3 | 
| {2,3,7} | 42 | 2 | + | 2 | 
| {2,5,7} | 70 | 1 | + | 1 | 
| {3,5,7} | 105 | 0 | + | 0 | 
| {2,3,5,7} | 210 | 0 | - | 0 | 

Tổng hợp các khoản đóng góp mang lại:$$bad=79.$$Kể từ đây$$F(100)=100-79=21.$$Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
13 37
```Chúng tôi tính toán:$$F(37)=9,$$Và$$F(12)=2.$$| Số lượng | Giá trị | 
| --- | --- | 
| Những con số tốt trong [1,37] | 9 | 
| Những con số tốt trong [1,12] | 2 | 
| Trả lời | 7 | 

Như vậy$$F(37)-F(12)=7.$$Điều này chứng tỏ tại sao việc đếm tiền tố lại hữu ích. Chúng tôi không bao giờ kiểm tra các số riêng lẻ trong khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^4)$mỗi trường hợp thử nghiệm | Bao gồm-loại trừ trên 15 tập con không trống | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Từ$2^4=16$, mỗi ca kiểm thử chỉ thực hiện một số phép tính số học. Ngay cả với$1000$trường hợp thử nghiệm, tổng công việc không đáng kể và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

PRIMES = [2, 3, 5, 7]

def count_good(x):
    if x <= 0:
        return 0

    bad = 0

    for mask in range(1, 16):
        prod = 1
        bits = 0

        for i in range(4):
            if mask & (1 << i):
                prod *= PRIMES[i]
                bits += 1

        if bits & 1:
            bad += x // prod
        else:
            bad -= x // prod

    return x - bad

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        l, r = map(int, input().split())
        out.append(str(count_good(r) - count_good(l - 1)))

    print("\n".join(out))

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    buf = io.StringIO()
    old = sys.stdout
    sys.stdout = buf

    solve()

    sys.stdout = old
    return buf.getvalue()

# provided sample
assert run(
"""4
2 100
2 1000
13 37
2 1000000000000000000
"""
) == (
"""21
227
7
228571428571428570
"""
)

# minimum interval
assert run(
"""1
2 2
"""
) == (
"""0
"""
)

# single good prime
assert run(
"""1
11 11
"""
) == (
"""1
"""
)

# divisible by forbidden prime
assert run(
"""1
49 49
"""
) == (
"""0
"""
)

# boundary around first good number
assert run(
"""1
10 13
"""
) == (
"""2
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`0`| Giá trị hợp lệ nhỏ nhất là không tốt | 
|`11 11`|`1`| số nguyên tố có hai chữ số là tốt | 
|`49 49`|`0`| Hệ số nguyên tố bị cấm lặp đi lặp lại | 
|`10 13`|`2`| Chuyển đổi ranh giới xung quanh số tốt đầu tiên | 

## Vỏ cạnh 

Hãy xem xét:```

```
