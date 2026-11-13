---
title: "CF 1760A - Số trung bình"
description: "Nhiệm vụ là xác định số không nhỏ nhất cũng không lớn nhất trong ba số nguyên phân biệt. Mỗi trường hợp đầu vào cung cấp ba số và đầu ra phải xác định giá trị “ở giữa”."
date: "2026-06-09T14:20:40+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1760
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 835 (Div. 4)"
rating: 800
weight: 1760
solve_time_s: 181
verified: false
draft: false
---

[CF 1760A - Số trung bình](https://codeforces.com/problemset/problem/1760/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác định số không nhỏ nhất cũng không lớn nhất trong ba số nguyên phân biệt. Mỗi trường hợp đầu vào cung cấp ba số và đầu ra phải xác định giá trị “ở giữa”. Về cơ bản, đây là việc tìm giá trị trung bình của ba giá trị nhưng không sắp xếp một bộ sưu tập lớn hơn. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có chính xác ba số nguyên, tất cả đều nằm trong phạm vi từ 1 đến 20. Phạm vi nhỏ và kích thước cố định của ba số có nghĩa là chúng ta có thể sử dụng các phép so sánh đơn giản hoặc logic điều kiện tối thiểu mà không phải lo lắng về hiệu suất. Các trường hợp cạnh bao gồm các chuỗi trong đó các số theo thứ tự tăng dần, thứ tự giảm dần hoặc có giá trị ở giữa ở bất kỳ vị trí nào. Cách tiếp cận bất cẩn, chẳng hạn như luôn chọn số đầu vào thứ hai, sẽ thất bại nếu số trung vị không ở vị trí đó. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là so sánh từng số với hai số còn lại. Người ta có thể kiểm tra xem`a`nằm giữa`b`Và`c`, liệu`b`nằm giữa`a`Và`c`, hoặc
