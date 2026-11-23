---
title: "CF 470C - Đánh giá"
description: "Nhiệm vụ là đánh giá một biểu thức số học đơn giản có dạng a?b trong đó a và b là các số nguyên từ 1 đến 999 và ? là một toán tử đơn từ tập hợp +, -, , / hoặc %."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "C"
codeforces_contest_name: "Surprise Language Round 7"
rating: 1900
weight: 470
solve_time_s: 62
verified: true
draft: false
---

[CF 470C - Đánh giá](https://codeforces.com/problemset/problem/470/C) 

**Xếp hạng:** 1900 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là đánh giá một biểu thức số học đơn giản có dạng`a?b`Ở đâu`a`Và`b`là các số nguyên từ 1 đến 999 và`?`là một toán tử duy nhất từ ​​tập hợp`+`,`-`,`*`,`/`, hoặc`%`. Về mặt khái niệm, đây chỉ là lấy hai số và áp dụng thao tác được chỉ định, sau đó in số nguyên kết quả. 

Đầu vào là một chuỗi dòng đơn chứa biểu thức không có khoảng trắng. Điều đó có nghĩa là chúng tôi không thể dựa vào mã thông báo tiêu chuẩn bằng khoảng trắng; chúng ta cần xác định toán tử trong chuỗi, chia hai số tương ứng và thực hiện thao tác. 

Các ràng buộc rất nhỏ: các số luôn nằm trong phạm vi từ 1 đến 999, vừa vặn với số nguyên 32 bit tiêu chuẩn. Phép chia số nguyên`/`phải cắt ngắn về 0 và modulo`%`trả về phần dư của phép chia. Vì đầu vào chỉ có một dòng nên hiệu suất không phải là vấn đề đáng lo ngại; mọi giải pháp đúng sẽ chạy tốt dưới 2 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các biểu thức liên quan đến phép chia hoặc modulo, đặc biệt đảm bảo rằng việc chia cho 0 không bao giờ được thực hiện. Ở đây đảm bảo rằng cả hai`a`Và`b`ít nhất là 1 nên không thể chia cho 0. Một điều tinh tế khác là phân tích cú pháp: vì đầu vào không có khoảng trắng nên các số có nhiều chữ số cần được trích xuất chính xác và cần phải cẩn thận nếu toán tử`-`, có thể bị nhầm lẫn với dấu âm. Tuy nhiên, ở đây tất cả các số đều dương, vì vậy số đầu tiên`-`Found được đảm bảo là toán tử. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là đánh giá biểu thức bằng Python`eval()`sau khi đọc đầu vào dưới dạng chuỗi. Điều này hoạt động vì`eval()`có thể phân tích trực tiếp các chuỗi số học, nhưng sử dụng`eval()`trong lập trình cạnh tranh không được khuyến khích do những lo ngại về bảo mật tiềm ẩn và vì nó che khuất logic phân tích cú pháp. 

Cách tiếp cận tiêu chuẩn là quét chuỗi đầu vào cho toán tử. Một khi chúng ta tìm thấy nó, mọi thứ trước đó đều như vậy`a`và mọi thứ sau đó là`b`. Sau đó, chúng tôi sử dụng một điều kiện đơn giản hoặc một từ điển để ánh xạ ký tự toán tử tới thao tác Python tương ứng. Điều này vừa đơn giản vừa hiệu quả, vì đầu vào được đảm bảo có chính xác một toán tử và số lượng nhỏ. 

Ở đây, ý tưởng mạnh mẽ về việc kiểm tra tất cả các toán tử trên tất cả các phần tách là không cần thiết vì chỉ có một toán tử, do đó, chỉ cần thực hiện một lần là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các phần tách) | O(n) | O(1) | Quá mức cần thiết nhưng được chấp nhận | 
| Quét toán tử một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và loại bỏ mọi khoảng trắng xung quanh để đảm bảo phân tích cú pháp rõ ràng. Điều này đảm bảo rằng các dòng mới vô tình ở cuối không cản trở việc chuyển đổi sang số nguyên. 
2. Quét từng ký tự chuỗi cho đến khi tìm thấy toán tử. Vì nhà điều hành được đảm bảo là một trong những`+`,`-`,`*`,`/`,`%`và có chính xác một, lần xuất hiện đầu tiên là toán tử. 
3. Tách chuỗi tại toán tử. Chuỗi con trước nó là`a`và chuỗi con sau nó là`b`. Chuyển đổi cả hai chuỗi con thành số nguyên. 
4. Tùy thuộc vào người vận hành mà thực hiện phép tính tương ứng: cộng, trừ, nhân, chia số nguyên hoặc modulo. 
5. In kết quả dưới dạng số nguyên. Để phân chia, Python`//`toán tử xử lý phép chia cắt ngắn một cách chính xác. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến tại bất kỳ thời điểm nào trước khi đánh giá,`a`Và`b`chính xác là các giá trị số nguyên từ chuỗi đầu vào và toán tử được xác định chính xác. Không có phép toán nào khác hoặc sự mơ hồ trong dữ liệu đầu vào, vì vậy điều này tạo ra kết quả số học chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

expr = input().strip()

for i, c in enumerate(expr):
    if c in '+-*/%':
        op_index = i
        break

a = int(expr[:op_index])
b = int(expr[op_index+1:])
op = expr[op_index]

if op == '+':
    result = a + b
elif op == '-':
    result = a - b
elif op == '*':
    result = a * b
elif op == '/':
    result = a // b
elif op == '%':
    result = a % b

print(result)
```Vòng lặp đầu tiên tìm toán tử bằng cách quét từ trái sang phải. Việc chuyển đổi các lát của chuỗi thành số nguyên đảm bảo rằng các số có nhiều chữ số được xử lý chính xác. Ánh xạ có điều kiện từ ký tự toán tử đến phép toán số học thực hiện các quy tắc đánh giá chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1`123+456`: 

| Bước | kinh nghiệm | tôi | c | op_index | một | b | op | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 123+456 | 0 | '1' | - | - | - | - | - | 
| 2 | 123+456 | 1 | '2' | - | - | - | - | - | 
| 3 | 123+456 | 2 | '3' | - | - | - | - | - | 
| 4 | 123+456 | 3 | '+' | 3 | 123 | 456 | '+' | 579 | 

Dấu vết xác nhận rằng toán tử được xác định chính xác, các số được phân tích cú pháp chính xác và phép cộng tạo ra kết quả mong đợi. 

Đầu vào khác`999/3`: 

| Bước | kinh nghiệm | tôi | c | op_index | một | b | op | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 999/3 | 0 | '9' | - | - | - | - | - | 
| 2 | 999/3 | 3 | '/' | 3 | 999 | 3 | '/' | 333 | 

Dấu vết xác nhận phép chia số nguyên hoạt động như dự định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét chuỗi một lần để tìm toán tử; n ₫ 7 vì các số là ∼999 và toán tử cộng thêm 1. | 
| Không gian | O(1) | Chỉ có một số biến số nguyên và các lát chuỗi được lưu trữ. | 

Giải pháp vừa vặn thoải mái trong vòng 2 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    expr = input().strip()
    for i, c in enumerate(expr):
        if c in '+-*/%':
            op_index = i
            break
    a = int(expr[:op_index])
    b = int(expr[op_index+1:])
    op = expr[op_index]
    if op == '+':
        result = a + b
    elif op == '-':
        result = a - b
    elif op == '*':
        result = a * b
    elif op == '/':
        result = a // b
    elif op == '%':
        result = a % b
    return str(result)

# provided sample
assert run("123+456") == "579", "sample 1"

# custom cases
assert run("1+1") == "2", "minimum values"
assert run("999*999") == "998001", "maximum values"
assert run("100-100") == "0", "equal numbers subtraction"
assert run("999/3") == "333", "integer division"
assert run("100%3") == "1", "modulo operation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1+1 | 2 | Giá trị đầu vào tối thiểu có thể | 
| 999*999 | 998001 | Giá trị đầu vào tối đa và độ chính xác của phép nhân | 
| 100-100 | 0 | Phép trừ dẫn đến số 0 | 
| 999/3 | 333 | Tính chính xác của phép chia số nguyên | 
| 100%3 | 1 | Độ chính xác của hoạt động Modulo | 

## Vỏ cạnh 

Đối với biểu thức`1+1`, thuật toán tìm đúng`+`tại chỉ số 1, phân tích`a=1`Và`b=1`và đầu ra 2. Đối với`999/3`, nó xác định`/`tại chỉ số 3, phân tích`a=999`Và`b=3`, thực hiện phép chia số nguyên với`//`, thu được 333. Đối với`100%3`, nó xác định`%`và đầu ra 1. Trong mỗi trường hợp, khả năng phân tách và phát hiện toán tử đều mạnh mẽ và thuật toán tạo ra kết quả chính xác mà không cần xử lý đặc biệt vì đầu vào được đảm bảo khớp với định dạng mong đợi.
