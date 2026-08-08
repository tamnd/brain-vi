---
title: "CF 102431A - Khởi động"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi có lịch trình các vòng Kick Start năm 2019 và ngày đại diện cho ngày hôm nay. Ngày đã lên lịch có thể xuất hiện theo bất kỳ thứ tự nào. Chúng ta cần tìm ngày đã lên lịch hoàn toàn sau ngày hôm nay và càng sớm càng tốt."
date: "2026-08-08T17:14:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "A"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 194
verified: true
draft: false
---

[CF 102431A - Khởi động](https://codeforces.com/problemset/problem/102431/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi có lịch trình các vòng Kick Start năm 2019 và ngày đại diện cho ngày hôm nay. Ngày đã lên lịch có thể xuất hiện theo bất kỳ thứ tự nào. Chúng ta cần tìm ngày đã lên lịch hoàn toàn sau ngày hôm nay và càng sớm càng tốt. Nếu mọi vòng đấu được lên lịch đều diễn ra vào hôm nay hoặc sớm hơn thì không còn vòng đấu nào trong năm 2019, vì vậy câu trả lời bắt buộc là`See you next year`. 

Ngày sử dụng tên tháng như`Jan`,`Feb`, Và`Sept`, theo sau là số thứ tự ngày như`1st`,`22nd`, hoặc`31st`. Vì mọi ngày đều thuộc năm 2019 nên việc so sánh hai ngày chỉ cần so sánh tháng và ngày của chúng. Chúng ta có thể chuyển đổi mỗi ngày thành một cặp`(month_number, day)`và sử dụng so sánh từ điển thông thường. 

Có tối đa 20 vòng được lên lịch trong một trường hợp kiểm thử và nhiều nhất là 100 trường hợp kiểm thử. Đây là kích thước đầu vào rất nhỏ, do đó, ngay cả một phương pháp thực hiện hàng trăm hoặc vài nghìn thao tác đơn giản cho mỗi trường hợp thử nghiệm cũng có thể nhanh chóng thoải mái. Đặc biệt, không cần cấu trúc dữ liệu nâng cao hoặc thuật toán sắp xếp phức tạp. Việc quét trực tiếp qua các ngày đã lên lịch chỉ mất`O(n)`thời gian, ở đâu`n <= 20`. 

Điều kiện ranh giới chính là bản thân ngày hôm nay không được coi là vòng tiếp theo. Ví dụ: nếu lịch trình có`Jan 2nd`và hôm nay là`Jan 2nd`, câu trả lời là không`Jan 2nd`. Nếu không có ngày sau đó thì kết quả đúng là`See you next year`. 

Trường hợp cạnh thứ hai xảy ra khi vòng tiếp theo diễn ra vào tháng sau. Ví dụ:```
1
2
Jan 31st
Feb 1st
Jan 31st
```Đầu ra đúng là:```
Case #1: Feb 1st
```Sự so sánh chỉ dựa trên số ngày sẽ không chính xác`Jan 31st`hoặc không sắp xếp ngày đúng cách. Tháng phải là một phần của so sánh. 

Trường hợp cạnh thứ ba xảy ra khi hôm nay diễn ra sau mỗi vòng đấu đã lên lịch. Ví dụ:```
1
2
Jan 10th
Mar 20th
Dec 31st
```Đầu ra đúng là:```
Case #1: See you next year
```Việc triển khai bất cẩn có thể trả về ngày cuối cùng mà nó kiểm tra thay vì kiểm tra rõ ràng liệu vòng sau có tồn tại hay không. 

Câu lệnh đầu vào đảm bảo rằng các ngày đã lên lịch là khác nhau, do đó hai vòng đã lên lịch không thể chiếm cùng một ngày. Tuy nhiên, ngày đại diện cho ngày hôm nay có thể bằng một trong các ngày đã lên lịch và sự bình đẳng đó phải được loại trừ vì vấn đề yêu cầu một vòng hoàn toàn muộn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là kiểm tra mọi ngày theo lịch sau ngày hôm nay, bắt đầu từ ngày mai, cho đến cuối năm 2019. Đối với mỗi ngày của ứng viên, chúng tôi có thể quét tất cả các vòng đã lên lịch và kiểm tra xem ứng viên đó có mặt hay không. Ngày phù hợp đầu tiên là câu trả lời. Điều này hiệu quả vì ngày được sắp xếp tự nhiên và ngày được lên lịch đầu tiên sau ngày hôm nay phải là ngày sớm nhất có thể. 

Với tối đa 365 ngày trong một năm và tối đa 20 vòng theo lịch trình, điều này thực hiện tối đa khoảng`365 * 20 = 7300`so sánh ngày cho mỗi trường hợp thử nghiệm. Trên 100 trường hợp thử nghiệm, tối đa là khoảng 730.000 so sánh, đủ nhanh. Vì vậy, mặc dù kém tinh tế hơn, phương pháp vũ phu này vẫn được chấp nhận với những ràng buộc nhất định. 

Một cách tiếp cận rõ ràng hơn xuất phát từ việc nhận thấy rằng chúng ta thực sự không cần phải liệt kê lịch. Đối với mỗi ngày đã lên lịch, chúng ta có thể hỏi một câu hỏi: ngày này có muộn hơn ngày hôm nay không? Nếu có thì đó là một ứng cử viên. Trong số tất cả các ứng cử viên, chúng tôi giữ người nhỏ nhất. Điều này ngay lập tức giảm thiểu vấn đề chỉ bằng một lần quét`n`những ngày đã lên lịch. 

Việc so sánh trở nên đơn giản sau khi chuyển đổi một ngày thành`(month, day)`. Ví dụ,`Mar 24th`trở thành`(3, 24)`, trong khi`Apr 20th`trở thành`(4, 20)`. Python so sánh các bộ dữ liệu theo từ điển, vì vậy`(4, 20)`được xem xét chính xác muộn hơn`(3, 24)`. 

Phương pháp brute-force hoạt động vì lịch có số ngày cố định rất nhỏ nhưng nó thực hiện công việc vào những ngày thậm chí không được lên lịch. Việc quét trực tiếp sẽ tránh hoàn toàn những công việc không cần thiết đó. Từ`n`chỉ là 20, giải pháp tối ưu chỉ đơn giản là kiểm tra từng vòng đã lên lịch một lần và duy trì ứng viên hợp lệ sớm nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(365n)`|`O(n)`| Đã chấp nhận nhưng công việc không cần thiết | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu tên tháng theo thứ tự thời gian bằng cách gán`Jan`đến 1,`Feb`đến 2, v.v. Đầu vào sử dụng`Sept`, do đó, chính tả phải được đưa vào ánh xạ. 
2. Chuyển ngày hôm nay thành một cặp`(month, day)`. Ngày được biểu thị theo cách này có thể được so sánh trực tiếp với ngày khác vì tháng là thành phần đầu tiên và ngày là thành phần thứ hai. 
3. Đọc từng câu`n`ngày đã lên lịch và chuyển đổi nó thành cùng một ngày`(month, day)`đại diện. Giữ nguyên văn bản gốc vì đầu ra phải sử dụng định dạng thứ tự ban đầu như`Feb 2nd`. 
4. Đối với mỗi ngày đã lên lịch, hãy kiểm tra xem liệu ngày đó có đúng không`(month, day)`cặp này hoàn toàn lớn hơn cặp ngày nay. Cần phải so sánh chặt chẽ vì vòng diễn ra hôm nay không phải là vòng tương lai. 
5. Trong số tất cả các ngày hoàn toàn muộn hơn ngày hôm nay, hãy giữ ngày có cặp ngày nhỏ nhất. Đây chính xác là vòng dự kiến ​​tiếp theo, bởi vì mọi ứng cử viên trước đó đều đã bị từ chối hoặc bị thay thế bởi ứng viên sau, trong khi ứng cử viên nhỏ nhất còn lại sẽ là ngày được lên lịch đầu tiên sau ngày hôm nay. 
6. Nếu không có ngày nào được lên lịch muộn hơn ngày hôm nay, hãy in`See you next year`. Nếu không, hãy in văn bản gốc của ngày đã lên lịch đã chọn. 

### Tại sao nó hoạt động 

Duy trì tính bất biến sau khi xử lý bất kỳ tiền tố nào của lịch trình,`best`là ngày được lên lịch sớm nhất trong tiền tố đó và diễn ra hoàn toàn sau ngày hôm nay. Khi một ngày mới không muộn hơn ngày hôm nay thì đó không thể là câu trả lời và bị bỏ qua. Khi muộn hơn ngày hôm nay, nó sẽ trở thành`best`nếu không có ứng cử viên nào trước đó tồn tại hoặc thay thế`best`nếu nó xảy ra trước ứng cử viên hiện tại. Sau khi tất cả các ngày đã lên lịch đã được xử lý, bất biến sẽ nói rằng`best`là vòng đấu được lên lịch sớm nhất sau ngày hôm nay. Nếu không tìm thấy ứng cử viên nào thì sẽ không có vòng đấu theo lịch nào diễn ra sau ngày hôm nay, vì vậy`See you next year`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MONTH = {
    "Jan": 1,
    "Feb": 2,
    "Mar": 3,
    "Apr": 4,
    "May": 5,
    "Jun": 6,
    "Jul": 7,
    "Aug": 8,
    "Sept": 9,
    "Oct": 10,
    "Nov": 11,
    "Dec": 12,
}

def parse_date(s):
    month, day = s.split()
    day = int(day.rstrip("stndrh"))
    return MONTH[month], day

def solve():
    t = int(input())

    for case in range(1, t + 1):
        n = int(input())

        schedule = []
        for _ in range(n):
            s = input().strip()
            schedule.append((parse_date(s), s))

        today_text = input().strip()
        today = parse_date(today_text)

        best_date = None
        best_text = None

        for date, text in schedule:
            if date > today:
                if best_date is None or date < best_date:
                    best_date = date
                    best_text = text

        if best_text is None:
            answer = "See you next year"
        else:
            answer = best_text

        print(f"Case #{case}: {answer}")

if __name__ == "__main__":
    solve()
```các`MONTH`từ điển chuyển đổi chữ viết tắt của tháng trong văn bản thành vị trí theo thời gian của nó. Điều này tránh việc cố gắng so sánh các chuỗi như`Apr`Và`Feb`, thứ tự chữ cái của nó không liên quan đến thứ tự lịch.`parse_date`phân biệt tháng và ngày thứ tự. Loại bỏ hậu tố thứ tự để lại một ngày nguyên, vì vậy`2nd`trở thành`2`Và`24th`trở thành`24`. Hậu tố này không liên quan về mặt ngữ nghĩa để so sánh ngày tháng. 

Mỗi mục nhập lịch trình lưu trữ cả cặp ngày chuẩn hóa và văn bản gốc của nó. Cặp chuẩn hóa được sử dụng để so sánh, trong khi chuỗi gốc được trả về trực tiếp để đầu ra giữ nguyên chính tả và hậu tố thứ tự được yêu cầu. 

điều kiện`date > today`là kiểm tra ranh giới quan trọng. sử dụng`>=`sẽ chọn không chính xác vòng thi hôm nay khi hôm nay chính là ngày đã lên lịch. Điều kiện thứ hai so sánh các ứng cử viên và giữ lại điều kiện nhỏ nhất, thực hiện định nghĩa của vòng tiếp theo. 

Không có vấn đề tràn số nguyên vì giá trị lớn nhất liên quan là số tháng là 12 và số ngày là 31. Đầu vào cũng rất nhỏ, vì vậy là tiêu chuẩn`sys.stdin.readline`đã quá đủ cho khối lượng I/O cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, lịch trình là`Jan 1st`,`Feb 2nd`, Và`Mar 3rd`, trong khi hôm nay là`Jan 2nd`. 

| Ngày dự kiến ​​| Ngày phân tích | Muộn hơn hôm nay? | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
|`Jan 1st`|`(1, 1)`| Không | Không có | 
|`Feb 2nd`|`(2, 2)`| Có |`Feb 2nd`| 
|`Mar 3rd`|`(3, 3)`| Có |`Feb 2nd`| 

Ngày đầu tiên là trước ngày hôm nay và bị loại bỏ.`Feb 2nd`là ứng cử viên hợp lệ đầu tiên.`Mar 3rd`cũng ở tương lai nhưng muộn hơn ứng viên hiện tại nên câu trả lời vẫn là`Feb 2nd`. 

Đối với mẫu thứ hai, ngày dự kiến ​​là`Mar 24th`,`Apr 20th`,`May 26th`,`Jul 28th`,`Aug 25th`,`Sept 29th`,`Oct 19th`, Và`Nov 17th`. Hôm nay là`Nov 17th`. 

| Ngày dự kiến ​​| Ngày phân tích | Muộn hơn hôm nay? | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
|`Mar 24th`|`(3, 24)`| Không | Không có | 
|`Apr 20th`|`(4, 20)`| Không | Không có | 
|`May 26th`|`(5, 26)`| Không | Không có | 
|`Jul 28th`|`(7, 28)`| Không | Không có | 
|`Aug 25th`|`(8, 25)`| Không | Không có | 
|`Sept 29th`|`(9, 29)`| Không | Không có | 
|`Oct 19th`|`(10, 19)`| Không | Không có | 
|`Nov 17th`|`(11, 17)`| Không | Không có | 

Mỗi ngày dự kiến ​​là hôm nay hoặc sớm hơn. Do đó, ứng viên vẫn trống, tạo ra`See you next year`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`mỗi trường hợp thử nghiệm | Mỗi vòng theo lịch trình được phân tích cú pháp và kiểm tra một lần | 
| Không gian |`O(n)`| Lịch trình được lưu trữ cùng với văn bản gốc | 

Với`n <= 20`và tối đa 100 trường hợp thử nghiệm, thuật toán chỉ thực hiện tổng cộng vài nghìn so sánh ngày. Việc sử dụng bộ nhớ cũng rất nhỏ vì mỗi trường hợp kiểm thử lưu trữ tối đa 20 mục lịch trình. 

## Trường hợp thử nghiệm```python
import sys
import io

MONTH = {
    "Jan": 1,
    "Feb": 2,
    "Mar": 3,
    "Apr": 4,
    "May": 5,
    "Jun": 6,
    "Jul": 7,
    "Aug": 8,
    "Sept": 9,
    "Oct": 10,
    "Nov": 11,
    "Dec": 12,
}

def parse_date(s):
    month, day = s.split()
    day = int(day.rstrip("stndrh"))
    return MONTH[month], day

def solve():
    input = sys.stdin.readline
    t = int(input())

    out = []

    for case in range(1, t + 1):
        n = int(input())

        schedule = []
        for _ in range(n):
            s = input().strip()
            schedule.append((parse_date(s), s))

        today = parse_date(input().strip())

        best_date = None
        best_text = None

        for date, text in schedule:
            if date > today:
                if best_date is None or date < best_date:
                    best_date = date
                    best_text = text

        if best_text is None:
            answer = "See you next year"
        else:
            answer = best_text

        out.append(f"Case #{case}: {answer}")

    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Sample 1
assert run(
    """1
3
Jan 1st
Feb 2nd
Mar 3rd
Jan 2nd
"""
) == "Case #1: Feb 2nd\n"

# Sample 2
assert run(
    """1
8
Mar 24th
Apr 20th
May 26th
Jul 28th
Aug 25th
Sept 29th
Oct 19th
Nov 17th
Nov 17th
"""
) == "Case #1: See you next year\n"

# Minimum-size input, with today equal to the only scheduled round
assert run(
    """1
1
Jan 1st
Jan 1st
"""
) == "Case #1: See you next year\n"

# Boundary between months, catches day-only comparison mistakes
assert run(
    """1
2
Jan 31st
Feb 1st
Jan 31st
"""
) == "Case #1: Feb 1st\n"

# Today is the earliest date, so the next round is the minimum
# scheduled date strictly after today
assert run(
    """1
4
Dec 31st
Feb 1st
Jan 2nd
Jun 15th
Jan 1st
"""
) == "Case #1: Jan 2nd\n"

# Maximum-size schedule with all 20 scheduled dates distinct
assert run(
    """1
20
Jan 1st
Jan 2nd
Jan 3rd
Jan 4th
Jan 5th
Jan 6th
Jan 7th
Jan 8th
Jan 9th
Jan 10th
Jan 11th
Jan 12th
Jan 13th
Jan 14th
Jan 15th
Jan 16th
Jan 17th
Jan 18th
Jan 19th
Jan 20th
Jan 10th
"""
) == "Case #1: Jan 11th\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`, đã lên lịch`Jan 1st`, Hôm nay`Jan 1st`|`See you next year`| Kích thước tối thiểu và sự bất bình đẳng nghiêm ngặt | 
|`Jan 31st`,`Feb 1st`, Hôm nay`Jan 31st`|`Feb 1st`| Thứ tự tháng chính xác tại ranh giới tháng | 
| Bốn ngày chưa được sắp xếp, hôm nay`Jan 1st`|`Jan 2nd`| Chọn ngày nhỏ nhất trong tương lai thay vì ngày nhập đầu tiên | 
| Hai mươi ngày tháng một rõ ràng, hôm nay`Jan 10th`|`Jan 11th`| Tối đa`n`và quét tuyến tính chính xác | 
| Nhiều bài kiểm tra trong một đầu vào | tương ứng`Case #x`dòng | Xử lý độc lập và đánh số trường hợp | 

Cụm từ "các giá trị hoàn toàn bằng nhau" yêu cầu một trình độ chuyên môn nhỏ cho vấn đề này. Ngày dự kiến ​​được đảm bảo là khác biệt, vì vậy một trường hợp thử nghiệm về mặt pháp lý không thể chứa nhiều ngày dự kiến ​​giống hệt nhau. Trường hợp đẳng thức hợp lệ gần nhất là khi ngày hôm nay bằng với ngày đã lên lịch, như trong thử nghiệm kích thước tối thiểu ở trên. Trường hợp đó hữu ích hơn ở đây vì nó trực tiếp kiểm tra tính nghiêm ngặt`>`tình trạng. 

## Vỏ cạnh 

Khi hôm nay là một ngày đã được lên lịch, thuật toán sẽ từ chối nó vì điều kiện yêu cầu`date > today`. Ví dụ:```
1
2
Jan 2nd
Feb 2nd
Jan 2nd
```

`Jan 2nd`so sánh bằng ngày hôm nay nên bị bỏ qua.`Feb 2nd`muộn hơn và trở thành ứng cử viên. Đầu ra là:```
Case #1: Feb 2nd
```Khi một ngày trong tương lai vượt qua ranh giới tháng, cả hai thành phần của cặp chuẩn hóa đều quan trọng. Vì:```
1
2
Jan 31st
Feb 1st
Jan 31st
```ngày tháng trở thành`(1, 31)`Và`(2, 1)`. Vì tháng được so sánh đầu tiên,`(2, 1)`là sau này. Câu trả lời là`Feb 1st`, mặc dù số ngày của nó nhỏ hơn. 

Khi mọi ngày đã lên lịch đều sớm hơn ngày hôm nay thì sẽ không có ứng viên nào được lưu trữ. Vì:```
1
3
Jan 10th
Jun 20th
Nov 30th
Dec 31st
```cả ba ngày đều thất bại`date > today`Bài kiểm tra.`best_text`còn lại`None`, do đó thuật toán in ra:```
Case #1: See you next year
```Khi lịch trình không được sắp xếp, thuật toán không dựa vào thứ tự đầu vào. Coi như:```
1
4
Dec 31st
Feb 1st
Jan 2nd
Jun 15th
Jan 1st
```Các ứng viên xuất hiện theo thứ tự`Dec 31st`,`Feb 1st`,`Jan 2nd`, Và`Jun 15th`. Sau khi nhìn thấy`Dec 31st`, nó trở thành tốt nhất hiện tại.`Feb 1st`thay thế nó và`Jan 2nd`thay thế nó một lần nữa. Câu trả lời cuối cùng là`Jan 2nd`. Đây chính xác là lý do tại sao việc duy trì ngày tối thiểu trong tương lai lại được ưu tiên hơn là trả về ngày tương lai đầu tiên gặp phải. 

Khi hôm nay là ngày cuối cùng của năm thì trong năm 2019 không thể có ngày nào muộn hơn. Ví dụ:```
1
1
Dec 31st
Dec 31st
```ngày được lên lịch duy nhất bằng ngày hôm nay nên nó bị loại trừ và kết quả là:```
Case #1: See you next year
```Logic tương tự cũng xử lý lịch trình chứa các ngày trong nhiều tháng mà không yêu cầu bất kỳ trường hợp đặc biệt nào cho tháng 12. Việc bình thường hóa`(month, day)`cách trình bày cung cấp cho mỗi ngày trong năm 2019 một thứ tự thời gian tổng thể, do đó, một quy tắc so sánh sẽ xử lý toàn bộ lịch.
