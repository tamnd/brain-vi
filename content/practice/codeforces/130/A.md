---
title: "CF 130A - Số lục giác"
description: "Chúng ta cần tính số lục giác thứ $n$-th. Trình tự được xác định bởi công thức $$hn = 2n^2 - n$$ Đầu vào chứa một số nguyên $n$ và đầu ra là giá trị được tạo ra bởi công thức này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "A"
codeforces_contest_name: "Unknown Language Round 4"
rating: 900
weight: 130
solve_time_s: 84
verified: true
draft: false
---

[CF 130A - Số lục giác](https://codeforces.com/problemset/problem/130/A) 

**Xếp hạng:** 900 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tính toán$n$-số lục giác thứ Trình tự được xác định bởi công thức$$h_n = 2n^2 - n$$Đầu vào chứa một số nguyên duy nhất$n$và đầu ra là giá trị được tạo ra bởi công thức này. 

Ví dụ, khi$n = 2$,$$h_2 = 2 \cdot 2^2 - 2 = 8 - 2 = 6$$vậy câu trả lời là`6`. 

Những hạn chế là cực kỳ nhỏ. Giá trị lớn nhất có thể có của$n$chỉ là$100$, vì vậy ngay cả một thuật toán rất chậm cũng sẽ chạy ngay lập tức. Số lục giác lớn nhất mà chúng ta có thể tính được là$$2 \cdot 100^2 - 100 = 19900$$dễ dàng phù hợp với các loại số nguyên tiêu chuẩn. 

Rủi ro chính trong vấn đề này không phải là hiệu suất mà là việc thực hiện sai công thức. Một số lỗi phổ biến rất dễ mắc phải. 

Một lỗi có thể xảy ra là quên quyền ưu tiên của toán tử. Xem xét đầu vào```
2
```Tính toán đúng là$$2 \cdot 2^2 - 2 = 6$$Việc thực hiện bất cẩn như`2*n*n-n`thì ổn, nhưng viết một cái gì đó như`2*n^(2-n)`hoặc khó hiểu`^`với phép lũy thừa trong Python sẽ cho kết quả sai vì Python sử dụng`**`cho quyền lực. 

Một lỗi phổ biến khác là sử dụng sai công thức. Một số công thức số hình tượng trông giống nhau. Đối với đầu vào```
1
```đầu ra đúng là```
1
```bởi vì$$2 \cdot 1^2 - 1 = 1$$Nếu ai đó sử dụng nhầm$n^2$hoặc$3n^2-n$, trình tự sẽ trở nên không chính xác ngay lập tức. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là xây dựng từng lớp mẫu hình lục giác. Hình đầu tiên chứa$1$ô, ô tiếp theo sẽ thêm các ô xung quanh nó, v.v. cho đến khi đạt đến ô$n$-lớp thứ. Từ$n \le 100$, thậm chí việc mô phỏng trực tiếp việc xây dựng cũng đủ nhanh. 

Ví dụ: chúng ta có thể liên tục thêm số lượng ô được đóng góp bởi mỗi lớp mới và duy trì tổng số đang chạy. Điều này có tác dụng vì mỗi bước phản ánh định nghĩa hình học của các số lục giác. 

Điểm yếu của phương pháp đó là nó thực hiện những công việc không cần thiết. Chúng ta đã biết công thức toán học chính xác cho câu trả lời:$$h_n = 2n^2 - n$$Một khi chúng ta nhận ra rằng trình tự đã được đưa ra một cách rõ ràng thì không có lý do gì để mô phỏng bất cứ điều gì. Chúng ta có thể tính toán câu trả lời trực tiếp trong thời gian không đổi. 

Giải pháp tối ưu chỉ cần đọc$n$, đánh giá công thức và in kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$từ đầu vào. 
2. Tính giá trị$$2n^2 - n$$Điều này phù hợp trực tiếp với định nghĩa của$n$-số lục giác được cho trong bài toán. 

1. In giá trị tính toán. 

### Tại sao nó hoạt động 

Bài toán xác định trình tự rõ ràng bằng công thức$$h_n = 2n^2 - n$$Thuật toán đánh giá chính xác biểu thức này cho$n$. Vì không có phép tính gần đúng, phép lặp hoặc phép biến đổi nào liên quan nên giá trị được tạo ra chính xác là số lục giác cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

answer = 2 * n * n - n

print(answer)
```Chương trình bắt đầu bằng cách đọc số nguyên$n$. Vì chỉ có một giá trị nên không cần vòng lặp hoặc phân tích cú pháp bổ sung. 

biểu thức```
2 * n * n - n
```tính toán$2n^2 - n$. Viết theo cách này sẽ tránh được bất kỳ sự nhầm lẫn nào về các toán tử số mũ và giữ cho phép tính số học đơn giản. 

Các số nguyên Python tự động xử lý các giá trị lớn hơn nhiều so với yêu cầu của bài toán này, vì vậy việc tràn ở đây là không thể. 

Cuối cùng, kết quả được in trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Bước | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | - | - | 
| Tính đáp án | 2 |$2 \cdot 2^2 - 2$| 6 | 
| In | 2 | - | 6 | 

Ví dụ này xác nhận rằng công thức tạo ra câu trả lời mẫu chính xác. 

### Ví dụ 2 

đầu vào:```
5
```| Bước | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 5 | - | - | 
| Tính đáp án | 5 |$2 \cdot 5^2 - 5$| 45 | 
| In | 5 | - | 45 | 

Dấu vết này cho thấy thuật toán chia tỷ lệ trực tiếp với công thức. Không cần lặp lại hoặc mô phỏng, bất kể giá trị của$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chương trình thực hiện một số phép tính số học cố định | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc rất nhỏ nên số học theo thời gian không đổi dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    print(2 * n * n - n)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    solve()

    return out.getvalue().strip()

# provided samples
assert run("2\n") == "6", "sample 1"

# custom cases
assert run("1\n") == "1", "minimum input"
assert run("3\n") == "15", "small odd value"
assert run("10\n") == "190", "two-digit input"
assert run("100\n") == "19900", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào hợp lệ tối thiểu | 
|`3`|`15`| Số học đúng cho các giá trị nhỏ | 
|`10`|`190`| Công thức hoạt động với số lớn hơn | 
|`100`|`19900`| Giá trị ràng buộc tối đa | 

## Vỏ cạnh 

Đầu vào nhỏ nhất có thể là```
1
```Thuật toán tính toán$$2 \cdot 1^2 - 1 = 1$$và in```
1
```Điều này xác nhận rằng công thức xử lý ranh giới dưới một cách chính xác mà không có bất kỳ trường hợp đặc biệt nào. 

Một trường hợp hữu ích khác là đầu vào tối đa được phép:```
100
```Việc tính toán trở thành$$2 \cdot 100^2 - 100 = 2 \cdot 10000 - 100 = 19900$$Thuật toán in```
19900
```Điều này xác minh rằng việc triển khai xử lý chính xác ràng buộc trên và việc tràn số nguyên không phải là vấn đề. 

Trường hợp cạnh cuối cùng kiểm tra tính chính xác của toán tử. Đối với đầu vào```
2
```chương trình tính toán$$2 \cdot 2^2 - 2 = 6$$và in```
6
```Điều này phát hiện việc triển khai vô tình sử dụng sai cú pháp lũy thừa hoặc áp dụng sai công thức.
