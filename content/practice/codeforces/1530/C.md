---
title: "CF 1530C - Truy Đuổi"
description: "Chúng tôi có một cuộc thi có nhiều giai đoạn, mỗi giai đoạn cho từ 0 đến 100 điểm. Bạn và Ilya đã hoàn thành n màn và chúng tôi biết điểm số của cả hai bạn."
date: "2026-06-10T16:52:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1530
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 733 (Div. 1 + Div. 2, based on VK Cup 2021 - Elimination (Engine))"
rating: 1200
weight: 1530
solve_time_s: 45
verified: false
draft: false
---

[CF 1530C - Truy đuổi](https://codeforces.com/problemset/problem/1530/C) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, sắp xếp 
**Thời gian giải:** 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cuộc thi có nhiều giai đoạn, mỗi giai đoạn cho từ 0 đến 100 điểm. Bạn và Ilya đã hoàn thành`n`giai đoạn, và chúng tôi biết điểm số của cả hai bạn. Cuộc thi xếp hạng thí sinh bằng cách chỉ lấy điểm cao nhất`k - floor(k / 4)`ghi điểm ngay từ đầu`k`giai đoạn, tổng hợp chúng để có được điểm tổng thể. 

Vấn đề đặt ra là: với số điểm hiện tại của bạn và điểm của Ilya, số giai đoạn ghi điểm hoàn hảo bổ sung nhỏ nhất (100 điểm) về mặt lý thuyết là bao nhiêu để bạn đạt được tổng điểm ít nhất bằng tổng điểm của Ilya, giả sử Ilya không đạt được điểm nào trong các giai đoạn bổ sung này? 

Đầu vào đảm bảo rằng tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`. Từ`n`có thể đạt được`10^5`, bất kỳ thuật toán nào thử mọi khả năng cho các giai đoạn bổ sung một cách ngây thơ sẽ quá chậm. Chúng tôi cần thứ gì đó nhanh hơn `O(n
