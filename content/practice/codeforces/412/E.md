---
title: "CF 412E - Địa chỉ E-mail"
description: "Chúng ta được cấp một chuỗi dài chỉ chứa các chữ cái viết thường, chữ số, @, và .. Chúng ta phải đếm xem có bao nhiêu chuỗi con của chuỗi này là địa chỉ email hợp lệ. Các chuỗi con được phân biệt bởi vị trí của chúng chứ không phải bởi nội dung văn bản của chúng."
date: "2026-06-07T02:21:17+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 412
codeforces_index: "E"
codeforces_contest_name: "Coder-Strike 2014 - Round 1"
rating: 1900
weight: 412
solve_time_s: 218
verified: false
draft: false
---

[CF 412E - Địa chỉ email](https://codeforces.com/problemset/problem/412/E) 

**Xếp hạng:** 1900 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi dài chỉ chứa các chữ cái viết thường, chữ số,`_`,`@`, Và`.`. 

Chúng ta phải đếm xem có bao nhiêu chuỗi con của chuỗi này là địa chỉ email hợp lệ. Các chuỗi con được phân biệt bởi vị trí của chúng chứ không phải bởi nội dung văn bản của chúng. Nếu cùng một văn bản e-mail xuất hiện ở hai vị trí khác nhau thì cả hai lần xuất hiện đều góp phần đưa ra câu trả lời. 

Một địa chỉ hợp lệ có dạng:`local_part@domain.tld`Phần cục bộ phải không trống, có thể chứa các chữ cái, chữ số và`_`, và ký tự đầu tiên của nó phải là một chữ cái. 

Phần miền giữa`@`Và`.`không được để trống và chỉ chứa các chữ cái hoặc chữ số. 

Phần cuối cùng sau`.`phải không trống và chỉ chứa các chữ cái. 

Chiều dài chuỗi có thể đạt tới$10^6$. Bất kỳ thuật toán nào kiểm tra tất cả các chuỗi con đều không thể thực hiện được. có khoảng$5 \cdot 10^{11}$chuỗi con trong trường hợp xấu nhất và thậm chí chỉ kiểm tra một phần rất nhỏ trong số chúng sẽ vượt quá giới hạn thời gian. 

Thử thách là một`@`có thể tham gia vào nhiều địa chỉ hợp lệ khác nhau vì phần cục bộ có thể bắt đầu ở các vị trí chữ cái khác nhau và một địa chỉ duy nhất`.`có thể tham gia vào nhiều địa chỉ hợp lệ khác nhau vì địa chỉ có thể kết thúc bằng các chữ cái khác nhau của khối chữ cái cuối cùng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1@a.b
```Phần cục bộ bắt đầu bằng một chữ số, do đó không có địa chỉ hợp lệ. Giải pháp chỉ kiểm tra xem phần cục bộ có chứa các ký tự hợp lệ hay không sẽ tính sai một địa chỉ. 

Coi như:```
ab@12.xyz
```Tên miền có thể chứa các chữ số, vì vậy điều này hợp lệ. Việc coi miền chỉ là chữ cái sẽ bỏ lỡ các câu trả lời hợp lệ. 

Coi như:```
a@b.cd3
```Địa chỉ không thể kết thúc sau`3`, vì thành phần cuối cùng chỉ được chứa các chữ cái. Phần cuối hợp lệ chỉ nằm trong khối chữ cái liên tiếp ngay sau dấu chấm. 

Coi như:```
ab_cd@x.yz
```Phần cục bộ có thể bắt đầu tại`a`hoặc tại`b`, bởi vì bất kỳ vị trí bắt đầu nào là một chữ cái và nằm trong cùng một khối ký tự cục bộ hợp lệ sẽ tạo ra một phần cục bộ hợp lệ. Chỉ tính phần cục bộ tối đa sẽ bị tính thiếu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi chuỗi con và kiểm tra xem nó có khớp với định dạng e-mail hay không. có$O(n^2)$các chuỗi con. Ngay cả khi xác nhận là$O(1)$, số tiền này đã quá lớn đối với$n = 10^6$. 

Quan sát quan trọng là một địa chỉ hợp lệ hoàn toàn được xác định bởi sự lựa chọn`@`Và`.`. 

Giả sử một dấu chấm được cố định ở vị trí`j`. 

Để phần miền hợp lệ, mọi ký tự giữa`@`Và`.`phải là chữ và số. Đặt khối chữ và số tối đa kết thúc tại`j - 1`bắt đầu ở vị trí`t`. 

Khi đó có nhiều nhất một khả năng`@`: nó phải chính xác ở vị trí`t - 1`. 

Nếu sớm hơn, khoảng sẽ chứa ký tự không phải chữ và số. Nếu muộn hơn thì nó sẽ nằm bên trong khối chữ và số, điều này là không thể vì`@`không phải là chữ và số. 

Điều này làm giảm vấn đề đáng kể. Với mỗi dấu chấm có nhiều nhất một ứng cử viên`@`. 

Bây giờ chúng ta chỉ cần hai bội số. 

Đối với giá trị cố định`@`, có bao nhiêu điểm bắt đầu có thể có của phần cục bộ? Mỗi chữ cái bên trong khối ký tự cục bộ tối đa được phép kết thúc ngay trước`@`có thể phục vụ như là sự khởi đầu. 

Đối với một dấu chấm hợp lệ cố định, có thể tồn tại bao nhiêu đầu? Mỗi chữ cái bên trong khối chữ cái liên tiếp ngay sau dấu chấm có thể đóng vai trò là vị trí kết thúc. 

Nếu như`starts = number of valid local-part starts`Và`ends = number of valid address endings`thì cái này`@`Và`.`đóng góp`starts × ends`các chuỗi con hợp lệ. 

Tất cả thông tin cần thiết có thể được tính toán trước bằng quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét từ trái sang phải và duy trì thông tin về khối ký tự tối đa được phép trong phần cục bộ, cụ thể là chữ cái, chữ số và`_`. 

Đối với mỗi vị trí, hãy lưu trữ số lượng chữ cái xuất hiện trong khối hiện tại kết thúc ở vị trí đó. 
2. Đối với mọi`@`vị trí, tính toán`start_count`. 

Giá trị này bằng số chữ cái trong khối ký tự cục bộ tối đa kết thúc ngay trước ký tự`@`. 

Mỗi chữ cái như vậy có thể được chọn làm ký tự đầu tiên của phần cục bộ. 
3. Quét từ trái sang phải và tính vị trí bắt đầu của khối chữ và số tối đa hiện tại. 

Đối với mỗi vị trí chữ và số`i`, lưu trữ ranh giới bên trái của khối chứa`i`. 
4. Quét từ phải sang trái và tính toán`letter_len[i]`, độ dài của khối chữ cái liên tiếp bắt đầu từ vị trí`i`. 
5. Lặp lại từng vị trí dấu chấm`j`. 
6. Thành phần cuối cùng phải bắt đầu ngay sau dấu chấm, vì vậy nếu`j + 1`không phải là một chữ cái, hãy bỏ qua dấu chấm này. 
7. Tên miền phải không trống và không có chữ và số, vì vậy`j - 1`phải là chữ và số. Nếu không, hãy bỏ qua dấu chấm này. 
8. Hãy để`t`là phần bắt đầu của khối chữ và số tối đa kết thúc tại`j - 1`. 

Điều duy nhất có thể`@`đang ở vị trí`t - 1`. 
9. Nếu vị trí đó tồn tại và chứa`@`, thêm vào`start_count[@] × letter_len[j + 1]`để trả lời. 
10. Xuất ra tổng số tích lũy. 

### Tại sao nó hoạt động 

Đối với mỗi chuỗi con e-mail hợp lệ, phần tên miền nằm giữa`@`Và`.`là một khối chữ và số không trống. Khối chữ và số tối đa kết thúc trước dấu chấm xác định duy nhất vị trí của`@`phải đứng, tức là ngay trước khối đó. 

Ngược lại, bất cứ khi nào một dấu chấm có phần trước như vậy`@`, mọi phần bắt đầu phần cục bộ hợp lệ và mọi phần kết thúc bằng chữ cái cuối cùng hợp lệ đều tạo ra một chuỗi con hợp lệ riêng biệt. 

Thuật toán tính chính xác những kết hợp đó và không có gì khác. Mỗi e-mail hợp lệ đóng góp một lần và mọi kết hợp được tính đều đáp ứng tất cả các yêu cầu về định dạng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    def is_letter(c):
        return 'a' <= c <= 'z'

    def is_digit(c):
        return '0' <= c <= '9'

    def is_alnum(c):
        return is_letter(c) or is_digit(c)

    def is_local(c):
        return is_alnum(c) or c == '_'

    local_letters = [0] * n
    cur = 0

    for i, ch in enumerate(s):
        if is_local(ch):
            if i > 0 and is_local(s[i - 1]):
                cur += 1 if is_letter(ch) else 0
            else:
                cur = 1 if is_letter(ch) else 0
            local_letters[i] = cur
        else:
            cur = 0

    start_count = [0] * n
    for i, ch in enumerate(s):
        if ch == '@' and i > 0:
            start_count[i] = local_letters[i - 1]

    run_start = [0] * n
    start = 0

    for i, ch in enumerate(s):
        if is_alnum(ch):
            if i > 0 and is_alnum(s[i - 1]):
                run_start[i] = start
            else:
                start = i
                run_start[i] = start

    letter_len = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        if is_letter(s[i]):
            letter_len[i] = letter_len[i + 1] + 1

    ans = 0

    for j, ch in enumerate(s):
        if ch != '.':
            continue

        if j + 1 >= n or letter_len[j + 1] == 0:
            continue

        if j == 0 or not is_alnum(s[j - 1]):
            continue

        t = run_start[j - 1]
        at = t - 1

        if at >= 0 and s[at] == '@':
            ans += start_count[at] * letter_len[j + 1]

    print(ans)

if __name__ == "__main__":
    solve()
```Lần quét đầu tiên tính toán, đối với mỗi vị trí, có bao nhiêu chữ cái thuộc khối ký tự cục bộ hiện tại. Điều này trực tiếp cung cấp số lượng vị trí bắt đầu phần cục bộ hợp lệ cho bất kỳ`@`. 

Lần quét thứ hai ghi lại thời điểm bắt đầu của mỗi lần chạy chữ và số. Đây là điều cho phép chúng tôi khôi phục lại điều duy nhất có thể`@`cho một dấu chấm nhất định. 

Quá trình quét ngược sẽ tính toán độ dài của khối chữ cái liên tiếp bắt đầu từ mỗi vị trí. Nếu một dấu chấm được theo sau bởi một khối chữ cái có độ dài`k`, thì có chính xác`k`vị trí kết thúc hợp lệ. 

Phần tế nhị nhất là tìm ứng viên`@`. Vị trí đúng không phải là tùy ý. Nó phải đứng ngay trước khối chữ và số tối đa kết thúc tại`j - 1`. Thiếu quan sát này thường dẫn đến hành vi bậc hai hoặc đếm không chính xác. 

Tất cả các số đếm đều vừa vặn với số nguyên 64 bit, nhưng không nhất thiết phải bằng số nguyên 32 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
agapov@r1.com
```Các giá trị liên quan: 

| Vị trí chấm | Ứng viên @ | Bắt đầu tại địa phương | Kết thúc thư | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 9 | 6 | 6 | 3 | 18 | 

Trả lời:```
18
```Sáu chữ cái trong`agapov`tất cả đều có thể được chọn làm điểm bắt đầu của phần địa phương. Ba chữ cái trong`com`đều có thể được chọn làm vị trí kết thúc. Mỗi sự kết hợp tạo ra một chuỗi con hợp lệ riêng biệt. 

### Ví dụ 2 

đầu vào:```
ab@c.de
```| Vị trí chấm | Ứng viên @ | Bắt đầu tại địa phương | Kết thúc thư | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 4 | 2 | 2 | 2 | 4 | 

Trả lời:```
4
```Phần cục bộ có thể bắt đầu tại`a`hoặc`b`. Địa chỉ có thể kết thúc tại`d`hoặc`e`. Điều đó mang lại bốn chuỗi con hợp lệ. 

Ví dụ này thể hiện cấu trúc nhân của công thức đếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Số lần quét tuyến tính không đổi | 
| Không gian | O(n) | Mảng lưu trữ thông tin chạy | 

Độ dài chuỗi có thể đạt tới một triệu ký tự. Một giải pháp tuyến tính chỉ thực hiện vài triệu thao tác đơn giản và dễ dàng phù hợp với thời gian giới hạn. Các mảng phụ cũng có kích thước tuyến tính và vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io():
    input = sys.stdin.readline
    s = input().strip()
    n = len(s)

    def is_letter(c):
        return 'a' <= c <= 'z'

    def is_digit(c):
        return '0' <= c <= '9'

    def is_alnum(c):
        return is_letter(c) or is_digit(c)

    def is_local(c):
        return is_alnum(c) or c == '_'

    local_letters = [0] * n
    cur = 0

    for i, ch in enumerate(s):
        if is_local(ch):
            if i > 0 and is_local(s[i - 1]):
                cur += 1 if is_letter(ch) else 0
            else:
                cur = 1 if is_letter(ch) else 0
            local_letters[i] = cur
        else:
            cur = 0

    start_count = [0] * n
    for i, ch in enumerate(s):
        if ch == '@' and i > 0:
            start_count[i] = local_letters[i - 1]

    run_start = [0] * n
    start = 0

    for i, ch in enumerate(s):
        if is_alnum(ch):
            if i > 0 and is_alnum(s[i - 1]):
                run_start[i] = start
            else:
                start = i
                run_start[i] = start

    letter_len = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        if is_letter(s[i]):
            letter_len[i] = letter_len[i + 1] + 1

    ans = 0

    for j, ch in enumerate(s):
        if ch != '.':
            continue

        if j + 1 >= n or letter_len[j + 1] == 0:
            continue

        if j == 0 or not is_alnum(s[j - 1]):
            continue

        t = run_start[j - 1]
        at = t - 1

        if at >= 0 and s[at] == '@':
            ans += start_count[at] * letter_len[j + 1]

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve_io()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("agapov@r1.com\n") == "18", "sample 1"

# custom cases
assert run("a\n") == "0", "minimum size"
assert run("a@b.c\n") == "1", "smallest valid email"
assert run("1@b.c\n") == "0", "local part must start with a letter"
assert run("ab@c.de\n") == "4", "multiple starts and endings"
assert run("ab_cd@x.yz\n") == "4", "underscore allowed inside local part"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`0`| Chuỗi kích thước tối thiểu | 
|`a@b.c`|`1`| Địa chỉ hợp lệ nhỏ nhất | 
|`1@b.c`|`0`| Phần cục bộ không thể bắt đầu bằng một chữ số | 
|`ab@c.de`|`4`| Nhiều lần bắt đầu và kết thúc hợp lệ | 
|`ab_cd@x.yz`|`4`| Xử lý dấu gạch dưới ở phần cục bộ | 

## Vỏ cạnh 

Hãy xem xét:```
1@a.b
```Khối ký tự cục bộ tối đa trước`@`không chứa chữ cái. Thuật toán lưu trữ`start_count = 0`, do đó, mặc dù tên miền và hậu tố hợp lệ nhưng mức đóng góp bằng không. Đầu ra là chính xác:```
0
```Coi như:```
a@12.xyz
```khối`12`là đầy đủ chữ và số. Dấu chấm tìm thấy dấu chấm duy nhất đứng trước`@`,`start_count = 1`, và độ dài hậu tố là`3`. Câu trả lời trở thành:```
3
```tương ứng với kết thúc tại`x`,`y`, Và`z`. 

Coi như:```
a@b.cd3
```Khối chữ liên tiếp sau dấu chấm có độ dài`2`, cụ thể là`cd`. chữ số`3`không phải là một phần của thành phần cuối cùng. Thuật toán sử dụng`letter_len[j + 1] = 2`, tạo ra chính xác hai kết thúc hợp lệ. 

Coi như:```
ab_cd@x.yz
```Trước`@`, khối ký tự cục bộ tối đa chứa các chữ cái`a`Và`b`. Cả hai đều là vị trí bắt đầu hợp lệ. Sau dấu chấm là khối chữ có độ dài`2`. Thuật toán tính toán`2 × 2 = 4`, khớp với bốn chuỗi con hợp lệ.
