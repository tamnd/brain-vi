---
title: "CF 154B - Máy va chạm"
description: "Chúng tôi quản lý một tập hợp các máy va chạm được đánh số từ 1 đến $n$. Ban đầu mọi máy va chạm đều bị tắt. Sau đó chúng tôi nhận được một chuỗi lệnh. Một lệnh có thể cố gắng kích hoạt hoặc vô hiệu hóa máy va chạm."
date: "2026-06-02T16:44:24+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 154
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 109 (Div. 1)"
rating: 1600
weight: 154
solve_time_s: 32
verified: false
draft: false
---

[CF 154B - Máy va chạm](https://codeforces.com/problemset/problem/154/B) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi quản lý một bộ máy va chạm được đánh số từ 1 đến$n$. Ban đầu mọi máy va chạm đều bị tắt. Sau đó chúng tôi nhận được một chuỗi lệnh. Một lệnh có thể cố gắng kích hoạt hoặc vô hiệu hóa máy va chạm. 

Quy tắc an toàn là điều quan trọng duy nhất: tất cả các số máy va chạm đang hoạt động phải là số nguyên tố cùng nhau theo cặp. Khi chúng tôi cố gắng kích hoạt máy va chạm$x$, việc kích hoạt chỉ được phép nếu mọi máy va chạm hiện đang hoạt động có ước số chung lớn nhất bằng 1 với$x$. Nếu một số máy va chạm đang hoạt động chia sẻ thừa số nguyên tố với$x$, việc kích hoạt phải bị từ chối và chúng tôi phải báo cáo một máy va chạm xung đột. Nếu máy va chạm đã hoạt động, chúng tôi sẽ báo cáo "Đã bật". 

Để hủy kích hoạt, hành vi đơn giản hơn. Nếu máy va chạm hiện đang hoạt động, chúng tôi sẽ xóa nó và in "Thành công". Nếu không, chúng tôi sẽ in "Đã tắt". 

Các ràng buộc đủ lớn nên việc mô phỏng đơn giản là không đủ. Cả hai$n$và số lượng hoạt động$m$có thể đạt được$10^5$. Một giải pháp so sánh một máy va chạm mới được kích hoạt với tất cả các máy va chạm đang hoạt động sẽ yêu cầu tới$10^5$tính toán gcd cho mỗi hoạt động. Trong trường hợp xấu nhất, điều này trở nên đại khái$10^{10}$hoạt động, vượt xa những gì phù hợp với giới hạn thời gian hai giây. 

Quan sát quan trọng là xung đột nảy sinh do hai
