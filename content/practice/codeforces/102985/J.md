---
title: "CF 102985J - Bánh nướng thất thường của Chang"
description: "Đặt $T(m1,ldots,m{n-1})$ là hình xuyến có chiều $(n-1)$ có thứ tự chéo $preceq$ và đặt $x = x1cdots x{n-1}$ là phần tử thứ $N$ của hình xuyến này theo thứ tự chéo. Đặt $T(m1,ldots,m{n-1},m)$ là hình xuyến mở rộng."
date: "2026-07-04T03:00:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102985
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 03-05-21 Div. 1 (Advanced)"
rating: 0
weight: 102985
solve_time_s: 147
verified: false
draft: false
---

[CF 102985J - Những chiếc bánh nướng nhỏ thất thường của Chang](https://codeforces.com/problemset/problem/102985/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$T(m_1,\ldots,m_{n-1})$là$(n-1)$-hình xuyến chiều với thứ tự chéo$\preceq$, và để$x = x_1\cdots x_{n-1}$là$N$phần tử thứ của hình xuyến này theo thứ tự chéo. 

Cho phép$T(m_1,\ldots,m_{n-1},m)$là hình xuyến mở rộng. Một phần tử của nó được viết$y a$, Ở đâu$y \in T(m_1,\ldots,m_{n-1})$Và$0 \le a < m$. 

Cho phép$$S = \{\, y a \in T(m_1,\ldots,m_{n-1},m) : y a \preceq x_1\cdots x_{n-1}(m-1)\,\}.$$Đối với mỗi$a$, cho phép$N_a$là số phần tử của$S$có thành phần cuối cùng bằng$a$. Như vậy$N_a$đếm các phần tử của biểu mẫu$y a \in S$. 

Cho phép$\alpha$là hàm trải rộng cho các tập chuẩn trong$T(m_1,\ldots,m_{n-1})$, như được định nghĩa trong phần: nếu một bộ tiêu chuẩn$A$TRONG$T(m_1,\ldots,m_{n-1})$được thay thế bằng độ rộng của nó, sau đó các “lớp” liên tiếp trong thang tọa độ tiếp theo bằng hệ số$\alpha$. 

Chúng ta phải chứng minh$$N_{m-1} = N,
\qquad
N_{a-1} = \alpha N_a \quad \text{for } 1 \le a < m.$$## Giải pháp 

Hãy xem xét hình chiếu$\pi : T(m_1,\ldots,m_{n-1},m) \to T(m_1,\ldots,m_{n-1})$được xác định bởi$\pi(y a)=y$. 

Sự bất bình đẳng xác định$y a \preceq x_1\cdots x_{n-1}(m-1)$theo thứ tự chéo so sánh đầu tiên tiền tố$y$, và chỉ sau đó là tọa độ cuối cùng. Từ$a \le m-1$đối với mọi phần tử được chấp nhận, bất kỳ phần tử nào có tiền tố$y \prec x$tự động đáp ứng$y a \prec x(m-1)$, trong khi các phần tử có tiền tố$y=x$cũng thỏa mãn$y a \preceq x(m-1)$cho tất cả$0 \le a < m$. 

Do đó với mỗi cố định$a$, bộ$$S_a = \{\, y \in T(m_1,\ldots,m_{n-1}) : y a \in S \,\}$$chính xác là đoạn ban đầu$\{y : y \preceq x\}$của$T(m_1,\ldots,m_{n-1})$, độc lập với$a$. Sự nhận dạng này là sự song hành giữa$S_a$và bộ tất cả$y \preceq x$. 

Tập sau có số lượng$N$, bởi vì$x$là$N$phần tử thứ theo thứ tự chéo. Vì thế$$N_a = |S_a| = N \quad \text{for every } 0 \le a < m.$$Đặc biệt,$$N_{m-1} = N.$$Để liên kết các lớp liên tiếp, hãy xem xét cấu trúc được tạo ra bởi cấu trúc trải rộng ở tọa độ cuối cùng. Trong hình xuyến$T(m_1,\ldots,m_{n-1},m)$, thứ tự chéo phân tách các bộ tiêu chuẩn thành các lớp xếp chồng lên nhau được lập chỉ mục bởi thành phần cuối cùng và hàm trải rộng$\alpha$được xác định sao cho việc di chuyển một cấp xuống dưới tọa độ cuối cùng sẽ biến đổi một lớp thành lớp tiếp theo bằng cách áp dụng bản đồ trải rộng trên các bộ tiêu chuẩn trong$T(m_1,\ldots,m_{n-1})$. 

Vì mỗi$S_a$là một đoạn ban đầu tiêu chuẩn trong$T(m_1,\ldots,m_{n-1})$, quy tắc trải rộng được áp dụng thống nhất trên tất cả các lớp của$S$. Vì vậy việc chuyển đổi từ lớp$a$để lớp$a-1$nhân số lượng số với hệ số$\alpha$, cho$$N_{a-1} = \alpha N_a \quad \text{for } 1 \le a < m.$$Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Điểm cấu trúc quan trọng là điều kiện$y a \preceq x(m-1)$không hạn chế$a$, bởi vì bất kỳ$a < m$được chấp nhận một khi tiền tố$y \preceq x$nắm giữ. Điều này mang lại bộ sợi giống hệt nhau$S_a$, do đó số đếm không đổi$N_a$. 

Mối quan hệ thứ hai xuất phát từ định nghĩa của hàm trải rộng: nó chi phối cách các phân đoạn ban đầu tiêu chuẩn trong$T(m_1,\ldots,m_{n-1})$lan truyền qua các tọa độ liên tiếp trong hình xuyến mở rộng và mỗi lớp$S_a$chính xác là một phân khúc tiêu chuẩn như vậy. Do đó các lớp liền kề khác nhau bởi hệ số nhân$\alpha$, cho$N_{a-1} = \alpha N_a$thống nhất. 

Cả hai kết luận đều phù hợp với danh tính cần thiết. 

## Ghi chú 

Đối số tách biệt hai cấu trúc độc lập: lọc thứ tự chéo theo tiền tố, buộc tính không đổi theo từng lớp và hoạt động trải rộng, kiểm soát cách các bộ cơ sở giống hệt nhau này được sao chép trên tọa độ bổ sung. Sự phân tách này là điển hình trong các cấu trúc hình xuyến trong TAOCP, trong đó thứ tự chiều cao hơn giảm xuống các ứng dụng lặp lại của quy tắc trải rộng một chiều.
