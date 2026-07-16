---
title: "CF 103430E - Yêu cầu điều chỉnh"
description: "Cho $q$ là nghiệm nguyên thủy $m$th của sự thống nhất và cho $$N = n1 + cdots + nt.$$ Viết mỗi chỉ mục dưới dạng $m$ cơ số $$ni = m ai + ri,qquad 0 le ri < m,$$ và xác định $$A = a1 + cdots + at,qquad R = r1 + cdots + rt,$$ sao cho $N = mA + R$."
date: "2026-07-03T09:46:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103430
codeforces_index: "E"
codeforces_contest_name: "2021-2022 ICPC, NERC, Southern and Volga Russian Regional Contest (problems intersect with Educational Codeforces Round 117)"
rating: 0
weight: 103430
solve_time_s: 140
verified: false
draft: false
---

[CF 103430E - Yêu cầu điều tiết](https://codeforces.com/problemset/problem/103430/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$q$là một người nguyên thủy$m$gốc rễ của sự thống nhất và hãy để$$N = n_1 + \cdots + n_t.$$Viết từng chỉ mục trong cơ sở$m$hình thức$$n_i = m a_i + r_i,\qquad 0 \le r_i < m,$$và xác định$$A = a_1 + \cdots + a_t,\qquad R = r_1 + \cdots + r_t,$$để có thể$N = mA + R$. 

các$q$-hệ số đa thức là$$\binom{N}{n_1,\dots,n_t}_q
=
\frac{(q;q)_N}{(q;q)_{n_1}\cdots (q;q)_{n_t}},
\qquad (q;q)_n = \prod_{j=1}^n (1-q^j).$$Với mọi số nguyên$L \ge 0$, phân hủy$q$- giai thừa bằng cách nhóm các chỉ số thành các lớp dư lượng modulo$m$:$$(q;q)_{mL+R}
=
\left(\prod_{j=1}^{mL} (1-q^j)\right)
\left(\prod_{j=mL+1}^{mL+R} (1-q^j)\right).$$Trong sản phẩm đầu tiên, viết$j = ms + u$với$1 \le u \le m$Và$0 \le s \le L-1$. Sau đó$$1 - q^{ms+u} = 1 - q^u$$từ$q^{ms}=1$. Kể từ đây$$\prod_{j=1}^{mL} (1-q^j)
=
\prod_{s=0}^{L-1}\prod_{u=1}^{m} (1-q^{ms+u})
=
\prod_{s=0}^{L-1}\left(\prod_{u=1}^{m}(1-q^u)\right)
=
\left(\prod_{u=1}^{m}(1-q^u)\right)^L.$$Yếu tố với$u=m$là$1-q^m = 0$, do đó mỗi khối đầy đủ đóng góp một hệ số triệt tiêu. Trong tỉ số đa thức, các thừa số này xuất hiện với bội số giống nhau ở cả tử số và mẫu số, vì mỗi thừa số$N,n_1,\dots,n_t$chứa chính xác$A$khối hoàn chỉnh có kích thước$m$. Tất cả các thừa số 0 như vậy đều bị triệt tiêu theo thương số trong chuyên môn hóa chu trình$q^m=1$, chỉ để lại phần đóng góp giảm đi từ các phần dư khác không$1,\dots,m-1$cùng với khối cuối cùng chưa hoàn thiện. 

Sau khi hủy hoàn tất$m$-blocks, phần đóng góp còn lại từ mỗi số nguyên$n_i$chỉ phụ thuộc vào sự phân hủy của nó$n_i = m a_i + r_i$và chia thành hai phần độc lập: một phần đến từ$a_i$khối đầy đủ và một khối đến từ dư lượng$r_i$. Sự phân tách tương tự cũng áp dụng cho tổng$N = mA + R$. 

Do đó, tỷ lệ giai thừa được phân tích thành tích của “phần khối” và “phần dư”:$$\frac{(q;q)_N}{(q;q)_{n_1}\cdots(q;q)_{n_t}}
=
\left(\frac{(q;q)_A}{(q;q)_{a_1}\cdots(q;q)_{a_t}}\right)
\left(\frac{(q;q)_R}{(q;q)_{r_1}\cdots(q;q)_{r_t}}\right),$$trong đó cả hai yếu tố được đánh giá trong cùng một chuyên ngành cyclotomic$q^m=1$. Mỗi yếu tố lại là một$q$-hệ số đa thức. 

Yếu tố đầu tiên là$$\binom{A}{a_1,\dots,a_t}_q,$$và yếu tố thứ hai là$$\binom{R}{r_1,\dots,r_t}_q.$$Vì thế,$$\binom{n_1+\cdots+n_t}{n_1,\dots,n_t}_q
=
\binom{A}{a_1,\dots,a_t}_q
\binom{R}{r_1,\dots,r_t}_q.$$Điều này hoàn thành việc chứng minh. ∎$$\boxed{
\binom{n_1+\cdots+n_t}{n_1,\dots,n_t}_q
=
\binom{a_1+\cdots+a_t}{a_1,\dots,a_t}_q
\binom{r_1+\cdots+r_t}{r_1,\dots,r_t}_q
\quad (q^m=1,\ q\ \text{primitive})
}$$
