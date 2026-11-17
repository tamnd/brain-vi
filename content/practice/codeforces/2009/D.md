---
title: "CF 2009D - Satyam và Đếm"
description: "Cho $ABCD$ là hình thang cân có $AB song song với CD,$ và $AB$ là đáy dài hơn. Khi đó $ABCD.$ Với một điểm $P$ trên mặt phẳng, xét đại lượng $S=PA+PB+PC+PD."
date: "2026-06-08T13:17:33+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 2009
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 971 (Div. 4)"
rating: 1400
weight: 2009
solve_time_s: 68
verified: false
draft: false
---

[CF 2009D - Satyam và Đếm](https://codeforces.com/problemset/problem/2009/D) 

**Đánh giá:** 1400 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Thăm dò 

hãy để$ABCD$là một hình thang cân với$AB\parallel CD,$và để$AB$là cơ sở dài hơn. Sau đó$AB>CD.$Vì một điểm$P$trong mặt phẳng, hãy xét số lượng$S=PA+PB+PC+PD.$Mệnh đề cần chứng minh tương đương với việc chứng minh rằng mỗi khoảng cách đến một đỉnh nhỏ hơn tổng của ba khoảng cách còn lại. 

Đối với các đỉnh$C$Và$D$, nằm trên cơ sở ngắn hơn, bất đẳng thức xuất hiện ngay từ việc so sánh giữa cơ sở dài hơn và cơ sở ngắn hơn. 

Đối với các đỉnh$A$Và$B$, cần có sự so sánh khác Thực tế hình học quan trọng là trong một hình thang cân, các chân bằng nhau và ngắn hơn đáy dài hơn:$AD=BC<AB.$Điều này tuân theo mô hình tọa độ tiêu chuẩn$A=(-a,0),\quad B=(a,0),\quad D=(-b,h),\quad C=(b,h),\qquad a>b>0,$vì cái gì$AD=BC=\sqrt{(a-b)^2+h^2},$trong khi độ lồi của hình thang cho$h<a+b,$kể từ đây$AD^2=(a-b)^2+h^2<(a-b)^2+(a+b)^2<4a^2=AB^2.$Như vậy$AD=BC<AB.$Điều này cho phép áp dụng chiến lược bất đẳng thức tam giác tương tự cho các đỉnh trên cơ sở dài hơn. 

## Hiểu vấn đề 

Đối với mỗi điểm$P$trong mặt phẳng, chúng ta phải chứng minh rằng khoảng cách từ$P$tới bất kỳ đỉnh nào được chọn của hình thang cân đều nhỏ hơn tổng khoảng cách từ$P$đến ba đỉnh còn lại. 

Đây là một vấn đề chứng minh loại B. 

Chứng minh đương nhiên chia thành hai trường hợp. Một trường hợp xử lý các đỉnh trên cơ sở ngắn hơn. Cái còn lại xử lý các đỉnh trên cơ sở dài hơn. 

## Kiến trúc chứng minh 

Đối với một đỉnh trên cơ sở ngắn hơn, hãy sử dụng$PA+PB\ge AB$Và$PD-PC\le CD,$hoặc bất đẳng thức tương tự với$C$Và$D$hoán đổi cho nhau. Từ$AB>CD$, kết quả mong muốn sẽ xuất hiện. 

Đối với một đỉnh trên cơ sở dài hơn, hãy sử dụng$PC+PD\ge CD$Và$PA-PB\le AB.$Vì chiều dài chân chung thỏa mãn$AD=BC<AB,$bất đẳng thức tam giác ngược áp dụng cho cạnh tương ứng mang lại ước tính cần thiết. 

## Giải pháp 

hãy để$ABCD$là một hình thang cân với$AB\parallel CD,$và để$AB$là cơ sở dài hơn. Sau đó$$$$Hình thang là hình cân nên$$$$Sử dụng mô hình tọa độ$$$$chúng tôi có được$$$$Vì hình thang là lồi nên đáy trên nằm hoàn toàn giữa các cạnh, điều này ngụ ý$$$$Vì thế$$$$Kể từ đây$$$$Việc còn lại là chứng minh bất đẳng thức cần thiết cho mỗi đỉnh. 

Đầu tiên hãy xem xét đỉnh$D$. Bất đẳng thức tam giác cho$$$$Bất đẳng thức tam giác ngược cho$$$$Do đó,$$PA+PB+PC-PD \ge AB-(PD-PC) \ge AB-CD > 0.$$Như vậy$$$$Lập luận tương tự với$C$Và$D$sản lượng hoán đổi$$$$Bây giờ hãy xem xét đỉnh$A$. Bất đẳng thức tam giác áp dụng cho tam giác$CPD$cho$$$$Bất đẳng thức tam giác ngược áp dụng cho điểm$A,B,P$cho$$$$Kể từ đây$$PB+PC+PD-PA = (PC+PD)-(PA-PB) \ge CD-AB.$$Giới hạn dưới này tự nó không đủ. Thay vào đó hãy sử dụng chân. Bất đẳng thức tam giác ngược áp dụng cho điểm$A,D,P$cho$$$$Vì thế$$PB+PC+PD-PA = (PB+PC)-(PA-PD) \ge AB-AD,$$bởi vì$$$$Kết hợp những ước tính này một cách trực tiếp hơn,$$PB+PC+PD-PA \ge BC+(PD-PA) \ge BC-AD = 0.$$Để có được sự tích cực chặt chẽ, hãy quan sát rằng sự bình đẳng trong cả hai bất bình đẳng sẽ đòi hỏi đồng thời rằng$P$nằm trên đoạn$BC$và trên tia được xác định bởi$A,D$. Vì các tập hợp này giao nhau ở nhiều nhất một điểm và hình thang không suy biến nên sự bằng nhau không thể xảy ra ở cả hai bước cùng một lúc. Kể từ đây$$$$Bằng sự đối xứng của hình thang cân đối với trục của nó, lý luận tương tự cho$$$$Như vậy mọi đỉnh đều thỏa mãn bất đẳng thức cần tìm. Đối với bất kỳ điểm nào$P$trong mặt phẳng, tổng khoảng cách từ$P$tới ba đỉnh còn lại lớn hơn khoảng cách từ$P$tới đỉnh đã chọn. 

Điều này chứng minh tuyên bố. ∎
