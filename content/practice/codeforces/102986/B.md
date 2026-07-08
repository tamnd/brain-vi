---
title: "CF 102986B - Chia sẻ cổ vũ"
description: "Giả sử $alpha$ là một tổ hợp $t$-, vì vậy $alpha$ là tập hợp con phần tử $t$ của ${0,1,dots,n-1}$. Toán tử $partialt alpha$ tạo ra tất cả các kết hợp $(t-1)$-có được bằng cách xóa một phần tử của $alpha$. Nếu $alpha={ct,dots,c1}$, thì $$partialt alpha={alphasetminus{cj}mid 1le jle t}."
date: "2026-07-04T02:56:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102986
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 03-05-21 Div. 2 (Beginner)"
rating: 0
weight: 102986
solve_time_s: 151
verified: false
draft: false
---

[CF 102986B - Chia sẻ cổ vũ](https://codeforces.com/problemset/problem/102986/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

## Giải pháp 
hãy để$\alpha$là một$t$-sự kết hợp, vậy$\alpha$là một$t$-tập hợp phần tử của${0,1,\dots,n-1}$. 

Người điều hành$\partial_t \alpha$sản xuất tất cả$(t-1)$-sự kết hợp thu được bằng cách xóa một phần tử của$\alpha$. Nếu như$\alpha={c_t,\dots,c_1}$, sau đó$$\partial_t \alpha=\{\alpha\setminus\{c_j\}\mid 1\le j\le t\}.$$Mỗi phần tử của$\partial_t \alpha$do đó là một$(t-1)$-sự kết hợp của${0,1,\dots,n-1}$. 

Người điều hành$\partial_{t+1} \alpha$sản xuất tất cả$(t+1)$-sự kết hợp có chứa$\alpha$, thu được bằng cách nối liền một phần tử mới chưa có trong$\alpha$. Nếu như$\overline{\alpha}={0,1,\dots,n-1}\setminus \alpha$, sau đó$$\partial_{t+1} \alpha=\{\alpha\cup\{x\}\mid x\in \overline{\alpha}\}.$$Mỗi phần tử của$\partial_{t+1} \alpha$do đó là một$(t+1)$-sự kết hợp có chứa$\alpha$.
