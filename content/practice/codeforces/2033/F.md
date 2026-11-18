---
title: "CF 2033F - Con lười của Kosuke"
description: "Đối với số nguyên cố định $k$, hãy xem chuỗi Fibonacci và đánh dấu mọi vị trí có giá trị Fibonacci chia hết cho $k$. Ví dụ: khi $k=2$: $$1,1,mathbf{2},3,5,mathbf{8},13,21,mathbf{34},dots$$ Các vị trí được đánh dấu là $3,6,9,dots$."
date: "2026-06-08T11:44:10+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2033
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 981 (Div. 3)"
rating: 1800
weight: 2033
solve_time_s: 121
verified: true
draft: false
---

[CF 2033F - Con lười của Kosuke](https://codeforces.com/problemset/problem/2033/F) 

**Đánh giá:** 1800 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với một số nguyên cố định$k$, nhìn vào dãy Fibonacci và đánh dấu mọi vị trí có giá trị Fibonacci chia hết cho$k$. 

Ví dụ, khi$k=2$:$$1,1,\mathbf{2},3,5,\mathbf{8},13,21,\mathbf{34},\dots$$Các vị trí được đánh dấu là$3,6,9,\dots$. 

Định nghĩa$G(n,k)$như vị trí của$n$-thứ được đánh dấu số Fibonacci. Chúng tôi được trao$n$Và$k$, và phải tính$G(n,k)$modulo$10^9+7$. 

Khó khăn chính đó là$n$có thể lớn như$10^{18}$. Chúng ta không thể tạo ra các số Fibonacci hoặc đếm từng số hạng chia hết. Thậm chí một$O(n)$thuật toán là hoàn toàn không thể. 

Hạn chế quan trọng khác là$k \le 10^5$, và tổng của tất cả$k$giá trị trên các trường hợp thử nghiệm là nhiều nhất$10^6$. Điều này gợi ý rõ ràng rằng chúng ta nên chi tiêu khoảng$O(k)$làm việc trên mỗi trường hợp thử nghiệm và khai thác một số cấu trúc chỉ phụ thuộc vào$k$. 

Trường hợp cạnh tinh tế xuất hiện khi$k=1$. Mọi số Fibonacci đều chia hết cho$1$, vì vậy các vị trí được đánh dấu chỉ đơn giản là$1,2,3,\dots$. Như vậy$G(n,1)=n$. Bất kỳ giải pháp nào giả định các sự kiện chia hết đều có thể thất bại ở đây. 

Ví dụ:```
Input:
1
5 1
```Câu trả lời đúng là:```
5
```Một sai lầm dễ mắc phải khác là tìm kiếm$n$-thứ Fibonacci chia trực tiếp. Coi như:```
Input:
1
1000000000000000000 2
```Câu trả lời tồn tại ở một chỉ số xung quanh$3 \times 10^{18}$. Mô phỏng từng vị trí một là vô vọng. 

Cạm bẫy thứ ba là quên rằng câu trả lời cuối cùng là bắt buộc theo modulo$10^9+7$. Chỉ số thực tế$G(n,k)$có thể lớn hơn nhiều so với giới hạn 64-bit khi$n$là rất lớn. Modulo phải được áp dụng bằng cách sử dụng số học mô-đun thay vì xây dựng giá trị đầy đủ. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tạo ra các số Fibonacci modulo$k$, kiểm tra xem mỗi số hạng có chia hết cho$k$và đếm xem có bao nhiêu vị trí như vậy đã xuất hiện. Ngay khi số đếm đạt đến$n$, chúng tôi xuất chỉ mục hiện tại. 

Điều này đúng vì phép chia cho$k$chỉ phụ thuộc vào giá trị Fibonacci modulo$k$. Vấn đề là ở chỗ đó$n$có thể$10^{18}$, vì vậy chúng tôi có thể cần xử lý xung quanh$10^{18}$các vị trí. Đó là nhiều mệnh lệnh lớn hơn những gì có thể. 

Quan sát quan trọng là các số Fibonacci modulo$k$mang tính định kỳ. Trình tự$$(F_i \bmod k)$$được gọi là dãy Pisano. Vì mỗi trạng thái được xác định bởi cặp$$(F_i \bmod k,\; F_{i+1} \bmod k),$$chỉ có$k^2$các trạng thái có thể. Cuối cùng, một trạng thái sẽ lặp lại và khi một trạng thái lặp lại thì toàn bộ chuỗi sẽ lặp lại. 

Trên thực tế, dãy Fibonacci modulo$k$luôn quay trở lại cặp bắt đầu$(0,1)$. Độ dài của chu kỳ này là thời kỳ Pisano. 

Trong một thời kỳ, các vị trí ở đó$F_i \equiv 0 \pmod k$tạo thành một mẫu lặp đi lặp lại. Hãy để vị trí đầu tiên như vậy là$z$. Một tính chất cổ điển của dãy Fibonacci nói rằng mọi chỉ số chia hết cho$z$cũng cho bội số của$k$và không có chỉ số dương nào nhỏ hơn. Do đó, tất cả các vị trí có thể chia được đều$$z,\;2z,\;3z,\dots$$các$n$-Số Fibonacci chia được thứ 2 do đó nằm ở vị trí$$n \cdot z.$$Toàn bộ bài toán quy về việc tìm chỉ số dương nhỏ nhất$z$như vậy$$F_z \equiv 0 \pmod k.$$Kể từ thời kỳ Pisano cho$k \le 10^5$nhiều nhất là$6k$, chúng ta có thể tìm chỉ số này bằng cách mô phỏng các số Fibonacci modulo$k$cho đến khi số 0 đầu tiên xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(G(n,k))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(k)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`MOD = 1_000_000_007`. 
2. Đối với trường hợp thử nghiệm$(n,k)$, tạo số Fibonacci modulo$k$. 
3. Bắt đầu với cặp$(1,1)$, tương ứng với$F_1$Và$F_2$. 
4. Tính toán nhiều lần modulo dư lượng Fibonacci tiếp theo$k$, trong khi theo dõi chỉ mục của nó. 
5. Chỉ số đầu tiên$z$trong đó phần dư trở thành 0 là chỉ số dương nhỏ nhất thỏa mãn$F_z \equiv 0 \pmod k$. 

Chỉ số này thường được gọi là thứ hạng xuất hiện của$k$. 
6. Mọi số Fibonacci chia hết cho$k$xảy ra tại một chỉ số là bội số của$z$. 
7. Các$n$-Số Fibonacci chia được thứ 2 do đó nằm ở chỉ số$n \cdot z$. 
8. Đầu ra$$(n \bmod MOD)\cdot(z \bmod MOD)\bmod MOD.$$### Tại sao nó hoạt động 

hãy để$z$là chỉ số dương nhỏ nhất với$F_z \equiv 0 \pmod k$. 

Thuộc tính chia hết Fibonacci tiêu chuẩn cho biết rằng$$\gcd(F_a,F_b)=F_{\gcd(a,b)}.$$Sử dụng thực tế này, người ta có thể chỉ ra rằng nếu$F_m$chia hết cho$k$, sau đó$z$chia rẽ$m$. Nếu không thì$F_{\gcd(z,m)}$cũng sẽ chia hết cho$k$, mâu thuẫn với tính tối thiểu của$z$. 

Ngược lại, số Fibonacci thỏa mãn$$F_z \mid F_{tz}$$với mọi số nguyên dương$t$. Từ$F_z$đã chia hết cho$k$, mọi$F_{tz}$chia hết cho$k$cũng vậy. 

Thus the divisible positions are exactly$$z,2z,3z,\dots$$và$n$-vị trí đó là$nz$. Thuật toán tìm$z$và tính giá trị này theo modulo$10^9+7$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def first_zero_index(k):
    if k == 1:
        return 1

    a, b = 1 % k, 1 % k
    idx = 2

    while True:
        a, b = b, (a + b) % k
        idx += 1

        if b == 0:
            return idx

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())

        z = first_zero_index(k)
        ans.append(str((n % MOD) * (z % MOD) % MOD))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```chức năng`first_zero_index`tìm kiếm chỉ số Fibonacci đầu tiên có giá trị chia hết cho`k`. Các biến`a`Và`b`luôn biểu diễn hai phần dư Fibonacci liên tiếp theo modulo`k`. 

Chỉ số bắt đầu lúc`2`bởi vì`a`Và`b`ban đầu tương ứng với`F1`Và`F2`. Sau mỗi lần chuyển đổi,`b`trở thành giá trị Fibonacci tiếp theo, vì vậy khi`b == 0`, giá trị hiện tại của`idx`chính xác là chỉ số mong muốn. 

Trường hợp đặc biệt`k == 1`được xử lý riêng vì mọi số Fibonacci đều chia hết cho`1`, làm cho chỉ số câu trả lời bằng`1`. 

Phép nhân cuối cùng sử dụng số học mô-đun ngay lập tức. Từ`n`có thể lớn như`10^18`, trước tiên tính toán modulo giữ tất cả các giá trị ở mức nhỏ và tránh các số nguyên lớn không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
```Chúng ta tìm số Fibonacci đầu tiên chia hết cho`2`. 

| Chỉ mục | Fibonacci mod 2 | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 

Chúng tôi tìm thấy: 

| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| z | 3 | 
| Trả lời | 3 × 3 = 9 | 

Đầu ra:```
9
```Điều này thể hiện mô hình cơ bản. Một lần lập chỉ mục`3`được tìm thấy, tất cả các vị trí chia hết đều là bội số của`3`. 

### Ví dụ 2 

đầu vào:```
100 1
```Vì mọi số Fibonacci đều chia hết cho`1`: 

| Biến | Giá trị | 
| --- | --- | 
| z | 1 | 
| n | 100 | 
| Trả lời | 100 | 

Đầu ra:```
100
```Đây là trường hợp đặc biệt khi vị trí chia hết đầu tiên xảy ra ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k)$mỗi trường hợp thử nghiệm | Số 0 đầu tiên xuất hiện trong chu trình Pisano, có độ dài tối đa là$6k$| 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Tổng của tất cả$k$giá trị nhiều nhất là$10^6$, do đó tổng lượng mô phỏng Fibonacci trên tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def first_zero_index(k):
        if k == 1:
            return 1

        a, b = 1 % k, 1 % k
        idx = 2

        while True:
            a, b = b, (a + b) % k
            idx += 1
            if b == 0:
                return idx

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        z = first_zero_index(k)
        out.append(str((n % MOD) * (z % MOD) % MOD))

    return "\n".join(out)

# provided samples
assert run(
"""3
3 2
100 1
1000000000000 1377
"""
) == """9
100
999244007"""

# minimum values
assert run(
"""1
1 1
"""
) == "1"

# first divisible position for k=2 is 3
assert run(
"""1
1 2
"""
) == "3"

# second divisible position for k=2 is 6
assert run(
"""1
2 2
"""
) == "6"

# large n with easy k
assert run(
"""1
1000000000000000000 1
"""
) == str(1000000000000000000 % MOD)
```### Tóm tắt thử nghiệm tùy chỉnh 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào nhỏ nhất có thể | 
|`1 2`|`3`| Vị trí Fibonacci chia hết đầu tiên | 
|`2 2`|`6`| Bội số của chỉ số 0 đầu tiên | 
|`10^18 1`|`10^18 mod MOD`| To lớn`n`, trường hợp đặc biệt`k=1`| 

## Vỏ cạnh 

Hãy xem xét:```
1
5 1
```Mọi số Fibonacci đều chia hết cho`1`. Thuật toán ngay lập tức trở lại`z = 1`. Câu trả lời trở thành`5 × 1 = 5`. Không cần tạo trình tự. 

Coi như:```
1
1 2
```Chuỗi Fibonacci modulo`2`là:```
1, 1, 0, 1, 1, 0, ...
```Số 0 đầu tiên xảy ra tại chỉ mục`3`, Vì thế`z = 3`. Câu trả lời là`1 × 3 = 3`. Điều này xác minh rằng số Fibonacci chia hết đầu tiên không nhất thiết phải ở chỉ số`1`. 

Coi như:```
1
1000000000000000000 2
```Giống nhau`z = 3`được tìm thấy. Thuật toán tính toán$$(10^{18} \bmod 10^9+7)\cdot 3 \bmod 10^9+7$$mà không bao giờ xây dựng chỉ số khổng lồ$3 \times 10^{18}$. Đây chính xác là lý do tại sao số học mô-đun được áp dụng ở phần cuối.
