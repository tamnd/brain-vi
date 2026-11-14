---
title: "CF 1776M - Parmigiana Với Hải Sản"
description: "Đây là bài toán Loại B (chứng minh bất đẳng thức), không phải Loại C. Nhiệm vụ là chứng minh rằng $$frac1{a^3(b+c)}+frac1{b^3(c+a)}+frac1{c^3(a+b)}ge frac32$$ với mọi $a,b,c$ thỏa mãn $abc=1$."
date: "2026-06-09T11:51:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "M"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 3000
weight: 1776
solve_time_s: 140
verified: false
draft: false
---

[CF 1776M - Parmigiana với hải sản](https://codeforces.com/problemset/problem/1776/M) 

**Đánh giá:** 3000 
**Tags:** tìm kiếm nhị phân, dp, tham lam, cây cối 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Kiểm tra loại sự cố 

Đây là bài toán **Loại B (chứng minh bất đẳng thức)**, không phải Loại C. 

Nhiệm vụ là chứng minh điều đó$$\frac1{a^3(b+c)}+\frac1{b^3(c+a)}+\frac1{c^3(a+b)}\ge \frac32$$cho tất cả tích cực$a,b,c$thỏa mãn$abc=1$. 

Một lời giải hợp lệ phải thiết lập được bất đẳng thức cho tất cả các bộ ba có thể chấp nhận được và xác định chính xác bất kỳ trường hợp đẳng thức nào nếu nó phát sinh từ lập luận. 

## Xác minh từng bước 

### Bước 1: Viết lại biểu thức bằng cách sử dụng$abc=1$Giải pháp yêu cầu$$\frac1{a^3(b+c)}=\frac{b^3c^3}{b+c},$$và tương tự đối với các số hạng tuần hoàn. 

**Đánh giá:** HỢP LỆ. 

Từ$abc=1$, chúng tôi có$a=\frac1{bc}$, kể từ đây$a^{-3}=b^3c^3$. 

Như vậy$$\sum \frac1{a^3(b+c)} = \sum \frac{b^3c^3}{b+c}.$$### Bước 2: Áp dụng Engel-Cauchy 

Giải pháp áp dụng 

\sum \frac{(b^{3/2}c^{3/2})^2}{b+c} \ge \frac{\left(\sum b^{3/2}c^{3/2}\right)^2} {(a+b)+(b+c)+(
