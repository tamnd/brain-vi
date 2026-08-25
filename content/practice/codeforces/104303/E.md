---
title: "CF 104303E - \u8bfb\u4e2d\u56fd\u6570\u5b57"
description: "Giả sử $x trong [0,1)$ có khai triển bậc ba $x = 0,x1 x2 x3 cdots quad (xj trong {0,1,2}),$ trong đó sử dụng biểu diễn không kết thúc. Xác định $omega = e^{2pi i/3}$, do đó $omega^3 = 1$ và $1 + omega + omega^2 = 0$."
date: "2026-07-01T20:11:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104303
codeforces_index: "E"
codeforces_contest_name: "2023 Xiangtan Unversity Freshman Conteset"
rating: 0
weight: 104303
solve_time_s: 135
verified: false
draft: false
---

[CF 104303E - \u8bfb\u4e2d\u56fd\u6570\u5b57](https://codeforces.com/problemset/problem/104303/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$x \in [0,1)$có sự mở rộng bậc ba$x = 0.x_1 x_2 x_3 \cdots \quad (x_j \in \{0,1,2\}),$nơi sử dụng các biểu diễn không kết thúc. 

Định nghĩa$\omega = e^{2\pi i/3}$, Vì thế$\omega^3 = 1$Và$1 + \omega + \omega^2 = 0$. 

Đối với mỗi$j \ge 1$, xác định các hàm Rademacher bậc ba$r_j(x) = \omega^{x_j}.$Mỗi$r_j(x)$chỉ phụ thuộc vào$j$chữ số thứ ba và nhận các giá trị bằng${1,\omega,\omega^2}$. 

Cho phép$k$là một số nguyên không âm với biểu diễn bậc ba$k = k_0 + 3k_1 + 3^2 k_2 + \cdots,$nơi chỉ có hữu hạn nhiều$k_j$đều khác 0 và mỗi$k_j \in {0,1,2}$. 

Xác định hàm Walsh bậc ba$w_k(x)$qua$w_k(x) = \prod_{j \ge 1} r_j(x)^{k_{j-1}}.$Tương đương,$w_k(x) = \omega^{\sum_{j \ge 1} k_{j-1} x_j} = \omega^{\sum_{j \ge 0} k_j x_{j+1}}.$Số mũ được lấy modulo$3$, từ$\omega^m$chỉ phụ thuộc vào$m \bmod 3$. 

Đối với mỗi cố định$k$, tích là hữu hạn vì$k_j = 0$cho tất cả đủ lớn$j$. 

Cho phép$m \ge 1$và hạn chế ở$\sigma$-đại số được tạo ra bởi lần đầu tiên$m$chữ số ba. Sau đó$w_k$chỉ phụ thuộc vào$x_1,\dots,x_m$bất cứ khi nào$k_j = 0$vì$j \ge m$. 

Đối với số nguyên$k,\ell$với chữ số ba$(k_j)$Và$(\ell_j)$, sản phẩm thỏa mãn$w_k(x)\,\overline{w_\ell(x)} = \omega^{\sum_{j \ge 1} (k_{j-1}-\ell_{j-1})x_j}.$Tính trực giao được tính bằng cách lấy tích phân từng chữ số. Đối với mỗi cố định$j$,$\int_0^1 \omega^{a x_j}\,dx = \frac{1}{3}(1 + \omega^a + \omega^{2a})$Ở đâu$a \in {0,1,2}$. Điều này bằng$1$khi$a \equiv 0 \pmod 3$và bằng$0$nếu không thì. 

Vì các chữ số bậc ba$x_j$độc lập và phân bố đều theo độ đo Lebesgue trên$[0,1)$, tích phân phân tích thành nhân tử: 

= \prod_{j \ge 1} \frac{1}{3}\left(1 + \omega^{k_{j-1}-\ell_{j-1}} + \omega^{2(k_{j-1}-\ell_{j-1})}\right).$$ Each factor equals $1$ if $k_{j-1} = \ell_{j-1}$ and equals $0$ otherwise. The product is therefore $1$ when $k = \ell$ and $0$ when $k \ne \ell$, yielding $$\int_0^1 w_k(x)\,\overline{w_\ell(x)}\,dx = \delta_{k\ell}.$$ Hệ thống$\{w_k\}$hoàn thành trong$L^2[0,1)$bởi vì nó trùng với hệ thống ký tự của nhóm abelian compact$\prod_{j \ge 1} \mathbb{Z}/3\mathbb{Z}$dưới sự nhận dạng được đưa ra bởi sự mở rộng bậc ba và các ký tự tạo thành một cơ sở trực giao cho tương ứng$L^2$không gian. Dưới bản đồ chữ số giữa$[0,1)$và trình tự ba ngôi, nhóm này đẳng cấu bảo toàn số đo với khoảng đơn vị với độ đo Lebesgue. Do đó, hệ thống Walsh ba ngôi có được bằng cách thay thế các chữ số nhị phân và nhóm dấu$\{\pm 1\}$với các chữ số ba ngôi và nhóm nhân của nghiệm thứ ba của đơn vị, bảo toàn tính trực giao và tính đầy đủ thông qua tính độc lập về mặt số hóa. Điều này hoàn thành việc xây dựng khái quát bậc ba của các hàm Walsh. ∎
