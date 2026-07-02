---
title: "CF 103600B - Mành"
description: "Đặt $Sigman = {0,1,2}^n$. Hai $n$-trits $x = (x1,dots,xn)$ và $y = (y1,dots,yn)$ liền kề nhau trong một mã ba ngôi chống Gray khi và chỉ nếu $xi neq yi$ với mỗi $1 le i le n$."
date: "2026-07-03T00:56:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103600
codeforces_index: "B"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2021"
rating: 0
weight: 103600
solve_time_s: 132
verified: false
draft: false
---

[CF 103600B - Rèm](https://codeforces.com/problemset/problem/103600/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\Sigma_n = {0,1,2}^n$. Hai$n$-trits$x = (x_1,\dots,x_n)$Và$y = (y_1,\dots,y_n)$liền kề nhau trong mã ba ngôi phản Gray khi và chỉ khi$x_i \neq y_i$cho mọi$1 \le i \le n$. Nhiệm vụ là xác định liệu có tồn tại một thứ tự tuần hoàn của tất cả các$3^n$các yếu tố của$\Sigma_n$sao cho các phần tử liên tiếp liền kề nhau theo nghĩa này. 

Viết phép cộng modulo$3$TRÊN${0,1,2}$và mở rộng nó theo chiều phối hợp để$\Sigma_n$. Vì$d \in {1,2}^n$, định nghĩa$x \oplus d = (x_1 + d_1,\dots,x_n + d_n)$. Sau đó$x$Và$y$liền kề nhau chính xác khi$y = x \oplus d$đối với một số người$d \in {1,2}^n$. Thực vậy,$d_i = y_i - x_i \not\equiv 0 \pmod 3$cho mọi$i$, Vì thế$d_i \in {1,2}$, và ngược lại bất kỳ điều gì như vậy$d$sản lượng$y_i \neq x_i$cho tất cả$i$. 

Như vậy cấu trúc cần tìm là chu trình Hamilton trong đồ thị Cayley của nhóm abelian$\mathbb{Z}_3^n$với tổ máy phát điện${1,2}^n$. Việc xây dựng tiến hành bằng quy nạp trên$n$. 

Vì$n = 1$, trình tự$$(0,1,2,0)$$thăm tất cả các yếu tố của$\Sigma_1$đúng một lần và thỏa mãn$0 \neq 1 \neq 2 \neq 0$, do đó điều kiện được giữ. 

Giả sử một thứ tự tuần hoàn$$x_0, x_1, \dots, x_{3^n-1}, x_0$$của$\Sigma_n$tồn tại sao cho$x_k$Và$x_{k+1}$khác nhau ở mọi tọa độ cho tất cả$k$, trong đó các chỉ số được lấy modulo$3^n$. Đặc biệt,$x_{3^n-1}$khác với$x_0$ở mọi tọa độ. 

Xác định ba bản sao của chu kỳ này trong$\Sigma_{n+1} = \Sigma_n \times {0,1,2}$. Vì$r \in {0,1,2}$, định nghĩa$$x_k^{(r)} = (x_k, r).$$Xây dựng trình tự$$x_0^{(0)}, x_1^{(0)}, \dots, x_{3^n-1}^{(0)},\;
x_0^{(1)}, x_1^{(1)}, \dots, x_{3^n-1}^{(1)},\;
x_0^{(2)}, x_1^{(2)}, \dots, x_{3^n-1}^{(2)},\;
x_0^{(0)}.$$Mỗi lần chuyển đổi bên trong một lớp cố định$r$có hình thức$$(x_k, r) \to (x_{k+1}, r),$$và kể từ đó$x_k$Và$x_{k+1}$khác nhau ở mọi tọa độ của$\Sigma_n$, tương ứng$(n+1)$-các bộ dữ liệu khác nhau ở tất cả$n+1$tọa độ. 

Sự chuyển tiếp giữa các lớp là$$(x_{3^n-1}, 0) \to (x_0, 1),$$và với mọi tọa độ$i \le n$một người có$x_{3^n-1,i} \neq x_{0,i}$bởi thuộc tính chu kỳ, trong khi$0 \neq 1$ở tọa độ cuối cùng. Do đó bước này cũng khác nhau ở mỗi tọa độ. Lập luận tương tự áp dụng cho việc chuyển đổi từ lớp$1$để lớp$2$và từ lớp$2$quay lại lớp$0$, bởi vì phép cộng bởi$1 \pmod 3$thay đổi mỗi mục. 

Chuỗi kết quả chứa chính xác$3 \cdot 3^n = 3^{n+1}$các yếu tố riêng biệt của$\Sigma_{n+1}$và mỗi cặp liên tiếp sẽ khác nhau ở mọi tọa độ. Nó mang tính tuần hoàn do xây dựng. 

Điều này hoàn thành việc xây dựng quy nạp của mã ternary anti-Gray cho tất cả$n \ge 1$. ∎
