---
title: "CF 162G - Tổng không thập phân"
description: "Chúng ta có một số số nguyên được viết theo cơ số tùy ý từ 2 đến 36. Các chữ số trên 9 được biểu thị bằng chữ in hoa, vì vậy trong cơ số 16, dãy chữ số tiếp tục là A, B, C, D, E, F và ở các cơ số lớn hơn, nó có thể tiếp tục đến Z."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "G"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 2000
weight: 162
solve_time_s: 106
verified: true
draft: false
---

[CF 162G - Tổng không thập phân](https://codeforces.com/problemset/problem/162/G) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số số nguyên được viết theo cơ số tùy ý từ 2 đến 36. Các chữ số trên 9 được biểu thị bằng chữ in hoa, vì vậy trong cơ số 16, dãy chữ số tiếp tục như sau`A, B, C, D, E, F`, và ở những căn cứ lớn hơn, nó có thể tiếp tục lên tới`Z`. 

Nhiệm vụ chỉ đơn giản là tính tổng của tất cả các số và in kết quả theo cùng một cơ số. 

Điều thú vị là các số đầu vào không được viết dưới dạng thập phân. Chúng ta phải giải thích chính xác từng chữ số theo cơ số đã cho, thực hiện phép cộng và sau đó chuyển đổi giá trị thập phân cuối cùng về cùng một hệ thống số. 

Những hạn chế là cực kỳ nhỏ. Có nhiều nhất 10 số và mỗi số có nhiều nhất 5 chữ số. Ngay cả trong cơ số lớn nhất, giá trị lớn nhất của một số là:$$36^5 - 1 = 60,\!466,\!175$$Tổng tổng dễ dàng phù hợp với các loại số nguyên tiêu chuẩn và dù sao thì số nguyên Python cũng xử lý các giá trị lớn hơn nhiều. Điều này có nghĩa là hiệu quả không phải là thách thức ở đây. Thách thức thực sự là thực hiện chuyển đổi cơ sở một cách chính xác. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Một lỗi phổ biến là xử lý sai các số 0 đứng đầu. Coi như:```
2
10
0012
0008
```Đầu ra đúng là:```
20
```Các số 0 đứng đầu chỉ là một phần của biểu diễn đầu vào. Chúng không ảnh hưởng đến giá trị số. 

Một sai lầm dễ mắc phải khác là quên rằng các chữ số trên 9 là các chữ cái. Ví dụ:```
2
16
A
1
```Đầu ra đúng là:```
B
```Nếu một giải pháp chỉ chuyển đổi các ký tự bằng cách sử dụng`'0'`ĐẾN`'9'`, nó sẽ thất bại trên các chữ số kiểu thập lục phân. 

Cơ sở 2 cũng nhanh chóng bộc lộ các lỗi chuyển đổi:```
2
2
1111
1
```Đầu ra đúng là:```
10000
```Điều này kiểm tra xem việc mang có được xử lý chính xác trong quá trình chuyển đổi trở lại cơ sở đích hay không. 

Một trường hợp tinh tế hơn xuất hiện khi tổng số tiền bằng 0:```
3
8
0
00
000
```Đầu ra đúng là:```
0
```Vòng lặp chuyển đổi chia liên tục trong khi giá trị là dương có thể vô tình in ra một chuỗi trống thay vì`"0"`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng thủ công việc chuyển đổi hệ thống số. 

Đối với mỗi chuỗi đầu vào, chúng ta có thể quét các chữ số từ trái sang phải và đánh giá giá trị thập phân của nó một cách chính xác giống như cách hoạt động của ký hiệu vị trí:$$(((d_1)\cdot radix + d_2)\cdot radix + d_3)\cdots$$Sau khi có được tất cả các giá trị thập phân, chúng ta cộng chúng lại với nhau. 

Cuối cùng, chúng ta chuyển đổi số thập phân kết quả trở lại cơ số đích bằng cách liên tục lấy phần còn lại theo modulo`radix`. Mỗi phần còn lại trở thành một chữ số đầu ra. 

Vì mỗi số có nhiều nhất là 5 chữ số và có nhiều nhất là 10 số nên ngay cả việc thực hiện hoàn toàn thủ công cũng chỉ thực hiện được vài trăm thao tác. 

Một giải pháp thay thế mạnh mẽ sẽ là phép cộng từng chữ số trực tiếp trong cơ sở mục tiêu, mô phỏng phép cộng sách giáo khoa có mang. Điều đó cũng có tác dụng vì các dây rất nhỏ. Chúng tôi sẽ căn chỉnh tất cả các số theo chữ số có nghĩa nhỏ nhất của chúng, tính tổng từng cột, truyền các số mang và xây dựng câu trả lời. 

Cách tiếp cận chuyển đổi cơ sở thủ công đơn giản hơn vì Python đã xử lý các số nguyên có kích thước tùy ý một cách an toàn. Thay vì triển khai logic phép cộng ở mọi cơ sở, chúng tôi giảm nhiệm vụ xuống còn: 

1. Chuyển đổi mọi số đầu vào thành số thập phân. 
2. Thêm chúng bình thường. 
3. Chuyển đổi giá trị cuối cùng trở lại. 

Cấu trúc của hệ thống số vị trí làm cho sự rút gọn này trở nên tự nhiên. Khi một số được hiểu là số nguyên, số học không còn phụ thuộc vào cách biểu diễn ban đầu của nó nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chữ số Brute Force | O(n * L) | O(L) | Đã chấp nhận | 
| Phương pháp chuyển đổi tối ưu | O(n * L) | O(L) | Đã chấp nhận | 

Đây,`L`là số chữ số tối đa, nhiều nhất là 5. 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`radix`. 
2. Tạo các hàm trợ giúp cho việc chuyển đổi chữ số. 

nhân vật`'0'`bởi vì`'9'`ánh xạ tới các giá trị`0`bởi vì`9`, trong khi`'A'`bởi vì`'Z'`bản đồ tới`10`bởi vì`35`. 
3. Chuyển từng chuỗi đầu vào sang dạng thập phân. 

Bắt đầu từ giá trị`0`. Đối với mỗi nhân vật:$$value = value \times radix + digit$$Điều này tuân theo định nghĩa của ký hiệu vị trí. 
4. Thêm tất cả các giá trị được chuyển đổi thành một số nguyên`total`. 
5. Xử lý trường hợp đặc biệt`total == 0`. 

đầu ra`"0"`ngay lập tức. Nếu không vòng chuyển đổi sẽ tạo ra một chuỗi trống. 
6. Chuyển đổi`total`trở lại căn cứ mục tiêu. 

Lặp đi lặp lại: 

- lấy`total % radix`- chuyển đổi phần còn lại thành biểu diễn ký tự của nó 
- nối nó vào 
- chia`total //= radix`Các chữ số xuất hiện theo thứ tự ngược lại vì chúng ta trích xuất các chữ số có nghĩa nhỏ nhất trước tiên. 
7. Đảo ngược các chữ số đã thu thập và in kết quả. 

### Tại sao nó hoạt động 

Thuật toán dựa trên định nghĩa vị trí của hệ thống số. 

Khi đọc một chữ số theo từng chữ số, nhân giá trị hiện tại với cơ số sẽ dịch chuyển tất cả các chữ số trước đó sang trái một vị trí. Việc thêm chữ số mới sẽ chèn chữ số có nghĩa thấp nhất tiếp theo. Sau khi xử lý toàn bộ chuỗi, số nguyên được tính toán bằng giá trị toán học được biểu thị bằng ký hiệu ban đầu. 

Việc chuyển đổi ngược lại cũng tuân theo ký hiệu vị trí. Lấy modulo`radix`trích xuất chữ số có nghĩa nhỏ nhất, vì mọi vị trí cao hơn đều chia hết cho cơ số. Phép chia lặp lại sẽ loại bỏ từng chữ số đã được xử lý cho đến khi toàn bộ biểu diễn được xây dựng lại. 

Vì cả hai chuyển đổi đều bảo toàn chính xác giá trị số nên chuỗi in cuối cùng biểu thị tổng chính xác trong cơ số ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

DIGITS = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"

def char_to_value(c):
    if '0' <= c <= '9':
        return ord(c) - ord('0')
    return ord(c) - ord('A') + 10

def to_decimal(s, radix):
    value = 0
    for ch in s:
        value = value * radix + char_to_value(ch)
    return value

def from_decimal(x, radix):
    if x == 0:
        return "0"

    result = []

    while x > 0:
        result.append(DIGITS[x % radix])
        x //= radix

    return ''.join(reversed(result))

def solve():
    n = int(input())
    radix = int(input())

    total = 0

    for _ in range(n):
        s = input().strip()
        total += to_decimal(s, radix)

    print(from_decimal(total, radix))

solve()
```Giải pháp được chia thành hai chức năng chuyển đổi độc lập.`to_decimal`diễn giải một chuỗi trong cơ số đã cho. Bước nhân dịch chuyển các chữ số đã xử lý trước đó sang một vị trí cơ sở, giống hệt như việc xây dựng các số bằng tay.`from_decimal`thực hiện phép biến đổi ngược. Hoạt động còn lại trích xuất các chữ số từ ít quan trọng nhất đến quan trọng nhất, do đó danh sách đã thu thập phải được đảo ngược ở cuối. 

các`DIGITS`chuỗi cung cấp một ánh xạ rõ ràng từ các giá trị chữ số trở lại ký tự. chỉ mục`10`trở thành`'A'`, chỉ số`15`trở thành`'F'`, vân vân. 

Việc xử lý đặc biệt cho số 0 là cần thiết. Không có nó, vòng lặp chuyển đổi sẽ không bao giờ thực thi`x == 0`, tạo ra một chuỗi trống thay vì`"0"`. 

Số nguyên Python đã hỗ trợ độ chính xác tùy ý, vì vậy việc tràn không bao giờ là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
16
F0
20B
004
```### Chuyển sang số thập phân 

| Chuỗi | Chữ số hiện tại | Giá trị trung gian | 
| --- | --- | --- | 
| F0 | F = 15 | 15 | 
| F0 | 0 = 0 | 15 × 16 + 0 = 240 | 
| 20B | 2 = 2 | 2 | 
| 20B | 0 = 0 | 2 × 16 + 0 = 32 | 
| 20B | B = 11 | 32 × 16 + 11 = 523 | 
| 004 | 0 = 0 | 0 | 
| 004 | 0 = 0 | 0 | 
| 004 | 4 = 4 | 4 | 

Tổng số: 

| Giá trị | Tổng hợp | 
| --- | --- | 
| 240 + 523 + 4 | 767 | 

### Chuyển về cơ số 16 

| Giá trị hiện tại | Phần còn lại | Chữ số | 
| --- | --- | --- | 
| 767 | 15 | F | 
| 47 | 15 | F | 
| 2 | 2 | 2 | 

Các chữ số được thu thập theo thứ tự ngược lại:`F F 2`. 

Câu trả lời cuối cùng:```
2FF
```Dấu vết này cho thấy cả hai hướng chuyển đổi đều hoạt động cùng nhau. Số 0 đứng đầu trong`"004"`biến mất một cách tự nhiên vì chúng không làm thay đổi giá trị số. 

### Ví dụ 2 

đầu vào:```
2
2
1111
1
```### Chuyển sang số thập phân 

| Chuỗi | Chữ số hiện tại | Giá trị trung gian | 
| --- | --- | --- | 
| 1111 | 1 | 1 | 
| 1111 | 1 | 3 | 
| 1111 | 1 | 7 | 
| 1111 | 1 | 15 | 
| 1 | 1 | 1 | 

Tổng số: 

| Giá trị | Tổng hợp | 
| --- | --- | 
| 15 + 1 | 16 | 

### Chuyển về cơ số 2 

| Giá trị hiện tại | Phần còn lại | Chữ số | 
| --- | --- | --- | 
| 16 | 0 | 0 | 
| 8 | 0 | 0 | 
| 4 | 0 | 0 | 
| 2 | 0 | 0 | 
| 1 | 1 | 1 | 

Đảo ngược các chữ số cho:```
10000
```Ví dụ này thể hiện sự lan truyền mang trên nhiều vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * L) | Mỗi chữ số được xử lý một lần trong quá trình phân tích cú pháp và một lần trong quá trình chuyển đổi đầu ra | 
| Không gian | O(L) | Danh sách chữ số đầu ra lưu trữ tối đa số chữ số trong câu trả lời | 

Đây,`n ≤ 10`Và`L ≤ 5`, vì vậy thời gian chạy thực tế rất nhỏ. Chương trình chỉ thực hiện vài chục phép tính số học và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

DIGITS = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"

def char_to_value(c):
    if '0' <= c <= '9':
        return ord(c) - ord('0')
    return ord(c) - ord('A') + 10

def to_decimal(s, radix):
    value = 0
    for ch in s:
        value = value * radix + char_to_value(ch)
    return value

def from_decimal(x, radix):
    if x == 0:
        return "0"

    result = []

    while x > 0:
        result.append(DIGITS[x % radix])
        x //= radix

    return ''.join(reversed(result))

def solve():
    input = sys.stdin.readline

    n = int(input())
    radix = int(input())

    total = 0

    for _ in range(n):
        total += to_decimal(input().strip(), radix)

    print(from_decimal(total, radix))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
"""3
16
F0
20B
004
"""
) == "2FF", "sample 1"

# minimum size
assert run(
"""1
2
0
"""
) == "0", "minimum case"

# carry propagation in binary
assert run(
"""2
2
1111
1
"""
) == "10000", "binary carry"

# leading zeros
assert run(
"""2
10
0012
0008
"""
) == "20", "leading zeros"

# largest digit symbols
assert run(
"""2
36
Z
1
"""
) == "10", "base 36 carry"

# many equal values
assert run(
"""4
16
FF
FF
FF
FF
"""
) == "3FC", "multiple additions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Số 0 đơn ở cơ số 2 |`0`| Xử lý đúng tổng số tiền bằng 0 | 
|`1111 + 1`ở dạng nhị phân |`10000`| Truyền bá qua nhiều chữ số | 
| Số có số 0 đứng đầu |`20`| Các số 0 đứng đầu bị bỏ qua một cách chính xác | 
|`Z + 1`ở căn cứ 36 |`10`| Chuyển đổi chữ số hợp pháp cao nhất | 
| Bốn bản sao của`FF`|`3FC`| Phép cộng lặp đi lặp lại đúng đắn | 

## Vỏ cạnh 

### Tổng số tiền bằng 0 

đầu vào:```
3
8
0
00
000
```Mọi số đều chuyển thành giá trị thập phân`0`, vậy tổng cũng là`0`. 

Trong quá trình chuyển đổi ngược, thuật toán ngay lập tức trả về`"0"`trước khi vào vòng chia.

 Đầu ra đúng:```
0
```Nếu không có trường hợp đặc biệt này, danh sách chữ số sẽ trống. 

### Số 0 đứng đầu 

đầu vào:```
2
10
0012
0008
```Dấu vết: 

| Chuỗi đã xử lý | Giá trị thập phân | 
| --- | --- | 
| 0012 | 12 | 
| 0008 | 8 | 

Tổng số trở thành`20`. 

Các số 0 đứng đầu không bao giờ ảnh hưởng đến giá trị tích lũy vì nhân số 0 với cơ số vẫn tạo ra số 0. 

Đầu ra đúng:```
20
```### Chữ số được hỗ trợ cao nhất 

đầu vào:```
2
36
Z
1
```

`Z`ánh xạ tới giá trị chữ số`35`. 

Tổng số trở thành:$$35 + 1 = 36$$Chuyển đổi`36`quay lại cơ sở 36 cho: 

| Giá trị hiện tại | Phần còn lại | 
| --- | --- | 
| 36 | 0 | 
| 1 | 1 | 

Sau khi đảo ngược, câu trả lời là:```
10
```Điều này xác nhận rằng chuyển đổi chữ cái-chữ số hoạt động chính xác ở ranh giới trên của phạm vi cơ số.
