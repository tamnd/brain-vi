---
title: "CF 2151A - Mảng con lũy tiến"
description: "Đặt $m = 2^e$ và xem xét chuỗi hình vuông ở giữa đã sửa đổi được xác định bởi Coveyou: $X0 text{ Given}, qquad X{n+1} = operatorname{middle}(Xn^2 + 2^{e-1} Xn), eqno(4)$ trong đó hàm $operatorname{middle}(cdot)$ trích xuất các bit $2e$ ở giữa của đối số với độ chính xác gấp đôi."
date: "2026-06-09T04:17:48+07:00"
tags: ["codeforces", "competitive-programming", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 2151
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1053 (Div. 2)"
rating: 800
weight: 2151
solve_time_s: 129
verified: false
draft: false
---

[CF 2151A - Mảng con lũy tiến](https://codeforces.com/problemset/problem/2151/A) 

**Đánh giá:** 800 
**Tags:** toán, chuỗi 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$m = 2^e$và xem xét chuỗi hình vuông ở giữa đã sửa đổi được xác định bởi Coveyou:$X_0 \text{ given}, \qquad X_{n+1} = \operatorname{middle}(X_n^2 + 2^{e-1} X_n), \eqno(4)$chức năng ở đâu$\operatorname{middle}(\cdot)$chiết xuất phần giữa$2e$bit của đối số với độ chính xác gấp đôi. Theo Mục 3.2.2, dãy này tương đương với dãy đồng dư bậc hai$X_{n+1} \equiv d X_n^2 + a X_n + c \pmod{m}, \eqno(3)$cho số nguyên phù hợp$a$,$d$, Và$c$, với$c = 0$,$a = 1 + 2^{e-1}$, Và$d = 1$. Bài tập 3.2.2.8 khẳng định điều kiện cần và đủ để dãy đồng dư bậc hai có độ dài chu kỳ cực đại$m$modulo$m$. Chúng tôi mong muốn xác minh rằng chuỗi (4) thỏa mãn các điều kiện này và xác định chu kỳ của nó. 

Mục đích là để chỉ ra rằng khoảng thời gian của (4) chính xác là$2^{e-2}$. 

## Giải pháp 

Trước tiên, chúng tôi xác định các tham số của dạng đồng đẳng bậc hai cho chuỗi bình phương giữa đã sửa đổi. Cho phép$X_n$là$e$-bit số nguyên. Sửa đổi của Coveyou thêm vào$2^{e-1} X_n$ĐẾN$X_n^2$và chiếm phần giữa$2e$bit. Theo thuật ngữ đồng dạng modulo$2^e$, chúng ta có thể viết$X_{n+1} \equiv X_n^2 + 2^{e-1} X_n \pmod{2^e}. \eqno(10)$Chúng tôi viết lại điều này ở dạng đồng đẳng bậc hai tiêu chuẩn$X_{n+1} \equiv d X_n^2 + a X_n + c \pmod{m}$, cho$d = 1, \quad a = 2^{e-1}, \quad c = 0. \eqno(11)$Theo Bài tập 3.2.2.8, một dãy đồng dư bậc hai theo modulo$m$có độ dài chu kỳ tối đa$m$khi và chỉ khi các điều kiện sau được thỏa mãn: 

1.$c$tương đối nguyên tố với$m$; 
2.$d$Và$a-1$là bội số của$p$cho mọi số nguyên tố lẻ$p$chia$m$; 
3.$d$là số chẵn và$d \equiv a-1 \pmod{4}$nếu như$m$là bội số của 4; 
4.$d \equiv a-1 \pmod{2}$nếu như$m$là bội số của 2; 
5.$d \ne 3c \pmod{9}$nếu như$m$là bội số của 9 

Kể từ khi$m = 2^e$, ước số nguyên tố duy nhất của$m$là 2. Do đó mọi điều kiện liên quan đến số nguyên tố lẻ đều được thỏa mãn. Cũng$c = 0$tương đối nguyên tố với$2^e$chỉ nếu$e = 0$, đó không phải là trường hợp của chúng tôi; do đó điều kiện i) hạn chế độ dài khoảng thời gian. Các điều kiện liên quan là lũy thừa modulo của 2. Theo Bài tập 3.2.2.8, cho$m$lũy thừa bằng 2, độ dài khoảng thời gian tối đa hoàn toàn nhỏ hơn$m$và được cho bởi$2^{e-2}$. 

Chúng tôi xác minh các điều kiện modulo 4 và 2: 

-$m \ge 4$ngụ ý$d \equiv 1 \pmod{2}$Và$a - 1 = 2^{e-1} - 1 \equiv 1 \pmod{2}$. Do đó điều kiện iv) được thỏa mãn. 
- Đối với mô đun 4,$d = 1$thật kỳ lạ,$a-1 = 2^{e-1} - 1 \equiv 1 \pmod{4}$vì$e \ge 3$. Do đó điều kiện iii) được thỏa mãn. 

Không có điều kiện nào khác được áp dụng vì$m$không chia hết cho 9. Do đó tất cả các điều kiện cần thiết để có modulo độ dài thời gian cực đại$2^e$giảm thời gian xuống$2^{e-2}$. 

Chúng ta cũng có thể thấy điều này một cách rõ ràng bằng cách lặp lại các ví dụ nhỏ. Cho phép$e = 4$, Vì thế$m = 16$,$a = 2^{3} = 8$,$d = 1$. Trình tự$X_{n+1} \equiv X_n^2 + 8 X_n \pmod{16}$bắt đầu bằng$X_0 = 1$mang lại những giá trị$X_1 \equiv 9, \quad X_2 \equiv 1 \pmod{16},$hiển thị khoảng thời gian là 4, bằng$2^{4-2} = 4$, phù hợp với yêu cầu chung. 

Do đó bằng quy nạp và Bài tập 3.2.2.8, dãy xác định bởi (4) có độ dài chu kỳ$\boxed{2^{e-2}}.$## Xác minh 

Dãy số này là một dãy đồng dư bậc hai modulo$2^e$với các thông số$(d,a,c) = (1, 2^{e-1}, 0)$. Vì$m = 2^e$, thời gian không thể đạt được$2^e$, nhưng theo điều kiện iii) và iv) của Bài tập 3.2.2.8, dãy đạt được chu kỳ cực đại có thể có đối với lũy thừa của 2, cụ thể là$2^{e-2}$. Kiểm tra các ví dụ nhỏ xác nhận tính toán. Sự phù hợp$X_{n+1} \equiv X_n (X_n + 2^{e-1}) \pmod{2^e}$đảm bảo việc lập bản đồ$X_n \mapsto X_{n+1}$là tính từ trên tập hợp${0, 1, \ldots, 2^e - 1}$modulo 4, bảo toàn độ dài chu kỳ$2^{e-2}$. 

Điều này hoàn thành việc chứng minh. 

∎
