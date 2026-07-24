---
title: "CF 103808B - Desordenado"
description: "Giả sử $f(x1,dots,xn)$ được biểu diễn bằng một BDD rút gọn có thứ tự với nút gốc $r$. Đối với mỗi nút $k$ trong BDD, hãy viết $V(k)$ cho chỉ mục biến của nó và viết $mathrm{LO}(k)$ và $mathrm{HI}(k)$ cho hai nút kế tiếp của nó. Bồn rửa là $bot$ và $top$."
date: "2026-07-02T08:38:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103808
codeforces_index: "B"
codeforces_contest_name: "XXVI Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 103808
solve_time_s: 124
verified: false
draft: false
---

[CF 103808B - Desordenado](https://codeforces.com/problemset/problem/103808/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$f(x_1,\dots,x_n)$được biểu diễn bằng một BDD rút gọn có thứ tự với nút gốc$r$. Đối với mỗi nút$k$trong BDD, viết$V(k)$cho chỉ số biến của nó và viết$\mathrm{LO}(k)$Và$\mathrm{HI}(k)$cho hai người kế nhiệm của nó. Các bồn rửa là$\bot$Và$\top$. 

Đối với mỗi nút$k$, định nghĩa hàm Boolean$f_k$về các biến$x_{V(k)}, x_{V(k)+1}, \dots, x_n$là chức năng con được đại diện bởi BDD bắt nguồn từ$k$. Cho phép$T(k)$biểu thị bảng chân lý của$f_k$theo thứ tự được sử dụng trong Phần 7.1.1, cụ thể là thứ tự từ điển trên$(x_{V(k)},\dots,x_n)$bắt đầu từ tất cả số không. 

Cấu trúc của BDD ngụ ý sự phân rã$$f_k(x_{V(k)},\dots,x_n) = 
\begin{cases}
f_{\mathrm{LO}(k)}(x_{V(k)+1},\dots,x_n) & \text{if } x_{V(k)}=0,\\
f_{\mathrm{HI}(k)}(x_{V(k)+1},\dots,x_n) & \text{if } x_{V(k)}=1.
\end{cases}$$Danh tính này xác định sự phân rã tương ứng của các bảng chân lý. Nếu như$T(k)$có chiều dài$2^m$, Ở đâu$m = n - V(k) + 1$, thì mỗi$T(\mathrm{LO}(k))$Và$T(\mathrm{HI}(k))$có chiều dài$2^{m-1}$. Quy ước sắp xếp cho các bảng chân lý ngụ ý rằng nửa đầu của$T(k)$tương ứng với$x_{V(k)}=0$và nửa thứ hai tương ứng với$x_{V(k)}=1$. Vì thế$$T(k) = T(\mathrm{LO}(k)) \, T(\mathrm{HI}(k)).$$Đối với sink, các hàm con tương ứng là hằng số. Nếu như$k=\bot$, sau đó$f_k$giống hệt nhau$0$, kể từ đây$$T(\bot) = 00\cdots 0 \quad (2^m \text{ zeros for the appropriate } m).$$Nếu như$k=\top$, sau đó$f_k$giống hệt nhau$1$, kể từ đây$$T(\top) = 11\cdots 1.$$Thuật toán C được sửa đổi bằng cách thay thế tổ hợp số học của các kết quả con bằng cách nối các chuỗi nhị phân. Việc đánh giá tiến hành từ dưới lên trên BDD theo thứ tự tôpô ngược được tạo ra bởi thứ tự biến. Đối với mỗi nút$k$, một lần$T(\mathrm{LO}(k))$Và$T(\mathrm{HI}(k))$đã được tính toán, giá trị$T(k)$được hình thành bằng cách nối. Vì BDD bị giảm nên mỗi nút được tính một lần và vì nó được sắp xếp theo thứ tự nên tất cả các phần phụ thuộc đều được chuyển tiếp nghiêm ngặt trong chỉ mục biến, do đó thứ tự đánh giá này được xác định rõ. 

Tính đúng đắn được thực hiện bằng quy nạp trên chỉ số thay đổi. Đối với bồn rửa, câu lệnh đúng theo định nghĩa. Đối với một nút nhánh$k$, cho rằng$T(\mathrm{LO}(k))$Và$T(\mathrm{HI}(k))$biểu diễn chính xác các hàm con thu được bằng cách sửa$x_{V(k)}=0$Và$x_{V(k)}=1$. Cấu trúc từ điển của bảng chân lý chia chính xác thành hai nửa này, do đó phép nối sẽ mang lại chính xác bảng chân lý của$f_k$. Điều này hoàn thành bước quy nạp. 

Áp dụng việc xây dựng cho nút gốc sẽ tạo ra$T(r)$, đó là bảng chân lý được xây dựng đầy đủ của$f$trong tiêu chuẩn$2^n$-bit thứ tự. 

Do đó, đầu ra của Thuật toán C đã sửa đổi$T(r)$, bảng chân lý của$f$.$$\boxed{T(r)}$$
