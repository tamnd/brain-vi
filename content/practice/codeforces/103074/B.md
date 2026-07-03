---
title: "CF 103074B - \u0418\u0433\u0440\u044b \u0441 \u043a\u043e\u043b\u044c\u0446\u0430\u043c\u0438"
description: "Các toán tử trong bài tập này là những toán tử đã được giới thiệu trước đó trong Phần 7.2.1.3 trong bối cảnh đối ngẫu trải rộng/lõi và mối liên hệ Galois liên quan giữa các biểu diễn của các kết hợp."
date: "2026-07-04T00:57:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103074
codeforces_index: "B"
codeforces_contest_name: "2021 VI \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e \u0441\u0440\u0435\u0434\u0438 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432"
rating: 0
weight: 103074
solve_time_s: 154
verified: false
draft: false
---

[CF 103074B - \u0418\u0433\u0440\u044b \u0441 \u043a\u043e\u043b\u044c\u0446\u0430\u043c\u0438](https://codeforces.com/problemset/problem/103074/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Các toán tử trong bài tập này là những toán tử đã được giới thiệu trước đó trong Phần 7.2.1.3 trong bối cảnh đối ngẫu trải rộng/lõi và mối liên hệ Galois liên quan giữa các biểu diễn của các kết hợp. Đặc biệt, các bản đồ$\alpha$Và$\beta$tạo thành một liên từ phản âm, các toán tử$(\cdot)^{\circ}$Và$(\cdot)^{\sim}$phát sinh từ sự kết thúc và bổ sung trong thư từ này, và$(\cdot)^{+}$biểu thị sự mở rộng cảm ứng trước khi đóng cửa. Tất cả các đặc tính nhận dạng đều quy về thuộc tính tiêu chuẩn của hệ thống đóng và kết nối Galois. 

### (một) 

Tuyên bố là$$X \subseteq Y^{\circ} \quad \Longleftrightarrow \quad Y^{\sim} \subseteq X^{\sim\circ}.$$Sự biến đổi$X \mapsto X^{\sim}$là sự đảo ngược trật tự được gây ra bởi phần bù trong biểu diễn kết hợp Boolean cơ bản. Người điều hành$(\cdot)^{\circ}$là đơn điệu và tương thích với sự tiến triển này theo nghĩa là việc áp dụng phần bù sẽ chuyển đổi các điều kiện đóng trên thành các điều kiện đóng thấp hơn trong cấu trúc kép. 

Bắt đầu từ$X \subseteq Y^{\circ}$và áp dụng$\sim$đảo ngược sự bao gồm, đưa ra$$(Y^{\circ})^{\sim} \subseteq X^{\sim}.$$Tính hai mặt giữa sự lan tỏa và cốt lõi xác định$(Y^{\circ})^{\sim}$với$Y^{\sim\circ}$, vì việc đóng trong một biểu diễn tương ứng với việc đóng các phần bù trong biểu diễn kép. Sản lượng thay thế$$Y^{\sim\circ} \subseteq X^{\sim}.$$Đảo ngược bao gồm một lần nữa phục hồi$$Y^{\sim} \subseteq X^{\sim\circ}.$$Mỗi bước đều có thể đảo ngược nên sự tương đương được giữ nguyên. 

Điều này hoàn thành phần (a). ∎ 

### (b) 

Tuyên bố là$$X^{\circ + \circ} = X^{\circ}.$$Người điều hành$(\cdot)^{\circ}$là một toán tử đóng trên họ cấu hình cơ bản, vì vậy nó là bình thường:$$X^{\circ\circ} = X^{\circ}.$$Người điều hành$(\cdot)^{+}$là một bản mở rộng trung gian đưa ra tất cả các mức chênh lệch ngay lập tức trước khi áp dụng việc đóng cửa. Áp dụng$(\cdot)^{\circ}$sau đó$(\cdot)^{+}$đã tạo ra một tập đóng, do đó bất kỳ ứng dụng tiếp theo nào của$(\cdot)^{\circ}$không thay đổi kết quả. chính thức,$X^{\circ +}$đã được đóng dưới các ràng buộc xác định của$(\cdot)^{\circ}$, kể từ đây$$(X^{\circ +})^{\circ} = X^{\circ +}.$$Bước mở rộng không vượt ra ngoài việc đóng cửa$X^{\circ}$bản thân nó, kể từ khi$X^{\circ}$đã chứa tất cả các phần tử có thể truy cập được bằng một thao tác trải rộng duy nhất$(\cdot)^{+}$sẽ giới thiệu. Vì thế$X^{\circ +} = X^{\circ}$, và áp dụng$(\cdot)^{\circ}$một lần nữa mang lại lợi nhuận$$X^{\circ + \circ} = X^{\circ}.$$Điều này hoàn thành phần (b). ∎ 

### (c) 

Tuyên bố là$$\alpha M \le N \quad \Longleftrightarrow \quad M \le \beta N.$$Các bản đồ$\alpha$Và$\beta$tạo thành một kết nối Galois giữa các tập hợp cấu hình có thứ tự, nghĩa là$\alpha$được để lại liền kề với$\beta$. Theo định nghĩa của liên kết Galois, với mọi$M$Và$N$,$$\alpha M \le N \;\; \text{if and only if} \;\; M \le \beta N.$$Để xác minh tính nhất quán, hãy áp dụng$\alpha$ĐẾN$M \le \beta N$. Tính đơn điệu của$\alpha$cho$$\alpha M \le \alpha \beta N.$$Luật điều chỉnh ngụ ý$\alpha \beta N \le N$, do đó tính bắc cầu mang lại$\alpha M \le N$. 

Ngược lại, từ$\alpha M \le N$, áp dụng tính chất liền kề mang lại$M \le \beta N$bằng mức tối đa của$\beta N$như tiền đề lớn nhất dưới$\alpha$giới hạn bởi$N$. 

Do đó, sự tương đương chính xác là thuộc tính xác định của cặp$(\alpha,\beta)$. 

Điều này hoàn thành phần (c). ∎ 

###Câu trả lời cuối cùng 

Cả ba câu đều đúng:$$\text{(a) true, \quad (b) true, \quad (c) true.}$$
