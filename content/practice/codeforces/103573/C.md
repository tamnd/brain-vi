---
title: "CF 103573C - \u0421\u0432\u043e\u0431\u043e\u0434\u043d\u043e\u0435 \u043f\u0435\u0440\u0435\u043c\u0435\u0449\u0435\u043d\u0438\u0435"
description: "Đặt bảng chữ cái là ${x1 < x2 < cdots < xt}$ với bội số $n1,ldots,nt$ và $sum{i=1}^t ni = n$. Thuật toán L tạo ra các hoán vị theo thứ tự từ điển chặt chẽ đối với bảng chữ cái có thứ tự này."
date: "2026-07-03T03:54:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103573
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2021-2022, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103573
solve_time_s: 127
verified: false
draft: false
---

[CF 103573C - \u0421\u0432\u043e\u0431\u043e\u0434\u043d\u043e\u0435 \u043f\u0435\u0440\u0435\u043c\u0435\u0449\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/103573/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Hãy để bảng chữ cái là${x_1 < x_2 < \cdots < x_t}$với bội số$n_1,\ldots,n_t$Và$\sum_{i=1}^t n_i = n$. Thuật toán L tạo ra các hoán vị theo thứ tự từ điển chặt chẽ đối với bảng chữ cái có thứ tự này. Thứ hạng của một hoán vị$a_1 \ldots a_n$do đó là số lượng hoán vị nhiều tập riêng biệt nhỏ hơn về mặt từ điển. 

Sửa tiền tố$a_1 \ldots a_{i-1}$. Tại vị trí$i$, giả sử bội số sẵn có còn lại là$m_1,\ldots,m_t$với tổng số$m = n-i+1$. Nếu một biểu tượng$x_k$được đặt ở vị trí$i$, số lần hoàn thành là hệ số đa thức$$\frac{(m-1)!}{m_1!\cdots (m_k-1)!\cdots m_t!}.$$Tổng hợp điều này trên tất cả$x_k < a_i$đóng góp vị trí$i$lên cấp bậc. Đây chính xác là mã Lehmer đa thức tiêu chuẩn được điều chỉnh phù hợp với các ký hiệu lặp lại. 

Bây giờ hãy xem xét hoán vị$$314159265.$$Các ký hiệu có thứ tự cơ bản là$$1 < 2 < 3 < 4 < 5 < 6 < 9,$$với bội số$$n_1 = 2,\quad n_5 = 2,\quad n_2 = n_3 = n_4 = n_6 = n_9 = 1.$$Chúng tôi tính toán thứ hạng tăng dần. 

Tại$a_1 = 3$, các ký hiệu nhỏ hơn$3$là$1$Và$2$. 

Nếu như$1$được đặt đầu tiên, các bội số còn lại sẽ đếm$$\frac{8!}{2!\,1!\,1!\,2!\,1!\,1!} = \frac{40320}{2} = 20160.$$Nếu như$2$được đặt đầu tiên, mẫu số tương tự sẽ xảy ra, cho ra một mẫu số khác$20160$. Do đó vị trí đầu tiên góp phần$40320$. 

Sau khi sửa$3$, multiset còn lại là$$\{1^2,2,4,5^2,6,9\}.$$Tại$a_2 = 1$, không có ký hiệu nào nhỏ hơn$1$, vậy phần đóng góp là$0$. 

Tại$a_3 = 4$, các ký hiệu còn lại nhỏ hơn$4$là$1$Và$2$. 

Nếu như$1$được đặt, multiset còn lại có kích thước$6$chỉ với một bản sao$5$, cho$$\frac{6!}{2!} = 360.$$Nếu như$2$được đặt, giá trị tương tự xảy ra, do đó sự đóng góp là$720$. 

Tại$a_4 = 1$, không có sự đóng góp nào phát sinh. 

Sau khi xử lý$a_5 = 5$, multiset còn lại là${2,6,9}$cùng với một bổ sung$5$đã được tính đến và các ký hiệu nhỏ hơn$5$là$1$Và$2$. 

Nếu như$1$được đặt, ba ký hiệu còn lại là khác biệt, góp phần$3! = 6$. 

Nếu như$2$được đặt, giá trị tương tự xảy ra, đóng góp$12$, do đó tổng cộng$48$. 

Tại$a_6 = 9$, các ký hiệu nhỏ hơn$9$trong số nhiều bộ còn lại${2,5,6,9}$là$2$,$5$, Và$6$. 

Mỗi lựa chọn để lại ba biểu tượng riêng biệt, góp phần$3! = 6$, do đó tổng cộng$18$. 

Tại$a_7 = 2$, không có biểu tượng nào nhỏ hơn có sẵn đóng góp. 

Tại$a_8 = 6$, biểu tượng nhỏ hơn duy nhất có sẵn là$5$và việc đặt nó để lại một lần hoàn thành duy nhất, đóng góp$1$. 

Tại$a_9 = 5$, không có sự đóng góp nào nữa xảy ra. 

Tổng hợp mọi đóng góp$$40320 + 720 + 48 + 18 + 1 = 41107.$$Thứ hạng của$314159265$do đó theo Thuật toán L là$$\boxed{41107}.$$
