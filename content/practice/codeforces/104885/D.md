---
title: "CF 104885D - \u0412\u0440\u0435\u043c\u044f \u043d\u0430 \u043c\u0430\u0440\u0441\u0435"
description: "Chúng ta có một khoảng thời gian trên đồng hồ, được viết bằng giờ và phút, từ thời điểm bắt đầu H1:M1 đến thời điểm kết thúc H2:M2."
date: "2026-06-28T09:08:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104885
codeforces_index: "D"
codeforces_contest_name: "Municipal stage of ROI in Nizhny Novgorod 2023"
rating: 0
weight: 104885
solve_time_s: 23
verified: false
draft: false
---

[CF 104885D - \u0412\u0440\u0435\u043c\u044f \u043d\u0430 \u043c\u0430\u0440\u0441\u0435](https://codeforces.com/problemset/problem/104885/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khoảng thời gian trên đồng hồ, được viết bằng giờ và phút, kể từ thời điểm bắt đầu`H1:M1`đến giây phút kết thúc`H2:M2`. Chúng tôi tiến về phía trước theo thời gian từng phút và trong mọi thời gian trung gian`h:m`chúng tôi xây dựng một chuỗi bằng cách nối biểu diễn thập phân của`h`Và`m`không có dải phân cách. Ví dụ,`7:05`trở thành chuỗi`"705"`, trong khi`12:30`trở thành`"1230"`. 

Đối với mỗi chuỗi như vậy, một quy tắc cố định từ câu lệnh sẽ chỉ định "chi phí hiển thị", tương ứng với số lượng phần tử hiển thị (ví dụ như các phân đoạn trên màn hình kỹ thuật số) cần thiết để hiển thị tất cả các chữ số của chuỗi đó. Nhiệm vụ là tính toán chi phí hiển thị tối đa trong tất cả các phút trong khoảng thời gian nhất định. 

Do đó, đầu vào chính không phải là biểu đồ hay mảng mà là một chuỗi thời gian liên tục. Đầu ra là một số nguyên duy nhất: yêu cầu hiển thị trong trường hợp xấu nhất trong khoảng thời gian đó. 

Ngay cả khi không có những ràng buộc nặng nề
