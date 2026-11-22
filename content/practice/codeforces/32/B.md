---
title: "CF 32B - Borze"
description: "Chúng ta được cung cấp một chuỗi được viết bằng hệ thống mã hóa Borze. Mỗi chữ số của số ba ngôi được biểu diễn bằng một trong ba mẫu sau:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 32
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 32 (Div. 2, Codeforces format)"
rating: 800
weight: 32
solve_time_s: 77
verified: true
draft: false
---
[CF 32B - Borze](https://codeforces.com/problemset/problem/32/B) 

**Đánh giá:** 800 
**Tags:** phân tích cú pháp biểu thức, triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi được viết bằng hệ thống mã hóa Borze. Mỗi chữ số của số ba ngôi được biểu diễn bằng một trong ba mẫu sau: 

-`"."`đại diện cho`0`-`"-."`đại diện cho`1`-`"--"`đại diện cho`2`Nhiệm vụ là giải mã toàn bộ chuỗi và in số thứ ba thu được. 

Chi tiết quan trọng là mã hóa an toàn tiền tố. Một dấu chấm luôn đứng một mình như`0`, trong khi dấu gạch ngang phải luôn được ghép với ký tự tiếp theo. Điều đó có nghĩa là bất cứ khi nào chúng ta gặp phải`'-'`, chúng ta biết ngay chữ số hiện tại sử dụng hai ký tự. 

Độ dài đầu vào tối đa là 200 ký tự, rất nhỏ. Ngay cả một giải pháp bậc hai cũng sẽ phù hợp một cách thoải mái trong giới hạn thời gian, vì tệ nhất là nó sẽ xử lý khoảng 40.000 phép tính ký tự. Tuy nhiên, cấu trúc mã hóa tự nhiên dẫn đến việc quét tuyến tính đơn giản, do đó không có lý do gì để thực hiện thêm công việc. 

Nguyên nhân chính của lỗi là do di chuyển con trỏ không chính xác trong khi phân tích chuỗi. 

Hãy xem xét đầu vào:```
--.
```Giải mã đúng là:```
20
```Việc thực hiện bất cẩn có thể đọc được điều đầu tiên`'-'`một mình và không thể kết hợp nó với ký tự tiếp theo. Quy tắc mã hóa yêu cầu mỗi dấu gạch ngang phải có hai ký tự. 

Một lỗi dễ xảy ra khác khi duyệt qua chuỗi sau khi giải mã mã thông báo gồm hai ký tự. 

Ví dụ:```
-.--
```Đầu ra đúng:```
12
```Nếu chúng ta giải mã`"-."`BẰNG`1`nhưng chỉ tiến về phía trước một vị trí thay vì hai vị trí, ký tự thứ hai`'.'`được xử lý lại và tạo ra kết quả sai. 

Trường hợp cạnh cuối cùng là một chuỗi được tạo hoàn toàn bằng các dấu chấm:```
...
```Đầu ra đúng:```
000
```Các số 0 đứng đầu phải được giữ nguyên chính xác như đã được giải mã. Việc chuyển đổi kết quả thành một số nguyên sẽ thu gọn nó thành một số nguyên một cách không chính xác.`0`. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là thử liên tục khớp tiền tố hiện tại với ba ký hiệu Borze hợp lệ. Tại mỗi vị trí, chúng tôi kiểm tra xem chuỗi con có bắt đầu bằng`"."`,`"-."`, hoặc`"--"`, nối thêm chữ số tương ứng và tiếp tục. 

Điều này hoạt động vì các quy tắc mã hóa rõ ràng. Không có ký hiệu hợp lệ nào có thể bị nhầm lẫn với ký hiệu khác khi chúng tôi kiểm tra ký tự hiện tại và ký tự tiếp theo khi cần thiết. 

Ngay cả khi được triển khai không hiệu quả bằng cách tạo chuỗi con ở mỗi bước, đầu vào rất nhỏ nên giải pháp vẫn được thực hiện một cách thoải mái. Với độ dài tối đa là 200, các chuỗi cắt liên tục vẫn sẽ hoạt động tốt sau vài chục nghìn thao tác. 

Quan sát giúp đơn giản hóa vấn đề hơn nữa là thực sự chỉ có hai tình huống: 

- Nếu ký tự hiện tại là`'.'`, chữ số trả lời chắc chắn là`0`. 
- Nếu ký tự hiện tại là`'-'`, chúng ta chỉ cần kiểm tra ký tự tiếp theo: 

-`"-."`có nghĩa`1`-`"--"`có nghĩa`2`Điều đó cho phép chúng tôi xử lý chuỗi trong một lần quét từ trái sang phải bằng con trỏ chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi Borze. 
2. Tạo chuỗi kết quả trống và khởi tạo chỉ mục`i = 0`. 
3. Trong khi`i`nằm bên trong chuỗi: 

1. Nếu`s[i]`là`'.'`, nối thêm`'0'`đến câu trả lời và di chuyển`i`chuyển tiếp 1. 
2. Nếu không thì ký tự hiện tại là`'-'`, vì vậy chữ số phải sử dụng hai ký tự. 

- Nếu như`s[i + 1]`là`'.'`, nối thêm`'1'`. 
- Nếu không thì nối thêm`'2'`. 

Di chuyển`i`chuyển tiếp 2. 
4. In chuỗi câu trả lời đã xây dựng. 

### Tại sao nó hoạt động 

Quy tắc mã hóa phân chia đầu vào thành các mã thông báo hợp lệ không chồng chéo. 

Dấu chấm luôn tạo thành mã thông báo`"."`và dấu gạch ngang luôn bắt đầu`"-."`hoặc`"--"`. Vì điều này, mã thông báo chính xác ở vị trí`i`được xác định duy nhất bởi ký tự hiện tại và có thể là ký tự tiếp theo. 

Thuật toán luôn tiêu thụ chính xác các ký tự thuộc chữ số hiện tại, không bao giờ bỏ qua ký tự và không bao giờ xử lý một ký tự hai lần. Sau mỗi bước, mọi tiền tố được xử lý đều đã được giải mã chính xác. Khi quá trình quét kết thúc, toàn bộ chuỗi đã được giải mã. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

ans = []
i = 0

while i < len(s):
    if s[i] == '.':
        ans.append('0')
        i += 1
    else:
        if s[i + 1] == '.':
            ans.append('1')
        else:
            ans.append('2')
        i += 2

print(''.join(ans))
```Giải pháp giữ một con trỏ`i`đại diện cho vị trí chưa đọc hiện tại trong chuỗi Borze. 

Khi ký tự hiện tại là`'.'`, việc lập bản đồ là ngay lập tức. Chúng tôi nối thêm`'0'`và tiến lên một bước vì mã thông báo này sử dụng chính xác một ký tự. 

Khi ký tự hiện tại là`'-'`, chúng ta phải kiểm tra ký tự tiếp theo để phân biệt giữa`"-."`Và`"--"`. Cả hai trường hợp đều sử dụng hai ký tự, do đó con trỏ tăng thêm 2. 

Câu trả lời được lưu trữ trong danh sách thay vì nối các chuỗi liên tục. Đối với vấn đề này, một trong hai cách tiếp cận sẽ thành công, nhưng việc tích lũy danh sách theo sau là`''.join()`là mẫu hiệu quả tiêu chuẩn trong Python. 

Quyền truy cập ranh giới`s[i + 1]`luôn an toàn vì đầu vào được đảm bảo hợp lệ. Một dấu vết đơn độc`'-'`không bao giờ có thể xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
.-.--
```| Bước | tôi | Nhân vật hiện tại | Chữ số được giải mã | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 |`.`|`0`|`0`| 
| 2 | 1 |`-.`|`1`|`01`| 
| 3 | 3 |`--`|`2`|`012`| 

Đầu ra cuối cùng:```
012
```Dấu vết này cho thấy cách thuật toán chuyển đổi giữa mã thông báo một ký tự và hai ký tự mà không có sự mơ hồ. 

### Ví dụ 2 

đầu vào:```
--..-.
```| Bước | tôi | Nhân vật hiện tại | Chữ số được giải mã | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 |`--`|`2`|`2`| 
| 2 | 2 |`.`|`0`|`20`| 
| 3 | 3 |`.`|`0`|`200`| 
| 4 | 4 |`-.`|`1`|`2001`| 

Đầu ra cuối cùng:```
2001
```Ví dụ này minh họa các mã thông báo một ký tự liên tiếp sau mã thông báo hai ký tự. Chuyển động của con trỏ vẫn chính xác vì mỗi nhánh tiến lên chính xác theo độ dài mã thông báo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Câu trả lời được giải mã được lưu trữ riêng | 

Với độ dài đầu vào tối đa chỉ 200 ký tự, giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()

    ans = []
    i = 0

    while i < len(s):
        if s[i] == '.':
            ans.append('0')
            i += 1
        else:
            if s[i + 1] == '.':
                ans.append('1')
            else:
                ans.append('2')
            i += 2

    print(''.join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run(".-.--\n") == "012", "sample 1"

# minimum size input
assert run(".\n") == "0", "single dot"

# only two-character tokens
assert run("------\n") == "222", "all twos"

# alternating token sizes
assert run(".--.-.\n") == "021", "mixed parsing"

# leading zeroes preserved
assert run("...\n") == "000", "leading zeroes"

# boundary condition with final two-character token
assert run(".--\n") == "02", "ending with double dash"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`.`|`0`| Đầu vào hợp lệ tối thiểu | 
|`------`|`222`| Mã thông báo hai ký tự liên tiếp | 
|`.--.-.`|`021`| Độ dài mã thông báo hỗn hợp | 
|`...`|`000`| Bảo quản bằng không hàng đầu | 
|`.--`|`02`| Xử lý đúng mã thông báo cuối cùng | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
--.
```Thuật toán bắt đầu tại chỉ mục`0`và nhìn thấy`'-'`. Nó kiểm tra ký tự tiếp theo và tìm ký tự khác`'-'`, vì vậy nó nối thêm`2`và thăng tiến 2 bậc. Hiện nay`i = 2`, trong đó ký tự còn lại là`'.'`, sản xuất`0`. 

Câu trả lời cuối cùng là:```
20
```Điều này xác nhận rằng mã thông báo hai ký tự được sử dụng cùng nhau và không bao giờ được xử lý một phần. 

Bây giờ hãy xem xét:```
-.--
```Tại`i = 0`, chuỗi con`"-."`trở thành`1`, và con trỏ nhảy trực tiếp đến`i = 2`. Chuỗi con còn lại`"--"`trở thành`2`. 

Đầu ra là:```
12
```Nếu con trỏ chỉ tiến lên một vị trí sau khi giải mã`"-."`, ký tự thứ hai sẽ được phân tích cú pháp lại và làm hỏng kết quả. 

Cuối cùng, hãy xem xét:```
...
```Mỗi dấu chấm ánh xạ độc lập tới`0`, sản xuất:```
000
```Thuật toán lưu trữ kết quả dưới dạng một chuỗi, do đó các số 0 đứng đầu vẫn được giữ nguyên. Chuyển đổi qua số nguyên sẽ làm mất thông tin một cách không chính xác.
