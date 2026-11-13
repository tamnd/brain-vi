---
title: "CF 1716B - Chuỗi hoán vị"
description: "Chúng ta bắt đầu từ hoán vị danh tính [1, 2, ..., n]. Ở mỗi bước, chúng ta được phép hoán đổi hai vị trí bất kỳ, tạo ra một hoán vị mới. Số lượng điểm cố định, nghĩa là các vị trí có giá trị bằng chỉ số vị trí, phải giảm nghiêm ngặt sau mỗi lần hoán đổi."
date: "2026-06-09T19:49:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1716
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 133 (Rated for Div. 2)"
rating: 800
weight: 1716
solve_time_s: 39
verified: false
draft: false
---

[CF 1716B - Chuỗi hoán vị](https://codeforces.com/problemset/problem/1716/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu từ hoán vị danh tính`[1, 2, ..., n]`. Ở mỗi bước, chúng ta được phép hoán đổi hai vị trí bất kỳ, tạo ra một hoán vị mới. Số lượng điểm cố định, nghĩa là các vị trí có giá trị bằng chỉ số vị trí, phải giảm nghiêm ngặt sau mỗi lần hoán đổi. 

Nhiệm vụ không chỉ đơn thuần là tạo ra một chuỗi hợp lệ. Chúng ta phải làm cho sợi dây xích càng dài càng tốt. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra số lượng hoán vị trong chuỗi và sau đó in mọi hoán vị theo thứ tự. 

Những hạn chế là rất nhỏ. Có nhiều nhất 99 ca kiểm thử và`n ≤ 100`, vì vậy ngay cả một`O(n²)`xây dựng cho mỗi trường hợp thử nghiệm là đủ nhanh. Bộ nhớ cũng không đáng kể vì mỗi hoán vị chỉ chứa 100 phần tử. 

Một điểm tinh tế là một lần hoán đổi có thể phá hủy một số điểm cố định cùng một lúc. Bắt đầu từ hoán vị danh tính với`n`điểm cố định, chúng ta muốn giảm số lượng điểm cố định càng chậm càng tốt, vì mỗi lần giảm sẽ mang lại cho chúng ta một hoán vị khác trong chuỗi. 

Coi như`n = 2`.```
1 2
```Sự hoán đổi duy nhất có thể mang lại```
2 1
```Số lượng điểm cố định thay đổi từ 2 thành 0, do đó độ dài chuỗi là 2. Cách tiếp cận bất cẩn có thể mong đợi một bước cho mỗi điểm cố định và đưa ra ba hoán vị không chính xác. 

Một trường hợp thú vị khác là`n = 3`.```
1 2 3
```Hoán đổi vị trí 1 và 2 tạo ra```
2 1 3
```có độ cố định là 1. Bất kỳ sự hoán đổi nào nữa liên quan đến vị trí 1 và 2 sẽ trả một trong số chúng về vị trí ban đầu và tăng tính cố định. Lon
