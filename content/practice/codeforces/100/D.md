---
title: "CF 100D - Thế Giới Miệng"
description: "Chúng ta bắt đầu với một chuỗi ban đầu và chuyển nó quanh một vòng tròn gồm n người. Mỗi người chỉ được phép sửa đổi chuỗi theo một trong hai cách: 1. Xóa chính xác một ký tự ở cuối. 2. Thêm chính xác một ký tự vào cuối. Một người cũng có thể chọn không làm gì cả."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "strings"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "D"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1500
weight: 100
solve_time_s: 138
verified: true
draft: false
---

[CF 100D - Thế giới miệng](https://codeforces.com/problemset/problem/100/D) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt, dây 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một chuỗi ban đầu và chuyển nó quanh một vòng tròn`n`mọi người. Mỗi người chỉ được phép sửa đổi chuỗi theo một trong hai cách: 

1. Xóa chính xác một ký tự ở cuối. 
2. Thêm chính xác một ký tự vào cuối. 

Một người cũng có thể chọn không làm gì cả. 

Sau chính xác`n`truyền đi, chúng ta thu được chuỗi cuối cùng. Chúng ta phải quyết định liệu chuỗi cuối cùng có thể được tạo ra một cách hợp pháp từ chuỗi ban đầu hay không. 

Chi tiết quan trọng là mọi thao tác chỉ ảnh hưởng đến hậu tố của chuỗi. Các nhân vật ở giữa không bao giờ được chạm trực tiếp. Phần có thể thay đổi duy nhất là phần cuối hiện tại của chuỗi. 

Những hạn chế lớn một cách bất thường. Số lần di chuyển có thể đạt tới`8 * 10^6`và mỗi chuỗi có thể có độ dài lên tới`10^7`. Bất kỳ thuật toán nào có hành vi bậc hai đều không thể thực hiện được. Ngay cả các thuật toán tuyến tính cũng phải tránh các bản sao không cần thiết hoặc các lần quét lặp lại. Giải pháp xây dựng lại chuỗi nhiều lần hoặc mô phỏng từng thao tác một sẽ hết thời gian hoặc hết bộ nhớ. 

Kích thước đầu vào lớn cũng buộc chúng ta phải suy nghĩ cẩn thận về những gì thực sự thay đổi trong quá trình này. Vì các thao tác chỉ diễn ra ở cuối nên thứ tự tương đối của các ký tự được giữ nguyên không bao giờ thay đổi. Sự quan sát đó là toàn bộ vấn đề. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
n = 2
initial = abc
final = abc
```Câu trả lời đúng là`Yes`. Cả hai người có thể đơn giản là không làm gì cả. Một giải pháp bất cẩn cho rằng mọi bước di chuyển đều phải sửa đổi chuỗi sẽ từ chối điều này một cách không chính xác. 

Bây giờ hãy xem xét:```
n = 1
initial = abc
final = ab
```Câu trả lời đúng là`Yes`. Một lần xóa là đủ. Nhưng:```
n = 1
initial = abc
final = a
```Câu trả lời đúng là`No`, bởi vì một lần di chuyển có thể thay đổi độ dài tối đa một. 

Một trường hợp tinh vi khác là khi một chuỗi là tiền tố của chuỗi kia:```
n = 3
initial = abc
final = abcde
```Câu trả lời là`Yes`. Chúng ta chỉ cần hai thao tác nối thêm và thao tác còn lại có thể không được sử dụng. 

Nhưng điều này không thành công:```
n = 1
initial = abc
final = abcde
```Câu trả lời là`No`, vì việc tăng thêm hai ký tự đòi hỏi ít nhất hai thao tác. 

Trường hợp cạnh cấu trúc quan trọng nhất là khi các chuỗi không đồng ý trước khi chuỗi ngắn hơn kết thúc:```
n = 10
initial = abcd
final = abxd
```Câu trả lời là`No`. Vì chỉ có mục đích mới có thể thay đổi nên chúng ta không bao giờ có thể thay đổi được`c`vào trong`x`mà không cần xóa mọi thứ sau nó. Phần được bảo toàn phải luôn là tiền tố chung. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Ở mỗi bước, chúng ta có thể thêm một ký tự, xóa ký tự cuối cùng hoặc không làm gì cả. Ngay cả khi chúng tôi hạn chế các lựa chọn nối thêm vào các chữ cái xuất hiện trong chuỗi mục tiêu thì hệ số phân nhánh vẫn rất lớn. Sau đó`n`bước, số lượng các trạng thái có thể có sẽ trở thành hàm mũ. 

Một cải tiến mạnh mẽ khác là suy nghĩ ngược lại từ chuỗi cuối cùng. Chúng tôi có thể thử mọi cách xóa và bổ sung có thể. Điều này vẫn không khả thi vì chuỗi có thể chứa hàng triệu ký tự. 

Lý do khiến vũ lực cảm thấy hấp dẫn là vì mỗi động tác đều đơn giản. Nhưng số lượng các chuỗi chuyển động là rất lớn, trong khi sự tự do về cấu trúc thực tế lại rất nhỏ. 

Quan sát quan trọng là các hoạt động chỉ ảnh hưởng đến hậu tố. Điều đó có nghĩa là mọi ký tự trước lần không khớp đầu tiên phải được giữ nguyên mãi mãi. Vì vậy, việc chuyển đổi có thể thực hiện được khi và chỉ khi hai chuỗi có chung một số tiền tố chung và mọi thứ sau tiền tố đó có thể bị xóa và xây dựng lại nhiều nhất trong phạm vi đó.`n`hoạt động. 

Giả sử độ dài tiền tố chung dài nhất là`L`. 

Để biến đổi:```
initial -> final
```chúng ta phải: 

1. Xóa cái cuối cùng`len(initial) - L`nhân vật. 
2. Nối cuối cùng`len(final) - L`nhân vật. 

Các hoạt động cần thiết tối thiểu là:```
(len(initial) - L) + (len(final) - L)
```Nếu giá trị này lớn nhất`n`, thì chúng ta có thể thực hiện thêm các bước di chuyển mà không cần làm gì cả, vì mỗi người được phép giữ nguyên chuỗi. 

Vì vậy, toàn bộ vấn đề giảm xuống việc tính toán tiền tố chung dài nhất và kiểm tra xem số lượng chỉnh sửa cần thiết có phù hợp với bên trong không`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(phút( | s | , | 

## Hướng dẫn thuật toán 

1. Đọc`n`, chuỗi ban đầu`s`, và chuỗi cuối cùng`t`. 
2. Quét cả hai chuỗi từ trái sang phải trong khi các ký tự khớp nhau. 

Duy trì một chỉ mục`L`, độ dài của tiền tố chung dài nhất. 

Khi chúng tôi gặp phải sự không khớp đầu tiên, không có ký tự nào sau này có thể được giữ nguyên vì các thao tác chỉ sửa đổi hậu tố. 
3. Tính xem cần xóa bao nhiêu lần. 

Chúng ta phải loại bỏ mọi ký tự trong`s`sau vị trí`L`. 

Số đó là:```
len(s) - L
```4. Tính xem cần bao nhiêu thao tác nối thêm. 

Chúng ta phải xây dựng hậu tố còn lại của`t`. 

Số đó là:```
len(t) - L
```5. Thêm hai giá trị để có được các hoạt động cần thiết tối thiểu. 
6. Nếu tối đa các hoạt động được yêu cầu tối thiểu`n`, in`Yes`. 

Nếu không thì in`No`. 

### Tại sao nó hoạt động 

Phần được bảo toàn của chuỗi phải luôn là tiền tố vì mọi thao tác xảy ra ở cuối. Khi hai chuỗi khác nhau ở một vị trí nào đó, mọi ký tự sau trong chuỗi gốc cuối cùng phải bị xóa trước khi hậu tố đích có thể được tạo. 

Do đó, tiền tố chung dài nhất là phần tối đa chúng ta có thể giữ nguyên. Mọi phép biến đổi hợp lệ đều phải xóa phần còn lại của chuỗi ban đầu và nối thêm phần còn lại của chuỗi đích. Điều đó đưa ra số lượng thao tác cần thiết tối thiểu và bất kỳ bước di chuyển bổ sung nào cũng có thể khiến chuỗi không thay đổi. Vậy điều kiện vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()
    t = input().strip()

    l = 0
    limit = min(len(s), len(t))

    while l < limit and s[l] == t[l]:
        l += 1

    operations = (len(s) - l) + (len(t) - l)

    if operations <= n:
        print("Yes")
    else:
        print("No")

solve()
```Việc thực hiện phản ánh trực tiếp đối số toán học. 

Vòng lặp tính toán tiền tố chung dài nhất mà không tạo chuỗi con. Điều đó quan trọng vì chuỗi có thể chứa tới mười triệu ký tự. Việc cắt lặp đi lặp lại sẽ phân bổ lượng bộ nhớ khổng lồ và làm chậm chương trình một cách đáng kể. 

Sau vòng lặp, mỗi ký tự sau chỉ mục`l`trong chuỗi gốc phải được loại bỏ và mọi ký tự sau chỉ mục`l`trong chuỗi đích phải được thêm vào. Công thức cho`operations`đến trực tiếp từ quan sát đó. 

Việc so sánh sử dụng`<= n`, không`== n`. Đây là một nguồn sai lầm phổ biến. Mọi người được phép giữ nguyên chuỗi trong khi di chuyển, vì vậy việc có thêm các bước di chuyển chưa sử dụng là hoàn toàn hợp lệ. 

Một chi tiết tinh tế khác là sử dụng`strip()`khi đọc chuỗi. Các dòng đầu vào chứa các dòng mới ở cuối mà không được trở thành một phần của chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
100
Codeforces
MMIODPC
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi ban đầu |`Codeforces`| 
| Chuỗi cuối cùng |`MMIODPC`| 
| Độ dài tiền tố chung dài nhất |`0`| 
| Cần xóa |`10`| 
| Cần bổ sung |`7`| 
| Tổng số hoạt động |`17`| 
| So sánh với`n=100`|`17 <= 100`| 

Đầu ra:```
Yes
```Ví dụ này cho thấy rằng những bước đi thêm không quan trọng. Chúng tôi chỉ cần 17 sửa đổi và 83 người còn lại có thể truyền chuỗi không thay đổi. 

### Ví dụ 2 

đầu vào:```
2
abcd
abxy
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi ban đầu |`abcd`| 
| Chuỗi cuối cùng |`abxy`| 
| Độ dài tiền tố chung dài nhất |`2`| 
| Cần xóa |`2`| 
| Cần bổ sung |`2`| 
| Tổng số hoạt động |`4`| 
| So sánh với`n=2`|`4 > 2`| 

Đầu ra:```
No
```Điều này chứng tỏ tại sao chỉ có tiền tố chung mới có thể tồn tại. Hậu tố`cd`phải xóa trước`xy`có thể được nối thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(phút( | s | 
| Không gian | O(1) thêm | Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp thoải mái phù hợp với giới hạn. Ngay cả với các chuỗi có độ dài mười triệu, một lần quét tuyến tính duy nhất vẫn khả thi trong Python khi được triển khai cẩn thận mà không cần tạo chuỗi con hoặc nối lặp lại. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()
    t = input().strip()

    l = 0
    limit = min(len(s), len(t))

    while l < limit and s[l] == t[l]:
        l += 1

    operations = (len(s) - l) + (len(t) - l)

    if operations <= n:
        print("Yes")
    else:
        print("No")

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
assert run(
"""100
Codeforces
MMIODPC
"""
) == "Yes", "sample 1"

# identical strings, zero operations needed
assert run(
"""2
abc
abc
"""
) == "Yes", "same strings"

# not enough operations
assert run(
"""1
abc
a
"""
) == "No", "requires two deletions"

# pure append
assert run(
"""3
abc
abcde
"""
) == "Yes", "two appends fit"

# mismatch inside string
assert run(
"""2
abcd
abxy
"""
) == "No", "needs four operations"

# full replacement
assert run(
"""6
aaaa
bbbb
"""
) == "No", "needs eight operations"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc -> abc`|`Yes`| Các bước di chuyển bổ sung có thể không được sử dụng | 
|`abc -> a`với`n=1`|`No`| Chỉ riêng sự khác biệt về chiều dài đã có thể vượt quá giới hạn | 
|`abc -> abcde`với`n=3`|`Yes`| Hoạt động nối thêm thuần túy | 
|`abcd -> abxy`với`n=2`|`No`| Nội bộ không khớp yêu cầu xây dựng lại hậu tố | 
|`aaaa -> bbbb`với`n=6`|`No`| Chi phí thay thế hoàn chỉnh | 

## Vỏ cạnh 

Hãy xem xét:```
1
abc
ab
```Tiền tố chung dài nhất là`ab`, Vì thế`L = 2`. 

Chúng tôi cần:```
3 - 2 = 1 deletion
2 - 2 = 0 additions
```Tổng số hoạt động =`1`. 

Từ`1 <= n`, thuật toán in`Yes`. 

Bây giờ hãy xem xét:```
1
abc
a
```Tiền tố chung dài nhất chỉ là`a`, Vì thế:```
3 - 1 = 2 deletions
1 - 1 = 0 additions
```Tổng số hoạt động =`2`. 

Từ`2 > 1`, thuật toán in chính xác`No`. 

Tiếp theo, kiểm tra trường hợp các chuỗi khác nhau ở giữa:```
10
abcd
abxd
```Tiền tố chung dài nhất là`ab`. 

Thuật toán tính toán:```
4 - 2 = 2 deletions
4 - 2 = 2 additions
```Tổng số hoạt động =`4`. 

Mặc dù các chuỗi có độ dài bằng nhau nhưng việc thay đổi`c`vào trong`x`vẫn yêu cầu xóa hậu tố và xây dựng lại nó. Thuật toán tự động nắm bắt điều này thông qua logic tiền tố chung. 

Cuối cùng, hãy xem xét các chuỗi giống hệt nhau:```
5
hello
hello
```Độ dài tiền tố chung dài nhất là`5`. 

Các thao tác bắt buộc:```
0 deletions
0 additions
```Tổng số hoạt động =`0`. 

Vì cho phép di chuyển không sử dụng nên thuật toán sẽ in chính xác`Yes`.
