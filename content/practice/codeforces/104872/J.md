---
title: "CF 104872J - Đường phố vùng đất bằng phẳng"
description: "Chúng ta có một cấu trúc liên thông được tạo thành từ $n$ vị trí được kết nối bởi chính xác $n-1$ những con đường vô hướng, vì vậy biểu đồ bên dưới là một cái cây."
date: "2026-06-28T10:34:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104872
codeforces_index: "J"
codeforces_contest_name: "2023-2024 Russia Team Open, High School Programming Contest (VKOSHP XXIV)"
rating: 0
weight: 104872
solve_time_s: 29
verified: false
draft: false
---

[CF 104872J - Đường phố vùng đất bằng](https://codeforces.com/problemset/problem/104872/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc kết nối được làm bằng$n$vị trí được kết nối chính xác$n-1$những con đường vô hướng nên đồ thị bên dưới là một cái cây. Mỗi con đường có thể được coi là một đoạn được định hướng theo cả hai hướng và vấn đề đưa ra ý tưởng rằng một số con đường liên tiếp nhất định có thể được hợp nhất thành một “đường cao tốc” dài hơn nếu chúng tương thích về mặt hình học tại điểm cuối chung của chúng. 

Phép toán chính là cục bộ tại một đỉnh: nếu hai con đường gặp nhau tại một đỉnh$v$, giả sử một người đến từ$u$ĐẾN$v$và một người khác rời đi$v$ĐẾN$w$, thì theo quy tắc hình học dựa trên thứ tự tuần hoàn của các cạnh xung quanh$v$, hai con đường này có thể được coi là tương thích và có thể hợp nhất thành một đoạn đường cao tốc liên tục đi qua$v$. Mỗi lần hợp nhất thành công sẽ giảm số lượng đường cao tốc riêng biệt xuống còn một. 

Trạng thái ban đầu là mỗi con đường hoạt động như một đoạn đường cao tốc độc lập. Vì có$n-1$ban đầu có những con đường$n-1$đường cao tốc. Nhiệm vụ là thực hiện càng nhiều lần hợp nhất hợp lệ càng tốt và xuất ra số lượng đường cao tốc tối thiểu còn lại sau tất cả các lần hợp nhất có thể. 

Từ quan điểm tính toán, đồ thị có$O(n)$các cạnh và mọi giải pháp đều phải chạy trong thời gian gần tuyến tính hoặc gần tuyến tính. Giải pháp kiểm tra các cặp đường trên toàn cầu sẽ quá chậm vì có thể có$O(n^2)$tương tác tiềm năng giữa các cạnh sự cố trên các đỉnh. 

Một vấn đề khó phát hiện khi một đỉnh có nhiều đường tới. Một cách tiếp cận ngây thơ có thể cố gắng kết hợp một cách tham lam các con đường ở địa phương mà không tôn trọng tính nhất quán toàn cầu. Điều này có thể thất bại khi các lựa chọn cục bộ ảnh hưởng lẫn nhau. 

Ví dụ, hãy xem xét một đỉnh có bốn cạnh liên tiếp được sắp xếp theo chu kỳ. Nếu thuật toán tham lam ghép cạnh đầu tiên với cạnh lân cận theo chiều kim đồng hồ mà không xem xét cấu trúc khớp toàn cục, thì nó có thể chặn một cặp tốt hơn mang lại nhiều sự hợp nhất tổng thể hơn. Hành vi đúng phụ thuộc vào thứ tự tuần hoàn chứ không phải ghép nối tùy ý. 

Khó khăn cốt lõi là khả năng tương thích được xác định bởi sự kề cận góc, do đó tính chính xác phụ thuộc vào thứ tự các cạnh xung quanh mỗi đỉnh chứ không chỉ khả năng kết nối. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các sự hợp nhất có thể xảy ra một cách rõ ràng. Người ta có thể quét liên tục tất cả các đỉnh, xem xét tất cả các cặp cạnh tới, kiểm tra tính tương thích và hợp nhất bất cứ khi nào có thể. Mỗi lần hợp nhất sẽ giảm số lượng đường cao tốc đi một và chúng tôi sẽ tiếp tục cho đến khi không thể hợp nhất được nữa. 

Cách tiếp cận này đúng vì nó trực tiếp tuân theo định nghĩa về các hoạt động được phép. Tuy nhiên, mỗi lần hợp nhất yêu cầu quét các cấu trúc lân cận và cập nhật chúng, và có thể có$O(n)$hợp nhất. Mỗi lần quét trên tất cả các đỉnh có giá$O(n)$, cho$O(n^2)$hoặc hành vi tệ hơn nói chung, quá chậm để$n$lên đến$10^5$. 

Quan sát quan trọng là việc hợp nhất ở các đỉnh khác nhau sẽ độc lập một khi chúng ta diễn giải lại cấu trúc. Mỗi con đường chỉ tham gia vào các quyết định tương thích thông qua việc sắp xếp cục bộ xung quanh mỗi điểm cuối. Do đó, chúng ta có thể phân tách đỉnh bài toán theo đỉnh. Tại mỗi đỉnh$v$, đồng
