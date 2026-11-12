---
title: "CF 162A - Số ngũ giác"
description: "Chúng ta cần tính số ngũ giác thứ n. Các số ngũ giác tạo thành một chuỗi được tạo bởi công thức toán học trực tiếp: $$Pn = frac{3n^2 - n}{2}$$ Đầu vào chứa một số nguyên n và đầu ra là giá trị của công thức này cho vị trí đó trong chuỗi."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1100
weight: 162
solve_time_s: 190
verified: true
draft: false
---

[CF 162A - Số ngũ giác](https://codeforces.com/problemset/problem/162/A) 

**Đánh giá:** 1100 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tính toán`n`-số ngũ giác thứ Các số ngũ giác tạo thành một chuỗi được tạo bởi một công thức toán học trực tiếp:$$P_n = \frac{3n^2 - n}{2}$$Đầu vào chứa một số nguyên duy nhất`n`và đầu ra là giá trị của công thức này cho vị trí đó trong chuỗi. 

Những hạn chế là cực kỳ nhỏ. Từ`n`nhiều nhất là`100`, ngay cả một giải pháp rất kém hiệu quả cũng sẽ chạy ngay lập tức. Một vòng lặp có vài trăm thao tác là chuyện nhỏ trong giới hạn 3 giây. Điều này cũng có nghĩa là chúng tôi không cần các kỹ thuật tối ưu hóa nâng cao, tính toán trước hoặc cấu trúc dữ liệu đặc biệt. 

Rủi ro thực sự duy nhất là thực hiện sai công thức. Một lỗi phổ biến là hiểu sai thứ tự ưu tiên của toán tử hoặc quên phép chia cho`2`. Ví dụ: nếu ai đó viết:```
3 * n * n - n / 2
```thay vì:```
(3 * n * n - n) // 2
```kết quả trở nên sai bởi vì chỉ`n`được chia cho`2`. 

Hãy xem xét đầu vào:```
2
```Phép tính đúng là:$$\frac{3 \cdot 2^2 - 2}{2} = \frac{12 - 2}{2} = 5$$Việc thực hiện bất cẩn có thể tạo ra`11`hoặc`10`tùy thuộc vào dấu ngoặc đơn đặt sai vị trí. 

Một vấn đề tế nhị khác là sử dụng phép chia dấu phẩy động. Các số ngũ giác luôn là số nguyên, vì vậy số học số nguyên là cách tiếp cận rõ ràng nhất. sử dụng`/`trong Python tạo ra một float, điều này không cần thiết và có thể gây ra các vấn đề về định dạng. sử dụng`//`giữ câu trả lời dưới dạng số nguyên. 

Ví dụ:```
1
```Đầu ra đúng:```
1
```Sử dụng phép chia dấu phẩy động có thể in`1.0`, điều đó sẽ bị từ chối. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là tạo từng số ngũ giác một. Sự khác biệt giữa các số ngũ giác liên tiếp tăng tuyến tính:$$P_n - P_{n-1} = 3n - 2$$Bắt đầu từ`1`, chúng ta có thể liên tục thêm những khác biệt này cho đến khi đạt được`n`-giá trị thứ. Điều này hoạt động vì trình tự được xác định tăng dần. Với`n ≤ 100`, cách tiếp cận này dễ dàng nằm trong giới hạn vì nó chỉ thực hiện khoảng một trăm lần lặp. 

Điểm yếu của phương pháp này là nó bỏ qua thực tế là giá trị chính xác đã được đưa ra bởi một công thức trực tiếp. Mặc dù phương pháp lặp ở đây đủ nhanh nhưng nó vẫn thực hiện những công việc không cần thiết. 

Quan sát quan trọng là bài toán cung cấp biểu thức dạng đóng cho câu trả lời theo đúng nghĩa đen. Vì công thức tính toán trực tiếp`n`-giá trị thứ, chúng ta có thể đánh giá nó ngay lập tức trong thời gian không đổi. 

Phương pháp brute-force hoạt động vì chuỗi ngắn nhưng công thức cho phép chúng ta bỏ qua tất cả các giá trị trung gian và chuyển thẳng đến câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`từ đầu vào. 
2. Tính giá trị:$$\frac{3n^2 - n}{2}$$Chúng tôi sử dụng số học số nguyên vì số ngũ giác luôn là số nguyên. 

1. In kết quả tính toán. 

### Tại sao nó hoạt động 

Công thức cho số ngũ giác được định nghĩa về mặt toán học là:$$P_n = \frac{3n^2 - n}{2}$$Thuật toán đánh giá chính xác biểu thức này cho giá trị đã cho`n`. Do việc triển khai khớp trực tiếp với định nghĩa của chuỗi nên giá trị được tạo ra được đảm bảo là chính xác`n`-số ngũ giác thứ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

answer = (3 * n * n - n) // 2

print(answer)
```Chương trình bắt đầu bằng cách đọc số nguyên`n`. 

Tính toán chính sử dụng định nghĩa toán học chính xác của dãy ngũ giác. Biểu thức:```
(3 * n * n - n) // 2
```trận đấu:$$\frac{3n^2 - n}{2}$$Dấu ngoặc đơn rất quan trọng vì phép trừ phải xảy ra trước phép chia. Không có chúng, Python sẽ chỉ chia phần cuối cùng`n`qua`2`, thay đổi ý nghĩa của công thức. 

Giải pháp sử dụng`//`thay vì`/`nên kết quả vẫn là số nguyên. Các số ngũ giác được đảm bảo là số nguyên nên phép chia số nguyên là phép toán đúng. 

Vì việc tính toán chỉ bao gồm một số phép tính số học nên việc triển khai sẽ diễn ra ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Bước | Giá trị | 
| --- | --- | 
| n | 2 | 
| 3×n×n | 12 | 
| 3 × n × n - n | 10 | 
| Câu trả lời cuối cùng | 5 | 

Việc tính toán tuân theo đúng công thức. Sau khi trừ`n`từ`3n²`, chia cho`2`cho số ngũ giác thứ hai,`5`. 

### Ví dụ 2 

đầu vào:```
5
```| Bước | Giá trị | 
| --- | --- | 
| n | 5 | 
| 3×n×n | 75 | 
| 3 × n × n - n | 70 | 
| Câu trả lời cuối cùng | 35 | 

Dấu vết này chứng tỏ rằng thuật toán sẽ chia tỷ lệ một cách tự nhiên cho các giá trị lớn hơn. Không cần vòng lặp hoặc đệ quy vì công thức tính toán trực tiếp câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên | 

Các ràng buộc rất nhỏ nên giải pháp này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. Ngay cả Python được giải thích cũng thực hiện tính toán ngay lập tức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    print((3 * n * n - n) // 2)

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

# provided sample
assert run("2\n") == "5\n", "sample 1"

# custom cases
assert run("1\n") == "1\n", "minimum input"
assert run("3\n") == "12\n", "small odd value"
assert run("10\n") == "145\n", "larger value"
assert run("100\n") == "14950\n", "maximum constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào hợp lệ tối thiểu | 
|`3`|`12`| Số học đúng cho các giá trị nhỏ | 
|`10`|`145`| Công thức hoạt động với số lớn hơn | 
|`100`|`14950`| Hạn chế tối đa được xử lý chính xác | 

## Vỏ cạnh 

Đầu vào nhỏ nhất có thể là:```
1
```Thuật toán tính toán:$$\frac{3 \cdot 1^2 - 1}{2} = \frac{2}{2} = 1$$Chương trình in:```
1
```Điều này xác nhận rằng không có sai sót nào trong việc lập chỉ mục cho trình tự. 

Một nơi dễ thất bại khác là quyền ưu tiên của nhà điều hành. Coi như:```
2
```Đánh giá đúng là:$$\frac{3 \cdot 2^2 - 2}{2} = 5$$Việc triển khai sử dụng dấu ngoặc đơn một cách rõ ràng:```
(3 * n * n - n) // 2
```vì vậy phép trừ xảy ra trước khi chia, tránh kết quả sai. 

Đầu vào tối đa là:```
100
```Thuật toán tính toán:$$\frac{3 \cdot 100^2 - 100}{2} = \frac{30000 - 100}{2} = 14950$$Số nguyên Python dễ dàng xử lý phạm vi này, do đó không có rủi ro tràn.
