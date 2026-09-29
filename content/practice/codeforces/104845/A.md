---
title: "CF 104845A - \u041f\u043e\u0434\u0434\u0435\u0440\u0436\u0430\u043d\u0438\u0435 \u0431\u043e\u0434\u0440\u043e\u0441\u0442\u0438"
description: "Chúng ta có một số ngày cố định và mỗi ngày Igor phải chọn chính xác một trong hai hành động. Anh ta có thể học, điều này làm giảm “năng lượng” của anh ta một lượng cố định, hoặc đi ngủ sớm, điều này làm tăng thêm một lượng cố định khác."
date: "2026-06-28T11:29:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104845
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104845
solve_time_s: 26
verified: false
draft: false
---

[CF 104845A - \u041f\u043e\u0434\u0434\u0435\u0440\u0436\u0430\u043d\u0438\u0435 \u0431\u043e\u0434\u0440\u043e\u0441\u0442\u0438](https://codeforces.com/problemset/problem/104845/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số ngày cố định và mỗi ngày Igor phải chọn chính xác một trong hai hành động. Anh ta có thể học, điều này làm giảm “năng lượng” của anh ta một lượng cố định, hoặc đi ngủ sớm, điều này làm tăng thêm một lượng cố định khác. Sau khi tất cả các ngày được quyết định, tổng mức thay đổi năng lượng phải chính xác bằng 0, nghĩa là anh ta kết thúc ở nơi anh ta bắt đầu. 

Mục tiêu không chỉ là tìm ra bất kỳ lịch trình hợp lệ nào mà còn là tối đa hóa số ngày dành cho việc học trong điều kiện hạn chế này. 

Nếu chúng ta biểu thị số ngày học bằng$x$, thì phần còn lại$N - x$ngày là những ngày nghỉ ngơi. Mỗi ngày học đều góp phần$-B$, và mỗi ngày nghỉ đều góp phần$+A$. Điều kiện cuối cùng là tổng số thay đổi phải bằng 0. 

Vì vậy, vấn đề giảm xuống còn kiểm tra xem có tồn tại số nguyên hay không$x$TRONG$[0, N]$như vậy:$$x \cdot (-B) + (N - x)\cdot A = 0$$Đầu ra là mức tối đa như vậy$x$, hoặc$-1$nếu không có lịch trình hợp lệ tồn tại. 

Những hạn chế là rất lớn, lên tới$N, A, B \le 10^{18}$, do đó, bất kỳ cách tiếp cận nào lặp đi lặp lại nhiều ngày hoặc thử tất cả các giá trị của$x$ngay lập tức là không thể. Quét tuyến tính ngay cả$x$sẽ là không thể thực hiện được, vì$10^{18}$hoạt động vượt xa mọi giới hạn. 

Một điểm tinh tế là phương trình có thể không có nghiệm nguyên ngay cả khi tồn tại nghiệm phân số. Một vấn đề quan trọng khác là tính chia hết: ngay cả khi đại số gợi ý một nghiệm, nó có thể không tích phân. 

Một lỗi phổ biến là coi phương trình luôn có thể giải được bằng cách sắp xếp lại nó mà không kiểm tra xem giá trị kết quả là số nguyên hay nằm trong giới hạn. Một chế độ thất bại khác là bỏ qua ràng buộc$x \le N$, dẫn đến các lịch trình có giá trị về mặt toán học nhưng không thể thực hiện được về mặt vật lý. 

Ví dụ về lỗi cạnh: 

đầu vào:```
2
1
2
```Đang cố gắng$x = 2$: sự thay đổi năng lượng là$-4$, không thể cân bằng được. Đang thử (x = 1
