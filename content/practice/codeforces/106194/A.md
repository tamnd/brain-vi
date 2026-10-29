---
title: "CF 106194A - Bài toán A+B"
description: "Chúng ta có hai số nguyên rất lớn, mỗi số biểu thị một số được ghi trên một thẻ ghi riêng. Nhiệm vụ không phải là tính toàn bộ tổng mà chỉ xác định chữ số cuối cùng của tổng của chúng."
date: "2026-06-19T18:35:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106194
codeforces_index: "A"
codeforces_contest_name: "2025 Winter China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 106194
solve_time_s: 55
verified: true
draft: false
---

[CF 106194A - Sự cố A + B](https://codeforces.com/problemset/problem/106194/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên rất lớn, mỗi số biểu thị một số được ghi trên một thẻ ghi riêng. Nhiệm vụ không phải là tính toàn bộ tổng mà chỉ xác định chữ số cuối cùng của tổng của chúng. Giá trị này sau đó được sử dụng làm mã truy cập một chữ số cho khóa giá sách cơ học. 

Nói một cách cụ thể hơn, đầu vào bao gồm hai số nguyên$A$Và$B$, mỗi trong số đó có thể rất lớn, lên tới$10^{100}$. Đầu ra là một chữ số, tương ứng với$(A + B) \bmod 10$. 

Hạn chế về kích thước là tín hiệu chính ở đây. Con số lên đến$10^{100}$không thể được lưu trữ ở dạng số nguyên 64 bit tiêu chuẩn và thậm chí việc cộng số nguyên có độ dài tùy ý cũng sẽ là quá mức cần thiết nếu chúng ta chỉ quan tâm đến chữ số cuối cùng. Bất kỳ giải pháp nào cố gắng phân tích các số nguyên đầy đủ và tính tổng của chúng trực tiếp bằng số học số nguyên gốc sẽ không thành công hoặc nặng nề không cần thiết. 

Một trường hợp phức tạp phát sinh khi nghĩ về định dạng đầu vào. Vì các số có thể rất lớn nên chúng có thể bao gồm các chữ số đứng đầu vượt xa mọi khả năng loại số tiêu chuẩn. Ví dụ: 

đầu vào:```
99999999999999999999999999999999999999999999999999 1
```Đầu ra đúng:```
0
```Việc triển khai ngây thơ cố gắng chuyển đổi sang loại số nguyên sẽ thất bại ở đây. Ngay cả khi sử dụng các số nguyên lớn, việc tính tổng đầy đủ là công việc không cần thiết khi chỉ có chữ số cuối cùng quan trọng. 

Một trường hợp đặc biệt khác là khi một hoặc cả hai số có giá trị một chữ số, trong đó logic vẫn phải hoạt động thống nhất: 

đầu vào:```
7 8
```Đầu ra:```
5
```Sự đơn giản của đầu ra che giấu một thực tế là cùng một logic phải hoạt động thống nhất trên mọi mức độ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ diễn giải cả hai số dưới dạng số nguyên đầy đủ, chuyển đổi chúng thành loại số nguyên lớn, tính tổng và sau đó trích xuất chữ số cuối cùng. Điều này đúng về mặt khái niệm, vì bài toán quy về số học mô-đun. Tuy nhiên, trong các ngôn ngữ không có hỗ trợ độ chính xác tùy ý, điều này sẽ thất bại ngay lập tức do tràn. Ngay cả trong các ngôn ngữ hỗ trợ số nguyên lớn, cách tiếp cận này vẫn lãng phí vì nó xử lý tất cả các chữ số mặc dù chỉ có chữ số cuối cùng là quan trọng. 

Điều quan trọng cần lưu ý là chữ số cuối cùng của tổng chỉ phụ thuộc vào chữ số cuối cùng của phần cộng. Điều này theo sau từ số học mô-đun:$$(A + B) \bmod 10 = ((A \bmod 10) + (B \bmod 10)) \bmod 10$$Vì vậy, thay vì phân tích toàn bộ các số, chúng ta chỉ cần đọc các ký tự cuối cùng của chúng, chuyển đổi chúng thành số nguyên và tính tổng theo modulo 10. 

Điều này làm giảm vấn đề từ số học có độ dài tùy ý sang tính toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (cộng số nguyên lớn đầy đủ) | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (chỉ sử dụng chữ số cuối cùng) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai số dưới dạng chuỗi thay vì số nguyên. Điều này tránh mọi nguy cơ tràn và bảo toàn tất cả các chữ số chính xác như đã cho. Cấu trúc của bài toán đảm bảo rằng chúng ta không cần phân tích số ngoài việc kiểm tra ký tự. 
2. Trích xuất ký tự cuối cùng của chuỗi đầu tiên và chuyển nó thành một chữ số nguyên. Điều này thể hiện$A \bmod 10$, vì chữ số cuối cùng của số thập phân xác định đầy đủ phần dư của nó theo modulo 10. 
3. Trích xuất ký tự cuối cùng của chuỗi thứ hai và chuyển nó thành một chữ số nguyên. Điều này thể hiện$B \bmod 10$vì lý do tương tự. 
4. Tính tổng của hai chữ số này. 
5. Lấy kết quả theo modulo 10 để xử lý bất kỳ số mang nào vượt quá một chữ số, vì đầu ra cũng phải là một chữ số. 
6. Xuất ra chữ số kết quả. 

Tại sao nó hoạt động: biểu diễn thập phân bảo toàn cấu trúc mô-đun đối với 10, nghĩa là mọi chữ số ngoại trừ chữ số cuối cùng đóng góp bội số của 10 và do đó biến mất theo modulo 10. Thuật toán dựa trên bất biến rằng đối với bất kỳ số nguyên nào được viết trong cơ số 10, tất cả các tiền tố kết thúc trước chữ số cuối cùng đều chia hết cho 10 nên chúng không ảnh hưởng đến số dư cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = input().split()

da = ord(a[-1]) - ord('0')
db = ord(b[-1]) - ord('0')

print((da + db) % 10)
```Giải pháp đọc hai giá trị dưới dạng chuỗi và tránh mọi chuyển đổi số ngoài các ký tự đơn. biểu thức`a[-1]`Và`b[-1]`truy cập một cách an toàn vào chữ số cuối cùng bất kể độ dài. sử dụng`ord(...)-ord('0')`là một cách nhanh chóng và rõ ràng để chuyển đổi một chữ số ký tự thành số nguyên mà không cần thực hiện các quy trình phân tích cú pháp chậm hơn. 

Phép toán modulo cuối cùng đảm bảo tính chính xác ngay cả khi tổng của hai chữ số vượt quá 9, nếu không sẽ tạo ra kết quả trung gian có hai chữ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
114514 1919810
```Chúng tôi chỉ theo dõi các chữ số cuối cùng: 

| Bước | một[-1] | b[-1] | da | db | da + db | (da + db) % 10 | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 0 | 4 | 0 | - | - | 
| Tính toán | - | - | 4 | 0 | 4 | 4 | 

Đầu ra:```
4
```Điều này chứng tỏ rằng toàn bộ số lớn không liên quan gì ngoài chữ số cuối cùng của chúng. 

### Ví dụ 2 

đầu vào:```
99999999999999999999 99999999999999999999
```| Bước | một[-1] | b[-1] | da | db | da + db | (da + db) % 10 | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 9 | 9 | 9 | 9 | - | - | 
| Tính toán | - | - | 9 | 9 | 18 | 8 | 

Đầu ra:```
8
```Điều này xác nhận rằng việc truyền sóng mang vượt quá chữ số cuối cùng không thành vấn đề đối với modulo 10. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ truy cập ký tự theo thời gian cố định và số học | 
| Không gian | O(1) | Không có bộ nhớ ngoài hai chuỗi đầu vào | 

Giải pháp này phù hợp một cách thoải mái với mọi ràng buộc vì nó thực hiện một số thao tác cố định bất kể kích thước đầu vào, khiến nó trở nên tối ưu ngay cả khi đầu vào đạt độ dài tối đa là$10^{100}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b = sys.stdin.readline().split()
    return str((int(a[-1]) + int(b[-1])) % 10)

assert run("1 1") == "2"
assert run("114514 1919810") == "4"
assert run("0 0") == "0"
assert run("99999999999999999999 1") == "0"
assert run("123456789 987654321") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 | tính đúng đắn cơ bản | 
| 114514 1919810 | 4 | đầu vào lớn kiểu mẫu | 
| 0 0 | 0 | trường hợp ranh giới tối thiểu | 
| 999...9 1 | 0 | mang theo nhiều chữ số | 
| 123456789 987654321 | 0 | hủy bỏ chữ số lớn đối xứng | 

## Vỏ cạnh 

Đối với các đầu vào có cả hai số kết thúc bằng 9, việc mang có thể đề xuất phép cộng đầy đủ là cần thiết, nhưng thuật toán sẽ bỏ qua tất cả các chữ số cao hơn một cách an toàn. 

đầu vào:```
9 9
```Thực thi:```
da = 9, db = 9
sum = 18
output = 8
```Mặc dù tổng thực là 18 nhưng chỉ cần có chữ số cuối cùng và phương pháp trực tiếp tạo ra số đó mà không cần tính toán phép cộng đầy đủ. 

Đối với các trường hợp không nặng như:```
0 99999999999999999999
```Chỉ có chữ số cuối cùng của số thứ hai quan trọng:```
da = 0, db = 9
output = 9
```Thuật toán bỏ qua một cách chính xác tiền tố dài của số 0 và chữ số lớn vì chúng không liên quan theo số học modulo 10.
