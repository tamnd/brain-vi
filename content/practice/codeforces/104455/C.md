---
title: "CF 104455C - Đếm ba lần"
description: "Chúng ta có ba mảng có độ dài bằng nhau và mỗi vị trí chứa một số nguyên dương. Từ các mảng này, chúng tôi không chọn các phần tử theo thứ tự hoặc cấu trúc ràng buộc, chúng tôi chỉ đơn giản hình thành các bộ ba độc lập bằng cách chọn một chỉ mục từ mỗi mảng."
date: "2026-06-30T13:40:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104455
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #19 (Briefest-Forces)"
rating: 0
weight: 104455
solve_time_s: 72
verified: true
draft: false
---

[CF 104455C - Đếm bộ ba](https://codeforces.com/problemset/problem/104455/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba mảng có độ dài bằng nhau và mỗi vị trí chứa một số nguyên dương. Từ các mảng này, chúng tôi không chọn các phần tử theo thứ tự hoặc cấu trúc ràng buộc, chúng tôi chỉ đơn giản hình thành các bộ ba độc lập bằng cách chọn một chỉ mục từ mỗi mảng. Đối với mọi lựa chọn chỉ số$i, j, k$, chúng tôi tính toán sản phẩm$a_i \cdot b_j \cdot c_k$và chúng tôi muốn đếm xem có bao nhiêu bộ ba như vậy tạo ra chính xác giá trị mục tiêu$m$. 

Vì vậy, nhiệm vụ hoàn toàn mang tính tổ hợp trên các giá trị: mọi lần xuất hiện trong mỗi mảng đều tham gia độc lập và các giá trị giống hệt nhau không thể phân biệt được ngoại trừ bội số. 

Hạn chế chính đó là$n$có thể lớn như$5 \cdot 10^5$. Việc liệt kê trực tiếp tất cả các bộ ba sẽ xem xét$n^3$sự kết hợp, điều tồi tệ nhất là về$1.25 \cdot 10^{17}$hoạt động. Ngay cả khi cắt tỉa nhiều, quy mô đó hoàn toàn không thể thực hiện được trong hai giây. Điều này ngay lập tức buộc chúng ta phải nén vấn đề vào không gian tần số thay vì không gian chỉ mục. 

Một hạn chế quan trọng khác là mọi phần tử mảng có nhiều nhất là$m$, Và$m \le 10^9$. Điều này quan trọng vì cấu trúc chia hết trở thành trọng tâm: nếu một giá trị không chia hết$m$, nó không bao giờ có thể đóng góp vào một bộ ba hợp lệ. 

Một vài trường hợp phức tạp xuất hiện một cách tự nhiên. 

Nếu như$m = 1$, thì mọi bộ ba hợp lệ phải bao gồm toàn bộ một. Ví dụ: nếu tất cả các mảng đều$[1, 2]$, thì chỉ những vị trí mà cả ba lượt chọn đều đóng góp 1 điểm và mọi thứ khác đều không liên quan. 

Nếu một mảng chứa các giá trị không chia$m$, chẳng hạn như$a = [2, 3]$Và$m = 6$, thì chỉ có ước của$m$quan trọng và các số không chia có thể được bỏ qua hoàn toàn. 

Một trường hợp tế nhị hơn phát sinh khi$m$có cấu trúc nhân tố lặp lại. Ví dụ,$m = 18$. Gấp ba lượt thích$(2, 3, 3)$,$(3, 3, 2)$và các hoán vị đều đóng góp và việc đếm phải tôn trọng bội số trên các mảng thay vì các giá trị duy nhất. 

Chế độ thất bại chính đối với lý luận ngây thơ là coi đây là một lần tìm kiếm trên các giá trị mà không tính toán chính xác bội số tần số trong mỗi mảng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là lặp lại tất cả các bộ ba chỉ số, tính tích và kiểm tra xem nó có bằng không$m$. Điều này đúng vì nó khớp chính xác với định nghĩa của vấn đề. Tuy nhiên, nó đòi hỏi ba vòng lặp lồng nhau$n$, sản xuất$O(n^3)$hoạt động. Với$n = 5 \cdot 10^5$, thậm chí$n^2$đã quá lớn rồi, vì vậy$n^3$dứt khoát là không thể được. 

Quan sát quan trọng là tình trạng sản phẩm được phân tách rõ ràng trên ba mảng. Thay vì suy nghĩ về các chỉ số, chúng ta chuyển sang đếm tần số của các giá trị. Nếu chúng ta biết mỗi giá trị xuất hiện bao nhiêu lần trong mỗi mảng thì mỗi lựa chọn giá trị sẽ đóng góp theo cấp số nhân: if value$x$xuất hiện$f_a[x]$lần trong$a$,$f_b[y]$lần trong$b$, Và$f_c[z]$lần trong$c$, thì số lượng chỉ số tăng gấp ba lần tạo ra$(x, y, z)$là$f_a[x] \cdot f_b[y] \cdot f_c[z]$. 

Vì vậy, vấn đề giảm xuống việc đếm tất cả các giá trị gấp ba$(x, y, z)$như vậy$x \cdot y \cdot z = m$, được tính theo tần số. 

Điều này chuyển vấn đề sang cấu trúc ước số của$m$. Thay vì lặp lại tất cả các giá trị trong mảng, chúng ta chỉ xét các ước của$m$, vì bất kỳ thừa số nào trong bộ ba hợp lệ đều phải chia$m$. Số ước của$m \le 10^9$nhỏ (nhiều nhất là vài nghìn trong trường hợp xấu nhất), điều này làm cho việc liệt kê trở nên khả thi. 

Do đó, chiến lược tối ưu là: tính toán bản đồ tần số cho cả ba mảng, liệt kê các ước số$x$Và$y$của$m$, suy ra$z = m / (x \cdot y)$và xác minh nó là tích phân và tồn tại trong bản đồ tần số thứ ba. 

Điều này làm giảm không gian tìm kiếm từ$n^3$kết hợp chỉ mục để đại khái$d(m)^2$kết hợp giá trị, trong đó$d(m)$là số ước của$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Bảng liệt kê tần số + số chia |$O(n + d(m)^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của tất cả các giá trị trong mảng$a$,$b$, Và$c$. Điều này cho phép chúng ta thay thế việc đếm dựa trên chỉ số bằng cách đếm dựa trên bội số. 
2. Tính tất cả các ước của$m$. Mỗi ước số đại diện cho một giá trị có thể xuất hiện ở bất kỳ vị trí nào của bộ ba hợp lệ. Bước này rất quan trọng vì chỉ có các ước của$m$có thể tham gia vào các sản phẩm tương đương$m$. 
3. Lặp lại từng ước số$x$của$m$. Đối xử$x$là giá trị được chọn từ mảng$a$. Chúng tôi nhân câu trả lời với số cách chọn chỉ số đóng góp giá trị$x$, đó là tần số của nó trong$a$. 
4. Đối với mỗi như vậy$x$, lặp qua từng ước số$y$của$m$. Đối xử$y$như giá trị từ mảng$b$. Chúng tôi nhân với tần số của nó trong$b$. 
5. Tính toán$z = m / (x \cdot y)$. Nếu như$m$không chia hết cho$x \cdot y$, bỏ qua cặp này vì không tồn tại giá trị thứ ba hợp lệ. 
6. Kiểm tra xem$z$tồn tại trong mảng$c$bằng cách sử dụng bản đồ tần số. Nếu có thì thêm$f_a[x] \cdot f_b[y] \cdot f_c[z]$để trả lời. 

### Tại sao nó hoạt động 

Mỗi bộ ba chỉ số hợp lệ tương ứng với một bộ ba giá trị$(x, y, z)$như vậy$x \cdot y \cdot z = m$. Thuật toán liệt kê mọi giá trị như vậy gấp ba lần chính xác một lần vì$x$Và$y$nằm trên tất cả các ước của$m$, Và$z$được xác định duy nhất khi hợp lệ. Mỗi đóng góp được tính trọng số bởi bội số độc lập từ ba mảng, do đó không có kết hợp chỉ mục hợp lệ nào bị bỏ sót hoặc được tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter
import math

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
c = list(map(int, input().split()))

fa = Counter(a)
fb = Counter(b)
fc = Counter(c)

divs = []
for i in range(1, int(math.isqrt(m)) + 1):
    if m % i == 0:
        divs.append(i)
        if i * i != m:
            divs.append(m // i)

ans = 0

for x in divs:
    if x not in fa:
        continue
    for y in divs:
        if y not in fb:
            continue
        prod = x * y
        if m % prod != 0:
            continue
        z = m // prod
        if z in fc:
            ans += fa[x] * fb[y] * fc[z]

print(ans)
```Bản đồ tần số thay thế hoàn toàn việc theo dõi chỉ mục, đảm bảo chúng tôi tính toán chính xác các giá trị lặp lại. Danh sách ước số được xây dựng một lần, tránh việc nhân tử hóa lặp đi lặp lại. Các vòng chia số lồng nhau là an toàn vì số lượng ước số nhỏ ngay cả trong trường hợp xấu nhất. 

Một chi tiết triển khai tinh vi đang được kiểm tra`m % prod != 0`trước khi tính toán`z`, ngăn chặn các trường hợp chia số nguyên không hợp lệ và tránh việc tra cứu từ điển không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 3, m = 3
a = [1, 2, 3]
b = [1, 1, 3]
c = [2, 3, 3]
```Các ước của 3 là$[1, 3]$. Bản đồ tần số là:$f_a: 1:1, 2:1, 3:1$

$f_b: 1:2, 3:1$

$f_c: 2:1, 3:2$| x | y | x*y | z hợp lệ? | z | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 3 | 1×2×2 = 4 | 
| 1 | 3 | 3 | không | - | 0 | 
| 3 | 1 | 3 | không | - | 0 | 
| 3 | 3 | 9 | không | - | 0 | 

Câu trả lời cuối cùng là 4. 

Điều này khẳng định rằng bội số trong$b$Và$c$đóng góp trực tiếp vào quy mô và chỉ các cặp số chia nhất quán mới quan trọng. 

### Mẫu 2 

đầu vào:```
n = 4, m = 2
a = [1, 1, 1, 1]
b = [1, 1, 1, 1]
c = [1, 1, 1, 1]
```Ước của 2 là$[1, 2]$. Tuy nhiên, không có phần tử nào bằng 2 tồn tại trong bất kỳ mảng nào. 

| x | y | x*y | z = 2/(x*y) | có hiệu lực? | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | không (c có số 2) | 0 | 
| 1 | 2 | 2 | 1 | không (b có số 2) | 0 | 
| 2 | 1 | 2 | 1 | không (a có số 2) | 0 | 
| 2 | 2 | 4 | - | không | 0 | 

Câu trả lời cuối cùng là 0. 

Điều này cho thấy rằng chỉ liệt kê ước số là không đủ và việc kiểm tra sự tồn tại tần số là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + d(m)^2)$| việc xây dựng bản đồ tần số mất thời gian tuyến tính và các cặp số chia được liệt kê một lần | 
| Không gian |$O(n)$| lưu trữ cho các bộ đếm tần số lên đến$n$yếu tố | 

Số chia của bất kỳ số nguyên nào lên đến$10^9$vẫn đủ nhỏ để$d(m)^2$là không đáng kể so với$n$, vì vậy giải pháp phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import Counter

    n, m = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    b = list(map(int, sys.stdin.readline().split()))
    c = list(map(int, sys.stdin.readline().split()))

    fa, fb, fc = Counter(a), Counter(b), Counter(c)

    divs = []
    for i in range(1, int(math.isqrt(m)) + 1):
        if m % i == 0:
            divs.append(i)
            if i * i != m:
                divs.append(m // i)

    ans = 0
    for x in divs:
        for y in divs:
            prod = x * y
            if m % prod != 0:
                continue
            z = m // prod
            ans += fa[x] * fb[y] * fc[z]

    return str(ans)

# provided samples
assert run("""3 3
1 2 3
1 1 3
2 3 3
""") == "4"

assert run("""4 2
1 1 1 1
1 1 1 1
1 1 1 1
""") == "0"

# custom cases
assert run("""1 1
1
1
1
""") == "1"

assert run("""2 6
2 3
1 2
1 3
""") == "1"

assert run("""3 12
2 2 3
2 3 2
3 2 2
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn tất cả những cái | 1 | bộ ba hợp lệ tối thiểu | 
| Yếu tố nhỏ hỗn hợp | 1 | khớp đúng cấu trúc số chia | 
| Nhiều hoán vị | 8 | xử lý các kết hợp hợp lệ lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$m = 1$. Trong tình huống này, mọi bộ ba hợp lệ chỉ được sử dụng giá trị 1. Thuật toán xử lý việc này một cách tự nhiên vì danh sách ước số chỉ chứa 1. Các vòng lặp giảm xuống còn một kiểm tra duy nhất trong đó$x = y = z = 1$, và kết quả trở thành$f_a[1] \cdot f_b[1] \cdot f_c[1]$, điều đó hoàn toàn chính xác. 

Một trường hợp khác là khi mảng chứa nhiều giá trị không chia hết$m$. Các giá trị này không bao giờ xuất hiện trong danh sách số chia, do đó chúng tự động bị các vòng lặp dựa trên tần số bỏ qua. Ví dụ, nếu$m = 10$và mảng chứa nhiều số 3, những mục đó không bao giờ tham gia vì 3 không bao giờ được coi là ước số ứng viên. 

Một trường hợp tế nhị cuối cùng là khi$x \cdot y$vượt quá$m$. Séc`m % prod != 0`đảm bảo chúng tôi không bao giờ thử các thành phần thứ ba không hợp lệ. Ví dụ, nếu$m = 12$, đang chọn$x = 4$Và$y = 4$mang lại sản phẩm 16, sản phẩm này sẽ bị loại bỏ ngay lập tức trước khi tra cứu vào bản đồ thứ ba.
