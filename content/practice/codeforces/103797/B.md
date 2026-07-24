---
title: "CF 103797B - Cược xe buýt"
description: "Đặt $$G(z)=sum{xin{0,1}^n} f(x),z^{x1+cdots+xn}.$$ Sau đó $$G(-1)=sum{x} f(x),(-1)^{ trong đó $ Viết $f$ trong khai triển đa tuyến tính duy nhất của nó trên $mathbb{R}$, $$f(x)=sum{Ssubseteq [n]} aS prod{iin S} xi,$$ sao cho $a{[n]}$ là hệ số của đơn thức đầy đủ $x1x2cdots xn$."
date: "2026-07-02T08:47:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103797
codeforces_index: "B"
codeforces_contest_name: "IME++ Starters Try-outs 2022"
rating: 0
weight: 103797
solve_time_s: 52
verified: false
draft: false
---

[CF 103797B - Đặt cược xe buýt](https://codeforces.com/problemset/problem/103797/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$$G(z)=\sum_{x\in\{0,1\}^n} f(x)\,z^{x_1+\cdots+x_n}.$$Sau đó$$G(-1)=\sum_{x} f(x)\,(-1)^{|x|},$$Ở đâu$|x|=x_1+\cdots+x_n$. 

Viết$f$trong việc mở rộng đa tuyến tính độc đáo của nó trên$\mathbb{R}$,$$f(x)=\sum_{S\subseteq [n]} a_S \prod_{i\in S} x_i,$$để có thể$a_{[n]}$là hệ số của đơn thức đầy đủ$x_1x_2\cdots x_n$. 

Thay thế khai triển này thành$G(-1)$và trao đổi tổng kết cho$$G(-1)=\sum_{S\subseteq[n]} a_S \sum_{x\in\{0,1\}^n} \left(\prod_{i\in S} x_i\right)(-1)^{|x|}.$$Tổng bên trong biến mất trừ khi$S=[n]$. Thật vậy, nếu$j\notin S$, thì với mỗi phép gán các biến khác, hai phép gán thu được bằng cách lật$x_j$đóng góp độ lớn bằng nhau và trái dấu vì$(-1)^{|x|}$đổi dấu trong khi thừa số$\prod_{i\in S} x_i$không thay đổi. Điều này tạo ra sự hủy bỏ hoàn toàn. Vì thế chỉ$S=[n]$sống sót, cho$$G(-1)=a_{[n]}\sum_{x\in\{0,1\}^n} x_1x_2\cdots x_n(-1)^{|x|}.$$Chỉ có nhiệm vụ duy nhất$x=(1,1,\dots,1)$đóng góp, vì vậy$$G(-1)=a_{[n]}(-1)^n.$$Kể từ đây$G(-1)\neq 0$ngụ ý$a_{[n]}\neq 0$, do đó đa thức đa tuyến tính cho$f$có bằng cấp$n$. 

Mỗi FBDD đặc biệt là một cây quyết định và phương pháp đa thức tiêu chuẩn cho cây quyết định ngụ ý rằng bất kỳ cấu trúc quyết định xác định nào đều có thể$f$phải có độ sâu ít nhất bằng mức biểu diễn đa thức đa tuyến tính. Vì vậy mỗi FBDD cho$f$phải chứa đường dẫn từ gốc tới lá truy vấn tất cả$n$các biến. 

Đây chính xác là định nghĩa của sự trốn tránh. 

Điều này hoàn thành việc chứng minh. ∎
