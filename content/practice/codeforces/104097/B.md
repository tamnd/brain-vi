---
title: "CF 104097B - \u66f4\u52a0 Tầm thường \u7684\u984c\u76ee (Quadrivial)"
description: "Đặt các biến có chỉ mục lẻ xác định một phân số nhị phân $$A = (0.x1x3x5ldots)2,$$ và các biến có chỉ số chẵn xác định $$B = (0.x2x4x6ldots)2.$$ Hàm Boolean là $$F = [AB ge 1/2]."
date: "2026-07-02T02:14:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104097
codeforces_index: "B"
codeforces_contest_name: "2022 Taiwan NHSPC Mock Contest"
rating: 0
weight: 104097
solve_time_s: 126
verified: false
draft: false
---

[CF 104097B - \u66f4\u52a0 Tầm thường \u7684\u984c\u76ee (Quadrivial)](https://codeforces.com/problemset/problem/104097/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Hãy để các biến có chỉ số lẻ xác định một phân số nhị phân$$A = (0.x_1x_3x_5\ldots)_2,$$và các biến được lập chỉ mục chẵn xác định$$B = (0.x_2x_4x_6\ldots)_2.$$Hàm Boolean là$$F = [AB \ge 1/2].$$Việc đánh giá tiến hành bằng cách hiển thị các bit theo thứ tự xen kẽ cố định$x_1, x_2, x_3, x_4, \ldots$. Sau khi xử lý lần đầu$k$các biến, nút BDD biểu thị tất cả các khả năng hoàn thành có thể có của hai phân số nhị phân một phần. Mỗi ràng buộc chuyển nhượng một phần$A$Và$B$đến các khoảng đôi có điểm cuối là bội số của$2^{-t}$, Ở đâu$t = \lfloor k/2 \rfloor$cho mỗi luồng. 

Chính xác hơn, sau$k$các bước chúng tôi đã xây dựng khoảng thời gian$$A \in [a_k, a_k + 2^{-t}], \quad B \in [b_k, b_k + 2^{-t}],$$Ở đâu$a_k$Và$b_k$chỉ phụ thuộc vào các bit được tiết lộ. Do đó, sản phẩm được chứa trong một khoảng$$AB \in [L_k, U_k],$$trong đó cả hai điểm cuối đều là các số hữu tỉ có mẫu số nhiều nhất$2^k$. 

Nút BDD ở cấp độ$k$được xác định hoàn toàn bằng cách ngưỡng$1/2$nằm tương đối với khoảng này: hoặc toàn bộ khoảng nằm trên$1/2$, hoàn toàn bên dưới$1/2$, hoặc nằm trên nó. Chỉ có trường hợp thứ ba đòi hỏi phải phân biệt rõ hơn ở những cấp độ sâu hơn. 

Quan sát quan trọng là ở cấp độ$k$, bất biến duy nhất tồn tại trong quá trình giảm là vị trí tương đối của$1/2$trong số$k+1$"cấu hình giao nhau" có thể có của các điểm cuối khoảng. Mỗi khi một bit mới được tiết lộ, một trong các điểm cuối sẽ dịch chuyển chính xác$2^{-t}$trong hệ tọa độ riêng của nó và việc sàng lọc khoảng sản phẩm duy trì cấu trúc sắp xếp một chiều. Điều này buộc tập hợp các trạng thái có thể phân biệt được ở cấp độ$k$để phát triển bằng cách chia mỗi trạng thái hiện có thành nhiều nhất một vị trí mới chưa được giải quyết, tạo ra mô hình tăng trưởng tuyến tính. 

Cụ thể hơn, sau$k$biến, ranh giới quyết định được xác định bằng bao nhiêu so sánh hiệu quả giữa các tiền tố của$A$Và$B$đã đóng góp độ lệch dương hoặc âm so với ngưỡng. Độ lệch này có thể được mã hóa dưới dạng tham số cân bằng số nguyên thay đổi tối đa một biến cho mỗi biến, bắt đầu từ$0$và không bao giờ cần độ lớn lớn hơn$k$ở cấp độ$k$. Hai tiền tố mang lại cùng một tham số cân bằng tạo ra các BDD phụ đẳng cấu, vì tất cả các sàng lọc trong tương lai chỉ phụ thuộc vào số dư hiện tại chứ không phụ thuộc vào lịch sử bit cụ thể. 

Do đó số lượng nút giảm riêng biệt ở cấp độ$k$bằng số lượng giá trị số dư có thể đạt được, cụ thể là$$\{-k, -k+2, \ldots, k\}$$sau khi chuẩn hóa bằng cách rút gọn các trường hợp đối xứng, sẽ thu gọn thành một chuỗi các lớp tương đương có thể phân biệt được được lập chỉ mục bởi$0,1,\ldots,k$. 

Do đó số lượng nút ở mức$k$là$$b_k = k+1.$$Điều này hoàn thành việc chứng minh. ∎
