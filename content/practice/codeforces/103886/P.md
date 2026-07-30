---
title: "CF 103886P - Ngũ Cốc Núi"
description: "Một $n$-tuple $(a1,dots,an)$ được chấp nhận khi nó thỏa mãn ràng buộc luân phiên $$a1 le a2 ge a3 le a4 ge cdots .$$ Đặt $mathcal{A}n$ biểu thị tập hợp tất cả các bộ $n$-tuple nhị phân như vậy."
date: "2026-07-02T07:43:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103886
codeforces_index: "P"
codeforces_contest_name: "CerealCodes 2022 Summer Contest"
rating: 0
weight: 103886
solve_time_s: 125
verified: false
draft: false
---

[CF 103886P - Núi ngũ cốc](https://codeforces.com/problemset/problem/103886/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Một$n$-tuple$(a_1,\dots,a_n)$được chấp nhận khi nó thỏa mãn ràng buộc xen kẽ$$a_1 \le a_2 \ge a_3 \le a_4 \ge \cdots .$$Cho phép$\mathcal{A}_n$biểu thị tập hợp tất cả các nhị phân như vậy$n$-bộ dữ liệu. Bài toán yêu cầu một thuật toán không vòng lặp truy cập mọi phần tử của$\mathcal{A}_n$chính xác một lần, với các bộ dữ liệu liên tiếp khác nhau trong một lần cập nhật thành phần và với tổng số$|\mathcal{A}_n| = F_{n+2}$. 

Mục tiêu là xây dựng một quá trình truyền tải kiểu mã Gray có không gian trạng thái chính xác là họ Fibonacci của các chuỗi nhị phân xen kẽ. 

Cấu trúc chính là mỗi bộ dữ liệu hợp lệ được xác định bởi mức tăng hoặc giảm yếu ban đầu của nó và sau đó, mẫu sẽ buộc hành vi đơn điệu xen kẽ, tạo ra đệ quy Fibonacci trên các tiền tố. 

## Giải pháp 

Xác định hai họ chuỗi nhị phân được chấp nhận. 

Cho phép$U_n$là tập hợp các chuỗi có độ dài chấp nhận được$n$thỏa mãn$a_1 \le a_2 \ge a_3 \le \cdots$, và để$D_n$là tập thỏa mãn bất đẳng thức ban đầu đảo ngược$a_1 \ge a_2 \le a_3 \ge \cdots$. Phép đệ quy giữa hai họ này nắm bắt tất cả các chuỗi có thể chấp nhận được với hướng luân phiên cố định. 

Mỗi trình tự trong$U_n$hoặc bắt đầu bằng$0$hoặc$1$. 

Nếu nó bắt đầu bằng$0$, sau đó$a_2$không bị hạn chế so với bất đẳng thức thứ nhất ngoại trừ điều kiện xen kẽ làm giảm hậu tố còn lại thành một chuỗi trong$D_{n-1}$. Nếu nó bắt đầu bằng$1$, lực bất đẳng thức thứ nhất$a_2 = 1$, và hậu tố còn lại lại thuộc về$D_{n-1}$. Do đó cả hai trường hợp đều rút gọn thành phần mở rộng có cấu trúc của$D_{n-1}$, với tọa độ đầu tiên bắt buộc hoặc tự do tùy thuộc vào nhánh. 

Tương tự, mỗi dãy trong$D_n$phân chia theo bit đầu tiên của nó và cả hai nhánh giảm xuống$U_{n-1}$. Sự đối xứng này mang lại các phép truy hồi Fibonacci$$|U_n| = |U_{n-1}| + |D_{n-1}|, \qquad |D_n| = |U_{n-1}| + |D_{n-1}|,$$với điều kiện ban đầu$|U_1| = |D_1| = 2$. Do đó cả hai chuỗi đều thỏa mãn cùng một phép truy toán, cho$$|U_n| = |D_n| = F_{n+2}.$$Để tạo ra tất cả các bộ dữ liệu, hãy xây dựng một đệ quy xuất ra$U_n$bằng cách kết hợp hai danh sách được tạo đệ quy từ$D_{n-1}$, và đối xứng cho$D_n$. Cấu trúc không vòng lặp phát sinh từ việc biểu diễn mỗi trạng thái dưới dạng một con trỏ tới vị trí hiện tại trong mã hóa cấu trúc quyết định nhị phân cho dù lần so sánh cuối cùng có được thực hiện hay không.$\le$hoặc$\ge$, cùng với chỉ số hiện tại$i$. 

Xác định trạng thái$(i, t, a_1,\dots,a_i)$, Ở đâu$t \in \{U,D\}$chỉ ra hướng bất bình đẳng hiện tại. Các quy tắc chuyển đổi chỉ được xác định bởi các cập nhật cục bộ tại vị trí$i+1$. Từ một tiểu bang ở$U$, bit được chấp nhận tiếp theo$a_{i+1}$phải thỏa mãn$a_i \le a_{i+1}$, và trạng thái tiếp theo chuyển sang$D$. Từ một tiểu bang ở$D$, bit tiếp theo phải thỏa mãn$a_i \ge a_{i+1}$, và trạng thái tiếp theo chuyển sang$U$. 

Để làm cho thuật toán không lặp, hãy thay thế phân nhánh bằng bảng kế thừa được tính toán trước trên bộ ba$(t, a_i)$. Từ$a_i \in \{0,1\}$Và$t \in \{U,D\}$, chỉ có bốn trường hợp và mỗi trường hợp xác định duy nhất bit tiếp theo được phép và trạng thái tiếp theo. Do đó, thuật toán tiến hành bằng cách áp dụng lặp lại quy tắc cập nhật kích thước không đổi. 

Quá trình khởi tạo bắt đầu với cả hai trạng thái bắt đầu có thể có:$U$bắt đầu bằng$0$Và$1$, Và$D$bắt đầu bằng$0$Và$1$, nhưng chỉ những giá trị phù hợp với bất đẳng thức thứ nhất mới được giữ lại. Điều này mang lại chính xác hai trạng thái ban đầu hợp lệ tương ứng với cấp độ đầu tiên của cấu trúc Fibonacci. 

Quá trình truyền tải tiến hành bằng cách luôn mở rộng bộ dữ liệu một phần hiện tại thêm một bit theo quy tắc trạng thái, trong khi việc quay lui được loại bỏ bằng cách mã hóa cây đệ quy dưới dạng cấu trúc luồng trong đó mỗi nút lưu trữ kế thừa duy nhất của nó được xác định bởi$(t,a_i)$cấu hình. 

Vì mọi chuỗi có thể chấp nhận đều tương ứng với một đường dẫn duy nhất trong máy tự động hai trạng thái này với các chuyển đổi cục bộ xác định và vì mỗi phần mở rộng đều bảo toàn khả năng có thể chấp nhận nên thuật toán sẽ truy cập chính xác tất cả các phần tử của$\mathcal{A}_n$một lần. 

## Xác minh 

Mỗi bước duy trì ràng buộc xen kẽ vì bit kế tiếp được chọn chính xác để đáp ứng một trong hai điều kiện đó.$a_i \le a_{i+1}$hoặc$a_i \ge a_{i+1}$tùy thuộc vào trạng thái chẵn lẻ$t$. Không thể tạo ra bộ dữ liệu không hợp lệ vì không có chuyển đổi nào vi phạm ràng buộc bất đẳng thức cục bộ. 

Sự độc đáo của việc thăm viếng là do mỗi tiểu bang$(t,a_i)$xác định một bit và trạng thái tiếp theo duy nhất, do đó đồ thị truyền tải là sự kết hợp rời rạc của các đường dẫn có hướng. Vì mọi điều được chấp nhận$n$-tuple tương ứng với chính xác một chuỗi chuyển đổi trạng thái nhất quán, mỗi nút được truy cập chính xác một lần. 

Số Fibonacci theo sau sự phân rã thành hai lớp đối xứng$U_n$Và$D_n$, mỗi cái thỏa mãn cùng một phép truy hồi do phần mở rộng tiền tố gây ra, cho tổng số lượng$F_{n+2}$, khớp với bảng liệt kê đã biết cho các chuỗi nhị phân xen kẽ. 

## Ghi chú 

Cấu trúc này là một máy tự động Gray hai trạng thái có đồ thị chuyển tiếp là chuỗi Fibonacci chứ không phải là siêu khối. Việc triển khai không vòng lặp tương tự như việc tạo mã Gray trong Phần 7.2.1.1, nhưng với không gian trạng thái bị giới hạn bởi ràng buộc tính đơn điệu cục bộ sẽ thu gọn toàn bộ$2^n$khối thành đồ thị con Fibonacci.
