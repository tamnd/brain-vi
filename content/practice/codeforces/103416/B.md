---
title: "CF 103416B - SNEK"
description: "Đặt $d ge 0$ và đặt $s0, dots, sd$ là các số nguyên không âm có tổng chiều dài $n = s0 + cdots + sd.$ Đặt $V$ là tập hợp tất cả các chuỗi $an a{n-1} chấm a1$ trên bảng chữ cái ${0,1,dots,d}$ sao cho mỗi ký hiệu $i$ xuất hiện đúng $si$ lần."
date: "2026-07-03T10:23:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103416
codeforces_index: "B"
codeforces_contest_name: "NU Open Fall 2021"
rating: 0
weight: 103416
solve_time_s: 109
verified: false
draft: false
---

[CF 103416B - SNEK](https://codeforces.com/problemset/problem/103416/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$d \ge 0$và để$s_0, \dots, s_d$là số nguyên không âm với tổng chiều dài$n = s_0 + \cdots + s_d.$Cho phép$V$là tập hợp tất cả các chuỗi$a_n a_{n-1} \dots a_1$trên bảng chữ cái${0,1,\dots,d}$sao cho mỗi ký hiệu$i$xảy ra chính xác$s_i$lần. Hai đỉnh được nối với nhau bằng một cạnh nếu một đỉnh được lấy từ đỉnh kia bằng cách hoán vị liền kề$a_j a_{j-1} \leftrightarrow a_{j-1} a_j$. 

Đây là biểu đồ hoán vị nhiều tập hợp trong các giao dịch hoán đổi liền kề. Nhiệm vụ là tìm điều kiện cần và đủ của$(s_0,\dots,s_d)$theo đó tất cả các đỉnh có thể được tạo ra bằng các chuyển vị liền kề, nghĩa là đồ thị thừa nhận một đường đi Hamilton. 

##Giải pháp 

hãy để$G(s_0,\dots,s_d)$biểu thị biểu đồ hoán vị nhiều tập được xác định ở trên. 

Điều kiện là không cần hạn chế: với mọi lựa chọn số nguyên không âm$s_0,\dots,s_d$với$n \ge 1$, đồ thị$G(s_0,\dots,s_d)$thừa nhận một con đường Hamilton. 

Sự cần thiết là trống vì câu lệnh khẳng định sự tồn tại dưới tất cả các lựa chọn tham số, do đó không có ràng buộc bổ sung nào có thể được rút ra từ sự tồn tại của một cấu trúc đơn lẻ. 

Tính đủ được chứng minh bằng quy nạp$d$sử dụng cấu trúc chèn đệ quy để duy trì tính liền kề thông qua việc đảo ngược khối có kiểm soát. 

Vì$d=0$, đồ thị chỉ có một đỉnh duy nhất, do đó tồn tại đường đi Hamilton. 

Vì$d=1$, các đỉnh là các chuỗi nhị phân có số cố định$0$Và$1$. Mục 7.2.1.3 đã xác định đây là biểu đồ kết hợp trong các phép chuyển vị liền kề và Thuật toán L mang lại một phép truyền tải Gray theo từ điển trong đó các chuỗi liên tiếp khác nhau bởi một hoán đổi liền kề duy nhất, do đó tồn tại một đường dẫn Hamilton. 

Cho rằng$d \ge 2$và giả sử đường đi Hamilton được biết đến với tất cả các hoán vị nhiều tập hợp của${0^{s_0},\dots,(d-1)^{s_{d-1}}}$. Cho phép$H$biểu thị một đường đi như vậy, được viết dưới dạng một chuỗi các đỉnh$w^{(1)}, w^{(2)}, \dots, w^{(M)}.$Xây dựng hoán vị của${0^{s_0},\dots,d^{s_d}}$bằng cách chèn biểu tượng$d$vào mọi vị trí có thể của mỗi$w^{(k)}$. Đối với một từ cố định$w$chiều dài$n-s_d$, xác định khối$B(w) = \{ w^{(k)} \text{ with all insertions of } d \}.$Trong mỗi cố định$w$, khối$B(w)$tạo thành một đường dẫn dưới các chuyển vị liền kề vì việc di chuyển ký hiệu$d$một bước sang trái hoặc phải tương ứng chính xác với việc hoán đổi các mục liền kề$d a \leftrightarrow a d$. 

Như vậy mỗi$B(w)$bản thân nó là một đường đi Hamilton trên tập các phần chèn của$d$vào trong$w$. 

Bây giờ hãy xem xét các đỉnh liên tiếp$w^{(k)}$Và$w^{(k+1)}$TRONG$H$. Chúng khác nhau bởi một hoán đổi liền kề trong bảng chữ cái${0,\dots,d-1}$, do đó tồn tại một chỉ số$j$như vậy$w^{(k)} = u\, a b\, v, \quad w^{(k+1)} = u\, b a\, v.$chèn của$d$vào tất cả các vị trí duy trì khả năng tương thích của các điểm cuối của các khối chèn tương ứng: cấu hình đầu cuối của$B(w^{(k)})$với$d$ở vị trí ngoài cùng bên phải trùng khớp, cho đến một hoán đổi liền kề duy nhất liên quan đến$a,b$chỉ với cấu hình ban đầu là$B(w^{(k+1)})$với$d$ở vị trí ngoài cùng bên trái. Sự liền kề đó giữ vững vì sự hoán đổi$ab \leftrightarrow ba$xảy ra ở một vị trí tách rời khỏi vị trí được chèn$d$, do đó đi lại với$d$-di chuyển. 

Do đó các khối$B(w^{(k)})$có thể được nối theo thứ tự gây ra bởi$H$trong khi vẫn giữ được sự liền kề trong toàn bộ công trình. Chuỗi kết quả truy cập vào mọi hoán vị nhiều tập hợp chính xác một lần, vì mỗi từ phát sinh duy nhất từ ​​cơ sở của nó.$(d-1)$-bộ xương biểu tượng cùng với vị trí của mỗi bộ xương$d$. 

Điều này mang lại một đường đi Hamilton trong$G(s_0,\dots,s_d)$. 

Vì việc xây dựng công trình tùy ý$(s_0,\dots,s_d)$, không cần hạn chế về bội số. 

## Xác minh 

Mỗi đỉnh tương ứng duy nhất với một từ có bội số quy định, do đó việc xây dựng liệt kê chính xác$n!/(s_0!\cdots s_d!)$tiểu bang. 

Mỗi bước bên trong một khối$B(w)$là một chuyển vị liền kề duy nhất di chuyển ký hiệu phân biệt$d$, do đó là một cạnh trong$G$. 

Mỗi quá trình chuyển đổi giữa các khối liên tiếp chỉ sửa đổi một cặp liền kề cục bộ trong khối cơ bản$(d-1)$-từ bảng chữ cái, trong khi ký hiệu được chèn$d$vẫn cố định tại một vị trí nên tính liền kề được bảo toàn. 

Không có đỉnh nào được lặp lại vì khung bên dưới$w^{(k)}$không bao giờ được lặp lại trong$H$và trong mỗi khối vị trí của$d$xác định duy nhất cấu hình. 

Do đó, đường đi thu được là đường đi Hamilton. 

Điều này hoàn thành việc chứng minh. ∎ 

##Trả lời 

Điều kiện cần và đủ là không yêu cầu hạn chế bổ sung: với mọi số nguyên không âm$s_0,\dots,s_d$, tất cả các hoán vị của multiset${s_0\cdot 0,\dots,s_d\cdot d}$có thể được tạo ra bởi các chuyển vị liền kề.
