---
title: "CF 105544F - Thế tiến thoái lưỡng nan của Baker"
description: "Tôi có thể xác định vấn đề và khôi phục một phần tuyên bố từ tài liệu cuộc thi, nhưng tôi không có quyền truy cập vào tuyên bố chính thức đầy đủ, đầu vào/đầu ra mẫu hoặc hướng dẫn."
date: "2026-06-25T06:05:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105544
codeforces_index: "F"
codeforces_contest_name: "The 2023 ICPC Asia Taoyuan Regional Programming Contest"
rating: 0
weight: 105544
solve_time_s: 28
verified: false
draft: false
---

[CF 105544F - Thế lưỡng nan của thợ làm bánh](https://codeforces.com/problemset/problem/105544/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
Tôi có thể xác định vấn đề và khôi phục một phần tuyên bố từ tài liệu cuộc thi, nhưng tôi không có quyền truy cập vào tuyên bố chính thức đầy đủ, đầu vào/đầu ra mẫu hoặc hướng dẫn. Nguồn có sẵn cho thấy đây là vấn đề lập kế hoạch trên một máy cổ điển: 

Một công việc đòi hỏi$D_i$ngày và phải chịu một hình phạt$S_i$vì mỗi ngày nó đều đợi trước khi bắt đầu. Chúng ta phải xuất ra thứ tự công việc với tổng số tiền phạt tối thiểu, phá vỡ các ràng buộc theo chuỗi chỉ số công việc nhỏ nhất về mặt từ điển. 

Đây là quy tắc lập kế hoạch nổi tiếng thường được gọi là Quy tắc Smith. Công việc nên được sắp xếp theo tỷ lệ giảm dần$S_i / D_i$. Để tránh các vấn đề về dấu phẩy động, hãy so sánh hai công việc$a,b$bằng cách kiểm tra$$S_a \cdot D_b \;>\; S_b \cdot D_a.$$Nếu các tích bằng nhau, câu trả lời hợp lệ nhỏ nhất về mặt từ điển sẽ thu được bằng cách đặt chỉ mục gốc nhỏ hơn trước. 

Tôi có thể viết một bài xã luận và giải pháp hoàn chỉnh, nhưng để thực hiện nó một cách chính xác theo đúng định dạng mà bạn yêu cầu, tôi cần báo cáo vấn đề đầy đủ hoặc ít nhất là đầu vào/đầu ra mẫu. Tài liệu có thể truy cập công khai hiện tại chỉ hiển thị mô tả và các ràng buộc, không hiển thị mẫu hoặc chi tiết định dạng chính thức. 

Vui lòng dán tuyên bố đầy đủ (hoặc các mẫu) và tôi sẽ tạo ra bài xã luận hoàn chỉnh với tất cả các phần được yêu cầu: 

Hiểu vấn đề, Phương pháp tiếp cận, Hướng dẫn về thuật toán, Chứng minh, Giải pháp Python, Ví dụ đã thực hiện, Phân tích độ phức tạp, Trường hợp kiểm thử và Trường hợp biên.
