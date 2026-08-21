---
title: "CF 104076C - Lệnh DFS 2"
description: "Cho $tau$ là bảng chân lý của $f(x1,ldots,xn)$, và $f^Z$ là hàm Boolean có bảng chân trị là $tau^Z$, trong đó $tau^Z$ được xác định bằng biến đổi Z đệ quy trong Bài tập 192."
date: "2026-07-02T02:46:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104076
codeforces_index: "C"
codeforces_contest_name: "2022 International Collegiate Programming Contest, Jinan Site"
rating: 0
weight: 104076
solve_time_s: 70
verified: false
draft: false
---

[CF 104076C - Đơn hàng DFS 2](https://codeforces.com/problemset/problem/104076/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\tau$là bảng chân lý của$f(x_1,\ldots,x_n)$, và để$f^Z$là hàm Boolean có bảng chân lý là$\tau^Z$, Ở đâu$\tau^Z$được xác định bởi biến đổi Z đệ quy trong Bài tập 192. 

hãy để$S_k(x_1,\ldots,x_n)$biểu thị chức năng phụ của$f$thu được bằng cách sửa chữa$x_1=\cdots=x_k=1$và để trống các biến còn lại, vì vậy bảng chân trị của nó là độ dài-$2^{n-k}$bảng phụ của$\tau$tương ứng với hậu tố được lập chỉ mục bởi bài tập$(1,\ldots,1, x_{k+1},\ldots,x_n)$. Cho phép$S_k^Z$biểu thị chức năng con tương tự của$f^Z$. 

Mục đích là xác định mối quan hệ giữa$S_k^Z$Và$S_k$. 

Biến đổi Z được xác định bằng cách phân tách đệ quy một chuỗi$\alpha\beta$tùy theo liệu$\beta$là khối 0, bằng$\alpha$, hoặc trường hợp nối chung. Cấu trúc duy nhất được bảo toàn qua cả ba mệnh đề là sự chia tách đệ quy thành các nửa có độ dài bằng nhau và nhận dạng các khối lặp lại hoặc khối 0. Điều này ngụ ý rằng phép biến đổi hoạt động theo từng cấp độ trên cây phân rã nhị phân của$\tau$. 

Ở độ sâu$k$trong phân rã bảng chân lý, chuỗi$\tau$được phân vùng thành$2^k$bảng phụ thứ tự$n-k$, mỗi cái tương ứng với việc sửa cái đầu tiên$k$các biến. Phép biến đổi Z không làm thay đổi việc lập chỉ mục của các bảng con này; nó chỉ thay thế mỗi bảng phụ$\sigma$qua$\sigma^Z$và có thể thay thế các cặp trùng lặp hoặc có cấu trúc bằng các khối số 0 hoặc khối lặp lại chuẩn. 

Do đó, mỗi bảng phụ xác định$S_k$được chuyển đổi độc lập thành bảng phụ tương ứng xác định$S_k^Z$. Đặc biệt, thao tác hạn chế “sửa lỗi đầu tiên”$k$biến” đi lại với phép biến đổi Z trên bảng chân lý. 

Về mặt hình thức, cho phép$\tau_{x_1=\cdots=x_k=1}$biểu thị hậu tố xác định bảng phụ$S_k$, định nghĩa đệ quy của$\tau^Z$sản lượng$$(\tau_{x_1=\cdots=x_k=1})^Z = (\tau^Z)_{x_1=\cdots=x_k=1}.$$Do đó bảng chân lý của$S_k^Z$chính xác là$(\tau_{x_1=\cdots=x_k=1})^Z$, có nghĩa là chính hàm con có được bằng cách áp dụng phép biến đổi Z cho hàm con$S_k$. 

Do đó, đối với mỗi$k$với$0 \le k \le n$,$$S_k^Z(x_1,\ldots,x_n) = (S_k(x_1,\ldots,x_n))^Z.$$Kể từ khi xác định hạn chế$S_k$giảm số lượng biến tự do xuống$n-k$, danh tính này giữ thống nhất trên tất cả các cấp độ phân rã hồ sơ và nó duy trì sự tương ứng giữa các chức năng con trong hồ sơ BDD của$f$và hồ sơ kiểu ZDD của$f^Z$được thiết lập trong Bài tập 192. 

Như vậy$$\boxed{S_k^Z = (S_k)^Z \quad \text{for } 0 \le k \le n.}$$Điều này hoàn thành việc chứng minh. ∎
