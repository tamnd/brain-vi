---
title: "CF 1310F - Mật mã xấu"
description: "Có, bất đẳng thức $nu(n) le 2^{l(n) - lambda(n)}$ đúng với mọi số nguyên dương $n$. Hãy xem xét một chuỗi bổ sung có độ dài tối thiểu $l(n)$ và đặt $lambda(n)$ là độ dài của chuỗi ngắn nhất chỉ bao gồm các bước nhân đôi."
date: "2026-06-11T17:22:12+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1310
codeforces_index: "F"
codeforces_contest_name: "VK Cup 2019-2020 - Elimination Round (Engine)"
rating: 3400
weight: 1310
solve_time_s: 124
verified: false
draft: false
---

[CF 1310F - Mật mã xấu](https://codeforces.com/problemset/problem/1310/F) 

**Đánh giá:** 3400 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
Vâng, sự bất bình đẳng$\nu(n) \le 2^{l(n) - \lambda(n)}$đúng với mọi số nguyên dương$n$. Xét một chuỗi bổ sung có độ dài tối thiểu$l(n)$và để$\lambda(n)$là độ dài của chuỗi ngắn nhất chỉ bao gồm các bước nhân đôi. Mỗi bước không nhân đôi nhiều nhất có thể nhân đôi số lượng đơn vị trong biểu diễn nhị phân của số mũ hiện tại. Vì vậy, bắt đầu từ một$1$, sau đó$l(n) - \lambda(n)$bước không nhân đôi, số bước$\nu(n)$thỏa mãn$\nu(n) \le 2^{l(n) - \lambda(n)}$. 

Điều này thiết lập ràng buộc trực tiếp. Do đó, áp dụng bất đẳng thức này cho$2^n - 1$, có biểu diễn nhị phân bao gồm$n$những cái, chúng tôi có được giới hạn dưới$l(2^n - 1) \ge n - 1 + \lfloor \lg n \rfloor$, phù hợp với phương trình (17) và (49). Điều này hoàn thành lập luận. ∎
