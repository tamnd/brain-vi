---
title: "CF 103729B - Thuốc (phiên bản dễ)"
description: "Giả sử $g$ thu được từ $f$ bằng cách đặt $x{k+1} leftarrow xk$. Mọi hàm con của $g$ thu được bằng cách cố định các biến giữa $x1,dots,xk,x{k+2},dots,xn$, sau đó đánh giá $f$ theo ràng buộc bổ sung $x{k+1}=xk$."
date: "2026-07-02T09:15:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103729
codeforces_index: "B"
codeforces_contest_name: "2022 Hubei Provincial Collegiate Programming Contest"
rating: 0
weight: 103729
solve_time_s: 125
verified: false
draft: false
---

[CF 103729B - Potion(phiên bản dễ)](https://codeforces.com/problemset/problem/103729/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$g$được lấy từ$f$bằng cách thiết lập$x_{k+1} \leftarrow x_k$. Mỗi chức năng phụ của$g$thu được bằng cách cố định các biến giữa$x_1,\dots,x_k,x_{k+2},\dots,x_n$, sau đó đánh giá$f$dưới sự ràng buộc bổ sung$x_{k+1}=x_k$. Vì vậy mọi chức năng con của$g$có hình thức$$g(c_1,\dots,c_k, x_{k+2},\dots,x_n)
=
f(c_1,\dots,c_k, c_k, x_{k+2},\dots,x_n),$$vì vậy nó là một chức năng phụ của$f$được giới hạn ở tập hợp con của các bài tập thỏa mãn$x_{k+1}=x_k$. 

Cho phép$\tau$là bất kỳ bảng phụ nào của$g$. Sau đó$\tau$được lấy từ một bảng phụ$\sigma$của$f$bằng cách xác định hai tọa độ$x_k$Và$x_{k+1}$trước khi đánh giá. Nếu như$\tau$là một hạt thì nó là bảng con nguyên thủy của$g$, do đó nó tương ứng với một nút nhánh duy nhất trong BDD của$g$bằng sự tương ứng giữa nút-hạt trong Phần 7.1.4. 

Xác định ánh xạ từ bảng con của$g$đến bảng phụ của$f$bằng cách nâng từng bài tập lên$g$đến một bài tập về$f$với$x_{k+1}=x_k$. bảng phụ riêng biệt của$g$ánh xạ tới bảng phụ của$f$vẫn khác biệt sau khi áp đặt ràng buộc đẳng thức, vì đẳng thức chỉ có thể xác định các đánh giá khác biệt trước đó, không bao giờ tách biệt các đánh giá giống hệt nhau. Do đó số lượng hạt riêng biệt của$g$không vượt quá số lượng hạt riêng biệt của$f$. Vì thế$$B(g)\le B(f).$$Điều này hoàn thành việc chứng minh. ∎ 

Đối với câu hỏi thứ hai, hãy$h$được lấy từ$f$bằng cách thiết lập$x_{k+2} \leftarrow x_k$. Sự bất bình đẳng$B(h)\le B(f)$nói chung là không giữ được. 

Để thấy được điều này, chỉ cần xây dựng một hàm trong đó sự nhận dạng sẽ phá hủy sự đối xứng về cấu trúc mà trước đây khiến nhiều hàm con trùng khớp với nhau. 

Cho phép$f(x_1,x_2,x_3,x_4)$được xác định bởi$$f(x_1,x_2,x_3,x_4)
=
(x_1 \land x_3)\ \lor\ (\bar{x}_1 \land x_4).$$Đây là sự phân rã kiểu Shannon trong$x_1$với hai nhánh độc lập. Chức năng phụ cho$x_1=1$là$x_3$và hàm con của$x_1=0$là$x_4$, vậy BDD có gốc được gắn nhãn$x_1$và hai đồ thị con rời rạc chỉ phụ thuộc vào các biến duy nhất. Mỗi sơ đồ con đó có một nút nhánh và cả hai đều có chung hai phần chìm, vì vậy$B(f)$là tối thiểu cho cấu trúc này. 

Bây giờ hãy hình thành$h$bằng cách thiết lập$x_3 \leftarrow x_1$, cho$$h(x_1,x_2,x_4)
=
(x_1 \land x_1)\ \lor\ (\bar{x}_1 \land x_4)
=
x_1 \lor (\bar{x}_1 \land x_4).$$Điều này đơn giản hóa thành một hàm trong đó$x_1=0$chi nhánh vẫn phụ thuộc vào$x_4$, nhưng$x_1=1$nhánh trở thành hằng số$1$. BDD kết quả có nhãn gốc$x_1$, cạnh HI đi thẳng tới$\top$và cạnh LO đánh giá$x_4$, sau đó kết nối với cả hai$\top$Và$\bot$. Điều này giới thiệu một cấu trúc nút nhánh không cần thiết bổ sung không tồn tại trong biểu diễn rút gọn của$f$bởi vì$x_1=1$Và$x_1=0$các nhánh trước đây có kích thước đối xứng và bây giờ thì không. 

Quan trọng hơn, mô hình chia sẻ ban đầu giữa các chức năng con của$f$dựa vào sự độc lập của$x_3$Và$x_4$. Sau khi xác định$x_3=x_1$, tính độc lập đó bị mất và hai chức năng con của$h$ở gốc trở nên không thể so sánh được về mặt cấu trúc, buộc phải có một sự phân hủy hoàn toàn khác. Tập hợp các hạt kết quả cho$h$chứa một tập hợp các bảng phụ hoàn toàn khác với các bảng phụ$f$và trong cách xây dựng này, sơ đồ rút gọn cho$h$yêu cầu các nút nonterminal khác biệt hơn so với sơ đồ rút gọn cho$f$. 

Do đó, tồn tại các hàm Boolean mà việc xác định các biến không liền kề sẽ làm tăng số lượng hạt sau khi giảm và do đó có thể tăng kích thước BDD. Do đó không có bất đẳng thức tổng quát$B(h)\le B(f)$nắm giữ. 

Điều này hoàn thành việc chứng minh. ∎
