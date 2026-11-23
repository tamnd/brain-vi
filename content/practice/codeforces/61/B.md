---
title: "CF 61B - Làm việc chăm chỉ"
description: "Chúng tôi được cung cấp ba chuỗi ban đầu. Câu trả lời của học sinh được coi là đúng nếu nó có thể được hình thành bằng cách nối ba chuỗi này theo bất kỳ thứ tự nào, sau khi bỏ qua hai loại khác biệt. Sự khác biệt đầu tiên là cách viết hoa chữ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 61
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 57 (Div. 2)"
rating: 1300
weight: 61
solve_time_s: 395
verified: true
draft: false
---

[CF 61B - Làm việc chăm chỉ](https://codeforces.com/problemset/problem/61/B) 

**Đánh giá:** 1300 
**Thẻ:** chuỗi 
**Thời gian giải:** 6 phút 35 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp ba chuỗi ban đầu. Câu trả lời của học sinh được coi là đúng nếu nó có thể được hình thành bằng cách nối ba chuỗi này theo bất kỳ thứ tự nào, sau khi bỏ qua hai loại khác biệt. 

Sự khác biệt đầu tiên là cách viết hoa chữ. Phiên bản chữ hoa và chữ thường của cùng một chữ cái phải được coi là bằng nhau. 

Điểm khác biệt thứ hai là sự có mặt của các ký tự đặc biệt`-`,`;`, Và`_`. Những ký tự này hoàn toàn bị bỏ qua. Học sinh có thể chèn những cái bổ sung, loại bỏ những cái hiện có hoặc đặt chúng ở bất cứ đâu. 

Đối với mỗi câu trả lời của học sinh, chúng ta phải in`ACC`nếu câu trả lời được làm sạch phù hợp với một số hoán vị của ba chuỗi gốc đã được làm sạch. Nếu không chúng tôi in`WA`. 

Những hạn chế là rất nhỏ. Mỗi chuỗi ban đầu có độ dài tối đa là 100 và chỉ có ba chuỗi trong số đó. Mỗi câu trả lời của học sinh có độ dài tối đa là 600 và có tối đa 1000 câu trả lời. Ngay cả một cách tiếp cận thử trực tiếp tất cả các hoán vị cũng đủ nhanh vì chỉ có`3! = 6`các đơn đặt hàng có thể. 

Phần khó khăn không phải là hiệu suất mà là sự chuẩn hóa. Việc triển khai bất cẩn có thể so sánh các chuỗi thô và từ chối các câu trả lời chỉ khác nhau ở các ký tự hoặc kiểu chữ bị bỏ qua. 

Hãy xem xét ví dụ này:```
Original strings:
Ab
Cd
Ef

Student answer:
__efABcd;;
```Kết quả đúng là`ACC`bởi vì sau khi loại bỏ dấu và chuyển sang chữ thường, câu trả lời sẽ trở thành`efabcd`, đó chính xác là một phép nối hợp lệ. 

Một lỗi phổ biến khác là quên rằng các dấu hiệu bên trong chuỗi gốc cũng phải bị bỏ qua.```
Original strings:
A_B
C
D

Student answer:
abcd
```Đầu ra đúng là`ACC`. Nếu chúng ta chỉ làm sạch câu trả lời của học sinh mà không xóa các chuỗi gốc, chúng ta sẽ mong đợi một cách sai lầm`a_bd`thay vì`ab`. 

Một trường hợp tinh tế hơn là khớp một phần.```
Original strings:
ab
cd
ef

Student answer:
abcdefg
```Đây phải là`WA`. Mặc dù câu trả lời có chứa tiền tố nối hợp lệ nhưng toàn bộ chuỗi đã làm sạch phải khớp chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tạo ra mọi thứ tự nối có thể có của ba chuỗi, chuẩn hóa từng chuỗi và so sánh mọi câu trả lời của học sinh với các kết quả hợp lệ này. 

Vì chỉ có ba chuỗi nên có đúng sáu hoán vị. Đối với mỗi hoán vị, chúng tôi nối các chuỗi, xóa tất cả các dấu và chuyển đổi mọi thứ thành chữ thường. Sau đó, câu trả lời của mỗi học sinh được chuẩn hóa theo cách tương tự và được kiểm tra dựa trên sáu chuỗi hợp lệ. 

Cách tiếp cận vũ phu này đã hoàn toàn được chấp nhận. Khối lượng công việc rất nhỏ: 

- Sáu hoán vị. 
- Mỗi chuỗi nối có độ dài tối đa là 300 trước khi làm sạch. 
- Tối đa 1000 câu trả lời của học sinh. 

Tổng số thao tác vẫn thấp hơn nhiều so với vài triệu thao tác ký tự. 

Quan sát quan trọng là số lượng đơn hàng cố định và cực kỳ nhỏ. Nếu số lượng chuỗi lớn, việc tạo ra tất cả các hoán vị sẽ bùng nổ theo giai thừa. Đây,`3!`chỉ là 6, vì vậy việc tính toán trước tất cả các phép nối chuẩn hóa hợp lệ là giải pháp đơn giản và rõ ràng nhất. 

Một phiên bản gọn gàng hơn một chút sẽ lưu trữ sáu chuỗi chuẩn hóa hợp lệ bên trong một tập hợp. Sau đó, mỗi truy vấn sẽ trở thành kiểm tra tư cách thành viên liên tục sau khi chuẩn hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force bằng cách kiểm tra tất cả các hoán vị trực tiếp trên mỗi truy vấn | O(6 × L × n) | O(1) | Đã chấp nhận | 
| Tối ưu với bộ chuẩn hóa được tính toán trước | O(6 × L + n × L) | O(1) | Đã chấp nhận | 

Đây,`L`đại diện cho độ dài chuỗi tối đa liên quan đến chuẩn hóa. 

## Hướng dẫn thuật toán 

1. Đọc ba chuỗi gốc. 
2. Xác định hàm chuẩn hóa. 

Hàm này loại bỏ mọi lần xuất hiện của`-`,`;`, Và`_`, sau đó chuyển các chữ cái còn lại thành chữ thường. Hai chuỗi được coi là tương đương chính xác khi dạng chuẩn hóa của chúng bằng nhau. 
3. Tạo ra tất cả sáu hoán vị của ba chuỗi. 

Đối với mỗi hoán vị, nối các chuỗi theo thứ tự đó và chuẩn hóa kết quả. 
4. Lưu trữ tất cả các phép nối chuẩn hóa bên trong một bộ. 

Việc sử dụng một học phần giúp kiểm tra tư cách thành viên nhanh chóng và tự động tránh các mục trùng lặp. 
5. Đọc câu trả lời của từng học sinh. 
6. Chuẩn hóa câu trả lời của học sinh bằng cách sử dụng chức năng chuẩn hóa tương tự. 

Việc sử dụng quy trình xử lý giống hệt nhau cho cả bản gốc và câu trả lời đảm bảo sự so sánh công bằng. 
7. Kiểm tra xem câu trả lời chuẩn hóa có tồn tại trong tập hợp các phép nối hợp lệ hay không. 

Nếu nó tồn tại, in`ACC`. Nếu không thì in`WA`. 

### Tại sao nó hoạt động 

Hàm chuẩn hóa loại bỏ mọi điểm khác biệt mà câu lệnh yêu cầu chúng ta bỏ qua. Sau khi chuẩn hóa, hai chuỗi bằng nhau chính xác khi chúng được coi là tương đương theo các quy tắc của bài toán. 

Mỗi câu trả lời hợp lệ phải tương ứng với một trong sáu thứ tự nối có thể có của ba chuỗi gốc. Vì chúng tôi tạo và chuẩn hóa tất cả sáu khả năng trước đó nên tập hợp này chứa mọi câu trả lời đúng và không có gì khác. 

Câu trả lời của học sinh được chấp nhận khi và chỉ khi dạng chuẩn hóa của nó khớp với một trong các phép nối hợp lệ đã chuẩn hóa này. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def normalize(s):
    res = []
    
    for ch in s:
        if ch not in "-;_":
            res.append(ch.lower())
    
    return "".join(res)

def solve():
    arr = [input().strip() for _ in range(3)]

    valid = set()

    for p in permutations(arr):
        combined = p[0] + p[1] + p[2]
        valid.add(normalize(combined))

    n = int(input())

    for _ in range(n):
        ans = normalize(input().strip())

        if ans in valid:
            print("ACC")
        else:
            print("WA")

solve()
```các`normalize`chức năng là cốt lõi của giải pháp. Nó loại bỏ tất cả các dấu hiệu bị bỏ qua và chuyển đổi các chữ cái thành chữ thường. Việc sử dụng cùng một hàm cho cả chuỗi gốc và câu trả lời của học sinh sẽ ngăn ngừa sự mâu thuẫn. 

Các hoán vị được tạo bằng cách sử dụng`itertools.permutations`. Vì chỉ có ba chuỗi nên điều này tạo ra chính xác sáu đơn hàng. 

Các phép nối chuẩn hóa hợp lệ được lưu trữ trong một tập hợp. Kiểm tra tư cách thành viên trong một tập hợp là hiệu quả và giữ cho logic kiểm tra đơn giản. 

Một chi tiết triển khai tinh tế là loại bỏ các ký tự dòng mới bằng`.strip()`trước khi xử lý. Quên điều này sẽ để lại dấu vết`\n`các ký tự bên trong chuỗi và ngắt so sánh. 

Một chi tiết quan trọng khác là làm sạch các chuỗi gốc cũng như các câu trả lời. Tuyên bố cho phép các dấu hiệu xuất hiện ở bất cứ đâu trong cả hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Iran_
Persian;
W_o;n;d;e;r;f;u;l;
WonderfulPersianIran
```Sau khi bình thường hóa: 

| Bước | Giá trị | 
| --- | --- | 
| Bản gốc 1 |`iran`| 
| Bản gốc 2 |`persian`| 
| Bản gốc 3 |`wonderful`| 
| Câu trả lời của sinh viên |`wonderfulpersianiran`| 

Một hoán vị hợp lệ là: 

| Hoán vị | Kết quả chuẩn hóa | 
| --- | --- | 
| Tuyệt vời + Ba Tư + Iran |`wonderfulpersianiran`| 

Câu trả lời đã chuẩn hóa của học sinh khớp với một phép nối chuẩn hóa hợp lệ, vì vậy kết quả đầu ra là:```
ACC
```Ví dụ này chứng minh rằng các dấu hiệu bị bỏ qua ở mọi nơi, kể cả bên trong chuỗi gốc. 

### Ví dụ 2 

đầu vào:```
ab
cd
ef
abcdefg
```Bình thường hóa không thay đổi gì ở đây. 

| Bước | Giá trị | 
| --- | --- | 
| Ví dụ nối hợp lệ |`abcdef`| 
| Câu trả lời của sinh viên |`abcdefg`| 

Câu trả lời có chứa một ký tự phụ`g`, vì vậy nó không khớp chính xác với bất kỳ phép nối hợp lệ nào. 

Đầu ra:```
WA
```Ví dụ này xác nhận rằng việc so sánh phải chính xác sau khi chuẩn hóa, không dựa trên chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O (n × L) | Mỗi câu trả lời được chuẩn hóa một lần và chỉ có sáu chuỗi hợp lệ được tính toán trước | 
| Không gian | O(L) | Bộ lưu trữ tối đa sáu phép nối chuẩn hóa | 

Đây,`L`là độ dài chuỗi tối đa sau khi nối. Vì tổng kích thước đầu vào rất nhỏ nên giải pháp này dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def normalize(s):
        res = []

        for ch in s:
            if ch not in "-;_":
                res.append(ch.lower())

        return "".join(res)

    arr = [input().strip() for _ in range(3)]

    valid = set()

    for p in permutations(arr):
        valid.add(normalize(p[0] + p[1] + p[2]))

    n = int(input())

    out = []

    for _ in range(n):
        ans = normalize(input().strip())

        if ans in valid:
            out.append("ACC")
        else:
            out.append("WA")

    return "\n".join(out)

# provided sample
assert run(
"""Iran_
Persian;
W_o;n;d;e;r;f;u;l;
7
WonderfulPersianIran
wonderful_PersIAN_IRAN;;_
WONDERFUL___IRAN__PERSIAN__;;
Ira__Persiann__Wonderful
Wonder;;fulPersian___;I;r;a;n;
__________IranPersianWonderful__________
PersianIran_is_Wonderful
"""
) == "\n".join([
    "ACC",
    "ACC",
    "ACC",
    "WA",
    "ACC",
    "ACC",
    "WA"
]), "sample 1"

# minimum-size strings
assert run(
"""a
b
c
2
abc
acb
"""
) == "\n".join([
    "ACC",
    "ACC"
]), "minimum size"

# case insensitivity
assert run(
"""Ab
Cd
Ef
2
abcdef
EFABCD
"""
) == "\n".join([
    "ACC",
    "ACC"
]), "case handling"

# ignored signs
assert run(
"""a_b
c;d
-e
2
abcde
ab_cd_e
"""
) == "\n".join([
    "ACC",
    "ACC"
]), "sign removal"

# incorrect extra character
assert run(
"""ab
cd
ef
1
abcdefg
"""
) == "WA", "extra characters must fail"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a b c`với`abc`Và`acb`|`ACC ACC`| Hoán vị hợp lệ kích thước tối thiểu | 
| Câu trả lời viết hoa hỗn hợp |`ACC`| So sánh không phân biệt chữ hoa chữ thường | 
| Chuỗi chứa dấu hiệu |`ACC`| Loại bỏ đúng các ký tự bị bỏ qua | 
| Ký tự cuối bổ sung |`WA`| Kết hợp chính xác sau khi chuẩn hóa | 

## Vỏ cạnh 

Hãy xem xét trường hợp các dấu hiệu bị bỏ qua xuất hiện bên trong chuỗi gốc.```
Input:
A_B
C
D
1
abcd
```Thuật toán chuẩn hóa bản gốc thành:```
ab
c
d
```Một nối được tạo ra sẽ trở thành`abcd`. Câu trả lời của học sinh cũng bình thường hóa thành`abcd`, do đó đầu ra là:```
ACC
```Điều này có tác dụng vì quá trình chuẩn hóa được áp dụng đối xứng cho cả hai bên. 

Bây giờ hãy xem xét vỏ hỗn hợp.```
Input:
Ab
Cd
Ef
1
eFaBcD
```Hoán vị hợp lệ`Ef + Ab + Cd`bình thường hóa thành:```
efabcd
```Câu trả lời của học sinh cũng bình thường hóa thành:```
efabcd
```Việc tra cứu thiết lập thành công nên câu trả lời được chấp nhận. 

Cuối cùng, hãy xem xét một kết quả gần giống với các ký tự phụ.```
Input:
ab
cd
ef
1
abcdefx
```Các chuỗi chuẩn hóa hợp lệ đều có độ dài 6. Câu trả lời chuẩn hóa của học sinh có độ dài 7:```
abcdefx
```Vì thuật toán so sánh các chuỗi chuẩn hóa hoàn chỉnh chứ không phải tiền tố nên câu trả lời sẽ bị từ chối một cách chính xác với:```
WA
```
