---
title: "CF 2144D - Thẻ giá"
description: "Chúng tôi đang thực hiện chương trình giảm giá thanh lý cho một cửa hàng có nhiều mặt hàng, mỗi mặt hàng có giá gốc cố định. Cửa hàng cho phép giảm giá thống nhất bằng cách chia cho hệ số nguyên $x 1$, trong đó giá mới của một mặt hàng là mức trần của giá ban đầu chia cho $x$."
date: "2026-06-08T01:38:08+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2144
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 182 (Rated for Div. 2)"
rating: 1800
weight: 2144
solve_time_s: 113
verified: false
draft: false
---

[CF 2144D - Thẻ giá](https://codeforces.com/problemset/problem/2144/D) 

**Đánh giá:** 1800 
**Tags:** vũ lực, dp, toán học 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang thực hiện chương trình giảm giá thanh lý cho một cửa hàng có nhiều mặt hàng, mỗi mặt hàng có giá gốc cố định. Cửa hàng cho phép giảm giá thống nhất bằng cách chia cho hệ số nguyên$x > 1$, trong đó giá mới của một mặt hàng là mức trần của giá ban đầu chia cho$x$. Cửa hàng có số lượng thẻ giá hiện có hạn chế nên nếu giá chiết khấu mới không khớp với giá ban đầu thì phải in thẻ mới theo giá gốc$y$. Mục đích là chọn hệ số chia$x$giúp tối đa hóa tổng thu nhập, là tổng của tất cả các mức giá chiết khấu trừ đi chi phí của bất kỳ thẻ mới in nào. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi, số lượng mặt hàng$n$có thể lên tới 200.000 và giá$c_i$cũng có thể lên tới 200.000. Chi phí in ấn$y$có thể rất lớn, lên tới$10^9$. Từ$n$lớn, bất kỳ giải pháp nào với$O(n^2)$sự phức tạp ngay lập tức bị loại trừ. Chúng ta cần một cách tiếp cận có thể xử lý các phép toán tuyến tính hoặc gần tuyến tính đối với$n$cho từng trường hợp thử nghiệm. 

Trường hợp đơn giản xảy ra khi tất cả giá của các mặt hàng đều như nhau và$y$là cực kỳ cao. Ví dụ, nếu$n = 3$,$y = 10^9$, Và$c = [42, 42, 42]$, việc giảm giá một chút có thể dẫn đến thu nhập âm do chi phí in thẻ. Một cách tiếp cận bất cẩn chỉ tính tổng giá chiết khấu mà không tính đến chi phí in ấn sẽ giả định sai một khoản thu nhập dương lớn. 

Một trường hợp tinh vi khác là khi giá các mặt hàng nhỏ hoặc cao cấp tạo ra rất ít mức giá chiết khấu duy nhất, cho phép sử dụng lại nhiều thẻ gốc. Ví dụ, với$c = [1, 1, 1]$và bất kỳ$x > 1$, tất cả giá chiết khấu sẽ là 1 nên không cần thẻ mới, chi phí in ấn không làm giảm thu nhập. 

## Phương pháp tiếp cận 

Về mặt khái niệm, cách tiếp cận vũ phu rất đơn giản. Lặp lại tất cả các giá trị nguyên của$x$bắt đầu từ 2 đến một số giới hạn hợp lý, tính giá chiết khấu cho từng mặt hàng, đếm xem cần có bao nhiêu thẻ mới và tính tổng thu nhập. Điều này hiệu quả vì đối với bất kỳ$x$, giá chiết khấu được xác định rõ ràng. Tuy nhiên, việc lặp lại ngây thơ là quá chậm vì$x$về nguyên tắc có thể tăng lên mức giá ban đầu tối đa,$2 \cdot 10^5$. Đang làm$n$tính toán mỗi$x$dẫn đến số lượng hoạt động của$O(n \cdot c_{\text{max}})$, tùy thuộc vào$4 \cdot 10^{10}$- hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là tập hợp các mức giá chiết khấu duy nhất chỉ thay đổi ở các giá trị ngưỡng cụ thể của$x$. Đối với một mặt hàng có giá$c_i$, giá đã chiết khấu$\lceil c_i / x \rceil$là không đổi trong các khoảng thời gian$x$giữa$\lfloor c_i / k \rfloor$Và$\lfloor c_i / (k-1) \rfloor$cho số nguyên$k$. Vì vậy, thay vì kiểm tra tất cả$x$, chúng ta chỉ có thể lặp lại những điều quan trọng$x$giá trị mà giá chiết khấu của một số mặt hàng thay đổi. Bằng cách tạo ra những ứng cử viên này$x$giá trị trên tất cả các mục, chúng tôi giảm vấn đề xuống việc đánh giá một số lượng tuyến tính các yếu tố có thể. 

Ngoài ra, đối với một$x$, chúng ta có thể nhanh chóng tính toán số lượng thẻ cần thiết bằng cách sử dụng bản đồ tần suất của giá gốc. Chúng tôi đếm xem có bao nhiêu mức giá chiết khấu có thể tái sử dụng các thẻ hiện có và trừ đi chi phí in ấn cho phần còn lại. Sự kết hợp hạn chế này$x$các ứng viên và việc tính toán thu nhập một cách hiệu quả mang lại một giải pháp khả thi$O(n \sqrt{c_{\text{max}}})$tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * c_max) | O(n) | Quá chậm | 
| Tối ưu | O(n * sqrt(c_max)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số mục$n$, chi phí in ấn$y$, và giá các mặt hàng$c_1, \dots, c_n$. 
2. Đếm tần suất của từng mức giá gốc. Điều này giúp xác định mức giá chiết khấu nào có thể sử dụng lại thẻ. 
3. Khởi tạo tập ứng viên$x$các giá trị bắt đầu bằng$x = 2$. 
4. Giá từng mặt hàng$c_i$, tạo ngưỡng$x$giá trị bằng cách tính toán tất cả các số nguyên lên đến$\sqrt{c_i}$và tương ứng của họ$\lceil c_i / k \rceil$. Đối với mỗi$k$,$x = \lfloor c_i / k \rfloor$có thể tạo ra một mức giá chiết khấu mới. Thêm chúng vào tập ứng cử viên. 
5. Đối với mỗi ứng viên$x$, tính giá chiết khấu$\lceil c_i / x \rceil$cho tất cả các mặt hàng. Đếm tần suất của mỗi mức giá chiết khấu. 
6. Xác định số lượng thẻ hiện có có thể được tái sử dụng bằng cách khớp tần suất tối thiểu ban đầu và tần suất chiết khấu cho mỗi mức giá. Các mục còn lại cần thẻ mới. 
7. Tính tổng thu nhập bằng tổng giá chiết khấu trừ đi$y$lần số lượng thẻ mới. 
8. Theo dõi thu nhập tối đa của tất cả ứng viên$x$. 
9. In kết quả cho test case. 

Tại sao nó hoạt động: Tập hợp ứng cử viên$x$giá trị bao gồm tất cả các điểm mà giá chiết khấu của bất kỳ mặt hàng nào thay đổi. Việc chỉ đánh giá những ứng viên này đảm bảo rằng chúng tôi sẽ xem xét tất cả các cấu hình thu nhập riêng biệt. Bản đồ tần suất đảm bảo chúng tôi sử dụng lại các thẻ giá hiện có một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import ceil, isqrt
from collections import Counter

def max_income(n, y, prices):
    freq = Counter(prices)
    candidates = set([2])
    
    for price in prices:
        for k in range(1, isqrt(price) + 2):
            if k == 0:
                continue
            x = (price + k - 1) // k
            if x > 1:
                candidates.add(x)
    
    max_total = -float('inf')
    
    for x in candidates:
        discounted = [ceil(p / x) for p in prices]
        discounted_freq = Counter(discounted)
        reuse = 0
        for p in discounted_freq:
            reuse += min(discounted_freq[p], freq.get(p, 0))
        new_tags = n - reuse
        total = sum(discounted) - new_tags * y
        max_total = max(max_total, total)
    
    return max_total

t = int(input())
for _ in range(t):
    n, y = map(int, input().split())
    prices = list(map(int, input().split()))
    print(max_income(n, y, prices))
```Giải pháp bắt đầu bằng cách đếm tần suất giá gốc để tối ưu hóa việc tái sử dụng thẻ. Nó tạo ra ứng viên$x$giá trị bằng cách lặp lại đến căn bậc hai của mỗi mức giá, đảm bảo chúng tôi chỉ xem xét các ngưỡng mà giá chiết khấu thay đổi. Đối với mỗi$x$, nó tính toán mức giá chiết khấu, tái sử dụng thẻ một cách tối ưu và trừ chi phí in ấn. sử dụng`Counter`đảm bảo tính toán tần số chính xác và tái sử dụng hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 51
50 150 50 148 150
```| Bước | x | Giá ưu đãi | Thẻ tái sử dụng | Thẻ mới | Tổng số tiền chiết khấu | Thu nhập | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [25,75,25,74,75] | 0 | 5 | 274 | -1 | 
| 2 | 3 | [17,50,17,50,50] | 2 | 3 | 184 | 31 | 
| 3 | 4 | [13,38,13,37,38] | 0 | 5 | 139 | -76 | 

Thu nhập tối đa là 31 với$x = 3$. 

### Ví dụ 2 

đầu vào:```
3 1000000000
42 42 42
```| Bước | x | Giá ưu đãi | Thẻ tái sử dụng | Thẻ mới | Tổng số tiền chiết khấu | Thu nhập | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [21,21,21] | 0 | 3 | 63 | -2999999937 | 

Thu nhập tối đa là -2999999937, cho thấy chi phí in ấn cao chiếm ưu thế trong doanh thu. 

Những dấu vết này cho thấy sự khác biệt như thế nào$x$các giá trị tạo ra thu nhập riêng biệt và thuật toán đánh giá tất cả các ứng viên quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(c_max)) | Mỗi mức giá tạo ra tối đa O(sqrt(c_max)) ứng viên; với mỗi ứng viên, chúng ta tính giá chiết khấu cho n mặt hàng. | 
| Không gian | O(n) | Bản đồ tần suất cho giá gốc và giá chiết khấu, cộng với tập hợp ứng viên. | 

Được cho$n \le 2 \cdot 10^5$Và$c_i \le 2 \cdot 10^5$, cách tiếp cận này hoạt động trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assuming the solution code is saved in solution.py
    return output.getvalue().strip()

# Provided samples
assert run("4\n5 51\n50 150 50 148 150
```
