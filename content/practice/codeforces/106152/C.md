---
title: "CF 106152C - Dòng Buffet"
description: "Chúng tôi có một dòng mặt hàng thực phẩm và mỗi mặt hàng đều có một giá trị mong muốn. Đối với mỗi món đồ, chúng ta có thể lấy hoặc bỏ qua. Hạn chế là chúng tôi không được phép lấy hai món đồ lân cận. Nếu chọn mục i thì phải bỏ qua mục i+1."
date: "2026-06-25T11:25:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106152
codeforces_index: "C"
codeforces_contest_name: "UT 104c Midterm #2"
rating: 0
weight: 106152
solve_time_s: 41
verified: true
draft: false
---

[CF 106152C - Dòng buffet](https://codeforces.com/problemset/problem/106152/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng mặt hàng thực phẩm và mỗi mặt hàng đều có một giá trị mong muốn. Đối với mỗi món đồ, chúng ta có thể lấy hoặc bỏ qua. 

Hạn chế là chúng tôi không được phép lấy hai món đồ lân cận. Nếu chúng ta chọn mục`i`, sau đó mục`i+1`phải được bỏ qua. 

Nhiệm vụ là tìm ra tổng mức mong muốn lớn nhất có thể đạt được theo quy tắc này. Đầu vào bao gồm số lượng mặt hàng thực phẩm và giá trị mong muốn của chúng. Đầu ra là một số nguyên duy nhất, tổng mức mong muốn tối đa có thể đạt được. 

Ràng buộc`n ≤ 10000`đủ nhỏ để một`O(n²)`giải pháp sẽ thực hiện khoảng 100 triệu thao tác trong trường hợp xấu nhất, điều này gây tốn kém không cần thiết. Vì bài toán chỉ hỏi về các vị trí liền kề nên nó gợi ý một giải pháp quy hoạch động trong đó mỗi vị trí phụ thuộc vào một số lượng không đổi các vị trí trước đó. MỘT`O(n)`giải pháp là mục tiêu tự nhiên. 

Một sai lầm phổ biến là tham lam lấy giá trị lớn hơn từ mọi cặp liền kề. Những lựa chọn địa phương không nhất thiết dẫn đến câu trả lời toàn cầu tốt nhất. 

Coi như:```
3
2 1 2
```Câu trả lời đúng là:```
4
```Chúng tôi lấy mục đầu tiên và thứ ba. Một chiến lược tham lam chỉ nhìn vào các giá trị lân cận có thể bỏ lỡ sự kết hợp này. 

Một trường hợp khác là khi chỉ có một mục.```
1
42
```Câu trả lời là:```
42
```Việc triển khai DP truy cập một cách mù quáng`dp[i-2]`không xử lý các chỉ số nhỏ có thể thất bại ở đây. 

Trường hợp cạnh thứ ba xảy ra khi nhiều giá trị bằng 0.```
5
0 0 0 0 0
```Câu trả lời là:```
0
```Thuật toán phải xử lý chính xác các tình huống trong đó không lấy gì là tối ưu một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là kiểm tra mọi tập hợp con của các mặt hàng thực phẩm, kiểm tra xem nó có chứa các vị trí được chọn liền kề hay không và tính tổng mức mong muốn của nó. Điều này đúng vì nó xem xét rõ ràng mọi lựa chọn hợp lệ. 

Vấn đề là có`2^n`tập hợp con. Ngay cả đối với`n = 50`, điều này đã vượt xa những gì có thể tính toán kịp thời và giới hạn thực tế là`n = 10000`. 

Nhận xét quan trọng là khi chúng ta đứng ở vị trí`i`, chỉ có hai khả năng có ý nghĩa. 

Hoặc chúng ta bỏ qua mục`i`, trong trường hợp đó câu trả lời tốt nhất cho đến`i`chính xác là câu trả lời tốt nhất cho đến`i-1`. 

Hoặc chúng tôi lấy hàng`i`, trong trường hợp đó mục`i-1`không thể lấy được. Giá trị tốt nhất có thể đạt được sẽ trở thành câu trả lời tốt nhất cho đến`i-2`cộng với sự mong muốn của mặt hàng`i`. 

Điều này có nghĩa là quyết định ở vị trí`i`chỉ phụ thuộc vào hai trạng thái được tính toán trước đó. Cấu trúc đó chính xác là mục đích của lập trình động. 

Cho phép`dp[i]`biểu thị mức độ mong muốn tối đa có thể đạt được bằng cách sử dụng`i`mặt hàng. 

Sau đó:```
dp[i] = max(
    dp[i-1],
    dp[i-2] + value[i]
)
```Mỗi trạng thái được tính toán một lần, đưa ra giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm | 
| DP tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và các giá trị mong muốn. 
2. Xử lý phần đế`n = 1`bằng cách trả về giá trị duy nhất. 
3. Tạo một mảng DP trong đó`dp[i]`lưu trữ mức độ mong muốn tối đa có thể đạt được từ lần đầu tiên`i`mặt hàng. 
4. Đặt`dp[0] = 0`, đại diện cho một tiền tố trống. 
5. Đặt`dp[1] = value[0]`, vì chỉ với một vật phẩm, lựa chọn tốt nhất là lấy nó. 
6. Đối với mọi vị trí`i`từ`2`ĐẾN`n`, tính:```
dp[i] = max(
    dp[i-1],
    dp[i-2] + value[i-1]
)
```Tùy chọn đầu tiên bỏ qua mục hiện tại. Tùy chọn thứ hai lấy nó và kết hợp nó với giải pháp hợp lệ tốt nhất kết thúc sớm hơn ít nhất một vị trí. 
7. Đầu ra`dp[n]`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[i]`luôn lưu trữ câu trả lời tối ưu cho câu trả lời đầu tiên`i`mặt hàng. 

Đối với`i`- mục thứ, mọi lời giải hợp lệ đều thuộc về đúng một trong hai nhóm. 

Hoặc là mục không được chọn, mang lại giá trị`dp[i-1]`, hoặc nó được chọn, buộc mục`i-1`bị loại trừ và đưa ra giá trị`dp[i-2] + value[i]`. 

Vì đây là những khả năng duy nhất và chúng tôi chọn khả năng lớn hơn,`dp[i]`là tối ưu. Bằng quy nạp, mọi trạng thái đều đúng, kể cả đáp án cuối cùng`dp[n]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

if n == 1:
    print(a[0])
else:
    dp = [0] * (n + 1)

    dp[0] = 0
    dp[1] = a[0]

    for i in range(2, n + 1):
        dp[i] = max(dp[i - 1], dp[i - 2] + a[i - 1])

    print(dp[n])
```Mảng DP sử dụng chỉ mục dựa trên 1 để thuận tiện.`dp[i]`đề cập đến cái đầu tiên`i`items, trong khi mảng ban đầu vẫn dựa trên 0. 

Việc chuyển đổi sử dụng`a[i - 1]`bởi vì`i`-trạng thái DP tương ứng với phần tử được lưu trữ tại chỉ mục`i - 1`. 

Lỗi triển khai phổ biến nhất là trộn lẫn các hệ thống lập chỉ mục mảng và vô tình sử dụng`a[i]`, làm dịch chuyển mọi giá trị theo một vị trí và gây ra lỗi chỉ mục hoặc kết quả không chính xác. 

Việc khởi tạo cũng rất quan trọng.`dp[0]`đại diện cho việc không lấy gì từ một tiền tố trống, trong khi`dp[1]`đại diện cho câu trả lời tốt nhất cho mục đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 8 10 2 0
```| tôi | Giá trị hiện tại | dp[i-2] + giá trị | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | - | - | 3 | 
| 2 | 8 | 8 | 3 | 8 | 
| 3 | 10 | 13 | 8 | 13 | 
| 4 | 2 | 10 | 13 | 13 | 
| 5 | 0 | 13 | 13 | 13 | 

Câu trả lời cuối cùng:```
13
```Sự lựa chọn tối ưu là lấy giá trị`3`Và`10`. 

### Ví dụ 2 

đầu vào:```
7
0 9 10 2 0 0 0
```| tôi | Giá trị hiện tại | dp[i-2] + giá trị | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | - | 0 | 
| 2 | 9 | 9 | 0 | 9 | 
| 3 | 10 | 10 | 9 | 10 | 
| 4 | 2 | 11 | 10 | 11 | 
| 5 | 0 | 10 | 11 | 11 | 
| 6 | 0 | 11 | 11 | 11 | 
| 7 | 0 | 11 | 11 | 11 | 

Câu trả lời cuối cùng:```
11
```Ví dụ này cho thấy rằng đôi khi việc lấy giá trị nhỏ hơn về sau sẽ tạo ra sự kết hợp tổng thể tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trạng thái DP được tính một lần | 
| Không gian | O(n) | Mảng DP lưu trữ`n + 1`tiểu bang | 

Với`n ≤ 10000`, thời gian tuyến tính cực kỳ thoải mái. Việc sử dụng bộ nhớ cũng rất nhỏ, chỉ cần vài nghìn số nguyên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    if n == 1:
        return str(a[0])

    dp = [0] * (n + 1)
    dp[1] = a[0]

    for i in range(2, n + 1):
        dp[i] = max(dp[i - 1], dp[i - 2] + a[i - 1])

    return str(dp[n])

# provided samples
assert run("5\n3 8 10 2 0\n") == "13"
assert run("7\n0 9 10 2 0 0 0\n") == "11"
assert run("1\n42\n") == "42"

# custom cases
assert run("1\n0\n") == "0", "minimum size"
assert run("5\n0 0 0 0 0\n") == "0", "all zeros"
assert run("3\n2 1 2\n") == "4", "take first and third"
assert run("6\n5 5 5 5 5 5\n") == "15", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0`| Kích thước đầu vào tối thiểu | 
|`5 / 0 0 0 0 0`|`0`| Tất cả các giá trị bằng 0 | 
|`3 / 2 1 2`|`4`| Lựa chọn không liền kề đánh bại sự lựa chọn tham lam | 
|`6 / 5 5 5 5 5 5`|`15`| Giá trị bằng nhau lặp lại | 

## Vỏ cạnh 

Hãy xem xét:```
1
42
```Thuật toán ngay lập tức sử dụng trường hợp cơ sở và in`42`. Không cần chuyển đổi DP. Điều này tránh việc truy cập không hợp lệ vào`dp[-1]`hoặc các vị trí ngoài phạm vi khác. 

Coi như:```
3
2 1 2
```Các giá trị DP trở thành:```
dp[1] = 2
dp[2] = 2
dp[3] = 4
```Thuật toán kết hợp chính xác mục thứ nhất và thứ ba, tạo ra`4`. 

Coi như:```
5
0 0 0 0 0
```Bảng DP vẫn toàn là số 0:```
dp = [0, 0, 0, 0, 0, 0]
```Đầu ra là`0`, đó là mức mong muốn tối đa có thể đạt được. 

Những trường hợp này chứng minh rằng phép truy toán xử lý các đầu vào nhỏ, các lựa chọn tối ưu không tham lam và các mục có giá trị bằng 0 một cách chính xác.
