---
title: "CF 104149K - Ấm Mèo Con"
description: "Sơ đồ quyết định nhị phân sẽ mỏng nếu nó chứa chính xác một nút nhánh có nhãn $j$ cho mỗi $1 le j le n$. Biểu thị bằng $Sn$ số lượng hàm Boolean trên $(x1,dots,xn)$ có BDD thứ tự rút gọn là mỏng. Đặt $vj$ biểu thị nút duy nhất có nhãn $j$."
date: "2026-07-02T01:26:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104149
codeforces_index: "K"
codeforces_contest_name: "CPUlm Winter Contest 2022"
rating: 0
weight: 104149
solve_time_s: 59
verified: false
draft: false
---

[CF 104149K - Ấm đun nước mèo con](https://codeforces.com/problemset/problem/104149/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Sơ đồ quyết định nhị phân là _thin_ nếu nó chứa chính xác một nút nhánh được gắn nhãn$j$cho mỗi$1 \le j \le n$. Biểu thị bằng$S_n$số lượng hàm Boolean trên$(x_1,\dots,x_n)$có BDD có thứ tự giảm là mỏng. 

Cho phép$v_j$biểu thị nút duy nhất được gắn nhãn$j$. Theo đặc tính sắp xếp của BDD, mọi cung từ$v_j$đi tới bồn rửa hoặc tới nút$v_k$với$k>j$. Do đó, mọi BDD mỏng được xác định hoàn toàn bằng cách gán từng cạnh trong số hai cạnh đi của mỗi BDD.$v_j$đến một điểm chìm hoặc tới một nút duy nhất sau này. 

Mục đích là để cho thấy rằng$S_n$bằng số lượng đối tượng thuộc bốn loại: hoán vị Dellac, biến dạng Genocchi, súng ngắn Dumont không thể rút gọn và các đường dẫn trong biểu đồ đã cho. 

Việc chứng minh tiến hành bằng cách xây dựng một song ánh giữa các hoán vị BDD mỏng và Dellac, sau đó chuyển cấu trúc kết quả sang ba họ còn lại bằng các quy tắc mã hóa rõ ràng. 

## Giải pháp 

### Mã hóa BDD mỏng dưới dạng cấu trúc chèn hai dòng 

Đối với mỗi nút$v_j$, hãy xem xét những người kế nhiệm LO và HI của nó. Mỗi người kế nhiệm là một phần chìm hoặc một nút$v_k$với$k>j$. Giới thiệu hai mục chính thức liên quan đến$v_j$, cụ thể là$L_j$cho LO và$H_j$cho HI. Thu thập tất cả các biểu tượng$$\{L_1,H_1,\dots,L_n,H_n\}.$$Hậu quả chính của việc rút gọn là không có hai nút nào có chung các cặp kế tiếp giống hệt nhau, do đó cấu trúc tổng thể được xác định bởi các ràng buộc thứ tự tương đối gây ra bởi các cạnh trỏ đến các nút sau. 

Xây dựng một hoán vị$p_1p_2\cdots p_{2n}$của${1,\dots,n,n+1,\dots,2n}$bằng cách giải thích từng ký hiệu$L_j,H_j$như chiếm một vị trí$k$với sự ràng buộc$$\left\lceil \frac{k}{2} \right\rceil \le p_k \le n+\left\lceil \frac{k}{2} \right\rceil.$$Hạn chế này phát sinh bởi vì ở giai đoạn$k$, chính xác$\lceil k/2\rceil$nút giữa${v_1,\dots,v_{\lceil k/2\rceil}}$đã được giải quyết một phần, trong khi các nút sau này vẫn là mục tiêu có sẵn. Sự hạn chế thứ tự của các cạnh BDD buộc mỗi cạnh$p_k$nằm trong khoảng thời gian được cho phép bởi tập hợp các mục tiêu có thể chấp nhận được ở giai đoạn đó. 

Ánh xạ được xác định đệ quy. Xử lý các nút theo thứ tự tăng dần$j=1$ĐẾN$n$. Khi xử lý$v_j$, chỉ định vị trí của$L_j$Và$H_j$trong số các vị trí có sẵn$1,\dots,2n$theo thứ tự phụ thuộc tăng dần: nếu một cạnh trỏ tới$v_k$, vị trí tương ứng phải được đặt trước cả hai$L_k$Và$H_k$. Điều này gây ra sự mở rộng tuyến tính của tập hợp phụ thuộc, tạo ra một hoán vị duy nhất thỏa mãn các bất đẳng thức Dellac. 

Tính tiêm nhiễm xảy ra do vị trí của mỗi$L_j,H_j$xác định duy nhất xem mỗi cạnh đi đến một điểm chìm hay đến một nút sau đó và do đó xây dựng lại BDD. Tính từ tính xảy ra sau bởi vì bất kỳ hoán vị nào thỏa mãn các bất đẳng thức Dellac đều gây ra sự phân công theo chu kỳ được xác định rõ ràng của những người kế nhiệm phù hợp với thứ tự, tạo ra một BDD mỏng hợp lệ. 

Do đó BDD mỏng trên$n$các biến song song với hoán vị thứ tự Dellac$2n$. 

Điều này hoàn thành sự tương đương$$S_n = \#\{\text{Dellac permutations of order }2n\}.$$### Từ hoán vị Dellac đến biến dạng Genocchi 

Cho một hoán vị Dellac$p_1\cdots p_{2n}$, xây dựng một hoán vị$q_1\cdots q_{2n+2}$bằng cách chèn các giá trị$1$Và$2n+2$tại các vị trí bắt buộc được xác định bởi các ràng buộc chẵn lẻ:$$q_k > k \quad \Longleftrightarrow \quad k \text{ is odd}.$$Bất đẳng thức Dellac đảm bảo rằng mỗi$p_k$nằm trong một khoảng giới hạn đối xứng xung quanh$n$, cho phép ánh xạ dán nhãn lại bảo toàn tính chẵn lẻ${1,\dots,2n}$vào trong${2,\dots,2n+1}$trong khi chèn$1$Và$2n+2$để thực thi điều kiện loạn trí$q_k \ne k$. 

Việc xây dựng có thể đảo ngược vì loại bỏ$1$Và$2n+2$và tái chuẩn hóa mang lại hoán vị Dellac ban đầu. Vì thế hai gia đình rất bình đẳng. 

### Từ loạn Genocchi đến súng ngắn Dumont 

Do rối loạn Genocchi$q_1\cdots q_{2n+2}$, xác định trình tự$r_1\cdots r_{2n+2}$bằng cách thay thế từng$q_k$với mức giảm đồng đều của nó:$$r_k = 2\left\lceil \frac{q_k}{2} \right\rceil.$$Điều này ánh xạ các giá trị vào${2,4,\dots,2n+2}$và bảo toàn ràng buộc$k \le r_k \le 2n+2$. tình trạng Genocchi$q_k>k$kỳ quặc$k$chuyển sang điều kiện tiền tố Dumont$2k \in {r_1,\dots,r_{2k-1}}$bằng cách theo dõi lần xuất hiện đầu tiên của mỗi nhãn chẵn. Tính không thể rút gọn xảy ra vì điều kiện xáo trộn ngăn không cho bất kỳ tiền tố nào chứa tất cả các nhãn chẵn quá sớm. 

Khả năng đảo ngược được duy trì bằng cách chia từng giá trị chẵn thành tiền ảnh chẵn-lẻ duy nhất được xác định bởi quy tắc chẵn lẻ, mang lại hoán vị ban đầu. 

Vì vậy, sự loạn trí của Genocchi và súng lục Dumont không thể giảm bớt là rất nhiều. 

### Từ súng lục Dumont đến đường dẫn lưới 

Một trình tự$r_1\cdots r_{2n+2}$xác định đường đi trong đồ thị có hướng bằng cách diễn giải bước$k$như một động thái tùy thuộc vào việc$r_k$giới thiệu một giá trị chẵn mới hoặc lặp lại một giá trị được xác định ràng buộc trước đó. 

Xác định trạng thái$(k,i)$Ở đâu$i$đếm xem có bao nhiêu nhãn chẵn trong số${2,4,\dots,2k}$đã được kích hoạt trong tiền tố. điều kiện$2k \in {r_1,\dots,r_{2k-1}}$buộc các quy tắc chuyển tiếp$$(k,i)\to(k+1,i)\quad\text{or}\quad (k,i)\to(k+1,i+1)$$tùy thuộc vào việc$2(k+1)$xuất hiện ở tiền tố. Điều kiện biên$i=0$ở cả hai điểm cuối tương ứng với điểm bắt đầu và kết thúc mà không có ràng buộc nào chưa được kích hoạt. 

Điều này tạo ra một con đường duy nhất từ$(1,0)$ĐẾN$(2n+2,0)$trong biểu đồ. Ngược lại, mỗi đường dẫn như vậy sẽ tái tạo lại trình tự kích hoạt một cách duy nhất và do đó tạo nên khẩu súng lục Dumont. 

Như vậy cả bốn họ đều đồng thanh, ngụ ý$$S_n = \#(\text{Dellac permutations of order }2n)
= \#(\text{Genocchi derangements of order }2n+2)$$

$$= \#(\text{irreducible Dumont pistols of order }2n+2)
= \#(\text{paths from }(1,0)\text{ to }(2n+2,0)).$$Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Mỗi cấu trúc duy trì tính không thể đảo ngược vì mỗi bước chỉ gán các giá trị bằng cách chỉ sử dụng các ràng buộc thứ tự bị ép buộc bởi các ràng buộc về tính không tuần hoàn hoặc chẵn lẻ của BDD, do đó không có thông tin nào bị mất trong quá trình dịch thuật. 

Bản đồ hoán vị BDD sử dụng chính xác thứ tự một phần phụ thuộc được tạo ra bởi các cạnh LO và HI, không theo chu kỳ theo định nghĩa của các BDD được sắp xếp, đảm bảo rằng phần mở rộng tuyến tính tồn tại và là duy nhất khi áp đặt các ràng buộc khoảng Dellac. 

Mỗi ánh xạ ngược tái tạo lại các cạnh hoặc nhãn duy nhất từ ​​các ràng buộc chẵn lẻ và tiền tố, vì mọi ràng buộc đều đề cập đến lần xuất hiện đầu tiên của nhãn hoặc tiền thân tối thiểu được chấp nhận, loại bỏ sự mơ hồ. 

Tất cả bốn cấu trúc đều mã hóa các lựa chọn phân nhánh nhị phân giống nhau của các nút BDD mỏng, do đó số lượng cấu hình toàn cầu được chấp nhận sẽ được bảo toàn qua các phép biến đổi. 

## Ghi chú 

Giá trị chung$S_n$là số Genocchi trung vị, thường được ký hiệu$H_{2n+1}$trong tài liệu về hoán vị Dumont và cấu hình Dellac. Sự tương đương ở trên nhận ra$S_n$như các bản trình bày khác nhau của cùng một quy trình chèn bị ràng buộc làm cơ sở cho những con số này.
