---
title: "CF 16C - Màn hình"
description: "Chúng ta bắt đầu với một màn hình có kích thước là a × b. Cả hai giá trị đều là số nguyên. Chúng tôi muốn thu nhỏ màn hình này để kích thước mới giữ nguyên tỷ lệ khung hình x : y."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 16
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 16 (Div. 2 Only)"
rating: 1800
weight: 16
solve_time_s: 97
verified: true
draft: false
---
[CF 16C - Màn hình](https://codeforces.com/problemset/problem/16/C) 

**Đánh giá:** 1800 
**Tas:** tìm kiếm nhị phân, lý thuyết số 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một màn hình có kích thước`a × b`. Cả hai giá trị đều là số nguyên. Chúng tôi muốn thu nhỏ màn hình này để kích thước mới giữ được tỷ lệ khung hình chính xác`x : y`. 

Màn hình mới phải đáp ứng đồng thời ba điều kiện. 

Đầu tiên, cả hai chiều phải là số nguyên. 

Thứ hai, chiều rộng mới không thể vượt quá`a`và chiều cao mới không thể vượt quá`b`, bởi vì chúng tôi chỉ được phép giảm kích thước màn hình. 

Thứ ba, trong số tất cả các màn hình giảm hợp lệ có tỷ lệ`x : y`, chúng ta muốn cái có diện tích lớn nhất có thể. 

Điều kiện tỷ lệ khung hình có nghĩa là kích thước cuối cùng phải giống như sau:$$(k \cdot x,\ k \cdot y)$$với một số nguyên dương`k`. 

Các ràng buộc đi lên đến`2 · 10^9`, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại qua tất cả các chiều rộng, chiều cao hoặc hệ số tỷ lệ có thể có, lần lượt. Một vòng lặp chạy hàng tỷ lần là không thể trong giới hạn lập trình cạnh tranh. Chúng ta cần một giải pháp thời gian không đổi hoặc logarit. 

Phần khó khăn là tỷ lệ này có thể chưa giảm. Ví dụ, nếu`x = 6`Và`y = 4`, thì tỷ lệ thực sự tương đương với`3 : 2`. Nếu chúng ta quên đơn giản hóa trước, chúng ta có thể kết luận sai rằng một số câu trả lời hợp lệ là không thể. 

Hãy xem xét ví dụ này:```
a = 10
b = 10
x = 6
y = 4
```Nếu chúng ta sử dụng`(6k, 4k)`trực tiếp, giá trị lớn nhất`k`là`1`, cho`(6, 4)`với diện tích`24`. 

Nhưng sau khi giảm tỷ lệ xuống`3 : 2`, chúng ta có thể sử dụng`k = 3`, cho`(9, 6)`với diện tích`54`, lớn hơn nhiều. 

Một trường hợp cạnh khác xuất hiện khi một chiều trở thành yếu tố giới hạn.```
a = 8
b = 5
x = 4
y = 3
```Việc triển khai bất cẩn có thể tối đa hóa chiều rộng trước tiên và thử`(8, 6)`, vượt quá giới hạn chiều cao. 

Cách tiếp cận đúng nhận ra rằng cả hai chiều phải phù hợp đồng thời. Hệ số tỷ lệ hợp lệ lớn nhất là:$$k = \min\left(\left\lfloor \frac{a}{x} \right\rfloor,\ \left\lfloor \frac{b}{y} \right\rfloor\right)$$sau khi giảm tỷ lệ. 

Một trường hợp tinh vi hơn là khi không có màn hình dương tính hợp lệ nào tồn tại.```
a = 1
b = 1
x = 2
y = 3
```Ngay cả màn hình duy trì tỷ lệ nhỏ nhất cũng cần có kích thước`(2, 3)`, không phù hợp. Đầu ra đúng là:```
0 0
```Việc triển khai có lỗi có thể vô tình tạo ra các giá trị âm, giá trị phân số hoặc`(2, 3)`mà không kiểm tra giới hạn một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi yếu tố mở rộng có thể`k`và kiểm tra xem`(k·x, k·y)`vừa vặn bên trong`(a, b)`. Trong số tất cả các lựa chọn hợp lệ, chúng tôi sẽ chọn lựa chọn có diện tích lớn nhất. 

Điều này hoạt động vì mọi màn hình hợp lệ có tỷ lệ`x : y`phải là bội số nguyên nào đó của`(x, y)`. Vấn đề là kích thước không gian tìm kiếm. Nếu như`a = b = 2 · 10^9`Và`x = y = 1`, sau đó`k`có thể lớn như`2 · 10^9`. Việc lặp qua hàng tỷ ứng viên là quá chậm. 

Quan sát quan trọng là khu vực này phát triển đơn điệu với`k`. 

Diện tích bằng:$$(kx)(ky) = k^2xy$$Từ`x`Và`y`là các số dương cố định, việc tối đa hóa diện tích hoàn toàn giống với việc tối đa hóa`k`. 

Vì vậy, thay vì kiểm tra mọi ứng viên, chúng ta chỉ cần số nguyên lớn nhất`k`thỏa mãn:$$kx \le a$$

$$ky \le b$$Điều đó mang lại:$$k \le \left\lfloor \frac{a}{x} \right\rfloor$$

$$k \le \left\lfloor \frac{b}{y} \right\rfloor$$Giá trị hợp lệ lớn nhất chỉ đơn giản là giá trị nhỏ hơn trong hai giới hạn đó. 

Vẫn còn một chi tiết quan trọng. Trước khi tính toán`k`, chúng ta phải giảm tỷ lệ`(x, y)`bởi ước chung lớn nhất của chúng. Nếu không, chúng tôi buộc kích thước màn hình phải lớn hơn mức cần thiết một cách giả tạo. 

Ví dụ, tỷ lệ`100 : 50`thực sự là`2 : 1`. Việc sử dụng phiên bản chưa được rút gọn sẽ hạn chế nghiêm trọng hệ số tỷ lệ có thể có. 

Sau khi chia cả hai số cho`gcd(x, y)`, thuật toán sẽ trở thành một vài phép tính số học và một phép tính gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(min(a/x, b/y)) | O(1) | Quá chậm | 
| Tối ưu | O(log(min(x, y))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên`a`,`b`,`x`, Và`y`.`a`Và`b`là giới hạn của màn hình, trong khi`x : y`là tỷ lệ khung hình cần thiết. 
2. Tính toán`g = gcd(x, y)`. 

Tỷ lệ có thể không giảm. Chia cho gcd sẽ cho tỷ số tương đương nhỏ nhất. 
3. Thay thế:$$x \leftarrow \frac{x}{g}$$

$$y \leftarrow \frac{y}{g}$$Sau bước này,`x`Và`y`là nguyên tố cùng nhau. 
4. Tính hệ số tỷ lệ lớn nhất có thể:$$k = \min\left(\left\lfloor \frac{a}{x} \right\rfloor,\ \left\lfloor \frac{b}{y} \right\rfloor\right)$$Mọi giá trị lớn hơn sẽ vi phạm ít nhất một giới hạn thứ nguyên. 
5. Đầu ra:$$(k \cdot x,\ k \cdot y)$$Nếu như`k = 0`, điều này tự động trở thành`0 0`, có nghĩa là không có màn hình giảm hợp lệ nào tồn tại. 

### Tại sao nó hoạt động 

Sau khi giảm tỷ lệ, mọi màn hình hợp lệ có tỷ lệ khung hình`x : y`có hình thức`(kx, ky)`đối với một số nguyên`k ≥ 0`. 

Các ràng buộc của màn hình yêu cầu cả hai kích thước phải phù hợp:$$kx \le a$$

$$ky \le b$$Vì vậy mọi giá trị hợp lệ`k`phải thỏa mãn:$$k \le \left\lfloor \frac{a}{x} \right\rfloor$$Và$$k \le \left\lfloor \frac{b}{y} \right\rfloor$$Lấy mức tối thiểu sẽ cho hệ số tỷ lệ khả thi lớn nhất. 

Vì diện tích bằng:$$(kx)(ky) = k^2xy$$Và`x`,`y`là các số nguyên dương cố định thì diện tích tăng đúng theo`k`tăng lên. hợp lệ lớn nhất`k`luôn tạo ra diện tích tối đa có thể. 

Trước tiên, việc giảm tỷ lệ là cần thiết vì tất cả các tỷ lệ khung hình tương đương phải đại diện cho cùng một dòng màn hình. Việc sử dụng các giá trị không rút gọn sẽ bỏ qua các ứng viên hợp lệ. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

a, b, x, y = map(int, input().split())

g = gcd(x, y)

x //= g
y //= g

k = min(a // x, b // y)

print(k * x, k * y)
```Lời giải tuân theo đạo hàm toán học trực tiếp. 

Bước quan trọng đầu tiên là giảm tỷ lệ bằng cách sử dụng`gcd`. Nếu không có điều này, thuật toán có thể tạo ra màn hình nhỏ hơn mức tối ưu. Từ`x`Và`y`có thể lớn như`2 · 10^9`, sử dụng thuật toán Euclid là lý tưởng vì nó chạy theo thời gian logarit. 

Sau khi chuẩn hóa, mã sẽ tính hệ số tỷ lệ lớn nhất có thể`k`. Phép chia số nguyên là cần thiết ở đây. Chúng ta cần hệ số nguyên lớn nhất vẫn vừa với giới hạn màn hình. 

Thứ tự của các hoạt động quan trọng. Chúng tôi giảm tỷ lệ trước khi tính toán các phép chia. Nếu chúng ta tính toán các giới hạn bằng cách sử dụng các giá trị chưa rút gọn, chúng ta có thể đánh giá thấp hệ số tỷ lệ hợp lệ. 

Số nguyên Python tự động tránh tràn, nhưng logic này cũng an toàn trong ngôn ngữ 64-bit vì tích lớn nhất có thể là:$$2 \cdot 10^9$$vừa vặn thoải mái bên trong các số nguyên 64 bit đã ký. 

Việc thực hiện tự nhiên xử lý các trường hợp không thể. Nếu một trong hai`a // x`hoặc`b // y`bằng 0 thì`k = 0`, và kết quả được in trở thành`0 0`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
800 600 4 3
```Tỷ lệ đã giảm rồi. 

| Bước | một | b | x | y | gcd | k | 
| --- | --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 800 | 600 | 4 | 3 | - | - | 
| Tính gcd | 800 | 600 | 4 | 3 | 1 | - | 
| Giảm tỷ lệ | 800 | 600 | 4 | 3 | 1 | - | 
| Tính k | 800 | 600 | 4 | 3 | 1 | 200 | 
| Câu trả lời cuối cùng | 800 | 600 | 4 | 3 | 1 | 200 | 

Màn hình kết quả là`(800, 600)`. Ví dụ này cho thấy màn hình gốc có thể đã đáp ứng tỷ lệ khung hình được yêu cầu. 

### Ví dụ 2 

đầu vào:```
10 10 6 4
```Tỷ lệ không giảm. 

| Bước | một | b | x | y | gcd | k | 
| --- | --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 10 | 10 | 6 | 4 | - | - | 
| Tính gcd | 10 | 10 | 6 | 4 | 2 | - | 
| Giảm tỷ lệ | 10 | 10 | 3 | 2 | 2 | - | 
| Tính k | 10 | 10 | 3 | 2 | 2 | 3 | 
| Câu trả lời cuối cùng | 9 | 6 | 3 | 2 | 2 | 3 | 

Câu trả lời trở thành`(9, 6)`. 

Dấu vết này chứng minh tại sao việc giảm tỷ lệ là bắt buộc. Nếu chúng ta bỏ qua bước gcd, chúng ta sẽ nhận được sai`(6, 4)`thay vì màn hình hợp lệ lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(x, y))) | Thuật toán gcd của Euclid thống trị thời gian chạy | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc là cực kỳ lớn nên bất kỳ tìm kiếm lặp lại nào cũng sẽ thất bại. Giải pháp này chỉ thực hiện phép tính gcd và một số phép tính số học, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from math import gcd

def solve():
    input = sys.stdin.readline

    a, b, x, y = map(int, input().split())

    g = gcd(x, y)

    x //= g
    y //= g

    k = min(a // x, b // y)

    print(k * x, k * y)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("800 600 4 3\n") == "800 600", "sample 1"

# minimum valid case
assert run("1 1 1 1\n") == "1 1", "minimum case"

# impossible case
assert run("1 1 2 3\n") == "0 0", "no valid monitor"

# ratio reduction required
assert run("10 10 6 4\n") == "9 6", "must reduce ratio"

# maximum values
assert run("2000000000 2000000000 1 1\n") == "2000000000 2000000000", "large values"

# one dimension limiting
assert run("8 5 4 3\n") == "4 3", "height constraint dominates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1 1`| Màn hình hợp lệ nhỏ nhất | 
|`1 1 2 3`|`0 0`| Cấu hình không thể | 
|`10 10 6 4`|`9 6`| Giảm gcd đúng | 
|`2000000000 2000000000 1 1`|`2000000000 2000000000`| Xử lý số nguyên lớn | 
|`8 5 4 3`|`4 3`| Lựa chọn giới hạn tối thiểu thích hợp | 

## Vỏ cạnh 

Xét trường hợp tỷ số không rút gọn:```
10 10 6 4
```Thuật toán tính toán:$$gcd(6,4)=2$$nên tỉ số trở thành`(3,2)`. 

Sau đó:$$k = \min(10//3,\ 10//2)=\min(3,5)=3$$Màn hình cuối cùng là:```
9 6
```Nếu chúng ta bỏ qua việc giảm, chúng ta sẽ chỉ nhận được`(6,4)`, có diện tích nhỏ hơn nhiều. 

Bây giờ hãy kiểm tra một cấu hình không thể thực hiện được:```
1 1 2 3
```Tỷ lệ đã giảm rồi. Hệ số tỷ lệ trở thành:$$k = \min(1//2,\ 1//3)=\min(0,0)=0$$Các đầu ra thuật toán:```
0 0
```chỉ ra chính xác rằng không có màn hình tích cực nào có thể đáp ứng tỷ lệ. 

Cuối cùng, hãy xem xét trường hợp một chiều là nút cổ chai:```
8 5 4 3
```Chúng tôi tính toán:$$k = \min(8//4,\ 5//3)=\min(2,1)=1$$Kết quả trở thành:```
4 3
```Mặc dù chiều rộng cho phép chia tỷ lệ theo`2`, chiều cao thì không. Việc lấy mức tối thiểu của hai giới hạn sẽ đảm bảo cả hai chiều vẫn có hiệu lực đồng thời.
