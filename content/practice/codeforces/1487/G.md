---
title: "CF 1487G - Đếm chuỗi"
description: "Tôi không thể chẩn đoán điều này một cách đáng tin cậy chỉ từ mẫu vì cặp đầu vào/đầu ra mẫu không xác định được vấn đề."
date: "2026-06-10T23:04:51+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 1487
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 104 (Rated for Div. 2)"
rating: 2700
weight: 1487
solve_time_s: 175
verified: false
draft: false
---

[CF 1487G - Đếm chuỗi](https://codeforces.com/problemset/problem/1487/G) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp, fft, toán 
**Thời gian giải:** 2m 55s 
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
