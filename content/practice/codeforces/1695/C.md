---
title: "CF 1695C - Đường dẫn 0"
description: "Chúng tôi có một lưới có các ô chỉ chứa 1 hoặc -1. Bắt đầu từ góc trên bên trái, chúng ta chỉ có thể di chuyển sang phải hoặc xuống dưới cho đến khi đến góc dưới bên phải. Mỗi ô được truy cập đều đóng góp giá trị của nó vào tổng đường dẫn. Nhiệm vụ không phải là tìm số đường đi như vậy hay tổng nhỏ nhất."
date: "2026-06-09T22:43:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1695
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 801 (Div. 2) and EPIC Institute of Technology Round"
rating: 1700
weight: 1695
solve_time_s: 181
verified: false
draft: false
---

[CF 1695C - Đường dẫn 0](https://codeforces.com/problemset/problem/1695/C) 

**Đánh giá:** 1700 
**Tags:** sức mạnh vũ phu, cấu trúc dữ liệu, dp, đồ thị, tham lam, đường dẫn ngắn nhất 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới có các ô chỉ chứa`1`hoặc`-1`. Bắt đầu từ góc trên bên trái, chúng ta chỉ có thể di chuyển sang phải hoặc xuống dưới cho đến khi đến góc dưới bên phải. Mỗi ô được truy cập đều đóng góp giá trị của nó vào tổng đường dẫn. 

Nhiệm vụ không phải là tìm số đường đi như vậy hay tổng nhỏ nhất. Chúng ta chỉ cần xác định xem ít nhất một đường dẫn hợp lệ có tổng chính xác hay không`0`. 

Một con đường từ`(1,1)`ĐẾN`(n,m)`luôn truy cập cùng một số lượng ô. Chúng tôi thực hiện chính xác`n-1`chuyển động đi xuống và`m-1`di chuyển sang phải, do đó đường dẫn chứa$$L=n+m-1$$tế bào. 

Tổng kích thước của tất cả các lưới tối đa là$10^6$tế bào. Điều này ngay lập tức loại trừ bất kỳ thuật toán liệt kê đường dẫn nào. Thậm chí một$20\times20$lưới đã chứa$$\binom{38}{19}\approx 3.5\times10^{10}$$những con đường khác nhau. Chúng ta cần một giải pháp có công tỷ lệ thuận với số lượng ô. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một ô duy nhất:```
1 1
1
```Con đường duy nhất có tổng$1$, vậy câu trả lời là`NO`. 

Một trường hợp tinh vi hơn là khi độ dài đường dẫn là số lẻ:```
1 2
1 -1
```Tổng đường dẫn là$0$, vậy câu trả lời là`YES`. 

Nhưng:```
1 3
1 -1 1
```Mỗi đường dẫn đều có ba
