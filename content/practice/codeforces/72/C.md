---
title: "CF 72C - Những Con Số Cực Đẹp"
description: "Chúng ta được yêu cầu xác định xem số nguyên dương $x$ có cực kỳ đẹp hay không. Theo định nghĩa của bài toán, một số cực kỳ đẹp nếu nó có cùng số ước chẵn và số ước lẻ. Đầu vào là một số nguyên $x$ trong khoảng từ 1 đến 1000."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 72
codeforces_index: "C"
codeforces_contest_name: "Unknown Language Round 2"
rating: 1200
weight: 72
solve_time_s: 86
verified: true
draft: false
---

[CF 72C - Những con số cực kỳ đẹp](https://codeforces.com/problemset/problem/72/C) 

**Đánh giá:** 1200 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một số nguyên dương có$x$là cực kỳ tốt đẹp. Theo định nghĩa của bài toán, một số cực kỳ đẹp nếu nó có cùng số ước chẵn và số ước lẻ. Đầu vào là một số nguyên duy nhất$x$trong khoảng từ 1 đến 1000. Đầu ra chỉ đơn giản là "có" nếu số đó cực kỳ đẹp và "không" nếu ngược lại. 

Nhìn vào những hạn chế,$x \le 1000$là nhỏ. Điều này có nghĩa là chúng ta có thể xem xét việc đếm các ước số trực tiếp cho từng số mà không gặp phải vấn đề về hiệu suất. Mỗi số$x$có nhiều nhất$\sqrt{x}$các ước số để kiểm tra hệ số hóa, và$1000 \times \sqrt{1000}$là khoảng 32.000 thao tác, tầm thường trong giới hạn thời gian 2 giây. Vì vậy, hiệu quả không phải là mối quan tâm lớn ở đây, nhưng một cái nhìn sâu sắc thông minh sẽ đơn giản hóa giải pháp một cách đáng kể. 

Trường hợp cạnh phát sinh với 1. Ước duy nhất của nó là chính nó, là số lẻ. Vì thế, 1 không phải là điều gì quá đẹp đẽ. Một kịch bản tinh tế khác là lũy thừa bất kỳ của 2: các ước số đều là 1, 2, 4, v.v., và việc đếm số chẵn và số lẻ đòi hỏi phải chú ý cẩn thận. Ví dụ: 2 có các ước {1, 2}-một lẻ, một chẵn-vì vậy nó đủ tiêu chuẩn. Bất kỳ số lẻ nào lớn hơn 1 sẽ có nhiều ước lẻ hơn số chẵn, vì 1 luôn là số lẻ và nhân với một số lẻ sẽ cho ra một ước số lẻ khác. 

Việc hiểu những mô hình này sẽ dẫn đến sự đơn giản hóa: một số chỉ có thể cực kỳ đẹp nếu nó là lũy thừa của 2, bởi vì chỉ khi đó các ước chẵn mới có thể “cân bằng” ước số lẻ duy nhất 1. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là lặp qua tất cả các số từ 1 đến$x$, kiểm tra xem mỗi số có phải là ước của$x$và đếm xem có bao nhiêu số chẵn và bao nhiêu số lẻ. Phương pháp này đơn giản và chính xác. Vì$x=1000$, số phép tính trong trường hợp xấu nhất gần bằng số lượng ước số, trong thực tế là khoảng 32 đối với các số dưới 1000. Mặc dù khả thi nhưng cách tiếp cận này còn hiệu quả hơn: chúng ta không cần danh sách chính xác các ước số, chỉ cần mẫu số chẵn và tỷ lệ cược. 

Cái nhìn sâu sắc quan trọng là yếu tố$x$BẰNG$2^k \cdot m$Ở đâu$m$thật kỳ quặc. Các ước số lẻ hoàn toàn đến từ$m$(bao gồm 1), trong khi các ước số chẵn có được từ lũy thừa của 2 với ước số là$m$. Để số ước chẵn bằng số ước lẻ$m$phải là 1. Điều này làm giảm vấn đề kiểm tra xem$x$là lũy thừa của 2, một phép toán đơn giản sử dụng phép chia lặp lại cho 2. 

Quan sát này biến một bài toán đếm lực lượng thành một bài kiểm tra đơn giản bằng cách sử dụng các phép chia hoặc bitwise. Sự tinh tế ở đây là thừa nhận tính chất cấu trúc của các ước số: chỉ riêng lũy ​​thừa của 2 cũng có thể thỏa mãn điều kiện "cực kỳ tốt". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(√x) | O(1) | Được chấp nhận cho vấn đề này, nhưng không cần thiết | 
| Kiểm tra sức mạnh của 2 | O(logx) | O(1) | Tối ưu, thanh lịch, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$x$. Đây là con số chúng ta cần kiểm tra. 
2. Khởi tạo một biến tạm thời$y = x$. Điều này tránh sửa đổi đầu vào. 
3. Chia nhiều lần$y$bằng 2 khi nó là số chẵn. Mỗi phần loại bỏ một thừa số của 2. 
4. Sau vòng lặp, kiểm tra xem$y$bằng 1. Nếu đúng như vậy,$x$là sức mạnh thuần túy của 2, và nó cực kỳ tốt. 
5. Nếu không,$y$chứa một thừa số lẻ lớn hơn 1, nghĩa là số ước lẻ lớn hơn số ước chẵn, vì vậy$x$không phải là đặc biệt tốt đẹp. 
6. In chữ “có” nếu$x$là cực kỳ tốt đẹp, và "không" nếu không. 

Thuật toán hoạt động vì lũy thừa của 2 có chính xác một ước số lẻ là 1 và tất cả các ước số khác đều là số chẵn, tạo thành một kết quả khớp 1-1 với số lũy thừa. Nếu có bất kỳ thừa số lẻ nào lớn hơn 1 thì số ước lẻ nhiều hơn số ước chẵn, vi phạm điều kiện. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input())
y = x

while y % 2 == 0:
    y //= 2

if y == 1:
    print("yes")
else:
    print("no")
```Chúng tôi sử dụng`y`như một bản sao làm việc để tránh sửa đổi`x`. các`while`vòng lặp loại bỏ tất cả lũy thừa của 2 từ`y`. Nếu như`y`trở thành 1, không còn thừa số lẻ, khẳng định$x$là lũy thừa của 2. Sử dụng phép chia số nguyên`//`đảm bảo rằng chúng ta ở trong không gian nguyên, tránh các lỗi dấu phẩy động tinh vi. 

## Ví dụ đã hoạt động 

**Ví dụ 1: x = 2** 

| Bước | y | y% 2 == 0? | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | Đúng | y //= 2 → y = 1 | 
| 2 | 1 | Sai | Thoát khỏi vòng lặp | 

Kiểm tra`y == 1`→ Đúng → Đầu ra: "có". Điều này chứng tỏ rằng 2, có ước số {1,2}, cực kỳ đẹp. 

**Ví dụ 2: x = 6** 

| Bước | y | y% 2 == 0? | Hành động | 
| --- | --- | --- | --- | 
| 1 | 6 | Đúng | y //= 2 → y = 3 | 
| 2 | 3 | Sai | Thoát khỏi vòng lặp | 

Kiểm tra`y == 1`→ Sai → Đầu ra: "không". 6 có các ước {1,2,3,6}; ước số lẻ {1,3} = 2, ước số chẵn {2,6} = 2. Đợi đã, có vẻ bằng nhau. Nhưng các ước số: lẻ 2, chẵn 2. Thực ra, đây là "có". Hmm, chúng ta hãy xem xét cẩn thận: 

Ước của 6: {1,2,3,6}. Lẻ: {1,3} → 2. Chẵn: {2,6} → 2. Số chẵn = số lẻ = 2. 

À, đây là một ngoại lệ: những số như 6 không phải là lũy thừa của 2 nhưng vẫn có ước số chẵn và số lẻ bằng nhau. Vì vậy khẳng định trước đây của tôi về lũy thừa 2 cần được sàng lọc lại. 

Hãy tinh chỉnh quan sát: 

- Hãy để$x = 2^a \cdot m$,$m$số lẻ. 
- Ước lẻ có được từ phép chia số lẻ$m$. Đếm:$\tau(m)$, số ước của$m$. 
- Các ước chẵn có được từ phép nhân 2^k (k ≥ 1) với các ước của$m$. Đếm: a * τ(m)? Trên thực tế, nó tinh tế hơn. 

Cách tiếp cận tốt hơn: Đếm tất cả các ước số, chia theo số chẵn lẻ. Đối với nhỏ$x \le 1000$, việc đếm vũ phu là an toàn. 

Thuật toán mới tối ưu cho an toàn: lặp i từ 1 đến sqrt(x), kiểm tra xem i có chia x hay không. Đếm chính xác các ước số lẻ và chẵn. Trả về "có" nếu số lượng bằng nhau. Điều này tránh việc phân loại sai. 

## Giải pháp Python (Brute-Force, Đúng cho mọi trường hợp)```python
import sys
input = sys.stdin.readline

x = int(input())
odd = 0
even = 0

for i in range(1, x + 1):
    if x % i == 0:
        if i % 2 == 0:
            even += 1
        else:
            odd += 1

print("yes" if odd == even else "no")
```Chúng tôi kiểm tra tất cả các ước số một cách rõ ràng. Điều này tránh các số bị thiếu như 6 trong đó các ước số chẵn và lẻ cân bằng nhưng số đó không phải là lũy thừa của 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x) | Lặp qua 1..x để tìm ước số | 
| Không gian | O(1) | Chỉ cần hai quầy | 

Được cho$x \le 1000$, điều này nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    odd = 0
    even = 0
    x = int(input())
    for i in range(1, x + 1):
        if x % i == 0:
            if i % 2 == 0:
                even += 1
            else:
                odd += 1
    return "yes" if odd == even else "no"

# provided sample
assert run("2\n") == "yes", "sample 1"

# custom cases
assert run("1\n") == "no", "minimum input, only odd divisor"
assert run("6\n") == "yes", "balanced divisors"
assert run("8\n") == "no", "power of 2 > 2, odd divisors = 1, evens = 3"
assert run("9\n") == "no", "odd number, all divisors odd"
```
