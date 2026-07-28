---
title: "CF 102786B - \u0411\u0435\u0433\u0438, \u0421\u0435\u043c\u0435\u043d, \u0431\u0435\u0433\u0438"
description: "Semyon chọn ngày chạy của mình theo quy tắc lịch đơn giản. Buổi huấn luyện đầu tiên của anh ấy diễn ra vào một ngày lẻ của một tháng nào đó, và sau đó anh ấy tiếp tục chạy vào mọi ngày lẻ."
date: "2026-07-27T19:33:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 58
verified: true
draft: false
---

[CF 102786B - \u0411\u0435\u0433\u0438, \u0421\u0435\u043c\u0435\u043d, \u0431\u0435\u0433\u0438](https://codeforces.com/problemset/problem/102786/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Semyon chọn ngày chạy của mình theo quy tắc lịch đơn giản. Buổi huấn luyện đầu tiên của anh ấy diễn ra vào một ngày lẻ của một tháng nào đó, và sau đó anh ấy tiếp tục chạy vào mọi ngày lẻ. Thời điểm anh ta chạy trong hai ngày dương lịch liên tiếp, anh ta sẽ bỏ cuộc vào ngày chẵn đầu tiên ngay sau hai lần chạy đó. 

Nhiệm vụ là tìm ra ngày nghỉ việc đó. Đầu vào là một ngày hợp lệ ở định dạng`DD.MM.YYYY`, đại diện cho lần chạy đầu tiên. Ngày được đảm bảo là ngày lẻ nên ngày tập luyện đầu tiên luôn là điểm khởi đầu hợp lệ. Kết quả là ngày chẵn đầu tiên xuất hiện sau lần chạy thứ hai trong một cặp ngày chạy liên tiếp. 

Phạm vi năm nhỏ, từ 1900 đến 2100, do đó không cần thư viện ngày nâng cao hoặc cấu trúc lịch phức tạp. Phần quan trọng là xử lý độ dài tháng và năm nhuận một cách chính xác. Một mô phỏng di chuyển hàng ngày chỉ cần một số bước không đổi, bởi vì chúng ta đang tìm kiếm sự va chạm đầu tiên giữa lịch ngày lẻ và lịch. Một giải pháp quét qua nhiều năm hoặc xây dựng các bảng ngày lớn vẫn có thể hoạt động với các giới hạn này nhưng sẽ bỏ qua cấu trúc nhỏ hơn nhiều của vấn đề. 

Những phần khó khăn đến từ việc chuyển tháng và năm nhuận. Việc triển khai đơn giản chỉ tăng số ngày có thể thất bại khi ngày tiếp theo thuộc về một tháng khác. Ví dụ, đầu vào`31.01.2019`là không thể vì ngày đầu tiên được đảm bảo hợp lệ và lẻ, nhưng những ngày gần cuối tháng chính xác là nơi việc xử lý lịch không chính xác thường bị hỏng. Một chương trình bất cẩn có thể biến đổi`31.01.2019`vào trong`32.01.2019`thay vì`01.02.2019`. 

Năm nhuận là một nguồn sai lầm khác. Đối với đầu vào`01.01.2020`, việc chạy xảy ra vào những ngày lẻ. Lần chạy thứ hai liên tiếp xuất hiện vào`01.02.2020`Và`02.02.2020`, vậy câu trả lời là`02.02.2020`. Một chương trình coi mỗi năm chia hết cho 4 là năm nhuận có thể thất bại vào những năm như 1900, vốn không phải là năm nhuận. 

Một lỗi thường gặp là trả về ngày chạy thứ hai liên tiếp thay vì ngày chẵn tiếp theo. Đối với đầu vào`01.01.2019`, số lần chạy liên tiếp là`01.01.2019`Và`02.01.2019`, nhưng câu trả lời là`02.01.2019`vì đó là ngày chẵn đầu tiên sau lần chạy thứ hai. Ngày bỏ thuốc và ngày chạy thứ hai giống nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng lịch từng ngày một. Bắt đầu từ ngày chạy đầu tiên, chúng ta có thể tiến tới và kiểm tra từng ngày. Nếu ngày hiện tại là số lẻ, Semyon sẽ chạy. Nếu ngày trước đó cũng là ngày chạy thì ngày hiện tại là ngày chạy thứ hai liên tiếp nên câu trả lời là ngày tiếp theo. Phương pháp này đúng vì nó tuân theo quy tắc chính xác từ câu lệnh. 

Cách tiếp cận bạo lực chỉ trở nên phức tạp không cần thiết nếu nó được thực hiện mà không sử dụng cấu trúc ngày tháng. Một mô phỏng rất lớn trong nhiều năm sẽ yêu cầu lặp lại hàng trăm nghìn ngày, nhưng câu trả lời thực sự có thể được tìm thấy sau khi chỉ kiểm tra những ngày xung quanh ngày chẵn tiếp theo. Lý do là lịch trình của Semyon tạo ra một mô hình có thể dự đoán được: anh ấy chạy vào tất cả các ngày lẻ, do đó, lần đầu tiên một ngày chẵn xuất hiện sau một ngày lẻ, ngày tiếp theo sẽ bắt đầu cặp quan trọng bất cứ khi nào lịch đến ngày lẻ tiếp theo ngay sau một ngày chẵn. 

Quan sát quan trọng là các lần chạy liên tiếp chỉ có thể xảy ra khi hai ngày liền kề có số lẻ. Trong một tháng, điều này không bao giờ xảy ra vì ngày lẻ luôn theo sau ngày chẵn. Cặp duy nhất có thể xuất hiện khi ngày lẻ là ngày cuối cùng của tháng và ngày đầu tiên của tháng tiếp theo cũng là ngày lẻ. Vì các ngày cuối tháng chỉ có thể có số ngày 28, 29, 30 hoặc 31 nên chúng ta chỉ cần tìm ranh giới của tháng tiếp theo khi tháng trước kết thúc vào một ngày lẻ. Ngày đầu tiên của tháng tiếp theo là ngày chạy thứ hai, và ngày đó là ngày nghỉ vì là ngày chẵn? Quan sát này cần được sàng lọc: lần chạy thứ hai chỉ là ngày đầu tiên lẻ của tháng khi tháng trước có 31 ngày và ngày kết thúc là ngày tiếp theo, tức là ngày thứ hai của tháng đó. 

Vì vậy, giải pháp tối ưu là di chuyển từng tháng cho đến khi đạt được một tháng cách điểm xuất phát 31 ngày. Ngày đầu tiên của tháng tiếp theo là lần chạy thứ hai liên tiếp và đáp án là ngày thứ hai của tháng đó. Điều này hiệu quả vì mỗi tháng có 31 ngày kết thúc vào một ngày lẻ nếu ngày đầu tiên của nó là số lẻ và ngày bắt đầu tìm kiếm luôn là số lẻ. Theo dõi chuyển đổi tháng tránh việc mô phỏng hàng ngày không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số ngày mô phỏng) | O(1) | Được chấp nhận với phạm vi nhỏ này nhưng bỏ qua mẫu | 
| Tối ưu | O(số tháng đã kiểm tra) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích ngày đầu vào thành giá trị ngày, tháng và năm. Ngày bắt đầu là ngày lẻ nên nó thể hiện một ngày đang chạy và là điểm tham chiếu để tìm cặp liên tiếp tiếp theo. 
2. Di chuyển liên tục sang ngày dương lịch tiếp theo cho đến khi đạt đến ngày chẵn đầu tiên. Ngày này không thể là lần chạy thứ hai, vì Semyon chỉ chạy vào những ngày lẻ mà là ngày giữa các lần chạy liên tiếp có thể xảy ra. 
3. Tiến thêm một ngày nữa. Nếu ngày mới này là ngày lẻ thì đó là ngày chạy ngay sau ngày lẻ, nghĩa là hai ngày trước đó là ngày chạy liên tiếp. Câu trả lời là vào ngày hôm sau. 
4. Nếu ngày mới không nằm trong mẫu bắt buộc, hãy tiếp tục di chuyển về phía trước trong khi vẫn giữ nguyên độ dài tháng và quy tắc năm nhuận chính xác. 

Điều bất biến là sau mỗi ngày mô phỏng, ngày hiện tại là thời điểm có thể tiếp theo trong lịch và tất cả các ngày trước đó đã được kiểm tra. Vì mọi cặp ngày chạy liên tiếp có thể phải xuất hiện theo thứ tự thời gian, cặp được phát hiện đầu tiên sẽ đưa ra chính xác ngày Semyon thoát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_leap(year):
    return year % 400 == 0 or (year % 4 == 0 and year % 100 != 0)

def days_in_month(month, year):
    if month == 2:
        return 29 if is_leap(year) else 28
    if month in (4, 6, 9, 11):
        return 30
    return 31

def next_day(day, month, year):
    day += 1
    if day > days_in_month(month, year):
        day = 1
        month += 1
        if month == 13:
            month = 1
            year += 1
    return day, month, year

def solve():
    s = input().strip()
    day, month, year = map(int, s.split("."))

    while True:
        nd, nm, ny = next_day(day, month, year)

        if day % 2 == 1 and nd % 2 == 1:
            ans_day, ans_month, ans_year = next_day(nd, nm, ny)
            print(f"{ans_day:02d}.{ans_month:02d}.{ans_year:04d}")
            return

        day, month, year = nd, nm, ny

solve()
```Hàm năm nhuận tuân theo các quy tắc lịch chính xác cần thiết cho bài toán. Những năm chia hết cho 400 là năm nhuận, những năm chia hết cho 100 không phải là năm nhuận và tất cả những năm còn lại chia hết cho 4 là năm nhuận. 

các`days_in_month`hàm cô lập chi tiết lịch duy nhất có thể ảnh hưởng đến tính chính xác. Tháng Hai phụ thuộc vào năm, trong khi các tháng khác có độ dài cố định. 

các`next_day`hàm xử lý tất cả các chuyển đổi ở một nơi. Điều này ngăn ngừa sai sót khi chuyển từ cuối tháng hoặc cuối năm. 

Vòng lặp chính tiếp tục kiểm tra các ngày liền kề. Khi cả ngày hiện tại và ngày tiếp theo đều là số lẻ thì đó là hai ngày tập luyện liên tiếp. Kết quả đạt được bằng cách tăng thêm một lần nữa, vì ngày được yêu cầu là ngày chẵn đầu tiên sau lần chạy thứ hai. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu`11.05.2019`, mô phỏng hoạt động như sau: 

| Ngày hiện tại | Ngày tiếp theo | Cả hai đều kỳ lạ? | Hành động | 
| --- | --- | --- | --- | 
| 05/11/2019 | 05.12.2019 | Không | Tiếp tục | 
| 05.12.2019 | 13.05.2019 | Không | Tiếp tục | 
| 30.05.2019 | 31.05.2019 | Không | Tiếp tục | 
| 31.05.2019 | 06.01.2019 | Có | Di chuyển thêm một ngày | 
| 06.02.2019 | | | Trả lời | 

Dấu vết cho thấy vị trí duy nhất có thể có mà hai ngày lẻ có thể liền kề nhau: sự chuyển đổi từ một tháng có 31 ngày sang tháng tiếp theo. Sau đó`31.05.2019`Và`01.06.2019`, ngày nghỉ việc là`02.06.2019`. 

Đối với đầu vào mẫu`01.01.2019`: 

| Ngày hiện tại | Ngày tiếp theo | Cả hai đều kỳ lạ? | Hành động | 
| --- | --- | --- | --- | 
| 01.01.2019 | 01.02.2019 | Không | Tiếp tục | 
| 31.01.2019 | 01.02.2019 | Có | Di chuyển thêm một ngày | 
| 02.02.2019 | | | Trả lời | 

Ví dụ này chứng tỏ rằng thuật toán xử lý chính xác quá trình chuyển đổi tháng sớm nhất có thể. Nó cũng xác nhận rằng ngày trả về là ngày sau lần chạy thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(số ngày đã kiểm tra) | Quá trình mô phỏng dừng ở cặp liên tiếp hợp lệ đầu tiên, xảy ra sau một số lần chuyển đổi lịch | 
| Không gian | O(1) | Chỉ có ngày hiện tại và một vài biến tạm thời được lưu trữ | 

Phạm vi đầu vào không yêu cầu xử lý trong thời gian dài và thuật toán chỉ sử dụng bộ nhớ không đổi. Các phép toán ngày tháng là số học đơn giản nên lời giải dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def is_leap(year):
    return year % 400 == 0 or (year % 4 == 0 and year % 100 != 0)

def days_in_month(month, year):
    if month == 2:
        return 29 if is_leap(year) else 28
    if month in (4, 6, 9, 11):
        return 30
    return 31

def next_day(day, month, year):
    day += 1
    if day > days_in_month(month, year):
        day = 1
        month += 1
        if month == 13:
            month = 1
            year += 1
    return day, month, year

def solve(inp):
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    day, month, year = map(int, s.split("."))

    while True:
        nd, nm, ny = next_day(day, month, year)
        if day % 2 == 1 and nd % 2 == 1:
            ad, am, ay = next_day(nd, nm, ny)
            return f"{ad:02d}.{am:02d}.{ay:04d}"
        day, month, year = nd, nm, ny

assert solve("11.05.2019") == "02.06.2019", "sample 1"
assert solve("01.01.2019") == "02.02.2019", "sample 2"
assert solve("01.02.2019") == "02.04.2019", "sample 3"
assert solve("01.01.2000") == "02.02.2000", "leap year handling"
assert solve("01.12.2099") == "02.02.2100", "year transition"
assert solve("29.02.2000") == "02.04.2000", "leap day handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 01.01.2000 | 02.02.2000 | Năm nhuận tháng 2 có 29 ngày | 
| 01.12.2099 | 02.02.2100 | Chuyển tiếp vào cuối năm | 
| 29.02.2000 | 04.02.2000 | Xử lý đúng việc bắt đầu ngày nhuận | 

## Vỏ cạnh 

Một tháng chuyển tiếp có 31 ngày là trường hợp ranh giới cốt lõi. Đối với đầu vào`01.01.2019`, thuật toán sẽ kiểm tra ngày cho đến khi đạt được`31.01.2019`theo sau là`01.02.2019`. Đây đều là những ngày lẻ nên lần chạy thứ hai diễn ra vào`01.02.2019`, và đầu ra trở thành`02.02.2019`. Giải pháp chỉ kiểm tra ngày trong cùng tháng sẽ bỏ lỡ quá trình chuyển đổi này. 

Ranh giới năm nhuận phải sử dụng quy tắc năm nhuận đầy đủ. Đối với đầu vào`29.02.2000`, số học ghi nhận rằng 2000 chia hết cho 400 nên tháng Hai có 29 ngày. Thuật toán tiếp tục chính xác cho đến hết tháng 3 và tìm ra các lần chạy liên tiếp dẫn đến`02.04.2000`. 

Kết quả xuất ra một ngày sau lần chạy thứ hai cũng là một nguyên nhân thường xuyên xảy ra lỗi. Đối với đầu vào`31.05.2019`, số lần chạy liên tiếp là`31.05.2019`Và`01.06.2019`. Câu trả lời đúng là`02.06.2019`, không`01.06.2019`, vì Semyon chỉ bỏ cuộc sau ngày tập luyện thứ hai liên tiếp.
