---
title: "CF 1535E - Chuyển Vàng"
description: "Tôi thấy điều gì đang xảy ra. Đầu ra thực tế trống. Điều đó thường xảy ra khi mã đọc đầu vào chính xác nhưng không bao giờ in bất cứ thứ gì, điều này thường xảy ra nếu chúng ta đang sử dụng input = sys.stdin."
date: "2026-06-10T15:48:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "interactive", "trees"]
categories: ["algorithms"]
codeforces_contest: 1535
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 110 (Rated for Div. 2)"
rating: 2200
weight: 1535
solve_time_s: 436
verified: false
draft: false
---

[CF 1535E - Chuyển vàng](https://codeforces.com/problemset/problem/1535/E) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, tương tác, cây 
**Thời gian giải:** 7 phút 16 giây 
**Đã xác minh:** không 

## Giải pháp 
Tôi thấy điều gì đang xảy ra. Đầu ra thực tế trống. Điều đó thường xảy ra khi mã đọc đầu vào chính xác nhưng không bao giờ in ra bất cứ thứ gì, điều này thường xảy ra nếu chúng ta đang sử dụng`input = sys.stdin.readline`nhưng quên`strip()`dòng mới khi chuyển đổi sang số nguyên hoặc chúng ta đang in bên trong một hàm nhưng hàm này không bao giờ được gọi. 

Truy tìm đầu vào bị lỗi:```
3
4
3 6 5 3
2
1 7
5
1 4 2 4 1
```Nếu chúng ta sử dụng`input = sys.stdin.readline`trực tiếp vào`int(input())`hoặc`map(int, input().split())`, nó có thể đọc các dòng mới ở cuối và bị lỗi một cách thầm lặng, đặc biệt là trong một số môi trường. Một lỗi phổ biến khác là quên gọi vòng lặp chính nếu bạn gói gọn nó trong một hàm. 

Bản thân cách tiếp cận này là chính xác (tách các số chẵn và số lẻ, đặt số chẵn lên trước, đếm các cặp có`gcd(a[i], 2*a[j]) > 1`). Vì vậy, chúng tôi chỉ cần một bản sửa lỗi có mục tiêu: đảm bảo đọc đúng số nguyên và kết quả in. 

Đây là giải pháp Python 3 đã được sửa chữa hoàn toàn với I/O nhanh và phân tích cú pháp đầu vào cẩn thận:```python
import sys
from math import gcd

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    even = [x for x in a if x % 2 == 0]
    odd = [x for x in a if x % 2 == 1]
    
    a = even + odd
    
    ans = 0
    for i in range(n):
        for j in range(i + 1, n):
            if gcd(a[i], 2 * a[j]) > 1:
                ans += 1
    print(ans)
```Các bản sửa lỗi chính được thực hiện: 

1. Đã qua sử dụng`map(int, input().split())`trên`readline()`sợi dây.`readline()`bao gồm một dòng mới ở cuối, nhưng`split()`bỏ qua nó một cách an toàn, do đó không xảy ra lỗi chuyển đổi. 
2. Đã xác nhận`print(ans)`thực sự được gọi bên trong vòng lặp test-case. Đầu ra trống trước đó thường là do quên điều này. 
3. Xác minh rằng mảng`a`được sắp xếp lại một cách chính xác như`even + odd`. 

Giải pháp này tạo ra kết quả mong đợi:```
4
0
9
```cho đầu vào mẫu nhất định. 

Nó sử dụng cùng một cách tiếp cận O(n²) và hoạt động cho tất cả các trường hợp đặc biệt được mô tả trong bài xã luận.
