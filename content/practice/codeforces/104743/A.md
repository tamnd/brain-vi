---
title: "CF 104743A - Tạo tất cả các phần tử 0"
description: "Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác đơn lẻ, chúng ta chọn một phân đoạn liền kề và áp dụng AND theo bit với giá trị $x$ nào đó, trong đó $1 le x le k$. Thao tác này ghi đè mọi phần tử trong phân đoạn đã chọn bằng cách xóa một số bit của nó theo $x$."
date: "2026-06-29T01:20:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104743
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #25(5^2-Forces)"
rating: 0
weight: 104743
solve_time_s: 30
verified: false
draft: false
---

[CF 104743A - Tạo tất cả các phần tử 0](https://codeforces.com/problemset/problem/104743/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác đơn lẻ, chúng tôi chọn một phân đoạn liền kề và áp dụng bit AND với một số giá trị$x$, Ở đâu$1 \le x \le k$. Thao tác này ghi đè lên mọi phần tử trong phân đoạn đã chọn bằng cách xóa một số bit của nó theo$x$. Mục tiêu là chuyển đổi toàn bộ mảng thành tất cả các số 0 bằng cách sử dụng số lượng tối thiểu các thao tác phân đoạn đó hoặc xác định rằng không thể thực hiện được. 

Quan sát quan trọng là bitwise AND chỉ có thể tắt các bit chứ không bao giờ bật. Vì vậy, mỗi phần tử chỉ có thể di chuyển xuống theo thứ tự từng phần theo bit được xác định bằng cách bao gồm các bit. Đạt đến 0 có nghĩa là mọi bit xuất hiện trong bất kỳ phần tử nào đều phải bị loại bỏ tại một thời điểm nào đó bằng thao tác áp dụng mặt nạ loại bỏ bit đó. 

Các ràng buộc đủ nhỏ để bất kỳ nghiệm bậc hai nào trong$n$mỗi trường hợp thử nghiệm có thể vượt qua, nhưng mọi thao tác tính toán lại theo khối hoặc liên quan đến từng bit trên tất cả các mảng con sẽ quá chậm. Tổng của tất cả$n$chỉ là$10^4$, điều này gợi ý mạnh mẽ rằng một$O(n^2)$hoặc$O(n \log k)$cách tiếp cận dự kiến. 

Trường hợp cạnh khóa phát sinh từ thực tế là phép toán bị giới hạn bởi$k$. Nếu như$k$không chứa một bit nhất định xuất hiện trong một số$a_i$, thì bit đó không bao giờ có thể bị xóa khỏi vị trí đó nếu chúng ta buộc phải sử dụng$x \le k$. Ví dụ, nếu$a = [1]$Và$k = 0$, vấn đề đã không thể thực hiện được vì không có giá trị$x$tồn tại. Một trường hợp tế nhị hơn là khi$k$thiếu một số bit cao nhất định cần thiết để xóa các phần tử. 

Một trường hợp lỗi khác xuất phát từ sự hiểu lầm rằng việc áp dụng AND trên một phân đoạn sẽ không hợp nhất thông tin giữa các phần tử. Mỗi phần tử được che dấu độc lập bởi cùng một$x$, do đó, việc lựa chọn phân khúc hoàn toàn là về việc nhóm các mức giảm giống hệt nhau hoặc tương thích chứ không phải về việc tích lũy tiến trình cho mỗi phần tử. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực xem xét mọi chuỗi hoạt động có thể xảy ra. Mỗi thao tác chọn một phân đoạn và một giá trị$x$, áp dụng mặt nạ và tiếp tục cho đến khi tất cả các phần tử trở về 0. Quan điểm này ngay lập tức trở nên khó hiểu vì ngay cả số lượng lựa chọn phân khúc cũng$O(n^2)$và chuỗi các hoạt động tăng theo cấp số nhân. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cố gắng quyết định, đối với mỗi phân đoạn, cần bao nhiêu thao tác để giảm tất cả các phần tử trong phân đoạn đó về 0. Ngay cả ở đây, việc tính toán lại trạng thái bit sau mỗi thao tác dẫn đến việc quét lặp lại các phân đoạn và thao tác lặp lại bit, tạo ra ít nhất$O(n^3)$hành vi trong việc thực hiện đơn giản. 

Quan sát cấu trúc quan trọng là các phép toán AND chỉ loại bỏ các bit và việc loại bỏ một bit khỏi nhiều phần tử liền kề có thể được coi là "che phủ" bit đó trên một phân đoạn. Mỗi thao tác tương ứng với việc chọn một đoạn và chọn mặt nạ$x$, xác định chính xác bit nào còn lại, nghĩa là nó xác định bit nào sẽ bị xóa. 

Chúng ta có thể đảo ngược quan điểm: thay vì nghĩ về những gì còn lại, chúng ta nghĩ về những phần nào phải loại bỏ. Mỗi phần tử có một tập hợp các bit 1 cố định phải được loại bỏ bằng một số thao tác bao gồm chỉ mục đó và sử dụng một$x$điều đó sẽ biến những bit đó thành 0. 

Bây giờ hãy xem xét một vị trí bit một cách độc lập. Đối với một bit cố định$b$, chúng tôi xem xét tất cả các chỉ số nơi bit đó được đặt. Để loại bỏ nó, chúng ta phải áp dụng một thao tác bao phủ các vị trí đó bằng một$x$cái đó có chút$b$đặt về không. Từ$x \le k$, chỉ cho phép một số mẫu bit nhất định và điều này hạn chế tính khả thi. 

Vấn đề giảm xuống còn việc nhóm các vị trí thành các phân đoạn trong đó một thao tác đơn lẻ có thể xóa đồng thời tất cả các bit cần thiết cho tất cả các phần tử được bao phủ. Mỗi hoạt động về cơ bản là một lớp phủ phân đoạn giúp giảm các yêu cầu về bitwise trên phân đoạn đó. Số lượng thao tác tối thiểu tương ứng với việc phân chia tối thiểu mảng thành các vùng trong đó mỗi vùng có thể được xóa một cách tối ưu. 

Điều này dẫn đến chiến lược phân đoạn tham lam: chúng tôi mở rộng một phân đoạn miễn là tất cả các phần tử trong đó có chung một ràng buộc tương thích dưới một số mặt nạ hợp lệ$x \le k$. Khi ràng buộc bị phá vỡ, chúng tôi cắt và bắt đầu một phân đoạn mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi vị trí, hãy xác định xem các bit của nó có thể bị xóa dưới ràng buộc hay không$x \le k$. Nếu một phần tử có một bit không thể bị loại bỏ bởi bất kỳ$x$, câu trả lời ngay lập tức là không thể. Việc kiểm tra này giúp tránh lãng phí thời gian vào những trường hợp không đạt yêu cầu. 
2. Duyệt mảng từ trái sang phải trong khi vẫn duy trì tập hợp các bit vẫn là “yêu cầu hoạt động” cho phân đoạn hiện tại. Ban đầu, bộ này xuất phát từ phần tử đầu tiên. 
3. Khi mở rộng phân đoạn để bao gồm một phần tử mới, hãy hợp nhất các bit cần thiết của nó với yêu cầu phân đoạn hiện tại. Điều này thể hiện thực tế là một thao tác đơn lẻ trên phân đoạn phải có khả năng xử lý đồng thời tất cả các phần tử. 
4. Ở mỗi bước, hãy xác minh xem có tồn tại một số$x \le k$có thể đồng thời đáp ứng yêu cầu xóa tất cả các bit trong phân đoạn hiện tại. Nếu không như vậy$x$tồn tại thì chúng ta phải kết thúc đoạn này trước phần tử này. 
5. Khi chúng tôi đóng một phân đoạn, chúng tôi sẽ tăng số lượng thao tác và khởi động lại phân đoạn mới bắt đầu từ vị trí hiện tại. Điều này đảm bảo mỗi hoạt động đều có hiệu quả tối đa. 
6. Tiếp tục cho đến khi toàn bộ mảng được xử lý. Số lượng phân đoạn được hình thành là câu trả lời. 

Lý do phân đoạn tham lam là chính xác là vì bất kỳ hoạt động nào hoạt động trên một khối liền kề và một khi ràng buộc tương thích bit không thành công thì không có phần mở rộng nào trong tương lai của phân đoạn đó có thể khắc phục được mà không vi phạm tính khả thi. Do đó, việc trì hoãn cắt giảm chỉ có thể làm tăng c
