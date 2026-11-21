---
title: "CF 2197F - Chuỗi ngoặc kép"
description: "Giá trị $V = 7frac{1}{16}$ tương ứng với thống kê chi bình phương được tính toán từ các danh mục $k = 11$, như trong biểu thức. (5). Do đó, số bậc tự do là $nu = k - 1 = 10$."
date: "2026-06-09T04:47:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "flows", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2197
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1079 (Div. 2)"
rating: 2500
weight: 2197
solve_time_s: 125
verified: false
draft: false
---

[CF 2197F - Chuỗi ngoặc kép](https://codeforces.com/problemset/problem/2197/F) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, dp, dòng chảy, tham lam 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
giá trị$V = 7\frac{1}{16}$tương ứng với thống kê chi bình phương được tính từ$k = 11$các loại, như trong phương trình. (5). Do đó số bậc tự do là$\nu = k - 1 = 10$. Để xác định liệu$V$cao đến mức không thể tin được, người ta hãy tham khảo dòng trong Bảng 1 tương ứng với$\nu = 10$, liệt kê các điểm phần trăm của phân bố chi bình phương cho 10 bậc tự do. Dòng này cung cấp các giá trị ngưỡng gần đúng mà$V$có thể được so sánh để đánh giá điểm cực trị của nó. 

Từ$7\frac{1}{16} = 7.0625$nằm giữa các mục 50 phần trăm và 75 phần trăm cho$\nu = 10$, nó không được coi là cao đáng kể; tuy nhiên, dòng thích hợp để sử dụng trong bảng là dòng có$\nu = 10$. 

∎
