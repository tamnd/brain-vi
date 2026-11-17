---
title: "CF 200A - Rạp chiếu phim"
description: "Chúng ta có một rạp hát có n hàng và m ghế mỗi hàng, tạo thành một lưới n × m. Một hàng k người đang xếp hàng mua vé. Mỗi người có một chỗ ngồi ưa thích, được biểu thị bằng tọa độ (x, y). Khi một người đến phòng vé, họ sẽ cố gắng ngồi vào chỗ ngồi ưa thích của mình."
date: "2026-06-03T16:29:32+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 200
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 126 (Div. 2)"
rating: 2400
weight: 200
solve_time_s: 37
verified: false
draft: false
---

[CF 200A - Rạp chiếu phim](https://codeforces.com/problemset/problem/200/A) 

**Đánh giá:** 2400 
**Tags:** bạo lực, cấu trúc dữ liệu 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một nhà hát với`n`hàng và`m`số ghế mỗi hàng, tạo thành một`n × m`lưới. Một dòng`k`mọi người đang chờ mua vé. Mỗi người có một chỗ ngồi ưa thích, được biểu thị bằng tọa độ`(x, y)`. Khi một người đến phòng vé, họ sẽ cố gắng ngồi vào chỗ ngồi ưa thích của mình. Nếu chỗ đó đã có người ngồi, họ phải chọn một chỗ trống thay thế gần nhất với khoảng cách ở Manhattan so với lựa chọn ban đầu của họ. Nếu nhiều ghế trống có cùng khoảng cách tối thiểu thì người đó chọn ghế có số hàng nhỏ nhất, còn nếu vẫn bằng nhau thì chọn số cột nhỏ nhất. Mục tiêu của chúng tôi là chỉ định cho mỗi người một chỗ ngồi thực tế theo quy trình này. 

Những hạn chế ngụ ý một khu vực chỗ ngồi khá lớn lên đến`2000 × 2000 = 4,000,000`chỗ ngồi, trong khi số lượng người được giới hạn ở`105`. Một thuật toán đơn giản quét tất cả chỗ ngồi cho mỗi người có thể`O(k * n * m)`hoạt động, có khả năng vượt quá 10^11 hoạt động, quá chậm trong giới hạn thời gian 2 giây. Quan sát này loại trừ việc tìm kiếm cưỡng bức trên toàn bộ lưới chỗ ngồi. 

Các trường hợp đặc biệt cần xem xét bao gồm nhiều người chọn cùng một chỗ ngồi ở đầu hàng, những người có ghế trống gần nhất ở xa và các tình huống
