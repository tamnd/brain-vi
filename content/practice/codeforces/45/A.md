---
title: "CF 45A - Codecraft III"
description: "Chúng ta được cung cấp tháng hiện tại dưới dạng một chuỗi và một số nguyên k biểu thị số tháng sau sẽ có một trò chơi mới được phát hành. Nhiệm vụ là xác định tháng sau thăng tiến đúng k tháng trong lịch."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "A"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 900
weight: 45
solve_time_s: 74
verified: true
draft: false
---
[CF 45A - Codecraft III](https://codeforces.com/problemset/problem/45/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp tháng hiện tại dưới dạng một chuỗi và một số nguyên`k`đại diện cho bao nhiêu tháng sau sẽ có một trò chơi mới được phát hành. Nhiệm vụ là xác định chính xác tháng sau khi thăng tiến`k`tháng tiếp theo trong lịch. 

Lịch có tính tuần hoàn. Sau tháng 12 lại đến tháng 1. Vì lý do này, vấn đề thực sự là việc di chuyển về phía trước bên trong một chuỗi vòng tròn cố định gồm các tên 12 tháng. 

Những hạn chế là cực kỳ nhỏ. Giá trị của`k`nhiều nhất là 100 và chỉ có 12 tháng. Mọi triển khai hợp lý sẽ chạy ngay lập tức. Ngay cả một mô phỏng tiến bộ từng tháng cũng thực hiện tối đa 100 thao tác, điều này là không đáng kể trong giới hạn thời gian 2 giây. 

Khó khăn chính không phải là hiệu suất mà là việc xử lý tính chất tuần hoàn của lịch một cách chính xác. 

Một sai lầm phổ biến là quên kết thúc sau tháng 12. Hãy xem xét đầu vào này:```
December
1
```Đầu ra đúng là:```
January
```Việc triển khai bất cẩn mà chỉ chuyển sang chỉ mục tiếp theo mà không có số học mô-đun sẽ vượt quá giới hạn. 

Một trường hợp tế nhị khác xuất hiện khi`k = 0`. Ví dụ:```
March
0
```Đầu ra đúng là:```
March
```Nếu ai đó luôn tiến bộ ít nhất một lần trước khi xử lý`k`, họ sẽ in sai tháng Tư. 

Trường hợp cạnh thứ ba xảy ra khi`k`lớn hơn 12. Ví dụ:```
January
25
```Đầu ra đúng là:```
February
```Vì lịch 12 tháng lặp lại nên tiến 25 tháng bằng tiến 1 tháng. Việc triển khai liên tục tiến về phía trước mà không giảm theo modulo vẫn hoạt động ở đây vì các ràng buộc nhỏ, nhưng các giải pháp lập chỉ mục trực tiếp phải áp dụng modulo một cách chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng thời gian trôi qua theo từng tháng. Chúng tôi lưu trữ tất cả tên tháng theo thứ tự bên trong một mảng, xác định tháng hiện tại, sau đó chuyển tiếp`k`lần. Mỗi khi chúng ta vượt qua tháng 12, chúng ta sẽ quay trở lại tháng 1. 

Cách tiếp cận mạnh mẽ này là chính xác vì mỗi thao tác đều phản ánh chính xác cách hoạt động của lịch. Bắt đầu từ tháng hiện tại và liên tục chuyển sang tháng tiếp theo, cuối cùng bạn sẽ có được câu trả lời đúng. 

Thời gian chạy là`O(k)`. Với`k ≤ 100`, điều này hoàn toàn có thể chấp nhận được. Tệ nhất, chúng tôi chỉ thực hiện 100 lần chuyển đổi. 

Quan sát cải thiện giải pháp là lịch lặp lại 12 tháng một lần. Tiến tới 12 tháng sẽ trở lại cùng tháng đó. Do cấu trúc tuần hoàn này nên chúng ta không cần mô phỏng gì cả. 

Nếu tháng hiện tại có chỉ mục`i`, thì câu trả lời chỉ đơn giản là:```
(i + k) % 12
```Điều này chuyển đổi vấn đề thành lập chỉ mục mảng tròn. Hoạt động modulo tự động xử lý việc gói từ tháng 12 trở lại tháng 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả mười hai tháng trong một mảng theo thứ tự lịch. 
2. Đọc tháng hiện tại và số nguyên`k`. 
3. Tìm chỉ số của tháng hiện tại trong mảng. 

Chỉ số này đại diện cho tháng bằng số từ`0`ĐẾN`11`. 
4. Tính chỉ số đích bằng cách sử dụng`(current_index + k) % 12`. 

Hoạt động modulo kết thúc vị trí một cách chính xác khi chúng ta di chuyển qua tháng 12. 
5. Xuất tháng được lưu trữ theo chỉ mục được tính toán. 

### Tại sao nó hoạt động 

Các tháng tạo thành một chu kỳ có độ dài 12. Tăng thêm một tháng tương ứng với việc tiến lên một bước trong mảng. Tiến lên bởi`k`tháng tương ứng với việc di chuyển`k`bước về phía trước. 

Số học Modulo bảo toàn vị trí chính xác trong cấu trúc tuần hoàn. Bất kỳ chuyển động nào ngoài chỉ số 11 sẽ quay trở lại điểm bắt đầu chính xác như lịch. Vì điều này,`(current_index + k) % 12`luôn khớp với tháng tương lai chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

months = [
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December"
]

current = input().strip()
k = int(input())

current_index = months.index(current)
answer_index = (current_index + k) % 12

print(months[answer_index])
```Mảng`months`xác định thứ tự lịch một cách rõ ràng. Điều này cho chúng ta ánh xạ trực tiếp giữa tên tháng và chỉ số. 

Cuộc gọi tới`months.index(current)`tìm thấy vị trí bắt đầu. Vì chỉ có 12 mục nên việc tra cứu này thực sự là thời gian không đổi. 

biểu thức`(current_index + k) % 12`là phần quan trọng của việc thực hiện. Không có`% 12`, các chỉ số lớn hơn 11 sẽ không hợp lệ. Modulo chuyển đổi các chỉ số lớn trở lại phạm vi hợp lệ trong khi vẫn giữ đúng vị trí tuần hoàn. 

sử dụng`input().strip()`rất quan trọng vì tên tháng xuất phát từ đầu vào tiêu chuẩn và có thể chứa ký tự dòng mới ở cuối. Nếu không loại bỏ khoảng trắng, việc tra cứu sẽ thất bại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
November
3
```| Bước | Giá trị | 
| --- | --- | 
| Tháng hiện tại | Tháng 11 | 
| Chỉ mục hiện tại | 10 | 
| k | 3 | 
| Chỉ số tính toán | (10 + 3) % 12 = 1 | 
| Trả lời | Tháng Hai | 

Dấu vết cho thấy hành vi bao quanh một cách rõ ràng. Di chuyển ba tháng sau tháng 11 sẽ chuyển sang tháng 12 và quay trở lại điểm bắt đầu của mảng. 

### Ví dụ 2 

đầu vào:```
January
25
```| Bước | Giá trị | 
| --- | --- | 
| Tháng hiện tại | Tháng Một | 
| Chỉ mục hiện tại | 0 | 
| k | 25 | 
| Chỉ số tính toán | (0 + 25) % 12 = 1 | 
| Trả lời | Tháng Hai | 

Ví dụ này chứng tỏ rằng phép toán modulo xử lý chính xác các giá trị lớn hơn 12. Tiến lên 25 tháng tương đương với tăng lên 1 tháng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Kích thước mảng được cố định ở mức 12, vì vậy mọi thao tác đều có thời gian không đổi | 
| Không gian | O(1) | Chỉ một mảng tên tháng có kích thước cố định được lưu trữ | 

Giải pháp dễ dàng phù hợp trong giới hạn. Chương trình chỉ thực hiện một số phép tính số học và lưu trữ một lượng dữ liệu không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
    ]

    current = input().strip()
    k = int(input())

    current_index = months.index(current)
    answer_index = (current_index + k) % 12

    print(months[answer_index])

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run("November\n3\n") == "February", "sample 1"

# custom cases
assert run("March\n0\n") == "March", "k = 0"
assert run("December\n1\n") == "January", "year wrap"
assert run("January\n12\n") == "January", "full cycle"
assert run("January\n25\n") == "February", "large k with modulo"
assert run("August\n100\n") == "December", "maximum-style large movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`March 0`|`March`| Không chuyển động | 
|`December 1`|`January`| Kết thúc sau tháng 12 | 
|`January 12`|`January`| Chu kỳ lịch đầy đủ | 
|`January 25`|`February`| Hành vi modulo đúng | 
|`August 100`|`December`| Giá trị chuyển động lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
December
1
```Thuật toán tìm`December`tại chỉ mục`11`. Sau đó nó tính toán:```
(11 + 1) % 12 = 0
```chỉ mục`0`tương ứng với`January`, khớp với quá trình chuyển đổi lịch chính xác. 

Bây giờ hãy xem xét:```
March
0
```chỉ số của`March`là`2`. Việc tính toán trở thành:```
(2 + 0) % 12 = 2
```Kết quả ở lại`March`. Thuật toán xử lý chính xác trường hợp không có tháng nào trôi qua. 

Cuối cùng, hãy xem xét:```
January
25
```chỉ số của`January`là`0`. Việc tính toán trở thành:```
(0 + 25) % 12 = 1
```chỉ mục`1`tương ứng với`February`. Vì lịch lặp lại cứ sau 12 tháng, nên tăng thêm 25 tháng tương đương với tăng lên 1 tháng và thuật toán sẽ tự động ghi lại điều đó thông qua số học modulo.
