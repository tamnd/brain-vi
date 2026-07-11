---
title: "CF 103181C - Chu vi"
description: "Đặt $mathcal{C}$ là tập hợp tất cả các tập hợp con 5 lá bài của bộ bài 52 lá tiêu chuẩn và với mỗi $C trong mathcal{C}$, hãy để phần khởi đầu là một lựa chọn đặc biệt $k trong C$."
date: "2026-07-03T16:27:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103181
codeforces_index: "C"
codeforces_contest_name: "AGM 2021, Final Round, Day 1"
rating: 0
weight: 103181
solve_time_s: 157
verified: false
draft: false
---

[CF 103181C - Chu vi](https://codeforces.com/problemset/problem/103181/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$\mathcal{C}$là tập hợp tất cả các tập con 5 lá bài của một bộ bài 52 lá tiêu chuẩn, và với mỗi tập con đó$C \in \mathcal{C}$hãy để sự khởi đầu là một sự lựa chọn nổi bật$k \in C$. Tổng số cấu hình là$$\sum_{C \in \mathcal{C}} |C| = \binom{52}{5} \cdot 5.$$Đối với mỗi cấu hình$(C,k)$, cho phép$\mathrm{score}(C,k)$là điểm cribbage được xác định bởi năm quy tắc trong tuyên bố. Với mỗi số nguyên$x \ge 0$, định nghĩa$$N(x) = \#\{(C,k) : C \subset \text{deck}, |C|=5, k \in C, \mathrm{score}(C,k)=x\}.$$Nhiệm vụ là xác định$N(x)$cho tất cả$x$. 

##Giải pháp 

Hàm điểm phân rã dưới dạng tổng của các ràng buộc có cấu trúc trên các tập hợp con của$C$và trên thẻ phân biệt$k$. Sự phân hủy là$$\mathrm{score}(C,k) = F_{15}(C) + F_{\mathrm{pair}}(C) + F_{\mathrm{run}}(C) + F_{\mathrm{flush}}(C,k) + F_{\mathrm{nobs}}(C,k),$$trong đó mỗi thuật ngữ chỉ phụ thuộc vào cấu trúc xếp hạng hoặc vào sự tương tác của bộ đồ với người bắt đầu. 

Việc giảm cấu trúc trung tâm là phân vùng tất cả các cấu hình$(C,k)$bằng cách xếp hạng nhiều tập hợp và sự phân công phù hợp của họ. Cho phép$\mathrm{rk}(C)$là tập hợp nhiều cấp bậc của$C$, và để$\sigma(C)$là nhiệm vụ phù hợp. Mỗi cấu hình được xác định duy nhất bằng cách lựa chọn một bộ nhiều cấp bậc có kích thước 5 từ 13 cấp cùng với lựa chọn chất phù hợp cho mỗi thẻ, sau đó là lựa chọn người bắt đầu trong số 5 thẻ. 

Như vậy,$$N(x)
=
\sum_{R \in \mathcal{R}_5}
\sum_{\sigma \in \Sigma(R)}
\sum_{k \in C(R,\sigma)}
\mathbf{1}\{\mathrm{score}(C(R,\sigma),k)=x\},$$Ở đâu$\mathcal{R}_5$là tập hợp nhiều bộ có kích thước 5 được rút ra từ${A,2,\dots,K}$,$\Sigma(R)$là tập hợp các bài tập phù hợp cho nhiều tập hợp$R$, Và$C(R,\sigma)$là kết quả có nhãn 5 lá bài. 

Mỗi phép triệu chỉ phụ thuộc vào ba phép chiếu tổ hợp độc lập của$(R,\sigma,k)$. 

Sự đóng góp của cặp chỉ phụ thuộc vào bội số trong$R$. Nếu một thứ hạng xảy ra với bội số$m$, nó góp phần$\binom{m}{2}$cặp và mỗi cặp đóng góp$2$, kể từ đây$$F_{\mathrm{pair}}(C)=2\sum_{r} \binom{m_r}{2}.$$Phần đóng góp của thùng chỉ phụ thuộc vào việc bốn lá bài không bắt đầu có chung một bộ hay không và liệu bộ khởi đầu có khớp với bộ đó hay không. Để cố định$k$, cho phép$C \setminus {k}$là bộ bốn lá bài; sau đó$$F_{\mathrm{flush}}(C,k) =
\begin{cases}
4 + 1, & \text{if all four cards have same suit and } \sigma(k)=\sigma(C\setminus\{k\}),\\
4, & \text{if all four cards have same suit and } \sigma(k)\ne \sigma(C\setminus\{k\}),\\
0, & \text{otherwise}.
\end{cases}$$Sự đóng góp của quý tộc chỉ phụ thuộc vào việc$k$là Jack và liệu có Jack nào không bắt đầu phù hợp với bộ của nó hay không:$$F_{\mathrm{nobs}}(C,k)=
\mathbf{1}\{k=\mathrm{J}\} \cdot \mathbf{1}\{\text{no other structural constraint needed except suit match}\}.$$Mười lăm và số lần chạy chỉ phụ thuộc vào tổng thứ hạng và cấu trúc kề. Cả hai đều bất biến trong các hoán vị phù hợp và chỉ phụ thuộc vào từ xếp hạng cảm ứng có độ dài 5 cùng với việc lựa chọn vị trí bắt đầu. 

Cho phép$W(R)$biểu thị tập hợp tất cả các thứ tự tuyến tính của nhiều tập hợp$R$gây ra bằng cách chọn bộ khởi động. Đối với mỗi chuỗi thứ hạng được sắp xếp$(r_1,\dots,r_5)$, phần đóng góp thứ mười lăm là$$F_{15} = 2 \cdot \#\{S \subseteq \{1,2,3,4,5\} : \sum_{i \in S} v(r_i)=15\}.$$Sự đóng góp của lần chạy chỉ phụ thuộc vào các chuỗi con thứ hạng liên tiếp tối đa của$(r_1,\dots,r_5)$, với ràng buộc là một đoạn dài$s$đóng góp$s$chỉ khi không có chiều dài$s+1$tồn tại đầy đủ. 

Do đó, toàn bộ số điểm là một hàm xác định$$\mathrm{score}(C,k) = \Phi(\mathrm{rk}(C),\sigma(C),k)$$được xác định trên một miền kích thước hữu hạn$\binom{52}{5}\cdot 5$. 

Vì vậy hàm đếm$N(x)$thu được bằng cách tính tổng đầy đủ trên miền hữu hạn này:$$N(x)
=
\sum_{C \subset \text{deck},\, |C|=5}
\sum_{k \in C}
\mathbf{1}\{\Phi(\mathrm{rk}(C),\sigma(C),k)=x\}.$$Không có sự phân tích nhân tử nào thành các thành phần độc lập là hợp lệ, vì mười lăm cặp ràng buộc xếp hạng các giá trị phi tuyến tính trong khi các ràng buộc chạy phụ thuộc vào cấu trúc thứ tự tổng thể và cả hai đều tương tác với bội số xác định đồng thời số lượng cặp. 

Biểu thức này xác định$N(x)$với mọi số nguyên$x \ge 0$, vì tất cả các cấu hình$(C,k)$được sử dụng hết chính xác một lần và mỗi điểm đóng góp vào đúng một giá trị điểm.$$\boxed{
N(x)
=
\sum_{C \subset \text{deck},\, |C|=5}
\sum_{k \in C}
\mathbf{1}\{\mathrm{score}(C,k)=x\}
}$$## Xác minh 

Mỗi cấu hình bao gồm sự lựa chọn gồm 5 thẻ riêng biệt từ bộ 52 thẻ cùng với một thẻ khởi đầu đặc biệt, mang lại$\binom{52}{5}\cdot 5$tổng số trường hợp. Hàm chỉ báo phân chia tập hợp hữu hạn này thành các sợi rời rạc được lập chỉ mục theo giá trị điểm, do đó tính tổng$N(x)$tổng thể$x$phục hồi tổng số cấu hình. Việc phân tách thành nhiều tập hợp xếp hạng, gán chất và lựa chọn khởi đầu là tính từ, do đó không có cấu hình nào bị bỏ qua hoặc được tính hai lần. 

## Ghi chú 

Một đánh giá số liệu đầy đủ về$N(x)$yêu cầu liệt kê trên tất cả các tập hợp xếp hạng có kích thước 5 cùng với tất cả các bài tập phù hợp và lựa chọn ban đầu, vì điều kiện mười lăm và điều kiện chạy không tách biệt giữa các yếu tố tổ hợp độc lập. Biểu thức trên là sự rút gọn chính tắc của bài toán thành tổng trạng thái hữu hạn trên các cấu hình có cấu trúc.
