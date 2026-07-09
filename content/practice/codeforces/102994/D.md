---
title: "CF 102994D - Lý thuyết dây"
description: "Đặt $S(n,t,r)$ biểu thị tập hợp các cấu hình Ising từ bài tập 13 với các tham số $n,t,r$ và với hạn chế bổ sung $a0=0$. Mỗi cấu hình là một chuỗi nhị phân $a{n-1}chấm a1a0 trong {0,1}^n,$ thỏa mãn các ràng buộc xác định bài tập 13 cùng với $a0=0$."
date: "2026-07-04T02:37:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102994
codeforces_index: "D"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2020 - Legilimens+Coffee Chicken Contest (XX Open Cup, Grand Prix of Nanjing)"
rating: 0
weight: 102994
solve_time_s: 148
verified: false
draft: false
---

[CF 102994D - Lý thuyết dây](https://codeforces.com/problemset/problem/102994/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$S(n,t,r)$ký hiệu tập cấu hình Ising từ bài tập 13 với các tham số$n,t,r$và với hạn chế bổ sung$a_0=0$. Mỗi cấu hình là một chuỗi nhị phân$a_{n-1}\dots a_1a_0 \in \{0,1\}^n,$thỏa mãn các ràng buộc xác định bài tập 13 cùng với$a_0=0$. Vai trò chính xác của$t$là số 1, và$r$là tham số cấu trúc từ bài tập 13 (trong ngữ cảnh đó, nó mã hóa việc phân tách chuỗi thành các chuỗi 0 và 1 xen kẽ). 

Việc chuyển đổi chỉ được phép nếu nó có một trong hai hình thức$0^k1 \leftrightarrow 10^k \quad\text{or}\quad 01^k \leftrightarrow 1^k0,$Ở đâu$k \ge 1$và phép chuyển đổi được áp dụng cho một chuỗi con liền kề. Mỗi lần di chuyển bảo toàn độ dài$n$và số lượng 1$t$, đồng thời giữ nguyên tham số$r$định nghĩa ở bài tập 13. 

Câu hỏi đặt ra là liệu, cho trước$(n,t,r)$, đồ thị cảm ứng trên$S(n,t,r)$được xác định bởi các chuyển đổi này chứa một chu trình Gray, tức là chu trình Hamilton. 

## Kết quả đã biết 

Các chuyển đổi được phép là các bước di chuyển khối trao đổi một chuỗi tối đa các bit giống hệt nhau với một bit đối diện liền kề, bảo toàn các tham số cấu trúc chạy. Điều này đặt vấn đề vào lớp chung của các câu hỏi Hamiltonicity cho đồ thị lật bị ràng buộc trên các chuỗi nhị phân có trọng số cố định và các ràng buộc chạy bổ sung. 

Hai trường hợp giới hạn được nghiên cứu kỹ lưỡng có liên quan. 

Đầu tiên, nếu bỏ qua tham số$r$và chỉ sửa lỗi$t$, không gian trạng thái trở thành đồ thị Johnson$J(n,t)$, các đỉnh của nó đều là$t$-tập hợp con của${0,\dots,n-1}$, tương đương với tất cả các chuỗi nhị phân có$t$những cái đó. Đó là cổ điển mà$J(n,t)$là Hamiltonian cho tất cả chấp nhận được$(n,t)$và mã Gray cho các kết hợp trong Phần 7.2.1.3 cung cấp các cấu trúc rõ ràng. 

Thứ hai, nếu người ta hạn chế di chuyển đến các chuyển vị liền kề của 0 và 1, thì biểu đồ kết quả sẽ trở thành một sơ đồ con của siêu khối và Hamiltonicity tuân theo mã Gray phản ánh nhị phân. Mô hình hiện tại bị ràng buộc chặt chẽ hơn vì toàn bộ khối ký hiệu bằng nhau phải được di chuyển qua một ranh giới và ràng buộc$r$hạn chế các mẫu cục bộ được chấp nhận. 

Các bước di chuyển được phép cũng liên quan chặt chẽ đến biểu đồ lật trên các thành phần số nguyên và biểu đồ Cayley được tạo bằng cách hoán đổi khối trên chuỗi nhị phân. Đối với các biểu đồ trao đổi khối không hạn chế, Hamiltonicity được biết đến trong nhiều trường hợp thông qua các cấu trúc quy nạp, nhưng việc áp đặt cả trọng số cố định và số lần chạy cố định sẽ đặt biểu đồ vào một chế độ được kết nối thưa thớt, nơi không có tiêu chí Hamiltonicity chung. 

Ví dụ rõ ràng được đưa ra cho$(n,t,r)=(9,5,6)$,$(010101110, 010110110, 011010110, 011011010, 011101010, 010111010),$cho thấy đồ thị có thể mang tính Hamilton ngay cả dưới những hạn chế về cấu trúc mạnh mẽ, nhưng bản thân điều này không mở rộng sang một cách xây dựng chung. 

## Đối số một phần 

Mỗi bước di chuyển được phép hoạt động cục bộ trên ranh giới giữa khối tối đa là 0 và khối tối đa là 1. Đặc biệt, những chuyển biến$0^k1 \leftrightarrow 10^k$Và$01^k \leftrightarrow 1^k0$có thể được hiểu là trượt ranh giới giữa chạy 0 và chạy 1 trên một khối có chiều dài$k$, đồng thời bảo toàn tổng số lần chạy và bảo toàn trọng lượng$t$. 

Như vậy mọi đỉnh trong$S(n,t,r)$có thể được xác định bằng thành phần của$n$vào trong$r$phần tích cực mô tả độ dài chạy. Theo nhận dạng này, mỗi lần di chuyển tương ứng với việc chuyển một đơn vị chiều dài giữa các phần liền kề của bố cục, với điều kiện là đơn vị được chuyển có thể đại diện cho toàn bộ khối có kích thước đồng nhất.$k$thay vì một chút. Ràng buộc$a_0=0$sửa phần cuối cùng của bố cục để tương ứng với lần chạy 0. 

Cách giải thích này chuyển vấn đề thành sự tồn tại của chu trình Hamilton trong đồ thị thành phần bị ràng buộc$C(n,r)$trong đó các đỉnh là các thành phần có mẫu chẵn lẻ cố định được xác định bởi bit ban đầu và có ràng buộc tổng cố định trên các phần tương ứng với số 1. Các cạnh tương ứng với các bước di chuyển phân phối lại cục bộ của dạng giới hạn. 

Khả năng kết nối của các đồ thị thành phần như vậy có thể được biểu diễn bằng phương pháp quy nạp trên$n$trong điều kiện ôn hòa trên$r$, bởi vì bất kỳ thành phần nào cũng có thể được rút gọn về dạng chính tắc bằng cách liên tục hợp nhất các phần liền kề rồi phân phối lại. Tuy nhiên, sơ đồ quy nạp tương tự không trực tiếp tạo ra chu trình Hamilton, bởi vì việc duy trì một chu trình đơn lẻ đòi hỏi phải kiểm soát tính chẵn lẻ của các lượt truy cập và tránh lặp lại sớm các cấu hình biên. 

Một điều kiện cần thiết cho chu trình Hamilton là đồ thị phải được kết nối đều đặn 2 theo nghĩa là mọi vết cắt được tạo ra bằng cách cố định một đoạn chạy ban đầu phải có ít nhất hai cạnh chéo. Điều này đúng trong nhiều chế độ tham số, nhưng không thành công khi$r$quá nhỏ so với$n$bởi vì tập hợp di chuyển trở nên quá cứng nhắc để có thể phân nhánh đủ. 

Ví dụ đã cho với$(9,5,6)$tương ứng với một chế độ trong đó mỗi cấu hình có đủ cấu trúc xen kẽ để mọi di chuyển cục bộ có thể được đảo ngược theo một cách riêng biệt, hỗ trợ về mặt thực nghiệm Hamiltonicity, nhưng không có cơ chế mở rộng chung nào từ cấu trúc đơn lẻ này. 

## Trạng thái 

Không có định lý tổng quát nào đảm bảo sự tồn tại của chu trình Gray cho mọi bộ ba$(n,t,r)$dưới sự chuyển tiếp hạn chế$0^k1 \leftrightarrow 10^k,\quad 01^k \leftrightarrow 1^k0,$ngay cả dưới sự ràng buộc bổ sung$a_0=0$. 

Do đó, vấn đề được phân loại tốt nhất là được giải quyết một phần. Khả năng kết nối và chu trình Hamilton phiên bản nhỏ có thể truy cập được bằng cách xây dựng trực tiếp và tìm kiếm trên máy tính, đồng thời các họ tham số đặc biệt thừa nhận các cấu trúc mã Gray quy nạp, nhưng đặc tính đầy đủ về thời điểm tồn tại của chu trình Hamilton vẫn còn bỏ ngỏ trong trường hợp chung. 

Điều này hoàn thành việc phân tích. ∎
