---
title: "CF 1725F - Chụp Ảnh Hiện Trường"
description: "Mỗi hàng ban đầu chứa một khối thí sinh liền kề được đặt trên một dòng cột số nguyên cực lớn. Hàng $i$ chiếm mọi vị trí từ $Li$ đến $Ri$, do đó về mặt hình học mỗi hàng chỉ là một khoảng đóng."
date: "2026-06-16T16:52:35+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1725
codeforces_index: "F"
codeforces_contest_name: "COMPFEST 14 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1725
solve_time_s: 175
verified: false
draft: false
---

[CF 1725F - Chụp ảnh hiện trường](https://codeforces.com/problemset/problem/1725/F) 

**Đánh giá:** 2100 
**Thẻ:** bitmask, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi hàng ban đầu chứa một khối thí sinh liền kề được đặt trên một dòng cột số nguyên cực lớn. Hàng ngang$i$chiếm giữ mọi vị trí từ$L_i$ĐẾN$R_i$, do đó về mặt hình học mỗi hàng chỉ là một khoảng đóng. 

Chúng ta được phép dịch chuyển toàn bộ một hàng sang trái hoặc sang phải bằng bất kỳ số nguyên dương nào$k$. Mỗi động thái như vậy sẽ bổ sung thêm$k$vào bộ tích lũy toàn cục bằng cách sử dụng bitwise OR. Đối với một giá trị truy vấn$W$, chúng ta phải chọn một seq
