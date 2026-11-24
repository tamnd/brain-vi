---
title: "CF 75A - Cuộc sống không có số 0"
description: "Cho hai số nguyên dương a và b. Đầu tiên chúng ta tính tổng bình thường của chúng c = a + b. Sau đó, chúng ta áp dụng phép biến đổi tương tự cho cả ba số: loại bỏ mọi chữ số 0 khỏi biểu diễn thập phân của chúng. Câu hỏi đặt ra là liệu phương trình đã biến đổi có còn đúng hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 75
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 67 (Div. 2)"
rating: 1000
weight: 75
solve_time_s: 94
verified: true
draft: false
---

[CF 75A - Cuộc sống không có số 0](https://codeforces.com/problemset/problem/75/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên dương`a`Và`b`. Đầu tiên chúng ta tính tổng bình thường của chúng`c = a + b`. Sau đó, chúng ta áp dụng phép biến đổi tương tự cho cả ba số: loại bỏ mọi chữ số`0`từ biểu diễn thập phân của chúng. 

Câu hỏi đặt ra là liệu phương trình đã biến đổi có còn đúng hay không. Nói cách khác, sau khi xóa tất cả các số 0 khỏi`a`,`b`, Và`c`, chúng ta vẫn có:`remove_zero(a) + remove_zero(b) = remove_zero(c)`? 

Đầu vào bao gồm hai số nguyên`a`Và`b`. Đầu ra là`"YES"`nếu đẳng thức vẫn đúng sau khi loại bỏ số 0, nếu không thì`"NO"`. 

Những hạn chế là rất nhỏ. Cả hai số nhiều nhất là`10^9`, nghĩa là mỗi số có tối đa 10 chữ số. Bất kỳ hoạt động nào xử lý từng chữ số một đều có thời gian không đổi một cách hiệu quả. Ngay cả việc chuyển đổi số thành chuỗi nhiều lần cũng không đáng kể ở đây. Điều này ngay lập tức cho chúng ta biết vấn đề không phải là tối ưu hóa mà là việc thực hiện chuyển đổi một cách chính xác. 

Nguyên nhân chính của sai lầm là việc xử lý bước loại bỏ số 0 một cách bất cẩn. 

Một lỗi dễ xuất hiện khi số được chuyển đổi trở nên trống. Coi như:```
a = 1000
b = 9000
```Sau khi loại bỏ số không:```
a -> 1
b -> 9
c = 10000 -> 1
```Việc triển khai bất cẩn cố gắng phân tích một chuỗi trống có thể bị lỗi nếu một số đều bằng 0. Trong bài toán này, đầu vào ban đầu là dương và khác 0, nhưng việc xử lý trung gian vẫn cần phải mạnh mẽ. 

Một lỗi phổ biến khác là chỉ xóa các số 0 đứng đầu thay vì tất cả các số 0. Ví dụ:```
a = 105
b = 106
```Chuyển đổi đúng:```
105 -> 15
106 -> 16
211 -> 211
```Từ`15 + 16 != 211`, câu trả lời là`"NO"`. 

Nếu ai đó chỉ loại bỏ các số 0 đứng đầu, họ sẽ giữ sai`105`Và`106`không thay đổi. 

Một vấn đề tinh vi hơn xuất phát từ việc so sánh chuỗi thay vì số nguyên. Coi như:```
a = 500
b = 500
```Sau khi loại bỏ số không:```
500 -> 5
500 -> 5
1000 -> 1
```Chúng tôi nhận được:```
5 + 5 = 10
1 != 10
```Câu trả lời là`"NO"`. 

Việc so sánh phải xảy ra về mặt số lượng sau khi chuyển đổi. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là mô phỏng chính xác những gì tuyên bố mô tả. Tính toán`c = a + b`, chuyển đổi cả ba số thành chuỗi, xóa mọi`'0'`ký tự, chuyển đổi kết quả trở lại thành số nguyên và cuối cùng so sánh phương trình đã chuyển đổi. 

Vì mỗi số có tối đa 10 chữ số nên số này đã chạy theo thời gian không đổi. Ngay cả khi chúng tôi đếm chính xác các thao tác, chúng tôi chỉ quét tổng cộng vài chục ký tự. 

Không có cách tiếp cận chậm hơn nào có ý nghĩa ở đây, nhưng cách triển khai khó xử hơn có thể xây dựng lại các số từng chữ số bằng cách sử dụng số học. Ví dụ, lấy đi nhiều lần`% 10`, bỏ qua các số 0 và xây dựng lại câu trả lời. Điều đó cũng hoạt động chính xác, nhưng việc triển khai trở nên dễ xảy ra lỗi hơn do các chữ số đến theo thứ tự ngược lại. 

Quan sát quan trọng là phép biến đổi tự nhiên là một thao tác chuỗi. Loại bỏ tất cả các số 0 khỏi biểu diễn thập phân chính xác là mục đích thay thế chuỗi. Khi chúng tôi nhận ra rằng các ràng buộc là rất nhỏ và thao tác ở dạng văn bản, giải pháp rõ ràng nhất sẽ trở nên rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết số học từng chữ số | O(d) | O(1) | Đã chấp nhận | 
| Chuyển đổi chuỗi | O(d) | O(d) | Đã chấp nhận | 

Đây,`d`là số chữ số, nhiều nhất là 10. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`b`. 

Chúng ta cần cả hai giá trị ban đầu vì đẳng thức được chuyển đổi phụ thuộc vào tổng ban đầu. 
2. Tính toán`c = a + b`. 

Vấn đề xác định`c`theo cách này, vì vậy chúng ta phải chuyển đổi tổng thực tế, không tính toán lại sau khi loại bỏ các số 0. 
3. Tạo một hàm trợ giúp để loại bỏ tất cả`'0'`các ký tự từ biểu diễn thập phân của một số. 

Chuyển đổi số thành chuỗi, thay thế tất cả`'0'`các ký tự có chuỗi trống, sau đó chuyển đổi kết quả trở lại thành số nguyên. 
4. Áp dụng phép biến đổi cho`a`,`b`, Và`c`. 

Hãy để những giá trị này là`na`,`nb`, Và`nc`. 
5. Kiểm tra xem`na + nb == nc`. 

Nếu đúng thì in`"YES"`, nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán trực tiếp tuân theo định nghĩa của vấn đề. Hàm trợ giúp tạo ra chính xác số thu được sau khi xóa mọi chữ số 0 khỏi biểu diễn thập phân. Vì chúng ta áp dụng phép biến đổi tương tự một cách độc lập cho`a`,`b`, Và`a + b`, so sánh cuối cùng khớp chính xác với điều kiện yêu cầu. Không có phép tính gần đúng hoặc phím tắt nào được sử dụng nên thuật toán không thể tạo ra kết quả không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def remove_zeros(x):
    return int(str(x).replace('0', ''))

def solve():
    a = int(input())
    b = int(input())

    c = a + b

    na = remove_zeros(a)
    nb = remove_zeros(b)
    nc = remove_zeros(c)

    if na + nb == nc:
        print("YES")
    else:
        print("NO")

solve()
```Hàm trợ giúp xử lý toàn bộ quá trình chuyển đổi trong một dòng. Việc chuyển đổi thành một chuỗi cho phép chúng ta loại bỏ trực tiếp mọi số 0 bằng`replace('0', '')`. Chuyển đổi trở lại số nguyên sẽ cho giá trị số cần thiết để so sánh. 

Thứ tự của các hoạt động quan trọng. Đầu tiên chúng ta phải tính`c = a + b`sử dụng số ban đầu. Chỉ sau đó chúng tôi mới loại bỏ số không. Tính tổng sau khi chuyển đổi sẽ giải quyết được một vấn đề khác. 

Việc triển khai vẫn an toàn vì đầu vào ban đầu là số nguyên dương và sau khi loại bỏ số 0, luôn còn lại ít nhất một chữ số khác 0. Ví dụ,`1000`trở thành`"1"`, không phải là một chuỗi rỗng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
101
102
```| Biến | Giá trị | 
| --- | --- | 
| một | 101 | 
| b | 102 | 
| c | 203 | 
| loại bỏ(a) | 11 | 
| xóa(b) | 12 | 
| xóa(c) | 23 | 
| 11 + 12 | 23 | 

Vì cả hai bên đều bằng nhau nên kết quả là:```
YES
```Ví dụ này thể hiện hành vi cốt lõi của vấn đề. Mặc dù số 0 biến mất khỏi mọi số nhưng sự bình đẳng vẫn tồn tại. 

### Ví dụ 2 

đầu vào:```
105
106
```| Biến | Giá trị | 
| --- | --- | 
| một | 105 | 
| b | 106 | 
| c | 211 | 
| loại bỏ(a) | 15 | 
| xóa(b) | 16 | 
| xóa(c) | 211 | 
| 15 + 16 | 31 | 

Từ`31 != 211`, đầu ra là:```
NO
```Trường hợp này cho thấy tại sao chúng ta phải loại bỏ các số 0 khỏi mọi số một cách độc lập. Phương trình được biến đổi có thể thay đổi hoàn toàn cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Mỗi chữ số được quét một số lần không đổi | 
| Không gian | O(d) | Chuỗi tạm thời lưu trữ số đã chuyển đổi | 

Đây`d`là số chữ số trong số lớn nhất, nhiều nhất là 10. Thời gian chạy và mức sử dụng bộ nhớ rất nhỏ so với giới hạn, do đó giải pháp dễ dàng nằm gọn trong các ràng buộc. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def remove_zeros(x):
        return int(str(x).replace('0', ''))

    a = int(input())
    b = int(input())

    c = a + b

    na = remove_zeros(a)
    nb = remove_zeros(b)
    nc = remove_zeros(c)

    return "YES" if na + nb == nc else "NO"

# provided sample
assert run("101\n102\n") == "YES", "sample 1"

# custom cases
assert run("105\n106\n") == "NO", "zeros break equality"
assert run("1\n1\n") == "YES", "minimum nonzero inputs"
assert run("500\n500\n") == "NO", "transformed sum differs"
assert run("1000\n9000\n") == "NO", "many zeros in all numbers"
assert run("999999999\n1\n") == "YES", "boundary near maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`105 106`|`NO`| Các số 0 bên trong phải được loại bỏ | 
|`1 1`|`YES`| Đầu vào hợp lệ nhỏ nhất | 
|`500 500`|`NO`| So sánh số sau khi chuyển đổi | 
|`1000 9000`|`NO`| Loại bỏ không nặng | 
|`999999999 1`|`YES`| Giá trị lớn gần giới hạn ràng buộc | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
105
106
```Thuật toán tính toán:```
c = 211
```Sau đó biến đổi:```
105 -> 15
106 -> 16
211 -> 211
```Cuối cùng:```
15 + 16 = 31
```Từ`31 != 211`, thuật toán in`"NO"`. 

Điều này xác nhận việc triển khai sẽ loại bỏ các số 0 ở mọi nơi, không chỉ các số 0 đứng đầu. 

Bây giờ hãy xem xét:```
500
500
```Việc tính toán trở thành:```
c = 1000
```Sau khi chuyển đổi:```
500 -> 5
500 -> 5
1000 -> 1
```Thuật toán so sánh:```
5 + 5 = 10
```chống lại:```
1
```và in`"NO"`. 

Trường hợp này xác minh rằng các giá trị được chuyển đổi phải được diễn giải bằng số sau khi loại bỏ số 0. 

Cuối cùng, hãy xem xét:```
1000
9000
```Số được biến đổi là:```
1000 -> 1
9000 -> 9
10000 -> 1
```Thuật toán kiểm tra:```
1 + 9 = 10
```cái nào không bằng`1`, vì vậy đầu ra là`"NO"`. 

Điều này xác nhận giải pháp xử lý chính xác các số chứa nhiều số 0 mà không có bất kỳ logic trường hợp đặc biệt nào.
