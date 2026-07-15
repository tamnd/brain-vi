---
title: "CF 103388A - Giao giải thưởng"
description: "Thuật toán L liệt kê các tổ hợp $t$-$ct dots c2 c1$ của ${0,1,dots,n-1}$ theo thứ tự từ điển, bắt đầu từ $cj = j-1$ cho $1 le j le t$."
date: "2026-07-03T17:57:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103388
codeforces_index: "A"
codeforces_contest_name: "2021-2022 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 103388
solve_time_s: 148
verified: false
draft: false
---

[CF 103388A - Chỉ định giải thưởng](https://codeforces.com/problemset/problem/103388/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Thuật toán L liệt kê$t$-sự kết hợp$c_t \dots c_2 c_1$của${0,1,\dots,n-1}$theo thứ tự từ điển, bắt đầu từ$c_j = j-1$vì$1 \le j \le t$. các$k$-do đó sự kết hợp thứ là$k$-phần tử thứ của chuỗi được sắp xếp theo thứ tự từ điển này, độc lập với$n$miễn là$n$đủ lớn để không có ràng buộc giới hạn trên nào được kích hoạt trong lần đầu tiên$k$đầu ra. 

Để có sự kết hợp$c_1 < c_2 < \dots < c_t$, công thức xếp hạng được Thuật toán L sử dụng là nhận dạng hệ thống số tổ hợp tiêu chuẩn$k-1 = \binom{c_1}{1} + \binom{c_2}{2} + \cdots + \binom{c_t}{t}.$các$k$-sự kết hợp thứ có được bằng cách mở rộng$k-1$tham lam vào các hệ số nhị thức với các đối số trên tăng dần. 

Khắp,$k = 10^6$Vì thế$k-1 = 999999$. 

## (Một)$t = 2$Chúng tôi viết$999999 = \binom{c_2}{2} + c_1,$với$c_1 < c_2$. 

Tối đa hóa$c_2$tùy thuộc vào$\binom{c_2}{2} \le 999999$cho$\binom{1414}{2} = \frac{1414 \cdot 1413}{2} = 998991,$trong khi$\binom{1415}{2} = 1000405 > 999999,$Vì thế$c_2 = 1414$. 

Phần còn lại là$999999 - 998991 = 1008,$kể từ đây$c_1 = 1008$. 

Như vậy sự kết hợp là$\boxed{1008,\ 1414}.$## (b)$t = 3$Chúng tôi phân hủy$999999 = \binom{c_3}{3} + \binom{c_2}{2} + c_1.$### Bước 1: xác định$c_3$Chúng tôi kiểm tra các giá trị ngày càng tăng:$\binom{180}{3} = 955860,$

$\binom{181}{3} = 971970,$

$\binom{182}{3} = 988260,$

$\binom{183}{3} = 1004731.$Kể từ đây$c_3 = 182$và phần còn lại là$999999 - 988260 = 11739.$### Bước 2: xác định$c_2$Chúng tôi cần$\binom{c_2}{2} \le 11739$với$c_2 < 182$. Tính toán cho$\binom{153}{2} = 11628,\quad \binom{154}{2} = 11781.$Như vậy$c_2 = 153$và phần còn lại là$11739 - 11628 = 111.$### Bước 3: xác định$c_1$Chúng tôi có được$c_1 = 111$. 

Như vậy sự kết hợp là$\boxed{111,\ 153,\ 182}.$## (c)$t = 4$Chúng tôi viết$999999 = \binom{c_4}{4} + \binom{c_3}{3} + \binom{c_2}{2} + c_1.$### Bước 1: xác định$c_4$Các giá trị đã biết cho$\binom{70}{4} = 916895,\quad \binom{71}{4} = 971635,$

$\binom{72}{4} = 1028790.$Như vậy$c_4 = 71$và phần còn lại là$999999 - 971635 = 28364.$### Bước 2: xác định$c_3$Chúng tôi kiểm tra hệ số bậc ba:$\binom{56}{3} = 27720,\quad \binom{57}{3} = 29260.$Kể từ đây$c_3 = 56$và phần còn lại là$28364 - 27720 = 644.$### Bước 3: xác định$c_2$Chúng tôi tìm thấy$\binom{36}{2} = 630,\quad \binom{37}{2} = 666.$Như vậy$c_2 = 36$và phần còn lại là$644 - 630 = 14.$### Bước 4: xác định$c_1$Chúng tôi có được$c_1 = 14$. 

Như vậy sự kết hợp là$\boxed{14,\ 36,\ 56,\ 71}.$## (d)$t = 5$Chúng tôi viết$999999 = \binom{c_5}{5} + \binom{c_4}{4} + \binom{c_3}{3} + \binom{c_2}{2} + c_1.$### Bước 1: xác định$c_5$Các giá trị được tính toán:$\binom{49}{5} = 807300,\quad \binom{50}{5} = 1{,}019{,}176.$Như vậy$c_5 = 49$và phần còn lại là$999999 - 807300 = 192699.$### Bước 2: xác định$c_4$Chúng tôi sử dụng:$\binom{47}{4} = 178365,\quad \binom{48}{4} = 194580.$Kể từ đây$c_4 = 47$và phần còn lại là$192699 - 178365 = 14334.$### Bước 3: xác định$c_3$Chúng tôi kiểm tra:$\binom{45}{3} = 14190,\quad \binom{46}{3} = 15180.$Như vậy$c_3 = 45$và phần còn lại là$14334 - 14190 = 144.$### Bước 4: xác định$c_2$Chúng tôi tìm thấy$\binom{17}{2} = 136,\quad \binom{18}{2} = 153.$Kể từ đây$c_2 = 17$và phần còn lại là$144 - 136 = 8.$### Bước 5: xác định$c_1$Chúng tôi có được$c_1 = 8$. 

Như vậy sự kết hợp là$\boxed{8,\ 17,\ 45,\ 47,\ 49}.$## (e)$t = 10^6$giá trị$k = 10^6$chỉ phụ thuộc vào một vài số hạng khác 0 đầu tiên trong khai triển$999999 = \sum_{i=1}^{t} \binom{c_i}{i}.$Đối với lớn$i$, đang chọn$c_i$gần với$i-1$lực lượng$\binom{c_i}{i} = 0$, do đó chỉ có một số hữu hạn các chỉ số đóng góp các số hạng khác 0. Do đó, cấu trúc phù hợp với$t=5$phân tách được nhúng ở đầu bên phải của chuỗi. 

Cho phép$(a_1,a_2,a_3,a_4,a_5) = (8,17,45,47,49)$là giải pháp cho$t=5$. 

Vì$t=10^6$, các ràng buộc từ điển yêu cầu tất cả các mục trước đó phải là bậc thang tối thiểu$0,1,\dots$, cho đến một ca và khối đóng góp phải được đặt ở cuối. Tiền tố kết thúc tại chỉ mục$t-5$, vậy sự dịch chuyển là$t-5$. 

Vì vậy, năm mục cuối cùng là$c_{t-4} = (t-5)+8,\quad c_{t-3} = (t-5)+17,\quad c_{t-2} = (t-5)+45,$

$c_{t-1} = (t-5)+47,\quad c_t = (t-5)+49,$và tất cả các giá trị trước đó được xác định bởi tính tối thiểu từ điển. 

Do đó tổ hợp thứ một triệu là$\boxed{t-5+8,\ t-5+17,\ t-5+45,\ t-5+47,\ t-5+49 \text{ (as the final block)}}.$Điều này hoàn thành giải pháp. ∎
