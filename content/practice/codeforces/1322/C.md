---
title: "CF 1322C - Mì Ăn Liền"
description: "Có, bất đẳng thức $nu(n) le 2^{l(n) - lambda(n)}$ đúng với mọi số nguyên dương $n$. Hãy xem xét một chuỗi bổ sung có độ dài tối thiểu $l(n)$ và đặt $lambda(n)$ là độ dài của chuỗi ngắn nhất chỉ bao gồm các bước nhân đôi."
date: "2026-06-11T16:48:39+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "hashing", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1322
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 626 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 2300
weight: 1322
solve_time_s: 162
verified: false
draft: false
---

[CF 1322C - Mì ăn liền](https://codeforces.com/problemset/problem/1322/C) 

**Đánh giá:** 2300 
**Tags:** đồ thị, hàm băm, toán, lý thuyết số 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
Vâng, sự bất bình đẳng$\nu(n) \le 2^{l(n) - \lambda(n)}$đúng với mọi số nguyên dương$n$. Xét một chuỗi bổ sung có độ dài tối thiểu$l(n)$và để$\lambda(n)$là độ dài của chuỗi ngắn nhất chỉ bao gồm các bước nhân đôi. Mỗi bước không nhân đôi nhiều nhất có thể nhân đôi số lượng đơn vị trong biểu diễn nhị phân của số mũ hiện tại. Vì vậy, bắt đầu từ một$1$, sau đó$l(n) - \lambda(n)$bước không nhân đôi, số bước$\nu(n)$thỏa mãn$\nu(n) \le 2^{l(n) - \lambda(n)}$. 

Điều này thiết lập ràng buộc trực tiếp. Do đó, áp dụng bất đẳng thức này cho$2^n - 1$, có biểu diễn nhị phân bao gồm$n$những cái, chúng tôi có được giới hạn dưới$l(2^n - 1) \ge n - 1 + \lfloor \lg n \rfloor$, phù hợp với phương trình (17) và (49). Điều này hoàn thành lập luận. ∎
