---
title: "CF 1744F - MEX vs MED"
description: "Đặt hình vuông vào tọa độ với (A(0,0)), (B(1,0)), (C(1,1)), (D(0,1)). Khi đó (E) nằm trên (AB) và (F) trên (BC) với (BE=BF). Viết (E=(x,0)) cho ra (BE=1-x), trong khi (F=(1,y)) cho ra (BF=y), do đó (y=1-x) và (F=(1,1-x))."
date: "2026-06-09T15:58:55+07:00"
tags: ["codeforces", "competitive-programming", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1744
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round  828 (Div. 3)"
rating: 2000
weight: 1744
solve_time_s: 114
verified: false
draft: false
---

[CF 1744F - MEX vs MED](https://codeforces.com/problemset/problem/1744/F) 

**Xếp hạng:** 2000 
**Tags:** toán, hai con trỏ 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Thăm dò 

Đặt hình vuông vào tọa độ với (A(0,0)), (B(1,0)), (C(1,1)), (D(0,1)). Khi đó (E) nằm trên (AB) và (F) trên (BC) với (BE=BF). Viết (E=(x,0)) cho ra (BE=1-x), trong khi (F=(1,y)) cho ra (BF=y), do đó (y=1-x) và (F=(1,1-x)). 

Trung điểm là 

[ 
L=\left(\frac{x+1}{2},\frac{1-x}{2}\right),\quad 
N=\left(\frac12,1-\frac{x}{2}\right),\quad 
O=\left(\frac12,\frac12\right). 
] 

Điểm (K) được lấy từ giao điểm của (AL) với (DF). 

Mục đích là để chứng minh rằng (C(1,1),K,L,O,N) là đồng quy. Vì tất cả các tọa độ đều là biểu thức hữu tỉ rõ ràng trong (x), nên cách tiếp cận ổn định nhất là xác định đường tròn đi qua ba điểm phụ thuộc vào (x) và xác minh hai điểm còn lại nằm trên đó. 

Điểm tinh tế nhất là đảm bảo rằng đường tròn được tính toán chính xác từ một cấu hình trong đó một điểm (L) phụ thuộc vào tham số (x), do đó các hệ số của đường tròn cũng phụ thuộc vào (x). 

## Hiểu vấn đề 

Nhiệm vụ là chứng minh rằng năm điểm được dựng rõ ràng trong một hình vuông nằm trên một đường tròn chung. 

Khó khăn chính là một trong các điểm xác định của đường tròn, (L), phụ thuộc vào một tham số tự do (x), do đó đường tròn phải
