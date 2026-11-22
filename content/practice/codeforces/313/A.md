---
title: "CF 313A - Ilya và Tài khoản ngân hàng"
description: "Vấn đề tập trung vào số dư tài khoản ngân hàng của Ilya, một số nguyên có dấu. Giá trị dương biểu thị khoản tiết kiệm và giá trị âm biểu thị nợ."
date: "2026-06-06T01:03:21+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 313
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 186 (Div. 2)"
rating: 900
weight: 313
solve_time_s: 56
verified: true
draft: false
---

[CF 313A - Ilya và Tài khoản ngân hàng](https://codeforces.com/problemset/problem/313/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, lý thuyết số 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề tập trung vào số dư tài khoản ngân hàng của Ilya, một số nguyên có dấu. Giá trị dương biểu thị khoản tiết kiệm và giá trị âm biểu thị nợ. Ngân hàng cho phép Ilya thực hiện thao tác một lần: anh ấy có thể xóa chữ số cuối cùng hoặc chữ số thứ hai đến chữ số cuối cùng khỏi số dư của mình để có khả năng cải thiện số dư. Mục tiêu là xác định số dư tối đa có thể đạt được bằng thao tác này hoặc giữ nguyên số nếu điều đó tạo ra kết quả tốt nhất. 

Đầu vào là một số nguyên duy nhất`n`Ở đâu`10 ≤ |n| ≤ 10^9`. Giới hạn dưới đảm bảo số có ít nhất hai chữ số, điều này làm cho thao tác loại bỏ các chữ số trở nên có ý nghĩa. Giới hạn trên nằm thoải mái trong phạm vi số nguyên 32 bit, do đó các phép tính số học được an toàn mà không cần xử lý chính xác tùy ý. 

Các trường hợp cạnh không rõ ràng bao gồm các số âm có giá trị tuyệt đối nhỏ, chẳng hạn như`-10`hoặc`-11`. Chỉ cần bỏ chữ số cuối cùng có thể tạo ra`-1`hoặc`-1`tương ứng, và việc bỏ chữ số từ thứ hai đến cuối cùng có thể tạo ra`0`hoặc`-1`. Việc triển khai bất cẩn có thể bỏ qua tùy chọn chữ số từ thứ hai đến cuối cùng hoặc tính toán sai giá trị mới, mang lại kết quả dưới mức tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thực hiện mọi sự kết hợp để loại bỏ một chữ số hoặc giữ nguyên số đó. Đối với các số dương, việc loại bỏ các chữ số sẽ hầu như không bao giờ làm tăng giá trị, do đó, lực lượng vũ phu giảm xuống việc kiểm tra hai thao tác để tìm số âm. Số lần kiểm tra tối đa là ba: giữ`n`nguyên trạng, xóa chữ số cuối cùng, xóa chữ số thứ hai đến cuối cùng. Vì đây là công việc không đổi trên mỗi đầu vào nên lực lượng mạnh mẽ thực sự có thể chấp nhận được trong thực tế. 

Quan sát quan trọng dẫn đến một giải pháp tối ưu đơn giản là đối với các số dương, không làm gì luôn là tối ưu. Đối với số âm, cải tiến tiềm năng duy nhất đến từ việc loại bỏ chữ số cuối cùng hoặc chữ số thứ hai đến cuối cùng. Việc chuyển đổi các số đã sửa đổi này thành số nguyên và lấy số lớn nhất trong số ba tùy chọn sẽ đảm bảo tính chính xác. Quan sát này làm giảm vấn đề thành các phép toán O(1) mà không có bất kỳ vòng lặp hoặc đệ quy nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`từ đầu vào. Điều này thể hiện trạng thái tài khoản ngân hàng hiện tại. 
2. Nếu`n`không âm, in`n`ngay lập tức vì việc loại bỏ các chữ số không thể cải thiện số dư dương. 
3. Nếu`n`là tiêu cực, chuyển đổi`n`vào một chuỗi để thao tác các chữ số của nó. Cho phép`s`là biểu diễn chuỗi của`n`. 
4. Lập 2 số dư dự tuyển: bỏ chữ số cuối cùng để tạo thành`candidate1`và xóa chữ số thứ hai đến cuối cùng để tạo thành`candidate2`. Điều này được thực hiện bằng cách cắt chuỗi:`s[:-1]`cho chữ số cuối cùng và`s[:-2] + s[-1]`cho chữ số thứ hai đến chữ số cuối cùng. Chuyển đổi các chuỗi này trở lại thành số nguyên. 
5. Tính giá trị lớn nhất trong ba giá trị:`n`,`candidate1`, Và`candidate2`. Mức tối đa này là số dư tài khoản tốt nhất có thể. 
6. In kết quả. 

Tính đúng đắn phụ thuộc vào tính bất biến, đối với số âm, việc loại bỏ các chữ số sẽ làm cho số đó ít âm hơn hoặc giữ nguyên. Không có hoạt động nào khác cần xem xét nên việc so sánh ba khả năng là đầy đủ và đảm bảo kết quả tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n >= 0:
    print(n)
else:
    s = str(n)
    candidate1 = int(s[:-1])  # remove last digit
    candidate2 = int(s[:-2] + s[-1])  # remove second-to-last digit
    print(max(n, candidate1, candidate2))
```Giải pháp bắt đầu bằng cách xử lý các số dương riêng biệt để tránh các thao tác chuỗi không cần thiết. Đối với số âm, việc cắt chuỗi đảm bảo chúng tôi loại bỏ chính xác các chữ số mục tiêu. Một lỗi phổ biến là lập chỉ mục sai khi loại bỏ chữ số từ thứ hai đến cuối cùng;`s[:-2] + s[-1]`bảo toàn chính xác các chữ số trước hai chữ số cuối cùng và nối thêm chữ số cuối cùng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2230
```| Bước | n | s | ứng cử viên1 | ứng cử viên2 | tối đa(n, c1, c2) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 22h30 | Không áp dụng | Không áp dụng | Không áp dụng | 22h30 | 

Đầu vào là dương nên việc loại bỏ chữ số không có lợi. Thuật toán trả về 2230. 

Đầu vào mẫu 2:```
-123
```| Bước | n | s | ứng cử viên1 | ứng cử viên2 | tối đa(n, c1, c2) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | -123 | "-123" | -12 | -13 | -12 | 

Loại bỏ chữ số cuối cùng sẽ cho ra -12, tốt hơn -13 hoặc -123. Số dư tối đa là -12. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học và cắt chuỗi theo thời gian không đổi. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên và một chuỗi ngắn được lưu trữ. | 

Thuật toán phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB cho bất kỳ đầu vào hợp lệ nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    if n >= 0:
        return str(n)
    else:
        s = str(n)
        candidate1 = int(s[:-1])
        candidate2 = int(s[:-2] + s[-1])
        return str(max(n, candidate1, candidate2))

# provided samples
assert run("2230\n") == "2230", "sample 1"
assert run("-123\n") == "-12", "sample 2"

# custom cases
assert run("-10\n") == "0", "negative two-digit boundary"
assert run("-11\n") == "-1", "negative two-digit equal digits"
assert run("10\n") == "10", "positive two-digit minimum"
assert run("-1000000000\n") == "-100000000", "large negative number"
assert run("999999999\n") == "999999999", "large positive number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| -10 | 0 | Xóa chữ số thứ hai đến cuối cùng sẽ tạo ra 0 | 
| -11 | -1 | Loại bỏ một trong hai chữ số mang lại kết quả tốt nhất là -1 | 
| 10 | 10 | Số dương sẽ không thay đổi | 
| -1000000000 | -100000000 | Xử lý chính xác số âm lớn | 
| 999999999 | 999999999 | Xử lý số dương lớn | 

## Vỏ cạnh 

cho`n = -10`, loại bỏ chữ số cuối cùng sẽ tạo ra`-1`nhưng loại bỏ chữ số thứ hai đến cuối cùng mang lại`0`, đó là mức tối đa. Thuật toán tính toán`candidate1 = -1`Và`candidate2 = 0`, sau đó quay trở lại`max(-10, -1, 0) = 0`, thể hiện việc xử lý đúng các số âm nhỏ. 

Vì`n = -11`, cả hai ứng cử viên đều sản xuất`-1`. Thuật toán tính toán`candidate1 = -1`Và`candidate2 = -1`, sau đó quay trở lại`max(-11, -1, -1) = -1`, xác nhận rằng nó xử lý chính xác các chữ số lặp lại.
