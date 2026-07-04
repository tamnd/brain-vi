---
title: "CF 103098F - Vòng tròn tình bạn"
description: "Đặt $X = a{n-1}a{n-2}cdots a0$ là biểu diễn nhị phân của một tổ hợp $(s,t)$, do đó $sum ai = t$. Viết dạng cột hàng rào liên quan của nó như trong phương trình (14), $$X = 0^{qt}1,0^{q{t-1}}1cdots 1,0^{q0},$$ trong đó mỗi $qi ge 0$ và $sum{i=0}^t qi = s$."
date: "2026-07-04T00:37:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103098
codeforces_index: "F"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, UPC contest"
rating: 0
weight: 103098
solve_time_s: 82
verified: false
draft: false
---

[CF 103098F - Vòng tròn tình bạn](https://codeforces.com/problemset/problem/103098/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$X = a_{n-1}a_{n-2}\cdots a_0$là biểu diễn nhị phân của một$(s,t)$-sự kết hợp, vậy$\sum a_i = t$. Viết dạng cột hàng rào liên quan của nó như trong phương trình (14),$$X = 0^{q_t}1\,0^{q_{t-1}}1\cdots 1\,0^{q_0},$$mỗi nơi$q_i \ge 0$Và$\sum_{i=0}^t q_i = s$. Sự lây lan của$X$, ký hiệu$X^{\sim}$, là thành phần$(q_t,\ldots,q_0)$được trích xuất từ ​​độ dài khối 0 giữa các giây 1 liên tiếp. 

Đồng thời, viết cùng một chuỗi nhị phân theo cấu trúc khối 0,$$X = 1^{p_t}0\,1^{p_{t-1}}0\cdots 0\,1^{p_0},$$với$p_i \ge 1$Và$\sum_{i=0}^t p_i = n+1$. Cốt lõi của$X$, ký hiệu$X^{\circ}$, là thành phần$(p_t,\ldots,p_0)$được xác định bằng chiều dài 1 khối và điều chỉnh biên như trong phương trình (10). 

Cho phép$X^{+}$biểu thị phần bù bit, thay thế từng phần$0$qua$1$và mỗi$1$qua$0$. Hoạt động này trao đổi khối 0 và khối một mà không thay đổi độ dài của chúng. 

Áp dụng$+$đến sự thể hiện của cột hàng rào$X$cho$$X^{+} = 1^{q_t}0\,1^{q_{t-1}}0\cdots 0\,1^{q_0}.$$Cấu trúc khối của$X^{+}$do đó bị chi phối bởi cùng một số nguyên$q_t,\ldots,q_0$, nhưng bây giờ được hiểu là độ dài của các khối 1 liên tiếp được phân tách bằng các số 0 đơn. Việc chuyển đổi biểu diễn này thành dữ liệu cốt lõi sử dụng chính xác quy tắc giống như trong phương trình (10), do đó cốt lõi của$X^{+}$là$$(X^{+})^{\circ} = (q_t,\ldots,q_0)^{\circ}.$$Mặt khác, việc áp dụng$\sim$ĐẾN$X^{\circ}$tương ứng với việc đảo ngược cách diễn giải từ cấu trúc 1 khối sang cấu trúc 0 khối, vì mã hóa kép hoán đổi vai trò của các phần tử được chọn và không được chọn trong phần cơ bản$(s,t)$-sự kết hợp. Hoạt động này chuyển đổi cùng một chuỗi độ dài khối$(q_t,\ldots,q_0)$vào biểu diễn trải rộng của cấu hình được chuyển đổi. 

Do đó, cả hai cấu trúc đều trích xuất cùng một bộ có thứ tự từ cùng một phân tách khối cơ bản của$X$, thu được sau khi hoán đổi vai trò của 0 và 1. Dữ liệu có độ dài khối không thay đổi khi bổ sung và chỉ được giải thích dưới dạng chênh lệch hoặc thay đổi cốt lõi. 

Do đó hai phép biến đổi trùng nhau$$X^{\sim +} = X^{\circ \sim}.$$Điều này hoàn thành việc chứng minh. ∎
