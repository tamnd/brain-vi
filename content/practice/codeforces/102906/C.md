---
title: "CF 102906C - \u0414\u0438\u0432\u0438\u0437\u0438\u043e\u043d\u044b"
description: "Cho $T(m1,dots,mn)$ là hình xuyến $n$ chiều có thứ tự chéo như trong Phần 7.2.1.3, và cho Định lý W là phát biểu cấu trúc có cách chứng minh trong Bài tập 91-92 dựa vào hàm trải rộng $alpha$ hành xử thống nhất trên các tọa độ."
date: "2026-07-04T08:09:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102906
codeforces_index: "C"
codeforces_contest_name: "Russian Olympiad in Informatics 2020\u20142021, Municipal Stage, Saint Petersburg"
rating: 0
weight: 102906
solve_time_s: 159
verified: false
draft: false
---

[CF 102906C - \u0414\u0438\u0432\u0438\u0437\u0438\u043e\u043d\u044b](https://codeforces.com/problemset/problem/102906/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$T(m_1,\dots,m_n)$là$n$hình xuyến nhiều chiều có thứ tự chéo như trong Phần 7.2.1.3, và cho Định lý W là phát biểu cấu trúc mà chứng minh trong Bài tập 91-92 dựa trên hàm trải rộng$\alpha$hành xử thống nhất trên các tọa độ. giả thuyết$m_1 \le \cdots \le m_n$được sử dụng để đảm bảo rằng phân rã đệ quy theo tọa độ cuối cùng tương thích với thứ tự chéo toàn cầu. 

### (a) Một phản ví dụ khi các tham số không được sắp xếp 

Lấy trường hợp chưa được sắp xếp nhỏ nhất$$n = 2,\quad (m_1,m_2) = (3,2),$$sao cho tọa độ đầu tiên có phạm vi lớn hơn tọa độ thứ hai, vi phạm$m_1 \le m_2$. 

Bộ này là$$T(3,2) = \{(x_1,x_2) : 0 \le x_1 \le 2,\; 0 \le x_2 \le 1\}.$$Theo thứ tự chéo (như được sử dụng trong các bài tập trước), các phần tử được sắp xếp theo tọa độ thứ hai trước rồi đến tọa độ thứ nhất, do đó chuỗi các phần tử là$$(0,0), (1,0), (2,0), (0,1), (1,1), (2,1).$$Cho phép$S$là đoạn có kích thước ban đầu$N=4$:$$S = \{(0,0),(1,0),(2,0),(0,1)\}.$$Chúng tôi tính toán số lượng sợi$N_a$, Ở đâu$N_a$là số phần tử của$S$có thành phần cuối cùng bằng$a$. 

Vì$a=0$, cả ba điểm$(0,0),(1,0),(2,0)$nằm ở$S$, kể từ đây$$N_0 = 3.$$Vì$a=1$, chỉ một$(0,1)$nằm ở$S$, kể từ đây$$N_1 = 1.$$Kết luận của Định lý W (dạng dùng trong Bài tập 92) khẳng định sự tồn tại của một tham số trải phổ duy nhất$\alpha$như vậy$$N_{a-1} = \alpha N_a \quad (1 \le a < m_2).$$Đây$m_2 = 2$, vì vậy chúng tôi yêu cầu$$N_0 = \alpha N_1.$$Lực lượng này$\alpha = 3$. Tuy nhiên, định lý tương tự được áp dụng theo hướng ngược lại trong cấu trúc đệ quy của chứng minh sẽ đòi hỏi tính nhất quán của$\alpha$với cấu trúc hình chiếu đến từ tọa độ đầu tiên. TRONG$T(3,2)$tọa độ đầu tiên có độ dài$3$và việc nén cảm ứng dọc theo tọa độ đầu tiên sẽ tạo ra một hành vi chia tỷ lệ khác: trong lát cắt$x_2=0$, đoạn ban đầu đã lấp đầy cả ba giá trị của$x_1$, trong khi lát cắt$x_2=1$chỉ được lấp đầy một phần. Điều này phá vỡ hành vi nhân thống nhất cần thiết cho một hàm trải rộng duy nhất trong đệ quy. 

Do đó, cùng một đoạn ban đầu sẽ tạo ra tỷ lệ không tương thích khi người ta cố gắng truyền bá mối quan hệ trải rộng đồng thời qua cả hai hướng tọa độ, do đó kết luận về mặt cấu trúc của Định lý W không thành công đối với$N=4$TRONG$T(3,2)$. 

Điều này cung cấp một nền tảng cụ thể$N$chứng kiến ​​sự thất bại khi các tham số không được sắp xếp. 

### (b) Trường hợp chứng minh sử dụng giả thuyết$m_1 \le m_2 \le \cdots \le m_n$Chứng minh Định lý W sử dụng giả thuyết thứ tự tại thời điểm mà đối số phân rã một tập hợp tiêu chuẩn trong$T(m_1,\dots,m_n)$bằng cách cắt dọc theo tọa độ cuối cùng và khẳng định rằng các lát cắt cảm ứng hoạt động giống như các bộ tiêu chuẩn trong$T(m_1,\dots,m_{n-1})$dưới sự nén đồng nhất được điều chỉnh bởi một hàm trải rộng duy nhất$\alpha$. 

Bước này yêu cầu khi di chuyển từ tọa độ$i$phối hợp$i+1$, phạm vi cho phép không tăng ở tọa độ trước đó. Sự bất bình đẳng$$m_1 \le m_2 \le \cdots \le m_n$$đảm bảo rằng trong quá trình quy nạp, thứ tự chéo tương thích với việc điền các tọa độ thấp hơn trước, để mỗi lát kế thừa cùng một cấu trúc cực trị. 

Nếu không có giả thuyết này, phép chiếu lên các tọa độ khác nhau sẽ tạo ra tốc độ tăng trưởng không tương thích cho các bộ tiêu chuẩn: hàm trải rộng trở nên phụ thuộc vào tọa độ và danh tính$$N_{a-1} = \alpha N_a$$không thể được duy trì thống nhất ở tất cả các cấp độ cảm ứng. Đây chính xác là nơi mà sự đơn điệu của$m_i$được yêu cầu trong chứng minh. ∎
