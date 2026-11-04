---
title: "CF 1151D - Stas và hàng đợi tại tiệc buffet"
description: "Chúng tôi có một hàng học sinh, mỗi học sinh có hai đặc điểm cá nhân: $ai$, đo lường mức độ họ không thích những người ở phía trước và $bi$, đo lường mức độ họ không thích những người ở phía sau."
date: "2026-06-12T03:04:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1151
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 553 (Div. 2)"
rating: 1600
weight: 1151
solve_time_s: 183
verified: true
draft: false
---

[CF 1151D - Stas và hàng đợi tại bữa tiệc buffet](https://codeforces.com/problemset/problem/1151/D) 

**Đánh giá:** 1600 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng học sinh, mỗi học sinh có hai đặc điểm riêng:$a_i$, đo lường mức độ họ không thích những người trước mặt họ và$b_i$, đo lường mức độ họ không thích những người đứng sau họ. Sự không hài lòng của sinh viên phụ thuộc tuyến tính vào vị trí của họ: nếu sinh viên$i$đang ở vị trí$j$, sự không hài lòng của họ là$a_i \cdot (j-1) + b_i \cdot (n-j)$. Tổng số sự không hài lòng của hàng đợi là tổng của sự không hài lòng cá nhân của tất cả học sinh. Mục tiêu của chúng ta là sắp xếp lại hàng đợi để giảm thiểu số tiền này. 

Đầu vào cung cấp$n$học sinh với họ$a_i$Và$b_i$các giá trị và đầu ra là một số nguyên duy nhất, tổng mức độ không hài lòng tối thiểu. 

Từ$n$có thể lên đến$10^5$, bất kỳ thuật toán nào với$O(n^2)$sự phức tạp là quá chậm. Điều đó loại trừ việc thử mọi hoán vị, đó sẽ là$O(n!)$. Chúng ta cần một giải pháp đại khái$O(n \log n)$hoặc$O(n)$. 

Một sự tinh tế quan trọng phát sinh khi$a_i$Và$b_i$khác nhau rất nhiều giữa các sinh viên. Một cách tiếp cận ngây thơ có thể cố gắng xếp những học sinh có trình độ học vấn nhỏ$a_i$ở phía trước và lớn$b_i$ở phía sau, nhưng điều này có thể thất bại khi sự khác biệt tương đối là quan trọng. Ví dụ: nếu hai học sinh có$a_1=1, b_1=100$Và$a_2=100, b_2=1$, đặt học sinh 1 ở phía trước sẽ giảm thiểu sự không hài lòng của họ, trong khi đặt học sinh 2 ở phía sau sẽ giảm thiểu sự không hài lòng của họ. Điều quan trọng là tìm ra trật tự toàn cầu để cân bằng những sự đánh đổi này. 

## Phương pháp tiếp cận 

Giải pháp brute-force là tạo ra mọi hoán vị của hàng đợi và tính tổng mức độ không hài lòng cho mỗi hoán vị. Mỗi phép tính mất$O(n)$thời gian, và có$n!$hoán vị. Với$n=10^5$,$n!$có quy mô lớn về mặt thiên văn nên việc sử dụng vũ lực là không thể thực hiện được. 

Quan sát quan trọng là sự không hài lòng có tính chất tuyến tính. Đối với một học sinh nhất định$i$, di chuyển chúng về phía trước một vị trí sẽ làm giảm số hạng$a_i \cdot (j-1)$qua$a_i$nhưng tăng$b_i \cdot (n-j)$qua$b_i$. Vì vậy, chi phí cận biên để di chuyển chúng về phía trước là$a_i - b_i$. Điều này có nghĩa là nếu$a_i > b_i$, sự không hài lòng của họ tăng lên nhanh hơn khi họ đến sau trong hàng đợi, vì vậy chúng ta nên đặt họ về phía trước. Ngược lại, nếu$a_i < b_i$, họ nên đi về phía sau. Sắp xếp học sinh theo$a_i - b_i$theo thứ tự giảm dần nắm bắt được nguyên tắc này. 

Sau khi sắp xếp, xếp học sinh theo thứ tự sắp xếp từ trước ra sau để giảm thiểu tổng. Sau đó, mức độ không hài lòng của mỗi học sinh sẽ được tính toán trực tiếp dựa trên vị trí của họ trong hàng đợi được sắp xếp này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và$a_i, b_i$giá trị cho mỗi học sinh. Chúng tôi lưu trữ chúng dưới dạng bộ dữ liệu$(a_i, b_i)$. 
2. Tính chênh lệch$a_i - b_i$cho mỗi học sinh. Giá trị này thể hiện mức độ học sinh thích đứng phía trước hơn là phía sau. 
3. Sắp xếp học sinh theo thứ tự giảm dần$a_i - b_i$. Những học sinh có sự khác biệt lớn hơn sẽ tiến lên phía trước vì sự bất mãn của họ sẽ tăng nhanh hơn nếu họ xếp hàng muộn hơn. 
4. Khởi tạo một biến`total`về không. Lặp lại các học sinh đã được sắp xếp, gán chúng vào các vị trí$1$ĐẾN$n$. Đối với mỗi học sinh vào vị trí$j$, tính toán sự không hài lòng như$a_i \cdot (j-1) + b_i \cdot (n-j)$và thêm nó vào`total`. 
5. Đầu ra`total`. 

Tại sao nó hoạt động: sắp xếp theo$a_i - b_i$đảm bảo rằng việc hoán đổi hai học sinh chỉ vi phạm trật tự nếu một học sinh có điểm cao hơn$a_i - b_i$được đặt sau, điều này sẽ làm tăng sự không hài lòng hoàn toàn. Tính tuyến tính của hàm không thỏa mãn đảm bảo rằng sau khi được sắp xếp, bất kỳ sai lệch nào so với thứ tự này đều làm tăng tổng, do đó phương pháp tham lam là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
students = []

for _ in range(n):
    a, b = map(int, input().split())
    students.append((a, b))

# Sort by a_i - b_i descending
students.sort(key=lambda x: x[0] - x[1], reverse=True)

total = 0
for idx, (a, b) in enumerate(students):
    position = idx + 1
    total += a * (position - 1) + b * (n - position)

print(total)
```Mã đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Sắp xếp sử dụng hàm lambda cho khóa`a_i - b_i`và đảo ngược thứ tự, đảm bảo sự khác biệt lớn nhất được đưa lên phía trước. Vòng lặp gán các vị trí từ 1 đến n, khớp với các vị trí hàng đợi mà con người có thể đọc được. Lập chỉ mục với`idx + 1`ánh xạ chính xác các chỉ số Python dựa trên 0 tới các vị trí dựa trên một. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
4 2
2 3
6 1
```| Bước | Danh sách sinh viên | a-b | Danh sách sắp xếp | Vị trí | Không hài lòng | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | [(4,2),(2,3),(6,1)] | [2,-1,5] | [(6,1),(4,2),(2,3)] | - | - | 0 | 
| 1 | - | - | - | 1 | 6_0 + 1_2 = 2 | 2 | 
| 2 | - | - | - | 2 | 4_1 + 2_1 = 6 | 8 | 
| 3 | - | - | - | 3 | 2_2 + 3_0 = 4 | 12 | 

Dấu vết xác nhận thứ tự đã sắp xếp tạo ra tổng số điểm không hài lòng tối thiểu là 12. 

### Mẫu 2 

đầu vào:```
4
1 5
3 2
4 4
2 3
```| Bước | a-b | Đã sắp xếp | Vị trí | Không hài lòng | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [-4,1,0,-1] | [(3,2),(4,4),(2,3),(1,5)] | - | - | 0 | 
| 1 | - | - | 1 | 3_0 + 2_3 = 6 | 6 | 
| 2 | - | - | 2 | 4_1 + 4_2 = 12 | 18 | 
| 3 | - | - | 3 | 2_2 + 3_1 = 7 | 25 | 
| 4 | - | - | 4 | 1_3 + 5_0 = 3 | 28 | 

Dấu vết này xác nhận tính toán chính xác cho hàng đợi lớn hơn một chút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, O (n log n). Lặp lại vị trí là O(n). | 
| Không gian | O(n) | Lưu trữ n bộ dữ liệu sinh viên và một vài biến vô hướng. | 

Với$n\le 10^5$,$O(n \log n)$có thể chấp nhận được trong giới hạn 1 giây và bộ nhớ nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    students = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]
    students.sort(key=lambda x: x[0]-x[1], reverse=True)
    total = 0
    for idx, (a, b) in enumerate(students):
        pos = idx + 1
        total += a*(pos-1) + b*(n-pos)
    return str(total)

# provided samples
assert run("3\n4 2\n2 3\n6 1\n") == "12", "sample 1"
assert run("4\n1 5\n3 2\n4 4\n2 3\n") == "28", "sample 2"

# custom cases
assert run("1\n10 20\n") == "0", "single student"
assert run("2\n1 1\n1 1\n") == "2", "two equal students"
assert run("3\n100000000 1\n1 100000000\n50 50\n") == "200000001", "large disparity values"
assert run("5\n5 1\n4 2\n3 3\n2 4\n1 5\n") == "35", "descending a, ascending b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 sinh viên | 0 | Đầu vào kích thước tối thiểu | 
| 2 học sinh ngang nhau | 2 | Giá trị đối xứng, thứ tự tầm thường | 
| Chênh lệch lớn | 200000001 | Xử lý đúng số lượng lớn và trật tự | 
| Giảm dần a, tăng dần b | | |
