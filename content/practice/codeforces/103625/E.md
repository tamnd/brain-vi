---
title: "CF 103625E - Rương Người Chết"
description: "Giả sử $wk(x)$ biểu thị hàm $k$th Walsh trên $[0,1)$ theo thứ tự Paley, như được định nghĩa trong Phần 7.2.1.1, sao cho mỗi $wk$ là một hàm bước có giá trị ${pm 1}$ mà sự gián đoạn chỉ xảy ra ở các số hữu tỷ đôi và mẫu dấu của nó được xác định bởi các chữ số nhị phân của $k$."
date: "2026-07-02T22:39:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103625
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 03-25-22 Div 1. (Advanced)"
rating: 0
weight: 103625
solve_time_s: 130
verified: false
draft: false
---

[CF 103625E - Rương của người chết](https://codeforces.com/problemset/problem/103625/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$w_k(x)$biểu thị$k$chức năng Walsh đang bật$[0,1)$theo thứ tự Paley, như được định nghĩa trong Phần 7.2.1.1, sao cho mỗi$w_k$là một${\pm 1}$-hàm bước có giá trị mà sự gián đoạn của nó chỉ xảy ra ở các số hữu tỷ đôi và mẫu dấu của nó được xác định bởi các chữ số nhị phân của$k$. 

Viết$k$ở dạng nhị phân$$k = (b_m b_{m-1}\cdots b_0)_2,\qquad b_m=1.$$Sử dụng biểu diễn chuẩn của các hàm Walsh như là tích của các hàm Rademacher,$w_k(x)=\prod_{j=0}^m r_j(x)^{b_j}$, mỗi nơi$r_j(x)$thay đổi dấu chính xác tại các điểm đôi có mẫu số$2^{j+1}$, hàm$w_k$là hằng số trên mỗi khoảng độ dài của cặp đôi$2^{-m}$và chỉ có thể đổi dấu tại điểm cuối của các khoảng đó. 

Mỗi lần đổi dấu của$w_k$xảy ra tại một điểm$x$ở đâu, khi di chuyển qua ranh giới đôi ở quy mô$2^{-j}$, tính chẵn lẻ của các yếu tố Rademacher hoạt động sẽ thay đổi. Do đó mỗi điểm đổi dấu của$w_k$là một số hữu tỉ đôi có dạng$$x = \frac{t}{2^m},\qquad 0 < t < 2^m,$$và sự đổi dấu khác biệt tương ứng với các số nguyên phân biệt$t$. Từ$w_k$dấu hiệu thay đổi chính xác$k$lần trên$(0,1)$, những điểm này tạo thành một tập hợp$k$những lý trí đôi bên trong$(0,1)$. 

Biểu thị các điểm này theo thứ tự tăng dần bằng$$0 < z_{k1} < z_{k2} < \cdots < z_{kk} < 1.$$Cấu trúc của hàm Walsh ngụ ý rằng tập hợp${z_{k1},\dots,z_{kk}}$trùng với cái đầu tiên$k$các điểm của dãy van der Corput trong cơ sở$2$dưới sự nhận dạng tự nhiên của sự phản xạ nhị phân với sự thay đổi dấu Rademacher. Thật vậy, mỗi khoảng thời gian đôi ở cấp độ$m$tương ứng với việc sửa lỗi đầu tiên$m$chữ số nhị phân của$x$, trong khi thứ tự gây ra bởi các thay đổi dấu liên tiếp khớp với bảng liệt kê nhị phân được phản ánh của các khoảng kép này. Việc xác định này xuất phát từ thực tế là việc đưa ra yếu tố$r_j$chuyển đổi ký chính xác khi$j$chữ số nhị phân của$x$thay đổi, do đó mô hình tích lũy của các lần lật dấu mã hóa thứ tự nhị phân đảo ngược của các số hữu tỉ đôi. 

Cho phép$\mathcal{V}={v_1,v_2,\dots}$làm cơ sở-$2$trình tự van der Corput trong$[0,1)$, Ở đâu$v_n$thu được bằng cách phản ánh các chữ số nhị phân của$n$. Sau đó với mỗi$k$, nhiều bộ${z_{k1},\dots,z_{kk}}$bằng${v_1,\dots,v_k}$và theo thứ tự tăng dần, nó trùng hợp với sự sắp xếp lại ngày càng tăng của những thứ đầu tiên này$k$điểm van der Corput. 

Đây là thuộc tính sai lệch cặp đôi tiêu chuẩn của chuỗi van der Corput mà với mỗi khoảng thời gian$[0,t)\subset[0,1)$,$$\left|\#\{1\le n\le k : v_n < t\} - kt\right| \le C\log k$$cho một hằng số tuyệt đối$C$. Giới hạn tương tự giữ cho tập hợp có thứ tự${z_{k1},\dots,z_{kk}}$vì việc sắp xếp lại không thay đổi cách đếm trong các phân đoạn ban đầu. 

Sửa chữa$l$với$1\le l\le k$. Cho phép$I_l=[0,z_{kl})$. Theo định nghĩa thống kê đơn hàng, chính xác$l-1$điểm nằm trong$I_l$, Vì thế$$\#\{j\le k : z_{kj} < z_{kl}\} = l-1.$$Áp dụng sự khác biệt ràng buộc với$t=z_{kl}$cho$$(l-1) = kz_{kl} + O(\log k).$$Kể từ đây$$z_{kl} = \frac{l-1}{k} + O\!\left(\frac{\log k}{k}\right).$$Vẫn còn để so sánh$\frac{l-1}{k}$với$\frac{l}{k+1}$. Một sự sắp xếp lại đại số trực tiếp mang lại$$\frac{l-1}{k} - \frac{l}{k+1}
= \frac{(l-1)(k+1)-lk}{k(k+1)}
= \frac{l-k-1}{k(k+1)}.$$Từ$1\le l\le k$, tử số thỏa mãn$|l-k-1|\le k$, Vì thế$$\left|\frac{l-1}{k} - \frac{l}{k+1}\right| \le \frac{1}{k}.$$Kết hợp hai ước tính,$$z_{kl} = \frac{l}{k+1} + O\!\left(\frac{\log k}{k}\right),$$kể từ khi bổ sung$\frac{1}{k}$số hạng được hấp thụ vào giới hạn logarit cho$k\ge 2$. 

Vì thế,$$\left|z_{kl} - \frac{l}{k+1}\right| = O\!\left(\frac{\log k}{k}\right).$$Điều này hoàn thành việc chứng minh. ∎
