---
title: "CF 105167K - Quan tâm đến R\u00f6sti"
description: "Chúng ta có một hàng gồm $n$ học sinh được sắp xếp theo một thứ tự cố định và một quá trình xác suất tác động nhiều lần lên học sinh ở đầu."
date: "2026-06-27T10:37:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105167
codeforces_index: "K"
codeforces_contest_name: "ETH Zurich Competitive Programming Contest Spring 2024"
rating: 0
weight: 105167
solve_time_s: 30
verified: false
draft: false
---

[CF 105167K - Quan tâm đến R\u00f6sti](https://codeforces.com/problemset/problem/105167/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng đợi$n$học sinh được sắp xếp theo một trật tự cố định và một quá trình xác suất tác động liên tục lên học sinh ở phía trước. Mỗi vòng hoạt động theo cùng một cách: học sinh đứng đầu sẽ nhận được Rösti duy nhất có sẵn với xác suất$\frac{a}{b}$, kết thúc quá trình ngay lập tức hoặc thất bại với xác suất$1 - \frac{a}{b}$. Trong trường hợp không đạt, học sinh đó trả một đồng xu và di chuyển về phía sau hàng đợi. 

Chúng ta được yêu cầu tính tổng số xu dự kiến ​​được trả bởi một sinh viên cụ thể, Oli, người bắt đầu ở vị trí$k$. 

Điểm tinh tế quan trọng là thứ tự hàng đợi thay đổi một cách xác định ngoại trừ việc xoay từ trước ra sau, trong khi điều kiện dừng chỉ phụ thuộc vào sự kiện thành công toàn cục. Mỗi học sinh đóng góp chính xác một xu mỗi lần họ dẫn đầu và thất bại, và chúng tôi chỉ quan tâm đến việc đếm xem có bao nhiêu thất bại như vậy liên quan đến Oli trước khi thành công đầu tiên xảy ra ở bất kỳ đâu. 

Những ràng buộc cho phép$n$lên đến$10^9$, vì vậy bất kỳ mô phỏng nào trên các trạng thái hàng đợi đều không thể thực hiện được. Ngay cả một vị trí vượt qua tuyến tính cũng quá lớn. Các giá trị xác suất cũng là những số nguyên lớn đến$10^9$, điều này buộc số học mô-đun theo$10^9+7$. 

Một cách tiếp cận đơn giản sẽ mô phỏng hàng đợi. Mỗi bước là$O(1)$, nhưng trong trường hợp xấu nhất, quy trình có thể chạy tùy ý nhiều bước trước khi thành công. Số bước dự kiến ​​là vô hạn theo quan điểm mô phỏng đơn giản, do đó mô phỏng trực tiếp là không khả thi. 

Trường hợp cạnh tinh tế hơn xuất hiện khi$a = b$. Trong trường hợp đó, học sinh đầu tiên luôn thành công ngay lập tức nên không ai di chuyển và chi phí dự kiến ​​của Oli chính xác là$0$. Bất kỳ giải pháp nào đảo ngược xác suất một cách mù quáng mà không kiểm tra tính suy biến này sẽ thất bại do chia cho 0 trong số học mô-đun. 

Một trường hợp cạnh khác là$a = 0$, điều đó có nghĩa là quá trình không bao giờ kết thúc, nhưng các ràng buộc nghiêm cấm nó một cách rõ ràng vì$a \ge 1$. Tuy nhiên, một đạo hàm đúng phải tránh giả sử các thuộc tính kết thúc phụ thuộc vào các điều kiện mạnh hơn đã cho. 

## Phương pháp tiếp cận 

Chế độ xem brute-force theo dõi hàng đợi một cách rõ ràng. Ở mỗi bước, chúng tôi xác định học viên đứng đầu, lấy mẫu xem họ có thành công hay không và nếu không, chúng tôi sẽ luân chuyển hàng đợi và tăng chi phí cho học viên đó. Chúng tôi dừng lại khi thành công xảy ra. Điều này mô hình hóa chính xác quy trình nhưng không dẫn đến kỳ vọng có thể tính toán được ở dạng đóng và không thể mô phỏng hiệu quả vì thời gian dừng là không giới hạn. 

Quan sát cấu trúc quan trọng là danh tính của các sinh viên phía trước có chu kỳ xác định theo chu kỳ$n$, độc lập với xác suất. Sự ngẫu nhiên duy nhất là liệu một “chuyến thăm mặt trận” nhất định có mang lại thành công hay không. Do đó, quá trình này có thể được xem như một chuỗi các phép thử Bernoulli độc lập được áp dụng cho các vị trí phía trước liên tiếp, trong khi việc xoay hàng đợi chỉ xác định học sinh nào bị tính phí trong mỗi lần thử. 

Theo quan điểm của Oli, mỗi chu kỳ đầy đủ của$n$thất bại làm thay đổi vị trí của anh ấy một cách chính xác$n$,
