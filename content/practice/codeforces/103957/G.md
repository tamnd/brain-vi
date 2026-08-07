---
title: "CF 103957G - Di Sản Của Khoảng Không"
description: "Bảng chân trị thứ tự $n$ là một chuỗi nhị phân có độ dài $2^n$. Một hạt là một bảng chân trị $beta$ không có dạng $alphaalpha$. Một hàm Boolean sẽ tốt nếu mọi bảng con thu được bằng cách sửa bất kỳ tiền tố nào của biến đều là một chuỗi hạt."
date: "2026-07-02T06:52:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103957
codeforces_index: "G"
codeforces_contest_name: "2015 ACM-ICPC Asia EC-Final Contest"
rating: 0
weight: 103957
solve_time_s: 121
verified: false
draft: false
---

[CF 103957G - Di sản của khoảng trống](https://codeforces.com/problemset/problem/103957/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Bảng chân lý thứ tự$n$là một chuỗi nhị phân có độ dài$2^n$. Một hạt là một bảng sự thật$\beta$đó không phải là hình thức$\alpha\alpha$. Một hàm Boolean sẽ tốt nếu mọi bảng con thu được bằng cách sửa bất kỳ tiền tố nào của biến đều là một chuỗi hạt. 

Cho phép$S(n)$biểu thị số lượng hàm Boolean ngọt ngào của$n$các biến. Mục tiêu là tính toán$S(n)$vì$n\le 7$. 

Một chức năng con ở cấp độ$k$tương ứng với việc sửa chữa$x_1,\dots,x_k$, tạo ra bảng chân lý thứ tự$n-k$. Sự ngọt ngào đòi hỏi mỗi bảng phụ như vậy đều là một hạt. 

## Giải pháp 

Một bảng sự thật$\tau$trật tự$m$là một hạt khi và chỉ nếu hai bảng con thứ tự của nó$m-1$khác nhau. Do đó, một hàm con của thứ tự$m$là một hạt khi và chỉ khi giới hạn LO và HI của nó đối với$x_1$là khác biệt. 

Hàm Boolean là tốt khi và chỉ khi, với mọi hàm con$\tau$phát sinh từ bất kỳ sự gán tiền tố nào, cặp cảm ứng$(\tau_0,\tau_1)$thỏa mãn$\tau_0\ne \tau_1$. 

Điều kiện này tương đương với việc yêu cầu mọi nút trong cây quyết định nhị phân đầy đủ của hàm phải có các hàm con LO và HI riêng biệt. Vì các hàm con được xác định hoàn toàn bởi bảng chân trị của chúng nên đẳng thức của các nút tương ứng chính xác với đẳng thức của các bảng con. 

Do đó, cấu trúc được tạo ra bởi hàm ngọt là một cây quyết định nhị phân đầy đủ có độ sâu$n$trong đó mỗi nút được gắn nhãn bởi một hàm con riêng biệt và mỗi nút có thứ tự$m>0$được xác định bởi một cặp hàm con riêng biệt có thứ tự$m-1$. 

Cho phép$T(m)$biểu thị tập hợp các hàm con riêng biệt xuất hiện ở cấp độ$m$trong một chức năng ngọt ngào trên$n$các biến. Ở cấp độ$0$có chính xác một hàm, hàm gốc. Ở mỗi bước sàng lọc từ cấp độ$m-1$để lên cấp$m$, mỗi hàm con$\tau$trật tự$n-m+1$được thay thế bằng một cặp có thứ tự$(\tau_0,\tau_1)$của các chức năng con riêng biệt của trật tự$n-m$. 

Do đó, số lượng các sàng lọc có thể chấp nhận được của một hàm con duy nhất là$S(n-m) (S(n-m)-1)$, từ$\tau_0$có thể được chọn tùy ý từ$S(n-m)$Và$\tau_1$từ phần còn lại$S(n-m)-1$khả năng. 

Ở cấp độ$m-1$có$2^{m-1}$các hàm con và các sàng lọc của chúng là độc lập vì các nút riêng biệt tương ứng với các bài toán con riêng biệt được xác định bởi các phép gán khác nhau của nút đầu tiên.$m-1$các biến. Do đó tổng số cách để mở rộng hàm ngọt từ độ sâu$m-1$đến độ sâu$m$là$$\bigl(S(n-m)(S(n-m)-1)\bigr)^{2^{m-1}}.$$Cài đặt$m=n$gây ra sự tái phát cho$S(n)$về mặt$S(n-1)$:$$S(n) = \bigl(S(n-1)(S(n-1)-1)\bigr)^{2^{n-1}}.$$Điều kiện ban đầu là$S(0)=2$, vì hàm Boolean duy nhất có biến 0 là các hằng số$0$Và$1$, cả hai hạt. 

Bây giờ hãy tính lần lượt. 

Vì$n=1$,$$S(1) = (S(0)(S(0)-1))^{1} = (2\cdot 1)^1 = 2.$$Vì$n=2$,$$S(2) = (S(1)(S(1)-1))^{2} = (2\cdot 1)^2 = 4.$$Vì$n=3$,$$S(3) = (4\cdot 3)^4 = 12^4 = 20736.$$Vì$n=4$,$$S(4) = (20736\cdot 20735)^8.$$Tính toán sản phẩm:$$20736\cdot 20735 = 20736(20736-1) = 20736^2 - 20736.$$Từ$20736^2 = 429981696$, phép trừ cho$$20736\cdot 20735 = 429981696 - 20736 = 429960960.$$Như vậy$$S(4) = (429960960)^8.$$Vì$n=5$,$$S(5) = \bigl((429960960)^8((429960960)^8-1)\bigr)^{16}.$$Vì$n=6$,$$S(6) = \Bigl(S(5)(S(5)-1)\Bigr)^{32}.$$Vì$n=7$,$$S(7) = \Bigl(S(6)(S(6)-1)\Bigr)^{64}.$$Do đó, các giá trị được xác định hoàn toàn bởi sự tái diễn. 

Do đó, đối với$n\le 7$,$$\boxed{
\begin{aligned}
S(0)&=2,\\
S(1)&=2,\\
S(2)&=4,\\
S(3)&=20736,\\
S(4)&=(429960960)^8,\\
S(5)&=\bigl((429960960)^8((429960960)^8-1)\bigr)^{16},\\
S(6)&=\Bigl(S(5)(S(5)-1)\Bigr)^{32},\\
S(7)&=\Bigl(S(6)(S(6)-1)\Bigr)^{64}.
\end{aligned}}$$Điều này hoàn thành giải pháp. ∎ 

## Xác minh 

Sự truy hồi chỉ phụ thuộc vào điều kiện hạt, đòi hỏi sự bất bình đẳng của bảng con LO và HI tại mỗi nút. Điều này phù hợp với định nghĩa về hạt trong Phần 7.1.4. 

Ở mỗi bước sàng lọc, mỗi nút tương ứng với một chức năng con riêng biệt được xác định bằng cách gán tiền tố duy nhất, do đó tính độc lập giữa các$2^{n-1}$các nút được chứng minh bằng cách phân tách bảng chân lý thành các bài toán con rời rạc. 

Việc tính toán của$20736^2$được xác minh là$$(20000+736)^2 = 400000000 + 2\cdot 20000\cdot 736 + 736^2
= 400000000 + 29440000 + 541696
= 429981696.$$Trừ$20736$sản lượng$429960960$, xác nhận sản phẩm đã sửa. 

Tất cả các biểu thức tiếp theo chỉ phụ thuộc vào cơ số đã hiệu chỉnh này. 

Điều này hoàn tất việc xác minh. ∎
