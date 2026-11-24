---
title: "CF 71A - Từ Quá Dài"
description: "Chúng ta được cung cấp một số từ viết thường và chỉ phải rút ngắn những từ được coi là \"quá dài\". Một từ sẽ quá dài nếu độ dài của nó lớn hơn 10. Quy tắc rút gọn rất cụ thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 71
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 65 (Div. 2)"
rating: 800
weight: 71
solve_time_s: 88
verified: true
draft: false
---

[CF 71A - Từ ngữ quá dài](https://codeforces.com/problemset/problem/71/A) 

**Đánh giá:** 800 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số từ viết thường và chỉ phải rút ngắn những từ được coi là "quá dài". Một từ sẽ quá dài nếu độ dài của nó lớn hơn 10. 

Quy tắc rút ngắn rất cụ thể. Chúng tôi giữ lại ký tự đầu tiên và ký tự cuối cùng, sau đó thay thế mọi thứ ở giữa bằng số lượng ký tự bị xóa. Ví dụ,`"localization"`có 12 ký tự nên chúng ta giữ nguyên`'l'`Và`'n'`, và có 10 ký tự giữa chúng. Kết quả trở thành`"l10n"`. 

Mỗi từ đầu vào được xử lý độc lập. Đối với mỗi từ, chúng tôi in không thay đổi hoặc in chữ viết tắt của nó. 

Những hạn chế là rất nhỏ. Có tối đa 100 từ và mỗi từ có độ dài tối đa là 100. Ngay cả giải pháp đơn giản theo từng ký tự cũng dễ dàng nằm trong giới hạn. Tổng số lượng văn bản được xử lý là rất nhỏ nên bất kỳ cách tiếp cận thời gian tuyến tính nào cũng là quá đủ. 

Phần khó khăn không phải là hiệu suất, mà là xử lý chính xác các điều kiện biên. 

Một lỗi phổ biến là viết tắt các từ có độ dài chính xác bằng 10. Điều kiện phải lớn hơn 10. 

Ví dụ đầu vào:```
1
abcdefghij
```Đầu ra đúng:```
abcdefghij
```Việc thực hiện bất cẩn bằng cách sử dụng`>= 10`sẽ tạo ra sai:```
a8j
```Một sai lầm khác là tính toán số đếm ở giữa không chính xác. Số được chèn là số ký tự giữa ký tự đầu tiên và ký tự cuối cùng, chính là`len(word) - 2`. 

Ví dụ đầu vào:```
1
localization
```Đầu ra đúng:```
l10n
```Nếu ai đó sử dụng`len(word) - 1`, họ sẽ in sai:```
l11n
```Các từ có một ký tự là một trường hợp đặc biệt khác đáng để kiểm tra. 

Ví dụ đầu vào:```
1
a
```Đầu ra đúng:```
a
```Từ này không đủ dài để viết tắt nên phải giữ nguyên không thay đổi. Việc truy cập ký tự đầu tiên và cuối cùng ở đây vẫn hợp lệ vì chúng giống nhau. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề là mô phỏng lực lượng vũ phu. Đối với mỗi từ, chúng tôi kiểm tra độ dài của nó. Nếu độ dài tối đa là 10 thì ta in chữ không thay đổi. Ngược lại, chúng ta tạo chữ viết tắt bằng cách lấy ký tự đầu tiên, tính xem có bao nhiêu ký tự nằm ở giữa và thêm ký tự cuối cùng. 

Điều này đã hoạt động hiệu quả vì các ràng buộc là cực kỳ nhỏ. Ngay cả khi mỗi từ có độ dài 100, chúng tôi sẽ xử lý tổng cộng tối đa 10.000 ký tự. Đó là hiệu quả ngay lập tức. 

Một biến thể brute-force chậm hơn theo nghĩa đen sẽ đếm từng ký tự ở giữa thay vì sử dụng số học. Ví dụ: đối với một từ có độ dài 20, chúng ta có thể lặp từ chỉ mục 1 đến chỉ mục 18 và tăng bộ đếm. Điều này vẫn trôi qua vì đầu vào rất nhỏ nhưng nó thực hiện những công việc không cần thiết. 

Điều quan trọng là số lượng ký tự bị loại bỏ không cần phải đếm thủ công. Khi chúng ta biết độ dài từ, số đếm ở giữa luôn là:```
length - 2
```Điều đó làm giảm hoạt động về thời gian không đổi cho mỗi từ, ngoài việc đọc chính đầu vào. 

Ý tưởng bạo lực có hiệu quả vì mọi từ đều độc lập. Không có sự tương tác giữa các từ, không có cấu trúc dữ liệu phức tạp và không có thủ thuật tối ưu hóa ẩn. Nhiệm vụ duy nhất là áp dụng quy tắc chuyển đổi cố định một cách nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đếm các chữ cái ở giữa theo cách thủ công | O(tổng ký tự) | O(1) | Đã chấp nhận | 
| Xây dựng số học tối ưu | O(tổng ký tự) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số lượng từ. 
2. Lặp lại`n`lần và đọc một từ mỗi lần lặp. 
3. Kiểm tra độ dài từ. 

Nếu độ dài nhỏ hơn hoặc bằng 10 thì in ra chữ không thay đổi vì không bị coi là quá dài. 
4. Ngược lại, viết tắt. 

Lấy ký tự đầu tiên bằng cách sử dụng`word[0]`. 
5. Tính xem ký tự đầu tiên và ký tự cuối cùng có bao nhiêu ký tự. 

Giá trị này là`len(word) - 2`bởi vì chúng tôi loại trừ chính xác hai ký tự, ký tự đầu tiên và ký tự cuối cùng. 
6. Lấy ký tự cuối cùng bằng cách sử dụng`word[-1]`. 
7. Nối ký tự đầu tiên, số ở giữa được chuyển thành chuỗi và ký tự cuối cùng, sau đó in kết quả. 

### Tại sao nó hoạt động 

Thuật toán trực tiếp tuân theo định nghĩa của chữ viết tắt. Mỗi từ dài được chuyển thành:```
first letter + number of middle letters + last letter
```Số lượng chữ cái ở giữa luôn bằng tổng chiều dài trừ đi hai ký tự ranh giới được giữ nguyên. Vì mỗi từ được xử lý độc lập và phép biến đổi khớp chính xác với định dạng được yêu cầu nên thuật toán luôn tạo ra kết quả đầu ra chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

for _ in range(n):
    word = input().strip()

    if len(word) <= 10:
        print(word)
    else:
        abbreviated = word[0] + str(len(word) - 2) + word[-1]
        print(abbreviated)
```Chương trình bắt đầu bằng việc đọc số từ. Sau đó, nó xử lý từng từ một cách độc lập. 

các`strip()`cuộc gọi sẽ xóa ký tự dòng mới ở cuối khỏi đầu vào. Nếu không có nó, dòng mới sẽ trở thành một phần của độ dài chuỗi và tạo ra kết quả không chính xác. 

điều kiện`len(word) <= 10`khớp chính xác với tuyên bố vấn đề. sử dụng`< 10`hoặc`>= 10`sẽ giới thiệu một lỗi riêng lẻ. 

Đối với những từ dài, chữ viết tắt được xây dựng từ ba phần.`word[0]`đưa ra ký tự đầu tiên,`len(word) - 2`đưa ra số lượng ký tự ở giữa bị bỏ qua và`word[-1]`đưa ra nhân vật cuối cùng. 

sử dụng`word[-1]`thuận tiện vì nó luôn đề cập đến ký tự cuối cùng bất kể độ dài từ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
word
localization
internationalization
pneumonoultramicroscopicsilicovolcanoconiosis
```Dấu vết: 

| Lời | Chiều dài | Quá dài? | Đếm giữa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| từ | 4 | Không | - | từ | 
| nội địa hóa | 12 | Có | 10 | l10n | 
| quốc tế hóa | 20 | Có | 18 | i18n | 
| bệnh phổi siêu nhỏsilicovolcanoconiosis | 45 | Có | 43 | p43s | 

Dấu vết này cho thấy quy tắc trung tâm của vấn đề. Những từ ngắn vẫn được giữ nguyên, trong khi những từ dài được nén vào chữ cái đầu tiên, số đếm ở giữa và chữ cái cuối cùng. 

### Ví dụ 2 

đầu vào:```
5
abcdefghij
abcdefghijk
a
codeforces
short
```Dấu vết: 

| Lời | Chiều dài | Quá dài? | Đếm giữa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| abcdefghij | 10 | Không | - | abcdefghij | 
| abcdefghijk | 11 | Có | 9 | a9k | 
| một | 1 | Không | - | một | 
| lực lượng mật mã | 10 | Không | - | lực lượng mật mã | 
| ngắn | 5 | Không | - | ngắn | 

Ví dụ này tập trung vào điều kiện biên. Các từ có độ dài đúng 10 không được viết tắt. Chỉ có độ dài lớn hơn 10 mới đủ điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng ký tự) | Mỗi từ được đọc một lần và được xử lý theo thời gian tuyến tính tương ứng với độ dài của nó | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Các giới hạn rất nhỏ nên giải pháp này dễ dàng phù hợp với cả giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. Ngay cả việc xử lý từng ký tự riêng lẻ cũng đủ nhanh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    out = []

    for _ in range(n):
        word = input().strip()

        if len(word) <= 10:
            out.append(word)
        else:
            out.append(word[0] + str(len(word) - 2) + word[-1])

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""4
word
localization
internationalization
pneumonoultramicroscopicsilicovolcanoconiosis
"""
) == (
"""word
l10n
i18n
p43s
"""
), "sample 1"

# minimum-size input
assert run(
"""1
a
"""
) == (
"""a
"""
), "single character word"

# boundary length 10 should not abbreviate
assert run(
"""2
abcdefghij
codeforces
"""
) == (
"""abcdefghij
codeforces
"""
), "length exactly 10"

# length 11 should abbreviate
assert run(
"""1
abcdefghijk
"""
) == (
"""a9k
"""
), "length 11"

# multiple equal long words
assert run(
"""3
localization
localization
localization
"""
) == (
"""l10n
l10n
l10n
"""
), "repeated inputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Độ dài từ tối thiểu có thể | 
|`abcdefghij`|`abcdefghij`| Điều kiện biên tại độ dài 10 | 
|`abcdefghijk`|`a9k`| Độ dài đầu tiên nên viết tắt | 
| lặp đi lặp lại`localization`| lặp đi lặp lại`l10n`| Xử lý nhất quán trên nhiều từ | 

## Vỏ cạnh 

Một từ có độ dài đúng bằng 10 phải không thay đổi. 

đầu vào:```
1
abcdefghij
```Thuật toán kiểm tra`len(word) <= 10`. Vì độ dài chính xác là 10 nên điều kiện đúng và từ gốc được in. 

Đầu ra:```
abcdefghij
```Điều này tránh được lỗi thường gặp khi viết tắt các từ không thay đổi. 

Một từ rất dài phải tính toán chính xác số đếm ở giữa. 

đầu vào:```
1
internationalization
```Độ dài là 20. Thuật toán tính:```
20 - 2 = 18
```Sau đó, nó kết hợp:```
i + 18 + n
```Đầu ra:```
i18n
```Điều này xác nhận rằng số đếm chỉ đề cập đến các ký tự giữa chữ cái đầu tiên và chữ cái cuối cùng. 

Các từ có một ký tự cũng hoạt động chính xác. 

đầu vào:```
1
a
```Độ dài là 1 nên từ này không được viết tắt. Thuật toán in trực tiếp từ gốc. 

Đầu ra:```
a
```Không cần xử lý trường hợp đặc biệt nào vì điều kiện ngăn chặn logic viết tắt không cần thiết.
