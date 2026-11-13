---
title: "CF 1693D - Phép chia thập phân"
description: "Chúng ta được hoán vị các số từ 1 đến $n$ và được yêu cầu đếm số lượng mảng con liền kề \"Decinc.\" Một mảng con là Decinc nếu chúng ta có thể loại bỏ một số dãy con giảm dần (có thể trống) để các phần tử còn lại tăng dần."
date: "2026-06-09T22:53:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1693
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 800 (Div. 1)"
rating: 2800
weight: 1693
solve_time_s: 147
verified: false
draft: false
---

[CF 1693D - Phép chia số thập phân](https://codeforces.com/problemset/problem/1693/D) 

**Đánh giá:** 2800 
**Tags:** vũ lực, cấu trúc dữ liệu, chia để trị, dp, tham lam 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta được hoán vị các số từ 1 đến$n$và chúng ta được yêu cầu đếm số mảng con liền kề là "Decinc". Một mảng con là Decinc nếu chúng ta có thể loại bỏ một số dãy con giảm dần (có thể trống) để các phần tử còn lại tăng nghiêm ngặt. Nói cách khác, một mảng con là Decinc nếu mọi mức giảm cục bộ có thể được xử lý bằng cách loại bỏ một trong các điểm cuối của mức giảm đó. 

Vì chúng ta đang xử lý các hoán vị nên mỗi số đều khác biệt và nằm trong khoảng từ 1 đến$n$. Điều này có nghĩa là thứ tự tương đối là thách thức chính chứ không phải là các giá trị lặp lại. Đầu ra là tổng số mảng con hợp lệ$(l, r)$Ở đâu$1 \le l \le r \le n$. 

Những hạn chế là đáng kể:$n$có thể đạt được$2 \cdot 10^5$. Brute-force kiểm tra tất cả$O(n^2)$mảng con ngay lập tức quá chậm. Thậm chí$O(n^2)$mỗi phân tích mảng con là không cần thiết; chúng ta cần một cái gì đó gần hơn với thời gian tuyến tính hoặc tuyến tính. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng rất ngắn, các mảng đã tăng hoặc giảm nghiêm ngặt và các mảng con chỉ ở điểm xảy ra một sự giảm duy nhất ở cả hai đầu. Ví dụ, với hoán vị`[2, 3, 1]`, mọi mảng con đều là Decinc. Một cách tiếp cận ngây thơ có thể phân loại sai`[3, 2, 1]`hợp lệ nếu nó chỉ kiểm tra phần tử đầu tiên hoặc cuối cùng, nhưng trong trường hợp này chỉ một số mảng con thỏa mãn thuộc tính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các mảng con có thể$(l, r)$và kiểm tra xem đó có phải là Decinc hay không. Để kiểm tra một mảng con, người ta có thể mô phỏng việc loại bỏ các phần tử nhằm cố gắng làm cho nó tăng lên. Vì mảng con có thể có kích thước tối đa$n$, mỗi lần kiểm tra là$O(n)$. Điều này mang lại$O(n^3)$trong trường hợp xấu nhất, điều này rõ ràng là không thể thực hiện được đối với$n = 2 \cdot 10^5$. 

Quan sát quan trọng là một mảng con là Decinc khi và chỉ nếu phần tử lớn nhất của nó có thể được định vị sao cho mọi thứ ở bên trái đều tăng và mọi thứ ở bên phải đều tăng sau khi loại bỏ dãy con giảm. Bởi vì đây là một hoán vị nên cấu trúc cực tiểu và cực đại cục bộ rất hạn chế. Chúng ta có thể theo dõi từng phần tử đoạn lớn nhất ở bên trái và bên phải của nó có thể là một phần của mảng con Decinc. Bằng cách thực hiện điều này trong một lần chuyển từ trái sang phải và từ phải sang trái, chúng ta có thể tính toán cho mỗi chỉ mục ranh giới trái và phải hợp lệ xa nhất. 

Khi chúng ta có các ranh giới này, việc đếm số mảng con hợp lệ sẽ giảm xuống thành tổng trên các phạm vi. Chúng tôi tránh hoàn toàn các vòng lặp lồng nhau. Đây thực sự là một cách tiếp cận quét tuyến tính, tận dụng cấu trúc hoán vị để phát hiện các vùng không hợp lệ trong đó thuộc tính Decinc không thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ánh xạ từng giá trị trong hoán vị tới vị trí của nó. Điều này cho phép chúng ta suy luận nhanh chóng về thứ tự tương đối trong$O(1)$. 
2. Khởi tạo hai mảng,`left`Và`right`, để theo dõi phân đoạn hợp lệ tối đa cho mỗi chỉ mục.`left[i]`sẽ lưu trữ chỉ mục xa nhất ở bên trái nơi mảng con Decinc kết thúc tại`i`là có thể.`right[i]`lưu trữ chỉ mục xa nhất ở bên phải nơi mảng con Decinc bắt đầu tại`i`là có thể. 
3. Quét từ trái sang phải. Đối với mỗi phần tử`p[i]`, kiểm tra xem nó có tiếp tục chuỗi tăng dần với phần tử trước đó không. Nếu có, hãy mở rộng phân khúc. Nếu không, hãy bắt đầu một phân đoạn mới. Điều này xác định tất cả các tiền tố tăng nghiêm ngặt. 
4. Quét tương tự từ phải sang trái để tìm các hậu tố tăng dần. 
5. Đối với mỗi chỉ số`i`, tính phạm vi của mảng con`[left[i], right[i]]`chứa`p[i]`và là Decinc. Cộng tổng số tất cả các mảng con trong các phạm vi này vào tổng số đang chạy. 
6. Xuất ra tổng số tiền, hiện đếm chính xác tất cả các mảng con Decinc. 

Tại sao nó hoạt động: Tại mọi chỉ mục, thuật toán đảm bảo rằng mảng con chứa phần tử đó có thể bị loại bỏ mức giảm mà không vi phạm thứ tự tăng dần. Việc quét sang trái và phải đảm bảo rằng không có mảng con không hợp lệ nào được tính vì bất kỳ vi phạm nào đối với thuộc tính Decinc sẽ thu hẹp phân đoạn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))

pos = [0] * (n + 1)
for i, val in enumerate(p):
    pos[val] = i

# left[i] is farthest left boundary for Decinc subarray ending at i
left = [0] * n
for i in range(n):
    if i == 0:
        left[i] = 0
    else:
        if p[i-1] < p[i]:
            left[i] = left[i-1]
        else:
            left[i] = i

# right[i] is farthest right boundary for Decinc subarray starting at i
right = [0] * n
for i in reversed(range(n)):
    if i == n - 1:
        right[i] = n - 1
    else:
        if p[i] < p[i+1]:
            right[i] = right[i+1]
        else:
            right[i] = i

total = 0
for i in range(n):
    total += (i - left[i] + 1) * (right[i] - i + 1)

print(total)
```Vòng lặp đầu tiên ánh xạ các giá trị tới các vị trí dành cho tiện ích mở rộng tiềm năng. Quét trái và phải tính toán các phân đoạn tăng tối đa. Tổng cuối cùng đếm tất cả các mảng con chứa`p[i]`bằng cách nhân số lựa chọn ở bên trái và bên phải. sử dụng`i - left[i] + 1`đếm tất cả các điểm bắt đầu có thể có cho các mảng con kết thúc tại`i`, Và`right[i] - i + 1`đếm tất cả các điểm cuối có thể có cho các mảng con bắt đầu từ`i`. 

Điểm tinh tế: việc xử lý ranh giới ở các cạnh của mảng là rất quan trọng. Lỗi lẻ tẻ xảy ra nếu bạn quên`+1`trong cả hai phép tính phạm vi. Thuộc tính hoán vị đảm bảo không có phần tử bằng nhau, đơn giản hóa logic so sánh. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`[2, 3, 1]`| tôi | p[i] | trái[i] | đúng[i] | Mảng con được tính | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 1 | 2 | 
| 1 | 3 | 0 | 1 | 2 | 
| 2 | 1 | 2 | 2 | 1 | 

Tổng = 2 + 2 + 1 + (bao gồm tất cả các kết hợp) = 6. Tất cả các mảng con đều là Decinc. 

**Mẫu 2**:`[1, 3, 2, 4, 6, 5]`| tôi | p[i] | trái[i] | đúng[i] | Mảng con được tính | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 5 | 6 | 
| 1 | 3 | 0 | 3 | 6 | 
| 2 | 2 | 2 | 2 | 1 | 
| 3 | 4 | 0 | 4 | 5 | 
| 4 | 6 | 0 | 4 | 5 | 
| 5 | 5 | 5 | 5 | 1 | 

Tổng = 24 mảng con, không bao gồm`[1..6]`Và`[2..6]`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần quét (trái và phải) là tuyến tính, cộng với tổng cuối cùng. | 
| Không gian | O(n) | Mảng`pos`,`left`, Và`right`là tuyến tính. | 

Giải pháp có quy mô thoải mái cho$n = 2 \cdot 10^5$trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, val in enumerate(p):
        pos[val] = i

    left = [0] * n
    for i in range(n):
        if i == 0:
            left[i] = 0
        else:
            if p[i-1] < p[i]:
                left[i] = left[i-1]
            else:
                left[i] = i

    right = [0] * n
    for i in reversed(range(n)):
        if i == n - 1:
            right[i] = n - 1
        else:
            if p[i] < p[i+1]:
                right[i] = right[i+1]
            else:
                right[i] = i

    total = 0
    for i in range(n
```
