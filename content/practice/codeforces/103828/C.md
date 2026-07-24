---
title: "CF 103828C - Basharo không hề xấu"
description: "Giả sử $H$ là ma trận kiểm tra chẵn lẻ $mtimes n$ trên $mathbb{F}2$ và đặt $$f(x)= [Hx=0], qquad x=(x1,dots,xn)^T.$$ Cố định một thứ tự biến $x1,dots,xn$."
date: "2026-07-02T08:13:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103828
codeforces_index: "C"
codeforces_contest_name: "(DCPC + TCPC + BCPC) 2022"
rating: 0
weight: 103828
solve_time_s: 53
verified: false
draft: false
---

[CF 103828C - Basharo không xấu](https://codeforces.com/problemset/problem/103828/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$H$là một$m\times n$ma trận kiểm tra chẵn lẻ kết thúc$\mathbb{F}_2$, và để$$f(x)= [Hx=0], \qquad x=(x_1,\dots,x_n)^T.$$Sửa một thứ tự thay đổi$x_1,\dots,x_n$. Vì$0\le k\le n$, viết$H_k$cho$m\times k$ma trận con gồm có ma trận đầu tiên$k$cột của$H$, và xác định$$r_k = \operatorname{rank}(H_k).$$Đối với một phần nhiệm vụ$u=(x_1,\dots,x_k)$, hội chứng là$$s(u)=H_k u \in \mathbb{F}_2^m.$$Hàm con gây ra bởi$u$về các biến còn lại$x_{k+1},\dots,x_n$là$$f_u(x_{k+1},\dots,x_n) = [H_{k+1..n}(x_{k+1},\dots,x_n) = s(u)].$$Hai bài tập$u,u'$mang lại hàm con tương tự khi và chỉ khi$s(u)=s(u')$, vì sự bình đẳng của các hội chứng đưa ra các ràng buộc affine giống hệt nhau đối với các biến còn lại. 

Bản đồ$u\mapsto s(u)$có hình ảnh bằng không gian cột của$H_k$, có bản số$2^{r_k}$. Do đó số lượng các chức năng con riêng biệt ở cấp độ$k$trong BDD bằng$2^{r_k}$. 

Mỗi chức năng con riêng biệt tương ứng với một nút BDD duy nhất ở cấp độ$k$. Do đó số lượng nút nonterminal bằng$$\sum_{k=0}^{n-1} 2^{r_k},$$từ$k=n$chỉ đóng góp các chức năng con đầu cuối. 

Ở cấp độ$n$, mọi phép gán đều mang lại tính nhất quán$s(u)=0$hoặc sự không nhất quán$s(u)\ne 0$, do đó tất cả các lá nhất quán sẽ hợp nhất thành một$\top$nút và tất cả các lá không nhất quán sẽ hợp nhất thành một nút duy nhất$\bot$nút. Điều này góp phần chính xác$2$các nút chìm. 

Kể từ đây$$B(f)=\sum_{k=0}^{n-1} 2^{r_k} + 2.$$Từ$r_0=0$, điều này tương đương$$B(f)=3+\sum_{k=1}^{n-1} 2^{r_k}.$$Đối với mã Hamming,$n=2^m-1$, Và$H$có các cột tất cả các vectơ khác 0 của$\mathbb{F}_2^m$. Với thứ tự tiêu chuẩn trong đó đầu tiên$m$các cột tạo thành một ma trận khả nghịch, việc tăng thứ hạng thỏa mãn$$r_k = k \quad (1\le k\le m), \qquad r_k=m \quad (m\le k\le n).$$Thay vào công thức sẽ cho$$B(f)=\sum_{k=0}^{m}2^k + \sum_{k=m+1}^{n-1}2^m + 2.$$Tổng đầu tiên là$$\sum_{k=0}^{m}2^k = 2^{m+1}-1.$$Tổng thứ hai chứa$n-1-m$điều khoản, do đó bằng$$(n-1-m)2^m.$$Với$n=2^m-1$, điều này trở thành$$(2^m-2-m)2^m.$$Vì thế$$B(f)=(2^{m+1}-1) + (2^m-2-m)2^m + 2.$$Đơn giản hóa,$$(2^{m+1}-1)+2 = 2^{m+1}+1,$$Vì thế$$B(f)=2^{m+1}+1 + 2^{2m} - (m+2)2^m.$$Từ$2^{m+1} = 2\cdot 2^m$, điều này trở thành$$B(f)=2^{2m} - m2^m + 1.$$Do đó đối với mã Hamming,$$\boxed{B(f)=2^{2m}-m2^m+1}.$$Đối với phần (c), từ nhận được$y=(y_1,\dots,y_n)$và xác suất kênh độc lập$p_k=\Pr[y_k=x_k]$tạo ra trọng số khả năng cho các bài tập$x$:$$\Pr(x\mid y) \propto \prod_{k=1}^n \bigl(p_k^{[x_k=y_k]} (1-p_k)^{[x_k\ne y_k]}\bigr).$$Từ mã MAP tối đa hóa sản phẩm này trên tất cả$x$thỏa mãn$f(x)=1$. Trong BDD của$f$, mỗi gốc tới-$\top$đường dẫn tương ứng với một từ mã. Gán từng cạnh HI ở cấp độ$k$yếu tố$$p_k \text{ if } y_k=1,\quad (1-p_k) \text{ if } y_k=0,$$và mỗi cạnh LO là thừa số bù. 

Từ mã MAP thu được bằng cách tính toán đường dẫn có trọng số tối đa từ gốc tới$\top$, trong đó trọng số đường đi là tích của các trọng số cạnh. Tương tự, việc lấy logarit sẽ chuyển đổi vấn đề này thành bài toán đường đi dài nhất trong BDD theo chu kỳ:$$\sum \log(\text{edge weight}).$$Lập trình động trên BDD, đánh giá các nút theo thứ tự tôpô, mang lại giá trị tối ưu ở gốc và quay lui mang lại từ mã tương ứng. 

Điều này hoàn thành giải pháp. ∎
