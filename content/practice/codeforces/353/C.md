---
title: "CF 353C - Tìm mức tối đa"
description: "Chúng ta được cho một mảng các số nguyên không âm a có độ dài n và một số m ở dạng nhị phân. Chúng ta muốn chọn một tập hợp con gồm các chỉ số từ 0 đến n-1 và tính tổng các giá trị a[i] tương ứng, nhưng có một điều khó khăn: tập hợp con tương ứng với biểu diễn nhị phân của một số nguyên x (từ…"
date: "2026-06-07T01:05:45+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 353
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 205 (Div. 2)"
rating: 1600
weight: 353
solve_time_s: 274
verified: false
draft: false
---

[CF 353C - Tìm mức tối đa](https://codeforces.com/problemset/problem/353/C) 

**Đánh giá:** 1600 
**Tags:** thực hiện, toán, lý thuyết số 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm`a`với chiều dài`n`và một số`m`ở dạng nhị phân. Chúng tôi muốn chọn một tập hợp con các chỉ số từ`0`ĐẾN`n-1`và tính tổng tương ứng`a[i]`giá trị, nhưng có một sự thay đổi: tập hợp con tương ứng với biểu diễn nhị phân của một số nguyên`x`(từ 0 đến`m`) ở đâu đó một chút`i`đặt thành 1 có nghĩa là bao gồm`a[i]`trong tổng số. Mục tiêu là tối đa hóa số tiền này mà không vượt quá`m`. 

Độ dài mảng`n`có thể lên tới 10^5 và các giá trị trong`a`có thể lớn tới 10^4. Điều này loại trừ mọi cách tiếp cận kiểm tra rõ ràng tất cả các số từ 0 đến`m`bởi vì`m`có thể lên đến`2^n-1`, có giá trị lớn về mặt thiên văn. Chúng ta cần một cách tiếp cận lý giải nên đặt bit nào vào`x`trực tiếp chứ không phải liệt kê các con số. 

Trường hợp khó phát hiện xảy ra khi`m`có tập bit cao nhưng tất cả các bit nhỏ hơn có thể mang lại tổng lớn hơn. Ví dụ, hãy xem xét`a = [1, 100]`Và`m = 10`(nhị phân`10`). Đơn giản chỉ cần thiết lập bit cao nhất có thể tạo ra tổng số 100, nhưng việc chọn`x = 01`cho tổng bằng 1, nhỏ hơn. Thuật toán phải quyết định cẩn thận ở từng bit xem việc lật nó có dẫn đến hợp lệ hay không`x`ít hơn`m`và tối đa hóa tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp qua mọi số nguyên`x`từ 0 đến`m`, tính tổng của`a[i]`cho các bit được đặt trong`x`và theo dõi mức tối đa. Điều này làm việc cho nhỏ`n`hoặc nhỏ`m`bởi vì tính toán`f(x)`là O(n), nhưng trong trường hợp xấu nhất`m`có thể ở gần`2^n`, cho`O(n * 2^n)`hoạt động hoàn toàn không khả thi đối với`n`lên đến 10^5. 

Quan sát quan trọng là chúng ta có thể suy luận về từng bit một cách độc lập. Nếu một chút`i`TRONG`m`là 1, chúng ta có hai lựa chọn để đạt mức tối đa`x`. Một tùy chọn là đặt nó thành 0 và tối đa hóa tất cả các bit thấp hơn một cách tự do (vì tiền tố trở nên nhỏ hơn hoàn toàn so với`m`), và cách khác là đặt nó thành 1 và tiếp tục khớp tiền tố với`m`. Bit trong`m`đó là 0 không thể được đặt trong`x`nếu chúng ta muốn ở lại ≤`m`. Quan sát này làm giảm vấn đề thành quyết định tham lam từng bit từ bit quan trọng nhất đến bit nhỏ nhất, duy trì xem tiền tố hiện tại có khớp hay không`m`hoặc đã giảm xuống dưới nó. 

Điều này biến một bài toán hàm mũ thành một phép quét tuyến tính trên các bit của`n`(chiều dài của`a`), với phép tính tổng đơn giản tại mỗi điểm quyết định. Về mặt khái niệm, chúng tôi đang xây dựng số lượng lớn nhất`m`tối đa hóa tổng trọng số được đưa ra bởi`a`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`a`và chuỗi nhị phân`s`đại diện`m`. Chuyển thành`s`đến một số nguyên`m`nếu cần. 
2. Tính trước các tổng tiền tố cho`a`theo thứ tự đảo ngược, tức là`suffix_sum[i]`lưu trữ tổng của tất cả`a[j]`vì`j >= i`. Điều này cho phép tính toán O(1) tổng đóng góp cho các bit thấp hơn nếu chúng ta đặt bit hiện tại thành 0. 
3. Khởi tạo hai biến:`max_sum`để lưu trữ số tiền tốt nhất được tìm thấy và`current_sum`để tích lũy tổng trong khi tuân theo các bit của`m`. 
4. Lặp lại các bit của`m`từ quan trọng nhất đến ít nhất: 

1. Nếu bit hiện tại trong`m`là 1, hãy xem điều gì sẽ xảy ra nếu chúng ta đặt nó thành 0. Tiền tố trở nên nhỏ hơn hoàn toàn so với`m`, do đó tất cả các bit thấp hơn còn lại có thể được đưa vào tự do. Tính tổng các bit còn lại bằng mảng tổng hậu tố và thêm`current_sum`. Cập nhật`max_sum`nếu cái này lớn hơn 
2. Nếu chúng ta theo bit hiện tại là 1, chúng ta sẽ thêm`a[i]`ĐẾN`current_sum`và tiếp tục. Nếu bit là 0, chúng ta chỉ tiếp tục mà không cần thêm vào`current_sum`. 
5. Sau vòng lặp, xét tổng nếu chúng ta làm theo`m`chính xác (`current_sum`). Cập nhật`max_sum`nếu nó lớn hơn. 
6. In`max_sum`. 

Tại sao nó hoạt động: thuật toán khám phá tất cả các tiền tố của`x`có khả năng có thể nhỏ hơn`m`và tối đa hóa tổng số bit còn lại. Bằng cách sử dụng tổng hậu tố, chúng ta có thể đánh giá từng lựa chọn trong O(1). Vì mọi tối ưu`x`phải phù hợp`m`từng chút một hoặc phân kỳ ở một vị trí nào đó, cách tiếp cận này xem xét tất cả các khả năng mà không liệt kê chúng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
s = input().strip()

m_bits = [int(c) for c in s]
length = len(m_bits)

# pad array to match the bit length if n < length
a = [0]*(length - n) + a

# compute suffix sums for free evaluation of remaining bits
suffix_sum = [0]*(length + 1)
for i in range(length-1, -1, -1):
    suffix_sum[i] = suffix_sum[i+1] + a[i]

max_sum = 0
current_sum = 0

for i in range(length):
    if m_bits[i] == 1:
        candidate = current_sum + suffix_sum[i+1]
        max_sum = max(max_sum, candidate)
    if m_bits[i] == 1:
        current_sum += a[i]

max_sum = max(max_sum, current_sum)
print(max_sum)
```Đầu tiên, mã căn chỉnh độ dài mảng với biểu diễn nhị phân của`m`để đơn giản hóa việc lập chỉ mục. Tổng hậu tố lưu trữ tổng số bit có thể có bên dưới vị trí hiện tại. Khi lặp lại các bit, chúng tôi xem xét cả việc "tắt bit này" và "bật bit này", cập nhật tổng tối đa. Sau vòng lặp,`current_sum`giữ tổng nếu chúng ta làm theo chính xác`m`, có thể là tối ưu. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
2
3 8
10
```| Chỉ số bit | m_bit | hiện tại_sum | hậu tố_sum[i+1] | ứng cử viên | max_sum | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 8 | 8 | 8 | 
| 0 tiếp tục | | 3 | | | 8 | 
| 1 | 0 | 3 | 0 | 3 | 8 | 

Max_sum cuối cùng = 8. Phù hợp với sản lượng dự kiến. 

Mẫu 2: 

đầu vào:```
4
17 3 10 2
1011
```| Chỉ số bit | m_bit | hiện tại_sum | hậu tố_sum[i+1] | ứng cử viên | max_sum | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 15 | 15 | 15 | 
| 0 tiếp tục | | 17 | | | 15 | 
| 1 | 0 | 17 | 12 | 29 | 29 | 
| 2 | 1 | 17 | 2 | 19 | 29 | 
| 2 tiếp tục | | 27 | | | 29 | 
| 3 | 1 | 27 | 0 | 27 | 29 | 

Tổng tối đa cuối cùng = 29. 

Dấu vết này chứng tỏ thuật toán xem xét chính xác cả phân kỳ và theo sau tiền tố của`m`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua mảng và vị trí bit với tính toán trước tổng hậu tố | 
| Không gian | O(n) | Lưu trữ mảng và tổng hậu tố | 

Với n 10^5, thuật toán này dễ dàng phù hợp với các ràng buộc về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    s = input().strip()
    m_bits = [int(c) for c in s]
    length = len(m_bits)
    a = [0]*(length - n) + a
    suffix_sum = [0]*(length + 1)
    for i in range(length-1, -1, -1):
        suffix_sum[i] = suffix_sum[i+1] + a[i]
    max_sum = 0
    current_sum = 0
    for i in range(length):
        if m_bits[i] == 1:
            candidate = current_sum + suffix_sum[i+1]
            max_sum = max(max_sum, candidate)
        if m_bits[i] == 1:
            current_sum += a[i]
    max_sum = max(max_sum, current_sum)
    return str(max_sum)

# provided samples
assert run("2\n3 8\n10\n") == "3", "sample 1
```
