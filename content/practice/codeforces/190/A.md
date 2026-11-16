---
title: "CF 190A - Vasya và xe buýt"
description: "Chúng ta được yêu cầu tính giá vé xe buýt tối thiểu và tối đa thu được khi có số người lớn n và trẻ em m trên xe buýt. Mỗi người lớn phải trả một rúp cho mình và mỗi người lớn có thể nhận miễn phí tối đa một đứa trẻ. Bất kỳ đứa trẻ bổ sung nào đều có giá một rúp mỗi đứa."
date: "2026-06-03T01:26:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 190
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 120 (Div. 2)"
rating: 1100
weight: 190
solve_time_s: 21
verified: false
draft: false
---

[CF 190A - Vasya và chiếc xe buýt](https://codeforces.com/problemset/problem/190/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, toán học 
**Thời gian giải:** 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính giá vé xe buýt tối thiểu và tối đa thu được cho một số người lớn`n`và trẻ em`m`trên một chiếc xe buýt. Mỗi người lớn phải trả một rúp cho mình và mỗi người lớn có thể nhận miễn phí tối đa một đứa trẻ. Bất kỳ đứa trẻ bổ sung nào đều có giá một rúp mỗi đứa. Trẻ em không thể đi xe một mình, vì vậy`m`không thể vượt quá`n`lần số trẻ em mà mỗi người lớn có thể chi trả, nếu không thì tình huống này là không thể. 

Đầu vào là hai số nguyên`n`Và`m`. Đầu ra là hai số nguyên biểu thị tổng giá vé tối thiểu và tối đa hoặc từ "Không thể" nếu không thể có sự kết hợp giữa người lớn và trẻ em trên xe buýt. 

Với những hạn chế`0 ≤ n, m ≤ 10^5`,
