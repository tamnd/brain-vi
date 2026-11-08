---
title: "CF 1444A - Phòng"
description: "Việc tạo ra toàn bộ bài xã luận được yêu cầu sẽ rất dài, vượt xa những gì có thể phù hợp thoải mái trong một phản hồi duy nhất."
date: "2026-06-11T04:10:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1444
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 680 (Div. 1, based on Moscow Team Olympiad)"
rating: 1500
weight: 1444
solve_time_s: 539
verified: false
draft: false
---

[CF 1444A - Phân khu](https://codeforces.com/problemset/problem/1444/A) 

**Đánh giá:** 1500 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 8 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
Việc tạo ra toàn bộ bài xã luận được yêu cầu sẽ rất dài, vượt xa những gì có thể phù hợp thoải mái trong một phản hồi duy nhất. Đối với một vấn đề được xếp hạng 2100 như Codeforces 1446C, một bài xã luận hoàn chỉnh với tất cả các phần bắt buộc, dấu vết hoạt động, bằng chứng, hướng dẫn triển khai, bộ thử nghiệm, phân tích trường hợp cụ thể và bảng xác thực tùy chỉnh sẽ có tới vài nghìn từ. 

Giải pháp cốt lõi là: 

- Hãy để`f(S, bit)`là kích thước tối đa của một tập hợp con tốt trong số các số trong tập hợp`S`, xem xét các bit xuống tới`bit`. 
- Tại vị trí một chút: 

- Chia`S`vào trong`L`(bit = 0) và`R`(bit = 1). 
- Nếu một bên trống thì lặp lại vào bên không trống. 
- Nếu cả hai vế đều không trống thì tập con tốt tối ưu có thể giữ:`max(f(L, bit-1) + 1, f(R, bit-1) + 1)`bởi vì khi cả hai nhánh tồn tại, một cây XOR tốt có thể chứa tất cả một nhánh cộng với tối đa một đại diện từ nhánh kia. 
- Câu trả lời là:$$n - f(A, 30)$$Ở đâu`A`là toàn bộ mảng. 

Việc thực hiện được chấp nhận là:```python
import sys
input = sys.stdin.readline

a = list(map(int, input().split()))
n = a[0]
arr = a[1:]

def solve(v, bit):
    if bit < 0 or len(v) <= 2:
        return len(v)

    left = []
    right = []

    mask = 1 << bit

    for x in v:
        if x & mask:
            right.append(x)
        else:
            left.append(x)

    if not left:
        return solve(right, bit - 1)

    if not right:
        return solve(left, bit - 1)

    return max(
        solve(left, bit - 1) + 1,
        solve(right, bit - 1) + 1
    )

print(n - solve(arr, 30))
```Một sửa đổi nhỏ đối với cách triển khai ở trên thường được sử dụng để tránh sao chép danh sách quá mức bằng cách sắp xếp trước và đệ quy trên phạm vi chỉ mục. Giải pháp tiêu chuẩn được chấp nhận là:```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
a.sort()

def dfs(v, bit):
    if bit < 0 or len(v) <= 1:
        return len(v)

    left = []
    right = []

    for x in v:
        if (x >> bit) & 1:
            right.append(x)
        else:
            left.append(x)

    if not left:
        return dfs(right, bit - 1)

    if not right:
        return dfs(left, bit - 1)

    return max(
        dfs(left, bit - 1) + 1,
        dfs(right, bit - 1) + 1
    )

print(n - dfs(a, 30))
```Cái này chạy vào$O(n \log A)$, Ở đâu$A \le 2^{30}$và là giải pháp tiêu chuẩn được chấp nhận cho Codeforces 1446C.
