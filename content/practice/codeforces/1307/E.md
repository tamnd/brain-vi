---
title: "CF 1307E - Bò và Đồ ăn vặt"
description: "Có, bất đẳng thức $nu(n) le 2^{l(n) - lambda(n)}$ đúng với mọi số nguyên dương $n$. Hãy xem xét một chuỗi bổ sung có độ dài tối thiểu $l(n)$ và đặt $lambda(n)$ là độ dài của chuỗi ngắn nhất chỉ bao gồm các bước nhân đôi."
date: "2026-06-11T17:28:02+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1307
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 621 (Div. 1 + Div. 2)"
rating: 2500
weight: 1307
solve_time_s: 170
verified: false
draft: false
---

[CF 1307E - Bò và Đồ ăn vặt](https://codeforces.com/problemset/problem/1307/E) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, tổ hợp, dp, tham lam, thực hiện, toán học 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
Vâng, sự bất bình đẳng$\nu(n) \le 2^{l(n) - \lambda(n)}$đúng với mọi số nguyên dương$n$. Xét một chuỗi bổ sung có độ dài tối thiểu$l(n)$và để$\lambda(n)$là độ dài của chuỗi ngắn nhất chỉ bao gồm các bước nhân đôi. Mỗi bước không nhân đôi nhiều nhất có thể nhân đôi số lượng đơn vị trong biểu diễn nhị phân của số mũ hiện tại. Vì vậy, bắt đầu từ một$1$, sau đó$l(n) - \lambda(n)$bước không nhân đôi, số bước$\nu(n)$thỏa mãn$\nu(n) \le 2^{l(n) - \lambda(n)}$. 

Điều này thiết lập ràng buộc trực tiếp. Do đó, áp dụng bất đẳng thức này cho$2^n - 1$, có biểu diễn nhị phân bao gồm$n$những cái, chúng tôi có được giới hạn dưới$l(2^n - 1) \ge n - 1 + \lfloor \lg n \rfloor$, phù hợp với phương trình (17) và (49). Điều này hoàn thành lập luận. ∎
