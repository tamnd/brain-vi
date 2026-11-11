---
title: "CF 1575H - Đồ trang trí tường ngày lễ"
description: "Chúng ta được cấp một chuỗi nhị phân đại diện cho một bức tường, trong đó mỗi ký tự là 0 hoặc 1. Chuỗi này, a, có độ dài n. Ông Chanek muốn đặt mẫu hình yêu thích của cháu trai mình, một chuỗi nhị phân b khác có chiều dài m, lên tường."
date: "2026-06-10T10:55:50+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "H"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 1575
solve_time_s: 37
verified: false
draft: false
---

[CF 1575H - Đồ trang trí tường ngày lễ](https://codeforces.com/problemset/problem/1575/H) 

**Xếp hạng:** 2200 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân đại diện cho một bức tường, trong đó mỗi ký tự là 0 hoặc 1. Chuỗi này,`a`, có độ dài`n`. Ông Chanek muốn đặt mẫu hình yêu thích của cháu trai mình, một chuỗi nhị phân khác`b`chiều dài`m`, lên tường. Nhiệm vụ là xác định, với mọi số lần xuất hiện có thể`k`từ`0`ĐẾN`n - m + 1`, số lần lật tối thiểu`a`cần thiết để đạt được chính xác`k`sự xuất hiện của`b`như một chuỗi con liền kề. 

Các chuỗi đầu vào đủ nhỏ để việc so khớp chuỗi mạnh mẽ về mặt lý thuyết có thể hoạt động, nhưng chúng tôi cũng phải tính toán số lần lật tối thiểu cho mỗi chuỗi.`k`. Mỗi lần lật thay đổi một ký tự trong`a`và các chuỗi con có thể chồng lên nhau. Những hạn chế`1 ≤ m ≤ n ≤ 500`ngụ ý rằng bất kỳ giải pháp nào tồi tệ hơn`O(n^3)`có thể sẽ quá chậm, vì`500^3`đã có 125 triệu thao tác, có thể đẩy giới hạn thời gian lên nếu các bước bên trong không hề tầm thường. 

Các trường hợp cạnh phát sinh khi`b`có các ký tự lặp đi lặp lại như`111`hoặc`000`, hoặc khi`a`đã chứa các lần xuất hiện chồng chéo của`b`. Ví dụ, nếu`a = 11111`Và`b = 111`, sau đó các lần xuất hiện chồng lên nhau: vị trí 0-2, 1-3, 2-4. Một phương pháp đếm ngây thơ chỉ xem xét các kết quả không trùng lặp sẽ tạo ra kết quả không chính xác. Một trường hợp cạnh khác là khi`k`là không thể do hạn chế về độ dài chuỗi hoặc cấu trúc chồng chéo, phải trả về`-1`. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ xem xét mọi tập hợp con các vị trí trong`a`để lật và đếm số lần xuất hiện của`b`sau đó. Cách tiếp cận này đúng về nguyên tắc vì nó khám phá mọi cách có thể để tiếp cận từng`k`. Tuy nhiên, với tối đa 500 vị trí, số lượng tập hợp con tăng theo cấp số nhân (`2^500`), vì vậy vũ phu là hoàn toàn không thực tế. 

Thông tin chi tiết quan trọng là các lần xuất hiện chồng chéo và lật ký tự gợi ý lập trình động. Chúng ta có thể mô hình hóa vấn đề như iterati
