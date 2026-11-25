---
title: "CF 99A - Giúp đỡ Vương quốc xa xôi"
description: "Chúng ta được cung cấp một số thập phân dưới dạng một chuỗi. Số chứa phần nguyên, sau đó là dấu chấm, rồi phần phân số. Nhiệm vụ là mô phỏng các quy tắc làm tròn kỳ lạ của vương quốc. Các quy tắc là cố ý không đầy đủ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 99
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 78 (Div. 2 Only)"
rating: 800
weight: 99
solve_time_s: 85
verified: true
draft: false
---

[CF 99A - Giúp đỡ Vương quốc xa xôi](https://codeforces.com/problemset/problem/99/A) 

**Đánh giá:** 800 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số thập phân dưới dạng một chuỗi. Số chứa phần nguyên, sau đó là dấu chấm, rồi phần phân số. Nhiệm vụ là mô phỏng các quy tắc làm tròn kỳ lạ của vương quốc. 

Các quy tắc là cố ý không đầy đủ. Nếu phần nguyên kết thúc bằng chữ số`9`, không ai ngoại trừ Vasilisa có thể xử lý thao tác mang, vì vậy chúng ta phải in:```
GOTO Vasilisa.
```Ngược lại, chúng ta chỉ nhìn vào chữ số đầu tiên sau dấu thập phân. Nếu nó nhỏ hơn`5`, chúng ta làm tròn xuống và in phần nguyên không thay đổi. Nếu ít nhất là`5`, chúng ta tăng phần nguyên lên`1`. 

Độ dài đầu vào có thể lớn tới 1000 ký tự. Điều đó ngay lập tức cho chúng ta biết vấn đề là về xử lý chuỗi chứ không phải tính toán số. Việc chuyển đổi toàn bộ giá trị thành số có dấu phẩy động sẽ nguy hiểm vì các loại dấu phẩy động sẽ mất độ chính xác đối với các đầu vào dài. Ngay cả việc chuyển đổi thành số nguyên bình thường cũng không cần thiết. Chúng ta chỉ cần một vài ký tự trong chuỗi. 

Giới hạn thời gian là rộng rãi cho việc xử lý tuyến tính. Ngay cả việc quét toàn bộ chuỗi một lần cũng không đáng kể với độ dài 1000. Bất kỳ giải pháp nào có độ phức tạp về thời gian O(n) hoặc tốt hơn đều dễ dàng phù hợp. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Đầu tiên là khi phần nguyên kết thúc bằng`9`. 

đầu vào:```
19.1
```Đầu ra đúng:```
GOTO Vasilisa.
```Việc triển khai đơn giản vẫn có thể cố gắng làm tròn số và tạo ra`19`, nhưng tuyên bố rõ ràng cấm xử lý mang trong tình huống này. 

Một trường hợp tinh tế khác là khi phần phân số có nhiều chữ số. 

đầu vào:```
12.499999999
```Đầu ra đúng:```
12
```Chỉ có chữ số đầu tiên sau dấu thập phân mới quan trọng. Các chữ số còn lại phải được bỏ qua. Việc xem toàn bộ giá trị phân số dưới dạng số thực có thể gây ra sự phức tạp không cần thiết. 

Trường hợp cạnh thứ ba là số nguyên nhỏ nhất có thể. 

đầu vào:```
0.5
```Đầu ra đúng:```
1
```Phần nguyên có thể bằng 0, do đó việc triển khai không được vô tình biến kết quả thành một chuỗi trống. 

Một trường hợp quan trọng hơn là khi số này đã gần đến ranh giới mang nhưng vẫn được phép. 

đầu vào:```
8.9
```Đầu ra đúng:```
9
```Phần nguyên không kết thúc bằng`9`, vì vậy việc thêm một là hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ phân tích toàn bộ số thập phân và thực hiện làm tròn thông thường. Một cách triển khai khả thi sẽ sử dụng số học thập phân có độ chính xác tùy ý hoặc so sánh thủ công phần phân số với`0.5`. 

Điều đó hoạt động hợp lý vì quy tắc làm tròn gần như là quy tắc làm tròn tiêu chuẩn. Vấn đề xuất hiện khi chúng ta nhớ hạn chế đặc biệt đối với các phần nguyên kết thúc bằng`9`. Việc triển khai làm tròn số chung sẽ làm tròn không chính xác các giá trị như`19.7`vào trong`20`, mặc dù đầu ra yêu cầu là`"GOTO Vasilisa."`. 

Ý tưởng vũ phu cũng có tác dụng nhiều hơn mức cần thiết. Đầu vào có thể chứa tối đa 1000 ký tự, nhưng câu trả lời chỉ phụ thuộc vào hai ký tự: 

1. Chữ số cuối cùng trước dấu chấm. 
2. Chữ số đầu tiên sau dấu chấm. 

Quan sát đó đưa ra giải pháp tối ưu ngay lập tức. Chúng tôi chia chuỗi xung quanh dấu thập phân, kiểm tra hai vị trí đó và trực tiếp quyết định câu trả lời. 

Nếu phần nguyên kết thúc bằng`9`, chúng tôi in tin nhắn đặc biệt. 

Mặt khác: 

1. Nếu chữ số phân số đầu tiên nhỏ hơn`5`, in phần nguyên. 
2. Nếu ít nhất`5`, in phần nguyên tăng thêm`1`. 

Vì phần nguyên không bao giờ kết thúc bằng`9`trong trường hợp thứ hai, việc thêm một cái không bao giờ gây ra chuỗi mang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 

Số được cung cấp dưới dạng văn bản và việc xử lý số đó dưới dạng văn bản sẽ tránh được các vấn đề về độ chính xác. 
2. Chia chuỗi ở dấu thập phân thành`integer_part`Và`fractional_part`. 

Bài toán đảm bảo chính xác một dấu chấm nên việc chia tách là an toàn. 
3. Kiểm tra chữ số cuối cùng của`integer_part`. 

Nếu chữ số này là`'9'`, in:```
GOTO Vasilisa.
```và dừng lại. 

Tuyên bố nói rõ ràng các số kết thúc bằng`9`không thể làm tròn bình thường. 
4. Nhìn vào chữ số đầu tiên của`fractional_part`. 

Chỉ riêng chữ số này sẽ quyết định liệu chúng ta làm tròn lên hay làm tròn xuống. 
5. Nếu chữ số nhỏ hơn`'5'`, in`integer_part`. 

Số làm tròn xuống. 
6. Nếu không, hãy chuyển đổi`integer_part`thành một số nguyên, cộng một và in kết quả. 

Vì chữ số cuối cùng không phải là`9`, sự bổ sung này không bao giờ tạo ra một chuỗi mang theo. 

### Tại sao nó hoạt động 

Quy tắc làm tròn chỉ phụ thuộc vào việc phần phân số có nằm dưới`0.5`hoặc ít nhất`0.5`. Trong ký hiệu thập phân, sự so sánh đó được xác định hoàn toàn bằng chữ số đầu tiên sau dấu thập phân. 

Nếu chữ số phân số đầu tiên là: 

-`0`bởi vì`4`, giá trị hoàn toàn dưới đây`0.5`. 
-`5`bởi vì`9`, giá trị ít nhất là`0.5`. 

Điều đặc biệt`"GOTO Vasilisa."`điều kiện ghi đè mọi thứ khác bất cứ khi nào phần nguyên kết thúc bằng`9`. Vì thuật toán kiểm tra điều kiện đó trước tiên nên mọi đầu vào có thể tuân theo chính xác bộ quy tắc bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

integer_part, fractional_part = s.split('.')

if integer_part[-1] == '9':
    print("GOTO Vasilisa.")
else:
    if fractional_part[0] < '5':
        print(integer_part)
    else:
        print(int(integer_part) + 1)
```Giải pháp bắt đầu bằng cách đọc số dưới dạng chuỗi. Đây là cách tiếp cận an toàn nhất vì đầu vào có thể rất dài và số học dấu phẩy động là không cần thiết. 

Thao tác phân chia tách biệt phần nguyên và phần phân số một cách rõ ràng. Vì câu lệnh đảm bảo biểu diễn thập phân hợp lệ nên luôn có chính xác một dấu chấm. 

Chi tiết triển khai quan trọng nhất là thứ tự kiểm tra. Chúng ta phải kiểm tra xem phần nguyên có kết thúc bằng`9`trước khi thử làm tròn bất kỳ. Mặt khác, đầu vào như`19.7`sẽ trở thành không chính xác`20`. 

Sự so sánh chống lại`'5'`được thực hiện dưới dạng so sánh ký tự, hoạt động vì các ký tự chữ số được sắp xếp theo thứ tự từ điển theo cùng thứ tự với các giá trị số của chúng. 

Việc bổ sung cuối cùng sử dụng`int(integer_part) + 1`. Điều này là an toàn vì độ dài phần nguyên tối đa là 999 chữ số, Python xử lý một cách tự nhiên với các số nguyên có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0.0
```| Bước | số nguyên_part | phần_phân số | Quyết định | 
| --- | --- | --- | --- | 
| Tách đầu vào |`0`|`0`| Tiếp tục | 
| Kiểm tra chữ số nguyên cuối cùng |`0`|`0`| Không`9`| 
| Kiểm tra chữ số phân số đầu tiên |`0`|`0`| Ít hơn`5`| 
| Đầu ra |`0`| - | Làm tròn xuống | 

Đầu ra:```
0
```Ví dụ này minh họa trường hợp làm tròn xuống đơn giản nhất. Thuật toán bỏ qua tất cả các chi tiết không cần thiết và chỉ sử dụng chữ số phân số đầu tiên. 

### Ví dụ 2 

đầu vào:```
8.9
```| Bước | số nguyên_part | phần_phân số | Quyết định | 
| --- | --- | --- | --- | 
| Tách đầu vào |`8`|`9`| Tiếp tục | 
| Kiểm tra chữ số nguyên cuối cùng |`8`|`9`| Không`9`| 
| Kiểm tra chữ số phân số đầu tiên |`8`|`9`| Ít nhất`5`| 
| Thêm một |`8 + 1`| - | Kết quả trở thành`9`| 

Đầu ra:```
9
```Dấu vết này cho thấy nhánh làm tròn. Vì phần nguyên không kết thúc bằng`9`, thêm một được cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tách và quét chuỗi mất thời gian tuyến tính | 
| Không gian | O(n) | Các chuỗi phân tách lưu trữ các phần đầu vào | 

Độ dài đầu vào tối đa là 1000 ký tự nên việc xử lý tuyến tính là cực kỳ nhỏ. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()

    integer_part, fractional_part = s.split('.')

    if integer_part[-1] == '9':
        print("GOTO Vasilisa.")
    else:
        if fractional_part[0] < '5':
            print(integer_part)
        else:
            print(int(integer_part) + 1)

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
assert run("0.0\n") == "0\n", "sample 1"

# custom cases
assert run("8.9\n") == "9\n", "round up without carry"
assert run("19.1\n") == "GOTO Vasilisa.\n", "integer part ends with 9"
assert run("12.499999\n") == "12\n", "ignore remaining fractional digits"
assert run("0.5\n") == "1\n", "minimum integer rounding up"
assert run("12345678987654321.4\n") == "12345678987654321\n", "large integer round down"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`8.9`|`9`| Làm tròn bình thường | 
|`19.1`|`GOTO Vasilisa.`| Hộp đựng bị cấm đặc biệt | 
|`12.499999`|`12`| Chỉ có chữ số phân số đầu tiên mới quan trọng | 
|`0.5`|`1`| Xử lý đúng số 0 | 
|`12345678987654321.4`|`12345678987654321`| Xử lý số nguyên lớn | 

## Vỏ cạnh 

Hãy xem xét tình huống bị cấm mang theo. 

đầu vào:```
29.8
```Thuật toán đầu tiên trích xuất`integer_part = "29"`Và`fractional_part = "8"`. 

Chữ số cuối cùng của phần nguyên là`'9'`, do đó thuật toán in ngay lập tức:```
GOTO Vasilisa.
```Nó không bao giờ cố gắng làm tròn. Điều này phù hợp chính xác với các quy tắc vấn đề. 

Bây giờ hãy xem xét một phần phân số gây hiểu lầm. 

đầu vào:```
7.499999999
```Thuật toán chỉ kiểm tra chữ số phân số đầu tiên, đó là`'4'`. Từ`'4' < '5'`, kết quả trở thành:```
7
```Mặc dù các chữ số sau đều là`9`, số đó vẫn nhỏ hơn hẳn`7.5`. 

Cuối cùng, hãy xem xét làm tròn từ số 0. 

đầu vào:```
0.7
```Phần nguyên không kết thúc bằng`9`, và chữ số phân số đầu tiên là`'7'`, do đó thuật toán tính toán:```
0 + 1 = 1
```và in:```
1
```Điều này xác nhận việc triển khai xử lý chính xác số nguyên hợp lệ nhỏ nhất.
