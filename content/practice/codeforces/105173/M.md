---
title: "CF 105173M - Nhà"
description: "Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng và chúng ta muốn đếm xem có bao nhiêu cách chọn năm điểm trong số đó để chúng tạo thành một cấu hình hình học cụ thể được gọi là “ngôi nhà”. Một ngôi nhà hợp lệ bao gồm năm điểm được dán nhãn $A, B, C, D, E$ với cấu trúc cứng nhắc."
date: "2026-06-27T08:21:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105173
codeforces_index: "M"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Northeast), The 18th Northeast Collegiate Programming Contest"
rating: 0
weight: 105173
solve_time_s: 29
verified: false
draft: false
---

[CF 105173M - Ngôi nhà](https://codeforces.com/problemset/problem/105173/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng và chúng ta muốn đếm xem có bao nhiêu cách chọn năm điểm trong số đó để chúng tạo thành một cấu hình hình học cụ thể được gọi là “ngôi nhà”. 

Một ngôi nhà hợp lệ bao gồm năm điểm được dán nhãn$A, B, C, D, E$với kết cấu cứng nhắc. Bốn điểm đầu tiên tạo thành một tứ giác vuông có các cạnh đối diện bằng nhau, buộc nó phải là một hình chữ nhật theo một trật tự tuần hoàn cố định. Điểm thứ năm$E$được gắn vào một cạnh của hình chữ nhật này và tạo thành một tam giác cân với hai đỉnh của cạnh đó, với một hạn chế bổ sung là góc ở một góc bị tù theo một hướng cụ thể. 

Đầu ra là số tập hợp con 5 điểm riêng biệt có thể được gán vai trò$A, B, C, D, E$thỏa mãn tất cả các ràng buộc hình học này. Hai ngôi nhà được coi là khác nhau nếu chúng sử dụng tọa độ khác nhau cho ít nhất một trong năm điểm, vì vậy chúng tôi đang đếm một cách hiệu quả các cấu trúc được gắn nhãn hợp lệ được tạo ra bởi tập hợp con các điểm. 

Những hạn chế là$n \le 300$, điều này ngay lập tức gợi ý rằng các giải pháp bậc ba hoặc kém hơn một chút so với bậc hai có thể vượt qua, nhưng bất kỳ giải pháp bậc bốn nào trên tất cả các kết hợp điểm sẽ không vượt qua. MỘT$O(n^3)$giải pháp nằm ở ranh giới nhưng có thể chấp nhận được với các hệ số không đổi cẩn thận. Điều này gợi ý rõ ràng rằng chúng ta nên liệt kê một cấu trúc lõi hình học trong$O(n^2)$, sau đó gắn điểm thứ năm một cách hiệu quả. 

Một vấn đề tế nhị là việc đếm quá mức do có nhiều nhãn hợp lệ có cùng cấu hình hình học. Vì vấn đề sử dụng vai trò cố định$A, B, C, D, E$, bất kỳ sự đối xứng nào trong hình chữ nhật hoặc hình tam giác đều có thể tạo ra nhiều cách hiểu nếu không được neo giữ cẩn thận. Một phương pháp đếm đơn giản chỉ tìm thấy hình chữ nhật hoặc chỉ tìm hình tam giác một cách độc lập sẽ bỏ lỡ các ràng buộc hoặc cấu hình đếm kép. 

Một cạm bẫy khác là xử lý tình trạng hình chữ nhật quá lỏng lẻo. Ví dụ: chọn bốn điểm tạo thành hình chữ nhật nhưng không đúng thứ tự có thể vi phạm các điều kiện vuông góc tùy theo phép gán. Tương tự, chọn điểm tam giác cân$E$không thực thi điều kiện góc tù có thể bao gồm không chính xác các cấu hình đối xứng trong đó$E$nằm ở phía sai của đế. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: chọn 5 điểm bất kỳ, thử tất cả các hoán vị gán chúng cho$A, B, C, D, E$và kiểm tra trực tiếp tất cả các điều kiện hình học. Điều này đúng vì nó trực tiếp xác minh định nghĩa. Tuy nhiên, số lượng lựa chọn$\binom{n}{5} \cdot 120$, trong trường hợp xấu nhất là vượt xa$300^5$, làm cho nó hoàn toàn không thể thực hiện được. 

Cấu trúc chính là bốn điểm tạo thành một hình dạng rất cứng: hình chữ nhật. Thay vì tìm kiếm các bộ tứ tùy ý, chúng ta có thể tạo ra tất cả các hình chữ nhật từ tập hợp điểm. Hình chữ nhật trong một tập hợp điểm có thể được tìm thấy trong$O(n^2)$bằng cách sử dụng thực tế là các đường chéo có cùng điểm giữa và độ dài. Khi một hình chữ nhật được cố định, vai trò o
