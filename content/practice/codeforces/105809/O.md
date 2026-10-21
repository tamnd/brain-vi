---
title: "CF 105809O - Kỹ thuật che giấu"
description: "Đây chỉ là vấn đề đầu ra. Không có đầu vào nào cả. Câu lệnh đưa ra một chuỗi các giá trị byte thập lục phân. Việc giải thích từng cặp chữ số thập lục phân dưới dạng ký tự ASCII sẽ tiết lộ một thông báo ẩn."
date: "2026-06-25T15:30:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "O"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 38
verified: true
draft: false
---

[CF 105809O - Kỹ thuật che giấu](https://codeforces.com/problemset/problem/105809/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đây chỉ là vấn đề đầu ra. Không có đầu vào nào cả. 

Câu lệnh đưa ra một chuỗi các giá trị byte thập lục phân. Việc giải thích từng cặp chữ số thập lục phân dưới dạng ký tự ASCII sẽ tiết lộ một thông báo ẩn. Nhiệm vụ chỉ đơn giản là xác định nội dung thông báo đó yêu cầu và in kết quả đầu ra được yêu cầu. 

Vì không có đầu vào và không có tính toán tùy thuộc vào trường hợp thử nghiệm nên độ phức tạp là không liên quan. Toàn bộ thách thức là nhận ra rằng văn bản được cung cấp là mã hóa ASCII thập lục phân và giải mã nó một cách chính xác. 

Một lỗi phổ biến là in chính câu được giải mã thay vì giá trị mà câu yêu cầu. 

Ví dụ, giải mã```
45 4C
```cho```
EL
```nhưng toàn bộ tin nhắn giải mã thành:```
EL CODIGO DE DESACTIVACION ES: "CODE:RUSH:TEC"
```Câu nói mã hủy kích hoạt là`CODE:RUSH:TEC`, vì vậy đó là đầu ra cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là giải mã thủ công từng byte thập lục phân thành ký tự ASCII của nó và xây dựng lại câu. Vì tin nhắn đã được sửa nên nó ngay lập tức đưa ra câu trả lời. Khối lượng công việc là không đổi. 

Không cần tối ưu hóa thuật toán vì đầu vào không bao giờ thay đổi. Khi chuỗi thập lục phân được giải mã, thông báo ẩn sẽ hiển thị rõ ràng đầu ra được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giải mã thập lục phân thủ công | O(1) | O(1) | Đã chấp nhận | 
| In trực tiếp câu trả lời được phát hiện | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng văn bản được cung cấp bao gồm các giá trị byte thập lục phân. 
2. Chuyển đổi từng giá trị thập lục phân thành ký tự ASCII của nó. 
3. Xây dựng lại tin nhắn. 
4. Đọc câu đã được giải mã:`EL CODIGO DE DESACTIVACION ES: "CODE:RUSH:TEC"`. 
5. In mã hủy kích hoạt theo yêu cầu của tin nhắn:`CODE:RUSH:TEC`. 

### Tại sao nó hoạt động 

Chuỗi thập lục phân là mã hóa cố định của câu ASCII. Việc giải mã nó xác định duy nhất thông điệp ẩn và thông báo xác định rõ ràng đầu ra được yêu cầu. Vì không có đầu vào nên việc in giá trị được phát hiện luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

print("CODE:RUSH:TEC")
```Giải pháp không đọc bất kỳ đầu vào nào vì sự cố chỉ rõ rằng không tồn tại. 

Văn bản thập lục phân trong câu lệnh là không đổi. Sau khi giải mã một lần, chúng ta biết đầu ra yêu cầu luôn là một chuỗi giống nhau. In trực tiếp chuỗi đó là giải pháp đơn giản và đáng tin cậy nhất. 

## Ví dụ đã hoạt động 

Vì vấn đề không có đầu vào nên mọi thực thi đều hoạt động giống hệt nhau. 

### Dấu vết thực thi 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Giải mã văn bản thập lục phân |`EL CODIGO DE DESACTIVACION ES: "CODE:RUSH:TEC"`| 
| 2 | Trích xuất mã được yêu cầu |`CODE:RUSH:TEC`| 
| 3 | In câu trả lời |`CODE:RUSH:TEC`| 

Dấu vết này cho thấy câu được giải mã trực tiếp chứa giá trị cần được in. 

### Một cuộc hành quyết khác 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Chạy chương trình | Không cần đầu vào | 
| 2 | Thực hiện lệnh in |`CODE:RUSH:TEC`| 

Vì không có đầu vào nên mỗi lần chạy đều tạo ra đầu ra chính xác như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một chuỗi cố định duy nhất được in | 
| Không gian | O(1) | Không có cấu trúc dữ liệu nào được sử dụng | 

Chương trình thực hiện một lượng công việc không đổi và sử dụng bộ nhớ không đổi, dễ dàng đáp ứng mọi giới hạn hợp lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    print("CODE:RUSH:TEC")

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

assert run("") == "CODE:RUSH:TEC\n", "empty input"
assert run("\n") == "CODE:RUSH:TEC\n", "extra newline"
assert run("anything\n") == "CODE:RUSH:TEC\n", "ignored data"
assert run("123 456\n789\n") == "CODE:RUSH:TEC\n", "still constant output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đầu vào trống |`CODE:RUSH:TEC`| Hành vi chính thức | 
| dòng mới duy nhất |`CODE:RUSH:TEC`| Không cần xử lý đầu vào | 
| văn bản tùy ý |`CODE:RUSH:TEC`| Output is constant |
 | multiple lines |`CODE:RUSH:TEC`| Bản chất chỉ đầu ra của vấn đề | 

## Vỏ cạnh 

Cạm bẫy chính không rõ ràng là in toàn bộ câu được giải mã thay vì mã được yêu cầu. 

Nếu văn bản được giải mã là:```
EL CODIGO DE DESACTIVACION ES: "CODE:RUSH:TEC"
```đầu ra đúng là:```
CODE:RUSH:TEC
```Thuật toán xử lý việc này vì nó diễn giải câu và chỉ in mã được xác định bởi thông báo. 

Một lỗi khác có thể xảy ra là đưa vào dấu ngoặc kép. 

Đầu ra đúng:```
CODE:RUSH:TEC
```Đầu ra không chính xác:```
"CODE:RUSH:TEC"
```Câu trả lời bắt buộc là chính mã đó, không có dấu ngoặc kép xung quanh. Giải pháp được cung cấp in chính xác chuỗi đó.
