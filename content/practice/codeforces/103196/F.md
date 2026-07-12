---
title: "CF 103196F - \u0424\u0443\u0442\u0431\u043e\u043b\u043a\u0438 \u043d\u0430 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0443"
description: "Giả sử $G$ là đa đồ thị có các đỉnh là ${0,1,2,3,4,5,6}$ và các cạnh của nó là các quân domino $28$ của bộ sáu nhân đôi, cụ thể là một cạnh giữa $i$ và $j$ cho mỗi $0 le i le j le 6$, bao gồm một vòng lặp ở mỗi đỉnh."
date: "2026-07-03T15:44:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103196
codeforces_index: "F"
codeforces_contest_name: "2020-2021 \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0437\u0430\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 103196
solve_time_s: 147
verified: false
draft: false
---

[CF 103196F - \u0424\u0443\u0442\u0431\u043e\u043b\u043a\u0438 \u043d\u0430 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0443](https://codeforces.com/problemset/problem/103196/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$G$là đa đồ thị có các đỉnh là${0,1,2,3,4,5,6}$và các cạnh của nó là$28$domino của bộ sáu đôi, cụ thể là một cạnh ở giữa$i$Và$j$cho mỗi$0 \le i \le j \le 6$, bao gồm một vòng lặp ở mỗi đỉnh. 

Một chu kỳ hợp lệ của các quân domino là một trật tự tuần hoàn của tất cả các cạnh sao cho các quân domino liên tiếp có chung một điểm cuối. Đây chính xác là mạch Euler của$G$, vì mỗi cạnh được sử dụng một lần và cạnh kề buộc phải có tính liên tục ở các đỉnh, bao gồm cả cạnh cuối cùng nối ngược lại với cạnh đầu tiên. 

Đồ thị được kết nối và Euler vì mỗi đỉnh$v$có bằng cấp$$\deg(v) = 6 + 2 = 8,$$vì nó liền kề với cái kia$6$đỉnh và có một vòng lặp đóng góp$2$đến mức độ. Kể từ đây$\deg(v)$thậm chí là dành cho tất cả$v$, vậy tồn tại mạch Euler. 

Để đếm các mạch Euler, chúng ta áp dụng công thức chuẩn cho đa đồ thị Euler vô hướng thu được từ định lý BEST áp dụng cho công thức đồ thị đường có hướng. Đối với đa đồ thị Euler được kết nối$G$, số mạch Euler là$$\tau(G)\,\prod_{v \in V} \left(\frac{\deg(v)}{2} - 1\right)!\,2^{|E| - |V| + 1},$$Ở đâu$\tau(G)$là số cây khung của đồ thị đơn bên dưới (các vòng lặp không ảnh hưởng đến cây khung). 

Đồ thị đơn giản cơ bản là$K_7$, do đó theo công thức Cayley,$$\tau(G) = 7^{7-2} = 7^5.$$Đối với mỗi đỉnh,$$\frac{\deg(v)}{2} - 1 = \frac{8}{2} - 1 = 3,$$vậy mỗi yếu tố là$3! = 6$. Với bảy đỉnh,$$\prod_{v \in V} \left(\frac{\deg(v)}{2} - 1\right)! = 6^7.$$Đồ thị có$|E| = 28$các cạnh và$|V| = 7$đỉnh, do đó$$2^{|E| - |V| + 1} = 2^{28 - 7 + 1} = 2^{22}.$$Nhân các đóng góp này sẽ được số mạch Euler:$$7^5 \cdot 6^7 \cdot 2^{22}.$$Do đó số chu kỳ domino là$$\boxed{7^5 \cdot 6^7 \cdot 2^{22}}.$$Điều này hoàn thành giải pháp. ∎
