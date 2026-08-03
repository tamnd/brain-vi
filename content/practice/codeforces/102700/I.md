---
title: "CF 102700I - Khả năng chụp ảnh đáng kinh ngạc"
description: "Tôi không thể viết một bài xã luận chính xác cho vấn đề này vì chỉ tuyên bố thôi thì không đủ để tự tin xây dựng lại thuật toán dự định. Sự mơ hồ quan trọng là quy tắc chuyển động. Tuyên bố cho biết Paula liên tục chọn một tòa nhà: 1."
date: "2026-08-03T14:20:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "I"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 62
verified: false
draft: false
---

[CF 102700I - Khả năng chụp ảnh đáng kinh ngạc](https://codeforces.com/problemset/problem/102700/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

##Giải pháp 
Tôi không thể viết một bài xã luận chính xác cho vấn đề này vì chỉ tuyên bố thôi thì không đủ để tự tin xây dựng lại thuật toán dự định. 

Sự mơ hồ quan trọng là quy tắc chuyển động. Tuyên bố cho biết Paula liên tục chọn một tòa nhà: 

1. có thể nhìn thấy được từ tòa nhà hiện tại của cô ấy, trong đó tầm nhìn có nghĩa là không có tòa nhà nào cao hơn hoàn toàn giữa chúng và 
2. có chiều cao lớn hơn tòa nhà hiện tại của cô ấy. 

Tuy nhiên, nó không bao giờ chỉ rõ cách cô ấy lựa chọn trong số nhiều ứng cử viên có thể nhìn thấy được. Câu nói "cô ấy sẽ chỉ chọn trong số những tòa nhà mà cô ấy có thể nhìn thấy" hạn chế một loạt các động thái hợp pháp nhưng không xác định nên thực hiện cái nào. Vì đầu ra yêu cầu **khoảng cách tối đa cô ấy sẽ đi bộ** nên việc diễn giải có thể là: 

- cô ấy có thể chọn bất kỳ tòa nhà cao hơn nào có thể nhìn thấy được để tối đa hóa tổng khoảng cách cuối cùng, 
- cô ấy luôn chọn một số tòa nhà được xác định duy nhất (ví dụ: tòa nhà có thể nhìn thấy gần nhất), 
- hoặc bản thân biểu đồ hiển thị có một số thuộc tính ẩn làm cho đường dẫn trở nên duy nhất. 

Những cách giải thích đó tạo ra những câu trả lời khác nhau. 

Ví dụ, hãy xem xét:```
4
1 3 2 4
```Từ tòa nhà 1, cả tòa nhà 2 và 4 đều có thể nhìn thấy và cao hơn. Nếu được phép lựa chọn tùy ý thì cách đi bộ tốt nhất là```
1 -> 2 -> 4
```với khoảng cách```
1 + 2 = 3.
```Nếu cô ấy nhảy thẳng tới số 4 thì khoảng cách ở đây cũng là 3, nhưng trên các ví dụ tương tự thì chúng khác nhau. Các yếu tố đầu vào khác thậm chí còn tạo ra sự khác biệt lớn hơn. 

Định dạng mẫu trong lời nhắc cũng bị hỏng. Các mẫu xuất hiện dưới dạng:```
3 1 2 4
3 6 5 0

3 3 1 5 5
4 3 6 0 0
```đã mất dấu ngắt dòng ngăn cách`n`, độ cao và đầu ra dự kiến, do đó không thể xác minh đầu vào và đầu ra mẫu chính thức. 

Vì những sự mơ hồ này, bất kỳ giải pháp biên tập hoặc Python nào tôi viết nhất thiết phải dựa trên cách giải thích được phỏng đoán và có thể dễ dàng sai đối với giám khảo chính thức. 

Nếu bạn có thể cung cấp một trong hai: 

1. trang Codeforces gốc, 
2. một tệp PDF có định dạng câu lệnh được giữ nguyên hoặc 
3. định dạng đầu vào/đầu ra mẫu chính xác, 

Sau đó, tôi có thể viết bài xã luận hoàn chỉnh theo cấu trúc chính xác mà bạn yêu cầu, bao gồm bằng chứng về tính chính xác, thuật toán tối ưu, cách triển khai Python, các ví dụ đã hoạt động, phân tích độ phức tạp và các trường hợp thử nghiệm toàn diện.
