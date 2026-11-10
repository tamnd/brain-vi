---
title: "CF 154A - Bài tập về nhà"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường và một tập hợp các cặp chữ cái bị cấm. Mỗi cặp bị cấm chứa hai chữ cái riêng biệt và nếu một cặp (a, b) bị cấm thì cả \"ab\" và \"ba\" đều bị cấm làm ký tự liền kề. Chúng tôi có thể xóa bất kỳ ký tự nào khỏi chuỗi."
date: "2026-06-02T16:49:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 154
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 109 (Div. 1)"
rating: 1600
weight: 154
solve_time_s: 30
verified: false
draft: false
---

[CF 154A - Bài tập về nhà](https://codeforces.com/problemset/problem/154/A) 

**Đánh giá:** 1600 
**Thẻ:** tham lam 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường và một tập hợp các cặp chữ cái bị cấm. Mỗi cặp cấm chứa hai chữ cái riêng biệt, và nếu một cặp`(a, b)`bị cấm thì cả hai`"ab"`Và`"ba"`bị cấm như các ký tự liền kề. 

Chúng tôi có thể xóa bất kỳ ký tự nào khỏi chuỗi. Sau khi xóa, các ký tự còn lại sẽ đóng lại, do đó các ký tự không liền kề ban đầu có thể trở thành hàng xóm. Mục tiêu của chúng tôi là loại bỏ càng ít ký tự càng tốt để không có cặp ký tự bị cấm nào xuất hiện giữa các ký tự liền kề trong chuỗi cuối cùng. 

Độ dài chuỗi tối đa là`100000`, trong khi có nhiều nhất`13`cặp cấm. Giải pháp quy hoạch bậc hai hoặc động trên tất cả các vị trí sẽ quá đắt. Với`n = 100000`, thậm chí`O(n²)`có nghĩa là về`10^10`hoạt động hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Hạn chế bất thường là mỗi chữ cái thuộc về nhiều nhất một chữ p bị cấm.
