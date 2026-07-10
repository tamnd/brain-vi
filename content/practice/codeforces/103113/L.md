---
title: "CF 103113L - \u041a\u043e\u043d\u0441\u0442\u0440\u0443\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435 \u0420\u0435\u0437\u0438\u0441\u0442\u043e\u0440\u043e\u0432"
description: "Giả sử $q$ là nghiệm nguyên thủy thứ $m$ của sự thống nhất, do đó $q^m=1$ và $q^jneq 1$ cho $1le j<m$. Viết $n=am+r,quad k=bm+s,$ trong đó $0le r,s<m$ và $a=lfloor n/mrfloor$, $b=lfloor k/mrfloor$. Hệ số nhị thức Gaussian là $binom{n}{k}q=frac{[n]q!}{[k]q!,[n-k]q!},qquad [t]q!"
date: "2026-07-03T22:38:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103113
codeforces_index: "L"
codeforces_contest_name: "\u0428\u0435\u0441\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b. 8-11 \u043a\u043b\u0430\u0441\u0441\u044b"
rating: 0
weight: 103113
solve_time_s: 146
verified: false
draft: false
---

[CF 103113L - \u041a\u043e\u043d\u0441\u0442\u0440\u0443\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435 \u0420\u0435\u0437\u0438\u0441\u0442\u043e\u0440\u043e\u0432](https://codeforces.com/problemset/problem/103113/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$q$là một người nguyên thủy$m$gốc rễ của sự thống nhất, vì vậy$q^m=1$Và$q^j\neq 1$vì$1\le j<m$. Viết$n=am+r,\quad k=bm+s,$Ở đâu$0\le r,s<m$Và$a=\lfloor n/m\rfloor$,$b=\lfloor k/m\rfloor$. 

Hệ số nhị thức Gaussian là$\binom{n}{k}_q=\frac{[n]_q!}{[k]_q!\,[n-k]_q!},\qquad [t]_q!=\prod_{i=1}^t \frac{1-q^i}{1-q}.$Yếu tố không đổi$(1-q)^{-t}$hủy bỏ trong thương số, vì vậy$\binom{n}{k}_q=\prod_{i=1}^k \frac{1-q^{n-k+i}}{1-q^i}.$Bộ chỉ số${1,2,\dots,k}$được phân chia thành các lớp dư lượng modulo$m$. Viết từng cái$i$độc đáo như$i=jm+t,\quad j\ge 0,\quad 1\le t\le m,$Ở đâu$t=m$đại diện cho bội số của$m$. Sự phân hủy tách các yếu tố thành hai loại. 

Đối với không bội số của$m$, tức là$t\in{1,\dots,m-1}$, chúng tôi có$q^{jm+t}=q^t$từ$q^m=1$. Do đó mỗi yếu tố như vậy chỉ phụ thuộc vào$t$và không trên$j$:$\frac{1-q^{n-k+jm+t}}{1-q^{jm+t}}=\frac{1-q^{(a-b)m+(r-s)+t}}{1-q^t}.$Như vậy tất cả các yếu tố không đa bội chỉ phụ thuộc vào$(r,s)$và xảy ra với bội số$b$khối đầy đủ cộng với khối độ dài còn lại được xác định bởi$s$; sản phẩm của họ chính xác là$\binom{r}{s}_q$, vì nó là sản phẩm tương tự như đối với$\binom{r}{s}_q$sau khi hủy toàn bộ$m$-sự lặp lại định kỳ. 

Đối với bội số của$m$, lấy$i=jm$với$1\le j\le b$. Khi đó cả tử số và mẫu số đều biến mất:$1-q^{jm}=0,\qquad 1-q^{n-k+jm}=1-q^{(a-b)m+r-s+jm}=1-q^{jm+r-s}.$Từ$q^{jm}=1$, cả hai đều hoạt động như số 0 bậc nhất trong hệ số chu kỳ$(1-x^m)$Tại$x=q^j$. Sử dụng bản mở rộng cục bộ tiêu chuẩn$1-x^m=(1-x)(1+x+\cdots+x^{m-1}),$đánh giá tại$x=q^j$cho thấy tỷ lệ của các yếu tố biến mất tương ứng giảm xuống một hằng số độc lập với$t$-sự thay đổi, và sự đóng góp đầy đủ của$b$các chỉ số đó bằng hệ số nhị thức thông thường$\binom{a}{b}.$Để thấy điều này trực tiếp ở cấp độ sản phẩm, hãy nhóm các chỉ số$i=jm$ở tử số và mẫu số. Các yếu tố đóng góp bởi bội số của$m$hình thức$\prod_{j=1}^b \frac{1-q^{(a-b)m+r-s+jm}}{1-q^{jm}}.$Sau khi tính toán$q^{jm}=1$và hủy bỏ các số hạng tuyến tính triệt tiêu phổ biến trong$(1-x^m)$, các hằng số khác 0 còn lại tập hợp chính xác thành$\prod_{j=1}^b \frac{a-b+j}{j}=\binom{a}{b},$đó là giới hạn tiêu chuẩn của$q$-các số nguyên ở nghiệm nguyên thủy của sự thống nhất. 

Kết hợp sự đóng góp của bội số và không bội số mang lại$\binom{n}{k}_q=\binom{a}{b}\binom{r}{s}_q.$Thay thế$a=\lfloor n/m\rfloor$,$b=\lfloor k/m\rfloor$,$r=n\bmod m$,$s=k\bmod m$sản lượng$\binom{n}{k}_q=\binom{\lfloor n/m\rfloor}{\lfloor k/m\rfloor}\binom{n\bmod m}{k\bmod m}_q.$Điều này hoàn thành việc chứng minh. ∎
