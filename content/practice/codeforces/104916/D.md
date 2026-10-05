---
title: "CF 104916D - \u041a\u0430\u043c\u044b\u0448\u043e\u0432\u044b\u0439 \u043a\u043e\u0442"
description: "Chúng ta được cung cấp một chuỗi các sự kiện được sắp xếp theo thời gian. Tại mỗi thời điểm, con mèo bắt được một số con chuột. Nhiều lần đánh bắt có thể xảy ra cùng một lúc, do đó, dữ liệu đầu vào thô có thể chứa các dấu thời gian lặp lại với số lượng liên quan."
date: "2026-06-28T08:11:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104916
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0421\u0430\u043c\u0430\u0440\u0435 2022-2023 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104916
solve_time_s: 26
verified: false
draft: false
---

[CF 104916D - \u041a\u0430\u043c\u044b\u0448\u043e\u0432\u044b\u0439 \u043a\u043e\u0442](https://codeforces.com/problemset/problem/104916/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các sự kiện được sắp xếp theo thời gian. Tại mỗi thời điểm, con mèo bắt được một số con chuột. Nhiều lần đánh bắt có thể xảy ra cùng một lúc, do đó, dữ liệu đầu vào thô có thể chứa các dấu thời gian lặp lại với số lượng liên quan. 

Nhiệm vụ là chọn một khoảng thời gian liên tục, được đo theo các sự kiện này, sao cho tổng số chuột bắt được trong khoảng thời gian đó ít nhất là một ngưỡng nhất định.$k$. Trong số tất cả các khoảng thời gian hợp lệ như vậy, chúng tôi muốn có sự khác biệt tối thiểu có thể có giữa dấu thời gian ngoài cùng bên phải và ngoài cùng bên trái trong khoảng thời gian đó. 

Trước khi suy luận về nhiệm vụ chính, cấu trúc đầu vào có một bước đơn giản hóa quan trọng. Nếu một số mục có cùng dấu thời gian, chúng có thể được hợp nhất thành một mục duy nhất có giá trị bằng tổng số chuột bị bắt tại thời điểm đó. Sau quá trình nén này, dấu thời gian sẽ tăng dần và mỗi vị trí biểu thị một thời điểm riêng biệt với trọng số liên quan. 

Các ràng buộc ngụ ý rằng độ dài chuỗi có thể lớn, do đó, bất kỳ phép quét bậc hai nào trên tất cả các cặp điểm cuối sẽ quá chậm. Một sự ngây thơ$O(n^2)$giải pháp sẽ thử mọi điểm cuối bên trái và quét sang phải cho đến khi điều kiện tổng được thỏa mãn, điều này không được chấp nhận đối với các điểm cuối lớn.$n$. 

Các trường hợp cạnh tinh tế chính đến từ hai hành vi. 

Đầu tiên là xử lý chính xác các dấu thời gian trùng lặp. Nếu chúng tôi không hợp nhất các dấu thời gian bằng nhau, một cửa sổ trượt có thể coi các thời điểm giống hệt nhau là các vị trí riêng biệt, điều này làm tăng độ dài khoảng thời gian một cách giả tạo hoặc phá vỡ tính chính xác khi tính toán chênh lệch thời gian. Ví dụ: nếu đầu vào chứa thời gian 5 được lặp lại hai lần, một với 3 con chuột và một với 4 con chuột, việc không hợp nhất sẽ cho phép các khoảng thời gian chỉ bao gồm một trong số chúng, nhưng bất kỳ cách giải thích chính xác nào về vấn đề đều coi cả hai xảy ra cùng một lúc. 

Trường hợp cạnh thứ hai là khi khoảng tối ưu không kết thúc ở điểm sớm nhất mà tổng đạt$k$. Cách tiếp cận tham lam “dừng ngay khi tổng ≥ k” có thể bỏ lỡ những câu trả lời tốt hơn. Hãy xem xét trường hợp việc thêm một phân đoạn bổ sung nhỏ hầu như không làm tăng khoảng thời gian nhưng cho phép thu hẹp ranh giới bên trái sau này để tạo ra phạm vi tổng thể nhỏ hơn. Giải pháp đúng phải duy trì một cửa sổ hợp lệ và tiếp tục điều chỉnh cả hai đầu thay vì cam kết sớm. 

## Phương pháp tiếp cận 

Phương pháp brute-force sửa chỉ mục bên trái và sau đó mở rộng chỉ mục bên phải cho đến khi tổng số chuột trong khoảng đó đạt ít nhất$k$. Đối với mỗi vị trí bên trái, chúng tôi tính lại tổng từ đầu hoặc quét dần sang phải. Trong trường hợp xấu nhất, mỗi lần quét con trỏ trái đều chạm vào gần như toàn bộ mảng, dẫn đến$O(n^2)$sự phức tạp về mặt thời gian. Điều này là quá chậm khi$n$là lớn. 

Cấu trúc của bài toán gợi ý một hành vi đơn điệu: khi chúng ta di chuyển điểm cuối bên phải sang bên phải, tổng số chuột không bao giờ giảm; khi chúng ta di chuyển điểm cuối bên trái sang bên phải, tổng không bao giờ tăng. Tính đơn điệu này cho phép sử dụng kỹ thuật hai con trỏ, trong đó cả hai con trỏ đều di chuyển về phía trước trên mảng nhiều nhất một lần. 

Thay vì bắt đầu lại tính toán cho mọi vị trí bên trái, chúng tôi duy trì tổng chạy cho một cửa sổ$[l, r]$. Chúng tôi mở rộng$r$cho đến khi tổng đủ lớn. Sau đó chúng tôi cố gắng thu nhỏ$l$trong khi vẫn duy trì tính hợp lệ, ghi lại khoảng thời gian tốt nhất mỗi khi có được cửa sổ hợp lệ. Nếu thu nhỏ phá vỡ tính hợp lệ, chúng tôi sẽ mở rộng$r$lại. Mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần, do đó tổng công việc trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Hai con trỏ |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xử lý trước trình tự 

Kết hợp
