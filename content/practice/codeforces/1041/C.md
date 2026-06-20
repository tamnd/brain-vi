---
title: "CF 1041C - Giải lao cà phê"
description: "Chúng ta có một số khoảnh khắc trong ngày làm việc khi Monocarp sẵn sàng uống cà phê. Mỗi khoảnh khắc là một phút cụ thể trong ngày kéo dài từ phút 1 đến phút m."
date: "2026-06-16T18:00:26+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1041
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 509 (Div. 2)"
rating: 1600
weight: 1041
solve_time_s: 276
verified: false
draft: false
---

[CF 1041C - Giải lao](https://codeforces.com/problemset/problem/1041/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số khoảnh khắc trong ngày làm việc khi Monocarp sẵn sàng uống cà phê. Mỗi khoảnh khắc là một phút cụ thể trong ngày kéo dài từ phút 1 đến phút m. Mỗi giờ nghỉ giải lao phải được ấn định vào một ngày làm việc và nhiều giờ nghỉ giải lao có thể diễn ra trong cùng một ngày, nhưng chỉ khi chúng cách nhau đủ xa về mặt thời gian trong ngày đó. Hạn chế là nếu hai lần nghỉ diễn ra trong cùng một ngày thì chênh lệch giữa số phút của chúng ít nhất phải là d. 

Chúng tôi không chỉ định lịch trình tùy ý, chúng tôi đang phân chia một tập hợp các điểm thời gian cố định thành nhiều chuỗi, mỗi chuỗi đại diện cho một ngày và mỗi chuỗi phải tôn trọng giới hạn khoảng cách tối thiểu giữa các thời điểm được chọn liên tiếp. Mục tiêu là giảm thiểu số lượng trình tự như vậy là cần thiết. 

Ràng buộc chính là n lên tới 200000, điều này buộc phải áp dụng cách tiếp cận O(n log n) hoặc O(n). Bất cứ điều gì quét liên tục đã được chỉ định ngắt quãng hoặc cố gắng mô phỏng từng ngày một cách tham lam mà không có cấu trúc sẽ thoái hóa thành O(n²) trong trường hợp xấu nhất và thất bại. 

MỘT
