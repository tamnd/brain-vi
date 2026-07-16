---
title: "CF 103428C - Gán hoặc nhân"
description: "Cho $q$ là nghiệm nguyên thủy thứ $m$ của đơn vị. Với mỗi $i$ với $1 le i le t$, hãy viết $$ni = m ai + bi, qquad 0 le bi < m,$$ và đặt $$N = n1 + cdots + nt, qquad A = a1 + cdots + at, qquad B = b1 + cdots + bt,$$ sao cho $N = mA + B$."
date: "2026-07-03T09:41:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103428
codeforces_index: "C"
codeforces_contest_name: "The 2021 CCPC Weihai Onsite"
rating: 0
weight: 103428
solve_time_s: 62
verified: false
draft: false
---

[CF 103428C - Gán hoặc nhân](https://codeforces.com/problemset/problem/103428/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$q$là một người nguyên thủy$m$gốc rễ của sự đoàn kết. Đối với mỗi$i$với$1 \le i \le t$, viết$$n_i = m a_i + b_i, \qquad 0 \le b_i < m,$$và thiết lập$$N = n_1 + \cdots + n_t, \qquad A = a_1 + \cdots + a_t, \qquad B = b_1 + \cdots + b_t,$$để có thể$N = mA + B$. 

các$q$-hệ số đa thức được xác định bởi$$\binom{N}{n_1,\ldots,n_t}_q = \frac{[N]!_q}{[n_1]!_q \cdots [n_t]!_q}.$$Từ Bài tập 49, cho mỗi cặp$(n,k)$người ta có hệ số hóa$$\binom{n}{k}_q = \binom{\lfloor n/m \rfloor}{\lfloor k/m \rfloor}\binom{n \bmod m}{k \bmod m}_q.$$Hệ số đa thức thừa nhận sự phân rã kính thiên văn$$\binom{N}{n_1,\ldots,n_t}_q
=
\binom{N}{n_1}_q
\binom{N-n_1}{n_2}_q
\cdots
\binom{n_t}{n_t}_q,$$kể từ khi hủy bỏ liên tiếp$q$- Năng suất giai thừa$$\frac{[N]!_q}{[n_1]!_q \cdots [n_t]!_q}
=
\frac{[N]!_q}{[n_1]!_q [N-n_1]!_q}
\cdot
\frac{[N-n_1]!_q}{[n_2]!_q [N-n_1-n_2]!_q}
\cdots
\frac{[n_t]!_q}{[n_t]!_q}.$$Với mỗi thừa số, áp dụng kết quả nhị thức từ Bài tập 49. Đối với thừa số đầu tiên,$$\binom{N}{n_1}_q
=
\binom{A}{a_1}
\binom{B}{b_1}_q,$$từ$N = mA + B$Và$n_1 = ma_1 + b_1$. 

Sau khi loại bỏ$n_1$, các tham số còn lại là$$N^{(1)} = N - n_1 = m(A-a_1) + (B-b_1),$$và việc lặp lại cùng một sự phân rã sẽ mang lại cho mỗi$j$,$$\binom{N - (n_1+\cdots+n_{j-1})}{n_j}_q
=
\binom{A - (a_1+\cdots+a_{j-1})}{a_j}
\binom{B - (b_1+\cdots+b_{j-1})}{b_j}_q.$$Nhân các danh tính này cho$j = 1,\ldots,t$mang lại sự hủy bỏ ở cả phần đa thức nguyên và phần$q$-phần đa thức. Kính thiên văn các thừa số nguyên để$$\binom{A}{a_1}\binom{A-a_1}{a_2}\cdots\binom{a_t}{a_t}
=
\binom{A}{a_1,\ldots,a_t},$$trong khi$q$- kính thiên văn yếu tố để$$\binom{B}{b_1}_q\binom{B-b_1}{b_2}_q\cdots\binom{b_t}{b_t}_q
=
\binom{B}{b_1,\ldots,b_t}_q.$$Kết hợp cả hai phần mang lại$$\binom{N}{n_1,\ldots,n_t}_q
=
\binom{A}{a_1,\ldots,a_t}
\binom{B}{b_1,\ldots,b_t}_q.$$Thay thế trở lại$A = \sum_{i=1}^t \lfloor n_i/m \rfloor$Và$B = \sum_{i=1}^t (n_i \bmod m)$mang lại phần mở rộng đã nêu:$$\boxed{
\binom{n_1+\cdots+n_t}{n_1,\ldots,n_t}_q
=
\binom{\sum_{i=1}^t \lfloor n_i/m \rfloor}{\lfloor n_1/m \rfloor,\ldots,\lfloor n_t/m \rfloor}
\binom{\sum_{i=1}^t (n_i \bmod m)}{n_1 \bmod m,\ldots,n_t \bmod m}_q
}.$$Điều này hoàn thành việc chứng minh. ∎
