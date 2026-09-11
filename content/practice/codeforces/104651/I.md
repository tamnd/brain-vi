---
title: "CF 104651I - Máy Tạo Quái Vật"
description: "Chúng tôi được cung cấp một nhóm quái vật cố định và chúng tôi mô phỏng quá trình huấn luyện trong một chuỗi ngày được lập chỉ mục bởi tham số $k$. Vào mỗi ngày $k$, mỗi quái vật có hai giá trị phụ thuộc vào ngày: tiêu tốn một lượng HP để đánh bại và sau đó nó sẽ trả lại một số phần thưởng HP sau khi bị đánh bại."
date: "2026-06-29T16:29:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104651
codeforces_index: "I"
codeforces_contest_name: "The 2023 CCPC Online Contest"
rating: 0
weight: 104651
solve_time_s: 40
verified: false
draft: false
---

[CF 104651I - Máy tạo quái vật](https://codeforces.com/problemset/problem/104651/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm quái vật cố định và chúng tôi mô phỏng quá trình huấn luyện trong một chuỗi ngày được lập chỉ mục bởi một tham số$k$. Vào mỗi ngày$k$, mỗi quái vật có hai giá trị phụ thuộc vào ngày: tiêu tốn một lượng HP để đánh bại và sau đó nó sẽ trả lại một số phần thưởng HP sau khi bị đánh bại. Người chơi chọn thứ tự chiến đấu với quái vật, nhưng bắt đầu ngày mới với giá trị HP ban đầu mới$s_k$, và mục tiêu là chọn$s_k$càng nhỏ càng tốt trong khi vẫn đảm bảo rằng trong toàn bộ chuỗi chiến đấu, HP không bao giờ giảm xuống dưới 0. 

Điều phức tạp quan trọng là cả chi phí và phần thưởng của mỗi quái vật đều thay đổi tuyến tính theo chỉ số ngày.$k$. Điều này có nghĩa là thứ tự chiến đấu tối ưu không cố định qua các ngày, bởi vì việc thay đổi$k$thay đổi “hình dạng” hiệu quả của từng quái vật. 

Kết quả đầu ra không phải là câu trả lời của bất kỳ ngày nào mà là tổng của tất cả các giá trị HP ban đầu tối thiểu được yêu cầu trong tất cả các ngày kể từ$k = 0$ĐẾN$k = m$, bao gồm. Từ$m$có thể lớn như$10^{18}$, chúng ta không thể mô phỏng hàng ngày được. 

Ràng buộc$n \le 100$là gợi ý cấu trúc quan trọng. Bất kỳ nghiệm nào ít nhất là bậc hai hoặc bậc ba trong$n$được chấp nhận, nhưng bất cứ điều gì tùy thuộc vào$m$trực tiếp là không thể. Điều này ngay lập tức gợi ý rằng toàn bộ giải pháp phải giảm sự phụ thuộc vào$k$thành một cấu trúc từng phần. 

Một trường hợp phức tạp xuất hiện khi nghĩ đến việc sắp xếp thứ tự: một cách tiếp cận ngây thơ có thể cho rằng thứ tự tối ưu của quái vật là cố định cho tất cả$k$, rồi tính công thức tuyến tính mỗi ngày. Điều này không thành công vì thứ tự tương đối giữa hai quái vật có thể bị đảo lộn$k$những thay đổi. 

Ví dụ: giả sử quái vật A có sát thương cơ bản lớn hơn một chút nhưng giảm nhanh hơn theo thời gian, trong khi quái vật B khởi đầu dễ dàng hơn nhưng lại tệ đi nhanh hơn. Lúc nhỏ$k$, A có thể tệ hơn; nói chung$k$, B có thể tệ hơn. Mọi thứ tự cố định sẽ trở nên dưới mức tối ưu sau điểm giao nhau. 

Một vấn đề tiềm ẩn khác là ngay cả đối với một thứ tự cố định, lượng HP ban đầu cần thiết không chỉ là một tổng; nó phụ thuộc vào mức thâm hụt tiền tố tối đa theo thời gian, vì vậy việc xử lý quái vật một cách độc lập cũng không thành công. 

## Phương pháp tiếp cận 

Nếu chúng ta sửa chữa một ngày$k$, vấn đề trở thành một câu hỏi lập kế hoạch cổ điển. Mỗi quái vật có cấu trúc trả giá rồi thưởng: đầu tiên bạn mất HP, sau đó bạn nhận được HP. Với một thứ tự cố định, chúng ta có thể tính HP ban đầu tối thiểu bằng cách quét theo thứ tự đó và theo dõi tổng tiền tố tối thiểu. 

The brute-force idea is straightforward. Cho mỗi ngày$k$, tính toán tất cả các giá trị của quái vật, thử mọi hoán vị của quái vật, tính lượng HP ban đầu cần thiết và lấy mức tối thiểu. Điều này đúng nhưng vô vọng:$n!$hoán vị mỗi ngày và$m$lên đến$10^{18}$ngày làm cho nó hoàn toàn không khả thi. 

Việc đơn giản hóa đầu tiên là loại bỏ tìm kiếm hoán vị. Đối với một cố định$k$, thứ tự tối ưu được xác định theo quy tắc tham lam: hoán đổi hai quái vật liền kề sẽ không cải thiện lượng HP ban đầu cần thiết. Điều này dẫn đến việc sắp xếp quái vật theo một phím tuyến tính tùy thuộc vào mức tăng và giảm hiệu quả của chúng trong ngày$k$. Cụ thể, mỗi quái vật đều có thông số hiệu quả:$$a_i(k) = a_i + \Delta a_i \cdot k, \quad b_i(k) = b_i + \Delta b_i \cdot k$$và mỗi quái vật đóng góp một “cấu trúc bất lợi ròng” dẫn đến tiêu chí đặt hàng:$$(a_i(k) - b_i(k))$$vì vậy thứ tự phụ thuộc vào hàm tuyến tính của$k$. 

Đây là vấn đề quan trọng: thứ tự sắp xếp tự nó thay đổi theo$k$. Tuy nhiên, so sánh theo cặp giữa hai con quái vật là sự bất bình đẳng tuyến tính trong$k$, do đó mỗi cặp xác định nhiều nhất một điểm giao nhau. Với$n \le 100$, điều này mang lại$O(n^2)$các điểm quan trọng chia dòng thời gian thành các khoảng trong đó thứ tự được cố định. 

Trong một khoảng như vậy, thứ tự là cố định, nên chúng ta chỉ cần tính HP ban đầu cần thiết như một hàm của$k$. Ngay cả khi đó, nó không phải là một hàm tuyến tính đơn lẻ: nó là mức thiếu hụt tiền tố tối đa và mỗi tiền tố là một hàm tuyến tính trong$k$. Vì vậy, câu trả lời sẽ trở thành phong bì trên của$O(n)$dòng trong$k$. 

Điều này dẫn đến một cấu trúc rõ ràng: đối với mỗi khoảng đặt hàng, hãy tính đường bao của các hàm tuyến tính, sau đó lấy tích phân hàm tuyến tính từng phần đó trong khoảng và tính tổng đóng góp trên tất cả các khoảng. 

Ý tưởng chính là chúng tôi không bao giờ trực tiếp xử lý$m$các bước. Chúng tôi chỉ xử lý: 

1. Khoảng thời gian O(n^2) trong đó thứ tự ổn định. 
2. O(n) dòng trên mỗi khoảng. 
3. Độ phức tạp của đường bao O(n) trên mỗi khoảng. 

| Tiếp cận |
