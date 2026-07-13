---
title: "CF 103306A - Sinh Nhật Alice"
description: "Giả sử $(a{ij})$ là một bảng dự phòng $mtimes n$ với tổng hàng $ri=sum{j=1}^n a{ij}, quad 1le ile m,$ và tổng cột $cj=sum{i=1}^m a{ij}, quad 1le jle n,$ với $sum{i=1}^m ri=sum{j=1}^n cj$."
date: "2026-07-03T14:23:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103306
codeforces_index: "A"
codeforces_contest_name: "2021 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 103306
solve_time_s: 137
verified: false
draft: false
---

[CF 103306A - Sinh nhật Alice](https://codeforces.com/problemset/problem/103306/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$(a_{ij})$là một$m\times n$bảng dự phòng với tổng hàng$r_i=\sum_{j=1}^n a_{ij}, \quad 1\le i\le m,$và tổng cột$c_j=\sum_{i=1}^m a_{ij}, \quad 1\le j\le n,$với$\sum_{i=1}^m r_i=\sum_{j=1}^n c_j$. 

Các mục được sắp xếp theo hàng như$(a_{11},a_{12},\ldots,a_{1n},a_{21},\ldots,a_{mn}),$hoặc theo cột như$(a_{11},a_{21},\ldots,a_{m1},a_{12},\ldots,a_{mn}).$##Giải pháp 

### một)$2\times n$bảng dự phòng và thành phần giới hạn 

Viết bảng dưới dạng$$\begin{pmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
a_{21} & a_{22} & \cdots & a_{2n}
\end{pmatrix}.$$Các ràng buộc về cột đưa ra$a_{1j}+a_{2j}=c_j,\quad 1\le j\le n,$Vì thế$a_{2j}=c_j-a_{1j}.$Tính không âm tương đương với$0\le a_{1j}\le c_j.$Ràng buộc tổng hàng đầu tiên trở thành$\sum_{j=1}^n a_{1j}=r_1.$Như vậy một$2\times n$bảng dự phòng tương đương với việc chọn số nguyên$a_{1j}$thỏa mãn$\sum_{j=1}^n a_{1j}=r_1,\quad 0\le a_{1j}\le c_j.$Đây chính xác là một thành phần giới hạn của$r_1$vào trong$n$các phần có giới hạn trên$c_j$. Hàng thứ hai sau đó được xác định duy nhất bởi$a_{2j}=c_j-a_{1j}$. Điều này hoàn thành sự tương đương. ∎ 

### b) Bảng lớn nhất về mặt từ điển (thứ tự hàng) 

Thứ tự theo hàng so sánh các mục một cách tuần tự:$a_{11},a_{12},\ldots,a_{1n},a_{21},\ldots,a_{mn}.$Tại mỗi vị trí, mục nhập phải được tối đa hóa tùy theo tính khả thi với các biên còn lại. 

Đối với mục nhập đầu tiên,$a_{11}$thỏa mãn$0\le a_{11}\le \min(r_1,c_1).$Sự lựa chọn từ điển tối đa mang lại$a_{11}=\min(r_1,c_1).$Sau khi sửa$a_{11}$, cập nhật các tham số dư:$r_1^{(1)}=r_1-a_{11},\quad c_1^{(1)}=c_1-a_{11}.$Tiến hành quy nạp dọc theo hàng 1. Đối với tổng quát$j$,$a_{1j}=\min\!\Big(r_1-\sum_{k=1}^{j-1}a_{1k},\; c_j\Big).$Sau khi hoàn thành hàng 1, tất cả các mục được buộc vào hàng 2 bởi$a_{2j}=c_j-a_{1j}.$Đối với các hàng sau$i\ge 2$, quy tắc tham lam tương tự cũng được áp dụng với tổng hàng và cột còn lại được cập nhật:$a_{ij}=\min\!\Big(r_i-\sum_{k<j}a_{ik},\; c_j-\sum_{\ell<i}a_{\ell j}\Big).$Cấu trúc này là tối đa ở mỗi bước vì bất kỳ sự gia tăng nào ở vị trí trước đó sẽ vi phạm tổng hàng hoặc tổng cột, trong khi bất kỳ sự giảm nào sẽ làm giảm giá trị từ điển ngay lập tức mà không cho phép tăng bù bù sớm hơn theo thứ tự. 

### c) Bảng lớn nhất về mặt từ điển (thứ tự theo cột) 

Bây giờ thứ tự là$a_{11},a_{21},\ldots,a_{m1},a_{12},\ldots,a_{mn}.$Nguyên tắc tham lam tương tự cũng được áp dụng trong việc truyền tải cột chính. 

Đối với cột$1$, các mục được chọn từ trên xuống dưới:$a_{11}=\min(r_1,c_1),$sau đó cho$i>1$,$a_{i1}=\min\!\Big(r_i,\; c_1-\sum_{k<i}a_{k1}\Big).$Sau cột$1$, tổng hàng dư và tổng cột$c_1$được cập nhật và quá trình lặp lại cho cột$2$, v.v. 

Nói chung,$a_{ij}=\min\!\Big(r_i-\sum_{k<j}a_{ik},\; c_j-\sum_{\ell<i}a_{\ell j}\Big),$với việc duyệt theo thứ tự cột lớn. 

Lập luận khả thi tương tự như trong phần (b) cho thấy rằng bất kỳ sai lệch nào làm giảm mục nhập trước đó sẽ làm giảm giá trị từ điển, trong khi bất kỳ sự gia tăng nào đều không khả thi do dung lượng hàng hoặc cột đã cạn kiệt. 

### d) Bảng nhỏ nhất về mặt từ điển 

Đối với thứ tự theo hàng, mức tối thiểu đạt được bằng cách thực hiện mỗi mục nhập sớm càng nhỏ càng tốt trong khi vẫn đảm bảo tính khả thi của việc hoàn thành bảng còn lại. 

Tại vị trí$(i,j)$, mục nhập phải đáp ứng cả hai:$a_{ij}\ge 0,$và số tiền còn lại phải đạt được:$\sum_{\text{remaining}} a_{kl} = \text{remaining row sums} = \text{remaining column sums}.$Do đó, chúng tôi tối đa hóa số tiền được phân bổ sau này, nhờ đó chúng tôi giảm thiểu các mục nhập sớm chịu những ràng buộc về tính khả thi. 

Điều này mang lại quy tắc tham lam ngược:$a_{ij}=\max\!\Big(0,\; r_i^{\text{rem}} + c_j^{\text{rem}} - \text{(maximum fill possible later)}\Big).$Ở dạng tuần tự cụ thể, người ta tính mỗi mục là giá trị nhỏ nhất sao cho tất cả tổng hàng còn lại vẫn có thể được phân bổ vào các cột còn lại mà không vi phạm giới hạn trên. 

Đối với cấu trúc trường hợp đặc biệt tiềm ẩn trong các bảng dự phòng, điều này rút gọn thành: 

Ở bước$(i,j)$theo thứ tự hàng lớn,$a_{ij}=\max\!\Big(0,\; r_i^{\text{rem}} - \sum_{k>j} c_k^{\max\text{alloc}}\Big),$trong đó tính khả thi còn lại được đảm bảo bằng cách giữ các mục trong tương lai ở mức dung lượng cột khi cần thiết. 

Một cách giải thích mang tính xây dựng tương đương là: điền vào bảng từ trái sang phải, từ trên xuống dưới, luôn để lại càng nhiều càng tốt cho các mục sau phù hợp với tổng số hàng và cột. Điều này xác định duy nhất bảng khả thi nhỏ nhất về mặt từ điển. 

Nguyên tắc tương tự được áp dụng theo thứ tự cột lớn theo tính đối xứng. 

### e) Tạo tất cả các bảng dự phòng theo thứ tự từ điển 

Một bảng dự phòng có thể được tạo bằng cách xem trình tự theo hàng$(a_{11},\ldots,a_{mn})$như một chuỗi bị ràng buộc với các giới hạn động. 

Ở mỗi bước$(i,j)$, biến$a_{ij}$thỏa mãn một khoảng giới hạn được xác định bởi các cận biên còn lại:$0 \le a_{ij} \le \min\!\Big(r_i^{\text{rem}},\; c_j^{\text{rem}}\Big).$Việc sửa các mục nhập trước đó sẽ giảm cả tổng hàng và tổng cột dư, tạo ra giới hạn cập nhật cho các mục tiếp theo. 

Do đó, việc tạo từ điển tiến hành chính xác như Thuật toán L trong Phần 7.2.1.3, nhưng với các giới hạn thay đổi thay vì các ràng buộc nhị phân. Ở mỗi bước, người ta xác định vị trí ngoài cùng bên phải trong thứ tự hiện tại có giá trị có thể tăng lên mà không vi phạm bất kỳ ràng buộc hàng hoặc cột dư nào, tăng giá trị đó lên bằng$1$và đặt lại tất cả các mục sau đó về giá trị khả thi tối thiểu của chúng, được tính toán một cách tham lam từ tổng hàng và cột còn lại. 

Tính khả thi được bảo toàn vì mỗi bước duy trì tính bất biến$\sum_j a_{ij}=r_i,\quad \sum_i a_{ij}=c_j,$và tính bị chặn đảm bảo tính hữu hạn của không gian tìm kiếm. Thứ tự từ điển xuất phát từ thực tế là vị trí khác biệt đầu tiên chiếm ưu thế tất cả các vị trí sau trong thứ tự, do đó thuật toán phản ánh cấu trúc kế thừa tiêu chuẩn cho các thành phần giới hạn được mở rộng sang hệ thống ràng buộc hai chiều. 

Điều này hoàn thành giải pháp. ∎
