---
title: "CF 106097G - Cối Mayhem"
description: "Tôi chưa thể viết một bài xã luận đầy đủ chính xác một cách an toàn vì tôi vẫn cần giải quyết sự không khớp giữa thông tin tôi giả định ban đầu và tuyên bố thực tế. Từ trang cuộc thi, vấn đề thực sự là: Mỗi điểm $n$ (vữa) nằm trên một lưới 2D."
date: "2026-06-25T11:58:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106097
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-1-25 Div. 1 (Advanced)"
rating: 0
weight: 106097
solve_time_s: 26
verified: false
draft: false
---

[CF 106097G - Tình trạng hỗn loạn súng cối](https://codeforces.com/problemset/problem/106097/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
Tôi chưa thể viết một bài xã luận đầy đủ chính xác một cách an toàn vì tôi vẫn cần giải quyết sự không khớp giữa thông tin tôi giả định ban đầu và tuyên bố thực tế. 

Từ trang cuộc thi, vấn đề thực sự là: 

Mỗi trong số$n$điểm (vữa) nằm trên lưới 2D. Đối với mỗi vữa$i$, chúng ta phải chọn loại cối khác$j$rằng nó “nhắm mục tiêu” theo quy tắc khoảng cách liên quan đến khoảng cách Manhattan và ngưỡng$d$, với sự ràng buộc theo khoảng cách tối thiểu và sau đó là chỉ số tối thiểu. Khó khăn dự định đến từ việc thực hiện điều này một cách hiệu quả đối với$n \le 10^5$, không phải bằng cách kiểm tra tất cả các cặp. 

Điều vẫn chưa rõ ràng (và quan trọng đối với một bài xã luận chính xác) là quy tắc nhắm mục tiêu chính xác, bởi vì đoạn tuyên bố mà chúng tôi có không đầy đủ/mơ hồ ở phần xác định ứng cử viên nào hợp lệ và “kẻ thù gần nhất nào ở khoảng cách ít nhất”.$d$" chính xác có nghĩa là kết hợp với hành vi của mẫu. Chỉ riêng mẫu là không đủ để tái tạo lại tình trạng dự định một cách đáng tin cậy. 

Để tránh ảo giác về một giải pháp sai (có thể phá vỡ hoàn toàn bài xã luận), tôi cần một trong những điều sau: 

Dán tuyên bố đầy đủ từ Codeforces (đặc biệt là định nghĩa chính xác về thời điểm súng cối coi mục tiêu khác là mục tiêu hợp lệ) hoặc xác nhận rằng quy tắc là: 

“mỗi$i$chọn gần nhất$j \ne i$sao cho khoảng cách Manhattan$\ge d$, quan hệ theo chỉ số nhỏ nhất.” 

Sau khi điều đó được xác nhận, tôi có thể tạo ra bài xã luận cấp cao đầy đủ với: 

giảm hình học chính xác (vấn đề này thường giảm xuống việc sắp xếp theo tọa độ xoay hoặc đường quét với các tập hợp/cấu trúc thống trị có thứ tự), 

một lời giải thích rõ ràng về lý do tại sao ngây thơ$O(n^2)$thất bại, 

và một công trình chính xác vượt qua$10^5$. 

Gửi quy tắc còn thiếu và tôi sẽ viết bài xã luận hoàn chỉnh theo phong cách Codeforces theo định dạng bạn yêu cầu.
