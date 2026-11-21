---
title: "CF 2202B - ABAB Construction"
description: "Giá trị $V = 7frac{1}{16}$ tương ứng với thống kê chi bình phương được tính toán từ các danh mục $k = 11$, như trong biểu thức. (5). Do đó số bậc tự do là $nu = k - 1 = 10$."
date: "2026-06-09T04:49:17+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2202
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1082 (Div. 2)"
rating: 1200
weight: 2202
solve_time_s: 128
verified: false
draft: false
---

[CF 2202B - Xây dựng ABAB](https://codeforces.com/problemset/problem/2202/B) 

**Đánh giá:** 1200 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
giá trị$V = 7\frac{1}{16}$tương ứng với thống kê chi bình phương được tính từ$k = 11$các loại, như trong phương trình. (5). Do đó số bậc tự do là$\nu = k - 1 = 10$. Để xác định liệu$V$là cao không thể tin được, người ta tham khảo dòng trong Bảng 1 tương ứng với$\nu = 10$, liệt kê các điểm phần trăm của phân bố chi bình phương cho 10 bậc tự do. Dòng này cung cấp các giá trị ngưỡng gần đúng mà$V$có thể được so sánh để đánh giá điểm cực trị của nó. 

Từ$7\frac{1}{16} = 7.0625$nằm giữa các mục 50 phần trăm và 75 phần trăm cho$\nu = 10$, nó không được coi là cao đáng kể; tuy nhiên, dòng thích hợp để sử dụng trong bảng là dòng có$\nu = 10$. 

∎
