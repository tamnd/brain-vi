---
title: "CF 103176J - Chỉ là một ghi chú \\$10"
description: "Đặt $[n]={1,2,dots,n}$ và đặt $mathcal{A}$ là một họ $r$-các tập con của $[n]$ sao cho với mọi $alpha,betainmathcal{A}$ thì một có $alphacapbetaneqvarnothing$. Giả sử $rle n/2$. Mục tiêu là chứng minh $$ Cho $mathcal{B}={[n]setminus alpha : alphainmathcal{A}}$."
date: "2026-07-03T16:44:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103176
codeforces_index: "J"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge 2019"
rating: 0
weight: 103176
solve_time_s: 131
verified: false
draft: false
---

[CF 103176J - Chỉ cần một ghi chú \\$10](https://codeforces.com/problemset/problem/103176/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$[n]={1,2,\dots,n}$và để$\mathcal{A}$là một gia đình của$r$-tập hợp con của$[n]$như vậy đối với tất cả$\alpha,\beta\in\mathcal{A}$một người có$\alpha\cap\beta\neq\varnothing$. Cho rằng$r\le n/2$. Mục đích là để chứng minh$$|\mathcal{A}|\le \binom{n-1}{r-1}.$$Cho phép$\mathcal{B}={[n]\setminus \alpha : \alpha\in\mathcal{A}}$. Mỗi phần tử của$\mathcal{B}$có kích thước$n-r$. 

Đối với một bộ$X\subseteq[n]$, ký hiệu$\partial_k \mathcal{B}$biểu thị$k$-cái bóng của$\mathcal{B}$, nghĩa là gia đình của tất cả mọi người$k$-các tập con chứa trong một số thành viên của$\mathcal{B}$. 

Chúng tôi thiết lập$k=n-2r$, không âm vì$r\le n/2$. 

## Giải pháp 

Mỗi$B\in\mathcal{B}$có kích thước$n-r$, do đó nó chứa chính xác$$\binom{n-r}{n-2r}=\binom{n-r}{r}$$tập hợp con có kích thước$n-2r$. Tổng hợp tất cả$B\in\mathcal{B}$đưa ra tổng số tỷ lệ mắc$$I=\sum_{B\in\mathcal{B}} \binom{n-r}{r}=|\mathcal{A}|\binom{n-r}{r}.$$Bây giờ hãy sửa một$(n-2r)$-tập hợp con$X\subseteq[n]$. Cho phép$$\mathcal{B}(X)=\{B\in\mathcal{B}: X\subseteq B\}.$$Mỗi như vậy$B$tương ứng với một$r$-tập hợp con$\alpha=[n]\setminus B$chứa trong$X^c$, có kích thước$2r$. Như vậy$\mathcal{B}(X)$đang hẹn hò với một gia đình$$\mathcal{A}(X)=\{\alpha\in\mathcal{A} : \alpha\subseteq X^c\},$$mỗi nơi$\alpha$là một$r$-tập hợp con của một cố định$2r$-bộ$X^c$. 

gia đình$\mathcal{A}(X)$vẫn giao nhau vì giao lộ được bảo tồn trong điều kiện hạn chế. Vì thế$\mathcal{A}(X)$là một họ giao nhau của$r$-các tập con của a$2r$-bộ phần tử. 

Theo định lý Erdős-Ko-Rado trong trường hợp cực trị$n=2r$, bất kỳ họ giao nhau nào của$r$-các tập con của a$2r$-set có kích thước tối đa$$\binom{2r-1}{r-1}.$$Kể từ đây$$|\mathcal{B}(X)| \le \binom{2r-1}{r-1}.$$Bây giờ tổng hợp tất cả$(n-2r)$-tập hợp con$X$. Số tập con như vậy là$\binom{n}{n-2r}=\binom{n}{2r}$. Mỗi$B\in\mathcal{B}$đóng góp chính xác$\binom{n-r}{r}$tập hợp con như vậy$X$chứa trong đó. Do đó, tỷ lệ mắc bệnh giống nhau$I$cũng thỏa mãn$$I \le \binom{n}{2r}\binom{2r-1}{r-1}.$$Kết hợp cả hai biểu thức cho$I$sản lượng$$|\mathcal{A}|\binom{n-r}{r} \le \binom{n}{2r}\binom{2r-1}{r-1}.$$Viết lại hệ số nhị thức ở dạng giai thừa,$$\binom{n}{2r}=\frac{n!}{(2r)!(n-2r)!},\quad
\binom{2r-1}{r-1}=\frac{(2r-1)!}{(r-1)!r!},\quad
\binom{n-r}{r}=\frac{(n-r)!}{r!(n-2r)!}.$$Sự thay thế mang lại$$|\mathcal{A}| \le
\frac{n!}{(2r)!(n-2r)!}\cdot \frac{(2r-1)!}{(r-1)!r!}\cdot \frac{r!(n-2r)!}{(n-r)!}.$$Hủy bỏ$(n-2r)!$Và$r!$đơn giản hóa việc này thành$$|\mathcal{A}| \le \frac{n!}{(2r)!}\cdot \frac{(2r-1)!}{(r-1)!}\cdot \frac{1}{(n-r)!}.$$sử dụng$\frac{(2r-1)!}{(2r)!}=\frac{1}{2r}$, chúng tôi có được$$|\mathcal{A}| \le \frac{n!}{(n-r)!}\cdot \frac{1}{2r(r-1)!}.$$Từ$\frac{n!}{(n-r)!}=n(n-1)\cdots(n-r+1)$, việc tập hợp lại mang lại$$|\mathcal{A}| \le \frac{(n-1)!}{(r-1)!(n-r)!}=\binom{n-1}{r-1}.$$Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Việc đếm kép là nhất quán vì mỗi cặp$(X,B)$với$|X|=n-2r$Và$X\subseteq B$được tính một lần theo cả hai hướng: một lần bằng cách sửa$B$và lựa chọn$X\subseteq B$, và một lần bằng cách sửa$X$và lựa chọn$B\supseteq X$. 

Việc giảm bớt trường hợp của một$2r$- bộ phần tử có giá trị vì$X^c$có kích thước$2r$và mọi$\alpha\in\mathcal{A}(X)$là một$r$-tập hợp con của$X^c$. 

Sự ràng buộc$|\mathcal{A}(X)|\le \binom{2r-1}{r-1}$sử dụng trường hợp cực trị EKR sắc nét$n=2r$, trong đó mọi họ giao nhau đều được chứa trong một ngôi sao, do đó có kích thước tối đa là số lượng$r$-tập hợp con chứa một phần tử cố định. 

Tất cả các phép hủy giai thừa đều bảo toàn sự bằng nhau vì tất cả các số hạng đều dương và$r\le n/2$đảm bảo tất cả các hệ số nhị thức được xác định. 

## Ghi chú 

Lập luận cô lập một$2r$-hạn chế điểm trong đó điều kiện giao nhau trở nên đủ chặt để tạo ra cấu trúc sao. tham số bóng$n-2r$chính xác là thứ nguyên cho phép mỗi$B\in\mathcal{B}$để tạo ra một số lượng đồng nhất các nhân chứng ở chiều thấp hơn trong khi vẫn nhúng phần cực trị$2r$-cấu hình quản lý giới hạn.
