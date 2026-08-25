---
title: "CF 102163F - Công trình nghiên cứu"
description: "Tổng cộng có (N) sinh viên và (K) trong số họ đã được giao cho các dự án nghiên cứu hiện có. Những học sinh (N-K) còn lại vẫn cần được đưa vào các dự án mới được tạo. Một dự án mới có thể chứa từ 1 đến 6 sinh viên."
date: "2026-08-24T02:58:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "F"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 1035
verified: false
draft: false
---

[CF 102163F - Dự án nghiên cứu](https://codeforces.com/problemset/problem/102163/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 15 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Tổng cộng có (N) sinh viên và (K) trong số họ đã được giao cho các dự án nghiên cứu hiện có. Những học sinh (N-K) còn lại vẫn cần được đưa vào các dự án mới được tạo. 

Một dự án mới có thể chứa từ 1 đến 6 sinh viên. Vì mỗi học sinh chỉ có thể thuộc về một dự án nên nhiệm vụ là chia tất cả (N-K) học sinh chưa được chỉ định thành các nhóm có quy mô tối đa là 6 trong khi sử dụng càng ít nhóm càng tốt. 

Quan sát quan trọng là một dự án có thể chứa tối đa 6 học sinh, vì vậy mỗi dự án mới có thể chứa tối đa 6 học sinh vẫn chưa được phân công. Do đó, câu trả lời là số nhóm có năng lực 6 nhỏ nhất có thể chứa (N-K) học sinh. 

Giá trị của (N) và (K) có thể lớn bằng (10^{18}). Một thuật toán xử lý từng học sinh có thể yêu cầu tối đa (10^{18}) lần lặp, vượt xa những gì có thể chạy trong giới hạn 1 giây. Chúng ta cần một giải pháp số học theo thời gian không đổi cho từng trường hợp thử nghiệm. Số nguyên Python cũng xử lý trực tiếp các giá trị có kích thước này, do đó không có vấn đề tràn
