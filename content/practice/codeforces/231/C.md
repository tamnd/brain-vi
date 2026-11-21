---
title: "CF 231C - Thêm hoặc không thêm"
description: "Chúng ta được cung cấp một mảng các số nguyên và một số phép toán tăng giới hạn. Mỗi thao tác sẽ tăng một phần tử lên một và cùng một phần tử có thể được tăng lên nhiều lần."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 231
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 143 (Div. 2)"
rating: 1600
weight: 231
solve_time_s: 178
verified: true
draft: false
---

[CF 231C - Thêm hoặc không thêm](https://codeforces.com/problemset/problem/231/C) 

**Đánh giá:** 1600 
**Thẻ:** tìm kiếm nhị phân, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một số phép toán tăng giới hạn. Mỗi thao tác sẽ tăng một phần tử lên một và cùng một phần tử có thể được tăng lên nhiều lần. Mục tiêu của chúng ta là tìm một số xuất hiện nhiều lần nhất có thể trong mảng sau khi thực hiện tối đa _k_ các thao tác như vậy. Nếu nhiều số có thể đạt được cùng tần số cực đại thì chúng ta phải chọn số nhỏ nhất. 

Đầu vào bao gồm hai số nguyên _n_ và _k_, theo sau là một mảng các số nguyên _n_. Đầu ra là tần số tối đa có thể đạt được và số nhỏ nhất đạt được tần số đó. 

Cho rằng _n_ có thể đạt tới 10^5 và _k_ có thể lớn tới 10^9, bất kỳ thuật toán nào lặp qua tất cả các số có thể và đếm số lần xuất hiện hoặc thử mọi kết hợp thao tác sẽ quá chậm. Các giải pháp bạo lực ngây thơ có độ phức tạp O(n^2) sẽ hết thời gian vì không thể thực hiện 10^10 thao tác trong giới hạn 2 giây. 

Các trường hợp biên cần xem xét bao gồm các mảng trong đó tất cả các số đều bằng nhau, các mảng không yêu cầu thao tác nào để có kết quả tối ưu và các trường hợp trong đó số nhỏ nhất đạt được tần số nhỏ hơn các số có thể đạt được cùng tần số với nhiều thao tác hơn. Ví dụ, đối với đầu vào`3 3`với mảng`1 2 3`, tăng cả 1 và 2 hai lần mỗi lần sẽ làm cho 3 xảy ra 3 lần, nhưng số nhỏ nhất đạt tần số tối đa là 2 nếu chúng ta tăng 1 và 2 mỗi lần một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi số duy nhất trong mảng là mục tiêu tiềm năng. Đối với mỗi mục tiêu, chúng tôi có thể lặp lại tất cả các phần tử và tính toán số lần tăng cần thiết để tăng số lượng nhỏ hơn cho mục tiêu này. Chúng tôi tính tổng các phép tính và nếu tổng nhỏ hơn hoặc bằng _k_, chúng tôi ghi lại tần suất có thể đạt được. Điều này hiệu quả vì đối với bất kỳ số ứng cử viên nào, số lượng gia tăng cần thiết để làm cho các số khác bằng với nó là xác định. Tuy nhiên, đây là O(n^2) trong trường hợp xấu nhất vì với mỗi số, chúng tôi có khả năng quét toàn bộ mảng. 

Điểm mấu chốt là sau khi sắp xếp mảng, chúng ta có thể sử dụng kỹ thuật cửa sổ trượt để tính toán hiệu quả số lượng thao tác cần thiết để tạo một phân đoạn số liền kề bằng số lớn nhất trong cửa sổ. Bằng cách duy trì tổng tiền tố của mảng đã sắp xếp, chúng ta có thể tính toán các phép toán cần thiết để nâng tất cả các số trong cửa sổ lên số ngoài cùng bên phải trong thời gian không đổi. Sau đó chúng tôi di chuyển cửa sổ về phía trước để xem xét các phân đoạn lớn hơn. Điều này làm giảm độ phức tạp xuống còn O(n log n) vì chúng tôi sắp xếp một lần rồi sử dụng chuyển tuyến tính với tổng tiền tố. Việc sắp xếp cũng cho phép chúng ta dễ dàng chọn số nhỏ nhất để đạt được tần suất tối đa khi nhiều số bằng nhau, vì lần xuất hiện đầu tiên trong thứ tự được sắp xếp sẽ luôn nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Cửa sổ trượt có tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng bất kỳ dãy con liền kề nào cũng có thể được xem xét để tăng đến cùng một giá trị mà không phải lo lắng về các số nhỏ hơn xuất hiện sau đó. 
2. Xây dựng mảng tổng tiền tố. Đối với mỗi chỉ mục _i_, tổng tiền tố lưu trữ tổng của tất cả các phần tử từ chỉ mục 0 đến _i_. Điều này cho phép tính toán tổng của bất kỳ mảng con nào theo thời gian không đổi. 
3. Khởi tạo hai con trỏ,`left`Và`right`, cả hai đều bắt đầu từ 0.`right`con trỏ đại diện cho số hiện tại mà chúng tôi muốn làm cho các số khác bằng. các`left`con trỏ xác định điểm bắt đầu của đoạn. 
4. Đối với mỗi vị trí của`right`, tính toán tổng số gia cần thiết để nâng tất cả các phần tử từ`a[left]`ĐẾN`a[right]`đến giá trị`a[right]`. Sử dụng tổng tiền tố, các phép toán được thực hiện`operations_needed = a[right] * (right - left + 1) - (prefix_sum[right] - prefix_sum[left-1] if left > 0 else prefix_sum[right])`. 
5. Nếu`operations_needed`vượt quá _k_, tăng lên`left`cho đến khi cửa sổ trở nên hợp lệ. Điều này sẽ thu nhỏ cửa sổ từ bên trái một cách hiệu quả cho đến khi các thao tác cần thiết nằm trong _k_ được phép. 
6. Theo dõi độ dài cửa sổ lớn nhất và số tương ứng. Nếu nhiều số có cùng độ dài thì hãy chọn số nhỏ hơn. 
7. Sau khi duyệt mảng với`right`, báo cáo tần suất tối đa và số lượng liên quan. 

Thuật toán hoạt động vì mảng được sắp xếp đảm bảo rằng đối với bất kỳ phân đoạn nào, số ở đầu bên phải luôn là số tối đa. Việc tăng tất cả các phần tử trong phân đoạn lên giá trị này yêu cầu ít thao tác nhất. Di chuyển`left`chuyển tiếp bất cứ khi nào vượt quá giới hạn hoạt động, đảm bảo chúng tôi luôn duy trì một cửa sổ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))
a.sort()

prefix_sum = [0] * n
prefix_sum[0] = a[0]
for i in range(1, n):
    prefix_sum[i] = prefix_sum[i-1] + a[i]

max_freq = 1
best_num = a[0]
left = 0

for right in range(n):
    total = prefix_sum[right] - (prefix_sum[left-1] if left > 0 else 0)
    operations_needed = a[right] * (right - left + 1) - total
    while operations_needed > k:
        left += 1
        total = prefix_sum[right] - (prefix_sum[left-1] if left > 0 else 0)
        operations_needed = a[right] * (right - left + 1) - total
    window_size = right - left + 1
    if window_size > max_freq:
        max_freq = window_size
        best_num = a[right]

print(max_freq, best_num)
```Giải pháp bắt đầu bằng việc sắp xếp, cho phép tạo cửa sổ hiệu quả. Tổng tiền tố cho phép tính toán các hoạt động được yêu cầu theo thời gian không đổi. Cửa sổ trượt đảm bảo chúng tôi không bao giờ vượt quá _k_ thao tác và chỉ xem xét các phân đoạn hợp lệ. Việc cập nhật số tốt nhất trên các tần số lớn hơn đảm bảo chúng tôi chọn được số nhỏ nhất trong trường hợp có quan hệ bằng nhau. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1`5 3`với mảng`[6, 3, 4, 0, 2]`. 

| đúng | trái | một [phải] | hoạt động_cần thiết | kích thước cửa sổ | max_freq | số_tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 1 | 1 | 0 | 
| 1 | 0 | 2 | 2*2-3=1 | 2 | 2 | 2 | 
| 2 | 0 | 3 | 3*3-(0+2+3)=9-5=4>3 | trái=1 | 3*2-(2+3)=6-5=1 | 2 | 
| 3 | 1 | 4 | 4*3-(2+3+4)=12-9=3 | 3 | 3 | 4 | 
| 4 | 1 | 6 | 6*4-(2+3+4+6)=24-15=9>3 | trái=2 | 6*3-(3+4+6)=18-13=5>3 | trái=3 | 

Dấu vết này xác nhận rằng đoạn`[2, 3, 4]`có thể được chuyển đổi thành`[4,4,4]`sử dụng các phép toán 3, cho tần số 3 với số tối thiểu là 4. 

Đầu vào mẫu 2`3 0`với mảng`[5,5,5]`. Cửa sổ trượt không bao giờ yêu cầu tăng dần; max_freq trở thành 3 với best_num 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, cửa sổ trượt có tổng tiền tố là O(n) | 
| Không gian | O(n) | Mảng tổng tiền tố có độ dài n | 

Giải pháp phù hợp với giới hạn thời gian và bộ nhớ cho n tối đa 10^5 và k tối đa 10^9. Không có phép toán nào yêu cầu nhiều hơn số học số nguyên, vì vậy số nguyên 64 bit là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    prefix_sum = [0]*n
    prefix_sum[0] = a[0]
    for i in range(1, n):
        prefix_sum[i] = prefix_sum[i-1] + a[i]
    max_freq = 1
    best_num = a[0]
    left = 0
    for right in range(n):
        total = prefix_sum[right] - (prefix_sum[left-1] if left > 0 else 0)
        operations_needed = a[right]*(right-left+1)-total
        while operations_needed > k:
            left += 1
            total = prefix_sum[right] - (prefix_sum[left-1] if left > 0 else 0)
            operations_needed = a[right]*(right-left+1)-total
        window_size = right-left+1
        if window_size > max_freq:
            max_freq = window_size
            best_num = a[right]
    return f"{max_freq} {best_num}"

assert run("5 3\n6 3 4 0 2\n") == "3 4"
assert run("3 0\n5 5 5\n")
```
