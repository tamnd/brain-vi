---
title: "CF 104128K - NaN trong đống"
description: "Đặt $h{a,b}(x)=((ax+b)gg(n-l)) bmod 2^l$, với $ain A={amid 0<a<2^n, a text{odd}}$ và $bin B={bmid 0le b<2^{n-l}}$. Đối với các tập cố định $P$ và $Q$ gồm các số nguyên $n$-bit, hãy xác định $$I={h{a,b}(p)mid pin P},qquad J={h{a,b}(q)mid qin Q}.$$ Đặt $ $$Pr[h{a,b}(x)=h{a,b}(y)]le 2^{-l}."
date: "2026-07-02T01:45:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104128
codeforces_index: "K"
codeforces_contest_name: "The 2022 ICPC Asia Nanjing Regional Contest"
rating: 0
weight: 104128
solve_time_s: 115
verified: false
draft: false
---

[CF 104128K - NaN trong đống](https://codeforces.com/problemset/problem/104128/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$h_{a,b}(x)=((ax+b)\gg(n-l)) \bmod 2^l$, với$a\in A={a\mid 0<a<2^n,\ a\ \text{odd}}$Và$b\in B={b\mid 0\le b<2^{n-l}}$. Đối với bộ cố định$P$Và$Q$của$n$-số nguyên bit, xác định$$I=\{h_{a,b}(p)\mid p\in P\},\qquad J=\{h_{a,b}(q)\mid q\in Q\}.$$Cho phép$|P|=|Q|=2^t$, như trong phần thiết lập Định lý X từ Mục 7.1.4 và Bài tập 6.4-78. Thuộc tính băm phổ quát ngụ ý rằng để phân biệt$x,y$,$$\Pr[h_{a,b}(x)=h_{a,b}(y)]\le 2^{-l}.$$Mục đích là để thiết lập sự tồn tại của$(a,b)$và sau đó xây dựng một tập hợp con có cấu trúc$Q^*\subseteq Q$thỏa mãn điều kiện phù hợp (120) trong Định lý X. 

## Giải pháp 

### (a) Sự tồn tại của hàm băm tốt 

Để cố định$(a,b)$, cho phép$X_P$biểu thị số lượng va chạm có thứ tự trong$P$:$$X_P=\#\{(p,p')\in P^2\mid p<p',\ h(p)=h(p')\}.$$Đối với mỗi cặp$(p,p')$, biến chỉ thị$\mathbf{1}[h(p)=h(p')]$có kỳ vọng nhiều nhất$2^{-l}$. Tổng hợp tất cả các cặp,$$\mathbb{E}[X_P]\le \binom{|P|}{2}2^{-l},\qquad \mathbb{E}[X_Q]\le \binom{|Q|}{2}2^{-l}.$$Từ$|P|=|Q|=2^t$,$$\mathbb{E}[X_P+X_Q]\le 2\binom{2^t}{2}2^{-l} < 2^{2t-l}.$$một cặp$(a,b)$có thể được cố định sao cho đồng thời$$X_P+X_Q \le 2^{2t-l}.$$Đối với sự lựa chọn như vậy, nhiều nhất$2^{2t-l}$xung đột xảy ra trong mỗi bộ ảnh, do đó ít nhất$$|P|-2^{2t-l},\qquad |Q|-2^{2t-l}$$các phần tử tham gia vào các lớp không va chạm. 

Mỗi lớp không xung đột đóng góp một giá trị riêng biệt cho$I$hoặc$J$, Vì thế$$|I|\ge |P|-2^{2t-l},\qquad |J|\ge |Q|-2^{2t-l}.$$giả thuyết$$2^l-1 \le \frac{2^{t-1}\varepsilon}{1-\varepsilon}$$ngụ ý sau khi sắp xếp lại rằng$$2^{2t-l}\le \varepsilon 2^l.$$Như vậy$$|I|\ge (1-\varepsilon)2^l,\qquad |J|\ge (1-\varepsilon)2^l.$$Điều này hoàn thành phần (a). 

### (b) Cấu trúc và tính nội xạ của$g$TRÊN$Q''$Cho phép$J={j_1,\dots,j_{|J|}}$với$0=j_1<\cdots<j_{|J|}<2^l$. Chọn$Q'={q_1,\dots,q_{|J|}}\subseteq Q$như vậy$h(q_k)=j_k$. 

Định nghĩa$$g(q)=(aq\gg(n-l+1))\bmod 2^{l-1},$$ở giữa$l-1$bit của$aq$. 

Cho phép$$Q''=\{q_1,q_3,\dots,q_{2\lceil |J|/2\rceil-1}\}.$$Nếu như$q_i,q_j\in Q''$với$i<j$, sau đó$h(q_i)\ne h(q_j)$và sự lãnh đạo của họ$l$-bit hình ảnh khác nhau. Việc xác định cắt ngắn$g$loại bỏ bit ít quan trọng nhất của$h(q)$cùng với thông tin mang ngăn cách các nhóm liền kề. Việc đặt hàng$j_1<\cdots<j_{|J|}$đảm bảo rằng các phần tử có chỉ số lẻ riêng biệt của$J$nằm trong các khoảng dư lượng rời rạc modulo$2^{l-1}$. 

Nếu như$g(q_i)=g(q_j)$, sau đó$h(q_i)$Và$h(q_j)$đồng ý ở trên$l-1$bit, do đó chỉ khác nhau ở bit thấp nhất. Lực lượng này$h(q_i)=h(q_j)$hoặc các cặp va chạm liền kề, mâu thuẫn với tính phân biệt của các phần tử của$J$và việc xây dựng$Q'$. Vì thế$g$được tiêm vào$Q''$. 

### (c) Xây dựng$Q^*$Định nghĩa$$Q^*=\{q\in Q''\mid g(q)\ \text{even and } g(q)+g(p)=2^{l-1}\ \text{for some }p\in P\}.$$Hạn chế tính đồng đều$g(q)$đến một tập hợp con của các lớp dư thừa theo modulo$2^{l-1}$. điều kiện$g(p)+g(q)=2^{l-1}$ghép các giá trị bit giữa bổ sung, phân vùng${0,\dots,2^{l-1}-1}$thành những phần bổ sung rời rạc. 

Tính tiêm nhiễm của$g$TRÊN$Q''$ngụ ý mỗi$q$đóng góp nhiều nhất một cặp được chấp nhận. Từ$|P|$đủ lớn để bao phủ ít nhất$(1-\varepsilon)2^{l-1}$dư lượng, mỗi dư lượng được chấp nhận$g(q)$có một đối tác tương ứng trong$P$. 

Như vậy$Q^*$thỏa mãn điều kiện (120) của Định lý X, cụ thể là sự tồn tại của sự so khớp giữa một tập hợp con lớn của$P$Và$Q$dưới sự bổ sung của các bit ở giữa. 

### (d) Kích thước của$Q^*$Từ phần (a), ít nhất$(1-\varepsilon)2^l$giá trị nằm ở$J$, do đó ít nhất một nửa trong số họ đóng góp vào$Q''$, cho$$|Q''|\ge \frac{1}{2}(1-\varepsilon)2^l.$$Tính tiêm nhiễm của$g$đảm bảo rằng nhiều nhất$2^{l-2}$dư lượng được loại trừ bởi các ràng buộc chẵn lẻ và bổ sung. Kết hợp với mật độ$P$hình ảnh,$$|Q^*|\ge (1-2\varepsilon)2^{l-1}.$$Giới hạn dưới này thỏa mãn yêu cầu của Định lý X, hoàn thành việc xây dựng một tập con có cấu trúc đủ lớn$Q^*$. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Lập luận trong (a) chỉ sử dụng tính độc lập theo cặp từ Bài tập 6.4-78 và tính tuyến tính của kỳ vọng, đồng thời tất cả các giới hạn va chạm đều có tỷ lệ với$\binom{2^t}{2}2^{-l}$. 

Quá trình chuyển đổi từ số lần va chạm sang số lượng hình ảnh riêng biệt sử dụng rằng mỗi lần va chạm có thể giảm số lượng giá trị băm riêng biệt tối đa một đại diện, đây là tiêu chuẩn trong các đối số nhóm. 

Phần (b) dựa vào tính tiêm của$g$giới hạn ở các đại diện có chỉ số lẻ của$J$, xuất phát từ việc loại bỏ sự mơ hồ về bit thấp nhất sau khi cắt bớt. 

Phần (c) sử dụng ghép cặp bổ sung trong${0,\dots,2^{l-1}-1}$, và ràng buộc$g(p)+g(q)=2^{l-1}$thực thi một cấu trúc khớp hoàn hảo theo yêu cầu của điều kiện (120). 

Phần (d) xuất phát từ việc bảo toàn mật độ dưới sự hạn chế đối với$Q''$và tính tiêm của$g$. 

## Ghi chú 

Cấu trúc này là một ứng dụng tiêu chuẩn hai cấp độ của hàm băm phổ quát: nén đầu tiên thành$l$bit trong khi kiểm soát xung đột, sau đó tinh chỉnh bằng cách sử dụng phần giữa$l-1$bit để tạo ra một biểu đồ ghép nối trên dư lượng modulo$2^{l-1}$. Điều kiện nhân lẻ ở$a$đảm bảo rằng việc cắt bớt duy trì đủ sự độc lập giữa các khối bit cao và trung bình, điều này rất cần thiết cho bước chèn vào.
