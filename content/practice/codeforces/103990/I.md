---
title: "CF 103990I - Thư mời"
description: "Cho $G = (V,E)$ và để $g$ biểu thị họ các cạnh được mã hóa theo nghĩa của Bài tập 236(e), sao cho $g = bigcup{u-v trong E}(eu sqcup ev)$ và họ các tập hợp độc lập được biểu diễn bằng một công thức trong đại số họ mở rộng như trong bài tập đó."
date: "2026-07-02T06:07:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103990
codeforces_index: "I"
codeforces_contest_name: "2022 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103990
solve_time_s: 40
verified: false
draft: false
---

[CF 103990I - Lời mời](https://codeforces.com/problemset/problem/103990/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$G = (V,E)$và để$g$biểu thị họ các cạnh được mã hóa theo nghĩa của Bài tập 236(e), sao cho$g = \bigcup_{u-v \in E}(e_u \sqcup e_v)$và họ các tập hợp độc lập được biểu diễn bằng một công thức trong đại số họ mở rộng như trong bài tập đó. 

Một nhóm ở$G$là một tập hợp các đỉnh$C \subseteq V$sao cho mọi cặp đỉnh phân biệt trong$C$được kết nối bởi một cạnh trong$G$. Cho phép$G^c$biểu thị đồ thị bù trên cùng một tập đỉnh, có họ cạnh$g^c$bao gồm tất cả các cặp không có thứ tự$u-v$không ở trong$E$. Sau đó một bộ$C$là một nhóm trong$G$khi và chỉ khi nó là tập độc lập trong$G^c$. Điều này chuyển đổi bảng liệt kê nhóm trong$G$vào tập liệt kê độc lập trong$G^c$. 

Cho phép$f_{\mathrm{ind}}(g)$biểu thị họ các tập hợp độc lập của đồ thị có họ cạnh$g$, như được trình bày trong Bài tập 236(e). Sau đó gia đình bè phái của$G$là$$f_{\mathrm{clique}}(G) = f_{\mathrm{ind}}(g^c).$$Nhóm tối đa của$G$do đó là các phần tử tối đa của họ này, vì vậy$$f_{\max\text{-clique}}(G) = \bigl(f_{\mathrm{ind}}(g^c)\bigr)^\uparrow.$$Biểu thức này đã có trong ngôn ngữ của đại số họ và có thể được thực hiện trực tiếp bằng các phép toán ZDD một lần.$g^c$có sẵn. Họ cạnh bổ sung có được bằng cách$$g^c = \binom{V}{2} \setminus g,$$vì vậy trong đại số họ mở rộng, nó được xây dựng bằng phép trừ họ phổ quát ở cấp độ tập hợp con 2 phần tử. 

Một tập đỉnh$U \subseteq V$có thể được bao phủ bởi$k$bè phái khi và chỉ nếu có tồn tại bè phái$C_1, \dots, C_k$TRONG$G$như vậy$$U \subseteq C_1 \cup \cdots \cup C_k.$$Tương đương, mỗi$C_i$là một tập độc lập trong$G^c$, Vì thế$U$có thể được bao phủ bởi$k$bè phái trong$G$nếu và chỉ nếu$U$có thể được bao phủ bởi$k$tập hợp độc lập trong$G^c$. Điều này tương đương với việc nói rằng$U$thừa nhận một màu thích hợp của đồ thị con cảm ứng$(G^c \mid U)$với nhiều nhất$k$màu sắc, trong đó mỗi lớp màu là một tập hợp độc lập trong$G^c$. 

Cho phép$F_k$biểu thị họ các tập đỉnh có thể được bao phủ bởi$k$bè phái trong$G$. Sau đó$$F_k = \{ U \subseteq V \mid U \text{ is $k$-colorable in } G^c \}.$$Các tập tối đa được bao phủ bởi$k$sau đó là bè phái$$F_k^\uparrow.$$Công thức này làm giảm vấn đề khi áp dụng lặp đi lặp lại việc xây dựng tập hợp độc lập trong đại số họ trên$G^c$, kết hợp với một$k$-xây dựng sản phẩm gấp tương ứng với sự kết hợp rời rạc của$k$những gia đình độc lập. Cụ thể, nếu$f = f_{\mathrm{ind}}(g^c)$là họ tập độc lập của đồ thị phần bù thì họ các tập hợp có thể bao phủ bởi$k$bè phái có được bởi$k$-fold liên kết đóng cửa$$F_k = \underbrace{f \sqcup f \sqcup \cdots \sqcup f}_{k\ \text{times}},$$Ở đâu$\sqcup$biểu thị sự kết hợp rời rạc của các họ được sử dụng trong đại số ZDD của Bài tập 236. 

Tối đa$k$-các tập hợp có thể che phủ được bằng cách áp dụng toán tử cực đại,$$F_k^\uparrow = \bigl(\underbrace{f \sqcup \cdots \sqcup f}_{k\ \text{times}}\bigr)^\uparrow.$$Đối với trường hợp cụ thể khi$G$là đồ thị liền kề của Hoa Kỳ (18), quá trình tính toán được tiến hành bằng cách xây dựng ZDD cho$f_{\mathrm{ind}}(g^c)$sử dụng họ cạnh của đồ thị phần bù, sau đó áp dụng$\uparrow$hoạt động để trích xuất các phần tử tối đa và cuối cùng lặp lại việc xây dựng liên minh ZDD$k$lần để tăng$k$. Các họ kết quả, bao gồm cả số lượng và các phần tử cực trị của chúng, phụ thuộc vào cấu trúc kề rõ ràng của đồ thị (18). Nếu không có danh sách cạnh của (18) trong ngữ cảnh đã cho, bảng liệt kê cuối cùng của cụm cực đại và cực đại$k$-Các tập đỉnh có thể che phủ được theo cụm không thể được khởi tạo. 

Điều này hoàn thành việc rút ra phép rút gọn đại số họ thành các phép toán ZDD và mô tả đặc điểm cấu trúc của các họ được yêu cầu. ∎
