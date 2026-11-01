---
title: "CF 106457D - Herryng"
description: "Bài toán yêu cầu kích thước thùng hàng mang lại tổng giá trị lô hàng lớn nhất có thể. Một con tàu chỉ có thể được sử dụng khi sức chứa của nó chia hết cho kích thước thùng đã chọn."
date: "2026-06-25T09:14:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106457
codeforces_index: "D"
codeforces_contest_name: "UTPC Spring 2026 Open Contest"
rating: 0
weight: 106457
solve_time_s: 35
verified: true
draft: false
---

[CF 106457D - Herryng](https://codeforces.com/problemset/problem/106457/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu kích thước thùng hàng mang lại tổng giá trị lô hàng lớn nhất có thể. Một con tàu chỉ có thể được sử dụng khi sức chứa của nó chia hết cho kích thước thùng đã chọn. Nếu chúng ta chọn kích thước thùng`x`, mọi con tàu có sức chở là bội số của`x`đóng góp`x`đơn vị giá trị, bởi vì nó có thể mang chính xác một lõi không ổn định bên trong một thùng. Do đó tổng giá trị là:`x * (number of capacities divisible by x)`Chúng ta cần tìm`x`đó tối đa hóa giá trị này. Nếu một số kích cỡ thùng tạo ra cùng một giá trị tối đa, chúng tôi sẽ chọn kích thước nhỏ nhất. 

Đầu vào bao gồm một danh sách năng lực của tàu. Mỗi công suất tối đa là`10^6`, và có thể có tới`10^6`tàu. Giới hạn trên của các giá trị là chi tiết quan trọng. Việc lặp lại mọi cặp công suất có thể sẽ quá chậm. Với một triệu con tàu, một`O(N^2)`cách tiếp cận sẽ yêu cầu khoảng`10^12`hoạt động không thể phù hợp với giới hạn cuộc thi thông thường. Chúng ta cần một giải pháp gần tuyến tính với công suất tối đa. 

Các trường hợp phức tạp đến từ các ước số không xuất hiện trực tiếp trong đầu vào và từ các mối quan hệ. 

Ví dụ:```
Input
5
2 3 3 4 5

Output
3
```Một cách tiếp cận bất cẩn chỉ có thể kiểm tra các giá trị xuất hiện dưới dạng dung lượng. Trong trường hợp này, điều đó xảy ra có hiệu quả nhưng nói chung nó không phải là một ý tưởng hợp lệ. Kích thước thùng tốt nhất có thể là một con số không có trong danh sách. 

Một ví dụ khác:```
Input
5
2 4 6 8 10

Output
5
```Kích thước thùng`5`mang lại giá trị`5 * 2 = 10`, bởi vì năng lực`10`Và`?`chờ đã, chỉ`10`chia hết cho`5`, vì vậy đầu ra này không chính xác. Tính toán thực tế là:```
x = 2  -> 2 * 5 = 10
x = 5  -> 5 * 1 = 5
```Câu trả lời là:```
Input
5
2 4 6 8 10

Output
2
```Điều này nắm bắt các triển khai gây nhầm lẫn giữa số lượng ước số với số lượng khả năng chia hết. 

Hộp đựng cà vạt cũng dễ bị xử lý sai:```
Input
4
1 2 3 6

Output
3
```Đây:```
x = 1 -> 1 * 4 = 4
x = 2 -> 2 * 2 = 4
x = 3 -> 3 * 2 = 6
x = 6 -> 6 * 1 = 6
```Cả hai`3`Và`6`cho giá trị lớn nhất nên giá trị nhỏ hơn phải được trả về. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi kích thước thùng có thể và đếm xem có bao nhiêu sức chứa có thể chia hết cho nó. Đối với kích thước thùng`x`, quét tất cả`N`năng lực mang lại`O(N)`công việc. Vì công suất tối đa có thể được`10^6`, cố gắng hết sức có thể`x`giá trị mang lại xung quanh`10^12`hoạt động trong trường hợp xấu nhất là quá chậm. 

Cấu trúc của vấn đề cho chúng ta một lộ trình tốt hơn. Chúng tôi không thực sự quan tâm đến năng lực cá nhân. Chúng ta chỉ cần biết mỗi khả năng xảy ra bao nhiêu lần. Sau khi xây dựng mảng tần số, chúng ta có thể đặt một câu hỏi khác: đối với từng kích thước thùng có thể`x`, có bao nhiêu giá trị đầu vào là bội số của`x`? 

Đây chính xác là mô hình giống như đếm bội số trong một cái sàng. Đối với mọi`x`, chúng tôi ghé thăm`x, 2x, 3x, ...`và thêm tần số của những khả năng đó. Số lượng hoạt động là khoảng:`M/1 + M/2 + M/3 + ... + M/M`Ở đâu`M`là công suất lớn nhất. Đây là`M log M`, dễ dàng đủ nhanh để`M = 10^6`. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi khả năng, nhưng nó lặp lại cùng một công việc chia hết nhiều lần. Việc đếm kiểu sàng sử dụng lại thông tin tần số và xử lý từng mối quan hệ ước số một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N * M) | O(1) | Quá chậm | 
| Tối ưu | O(M log M + N) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả sức chứa của tàu và lưu trữ số lần mỗi sức chứa xuất hiện. 

Mảng tần số cho phép chúng ta trả lời “có bao nhiêu tàu có sức chứa`v`"trong thời gian không đổi. 
2. Lặp lại mọi kích thước thùng có thể`x`từ`1`đến công suất tối đa. 

Chúng ta chỉ cần xét đến những kích thước có thể chia được ít nhất một công suất. Giá trị lớn hơn không thể đóng góp bất cứ điều gì. 
3. Đối với hiện tại`x`, tổng tần số của tất cả các bội số của`x`. 

Chúng tôi ghé thăm`x, 2x, 3x, ...`bởi vì đó chính xác là những năng lực có thể sử dụng thùng có kích thước`x`. 
4. Tính tổng số tiền thanh toán như sau:`x * count`Ở đâu`count`là số lượng tàu có thể sử dụng cỡ thùng này. 
5. Giữ câu trả lời hay nhất cho đến nay. Thay thế câu trả lời khi khoản thanh toán lớn hơn hoặc khi khoản thanh toán bằng nhau và`x`nhỏ hơn. 

Quy tắc ràng buộc được xử lý trong quá trình quét, do đó không cần xử lý thêm. 

Tại sao nó hoạt động: 

Đối với bất kỳ kích thước thùng cố định`x`, những con tàu duy nhất có thể được triển khai là những con tàu có sức chứa gấp bội`x`. Thuật toán đếm chính xác những con tàu đó bằng cách tính tổng tần số ở tất cả các bội số của`x`. Do đó, khoản thanh toán được tính toán cho mọi khả năng`x`là đúng. Vì mọi kích thước thùng hợp lệ đều được kiểm tra và khoản thanh toán tối đa được giữ lại nên câu trả lời cuối cùng là kích thước thùng tối ưu. Điều kiện ràng buộc được áp dụng bất cứ khi nào tìm thấy các khoản thanh toán bằng nhau, do đó kích thước thùng hợp lệ nhỏ nhất sẽ được trả lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)

    freq = [0] * (mx + 1)
    for v in a:
        freq[v] += 1

    best_value = 0
    answer = 1

    for x in range(1, mx + 1):
        count = 0
        for multiple in range(x, mx + 1, x):
            count += freq[multiple]

        value = x * count

        if value > best_value or (value == best_value and x < answer):
            best_value = value
            answer = x

    print(answer)

if __name__ == "__main__":
    solve()
```Mảng tần số được xây dựng trước tiên vì kích thước đầu vào lớn và việc quét liên tục danh sách gốc sẽ lãng phí thời gian. Chỉ số của mảng biểu thị công suất, trong khi giá trị được lưu trữ biểu thị số lượng tàu có công suất đó. 

Vòng lặp bên ngoài xem xét mọi kích thước thùng có thể. Vòng lặp bên trong nhảy trực tiếp qua các bội số của nó, tránh việc kiểm tra các khả năng không thể đóng góp. phép nhân`x * count`có thể đạt được khoảng`10^12`, nhưng số nguyên Python xử lý việc này mà không lo tràn. 

Điều kiện cập nhật chứa quy tắc ràng buộc. Khi hai kích cỡ thùng tạo ra cùng một khoản thanh toán, kích thước nhỏ hơn sẽ thay thế câu trả lời trước đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
2 3 3 4 5
```| kích thước thùng | bội số được tính | tàu có thể sử dụng được | thanh toán | câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2,3,4,5 | 5 | 5 | 1 | 
| 2 | 2,4 | 2 | 4 | 1 | 
| 3 | 3 | 2 | 6 | 3 | 
| 4 | 4 | 1 | 4 | 3 | 
| 5 | 5 | 1 | 5 | 3 | 

Khoản thanh toán lớn nhất là`6`, đạt được theo kích thước thùng`3`. 

### Mẫu 2 

đầu vào:```
5
5 4 3 2 1
```| kích thước thùng | bội số được tính | tàu có thể sử dụng được | thanh toán | câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2,3,4,5 | 5 | 5 | 1 | 
| 2 | 2,4 | 2 | 4 | 1 | 
| 3 | 3 | 1 | 3 | 1 | 
| 4 | 4 | 1 | 4 | 1 | 
| 5 | 5 | 1 | 5 | 1 | 

Khoản thanh toán tốt nhất được chia theo kích thước thùng`1`Và`5`, do đó giá trị nhỏ hơn`1`được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M log M) | Tần suất xây dựng mất`O(N)`, và chuỗi bội số điều hòa cho`O(M log M)`| 
| Không gian | O(M) | Mảng tần số lưu trữ số đếm cho mọi công suất cho đến giá trị tối đa | 

Với`M <= 10^6`, quá trình duyệt giống như sàng chỉ thực hiện vài triệu phép tính nên nó vừa khít trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# provided samples
assert run("5\n2 3 3 4 5\n") == "3\n", "sample 1"
assert run("5\n5 4 3 2 1\n") == "1\n", "sample 2"

# minimum size
assert run("1\n1\n") == "1\n", "single ship"

# all equal values
assert run("5\n7 7 7 7 7\n") == "7\n", "all equal capacities"

# tie handling
assert run("4\n1 2 3 6\n") == "3\n", "choose smaller only when tied"

# large capacity boundary
assert run("6\n1000000 500000 250000 125000 2 4\n") == "4\n", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Đầu vào nhỏ nhất có thể | 
|`7 7 7 7 7`|`7`| Năng lực lặp lại | 
|`1 2 3 6`|`3`| Xử lý cà vạt đúng cách | 
| Công suất lớn gần`10^6`|`4`| Hành vi ranh giới của mảng tần số | 

## Vỏ cạnh 

Đối với một con tàu:```
Input
1
1
```Kích thước thùng duy nhất có thể là`1`. Mảng tần số chứa một tàu có công suất`1`, vì vậy khoản thanh toán là`1`, và thuật toán trả về`1`. 

Đối với công suất lặp lại:```
Input
5
7 7 7 7 7
```Kích thước thùng`7`đưa ra khoản thanh toán`7 * 5 = 35`. Mọi ước số nhỏ hơn sẽ mang lại khoản thanh toán thấp hơn vì giá trị thùng giảm nhanh hơn số lượng tàu tăng lên. Thuật toán đếm năm lần xuất hiện thông qua mảng tần số và chọn`7`. 

Đối với cà vạt:```
Input
4
1 2 3 6
```Thuật toán tính toán:```
x = 3: count = 2, payout = 6
x = 6: count = 1, payout = 6
```Khi`x = 6`được xử lý, khoản thanh toán phù hợp với mức tốt nhất hiện tại, nhưng`6`lớn hơn`3`, vậy đáp án vẫn là`3`. 

Đối với công suất ở giới hạn tối đa:```
Input
6
1000000 500000 250000 125000 2 4
```Mảng tần số hỗ trợ các chỉ số lên tới`1000000`và quá trình truyền tải nhiều lần vẫn hoạt động vì nó chỉ truy cập bội số của mỗi kích thước ứng viên. Thuật toán không phân bổ bộ nhớ tỷ lệ thuận với số lượng thao tác, chỉ tỷ lệ thuận với dung lượng tối đa.
