---
title: "CF 171A - Những con số bí ẩn - 1"
description: "Vấn đề này xuất phát từ một cuộc thi Cá tháng Tư, trong đó tuyên bố cố tình che giấu nhiệm vụ thực sự. Cho ta hai số nguyên không âm. Thao tác bắt buộc là: 1. Đảo ngược biểu diễn thập phân của số thứ hai. 2. Cộng kết quả vào số đầu tiên. 3."
date: "2026-06-02T08:58:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 171
codeforces_index: "A"
codeforces_contest_name: "April Fools Day Contest"
rating: 1200
weight: 171
solve_time_s: 103
verified: true
draft: false
---

[CF 171A - Những con số bí ẩn - 1](https://codeforces.com/problemset/problem/171/A) 

**Đánh giá:** 1200 
**Tas:** *thuật toán đặc biệt, mang tính xây dựng 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề này xuất phát từ một cuộc thi Cá tháng Tư, trong đó tuyên bố cố tình che giấu nhiệm vụ thực sự. 

Cho ta hai số nguyên không âm. Thao tác cần thực hiện là: 

1. Đảo ngược biểu diễn thập phân của số thứ hai. 
2. Cộng kết quả vào số đầu tiên. 
3. Xuất ra tổng. 

Ví dụ: nếu đầu vào là`3 14`, ngược lại của`14`là`41`, và câu trả lời là`3 + 41 = 44`. Điều này phù hợp với đầu ra mẫu. 

Số lượng nhiều nhất là$10^9$, vậy mỗi số có nhiều nhất là mười chữ số. Bất kỳ thao tác nào trên các chữ số của một số đều mất thời gian không đổi trong thực tế. Không cần các thuật toán, cấu trúc dữ liệu hoặc tối ưu hóa phức tạp. Việc đảo ngược chữ số đơn giản theo sau là một phép cộng dễ dàng nằm trong giới hạn. 

Nguyên nhân chính của sai lầm là việc xử lý các số 0 đứng đầu sau khi đảo ngược. 

Hãy xem xét đầu vào:```
100 200
```đảo ngược`"200"`cho`"002"`, đại diện cho số nguyên`2`. Câu trả lời là:```
100 + 2 = 102
```Việc triển khai bất cẩn nhằm bảo toàn các số 0 đứng đầu dưới dạng chữ số có thể coi giá trị đảo ngược là không chính xác.`200`. 

Một trường hợp cạnh khác là khi số thứ hai bằng 0:```
5 0
```Sự đảo ngược của`0`vẫn còn`0`, vậy đáp án là:```
5
```Một số triển khai dựa trên chuỗi vô tình tạo ra một chuỗi trống khi loại bỏ các số 0. 

Trường hợp cạnh thứ ba là khi số đảo ngược kết thúc bằng số 0 trước khi đảo ngược:```
27 12
```Sự đảo ngược của`12`là`21`, cho:```
27 + 21 = 48
```Mẫu này xác nhận rằng thao tác này là đảo ngược chữ số thực sự thay vì sắp xếp hoặc sắp xếp lại các chữ số. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ kiểm tra từng chữ số của số thứ hai, xây dựng dạng đảo ngược của nó, chuyển nó trở lại thành số nguyên và sau đó thêm nó vào số đầu tiên. 

Vì số thứ hai chứa tối đa mười chữ số nên số này đã chạy trong thời gian không đổi. Ngay cả khi chúng tôi xử lý hàng triệu trường hợp thử nghiệm thì công việc trên mỗi trường hợp vẫn rất nhỏ. 

Quan sát quan trọng chỉ đơn giản là hiểu được hoạt động ẩn. Một khi chúng ta nhận ra rằng câu trả lời là:$$a_1 + \text{reverse}(a_2)$$việc thực hiện trở nên đơn giản. 

Có hai cách phổ biến để đảo ngược một số. Một là số học, liên tục lấy chữ số cuối cùng và nối nó vào một số mới. Cách còn lại là chuyển đổi số thành chuỗi và đảo ngược các ký tự. Vì kích thước đầu vào cực kỳ nhỏ nên cả hai cách tiếp cận đều được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đảo ngược chữ số Brute Force | O(d) | O(1) | Đã chấp nhận | 
| Tối ưu | O(d) | O(1) | Đã chấp nhận | 

Đây$d$là số chữ số của số thứ hai, nhiều nhất là 10. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a1`Và`a2`. 
2. Reverse the decimal digits of`a2`. 

Phương pháp đơn giản nhất là trích xuất liên tục chữ số cuối cùng bằng`% 10`và nối nó vào một số mới. 
3. Thêm giá trị đảo ngược vào`a1`. 
4. Xuất kết quả. 

### Tại sao nó hoạt động 

Quy tắc ẩn của bài toán chính xác là cộng số thứ nhất với số thứ hai bị đảo ngược. Thủ tục đảo ngược sẽ xây dựng lại số nguyên có biểu diễn thập phân được viết theo thứ tự ngược lại. Mỗi chữ số của`a2`xuất hiện một lần và ở vị trí đảo ngược chính xác, do đó giá trị được xây dựng chính xác`reverse(a2)`. Thêm giá trị này vào`a1`mang lại câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a1, a2 = map(int, input().split())

rev = 0
if a2 == 0:
    rev = 0
else:
    while a2 > 0:
        rev = rev * 10 + (a2 % 10)
        a2 //= 10

print(a1 + rev)
```Phần đầu tiên đọc hai số nguyên. 

Biến`rev`lưu trữ dạng đảo ngược của số thứ hai. Trong mỗi lần lặp, chữ số cuối cùng hiện tại được thêm vào cuối`rev`. Ví dụ, nếu`a2 = 123`, dãy số trở thành:```
rev = 3
rev = 32
rev = 321
```Trường hợp đặc biệt`a2 = 0`được xử lý một cách rõ ràng. Nếu không có nó, vòng lặp sẽ không bao giờ thực thi và câu trả lời vẫn đúng nhưng ý định trở nên rõ ràng hơn. 

Cuối cùng, mã thêm`a1`và số đảo ngược và in kết quả. 

Vì số nguyên Python có độ chính xác tùy ý nên không có rủi ro tràn. Ngay cả trong các ngôn ngữ có số nguyên 64 bit có chiều rộng cố định, câu trả lời lớn nhất có thể có là rất nhỏ so với giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 14
```| Bước | a2 | Chữ số được trích xuất | vòng quay | 
| --- | --- | --- | --- | 
| Bắt đầu | 14 | - | 0 | 
| 1 | 14 | 4 | 4 | 
| 2 | 1 | 1 | 41 | 

Tính toán cuối cùng: 

| a1 | đảo ngược(a2) | Trả lời | 
| --- | --- | --- | 
| 3 | 41 | 44 | 

Dấu vết cho thấy mỗi chữ số được trích xuất sẽ được thêm vào số mới như thế nào, tạo ra`41`. 

### Ví dụ 2 

đầu vào:```
100 200
```| Bước | a2 | Chữ số được trích xuất | vòng quay | 
| --- | --- | --- | --- | 
| Bắt đầu | 200 | - | 0 | 
| 1 | 200 | 0 | 0 | 
| 2 | 20 | 0 | 0 | 
| 3 | 2 | 2 | 2 | 

Tính toán cuối cùng: 

| a1 | đảo ngược(a2) | Trả lời | 
| --- | --- | --- | 
| 100 | 2 | 102 | 

Ví dụ này chứng minh rằng các số 0 đứng đầu biến mất một cách tự nhiên khi giá trị đảo ngược được lưu dưới dạng số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Xử lý từng chữ số của số thứ hai một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Từ$d \le 10$, thời gian chạy thực tế là không đổi. Giải pháp thấp hơn nhiều so với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    a1, a2 = map(int, input().split())

    rev = 0
    if a2 == 0:
        rev = 0
    else:
        while a2 > 0:
            rev = rev * 10 + (a2 % 10)
            a2 //= 10

    print(a1 + rev)

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

# provided samples
assert run("3 14\n") == "44\n", "sample 1"
assert run("27 12\n") == "48\n", "sample 2"
assert run("100 200\n") == "102\n", "sample 3"

# custom cases
assert run("0 0\n") == "0\n", "both zero"
assert run("5 0\n") == "5\n", "reverse of zero"
assert run("0 1200\n") == "21\n", "leading zeros after reversal"
assert run("1000000000 1000000000\n") == "1000000001\n", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Giá trị tối thiểu có thể | 
|`5 0`|`5`| Đảo ngược số 0 | 
|`0 1200`|`21`| Các số 0 đứng đầu biến mất sau khi đảo ngược | 
|`1000000000 1000000000`|`1000000001`| Đầu vào lớn nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
100 200
```Thuật toán trích xuất chữ số`0`,`0`, Và`2`. Giá trị đảo ngược trở thành`2`, không`002`như một đối tượng riêng biệt. Câu trả lời cuối cùng là:```
100 + 2 = 102
```Điều này phù hợp với hành vi mong đợi và tránh những sai lầm liên quan đến số 0 đứng đầu. 

Coi như:```
5 0
```Số thứ hai đã bằng 0. Giá trị đảo ngược cũng bằng không. Thuật toán in:```
5
```Không có vấn đề định dạng đặc biệt nào xảy ra. 

Coi như:```
0 1200
```Trình tự trích xuất chữ số là:```
0 -> 0
0 -> 0
2 -> 2
1 -> 21
```Giá trị đảo ngược là`21`, và đáp án cuối cùng là:```
21
```Điều này xác nhận rằng các số 0 ở cuối số ban đầu sẽ trở thành các số 0 đứng đầu sau khi đảo ngược và tự động bị loại bỏ khi được biểu thị dưới dạng số nguyên.
