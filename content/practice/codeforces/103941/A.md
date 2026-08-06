---
title: "CF 103941A - Mocha \u4e0a\u5c0f\u73ed\u5566"
description: "Hàm Boolean rất thú vị khi mọi bảng con phát sinh từ bất kỳ phép gán tiền tố nào đều là một chuỗi hạt. Một bảng chân lý chính xác là một hạt khi nó không có dạng $alphaalpha$, vì vậy mỗi hàm con phải có các bảng con LO và HI riêng biệt tại mỗi nút trong cấu trúc quyết định có thứ tự của nó."
date: "2026-07-02T06:56:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103941
codeforces_index: "A"
codeforces_contest_name: "2022 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 103941
solve_time_s: 123
verified: false
draft: false
---

[CF 103941A - Mocha \u4e0a\u5c0f\u73ed\u5566](https://codeforces.com/problemset/problem/103941/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hàm Boolean rất thú vị khi mọi bảng con phát sinh từ bất kỳ phép gán tiền tố nào đều là một chuỗi hạt. Một bảng chân lý chính xác là một hạt khi nó không có dạng$\alpha\alpha$, do đó, mỗi hàm con phải có các bảng con LO và HI riêng biệt tại mỗi nút trong cấu trúc quyết định có thứ tự của nó. 

Cho phép$f$Và$g$là hàm Boolean ngọt ngào của$n$các biến và xác định$$h(x_1,\dots,x_n)=f(x_1,\dots,x_n)\wedge g(x_1,\dots,x_n).$$Đối với bất kỳ phép gán tiền tố cố định nào$(x_1=c_1,\dots,x_k=c_k)$, cho phép$f_c$,$g_c$, Và$h_c$biểu thị các bảng con cảm ứng theo thứ tự$n-k$. Sau đó$$h_c = f_c \wedge g_c$$được tính theo từng điểm trên bảng chân lý. 

Một bảng phụ không thể trở thành một hạt chính xác khi nó có dạng$\beta\beta$, nghĩa là hai nửa của nó (tương ứng với$x_{k+1}=0$Và$x_{k+1}=1$) đều bằng nhau. Như vậy$h$không chính xác khi tồn tại một phép gán tiền tố sao cho hàm con cảm ứng thỏa mãn$$h_c(0,x_{k+2},\dots,x_n)=h_c(1,x_{k+2},\dots,x_n).$$sử dụng$h_c=f_c\wedge g_c$, sự bằng nhau của hai nửa có nghĩa là với mỗi lần tiếp tục,$$f_c(0,\dots)\wedge g_c(0,\dots) = f_c(1,\dots)\wedge g_c(1,\dots).$$Từ$f$Và$g$ngọt ngào, mỗi cái thỏa mãn rằng với mỗi tiền tố, các bảng phụ LO và HI của nó là khác biệt. Vì vậy, đối với mỗi tiền tố như vậy, ít nhất một trong các$f_c(0,\dots)\ne f_c(1,\dots)$Và$g_c(0,\dots)\ne g_c(1,\dots)$nắm giữ. 

Hãy xem xét một tiền tố trong đó cả hai$f_c$Và$g_c$khác nhau theo cách mà sự kết hợp của chúng trở nên không đổi trong suốt quá trình phân chia. Điều này xảy ra chính xác khi một hàm có giá trị$0$trên tất cả các đầu vào trong đó đầu vào kia có giá trị khác nhau, khiến AND thu gọn cả hai nhánh để có kết quả giống hệt nhau. 

Một trở ngại cụ thể phát sinh khi tồn tại một sự gán tiền tố sao cho một trong$f_c$hoặc$g_c$giống hệt nhau$0$trên cả hai nhánh trong khi nhánh kia là tùy ý. Độ ngọt không loại trừ khả năng này, vì một hàm có thể có hàm con bằng hằng số$0$tại một nút nào đó trong khi vẫn có bảng con LO và HI riêng biệt ở nút đó ở các cấp độ cao hơn. 

Lấy tiền tố ở đâu$f_c(0,\dots)=f_c(1,\dots)=0$. Điều này không vi phạm vị ngọt của$f$bởi vì điều kiện liên quan liên quan đến sự bằng nhau của hai bảng phụ, chứ không phải tính không đổi của chúng đối với tất cả các biến bên dưới. Điều tương tự cũng áp dụng cho$g$. Tại một nút như vậy,$$h_c(0,\dots)=h_c(1,\dots)=0$$Vì thế$h_c$không phải là một hạt. 

Vẫn còn phải chứng minh rằng cấu hình như vậy có thể xảy ra ngay cả khi cả hai$f$Và$g$thật ngọt ngào. xây dựng$f$sao cho tại một số nút, các hàm con LO và HI của nó khác nhau nhưng cả hai đều có giá trị là$0$ở mức hạn chế tiếp theo và xây dựng tương tự$g$sao cho sự biến thiên của nó xảy ra trên sự hỗ trợ rời rạc. Sau đó tại nút kết hợp AND buộc cả hai nhánh giống hệt nhau$0$các giá trị. 

Những cấu hình như vậy đã tồn tại cho$n=2$. Cho phép$$f(x_1,x_2)=x_1,\quad g(x_1,x_2)=\overline{x_1}.$$Cả hai đều tuyệt vời vì các hàm con không tầm thường duy nhất của chúng có các giá trị LO và HI riêng biệt. Tuy nhiên,$$h(x_1,x_2)=f\wedge g = x_1\wedge \overline{x_1}=0,$$và hàm số 0 có bảng chân trị$00$, không phải là một hạt. Kể từ đây$h$không ngọt ngào. 

Do đó, lớp hàm ngọt không bị đóng dưới sự kết hợp.$$\boxed{\text{false}}$$Điều này hoàn thành giải pháp. ∎
