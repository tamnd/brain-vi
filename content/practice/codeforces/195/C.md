---
title: "CF 195C - Thử và Bắt"
description: "Chúng ta được cung cấp mã nguồn của một ngôn ngữ nhỏ chỉ chứa ba loại câu lệnh: thử, bắt (loại, thông báo) và chính xác một lần ném (loại). Mỗi lần thử được ghép nối với lần bắt sau, tạo thành khối thử bắt."
date: "2026-06-05T00:36:58+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 195
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 123 (Div. 2)"
rating: 1800
weight: 195
solve_time_s: 143
verified: true
draft: false
---

[CF 195C - Thử và Bắt](https://codeforces.com/problemset/problem/195/C) 

**Đánh giá:** 1800 
**Tags:** phân tích biểu thức, triển khai 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp mã nguồn của một ngôn ngữ nhỏ chỉ chứa ba loại câu lệnh:`try`,`catch(type, message)`, và đúng một`throw(type)`. 

Mỗi`try`được ghép nối với sau này`catch`, tạo thành khối try-catch. Đầu vào được đảm bảo đúng về mặt cú pháp, do đó các khối này được lồng đúng cách. 

Khi một ngoại lệ được ném ra, chúng ta phải xác định khối bắt nào xử lý nó. Khối bắt có thể xử lý ngoại lệ nếu có ba điều kiện: 

1. Tương ứng với nó`try`xuất hiện trước`throw`. 
2. Nó`catch`xuất hiện sau`throw`. 
3. Loại ngoại lệ của nó khớp với loại được ném. 

Trong số tất cả các khối như vậy, ngôn ngữ chọn khối có`catch`tuyên bố xuất hiện sớm nhất sau khi ném. Chúng ta phải in thông báo của khối bắt đó. Nếu không có khối nào khớp, chúng ta in:```
Unhandled Exception
```Chương trình chứa tối đa$10^5$dòng. Điều này ngay lập tức loại trừ mọi giải pháp quét liên tục các phần lớn của chương trình. Một thuật toán bậc hai sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Bản thân việc phân tích cú pháp cũng là một phần của thách thức. Các dòng có thể chứa khoảng trắng tùy ý xung quanh từ khóa, dấu ngoặc đơn, dấu phẩy và dấu ngoặc kép. Giải pháp dựa vào định dạng chính xác sẽ thất bại. 

Một điểm tinh tế là trình xử lý được chọn không nhất thiết phải là trình xử lý xung quanh trong cùng`try`. Ngôn ngữ chọn khối phù hợp có`catch`xảy ra đầu tiên sau khi ném. 

Coi như:```
try
    try
        throw(AE)
    catch(BE,"x")
catch(AE,"y")
```Khối bên trong bao quanh cú ném, nhưng loại ngoại lệ của nó không khớp. Đầu ra đúng là:```
y
```Một sai lầm dễ mắc phải khác là chỉ tìm kiếm trong số các khối vẫn còn "mở" khi đến lượt ném. Định nghĩa của ngôn ngữ phụ thuộc vào vị trí tương đối của`try`,`throw`, Và`catch`, không phải về việc thực hiện mô phỏng. 

Ví dụ:```
try
    throw(AE)
catch(AE,"first")
try
catch(AE,"second")
```Đầu ra đúng là:```
first
```bởi vì lần bắt bóng phù hợp đầu tiên sau khi ném được chọn. 

Trường hợp cạnh thứ ba xảy ra khi không có trình xử lý nào khớp:```
try
    throw(AE)
catch(BE,"wrong")
```Đầu ra phải là:```
Unhandled Exception
```mặc dù cú ném nằm trong khối thử bắt. 

## Phương pháp tiếp cận 

Giải pháp brute-force trước tiên sẽ xây dựng lại tất cả các khối try-catch. Sau đó, sau khi xác định vị trí ném, nó có thể kiểm tra từng khối và kiểm tra xem: 

- khối của`try`là trước khi ném, 
- khối của`catch`là sau cú ném, 
- các loại ngoại lệ phù hợp. 

Trong số tất cả các khối đủ điều kiện, chúng tôi sẽ chọn khối có vị trí bắt nhỏ nhất. 

Cách tiếp cận này đúng vì nó trực tiếp thực hiện đặc tả. Nếu có$m$khối, công việc là$O(m)$, điều này đã được chấp nhận rồi. Khó khăn thực sự là việc xây dựng lại các khối một cách hiệu quả trong khi phân tích cú pháp. 

Việc triển khai ít cẩn thận hơn có thể liên tục tìm kiếm các sản phẩm đánh bắt phù hợp hoặc đi qua nhiều lần các cấu trúc làm tổ, dẫn đến$O(n^2)$hành vi trên đầu vào được lồng sâu. 

Quan sát quan trọng là cấu trúc chương trình được lồng vào nhau một cách hoàn hảo. Mọi`catch`đóng cái chưa từng có gần đây nhất`try`. Đây chính xác là hành vi của một ngăn xếp. 

Trong khi quét chương trình một lần, chúng ta có thể đẩy số dòng của mỗi`try`. Khi một`catch`xuất hiện, nó đóng trên cùng`try`, cho phép chúng ta xây dựng ngay một khối hoàn chỉnh. 

Sau khi thu thập tất cả các khối và xác định vị trí của lần ném duy nhất, việc xác định câu trả lời sẽ trở thành một lần quét tuyến tính đơn giản trên các khối. 

Toàn bộ giải pháp chạy theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với các tìm kiếm lặp đi lặp lại | O(n²) | O(n) | Quá chậm | 
| Phân tích dựa trên ngăn xếp + một lần quét | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng chương trình. 
2. Đối với mỗi dòng, hãy xóa khoảng trắng ở đầu và cuối. 
3. Phát hiện dòng chứa câu lệnh nào. 
4. Khi một`try`được tìm thấy, đẩy chỉ mục dòng của nó vào ngăn xếp. 

Chưa từng có gần đây nhất`try`phải là cái đóng cửa tiếp theo. 
5. Khi một`catch(type,message)`được tìm thấy, hãy bật ngăn xếp. 

Vị trí bật ra là khớp`try`cho lần đánh bắt này. Lưu trữ một bản ghi có chứa: 

- thử vị trí 
- vị trí bắt 
- loại ngoại lệ 
- tin nhắn 
6. Khi một`throw(type)`được tìm thấy, lưu trữ: 

- tư thế ném 
- loại ngoại lệ ném 
7. Sau khi phân tích toàn bộ chương trình, hãy kiểm tra từng khối được ghi lại. 
8. Một khối có thể xử lý ngoại lệ nếu: 

-`try_position < throw_position`-`catch_position > throw_position`-`block_type == thrown_type`9. Trong số tất cả các khối phù hợp, hãy chọn khối có vị trí bắt nhỏ nhất. 
10. In tin nhắn của nó. 
11. Nếu không có khối nào đủ tiêu chuẩn, hãy in:```
Unhandled Exception
```### Tại sao nó hoạt động 

Việc xây dựng lại ngăn xếp là chính xác vì câu lệnh đảm bảo rằng mọi`catch`đóng cái được mở gần đây nhất chưa từng có`try`. Đây chính xác là thuộc tính khớp với dấu ngoặc đơn tiêu chuẩn. 

Do đó, mỗi khối được ghi đều chứa cặp vị trí chính xác. Một khối đủ điều kiện chính xác khi nó`try`là trước khi ném và nó`catch`là sau khi ném, phù hợp với định nghĩa ngôn ngữ. Trong số các khối đủ điều kiện, việc chọn vị trí bắt nhỏ nhất sẽ thực hiện quy tắc kích hoạt lần bắt sớm nhất sau khi ném. Vì mỗi khối có thể được kiểm tra chính xác một lần nên thuật toán không thể bỏ sót trình xử lý chính xác. 

## Giải pháp Python```python
import sys
import re

input = sys.stdin.readline

def solve():
    n = int(input())

    stack = []
    blocks = []

    throw_pos = -1
    throw_type = ""

    catch_re = re.compile(
        r'catch\s*\(\s*([A-Za-z]+)\s*,\s*"([^"]+)"\s*\)'
    )
    throw_re = re.compile(
        r'throw\s*\(\s*([A-Za-z]+)\s*\)'
    )

    for i in range(n):
        line = input().strip()

        if not line:
            continue

        if line.startswith("try"):
            stack.append(i)

        elif line.startswith("throw"):
            m = throw_re.match(line)
            throw_type = m.group(1)
            throw_pos = i

        elif line.startswith("catch"):
            m = catch_re.match(line)
            exc_type = m.group(1)
            message = m.group(2)

            try_pos = stack.pop()
            blocks.append((try_pos, i, exc_type, message))

    answer = None
    best_catch_pos = n + 1

    for try_pos, catch_pos, exc_type, message in blocks:
        if (
            try_pos < throw_pos < catch_pos
            and exc_type == throw_type
        ):
            if catch_pos < best_catch_pos:
                best_catch_pos = catch_pos
                answer = message

    if answer is None:
        print("Unhandled Exception")
    else:
        print(answer)

solve()
```Phần đầu tiên của mã phân tích cú pháp chương trình trong khi xây dựng lại các khối thử bắt. Các cửa hàng ngăn xếp chưa từng có`try`các vị trí. Bất cứ khi nào một`catch`xuất hiện, phần trên cùng của ngăn xếp là kết quả khớp hợp lệ duy nhất`try`, vì vậy chúng tôi ngay lập tức tạo một bản ghi khối hoàn chỉnh. 

Biểu thức chính quy xử lý khoảng cách tùy ý. Điều này tránh được nhiều lỗi phân tích cú pháp do đầu vào gây ra như:```
catch ( AE , "msg" )
```hoặc```
throw( AE )
```Giai đoạn thứ hai đánh giá mọi khối dựa trên ngoại lệ được ném ra. điều kiện```
try_pos < throw_pos < catch_pos
```khớp chính xác với yêu cầu của tuyên bố rằng quả ném nằm bên trong khối. 

Trận hòa cuối cùng sử dụng vị trí bắt bóng nhỏ nhất, vì ngôn ngữ chọn cú bắt bóng phù hợp xuất hiện đầu tiên sau cú ném. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
try
    try
        throw(AE)
    catch(BE,"BE in line 3")

    try
    catch(AE,"AE in line 5")
catch(AE,"AE somewhere")
```| Sự kiện | Xếp chồng sau sự kiện | Khối được ghi | 
| --- | --- | --- | 
| thử ở 0 | [0] | - | 
| thử lúc 1 | [0,1] | - | 
| ném(AE) ở mức 2 | [0,1] | - | 
| bắt(BE) lúc 3 | [0] | (1,3,BE) | 
| thử lúc 5 | [0,5] | trước | 
| bắt(AE) lúc 6 | [0] | (5,6,AE) | 
| bắt(AE) lúc 7 | [] | (0,7,AE) | 

Kiểm tra các khối: 

| Chặn | Chứa Ném? | Loại phù hợp? | Đạt chuẩn? | 
| --- | --- | --- | --- | 
| (1,3,BE) | Có | Không | Không | 
| (5,6,AE) | Không | Có | Không | 
| (0,7,AE) | Có | Có | Có | 

Tin nhắn được chọn là:```
AE somewhere
```Ví dụ này cho thấy loại phù hợp là không đủ. Cú ném cũng phải nằm giữa lần thử và bắt tương ứng. 

### Ví dụ 2```
try
    try
        throw(AE)
    catch(AE,"inner")
catch(AE,"outer")
```| Chặn | Hãy thử vị trí | Vị trí bắt | Loại | 
| --- | --- | --- | --- | 
| Nội tâm | 1 | 3 | AE | 
| Bên ngoài | 0 | 4 | AE | 

Cả hai khối đều chứa cú ném và cả hai loại đều khớp nhau. 

| Chặn | Vị trí bắt | 
| --- | --- | 
| Nội tâm | 3 | 
| Bên ngoài | 4 | 

Người bắt sớm nhất là vị trí thứ 3 nên đáp án là:```
inner
```Điều này thể hiện quy tắc hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để phân tích cú pháp, một lần vượt qua các khối | 
| Không gian | O(n) | Ngăn xếp và lưu trữ thông tin khối | 

Với nhiều nhất$10^5$dòng, xử lý tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng tuyến tính và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import re

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    stack = []
    blocks = []

    throw_pos = -1
    throw_type = ""

    catch_re = re.compile(
        r'catch\s*\(\s*([A-Za-z]+)\s*,\s*"([^"]+)"\s*\)'
    )
    throw_re = re.compile(
        r'throw\s*\(\s*([A-Za-z]+)\s*\)'
    )

    for i in range(n):
        line = input().strip()

        if not line:
            continue

        if line.startswith("try"):
            stack.append(i)

        elif line.startswith("throw"):
            m = throw_re.match(line)
            throw_type = m.group(1)
            throw_pos = i

        elif line.startswith("catch"):
            m = catch_re.match(line)
            exc_type = m.group(1)
            message = m.group(2)

            blocks.append((stack.pop(), i, exc_type, message))

    ans = None
    best = n + 1

    for l, r, typ, msg in blocks:
        if l < throw_pos < r and typ == throw_type:
            if r < best:
                best = r
                ans = msg

    return (ans if ans is not None else "Unhandled Exception") + "\n"

# provided sample
assert run(
"""8
try
    try
        throw ( AE )
    catch ( BE, "BE in line 3" )

    try
    catch(AE, "AE in line 5")
catch(AE,"AE somewhere")
"""
) == "AE somewhere\n"

# minimum size
assert run(
"""3
try
throw(AE)
catch(AE,"ok")
"""
) == "ok\n"

# no matching handler
assert run(
"""3
try
throw(AE)
catch(BE,"x")
"""
) == "Unhandled Exception\n"

# earliest catch wins
assert run(
"""5
try
try
throw(AE)
catch(AE,"inner")
catch(AE,"outer")
"""
) == "inner\n"

# spaces everywhere
assert run(
"""3
try
 throw (  ABC  )
 catch ( ABC , "works" )
"""
) == "works\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ném thử một lần xung quanh |`ok`| Trường hợp hợp lệ nhỏ nhất | 
| Loại ngoại lệ không khớp |`Unhandled Exception`| Không tìm thấy trình xử lý | 
| Trình xử lý khớp lồng nhau |`inner`| Lựa chọn đánh bắt sớm nhất | 
| Biến thể khoảng trắng nặng |`works`| Phân tích cú pháp mạnh mẽ | 
| Mẫu chính thức |`AE somewhere`| Thông số kỹ thuật đầy đủ | 

## Vỏ cạnh 

### Khối bên ngoài khớp, khối bên trong không khớp 

đầu vào:```
3
try
throw(AE)
catch(BE,"x")
```Khối duy nhất chứa cú ném, nhưng loại của nó là`BE`trong khi kiểu ném là`AE`. 

Thuật toán ghi lại một khối: 

| Hãy thử | Bắt | Loại | 
| --- | --- | --- | 
| 0 | 2 | ĐƯỢC | 

Việc so sánh loại không thành công, do đó không có ứng cử viên nào được chọn và kết quả đầu ra trở thành:```
Unhandled Exception
```### Nhiều trình xử lý phù hợp 

đầu vào:```
5
try
try
throw(AE)
catch(AE,"inner")
catch(AE,"outer")
```Các khối được ghi: 

| Hãy thử | Bắt | Loại | 
| --- | --- | --- | 
| 1 | 3 | AE | 
| 0 | 4 | AE | 

Cả hai đều chứa cú ném và cả hai đều phù hợp với loại. Thuật toán chọn vị trí bắt nhỏ hơn, cụ thể là`3`, sản xuất:```
inner
```### Trình xử lý sau khi ném nhưng thử bắt đầu sau khi ném 

đầu vào:```
5
throw(AE)
try
catch(AE,"later")
try
catch(AE,"another")
```Không khối nào thỏa mãn:```
try_position < throw_position
```vì vậy không thể xử lý ngoại lệ. 

Thuật toán từ chối cả khối và bản in:```
Unhandled Exception
```phù hợp với định nghĩa ngôn ngữ.
