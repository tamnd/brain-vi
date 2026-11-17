---
title: "CF 198C - Cung cấp chất gây ung thư"
description: "Chúng ta được yêu cầu tính thời gian tối thiểu để tàu của Qwerty có thể chặn một hành tinh đang chuyển động trong mặt phẳng 2D. Ngôi sao Diatar nằm ở điểm gốc, Persephone quay quanh ngôi sao theo một vòng tròn hoàn hảo có bán kính $R$ với tốc độ tuyến tính không đổi $vp$, và con tàu của Qwerty bắt đầu ở một thời điểm tùy ý…"
date: "2026-06-03T16:20:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry"]
categories: ["algorithms"]
codeforces_contest: 198
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 125 (Div. 1)"
rating: 2400
weight: 198
solve_time_s: 45
verified: false
draft: false
---

[CF 198C - Cung cấp chất gây ung thư](https://codeforces.com/problemset/problem/198/C) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, hình học 
**Thời gian giải:** 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính thời gian tối thiểu để tàu của Qwerty có thể chặn một hành tinh đang chuyển động trong mặt phẳng 2D. Ngôi sao Diatar nằm ở điểm gốc, Persephone quay quanh ngôi sao theo một vòng bán kính hoàn hảo$R$ở tốc độ tuyến tính không đổi$v_p$, và con tàu của Qwerty bắt đầu ở một vị trí tùy ý nào đó$(x, y)$với tốc độ tối đa$v$nó thực sự nhanh hơn Persephone. Con tàu không thể tiếp cận ngôi sao gần hơn bán kính$r < R$. Mục tiêu là xác định xem Qwerty có thể đáp ứng hành tinh nhanh như thế nào với những hạn chế về chuyển động này. 

Đầu vào cung cấp tọa độ ban đầu của hành tinh$(x_p, y_p)$và tốc độ quỹ đạo$v_p$, và tọa độ ban đầu của tàu$(x, y)$, tốc độ tối đa$v$và khoảng cách an toàn tối thiểu$r$. Đầu ra là thời gian giao hàng tối thiểu có thể. 

Những ràng buộc của vấn đề đảm bảo rằng$v > v_p$và cả con tàu và hành tinh đều xuất phát bên ngoài vùng cấm xung quanh ngôi sao. Bởi vì tất cả tọa độ và tốc độ đều lên tới$10^4$, thuật toán phải hoạt động hiệu quả. Một cách tiếp cận đơn giản mô phỏng chuyển động của con tàu theo từng khoảng thời gian nhỏ sẽ yêu cầu kích thước bước nhỏ để đảm bảo độ chính xác trong$10^{-6}$, dẫn đến hàng tỷ lần lặp và vượt quá giới hạn thời gian 2 giây. Vì vậy, cần phải có một cách tiếp cận mang tính phân tích hoặc số học hơn. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó con tàu khởi hành đã thẳng hàng với quỹ đạo của hành tinh, yêu cầu đánh chặn trực tiếp dọc theo đường tiếp tuyến hoặc khi con tàu phải lướt gần bán kính an toàn.$r$để giảm thiểu thời gian. Một giải pháp bất cẩn bỏ qua vòng tròn cấm bên trong sẽ tính ra một đường đi ngắn hơn mức có thể về mặt vật lý. 

## Phương pháp tiếp cận 

Phương pháp vũ lực sẽ cố gắng mô phỏng quỹ đạo của con tàu theo thời gian. Ở mỗi bước thời gian nhỏ, nó sẽ di chuyển con tàu theo hướng vị trí dự đoán của hành tinh, tính toán khoảng cách đến bán kính cấm và điều chỉnh chuyển động. Mặc dù đúng về mặt khái niệm
