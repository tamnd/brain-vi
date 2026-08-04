---
title: "CF 102556B - Riana và buổi hẹn hò mù quáng"
description: "Nhiệm vụ là đếm tổng chi phí bút chì để viết mỗi ngày theo lịch từ năm A đến năm B. Một ngày được biểu thị không có năm bằng cách nối số tháng và số ngày. Ví dụ: ngày 7 tháng 3 trở thành 37, trong khi ngày 24 tháng 12 trở thành 1224."
date: "2026-08-04T09:16:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102556
codeforces_index: "B"
codeforces_contest_name: "2020 Ateneo de Manila University DISCS PrO HS Division"
rating: 0
weight: 102556
solve_time_s: 65
verified: true
draft: false
---

[CF 102556B - Riana và buổi hẹn hò mù quáng](https://codeforces.com/problemset/problem/102556/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là tính tổng chi phí viết bút chì vào mỗi ngày dương lịch kể từ năm`A`qua năm`B`. Ngày được thể hiện không có năm bằng cách nối số tháng và số ngày. Ví dụ, ngày 7 tháng 3 trở thành`37`, trong khi ngày 24 tháng 12 trở thành`1224`. Chi phí để viết một ngày là giá trị số của chính biểu diễn đó. 

Phần duy nhất của một năm ảnh hưởng đến câu trả lời là tháng Hai có 28 hay 29 ngày. Đầu vào cho hai năm, có thể lớn bằng`10^18`và đầu ra là tổng của tất cả các giá trị ngày trong tất cả các năm đó, giảm modulo`104206969`. 

Kích thước của năm ngay lập tức loại trừ việc lặp lại qua năm hoặc ngày. Một phạm vi chứa`10^18`năm sẽ yêu cầu khoảng`10^18 * 365`tính toán ngày, vượt xa những gì một chương trình có thể thực hiện trong một thời gian hợp lý. Lời giải phải sử dụng thực tế là hầu hết các năm đều giống hệt nhau và chỉ có số năm nhuận là quan trọng. 

Có một số nơi việc triển khai trực tiếp có thể thất bại. Năm`0`là một trong số đó. Nó thỏa mãn quy tắc năm nhuận vì nó chia hết cho 400, do đó đầu vào`0 0`phải sử dụng tháng 2 với 29 ngày và tạo ra`180987`. Một giải pháp bắt đầu tính năm nhuận từ`1`sẽ coi nó như một năm bình thường một cách không chính xác. 

Một cái bẫy khác là quy luật thế kỷ. đầu vào`1900 1900`là một năm bình thường mặc dù nó chia hết cho 4, vì nó cũng chia hết cho 100 chứ không chia hết cho 400. Câu trả lời của nó là`180758`. Giải pháp chỉ sử dụng tính chia hết cho 4 sẽ trả về giá trị của năm nhuận. 

Ranh giới đối diện xuất hiện tại`2000 2000`. Năm nay chia hết cho 400 nên là năm nhuận và đáp án là`180987`. Các giải pháp bác bỏ tất cả các năm thế kỷ vì năm không nhuận đều thất bại ở đây. 

# Phương pháp tiếp cận 

Giải pháp Brute Force sẽ xử lý hàng năm trong khoảng thời gian đó, xác định xem đó có phải là năm nhuận hay không, sau đó lặp qua 12 tháng và tất cả các ngày trong những tháng đó. Đối với mỗi ngày, nó sẽ xây dựng giá trị ngày và thêm nó vào câu trả lời. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. 

Tuy nhiên, khoảng lớn nhất có thể chứa khoảng`10^18`năm. Ngay cả một hoạt động liên tục mỗi năm cũng đã quá chậm và việc mô phỏng toàn bộ ngày sẽ yêu cầu khoảng`3.65 * 10^20`hoạt động trong ngày. Cấu trúc lặp đi lặp lại của lịch là quan sát quan trọng giúp loại bỏ nhu cầu này. 

Mỗi năm bình thường đều đóng góp số tiền như nhau, vì độ dài tháng của nó không bao giờ thay đổi. Mỗi năm nhuận cũng đóng góp số tiền bằng nhau. Thay vì tính tổng các năm riêng lẻ, chúng ta chỉ cần đếm xem có bao nhiêu năm bình thường và năm nhuận xuất hiện trong dãy. 

Trong một tháng`m`với`L`ngày, chín ngày đầu tiên đóng góp các giá trị có dạng`10m + day`và những ngày còn lại đóng góp các giá trị có dạng`100m + day`. Kết hợp hai phần này sẽ cho một công thức trực tiếp:`month_sum = m * (100L - 810) + L(L+1)/2`Sử dụng điều này cho tất cả các tháng sẽ mang lại sự đóng góp cố định của một năm bình thường và một năm nhuận. 

Vấn đề còn lại là tính năm nhuận từ`0`đến một năm nhất định. Số năm chia hết cho`k`từ`0`bởi vì`n`là`n//k + 1`, vì năm 0 được bao gồm. Loại trừ bao gồm cho số bước nhảy vọt:`multiples of 4 - multiples of 100 + multiples of 400`Sử dụng hàm tiền tố trong nhiều năm`0..n`chúng ta hãy trả lời bất kỳ khoảng nào bằng phép trừ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số năm * 365) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Tính trước phần đóng góp của một năm bình thường và một năm nhuận. Đối với mỗi tháng, hãy áp dụng công thức tính tổng tất cả các ngày được mã hóa trong tháng đó. Điều này tránh việc tính toán lại cùng một mẫu lịch nhiều lần. 
2. Tạo hàm tiền tố`pref(x)`trả về tổng số tiền đóng góp của tất cả các năm từ`0`bởi vì`x`. Nếu như`x`là số âm, tiền tố bằng 0 vì không có năm nào trước số 0. 
3. Đếm số năm nhuận trong phạm vi tiền tố bằng cách chia hết cho 4, 100 và 400. Những năm còn lại là năm bình thường, do đó hãy nhân hai số đếm với số tiền đóng góp hàng năm tương ứng của chúng. 
4. Tính khoảng yêu cầu bằng cách trừ các tiền tố:`pref(B) - pref(A-1)`. Áp dụng mô đun sau phép trừ để giữ kết quả dương. 

Tại sao nó hoạt động: mỗi năm thuộc về đúng một trong hai loại, bình thường hoặc nhảy vọt. Tất cả các năm bình thường đều có độ dài 12 tháng như nhau nên chúng đóng góp số tiền như nhau. Điều này cũng đúng đối với những năm nhuận. Hàm tiền tố đếm chính xác số năm tồn tại của mỗi loại, có nghĩa là tổng số tiền là sự kết hợp chính xác của hai khoản đóng góp cố định đó. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 104206969

def year_value(leap):
    days = [31, 29 if leap else 28, 31, 30, 31, 30,
            31, 31, 30, 31, 30, 31]

    total = 0
    for month, length in enumerate(days, 1):
        total += month * (100 * length - 810)
        total += length * (length + 1) // 2
    return total % MOD

NORMAL_YEAR = year_value(False)
LEAP_YEAR = year_value(True)

def leap_count(x):
    if x < 0:
        return 0
    return (x // 4 + 1) - (x // 100 + 1) + (x // 400 + 1)

def prefix(x):
    if x < 0:
        return 0

    leaps = leap_count(x)
    years = x + 1
    normal = years - leaps

    return (normal * NORMAL_YEAR + leaps * LEAP_YEAR) % MOD

def solve():
    A, B = map(int, input().split())
    print((prefix(B) - prefix(A - 1)) % MOD)

solve()
```

`year_value`nén tất cả mười hai tháng vào một phép tính duy nhất. Công thức xử lý các ngày có một chữ số và hai chữ số cùng nhau, giúp tránh lặp lại các ngày riêng lẻ.`leap_count`cố tình sử dụng`x//k + 1`thay vì chỉ`x//k`. Số dư đại diện cho năm 0, tuân theo các quy tắc chia hết giống như mọi năm khác. 

Việc tính toán tiền tố sử dụng`x + 1`năm vì khoảng thời gian bắt đầu từ 0 và bao gồm cả hai điểm cuối. Phép trừ cuối cùng sử dụng`A - 1`, vậy năm`A`bản thân nó vẫn được bao gồm. 

Số nguyên Python không bị tràn, nhưng các giá trị trung gian có thể trở nên lớn. Mô-đun được áp dụng khi trả về giá trị tiền tố và câu trả lời cuối cùng cũng được chuẩn hóa bằng`% MOD`vì phép trừ có thể âm. 

# Ví dụ đã hoạt động 

Đối với đầu vào`1 3`, cả ba năm đều là năm bình thường. 

| Phạm vi năm | Năm nhuận | Năm bình thường | Đóng góp | 
| --- | --- | --- | --- | 
| 0..3 | 1 | 3 | giá trị tiền tố | 
| 0..0 | 1 | 0 | giá trị tiền tố | 
| 1..3 | 0 | 3 | 542274 | 

Kết quả là`3 * 180758 = 542274`, phù hợp với mẫu Dấu vết này xác nhận rằng phép trừ khoảng thời gian loại bỏ chính xác các năm trước`A`. 

Đối với đầu vào`2000 2000`, năm duy nhất là năm nhuận. 

| Phạm vi năm | Năm nhuận | Năm bình thường | Đóng góp | 
| --- | --- | --- | --- | 
| 0..2000 | 486 | 1515 | giá trị tiền tố | 
| 0..1999 | 485 | 1515 | giá trị tiền tố | 
| 2000..2000 | 1 | 0 | 180987 | 

Sự khác biệt để lại chính xác một đóng góp của năm nhuận, xác nhận rằng quy tắc thế kỷ được xử lý chính xác. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và phép tính mười hai tháng cố định được thực hiện | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ các giá trị có kích thước không đổi | 

Giải pháp không phụ thuộc vào kích thước của khoảng năm. Ngay cả các giá trị đầu vào tối đa gần`10^18`yêu cầu số lượng công việc tương tự như khoảng thời gian một năm. 

# Trường hợp thử nghiệm```python
import sys
import io

MOD = 104206969

def year_value(leap):
    days = [31, 29 if leap else 28, 31, 30, 31, 30,
            31, 31, 30, 31, 30, 31]
    total = 0
    for month, length in enumerate(days, 1):
        total += month * (100 * length - 810)
        total += length * (length + 1) // 2
    return total % MOD

NORMAL_YEAR = year_value(False)
LEAP_YEAR = year_value(True)

def leap_count(x):
    if x < 0:
        return 0
    return (x // 4 + 1) - (x // 100 + 1) + (x // 400 + 1)

def prefix(x):
    if x < 0:
        return 0
    l = leap_count(x)
    return ((x + 1 - l) * NORMAL_YEAR + l * LEAP_YEAR) % MOD

def solve_case(inp):
    a, b = map(int, inp.split())
    return str((prefix(b) - prefix(a - 1)) % MOD)

assert solve_case("1 3") == "542274", "sample"
assert solve_case("0 0") == "180987", "year zero leap"
assert solve_case("1 1") == "180758", "single normal year"
assert solve_case("1900 1900") == "180758", "century non leap"
assert solve_case("2000 2000") == "180987", "century leap"
assert solve_case("1000000000000000000 1000000000000000000") == "180987", "maximum year"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`180987`| Xử lý năm không | 
|`1 1`|`180758`| Năm thường đơn | 
|`1900 1900`|`180758`| Ngoại lệ thế kỷ | 
|`2000 2000`|`180987`| Chia hết cho 400 năm nhuận | 
|`1000000000000000000 1000000000000000000`|`180987`| Ranh giới tối đa | 

# Vỏ cạnh 

Đối với đầu vào`0 0`, thuật toán tính toán`leap_count(0)`BẰNG`1//?`thông qua công thức`(0//4+1) - (0//100+1) + (0//400+1)`, mang lại`1`. Năm được xếp vào loại nhuận và góp phần`180987`. 

Đối với đầu vào`1900 1900`, số chia hết cho 4 sẽ bao gồm năm, nhưng số chia hết cho 100 sẽ loại bỏ nó. Vì 1900 không chia hết cho 400 nên nó không được cộng lại. Kết quả sử dụng sự đóng góp của năm bình thường. 

Đối với đầu vào`2000 2000`, năm bị xóa theo quy tắc chia hết cho 100 và được khôi phục theo quy tắc chia hết cho 400. Nó nhận được sự đóng góp của năm nhuận. 

Đối với một phạm vi rộng lớn như`1 10^18`, thuật toán không bao giờ lặp qua nhiều năm. Nó chỉ đánh giá số năm nhuận và năm bình thường bằng phép chia số nguyên, do đó thời gian chạy không đổi ngay cả ở giới hạn tối đa.
