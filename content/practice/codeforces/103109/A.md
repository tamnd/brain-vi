---
title: "CF 103109A - Hoán vị Pok\u00e9mon"
description: "Giả sử $kappat$ là hàm được xác định trong phần này, với $mut$ nghịch đảo theo nghĩa là $$M ge mut N quad Longleftrightarrow quad kappat(M) ge N,$$ cho $t ge 2$."
date: "2026-07-03T21:13:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103109
codeforces_index: "A"
codeforces_contest_name: "mBIT Advanced Spring 2021"
rating: 0
weight: 103109
solve_time_s: 156
verified: false
draft: false
---

[CF 103109A - Hoán vị Pok\u00e9mon](https://codeforces.com/problemset/problem/103109/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\kappa_t$là hàm được xác định trong phần, với nghịch đảo$\mu_t$theo nghĩa đó$$M \ge \mu_t N \quad \Longleftrightarrow \quad \kappa_t(M) \ge N,$$vì$t \ge 2$. Cho phép$\lambda_{t-1} M$biểu thị$(t-1)$- mức đóng góp trong biểu diễn nhị thức của$\kappa_t(M)$, do đó việc phân rã xác định từ việc xây dựng$\kappa_t$cho$$\kappa_t(M) = M + \lambda_{t-1} M.$$Danh tính này xuất phát từ sự đại diện của$\kappa_t(M)$dưới dạng tổng của các đóng góp nhị thức trong đó số hạng cấp cao nhất là$M$và phần đóng góp còn lại chính xác là$(t-1)$-Cấu trúc áp dụng cho$M$. 

Giả sử đầu tiên rằng$M \ge \mu_t N$. Bằng tính chất xác định của$\mu_t$, điều này tương đương với$\kappa_t(M) \ge N$. Thay thế sự phân hủy của$\kappa_t(M)$sản lượng$$M + \lambda_{t-1} M \ge N.$$Ngược lại, giả sử$M + \lambda_{t-1} M \ge N$. Viết lại vế trái bằng cách sử dụng cùng cách phân tách sẽ cho$\kappa_t(M) \ge N$, do đó bằng cách xác định sự tương đương của$\mu_t$,$$M \ge \mu_t N.$$Cả hai hàm ý đều có thể đảo ngược vì mỗi bước sử dụng một đẳng thức trong việc phân tách$\kappa_t(M)$và sự tương đương xác định giữa$\mu_t$Và$\kappa_t$. Vì thế,$$M \ge \mu_t N \quad \Longleftrightarrow \quad M + \lambda_{t-1} M \ge N.$$Điều này hoàn thành việc chứng minh. ∎
