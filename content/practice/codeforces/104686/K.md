---
title: "CF 104686K - Kỹ năng dùng thuốc"
description: "Chúng tôi đang xây dựng một lịch trình trong n ngày. Mỗi ngày, chúng ta có thể uống hoặc không uống hai viên thuốc khác nhau, nhưng với một quy định nghiêm ngặt là không bao giờ được uống cả hai viên thuốc trong cùng một ngày."
date: "2026-06-29T08:51:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104686
codeforces_index: "K"
codeforces_contest_name: "2022-2023 ICPC Central Europe Regional Contest (CERC 22)"
rating: 0
weight: 104686
solve_time_s: 20
verified: false
draft: false
---

[CF 104686K - Kỹ năng sử dụng thuốc](https://codeforces.com/problemset/problem/104686/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một lịch trình trên một dòng`n`ngày. Mỗi ngày, chúng ta có thể uống hoặc không uống hai viên thuốc khác nhau, nhưng với một quy định nghiêm ngặt là không bao giờ được uống cả hai viên thuốc trong cùng một ngày. 

Viên A có quy luật tái phát: trong bất kỳ giai đoạn nào của`k`ngày liên tiếp thì ít nhất một ngày phải dùng viên A. Tương tự, không có khoảng cách giữa`k`ngày liên tục không có A. Viên B có cùng loại yêu cầu về thông số`j`. Mục tiêu là xây dựng một lịch trình hợp lệ`n`ngày tôn trọng cả những hạn chế về khoảng cách đồng thời tôn trọng quy tắc loại trừ lẫn nhau và chúng tôi muốn giảm thiểu tổng số lần uống thuốc. 

Đầu ra chỉ đơn giản là tổng số viên thuốc đã uống tối thiểu trong tất cả các ngày, tính riêng A và B. 

Hạn chế chính là`n ≤ 10^6`, điều này ngay lập tức loại trừ mọi tìm kiếm hàm mũ hoặc bậc hai theo lịch trình. Thậm chí một`O(n log n)`xây dựng là ranh giới nhưng vẫn khả thi. Bất kỳ giải pháp đúng nào về cơ bản đều phải suy luận về mật độ hoặc cấu trúc tuần hoàn thay vì liệt kê rõ ràng tất cả các lịch trình hợp lệ. 

Một ý tưởng ngây thơ là mô phỏng tất cả các vị trí có thể có của A và B, nhưng ngay cả đối với những vị trí nhỏ.`n`, số lượng cấu hình hợp lệ tăng lên theo kiểu tổ hợp. Một sai lầm hấp dẫn khác là tham lam đặt A vào mỗi vị trí.`k`ngày và B mỗi`j`ngày độc lập; điều này không thành công vì những ngày chồng chéo bị cấm, vì vậy việc xây dựng độc lập có thể tạo ra va chạm hoặc buộc phải bổ sung thêm những viên thuốc không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi`k`Và`j`gần gũi chẳng hạn`k = j = 2`. Một lịch trình xen kẽ ngây thơ nhanh chóng bị phá vỡ vì cả hai viên thuốc đều cố gắng sử dụng cách ngày, nhưng chúng không thể trùng khớp, buộc phải đặt thêm các vị trí khác. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là mỗi viên thuốc đều áp đặt mật độ sử dụng tối thiểu một cách độc lập, nhưng chúng cạnh tranh nhau.
