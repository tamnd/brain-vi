---
title: "CF 1877A - Mục Tiêu Chiến Thắng"
description: "Chúng tôi có một giải đấu bóng đá vòng tròn với các đội $n$. Mỗi cặp đội thi đấu đúng một trận. Đối với mỗi đội, hiệu quả của nó được xác định là: $$text{số bàn thắng được ghi} - text{số bàn thua}$$ Sau khi giải đấu kết thúc, hiệu quả của tất cả các đội sẽ được tính toán."
date: "2026-06-08T22:55:11+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1877
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 902 (Div. 2, based on COMPFEST 15 - Final Round)"
rating: 800
weight: 1877
solve_time_s: 92
verified: true
draft: false
---

[CF 1877A - Mục tiêu chiến thắng](https://codeforces.com/problemset/problem/1877/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một giải đấu bóng đá vòng tròn với$n$các đội. Mỗi cặp đội thi đấu đúng một trận. 

Đối với mỗi đội, hiệu quả của nó được xác định như sau:$$\text{goals scored} - \text{goals conceded}$$Sau khi giải đấu kết thúc, hiệu quả của tất cả các đội sẽ được tính toán. Hiệu quả của một đội bị mất đi và chúng ta được hưởng hiệu quả của những đội còn lại$n-1$các đội. Nhiệm vụ là xác định hiệu quả còn thiếu. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 500 trường hợp thử nghiệm và tối đa 100 nhóm cho mỗi trường hợp thử nghiệm. Thậm chí một$O(n^2)$giải pháp sẽ là tầm thường ở đây. Thách thức thực sự là nhận ra thuộc tính toán học xác định duy nhất giá trị còn thiếu. 

Một sai lầm phổ biến là nghĩ rằng chúng ta cần phải xây dựng lại kết quả trận đấu. Tuyên bố thậm chí còn cung cấp một giải đấu khả thi, có thể lôi kéo chúng ta vào các trận đấu mô hình hóa. Điều đó là không cần thiết vì hiệu suất còn thiếu có thể được suy ra trực tiếp từ bất biến toàn cục. 

Hãy xem xét một ví dụ đơn giản:```
n = 2
known efficiencies = [5]
```Chỉ có một trận đấu. Nếu một đội có hiệu quả$+5$, cái kia phải có hiệu quả$-5$. Câu trả lời là:```
-5
```Một trường hợp dễ bỏ sót khác là khi hiệu quả âm:```
n = 3
known efficiencies = [-3, -7]
```Câu trả lời đúng là:```
10
```Việc thực hiện bất cẩn lấy giá trị tuyệt đối hoặc giả định hiệu suất không âm sẽ thất bại. 

Trường hợp cạnh thứ ba là khi tất cả hiệu suất đã biết đều bằng 0:```
n = 4
known efficiencies = [0, 0, 0]
```Hiệu suất còn thiếu cũng là:```
0
```Bất biến được sử dụng bởi giải pháp vẫn giữ nguyên hoàn hảo. 

## Phương pháp tiếp cận 

Tư duy bạo lực bắt đầu bằng việc cố gắng xây dựng lại giải đấu. Vì mọi hiệu quả đều phụ thuộc vào nhiều trận đấu, nên chúng ta có thể cố gắng suy ra các điểm thỏa mãn các giá trị đã cho và sau đó suy ra hiệu quả của đội còn thiếu. 

Cách tiếp cận này nhanh chóng trở nên không thực tế. Một giải đấu vòng tròn có$$\frac{n(n-1)}{2}$$trận đấu và mỗi trận đấu có thể có nhiều cách kết hợp điểm số có thể xảy ra. Việc tìm kiếm thông qua các giải đấu có thể thực hiện là hoàn toàn không cần thiết. 

Quan sát quan trọng xuất phát từ việc xem xét hiệu quả được hình thành như thế nào. 

Đối với bất kỳ trận đấu nào, giả sử Đội A ghi bàn$x$bàn thắng và tỷ số của đội B$y$mục tiêu. 

Đội A đóng góp:$$x-y$$đến hiệu quả của nó. 

Đội B đóng góp:$$y-x$$đến hiệu quả của nó. 

Việc thêm những đóng góp này sẽ mang lại:$$(x-y)+(y-x)=0$$Mỗi trận đấu đều không đóng góp gì vào tổng hiệu quả của tất cả các đội. Vì giải đấu chỉ là tập hợp các trận đấu nên tổng hiệu quả của tất cả các đội cũng phải bằng 0. 

Nếu hiệu suất là:$$e_1,e_2,\dots,e_n$$sau đó$$e_1+e_2+\cdots+e_n=0$$Chúng tôi được trao$n-1$của họ. Nếu tổng của chúng là$S$, thì hiệu suất còn thiếu phải là:$$-S$$Điều này làm giảm toàn bộ vấn đề để tính tổng các số đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Tổng bất biến | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc$n$. 
3. Đọc$n-1$hiệu quả đã biết. 
4. Tính tổng của chúng$S$. 
5. Đầu ra$-S$. 

Lý do bước 5 có hiệu quả là vì tổng hiệu quả của tất cả các đội trong một giải đấu luôn bằng không. Giá trị còn thiếu phải hủy bỏ chính xác tổng các giá trị đã biết. 

### Tại sao nó hoạt động 

Mỗi trận đấu đều đóng góp những giá trị trái ngược nhau cho hai đội tham gia. Nếu một đội thắng$x-y$, những lợi ích khác$y-x$. Tổng đóng góp của trận đấu đó vào tổng hiệu quả toàn cầu là bằng không. 

Vì mỗi trận đấu không đóng góp gì nên tổng hiệu quả của tất cả các đội sau toàn bộ giải đấu cũng bằng 0. Hãy để hiệu quả còn thiếu là$m$. Nếu tổng hiệu suất đã biết là$S$, sau đó:$$S + m = 0$$ngụ ý:$$m = -S$$Thuật toán tính toán chính xác giá trị này nên luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(-sum(a))
```Việc thực hiện tuân theo quan sát toán học trực tiếp. 

Biến`a`lưu trữ$n-1$hiệu quả đã biết. Tổng của chúng được tính bằng công cụ tích hợp sẵn của Python`sum()`chức năng. Vì tổng hiệu suất của tất cả các đội phải bằng 0 nên hiệu suất còn thiếu chỉ đơn giản là số âm của tổng đó. 

Không có điều kiện biên phức tạp. Các giá trị âm, giá trị dương và số 0 đều được xử lý một cách tự nhiên. Tràn số nguyên không phải là vấn đề đáng lo ngại trong Python và ngay cả trong các ngôn ngữ có chiều rộng cố định, các giá trị vẫn rất nhỏ theo các ràng buộc nhất định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [3, -4, 5]
```| Bước | Giá trị | 
| --- | --- | 
| Đọc hiệu quả | [3, -4, 5] | 
| Tính tổng S | 4 | 
| Thiếu hiệu quả | -4 | 

Đầu ra:```
-4
```Hiệu suất đã biết cộng lại bằng 4. Vì tổng số phải bằng 0 nên đội thiếu phải đóng góp -4. 

### Ví dụ 2 

đầu vào:```
n = 11
a = [-30, 12, -57, 7, 0, -81, -68, 41, -89, 0]
```| Bước | Giá trị | 
| --- | --- | 
| Đọc hiệu quả | danh sách nhất định | 
| Tính tổng S | -265 | 
| Thiếu hiệu quả | 265 | 

Đầu ra:```
265
```Ví dụ này chứng minh rằng tổng số âm lớn được xử lý theo cùng một cách. Giá trị còn thiếu chỉ đơn giản là hủy bỏ số tiền tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính tổng$n-1$hiệu quả một lần | 
| Không gian | O(1) | Chỉ có một vài biến ngoài danh sách đầu vào | 

Với$n \le 100$và nhiều nhất là 500 trường hợp thử nghiệm, tổng khối lượng công việc là rất nhỏ. Giải pháp chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans.append(str(-sum(a)))

    sys.stdout.write("\n".join(ans))

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
assert run(
"""2
4
3 -4 5
11
-30 12 -57 7 0 -81 -68 41 -89 0
"""
) == """-4
265"""

# minimum size
assert run(
"""1
2
5
"""
) == """-5"""

# all zeros
assert run(
"""1
4
0 0 0
"""
) == """0"""

# all equal values
assert run(
"""1
5
7 7 7 7
"""
) == """-28"""

# mixed positive and negative
assert run(
"""1
3
-3 -7
"""
) == """10"""

# boundary-style values
assert run(
"""1
4
100 -100 100
"""
) == """-100"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, [5]`|`-5`| Giải đấu hợp lệ tối thiểu | 
|`n=4, [0,0,0]`|`0`| Trường hợp có tổng bằng 0 | 
|`n=5, [7,7,7,7]`|`-28`| Tất cả các giá trị bằng nhau | 
|`n=3, [-3,-7]`|`10`| Hiệu quả tiêu cực | 
|`n=4, [100,-100,100]`|`-100`| Hủy bỏ tích cực và tiêu cực | 

## Vỏ cạnh 

Hãy xem xét giải đấu có quy mô tối thiểu:```
1
2
5
```Thuật toán tính toán:```
S = 5
answer = -5
```Chỉ có một trận đấu nên hiệu quả của hai đội hẳn là trái ngược nhau. Đầu ra là chính xác. 

Hãy xem xét hiệu quả hoàn toàn bằng không:```
1
4
0 0 0
```Thuật toán tính toán:```
S = 0
answer = 0
```Vì tổng hiệu suất phải bằng 0 nên giá trị còn thiếu cũng bằng 0. 

Xem xét các giá trị âm:```
1
3
-3 -7
```Thuật toán tính toán:```
S = -10
answer = 10
```Bất biến không phụ thuộc vào dấu. Hiệu suất còn thiếu chỉ đơn giản là cân bằng tổng số trở về không. 

Hãy xem xét một đầu vào có tính hủy bỏ cao:```
1
4
100 -100 100
```Thuật toán tính toán:```
S = 100
answer = -100
```Mặc dù một số giá trị triệt tiêu lẫn nhau, tổng giá trị còn lại sẽ xác định duy nhất hiệu suất còn thiếu. Kết quả đầu ra đúng vì tổng tổng bằng 0.
