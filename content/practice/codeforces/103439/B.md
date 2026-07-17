---
title: "CF 103439B - Truy vấn mới về phân khúc cao cấp"
description: "Giả sử chuỗi bit đã cho là $a{25}dots a0$, trong đó $s=12$ số 0 và $t=14$ số một. Trong chuỗi $C{st}$ của Chase như được định nghĩa trong (41), các kết hợp liên tiếp có được bằng cách hoán đổi một mẫu liền kề $10 leftrightarrow 01$, do đó, một nước đi sẽ hoán đổi $1$ với $0$ lân cận."
date: "2026-07-03T07:46:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103439
codeforces_index: "B"
codeforces_contest_name: "XXII Open Cup, Grand Prix of Southeastern Europe"
rating: 0
weight: 103439
solve_time_s: 131
verified: false
draft: false
---

[CF 103439B - Truy vấn mới trên phân đoạn cao cấp](https://codeforces.com/problemset/problem/103439/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Đặt chuỗi bit đã cho là$a_{25}\dots a_0$, Ở đâu$s=12$số không và$t=14$những cái đó. Theo trình tự của Chase$C_{st}$như được định nghĩa trong (41), các kết hợp liên tiếp thu được bằng cách trao đổi một mẫu liền kề$10 \leftrightarrow 01$, do đó một nước đi sẽ hoán đổi một$1$với người hàng xóm$0$. 

Trình tự bắt đầu với cấu hình trong đó tất cả các số 0 đứng trước tất cả các số 1, cụ thể là$00\dots 011\dots 1.$Từ điểm xuất phát này, bất kỳ$1$phải di chuyển sang trái qua mỗi$0$ban đầu nằm ở bên trái cho đến khi đạt đến vị trí cuối cùng trong chuỗi đích. Mỗi trao đổi như vậy là một trao đổi liền kề của một$10$đôi. Do đó, số bước cần thiết để đạt được một cấu hình nhất định bằng số lần đảo ngược dạng$(1,0)$, nghĩa là cặp vị trí$i<j$với$a_i=1$Và$a_j=0$. 

Đối với mỗi vị trí$i$chứa một$1$, cho phép$Z(i)$biểu thị số lượng số không ở các vị trí$i,i+1,\dots,25$. Mỗi số 0 như vậy đóng góp chính xác một lần hoán đổi với$1$ở vị trí$i$, vậy tổng số lần hoán đổi cần thiết là$\sum_{a_i=1} Z(i).$Chuỗi đã cho là$11001001000011111101101010.$Tính toán$Z(i)$bằng cách đếm hậu tố số 0 từ phải sang trái. Các giá trị kết quả là:$$\begin{aligned}
Z(1)&=12,\quad Z(2)=12,\quad Z(5)=10,\quad Z(8)=8,\\
Z(13)&=4,\quad Z(14)=4,\quad Z(15)=4,\quad Z(16)=4,\quad Z(17)=4,\quad Z(18)=4,\\
Z(20)&=3,\quad Z(21)=3,\quad Z(23)=2,\quad Z(25)=1.
\end{aligned}$$Tính tổng tất cả các vị trí có chứa a$1$cho$$12+12+10+8+4+4+4+4+4+4+3+3+2+1=75.$$Giá trị này là số lượng trao đổi liền kề cần thiết để chuyển đổi cấu hình ban đầu thành chuỗi bit đã cho trong chuỗi Chase$C_{st}$, do đó nó bằng số lượng kết hợp trước nó. 

Do đó số tổ hợp trước chuỗi bit đã cho là$\boxed{75}.$Điều này hoàn thành giải pháp. ∎
