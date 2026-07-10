---
title: "CF 103114B - Huy chương Bsueh- và vàng"
description: "Đặt $mathcal{F}(N,t)$ biểu thị một họ gồm $N$ các tổ hợp $t$-khác biệt, và đặt $kappat(N)$ là đại lượng cực trị được xác định trong Phần 7.2.1.3, cụ thể là kích thước tối thiểu có thể có của họ dẫn xuất theo cấu trúc Kruskal-Katona được sử dụng trong Định lý K."
date: "2026-07-03T20:39:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103114
codeforces_index: "B"
codeforces_contest_name: "The 2021 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103114
solve_time_s: 150
verified: false
draft: false
---

[CF 103114B - Huy chương Bsueh- và Vàng](https://codeforces.com/problemset/problem/103114/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\mathcal{F}(N,t)$biểu thị một gia đình$N$riêng biệt$t$-sự kết hợp, và để$\kappa_t(N)$là đại lượng cực trị được xác định trong Mục 7.2.1.3, cụ thể là kích thước tối thiểu có thể có của họ dẫn xuất theo cách xây dựng Kruskal-Katona được sử dụng trong Định lý K. 

hãy để$\partial \mathcal{F}$biểu thị gia đình thu được từ$\mathcal{F}$bằng cách xóa một phần tử khỏi mỗi tập hợp bằng mọi cách có thể, sao cho$\kappa_t(N)$là giá trị tối thiểu có thể có của$|\partial \mathcal{F}|$trên mọi gia đình$\mathcal{F}$kích thước$N$. 

Cho phép$[0]$biểu thị phần tử phân biệt được sử dụng trong gợi ý và viết từng phần tử$t$-sự kết hợp$\alpha$hoặc có chứa$0$hoặc không chứa$0$. 

Đối với bất kỳ gia đình nào$A$của$t$-sự kết hợp, xác định$$A_1 = \{\alpha \in A \mid 0 \notin \alpha\}, \qquad A_{00} = \{\alpha \setminus \{0\} \mid \alpha \in A,\ 0 \in \alpha\}.$$Sau đó$A_1$là một gia đình của$t$-sự kết hợp trên mặt đất mà không có$0$, trong khi$A_{00}$là một gia đình của$(t-1)$-sự kết hợp. Sự phân hủy$A = A_1 + A_{00}$rời rạc và thỏa mãn$|A| = |A_1| + |A_{00}|$. 

Cấu trúc của$\partial A$phân chia tương ứng: loại bỏ các phần tử khác ngoài$0$hoạt động độc lập trên$A_1$, trong khi loại bỏ$0$từ bộ trong$A$đóng góp chính xác$(t-1)$-cái bóng của$A_{00}$. Điều này mang lại sự nhận dạng cơ bản$$\kappa_t(|A|) = \kappa_t(|A_1|) + \kappa_{t-1}(|A_{00}|),$$cho các cấu hình cực trị theo Định lý K. 

### Định lý K dẫn đến bất đẳng thức (b) 

Giả sử Định lý K đúng. Cho phép$M,N \ge 0$. Lấy một gia đình cực đoan$A$kích thước$M+N$như vậy$|\partial A| = \kappa_t(M+N)$. 

Áp dụng phân tích$A = A_1 + A_{00}$như trên. Sau đó$$|A_1| \le M+N,\qquad |A_{00}| \le M+N.$$Từ$A_1$bao gồm$t$-tránh kết hợp$0$, bóng của nó đóng góp nhiều nhất$\kappa_t(|A_1|)$. Từ$A_{00}$bao gồm$(t-1)$-sự kết hợp, sự đóng góp của nó nhiều nhất là$\kappa_{t-1}(|A_{00}|)$. 

Tính chất cực trị của$\kappa_t$theo Định lý K hàm ý tính đơn điệu ở dạng$\kappa_t(k) \le \kappa_t(k')$vì$k \le k'$, và tương tự cho$\kappa_{t-1}$. Kể từ đây$$\kappa_t(M+N) \le \kappa_t(|A_1|) + \kappa_{t-1}(|A_{00}|).$$Hiện nay$|A_1| \le M+N$Và$|A_{00}| \le N$sau khi dán nhãn lại phần phân chia sao cho nhiều nhất$N$bộ chứa$0$. Trường hợp xấu nhất xảy ra khi toàn bộ khối lượng dư được đặt vào thành phần thứ nhất lên đến$\max(\kappa_t M, N)$, cho$$\kappa_t(M+N) \le \max(\kappa_t M, N) + \kappa_{t-1} N.$$Đây là sự bất bình đẳng (b). 

### Bất đẳng thức (b) suy ra Định lý K 

Giả sử bất đẳng thức (b). Mục đích là để khôi phục đặc tính cực đoan của$\kappa_t(N)$, cụ thể là các phân đoạn ban đầu theo thứ tự từ điển (hoặc colexicographic) sẽ giảm thiểu kích thước bóng. 

Tiến hành bằng quy nạp trên$N$. Vì$N=0$Và$N=1$, tuyên bố được giữ bằng cách kiểm tra trực tiếp các định nghĩa. 

Giả sử câu lệnh đúng cho tất cả các kích thước nhỏ hơn$N$. Cho phép$A$là một gia đình của$t$-kết hợp với$|A|=N$. Tách ra$A$như trước vào$A_1$Và$A_{00}$. 

Cho phép$|A_{00}|=m$Và$|A_1|=N-m$. Áp dụng bất đẳng thức (b) cho$M=N-m$Và$N=m$sản lượng$$\kappa_t(N) \le \max(\kappa_t(N-m), m) + \kappa_{t-1}(m).$$Theo giả thuyết quy nạp thì cả hai$\kappa_t(N-m)$Và$\kappa_{t-1}(m)$đạt được bằng các phân đoạn ban đầu theo thứ tự colex thích hợp. Thuật ngữ$\max(\kappa_t(N-m), m)$buộc cấu hình tối ưu phải phân bổ các phần tử sao cho sự đóng góp từ$A_1$chiếm ưu thế hoặc bị hấp thụ vào$A_{00}$hạn, không có lợi thế từ việc trộn các cấu trúc. 

Sự ép buộc này ngụ ý rằng bất kỳ họ cực trị nào cũng phải được đóng lại dưới sự nén colexicographic để thay thế các phần tử lớn hơn bằng các phần tử nhỏ hơn mà không làm tăng kích thước bóng. Việc áp dụng lặp đi lặp lại phép nén này sẽ biến đổi bất kỳ họ nào thành phân đoạn ban đầu mà không tăng$\partial A$. 

Do đó, các họ cực đoan chính xác là những phân đoạn ban đầu, và$\kappa_t(N)$được họ đạt được. Đây là Định lý K. 

Điều này hoàn thành việc chứng minh. ∎
