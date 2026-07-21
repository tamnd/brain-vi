---
title: "CF 103640B - Bởi vì, nghệ thuật!"
description: "Đặt $x trong [0,1)$ và viết khai triển nhị phân của nó $$x = 0.x1 x2 x3 ldots,qquad xj trong {0,1}.$$ Đặt $rj(x)$ biểu thị hàm Rademacher $j$-th, $$rj(x) = (-1)^{xj}."
date: "2026-07-02T22:15:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103640
codeforces_index: "B"
codeforces_contest_name: "2021-2022 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 103640
solve_time_s: 128
verified: false
draft: false
---

[CF 103640B - Bởi vì, Nghệ thuật!](https://codeforces.com/problemset/problem/103640/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$x \in [0,1)$và viết khai triển dyadic của nó$$x = 0.x_1 x_2 x_3 \ldots,\qquad x_j \in \{0,1\}.$$Cho phép$r_j(x)$biểu thị$j$-Chức năng Rademacher thứ,$$r_j(x) = (-1)^{x_j}.$$Cho phép$k$có khai triển nhị phân$$k = (b_m \cdots b_1 b_0)_2,\qquad b_j \in \{0,1\},$$và để hàm Walsh được xác định bởi$$w_k(x) = \prod_{j \ge 0} r_{j+1}(x)^{b_j}.$$### Chuyển đổi theo$x \mapsto 1-x$Đối với việc mở rộng dyadic, thay thế$x$qua$1-x$lật từng chữ số nhị phân theo nghĩa mỗi hàm Rademacher đổi dấu:$$r_j(1-x) = -r_j(x),$$kể từ khi$j$-chữ số nhị phân thứ được bổ sung dưới$x \mapsto 1-x$trong hệ thống dyadic, do đó$(-1)^{(1-x_j)} = -(-1)^{x_j}$. 

Áp dụng điều này vào$w_k$,$$w_k(1-x)
= \prod_{j \ge 0} r_{j+1}(1-x)^{b_j}
= \prod_{j \ge 0} (-r_{j+1}(x))^{b_j}.$$Mỗi yếu tố đóng góp một dấu hiệu$-1$chính xác khi nào$b_j = 1$, kể từ đây$$w_k(1-x)
= (-1)^{\sum_{j \ge 0} b_j} \prod_{j \ge 0} r_{j+1}(x)^{b_j}.$$Cho phép$\nu(k) = \sum_{j \ge 0} b_j$là số lượng$1$-bit trong khai triển nhị phân của$k$. Sau đó$$w_k(1-x) = (-1)^{\nu(k)} w_k(x).$$### So sánh với danh tính được xác nhận 

Tuyên bố để kiểm tra là$$w_k(-x) = (-1)^k w_k(x).$$Vì các hàm Walsh thường được xác định trên$[0,1)$và được gia hạn định kỳ,$-x$tương ứng với$1-x$trong cài đặt này. Danh tính dẫn xuất cho thấy số mũ chính xác phụ thuộc vào trọng số Hamming$\nu(k)$, không bật$k$chính nó. 

Để bác bỏ yêu cầu bồi thường, hãy lấy$k=2$. Sau đó$k=(10)_2$, Vì thế$\nu(k)=1$. Nhận dạng trên mang lại$$w_2(1-x) = -w_2(x),$$trong khi$$(-1)^k = (-1)^2 = 1.$$Kể từ đây$$w_2(1-x) \ne (-1)^k w_2(x)$$cho tất cả$x$Ở đâu$w_2(x) \ne 0$. 

Danh tính được xác nhận không thành công.$$\boxed{\text{False}}$$
