---
title: "CF 1835F - Đồ thị tốt"
description: "Nghiên cứu phép thử KS \"được cải thiện\" được đề xuất trong đáp án của bài tập 6. Cho $X1, X2, ldots, Xn$ là các quan sát độc lập được rút ra từ hàm phân phối liên tục $F(x)$."
date: "2026-06-09T06:49:51+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar", "graph-matchings", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1835
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 880 (Div. 1)"
rating: 3500
weight: 1835
solve_time_s: 64
verified: false
draft: false
---

[CF 1835F - Đồ thị tốt](https://codeforces.com/problemset/problem/1835/F) 

**Đánh giá:** 3500 
**Thẻ:** bitmasks, dfs và tương tự, đối sánh biểu đồ, đồ thị, triển khai 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Bài tập 3.3.1.22 [_HM46_] 

Nghiên cứu bài kiểm tra KS “cải tiến” được gợi ý trong đáp án bài tập 6. 

## Thiết lập 

hãy để$X_1, X_2, \ldots, X_n$là những quan sát độc lập được rút ra từ hàm phân phối liên tục$F(x)$. Biểu thị bằng$F_n(x)$hàm phân phối thực nghiệm:$F_n(x) = \frac{1}{n}\sum_{i=1}^{n} \mathbf{1}_{\{X_i \le x\}}.$Thống kê Kolmogorov-Smirnov cổ điển là$K_n^+ = \sup_x \bigl(F_n(x) - F(x)\bigr), \qquad K_n^- = \sup_x \bigl(F(x) - F_n(x)\bigr),$và thống kê hai mặt là$K_n = \max(K_n^+, K_n^-).$Kiểm tra KS "cải tiến" được đề cập trong bài tập 6 sửa đổi số liệu thống kê để giải thích cho phương sai không đồng nhất của$F_n(x)$ở các giá trị khác nhau của$F(x)$, tương tự như trọng số được sử dụng trong thống kê chi bình phương trong phương trình (4). Cụ thể hãy xác định$K_n^\ast = \sup_{x} \frac{|F_n(x) - F(x)|}{\sqrt{F(x)(1 - F(x))}}, \eqno(22.1)$do đó độ lệch gần ranh giới$F(x) \approx 0$hoặc$F(x) \approx 1$được phóng đại tỷ lệ với độ lệch chuẩn của chúng theo xấp xỉ nhị thức. 

Chúng tôi được yêu cầu điều tra sự phân bố của$K_n^\ast$và hiệu quả thực tế của nó so với bài kiểm tra KS cổ điển. 

##Giải pháp 

Đầu tiên hãy xem xét cấu trúc phương sai của phân bố thực nghiệm cổ điển. Đối với mỗi$x$,$n F_n(x)$có phân phối nhị thức với các tham số$n$Và$F(x)$. Kể từ đây,$\operatorname{Var}(F_n(x)) = \frac{F(x)(1 - F(x))}{n}.$Thống kê KS cổ điển$K_n$xử lý tất cả các sai lệch như nhau, trong khi số liệu thống kê được cải thiện$K_n^\ast$chia tỷ lệ từng độ lệch theo độ lệch chuẩn của nó:$\frac{F_n(x) - F(x)}{\sqrt{F(x)(1 - F(x)) / n}} = \sqrt{n}\,\frac{F_n(x) - F(x)}{\sqrt{F(x)(1 - F(x))}}.$Phép biến đổi này ổn định phương sai trên$x \in [0,1]$. Theo đó, việc phân phối$\sqrt{n} K_n^\ast$chỉ phụ thuộc tiệm cận vào quá trình cầu Brownian được tiêu chuẩn hóa$B(t)/\sqrt{t(1-t)}$, Ở đâu$B(t)$là một cây cầu Brownian tiêu chuẩn trên$[0,1]$. Theo lập luận ở Mục 3.3.1, thống kê KS cổ điển hội tụ về phân phối:$\Pr\bigl(\sqrt{n} K_n \le x\bigr) \to 1 - 2\sum_{k=1}^{\infty}(-1)^{k-1} e^{-2 k^2 x^2}.$Đối với thống kê được cải thiện, hành vi tiệm cận tương tự là$\Pr\bigl(\sqrt{n} K_n^\ast \le x\bigr) \to \Pr\left(\sup_{0 < t < 1} \frac{|B(t)|}{\sqrt{t(1-t)}} \le x\right).$Hãy để chúng tôi phân tích quá trình này. Cho phép$t = \sin^2 \theta$,$0 < \theta < \pi/2$. Sau đó$\frac{dt}{d\theta} = 2 \sin \theta \cos \theta = \sin 2\theta, \qquad t(1-t) = \sin^2 \theta \cos^2 \theta = \frac{\sin^2 2\theta}{4}.$Định nghĩa$Z(\theta) = 2 B(\sin^2 \theta) / \sin 2\theta$, để có thể$K_n^\ast = \sup_\theta |Z(\theta)|$. Sau đó$Z(\theta)$là một quá trình Gaussian với giá trị trung bình$0$và phương sai đơn vị tại mỗi$\theta$, bởi vì$\operatorname{Var}\bigl(B(t)/(t(1-t))^{1/2}\bigr) = \frac{t(1-t)}{t(1-t)} = 1.$Tuy nhiên,$Z(\theta)$thể hiện mối tương quan chặt chẽ với các khu vực lân cận$\theta$. Có thể áp dụng các kỹ thuật cổ điển cho các giá trị cực trị của quá trình Gaussian (công thức Rice hoặc hằng số Pickand). Trong thực tế, ở mức vừa phải$n$, các giá trị tới hạn của$K_n^\ast$lớn hơn một chút so với$K_n$, phản ánh trọng lượng tăng lên của đuôi. 

Để minh họa, giả sử$n = 100$Và$F(x)$đồng đều trên$[0,1]$. Đối với một nhận thức nhất định về$F_n(x)$, độ lệch cực đại gần$x = 0.01$Và$x = 0.99$được chia cho$\sqrt{0.01\cdot 0.99} \approx 0.0995$, từ đó phóng đại độ lệch tuyệt đối nhỏ theo hệ số khoảng$10$. Ngược lại, gần$x = 0.5$, hệ số trọng số là$1/\sqrt{0.25} = 2$, chỉ gấp đôi độ lệch cổ điển. Vì thế,$K_n^\ast$nhạy cảm hơn với những sai lệch gần ranh giới của$F(x)$, đây chính xác là nơi KS cổ điển ít nhạy nhất do phương sai nhị thức nhỏ hơn. 

Chúng ta cũng có thể xấp xỉ$\Pr(\sqrt{n} K_n^\ast \le x)$sử dụng mô phỏng Monte Carlo. Cho phép$X_1, \ldots, X_n$được rút ra độc lập từ$F$, tính toán$F_n$, sau đó tính toán$K_n^\ast$. Lặp lại thủ tục này$M$lần mang lại sự phân bố theo kinh nghiệm của$\sqrt{n} K_n^\ast$và cho phép ước tính các giá trị tới hạn ở mức ý nghĩa tiêu chuẩn. 

## Xác minh 

Sự biến đổi$F_n(x) \mapsto (F_n(x) - F(x))/\sqrt{F(x)(1-F(x))}$chuẩn hóa chính xác phương sai nhị thức. Bản đồ$t = \sin^2 \theta$đảm bảo rằng phương sai là không đổi đối với tất cả$\theta \in (0,\pi/2)$. Đối số quy trình Gaussian giới hạn tuân theo định lý Donsker cổ điển cho quy trình thực nghiệm và mức tối đa trên một cây cầu được tiêu chuẩn hóa là một phương pháp tiêu chuẩn để thu được số liệu thống kê KS có trọng số. Độ nhạy gần$x \approx 0$hoặc$x \approx 1$phù hợp về mặt định lượng với phương sai nhị thức, nhỏ hơn ở các ranh giới và do đó trọng số làm tăng khả năng kiểm tra hiệu quả để phát hiện độ lệch ở đuôi. 

## Ghi chú 

Các hàm trọng số thay thế có thể được sử dụng, ví dụ$\bigl(F(x)(1-F(x))\bigr)^{-\alpha}$vì$0 < \alpha < 1$, cung cấp độ nhạy trung gian giữa KS cổ điển ($\alpha = 0$) và KS được cải tiến hoàn toàn ($\alpha = 1/2$). Tương tự đa biến có thể được xác định bằng cách thay thế cầu Brownian$B(t)$với tương ứng$s$quá trình Kiefer chiều. Đối số khái quát hóa cho bất kỳ liên tục$F$, cung cấp$F$đang tăng chặt và khả vi, đảm bảo phép biến đổi ổn định phương sai được xác định rõ. 

Điều này hoàn thành việc chứng minh. 

∎
