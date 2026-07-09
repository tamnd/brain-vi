---
title: "CF 103003D - Bộ điểm"
description: "Cho $n=s+t$ như trong (1), và viết một tổ hợp $(s,t)$-dưới dạng $ct cdots c2 c1$ thỏa mãn (3), đó là $n ct cdots c2 c1 ge 0."
date: "2026-07-04T02:23:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103003
codeforces_index: "D"
codeforces_contest_name: "box 2020"
rating: 0
weight: 103003
solve_time_s: 163
verified: false
draft: false
---

[CF 103003D - Tập hợp điểm](https://codeforces.com/problemset/problem/103003/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$n=s+t$như trong (1), và cho một$(s,t)$-tổ hợp được viết dưới dạng$c_t \cdots c_2 c_1$thỏa mãn (3), tức là$n > c_t > \cdots > c_2 > c_1 \ge 0.$Xác định các số nguyên không âm liên quan$q_t, \ldots, q_0$bởi (11) và (12):$q_t = s - d_t,\quad q_{t-1} = d_t - d_{t-1},\quad \ldots,\quad q_1 = d_2 - d_1,\quad q_0 = d_1,$nơi các số nguyên$d_j$có liên hệ với tổ hợp của (7),$c_j = d_j + j - 1 \quad (1 \le j \le t).$Từ những mối quan hệ này,$q_t + \cdots + q_1 + q_0 = s,$vì vậy mỗi$(s,t)$-sự kết hợp quyết định thành phần của$s$vào trong$t+1$những phần không âm. 

Để chứng minh bổ đề nén (85), chỉ cần chứng minh rằng sự tương ứng này là phỏng đoán. 

Tính tiêm nhiễm xuất phát từ việc tái cấu trúc$d_1, \ldots, d_t$duy nhất từ$q_0, \ldots, q_t$. Từ (12),$d_1 = q_0,$

$d_2 = q_1 + d_1,$

$d_3 = q_2 + d_2,$và nói chung$d_j = q_{j-1} + d_{j-1} \quad (2 \le j \le t).$Như vậy mỗi$d_j$được xác định duy nhất bởi$q$- theo trình tự, và sau đó mỗi$c_j = d_j + j - 1$được xác định duy nhất. Do đó sự kết hợp khác biệt mang lại sự khác biệt$q$-trình tự. 

Tính thẩm thấu thu được bằng cách đảo ngược việc xây dựng. Cho phép$q_t, \ldots, q_0$là bất kỳ thành phần nào của$s$vào trong$t+1$những phần không âm. Định nghĩa$d_1 = q_0$và đệ quy$d_j = q_{j-1} + d_{j-1} \quad (2 \le j \le t),$sau đó đặt$c_j = d_j + j - 1 \quad (1 \le j \le t).$Sự đệ quy ngụ ý$d_1 \le d_2 \le \cdots \le d_t$, kể từ đây$c_1 < c_2 < \cdots < c_t,$và cả$c_t \le (q_0 + \cdots + q_{t-1}) + (t-1) = (s - q_t) + (t-1) < s + t = n,$Vì thế$n > c_t > \cdots > c_1 \ge 0$nắm giữ, làm$c_t \cdots c_1$hợp lệ$(s,t)$-sự kết hợp. 

Vì vậy mỗi thành phần của$s$phát sinh từ một sự kết hợp duy nhất và mọi sự kết hợp đều tạo ra một thành phần, do đó sự tương ứng là tính từ. Điều này thiết lập bổ đề nén (85). ∎
