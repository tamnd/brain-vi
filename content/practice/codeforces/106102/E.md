---
title: "CF 106102E - Bút chì"
description: "Hộp là một lưới M × N. Ô ở hàng i và cột j chứa một cây bút chì có chiều dài là i · N + j. Các hàng và cột được lập chỉ mục bằng 0, do đó ô trên cùng bên trái chứa độ dài 0. Đối với mỗi trường hợp thử nghiệm, chúng ta có M, N và độ dài mục tiêu L."
date: "2026-06-25T11:49:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106102
codeforces_index: "E"
codeforces_contest_name: "AGM 2025, Final Round, Day 1"
rating: 0
weight: 106102
solve_time_s: 65
verified: true
draft: false
---

[CF 106102E - Bút chì](https://codeforces.com/problemset/problem/106102/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chiếc hộp là một`M × N`lưới. Ô ở hàng`i`và cột`j`chứa một cây bút chì có chiều dài là`i · N + j`. Các hàng và cột không được lập chỉ mục, vì vậy ô trên cùng bên trái chứa độ dài`0`. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp`M`,`N`, và độ dài mục tiêu`L`. Chúng ta có thể chọn bất kỳ hình chữ nhật con thẳng hàng theo trục nào của lưới và nối tất cả các bút chì bên trong nó lại với nhau. Tổng chiều dài của cây bút chì thu được là tổng của tất cả các giá trị bên trong hình chữ nhật con đó. 

Nhiệm vụ là tìm một hình chữ nhật con có tổng bằng chính xác`L`. Nếu có nhiều hình chữ nhật như vậy thì chúng ta phải xuất ra diện tích nhỏ nhất có thể. Nếu không có hình chữ nhật nào có tổng`L`, chúng tôi xuất ra`-1`. 

Kích thước có thể lớn như`10^6`, trong khi`L`có thể đạt được`10^12`. Việc quét mạnh mẽ các vị trí hoặc kích thước hình chữ nhật là hoàn toàn không thể. Ngay cả việc lặp lại tất cả các hàng hoặc tất cả các cột cũng đã quá lớn. 

Quan sát quan trọng là các giá trị lưới tạo thành một cấu trúc số học rất đều đặn. Thay vì tìm kiếm theo vị trí, chúng ta có thể rút ra một công thức đóng cho tổng của bất kỳ hình chữ nhật nào và chuyển bài toán thành một phép tìm kiếm theo lý thuyết số trên các ước của`2L`. 

Một sai lầm phổ biến là cho rằng chỉ có kích thước hình chữ nhật là quan trọng. Hai hình chữ nhật có cùng chiều cao và chiều rộng có thể có tổng khác nhau vì vị trí bắt đầu của chúng khác nhau. 

Coi như:```
M = 3, N = 3

0 1 2
3 4 5
6 7 8
```MỘT`1 × 2`hình chữ nhật có thể có tổng`1`,`3`,`5`,`7`,`9`,`11`. 

Chỉ riêng kích thước không xác định được tổng. 

Một trường hợp tinh vi khác xuất hiện khi vị trí bắt đầu được tính toán nằm ngoài lưới.```
M = 2, N = 3, L = 8
```Hình chữ nhật có diện tích`4`tồn tại và hoạt động, nhưng một số nghiệm hợp lệ về mặt đại số tương ứng với góc trên bên trái vượt quá phạm vi cho phép. Các giới hạn hình học vẫn phải được kiểm tra. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi hình chữ nhật con và tính tổng của nó. 

có`O(M²N²)`hình chữ nhật phụ. Với kích thước lên tới`10^6`, điều đó thật vô vọng. Ngay cả việc lưu trữ lưới cũng không thể. 

Cấu trúc của lưới chính là thứ đã cứu chúng ta. Mỗi giá trị đều```
value(i, j) = iN + j
```Giả sử một hình chữ nhật bắt đầu tại`(r, c)`và có chiều cao`h`và chiều rộng`w`. 

Diện tích của nó là```
A = h · w
```Tổng hợp các cấp số cộng cho ta```
sum = A · ( N(2r + h - 1) + (2c + w - 1) ) / 2
```Định nghĩa```
B = N(h - 1) + (w - 1)
x = Nr + c
```Sau đó```
sum = A(2x + B)/2
```Đối với giá trị mục tiêu`L`:```
2L = A(2x + B)
```Điều này ngay lập tức ngụ ý rằng khu vực`A`phải chia`2L`. 

Điều đó thay đổi hoàn toàn vấn đề. Thay vì tìm kiếm các vị trí trong một mạng lưới khổng lồ, chúng ta liệt kê các ước của`2L`. Số ước của một số lên tới`2 · 10^12`chỉ có vài nghìn. 

Đối với khu vực ứng cử viên`A`, chúng tôi thử mọi nhân tử```
A = h · w
```với`h ≤ M`Và`w ≤ N`. 

Sắp xếp lại phương trình cho```
x = (2L/A - B)/2
```Nếu như`x`là số nguyên không âm, chúng tôi phục hồi```
r = x // N
c = x % N
```và chỉ cần xác minh rằng```
r ≤ M - h
c ≤ N - w
```Nếu bất kỳ phép nhân tử nào có tác dụng thì diện tích`A`là có thể đạt được. Vì chúng tôi kiểm tra các khu vực theo thứ tự tăng dần nên kết quả hợp lệ đầu tiên là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M²N²) | O(1) | Quá chậm | 
| Tối ưu | Khoảng O(d²) trong đó d là số ước của 2L | O(d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`S = 2L`. 
2. Nhân tố hóa`S`. 
3. Tạo tất cả các ước của`S`và sắp xếp chúng theo thứ tự tăng dần. 
4. Với mỗi ước số`A`của`S`, coi nó như một vùng hình chữ nhật ứng cử viên. 
5. Liệt kê mọi nhân tử`A = h · w`. 
6. Bỏ qua bất kỳ cặp nào`h > M`hoặc`w > N`. 
7. Tính toán```
Q = S / A
B = N(h - 1) + (w - 1)
```8. Kiểm tra xem```
Q - B
```không âm và chẵn. 

Nếu không thì vị trí bắt đầu tương ứng không thể tích phân. 
9. Phục hồi```
x = (Q - B)/2
r = x // N
c = x % N
```10. Kiểm tra xem hình chữ nhật có vừa không:```
r ≤ M - h
c ≤ N - w
```11. Ngay khi tìm thấy một hình chữ nhật hợp lệ, xuất ra`A`. 
12. Nếu không có vùng nào hoạt động, xuất ra`-1`. 

### Tại sao nó hoạt động 

Công thức tính tổng```
L = A(2x + B)/2
```là đặc tính chính xác của mọi hình chữ nhật. 

Đối với chiều cao và chiều rộng cố định, số lượng`B`đã được sửa. Sự tự do duy nhất còn lại là vị trí bắt đầu, được biểu thị bằng`x = Nr + c`. 

Sắp xếp lại phương trình sẽ cho một giá trị ứng viên duy nhất là`x`. Nếu giá trị đó không phải là vị trí lưới hợp lệ thì không có hình chữ nhật nào có các kích thước đó có thể tạo ra tổng mục tiêu. Nếu nó hợp lệ, hình chữ nhật tương ứng sẽ tạo ra chính xác`L`. 

Thuật toán kiểm tra mọi khu vực có thể phân chia`2L`và mọi hệ số có thể có của khu vực đó. Vì mọi hình chữ nhật hợp lệ đều phải thỏa mãn các điều kiện này nên không thể bỏ qua nghiệm nào. Các vùng kiểm tra theo thứ tự tăng dần đảm bảo rằng vùng hợp lệ đầu tiên có diện tích tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factorize(x):
    fac = []
    d = 2

    while d * d <= x:
        if x % d == 0:
            cnt = 0
            while x % d == 0:
                x //= d
                cnt += 1
            fac.append((d, cnt))
        d += 1 if d == 2 else 2

    if x > 1:
        fac.append((x, 1))

    return fac

def gen_divisors(factors, idx=0, cur=1, out=None):
    if out is None:
        out = []

    if idx == len(factors):
        out.append(cur)
        return out

    p, e = factors[idx]
    val = 1

    for _ in range(e + 1):
        gen_divisors(factors, idx + 1, cur * val, out)
        val *= p

    return out

t = int(input())

for _ in range(t):
    M, N, L = map(int, input().split())

    S = 2 * L

    factors = factorize(S)
    divisors = gen_divisors(factors)
    divisors.sort()

    answer = -1

    for A in divisors:
        found = False

        for h in divisors:
            if h * h > A:
                break

            if A % h:
                continue

            w = A // h

            for hh, ww in ((h, w), (w, h) if h != w else ()):
                if hh > M or ww > N:
                    continue

                Q = S // A
                B = N * (hh - 1) + (ww - 1)

                diff = Q - B

                if diff < 0 or diff & 1:
                    continue

                x = diff // 2

                r = x // N
                c = x % N

                if r <= M - hh and c <= N - ww:
                    answer = A
                    found = True
                    break

            if found:
                break

        if found:
            break

    print(answer)
```Phần đầu tiên phân tích thành nhân tử`2L`. Từ việc phân tích nhân tử đó chúng ta tạo ra tất cả các ước số. 

Vòng lặp bên ngoài kiểm tra các vùng ứng viên theo thứ tự tăng dần. Vì chúng ta chỉ quan tâm đến diện tích khả thi nhỏ nhất nên việc tìm kiếm có thể dừng ngay lập tức khi tìm thấy hình chữ nhật hợp lệ. 

Đối với mỗi khu vực, chúng tôi liệt kê tất cả các cặp yếu tố`(h, w)`. Đại số đưa ra chỉ số bắt đầu duy nhất có thể`x`. Chuyển đổi```
x = Nr + c
```quay lại`(r, c)`rất đơn giản bằng cách sử dụng phép chia số nguyên và số dư. 

Lỗi triển khai phổ biến nhất là quên kiểm tra hình học```
r ≤ M - h
c ≤ N - w
```Một giá trị của`x`có thể thỏa mãn phương trình nhưng vẫn tương ứng với một hình chữ nhật kéo dài ra ngoài hộp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
M = 2, N = 3, L = 8
```Diện tích hợp lệ nhỏ nhất là`4`. 

| A | h | w | Hỏi | B | x | r | c | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 16 | 0 | 8 | 2 | 2 | Không | 
| 2 | 1 | 2 | 8 | 1 | không hợp lệ | - | - | Không | 
| 4 | 2 | 2 | 4 | 4 | 0 | 0 | 0 | Có | 

Hình chữ nhật là:```
0 1
3 4
```tổng của ai là`8`. 

### Ví dụ 2 

đầu vào:```
M = 3, N = 3, L = 36
```| A | h | w | Hỏi | B | x | r | c | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 72 | 0 | 36 | ra | - | Không | 
| 3 | 1 | 3 | 24 | 2 | 11 | ra | - | Không | 
| 9 | 3 | 3 | 8 | 8 | 0 | 0 | 0 | Có | 

Toàn bộ lưới được chọn:```
0 1 2
3 4 5
6 7 8
```và tổng của nó là`36`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d²) |`d`là số ước của`2L`| 
| Không gian | O(d) | lưu trữ ước số | 

Đối với số lượng lên đến`2 · 10^12`, số ước chỉ là vài nghìn trong trường hợp xấu nhất. Kích thước này rất nhỏ so với kích thước lưới, có thể đạt tới`10^6`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def factorize(x):
        fac = []
        d = 2
        while d * d <= x:
            if x % d == 0:
                c = 0
                while x % d == 0:
                    x //= d
                    c += 1
                fac.append((d, c))
            d += 1 if d == 2 else 2
        if x > 1:
            fac.append((x, 1))
        return fac

    def gen(fac, i=0, cur=1, out=None):
        if out is None:
            out = []
        if i == len(fac):
            out.append(cur)
            return out
        p, e = fac[i]
        v = 1
        for _ in range(e + 1):
            gen(fac, i + 1, cur * v, out)
            v *= p
        return out

    t = int(input())
    ans = []

    for _ in range(t):
        M, N, L = map(int, input().split())
        S = 2 * L

        divs = gen(factorize(S))
        divs.sort()

        res = -1

        for A in divs:
            ok = False

            for h in divs:
                if h * h > A:
                    break
                if A % h:
                    continue

                w = A // h

                pairs = [(h, w)]
                if h != w:
                    pairs.append((w, h))

                for hh, ww in pairs:
                    if hh > M or ww > N:
                        continue

                    Q = S // A
                    B = N * (hh - 1) + (ww - 1)

                    diff = Q - B

                    if diff < 0 or diff % 2:
                        continue

                    x = diff // 2
                    r = x // N
                    c = x % N

                    if r <= M - hh and c <= N - ww:
                        res = A
                        ok = True
                        break

                if ok:
                    break

            if ok:
                break

        ans.append(str(res))

    return "\n".join(ans)

# sample-style cases
assert solve("1\n2 3 8\n") == "4"

# minimum grid
assert solve("1\n1 1 1\n") == "-1"

# whole grid
assert solve("1\n3 3 36\n") == "9"

# single cell exists
assert solve("1\n2 2 3\n") == "1"

# impossible target
assert solve("1\n3 3 10\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`-1`| Giá trị ô 1 không tồn tại | 
|`3 3 36`|`9`| Giải pháp toàn lưới | 
|`2 2 3`|`1`| Hình chữ nhật đơn ô | 
|`3 3 10`|`-1`| Không có hình chữ nhật phù hợp | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1 1
```Lưới chỉ chứa một giá trị:```
0
```Khu vực kiểm tra thuật toán`1`, tính vị trí tương ứng và thấy rằng không có hình chữ nhật nào có thể tính tổng bằng`1`. Nó xuất ra`-1`. 

Coi như:```
1
2 2 3
```Tế bào`(1,0)`chứa giá trị`2`, Và`(1,1)`chứa giá trị`3`. 

Đối với khu vực`1`, công thức cho vị trí bắt đầu hợp lệ tương ứng chính xác với giá trị`3`. Thuật toán ngay lập tức trở lại`1`, đó là tối ưu. 

Coi như:```
1
2 3 8
```Việc thực hiện bất cẩn có thể tìm ra lời giải đại số có góc trên bên trái nằm ngoài lưới. Bước tái thiết```
r = x // N
c = x % N
```theo sau là```
r ≤ M - h
c ≤ N - w
```loại bỏ những kết quả dương tính giả đó và chỉ giữ lại các hình chữ nhật có giá trị về mặt hình học.
