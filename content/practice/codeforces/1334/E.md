---
title: "CF 1334E - Đường chia số"
description: "Chúng ta được cho một số $D$ và cần suy luận về tất cả các ước của nó. Bài toán xác định một đồ thị có các đỉnh đều là ước của $D$. Một cạnh tồn tại từ ước số $y$ đến ước số lớn hơn $x$ nếu $x$ chia hết cho $y$ và thương $x / y$ là số nguyên tố."
date: "2026-06-11T15:59:29+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "graphs", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1334
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 85 (Rated for Div. 2)"
rating: 2200
weight: 1334
solve_time_s: 49
verified: false
draft: false
---

[CF 1334E - Đường chia chia](https://codeforces.com/problemset/problem/1334/E) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, đồ thị, tham lam, toán, lý thuyết số 
**Thời gian giải:** 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$D$và cần suy luận về tất cả các ước của nó. Bài toán xác định một đồ thị có các đỉnh đều là ước của$D$. Một cạnh tồn tại từ một số chia$y$đến một số chia lớn hơn$x$nếu như$x$chia hết cho$y$và thương số$x / y$là nguyên tố. Trọng số của cạnh đó là số ước của$x$đó không phải là ước của$y$. Nhiệm vụ là, đối với nhiều truy vấn, mỗi truy vấn yêu cầu một cặp ước số$v$Và$u$, để đếm số đường đi ngắn nhất giữa chúng trong biểu đồ này. 

Những hạn chế là đáng kể:$D$có thể lớn như$10^{15}$và có tới$3 \cdot 10^5$truy vấn. Liệt kê tất cả các ước của$D$hoặc xây dựng biểu đồ đầy đủ một cách rõ ràng là không khả thi đối với số lượng lớn
