---
title: "CF 1534H - Nút bị mất"
description: "Xét một bình kín có thể tích cố định $V$ chứa một loại khí lý tưởng có nhiệt độ khối là $T1$. Thành thùng chứa được duy trì ở nhiệt độ $T$, và $T$ không nhất thiết phải bằng $T1$."
date: "2026-06-10T16:07:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "graphs", "interactive", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1534
codeforces_index: "H"
codeforces_contest_name: "Codeforces LATOKEN Round 1 (Div. 1 + Div. 2)"
rating: 3500
weight: 1534
solve_time_s: 92
verified: false
draft: false
---

[CF 1534H - Nút bị mất](https://codeforces.com/problemset/problem/1534/H) 

**Đánh giá:** 3500 
**Thẻ:** thuật toán xây dựng, dp, đồ thị, tương tác, sắp xếp, cây 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập và giả định 

Hãy xem xét một thùng chứa kín có thể tích cố định$V$chứa một lượng khí lý tưởng có nhiệt độ chung là$T_1$. Thành thùng chứa được duy trì ở nhiệt độ$T$, Và$T$không cần bằng$T_1$. Bài toán yêu cầu áp suất tức thời do chất khí tác dụng lên thành khi chất khí và thành không ở trạng thái cân bằng nhiệt. 

Điểm cốt yếu là các phân tử đi tới thành đến từ chất khí và có sự phân bố vận tốc tương ứng với$T_1$, trong khi các phân tử rời khỏi thành sau khi chịu nhiệt độ của thành có phân bố vận tốc tương ứng với$T$. Vì áp suất là tốc độ truyền động lượng vào tường nên cả hai nhiệt độ đều được đưa vào tính toán. 

Giả sử chỗ ở nhiệt hoàn toàn ở tường. Các phân tử đập vào tường được đặc trưng bởi nhiệt độ$T_1$và các phân tử phát xạ lại từ vách được đặc trưng bởi nhiệt độ$T$. Khí đủ loãng để các biểu thức lý thuyết động học cho dòng phân tử có thể được sử dụng cục bộ trên tường. 

Bởi vì bình đóng kín và trạng thái cân bằng cơ học được thiết lập nhanh hơn nhiều so với trạng thái cân bằng nhiệt nên áp suất$P$là một đại lượng duy nhất trong toàn bộ khí. Mật độ phân tử gần tường không phải là hằng số quy định bên ngoài; nó điều chỉnh sao cho áp suất như nhau tác động lên mọi nơi trong bình. Phép tính dưới đây xác định mối quan hệ giữa$P$,$T$, và mật độ tường$n$. 

## Nguyên lý vật lý 

Áp lực lên một bức tường bằng động lượng pháp tuyến truyền tới bức tường đó trên một đơn vị diện tích và trên một đơn vị thời gian. 

Cho phép$n$là mật độ số phân tử liền kề với bức tường. Đối với khí Maxwell có nhiệt độ$T_1$, dòng phân tử chạm vào một đơn vị diện tích tường trong một đơn vị thời gian là$$\Phi_{\rm in}=n\sqrt{\frac{k_B T_1}{2\pi m}},$$Ở đâu$m$là khối lượng phân tử. 

Động lượng pháp tuyến trung bình được mang bởi một phân tử sự cố là$$\langle p_x\rangle_{\rm in} = \frac{\displaystyle\int_0^\infty m v_x^2 e^{-m v_x^2/(2k_B T_1)}\,dv_x} {\displaystyle\int_0^\infty v_x e^{-m v_x^2/(2k_B T_1)}\,dv_x} = \sqrt{\frac{\pi m k_B T_1}{2}}.$$Nhân thông lượng sự cố với động lượng trung bình trên mỗi phân tử sự cố sẽ cho$$J_{\rm in} = n\sqrt{\frac{k_B T_1}{2\pi m}} \sqrt{\frac{\pi m k_B T_1}{2}} = \frac12 n k_B T_1.$$Các phân tử rời khỏi tường có nhiệt độ$T$. Đặt mật độ số lượng của chúng ngay lúc phát xạ là$n_w$. Lặp lại phép tính tương tự cho hàm Maxwell được phát ra sẽ cho$$J_{\rm out} = \frac12 n_w k_B T.$$Áp suất tác dụng lên tường là tổng độ lớn của các thông lượng động lượng tới và đi:$$P=J_{\rm in}+J_{\rm out}.$$Một bức tường cố định không thể tích lũy các phân tử. Số lượng phân tử đến tường trên một đơn vị diện tích và đơn vị thời gian phải bằng số lượng phân tử rời khỏi nó. Kể từ đây$$n\sqrt{\frac{k_B T_1}{2\pi m}} = n_w\sqrt{\frac{k_B T}{2\pi m}},$$mang lại lợi nhuận$$n_w=n\sqrt{\frac{T_1}{T}}.$$## Đạo hàm 

Thay thế$$n_w=n\sqrt{\frac{T_1}{T}}$$vào biểu thức của thông lượng động lượng đi ra cho$$J_{\rm out} = \frac12 n\sqrt{\frac{T_1}{T}}\,k_B T = \frac12 n k_B\sqrt{T_1T}.$$Thông lượng đến vẫn còn$$J_{\rm in} = \frac12 n k_B T_1.$$Kể từ đây$$P = \frac12 n k_B T_1 + \frac12 n k_B\sqrt{T_1T} = \frac12 n k_B\left(T_1+\sqrt{T_1T}\right).$$Ở giai đoạn này$n$là mật độ tiếp giáp với tường. Vì áp suất trong bình kín là áp suất cơ học duy nhất nên hệ thức này có thể được giải đối với mật độ thành:$$n = \frac{2P} {k_B\left(T_1+\sqrt{T_1T}\right)}.$$Đối với áp suất cố định$P$và nhiệt độ khí cố định$T_1$, tăng nhiệt độ thành làm giảm mật độ thành theo công thức này. Mật độ tường điều chỉnh chính xác để duy trì cùng một áp suất. 

Biểu thức cho$P$do đó rút ra ở trên không phải là dự đoán rằng áp suất thay đổi khi$T$những thay đổi. Đó là mối quan hệ giữa$P$và mật độ địa phương$n$. Từ$n$không được cố định độc lập trong một thùng kín, người ta không thể so sánh áp suất ở các nhiệt độ thành khác nhau bằng cách xử lý$n$như hằng số. 

Áp suất được xác định bởi trạng thái của khí trong bình, trong khi nhiệt độ thành bình làm thay đổi mật độ của lớp Knudsen tiếp giáp với thành bình. 

## Kết quả 

Việc tính toán lý thuyết động học cho$$P = \frac12 n k_B\left(T_1+\sqrt{T_1T}\right),$$Ở đâu$n$là mật độ phân tử liền kề với bức tường. 

Trong một thùng kín,$n$không cố định khi$T$những thay đổi. Thay vì,$$n = \frac{2P} {k_B\left(T_1+\sqrt{T_1T}\right)}$$điều chỉnh sao cho áp suất cơ học như nhau tồn tại khắp bình. 

Do đó, tính toán không hỗ trợ cho khẳng định rằng áp suất lớn hơn đối với$T>T_1$hoặc nhỏ hơn cho$T<T_1$. Áp suất có thể giữ nguyên trong khi mật độ gần thành thay đổi. Từ thông tin cho trong bài toán, không có bất đẳng thức nào$$P(T>T_1)>P(T<T_1)$$cũng không$$P(T>T_1)<P(T<T_1)$$được thành lập. 

Kết luận chính xác từ phân tích lý thuyết động học là các bức tường nóng hơn tương ứng với mật độ tường liền kề thấp hơn và các bức tường lạnh hơn tương ứng với mật độ tường liền kề cao hơn, trong khi bản thân áp suất không được xác định chỉ bởi nhiệt độ tường. 

## Kiểm tra độ tỉnh táo 

Khi nào$T=T_1$,$$P = \frac12 n k_B(T_1+T_1) = n k_B T_1,$$đó là áp suất khí lý tưởng tiêu chuẩn. 

Giải quyết cho$n$cho$$n=\frac{P}{k_B T_1},$$một lần nữa tái tạo mối quan hệ khí lý tưởng cân bằng. 

Khi$T\to0$,$$P = \frac12 n k_B T_1,$$và do đó$$n=\frac{2P}{k_B T_1}.$$Mật độ thành gấp đôi giá trị cân bằng cần thiết để duy trì cùng một áp suất. 

Khi$T\gg T_1$,$$P \sim \frac12 n k_B\sqrt{T_1T},$$Vì thế$$n \sim \frac{2P}{k_B\sqrt{T_1T}}.$$Mật độ tường giảm khi$T^{-1/2}$trong khi áp suất vẫn hữu hạn. 

Các trường hợp giới hạn này phù hợp với cách giải thích rằng nhiệt độ của thành làm thay đổi mật độ ở vùng lân cận của thành thay vì xác định trực tiếp áp suất của bình kín.
