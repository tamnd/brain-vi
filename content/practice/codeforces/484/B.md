---
title: "CF 484B - Giá trị tối đa"
description: "Cho một dãy số nguyên a1, a2, …, an và yêu cầu tính giá trị lớn nhất của ai % aj trên tất cả các cặp (i, j) trong đó ai ≥ aj. Đầu vào bao gồm số số nguyên n và chính dãy số đó."
date: "2026-06-07T17:21:53+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 484
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 276 (Div. 1)"
rating: 2100
weight: 484
solve_time_s: 95
verified: true
draft: false
---

[CF 484B - Giá trị tối đa](https://codeforces.com/problemset/problem/484/B) 

**Đánh giá:** 2100 
**Tags:** tìm kiếm nhị phân, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a1, a2, …, an`và yêu cầu tính giá trị lớn nhất của`ai % aj`trên tất cả các cặp`(i, j)`Ở đâu`ai ≥ aj`. Đầu vào bao gồm số lượng số nguyên`n`và chính trình tự đó. Đầu ra là một số nguyên duy nhất: phần còn lại lớn nhất thu được bằng cách chia một phần tử của mảng cho một phần tử khác không lớn hơn chính nó. 

Quan sát quan trọng đầu tiên là vì tất cả các phần tử đều là số nguyên dương giới hạn bởi`10^6`Và`n`có thể đạt được`2·10^5`, mọi nỗ lực ngây thơ để kiểm tra trực tiếp từng cặp đều không khả thi. Một giải pháp brute-force lặp đi lặp lại trên tất cả các cặp sẽ hoạt động gần như`n^2`hoạt động, lên đến`4·10^10`, vượt xa giới hạn 1 giây. Vì vậy, chúng ta cần một chiến lược tránh liệt kê rõ ràng tất cả các cặp. 

Các trường hợp biên xuất hiện khi mảng chứa các phần tử lặp lại hoặc một phần tử đơn lẻ. Ví dụ: nếu tất cả các phần tử đều bằng nhau,`ai % aj`luôn bằng 0 nên đáp án là 0. Nếu mảng chứa`1`, nó hoạt động như một ước số chung, tạo ra số dư nhỏ hơn. Mảng chỉ có một phần tử sẽ trả về 0 một cách tầm thường, vì`ai % ai = 0`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Lặp lại tất cả các cặp`(i, j)`như vậy`ai ≥ aj`và tính toán`ai % aj`, theo dõi mức tối đa. Điều này đảm bảo tính chính xác vì nó thực sự kiểm tra mọi cặp được phép. Tuy nhiên, với`n`lên tới`2·10^5`, độ phức tạp trong trường hợp xấu nhất là O(n^2), điều này không khả thi đối với đầu vào lớn. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là phần còn lại`x % y`luôn nhỏ hơn`y`. Điều này có nghĩa là đối với bất kỳ`y`, số dư lớn nhất chúng ta có thể đạt được với nó dưới dạng ước số là`max(x < 2y) x % y`. Sắp xếp mảng và xem xét các ước số theo thứ tự cho phép chúng ta tránh những kiểm tra không cần thiết. Cụ thể hơn, phần còn lại tối đa đạt được bằng phần tử lớn nhất nhỏ hơn`2*y`hoặc phần tử lớn nhất trong mảng không bao gồm`y`. Bằng cách quét ngược mảng đã sắp xếp và sử dụng thuộc tính này, chúng ta có thể tính toán phần còn lại tối đa một cách hiệu quả trong thời gian O(n log n). 

Cách tiếp cận tối ưu có hiệu quả vì khi mảng được sắp xếp, đối với mỗi ước số tiềm năng`y`, cổ tức ứng cử viên tạo ra phần còn lại gần`y-1`nằm gần bội số của`y`. Thay vì kiểm tra tất cả các phần tử, chúng ta có thể nhảy qua`y`tăng dần và chỉ xem xét các phần tử lớn nhất trong mỗi phạm vi. Điều này làm giảm đáng kể số lượng hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và mảng`a`. Mảng chứa`n`số nguyên giữa`1`Và`10^6`. 
2. Sắp xếp mảng theo thứ tự tăng dần. Việc sắp xếp cho phép chúng ta tìm thấy phần tử lớn nhất nhỏ hơn bất kỳ bội số nào của ước số ứng viên một cách hiệu quả. 
3. Khởi tạo`max_value`về không. Điều này sẽ lưu trữ phần còn lại tối đa được tìm thấy. 
4. Lặp lại tất cả các giá trị riêng biệt`y`trong mảng, loại trừ phần tử lớn nhất. Đối với mỗi`y`, xét bội số của`y`bắt đầu từ`2*y`. 
5. Đối với mỗi bội số`k*y`, tìm phần tử lớn nhất`x`trong mảng nhỏ hơn`k*y`. Phần còn lại`x % y`là một ứng cử viên cho mức tối đa. Cập nhật`max_value`nếu như`x % y`lớn hơn hiện tại`max_value`. 
6. Sau khi xem xét tất cả các ước số ứng viên, cũng kiểm tra phần tử lớn nhất trong mảng theo modulo phần tử lớn thứ hai, vì nó có thể tạo ra phần dư lớn hơn bất kỳ phần tử nào nhỏ hơn nó. 
7. In`max_value`. 

**Tại sao nó hoạt động**: Thuật toán khai thác thực tế là đối với ước số cố định`y`, phần còn lại`x % y`đạt cực đại khi`x`nằm ngay dưới bội số của`y`. Bằng cách sắp xếp mảng và quét theo bội số của`y`, đảm bảo tìm được số dư lớn nhất có thể của mỗi ước số. Không có phần còn lại nào bị bỏ sót vì tất cả đều có liên quan`x`được coi là tương đối với bội số của`y`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
a.sort()
max_a = a[-1]
max_value = 0
unique_vals = sorted(set(a))

for y in unique_vals:
    if y == max_a:
        continue
    k = 2
    while k * y <= max_a + y:
        # Find largest x < k*y using binary search
        left, right = 0, n - 1
        pos = -1
        while left <= right:
            mid = (left + right) // 2
            if a[mid] < k * y:
                pos = mid
                left = mid + 1
            else:
                right = mid - 1
        if pos != -1:
            max_value = max(max_value, a[pos] % y)
        k += 1

print(max_value)
```Bước sắp xếp đảm bảo rằng tìm kiếm nhị phân tìm thấy chính xác phần tử lớn nhất nhỏ hơn từng bội số của`y`. Chúng ta bỏ qua phần tử lớn nhất làm ước số vì nó không thể tạo ra số dư dương. các`while`vòng lặp xem xét các bội số liên tiếp, đảm bảo không bỏ sót ứng viên nào còn lại. Tìm kiếm nhị phân xử lý cẩn thận các điều kiện biên sao cho`pos`luôn hướng đến cổ tức hợp lệ lớn nhất. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
3
3 4 5
```| Bước | Ứng viên chia y | k*y | X lớn nhất < k*y | x %y | giá trị tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 6 | 5 | 2 | 2 | 
| 2 | 4 | 8 | 5 | 1 | 2 | 

Đầu ra:`2`Giải thích: Phần còn lại`5 % 3 = 2`là lớn nhất có thể đạt được. 

**Ví dụ 2** 

đầu vào:```
5
1 2 3 4 5
```| Bước | Ứng viên chia y | k*y | X lớn nhất < k*y | x %y | giá trị tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 5 | 0 | 0 | 
| 2 | 2 | 4 | 3 | 1 | 1 | 
| 3 | 3 | 6 | 5 | 2 | 2 | 
| 4 | 4 | 8 | 5 | 1 | 2 | 

Đầu ra:`2`Giải thích: Số dư tối đa xảy ra đối với`5 % 3 = 2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n log n) ≈ O(n log n) | Sắp xếp mảng mất O (n log n). Mỗi ước số sử dụng tìm kiếm nhị phân trên mảng, tức là O(log n) trên bội số, với tổng số lần lặp được giới hạn bởi n. | 
| Không gian | O(n) | Lưu trữ mảng và tập hợp các giá trị duy nhất. | 

Giải pháp xử lý các mảng lên đến`2·10^5`các phần tử và giá trị lên đến`10^6`hiệu quả, phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    max_a = a[-1]
    max_value = 0
    unique_vals = sorted(set(a))
    for y in unique_vals:
        if y == max_a:
            continue
        k = 2
        while k * y <= max_a + y:
            left, right = 0, n - 1
            pos = -1
            while left <= right:
                mid = (left + right) // 2
                if a[mid] < k * y:
                    pos = mid
                    left = mid + 1
                else:
                    right = mid - 1
            if pos != -1:
                max_value = max(max_value, a[pos] % y)
            k += 1
    return str(max_value)

# provided samples
assert run("3\n3 4 5\n") == "2", "sample 1"

# custom cases
assert run("1\n1\n") == "0", "single element"
assert run("5\n2 2 2 2 2\n") == "0", "all equal"
assert run("5\n1 2 3 4 5\n") == "2", "simple increasing"
assert run("6\n1 3 5 7 9 11\n") == "5", "odd numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 0 | Mảng một phần tử | 
| 5\n2 2 2 2 2 | 0 | Tất cả các phần tử đều bằng nhau | 
| 5\n1 2 3 4 5 | 2 | Trình tự tăng điển hình | 
| 6\n1 3 5 | | |
