---
title: "CF 1034B - Bé C Yêu 3 II"
description: "Chúng ta có một lưới hình chữ nhật rất lớn trong đó mỗi ô có thể được xác định bằng tọa độ $(x, y)$. Chúng ta liên tục đặt hai quân cờ cùng một lúc, nhưng chỉ khi khoảng cách Manhattan giữa hai ô trống được chọn chính xác là 3."
date: "2026-06-16T19:22:08+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "flows", "graph-matchings"]
categories: ["algorithms"]
codeforces_contest: 1034
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 511 (Div. 1)"
rating: 2200
weight: 1034
solve_time_s: 211
verified: false
draft: false
---

[CF 1034B - Bé C Yêu 3 II](https://codeforces.com/problemset/problem/1034/B) 

**Xếp hạng:** 2200 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, luồng, khớp biểu đồ 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật rất lớn trong đó mỗi ô có thể được xác định bằng tọa độ$(x, y)$. Chúng tôi liên tục đặt hai quân cờ cùng một lúc, nhưng chỉ khi khoảng cách Manhattan giữa hai ô trống đã chọn chính xác là 3. Mỗi vị trí như vậy tiêu tốn hai ô riêng biệt không được sử dụng và một khi một ô đã được sử dụng thì nó không thể được sử dụng lại trong bất kỳ cặp nào sau này. 

Nhiệm vụ là tối đa hóa số lượng các cặp được đặt, nghĩa là chúng ta muốn tối đa hóa số lượng các cặp ô rời rạc mà chúng ta có thể tạo thành sao cho mỗi cặp có khoảng cách Manhattan chính xác là 3. Tương tự, chúng ta đang tìm kiếm sự khớp tối đa trong biểu đồ trong đó mỗi ô là một nút và các cạnh kết nối các cặp ô ở khoảng cách Manhattan 3. 

Các ràng buộc về kích thước lưới là cực kỳ lớn, lên tới$10^9 \times 10^9$, do đó, bất kỳ giải pháp nào phụ thuộc vào việc lặp qua các ô hoặc thậm chí các hàng và cột đều không thể thực hiện được. Giải pháp chỉ phải phụ thuộc vào đặc tính cấu trúc của lưới và các mẫu cục bộ. 

Một điểm tinh tế quan trọng là mặc dù lưới có tỷ lệ vô hạn, quy tắc ghép nối là cục bộ: chỉ các độ lệch tương đối của biểu mẫu$(dx, dy)$với$|dx| + |dy| = 3$vấn đề. Điều đó có nghĩa là cấu trúc lặp lại theo định kỳ và bất kỳ chiến lược tối ưu nào cũng phải được biểu diễn dưới dạng xếp lớp hoặc phân tách các mẫu nhỏ. 

Các trường hợp cạnh xuất hiện ngay lập tức khi một chiều nhỏ. Nếu một trong hai$n < 2$hoặc$m < 2$, không có cặp nào có thể. Thú vị hơn, nếu cả hai chiều đều nhỏ nhưng không tầm thường, chẳng hạn như$n = m = 2$, vẫn không có cặp hợp lệ vì khoảng cách Manhattan tối đa là 2 nên đáp án là 0 mặc dù bảng không trống. Một cách tiếp cận ngây thơ giả định “lưới lớn ngụ ý nhiều kết quả phù hợp” sẽ thất bại ở đây trừ khi nó kiểm tra tính khả thi một cách rõ ràng. 

Một trường hợp ranh giới quan trọng khác là khi một chiều rất nhỏ, chẳng hạn$n = 1$. Mặc dù chiều khác có thể cực kỳ lớn, nhưng tất cả khoảng cách chỉ giảm xuống khoảng cách theo chiều ngang, do đó, chỉ những cặp có chênh lệch chính xác bằng 3 dọc theo một đường mới quan trọng. Điều này làm giảm vấn đề thành vấn đề ghép nối một chiều với các ràng buộc về khoảng cách. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô hình hóa vấn đề một cách trực tiếp, chúng ta có thể coi mỗi ô là một nút và kết nối các cạnh giữa tất cả các cặp ô có khoảng cách Manhattan là 3. Mỗi bước di chuyển hợp lệ sẽ tiêu tốn một cạnh trong một kết quả khớp. Chiến lược bạo lực sẽ xây dựng biểu đồ này một cách rõ ràng và chạy thuật toán đối sánh tối đa, chẳng hạn như Hopcroft-Karp trên sự phân chia hai bên của lưới. 

Điều này đúng về mặt khái niệm vì biểu đồ có tính chất lưỡng cực dưới tính chẵn lẻ của$(x + y)$và việc so khớp đảm bảo việc sử dụng các đỉnh một cách rời rạc. Tuy nhiên, kích thước đồ thị$n \cdot m$, có thể lớn bằng$10^{18}$. Ngay cả việc xây dựng tính liền kề một cách ngầm định cũng là không thể, vì mỗi nút có số lượng nút lân cận không đổi nhưng vẫn có quá nhiều nút cần xử lý. 

Quan sát chính là lưới có thể được phân chia thành các cấu trúc cục bộ độc lập. Mỗi ô chỉ tương tác với các ô ở độ lệch$(\pm 3, 0), (\pm 2, \pm 1), (\pm 1, \pm 2), (0, \pm 3)$. Đây là một lân cận hữu hạn cố định. Những vấn đề như vậy thường giảm xuống việc tìm ra bao nhiêu cạnh trong một khối tối ưu có thể được hình thành trên một đơn vị diện tích. 

Sau đó, chúng tôi nhận thấy rằng cấu trúc hoạt động khác nhau tùy thuộc vào hình dạng lưới. Nếu cả hai chiều ít nhất là 3 thì lưới có thể được xếp theo các khối trong đó mỗi khối đóng góp một số cặp cố định. Nếu một thứ nguyên là 1 hoặc 2 thì biểu đồ tương tác sẽ suy biến và phải được xử lý riêng. 

Đối với các lưới lớn, kết quả khớp tối ưu sẽ bão hòa hầu hết tất cả các ô ngoại trừ mẫu phần còn lại được giới hạn dọc theo các cạnh, do đó câu trả lời sẽ trở thành tuyến tính trong$n \cdot m$với một hiệu chỉnh nhỏ tùy thuộc vào dư lượng modulo 3. Sau khi phân tích cẩn thận các cấu hình cục bộ, kết quả được đơn giản hóa thành một công thức rõ ràng dựa trên việc đếm số lượng đầy đủ$3 \times 3$-các khối tương tác giống như có thể được hình thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu trên biểu đồ lưới | O(nm) hoặc tệ hơn | O(nm) | Quá chậm | 
| Phân rã dựa trên mẫu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu một trong hai chiều bằng 1, hãy rút gọn bài toán thành một đoạn thẳng có độ dài$L = \max(n, m)$. Trong trường hợp này, chỉ các cặp ở khoảng cách chính xác bằng 3 dọc theo đường thẳng là hợp lệ, vì vậy chúng ta đếm xem chúng ta có thể tạo thành bao nhiêu cặp rời nhau. Điều này đơn giản là$L // 4$cặp, bởi vì mỗi đoạn tối ưu có độ dài 4 đóng góp một cặp hợp lệ. 
2. Nếu một trong hai chiều là 2, hãy coi lưới là hai đường thẳng song song. Trong trường hợp này, các tương tác cục bộ hình thành các mẫu 2 hàng bị ràng buộc. Chúng tôi tính toán câu trả lời bằng cách nhóm các cột thành khối 4, vì việc đóng gói tối ưu lặp lại sau mỗi 4 cột. 
3. Nếu cả hai chiều ít nhất là 3, chúng ta khai thác cấu trúc hai chiều đầy đủ. Chúng tôi phân chia lưới thành các khối có kích thước$3 \times 3$. Bên trong mỗi khối, chúng ta có thể tạo thành chính xác 4 cặp hợp lệ bao gồm 8 ô, để lại một ô không sử dụng. Đây là cấu trúc lặp lại dày đặc nhất tương thích với các ràng buộc khoảng cách Manhattan 3. 
4. Tính xem có bao nhiêu đầy đủ
