---
title: "CF 2011H - Ma Trận Kỳ Lạ"
description: "Đây là một vấn đề Loại B (bằng chứng). Nhiệm vụ yêu cầu là chứng minh các đồng dạng vectơ đã nêu cho: 1. Tam giác đều. 2. Một $n$-giác thông thường. 3. Một tứ diện đều. Một giải pháp hợp lệ phải tự thiết lập danh tính."
date: "2026-06-08T13:11:48+07:00"
tags: ["codeforces", "competitive-programming", "*special", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2011
codeforces_index: "H"
codeforces_contest_name: "Kotlin Heroes: Episode 11"
rating: 0
weight: 2011
solve_time_s: 47
verified: false
draft: false
---

[CF 2011H - Ma trận kỳ lạ](https://codeforces.com/problemset/problem/2011/H) 

**Đánh giá:** - 
**Tags:** *đặc biệt, đồ thị, tham lam 
**Thời gian giải:** 47s 
**Đã xác minh:** không 

## Giải pháp 
## Kiểm tra loại sự cố 

Đây là vấn đề **Loại B (bằng chứng)**. 

Nhiệm vụ được yêu cầu là chứng minh các đồng nhất thức vectơ đã nêu cho: 

1. Một tam giác đều. 
2. Thường xuyên$n$-gon. 
3. Một tứ diện đều. 

Một giải pháp hợp lệ phải tự thiết lập danh tính. Không phát sinh yêu cầu phân loại, tối ưu hóa hoặc xây dựng. 

Giải pháp đề xuất cố gắng chứng minh tính quy luật tổng quát$n$câu lệnh -gon, suy ra trường hợp tam giác như$n=3$, rồi chứng minh riêng trường hợp tứ diện. Chiến lược đó là phù hợp. 

## Xác minh từng bước 

### Bước 1: Đạo hàm của$$\overrightarrow{MK_i}=(r-u_i\cdot M)u_i$$Giải pháp viết đường hoặc mặt phẳng hỗ trợ là$$u_i\cdot x=r$$và biểu thị chân đường vuông góc là$$K_i=M+t_i u_i.$$Thay thế vào phương trình đường thẳng hoặc mặt phẳng sẽ cho$$t_i=r-u_i\cdot M.$$Kể từ đây$$\overrightarrow{MK_i}=(r-u_i\cdot M)u_i.$$**Trạng thái:** HỢP LỆ. 

Tính toán này đúng cho cả cài đặt đa giác và tứ diện. 

### Bước 2: Công thức tính tổng 

Tổng kết các kết quả nhận dạng trước đó$$\sum_i \overrightarrow{MK_i} = r\sum_i u_i - \sum_i (u_i\cdot M)u_i.$$**Trạng thái:** HỢP LỆ. 

Điều này suy ra trực tiếp từ tính tuyến tính. 

### Bước 3: Đối với thông thường$n$-gon,$$\sum_i u_i=0$$Các pháp tuyến đơn vị bên ngoài cách đều nhau xung quanh vòng tròn đơn vị. 

**Trạng thái:** HỢP LỆ. 

Đối với một thường xuyên$n$-gon với$n\ge3$, tổng vectơ của các hướng cách đều nhau này thực sự bằng không. 

### Bước 4: Định nghĩa toán tử 

Giải pháp xác định$$T(x)=\sum_i (u_i\cdot x)u_i$$và viết lại số tiền mong muốn là$$\sum_i \overrightarrow{MK_i} = -T(M).$$**Trạng thái:** HỢP LỆ. 

### Bước 5:$T$di chuyển với sự quay của góc$2\pi/n$Đối số sử dụng thực tế là phép quay hoán vị các chuẩn mực. 

**Trạng thái:** HỢP LỆ. 

Tính toán được hiển thị hiển thị chính xác$$TR=RT.$$### Bước 6: Khấu trừ$T=\lambda I$trong trường hợp đa giác 

Giải pháp nêu rõ rằng một thực tế$2\times2$ma trận đi lại với một phép quay qua một góc khác với$0$Và$\pi$phải có hình thức$$\begin{pmatrix} a&-b\\ b&a \end{pmatrix}.$$Từ$T$là đối xứng, do đó$b=0$, Vì thế$$T=\lambda I.$$**Trạng thái:** HỢP LỆ. 

Đối với đa giác đều,$n\ge3$, kể từ đây$2\pi/n\neq0,\pi$. 

### Bước 7: Vẽ vết tính toán trong trường hợp đa giác 

Giải pháp tính toán$$\operatorname{tr}(T) = \sum_i \operatorname{tr}(u_i u_i^T) = \sum_i |u_i|^2 = n.$$Từ$T=\lambda I$trong không gian hai chiều,$$2\lambda=n.$$Vì thế$$T=\frac n2 I.$$**Trạng thái:** HỢP LỆ. 

### Bước 8: Trừ số thường xuyên$n$-gon danh tính 

Thay thế$T=(n/2)I$cho$$\sum_i \overrightarrow{MK_i} = -\frac n2 M.$$Vì gốc tọa độ đặt tại$O$,$$\overrightarrow{MO}=-M.$$Kể từ đây$$\sum_i \overrightarrow{MK_i} = \frac n2\,\overrightarrow{MO}.$$**Trạng thái:** HỢP LỆ. 

### Bước 9: Đạo hàm trường hợp tam giác đều 

Cài đặt$n=3$cho$$\sum_{i=1}^3 \overrightarrow{MK_i} = \frac32\,\overrightarrow{MO}.$$**Trạng thái:** HỢP LỆ. 

### Bước 10: Đối với khối tứ diện,$$\sum_i u_i=0$$Giải pháp gọi sự đối xứng tứ diện. 

**Trạng thái:** HỢP LỆ. 

Các pháp tuyến bên ngoài của bốn mặt được sắp xếp đối xứng và tổng của chúng bằng không. 

### Bước 11: Định nghĩa toán tử tứ diện 

Giải pháp một lần nữa xác định$$T(x)=\sum_{i=1}^4 (u_i\cdot x)u_i$$và thu được$$\sum_{i=1}^4 \overrightarrow{MK_i} = -T(M).$$**Trạng thái:** HỢP LỆ. 

### Bước 12: Chứng minh rằng nhóm đối xứng tứ diện tác dụng bất khả quy lên$\mathbb R^3$Giải pháp nêu rõ: 

> Nhóm quay của tứ diện tác dụng tối giản lên$\mathbb R^3$. 

và sau đó áp dụng bổ đề Schur. 

**Trạng thái:** KHÔNG CHÍNH XÁC. 

**Phân loại:** Khoảng cách biện minh. 

Tuyên bố này đúng, nhưng lời giải không chứng tỏ được tính tối giản. 

Đây là một bổ đề bị bỏ qua đáng kể. Để viện dẫn bổ đề Schur, trước tiên phải thiết lập tính tối giản. Lời giải không cung cấp bằng chứng về tính tối giản cũng như không đưa ra một lập luận trực tiếp thay thế nào. 

Khoảng cách có thể sửa chữa được. Ví dụ, người ta có thể chỉ ra rằng bất kỳ không gian con bất biến khác 0 nào cũng sẽ chứa quỹ đạo của một vectơ khác 0 trong nhóm tứ diện, và quỹ đạo đó trải rộng trên tất cả$\mathbb R^3$. Ngoài ra, người ta có thể bỏ qua hoàn toàn lý thuyết biểu diễn và tính toán trực tiếp bằng cách sử dụng tọa độ cho bốn chuẩn mực. 

### Bước 13: Khấu trừ$$T=\lambda I$$Kết luận rút ra từ bổ đề Schur chỉ sau khi tính tối giản được thiết lập. 

**Trạng thái:** KHÔNG CHÍNH XÁC. 

**Phân loại:** Khoảng cách biện minh. 

Kết luận đúng nhưng điều kiện tiên quyết để áp dụng bổ đề Schur chưa được chứng minh. 

### Bước 14: Vẽ vết tính toán trong trường hợp tứ diện 

Giả sử$T=\lambda I$,$$\operatorname{tr}(T) = 4, \qquad 3\lambda=4, \qquad \lambda=\frac43.$$**Trạng thái:** HỢP LỆ. 

### Bước 15: Nhận dạng tứ diện cuối cùng 

sử dụng$$T=\frac43 I,$$giải pháp thu được$$\sum_{i=1}^4 \overrightarrow{MK_i} = \frac43\,\overrightarrow{MO}.$$**Trạng thái:** HỢP LỆ với điều kiện Bước 12 và 13 hợp lý. 

## Kiểm tra tính đầy đủ 

Bằng chứng đa giác thông thường là đầy đủ và nghiêm ngặt. 

Việc chứng minh tứ diện chưa hoàn chỉnh. Lập luận dựa trên phát biểu rằng nhóm quay tứ diện tác dụng tối giản lên$\mathbb R^3$, sau đó viện dẫn bổ đề Schur. Cả yêu cầu về khả năng tối giản cũng như lập luận thay thế trực tiếp đều không được cung cấp. 

Công thức hiển thị cuối cùng cho khối tứ diện cũng bị cắt bớt trong văn bản đã gửi, nhưng phần thảo luận trước đó đã làm rõ kết luận dự định. Vấn đề toán học thực sự là thiếu sự biện minh cho tính tối giản. 

Không có trường hợp nào khác được bỏ qua. thường xuyên$n$Câu lệnh -gon dành cho$n\ge3$, nên không có vấn đề gì với$n=1$hoặc$n=2$. 

## Tóm tắt 

Phần đa giác là hoàn toàn chính xác. 

Lập luận tứ diện vẫn chứa đựng khoảng trống cơ bản tương tự như đã được xác định trong phần đánh giá trước đó. Lời giải thay thế lập luận đối xứng trước đó bằng lời kêu gọi bổ đề Schur, nhưng nó không chứng minh được giả thuyết cần thiết rằng biểu diễn phép quay tứ diện trên$\mathbb R^3$là không thể giảm được. Do đó việc khấu trừ đó$T$là bội số vô hướng của danh tính không được thiết lập trong bằng chứng. 

KẾT LUẬN: THẤT BẠI - chứng minh tứ diện viện dẫn bổ đề Schur mà không chứng minh được tính tối giản cần thiết của tác dụng quay tứ diện trên$\mathbb R^3$.
