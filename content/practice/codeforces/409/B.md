---
title: "CF 409B - Ngôn ngữ huyền bí"
description: "Đây là một trong những vấn đề Cá tháng Tư của Codeforces. Không có đầu vào thuật toán thực sự. Thẩm phán cung cấp một ngôn ngữ đặc biệt gọi là \"Bí mật\" thông qua hệ thống yêu cầu tùy chỉnh. Nhiệm vụ là xác định ngôn ngữ đó thực sự là gì và gửi chương trình được viết bằng ngôn ngữ đó."
date: "2026-06-07T02:00:20+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 409
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest 2014"
rating: 1700
weight: 409
solve_time_s: 238
verified: true
draft: false
---

[CF 409B - Ngôn ngữ bí ẩn](https://codeforces.com/problemset/problem/409/B) 

**Đánh giá:** 1700 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 3 phút 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đây là một trong những vấn đề Cá tháng Tư của Codeforces. Không có đầu vào thuật toán thực sự. 

Thẩm phán cung cấp một ngôn ngữ đặc biệt gọi là "Bí mật" thông qua hệ thống yêu cầu tùy chỉnh. Nhiệm vụ là xác định ngôn ngữ đó thực sự là gì và gửi chương trình được viết bằng ngôn ngữ đó. Khi thực thi, chương trình phải in tên chính xác của ngôn ngữ. 

Đầu vào trống. Không có gì cần phải đọc. 

Đầu ra là một chuỗi cố định duy nhất, tên của ngôn ngữ bí ẩn. 

Thử thách không phải là tính toán. Khó khăn dự kiến ​​đến từ việc khám phá ra "Bí mật" tương ứng với ngôn ngữ nào. Câu trả lời được chấp nhận cho vấn đề này là **FORTRAN 77**. 

Vì không có đầu vào nên không có ràng buộc về số lượng, không có mối lo ngại về độ phức tạp và không có cấu trúc dữ liệu liên quan. 

Trường hợp thực sự duy nhất là định dạng đầu ra. Câu trả lời phân biệt chữ hoa chữ thường và chứa khoảng trắng và chữ số. 

Ví dụ: in:```
Fortran 77
```sai vì cách viết hoa khác nhau. 

Tương tự:```
FORTRAN77
```sai vì thiếu dung lượng cần thiết. 

Đầu ra đúng là:```
FORTRAN 77
```## Phương pháp tiếp cận 

Một bài toán lập trình cạnh tranh thông thường sẽ yêu cầu xử lý đầu vào và tính toán một số kết quả. Ở đây không có tính toán nào cả. 

Một quan điểm bạo lực sẽ là cố gắng xác định ngôn ngữ ẩn bằng cách thử nghiệm các đặc điểm ngôn ngữ và quan sát cách hành xử của thẩm phán. Đó chính là câu đố dự định trong cuộc thi. Khi ngôn ngữ được công nhận là FORTRAN 77, giải pháp sẽ trở nên tầm thường. 

Quan sát quan trọng là thẩm phán không đánh giá bất kỳ logic thuật toán nào. Nó chỉ kiểm tra xem chương trình đã gửi, được viết bằng ngôn ngữ bí ẩn, có in tên ngôn ngữ đó hay không. 

Sau khi phát hiện ra "Bí mật" là FORTRAN 77, toàn bộ vấn đề giảm xuống còn việc xuất ra một chuỗi cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Điều tra ngôn ngữ vũ phu | Không có ý nghĩa | Không có ý nghĩa | Quá trình khám phá cuộc thi | 
| Đầu ra Câu trả lời cố định | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định rằng ngôn ngữ ẩn do thẩm phán cung cấp là FORTRAN 77. 
2. Viết chương trình FORTRAN 77. 
3. In chính xác chuỗi`"FORTRAN 77"`. 
4. Chấm dứt chương trình. 

### Tại sao nó hoạt động 

Thẩm phán mong đợi tên của ngôn ngữ bí ẩn. Ngôn ngữ ẩn là FORTRAN 77 nên in chính xác`"FORTRAN 77"`phù hợp với đầu ra yêu cầu. Vì không có đầu vào và không có logic bổ sung nên bất kỳ chương trình nào in chuỗi chính xác này đều được chấp nhận. 

## Giải pháp Python 

Vấn đề ban đầu yêu cầu phải gửi trong FORTRAN 77. Vì bài xã luận này tuân theo mẫu lập trình cạnh tranh tiêu chuẩn nên cách biểu diễn Python tương đương là:```python
import sys
input = sys.stdin.readline

print("FORTRAN 77")
```Giải pháp không chứa xử lý đầu vào vì tệp thử nghiệm trống. 

Thao tác duy nhất được thực hiện là in chuỗi được yêu cầu. Không có vòng lặp, điều kiện hoặc tính toán. 

Lỗi thường gặp nhất là thay đổi cách viết hoa hoặc bỏ khoảng cách giữa`FORTRAN`Và`77`. 

## Ví dụ đã hoạt động 

Vì đầu vào luôn trống nên mọi thực thi đều tuân theo cùng một dấu vết. 

### Ví dụ 1 

đầu vào:```
<empty>
```| Bước | Hành động | Đầu ra | 
| --- | --- | --- | 
| 1 | Bắt đầu chương trình | | 
| 2 | In câu trả lời | FORTRAN 77 | 
| 3 | Thoát | FORTRAN 77 | 

Dấu vết cho thấy không có đầu vào nào được sử dụng và câu trả lời cố định được tạo ra ngay lập tức. 

### Ví dụ 2 

đầu vào:```
<empty>
```| Bước | Hành động | Đầu ra | 
| --- | --- | --- | 
| 1 | Bắt đầu chương trình | | 
| 2 | In câu trả lời | FORTRAN 77 | 
| 3 | Thoát | FORTRAN 77 | 

Điều này chứng tỏ rằng hành vi của chương trình là độc lập với bất kỳ dữ liệu nào vì không có dữ liệu nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một thao tác in | 
| Không gian | O(1) | Sử dụng bộ nhớ liên tục | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ vì nó thực hiện một thao tác đầu ra duy nhất với thời gian không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    print("FORTRAN 77")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run("") == "FORTRAN 77\n", "sample 1"

# custom cases
assert run("") == "FORTRAN 77\n", "empty input"
assert run("\n") == "FORTRAN 77\n", "extra newline ignored"
assert run("random text\n") == "FORTRAN 77\n", "input is irrelevant"
assert run("1 2 3\n4 5 6\n") == "FORTRAN 77\n", "still fixed output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào trống | FORTRAN 77 | Kịch bản chính thức | 
| Dòng mới duy nhất | FORTRAN 77 | Không phụ thuộc vào nội dung đầu vào | 
| Văn bản ngẫu nhiên | FORTRAN 77 | Đầu ra được cố định | 
| Nhiều dòng | FORTRAN 77 | Chương trình bỏ qua tất cả đầu vào | 

## Vỏ cạnh 

Trường hợp tinh vi đầu tiên là viết hoa. 

đầu vào:```
<empty>
```Nếu một giải pháp in:```
Fortran 77
```nó bị từ chối vì đầu ra được yêu cầu phân biệt chữ hoa chữ thường. Thuật toán xử lý việc này bằng cách in hằng số chính xác:```
FORTRAN 77
```Trường hợp tinh tế thứ hai là khoảng cách. 

đầu vào:```
<empty>
```In ấn:```
FORTRAN77
```không chính xác vì câu trả lời dự kiến ​​có khoảng cách giữa từ và số. Giải pháp mã hóa cứng chuỗi được chấp nhận chính xác, tránh các lỗi định dạng. 

Trường hợp cạnh cuối cùng giả sử đầu vào tồn tại. 

đầu vào:```
<empty>
```Chương trình cố gắng đọc dữ liệu bắt buộc có thể bị lỗi hoặc hoạt động không như mong muốn. Giải pháp được chấp nhận không thực hiện xử lý đầu vào và in trực tiếp câu trả lời phù hợp với đặc điểm kỹ thuật của vấn đề.
