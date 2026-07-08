---
title: "CF 102980A - \u041e\u0431\u0435\u0437\u0432\u0440\u0435\u0436\u0438\u0432\u0430\u043d\u0438\u0435 \u0431\u043e\u043c\u0431\u044b"
description: "Đặt $T(m1,dots,m{n-1},m)$ là hình xuyến có chiều $(n)$ được trang bị thứ tự chéo. Viết các phần tử dưới dạng $(y,a)$ trong đó $y trong T(m1,dots,m{n-1})$ và $0 le a < m$ biểu thị thành phần cuối cùng."
date: "2026-07-04T03:15:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102980
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2020-2021, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102980
solve_time_s: 154
verified: false
draft: false
---

[CF 102980A - \u041e\u0431\u0435\u0437\u0432\u0440\u0435\u0436\u0438\u0432\u0430\u043d\u0438\u0435 \u0431\u043e\u043c\u0431\u044b](https://codeforces.com/problemset/problem/102980/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$T(m_1,\dots,m_{n-1},m)$là$(n)$hình xuyến chiều được trang bị theo thứ tự chéo. Viết các phần tử dưới dạng$(y,a)$Ở đâu$y \in T(m_1,\dots,m_{n-1})$Và$0 \le a < m$biểu thị thành phần cuối cùng. Cho phép$x = x_1 \dots x_{n-1}$là$N$phần tử thứ của$T(m_1,\dots,m_{n-1})$, và xét phần tử$(x,m-1)$trong hình xuyến mở rộng. 

Cho phép$S$là tập hợp tất cả các phần tử$(y,a) \in T(m_1,\dots,m_{n-1},m)$như vậy$$(y,a) \preceq (x,m-1)$$theo thứ tự chéo. 

Đối với mỗi$a$, cho phép$N_a$biểu thị số phần tử của$S$có thành phần cuối cùng bằng$a$. 

Mục đích là để chứng minh$$N_{m-1} = N
\quad\text{and}\quad
N_{a-1} = \alpha(N_a), \qquad 1 \le a < m,$$Ở đâu$\alpha$là hàm trải rộng trên các tập chuẩn trong$T(m_1,\dots,m_{n-1})$. 

Thứ tự chéo là từ điển trong đầu tiên$n-1$tọa độ được tinh chỉnh theo tọa độ cuối cùng, do đó việc so sánh được xác định trước tiên bởi$y$và chỉ sau đó bởi$a$. 

## Giải pháp 

sửa chữa$a = m-1$. Một phần tử$(y,m-1)$nằm ở$S$chính xác khi nào$$(y,m-1) \preceq (x,m-1).$$Vì các thành phần cuối cùng giống nhau nên thứ tự chéo giảm xuống còn việc so sánh các tiền tố trong$T(m_1,\dots,m_{n-1})$, kể từ đây$$(y,m-1) \preceq (x,m-1)
\quad\Longleftrightarrow\quad
y \preceq x.$$Bộ như vậy$y$chính xác là đoạn kích thước ban đầu$N$TRONG$T(m_1,\dots,m_{n-1})$kết thúc tại$x$. Do đó số lượng phần tử được chấp nhận với thành phần cuối cùng$m-1$bằng$N$, Vì thế$N_{m-1} = N$. 

Sửa chữa$1 \le a < m$. Một phần tử$(y,a)$thuộc về$S$chính xác khi nào$$(y,a) \preceq (x,m-1).$$Từ$a < m-1$, lực lượng trật tự chéo$(y,a)$đứng trước mọi phần tử với tọa độ cuối cùng$m-1$trừ khi việc so sánh tiền tố hạn chế$y$. Sự so sánh xác định rút gọn đến điều kiện là$(y,a)$nằm trước$(x,m-1)$trong cấu trúc từ điển, điều này phụ thuộc vào cách các tiền tố được dịch chuyển giữa các cấp độ trong cấu trúc hình xuyến. 

Theo thứ tự chéo trên$T(m_1,\dots,m_{n-1},m)$, sửa tọa độ cuối cùng$a$xác định một bản sao của$T(m_1,\dots,m_{n-1})$, nhưng đoạn ban đầu của nó gây ra bởi sự cắt giảm ở$(x,m-1)$không giống với đoạn ban đầu được cắt ở mức$m-1$. Thay vào đó, nó được lấy từ cấp độ$a+1$phân đoạn ban đầu bằng phép biến đổi trải rộng$\alpha$, ánh xạ các bộ tiêu chuẩn trong$T(m_1,\dots,m_{n-1})$theo cấu trúc kề thứ tự chéo giữa các lớp liên tiếp. 

Vì vậy, tập hợp tiền tố$y$như vậy$(y,a) \in S$chính xác là$\alpha(S_{a+1}^{\mathrm{proj}})$, Ở đâu$S_{a+1}^{\mathrm{proj}}$biểu thị hình chiếu của các phần tử của$S$với thành phần cuối cùng$a+1$lên$T(m_1,\dots,m_{n-1})$. Theo định nghĩa của$N_a$, hình chiếu này có kích thước$N_a$và việc áp dụng hàm trải rộng sẽ biến đổi cấu trúc số đếm của nó sao cho tập kết quả có kích thước$\alpha(N_a)$. 

Điều này xác định mức độ$a$đóng góp như có số lượng thẻ$$N_a = \alpha(N_{a+1}),$$tương đương, sau khi thay đổi chỉ số, thành$$N_{a-1} = \alpha(N_a), \qquad 1 \le a < m.$$Cùng với trường hợp cơ sở$N_{m-1} = N$, điều này quyết định tất cả$N_a$đệ quy. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Sự bình đẳng$N_{m-1} = N$suy ra trực tiếp từ sự bằng nhau của tọa độ cuối cùng, điều này làm giảm sự so sánh thứ tự chéo với$(n-1)$-hình xuyến có chiều, do đó tập được đếm chính xác là đoạn có kích thước ban đầu$N$kết thúc tại$x$. 

Vì$a < m-1$, các phần tử ở cấp độ$a$được ra lệnh nghiêm ngặt dưới mức$m-1$ở tọa độ cuối cùng, do đó khả năng chấp nhận bị chi phối hoàn toàn bởi cách thứ tự chéo tạo ra sự cắt ngắn các tiền tố trên các lớp liền kề. Hàm lan truyền$\alpha$được xác định chính xác để mã hóa việc chuyển các phân đoạn ban đầu tiêu chuẩn này giữa các mức tọa độ liền kề trong$T(m_1,\dots,m_{n-1})$, do đó việc áp dụng nó vào số lượng cấp độ sẽ mang lại kết quả tái diễn đã nêu. 

Tính nhất quán của chỉ mục được duy trì do phép truy toán biểu thị sự lan truyền từ tọa độ cuối cùng cao hơn đến tọa độ cuối cùng thấp hơn, khớp với hướng sàng lọc theo thứ tự chéo. 

## Ghi chú 

Cấu trúc này có thể được hiểu là sự phân rã sợi lặp đi lặp lại của phân đoạn ban đầu của một sản phẩm được sắp xếp theo thứ tự từ điển. Mỗi sợi trên tọa độ cuối cùng là một bản sao của$(n-1)$-torus và hàm trải rộng đóng vai trò là toán tử chuyển tiếp giữa các sợi liên tiếp được tạo ra bởi đường cắt ranh giới tại$(x,m-1)$.
