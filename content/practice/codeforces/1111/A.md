---
title: "CF 1111A - Biến hình siêu anh hùng"
description: "Chúng ta được cấp hai chuỗi chữ thường đại diện cho tên các siêu anh hùng. Được phép chuyển đổi nếu mọi nguyên âm có thể được thay đổi thành bất kỳ nguyên âm nào khác và mọi phụ âm có thể được thay đổi thành bất kỳ phụ âm nào khác. Các chữ cái thực tế không quan trọng."
date: "2026-06-12T05:02:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1111
codeforces_index: "A"
codeforces_contest_name: "CodeCraft-19 and Codeforces Round 537 (Div. 2)"
rating: 1000
weight: 1111
solve_time_s: 323
verified: false
draft: false
---

[CF 1111A - Biến hình siêu anh hùng](https://codeforces.com/problemset/problem/1111/A) 

**Đánh giá:** 1000 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 5 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp hai chuỗi chữ thường đại diện cho tên các siêu anh hùng. Được phép chuyển đổi nếu mọi nguyên âm có thể được thay đổi thành bất kỳ nguyên âm nào khác và mọi phụ âm có thể được thay đổi thành bất kỳ phụ âm nào khác. 

Các chữ cái thực tế không quan trọng. Điều quan trọng là mỗi vị trí có chứa nguyên âm hay phụ âm hay không. Ví dụ,`'a'`có thể trở thành`'u'`vì cả hai đều là nguyên âm và`'b'`có thể trở thành`'z'`vì cả hai đều là phụ âm. Nguyên âm không bao giờ có thể trở thành phụ âm và phụ âm không bao giờ có thể trở thành nguyên âm. 

Nhiệm vụ là xác định xem tên đầu tiên có thể được chuyển đổi thành tên thứ hai hay không bằng cách sử dụng các quy tắc này. 

Các chuỗi có độ dài tối đa là 1000, rất nhỏ. Ngay cả một thuật toán quét chuỗi nhiều lần cũng có thể đủ nhanh. Điều duy nhất quan trọng là thực hiện chính xác điều kiện chuyển đổi. 

Một quan sát quan trọng là việc thay đổi ký tự không bao giờ làm thay đổi độ dài chuỗi. Mỗi thao tác thay thế một ký tự bằng một ký tự khác cùng loại. Nếu hai tên có độ dài khác nhau thì câu trả lời ngay là “Không”. 

Có một số trường hợp khó bỏ sót. 

Coi như:```
a
bb
```Câu trả lời đúng là:```
No
```Một giải pháp bất cẩn chỉ so sánh số lượng nguyên âm/phụ âm có thể chấp nhận sai. Độ dài bằng nhau là bắt buộc. 

Coi như:```
ab
cd
```Câu trả lời đúng là:```
No
```Vị trí đầu tiên là nguyên âm và phụ âm. Mặc dù cả hai chuỗi đều chứa một nguyên âm và một phụ âm nhưng vị trí vẫn quan trọng. 

Coi như:```
ae
io
```Câu trả lời đúng là:```
Yes
```Các chữ cái khác nhau, nhưng mọi vị trí đều là nguyên âm so với nguyên âm. 

Coi như:```
code
java
```Câu trả lời đúng là:```
No
```Ở một số vị trí, một chuỗi chứa phụ âm trong khi chuỗi kia chứa nguyên âm. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng xem liệu mỗi ký tự có thể được thay đổi thành ký tự tương ứng của chuỗi kia hay không. Đối với mọi vị trí, chúng ta có thể hỏi liệu ký tự nguồn và ký tự đích có thuộc cùng một loại hay không. Nếu bất kỳ vị trí nào vi phạm quy tắc này, việc chuyển đổi là không thể. 

Vì mỗi chuỗi có tối đa 1000 ký tự nên việc kiểm tra trực tiếp mọi vị trí chỉ cần 1000 so sánh. Thế là đã đủ nhanh rồi. 

Cái nhìn sâu sắc quan trọng là danh tính thực sự của một bức thư không bao giờ quan trọng. Các quy tắc chuyển đổi cho phép bất kỳ nguyên âm nào trở thành bất kỳ nguyên âm nào và bất kỳ phụ âm nào trở thành phụ âm bất kỳ. Điều này có nghĩa là mỗi ký tự có thể được rút gọn thành một bit thông tin: nguyên âm hoặc phụ âm. 

Sau quan sát đó, vấn đề trở nên rất đơn giản. Đầu tiên hãy xác minh rằng độ dài bằng nhau. Sau đó quét cả hai chuỗi theo từng vị trí. Tại mỗi chỉ số, hãy kiểm tra xem cả hai ký tự đều là nguyên âm hay cả hai đều là phụ âm. Nếu không, việc chuyển đổi là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Trong vấn đề này, cách tiếp cận bạo lực tự nhiên đã là tối ưu vì kích thước đầu vào rất nhỏ và mỗi ký tự phải được kiểm tra ít nhất một lần. 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi. 
2. Nếu độ dài của chúng khác nhau, hãy xuất ra`"No"`và dừng lại. 

Một phép biến đổi chỉ thay thế các ký tự. Nó không bao giờ chèn hoặc xóa ký tự. 
3. Tạo một bộ chứa các nguyên âm`{a, e, i, o, u}`. 
4. Lặp lại tất cả các vị trí của chuỗi. 
5. Đối với mỗi vị trí, xác định xem ký tự ở chuỗi đầu tiên có phải là nguyên âm hay không và ký tự ở chuỗi thứ hai có phải là nguyên âm hay không. 
6. Nếu một ký tự là nguyên âm và ký tự còn lại là phụ âm, hãy xuất ra`"No"`và dừng lại. 

Một vị trí như vậy không bao giờ có thể được chuyển đổi theo các hoạt động được phép. 
7. Nếu toàn bộ quá trình quét kết thúc mà không tìm thấy điểm không khớp, hãy xuất ra`"Yes"`. 

### Tại sao nó hoạt động 

Một phép biến đổi có hiệu lực chính xác khi mọi vị trí có thể được biến đổi một cách độc lập. 

Ở bất kỳ vị trí nào cũng chỉ có hai khả năng: nguyên âm hoặc phụ âm. Nguyên âm có thể trở thành bất kỳ nguyên âm nào và phụ âm có thể trở thành bất kỳ phụ âm nào. Do đó, một vị trí có thể được chuyển đổi khi và chỉ khi cả hai ký tự thuộc cùng một danh mục. 

Nếu các chuỗi có độ dài khác nhau thì không có chuỗi thay thế nào có thể làm cho chúng bằng nhau. Nếu độ dài bằng nhau và mọi vị trí đều có các danh mục phù hợp, chúng ta có thể chọn sự thay thế thích hợp ở mỗi vị trí và thu được chuỗi đích. Các điều kiện này đều cần và đủ nên thuật toán đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
t = input().strip()

if len(s) != len(t):
    print("No")
    sys.exit()

vowels = set("aeiou")

for a, b in zip(s, t):
    if (a in vowels) != (b in vowels):
        print("No")
        break
else:
    print("Yes")
```Kiểm tra đầu tiên xử lý hạn chế toàn cầu duy nhất: độ dài phải khớp. 

các`vowels`set cho phép kiểm tra tư cách thành viên liên tục. Với mỗi cặp ký tự, biểu thức```
(a in vowels) != (b in vowels)
```đúng khi một ký tự là nguyên âm và ký tự kia là phụ âm. Ngay khi tìm được vị trí như vậy, câu trả lời sẽ là`"No"`. 

các`for-else`việc xây dựng ở đây rất thuận tiện. các`else`khối chỉ thực thi nếu vòng lặp kết thúc mà không gặp phải`break`, nghĩa là mọi vị trí đều thỏa mãn điều kiện yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a
u
```| Vị trí | s[i] | t[i] | nguyên âm s[i]? | nguyên âm t[i]? | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | bạn | Có | Có | hợp lệ | 

Không có sự không phù hợp được tìm thấy. Mọi vị trí đều khớp theo danh mục, vì vậy câu trả lời là:```
Yes
```Ví dụ này chứng minh rằng nguyên âm chính xác không quan trọng. Bất kỳ nguyên âm nào cũng có thể trở thành bất kỳ nguyên âm nào khác. 

### Ví dụ 2 

đầu vào:```
ab
cd
```| Vị trí | s[i] | t[i] | nguyên âm s[i]? | nguyên âm t[i]? | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | c | Có | Không | Không hợp lệ | 

Vị trí đầu tiên đã chứa nguyên âm và phụ âm. Thuật toán ngay lập tức dừng lại và in:```
No
```Ví dụ này cho thấy rằng số lượng nguyên âm và phụ âm trùng nhau là không liên quan. Danh mục phải phù hợp ở mọi vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được kiểm tra một lần | 
| Không gian | O(1) | Chỉ một bộ nguyên âm nhỏ và một vài biến được lưu trữ | 

Độ dài tối đa chỉ là 1000, do đó, một lần quét tuyến tính dễ dàng nằm trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi và không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()
    t = input().strip()

    if len(s) != len(t):
        print("No")
        return

    vowels = set("aeiou")

    for a, b in zip(s, t):
        if (a in vowels) != (b in vowels):
            print("No")
            return

    print("Yes")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("a\nu\n") == "Yes", "sample 1"

# custom cases
assert run("a\nb\n") == "No", "vowel versus consonant"
assert run("ab\ncd\n") == "No", "position mismatch"
assert run("aeiou\nuoiea\n") == "Yes", "all vowels"
assert run("b\nz\n") == "Yes", "single consonants"
assert run(("a" * 1000) + "\n" + ("e" * 1000) + "\n") == "Yes", "maximum length"
assert run("abc\nab\n") == "No", "different lengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / b`|`No`| Nguyên âm không thể trở thành phụ âm | 
|`ab / cd`|`No`| Cần phải kết hợp vị trí khôn ngoan | 
|`aeiou / uoiea`|`Yes`| Bất kỳ nguyên âm nào cũng có thể trở thành bất kỳ nguyên âm nào khác | 
|`b / z`|`Yes`| Bất kỳ phụ âm nào cũng có thể trở thành bất kỳ phụ âm nào khác | 
| Độ dài 1000 chuỗi nguyên âm |`Yes`| Kích thước đầu vào tối đa | 
|`abc / ab`|`No`| Độ dài không khớp | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
a
bb
```Thuật toán đầu tiên so sánh độ dài. Từ`1 != 2`, nó in ngay:```
No
```Không cần kiểm tra nhân vật. Điều này xử lý thực tế là các phép biến đổi không bao giờ thay đổi độ dài chuỗi. 

Hãy xem xét đầu vào:```
ab
ba
```Tại vị trí 0,`'a'`là một nguyên âm trong khi`'b'`là một phụ âm. Thuật toán phát hiện một danh mục không khớp và in:```
No
```Điều này cho thấy tại sao chỉ kiểm tra tổng số nguyên âm sẽ không chính xác. Vị trí quan trọng. 

Hãy xem xét đầu vào:```
ae
io
```Cả hai chuỗi đều có độ dài 2. 

Ở vị trí 0, nguyên âm khớp với nguyên âm. 

Ở vị trí 1, nguyên âm khớp với nguyên âm. 

Quá trình quét hoàn tất thành công nên thuật toán sẽ in:```
Yes
```Điều này xác nhận rằng các chữ cái chính xác là không liên quan. Chỉ có phân loại nguyên âm/phụ âm mới quan trọng. 

Hãy xem xét đầu vào:```
bcdf
wxyz
```Mỗi vị trí đều chứa phụ âm và phụ âm. Thuật toán không bao giờ tìm thấy sự không khớp và in:```
Yes
```Điều này xác minh quy tắc tương tự cho phụ âm. Bất kỳ phụ âm nào cũng có thể được thay thế bằng bất kỳ phụ âm nào khác.
