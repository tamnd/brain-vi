---
title: "CF 1551A - Polycarp và tiền xu"
description: "Polycarp cần thanh toán chính xác n số tiền chỉ bằng cách sử dụng đồng xu có giá trị 1 số tiền và 2 số tiền. Gọi c1 là số đồng xu 1-burle và c2 là số đồng xu 2-burle."
date: "2026-06-10T13:17:14+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1551
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 734 (Div. 3)"
rating: 800
weight: 1551
solve_time_s: 200
verified: true
draft: false
---

[CF 1551A - Polycarp và tiền xu](https://codeforces.com/problemset/problem/1551/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Polycarp cần thanh toán chính xác`n`burles chỉ sử dụng đồng xu có giá trị 1 burle và 2 burles. Cho phép`c1`là số đồng xu 1-burle và`c2`là số đồng xu 2-burle. 

Việc thanh toán phải đáp ứng:$$c_1 + 2c_2 = n$$Trong số tất cả các cách hợp lệ để tính tổng`n`, chúng tôi muốn số lượng của hai loại tiền xu được cân bằng nhất có thể. Nói cách khác, chúng ta phải giảm thiểu:$$|c_1 - c_2|$$Đối với mỗi trường hợp thử nghiệm, chúng ta cần xuất ra một cặp tối ưu`(c1, c2)`. 

Số lượng ca kiểm thử có thể lên tới 10.000 và`n`có thể lớn như`10^9`. Bất kỳ giải pháp nào thử nghiệm nhiều khả năng cho một trường hợp thử nghiệm sẽ ngay lập tức bị nghi ngờ. Thậm chí quét tuyến tính lên đến`n`sẽ yêu cầu tới một tỷ lần lặp lại cho một trường hợp thử nghiệm, vượt xa giới hạn thời gian. Chúng ta cần một công thức toán học trực tiếp để tính ra câu trả lời trong thời gian không đổi. 

Một số trường hợp khó xử lý sai nếu chúng ta chỉ nghĩ về phương trình mà quên mất yêu cầu cân bằng. 

Coi như`n = 1`.```
1
```Khoản thanh toán hợp lệ duy nhất là:```
1 0
```Một công thức chia số tiền thành các phần bằng nhau một cách mù quáng có thể vô tình tạo ra các giá trị âm hoặc không đáp ứng được tổng. 

Coi như`n = 2`.```
2
```Câu trả lời đúng là:```
0 1
```Sự khác biệt là 1. Không thể đếm bằng nhau vì một đồng xu đóng góp 1 và đồng xu kia đóng góp 2. 

Hãy xem xét`n = 5`.```
5
```Câu trả lời tối ưu là:```
1 2
```Một cách tiếp cận bất cẩn có thể chọn`(3,1)`bởi vì nó cũng có tổng bằng 5, nhưng hiệu của nó là 2 thay vì 1. 

Những trường hợp thú vị xảy ra khi`n`không chia hết cho 3. Số dư đó xác định loại xu nào nhận được thêm xu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đơn giản sẽ thử mọi giá trị có thể có của`c2`từ 0 đến`n/2`. Với mỗi lựa chọn, chúng ta có thể tính:$$c_1 = n - 2c_2$$và đánh giá`|c1 - c2|`. Cặp có sự khác biệt nhỏ nhất là câu trả lời. 

Điều này có tác dụng vì mọi khoản thanh toán hợp lệ đều tương ứng với chính xác một lựa chọn`c2`, vì vậy việc kiểm tra tất cả các khả năng đảm bảo tìm được phương án tối ưu. 

Vấn đề là quy mô. Khi`n = 10^9`, vòng lặp sẽ thực hiện khoảng 500 triệu lần lặp cho một trường hợp thử nghiệm. Với tối đa 10.000 trường hợp thử nghiệm, thời gian chạy trở nên hoàn toàn không khả thi. 

Quan sát chính xuất phát từ việc xem xét phương trình:$$c_1 + 2c_2 = n$$Nếu như`c1`Và`c2`hoàn toàn bằng nhau, nói cả hai bằng nhau`x`, sau đó:$$x + 2x = 3x = n$$Vì vậy, giải pháp cân bằng hoàn hảo sẽ chia`n`thành ba phần bằng nhau. Điều này ngay lập tức gợi ý rằng cả hai số đếm phải càng gần càng tốt với`n/3`. 

Cho phép:$$n = 3k + r$$Ở đâu`r`là 0, 1 hoặc 2. 

Khi nào`r = 0`, sự phân chia là hoàn hảo:$$c_1 = k,\quad c_2 = k$$Khi`r = 1`, chúng ta cần thêm một điều nữa`3k`. Tặng thêm một đồng xu 1 burle sẽ tăng tổng số lên đúng 1 trong khi vẫn giữ số lượng cân bằng nhất có thể:$$c_1 = k+1,\quad c_2 = k$$Khi`r = 2`, chúng ta cần thêm hai chiếc burles nữa`3k`. Tặng thêm một đồng xu 2 burle sẽ tăng tổng số lên đúng 2:$$c_1 = k,\quad c_2 = k+1$$Điều này mang lại một giải pháp trực tiếp theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của`n`. 
2. Tính toán:$$k = n // 3$$Đây là số tiền cơ bản cân bằng vì một đồng xu 1 burle và một đồng xu 2 burle cùng nhau đóng góp 3 burle. 
3. Tính toán:$$r = n \bmod 3$$Phần còn lại cho chúng ta biết còn lại bao nhiêu chốt sau khi tạo thành phần đế cân bằng. 
4. Nếu`r = 0`, đầu ra:$$c_1 = k,\quad c_2 = k$$Số tiền được chia chính xác thành số lượng bằng nhau. 
5. Nếu`r = 1`, đầu ra:$$c_1 = k+1,\quad c_2 = k$$Thêm một đồng xu 1-burle bổ sung sẽ tăng tổng số lên đúng 1 và giữ mức chênh lệch ở mức tối thiểu. 
6. Nếu`r = 2`, đầu ra:$$c_1 = k,\quad c_2 = k+1$$Thêm một đồng xu 2-burle bổ sung sẽ tăng tổng số lên đúng 2 và giữ mức chênh lệch ở mức tối thiểu. 

### Tại sao nó hoạt động 

Phương trình tự nhiên nhóm các đồng xu thành từng cặp bao gồm một đồng xu 1-burle và một đồng xu 2-burle. Mỗi cặp cân bằng như vậy đóng góp 3 trọng lượng. Nếu như`n = 3k`, sau đó`k`các cặp đã tạo thành số lượng chính xác và số lượng bằng nhau. 

Khi số dư là 1, cách duy nhất để thêm chính xác một burle nữa là tăng số lượng đồng xu 1 burle lên một. Khi số còn lại là 2, cách duy nhất để thêm chính xác hai đồng xu nữa trong khi vẫn gần số dư nhất là tăng số lượng đồng xu 2 đồng lên một. 

Kết quả đếm luôn thỏa mãn`c1 + 2*c2 = n`và bất kỳ giải pháp hợp lệ nào khác sẽ di chuyển số đếm ra xa nhau hơn. Do đó cặp được tạo ra giảm thiểu`|c1 - c2|`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())

    k = n // 3
    r = n % 3

    if r == 0:
        c1, c2 = k, k
    elif r == 1:
        c1, c2 = k + 1, k
    else:
        c1, c2 = k, k + 1

    print(c1, c2)
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

Biến`k`đại diện cho sự phân phối cơ sở cân bằng. Mỗi nhóm 3 burles có thể được biểu diễn bằng một đồng xu của mỗi loại, vì vậy`n // 3`đưa ra số lượng tối đa các nhóm cân bằng như vậy. 

Phần còn lại xác định giá trị bổ sung phải đi đâu. Phần còn lại của 1 chỉ có thể được cung cấp bởi một đồng xu 1 burle bổ sung. Phần còn lại của 2 có thể được cung cấp bởi một đồng xu 2 burle bổ sung. 

Không có lo ngại về tràn vì số nguyên Python xử lý các giá trị lớn hơn nhiều so với`10^9`. Điểm tinh tế duy nhất là gán đồng xu phụ vào đúng mệnh giá theo phần còn lại. Hoán đổi những trường hợp đó vẫn thỏa mãn tổng nhưng sẽ không giảm thiểu sự khác biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

| Bước | Giá trị | 
| --- | --- | 
| n | 5 | 
| k = n // 3 | 1 | 
| r = n% 3 | 2 | 
| c1 | 1 | 
| c2 | 2 | 

Xác minh:$$1 + 2 \cdot 2 = 5$$Sự khác biệt:$$|1 - 2| = 1$$Ví dụ này chứng tỏ`r = 2`trường hợp. Giá trị bổ sung được cung cấp bởi một đồng xu 2-burle bổ sung. 

### Ví dụ 2: n = 1000 

| Bước | Giá trị | 
| --- | --- | 
| n | 1000 | 
| k = n // 3 | 333 | 
| r = n% 3 | 1 | 
| c1 | 334 | 
| c2 | 333 | 

Xác minh:$$334 + 2 \cdot 333 = 1000$$Sự khác biệt:$$|334 - 333| = 1$$Ví dụ này chứng tỏ`r = 1`trường hợp. Burle bổ sung được cung cấp bởi một đồng xu 1 burle bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Ngay cả với 10.000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        k = n // 3
        r = n % 3

        if r == 0:
            ans.append(f"{k} {k}")
        elif r == 1:
            ans.append(f"{k + 1} {k}")
        else:
            ans.append(f"{k} {k + 1}")

    return "\n".join(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""6
1000
30
1
32
1000000000
5
"""
) == (
"""334 333
10 10
1 0
10 11
333333334 333333333
1 2"""
), "sample 1"

# minimum value
assert run(
"""1
1
"""
) == "1 0", "minimum n"

# divisible by 3
assert run(
"""1
6
"""
) == "2 2", "equal counts"

# remainder 1
assert run(
"""1
4
"""
) == "2 1", "remainder 1 case"

# remainder 2
assert run(
"""1
2
"""
) == "0 1", "remainder 2 case"

# maximum value
assert run(
"""1
1000000000
"""
) == "333333334 333333333", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1 0`| Số tiền nhỏ nhất có thể | 
|`6`|`2 2`| Vỏ cân bằng hoàn hảo | 
|`4`|`2 1`| Xử lý còn lại 1 | 
|`2`|`0 1`| Xử lý còn lại 2 | 
|`1000000000`|`333333334 333333333`| Giá trị được phép lớn nhất | 

## Vỏ cạnh 

### Trường hợp cạnh 1: Số tiền nhỏ nhất 

đầu vào:```
1
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| k | 0 | 
| r | 1 | 
| c1 | 1 | 
| c2 | 0 | 

Đầu ra là:```
1 0
```Số tiền là chính xác và không có khoản thanh toán hợp lệ nào khác. Thuật toán xử lý trường hợp ranh giới này một cách tự nhiên mà không cần logic đặc biệt. 

### Trường hợp Edge 2: Số tiền bằng 2 

đầu vào:```
2
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| k | 0 | 
| r | 2 | 
| c1 | 0 | 
| c2 | 1 | 

Đầu ra là:```
0 1
```Khoản thanh toán chính xác bằng 2 và chênh lệch là 1. Bất kỳ nỗ lực nào để thêm giá trị bổ sung vào`c1`sẽ không đạt được tổng số yêu cầu. 

### Trường hợp cạnh 3: bội số của 3 

đầu vào:```
30
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| k | 10 | 
| r | 0 | 
| c1 | 10 | 
| c2 | 10 | 

Đầu ra là:```
10 10
```Số lượng hoàn toàn bằng nhau, đó là sự khác biệt nhỏ nhất có thể. 

### Trường hợp cạnh 4: Giá trị lớn có số dư 1 

đầu vào:```
1000000000
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| k | 333333333 | 
| r | 1 | 
| c1 | 333333334 | 
| c2 | 333333333 | 

Đầu ra là:```
333333334 333333333
```Số lượng chỉ khác nhau một, đó là tối ưu. Việc tính toán chỉ sử dụng phép chia số nguyên và modulo, do đó các giá trị lớn được xử lý dễ dàng như các giá trị nhỏ.
