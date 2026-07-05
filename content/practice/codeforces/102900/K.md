---
title: "CF 102900K - Thương gia du lịch"
description: "Cho $n ge m ge 1$ và cho $a1 ge a2 ge cdots ge am ge 1$ là một phân vùng của $n$ sao cho $ Thật vậy, nếu $a1$ là phần lớn nhất và $am$ là phần nhỏ nhất, thì điều kiện cho $a1 - am le 1$, do đó $am thuộc {a1, a1 - 1}$. Do đó mọi phần đều bằng $a1$ hoặc $a1 - 1$."
date: "2026-07-04T08:18:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102900
codeforces_index: "K"
codeforces_contest_name: "2020 ICPC Shanghai Site"
rating: 0
weight: 102900
solve_time_s: 162
verified: false
draft: false
---

[CF 102900K - Thương gia du lịch](https://codeforces.com/problemset/problem/102900/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$n \ge m \ge 1$và để$a_1 \ge a_2 \ge \cdots \ge a_m \ge 1$là một phân vùng của$n$như vậy$|a_i - a_j| \le 1$cho tất cả$i,j$. Khi đó tập hợp các giá trị phần riêng biệt chứa tối đa hai số nguyên liên tiếp. 

Thật vậy, nếu$a_1$là phần lớn nhất và$a_m$là phần tối thiểu, điều kiện cho$a_1 - a_m \le 1$, kể từ đây$a_m \in {a_1, a_1 - 1}$. Do đó mọi phần đều bằng$a_1$hoặc$a_1 - 1$. 

Cho phép$k$biểu thị số phần bằng$a_1$. Sau đó$m-k$phần bằng nhau$a_1 - 1$, và điều kiện tổng trở thành$$n = k a_1 + (m-k)(a_1 - 1).$$Mở rộng mang lại$$n = k a_1 + m a_1 - k a_1 - m + k = m(a_1 - 1) + k.$$Kể từ đây$$k = n - m(a_1 - 1).$$Từ$0 \le k \le m$, số nguyên$a_1$bị hạn chế bởi$$m(a_1 - 1) \le n \le m(a_1 - 1) + m,$$tương đương với$$a_1 - 1 \le \frac{n}{m} \le a_1.$$Như vậy$a_1 = \left\lceil \frac{n}{m} \right\rceil$. Viết$n = qm + r$với$0 \le r < m$, điều này mang lại$q = \left\lfloor \frac{n}{m} \right\rfloor$Và$$a_1 =
\begin{cases}
q, & r = 0,\\
q+1, & r > 0.
\end{cases}$$Nếu như$r = 0$, sau đó$k = n - m(q-1) = mq - m(q-1) = m$, vậy mọi phần đều bằng nhau$q$. 

Nếu như$r > 0$, sau đó$a_1 = q+1$, Và$$k = n - m q = r.$$Như vậy chính xác$r$phần bằng nhau$q+1$, và phần còn lại$m-r$phần bằng nhau$q$. 

Vì phân vùng không tăng nên phân vùng đầu tiên$r$các bộ phận là$q+1$và phần còn lại$m-r$các bộ phận là$q$, vì vậy$j$phần thứ là$$a_j =
\begin{cases}
q+1, & 1 \le j \le r,\\
q, & r < j \le m.
\end{cases}$$Bất kỳ phân vùng cân bằng tối ưu nào cũng phải có dạng này vì số phần bằng giá trị lớn hơn được xác định duy nhất bởi ràng buộc tổng và không có lựa chọn giá trị nào khác nhau nhiều nhất$1$có thể đáp ứng tổng số yêu cầu. Điều này hoàn thành việc chứng minh. ∎
