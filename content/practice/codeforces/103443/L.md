---
title: "CF 103443L - Chân Chì"
description: "Giả sử chuỗi bit đã cho được hiểu là một tổ hợp $(s,t)$-với các số 0 $s=12$ và $t=14$, do đó $n=s+t=26$. Chuỗi là $11001001000011111101101010.$ Chuỗi của Chase $C{st}$, như được định nghĩa trong phương trình (41), là thứ tự tạo trên các tổ hợp $(s,t)$."
date: "2026-07-03T07:44:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103443
codeforces_index: "L"
codeforces_contest_name: "The 2021 ICPC Asia Taipei Regional Programming Contest"
rating: 0
weight: 103443
solve_time_s: 112
verified: false
draft: false
---

[CF 103443L - Chân chì](https://codeforces.com/problemset/problem/103443/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hãy để chuỗi bit đã cho được hiểu là một$(s,t)$-kết hợp với$s=12$số không và$t=14$những cái đó, do đó$n=s+t=26$. Chuỗi là$11001001000011111101101010.$Trình tự của Chase$C_{st}$, như được định nghĩa trong phương trình (41), là thứ tự tạo trên$(s,t)$-sự kết hợp. Trong bài tập này, nó được sử dụng ở dạng từ điển trên các chuỗi bit, do đó thứ hạng của một tổ hợp được xác định bằng quy tắc đếm từ điển tiêu chuẩn: tại mỗi vị trí, nếu một$1$xuất hiện, chúng tôi có thể thay thế nó bằng một$0$và đếm tất cả các lần hoàn thành với số lần hoàn thành còn lại. 

Cho phép$r$biểu thị số lượng còn lại$1$-bit vẫn được đặt. Ban đầu$r=14$. Tại vị trí$i$, nếu bit đó là$1$, sau đó đặt nó thành$0$buộc một số lượng$\binom{26-i}{r}$hoàn thành trong số các vị trí còn lại. Sau khi xử lý một$1$, chúng tôi cập nhật$r \leftarrow r-1$. 

Chúng tôi quét chuỗi từ trái sang phải. 

Tại vị trí$1$, bit là$1$, vậy phần đóng góp là$\binom{25}{14}=\binom{25}{11}=4,457,400$. Sau đó$r=13$. 

Tại vị trí$2$, bit là$1$, vậy phần đóng góp là$\binom{24}{13}=\binom{24}{11}=2,496,144$. Sau đó$r=12$. 

Vị trí$3,4$là$0$, không đóng góp gì 

Tại vị trí$5$, bit là$1$, vậy phần đóng góp là$\binom{21}{12}=\binom{21}{9}=293,930$. Sau đó$r=11$. 

Vị trí$6,7$là$0$. 

Tại vị trí$8$, bit là$1$, vậy phần đóng góp là$\binom{18}{11}=\binom{18}{7}=31,824$. Sau đó$r=10$. 

Vị trí$9,10,11,12$là$0$. 

Tại vị trí$13$, bit là$1$, vậy phần đóng góp là$\binom{13}{10}=\binom{13}{3}=286$. Sau đó$r=9$. 

Tại vị trí$14$, bit là$1$, vậy phần đóng góp là$\binom{12}{9}=\binom{12}{3}=220$. Sau đó$r=8$. 

Tại vị trí$15$, bit là$1$, vậy phần đóng góp là$\binom{11}{8}=\binom{11}{3}=165$. Sau đó$r=7$. 

Tại vị trí$16$, bit là$1$, vậy phần đóng góp là$\binom{10}{7}=\binom{10}{3}=120$. Sau đó$r=6$. 

Tại vị trí$17$, bit là$1$, vậy phần đóng góp là$\binom{9}{6}=\binom{9}{3}=84$. Sau đó$r=5$. 

Tại vị trí$18$, bit là$1$, vậy phần đóng góp là$\binom{8}{5}=\binom{8}{3}=56$. Sau đó$r=4$. 

Chức vụ$19$là$0$. 

Tại vị trí$20$, bit là$1$, vậy phần đóng góp là$\binom{6}{4}=\binom{6}{2}=15$. Sau đó$r=3$. 

Tại vị trí$21$, bit là$1$, vậy phần đóng góp là$\binom{5}{3}=10$. Sau đó$r=2$. 

Chức vụ$22$là$0$. 

Tại vị trí$23$, bit là$1$, vậy phần đóng góp là$\binom{3}{2}=3$. Sau đó$r=1$. 

Chức vụ$24$là$0$. 

Tại vị trí$25$, bit là$1$, vậy phần đóng góp là$\binom{2}{1}=2$. Sau đó$r=0$. 

Chức vụ$26$là$0$. 

Tổng hợp mọi đóng góp$$\begin{aligned}
&4\,457\,400 + 2\,496\,144 + 293\,930 + 31\,824 + 286 + 220 + 165 + 120 + 84 + 56 + 15 + 10 + 3 + 2 \\
&= 7\,280\,259.
\end{aligned}$$Do đó, số lượng kết hợp trước chuỗi bit đã cho trong$C_{st}$là$\boxed{7\,280\,259}.$Điều này hoàn thành việc tính toán. ∎
