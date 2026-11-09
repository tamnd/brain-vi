---
title: "CF 1475B - Số đầu năm mới"
description: "Chúng ta được cho một số $n$, và chúng ta muốn biết liệu nó có thể được xây dựng bằng cách cộng một số số năm 2020 và một số số năm 2021 lại với nhau hay không."
date: "2026-06-11T00:08:36+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1475
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 697 (Div. 3)"
rating: 900
weight: 1475
solve_time_s: 221
verified: true
draft: false
---

[CF 1475B - Con số năm mới](https://codeforces.com/problemset/problem/1475/B) 

**Xếp hạng:** 900 
**Tags:** vũ lực, dp, toán học 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$, và chúng tôi muốn biết liệu nó có thể được xây dựng bằng cách cộng một số số năm 2020 và một số số năm 2021 lại với nhau hay không. 

Nếu chúng ta chọn$a$bản sao của năm 2020 và$b$bản sao của năm 2021 thì giá trị thu được là$$2020a + 2021b$$Nhiệm vụ đơn giản là xác định xem có tồn tại số nguyên không âm hay không$a$Và$b$tạo ra cái đã cho$n$. 

Đầu vào chứa tối đa$10^4$trường hợp thử nghiệm. Mỗi giá trị của$n$nhiều nhất là$10^6$. Vì có thể có nhiều trường hợp thử nghiệm nên chúng tôi muốn một thuật toán về cơ bản có thời gian không đổi cho mỗi truy vấn. Một cách tiếp cận chỉ thực hiện một số phép tính số học cho mỗi trường hợp thử nghiệm là lý tưởng. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
1
2020
```Câu trả lời đúng là:```
YES
```bởi vì một bản sao của năm 2020 được cho phép. Giải pháp yêu cầu nhầm cả hai số xuất hiện ít nhất một lần sẽ không thành công. 

Coi như:```
1
2019
```Câu trả lời đúng là:```
NO
```bởi vì mọi số có thể sử dụng được ít nhất là 2020. Mọi kết hợp dương ít nhất phải là 2020. 

Hãy xem xét:```
1
2021
```Câu trả lời đúng là:```
YES
```bởi vì một năm 2021 là hợp lệ. 

Một trường hợp tế nhị hơn là:```
1
4040
```Câu trả lời đúng là:```
YES
```từ$4040 = 2020 + 2020$. Một giải pháp bất cẩn chỉ kiểm tra khả năng chia hết vào năm 2021 sẽ bác bỏ nó một cách sai lầm. 

Thách thức chính là tìm ra đặc tính toán học giúp tránh việc tìm kiếm thông qua nhiều kết hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Chúng ta có thể thử mọi con số có thể của năm 2021. Giả sử chúng ta chọn$b$bản 2021. Giá trị còn lại là$$n - 2021b$$Nếu số dư đó không âm và chia hết cho 2020 thì chúng ta đã tìm được biểu diễn hợp lệ. 

Phương pháp này đúng vì mọi biểu diễn có thể có đều tương ứng với một giá trị duy nhất của$b$, và chúng tôi kiểm tra tất cả chúng. 

Vấn đề là hiệu quả. Vì$n = 10^6$, số lượng giá trị có thể có của$b$đại khái là$$\frac{10^6}{2021} \approx 495.$$Thậm chí nó thực sự đủ nhỏ cho vấn đề này, vì vậy giải pháp vũ lực sẽ được chấp nhận. Nhưng có một quan sát rõ ràng hơn có thể rút gọn toàn bộ câu hỏi thành một vài phép tính số học. 

Viết lại biểu thức:$$n = 2020a + 2021b$$Từ$2021 = 2020 + 1$,$$n = 2020a + (2020+1)b$$

$$n = 2020(a+b) + b$$Cho phép$$k = a+b.$$Sau đó$$n = 2020k + b.$$Khi chia$n$đến năm 2020:$$n = 2020 \left\lfloor \frac{n}{2020} \right\rfloor + (n \bmod 2020).$$So sánh với hình thức trước, chúng ta thấy rằng$b$phải bằng số dư. 

Phần còn lại được cố định:$$b = n \bmod 2020.$$Thương số là:$$k = \left\lfloor \frac{n}{2020} \right\rfloor.$$Từ$a = k-b$, chúng tôi cần$$k \ge b.$$Thay thế thương và số dư ta có một điều kiện rất đơn giản:$$\left\lfloor \frac{n}{2020} \right\rfloor \ge n \bmod 2020.$$Nếu bất đẳng thức này đúng thì câu trả lời là CÓ. Ngược lại, câu trả lời là KHÔNG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n/2020)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị$n$. 
2. Tính thương:$$q = n // 2020$$Điều này thể hiện giá trị tối đa có thể có của$a+b$. 
3. Tính số dư:$$r = n \% 2020$$Mọi biểu diễn hợp lệ đều phải sử dụng chính xác$r$bản sao của năm 2021. 
4. Kiểm tra xem$$q \ge r.$$Nếu đúng thì ta có thể chọn$b=r$Và$a=q-r$, cả hai đều không âm. 
5. Đầu ra`"YES"`khi bất đẳng thức giữ nguyên, nếu không thì xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Mọi biểu diễn đều có thể được viết dưới dạng$$n = 2020(a+b) + b.$$Số dư sau khi chia cho năm 2020 được xác định duy nhất nên mọi biểu diễn hợp lệ đều phải có$$b = n \bmod 2020.$$Thương số cho$$a+b = n // 2020.$$Để có giải pháp hợp lý,$a$phải không âm:$$a = (n // 2020) - (n \bmod 2020) \ge 0.$$Điều kiện này vừa cần vừa đủ. Nếu nó đúng, chúng ta có thể xây dựng một cách rõ ràng$a$Và$b$. Nếu thất bại, không có sự lựa chọn không âm của$a$tồn tại. Như vậy thuật toán là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        q = n // 2020
        r = n % 2020

        if q >= r:
            ans.append("YES")
        else:
            ans.append("NO")

    sys.stdout.write("\n".join(ans))

solve()
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

thương số`q`đại diện cho tổng số số hạng nếu mỗi năm 2021 được coi là năm 2020 cộng thêm 1. Phần còn lại`r`biểu thị số lượng số 1 bổ sung đó là cần thiết, chính xác là số lượng của năm 2021. 

Điều kiện duy nhất để xác minh là liệu có đủ tổng số điều khoản để đáp ứng những điều khoản đó hay không.`r`bản sao của năm 2021. Đó chính xác là séc`q >= r`. 

Không có lo ngại tràn vì giá trị lớn nhất của`n`chỉ là$10^6$, thấp hơn nhiều so với giới hạn số nguyên của Python. Việc triển khai chỉ thực hiện phép chia số nguyên, modulo và so sánh cho từng trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4041
```| n | q = n // 2020 | r = n % 2020 | q ≥ r | Trả lời | 
| --- | --- | --- | --- | --- | 
| 4041 | 2 | 1 | Có | CÓ | 

Đây$r=1$, nên phải dùng đúng 1 bản 2021. Từ$q=2$, chúng ta có đủ tổng số điều khoản. Việc xây dựng là$2020 + 2021$. 

### Ví dụ 2 

đầu vào:```
8079
```| n | q = n // 2020 | r = n % 2020 | q ≥ r | Trả lời | 
| --- | --- | --- | --- | --- | 
| 8079 | 3 | 2019 | Không | KHÔNG | 

Số còn lại là năm 2019, cần có bản 2019 của năm 2021. Nhưng thương chỉ là 3, nghĩa là$a+b=3$. Hai sự thật đó không thể đều đúng nên không có đại diện nào tồn tại. 

Ví dụ này thể hiện tính bất biến của khóa. Phần còn lại ấn định số lượng yêu cầu là 2021 và nếu con số đó vượt quá tổng số điều khoản có sẵn thì việc trình bày là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Một phép chia, một modulo, một so sánh | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Với nhiều nhất$10^4$trường hợp thử nghiệm thì tổng công việc chỉ là vài chục nghìn phép tính số học. Điều này là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        q = n // 2020
        r = n % 2020
        ans.append("YES" if q >= r else "NO")

    return "\n".join(ans)

# provided sample
assert run(
"""5
1
4041
4042
8081
8079
"""
) == (
"""NO
YES
YES
YES
NO"""
), "sample 1"

# minimum value
assert run(
"""1
1
"""
) == "NO", "smallest possible n"

# exact 2020
assert run(
"""1
2020
"""
) == "YES", "single 2020"

# exact 2021
assert run(
"""1
2021
"""
) == "YES", "single 2021"

# boundary near first achievable range
assert run(
"""2
2019
2022
"""
) == (
"""NO
NO"""
), "boundary values"

# maximum constraint
assert run(
"""1
1000000
"""
) == "YES", "largest n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO`| Giá trị dưới 2020 là không thể | 
|`2020`|`YES`| Tác phẩm đơn 2020 | 
|`2021`|`YES`| Tác phẩm đơn năm 2021 | 
|`2019`,`2022`|`NO`,`NO`| Hành vi ranh giới xung quanh các số hợp lệ đầu tiên | 
|`1000000`|`YES`| Đầu vào lớn gần giới hạn ràng buộc | 

## Vỏ cạnh 

Hãy xem xét:```
1
2019
```Thuật toán tính toán: 

| q | r | 
| --- | --- | 
| 0 | 2019 | 

Từ$0 < 2019$, câu trả lời là KHÔNG. Điều này đúng vì mọi khối xây dựng có sẵn ít nhất là năm 2020. 

Hãy xem xét:```
1
2020
```Thuật toán tính toán: 

| q | r | 
| --- | --- | 
| 1 | 0 | 

Từ$1 \ge 0$, câu trả lời là CÓ. Kết quả xây dựng là một bản sao của năm 2020 và không có bản sao nào của năm 2021. 

Hãy xem xét:```
1
2021
```Thuật toán tính toán: 

| q | r | 
| --- | --- | 
| 1 | 1 | 

Từ$1 \ge 1$, câu trả lời là CÓ. Đây$a=0$Và$b=1$, đưa ra chính xác là năm 2021. 

Hãy xem xét:```
1
4040
```Thuật toán tính toán: 

| q | r | 
| --- | --- | 
| 2 | 0 | 

Từ$2 \ge 0$, câu trả lời là CÓ. Việc xây dựng là$2020 + 2020$. Trường hợp này xác nhận rằng chỉ sử dụng một trong hai số được phép là hoàn toàn hợp lệ. 

Coi như:```
1
8079
```Thuật toán tính toán: 

| q | r | 
| --- | --- | 
| 3 | 2019 | 

Từ$3 < 2019$, câu trả lời là KHÔNG. Phần còn lại buộc phải có nhiều bản sao của năm 2021 hơn mức thương số cho phép, khiến cho bất kỳ sự thể hiện nào đều không thể thực hiện được.
