---
title: "CF 1775D - Loài nhện thân thiện"
description: "Chúng ta được cấp cho một đàn nhện trên sao Hỏa, mỗi đàn được xác định bằng số lượng chân mà nó có. Hai con nhện được coi là bạn bè nếu ước số chung lớn nhất của số chân của chúng lớn hơn một."
date: "2026-06-09T11:53:38+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "math", "number-theory", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1775
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 843 (Div. 2)"
rating: 1800
weight: 1775
solve_time_s: 27
verified: false
draft: false
---

[CF 1775D - Những con nhện thân thiện](https://codeforces.com/problemset/problem/1775/D) 

**Đánh giá:** 1800 
**Tags:** dfs và tương tự, đồ thị, toán học, lý thuyết số, đường đi ngắn nhất 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp cho một đàn nhện trên sao Hỏa, mỗi đàn được xác định bằng số lượng chân mà nó có. Hai con nhện được coi là bạn bè nếu ước số chung lớn nhất của số chân của chúng lớn hơn một. Tình bạn cho phép gửi tin nhắn ngay lập tức; nếu không, tin nhắn phải chuyển từ con nhện này sang con nhện khác dọc theo một chuỗi bạn bè. Nhiệm vụ là tính toán đường đi ngắn nhất về mặt truyền thông điệp từ một con nhện cụ thể này đến một con nhện cụ thể khác, đồng thời cung cấp một chuỗi các con nhện hợp lệ dọc theo con đường đó. 

Đầu vào cung cấp tới 300.000 con nhện, mỗi con có số lượng chân lên tới 300.000. Các nhện nguồn và mục tiêu được xác định bằng các chỉ số dựa trên 1. Với các kích thước đầu vào này và giới hạn 2 giây, chúng tôi không thể cung cấp một thuật toán kiểm tra trực tiếp tất cả tình bạn theo cặp vì điều đó sẽ yêu cầu tính toán lên tới 9·10^10 gcd, điều này là không khả thi. Do đó, lời giải phải khai thác cấu trúc lý thuyết số thay vì kiểm tra từng cặp một cách đơn giản. 

Một trường hợp khó thấy rõ ràng phát sinh khi số chân của con nhện là số nguyên tố và không có thừa số chung nào với bất kỳ con nhện nào khác.
