---
title: "CF 1525B - Sắp xếp hoán vị"
description: "Chúng ta được hoán vị các số từ 1 đến n. Trong một thao tác, chúng ta có thể chọn bất kỳ phân đoạn liền kề nào của mảng và sắp xếp lại các phần tử bên trong phân đoạn đó theo ý muốn. Hạn chế duy nhất là phân đoạn được chọn không thể là toàn bộ mảng."
date: "2026-06-10T17:24:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1525
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 109 (Rated for Div. 2)"
rating: 900
weight: 1525
solve_time_s: 136
verified: true
draft: false
---

[CF 1525B - Sắp xếp hoán vị](https://codeforces.com/problemset/problem/1525/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ`1`ĐẾN`n`. Trong một thao tác, chúng ta có thể chọn bất kỳ phân đoạn liền kề nào của mảng và sắp xếp lại các phần tử bên trong phân đoạn đó theo ý muốn. Hạn chế duy nhất là phân đoạn được chọn không thể là toàn bộ mảng. 

Mục tiêu là xác định số lượng tối thiểu các phép toán cần thiết để chuyển đổi hoán vị thành thứ tự được sắp xếp`[1, 2, ..., n]`. 

Chi tiết quan trọng là việc sắp xếp lại một phân khúc đã chọn có tác dụng cực kỳ mạnh mẽ. Chúng tôi không bị giới hạn trong việc hoán đổi hoặc đảo ngược. Khi một phân đoạn được chọn, nội dung của nó có thể được sắp xếp theo bất kỳ thứ tự nào. Hạn chế duy nhất đến từ thực tế là phân khúc không thể bao gồm tất cả`n`các vị trí. 

Những hạn chế là rất nhỏ. Độ dài mảng không bao giờ vượt quá 50 và có tối đa 2000 trường hợp thử nghiệm. Ngay cả những tính toán đắt tiền cũng sẽ phù hợp một cách thoải mái. Tuy nhiên, vấn đề này được thiết kế xoay quanh việc khám phá một quan sát cấu trúc đơn giản hơn là mô phỏng các hoạt động. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một hoán vị đã được sắp xếp:```
1 2 3 4
```Câu trả lời là`0`, không`1`. Vì không cần thực hiện công việc nào nên việc áp dụng bất kỳ thao tác nào sẽ chỉ làm cho mảng ít được sắp xếp hơn. 

Coi như:```
2 1 3 4
```Câu trả lời là`1`. Một giải pháp bất cẩn có thể cho rằng vì phần tử đầu tiên sai nên phải thực hiện nhiều thao tác. Trong thực tế, chúng ta có thể chọn vị trí`[1,2]`và sắp xếp lại chúng. 

Một trường hợp tế nhị hơn là:```
4 2 3 1
```Ở đây phần tử đầu tiên không`1`và phần tử cuối cùng thì không`n`. Câu trả lời là`3`. Nhiều giải pháp trả về sai`2`. Lý do là cả hai đầu đều bị đặt sai vị trí và các giá trị thuộc về hai đầu cũng bị mắc kẹt ở những vị trí không đúng. Đây hóa ra là tình huống đặc biệt cần tới ba thao tác. 

## Phương pháp tiếp cận 

Quan điểm mạnh mẽ là suy nghĩ về tất cả các phân khúc có thể có và tất cả các khả năng sắp xếp lại các phân khúc đó. Vì một đoạn có thể được sắp xếp lại một cách tùy ý nên hệ số phân nhánh là rất lớn. Ngay cả đối với`n = 50`, việc liệt kê các trạng thái có thể là hoàn toàn không khả thi. 

Phần thú vị của vấn đề là chúng ta không được yêu cầu xây dựng các phép toán. Chúng tôi chỉ cần số lượng tối thiểu. Điều đó gợi ý rằng hãy tìm kiếm đặc điểm của câu trả lời hơn là tìm kiếm nó. 

Quan sát quan trọng là câu trả lời chỉ có thể là`0`,`1`,`2`, hoặc`3`. 

Nếu hoán vị đã được sắp xếp thì câu trả lời rõ ràng là`0`. 

Giả sử hoán vị không được sắp xếp. Nếu vị trí đầu tiên đã chứa`1`hoặc vị trí cuối cùng đã chứa`n`, thì một thao tác là đủ. Chúng ta có thể chọn phần còn lại của mảng và sắp xếp lại theo đúng thứ tự. 

Ví dụ:```
1 4 3 2
```Vị trí đầu tiên đã chính xác. Chúng ta có thể chọn vị trí`[2,4]`và sắp xếp chúng. 

Bây giờ hãy xem xét trường hợp không có đầu nào đúng. 

Nếu giá trị`1`hiện đang ở vị trí cuối cùng và giá trị`n`hiện đang ở vị trí đầu tiên thì câu trả lời là`3`. Đây là sự sắp xếp tồi tệ nhất có thể có của các điểm cuối. Không một thao tác đơn lẻ nào có thể chạm đồng thời vào cả hai đầu vì toàn bộ mảng bị cấm và việc sửa một bên không tự động sửa được bên kia. 

Trong mọi trường hợp còn lại, câu trả lời là`2`. Cả hai điểm cuối đều không chính xác nhưng hoán vị không nằm trong cấu hình đặc biệt cho trường hợp xấu nhất. 

Điều này hoàn toàn xác định câu trả lời từ một vài lần kiểm tra điểm cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị. 
2. Kiểm tra xem hoán vị đã được sắp xếp chưa. 

Nếu mọi vị trí`i`chứa`i + 1`, không cần thực hiện thao tác nào, do đó đầu ra`0`. 
3. Kiểm tra xem phần tử đầu tiên có`1`hoặc phần tử cuối cùng là`n`. 

Nếu một trong hai điều kiện được giữ, xuất ra`1`. 

Lý do là phần còn lại của mảng tạo thành một mảng con thích hợp, có thể được sắp xếp lại một cách tự do để hoàn thành việc sắp xếp. 
4. Kiểm tra xem phần tử đầu tiên có`n`và phần tử cuối cùng là`1`. 

Nếu cả hai điều kiện đều giữ nguyên thì xuất ra`3`. 

Đây là cấu hình duy nhất yêu cầu số lượng hoạt động tối đa. 
5. Nếu không thì xuất ra`2`. 

Cả hai điểm cuối đều không chính xác, nhưng cách sắp xếp không phải là điều tồi tệ nhất có thể, vì vậy hai thao tác luôn đủ. 

### Tại sao nó hoạt động 

Toàn bộ vấn đề được kiểm soát bởi các điểm cuối. 

Nếu mảng đã được sắp xếp thì câu trả lời là`0`. 

Nếu một điểm cuối đã chính xác thì các vị trí còn lại sẽ tạo thành một mảng con hợp lệ. Vì một mảng con đã chọn có thể được sắp xếp lại tùy ý nên chỉ cần thực hiện một thao tác. 

Khi cả hai điểm cuối đều không chính xác, một thao tác đơn lẻ không thể sắp xếp hoán vị. Bất kỳ mảng con thích hợp nào đều thiếu ít nhất một điểm cuối, do đó sẽ vẫn còn ít nhất một điểm cuối không chính xác. 

Trong số những trường hợp đó, việc thỏa thuận với`n`lúc đầu và`1`cuối cùng là khó nhất. Việc sửa cả hai giá trị cực trị bị đặt sai vị trí không thể được thực hiện chỉ bằng hai thao tác vì có thể không bao giờ toàn bộ mảng được chọn. Phân tích chính thức cho thấy cấu hình này yêu cầu chính xác ba thao tác. 

Mọi hoán vị chưa sắp xếp khác đều thuộc loại còn lại và có thể được sửa bằng hai thao tác. 

Vì các trường hợp loại trừ lẫn nhau và bao gồm tất cả các hoán vị nên thuật toán luôn trả về số phép toán tối thiểu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if a == list(range(1, n + 1)):
            print(0)
        elif a[0] == 1 or a[-1] == n:
            print(1)
        elif a[0] == n and a[-1] == 1:
            print(3)
        else:
            print(2)

solve()
```Điều kiện đầu tiên kiểm tra xem hoán vị đã được sắp xếp chưa. Điều này phải được xử lý trước bất kỳ logic điểm cuối nào, nếu không thì hoán vị được sắp xếp như`[1,2,3]`sẽ khớp không chính xác với trường hợp thứ hai và trả về`1`. 

Điều kiện thứ hai phát hiện các tình huống trong đó một thao tác là đủ. Chúng tôi chỉ cần một trong các điểm cuối là chính xác. 

Điều kiện thứ ba xác định cách sắp xếp trường hợp xấu nhất duy nhất. Nó phải được kiểm tra sau các điều kiện trước đó vì một hoán vị với`a[0] = n`Và`a[n-1] = 1`dù sao đi nữa cũng không bao giờ có thể thỏa mãn điều kiện một thao tác. 

Mọi hoán vị còn lại đều thuộc loại hai phép toán. 

Không có xử lý đặc biệt cho nhỏ`n`được yêu cầu. Logic hoạt động thống nhất cho tất cả các đầu vào hợp lệ. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
1 3 2 4
```| Kiểm tra | Kết quả | 
| --- | --- | 
| Sắp xếp? | Không | 
| Phần tử đầu tiên = 1? | Có | 
| Trả lời | 1 | 

Vị trí đầu tiên đã chứa giá trị cuối cùng của nó. Chúng ta có thể sắp xếp hậu tố`[3,2,4]`trong một hoạt động duy nhất. 

### Đầu vào mẫu 2```
5
2 1 4 5 3
```| Kiểm tra | Kết quả | 
| --- | --- | 
| Sắp xếp? | Không | 
| Phần tử đầu tiên = 1? | Không | 
| Phần tử cuối cùng = n? | Không | 
| Phần tử đầu tiên = n? | Không | 
| Phần tử cuối cùng = 1? | Không | 
| Trả lời | 2 | 

Cả hai điểm cuối đều không chính xác nên không thể thực hiện được một thao tác nào. Hoán vị không ở cấu hình trường hợp xấu nhất, vì vậy hai thao tác là đủ. 

### Ví dụ về trường hợp xấu nhất```
5
5 2 3 4 1
```| Kiểm tra | Kết quả | 
| --- | --- | 
| Sắp xếp? | Không | 
| Phần tử đầu tiên = 1? | Không | 
| Phần tử cuối cùng = n? | Không | 
| Phần tử đầu tiên = n? | Có | 
| Phần tử cuối cùng = 1? | Có | 
| Trả lời | 3 | 

Đây chính xác là cấu hình đặc biệt yêu cầu ba thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một so sánh với hoán vị được sắp xếp cộng với một vài kiểm tra theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Với`n ≤ 50`, giải pháp này thấp hơn nhiều so với giới hạn. Ngay cả với 2000 trường hợp thử nghiệm, tổng khối lượng công việc vẫn rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if a == list(range(1, n + 1)):
            out.append("0")
        elif a[0] == 1 or a[-1] == n:
            out.append("1")
        elif a[0] == n and a[-1] == 1:
            out.append("3")
        else:
            out.append("2")

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""3
4
1 3 2 4
3
1 2 3
5
2 1 4 5 3
"""
) == "1\n0\n2\n", "sample"

# already sorted
assert run(
"""1
3
1 2 3
"""
) == "0\n"

# first element correct
assert run(
"""1
5
1 5 4 3 2
"""
) == "1\n"

# last element correct
assert run(
"""1
5
2 5 4 1 5
"""
) != "1\n"  # not a valid permutation, avoid using

assert run(
"""1
5
2 1 3 4 5
"""
) == "1\n"

# worst case
assert run(
"""1
5
5 2 3 4 1
"""
) == "3\n"

# generic two-operation case
assert run(
"""1
5
2 1 4 5 3
"""
) == "2\n"
```### Tóm tắt thử nghiệm tùy chỉnh 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3`|`0`| Đã sắp xếp hoán vị | 
|`1 5 4 3 2`|`1`| Điểm cuối đầu tiên đã đúng | 
|`2 1 3 4 5`|`1`| Điểm cuối cuối cùng đã chính xác | 
|`5 2 3 4 1`|`3`| Sắp xếp trường hợp xấu nhất | 
|`2 1 4 5 3`|`2`| Trường hợp hai thao tác chung | 

## Vỏ cạnh 

Hãy xem xét hoán vị đã được sắp xếp:```
1
3
1 2 3
```Thuật toán trước tiên kiểm tra xem toàn bộ mảng có bằng không`[1,2,3]`. Đúng vậy, vì vậy câu trả lời là`0`. Điều này ngăn mảng khớp không chính xác với trường hợp một thao tác. 

Coi như:```
1
4
1 4 3 2
```Mảng chưa được sắp xếp nhưng phần tử đầu tiên đã được sắp xếp`1`. Thuật toán trả về`1`. Chúng ta có thể sắp xếp mảng con`[4,3,2]`trực tiếp vì nó là một mảng con thích hợp. 

Coi như:```
1
4
4 2 3 1
```Mảng không được sắp xếp. Không có điểm cuối nào là chính xác. Phần tử đầu tiên bằng`n`và phần tử cuối cùng bằng`1`, do đó thuật toán trả về`3`. Đây chính xác là cấu hình trường hợp xấu nhất đặc biệt được xác định bằng bằng chứng. 

Coi như:```
1
5
2 3 1 5 4
```Mảng không được sắp xếp. Cả hai điểm cuối đều không chính xác nhưng đó không phải là sự sắp xếp trong trường hợp xấu nhất. Thuật toán đi đến nhánh cuối cùng và trả về`2`, là số lượng hoạt động tối thiểu cho danh mục này.
