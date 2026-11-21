---
title: "CF 254B - Kích thước ban giám khảo"
description: "Mỗi kỳ Olympic diễn ra vào một ngày cố định trong năm 2013. Trước ngày đó, một nhóm thành viên ban giám khảo phải làm việc liên tục trong nhiều ngày. Nếu một cuộc thi Olympic cần p người và t ngày chuẩn bị, thì chính xác p người sẽ bận rộn trong t ngày ngay trước Olympic."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 254
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 155 (Div. 2)"
rating: 1500
weight: 254
solve_time_s: 892
verified: true
draft: false
---

[CF 254B - Quy mô ban giám khảo](https://codeforces.com/problemset/problem/254/B) 

**Đánh giá:** 1500 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 14 phút 52 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi kỳ Olympic diễn ra vào một ngày cố định trong năm 2013. Trước ngày đó, một nhóm thành viên ban giám khảo phải làm việc liên tục trong nhiều ngày. Nếu một cuộc thi Olympic cần`p`mọi người và`t`ngày chuẩn bị thì chính xác`p`mọi người đang bận rộn trên mỗi`t`ngày ngay trước Thế vận hội. 

Cùng một người không thể chuẩn bị hai kỳ thi Olympic khác nhau trong cùng một ngày. Chúng ta cần tổng số thành viên ban giám khảo tối thiểu để mọi lịch trình chuẩn bị có thể diễn ra đồng thời. 

Toàn bộ vấn đề rút gọn thành một câu hỏi: 

Mỗi ngày dương lịch cần bao nhiêu người cùng một lúc? 

Câu trả lời là mức tối đa của những yêu cầu hàng ngày đó. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 kỳ thi Olympic và thời gian chuẩn bị cho mỗi kỳ nhiều nhất là 100 ngày. Ngay cả khi chúng tôi mô phỏng rõ ràng từng ngày chuẩn bị, tổng công việc vẫn xấp xỉ`100 * 100 = 10,000`hoạt động, đó là rất nhỏ. 

Khó khăn chính không phải là hiệu suất mà là việc xử lý ngày tháng một cách chính xác. 

Một số trường hợp đặc biệt dễ dàng tạo ra những sai lầm riêng lẻ. 

Giả sử một kỳ thi Olympic diễn ra vào ngày 10 tháng 3 và cần 3 ngày chuẩn bị. Công việc diễn ra vào ngày 7 tháng 3, ngày 8 tháng 3 và ngày 9 tháng 3. Không bao gồm ngày 10 tháng 3. Việc thực hiện bất cẩn có thể vô tình tính cả ngày thi Olympic. 

Ví dụ:```
1
3 10 5 3
```Câu trả lời đúng:```
5
```Những ngày chiếm đóng chỉ từ ngày 7 tháng 3 đến ngày 9 tháng 3. 

Một trường hợp phức tạp khác là việc chuẩn bị bước sang năm 2012. 

Ví dụ:```
1
1 3 4 5
```Ngày chuẩn bị là từ ngày 29 tháng 12 đến ngày 2 tháng 1. Nếu chúng ta chỉ lưu trữ các ngày trong năm 2013, chúng ta sẽ mất một phần khoảng thời gian. 

Câu trả lời đúng:```
4
```Một số Olympic có thể diễn ra trong cùng một ngày và khoảng thời gian chuẩn bị của chúng có thể trùng lặp hoàn toàn. 

Ví dụ:```
2
5 20 3 2
5 20 4 2
```Cả hai đều yêu cầu làm việc vào ngày 18 tháng 5 và ngày 19 tháng 5. 

Câu trả lời đúng:```
7
```Một giải pháp có lỗi xử lý các sự kiện một cách độc lập mà không tích lũy tải hàng ngày sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng từng ngày chuẩn bị cho mỗi kỳ Olympic. 

Đối với mỗi kỳ Olympic, chúng tôi xác định chính xác ngày mà các thành viên ban giám khảo làm việc. Sau đó chúng tôi thêm`p[i]`với khối lượng công việc của mỗi ngày đó. Sau khi xử lý tất cả các Olympic, chúng tôi quét tất cả các ngày và lấy khối lượng công việc tối đa. 

Điều này hiệu quả vì xung đột giữa bồi thẩm đoàn hoàn toàn mang tính cục bộ trong một ngày. Nếu một ngày nào đó cần có 12 người cùng một lúc thì phải có ít nhất 12 thành viên ban giám khảo. Ngược lại, nếu yêu cầu đồng thời tối đa là 12, chúng tôi có thể sử dụng lại 12 người đó trong các ngày khác nhau. 

Việc triển khai ngây thơ có thể biểu thị ngày tháng như`(month, day)`theo cặp và liên tục lùi lại từng ngày một bằng cách sử dụng logic lịch. Ở đây tốc độ đó đã đủ nhanh vì tổng số ngày mô phỏng còn nhỏ. 

Có một cách tiếp cận thậm chí còn sạch hơn. Chuyển đổi mỗi ngày thành một chỉ số ngày nguyên duy nhất. Ví dụ: ngày 1 tháng 1 trở thành ngày 1, ngày 1 tháng 2 trở thành ngày 32, v.v. Khi ngày tháng trở thành số nguyên, mỗi khoảng thời gian chuẩn bị chỉ đơn giản là một đoạn liền kề trên trục số. 

Sau đó, chúng ta chỉ cần một mảng lưu trữ số lượng thành viên ban giám khảo bận rộn mỗi ngày. 

Đối với một Olympic diễn ra vào ngày`D`với chiều dài chuẩn bị`t`, khoảng thời gian bị chiếm dụng là:```
[D - t, D - 1]
```Chúng tôi thêm`p`đến mọi ngày trong khoảng thời gian đó. 

Vì lịch chỉ có 365 ngày và việc chuẩn bị có thể kéo dài khoảng 100 ngày so với năm trước nên ngay cả việc cập nhật phạm vi trực tiếp bằng cách lặp lại cũng không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với bước ngày thủ công | O(n · t) | O(365) | Đã chấp nhận | 
| Tối ưu với chuyển đổi chỉ số ngày | O(n · t) | O(500) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng chứa số ngày trong mỗi tháng của năm 2013. 
2. Xây dựng hàm trợ giúp chuyển đổi ngày dương lịch`(month, day)`vào số ngày của nó trong năm. 

Ví dụ:```
Jan 1  -> 1
Feb 1  -> 32
Mar 1  -> 60
```3. Tạo một mảng`cnt`đủ lớn để lưu trữ khối lượng công việc trong những ngày trước ngày 1 tháng 1. 

Chúng tôi sử dụng phần bù để chỉ số ngày âm không bao giờ xuất hiện. 
4. Đối với mỗi kỳ Olympic: 

Chuyển đổi ngày của nó thành một số nguyên`D`. 
5. Khoảng thời gian chuẩn bị là từ`D - t`ĐẾN`D - 1`. 

Đây chính xác là những ngày trước Olympic. 
6. Đối với mỗi ngày trong khoảng thời gian đó, hãy thêm`p`ĐẾN`cnt[day]`. 

Điều này có nghĩa`p`các thành viên bồi thẩm đoàn đều bận rộn vào mỗi ngày đó. 
7. Sau khi xử lý tất cả các Olympic, hãy quét mảng và tìm giá trị lớn nhất. 
8. In tối đa. 

### Tại sao nó hoạt động 

Đối với mỗi ngày,`cnt[d]`bằng tổng số thành viên bồi thẩm đoàn được yêu cầu đồng thời trong ngày hôm đó. 

Bất kỳ lịch trình hợp lệ nào cũng phải chứa ít nhất`cnt[d]`mọi người mỗi ngày`d`, bởi vì những sự chuẩn bị đó trùng lặp về mặt thời gian và không thể chia sẻ cho mọi người. 

Nếu nhu cầu tối đa hàng ngày là`M`, sau đó`M`người cũng đủ. Chúng ta có thể chỉ cần phân công những người riêng biệt cho tất cả các nhiệm vụ đang hoạt động mỗi ngày, sử dụng lại chúng một cách thoải mái vào những ngày khác nhau. 

Vì vậy, số lượng bồi thẩm đoàn tối thiểu có thể chính xác là khối lượng công việc tối đa hàng ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MONTHS = [31, 28, 31, 30, 31, 30,
          31, 31, 30, 31, 30, 31]

def day_number(month, day):
    total = 0
    for i in range(month - 1):
        total += MONTHS[i]
    return total + day

def solve():
    n = int(input())

    OFFSET = 200
    cnt = [0] * 1000

    for _ in range(n):
        m, d, p, t = map(int, input().split())

        D = day_number(m, d)

        start = D - t
        end = D - 1

        for day in range(start, end + 1):
            cnt[day + OFFSET] += p

    print(max(cnt))

solve()
```Hàm trợ giúp chuyển đổi ngày thành số ngày tuần tự. Điều này loại bỏ tất cả các biến chứng lịch khỏi logic chính. 

Mảng sử dụng giá trị bù vì quá trình chuẩn bị có thể bắt đầu trước ngày 1 tháng 1. Ví dụ: ngày 3 tháng 1 với`t = 5`cần ngày`-2, -1, 0, 1, 2`so với cách đánh số của năm. Danh sách Python không cho phép các chỉ số logic âm theo cách chúng tôi muốn ở đây, vì vậy chúng tôi dịch chuyển mọi thứ bằng một độ lệch không đổi. 

Ranh giới khoảng cách là phần tế nhị nhất của việc thực hiện. 

Nếu Olympic diễn ra vào ngày`D`, việc chuẩn bị chiếm:```
[D - t, D - 1]
```ngày`D`bản thân nó không được đưa vào. 

Vòng lặp:```
for day in range(start, end + 1):
```bao gồm chính xác cả hai đầu của khoảng thời gian chuẩn bị. 

Kích thước mảng cố ý lớn hơn nhiều so với mức cần thiết. Số ngày tối đa là 365 và thời gian chuẩn bị tối đa là 100, do đó, mảng có kích thước 1000 là an toàn thoải mái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
5 23 1 2
3 13 2 3
```Olympic ngày 23/5 cần 1 người vào ngày 21/5 và 22/5. 

Olympic ngày 13/3 cần 2 người vào các ngày 10/3, 11/3 và 12/3. 

| Olympic | Ngày chuẩn bị | Đã thêm người | 
| --- | --- | --- | 
| 23 tháng 5 | 21 tháng 5, 22 tháng 5 | +1 | 
| 13 tháng 3 | Ngày 10, 11, 12 tháng 3 | +2 | 

Khối lượng công việc hàng ngày trở thành: 

| Ngày | Khối lượng công việc | 
| --- | --- | 
| Ngày 10 tháng 3 | 2 | 
| Ngày 11 tháng 3 | 2 | 
| 12 tháng 3 | 2 | 
| 21 tháng 5 | 1 | 
| 22 tháng 5 | 1 | 

Tối đa là`2`. 

Đầu ra:```
2
```Ví dụ này cho thấy rằng các giai đoạn chuẩn bị không trùng lặp có thể sử dụng lại cùng các thành viên ban giám khảo. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
1 5 3 3
1 6 2 2
1 7 4 1
```Khoảng thời gian chuẩn bị: 

| Olympic | Ngày chuẩn bị | Đã thêm người | 
| --- | --- | --- | 
| Ngày 5 tháng 1 | Ngày 2, 3, 4 tháng 1 | +3 | 
| Ngày 6 tháng 1 | Ngày 4 tháng 1 năm 5 | +2 | 
| Ngày 7 tháng 1 | Ngày 6 tháng 1 | +4 | 

Khối lượng công việc tích lũy: 

| Ngày | Tổng cộng | 
| --- | --- | 
| Ngày 2 tháng 1 | 3 | 
| Ngày 3 tháng 1 | 3 | 
| Ngày 4 tháng 1 | 5 | 
| Ngày 5 tháng 1 | 2 | 
| Ngày 6 tháng 1 | 4 | 

Tối đa là`5`. 

Đầu ra:```
5
```Dấu vết này cho thấy các khoảng thời gian chồng chéo tích lũy các yêu cầu bồi thẩm đoàn của họ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · t) | Mỗi Olympic cập nhật tối đa 100 ngày | 
| Không gian | O(1) | Mảng khối lượng công việc có kích thước cố định | 

Trường hợp xấu nhất thực hiện khoảng 10.000 cập nhật, rất nhỏ trong giới hạn 1 giây. Việc sử dụng bộ nhớ cũng không đáng kể vì kích thước mảng không phụ thuộc vào cường độ đầu vào theo bất kỳ cách nào có ý nghĩa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MONTHS = [31, 28, 31, 30, 31, 30,
          31, 31, 30, 31, 30, 31]

def solve_io(inp: str) -> str:
    input = io.StringIO(inp).readline

    def day_number(month, day):
        total = 0
        for i in range(month - 1):
            total += MONTHS[i]
        return total + day

    n = int(input())

    OFFSET = 200
    cnt = [0] * 1000

    for _ in range(n):
        m, d, p, t = map(int, input().split())

        D = day_number(m, d)

        for day in range(D - t, D):
            cnt[day + OFFSET] += p

    return str(max(cnt))

def run(inp: str) -> str:
    return solve_io(inp).strip()

# provided sample
assert run(
"""2
5 23 1 2
3 13 2 3
"""
) == "2", "sample 1"

# minimum input
assert run(
"""1
1 1 1 1
"""
) == "1", "minimum case"

# fully overlapping intervals
assert run(
"""2
5 20 3 2
5 20 4 2
"""
) == "7", "complete overlap"

# crossing into previous year
assert run(
"""1
1 3 4 5
"""
) == "4", "cross-year preparation"

# no overlap at all
assert run(
"""3
1 10 2 1
2 10 3 1
3 10 4 1
"""
) == "4", "independent schedules"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Olympic đơn vào ngày 1 tháng 1 | 1 | Xử lý đúng ngày sớm nhất | 
| Hai khoảng giống hệt nhau | 7 | Tích lũy chồng chéo hợp lý | 
| Chuẩn bị bắt đầu từ năm 2012 | 4 | Xử lý ngày tương đối âm | 
| Khoảng thời gian hoàn toàn riêng biệt | 4 | Ban giám khảo tái sử dụng qua các ngày khác nhau | 

## Vỏ cạnh 

Hãy cân nhắc việc chuẩn bị chuyển sang năm trước. 

đầu vào:```
1
1 3 4 5
```Ngày 3 tháng 1 tương ứng với ngày thứ 3. Khoảng thời gian chuẩn bị là:```
[3 - 5, 3 - 1] = [-2, 2]
```Thuật toán lưu trữ các giá trị này một cách an toàn bằng cách sử dụng offset: 

| Ngày Tương Đối | Chỉ mục được lưu trữ | Khối lượng công việc | 
| --- | --- | --- | 
| -2 | 198 | 4 | 
| -1 | 199 | 4 | 
| 0 | 200 | 4 | 
| 1 | 201 | 4 | 
| 2 | 202 | 4 | 

Tối đa trở thành`4`, điều đó đúng. 

Bây giờ hãy xem xét ranh giới riêng biệt xung quanh ngày Olympic. 

đầu vào:```
1
3 10 5 3
```Số ngày của ngày 10 tháng 3 là 69. 

Khoảng thời gian chuẩn bị là:```
[66, 68]
```Chỉ có ba ngày được cập nhật: 

| Ngày | Lịch Ngày | 
| --- | --- | 
| 66 | Ngày 7 tháng 3 | 
| 67 | 8 tháng 3 | 
| 68 | Ngày 9 tháng 3 | 

Ngày 10 tháng 3 không bao giờ được tính. Câu trả lời là chính xác`5`. 

Cuối cùng, hãy cân nhắc việc chuẩn bị chồng chéo với các phạm vi khác nhau. 

đầu vào:```
2
1 10 3 5
1 12 4 3
```Khoảng thời gian:```
Jan 5 to Jan 9
Jan 9 to Jan 11
```Sự chồng chéo chỉ xảy ra vào ngày 9 tháng 1. 

| Ngày | Tổng số ban giám khảo cần thiết | 
| --- | --- | 
| Ngày 5 tháng 1 | 3 | 
| Ngày 6 tháng 1 | 3 | 
| Ngày 7 tháng 1 | 3 | 
| Ngày 8 tháng 1 | 3 | 
| Ngày 9 tháng 1 | 7 | 
| Ngày 10 tháng 1 | 4 | 
| Ngày 11 tháng 1 | 4 | 

Thuật toán tích lũy cả đóng góp vào ngày chia sẻ và kết quả đầu ra`7`.
