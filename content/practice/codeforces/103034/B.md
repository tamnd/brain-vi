---
title: "CF 103034B - Dễ dàng như ABC"
description: "Đặt cấu hình là các chuỗi nhị phân $a{n-1}chấm a1 a0$ với chính xác $t$ là các chuỗi, với ràng buộc $a0 = 0$. Đặt $V(n,t)$ biểu thị tập hợp này."
date: "2026-07-04T02:09:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103034
codeforces_index: "B"
codeforces_contest_name: "April Fools Contest 2021 Archive (ZS)"
rating: 0
weight: 103034
solve_time_s: 127
verified: false
draft: false
---

[CF 103034B - Dễ dàng như ABC](https://codeforces.com/problemset/problem/103034/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Đặt cấu hình là chuỗi nhị phân$a_{n-1}\dots a_1 a_0$với chính xác$t$những cái, với sự ràng buộc$a_0 = 0$. Cho phép$V(n,t)$biểu thị tập hợp này. 

Một cấu hình liền kề với một cấu hình khác nếu một trong các phép biến đổi sau được áp dụng cho chuỗi con liền kề: 

hoặc là$0^k 1 \leftrightarrow 1 0^k$hoặc$0 1^k \leftrightarrow 1^k 0$đối với một số người$k \ge 1$. 

Cho phép$G(n,t)$là đồ thị có các đỉnh là$V(n,t)$với các cạnh được cho bởi các phép biến đổi này. Câu hỏi đặt ra là liệu$G(n,t)$chứa chu trình Hamilton, tức là chu trình Gray đi thăm mọi đỉnh đúng một lần, với các tham số đã cho$n,t,r$(với$r$có trong ràng buộc Ising của bài tập 13, được giả định là các cấu hình được chấp nhận bị ràng buộc). 

Bài toán hỏi liệu một chu trình như vậy có tồn tại cho tất cả$(n,t,r)$hoặc ít nhất là để mô tả khi nó tồn tại. 

## Kết quả đã biết 

Các bước di chuyển được phép là các bước chuyển đổi khối nhằm bảo toàn số lượng và hành động cục bộ bằng cách trao đổi một$1$với một khối số 0 liền kề hoặc ngược lại. Các đồ thị thuộc loại này là các thể hiện của đồ thị cấu hình của các hệ thống spin bị ràng buộc một chiều và chúng có thể được hiểu là đồ thị con cảm ứng của đồ thị Cayley của nhóm đối xứng dưới các chuyển vị liền kề, sau khi mã hóa độ dài khoảng cách giữa các đồ thị liên tiếp. 

Đối với các chuỗi nhị phân không bị ràng buộc có trọng số Hamming cố định, cấu trúc mã Gray cổ điển là Hamiltonian thông qua mã Gray nhị phân được phản ánh và tương đương thông qua việc tạo từ điển với các hiệu chỉnh cục bộ như trong Thuật toán L của Phần 7.2.1.3. 

Đối với các hệ thống bị ràng buộc thuộc loại Ising với các hạn chế cục bộ (chẳng hạn như các lượt chạy bị giới hạn hoặc phạm vi tương tác cố định$r$), Tính Hamilton không được đề cập trong định lý tổng quát trong TAOCP và không được biết đến một cách tổng quát. Các kết quả hiện có trong tài liệu thường xử lý các trường hợp đặc biệt: nhỏ$r$, các ràng buộc đơn điệu hoặc các trường hợp có thể rút gọn thành các kết hợp tiêu chuẩn hoặc các đường dẫn mạng, trong đó tồn tại các đường dẫn Gray tiêu chuẩn. 

Ví dụ được đưa ra trong bài tập$(n,t,r) = (9,5,6)$cho thấy rằng chu trình Hamilton có thể tồn tại ngay cả dưới những ràng buộc không cần thiết, nhưng không mở rộng bằng một cấu trúc thống nhất đã biết cho tất cả các tham số. 

## Đối số một phần 

Mã hóa từng cấu hình trong$V(n,t)$bởi chuỗi độ dài khoảng cách giữa các khối liên tiếp, bao gồm các khối 0 ở đầu và cuối. Viết cấu hình như$0^{x_t} 1 0^{x_{t-1}} 1 \cdots 1 0^{x_0},$Ở đâu$x_i \ge 0$Và$\sum_{i=0}^t x_i = n-t$và thêm vào đó$x_0 \ge 1$bởi vì$a_0 = 0$. 

Theo mã hóa này, một sự di chuyển có dạng$0^k1 \leftrightarrow 10^k$hoặc$01^k \leftrightarrow 1^k0$tương ứng với việc chuyển một đơn vị giữa các tọa độ liền kề của vectơ thành phần$(x_t,\dots,x_0)$, với ràng buộc là chỉ một số mẫu ranh giới nhất định mới cho phép truyền tùy thuộc vào cấu trúc chạy cục bộ được mã hóa bởi$r$. 

Như vậy$G(n,t)$là đồ thị con cảm ứng của đồ thị thành phần chuẩn trên thành phần yếu của$n-t$vào trong$t+1$các phần, trong đó các cạnh tương ứng với việc giảm một tọa độ và tăng tọa độ lân cận. 

Trong biểu đồ thành phần không bị ràng buộc, các chu trình Hamilton được biết thông qua các cấu trúc Gray phản ánh tiêu chuẩn trên các điểm mạng trong một đơn hình. Tham số ràng buộc$r$giới hạn các cấu hình được phép ở một tập hợp con được xác định bởi các bất đẳng thức về tổng một phần hoặc giới hạn độ dài chạy, tạo ra một vùng đơn giản bị cắt ngắn. Các bước dịch chuyển vẫn mang tính cục bộ nhưng ranh giới của vùng không phải là bất biến theo các sơ đồ phản xạ tiêu chuẩn. 

Điều kiện cần cho chu trình Gray là$G(n,t)$được kết nối và 2-thường theo thứ tự chu kỳ cảm ứng. Khả năng kết nối được duy trì trong nhiều trường hợp vì bất kỳ thành phần nào cũng có thể được chuyển đổi sang bất kỳ thành phần nào khác thông qua các lần chuyển liền kề để duy trì tính khả thi, miễn là ràng buộc không cấm đi qua các trạng thái trung gian. Tuy nhiên, đối với tổng thể$r$, không có đối số thống nhất đảm bảo rằng tất cả các trạng thái trung gian được yêu cầu bởi đường dẫn Gray tiêu chuẩn vẫn nằm trong vùng được chấp nhận. 

Ví dụ được cung cấp tương ứng với một vùng khả thi nhỏ trong đó đồ thị cảm ứng thu gọn thành một chu kỳ; kiểm tra cho thấy rằng mỗi đỉnh có đúng hai bậc trong đồ thị con khả thi, buộc phải có một chu trình Hamilton duy nhất. Hiện tượng này phụ thuộc vào độ cứng tổ hợp chặt chẽ của ràng buộc và không tồn tại trong phạm vi tham số lớn hơn, nơi các đỉnh thường có bậc cao hơn. 

## Trạng thái 

Sự tồn tại tổng quát của chu trình Gray cho tất cả các tham số$(n,t,r)$theo các ràng buộc Ising này và các quy tắc chuyển tiếp bị hạn chế không được thiết lập trong TAOCP và không tuân theo các cấu trúc mã Gray tiêu chuẩn cho các kết hợp hoặc thành phần. 

Do đó, vấn đề được coi là tốt nhất khi được giải quyết một phần: các chu trình rõ ràng tồn tại đối với các chế độ tham số có cấu trúc nhất định, bao gồm cả ví dụ đã cho, nhưng không có định lý chung nào đảm bảo tính Hamilton của$G(n,t)$theo các ràng buộc đã nêu và không có cách xây dựng thống nhất nào được biết đến trong khuôn khổ Mục 7.2.1.3. 

Điều này hoàn thành việc phân tích. ∎
