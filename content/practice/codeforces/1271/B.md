---
title: "CF 1271B - Khối"
description: "Chúng ta được cho một dãy các khối, mỗi khối được sơn màu đen hoặc trắng. Động thái duy nhất được phép là chọn hai khối lân cận và lật cả hai khối cùng lúc, biến trắng thành đen và đen thành trắng."
date: "2026-06-16T01:14:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1271
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 608 (Div. 2)"
rating: 1300
weight: 1271
solve_time_s: 252
verified: false
draft: false
---

[CF 1271B - Khối](https://codeforces.com/problemset/problem/1271/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các khối, mỗi khối được sơn màu đen hoặc trắng. Động thái duy nhất được phép là chọn hai khối lân cận và lật cả hai khối cùng lúc, biến trắng thành đen và đen thành trắng. 

Nhiệm vụ không phải là tối ưu hóa số lần di chuyển mà là quyết định xem liệu chúng ta có thể làm cho toàn bộ hàng đồng nhất hay không, tất cả các khối có cùng màu và nếu có, xuất ra một chuỗi thao tác hợp lệ với độ dài tối đa gấp ba lần số khối. 

Mỗi thao tác chỉ ảnh hưởng đến hai vị trí liền kề, vì vậy mọi chiến lược đều phải truyền bá các thay đổi cục bộ. Khó khăn chính là việc lật một cặp sẽ làm thay đổi cấu trúc giống như tính chẵn lẻ theo cách bị hạn chế, do đó không phải mọi cấu hình đều có thể sửa được. 

Ràng buộc n 200 có nghĩa là chúng ta có thể đủ khả năng xây dựng các phép toán O(n^2) hoặc thậm chí O(n^3). Chúng ta không bị buộc phải suy nghĩ chỉ vì tham lam về tốc độ mà còn phải giữ đầu ra được giới hạn bởi O(n). 

Trường hợp cạnh phím xuất hiện khi số lượng khối đen và trắng đều tương tác theo cách ngăn cản sự liên kết toàn cục. Ví dụ: nếu tất cả các khối giống hệt nhau thì không cần di chuyển. Nếu chuỗi xen kẽ như "BWBW", các cú lật cục bộ cưỡng bức có thể dễ dàng quay vòng mà không tiến triển trừ khi chúng tôi thực thi một hướng có hệ thống. 

Một trường hợp khó phát hiện khác là khi một số điểm không khớp cuối cùng vẫn bị cô lập. Vì các thao tác luôn ảnh hưởng đến các cặp nên việc để lại một vị trí “chẵn lẻ sai” ở cuối không thể sửa chữa được trừ khi cấu trúc trước đó được xử lý chính xác. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ cố gắng mô phỏng chuỗi các lần lật liền kề và tìm kiếm một phép biến đổi mang lại tất cả các màu bằng nhau. Vì mỗi thao tác sửa đổi hai vị trí, mỗi trạng thái có tối đa n-1 bước di chuyển và độ sâu lên tới 3n, không gian tìm kiếm đơn giản tăng theo cấp số nhân và không khả thi ngay cả với n = 200. 

Quan sát quan trọng là chúng ta
