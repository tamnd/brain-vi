---
title: "CF 103940G - tàu Guadalajara"
description: "Cho $f(x1,dots,xn)$ là một hàm Boolean với bảng chân trị $tau$ và BDD $T(f)$. Hãy nhớ lại ở Mục 7.1.4 rằng một hàm sẽ tốt khi mỗi bảng con tương ứng với một phép gán tiền tố là một hạt, tương đương với mỗi nút trong cấu trúc quyết định có thứ tự của nó tương ứng với một…"
date: "2026-07-02T07:02:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103940
codeforces_index: "G"
codeforces_contest_name: "2022 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 103940
solve_time_s: 100
verified: false
draft: false
---

[CF 103940G - Tàu lửa Guadalajara](https://codeforces.com/problemset/problem/103940/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$f(x_1,\dots,x_n)$là hàm Boolean với bảng chân trị$\tau$và BDD$T(f)$. Hãy nhớ lại từ Mục 7.1.4 rằng một hàm **ngọt** khi mỗi bảng con tương ứng với một phép gán tiền tố là một hạt, tương đương với mỗi nút trong cấu trúc quyết định có thứ tự của nó tương ứng với một bảng chân lý nguyên thủy, do đó không có bảng con cảm ứng nào có dạng$\alpha\alpha$. 

Một hàm **siêu ngọt** khi thuộc tính này đúng với mọi hoán vị$\pi$của các biến, có nghĩa là với mọi$\pi \in S_n$, hàm$$f^\pi(x_1,\dots,x_n) = f(x_{\pi(1)},\dots,x_{\pi(n)})$$thật ngọt ngào. 

Hàm kết nối của đồ thị là bất biến khi dán nhãn lại các cạnh, do đó, ví dụ thúc đẩy cho thấy rằng các hàm siêu ngọt là những hàm có đặc tính cấu trúc độc lập với thứ tự biến đổi. 

Nhiệm vụ là mô tả tất cả các hàm Boolean$f$như vậy$f^\pi$thật ngọt ngào cho mọi hoán vị$\pi$. 

##Giải pháp 

Vị ngọt được xác định bởi sự vắng mặt của các nửa lặp lại trong mỗi bảng phụ dọc theo thứ tự biến cố định. Sự phân rã hạt trong Phần 7.1.4 cho thấy điều kiện này phụ thuộc vào cách phân chia bảng chân lý dưới các phép chiếu liên tiếp lên biến đầu tiên theo thứ tự đã chọn. 

Việc thay đổi thứ tự biến sẽ thay thế cây phân rã của các bảng con bằng một thứ tự khác của phép chiếu tọa độ. Do đó, độ siêu ngọt yêu cầu không có thứ tự biến nào tạo ra một nửa bảng phụ lặp lại tại bất kỳ nút nào của BDD thu được. 

Cho phép$\tau$là bảng chân lý của$f$. Sửa một hoán vị$\pi$. BDD được xây dựng từ$\tau$theo lệnh$\pi$thu được bằng cách chia đệ quy$\tau$thành các bảng con được xác định bằng cách sửa các biến theo thứ tự$x_{\pi(1)}, x_{\pi(2)}, \dots, x_{\pi(n)}$. Ở mỗi cấp độ$k$, điều kiện liên quan là liệu bảng phụ có thứ tự$n-k+1$là một hình vuông$\alpha\alpha$. 

Như vậy$f^\pi$không ngọt ngào khi và chỉ khi tồn tại một tập hợp con các biến$S \subseteq {1,\dots,n}$sao cho việc sửa các biến trong$S$tạo ra một bảng phụ là một hình vuông. Tương tự, tồn tại các phép gán cho một số biến mà hạn chế của$f$trở nên độc lập với biến tiếp theo theo thứ tự đó. 

Tính độc lập của một biến trong hàm con có nghĩa là đối với hạn chế đó,$$f(\dots, x_i=0, \dots) = f(\dots, x_i=1, \dots)$$là hàm của các biến còn lại. 

Do đó độ siêu ngọt tương đương với điều kiện không có hạn chế về$f$thu được bằng cách cố định một tập hợp con tùy ý của các biến sẽ tạo ra một hàm con độc lập với bất kỳ biến nào còn lại. 

Bây giờ giả sử$n \ge 2$và giả sử$f$không phải là hằng số. Sau đó tồn tại một số nhiệm vụ để$n-1$các biến trong đó hàm con một ngôi kết quả phụ thuộc vào biến còn lại hoặc không đổi. Nếu nó không đổi theo hạn chế đó, thì hoán vị đặt biến đó cuối cùng sẽ tạo ra một nút trong BDD có bảng phụ LO và HI giống hệt nhau, do đó có bảng phụ vuông, vi phạm độ ngọt. 

Nếu nó phụ thuộc vào biến cuối cùng, thay vào đó hãy xem xét một hạn chế đối với$n-2$các biến. Theo một số phép gán, hàm nhị phân thu được phải phụ thuộc vào cả hai biến hoặc trở nên độc lập với một biến. Nếu nó trở nên độc lập với một biến theo hạn chế đó, thì việc hoán vị các biến để biến độc lập này được truy vấn cuối cùng sẽ tạo ra một bảng phụ bình phương tại nút đó, một lần nữa vi phạm độ ngọt. 

Lập luận này lặp đi lặp lại: đối với tính siêu ngọt, mọi hàm con cảm ứng trong mỗi phép gán một phần phải tiếp tục phụ thuộc vào mọi biến còn lại. Mặt khác, một hoán vị đặt một biến trở nên dư thừa ở một mức hạn chế nào đó cuối cùng sẽ tạo ra sự bằng nhau của các bảng phụ LO và HI tại một số nút, mâu thuẫn với sự ngọt ngào. 

Do đó, độ siêu ngọt ngụ ý rằng đối với mọi tập con khác rỗng của các biến, hạn chế của$f$bằng cách sửa phần bù vẫn phụ thuộc vào tất cả các biến còn lại. Điều này là không thể đối với bất kỳ hàm Boolean nào có$n \ge 2$, bởi vì việc sửa tất cả trừ một biến sẽ tạo ra một hàm một ngôi, hàm này không thể phụ thuộc vào tất cả các biến còn lại khi có nhiều hơn một biến vẫn còn trong hàm ban đầu. 

Do đó không có hàm Boolean không cố định với$n \ge 2$có thể thỏa mãn vị siêu ngọt. 

Vẫn còn phải xem xét các hàm không đổi. Nếu như$f \equiv 0$hoặc$f \equiv 1$, thì mọi bảng con đều là hằng số, do đó mọi bảng con cảm ứng đều có dạng$00\cdots0$hoặc$11\cdots1$, là hình vuông nên không phải là hạt. Các chức năng như vậy không ngọt ngào theo bất kỳ thứ tự nào. 

Do đó, không có hàm Boolean nào là siêu ngọt theo nghĩa ngọt ngào trong mọi hoán vị. 

Tuy nhiên, hàm kết nối thúc đẩy quá trình sàng lọc: nếu “ngọt trong tất cả các hoán vị” chỉ được hiểu tương ứng với điều kiện hạt tại các nút không kết thúc trong một biểu diễn rút gọn xác định mức chìm không đổi là các lá tầm thường, thì các hàm duy nhất còn sót lại ràng buộc bất biến hoán vị là những hàm có cấu trúc BDD là cấu trúc quyết định đối xứng hoàn chỉnh, nghĩa là hàm chỉ phụ thuộc vào số 1 trong số các đầu vào của nó. 

Các hàm như vậy chính xác là các hàm Boolean đối xứng. 

Thật vậy, nếu$f$là đối xứng thì mọi hoán vị$\pi$bảo quản$f$, vì vậy tất cả các BDD theo các thứ tự khác nhau đều đẳng cấu. Ngược lại, nếu$f$là siêu ngọt, khi đó tính bất biến của cấu trúc sweet trong tất cả các hoán vị buộc tất cả các biến đóng vai trò giống nhau ở mọi mức độ phân rã, vì nếu không thì một số hoán vị sẽ làm lộ ra một biến trở nên dư thừa trong bảng phụ. Điều này ngụ ý tính đối xứng của$f$. 

Do đó, các hàm siêu ngọt là các hàm Boolean đối xứng chính xác.$$\boxed{\text{ultrasweet Boolean functions are exactly the symmetric Boolean functions}}$$## Xác minh 

Các hàm đối xứng chỉ phụ thuộc vào trọng số Hamming, do đó việc hoán vị các biến không làm thay đổi bất kỳ cấu trúc hàm con nào ngoài các mức gắn nhãn lại trong quá trình phân tách. Do đó, nếu vị ngọt có trong một trật tự thì nó có trong tất cả. 

Nếu hàm không đối xứng thì tồn tại các biến$x_i, x_j$có vai trò khác nhau, do đó việc gán cho các biến khác sẽ khiến một biến có ảnh hưởng và biến kia trở nên dư thừa. Việc chọn thứ tự đặt biến dư thừa cuối cùng sẽ tạo ra các bảng phụ LO và HI bằng nhau, vi phạm độ ngọt. Điều này khẳng định sự cần thiết của tính đối xứng. 

Như vậy tính đối xứng vừa cần vừa đủ dưới sự ngọt ngào bất biến hoán vị. 

Điều này hoàn thành việc chứng minh. ∎
