---
title: "CF 153A - A + B"
description: "Chúng ta được yêu cầu tính tổng của hai số nguyên, nhưng điểm nhấn là phải định dạng đầu ra cẩn thận. Cụ thể, đầu vào bao gồm hai số, mỗi số nằm trên một dòng riêng và chúng ta cần in tổng của chúng mà không có bất kỳ số 0 nào đứng đầu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 153
codeforces_index: "A"
codeforces_contest_name: "Surprise Language Round 5"
rating: 1600
weight: 153
solve_time_s: 59
verified: true
draft: false
---

[CF 153A - A + B](https://codeforces.com/problemset/problem/153/A) 

**Đánh giá:** 1600 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính tổng của hai số nguyên, nhưng điểm nhấn là phải định dạng đầu ra cẩn thận. Cụ thể, đầu vào bao gồm hai số, mỗi số nằm trên một dòng riêng và chúng ta cần in tổng của chúng mà không có bất kỳ số 0 nào đứng đầu. Điều này có nghĩa là ngay cả khi tổng đó là, chẳng hạn,`00015`, chúng ta nên xuất ra`15`. 

Các ràng buộc rất đơn giản: cả hai số nguyên đều nằm trong khoảng từ 1 đến 100.000. Các giới hạn này đủ nhỏ để chúng ta không cần lo lắng về việc tràn số nguyên trong Python và tổng sẽ luôn nhỏ hơn hoặc bằng 200.000. Kích thước đầu vào rất nhỏ, vì vậy mọi thuật toán hợp lý sẽ chạy trong micro giây; thách thức là tính đúng đắn chứ không phải tính hiệu quả. 

Các trường hợp đặc biệt không rõ ràng phát sinh từ khả năng hiểu sai đầu vào dưới dạng chuỗi. Ví dụ, nếu một số`"00012"`(về mặt kỹ thuật nằm ngoài ràng buộc đầu vào vì A và B là số nguyên dương, nhưng có thể xuất hiện trong các biến thể của vấn đề), việc nối hoặc xuất chuỗi bất cẩn có thể tạo ra`"00015"`thay vì`"15"`. Một kịch bản khác là khi tổng chính xác bằng`100000`hoặc một số giá trị biên, một lần nữa kiểm tra xem đầu ra có phải là số và bị loại bỏ các số 0 ở đầu hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản coi các số là chuỗi, chuyển đổi chúng thành số nguyên, cộng chúng và in kết quả. Điều này hoạt động vì Python xử lý số học số nguyên một cách chính xác cho đến các giá trị lớn tùy ý. Không cần phải triển khai phép cộng từng chữ số theo cách thủ công vì phạm vi đầu vào nhỏ và số nguyên Python tự động quản lý việc chuyển nguồn. Trong trường hợp bắt buộc, người ta có thể cố gắng lặp qua từng chữ số từ ít quan trọng nhất đến quan trọng nhất và tính tổng theo cách thủ công, nhưng điều này sẽ dài dòng không cần thiết và dễ xảy ra lỗi. 

Quan sát quan trọng là tính năng tích hợp sẵn của Python`int`type đã xử lý cả phân tích số từ chuỗi và xóa các số 0 đứng đầu khi chuyển đổi lại thành chuỗi thông qua`print`. Do đó, cách tiếp cận tối ưu chỉ đơn giản là đọc từng số, chuyển đổi nó thành số nguyên, tính tổng và in kết quả. Không cần mảng, vòng lặp hoặc kiểm tra bổ sung. Cách tiếp cận vũ phu sẽ chỉ "quá chậm" trong ngôn ngữ giả định nếu không có sự hỗ trợ số nguyên lớn, nhưng ở đây nó thực sự tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cộng từng chữ số theo cách thủ công | O(log(max(A,B))) | O(log(max(A,B))) | Không cần thiết, dài dòng | 
| Số học tích hợp Python | O(1) | O(1) | Được chấp nhận, đơn giản và chính xác | 

## Hướng dẫn thuật toán 

1. Đọc dòng đầu tiên của dữ liệu đầu vào và chuyển nó thành số nguyên. Điều này đảm bảo rằng mọi số 0 đứng đầu trong đầu vào, nếu có, sẽ tự động bị bỏ qua. 
2. Đọc dòng đầu vào thứ hai và chuyển nó thành số nguyên vì lý do tương tự. 
3. Tính tổng của hai số nguyên. 
4. In số tiền trực tiếp. các`print`hàm chuyển đổi số nguyên thành một chuỗi, theo mặc định không bao gồm các số 0 đứng đầu. 

Tại sao nó hoạt động: điều bất biến ở đây là ở mỗi bước chúng ta thao tác trên các giá trị nguyên thực. Bằng cách chuyển đổi ngay lập tức từ chuỗi sang số nguyên, chúng tôi loại bỏ mọi sự mơ hồ khỏi các số 0 đứng đầu. Tổng của hai số nguyên dương luôn tạo ra một số nguyên dương và việc in một số nguyên trong Python đảm bảo không có số 0 đứng đầu, khớp chính xác với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A = int(input())
B = int(input())
print(A + B)
```Hai dòng đầu tiên đọc dữ liệu đầu vào và chuyển nó thành số nguyên. Chuyển đổi này tự động xử lý việc loại bỏ mọi số 0 vô tình đứng đầu. Dòng cuối cùng tính tổng và in ra. Không có vòng lặp hoặc điều kiện vì bài toán không yêu cầu chúng. Vì kích thước đầu vào rất nhỏ nên mã này chạy tốt trong giới hạn thời gian và bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
12
3
```| Bước | A | B | Tổng hợp | 
| --- | --- | --- | --- | 
| Đọc A | 12 | - | - | 
| Đọc B | 12 | 3 | - | 
| Tính A + B | 12 | 3 | 15 | 
| Đầu ra | 15 | | | 

Dấu vết này xác nhận rằng việc chuyển đổi từ chuỗi sang số nguyên xử lý chính xác các số bình thường và tổng được tính chính xác. 

### Mẫu 2 (đã thi công) 

đầu vào:```
100000
5
```| Bước | A | B | Tổng hợp | 
| --- | --- | --- | --- | 
| Đọc A | 100000 | - | - | 
| Đọc B | 100000 | 5 | - | 
| Tính A + B | 100000 | 5 | 100005 | 
| Đầu ra | 100005 | | | 

Điều này chứng tỏ rằng thuật toán xử lý các giá trị biên mà không gặp vấn đề gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Đọc và chuyển đổi hai số nguyên và tính tổng chúng là thời gian không đổi với kích thước đầu vào nhỏ. | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được lưu trữ; không có mảng hoặc cấu trúc phụ trợ được yêu cầu. | 

Với những hạn chế, giải pháp này chạy trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu, đáp ứng đầy đủ các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    A = int(input())
    B = int(input())
    return str(A + B)

# provided sample
assert run("12\n3\n") == "15", "sample 1"

# minimum input
assert run("1\n1\n") == "2", "minimum input"

# maximum input
assert run("100000\n100000\n") == "200000", "maximum input"

# mixed size inputs
assert run("100\n5\n") == "105", "small and small input"

# edge case for output without leading zeros
assert run("00012\n00003\n") == "15", "leading zeros input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 12\n3 | 15 | Tổng bình thường | 
| 1\n1 | 2 | Giá trị đầu vào tối thiểu | 
| 100000\n100000 | 200000 | Giá trị đầu vào tối đa | 
| 100\n5 | 105 | Giá trị nhỏ hỗn hợp | 
| 00012\n00003 | 15 | Đầu vào có số 0 đứng đầu | 

## Vỏ cạnh 

Đối với đầu vào có số 0 đứng đầu ngẫu nhiên, chẳng hạn như`"00012"`Và`"00003"`, thuật toán sẽ chuyển đổi cả hai thành số nguyên ngay lập tức. Python phiên dịch`"00012"`là 12 và`"00003"`bằng 3. Tổng được tính là 15 và được in ra mà không có số 0 đứng đầu. Việc chuyển đổi nội bộ đảm bảo tính chính xác ngay cả khi định dạng đầu vào không thông thường. Các giá trị biên 1 và 100.000 xác nhận rằng không xảy ra hiện tượng tràn số nguyên cũng như định dạng sai.
