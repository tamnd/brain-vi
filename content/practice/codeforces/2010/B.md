---
title: "CF 2010B - Ba Anh Em"
description: "Đây là một vấn đề Loại B (bằng chứng). Để thành công, giải pháp phải thiết lập chặt chẽ các đồng nhất thức vectơ đã nêu cho: 1. Một tam giác đều. 2. Một $n$-giác chính quy tùy ý. 3. Một tứ diện đều."
date: "2026-06-08T13:14:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2010
codeforces_index: "B"
codeforces_contest_name: "Testing Round 19 (Div. 3)"
rating: 800
weight: 2010
solve_time_s: 71
verified: false
draft: false
---

[CF 2010B - Ba anh em](https://codeforces.com/problemset/problem/2010/B) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Kiểm tra loại sự cố 

Đây là một vấn đề Loại B (bằng chứng). 

Để thành công, giải pháp phải thiết lập chặt chẽ các danh tính vectơ đã nêu cho: 

1. Một tam giác đều. 
2. Một quy tắc tùy ý$n$-gon. 
3. Một tứ diện đều. 

Một bằng chứng của định luật chung$n$trường hợp -gon cùng với chuyên môn$n=3$được chấp nhận ở phần (1) và (2). Cần có một chứng minh riêng cho tứ diện. 

## Xác minh từng bước 

### Bước 1: Đạo hàm của$$K_i=M+(r-u_i\cdot M)u_i$$Và$$\overrightarrow{MK_i}=(r-u_i\cdot M)u_i$$- HỢP LỆ 

Mặt bên hoặc mặt được thể hiện bằng$$u_i\cdot x=r.$$Viết$$K_i=M+t_i u_i$$và thay thế vào phương trình của cạnh hoặc mặt sẽ cho$$u_i\cdot M+t_i=r,$$từ$|u_i|=1$. Kể từ đây$$t_i=r-u_i\cdot M.$$Công thức sau đây chính xác. 

### Bước 2: Công thức tính tổng$$\sum \overrightarrow{MK_i} = r\sum u_i-\sum (u_i\cdot M)u_i$$- HỢP LỆ 

Điều này là trực tiếp từ tuyến tính. 

### Bước 3: Đối với thông thường$n$-gon,$$\sum_{i=1}^n u_i=0$$- HỢP LỆ 

Các pháp tuyến bên ngoài là các vectơ đơn vị cách đều nhau trên đường tròn. Tổng vectơ của chúng bằng không. 

Câu lệnh này dành cho đa giác đều, do đó$n\ge 3$. Kiểm tra các giá trị nhỏ: 

cho$n=3$,$n=4$, Và$n=5$, khẳng định là đúng. 

### Bước 4: Định nghĩa$$T(x)=\sum_i (u_i\cdot x)u_i$$và giảm xuống$$\sum_i\overrightarrow{MK_i}=-T(M)$$- HỢP LỆ 

Đây là cách viết lại trực tiếp Bước 2 bằng Bước 3. 

### Bước 5:$T$đi lại bằng phép quay bằng$2\pi/n$- KHÔNG CHÍNH XÁC 

Phân loại: **Khoảng cách biện minh** 

Bằng chứng nêu rõ: 

> Theo tính đối xứng,$T$đi lại với chuyển động quay qua$2\pi/n$. 

Điều này đúng nhưng không có đối số nào được đưa ra trong phần Giải pháp. 

Một bằng chứng hoàn chỉnh sẽ chỉ ra rằng phép quay hoán vị các chuẩn tắc và sau đó tính toán$$T(Rx)=RT(x).$$Tuyên bố này là tiêu chuẩn và gần như chắc chắn là có chủ đích, nhưng nó chưa được chứng minh trên thực tế. 

### Bước 6: Đối xứng$2\times2$ma trận đi lại với một phép quay không cần thiết là bội số vô hướng của danh tính 

- KHÔNG CHÍNH XÁC 

Phân loại: **Khoảng cách biện minh** 

Tuyên bố này là đúng. 

Tuy nhiên, giải pháp chỉ khẳng định điều đó: 

> Đối xứng$2\times2$ma trận đi lại với một phép quay không cần thiết là bội số vô hướng của danh tính. 

Không có bằng chứng nào được cung cấp. 

Một đối số chặt chẽ sẽ tính toán giao hoán của ma trận quay một cách rõ ràng hoặc viện dẫn một thực tế đại số tuyến tính đã được thiết lập trước đó. 

### Bước 7: Tính toán vết$$\operatorname{tr}(T) = \sum_i \operatorname{tr}(u_i u_i^T) = n$$và do đó$$T=\frac n2 I$$- HỢP LỆ 

Giả sử Bước 6, tính toán theo dõi là chính xác. 

### Bước 8: Trừ số thường xuyên$n$-gon danh tính 

- KHÔNG HOÀN THÀNH 

Phân loại: **Lỗi nghiêm trọng** 

Bằng chứng kết thúc bằng$$T=\frac n2 I.$$Ngay sau đó bản thảo kết thúc với$$\text{“Consequently }\su”$$Lập luận không bao giờ thực sự xuất phát$$\sum_i \overrightarrow{MK_i} = \frac n2\,\overrightarrow{MO},$$nó cũng không rút ra được trường hợp tam giác một cách rõ ràng từ$n=3$. 

Kết luận dự định đã rõ ràng từ công việc trước đó, nhưng nó không có trong bằng chứng được đệ trình. 

### Bước 9: Lập luận bất khả quy cho tứ diện 

- KHÔNG ĐẠT ĐƯỢC GIẢI PHÁP 

Phần Kiến trúc Chứng minh chứa một lập luận có thể rút gọn hợp lý. 

Tuy nhiên, phần Giải pháp thực tế bị cắt bớt trước khi việc chứng minh tứ diện bắt đầu. 

Một kiến ​​trúc bằng chứng không phải là một bằng chứng. Chỉ có phần Giải pháp thiết lập kết quả. 

### Bước 10: Nhận dạng tứ diện 

- KHÔNG CHỨNG MINH 

Phân loại: **Lỗi nghiêm trọng** 

Đối số tứ diện không bao giờ xuất hiện trong phần Giải pháp vì phần chứng minh đã bị cắt bỏ trước đó. 

Danh tính cần thiết$$\overrightarrow{MK_1}+\overrightarrow{MK_2} +\overrightarrow{MK_3}+\overrightarrow{MK_4} = \frac43\,\overrightarrow{MO}$$không bao giờ được thiết lập. 

## Kiểm tra tính đầy đủ 

Giải pháp đề xuất chưa đầy đủ. 

thường xuyên$n$-gon bằng chứng được cắt ngắn trước khi khấu trừ cuối cùng. Kết luận tam giác đều không bao giờ có được một cách rõ ràng. Chứng minh tứ diện hoàn toàn không có trong phần Giải pháp. 

Ngoài ra còn có hai khẳng định chưa được chứng minh trong lập luận đa giác:$$TR=RT$$và 

> đối xứng$2\times2$ma trận đi lại với một phép quay không cần thiết là bội số vô hướng của danh tính. 

Đây là những khoảng trống biện minh. 

Vấn đề nghiêm trọng hơn là bằng chứng được đệ trình thực sự dừng lại trước khi hoàn thành lập luận về đa giác và trước khi đưa ra bất kỳ bằng chứng tứ diện nào. 

Các kết luận cuối cùng không tuân theo Giải pháp bằng văn bản vì thiếu các dẫn xuất cần thiết. 

## Tóm tắt 

Cách tiếp cận toán học là hợp lý và dường như đang hướng tới một bằng chứng chính xác. Đối số tối giản được phác thảo trong Kiến trúc Chứng minh có thể sẽ sửa chữa vấn đề đối xứng tứ diện tồn tại trong các phiên bản trước. 

Tuy nhiên, Giải pháp được gửi chưa đầy đủ. Nó kết thúc ở giữa phép chứng minh đa giác và không bao giờ chứa chứng minh tứ diện. Như đã viết, các tuyên bố cần thiết không được chứng minh. 

KIỂM TRA: THẤT BẠI - lời giải chưa đầy đủ, kết thúc trước khi hoàn thành việc chứng minh đa giác và trước khi chứng minh trường hợp tứ diện.
