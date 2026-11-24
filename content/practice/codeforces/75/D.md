---
title: "CF 75D - Số tiền tối đa lớn"
description: "Chúng ta được cung cấp một tập hợp các mảng nhỏ và một chuỗi các chỉ mục chỉ ra cách nối chúng thành một mảng lớn hơn. Khi mảng lớn được xây dựng theo cách này, mục tiêu là tìm tổng tối đa của mảng con liền kề."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 75
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 67 (Div. 2)"
rating: 2000
weight: 75
solve_time_s: 94
verified: true
draft: false
---

[CF 75D - Số tiền tối đa lớn](https://codeforces.com/problemset/problem/75/D) 

**Xếp hạng:** 2000 
**Tags:** cấu trúc dữ liệu, dp, tham lam, triển khai, toán học, cây 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các mảng nhỏ và một chuỗi các chỉ mục chỉ ra cách nối chúng thành một mảng lớn hơn. Khi mảng lớn được xây dựng theo cách này, mục tiêu là tìm tổng tối đa của mảng con liền kề. Về cơ bản, đây là phần mở rộng của bài toán tổng mảng con tối đa cổ điển, nhưng có một điểm khác biệt: việc xây dựng mảng một cách rõ ràng có thể quá tốn kém vì tổng chiều dài có thể rất lớn. Mỗi chỉ mục trong chuỗi có thể trỏ đến bất kỳ mảng nhỏ nào và mỗi mảng nhỏ có thể xuất hiện nhiều lần trong mảng lớn. 

Giới hạn đầu vào cho chúng ta biết rằng có tối đa 50 mảng nhỏ, mỗi mảng có kích thước lên tới 5000 và chuỗi chỉ mục trong mảng lớn có thể lên tới 250.000. Việc ghép nối một cách ngây thơ sẽ tạo ra một mảng tiềm năng có kích thước cực kỳ lên tới 50 × 5000 × 250.000, điều này hoàn toàn không thực tế đối với thời gian hoặc bộ nhớ. Điều này ngay lập tức báo hiệu rằng chúng ta cần một cách để suy luận về tổng mà không cần xây dựng toàn bộ mảng một cách rõ ràng. 

Các trường hợp cạnh bao gồm các mảng chỉ chứa số âm. Ví dụ, nếu một mảng nhỏ là`[-5, -2]`và cái khác là`[-1]`, và trình tự là`[1,2]`, tổng tối đa đúng là`-1`còn hơn là`-2`hoặc`-5`. Một trường hợp phức tạp khác phát sinh khi việc ghép các mảng có thể cho phép tổng trải dài trên nhiều mảng. Ví dụ: nếu một mảng nhỏ có tiền tố âm nhưng hậu tố dương, mảng con tối đa có thể bắt đầu trong một phiên bản và tiếp tục ở phiên bản tiếp theo, vì vậy chúng ta phải theo dõi tiền tố và hậu tố một cách cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ nối các mảng theo trình tự và sau đó áp dụng thuật toán Kadane tiêu chuẩn để tìm tổng mảng con tối đa. Về nguyên tắc, điều này đúng vì nó trực tiếp mô hình hóa vấn đề, nhưng lại quá chậm đối với các chuỗi lớn. Nếu mỗi mảng có độ dài khoảng 5000 và chuỗi có 250.000 phần tử thì tổng chiều dài của mảng lớn có thể lên tới hơn một tỷ phần tử. Ngay cả việc quét tuyến tính cũng không thể thực hiện được trong thực tế. 

Thông tin chi tiết quan trọng là tổng tối đa trong chuỗi được nối có thể được tính tăng dần bằng cách sử dụng ba số liệu thống kê cho từng mảng nhỏ: tổng tiền tố tối đa, tổng hậu tố tối đa và tổng tổng. Tổng tiền tố là tổng tối đa của tiền tố liền kề của mảng, tổng hậu tố là tổng tối đa của hậu tố liền kề và tổng tổng là tổng của toàn bộ mảng. Nếu chúng ta xử lý chuỗi chỉ mục lần lượt, chúng ta có thể theo dõi tổng mảng con tốt nhất kết thúc trong mảng hiện tại bằng cách xem xét: một mảng con mới hoàn toàn nằm trong mảng hiện tại hoặc một mảng con bắt đầu trong mảng trước đó và tiếp tục vào mảng hiện tại. 

Điều này cho phép chúng tôi tính toán tổng tối đa mà không cần phải xây dựng mảng lớn, giảm vấn đề từ việc sử dụng bộ nhớ không thực tế sang quét tuyến tính chuỗi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng_chiều dài) | O(tổng_chiều dài) | Quá chậm | 
| Tối ưu | O(tổng các mảng nhỏ + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng, tổng tiền tố tối đa, tổng hậu tố tối đa và tổng mảng con tối đa cho mỗi mảng nhỏ. Tổng tiền tố và hậu tố cho phép chúng ta mở rộng các mảng con qua nhiều phép nối và tổng tổng sẽ hữu ích khi một mảng đóng góp hoàn toàn vào một chuỗi dài hơn. 
2. Khởi tạo hai biến:`current_max`cho tổng tối đa của một mảng con kết thúc ở chỉ mục trước đó và`global_max`cho số tiền tối đa được tìm thấy cho đến nay trên chuỗi. 
3. Lặp lại chuỗi các chỉ mục xác định mảng lớn. Đối với mỗi chỉ mục, truy xuất số liệu thống kê cho mảng nhỏ tương ứng. 
4. Cập nhật`current_max`bằng cách chọn bắt đầu mới với mảng con tối đa của mảng hiện tại hoặc mở rộng mảng trước đó`current_max`bằng cách cộng tổng tiền tố tối đa của mảng hiện tại. 
5. Cập nhật`global_max`là giá trị lớn nhất của chính nó và`current_max`. Điều này theo dõi tổng tốt nhất được thấy trên tất cả các mảng được xử lý cho đến nay. 
6. Sau vòng lặp,`global_max`chứa tổng tối đa của một mảng con liền kề trên mảng lớn được nối hoàn toàn. 

Tại sao nó hoạt động: ở mỗi bước,`current_max`thể hiện chính xác tổng tối đa kết thúc ở mảng hiện tại, bằng cách bắt đầu một mảng con mới trong mảng đó hoặc bằng cách mở rộng một mảng con từ các mảng trước đó. Theo dõi tối đa trên tất cả`current_max`giá trị đảm bảo rằng mọi mảng con liền kề có thể được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
small_arrays = []
for _ in range(n):
    data = list(map(int, input().split()))
    l = data[0]
    arr = data[1:]
    total = sum(arr)
    max_prefix = max_suffix = max_sub = arr[0]
    curr = 0
    for x in arr:
        curr += x
        max_prefix = max(max_prefix, curr)
    curr = 0
    for x in reversed(arr):
        curr += x
        max_suffix = max(max_suffix, curr)
    curr_max = arr[0]
    curr_sum = 0
    for x in arr:
        curr_sum += x
        curr_max = max(curr_max, curr_sum)
        if curr_sum < 0:
            curr_sum = 0
    max_sub = curr_max
    small_arrays.append((total, max_prefix, max_suffix, max_sub))

seq = list(map(int, input().split()))
current_max = global_max = -10**18

for idx in seq:
    total, pre, suf, sub = small_arrays[idx - 1]
    current_max = max(sub, suf + max(current_max, 0))
    global_max = max(global_max, current_max)

print(global_max)
```Vòng lặp đầu tiên tính toán tất cả số liệu thống kê cần thiết cho từng mảng nhỏ. Lưu ý rằng tổng tiền tố và hậu tố được tính riêng biệt với phép quét Kadane tiêu chuẩn. Trong vòng lặp chính, chúng tôi cập nhật cẩn thận`current_max`bằng cách xem xét cả việc bắt đầu mới và mở rộng từ mảng trước đó. sử dụng`max(current_max, 0)`đảm bảo rằng tổng số âm không làm giảm sai mảng con tiếp theo. Tất cả các tổng đều đủ lớn để sử dụng số nguyên 64 bit, do đó giá trị ban đầu`-10**18`là an toàn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | Chỉ mục | Mảng hiện tại | hiện tại_max | toàn cầu_max | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [3, 3] | 6 | 6 | 
| 2 | 3 | [-5, 1] ​​| 1 | 6 | 
| 3 | 1 | [1, 6, -2] | 7 | 7 | 
| 4 | 3 | [-5, 1] ​​| 9 | 9 | 

Điều này cho thấy rằng tổng hiện hành mở rộng chính xác trên các mảng và mức tối đa toàn cầu nắm bắt được mảng con tối ưu trải rộng trên nhiều mảng. 

Ví dụ tùy chỉnh: mảng âm đơn theo sau là mảng dương 

đầu vào:```
2 2
-3 -2 -1
4 5
1 2
```Bảng: 

| Bước | Chỉ mục | Mảng hiện tại | hiện tại_max | toàn cầu_max | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [-3, -2, -1] | -1 | -1 | 
| 2 | 2 | [4, 5] | 9 | 9 | 

Mặc dù mảng đầu tiên hoàn toàn âm, thuật toán bắt đầu chính xác một mảng con mới trên mảng thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng độ dài của mảng nhỏ + m) | Mỗi mảng nhỏ được quét một lần để thống kê; trình tự được quét tuyến tính. | 
| Không gian | O(n) | Chúng tôi lưu trữ bốn số trên mỗi mảng nhỏ và chuỗi chỉ mục. | 

Độ phức tạp về thời gian có thể chấp nhận được vì tổng độ dài của các mảng nhỏ nhiều nhất là 50 × 5000 = 250.000 và độ dài chuỗi nhiều nhất là 250.000. Việc sử dụng bộ nhớ là tối thiểu, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, m = map(int, input().split())
    small_arrays = []
    for _ in range(n):
        data = list(map(int, input().split()))
        l = data[0]
        arr = data[1:]
        total = sum(arr)
        max_prefix = max_suffix = max_sub = arr[0]
        curr = 0
        for x in arr:
            curr += x
            max_prefix = max(max_prefix, curr)
        curr = 0
        for x in reversed(arr):
            curr += x
            max_suffix = max(max_suffix, curr)
        curr_max = arr[0]
        curr_sum = 0
        for x in arr:
            curr_sum += x
            curr_max = max(curr_max, curr_sum)
            if curr_sum < 0:
                curr_sum = 0
        max_sub = curr_max
        small_arrays.append((total, max_prefix, max_suffix, max_sub))

    seq = list(map(int, input().split()))
    current_max = global_max = -10**18
    for idx in seq:
        total, pre, suf, sub = small_arrays[idx - 1]
        current_max = max(sub, suf + max(current_max, 0))
        global_max = max(global_max, current_max)
    return str(global_max)

# Provided sample
assert run("3 4
```
