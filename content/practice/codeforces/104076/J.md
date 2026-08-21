---
title: "CF 104076J - Kỹ năng"
description: "Cho $f(x1,ldots,xn)$ có bảng chân lý $tau$, và cho $f^Z$ có bảng chân lý $tau^Z$. Với $0 le k le n$, hãy $Sk(x1,ldots,xn)$ biểu thị hàm con thu được bằng cách sửa $x1=cdots=xk=1$, do đó bảng chân trị của nó là bảng con của $tau$ theo thứ tự $n-k$ bắt đầu từ vị trí $2^k$ trong…"
date: "2026-07-02T02:50:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104076
codeforces_index: "J"
codeforces_contest_name: "2022 International Collegiate Programming Contest, Jinan Site"
rating: 0
weight: 104076
solve_time_s: 127
verified: false
draft: false
---

[CF 104076J - Kỹ năng](https://codeforces.com/problemset/problem/104076/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$f(x_1,\ldots,x_n)$có bảng sự thật$\tau$, và để$f^Z$có bảng sự thật$\tau^Z$. Vì$0 \le k \le n$, cho phép$S_k(x_1,\ldots,x_n)$biểu thị hàm con thu được bằng cách sửa$x_1=\cdots=x_k=1$, vậy bảng chân lý của nó là bảng phụ của$\tau$trật tự$n-k$bắt đầu ở vị trí$2^k$theo thứ tự từ điển của đầu vào. Mục đích là xác định hàm con tương ứng$S_k^Z$của$f^Z$. 

Biến đổi Z được xác định đệ quy trên các phép nối$\alpha\beta$bằng cách phân tách các khối có độ dài bằng nhau và thay thế các phép nối có cấu trúc nhất định bằng cách sao chép, đệm bằng 0 hoặc ứng dụng đệ quy của phép biến đổi thành các khối nhỏ hơn. Mọi mệnh đề trong định nghĩa đều bảo toàn thuộc tính là phép biến đổi hoạt động độc lập trên các khối tương ứng với tiền tố cố định của biến. Phân rã bảng chân lý thành các bảng con bằng cách sửa$x_1,\ldots,x_k$chỉ phụ thuộc vào điều đầu tiên$k$mức độ của cấu trúc khối đệ quy này. 

Nhận xét quan trọng là định nghĩa của$\tau^Z$tương thích với hạn chế đối với bất kỳ phân đoạn ban đầu nào của thứ tự biến. Nếu như$\tau$được viết dưới dạng nối của$2^k$khối có chiều dài$2^{n-k}$tương ứng với nhiệm vụ của$(x_1,\ldots,x_k)$, thì mỗi mệnh đề trong biến đổi Z áp dụng thống nhất cho toàn bộ khối hoặc đệ quy bên trong các khối có cấu trúc bằng nhau. Không có mệnh đề nào trộn các bit từ các khối khác nhau được xác định bởi khối đầu tiên$k$các biến. 

Vì vậy việc hạn chế về$\tau^Z$thu được bằng cách sửa chữa$x_1=\cdots=x_k=1$chính xác là biến đổi Z của chuỗi bị hạn chế$\tau_{x_1=\cdots=x_k=1}$. Điều này mang lại danh tính của các bảng chân lý$$(\tau_{x_1=\cdots=x_k=1})^Z = (\tau^Z)_{x_1=\cdots=x_k=1}.$$Giải thích cả hai vế dưới dạng hàm Boolean sẽ cho rằng hàm con của$f^Z$tương ứng với việc sửa lỗi đầu tiên$k$các biến bằng biến đổi Z của hàm con tương ứng của$f$. Trong phần ký hiệu của bài tập,$$S_k^Z(x_1,\ldots,x_n) = (S_k(x_1,\ldots,x_n))^Z.$$Vì sự bình đẳng này đúng với mọi$k$với$0 \le k \le n$, toàn bộ$k$-hồ sơ của$f^Z$thu được bằng cách áp dụng biến đổi Z theo cấp độ cho$k$-hồ sơ của$f$, khớp với sự tương ứng được thiết lập trong Bài tập 192 giữa các biên dạng và biên dạng z. 

Như vậy$$\boxed{S_k^Z = (S_k)^Z \quad \text{for all } 0 \le k \le n.}$$Điều này hoàn thành việc chứng minh. ∎
