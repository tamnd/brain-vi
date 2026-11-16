---
title: "CF 1916B - Hai ước số"
description: "Chúng ta có hai số nguyên, $a$ và $b$, là hai ước thực sự lớn nhất của một số nguyên $x$ chưa biết. Ở đây, $1 le a < b < x$, và nhiệm vụ của chúng ta là tìm bất kỳ số nguyên $x$ nào phù hợp với mô tả này."
date: "2026-06-08T19:49:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1916
codeforces_index: "B"
codeforces_contest_name: "Good Bye 2023"
rating: 1000
weight: 1916
solve_time_s: 38
verified: false
draft: false
---

[CF 1916B - Hai ước số](https://codeforces.com/problemset/problem/1916/B) 

**Đánh giá:** 1000 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 38s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên$a$Và$b$, là hai ước thực sự lớn nhất của một số nguyên chưa biết$x$. Đây,$1 \le a < b < x$, và nhiệm vụ của chúng ta là tìm số nguyên bất kỳ$x$phù hợp với mô tả này. Về mặt khái niệm,$x$là số mà$b$là ước số lớn nhất dưới đây$x$, Và$a$là ước số lớn thứ hai. Chúng tôi phải xử lý nhiều trường hợp thử nghiệm độc lập. 

Giới hạn$1 \le a < b \le 10^9$Và$1 \le t \le 10^4$cho chúng tôi biết rằng việc lặp lại mạnh mẽ trên tất cả các số lên đến$10^9$sẽ là quá chậm. Bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các ước của mọi số ứng cử viên có thể sẽ dẫn đến ít nhất$O(\sqrt{10^9})$hoạt động cho mỗi trường hợp thử nghiệm, đưa ra khoảng$3 \cdot 10^7$hoạt động trong trường hợp xấu nhất cho một truy vấn duy nhất và hơn thế nữa$3 \cdot 10^{11}$vì$10^4$trường hợp thử nghiệm. Điều này rõ ràng là không thể thực hiện được. 

Các trường hợp cạnh không rõ ràng phát sinh khi$a = 1$, từ$1$chia mọi số nguyên, hoặc khi$b$là bội số của$a$. Ví dụ, nếu$a = 1$Và$b = 2$, nhỏ nhất$x$cái đó có$2$là ước số thực sự lớn nhất là$4$. Nếu như$a = 3$Và$b = 11$, sản phẩm$3 \cdot 11 = 33$sản xuất$11$là ước số thực sự lớn nhất và$3$là lớn thứ hai. Một cách tiếp cận ngây thơ chỉ đơn giản là trả về$a \cdot b$sẽ có tác dụng trong hầu hết các trường hợp, nhưng chúng ta phải hiểu tại sao điều này đúng trong mọi trường hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các số$x$lớn hơn$b$và kiểm tra tất cả các ước của chúng để xem liệu$a$Và$b$thực sự là hai lớn nhất. Điều này đúng về mặt định nghĩa nhưng không thực tế do giới hạn trên cao của$10^9$và số lượng ca kiểm thử. Kiểm tra tất cả các ước số cho đến$\sqrt{x}$cho mỗi ứng viên$x$và mỗi truy vấn dẫn đến độ phức tạp về thời gian không thể chấp nhận được. 

Cách tiếp cận tối ưu sử dụng quan sát mang tính xây dựng. Với mọi số nguyên$x$, nếu như$b$là ước số thực sự lớn nhất,$x$phải là bội số của$b$và bất kỳ ước số thích hợp nào khác nhỏ hơn$b$phải chia $
