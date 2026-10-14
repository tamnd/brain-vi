---
title: "CF 105190H - Ali nhìn thấy màu đen như thế nào"
description: "Đặt dp[n] là số phép toán tối thiểu cần thiết cho tập hợp ban đầu {1,2,...,n}. Giả sử thao tác đầu tiên chọn x. Phần tử x biến mất ngay lập tức. Mọi giá trị nhỏ hơn x không thay đổi, trong khi mọi giá trị lớn hơn x đều bị giảm x."
date: "2026-06-27T04:20:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105190
codeforces_index: "H"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2024"
rating: 0
weight: 105190
solve_time_s: 36
verified: true
draft: false
---

[CF 105190H - Ali nhìn thấy màu đen như thế nào](https://codeforces.com/problemset/problem/105190/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
hãy để`dp[n]`là số lượng thao tác tối thiểu cần thiết cho tập hợp ban đầu`{1,2,...,n}`. 

Giả sử thao tác đầu tiên chọn`x`. 

phần tử`x`biến mất ngay lập tức. Mọi giá trị đều nhỏ hơn`x`không thay đổi, trong khi mọi giá trị lớn hơn`x`được giảm bởi`x`. Sau khi loại bỏ số 0, các giá trị còn lại chính xác là sự kết hợp của`{1,2,...,x-1}`Và`{1,2,...,n-x}`. 

Hai phần hoàn toàn độc lập. Vì các giá trị trùng lặp không bao giờ làm cho bài toán trở nên khó hơn nên công việc còn lại chỉ được xác định bằng giá trị lớn hơn trong hai phạm vi này. Kích thước lớn hơn là`max(x-1, n-x)`. 

Kể từ đây`dp[n] = 1 + min(max(dp[x-1], dp[n-x]))`. 

Lựa chọn tốt nhất luôn là chia phạm vi càng đồng đều càng tốt, do đó`max(x-1, n-x) = floor(n/2)`, 

đưa ra sự tái phát đơn giản hơn nhiều`dp[0] = 0`

`dp[n] = dp[n//2] + 1`. 

Vì`n ≤ 100`, một DP nhỏ là đủ.```python
import sys
input = sys.stdin.readline

n = int(input())

dp = [0] * (n + 1)
for i in range(1, n + 1):
    dp[i] = dp[i // 2] + 1

print(dp[n])
```Phép truy hồi này tương đương với dạng đóng`dp[n] = floor(log2(n)) + 1`vì`n ≥ 1`. Vì vậy, ví dụ,`n = 47`cho`5 + 1 = 6`, phù hợp với mẫu
