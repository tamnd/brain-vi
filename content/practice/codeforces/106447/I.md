---
title: "CF 106447I - \u041f\u0440\u0430\u0432\u043e \u043d\u0430 \u043e\u0434\u0438\u043d\u043e\u0447\u0435\u0441\u0442\u0432\u043e"
description: "Tôi có thể viết bài xã luận, nhưng tôi không đủ tin tưởng vào giải pháp chính xác dự định cho vấn đề này chỉ từ tuyên bố. Sự mơ hồ chính là thao tác chỉnh sửa: nó nói rằng một cây con có thể được thay thế bằng một cây con khác “có thể có kích thước khác”."
date: "2026-06-25T09:24:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106447
codeforces_index: "I"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2025-2026. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 106447
solve_time_s: 53
verified: false
draft: false
---

[CF 106447I - \u041f\u0440\u0430\u0432\u043e \u043d\u0430 \u043e\u0434\u0438\u043d\u043e\u0447\u0435\u0441\u0442\u0432\u043e](https://codeforces.com/problemset/problem/106447/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** không 

##Giải pháp 
Tôi có thể viết bài xã luận, nhưng tôi không đủ tin tưởng vào giải pháp chính xác dự định cho vấn đề này chỉ từ tuyên bố. 

Sự mơ hồ chính là thao tác chỉnh sửa: nó nói rằng một cây con có thể được thay thế bằng một cây con khác “có thể có kích thước khác”. Điều đó làm thay đổi số lượng lá trong cây, khiến cho cách giải thích thông thường về việc “làm cho tất cả các giá trị lá khác biệt theo cặp” rất khác với bài toán cây nhị phân có kích thước cố định. Chiến lược chỉnh sửa tối thiểu phụ thuộc hoàn toàn vào việc: 

1. toàn bộ cây có thể được chọn làm cây con thay thế, 
2. Thay thế cây con được phép thay đổi số lá cuối cùng, 
3. cây con được sao chép phải đến từ trạng thái cây hiện tại sau các lần chỉnh sửa trước đó, 
4. Cây con được sao chép phải có cùng chiều cao với cây con bị loại bỏ. 

Những cách hiểu khác nhau dẫn đến những thuật toán khác nhau và thậm chí những câu trả lời khác nhau trong những trường hợp nhỏ. 

Nếu bạn cung cấp tuyên bố chính thức bằng tiếng Anh hoặc phần làm rõ hoạt động dự định, tôi có thể tạo ra bài xã luận đầy đủ theo phong cách Codeforces với bằng chứng, phân tích độ phức tạp, giải pháp Python, hướng dẫn và bài kiểm tra ở định dạng được yêu cầu.
