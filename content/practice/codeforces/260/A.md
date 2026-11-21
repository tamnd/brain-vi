---
title: "CF 260A - Cộng chữ số"
description: "Chúng ta bắt đầu bằng số a. Chúng tôi muốn gia hạn nó chính xác n lần. Một phần mở rộng duy nhất có nghĩa là thêm một chữ số thập phân vào bên phải số hiện tại. Chữ số có thể là bất cứ thứ gì từ 0 đến 9, nhưng sau khi thêm nó vào, số kết quả phải chia hết cho b."
date: "2026-06-04T17:49:33+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 260
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 158 (Div. 2)"
rating: 1400
weight: 260
solve_time_s: 146
verified: false
draft: false
---

[CF 260A - Cộng chữ số](https://codeforces.com/problemset/problem/260/A) 

**Đánh giá:** 1400 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một con số`a`. Chúng tôi muốn mở rộng nó một cách chính xác`n`lần. 

Một phần mở rộng duy nhất có nghĩa là thêm một chữ số thập phân vào bên phải số hiện tại. Chữ số có thể là bất cứ thứ gì từ`0`ĐẾN`9`, nhưng sau khi cộng vào thì số thu được phải chia hết cho`b`. 

Sau khi thực hiện chính xác thao tác này`n`nhiều lần, chúng ta phải xuất ra con số cuối cùng. Nếu tại bất kỳ bước nào không có chữ số nào có thể được thêm vào mà vẫn giữ nguyên tính chia hết cho`b`, thì quá trình dừng lại và câu trả lời là`-1`. 

Những hạn chế là nhỏ đối với`a`Và`b`, Nhưng`n`có thể lớn như`100000`. Điều đó ngay lập tức cho chúng ta biết rằng bất kỳ thuật toán nào thực hiện phép tính tốn kém trên toàn bộ số đang tăng đều đều nguy hiểm, vì bản thân số đó có thể chứa hơn 100000 chữ số. Chúng ta cần một phương thức mà công việc trên mỗi chữ số được nối thêm là không đổi hoặc gần như không đổi. 

Một điểm tinh tế là điều kiện chia hết phải được giữ sau mỗi phần mở rộng chứ không chỉ ở phần cuối. Ví dụ:```
a = 1, b = 11, n = 2
```Nếu chúng ta thêm các chữ số một cách tùy ý và chỉ kiểm tra số cuối cùng, chúng ta có thể tạo ra trạng thái trung gian không hợp lệ. Bản thân hoạt động đòi hỏi sự phân chia ở mỗi bước. 

Một sai lầm dễ mắc phải khác là cho rằng nếu một tiện ích mở rộng có thể thực hiện được thì tất cả các tiện ích mở rộng trong tương lai đều có thể thực hiện được. Coi như:```
a = 1, b = 2, n = 1
```Đang bổ sung`0`cho`10`, cái nào hoạt động. 

Nếu không có```
a = 1, b = 11, n = 1
```không có chữ số`d`như vậy`1d`chia hết cho`11`, vậy đáp án đúng là:```
-1
```Việc triển khai bất cẩn mà vẫn cố gắng tiếp tục sẽ là sai lầm. 

Ngoài ra còn có một quan sát quan trọng về các bước sau. Khi chúng ta đã tìm được một số chia hết cho`b`, thêm một`0`chỉ giữ tính chia hết khi`10`tương thích với`b`. Nói chung là bổ sung thêm`0`không bảo toàn tính chia hết. Ví dụ:```
12 % 3 = 0
120 % 3 = 0
```Nhưng```
14 % 7 = 0
140 % 7 = 0
```chỉ hoạt động vì ước số cụ thể. Chúng ta phải suy luận cẩn thận về điều gì sẽ xảy ra sau lần gia hạn thành công đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình chính xác như được mô tả. Ở mỗi bước, hãy thử tất cả mười chữ số có thể. Với mỗi chữ số, hãy xây dựng số mới và kiểm tra xem nó có chia hết cho không`b`. Nếu một số chữ số hoạt động, hãy nối thêm nó và tiếp tục. 

Logic này đúng vì nó tuân theo đúng định nghĩa của phép toán. Vấn đề là con số nhanh chóng trở nên khổng lồ. Sau đó`100000`phần mở rộng, nó có thể chứa hơn`100000`chữ số. Việc liên tục xây dựng và chia những con số như vậy là quá tốn kém. 

Quan sát quan trọng là khả năng chia hết chỉ phụ thuộc vào modulo còn lại`b`. 

Giả sử số hiện tại là`x`. Chữ số nối thêm`d`tạo ra```
x' = 10x + d
```Chúng tôi cần```
(10x + d) % b = 0
```Giá trị hiện tại của`x`bản thân nó không quan trọng. Chỉ một`x % b`vấn đề. 

Lúc đầu ta biết phần còn lại của`a`modulo`b`. Chúng tôi thử chữ số`0`bởi vì`9`và tìm kiếm một cái làm cho```
(10 * remainder + d) % b == 0
```Nếu tồn tại chữ số như vậy thì số mới tạo thành chia hết cho`b`. 

Bây giờ đến sự đơn giản hóa quan trọng. Sau lần gia hạn thành công đầu tiên, số hiện tại chia hết cho`b`, do đó phần còn lại của nó trở thành`0`. 

Từ thời điểm đó trở đi, mọi bước đi trong tương lai đều bắt đầu từ phần còn lại`0`. Chúng tôi chỉ cần một chữ số`d`thỏa mãn```
d % b = 0
```Từ`0`luôn luôn là một chữ số như vậy, nối thêm`0`hoạt động mãi mãi khi chúng ta đã có số chia hết. 

Vì vậy, toàn bộ vấn đề quy về việc tìm một chữ số duy nhất làm cho phần mở rộng đầu tiên chia hết cho`b`. Nếu không tồn tại chữ số đó thì câu trả lời là`-1`. Nếu không, hãy nối thêm chữ số đó một lần rồi nối thêm`n - 1`số không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số học O(10n) trên số nguyên ngày càng lớn | O(n) | Quá chậm | 
| Tối ưu | O(10 + n) | O(n) cho chuỗi đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`r = a % b`. 
2. Hãy thử từng chữ số`d`từ`0`ĐẾN`9`. 
3. Kiểm tra xem`(r * 10 + d) % b == 0`. 
4. Nếu không có chữ số nào thỏa mãn điều kiện, xuất ra`-1`. 
5. Ngược lại, đặt chữ số hợp lệ đầu tiên là`d`. 
6. Viết đáp án dưới dạng biểu diễn thập phân của`a`, theo sau là chữ số`d`. 
7. Nối chính xác`n - 1`số không. 
8. Xuất chuỗi kết quả. 

Lý do bước 7 có tác dụng là vì sau khi thêm chữ số hợp lệ đầu tiên, số hiện tại chia hết cho`b`. modulo còn lại của nó`b`bây giờ là số không. Việc thêm số 0 sẽ tạo ra```
10 * current
```số dư của nó vẫn bằng 0 vì```
(10 * 0) % b = 0
```khi làm việc với biểu diễn phần dư hiện tại. 

### Tại sao nó hoạt động 

hãy để`r = a % b`. 

Chữ số được thêm vào đầu tiên phải thỏa mãn```
(10r + d) % b = 0
```bởi vì nối thêm`d`biến số thành`10a + d`, và phần dư modulo`b`được bảo toàn bởi biểu thức này. 

Nếu không có chữ số từ`0`ĐẾN`9`thỏa mãn phương trình, phép toán đầu tiên là không thể thực hiện được, vì vậy câu trả lời phải là`-1`. 

Nếu một chữ số`d`thỏa mãn thì số mới chia hết cho`b`. Phần còn lại của nó trở thành số không. Mỗi tiện ích mở rộng sau này sẽ thêm chữ số`0`. Bắt đầu từ số dư 0, số dư tiếp theo là```
(10 * 0 + 0) % b = 0.
```Phần còn lại không bao giờ thay đổi nữa. Mọi số trung gian đều chia hết cho`b`, vì vậy tất cả các thao tác được yêu cầu đều hợp lệ. Số được xây dựng chính xác là kết quả sau`n`phần mở rộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, n = map(int, input().split())

    r = a % b

    for d in range(10):
        if (r * 10 + d) % b == 0:
            ans = str(a) + str(d) + "0" * (n - 1)
            print(ans)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính phần còn lại của`a`modulo`b`. Đây là thông tin duy nhất cần thiết để xác định liệu một chữ số có thể được thêm vào thành công hay không. 

Vòng lặp trên các chữ số`0`bởi vì`9`tìm kiếm phần mở rộng hợp lệ đầu tiên. Chỉ có mười khả năng, nên việc tìm kiếm này là thời gian không đổi. 

Khi tìm thấy một chữ số hợp lệ, câu trả lời sẽ được tạo dưới dạng chuỗi. Việc sử dụng chuỗi rất quan trọng vì số cuối cùng có thể chứa hơn 100000 chữ số. Lưu trữ nó dưới dạng số nguyên sẽ không cần thiết và không hiệu quả. 

biểu hiện`"0" * (n - 1)`nối thêm chính xác các phần mở rộng còn lại. Khi`n = 1`, chuỗi này sẽ trở thành một chuỗi trống, chỉ tạo ra một chữ số được nối thêm một cách chính xác. 

Nếu vòng lặp kết thúc mà không tìm thấy chữ số hợp lệ thì thao tác đầu tiên không thể được thực hiện và câu trả lời đúng là`-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 5
```| Bước | Giá trị | 
| --- | --- | 
| một | 5 | 
| b | 4 | 
| n | 5 | 
| r = a % b | 1 | 
| Chữ số hợp lệ đầu tiên | 2 | 
| Tiền tố sau phần mở rộng đầu tiên | 52 | 
| Đã thêm số không | 4 | 
| Câu trả lời cuối cùng | 520000 | 

Việc kiểm tra thành công vì:```
(1 * 10 + 2) % 4 = 0
```Sau khi có được`52`, chia hết cho`4`, mọi phần mở rộng còn lại đều có thể sử dụng chữ số`0`. 

### Ví dụ 2 

đầu vào:```
1 11 1
```| Bước | Giá trị | 
| --- | --- | 
| r = 1 % 11 | 1 | 
| Các chữ số đã được kiểm tra | 0..9 | 
| Chữ số thành công | không | 
| Đầu ra | -1 | 

Các giá trị được kiểm tra là:```
10, 11, 12, ..., 19
```Không ai trong số họ chia hết cho`11`, vì vậy phần mở rộng đầu tiên là không thể. 

Ví dụ này minh họa trường hợp lỗi khi không tồn tại chữ số hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Kiểm tra mười chữ số cộng với việc xây dựng chuỗi đầu ra có độ dài khoảng n | 
| Không gian | O(n) | Bản thân chuỗi đầu ra chứa O(n) ký tự | 

Chi phí chủ yếu là tạo ra câu trả lời, có thể chứa tới khoảng 100001 chữ số. Điều này phù hợp thoải mái trong giới hạn. Thuật toán chỉ thực hiện mười lần kiểm tra mô-đun bất kể`n`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        a, b, n = map(int, input().split())

        r = a % b

        for d in range(10):
            if (r * 10 + d) % b == 0:
                return str(a) + str(d) + "0" * (n - 1)

        return "-1"

    return solve()

# sample from statement
assert run("5 4 5\n") == "520000", "sample"

# minimum values
assert run("1 1 1\n") == "10", "minimum case"

# impossible first extension
assert run("1 11 1\n") == "-1", "no valid digit"

# n = 1, exactly one append
assert run("12 3 1\n") == "120", "single extension"

# divisor requiring nonzero first digit
assert run("5 7 2\n") == "560", "first digit 6 gives divisibility"

# large n shape check
out = run("1 1 100000\n")
assert len(out) == 100001, "large output length"
assert out[0] == "1", "starts correctly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`10`| Đầu vào pháp lý nhỏ nhất | 
|`1 11 1`|`-1`| Không có tiện ích mở rộng đầu tiên hợp lệ | 
|`12 3 1`|`120`| Chính xác một phần mở rộng | 
|`5 7 2`|`560`| Cần có chữ số đầu tiên khác 0 | 
|`1 1 100000`| Chiều dài 100001 | Đầu ra kích thước tối đa | 

## Vỏ cạnh 

### Không có chữ số đầu tiên hợp lệ 

đầu vào:```
1 11 1
```Phần còn lại là:```
1 % 11 = 1
```Thuật toán kiểm tra tất cả các chữ số:```
(10 + d) % 11
```vì`d = 0..9`. 

Không có gì tạo ra số không. Vì thao tác đầu tiên không thể được thực hiện nên thuật toán ngay lập tức đưa ra:```
-1
```phù hợp với định nghĩa của quá trình. 

### Chỉ cần một tiện ích mở rộng 

đầu vào:```
12 3 1
```Phần còn lại bằng không. 

chữ số`0`thỏa mãn ngay:```
(0 * 10 + 0) % 3 = 0
```Câu trả lời trở thành:```
120
```Không có số 0 nào được thêm vào vì`n - 1 = 0`. 

### Rất lớn n 

đầu vào:```
1 1 100000
```Chữ số hợp lệ đầu tiên là`0`. 

Cấu trúc thuật toán:```
10 + 99999 zeros
```mà không cần thực hiện bất kỳ phép tính tốn kém nào trên số lượng lớn. Thời gian chạy tăng tuyến tính với kích thước đầu ra, đây là mức tối ưu vì bản thân đầu ra chứa 100001 chữ số. 

### Chữ số đầu tiên hợp lệ không phải là 0 

đầu vào:```
5 7 2
```Phần còn lại là:```
5
```Thuật toán kiểm tra các chữ số cho đến khi:```
(5 * 10 + 6) % 7 = 56 % 7 = 0
```Nó chọn chữ số`6`, sản xuất`56`. Một số 0 bổ sung được thêm vào, cho:```
560
```Mọi số trung gian đều thỏa mãn yêu cầu chia hết nên việc xây dựng là hợp lệ.
