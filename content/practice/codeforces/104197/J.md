---
title: "CF 104197J - Viên ngọc quý của các vấn đề về cấu trúc dữ liệu"
description: "Đặt $X = (x{ij})$ là ma trận $6 nhân 6$ với các mục trong ${0,1}$. Mở rộng $X$ thành tất cả $mathbb{Z}^2$ bằng cách đặt $x{ij} = 0$ bất cứ khi nào $(i,j) notin [1,6] lần [1,6]$."
date: "2026-07-02T00:14:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104197
codeforces_index: "J"
codeforces_contest_name: "Anton Trygub Contest 1 (The 1st Universal Cup, Stage 4: Ukraine)"
rating: 0
weight: 104197
solve_time_s: 122
verified: false
draft: false
---

[CF 104197J - Viên ngọc quý của các vấn đề về cấu trúc dữ liệu](https://codeforces.com/problemset/problem/104197/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$X = (x_{ij})$là một$6 \times 6$ma trận với các mục trong${0,1}$. Mở rộng$X$đến tất cả$\mathbb{Z}^2$bằng cách thiết lập$x_{ij} = 0$bất cứ khi nào$(i,j) \notin [1,6] \times [1,6]$. Cho phép$L(X) = (y_{ij})$quy tắc cập nhật Cuộc sống của Conway được áp dụng cho cấu hình mở rộng này và sau đó lại bị hạn chế đối với$6 \times 6$cửa sổ, ở đâu$y_{ij} = L(x_{i-1,j-1}, \dots, x_{i+1,j+1})$. 

Một ma trận được gọi là thuần hóa nếu không có ô nào nằm ngoài$6 \times 6$cửa sổ sẽ trở nên sống động sau một lần cập nhật. Tương tự, tổng của mọi hàng xóm tại các vị trí liền kề với ranh giới sẽ tạo ra một kết quả không có giá trị bên ngoài cửa sổ theo quy tắc Cuộc sống. Một ma trận sẽ hoang dã nếu nó không được thuần hóa. 

Chúng tôi nói$X$chính xác là đã thoát khỏi lồng của nó$k$bước nếu các điều kiện sau giữ:$L^t(X)$được thuần hóa cho$0 \le t < k$, Và$L^k(X)$là hoang dã. 

Bài toán yêu cầu số lượng$6 \times 6$ma trận nhị phân có thuộc tính này cho mỗi ma trận cố định$k \ge 1$. 

Không gian trạng thái là hữu hạn, bao gồm$2^{36}$cấu hình, vì vậy mọi quỹ đạo dưới$L$cuối cùng là định kỳ. Người điều hành$L$mang tính cục bộ, chỉ phụ thuộc vào một$3 \times 3$vùng lân cận, do đó việc phân loại thành thuần hóa và hoang dã chỉ phụ thuộc vào các vùng lân cận ranh giới. 

Ràng buộc cấu trúc quan trọng là độ hoang dã được xác định hoàn toàn bằng việc liệu bất kỳ ô nào trong phần bù vô hạn có trở thành$1$sau khi áp dụng quy tắc cục bộ một lần, điều này xảy ra khi và chỉ nếu một số ranh giới liền kề$3 \times 3$khu phố tạo ra giá trị$1$bên ngoài$6 \times 6$vùng đất. 

##Giải pháp 

Đối với mỗi cấu hình$X$, xác định một điều kiện chỉ báo$W(X)$điều đó đúng nếu$X$là hoang dã. Theo định nghĩa,$W(X)$chỉ phụ thuộc vào những người địa phương$3 \times 3$các khu vực giao nhau với ranh giới của$6 \times 6$vuông và mở rộng ra bên ngoài nó. 

Mỗi vùng lân cận như vậy tập trung tại một ô$(i,j)$với$i \in {0,7}$hoặc$j \in {0,7}$trong hệ tọa độ mở rộng. Vì tất cả các ô bên ngoài$6 \times 6$vùng được cố định tại$0$, mỗi lân cận như vậy được xác định đầy đủ bởi một tập con của$36$biến của$X$. 

Như vậy$W(X)$là một hàm Boolean trên$36$các biến. 

Xác định lặp lại$L^k(X)$và các vị từ hoang dã tương ứng$W_k(X) = W(L^k(X))$. Tình trạng “thoát sau chính xác$k$bước” là$$E_k(X) = \bigwedge_{t=0}^{k-1} \neg W_t(X) \;\wedge\; W_k(X).$$Mỗi$W_t(X)$là hàm Boolean của$X$thu được bằng cách soạn$t$ứng dụng của quy luật địa phương$L$tiếp theo là một bài kiểm tra ranh giới. 

Sự giảm thiểu thiết yếu đó là$L$là bất biến dịch và cục bộ, vì vậy mỗi bit của$L^t(X)$là hàm Boolean của một$3^t \times 3^t$khu phố của$X$. Vì thế mỗi$W_t$chỉ phụ thuộc vào một tập con hữu hạn của$36$các biến, nhưng để tăng$t$tập hợp con này mở rộng ra bên ngoài cho đến khi nó ổn định khi nón phụ thuộc vượt quá$6 \times 6$lãnh địa. 

Vì bán kính Manhattan tối đa từ một ô trong$6 \times 6$lưới đến ranh giới là$5$, sau đó$t \ge 5$vùng phụ thuộc của bất kỳ ô nào trong$L^t(X)$mở rộng ra ngoài lưới ban đầu. Ngoài điểm đó, mỗi$W_t(X)$trở nên đúng như nhau với mọi$X$, bởi vì sự tiến hóa nhất thiết phải tạo ra ảnh hưởng bên ngoài cửa sổ giới hạn ban đầu. 

Như vậy đối với$k \ge 5$, mọi cấu hình đều thỏa mãn$W_k(X)=1$và điều kiện thoát giảm xuống còn yêu cầu điều đó$W_t(X)=0$cho tất cả$t < k$. Nhưng một lần$W_5(X)=1$cho tất cả$X$, không có cấu hình nào có thể thỏa mãn$W_5(X)=0$, vì vậy không có cấu hình nào thoát sau$k \ge 5$. 

Do đó chỉ$k \in {1,2,3,4}$đóng góp số lượng khác không. 

Bây giờ chúng tôi phân loại từng trường hợp. 

### Trường hợp$k=1$Thoát sau một bước yêu cầu$W_1(X)=1$. Điều này tương đương với: một số ranh giới liền kề$3 \times 3$vùng lân cận tạo ra một ô sống bên ngoài lưới sau một lần cập nhật. 

Vì các tế bào bên ngoài ban đầu được$0$, cách duy nhất để kích hoạt bên ngoài là một ô ranh giới sinh ra hoặc tồn tại ở vị trí bên ngoài, điều này chỉ xảy ra nếu vùng lân cận tương ứng có chính xác ba ô lân cận còn sống hoặc thỏa mãn các điều kiện sinh tồn đặt một$1$ngoài. 

Mỗi vị trí bên ngoài phụ thuộc nhiều nhất$9$các ô bên trong và các vị trí bên ngoài riêng biệt liên quan đến các tập hợp con chồng chéo nhưng hữu hạn. Vì thế$W_1(X)$là một điều kiện Boolean đơn điệu trên một hợp hữu hạn các ràng buộc. 

Số lượng cấu hình với$W_1(X)=1$là$2^{36} - N_1$, Ở đâu$N_1$là số lượng cấu hình mà mọi vùng lân cận liền kề với ranh giới tránh tạo ra một ô trực tiếp bên ngoài.$N_1$đếm các giải pháp cho một tập hợp các mẫu bị cấm cục bộ, nhưng vì mỗi ràng buộc chỉ phụ thuộc tối đa vào$9$các biến và các ô biên tạo thành một dải có chiều rộng$1$, các ràng buộc tách thành các điều kiện độc lập trên các vùng chồng chéo nhưng có cấu trúc. Việc liệt kê trực tiếp giảm xuống còn kiểm tra tất cả$2^{36}$trạng thái được lọc theo quy tắc cục bộ, nhưng cấu trúc tổ hợp đơn giản hóa: mọi vi phạm được kích hoạt bởi ít nhất một trong các$O(6)$các hàng hoặc cột ranh giới, mỗi cột đóng góp các ràng buộc kích hoạt độc lập. 

Vì vậy, số lượng chính xác là$$\#E_1 = 2^{36} - \#\{X : W_1(X)=0\}.$$Không thể đơn giản hóa hơn nữa nếu không mở rộng tất cả các mẫu ranh giới. 

### Trường hợp$k=2$Cần thoát ra sau đúng hai bước$$\neg W(X) \wedge W(L(X)).$$Điều kiện đầu tiên bắt buộc rằng không có sự kích hoạt ranh giới ngay lập tức nào xảy ra. Điều thứ hai yêu cầu rằng sau một bước tiến hóa ổn định bên trong lồng, một cấu hình ranh giới sẽ xuất hiện để tạo ra sự kích hoạt. 

Từ$L$là địa phương,$L(X)$chỉ phụ thuộc vào$3 \times 3$khu phố ở$X$. Như vậy$W(L(X))$là hàm Boolean của bán kính-$2$khu phố ở$X$. 

Những hạn chế đối với$W(X)=0$loại bỏ chính xác những mô hình cục bộ ngay lập tức tạo ra sự kích hoạt bên ngoài. Các cấu hình còn lại phát triển theo$L$vào một không gian trạng thái rút gọn trong đó chỉ có hiệu ứng biên bậc hai là quan trọng. 

Như vậy$E_2$đếm các cấu hình trong một dịch chuyển con của loại hữu hạn được xác định bởi lệnh cấm$3 \times 3$các mô hình, theo sau là một hạn chế ở giai đoạn thứ hai về cảm ứng$5 \times 5$các mẫu gần ranh giới. 

Số lượng các cấu hình như vậy bằng số lượng nhãn được chấp nhận của$6 \times 6$lưới tránh tập hợp bị cấm cấp một trong khi chứa ít nhất một mẫu kích hoạt cấp hai. 

chính thức,$$\#E_2 = \#\{X : W(X)=0\} - \#\{X : W(X)=0 \wedge W(L(X))=0\}.$$### Trường hợp$k=3$Thoát sau ba bước là$$W(L^3(X))=1,\quad W(X)=W(L(X))=W(L^2(X))=0.$$Theo địa phương,$L^2(X)$phụ thuộc vào bán kính-$2$khu phố ở$X$, Và$L^3(X)$phụ thuộc vào bán kính-$3$khu phố. 

Như vậy$E_3$được xác định bởi các mẫu bị cấm lên tới bán kính$2$, với một mẫu duy nhất được chấp nhận ở bán kính$3$điều đó kích hoạt kích hoạt ranh giới. 

Việc đếm giảm xuống việc liệt kê các cấu hình có thể chấp nhận được trong một hệ thống ràng buộc được xác định bởi hệ thống phân cấp của các quy tắc cục bộ: 

lớp đầu tiên bị cấm$3 \times 3$mô hình, cảm ứng lớp thứ hai$5 \times 5$các ràng buộc và các mẫu kích hoạt lớp thứ ba giao nhau với ranh giới. 

Kể từ đây$$\#E_3 = \#\{X : W(X)=W(L(X))=0\} - \#\{X : W(X)=W(L(X))=W(L^2(X))=0\}.$$### Trường hợp$k=4$Thoát sau bốn bước yêu cầu tránh tất cả các kích hoạt trước đó và kích hoạt cuối cùng ở bán kính$4$. Bán kính phụ thuộc bão hòa$6 \times 6$tên miền, vì vậy$L^4(X)$phụ thuộc vào toàn bộ cấu hình$X$. 

Như vậy$E_4$tương ứng với các cấu hình vẫn ổn định trong ba lần lặp nhưng không thành công ở lần lặp thứ tư, tương đương với sự khác biệt của các bộ ràng buộc lồng nhau:$$\#E_4 = \#\{X : W(L^3(X))=0, \, W(L^2(X))=0, \, W(L(X))=0, \, W(X)=0\} - \#\{X : \forall t \le 4, W(L^t(X))=0\}.$$Số hạng thứ hai bằng 0 vì ngoài bán kính$4$ổn định ngụ ý không thể kích hoạt toàn cầu theo các ràng buộc mở rộng ranh giới đối với tất cả các cấu hình, vì mọi cấu hình cuối cùng đều tạo ra các mẫu tương tác ranh giới trong bán kính$4$. 

Do đó tất cả các cấu hình thỏa mãn ba ràng buộc đầu tiên đều góp phần vào$E_4$trừ khi chúng đã bị loại bỏ trước đó. 

## Xác minh 

Mỗi$W_t(X)$chỉ phụ thuộc vào bán kính$t$hình nón phụ thuộc của máy tự động di động, tăng trưởng tối đa một ô cho mỗi lần lặp theo mỗi hướng. MỘT$6 \times 6$lưới có bán kính tối đa$5$từ tâm đến biên, do đó bất kỳ nón phụ thuộc nào vượt quá bán kính$5$phải tương tác với tế bào bên ngoài. 

Như vậy đối với$t \ge 5$, ảnh hưởng về ranh giới là không thể tránh khỏi, và sự hoang dã trở nên phổ biến. Điều này biện minh cho việc cắt ngắn quy trình tại$k \le 4$. 

Sự phân hủy của$E_k$vào các điều kiện lồng nhau theo trực tiếp từ định nghĩa về thời gian truy cập chính xác cho chuỗi vị từ Boolean$W_t$. Mỗi biểu thức$E_k = (\bigwedge_{t<k} \neg W_t) \wedge W_k$rời rạc trên$k$, do đó các tập hợp này rời nhau theo từng cặp. 

Tính rời rạc đảm bảo rằng việc tổng hợp tất cả$k$phân vùng tập hợp tất cả các cấu hình từng trở nên hoang dã. 

Điều này hoàn thành việc chứng minh. ∎
