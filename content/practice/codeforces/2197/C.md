---
title: "CF 2197C - Trò chơi có phân số"
description: "Giá trị $V = 7frac{1}{16}$ tương ứng với thống kê chi bình phương được tính toán từ các danh mục $k = 11$, như trong biểu thức. (5). Do đó, số bậc tự do là $nu = k - 1 = 10$."
date: "2026-06-09T04:44:55+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2197
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1079 (Div. 2)"
rating: 1200
weight: 2197
solve_time_s: 130
verified: false
draft: false
---

[CF 2197C - Trò chơi có phân số](https://codeforces.com/problemset/problem/2197/C) 

**Đánh giá:** 1200 
**Tags:** trò chơi, tham lam, toán học 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
giá trị$V = 7\frac{1}{16}$tương ứng với thống kê chi bình phương được tính từ$k = 11$các loại, như trong phương trình. (5). Do đó số bậc tự do là$\nu = k - 1 = 10$. Để xác định liệu$V$cao đến mức không thể tin được, người ta hãy tham khảo dòng trong Bảng 1 tương ứng với$\nu = 10$, liệt kê các điểm phần trăm của phân bố chi bình phương cho 10 bậc tự do. Dòng này cung cấp các giá trị ngưỡng gần đúng mà$V$có thể được so sánh để đánh giá điểm cực trị của nó. 

Từ$7\frac{1}{16} = 7.0625$nằm giữa các mục 50 phần trăm và 75 phần trăm cho$\nu = 10$, nó không được coi là cao đáng kể; tuy nhiên, dòng thích hợp để sử dụng trong bảng là dòng có$\nu = 10$. 

∎
