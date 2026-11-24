---
title: "CF 80B - Trầm cảm"
description: "Chúng ta có thời gian kỹ thuật số ở định dạng HH:MM và cần xác định xem kim đồng hồ analog phải quay bao xa so với vị trí ban đầu là 12:00. Đồng hồ bắt đầu với cả hai kim chỉ vào số 12. Chúng ta có thể xoay từng kim một cách độc lập và chỉ theo chiều kim đồng hồ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 80
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 69 (Div. 2 Only)"
rating: 1200
weight: 80
solve_time_s: 98
verified: true
draft: false
---

[CF 80B - Trầm cảm](https://codeforces.com/problemset/problem/80/B) 

**Đánh giá:** 1200 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một thời gian kỹ thuật số trong`HH:MM`định dạng và cần xác định kim đồng hồ analog phải quay bao xa so với vị trí ban đầu`12:00`. 

Đồng hồ bắt đầu với cả hai kim chỉ vào số 12. Chúng ta có thể xoay từng kim một cách độc lập và chỉ theo chiều kim đồng hồ. Nhiệm vụ là tính góc quay không âm nhỏ nhất theo chiều kim đồng hồ cho kim giờ và kim phút để đồng hồ hiển thị thời gian cần thiết. 

Phần khó khăn là kim giờ không nhảy một lần mỗi giờ. Nó di chuyển liên tục theo từng phút. Ví dụ, tại`04:30`kim giờ chỉ ở giữa số 4 và số 5. 

Các ràng buộc rất nhỏ vì chỉ có một đầu vào thời gian duy nhất. Bất kỳ phép tính toán học có thời gian không đổi hợp lý nào cũng dễ dàng nằm trong giới hạn. Thử thách thực sự là phải hiểu hình học một cách chính xác và tránh những lỗi nhỏ trong công thức. 

Một số trường hợp đặc biệt thường phá vỡ việc triển khai không chính xác. 

Đầu tiên là quên rằng kim giờ chuyển động liên tục. 

Đối với đầu vào:```
04:30
```góc kim giờ đúng là:```
4 * 30 + 30 * 0.5 = 135
```Một giải pháp bất cẩn chỉ sử dụng giá trị giờ sẽ tạo ra`120`, điều này sai vì nó bỏ qua 30 phút tiến tới 5. 

Một trường hợp tế nhị khác là nửa đêm. 

Đối với đầu vào:```
00:00
```đầu ra đúng là:```
0 0
```Việc triển khai có lỗi có thể tính toán góc giờ là`24 * 30 = 720`và quên giảm modulo 360. 

Vấn đề phổ biến thứ ba là thời gian xử lý sau 12 giờ một cách chính xác. 

Đối với đầu vào:```
23:59
```đồng hồ analog sẽ hoạt động như thế nào`11:59`, không`23:59`. Giá trị giờ phải giảm theo modulo 12 trước khi tính góc. 

Góc giờ đúng là:```
11 * 30 + 59 * 0.5 = 359.5
```và góc phút là:```
59 * 6 = 354
```## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng chuyển động của đồng hồ từng phút từ`12:00`cho đến khi đạt được thời gian mục tiêu. Vì kim giờ chuyển động liên tục nên chúng ta có thể tăng kim phút thêm 6 độ mỗi phút và kim giờ tăng thêm 0,5 độ mỗi phút. 

Cách tiếp cận này đúng về mặt khái niệm vì đồng hồ analog phát triển một cách rõ ràng theo thời gian. Sau khi tăng số phút chính xác, cả hai tay đều ở đúng vị trí của chúng. 

Điểm yếu là mô phỏng giải quyết được vấn đề lớn hơn nhiều so với mức cần thiết. Mặc dù chỉ có 1440 phút trong một ngày, việc lặp lại từng phút là không cần thiết khi vị trí cuối cùng có thể được tính trực tiếp từ hình học. 

Quan sát quan trọng là cả hai tay đều chuyển động với tốc độ góc không đổi. 

Kim phút hoàn thành một vòng quay 360 độ đầy đủ cứ sau 60 phút, do đó nó di chuyển:```
360 / 60 = 6 degrees per minute
```Kim giờ hoàn thành một vòng quay hết 12 giờ hoặc 720 phút, do đó nó di chuyển:```
360 / 720 = 0.5 degrees per minute
```Một khi chúng ta biết những tốc độ góc này, câu trả lời sẽ trở thành một công thức đánh giá trực tiếp. 

Góc của kim phút là:```
minutes * 6
```Góc của kim giờ là:```
(hours mod 12) * 30 + minutes * 0.5
```Toàn bộ vấn đề giảm xuống còn việc phân tích cú pháp đầu vào một cách chính xác và áp dụng các công thức này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1440) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Công thức toán học trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào trong`HH:MM`định dạng. 
2. Chia chuỗi xung quanh`:`để trích xuất các giá trị giờ và phút dưới dạng số nguyên. 
3. Chuyển đổi giờ sang định dạng 12 giờ bằng cách sử dụng:```
hour %= 12
```Đồng hồ analog lặp lại sau mỗi 12 giờ, vì vậy`13:00`cư xử như`1:00`. 
4. Tính góc kim phút:```
minute_angle = minute * 6
```Kim phút quay 360 độ trong 60 phút. 
5. Tính góc kim giờ:```
hour_angle = hour * 30 + minute * 0.5
```Mỗi giờ đóng góp 30 độ và mỗi phút đóng góp thêm 0,5 độ vì kim giờ chuyển động liên tục. 
6. In góc kim giờ và góc kim phút. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến là cả hai kim đồng hồ đều chuyển động với vận tốc góc không đổi. 

Kim phút quay 60 phút một lần nên vị trí của nó chỉ phụ thuộc vào số phút hiện tại. Kim giờ cứ 12 giờ lại quay một lần và liên tục tiến lên khi phút trôi qua. Vì chuyển động góc là tuyến tính nên vị trí chính xác tại bất kỳ thời điểm nào cũng có thể được tính toán trực tiếp từ thời gian đã trôi qua mà không cần mô phỏng. 

Các công thức khớp với chuyển động vật lý của đồng hồ kim thực, do đó các góc được tính toán luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

h, m = map(int, s.split(':'))

h %= 12

hour_angle = h * 30 + m * 0.5
minute_angle = m * 6

print(hour_angle, minute_angle)
```Giải pháp bắt đầu bằng cách đọc chuỗi thời gian và chia nó thành các thành phần giờ và phút. 

Sự chuyển đổi`h %= 12`là điều cần thiết vì đồng hồ analog lặp lại sau mỗi 12 giờ. Nếu không có bước này, những lúc như`23:00`sẽ tạo ra những góc không thể lớn hơn 360 độ. 

Việc tính toán kim phút rất đơn giản vì mỗi phút tương ứng với chính xác 6 độ. 

Việc tính toán kim giờ là nơi xảy ra nhiều sai sót nhất. Mỗi giờ đóng góp 30 độ vì:```
360 / 12 = 30
```Nhưng kim giờ cũng tiến liên tục trong giờ hiện tại. Mỗi phút sẽ dịch chuyển nó bằng cách:```
30 / 60 = 0.5 degrees
```Sử dụng số học dấu phẩy động ở đây là hoàn toàn an toàn vì tất cả các giá trị đều là bội số của`0.5`, dấu phẩy động nhị phân nào thể hiện chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12:00
```| Biến | Giá trị | 
| --- | --- | 
| h | 12 | 
| m | 0 | 
| h % 12 | 0 | 
| giờ_góc | 0 * 30 + 0 * 0,5 = 0 | 
| phút_góc | 0 * 6 = 0 | 

Đầu ra:```
0 0
```Ví dụ này xác nhận rằng trạng thái đồng hồ ban đầu đã khớp`12:00`, nên cả hai tay đều không cần xoay. 

### Ví dụ 2 

đầu vào:```
04:30
```| Biến | Giá trị | 
| --- | --- | 
| h | 4 | 
| m | 30 | 
| h % 12 | 4 | 
| giờ_góc | 4 * 30 + 30 * 0,5 = 135 | 
| phút_góc | 30 * 6 = 180 | 

Đầu ra:```
135.0 180
```Dấu vết này chứng tỏ tại sao kim giờ phải bao gồm tiến trình từng phút. Lúc 4:30 kim giờ ở giữa số 4 và số 5, không hẳn là ở số 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì nó thực hiện các phép tính theo thời gian không đổi bất kể đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()

    h, m = map(int, s.split(':'))

    h %= 12

    hour_angle = h * 30 + m * 0.5
    minute_angle = m * 6

    print(hour_angle, minute_angle)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("12:00\n") == "0.0 0", "sample 1"

# custom cases
assert run("00:00\n") == "0.0 0", "midnight handling"

assert run("04:30\n") == "135.0 180", "continuous hour-hand movement"

assert run("23:59\n") == "359.5 354", "modulo 12 conversion"

assert run("06:00\n") == "180.0 0", "exact opposite direction"

assert run("01:01\n") == "30.5 6", "small offset after one minute"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`00:00`|`0.0 0`| Xử lý modulo nửa đêm | 
|`04:30`|`135.0 180`| Chuyển động kim giờ liên tục | 
|`23:59`|`359.5 354`| Đúng chuyển đổi 24 giờ sang 12 giờ | 
|`06:00`|`180.0 0`| Nửa vòng quay chính xác | 
|`01:01`|`30.5 6`| Đóng góp phút cho kim giờ | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
04:30
```Thuật toán tính toán:```
hour_angle = 4 * 30 + 30 * 0.5 = 135
minute_angle = 30 * 6 = 180
```phần bổ sung`15`độ tính từ phút đặt kim giờ chính xác ở giữa số 4 và số 5. ​​Giải pháp bỏ qua phần đóng góp số phút sẽ đưa ra kết quả không chính xác`120`. 

Bây giờ hãy xem xét:```
00:00
```Sau khi áp dụng:```
h %= 12
```giờ trở thành`0`. Các đầu ra thuật toán:```
0.0 0
```Nếu không có hoạt động modulo, việc triển khai có lỗi có thể tính toán`720`độ cho kim giờ. 

Cuối cùng, hãy xem xét:```
23:59
```Thuật toán chuyển đổi:```
23 % 12 = 11
```Sau đó tính toán:```
hour_angle = 11 * 30 + 59 * 0.5 = 359.5
minute_angle = 59 * 6 = 354
```Thao tác này đặt kim giờ ngay trước số 12 và kim phút ở gần cuối mặt đồng hồ một cách chính xác.
