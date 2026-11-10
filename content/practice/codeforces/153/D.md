---
title: "CF 153D - Thay đổi ngày"
description: "Chúng ta được yêu cầu thao tác ngày tháng bằng cách cộng hoặc trừ một số ngày. Dữ liệu đầu vào bao gồm một chuỗi ngày ở định dạng \"DD.MM.YYYY\" và một số nguyên biểu thị sự thay đổi về số ngày, có thể là dương hoặc âm."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 153
codeforces_index: "D"
codeforces_contest_name: "Surprise Language Round 5"
rating: 2000
weight: 153
solve_time_s: 76
verified: true
draft: false
---

[CF 153D - Thay đổi ngày](https://codeforces.com/problemset/problem/153/D) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu thao tác ngày tháng bằng cách cộng hoặc trừ một số ngày. Dữ liệu đầu vào bao gồm một chuỗi ngày ở định dạng "DD.MM.YYYY" và một số nguyên biểu thị sự thay đổi về số ngày, có thể là dương hoặc âm. Đầu ra là một chuỗi ngày có cùng định dạng biểu thị ngày ban đầu được dịch chuyển trong nhiều ngày đó. Về mặt khái niệm, bài toán quy về việc tiến hoặc lùi dọc theo lịch trong khi vẫn tôn trọng độ dài của tháng và năm nhuận. 

Các ràng buộc tương đối nhỏ: sự thay đổi được giới hạn ở ±1000 ngày và năm đầu vào luôn nằm trong khoảng từ 1980 đến 2020. Điều này có nghĩa là chúng tôi không cần tối ưu hóa cho các phạm vi cực lớn; hoạt động tỷ lệ thuận với số ngày là khả thi. Tuy nhiên, việc triển khai đơn giản thường thất bại trong các trường hợp ranh giới như chuyển tiếp vào cuối tháng, ngày nhuận trong tháng 2 và ca âm. Ví dụ: bắt đầu từ "28.02.2012" với sự thay đổi là 1 sẽ mang lại "29.02.2012" và sự thay đổi 2 sẽ cho ra "01.03.2012". Một cách tiếp cận bất cẩn mà bỏ qua những năm nhuận sẽ tạo ra "01.03.2012" cho cả hai. 

Những thay đổi tiêu cực cũng đòi hỏi phải xử lý cẩn thận. Nếu đầu vào là "01.03.2012" và độ dịch chuyển là -1 thì đầu ra chính xác là "29.02.2012", chứ không phải "28.02.2012" như một phép tính đơn giản có thể tạo ra. Các trường hợp đặc biệt này cho thấy các quy tắc về độ dài tháng và năm nhuận phải được áp dụng rõ ràng hoặc thông qua thư viện ngày đáng tin cậy. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: chuyển đổi chuỗi ngày thành các số nguyên ngày, tháng và năm riêng biệt, sau đó tăng hoặc giảm từng ngày một, điều chỉnh tháng và năm bất cứ khi nào ngày vượt quá số ngày trong tháng hoặc giảm xuống dưới một. Cách tiếp cận này đúng vì nó mô phỏng rõ ràng lịch, nhưng nó có độ phức tạp về thời gian trong trường hợp xấu nhất là O(|shift|), có thể chấp nhận được ở đây vì |shift| 1000, nhưng sẽ quá chậm nếu số ca chẳng hạn là hàng triệu. 

Cách tiếp cận tối ưu tận dụng tính năng tích hợp sẵn của Python`datetime`mô-đun xử lý nội bộ các năm nhuận, ranh giới tháng và các thay đổi âm. Điều này loại bỏ sự cần thiết phải xử lý số học ngày và trường hợp cạnh theo cách thủ công. Cái nhìn sâu sắc quan trọng là`datetime.date`các đồ vật có thể được kết hợp an toàn với`datetime.timedelta`đối tượng đại diện cho một số ngày. Thêm tích cực hoặc tiêu cực`timedelta`tự động tính toán kết quả ngày, tháng, năm một cách chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | dịch chuyển | ) | 
| Tối ưu | O(1) | O(1) | Được chấp nhận, ngắn gọn, xử lý tất cả các trường hợp khó khăn | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi ngày đầu vào và chia nó thành các số nguyên ngày, tháng và năm bằng cách cắt hoặc tách chuỗi trên ký tự dấu chấm. Điều này chuyển đổi "DD.MM.YYYY" thành ba số nguyên để tính toán. 
2. Đọc giá trị dịch chuyển dưới dạng số nguyên. Giá trị này có thể dương hoặc âm, thể hiện chuyển động tiến hoặc lùi trên lịch. 
3. Tạo một`datetime.date`đối tượng sử dụng năm, tháng và ngày được phân tích cú pháp. Đối tượng này đại diện cho ngày đầu vào chính xác. 
4. Tạo một`datetime.timedelta`đối tượng có số ngày bằng ca làm việc.`timedelta`hỗ trợ các giá trị âm nguyên bản. 
5. Thêm`timedelta`đến`date`sự vật. Python tính toán nội bộ ngày kết quả, xử lý chính xác các lần chuyển đổi tháng, năm nhuận và các ca âm. 
6. Định dạng lại đối tượng ngày kết quả thành một chuỗi theo định dạng "DD.MM.YYYY", đảm bảo hai chữ số cho ngày và tháng và bốn chữ số cho năm. Điều này bảo tồn định dạng đầu ra được yêu cầu. 

Lý do điều này hoạt động là vì Python`datetime`mô-đun đảm bảo rằng số học trên`date`các đối tượng tôn trọng tất cả các quy tắc lịch, kể cả năm nhuận. Không cần phải theo dõi thủ công các ngày trong tháng hoặc điều chỉnh cho ngày 29 tháng 2, điều này giúp loại bỏ các lỗi tiềm ẩn. 

## Giải pháp Python```python
import sys
import datetime
input = sys.stdin.readline

date_str = input().strip()
shift = int(input().strip())

day, month, year = map(int, date_str.split('.'))
current_date = datetime.date(year, month, day)
shifted_date = current_date + datetime.timedelta(days=shift)

print(shifted_date.strftime('%d.%m.%Y'))
```Phần đầu tiên phân tích đầu vào. Chúng tôi sử dụng`strip()`để loại bỏ dấu dòng mới.`map(int, ...)`chuyển đổi các chuỗi phân tách thành số nguyên. các`datetime.date`hàm tạo đảm bảo đầu vào là ngày hợp lệ.`datetime.timedelta`xử lý sự thay đổi và cho phép các giá trị âm, loại bỏ số học dễ bị lỗi thủ công. Cuối cùng,`strftime`đảm bảo định dạng đầu ra khớp chính xác với "DD.MM.YYYY" với các số 0 đứng đầu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
10.02.2012
12
```| Biến | Giá trị | 
| --- | --- | 
| ngày | 10 | 
| tháng | 2 | 
| năm | 2012 | 
| hiện tại_ngày | 10-02-2012 | 
| đã chuyển_ngày | 22-02-2012 | 

Thuật toán cộng đúng 12 ngày, tính ra tháng 2 năm 2012 có 29 ngày. 

Đầu vào mẫu 2 (dịch chuyển âm):```
01.03.2012
-1
```| Biến | Giá trị | 
| --- | --- | 
| ngày | 1 | 
| tháng | 3 | 
| năm | 2012 | 
| hiện tại_ngày | 01-03-2012 | 
| đã chuyển_ngày | 29-02-2012 | 

Điều này thể hiện việc xử lý các ca âm và ngày nhuận một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các hoạt động đều sử dụng tính năng phân tích chuỗi và số học ngày tích hợp của Python, không phụ thuộc vào kích thước dịch chuyển. | 
| Không gian | O(1) | Chỉ có một số đối tượng số nguyên và ngày tháng được tạo, không có cấu trúc dữ liệu bổ sung. | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB cho bất kỳ đầu vào hợp lệ nào. 

## Trường hợp thử nghiệm```python
import sys, io
import datetime

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    date_str = input().strip()
    shift = int(input().strip())
    day, month, year = map(int, date_str.split('.'))
    current_date = datetime.date(year, month, day)
    shifted_date = current_date + datetime.timedelta(days=shift)
    return shifted_date.strftime('%d.%m.%Y')

# Provided samples
assert run("10.02.2012\n12\n") == "22.02.2012", "sample 1"
assert run("01.03.2012\n-1\n") == "29.02.2012", "sample 2"

# Custom cases
assert run("28.02.2011\n1\n") == "01.03.2011", "non-leap year feb"
assert run("28.02.2012\n1\n") == "29.02.2012", "leap year feb"
assert run("31.12.2019\n1\n") == "01.01.2020", "year change forward"
assert run("01.01.1980\n-1\n") == "31.12.1979", "year change backward"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 28.02.2011 1 | 03.01.2011 | Chuyển đổi bình thường từ tháng 2 sang tháng 3 trong năm không nhuận | 
| 28.02.2012 1 | 29.02.2012 | Xử lý ngày nhuận | 
| 31.12.2019 1 | 01.01.2020 | Tăng cuối năm | 
| 01.01.1980 -1 | 31.12.1979 | Sự dịch chuyển âm qua ranh giới năm | 

## Vỏ cạnh 

Bắt đầu từ ngày 29 tháng 2 trong năm nhuận và dịch chuyển một năm trừ đi một ngày là một điều khó nhận thấy. Đối với đầu vào "29.02.2012" với ca 365, Python tính toán đầu ra chính xác là "28.02.2013" vì năm 2013 không phải là năm nhuận. Tương tự, những thay đổi âm vượt qua ranh giới tháng hoặc năm sẽ được xử lý tự động, chẳng hạn như "01.03.2012" với ca -1 tạo ra "29.02.2012". Sử dụng`datetime`mô-đun loại bỏ các lỗi theo dõi thủ công thường xảy ra trong các trường hợp đặc biệt này.
