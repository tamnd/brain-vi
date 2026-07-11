---
title: "CF 103149A - Cơn sốt mua sắm"
description: "Giả sử biểu diễn tổ hợp bậc-$t$ (57) của $N$ được viết dưới dạng $$N = binom{ct}{t} + binom{c{t-1}}{t-1} + cdots + binom{c1}{1},$$ trong đó $$s+t ct cdots c1 ge 0.$$ Đặt $$M = binom{s+t}{t} - N."
date: "2026-07-03T18:57:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103149
codeforces_index: "A"
codeforces_contest_name: "EGOI 2021 Day 2"
rating: 0
weight: 103149
solve_time_s: 151
verified: false
draft: false
---

[CF 103149A - Cơn sốt mua sắm](https://codeforces.com/problemset/problem/103149/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hãy để bằng cấp-$t$biểu diễn tổ hợp (57) của$N$được viết dưới dạng$$N = \binom{c_t}{t} + \binom{c_{t-1}}{t-1} + \cdots + \binom{c_1}{1},$$Ở đâu$$s+t > c_t > \cdots > c_1 \ge 0.$$Cho phép$$M = \binom{s+t}{t} - N.$$Phép toán bù được thực hiện trong tập hợp đầy đủ của tất cả$t$-sự kết hợp của${0,1,\dots,s+t-1}$. Mỗi học kỳ$\binom{c_j}{j}$đếm khối của$j$-tổ hợp có phần tử lớn nhất là$c_j$. Trừ$N$loại bỏ các khối này khỏi phân đoạn ban đầu đầy đủ, vì vậy$M$được xác định bởi các khối bổ sung trong cùng một hệ số tổ hợp. 

Đối với một cố định$j$, khối được tính bằng$\binom{c_j}{j}$chiếm chính xác khoảng của các chỉ số tương ứng với$j$-các kết hợp có mục nhập hàng đầu nhiều nhất$c_j$. Do đó, phần bù thay thế phần cắt bớt này bằng cách đếm tất cả$j$-các tổ hợp có mục nhập chính nằm ở${c_j+1,\dots,s+t-1}$. Bằng tính chất xác định của hệ thống số tổ hợp, sự dịch chuyển này đảo ngược thứ tự đóng góp và tạo ra các thuật ngữ hiệu chỉnh xen kẽ khi viết lại ở dạng chính tắc. 

Nhận dạng chính chi phối một mức độ dịch chuyển duy nhất là mối quan hệ kính thiên văn$$\binom{x}{j} = \binom{x-1}{j} + \binom{x-1}{j-1},$$được áp dụng nhiều lần để di chuyển các chỉ số trên từ$c_j$lên đến$s+t-1$. Lặp lại việc mở rộng này$s+t-1-c_j$lần sản lượng$$\binom{s+t-1}{j} - \binom{c_j}{j}
=
\sum_{k=0}^{s+t-2-c_j} \binom{c_j+k}{j-1}.$$Khi những phần mở rộng này được chèn vào biểu thức$$M = \binom{s+t}{t} - \sum_{j=1}^t \binom{c_j}{j},$$hệ số nhị thức đầy đủ$\binom{s+t}{t}$phân hủy thành tổng trên tất cả các lớp$j$trong biểu diễn tổ hợp. Mỗi lớp đóng góp một loạt các thuật ngữ với các dấu hiệu xen kẽ được tạo ra bằng cách sử dụng nhiều lần danh tính$$\binom{x}{r} = \binom{x-1}{r} + \binom{x-1}{r-1}.$$Sau khi thu thập các thuật ngữ ở mỗi cấp bậc cố định$j$, sự hủy bỏ xảy ra giữa các lần khai triển liên tiếp vì mỗi số hạng trung gian xuất hiện đúng hai lần trái dấu, một lần từ mức khai triển$j$và một lần từ cấp độ$j-1$. Các số hạng chưa bị hủy bỏ còn lại chính xác là số hạng thu được bằng cách xen kẽ sự đóng góp của các lớp nhị thức liên tiếp. 

Điều này tạo ra luật kết hợp xen kẽ (30), thể hiện sự biểu diễn phần bù hoàn toàn dưới dạng đóng góp nhị thức xen kẽ bắt nguồn từ các chữ số gốc$c_t,\dots,c_1$:$$M = \sum_{j=1}^t (-1)^{t-j} \, \Phi_j(c_j),$$Ở đâu$\Phi_j(c_j)$là kinh điển$j$-mức đóng góp thu được bằng cách dịch chuyển$c_j$vào phạm vi đầy đủ${0,\dots,s+t-1}$thông qua việc khai triển Pascal lặp đi lặp lại. 

Vì mỗi bước có được bằng cách lặp đi lặp lại việc áp dụng đồng nhất Pascal và hủy bỏ giữa các cấp độ liền kề nên biểu thức thu được là chính xác và duy nhất. Điều này hoàn thành việc chứng minh. ∎
