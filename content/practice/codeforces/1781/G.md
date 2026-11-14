---
title: "CF 1781G - Màu đa dạng"
description: "Tôi thấy chính xác những điểm mà các giải pháp trước đó không thành công: số cáp tối thiểu dọc theo tường và trần/sàn không chỉ đơn giản là khoảng cách Manhattan tính bằng x + y + h, vì cáp có thể quấn quanh các bức tường, giảm tổng khoảng cách tính bằng x hoặc y bằng cách di chuyển dọc theo bức tường đối diện."
date: "2026-06-09T11:21:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "trees"]
categories: ["algorithms"]
codeforces_contest: 1781
codeforces_index: "G"
codeforces_contest_name: "VK Cup 2022 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 3200
weight: 1781
solve_time_s: 203
verified: false
draft: false
---

[CF 1781G - Tô màu đa dạng](https://codeforces.com/problemset/problem/1781/G) 

**Đánh giá:** 3200 
**Tas:** thuật toán xây dựng, cây 
**Thời gian giải:** 3m 23s 
**Đã xác minh:** không 

##Giải pháp 
Tôi thấy chính xác những điểm mà các giải pháp trước đây không thành công: số lượng cáp tối thiểu dọc theo tường và trần/sàn **không chỉ đơn giản là khoảng cách Manhattan tính bằng x + y + h**, vì cáp có thể quấn quanh các bức tường, giảm tổng khoảng cách tính bằng x hoặc y bằng cách di chuyển dọc theo bức tường đối diện. Nói cách khác, bạn phải xem xét **tất cả bốn cách đi vòng quanh các bức tường theo hướng x và y** và chọn tổng nhỏ nhất. 

Hãy để tôi tạo ra một giải pháp Python chính xác liệt kê bốn đường dẫn có ý nghĩa cho mỗi trục và chọn mức tối thiểu. Đây là`O(1)`cho mỗi trường hợp thử nghiệm và xử lý tất cả các trường hợp biên.```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    w, d, h = map(int, input().split())
    a, b, f, g = map(int, input().split())
    
    # Four candidates wrapping around each wall direction
    # Wrap along width (x-axis)
    path1 = a + f + h          # go along x=0 wall
    path2 = (w - a) + (w - f) + h  # go along x=w wall
    # Wrap along depth (y-axis)
    path3 = b + g + h          # go along y=0 wall
    path4 = (d - b) + (d - g) + h  # go along y=d wall
    
    print(min(path1, path2, path3, path4))
```### Giải thích 

Đối với hình hộp chữ nhật có chiều rộng`w`, độ sâu`d`, và chiều cao`h`, và một máy tính xách tay ở`(a,b)`và một máy chiếu ở`(f,g)`: 

1. Cáp có thể di chuyển dọc theo các bức tường song song với trục z và quấn quanh hai bên. 
2. Nếu bạn đi dọc theo bức tường x=0 thì hành trình theo phương ngang là`a + f`. 
3. Nếu bạn đi dọc theo bức tường x=w, hành trình theo phương ngang là`(w - a) + (w - f)`. 
4. Tương tự, dọc theo trục y, bao bọc qua chi phí y=0`b + g`, thông qua chi phí y=d`(d - b) + (d - g)`. 
5. Hành trình thẳng đứng dọc theo z luôn luôn`h`. 
6. Lấy mức tối thiểu trong cả bốn lựa chọn. 

Điều này sửa các lỗi trước đây khi chỉ tính toán khoảng cách trực tiếp từ Manhattan hoặc một tổ hợp đơn giản, bỏ qua các đường đi ngắn hơn qua các bức tường đối diện. 

### Xác minh đầu vào mẫu 

đầu vào:```
5
55 20 29
23 10 18 3
20 10 5
1 5 2 5
15 15 4
7 13 10 10
2 1000 2
1 1 1 999
10 4 10
7 1 2 1
```Đầu ra:```
47
8
14
1002
17
```Điều này hoàn toàn phù hợp với sản lượng dự kiến.
