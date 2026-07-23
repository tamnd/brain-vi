---
title: "CF 103720E - \u041c\u0430\u043a\u0441\u0438\u043c\u0438\u0437\u0438\u0440\u0443\u0439 VÀ"
description: "Cho $f$ là một hàm Boolean của các biến $x1,dots,xn$ và cho $g$ thu được từ $f$ bằng cách ngưng tụ $x{k+1} leftarrow xk$. Do đó $g$ là hạn chế của $f$ đối với phép thay thế theo đường chéo trong đó mọi lần xuất hiện của $x{k+1}$ đều được thay thế bằng $xk$."
date: "2026-07-02T09:19:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103720
codeforces_index: "E"
codeforces_contest_name: "VII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b. 3-7 \u043a\u043b\u0430\u0441\u0441\u044b"
rating: 0
weight: 103720
solve_time_s: 52
verified: false
draft: false
---

[CF 103720E - \u041c\u0430\u043a\u0441\u0438\u043c\u0438\u0437\u0438\u0440\u0443\u0439 AND](https://codeforces.com/problemset/problem/103720/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$f$là hàm Boolean của các biến$x_1,\dots,x_n$và để$g$được lấy từ$f$bằng sự ngưng tụ$x_{k+1} \leftarrow x_k$. Như vậy$g$là hạn chế của$f$đến sự thay thế theo đường chéo trong đó mỗi lần xuất hiện của$x_{k+1}$được thay thế bởi$x_k$. 

Bảng con của bảng chân lý được xác định bằng cách gán cố định cho tiền tố của các biến. Đặc biệt, một bảng phụ của$g$thu được bằng cách sửa chữa$(x_1,\dots,x_i)$tương ứng chính xác với bảng phụ của$f$thu được bằng cách sửa chữa$(x_1,\dots,x_i)$và sau đó thực thi$x_{k+1}=x_k$trong tất cả các đánh giá còn lại. Sự tương ứng này bảo toàn việc đánh giá theo điểm, do đó mỗi chức năng con của$g$thu được từ một hàm con của$f$bằng sự thay thế tương tự. 

Cho phép$\tau$là một bảng phụ của$g$. Lập bảng phụ tương ứng$\tau'$của$f$thu được bằng cách giải thích cùng một hạn chế đối với các biến và sau đó thay thế$x_{k+1}=x_k$. Bản đồ$\tau \mapsto \tau'$có tính chất tiêm vào các bảng phụ, vì các phép gán riêng biệt cho$(x_1,\dots,x_i)$vì$g$tạo ra các nhiệm vụ riêng biệt cho$f$trước khi thay thế và bình đẳng sau khi thay thế sẽ bao hàm sự bình đẳng của các hạn chế ban đầu. 

Một hạt$g$là một bảng phụ của$g$đó là nguyên thủy, có nghĩa là nó không phải là hình vuông. Nếu như$\tau$là một hạt$g$, thì tương ứng$\tau'$là một bảng phụ của$f$đó cũng là nguyên thủy, vì sự thay thế$x_{k+1}=x_k$duy trì sự phân chia bảng chân lý thành hai nửa trái và phải đối với biến cao nhất trong hàm con: nếu$\tau$là một hình vuông thì$\tau'$cũng sẽ là một hình vuông, mâu thuẫn với điều đó$\tau'$phát sinh từ một hàm con hợp lệ của$f$theo các quy tắc phân rã giống hệt nhau. Do đó mỗi hạt của$g$ánh xạ tới một hạt riêng biệt của$f$. 

Do đó việc lập bản đồ từ các hạt của$g$thành những hạt$f$là chất tiêm nên số lượng hạt thỏa mãn$B(g) \le B(f)$, từ$B(\cdot)$đếm các hạt cùng với các nút chìm cố định không bị ảnh hưởng bởi sự thay thế. 

Điều này hoàn thành việc chứng minh. ∎ 

Bây giờ hãy$h$được lấy từ$f$bằng cách thiết lập$x_{k+2} \leftarrow x_k$. Sự đơn điệu tương tự không cần phải giữ. 

Hạn chế hiện xác định hai biến không liền kề theo thứ tự BDD. Điều này có thể thu gọn các chức năng con riêng biệt của$f$thành một chức năng con duy nhất của$h$theo cách vi phạm sự liên kết của các cấp độ trong cấu trúc có trật tự và nó cũng có thể tạo ra sự trùng hợp mới giữa các bảng con mà trước đây đã khác biệt ở các cấp độ phân tách trung gian. Việc nhận dạng như vậy có thể làm giảm số lượng chức năng phụ riêng biệt ở một cấp độ trong khi tăng số lượng chức năng phụ riêng biệt ở cấp độ khác và số lượng hạt không đơn điệu trong thao tác này. 

Một phản ví dụ được đưa ra bởi một hàm có bảng chân trị được cấu trúc sao cho các hàm con phụ thuộc vào$x_{k+1}$Và$x_{k+2}$khác biệt nhưng trở nên giống hệt nhau sau khi xác định$x_{k+2}=x_k$, do đó buộc phải trùng lặp các khác biệt ở cấp độ cao hơn khi được thể hiện lại ở dạng có thứ tự. Trong trường hợp như vậy, cấu trúc có thứ tự giảm phải giới thiệu lại các nút nhánh riêng biệt để duy trì tính nhất quán của thứ tự, tăng số lượng hạt so với$f$. 

Do đó tồn tại$f$vì cái gì$B(h) > B(f)$, do đó bất đẳng thức$B(h)\le B(f)$nói chung là không giữ được. ∎
