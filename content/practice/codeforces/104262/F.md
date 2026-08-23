---
title: "CF 104262F - Quầy bán xúc xích Plutonian"
description: "Giả sử $mathcal{S}(f)$ biểu thị tập hợp tất cả các hàm con riêng biệt của $f(x1,dots,xn)$ thu được bằng cách phân tách Shannon lặp lại đối với các biến $x1,dots,xn$, như được biểu thị trong biểu đồ hồ sơ chính."
date: "2026-07-01T21:37:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104262
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 1 (Advanced)"
rating: 0
weight: 104262
solve_time_s: 124
verified: false
draft: false
---

[CF 104262F - Quầy bán xúc xích Plutonian](https://codeforces.com/problemset/problem/104262/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$\mathcal{S}(f)$biểu thị tập hợp tất cả các hàm con riêng biệt của$f(x_1,\dots,x_n)$thu được bằng cách phân tách Shannon lặp đi lặp lại đối với các biến$x_1,\dots,x_n$, như được thể hiện trong biểu đồ hồ sơ chính. Mỗi phần tử của$\mathcal{S}(f)$tương ứng với một hạt duy nhất theo nghĩa của Phần 7.1.4, do đó tương ứng với một nút BDD duy nhất theo thứ tự biến đổi nào đó. 

Sửa một hoán vị$\pi$của${1,\dots,n}$. BDD được xây dựng theo$\pi$thu được bằng cách đánh giá các khai triển Shannon liên tiếp theo thứ tự các biến được quy định bởi$\pi$. Mỗi nút gặp phải trong quá trình này là một hàm con có dạng$$f(x_{\pi(1)}=c_1,\dots,x_{\pi(k-1)}=c_{k-1},x_{\pi(k)},\dots,x_{\pi(n)}),$$và quy tắc rút gọn xác định các hàm con giống hệt nhau. Do đó kích thước$B(f,\pi)$bằng số lượng các hàm con riêng biệt từ$\mathcal{S}(f)$có thể truy cập được dưới sự hạn chế lặp đi lặp lại theo thứ tự cố định$\pi$. 

Biểu đồ hồ sơ chính mã hóa cho từng chức năng phụ$g \in \mathcal{S}(f)$và mỗi biến$x_i$, cặp phân rã Shannon của nó$$g = g_{i,0} \;\text{on } x_i=0,\quad g = g_{i,1} \;\text{on } x_i=1,$$cùng với việc xác định thời điểm$g_{i,0} = g_{i,1}$, tương ứng với một bảng phụ hình vuông và do đó là một nút chìm hoặc nút thu gọn. Do đó, biểu đồ xác định cấu trúc chuyển tiếp có hướng trên$\mathcal{S}(f)$. 

Đối với một đơn đặt hàng cố định$\pi$, mỗi nút không kết thúc$g$được gán biến tiếp theo$x_{\pi(k)}$, và các cạnh đi ra của nó buộc phải là$(g_{\pi(k),0}, g_{\pi(k),1})$. Kích thước BDD$B(f,\pi)$do đó là tính chính yếu của việc đóng cửa${f}$dưới những chuyển đổi xác định này, với sự hợp nhất của các trạng thái giống hệt nhau. Tương đương,$B(f,\pi)$là số nút riêng biệt trong biểu đồ chu kỳ có hướng gốc thu được bằng cách mở biểu đồ cấu hình dọc theo$\pi$và thương số bằng sự bình đẳng của các hàm con. 

Kích thước tối thiểu trên tất cả các thứ tự có được bằng cách tối ưu hóa việc đóng này trên tất cả các hoán vị:$$B_{\min}(f) = \min_{\pi} B(f,\pi).$$Như vậy$B_{\min}(f)$được tính toán từ biểu đồ hồ sơ chính bằng cách chọn một hoán vị$\pi$giúp giảm thiểu số lượng các hàm con riêng biệt được tạo ra khi duyệt biểu đồ với các quyết định thay đổi được cố định theo thứ tự$\pi$và đếm các nút có thể truy cập được. 

Tương tự, kích thước tối đa đạt được bằng cách chọn thứ tự tối đa hóa số lượng các hàm con riêng biệt gặp phải trước khi thu gọn:$$B_{\max}(f) = \max_{\pi} B(f,\pi).$$Điều này tương ứng với việc chọn một hoán vị$\pi$buộc quá trình phân rã phải chia thường xuyên nhất có thể thành các hàm con không tương đương, do đó tối đa hóa phần có thể tiếp cận của$\mathcal{S}(f)$dưới cùng một cấu trúc đóng cửa. 

Do đó, biểu đồ hồ sơ chính xác định cả hai đại lượng bằng cách mã hóa biểu đồ phân rã đầy đủ của các hàm con;$B_{\min}(f)$Và$B_{\max}(f)$tương ứng là kích thước tối thiểu và tối đa của đồ thị con gốc thu được bằng cách áp đặt thứ tự tuyến tính trên các biến và mở biểu đồ tương ứng. Điều này hoàn thành nguyên tắc tính toán cho cả hai cực trị. ∎
