---
title: "CF 104757D - Cornhusker"
description: "Chúng tôi đang mô phỏng một quy trình ước tính nông nghiệp đơn giản hóa về năng suất ngô. Dữ liệu đầu vào mô tả năm bắp ngô được lấy mẫu, trong đó mỗi bắp được đặc trưng bởi hai phép đo: có bao nhiêu hạt quấn quanh bắp và bao nhiêu hạt chạy dọc theo chiều dài của bắp."
date: "2026-06-28T22:47:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104757
codeforces_index: "D"
codeforces_contest_name: "2023-2024 ICPC East North America Regional Contest (ECNA 2023)"
rating: 0
weight: 104757
solve_time_s: 23
verified: false
draft: false
---

[CF 104757D - Cornhusker](https://codeforces.com/problemset/problem/104757/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quy trình ước tính nông nghiệp đơn giản hóa về năng suất ngô. Dữ liệu đầu vào mô tả năm bắp ngô được lấy mẫu, trong đó mỗi bắp được đặc trưng bởi hai phép đo: có bao nhiêu hạt quấn quanh bắp và bao nhiêu hạt chạy dọc theo chiều dài của bắp. Từ năm mẫu này, chúng tôi tính toán số lượng hạt nhân trung bình trên mỗi bắp. 

Sau đó, chúng tôi chia tỷ lệ trung bình này theo tổng số tai trong một đoạn hàng cố định. Điều này đưa ra ước tính về tổng số hạt nhân trong phân khúc đó. Cuối cùng, chúng tôi chuyển đổi hạt nhân thành giạ bằng cách sử dụng ước số được gọi là Hệ số trọng lượng hạt nhân (KWF) và chúng tôi được hướng dẫn sử dụng số học số nguyên xuyên suốt, nghĩa là phép chia sẽ rút ngắn về 0. 

Điểm mấu chốt là tất cả các cấu trúc đều tuyến tính. Chúng tôi giảm số đo thô thành một giá trị trung bình duy nhất, nhân với số lượng và chia cho một hằng số. Không có sự phức tạp về tổ hợp hoặc thuật toán nào ngoài số học và thứ tự các phép tính số nguyên cẩn thận. 

Các ràng buộc cực kỳ nhỏ: chỉ có năm tai, đầu vào có kích thước cố định và các giá trị được giới hạn ở mức hàng chục. Điều này đảm bảo rằng mọi lời giải đúng về cơ bản đều là thời gian không đổi. Chế độ lỗi duy nhất xuất phát từ việc xử lý sai thứ tự chia số nguyên. Nếu chúng ta chia quá sớm, chúng ta sẽ vĩnh viễn mất đi độ chính xác và kết quả cuối cùng sẽ không chính xác. 

Một cạm bẫy tinh vi sẽ xuất hiện nếu người giải tính trung bình sớm bằng cách sử dụng phép chia số nguyên. Ví dụ: nếu tổng số hạt nhân của năm tai có tổng bằng như vậy
