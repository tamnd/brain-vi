---
title: "CF 1344A - Khách sạn Hilbert"
description: "Chúng ta đang xử lý một khách sạn vô tận, trong đó mỗi phòng, được đánh dấu bằng một số nguyên, có chính xác một khách. Quy tắc xáo trộn được áp dụng khi mỗi khách di chuyển từ phòng hiện tại $k$ sang phòng mới $k + a{k bmod n}$, trong đó $a$ là một mảng có độ dài $n$."
date: "2026-06-11T15:07:52+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1344
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 639 (Div. 1)"
rating: 1600
weight: 1344
solve_time_s: 320
verified: false
draft: false
---

[CF 1344A - Khách sạn Hilbert](https://codeforces.com/problemset/problem/1344/A) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số, sắp xếp 
**Thời gian giải:** 5 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một khách sạn vô tận, trong đó mỗi phòng, được đánh dấu bằng một số nguyên, có chính xác một khách. Quy tắc xáo trộn được áp dụng khi mỗi khách di chuyển khỏi phòng hiện tại của họ$k$đến một căn phòng mới$k + a_{k \bmod n}$, Ở đâu$a$là một mảng có độ dài$n$. Nhiệm vụ là xác định xem việc xáo trộn này có tạo ra một vị khách duy nhất trong mỗi phòng hay không hay có xảy ra xung đột hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định$n$và mảng$a$. Đầu ra là "CÓ" nếu việc xáo trộn duy trì tính duy nhất và "KHÔNG" nếu ngược lại. Với$n$lên tới$2 \cdot 10^5$Và$t$lên tới$10^4$, tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm được giới hạn ở (2 \cdot 10^5\
