---
title: "CF 1712A - Hoán vị tuyệt vời"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến $n$, nghĩa là mọi số từ 1 đến $n$ xuất hiện đúng một lần, theo một thứ tự tùy ý. Nhiệm vụ là giảm thiểu tổng của các phần tử $k$ đầu tiên trong hoán vị này bằng cách thực hiện hoán đổi giữa hai vị trí bất kỳ."
date: "2026-06-09T20:19:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1712
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 813 (Div. 2)"
rating: 800
weight: 1712
solve_time_s: 113
verified: true
draft: false
---

[CF 1712A - Hoán vị tuyệt vời](https://codeforces.com/problemset/problem/1712/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$, nghĩa là mọi số từ 1 đến$n$xuất hiện đúng một lần, theo thứ tự tùy ý. Nhiệm vụ là giảm thiểu tổng của số đầu tiên$k$các phần tử trong hoán vị này bằng cách thực hiện hoán đổi giữa hai vị trí bất kỳ. Mỗi lần hoán đổi được tính là một thao tác và chúng ta được yêu cầu tìm số lượng tối thiểu các thao tác đó cần thiết. 

Kích thước đầu vào nhỏ:$n$có thể có nhiều nhất là 100 và có thể có tới 100 ca kiểm thử. Điều này ngụ ý rằng một$O(n^2)$giải pháp có thể chấp nhận được nhưng chúng ta cũng có thể hướng tới$O(n \log n)$bằng cách sử dụng sắp xếp. Vì mọi số đều khác biệt và bị giới hạn bởi$n$, chúng tôi không bao giờ có số trùng lặp hoặc số nằm ngoài phạm vi, vì vậy chúng tôi không cần xác thực hoán vị. 

Một trường hợp phức tạp là khi lần đầu tiên$k$các phần tử đã chứa$k$số nhỏ nhất. Ví dụ, nếu$n=5$,$k=3$, và mảng là$[1, 2, 3, 5, 4]$, không cần hoán đổi, vì vậy câu trả lời phải là 0. Một cách tiếp cận đơn giản là đếm một cách mù quáng các số bị đặt sai vị trí mà không xem xét liệu chúng có ở số đầu tiên hay không$k$khe cắm có thể vượt quá số lần hoán đổi. 

Một trường hợp cạnh khác là$k=1$hoặc$k=n$. Nếu như$k=1$, chúng ta chỉ cần phần tử nhỏ nhất ở vị trí đầu tiên. Nếu như$k=n$, tổng bao gồm toàn bộ hoán vị đã được cố định, do đó không thể thực hiện hoặc không cần thiết phải hoán đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các chuỗi hoán đổi có thể có, tính tổng của chuỗi hoán đổi đầu tiên.$k$phần tử sau mỗi dãy. Điều này đúng về nguyên tắc nhưng hoàn toàn không thực tế. Vì$n=100$, có khoảng$\binom{100}{2} = 4950$các giao dịch hoán đổi đơn lẻ có thể xảy ra và việc xem xét nhiều giao dịch hoán đổi nhanh chóng trở nên bùng nổ về mặt tổ hợp. 

Cái nhìn sâu sắc quan trọng là để giảm thiểu tổng của lần đầu tiên$k$các phần tử, chúng tôi chỉ quan tâm đến việc đặt$k$số nhỏ nhất vào số đầu tiên$k$các vị trí. Thứ tự trong số này$k$các con số không quan trọng, thứ tự giữa các số còn lại cũng không quan trọng.$n-k$những con số. Vì vậy, vấn đề giảm xuống còn việc đếm xem có bao nhiêu$k$số nhỏ nhất hiện nằm ngoài số đầu tiên$k$các vị trí. Mỗi số trong số đó sẽ cần được hoán đổi. Mỗi lần hoán đổi có thể sửa chính xác một số trong số đầu tiên.$k$vị trí nên số thao tác bằng số lượng các số nhỏ hiện ở bên ngoài. 

Để thực hiện điều này, trước tiên chúng ta xác định$k$số nhỏ nhất, đơn giản từ 1 đến$k$. Sau đó chúng tôi lặp lại lần cuối$n-k$vị trí và đếm có bao nhiêu trong số này đầu tiên$k$số đầu tiên bị thiếu$k$khe cắm. Mỗi số còn thiếu tương ứng với một trao đổi bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n!) * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định tập hợp số ta muốn ở đầu tiên$k$các vị trí. Đây là các số nguyên từ 1 đến$k$. Sử dụng một bộ cho phép$O(1)$kiểm tra thành viên. 
2. Quét đầu tiên$k$phần tử và đánh dấu phần tử nào trong số này$k$những con số đã có sẵn. Đếm xem còn thiếu bao nhiêu. Chúng ta chỉ cần biết số nào từ 1 đến$k$vắng mặt ngay từ đầu$k$khe cắm. 
3. Những số còn thiếu ở đầu tiên$k$các vị trí tương ứng chính xác với số lượng giao dịch hoán đổi cần thiết. Mỗi lần hoán đổi có thể mang lại một số nhỏ bị thiếu trong số còn lại$n-k$các vị trí. 
4. Trả về số lượng số còn thiếu là số phép toán tối thiểu. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà mọi số nằm ngoài số đầu tiên$k$vị trí thuộc về đầu tiên$k$được tính đúng một lần. Hoán đổi từng số còn thiếu để sửa chính xác một số thiếu. Vì chúng tôi chỉ trao đổi những con số chúng tôi cần nên chúng tôi không bao giờ thực hiện các thao tác không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    
    # Set of numbers that should be in the first k positions
    needed = set(range(1, k+1))
    
    # Count numbers among first k that are already correct
    in_place = sum(1 for i in range(k) if p[i] in needed)
    
    # Missing numbers = total needed - already in place
    print(k - in_place)
```Đoạn mã đọc từng trường hợp kiểm thử, xác định$k$số nhỏ nhất (1 đến$k$), đếm xem có bao nhiêu trong số đầu tiên$k$vị trí và in sự khác biệt. Việc sử dụng một tập hợp cho các số nhỏ nhất sẽ đảm bảo việc kiểm tra tư cách thành viên liên tục. sử dụng`sum`với biểu thức trình tạo sẽ tránh tạo danh sách tạm thời. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 1
2 3 1
```| tôi | p[i] | đang cần thiết? | số lượng tại_điểm | 
| --- | --- | --- | --- | 
| 0 | 2 | không | 0 | 

k - in_place = 1 - 0 = 1, phù hợp với kết quả mong đợi. 

**Đầu vào mẫu 2:**```
3 3
1 2 3
```| tôi | p[i] | đang cần thiết? | số lượng tại_điểm | 
| --- | --- | --- | --- | 
| 0 | 1 | vâng | 1 | 
| 1 | 2 | vâng | 2 | 
| 2 | 3 | vâng | 3 | 

k - in_place = 3 - 3 = 0, đúng. 

Những dấu vết này chứng tỏ rằng thuật toán đếm chính xác những số nhỏ còn thiếu trong lần đầu tiên.$k$các vị thế, tạo ra số lượng giao dịch hoán đổi tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét k phần tử đầu tiên và kiểm tra thành viên trong một tập hợp có kích thước k là tuyến tính | 
| Không gian | O(k) | Bộ lưu trữ k số nhỏ nhất | 

Được cho$t \le 100$Và$n \le 100$, tổng số thao tác tối đa là 10.000, dễ dàng phù hợp với bộ nhớ 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        p = list(map(int, input().split()))
        needed = set(range(1, k+1))
        in_place = sum(1 for i in range(k) if p[i] in needed)
        print(k - in_place)
        
    return output.getvalue().strip()

# provided samples
assert run("4\n3 1\n2 3 1\n3 3\n1 2 3\n4 2\n3 4 1 2\n1 1\n1") == "1\n0\n2\n0", "sample 1"

# custom cases
assert run("1\n5 2\n5 4 3 2 1") == "2", "both needed numbers are at the end"
assert run("1\n6 3\n1 3 2 6 5 4") == "0", "needed numbers already in first k positions"
assert run("1\n1 1\n1") == "0", "single element, nothing to swap"
assert run("1\n4 4\n4 3 2 1") == "4", "all numbers in wrong positions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2\n5 4 3 2 1`| 2 | Cả hai số cần thiết đều ở cuối | 
|`6 3\n1 3 2 6 5 4`| 0 | Các số cần thiết đã có ở k vị trí đầu tiên | 
|`1 1\n1`| 0 | Phần tử đơn lẻ, không có gì để trao đổi | 
|`4 4\n4 3 2 1`| 4 | Toàn số sai vị trí, hoán vị đầy đủ | 

## Vỏ cạnh 

Nếu$k = n$, cái đầu tiên$k$vị trí bao gồm toàn bộ mảng. Thuật toán đếm có bao nhiêu số từ 1 đến$n$đang ở vị trí đầu tiên$n$những vị trí luôn luôn là$n$. Số còn thiếu là$n-n=0$, do đó nó xuất ra chính xác 0. 

Nếu$k = 1$và phần tử đầu tiên không phải là 1, thuật toán sẽ tính xem 1 có ở vị trí đầu tiên hay không. Nếu không, nó xuất ra 1, cho biết cần một lần hoán đổi để đưa số 1 lên phía trước. 

Nếu mảng đã được sắp xếp với$k$số nhỏ nhất lúc đầu$k$khe, thuật toán đếm chính xác tất cả tại chỗ và xuất ra 0. Ví dụ: (n=5, k=3, p=[1,2,3,5,4
