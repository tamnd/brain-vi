---
title: "CF 136B - Logic bậc ba"
description: "Cho hai số thập phân a và c. Máy tính trong bài toán này không sử dụng xor nhị phân. Thay vào đó, nó sử dụng một phép toán ba ngôi gọi là tor. Để áp dụng tor, cả hai số đều được viết ở cơ số 3."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 136
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 97 (Div. 2)"
rating: 1100
weight: 136
solve_time_s: 101
verified: true
draft: false
---

[CF 136B - Logic bậc ba](https://codeforces.com/problemset/problem/136/B) 

**Đánh giá:** 1100 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên thập phân,`a`Và`c`. Máy tính trong bài toán này không sử dụng xor nhị phân. Thay vào đó, nó sử dụng một phép toán bậc ba được gọi là`tor`. 

Để áp dụng`tor`, cả hai số đều được viết ở cơ số 3. Nếu một số có ít chữ số hơn, chúng ta đệm nó bằng các số 0 đứng đầu để cả hai độ dài đều khớp nhau. Sau đó, chúng ta cộng các chữ số tương ứng theo modulo 3, độc lập cho mọi vị trí. Không có sự mang theo giữa các chữ số. 

Ví dụ: ở cơ sở 3: 

-`14 = 112`-`50 = 1212`Sau khi đệm: 

-`0112`-`1212`Phép cộng modulo-3 theo chữ số mang lại: 

-`(0+1)%3 = 1`-`(1+2)%3 = 0`-`(1+1)%3 = 2`-`(2+2)%3 = 1`Vậy kết quả là`1021₃ = 34`. 

Nhiệm vụ bị đảo ngược. Chúng tôi biết`a`Và`c`, và chúng ta phải xây dựng số nguyên thập phân nhỏ nhất`b`như vậy:```
a tor b = c
```Các ràng buộc rất nhỏ về mặt số lượng, cả hai giá trị nhiều nhất là`10^9`. Một đại diện cơ sở 3 của`10^9`chỉ có khoảng 20 chữ số, bởi vì:```
3^20 ≈ 3.4 × 10^9
```Điều đó có nghĩa là chúng ta có thể xử lý từng chữ số một cách an toàn ở dạng bậc ba. Bất kỳ thuật toán nào hoạt động theo thời gian tuyến tính đối với số chữ số bậc ba đều thực sự là thời gian không đổi đối với các ràng buộc này. 

Phần nguy hiểm của vấn đề này là xử lý phép trừ modulo 3 một cách chính xác. Việc thực hiện bất cẩn có thể tính toán:```
b_digit = c_digit - a_digit
```và quên rằng kết quả phải nằm trong phạm vi`[0, 2]`. 

Coi như:```
a = 2
c = 0
```Trong ternary:```
2 tor 1 = 0
```bởi vì:```
(2 + 1) % 3 = 0
```Câu trả lời đúng là`1`, không`-2`. Công thức thích hợp là:```
b_digit = (c_digit - a_digit + 3) % 3
```Another easy mistake is forgetting to process remaining digits after one number becomes zero.

 Ví dụ:```
a = 0
c = 5
```Từ`5`trong ternary là`12`, chúng ta cần:```
0 tor b = b
```vậy câu trả lời phải là`5`. 

Nếu chúng ta dừng lại ngay khi`a == 0`, chúng ta sẽ bỏ qua các chữ số bậc ba cao hơn của`c`. 

Các số 0 đứng đầu cũng có ý nghĩa quan trọng về mặt khái niệm. Giả định:```
a = 14
c = 34
```Các dạng bậc ba có độ dài khác nhau:```
14 = 112
34 = 1021
```Chúng ta phải giải thích chúng như sau:```
0112
1021
```Nếu không, việc căn chỉnh chữ số sẽ bị hỏng và tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ trực tiếp nhất là thử mọi giá trị có thể có của`b`, tính toán`a tor b`, và kiểm tra xem nó có bằng không`c`. 

Để tính toán`tor`, chúng tôi chuyển đổi cả hai số thành cơ số 3 và mô phỏng phép cộng modulo-3 theo chữ số. Bản thân hoạt động này là rẻ, hoạt động khoảng 20 chữ số. Vấn đề là không gian tìm kiếm. Từ`b`có thể lớn như khoảng`10^9`, cách tiếp cận bạo lực có thể yêu cầu hàng tỷ lần kiểm tra. Ngay cả với tốc độ vài triệu phép tính mỗi giây, điều này hoàn toàn không thể thực hiện được. 

Lý do brute-force hoạt động hợp lý là hoạt động mang tính quyết định. Đối với mỗi ứng viên`b`, hoặc:```
a tor b = c
```hoặc nó không. 

Quan sát quan trọng là`tor`hoạt động độc lập trên mỗi chữ số bậc ba. Không có sự lan truyền mang theo giữa các vị trí. Điều đó thay đổi vấn đề từ tìm kiếm toàn cầu thành vấn đề tái thiết cục bộ. 

Giả sử chúng ta đang xem xét một vị trí chữ số bậc ba:```
(a_digit + b_digit) % 3 = c_digit
```Chúng tôi biết`a_digit`Và`c_digit`, nên ta có thể giải trực tiếp cho`b_digit`:```
b_digit = (c_digit - a_digit + 3) % 3
```Mỗi vị trí bậc ba có thể được xây dựng lại một cách độc lập. Khi tất cả các chữ số được phục hồi, chúng tôi chuyển đổi kết quả bậc ba trở lại số thập phân. 

Điều này hiệu quả vì số học modulo đưa ra một câu trả lời duy nhất trong`{0,1,2}`cho mỗi cặp`(a_digit, c_digit)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^9 × log₃10^9) | O(log₃10^9) | Quá chậm | 
| Tối ưu | O(log₃10^9) | O(log₃10^9) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`c`. 
2. Khởi tạo: 

-`power = 1`-`b = 0`Biến`power`đại diện cho giá trị vị trí thứ ba hiện tại:`1, 3, 9, 27, ...`. 
3. Trong khi một trong hai`a`hoặc`c`vẫn còn các chữ số thứ ba còn lại: 

- Trích xuất các chữ số bậc ba hiện tại:```
da = a % 3
dc = c % 3
```- Tính chữ số cần tìm của`b`:```
db = (dc - da + 3) % 3
```- Thêm chữ số này vào đáp án:```
b += db * power
```Điều này xây dựng lại chữ số ternary duy nhất thỏa mãn:```
(da + db) % 3 = dc
```4. Xóa các chữ số ternary đã xử lý:```
a //= 3
c //= 3
```5. Di chuyển tới vị trí thứ ba tiếp theo:```
power *= 3
```6. Sau khi tất cả các chữ số được xử lý, hãy in`b`. 

### Tại sao nó hoạt động 

hoạt động`tor`xử lý mọi chữ số bậc ba một cách độc lập vì không có sự mang theo giữa các vị trí. Với mỗi vị trí ta giải:```
(a_digit + b_digit) % 3 = c_digit
```Số học Modulo đảm bảo chính xác một chữ số hợp lệ`b_digit`TRONG`{0,1,2}`:```
b_digit = (c_digit - a_digit) mod 3
```Vì mọi vị trí đều được xây dựng lại chính xác và các vị trí không tương tác nên số lượng đầy đủ`b`là đúng. 

Giải pháp cũng tự động tối thiểu. Mỗi chữ số bậc ba được xác định duy nhất nên chỉ có thể có một`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, c = map(int, input().split())

b = 0
power = 1

while a > 0 or c > 0:
    da = a % 3
    dc = c % 3

    db = (dc - da + 3) % 3

    b += db * power

    a //= 3
    c //= 3
    power *= 3

print(b)
```Giải pháp xử lý các số trực tiếp ở dạng ba ngôi mà không cần xây dựng các chuỗi hoặc mảng chữ số một cách rõ ràng. 

Biểu thức:```
da = a % 3
```trích xuất chữ số ternary ít quan trọng nhất. Phép chia số nguyên cho 3 sẽ loại bỏ chữ số đó sau đó. 

Phần tinh tế là:```
db = (dc - da + 3) % 3
```phần bổ sung`+3`ngăn chặn các giá trị trung gian âm. Không có nó, các trường hợp như:```
dc = 0
da = 2
```sẽ sản xuất không chính xác`-2`thay vì`1`. 

Biến`power`theo dõi giá trị vị trí thứ ba hiện tại. Khi chúng ta tính một chữ số`db`, chúng ta đặt nó vào đúng vị trí trong câu trả lời thập phân cuối cùng bằng cách sử dụng:```
b += db * power
```Vòng lặp tiếp tục trong khi một trong hai số vẫn còn các chữ số thứ ba. Điều này xử lý chính xác các trường hợp`a`Và`c`có độ dài bậc ba khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
14 34
```Các dạng bậc ba:```
14 = 112
34 = 1021
```| Bước | da | dc | db = (dc-da+3)%3 | quyền lực | b | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 1 | 2 | 
| 2 | 1 | 2 | 1 | 3 | 5 | 
| 3 | 1 | 0 | 2 | 9 | 23 | 
| 4 | 0 | 1 | 1 | 27 | 50 | 

Câu trả lời cuối cùng:```
50
```Dấu vết này cho thấy độ dài bậc ba khác nhau được xử lý một cách tự nhiên như thế nào. Một lần`a`hết chữ số, các chữ số còn lại của nó hoạt động giống như số 0 đứng đầu. 

### Ví dụ 2 

đầu vào:```
2 0
```| Bước | da | dc | db | quyền lực | b | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 1 | 1 | 1 | 

Câu trả lời cuối cùng:```
1
```Ví dụ này chứng minh tại sao phép trừ modulo lại quan trọng. Một phép trừ ngây thơ sẽ cho`-2`, nhưng số học modulo 3 gói gọn nó một cách chính xác`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log₃ n) | Một lần lặp cho mỗi chữ số bậc ba | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Ngay cả đối với kích thước đầu vào tối đa của`10^9`, vòng lặp chỉ chạy khoảng 20 lần. Giải pháp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a, c = map(int, input().split())

    b = 0
    power = 1

    while a > 0 or c > 0:
        da = a % 3
        dc = c % 3

        db = (dc - da + 3) % 3

        b += db * power

        a //= 3
        c //= 3
        power *= 3

    print(b)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("14 34\n") == "50\n", "sample 1"

# minimum values
assert run("0 0\n") == "0\n", "both zero"

# modulo wraparound
assert run("2 0\n") == "1\n", "requires modulo subtraction"

# different ternary lengths
assert run("0 5\n") == "5\n", "remaining digits must be processed"

# large values
assert run("1000000000 1000000000\n") == "0\n", "a tor 0 = a"

# carry-like situation should not exist
assert run("8 0\n") == "4\n", "independent ternary digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Ranh giới tối thiểu | 
|`2 0`|`1`| Phép trừ modulo đúng | 
|`0 5`|`5`| Xử lý các độ dài bậc ba khác nhau | 
|`1000000000 1000000000`|`0`| Giá trị đầu vào lớn | 
|`8 0`|`4`| Không lan truyền giữa các chữ số | 

## Vỏ cạnh 

Hãy xem xét:```
0 0
```Vòng lặp không bao giờ thực hiện vì cả hai số đều bằng 0. Câu trả lời khởi tạo`b = 0`được in trực tiếp. Điều này đúng vì:```
0 tor 0 = 0
```Bây giờ hãy xem xét:```
2 0
```Trong ternary:```
2 tor 1 = 0
```Thuật toán tính toán:```
db = (0 - 2 + 3) % 3 = 1
```và trả về`1`. Điều này xác nhận việc hiệu chỉnh modulo xử lý chính xác những khác biệt âm. 

Xem xét độ dài chữ số khác nhau:```
0 5
```Đại diện bậc ba:```
0 = 0
5 = 12
```Thuật toán tiếp tục chạy trong khi`c > 0`, tạo ra các chữ số bậc ba`2`Và`1`. Số được xây dựng lại chính xác`12₃ = 5`. 

Cuối cùng, hãy xem xét:```
8 0
```Trong ternary:```
8 = 22
```Chúng tôi giải quyết từng chữ số:```
(2 + 1) % 3 = 0
(2 + 1) % 3 = 0
```Vì thế:```
b = 11₃ = 4
```Ví dụ này xác nhận không có sự tương tác giữa các vị trí. Mỗi chữ số được giải độc lập.
