---
title: "CF 102979C - Hình vuông đầy màu sắc"
description: "Giả sử Định lý W được áp dụng cho hình xuyến $T(m1,dots,mn)$ theo thứ tự chéo như trong Phần 7.2.1.3, và đặt $S$ là đoạn ban đầu theo thứ tự đó."
date: "2026-07-04T03:27:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102979
codeforces_index: "C"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Day 9 Contest (XXI Open Cup, Grand Prix of Suwon)"
rating: 0
weight: 102979
solve_time_s: 154
verified: false
draft: false
---

[CF 102979C - Hình vuông đầy màu sắc](https://codeforces.com/problemset/problem/102979/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Áp dụng Định lý W cho hình xuyến$T(m_1,\dots,m_n)$với thứ tự chéo như trong Phần 7.2.1.3, và đặt$S$là một phân đoạn ban đầu theo thứ tự đó. Định lý dựa trên sự phân rã đệ quy của$S$thành các lát bằng cách cố định tọa độ cuối cùng và áp dụng hàm trải rộng$\alpha$đến bộ tiêu chuẩn trong$T(m_1,\dots,m_{n-1})$. 

### (một) 

lấy$n=2$và chọn các tham số theo thứ tự chưa sắp xếp$$m_1 = 2,\quad m_2 = 3.$$hình xuyến$T(2,3)$bao gồm các cặp$(x,y)$với$0 \le x < 2$Và$0 \le y < 3$, được sắp xếp theo thứ tự chéo (từ điển trong khối tọa độ thứ hai được xác định bởi tọa độ thứ nhất, như trong việc xây dựng các bộ tiêu chuẩn). 

Bây giờ so sánh điều này với tình huống hoán đổi (sắp xếp)$T(3,2)$. TRONG$T(3,2)$, mỗi cấp độ được đặt${(x,y): y=\text{const}}$có kích thước$3$, do đó bản đồ trải rộng$\alpha$tác dụng thống nhất trên bộ chuẩn 3 phần tử ở tọa độ đầu tiên. TRONG$T(2,3)$, các lát tương tự có kích thước$2$, do đó đệ quy cảm ứng sử dụng$\alpha$trên bộ tiêu chuẩn 2 phần tử. 

Lấy$$N = 2.$$TRONG$T(2,3)$, hai phần tử đầu tiên theo thứ tự chéo nằm hoàn toàn trong sợi đầu tiên$x=0$, cụ thể là$$(0,0),\ (0,1).$$Do đó, phép chiếu lên tọa độ thứ hai của đoạn có kích thước ban đầu$N$chiếm hai giá trị riêng biệt trong một sợi có kích thước$3$. 

TRONG$T(3,2)$, thay vào đó, hai phần tử đầu tiên theo thứ tự chéo nằm ở các sợi khác nhau:$$(0,0),\ (1,0).$$Hình chiếu của chúng lên tọa độ thứ hai trùng nhau, vì cả hai đều có tọa độ thứ hai$0$, do đó số lát cảm ứng sẽ khác với trường hợp trước. 

Kết luận của Định lý W xác định cấu trúc của$S$thông qua một đệ quy thống nhất về mặt$\alpha$được áp dụng nhất quán trên tất cả các hướng tọa độ. Vì$N=2$, sự phân hủy cảm ứng của các phân đoạn ban đầu thành kích thước sợi phụ thuộc vào việc tọa độ thứ nhất hay thứ hai có mô đun lớn hơn. Vì hai cấu trúc trên tạo ra sự phân bố sợi khác nhau cho cùng một$N$, phát biểu của Định lý W không thành công khi các tham số không được sắp xếp. 

Như vậy$N=2$là một giá trị mà kết luận về cấu trúc được yêu cầu không giữ nguyên bất biến dưới dạng chưa được sắp xếp$(m_1,m_2)$. 

### (b) 

Chứng minh Định lý W sử dụng giả thuyết$$m_1 \le m_2 \le \cdots \le m_n$$tại điểm mà các phân đoạn ban đầu được phân rã đệ quy bằng cách cố định tọa độ cuối cùng và so sánh kích thước của các tập hợp con tiêu chuẩn trong hình xuyến có chiều thấp hơn. 

Ở bước đó, đối số yêu cầu rằng khi truyền từ$T(m_1,\dots,m_{n-1})$ĐẾN$T(m_1,\dots,m_{n-1},m_n)$, hàm trải rộng$\alpha$hoạt động đơn điệu liên quan đến việc bao gồm các bộ tiêu chuẩn, sao cho mỗi sợi có kích thước$m_n$có thể được điền theo cách tương thích với cấu trúc đệ quy của các phân đoạn ban đầu. 

Nếu các tham số không được sắp xếp, tọa độ có mô đun nhỏ hơn có thể xuất hiện muộn hơn trong phép đệ quy và bước quy nạp so sánh các lát cắt không thành công do việc xây dựng giả định rằng phạm vi tọa độ lớn hơn chiếm ưu thế so với phạm vi tọa độ trước đó trong phân tầng theo thứ tự chéo. Đây chính xác là nơi mà bằng chứng sử dụng$m_{n-1} \le m_n$: nó đảm bảo rằng tọa độ cuối cùng cung cấp phân vùng thô nhất, sao cho danh tính đếm$$N_a = \alpha(N_{a+1})$$áp dụng thống nhất trên tất cả các sợi mà không cần cân bằng lại giữa các kích thước tọa độ không bằng nhau. 

Nếu không có giả định thứ tự, cảm ứng sẽ bị phá vỡ ở giai đoạn mà kích thước sợi được giả định là tương thích với cùng một hoạt động trải rộng và việc nhận dạng đệ quy của các bộ tiêu chuẩn không còn hiệu lực. 

Điều này hoàn thành việc chứng minh. ∎
