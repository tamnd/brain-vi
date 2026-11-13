---
title: "CF 1741C - Giảm thiểu độ dày"
description: "Chúng ta được cho một mảng các số nguyên dương và được yêu cầu chia nó thành các phân đoạn liên tiếp sao cho tất cả các phân đoạn đều có tổng bằng nhau. Mỗi phân đoạn phải liền kề nhau và mỗi phần tử thuộc về chính xác một phân đoạn."
date: "2026-06-09T16:25:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1741
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 826 (Div. 3)"
rating: 1100
weight: 1741
solve_time_s: 159
verified: false
draft: false
---

[CF 1741C - Giảm thiểu độ dày](https://codeforces.com/problemset/problem/1741/C) 

**Đánh giá:** 1100 
**Tags:** bạo lực, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và được yêu cầu chia nó thành các phân đoạn liên tiếp sao cho tất cả các phân đoạn đều có tổng bằng nhau. Mỗi phân đoạn phải liền kề nhau và mỗi phần tử thuộc về chính xác một phân đoạn. Độ dày của phần phân chia là chiều dài của đoạn lớn nhất và mục tiêu là giảm thiểu độ dày này. 

Các ràng buộc ở mức vừa phải: độ dài mảng cho mỗi trường hợp thử nghiệm tối đa là 2000 và tổng số trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 2000. Điều này cho phép một thuật toán có độ phức tạp về thời gian trong trường hợp xấu nhất$O(n^2)$mỗi trường hợp thử nghiệm, vì$2000^2$hoạt động có thể chấp nhận được trong giới hạn 2 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng không thể chia đều ngoại trừ việc lấy toàn bộ mảng làm một phân đoạn, ví dụ: một mảng như `[10, 23, 7,
