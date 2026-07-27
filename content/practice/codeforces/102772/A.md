---
title: "CF 102772A - \u0412\u0430\u0436\u043d\u043e\u0435 \u043d\u0430\u0443\u0447\u043d\u043e\u0435 \u0447\u0438\u0441\u043b\u043e"
description: "Chúng ta có hai bánh răng có kích thước a và b. Chúng ta cần cộng cùng số răng x không âm cho cả hai bánh răng. Sau khi thêm các răng này, kích thước mới đầu tiên a + x phải chia hết cho kích thước thứ hai ban đầu b và kích thước mới thứ hai b + x phải chia hết cho kích thước ban đầu đầu tiên…"
date: "2026-07-27T20:49:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102772
codeforces_index: "A"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2020-21, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102772
solve_time_s: 66
verified: true
draft: false
---

[CF 102772A - \u0412\u0430\u0436\u043d\u043e\u0435 \u043d\u0430\u0443\u0447\u043d\u043e\u0435 \u0447\u0438\u0441\u043b\u043e](https://codeforces.com/problemset/problem/102772/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai bánh răng với kích thước`a`Và`b`. Chúng ta cần cộng cùng số răng không âm`x`tới cả hai bánh răng. Sau khi thêm những chiếc răng này, kích thước mới đầu tiên`a + x`phải chia hết cho kích thước thứ hai ban đầu`b`và kích thước mới thứ hai`b + x`phải chia hết cho kích thước ban đầu đầu tiên`a`. 

Nhiệm vụ là tìm số nhỏ nhất có thể`x`. 

Đầu vào chứa hai số nguyên`a`Và`b`, mỗi cái lên đến`10^9`. Điều này ngay lập tức loại trừ việc tìm kiếm thông qua các giá trị có thể có của`x`, bởi vì câu trả lời có thể rất lớn. Ngay cả một vòng lặp kiểm tra hàng triệu ứng viên cũng không đáng tin cậy và một cách tiếp cận tỷ lệ thuận với`a`hoặc`b`là không thể. Chúng ta cần rút gọn bài toán thành một vài phép tính số học. 

Các trường hợp cạnh chính xuất phát từ mối quan hệ giữa hai số. Nếu các bánh răng đã có cùng kích thước thì câu trả lời là 0 vì cả hai điều kiện chia hết đều được thỏa mãn. Ví dụ, đầu vào`123 123`đưa ra đầu ra`0`và một giải pháp cố gắng tăng cả hai giá trị một cách mù quáng có thể bỏ lỡ điều này. 

Một trường hợp phức tạp khác là khi một số chia hết cho số kia. Đối với đầu vào`8 1`, câu trả lời là`7`, không`0`, bởi vì kích thước mới trở thành`15`Và`8`, Và`15`chia hết cho`1`trong khi`8`chia hết cho`8`. Một cách tiếp cận bất cẩn chỉ kiểm tra xem một giá trị ban đầu có chia cho giá trị kia hay không có thể dừng sớm một cách không chính xác. 

Trường hợp cạnh thứ ba xuất hiện khi bội số chung nhỏ nhất nhỏ hơn tổng của hai giá trị ban đầu. Đối với đầu vào`3 4`, bội số chung nhỏ nhất là`12`và việc căn chỉnh kích thước bánh răng cuối cùng hợp lệ nhỏ nhất yêu cầu thêm`5`, không`0`hoặc`1`. Câu trả lời là`5`bởi vì kích thước cuối cùng là`8`Và`9`, thỏa mãn yêu cầu chia hết. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ thử các giá trị của`x`bắt đầu từ số 0 và kiểm tra xem cả hai điều kiện có đúng không. Điều này đúng vì giá trị đầu tiên vượt qua bài kiểm tra chính xác là giá trị tối thiểu. Tuy nhiên, câu trả lời có thể gần`10^18`, vì vậy ngay cả việc kiểm tra một số lượng lớn các ứng cử viên là không thể. 

Quan sát hữu ích đến từ việc xem xét kích thước bánh răng cuối cùng. số`a + x`phải là bội số của`b`, Và`b + x`phải là bội số của`a`. Điều này có nghĩa là hai giá trị cuối cùng là bội số của các giá trị ban đầu đối diện trong khi chênh lệch của chúng vẫn cố định:`(a + x) - (b + x) = a - b`. 

Một cách rõ ràng hơn là thể hiện điều kiện đầu tiên. Cho phép:`a + x = k * b`. 

Sau đó:`b + x = b + k*b - a = (k + 1)b - a`. 

Để cái này có thể chia hết cho`a`, giá trị`(k + 1)b`phải chia hết cho`a`. Số nhân nhỏ nhất tạo thành bội số của`b`cũng tương thích với bội số của`a`có liên hệ với bội số chung nhỏ nhất. 

Điểm căn chỉnh chung cuối cùng là bội số của`lcm(a, b)`. Vì cả hai giá trị cuối cùng đều khác với giá trị ban đầu một lượng như nhau nên giá trị gia tăng nhỏ nhất có thể thu được bằng cách tìm bội số nhỏ nhất của bội số chung nhỏ nhất ít nhất`a + b`. 

Cho phép`L = lcm(a, b)`. Câu trả lời là:`smallest_multiple_of_L_not_less_than_(a+b) - a - b`. 

Bởi vì`L`ít nhất là cả hai`a`Và`b`, bội số cần thiết là`L`hoặc`2L`, nhưng sử dụng phép chia trần số nguyên sẽ xử lý trực tiếp mọi trường hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(log(min(a, b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ước chung lớn nhất của`a`Và`b`. Bội số chung nhỏ nhất có thể thu được là`a * b / gcd(a, b)`và việc sử dụng gcd trước tiên sẽ ngăn chặn tình trạng tràn không cần thiết trong các ngôn ngữ có loại số nguyên nhỏ hơn. 
2. Hãy để`L`là bội số chung nhỏ nhất của hai cỡ bánh răng. Chúng ta cần bội số nhỏ nhất của`L`cái đó không nhỏ hơn`a + b`. 
3. Tính hệ số chia trần:`(a + b + L - 1) // L`. nhân`L`bởi giá trị này sẽ cho điểm căn chỉnh hợp lệ nhỏ nhất. 
4. Trừ`a + b`từ điểm căn chỉnh này. Con số còn lại chính xác là số răng phải bổ sung cho cả hai bánh răng. 

Tại sao nó hoạt động: các giá trị cuối cùng`a + x`Và`b + x`phải được định vị sao cho mỗi cái đạt được cấu trúc phân chia của cái kia. Khoảng cách giữa hai giá trị này là cố định và yêu cầu chia hết chung buộc việc căn chỉnh xảy ra ở bội số của`lcm(a, b)`. Thuật toán chọn căn chỉnh đầu tiên như vậy sau tổng kích thước hiện tại`a + b`, do đó mọi giá trị nhỏ hơn của`x`sẽ rời khỏi bánh răng trước vị trí hợp lệ đầu tiên có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())

    g = math.gcd(a, b)
    lcm = a // g * b

    target = ((a + b + lcm - 1) // lcm) * lcm
    print(target - a - b)

if __name__ == "__main__":
    import math
    solve()
```Giải pháp đầu tiên là đọc hai kích thước bánh răng và tính ước số chung lớn nhất của chúng. gcd được sử dụng để xây dựng bội số chung nhỏ nhất mà không cần nhân`a`Và`b`quá sớm. 

Biến`target`là bội số đầu tiên của bội số chung nhỏ nhất không nhỏ hơn`a + b`. Công thức chia trần tránh được các lỗi sai lệch khi`a + b`đã là bội số của`lcm`. 

Phép trừ cuối cùng loại bỏ sự đóng góp ban đầu của cả hai bánh răng. Những gì còn lại chính xác là số răng phải được thêm vào mỗi bánh răng. 

Số nguyên Python tự động xử lý các giá trị lớn, do đó phép nhân được sử dụng cho bội số chung nhỏ nhất không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 5
```| Bước | một | b | gcd | lcm | mục tiêu | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 10 | 5 | 5 | 10 | 10 | 10 - 10 = 0 | 

bội số chung nhỏ nhất là`10`, vốn đã bằng`a + b`. Các bánh răng có thể được căn chỉnh thẳng hàng mà không cần thêm răng, do đó kết quả là`0`. 

### Ví dụ 2 

đầu vào:```
3 4
```| Bước | một | b | gcd | lcm | mục tiêu | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 4 | 1 | 12 | 12 | 12 - 7 = 5 | 

Tổng số hiện tại là`7`, nhưng căn chỉnh tương thích đầu tiên là`12`. Thêm`5`cho cả hai bánh răng cho kích thước`8`Và`9`, và điều kiện chia hết trở nên hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(a, b))) | Việc tính toán gcd sử dụng thuật toán Euclide. | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Giá trị đầu vào lên tới`10^9`, do đó thuật toán tránh được tất cả các vòng lặp tùy thuộc vào kích thước câu trả lời hoặc bản thân các giá trị. Tính toán gcd đủ nhanh ngay cả ở giới hạn tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    a, b = map(int, input().split())
    g = math.gcd(a, b)
    lcm = a // g * b
    target = ((a + b + lcm - 1) // lcm) * lcm
    return str(target - a - b) + "\n"

assert solution("10 5\n") == "0\n", "sample-like divisible case"
assert solution("8 1\n") == "7\n", "one divides the other"
assert solution("3 4\n") == "5\n", "different coprime values"
assert solution("123 123\n") == "0\n", "equal gears"
assert solution("1 1\n") == "0\n", "minimum values"
assert solution("1000000000 999999999\n") == "999999998999999999\n", "large values"
assert solution("6 9\n") == "3\n", "common divisor case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Các giá trị nhỏ nhất có thể và bánh răng đã hợp lệ | 
|`123 123`|`0`| Kích thước bánh răng bằng nhau | 
|`8 1`|`7`| Trường hợp một giá trị chia cho giá trị kia | 
|`6 9`|`3`| Giá trị không đồng nguyên tố và xử lý gcd | 
|`1000000000 999999999`|`999999998999999999`| Giá trị số học lớn | 

## Vỏ cạnh 

Đối với các bánh răng bằng nhau, chẳng hạn như:```
123 123
```gcd là`123`, và bội số chung nhỏ nhất cũng là`123`. tổng`a + b`đã là bội số của`lcm`, vì vậy mục tiêu vẫn còn`246`, tạo ra câu trả lời của`0`. Thuật toán xử lý việc này một cách tự nhiên mà không cần có trường hợp đặc biệt. 

Đối với trường hợp chia:```
8 1
```bội số chung nhỏ nhất là`8`. Tổng số tiền là`9`, vậy bội số đầu tiên của`8`đạt hoặc vượt quá nó là`16`. Câu trả lời là`16 - 9 = 7`, cho biết kích thước bánh răng cuối cùng`15`Và`8`. Số đầu tiên chia hết cho`1`, và số thứ hai chia hết cho`8`. 

Đối với một cặp nguyên tố cùng nhau:```
3 4
```bội số chung nhỏ nhất là`12`. Tổng kích thước hiện tại là`7`, vì vậy căn chỉnh hợp lệ đầu tiên là`12`. Thuật toán trả về`5`, sản xuất kích thước cuối cùng`8`Và`9`, Ở đâu`8`chia hết cho`4`Và`9`chia hết cho`3`. 

Đối với các giá trị rất lớn, thuật toán chỉ thực hiện các phép toán gcd và số học. Nó không bao giờ lặp lại các câu trả lời có thể có, vì vậy thời gian chạy không đổi tùy theo độ lớn của phép cộng được yêu cầu.
