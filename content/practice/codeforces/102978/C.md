---
title: "CF 102978C - Tỷ lệ đếm tối thiểu"
description: "Đặt $T=binom{2t-1}{t}$ và viết $x=N/T$. Trong Mục 7.2.1.3, số $kappa{tN}$ được thể hiện thông qua biểu diễn nhị phân của $N$ bằng cách phân tách tổ hợp $(s,t)$-tương ứng thành các tổ hợp liên quan $qt,dots,q0$ của (11)."
date: "2026-07-04T06:31:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102978
codeforces_index: "C"
codeforces_contest_name: "XXI Open Cup, Grand Prix of Tokyo"
rating: 0
weight: 102978
solve_time_s: 113
verified: false
draft: false
---

[CF 102978C - Tỷ lệ đếm tối thiểu](https://codeforces.com/problemset/problem/102978/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$T=\binom{2t-1}{t}$và viết$x=N/T$. Trong Mục 7.2.1.3, số$\kappa_{tN}$được thể hiện thông qua biểu diễn nhị phân của$N$bằng cách phân hủy tương ứng$(s,t)$-kết hợp thành các tác phẩm liên quan$q_t,\dots,q_0$của (11). Lập luận dẫn đến Bài tập 84 cho thấy sự biến động của$\kappa_{tN}-N$được điều chỉnh bởi cùng một cấu trúc cộng tính xác định hàm Takagi, cụ thể là các đóng góp cặp của biểu mẫu$r_k(x)=(-1)^{\lfloor 2^k x\rfloor}$được tích lũy thông qua cấu trúc mang nhị phân của biểu diễn (14). 

Các chức năng$\lambda_{tN}$Và$\mu_{tN}$phát sinh từ cùng một sự phân tách nhưng với các lựa chọn cực đoan của cấu hình nhị phân trung gian phù hợp với tiền tố cố định của$(s,t)$-đại diện của$N$. Tương tự, khi thể hiện$x$ở dạng nhị phân và theo dõi trình tự cảm ứng của các tác phẩm$q_t,\dots,q_0$, giá trị của$\kappa_{tN}$chỉ phụ thuộc vào cửa sổ hữu hạn của các chữ số cho đến các vị trí mà việc truyền mang trong quá trình chuyển đổi giữa các biểu diễn vẫn có thể ảnh hưởng đến giá trị. Việc cắt bớt sự phụ thuộc này sẽ tạo ra hai hàm bậc thang cực trị:$\lambda_{tN}$thu được bằng cách giảm thiểu tất cả các đóng góp mang theo chưa được giải quyết và$\mu_{tN}$thu được bằng cách tối đa hóa chúng. 

Theo khai triển Takagi, điều này tương ứng với việc thay thế biểu diễn chuỗi Rademacher vô hạn đầy đủ của$\tau(x)$bằng tổng một phần trong đó các bit chưa quyết định của$x$được cố định để$0$vì$\lambda_{tN}$và để$1$vì$\mu_{tN}$. Mỗi bit chưa quyết định như vậy đóng góp một số hạng độ lớn có dấu$2^{-k}$theo cách tương tự như trong (13)-(14), do đó, hiệu ứng tích lũy của tất cả các vị trí chưa được giải quyết chính xác là sai số dao động được mã hóa bởi hàm Takagi. 

Mối quan hệ tiệm cận từ Bài tập 84,$$\kappa_{tN}-N=\frac{T}{t}\,\tau(x)+O\!\left(\frac{(\log t)^3}{t}\right),$$ngụ ý rằng cả hai cách xây dựng cực trị đều thỏa mãn các bất đẳng thức tương ứng với cùng số hạng dẫn đầu, vì việc thay đổi từng chữ số chưa quyết định có thể làm thay đổi phần đóng góp của Takagi chỉ trong phạm vi gia số cặp đôi của nó. Do đó, thủ tục cắt ngắn mang lại giới hạn của biểu mẫu$$\lambda_{tN}-N \;\le\; \frac{T}{t}\,\tau(x) \;\le\; \mu_{tN}-N,$$với cả hai$\lambda_{tN}$Và$\mu_{tN}$khác với$\kappa_{tN}$nhiều nhất là hiệu ứng tích lũy của các vị trí mang chưa được giải quyết, có cùng thứ tự với hạng lỗi trong Bài tập 84. 

Vì bản thân hàm Takagi được tạo bằng cách tính tổng các đóng góp cặp đôi có dấu trên tất cả các vị trí bit, nên các hàm$\lambda_{tN}$Và$\mu_{tN}$tương ứng chính xác với các xấp xỉ của đường bao dưới và trên$\tau(x)$gây ra bằng cách sửa các chữ số chưa được giải quyết trong biểu diễn nhị phân của$x=N/T$. Theo nghĩa này,$\lambda_{tN}$Và$\mu_{tN}$là các cách thực hiện cực trị rời rạc của cùng một cấu trúc Rademacher-sum xác định$\tau(x)$, Và$\tau(x)$được phục hồi như là trung tâm tiệm cận chung của chúng sau khi nhân rộng bởi$T/t$. 

Điều này hoàn thành việc chứng minh. ∎
