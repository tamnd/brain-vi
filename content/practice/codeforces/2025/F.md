---
title: "CF 2025F - Chọn câu hỏi của bạn"
description: "Đối với đa thức $f$ bậc $n$, hãy xác định $Fk(x)=sum{j=0}^{k}f(x+j).$ Sự phụ thuộc vào $k$ được mô tả bằng công thức Faulhaber."
date: "2026-06-09T03:15:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2025
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 170 (Rated for Div. 2)"
rating: 2700
weight: 2025
solve_time_s: 129
verified: false
draft: false
---

[CF 2025F - Chọn câu hỏi của bạn](https://codeforces.com/problemset/problem/2025/F) 

**Đánh giá:** 2700 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dp, đồ thị, tham lam, cây 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Thăm dò 

Đối với một đa thức$f$mức độ$n$, định nghĩa$F_k(x)=\sum_{j=0}^{k}f(x+j).$Sự phụ thuộc vào$k$được mô tả bằng công thức Faulhaber. Nếu như$f(t)=a_nt^n+a_{n-1}t^{n-1}+\cdots,$sau đó$$=\frac{(x+k)^{n+1}-x^{n+1}}{n+1} +O(k^n),$$thống nhất cho$x$trong các khoảng có độ dài tỉ lệ với$k$. 

Điều này gợi ý giới thiệu biến được thay đổi tỷ lệ$$\qquad G_k(y)=\frac{F_k(ky)}{k^{n+1}}.$$Các tổng sau đó trở thành tổng Riemann và người ta kỳ vọng$$=\frac{a_n}{n+1}\bigl((y+1)^{n+1}-y^{n+1}\bigr).$$Đa thức giới hạn là rõ ràng. Số 0 thực của nó có thể được phân tích trực tiếp. 

## Hiểu vấn đề 

Nhiệm vụ là chứng minh sự tồn tại của một phương trình thích hợp$k$. 

Thậm chí$\deg f=n$, chúng ta phải chỉ ra rằng một số$F_k$không có rễ thực sự. 

Đối với số lẻ$\deg f=n$, chúng ta phải chỉ ra rằng một số$F_k$có đúng một gốc thực sự. 

Quan sát trung tâm là sau khi nhân rộng bằng$k$, đa thức chuẩn hóa$F_k(ky)/k^{n+1}$cách tiếp cận$$$$Từ$n+1$có tính chẵn lẻ ngược lại với$n$, cấu trúc gốc của$H$rất đơn giản và có thể được chuyển sang$G_k$cho đủ lớn$k$. 

## Kiến trúc chứng minh 

Đầu tiên chúng ta thiết lập giới hạn tiệm cận$$\longrightarrow H(y) =\frac{a_n}{n+1}\bigl((y+1)^{n+1}-y^{n+1}\bigr),$$thống nhất trên mọi khoảng giới hạn. 

Sau đó chúng tôi phân tích$H$. 

Nếu như$n$thì chẵn$n+1$thật kỳ quặc. phương trình$(y+1)^{n+1}-y^{n+1}=0$ngụ ý$(y+1)^{n+1}=y^{n+1}.$Từ$n+1$thật kỳ lạ, hàm số$t\mapsto t^{n+1}$được tiêm vào$\mathbb R$, kể từ đây$y+1=y$, không thể nào. Như vậy$H$không có rễ thực sự. 

Nếu như$n$thế thì kỳ quặc$n+1$là chẵn. phương trình$(y+1)^{n+1}=y^{n+1}$cho$|y+1|=|y|,$giải pháp thực sự độc đáo của họ là$y=-\frac12.$Hơn nữa gốc này đơn giản vì$$=a_n\bigl((y+1)^n-y^n\bigr),$$Và$$=a_n\left(\left(\frac12\right)^n-\left(-\frac12\right)^n\right) \neq0$$từ$n$thật kỳ quặc. 

Như vậy$H$có đúng một gốc thực đơn giản. 

Sự hội tụ đồng đều ngụ ý rằng với đủ lớn$k$, đa thức$G_k$có cùng số nghiệm thực như$H$. Thay đổi kích thước trở lại từ$G_k$ĐẾN$F_k$bảo toàn số lượng rễ. 

## Giải pháp 

hãy để$$\qquad a_n\neq0.$$Định nghĩa$$$$Và$$$$Viết$$= a_n(ky+j)^n + O(k^{n-1}),$$thống nhất cho$y$trong bất kỳ khoảng giới hạn cố định nào và cho$0\le j\le k$. 

Kể từ đây$$G_k(y) = a_n\frac1k \sum_{j=0}^{k} \left(y+\frac{j}{k}\right)^n +O\!\left(\frac1k\right).$$Số hạng đầu tiên là tổng Riemann. Do đó, thống nhất trên mọi khoảng giới hạn,$$G_k(y) \longrightarrow a_n\int_0^1 (y+t)^n\,dt = \frac{a_n}{n+1} \bigl((y+1)^{n+1}-y^{n+1}\bigr).$$Biểu thị đa thức giới hạn bằng$$H(y) = \frac{a_n}{n+1} \bigl((y+1)^{n+1}-y^{n+1}\bigr).$$### Phần 1 

Giả sử$n$là chẵn. 

Sau đó$n+1$thật kỳ quặc. Nếu như$H(y)=0$, sau đó$$(y+1)^{n+1}=y^{n+1}.$$Từ$n+1$thật kỳ lạ, bản đồ$t\mapsto t^{n+1}$là hoàn toàn đơn điệu, vì vậy$y+1=y$, không thể nào. 

Như vậy$H$không có rễ thực sự. 

Từ$H$liên tục và không bao giờ biến mất, tồn tại$c>0$như vậy$$|H(y)|\ge c$$cho tất cả$y$trong một khoảng đủ lớn$[-R,R]$. Sự hội tụ đồng nhất mang lại$$|G_k(y)-H(y)|<c/2$$ở đó đủ lớn$k$, kể từ đây$G_k$không có rễ trong$[-R,R]$. 

Ngoài khoảng đó,$H$giữ dấu cố định và có cùng tốc độ tăng trưởng như$y^n$. Sự hội tụ đồng đều của các hệ số ngụ ý rằng với đủ lớn$k$, dấu hiệu tương tự vẫn tồn tại đối với$G_k$. Do đó$G_k$không có gốc rễ thực sự nào cả. 

Từ$$G_k(y)=\frac{F_k(ky)}{k^{n+1}},$$đa thức$F_k$cũng không có gốc rễ thực sự. 

### Phần 2 

Giả sử$n$thật kỳ quặc. 

Sau đó$n+1$là chẵn. phương trình$$H(y)=0$$tương đương với$$|y+1|=|y|,$$giải pháp độc đáo của ai là$$y_0=-\frac12.$$Hơn nữa,$$H'(y) = a_n\bigl((y+1)^n-y^n\bigr),$$Và$$H'(y_0) = a_n\left( \left(\frac12\right)^n - \left(-\frac12\right)^n \right) \neq0.$$Như vậy$y_0$là một nghiệm đơn và là nghiệm thực duy nhất của$H$. 

Chọn một vùng lân cận nhỏ của$y_0$không chứa số 0 nào khác của$H$. Sự hội tụ đồng nhất ngụ ý rằng với đủ lớn$k$, đa thức$G_k$có chính xác một gốc thực sự trong vùng lân cận đó. Bên ngoài khu phố,$H$vẫn bị giới hạn từ$0$, do đó sự hội tụ đều chứng tỏ rằng$G_k$không có rễ thực sự bổ sung. 

Do đó để đủ lớn$k$,$G_k$có đúng một gốc thực sự. 

Thay đổi kích thước từ$G_k$quay lại$F_k$bảo tồn số lượng rễ thực sự, bởi vì$$G_k(y)=0 \iff F_k(ky)=0.$$Vì thế$F_k$có đúng một gốc thực sự. 

Cả hai tuyên bố đều được chứng minh. ∎
