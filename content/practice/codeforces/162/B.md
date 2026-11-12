---
title: "CF 162B - Ký hiệu nhị phân"
description: "Chúng ta được cấp một số nguyên dương duy nhất và phải in biểu diễn của nó theo cơ số 2. Nói cách khác, thay vì biểu thị số đó dưới dạng lũy ​​thừa của 10, chúng ta biểu diễn nó dưới dạng lũy ​​thừa của 2 chỉ sử dụng các chữ số 0 và 1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1800
weight: 162
solve_time_s: 80
verified: true
draft: false
---

[CF 162B - Ký hiệu nhị phân](https://codeforces.com/problemset/problem/162/B) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên dương duy nhất và phải in biểu diễn của nó theo cơ số 2. Nói cách khác, thay vì biểu thị số dưới dạng lũy thừa của 10, chúng ta biểu thị nó dưới dạng lũy thừa của 2 chỉ sử dụng các chữ số`0`Và`1`. 

Ví dụ, số thập phân`5`trở thành`101`bởi vì:$$5 = 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0$$Hạn chế là rất nhỏ. Giá trị của`n`nhiều nhất là`10^6`, có nghĩa là biểu diễn nhị phân của nó chứa ít hơn 20 bit vì:$$2^{20} = 1,\!048,\!576$$Ngay cả những giải pháp không hiệu quả cũng sẽ hoạt động thoải mái trong giới hạn. Việc quét tuyến tính trên tất cả các vị trí bit có thể có ở đây đã là chuyện nhỏ. 

Nguồn gốc của lỗi chính là định dạng đầu ra một cách chính xác. Ký hiệu nhị phân không được chứa số 0 đứng đầu. 

Hãy xem xét đầu vào:```
1
```Đầu ra đúng là:```
1
```Việc triển khai bất cẩn luôn in một số bit cố định, chẳng hạn như 32 bit, sẽ tạo ra:```
00000000000000000000000000000001
```điều đó là sai. 

Một trường hợp tinh tế khác xuất hiện khi liên tục chia cho hai. Phần còn lại được tạo từ bit có trọng số thấp nhất đến bit có trọng số cao nhất, vì vậy chúng phải được đảo ngược trước khi in. 

Ví dụ:```
6
```Trình tự phân chia: 

| Số hiện tại | Phần còn lại | 
| --- | --- | 
| 6 | 0 | 
| 3 | 1 | 
| 1 | 1 | 

Các bit được thu thập là`011`, nhưng câu trả lời đúng là:```
110
```In các bit theo thứ tự tập hợp sẽ âm thầm tạo ra số nhị phân đảo ngược. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là kiểm tra mọi lũy thừa của hai và xác định xem bit đó có trong số hay không. Vì giá trị tối đa là dưới đây`2^20`, chúng ta có thể lặp từ bit cao nhất xuống 0 và in`1`hoặc`0`tùy thuộc vào việc sức mạnh tương ứng có đóng góp vào số lượng hay không. 

Điều này hiệu quả vì mỗi số nguyên có một phân tách nhị phân duy nhất. Đối với mỗi vị trí, chúng tôi hỏi liệu giá trị còn lại có chứa lũy thừa bằng hai hay không. 

Ngay cả khi chúng tôi kiểm tra tất cả các vị trí 32 hoặc 64 bit, thời gian chạy vẫn sẽ không đổi. Vấn đề không phải là hiệu suất mà là sự rõ ràng trong việc triển khai. Chúng ta cũng phải cẩn thận bỏ qua các số 0 đứng đầu. 

Một cách tiếp cận cổ điển khác là chia số đó nhiều lần cho hai và ghi lại số dư. Mỗi phần còn lại trở thành một chữ số nhị phân. Phần còn lại đầu tiên tương ứng với bit có trọng số nhỏ nhất, phần còn lại tiếp theo tương ứng với bit tiếp theo, v.v. 

Lý do hoạt động này được gắn trực tiếp với ký hiệu vị trí. Khi chia cho hai:$$n = 2q + r$$Ở đâu`r`là một trong hai`0`hoặc`1`. Phần còn lại đó chính xác là chữ số nhị phân hiện tại. 

Cách tiếp cận phép chia đơn giản hơn vì nó trích xuất một cách tự nhiên từng chữ số biểu diễn nhị phân mà không cần suy luận thủ công về lũy thừa của hai. 

Vì mỗi phép chia chia số đó ra làm đôi nên số lần lặp lại bằng số bit trong câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(log n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(log n) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Tạo danh sách trống`bits`để lưu trữ các chữ số nhị phân. 
3. Trong khi`n`lớn hơn 0, hãy tính`n % 2`và nối kết quả vào`bits`. 

Phần còn lại cho chúng ta biết liệu bit có ý nghĩa nhỏ nhất hiện tại có phải là`0`hoặc`1`. 
4. Thay thế`n`với`n // 2`. 

Phép chia số nguyên loại bỏ chữ số nhị phân có ý nghĩa nhỏ nhất mà chúng tôi đã xử lý. 
5. Sau khi vòng lặp kết thúc, đảo ngược các chữ số đã thu thập. 

Phần còn lại được trích xuất đầu tiên tương ứng với bit có trọng số thấp nhất, nhưng ký hiệu nhị phân được viết từ bit có trọng số cao nhất đến bit có trọng số nhỏ nhất. 
6. Nối các chữ số thành một chuỗi và in kết quả. 

### Tại sao nó hoạt động 

Mỗi lần lặp lại phân tách số hiện tại thành:$$n = 2q + r$$Ở đâu`r`là một trong hai`0`hoặc`1`. Số dư đó chính xác là chữ số nhị phân thấp nhất của`n`. 

Sau khi loại bỏ chữ số đó bằng phép chia số nguyên, logic tương tự sẽ áp dụng cho thương số. Lặp lại quá trình này sẽ trích xuất mọi chữ số nhị phân chính xác một lần. 

Thuật toán dừng khi thương số bằng 0, nghĩa là tất cả các vị trí nhị phân đã được xử lý. Đảo ngược các chữ số đã thu thập sẽ khôi phục đúng thứ tự từ trái sang phải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

bits = []

while n > 0:
    bits.append(str(n % 2))
    n //= 2

bits.reverse()

print("".join(bits))
```Chương trình bắt đầu bằng cách đọc số nguyên từ đầu vào tiêu chuẩn. 

các`while`vòng lặp liên tục trích xuất bit có ý nghĩa nhỏ nhất bằng cách sử dụng`n % 2`. Từ`% 2`chỉ có thể sản xuất`0`hoặc`1`, mỗi số dư là một chữ số nhị phân hợp lệ. 

Sau khi ghi lại chữ số, mã thực hiện phép chia số nguyên cho hai. Điều này loại bỏ bit được xử lý và dịch chuyển số sang phải một vị trí nhị phân. 

Các chữ số được thu thập theo thứ tự ngược lại vì bit có trọng số thấp nhất xuất hiện đầu tiên trong quá trình chia lặp lại. Đảo ngược danh sách trước khi in sẽ khôi phục cách trình bày chính xác. 

Việc sử dụng các chuỗi bên trong danh sách sẽ tránh được việc lặp lại các chuyển đổi từ số nguyên sang chuỗi trong khi nối. 

Điều kiện vòng lặp`while n > 0`hoạt động vì vấn đề đảm bảo`n`là tích cực. Nếu cho phép số 0, chúng ta sẽ cần một trường hợp đặc biệt để in`"0"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Dấu vết thực hiện: 

| Hiện tại | n % 2 | bit sau khi nối thêm | 
| --- | --- | --- | 
| 5 | 1 | [1] | 
| 2 | 0 | [1, 0] | 
| 1 | 1 | [1, 0, 1] | 

Sau khi đảo ngược: 

| Bit đảo ngược | Đầu ra | 
| --- | --- | 
| [1, 0, 1] | 101 | 

Ví dụ này cho thấy cách biểu diễn nhị phân đối xứng trong đó việc đảo ngược không làm thay đổi trình tự một cách rõ ràng. Bất biến vẫn giữ nguyên: mọi phần còn lại tương ứng với bit có trọng số thấp nhất hiện tại. 

### Ví dụ 2 

đầu vào:```
6
```Dấu vết thực hiện: 

| Hiện tại | n % 2 | bit sau khi nối thêm | 
| --- | --- | --- | 
| 6 | 0 | [0] | 
| 3 | 1 | [0, 1] | 
| 1 | 1 | [0, 1, 1] | 

Sau khi đảo ngược: 

| Bit đảo ngược | Đầu ra | 
| --- | --- | 
| [1, 1, 0] | 110 | 

Dấu vết này chứng minh tại sao sự đảo ngược là cần thiết. Nếu không đảo ngược, chúng ta sẽ in sai`011`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần lặp chia số đó cho 2 | 
| Không gian | O(log n) | Biểu diễn nhị phân lưu trữ một chữ số trên mỗi bit | 

Đầu vào tối đa dưới đây`2^20`, do đó vòng lặp thực hiện tối đa 20 lần. Cả thời gian chạy và mức sử dụng bộ nhớ đều rất nhỏ so với giới hạn của sự cố. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())

    bits = []

    while n > 0:
        bits.append(str(n % 2))
        n //= 2

    bits.reverse()

    print("".join(bits))

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
assert run("5\n") == "101", "sample 1"

# minimum input
assert run("1\n") == "1", "minimum value"

# power of two
assert run("8\n") == "1000", "single set bit"

# mixed bits
assert run("13\n") == "1101", "general binary conversion"

# maximum constraint region
assert run("1000000\n") == "11110100001001000000", "large value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào hợp lệ tối thiểu | 
|`8`|`1000`| Xử lý đúng quyền hạn của hai | 
|`13`|`1101`| Chuyển đổi chung với các bit hỗn hợp | 
|`1000000`|`11110100001001000000`| Đầu vào lớn gần giới hạn trên | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là số hợp lệ nhỏ nhất. 

đầu vào:```
1
```Thực thi: 

| Hiện tại | n % 2 | bit | 
| --- | --- | --- | 
| 1 | 1 | [1] | 

Sau khi đảo ngược, đầu ra vẫn là:```
1
```Điều này xác nhận thuật toán không đưa ra các số 0 đứng đầu. 

Một trường hợp tế nhị khác là lũy thừa của hai. 

đầu vào:```
8
```Thực thi: 

| Hiện tại | n % 2 | bit | 
| --- | --- | --- | 
| 8 | 0 | [0] | 
| 4 | 0 | [0, 0] | 
| 2 | 0 | [0, 0, 0] | 
| 1 | 1 | [0, 0, 0, 1] | 

Sau khi đảo chiều:```
1000
```Điều này xác minh rằng các số 0 ở cuối trong bộ sưu tập đảo ngược trở thành các số 0 kết thúc hợp lệ trong biểu diễn nhị phân cuối cùng. 

Trường hợp cạnh thứ ba là một số có chuỗi số dư đảo ngược rõ ràng khác với đáp án. 

đầu vào:```
6
```Số dư đã thu thập:```
011
```Sau khi đảo chiều:```
110
```Điều này chứng tỏ tại sao việc đảo ngược danh sách chữ số là cần thiết để đảm bảo tính chính xác.
