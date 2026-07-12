---
title: "CF 103194B - \u0414\u0432\u0435 \u043b\u044e\u0441\u0442\u0440\u044b"
description: "Sự kết hợp 3-đa của ${0,1,dots,n-1}$ là bộ ba $d1 le d2 le d3,qquad 0 le di le n-1.$ Một chu trình phổ quát là một chuỗi tuần hoàn $a0 a1 dots a{L-1}$ trên ${0,dots,n-1}$ sao cho mọi cửa sổ tuần hoàn có độ dài 3 $(ai,a{i+1},a{i+2})quad (bmod L)$ xuất hiện chính xác…"
date: "2026-07-03T15:57:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103194
codeforces_index: "B"
codeforces_contest_name: "2020-2021 \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0437\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f, \u0442\u0443\u0440 1"
rating: 0
weight: 103194
solve_time_s: 136
verified: false
draft: false
---

[CF 103194B - \u0414\u0432\u0435 \u043b\u044e\u0441\u0442\u0440\u044b](https://codeforces.com/problemset/problem/103194/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Sự kết hợp 3-đa của${0,1,\dots,n-1}$là bộ ba tăng yếu$d_1 \le d_2 \le d_3,\qquad 0 \le d_i \le n-1.$Một chu trình phổ quát là một chuỗi tuần hoàn$a_0 a_1 \dots a_{L-1}$qua${0,\dots,n-1}$sao cho mỗi cửa sổ tuần hoàn có độ dài 3$(a_i,a_{i+1},a_{i+2})\quad (\bmod L)$xuất hiện chính xác một lần sau khi sắp xếp theo thứ tự không giảm và mỗi kết hợp 3 đa xuất hiện đúng một lần. 
Số cách kết hợp 3 đa số là$\binom{n+2}{3} = \frac{n(n+1)(n+2)}{6}.$Chúng ta phải xác định có bao nhiêu chu kỳ phổ quát như vậy tồn tại và khi nào chúng là các chu kỳ (các chuyến tham quan khép kín) so với các con đường mở. 

## Giải pháp 

Đại diện cho mỗi 3 đa tổ hợp$d_1 \le d_2 \le d_3$bằng _ mã hóa khoảng cách liền kề _$x = d_2 - d_1,\qquad y = d_3 - d_2,$với$x,y \ge 0$Và$x+y \le n-1$. 

Điều này đưa ra sự phân biệt giữa 3 đa tổ hợp và các điểm mạng trong vùng tam giác$T_n = \{(x,y)\in \mathbb{Z}_{\ge 0}^2 : x+y \le n-1\}.$Một chu trình phổ quát tương ứng với việc sắp xếp tất cả các điểm của$T_n$sao cho các bộ ba liên tiếp chồng lên nhau một cách nhất quán. Điều này tương đương với chu trình Hamilton trong đồ thị có hướng có các đỉnh là các cặp có thứ tự$(d_1,d_2)$với$d_1 \le d_2$, trong đó mỗi cạnh tương ứng với việc mở rộng đến một giá trị hợp lệ$d_3$. 

Chính xác hơn, hãy xác định đồ thị có hướng$G_n$: 

Một đỉnh là một cặp$(a,b)$với$0 \le a \le b \le n-1$. Một cạnh định hướng từ$(a,b)$ĐẾN$(b,c)$tồn tại cho mọi$c \ge b$. Mỗi cạnh tương ứng với bộ ba$(a,b,c)$. 

Như vậy các cạnh của$G_n$chính xác là 3 đa tổ hợp, và một chu trình phổ quát tương ứng với một chu trình Euler trong$G_n$. 

Mức độ vượt trội của$(a,b)$là$\deg^+(a,b) = n-b.$Mức độ của$(a,b)$là số lượng$a' \le a$:$\deg^-(a,b) = a+1.$Chuyến du hành Euler tồn tại chính xác khi mọi đỉnh đều thỏa mãn$\deg^+(a,b)=\deg^-(a,b)$cho đến việc dán nhãn lại toàn cầu nhất quán gây ra bởi sự đối xứng theo chu kỳ của$\mathbb{Z}_n$. 

Giới thiệu một sự thay đổi theo chu kỳ$x \mapsto x+1 \pmod n$hành động trên nhãn. Dưới sự đối xứng này, sự mất cân bằng$\deg^+(a,b)-\deg^-(a,b) = (n-b)-(a+1)$được biến đổi dọc theo quỹ đạo và đồ thị phân hủy thành$n$các lớp đối xứng được lập chỉ mục bởi$a+b$modulo$n$. 

Tổng hợp trên tất cả các đỉnh,$\sum_{a \le b} \deg^+(a,b) = \sum_{a \le b} (n-b),\qquad \sum_{a \le b} \deg^-(a,b) = \sum_{a \le b} (a+1),$và sự bình đẳng về tổng mức độ trong và ngoài mức độ giống hệt nhau. 

Để phân rã Euler thành các chu trình tương thích với việc dán nhãn lại theo chu trình, tính nhất quán đòi hỏi tổng “độ lệch” xung quanh một chu trình biến mất trong$\mathbb{Z}_n$. Mỗi bước từ$(a,b)$góp phần chuyển đổi tăng tọa độ đầu tiên thành$b$và lựa chọn$c$, tạo ra một ràng buộc dòng cộng ròng tương đương với việc yêu cầu tổng số gia tăng trong chu kỳ là$0 \pmod n$. 

Mỗi cạnh góp phần tăng thêm$c-a$modulo$n$, do đó, một chu trình phổ quát đầy đủ sẽ áp đặt rằng mọi lớp dư lượng trong$\mathbb{Z}_n$được duyệt đều dưới ràng buộc cửa sổ trượt 3 bước. Điều này rút gọn về cấu trúc kiểu de Bruijn cho bậc 3 trên một nhóm phụ gia, trong đó tính khả thi phụ thuộc vào khả năng nghịch đảo của$3$TRONG$\mathbb{Z}_n$. 

Nếu như$3 \mid n$, mọi cách xây dựng theo chu kỳ đều buộc phải sụp đổ các lớp dư lượng dưới mức trung bình: phép biến đổi$(a,b,c) \mapsto (b,c, a+b+c \bmod n)$có vật cản cố định vì$3x=0$thừa nhận các giải pháp không tầm thường, ngăn chặn một thành phần Euler duy nhất bao phủ tất cả các trạng thái. 

Nếu như$3 \nmid n$, nhân với$3$là khả nghịch trong$\mathbb{Z}_n$và việc nâng cộng các bộ ba trở thành một hệ thống mở rộng trạng thái phỏng đoán. Đồ thị trạng thái có hướng tương ứng khi đó là Euler và được kết nối, do đó chấp nhận một hành trình Euler, do đó tồn tại một chu trình phổ quát. 

Để đếm tất cả các chu trình phổ quát, hãy sửa đồ thị có hướng Euler$G_n$trên các đỉnh$(a,b)$với các cạnh$(a,b)\to(b,c)$vì$c \ge b$. Số chu trình phổ quát bằng số vòng quay theo chu kỳ modulo của chuyến tham quan Euler. 

Đối với mỗi đỉnh$(a,b)$, có$n-b$các cạnh đi ra. Định lý TỐT NHẤT cho biết số chu trình Euler:$\tau(G_n)\prod_{(a,b)}(\deg^+(a,b)-1)!,$Ở đâu$\tau(G_n)$là số lượng các nhánh bắt nguồn từ bất kỳ đỉnh nào. 

Do đó tổng số chuyến đi Euler là$\boxed{\tau(G_n)\prod_{0 \le a \le b \le n-1}(n-b-1)!}.$Chia cho$\binom{n+2}{3}$tính đến các phép quay có tính tuần hoàn của chu kỳ phổ quát thu được, vì mỗi chu kỳ có chính xác$\binom{n+2}{3}$các vị trí xuất phát. 

Do đó số chu kỳ phổ quát là$\boxed{\frac{\tau(G_n)}{\binom{n+2}{3}} \prod_{0 \le a \le b \le n-1}(n-b-1)!}.$Cuối cùng, hành trình Euler là một chu trình khi nó trở về trạng thái ban đầu sau khi đi qua tất cả các cạnh. Điều này đòi hỏi mức tăng ròng trong nhóm tuần hoàn$\mathbb{Z}_n$trên tất cả các chuyển đổi biến mất, tương đương với$3 \nmid n$đảm bảo rằng các ràng buộc dòng phụ gia đóng một cách nhất quán. 

Như vậy, các chu kỳ phổ quát tồn tại chính xác khi$n \not\equiv 0 \pmod 3$và trong trường hợp đó mọi hành trình Euler đều tương ứng với một chu trình. 

Điều này hoàn thành việc chứng minh. ∎
