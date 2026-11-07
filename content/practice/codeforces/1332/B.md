---
title: "CF 1332B - Màu tổng hợp"
description: "Người đánh giá là đúng. Các chuỗi được hiển thị không phải là chuỗi cộng, vì số lượng $2^g,2^h,2^k,2^m$ được sử dụng làm số hiệu mà không xuất hiện trước tiên dưới dạng phần tử chuỗi. Lập luận phải được xây dựng lại từ định nghĩa về chuỗi sao."
date: "2026-06-11T16:06:43+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1332
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 630 (Div. 2)"
rating: 1400
weight: 1332
solve_time_s: 115
verified: false
draft: false
---

[CF 1332B - Tô màu tổng hợp](https://codeforces.com/problemset/problem/1332/B) 

**Đánh giá:** 1400 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
Người đánh giá là đúng. Các chuỗi được hiển thị không phải là chuỗi cộng, vì số lượng$2^g,2^h,2^k,2^m$được sử dụng làm lệnh triệu tập mà không xuất hiện lần đầu dưới dạng các phần tử chuỗi. Lập luận phải được xây dựng lại từ định nghĩa về chuỗi sao. 

Giả sử bốn trường hợp của Định lý C là$$n=2^m+1,$$

$$n=2^m+2^k+1 \qquad (m>k\ge 0),$$

$$n=2^m+2^k+2^h+1 \qquad (m>k>h\ge 0),$$

$$n=2^m+2^k+2^h+2^g+1 \qquad (m>k>h>g\ge 0).$$Trong mỗi trường hợp$A$là số quyền hạn của$2$xảy ra ở$n-1$. Chúng ta phải trưng bày một chuỗi sao có độ dài$A+2$. 

Nhớ lại rằng một chuỗi sao thỏa mãn$$a_r=a_{r-1}+a_j, \qquad j<r.$$Do đó, mọi thuật ngữ mới phải được lấy từ thuật ngữ liền trước bằng cách thêm một phần tử chuỗi trước đó. 

Quan sát quan trọng là sức mạnh của cả hai đã tạo thành một chuỗi sao:$$1,2,4,\ldots,2^m.$$Một lần$2^m$đã đạt tới, mỗi lũy thừa nhỏ hơn của 2 xuất hiện trong khai triển nhị phân của$n-1$đã có sẵn như một phần tử chuỗi trước đó. Sau đó chúng ta có thể cộng các lũy thừa đó lần lượt vào giá trị hiện tại. 

### Trường hợp 1 

hãy để$$n=2^m+1.$$Hãy xem xét chuỗi$$1,2,4,\ldots,2^m,2^m+1.$$đầu tiên$m$các bước được nhân đôi,$$2^r=2^{r-1}+2^{r-1},$$vì vậy chúng là những bước đi của ngôi sao. 

Bước cuối cùng là$$2^m+1=2^m+1,$$Ở đâu$1$là phần tử đầu tiên của chuỗi. 

Vì vậy đây là một chuỗi sao. 

chiều dài của nó là$$m+1.$$Trong Định lý C, đối với trường hợp này,$$A=m-1,$$kể từ đây$$A+2=m+1.$$Do đó đạt được độ dài cần thiết. 

### Trường hợp 2 

hãy để$$n=2^m+2^k+1, \qquad m>k\ge0.$$Lấy$$1,2,4,\ldots,2^m, \quad 2^m+2^k, \quad 2^m+2^k+1.$$Sau khi đạt$2^m$, phần tử$2^k$đã xảy ra trước đó trong chuỗi. Kể từ đây$$2^m+2^k = 2^m+2^k$$là một bước sao. 

Từ$1$cũng là một phần tử dây chuyền có trước đó,$$2^m+2^k+1 = (2^m+2^k)+1$$là một bước sao khác. 

Do đó toàn bộ chuỗi là một chuỗi sao. 

chiều dài của nó là$$m+2.$$Đối với trường hợp này của Định lý C,$$A=m,$$Vì thế$$A+2=m+2.$$### Trường hợp 3 

hãy để$$n=2^m+2^k+2^h+1, \qquad m>k>h\ge0.$$Coi như$$1,2,4,\ldots,2^m, \quad 2^m+2^k, \quad 2^m+2^k+2^h, \quad 2^m+2^k+2^h+1.$$Quyền lực$2^k$Và$2^h$đã xuất hiện sớm hơn trong chuỗi, vì vậy$$2^m+2^k = 2^m+2^k,$$Và$$2^m+2^k+2^h = (2^m+2^k)+2^h$$là các bước sao. 

Cuối cùng,$$2^m+2^k+2^h+1 = (2^m+2^k+2^h)+1,$$Và$1$là một phần tử chuỗi trước đó. 

Do đó chuỗi là chuỗi sao. 

chiều dài của nó là$$m+3.$$Đối với trường hợp này,$$A=m+1,$$Vì vậy$$A+2=m+3.$$### Trường hợp 4 

hãy để$$n=2^m+2^k+2^h+2^g+1, \qquad m>k>h>g\ge0.$$Lấy$$1,2,4,\ldots,2^m,$$theo sau là$$2^m+2^k,$$

$$2^m+2^k+2^h,$$

$$2^m+2^k+2^h+2^g,$$

$$2^m+2^k+2^h+2^g+1.$$Mỗi quyền lực$$2^k,\quad 2^h,\quad 2^g,$$đã xảy ra sớm hơn trong chuỗi và$1$là phần tử ban đầu Do đó mỗi một trong bốn bước cuối cùng đều có dạng$$a_r=a_{r-1}+a_j$$với$j<r$. 

Vậy chuỗi đó là chuỗi sao. 

chiều dài của nó là$$m+4.$$Đối với trường hợp này,$$A=m+2,$$Vì thế$$A+2=m+4.$$Trong cả bốn trường hợp, chúng tôi đã xây dựng được các chuỗi sao rõ ràng có độ dài chính xác bằng$A+2$. Do đó phát biểu của Định lý C vẫn đúng khi$l$được thay thế bởi$l^*$. ∎
