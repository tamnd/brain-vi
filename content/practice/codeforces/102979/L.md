---
title: "CF 102979L - Đèn Trên Đường"
description: "Định lý W được chứng minh trong Mục 7.2.1.3 với giả định rằng các tham số $m1 le m2 le cdots le mn$."
date: "2026-07-04T03:44:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102979
codeforces_index: "L"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Day 9 Contest (XXI Open Cup, Grand Prix of Suwon)"
rating: 0
weight: 102979
solve_time_s: 149
verified: false
draft: false
---

[CF 102979L - Ánh đèn trên đường](https://codeforces.com/problemset/problem/102979/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Định lý W được chứng minh ở Mục 7.2.1.3 với giả định rằng các tham số$m_1 \le m_2 \le \cdots \le m_n$. Kết luận mô tả cấu trúc đệ quy thống nhất các đoạn chéo trong hình xuyến$T(m_1,\dots,m_n)$, được biểu thị thông qua hàm trải rộng$\alpha$đó là độc lập với chỉ số lát cắt. Bài tập 92 tách biệt bất biến khóa dùng trong chứng minh: cho tập chuẩn$S \subseteq T(m_1,\dots,m_{n-1},m)$, kích thước sợi$N_a$thỏa mãn$$N_{m-1}=N,\qquad N_{a-1}=\alpha N_a \quad (1 \le a < m),$$Ở đâu$\alpha$chỉ phụ thuộc vào bộ tiêu chuẩn trong$T(m_1,\dots,m_{n-1})$. 

### (a) Phản ví dụ khi tham số không được sắp xếp 

lấy$n=2$và các tham số chưa được sắp xếp$(m_1,m_2)=(3,2)$. Hình xuyến là$$T(3,2)=\{(i,j)\mid 0\le i<3,\ 0\le j<2\}.$$Liệt kê các phần tử của nó theo thứ tự chéo như được tạo ra trong cách xây dựng được sử dụng trong Định lý W (sẽ giảm xuống thứ tự từ điển khi áp dụng tọa độ trong chứng minh):$$(0,0),(0,1),(1,0),(1,1),(2,0),(2,1).$$Cho phép$S$là đoạn ban đầu bao gồm đoạn đầu tiên$N=3$các yếu tố:$$S=\{(0,0),(0,1),(1,0)\}.$$Xác định số lượng sợi liên quan đến tọa độ thứ hai (vai trò của tọa độ cuối cùng trong bước quy nạp của Định lý W):$$N_a = |\{(i,a)\in S\}|.$$Sau đó$$N_0=2,\qquad N_1=1.$$Nếu kết luận của Định lý W đúng mà không cần có thứ tự thì sẽ tồn tại một hằng số$\alpha$như vậy$$N_0 = \alpha N_1.$$Lực lượng này$\alpha=2$. 

Bây giờ hãy mở rộng thêm một bước nữa trong cùng cách xây dựng phân đoạn ban đầu để$N=4$:$$S'=\{(0,0),(0,1),(1,0),(1,1)\}.$$Sau đó$$N_0'=2,\qquad N_1'=2.$$Quy tắc tương tự sẽ yêu cầu$N_0'=\alpha N_1'$, kể từ đây$\alpha=1$. 

Sự mâu thuẫn$\alpha=2$Và$\alpha=1$cho thấy rằng không có tham số trải phổ đơn nào có thể thỏa mãn kết luận của Định lý W cho tất cả các phân đoạn ban đầu trong$T(3,2)$. Do đó định lý không thành công khi các tham số không ở thứ tự không giảm, và$N=3$đã cung cấp nhân chứng nơi cấu trúc bị phá vỡ. 

### (b) Trường hợp chứng minh sử dụng giả thuyết thứ tự 

Chứng minh Định lý W sử dụng điều kiện$$m_1 \le m_2 \le \cdots \le m_n$$tại điểm mà các lát cắt của một tiêu chuẩn được đặt vào$T(m_1,\dots,m_n)$được so sánh sau khi chiếu lên các trục con tọa độ. 

Trong bước quy nạp, đối số giả định rằng khi tọa độ được cố định ở mức$a$, cấu trúc còn lại hoạt động giống như một bộ tiêu chuẩn trong một hình xuyến nhỏ hơn với hàm trải rộng được xác định rõ ràng, không phụ thuộc vào tọa độ nào được chọn cuối cùng. Sự độc lập này dựa trên thực tế là việc mở rộng hướng tọa độ không tạo ra “nút cổ chai” nhỏ hơn các tọa độ trước đó, do đó, các đối số nén và thứ tự chéo duy trì tính đơn điệu của cấu trúc sợi. 

Khi giả thuyết thứ tự bị loại bỏ, bằng chứng sẽ bị phá vỡ chính xác ở bước mà người ta xác định được hàm trải phổ đều$\alpha$trên mọi tọa độ. Nếu một số$m_i > m_{i+1}$, sau đó chiếu lên tọa độ$i$và phối hợp$i+1$mang lại tốc độ tăng trưởng không tương thích cho các lát tương ứng và nhận dạng quy nạp của một$\alpha$thất bại. Điều này làm mất hiệu lực quá trình chuyển đổi từ hành vi nén cục bộ trong$T(m_1,\dots,m_{n-1})$đến sự tái diễn toàn cầu đối với$T(m_1,\dots,m_n)$. 

Điều này hoàn thành việc chứng minh. ∎
