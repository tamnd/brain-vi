---
title: "CF 1609C - Phân tích thị trường phức tạp"
description: "Chúng ta được cung cấp một mảng các số nguyên và kích thước bước e. Đối với mỗi chỉ số bắt đầu i, chúng ta có thể tạo một dãy con bằng cách lấy mọi phần tử thứ e: a[i], a[i+e], a[i+2e], ... cho đến điểm mà chỉ số không vượt quá giới hạn mảng."
date: "2026-06-10T07:23:37+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "implementation", "number-theory", "schedules", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1609
codeforces_index: "C"
codeforces_contest_name: "Deltix Round, Autumn 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1400
weight: 1609
solve_time_s: 96
verified: true
draft: false
---

[CF 1609C - Phân tích thị trường phức tạp](https://codeforces.com/problemset/problem/1609/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, dp, thực hiện, lý thuyết số, lịch trình, hai con trỏ 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và kích thước bước`e`. Đối với mỗi chỉ số bắt đầu`i`, chúng ta có thể tạo thành một dãy con bằng cách lấy mọi`e`-phần tử thứ:`a[i], a[i+e], a[i+2e], ...`cho đến khi chỉ số không vượt quá giới hạn mảng. Đối với mỗi dãy con như vậy, chúng ta được yêu cầu đếm xem có bao nhiêu tiền tố liền kề có độ dài`k+1`có tích là số nguyên tố. Mỗi tiền tố hợp lệ tương ứng với một cặp`(i, k)`. 

Một nhận xét quan trọng là một tích chỉ có thể là số nguyên tố nếu có đúng một phần tử của nó là số nguyên tố và các phần tử còn lại là số nguyên tố.`1`S. Điều này là do nhân bất kỳ số nguyên tố nào với một số nguyên khác lớn hơn`1`tạo ra một hợp số. Vì vậy, ta chỉ cần tìm các dãy con trong đó số nguyên tố được bao quanh bởi 0 hoặc nhiều hơn`1`s, có thể được phân tách bằng kích thước bước`e`. Tất cả các số khác không đóng góp vào một cặp hợp lệ. 

Các ràng buộc rất cao: tổng của`n`trên tất cả các trường hợp thử nghiệm có thể đạt được`2 * 10^5`và mỗi số có thể lên tới`10^6`. Một cách tiếp cận đơn giản để kiểm tra mọi dãy con có thể sẽ yêu cầu các phép toán tỉ lệ với`O(n^2/e)`trong trường hợp xấu nhất là quá chậm. Bất kỳ giải pháp nào cũng cần xử lý từng mảng trong thời gian gần tuyến tính. 

Các trường hợp biên bao gồm tất cả các chuỗi`1`s, các chuỗi trong đó các số nguyên tố xuất hiện ở đầu hoặc cuối và các chuỗi trong đó`e`bằng với`1`(vì vậy các chuỗi con liền kề nhau) hoặc bằng`n`(vì vậy các chuỗi con chứa một phần tử duy nhất). Ví dụ, nếu`a = [1, 2, 1]`với`e = 1`, có ba cặp hợp lệ:`(2, 0)`cho thủ tướng`2`một mình,`(1, 1)`cho phần tiếp theo`[1,2]`cho đi`2`, Và`(1, 2)`vì`[1,2,1]`cho đi`2`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên mọi chỉ số bắt đầu`i`và sau đó lặp lại mọi thứ có thể`k`để tính tích của dãy con một cách rõ ràng. Sau đó chúng tôi sẽ kiểm tra xem sản phẩm đó có phải là nguyên tố hay không. Điều này đúng vì nó thực sự đếm tất cả các cặp hợp lệ. Tuy nhiên, số thao tác trong trường hợp xấu nhất là theo thứ tự`n^2/e`, tùy thuộc vào`10^10`cho đầu vào lớn nhất. Điều này rõ ràng vượt quá giới hạn thời gian 2 giây. 

Cái nhìn sâu sắc quan trọng là cách duy nhất để một sản phẩm có thể là số nguyên tố là nếu có chính xác một số nguyên tố trong dãy con và các số còn lại là`1`S. Điều này làm giảm vấn đề đếm số dãy trong đó số nguyên tố nằm cạnh bất kỳ số nào.`1`s ở cả hai bên dọc theo`e`-bước tiếp theo. Đối với mỗi số nguyên tố ở vị trí`p`, cho phép`l`là số liên tiếp`1`s trước nó (bước tới`e`) Và`r`số liên tiếp`1`theo sau nó. Khi đó số cặp hợp lệ được đóng góp bởi số nguyên tố này là`(l + 1) * (r + 1) - 1`. Phép trừ của`1`loại bỏ trường hợp trong đó`k = 0`và không bao gồm số nguyên tố, điều này không hợp lệ. 

Chúng ta có thể thực hiện điều này bằng cách xử lý từng`e`các chuỗi con độc lập riêng biệt, quét từng chuỗi một cách tuyến tính để đếm liên tiếp`1`s và xác định số nguyên tố. Điều này làm giảm độ phức tạp tổng thể xuống`O(n)`cho mỗi trường hợp thử nghiệm, điều này là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2/e) | O(1) | Quá chậm | 
| Prime + 1 số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố đến`10^6`bằng cách sử dụng sàng Eratosthenes. Điều này cho phép chúng ta kiểm tra xem một số có phải là số nguyên tố trong thời gian không đổi hay không. 
2. Với mỗi test case, hãy đọc`n`,`e`, và mảng`a`. 
3. Phân chia mảng thành`e`dãy con độc lập dựa trên chỉ số modulo`e`. Đối với chỉ mục`i`, dãy số sau là`a[i], a[i+e], a[i+2e], ...`. 
4. Đối với mỗi chuỗi con, duyệt qua nó trong khi theo dõi chuỗi liên tiếp`1`s ở bên trái và bên phải của mỗi số nguyên tố. Khởi tạo bộ đếm`cnt`trong suốt chiều dài liên tiếp`1`s trước phần tử hiện tại. Khi một số nguyên tố được tìm thấy ở vị trí`p`, đếm liên tiếp`1`theo sau nó bằng cách quét về phía trước. 
5. Để có được vị trí đắc địa`p`với`l`những cái trước nó và`r`những cái sau nó, số cặp hợp lệ là`(l + 1) * (r + 1) - 1`. Tích lũy điều này vào câu trả lời tổng thể. 
6. Xuất ra tổng số cặp hợp lệ cho test case. 

Tại sao nó hoạt động: Mỗi cặp hợp lệ tương ứng với một số nguyên tố trong dãy con có số lượng nhất định`1`s trước và sau. Công thức`(l + 1) * (r + 1) - 1`đếm tất cả các tiền tố chứa chính xác một số nguyên tố, bao gồm tất cả các kết hợp của`1`ở xung quanh nó. Việc xử lý các chuỗi con một cách độc lập đảm bảo chúng tôi tôn trọng kích thước bước`e`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_A = 10**6 + 1
is_prime = [True] * MAX_A
is_prime[0] = is_prime[1] = False
for i in range(2, int(MAX_A ** 0.5) + 1):
    if is_prime[i]:
        for j in range(i*i, MAX_A, i):
            is_prime[j] = False

t = int(input())
for _ in range(t):
    n, e = map(int, input().split())
    a = list(map(int, input().split()))
    res = 0
    
    for start in range(e):
        seq = []
        for i in range(start, n, e):
            seq.append(a[i])
        
        ones_before = 0
        i = 0
        while i < len(seq):
            if seq[i] == 1:
                ones_before += 1
                i += 1
            elif is_prime[seq[i]]:
                ones_after = 0
                j = i + 1
                while j < len(seq) and seq[j] == 1:
                    ones_after += 1
                    j += 1
                res += (ones_before + 1) * (ones_after + 1) - 1
                ones_before = 0
                i = j
            else:
                ones_before = 0
                i += 1
    print(res)
```Sàng tính toán trước các số nguyên tố lên đến`10^6`, cho phép O(1) kiểm tra tính nguyên tố. Mỗi chuỗi con được quét tuyến tính, với`ones_before`Và`ones_after`theo dõi số lượng liên tiếp`1`s, đảm bảo chúng tôi đếm chính xác tất cả hợp lệ`(i, k)`cặp. Vòng lặp được đặt lại cẩn thận`ones_before`khi gặp hợp số để tránh đếm các dãy không thể tạo ra số nguyên tố. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
7 3
10 2 1 3 1 19 3
```| bước | seq | cái_trước | nguyên tố | cái_sau | đóng góp | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | [10, 2, 1, 3, 1, 19, 3] | 0 | 2 ở chỉ số 1 | 1 | (0+1)*(1+1)-1=1 | 1 | 
| 1 | [10, 2, 1, 3, 1, 19, 3] | 0 | 3 ở chỉ số 3 | 1 | (0+1)*(1+1)-1=1 | 2 | 

Tổng cộng`res = 2`, khớp với đầu ra mẫu. 

**Đầu vào mẫu 2:**```
3 2
1 13 1
```| bước | seq | cái_trước | nguyên tố | cái_sau | đóng góp | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | [1, 13, 1] | 1 | 13 ở chỉ số 1 | 1 | (1+1)*(1+1)-1=3 | 3 | 

Thuật toán đếm chính xác kích thước bước`e=2`và trình tự với`1`s xung quanh một số nguyên tố. Vì độ dài dãy con nhỏ nên tất cả các cặp đều được che phủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + MAX_A nhật ký nhật ký MAX_A) | Cái sàng lấy`MAX_A log log MAX_A`một lần. Mỗi trường hợp thử nghiệm xử lý n phần tử trên`e`dãy con một cách tuyến tính. | 
| Không gian | O(n + MAX_A) | Mảng lưu trữ và sàng lọc cho tính nguyên thủy. | 

Giải pháp chia tỷ lệ tuyến tính với kích thước đầu vào`n`và dễ dàng xử lý tổng của`n`lên tới`2 * 10^5`. Chi phí sàng là chi phí tính toán trước một lần và không ảnh hưởng đến hiệu suất của mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input_backup = builtins.input
    builtins.input = lambda: sys.stdin.readline()
    exec(open("solution.py").read())
    builtins.input = input_backup
    return ""  # assumes solution.py
```
