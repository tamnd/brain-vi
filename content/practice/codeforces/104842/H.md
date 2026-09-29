---
title: "CF 104842H - Kẻ ăn thịt người đói khát"
description: "Chúng tôi gặp một nhóm người đang cố gắng vượt sông bằng một chiếc thuyền rất nhỏ. Có hai loại người: ăn thịt người và truyền giáo."
date: "2026-06-28T11:32:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104842
codeforces_index: "H"
codeforces_contest_name: "2020-2021 ICPC, Moscow Subregional"
rating: 0
weight: 104842
solve_time_s: 21
verified: false
draft: false
---

[CF 104842H - Những kẻ ăn thịt người đói khát](https://codeforces.com/problemset/problem/104842/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi gặp một nhóm người đang cố gắng vượt sông bằng một chiếc thuyền rất nhỏ. Có hai loại người: ăn thịt người và truyền giáo. Thuyền có thể chở tối đa hai hành khách mỗi chuyến, nhưng nó không thể di chuyển trừ khi ít nhất một trong số các hành khách có khả năng điều khiển mái chèo. Chỉ một nhóm nhỏ những kẻ ăn thịt người và một nhóm nhỏ những người truyền giáo mới có khả năng này. 

Ràng buộc an toàn là quy tắc săn mồi cổ điển được áp dụng độc lập trên mỗi bờ sông. Tại bất kỳ thời điểm nào, nếu ngân hàng có ít nhất một nhà truyền giáo thì số lượng người ăn thịt người trong cùng ngân hàng đó không được vượt quá số lượng nhà truyền giáo. Nếu không có người truyền giáo trên ngân hàng, những kẻ ăn thịt người có thể có mặt tự do mà không gây nguy hiểm. 

Nhiệm vụ là xác định xem liệu có thể di chuyển tất cả mọi người từ bờ xuất phát sang bờ đối diện bằng cách sử dụng một chuỗi các chuyến đi thuyền hợp lệ mà không bao giờ vi phạm các ràng buộc an toàn ở cả hai bờ hay không. 

Mỗi trường hợp thử nghiệm đưa ra số lượng người ăn thịt người và người truyền giáo, cùng với số lượng trong mỗi nhóm có thể điều khiển con thuyền. 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm, với kích thước mỗi nhóm lên tới 1000. Điều này ngay lập tức gợi ý rằng mọi mô phỏng trạng thái phải cực kỳ nhỏ gọn. Một biểu đồ trạng thái đơn giản về tất cả sự phân bổ người giữa các ngân hàng sẽ lớn, nhưng quan trọng hơn, hạn chế về thuyền sẽ bổ sung thêm các hạn chế về định hướng và kỹ năng, khiến BFS đầy đủ trên các cấu hình trở nên quá đắt nếu được thực hiện bất cẩn ở mỗi trạng thái không có cấu trúc. 

Các trường hợp khó khăn chính xuất phát từ các tình huống không thể di chuyển do thiếu người chèo hoặc cấu hình trung gian buộc phải không an toàn. Ví dụ: nếu không ai có thể vận hành con thuyền, thì ngay cả một người cũng không thể di chuyển được, do đó, mọi cấu hình ban đầu không trống với cả hai bờ có liên quan đều trở thành không thể trừ khi mọi người đã ở một bên. Một trường hợp tế nhị khác là khi những người truyền giáo có mặt nhưng luôn đông hơn ở một số phía sau khi thuyên chuyển, điều này có thể xảy ra ngay cả khi tổng số lượng có vẻ cân bằng. 

Một kịch bản thất bại cụ thể là: 

Inp
