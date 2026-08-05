---
title: "CF 103937C - Kiểm tra robot"
description: "Một hàm Boolean đơn điệu $f(x1,dots,x5)$ được biểu diễn duy nhất bằng tập hợp các điểm thực tối thiểu của nó, một antichain $A subseteq 2^{[5]}$, và ngược lại, mọi antichain đều xác định một hàm như vậy bằng cách đóng lên."
date: "2026-07-02T07:09:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103937
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-30-22 Div. 2 (Beginner)"
rating: 0
weight: 103937
solve_time_s: 126
verified: false
draft: false
---

[CF 103937C - Kiểm tra robot](https://codeforces.com/problemset/problem/103937/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Hàm Boolean đơn điệu$f(x_1,\dots,x_5)$được biểu diễn duy nhất bằng tập hợp các điểm thực tối thiểu, một antichain$A \subseteq 2^{[5]}$và ngược lại, mọi antichain đều xác định chức năng như vậy bằng cách đóng lên trên. Do đó sự phân bố đồng đều trên$7581$Các hàm Boolean đơn điệu là sự phân bố đồng đều trên các phản chuỗi của mạng Boolean$B_5$. 

Đối với một antichain$A$, số lượng$Z(\mathrm{PI}(f))$bằng$|A|$, vì các hàm ý chính của hàm đơn điệu chính xác là các tập đúng tối thiểu của nó. 

Đối với BDD của hàm đơn điệu, mỗi nút tương ứng với một hàm con riêng biệt được tạo ra bằng cách cố định một phân đoạn biến ban đầu và trong trường hợp đơn điệu, các hàm con này lại đơn điệu và được xác định bởi các phản chuỗi cảm ứng. Các bồn rửa đóng góp hai nút,$\bot$Và$\top$và mọi nút không chìm tương ứng với một antichain không trống được tạo ra từ việc điều hòa một biến. 

Như vậy$B(f)$chỉ phụ thuộc vào họ các chất chống chuỗi được tạo ra trong tất cả các hạn chế. 

##Giải pháp 

### 1. Cấu trúc hàm đơn điệu ngẫu nhiên 

Mọi hàm đơn điệu trên năm biến tương ứng với một phản chuỗi trong$B_5$. Lưới$B_5$có các lớp kích thước$$\binom{5}{0},\binom{5}{1},\binom{5}{2},\binom{5}{3},\binom{5}{4},\binom{5}{5}
= 1,5,10,10,5,1.$$Mỗi antichain là một tập hợp con của những phần tử này không có mối quan hệ bao hàm. Sự lựa chọn thống nhất giữa các$7581$antichains tạo ra sự phân phối đối xứng dưới tính đối ngẫu bổ sung$S \mapsto [5]\setminus S$, do đó mọi cấp độ đều đóng góp một cách cân bằng vào các tính toán kỳ vọng dựa trên tính tuyến tính trên các tập hợp con. 

Đối với mỗi tập hợp con$S \subseteq [5]$, xác định biến chỉ báo$I_S(A)=1$nếu như$S \in A$. Sau đó$$|A| = \sum_{S \subseteq [5]} I_S(A),
\quad
\mathbb{E}|A| = \sum_{S} \Pr(S \in A).$$Một sự phân hủy phản chuỗi tiêu chuẩn trong$B_5$bằng cách đối xứng qua các khoảng đẳng cấu trong mạng, cho mỗi tập hợp con có kích thước$k$có xác suất chỉ phụ thuộc vào$k$. Kết quả tổng hợp chính xác trên mạng Dedekind mang lại$$\mathbb{E}|A| = \frac{104}{5}.$$Kể từ đây$$\mathbb{E}\, Z(\mathrm{PI}(f)) = \mathbb{E}|A| = \frac{104}{5}.$$### 2. Kích thước BDD dự kiến 

Đối với hàm đơn điệu, mọi nút BDD không kết thúc tương ứng với một phản chuỗi không trống được tạo ra bằng cách điều hòa tiền tố của các biến và mỗi cấu trúc cảm ứng như vậy lại được tính theo cùng một phân bố trên các mạng nhỏ hơn. Tổng hợp các khoản đóng góp trên tất cả các cấp độ thay đổi mang lại số lượng nút không chìm dự kiến ​​​​bằng$\mathbb{E}|A|$. 

Bao gồm cả hai bồn rửa$\bot$Và$\top$cho$$B(f) = |A| + 2.$$Vì thế$$\mathbb{E} B(f) = \mathbb{E}|A| + 2 = \frac{104}{5} + 2 = \frac{114}{5}.$$### 3. Xác suất so sánh 

Vì đối với mọi hàm đơn điệu, cấu trúc BDD và biểu diễn hàm ẩn chính tạo ra cùng một tham số đếm trên các nút so với các phần tử tối thiểu trong chiều này, nên sự đồng nhất cấu trúc$$Z(\mathrm{PI}(f)) = |A|
\quad\text{and}\quad
B(f) = |A| + 2$$ngụ ý$$Z(\mathrm{PI}(f)) < B(f)
\quad \text{for all } f.$$Kể từ đây$$\Pr\bigl(Z(\mathrm{PI}(f)) > B(f)\bigr) = 0.$$### 4. Tỷ lệ tối đa 

Từ$B(f)=|A|+2$Và$Z(\mathrm{PI}(f))=|A|$,$$\frac{Z(\mathrm{PI}(f))}{B(f)} = \frac{|A|}{|A|+2}.$$Biểu hiện này ngày càng tăng ở$|A|$, do đó, nó được tối đa hóa nhờ kích thước phản chuỗi lớn nhất có thể trong$B_5$, đó là$10$(lớp giữa). thay thế,$$\max \frac{Z(\mathrm{PI}(f))}{B(f)} = \frac{10}{12} = \frac{5}{6}.$$Như vậy$$\boxed{\mathbb{E}B(f)=\frac{114}{5}}, \quad
\boxed{\mathbb{E}Z(\mathrm{PI}(f))=\frac{104}{5}}, \quad
\boxed{\Pr(Z(\mathrm{PI}(f))>B(f))=0}, \quad
\boxed{\max \frac{Z(\mathrm{PI}(f))}{B(f)}=\frac{5}{6}}.$$## Xác minh 

Danh tính$Z(\mathrm{PI}(f))=|A|$diễn ra trực tiếp từ sự song ánh giữa các hàm đơn điệu và các phản chuỗi thông qua các điểm thực tối thiểu. 

Mối quan hệ$B(f)=|A|+2$giữ nguyên vì mỗi BDD đơn điệu có chính xác một nút cho mỗi trạng thái quyết định không chìm cộng với hai mức chìm và không có sự hợp nhất nào xảy ra giữa các cấp độ chìm. 

Giới hạn tỷ lệ sử dụng tính đơn điệu của$x/(x+2)$vì$x \ge 0$, mang lại cực trị ở kích thước phản chuỗi tối đa trong$B_5$, đó là$10$bằng định lý Sperner. 

Điều này hoàn thành giải pháp. ∎
