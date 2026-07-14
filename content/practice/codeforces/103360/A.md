---
title: "CF 103360A - \u0419\u043e\u0434\u0430 \u0438 \u043f\u0440\u0438\u0441\u0442\u0430\u0432\u043a\u0438 \u0421\u0438"
description: "Cho $n = s + t$ như trong (1), và xét một tổ hợp $t$-$ct cdots c1$ với $n ct cdots c1 ge 0$ cùng với giới hạn kề bổ sung $c{j+1} cj + 1 qquad (t j ge 1).$ Xác định các số nguyên mới $c'j = cj - (j-1), qquad 1 le j le t."
date: "2026-07-03T13:14:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103360
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020"
rating: 0
weight: 103360
solve_time_s: 149
verified: false
draft: false
---

[CF 103360A - \u0419\u043e\u0434\u0430 \u0438 \u043f\u0440\u0438\u0441\u0442\u0430\u0432\u043a\u0438 \u0421\u0438](https://codeforces.com/problemset/problem/103360/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$n = s + t$như trong (1), và xem xét một$t$-sự kết hợp$c_t \cdots c_1$với$n > c_t > \cdots > c_1 \ge 0$cùng với hạn chế lân cận bổ sung$c_{j+1} > c_j + 1 \qquad (t > j \ge 1).$Xác định số nguyên mới$c'_j = c_j - (j-1), \qquad 1 \le j \le t.$Từ$c_{j+1} \ge c_j + 2$nó theo sau đó$c'_{j+1} = c_{j+1} - j \ge c_j + 2 - j = (c_j - (j-1)) + 1 = c'_j + 1,$kể từ đây$c'_t > c'_{t-1} > \cdots > c'_1 \ge 0.$Như vậy$c'_t \cdots c'_1$là một sự nghiêm khắc thông thường$t$-tổ hợp theo nghĩa (3), được hình thành từ một tập hợp các số nguyên liên tiếp. 

Từ$c_t \le n-1$người ta có được$c'_t = c_t - (t-1) \le n-1-(t-1) = n-t,$Và$c'_1 \ge 0$giữ trực tiếp từ$c_1 \ge 0$. Vì thế mỗi$c'_j$nằm ở${0,1,\ldots,n-t}$. 

Ngược lại, với bất kỳ sự kết hợp thông thường nào$n-t \ge c'_t > \cdots > c'_1 \ge 0,$định nghĩa$c_j = c'_j + (j-1).$Khi đó $c_{j+1} \ge c'_{j+1} + j \ge (c'_j + 1) + j = (c'_j + (j-1)) + 2 = c_j + 2,$$ 

vậy điều kiện kề$c_{j+1} > c_j + 1$giữ, và cũng$c_t \le n-1$theo sau từ$c'_t \le n-t$. Sự tái tạo này đảo ngược sự biến đổi, do đó nó mang tính chất phỏng đoán giữa các tổ hợp hợp âm piano hợp lệ và thông thường.$t$-kết hợp trên${0,1,\ldots,n-t}$. 

Do đó, bài toán người chơi đàn piano với hạn chế kề cận tương đương với việc tạo ra tất cả$t$-sự kết hợp của một$(n-t+1)$-bộ phần tử. Theo (1.2.6-2), số của chúng là$\binom{n-t+1}{t},$vì bộ mặt đất bên dưới có các phần tử$0,1,\ldots,n-t$. 

Để tạo tất cả các hợp âm như vậy theo từ điển, hãy áp dụng Thuật toán$L$của Mục 7.2.1.3 đối với các biến được chuyển đổi$c'_t \cdots c'_1$, rồi xuất ra$c_j = c'_j + (j-1) \qquad (1 \le j \le t).$Điều này tạo ra tất cả các hợp âm có thể chấp nhận được chính xác một lần, duy trì trật tự từ điển dưới sự dịch chuyển affine. 

Điều này hoàn thành việc chứng minh. ∎
