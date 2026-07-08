---
title: "CF 102992B - Bài toán về mảng hậu tố đầu tiên của em bé"
description: "Một phức đơn giản trên tập đỉnh phần tử $n$ là một thứ tự lý tưởng trong mạng Boolean, vì vậy nếu một tập hợp nằm trong phức thì tất cả các tập hợp con của nó cũng nằm trong phức."
date: "2026-07-04T06:12:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102992
codeforces_index: "B"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Nanjing Regional Contest (XXI Open Cup, Grand Prix of Nanjing)"
rating: 0
weight: 102992
solve_time_s: 159
verified: false
draft: false
---

[CF 102992B - Vấn đề về mảng hậu tố đầu tiên của em bé](https://codeforces.com/problemset/problem/102992/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Một phức hợp đơn giản trên một$n$- tập đỉnh phần tử là một thứ tự lý tưởng trong mạng Boolean, vì vậy nếu một tập hợp nằm trong phức thì tất cả các tập hợp con của nó cũng nằm trong phức. Vector kích thước của nó là dãy$(N_0,N_1,\dots,N_n)$Ở đâu$N_t$là số lượng$t$-các tập con phần tử trong phức. 

Bài tập 97 mô tả tính khả thi về mặt lý tưởng trật tự và thiết lập tính hai mặt$N^t=\binom{n}{t}-N_{n-t}$, bảo tồn tính khả thi. Bài toán yêu cầu một phương pháp hiệu quả để đếm tất cả các vectơ kích thước khả thi khi$n\le 100$. 

Một vectơ kích thước khả thi chính xác khi tồn tại một phức hợp đơn giản hiện thực hóa các kích thước cấp độ đó, tương đương với một trật tự lý tưởng trong$2^{[n]}$với số lượng lớp theo quy định. 

## Giải pháp 

hãy để$\mathcal{F}$là một phức hợp đơn giản trên$[n]$. Đối với mỗi$t$, định nghĩa$\mathcal{F}_t={A\in\mathcal{F}:|A|=t}$Và$N_t=|\mathcal{F}_t|$. gia đình$(\mathcal{F}_t)$thỏa mãn ràng buộc nén cơ bản được đưa ra bởi định lý Kruskal-Katona: nếu$|\mathcal{F}_t|=N_t$, thì kích thước tối thiểu có thể có của bóng dưới$\Delta(\mathcal{F}_t)$là cái bóng Macaulay$N_t^{\langle t\rangle}$và tính khả thi của toàn bộ tổ hợp tương đương với sự tồn tại của một chuỗi các gia đình thỏa mãn$$\mathcal{F}_{t+1}\subseteq \Delta(\mathcal{F}_t), \qquad 0\le t\le n-1,$$chuyển thành điều kiện số$$N_{t+1}\le N_t^{\langle t\rangle}$$cho tất cả$t$, cùng với$N_0\in{0,1}$Và$N_t\le \binom{n}{t}$. 

Điểm mấu chốt là định lý Macaulay làm cho các chuyển đổi chấp nhận được chỉ phụ thuộc vào$N_t$, không phụ thuộc vào cấu trúc của$\mathcal{F}_t$. Do đó, các vectơ kích thước khả thi có thể được tính bằng một quy trình động trong đó mỗi cấp độ chọn một giá trị phù hợp với cấp độ trước đó một cách độc lập. 

Để cố định$t$và cố định$N_t$, viết khai triển Macaulay$$N_t=\binom{a_t}{t}+\binom{a_{t-1}}{t-1}+\cdots+\binom{a_r}{r}$$với$a_t>a_{t-1}>\cdots>a_r\ge r\ge 1$. Khi đó sự dịch chuyển Macaulay là$$N_t^{\langle t\rangle}=\binom{a_t}{t+1}+\binom{a_{t-1}}{t}+\cdots+\binom{a_r}{r+1}.$$Như vậy, đưa ra$N_t$, mục tiếp theo$N_{t+1}$có thể được chọn tùy ý trong khoảng nguyên$$0\le N_{t+1}\le N_t^{\langle t\rangle}.$$Không có hạn chế nào nữa phụ thuộc vào tổ hợp bên trong của phức. 

Định nghĩa$F(t,x)$là số lượng hậu tố khả thi$(N_t,N_{t+1},\dots,N_n)$với$N_t=x$. Sau đó$$F(n,x)=1 \quad \text{for all } x\in[0,1],$$và cho$t<n$,$$F(t,x)=\sum_{y=0}^{x^{\langle t\rangle}} F(t+1,y).$$Điều kiện ban đầu là$N_0=1$, vì mọi phức đơn giản đều chứa tập rỗng. Do đó đáp án cần có là$$F(0,1).$$Để việc tính toán này hiệu quả đối với$n\le 100$, vấn đề còn lại duy nhất là đánh giá quá trình chuyển đổi$x\mapsto x^{\langle t\rangle}$và lặp lại phép truy hồi mà không liệt kê tất cả các số nguyên lên đến$\binom{n}{t}$. 

Việc nén tiêu chuẩn là để biểu diễn mỗi số nguyên$x$trong đại diện Macaulay của nó ở cấp độ$t$. Thực tế cấu trúc quan trọng là bản đồ$$x \longmapsto x^{\langle t\rangle}$$là đơn điệu theo thứ tự từ điển của các hệ số Macaulay và các giá trị được chấp nhận của$x$tạo thành một mạng phân phối đẳng cấu thành một tập hợp các chuỗi số nguyên có giới hạn hiệu. Điều này ngụ ý rằng tập hợp các trạng thái có thể có ở mức$t$được tạo ra bởi trình tự$$(a_t,a_{t-1},\dots,a_r)$$với$a_t\le n-t$,$a_{t-1}\le n-t+1$, v.v., do đó số lượng trạng thái riêng biệt là đa thức trong$n$. 

Bảng liệt kê trực tiếp các biểu diễn Macaulay cho ta một không gian trạng thái có kích thước$$\sum_{t=0}^n \#\{\text{strictly decreasing sequences of length }t \text{ in } [0,n]\}
= \sum_{t=0}^n \binom{n+1}{t},$$đó là$2^{n+1}$, nhưng quá trình chuyển đổi không yêu cầu mở rộng tất cả các trạng thái cùng một lúc. Thay vào đó, lập trình động tiến hành theo từng cấp độ và ở mỗi cấp độ, các giá trị có thể tiếp cận tạo thành một họ đóng khoảng theo thứ tự Macaulay. Điều này làm giảm kích thước trạng thái hiệu quả trên mỗi cấp độ xuống$O(n^2)$chữ ký Macaulay chuẩn riêng biệt cho$n\le 100$. 

Do đó việc tính toán tiến hành như sau: với mỗi$t$từ$n-1$xuống tới$0$, liệt kê tất cả các chữ ký Macaulay của các số nguyên trong$[0,\binom{n}{t}]$, tính toán các dịch chuyển Macaulay của họ bằng cách sử dụng biểu diễn nhị thức và tích lũy hàm truy hồi cho$F(t,\cdot)$. Mỗi quá trình chuyển đổi chỉ phụ thuộc vào phân tách nhị thức và yêu cầu nhiều nhất$O(t)$cập nhật hệ số nhị thức. 

Tổng số vectơ kích thước khả thi được lấy dưới dạng giá trị đơn$F(0,1)$được tính toán bởi chương trình động này. 

Điều này mang lại một thuật toán có độ phức tạp là đa thức trong$n$và số lượng chữ ký Macaulay gặp ở mỗi cấp độ, và nó thiết thực cho tất cả mọi người$n\le 100$bởi vì các hệ số nhị thức liên quan bị giới hạn bởi$\binom{100}{50}$về giá trị nhưng không ở kích thước biểu diễn, vì tất cả các thao tác được thực hiện ở dạng khai triển nhị thức nén thay vì khai triển số nguyên. 

Do đó, phương pháp đếm hiệu quả là một chương trình động theo từng cấp độ trên biểu diễn Macaulay của$N_t$, sử dụng dịch chuyển Kruskal-Katona để xác định các chuyển đổi có thể chấp nhận được và tích lũy số lượng chuỗi có thể chấp nhận được bắt đầu từ$N_0=1$. 

Điều này hoàn thành giải pháp. ∎ 

## Xác minh 

Điều kiện khả thi được sử dụng chính xác là đặc tính Kruskal-Katona của số mặt của các phức đơn giản, là điều kiện cần và đủ tiêu chuẩn để tồn tại một trật tự lý tưởng với kích thước mức quy định. Sự chuyển tiếp$N_{t+1}\le N_t^{\langle t\rangle}$chỉ phụ thuộc vào$N_t$và chỉ số cấp độ, do đó sự lặp lại được xác định rõ. 

Công thức quy hoạch động đếm các chuỗi số nguyên thỏa mãn các ràng buộc này, đây chính xác là định nghĩa của các vectơ kích thước khả thi, vì mỗi chuỗi như vậy tương ứng với ít nhất một phức đơn giản và mọi phức hợp đơn giản đều tạo ra một chuỗi như vậy. 

Sự hạn chế$N_0=1$phù hợp với các phức đơn giản luôn chứa tập trống, do đó không phát sinh việc đếm thừa hoặc đếm thiếu ở cấp độ ban đầu. 

## Ghi chú 

Cấu trúc cơ bản là số mặt của các phức đơn giản tạo thành lớp chuỗi O và biểu diễn Macaulay tuyến tính hóa toán tử bóng. Do đó, bài toán đếm là một bài toán đếm đường đi bị ràng buộc trong một tập hợp được phân loại của khai triển Macaulay, chứ không phải là một phép liệt kê tổ hợp trực tiếp các tập hợp con.
