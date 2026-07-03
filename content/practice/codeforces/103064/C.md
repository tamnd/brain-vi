---
title: "CF 103064C - Baba là Bạn"
description: "Giả sử $(a{ij})$ là một bảng dự phòng $mtimes n$ với tổng hàng cố định $sum{j=1}^n a{ij}=ri quad (1le ile m)$ và tổng cột $sum{i=1}^m a{ij}=cj quad (1le jle n),$ trong đó $sumi ri=sumj cj$."
date: "2026-07-04T01:05:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103064
codeforces_index: "C"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2021"
rating: 0
weight: 103064
solve_time_s: 159
verified: false
draft: false
---

[CF 103064C - Baba là bạn](https://codeforces.com/problemset/problem/103064/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$(a_{ij})$là một$m\times n$bảng dự phòng với tổng hàng cố định$\sum_{j=1}^n a_{ij}=r_i \quad (1\le i\le m)$và tổng cột$\sum_{i=1}^m a_{ij}=c_j \quad (1\le j\le n),$Ở đâu$\sum_i r_i=\sum_j c_j$. 

Cần một bước duy nhất để sửa đổi chính xác bốn mục của ma trận trong khi vẫn giữ nguyên tất cả tổng hàng và tổng cột. Do đó, bất kỳ sự chuyển đổi nào giữa các bảng liên tiếp đều phải bao gồm một thao tác ảnh hưởng đến hai hàng và hai cột, tăng hai mục lên thêm$1$và giảm hai mục bằng$1$. 

Mục tiêu là xây dựng một bảng liệt kê tất cả các bảng như vậy sao cho các bảng liên tiếp khác nhau đúng một lần di chuyển bốn mục như vậy. 

##Giải pháp 

Việc xây dựng tiến hành bằng quy nạp trên$m$. 

Vì$m=1$, bảng dự phòng được xác định duy nhất bởi tổng hàng, do đó không có gì để tạo và yêu cầu được giữ trống. 

Cho rằng$m\ge 2$và xem xét một bảng hợp lệ tùy ý$(a_{ij})$. Tách dòng cuối cùng khỏi phần còn lại của ma trận. Vì$1\le i\le m-1$, cho phép$b_{ij}=a_{ij}.$Hàng cuối cùng được xác định bởi các ràng buộc cột:$a_{mj}=c_j-\sum_{i=1}^{m-1} b_{ij}.$Vì vậy mọi sự lựa chọn đầu tiên$m-1$các hàng mang lại giá trị không âm$a_{mj}$tương ứng với chính xác một bảng dự phòng. 

Xác định tổng cột còn lại cho cột đầu tiên$m-1$hàng:$c'_j = c_j - a_{mj},$để có thể$(b_{ij})_{1\le i\le m-1,,1\le j\le n}$là một$(m-1)\times n$bảng dự phòng với tổng hàng$r_1,\dots,r_{m-1}$và tổng cột$c'_1,\dots,c'_n$. 

Do đó, không gian trạng thái của tất cả các bảng có thể được tạo bằng cách tạo ra tất cả các cặp có thể chấp nhận được bao gồm: 

cái đầu tiên$m-1$các hàng dưới dạng bảng dự phòng có lề$(r_1,\dots,r_{m-1};c'_1,\dots,c'_n)$, cùng với hàng cuối cùng được tạo ra. 

Sửa một bảng hiện tại. Quá trình chuyển đổi được xây dựng theo hai giai đoạn kết hợp. 

Một động thái đầu tiên$m-1$các hàng được thực hiện bằng cách chọn các chỉ số$i,k\in{1,\dots,m-1}$Và$j,\ell\in{1,\dots,n}$và áp dụng tiêu chuẩn$2\times2$hoạt động vận tải:$b_{ij}\leftarrow b_{ij}+1,\quad b_{i\ell}\leftarrow b_{i\ell}-1,\quad b_{k\ell}\leftarrow b_{k\ell}+1,\quad b_{kj}\leftarrow b_{kj}-1.$Điều này bảo toàn tất cả các tổng hàng và tổng cột của cột đầu tiên$m-1$hàng và thay đổi chính xác bốn mục. 

Hiệu ứng cảm ứng ở hàng cuối cùng bị ép buộc bởi sự bảo toàn cột:$a_{mj}\leftarrow a_{mj}-1,\quad a_{m\ell}\leftarrow a_{m\ell}+1,$kể từ cột$j$lúc đầu mất một đơn vị$m-1$hàng và cột$\ell$tăng một đơn vị ở đó và tổng số cột phải cố định. Không có mục nào khác ở hàng cuối cùng thay đổi. 

Như vậy đầy đủ$m\times n$bảng thay đổi chính xác trong bốn mục$b_{ij},\; b_{kj},\; a_{mj},\; a_{m\ell},$với hai lần tăng và hai lần giảm, bảo toàn tất cả các tổng hàng và cột. 

Vẫn còn phải tạo ra một trật tự thế hệ trong đó các thế hệ đầu tiên nối tiếp nhau$m-1$cấu hình hàng khác nhau bởi một giới hạn duy nhất$2\times2$di chuyển trong khi đảm bảo rằng hàng cuối cùng cảm ứng vẫn không âm trong suốt. Giả thuyết quy nạp cung cấp một thế hệ của tất cả$(m-1)\times n$các bảng dự phòng trong đó các bảng kế tiếp khác nhau một cách chính xác như vậy$2\times2$di chuyển. 

Đối với mỗi động thái như vậy trong lần đầu tiên$m-1$hàng, sự sửa đổi cảm ứng của hàng cuối cùng là sự chuyển đổi đơn vị giữa hai cột. Tính không âm được bảo toàn vì mọi chuyển động được chấp nhận trong chuỗi quy nạp chỉ xảy ra giữa các trạng thái trong đó cả hai cột bị ảnh hưởng đều có độ chùng dương ở hàng cuối cùng, vì nếu không thì cột đầu tiên tương ứng sẽ bị ảnh hưởng.$m-1$cấu hình hàng sẽ vi phạm điều kiện khả thi của cột xác định không gian trạng thái. 

Do đó, mọi chuyển đổi trong chuỗi quy nạp đều mở rộng đến một chuyển đổi hợp lệ của các bảng đầy đủ và tạo ra chính xác bốn thay đổi mục nhập. 

Điều này tạo nên một bước đi qua tất cả các bảng dự phòng trong đó các trạng thái liên tiếp khác nhau đúng bốn mục. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Mỗi quá trình chuyển đổi được hỗ trợ bởi một$2\times2$di chuyển chu kỳ trong biểu đồ tần suất lưỡng cực của hàng và cột, là hạt nhân tiêu chuẩn của hệ thống tuyến tính xác định tổng hàng và cột. Việc di chuyển như vậy sẽ bảo toàn tất cả các ràng buộc vì mỗi hàng và cột bị ảnh hưởng sẽ nhận được một mức tăng và một mức giảm. 

Hàng cuối cùng được xác định duy nhất bởi tổng các cột, do đó bất kỳ thay đổi nào ở hàng đầu tiên$m-1$các hàng buộc phải thay đổi bù đắp trong chính xác hai mục của hàng cuối cùng. Kết hợp điều này với hai thay đổi trong lần đầu tiên$m-1$các hàng mang lại chính xác bốn mục được sửa đổi. 

Cảm ứng đóng vì trường hợp cơ sở$m=1$là tầm thường và bước quy nạp duy trì tính khả thi và khả năng tiếp cận của tất cả các cấu hình. 

## Ghi chú 

Nước đi bốn mục là nước đi có chu kỳ không tầm thường tối thiểu trong biểu đồ hai bên hoàn chỉnh$K_{m,n}$; bất kỳ sự khác biệt hợp lệ nào giữa các bảng dự phòng sẽ phân hủy thành như vậy$4$-chu kỳ. Cấu trúc trên hiện thực hóa mã Gray trên mạng các luồng số nguyên này bằng cách ghép mã Gray thành phần bị chặn cho các hàng có lan truyền xác định đến hàng cuối cùng.
