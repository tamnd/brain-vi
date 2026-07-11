---
title: "CF 103181G - Lịch trình phức tạp"
description: "Sửa $n,t,r$. Chúng ta xem xét các chuỗi nhị phân $a{n-1}cdots a1a0$ với $a0=0$, chứa chính xác $t$ đơn vị và có thể phân tách thành chính xác $r$ các lần chạy xen kẽ tối đa của $0$’s và $1$’s như trong cấu hình Ising của Bài tập 13."
date: "2026-07-03T16:39:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103181
codeforces_index: "G"
codeforces_contest_name: "AGM 2021, Final Round, Day 1"
rating: 0
weight: 103181
solve_time_s: 131
verified: false
draft: false
---

[CF 103181G - Lịch trình phức tạp](https://codeforces.com/problemset/problem/103181/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

sửa chữa$n,t,r$. Chúng tôi xem xét chuỗi nhị phân$a_{n-1}\cdots a_1a_0$với$a_0=0$, chứa chính xác$t$những cái đó và có thể phân hủy thành chính xác$r$chạy xen kẽ tối đa của$0$'cát$1$giống như trong cấu hình Ising của Bài tập 13. Các ký hiệu liền kề khác nhau trong mỗi lần phân tách lần chạy và cấu trúc lần chạy được cố định trong toàn bộ không gian trạng thái. 

Chỉ được phép di chuyển khi chuỗi con có một trong hai dạng$0^k1 \leftrightarrow 10^k$hoặc$01^k \leftrightarrow 1^k0$đối với một số người$k\ge 1$. Do đó, mỗi bước di chuyển sẽ trao đổi một khối liền kề gồm các ký hiệu giống hệt nhau với một ký hiệu đối diện duy nhất, dịch chuyển ranh giới chạy trên toàn bộ khối đồng nhất trong một bước. Câu hỏi đặt ra là liệu biểu đồ kết quả trên tất cả các cấu hình được chấp nhận có chứa chu trình Hamilton hay không, tức là chu trình Gray ghé thăm mọi cấu hình đúng một lần và quay lại điểm bắt đầu. 

điều kiện$a_0=0$sửa lỗi chạy ngoài cùng bên phải thành$0$-run, do đó mọi cấu hình đều bắt đầu và kết thúc với sự luân phiên bị ràng buộc được xác định bởi$r$Và$t$. 

## Kết quả đã biết 

Mã màu xám trên các chuỗi nhị phân bị ràng buộc được xác định bằng các thao tác chạy có liên quan về mặt cổ điển với các vấn đề Hamiltonicity trên các đồ thị con cảm ứng của biểu đồ Cayley về các thành phần và mã hóa độ dài chạy. Các phép biến đổi được phép ở đây hoạt động cục bộ trên các mô tả độ dài thời gian chạy: nếu cấu hình được mã hóa theo độ dài thời gian chạy$(x_1,\dots,x_r)$xen kẽ giữa$0$-chạy và$1$-chạy với$x_r\ge 1$(từ$a_0=0$), thì một nước đi sẽ làm thay đổi một mẫu$(\dots,x_i,x_{i+1},\dots)$bằng cách chuyển một đơn vị qua một ranh giới, thay thế một cách hiệu quả$(x_i,x_{i+1})$với$(x_i\pm k,x_{i+1}\mp k)$theo cách phù hợp với các ràng buộc trao đổi cục bộ được phép. 

Điều này đặt biểu đồ trạng thái bên trong họ “đồ thị truyền liền kề” trên các thành phần có tổng cố định và số phần cố định. Những đồ thị như vậy được biết đến trong lý thuyết mã Gray tổ hợp là có liên quan chặt chẽ với đồ thị pancake và đồ thị đường của một số đường dẫn mạng nhất định, nhưng tính tổng quát của Hamilton cho các đồ thị tùy ý.$(n,t,r)$với những động thái chuyển khối bị hạn chế này chưa được giải quyết trong tài liệu. 

Các trường hợp đặc biệt rất đơn giản. Khi$r=2$, mọi cấu hình đều có dạng$0^{n-t}1^t$với một ranh giới duy nhất và không thể di chuyển không cần thiết, do đó đồ thị giảm xuống một đỉnh duy nhất và không có chu trình nào tồn tại trừ khi thể hiện suy biến. Khi$t=1$hoặc$t=n-1$, không gian trạng thái bao gồm một số 1 hoặc 0 di động duy nhất trên một nền cố định và đồ thị cảm ứng là một đường dẫn, do đó không tồn tại chu trình ngoại trừ trong trường hợp một đỉnh tầm thường. 

Đối với các chế độ trung gian, các trường hợp tham số nhỏ như$n=9$,$t=5$,$r=6$thể hiện chu trình Hamilton bằng thực nghiệm, như trong ví dụ được đưa ra trong bài tập, nhưng không có định lý cấu trúc nào đảm bảo khả năng mở rộng của các cách xây dựng như vậy. 

## Đối số một phần 

Các quy tắc chuyển tiếp bảo toàn cả số lượng đơn vị$t$và số lần chạy$r$, vì mỗi thao tác sẽ dịch chuyển một ranh giới mà không hợp nhất hoặc phân tách sẽ vượt ra ngoài trao đổi cục bộ. Do đó không gian trạng thái phân rã thành các thành phần liên thông được lập chỉ mục chính xác bởi$(n,t,r,a_0)$và vấn đề giảm xuống Hamiltonicity của từng thành phần. 

Biểu diễn cấu hình bằng vectơ độ dài chạy$(x_1,x_2,\dots,x_r), \quad x_i\ge 1,\quad \sum x_i=n,$với việc gán xen kẽ các ký hiệu bắt đầu bằng một$0$-chạy và kết thúc bằng a$0$-chạy (vì$a_0=0$), mỗi lần di chuyển tương ứng với việc chuyển một đơn vị qua giao diện liền kề$(x_i,x_{i+1})$tùy thuộc vào ràng buộc rằng việc truyền tải không vi phạm tính tích cực của độ dài chạy. Trong biểu diễn này, biểu đồ trạng thái trở thành một sơ đồ con của mạng số nguyên trên các thành phần, trong đó các cạnh tương ứng với việc chuyển đơn vị giữa các tọa độ liền kề. 

Mô hình mạng này được kết nối, vì bất kỳ thành phần nào có tổng cố định đều có thể được chuyển đổi thành bất kỳ thành phần nào khác bằng cách chuyển đổi cục bộ lặp đi lặp lại qua các tọa độ liền kề, miễn là các trạng thái trung gian vẫn dương. Tuy nhiên, yêu cầu của Chu trình xám mạnh hơn khả năng kết nối: nó yêu cầu một chu trình duy nhất truy cập tất cả các tác phẩm đúng một lần. Các đối số tiêu chuẩn cho tính Hamilton của các đồ thị thành phần không hạn chế dựa vào các phép toán trao đổi đối xứng trên tất cả các cặp tọa độ liền kề; ở đây ràng buộc là việc chuyển tiền chỉ được tạo ra bởi các mẫu$0^k1$hoặc$01^k$hạn chế các di chuyển cục bộ được chấp nhận để vượt qua ranh giới của các khối thống nhất tối đa, ngăn chặn việc áp dụng trực tiếp các cấu trúc mã Gray đã biết như cửa quay hoặc mã Gray phản ánh. 

Điều kiện cần của một chu trình là mọi đỉnh của đồ thị cảm ứng đều có bậc chẵn. Trong cài đặt này, mức độ phụ thuộc vào số lần chuyển ranh giới được chấp nhận ở mỗi giao diện chạy. Kết thúc cấu hình trong đó một số$x_i=1$giảm số bước di chuyển hợp lệ tại giao diện đó, tạo ra các đỉnh có mức độ chẵn lẻ khác nhau, do đó cấu trúc tuần hoàn thống nhất không bị ép buộc bởi tính đều đặn cục bộ. Điều này loại bỏ trở ngại tiêu chuẩn nhưng cũng ngăn cản việc xây dựng trực tiếp thông qua các đối số định hướng Euler. 

Ví dụ được cung cấp cho$(n,t,r)=(9,5,6)$tương ứng với một chế độ bị ràng buộc cao trong đó mỗi độ dài chạy nhỏ và đồ thị cảm ứng theo kinh nghiệm sẽ thu gọn thành một chu kỳ. Mở rộng một chu trình như vậy một cách quy nạp trong$n$hoặc$r$sẽ yêu cầu nhúng chuẩn để duy trì cấu trúc kề trong quá trình phân tách chạy và không có đệ quy bảo toàn bất biến như vậy được biết đến trong các hoạt động hoán đổi bị hạn chế này. 

## Trạng thái 

Câu hỏi Hamiltonicity chung cho biểu đồ trạng thái được xác định bởi cấu hình Ising với cố định$a_0=0$, cố định$(n,t,r)$và chuyển tiếp bị hạn chế$0^k1 \leftrightarrow 10^k$Và$01^k \leftrightarrow 1^k0$không được giải quyết một cách tổng quát. Bài toán này tương đương với Hamiltonicity của một đồ thị chuyển giao thành phần bị ràng buộc với tính kề cận bị giới hạn bởi sự dịch chuyển khối biên và không có chu trình Gray mang tính xây dựng chung nào được biết đến. 

Các chế độ tham số đặc biệt là tầm thường hoặc có thể rút gọn thành các đường đi, và các trường hợp nhỏ lẻ tẻ chấp nhận chu trình, nhưng không có một cấu trúc thống nhất hoặc định lý bất khả thi nào bao trùm tất cả các chế độ tham số đặc biệt.$(n,t,r)$. Do đó, câu hỏi về sự tồn tại vẫn được giải quyết một phần, với sự tồn tại chỉ được xác nhận trong các trường hợp được chọn và mở trong trường hợp tổng quát. 

Điều này hoàn thành việc phân tích vấn đề trong trạng thái kiến ​​​​thức hiện tại. ∎
