---
title: "CF 967A - Tâm trí khoảng cách"
description: "Chúng tôi được cung cấp lịch trình hạ cánh theo lịch trình, mỗi lần hạ cánh đúng một phút, đã được sắp xếp theo thứ tự tăng dần. Chúng ta phải chèn thêm một sự kiện, một sự kiện diễn ra cũng kéo dài một phút, vào dòng thời gian này. Hạn chế chính là khoảng cách an toàn."
date: "2026-06-17T01:36:09+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 967
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 477 (rated, Div. 2, based on VK Cup 2018 Round 3)"
rating: 1100
weight: 967
solve_time_s: 27
verified: false
draft: false
---

[CF 967A - Lưu ý đến khoảng trống](https://codeforces.com/problemset/problem/967/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp lịch trình hạ cánh theo lịch trình, mỗi lần hạ cánh đúng một phút, đã được sắp xếp theo thứ tự tăng dần. Chúng ta phải chèn thêm một sự kiện, một sự kiện diễn ra cũng kéo dài một phút, vào dòng thời gian này. 

Hạn chế chính là khoảng cách an toàn. Nếu việc hạ cánh xảy ra tại thời điểm t thì việc cất cánh không được diễn ra trong bất kỳ phút nào từ t − s đến t + s. Vì cả hai sự kiện đều kéo dài một phút nên điều này tạo ra một khoảng thời gian bị chặn xung quanh mỗi lần hạ cánh mà không thể bắt đầu cất cánh. 

Nhiệm vụ là tìm ra thời điểm cất cánh sớm nhất có thể để không vi phạm các vùng cấm này. Thời gian được tính từ 00:00 vào ngày 0 và chúng tôi có thể cần phải vượt quá 24 giờ nếu cần thiết. 

Các hạn chế rất nhỏ: tối đa 100 lần hạ cánh và giới hạn an toàn lên tới 60 phút. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài việc quét hoặc mô phỏng tuyến tính đơn lẻ. Ngay cả việc kiểm tra trực tiếp thời gian ứng cử viên cũng có thể khả thi vì thời gian chỉ tiến triển qua một số lượng nhỏ các khoảng thời gian có liên quan. 

Một số trường hợp đặc biệt quan trọng. 

Một là khi có một khoảng cách lớn sớm, chẳng hạn như lần hạ cánh đầu tiên lúc 00:00 và s = ​​60. Một ý tưởng ngây thơ rằng chỉ kiểm tra giữa các lần hạ cánh có thể bỏ sót thực tế rằng thời gian trước lần hạ cánh đầu tiên cũng bị hạn chế bởi cửa sổ an toàn của nó. 

Một trường hợp khác là khi các cuộc đổ bộ quá gần đến nỗi vùng cấm của chúng chồng lên nhau. Ví dụ: nếu một lần hạ cánh lúc 1:00 và một lần hạ cánh khác lúc 1:30 với s = 60, các khoảng thời gian bị cấm của chúng sẽ hợp nhất thành một vùng bị chặn liên tục và giải pháp đúng phải coi đây là một hạn chế duy nhất thay vì hai khoảng trống độc lập. 

Cuối cùng, câu trả lời có thể là sau lần hạ cánh cuối cùng, có thể vượt xa ranh giới ngày ban đầu nên việc giới hạn tìm kiếm trong 24 giờ sẽ là không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng phút có thể bắt đầu từ thời điểm 0 trở đi. Đối với mỗi ứng cử viên vào thời điểm t, chúng tôi kiểm tra xem việc đặt khoảng thời gian một phút bắt đầu từ t có xung đột với bất kỳ lần hạ cánh nào hay không. Xung đột xảy ra nếu |t − li| ≤ s đối với thời gian hạ cánh bất kỳ li. 

Điều này đúng vì nó trực tiếp thực thi điều kiện an toàn. Tuy nhiên, trong trường hợp xấu nhất, chúng ta có thể cần phải quét xa hơn trong tương lai cho đến khi tìm được một vị trí hợp lệ. Nếu các lần hạ cánh kéo dài tới 24 giờ và khoảng cách lớn, thì cuối cùng chúng ta có thể kiểm tra theo thứ tự hàng chục nghìn phút và mỗi lần kiểm tra sẽ quét tối đa n lần hạ cánh, cho ra khoảng O(T·n), điều này là không cần thiết. 

Điều quan trọng nhất là chúng ta không cần phải xem xét từng phút một. Những lần duy nhất quan trọng là những lần gần hạ cánh. Giữa hai lần hạ cánh liên tiếp, câu hỏi liên quan duy nhất là liệu khoảng cách giữa các khoảng cấm của chúng có đủ lớn để cất cánh hay không. Thay vì kiểm tra từng phút, chúng ta có thể “nhảy” từ vùng cấm này sang vùng cấm khác bằng cách theo dõi thời gian sớm nhất chúng ta được phép bắt đầu. 

Chúng tôi xử lý việc hạ cánh theo thứ tự, duy trì thời gian hợp lệ sớm nhất mà chúng tôi có thể đặt chuyến cất cánh cho đến nay. Đối với mỗi lần hạ cánh, chúng tôi đảm bảo rằng thời gian dự kiến ​​của chúng tôi không nằm trong khoảng thời gian bị cấm; nếu đúng như vậy, chúng tôi sẽ đẩy nó về phía cuối khoảng thời gian đó. Điều này biến vấn đề thành một lần quét tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T · n) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi tất cả thời gian thành phút ngay từ đầu. Điều này giúp đơn giản hóa việc so sánh và tránh phải xử lý nhiều lần các trường hợp ranh giới theo giờ-phút. 

1. Chuyển đổi mỗi lần hạ cánh thành tổng số phút tính từ thời điểm 0. Điều này cho phép so sánh số học trực tiếp với giới hạn an toàn. 
2. Khởi tạo một biến current_time = 0. Đây là biến ti_ sớm nhất
