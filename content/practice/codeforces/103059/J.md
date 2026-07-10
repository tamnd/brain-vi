---
title: "CF 103059J - Mã hóa cuộn"
description: "Gọi $C$ là tập hợp tất cả các tổ hợp $t$-$ct dots c2 c1$ của ${0,1,dots,n-1}$, được viết theo thứ tự giảm dần như trong (3). Bổ đề S liên quan đến thứ tự được sử dụng ngầm định trong Mục 7.2.1."
date: "2026-07-04T01:20:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103059
codeforces_index: "J"
codeforces_contest_name: "UTPC Spring 2021 Open Contest"
rating: 0
weight: 103059
solve_time_s: 144
verified: false
draft: false
---

[CF 103059J - Mã hóa cuộn](https://codeforces.com/problemset/problem/103059/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$C$biểu thị tập hợp tất cả$t$-sự kết hợp$c_t \dots c_2 c_1$của${0,1,\dots,n-1}$, được viết theo thứ tự giảm dần như trong (3). Bổ đề S liên quan đến thứ tự được sử dụng ngầm định trong Phần 7.2.1.3 khi các kết hợp được duyệt dưới dạng từ điển trong khi các biểu diễn kép của chúng tiến hóa theo hướng ngược lại. Trật tự chéo là cơ chế kết hợp hai khung nhìn này sao cho một thay đổi cục bộ trong một biểu diễn tương ứng với một thay đổi đơn điệu được kiểm soát trong biểu diễn kia. 

Viết$c \in C$BẰNG$c = (c_t,\dots,c_1)$và để$b = (b_s,\dots,b_1)$là biểu diễn vị trí kép của nó (5), trong đó$b_i$liệt kê các chỉ số bổ sung. Đặc điểm xác định của trật tự chéo là hai chuỗi được so sánh theo các hướng từ điển trái ngược nhau: tăng dần về$c$-tọa độ tương ứng với sự giảm dần trong$b$-tọa độ và ngược lại. Sự ghép nối này chính xác là thứ khiến cho lệnh bị “vượt qua”. 

Bổ đề S khẳng định rằng thứ tự chéo gây ra sự mở rộng tuyến tính hợp lệ của mối quan hệ kề cận tiềm ẩn được sử dụng trong quá trình tạo, theo nghĩa là các đối tượng liên tiếp khác nhau theo cách tối thiểu được kiểm soát nhất quán với cấu trúc kế tiếp trong Thuật toán L. 

Để hoàn thành chứng minh, xét hai tổ hợp liên tiếp$c = (c_t,\dots,c_1)$Và$c' = (c'_t,\dots,c'_1)$theo thứ tự chéo. Bằng cách xây dựng hệ từ điển trong (L3)-(L5), chỉ mục$j$được chọn là vị trí ngoài cùng bên phải nơi có thể tăng dần, vì vậy$c_j$được tăng lên$c'_j = c_j + 1$. Cho tất cả$i < j$, thuật toán đặt lại$c_i = i-1$, kể từ đây$c'_i = i-1$. Cho tất cả$i > j$, không có thay đổi nào xảy ra, vì vậy$c'_i = c_i$. Điều này cho thấy quá trình chuyển đổi ảnh hưởng đến một khối hậu tố duy nhất trong$c$-biểu diễn, với thiết lập lại xác định bắt buộc bên dưới chỉ số trục. 

Trong biểu diễn kép, cùng một sửa đổi tương ứng với một mức giảm được kiểm soát duy nhất trong$b$-tọa độ, vì tăng$c_j$loại bỏ chính xác một phần tử khỏi phần bù và dịch chuyển cấu trúc phần bù còn lại sang trái theo thứ tự (5). Kể từ khi$b$-trình tự được sắp xếp ngày càng trái ngược với$c$, sự thay đổi vị trí$j$chỉ lan truyền cục bộ và không làm xáo trộn các tọa độ trước đó theo thứ tự kép. Do đó, chuỗi kép phát triển đơn điệu theo hướng ngược lại. 

Sự ghép nối này ngụ ý rằng mỗi bước của thứ tự chéo tương ứng với một bước di chuyển cơ bản được chấp nhận duy nhất trong chính xác một biểu diễn trong khi vẫn duy trì các ràng buộc thứ tự trong biểu diễn kia. Không có sự mơ hồ nào có thể nảy sinh ở người kế nhiệm, bởi vì việc lựa chọn$j$bị ép buộc bởi sự vi phạm đầu tiên của điều kiện cực đại$c_i + 1 = c_{i+1}$và điều kiện này chỉ phụ thuộc vào so sánh cục bộ của các phần tử liền kề. Do đó người kế vị được xác định duy nhất. 

Sự hữu ích của trật tự chéo bắt nguồn từ địa phương này. Vì mỗi lần chuyển đổi chỉ sửa đổi một chỉ mục$c_j$và đặt lại hậu tố về cấu hình tối thiểu của nó, cấu trúc của không gian tìm kiếm sẽ phân tách thành các khối được sắp xếp theo thứ tự từ điển rời rạc được lập chỉ mục bởi$j$. Biểu diễn kép đảm bảo rằng các khối này khớp với nhau mà không bị chồng chéo hoặc thiếu sót, vì tọa độ bổ sung được duyệt theo thứ tự từ điển ngược. Điều này ngăn cản việc xem lại các cấu hình và đảm bảo rằng việc truyền tải do Lemma S tạo ra bao gồm mọi kết hợp chính xác một lần trong khi vẫn duy trì khái niệm nhất quán về tính kề cận. 

Do đó, trật tự chéo rất hữu ích vì nó chuyển đổi một bài toán liệt kê tổ hợp toàn cục thành một chuỗi các cập nhật được kiểm soát cục bộ trong tọa độ kép, và đây chính xác là cơ chế hoàn thành việc chứng minh Bổ đề S. 

Điều này hoàn thành việc chứng minh. ∎
