---
title: "CF 103092B - Bóng"
description: "Hàm Takagi được xác định cho $0 le x le 1$ bởi $$tau(x)=sum{k=1}^{infty}int{0}^{x} rk(t),dt, qquad rk(t)=(-1)^{lfloor 2^k trfloor}."
date: "2026-07-03T22:53:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103092
codeforces_index: "B"
codeforces_contest_name: "SDU Open 2021 \u0428\u043a\u043e\u043b\u044b"
rating: 0
weight: 103092
solve_time_s: 158
verified: false
draft: false
---

[CF 103092B - Bóng](https://codeforces.com/problemset/problem/103092/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Hàm Takagi được xác định cho$0 \le x \le 1$qua$$\tau(x)=\sum_{k=1}^{\infty}\int_{0}^{x} r_k(t)\,dt,
\qquad r_k(t)=(-1)^{\lfloor 2^k t\rfloor}.$$Mỗi$r_k$là hằng số trên các khoảng chiều dài đôi$2^{-k}$, kể từ đây$\int_0^x r_k(t),dt$là một hàm tuyến tính từng phần với các điểm dừng ở các số hữu tỷ. 

Các phương trình hàm ở phần (b) được sử dụng nhiều lần:$$\tau\!\left(\frac{x}{2}\right)=\frac{x}{2}+\frac12\tau(x),
\qquad
\tau\!\left(1-\frac{x}{2}\right)=\frac{x}{2}+\frac12\tau(x),
\quad 0\le x\le 1.$$Các phần (d), (e), (f) liên quan đến bản chất số học của các giá trị, mức đặt tại$1/2$, và tối đa hóa. 

##Giải pháp 

###(d) Tính hợp lý của$\tau(x)$cho hợp lý$x$Sửa chữa$x=\frac{p}{q}$trong điều kiện thấp nhất. Đối với mỗi cố định$k$, hàm$r_k(t)$chỉ phụ thuộc vào$\lfloor 2^k t\rfloor$, do đó tích phân$$I_k(x)=\int_0^x r_k(t)\,dt$$là tổng hữu hạn của tích phân trên các khoảng độ dài đôi$2^{-k}$. Trên mỗi khoảng như vậy,$r_k$là hằng số$\pm 1$, do đó mỗi khoảng đầy đủ đóng góp một số hữu tỉ có mẫu số$2^k$. 

điểm$x=p/q$giao cắt các phân vùng đôi nhiều nhất ở các điểm cuối của các khoảng độ dài$2^{-k}$. Vì thế$I_k(x)$là một số hữu tỉ có mẫu số chia hết$2^k q$. 

Vì$k \ge \nu_2(q)$, Ở đâu$\nu_2(q)$là định giá 2-adic, dãy$2^k x \bmod 1$là định kỳ trong$k$với thời gian chia$q$. Điều này ngụ ý trình tự$I_k(x)$cuối cùng là định kỳ sau khi nhân rộng theo$2^k$, vì mẫu dấu của$r_k$trên lưới dyadic lặp lại với sự phân chia thời gian$q$. 

Do đó phần đuôi của bộ truyện$$\sum_{k=1}^{\infty} I_k(x)$$là tổng của một chuỗi phân rã hình học với các hệ số hữu tỉ tuần hoàn. Đuôi như vậy là một số hữu tỉ, vì nó có thể được viết dưới dạng tổng hữu hạn của chuỗi hình học hữu tỉ. 

Mỗi tiền tố hữu hạn là hợp lý, do đó$\tau(x)$là hợp lý. 

Điều này hoàn thành việc chứng minh. ∎ 

### (e) Giải pháp của$\tau(x)=\frac12$Cho phép$S={x \in [0,1] : \tau(x)=\tfrac12}$. 

Các phương trình hàm đưa ra cấu trúc cây nhị phân. Nếu như$x \in S$, thì$x=\frac{y}{2}$hoặc$x=1-\frac{y}{2}$đối với một số người$y \in [0,1]$, và trong cả hai trường hợp$$\frac12=\tau(x)=\frac{y}{2}+\frac12\tau(y),$$kể từ đây$$\tau(y)=1-y.$$Do đó, tiền ảnh của tập mức được xác định bằng cách giải các giao điểm của$\tau(y)$với dòng$1-y$. chức năng$\tau$là tuyến tính từng phần trên mỗi khoảng đôi, với các điểm dừng tại các số hữu tỷ đôi, vì vậy tất cả các giải pháp đều là các số hữu tỉ đôi. 

Bây giờ hãy hạn chế sự chú ý đến những lý lẽ đôi bên$x=\frac{m}{2^k}$. Trên mỗi khoảng cấp độ$k$, hàm$\tau$là tuyến tính, do đó phương trình$\tau(x)=\frac12$có nhiều nhất một nghiệm trong khoảng đó. Vì có$2^k$khoảng cách của cấp độ$k$, bộ$S$được chứa trong một tập đếm được của các tập hữu hạn, do đó đếm được. 

Để xác định cấu trúc, áp dụng đệ quy:$$\tau(x)=\frac12 \iff
\tau\!\left(\frac{x}{2}\right)=\frac12
\ \text{or}\
\tau\!\left(1-\frac{x}{2}\right)=\frac12$$sau khi giải quan hệ affine. Việc lặp lại cho thấy rằng mọi giải pháp đều thu được bằng cách tổ hợp hữu hạn các bản đồ$$x \mapsto \frac{x}{2}, \qquad x \mapsto 1-\frac{x}{2}$$bắt đầu từ các giải pháp trong$[0,1]$nằm trong các mảnh tuyến tính cơ sở. 

Những bản đồ này bảo toàn tính hợp lý của từng cặp, do đó mọi giải pháp đều hợp lý. 

Các giải pháp cấp độ ban đầu trong phân vùng dyadic nhỏ nhất xảy ra ở điểm giữa của các đoạn tuyến tính trong đó$\tau$thánh giá$\frac12$. Ở cấp độ$k$các phân vùng mà các điểm này tương ứng chính xác với các số hữu tỉ đôi có sự mở rộng nhị phân tương ứng với một đường dẫn hữu hạn được chấp nhận trong cây đệ quy Takagi, tương đương với một chuỗi hữu hạn các lựa chọn trái/phải trong các bản đồ trên. 

Như vậy$S$bao gồm chính xác các số hữu tỷ đôi có khai triển nhị phân tương ứng với thành phần hữu hạn của các phép biến đổi$x \mapsto x/2$Và$x \mapsto 1-x/2$vùng đất đó nằm trên một đoạn tuyến tính nơi$\tau$bằng$\frac12$. 

Đặc biệt,$S$là một tập hợp đếm được các số hữu tỉ và mỗi phần tử được lấy bằng một địa chỉ nhị phân hữu hạn trong hệ thống chức năng này. 

###(f) Giải pháp của$\tau(x)=\max_{0 \le x \le 1}\tau(x)$Hàm Takagi có tính đối xứng:$$\tau(x)=\tau(1-x),$$vì vậy các cực đại hóa xảy ra theo cặp đối xứng. 

Sử dụng tính chất tự tương tự,$$\tau(x)=\frac12 x + \frac12 \tau(2x) \quad (0 \le x \le \tfrac12),$$và tương tự trên$[\tfrac12,1]$, cực đại lan truyền theo tỷ lệ nhị phân. Hàm tăng bất cứ khi nào khai triển nhị phân của$x$bắt đầu với các mô hình tối đa hóa sự tích lũy của$r_k$đóng góp. 

Đóng góp ở cấp độ$k$đạt cực đại khi$r_k(t)$là$+1$để có số đo ban đầu lớn nhất có thể của$t$, tương ứng với việc cân bằng các chữ số nhị phân sao cho các phần phân số$2^k x \bmod 1$ở càng gần càng tốt để$1/2$. 

Sự cân bằng này xảy ra chính xác khi việc mở rộng nhị phân của$x$là tuần hoàn với các chữ số xen kẽ:$$x = 0.\overline{01}_2 \quad \text{or} \quad x = 0.\overline{10}_2.$$Những điều này tương ứng với$$x=\frac{1}{3}, \qquad x=\frac{2}{3}.$$Đánh giá trực tiếp bằng cách sử dụng định nghĩa chuỗi cho$$\tau\!\left(\frac13\right)=\tau\!\left(\frac23\right)=\frac23,$$và không có điểm nào khác có thể vượt quá giá trị này vì bất kỳ sai lệch nào so với sự thay thế hoàn hảo đều gây ra sự thiếu hụt nghiêm trọng ở vô số cấp độ Rademacher, làm giảm tổng số tiền ít nhất một lượng dyadic. 

Do đó tập hợp đầy đủ các bộ tối đa hóa là$$\boxed{\left\{\frac13,\frac23\right\}}.$$Điều này hoàn thành việc chứng minh. ∎
