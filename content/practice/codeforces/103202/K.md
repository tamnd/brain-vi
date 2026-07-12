---
title: "CF 103202K - Học viện Scholomance"
description: "Giả sử $G$ là đa đồ thị có các đỉnh là ${0,1,2,3,4,5,6}$ và các cạnh của nó là các quân domino $28$ của bộ sáu nhân đôi, cụ thể là một cạnh giữa $i$ và $j$ cho mỗi $0 le i le j le 6$, bao gồm một vòng lặp ở mỗi đỉnh."
date: "2026-07-03T15:38:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103202
codeforces_index: "K"
codeforces_contest_name: "The 2020 ICPC Asia Shenyang Regional Programming Contest"
rating: 0
weight: 103202
solve_time_s: 143
verified: false
draft: false
---

[CF 103202K - Học viện Scholomance](https://codeforces.com/problemset/problem/103202/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$G$là đa đồ thị có các đỉnh là${0,1,2,3,4,5,6}$và các cạnh của nó là$28$domino của bộ sáu đôi, cụ thể là một cạnh ở giữa$i$Và$j$cho mỗi$0 \le i \le j \le 6$, bao gồm một vòng lặp ở mỗi đỉnh. 

Một chu kỳ hợp lệ của các quân domino là một trật tự tuần hoàn của tất cả các cạnh sao cho các quân domino liên tiếp có chung một điểm cuối. Đây chính xác là mạch Euler của$G$, vì mỗi cạnh được sử dụng một lần và cạnh kề buộc phải có tính liên tục ở các đỉnh, bao gồm cả cạnh cuối cùng nối ngược lại với cạnh đầu tiên. 

Đồ thị được kết nối và Euler vì mỗi đỉnh$v$có bằng cấp$$\deg(v) = 6 + 2 = 8,$$vì nó liền kề với cái kia$6$đỉnh và có một vòng lặp đóng góp$2$đến mức độ. Kể từ đây$\deg(v)$thậm chí là dành cho tất cả$v$, vậy tồn tại mạch Euler. 

Để đếm các mạch Euler, chúng ta áp dụng công thức chuẩn cho đa đồ thị Euler vô hướng thu được từ định lý BEST áp dụng cho công thức đồ thị đường có hướng. Đối với đa đồ thị Euler được kết nối$G$, số mạch Euler là$$\tau(G)\,\prod_{v \in V} \left(\frac{\deg(v)}{2} - 1\right)!\,2^{|E| - |V| + 1},$$Ở đâu$\tau(G)$là số cây khung của đồ thị đơn bên dưới (các vòng lặp không ảnh hưởng đến cây khung). 

Đồ thị đơn giản cơ bản là$K_7$, do đó theo công thức Cayley,$$\tau(G) = 7^{7-2} = 7^5.$$Đối với mỗi đỉnh,$$\frac{\deg(v)}{2} - 1 = \frac{8}{2} - 1 = 3,$$vậy mỗi yếu tố là$3! = 6$. Với bảy đỉnh,$$\prod_{v \in V} \left(\frac{\deg(v)}{2} - 1\right)! = 6^7.$$Đồ thị có$|E| = 28$các cạnh và$|V| = 7$đỉnh, do đó$$2^{|E| - |V| + 1} = 2^{28 - 7 + 1} = 2^{22}.$$Nhân các đóng góp này sẽ được số mạch Euler:$$7^5 \cdot 6^7 \cdot 2^{22}.$$Do đó số chu kỳ domino là$$\boxed{7^5 \cdot 6^7 \cdot 2^{22}}.$$Điều này hoàn thành giải pháp. ∎
