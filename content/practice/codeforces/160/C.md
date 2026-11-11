---
title: "CF 160C - Tìm Cặp"
description: "Chúng ta được cho một mảng gồm n số nguyên. Từ mảng này, chúng ta tạo thành mọi cặp có thứ tự (a[i], a[j]), bao gồm các cặp trong đó i = j. Vì cả hai vị trí được chọn độc lập nên có chính xác n2 cặp. Tất cả các cặp này được sắp xếp theo từ điển."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 160
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 111 (Div. 2)"
rating: 1700
weight: 160
solve_time_s: 107
verified: true
draft: false
---

[CF 160C - Tìm cặp](https://codeforces.com/problemset/problem/160/C) 

**Đánh giá:** 1700 
**Tags:** triển khai, toán học, sắp xếp 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt`n`số nguyên. Từ mảng này, chúng tôi tạo thành mọi cặp có thứ tự`(a[i], a[j])`, bao gồm các cặp ở đó`i = j`. Vì cả hai vị trí được chọn độc lập nên có chính xác`n²`cặp. 

Tất cả các cặp này được sắp xếp theo từ điển. Điều đó có nghĩa là các cặp được sắp xếp đầu tiên theo giá trị đầu tiên và nếu chúng bằng nhau thì theo giá trị thứ hai. 

Nhiệm vụ là xuất ra`k`-cặp thứ theo thứ tự được sắp xếp này. 

Các ràng buộc loại trừ hoàn toàn việc tạo trực tiếp tất cả các cặp. Khi`n = 10^5`, tổng số cặp trở thành`10^10`, vượt xa những gì có thể chứa trong bộ nhớ hoặc được xử lý kịp thời. Ngay cả việc lặp lại tất cả các cặp một lần cũng không thể thực hiện được dưới giới hạn 1 giây. 

Điều này buộc chúng ta phải khai thác cấu trúc thứ tự từ điển thay vì xây dựng danh sách một cách rõ ràng. 

Một điểm tinh tế là mảng có thể chứa các giá trị trùng lặp. Thứ tự dựa trên các giá trị, không phải chỉ số. Nếu mảng là`[1, 1, 2]`, sau đó cặp bắt đầu bằng`1`xuất hiện nhiều lần vì có hai lần xuất hiện`1`. 

Ví dụ:```
Input:
3 4
1 1 2
```Tất cả các cặp sau khi sắp xếp là:```
(1,1)
(1,1)
(1,2)
(1,1)
(1,1)
(1,2)
(2,1)
(2,1)
(2,2)
```Cặp thứ 4 là`(1,1)`. 

Việc triển khai bất cẩn nén các bản sao thành các giá trị duy nhất sẽ tạo ra bội số sai. 

Một lỗi dễ mắc phải khác là trộn lẫn việc lập chỉ mục dựa trên 0 và dựa trên 1 cho`k`. 

Coi như:```
Input:
2 1
5 7
```Các cặp được sắp xếp:```
(5,5)
(5,7)
(7,5)
(7,7)
```Cặp đầu tiên là`(5,5)`. 

Nếu chúng ta trừ đi một từ`k`quá muộn hoặc quá sớm, chúng ta có thể vô tình quay trở lại`(5,7)`thay vì. 

Giá trị lớn cũng quan trọng vì`k`có thể lớn như`n² = 10^10`. Số nguyên 32 bit là không đủ trong một số ngôn ngữ. Python xử lý việc này một cách tự nhiên, nhưng trong C++ chúng ta cần`long long`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo tất cả`n²`các cặp có thứ tự, sắp xếp chúng theo từ điển, sau đó in`k`-cặp thứ 

Điều này có hiệu quả vì việc sắp xếp từ điển khớp trực tiếp với báo cáo vấn đề. Vấn đề là quy mô. Với`n = 10^5`, số cặp trở thành`10^10`. Ngay cả việc lưu trữ các cặp cũng đòi hỏi bộ nhớ rất lớn và việc sắp xếp chúng là hoàn toàn không khả thi. 

Quan sát quan trọng là thứ tự từ điển có cấu trúc rất đều đặn sau khi sắp xếp mảng ban đầu. 

Giả sử mảng được sắp xếp là:```
b[0], b[1], ..., b[n-1]
```Sau đó, mỗi cặp bắt đầu bằng`b[0]`đến trước, tiếp theo là mỗi cặp bắt đầu bằng`b[1]`, vân vân. 

Mỗi yếu tố đóng góp chính xác`n`các cặp liên tiếp làm thành phần đầu tiên. 

Ví dụ: nếu:```
b = [1, 2, 5]
```sau đó các cặp được sắp xếp sẽ được nhóm lại như thế này:```
First element = 1:
(1,1), (1,2), (1,5)

First element = 2:
(2,1), (2,2), (2,5)

First element = 5:
(5,1), (5,2), (5,5)
```Điều này có nghĩa là phần tử đầu tiên của câu trả lời có thể được tìm thấy bằng cách xác định khối có kích thước`n`chứa`k`-cặp thứ 

Khi giá trị đầu tiên được cố định, giá trị thứ hai được xác định bởi vị trí bên trong khối đó. 

Việc trùng lặp sẽ làm phức tạp mọi thứ một chút vì một số chỉ mục liên tiếp có thể chứa cùng một giá trị. Nếu một giá trị xuất hiện`cnt`lần thì tất cả các cặp bắt đầu bằng giá trị đó sẽ chiếm`cnt * n`các vị trí liên tiếp. 

Thành phần thứ hai cũng được chọn từ mảng đã sắp xếp. Sau khi xác định được giá trị đầu tiên, chúng ta chỉ cần xác định phần tử nào của mảng đã sắp xếp tương ứng với offset còn lại. 

Điều này làm giảm vấn đề về số học trên các vị trí được sắp xếp thay vì tạo cặp rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2logn2) | O(n²) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và sắp xếp nó. 

Thứ tự từ điển chỉ phụ thuộc vào các giá trị, do đó việc sắp xếp mảng sẽ tiết lộ cấu trúc chính xác của thứ tự cặp. 
2. Chuyển đổi`k`sang lập chỉ mục dựa trên 0 bằng cách trừ 1. 

Điều này làm cho việc tính toán khối dễ dàng hơn vì phép chia số nguyên hoạt động tự nhiên với độ lệch dựa trên 0. 
3. Tính chỉ số của phần tử đầu tiên là`first_idx = k // n`. 

Mọi phần tử trong mảng được sắp xếp đều đóng góp chính xác`n`cặp làm thành phần đầu tiên. Chia cho`n`cho chúng ta biết khối nào chứa câu trả lời. 
4. Hãy để`first_value = b[first_idx]`. 

Đây là số đầu tiên của cặp câu trả lời. 
5. Đếm xem bao nhiêu lần`first_value`xuất hiện trong mảng đã sắp xếp. 

Giả sử nó xuất hiện`cnt`lần và lần xuất hiện đầu tiên của nó là ở chỉ mục`l`. 

Sau đó tất cả các cặp bắt đầu bằng`first_value`chiếm một đoạn kích thước lớn liền kề`cnt * n`. 
6. Tính toán xem cặp mong muốn nằm trong đoạn này bao xa. 

Số cặp trước đoạn này là`l * n`. 

Vì thế:```
offset = k - l * n
```7. Yếu tố thứ hai được xác định bởi:```
second_idx = offset // cnt
```Mỗi giá trị thứ hai riêng biệt lặp lại`cnt`nhiều lần vì có`cnt`lựa chọn giống hệt nhau cho thành phần đầu tiên. 
8. Đầu ra:```
(first_value, b[second_idx])
```### Tại sao nó hoạt động 

Sau khi sắp xếp mảng, các nhóm thứ tự từ điển sẽ ghép cặp theo thành phần đầu tiên của chúng. Mỗi lần xuất hiện của một giá trị đều đóng góp một khối đầy đủ`n`cặp. 

Nếu một giá trị xuất hiện nhiều lần, các khối đó sẽ liền kề nhau vì các thành phần đầu tiên bằng nhau. Bên trong phân đoạn kết hợp này, các phần tử thứ hai xuất hiện theo thứ tự được sắp xếp và mỗi phần tử lặp lại chính xác`cnt`lần, một lần cho mỗi lần xuất hiện của giá trị đầu tiên. 

Thuật toán xác định chính xác phân đoạn nào chứa`k`- cặp thứ , sau đó xác định giá trị thứ hai lặp lại nào tương ứng với phần bù còn lại. Vì cấu trúc khớp chính xác với thứ tự từ điển nên cặp được tạo ra phải chính xác. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

a.sort()

k -= 1

first_idx = k // n
first_value = a[first_idx]

left = bisect_left(a, first_value)
right = bisect_right(a, first_value)

cnt = right - left

offset = k - left * n
second_idx = offset // cnt

print(first_value, a[second_idx])
```Giải pháp bắt đầu bằng cách sắp xếp mảng vì thứ tự cặp từ điển trực tiếp tuân theo thứ tự giá trị được sắp xếp. 

Sau khi chuyển đổi`k`vào chỉ mục dựa trên 0, biểu thức`k // n`xác định câu trả lời thuộc về nhóm cặp nào. Mọi chỉ số đều đóng góp chính xác`n`ghép làm thành phần đầu tiên, do đó phép chia số nguyên sẽ tách biệt khối chính xác ngay lập tức. 

Bước tiếp theo tìm toàn bộ các giá trị bằng nhau bằng cách sử dụng`bisect_left`Và`bisect_right`. Điều này là cần thiết vì các giá trị đầu tiên trùng lặp sẽ hợp nhất thành một phân đoạn lớn hơn theo thứ tự từ điển. 

Biểu thức:```
offset = k - left * n
```loại bỏ tất cả các cặp thuộc giá trị đầu tiên nhỏ hơn. Phần còn lại là vị trí bên trong đoạn mà thành phần đầu tiên bằng`first_value`. 

Bên trong phân đoạn này, mỗi giá trị thứ hai xuất hiện chính xác`cnt`lần liên tiếp. Chia cho`cnt`đưa ra chỉ số phần tử thứ hai chính xác. 

Một điểm tinh tế là`second_idx`đề cập trực tiếp đến mảng được sắp xếp. Chúng tôi không nén các bản sao vì các giá trị lặp lại phải được lặp lại theo thứ tự. 

Một chi tiết tinh tế khác là sử dụng phép chia số nguyên ở mọi nơi. Vì thứ tự tạo thành các khối hoàn toàn đều đặn nên việc phân chia tầng sẽ ánh xạ các vị trí thành các giá trị mà không cần bất kỳ tìm kiếm hoặc mô phỏng nhị phân nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
2 1
```Mảng được sắp xếp:```
[1, 2]
```Sau khi chuyển đổi sang lập chỉ mục dựa trên 0:```
k = 3
```| Bước | Giá trị | 
| --- | --- | 
|`first_idx = k // n`|`3 // 2 = 1`| 
|`first_value`|`2`| 
|`left`|`1`| 
|`right`|`2`| 
|`cnt`|`1`| 
|`offset = k - left*n`|`3 - 2 = 1`| 
|`second_idx = offset // cnt`|`1 // 1 = 1`| 
| Trả lời |`(2, 2)`| 

Ví dụ này cho thấy cấu trúc khối đơn giản nhất có thể. Mỗi giá trị xuất hiện một lần, do đó mỗi thành phần đầu tiên sở hữu chính xác một khối kích thước`n`. 

### Ví dụ 2 

đầu vào:```
3 2
3 1 5
```Mảng được sắp xếp:```
[1, 3, 5]
```Sau khi chuyển đổi sang lập chỉ mục dựa trên 0:```
k = 1
```| Bước | Giá trị | 
| --- | --- | 
|`first_idx = k // n`|`1 // 3 = 0`| 
|`first_value`|`1`| 
|`left`|`0`| 
|`right`|`1`| 
|`cnt`|`1`| 
|`offset = k - left*n`|`1 - 0 = 1`| 
|`second_idx = offset // cnt`|`1 // 1 = 1`| 
| Trả lời |`(1, 3)`| 

Dấu vết này cho thấy thành phần thứ hai được chọn bên trong khối tương ứng với giá trị đầu tiên như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị thời gian chạy | 
| Không gian | O(n) | Mảng đã sắp xếp được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc. Sắp xếp`10^5`số nguyên đủ nhanh cho giới hạn 1 giây trong Python và tất cả các phép toán còn lại là thời gian không đổi hoặc logarit do tìm kiếm nhị phân. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left, bisect_right

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    a.sort()

    k -= 1

    first_idx = k // n
    first_value = a[first_idx]

    left = bisect_left(a, first_value)
    right = bisect_right(a, first_value)

    cnt = right - left

    offset = k - left * n
    second_idx = offset // cnt

    print(first_value, a[second_idx])

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

# provided samples
assert run("2 4\n2 1\n") == "2 2", "sample 1"
assert run("3 2\n3 1 5\n") == "1 3", "sample 2"

# minimum size
assert run("1 1\n42\n") == "42 42", "single element"

# all equal values
assert run("3 5\n7 7 7\n") == "7 7", "all equal"

# duplicate handling
assert run("3 4\n1 1 2\n") == "1 1", "duplicate multiplicity"

# last possible pair
assert run("2 4\n1 5\n") == "5 5", "largest k"

print("All tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 42`|`42 42`| Mảng kích thước tối thiểu | 
|`3 5 / 7 7 7`|`7 7`| Tất cả các giá trị giống nhau | 
|`3 4 / 1 1 2`|`1 1`| Các bội số trùng lặp được xử lý chính xác | 
|`2 4 / 1 5`|`5 5`| Hợp lệ lớn nhất`k`| 

## Vỏ cạnh 

Hãy xem xét trường hợp nặng trùng lặp:```
Input:
3 4
1 1 2
```Mảng được sắp xếp:```
[1, 1, 2]
```Tất cả các cặp theo thứ tự là:```
(1,1)
(1,1)
(1,2)
(1,1)
(1,1)
(1,2)
(2,1)
(2,1)
(2,2)
```Câu trả lời đúng là cặp thứ 4,`(1,1)`. 

Thuật toán tính toán:```
k = 3
first_idx = 1
first_value = 1
cnt = 2
offset = 3
second_idx = 1
```mang lại`(1,1)`. 

Điều này hoạt động vì thuật toán bảo toàn bội số thay vì nén các giá trị bằng nhau. 

Bây giờ hãy xem xét đầu vào nhỏ nhất có thể:```
Input:
1 1
42
```Chỉ có một cặp:```
(42,42)
```Thuật toán tính toán:```
k = 0
first_idx = 0
second_idx = 0
```và trả về`(42,42)`một cách chính xác. 

Cuối cùng, hãy xem xét cặp từ điển lớn nhất:```
Input:
2 4
1 5
```Các cặp được sắp xếp là:```
(1,1)
(1,5)
(5,1)
(5,5)
```Cặp thứ 4 là`(5,5)`. 

Thuật toán tìm thấy:```
k = 3
first_idx = 1
first_value = 5
offset = 1
second_idx = 1
```trỏ chính xác đến cặp cuối cùng trong thứ tự.
