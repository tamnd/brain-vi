---
title: "CF 1801C - Lễ Hội Âm Nhạc"
description: "Lý do giải pháp của bạn không tạo ra đầu ra là vì nó không bao giờ thực sự ghi vào thiết bị xuất chuẩn. Trong các bản nháp trước, bạn đã thêm kết quả vào một danh sách nhưng chưa bao giờ được gọi là print hoặc sys.stdout.write. Đây là lý do tại sao chương trình chạy không có lỗi nhưng lại hiển thị đầu ra trống."
date: "2026-06-09T09:33:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1801
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 857 (Div. 1)"
rating: 1900
weight: 1801
solve_time_s: 264
verified: false
draft: false
---

[CF 1801C - Lễ hội âm nhạc](https://codeforces.com/problemset/problem/1801/C) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, sắp xếp 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
Lý do giải pháp của bạn không tạo ra đầu ra là vì nó không bao giờ thực sự ghi vào thiết bị xuất chuẩn. Trong các bản nháp trước, bạn đã thêm kết quả vào danh sách`ans`nhưng chưa bao giờ gọi`print`hoặc`sys.stdout.write`. Đây là lý do tại sao chương trình chạy không có lỗi nhưng lại hiển thị đầu ra trống. Bản thân thuật toán (công thức`2*max(abs(a),abs(b)) - 1`khi không bình đẳng,`2*abs(a)`khi bằng nhau) là đúng. 

Đây là giải pháp Python 3 hoàn chỉnh, chính xác với I/O nhanh và xử lý đầu ra thích hợp.```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    res = []
    for _ in range(t):
        a, b = map(int, input().split())
        x, y = abs(a), abs(b)
        if x == y:
            res.append(str(2 * x))
        else:
            res.append(str(2 * max(x, y) - 1))
    print("\n".join(res))

if __name__ == "__main__":
    solve()
```### Giải thích 

1.`input = sys.stdin.readline`đảm bảo đọc đầu vào nhanh chóng. 
2.`res`là một danh sách thu thập tất cả các câu trả lời. 
3.`x, y = abs(a), abs(b)`chuyển đổi tọa độ thành giá trị tuyệt đối nên hướng không quan trọng. 
4. Nếu các giá trị tuyệt đối bằng nhau
