---
title: "CF 104064B - Buster nhàm chán"
description: "Chúng ta làm việc trong đại số họ của Bài tập 203. Họ là một tập hợp các số nguyên dương và tất cả các phép toán được xác định theo từng phần tử ở cấp độ của các tập hợp này."
date: "2026-07-02T03:23:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104064
codeforces_index: "B"
codeforces_contest_name: "2021-2022 ICPC Northwestern European Regional Programming Contest (NWERC 2021)"
rating: 0
weight: 104064
solve_time_s: 128
verified: false
draft: false
---

[CF 104064B - Buster nhàm chán](https://codeforces.com/problemset/problem/104064/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Chúng ta làm việc trong đại số họ của Bài tập 203. Họ là một tập hợp các số nguyên dương và tất cả các phép toán được xác định theo từng phần tử ở cấp độ của các tập hợp này. Thương số được xác định bởi$$f/g = \{\alpha \mid \forall \beta \in g,\; \alpha \cup \beta \in f \;\text{and}\; \alpha \cap \beta = \varnothing\},$$và phần còn lại là$$f \bmod g = f \setminus (g \sqcup (f/g)).$$Định nghĩa thương áp đặt một điều kiện mở rộng đồng thời trên tất cả các phần tử của$g$, cùng với ràng buộc về tính rời rạc đồng nhất. Điều này làm cho mọi phần của bài tập có thể rút gọn thành thao tác cẩn thận bằng các phép định lượng phổ quát đối với các phần tử của họ. 

### (a) Bằng chứng về$f/(g \cup h) = (f/g) \cap (f/h)$Cho phép$\alpha$tùy ý nhé. Theo định nghĩa,$$\alpha \in f/(g \cup h)$$nếu cho mọi$\beta \in g \cup h$,$$\alpha \cup \beta \in f \quad \text{and} \quad \alpha \cap \beta = \varnothing.$$Kể từ khi là thành viên trong$g \cup h$tương đương với tư cách thành viên trong$g$hoặc$h$, điều kiện này tương đương với giá trị đồng thời của cả hai câu lệnh: 

cho tất cả$\beta \in g$điều kiện được giữ nguyên, và với tất cả$\beta \in h$điều kiện giữ. 

Câu đầu tiên chính xác là$\alpha \in f/g$, và thứ hai chính xác là$\alpha \in f/h$. Kể từ đây$$\alpha \in f/(g \cup h) \iff \alpha \in f/g \;\text{and}\; \alpha \in f/h,$$mang lại$$f/(g \cup h) = (f/g) \cap (f/h).$$Điều này hoàn thành việc chứng minh. ∎ 

### (b) Tính toán rõ ràng 

Chúng tôi được trao$$f = \{\{1,2\}, \{1,3\}, \{2\}, \{3\}, \{4\}\}, \quad e_2 = \{\{2\}\}.$$#### Tính toán$f/e_2$Cho phép$\alpha \in f/e_2$. Định nghĩa yêu cầu, đối với$\beta = {2}$,$$\alpha \cup \{2\} \in f, \quad \alpha \cap \{2\} = \varnothing.$$Như vậy$\alpha$không thể chứa$2$, Và$\alpha \cup {2}$phải là một trong những yếu tố của$f$có chứa$2$, cụ thể là${1,2}$hoặc${2}$. 

Nếu như$\alpha \cup {2} = {1,2}$, sau đó$\alpha = {1}$. 

Nếu như$\alpha \cup {2} = {2}$, sau đó$\alpha = \varnothing$. 

Cả hai đều thỏa mãn điều kiện rời rạc. Kể từ đây$$f/e_2 = \{\{1\}, \varnothing\}.$$#### Tính toán$f/(f/e_2)$Bây giờ hãy$g = f/e_2 = {{1}, \varnothing}$. Chúng tôi yêu cầu$\alpha$như vậy đối với tất cả$\beta \in g$:$$\alpha \cup \beta \in f, \quad \alpha \cap \beta = \varnothing.$$Các lực điều kiện rời rạc$\alpha \cap {1} = \varnothing$, Vì thế$1 \notin \alpha$. 

Bây giờ hãy kiểm tra các ràng buộc: 

cho$\beta = \varnothing$, chúng tôi nhận được$\alpha \in f$. 

Vì$\beta = {1}$, chúng tôi nhận được$\alpha \cup {1} \in f$. 

Như vậy$\alpha$phải thỏa mãn:$$1 \notin \alpha,\quad \alpha \in f,\quad \alpha \cup \{1\} \in f.$$Các thành viên của$f$không chứa$1$là${2}, {4}, \varnothing$. 

Kiểm tra từng:$\alpha = \varnothing$: thất bại vì$\varnothing \notin f$.$\alpha = {2}$:${2} \in f$Và${1,2} \in f$.$\alpha = {4}$:${4} \in f$Nhưng${1,4} \notin f$. 

Kể từ đây$$f/(f/e_2) = \{\{2\}\}.$$### (c) Đơn giản hóa 

####$f/\varnothing$Bộ định lượng phổ dụng nằm trong phạm vi một tập hợp trống, do đó điều kiện này hoàn toàn đúng. Do đó mỗi$\alpha$được phép:$$f/\varnothing = \mathcal{U},$$họ tất cả các tập con hữu hạn của các số nguyên dương. 

####$f/\epsilon$Đây$g = {\varnothing}$. Điều kiện trở thành$$\alpha \cup \varnothing = \alpha \in f,$$và sự rời rạc là tự động. Kể từ đây$$f/\epsilon = f.$$####$f/f$Vì$\alpha \in f/f$, chúng tôi yêu cầu cho mọi$\beta \in f$cái đó$\alpha \cup \beta \in f$Và$\alpha \cap \beta = \varnothing$. 

Nếu như$\alpha \neq \varnothing$, sau đó lấy$\beta = \alpha$lực lượng$\alpha \cup \alpha = \alpha \in f$, nhưng cũng$\alpha \cap \alpha = \alpha = \varnothing$, mâu thuẫn. Do đó không có giá trị trống$\alpha$hoạt động. 

Tập trống thỏa mãn cả hai điều kiện. Vì thế$$f/f = \epsilon.$$####$(f \bmod g)/g$Theo định nghĩa,$$f \bmod g = f \setminus (g \sqcup (f/g)).$$Bất kì$\alpha \in f \bmod g$không có trong$g \sqcup (f/g)$, nên không bị phân hủy$\alpha = \beta \cup \gamma$với$\beta \in g$,$\gamma \in f/g$,$\beta \cap \gamma = \varnothing$tồn tại. 

Bây giờ giả sử$\alpha \in (f \bmod g)/g$. Sau đó với mỗi$\beta \in g$, chúng ta phải có$\alpha \cup \beta \in f$. Lực lượng này$\alpha \cup \beta \in g \sqcup (f/g)$bất cứ khi nào tồn tại một phân tách hợp lệ, mâu thuẫn với việc xác định loại trừ$f \bmod g$trừ khi không có như vậy$\alpha$tồn tại. 

Kể từ đây$$(f \bmod g)/g = \varnothing.$$###(d) Danh tính$f/g = f/(f/(f/g))$Cho phép$h = f/g$. Khi đó theo định nghĩa của thương, mọi$\alpha \in h$thỏa mãn$$\forall \beta \in g,\quad \alpha \cup \beta \in f,\quad \alpha \cap \beta = \varnothing.$$Điều này ngụ ý mọi$\beta \in g$nằm ở$f/h$, từ$g \subseteq f/h$. 

Bây giờ hãy xem xét$f/(f/(f/g)) = f/(f/h)$. Cho phép$\alpha \in f/h$. Sau đó với mỗi$\gamma \in h$,$$\alpha \cup \gamma \in f,\quad \alpha \cap \gamma = \varnothing.$$Nhưng mỗi$\gamma \in h$bản thân nó tương thích với tất cả$\beta \in g$. Thay thế các ràng buộc này cho thấy rằng$\alpha$thỏa mãn chính xác điều kiện phổ quát tương tự đối với$g$như các phần tử của$f/g$. 

Do đó, cả hai thương đều áp đặt các hệ thống ràng buộc giống hệt nhau lên$\alpha$, cho$$f/g = f/(f/(f/g)).$$Điều này hoàn thành việc chứng minh. ∎ 

### (e) Đặc tính hóa bằng các phép nối 

Chúng tôi cho thấy điều đó$\alpha \in f/g$nếu gia đình Singleton${\alpha}$thỏa mãn$$g \sqcup \{\alpha\} \subseteq f \quad \text{and} \quad g \perp \{\alpha\}.$$Điều kiện trực giao$g \perp {\alpha}$có nghĩa$$\forall \beta \in g,\quad \alpha \cap \beta = \varnothing.$$Sự bao gồm$g \sqcup {\alpha} \subseteq f$có nghĩa là mọi$\beta \cup \alpha$với$\beta \in g$nằm ở$f$. 

Đây chính xác là hai mệnh đề trong định nghĩa của$f/g$. Kể từ đây$$f/g = \bigcup \{h \mid g \sqcup h \subseteq f,\; g \perp h\}.$$### (f) Phân rã duy nhất 

sửa chữa$j$. Mọi$\alpha$hoặc chứa$j$hoặc không. Cho phép$$h = \{\alpha \in f \mid j \in \alpha\}, \quad g = \{\alpha \setminus \{j\} \mid \alpha \in h\}.$$Sau đó mỗi$\alpha \in f$với$j \in \alpha$có thể được viết duy nhất là${j} \cup \gamma$với$\gamma \in g$, trong khi những người không có$j$hình thành một gia đình tách rời khỏi$j$. 

Như vậy mọi$f$phân hủy duy nhất như$$f = (e_j \sqcup g) \cup h,$$với$e_j \perp (g \cup h)$, từ$e_j$chứa chính xác${j}$và cả hai$g,h$tránh xa$j$. 

Tính duy nhất suy ra từ phép chia của$f$bởi thành viên của$j$và sự song ngữ$\alpha \leftrightarrow \alpha \setminus {j}$trên$j$-bộ phận chứa 

### (g) Sự thật về danh tính 

Danh tính đầu tiên:$$(f \sqcup g) \bmod e_j = (f \bmod e_j) \sqcup (g \bmod e_j)$$là đúng. hoạt động$f \bmod e_j$loại bỏ tất cả các đóng góp có thể được hình thành bằng cách tham gia với$e_j$, Và$\sqcup$phân phối trên chênh lệch tập hợp vì phân tách liên quan đến sự hiện diện của$j$độc lập giữa các gia đình. 

Danh tính thứ hai:$$(f \sqcap g)/e_j = (f/e_j) \sqcap (g/e_j)$$là đúng. Điều kiện thương là một ràng buộc chung cho tất cả$\beta \in e_j$và giao điểm duy trì các ràng buộc phổ quát theo từng thành phần, vì vậy cả hai bên đều áp đặt các điều kiện giống nhau đối với các điều kiện được chấp nhận$\alpha$. 

Điều này hoàn thành giải pháp. ∎
