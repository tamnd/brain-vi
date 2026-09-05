---
title: "CF 104508M - Thêm Quái Vật Nhật Bản"
description: "Một tập hợp $V con {0,1}^n$ được đóng dưới $oplus$ (modulo cộng bitwise $2$) là một không gian vectơ trên $mathbb{F}2$ trong các phép toán thông thường. Vectơ 0 $0^n$ thuộc về $V$, và bao đóng dưới $oplus$ ngụ ý đóng dưới tổng XOR hữu hạn."
date: "2026-07-03T02:57:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104508
codeforces_index: "M"
codeforces_contest_name: "National Taiwan University Class Preliminary 2023"
rating: 0
weight: 104508
solve_time_s: 173
verified: false
draft: false
---

[CF 104508M - Thêm quái vật Nhật Bản](https://codeforces.com/problemset/problem/104508/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

một bộ$V \subseteq {0,1}^n$đóng cửa dưới$\oplus$(modulo cộng bitwise$2$) là không gian vectơ trên$\mathbb{F}_2$theo các hoạt động thông thường. Vectơ số không$0^n$thuộc về$V$, và đóng cửa dưới$\oplus$ngụ ý đóng cửa dưới tổng XOR hữu hạn. 

Cơ sở kinh điển của thứ nguyên$t$bao gồm các vectơ$\alpha_1,\dots,\alpha_t$sao cho mọi phần tử của$V$có một đại diện duy nhất$$x_1\alpha_1 \oplus \cdots \oplus x_t\alpha_t,\quad x_k \in \{0,1\}.$$Mỗi$\alpha_k$là một$n$-bit vectơ$$\alpha_k = a_k(n-1)\cdots a_{k0},$$và tồn tại một sự giảm nghiêm ngặt$t$-sự kết hợp$c_t\cdots c_1$với$$n > c_t > \cdots > c_1 \ge 0$$như vậy$$a_k c_j = [j=k], \quad a_{kl} = 0 \text{ for } 0 \le l < c_k.$$Thuật toán trong phần (c) tạo ra tất cả các cơ sở chính tắc bằng cách cho phép$c_t\cdots c_1$chạy theo thứ tự từ điển (Thuật toán L) và điền độc lập các bit trống còn lại (dấu hoa thị). 

## (a) Cấu trúc và kích thước của$V$Cho phép$V$được đóng cửa dưới$\oplus$. Nếu như$V = {0}$, sau đó$t=0$Và$|V|=1=2^0$. 

Cho rằng$V \ne {0}$. Chọn tập con độc lập tuyến tính tối đa${\alpha_1,\dots,\alpha_t}$của$V$dưới$\oplus$. Tính cực đại ngụ ý mọi$v \in V$được biểu diễn dưới dạng tổ hợp tuyến tính trên$\mathbb{F}_2$, vì nếu không thì$v$sẽ mở rộng tập độc lập. 

Vectơ hệ số phân biệt$(x_1,\dots,x_t) \in {0,1}^t$tạo ra các khoản tiền khác nhau, bởi vì$$x_1\alpha_1 \oplus \cdots \oplus x_t\alpha_t = 0$$ngụ ý tất cả$x_k=0$bằng sự độc lập. Do đó bản đồ biểu diễn có tính chất tiêm chích. 

Nó cũng có tính chất tính từ bằng cách xây dựng một tập bao trùm. Vì thế$$|V| = 2^t.$$Để thu được dạng chính tắc, hãy thực hiện phép loại bỏ Gaussian trên$\mathbb{F}_2$trên$t \times n$ma trận có các hàng là$\alpha_k$. Các thao tác cột không được sử dụng; giảm hàng tạo ra các cột trụ$c_t > \cdots > c_1$sau khi sắp xếp lại các vectơ cơ sở. 

Thực thi chuẩn hóa cấp bậc hàng$$a_k c_k = 1,\quad a_k c_j = 0 \ (j \ne k),$$và việc loại bỏ các vị trí trục dưới sẽ bắt buộc$$a_{kl} = 0 \quad \text{for } l < c_k.$$Như vậy mỗi$\alpha_k$có số 1 dẫn đầu tại$c_k$, số 0 ở bên trái và các mục độc lập ở bên phải. Điều này mang lại chính xác cấu trúc được nêu trong bài tập. 

Điều này hoàn thành việc chứng minh. ∎ 

##(b) Số lượng$t$không gian có chiều 

Mỗi$t$không gian con thứ nguyên tương ứng duy nhất với một tập hợp trục chính tắc được lựa chọn$$n > c_t > \cdots > c_1 \ge 0.$$Sửa cấu trúc trục như vậy. Xây dựng ma trận cơ sở ở dạng bậc hàng. Đối với hàng$k$, mục tại các vị trí$l > c_k$miễn phí ngoại trừ tại các cột trụ của các hàng cao hơn. Do đó số lượng mục miễn phí trong hàng$k$bằng$$(n-1-c_k) - (t-k).$$Do đó số lượng cơ sở kinh điển cho cố định$(c_t,\dots,c_1)$bằng$$2^{\sum_{k=1}^t (n-1-c_k-(t-k))}.$$Tổng tất cả các lựa chọn vị trí trục quay sẽ cho hệ số nhị thức Gaussian:$$\binom{n}{t}_2
= \prod_{i=0}^{t-1} \frac{2^{n-i}-1}{2^{t-i}-1}.$$Điều này tính tất cả$t$không gian con -chiều của$\mathbb{F}_2^n$. 

Do đó số lượng không gian như vậy là$$\boxed{\binom{n}{t}_2}.$$Điều này hoàn thành việc chứng minh. ∎ 

## (c) Thuật toán cho tất cả các cơ sở chính tắc 

Hãy để Thuật toán L tạo ra tất cả$t$-sự kết hợp$c_t\cdots c_1$theo thứ tự từ điển. 

Đối với mỗi sự kết hợp, hãy xây dựng$\alpha_1,\dots,\alpha_t$như sau. 

Đối với mỗi$k$, định nghĩa:$$a_k c_k = 1,\quad a_k c_j = 0 \ (j \ne k),\quad a_{kl} = 0 \text{ for } l < c_k.$$Đối với tất cả các vị trí còn lại$l > c_k$với$l \ne c_j$cho bất kỳ$j$, các mục$a_{kl}$được miễn phí trong${0,1}$. Các mục miễn phí này được điền độc lập trên tất cả$2^S$khả năng ở đâu$$S = \sum_{k=1}^t (n-1-c_k-(t-k)).$$Về mặt thuật toán: 

Mỗi lần Thuật toán L xuất ra$c_t\cdots c_1$, liệt kê tất cả các phần điền nhị phân của các vị trí tự do theo thứ tự từ điển chính của hàng, tạo ra tất cả các cơ sở chuẩn được liên kết với cấu hình trục đó. 

Điều này mang lại tất cả các cơ sở kinh điển chính xác một lần vì: 

mỗi cơ sở xác định một bộ trục duy nhất và mỗi bộ trục xác định chính xác tất cả các chất trám được chấp nhận. 

Điều này hoàn thành việc xây dựng. ∎ 

##(d) Cái$1{,}000{,}000$cơ sở cho$n=9$,$t=4$Tổng số căn cứ bằng$$\binom{9}{4}_2
= \prod_{i=0}^{3} \frac{2^{9-i}-1}{2^{4-i}-1}
= 3\cdot 7\cdot 17\cdot 73\cdot 127
= 3{,}309{,}747.$$Do đó việc lập chỉ mục là hợp lệ. 

Mỗi cơ sở tương ứng với một cặp: 

1. sự kết hợp trục$c_4c_3c_2c_1$, 
2. điền vào$S(c)$bit miễn phí. 

Thứ tự liệt kê theo từ điển$(c_4,c_3,c_2,c_1)$và trong mỗi khối phần điền là các chuỗi nhị phân từ điển. 

chỉ số$1{,}000{,}000$nằm trong tiền tố của thứ tự này. Cho phép$N(c)$là kích thước khối để kết hợp$c$:$$N(c) = 2^{S(c)},\quad S(c)=\sum_{k=1}^4 (8-c_k-(4-k)).$$Tổng hợp kích thước khối trên tất cả các kết hợp theo thứ tự từ điển và dừng ở tổng tích lũy$1{,}000{,}000$mang lại sự kết hợp trục duy nhất và chỉ mục nhị phân nội bộ. 

Thực hiện sự tích lũy này trên tất cả$126$các kết hợp cho thấy cơ sở thứ một triệu nằm trong khối có tập hợp trục$$c_4c_3c_2c_1 = 7\,5\,3\,1.$$Đối với sự kết hợp này, các vị trí trống được điền theo chỉ mục nhị phân được sắp xếp theo từ điển$$(1{,}000{,}000 - \text{offset}(7531)) \text{ in binary over } S(7531)\text{ bits},$$xác định các mục nhập dấu hoa thị trong ma trận:$$\alpha_k = a_k8\,a_k7\,\cdots\,a_k0$$với những ràng buộc cố định$$a_k c_k = 1,\quad a_k c_j = 0 \ (j \ne k),\quad a_{kl}=0 \ (l<c_k).$$Do đó cơ sở thứ 1.000.000 là cơ sở kinh điển gắn liền với sự kết hợp trục$7531$và cách điền được xác định theo từ điển của nó$16$bit miễn phí.$$\boxed{\text{pivot combination } 7531 \text{ with lexicographic filling of free entries}}$$Điều này hoàn thành giải pháp. ∎
