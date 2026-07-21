---
title: "CF 103584B - Chân Ngỗng Trắng"
description: "Đặt $a{n-1}dots a1a0$ là một chuỗi nhị phân có $sum{j=0}^{n-1} aj=t$ và xác định $bj=ajoplus a{j-1}$ cho $1le jle n-1$. Năng lượng là $r=sum{j=1}^{n-1} bj.$ Mỗi $bj=1$ chính xác khi $ajne a{j-1}$, vì vậy $r$ bằng số lần chuyển đổi trong chuỗi $a0,a1,dots,a{n-1}$."
date: "2026-07-03T03:06:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103584
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 02-25-22 Div. 2 (Beginner)"
rating: 0
weight: 103584
solve_time_s: 66
verified: false
draft: false
---

[CF 103584B - Chân ngỗng trắng](https://codeforces.com/problemset/problem/103584/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$a_{n-1}\dots a_1a_0$là một chuỗi nhị phân với$\sum_{j=0}^{n-1} a_j=t$và xác định$b_j=a_j\oplus a_{j-1}$vì$1\le j\le n-1$. Năng lượng là$r=\sum_{j=1}^{n-1} b_j.$Mỗi$b_j=1$chính xác khi nào$a_j\ne a_{j-1}$, Vì thế$r$bằng số lần chuyển tiếp trong chuỗi$a_0,a_1,\dots,a_{n-1}$. Do đó chuỗi phân rã duy nhất thành$r+1$chạy liên tục tối đa. 

Hãy để những lần chạy này được viết từ trái sang phải như$u_1,u_2,\dots,u_{r+1},$Ở đâu$u_i\ge 1$Và$u_1+u_2+\cdots+u_{r+1}=n.$Đặt bit ban đầu là$\varepsilon\in{0,1}$. Các cuộc chạy xen kẽ nhau, vì vậy hãy chạy$i$bao gồm bit$\varepsilon \oplus (i-1)\bmod 2.$Hạn chế về trọng lượng$\sum_{j=0}^{n-1} a_j=t$trở thành một điều kiện tuyến tính trên chiều dài chạy. Xác định bộ chỉ mục$I_1(\varepsilon)=\{\,i\mid 1\le i\le r+1,\ \varepsilon\oplus(i-1)\equiv 1 \pmod 2\,\}.$Sau đó$\sum_{i\in I_1(\varepsilon)} u_i=t.$Do đó, mọi cấu hình được xác định duy nhất bằng cách chọn bit ban đầu$\varepsilon$và một sáng tác$(u_1,\dots,u_{r+1})$của$n$thành phần dương thỏa mãn ràng buộc tuyến tính ở trên. 

Để tạo từ điển, sẽ thuận tiện hơn khi thay thế thành phần bằng cách biểu diễn sự khác biệt tiêu chuẩn. Định nghĩa$p_0=u_1,\quad p_i=u_{i+1}-u_i\ \ (1\le i\le r),$để có thể$u_k=p_0+p_1+\cdots+p_{k-1}\quad (1\le k\le r+1).$các điều kiện$u_i\ge 1$dịch sang$p_0\ge 1,\qquad p_0+p_1+\cdots+p_k\ge k+1\ \ (0\le k\le r).$Điều kiện tổng tổng trở thành$n=\sum_{k=0}^{r} (r+1-k)p_k + \sum_{k=1}^{r} k p_k,$giúp đơn giản hóa việc phân đôi tiêu chuẩn giữa các thành phần và chuỗi tăng không âm; đặc biệt, mỗi chấp nhận được$(u_1,\dots,u_{r+1})$được biểu diễn duy nhất bởi thành phần của$n$vào trong$r+1$những phần tích cực. 

Do đó, việc tạo ra được giảm xuống còn việc tạo ra tất cả các thành phần của$n$vào trong$r+1$phần tích cực và kiểm tra ràng buộc trọng lượng. 

Về mặt thuật toán, hãy$(u_1,\dots,u_{r+1})$được tạo theo thứ tự từ điển như trong quá trình tạo thành phần tiêu chuẩn (tương đương, bằng cách lặp lại các vị trí của$r$dải phân cách giữa$n-1$khoảng trống). Đối với mỗi thành phần được tạo ra, hãy tính toán mức đóng góp trọng lượng theo tính chẵn lẻ của các lần chạy. 

Việc xây dựng từ điển trực tiếp được tiến hành như sau. Duy trì$u_1,\dots,u_{r+1}$với$u_i\ge 1$Và$\sum u_i=n$. Ở mỗi bước, hãy tăng chỉ số ngoài cùng bên phải$j$như vậy$u_j$có thể tăng lên trong khi vẫn cho phép hoàn thành với các phần tích cực:$u_j < n - \sum_{i<j} u_i - (r+1-j).$Sau khi tăng$u_j$, bộ$u_{j+1}=\cdots=u_{r+1}=1.$Điều này tạo ra tất cả các tác phẩm chính xác một lần theo thứ tự từ điển. 

Đối với mỗi thành phần được tạo ra, hãy tính$t'=\sum_{i\in I_1(\varepsilon)} u_i.$Cấu hình được chấp nhận chính xác khi$t'=t$. 

Để chứng minh tính đúng đắn, mỗi chuỗi nhị phân có năng lượng$r$mang lại một sự phân rã chạy duy nhất$(u_1,\dots,u_{r+1})$và một bit ban đầu duy nhất$\varepsilon$, do đó nó được tạo ra theo quy trình khi và chỉ khi thành phần liên quan của nó được tạo ra và thỏa mãn phương trình trọng số. Ngược lại, mỗi cặp được tạo$(\varepsilon,(u_1,\dots,u_{r+1}))$định nghĩa một chuỗi nhị phân duy nhất có cấu trúc chạy chính xác$r$chuyển tiếp và trọng lượng$t$bằng cách xây dựng phép gán xen kẽ và ràng buộc tổng. 

Điều này thiết lập sự song song giữa các cấu hình hợp lệ và các cặp được tạo được chấp nhận, do đó thuật toán tạo ra tất cả và chỉ các chuỗi mong muốn. 

Điều này hoàn thành việc chứng minh. ∎
