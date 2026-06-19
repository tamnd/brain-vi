---
title: "CF 1B - Bảng tính"
description: "Tác vụ đưa ra tọa độ ô bảng tính được viết theo một trong hai định dạng và với mỗi tọa độ chúng ta phải chuyển đổi nó sang định dạng khác."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 1"
rating: 1600
weight: 1
solve_time_s: 295
verified: true
draft: false
---
## Hiểu vấn đề 

Tác vụ đưa ra tọa độ ô bảng tính được viết theo một trong hai định dạng và với mỗi tọa độ chúng ta phải chuyển đổi nó sang định dạng khác. 

Định dạng đầu tiên là ký hiệu kiểu Excel thông thường. Một cột được viết bằng các chữ cái, sau đó là số hàng ngay sau cột đó. Ví dụ,`BC23`có nghĩa là cột`BC`và hàng`23`. Phần khó khăn là cột hoạt động giống như một hệ thống đánh số cơ số 26 mà không có chữ số 0.`A = 1`,`B = 2`, ...,`Z = 26`,`AA = 27`, vân vân. 

Định dạng thứ hai ghi thông tin tương tự như`R<row>C<column>`. Ví dụ,`R23C55`có nghĩa là hàng`23`, cột`55`. 

Đối với mỗi chuỗi đầu vào, trước tiên chúng tôi xác định ký hiệu nào được sử dụng, sau đó chuyển đổi nó sang ký hiệu khác. 

Các ràng buộc đủ lớn nên hiệu quả là vấn đề quan trọng. Có thể có tới`10^5`tọa độ và mỗi tọa độ có thể biểu thị các giá trị lên đến`10^6`. Một giải pháp mô phỏng nhiều lần việc đánh số bảng tính theo từng ký tự cho các phạm vi lớn sẽ quá chậm. Chúng ta cần một cái gì đó gần tuyến tính về độ dài của chuỗi đầu vào. Vì mỗi tọa độ đều ngắn nên`O(length)`chuyển đổi cho mỗi truy vấn dễ dàng đủ nhanh. 

Khó khăn chính là nhận dạng chính xác định dạng. Một kiểm tra ngây thơ như “bắt đầu bằng`R`” không thành công vì tọa độ kiểu Excel hợp lệ cũng có thể bắt đầu bằng`R`. Ví dụ:```
R23C55
```đang ở trong`RXCY`định dạng, nhưng```
RC23
```thực ra là ký hiệu kiểu Excel, bởi vì`RC`là tên cột và`23`là hàng. 

Một trình phân tích cú pháp bất cẩn có thể phân loại sai`RC23`BẰNG`RXCY`bởi vì nó bắt đầu bằng`R`và chứa`C`. Quy tắc đúng chặt chẽ hơn: sau khi dẫn đầu`R`, phải có ít nhất một chữ số thì một`C`, sau đó lại có ít nhất một chữ số. 

Một nguồn lỗi phổ biến khác là việc chuyển đổi cột trong bảng tính. Các cột trong bảng tính không phải là cơ số 26 tiêu chuẩn vì không có chữ số 0. Ví dụ:```
26 -> Z
27 -> AA
52 -> AZ
53 -> BA
```Nếu chúng ta trực tiếp sử dụng`% 26`mà không điều chỉnh chữ số 0 bị thiếu này,`26`trở thành không chính xác`BA`thay vì`Z`. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là tạo ra các tên cột trong bảng tính một cách rõ ràng theo thứ tự:```
A, B, C, ..., Z, AA, AB, ...
```và dừng lại khi chúng tôi đạt đến số cột được yêu cầu. Việc chuyển đổi từ các chữ cái trở lại thành số thật dễ dàng với số học vị trí, nhưng việc chuyển đổi một số thành các chữ cái bằng cách tạo ra mọi cột trước đó sẽ nhanh chóng trở nên không thực tế. 

Giả sử chúng ta cần cột`10^6`. Ngay cả khi mỗi nhãn được tạo mất thời gian không đổi, chúng tôi vẫn sẽ thực hiện khoảng một triệu lần lặp cho một truy vấn. Với`10^5`truy vấn, điều này bùng nổ xung quanh`10^11`hoạt động vượt quá giới hạn có thể chấp nhận được. 

Quan sát quan trọng là các cột trong bảng tính tạo thành một hệ thống số theo vị trí. Phần bất thường duy nhất là các chữ số nằm trong khoảng từ`1`ĐẾN`26`thay vì`0`ĐẾN`25`. 

Điều đó có nghĩa là chúng ta có thể chuyển đổi trực tiếp giữa hai biểu diễn về mặt toán học. 

Để chuyển đổi các chữ cái thành số, chúng tôi xử lý chuỗi chính xác như chuyển đổi cơ sở:```
ABC = 1 * 26^2 + 2 * 26^1 + 3
```Để chuyển đổi một số trở lại thành chữ cái, chúng tôi liên tục trích xuất chữ số cuối cùng bằng cách sử dụng số học modulo. Vì không có chữ số 0 nên ta trừ đi một trước khi lấy`% 26`. 

Để phát hiện định dạng, chúng tôi quét chuỗi và kiểm tra xem nó có khớp không:```
R + digits + C + digits
```Nếu có, chúng tôi chuyển đổi từ`RXCY`sang kiểu Excel. Nếu không, chúng tôi chuyển đổi theo hướng ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(giá trị cột) cho mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(độ dài chuỗi) cho mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi tọa độ. 
2. Xác định định dạng của nó. 

Một chuỗi ở trong`RXCY`định dạng nếu: 

- nó bắt đầu bằng`'R'`- sau đó`'R'`có ít nhất một chữ số 
- sau những chữ số đó có một`'C'`- sau đó`'C'`có ít nhất một chữ số 

Điều này tránh các chuỗi phân loại sai như`RC23`. 
3. Nếu chuỗi theo kiểu Excel: 

Chia nó thành hai phần: 

- các chữ cái đầu, đại diện cho cột 
- các chữ số ở cuối, đại diện cho hàng 
4. Chuyển các chữ cái trong cột thành số. 

Xử lý từng chữ cái từ trái sang phải:```
value = value * 26 + letter_value
```Ở đâu`A = 1`,`B = 2`, ...,`Z = 26`. 
5. Xuất kết quả dưới dạng:```
R<row>C<column_number>
```6. Nếu chuỗi đã có sẵn`RXCY`định dạng: 

Trích xuất số hàng và số cột. 
7. Chuyển đổi số cột thành chữ cái trong bảng tính. 

Lặp đi lặp lại: 

- trừ 1 từ số 
- lấy`% 26`để có được bức thư hiện tại 
- chia cho 26 cho bước tiếp theo 

Phép trừ là cần thiết vì các cột trong bảng tính được lập chỉ mục 1 thay vì được lập chỉ mục 0. 
8. Đảo ngược các chữ cái đã thu thập vì quá trình chuyển đổi sẽ xây dựng chuỗi từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa nhất. 
9. Đầu ra:```
<column_letters><row>
```### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là các cột trong bảng tính hoạt động chính xác giống như một hệ thống số vị trí có cơ số 26 và các chữ số.`1..26`. 

Khi chuyển đổi chữ cái thành số, mỗi bước sẽ dịch chuyển giá trị hiện tại theo một vị trí cơ số 26 và thêm chữ số tiếp theo. Điều này giống hệt với phân tích cú pháp thập phân thông thường. 

Khi chuyển đổi số trở lại thành chữ cái, trừ đi một sẽ biến đổi phạm vi`1..26`vào tiêu chuẩn`0..25`phạm vi cần thiết cho số học modulo. Mỗi lần lặp sẽ trích xuất chính xác một chữ số cơ sở 26, do đó các chữ cái được tạo ra đại diện duy nhất cho số cột ban đầu. 

Quy tắc phát hiện định dạng đảm bảo rằng mọi đầu vào đều được phân loại chính xác vì hai định dạng này có cấu trúc khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_rxcy(s):
    if not s or s[0] != 'R':
        return False

    i = 1

    if i >= len(s) or not s[i].isdigit():
        return False

    while i < len(s) and s[i].isdigit():
        i += 1

    if i >= len(s) or s[i] != 'C':
        return False

    i += 1

    if i >= len(s):
        return False

    while i < len(s):
        if not s[i].isdigit():
            return False
        i += 1

    return True

def letters_to_number(col):
    value = 0

    for ch in col:
        value = value * 26 + (ord(ch) - ord('A') + 1)

    return value

def number_to_letters(num):
    result = []

    while num > 0:
        num -= 1
        result.append(chr(num % 26 + ord('A')))
        num //= 26

    return ''.join(reversed(result))

def solve():
    n = int(input())
    ans = []

    for _ in range(n):
        s = input().strip()

        if is_rxcy(s):
            c_pos = s.index('C')

            row = s[1:c_pos]
            col_num = int(s[c_pos + 1:])

            col_letters = number_to_letters(col_num)

            ans.append(col_letters + row)

        else:
            i = 0

            while s[i].isalpha():
                i += 1

            col_letters = s[:i]
            row = s[i:]

            col_num = letters_to_number(col_letters)

            ans.append(f"R{row}C{col_num}")

    print('\n'.join(ans))

solve()
```Giải pháp tách vấn đề thành ba phần độc lập: phát hiện định dạng, chuyển đổi từ chữ cái sang số và chuyển đổi từ số sang chữ cái. 

các`is_rxcy`chức năng chặt chẽ hơn việc kiểm tra mẫu đơn giản. Nó xác minh rõ ràng cấu trúc được yêu cầu:```
R + digits + C + digits
```Điều này ngăn chặn các kết quả khớp sai như`RC23`. 

các`letters_to_number`hàm thực hiện phân tích cú pháp vị trí thông thường. Mỗi ký tự mới sẽ dịch chuyển giá trị hiện tại một vị trí cơ số 26 và thêm giá trị chữ số tiếp theo. 

Việc chuyển đổi ngược lại tinh tế hơn. Các cột trong bảng tính không được lập chỉ mục bằng 0, vì vậy trước khi lấy`% 26`chúng tôi trừ đi một từ số hiện tại. Nếu không có sự điều chỉnh này:```
26 % 26 = 0
```sẽ lập bản đồ không chính xác`26`ĐẾN`'A'`thay vì`'Z'`. 

Quá trình chuyển đổi sẽ xây dựng các ký tự từ phải sang trái một cách tự nhiên nên chúng tôi đảo ngược kết quả ở cuối. 

Việc triển khai chỉ sử dụng bộ nhớ bổ sung không đổi ngoài danh sách đầu ra và mỗi ký tự được xử lý với số lần không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
R23C55
```#### Dấu vết 

| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Phát hiện định dạng | is_rxcy | Đúng | 
| Trích xuất hàng | hàng |`"23"`| 
| Trích xuất cột | cột_num |`55`| 
| Lần lặp đầu tiên | số |`55 -> 54`| 
| Chữ cái đầu tiên | 54 % 26 |`2 -> 'C'`| 
| Số còn lại | 54 // 26 |`2`| 
| Lặp lại lần thứ hai | số |`2 -> 1`| 
| Bức thư thứ hai | 1 % 26 |`1 -> 'B'`| 
| Kết quả cuối cùng | chữ đảo ngược |`"BC"`| 

Đầu ra:```
BC23
```Dấu vết này cho thấy tại sao việc trừ một trước modulo là cần thiết. Không có nó, cột`55`sẽ không ánh xạ chính xác tới`BC`. 

### Ví dụ 2 

đầu vào:```
BC23
```#### Dấu vết 

| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Phát hiện định dạng | is_rxcy | Sai | 
| Tách chuỗi | thư col_let |`"BC"`| 
| Tách chuỗi | hàng |`"23"`| 
| Quá trình`'B'`| giá trị |`0 * 26 + 2 = 2`| 
| Quá trình`'C'`| giá trị |`2 * 26 + 3 = 55`| 
| Số cột cuối cùng | cột_num |`55`| 

Đầu ra:```
R23C55
```Điều này thể hiện việc giải thích vị trí của các cột trong bảng tính.`BC`hoạt động chính xác như số cơ sở 26 có hai chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài đầu vào) | Mỗi ký tự được xử lý một số lần không đổi | 
| Không gian | O(1) phụ trợ | Chỉ một số biến được sử dụng trong quá trình chuyển đổi | 

Ngay cả với`10^5`tọa độ, tổng số lượng văn bản được xử lý nhỏ. Thuật toán dễ dàng phù hợp trong giới hạn thời gian vì mỗi truy vấn chỉ yêu cầu phân tích cú pháp và số học trực tiếp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    input_stream = io.StringIO(inp)
    output_stream = io.StringIO()

    input = input_stream.readline

    def is_rxcy(s):
        if not s or s[0] != 'R':
            return False

        i = 1

        if i >= len(s) or not s[i].isdigit():
            return False

        while i < len(s) and s[i].isdigit():
            i += 1

        if i >= len(s) or s[i] != 'C':
            return False

        i += 1

        if i >= len(s):
            return False

        while i < len(s):
            if not s[i].isdigit():
                return False
            i += 1

        return True

    def letters_to_number(col):
        value = 0

        for ch in col:
            value = value * 26 + (ord(ch) - ord('A') + 1)

        return value

    def number_to_letters(num):
        result = []

        while num > 0:
            num -= 1
            result.append(chr(num % 26 + ord('A')))
            num //= 26

        return ''.join(reversed(result))

    n = int(input())
    ans = []

    for _ in range(n):
        s = input().strip()

        if is_rxcy(s):
            c_pos = s.index('C')

            row = s[1:c_pos]
            col_num = int(s[c_pos + 1:])

            ans.append(number_to_letters(col_num) + row)

        else:
            i = 0

            while s[i].isalpha():
                i += 1

            col = s[:i]
            row = s[i:]

            ans.append(f"R{row}C{letters_to_number(col)}")

    output_stream.write('\n'.join(ans))
    return output_stream.getvalue()

# provided samples
assert solve_io(
    "2\nR23C55\nBC23\n"
) == "BC23\nR23C55", "sample 1"

# minimum values
assert solve_io(
    "1\nA1\n"
) == "R1C1", "minimum coordinate"

# boundary around Z -> AA
assert solve_io(
    "3\nZ1\nAA1\nR1C26\n"
) == "R1C26\nR1C27\nZ1", "base-26 transition"

# tricky parsing case
assert solve_io(
    "1\nRC23\n"
) == "R23C471", "must not classify as RXCY"

# larger column values
assert solve_io(
    "2\nR999C702\nZZ999\n"
) == "ZZ999\nR999C702", "double-letter boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A1`|`R1C1`| Tọa độ hợp lệ nhỏ nhất | 
|`Z1`,`AA1`,`R1C26`| Chuyển tiếp đúng | Xử lý từng cái một trong chuyển đổi cơ sở 26 | 
|`RC23`|`R23C471`| Phát hiện định dạng đúng | 
|`R999C702`|`ZZ999`| Độ chính xác chuyển đổi nhiều chữ cái | 

## Vỏ cạnh 

Trường hợp đặc biệt nguy hiểm là:```
RC23
```Một trình phân tích cú pháp yếu có thể coi đây là`RXCY`định dạng vì nó bắt đầu bằng`R`và chứa`C`. Thuật toán kiểm tra các chữ số ngay sau`R`. Vì ký tự thứ hai là`C`, mẫu không thành công, do đó chuỗi được xử lý chính xác dưới dạng ký hiệu kiểu Excel. 

Việc chuyển đổi tiến hành như sau:```
R = 18
C = 3
18 * 26 + 3 = 471
```Đầu ra cuối cùng trở thành:```
R23C471
```Một ranh giới phức tạp khác là cột`26`. 

đầu vào:```
R1C26
```Trong quá trình chuyển đổi: 

| Lặp lại | số trước | num sau khi trừ | phần còn lại | lá thư | 
| --- | --- | --- | --- | --- | 
| 1 | 26 | 25 | 25 | Z | 

Các đầu ra thuật toán:```
Z1
```Nếu chúng ta bỏ qua bước trừ,`% 26`sẽ sản xuất`0`, dẫn đến ánh xạ không chính xác. 

Sự chuyển tiếp từ`Z`ĐẾN`AA`là một cái bẫy cổ điển khác. 

đầu vào:```
R1C27
```Dấu vết: 

| Lặp lại | số trước | num sau khi trừ | phần còn lại | lá thư | 
| --- | --- | --- | --- | --- | 
| 1 | 27 | 26 | 0 | A | 
| 2 | 1 | 0 | 0 | A | 

Đảo ngược các chữ cái đã thu thập sẽ cho:```
AA1
```Điều này xác nhận hệ thống đánh số hoạt động giống như cơ sở 26 được lập chỉ mục 1 thay vì cơ sở 26 tiêu chuẩn.
