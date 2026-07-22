---
title: "CF 103666H - \u0420\u043e\u0431\u043e\u0442"
description: "Biểu diễn mỗi domino ${i,j}$, $0 le i le j le 6$, dưới dạng một cạnh vô hướng giữa các đỉnh $i$ và $j$ trong đồ thị nhiều đồ thị $G$ trên tập đỉnh ${0,1,dots,6}$, với một vòng lặp ở mỗi đỉnh $i$ tương ứng với ${i,i}$."
date: "2026-07-03T02:20:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103666
codeforces_index: "H"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2016"
rating: 0
weight: 103666
solve_time_s: 138
verified: false
draft: false
---

[CF 103666H - \u0420\u043e\u0431\u043e\u0442](https://codeforces.com/problemset/problem/103666/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Đại diện cho mỗi domino${i,j}$,$0 \le i \le j \le 6$, như một cạnh vô hướng giữa các đỉnh$i$Và$j$trong đa đồ thị$G$trên tập đỉnh${0,1,\dots,6}$, với một vòng lặp ở mỗi đỉnh$i$tương ứng với${i,i}$. Một sự sắp xếp tuần hoàn hợp lệ của tất cả 28 quân domino chính xác là một chu trình Euler trong$G$, vì mỗi domino được sử dụng một lần và các domino liên tiếp phải có chung một điểm cuối. 

Để áp dụng định lý liệt kê mạch Euler, hãy thay thế$G$bằng đồ thị đa giác Euler có hướng$D$thu được bằng cách thay thế mỗi cạnh${i,j}$với hai cung$i \to j$Và$j \to i$khi$i \ne j$và thay thế từng vòng lặp${i,i}$bởi một cung vòng đơn tại$i$. TRONG$D$, mỗi cạnh không vòng lặp đóng góp một cung đi ra tại mỗi điểm cuối và mỗi vòng lặp đóng góp một cung đi ra và một cung đi vào ở đỉnh của nó. 

Đối với mỗi đỉnh$v$, có$6$hàng xóm$w \ne v$, đóng góp$6$vòng cung đi$v \to w$và một vòng lặp đóng góp một cung đi$v \to v$. Kể từ đây$\operatorname{outdeg}(v) = 7,$và tương tự$\operatorname{indeg}(v)=7$, Vì thế$D$là Euler. 

Định lý TỐT NHẤT cho biết số chu trình Euler trong đồ thị Euler có hướng bắt nguồn từ một đỉnh bắt đầu cố định$r$BẰNG$$N_r = \tau_r(D)\,\prod_{v} (\operatorname{outdeg}(v)-1)!,$$Ở đâu$\tau_r(D)$là số lượng các cụm cây trải dài có hướng hướng về phía$r$. 

Vì mọi cặp đỉnh phân biệt trong$D$được nối theo cả hai hướng và các vòng lặp không ảnh hưởng đến các dạng cây,$\tau_r(D)$bằng số lượng các chùm hoa trong đồ thị hai hướng hoàn chỉnh trên$7$các đỉnh, đó là$$\tau_r(D) = 7^{7-2} = 7^5.$$Đối với mỗi đỉnh,$$(\operatorname{outdeg}(v)-1)! = 6!,$$kể từ đây$$\prod_v (\operatorname{outdeg}(v)-1)! = (6!)^7.$$Do đó số mạch Euler trong$D$bắt đầu từ một đỉnh cố định là$$N_r = 7^5 (6!)^7.$$Mỗi mạch Euler tương ứng với một trật tự tuần hoàn của 28 quân domino. Một chu trình không phụ thuộc vào điểm xuất phát và hướng. Một chu kỳ dài$28$có chính xác$28$lựa chọn cạnh bắt đầu và$2$định hướng, tất cả đều mang lại sự sắp xếp theo chu kỳ giống nhau. Do đó số chu kỳ phân biệt là$$\frac{7^5 (6!)^7}{28 \cdot 2} = \frac{7^5 (6!)^7}{56}.$$Từ$56 = 7 \cdot 8$, điều này đơn giản hóa thành$$\frac{7^4 (6!)^7}{8}.$$Do đó số cách sắp xếp tuần hoàn hợp lệ là$$\boxed{\frac{7^5 (6!)^7}{56}}.$$∎
