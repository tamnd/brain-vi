---
title: "CF 1472E - Vị trí chính xác"
description: "Tôi không thể chẩn đoán điều này một cách đáng tin cậy chỉ từ mẫu vì cặp đầu vào/đầu ra mẫu không xác định được vấn đề."
date: "2026-06-11T00:34:24+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1472
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 693 (Div. 3)"
rating: 1700
weight: 1472
solve_time_s: 188
verified: false
draft: false
---

[CF 1472E - Vị trí chính xác](https://codeforces.com/problemset/problem/1472/E) 

**Đánh giá:** 1700 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, sắp xếp, hai con trỏ 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
Tôi không thể chẩn đoán điều này một cách đáng tin cậy chỉ từ mẫu vì cặp đầu vào/đầu ra mẫu không xác định được vấn đề. 

Hành vi này gợi ý rõ ràng rằng chương trình Python đang được thử nghiệm thực sự là một giải pháp cho một vấn đề hoàn toàn khác: 

- Đầu vào của bạn chứa 3 trường hợp thử nghiệm. 
- Kết quả mong đợi là ba số nguyên:`202`,`13`, Và`19`. 
- Sản lượng thực tế là:```
NO
NO
YES
1 4 2 3
```đây là định dạng của bài toán kiểu "tìm bốn chỉ số có tổng cặp bằng nhau", không phải là bài toán trả lời bằng số. 

Điều đó có nghĩa là mã đang chạy gần như chắc chắn không giải quyết được nhiệm vụ giống như mẫu bạn đã cung cấp. Do đó, việc truy tìm lỗi bên trong thuật toán là không thể nếu không biết báo cáo vấn đề thực tế. Có vô số vấn đề mà mẫu```
2
13 88
```có thể có câu trả lời hợp pháp`202`. 

Vui lòng cung cấp một trong những thông tin sau: 

1. Tuyên bố vấn đề đầy đủ, hoặc 
2. Tên vấn đề / Cuộc thi Codeforces và thư vấn đề, hoặc 
3. Giải pháp Python tạo ra kết quả đầu ra không chính xác. 

Khi tôi gặp vấn đề thực sự, tôi có thể: 

- theo dõi thuật toán trên mẫu bị lỗi, 
- xác định lỗi logic chính xác, 
- giải thích tại sao nó đưa ra câu trả lời sai, 
- và cung cấp giải pháp Python 3 hoàn chỉnh đã được sửa chữa.
