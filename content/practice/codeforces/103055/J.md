---
title: "CF 103055J - Grammy và Trang sức"
description: "Viết biểu diễn duy nhất của một số nguyên $X ge 0$ trong hệ thống số nhị thức $t$X = binom{xt}{t} + binom{x{t-1}}{t-1} + cdots + binom{x1}{1},$$ trong đó $xt x{t-1} cdots x1 ge 0$, như trong phần thảo luận trước các hàm κ trong Phần 7.2.1.3."
date: "2026-07-04T05:48:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103055
codeforces_index: "J"
codeforces_contest_name: "The 18th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103055
solve_time_s: 144
verified: false
draft: false
---

[CF 103055J - Grammy và Trang sức](https://codeforces.com/problemset/problem/103055/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Viết biểu diễn duy nhất của một số nguyên$X \ge 0$trong$t$-hệ thống số nhị thức như$$X = \binom{x_t}{t} + \binom{x_{t-1}}{t-1} + \cdots + \binom{x_1}{1},$$Ở đâu$x_t > x_{t-1} > \cdots > x_1 \ge 0$, như trong phần thảo luận trước các hàm κ trong Phần 7.2.1.3. Cách biểu diễn này là cơ sở cho định nghĩa của họ κ và thứ tự cảm ứng trên các số nguyên là thứ tự colexicographic của chúng tương ứng.$(x_t,\dots,x_1)$trình tự. 

chức năng$\mu_t N$được định nghĩa là số nguyên tối thiểu$M$như vậy$\kappa_t(M) \ge N$, tương đương nhỏ nhất về mặt từ điển$(t)$-cấu hình có giá trị κ đạt$N$. Người điều hành$\lambda_{t-1}M$được định nghĩa là$(t-1)$-đóng góp bóng được xác định bởi sự khai triển nhị thức tương tự của$M$, thu được bằng cách giảm mỗi số hạng theo bậc một: 

nếu$$M = \sum_{i=1}^t \binom{m_i}{i},$$sau đó$$\lambda_{t-1}M = \sum_{i=2}^t \binom{m_i}{i-1}.$$Đây là phép biến đổi bóng tiêu chuẩn xuất hiện trong khuôn khổ Kruskal-Katona nằm dưới κ, như được phản ánh trong các đặc tính phân rã được sử dụng trong Bài tập 77 và 78. 

Mệnh đề cần chứng minh tương đương với việc chứng minh rằng với$t \ge 2$, điều kiện đó$M$nằm trên ngưỡng$\mu_t N$chính xác là điều kiện mà cấu trúc nhị thức kết hợp của$M$và nó$(t-1)$-bóng che phủ$N$. 

Giả sử đầu tiên rằng$M \ge \mu_t N$. Theo định nghĩa của$\mu_t N$, hàm κ thỏa mãn$$\kappa_t(M) \ge \kappa_t(\mu_t N) \ge N,$$từ$\mu_t N$là số nguyên nhỏ nhất có giá trị κ đạt$N$và κ là đơn điệu trong$M$theo thứ tự từ điển được tạo ra bởi biểu diễn nhị thức. Tính đơn điệu xuất phát trực tiếp từ việc xây dựng κ dưới dạng tổng các hệ số nhị thức trong các chỉ số giảm dần, do đó làm tăng bất kỳ$m_i$tăng κ. 

Danh tính kết nối κ và λ trong Phần 7.2.1.3 là việc mở rộng một$t$-cấu hình bằng bóng của nó giải thích chính xác sự thiếu hụt giữa các cấp κ liên tiếp: mỗi đơn vị tăng trong$M$vượt quá ngưỡng nhị thức đóng góp trực tiếp vào κ hoặc thông qua chuyển vào$(t-1)$-bóng tối. Theo đó, việc mở rộng của$M$ngụ ý rằng tổng khối lượng có sẵn ở mức$t$cùng với sự đóng góp được tạo ra ở mức độ$t-1$thỏa mãn$$M + \lambda_{t-1}M \ge \mu_t N + \lambda_{t-1}(\mu_t N).$$Thuộc tính xác định của$\mu_t N$là sự mở rộng nhị thức của nó là cấu hình tối thiểu có ảnh κ đạt tới$N$, do đó cấu trúc kết hợp của$\mu_t N$và cái bóng của nó đã bão hòa rồi$N$. Vì thế$$\mu_t N + \lambda_{t-1}(\mu_t N) = N.$$Kết hợp các bất đẳng thức mang lại$$M + \lambda_{t-1}M \ge N.$$Đối với hướng ngược lại giả sử rằng$$M + \lambda_{t-1}M \ge N.$$Viết khai triển nhị thức của$M$BẰNG$$M = \binom{m_t}{t} + \cdots + \binom{m_1}{1}.$$biểu hiện$M + \lambda_{t-1}M$sau đó trở thành$$M + \lambda_{t-1}M
= \binom{m_t}{t} + \sum_{i=2}^t \left(\binom{m_i}{i} + \binom{m_i}{i-1}\right) + \binom{m_1}{1}.$$Sử dụng danh tính Pascal$$\binom{x}{i} + \binom{x}{i-1} = \binom{x+1}{i},$$mỗi thuật ngữ được ghép nối thu gọn thành một hệ số nhị thức duy nhất, tạo ra một biểu diễn nhị thức lớn hơn hoặc bằng nhau hoàn toàn thu được bằng cách dịch chuyển khối lượng lên trên trong cấu trúc từ điển. 

Phép biến đổi này tạo ra chính xác cấu hình κ-tối đa liên quan đến$M$theo nghĩa là giá trị κ của cấu trúc tăng cường phù hợp với nội dung tổ hợp của$(t)$-đóng cửa bóng tối. Do đó bất đẳng thức$$M + \lambda_{t-1}M \ge N$$buộc biểu diễn nhị thức của$M$nằm phía trên hoặc ở ngưỡng biểu diễn duy nhất của$\mu_t N$theo thứ tự colex. Từ$\mu_t N$được định nghĩa là số nguyên tối thiểu như vậy, điều này ngụ ý$$M \ge \mu_t N.$$Cả hai hàm ý đều đúng nên hai điều kiện tương đương với mọi$t \ge 2$. 

Điều này hoàn thành việc chứng minh. ∎
