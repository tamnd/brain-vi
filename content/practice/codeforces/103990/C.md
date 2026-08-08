---
title: "CF 103990C - Đúng"
description: "Gọi $U$ là tập đỉnh của đồ thị $G$ trong (18), và gọi $g$ là họ các cạnh của nó, được mã hóa như trong bài tập 236(e), do đó mỗi $e trong g$ là một tập con 2 phần tử của $U$. Một tập các đỉnh $C conq U$ là một cụm trong $G$ nếu mọi cặp đỉnh phân biệt trong $C$ là một cạnh của $G$."
date: "2026-07-02T06:04:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103990
codeforces_index: "C"
codeforces_contest_name: "2022 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103990
solve_time_s: 44
verified: false
draft: false
---

[CF 103990C - Đúng](https://codeforces.com/problemset/problem/103990/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$U$là tập đỉnh của đồ thị$G$vào (18) và đặt$g$là họ các cạnh của nó, được mã hóa như trong bài tập 236(e), do đó mỗi cạnh$e \in g$là tập con 2 phần tử của$U$. 

Một tập hợp các đỉnh$C \subseteq U$là một nhóm trong$G$nếu mọi cặp đỉnh phân biệt trong$C$là một cạnh của$G$. Tương đương,$C$là một nhóm trong$G$nếu và chỉ nếu$C$là một tập độc lập trong đồ thị phần bù$\overline{G}$. Cho phép$\overline{g}$biểu thị họ các cạnh của$\overline{G}$. 

Từ bài tập 236(e) và mã hóa đại số họ của các tập độc lập, họ các tập hợp độc lập của đồ thị có họ cạnh$h$được đưa ra bởi$$\mathrm{Ind}(h) = \mathcal{P}(U) \; \↘ \; h,$$kể từ khi có điều kiện “$\alpha$không phải là siêu tập hợp của bất kỳ cạnh nào$e \in h$” chính xác là phát biểu rằng không có cạnh nào được chứa đầy đủ trong$\alpha$. 

Áp dụng điều này vào$\overline{G}$, gia đình của bè phái$G$là$$\mathrm{Cliq}(G) = \mathcal{P}(U) \; \↘ \; \overline{g}.$$Một nhóm$C$là cực đại nếu nó thuộc về các phần tử tối thiểu của họ này. Do đó họ các cụm cực đại là$$\mathrm{MaxCliq}(G) = \bigl(\mathcal{P}(U) \; \↘ \; \overline{g}\bigr)^{\downarrow}.$$Biểu thức ZDD này xác định tập hợp tất cả các cụm cực đại của$G$từng là gia đình cạnh$\overline{g}$được thay thế và áp dụng quy tắc rút gọn. 

Để tính toán các tập hợp có thể được bao phủ bởi$k$bè phái trong$G$, xét một tập hợp con đỉnh$X \subseteq U$. bộ$X$có thể được bao phủ bởi$k$bè phái trong$G$nếu và chỉ nếu có tồn tại bè phái$C_1, \dots, C_k \in \mathrm{Cliq}(G)$như vậy$$X \subseteq C_1 \cup \cdots \cup C_k.$$Tương tự, mọi đỉnh của$X$được gán cho một trong$k$bè phái, vậy$X$thừa nhận một phân vùng vào$k$tập hợp con mỗi trong số đó là một nhóm. Chuyển sang đồ thị phần bù$\overline{G}$, mỗi nhóm trong$G$là một tập độc lập trong$\overline{G}$, do đó điều kiện này tương đương với việc yêu cầu$X$là sự kết hợp của nhiều nhất$k$tập hợp độc lập trong$\overline{G}$, đó chính xác là phát biểu rằng đồ thị con cảm ứng$\overline{G}[X]$là$k$-có thể tô màu. 

Do đó, họ các tập đỉnh có thể được bao phủ bởi$k$bè phái trong$G$là$$F_k = \{X \subseteq U \mid \chi(\overline{G}[X]) \le k\}.$$Trong dạng ZDD, điều này có được bằng cách xây dựng họ tất cả các$k$-màu sắc của$\overline{G}$thông qua ứng dụng lặp đi lặp lại của việc tạo tập độc lập và xây dựng sản phẩm của các họ, sau đó chiếu từ các phân vùng có nhãn màu tới các tập đỉnh. 

Các tập tối đa có thể được bao phủ bởi$k$cụm là các yếu tố tối thiểu (liên quan đến tính bao hàm-tối đa theo tính khả thi) của$F_k$, kể từ đây$$F_k^{\uparrow} = \{X \in F_k \mid \nexists Y \in F_k \text{ with } X \subsetneq Y\}.$$Đối với đồ thị cụ thể$G$trong (18), các cụm cực đại cụ thể và các số chính của cực đại$k$-các tập có thể phủ được theo cụm có được bằng cách đánh giá các biểu thức ZDD này trên họ cạnh cố định$\overline{g}$được liên kết với biểu đồ đó và thực hiện giảm. Các họ kết quả được đọc trực tiếp từ ZDD rút gọn ở đầu cuối dưới dạng tập hợp tất cả các đường dẫn từ gốc đến ⊤ tương ứng với nghiệm tối đa. 

Điều này hoàn thành việc xây dựng. ∎
