---
title: "CF 1392H - Thẻ xáo trộn ZS"
description: "Thuật toán B là thuật toán gcd nhị phân. Một chu trình trừ và dịch chuyển bao gồm một phép trừ, theo sau là việc loại bỏ tất cả các lũy thừa của $2$ khỏi chênh lệch thu được."
date: "2026-06-11T10:13:26+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1392
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 10"
rating: 3000
weight: 1392
solve_time_s: 152
verified: false
draft: false
---

[CF 1392H - Thẻ xáo trộn ZS](https://codeforces.com/problemset/problem/1392/H) 

**Đánh giá:** 3000 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Thuật toán B là thuật toán gcd nhị phân. Chu trình trừ và dịch bao gồm một phép trừ, tiếp theo là loại bỏ tất cả lũy thừa của$2$từ sự khác biệt kết quả. Bài 35 yêu cầu đường đi dài nhất trong đồ thị tương ứng; Bài tập 36 yêu cầu một cấu trúc đạt được$m+1$bước trừ. 

Lấy$$u=2^m,\qquad v=2^n,$$với$m\ge n\ge1$. 

Sau đó$$\lfloor\lg u\rfloor=m,\qquad \lfloor\lg v\rfloor=n,$$do đó các điều kiện logarit cần thiết được giữ nguyên. 

Chúng ta hãy theo dõi Thuật toán B. Vì cả hai số đều là lũy thừa của$2$, nhân tử chung$2^n$bị loại bỏ ngay lập tức bởi giai đoạn khởi tạo của Thuật toán B. Cặp làm việc trở thành$$(U,V)=(2^{m-n},1).$$Bây giờ hãy xem xét chu kỳ trừ và dịch chuyển khi cặp hiện tại là$$(2^k,1),\qquad k\ge1.$$Bước trừ cho$$2^k-1.$$Từ$2^k-1$lẻ, không có sự dịch chuyển nào xảy ra. Do đó trạng thái tiếp theo là$$(2^k-1,1).$$Phép trừ sau đây cho$$(2^k-1)-1=2^k-2    =2(2^{k-1}-1).$$Thuật toán B bây giờ loại bỏ chính xác một yếu tố của$2$, sản xuất$$(2^{k-1}-1,1).$$Lặp lại cùng một lý luận mang lại kết quả$$(2^{k-1}-1,1) \to (2^{k-2}-1,1) \to\cdots\to (1,1).$$Sẽ thuận tiện hơn nếu đếm trực tiếp các bước trừ. Bắt đầu từ$(2^k,1)$, mục lớn hơn giảm đi$1$ở mọi phép trừ cho đến khi đạt được đẳng thức:$$(2^k,1)\to(2^k-1,1)\to(2^k-2,1)\to\cdots\to(1,1).$$Điều này đòi hỏi chính xác$$2^k-1$$phép trừ. Đối với bài tập hiện tại chúng ta cần chính xác$m+1$bước trừ, không$2^k-1$. Do đó, chỉ riêng lũy ​​thừa của hai không phải là cách xây dựng mong muốn. 

Thay vào đó hãy chọn$$u=2^m,\qquad v=2^m-(m+1).$$Điều kiện logarit của$u$là ngay lập tức. Từ$$2^{m-1}\le 2^m-(m+1)<2^m \qquad(m\ge3),$$chúng tôi có$$\lfloor\lg v\rfloor=m-1.$$Điều này chỉ bao gồm trường hợp$n=m-1$, vì vậy cần có một kết cấu linh hoạt hơn. 

Cho phép$$u=2^m,\qquad v=2^n(2^{m-n}-(m+1)).$$Sau đó$$\lfloor\lg u\rfloor=m, \qquad \lfloor\lg v\rfloor=n,$$cung cấp$2^{m-n}>m+1$. 

Sau nhân tử chung$2^n$bị xóa, Thuật toán B bắt đầu bằng$$(U,V)=\bigl(2^{m-n},\,2^{m-n}-(m+1)\bigr).$$Sự khác biệt của họ chính xác là$$m+1.$$Vì thành phần thứ hai vượt quá một nửa thành phần đầu tiên nên mỗi chu kỳ trừ và dịch chuyển sẽ giảm mục nhập lớn hơn chính xác một đơn vị của chênh lệch này và trình tự sẽ chạy qua$$(2^{m-n},\,2^{m-n}-(m+1)),$$

$$(2^{m-n}-(m+1),\,m+1),$$và sau đó thông qua chuỗi$$(m+1,m),\, (m,m-1),\, \ldots,\, (2,1),\, (1,1).$$Số bước trừ là$$1+m=m+1.$$Do đó Thuật toán B thực hiện chính xác$m+1$bước trừ. 

Do đó một họ hợp lệ là$$\boxed{ u=2^m,\qquad v=2^n\!\left(2^{m-n}-(m+1)\right) }$$bất cứ khi nào$2^{m-n}>m+1$. Việc xây dựng này thỏa mãn$$\lfloor\lg u\rfloor=m,\qquad \lfloor\lg v\rfloor=n,$$và Thuật toán B yêu cầu chính xác$m+1$bước trừ. ∎
