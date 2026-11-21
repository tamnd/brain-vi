---
title: "CF 2193G - Đường đi trong cây"
description: "Chúng ta được cho một cây, là một đồ thị tuần hoàn vô hướng, liên thông của $n$ đỉnh. Hai đỉnh ẩn, $x$ và $y$, xác định một đường đi duy nhất vì cây có chính xác một đường đi đơn giản giữa bất kỳ cặp đỉnh nào."
date: "2026-06-07T20:53:34+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "interactive", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 2100
weight: 2193
solve_time_s: 185
verified: false
draft: false
---

[CF 2193G - Đường dẫn trong cây](https://codeforces.com/problemset/problem/2193/G) 

**Xếp hạng:** 2100 
**Thẻ:** dfs và tương tự, tương tác, phân loại, cây 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, là một đồ thị tuần hoàn vô hướng, liên thông của$n$đỉnh. Hai đỉnh ẩn,$x$Và$y$, xác định một đường đi duy nhất vì cây có chính xác một đường đi đơn giữa bất kỳ cặp đỉnh nào. Chúng tôi không biết danh tính của$x$Và$y$, nhưng chúng ta có thể tương tác với cây bằng cách hỏi liệu đường đi giữa hai đỉnh được chọn bất kỳ có$a$Và$b$giao nhau với con đường ẩn giữa$x$Và$y$. Mục tiêu của chúng ta là tìm ra ít nhất một đỉnh trên đường đi ẩn bằng cách sử dụng nhiều nhất
