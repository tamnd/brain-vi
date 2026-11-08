---
title: "CF 1451A - Trừ hoặc chia"
description: "ZeroDivisionError được báo cáo là một triệu chứng, không phải nguyên nhân gốc rễ. Hãy theo dõi dữ liệu đầu vào bị lỗi so với mã đã được đề xuất trước đó: Mã diễn giải: - t = 5 - Kiểm tra 1: a = [2, 4], do đó n = 2 - Kiểm tra 2: a = [3, 1], do đó n = 2 - Kiểm tra 3: a = [3, 4], do đó n = 2 - Kiểm tra 4: a =…"
date: "2026-06-11T03:31:04+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1451
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 685 (Div. 2)"
rating: 800
weight: 1451
solve_time_s: 148
verified: false
draft: false
---

[CF 1451A - Trừ hoặc Chia](https://codeforces.com/problemset/problem/1451/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
Việc báo cáo`ZeroDivisionError`là một triệu chứng, không phải là nguyên nhân gốc rễ. 

Hãy theo dõi đầu vào bị lỗi so với mã đã được đề xuất trước đó:```
5
2 4
3 1
3 4
3 5
2
4 5
```Mã giải thích: 

-`t = 5`- Bài kiểm tra 1:`a = [2, 4]`, Vì thế`n = 2`- Bài kiểm tra 2:`a = [3, 1]`, Vì thế`n = 2`- Bài kiểm tra 3:`a = [3, 4]`, Vì thế`n = 2`- Bài kiểm tra 4:`a = [3, 5]`, Vì thế`n = 2`- Bài kiểm tra 5:`a = [2]`, Vì thế`n = 1`Bây giờ mã đạt:```
target % (n - 1)
```Đối với trường hợp thử nghiệm cuối cùng,`n = 1`, Vì vậy:```
n - 1 = 0
```và Python tăng:```
ZeroDivisionError: integer modulo by zero
```Tuy nhiên, điều này lại bộc lộ một vấn đề sâu xa hơn: toàn bộ thuật toán đang giải quyết một vấn đề hoàn toàn khác, đó là Codeforces 1452B (Toy Blocks). Đầu vào mẫu```
5
2 4
3 1
3 4
3 5
2
4 5
```và sản lượng dự kiến```
2 1 2 0 0
```rõ ràng không hề thuộc về Toy Blocks. 

Ví dụ: nếu chúng ta diễn giải trường hợp thử nghiệm đầu tiên là một phiên bản Khối đồ chơi có các hộp`[2,4]`, câu trả lời đúng về Khối đồ chơi sẽ là`0`, không`2`. 

Vì vậy, có hai vấn đề riêng biệt: 

1. Sự cố ngay lập tức xuất phát từ việc giả định không chính xác mọi trường hợp thử nghiệm đều chứa ít nhất hai số. 
2. Quan trọng hơn, bản thân thuật toán đã dành cho bài toán sai. Kết quả đầu ra dự kiến ​​không khớp với Codeforces 1452B, vì vậy không một sửa chữa nhỏ nào có thể giúp giải pháp này chính xác. 

Vì đầu vào mẫu không bao gồm câu lệnh bài toán thực tế nên không thể xây dựng lại thuật toán dự kiến ​​chỉ từ cặp đầu vào/đầu ra. Nhiều vấn đề hoàn toàn khác nhau có thể tạo ra những kết quả đầu ra đó. 

Vui lòng cung cấp báo cáo vấn đề ban đầu (hoặc ít nhất là tên vấn đề/số Codeforces tương ứng với mẫu này). Khi tôi gặp vấn đề thực tế, tôi có thể theo dõi thuật toán một cách chính xác và đưa ra giải pháp hoàn chỉnh đã được sửa chữa.
