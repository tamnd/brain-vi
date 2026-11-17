---
title: "CF 198A - Về vi khuẩn"
description: "Vấn đề liên quan đến việc mô hình hóa sự phát triển của vi khuẩn trong ống nghiệm theo sự tái phát theo thời gian riêng biệt. Mỗi vi khuẩn phân chia thành k vi khuẩn mỗi giây và thêm một vi khuẩn b được thêm vào do tác động bất thường."
date: "2026-06-03T16:22:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 198
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 125 (Div. 1)"
rating: 1700
weight: 198
solve_time_s: 35
verified: false
draft: false
---

[CF 198A - Giới thiệu về vi khuẩn](https://codeforces.com/problemset/problem/198/A) 

**Đánh giá:** 1700 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề liên quan đến việc mô hình hóa sự phát triển của vi khuẩn trong ống nghiệm theo sự tái phát theo thời gian riêng biệt. Mỗi vi khuẩn phân chia thành _k_ vi khuẩn mỗi giây và một vi khuẩn _b_ bổ sung được thêm vào do tác động bất thường. Sau _n_ giây bắt đầu từ một vi khuẩn, thí nghiệm đầu tiên tiếp cận chính xác _z_ vi khuẩn. Thí nghiệm thứ hai bắt đầu với _t_ vi khuẩn và chúng tôi muốn xác định số giây tối thiểu cần thiết để cùng một quá trình phát triển để đạt được ít nhất _z_ vi khuẩn. 

Các đầu vào _k_, _b_, _n_ và _t_ đều là số nguyên từ 1 đến 10^6. Điều này ngụ ý rằng bất kỳ tính toán lặp lại nào theo thời gian nên tránh quét tuyến tính _z_ hoặc các vòng lặp lớn lên đến _z_, vì số lượng vi khuẩn có thể phát triển cực kỳ nhanh - theo cấp số nhân trong _n_ đối với hệ số nhân _k_. Tràn không phải là một vấn đề trong Python, nhưng trong các ngôn ngữ khác, cần phải cẩn thận. Đầu ra là một số nguyên duy nhất, biểu thị số giây. 

Các trường hợp cạnh phát sinh khi số lượng vi khuẩn ban đầu _t_ đã bằng hoặc lớn hơn _z_ mục tiêu. Ví dụ: nếu _t_ = _z_ thì đáp án sh
