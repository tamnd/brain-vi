---
title: "CF 105204A - \u0420\u0430\u0437\u0431\u0438\u0432\u0430\u0435\u043c \u043b\u0430\u0433\u0435\u0440\u044c"
description: "Chúng tôi có ba loại người tham gia phải được xếp vào các lều giống hệt nhau, mỗi lều có sức chứa tối đa ba người. Điều khó khăn là mỗi loại áp đặt một hạn chế về cách họ được phép ở chung lều. Người hướng nội khăng khăng muốn ở một mình trong lều của họ."
date: "2026-06-27T02:41:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105204
codeforces_index: "A"
codeforces_contest_name: "\u0412\u041a\u041e\u0428\u041f.Junior 2024"
rating: 0
weight: 105204
solve_time_s: 50
verified: false
draft: false
---

[CF 105204A - \u0420\u0430\u0437\u0431\u0438\u0432\u0430\u0435\u043c \u043b\u0430\u0433\u0435\u0440\u044c](https://codeforces.com/problemset/problem/105204/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba loại người tham gia phải được xếp vào các lều giống hệt nhau, mỗi lều có sức chứa tối đa ba người. Điều khó khăn là mỗi loại áp đặt một hạn chế về cách họ được phép ở chung lều. 

Người hướng nội khăng khăng muốn ở một mình trong lều của họ. Họ vẫn chiếm một chiếc lều đầy đủ sức chứa ba người, nhưng không ai khác có thể ở cùng họ. Người hướng ngoại yêu cầu thái cực ngược lại: mỗi người hướng ngoại phải ở trong một căn lều có đúng hai người khác, nghĩa là mỗi căn lều có người hướng ngoại bất kỳ phải có đầy đủ ba người. Các phổ quát rất linh hoạt và có thể được đặt trong bất kỳ cấu hình nào, một mình hoặc cùng với các cấu hình khác, miễn là không có ràng buộc nào khác bị vi phạm. 

Nhiệm vụ là xác định số lượng lều tối thiểu cần thiết để chứa tất cả mọi người trong khi vẫn tôn trọng mọi ràng buộc hoặc quyết định rằng không có sự sắp xếp hợp lệ nào tồn tại. 

Các ràng buộc cho phép số lượng lên tới 10^9, vì vậy mọi giải pháp đều phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào liên quan đến việc tìm kiếm sự phân bổ người vào các lều hoặc thử kết hợp một cách rõ ràng sẽ quá chậm. 

Một số tình huống rất dễ mắc sai lầm nếu tiếp cận một cách tham lam mà không có cơ cấu. 

Nếu không có phổ quát và số lượng người hướng ngoại không chia hết cho ba, ví dụ như đầu vào`0 2 0`, chúng ta không thể dựng lều đầy đủ cho hai người hướng ngoại còn lại nên câu trả lời là không thể. Một giải pháp ngây thơ chỉ đơn giản là tính toán`ceil(b / 3)`sẽ tuyên bố không chính xác về tính khả thi. 

Một trường hợp thất bại khác xuất hiện khi những điều phổ biến gần như không đủ để hoàn thiện nhóm hướng ngoại cuối cùng. Ví dụ`0 1 1`không thể làm việc được, bởi vì người hướng ngoại độc thân cần thêm hai người nữa để tạo thành một chiếc lều hợp lệ, nhưng chỉ tồn tại một phổ quát. 

Khó khăn cốt lõi là người hướng ngoại áp đặt những ràng buộc nghiêm ngặt “nhóm đầy đủ ba người”, trong khi người hướng nội áp đặt những ràng buộc nghiêm ngặt “không chia sẻ” và những cái phổ quát hoạt động như một phần đệm linh hoạt phải được phân bổ cẩn thận. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ cố gắng
