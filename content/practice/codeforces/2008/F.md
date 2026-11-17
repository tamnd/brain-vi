---
title: "CF 2008F - Chiếc Hộp Của Sakurako"
description: "Đây là một vấn đề loại B (chứng minh). Yêu cầu là chứng minh rằng với mọi điểm $P$ trong mặt phẳng và với mọi cách chọn đỉnh phân biệt của hình thang cân thì tổng khoảng cách từ $P$ đến ba đỉnh còn lại đều lớn hơn khoảng cách…"
date: "2026-06-08T13:25:38+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2008
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 970 (Div. 3)"
rating: 1400
weight: 2008
solve_time_s: 42
verified: false
draft: false
---

[CF 2008F - Hộp của Sakurako](https://codeforces.com/problemset/problem/2008/F) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 42s 
**Đã xác minh:** không 

## Giải pháp 
## Kiểm tra loại sự cố 

Đây là vấn đề **Loại B (chứng minh)**. 

Yêu cầu là phải chứng minh với mọi điểm$P$trong mặt phẳng và với mọi cách chọn đỉnh phân biệt của hình thang cân, tổng khoảng cách từ$P$tới ba đỉnh còn lại lớn hơn khoảng cách từ$P$đến đỉnh phân biệt. 

## Xác minh từng bước 

**Bước 1: Cho$ABCD$là một hình thang cân với$AB\parallel CD$Và$AB>CD$. - HỢP LỆ.** 

Đây là một sự bình thường hóa hợp pháp. 

**Bước 2: Chia các đỉnh thành hai loại, đỉnh có đáy ngắn và đỉnh có đáy dài. - HỢP LỆ.** 

Đây là một chiến lược chứng minh hợp lý. 

**Bước 3: Đối với đỉnh có đáy ngắn hơn, sử dụng**$PA+PB\ge AB$Và$PD-PC\le CD.$**- HỢP LỆ.** 

Đây là các bất đẳng thức tam giác chuẩn và tam giác ngược. 

**Bước 4: Suy ra**$PA+PB+PC-PD\ge AB-(PD-PC)\ge AB-CD>0.$**- HỢP LỆ.** 

Kể từ khi$AB>CD$, điều này chứng minh đúng$PA+PB+PC>PD.$Lập luận tương tự chứng minh bất đẳng thức tương ứng cho$C$. 

**Bước 5: Cho đỉnh trên đáy dài hơn$A$, sử dụng**$PC+PD\ge CD$Và$|PA-PD|\le AD.$**- HỢP LỆ.** 

Cả hai bất đẳng thức đều đúng. 

**Bước 6: Yêu cầu**$PB+PC+PD-PA\ge PC+PD-(PA-PB)\ge CD-AD>0.$**- SAI (Lỗi nghiêm trọng).** 

Sự bất bình đẳng được hiển thị không tuân theo các ước tính trước đó. 

Từ$|PA-PD|\le AD$người ta có thể suy luận$PA-PD\le AD,$nhưng điều này không kiểm soát được số lượng$PA-PB.$Bằng chứng âm thầm thay thế$PA-PB$bởi một cái gì đó giới hạn bởi$AD$, điều đó là không hợp lý. 

Thật vậy, ước tính$PC+PD-(PA-PB)\ge CD-AD$sẽ yêu cầu$PA-PB\le AD,$và không có sự bất bình đẳng nào như vậy được thiết lập. 

**Bước 7: Xác nhận rằng$CD-AD>0$. - SAI (Lỗi nghiêm trọng).** 

Bằng chứng khẳng định rõ ràng tính khẳng định chặt chẽ của$CD-AD.$Điều này nói chung là sai. 

Đối với hình thang cân có tọa độ$A=(-5,0),\quad B=(5,0),\quad D=(-1,4),\quad C=(1,4),$chúng tôi có$CD=2,\qquad AD=\sqrt{32}\approx5.66,$Vì thế$CD-AD<0.$Vì vậy, ngay cả khi bước trước đó là hợp lý thì kết luận vẫn thất bại. 

**Bước 8: Suy ra trường hợp đỉnh$B$bằng sự đối xứng. - KHÔNG CHÍNH XÁC.** 

Đối số đối xứng phụ thuộc vào tính hợp lệ của Bước 6. Vì Bước 6 không hợp lệ nên việc khấu trừ này không được hỗ trợ. 

**Bước 9: Kết luận rằng cả 4 trường hợp đỉnh đều được chứng minh. - SAI (Lỗi nghiêm trọng).** 

Chỉ những trường hợp tương ứng với đỉnh$C$Và$D$thực tế đã được thành lập. 

Các trường hợp tương ứng$A$Và$B$vẫn chưa được chứng minh. 

## Kiểm tra tính đầy đủ 

Bằng chứng là không đầy đủ. 

Lập luận cho các đỉnh trên cơ sở ngắn hơn là chính xác và đầy đủ. 

Lập luận cho các đỉnh trên cơ sở dài hơn có hai khiếm khuyết nghiêm trọng độc lập: 

1. Nó sử dụng bất đẳng thức liên quan đến$PA-PB$không tuân theo bất đẳng thức tam giác ngược áp dụng cho$A,D,P$. 
2. Nó dựa vào tuyên bố sai lầm rằng$CD-AD>0$. 

Vì những sai sót này nên sự bất bình đẳng$PB+PC+PD>PA$Và$PA+PC+PD>PB$không được chứng minh. 

Do đó, kết luận cuối cùng không tuân theo những gì đã thực sự được thiết lập. 

## Tóm tắt 

Chứng minh xử lý thành công các đỉnh trên cơ sở ngắn hơn nhưng thất bại hoàn toàn đối với các đỉnh trên cơ sở dài hơn. Ước tính chính cho đỉnh$A$không hợp lệ và nó còn phụ thuộc vào bất đẳng thức sai$CD>AD$. 

XÁC MINH: THẤT BẠI - đối số của bằng chứng cho các đỉnh trên cơ sở dài hơn là không hợp lệ vì nó giới hạn một cách không chính đáng$PA-PB$và giả định không chính xác$CD-AD>0$.
