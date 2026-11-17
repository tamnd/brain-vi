---
title: "CF 1950C - Chuyển đổi đồng hồ"
description: "Chúng ta có thời gian nhất định được thể hiện ở định dạng đồng hồ 24 giờ, ví dụ: 00:00 là nửa đêm, 13:45 là 1 giờ 45 phút chiều hoặc 23:59 là 1 phút trước nửa đêm. Nhiệm vụ của chúng ta là chuyển đổi mỗi lần sang định dạng đồng hồ 12 giờ, sử dụng nhãn AM và PM quen thuộc."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1950
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 937 (Div. 4)"
rating: 800
weight: 1950
solve_time_s: 51
verified: true
draft: false
---

[CF 1950C - Chuyển đổi đồng hồ](https://codeforces.com/problemset/problem/1950/C) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ví dụ: chúng tôi được cung cấp thời gian được thể hiện ở định dạng đồng hồ 24 giờ`00:00`vào nửa đêm,`13:45`vào lúc 1 giờ 45 phút chiều, hoặc`23:59`trong một phút trước nửa đêm. Nhiệm vụ của chúng ta là chuyển đổi mỗi lần sang định dạng đồng hồ 12 giờ, sử dụng nhãn AM và PM quen thuộc. Trong hệ thống này, nửa đêm tương ứng với`12:00 AM`, buổi trưa tương ứng với`12:00 PM`và các giờ từ 1 đến 11 được gắn nhãn đơn giản là AM vào buổi sáng hoặc PM vào buổi chiều. Số phút không thay đổi. 

Đầu vào cung cấp tới 1440 trường hợp kiểm thử, mỗi trường hợp bao gồm một chuỗi ở định dạng`hh:mm`. Mỗi`hh`nằm giữa`00`Và`23`Và`mm`nằm giữa`00`Và`59`. Vì mỗi thao tác trên một chuỗi có thời gian không đổi nên số lượng thao tác tối đa gần như tuyến tính theo số lượng ca kiểm thử. Việc này có thể quản lý được trong giới hạn thời gian 1 giây, vì vậy chúng tôi không cần bất kỳ tối ưu hóa nâng cao nào. 

Các trường hợp biên không rõ ràng xuất hiện khi giờ nằm ​​ở ranh giới của phần chia Sáng/Chiều. Cụ thể,`00:mm`phải trở thành`12:mm AM`Và`12:mm`phải trở thành`12:mm PM`. Những cách tiếp cận ngây thơ chỉ đơn giản trừ 12 cho số giờ trên 12 hoặc giữ nguyên số giờ sẽ phân loại sai những trường hợp này. Ví dụ,`00:59`phải chuyển đổi thành`12:59 AM`, không`00:59 AM`, Và`12:14`phải chuyển đổi thành`12:14 PM`, không`00:14 PM`. 

## Phương pháp tiếp cận 

Phương pháp brute-force đọc từng thời gian đầu vào, chia giờ và phút và áp dụng một loạt quy tắc có điều kiện. Chúng tôi sẽ kiểm tra xem giờ bằng 0, 12 hay lớn hơn 12 và điều chỉnh cả giờ và hậu tố AM/PM cho phù hợp. Mỗi lần chuyển đổi là thời gian không đổi, do đó, ngay cả với tối đa 1440 trường hợp thử nghiệm, phương pháp mạnh mẽ này vẫn sẽ chạy hiệu quả. Không có tắc nghẽn thuật toán. 

Thông tin chi tiết quan trọng giúp đơn giản hóa việc triển khai là các quy tắc chuyển đổi cho giờ là số học đơn giản. Đối với các giờ từ 1 đến 11, nhãn AM/PM chỉ phụ thuộc vào việc giờ ban đầu nhỏ hơn 12 (AM) hay 12 hay lớn hơn (PM). Nửa đêm và trưa là những trường hợp đặc biệt duy nhất mà chúng ta cần xuất 12 thay vì 0 hoặc giữ nguyên giờ. Khi chúng tôi xác định rõ ràng hai trường hợp này, tất cả các giờ khác đều tuân theo công thức đơn giản`hour % 12`từ 1-11 giờ chiều và không thay đổi vào buổi sáng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,`t`, từ đầu vào. Mỗi trường hợp thử nghiệm đại diện cho một chuỗi thời gian. 
2. Với mỗi test case, hãy đọc chuỗi`s`ở định dạng`hh:mm`. Chia nó thành`hh`Và`mm`dưới dạng số nguyên. 
3. Kiểm tra xem giờ có bằng 0 không. Nếu vậy, hãy đặt giờ 12 giờ thành 12 và gắn nhãn là AM. 
4. Kiểm tra xem giờ có chính xác là 12 không. Nếu vậy, hãy để giờ là 12 và gắn nhãn PM. 
5. Nếu giờ lớn hơn 12, hãy trừ 12 khỏi giờ để chuyển đổi thành 12 giờ tương đương và gắn nhãn là PM. 
6. Nếu giờ nằm ​​trong khoảng từ 1 đến 11, hãy giữ nguyên và gắn nhãn là AM. 
7. Định dạng giờ và phút dưới dạng chuỗi hai chữ số để giữ nguyên các số 0 đứng đầu. 
8. In chuỗi được định dạng với hậu tố AM hoặc PM thích hợp. 

Tại sao nó hoạt động: Ở mỗi bước, việc chuyển đổi tuân theo định nghĩa rõ ràng về đồng hồ 12 giờ. Nửa đêm và buổi trưa được xử lý dưới dạng ngoại lệ, trong khi các giờ khác được ánh xạ đơn giản bằng cách sử dụng modulo số học 12. Điều này đảm bảo rằng tất cả thời gian 24 giờ hợp lệ đều được chuyển đổi chính xác và nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    hh, mm = map(int, s.split(":"))
    if hh == 0:
        period = "AM"
        hh_12 = 12
    elif hh == 12:
        period = "PM"
        hh_12 = 12
    elif hh > 12:
        period = "PM"
        hh_12 = hh - 12
    else:
        period = "AM"
        hh_12 = hh
    print(f"{hh_12:02d}:{mm:02d} {period}")
```Giải pháp đọc từng trường hợp kiểm thử, chuyển đổi chuỗi giờ và phút thành số nguyên và áp dụng quy tắc chuyển đổi AM/PM. Chúng tôi xử lý rõ ràng`hh = 0`Và`hh = 12`để tránh mắc phải những lỗi lầm nhỏ. Chuỗi định dạng`"{hh_12:02d}:{mm:02d}"`đảm bảo rằng các số 0 đứng đầu được giữ nguyên, điều này rất quan trọng đối với ký hiệu đồng hồ 12 giờ. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`00:59`: 

| hh | mm | hh_12 | kỳ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 59 | 12 | LÀ | 12:59 sáng | 

Chuyển đổi ánh xạ chính xác nửa đêm đến 12 giờ sáng. 

Đối với đầu vào`18:06`: 

| hh | mm | hh_12 | kỳ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 18 | 6 | 6 | Thủ tướng | 06:06 chiều | 

Giờ 18 lớn hơn 12 nên trừ 12 được 6 và có nhãn là PM. 

Những dấu vết này xác nhận rằng các điều kiện biên và các trường hợp chung được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Chỉ cần một vài biến cho mỗi trường hợp thử nghiệm; không sử dụng cấu trúc dữ liệu bổ sung | 

Với tối đa 1440 trường hợp thử nghiệm, điều này nằm trong giới hạn thời gian 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        s = input().strip()
        hh, mm = map(int, s.split(":"))
        if hh == 0:
            period = "AM"
            hh_12 = 12
        elif hh == 12:
            period = "PM"
            hh_12 = 12
        elif hh > 12:
            period = "PM"
            hh_12 = hh - 12
        else:
            period = "AM"
            hh_12 = hh
        print(f"{hh_12:02d}:{mm:02d} {period}")
    return out.getvalue().strip()

# Provided samples
assert run("11\n09:41\n18:06\n12:14\n00:59\n00:00\n14:34\n01:01\n19:07\n11:59\n12:00\n21:37\n") == \
"""09:41 AM
06:06 PM
12:14 PM
12:59 AM
12:00 AM
02:34 PM
01:01 AM
07:07 PM
11:59 AM
12:00 PM
09:37 PM"""

# Custom cases
assert run("3\n00:00\n12:00\n23:59\n") == "12:00 AM\n12:00 PM\n11:59 PM", "boundary times"
assert run("2\n01:05\n13:05\n") == "01:05 AM\n01:05 PM", "simple AM/PM mapping"
assert run("2\n11:11\n22:22\n") == "11:11 AM\n10:22 PM", "before noon and evening"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00:00, 12:00, 23:59 | 12:00 sáng, 12:00 trưa, 11:59 tối | Các trường hợp nửa đêm, trưa và phút cuối | 
| 01:05, 13:05 | 01:05 sáng, 01:05 chiều | AM và PM trong cùng một giờ số | 
| 11:11, 22:22 | 11:11 sáng, 10:22 tối | Giờ ngay trước buổi trưa và buổi tối chuyển đổi | 

## Vỏ cạnh 

cho`00:00`, bộ thuật toán`hh_12`đến 12 và đến giờ sáng. Chuỗi được định dạng trở thành`12:00 AM`. Vì`12:00`,`hh_12`vẫn là 12 và thời gian là PM, cho`12:00 PM`. Những trường hợp này sẽ thất bại nếu chúng ta áp dụng một cách ngây thơ`hh % 12`mà không xử lý số không. Vì`23:59`, trừ 12 được 11 và chu kỳ PM, cho kết quả`11:59 PM`, xác nhận việc lập bản đồ từ chiều đến tối là chính xác. Dấu vết cho thấy thuật toán luôn tạo ra kết quả đầu ra chính xác trong 12 giờ cho tất cả các thời điểm biên.
