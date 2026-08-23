---
title: "CF 104274A - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0430\u0440\u0435\u043d\u0434\u0430"
description: "Rudolf dành tới một triệu ngày trên một hành tinh xa lạ và toàn bộ dòng thời gian được coi là một lịch liên tục duy nhất bắt đầu từ ngày đầu tiên. Sự phức tạp chính là hai lịch trình độc lập chồng chéo lên nhau. Lịch trình đầu tiên là tiền thuê hàng tháng."
date: "2026-07-01T21:17:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104274
codeforces_index: "A"
codeforces_contest_name: "2023 VIII \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e"
rating: 0
weight: 104274
solve_time_s: 33
verified: false
draft: false
---

[CF 104274A - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0430\u0440\u0435\u043d\u0434\u0430](https://codeforces.com/problemset/problem/104274/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Rudolf dành tới một triệu ngày trên một hành tinh xa lạ và toàn bộ dòng thời gian được coi là một lịch liên tục duy nhất bắt đầu từ ngày đầu tiên. Sự phức tạp chính là hai lịch trình độc lập chồng chéo lên nhau. 

Lịch trình đầu tiên là tiền thuê hàng tháng. Thời gian được chia thành các khối có độ dài lặp lại$M$ngày. Trong mỗi tháng, chỉ được phép thanh toán trong thời gian cuối cùng$H$những ngày của tháng đó. Vì các tháng lặp lại theo định kỳ nên tập hợp các ngày thanh toán hợp lệ là mỗi ngày có vị trí trong tháng nằm trong khoảng từ$M - H + 1$ĐẾN$M$. Tuy nhiên, việc thanh toán chỉ có thể thực hiện được nếu Rudolf có mặt trên hành tinh vào ngày hôm đó. 

Lịch trình thứ hai là sự có mặt của Rudolf. Anh ta liên tục đến theo chu kỳ: anh ta ở lại$A$ngày liên tục, sau đó vắng mặt trong$B-A$ngày và mô hình lặp lại. Lần lưu trú đầu tiên bắt đầu vào ngày$C$. Ngoài những khoảng thời gian này, anh ta không thể thực hiện bất kỳ hành động nào trên hành tinh. 

Cuối cùng, có một bộ$N$những ngày bị cấm khi Rudolf bận và không thể trả tiền ngay cả khi cả hai điều kiện trên cho phép. Nếu một tháng trôi qua mà không có ngày thanh toán hợp lệ nào xảy ra trong khi anh ta có mặt và sẵn sàng, Rudolf sẽ bị đuổi khỏi nhà vào ngày đầu tiên của tháng tiếp theo. 

Nhiệm vụ là xác định chính xác ngày bị trục xuất hoặc xác nhận rằng anh ta sống sót sau tất cả.$10^6$ngày. 

Các ràng buộc ngụ ý rằng dòng thời gian có giá trị tuyệt đối lớn nhưng tất cả các thành phần có cấu trúc đều nhỏ. Số ngày bận rộn lên tới$10^5$, cho phép quét tuyến tính hoặc bỏ qua dựa trên tìm kiếm nhị phân nhưng làm cho bất kỳ mô phỏng mỗi ngày nào có khả năng vượt quá ranh giới nếu được thực hiện với các hoạt động nặng. Cấu trúc định kỳ của cả hệ thống tháng và lịch hiện diện gợi ý rằng một giải pháp hiệu quả nên tránh lặp lại từng ngày trên toàn bộ phạm vi mà không có bước nhảy. 

Một vấn đề tế nhị xuất hiện xung quanh ranh giới của các tháng. Thanh toán được gắn với lần cuối cùng$H$ngày, do đó, một cách tiếp cận ngây thơ chỉ kiểm tra “cuối tháng” mà không xác minh sự trùng lặp thực tế với khoảng thời gian hiện diện có thể bỏ lỡ các cơ hội thanh toán hợp lệ. Một dạng thất bại khác xuất phát từ việc giả định rằng những ngày bận rộn chỉ cản trở một quyết định trong một tháng; trên thực tế, họ có thể loại bỏ một cách có hệ thống tất cả các ngày thanh toán hợp lệ trong một tháng, buộc phải trục xuất chính xác ở ranh giới. 

Các trường hợp Edge thường phá vỡ quá trình triển khai ngây thơ bao gồm các tháng mà Rudolf chỉ có mặt bên ngoài cửa sổ thanh toán, các tháng mà anh ấy có mặt bên trong nhưng tất cả các ngày hợp lệ đều bận và các trường hợp khoảng thời gian hiện diện của anh ấy chồng lên nhau hai tháng nhưng chỉ giao nhau một phần với cửa sổ thanh toán. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng hàng ngày lên tới$10^6$, theo dõi xem Rudolf có mặt hay không, ngày đó có nằm trong cửa sổ thanh toán hay không và liệu ngày đó có bị chặn bởi một ngày bận rộn hay không. Chúng tôi sẽ duy trì một con trỏ trong những ngày bận rộn và một con trỏ trong các khoảng thời gian hiện diện và trong mỗi tháng, chúng tôi sẽ quét tất cả các ngày, kiểm tra xem có tồn tại ít nhất một cơ hội thanh toán hợp lệ hay không. Điều này đúng vì nó trực tiếp mô hình hóa các quy tắc, nhưng chi phí của nó trở nên đáng kể: một triệu lần lặp, mỗi lần lặp có khả năng thực hiện nhiều kiểm tra và tính toán biên, dẫn đến một giải pháp hệ số không đổi lớn chỉ có thể chấp nhận được nếu được tối ưu hóa cẩn thận.
