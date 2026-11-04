---
title: "CF 1177B - Chuỗi chữ số (Phiên bản cứng)"
description: "Chúng ta được yêu cầu xem xét chuỗi vô hạn được hình thành bằng cách viết tất cả các số nguyên dương liên tiếp mà không có bất kỳ dấu phân cách nào: \"1234567891011121314…\". Với một vị trí $k$, chúng ta phải xác định chữ số nào chiếm vị trí đó."
date: "2026-06-12T01:43:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1177
codeforces_index: "B"
codeforces_contest_name: "Testing Round 15 (Unrated)"
rating: 1800
weight: 1177
solve_time_s: 92
verified: true
draft: false
---

[CF 1177B - Chuỗi chữ số (Phiên bản cứng)](https://codeforces.com/problemset/problem/1177/B) 

**Đánh giá:** 1800 
**Tas:** tìm kiếm nhị phân, phân chia và chinh phục, thực hiện 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét chuỗi vô hạn được hình thành bằng cách viết tất cả các số nguyên dương liên tiếp mà không có bất kỳ dấu phân cách nào: "1234567891011121314...". Được trao một vị trí$k$, ta phải xác định chữ số nào chiếm vị trí đó. Đầu vào là một số nguyên duy nhất$k$lên đến$10^{12}$, và đầu ra là chữ số tương ứng. 

Quan sát đầu tiên là chuỗi được cấu trúc ngầm bởi độ dài của các số. Số có một chữ số chiếm vị trí từ 1 đến 9. Số có hai chữ số chiếm vị trí từ 10 đến 189, số có ba chữ số chiếm vị trí từ 190 đến 2889, v.v. Việc phân lớp theo chiều dài chữ số này là cần thiết để đạt được hiệu quả. Một cách tiếp cận ngây thơ xây dựng chuỗi lên tới$k$sẽ yêu cầu lưu trữ lên đến$10^{12}$các chữ số không khả thi trong bộ nhớ và quá chậm để tạo ra trong một thời gian hợp lý. 

Một trường hợp cạnh quan trọng xảy ra gần ranh giới giữa độ dài chữ số. Ví dụ, nếu$k = 10$, chữ số đó thuộc về "10", số có hai chữ số đầu tiên. Việc triển khai đơn giản đếm các chữ số mà không xem xét các nhóm độ dài có thể đếm sai và chọn sai chữ số. 

Một trường hợp tế nhị khác xảy ra khi$k$ví dụ: tiếp đất chính xác ở chữ số cuối cùng của nhóm độ dài$k = 9$hoặc$k = 189$. Các vị trí này tương ứng với chữ số cuối cùng của số có 1 chữ số hoặc 2 chữ số. Xử lý các ranh giới một cách cẩn thận sẽ ngăn chặn từng lỗi một. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: bắt đầu từ 1 và nối mỗi số dưới dạng một chuỗi cho đến khi đạt tổng độ dài$k$. Sau đó trả lại$k$-nhân vật thứ. Điều này đúng vì chúng tôi mô phỏng trình tự theo đúng nghĩa đen. Vấn đề phát sinh trong hiệu suất. Vì$k$gần$10^{12}$, việc tạo từng chữ số một sẽ yêu cầu ít nhất$10^{12}$hoạt động, vượt xa giới hạn thực tế trong giới hạn thời gian 1 giây. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là chuỗi được cấu trúc thành các khối số có độ dài chữ số bằng nhau. Số với$d$các chữ số chiếm một khối độ dài liền kề$d \times 9 \times 10^{d-1}$. Bằng cách tính toán tổng hợp độ dài của các khối này, chúng ta có thể xác định khối nào chứa$k$-chữ số thứ mà không tạo ra toàn bộ chuỗi. Khi khối được xác định, phép chia số nguyên và số học modulo xác định số và chữ số chính xác trong số đó. Điều này làm giảm vấn đề từ tuyến tính trong$k$logarit về số độ dài chữ số, có thể chấp nhận được đối với$k$lên đến$10^{12}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(k) | Quá chậm | 
| Tối ưu | O(logk) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm độ dài chữ số`d = 1`và số bắt đầu cho độ dài này`start = 1`. Khởi tạo bộ đếm chiều dài tích lũy`count = 0`để theo dõi tổng số chữ số được xử lý cho đến nay. 
2. Tính độ dài của khối hiện tại`d`-các số có chữ số như`block_len = 9 * 10^(d-1) * d`. Nếu thêm`block_len`ĐẾN`count`giữ`count < k`, tăng`d`và cập nhật`count += block_len`. Lặp lại cho đến khi khối chứa`k`được tìm thấy. 
3. Khi độ dài chữ số chính xác`d`được tìm thấy, hãy tính phần bù của`k`trong khối:`offset = k - count - 1`. các`-1`chuyển đổi vị trí thành lập chỉ mục dựa trên 0 trong khối. 
4. Xác định số nào trong khối chứa chữ số mong muốn bằng phép chia số nguyên:`number_index = offset // d`. Con số thực tế là`number = 10^(d-1) + number_index`. 
5. Xác định chữ số nào trong số này là mục tiêu bằng modulo:`digit_index = offset % d`. Trích xuất chữ số bằng cách chuyển đổi số thành chuỗi và lập chỉ mục:`str(number)[digit_index]`. 
6. Xuất ra chữ số làm đáp án. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi duy trì tính bất biến`count`theo dõi tổng số chữ số được bao phủ bởi tất cả các khối nhỏ hơn. Bằng cách xác định chiều dài khối`d`có chứa`k`và sử dụng phép chia số nguyên và modulo, chúng tôi lập bản đồ chính xác vị trí toàn cầu`k`đến một số và chữ số trong số đó. Không có sự mơ hồ vì các số trong mỗi khối đều liên tiếp nhau và có kích thước đồng đều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())

d = 1
count = 0

while True:
    block_len = 9 * (10 ** (d - 1)) * d
    if count + block_len >= k:
        break
    count += block_len
    d += 1

offset = k - count - 1
number_index = offset // d
digit_index = offset % d
number = 10 ** (d - 1) + number_index
print(str(number)[digit_index])
```Mã tuân theo thuật toán chính xác. các`while`vòng lặp tìm kiếm khối có độ dài chữ số chính xác bằng cách tích lũy`count`. Lập chỉ mục dựa trên số không`offset = k - count - 1`đảm bảo rằng phép chia số nguyên và modulo xác định chính xác số và chữ số. Việc chuyển đổi sang chuỗi là an toàn vì các số có tối đa 12 chữ số (vì`k ≤ 10^12`) và vừa vặn thoải mái với số nguyên Python. 

## Ví dụ đã hoạt động 

Mẫu 1:`k = 7`| Biến | Giá trị | 
| --- | --- | 
| d | 1 | 
| đếm | 0 | 
| khối_len | 9 | 
| bù đắp | 6 | 
| số_index | 6 | 
| số | 7 | 
| chữ số_index | 0 | 
| đầu ra | 7 | 

Dấu vết xác nhận rằng`k=7`nằm trong các số có 1 chữ số, ánh xạ chính xác đến số 7, chữ số 0. 

Mẫu 2:`k = 12`| Biến | Giá trị | 
| --- | --- | 
| d | 2 | 
| đếm | 9 | 
| khối_len | 180 | 
| bù đắp | 2 | 
| số_index | 1 | 
| số | 10 + 1 = 11 | 
| chữ số_index | 0 | 
| đầu ra | 1 | 

Dấu vết này thể hiện việc xử lý việc chuyển đổi từ số có 1 chữ số sang số có 2 chữ số một cách chính xác. Phép tính offset ánh xạ vị trí chung tới số và chữ số thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logk) | Mỗi lần lặp nhân số bắt đầu với 10, do đó số độ dài chữ số được xem xét nhiều nhất là 12 đối với`k ≤ 10^12`. Tất cả các tính toán tiếp theo là thời gian không đổi. | 
| Không gian | O(1) | Chỉ các biến số nguyên được lưu trữ, không có mảng hoặc chuỗi lớn. | 

Với những hạn chế, điều này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(input())
    d = 1
    count = 0
    while True:
        block_len = 9 * (10 ** (d - 1)) * d
        if count + block_len >= k:
            break
        count += block_len
        d += 1
    offset = k - count - 1
    number_index = offset // d
    digit_index = offset % d
    number = 10 ** (d - 1) + number_index
    return str(number)[digit_index]

# provided samples
assert run("7") == "7", "sample 1"

# custom cases
assert run("1") == "1", "first digit"
assert run("10") == "1", "transition from 1-digit to 2-digit"
assert run("190") == "1", "
```
