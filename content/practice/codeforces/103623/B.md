---
title: "CF 103623B - Sắp xếp bất thường"
description: "Giả sử $omega = e^{2pi i/3}$, vậy $omega^3 = 1$ và $1 + omega + omega^2 = 0$. Viết mỗi số nguyên không âm $k$ trong cơ số $3$ dưới dạng $$k = sum{j ge 0} kj 3^j, quad kj trong {0,1,2}."
date: "2026-07-02T22:44:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103623
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2022"
rating: 0
weight: 103623
solve_time_s: 133
verified: false
draft: false
---

[CF 103623B - Sắp xếp bất thường](https://codeforces.com/problemset/problem/103623/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\omega = e^{2\pi i/3}$, Vì thế$\omega^3 = 1$Và$1 + \omega + \omega^2 = 0$. Viết mỗi số nguyên không âm$k$trong căn cứ$3$BẰNG$$k = \sum_{j \ge 0} k_j 3^j, \quad k_j \in \{0,1,2\}.$$Vì$x \in [0,1)$xác định khai triển bậc ba của nó$$x = \sum_{j \ge 1} x_j 3^{-j}, \quad x_j \in \{0,1,2\},$$chọn cách biểu diễn mà cuối cùng không phải là$2$. 

Xác định$j$hàm số thứ ba$$\tau_j(x) = \lfloor 3^j x \rfloor \bmod 3,$$Vì thế$\tau_j(x) = x_j$và mỗi$\tau_j(x)$chỉ phụ thuộc vào khoảng ba chiều của độ dài$3^{-j}$chứa đựng$x$. 

Đối với mỗi$k \ge 0$, định nghĩa hàm Walsh bậc ba$$w_k(x) = \omega^{\sum_{j \ge 0} k_j \tau_{j+1}(x)}.$$Biểu thức này được xác định rõ vì chỉ có hữu hạn nhiều chữ số$k_j$khác 0 nên số mũ là tổng hữu hạn trong$\mathbb{Z}/3\mathbb{Z}$. 

Để cố định$j$, hàm$\tau_j(x)$là hằng số trên mỗi khoảng$$\left[\frac{m}{3^j}, \frac{m+1}{3^j}\right), \quad 0 \le m < 3^j,$$do đó mỗi$w_k(x)$là hằng số trên các khoảng ba chiều có độ dài$3^{-m}$, Ở đâu$m = 1 + \max{j : k_j \ne 0}$. 

Nếu như$k$Và$\ell$có cơ sở-$3$mở rộng$k_j$Và$\ell_j$, sau đó$$w_k(x)\, w_\ell(x) = \omega^{\sum_{j \ge 0} (k_j + \ell_j)\tau_{j+1}(x)}
= w_{k \oplus_3 \ell}(x),$$Ở đâu$\oplus_3$biểu thị phép cộng số theo modulo$3$. Điều này xác định gia đình${w_k}$với các đặc tính của nhóm phụ gia$\bigoplus_{j \ge 0} \mathbb{Z}/3\mathbb{Z}$được đánh giá trên các hàm tọa độ$\tau_j(x)$. 

Tính trực giao xuất phát từ sự độc lập của các chữ số bậc ba. Vì$k \ne 0$, chọn$m$như vậy$k_m \ne 0$. Phân vùng$[0,1)$thành các khoảng có độ dài$3^{-m}$, trên mỗi số đó tất cả các chữ số$\tau_j(x)$vì$j \le m$được cố định ngoại trừ$\tau_m(x)$, nhận các giá trị$0,1,2$bằng nhau trên các khoảng nhỏ của độ dài$3^{-(m+1)}$. Trên một khoảng con như vậy, hàm$w_k(x)$thu được yếu tố$\omega^{k_m \tau_m(x)}$trong khi tất cả các yếu tố khác không đổi. Tổng hợp ba giá trị của$\tau_m(x)$cho$$1 + \omega^{k_m} + \omega^{2k_m} = 0,$$từ$k_m \in {1,2}$ngụ ý$\omega^{k_m}$là căn bậc ba nguyên thủy của sự thống nhất. Vì thế$$\int_0^1 w_k(x)\,dx = 0 \quad \text{for } k \ne 0.$$Vì$k = \ell$, cùng một kết quả phân rã số hóa$w_k(x)\overline{w_k(x)} = 1$, kể từ đây$$\int_0^1 w_k(x)\overline{w_k(x)}\,dx = 1.$$Vì$k \ne \ell$, áp dụng lập luận tương tự cho$w_k(x)\overline{w_\ell(x)} = w_{k \oplus_3 (-\ell)}(x)$trong modulo số học chữ số$3$, khác 0 ở một số vị trí chữ số, tạo ra sự hủy bỏ theo cách tương tự. Kể từ đây$$\int_0^1 w_k(x)\overline{w_\ell(x)}\,dx = 0 \quad (k \ne \ell).$$Các chức năng${w_k(x)}_{k \ge 0}$tạo thành một hệ thống trực chuẩn trong$L^2[0,1]$và mỗi hàm là một ký tự được xác định bởi base-$3$các chữ số chính xác như các hàm Walsh tương ứng với cơ số-$2$nhân vật. Điều này hoàn thành việc khái quát hóa bậc ba của hệ thống Walsh. ∎
