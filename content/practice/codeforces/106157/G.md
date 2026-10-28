---
title: "CF 106157G - Nhận Tốt"
description: "Charlie có n ngày trước cuộc thi. Khi còn mới, anh ta nhận được điểm kỹ năng mỗi ngày, nhưng chỉ trong x ngày làm việc liên tiếp đầu tiên sau khi thiết lập lại. Nếu anh ta tiếp tục làm việc quá x ngày đó, anh ta sẽ trở nên mệt mỏi và chỉ nhận được b điểm kỹ năng mỗi ngày, trong đó a ≥ b."
date: "2026-06-25T11:19:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "G"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 51
verified: true
draft: false
---

[CF 106157G - Làm tốt](https://codeforces.com/problemset/problem/106157/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Charlie có`n`ngày trước cuộc thi. 

Khi anh ta còn tươi, anh ta đạt được`a`điểm kỹ năng mỗi ngày, nhưng chỉ cho lần đầu tiên`x`ngày làm việc liên tiếp sau khi thiết lập lại. Nếu anh ấy tiếp tục làm việc ngoài những điều đó`x`ngày, anh ta trở nên mệt mỏi và chỉ đạt được`b`điểm kỹ năng mỗi ngày, ở đâu`a ≥ b`. 

Bất cứ lúc nào anh ấy cũng có thể bắt đầu nghỉ ngơi. Một kỳ nghỉ kéo dài chính xác`y`ngày liên tiếp và không có điểm kỹ năng. Sau khi giờ nghỉ kết thúc, Charlie trở lại tươi tắn và một lần nữa có thể tận hưởng`x`ngày đạt được`a`. Mục tiêu là tối đa hóa tổng số điểm kỹ năng thu thập được trong khoảng thời gian có sẵn.`n`ngày. 

Những hạn chế là phần then chốt của vấn đề. Cả hai`n`và các tham số khác có thể lớn bằng`10^9`, và có thể có tới`10^4`trường hợp thử nghiệm. Bất kỳ mô phỏng hàng ngày là không thể. Giải pháp phải dựa trên tối ưu hóa dạng đóng chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là việc nghỉ giải lao không phải lúc nào cũng hữu ích. Nếu việc nghỉ ngơi khiến bạn tốn nhiều kỹ năng hơn so với thời gian mới thu được, thì chiến lược tối ưu có thể là không bao giờ nghỉ ngơi chút nào. 

Một sai lầm dễ mắc phải khác là cho rằng mỗi kỳ kinh mới phải luôn được sử dụng đầy đủ. Gần hết lịch trình, có thể không còn đủ số ngày để tiêu hết`x`ngày mới nên khâu cuối cùng phải được xử lý cẩn thận. 

Coi như:```
n = 5, a = 4, b = 1, x = 3, y = 1
```Làm việc cả năm ngày mang lại:```
4 + 4 + 4 + 1 + 1 = 14
```Nghỉ ngơi sau ngày thứ 3 mang lại:```
4 + 4 + 4 + 0 + 4 = 16
```Sự nghỉ ngơi đáng giá vì nó đánh đổi một ngày mệt mỏi để lấy một ngày tươi mới khác. 

Bây giờ hãy xem xét:```
n = 10, a = 6, b = 5, x = 1, y = 5
```Việc nghỉ ngơi tiêu tốn năm ngày chỉ để phục hồi một ngày có giá trị cao. Tiếp tục làm việc thì tốt hơn nhiều. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là thử mọi vị trí nghỉ giải lao có thể. Mỗi lịch trình bao gồm các phân đoạn làm việc được phân tách bằng giờ nghỉ. Độ lợi của một đoạn có chiều dài`L`là```
a * min(L, x) + b * max(0, L - x)
```Công thức này đúng, nhưng số lượng vị trí ngắt có thể tăng theo cấp số nhân và trở nên vô vọng ngay cả đối với các giá trị vừa phải của`n`. 

Điều quan trọng cần lưu ý là chỉ có số lần nghỉ giải lao mới quan trọng. 

Giả sử chúng ta lấy chính xác`m`nghỉ giải lao. 

Những giờ nghỉ đó tiêu tốn`m * y`ngày, rời đi```
W = n - m * y
```ngày làm việc. 

Tổng số tiền thưởng trên mức mệt mỏi đến từ những ngày mới. Mỗi ngày làm việc ít nhất có giá trị`b`. Một ngày tươi mới góp phần thêm```
a - b
```thưởng. 

Với`m`nghỉ giải lao, có`m + 1`các phân đoạn làm việc. Mỗi phân khúc có thể đóng góp nhiều nhất`x`ngày tươi nên tổng số ngày tươi không thể vượt quá```
(m + 1) * x
```và cũng không thể vượt quá số ngày làm việc`W`. 

Kể từ đây:```
fresh_days = min(W, (m + 1) * x)
```Tổng lợi nhuận trở thành```
b * W + (a - b) * fresh_days
```hoặc```
G(m) = b * (n - m y)
     + (a - b) * min(n - m y, (m + 1) x)
```Bây giờ vấn đề đã trở thành tối ưu hóa một chiều. 

Có hai khu vực. 

Khi```
n - m y > (m + 1) x
```tất cả các khe mới đều được sử dụng đầy đủ. Sau đó```
G(m)
= b n + (a - b) x + m((a - b)x - by)
```đó là tuyến tính trong`m`. 

Khi```
n - m y ≤ (m + 1) x
```mỗi ngày làm việc đều tươi mới, vì vậy```
G(m) = a(n - m y)
```giảm dần khi`m`tăng lên. 

Hàm tuyến tính theo sau là hàm giảm có nghĩa là điều tối ưu chỉ có thể xảy ra tại một trong các điểm biên. Chúng ta có thể tính toán chúng một cách trực tiếp trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt quá lịch trình | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu hóa dạng đóng | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`n ≤ x`, mỗi ngày có sẵn có thể được sử dụng trong khoảng thời gian mới ban đầu. Câu trả lời đơn giản là:```
a * n
```2. Tính toán```
m0 = ceil((n - x) / (x + y))
```Đây là số lần nghỉ giải lao nhỏ nhất mà mỗi ngày làm việc còn lại có thể được nghỉ ngơi. 
3. Đánh giá ứng viên đầu tiên:```
G2 = a * (n - m0 * y)
```Điều này tương ứng với khu vực thứ hai nơi tất cả các ngày làm việc đều mới. 
4. Tính toán```
c = (a - b) * x - b * y
```Đây là lợi nhuận thu được bằng cách thêm một chu trình hoàn chỉnh nữa bao gồm`x`những ngày tươi mới và`y`ngày nghỉ. 
5. Nếu`c ≤ 0`, vùng tuyến tính đang giảm dần, vì vậy điểm tốt nhất của nó là`m = 0`. 
6. Nếu`c > 0`, vùng tuyến tính đang tăng lên, vì vậy điểm tốt nhất của nó là giá trị hợp lệ lớn nhất`m`trước khi chuyển sang khu vực thứ hai:```
m1 = m0 - 1
```7. Đánh giá```
G1 = G(m1)
```hoặc`G(0)`khi`c ≤ 0`. 
8. Câu trả lời là```
max(G1, G2)
```### Tại sao nó hoạt động 

Đối với số lần nghỉ cố định, sự sắp xếp tối ưu được xác định hoàn toàn bằng số lượng phân đoạn làm việc tồn tại. Mỗi phân khúc đóng góp nhiều nhất`x`ngày tươi nên tổng số ngày tươi chính xác là`min(W, (m + 1)x)`. 

Điều này làm giảm toàn bộ vấn đề thành một hàm của một số nguyên`m`. Hàm đó là tuyến tính trong khi dung lượng mới đã bão hòa hoàn toàn, sau đó trở thành đường giảm dần khi mỗi ngày làm việc đều đã có dung lượng mới. Vì hàm số chỉ có một điểm chuyển tiếp nên giá trị lớn nhất phải xảy ra ở điểm tốt nhất của dòng đầu tiên hoặc tại điểm đầu tiên của dòng thứ hai. Đánh giá những ứng viên đó là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, a, b, x, y = map(int, input().split())

        if n <= x:
            print(a * n)
            continue

        m0 = (n - x + (x + y) - 1) // (x + y)

        ans = a * (n - m0 * y)

        c = (a - b) * x - b * y

        if c <= 0:
            m = 0
        else:
            m = m0 - 1

        w = n - m * y
        g = b * w + (a - b) * min(w, (m + 1) * x)

        ans = max(ans, g)

        print(ans)

solve()
```Nhánh đầu tiên xử lý tình huống dễ dàng khi khoảng thời gian mới ban đầu đã bao gồm tất cả các ngày có sẵn.`m0`là số lần nghỉ đầu tiên mà tổng công suất mới trở nên đủ lớn để đáp ứng mỗi ngày làm việc. Tính toán nó với phép chia trần là phần phức tạp nhất trong quá trình thực hiện. 

Biến`c`đo lường xem việc thêm một chu kỳ mới đầy đủ khác có mang lại lợi nhuận hay không. Nếu không mang lại lợi nhuận, điểm tốt nhất trong vùng tuyến tính là ngay tại`m = 0`. Mặt khác, vùng tuyến tính tăng đến ranh giới của nó, vì vậy chúng tôi đánh giá điểm hợp lệ cuối cùng trước khi chuyển đổi. 

Tất cả số học đều phù hợp thoải mái với số nguyên 64 bit, nhưng số nguyên có độ chính xác tùy ý của Python thậm chí còn loại bỏ mối lo ngại đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 1 3 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 5 | 
| một | 4 | 
| b | 1 | 
| x | 3 | 
| y | 1 | 

Tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| m0 | 1 | 
| G2 | 4 × (5 - 1) = 16 | 
| c | (4 - 1) × 3 - 1 × 1 = 8 | 
| m1 | 0 | 
| G1 | 14 | 

Trả lời:```
16
```Điều này chứng tỏ một trường hợp nghỉ ngơi là có lợi. 

### Ví dụ 2 

đầu vào:```
10 6 5 1 5
```| Bước | Giá trị | 
| --- | --- | 
| m0 | 2 | 
| G2 | 0 | 
| c | (6 - 5) × 1 - 5 × 5 = -24 | 
| m | 0 | 
| G1 | 51 | 

Trả lời:```
51
```Điều này cho thấy tình huống mà thời gian nghỉ giải lao quá tốn kém và chiến lược tối ưu là làm việc liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Không có công trình phụ trợ | 

Ngay cả với`10^4`trường hợp thử nghiệm và các tham số lên đến`10^9`, điều này diễn ra thoải mái trong giới hạn vì mọi trường hợp thử nghiệm đều được giải bằng cách sử dụng số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
# helper reference implementation

def solve_case(n, a, b, x, y):
    if n <= x:
        return a * n

    m0 = (n - x + (x + y) - 1) // (x + y)

    ans = a * (n - m0 * y)

    c = (a - b) * x - b * y

    if c <= 0:
        m = 0
    else:
        m = m0 - 1

    w = n - m * y
    g = b * w + (a - b) * min(w, (m + 1) * x)

    return max(ans, g)

assert solve_case(2, 2, 2, 2, 2) == 4
assert solve_case(4, 4, 2, 3, 3) == 14
assert solve_case(5, 3, 2, 3, 1) == 13
assert solve_case(5, 4, 1, 3, 1) == 16

assert solve_case(1, 10, 5, 7, 3) == 10
assert solve_case(10, 5, 5, 3, 2) == 50
assert solve_case(10, 6, 5, 1, 5) == 51
assert solve_case(1000000000, 1000000000, 1, 1000000000, 1) == 1000000000000000000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10 5 7 3`|`10`| Lịch trình có độ dài tối thiểu | 
|`10 5 5 3 2`|`50`| Tỷ lệ bằng nhau, nghỉ giải lao không bao giờ giúp ích | 
|`10 6 5 1 5`|`51`| Lợi nhuận chu kỳ âm | 
| Giá trị lớn |`10^18`| Số học tỷ lệ 64-bit | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 10 5 7 3
```Từ`n ≤ x`, Charlie không bao giờ cảm thấy mệt mỏi. Thuật toán ngay lập tức trả về:```
10 × 1 = 10
```Không cần logic liên quan đến ngắt. 

Coi như:```
1
10 5 5 3 2
```Những ngày tươi mới và mệt mỏi đều có giá trị như nhau. Lợi nhuận của chu kỳ là âm vì nghỉ giải lao chỉ lãng phí thời gian. Thuật toán chọn`m = 0`, sản xuất:```
10 × 5 = 50
```đó là tối ưu. 

Coi như:```
1
5 4 1 3 1
```Điểm chuyển tiếp xảy ra chính xác tại một điểm dừng. Thuật toán đánh giá cả hai phía của ranh giới:```
G(0) = 14
G(1) = 16
```và chọn`16`, xử lý chính xác thời điểm mà chiến lược tối ưu thay đổi từ hoạt động liên tục sang sử dụng thiết lập lại.
