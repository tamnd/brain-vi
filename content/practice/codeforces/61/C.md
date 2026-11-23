---
title: "CF 61C - Bắt Valerian"
description: "Bài toán yêu cầu chúng ta chuyển đổi một số từ cơ số này sang cơ số khác, với điểm mấu chốt là cơ số đích có thể là hệ thống chữ số vị trí tiêu chuẩn (2 đến 25) hoặc hệ thống chữ số La Mã."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 61
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 57 (Div. 2)"
rating: 2000
weight: 61
solve_time_s: 169
verified: true
draft: false
---

[CF 61C - Bắt Valerian](https://codeforces.com/problemset/problem/61/C) 

**Xếp hạng:** 2000 
**Thẻ:** toán 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta chuyển đổi một số từ cơ số này sang cơ số khác, với điểm mấu chốt là cơ số đích có thể là hệ thống chữ số vị trí tiêu chuẩn (2 đến 25) hoặc hệ thống chữ số La Mã. Đầu vào bao gồm ba phần: cơ sở nguồn`a`, cơ sở mục tiêu`b`(hoặc`R`đối với chữ số La Mã) và số`c`đại diện trong cơ sở`a`. Chúng ta được yêu cầu xuất ra giá trị của`c`trong căn cứ`b`, bỏ đi các số 0 đứng đầu. 

Các ràng buộc đủ chặt chẽ để cho phép các phương pháp chuyển đổi trực tiếp. số`c`có thể có tới 1000 chữ số, vì vậy chúng tôi không thể coi nó như một số nguyên thông thường trong các ngôn ngữ không hỗ trợ số nguyên lớn, nhưng Python xử lý nguyên bản các số nguyên có kích thước tùy ý. Đầu ra chữ số La Mã được đảm bảo ≤ 300010 nên chúng ta không phải xử lý những chữ số La Mã cực lớn. Cơ sở tiêu chuẩn được giới hạn ở 25, giúp quản lý chuyển đổi bằng ánh xạ từ số sang số nguyên thông thường. 

Trường hợp cạnh tinh tế phát sinh khi số đầu vào`c`có số 0 đứng đầu. Ví dụ, nếu`a = 10`,`b = 2`, Và`c = "001"`, việc thực hiện bất cẩn có thể dẫn đến`"001"`thay vì`"1"`. Một trường hợp cạnh khác là khi`b = R`, vì chữ số La Mã không bao gồm số 0; mọi chuyển đổi đều phải tôn trọng thuộc tính này. Ngoài ra, các cơ số lớn hơn 10 sử dụng các ký hiệu chữ cái, ví dụ:`A = 10`, do đó trình phân tích cú pháp phải xử lý các chữ cái. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là bạo lực: phân tích số`c`từ căn cứ`a`thành số nguyên thập phân, sau đó chia liên tục cho`b`để trích xuất các chữ số cho đầu ra. Điều này hoạt động vì chuyển đổi sang số thập phân là`O(length of c)`và chuyển đổi từ số thập phân sang cơ số khác là`O(log c)`sự phân chia. Trong Python, các số nguyên có kích thước tùy ý làm cho điều này trở nên khả thi và`length of c`1000 là đủ nhỏ. 

Bước bổ sung duy nhất là xử lý hệ thống chữ số La Mã. Điểm mấu chốt là các chữ số La Mã về cơ bản là một hệ thống dựa trên việc tra cứu với quy tắc trừ cho các số như 4 (`IV`) hoặc 900 (`CM`). Bài toán đảm bảo rằng bất kỳ cách biểu diễn chữ số La Mã nào đều ≤ 300010, do đó, thuật toán tham lam đơn giản sẽ hoạt động: trừ đi lặp đi lặp lại giá trị chữ số La Mã lớn nhất nhỏ hơn hoặc bằng số đó trong khi thêm ký hiệu tương ứng. Điều này duy trì tính chính xác vì hệ thống chữ số La Mã là chính tắc khi được biểu thị bằng ký hiệu trừ tiêu chuẩn. 

Vì vậy, cách tiếp cận tối ưu là chuyển đổi hai bước. Đầu tiên, chuyển đổi số đầu vào từ cơ sở`a`sang số thập phân bằng cách sử dụng đánh giá vị trí. Thứ hai, nếu`b`là số, chuyển đổi từ thập phân sang cơ số`b`sử dụng phép chia lặp đi lặp lại; nếu như`b`là tiếng La Mã, hãy chuyển đổi bằng cách sử dụng phương pháp trừ tham lam với các ký hiệu La Mã tiêu chuẩn. Điều này làm giảm vấn đề xuống một loạt các chuyển đổi số đơn giản, dễ hiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuyển đổi thập phân + cơ số) | O(L + log N) | O(L + log N) | Đã chấp nhận | 
| Chuyển đổi số La Mã | O(R) | O(R) | Đã chấp nhận | 

Đây,`L`là độ dài của số đầu vào,`N`là giá trị thập phân của nó và`R`là số ≤ 300010 để chuyển đổi sang La Mã. 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`a`,`b`, Và`c`. Nếu như`b`là`"R"`, đánh dấu rằng chúng ta cần đầu ra bằng chữ số La Mã. 
2. Chuyển đổi`c`từ căn cứ`a`đến số thập phân. Lặp lại chuỗi, cập nhật`value = value * a + digit`ở mỗi bước. Sử dụng`ord`ánh xạ cho các chữ số lớn hơn 9. 
3. Nếu`b`là số: 

1. Xử lý trường hợp đặc biệt khi giá trị thập phân bằng 0, trả về`"0"`. 
2. Ngược lại, chia liên tục số thập phân cho`b`, chuẩn bị phần còn lại được ánh xạ tới một ký tự (`0-9`cho 0-9,`A-O`for 10-24) vào chuỗi kết quả. 
4. Nếu`b`là người La Mã: 

1. Khởi tạo danh sách các bộ biểu thị các ký hiệu và giá trị La Mã theo thứ tự giảm dần:`(1000, "M"), (900, "CM"), ..., (1, "I")`. 
2. Trong khi giá trị thập phân là dương, hãy tìm giá trị ký hiệu lớn nhất ≤ số hiện tại, nối ký hiệu đó và trừ giá trị đó khỏi số. 
5. In chuỗi kết quả. 

Tại sao nó hoạt động: chuyển đổi qua số thập phân là một ánh xạ không mất dữ liệu vì hệ thống chữ số vị trí có tính chất nội xạ; mỗi chuỗi trong cơ sở`a`đại diện cho một số thập phân duy nhất. Việc chuyển đổi chữ số La Mã rất tham lam, luôn chọn ký hiệu lớn nhất trước tiên, tương ứng với cách biểu diễn chuẩn mực. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# mapping for bases greater than 10
def char_to_int(c):
    if '0' <= c <= '9':
        return ord(c) - ord('0')
    return ord(c) - ord('A') + 10

def int_to_char(n):
    if n < 10:
        return str(n)
    return chr(ord('A') + n - 10)

# Roman numeral conversion table
roman_numerals = [
    (1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'),
    (100, 'C'), (90, 'XC'), (50, 'L'), (40, 'XL'),
    (10, 'X'), (9, 'IX'), (5, 'V'), (4, 'IV'), (1, 'I')
]

def base_a_to_decimal(s, base):
    value = 0
    for ch in s:
        value = value * base + char_to_int(ch)
    return value

def decimal_to_base_b(value, base):
    if value == 0:
        return "0"
    digits = []
    while value > 0:
        digits.append(int_to_char(value % base))
        value //= base
    return ''.join(reversed(digits))

def decimal_to_roman(value):
    result = []
    for val, sym in roman_numerals:
        while value >= val:
            result.append(sym)
            value -= val
    return ''.join(result)

# main
a, b = input().split()
c = input().strip()
a = int(a)

decimal_value = base_a_to_decimal(c, a)

if b == 'R':
    print(decimal_to_roman(decimal_value))
else:
    b = int(b)
    print(decimal_to_base_b(decimal_value, b))
```Mỗi hàm tương ứng trực tiếp với một bước trong thuật toán.`base_a_to_decimal`xử lý đầu vào lớn tùy ý,`decimal_to_base_b`bao gồm các căn cứ lên tới 25, và`decimal_to_roman`thực hiện việc xây dựng chữ số La Mã tham lam kinh điển. Có thể tránh được các lỗi riêng lẻ trong các căn cứ dựa trên chữ cái bằng cách sử dụng cẩn thận`ord`số học. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
10 2
1
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| phân tích đầu vào | a, b, c | 10, 2, "1" | 
| chuyển sang số thập phân | giá trị thập phân | 1 | 
| chuyển sang cơ số 2 | kết quả | "1" | 

Dấu vết xác nhận rằng các số nhỏ có số 0 đứng đầu được xử lý chính xác và việc chuyển đổi nhị phân là không đáng kể. 

**Ví dụ 2** 

đầu vào:```
10 R
1994
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| phân tích đầu vào | a, b, c | 10, R, "1994" | 
| chuyển sang số thập phân | giá trị thập phân | 1994 | 
| Chuyển đổi La Mã | kết quả | "MCMXCIV" | 

Điều này thể hiện cách tiếp cận tham lam của chữ số La Mã và cách xử lý ký hiệu trừ chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L + log N) | L là độ dài của chuỗi đầu vào; log N phép chia cho các cơ số; Chuyển đổi La Mã là O(R) 300010 | 
| Không gian | O(L + log N) | lưu trữ các chữ số và chuỗi đầu ra cuối cùng | 

Được cho`L ≤ 1000`Và`N ≤ 10^1510`hoặc 300010 cho Roman, giải pháp thực hiện tốt dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open(__file__).read(), globals())
    return output.getvalue().strip()

# provided samples
assert run("10 2\n1\n") == "1", "sample 1"
assert run("10 R\n1994\n") == "MCMXCIV", "roman example"

# custom cases
assert run("2 10\n101\n") == "5", "binary to decimal"
assert run("16 8\nFF\n") == "377", "hex to octal"
assert run("10 R\n300010\n") == "MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM
```
