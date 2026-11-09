---
title: "CF 1505B - ​​DMCA"
description: "Nhiệm vụ che giấu khái niệm quen thuộc về gốc kỹ thuật số đằng sau thuật ngữ bất thường. Chúng ta được cho một số nguyên dương và cần phải thay thế nó nhiều lần bằng tổng các chữ số thập phân của nó cho đến khi chỉ còn lại một chữ số duy nhất. Chữ số cuối cùng đó là câu trả lời."
date: "2026-06-10T20:28:50+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1505
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest 2021"
rating: 1600
weight: 1505
solve_time_s: 340
verified: false
draft: false
---

[CF 1505B - DMCA](https://codeforces.com/problemset/problem/1505/B) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, thực hiện, lý thuyết số 
**Thời gian giải:** 5 phút 40 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ che giấu khái niệm quen thuộc về gốc kỹ thuật số đằng sau thuật ngữ bất thường. Chúng ta được cho một số nguyên dương và cần phải thay thế nó nhiều lần bằng tổng các chữ số thập phân của nó cho đến khi chỉ còn lại một chữ số duy nhất. Chữ số cuối cùng đó là câu trả lời. Ví dụ: bắt đầu từ 81 ta được 8 + 1 = 9 nên kết quả là 9. Bắt đầu từ 999 ta được 27, rồi 2 + 7 = 9. 

Giá trị đầu vào tối đa là một triệu. Ngay cả một mô phỏng đơn giản cũng chỉ thực hiện một vài lần lặp vì tổng mỗi chữ số làm giảm đáng kể số lượng. Với những giới hạn nhỏ như vậy, mọi cách tiếp cận hợp lý đều diễn ra thoải mái trong vòng một giây. 

Nguồn gốc của sai lầm là do hiểu sai loại "root" nào là cần thiết. Thay vào đó, một giải pháp bất cẩn có thể tính ra căn bậc hai. 

Xem xét đầu vào```
81
```Đầu ra đúng là```
9
```vì thao tác bắt buộc là tính tổng các chữ số chứ không phải √81. 

Một trường hợp tinh tế khác là một số đã có một chữ số. 

Đối với đầu vào```
1
```câu trả lời là```
1
```Công thức dựa trên modulo 9 phải xử lý trường hợp này một cách cẩn thận, nếu không nó có thể trả về 0 không chính xác. 

Bội số của 9 là một cái bẫy phổ biến khác. 

Đối với đầu vào```
18
```câu trả lời là```
9
```sử dụng`a % 9`trực tiếp cho kết quả 0, điều này sai vì căn bậc hai của mọi bội số dương của 9 đều bằng 9. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp nhất liên tục tính tổng các chữ số thập phân cho đến khi giá trị hiện tại trở thành một chữ số. Quá trình này đúng vì nó tuân theo định nghĩa một cách chính xác. Vì đầu vào không bao giờ vượt quá một triệu nên số này có tối đa bảy chữ số và chỉ cần thực hiện vài vòng. Mặc dù thời gian chạy đã rất ít nhưng vẫn có một cách thậm chí còn ngắn hơn. 

Quan sát quan trọng đến từ đặc tính nổi tiếng của gốc kỹ thuật số. Việc thay thế một số bằng tổng các chữ số của nó sẽ bảo toàn số dư của nó theo modulo 9. Lặp lại thao tác cho đến khi còn lại một chữ số có nghĩa là đáp án cuối cùng được xác định hoàn toàn bởi modulo 9 còn lại. 

Có một ngoại lệ. Các bội số dương của 9 có số dư là 0, nhưng nghiệm số của chúng là 9 chứ không phải 0. Việc kết hợp các số liệu này sẽ cho một nghiệm thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số chữ số × lần lặp) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`. 
2. Nếu`a % 9 != 0`, đầu ra`a % 9`. Các số có cùng phần dư modulo 9 có cùng gốc số. 
3. Mặt khác, phân biệt giá trị 0 và bội số dương của 9. Vì đầu vào luôn dương nên đầu ra 9. 

### Tại sao nó hoạt động 

Lấy tổng các chữ số thập phân sẽ thay đổi một số thành bội số của 9, do đó phần dư 9 không bao giờ thay đổi. Việc lặp lại thao tác cuối cùng tạo ra một chữ số từ 1 đến 9. Trong số các chữ số đó, chữ số duy nhất bằng 0 modulo 9 chính là số 9. Do đó, mọi bội số dương của 9 đều có căn số 9 và mọi số khác đều có căn số bằng phần dư modulo 9 của nó. Thuật toán tính trực tiếp giá trị đó nên không thể tạo ra câu trả lời sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())

ans = a % 9
if ans == 0:
    ans = 9

print(ans)
```Chương trình bắt đầu bằng cách đọc số đầu vào duy nhất. 

Biến`ans`lưu trữ phần dư modulo 9. Đối với các số không chia hết cho 9, phần dư này đã bằng gốc số. 

Trường hợp đặc biệt duy nhất xảy ra khi số dư bằng 0. Vì đầu vào được đảm bảo là số dương, điều đó có nghĩa là số này là bội số dương của 9, có gốc số là 9. Việc thay thế 0 bằng 9 sẽ xử lý ranh giới này một cách chính xác. 

Không có vòng lặp hoặc đệ quy được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| một | một % 9 | Câu trả lời cuối cùng | 
| --- | --- | --- | 
| 1 | 1 | 1 | 

Số dư đã khác 0 nên nó trở thành đáp án trực tiếp. Điều này xác nhận rằng các số có một chữ số vẫn không thay đổi. 

### Ví dụ 2 

đầu vào:```
81
```| một | một % 9 | Câu trả lời cuối cùng | 
| --- | --- | --- | 
| 81 | 0 | 9 | 

Vì 81 chia hết cho 9 nên số dư bằng 0. Quy tắc đặc biệt chuyển đổi giá trị đó thành 9. Ví dụ này thể hiện trường hợp ranh giới không tầm thường duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không có thêm dung lượng tùy thuộc vào kích thước đầu vào được sử dụng | 

Giải pháp dễ dàng thỏa mãn các giới hạn. Thời gian không đổi và bộ nhớ không đổi nhỏ hơn nhiều so với những gì các ràng buộc cho phép. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    a = int(input())
    ans = a % 9
    if ans == 0:
        ans = 9

    return str(ans) + "\n"

# provided samples
assert run("1\n") == "1\n", "sample 1"

# custom cases
assert run("81\n") == "9\n", "multiple of 9"
assert run("18\n") == "9\n", "boundary remainder 0"
assert run("10\n") == "1\n", "digit sum 1+0"
assert run("1000000\n") == "1\n", "maximum input"
assert run("999999\n") == "9\n", "large multiple of 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 81 | 9 | Xử lý bội số của 9 | 
| 18 | 9 | Trường hợp đặc biệt còn lại bằng 0 | 
| 10 | 1 | Số dư khác 0 thông thường | 
| 1000000 | 1 | Giá trị đầu vào tối đa | 
| 999999 | 9 | Bội số lớn của 9 | 

## Vỏ cạnh 

Một số đã có một chữ số sẽ không thay đổi. 

Đối với đầu vào```
1
```thuật toán tính toán`1 % 9 = 1`, vì vậy nó in 1. Không cần xử lý đặc biệt. 

Bội số dương của 9 cần được chăm sóc cẩn thận. 

Đối với đầu vào```
18
```việc thực hiện là: 

| một | một % 9 | Đầu ra | 
| --- | --- | --- | 
| 18 | 0 | 9 | 

Phần còn lại bằng 0 nên thuật toán thay thế nó bằng 9. Số này khớp với dãy 18 → 9 thu được bằng các tổng chữ số lặp lại. 

Các giá trị lớn hoạt động theo cùng một cách. 

Đối với đầu vào```
1000000
```việc thực hiện là: 

| một | một % 9 | Đầu ra | 
| --- | --- | --- | 
| 1000000 | 1 | 1 | 

Tổng các chữ số lặp lại cho 1 + 0 + 0 + 0 + 0 + 0 + 0 = 1, phù hợp với phép tính modulo.
