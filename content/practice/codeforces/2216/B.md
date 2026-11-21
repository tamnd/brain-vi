---
title: "CF 2216B - Câu đố đóng gói THU"
description: "Giá trị $V = 7frac{1}{16}$ tương ứng với thống kê chi bình phương được tính toán từ các danh mục $k = 11$, như trong biểu thức. (5). Do đó, số bậc tự do là $nu = k - 1 = 10$."
date: "2026-06-09T04:52:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2216
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1092 (Unrated, Div. 2, Based on THUPC 2026 \u2014 Finals)"
rating: 1300
weight: 2216
solve_time_s: 40
verified: false
draft: false
---

[CF 2216B - Câu đố đóng gói THU](https://codeforces.com/problemset/problem/2216/B) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** không 

## Giải pháp 
giá trị$V = 7\frac{1}{16}$tương ứng với thống kê chi bình phương được tính từ$k = 11$các loại, như trong phương trình. (5). Do đó số bậc tự do là$\nu = k - 1 = 10$. Để xác định liệu$V$cao đến mức không thể tin được, người ta hãy tham khảo dòng trong Bảng 1 tương ứng với$\nu = 10$, liệt kê các điểm phần trăm của phân bố chi bình phương cho 10 bậc tự do. Dòng này cung cấp các giá trị ngưỡng gần đúng mà$V$có thể được so sánh để đánh giá điểm cực trị của nó. 

Từ$7\frac{1}{16} = 7.0625$nằm giữa các mục 50 phần trăm và 75 phần trăm cho$\nu = 10$, nó không được coi là cao đáng kể; tuy nhiên, dòng thích hợp để sử dụng trong bảng là dòng có$\nu = 10$. 

∎
