---
title: "CF 282C - XOR và OR"
description: "Chúng tôi được cung cấp hai chuỗi nhị phân. Một thao tác chọn bất kỳ cặp bit liền kề nào. Nếu cặp là (x, y), chúng ta tính: - p = x xor y - q = x hoặc y Sau đó, chúng ta viết p vào một vị trí và q vào vị trí kia, theo cả hai thứ tự. Độ dài của chuỗi không bao giờ thay đổi."
date: "2026-06-05T09:17:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 282
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 173 (Div. 2)"
rating: 1500
weight: 282
solve_time_s: 94
verified: true
draft: false
---

[CF 282C - XOR và OR](https://codeforces.com/problemset/problem/282/C) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi nhị phân. Một thao tác chọn bất kỳ cặp bit liền kề nào. Nếu cặp đó là`(x, y)`, chúng tôi tính toán: 

-`p = x xor y`-`q = x or y`Sau đó chúng tôi viết`p`vào một vị trí và`q`vào vị trí khác, theo một trong hai thứ tự. 

Độ dài của chuỗi không bao giờ thay đổi. Bắt đầu từ chuỗi`a`, chúng ta phải xác định liệu một số chuỗi thao tác như vậy có thể chuyển đổi nó thành chuỗi hay không`b`. 

Các chuỗi có thể dài tới một triệu ký tự. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các phép biến đổi hoặc tìm kiếm thông qua các trạng thái có thể tiếp cận. Ngay cả quét tuyến tính cũng đã xử lý được một triệu ký tự, do đó độ phức tạp của mục tiêu phải là`O(n)`. 

Phần khó khăn là hiểu thông tin nào hoạt động lưu giữ và thông tin nào nó có thể thay đổi. 

Trường hợp cạnh không rõ ràng đầu tiên là khi độ dài khác nhau. 

đầu vào:```
1
11
```Đầu ra:```
NO
```Mọi thao tác đều hoạt động trên hai vị trí hiện có và không bao giờ chèn hoặc xóa ký tự. Độ dài là bất biến nên các chuỗi có độ dài khác nhau không bao giờ có thể chuyển đổi thành nhau. 

Một trường hợp quan trọng khác là khi một chuỗi chỉ chứa số 0. 

đầu vào:```
000
001
```Đầu ra:```
NO
```Bắt đầu từ tất cả các số 0, mọi cặp liền kề đều là`(0,0)`. Hoạt động này tạo ra`(0 xor 0, 0 or 0) = (0,0)`, do đó chuỗi không bao giờ thay đổi. Một giải pháp bất cẩn chỉ kiểm tra độ dài sẽ trả lời sai CÓ. 

Một trường hợp tế nhị hơn là: 

đầu vào:```
10
01
```Đầu ra:```
YES
```Hoạt động trên`(1,0)`cho`(1,1)`Và`(0,1)`đầu ra nhất có thể. Việc chọn cách sắp xếp thứ hai trực tiếp tạo ra`01`. Giải pháp theo dõi vị trí chính xác của một người sẽ thất bại vì người ta có thể di chuyển xung quanh. 

Một trường hợp cạnh khác là: 

đầu vào:```
111
000
```Đầu ra:```
NO
```Mặc dù các thao tác có thể giảm số lượng cái, nhưng chúng không bao giờ có thể loại bỏ cái cuối cùng còn lại. Sự hiện diện của ít nhất một`1`hóa ra là bất biến khóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem mọi chuỗi nhị phân có độ dài cố định dưới dạng trạng thái và thực hiện tìm kiếm biểu đồ trên tất cả các trạng thái có thể truy cập. Trong một thời gian dài`n`, có`2^n`các chuỗi có thể. Ngay cả đối với`n = 50`, con số đó đã lớn về mặt thiên văn và giới hạn thực tế là`10^6`. Việc thăm dò không gian trạng thái là hoàn toàn không thể. 

Để tìm ra giải pháp tốt hơn, chúng ta cần hiểu thao tác thực sự làm gì. 

Xem xét tất cả các cặp liền kề có thể: 

| Bản gốc | xor | hoặc | 
| --- | --- | --- | 
| 00 | 0 | 0 | 
| 01 | 1 | 1 | 
| 10 | 1 | 1 | 
| 11 | 0 | 1 | 

Quan sát quan trọng là cặp sau hoạt động chứa một`1`khi và chỉ nếu cặp ban đầu chứa một`1`. 

Vì`00`, kết quả vẫn là`00`. 

Vì`01`Và`10`, kết quả trở thành hai bit có giá trị là`{1,1}`. 

Vì`11`, kết quả trở thành`{0,1}`. 

Trong mọi trường hợp, một cặp có chứa ít nhất một`1`tiếp tục chứa ít nhất một`1`. Một cặp không có`1`không bao giờ đạt được một. 

Điều này có nghĩa là toàn bộ chuỗi không bao giờ có thể mất phần còn lại cuối cùng của nó`1`và một chuỗi hoàn toàn bằng 0 không bao giờ có thể tạo ra một`1`. 

Câu hỏi tiếp theo là liệu đó có phải là hạn chế duy nhất hay không. 

Giả sử chuỗi chứa ít nhất một`1`. Sau đó chúng ta có thể sử dụng nhiều lần các thao tác để di chuyển`1`xung quanh và tạo ra bất kỳ sự sắp xếp mong muốn nào của số không và số một. Trong thực tế, bất kỳ hai chuỗi nào có cùng độ dài đều chứa ít nhất một`1`có thể tiếp cận được lẫn nhau. Đây là định lý then chốt đằng sau vấn đề. 

Do đó, câu hỏi chuyển đổi chỉ còn hai bước kiểm tra: 

1. Độ dài phải bằng nhau. 
2. Cả hai chuỗi đều chứa ít nhất một`1`, hoặc cả hai chuỗi đều không chứa`1`. 

Điều đó đưa ra một giải pháp thời gian tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`a`Và`b`. 
2. Nếu độ dài của chúng khác nhau, hãy in`"NO"`. 

Hoạt động không bao giờ thay đổi số lượng vị trí nên không thể có độ dài khác nhau. 
3. Kiểm tra xem`a`chứa ít nhất một`'1'`. 
4. Kiểm tra xem`b`chứa ít nhất một`'1'`. 
5. Nếu cả hai lần kiểm tra đều có kết quả giống nhau, hãy in`"YES"`. 

Điều này bao gồm hai tình huống: 

- Cả hai chuỗi đều là số 0. 
- Cả hai chuỗi đều chứa ít nhất một`1`. 
6. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Thao tác này bảo toàn thuộc tính "chuỗi này có chứa ít nhất một`1`?". Một chuỗi toàn 0 sẽ mãi mãi là 0 vì mọi thao tác trên`00`sản xuất`00`. Ngược lại, nếu một chuỗi chứa một`1`, mọi thao tác đều bảo toàn sự tồn tại của ít nhất một`1`, vậy là trận chung kết`1`không bao giờ có thể biến mất. 

Sự thật sâu sắc hơn là khi một chuỗi chứa ít nhất một`1`, cái đó`1`có thể được truyền bá và di chuyển trong chuỗi. Bằng cách sử dụng các thao tác lặp lại, chúng ta có thể chuyển đổi bất kỳ chuỗi nhị phân khác 0 nào thành bất kỳ chuỗi nhị phân khác 0 nào có cùng độ dài. Như vậy, trong số các chuỗi có độ dài cố định, có đúng hai thành phần được kết nối: chuỗi toàn 0 và tất cả các chuỗi chứa ít nhất một`1`. 

Thuật toán kiểm tra chính xác các bất biến này nên vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = input().strip()
b = input().strip()

if len(a) != len(b):
    print("NO")
elif ('1' in a) == ('1' in b):
    print("YES")
else:
    print("NO")
```Điều kiện đầu tiên xử lý bất biến có độ dài không bao giờ thay đổi. 

Điều kiện thứ hai kiểm tra xem cả hai chuỗi có thuộc cùng một lớp khả năng tiếp cận hay không. biểu hiện`('1' in a)`đánh giá để`True`nếu chuỗi chứa ít nhất một bit một và`False`nếu không thì. Khả năng tiếp cận yêu cầu các giá trị này phải khớp. 

Không cần mô phỏng. Toàn bộ giải pháp dựa trên thuộc tính cấu trúc của hoạt động. 

Một lỗi phổ biến là cố gắng so sánh số lượng. Số lượng những cái không được bảo tồn. Ví dụ,`11`có thể trở thành`01`, giảm số lượng từ hai xuống một. Thuộc tính duy nhất được bảo toàn là số đếm bằng 0 hay dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
11
10
```| Biến | Giá trị | 
| --- | --- | 
| một | 11 | 
| b | 10 | 
| len(a) == len(b) | Đúng | 
| '1' trong | Đúng | 
| '1' trong b | Đúng | 

Kết quả:```
YES
```Cả hai chuỗi đều có cùng độ dài và đều chứa ít nhất một`1`. Chúng thuộc cùng một lớp khả năng tiếp cận. 

### Ví dụ 2 

đầu vào:```
000
001
```| Biến | Giá trị | 
| --- | --- | 
| một | 000 | 
| b | 001 | 
| len(a) == len(b) | Đúng | 
| '1' trong | Sai | 
| '1' trong b | Đúng | 

Kết quả:```
NO
```Chuỗi bắt đầu là trạng thái hoàn toàn bằng 0 duy nhất. Vì không có hoạt động nào có thể tạo ra một`1`, đạt`001`là không thể. 

Ví dụ này thể hiện tính bất biến chính được bảo toàn bởi mọi thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Kiểm tra thành viên cho`'1'`quét từng chuỗi một lần | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Với chiều dài lên đến`10^6`, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    a = input().strip()
    b = input().strip()

    if len(a) != len(b):
        print("NO")
    elif ('1' in a) == ('1' in b):
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("11\n10\n") == "YES", "sample 1"

# minimum size
assert run("0\n0\n") == "YES", "single zero"

# minimum size, different classes
assert run("0\n1\n") == "NO", "cannot create a one"

# all-zero strings
assert run("00000\n00000\n") == "YES", "all-zero component"

# same length, both non-zero
assert run("1000\n0001\n") == "YES", "all non-zero strings are connected"

# different lengths
assert run("1\n11\n") == "NO", "length is invariant"

# large-style boundary case
assert run(("1" + "0" * 20) + "\n" + ("0" * 20 + "1") + "\n") == "YES", "non-zero strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 / 0`| CÓ | Đầu vào hợp lệ nhỏ nhất có thể | 
|`0 / 1`| KHÔNG | Không thể tạo một cái | 
|`00000 / 00000`| CÓ | Thành phần hoàn toàn bằng không | 
|`1000 / 0001`| CÓ | Vị trí của một người không quan trọng | 
|`1 / 11`| KHÔNG | Độ dài bất biến | 
| Cặp lớn khác 0 | CÓ | Mọi chuỗi khác 0 có độ dài bằng nhau đều được kết nối | 

## Vỏ cạnh 

Hãy xem xét:```
0
0
```Thuật toán trước tiên sẽ kiểm tra độ dài phù hợp. Cả hai chuỗi đều không có`'1'`, do đó các giá trị boolean bằng nhau và câu trả lời là CÓ. Điều này đúng vì không có thao tác nào đã đạt được mục tiêu. 

Coi như:```
0
1
```Độ dài phù hợp, nhưng`'1' in a`là`False`trong khi`'1' in b`là`True`. Thuật toán xuất ra NO. Từ`00 -> 00`là hành vi duy nhất có sẵn trong chuỗi hoàn toàn bằng 0, việc tạo một bit là không thể. 

Coi như:```
111
000
```Cả hai chuỗi đều có độ dài bằng nhau. Cái đầu tiên chứa một`1`, cái thứ hai thì không. Thuật toán xuất ra NO. Mỗi hoạt động duy trì sự tồn tại của ít nhất một`1`, nên cái cuối cùng còn lại không bao giờ có thể biến mất. 

Coi như:```
10
01
```Độ dài khớp nhau và cả hai chuỗi đều chứa một`1`, do đó thuật toán xuất ra CÓ. Thật vậy, áp dụng phép toán cho cặp`(1,0)`và lựa chọn cách sắp xếp`(0,1)`trực tiếp tiếp cận mục tiêu. 

Những trường hợp này bao gồm các thuộc tính duy nhất quan trọng: độ dài bằng nhau và sự thống nhất về việc liệu ít nhất một`1`tồn tại.
