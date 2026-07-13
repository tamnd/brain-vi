---
title: "CF 103264B - \u041f\u043e\u0441\u044b\u043b\u043a\u0438 \u0432 \u0437\u0430\u043c\u043e\u0440\u043e\u0437\u043a\u0443"
description: "Giả sử $(a{ij})$ là một bảng dự phòng $mtimes n$ với các mục nhập số nguyên không âm, tổng hàng $ri=sum{j=1}^n a{ij},$ và tổng cột $cj=sum{i=1}^m a{ij},$ với $sum{i=1}^m ri=sum{j=1}^n cj$."
date: "2026-07-03T14:49:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103264
codeforces_index: "B"
codeforces_contest_name: "XVI \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 103264
solve_time_s: 148
verified: false
draft: false
---

[CF 103264B - \u041f\u043e\u0441\u044b\u043b\u043a\u0438 \u0432 \u0437\u0430\u043c\u043e\u0440\u043e\u0437\u043a\u0443](https://codeforces.com/problemset/problem/103264/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$(a_{ij})$là một$m\times n$bảng dự phòng với các mục số nguyên không âm, tổng hàng$r_i=\sum_{j=1}^n a_{ij},$và tổng cột$c_j=\sum_{i=1}^m a_{ij},$với$\sum_{i=1}^m r_i=\sum_{j=1}^n c_j$. 

Chúng ta phải chỉ ra rằng tất cả các bảng như vậy có thể được liệt kê theo một trình tự trong đó các bảng kế tiếp khác nhau đúng bốn phần tử của ma trận. 

Một thay đổi ảnh hưởng đến chính xác bốn mục có nghĩa là một nước đi được hỗ trợ trên một$2\times 2$ma trận con$$\begin{pmatrix}
(i,j) & (i,j')\\
(i',j) & (i',j')
\end{pmatrix},$$trong đó hai mục tăng thêm$1$và hai số còn lại giảm đi$1$, bảo toàn tổng hàng và cột. 

##Giải pháp 

Sửa cây bao trùm$T$của đồ thị lưỡng cực hoàn chỉnh$K_{m,n}$gồm tất cả các cạnh liên tiếp với đỉnh$1$ở mỗi phần:$$T=\{(i,1)\mid 1\le i\le m\}\cup \{(1,j)\mid 2\le j\le n\}.$$Đây là một cây có$m+n-1$các cạnh. 

Mỗi cạnh không phải cây là một cặp$(i,j)$với$i\ge 2$Và$j\ge 2$. Đối với mỗi cạnh như vậy, việc thêm nó vào$T$tạo ra một chu kỳ dài duy nhất$4$:$$(i,j)\to (i,1)\to (1,1)\to (1,j)\to (i,j).$$Do đó mỗi cạnh không phải là cây xác định một$4$-xe đạp. 

Giới thiệu các biến$x_{ij}=a_{ij}\quad (2\le i\le m,\ 2\le j\le n).$Các mục còn lại được xác định bởi các ràng buộc hàng và cột. Vì$i\ge 2$,$a_{i1}=r_i-\sum_{j=2}^n x_{ij}.$Vì$j\ge 2$,$a_{1j}=c_j-\sum_{i=2}^m x_{ij}.$Cuối cùng,$a_{11}=r_1-\sum_{j=2}^n a_{1j}.$Thay thế biểu thức cho$a_{1j}$cho$a_{11}=r_1-\sum_{j=2}^n c_j+\sum_{j=2}^n\sum_{i=2}^m x_{ij}.$Do đó, mỗi bảng dự phòng được biểu diễn duy nhất bằng vectơ$(x_{ij})_{i\ge 2,j\ge 2}$chỉ tuân theo các ràng buộc không âm trên các mục dẫn xuất. 

Các biến tự do tạo thành một mảng hình chữ nhật có kích thước$(m-1)\times (n-1)$. Mỗi$x_{ij}$phạm vi trong một khoảng được xác định bởi các giá trị cố định trước đó: 

tăng hoặc giảm$x_{ij}$qua$1$thay đổi chính xác bốn mục phụ thuộc$a_{ij},\quad a_{i1},\quad a_{1j},\quad a_{11}.$Quả thực, ngày càng tăng$x_{ij}$qua$1$tạo ra các bản cập nhật$a_{ij}\leftarrow a_{ij}+1,\quad a_{i1}\leftarrow a_{i1}-1,\quad a_{1j}\leftarrow a_{1j}-1,\quad a_{11}\leftarrow a_{11}+1,$bảo toàn tất cả các tổng hàng và cột vì mỗi hàng và cột bị ảnh hưởng nhận được một$+1$và một$-1$sự đóng góp. 

Mọi chuyển đổi khả thi giữa hai bảng khác nhau một đơn vị trong một số$x_{ij}$do đó được thực hiện bằng chính xác bốn thay đổi mục nhập. 

Để tạo tất cả các bảng, hãy sắp xếp các biến$x_{ij}$theo thứ tự từ điển$(x_{22},x_{23},\dots,x_{2n},x_{32},\dots,x_{mn}).$Mỗi$x_{ij}$có một khoảng hữu hạn các giá trị nguyên chấp nhận được xác định bởi tính không âm của$a_{i1}$Và$a_{1j}$. Các giới hạn này chỉ phụ thuộc vào các biến được chọn trước đó theo thứ tự từ điển, do đó là tập hợp các khả thi$(x_{ij})$là tích của các phạm vi số nguyên giới hạn ở dạng từ điển. 

Áp dụng việc tạo từ điển trên mảng giới hạn này: ở mỗi bước, hãy tăng biến ngoài cùng bên phải$x_{ij}$có thể tăng lên trong khi vẫn giữ được tính khả thi và đặt lại tất cả các biến sau này về giá trị tối thiểu có thể chấp nhận được. Điều này tạo ra một đường đi Hamilton đi qua tất cả các khả thi$(x_{ij})$vectơ. 

Mỗi bước của quá trình này thay đổi chính xác một tọa độ$x_{ij}$qua$\pm 1$, do đó gây ra chính xác bốn thay đổi trong các mục ma trận tương ứng. Vì mỗi bảng khả thi tương ứng duy nhất với một vectơ$(x_{ij})$, tất cả các bảng dự phòng đều được tạo và các bảng kế tiếp khác nhau ở đúng bốn mục. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Việc xây dựng cây bao trùm đảm bảo rằng mọi cạnh không phải là cây$(i,j)$với$i,j\ge 2$đóng một độc đáo$4$-đi qua$(i,1)$,$(1,1)$, Và$(1,j)$. Bất kỳ sự thay đổi đơn vị nào trong chu kỳ này đều bảo toàn tổng số hàng vì hàng$i$và hàng$1$mỗi người nhận được một mức tăng và một mức giảm. Điều tương tự cũng xảy ra với các cột$j$Và$1$. 

Các công thức tái cấu trúc cho$a_{i1}$,$a_{1j}$, Và$a_{11}$đảm bảo rằng mỗi bảng được xác định bởi$(m-1)(n-1)$các biến tự do không có dư thừa và mọi ràng buộc không âm sẽ chuyển thành giới hạn hữu hạn trên mỗi biến sau khi các biến trước đó được cố định. 

Mỗi bước từ điển thay đổi chính xác một biến, do đó, sự thay đổi ma trận cảm ứng được giới hạn trong bốn ô tương ứng với giá trị liên quan.$4$-cycle, không có mục bổ sung nào bị ảnh hưởng. 

## Ghi chú 

Cấu trúc được sử dụng ở đây là không gian chu trình chuẩn của đồ thị hai bên hoàn chỉnh, với cơ sở là$4$-xe đạp neo ở đỉnh$(1,1)$. Đối số này giảm các bảng dự phòng thành các lưới số nguyên giới hạn, sau đó việc tạo từ điển mang lại một phép duyệt giống như mã Gray với các cập nhật kích thước không đổi trong biểu diễn ma trận ban đầu.
