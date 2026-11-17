---
title: "CF 199A - Định lý thập lục phân"
description: "Chúng ta được cho một số nguyên n và câu lệnh đảm bảo rằng chính n là số Fibonacci. Nhiệm vụ là biểu diễn n dưới dạng tổng của ba số Fibonacci."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 199
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 125 (Div. 2)"
rating: 900
weight: 199
solve_time_s: 97
verified: true
draft: false
---

[CF 199A - Định lý thập lục phân](https://codeforces.com/problemset/problem/199/A) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thuật toán xây dựng, thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất`n`, và tuyên bố đảm bảo rằng`n`chính nó là một số Fibonacci. Nhiệm vụ là đại diện`n`bằng tổng của ba số Fibonacci. Ba giá trị được phép lặp lại và số 0 cũng là số Fibonacci hợp lệ vì chuỗi bắt đầu bằng`F0 = 0`. 

Vấn đề ban đầu nghe có vẻ giống như một vấn đề tìm kiếm lý thuyết số, nhưng các ràng buộc đã thay đổi hoàn toàn tình hình. Giá trị của`n`nhỏ hơn`10^9`, có nghĩa là dãy Fibonacci chứa rất ít số liên quan. Các giá trị Fibonacci tăng theo cấp số nhân nên chỉ có khoảng 45 số Fibonacci bên dưới`10^9`. Ngay cả lực lượng vũ phu bậc ba đối với tất cả các giá trị Fibonacci vẫn đủ nhỏ. 

Sai lầm nguy hiểm nhất là suy nghĩ quá nhiều về định lý. Câu lệnh cố gắng gợi ý rằng chúng ta cần chứng minh hoặc tìm kiếm một phân tách, nhưng đầu vào đã được đảm bảo là số Fibonacci. Điều đó ngay lập tức tạo ra một công trình tầm thường. 

Ví dụ: nếu đầu vào là:```
5
```sau đó:```
0 0 5
```đã hợp lệ vì cả ba số đều là số Fibonacci và tổng của chúng là`5`. 

Một sai lầm dễ dàng khác là quên điều đó`0`là một phần của dãy Fibonacci. Việc thực hiện bất cẩn có thể tạo ra các số Fibonacci bắt đầu từ`1, 1`, điều này sẽ từ chối không chính xác các câu trả lời hợp lệ, chẳng hạn như:```
1
```Đầu ra đúng có thể là:```
0 0 1
```Trường hợp tinh tế thứ ba là giá trị tối thiểu:```
0
```Đầu ra đúng là:```
0 0 0
```Một số triển khai vô tình coi số 0 là trường hợp đặc biệt không thể xảy ra, mặc dù bản thân nó là số Fibonacci. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đầu tiên hãy tạo mọi số Fibonacci lên đến`10^9`. Vì dãy phát triển nhanh chóng nên danh sách này chỉ có khoảng 45 phần tử. Sau đó thử từng bộ ba`(a, b, c)`và kiểm tra xem:```
a + b + c = n
```Điều này hoạt động vì không gian tìm kiếm rất nhỏ. Xung quanh`45^3 ≈ 91,000`sự kết hợp hoàn toàn có thể chấp nhận được trong giới hạn 2 giây. 

Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng mọi tổ hợp số Fibonacci có thể có. Vấn đề ở đây không phải là hiệu suất mà là sự phức tạp không cần thiết. Chúng ta đang giải quyết một vấn đề khó khăn hơn nhiều so với yêu cầu thực sự của tuyên bố. 

Quan sát quan trọng là bản thân đầu vào được đảm bảo là số Fibonacci. Vì số 0 cũng là Fibonacci nên ta luôn có thể viết:```
n = 0 + 0 + n
```Điều này ngay lập tức đưa ra một cấu trúc hợp lệ cho mọi đầu vào có thể. Không cần tìm kiếm, lập trình động hoặc máy móc lý thuyết số. 

Vấn đề thực sự là kiểm tra xem liệu chúng ta có nhận thấy cách biểu diễn hợp lệ đơn giản nhất hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k³) | O(k) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

Đây`k`là số dãy số Fibonacci dưới đây`10^9`, khoảng 45. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. In ra ba số Fibonacci có tổng bằng`n`. 
3. Vì`0`là số Fibonacci và`n`được đảm bảo là số Fibonacci, đầu ra:```
0 0 n
```1. Công trình này luôn hiệu quả vì:```
0 + 0 + n = n
```và cả ba số hạng đều là số Fibonacci hợp lệ. 

### Tại sao nó hoạt động 

Tính đúng đắn đến trực tiếp từ sự đảm bảo của vấn đề. Dãy Fibonacci bao gồm số 0 và đầu vào`n`cũng được đảm bảo thuộc về dãy. Thuật toán đưa ra chính xác ba số Fibonacci và tổng của chúng bằng mục tiêu cần thiết. Không có đầu vào nào khiến việc xây dựng này thất bại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

print(0, 0, n)
```Việc triển khai được cố ý tối thiểu vì quan sát toán học sẽ loại bỏ mọi sự phức tạp khỏi vấn đề. 

Chương trình đọc số Fibonacci đã cho và in ngay`0 0 n`. Không cần phải tạo chuỗi Fibonacci hoặc xác thực bất cứ điều gì vì câu lệnh đã đảm bảo rằng`n`là Fibonacci. 

Một chi tiết tinh tế là số 0 phải được coi là số Fibonacci hợp lệ. Toàn bộ công trình phụ thuộc vào thực tế đó. Vì trình tự được xác định bắt đầu từ`F0 = 0`, đầu ra luôn hợp pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Dấu vết: 

| Bước | n | Đầu ra | 
| --- | --- | --- | 
| Đọc đầu vào | 3 | | 
| Xây dựng câu trả lời | 3 | 0 0 3 | 

Xác minh:```
0 + 0 + 3 = 3
```Cả ba giá trị đều là số Fibonacci. 

Ví dụ này thể hiện quan sát cốt lõi của vấn đề. Mặc dù đầu ra mẫu sử dụng`1 1 1`, chúng ta có thể tự do in bất kỳ sự phân rã hợp lệ nào. 

### Ví dụ 2 

đầu vào:```
0
```Dấu vết: 

| Bước | n | Đầu ra | 
| --- | --- | --- | 
| Đọc đầu vào | 0 | | 
| Xây dựng câu trả lời | 0 | 0 0 0 | 

Xác minh:```
0 + 0 + 0 = 0
```Ví dụ này thực hiện trường hợp biên tối thiểu. Thuật toán vẫn hoạt động mà không cần xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện đọc và in đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp dễ dàng phù hợp trong giới hạn. Nó thực hiện một lượng công việc không đổi bất kể giá trị đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    print(0, 0, n)

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

# provided sample
assert run("3\n") == "0 0 3\n", "sample 1"

# minimum value
assert run("0\n") == "0 0 0\n", "minimum case"

# small Fibonacci number
assert run("1\n") == "0 0 1\n", "small Fibonacci"

# medium Fibonacci number
assert run("13\n") == "0 0 13\n", "medium case"

# large Fibonacci number below 1e9
assert run("832040\n") == "0 0 832040\n", "large Fibonacci"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`0 0 3`| Phân hủy hợp lệ tiêu chuẩn | 
|`0`|`0 0 0`| Trường hợp ranh giới tối thiểu | 
|`1`|`0 0 1`| Xử lý đúng các giá trị Fibonacci nhỏ lặp đi lặp lại | 
|`13`|`0 0 13`| Tính đúng đắn chung | 
|`832040`|`0 0 832040`| Đầu vào Fibonacci hợp lệ lớn | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là đầu vào nhỏ nhất có thể:```
0
```Thuật toán in:```
0 0 0
```Tổng vẫn bằng 0 và tất cả các giá trị đều thuộc dãy Fibonacci. Điều này xác nhận rằng công trình xử lý ranh giới phía dưới một cách chính xác. 

Một trường hợp tế nhị khác là:```
1
```Một số triển khai vô tình từ chối điều này vì họ quên rằng các số Fibonacci lặp lại được cho phép. Đầu ra thuật toán của chúng tôi:```
0 0 1
```đó là hợp lệ ngay lập tức. 

Một trường hợp dễ gây nhầm lẫn hơn là số Fibonacci lớn hơn, chẳng hạn như:```
144
```Các đầu ra thuật toán:```
0 0 144
```Không cần phải tìm kiếm các kết hợp như`55 + 89 + 0`. Việc xây dựng trực tiếp luôn thành công vì bản thân đầu vào đã là Fibonacci.
