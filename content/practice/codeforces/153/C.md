---
title: "CF 153C - Mật mã Caesar"
description: "Chúng ta được yêu cầu triển khai mật mã Caesar trên một chuỗi đầu vào gồm các chữ cái Latinh viết hoa. Về mặt khái niệm, điều này có nghĩa là mỗi chữ cái được dịch chuyển về phía trước trong bảng chữ cái theo một số vị trí cố định, ký hiệu là k. Nếu ca đi qua 'Z', nó sẽ chuyển sang 'A'."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 153
codeforces_index: "C"
codeforces_contest_name: "Surprise Language Round 5"
rating: 2200
weight: 153
solve_time_s: 62
verified: true
draft: false
---

[CF 153C - Mật mã Caesar](https://codeforces.com/problemset/problem/153/C) 

**Đánh giá:** 2200 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu triển khai mật mã Caesar trên một chuỗi đầu vào gồm các chữ cái Latinh viết hoa. Về mặt khái niệm, điều này có nghĩa là mỗi chữ cái được dịch chuyển về phía trước trong bảng chữ cái theo một số vị trí cố định, ký hiệu là _k_. Nếu ca đi qua 'Z', nó sẽ chuyển sang 'A'. Ví dụ: với độ dịch chuyển là 3, 'X' trở thành 'A', 'Y' trở thành 'B' và 'Z' trở thành 'C'. 

Đầu vào bao gồm một chuỗi có độ dài từ 1 đến 10 ký tự, vì vậy chúng tôi được đảm bảo một tập dữ liệu rất nhỏ. Dòng thứ hai là số nguyên _k_ trong khoảng từ 0 đến 25, có nghĩa là phép dịch sẽ không bao giờ cần nhiều chu kỳ của bảng chữ cái. Đầu ra phải là chuỗi được chuyển đổi sau khi áp dụng mật mã Caesar cho mọi ký tự. 

Một trường hợp khó phát hiện xảy ra khi chữ cái ở gần cuối bảng chữ cái và _k_ đẩy nó qua 'Z'. Ví dụ, với đầu vào`"XYZ"`Và`k = 5`, sản lượng dự kiến ​​​​là`"CDE"`. Một cách tiếp cận đơn giản chỉ cần thêm _k_ vào giá trị ASCII của mỗi ký tự mà không gói sẽ cho các chữ cái không hợp lệ ngoài 'Z'. Một trường hợp cạnh khác là`k = 0`, trong đó chuỗi sẽ không thay đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại từng ký tự, chuyển đổi nó thành một số từ 0 đến 25 tương ứng với vị trí của nó trong bảng chữ cái, thêm _k_, rồi gói gọn bằng modulo 26. Cách này hiệu quả vì bảng chữ cái có kích thước cố định và chúng ta có thể ánh xạ các chữ cái thành số nguyên một cách dễ dàng bằng cách sử dụng số học ASCII. Đối với một chuỗi có độ dài _n_, cách tiếp cận này thực hiện chính xác các phép toán _n_, trong trường hợp của chúng tôi nhiều nhất là 10. Không có vấn đề về hiệu suất; "lực lượng vũ phu" về cơ bản là tối ưu với các ràng buộc. 

Quan sát giúp đơn giản hóa mọi thứ là hoạt động modulo vốn đã xử lý việc bao bọc. Cụ thể, nếu chúng ta ánh xạ 'A' thành 0, 'B' thành 1, ..., 'Z' thành 25, thì`(index + k) % 26`tạo ra chỉ số dịch chuyển chính xác. Hoạt động modulo này là thông tin chi tiết quan trọng duy nhất cần thiết. Một khi bạn nghĩ về các chỉ số bằng số thay vì các chữ cái, vấn đề sẽ giảm xuống thành một phép biến đổi mảng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và phép dịch số nguyên _k_. Chuỗi sẽ được coi là một chuỗi ký tự và _k_ được đảm bảo nằm trong khoảng từ 0 đến 25. 
2. Khởi tạo một danh sách trống để giữ các ký tự kết quả. Việc sử dụng danh sách thay vì xây dựng chuỗi dần dần sẽ tránh được độ phức tạp về thời gian bậc hai trong các ngôn ngữ trong đó chuỗi là bất biến. 
3. Lặp lại từng ký tự trong chuỗi đầu vào. Chuyển đổi ký tự thành chỉ mục từ 0 đến 25 bằng cách trừ giá trị ASCII của 'A'. Điều này đưa ra một biểu diễn số phù hợp với số học. 
4. Tính chỉ số mới sau khi dịch chuyển bằng cách thêm _k_ và lấy modulo 26. Modulo đảm bảo rằng dịch chuyển kết thúc chính xác từ 'Z' trở lại 'A'. 
5. Chuyển đổi chỉ mục mới trở lại thành ký tự bằng cách thêm giá trị ASCII của 'A' và thêm nó vào danh sách kết quả. 
6. Nối danh sách các ký tự thành một chuỗi duy nhất và in nó. 

Tại sao nó hoạt động: Thuật toán duy trì ánh xạ nhất quán từ các ký tự đến chỉ số số, áp dụng phép dịch chuyển trong hệ thống số đóng modulo 26 và ánh xạ trở lại các ký tự. Điều bất biến là chỉ số của mọi ký tự luôn nằm trong khoảng từ 0 đến 25, do đó phép toán modulo đảm bảo tính chính xác cho cả những dịch chuyển nhỏ và dịch chuyển biên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
k = int(input())

result = []

for char in s:
    index = ord(char) - ord('A')
    new_index = (index + k) % 26
    result.append(chr(new_index + ord('A')))

print(''.join(result))
```Đầu tiên, mã sẽ đọc chuỗi và phím shift, loại bỏ mọi ký tự dòng mới bổ sung. Sau đó, chúng tôi lặp lại từng ký tự, chuyển đổi nó thành chỉ mục dựa trên số 0. Việc thêm _k_ và lấy modulo 26 đảm bảo việc bao bọc được xử lý chính xác. Cuối cùng, chúng tôi chuyển đổi từng chỉ mục đã dịch chuyển trở lại thành một ký tự và tập hợp chuỗi đầu ra. sử dụng`ord`Và`chr`đối với ánh xạ chỉ mục ký tự là tiêu chuẩn và thứ tự các thao tác được sắp xếp cẩn thận để tránh các lỗi sai sót một. 

## Ví dụ đã hoạt động 

đầu vào`"CODEFORCES"`với`k = 5`: 

| char | chỉ mục | new_index | ký tự kết quả | 
| --- | --- | --- | --- | 
| C | 2 | 7 | H | 
| Ồ | 14 | 19 | T | 
| D | 3 | 8 | Tôi | 
| E | 4 | 9 | J | 
| F | 5 | 10 | K | 
| Ồ | 14 | 19 | T | 
| R | 17 | 22 | W | 
| C | 2 | 7 | H | 
| E | 4 | 9 | J | 
| S | 18 | 23 | X | 

Điều này chứng tỏ rằng mọi ký tự được dịch chuyển độc lập và thao tác modulo xử lý việc gói một cách tự động. 

đầu vào`"XYZ"`với`k = 5`: 

| char | chỉ mục | new_index | ký tự kết quả | 
| --- | --- | --- | --- | 
| X | 23 | 2 | C | 
| Y | 24 | 3 | D | 
| Z | 25 | 4 | E | 

Điều này cho thấy hành vi bao quanh ở cuối bảng chữ cái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong một chuỗi phép tính số học cố định. | 
| Không gian | O(n) | Một danh sách có độ dài n được sử dụng để lưu trữ các ký tự kết quả. | 

Với n 10, quá trình này chạy trong thời gian không đáng kể và sử dụng bộ nhớ tầm thường, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    k = int(sys.stdin.readline())
    result = [chr((ord(c) - ord('A') + k) % 26 + ord('A')) for c in s]
    return ''.join(result)

# Provided samples
assert run("CODEFORCES\n5\n") == "HTIJKTWHJX", "sample 1"

# Minimum-size input
assert run("A\n0\n") == "A", "min size, no shift"

# Maximum-size input
assert run("ABCDEFGHIJ\n25\n") == "ZABCDEFGHI", "max size, wrap-around"

# All-equal letters
assert run("ZZZZZ\n1\n") == "AAAAA", "all Zs wrap to A"

# Boundary shift
assert run("XYZ\n5\n") == "CDE", "wrap-around near end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A\n0 | A | chiều dài tối thiểu, độ dịch chuyển bằng 0 | 
| ABCDEFGHIJ\n25 | ZABCDEFGHI | chiều dài tối đa, dịch chuyển gần đầy | 
| ZZZZZ\n1 | AAAAA | tất cả các chữ cái bọc từ Z đến A | 
| XYZ\n5 | CDE | ranh giới bao quanh | 

## Vỏ cạnh 

Đối với đầu vào`"XYZ"`với`k = 5`, các chỉ số bằng số là 23, 24, 25. Cộng 5 sẽ có 28, 29, 30, modulo 26 sẽ cho ra 2, 3, 4. Việc ánh xạ các giá trị này trở lại các ký tự sẽ tạo ra`"CDE"`. Thuật toán xử lý chính xác các chữ cái ở gần cuối bảng chữ cái mà không cần phân nhánh hoặc điều kiện đặc biệt, xác nhận rằng hoạt động modulo quản lý hoàn toàn việc bao bọc cạnh.
