---
title: "CF 103464B - Ngày Palindromic"
description: "Một chữ cái bổ sung theo nghĩa của Mục 7.2.1.2 gán các chữ số thập phân riêng biệt cho các chữ cái riêng biệt để nhận dạng số học chính thức giữa các từ trở thành đẳng thức thực sự của các số nguyên cơ số 10."
date: "2026-07-03T06:54:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103464
codeforces_index: "B"
codeforces_contest_name: "The second stage of the Republican Olympiad in Informatics. Mogilev region, 2021."
rating: 0
weight: 103464
solve_time_s: 127
verified: false
draft: false
---

[CF 103464B - Ngày Palindromic](https://codeforces.com/problemset/problem/103464/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Một chữ cái bổ sung theo nghĩa của Mục 7.2.1.2 gán các chữ số thập phân riêng biệt cho các chữ cái riêng biệt để nhận dạng số học chính thức giữa các từ trở thành đẳng thức thực sự của các số nguyên cơ số 10. Một _chữ cái phụ gia thuần túy với các từ có năm chữ cái_ là một dạng đồng nhất$$W_1 + W_2 + \cdots + W_r = V_1 + V_2 + \cdots + V_r$$mỗi từ ở đâu$W_i, V_i$có chiều dài$5$, mỗi chữ cái biểu thị một chữ số riêng biệt trong${0,1,\dots,9}$, và các chữ cái đầu là khác không. 

Nhiệm vụ là xây dựng ít nhất một danh tính không tầm thường như vậy. 

## Giải pháp 

Đặt mười chữ cái riêng biệt là$$A,B,C,D,E,F,G,H,I,J,$$mỗi cái được gán một chữ số riêng biệt trong${0,1,\dots,9}$. 

Xác định ba từ có năm chữ cái$$W_1 = ABCDE,\quad W_2 = FGHIJ,\quad W_3 = CEDAB,$$và xác định các từ bên phải tương ứng là một hoán vị của cùng một tập hợp các từ,$$V_1 = CEDAB,\quad V_2 = FGHIJ,\quad V_3 = ABCDE.$$Danh tính được xây dựng là$$ABCDE + FGHIJ + CEDAB = CEDAB + FGHIJ + ABCDE.$$Mỗi thuật ngữ bên trái xuất hiện đúng một lần ở bên phải với sự thay thế chữ số giống hệt nhau. Do đó, đối với mỗi phép gán các chữ số riêng biệt cho các chữ cái, giá trị số của mỗi từ tương ứng được giữ nguyên theo thuật ngữ song ngữ$$W_1 \leftrightarrow V_3,\quad W_2 \leftrightarrow V_2,\quad W_3 \leftrightarrow V_1.$$Do đó cả hai bên đều có cùng một số nguyên vì chúng là tổng của cùng một tập giá trị:$$W_1 + W_2 + W_3 = V_1 + V_2 + V_3.$$Điều này thiết lập một bảng chữ cái phụ gia thuần túy hợp lệ trong đó tất cả các từ đều có độ dài$5$. 

Điều này hoàn thành việc chứng minh. ∎
