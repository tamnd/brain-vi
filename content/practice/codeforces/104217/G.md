---
title: "CF 104217G - Hành trình tới Nome"
description: "Giả sử BDD đã cho của $f(x1,dots,xn)$ là một biểu đồ chu kỳ có hướng gốc có các nút nhánh được gắn nhãn bởi các biến $V(u)in{1,dots,n}$ và có các nút chìm là $bot,top$, với tính thứ tự đảm bảo rằng dọc theo mọi đường dẫn có hướng, các nhãn sẽ tăng nghiêm ngặt."
date: "2026-07-01T23:54:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104217
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 2 (Beginner)"
rating: 0
weight: 104217
solve_time_s: 30
verified: false
draft: false
---

[CF 104217G - Hành trình đến Nome](https://codeforces.com/problemset/problem/104217/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Đặt BDD đã cho cho$f(x_1,\dots,x_n)$là một biểu đồ tuần hoàn có hướng gốc có các nút nhánh được gắn nhãn bởi các biến$V(u)\in{1,\dots,n}$và các nút chìm của nó là$\bot,\top$, với tính trật tự đảm bảo rằng dọc theo mọi đường dẫn có hướng, các nhãn sẽ tăng lên một cách chặt chẽ. 

Một vectơ$(x_1,\dots,x_n)$thỏa mãn$f(x_1,\dots,x_n)=1$chính xác khi đường dẫn đánh giá được xác định bằng cách bắt đầu từ gốc và theo cạnh LO để tìm giá trị$0$và cạnh HI cho giá trị$1$kết thúc tại$\top$. 

Để tạo ra tất cả các vectơ như vậy, chỉ cần duyệt qua tất cả các vectơ từ gốc tới$\top$đường dẫn trong BDD trong khi tính toán các biến không được kiểm tra dọc theo đường dẫn. Nếu một đường dẫn đến một nút có nhãn$V(u)=j$sau khi có biến cố định gần đây nhất$x_k$với$k<j-1$, sau đó biến$x_{k+1},\dots,x_{j-1}$không bị ràng buộc bởi cấu trúc BDD và có thể được chọn tùy ý. 

Xác định một thủ tục đệ quy$\mathrm{ENUM}(u,k,a_1,\dots,a_k)$, Ở đâu$u$là một nút BDD,$k$là chỉ số biến được gán cuối cùng và$(a_1,\dots,a_k)$là vectơ từng phần hiện tại. 

Tại một nút đích, quy trình hoạt động như sau. Nếu như$u=\bot$, không thể hoàn thành và thủ tục trả về mà không có đầu ra. Nếu như$u=\top$, thì tất cả sự hoàn thành của$(a_1,\dots,a_k)$đến một$n$-tuple là kết quả đầu ra hợp lệ, do đó thủ tục xuất ra mọi vectơ$(a_1,\dots,a_n)$với$a_{k+1},\dots,a_n\in{0,1}$. 

Tại một nút nhánh$u$với$V(u)=j$, các biến$x_{k+1},\dots,x_{j-1}$vẫn chưa được phân công. Do đó, quy trình trước tiên liệt kê tất cả$2^{j-k-1}$gán cho các biến này, mở rộng tiền tố hiện tại thành$(a_1,\dots,a_k,b_{k+1},\dots,b_{j-1})$với mỗi$b_i\in{0,1}$. Sau lần mở rộng này, nó tiếp tục giảm BDD bằng cách gán$x_j=0$và gọi$\mathrm{ENUM}(\mathrm{LO}(u),j,\dots)$, và gán riêng$x_j=1$và gọi$\mathrm{ENUM}(\mathrm{HI}(u),j,\dots)$. 

Cuộc gọi ban đầu là$\mathrm{ENUM}(\mathrm{ROOT},0)$với tiền tố trống. 

Tính đúng đắn bắt nguồn từ tính bất biến ở mỗi lần gọi$\mathrm{ENUM}(u,k,a_1,\dots,a_k)$tiền tố mã hóa chính xác các biến$x_1,\dots,x_k$và mọi phần mở rộng của tiền tố này tương ứng với một đường dẫn duy nhất trong BDD phù hợp với ràng buộc thứ tự. Tính thứ tự đảm bảo rằng không có biến nào được xem lại sau khi rời khỏi một nút, vì vậy mỗi phép gán được xác định chính xác một lần bằng các lựa chọn tại các nút nhánh cùng với các lựa chọn tự do trên các chỉ mục bị bỏ qua. Việc giảm BDD đảm bảo rằng không có nút trùng lặp giả mạo nào tạo ra các cấu trúc con không nhất quán trùng lặp, vì các sơ đồ con giống hệt nhau được chia sẻ và do đó tạo ra các tập hợp tiếp tục giống hệt nhau. 

Nếu như$(x_1,\dots,x_n)$thỏa mãn$f=1$, đường dẫn đánh giá của nó đạt đến$\top$tại một nút nào đó$u$và đệ quy nhất thiết phải tạo ra nó khi thủ tục đạt đến$u$và mở rộng các biến không bị ràng buộc chính xác như trên. Ngược lại, bất kỳ vectơ nào được tạo bởi thủ tục đều đi theo một đường dẫn đến$\top$, vì đầu ra chỉ xảy ra từ bộ thu$\top$và tất cả các chuyển đổi trước đó đều tôn trọng ngữ nghĩa LO và HI của đánh giá BDD. Do đó thủ tục đưa ra chính xác tập hợp các bài tập thỏa mãn. 

Điều này hoàn thành việc chứng minh. ∎
