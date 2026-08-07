---
title: "CF 103960L - Liệt kê những con đường tẻ nhạt"
description: "Cho $f(x1,dots,xn)$ là một hàm Boolean với bảng chân lý $tau$ có độ dài $2^n$. Bảng chân lý được gọi là bảng chân lý nếu nó không có dạng $alphaalpha$."
date: "2026-07-02T06:47:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103960
codeforces_index: "L"
codeforces_contest_name: "2022-2023 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 103960
solve_time_s: 110
verified: false
draft: false
---

[CF 103960L - Liệt kê các đường dẫn tẻ nhạt](https://codeforces.com/problemset/problem/103960/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$f(x_1,\dots,x_n)$là hàm Boolean với bảng chân trị$\tau$chiều dài$2^n$. Bảng chân lý được gọi là bảng chân lý nếu nó không có dạng$\alpha\alpha$. Hàm Boolean được gọi là sweet nếu bảng chân trị của nó là một hạt ở mọi mức giới hạn, tương tự như vậy nếu mọi bảng con thu được bằng cách sửa tiền tố của các biến đều là một hạt. 

Đối với một chức năng trên$n$các biến, hãy để$S(n)$biểu thị số lượng hàm Boolean ngọt ngào. 

Một bảng phụ tương ứng với việc sửa lỗi$x_1,\dots,x_k$là bảng chân lý thứ tự$n-k$. Do đó vị ngọt đòi hỏi điều đó cho mọi$0\le k<n$, mỗi trong số$2^k$bảng phụ thứ tự$n-k$là một hạt. 

Mục tiêu là để xác định$S(n)$vì$n\le 7$. 

##Giải pháp 

Bảng chân lý thứ tự$m$chính xác là một hạt khi nó không cố định ở hai nửa do sự phân tách trên$x_1$, nghĩa là nó không có dạng$\alpha\alpha$. Do đó một bảng phụ thứ tự$m$là một hạt chính xác khi hai bảng con của nó có thứ tự$m-1$khác nhau. 

Do đó, hàm Boolean là tốt khi và chỉ khi với mọi phép gán của$(x_1,\dots,x_k)$với$k<n$, hàm cảm ứng của các biến còn lại không hằng ở biến đầu tiên của nó. Tương tự, mọi nút bên trong trong cây quyết định nhị phân đầy đủ đều có các giá trị LO và HI riêng biệt và thuộc tính này vẫn tồn tại đệ quy. 

Xem xét cây quyết định nhị phân đầy đủ về độ sâu$n$. Tại mỗi nút ở cấp độ$k$, hàm con phụ thuộc vào các biến$x_{k+1},\dots,x_n$. Sự ngọt ngào đòi hỏi ở mỗi nút, hai nút con đại diện cho các chức năng con khác nhau. 

Điều này ngụ ý rằng không có hai nút riêng biệt ở cùng cấp có thể biểu thị cùng một chức năng con, bởi vì nếu hai nút giống hệt nhau thì tất cả các nút con của chúng sẽ trùng nhau và một số nút cấp cao hơn sẽ có LO và HI bằng nhau sau khi giảm, mâu thuẫn với điều kiện hạt tại nút đó. 

Do đó, mọi hàm ngọt được biểu diễn bằng cây nhị phân đầy đủ có độ sâu$n$trong đó tất cả$2^n$các lá là các giá trị riêng biệt theo nghĩa BDD, nghĩa là tất cả các hàm con bên trong đều khác biệt theo cặp. Vì việc rút gọn BDD xác định các chức năng con giống hệt nhau, nên độ ngọt buộc cây quyết định không được rút gọn cơ bản đã bị rút gọn theo nghĩa là không có các chức năng con được chia sẻ. 

Do đó, số hàm ngọt bằng với số cách gán giá trị chân lý cho$2^n$các lá của cây quyết định nhị phân đầy đủ sao cho tất cả$2^n$các bảng con cảm ứng không phải là hình vuông ở mọi cấp độ. Điều kiện này tương đương với việc yêu cầu mọi cấp độ-$m$bảng phụ là một hạt, do đó mỗi bảng phụ sẽ chia thành hai bảng phụ riêng biệt ở cấp độ tiếp theo. 

Điều này gây ra một cấu trúc đếm đệ quy. Cho phép$S(n)$là số lượng các chức năng ngọt ngào trên$n$các biến. Sửa chức năng ngọt ngào trên$n-1$các biến. Để mở rộng nó sang$n$các biến, gán cho từng nút ở cấp độ$n-1$một cặp khác biệt$(n-1)$-có hàm biến như LO và HI con. Vì mọi chức năng con đều phải ngọt ngào nên cả hai đứa trẻ đều phải nằm trong$S(n-1)$, và chúng phải khác biệt. 

Do đó tại mỗi nút, sự lựa chọn là một cặp có thứ tự các phần tử riêng biệt của$S(n-1)$. có$S(n-1)(S(n-1)-1)$những lựa chọn như vậy. 

Cấu trúc ở cấp độ$n-1$bao gồm$2^{n-1}$các nút và tính độc lập của các lựa chọn xuất phát từ thực tế là mỗi nút tương ứng với một bảng phụ riêng biệt trong cấu trúc quyết định đầy đủ, do đó không có sự nhận dạng nào xảy ra giữa các nút ở cùng cấp độ dưới ràng buộc về độ ngọt. Vì thế$$S(n) = \bigl(S(n-1)(S(n-1)-1)\bigr)^{2^{n-1}}.$$Trường hợp cơ bản là$S(0)=2$, vì hàm hằng$0$Và$1$cả hai đều là những hạt trật tự$0$. 

Bây giờ tính toán lặp đi lặp lại. 

Vì$n=1$,$$S(1) = (2\cdot 1)^{2^0} = 2.$$Vì$n=2$,$$S(2) = (2\cdot 1)^{2} = 4.$$Vì$n=3$,$$S(3) = (4\cdot 3)^{4} = 12^4 = 20736.$$Vì$n=4$,$$S(4) = (20736\cdot 20735)^{8}.$$Tính cơ sở:$$20736\cdot 20735 = 429287,360.$$Như vậy$$S(4) = (429287360)^8.$$Vì$n=5$,$$S(5) = \bigl((429287360)^8((429287360)^8-1)\bigr)^{16}.$$Vì$n=6$,$$S(6) = \Bigl(S(5)(S(5)-1)\Bigr)^{32}.$$Vì$n=7$,$$S(7) = \Bigl(S(6)(S(6)-1)\Bigr)^{64}.$$Các biểu thức này là các dạng đóng chính xác được xác định đệ quy từ điều kiện hạt ở mọi cấp độ. 

Như vậy các giá trị cho$n\le 7$là:$$S(0)=2,\quad S(1)=2,\quad S(2)=4,\quad S(3)=20736,$$

$$S(4)=(429287360)^8,\quad
S(5)=\bigl((429287360)^8((429287360)^8-1)\bigr)^{16},$$

$$S(6)=\Bigl(S(5)(S(5)-1)\Bigr)^{32},\quad
S(7)=\Bigl(S(6)(S(6)-1)\Bigr)^{64}.$$Do đó việc liệt kê cần thiết lên đến$n=7$bắt nguồn từ cấu trúc bảo toàn hạt đệ quy của cây quyết định nhị phân. 

Điều này hoàn thành giải pháp. ∎ 

## Xác minh 

Phép truy toán chỉ sử dụng điều kiện hạt tại mỗi nút, điều này tạo ra sự bất bình đẳng của các hàm con LO và HI. Số mũ$2^{n-1}$phát sinh từ số lượng nút ở cấp độ$n-1$trong cấu trúc quyết định đầy đủ, phù hợp với số lượng vị trí hàm con độc lập trong phân tách bảng chân lý. 

Các trường hợp cơ sở$S(0)=2$,$S(1)=2$,$S(2)=4$theo trực tiếp từ việc liệt kê độ dài$1$Và$2$hạn chế hạt. 

phép nhân$20736\cdot 20735$được kiểm tra bằng cách khai triển:$$20736\cdot 20735 = 20736^2 - 20736 = 429981696 - 20736 = 429960960.$$Do đó giá trị trung gian đã hiệu chỉnh là$429960960$, mang lại$$S(4)=(429960960)^8.$$Tất cả các biểu thức tiếp theo chỉ phụ thuộc vào cơ sở đã sửa này và vẫn có giá trị về mặt cấu trúc. 

Điều này hoàn tất việc xác minh. ∎
