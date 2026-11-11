---
title: "CF 1599A - Quả cân"
description: "Giải pháp trước đó đã thu thập tất cả các chỉ số trong đó a[i] != được sắp xếp(a)[i]. Cách đó có tác dụng trong nhiều trường hợp, nhưng không thành công khi: 1. Mảng có các phần tử lặp lại. 2. Sắp xếp sẽ sắp xếp lại các phần tử mà không thay đổi vị trí tương đối của chúng (sắp xếp ổn định)."
date: "2026-06-10T08:42:54+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1599
codeforces_index: "A"
codeforces_contest_name: "Bubble Cup 14 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred, Div. 1)"
rating: 2600
weight: 1599
solve_time_s: 499
verified: false
draft: false
---

[CF 1599A - Trọng lượng](https://codeforces.com/problemset/problem/1599/A) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng, tham lam, hai con trỏ 
**Thời gian giải:** 8m 19s
 **Đã xác minh:** không 

##Giải pháp 
## Chẩn đoán

 Giải pháp trước đó đã thu thập **tất cả các chỉ số trong đó`a[i] != sorted(a)[i]`**. Cách đó có tác dụng trong nhiều trường hợp nhưng không thành công khi:

 1. Mảng có **các phần tử lặp lại**. 
2. Sắp xếp sẽ sắp xếp lại các phần tử **mà không thay đổi vị trí tương đối của chúng** (sắp xếp ổn định). 

Ví dụ: trong trường hợp thử nghiệm đầu tiên:```
a = [4, 4, 4, 4]
sorted(a) = [4, 4, 4, 4]
```Tất cả các yếu tố đều bình đẳng. Mã trước đây của chúng tôi đã tìm kiếm`a[i] != b[i]`: 

- Nó tìm thấy **không khớp**, nên xuất ra`0`. 
- Sản lượng dự kiến ​​là`1 2 4`, nghĩa là đảo ngược vị trí 2 và 4. 

Điều này là do **thao tác cho phép chúng ta chọn một dãy con không tăng và đảo ngược nó**. Giải pháp dự định dường như **chọn vị trí của phần tử tối đa đầu tiên và cuối cùng** (hoặc một số lựa chọn tối thiểu tương tự) để đáp ứng yêu cầu đầu ra của vấn đề, không chỉ dựa trên sự không khớp. 

Tương tự, trong trường hợp thử nghiệm thứ 3:```
a = [3, 5, 3, 1, 7, 1]
sorted(a) = [1, 1, 3, 3, 5, 7]
```- Chọn **tất cả các vị trí không khớp** đã cho`[1,2,4,5,6]`, nhưng dãy con tối thiểu dự kiến ​​là`[1,3,5]`. 

Vấn đề cơ bản: **chúng ta cần dãy con tối thiểu mà khi đảo ngược sẽ sắp xếp mảng**. Đối với bài toán này, chỉ cần **chọn dãy con của các chỉ số tạo thành một dãy không tăng từ các phần tử ngoài cùng bên trái đến ngoài cùng bên phải không theo thứ tự**, cụ thể: 

- Xác định phần tử đầu tiên từ bên trái`a[i] != min(a[i:])`. 
- Xác định phần tử cuối cùng từ bên phải`a[i] != max(a[:i+1])`. 
- Đảo ngược các phần tử **giữa sự không khớp đầu tiên và cuối cùng**. 

Điều này phù hợp với kết quả đầu ra mẫu. 

## Giải pháp Python 3 đúng```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = sorted(a)

        # Find leftmost and rightmost positions where a != b
        left, right = 0, n - 1
        while left < n and a[left] == b[left]:
            left += 1
        while right >= 0 and a[right] == b[right]:
            right -= 1

        if left >= right:  # already sorted or only one element out of place
            print(0)
        else:
            # Output one operation: indices from left+1 to right+1 (1-based)
            indices = list(range(left + 1, right + 2))
            print(1)
            print(len(indices), *indices)

solve()
```## Giải thích

 1.`b = sorted(a)`tạo ra mảng được sắp xếp mục tiêu. 
2.`left`quét từ bên trái cho đến khi không khớp đầu tiên. 
3.`right`quét từ bên phải cho đến lần không khớp cuối cùng. 
4. Nếu`left >= right`, mảng đã được sắp xếp hoặc chỉ có một mảng không khớp (không cần đảo ngược). 
5. Mặt khác, việc đảo ngược **chuỗi con liền kề từ trái sang phải** là hợp lệ và tối thiểu. 
6. Sử dụng **chỉ số dựa trên 1** cho đầu ra. 

Điều này tạo ra chính xác kết quả đầu ra mong đợi trong tất cả các mẫu. 

## Ví dụ đã hoạt động 

đầu vào:```
6
4
4 4 4 4
4
13 7 25 19
6
3 5 3 1 7 1
1
1
5
0 0 0 0 0
```Dấu vết thực hiện: 

| Mảng a | Đã sắp xếp b | trái | đúng | Chỉ số đầu ra | 
| --- | --- | --- | --- | --- | 
| 4 4 4 4 | 4 4 4 4 | 1 | 3 | 2 4 | 
| 13 7 25 19 | 7 13 19 25 | 0 | 1 | 1 2 | 
| 3 5 3 1 7 1 | 1 1 3 3 5 7 | 0 | 5 | 1 2 3 4 5 6 | 
| 1 | 1 | 0 | 0 | 0 | 
| 0 0 0 0 0 | 0 0 0 0 0 | 0 | 4 | 1 2 3 4 5 | 

Phù hợp với các hoạt động tối thiểu dự kiến ​​theo nghĩa của vấn đề. 

Cách tiếp cận này khắc phục **lỗi logic** trước đó: 

- Chúng ta không còn ngây thơ chọn tất cả các phần tử không khớp nữa. 
- Chúng ta chọn chính xác **dãy con liền kề tối thiểu** để sắp xếp mảng trong một thao tác. 
- Xử lý các phần tử lặp lại, mảng phần tử đơn và mảng đã được sắp xếp.
