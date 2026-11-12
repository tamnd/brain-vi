---
title: "CF 168A - Pháp sư và biểu diễn"
description: "Thành phố có n cư dân thực sự. Trong số đó, có chính xác x là các pháp sư và tất cả các pháp sư đó sẽ tham dự một buổi biểu diễn. Sẽ không có ai khác tham dự. Chính quyền đo lường số người tham dự theo tỷ lệ phần trăm của dân số thành phố thực, tỷ lệ này vẫn là n ngay cả khi các pháp sư tạo ra các bản sao."
date: "2026-06-02T08:31:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 168
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 114 (Div. 2)"
rating: 900
weight: 168
solve_time_s: 121
verified: true
draft: false
---

[CF 168A - Pháp sư và Trình diễn](https://codeforces.com/problemset/problem/168/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thành phố có`n`cư dân thực sự. Trong số đó, chính xác`x`đều là những pháp sư, và tất cả những pháp sư đó sẽ tham dự một buổi biểu diễn. Sẽ không có ai khác tham dự. 

Chính quyền đo lường số lượng người tham dự theo phần trăm dân số thực của thành phố, con số này vẫn còn`n`ngay cả khi các trình hướng dẫn tạo bản sao. Một bản sao được tính là người tham dự nhưng không làm tăng quy mô dân số được sử dụng trong tính toán tỷ lệ phần trăm. 

Chúng ta phải tìm số lượng bản sao nhỏ nhất sao cho tổng số người tham dự là`x + clones`, ít nhất là`y%`của`n`. 

Những hạn chế là rất nhỏ. Mọi giá trị nhiều nhất là`10^4`, vì vậy ngay cả một mô phỏng đơn giản cũng có thể chạy thoải mái trong thời gian giới hạn. Tuy nhiên, bài toán vẫn có lời giải toán học trực tiếp để tính ra đáp án trong thời gian không đổi. 

Sự tinh tế chính là sự tham dự là một số nguyên người. Nếu tỷ lệ phần trăm được yêu cầu tương ứng với số người tham dự không phải là số nguyên thì chúng tôi phải làm tròn số. 

Coi như`n = 10`,`x = 1`,`y = 14`. 

Mười bốn phần trăm của mười người là`1.4`. Vì điểm danh phải là số nguyên nên ít nhất`2`người tham dự được yêu cầu. Một thuật sĩ đã tham dự nên cần có một bản sao. 

Một lỗi thường gặp là tính toán`(n * y) // 100`, làm tròn xuống. Ở đây nó mang lại`1`, gợi ý không chính xác rằng không cần phải sao chép. 

Một trường hợp đặc biệt khác xuất hiện khi các trình hướng dẫn hiện tại đã đáp ứng được yêu cầu. 

Ví dụ:```
10 10 100
```Sự tham dự cần thiết là`10`, và tất cả mười pháp sư đều đã tham dự. Câu trả lời đúng là`0`. Thực hiện bất cẩn luôn tính toán`required - x`không lấy mức tối đa bằng 0 có thể tạo ra câu trả lời phủ định trong các trường hợp khác. 

Tuyên bố cũng cho phép tỷ lệ phần trăm lớn hơn 100. 

Ví dụ:```
10 10 150
```Sự tham dự cần thiết là`15`, mặc dù thành phố chỉ có 10 cư dân. Vì được phép nhân bản nên câu trả lời là`5`. Bất kỳ giải pháp nào giả định tỷ lệ phần trăm không bao giờ vượt quá 100 sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử đếm số lượng bản sao bắt đầu từ số 0. Đối với mỗi giá trị ứng cử viên`c`, kiểm tra xem`x + c`người tham dự đủ để tiếp cận ít nhất`y%`của dân số thành phố. hợp lệ đầu tiên`c`là câu trả lời. 

Điều này hiệu quả vì số lượng bản sao được kiểm tra theo thứ tự tăng dần, do đó giá trị thành công đầu tiên là tối thiểu. Với những ràng buộc đã cho, câu trả lời không bao giờ có thể vượt quá`10^4`, khiến cho việc mô phỏng lực lượng vũ phu này cũng đủ nhanh. 

Lý do khiến vấn đề trở nên đơn giản hơn là yêu cầu có thể được thể hiện trực tiếp dưới dạng số lượng người tham dự tối thiểu. 

Chúng tôi cần:$$x + c \ge \frac{n \cdot y}{100}$$Vì điểm danh phải là số nguyên nên yêu cầu thực sự là:$$x + c \ge \left\lceil \frac{n \cdot y}{100} \right\rceil$$Cho phép$$required = \left\lceil \frac{n \cdot y}{100} \right\rceil$$Khi đó số lượng bản sao nhỏ nhất chỉ đơn giản là:$$required - x$$trừ khi giá trị đó âm, trong trường hợp đó câu trả lời là 0. 

Nhiệm vụ duy nhất còn lại là tính giới hạn của phép chia số nguyên. Một công thức chuẩn là:$$\left\lceil \frac{a}{b} \right\rceil = \frac{a+b-1}{b}$$dùng phép chia số nguyên. 

Vì thế:```
required = (n * y + 99) // 100
answer = max(0, required - x)
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`x`, Và`y`. 
2. Tính số người đi học tối thiểu cần thiết để đáp ứng yêu cầu về tỷ lệ phần trăm:$$required = \left\lceil \frac{n \cdot y}{100} \right\rceil$$Vì tất cả các giá trị đều là số nguyên nên hãy sử dụng:```
required = (n * y + 99) // 100
```3. Tính toán cần thêm bao nhiêu người tham dự ngoài số lượng hiện tại`x`pháp sư:```
required - x
```4. Nếu giá trị này âm thì không cần sao chép. Trở lại:```
max(0, required - x)
```### Tại sao nó hoạt động 

Chính quyền chỉ quan tâm đến số lượng người tham dự cuối cùng so với quy mô dân số cố định`n`. Số lượng tham dự hợp lệ nhỏ nhất chính xác là mức trần của`n * y / 100`, bởi vì sự tham dự phải là một số nguyên. 

Bất kỳ sự tham dự nào nhỏ hơn`required`không đạt được điều kiện phần trăm, trong khi bất kỳ sự tham dự nào ít nhất`required`thành công. Từ`x`các pháp sư đã tham dự, số lượng bản sao tối thiểu chính xác là sự thiếu hụt giữa`required`Và`x`, với số 0 được sử dụng khi không có thâm hụt. Điều này trực tiếp mang lại câu trả lời tối thiểu duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x, y = map(int, input().split())

required = (n * y + 99) // 100
print(max(0, required - x))
```Dòng đầu tiên đọc ba giá trị đầu vào. 

Tính toán chính là:```
required = (n * y + 99) // 100
```Điều này thực hiện phép chia trần cho 100. Ví dụ: nếu`n * y = 140`, sau đó:```
(140 + 99) // 100 = 239 // 100 = 2
```tính toán chính xác`ceil(1.4)`. 

Câu trả lời cuối cùng là:```
max(0, required - x)
```Điều này xử lý cả hai tình huống. Nếu cần nhiều người tham dự hơn, sự khác biệt sẽ mang lại số lượng bản sao cần thiết. Nếu trình hướng dẫn hiện tại đã đáp ứng được mục tiêu thì kết quả sẽ trở thành âm và được thay thế bằng 0. 

Không có mối lo ngại tràn tồn tại bởi vì`n * y ≤ 10^8`, có thể dễ dàng xử lý bằng số nguyên Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 1 14
```| Biến | Giá trị | 
| --- | --- | 
| n | 10 | 
| x | 1 | 
| y | 14 | 
| n × y | 140 | 
| bắt buộc | (140 + 99) // 100 = 2 | 
| bắt buộc - x | 1 | 
| trả lời | 1 | 

Số lượng tham dự yêu cầu là 2 người vì 14% của 10 bằng 1,4 và phải làm tròn. Một thuật sĩ đã tham dự nên cần có một bản sao. 

### Mẫu 2 

đầu vào:```
10 10 100
```| Biến | Giá trị | 
| --- | --- | 
| n | 10 | 
| x | 10 | 
| y | 100 | 
| n × y | 1000 | 
| bắt buộc | (1000 + 99) // 100 = 10 | 
| bắt buộc - x | 0 | 
| trả lời | 0 | 

Tất cả mười pháp sư đều đã tham dự, đáp ứng đúng số lượng yêu cầu. Không có bản sao là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Thuật toán sử dụng thời gian không đổi và bộ nhớ không đổi bất kể giá trị đầu vào. Điều này thấp hơn nhiều so với giới hạn và dễ dàng phù hợp với nguồn lực được phép. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    n, x, y = map(int, input().split())
    required = (n * y + 99) // 100
    print(max(0, required - x))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("10 1 14\n") == "1\n", "sample 1"

# equivalent to the statement's second example
assert run("10 10 100\n") == "0\n", "sample 2"

# minimum-size input
assert run("1 1 1\n") == "0\n", "minimum values"

# off-by-one ceiling case
assert run("10 1 11\n") == "1\n", "11% of 10 requires 2 attendees"

# percentage above 100
assert run("10 10 150\n") == "5\n", "clones beyond population"

# maximum values
assert run("10000 10000 10000\n") == "990000\n", "largest inputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| Đầu vào pháp lý nhỏ nhất | 
|`10 1 11`|`1`| Làm tròn trần đúng cách | 
|`10 10 150`|`5`| Tỷ lệ phần trăm lớn hơn 100 | 
|`10000 10000 10000`|`990000`| Giá trị lớn nhất và câu trả lời lớn | 

## Vỏ cạnh 

Hãy xem xét:```
10 1 14
```Thuật toán tính toán:```
required = (10 * 14 + 99) // 100
         = 239 // 100
         = 2
```Câu trả lời trở thành:```
max(0, 2 - 1) = 1
```Điều này xử lý chính xác tỷ lệ phần trăm không nguyên. Cách tiếp cận phân chia tầng không chính xác sẽ chỉ yêu cầu một người tham dự. 

Bây giờ hãy xem xét:```
10 10 100
```Chúng tôi có được:```
required = 10
answer = max(0, 10 - 10) = 0
```Các trình hướng dẫn hiện có đã đáp ứng yêu cầu nên không có bản sao nào được tạo. 

Cuối cùng, hãy xem xét:```
10 10 150
```Việc tính toán cho:```
required = (1500 + 99) // 100
         = 15
answer = max(0, 15 - 10)
         = 5
```Thuật toán không giả định tỷ lệ phần trăm nhiều nhất là 100. Thuật toán xác định chính xác rằng cần có thêm năm bản sao để tiếp cận 15 người tham dự.
