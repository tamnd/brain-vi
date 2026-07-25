---
title: "CF 103870Q - Ngộ độc thực phẩm"
description: "Giả sử $H$ là một ma trận nhị phân $m nhân n$ và đặt $$f(x) = [Hx = 0],$$ trong đó số học lớn hơn $mathbb{F}2$. BDD cho $f$ được xây dựng theo thứ tự cố định của các biến $x1,dots,xn$ như trong Phần 7.1.4."
date: "2026-07-02T07:49:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103870
codeforces_index: "Q"
codeforces_contest_name: "TeamsCode Summer 2022 Contest"
rating: 0
weight: 103870
solve_time_s: 31
verified: false
draft: false
---

[CF 103870Q - Ngộ độc thực phẩm](https://codeforces.com/problemset/problem/103870/Q) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$H$là một$m \times n$ma trận nhị phân và để$$f(x) = [Hx = 0],$$nơi số học kết thúc$\mathbb{F}_2$. BDD dành cho$f$được xây dựng theo một thứ tự cố định của các biến$x_1,\dots,x_n$như trong Phần 7.1.4. Mỗi nút ở cấp độ$k$tương ứng với một hàm con thu được bằng cách sửa$x_1,\dots,x_{k-1}$. 

Ở cấp độ$k$, điều kiện còn lại trên các biến không cố định$x_k,\dots,x_n$được xác định bởi hệ tuyến tính dư thu được bằng cách loại bỏ phần đầu tiên$k-1$cột của$H$sử dụng các giá trị cố định. 

Đối với một phần nhiệm vụ$x_1,\dots,x_{k-1}$, viết vectơ$$u = \sum_{j=1}^{k-1} x_j H_j,$$Ở đâu$H_j$là$j$-cột thứ của$H$. Ràng buộc$Hx=0$trở thành$$\sum_{j=k}^{n} x_j H_j = u,$$hoặc tương đương$$H^{(k)} x^{(k)} = u,$$Ở đâu$H^{(k)}$là ma trận con gồm các cột$k,\dots,n$, Và$x^{(k)} = (x_k,\dots,x_n)^T$. 

Do đó, mỗi nút trong BDD ở cấp độ$k$được xác định duy nhất bởi một vector hội chứng có thể tiếp cận$u$trong khoảng cột của ma trận tiền tố$H_{1..k-1}$, cùng với ma trận còn lại$H^{(k)}$. 

Hai phép gán một phần mang lại cùng một nút BDD ở cấp độ$k$nếu và chỉ nếu chúng tạo ra cùng một hội chứng$u$, vì điều kiện tiếp tục chỉ phụ thuộc vào$u$. Do đó các nút ở mức$k$tương ứng một-một với không gian hình ảnh$$U_k = \{u : u = \sum_{j=1}^{k-1} x_j H_j,\ x_j \in \{0,1\}\}.$$Bản đồ$x_1,\dots,x_{k-1} \mapsto u$tuyến tính trên$\mathbb{F}_2$, Vì thế$U_k$là không gian vectơ được tạo bởi lần đầu tiên$k-1$cột của$H$. Vì thế$$|U_k| = 2^{\operatorname{rank}(H_{1..k-1})}.$$Mỗi hội chứng riêng biệt$u \in U_k$tương ứng với chính xác một nút BDD ở cấp độ$k$, vì việc rút gọn xác định các chức năng con có hệ thống tiếp tục giống hệt nhau và việc sắp xếp ngăn cản việc sử dụng lại ở các cấp độ khác nhau. 

Do đó số lượng nút ở mức$k$bằng$2^{r_k}$, Ở đâu$r_k = \operatorname{rank}(H_{1..k-1})$. Tổng kích thước BDD là$$B(f) = 1 + 1 + \sum_{k=1}^{n} 2^{r_k},$$trong đó hai chữ cái đầu tiên$1$các thuật ngữ tương ứng với các nút chìm$\bot$Và$\top$, vì cả hai đều có trong BDD giảm. 

Đối với mã Hamming,$n = 2^m - 1$, Và$H$là$m \times n$ma trận có các cột đều là vectơ khác 0 trong$\mathbb{F}_2^m$. Bất kỳ bộ nào$t$các cột riêng biệt độc lập tuyến tính khi và chỉ khi nó chứa nhiều nhất$m$các cột, vì mọi sự phụ thuộc giữa các vectơ khác 0 trong$\mathbb{F}_2^m$có kích thước ít nhất$m+1$. Do đó với mọi tiền tố chứa$k-1 < m$cột,$$r_k = k-1.$$Một lần$k-1 \ge m$, thứ hạng ổn định tại$m$, vì tất cả$\mathbb{F}_2^m$được kéo dài. 

Vì thế$$r_k =
\begin{cases}
k-1 & k \le m+1,\\
m & k \ge m+1.
\end{cases}$$Thay thế vào công thức kích thước cho$$B(f) = 2 + \sum_{k=1}^{m} 2^{k-1} + \sum_{k=m+1}^{2^m-1} 2^m.$$Tổng đầu tiên ước tính là$$\sum_{k=1}^{m} 2^{k-1} = 2^m - 1.$$Tổng thứ hai chứa$2^m - 1 - m$điều khoản, mỗi điều khoản bằng$2^m$, do đó bằng$$(2^m - 1 - m)2^m.$$Kết hợp các thuật ngữ,$$B(f) = 2 + (2^m - 1) + (2^m - 1 - m)2^m.$$Đơn giản hóa,$$B(f) = 1 + 2^m + (2^m - 1 - m)2^m.$$Mở rộng,$$B(f) = 1 + 2^m + 2^{2m} - 2^m - m2^m.$$Lợi nhuận hủy bỏ$$B(f) = 1 + 2^{2m} - m2^m.$$Do đó đối với mã Hamming,$$\boxed{B(f) = 2^{2m} - m2^m + 1}.$$Để giải mã khả năng tối đa, từ nhận được$y$xác định trọng số khả năng cho mỗi từ mã ứng viên$x$,$$P(y \mid x) = \prod_{k=1}^n p_k^{[x_k = y_k]} (1-p_k)^{[x_k \ne y_k]}.$$Tối đa hóa điều này tương đương với việc tối đa hóa khả năng ghi nhật ký$$\sum_{k=1}^n \left( x_k \log \frac{p_k}{1-p_k} \cdot (2y_k-1) \right),$$lên đến hằng số phụ gia độc lập với$x$. 

BDD dành cho$f(x) = [Hx=0]$giới hạn không gian tìm kiếm trong các từ mã. Mỗi root-to-$\top$đường dẫn tương ứng với một từ mã, vì chỉ có các bài tập đáp ứng mới tồn tại được trong quá trình đánh giá. 

Đánh giá động trên BDD tính toán tại mỗi nút tương ứng với chức năng con$g$, khả năng tối ưu để hoàn thành việc gán một phần thông qua cây con. Nếu một nút có biến$x_k$, sự tái diễn là$$L(g) = \max\{ L(g_{x_k=0}) + w_k(0),\ L(g_{x_k=1}) + w_k(1)\},$$Ở đâu$w_k(b)$là sự đóng góp khả năng ghi nhật ký của cài đặt$x_k=b$. 

Việc đánh giá sự lặp lại này từ dưới lên trên BDD không theo chu kỳ mang lại từ mã có khả năng xảy ra tối đa, vì mỗi nút tổng hợp các lần hoàn thành tối ưu của tất cả các ràng buộc hậu tố được biểu thị bởi nút đó. Điều này hoàn thành việc xây dựng giải mã thông qua đánh giá BDD. ∎
