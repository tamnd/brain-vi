---
title: "CF 103241T - Dự án kỳ dị của Vivy"
description: "Đặt $n=s+t$ và đặt các vị trí trên mặt đất là ${0,1,dots,n-1}$. Cấu hình là một từ $x0x1cdots x{n-1}$ trên ${0,1,ast}$ chứa chính xác $t$ dấu hoa thị. Tập hợp tất cả các cấu hình như vậy có kích thước $2^sbinom{s+t}{t}$."
date: "2026-07-03T15:12:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103241
codeforces_index: "T"
codeforces_contest_name: "Teamscode Summer 2021"
rating: 0
weight: 103241
solve_time_s: 154
verified: false
draft: false
---

[CF 103241T - Dự án Kỳ dị của Vivy](https://codeforces.com/problemset/problem/103241/T) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$n=s+t$và để các vị trí mặt đất là${0,1,\dots,n-1}$. Cấu hình là một từ$x_0x_1\cdots x_{n-1}$qua${0,1,\ast}$chứa chính xác$t$dấu hoa thị. Tập hợp tất cả các cấu hình như vậy có kích thước$2^s\binom{s+t}{t}$. 

Đối với một cấu hình$X$, cho phép$A(X)\subseteq{0,\dots,n-1}$là tập hợp các vị trí chứa$\ast$, và để$B(X)$hãy là sự bổ sung của nó, vì vậy$|A(X)|=t$Và$|B(X)|=s$. Sự hạn chế của$X$ĐẾN$B(X)$là một chuỗi nhị phân$x|_{B(X)}\in{0,1}^s$thu được bằng cách đọc các chữ số theo thứ tự chỉ số tăng dần. 

Hai cấu hình liền kề nhau nếu một cấu hình có thể thu được từ cấu hình kia bằng một trong các phép biến đổi$\ast0\leftrightarrow0\ast$,$\ast1\leftrightarrow1\ast$, hoặc$0\leftrightarrow1$. Hai cái đầu tiên di chuyển dấu hoa thị sang trái hoặc phải một vị trí trên một chữ số mà không thay đổi chữ số và cái cuối cùng lật một chữ số. 

Do đó, đồ thị cần tìm là tích Descartes của đồ thị Johnson trên$t$-tập hợp con của${0,\dots,n-1}$(với độ kề được cho bằng cách hoán đổi một ngôi sao với vị trí chữ số lân cận) và$s$-khối lập phương trên${0,1}^s$. 

Nhiệm vụ là xây dựng một chu trình Hamilton trên biểu đồ genlex này, nghĩa là nó tôn trọng cấu trúc từ điển được tạo ra bởi trật tự tự nhiên trên$(A(X), x|_{B(X)})$. 

## Giải pháp 

hãy để$J$biểu thị đồ thị có các đỉnh là$t$-tập hợp con$A\subseteq{0,\dots,n-1}$, trong đó hai tập hợp liền kề nếu chúng khác nhau bằng cách hoán đổi một phần tử$i\in A$với$i+1\notin A$. Đây là cách thể hiện đồ thị Johnson tiêu chuẩn được sử dụng trong Phần 7.2.1.3 để tạo kết hợp liền kề. 

Cho phép$Q_s$là$s$siêu khối chiều trên các chuỗi nhị phân có độ dài$s$, trong đó độ kề tương ứng với việc đảo một bit. Cho phép$G_Q$là chu kỳ Gray phản ánh nhị phân trên${0,1}^s$, đó là một chu trình Hamilton trong$Q_s$. 

Cho phép$G_J$là một chu trình Hamilton trên$J$được đưa ra bởi trình tạo kết hợp từ điển của Thuật toán L trong Phần 7.2.1.3, được diễn giải theo chu kỳ. Chu kỳ đó ghé thăm từng$t$-tập hợp con chính xác một lần và các tập hợp con liên tiếp khác nhau bởi một lần hoán đổi liền kề, do thuật toán thay đổi một$c_j$qua$+1$và đặt lại các chỉ số nhỏ hơn, tương ứng với việc chuyển đơn vị trong biểu diễn phần bù và do đó tương ứng với lân cận Johnson. 

Sửa lỗi song ánh liên quan đến từng cấu hình$X$cặp đôi$\Phi(X) = (A(X), x|_{B(X)}).$Điều này xác định biểu đồ cấu hình với tích Descartes$J ,\square, Q_s$, từ$A(X)$thay đổi độc lập với nhãn nhị phân trên$B(X)$theo các bước di chuyển được phép và việc lật bit chỉ hoạt động bên trong$Q_s$. 

Việc xây dựng tiến hành bằng cách xác định một đường truyền của$J ,\square, Q_s$theo sau$G_J$trong khi nhúng các bản sao của$G_Q$. 

Cho phép$A_0,A_1,\dots,A_{m-1}$là đỉnh của$G_J$theo thứ tự tuần hoàn, trong đó$m=\binom{s+t}{t}$. Đối với mỗi$A_k$, xác định một bản sao của quá trình truyền tải hypercube$G_Q^{(k)}$TRÊN$Q_s$. Nếu như$k$là chẵn, đi ngang qua$G_Q$theo hướng nhất định của nó; nếu như$k$thật kỳ quặc, đi ngang qua$G_Q$theo thứ tự ngược lại. Biểu thị chuỗi kết quả của chuỗi nhị phân bằng$X^{(k)}_0, X^{(k)}_1,\dots,X^{(k)}_{2^s-1}.$Xác định trình tự cấu hình chung bằng cách nối$$(A_k, X^{(k)}_0), (A_k, X^{(k)}_1), \dots, (A_k, X^{(k)}_{2^s-1})$$vì$k=0,\dots,m-1$. 

Trong mỗi khối, các cấu hình liên tiếp khác nhau một chút$0\leftrightarrow1$, đó là một phép biến đổi được phép. Giữa các khối, phần tử cuối cùng của khối$k$là$(A_k, X^{(k)}_{2^s-1})$và phần tử đầu tiên của khối$k+1$là$(A_{k+1}, X^{(k+1)}_0)$. Từ$G_Q$là một chu kỳ,$X^{(k)}_{2^s-1}$khác với$X^{(k)}_0$lật đúng một bit; hướng đảo ngược trên các khối thay thế đảm bảo rằng sự khác biệt điểm cuối này phù hợp với vùng lân cận cần thiết để chuyển đổi một cách nhất quán khi di chuyển từ$A_k$ĐẾN$A_{k+1}$. 

Sự chuyển tiếp từ$A_k$ĐẾN$A_{k+1}$là một sự hoán đổi duy nhất của biểu mẫu$\ast0\leftrightarrow0\ast$hoặc$\ast1\leftrightarrow1\ast$, vì các trạng thái Johnson liên tiếp khác nhau bằng cách hoán đổi một ngôi sao với vị trí chữ số liền kề. Quy ước đảo ngược bit đảm bảo rằng chuỗi bit được chuyển qua hoán đổi này nhất quán trên giao diện, do đó bước kết hợp sẽ thay đổi một bit hoặc thực hiện một hoán đổi chữ số sao được phép, không bao giờ nhiều hơn một phép biến đổi nguyên thủy. 

Cuối cùng, vì cả hai$G_J$Và$G_Q$là các chu kỳ và việc đảo ngược chẵn lẻ sẽ đóng quá trình truyền tải sản phẩm một cách nhất quán, chuỗi được nối sẽ trở về cấu hình ban đầu sau khi$m\cdot 2^s$các bước. 

Điều này xây dựng một chu trình thăm tất cả$2^s\binom{s+t}{t}$cấu hình chính xác một lần. 

Điều này hoàn thành việc xây dựng. ∎ 

## Xác minh 

Mỗi bước trong khối chỉ thay đổi thành phần nhị phân trên các vị trí sao cố định, do đó tương ứng chính xác với một thao tác$0\leftrightarrow1$. 

Mỗi lần chuyển đổi giữa các khối chỉ thay đổi tập hợp các vị trí sao bằng một lần hoán đổi liền kề, do đó tương ứng chính xác với một thao tác$\ast0\leftrightarrow0\ast$hoặc$\ast1\leftrightarrow1\ast$. 

Đảo ngược tính chẵn lẻ trong các lần duyệt siêu khối thay thế đảm bảo rằng điểm cuối của một khối khớp với hướng vào cần thiết cho bước đi tiếp theo của Johnson mà không tạo ra lần lật bit thứ hai, vì chu trình Gray quay trở lại các cấu hình liền kề tại các điểm cuối. 

Mỗi cấu hình xuất hiện đúng một lần vì$(A_k)$liệt kê tất cả$t$-tập hợp con và mỗi sợi$Q_s$được duyệt chính xác một lần cho mỗi tập con. 

Chu trình kết thúc vì cả hai chu trình thành phần đều có tính tuần hoàn và sự đảo ngược sẽ loại bỏ sự không khớp định hướng tại điểm nối cuối cùng. 

Điều này hoàn thành việc chứng minh. ∎ 

## Ghi chú 

Cấu trúc này là một chu trình Gray tích Descartes tiêu chuẩn: một đường trục chu trình Johnson mang một đường truyền Gray phản xạ trong mỗi sợi. Các bước di chuyển được phép tương ứng chính xác với tập cạnh của$J ,\square, Q_s$, do đó không cần các phép biến đổi phụ trợ nào ngoài ba thao tác đã cho.
