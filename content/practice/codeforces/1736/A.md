---
title: "CF 1736A - Tạo A bằng B"
description: "Đây là một vấn đề Loại B (chứng minh). Tuyên bố yêu cầu một yêu cầu chung cho tất cả các số tự nhiên $n$, cụ thể là ít nhất một trong số $S(7n)$ hoặc $S(9n)$ vượt quá $S(n)/5$."
date: "2026-06-09T18:00:22+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1736
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 825 (Div. 2)"
rating: 800
weight: 1736
solve_time_s: 117
verified: false
draft: false
---

[CF 1736A - Tạo A bằng B](https://codeforces.com/problemset/problem/1736/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Kiểm tra loại sự cố 

Đây là vấn đề **Loại B (chứng minh)**. Tuyên bố yêu cầu một yêu cầu phổ quát cho tất cả các số tự nhiên$n$, cụ thể là ít nhất một trong số$S(7n)$hoặc$S(9n)$vượt quá$S(n)/5$. Một lời giải hợp lệ phải thiết lập được sự bất đẳng thức một cách chặt chẽ cho mọi$n$, không chỉ cho những ví dụ nhỏ hay thông qua trực giác. Cần phải suy luận logic từng bước với những lý do chính đáng. 

## Xác minh từng bước 

**Bước 1: Thăm dò với các giá trị nhỏ của$n$** - CÓ HIỆU LỰC. Các tính toán cho$n=1,2,10,111$minh họa chính xác tính hợp lý của bất đẳng thức, nhưng chúng không tạo thành một bằng chứng. 

**Bước 2: Bổ đề 1, khẳng định$S(9k) \ge S(k)$cho tất cả$k$**
