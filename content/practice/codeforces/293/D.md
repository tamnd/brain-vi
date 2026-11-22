---
title: "CF 293D - Ksusha và hình vuông"
description: "Chúng ta có một đa giác lồi có các đỉnh có tọa độ nguyên. Xem xét mọi điểm mạng, nghĩa là mọi điểm có tọa độ nguyên, nằm bên trong đa giác hoặc trên ranh giới của nó."
date: "2026-06-05T17:22:49+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math", "probabilities", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 293
codeforces_index: "D"
codeforces_contest_name: "Croc Champ 2013 - Round 2"
rating: 2700
weight: 293
solve_time_s: 43
verified: false
draft: false
---

[CF 293D - Ksusha và Square](https://codeforces.com/problemset/problem/293/D) 

**Đánh giá:** 2700 
**Tags:** hình học, toán học, xác suất, hai con trỏ 
**Thời gian giải:** 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi có các đỉnh có tọa độ nguyên. Xem xét mọi điểm mạng, nghĩa là mọi điểm có tọa độ nguyên, nằm bên trong đa giác hoặc trên ranh giới của nó. 

Trong số tất cả các cặp điểm mạng riêng biệt không có thứ tự, chúng ta chọn ngẫu nhiên một cặp đồng nhất. Hai điểm đó trở thành hai góc đối diện của hình vuông. Nhiệm vụ là tính diện tích dự kiến ​​của hình vuông đó. 

Hình học của hình vuông thực sự là phần dễ dàng. Nếu hai điểm là hai đỉnh đối diện của một hình vuông và có độ dài đường chéo là$d$, thì độ dài cạnh là$d/\sqrt 2$, vậy diện tích là$$\frac{d^2}{2}.$$Toàn bộ vấn đề trở thành: 

\mathbb E[\text{area}] = \frac12 \cdot \mathbb E[\text{khoảng cách bình phương be]()
