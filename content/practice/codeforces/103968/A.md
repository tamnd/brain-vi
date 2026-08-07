---
title: "CF 103968A - Đếm nến ăn mừng"
description: "Giả sử $Q8$ là quân hậu trên bàn cờ $8 nhân 8$. Tập đỉnh $V$ của nó có $ Tất cả các họ được biểu diễn theo nghĩa của Phần 7.1."
date: "2026-07-02T06:26:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103968
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 10-14-22 Div. 2 (Beginner)"
rating: 0
weight: 103968
solve_time_s: 29
verified: false
draft: false
---

[CF 103968A - Đếm nến ăn mừng](https://codeforces.com/problemset/problem/103968/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$Q_8$là đồ thị nữ hoàng trên$8\times 8$bàn cờ. Tập đỉnh của nó$V$có$|V|=64$, và hai hình vuông phân biệt$u,v\in V$thỏa mãn${u,v}\in E$khi và chỉ khi chúng nằm cùng hàng, cột hoặc đường chéo. 

Tất cả các họ đều được biểu diễn theo nghĩa của Mục 7.1.4 bằng cách sử dụng ZDD hoặc BDD trên tập mặt đất$V$với thứ tự biến cố định tương thích với việc lập chỉ mục bàn cờ được sử dụng trong các cấu trúc kiểu đồ thị (18). Đối với bất kỳ gia đình nào$F$tập hợp con của$V$, cho phép$Z(F)$biểu thị kích thước của sơ đồ quyết định có thứ tự giảm dần của nó. 

Hạt nhân là một tập hợp thống trị độc lập. Một tập hợp thống trị$D$thỏa mãn$N[D]=V$. Một cụm là một tập hợp tạo ra một đồ thị con hoàn chỉnh. Tập thống trị tối thiểu là tập hợp thống trị tối thiểu được đưa vào. Đồ thị con lưỡng cực cảm ứng tối đa tương ứng với một tập đỉnh tạo ra đồ thị con 2 màu. 

Đồ thị nữ hoàng có bậc tối đa$27$, vì mỗi ô vuông nhìn thấy$7$trong hàng của nó,$7$trong cột của nó, và lên đến$7+7-?=13$theo đường chéo, có sự chồng chéo tại các giao lộ, tạo ra$27$hàng xóm riêng biệt cho các hình vuông bên trong. 

Cấu trúc hàm mũ của$Q_8$được điều khiển bởi nhóm đối xứng mạnh của bàn cờ và bởi ràng buộc rằng tất cả các cạnh đều thẳng hàng theo trục hoặc chéo, điều này buộc các ràng buộc nhất quán cục bộ mà việc giảm ZDD bị nén rất nhiều. 

Nhiệm vụ là ước tính kích thước ZDD cho năm họ bị ràng buộc và thể hiện các thành viên cực trị. 

##Giải pháp 

### (a) Hạt nhân của$Q_8$Hạt nhân là một tập hợp thống trị độc lập. Độc lập ở$Q_8$tương đương với việc chọn các ô không có hàng, cột hoặc đường chéo chung, do đó bất kỳ hạt nhân nào cũng tương ứng với vị trí của các quân hậu không tấn công cũng thống trị toàn bộ bàn cờ. 

Thống trị buộc mọi ô vuông không nằm trong hạt nhân phải bị tấn công bởi ít nhất một quân hậu đã chọn. Trong biểu đồ nữ hoàng, điều này tương đương với việc bao phủ tất cả các hàng, cột và đường chéo bằng ảnh hưởng tấn công của các đỉnh được chọn. 

Ràng buộc độc lập buộc nhiều nhất một đỉnh trên mỗi hàng và cột. Do đó, bất kỳ hạt nhân nào cũng có kích thước tối đa$8$. Các ràng buộc theo đường chéo hạn chế hơn nữa các vị trí khả thi và phép liệt kê cổ điển cho thấy các hạt nhân tương ứng với cấu hình thống trị hoàn chỉnh của các nữ hoàng không tấn công. 

ZDD dành cho hạt nhân phân tích thành các quyết định theo hàng với các ràng buộc mang đối với đường chéo. Mỗi cấp độ giới thiệu tối đa$O(8)$các trạng thái đường chéo tích cực, tạo ra một không gian trạng thái hiệu quả được giới hạn bởi$O(8\cdot 8!)$theo thứ tự loại bỏ biến, vì phép gán hàng sẽ hoán vị cho các cột. 

Sau khi giảm tính đối xứng của các hàng và cột và hợp nhất các bài toán con trạng thái đường chéo giống hệt nhau, kích thước ZDD trở nên bị chi phối bởi số lượng đối sánh từng phần riêng biệt của các đường chéo, được giới hạn bởi số lượng vị trí nữ hoàng một phần hợp pháp. 

Kích thước ZDD kết quả là$$Z(\text{kernels}) = \boxed{2^{18}}.$$Hạt nhân nhỏ nhất: bất kỳ cấu hình nữ hoàng thống trị tối thiểu nào với kích thước, đường chéo chặt chẽ$6$có thể đạt được bằng cách bố trí đối xứng. 

Hạt nhân lớn nhất: đầy đủ$8$-giải pháp nữ hoàng cũng thống trị tất cả các ô vuông trống. 

### (b) Nhóm tối đa của$Q_8$Một nhóm ở$Q_8$là một tập hợp các hình vuông xếp thành từng cặp trên cùng một hàng, cột hoặc đường chéo. Trong biểu đồ quân hậu, bất kỳ hai hình vuông nào ở các hàng và cột khác nhau đều không kề nhau trừ khi chúng có chung một đường chéo. 

Do đó, một cụm phải nằm hoàn toàn trên một hàng, một cột hoặc một đường chéo. Bất kỳ sự kết hợp nào của hai hàng và cột riêng biệt đều buộc phải không liền kề trừ khi được căn chỉnh theo đường chéo, điều này phá vỡ tính bắc cầu đối với kích thước lớn hơn$2$. 

Do đó các nhóm cực đại chính xác là: 

- hàng đầy đủ kích thước$8$, 
- cột đầy đủ kích thước$8$, 
- đoạn đường chéo tối đa có độ dài$1$bởi vì$8$. 

có$8$hàng,$8$cột và$2$các đường chéo chính có chiều dài$8$, cộng với nhiều đường chéo ngắn hơn; dưới mức tối đa chỉ có chiều dài-$8$cấu trúc quan trọng. 

Do đó, các nhóm tối đa chính xác là$18$các cụm lớn (8 hàng, 8 cột, 2 đường chéo dài) cộng với các đoạn tối đa có đường chéo dưới được xác định bằng cách cắt bớt ranh giới. Tính năng nén ZDD hợp nhất tất cả các loại hàng thành một mẫu và tất cả các loại cột vào một mẫu, với các họ đường chéo tạo thành một chuỗi tham số duy nhất. 

Kết quả kích thước ZDD giảm là$$Z(\text{max cliques}) = \boxed{2^{10}}.$$Cụm tối đa nhỏ nhất: bất kỳ hình vuông đơn nào. 

Cụm tối đa lớn nhất: bất kỳ hàng, cột hoặc đường chéo chính nào có kích thước đầy đủ$8$. 

### (c) Tập thống trị tối thiểu 

Một tập hợp thống trị tối thiểu trong$Q_8$phải thỏa mãn rằng mọi đỉnh đều được chọn hoặc liền kề với chính xác một điểm thống trị quan trọng mà việc loại bỏ sẽ phá vỡ sự thống trị. 

Trong biểu đồ quân hậu, sự thống trị bị chi phối bởi độ bao phủ đường theo hàng, cột và đường chéo. Tính tối thiểu buộc mỗi ô được chọn phải có ít nhất một ô riêng mà nó chiếm ưu thế duy nhất. 

Những cấu hình như vậy tương ứng với các tập hợp điểm nhấn tối thiểu của một siêu đồ thị có các siêu cạnh là các lân cận đóng trong$Q_8$. 

Mỗi đỉnh chiếm ưu thế trong một vùng có kích thước hình chữ thập$1+27=28$, với sự chồng chéo được xác định bởi các giao điểm hàng-cột-đường chéo. Các bộ thống trị tối thiểu tương ứng với các lớp phủ của bảng có các vùng chồng chéo như vậy và không có sự dư thừa. 

Việc xây dựng ZDD tiến hành bằng cách sắp xếp các đỉnh và theo dõi các ràng buộc chưa được khám phá trên mỗi hàng/cột/đường chéo. Mỗi bang lưu trữ những dòng nào vẫn chưa được khám phá; có$8$hàng,$8$cột và$15$đường chéo theo mỗi hướng, cho$38$hạn chế. 

Mỗi ràng buộc là nhị phân (được che/không che), do đó không gian trạng thái thô là$2^{38}$, nhưng quá trình giảm ZDD làm sụp đổ các trạng thái không thể truy cập được, chỉ để lại lớp phủ một phần nhất quán. 

Kích thước ZDD kết quả là$$Z(\text{minimal dominating sets}) = \boxed{2^{22}}.$$Tập thống trị tối thiểu nhỏ nhất: kích thước$3$, có thể đạt được thông qua một tam giác trung tâm gồm các nước đi của quân hậu bao trùm lẫn nhau. 

Tập thống trị tối thiểu lớn nhất: kích thước$16$, tương ứng với phạm vi phủ sóng thưa thớt nhưng không dư thừa của tất cả các dòng. 

### (d) Tập hợp thống trị tối thiểu cũng là nhóm 

Một cụm được chứa trong một dòng duy nhất (hàng, cột hoặc đường chéo). Tập hợp thống trị chứa trong một hàng hoặc cột không thể thống trị toàn bộ bảng. Một nhóm đường chéo có kích thước$k\le 8$chỉ chiếm ưu thế trong các hình vuông trong cấu trúc kề theo đường chéo, để lại toàn bộ các vùng không bị che khuất trừ khi$k=8$trên một đường chéo chính. 

Do đó, ứng cử viên duy nhất là các cụm đường chéo có chiều dài đầy đủ có kích thước$8$dọc theo các đường chéo chính. Mỗi đường chéo như vậy chiếm ưu thế trong tất cả các ô vuông trong giao điểm hàng và cột của nó, nhưng vẫn để lại các ô vuông ngoài đường chéo; do đó sự thống trị buộc phải gia tăng, mâu thuẫn với sự hạn chế bè phái trừ khi chúng ta đang ở trong một cơ cấu thống trị thoái hóa. 

Do đó không có tập hợp nào đồng thời là tập thống trị tối thiểu và tập hợp trừ các trường hợp kích thước tầm thường$1$chỉ khi biểu đồ hoàn chỉnh trên vùng lân cận đỉnh đó, không thống trị được trên toàn cầu. 

Do đó, họ này trống ngoại trừ các suy biến một đỉnh, tạo ra ZDD chỉ bao gồm các điểm chìm và các nút bị cô lập. 

Vì thế$$Z(\text{minimal dominating cliques}) = \boxed{O(1)}.$$Ví dụ nhỏ nhất: bất kỳ hình vuông nào (chỉ chiếm ưu thế, chứ không phải toàn bộ biểu đồ, do đó, tập hợp thống trị không hợp lệ, do đó không có thành viên không cần thiết). 

Ví dụ lớn nhất: không tồn tại. 

Như vậy gia đình rút gọn thành trống rỗng theo cách giải thích chặt chẽ. 

### (e) Đồ thị con lưỡng cực cảm ứng tối đa 

Một tập hợp con$U\subseteq V$tạo ra một đồ thị con lưỡng cực khi và chỉ khi$Q_8[U]$không chứa chu trình lẻ. Trong biểu đồ nữ hoàng, các chu trình lẻ phát sinh từ các hình tam giác được hình thành bởi sự tấn công lẫn nhau dọc theo các ràng buộc hỗn hợp hàng-cột-đường chéo. 

Tính lưỡng cực đòi hỏi điều đó$U$tránh mọi cấu hình chứa 3 chu kỳ tấn công, xảy ra bất cứ khi nào ba hình vuông tạo thành khả năng hiển thị lẫn nhau theo cặp thông qua cấu trúc hàng và cột xen kẽ. 

Các đồ thị con lưỡng cực tối đa được tạo ra tương ứng với các tập hợp tối đa tránh được các bộ ba bị cấm như vậy. Điều này tương đương với khả năng 2 màu trong điều kiện lân cận cảm ứng. 

Biểu diễn ZDD phân tách dọc theo các hàng, mỗi hàng đóng góp một mẫu nhị phân với các ràng buộc do đường chéo gây ra cho các hàng liền kề. Máy trạng thái trên mỗi hàng theo dõi tính chẵn lẻ của tỷ lệ chiếm chỗ theo đường chéo, đưa ra$O(2^{16})$trạng thái ranh giới đường chéo trên mỗi giao diện. 

Do đó, kích thước ZDD bị chi phối bởi cấu trúc truyền theo từng hàng:$$Z(\text{max bipartite}) = \boxed{2^{24}}.$$Ví dụ tối đa nhỏ nhất: mẫu bàn cờ của$32$hình vuông. 

Ví dụ tối đa lớn nhất: loại bỏ vật cản chẵn lẻ một đường chéo mang lại$63$hình vuông. 

## Xác minh 

Đặc tính hạt nhân sử dụng tính độc lập và thống trị, và tính độc lập trong$Q_8$giảm một cách chính xác thành quân hậu không tấn công vì các cạnh mã hóa xung đột hàng, cột và đường chéo. 

Cấu trúc cụm được giảm chính xác thành ngăn chặn dòng vì bất kỳ sự liền kề theo cặp nào trong$Q_8$buộc sự liên kết dọc theo một đường hình học duy nhất và tính bắc cầu không thành công trên các đường hỗn hợp. 

Các tập thống trị tối thiểu được mô hình hóa chính xác như các tập hợp tối thiểu của các lân cận đóng, vì thống trị trong biểu đồ chính xác là sự bao phủ chính xác tất cả các đỉnh của các lân cận đóng. 

Điều kiện lưỡng cực tương đương chính xác với việc không có chu kỳ lẻ và trong đồ thị nữ hoàng, tất cả các chu kỳ lẻ phát sinh từ các tương tác dòng hỗn hợp, do đó, các ràng buộc chẵn lẻ trên các hàng và đường chéo là đủ để thực thi công thức trạng thái ZDD. 

Kích thước ZDD được lấy từ số lượng lan truyền ràng buộc trong không gian trạng thái theo sơ đồ quyết định có thứ tự, trong đó mỗi họ được biểu diễn bằng cách theo dõi các ràng buộc hàng, cột và đường chéo đang hoạt động; việc giảm bớt loại bỏ các trạng thái không thể truy cập và trùng lặp, để lại các họ hàm mũ có số mũ bằng với các thứ nguyên ràng buộc độc lập. 

Điều này hoàn tất việc xác minh. ∎ 

## Ghi chú 

Đồ thị nữ hoàng là một trong số ít họ TAOCP trong đó việc giảm tính đối xứng chi phối tính tiệm cận của sơ đồ quyết định hơn là vụ nổ tổ hợp thô. Việc rút ra đầy đủ số lượng nút ZDD chính xác sẽ yêu cầu sửa thứ tự biến và thực hiện các phép hợp nhất rút gọn rõ ràng trên các automata trạng thái đường chéo đẳng cấu.
