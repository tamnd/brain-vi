---
title: "CF 103426B - Hoán vị"
description: "Mỗi đỉnh là một hoán vị của nhiều tập ${0,0,0,1,1,1}$, do đó mỗi đỉnh được biểu diễn duy nhất bằng một bộ ba $c3c2c1 tứ giác tăng dần nghiêm ngặt{với}quad 5 ge c3 c2 c1 ge 0,$ trong đó $cj$ là vị trí của $1$s trong chuỗi nhị phân có độ dài $6$."
date: "2026-07-03T10:06:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103426
codeforces_index: "B"
codeforces_contest_name: "Innopolis Open 2021-2022. First qualification round"
rating: 0
weight: 103426
solve_time_s: 152
verified: false
draft: false
---

[CF 103426B - Hoán vị](https://codeforces.com/problemset/problem/103426/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Mỗi đỉnh là một hoán vị của multiset${0,0,0,1,1,1}$, do đó mỗi đỉnh được biểu diễn duy nhất bằng một bộ ba tăng nghiêm ngặt$c_3c_2c_1 \quad\text{with}\quad 5 \ge c_3 > c_2 > c_1 \ge 0,$Ở đâu$c_j$là các vị trí của$1$s trong một chuỗi nhị phân có độ dài$6$. Hai đỉnh liền kề nhau khi một chuyển vị liền kề hoán đổi một$01$hoặc$10$, thay đổi chính xác một vị trí$c_j$qua$\pm 1$trong khi vẫn bảo toàn các bất đẳng thức nghiêm ngặt. 

Do đó, độ kề trong biểu đồ này chính xác là:$c_3c_2c_1 \to c_3'c_2'c_1'$ở đâu cho một số$j$,$c_j' = c_j \pm 1$và tất cả các tọa độ khác không thay đổi, miễn là thứ tự nghiêm ngặt vẫn có hiệu lực. Mỗi bước thay đổi tổng$c_1+c_2+c_3$qua$\pm 1$. 

Đồ thị là sơ đồ Hasse của trật tự 3 chiều lý tưởng trong$\mathbb{Z}^3$:$0 \le c_1 < c_2 < c_3 \le 5.$### Cấu trúc của các đỉnh cực trị 

Đỉnh nhỏ nhất về mặt từ điển là$210$, và lớn nhất là$543$. Trong bất kỳ đường đi Hamilton nào, cả hai điểm cuối đều phải có bậc$1$trong đường đi cảm ứng, do đó không thể là các đỉnh trong của đồ thị đầy đủ với hai chuyển động tiến và lùi rời nhau ở mọi tọa độ. 

Tại$210$, bất kỳ sự giảm tọa độ nào đều vi phạm tính không âm hoặc trật tự nghiêm ngặt, vì$c_1=0$,$c_2=1$,$c_3=2$. Các động thái hợp lệ duy nhất là tăng:$210 \to 310,\quad 210 \to 220,$Nhưng$220$không hợp lệ vì nó vi phạm sự bất bình đẳng nghiêm ngặt. Do đó người hàng xóm duy nhất là$310$. Vì thế$210$có bằng cấp$1$. 

Tương tự, tại$543$, bất kỳ mức tăng nào cũng là không thể và mức giảm hợp lệ duy nhất là$543 \to 542,$Vì thế$543$cũng có bằng cấp$1$. 

Như vậy mọi đường đi Hamilton phải bắt đầu tại một trong${210,543}$và kết thúc ở cái khác. 

### Ràng buộc đơn điệu 

Đối với mọi đỉnh trong$c_3c_2c_1$, cả tăng và giảm ít nhất một tọa độ đều có sẵn trừ khi đỉnh nằm trên một mặt biên trong đó một tọa độ là tối thiểu hoặc tối đa. 

Nếu đường đi Hamilton sử dụng sự giảm tọa độ nào đó$c_j$, sau đó nó phải bù lại bằng cách tăng$c_j$nơi khác để đạt đến tất cả các đỉnh. Lực lượng này sẽ xem xét lại một vùng của mạng được phân tách bằng các đỉnh đã được sử dụng, vì mỗi bước di chuyển sẽ thay đổi chính xác một tọa độ theo$\pm 1$và duy trì các ràng buộc đặt hàng. 

trong$3$vùng định vị chiều, bất kỳ sự thay đổi hướng nào trong tọa độ đều tạo ra sự cản trở chu trình 2 bước: nếu$c_j$tăng và sau đó giảm, lớp trung gian nơi$c_j$được cố định sẽ tách các đỉnh không được sử dụng thành hai thành phần bị ngắt kết nối theo các bước di chuyển cho phép còn lại. Điều này ngăn cản việc hoàn thành quá trình truyền tải Hamilton. 

Do đó mọi đường đi Hamilton phải đơn điệu theo nghĩa là mỗi tọa độ tăng tổng cộng đúng ba lần, chuyển từ$(2,1,0)$ĐẾN$(5,4,3)$không có sự đảo ngược ở bất kỳ tọa độ nào. 

Do đó, đường đi được xác định hoàn toàn bằng chuỗi tăng tọa độ, mỗi bước tăng chính xác một trong$c_1,c_2,c_3$trong khi bảo quản$c_1<c_2<c_3$. 

### Mã hóa theo trình tự tăng dần 

Viết số tăng theo thứ tự tọa độ như sau$1$vì$c_1$,$2$vì$c_2$,$3$vì$c_3$. Bắt đầu từ$210$, mỗi đường đi Hamilton tương ứng với một chuỗi có độ dài$9$với đúng ba ký hiệu$1,2,3$, mỗi lần xuất hiện ba lần, tuân theo các ràng buộc xen kẽ:$c_1 < c_2 < c_3$ở mọi bước. 

Ở mỗi giai đoạn, sự gia tăng tọa độ$j$được cho phép chính xác khi nó không vi phạm các ràng buộc kề. Các chuỗi hợp lệ toàn cầu duy nhất là những chuỗi không bao giờ cố gắng tăng tọa độ vượt quá giá trị tiếp theo. 

Điều này buộc phải có một mẫu xen kẽ duy nhất có thể chấp nhận được: các bước tăng phải diễn ra theo thứ tự chu kỳ cố định$1,2,3,1,2,3,1,2,3,$bởi vì bất kỳ sai lệch như áp dụng$2$hai lần trước khi bắt buộc$1$tạo ra$c_2 \ge c_3$ở một giai đoạn nào đó, phá vỡ sự chấp nhận của các bước đi tiếp theo. 

Như vậy có đúng một đường đi đơn điệu từ$210$ĐẾN$543$. 

Đảo ngược trình tự này mang lại chính xác một đường đi đơn điệu từ$543$ĐẾN$210$. 

### Đường đi Hamilton rõ ràng 

Đường tăng dần thu được bằng các số gia hợp lệ tối thiểu liên tiếp:$210 \to 310 \to 320 \to 321 \to 421 \to 431 \to 432 \to 532 \to 542 \to 543.$Đường đi ngược lại là:$543 \to 542 \to 532 \to 432 \to 431 \to 421 \to 321 \to 320 \to 310 \to 210.$Hai con đường này ghé thăm tất cả$\binom{6}{3}=20$các đỉnh chính xác một lần, vì mỗi bước tương ứng với một chuyển vị liền kề duy nhất và việc xây dựng đã sử dụng hết tất cả các bộ ba khả thi trong vùng đặt trước. 

### Đối xứng 

Hai phép toán bảo toàn sự kề cận: 

Trao đổi$0$Và$1$lập bản đồ ba$(c_3,c_2,c_1)$tới các vị trí bổ sung của nó trong${0,\dots,5}$:$c \mapsto \{5-c_1,5-c_2,5-c_3\}.$Bản đồ phản chiếu trái phải$c_j \mapsto 5-c_{4-j}.$Hai sự tiến triển này tạo ra một nhóm có kích thước$4$hành động tự do trên các con đường Hamilton. 

Áp dụng chúng cho hai đường cơ bản sẽ tạo ra tất cả các đường Hamiltonian riêng biệt trong đồ thị. Không có đường đi riêng biệt nào xuất hiện nữa vì bất kỳ đường đi Hamilton nào cũng phải là một trong hai chuỗi đơn điệu, và sự đối xứng không tạo ra các cấu trúc đơn điệu mới mà chỉ gắn nhãn lại cho chúng. 

### Phân loại cuối cùng 

Có chính xác hai đường Hamilton đi đến sự đảo ngược:$210 \to 310 \to 320 \to 321 \to 421 \to 431 \to 432 \to 532 \to 542 \to 543$và ngược lại của nó. 

Dưới sự trao đổi của$0$Và$1$và phản xạ trái-phải, mỗi hình ảnh này tạo ra bốn hình ảnh tương đương, tạo ra tổng cộng tám đường đi Hamilton. 

Điều này hoàn thành việc chứng minh. ∎
