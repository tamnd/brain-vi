---
title: "CF 417D - Gena Xảo quyệt"
description: "Gena có một loạt vấn đề và một nhóm bạn, mỗi người có thể giải quyết một số vấn đề nhỏ trong số đó. Nếu Gena thuê một người bạn, người bạn đó sẽ giải quyết mọi vấn đề mà họ có khả năng giải quyết, giải quyết nhiều vấn đề cùng một lúc."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 417
codeforces_index: "D"
codeforces_contest_name: "RCC 2014 Warmup (Div. 2)"
rating: 1900
weight: 417
solve_time_s: 34
verified: false
draft: false
---

[CF 417D - Gena xảo quyệt](https://codeforces.com/problemset/problem/417/D) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, dp, tham lam, sắp xếp 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Gena có một loạt vấn đề và một nhóm bạn, mỗi người có thể giải quyết một số vấn đề nhỏ trong số đó. Nếu Gena thuê một người bạn, người bạn đó sẽ giải quyết mọi vấn đề mà họ có khả năng giải quyết, giải quyết nhiều vấn đề cùng một lúc. Mỗi người bạn có hai chi phí độc lập: khoản thanh toán bằng tiền cố định và yêu cầu về cơ sở hạ tầng, được biểu thị bằng số lượng màn hình tối thiểu phải được kết nối. Mỗi màn hình có giá cố định như nhau. 

Mục tiêu là chọn một tập hợp con những người bạn sao cho mọi vấn đề đều được giải quyết bởi ít nhất một người bạn đã chọn và tổng chi phí, bao gồm cả khoản thanh toán cho bạn bè và chi phí mua đủ màn hình để làm hài lòng tất cả những người bạn đã chọn, được giảm thiểu. 

Kích thước đầu vào nhỏ về số lượng vấn đề, nhiều nhất là 20, điều này gợi ý rõ ràng về sự biểu diễn mặt nạ bit trên các tập hợp con của vấn đề. Số lượng bạn bè lên tới 100, điều này cho phép lặp lại họ trong khi thực hiện phép tính theo cấp số nhân trên các tập hợp con bài toán. 

Điểm tinh tế chính là yêu cầu về màn hình phụ thuộc vào tập hợp con bạn bè đã chọn: nếu bất kỳ người bạn nào được chọn yêu cầu k màn hình thì số lượng màn hình cuối cùng ít nhất phải là k tối đa trong số tất cả những người bạn được chọn. Điều này kết hợp việc lựa chọn tập hợp con với một ràng buộc tối đa, rất dễ bị xử lý sai nếu được đối xử một cách tham lam với mỗi người bạn một cách độc lập. 

Một sai lầm ngây thơ là cho rằng mỗi người bạn có thể được đánh giá độc lập hoặc tham lam giành lấy những người bạn rẻ tiền trước. Điều đó không thành công vì một người bạn đắt tiền hơn một chút có thể mở khóa toàn bộ phạm vi phủ sóng trong khi giảm số lượng bạn bè cần thiết, điều này có thể làm giảm yêu cầu giám sát tối đa và tổng chi phí. 

Chế độ thất bại thứ hai xuất hiện khi tất cả bạn bè cùng nhau giải quyết mọi vấn đề, nhưng từng người bạn riêng lẻ thì không. Điều này buộc việc kết hợp các tập hợp con và cấu trúc phụ thuộc trở thành tổ hợp thuần túy trên các tập hợp con của bạn bè và liên minh chính phủ của họ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi nhóm bạn bè. Đối với mỗi tập hợp con, hãy tính hợp các bài toán đã giải, kiểm tra xem nó có bao gồm tất cả m bài toán hay không, tính tổng số tiền thanh toán và tính số màn hình cần thiết là k tối đa trong số những người bạn được chọn. Điều này đúng nhưng đắt tiền. 

Số lượng tập hợp con là 2^n và đối với mỗi tập hợp con, chúng ta có thể cần tính toán lại phạm vi bao phủ lên tới m bit. Với n = 100, đây là khoảng 2^100 tập hợp con, điều này hoàn toàn không khả thi. 

Nhận xét quan trọng là trạng thái mà chúng ta quan tâm không phải là tập hợp con của những người bạn mà là tập hợp các vấn đề đã được giải quyết. Vì m 20, tất cả các trạng thái vấn đề có thể xảy ra đều nằm gọn trong một mặt nạ bit có kích thước 2^m, tức là khoảng một triệu trạng thái. Điều này đủ nhỏ để lập trình động. 

Thay vì lặp lại các tập hợp con của bạn bè, chúng tôi lặp lại các mặt nạ có vấn đề và tính toán cách rẻ nhất để đạt được từng mặt nạ. Mỗi người bạn đóng vai trò như một quá trình chuyển đổi, bổ sung thêm mặt nạ bit của các vấn đề có thể giải quyết được, đồng thời đóng góp chi phí và có thể tăng số lượng màn hình cần thiết. 

Điều phức tạp là yêu cầu giám sát: nó phụ thuộc vào k tối đa trong số những người bạn được chọn, vì vậy chúng ta phải coi k như một phần của kích thước trạng thái. Chúng tôi sắp xếp bạn bè theo k và xử lý DP theo thứ tự k tăng dần, sao cho khi chúng tôi đưa vào một người bạn, tất cả các trạng thái trước đó đã đáp ứng các ràng buộc giám sát thấp hơn hoặc bằng nhau. Sau đó, chúng ta có thể áp dụng các chuyển đổi DP tập hợp con tiêu chuẩn trên các mặt nạ vấn đề. 

Điều này biến vấn đề thành một DP phân lớp trong đó mỗi lớp tương ứng với việc cho phép bạn bè đạt đến một ngưỡng yêu cầu giám sát nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với tập hợp con bạn bè | O(2^n · m) | O(m) | Quá chậm | 
| Bitmask DP gặp vấn đề với k lớp được sắp xếp | O(n · 2^m) | O(2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi người bạn như một món đồ có chi phí, một chút vấn đề họ giải quyết và yêu cầu giám sát.

Chúng tôi duy trì một mảng DP trên các mặt nạ có vấn đề, trong đó dp[mask] lưu trữ chi phí tối thiểu của số tiền chi cho bạn bè dưới ngưỡng giám sát cố định. Chúng tôi sẽ tính toán lại hoặc cập nhật DP này đồng thời tăng dần mức giám sát được phép. 

Chúng tôi xử lý bạn bè được sắp xếp theo mức tăng k. 

1. Sắp xếp bạn bè theo màn hình yêu cầu của họ k. Điều này đảm bảo rằng khi chúng tôi xem xét một người bạn, bất kỳ người bạn nào được coi là bạn bè trước đây đều không yêu cầu nhiều màn hình hơn ngưỡng hiện tại, điều này cho phép chúng tôi hiểu lớp hiện tại là “chúng tôi chỉ được phép sử dụng những người bạn này”. 
2. Khởi tạo mảng dp có kích thước 2^m với vô cùng và đặt dp[0] = 0. Điều này thể hiện rằng việc giải quyết không tốn kém gì. 
3. Lặp đi lặp lại
