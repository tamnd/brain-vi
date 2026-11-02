---
title: "CF 108B - Kiểu dữ liệu"
description: "Chúng ta được cung cấp một số kiểu dữ liệu số nguyên không dấu, mỗi kiểu được xác định bởi độ dài bit của nó. Kiểu dữ liệu có bit có thể lưu trữ mọi số nguyên từ 0 đến 2^a - 1. Chúng tôi muốn biết liệu có tồn tại một số số nguyên x và hai kiểu dữ liệu có kích thước a[i] < a[j] sao cho: 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 108
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 83 (Div. 2 Only)"
rating: 1400
weight: 108
solve_time_s: 247
verified: true
draft: false
---

[CF 108B - Kiểu dữ liệu](https://codeforces.com/problemset/problem/108/B) 

**Đánh giá:** 1400 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 4 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số kiểu dữ liệu số nguyên không dấu, mỗi kiểu được xác định bởi độ dài bit của nó. Một kiểu dữ liệu với`a`bit có thể lưu trữ mọi số nguyên từ`0`lên tới`2^a - 1`. 

Chúng tôi muốn biết liệu có tồn tại một số nguyên`x`và hai kiểu dữ liệu có kích thước`a[i] < a[j]`như vậy: 

1.`x`phù hợp với kiểu dữ liệu nhỏ hơn. 
2.`x * x`không vừa với kiểu dữ liệu lớn hơn. 

Nếu một cặp như vậy tồn tại, ngôn ngữ sẽ hoạt động kỳ lạ đến mức Tuftuf quyết định từ bỏ nó. 

Quan sát quan trọng là giá trị thực tế của`x`vấn đề chỉ thông qua sức mạnh của hai. Vì số lượng tối đa có thể biểu diễn trong`a`bit là`2^a - 1`, bình phương lớn nhất có thể từ các số phù hợp`a`đại khái là bit`(2^a)^2 = 2^(2a)`. Vì vậy, câu hỏi trở thành: 

Có thể bình phương một số từ một`a`-loại bit yêu cầu nhiều hơn`b`bit, ở đâu`a < b`? 

Kích thước đầu vào đạt`10^5`, do đó, bất kỳ thuật toán nào kiểm tra trực tiếp mọi cặp theo thời gian bậc hai sẽ thực hiện xung quanh`10^10`so sánh trong trường hợp xấu nhất Điều đó vượt xa những gì phù hợp với giới hạn 2 giây. Chúng ta cần thứ gì đó gần gũi hơn`O(n log n)`hoặc`O(n)`. 

Một phần tinh tế của vấn đề là điều kiện liên quan đến sự tồn tại chứ không phải tính phổ quát. Chúng ta chỉ cần một số nguyên`x`phá vỡ kiểu dữ liệu lớn hơn. Một giải pháp bất cẩn có thể chỉ kiểm tra giá trị tối đa của kiểu dữ liệu nhỏ hơn và bỏ lỡ các nhân chứng đơn giản hơn. 

Hãy xem xét đầu vào này:```
2
3 4
```Câu trả lời đúng là`"YES"`bởi vì`x = 7`phù hợp với 3 bit, nhưng`49`không phù hợp với 4 bit. Việc triển khai ngây thơ chỉ lý do với số lượng bit không chính thức có thể nghĩ không chính xác rằng việc tăng kiểu dữ liệu lên một bit luôn là đủ. 

Một lỗi dễ mắc phải khác là xử lý sai kích thước kiểu dữ liệu bằng nhau.```
3
5 5 5
```Câu trả lời đúng là`"NO"`bởi vì điều kiện rõ ràng yêu cầu`a[i] < a[j]`. Kích thước bằng nhau không thể tạo thành một cặp hợp lệ. 

Cạm bẫy thứ ba là tràn ngập các ngôn ngữ có số nguyên có chiều rộng cố định. Nếu ai đó tính toán`2^a`trực tiếp cho lớn`a`, giá trị như`2^10^9`không thể lưu trữ được. Giải pháp mong muốn không bao giờ cần lũy thừa thực tế của hai, chỉ có mối quan hệ giữa các số mũ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp kích thước kiểu dữ liệu được sắp xếp`(a, b)`Ở đâu`a < b`, chúng tôi hỏi liệu có tồn tại một số giá trị phù hợp trong`a`bit có bình phương vượt quá giá trị tối đa có thể biểu diễn trong`b`bit. 

Một con số phù hợp`a`bit nếu:```
x ≤ 2^a - 1
```Hình vuông của nó không vừa với`b`bit nếu:```
x^2 > 2^b - 1
```Hình vuông lớn nhất có thể đến từ việc lấy`x = 2^a - 1`. Bình phương nó sẽ cho ta xấp xỉ`2^(2a)`. Điều đó có nghĩa là hình vuông có thể vượt quá`b`-bit giới hạn bất cứ khi nào:```
2a > b
```Vì vậy, giải pháp vũ phu trở thành: 

1. Kiểm tra từng cặp`(a, b)`với`a < b`. 
2. Nếu`2a > b`, trả lời`"YES"`. 

Logic này đúng, nhưng việc kiểm tra tất cả các cặp có giá`O(n^2)`. Với`n = 10^5`, điều này trở nên không sử dụng được. 

Thông tin chi tiết quan trọng là chỉ có kích thước kiểu dữ liệu liên quan nhỏ nhất và lớn nhất mới quan trọng sau khi sắp xếp. 

Giả sử mảng được sắp xếp:```
a[0] ≤ a[1] ≤ ... ≤ a[n-1]
```Đối với một số kích thước kiểu dữ liệu`a[i]`, chúng tôi muốn biết liệu có tồn tại kiểu dữ liệu lớn hơn không`a[j]`thỏa mãn:```
a[j] < 2 * a[i]
```Vì mảng đã được sắp xếp nên ứng cử viên dễ dàng nhất là kiểu dữ liệu nhỏ nhất lớn hơn rất nhiều so với`a[i]`. Nếu thậm chí kiểu dữ liệu đó ít nhất là`2 * a[i]`, thì mọi kiểu dữ liệu sau này cũng quá lớn và không tồn tại cặp hợp lệ nào cho kiểu dữ liệu này`a[i]`. 

Vì vậy sau khi sắp xếp, chúng ta chỉ cần so sánh các kích thước khác biệt lân cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) ngoài việc sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước kiểu dữ liệu thành một mảng. 
2. Sắp xếp mảng theo thứ tự không giảm. 

Việc sắp xếp cho phép chúng ta kiểm tra kích thước kiểu dữ liệu theo thứ tự tăng dần và lý giải hiệu quả về các ứng cử viên lớn hơn. 
3. Lặp qua các cặp liền kề trong mảng đã sắp xếp. 
4. Đối với mỗi cặp liền kề`(a[i], a[i+1])`, trước tiên hãy kiểm tra xem chúng có hoàn toàn khác nhau không. 

Kích thước bằng nhau không thể thỏa mãn điều kiện vì bài toán yêu cầu`a[i] < a[j]`. 
5. Nếu`a[i+1] < 2 * a[i]`, in`"YES"`và chấm dứt. 

Điều này có nghĩa là tồn tại một kiểu dữ liệu nhỏ hơn với kích thước`a[i]`và kiểu dữ liệu lớn hơn với kích thước`a[i+1]`sao cho bình phương một số từ loại nhỏ hơn yêu cầu nhiều bit hơn loại lớn hơn có thể chứa. 
6. Nếu vòng lặp kết thúc mà không tìm thấy cặp nào như vậy, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Một kiểu dữ liệu với`a`bit có thể lưu trữ số lượng lên đến`2^a - 1`. Bình phương giá trị lớn nhất có thể cho:```
(2^a - 1)^2 = 2^(2a) - 2^(a+1) + 1
```Giá trị này ít nhất là`2^b`bất cứ khi nào`2a > b`, nghĩa là nó không còn vừa bên trong nữa`b`bit. 

Vì vậy, toàn bộ vấn đề giảm xuống việc tìm hai kích thước kiểu dữ liệu với:```
a < b < 2a
```Sau khi sắp xếp, nếu một cặp như vậy tồn tại ở bất kỳ đâu thì cũng phải tồn tại một cặp trong số các giá trị phân biệt liền kề. Giả định`(a[i], a[j])`hợp lệ với`i < j`. Vì mảng được sắp xếp:```
a[i+1] ≤ a[j] < 2a[i]
```Vậy cặp liền kề`(a[i], a[i+1])`đã đủ rồi. Kiểm tra các cặp liền kề là đủ để phát hiện mọi cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    for i in range(n - 1):
        if a[i] < a[i + 1] and a[i + 1] < 2 * a[i]:
            print("YES")
            return

    print("NO")

solve()
```Phần đầu tiên đọc đầu vào và sắp xếp kích thước kiểu dữ liệu. Sắp xếp là nền tảng của việc tối ưu hóa vì nó cho phép chúng ta giảm việc tìm kiếm sang các giá trị lân cận. 

Vòng lặp chỉ kiểm tra các cặp liền kề. Điều kiện:```
a[i] < a[i + 1]
```lọc ra các kích thước kiểu dữ liệu bằng nhau, không hợp lệ theo câu lệnh. 

Điều kiện thứ hai:```
a[i + 1] < 2 * a[i]
```xuất phát trực tiếp từ đạo hàm toán học. Nếu bất đẳng thức này đúng thì một số nào đó phù hợp`a[i]`bit có một hình vuông đòi hỏi nhiều hơn`a[i+1]`bit. 

Việc triển khai không bao giờ tính toán lũy thừa thực tế của 2, do đó không có rủi ro tràn ngay cả khi kích thước kiểu dữ liệu đạt tới`10^9`. 

Lời giải thoát ra ngay sau khi tìm được một cặp hợp lệ vì bài toán chỉ hỏi liệu cặp đó có tồn tại hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
64 16 32
```Sau khi sắp xếp:```
[16, 32, 64]
```| tôi | một [tôi] | a[i+1] | Riêng biệt? | Kiểm tra`a[i+1] < 2*a[i]`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 16 | 32 | Có | 32 < 32 → Không | Tiếp tục | 
| 1 | 32 | 64 | Có | 64 < 64 → Không | Tiếp tục | 

Không có cặp hợp lệ nào tồn tại nên câu trả lời là:```
NO
```Dấu vết này cho thấy trường hợp ranh giới trong đó kiểu dữ liệu lớn hơn chính xác gấp đôi kiểu dữ liệu nhỏ hơn. Bình đẳng thôi chưa đủ vì bình phương vẫn vừa. 

### Ví dụ 2 

đầu vào:```
2
3 4
```Sau khi sắp xếp:```
[3, 4]
```| tôi | một [tôi] | a[i+1] | Riêng biệt? | Kiểm tra`a[i+1] < 2*a[i]`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 4 | Có | 4 < 6 → Có | In CÓ | 

Thuật toán dừng ngay lập tức và xuất ra:```
YES
```Thực vậy,`x = 7`phù hợp với 3 bit, trong khi`49`không phù hợp với 4 bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) ngoài việc sắp xếp | Chỉ có một vài biến được sử dụng | 

Với`n = 10^5`, MỘT`O(n log n)`giải pháp dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tối thiểu vì thuật toán hoạt động trực tiếp trên mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    for i in range(n - 1):
        if a[i] < a[i + 1] and a[i + 1] < 2 * a[i]:
            print("YES")
            return

    print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided samples
assert run("3\n64 16 32\n") == "NO\n", "sample 1"

# sample 2 from statement notes
assert run("2\n3 4\n") == "YES\n", "sample 2"

# minimum size, no valid pair
assert run("2\n1 2\n") == "NO\n", "minimum case"

# all equal values
assert run("4\n5 5 5 5\n") == "NO\n", "equal sizes invalid"

# valid pair appears after sorting
assert run("5\n20 3 100 5 50\n") == "YES\n", "sorting correctness"

# boundary where larger is exactly double
assert run("3\n7 14 28\n") == "NO\n", "strict inequality required"

# large values
assert run("2\n1000000000 1999999999\n") == "YES\n", "large integers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`NO`| Nhân đôi chính xác là không đủ | 
|`4 / 5 5 5 5`|`NO`| Kích thước kiểu dữ liệu bằng nhau không hợp lệ | 
|`5 / 20 3 100 5 50`|`YES`| Phân loại và kiểm tra liền kề | 
|`3 / 7 14 28`|`NO`| Ranh giới bất bình đẳng nghiêm ngặt | 
|`2 / 1000000000 1999999999`|`YES`| Không có vấn đề tràn | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các kích thước kiểu dữ liệu đều bằng nhau. 

đầu vào:```
4
5 5 5 5
```Sau khi sắp xếp, mảng vẫn giữ nguyên. Mỗi cặp liền kề đều thất bại trong việc so sánh nghiêm ngặt:```
5 < 5
```Vì vậy thuật toán không bao giờ đạt điều kiện thành công và in ra`"NO"`. 

Điều này xử lý yêu cầu rằng kiểu dữ liệu lớn hơn phải thực sự lớn hơn. 

Bây giờ hãy xem xét ranh giới nhân đôi chính xác. 

đầu vào:```
2
16 32
```Thuật toán kiểm tra:```
32 < 2 * 16
32 < 32
```đó là sai. 

Đầu ra là`"NO"`bởi vì kiểu dữ liệu 32 bit đủ lớn để chứa các bình phương của tất cả các số 16 bit. 

Cuối cùng, hãy xem xét trường hợp cặp hợp lệ không liền kề trong đầu vào ban đầu. 

đầu vào:```
5
100 3 8 20 5
```Sau khi sắp xếp:```
[3, 5, 8, 20, 100]
```Thuật toán kiểm tra các cặp liền kề:```
5 < 6
```đó là sự thật, vì vậy nó ngay lập tức in`"YES"`. 

Điều này chứng tỏ tại sao việc phân loại là cần thiết. Thứ tự ban đầu ẩn mối quan hệ, nhưng thứ tự được sắp xếp sẽ hiển thị kiểu dữ liệu lớn hơn nhỏ nhất cho mỗi giá trị.
