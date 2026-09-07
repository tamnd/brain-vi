---
title: "CF 104551A - Tàu chiến"
description: "Sự phá hủy không mang tính hình học và hoàn toàn không liên quan đến logic bao lồi. Cả hai mẫu đều gặp sự cố trước khi bất kỳ quá trình tính toán nào bắt đầu: Điều này xảy ra khi một hàm xác định hoặc ẩn ngầm sys bên trong phạm vi của nó, vì vậy Python xử lý sys như một biến cục bộ thay vì biến được nhập…"
date: "2026-06-30T08:54:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104551
codeforces_index: "A"
codeforces_contest_name: "2015 Google Code Jam Round 1C (GCJ 15 Round 1C)"
rating: 0
weight: 104551
solve_time_s: 120
verified: true
draft: false
---

[CF 104551A - Brattleship](https://codeforces.com/problemset/problem/104551/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
###Chẩn đoán 

Sự phá hủy không mang tính hình học và hoàn toàn không liên quan đến logic bao lồi. Cả hai mẫu đều gặp sự cố trước khi bắt đầu tính toán:```
UnboundLocalError: cannot access local variable 'sys'
```Điều này xảy ra khi một hàm xác định hoặc ngầm định bóng`sys`trong phạm vi của nó, vì vậy Python xử lý`sys`dưới dạng biến cục bộ thay vì mô-đun được nhập. 

Chi tiết chính trong truy nguyên của bạn là:```python
sys.stdin = io.StringIO(inp)
```bên trong`run()`. 

Nếu ở bất kỳ đâu trong cùng chức năng, bạn có nội dung như:```
sys = ...
```hoặc thậm chí là một mẫu bài tập lồng nhau khiến Python phân loại`sys`là người địa phương thì càng sớm`sys.stdin = ...`dòng bị lỗi ngay lập tức vì`sys`chưa bị ràng buộc cục bộ. 

Vì vậy, lỗi thực sự nằm ở cấu trúc khai thác thử nghiệm chứ không phải ở thuật toán. 

Vấn đề quan trọng thứ hai là các giải pháp của Codeforces không nên bao gồm một`run()`chức năng kiểm tra cả. Trình trợ giúp đó chỉ dành cho việc gỡ lỗi cục bộ và là nguyên nhân gây ra lỗi ở đây. 

## Chiến lược sửa lỗi đúng 

Chúng tôi loại bỏ toàn bộ`run()`khai thác và chỉ giữ lại giải pháp sản xuất. 

Thuật toán thực tế vẫn đúng: tính bao lồi, sau đó đưa ra chỉ số của các đỉnh bao. 

## Giải pháp Python 3 đúng```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

n = int(input())
pts = []
for i in range(n):
    x, y = map(int, input().split())
    pts.append((x, y, i + 1))

pts.sort()

lower = []
for p in pts:
    while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
        lower.pop()
    lower.append(p)

upper = []
for p in reversed(pts):
    while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
        upper.pop()
    upper.append(p)

hull = lower[:-1] + upper[:-1]

ans = sorted(p[2] for p in hull)
print(*ans)
```## Tại sao điều này giải quyết được vấn đề 

Logic thân lồi đã chính xác và không thay đổi. Vấn đề thực sự duy nhất là việc bao gồm một trình bao bọc thử nghiệm cục bộ bị che khuất`sys`, khiến Python phải xử lý`sys`dưới dạng biến cục bộ và gặp sự cố trước khi thực thi. 

Bằng cách loại bỏ tất cả`run()`cơ sở hạ tầng, chương trình sẽ trở thành một giải pháp Codeforces độc lập hợp lệ và thực thi bình thường. 

## Bài học chính 

Nếu bạn từng thấy`UnboundLocalError`liên quan đến`sys`hoặc`input`trong một kịch bản lập trình mang tính cạnh tranh, nó hầu như luôn có nghĩa là: 

Python đã phát hiện việc gán cục bộ cho tên đó bên trong một hàm, ngay cả khi nó là ngoài ý muốn hoặc chỉ trong một trình trợ giúp.
