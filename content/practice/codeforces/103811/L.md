---
title: "CF 103811L - Khóa"
description: "Đặt đóng góp của một tiểu hạng tương ứng với một phép gán $x1 ldots xn$ là $$C(x1,ldots,xn)=prod{i=1}^n (1-pi)^{1-xi}pi^{xi}."
date: "2026-07-02T08:30:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103811
codeforces_index: "L"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2021"
rating: 0
weight: 103811
solve_time_s: 125
verified: false
draft: false
---

[CF 103811L - Khóa](https://codeforces.com/problemset/problem/103811/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Đặt sự đóng góp của một minterm tương ứng với một bài tập$x_1 \ldots x_n$là$$C(x_1,\ldots,x_n)=\prod_{i=1}^n (1-p_i)^{1-x_i}p_i^{x_i}.$$Lấy logarit sẽ chuyển đổi cực đại hóa tích thành cực đại hóa tổng:$$\log C(x_1,\ldots,x_n)=\sum_{i=1}^n \bigl((1-x_i)\log(1-p_i)+x_i\log p_i\bigr).$$Đối với mỗi chỉ số$i$, xác định hai trọng số cục bộ$$w_i(0)=\log(1-p_i), \qquad w_i(1)=\log p_i.$$Khi đó mục tiêu sẽ trở thành$\sum_{i=1}^n w_i(x_i)$và tối đa hóa đóng góp của minterm tương đương với việc tối đa hóa tổng này tùy thuộc vào$f(x_1,\ldots,x_n)=1$. 

Một root-to-$\top$đường dẫn trong BDD sửa một số biến và bỏ qua các biến khác theo thuộc tính đặt hàng. Nếu một nút có chỉ số thay đổi$j$và người kế nhiệm của nó có chỉ mục$k>j+1$, sau đó biến$j+1,\ldots,k-1$không xuất hiện trên con đường đó. Đối với mỗi biến bị bỏ qua như vậy$i$, giá trị của nó có thể được chọn độc lập, do đó đóng góp tối ưu của nó là$$m_i=\max(\log(1-p_i),\log p_i).$$Do đó, mọi chuyển đổi trong BDD đều đóng góp một khoảng cách cố định được xác định duy nhất bởi các chỉ số của điểm cuối, cùng với sự đóng góp của nhánh được chọn. 

Xác định giá trị lập trình động$H(v)$cho mỗi nút$v$, được hiểu là mức đóng góp nhật ký tối đa có thể đạt được từ$v$ĐẾN$\top$. 

Đối với một nút chìm,$$H(\top)=0, \qquad H(\bot)=-\infty.$$Đối với một nút nhánh$v$được gắn nhãn theo biến$x_j$, hãy để những người kế vị thấp và cao của nó$\mathrm{LO}(v)$Và$\mathrm{HI}(v)$. Đối với bất kỳ người kế nhiệm$u$của$v$, xác định sự đóng góp của khoảng cách$$G(j,u)=\sum_{i=j+1}^{V(u)-1} m_i,$$Ở đâu$V(u)$là chỉ số thay đổi của nút$u$và đối với bồn rửa, chúng tôi lấy$V(\top)=n+1$. 

Sau đó sự tái diễn là$$H(v)=\max\Bigl(w_j(0)+G(j,\mathrm{LO}(v))+H(\mathrm{LO}(v)),\; w_j(1)+G(j,\mathrm{HI}(v))+H(\mathrm{HI}(v))\Bigr).$$Gốc có chỉ số$j_0=V(\mathrm{ROOT})$, và khoảng cách ban đầu của nó góp phần$\sum_{i=1}^{j_0-1} m_i$. Giá trị tối ưu thu được bằng cách thêm khoảng cách ban đầu này vào$H(\mathrm{ROOT})$và phép gán tối ưu được phục hồi bằng cách theo dõi tại mỗi nút nhánh đạt được mức tối đa trong lần lặp lại. 

Tính chính xác xuất phát từ việc phân tách bất kỳ phép gán hoàn chỉnh nào thành ba phần độc lập: quyết định cố định ở biến hiện tại, các lựa chọn tối ưu độc lập cho các biến bị bỏ qua và phần tiếp tục tối ưu trong BDD phụ. Mỗi phép gán hoàn chỉnh tương ứng với chính xác một root-to-$\top$con đường cùng với những lựa chọn độc lập về những khoảng trống, và sự tái diễn sẽ làm cạn kiệt tất cả những khả năng đó mà không bỏ sót hoặc trùng lặp. Tính không tuần hoàn của thứ tự BDD đảm bảo rằng$H(v)$được xác định rõ bằng quy nạp ngược khi giảm các chỉ số biến đổi, vì tất cả các biến kế tiếp đều có chỉ số lớn hơn. 

Bài toán được xây dựng thỏa mãn$f(x_1,\ldots,x_n)=1$bởi vì chỉ$\top$-các đường đi có thể tiếp cận được xem xét và đóng góp tối thiểu của nó là tối đa vì mọi quyết định cục bộ và mọi lựa chọn biến bị bỏ qua đều tối ưu riêng lẻ theo phân tích cộng của$\log C$. 

Điều này hoàn thành giải pháp. ∎
