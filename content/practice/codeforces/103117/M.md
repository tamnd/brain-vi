---
title: "CF 103117M - Chuyện Có Thật"
description: "Giả sử $kappat(N)$ biểu thị hàm được xác định trong Phần 7.2.1.3 thông qua biểu diễn tổ hợp $$N = binom{nt}{t} + binom{n{t-1}}{t-1} + cdots + binom{n1}{1}, qquad nt n{t-1} cdots n1 ge 0,$$ và $$kappat(N) = binom{nt}{t-1} + binom{n{t-1}}{t-2} + cdots + binom{n1}{0}."
date: "2026-07-03T20:21:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103117
codeforces_index: "M"
codeforces_contest_name: "The 2021 Sichuan Provincial Collegiate Programming Contest"
rating: 0
weight: 103117
solve_time_s: 122
verified: false
draft: false
---

[CF 103117M - Câu chuyện có thật](https://codeforces.com/problemset/problem/103117/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$\kappa_t(N)$biểu thị hàm được xác định tại Mục 7.2.1.3 thông qua biểu diễn tổ hợp$$N = \binom{n_t}{t} + \binom{n_{t-1}}{t-1} + \cdots + \binom{n_1}{1},
\qquad n_t > n_{t-1} > \cdots > n_1 \ge 0,$$Và$$\kappa_t(N) = \binom{n_t}{t-1} + \binom{n_{t-1}}{t-2} + \cdots + \binom{n_1}{0}.$$Cho phép$M$Và$N$có những đại diện như vậy$$M = \sum_{i=1}^t \binom{m_i}{i}, \qquad N = \sum_{i=1}^t \binom{n_i}{i},$$với$m_t > \cdots > m_1 \ge 0$Và$n_t > \cdots > n_1 \ge 0$. 

Đối với mỗi$i$, định nghĩa$$u_i = \max(m_i,n_i), \qquad \ell_i = \min(m_i,n_i).$$Danh tính$$\binom{m_i}{i} + \binom{n_i}{i} = \binom{u_i}{i} + \binom{\ell_i}{i}$$tuân theo tính đối xứng của các hệ số nhị thức theo thứ tự tham số trên cùng. 

Định nghĩa$$U = \sum_{i=1}^t \binom{u_i}{i}, \qquad L = \sum_{i=1}^t \binom{\ell_i}{i}.$$Sau đó$M+N = U+L$. 

##Giải pháp 

Đối với mỗi cố định$i$, tính đơn điệu của các hệ số nhị thức ở đối số trên mang lại$$\binom{u_i}{i-1} \ge \binom{m_i}{i-1}, \qquad \binom{u_i}{i-1} \ge \binom{n_i}{i-1},$$và tương tự$$\binom{\ell_i}{i-1} \le \binom{m_i}{i-1}, \qquad \binom{\ell_i}{i-1} \le \binom{n_i}{i-1}.$$Tổng hợp lại$i$cho$$\kappa_t(U) \ge \max(\kappa_t(M), \kappa_t(N)), \qquad \kappa_t(L) \le \min(\kappa_t(M), \kappa_t(N)).$$Sự đại diện xác định$\kappa_t$là bảo toàn trật tự khi cộng các khai triển nhị thức rời rạc, do đó áp dụng phân rã nhị thức tham lam cho$U+L$không thể tăng tổng vượt quá tổng các phân hủy tham lam của$U$Và$L$, cho$$\kappa_t(M+N) = \kappa_t(U+L) \le \kappa_t(U) + \kappa_t(L).$$Thay thế giới hạn trên$\kappa_t(U)$Và$\kappa_t(L)$sản lượng$$\kappa_t(M+N) \le \kappa_t(M) + \kappa_t(N),$$chứng minh phần (a). 

Đối với phần (b), hãy chia biểu diễn của$N$vào đầu nó$t$-cấp độ và$(t-1)$- mức độ đóng góp Viết$$N = \binom{n_t}{t} + N',$$Ở đâu$$N' = \sum_{i=1}^{t-1} \binom{n_i}{i}.$$Sau đó$$\kappa_{t-1}(N) = \sum_{i=1}^{t-1} \binom{n_i}{i-1}.$$Sử dụng cùng một phân tách tối đa tối thiểu,$$M+N = (M \vee N_t) + (M \wedge N_t) + N',$$Ở đâu$N_t = \binom{n_t}{t}$chỉ đóng góp ở mức độ$t$. 

Thuật ngữ$M \vee N_t$đóng góp nhiều nhất$\max(\kappa_t M, N)$, kể từ khi$t$-phần nhị thức cấp được giới hạn bởi$N$và tất cả những đóng góp thấp hơn được giới hạn bởi$\kappa_t M$bởi sự đơn điệu. 

Phần còn lại$M \wedge N_t + N'$đóng góp nhiều nhất$\kappa_{t-1}(N)$sau khi dịch chuyển các chỉ số xuống một đơn vị trong khai triển nhị thức. 

Việc kết hợp những đóng góp này mang lại$$\kappa_t(M+N) \le \max(\kappa_t M, N) + \kappa_{t-1}(N),$$chứng minh phần (b). ∎ 

## Xác minh 

Mỗi bước chỉ sử dụng tính đơn điệu của$\binom{x}{k}$TRONG$x$cố định$k$và cấu trúc tham lam xác định của$\kappa_t$đại diện. Phân rã max-min bảo toàn sự bằng nhau ở mức tổng nhị thức theo kỳ hạn. Sự phân hủy của$N$vào số hạng cấp cao nhất của nó và phần còn lại phù hợp với mối quan hệ dịch chuyển giữa$\kappa_t$Và$\kappa_{t-1}$. 

Không có bước nào đưa ra các thuật ngữ bên ngoài các chỉ số khai triển nhị thức được phép và mỗi bất đẳng thức xuất phát từ việc so sánh tọa độ các đối số trên. 

## Ghi chú 

Cấu trúc này là một dạng tương tự rời rạc của tính cộng tính phụ đối với các thứ tự lồi được tạo ra bởi các cơ sở hệ số nhị thức. Phân rã max-min là sự tương tự tổ hợp của phân tách mang trong các hệ cơ số hỗn hợp được xác định bởi các hệ số nhị thức.
