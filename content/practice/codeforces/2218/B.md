---
title: "CF 2218B - Bài toán số nguyên 6-7 lần thứ 67"
description: "Chúng ta được cho đúng bảy số nguyên. Nhiệm vụ là phủ định sáu trong số chúng – nhân chúng với -1 – và giữ nguyên một giá trị, sau đó tính tổng. Chúng ta muốn số tiền tối đa có thể đạt được bằng cách chọn sáu số nào để phủ định."
date: "2026-06-07T18:30:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2218
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1090 (Div. 4)"
rating: 800
weight: 2218
solve_time_s: 109
verified: false
draft: false
---

[CF 2218B - Bài toán số nguyên 6-7 lần thứ 67](https://codeforces.com/problemset/problem/2218/B) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho đúng bảy số nguyên. Nhiệm vụ là phủ định sáu trong số chúng – nhân chúng với`-1`-và giữ nguyên một số, sau đó tính tổng. Chúng ta muốn số tiền tối đa có thể đạt được bằng cách chọn sáu số nào để phủ định. Về mặt khái niệm, mỗi số có thể đóng góp tích cực hoặc tiêu cực tùy thuộc vào việc chúng ta lật nó hay không. Vì chỉ có bảy số nên chỉ có bảy cách có thể để chọn số không âm. 

Những hạn chế là rất nhỏ. Mỗi số nằm giữa`-67`Và`67`, và có nhiều nhất`6767`trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ thuật toán nào xử lý một trường hợp thử nghiệm trong thời gian không đổi sẽ chạy hiệu quả, ngay cả ở giới hạn trên của đầu vào. Các trường hợp cạnh bao gồm tất cả các số dương, tất cả âm hoặc hỗn hợp trong đó giá trị tuyệt đối lớn nhất có thể là dương hoặc âm. For example, if all numbers are positive, leaving the largest positive number unnegated will maximize the sum because negating it would subtract the largest amount.

 A naive approach that tries all subsets of six numbers to negate is feasible for seven numbers, but understanding the underlying pattern is better. Chúng ta cần xác định số nào không bị âm để tối đa hóa tổng. 

Một ví dụ về trường hợp cạnh tinh tế là`a = [1, 2, 3, 4, 5, 6, 7]`. Phủ định sáu số và rời đi`7`không phủ định tạo ra một tổng`-1-2-3-4-5-6+7 = -14`. rời đi`1`sản xuất không phủ định`1-2-3-4-5-6-7 = -26`. Giá trị lớn nhất đạt được bằng cách để lại số có giá trị âm nhỏ nhất nếu hầu hết các số đều dương. Một ví dụ khác là`a = [-1, -2, -3, -4, -5, -6, -7]`. Phủ định sáu và rời đi`-1`cho`1+2+3+4+5+6-7 = 14`. rời đi`-7`cho`1+2+3+4+5+6-7 = 14`. Nguyên tắc là tổng tối đa đạt được bằng cách giữ nguyên số có giá trị tuyệt đối nhỏ nhất nếu tất cả đều âm hoặc giữ nguyên số lớn nhất nếu tất cả đều dương. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ liệt kê tất cả các tập hợp con của sáu số để phủ định và tính tổng mỗi lần. Có chính xác bảy tập con như vậy vì chọn sáu tập con để phủ định tương đương với việc chọn một tập con để bỏ. Việc tính tổng của mỗi tập hợp con yêu cầu tính tổng bảy số, dẫn đến tổng số`7*7 = 49`hoạt động cho mỗi trường hợp thử nghiệm. Điều này cực kỳ nhỏ và khả thi, nhưng chúng ta có thể suy luận cẩn thận hơn để tránh điều này. 

Quan sát quan trọng là việc phủ định sáu số trong bảy số tương đương với việc phủ định tất cả các số ngoại trừ một số. Vì vậy, đối với mỗi trường hợp thử nghiệm, chúng ta chỉ cần xem xét việc để lại từng số không âm và tính tổng mảng đã chuyển đổi. Vì số tiền sau khi rời đi`a[i]`không âm bằng số âm của tổng các số khác cộng`a[i]`, bài toán quy về việc tính tổng của tất cả các số một lần và sau đó, đối với mỗi ứng cử viên, tính tổng`sum_except_i = total_sum - a[i]`và tổng ứng viên`candidate_sum = -sum_except_i + a[i] = -total_sum + 2 * a[i]`. Giá trị tối đa trong bảy giá trị này là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(7^2) = O(49) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(7) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận, tính toán tối thiểu | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc bảy số nguyên vào một danh sách`a`. 
2. Tính tổng của danh sách,`total_sum = sum(a)`. 
3. Khởi tạo một biến`max_sum`đến một số rất nhỏ, ví dụ`-inf`. 
4. Với mỗi số`a[i]`trong danh sách, tính tổng thu được bằng cách để nó không âm. Đây là`candidate_sum = -total_sum + 2 * a[i]`. 
5. Nếu`candidate_sum`lớn hơn`max_sum`, cập nhật`max_sum`. 
6. Sau khi kiểm tra đủ bảy số,`max_sum`chứa tổng lớn nhất có thể sau khi phủ định sáu số. Xuất nó ra. 

Tại sao nó hoạt động: Phủ định sáu trên bảy số tương đương với việc phủ định tất cả các số ngoại trừ một số. Máy tính`-total_sum + 2 * a[i]`đưa ra tổng một cách hiệu quả mà không cần tạo ra một mảng mới cho mỗi lựa chọn. Việc lặp lại tất cả bảy khả năng đảm bảo chúng tôi xem xét mọi lựa chọn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a = list(map(int, input().split()))
    total_sum = sum(a)
    max_sum = -float('inf')
    for x in a:
        candidate = -total_sum + 2 * x
        if candidate > max_sum:
            max_sum = candidate
    print(max_sum)
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và sau đó lặp lại từng trường hợp. Nó chuyển đổi dòng đầu vào thành số nguyên và tính tổng. Vòng lặp tính tổng ứng cử viên để mỗi số không bị âm và theo dõi mức tối đa. sử dụng`-total_sum + 2 * x`tránh tạo mảng mới và đảm bảo tính toán chính xác. Việc khởi tạo của`max_sum`đến vô cực âm xử lý các mảng toàn âm một cách chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`441 41 41 41 41 41 416`| Bước | tổng_tổng | Tổng thí sinh bỏ từng số | max_sum | 
| --- | --- | --- | --- | 
| Tính tổng_sum | 1022 | - | - | 
| Để lại 441 | -1022 + 882 = -140 | -140 | -140 | 
| Để lại 41 | -1022 + 82 = -940 | -140 | -140 | 
| Để lại 41 | -940 | -140 | -140 | 
| Để lại 41 | -940 | -140 | -140 | 
| Để lại 41 | -940 | -140 | -140 | 
| Để lại 41 | -940 | -140 | -140 | 
| Để lại 416 | -1022 + 832 = -190 | -140 | -140 | 

Số tiền tối đa là`-140`. 

Một đầu vào khác:`6 9 4 20 6 7 67`| Bước | tổng_tổng | Tổng số tiền ứng cử viên | max_sum | 
| --- | --- | --- | --- | 
| tổng_tổng | 119 | - | - | 
| Để lại 6 | -119 + 12 = -107 | -107 | -107 | 
| Để lại 9 | -119 + 18 = -101 | -101 | -101 | 
| Để lại 4 | -119 + 8 = -111 | -101 | -101 | 
| Để lại 20 | -119 + 40 = -79 | -79 | -79 | 
| Để lại 6 | -119 + 12 = -79 | -79 | -79 | 
| Để lại 7 | -119 + 14 = -79 | -79 | -79 | 
| Để lại 67 | -119 + 134 = 15 | 15 | 15 | 

Số tiền tối đa là`15`, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * 7) = O(t) | Mỗi trường hợp thử nghiệm liên quan đến việc tính tổng 7 số và lặp lại 7 số | 
| Không gian | O(7) = O(1) | Chỉ có mảng 7 số và một vài biến được lưu trữ | 

Được cho`t <= 6767`, giải pháp thực hiện tối đa`6767 * 7 ≈ 47000`hoạt động, thấp hơn nhiều so với giới hạn thông thường. Việc sử dụng bộ nhớ ở mức tối thiểu và an toàn dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a = list(map(int, input().split()))
        total_sum = sum(a)
        max_sum = -float('inf')
        for x in a:
            candidate = -total_sum + 2 * x
            if candidate > max_sum:
                max_sum = candidate
        print(max_sum)
    return output.getvalue().replace('\r','')

# provided samples
assert run("2\n441 41 41 41 41 41 416\n6 9 4 20 6 7 67\n") == "-205\n15\n", "sample 1"

# custom cases
assert run("1\n1 1 1 1 1 1 1\n") == "-5\n", "all ones"
assert run("1\n-1 -1 -1 -1 -1 -1 -1\n") == "5\n", "all negative ones"
assert run("1\n-5 -4 0 3 2 1 -1\n") == "13\n", "mixed numbers"
assert run("1\n67 67 67 67 67 67 67\n") == "67\n", "all max values"
assert run("1\n-67 -67 -67 -67 -67 -67 -67\n") == "67\n", "all min values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| `1 1 1 1 1 | | |
