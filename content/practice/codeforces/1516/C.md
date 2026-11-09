---
title: "CF 1516C - Lại phân vùng Ehab cho bé"
description: "Phương pháp 1b đã cung cấp ý tưởng chính. Nếu $(umldots u1u0)3$ là số nguyên bậc ba, thì giá trị thập phân của nó được lấy từ dạng lồng nhau $$(cdots((umcdot3+u{m-1})cdot3+u{m-2})cdots)cdot3+u0.$$ Đối với bài tập viết bằng bút chì và giấy, điều này có nghĩa là: Bắt đầu với chữ số đứng đầu."
date: "2026-06-10T18:25:18+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1516
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 717 (Div. 2)"
rating: 1700
weight: 1516
solve_time_s: 171
verified: false
draft: false
---

[CF 1516C - Lại phân vùng Baby Ehab](https://codeforces.com/problemset/problem/1516/C) 

**Đánh giá:** 1700 
**Tags:** bitmask, thuật toán xây dựng, dp, toán học 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Phương pháp 1b đã cung cấp ý tưởng chính. Nếu như$(u_m\ldots u_1u_0)_3$là một số nguyên ba ngôi, giá trị thập phân của nó được lấy từ dạng lồng nhau$$(\cdots((u_m\cdot3+u_{m-1})\cdot3+u_{m-2})\cdots)\cdot3+u_0.$$Đối với công việc bằng bút chì và giấy, điều này có nghĩa là: 

Bắt đầu với chữ số hàng đầu. Nhân kết quả hiện tại nhiều lần với$3$và thêm chữ số thứ ba tiếp theo. 

Chỉ nhân với$3$và bổ sung$0$,$1$, hoặc$2$được yêu cầu. 

Vì$$(12120112120210)_3,$$quá trình tính toán tiến hành như sau:$$\begin{array}{rcl} 1 &\to& 1\cdot3+2=5,\\ 5 &\to& 5\cdot3+1=16,\\ 16 &\to& 16\cdot3+2=50,\\ 50 &\to& 50\cdot3+0=150,\\ 150 &\to& 150\cdot3+1=451,\\ 451 &\to& 451\cdot3+1=1354,\\ 1354 &\to& 1354\cdot3+2=4064,\\ 4064 &\to& 4064\cdot3+1=12193,\\ 12193 &\to& 12193\cdot3+2=36581,\\ 36581 &\to& 36581\cdot3+0=109743,\\ 109743 &\to& 109743\cdot3+2=329231,\\ 329231 &\to& 329231\cdot3+1=987694,\\ 987694 &\to& 987694\cdot3+0=2963082. \end{array}$$Kể từ đây$$(12120112120210)_3=(2963082)_{10}.$$Do đó giá trị thập phân là$$\boxed{2963082}.$$Để chuyển đổi theo hướng ngược lại, từ số thập phân sang số ba, hãy sử dụng Cách 1a với$B=3$. Nhiều lần chia cho$3$và ghi số dư. 

Đối với một số nguyên$u$,$$u=3q_0+r_0,\qquad 0\le r_0<3,$$sau đó$$q_0=3q_1+r_1,$$và cứ như vậy cho đến khi thương số trở thành$0$. Các chữ số bậc ba là số dư được đọc theo thứ tự ngược lại:$$u=(\ldots r_2r_1r_0)_3.$$Do đó, một phương pháp nhanh chóng từ số thập phân đến số thập phân là phép nhân lặp lại với$3$và phép cộng chữ số tiếp theo; một phương pháp thập phân nhanh đến bậc ba được lặp đi lặp lại phép chia cho$3$với số dư. 

∎
