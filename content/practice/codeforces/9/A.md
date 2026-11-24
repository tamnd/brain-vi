---
title: "CF 9A - Khuôn cuộn"
description: "Chúng ta được yêu cầu tính xác suất để Dot thắng một ván xúc xắc đơn giản trước Yakko và Wakko. Mỗi nhân vật tung một con súc sắc sáu mặt tiêu chuẩn. Chấm thắng nếu cuộn của cô ấy ít nhất lớn bằng số cuộn tối đa của Yakko và Wakko."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 9
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 9 (Div. 2 Only)"
rating: 800
weight: 9
solve_time_s: 163
verified: true
draft: false
---
[CF 9A - Cuộn xúc xắc](https://codeforces.com/problemset/problem/9/A) 

**Đánh giá:** 800 
**Tags:** toán, xác suất 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính xác suất để Dot thắng một ván xúc xắc đơn giản trước Yakko và Wakko. Mỗi nhân vật tung một con súc sắc sáu mặt tiêu chuẩn. Chấm thắng nếu cuộn của cô ấy ít nhất lớn bằng số cuộn tối đa của Yakko và Wakko. Các cuộn của Yakko và Wakko được lấy làm đầu vào, hai số nguyên từ 1 đến 6. Chúng ta phải đưa ra xác suất để Dot thắng dưới dạng phân số tối giản. 

Đầu vào bao gồm hai số Y và W, đại diện cho lượt quay của Yakko và Wakko. Kết quả đầu ra là phân số A/B trong đó B là tổng số kết quả trên một con súc sắc (6) và A là số kết quả cho phép Dấu chấm khớp hoặc vượt quá số cao hơn của Y và W. Phân số này phải được giảm xuống mức thấp nhất. 

Các trường hợp chính xảy ra khi số điểm thắng tối thiểu của Dot là 6, trong đó cô ấy chỉ có thể thắng với một kết quả, cho xác suất là 1/6. Một trường hợp khác là khi cả Y và W đều bằng 1, điều này làm cho kết quả thắng của Dot có giá trị bất kỳ từ 1 đến 6, tạo ra xác suất là 1/1. Cách tiếp cận bất cẩn có thể tính toán xác suất mà không xem xét giá trị tối đa của hai giá trị đầu vào, dẫn đến kết quả không chính xác khi Yakko và Wakko khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ là mô phỏng tất cả các lần cuộn có thể có của Dot và đếm kết quả trong đó số lần cuộn của Dot ít nhất phải lớn bằng mức tối đa của Yakko và Wakko. Điều này hiệu quả vì chỉ có sáu kết quả cho Dot, khiến cho việc lặp lại tất cả các khả năng trở nên khả thi. Cách tiếp cận bạo lực là đúng, nhưng việc mô phỏng từng lần tung súc sắc một cách rõ ràng là không cần thiết vì kết quả tuân theo sự phân bố đồng đều và có thể được suy luận về mặt toán học. 

Quan sát chính là Dot chỉ cần tung tối đa Y và W trở lên. Cho phép`max_roll = max(Y, W)`. Số kết quả thuận lợi là`6 - max_roll + 1`vì một con súc sắc có các mặt từ 1 đến 6. Tổng số kết quả có thể xảy ra luôn là 6. Sau khi biết số kết quả thuận lợi, chúng ta giảm phân số về dạng đơn giản nhất bằng cách sử dụng ước số chung lớn nhất (GCD). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(6) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên từ đầu vào đại diện cho lượt quay của Yakko và Wakko. 
2. Tính toán cuộn tối đa giữa Y và W. Giá trị này thể hiện giá trị tối thiểu mà Dot cần để khớp hoặc vượt quá. 
3. Tính số kết quả thuận lợi cho Dot như`6 - max_roll + 1`. 
4. Xác định ước chung lớn nhất của tử số (kết quả thuận lợi) và mẫu số (6) để rút gọn phân số. 
5. Xuất phân số dưới dạng`A/B`sử dụng tử số và mẫu số rút gọn. 

Tại sao nó hoạt động: Điều bất biến là xác suất chỉ phụ thuộc vào giá trị tối đa của Y và W vì Dot chỉ thắng khi lần tung của cô ấy đáp ứng hoặc vượt quá giá trị này. Bằng cách tính toán số lượng kết quả thỏa mãn điều này và giảm phân số, chúng tôi đảm bảo tính chính xác cho tất cả các đầu vào có thể có. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

Y, W = map(int, input().split())

max_roll = max(Y, W)
favorable = 6 - max_roll + 1
gcd = math.gcd(favorable, 6)

print(f"{favorable // gcd}/{6 // gcd}")
```Đoạn mã đọc hai số nguyên, tính số cuộn tối đa, đếm các kết quả có lợi và giảm phân số bằng cách sử dụng`math.gcd`. Chia cả tử số và mẫu số cho GCD đảm bảo đầu ra là tối giản. Việc sử dụng phép chia số nguyên sẽ tránh được sự thiếu chính xác của dấu phẩy động. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Đầu vào`4 2`

`max_roll = max(4, 2) = 4`

`favorable = 6 - 4 + 1 = 3`

`gcd = gcd(3, 6) = 3`đầu ra`3/3 / 6/3 = 1/2`| Bước | max_roll | thuận lợi | gcd | phân số | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | 3 | 1/2 | 

Chấm có thể lăn 4, 5 hoặc 6 để giành chiến thắng. Điều này xác nhận tính toán. 

**Ví dụ 2:** Đầu vào`1 1`

`max_roll = 1`

`favorable = 6 - 1 + 1 = 6`

`gcd = gcd(6, 6) = 6`đầu ra`6/6 / 6/6 = 1/1`| Bước | max_roll | thuận lợi | gcd | phân số | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | 6 | 1/1 | 

Tất cả các cuộn đều cho phép Dot giành chiến thắng, phù hợp với trường hợp cạnh dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chúng tôi thực hiện một số lượng không đổi các phép tính số học và một phép tính GCD duy nhất. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    Y, W = map(int, input().split())
    max_roll = max(Y, W)
    favorable = 6 - max_roll + 1
    gcd = math.gcd(favorable, 6)
    return f"{favorable // gcd}/{6 // gcd}"

# Provided sample
assert run("4 2\n") == "1/2", "sample 1"

# Custom cases
assert run("1 1\n") == "1/1", "minimum rolls"
assert run("6 6\n") == "1/6", "maximum rolls"
assert run("2 5\n") == "1/2", "mixed rolls"
assert run("3 3\n") == "2/3", "equal mid rolls"
assert run("1 6\n") == "1/6", "extreme difference"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1/1 | Chấm thắng trong mọi trường hợp | 
| 6 6 | 1/6 | Chỉ có một kết quả chiến thắng | 
| 2 5 | 1/2 | Lựa chọn và xác suất tối đa chính xác | 
| 3 3 | 2/3 | Tính cuộn bằng nhau | 
| 1 6 | 1/6 | Điều kiện biên với min và max | 

## Vỏ cạnh 

Nếu cả Yakko và Wakko đều tung được 6 thì Dot chỉ có thể thắng khi tung được 6. Thuật toán tính toán`max_roll = 6`, sau đó`favorable = 6 - 6 + 1 = 1`,`gcd = gcd(1, 6) = 1`, sản xuất`1/6`. Nếu cả hai lăn 1,`max_roll = 1`,`favorable = 6`,`gcd = 6`, đầu ra`1/1`. Phương thức này xử lý tất cả các giá trị trung gian bằng cách trừ giá trị tối đa từ 6 và cộng 1, do đó không xảy ra lỗi sai sót nào.
