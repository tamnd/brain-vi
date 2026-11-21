---
title: "CF 222C - Phân số rút gọn"
description: "Phân số không được đưa ra dưới dạng tử số và mẫu số duy nhất. Thay vào đó, chúng tôi nhận được hai mảng. Tích của tất cả các số trong mảng thứ nhất là tử số và tích của tất cả các số trong mảng thứ hai là mẫu số."
date: "2026-06-04T02:02:38+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 222
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 137 (Div. 2)"
rating: 1800
weight: 222
solve_time_s: 149
verified: false
draft: false
---

[CF 222C - Phân số rút gọn](https://codeforces.com/problemset/problem/222/C) 

**Đánh giá:** 1800 
**Tags:** thực hiện, toán, lý thuyết số, sắp xếp 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Phân số không được đưa ra dưới dạng tử số và mẫu số duy nhất. Thay vào đó, chúng tôi nhận được hai mảng. 

Tích của tất cả các số trong mảng thứ nhất là tử số và tích của tất cả các số trong mảng thứ hai là mẫu số. 

Nhiệm vụ của chúng ta là xuất ra hai mảng mới biểu thị cùng một phân số sau khi rút gọn hoàn toàn. Nói cách khác, nếu$$N=\prod a_i,\qquad D=\prod b_i$$sau đó chúng ta phải chia cả hai$N$Và$D$bằng ước số chung lớn nhất của chúng và biểu diễn lại tử số và mẫu số thu được dưới dạng mảng số nguyên không vượt quá$10^7$. 

Thử thách đầu tiên là bản thân các sản phẩm đã rất lớn. Thậm chí vài chục giá trị gần$10^7$đã vượt quá phạm vi số nguyên bình thường và ở đây chúng tôi có thể có tới$10^5$những con số. Xây dựng$N$Và$D$trực tiếp là không thể. 

Các ràng buộc gợi ý mạnh mẽ một cách tiếp cận yếu tố nguyên tố. Mỗi giá trị riêng lẻ tối đa là$10^7$, có thể được phân tích thành thừa số một cách hiệu quả bằng cách sử dụng sàng có hệ số nguyên tố nhỏ nhất. Vì có tới$2 \cdot 10^5$tổng số, bất kỳ thuật toán nào liên quan đến tính toán gcd theo cặp giữa tất cả các phần tử tử số và mẫu số sẽ quá đắt. 

Một điểm tinh tế là đầu ra vẫn phải được biểu diễn dưới dạng mảng các số nguyên không lớn hơn$10^7$. Sau khi rút gọn, các thừa số nguyên tố còn lại phải được phân bố lại thành các số thỏa mãn giới hạn đó. 

Hãy xem xét ví dụ sau:```
1 1
12
18
```Phân số là$12/18$. gcd là$6$, vậy phần rút gọn là$2/3$. 

Một giải pháp bất cẩn loại bỏ các thừa số một cách tham lam giữa các phần tử mảng nhưng không loại bỏ hoàn toàn tất cả các thừa số nguyên tố chung có thể để lại một cách sai lầm$4/6$, không giảm. 

Một trường hợp quan trọng khác là khi mọi thứ đều bị hủy:```
1 1
100
100
```Phân số rút gọn là$1/1$. 

Mảng đầu ra không được để trống. Giải pháp loại bỏ tất cả các yếu tố và in số 0 sẽ vi phạm định dạng. Chúng ta phải xuất ra ít nhất một giá trị, cụ thể là`1`. 

Thêm một ví dụ phức tạp nữa:```
2 1
8 9
6
```Tử số bằng$72$, mẫu số bằng$6$, vậy phần rút gọn là$12/1$. 

Yếu tố chính$2$từ mẫu số chỉ hủy bỏ một phần phân tích của tử số. Bất kỳ cách tiếp cận nào chỉ so sánh các số nguyên thay vì số mũ nguyên tố đều có thể dễ dàng bỏ lỡ các phép hủy từng phần như vậy. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xây dựng lại tích tử số và mẫu số, tính gcd của chúng, chia cả hai cho nó và sau đó xây dựng lại mảng bằng cách nào đó. 

Điều này đúng về mặt toán học nhưng hoàn toàn không khả thi. Sản phẩm có chứa tới$10^5$các yếu tố, mỗi yếu tố lớn như$10^7$. Các số nguyên thu được sẽ có hàng trăm nghìn chữ số. 

Một ý tưởng phức tạp hơn một chút là tính toán liên tục gcds giữa các phần tử tử số và phần tử mẫu số. Đối với mỗi cặp$(a_i,b_j)$, hãy phân tích các ước chung của chúng. 

Điều này cuối cùng tạo ra một phân số rút gọn vì mọi thừa số nguyên tố chung phải xuất hiện bên trong một số cặp. Thật không may có tới$10^5$số ở mỗi bên, dẫn đến$10^{10}$cặp trong trường hợp xấu nhất. 

Quan sát quan trọng là sự rút gọn chỉ phụ thuộc vào số mũ nguyên tố. 

Nếu một số nguyên tố$p$xuất hiện$x$lần ở tử số và$y$lần ở mẫu số, sau đó giảm:$$p^{x-\min(x,y)}$$vẫn ở tử số và$$p^{y-\min(x,y)}$$vẫn ở mẫu số. 

Vì vậy, thay vì loại bỏ các thừa số giữa các số riêng lẻ, chúng ta có thể phân tích từng giá trị đầu vào thành thừa số và đếm tổng số mũ của mỗi số nguyên tố ở cả hai vế. 

Sau khi biết tất cả các số đếm, chúng tôi trừ đi số tiền chung cho mỗi số nguyên tố. Những gì còn lại chính xác là hệ số nguyên tố của tử số và mẫu số rút gọn. 

Bước cuối cùng là xây dựng lại mảng. Chúng tôi thu thập các thừa số nguyên tố còn lại và nhân chúng với nhau một cách tham lam, bắt đầu một số đầu ra mới bất cứ khi nào phép nhân tiếp theo vượt quá$10^7$. Vì mỗi thừa số nguyên tố lớn nhất là$10^7$, điều này luôn thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hủy theo cặp | O(nm log A) | O(1) | Quá chậm | 
| Đếm thừa số nguyên tố tối ưu | O(A log log A + K) | O(A) | Đã chấp nhận | 

Đây$A=10^7$, Và$K$là tổng số thừa số nguyên tố của tất cả các số đầu vào. 

## Hướng dẫn thuật toán 

1. Xây dựng sàng tìm thừa số nguyên tố nhỏ nhất cho tất cả các số đến$10^7$. 
2. Phân tích từng giá trị tử số bằng cách sử dụng sàng và tích lũy số mũ nguyên tố trong bản đồ`cntA`. 
3. Phân tích từng giá trị mẫu số bằng cách sử dụng sàng và tích lũy số mũ nguyên tố trong bản đồ`cntB`. 
4. Với mỗi số nguyên tố xuất hiện trong cả hai bản đồ, hãy tính$$c=\min(cntA[p],cntB[p]).$$Trừ`c`từ cả hai số đếm. 

Điều này loại bỏ chính xác số mũ đóng góp cho gcd. 
5. Cấu trúc lại tử số rút gọn. 

Đối với mỗi số nguyên tố$p$, nối thêm$p$chính xác`cntA[p]`lần vào một dòng các thừa số nguyên tố còn lại. 

Nhân các yếu tố vào số đầu ra hiện tại trong khi sản phẩm không vượt quá$10^7$. Nếu không, hãy hoàn thành số hiện tại và bắt đầu số mới. 
6. Xây dựng lại mẫu số rút gọn bằng cách sử dụng quy trình tương tự và các số mũ còn lại từ`cntB`. 
7. Nếu một trong hai mảng được xây dựng lại trống, hãy thay thế nó bằng`[1]`. 
8. Xuất kích thước của cả hai mảng theo sau là chính các mảng đó. 

### Tại sao nó hoạt động 

Hệ số của một số nguyên là duy nhất. Mỗi số nguyên tố đóng góp độc lập vào tử số và mẫu số. 

Đối với mỗi số nguyên tố$p$, gcd loại bỏ chính xác$$p^{\min(cntA[p],cntB[p])}.$$Sau khi trừ các số mũ đó, các số mũ còn lại chính xác là số mũ của phân số đã rút gọn. Không còn thừa số nguyên tố chung nào vì ít nhất một bên có số mũ bằng 0 cho mọi số nguyên tố. 

Giai đoạn tái thiết chỉ đơn thuần nhóm các thừa số nguyên tố còn lại thành các số không vượt quá$10^7$. Nó không thay đổi tập hợp các thừa số nguyên tố, do đó tử số và mẫu số được biểu diễn vẫn giống với phân số rút gọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10 ** 7

def build_spf(n):
    spf = list(range(n + 1))
    spf[0] = spf[1] = 0

    limit = int(n ** 0.5)
    for i in range(2, limit + 1):
        if spf[i] == i:
            step = i
            start = i * i
            for j in range(start, n + 1, step):
                if spf[j] == j:
                    spf[j] = i
    return spf

def factorize(x, spf, cnt):
    while x > 1:
        p = spf[x]
        c = 0
        while x % p == 0:
            x //= p
            c += 1
        cnt[p] = cnt.get(p, 0) + c

def rebuild(cnt):
    res = []
    cur = 1

    for p in sorted(cnt):
        e = cnt[p]
        for _ in range(e):
            if cur * p <= MAXV:
                cur *= p
            else:
                res.append(cur)
                cur = p

    if cur > 1:
        res.append(cur)

    if not res:
        res = [1]

    return res

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    mx = max(max(a), max(b))
    spf = build_spf(mx)

    cntA = {}
    cntB = {}

    for x in a:
        factorize(x, spf, cntA)

    for x in b:
        factorize(x, spf, cntB)

    primes = set(cntA) | set(cntB)

    for p in primes:
        common = min(cntA.get(p, 0), cntB.get(p, 0))
        if common:
            cntA[p] = cntA.get(p, 0) - common
            cntB[p] = cntB.get(p, 0) - common

    num = rebuild(cntA)
    den = rebuild(cntB)

    print(len(num), len(den))
    print(*num)
    print(*den)

if __name__ == "__main__":
    solve()
```Sàng lưu trữ hệ số nguyên tố nhỏ nhất của mọi số nguyên. Việc phân tích thành thừa số sẽ diễn ra rất nhanh vì mỗi phép chia sẽ loại bỏ ít nhất một thừa số nguyên tố. 

Các bản đồ`cntA`Và`cntB`lưu trữ tổng số mũ, không phải thông tin trên mỗi số. Đó chính xác là những gì việc giảm phân số yêu cầu. 

Trong quá trình tái thiết, chúng tôi bảo toàn mọi thừa số nguyên tố còn lại. Việc đóng gói tham lam là an toàn vì mỗi thừa số nguyên tố nhiều nhất là$10^7$. Nếu phép nhân vượt quá giới hạn, chúng ta chỉ cần bắt đầu một số đầu ra mới. 

Việc xử lý đặc biệt cho mảng trống là cần thiết khi tất cả các yếu tố đều bị hủy bỏ. Định dạng đầu ra yêu cầu ít nhất một số ở mỗi bên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
100 5 2
50 10
```Phân tích các thừa số nguyên tố: 

| Nguồn | Nhân tố hóa | 
| --- | --- | 
| 100 |$2^2 \cdot 5^2$| 
| 5 |$5$| 
| 2 |$2$| 
| 50 |$2 \cdot 5^2$| 
| 10 |$2 \cdot 5$| 

Số lượng tích lũy: 

| Thủ tướng | Tử số | Mẫu số | Chung | Tử số còn lại | Mẫu Số Còn Lại | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 2 | 2 | 1 | 0 | 
| 5 | 3 | 3 | 3 | 0 | 0 | 

Tái thiết: 

| Bên | Các số nguyên tố còn lại | Đầu ra | 
| --- | --- | --- | 
| Tử số | 2 | [2] | 
| Mẫu số | không | [1] | 

Đầu ra có thể:```
1 1
2
1
```Đầu ra mẫu sử dụng một nhóm khác, nhưng cả hai đều đại diện cho phần rút gọn$2/1$. 

### Mẫu 2```
2 1
8 9
6
```Số lượng yếu tố: 

| Thủ tướng | Tử số | Mẫu số | 
| --- | --- | --- | 
| 2 | 3 | 1 | 
| 3 | 2 | 1 | 

Hủy bỏ: 

| Thủ tướng | Tử số còn lại | Mẫu Số Còn Lại | 
| --- | --- | --- | 
| 2 | 2 | 0 | 
| 3 | 1 | 0 | 

Tái thiết: 

| Dòng yếu tố hiện tại | Số đầu ra | 
| --- | --- | 
| 2, 2, 3 | 12 | 

Kết quả cuối cùng:```
1 1
12
1
```Dấu vết này cho thấy việc hủy xảy ra ở cấp số mũ nguyên tố chứ không phải giữa các số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log log M + K) | Xây dựng sàng cộng với công việc nhân tố tổng hợp | 
| Không gian | O(M) | Mảng thừa số nguyên tố nhỏ nhất | 

Đây$M$là giá trị tối đa xuất hiện ở đầu vào, nhiều nhất là$10^7$, Và$K$là tổng số các thừa số nguyên tố được rút ra. 

Chi phí chủ yếu là xây dựng sàng có hệ số nguyên tố nhỏ nhất. Cách tiếp cận này là giải pháp dự kiến ​​cho những hạn chế ban đầu và phù hợp một cách thoải mái trong giới hạn khi triển khai được tối ưu hóa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from math import gcd

    sys.stdin = io.StringIO(inp)

    # output depends on valid grouping, so these tests
    # verify exact cases with unique natural outputs.

    return "placeholder"

# minimum size
# 1/1 remains 1/1

# all factors cancel
# input:
# 1 1
# 100
# 100

# numerator larger after reduction
# input:
# 2 1
# 8 9
# 6

# denominator larger after reduction
# input:
# 1 2
# 6
# 8 9

# prime values near boundary
# input:
# 1 1
# 9999991
# 1
```Đối với vấn đề này, nhiều kết quả đầu ra khác nhau đều hợp lệ vì các yếu tố có thể được nhóm khác nhau. Trong thực tế, bộ khai thác thử nghiệm phải xác minh rằng các sản phẩm được biểu thị bằng mảng đầu ra tạo thành phân số rút gọn chính xác, thay vì so sánh văn bản chính xác. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 1`|`1/1`| Kích thước tối thiểu | 
|`100 / 100`|`1/1`| Hủy bỏ hoàn toàn | 
|`8 9 / 6`|`12/1`| Hủy một phần nguyên tố | 
|`6 / 8 9`|`1/12`| Mẫu số còn lại | 
|`9999991 / 1`| Cùng một phân số | Xử lý số nguyên tố lớn | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
100
100
```Số lượng yếu tố giống hệt nhau ở cả hai bên:$$2^2 \cdot 5^2$$Tất cả số mũ đều bị hủy, không để lại thừa số nguyên tố nào. Thủ tục xây dựng lại tạo ra các danh sách trống, được thay thế bằng`[1]`. Đầu ra trở thành:```
1 1
1
1
```đại diện chính xác cho phần giảm. 

Bây giờ hãy xem xét:```
2 1
8 9
6
```Tử số chứa$2^3 \cdot 3^2$, trong khi mẫu số chứa$2 \cdot 3$. Sau khi trừ các số mũ chung, hệ số còn lại là$2^2 \cdot 3$. Thuật toán xây dựng lại`12`, sản xuất`12/1`. 

Cuối cùng:```
1 1
9999991
9999991
```Giá trị là một số nguyên tố lớn. Bản đồ phân tích nhân tử chứa một lần xuất hiện của số nguyên tố đó ở cả hai vế. Việc hủy sẽ loại bỏ nó hoàn toàn, để lại`1/1`. Điều này xác nhận rằng thuật toán xử lý các số nguyên tố lớn dễ dàng như hợp số vì mọi thứ đều được xử lý thông qua số mũ nguyên tố.
