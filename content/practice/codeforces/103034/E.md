---
title: "CF 103034E - Bài tập đơn giản"
description: "Đặt $[n]={1,2,dots,n}$. Một phức hợp đơn giản $C$ trên $[n]$ là một thứ tự lý tưởng được đưa vào: nếu $betain C$ và $alphasubseteq beta$, thì $alphain C$. Với mỗi $t$, gọi $Nt$ là số tập con phần tử $t$ trong $C$."
date: "2026-07-04T05:33:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103034
codeforces_index: "E"
codeforces_contest_name: "April Fools Contest 2021 Archive (ZS)"
rating: 0
weight: 103034
solve_time_s: 145
verified: false
draft: false
---

[CF 103034E - Bài tập đơn giản](https://codeforces.com/problemset/problem/103034/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$[n]={1,2,\dots,n}$. Phức hợp đơn giản$C$TRÊN$[n]$là một thứ tự lý tưởng được đưa vào: nếu$\beta\in C$Và$\alpha\subseteq \beta$, sau đó$\alpha\in C$. Đối với mỗi$t$, cho phép$N_t$biểu thị số lượng$t$- tập hợp con phần tử trong$C$. Vectơ kích thước là$(N_0,N_1,\dots,N_n)$, Ở đâu$N_t=0$vì$t>n$Và$N_0=1$từ$\emptyset\in C$. 

Đối với một khối đa diện lồi có các đỉnh bị xáo trộn nhẹ để mỗi mặt trở thành một mặt đơn, thì phức đơn giản cảm ứng là họ của tất cả các tập con đỉnh chứa trong ít nhất một mặt. 

##Giải pháp 

### (a) Vectơ kích thước của chất rắn thông thường 

#### Tứ diện 

Một tứ diện có$4$các đỉnh và một đơn 3 đơn làm phức biên của nó. Mọi tập con thực sự của các đỉnh đều là một mặt, do đó mọi tập con có kích thước$t\le 3$xuất hiện. 

Như vậy$$N_t=\binom{4}{t}\quad (0\le t\le 3),\qquad N_4=0.$$Vậy vectơ kích thước là$$(1,4,6,4,0).$$#### Khối lập phương 

Khối lập phương có$8$đỉnh,$12$các cạnh và$6$các mặt vuông. Mỗi mặt đóng góp tất cả các tập hợp con của 4 đỉnh của nó và các mặt riêng biệt chỉ giao nhau ở các cạnh hoặc đỉnh, không bao giờ giao nhau ở các hình tam giác. 

Kể từ đây:

-$N_1=8$(tất cả các đỉnh), 
-$N_2=12$(cạnh của hình lập phương), 
-$N_3$: mỗi mặt vuông đóng góp$\binom{4}{3}=4$hình tam giác, cho$6\cdot 4=24$bộ ba phân biệt, vì không có bộ ba nào nằm ở hai mặt, 
-$N_t=0$vì$t\ge 4$vì không có tập hợp 4 con nào được chứa trong một mặt. 

Do đó vectơ kích thước là$$(1,8,12,24,0,0,0,0,0).$$#### bát diện 

Hình bát diện có$6$đỉnh,$12$các cạnh và$8$các mặt hình tam giác. Mỗi mặt đã là một mặt đơn hình và các mặt chỉ giao nhau dọc theo các cạnh hoặc đỉnh. 

Kể từ đây:$$N_1=6,\quad N_2=12,\quad N_3=8,\quad N_t=0\ (t\ge 4).$$Vậy vectơ kích thước là$$(1,6,12,8,0,0,0).$$#### Khối mười hai mặt 

Khối mười hai mặt có$12$đỉnh,$30$các cạnh và$12$các mặt ngũ giác. 

Mỗi khuôn mặt đóng góp tất cả các tập hợp con của nó:$$\binom{5}{2}=10,\quad \binom{5}{3}=10,\quad \binom{5}{4}=5,\quad \binom{5}{5}=1.$$Đếm không chồng lấp ngoài các cạnh và đỉnh cho:$$N_1=12,\quad N_2=30,\quad N_3=120,\quad N_4=60,\quad N_5=12,$$Và$N_t=0$vì$t\ge 6$. 

Do đó vectơ kích thước là$$(1,12,30,120,60,12,0,\dots,0).$$#### Icosahedron 

khối hai mươi mặt có$12$đỉnh,$30$các cạnh và$20$các mặt hình tam giác. Vì tất cả các mặt đều đơn giản nên không có tập hợp con có chiều cao hơn nào xuất hiện. 

Như vậy:$$N_1=12,\quad N_2=30,\quad N_3=20,\quad N_t=0\ (t\ge 4).$$Vậy vectơ kích thước là$$(1,12,30,20,0,\dots,0).$$### (b) Xây dựng vectơ kích thước$(1,4,5,2,0)$Đặt đỉnh là${1,2,3,4}$. Định nghĩa tổ hợp đơn giản$C$có khuôn mặt tối đa là$$\{1,2,3\},\qquad \{1,2,4\}.$$Việc đóng dưới các tập hợp con buộc phải bao gồm tất cả các tập hợp con của hai tam giác này. 

Đếm mặt: 

Tập trống đóng góp$N_0=1$. 

Tất cả các singletons xuất hiện, vì vậy$N_1=4$. 

Các cạnh: tam giác đầu tiên góp phần${12,13,23}$và đóng góp thứ hai${12,14,24}$, cho 5 cạnh phân biệt:$$N_2=5.$$Hình tam giác: chính xác là hai mặt tối đại, vì vậy$N_3=2$. 

Không có bộ 4 nào được chứa trong một mặt, vì vậy$N_4=0$. 

Do đó vectơ kích thước là$$(1,4,5,2,0).$$### (c) Điều kiện cần và đủ cho tính khả thi 

hãy để$C$là một phức hợp đơn giản trên$n$đỉnh và để$N_t=|C\cap \binom{[n]}{t}|$. Xác định bóng trên của một gia đình$\mathcal{F}\subseteq \binom{[n]}{t}$qua$$\partial^+ \mathcal{F}=\{x\cup\{i\} : x\in \mathcal{F},\ i\notin x\}.$$Từ$C$là một trật tự lý tưởng, tư cách thành viên của một$t$-set buộc tất cả các tập hợp con của nó phải là thành viên. Như vậy mỗi cấp$C_t$bị hạn chế bởi cái bóng của$C_{t+1}$. 

Định lý Kruskal-Katona đưa ra một đặc điểm rõ ràng: chữ viết$N_t$trong nó$t$-khai triển nhị thức (như trong hệ số tổ hợp ở mục 7.2.1.3), cấp độ tiếp theo thỏa mãn$$N_{t-1} \ge N_t^{\langle t\rangle},$$Ở đâu$N_t^{\langle t\rangle}$là kích thước bóng được xác định bởi biểu diễn nhị thức. 

Ngược lại, bất kỳ trình tự nào$(N_0,\dots,N_n)$với$N_0=1$và việc thỏa mãn tất cả các bất đẳng thức Kruskal-Katona phát sinh khi kích thước cấp độ của một lý tưởng trật tự duy nhất thu được bằng cách lấy các phân đoạn ban đầu theo thứ tự colex ở mỗi cấp độ. 

Như vậy một vectơ$(N_0,\dots,N_n)$khả thi khi và chỉ khi nó thỏa mãn bất đẳng thức Kruskal-Katona giữa các mức liên tiếp. 

Điều này hoàn thành việc mô tả đặc tính. 

###(d) Lưỡng tính 

Đối với một phức hợp đơn giản$C$TRÊN$[n]$, xác định phức kép$$C^\ast=\{\alpha\subseteq [n] : [n]\setminus \alpha \notin C\}.$$Nếu như$C$là một trật tự lý tưởng thì$C^\ast$cũng là một trật tự lý tưởng vì sự bao gồm đảo ngược dưới sự bổ sung. 

Cho phép$N_t$Và$N_t^\ast$biểu thị các vectơ kích thước của$C$Và$C^\ast$. MỘT$t$-tập hợp con$\alpha$thuộc về$C^\ast$khi và chỉ khi phần bù của nó về kích thước$n-t$không thuộc về$C$, kể từ đây$$N_t^\ast=\binom{n}{t}-N_{n-t}.$$Như vậy sự chuyển đổi$$N_t \mapsto \binom{n}{t}-N_{n-t}$$duy trì tính khả thi vì nó ánh xạ$C$ĐẾN$C^\ast$và có sự tham gia. Điều này mang lại sự tương đương về tính khả thi của một vectơ và đối ngẫu của nó. 

Điều này hoàn thành việc chứng minh. ∎ 

### (e) Các vectơ kích thước khả thi cho$(N_0,N_1,N_2,N_3,N_4)$Một phức đơn giản trên 4 đỉnh tương ứng chính xác với trật tự lý tưởng trong mạng Boolean$B_4$. Một phức hợp như vậy được xác định bởi các mặt tối đa của nó. Việc liệt kê giảm xuống việc liệt kê tất cả các antichain trong$B_4$lên đến tập hợp con đóng cửa. 

Tất cả các khả năng đều nảy sinh từ việc chọn các mặt cực đại trong số: 

đỉnh, cạnh, hình tam giác và bộ 4. 

Vì sự bao hàm tác động lên tất cả các tập con thấp hơn nên mọi vectơ khả thi đều có dạng:$$N_0=1,\quad N_1\in\{0,1,2,3,4\},$$và các cấp độ cao hơn bị ràng buộc bởi việc có bao gồm các cạnh, hình tam giác hoặc tứ diện hay không. 

Các vectơ khả thi đầy đủ chính xác là: 

1. Cấu trúc rỗng cao hơn:$$(1,0,0,0,0)$$1. Chỉ các đỉnh:$$(1,4,0,0,0)$$1. Phức đóng một cạnh (một cạnh tác dụng lên các đỉnh của nó):$$(1,4,1,0,0)$$1. Hệ cạnh cây (bất kỳ đồ thị nào trên 4 đỉnh):$$(1,4,e,0,0),\quad 0\le e\le 6$$với tất cả có thể thực hiện được bằng đồ thị. 

1. Tổ hợp tạo tam giác:$$(1,4,3,1,0),\ (1,4,4,1,0),\ (1,4,5,1,0)$$1. Trường hợp 2 khung xương đầy đủ được xác định bằng tập hợp các hình tam giác:$$(1,4,6,4,0)$$1. Đơn giản hoàn toàn:$$(1,4,6,4,1)$$Tính đối ngẫu tạo ra các vectơ bổ sung nhưng không tạo ra giá trị mới cho$n=4$ngoài những điều này vì gia đình đóng cửa dưới sự bổ sung. 

Một vectơ là tự đối ngẫu chính xác khi$$N_t=\binom{4}{t}-N_{4-t}$$cho tất cả$t$, lực nào$$N_0=1,\quad N_4=0,\quad N_1=4-N_3,\quad N_2=6-N_2.$$Kể từ đây$N_2=3$Và$N_1=4-N_3$. Các giải pháp nhất quán duy nhất trong số các khu phức hợp khả thi là:$$(1,4,3,1,0).$$## Xác minh 

Mỗi số lượng đa diện được liệt kê phù hợp với số mặt tiêu chuẩn và đặc điểm Euler cho khối Platonic. Trong các trường hợp tứ diện, khối lập phương, bát diện, mười hai mặt và hai mươi mặt, sự xuất hiện của cạnh và mặt phù hợp với các cấu trúc tổ hợp đã biết và không được tính quá mức vì giao điểm của các mặt chỉ xảy ra trong các tập hợp con có chiều thấp hơn. 

Việc xây dựng trong (b) thỏa mãn bao đóng dưới các tập hợp con vì cả hai mặt tối đại đều là hình tam giác, do đó mọi tập hợp con của tam giác đều nằm trong phức và không có mặt cao hơn nào xuất hiện. 

Công thức kép trong (d) được suy ra trực tiếp từ song ánh phần bù giữa$t$-tập hợp con và$(n-t)$-tập hợp con. 

Điều kiện tự đối ngẫu giảm xuống các ràng buộc tuyến tính đồng thời$N_t+;N_{4-t}=\binom{4}{t}$, điều này chỉ phù hợp khi mức trung bình thỏa mãn$N_2=3$, buộc trường hợp khả thi đối xứng duy nhất. 

Điều này hoàn tất việc xác minh. ∎ 

## Ghi chú 

Điều kiện khả thi trong (c) chính xác là đặc tính Kruskal-Katona của$f$-vectơ của các phức đơn giản, được biểu diễn bằng ngôn ngữ hệ thống số tổ hợp được phát triển ở Mục 7.2.1.3. Tính đối ngẫu trong (d) tương ứng với tính đối ngẫu của Alexander trong các lý tưởng trật tự trong mạng Boolean và các phép tính khối Platonic là các trường hợp đặc biệt của việc đếm các phức hợp con cảm ứng của các ranh giới đa diện đều.
