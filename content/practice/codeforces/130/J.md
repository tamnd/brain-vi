---
title: "CF 130J - Tính ngày"
description: "Chúng ta có một năm trong lịch Gregory và một số ngày trong năm đó. Nhiệm vụ là xác định ngày dương lịch thực tế tương ứng với số ngày đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "J"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1800
weight: 130
solve_time_s: 90
verified: true
draft: false
---

[CF 130J - Tính ngày](https://codeforces.com/problemset/problem/130/J) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một năm trong lịch Gregory và một số ngày trong năm đó. Nhiệm vụ là xác định ngày dương lịch thực tế tương ứng với số ngày đó. 

Ví dụ: nếu năm đầu vào là 2011 và chỉ số ngày là 324, chúng tôi sẽ đếm tiếp từ ngày 1 tháng 1 cho đến ngày thứ 324 trong năm. Câu trả lời là ngày dương lịch mà việc đếm đó dừng lại. 

Sự phức tạp duy nhất đến từ những năm nhuận. Tháng Hai thường có 28 ngày nhưng vào năm nhuận thì có 29 ngày. Một năm chia hết cho 400 hoặc chia hết cho 4 nhưng không chia hết cho 100 là năm nhuận. 

Những hạn chế là rất nhỏ. Năm nằm trong khoảng từ 1600 đến 2400 và chỉ số ngày cao nhất là 366. Ngay cả một mô phỏng rất trực tiếp qua các tháng cũng dễ dàng đủ nhanh vì chỉ có 12 tháng trong một năm. Bất kỳ giải pháp nào xử lý từng tháng một đều chạy trong thời gian không đổi. 

Mối nguy hiểm chính là xử lý năm nhuận không chính xác. Nhiều cách triển khai không chính xác chỉ kiểm tra khả năng chia hết cho 4, điều này sẽ bị gián đoạn trong những năm như năm 1900. Một lỗi phổ biến khác là quên tăng tháng 2 lên 29 ngày trong những năm nhuận. 

Hãy xem xét ví dụ này:```
Input:
1900
60
```Năm 1900 chia hết cho 100 nhưng không chia hết cho 400 nên không phải là năm nhuận. Tháng hai có 28 ngày. Ngày thứ 60 là ngày 1 tháng 3. 

Đầu ra đúng:```
1 3
```Việc thực hiện bất cẩn chỉ sử dụng`year % 4 == 0`sẽ coi năm 1900 là năm nhuận một cách không chính xác và tạo ra ngày 29 tháng 2. 

Một trường hợp cạnh khác xuất hiện vào gần cuối năm nhuận.```
Input:
2000
366
```Năm 2000 chia hết cho 400 nên là năm nhuận. Ngày thứ 366 tồn tại và tương ứng với ngày 31 tháng 12. 

Đầu ra đúng:```
31 12
```Nếu tháng Hai không được điều chỉnh thành 29 ngày thì việc tính toán sẽ dịch chuyển lùi lại một ngày vào mỗi tháng sau đó. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản nhất là mô phỏng lịch từng ngày. Bắt đầu từ ngày 1 tháng 1 và liên tục tăng dần một ngày cho đến khi đạt được chỉ số ngày yêu cầu. Điều này hiệu quả vì các quy tắc lịch Gregorian có tính xác định và có tối đa 366 ngày để xử lý. 

Mô phỏng bạo lực đó thực hiện tối đa 365 lần chuyển đổi giữa các ngày. Thậm chí điều đó hoàn toàn có thể chấp nhận được đối với các giới hạn nhất định. Tuy nhiên, việc triển khai trở nên phức tạp hơn vì việc nâng cao ngày một cách chính xác đòi hỏi phải xử lý ranh giới tháng và năm nhuận một cách cẩn thận. 

Một cách tiếp cận rõ ràng hơn xuất phát từ việc quan sát rằng vấn đề chỉ phụ thuộc vào độ dài tháng. Thay vì di chuyển từng ngày một, chúng ta có thể trừ toàn bộ tháng khỏi chỉ số ngày. 

Giả sử chỉ số ngày mục tiêu là 324. Tháng 1 có 31 ngày, do đó ngày mục tiêu không thể nằm trong tháng 1. Chúng tôi trừ 31 và tiếp tục với tháng Hai. Chúng tôi tiếp tục loại bỏ các tháng đầy đủ cho đến khi giá trị còn lại vừa với tháng hiện tại. Giá trị còn lại đó chính xác là ngày trong tháng. 

Điều này có tác dụng vì các tháng chia năm thành các khối ngày liên tiếp. Khi chúng ta biết độ dài tháng chính xác, đặc biệt là tháng 2 trong năm nhuận, ngày có thể được xác định bằng cách quét tuyến tính đơn giản trong 12 tháng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(366) | O(1) | Đã chấp nhận | 
| Tối ưu | O(12) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chỉ số năm và ngày. 
2. Xác định xem năm đó có phải là năm nhuận hay không. 

Một năm là năm nhuận nếu: 

- nó chia hết cho 400, hoặc 
- chia hết cho 4 nhưng không chia hết cho 100. 
3. Tạo mảng độ dài tháng. 

Bắt đầu với:```
[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
```Nếu là năm nhuận thì đổi ngày 28 tháng 2 thành 29. 
4. Lặp lại các tháng theo thứ tự. 

Đối với mỗi tháng: 

- nếu chỉ số ngày còn lại lớn hơn độ dài tháng hiện tại thì trừ cả tháng và tiếp tục; 
- nếu không thì câu trả lời nằm ở tháng này. 
5. Sau khi tìm thấy đúng tháng: 

- chỉ số ngày còn lại là ngày trong tháng; 
- vị trí tháng hiện tại cộng với một là số tháng. 
6. Xuất ngày và tháng. 

### Tại sao nó hoạt động 

Ở mỗi bước lặp, chỉ số ngày còn lại biểu thị vị trí bên trong hậu tố vẫn chưa được xử lý của năm. Khi chúng tôi trừ đi độ dài của một tháng, chúng tôi sẽ xóa chính xác tất cả các ngày thuộc tháng đó. Sau khi xử lý vài tháng, giá trị còn lại sẽ đủ nhỏ để chứa vào tháng tiếp theo. Giá trị đó chính xác là ngày dương lịch trong tháng đó nên thuật toán luôn trả về ngày chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    year = int(input())
    day_index = int(input())

    leap = (
        year % 400 == 0 or
        (year % 4 == 0 and year % 100 != 0)
    )

    months = [31, 28, 31, 30, 31, 30,
              31, 31, 30, 31, 30, 31]

    if leap:
        months[1] = 29

    month = 1

    for days in months:
        if day_index > days:
            day_index -= days
            month += 1
        else:
            print(day_index, month)
            return

solve()
```Phần đầu tiên tính xem năm đó có phải là năm nhuận hay không. Điều kiện phải tuân theo chính xác các quy tắc Gregorian. Chỉ kiểm tra tính chia hết cho 4 sẽ tạo ra đáp án sai cho các năm như 1700, 1800 và 1900. 

các`months`mảng lưu trữ số ngày trong mỗi tháng. Tháng 2 chỉ được cập nhật thành 29 khi năm đó là năm nhuận. 

Vòng lặp xử lý các tháng theo thứ tự thời gian. Nếu chỉ số ngày mục tiêu lớn hơn độ dài tháng hiện tại thì ngày mục tiêu phải nằm muộn hơn trong năm nên chúng tôi trừ cả tháng và tiếp tục. 

Điều kiện dừng là tinh tế. Chúng tôi sử dụng`if day_index > days`thay vì`>=`. Nếu giá trị còn lại bằng chính xác độ dài tháng thì câu trả lời là ngày cuối cùng của tháng hiện tại, không phải ngày đầu tiên của tháng tiếp theo. 

Ví dụ: trong năm không nhuận:```
day_index = 31
```Điều này sẽ tạo ra ngày 31 tháng 1. sử dụng`>=`sẽ trừ tháng Giêng không chính xác và chuyển sang tháng Hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2011
324
```Năm 2011 không phải là năm nhuận. 

| Tháng | Ngày trong tháng | Chỉ số ngày còn lại trước | Hành động | Còn Lại Sau | 
| --- | --- | --- | --- | --- | 
| 1 | 31 | 324 | trừ | 293 | 
| 2 | 28 | 293 | trừ | 265 | 
| 3 | 31 | 265 | trừ | 234 | 
| 4 | 30 | 234 | trừ | 204 | 
| 5 | 31 | 204 | trừ | 173 | 
| 6 | 30 | 173 | trừ | 143 | 
| 7 | 31 | 143 | trừ | 112 | 
| 8 | 31 | 112 | trừ | 81 | 
| 9 | 30 | 81 | trừ | 51 | 
| 10 | 31 | 51 | trừ | 20 | 
| 11 | 30 | 20 | dừng lại | 20 | 

Đầu ra:```
20 11
```Dấu vết này cho thấy mỗi tháng được xử lý sẽ loại bỏ toàn bộ khối ngày liền kề trong năm như thế nào cho đến khi giá trị còn lại khớp với tháng hiện tại. 

### Ví dụ 2 

đầu vào:```
2000
60
```Năm 2000 là năm nhuận vì chia hết cho 400. 

| Tháng | Ngày trong tháng | Chỉ số ngày còn lại trước | Hành động | Còn Lại Sau | 
| --- | --- | --- | --- | --- | 
| 1 | 31 | 60 | trừ | 29 | 
| 2 | 29 | 29 | dừng lại | 29 | 

Đầu ra:```
29 2
```Ví dụ này thực hiện logic năm nhuận. Tháng 2 có 29 ngày nên ngày thứ 60 trở thành ngày 29 tháng 2 thay vì ngày 1 tháng 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(12) | Tối đa 12 tháng mới được xử lý | 
| Không gian | O(1) | Chỉ một mảng nhỏ có kích thước cố định được lưu trữ | 

Thời gian chạy thực tế là không đổi vì lịch luôn có 12 tháng. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    year = int(input())
    day_index = int(input())

    leap = (
        year % 400 == 0 or
        (year % 4 == 0 and year % 100 != 0)
    )

    months = [31, 28, 31, 30, 31, 30,
              31, 31, 30, 31, 30, 31]

    if leap:
        months[1] = 29

    month = 1

    for days in months:
        if day_index > days:
            day_index -= days
            month += 1
        else:
            print(day_index, month)
            return

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("2011\n324\n") == "20 11", "sample 1"

# minimum valid input
assert run("1600\n1\n") == "1 1", "minimum case"

# leap year, February 29
assert run("2000\n60\n") == "29 2", "leap year handling"

# non-leap century year
assert run("1900\n60\n") == "1 3", "century year rule"

# last day of leap year
assert run("2400\n366\n") == "31 12", "end of leap year"

# boundary at end of January
assert run("2011\n31\n") == "31 1", "month boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1600 / 1`|`1 1`| Chỉ số ngày nhỏ nhất | 
|`2000 / 60`|`29 2`| Năm nhuận tháng 2 | 
|`1900 / 60`|`1 3`| Năm thế kỷ không nhảy vọt | 
|`2400 / 366`|`31 12`| Ngày hợp lệ tối đa trong năm nhuận | 
|`2011 / 31`|`31 1`| Khác nhau ở ranh giới tháng | 

## Vỏ cạnh 

Xét trường hợp năm thế kỷ không nhuận:```
Input:
1900
60
```Thuật toán kiểm tra trạng thái năm nhuận:```
1900 % 400 != 0
1900 % 4 == 0
1900 % 100 == 0
```Vì năm chia hết cho 100 chứ không chia hết cho 400 nên không phải là năm nhuận. Tháng Hai còn lại 28 ngày. 

Thuật toán trừ tháng 1:```
60 - 31 = 29
```Tháng 2 chỉ có 28 ngày nên trừ tháng 2:```
29 - 28 = 1
```Giá trị còn lại nằm gọn trong tháng 3, tạo ra:```
1 3
```Điều này xác nhận quy tắc thế kỷ được xử lý chính xác. 

Bây giờ hãy xem xét ngày hợp lệ tối đa trong một năm nhuận:```
Input:
2000
366
```Năm 2000 là năm nhuận vì chia hết cho 400 nên tháng 2 có 29 ngày. 

Thuật toán trừ tuần tự hàng tháng:```
366 - 31 - 29 - 31 - 30 - ...
```Sau khi trừ đi 11 tháng đầu tiên, giá trị còn lại sẽ là 31, khớp chính xác với tháng 12. 

Câu trả lời là:```
31 12
```Trường hợp này xác nhận hai chi tiết quan trọng: việc điều chỉnh năm nhuận là chính xác và phép so sánh sử dụng`>`còn hơn là`>=`, để ngày cuối cùng của tháng được xử lý đúng cách.
