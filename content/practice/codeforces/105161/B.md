---
title: "CF 105161B - Khu vực của quỷ"
description: "Chúng ta có năm cung rời nhau trên một vòng tròn. Từ mỗi cung, chúng ta chọn một điểm và nối năm điểm đã chọn theo thứ tự, tạo thành một hình ngôi sao giống như ngũ giác khép kín."
date: "2026-06-27T10:56:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105161
codeforces_index: "B"
codeforces_contest_name: "2024 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 105161
solve_time_s: 30
verified: false
draft: false
---

[CF 105161B - Khu vực của quỷ](https://codeforces.com/problemset/problem/105161/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có năm cung rời nhau trên một vòng tròn. Từ mỗi cung, chúng ta chọn một điểm và nối năm điểm đã chọn theo thứ tự, tạo thành một hình ngôi sao giống như ngũ giác khép kín. Nhiệm vụ là tính diện tích của đa giác giống như ngôi sao năm cánh được hình thành bởi năm điểm này trên đường tròn. 

Một cách hữu ích để giải thích việc xây dựng là quên đi khía cạnh “sự lựa chọn” và thay vào đó hãy nghĩ về mặt hình học về vùng bị quét khi mỗi điểm có thể thay đổi dọc theo cung của nó. Khi điểm di chuyển dọc theo một cung, diện tích đa giác thu được sẽ thay đổi liên tục và giá trị yêu cầu cuối cùng tương ứng với một phân rã hình học cố định chỉ phụ thuộc vào các điểm cuối của cung. 

Đầu ra là một số thực duy nhất đại diện cho khu vực này. 

Cấu trúc cơ bản mang tính hình học mạnh mẽ: mọi thứ nằm trên một vòng tròn có bán kính r (được cho trước hoặc suy ra từ tọa độ hoặc góc), do đó tất cả hình học có liên quan đều quy về các cung tròn, dây cung và hình tam giác được hình thành bởi giao điểm của dây cung. 

Các ràng buộc không được hiển thị rõ ràng ở đây, nhưng từ cấu trúc biên tập và O(T) cho mỗi giải pháp thử nghiệm, chúng ta có thể suy ra rằng có nhiều trường hợp thử nghiệm và mỗi trường hợp phải được giải trong thời gian không đổi sau khi xử lý trước hình dạng cung. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào làm rời rạc hóa các cung hoặc mô phỏng việc lựa chọn điểm. Bất kỳ phương pháp nào liên quan đến việc quét qua các điểm hoặc lấy mẫu đều sẽ quá chậm. 

Một cách tiếp cận hình học đơn giản có thể cố gắng xem xét tất cả các lựa chọn có thể có của một điểm trên mỗi cung và tính toán các cấu hình cực trị, nhưng đây là cách tiếp cận tổ hợp liên tục và không thể rời rạc. Khó khăn chính là lời giải chỉ phụ thuộc vào điểm cuối của cung chứ không phụ thuộc vào điểm bên trong. 

Trường hợp cạnh tinh tế phát sinh khi các cung bao quanh điểm góc 0. Ví dụ: một cung có thể trải dài từ 350° đến 10°, vượt qua sự gián đoạn góc một cách trực quan. TÔI
