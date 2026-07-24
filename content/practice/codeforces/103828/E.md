---
title: "CF 103828E - Bạn có Naseem ở đâu không?"
description: "Đặt $G(z)=sum{x1=0}^{1}cdotssum{xn=0}^{1} z^{x1+cdots+xn} f(x1,ldots,xn)$ là hàm sinh được xác định trong Bài tập 25, và đặt $F(p)$ biểu thị đa thức độ tin cậy khi $p1=cdots=pn=p$, sao cho $$F(p)=sum{x1=0}^{1}cdotssum{xn=0}^{1} (1-p)^{1-x1}p^{x1}cdots…"
date: "2026-07-02T08:14:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103828
codeforces_index: "E"
codeforces_contest_name: "(DCPC + TCPC + BCPC) 2022"
rating: 0
weight: 103828
solve_time_s: 95
verified: false
draft: false
---

[CF 103828E - Bạn có Naseem ở đâu không?](https://codeforces.com/problemset/problem/103828/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$G(z)=\sum_{x_1=0}^{1}\cdots\sum_{x_n=0}^{1} z^{x_1+\cdots+x_n} f(x_1,\ldots,x_n)$là hàm sinh được xác định trong Bài tập 25 và đặt$F(p)$biểu thị đa thức độ tin cậy khi$p_1=\cdots=p_n=p$, để có thể$$F(p)=\sum_{x_1=0}^{1}\cdots\sum_{x_n=0}^{1} (1-p)^{1-x_1}p^{x_1}\cdots (1-p)^{1-x_n}p^{x_n} f(x_1,\ldots,x_n).$$Đối với một vectơ cố định$x=(x_1,\ldots,x_n)$, các hệ số sản phẩm theo trọng số Hamming$w(x)=x_1+\cdots+x_n$, cho$$(1-p)^{1-x_1}p^{x_1}\cdots (1-p)^{1-x_n}p^{x_n}=(1-p)^{n-w(x)}p^{w(x)}.$$Sản lượng thay thế$$F(p)=\sum_{x} f(x)\, (1-p)^{n-w(x)} p^{w(x)}.$$Bao thanh toán ra$(1-p)^n$sản xuất$$F(p)=(1-p)^n \sum_{x} f(x)\left(\frac{p}{1-p}\right)^{w(x)}.$$Tổng còn lại phù hợp với định nghĩa của$G(z)$đánh giá tại$z=\frac{p}{1-p}$, từ$z^{w(x)}$xuất hiện với cùng cấu trúc số mũ. Vì thế,$$F(p)=(1-p)^n G\!\left(\frac{p}{1-p}\right).$$Điều này thể hiện$F(p)$như một phép thay đổi tỷ lệ đơn giản của hàm tạo, thu được bằng một phép thay thế duy nhất, sau đó là phép nhân với một hệ số đơn thức. Điều này hoàn thành giải pháp. ∎
