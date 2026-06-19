---
title: "CF 1A - Quảng trường Nhà hát"
description: "Chúng ta có một quảng trường hình chữ nhật, Quảng trường Nhà hát, có kích thước dài _n_ mét và rộng _m_ mét. Thành phố muốn lát toàn bộ khu vực bằng những phiến đá hình vuông cạnh _a_."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 1"
rating: 1000
weight: 1
solve_time_s: 241
verified: true
draft: false
---
## Hiểu vấn đề 

Chúng ta có một quảng trường hình chữ nhật, Quảng trường Nhà hát, có kích thước dài _n_ mét và rộng _m_ mét. Thành phố muốn lát toàn bộ khu vực bằng những phiến đá hình vuông cạnh _a_. Câu hỏi yêu cầu số lượng đá lát tối thiểu cần thiết để che phủ hoàn toàn hình vuông. Mỗi phiến đá phải được đặt thẳng hàng với các cạnh của hình vuông; chúng ta không thể cắt chúng, nhưng có thể chấp nhận được những viên đá cờ vượt ra ngoài các cạnh của hình vuông. 

Số đầu vào có thể rất lớn, lên tới 10^9. Điều này cho chúng ta biết rằng bất kỳ cách tiếp cận nào lặp đi lặp lại trên từng mét vuông hoặc từng vị trí xếp gạch có thể đều không khả thi. Chúng ta cần một giải pháp dựa trên số học, theo thời gian không đổi. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó kích thước của hình vuông không chia hết cho kích thước ô. Ví dụ: nếu hình vuông có kích thước 6×6 mét và cột cờ có kích thước 4×4 mét, phép chia số nguyên đơn giản có thể đề xuất 1 cột cờ cho mỗi chiều (6 // 4 = 1), cho tổng số là 1. Điều này rõ ràng là sai vì một cột cờ 4×4 không thể bao phủ 6 mét. Cách tiếp cận đúng phải làm tròn phép chia để tính phần còn lại. 

Một trường hợp tinh tế khác là khi _n_ hoặc _m_ chia hết chính xác cho _a_, điều này sẽ không thêm một cột cờ bổ sung. Ví dụ: một hình vuông 12×8 với các ô 4×4 phải sử dụng chính xác 6 ô (3 ô dọc theo chiều dài, 2 ô dọc theo chiều rộng), không được nhiều hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ cố gắng mô phỏng việc đặt từng ô, đếm xem có bao nhiêu ô phù hợp dọc theo chiều dài và chiều rộng, đồng thời thêm các ô bổ sung cho khoảng trống còn sót lại. Về mặt khái niệm, chúng ta có thể tưởng tượng một vòng lặp lồng nhau lặp đi lặp lại trên góc trên bên trái của mỗi ô. Mặc dù đúng nhưng số thao tác trong trường hợp xấu nhất có thể lên tới 10^18 đối với đầu vào tối đa (n = m = 10^9, a = 1), điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải mô phỏng bất cứ điều gì. Mỗi chiều có thể được xử lý độc lập. Cùng một chiều, số lượng gạch cần thiết là trần của việc chia chiều dài hình vuông cho kích thước gạch. Về mặt toán học, đây là`ceil(n / a)`. Thực hiện điều này cho cả hai chiều và nhân kết quả sẽ ra tổng số ô. Việc làm tròn đảm bảo rằng ngay cả khi có một phần nhỏ còn lại, chúng tôi sẽ phân bổ thêm một ô để che nó. 

Hoạt động trần có thể được tính toán mà không cần số học dấu phẩy động bằng cách sử dụng phép chia số nguyên:`(n + a - 1) // a`. Điều này hoạt động bởi vì nếu`n`không chia hết cho`a`, thêm`a-1`đảm bảo thương làm tròn lên. Nếu như`n`chia hết cho`a`, phép cộng không làm thay đổi thương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m / a^2) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`,`m`, Và`a`. Chúng đại diện cho chiều dài, chiều rộng và kích thước ô. 
2. Tính số ô dọc theo chiều dài bằng phép tính số nguyên:`tiles_along_length = (n + a - 1) // a`. Thêm`a - 1`đảm bảo làm tròn nếu`n`không chia hết cho`a`. 
3. Tính số ô dọc theo chiều rộng theo cách tương tự:`tiles_along_width = (m + a - 1) // a`. 
4. Nhân hai kết quả để được tổng số ô:`total_tiles = tiles_along_length * tiles_along_width`. 
5. In kết quả. 

Tại sao nó hoạt động: Mỗi kích thước được bao phủ độc lập bằng cách sử dụng số lượng ô tối thiểu để bao phủ toàn bộ chiều dài. Phép nhân đảm bảo phạm vi bao phủ hai chiều của khu vực hình chữ nhật. Bước làm tròn đảm bảo không có khoảng trống nào bị che khuất ngay cả khi`n`hoặc`m`không phải là bội số của`a`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, a = map(int, input().split())

tiles_along_length = (n + a - 1) // a
tiles_along_width = (m + a - 1) // a

total_tiles = tiles_along_length * tiles_along_width
print(total_tiles)
```Mã trực tiếp theo thuật toán. Đọc đầu vào với`sys.stdin.readline`tránh chi phí I/O cho đầu vào lớn. các`(n + a - 1) // a`thủ thuật tránh phân chia dấu phẩy động, ngăn chặn các vấn đề về độ chính xác và tràn. Nhân hai số đếm sẽ có tổng số đá cờ chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6 6 4
```| n | m | một | gạch_along_length | gạch_along_width | tổng_tiles | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 6 | 4 | 2 | 2 | 4 | 

Điều này cho thấy rằng mặc dù 6/4 là 1,5 nhưng làm tròn lên 2 vẫn đảm bảo phạm vi bao phủ đầy đủ. 

Đầu vào mẫu 2 (được xây dựng):```
12 8 4
```| n | m | một | gạch_along_length | gạch_along_width | tổng_tiles | 
| --- | --- | --- | --- | --- | --- | 
| 12 | 8 | 4 | 3 | 2 | 6 | 

Điều này chứng tỏ trường hợp cả hai chiều đều chia hết cho`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học được thực hiện; không có vòng lặp. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Với các giới hạn lên tới 10^9, giải pháp dễ dàng chạy trong vòng 1 giây và 256 MB bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, a = map(int, sys.stdin.readline().split())
    return str(((n + a - 1) // a) * ((m + a - 1) // a))

# Provided sample
assert run("6 6 4\n") == "4", "sample 1"

# Custom cases
assert run("12 8 4\n") == "6", "exact division"
assert run("1 1 1\n") == "1", "minimum-size input"
assert run("1000000000 1000000000 1\n") == "1000000000000000000", "maximum-size input"
assert run("5 5 3\n") == "4", "odd dimensions with remainder"
assert run("10 10 10\n") == "1", "tile exactly fits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 12 8 4 | 6 | Phép chia chính xác, không cần làm tròn | 
| 1 1 1 | 1 | Đầu vào kích thước tối thiểu | 
| 1000000000 1000000000 1 | 10000000000000000000 | Giá trị đầu vào tối đa, an toàn tràn số nguyên | 
| 5 5 3 | 4 | Phần còn lại làm tròn | 
| 10 10 10 | 1 | Ngói bao phủ chính xác kích thước | 

## Vỏ cạnh 

Đối với trường hợp`n = 5, m = 5, a = 3`, phép tính là`(5 + 3 - 1) // 3 = 7 // 3 = 2`gạch cho mỗi kích thước. Nhân cho 4 ô. Mỗi chiều được bao phủ hoàn toàn mặc dù phần còn lại là 2 mét. Một phép chia số nguyên đơn giản mà không làm tròn số sẽ trả về ô 1×1 = 1, điều này rõ ràng là không đủ. Điều này xác nhận logic làm tròn xử lý chính xác các kích thước không chia hết. 

Đối với giá trị tối đa`n = m = 10^9, a = 1`, phép tính`(10^9 + 1 - 1) // 1 = 10^9`theo từng chiều, đưa ra`10^9 × 10^9 = 10^18`gạch lát. Python xử lý số nguyên lớn này mà không bị tràn, xác thực phương pháp tiếp cận cho các đầu vào cực trị.
