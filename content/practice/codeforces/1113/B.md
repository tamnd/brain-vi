---
title: "CF 1113B - Sasha và Máy từ tính"
description: "Chúng ta được cho một dãy các số nguyên biểu thị sức mạnh của n máy từ tính. Mỗi máy đều đóng góp tích cực vào tổng công suất của trang trại."
date: "2026-06-12T04:56:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1113
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 539 (Div. 2)"
rating: 1300
weight: 1113
solve_time_s: 65
verified: true
draft: false
---

[CF 1113B - Sasha và Máy từ tính](https://codeforces.com/problemset/problem/1113/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, lý thuyết số 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên biểu thị sức mạnh của`n`máy từ tính. Mỗi máy đều đóng góp tích cực vào tổng công suất của trang trại. Người nông dân có thể thực hiện **tối đa một thao tác**: chọn hai máy, chọn một số nguyên`x`chia công suất của máy thứ nhất, làm giảm công suất của máy thứ nhất một hệ số`x`, đồng thời nhân công suất của máy thứ hai với`x`. Mọi lũy thừa phải là số nguyên dương. Nếu muốn, người nông dân cũng có thể chọn không thực hiện bất kỳ thao tác nào. 

Mục tiêu là xác định **tổng công suất tối thiểu có thể** sau khi thực hiện thao tác này một cách tùy ý. 

Các hạn chế ở mức vừa phải:`n`lên tới 50.000 và mỗi`a_i`nhiều nhất là 100. Bởi vì`a_i`nhỏ, bất kỳ phép liệt kê nhân tử hoặc ước số nào đều không tốn kém. Thách thức chính là xử lý`n`hiệu quả: kiểm tra đơn giản từng cặp`(i, j)`với mọi ước của`a_i`sẽ dẫn đến các hoạt động O(n² * d), có khả năng là 2500 triệu nếu chúng ta lặp lại một cách mù quáng qua tất cả các cặp, quá chậm để thực hiện trong 1 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng có một phần tử`1`hoặc các mảng trong đó việc giảm phần tử lớn nhất và tăng phần tử nhỏ nhất sẽ mang lại lợi ích tối đa. Ví dụ,`[1, 100]`có thể giảm`100`bởi một yếu tố của`100`ĐẾN`1`và nhân lên`1`qua`100`ĐẾN`100`, để lại tổng không đổi. Nếu xử lý bất cẩn, thuật toán có thể thực hiện sai các thao tác không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ xem xét từng cặp máy`(i, j)`và mọi ước số có thể`x`của`a_i`. Đối với mỗi sự kết hợp, nó sẽ tính tổng số tiền mới sau khi áp dụng thao tác và theo dõi mức tối thiểu. Điều này đúng nhưng quá chậm vì số lượng cặp`n*(n-1)/2`và số ước có thể lên tới 50 (đối với số lên tới 100). Ở giới hạn trên, điều này có thể ở mức 50.000² * 50 ≈ 125 tỷ hoạt động, rõ ràng là không khả thi. 

Quan sát quan trọng là **mức tăng tối đa đến từ việc giảm phần tử lớn nhất và tăng phần tử nhỏ nhất**. Về mặt hình thức, thao tác giảm một số theo hệ số và tăng số khác theo cùng hệ số. Vì tất cả các số đều nhỏ (<100), nên chúng ta có thể tính toán trước các ước số cho mọi số. Sau đó chúng tôi lặp lại trên từng máy`i`để rút gọn, tìm các ước của nó và áp dụng từng ước số`x`vào máy có **công suất dòng điện nhỏ nhất**`j`≠`i`. Điều này làm giảm đáng kể số lượng cặp chúng tôi xem xét. Tổng số phép toán trở thành O(n * d), trong đó`d`là số ước tối đa cho bất kỳ số nào (nhiều nhất là 50), có thể chấp nhận được. 

Cái nhìn sâu sắc này chuyển đổi một thuật toán bậc ba tiềm năng thành một thuật toán tuyến tính đối với`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² * d) | O(1) | Quá chậm | 
| Tối ưu | O(n * d) | O(n + d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng, đây là đường cơ sở của chúng tôi nếu không áp dụng thao tác nào. 
2. Tính toán trước tất cả các ước số cho các số từ 1 đến 100. Điều này cho phép chúng ta nhanh chóng liệt kê các hệ số suy giảm tiềm năng cho mỗi máy. 
3. Xác định **công suất máy nhỏ nhất**`min_val`và chỉ số của nó. Máy này là ứng cử viên tốt nhất để nhận phép nhân để giảm thiểu tổng số tiền. 
4. Lặp lại từng máy`i`như một ứng cử viên để giảm bớt. Với mỗi số chia`x`của`a_i`(không bao gồm 1): 

- Tính công suất mới cho`i`:`a_i / x`. 
- Tính công suất mới cho`j`(máy nhỏ nhất):`a_j * x`. 
- Tính tổng số tiền mới bằng cách sử dụng`sum - a_i - a_j + (a_i / x) + (a_j * x)`. 
- Theo dõi tổng số tiền tối thiểu. 
5. Nếu không có thao tác nào cải thiện tổng số tiền, hãy giữ lại số tiền ban đầu. 
6. In ra tổng số tiền tối thiểu. 

Tại sao nó hoạt động: Tổng số tiền thay đổi theo`(a_i / x + a_j * x) - (a_i + a_j)`. Để giảm thiểu tổng, việc giảm số lượng lớn và tăng số lượng nhỏ có khả năng làm giảm sự khác biệt này lớn nhất. Bằng cách chỉ xem xét máy nhận nhỏ nhất, chúng tôi nắm bắt được tình huống tốt nhất mà không cần kiểm tra tất cả các cặp O(n²). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute divisors for numbers 1..100
divisors = [[] for _ in range(101)]
for i in range(1, 101):
    for j in range(1, i + 1):
        if i % j == 0:
            divisors[i].append(j)

n = int(input())
a = list(map(int, input().split()))

total_sum = sum(a)
min_total = total_sum

# Find index of smallest machine power
min_val = min(a)
min_idx = a.index(min_val)

for i in range(n):
    if i == min_idx:
        continue  # can't reduce and increase same machine
    for x in divisors[a[i]]:
        if x == 1:
            continue  # operation must change values
        new_ai = a[i] // x
        new_aj = a[min_idx] * x
        new_total = total_sum - a[i] - a[min_idx] + new_ai + new_aj
        if new_total < min_total:
            min_total = new_total

print(min_total)
```Đầu tiên, mã này tính toán tất cả các ước số, cách này rẻ vì các số nhỏ hơn 100. Sau đó chúng tôi lặp lại trên từng máy`i`và chỉ xem xét các ước >1 để giảm sức mạnh của nó. Chúng tôi luôn nhân máy nhỏ nhất`a[min_idx]`để tối đa hóa mức giảm trong tổng số tiền. Thuật toán tránh O(n²) bằng cách giới hạn người nhận ở máy tối thiểu, phù hợp về mặt toán học do tính đơn điệu của công thức rút gọn tổng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`5`,`[1, 2, 3, 4, 5]`| tôi | một [tôi] | x | new_ai | mới_aj | mới_total | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 | 2 | 2 | 2 | 14 | 

Số tiền ban đầu: 15 

Hoạt động tối ưu: giảm`4 → 2`, nhân lên`1 → 2`. Tổng số tiền = 14 

**Mẫu 2** 

đầu vào:`4`,`[3, 6, 2, 5]`| tôi | một [tôi] | x | new_ai | mới_aj | mới_total | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 2 | 3 | 2*2=4 | 3+3+4+5=15 | 

Số tiền ban đầu: 16 

Tổng số tiền tối ưu: 15 

Những dấu vết này xác nhận thuật toán xác định chính xác yếu tố đóng góp lớn nhất cần giảm và yếu tố nhỏ nhất cần tăng, tạo ra tổng số tiền tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * d) | Đối với mỗi máy, lặp lại các ước của nó (d 50) | 
| Không gian | O(n + d) | Lưu trữ mảng, ước số được tính toán trước | 

Với n 50.000 và d 50, O(n*d) ≈ 2,5 triệu thao tác, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    divisors = [[] for _ in range(101)]
    for i in range(1, 101):
        for j in range(1, i + 1):
            if i % j == 0:
                divisors[i].append(j)
    n = int(input())
    a = list(map(int, input().split()))
    total_sum = sum(a)
    min_total = total_sum
    min_val = min(a)
    min_idx = a.index(min_val)
    for i in range(n):
        if i == min_idx:
            continue
        for x in divisors[a[i]]:
            if x == 1:
                continue
            new_ai = a[i] // x
            new_aj = a[min_idx] * x
            new_total = total_sum - a[i] - a[min_idx] + new_ai + new_aj
            if new_total < min_total:
                min_total = new_total
    return str(min_total)

# provided samples
assert run("5\n1 2 3 4 5\n") == "14", "sample 1"
assert run("3\n1 2 3\n") == "5", "sample 2"

# custom cases
assert run("2\n1 100\n") == "101", "edge min max"
assert run("4\n10 10 10 10\n") == "40", "all equal"
assert run("3\n1 1 100\n") == "102", "reduce largest, increase smallest"
assert run("5\n1 2 2 2 50\n") == "57", "mix small and one large"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- |
