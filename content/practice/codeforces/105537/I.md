---
title: "CF 105537I - Nếu tôi có thể quay ngược thời gian"
description: "Chúng ta có hai dãy núi có độ cao bằng nhau. Một cái đại diện cho cảnh quan hiện tại, cái còn lại đại diện cho trạng thái trước đó. Cảnh quan phát triển qua nhiều năm riêng biệt."
date: "2026-06-27T00:59:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105537
codeforces_index: "I"
codeforces_contest_name: "2024-2025 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 105537
solve_time_s: 21
verified: false
draft: false
---

[CF 105537I - Nếu tôi có thể quay ngược thời gian](https://codeforces.com/problemset/problem/105537/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy núi có độ cao bằng nhau. Một cái đại diện cho cảnh quan hiện tại, cái còn lại đại diện cho trạng thái trước đó. Cảnh quan phát triển qua nhiều năm riêng biệt. Mỗi năm, một giá trị ngưỡng được chọn và mỗi ngọn núi có chiều cao hiện tại ít nhất bằng ngưỡng đó sẽ bị giảm đúng một đơn vị. Hoạt động này có thể ảnh hưởng đến nhiều ngọn núi cùng một lúc và các năm khác nhau có thể sử dụng các ngưỡng khác nhau. 

Nhiệm vụ là xác định liệu có thể chuyển đổi độ cao cũ thành độ cao hiện tại bằng cách sử dụng một số chuỗi hoạt động hàng năm như vậy hay không và nếu có, hãy tính số năm tối thiểu cần thiết. 

Một cách hữu ích để diễn giải quá trình này là hãy nghĩ đến việc mỗi ngọn núi độc lập nhận được một số mức giảm, nhưng có ràng buộc kết hợp: trong một năm, chỉ những ngọn núi nằm trên ngưỡng đã chọn mới bị ảnh hưởng cùng nhau. Điều này có nghĩa là chúng ta không thể tự do giảm các tập con tùy ý một cách độc lập; cấu trúc của những ngọn núi nào bị suy giảm cùng nhau là vấn đề quan trọng. 

Các ràng buộc ngụ ý tổng số lên tới 10^5 ngọn núi trong các trường hợp thử nghiệm. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Việc so sánh bậc hai giữa tất cả các cặp núi là không thể, và ngay cả các phương pháp tiếp cận O(n log n) cũng cần phải được chứng minh cẩn thận khi chỉ phân loại toàn cầu cộng với quét tuyến tính. 

Một trường hợp thất bại khó phát hiện khi việc chuyển đổi có vẻ hợp lý cục bộ trên mỗi ngọn núi nhưng không nhất quán trên toàn cầu do hạn chế về ngưỡng chung hàng năm. 

Ví dụ: nếu một ngọn núi phải giảm đi nhiều lần so với ngọn núi khác nhưng bắt đầu có chênh lệch độ cao thấp hơn, thì việc kiểm tra phép trừ trên mỗi chỉ số ngây thơ sẽ chấp nhận nó không chính xác. Hãy xem xét: 

đầu vào: 

n = 2 

hiện tại = [1, 10] 

quá khứ = [5, 12] 

Sự khác biệt là [4, 2], cho thấy mức giảm 4 và 2 trên mỗi ngọn núi. Một cách tiếp cận ngây thơ có thể cho rằng chúng ta có thể áp dụng 4 và 2 bước độc lập. Nhưng trên thực tế, một khi chúng ta chọn ngưỡng ảnh hưởng đến ngọn núi thứ hai, nó cũng có thể ảnh hưởng đến ngọn núi đầu tiên tùy thuộc vào độ cao tương đối và việc ghép đôi có thể khiến lịch trình không khả thi. 

Khó khăn cốt lõi là các hoạt động không phải trên từng phần tử mà
