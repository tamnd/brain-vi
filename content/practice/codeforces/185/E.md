---
title: "CF 185E - Thời gian xà phòng! - 2"
description: "Chúng ta được cung cấp một tập hợp người lùn nằm ở tọa độ nguyên trên mặt phẳng Descartes và tùy chọn một tập hợp các ga tàu điện ngầm cũng ở tọa độ nguyên."
date: "2026-06-03T00:57:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 185
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 118 (Div. 1)"
rating: 3000
weight: 185
solve_time_s: 23
verified: false
draft: false
---

[CF 185E - Thời gian xà phòng! - 2](https://codeforces.com/problemset/problem/185/E) 

**Đánh giá:** 3000 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp người lùn nằm ở tọa độ nguyên trên mặt phẳng Descartes và tùy chọn một tập hợp các ga tàu điện ngầm cũng ở tọa độ nguyên. Mỗi người lùn có thể di chuyển một đơn vị theo bốn hướng chính mỗi giây, nhưng họ cũng có thể dịch chuyển tức thời giữa bất kỳ ga tàu điện ngầm nào. Những người lùn muốn tập hợp tại một tọa độ nguyên duy nhất trong mặt phẳng và mục tiêu là tính toán số giây tối thiểu cần thiết để tất cả người lùn đạt đến điểm tập trung đó nếu họ di chuyển một cách tối ưu. 

Đầu vào chỉ định số lượng người lùn, số lượng ga tàu điện ngầm và tọa độ của tất cả người lùn và nhà ga. Đầu ra là một số nguyên duy nhất biểu thị thời gian thu thập tối thiểu. 

Cho rằng số lượng người lùn và ga tàu điện ngầm có thể lên tới 100.000 và phạm vi tọa độ mở rộng lên tới 10^8 theo mỗi hướng, chúng tôi không thể xem xét rõ ràng tất cả các điểm tập trung có thể có. Việc tìm kiếm mạnh mẽ trên mọi điểm nguyên là không khả thi vì lưới quá lớn. Chúng ta cần một cách tiếp cận tận dụng các đặc tính về khoảng cách và hệ thống tàu điện ngầm để giảm không gian tìm kiếm một cách hiệu quả. 

Các trường hợp cạnh bao gồm sự vắng mặt của các ga tàu điện ngầm, một người lùn hoặc nhiều người lùn đều bắt đầu tại cùng một điểm. Trong trường hợp đơn giản nhất, nếu không có trạm nào và tất cả người lùn đều ở cùng nhau thì câu trả lời là không. Một cách tiếp cận ngây thơ mà bỏ qua các ga tàu điện ngầm hoặc không tính toán khoảng cách một cách chính xác sẽ đưa ra câu trả lời sai cho những trường hợp nhỏ hoặc suy biến này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên mọi tọa độ nguyên trong hộp giới hạn của người lùn và nhà ga, tính toán thời gian tối đa mà bất kỳ người lùn nào cần để đến điểm đó (đi bộ trực tiếp hoặc sử dụng tàu điện ngầm một cách tối ưu) và chọn thời gian tối thiểu. Cách tiếp cận này đúng nhưng không thể tính toán được. Ngay cả việc giới hạn trong phạm vi bao lồi của người lùn vẫn là quá chậm vì khoảng cách có thể lên tới 2×10^8 và các người lùn có thể ở rất xa nhau. 

Quan sát quan trọng là khoảng cách Manhattan có thể được phân tách dọc theo trục x và y. Điều này làm giảm vấn đề tính toán khoảng cách một chiều dọc theo mỗi trục một cách độc lập. Mạng lưới tàu điện ngầm cho phép người lùn “dịch chuyển” đến một trong các ga một cách hiệu quả, vì vậy đối với mỗi người lùn, thời gian tối thiểu đến điểm tập trung là khoảng cách tối thiểu giữa khoảng cách trực tiếp từ Manhattan đến điểm đó và khoảng cách đến ga tàu điện ngầm gần nhất cộng với khoảng cách.
