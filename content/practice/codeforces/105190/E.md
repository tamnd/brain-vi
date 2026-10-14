---
title: "CF 105190E - Bài kiểm tra khó"
description: "Đầu vào chứa một số nguyên n. Giá trị này không ảnh hưởng đến đầu ra được yêu cầu. Nhiệm vụ đơn giản là đọc thành công số nguyên từ đầu vào tiêu chuẩn và sau đó in chuỗi AC."
date: "2026-06-27T04:19:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105190
codeforces_index: "E"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2024"
rating: 0
weight: 105190
solve_time_s: 31
verified: true
draft: false
---

[CF 105190E - Bài kiểm tra khó](https://codeforces.com/problemset/problem/105190/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào chứa một số nguyên duy nhất`n`. Giá trị này không ảnh hưởng đến đầu ra được yêu cầu. Nhiệm vụ đơn giản là đọc thành công số nguyên từ đầu vào tiêu chuẩn rồi in chuỗi`AC`. 

Ràng buộc`1 ≤ n ≤ 1000`không liên quan đến thuật toán vì giá trị không bao giờ được sử dụng. Đọc một số nguyên và in một chuỗi cố định đều mất thời gian và bộ nhớ không đổi. 

Lỗi nhỏ duy nhất là quên đọc đầu vào trước khi in. Trong khi nhiều giám khảo bỏ qua thông tin đầu vào chưa đọc đối với một bài toán đơn giản như vậy, thì các giải pháp lập trình cạnh tranh dự kiến ​​sẽ tiêu thụ thông tin đầu vào được cung cấp. 

Ví dụ, với đầu vào```
1
```đầu ra đúng là```
AC
```Một lỗi khác có thể xảy ra là in bất cứ thứ gì ngoài chuỗi chữ hoa chính xác. 

Ví dụ, với đầu vào```
1000
```in ấn```
ac
```hoặc```
Accepted
```không chính xác vì vấn đề rõ ràng yêu cầu đầu ra chính xác`AC`. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản đọc số nguyên và in`AC`. Vì chỉ có một giá trị đầu vào và không có phép tính nào phụ thuộc vào nó nên điều này đã giải quyết được toàn bộ vấn đề. Thời gian chạy là không đổi vì chính xác một số nguyên được đọc và một chuỗi được in. 

Không có thuật toán nào phức tạp hơn vì đầu vào là vô nghĩa. Thử thách chỉ tồn tại để xác minh rằng thí sinh có thể thực hiện chính xác đầu vào và đầu ra cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`từ đầu vào tiêu chuẩn. Bản thân giá trị này bị bỏ qua vì nó không ảnh hưởng đến đầu ra được yêu cầu. 
2. In chuỗi`AC`đúng như đã chỉ định. 

### Tại sao nó hoạt động 

Sự cố chấp nhận bất kỳ giá trị đầu vào nào nhưng luôn yêu cầu đầu ra giống nhau. Vì thuật toán luôn in`AC`sau khi sử dụng đầu vào, nó khớp với đặc điểm kỹ thuật cho mọi trường hợp kiểm thử hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

input()
print("AC")
```Đầu tiên chương trình sẽ đọc dòng đầu vào duy nhất. Giá trị bị loại bỏ vì nó không bao giờ cần thiết. Sau đó, nó in chính xác chuỗi yêu cầu. Không cần phân tích cú pháp hoặc tính toán bổ sung, do đó không có mối lo ngại nào về các điều kiện biên, lỗi tràn hoặc lỗi sai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
```| Bước | Đọc đầu vào | Đầu ra | 
| --- | --- | --- | 
| Đọc đầu vào | 1 | | 
| In | 1 | AC | 

Giá trị bị bỏ qua sau khi được đọc và chương trình sẽ in kết quả được yêu cầu. 

### Mẫu 2 

đầu vào:```
1000
```| Bước | Đọc đầu vào | Đầu ra | 
| --- | --- | --- | 
| Đọc đầu vào | 1000 | | 
| In | 1000 | AC | 

Ví dụ này cho thấy ngay cả đầu vào lớn nhất được phép cũng tạo ra đầu ra giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Đọc một dòng đầu vào và in một dòng đầu ra. | 
| Không gian | O(1) | Chỉ sử dụng một lượng bộ nhớ không đổi. | 

Giải pháp thực hiện một lượng công việc cố định bất kể giá trị đầu vào, do đó, nó dễ dàng đáp ứng mọi giới hạn thời gian và bộ nhớ hợp lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    input()
    print("AC")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out

# sample-style case
assert run("1\n") == "AC\n", "sample"

# custom cases
assert run("1000\n") == "AC\n", "maximum input"
assert run("500\n") == "AC\n", "middle value"
assert run("999\n") == "AC\n", "large value"
assert run("42\n") == "AC\n", "arbitrary value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1000`|`AC`| Đầu vào lớn nhất được phép. | 
|`500`|`AC`| Giá trị hợp lệ điển hình. | 
|`999`|`AC`| Một giá trị hợp lệ lớn khác. | 
|`42`|`AC`| Xác nhận giá trị đầu vào bị bỏ qua. | 

## Vỏ cạnh 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
1
```Thuật toán đọc giá trị, bỏ qua nó và in:```
AC
```Vì đầu ra độc lập với giá trị đầu vào nên điều này đúng. 

Bây giờ hãy xem xét đầu vào hợp lệ lớn nhất:```
1000
```Thuật toán lại đọc số nguyên và in ngay:```
AC
```Giá trị này không bao giờ ảnh hưởng đến việc thực thi nên dữ liệu đầu vào lớn nhất được xử lý giống hệt như mọi dữ liệu đầu vào hợp lệ khác.
