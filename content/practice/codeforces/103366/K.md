---
title: "CF 103366K - Nhiều Bé Cười Nhí Nhí"
description: "Một tổ hợp (s, t) $c4 c3 c2 c1$ với $t=4$ là một bộ 4 giảm nghiêm ngặt $$n c4 c3 c2 c1 ge 0,$$ và điều kiện $c4 - c1 < m$ tương đương với việc yêu cầu tất cả các phần tử được chọn nằm trong một khoảng có độ dài $m-1$."
date: "2026-07-03T13:00:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103366
codeforces_index: "K"
codeforces_contest_name: "2021 Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 103366
solve_time_s: 66
verified: false
draft: false
---

[CF 103366K - Nhiều đứa trẻ cười khúc khích](https://codeforces.com/problemset/problem/103366/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Sự kết hợp (s, t)$c_4 c_3 c_2 c_1$với$t=4$là một bộ 4 giảm nghiêm ngặt$$n > c_4 > c_3 > c_2 > c_1 \ge 0,$$và điều kiện$c_4 - c_1 < m$tương đương với việc yêu cầu tất cả các phần tử được chọn nằm trong một khoảng độ dài$m-1$. Tương đương, cho phép$c_1 = a$, mọi phần tử được chọn khác phải nằm trong${a+1,\dots,a+m-1}$, Và$c_4 \le a+m-1$. 

Đối với mỗi mức tối thiểu khả thi$a$, định nghĩa$$B_a = \{ \{a\} \cup X : X \subseteq \{a+1,\dots,a+m-1\},\ |X|=3 \}.$$Mọi tổ hợp 4 được chấp nhận đều thuộc về đúng một$B_a$, cụ thể là$a=c_1$, Và$B_a$không trống chính xác khi nào$a+m-1 \le n-1$, tức là$0 \le a \le n-m$. 

Trong mỗi$B_a$, vấn đề giảm xuống còn việc tạo ra tất cả 3 kết hợp của một$(m-1)$-bộ phần tử, với độ kề được cho bằng cách thay thế một phần tử, vì việc thay đổi một ngón tay tương ứng với việc thay thế chính xác một mục của tổ hợp 4. 

### Kết nối trong từng khối 

sửa chữa$a$. bộ${a+1,\dots,a+m-1}$là một khoảng kích thước$m-1$. Bằng thuật toán L ở mục 7.2.1.3 áp dụng cho$t=3$, tất cả 3 kết hợp của bộ này có thể được tạo theo thứ tự từ điển sao cho các kết hợp liên tiếp khác nhau bằng cách thay đổi chính xác một phần tử. 

Nhúng chúng vào 4 tổ hợp bằng cách nối phần tử cố định$a$bảo toàn sự kề cận. Do đó mỗi$B_a$thừa nhận một đường đi Hamilton dưới sự thay đổi một phần tử. 

Nó vẫn còn để kết nối các khối$B_a$vào một con đường toàn cầu duy nhất. 

### Liên kết các khối liên tiếp 

hãy để$a$được cố định với$a < n-m$. Trong khối$B_a$, chọn đường dẫn Hamilton từ điển gồm 3 tổ hợp của${a+1,\dots,a+m-1}$được tạo bởi Thuật toán L. Đường dẫn này bắt đầu tại$$X_{\min}(a) = \{a+1,a+2,a+3\},$$và kết thúc tại$$X_{\max}(a) = \{a+m-3,a+m-2,a+m-1\}.$$Vì vậy sự kết hợp cuối cùng trong$B_a$là$$C^{\mathrm{end}}_a = \{a,a+m-3,a+m-2,a+m-1\}.$$Trong khối$B_{a+1}$, sự kết hợp đầu tiên theo thứ tự từ điển là$$C^{\mathrm{start}}_{a+1} = \{a+1,a+2,a+3,a+4\}.$$Để kết nối$C^{\mathrm{end}}_a$ĐẾN$C^{\mathrm{start}}_{a+1}$bằng một thay đổi duy nhất, thay thế phần tử$a+m-1$TRONG$C^{\mathrm{end}}_a$qua$a+4$. Điều này có giá trị bất cứ khi nào$$a+4 \le a+m-1,$$tức là$m \ge 5$, đúng trong cả hai trường hợp$m=8$Và$m=13$. Sự kết hợp trung gian thu được là$$\{a,a+m-3,a+m-2,a+4\}.$$Sau đó thay thế$a$qua$a+1$, sản xuất$$\{a+1,a+m-3,a+m-2,a+4\}.$$Cuối cùng, trong cấu trúc từ điển của$B_{a+1}$, cấu hình này nằm trên đường đi Hamilton của$B_{a+1}$và có thể được kết nối với$C^{\mathrm{start}}_{a+1}$bởi cùng một cấu trúc chuyển tiếp phần tử đơn bên trong khối (vì cả hai đều khác nhau bởi chính xác một sự thay thế trong thành phần 3 tập hợp con). 

Như vậy sự kết thúc của$B_a$có thể được kết nối với sự bắt đầu của$B_{a+1}$thông qua một chuỗi các thay đổi bằng một ngón tay để duy trì ràng buộc$c_4-c_1<m$, bởi vì tất cả các cấu hình trung gian vẫn được chứa trong${a,\dots,a+m}$. 

### Xây dựng toàn cầu 

Bắt đầu tại khối$B_0$với thứ tự từ điển gồm 3 tổ hợp trên${1,\dots,m-1}$. Đi qua từng khối$B_a$thông qua Thuật toán L được nâng lên thành 4 tổ hợp và kết nối các khối liên tiếp bằng cách sử dụng các chuyển tiếp ranh giới được mô tả ở trên. Mỗi bước thay đổi chính xác một phần tử của tổ hợp 4. 

Mỗi cấu hình được chấp nhận nằm trong đúng một khối$B_a$, vì vậy mọi hợp âm hợp lệ đều được truy cập đúng một lần. Việc xây dựng tạo ra đường đi Hamilton trong biểu đồ gồm 4 tổ hợp khả thi dưới sự thay đổi một phần tử. 

Điều này đúng cho cả hai bộ tham số vì$m \ge 5$đảm bảo sự thay thế ranh giới$a+m-1 \mapsto a+4$luôn luôn hợp lệ và$n \ge m$đảm bảo sự tồn tại của tất cả các khối được yêu cầu. 

Điều này hoàn thành việc chứng minh. ∎
