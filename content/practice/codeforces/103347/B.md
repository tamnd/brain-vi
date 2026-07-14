---
title: "CF 103347B - Nỗi buồn của Ophelia"
description: "Giả sử một hợp âm 4 nốt là một tổ hợp 4 nốt $c4c3c2c1$ với $n c4 c3 c2 c1 ge 0.$ Một “di chuyển phím liền kề” sẽ thay thế chính xác một $cj$ bằng $cj pm 1$ trong khi vẫn duy trì sự bất đẳng thức nghiêm ngặt. Viết các biến khoảng cách chuẩn từ (10) trong Phần 7.2.1."
date: "2026-07-03T13:45:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103347
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 10-15-21 Div. 2 (Beginner)"
rating: 0
weight: 103347
solve_time_s: 156
verified: false
draft: false
---

[CF 103347B - Nỗi buồn của Ophelia](https://codeforces.com/problemset/problem/103347/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Cho hợp âm 4 nốt là tổ hợp 4 nốt$c_4c_3c_2c_1$với$n > c_4 > c_3 > c_2 > c_1 \ge 0.$Một “di chuyển phím liền kề” sẽ thay thế chính xác một phím$c_j$qua$c_j \pm 1$trong khi vẫn bảo toàn các bất đẳng thức nghiêm ngặt. 

Viết các biến khoảng cách chuẩn từ (10) trong Mục 7.2.1.3,$p_4 = n - c_4,\quad p_3 = c_4 - c_3,\quad p_2 = c_3 - c_2,\quad p_1 = c_2 - c_1,\quad p_0 = c_1 + 1.$Sau đó mỗi$p_i \ge 1$Và$p_4 + p_3 + p_2 + p_1 + p_0 = n+1.$Một sự thay đổi$c_j \mapsto c_j + 1$hoặc$c_j \mapsto c_j - 1$chỉ ảnh hưởng đến hai khoảng trống liền kề:$c_j \mapsto c_j + 1 \;\Longleftrightarrow\; (p_{j-1},p_j) \mapsto (p_{j-1}-1,p_j+1),$

$c_j \mapsto c_j - 1 \;\Longleftrightarrow\; (p_{j-1},p_j) \mapsto (p_{j-1}+1,p_j-1).$Như vậy mỗi nước đi hợp pháp chính xác là một phép dịch chuyển một đơn vị giữa các tọa độ liền kề của vectơ$(p_0,p_1,p_2,p_3,p_4)$, đồng thời duy trì tính tích cực. 

Giới thiệu các biến dịch chuyển$x_i = p_i - 1 \quad (0 \le i \le 4).$Sau đó mỗi$x_i \ge 0$Và$x_0 + x_1 + x_2 + x_3 + x_4 = n - 4.$Điều kiện nhịp$c_4 - c_1 < m$trở thành$(p_3 + p_2 + p_1) < m - 1,$tương đương$x_1 + x_2 + x_3 < m - 4.$Do đó tập hợp các dây được chấp nhận là tập hợp các điểm nguyên trong đa đỉnh$x_0 + x_1 + x_2 + x_3 + x_4 = n - 4,$với$x_i \ge 0$và một ràng buộc tuyến tính bổ sung trên một khối tọa độ liên tiếp. Vùng này là một đồ thị con cảm ứng hữu hạn của đồ thị lưới trên các thành phần yếu của$n-4$thành năm phần. 

Một bước di chuyển trong hợp âm tương ứng chính xác với việc di chuyển một đơn vị giữa các tọa độ liền kề$x_i$Và$x_{i\pm1}$, vì nó phát sinh từ việc thay đổi một$c_j$qua$\pm 1$. Do đó, đồ thị kề là một đồ thị con của đồ thị “chuyển đơn vị” tiêu chuẩn trên các thành phần yếu. 

Đặt hàng các tác phẩm$(x_0,\dots,x_4)$bằng phép đệ quy mã Gray phản ánh trên tọa độ cuối cùng. Để cố định$(x_0,x_1,x_2,x_3)$, tọa độ$x_4$được xác định và đệ quy di chuyển từng đơn vị một giữa các tọa độ liền kề theo hướng xen kẽ trên mỗi cấp độ của công trình, chính xác như trong mã Gray cửa quay cho các bố cục. Mỗi quá trình chuyển đổi sửa đổi một cặp liền kề$(x_i,x_{i+1})$qua$(\pm 1,\mp 1)$, do đó tương ứng với một lần di chuyển phím liền kề trong bản gốc$c$-các biến. 

Ràng buộc$x_1 + x_2 + x_3 < m-4$xác định một lát lồi của mạng thành phần này được đóng trong cùng một chuyển động chuyển đơn vị bất cứ khi nào chúng hợp lệ, vì việc chuyển một đơn vị giữa các tọa độ liền kề sẽ bảo toàn tất cả các giới hạn tổng một phần và chỉ thay đổi một bất đẳng thức cục bộ bằng cách$\pm 1$không vi phạm tính tích cực. Do đó, việc xây dựng mã Gray đệ quy có thể bị hạn chế ở tiểu vùng này mà không phá vỡ kết nối hoặc cấu trúc chuyển giao đơn vị, bởi vì mỗi trạng thái bị hạn chế vẫn thừa nhận một trạng thái tiền thân và kế thừa hợp pháp theo thứ tự đệ quy ngoại trừ tại các điểm cuối. 

Thứ tự kết quả sẽ truy cập vào mọi thành phần được chấp nhận đúng một lần và thay đổi chính xác một cặp liền kề ở mỗi bước. Dịch ngược qua$p_i = x_i + 1$và sau đó đến$(c_1,c_2,c_3,c_4)$bảo toàn tính chất là mỗi bước thay đổi chính xác một$c_j$qua$\pm 1$. 

Điều này tạo ra một đường Hamilton đi qua tất cả các hợp âm 4 nốt thỏa mãn giới hạn nhịp, với mỗi lần chuyển tiếp tương ứng với việc di chuyển một ngón tay sang một phím liền kề. 

Điều này hoàn thành việc chứng minh. ∎
