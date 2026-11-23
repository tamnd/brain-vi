---
title: "CF 42A - Có tội --- vào bếp!"
description: "Chúng tôi muốn nấu súp bằng cách sử dụng một số nguyên liệu phải có tỷ lệ cố định. Nếu công thức ghi tỷ lệ là a1, a2, ..., an thì món súp cuối cùng phải chứa:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 42
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 41"
rating: 1400
weight: 42
solve_time_s: 92
verified: true
draft: false
---
[CF 42A - Có tội --- vào bếp!](https://codeforces.com/problemset/problem/42/A) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi muốn nấu súp bằng cách sử dụng một số nguyên liệu phải có tỷ lệ cố định. Nếu công thức cho biết tỷ lệ là`a1, a2, ..., an`, thì món súp cuối cùng phải chứa:`a1 * x, a2 * x, ..., an * x`lít mỗi thành phần cho một số giá trị thực không âm`x`. 

Chúng tôi đã có số lượng hạn chế của từng thành phần, được lưu trữ trong`b1, b2, ..., bn`. Chúng tôi cũng không thể vượt quá dung lượng chảo`V`. Nhiệm vụ là xác định tổng khối lượng súp tối đa mà chúng ta có thể nấu được. 

Chi tiết quan trọng là tỷ lệ công thức rất cứng nhắc. Chúng tôi không được phép độc lập lựa chọn số lượng thành phần. Một khi chúng ta chọn`x`, mọi lượng thành phần sẽ được cố định tự động. 

Những hạn chế là rất nhỏ. Có tối đa 20 thành phần và tất cả các giá trị đều là số nguyên nhỏ. Điều này có nghĩa là hầu hết mọi phép tính đơn giản sẽ dễ dàng phù hợp trong thời gian giới hạn. Thách thức không phải là hiệu suất, mà là nhận ra mối quan hệ toán học giữa tỷ lệ, giới hạn thành phần và tổng khối lượng. 

Một sai lầm phổ biến là suy nghĩ tham lam từng thành phần. Ví dụ:```
2 100
1 2
10 10
```Một giải pháp bất cẩn có thể nói rằng chúng ta có sẵn tổng cộng 20 lít nguyên liệu, vì vậy chúng ta có thể nấu được 20 lít súp. Điều đó là sai. 

Tỷ lệ yêu cầu lượng thành phần`(x, 2x)`. Vì nguyên liệu thứ hai chỉ có 10 lít nên ta phải có:`2x <= 10`Vì thế`x <= 5`. 

Tổng khối lượng súp trở thành:`x + 2x = 15`Câu trả lời đúng là`15`. 

Một lỗi dễ xảy ra khác là quên dung tích chảo. Coi như:```
1 5
1
100
```Chúng tôi có đủ nguyên liệu cho 100 lít, nhưng chảo chỉ chứa được 5 lít. Câu trả lời là`5`. 

Xử lý dấu phẩy động cũng có vấn đề. Hệ số tỷ lệ`x`thường là phân số. Ví dụ:```
2 100
3 2
10 10
```Thành phần đầu tiên cho phép`x <= 10 / 3`, trong khi thứ hai cho phép`x <= 5`. 

Yếu tố hạn chế là`10 / 3`, do đó thể tích súp trở thành:`(3 + 2) * (10 / 3) = 50 / 3 = 16.666666...`Việc sử dụng phép chia số nguyên sẽ cắt ngắn kết quả một cách không chính xác. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ thử nhiều giá trị ứng cử viên cho`x`. Vì lượng súp thay đổi liên tục nên chúng ta có thể tưởng tượng việc tìm kiếm nhị phân khả thi lớn nhất`x`. Đối với mỗi ứng viên, chúng tôi sẽ xác minh xem mọi yêu cầu về thành phần có phù hợp với nguồn cung sẵn có hay không và liệu tổng khối lượng có vừa với chảo hay không. 

Điều này hiệu quả vì tính khả thi là đơn điệu. Nếu một số giá trị của`x`là có thể thì mọi giá trị nhỏ hơn cũng có thể. 

Vấn đề là tìm kiếm nhị phân là không cần thiết ở đây. Cấu trúc của các ràng buộc đã trực tiếp đưa ra câu trả lời chính xác. 

Đối với mỗi thành phần`i`, công thức yêu cầu`ai * x`lít, nhưng chúng tôi chỉ sở hữu`bi`lít. Điều đó mang lại:```
ai * x <= bi
x <= bi / ai
```Mỗi thành phần đặt một giới hạn trên`x`. Hệ số tỷ lệ hợp lệ lớn nhất chỉ đơn giản là mức tối thiểu của tất cả các giới hạn này:```
x = min(bi / ai)
```Một lần`x`được cố định, tổng khối lượng súp trở thành:```
(a1 + a2 + ... + an) * x
```Cuối cùng, dung lượng chảo áp đặt thêm một giới hạn. Ngay cả khi thành phần cho phép số lượng lớn hơn, câu trả lời không thể vượt quá`V`. 

Vì vậy, kết quả cuối cùng là:```
min(V, sum(a) * min(bi / ai))
```Điều này làm giảm toàn bộ vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tìm kiếm nhị phân | O(n độ chính xác của nhật ký) | O(1) | Đã chấp nhận | 
| Giải toán trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`V`. 
2. Đọc mảng`a`Và`b`. 
3. Tính hệ số tỷ lệ tối đa có thể`x`. 

Với mỗi thành phần hãy tính:```
bi / ai
```Điều này cho chúng ta biết lớn như thế nào`x`có thể trở thành trước khi thành phần đó hết. 
4. Lấy giá trị tối thiểu trong số tất cả các giá trị này. 

Tỷ lệ nhỏ nhất xác định thành phần gây tắc nghẽn. Bất kỳ lớn hơn`x`sẽ vi phạm ít nhất một hạn chế về thành phần. 
5. Tính tổng hệ số công thức:```
sum(a)
```Nếu công thức được chia tỷ lệ theo`x`, thì tổng thể tích súp trở thành:```
sum(a) * x
```6. So sánh thể tích này với dung tích chảo`V`. 

Ngay cả khi nguyên liệu cho phép nhiều súp hơn, chảo cũng không thể chứa được nhiều hơn`V`. 
7. Xuất ra giá trị nhỏ hơn. 

### Tại sao nó hoạt động 

Mỗi món súp khả thi đều phải tuân theo tỷ lệ công thức chính xác. Điều đó có nghĩa là mọi số lượng thành phần hoàn toàn được xác định bởi một hệ số tỷ lệ duy nhất`x`. 

Mỗi thành phần độc lập hạn chế mức độ lớn`x`Có lẽ. Công thức này chỉ khả thi nếu tất cả các ràng buộc được giữ đồng thời, do đó phạm vi hợp lệ cho`x`được giới hạn bởi giá trị nhỏ nhất của`bi / ai`. 

Việc chọn mức tối thiểu này sẽ mang lại hệ số tỷ lệ khả thi lớn nhất. Nhân với tổng hệ số công thức sẽ chuyển đổi hệ số tỷ lệ thành khối lượng súp cuối cùng. Áp dụng`min(..., V)`thực thi ràng buộc công suất pan. 

Vì mọi giải pháp khả thi đều tương ứng với đúng một giá trị của`x`và chúng tôi chọn giá trị hợp lệ tối đa có thể`x`, thuật toán luôn cho ra câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, V = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

x = float('inf')

for ai, bi in zip(a, b):
    x = min(x, bi / ai)

answer = min(V, sum(a) * x)

print(answer)
```Phần đầu tiên đọc các mảng đầu vào. Vì tỷ lệ công thức và số lượng thành phần có sẵn được ghép nối từng thành phần, lặp đi lặp lại với`zip(a, b)`giữ logic đơn giản và an toàn. 

Biến`x`lưu trữ hệ số tỷ lệ khả thi lớn nhất được phát hiện cho đến nay. Nó bắt đầu ở vô cùng để thành phần đầu tiên tự động trở thành giới hạn ban đầu. 

Đối với mỗi thành phần, chúng tôi tính toán`bi / ai`. Sự phân chia này phải vẫn là dấu phẩy động. Việc sử dụng phép chia số nguyên sẽ âm thầm cắt bớt các câu trả lời phân số hợp lệ. 

Sau khi tìm được hệ số tỷ lệ tối thiểu, chúng tôi nhân nó với`sum(a)`. Điều này có tác dụng vì nhân rộng công thức theo`x`nhân mỗi lượng thành phần với`x`, do đó tổng khối lượng tỷ lệ theo cùng một hệ số. 

Cuối cùng, chúng tôi áp dụng giới hạn dung lượng chảo với`min(V, ...)`. 

Không cần xử lý đặc biệt đối với lượng thành phần bằng không trong`b`, vì công thức xử lý chúng một cách tự nhiên. Nếu một số`bi = 0`, thì lực thành phần đó`x = 0`, nghĩa là không thể nấu được món súp nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 100
1
40
```| Thành phần | ai | bi | bi/ai | Tối thiểu hiện tại x | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 40 | 40,0 | 40,0 | 

Sau đó: 

| Giá trị | Kết quả | 
| --- | --- | 
| tổng(a) | 1 | 
| khối lượng súp | 1 × 40 = 40 | 
| công suất chảo | 100 | 
| câu trả lời cuối cùng | 40 | 

Thành phần duy nhất trực tiếp xác định khối lượng súp tối đa. Chảo đủ lớn nên sự sẵn có của nguyên liệu là hạn chế duy nhất. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 10
1 2
10 10
```| Thành phần | ai | bi | bi/ai | Tối thiểu hiện tại x | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 10.0 | 10.0 | 
| 2 | 2 | 10 | 5.0 | 5.0 | 

Sau đó: 

| Giá trị | Kết quả | 
| --- | --- | 
| tổng(a) | 3 | 
| khối lượng súp | 3 × 5 = 15 | 
| công suất chảo | 10 | 
| câu trả lời cuối cùng | 10 | 

Nguyên liệu cho phép chứa 15 lít, nhưng chảo chỉ chứa được 10 lít. Ví dụ này xác nhận rằng cả hai ràng buộc phải được thực thi một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét tuyến tính qua các thành phần | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Với tối đa 20 thành phần, thời gian chạy rất nhỏ. Giải pháp chỉ thực hiện một số phép tính số học cho mỗi thành phần và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, V = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    x = float('inf')

    for ai, bi in zip(a, b):
        x = min(x, bi / ai)

    print(min(V, sum(a) * x))

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
assert run("1 100\n1\n40\n") == "40.0", "sample 1"

# minimum-size input with zero ingredient
assert run("1 10\n1\n0\n") == "0.0", "zero soup possible"

# pan capacity is the limiting factor
assert run("1 5\n1\n100\n") == "5", "pan capacity limit"

# fractional scaling factor
assert run("2 100\n3 2\n10 10\n") == "16.666666666666668", "floating point handling"

# all ingredients equally limiting
assert run("3 100\n1 1 1\n5 5 5\n") == "15.0", "balanced ratios"

# larger case
assert run(
    "5 1000\n1 2 3 4 5\n10 20 30 40 50\n"
) == "150.0", "all ratios identical"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10 / 1 / 0`|`0.0`| Không thể nấu súp khi thiếu một thành phần | 
|`1 5 / 1 / 100`|`5`| Công suất chảo ghi đè nguồn cung cấp nguyên liệu | 
|`2 100 / 3 2 / 10 10`|`16.666666666666668`| Tính toán dấu phẩy động đúng | 
|`3 100 / 1 1 1 / 5 5 5`|`15.0`| Ràng buộc thành phần đối xứng | 
|`5 1000 / 1 2 3 4 5 / 10 20 30 40 50`|`150.0`| Tất cả các tỷ lệ đều tạo ra cùng một nút thắt | 

## Vỏ cạnh 

Hãy xem xét trường hợp một thành phần không có sẵn:```
2 100
1 1
10 0
```Thuật toán tính toán:```
10 / 1 = 10
0 / 1 = 0
```Hệ số tỷ lệ tối thiểu trở thành`0`. Tổng khối lượng súp cũng là`0`. 

Điều này đúng vì công thức yêu cầu cả hai thành phần. Thiếu dù chỉ một thành phần cũng khiến công thức không thể thực hiện được. 

Bây giờ hãy xem xét trường hợp chảo là nút cổ chai:```
2 5
1 1
100 100
```Các ràng buộc về thành phần cho phép:```
x = 100
```Tổng hệ số của công thức là:```
1 + 1 = 2
```Vì vậy, các thành phần sẽ cho phép`200`lít súp. Thuật toán sau đó được áp dụng:```
min(5, 200)
```và đầu ra`5`. 

Cuối cùng, hãy xem xét một câu trả lời phân số:```
2 100
2 3
5 10
```Các giới hạn là:```
5 / 2 = 2.5
10 / 3 = 3.333...
```Hệ số tỷ lệ tối thiểu là`2.5`. 

Khối lượng súp trở thành:```
(2 + 3) * 2.5 = 12.5
```Thuật toán duy trì độ chính xác của dấu phẩy động trong suốt quá trình tính toán, do đó, câu trả lời không nguyên chính xác sẽ được tạo ra.
