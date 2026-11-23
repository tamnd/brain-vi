---
title: "CF 470A - Chuỗi quả cầu pha lê"
description: "Trình tự trong bài toán này xuất phát từ việc đếm các điểm mạng bên trong một hình lục giác đang phát triển trên một lưới lục giác. Thay vì phải xây dựng hình lục giác hoặc đếm điểm theo cách thủ công, bài toán đã đưa ra công thức cho dãy: $$Hn = 3 cdot n cdot (n+1) + 1$$ Đầu vào…"
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "A"
codeforces_contest_name: "Surprise Language Round 7"
rating: 1400
weight: 470
solve_time_s: 96
verified: true
draft: false
---

[CF 470A - Chuỗi quả cầu pha lê](https://codeforces.com/problemset/problem/470/A) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trình tự trong bài toán này xuất phát từ việc đếm các điểm mạng bên trong một hình lục giác đang phát triển trên một lưới lục giác. Thay vì phải dựng hình lục giác hoặc đếm điểm một cách thủ công, bài toán đã đưa ra công thức cho dãy:$$H_n = 3 \cdot n \cdot (n+1) + 1$$Đầu vào bao gồm một số nguyên duy nhất`n`, biểu thị chỉ số của phần tử dãy mà chúng ta cần tính toán. Đầu ra là giá trị của$H_n$. 

Những hạn chế là cực kỳ nhỏ. Giá trị của`n`bao gồm từ 0 đến 9. Ngay cả câu trả lời lớn nhất có thể cũng rất nhỏ:$$H_9 = 3 \cdot 9 \cdot 10 + 1 = 271$$Vì những giới hạn này nên hiệu suất không phải là vấn đề đáng lo ngại. Bất kỳ giải pháp hợp lý nào đều chạy ngay lập tức. Nhiệm vụ thực sự chỉ đơn giản là áp dụng đúng công thức đã cho. 

Trường hợp một cạnh là`n = 0`. Nhiều vấn đề về trình tự bắt đầu từ chỉ mục 1 và việc triển khai bất cẩn có thể vô tình cho rằng điều đó. Ví dụ: 

đầu vào:```
0
```Đầu ra đúng:```
1
```Thay vào công thức sẽ có:$$3 \cdot 0 \cdot 1 + 1 = 1$$Một lỗi phổ biến khác là đọc sai công thức`3*n*n + 1`thay vì`3*n*(n+1) + 1`. 

Ví dụ: 

đầu vào:```
1
```Đầu ra đúng:```
7
```Phép tính đúng là:$$3 \cdot 1 \cdot 2 + 1 = 7$$sử dụng`3*n*n+1`sẽ tạo ra sai 4. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là lập mô hình mạng lục giác và đếm tất cả các điểm chứa trong một hình lục giác với`n+1`điểm ở mỗi bên. Giải pháp như vậy sẽ tạo ra tọa độ, xác định điểm nào thuộc về hình dạng và đếm chúng. Nó sẽ đúng vì nó mô phỏng trực tiếp định nghĩa hình học của chuỗi. 

Mặc dù những hạn chế là rất nhỏ nhưng cách tiếp cận này là không cần thiết. Vấn đề đã cung cấp công thức chính xác cho câu trả lời. Một khi định nghĩa dãy đã được đưa ra bằng đại số thì không có lý do gì để tái tạo lại hình học. 

Quan sát chính là giá trị mong muốn được xác định rõ ràng là:$$H_n = 3n(n+1)+1$$Vì vậy, toàn bộ vấn đề giảm xuống việc đọc`n`, đánh giá biểu thức và in kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Hn) | O(Hn) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`từ đầu vào. 
2. Tính giá trị chuỗi bằng công thức đã cho:$$H_n = 3 \cdot n \cdot (n+1) + 1$$Đây chính xác là cách xác định trình tự, do đó không cần phải có đạo hàm bổ sung. 
3. Xuất giá trị tính toán. 

### Tại sao nó hoạt động 

Vấn đề xác định trực tiếp phần tử chuỗi là:$$H_n = 3n(n+1)+1$$Thuật toán đánh giá công thức này cho đầu vào đã cho`n`. Vì bản thân công thức là định nghĩa của dãy nên giá trị được tính toán chính xác là câu trả lời cần thiết. Không có phép tính gần đúng, mô phỏng hoặc lý luận trung gian nào có thể làm thay đổi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
print(3 * n * (n + 1) + 1)
```Chương trình bắt đầu bằng cách đọc giá trị đầu vào duy nhất`n`. 

Sau đó nó đánh giá`3 * n * (n + 1) + 1`, phù hợp với định nghĩa toán học được đưa ra trong câu lệnh. Phép nhân được thực hiện trước phép cộng theo thứ tự ưu tiên của toán tử Python, nhưng các dấu ngoặc đơn xung quanh`(n + 1)`làm cho tính toán dự định hoàn toàn rõ ràng. 

Cuối cùng, kết quả được in. 

Không có vấn đề về ranh giới vì công thức hoạt động với mọi đầu vào hợp lệ, bao gồm`n = 0`. Tràn số nguyên cũng không thể xảy ra trong Python và câu trả lời lớn nhất trong bài toán này chỉ là 271. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Bước | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 1 | - | - | 
| Áp dụng công thức | 1 | 3 × 1 × (1 + 1) + 1 | 7 | 
| Đầu ra | 1 | - | 7 | 

Dấu vết cho thấy trường hợp khác 0 đơn giản nhất. Công thức tạo ra 7, khớp với kết quả mẫu. 

### Ví dụ 2 

đầu vào:```
9
```| Bước | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 9 | - | - | 
| Áp dụng công thức | 9 | 3×9×10+1 | 271 | 
| Đầu ra | 9 | - | 271 | 

Ví dụ này sử dụng đầu vào lớn nhất được phép. Việc tính toán vẫn là một biểu thức số học duy nhất và tạo ra 271. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Thuật toán thực hiện một đánh giá công thức bất kể giá trị đầu vào. Thời gian chạy và mức sử dụng bộ nhớ của nó là không đổi, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input())
    print(3 * n * (n + 1) + 1)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("1\n") == "7", "sample 1"

# custom cases
assert run("0\n") == "1", "minimum input"
assert run("2\n") == "19", "small positive value"
assert run("5\n") == "91", "middle of range"
assert run("9\n") == "271", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Trình tự bắt đầu chính xác ở chỉ số 0 | 
|`2`|`19`| Sử dụng đúng`n + 1`trong công thức | 
|`5`|`91`| Trường hợp không ranh giới điển hình | 
|`9`|`271`| Đầu vào tối đa được phép | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là chỉ số nhỏ nhất có thể. 

đầu vào:```
0
```Thuật toán tính toán:$$3 \cdot 0 \cdot (0+1) + 1 = 1$$Đầu ra:```
1
```Điều này xác nhận rằng công thức vẫn hợp lệ ở ranh giới dưới và chuỗi có chỉ mục bằng 0. 

Một trường hợp tinh vi khác là kiểm tra xem việc triển khai có sử dụng đúng cách hay không`(n + 1)`. 

đầu vào:```
1
```Thuật toán tính toán:$$3 \cdot 1 \cdot 2 + 1 = 7$$Đầu ra:```
7
```Việc triển khai không chính xác bằng cách sử dụng`3*n*n + 1`sẽ trả về 4. Kiểm tra này xác minh rằng công thức đã được sao chép chính xác. 

Ranh giới trên cũng đáng được kiểm tra. 

đầu vào:```
9
```Thuật toán tính toán:$$3 \cdot 9 \cdot 10 + 1 = 271$$Đầu ra:```
271
```Quá trình tính toán vẫn nằm trong giới hạn số nguyên và chứng minh rằng giải pháp xử lý chính xác dữ liệu đầu vào lớn nhất được phép.
