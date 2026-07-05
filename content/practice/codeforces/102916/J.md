---
title: "CF 102916J - Đảo Lạc"
description: "Giả sử $a1 ge a2 ge cdots ge am ge 1$ là một phân vùng của $n$ thành $m$ phần được cân bằng tối ưu, nghĩa là $ Giả sử $t$ là số phần bằng $x$ và $m-t$ là số phần bằng $x-1$. Phân vùng có tổng tổng $$n = tx + (m-t)(x-1) = mx - (m-t)."
date: "2026-07-04T08:03:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "J"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 152
verified: false
draft: false
---

[CF 102916J - Đảo bị mất](https://codeforces.com/problemset/problem/102916/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$a_1 \ge a_2 \ge \cdots \ge a_m \ge 1$là một phân vùng của$n$vào trong$m$các bộ phận được cân bằng tối ưu, nghĩa là$|a_i-a_j|\le 1$cho tất cả$1\le i,j\le m$. Điều kiện này tương đương với việc yêu cầu phần lớn nhất và phần nhỏ nhất khác nhau nhiều nhất$1$, vậy nếu$a_1 = x$, thì mọi phần đều thỏa mãn$a_m \in {x, x-1}$và không thể có giá trị nào khác. 

Cho phép$t$có số phần bằng$x$Và$m-t$số phần bằng$x-1$. Phân vùng có tổng số tiền$$n = tx + (m-t)(x-1) = mx - (m-t).$$Giải quyết cho$x$cho$$mx = n + m - t,\quad x = \frac{n+m-t}{m}.$$Từ$x$là một số nguyên,$n+m-t \equiv 0 \pmod m$, kể từ đây$t \equiv n \pmod m$. Viết$$n = mq + r,\quad 0 \le r < m,$$Vì thế$q = \lfloor n/m \rfloor$Và$r = n \bmod m$. 

Thay thế vào biểu thức cho$n$,$$n = m q + r.$$Một phân vùng cân bằng tối ưu phải sử dụng các phần khác nhau nhiều nhất$1$, vì vậy các giá trị duy nhất có thể là$q$Và$q+1$. Cho phép$t$có số phần bằng$q+1$, Và$m-t$số phần bằng$q$. Sau đó, ràng buộc tổng trở thành$$n = t(q+1) + (m-t)q = mq + t.$$So sánh với$n = mq + r$cho$t = r$. Do đó chính xác$r$các bộ phận là$q+1$và phần còn lại$m-r$các bộ phận là$q$. 

Điều này xác định một phân vùng duy nhất vì chuỗi buộc phải không tăng, với tất cả các phần lớn hơn được đặt trước:$$a_1 = \cdots = a_r = q+1,\quad a_{r+1} = \cdots = a_m = q.$$Để xác minh sự cân bằng tối ưu, bất kỳ cặp bộ phận nào cũng khác nhau$0$hoặc$1$vì các giá trị duy nhất hiện tại là$q$Và$q+1$, Vì thế$|a_i-a_j|\le 1$nắm giữ. Bất kỳ phân vùng nào khác vào$m$các phần phải đi chệch khỏi sự phân bố thương số và số dư này, điều này sẽ buộc một số phần ít nhất phải$q+2$hoặc nhiều nhất$q-1$, mâu thuẫn với ràng buộc tổng hoặc điều kiện không tăng với mức chênh lệch tối thiểu. 

Như vậy có chính xác một phân vùng cân bằng tối ưu. 

các$j$do đó phần thứ là$$a_j =
\begin{cases}
\left\lfloor \frac{n}{m} \right\rfloor + 1 & \text{if } 1 \le j \le n \bmod m,\\[6pt]
\left\lfloor \frac{n}{m} \right\rfloor & \text{if } n \bmod m < j \le m.
\end{cases}$$Điều này hoàn thành việc chứng minh. ∎
