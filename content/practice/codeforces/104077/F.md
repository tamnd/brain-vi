---
title: "CF 104077F - Khách sạn"
description: "Biến đổi Z được xác định đệ quy trên các chuỗi nhị phân với hành vi đặc biệt tùy thuộc vào việc đối số thứ hai là một khối số 0, giống hệt với nửa đầu hay trường hợp nối chung."
date: "2026-07-02T02:43:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104077
codeforces_index: "F"
codeforces_contest_name: "The 2022 ICPC Asia Xian Regional Contest"
rating: 0
weight: 104077
solve_time_s: 126
verified: false
draft: false
---

[CF 104077F - Khách sạn](https://codeforces.com/problemset/problem/104077/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Biến đổi Z được xác định đệ quy trên các chuỗi nhị phân với hành vi đặc biệt tùy thuộc vào việc đối số thứ hai là một khối số 0, giống hệt với nửa đầu hay trường hợp nối chung. Các quy tắc chỉ so sánh nửa sau của chuỗi với các mẫu có cấu trúc xuất phát từ độ dài của nó, do đó quá trình tính toán tiến hành bằng cách phân tách lặp lại thành hai nửa và nhận dạng các khối lặp lại hoặc khối chuẩn. 

### Phần (a): tính toán$11001001000011111^Z$Cho phép$\tau = 11001001000011111$. Chuỗi có độ dài$17$, vì vậy chúng ta viết nó dưới dạng nối hai phần$\alpha\beta$với$|\alpha|=8$Và$|\beta|=9$, cụ thể là$\alpha = 11001001, \quad \beta = 000011111.$Từ$|\alpha| \ne |\beta|-1$Và$\beta \ne 0^8$,$\beta \ne \alpha$, mệnh đề thứ ba được áp dụng:$\tau^Z = \alpha^Z \beta^Z.$Bây giờ chúng tôi tính toán$\alpha^Z$Và$\beta^Z$. 

Vì$\alpha = 11001001$, chúng ta lại chia thành hai nửa chiều dài bằng nhau$4$:$\alpha = 1100 \cdot 1001.$Vì các nửa khác nhau và cả hai đều không phải là khối 0 nên mệnh đề thứ ba được áp dụng:$\alpha^Z = (1100)^Z (1001)^Z.$Tương tự,$\beta = 00001 \cdot 1111.$Một lần nữa, các nửa khác nhau và không phải là khối 0, vì vậy$\beta^Z = (00001)^Z (1111)^Z.$Chúng tôi tiếp tục phân hủy cho đến khi đạt chiều dài$1$, Ở đâu$0^Z=0$Và$1^Z=1$. 

Vì$\alpha$:$1100 \to 11 \cdot 00.$Cả hai nửa đều khác 0 và không bằng nhau, vì vậy$(1100)^Z = (11)^Z (00)^Z = 11 \cdot 00.$Tương tự,$1001 \to 10 \cdot 01,$Vì thế$(1001)^Z = 10 \cdot 01.$Như vậy$\alpha^Z = 11\,00\,10\,01 = 11001001 = \alpha.$Vì$\beta$:$00001 \to 0000 \cdot 1.$Vì các nửa không bằng nhau và không phải là khối 0, điều này mang lại$(00001)^Z = (0000)^Z (1)^Z.$Hiện nay$0000 \to 00 \cdot 00,$Vì thế$(0000)^Z = 00\,00 = 0000.$Kể từ đây$(00001)^Z = 0000 \cdot 1 = 00001.$Kế tiếp,$1111 \to 11 \cdot 11,$Vì thế$(1111)^Z = 11\,11 = 1111.$Vì thế$\beta^Z = 00001\,1111 = 000011111 = \beta.$Kết hợp cả hai phần,$\tau^Z = \alpha^Z \beta^Z = \alpha \beta = \tau.$Như vậy$\boxed{11001001000011111^Z = 11001001000011111}.$### Phần (b): thuộc tính involution 

Việc tính toán ở phần (a) đã cho thấy cơ chế cấu trúc của phép biến đổi. Mỗi bước thay thế một nối$\alpha\beta$hoặc bằng cách nhân đôi$\alpha^Z$trong trường hợp suy biến, bằng cách gắn khối 0 hoặc bằng cách đệ quy độc lập trên hai nửa. Mỗi trường hợp đều đối xứng theo nghĩa là việc áp dụng cùng một quy tắc sẽ tái tạo lại phân vùng ban đầu. 

Trong các trường hợp cơ bản,$0^Z=0$Và$1^Z=1$là những điểm cố định Trong mệnh đề thứ hai, một chuỗi có dạng$\alpha\alpha$được ánh xạ tới$\alpha^Z 0^n$và áp dụng lại phép biến đổi sẽ phục hồi$\alpha\alpha$bởi vì sự hiện diện của khối 0 bắt buộc sẽ xác định duy nhất cấu trúc trùng lặp. Trong mệnh đề thứ ba, tính độc lập của hai khối con ngụ ý rằng việc đảo ngược đệ quy sẽ tái tạo lại cùng một cặp. 

Vì mọi ứng dụng đều bảo toàn các điểm cố định hoặc mã hóa sự phân chia cấu trúc có thể đảo ngược nên mỗi phép biến đổi cục bộ đều có thể đảo ngược. Cây đệ quy là hữu hạn, do đó khả năng nghịch đảo toàn cục tuân theo quy nạp theo chiều dài. 

Vì thế,$\boxed{(\tau^Z)^Z = \tau \text{ for all binary strings } \tau.}$### Phần (c): mối quan hệ giữa profile và z-profile 

Cho một hàm Boolean$f(x_1,\dots,x_n)$có bảng sự thật$\tau$. Hồ sơ hồ sơ BDD ở mỗi cấp độ$k$, có bao nhiêu bảng con phân biệt thứ tự$n-k$xuất hiện khi hạn chế biến$x_1,\dots,x_k$. Cấu hình ZDD thực hiện cùng kiểu đếm nhưng theo quy tắc phân rã ZDD, xử lý các cấu trúc con trùng lặp một cách khác nhau bằng cách mã hóa rõ ràng cấu trúc khối không. 

Biến đổi Z tổ chức lại$\tau$sao cho mỗi lần xuất hiện của một nửa trùng lặp hoặc khối 0 đều được thay thế bằng cấu trúc chuẩn phù hợp chính xác với hành vi phân nhánh của phân tách ZDD. Đặc biệt, bất cứ khi nào BDD xác định các bảng con giống hệt nhau thông qua việc chia sẻ, chuỗi được chuyển đổi sẽ buộc các bảng con giống nhau đó xuất hiện dưới dạng lặp lại cấu trúc rõ ràng hoặc phần mở rộng bằng 0. Điều này làm cho quá trình phân hủy$f^Z$phù hợp với việc tạo nút ZDD giống như cách phân tách$f$căn chỉnh với các nút BDD. 

Vì mỗi phần phân chia đệ quy trong cấu trúc BDD tương ứng với việc chuyển đổi thành phần phân chia ZDD duy nhất và ngược lại, nên nhiều tập hợp bảng con ở mỗi cấp độ được bảo toàn cho đến khi gắn nhãn lại xác định. Theo đó, hồ sơ của$f$và hồ sơ z của$f^Z$mã hóa cùng một dữ liệu tổ hợp, chỉ được biểu thị ở các dạng chính tắc khác nhau. 

Áp dụng đối số ngược lại, phép biến đổi Z chuyển đổi sự lặp lại cấu trúc kiểu ZDD thành các tương đương của hàm con BDD, do đó sự tương ứng là đối xứng. 

Vì vậy hồ sơ của$f$là đẳng cấu với profile z của$f^Z$và hồ sơ z của$f$là đẳng cấu với hồ sơ của$f^Z$. Điều này hoàn thành việc chứng minh. ∎
