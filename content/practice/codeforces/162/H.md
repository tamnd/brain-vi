---
title: "CF 162H - Vỏ thay thế"
description: "Chúng ta được cung cấp một chuỗi chứa các chữ cái tiếng Anh viết hoa tùy ý. Nhiệm vụ là viết lại chuỗi sao cho các ký tự ở vị trí lẻ trở thành chữ hoa và các ký tự ở vị trí chẵn trở thành chữ thường. Các vị trí được tính bắt đầu từ 1, không phải từ 0."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "H"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1800
weight: 162
solve_time_s: 191
verified: false
draft: false
---

[CF 162H - Trường hợp thay thế](https://codeforces.com/problemset/problem/162/H) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chứa các chữ cái tiếng Anh viết hoa tùy ý. Nhiệm vụ là viết lại chuỗi sao cho các ký tự ở vị trí lẻ trở thành chữ hoa và các ký tự ở vị trí chẵn trở thành chữ thường. Các vị trí được tính bắt đầu từ 1, không phải từ 0. 

Ví dụ: nếu đầu vào là`Codeforces`, ký tự đầu tiên sẽ trở thành chữ hoa, chữ thường thứ hai, chữ hoa thứ ba trở lại, v.v. Chuỗi kết quả là`CoDeFoRcEs`. 

Những hạn chế là rất nhỏ. Độ dài chuỗi tối đa là 100 ký tự, do đó, ngay cả những cách tiếp cận không hiệu quả cũng sẽ chạy ngay lập tức. Quét tuyến tính trên chuỗi là quá đủ. Vì mỗi ký tự được xử lý độc lập nên giải pháp tự nhiên là lặp lại một lần qua chuỗi và quyết định trường hợp nào dựa trên tính chẵn lẻ của vị trí. 

Nguồn gốc chính của sai lầm là việc lập chỉ mục. Python sử dụng các chỉ mục dựa trên 0, trong khi vấn đề mô tả các vị trí bắt đầu từ 1. Việc triển khai bất cẩn có thể vô tình đảo ngược mẫu. 

Hãy xem xét đầu vào:```
abcde
```Đầu ra đúng là:```
AbCdE
```Nếu chúng ta xử lý sai chỉ mục`0`như một vị trí chẵn theo nghĩa của phát biểu vấn đề, chúng ta sẽ tạo ra:```
aBcDe
```Một sai lầm dễ mắc phải khác là quên chuẩn hóa vỏ ban đầu trước khi áp dụng mẫu. Đầu vào có thể đã chứa các chữ cái viết hoa ở những vị trí tùy ý. 

Ví dụ:```
aBcD
```Đầu ra đúng là:```
AbCd
```Nếu chúng ta chỉ sửa đổi các ký tự ở vị trí lẻ và giữ nguyên các ký tự khác thì kết quả có thể không chính xác. 

Chuỗi ký tự đơn là một trường hợp ranh giới khác:```
z
```Câu trả lời phải là:```
Z
```Vị trí đầu tiên luôn là số lẻ nên ký tự luôn phải viết hoa. 

## Phương pháp tiếp cận 

Cách thức bạo lực ở đây đã đơn giản rồi. Chúng ta có thể lặp qua từng ký tự trong chuỗi, kiểm tra xem vị trí hiện tại là số lẻ hay số chẵn và nối thêm phiên bản chữ hoa hoặc chữ thường của ký tự đó vào câu trả lời. 

Vì độ dài tối đa chỉ là 100 nên thậm chí việc xây dựng lại chuỗi nhiều lần vẫn đủ nhanh. Việc triển khai đơn giản bằng cách sử dụng phép nối chuỗi bên trong một vòng lặp thực hiện tối đa khoảng 100 phép nối, con số này không đáng kể. 

Cách tiếp cận rõ ràng hơn và có khả năng mở rộng hơn sử dụng danh sách các ký tự và nối chúng ở cuối. Điều này tránh việc phân bổ lại chuỗi lặp đi lặp lại và phù hợp với phong cách lập trình cạnh tranh tiêu chuẩn. 

Quan sát quan trọng là mỗi ký tự độc lập với mọi ký tự khác. Không có sự tương tác giữa các vị trí, không có trạng thái lập trình động và không cần tiền xử lý. Trường hợp mong muốn chỉ phụ thuộc vào số vị trí là số lẻ hay số chẵn. 

Điều đó làm giảm toàn bộ vấn đề thành một đường tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tạo một danh sách trống để lưu trữ các ký tự được chuyển đổi. 
3. Lặp lại chuỗi bằng cách sử dụng các chỉ số từ`0`ĐẾN`n - 1`. 
4. Đối với mỗi ký tự, xác định xem vị trí của nó trong chỉ mục dựa trên 1 là lẻ hay chẵn. 

Vì các chỉ mục Python dựa trên 0 nên chỉ mục`0`tương ứng với vị trí`1`, chỉ số`1`tương ứng với vị trí`2`, vân vân. 
5. Nếu chỉ mục là số chẵn, hãy chuyển ký tự thành chữ hoa và thêm nó vào danh sách câu trả lời. 

Các chỉ số chẵn tương ứng với các vị trí lẻ trong phát biểu bài toán. 
6. Nếu không, hãy chuyển ký tự này thành chữ thường và nối thêm ký tự đó. 
7. Nối tất cả các ký tự trong danh sách thành một chuỗi duy nhất. 
8. In kết quả. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, thuật toán thực thi chính xác quy tắc mà câu lệnh yêu cầu. chỉ mục`i`tương ứng với vị trí`i + 1`. 

Nếu như`i`thì chẵn`i + 1`là số lẻ nên ký tự phải viết hoa. 

Nếu như`i`thế thì kỳ quặc`i + 1`là số chẵn nên ký tự phải là chữ thường. 

Mỗi ký tự được chuyển đổi độc lập theo đúng quy tắc vị trí nên sau khi xử lý tất cả các vị trí, chuỗi kết quả đều thỏa mãn yêu cầu viết hoa xen kẽ ở mọi nơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

ans = []

for i, ch in enumerate(s):
    if i % 2 == 0:
        ans.append(ch.upper())
    else:
        ans.append(ch.lower())

print("".join(ans))
```Chương trình bắt đầu bằng cách đọc chuỗi đầu vào và xóa dòng mới ở cuối bằng`strip()`. 

Câu trả lời được xây dựng bằng cách sử dụng danh sách vì việc thêm vào danh sách sẽ hiệu quả. Sau khi xử lý tất cả các ký tự,`"".join(ans)`xây dựng chuỗi cuối cùng trong một thao tác. 

Chi tiết triển khai quan trọng là kiểm tra tính chẵn lẻ. Vấn đề sử dụng vị trí dựa trên 1, nhưng chỉ số Python bắt đầu từ 0. Điều đó có nghĩa là chỉ mục`0`đại diện cho vị trí`1`, là số lẻ và phải viết hoa. 

Đây là lý do tại sao điều kiện kiểm tra:```
if i % 2 == 0
```thay vì kiểm tra các chỉ số lẻ. 

sử dụng`upper()`Và`lower()`trực tiếp đảm bảo tính chính xác ngay cả khi dữ liệu đầu vào đã chứa chữ viết hoa hỗn hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Codeforces
```| chỉ mục`i`| Nhân vật | Chức vụ`i+1`| Hành động | Kết quả cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | C | 1 | chữ hoa | C | 
| 1 | o | 2 | chữ thường | Có | 
| 2 | d | 3 | chữ hoa | CoD | 
| 3 | e | 4 | chữ thường | Mã Đê | 
| 4 | f | 5 | chữ hoa | CoDeF | 
| 5 | o | 6 | chữ thường | CoDeFo | 
| 6 | r | 7 | chữ hoa | CoDeForR | 
| 7 | c | 8 | chữ thường | CoDeForRc | 
| 8 | e | 9 | chữ hoa | CoDeForRcE | 
| 9 | s | 10 | chữ thường | CoDeForRcEs | 

Đầu ra cuối cùng:```
CoDeFoRcEs
```Dấu vết này cho thấy mô hình xen kẽ rõ ràng. Mọi vị trí lẻ sẽ trở thành chữ hoa và mọi vị trí chẵn sẽ trở thành chữ thường. 

### Ví dụ 2 

đầu vào:```
aBcD
```| chỉ mục`i`| Nhân vật | Chức vụ`i+1`| Hành động | Kết quả cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | một | 1 | chữ hoa | A | 
| 1 | B | 2 | chữ thường | Ab | 
| 2 | c | 3 | chữ hoa | AbC | 
| 3 | D | 4 | chữ thường | AbCd | 

Đầu ra cuối cùng:```
AbCd
```Ví dụ này chứng minh tại sao chúng ta phải gọi một cách rõ ràng`upper()`Và`lower()`thay vì giả sử đầu vào đã có cách viết hoa chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Danh sách câu trả lời lưu trữ chuỗi đã chuyển đổi | 

Với`n ≤ 100`, chương trình sẽ chạy ngay lập tức trong giới hạn. Việc sử dụng bộ nhớ cũng rất nhỏ vì thuật toán chỉ lưu trữ chuỗi kết quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()

    ans = []

    for i, ch in enumerate(s):
        if i % 2 == 0:
            ans.append(ch.upper())
        else:
            ans.append(ch.lower())

    print("".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("Codeforces\n") == "CoDeFoRcEs", "sample 1"

# minimum size
assert run("z\n") == "Z", "single character"

# already alternating
assert run("AbCdE\n") == "AbCdE", "already correct"

# all lowercase
assert run("abcdef\n") == "AbCdEf", "lowercase input"

# all uppercase
assert run("ABCDEF\n") == "AbCdEf", "uppercase input"

# off-by-one check
assert run("ab\n") == "Ab", "index parity correctness"

# maximum length
assert run(("a" * 100) + "\n") == ("Ab" * 50), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`z`|`Z`| Đầu vào có độ dài tối thiểu | 
|`AbCdE`|`AbCdE`| Mẫu đã đúng | 
|`abcdef`|`AbCdEf`| Chuẩn hóa chữ thường | 
|`ABCDEF`|`AbCdEf`| Chuẩn hóa chữ hoa | 
|`ab`|`Ab`| Xử lý đúng tính chẵn lẻ dựa trên 1 | 
|`a * 100`|`AbAb...`| Độ dài tối đa cho phép | 

## Vỏ cạnh 

Chuỗi ký tự đơn là đầu vào nhỏ nhất có thể:```
z
```Thuật toán xử lý chỉ số`0`. Từ`0`chẵn thì ký tự sẽ trở thành chữ hoa. Đầu ra là:```
Z
```Điều này xác nhận rằng vị trí đầu tiên được xử lý chính xác. 

Viết hoa hỗn hợp có thể cho thấy việc triển khai không bình thường hóa chữ hoa chữ thường một cách chính xác. 

đầu vào:```
aBcD
```Thuật toán không tin cậy vào cách viết hoa hiện có. Tại chỉ mục`1`, nhân vật`B`được chuyển thành chữ thường vì nó tương ứng với vị trí`2`. Tại chỉ mục`2`, nhân vật`c`trở thành chữ hoa vì nó tương ứng với vị trí`3`. 

Kết quả cuối cùng là:```
AbCd
```Các lỗi lỗi xuất hiện dễ dàng khi chuyển đổi giữa lập chỉ mục dựa trên 0 và dựa trên 1. 

đầu vào:```
ab
```chỉ mục`0`bản đồ tới vị trí`1`, Vì thế`a`trở thành chữ hoa. 

chỉ mục`1`bản đồ tới vị trí`2`, Vì thế`b`trở thành chữ thường. 

Đầu ra là:```
Ab
```Thay vào đó, một điều kiện chẵn lẻ không chính xác sẽ tạo ra`aB`, vi phạm tuyên bố vấn đề.
