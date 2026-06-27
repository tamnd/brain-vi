---
title: "CF 105383J - Chỉ cần làm tròn xuống"
description: "Chúng ta được cho một số thực dương duy nhất được viết dưới dạng một chuỗi có dấu thập phân. Nhiệm vụ là tính toán giá trị sàn của nó, nghĩa là số nguyên lớn nhất không vượt quá giá trị và xuất ra số nguyên đó không có phần thập phân."
date: "2026-06-23T05:26:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105383
codeforces_index: "J"
codeforces_contest_name: "2024 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 105383
solve_time_s: 49
verified: true
draft: false
---

[CF 105383J - Chỉ cần làm tròn xuống](https://codeforces.com/problemset/problem/105383/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số thực dương duy nhất được viết dưới dạng một chuỗi có dấu thập phân. Nhiệm vụ là tính toán giá trị sàn của nó, nghĩa là số nguyên lớn nhất không vượt quá giá trị và xuất ra số nguyên đó không có phần thập phân. 

Định dạng đầu vào được cố ý tối thiểu, chỉ một dòng như`1999.99`,`2.00000`hoặc các giá trị tiềm năng như`0.123`. Cấu trúc đảm bảo chính xác một dấu thập phân và ít nhất một chữ số ở cả hai mặt của nó. Giá trị được giới hạn ở trên bởi 10^8, nhưng vì đầu vào là một chuỗi nên hạn chế thực sự quan trọng là nó vừa khít với bộ nhớ và có thể được xử lý từng ký tự. 

Từ quan điểm phức tạp, kích thước đầu vào được giới hạn ở mức 15 byte, điều này ngay lập tức loại trừ mọi nhu cầu phân tích thư viện hoặc thuật toán số hiệu suất cao. Bất kỳ giải pháp nào quét chuỗi một lần là đủ. Ngay cả cách tiếp cận O(n) với công việc liên tục trên mỗi ký tự cũng nhanh chóng một cách tầm thường. 

Điểm tinh tế chính là việc phân tích cú pháp dấu phẩy động trong các ngôn ngữ lập trình không phải là thứ chúng ta nên dựa vào ở đây. Việc chuyển đổi sang dạng float và sau đó áp dụng hàm sàn có nguy cơ xảy ra lỗi chính xác đối với các biểu diễn thập phân lớn hơn hoặc khó xử lý. Cách tiếp cận dựa trên chuỗi sẽ tránh được điều này hoàn toàn. 

Trường hợp cạnh xuất hiện ở hai dạng. Đầu tiên, các số đã là số nguyên nhưng được viết bằng các số 0 ở cuối dấu thập phân. Ví dụ,`2.00000`nên xuất ra`2`. Một chuyển đổi float ngây thơ có thể tạo ra`1`nếu xảy ra lỗi làm tròn, mặc dù trong hầu hết các ngôn ngữ, đầu vào cụ thể này là an toàn nhưng vẫn có rủi ro không cần thiết. 

Thứ hai, các số nhỏ hơn 1 chẳng hạn như`0.99999`hoặc`0.12345`. Trong những trường hợp này, sàn được`0`. Một cách tiếp cận ngây thơ trích xuất không chính xác phần nguyên dưới dạng số và cho rằng nó khác 0 sẽ thất bại nếu nó cố diễn giải chuỗi không đúng. 

Trường hợp tinh vi thứ ba là các phần nguyên cực lớn như`100000000.000`, trong đó tính chính xác phụ thuộc vào việc bảo toàn chính xác chuỗi con số nguyên mà không bị tràn hoặc thay đổi định dạng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi đầu vào là số thực: chuyển chuỗi thành giá trị dấu phẩy động, áp dụng thao tác sàn và in kết quả. Điều này hoạt động theo nghĩa là nó phù hợp với định nghĩa toán học và trong nhiều trường hợp điển hình, nó sẽ vượt qua. 

Tuy nhiên, cách tiếp cận này dựa vào biểu diễn dấu phẩy động nhị phân. Những con số như`1999.99`không thể biểu diễn chính xác ở dạng nhị phân, vì vậy việc làm tròn trung gian có thể gây ra các lỗi nhỏ. Mặc dù những lỗi này thường không nhìn thấy được nhưng các thao tác trên sàn rất nhạy cảm với chúng vì một giá trị như`1.99999999997`có thể được hiểu chính xác là`1`, nhưng trường hợp ranh giới bị trình bày sai một chút có thể thay đổi bất ngờ. 

Quan trọng hơn, bài toán này không yêu cầu bất kỳ phép tính số học nào cả. Cấu trúc của dữ liệu đầu vào đã mã hóa trực tiếp câu trả lời: mọi phần trước dấu thập phân là phần nguyên và sàn bỏ qua mọi phần sau dấu thập phân. 

Quan sát quan trọng là đối với bất kỳ số thập phân dương nào được viết ở dạng chuẩn, giá trị sàn chính xác là chuỗi con trước dấu thập phân được hiểu là số nguyên. Không cần tính toán thêm. 

Điều này làm giảm nhiệm vụ từ tính toán số đến phân tích chuỗi, loại bỏ hoàn toàn các rủi ro về độ chính xác và giảm giải pháp xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuyển đổi float + sàn) | O(n) | O(1) | Rủi ro / không cần thiết | 
| Tối ưu (phân tích chuỗi) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng kết quả trực tiếp từ chuỗi đầu vào. 

1. Đọc chuỗi đầu vào. 
2. Quét chuỗi cho đến khi gặp dấu thập phân. 
3. Trích xuất tất cả các ký tự trước dấu thập phân. 
4. Xuất chuỗi con này dưới dạng số nguyên. 

Mỗi bước được chứng minh bằng cấu trúc biểu diễn thập phân. Dấu thập phân là dấu phân cách chính xác giữa phần nguyên và phần phân số, vì vậy mọi thứ sau nó không liên quan đến sàn. 

### Tại sao nó hoạt động 

Giá trị sàn của một số thập phân dương chỉ phụ thuộc vào số lượng đơn vị được chứa đầy đủ trong giá trị. Các chữ số trước dấu thập phân mã hóa chính xác đại lượng đó. Phần phân số, bất kể độ lớn, không thể tăng giá trị sàn vì nó luôn nhỏ hơn 1. Do đó, việc loại bỏ phần phân số sẽ bảo toàn kết quả nguyên chính xác trong tất cả các dữ liệu đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

i = 0
while i < len(s) and s[i] != '.':
    i += 1

print(int(s[:i]))
```Giải pháp đọc đầu vào dưới dạng chuỗi và quét cho đến khi tìm thấy dấu thập phân. Mọi thứ trước điểm đó đều là phần nguyên. Việc chuyển đổi chuỗi con đó thành số nguyên sẽ tự động xử lý các trường hợp như số 0 đứng đầu nếu chúng tồn tại, mặc dù vấn đề đảm bảo không có số 0 đứng đầu cho các số lớn hơn hoặc bằng 1. 

Chi tiết triển khai quan trọng là tránh mọi chuyển đổi dấu phẩy động. Mặc dù Python xử lý những điều này một cách an toàn cho nhiều đầu vào, cách tiếp cận chuỗi mang tính xác định và căn chỉnh chính xác với định nghĩa về sàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1999.99`Chúng tôi quét chuỗi cho đến dấu thập phân và tách tiền tố. 

| Bước | Chỉ mục | Nhân vật | Tiền tố | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 9 | 19 | 
| 3 | 2 | 9 | 199 | 
| 4 | 3 | 9 | 1999 | 
| 5 | 4 | . | dừng lại | 

Tiền tố được trích xuất là`1999`, được in. Phần phân số`.99`không liên quan đến sàn vì nó hoàn toàn nhỏ hơn 1. 

### Ví dụ 2:`2.00000`| Bước | Chỉ mục | Nhân vật | Tiền tố | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | 
| 2 | 1 | . | dừng lại | 

Tiền tố là`2`, vì vậy đầu ra là`2`. Mặc dù phần phân số dài nhưng nó không đóng góp gì vào số nguyên. 

Những ví dụ này xác nhận rằng thuật toán chỉ phụ thuộc vào việc định vị dấu phân cách thập phân và bỏ qua mọi thứ sau nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Vượt qua một lần cho đến khi tìm thấy dấu thập phân | 
| Không gian | O(1) | Chỉ sử dụng chỉ mục và tham chiếu chuỗi con | 

Kích thước đầu vào tối đa là 15 ký tự, do đó, ngay cả quét tuyến tính cũng có hiệu quả là thời gian không đổi. Giải pháp tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    i = 0
    while i < len(s) and s[i] != '.':
        i += 1
    return str(int(s[:i]))

# provided samples
assert run("1999.99\n") == "1999"
assert run("2.00000\n") == "2"

# custom cases
assert run("0.123\n") == "0", "fraction less than 1"
assert run("100.999\n") == "100", "normal rounding down behavior"
assert run("7.0\n") == "7", "integer represented as float"
assert run("99999999.999\n") == "99999999", "large integer part boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0,123 | 0 | giá trị dưới 1 | 
| 100.999 | 100 | cắt ngắn phân số | 
| 7.0 | 7 | số float giống như số nguyên | 
| 99999999.999 | 99999999 | số nguyên biên lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là các số nhỏ hơn 1, chẳng hạn như`0.123`. Thuật toán quét cho đến dấu thập phân và tạo ra tiền tố trống nếu số bắt đầu bằng`0.`. Chuyển đổi`0`mang lại sàn một cách chính xác, đó là`0`. 

Một trường hợp khác là đầu vào như`2.00000`, trong đó phần phân số không trống nhưng hoàn toàn bằng không. Quá trình quét dừng ở dấu thập phân và bỏ qua tất cả các số 0 ở cuối. Tiền tố số nguyên`2`đại diện chính xác cho sàn. 

Một trường hợp ranh giới số nguyên lớn hơn như`99999999.999`chứng minh rằng không cần chuyển đổi tràn hoặc dấu phẩy động. Tiền tố được trích xuất an toàn dưới dạng chuỗi và được chuyển đổi thành số nguyên mà không làm mất độ chính xác. 

Trong tất cả các trường hợp, bất biến chính là chuỗi con trước dấu thập phân luôn bằng tầng toán học của số cho tất cả các đầu vào hợp lệ trong biểu diễn thập phân tiêu chuẩn.
