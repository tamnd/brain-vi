---
title: "CF 105164H - APPQ điểm cao nhất"
description: "Chúng ta được cung cấp một vũ trụ các con số được tạo ra theo một cách rất có cấu trúc. Mỗi số tương ứng với một vectơ số mũ trên các số nguyên tố $n$ đầu tiên."
date: "2026-06-27T10:45:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "H"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 42
verified: false
draft: false
---

[CF 105164H - APPQ có điểm cao nhất](https://codeforces.com/problemset/problem/105164/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một vũ trụ các con số được tạo ra theo một cách rất có cấu trúc. Mỗi số tương ứng với một vectơ số mũ trên số đầu tiên$n$số nguyên tố. các$i$-th tọa độ$e_i$cho chúng ta biết bao nhiêu lần$i$-số nguyên tố thứ xuất hiện trong phân tích nhân tử và mỗi tọa độ được giới hạn bởi$0 \le e_i \le a_i$. Vì vậy, toàn bộ đầu vào mô tả một lưới hữu hạn các vectơ số mũ và mỗi số hợp lệ là một điểm trong lưới này. 

Chúng ta phải chọn một tập con của những số này theo một hạn chế về cấu trúc theo cặp. Nếu lấy hai số$x < y$, thế thì bị cấm đấy
