---
title: "CF 1500F - Tủ Nhảy"
description: "Chúng ta được yêu cầu xây dựng lại chuỗi chiều cao của tủ dựa trên một phần thông tin. Cụ thể, Krosh nhớ lại sự khác biệt giữa chiếc tủ cao nhất và tủ ngắn nhất trong mỗi bộ ba liên tiếp."
date: "2026-06-10T21:06:31+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1500
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 707 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 3500
weight: 1500
solve_time_s: 78
verified: false
draft: false
---

[CF 1500F - Nhảy tủ](https://codeforces.com/problemset/problem/1500/F) 

**Đánh giá:** 3500 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng lại chuỗi chiều cao của tủ dựa trên một phần thông tin. Cụ thể, Krosh nhớ lại sự khác biệt giữa chiếc tủ cao nhất và tủ ngắn nhất trong mỗi bộ ba liên tiếp. Về mặt hình thức, đối với một dãy độ cao$h_1, h_2, \dots, h_n$, anh nhớ lại$w_i = \max(h_i, h_{i+1}, h_{i+2}) - \min(h_i, h_{i+1}, h_{i+2})$cho tất cả$i$từ$1$ĐẾN$n-2$. Nhiệm vụ của chúng ta là tìm dãy bất kỳ$h_1', h_2', \dots, h_n'$phù hợp với những khác biệt này hoặc báo cáo là không thể thực hiện được. 

Ràng buộc đầu vào$n \le 10^6$có nghĩa là bất kỳ giải pháp bậc hai nào trong$n$là không thể thực hiện được. Phương pháp tiếp cận thời gian tuyến tính hoặc gần thời gian tuyến tính đều được chấp nhận. Mỗi$w_i$được giới hạn bởi$C \le 10^{12}$và độ cao được xây dựng lại có thể lớn bằng$10^{18}$. Điều này gợi ý rằng số học với số nguyên phải tránh tràn nhưng không yêu cầu tính toán dấu phẩy động. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi trong đó tất cả$w_i = 0$, ngụ ý tất cả các tủ trong bộ ba tương ứng phải bằng nhau. Ví dụ: nếu những khác biệt được ghi nhớ không nhất quán$w_1 = 5$Và$w_2 = 0$, có thể không có chiều cao hợp lệ. Ngoài ra, sự khác biệt tối đa$w_i = C$nên được xử lý cẩn thận mà không vượt quá giới hạn chiều cao. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng liệt kê tất cả các chuỗi$h_1, \dots, h_n$và kiểm tra xem mỗi bộ ba liên tiếp có khớp với các hiệu đã cho hay không. Mỗi bộ ba có thể tạo ra nhiều cách sắp xếp giá trị giữa mức tối thiểu và tối đa của nó. Số lượng các trình tự có thể tăng theo cấp số nhân, khiến phương pháp này trở nên vô vọng đối với$n \approx 10^6$. Số lượng hoạt động sẽ theo thứ tự$O(2^n)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng tạo nên một giải pháp hiệu quả là mỗi sự khác biệt chỉ hạn chế thứ tự tương đối của ba phần tử liên tiếp. Chúng ta có thể mã hóa những ràng buộc này thành sự bất bình đẳng giữa các phần tử và truyền chúng về phía trước. Cụ thể, đối với ba$(h_i, h_{i+1}, h_{i+2})$, sự khác biệt$w_i$xác định một phạm vi giữa các giá trị tối thiểu và tối đa. Trình tự của$w_i$về cơ bản hạn chế mức tối thiểu và tối đa của mỗi bộ ba trượt. Bằng cách duy trì một khoảng giá trị khả thi cho hai tủ liên tiếp, chúng ta có thể xây dựng tủ thứ ba theo cách thỏa mãn yêu cầu tiếp theo.$w_i$hạn chế. Điều này làm giảm vấn đề thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tuyên truyền ràng buộc tuyến tính | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo chuỗi$h_1$Và$h_2$đến 0. Sự lựa chọn này là tùy ý; mọi giá trị ban đầu đều có tác dụng vì các giải pháp không phải là duy nhất. 
2. Lặp lại từ$i = 1$ĐẾN$n-2$. Ở mỗi bước, chúng tôi có tủ hiện tại$h_i$Và$h_{i+1}$cố định và chúng ta phải xác định$h_{i+2}$như vậy$\max(h_i, h_{i+1}, h_{i+2}) - \min(h_i, h_{i+1}, h_{i+2}) = w_i$. 
3. Tính các giá trị ứng viên cho$h_{i+2}$. Cho phép$a = h_i$,$b = h_{i+1}$. Có ba khả năng: 

-$h_{i+2} = \min(a, b) + w_i$-$h_{i+2} = \max(a, b) - w_i$-$h_{i+2} = \min(a, b)$hoặc$h_{i+2} = \max(a, b)$nếu như$w_i = |a - b|$Chọn giá trị giữ cho tất cả độ cao không âm. 
4. Nối thêm$h_{i+2}$đến trình tự. 
5. Sau khi xây dựng trình tự, hãy xác minh rằng tất cả$w_i$các ràng buộc được thỏa mãn. Nếu không thành công, xuất ra "NO". Ngược lại, xuất "CÓ" và chuỗi_
