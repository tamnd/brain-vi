---
title: "CF 104031C - \u0420\u043e\u043b\u043b\u0435\u0440"
description: "Đặt $A={i1,i2,ldots,iell}$ và đặt $F = e{i1}cupcdotscup e{iell}$. ZDD cho $F$ bao gồm một chuỗi quyết định duy nhất được sắp xếp theo chỉ số, bởi vì mỗi $e{it}$ đóng góp một nút kiểm tra tư cách thành viên của $it$ và giảm xuống $perp$ hoặc $top$ như trong các quy ước của Bài tập 7.1.4."
date: "2026-07-02T04:02:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104031
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0421\u0430\u043c\u0430\u0440\u0435 2021-2022 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104031
solve_time_s: 116
verified: false
draft: false
---

[CF 104031C - \u0420\u043e\u043b\u043b\u0435\u0440](https://codeforces.com/problemset/problem/104031/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$A={i_1,i_2,\ldots,i_\ell}$và để$F = e_{i_1}\cup\cdots\cup e_{i_\ell}$. ZDD dành cho$F$bao gồm một chuỗi quyết định duy nhất được sắp xếp theo chỉ số, bởi vì mỗi$e_{i_t}$đóng góp một nút kiểm tra tư cách thành viên của$i_t$và giảm xuống$\perp$hoặc$\top$như trong các quy ước của Bài tập 7.1.4.203. hoạt động$F § k$được định nghĩa là hàm Boolean đối xứng$S_k(x_{i_1},\ldots,x_{i_\ell})$, đánh giá là$1$chính xác khi nào chính xác$k$của các biến trong$A$là$1$. 

### Giải thích cấu trúc 

Mỗi bộ$\alpha \subseteq A$tương ứng với việc gán các biến$x_{i_t}$, Ở đâu$i_t \in \alpha$có nghĩa$x_{i_t}=1$. giá trị$S_k(x_{i_1},\ldots,x_{i_\ell})$chỉ phụ thuộc vào bản số$|\alpha|$. Vì thế$F § k$là chức năng đặc trưng của gia đình$$\{\alpha \subseteq A \mid |\alpha|=k\}.$$Gia đình này thống nhất hơn$A$, do đó cách biểu diễn ZDD của nó chỉ phụ thuộc vào số phần tử còn lại cần chọn và các vị trí còn lại trong chuỗi chỉ mục có thứ tự. 

### Phân rã ZDD đệ quy 

hãy để$F^{(t)} = e_{i_1}\cup\cdots\cup e_{i_t}$. Sửa chữa$t>0$và để$j=i_t$. Mỗi tập hợp con$\alpha \subseteq {i_1,\ldots,i_t}$chia duy nhất thành hai lớp riêng biệt tùy theo việc$j \in \alpha$hoặc$j \notin \alpha$. Điều này gây ra sự phân rã rời rạc của các gia đình:$$\{\alpha \subseteq F^{(t)} \mid |\alpha|=k\}
=
\{\alpha \subseteq F^{(t-1)} \mid |\alpha|=k\}
\;\cup\;
\{\alpha \cup \{j\} \mid \alpha \subseteq F^{(t-1)},\ |\alpha|=k-1\}.$$Chuyển điều này sang các hoạt động ZDD, thuật ngữ thứ hai tương ứng với việc tham gia$e_j$với tất cả các bộ kích thước$k-1$, trong khi số hạng đầu tiên tương ứng với việc loại trừ$j$:$$F^{(t)} § k
=
(F^{(t-1)} § k)
\;\cup\;
(e_j \sqcup (F^{(t-1)} § (k-1))).$$### Điều kiện cơ bản 

Khi nào$t=0$, họ trống nên chỉ tồn tại tập trống. Vì thế$$F^{(0)} § 0 = \epsilon,
\qquad
F^{(0)} § k = \perp \ \text{for } k>0.$$### Tính đúng đắn 

Mỗi$\alpha$với$|\alpha|=k$TRONG$F^{(t)}$hoặc chứa$j$hoặc không chứa$j$. Nếu như$j \notin \alpha$, sau đó$\alpha \subseteq F^{(t-1)}$Và$|\alpha|=k$, Vì thế$\alpha$được đại diện trong$F^{(t-1)} § k$. Nếu như$j \in \alpha$, sau đó$\alpha \setminus {j} \subseteq F^{(t-1)}$Và$|\alpha \setminus {j}|=k-1$, Vì thế$\alpha$được tạo ra duy nhất bằng cách áp dụng$e_j \sqcup$tới một phần tử của$F^{(t-1)} § (k-1)$. Hai trường hợp này khác nhau vì$j$không thể vừa được bao gồm vừa bị loại trừ trong cùng một tập hợp, vì vậy phép hợp là chính xác. 

### Triển khai dưới dạng hoạt động ZDD 

Việc tính toán tiến hành bằng cách xây dựng động trên các trạng thái$(t,k)$giảm dần$t$và cố định$k$. Mỗi trạng thái tạo ra một nút ZDD được xác định bởi$$G(t,k) = G(t-1,k)\ \cup\ (e_{i_t} \sqcup G(t-1,k-1)),$$với điều kiện biên$G(0,0)=\epsilon$Và$G(0,k)=\perp$vì$k>0$. Đầu ra cần thiết là$G(\ell,k)$. 

Bởi vì việc giảm ZDD xác định các đồ thị con giống hệt nhau và loại bỏ các nút dư thừa, nên sự xuất hiện lặp lại của các cặp giống hệt nhau$(t,k)$cấu trúc chia sẻ, do đó mỗi cặp riêng biệt đóng góp tối đa một nút ZDD trong biểu diễn rút gọn. 

### Độ phức tạp 

Mỗi tiểu bang$(t,k)$yêu cầu một ứng dụng$\cup$và một ứng dụng của$\sqcup$, cả hai hoạt động ZDD thời gian không đổi theo thuật toán áp dụng tiêu chuẩn với khả năng ghi nhớ. Số lượng các trạng thái riêng biệt được giới hạn bởi$\ell(k+1)$, từ$t$dao động từ$0$ĐẾN$\ell$Và$k$dao động từ$0$đến giá trị mục tiêu. 

Mỗi trạng thái được xử lý một lần và mỗi bước xử lý sẽ đưa ra một số lượng thao tác ZDD không đổi. Do đó, tổng số phép toán ZDD nguyên thủy tỷ lệ thuận với số trạng thái có thể truy cập được trong biểu đồ đệ quy. 

Đệ quy chỉ tạo ra các trạng thái có$0 \le k \le t \le \ell$, vì vậy số lượng cặp có thể truy cập nhiều nhất là$\ell(\ell+1)/2$. Mỗi cặp như vậy được đánh giá một lần và mỗi đánh giá thực hiện một số giới hạn các hoạt động nguyên thủy ZDD, do đó việc xây dựng sẽ chạy theo thời gian tỷ lệ thuận với số lượng có thể truy cập được.$(t,k)$tiểu bang. 

### Kết luận 

hoạt động$(e_{i_1}\cup\cdots\cup e_{i_\ell}) § k$được triển khai bằng đệ quy hai nhánh bao gồm hoặc loại trừ chỉ mục cuối cùng ở mỗi bước, kết hợp các kết quả thông qua$\cup$Và$\sqcup$. ZDD thu được đại diện chính xác cho họ của tất cả$k$- tập hợp con phần tử của${i_1,\ldots,i_\ell}$và được xây dựng bởi một số phép toán ZDD bị chặn trên mỗi trạng thái đệ quy có thể truy cập. Điều này hoàn thành việc chứng minh. ∎
