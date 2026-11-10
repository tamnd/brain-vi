---
title: "CF 1519D - Tổng sản phẩm tối đa"
description: "Chúng ta có hai mảng số nguyên a và b, cả hai đều có cùng độ dài n. Nhiệm vụ là tối đa hóa tổng của các tích theo phần tử, $sum ai cdot bi$, bằng cách đảo ngược tối đa một mảng con liền kề của a."
date: "2026-06-10T18:12:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1519
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 108 (Rated for Div. 2)"
rating: 1600
weight: 1519
solve_time_s: 124
verified: true
draft: false
---

[CF 1519D - Tổng sản phẩm tối đa](https://codeforces.com/problemset/problem/1519/D) 

**Đánh giá:** 1600 
**Tags:** vũ lực, dp, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên,`a`Và`b`, cả hai đều có cùng chiều dài`n`. The task is to maximize the sum of element-wise products,$\sum a_i \cdot b_i$, bằng cách đảo ngược tối đa một mảng con liền kề của`a`. Đảo ngược một mảng con có nghĩa là chọn một đoạn của`a`và đảo ngược thứ tự các phần tử của nó, trong khi`b`vẫn không thay đổi. 

Những ràng buộc cho chúng ta`n`up to 5000. This means an O(n^3) solution is impractical, as it could require around 125 billion operations. An O(n^2) solution may be feasible with careful implementation. Giá trị trong`a`Và`b`có thể lên tới 10^7, vì vậy chúng ta phải sử dụng kiểu dữ liệu có khả năng lưu trữ tổng lên đến khoảng`5000 * 10^7 * 10^7 = 5 * 10^18`. của Python`int`xử lý việc này mà không bị tràn. 

Các trường hợp cạnh bao gồm các mảng nhỏ có độ dài 1 hoặc 2, trong đó việc đảo ngược có thể không cải thiện được tổng. Một trường hợp tinh tế khác xảy ra khi mảng con tối ưu là toàn bộ mảng hoặc khi không cần đảo ngược vì mảng đã được căn chỉnh với`b`cho số tiền tối đa. Ví dụ,`a = [1, 2, 3]`,`b = [3, 2, 1]`. Đảo ngược toàn bộ`a`cho`[3, 2, 1]`và số tiền tối đa`3*3 + 2*2 + 1*1 = 14`. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: lặp qua tất cả các mảng con có thể`(l, r)`của`a`, đảo ngược mảng con, tính tổng$\sum a_i \cdot b_i$và theo dõi mức tối đa. Điều này hiệu quả vì mọi sự đảo chiều hợp lệ đều được xem xét. Tuy nhiên, điều này đòi hỏi các phép toán O(n^3): O(n^2) cho tất cả các mảng con và O(n) để tính từng tổng. Với`n`lên tới 5000, điều này là quá chậm. 

Thông tin chi tiết quan trọng là việc đảo ngược một mảng con chỉ làm thay đổi phần đóng góp của mảng con đó vào tổng số tiền. Các bộ phận của`a`bên ngoài đoạn đảo ngược vẫn thẳng hàng với`b`. Do đó, chúng ta có thể tính toán trước tổng số tiền mà không cần đảo ngược và chỉ tập trung vào "lợi ích" từ việc đảo ngược các mảng con. 

Chúng ta có thể khai thác tính đối xứng: xem xét một mảng con từ`l`ĐẾN`r`. Nếu chúng ta đảo ngược nó, sự đóng góp của các yếu tố`a[l+i] * b[l+i]`thay đổi thành`a[r-i] * b[l+i]`. Bằng cách mở rộng từ tâm của mảng con ra ngoài, chúng ta có thể tính mức tăng dần theo thời gian O(n^2). Kỹ thuật mở rộng động hoặc hai con trỏ này làm giảm độ phức tạp từ O(n^3) xuống O(n^2), điều này khả thi đối với`n=5000`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng sản phẩm ban đầu mà không đảo ngược. Gọi đây`base_sum`. 
2. Khởi tạo`max_gain = 0`để theo dõi mức tăng tối đa từ việc đảo ngược bất kỳ mảng con nào. 
3. Xem xét mọi tâm có thể có của một mảng con. Đối với các mảng con có độ dài lẻ, một phần tử có thể là trung tâm; đối với các mảng con có độ dài chẵn, tâm nằm giữa hai phần tử. 
4. Mở rộng đối xứng xung quanh trung tâm. Cho phép`l`Và`r`là các chỉ số được hoán đổi trong bản mở rộng này. Tính mức tăng`gain = a[r] * b[l] + a[l] * b[r] - a[l] * b[l] - a[r] * b[r]`. Đây là tổng số tiền tăng ròng nếu chúng ta đảo ngược cặp này. 
5. Tích lũy`gain`khi chúng tôi mở rộng ra bên ngoài và cập nhật`max_gain`bất cứ khi nào lợi ích tích lũy vượt quá nó. 
6. Sau khi kiểm tra tất cả các trung tâm có thể, câu trả lời là`base_sum + max_gain`. 

Tại sao nó hoạt động: đảo ngược một mảng con chỉ ảnh hưởng đến các vị trí bên trong nó. By expanding from the center outward and calculating the incremental gain at each step, we ensure every possible subarray is considered exactly once. Vì mức tăng được tính tương ứng với tổng ban đầu nên tổng bên ngoài mảng con không thay đổi, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

base_sum = sum(a[i] * b[i] for i in range(n))
max_gain = 0

# Odd-length centers
for center in range(n):
    gain = 0
    l, r = center - 1, center + 1
    while l >= 0 and r < n:
        gain += a[r] * b[l] + a[l] * b[r] - a[l] * b[l] - a[r] * b[r]
        if gain > max_gain:
            max_gain = gain
        l -= 1
        r += 1

# Even-length centers
for center in range(n - 1):
    gain = 0
    l, r = center, center + 1
    while l >= 0 and r < n:
        gain += a[r] * b[l] + a[l] * b[r] - a[l] * b[l] - a[r] * b[r]
        if gain > max_gain:
            max_gain = gain
        l -= 1
        r += 1

print(base_sum + max_gain)
```Mã đầu tiên tính tổng mà không có bất kỳ sự đảo ngược nào. Sau đó, nó kiểm tra tất cả các mảng con có độ dài lẻ và độ dài chẵn bằng cách mở rộng từ tâm. Công thức tăng dần đảm bảo chúng tôi chỉ tính đến các khoản đóng góp hoán đổi, tránh tính toán lại tổng từ đầu. Xử lý ranh giới cho`l`Và`r`đảm bảo không có lỗi chỉ mục. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`a = [2,3,2,1,3], b = [1,3,2,4,2]`| tôi | r | a[l]*b[l]+a[r]*b[r] | a[r]*b[l]+a[l]*b[r] | đạt được | tăng_tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 | 1_4+3_2=10 | 3_4+1_2=14 | 4 | 4 | 
| 2 | 5 | ... | ... | ... | 4 | 

Mảng con tối ưu để đảo ngược là`[1, 3]`(chỉ số 3 đến 4), cho mức tăng là 4. Tổng cơ sở là 25, do đó tổng tối đa là 29. 

**Mẫu 2:** 

đầu vào:`a = [1,2,3], b = [3,2,1]`| tôi | r | đạt được | tăng_tối đa | 
| --- | --- | --- | --- | 
| 0 | 2 | (3_1 +1_3)-(1_3 +3_1)=0 | 0 | 

Việc đảo ngược toàn bộ mảng sẽ không mang lại lợi ích bổ sung nào trong bản mở rộng này, tổng cơ sở là 10, tổng tối đa vẫn là 10. 

Điều này chứng tỏ tính toán khuếch đại xác định chính xác khi không có sự đảo ngược nào cải thiện tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi trung tâm có thể (O(n)), chúng tôi mở rộng ra bên ngoài tối đa n/2 lần | 
| Không gian | O(1) | Chỉ có một vài biến số để theo dõi số tiền và lợi nhuận; không có mảng bổ sung | 

Các thao tác O(n^2) với n=5000 là khoảng 25 triệu, có thể chấp nhận được trong giới hạn 2 giây trong Python. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    base_sum = sum(a[i] * b[i] for i in range(n))
    max_gain = 0

    for center in range(n):
        gain = 0
        l, r = center - 1, center + 1
        while l >= 0 and r < n:
            gain += a[r] * b[l] + a[l] * b[r] - a[l] * b[l] - a[r] * b[r]
            if gain > max_gain:
                max_gain = gain
            l -= 1
            r += 1

    for center in range(n - 1):
        gain = 0
        l, r = center, center + 1
        while l >= 0 and r < n:
            gain += a[r] * b[l] + a[l] * b[r] - a[l] * b[l] - a[r] * b[r]
            if gain > max_gain:
                max_gain = gain
            l -= 1
            r += 1

    return str(base_sum + max_gain)

# provided samples
assert run("5\n2 3 2 1 3\n1 3 2 4 2\n") == "29", "sample 1"
assert run("2\n13 37\n2 4\n") == "174", "sample 2"

# custom cases
assert run("3\n1 2 3\n3
```
