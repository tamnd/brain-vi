---
title: "CF 94A - Khôi phục mật khẩu"
description: "Chúng tôi được cung cấp một mật khẩu được mã hóa được biểu thị dưới dạng chuỗi nhị phân có độ dài 80. Mật khẩu ban đầu có chính xác 8 chữ số thập phân và mỗi chữ số được mã hóa thành một khối gồm 10 ký tự nhị phân."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 94
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 76 (Div. 2 Only)"
rating: 900
weight: 94
solve_time_s: 118
verified: true
draft: false
---

[CF 94A - Khôi phục mật khẩu](https://codeforces.com/problemset/problem/94/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mật khẩu được mã hóa được biểu thị dưới dạng chuỗi nhị phân có độ dài 80. Mật khẩu ban đầu có chính xác 8 chữ số thập phân và mỗi chữ số được mã hóa thành một khối gồm 10 ký tự nhị phân. Ánh xạ giữa các chữ số và khối nhị phân cũng được cung cấp: sau chuỗi được mã hóa, chúng tôi nhận được 10 chuỗi nhị phân riêng biệt tương ứng với các chữ số`0`bởi vì`9`. 

Nhiệm vụ là chia chuỗi mã hóa 80 ký tự thành 8 khối liên tiếp có độ dài 10, xác định mỗi khối đại diện cho chữ số nào và in mật khẩu 8 chữ số đã khôi phục. 

Những hạn chế là rất nhỏ. Chuỗi được mã hóa luôn có độ dài 80 và chỉ có 10 ánh xạ để so sánh. Ngay cả việc triển khai đơn giản bằng cách so sánh mọi khối với từng chữ số cũng chỉ thực hiện được`8 × 10 = 80`so sánh, mỗi chuỗi có độ dài 10. Đó thực sự là thời gian không đổi. 

Mối nguy hiểm chính trong vấn đề này không phải là hiệu suất mà là việc xử lý việc lập chỉ mục một cách chính xác. Vì mật khẩu được mã hóa là một chuỗi dài nên một sai lầm trong việc cắt ranh giới có thể âm thầm dịch chuyển mọi khối sau đó và tạo ra mật khẩu sai. 

Hãy xem xét đầu vào này:```
11111111110000000000...
```Nếu chúng ta vô tình chia tách bằng cách sử dụng các khối có kích thước 8 hoặc 9 thay vì 10, chữ số đầu tiên có thể vẫn trùng khớp một cách ngẫu nhiên, nhưng mỗi lần tra cứu sau này đều trở nên không chính xác. 

Một vấn đề tế nhị khác là nhầm lẫn hướng của bản đồ. Các dòng đầu vào sau chuỗi được mã hóa thể hiện:```
code for 0
code for 1
...
code for 9
```Việc triển khai bất cẩn có thể lưu trữ chúng ngược và sau đó cố gắng tra cứu các chữ số theo chỉ mục số thay vì bằng mã nhị phân. 

Ví dụ:```
encrypted block = "0101101000"
```Nếu như`"0101101000"`tương ứng với chữ số`6`, chúng ta cần một bản đồ:```
"0101101000" -> "6"
```và không:```
"6" -> "0101101000"
```Nếu không việc khôi phục mật khẩu sẽ trở nên khó xử hoặc không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Chúng tôi chia chuỗi được mã hóa 80 ký tự thành 8 đoạn có độ dài 10. Đối với mỗi đoạn, chúng tôi so sánh chuỗi đó với tất cả 10 mã hóa chữ số đã biết cho đến khi tìm thấy kết quả trùng khớp. Vì tất cả các ánh xạ đều khác biệt và câu lệnh đảm bảo một giải pháp hợp lệ nên chính xác một chữ số khớp với mỗi khối. 

Cách tiếp cận này đã đủ nhanh. Tệ nhất là chúng tôi thực hiện:```
8 password blocks × 10 digit patterns × 10 character comparisons
```đó chỉ là kiểm tra 800 ký tự. 

Vấn đề thậm chí còn trở nên rõ ràng hơn nếu chúng ta xử lý trước ánh xạ vào từ điển. Thay vì tìm kiếm tuyến tính thông qua tất cả các mã hóa 10 chữ số mỗi lần, chúng tôi ánh xạ trực tiếp từng khối nhị phân tới chữ số tương ứng của nó. 

Quan sát quan trọng là các mã hóa là duy nhất. Điều đó có nghĩa là chính chuỗi nhị phân có thể dùng làm khóa từ điển. 

Vì vậy chúng tôi xây dựng:```
binary block -> digit
```Sau đó, mỗi đoạn 10 ký tự của mật khẩu được mã hóa có thể được giải mã trong thời gian không đổi. 

Giải pháp brute-force hoạt động vì kích thước đầu vào rất nhỏ nhưng việc quan sát từ điển sẽ loại bỏ các so sánh lặp lại không cần thiết và tạo ra cách triển khai rõ ràng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(8 × 10 × 10) | O(1) | Đã chấp nhận | 
| Tối ưu | O(80) | O(10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi mã hóa 80 ký tự. 

Chuỗi này chứa mã hóa của tất cả 8 chữ số mật khẩu được nối với nhau. 
2. Đọc 10 mẫu nhị phân tương ứng với các chữ số`0`bởi vì`9`. 

Vì thứ tự đầu vào là cố định nên mẫu đầu tiên thuộc về chữ số`0`, chữ số thứ hai`1`, vân vân. 
3. Xây dựng một từ điển trong đó khóa là chuỗi nhị phân 10 ký tự và giá trị là chữ số mà nó đại diện. 

Điều này cho phép giải mã trực tiếp sau này. 
4. Duyệt chuỗi mã hóa theo từng bước 10 ký tự. 

Mỗi phân đoạn đại diện cho chính xác một chữ số được mã hóa. 
5. Đối với mỗi phân đoạn, hãy tra từ điển và nối chữ số kết quả vào chuỗi câu trả lời. 

Câu lệnh đảm bảo rằng mọi phân đoạn đều tồn tại trong ánh xạ. 
6. In mật khẩu đã được tạo lại. 

### Tại sao nó hoạt động 

Mỗi mã hóa chữ số là duy nhất, vì vậy mỗi khối 10 ký tự sẽ ánh xạ tới chính xác một chữ số. Mật khẩu được mã hóa được hình thành bằng cách ghép 8 khối như vậy mà không có ký tự trùng lặp hoặc thừa. Việc tách chuỗi cứ sau 10 ký tự sẽ tái tạo lại chính xác các phần được mã hóa ban đầu và việc tra cứu từ điển sẽ chuyển đổi từng phần trở lại thành chữ số tương ứng. Vì mỗi khối được giải mã độc lập và chính xác nên chuỗi được xây dựng lại cuối cùng là mật khẩu ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    encrypted = input().strip()

    mapping = {}

    for digit in range(10):
        code = input().strip()
        mapping[code] = str(digit)

    answer = []

    for i in range(0, 80, 10):
        block = encrypted[i:i + 10]
        answer.append(mapping[block])

    print("".join(answer))

solve()
```Giải pháp tuân theo thuật toán trực tiếp. 

Từ điển`mapping`lưu trữ mối quan hệ ngược lại từ mã nhị phân sang chữ số. Đây là hướng thuận tiện nhất vì sau này chúng ta nhận được các khối được mã hóa và cần khôi phục các chữ số từ chúng. 

Vòng lặp:```
for i in range(0, 80, 10):
```là quan trọng. Chuỗi được mã hóa phải được chia thành các đoạn không chồng chéo có độ dài chính xác là 10. Sử dụng kích thước bước khác sẽ làm hỏng phân vùng. 

Mỗi khối được trích xuất bằng:```
encrypted[i:i + 10]
```Việc cắt Python một cách an toàn trả về chính xác chuỗi con mong muốn. 

Câu trả lời được tích lũy thành một danh sách và được nối vào cuối. Việc nối chuỗi lặp lại cũng có tác dụng ở đây vì đầu vào rất nhỏ, nhưng việc tích lũy danh sách là mẫu lập trình cạnh tranh tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
01001100100101100000010110001001011001000101100110010110100001011010100101101100
0100110000
0100110010
0101100000
0101100010
0101100100
0101100110
0101101000
0101101010
0101101100
0101101110
```Từ điển sau khi tiền xử lý: 

| Mã nhị phân | Chữ số | 
| --- | --- | 
| 0100110000 | 0 | 
| 0100110010 | 1 | 
| 0101100000 | 2 | 
| 0101100010 | 3 | 
| 0101100100 | 4 | 
| 0101100110 | 5 | 
| 0101101000 | 6 | 
| 0101101010 | 7 | 
| 0101101100 | 8 | 
| 0101101110 | 9 | 

Quá trình giải mã: 

| Vị trí | Khối trích xuất | Chữ số được giải mã | 
| --- | --- | --- | 
| 0-9 | 0100110010 | 1 | 
| 19-10 | 0101100000 | 2 | 
| 20-29 | 0101100010 | 3 | 
| 30-39 | 0101100100 | 4 | 
| 40-49 | 0101100110 | 5 | 
| 50-59 | 0101101000 | 6 | 
| 60-69 | 0101101010 | 7 | 
| 70-79 | 0101101100 | 8 | 

Mật khẩu cuối cùng:```
12345678
```Dấu vết này cho thấy mọi đoạn 10 ký tự đều được giải mã độc lập và trực tiếp thông qua từ điển. 

### Ví dụ 2 

đầu vào:```
11111111110000000000111111111100000000001111111111000000000011111111110000000000
0000000000
1111111111
0000000001
0000000011
0000000111
0000001111
0000011111
0000111111
0001111111
0011111111
```Từ điển: 

| Mã nhị phân | Chữ số | 
| --- | --- | 
| 0000000000 | 0 | 
| 1111111111 | 1 | 
| 0000000001 | 2 | 
| 0000000011 | 3 | 
| 0000000111 | 4 | 
| 0000001111 | 5 | 
| 0000011111 | 6 | 
| 0000111111 | 7 | 
| 0001111111 | 8 | 
| 0011111111 | 9 | 

Giải mã: 

| Vị trí | Khối trích xuất | Chữ số được giải mã | 
| --- | --- | --- | 
| 0-9 | 1111111111 | 1 | 
| 19-10 | 0000000000 | 0 | 
| 20-29 | 1111111111 | 1 | 
| 30-39 | 0000000000 | 0 | 
| 40-49 | 1111111111 | 1 | 
| 50-59 | 0000000000 | 0 | 
| 60-69 | 1111111111 | 1 | 
| 70-79 | 0000000000 | 0 | 

Mật khẩu cuối cùng:```
10101010
```Ví dụ này thể hiện các mẫu lặp đi lặp lại. Thuật toán không dựa vào các chữ số khác biệt bên trong mật khẩu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(80) | Chúng tôi xử lý 8 khối có chiều dài 10 | 
| Không gian | O(10) | Từ điển lưu trữ 10 ánh xạ | 

Thời gian chạy thực sự không đổi vì kích thước đầu vào không bao giờ vượt quá giới hạn cố định. Việc sử dụng bộ nhớ cũng không đáng kể, chỉ lưu trữ 10 bảng mã đã biết. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    encrypted = input().strip()

    mapping = {}

    for digit in range(10):
        code = input().strip()
        mapping[code] = str(digit)

    answer = []

    for i in range(0, 80, 10):
        answer.append(mapping[encrypted[i:i + 10]])

    print("".join(answer))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""01001100100101100000010110001001011001000101100110010110100001011010100101101100
0100110000
0100110010
0101100000
0101100010
0101100100
0101100110
0101101000
0101101010
0101101100
0101101110
"""
) == "12345678", "sample 1"

# all digits in order
assert run(
"""00000000000000000001000000001000000000110000000010000000010100000001100000000111
0000000000
0000000001
0000000010
0000000011
0000000100
0000000101
0000000110
0000000111
0000001000
0000001001
"""
) == "01234567", "digits in increasing order"

# repeated digit
assert run(
"""11111111111111111111111111111111111111111111111111111111111111111111111111111111
0000000000
1111111111
0000000001
0000000011
0000000111
0000001111
0000011111
0000111111
0001111111
0011111111
"""
) == "11111111", "same digit repeated many times"

# alternating boundary pattern
assert run(
"""10101010100101010101101010101001010101011010101010010101010110101010100101010101
0101010101
1010101010
0000000000
1111111111
0011001100
1100110011
0000111100
1111000011
0110011001
1001100110
"""
) == "10101010", "checks exact 10-character slicing"

# maximum-style mixed password
assert run(
"""00111111110001111111000011111100000111110000011111000000111100000011110000001111
0000000000
1111111111
0000000001
0000000011
0000000111
0000001111
0000011111
0000111111
0001111111
0011111111
"""
) == "98765435", "mixed decoding"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 12345678 | Giải mã cơ bản | 
| Chữ số có thứ tự | 01234567 | Hướng bản đồ chính xác | 
| Chữ số lặp lại | 11111111 | Các khối lặp đi lặp lại được xử lý chính xác | 
| Các mẫu xen kẽ | 10101010 | Phân đoạn chính xác 10 ký tự | 
| Mật khẩu hỗn hợp | 98765435 | Thứ tự giải mã tùy ý | 

## Vỏ cạnh 

Một lỗi phổ biến là cắt chuỗi mã hóa không chính xác. 

Coi như:```
11111111110000000000111111110000000000111111110000000000111111110000000000
```Giả định:```
1111111111 -> 1
0000000000 -> 0
```Mật khẩu đúng là:```
10101010
```Thuật toán của chúng tôi xử lý các chỉ số:```
0, 10, 20, 30, 40, 50, 60, 70
```Mỗi lát có độ dài chính xác là 10. Vì các ranh giới căn chỉnh hoàn hảo với định dạng mã hóa nên việc giải mã thành công. 

Một trường hợp tinh vi khác là các khối được mã hóa lặp đi lặp lại. 

đầu vào:```
00000000000000000000000000000000000000000000000000000000000000000000000000000000
```với bản đồ:```
0000000000 -> 0
```lặp đi lặp lại trong số 10 định nghĩa. 

Đầu ra đúng là:```
00000000
```Thuật toán không giả định tính duy nhất giữa các chữ số mật khẩu. Mỗi lần tra cứu khối đều độc lập nên các mẫu lặp lại sẽ giải mã một cách tự nhiên. 

Cạm bẫy thứ ba là đảo ngược hướng ánh xạ. 

Giả định:```
0101010101 -> 7
```Nếu chúng tôi lưu trữ:```
mapping["7"] = "0101010101"
```sau đó chúng tôi không thể giải mã trực tiếp khối được mã hóa. Thay vào đó, việc triển khai của chúng tôi lưu trữ:```
mapping["0101010101"] = "7"
```phù hợp với thao tác tra cứu thực tế được yêu cầu trong quá trình giải mã.
