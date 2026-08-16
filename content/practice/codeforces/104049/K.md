---
title: "CF 104049K - Nhà giả kim Fullmetal II"
description: "Chúng ta biểu diễn họ $f$ dưới dạng sơ đồ quyết định có thứ tự rút gọn trên các biến $x1,x2,dots,xn$, sử dụng các quy ước của Phần 7.1.4 và Bài tập 203. Một nút $v$ có các trường $$V(v),quad LO(v),quad HI(v),$$ và các thiết bị đầu cuối $bot,top$."
date: "2026-07-02T03:45:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104049
codeforces_index: "K"
codeforces_contest_name: "UTPC Contest 11-11-22 Div. 1 (Advanced)"
rating: 0
weight: 104049
solve_time_s: 130
verified: false
draft: false
---

[CF 104049K - Nhà giả kim Fullmetal II](https://codeforces.com/problemset/problem/104049/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Chúng tôi đại diện cho một gia đình$f$như một sơ đồ quyết định có thứ tự rút gọn đối với các biến$x_1,x_2,\dots,x_n$, sử dụng các quy ước của Phần 7.1.4 và Bài tập 203. Một nút$v$có trường$$V(v),\quad LO(v),\quad HI(v),$$và thiết bị đầu cuối$\bot,\top$. Thứ tự biến đổi đang tăng dần dọc theo mọi cạnh. 

Tất cả các hoạt động bên dưới được thực hiện bằng đệ quy cấu trúc trên các cặp nút, với việc ghi nhớ các cặp được tính toán trước đó. 

Cho phép$\mathrm{Apply}(op,f,g)$biểu thị một thủ tục đệ quy được ghi nhớ trả về kết quả của việc áp dụng$op$đến các nút$f,g$. Cho phép$\mathrm{top}(v)$biểu thị$V(v)$, và để$\mathrm{low}(v),\mathrm{high}(v)$biểu thị con cái của nó. 

Khi$f$Và$g$là nonterminal, hãy để$i=\mathrm{top}(f)$,$j=\mathrm{top}(g)$. Cho phép$k=\min(i,j)$. Chúng tôi chia theo khai triển Shannon trên biến$x_k$. 

Một nút có chỉ số thay đổi$k$nhưng việc thiếu một toán hạng sẽ được xử lý bằng cách sao chép toán hạng đó trên cả hai nhánh. 

Tất cả các kết quả đều được rút gọn theo quy tắc thông thường: các đồ thị con giống hệt nhau được chia sẻ và các nút có con cao và thấp bằng nhau sẽ bị loại bỏ. 

### (a) Tham gia$f \sqcup g$Sự tham gia được xác định bởi$$f \sqcup g = \{\alpha \cup \beta \mid \alpha \in f,\ \beta \in g\}.$$Đệ quy: 

Nếu$f=\bot$hoặc$g=\bot$, sau đó$f \sqcup g=\bot$. 

Nếu như$f=\top$, sau đó$f \sqcup g=g$. Nếu như$g=\top$, sau đó$f \sqcup g=f$. 

Nếu không hãy để$k=\min(V(f),V(g))$. Xác định các phép chiếu$$f_0 = f|_{x_k=0},\quad f_1 = f|_{x_k=1},\quad g_0 = g|_{x_k=0},\quad g_1 = g|_{x_k=1}.$$Sau đó$$(f \sqcup g)_0 = (f_0 \sqcup g_0),
\qquad
(f \sqcup g)_1 = (f_1 \sqcup g_0)\ \sqcup\ (f_0 \sqcup g_1)\ \sqcup\ (f_1 \sqcup g_1).$$Nút gốc được tạo ở cấp độ$k$với những đứa trẻ này, sau đó là giảm bớt. 

Sự tái diễn này phản ánh rằng một tập hợp trong$f \sqcup g$hoặc bỏ qua$x_k$trong cả hai thành phần hoặc bao gồm nó từ ít nhất một phía, tạo ra tất cả các hợp của các trường hợp con. 

### (b) Gặp gỡ$f \sqcap g$Cuộc gặp gỡ là$$f \sqcap g = \{\alpha \cap \beta \mid \alpha \in f,\ \beta \in g\}.$$Các trường hợp cơ bản:$$\bot \sqcap g = \bot,\quad f \sqcap \bot = \bot,\quad \top \sqcap g = g,\quad f \sqcap \top = f.$$Đệ quy với$k=\min(V(f),V(g))$:$$(f \sqcap g)_0 = f_0 \sqcap g_0,
\qquad
(f \sqcap g)_1 = f_1 \sqcap g_1.$$Các số hạng chéo biến mất vì một phần tử chỉ thuộc về giao điểm nếu nó có mặt trong cả hai toán hạng ở mọi vị trí biến đổi. 

### (c) Sai đối xứng$f \Delta g$Đây$$f \Delta g = \{ \alpha \oplus \beta \mid \alpha \in f,\ \beta \in g \},$$Ở đâu$\oplus$là hiệu đối xứng của các tập hợp. 

Các trường hợp cơ bản:$$\bot \Delta g = \bot,\quad f \Delta \bot = \bot,\quad \top \Delta g = g,\quad f \Delta \top = f.$$Đệ quy với$k=\min(V(f),V(g))$:$$(f \Delta g)_0 = f_0 \Delta g_0,$$

$$(f \Delta g)_1 = (f_1 \Delta g_0)\ \sqcup\ (f_0 \Delta g_1).$$Dòng thứ hai nối tiếp từ danh tính$$(A\oplus x)\oplus B = (A\oplus B)\oplus x,$$và phân vùng bởi sự hiện diện của$x_k$. 

### (d) Thương số$f/g$Theo định nghĩa,$$f/g = \{\alpha \mid \forall \beta \in g,\ \alpha \cup \beta \in f,\ \alpha \cap \beta = \varnothing\}.$$Các trường hợp cơ bản: 

Nếu$g=\bot$, điều kiện phổ quát là trống rỗng, do đó mọi$\alpha$được cho phép, vì vậy$f/g$là họ phổ quát trên miền biến, được biểu thị bằng thiết bị đầu cuối$\top$trong việc giải thích hàm Boolean. 

Nếu như$f=\bot$Và$g\neq \bot$, sau đó$f/g=\bot$. 

Nếu như$g=\top=\{\varnothing\}$, thì điều kiện giảm xuống$\alpha \in f$, kể từ đây$$f/\top = f.$$Đệ quy với$k=\min(V(f),V(g))$. Tách ra$g = g_0 \cup (e_k \sqcup g_1)$và tương tự cho$f$. 

Điều kiện thương được phân tách bằng việc liệu$x_k$bị buộc phải vắng mặt$\alpha$. 

Nếu như$k \notin g$(tất cả các bộ trong$g$bỏ qua$x_k$), sau đó$$(f/g)_0 = f_0/g,\qquad (f/g)_1 = f_1/g.$$Nếu như$k \in g$, thì bất kỳ$\alpha \in f/g$phải thỏa mãn tính rời rạc của tất cả các tập hợp trong$g_1$, buộc loại trừ$x_k$từ thuật ngữ tương tác và chúng tôi có được:$$(f/g)_0 = (f_0/g_0)\ \cap\ (f_1/g_1),
\qquad
(f/g)_1 = (f_1/g_0)\ \cap\ (f_0/g_1).$$Các mệnh đề này xuất phát trực tiếp từ việc phân phối bộ định lượng phổ quát qua việc phân rã$g$thành các phần bao gồm hoặc loại trừ$x_k$và thực thi khả năng tương thích trong$f$theo thành phần. 

###(e) Số dư$f \bmod g$Theo định nghĩa,$$f \bmod g = f \setminus (g \sqcup (f/g)).$$Đệ quy sử dụng các hoạt động đã được xác định:$$\mathrm{mod}(f,g) = \mathrm{BUTNOT}(f,\ \mathrm{Join}(g,\ \mathrm{Quot}(f,g))).$$Ở cấp độ nút, với$k=\min(V(f),V(g))$, chúng tôi tính toán:$$(f \bmod g)_0 = f_0 \bmod g_0,$$

$$(f \bmod g)_1 = f_1 \bmod g_0\ \setminus\ (g_1 \sqcup (f/g)).$$Thuật ngữ thứ hai loại bỏ chính xác những phần tử được tạo ra bằng cách nối$g$với thương số, được áp dụng nhất quán tại$x_k=1$chi nhánh. 

Cuối cùng, tất cả các kết quả đều được giảm bớt bằng cách chia sẻ nút và loại bỏ các bài kiểm tra dư thừa. 

Điều này hoàn thành việc xây dựng tất cả năm hoạt động bằng cách sử dụng khung BDD giảm theo thứ tự. ∎
