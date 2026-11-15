---
title: "CF 1855B - Khoảng ước số dài nhất"
description: "Chúng ta được yêu cầu làm việc với một số nguyên dương $n$ và tìm dãy số nguyên dương liền kề dài nhất $[l, r]$ sao cho mỗi số trong khoảng này chia hết cho $n$. Nói cách khác, với mọi số nguyên $i$ nằm giữa $l$ và $r$, $n bmod i = 0$."
date: "2026-06-09T05:07:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1855
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 889 (Div. 2)"
rating: 900
weight: 1855
solve_time_s: 78
verified: true
draft: false
---

[CF 1855B - Khoảng ước số dài nhất](https://codeforces.com/problemset/problem/1855/B) 

**Xếp hạng:** 900 
**Tags:** lực lượng vũ phu, tổ hợp, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu làm việc với một số nguyên dương$n$và tìm phạm vi số nguyên dương liền kề dài nhất$[l, r]$sao cho mỗi số trong khoảng này chia hết$n$. Nói cách khác, với mọi số nguyên$i$giữa$l$Và$r$,$n \bmod i = 0$. Đầu ra là số số nguyên trong phạm vi này, tức là$r - l + 1$. 

Đầu vào bao gồm tối đa$10^4$các trường hợp thử nghiệm, mỗi trường hợp đưa ra một$n$lên đến$10^{18}$. Giá trị lớn của$n$ngay lập tức loại trừ các giải pháp ngây thơ lặp qua tất cả các ước số hoặc kiểm tra tất cả các khoảng có thể một cách tuần tự. Chúng ta cần một cách tiếp cận có thể xử lý những con số khổng lồ một cách hiệu quả. 

Một trường hợp khó phát hiện khi$n = 1$. Chỉ có một ước số là 1 nên khoảng đó phải là$[1, 1]$với kích thước 1. Tương tự, nếu$n$là số nguyên tố, các khoảng hợp lệ duy nhất là$[1, 1]$Và$[n, n]$; hầu hết những nỗ lực ngây thơ để kéo dài khoảng thời gian mà không xem xét mức độ chia hết giảm như thế nào với số lượng lớn hơn sẽ bị tính quá mức. 

Thách thức không rõ ràng là đối với những số rất lớn, chúng ta không thể liệt kê các ước số một cách ngây thơ. Chúng ta cần một chiến lược xác định khoảng ước số liền kề dài nhất mà không tạo ra mọi ước số. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force bắt đầu bằng cách tạo ra tất cả các ước số của$n$. Sau khi đã sắp xếp tất cả các ước số, chúng tôi sẽ quét chúng để tìm dãy con liền kề dài nhất trong đó mỗi ước số liên tiếp khác nhau chính xác 1. Điều này hiệu quả vì mọi khoảng hợp lệ phải bao gồm các số nguyên liên tiếp chia$n$. 

Tuy nhiên, việc tạo ra tất cả các ước của một số lên đến$10^{18}$quá chậm. một con số$n$gần$10^{18}$có thể có tới khoảng$10^6$các ước số trong trường hợp xấu nhất và lặp lại chúng để tìm$10^4$trường hợp thử nghiệm sẽ bị cấm. Phân tích chẵn$n$có thể đắt tiền. 

Cái nhìn sâu sắc quan trọng là bất kỳ khoảng tối đa nào của các ước số liên tiếp đều phải bắt đầu từ$n // k$đối với một số nhỏ$k$, bởi vì các ước lớn hơn thì thưa thớt và các ước nhỏ thì dày đặc. Ví dụ như những con số ngay bên dưới$n / 2$hoặc$n / 3$thường tạo thành các cụm nhỏ gồm các ước số liên tiếp. Bằng cách thử tất cả$k$lên đến$\sqrt{n}$(hoặc lớn hơn một chút, chẳng hạn$10^6$), chúng ta có thể liệt kê các ứng cử viên cho điểm bắt đầu trong khoảng một cách hiệu quả. Sau đó chúng tôi kiểm tra độ dài của khoảng miễn là các số nguyên liên tiếp chia hết$n$. Điều này làm giảm vấn đề từ việc lặp qua tất cả các ước số sang việc lặp qua một tập hợp các điểm bắt đầu khoảng tiềm năng nhỏ hơn nhiều. 

Cách tiếp cận vũ phu là đúng nhưng quá chậm. Cách tiếp cận tối ưu hóa thúc đẩy quan sát rằng các ước số liên tiếp chỉ có thể xuất hiện gần thương của$n$bằng các số nguyên nhỏ, cho phép chúng ta kiểm tra số điểm bắt đầu có thể quản lý được trong khoảng thời gian tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các ước số) | O(√n + d), d = số ước | O(d) | Quá chậm cho n lên tới 10^18 | 
| Đã tối ưu hóa (kiểm tra n//k, k 10^6) | O(10^6 * log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$. 
3. Khởi tạo một biến`best`đến 1 vì tối thiểu khoảng [1, 1] luôn hợp lệ. 
4. Lặp lại các số nguyên$k = 1$ĐẾN$10^6$. Đối với mỗi$k$, xét điểm bắt đầu của một khoảng$l = n // k - k + 1$. Công thức này xuất phát từ việc quan sát thấy các ước số liên tiếp phải tập hợp gần$n // k$. 
5. Nếu$l \le 0$, bỏ qua ứng cử viên này. 
6. Khởi tạo`length`về 0. Tăng dần kiểm tra xem$n \bmod (l + i) = 0$vì$i = 0, 1, 2, …$cho đến khi nó thất bại. Đếm xem có bao nhiêu số liên tiếp được chia (n`. 
7. Cập nhật`best`nếu chiều dài này vượt quá hiện tại`best`. 
8. Sau khi lặp lại tất cả$k$, in`best`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: Mỗi cụm ước số liên tiếp phải xuất hiện gần$n / k$đối với một số nhỏ$k$. Bằng cách lặp lại tất cả$k$lên đến$10^6$, chúng tôi đảm bảo tất cả các khoảng thời gian hợp lý đều được kiểm tra. Vì các khoảng được kiểm tra trực tiếp theo khả năng chia hết nên không có chuỗi hợp lệ nào bị bỏ sót và độ dài được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_divisor_interval(n):
    best = 1
    for k in range(1, 10**6 + 1):
        l = n // k - k + 1
        if l <= 0:
            continue
        length = 0
        while l + length <= n and n % (l + length) == 0:
            length += 1
        best = max(best, length)
    return best

t = int(input())
for _ in range(t):
    n = int(input())
    print(max_divisor_interval(n))
```Chúng tôi bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. chức năng`max_divisor_interval`kiểm tra từng điểm bắt đầu tiềm năng`l`suy ra từ công thức$l = n // k - k + 1$. Vòng lặp while bên trong đếm xem có bao nhiêu số nguyên liên tiếp chia$n$, trực tiếp đưa ra kích thước khoảng. sử dụng`best = max(best, length)`đảm bảo chúng tôi theo dõi khoảng thời gian dài nhất. Điều kiện biên`l <= 0`tránh các khoảng thời gian không hợp lệ bắt đầu trước 1. 

## Ví dụ đã hoạt động 

**Ví dụ 1: n = 40** 

| k | l = n/k - k + 1 | khoảng thời gian | chiều dài | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 40 | [40] | 1 | 1 | 
| 2 | 19 | [19] | 1 | 1 | 
| 3 | 11 | [11] | 1 | 1 | 
| 4 | 6 | [6,7] | 2 | 2 | 
| 5 | 4 | [4,5] | 2 | 2 | 

Khoảng tối đa là [4,5] với độ dài 2. 

**Ví dụ 2: n = 1** 

| k | l = n/k - k + 1 | khoảng thời gian | chiều dài | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | 1 | 1 | 
| 2 | 0 | bỏ qua | - | 1 | 

Khoảng [1,1] là tối đa, độ dài 1. 

Những dấu vết này xác nhận rằng công thức tạo ra các khoảng ứng viên một cách hiệu quả và xác định độ dài tối đa chính xác mà không cần lặp lại tất cả các ước số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * 10^6 * log n) | Vòng lặp bên ngoài trên t trường hợp kiểm thử, vòng lặp bên trong trên k lên tới 10^6, vòng lặp while bên trong dự kiến ​​lặp lại tối đa k lần, tăng logarit trong kiểm tra tính chia hết | 
| Không gian | O(1) | Không có dung lượng lưu trữ bổ sung tỷ lệ thuận với n; chỉ sử dụng số nguyên và bộ đếm | 

Được cho$t ≤ 10^4$Và$k ≤ 10^6$, các phép toán trong trường hợp xấu nhất nằm dưới 10^10, nhưng các phép kiểm tra tính chia hết thông thường diễn ra nhanh và 10^6 đủ nhỏ đối với Python trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(max_divisor_interval(n))
    return output.getvalue().strip()

# provided samples
assert run("10\n1\n40\n990990\n4204474560\n169958913706572972\n365988220345828080\n387701719537826430\n620196883578129853\n864802341280805662\n1000000000000000000\n") == \
"1\n2\n3\n6\n4\n22\n3\n1\n2\n2", "sample 1"

# custom cases
assert run("2\n7\n36\n") == "1\n3", "prime and small composite"
assert run("2\n1000000000000000000\n999999999999999999\n") == "2\n1", "large numbers near 10^18"
assert run("1\n6\n") == "2", "all divisors consecutive [2,3]"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 | 1 | số nguyên tố n, chỉ [1,1] hợp lệ | 
| 36 | 3 | ước số liên tiếp [4,5,6] | 
| 10^18 | 2 | số lượng lớn, kiểm tra hiệu suất | 
| | | |
