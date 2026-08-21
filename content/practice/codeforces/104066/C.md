---
title: "CF 104066C - \u0414\u0430\u0432\u0430\u0439\u0442\u0435 \u0440\u0430\u0437\u0434\u0435\u043b\u0438\u043c\u0441\u044f!"
description: "Đặt $F = mathrm{MUX}(f,g,h)$ biểu thị hàm Boolean được xác định bằng cách chọn $g$ khi $f=1$ và chọn $h$ khi $f=0$, sao cho $$F = (f nêm g) vee (neg f nêm h)."
date: "2026-07-02T03:14:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104066
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u0431\u0430\u0437\u043e\u0432\u0430\u044f \u0432\u0435\u0440\u0441\u0438\u044f)"
rating: 0
weight: 104066
solve_time_s: 128
verified: false
draft: false
---

[CF 104066C - \u0414\u0430\u0432\u0430\u0439\u0442\u0435 \u0440\u0430\u0437\u0434\u0435\u043b\u0438\u043c\u0441\u044f!](https://codeforces.com/problemset/problem/104066/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$F = \mathrm{MUX}(f,g,h)$biểu thị hàm Boolean được xác định bằng cách chọn$g$khi$f=1$và lựa chọn$h$khi$f=0$, để có thể$$F = (f \wedge g)\ \vee\ (\neg f \wedge h).$$Nhiệm vụ là xây dựng ZDD cho$F$sử dụng các phép toán ZDD tiêu chuẩn và tôn trọng các quy tắc sắp xếp và rút gọn biến cố định trong Phần 7.1.4. 

Việc xây dựng tiến hành bằng đệ quy cấu trúc trên biến trên cùng của các sơ đồ liên quan. Cho phép$V(f)$biểu thị chỉ số của biến gốc của$f$, và tương tự cho$g$Và$h$. Cho phép$v$là mức tối thiểu của các chỉ số này, vì vậy$v = \min(V(f), V(g), V(h))$, trong đó mức chìm được coi là không có biến và do đó không ràng buộc mức tối thiểu. 

Mỗi sơ đồ được phân rã theo biến$x_v$thành cofactor thấp và cofactor cao. Nếu như$x_v$không xuất hiện ở gốc của sơ đồ, sơ đồ đó không thay đổi trong cả hai hệ số. Nếu nó xuất hiện thì các phiên bản kế nhiệm cao và thấp của nó sẽ được sử dụng. 

Như vậy mỗi đối số được viết lại dưới dạng$$f = (f_0, f_1), \quad g = (g_0, g_1), \quad h = (h_0, h_1),$$trong đó cặp biểu thị các đồng yếu tố ZDD đối với$x_v$, được hiểu là đẳng thức khi biến gốc vượt quá$v$. 

chức năng$F$sau đó được xác định bằng khai triển Shannon tại$x_v$. Thay thế$f = (x_v?f_1:f_0)$vào biểu thức xác định mang lại$$F = (x_v \wedge f_1 \wedge g)\ \vee\ (\neg x_v \wedge f_0 \wedge h).$$Tách các trường hợp$x_v=0$Và$x_v=1$cung cấp cho các đồng yếu tố của$F$:$$F_0 = f_0 \wedge h_0,
\qquad
F_1 = f_1 \wedge g_1.$$Sự đơn giản hóa này sử dụng điều đó trong$x_v=0$nhánh, điều kiện$f$giảm xuống$f_0$, do đó việc lựa chọn giảm xuống còn$h$, và trong$x_v=1$nhánh, lựa chọn giảm xuống$g$. 

Tuy nhiên, khi$x_v$không xảy ra ở một hoặc nhiều$f,g,h$, các hệ số được diễn giải theo quy tắc mở rộng tiêu chuẩn: sơ đồ không có biến$v$được sao chép không thay đổi vào cả hai nhánh. 

Do đó, định nghĩa đệ quy của nút ZDD cho$F$được xác định như sau. Nếu như$f$thì là một cái bồn rửa$$\mathrm{MUX}(\bot,g,h) = h,
\qquad
\mathrm{MUX}(\top,g,h) = g.$$Những danh tính này theo trực tiếp từ$(\bot \wedge g)\vee(\top \wedge h)=h$Và$(\top \wedge g)\vee(\bot \wedge h)=g$. 

Nếu như$f$là một nút không chìm được gắn nhãn bởi$x_v$, viết$f=(f_0,f_1)$. Sau đó, kết quả là một nút được gắn nhãn bởi$x_v$có con trỏ thấp và cao được tính toán đệ quy:$$\mathrm{MUX}(f,g,h)_0 = \mathrm{MUX}(f_0, g_0, h_0),
\qquad
\mathrm{MUX}(f,g,h)_1 = \mathrm{MUX}(f_1, g_1, h_1).$$Nếu như$g$hoặc$h$có biến gốc lớn hơn$v$, đồng yếu tố của họ thỏa mãn$g_0=g_1=g$Và$h_0=h_1=h$, do đó phép đệ quy tự động duy trì tính chính xác theo ràng buộc thứ tự. 

Việc giảm thiểu được thực thi theo cách ZDD tiêu chuẩn. Nếu nút con thấp và cao được tính toán trùng nhau thì nút đó sẽ bị loại bỏ và thay thế bằng nút con đó. Nếu một bộ ba giống hệt nhau$(v,F_0,F_1)$đã được xây dựng, nút hiện có sẽ được sử dụng lại, đảm bảo tính chính tắc. 

Tính đúng đắn xuất phát từ thực tế là phân rã Shannon tại biến nhỏ nhất$x_v$phân vùng miền thành các trường hợp rời rạc$x_v=0$Và$x_v=1$và trong mỗi trường hợp biểu thức xác định của MUX giảm chính xác thành một trong hai$h$hoặc$g$với các đồng yếu tố nhất quán. Phép đệ quy duy trì thứ tự biến vì mỗi lệnh gọi đệ quy được thực hiện trên các chỉ số biến lớn hơn và nó duy trì mức giảm ZDD do không có nút nào có các nút kế thừa thấp và cao giống hệt nhau được giữ lại. 

Điều này hoàn thành việc xây dựng$\mathrm{MUX}(f,g,h)$như một hoạt động ZDD. ∎
