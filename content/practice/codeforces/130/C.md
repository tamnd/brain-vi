---
title: "CF 130C - Tổng thập phân"
description: "Chúng ta được cho một dãy số nguyên và cần tính tổng của chúng. Đầu vào bắt đầu bằng số nguyên n, cho chúng ta biết có bao nhiêu số theo sau. Trong n dòng tiếp theo, mỗi dòng chứa một phần tử của mảng. Nhiệm vụ chỉ đơn giản là cộng tất cả chúng lại với nhau và in kết quả."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "C"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1500
weight: 130
solve_time_s: 108
verified: true
draft: false
---

[CF 130C - Tổng thập phân](https://codeforces.com/problemset/problem/130/C) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và cần tính tổng của chúng. Đầu vào bắt đầu bằng một số nguyên`n`, cho chúng ta biết có bao nhiêu số theo sau. Mỗi cái tiếp theo`n`dòng chứa một phần tử của mảng. Nhiệm vụ chỉ đơn giản là cộng tất cả chúng lại với nhau và in kết quả. 

Những hạn chế là cực kỳ nhỏ. Mảng chứa tối đa 100 số và mỗi số nằm trong khoảng từ 1 đến 100. Ngay cả thuật toán đơn giản nhất cũng dễ dàng nằm trong giới hạn. Quét tuyến tính trên mảng chỉ thực hiện 100 phép cộng trong trường hợp xấu nhất, tức thời. 

Mặc dù nhiệm vụ rất đơn giản nhưng vẫn có một số nơi việc triển khai bất cẩn có thể thất bại. 

Một lỗi phổ biến là quên chuyển đổi chuỗi đầu vào thành số nguyên trước khi thêm chúng. Ví dụ:```
3
1
2
3
```Đầu ra đúng là:```
6
```Nếu ai đó vô tình nối các chuỗi thay vì tính tổng các số nguyên, họ có thể tạo ra`"123"`thay vì. 

Một sai lầm khác là chỉ đọc một dòng sau`n`thay vì đọc tất cả`n`các phần tử. Coi như:```
5
1
2
3
4
5
```Câu trả lời đúng là:```
15
```Việc triển khai có lỗi chỉ đọc số đầu tiên sẽ xuất ra không chính xác`1`. 

Ngoài ra còn có trường hợp kích thước tối thiểu:```
1
42
```Câu trả lời vẫn phải được tính toán chính xác:```
42
```Các thuật toán giả định ít nhất hai phần tử có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là dùng vũ lực: đọc từng số một và tính tổng. Sau khi xử lý hết`n`số, in tổng tích lũy. 

Điều này hiệu quả vì bản thân định nghĩa của vấn đề đã mang tính bổ sung. Mọi phần tử đều đóng góp độc lập vào kết quả cuối cùng, vì vậy chúng ta không bao giờ cần phải xem lại các giá trị trước đó hoặc lưu trữ trạng thái phức tạp. Thuật toán thực hiện chính xác`n`bổ sung. Với`n ≤ 100`, trường hợp xấu nhất chỉ là 100 phép tính. 

Không cần phải tối ưu hóa phức tạp hơn. Cách tiếp cận bạo lực đã là tối ưu vì mọi số đầu vào phải được kiểm tra ít nhất một lần. Bất kỳ thuật toán đúng nào cũng có giới hạn dưới của`O(n)`thời gian kể từ khi bỏ qua một phần tử có thể bỏ sót một phần của tổng. 

Quan sát quan trọng là vấn đề không có cấu trúc ẩn. Không có truy vấn, cập nhật hoặc ràng buộc đặt hàng. Một đường truyền tuyến tính đơn vừa là giải pháp đơn giản nhất vừa nhanh nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số phần tử trong mảng. 
2. Khởi tạo một biến`total = 0`. Biến này lưu trữ tổng chạy của tất cả các số được xử lý cho đến nay. 
3. Lặp lại`n`lần: 

1. Đọc số nguyên tiếp theo. 
2. Thêm nó vào`total`. 
4. Sau khi tất cả các số được xử lý, hãy in`total`. 

Tại sao nó hoạt động: 

Ở mỗi bước,`total`bằng tổng của tất cả các phần tử đã đọc cho đến nay. Ban đầu, không có phần tử nào được xử lý nên tổng đúng là`0`. Mỗi lần lặp sẽ thêm chính xác một phần tử mảng mới vào tổng hiện có, giữ nguyên bất biến. Sau lần lặp cuối cùng, mỗi phần tử mảng đã được thêm vào đúng một lần, vì vậy`total`bằng tổng mảng hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

total = 0

for _ in range(n):
    total += int(input())

print(total)
```Chương trình bắt đầu bằng việc đọc`n`, xác định có bao nhiêu số nguyên theo sau. Biến`total`lưu trữ số tiền tích lũy. 

Vòng lặp chạy chính xác`n`lần. Trong mỗi lần lặp, một số nguyên được đọc và thêm ngay vào`total`. Vì các ràng buộc rất nhỏ nên không cần tối ưu hóa đặc biệt nào ngoài việc xử lý đầu vào nhanh tiêu chuẩn. 

Một chi tiết tinh tế là chuyển đổi đầu vào thành số nguyên bằng cách sử dụng`int(input())`. Đầu vào đến dưới dạng chuỗi và việc không chuyển đổi sẽ tạo ra hành vi không chính xác. 

Việc triển khai cũng tránh việc lưu trữ toàn bộ mảng. Vì mỗi giá trị được sử dụng chính xác một lần nên việc xử lý các số khi chúng được đọc sẽ giữ mức sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1
2
3
4
5
```| Bước | Số hiện tại | tổng cộng Trước | tổng Sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 2 | 1 | 3 | 
| 3 | 3 | 3 | 6 | 
| 4 | 4 | 6 | 10 | 
| 5 | 5 | 10 | 15 | 

Đầu ra cuối cùng:```
15
```Dấu vết này cho thấy sự bất biến một cách rõ ràng. Sau mỗi lần lặp,`total`khớp với tổng của tất cả các phần tử được xử lý trước đó. 

### Ví dụ 2 

đầu vào:```
1
42
```| Bước | Số hiện tại | tổng cộng Trước | tổng Sau | 
| --- | --- | --- | --- | 
| 1 | 42 | 0 | 42 | 

Đầu ra cuối cùng:```
42
```Ví dụ này thực hiện trường hợp ranh giới kích thước tối thiểu. Thuật toán vẫn hoạt động vì vòng lặp tự nhiên xử lý một lần lặp duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đọc và thêm một lần | 
| Không gian | O(1) | Chỉ có tổng số tiền được lưu trữ | 

Với tối đa 100 số, chương trình thực hiện tối đa 100 phép cộng. Cả thời gian chạy và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn của vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    total = 0

    for _ in range(n):
        total += int(input())

    print(total)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run("5\n1\n2\n3\n4\n5\n") == "15\n", "sample 1"

# minimum-size input
assert run("1\n42\n") == "42\n", "single element"

# all equal values
assert run("4\n7\n7\n7\n7\n") == "28\n", "all equal"

# boundary values
assert run("3\n100\n100\n100\n") == "300\n", "maximum element values"

# off-by-one style case
assert run("2\n1\n100\n") == "101\n", "reads exactly n elements"

# larger case
assert run("5\n10\n20\n30\n40\n50\n") == "150\n", "general accumulation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 42`|`42`| Mảng kích thước tối thiểu | 
|`4 7 7 7 7`|`28`| Các giá trị giống hệt nhau lặp đi lặp lại | 
|`3 100 100 100`|`300`| Giá trị phần tử tối đa được phép | 
|`2 1 100`|`101`| Xử lý đúng chính xác`n`đầu vào | 
|`5 10 20 30 40 50`|`150`| Logic tích lũy chung | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
42
```Thuật toán khởi tạo`total = 0`, sau đó xử lý chính xác một số. 

Sau khi đọc`42`, tổng số tiền sẽ trở thành:```
total = 0 + 42 = 42
```Câu trả lời được in cuối cùng là:```
42
```Điều này xác nhận vòng lặp xử lý chính xác một lần lặp. 

Bây giờ hãy xem xét các giá trị lặp lại:```
4
7
7
7
7
```Việc thực thi diễn ra như sau:```
0 -> 7 -> 14 -> 21 -> 28
```Mỗi lần xuất hiện được thêm độc lập nên việc trùng lặp không gây ra vấn đề gì. 

Cuối cùng, hãy xem xét các giá trị phần tử tối đa:```
3
100
100
100
```Tổng số tiền đang chạy phát triển như thế này:```
0 -> 100 -> 200 -> 300
```Số nguyên Python dễ dàng xử lý các giá trị này, do đó không có rủi ro tràn. Thuật toán xuất ra chính xác:```
300
```
