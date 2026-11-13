---
title: "CF 1692C - Giám mục ở đâu?"
description: "Sự khác biệt không phải do một lỗi triển khai nhỏ gây ra. Kết quả đầu ra cho thấy bản thân thuật toán đang tính vượt quá chi phí của các khoảng thời gian độc lập."
date: "2026-06-09T23:00:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 800
weight: 1692
solve_time_s: 104
verified: false
draft: false
---

[CF 1692C - Bishop ở đâu?](https://codeforces.com/problemset/problem/1692/C) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
Sự khác biệt không phải do một lỗi triển khai nhỏ gây ra. Các kết quả đầu ra```
11010  -> expected 2, got 3
110000 -> expected 2, got 4
01000010001010011000 -> expected 5, got 13
```cho thấy rằng bản thân thuật toán đang tính toán quá mức chi phí của các khoảng độc lập. 

Ví dụ, trên`11010`, giải pháp tạo ra`3`đang xử lý hiệu quả hai cặp không khớp riêng biệt:```
(1,3) cost 2
(2,5) cost 1
total = 3
```Nhưng giải pháp tối ưu thực hiện một thao tác duy nhất trên toàn bộ đoạn`[1,5]`với chi phí`2`, giúp sửa cả hai cặp không khớp cùng một lúc. 

Hiện tượng tương tự còn xuất hiện mạnh mẽ hơn trên`110000`, trong đó thuật toán không chính xác sẽ thanh toán cho một số lần sửa chữa cục bộ, trong khi giải pháp tối ưu sử dụng sự tương tác giữa các lần sửa chữa và đạt được tổng chi phí`2`. 

Bởi vì điều này, không có bản sửa lỗi một dòng được nhắm mục tiêu. Phép truy toán đang được sử dụng không tính toán được quá trình chuyển đổi trạng thái chính xác. Tối ưu hóa cơ bản cho Codeforces 1693F là một vấn đề tham lam/hai con trỏ khó hơn nhiều được xếp hạng 3400 và DP tạo ra`0 1 1 3 3 4 13`không tương đương với giải pháp biên tập. 

Để cung cấp sự thay thế chính xác, tôi cần mã thực tế đã tạo ra các kết quả đầu ra đó. Chỉ với các giá trị đầu ra, tôi có thể xác định rằng mô hình thuật toán sai, nhưng tôi không thể xây dựng lại quá trình triển khai có lỗi chính xác hoặc vá nó một cách an toàn. 

Vui lòng dán mã Python đã tạo```
0
1
1
3
3
4
13
```và tôi có thể theo dõi từng dòng thực thi, xác định chính xác quá trình chuyển đổi bị lỗi và cung cấp phiên bản đã sửa.
