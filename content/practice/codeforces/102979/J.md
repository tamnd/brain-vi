---
title: "CF 102979J - Cuộc thi của Junkyeom"
description: "Đặt $U$ biểu thị tập hợp cơ bản của nhiều tổ hợp (92). Trong biểu diễn (6), mỗi đa tổ hợp là một chuỗi không tăng $$dt ge d{t-1} ge cdots ge d1,qquad s ge dt,$$ và phần bù của nó đối với $U$ được hình thành bằng cách lấy các phần tử của $U$ không…"
date: "2026-07-04T04:10:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102979
codeforces_index: "J"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Day 9 Contest (XXI Open Cup, Grand Prix of Suwon)"
rating: 0
weight: 102979
solve_time_s: 138
verified: false
draft: false
---

[CF 102979J - Cuộc thi của Junkyeom](https://codeforces.com/problemset/problem/102979/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$U$biểu thị tập hợp cơ bản của nhiều tổ hợp (92). Trong biểu diễn (6), mỗi đa tổ hợp là một chuỗi không tăng$$d_t \ge d_{t-1} \ge \cdots \ge d_1,\qquad s \ge d_t,$$và sự bổ sung của nó đối với$U$được hình thành bằng cách lấy các phần tử của$U$không được đại diện bởi lựa chọn đã cho, sau đó viết lại chúng dưới dạng một chuỗi không tăng cùng loại. Gợi ý liệt kê những phần bổ sung này một cách rõ ràng:$$3211,\;3210,\;3200,\;3110,\;3100,\;3000,\;2110,\;2100,\;2000,\;1100,\;1000.$$Trong cài đặt của (92), mọi đối tượng trong$U$được biểu diễn chính xác một lần bằng tư cách thành viên trong đa tổ hợp hoặc bằng tư cách thành viên trong phần bù của nó, do đó phần bù xác định ánh xạ$$\mathcal{C}: \mathcal{M}_{s,t} \to \mathcal{M}_{t,s},$$Ở đâu$\mathcal{M}_{s,t}$biểu thị tập hợp nhiều tổ hợp (92). Định nghĩa chỉ sử dụng phần bù được đặt bên trong$U$, do đó đối với bất kỳ sự kết hợp nào$A \subseteq U$,$$\mathcal{C}(A) = U \setminus A.$$Áp dụng phần bù hai lần sẽ khôi phục tập hợp ban đầu, vì$$U \setminus (U \setminus A) = A,$$Vì thế$\mathcal{C}$là một sự tiến hóa. Điều này ngụ ý rằng$\mathcal{C}$là sự song ánh giữa họ các đối tượng đang được xem xét và ảnh của nó. 

Cấu trúc của (92) đối xứng về các tham số$s$Và$t$bởi vì sự bổ sung của một sự lựa chọn của$t$các phần tử từ một$(s+t)$-yếu tố vũ trụ là sự lựa chọn của$s$các yếu tố từ cùng một vũ trụ. Trong mã hóa đa tổ hợp (6), tính đối xứng này tương ứng với việc thay thế chuỗi$(d_t,\dots,d_1)$bởi chuỗi bổ sung được liệt kê trong gợi ý, chuỗi này lại không tăng và thỏa mãn cùng giới hạn với$s$Và$t$hoán đổi cho nhau. 

Do đó phép toán bù biến đổi mọi cấu hình được tính trong$\partial$một nửa Hệ quả C thành một cấu hình duy nhất được tính theo chiều ngược lại$\partial$một nửa và ngược lại, vì$\mathcal{C}$là nghịch đảo của chính nó. Điều này thiết lập một mối quan hệ song phương giữa hai lớp. 

Do đó bất kỳ danh tính hoặc tuyên bố nào đã được chứng minh cho một$\partial$một nửa giữ cho nửa kia$\partial$một nửa bằng cách vận chuyển các vật thể thông qua song ánh bổ sung. 

Điều này hoàn thành việc chứng minh. ∎
