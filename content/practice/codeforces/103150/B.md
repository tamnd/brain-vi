---
title: "CF 103150B - Quy trình mũi tên"
description: "Đặt κt(N) là tham số hàng đầu trong biểu diễn tổ hợp bậc-$t$ của $N$, sao cho κt(N) là số nguyên duy nhất $nt$ thỏa mãn $$binom{nt}{t} le N < binom{nt+1}{t}."
date: "2026-07-03T19:54:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103150
codeforces_index: "B"
codeforces_contest_name: "EZ Programming Contest #1"
rating: 0
weight: 103150
solve_time_s: 78
verified: false
draft: false
---

[CF 103150B - Quy trình mũi tên](https://codeforces.com/problemset/problem/103150/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Đặt κ_t(N) là tham số hàng đầu trong mức độ-$t$biểu diễn tổ hợp của$N$, do đó κ_t(N) là số nguyên duy nhất$n_t$thỏa mãn$$\binom{n_t}{t} \le N < \binom{n_t+1}{t}.$$Đặc tính này xuất phát từ cách xây dựng tham lam tiềm ẩn trong Định lý K và cách biểu diễn trong bài tập 75, trong đó κ_t(N) là chỉ số lớn nhất xuất hiện trong$t$-đại diện của$N$. 

Viết$n_t = \kappa_t(N)$. Khi đó tồn tại số dư$R$như vậy$$N = \binom{n_t}{t} + R, \qquad 0 \le R < \binom{n_t+1}{t} - \binom{n_t}{t}.$$Đặc biệt, sự bất bình đẳng xác định ngụ ý$$0 \le R < \binom{n_t+1}{t} - \binom{n_t}{t},$$và kể từ đó$\binom{n_t+1}{t} > \binom{n_t}{t}$, sự gia tăng$N \mapsto N+1$thay đổi$n_t$chỉ khi$R$đạt giá trị tối đa có thể trước khi xảy ra chuyển sang ngưỡng nhị thức tiếp theo. 

Vì$N+1$, có hai trường hợp. 

Nếu như$R+1 < \binom{n_t+1}{t} - \binom{n_t}{t}$, sau đó$N+1$vẫn nằm trong cùng một khoảng$$\binom{n_t}{t} \le N+1 < \binom{n_t+1}{t},$$vì vậy κ_t(N+1) = n_t và do đó κ_t(N+1) - κ_t(N) = 0. 

Nếu$R$đạt giá trị cực đại phù hợp với$n_t$, sau đó$N+1$đạt đến ranh giới nhị thức tiếp theo, nghĩa là$$N+1 = \binom{n_t+1}{t}.$$Trong trường hợp này tính chất cực đại của lực κ_t$$\kappa_t(N+1) = n_t+1,$$từ$\binom{n_t+1}{t}$là giá trị đầu tiên yêu cầu chỉ số hàng đầu lớn hơn$n_t$trong biểu diễn tổ hợp. 

Không thể có bước nhảy lớn hơn vì bất đẳng thức xác định cho κ_t cho thấy rằng việc tăng$N$qua$1$có thể vượt qua nhiều nhất một ngưỡng nhị thức có dạng$\binom{m}{t}$. 

Vì thế,$$\kappa_t(N+1) - \kappa_t(N) =
\begin{cases}
1, & \text{if } N+1 = \binom{m}{t} \text{ for some } m,\\[4pt]
0, & \text{otherwise}.
\end{cases}$$Tương tự, sự gia tăng xảy ra chính xác khi$N+1$là một$t$-hệ số nhị thức thứ trong phép liệt kê tam giác Pascal và trong tất cả các trường hợp khác, chỉ số tổ hợp hàng đầu không thay đổi.$$\boxed{\kappa_t(N+1)-\kappa_t(N)\in\{0,1\},\ \text{equal to }1 \text{ iff } N+1=\binom{m}{t}\text{ for some }m.}$$Điều này hoàn thành giải pháp. ∎
