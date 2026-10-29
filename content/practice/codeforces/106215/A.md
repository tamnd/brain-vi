---
title: "CF 106215A - Một sự trùng hợp đáng tiếc"
description: "Chúng ta được cho n từ. Đối với mỗi từ, chúng ta phải kiểm tra xem đó có chính xác là chuỗi \"WY\" hay không. Nếu một từ chính xác là \"WY\", chúng tôi sẽ thay thế nó bằng \"Whitney Young\" ở đầu ra. Mọi từ khác phải được in không thay đổi. Đầu vào bao gồm một số nguyên n, theo sau là n chuỗi."
date: "2026-06-25T06:50:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106215
codeforces_index: "A"
codeforces_contest_name: "2025-2026 Whitney Young Practice Contest 1"
rating: 0
weight: 106215
solve_time_s: 40
verified: true
draft: false
---

[CF 106215A - Một sự trùng hợp đáng tiếc](https://codeforces.com/problemset/problem/106215/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`n`từ. Đối với mỗi từ, chúng ta phải kiểm tra xem nó có chính xác là chuỗi không`"WY"`. 

Nếu một từ là chính xác`"WY"`, chúng tôi thay thế nó bằng`"Whitney Young"`ở đầu ra. Mọi từ khác phải được in không thay đổi. 

Đầu vào bao gồm một số nguyên`n`, theo sau là`n`dây. Mỗi chuỗi chỉ chứa các chữ cái tiếng Anh viết hoa và viết thường. Đầu ra chứa chính xác`n`dòng, một kết quả cho mỗi từ đầu vào. 

Những hạn chế là cực kỳ nhỏ. Có nhiều nhất 2025 từ và mỗi từ có độ dài tối đa là 67. Ngay cả giải pháp trực tiếp nhất kiểm tra từng ký tự của mỗi từ cũng chỉ thực hiện vài trăm nghìn thao tác. Bất kỳ quét tuyến tính nào cũng dễ dàng đủ nhanh. 

Nguồn gốc chính của sai lầm là do hiểu lầm những gì được coi là phù hợp. 

Hãy xem xét đầu vào:```
3
WY
wy
WYOMING
```Đầu ra đúng là:```
Whitney Young
wy
WYOMING
```Chỉ có từ đầu tiên được thay thế. Việc so sánh phân biệt chữ hoa chữ thường, vì vậy`"wy"`không khớp. Cũng,`"WYOMING"`chứa`"WY"`làm tiền tố, nhưng toàn bộ từ không bằng`"WY"`. 

Một sai lầm dễ mắc phải khác là cố gắng thay thế chuỗi con. Ví dụ:```
2
AWYB
WY
```Đầu ra đúng là:```
AWYB
Whitney Young
```Chỉ có vấn đề bình đẳng từ đầy đủ. 

## Phương pháp tiếp cận 

Giải pháp đơn giản nhất là xử lý từng từ một. 

Đối với mỗi từ, hãy so sánh nó với`"WY"`. 

Nếu chúng bằng nhau thì in`"Whitney Young"`. Nếu không, hãy in từ gốc. 

Việc giải thích bạo lực có thể kiểm tra mọi ký tự theo cách thủ công. Vì mỗi từ dài tối đa 67 ký tự nên từ này vẫn chạy thoải mái trong giới hạn. Tổng công việc tỷ lệ thuận với tổng số ký tự đầu vào. 

Quan sát quan trọng là tác vụ không yêu cầu khớp mẫu bên trong một chuỗi. Nó chỉ hỏi liệu một từ có chính xác bằng một giá trị cụ thể hay không. Các ngôn ngữ hiện đại đã cung cấp các phép toán bình đẳng chuỗi, khiến giải pháp trở nên gần như tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh từng nhân vật Brute Force | O(tổng chiều dài đầu vào) | O(1) | Đã chấp nhận | 
| Kiểm tra đẳng thức chuỗi trực tiếp | O(tổng chiều dài đầu vào) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Lặp lại`n`lần: 

Đọc từ tiếp theo. 
3. Nếu từ đó chính xác`"WY"`, in`"Whitney Young"`. 
4. Nếu không, hãy in từ không thay đổi. 

### Tại sao nó hoạt động 

Đối với mỗi từ đầu vào, bài toán xác định chính xác một điều kiện kích hoạt sự thay thế: từ đó phải bằng`"WY"`. 

Thuật toán kiểm tra chính xác điều kiện đó. Khi điều kiện đúng, nó sẽ xuất ra văn bản thay thế được yêu cầu. Khi điều kiện sai, nó sẽ xuất ra từ gốc. Vì mỗi từ được xử lý độc lập và theo định nghĩa của bài toán nên kết quả đầu ra luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

for _ in range(n):
    s = input().strip()
    if s == "WY":
        print("Whitney Young")
    else:
        print(s)
```Đầu tiên chương trình sẽ đọc số từ. 

Mỗi dòng tiếp theo được đọc và loại bỏ dòng mới ở cuối nó. Sự so sánh`s == "WY"`thực hiện kiểm tra đẳng thức chính xác, phân biệt chữ hoa chữ thường. 

Khi so sánh thành công, văn bản thay thế cần thiết sẽ được in. Ngược lại, từ gốc được in không thay đổi. 

sử dụng`strip()`rất quan trọng vì dòng đầu vào có chứa ký tự dòng mới ở cuối. Không loại bỏ nó, việc so sánh với`"WY"`sẽ thất bại. 

Không cần cấu trúc dữ liệu bổ sung vì mỗi từ có thể được xử lý ngay sau khi đọc nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
I
miss
WY
Math
Team
and
WY
Coding
Club
```Dấu vết: 

| Lời | Bằng "WY"? | Đầu ra | 
| --- | --- | --- | 
| Tôi | Không | Tôi | 
| nhớ | Không | nhớ | 
| WY | Có | Whitney trẻ | 
| Toán | Không | Toán | 
| Đội | Không | Đội | 
| và | Không | và | 
| WY | Có | Whitney trẻ | 
| Mã hóa | Không | Mã hóa | 
| Câu lạc bộ | Không | Câu lạc bộ | 

Đầu ra:```
I
miss
Whitney Young
Math
Team
and
Whitney Young
Coding
Club
```Ví dụ này cho thấy rằng mỗi lần xuất hiện của từ chính xác`"WY"`được thay thế một cách độc lập. 

### Ví dụ 2 

đầu vào:```
4
WY
wy
WYOMING
WY
```Dấu vết: 

| Lời | Bằng "WY"? | Đầu ra | 
| --- | --- | --- | 
| WY | Có | Whitney trẻ | 
| wy | Không | wy | 
| WYOMING | Không | WYOMING | 
| WY | Có | Whitney trẻ | 

Đầu ra:```
Whitney Young
wy
WYOMING
Whitney Young
```Điều này thể hiện cả sự phân biệt chữ hoa chữ thường và yêu cầu toàn bộ từ phải khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài đầu vào) | Mỗi từ được đọc một lần và so sánh một lần | 
| Không gian | O(1) | Mỗi lần chỉ có một từ được lưu trữ | 

Tổng kích thước đầu vào rất nhỏ nên giải pháp này chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    for _ in range(n):
        s = input().strip()
        if s == "WY":
            print("Whitney Young")
        else:
            print(s)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""9
I
miss
WY
Math
Team
and
WY
Coding
Club
"""
) == (
"""I
miss
Whitney Young
Math
Team
and
Whitney Young
Coding
Club
"""
)

# minimum size
assert run(
"""1
WY
"""
) == (
"""Whitney Young
"""
)

# no replacements
assert run(
"""3
abc
DEF
Wy
"""
) == (
"""abc
DEF
Wy
"""
)

# all replacements
assert run(
"""4
WY
WY
WY
WY
"""
) == (
"""Whitney Young
Whitney Young
Whitney Young
Whitney Young
"""
)

# substring but not exact match
assert run(
"""3
AWYB
WYOMING
WY
"""
) == (
"""AWYB
WYOMING
Whitney Young
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Từ đơn`WY`|`Whitney Young`| Kích thước đầu vào tối thiểu | 
| Không có từ nào bằng`WY`| Lời gốc | Không có sự thay thế ngẫu nhiên | 
| Tất cả các từ đều bình đẳng`WY`| Tất cả đã thay thế | Thay thế lặp đi lặp lại | 
|`AWYB`,`WYOMING`,`WY`| Chỉ được thay thế lần cuối | Bình đẳng chính xác, không khớp chuỗi con | 

## Vỏ cạnh 

Hãy xem xét:```
1
wy
```Thuật toán so sánh`"wy"`với`"WY"`. Vì so sánh chuỗi phân biệt chữ hoa chữ thường nên chúng khác nhau. Đầu ra vẫn còn:```
wy
```Điều này xử lý chính xác các trường hợp khác nhau. 

Coi như:```
1
WYOMING
```Thuật toán kiểm tra xem toàn bộ chuỗi có bằng không`"WY"`. Không, vì vậy đầu ra là:```
WYOMING
```Điều này tránh được lỗi phổ biến là thay thế những từ chỉ chứa`"WY"`như một chuỗi con. 

Coi như:```
3
WY
WY
WY
```Mỗi từ được xử lý độc lập. Mọi so sánh đều thành công, tạo ra:```
Whitney Young
Whitney Young
Whitney Young
```Thuật toán không dừng lại sau lần thay thế đầu tiên và xử lý chính xác nhiều lần xuất hiện.
