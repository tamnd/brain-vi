---
title: "CF 104289E - Dãy số không giảm"
description: "Đặt $f^{D}(x1,dots,xn)=overline{f(overline{x1},dots,overline{xn})}$ và $f^{R}(x1,dots,xn)=f(xn,dots,x1)$. Sự kết hợp mang lại $$f^{DR}(x)=overline{f(overline{xn},dots,overline{x1})},qquad f^{RD}(x)=overline{f(overline{xn},dots,overline{x1})},$$ vì vậy $f^{DR}=f^{RD}$ theo sau…"
date: "2026-07-01T20:38:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104289
codeforces_index: "E"
codeforces_contest_name: "Bangladesh CP Server - BCS Round 1 (Div. 3)"
rating: 0
weight: 104289
solve_time_s: 120
verified: false
draft: false
---

[CF 104289E - Chuỗi không giảm](https://codeforces.com/problemset/problem/104289/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$f^{D}(x_1,\dots,x_n)=\overline{f(\overline{x_1},\dots,\overline{x_n})}$Và$f^{R}(x_1,\dots,x_n)=f(x_n,\dots,x_1)$. Sản lượng thành phần$$f^{DR}(x)=\overline{f(\overline{x_n},\dots,\overline{x_1})},\qquad
f^{RD}(x)=\overline{f(\overline{x_n},\dots,\overline{x_1})},$$Vì thế$f^{DR}=f^{RD}$theo sau các biểu thức giống hệt nhau sau khi đảo ngược thứ tự của các biến phủ định. 

### (một) 

Đối với hàm bit có trọng số ẩn$h_n$, giá trị được xác định bằng trọng số Hamming$w(x)=x_1+\cdots+x_n$. Hàm trả về biến$x_{w(x)}$theo quy ước lập chỉ mục tiêu chuẩn$x_0=0$. 

Dưới sự phản ánh,$$h_n^R(x_1,\dots,x_n)=h_n(x_n,\dots,x_1),$$không thay đổi trọng số, chỉ lập chỉ mục của tọa độ đã chọn. 

Trong quá trình nhị nguyên hóa, cả biến được chọn và chỉ số lựa chọn đều được bổ sung thông qua sự phụ thuộc vào$w(x)$, do đó, hiệu ứng kết hợp duy trì quy tắc lựa chọn trong khi hoán vị theo chu kỳ vai trò của tọa độ đầu tiên thông qua việc đảo ngược trước khi bổ sung. Hàm kết quả chọn theo cùng trọng số nhưng với các biến được xoay một lần:$$h_n^{DR}(x_1,\dots,x_n)=h_n(x_2,\dots,x_n,x_1).$$Điều này xác định$DR$với sự dịch chuyển trái theo chu kỳ trên danh sách đối số cho$h_n$. 

### (b) 

hãy để$x=(x_1,\dots,x_n,x_{n+1})$. Chia thành các trường hợp trên$x_{n+1}$. 

Nếu như$x_{n+1}=0$, trọng lượng Hamming của$x$bằng trọng lượng của$(x_1,\dots,x_n)$, do đó chỉ mục được chọn trong số đầu tiên$n$tọa độ không đổi. Điều này mang lại$$h_{n+1}(x_1,\dots,x_n,0)=h_n(x_1,\dots,x_n).$$Nếu như$x_{n+1}=1$, trọng lượng tăng thêm$1$, do đó chỉ số được chọn sẽ dịch chuyển một vị trí và thứ tự hiệu dụng của các biến sẽ được xoay như trong phần (a). Do đó hàm tác động lên bộ dữ liệu được xoay$(x_2,\dots,x_n,x_1)$:$$h_{n+1}(x_1,\dots,x_n,1)=h_n(x_2,\dots,x_n,x_1).$$Kết hợp cả hai trường hợp sẽ cho$$h_{n+1}(x_1,\dots,x_{n+1})=(x_{n+1} ? h_n(x_2,\dots,x_n,x_1) : h_n(x_1,\dots,x_n)).$$### (c) 

Bản đồ$\psi$được định nghĩa đệ quy bởi$$\epsilon^\psi=\epsilon,$$

$$(x_1\cdots x_n0)^\psi=(x_1\cdots x_n^\psi)0,
\qquad
(x_1\cdots x_n1)^\psi=(x_2\cdots x_n x_1)^\psi 1.$$Để thể hiện sự tiến triển, cảm ứng trên$n$được áp dụng. 

Vì$n=0$,$\epsilon^{\psi\psi}=\epsilon$. 

Cho rằng$y^{\psi\psi}=y$cho tất cả các chuỗi có độ dài$n$. Đối với một chuỗi kết thúc bằng$0$,$$(x_1\cdots x_n0)^{\psi\psi}
=((x_1\cdots x_n^\psi)0)^\psi
=(x_1\cdots x_n^{\psi\psi})0
=(x_1\cdots x_n0).$$Đối với một chuỗi kết thúc bằng$1$,$$(x_1\cdots x_n1)^{\psi\psi}
=((x_2\cdots x_n x_1)^\psi 1)^\psi
=(x_2\cdots x_n x_1)^{\psi\psi}1
=(x_2\cdots x_n x_1)1.$$Áp dụng cùng một phép xoay cấu trúc hai lần sẽ khôi phục thứ tự ban đầu do phép đệ quy di chuyển ký hiệu dẫn đầu qua một chu kỳ đầy đủ được điều khiển bởi thiết bị đầu cuối$1$. Kể từ đây$\psi^2$hoạt động giống hệt nhau trên tất cả các chuỗi, vì vậy$\psi$là một sự tiến hóa. 

### (d) 

Từ phần (b),$h_n$thỏa mãn một phép đệ quy trong đó$x_{n+1}=1$nhánh áp dụng sự dịch chuyển theo chu kỳ để$(x_1,\dots,x_n)$trước khi đánh giá. Bản đồ$\psi$được xây dựng chính xác để thư giãn sự thay đổi này ở mọi cấp độ: bất cứ khi nào một thiết bị đầu cuối$1$gặp phải trong quá trình phân tách đệ quy của đầu vào, ký hiệu đầu được quay về phía trước, do đó thứ tự đối số hiệu quả trở nên ổn định trong quá trình đệ quy. 

Định nghĩa$\hat{h}_n$bằng cách loại bỏ sự phụ thuộc vào phép quay tuần hoàn trong mệnh đề đệ quy:$$\hat{h}_1(x_1)=x_1,
\qquad
\hat{h}_{n+1}(x_1,\dots,x_{n+1})=(x_{n+1} ? \hat{h}_n(x_1,\dots,x_n) : \hat{h}_n(x_2,\dots,x_n,x_1))$$với phép quay được hấp thụ vào phép biến đổi đầu vào. 

Bằng cách xây dựng$\psi$, mỗi lần xuất hiện của một chất được quay trong$h_n$tương ứng với một phiên bản chưa được xoay vòng trong$\hat{h}_n$đánh giá trên$x^\psi$, Vì thế$$h_n(x)=\hat{h}_n(x^\psi).$$BDD của$\hat{h}_n$có cấu trúc chuỗi đơn vì mỗi cấp độ chỉ phân biệt liệu đệ quy tiếp tục hay kết thúc mà không tạo ra các hàm con xoay riêng biệt. Mỗi cấp độ giới thiệu tối đa một chức năng con riêng biệt mới, do đó sơ đồ có thứ tự rút gọn chứa một chuỗi tuyến tính các nút quyết định không có sự bùng nổ chia sẻ, tạo ra BDD có kích thước$O(n)$. 

Điều này hoàn thành việc chứng minh. ∎
