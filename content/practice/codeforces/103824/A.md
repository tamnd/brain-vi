---
title: "CF 103824A - \u5361\u5c14\u7684\u793c\u7269"
description: "Đặt $f(x1,ldots,xn)$ là một hàm Boolean và đặt $$G(z)=sum{x1=0}^1 cdots sum{xn=0}^1 z^{x1+cdots+xn} f(x1,ldots,xn)$$ là hàm tạo của nó như được định nghĩa trong bài tập trước."
date: "2026-07-02T08:17:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103824
codeforces_index: "A"
codeforces_contest_name: "2022 Summer Camp of XTU Qualifying Round"
rating: 0
weight: 103824
solve_time_s: 42
verified: false
draft: false
---

[CF 103824A - \u5361\u5c14\u7684\u793c\u7269](https://codeforces.com/problemset/problem/103824/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$f(x_1,\ldots,x_n)$là một hàm Boolean và để$$G(z)=\sum_{x_1=0}^1 \cdots \sum_{x_n=0}^1 z^{x_1+\cdots+x_n} f(x_1,\ldots,x_n)$$là hàm tạo của nó như được định nghĩa trong bài tập trước. 

Cho phép$$F(p_1,\ldots,p_n)=\sum_{x_1=0}^1 \cdots \sum_{x_n=0}^1
\prod_{k=1}^n (1-p_k)^{1-x_k} p_k^{x_k}\, f(x_1,\ldots,x_n)$$là đa thức độ tin cậy. 

Chuyên về vụ án$p_1=\cdots=p_n=p$. Khi đó mọi số hạng trong tích sẽ trở thành$(1-p)^{1-x_k}p^{x_k}$, do đó trọng lượng của một vectơ$x=(x_1,\ldots,x_n)$chỉ phụ thuộc vào$s=x_1+\cdots+x_n$. Sản phẩm được đơn giản hóa để$$\prod_{k=1}^n (1-p)^{1-x_k}p^{x_k} = (1-p)^{n-s} p^s.$$Định nghĩa$$A_s = \sum_{x_1=0}^1 \cdots \sum_{x_n=0}^1 [x_1+\cdots+x_n=s]\; f(x_1,\ldots,x_n),$$để có thể$A_s$tính (với trọng lượng$f$) tất cả các bài tập có chính xác$s$những cái đó. 

Khi đó đa thức độ tin cậy trở thành$$F(p)=\sum_{s=0}^n A_s p^s (1-p)^{n-s}.$$Hàm tạo thỏa mãn$$G(z)=\sum_{s=0}^n A_s z^s,$$kể từ khi nhóm các thuật ngữ theo trọng số Hamming$s$tạo ra các hệ số giống hệt nhau$A_s$. 

Bây giờ viết lại$F(p)$bằng cách bao thanh toán$(1-p)^n$:$$F(p)=(1-p)^n \sum_{s=0}^n A_s \left(\frac{p}{1-p}\right)^s.$$Tổng bên trong là$G!\left(\frac{p}{1-p}\right)$bằng cách thay thế vào hàm tạo. 

Vì thế$$F(p)=(1-p)^n G\!\left(\frac{p}{1-p}\right).$$Điều này biểu thị đa thức độ tin cậy với các tham số bằng nhau trực tiếp theo hàm tạo.$$\boxed{F(p)=(1-p)^n\, G\!\left(\frac{p}{1-p}\right)}$$Điều này hoàn thành giải pháp. ∎
