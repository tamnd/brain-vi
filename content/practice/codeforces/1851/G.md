---
title: "CF 1851G - Vlad và những ngọn núi"
description: "Giải pháp này trả lời đúng bài tập: nó cố gắng mở rộng Bổ đề B từ tích phân $c$ đến $c$ thực tùy ý với $0le c<k$, và nó đề xuất một công thức tương hỗ rõ ràng. Tuy nhiên, bằng chứng không chứng minh được công thức mà nó tuyên bố."
date: "2026-06-09T17:21:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dsu", "graphs", "implementation", "sortings", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1851
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 888 (Div. 3)"
rating: 2000
weight: 1851
solve_time_s: 122
verified: false
draft: false
---

[CF 1851G - Vlad và những ngọn núi](https://codeforces.com/problemset/problem/1851/G) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dsu, đồ thị, triển khai, sắp xếp, cây, hai con trỏ 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Tính đúng đắn 

Lời giải trả lời đúng bài tập: nó cố gắng mở rộng Bổ đề B từ tích phân$c$thực tế tùy ý$c$với$0\le c<k$, và nó đề xuất một công thức có đi có lại rõ ràng. 

Tuy nhiên, bằng chứng không chứng minh được công thức mà nó tuyên bố. Khó khăn chính trong việc tổng quát hóa Bổ đề B là xác định chính xác các số hạng biên phát sinh từ hàm răng cưa thay đổi như thế nào khi$c$không còn tách rời nữa. Lời giải chỉ đơn giản là xác nhận dạng của những hiệu chỉnh đó mà không lấy được chúng. Kết quả là công thức hiển thị (8) không được thiết lập. 

Hơn nữa, việc kiểm tra tính nhất quán cuối cùng với trường hợp tích phân là không chính xác. Việc quy giản được tuyên bố thành Bổ đề B không thực sự tái tạo lại công thức tích phân đã nêu trong mọi trường hợp. 

Do đó, bằng chứng không đầy đủ và không thể được chấp nhận như là một dẫn xuất hợp lệ của luật tương hỗ tổng quát. 

## Khoảng trống và lỗi 

### 1. Lỗi nghiêm trọng: Phương trình (3) được khẳng định, không dẫn xuất 

Bằng chứng nêu rõ$$R(h,k,c) = R(h,k,0) +\frac{6c^{2}}{hk} -6\Bigl\lfloor\frac ch\Bigr\rfloor +B(c). \tag{3}$$Điều này được trình bày như một điều gì đó xuất phát từ việc "tuân theo chính xác đạo hàm của Bổ đề B". 

Không có dẫn xuất được đưa ra. 

Toàn bộ vấn đề là xác định xem công thức tương hỗ thay đổi như thế nào khi$c$trở thành hiện thực. Sự xuất hiện của các điều khoản$$\frac{6c^{2}}{hk} \qquad\text{and}\qquad -6\Bigl\lfloor\frac ch\Bigr\rfloor$$phải được chứng minh từ khai triển thực tế. Chúng không thể được nhập đơn giản từ trường hợp tích hợp. 

Vì (3) là nền tảng cho mọi điều tiếp theo nên lập luận này không đầy đủ. 

### 2. Lỗi nghiêm trọng: Việc hiệu chỉnh ranh giới$B(c)$được đoán thay vì được tính toán 

Bằng chứng khẳng định: 

> Mỗi điểm ranh giới như vậy góp phần$-3$đến tổng tương hỗ. 

và kết luận$$B(c) = -3\,\delta(c) -3\,\delta(r) +3.$$Không có tính toán nào được đưa ra. 

Bằng chứng tương hỗ của Bổ đề B bao gồm số điểm mạng tinh tế. Thật sự$c$, các vị trí mà các đối số răng cưa trở thành tích phân phải được phân tích rõ ràng. Giải pháp không bao giờ thực hiện phân tích này. 

Thay vào đó, nó xác định hai điều kiện biên có thể có và ấn định một đóng góp$-3$với mỗi người bằng sự khẳng định. 

Đây chính xác là hiện tượng mới phải được thiết lập trong quá trình khái quát hóa, vì vậy việc bỏ sót sẽ rất tai hại. 

### 3. Lỗi nghiêm trọng: Không có bằng chứng nào cho thấy điều kiện biên duy nhất là$c\in\mathbb Z$Và$r\in\mathbb Z$Bằng chứng nêu rõ:$$c\in\mathbb Z, \qquad r=c-qh\in\mathbb Z.$$và tuyên bố đây là nguồn điều chỉnh ranh giới duy nhất. 

Không có lời biện minh nào được đưa ra. 

Đối số có đi có lại thực tế liên quan đến tính toàn vẹn của các biểu thức xảy ra bên trong các hàm răng cưa. Nó không cho thấy rằng các điều kiện này giảm chính xác đến hai chỉ báo được hiển thị. 

Do đó, tính đúng đắn của công thức (5) vẫn chưa được chứng minh. 

### 4. Lỗi nghiêm trọng: Việc rút gọn về bổ đề B chưa được kiểm chứng chính xác 

Các bằng chứng khẳng định$$-3-3[c\bmod h=0] = -3e(h,c)+3.$$Cho phép$c=0$. 

Khi đó vế trái bằng$$-3-3=-6,$$trong khi$$e(h,0)=1,$$vậy vế phải bằng$$-3(1)+3=0.$$Hai bên không bằng nhau. 

Do đó danh tính được hiển thị là sai. 

Vì đồng nhất thức này được sử dụng để khôi phục Bổ đề B từ sự khái quát hóa được đề xuất nên việc kiểm tra tính nhất quán không thành công. 

### 5. Lỗi nghiêm trọng: Sự tương đương được tuyên bố với Bổ đề B không tuân theo 

Bởi vì đồng nhất thức trước là sai nên phương trình (9) không rút gọn về dạng đã nêu của Bổ đề B. 

Do đó, giải pháp không thực hiện được bước xác minh của chính nó. 

Bất kỳ sự khái quát hóa được đề xuất nào cũng phải tái tạo chính xác công thức tích phân đã biết. Bằng chứng không chứng minh được điều này. 

### 6. Khoảng cách biện minh: Sử dụng$c \bmod h$thực sự$c$ký hiệu$$c\bmod h$$được sử dụng thực sự$c$. 

Mặc dù nó có thể được hiểu là$$c-h\Bigl\lfloor\frac ch\Bigr\rfloor,$$bằng chứng nên xác định điều này một cách rõ ràng trước khi sử dụng các hàm chỉ báo như$$\delta(c\bmod h).$$Vấn đề này là nhỏ so với các lỗi cơ bản ở trên. 

## Tóm tắt 

Giải pháp đã giải quyết được bài tập đúng, nhưng phân tích giới hạn quan trọng không bao giờ được thực hiện. Thời hạn điều chỉnh được đề xuất$B(c)$được khẳng định chứ không phải dẫn xuất, và phép kiểm tra cuối cùng đối với dạng tích phân đã biết của Bổ đề B chứa đựng mâu thuẫn đại số trực tiếp. 

XÁC MINH: THẤT BẠI - công thức tương hỗ được đề xuất không được suy ra và việc quy giản nó thành Bổ đề B là không chính xác về mặt toán học.
