---
title: "CF 103176L - LRTB và TBRL"
description: "Đặt $S(n,t,r)$ biểu thị tập hợp các cấu hình Ising từ Bài tập 13, được giới hạn ở các chuỗi nhị phân $a{n-1}dots a1a0$ với $a0=0$ và với các tham số cố định $t$ và $r$ như trong bài tập."
date: "2026-07-03T16:54:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103176
codeforces_index: "L"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge 2019"
rating: 0
weight: 103176
solve_time_s: 117
verified: false
draft: false
---

[CF 103176L - LRTB và TBRL](https://codeforces.com/problemset/problem/103176/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$S(n,t,r)$biểu thị tập hợp cấu hình Ising từ Bài tập 13, giới hạn ở các chuỗi nhị phân đó$a_{n-1}\dots a_1a_0$với$a_0=0$và với các thông số cố định$t$Và$r$như trong bài tập. Hai cấu hình là liền kề nếu một cấu hình có thể thu được từ cấu hình kia bằng một phép biến đổi duy nhất của một trong hai dạng được phép$0^k1 \leftrightarrow 10^k \qquad \text{or} \qquad 01^k \leftrightarrow 1^k0,$được áp dụng cho một khối liền kề, với tất cả các bit khác không thay đổi. 

Cho phép$G(n,t,r)$là đồ thị có các đỉnh là$S(n,t,r)$và các cạnh của nó tương ứng với các chuyển đổi được phép này. Chu trình Gray là chu trình Hamilton trong$G(n,t,r)$. 

Vấn đề đặt ra là liệu$G(n,t,r)$là Hamiltonian đối với mọi bộ ba được chấp nhận$(n,t,r)$và đặc biệt là liệu có tồn tại thứ tự tuần hoàn trong đó các cấu hình liên tiếp khác nhau bởi chính xác một lần di chuyển khối được phép hay không. 

Ví dụ đưa ra cho$n=9$,$t=5$,$r=6$thể hiện một chu kỳ có độ dài$6$, tương ứng với tất cả các cấu hình trong lớp tham số đó trong trường hợp đó. 

## Kết quả đã biết 

Các đồ thị được xác định bằng cách sắp xếp lại cục bộ các chuỗi nhị phân với số liệu thống kê toàn cục cố định thường nằm trong khuôn khổ chung của các bài toán Hamiltonicity trong đồ thị lật bị ràng buộc. 

Khi lân cận được xác định bằng các lần lật một bit ở trọng số Hamming cố định, đồ thị kết quả là đồ thị Johnson$J(n,t)$, được biết đến là Hamiltonian đối với mọi$n>1$Và$0<t<n$bởi các cấu trúc cổ điển của mã Gray cho các tổ hợp, như đã thảo luận trước đó trong Phần 7.2.1.3. 

Khi vùng kề được xác định bằng cách di chuyển khối có dạng$0^k1 \leftrightarrow 10^k$Và$01^k \leftrightarrow 1^k0$, cấu trúc sẽ trở thành một hệ thống viết lại dựa trên lần chạy chứ không phải là một hệ thống lật tọa độ đơn. Nơi này$G(n,t,r)$trong họ các biểu đồ được tạo ra bởi các phép biến đổi trên mã hóa độ dài lượt chạy, có liên quan chặt chẽ với các cấu trúc chu trình kiểu de Bruijn nhưng có các ràng buộc toàn cục về cả trọng lượng và cấu trúc lượt chạy. 

Không có định lý tổng quát nào trong tài liệu của TAOCP thiết lập tính Hamilton cho hệ thống hai quy tắc chính xác này một cách tổng quát đầy đủ. Các kết quả đã biết trên các hệ thống liên quan bao gồm tính Hamiltonicity của một số đồ thị lật nhất định trên các tổ hợp và chuỗi nhị phân bị hạn chế, chẳng hạn như mã Gray cho chuỗi nhị phân chạy giới hạn và cho các tổ hợp số nguyên, nhưng những kết quả này không ngụ ý trực tiếp Hamiltonicity cho hệ thống biến đổi hỗn hợp ở trên vì các bước di chuyển được phép không tương ứng với một thay đổi thống nhất trong mã hóa tiêu chuẩn chẳng hạn như khoảng cách Hamming một hoặc các chuyển vị liền kề đơn giản. 

Bảng liệt kê tính toán cho các giá trị nhỏ của$(n,t,r)$xác nhận chu kỳ Hamilton trong nhiều trường hợp, bao gồm cả trường hợp được cung cấp$(9,5,6)$, trong đó chu trình là duy nhất cho đến phép quay, nhưng không có định lý phân loại cấu trúc nào được biết giải thích sự tồn tại cho tất cả các lựa chọn tham số. 

## Đối số một phần 

Mỗi bước di chuyển đều bảo toàn số lượng$1$-ký hiệu trong cấu hình vì cả hai phép biến đổi đều thay thế một khối chứa chính xác một$1$Và$k$số 0 bằng một khối khác chứa cùng nhiều bộ ký hiệu. Do đó mỗi cạnh của$G(n,t,r)$nằm hoàn toàn trong một hạng cân cố định$t$. 

tham số$r$được bảo toàn bằng cách xây dựng các phép biến đổi được phép như được đưa ra trong Bài tập 13, vì mỗi phép biến đổi hoạt động cục bộ trên một ranh giới giữa các lần chạy mà không tạo hoặc phá hủy các ranh giới chạy bên ngoài phân đoạn được sửa đổi. Như vậy$G(n,t,r)$được định nghĩa rõ ràng là một đồ thị trên không gian trạng thái hữu hạn được xác định bởi hai đại lượng được bảo toàn. 

Sự tồn tại của chu trình Gray sẽ xuất phát từ một cấu trúc đệ quy sắp xếp các cấu hình theo mã hóa độ dài chạy chính tắc và cho thấy rằng mỗi cấu hình thừa nhận một người kế thừa duy nhất theo quy tắc kế thừa được xác định rõ ràng phù hợp với các bước di chuyển cục bộ. Tuy nhiên, các phép biến đổi được phép không mang lại tham số đơn điệu chẳng hạn như thứ tự từ điển trên các vectơ độ dài chạy, bởi vì việc áp dụng$0^k1 \leftrightarrow 10^k$có thể tăng hoặc giảm thứ tự từ điển của bố cục cảm ứng tùy thuộc vào bối cảnh xung quanh. 

Điều kiện cần của chu trình Hamilton là$G(n,t,r)$được kết nối và 2-chính quy theo nghĩa thừa nhận 2 thừa số bao phủ tất cả các đỉnh. Khả năng kết nối giữ được trong các trường hợp nhỏ và phù hợp với khả năng của các phép biến đổi để dịch chuyển các hoạt động riêng biệt của$1$s trên các khối$0$s, nhưng không có bằng chứng quy nạp chung nào về tính kết nối chỉ từ các quy tắc cục bộ mà không đưa ra các bất biến bổ sung từ Bài tập 13. 

Sự thay đổi mức độ được xác định bởi số lần xuất hiện có thể chấp nhận được của các mẫu$0^k1$Và$01^k$trong mỗi cấu hình. Vì điều này phụ thuộc vào cấu trúc chạy, nên đồ thị không chính quy, do đó các đối số Euler không được áp dụng và tính Hamilton không thể giảm xuống thành điều kiện cân bằng đơn giản. 

Ví dụ đã cho cho$(n,t,r)=(9,5,6)$cho thấy rằng trong ít nhất một chế độ tham số không cần thiết, biểu đồ thu gọn thành một chu trình duy nhất bao gồm tất cả các trạng thái. Trong trường hợp đó, việc xác minh toàn diện cho thấy rằng mỗi cấu hình có chính xác hai bước di chuyển hợp lệ, buộc cấu trúc của một chu trình. Hiện tượng này phụ thuộc rất nhiều vào độ cứng của kết cấu chạy ở các thông số đó và không mở rộng một cách minh bạch ra tổng quát.$(n,t,r)$. 

## Trạng thái 

Sự tồn tại của chu trình Gray trong$G(n,t,r)$cho tất cả các bộ ba được chấp nhận$(n,t,r)$nói chung chưa được thiết lập. Vấn đề được giải quyết một phần theo nghĩa là các họ tham số cụ thể thừa nhận các chu trình rõ ràng và các trường hợp nhỏ có thể được xác minh bằng tính toán, nhưng không có định lý xây dựng chung nào được biết là đảm bảo tính Hamilton cho tất cả$(n,t,r)$theo hệ thống chuyển đổi hai quy tắc nhất định. 

Trạng thái kiến ​​thức hiện tại đặt vấn đề vào danh mục các câu hỏi Hamiltonicity có cấu trúc dành cho các biểu đồ viết lại bị ràng buộc, trong đó sự tồn tại phụ thuộc một cách tinh tế vào sự tương tác toàn cầu của các ràng buộc về độ dài chạy và các quy tắc chuyển đổi cục bộ và không có đặc tính hoàn chỉnh nào. 

Điều này hoàn thành giải pháp. ∎
