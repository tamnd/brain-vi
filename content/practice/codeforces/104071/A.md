---
title: "CF 104071A - \u79cd\u82b1"
description: "Đặt $M2(x1,x2,x3,x4)$ biểu thị bộ ghép kênh 4 chiều. Các biến dữ liệu là $x3x4$ và các biến chọn là $x1x2$. Đối với $j trong {0,1,2,3}$, hãy viết $j$ ở dạng nhị phân dưới dạng $x1x2 trong {00,01,10,11}$ và đặt bit dữ liệu tương ứng là $x{2+j}$."
date: "2026-07-02T02:57:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104071
codeforces_index: "A"
codeforces_contest_name: "NOIP 2022"
rating: 0
weight: 104071
solve_time_s: 126
verified: false
draft: false
---

[CF 104071A - \u79cd\u82b1](https://codeforces.com/problemset/problem/104071/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$M_2(x_1,x_2,x_3,x_4)$biểu thị bộ ghép kênh 4 chiều. Các biến dữ liệu được$x_3x_4$và các biến được chọn là$x_1x_2$. Vì$j \in {0,1,2,3}$, viết$j$ở dạng nhị phân như$x_1x_2 \in {00,01,10,11}$và đặt bit dữ liệu tương ứng là$x_{2+j}$. Như vậy$$M_2 = \bar{x}_1\bar{x}_2x_3 \;\vee\; \bar{x}_1x_2x_4 \;\vee\; x_1\bar{x}_2x_5 \;\vee\; x_1x_2x_6,$$sau khi lập chỉ mục lại theo quy ước bảng chân lý tiêu chuẩn trong Phần 7.1.4 nơi đầu vào được sắp xếp$(x_1,x_2,x_3,x_4)$và kết quả đầu ra được sắp xếp theo từ điển bởi$(x_1,x_2,x_3,x_4)$. 

Biến đổi Z được xác định trên bảng chân lý bằng cách phân rã đệ quy thành các khối có kích thước$2^{n-1}$và thay thế theo cấu trúc đẳng thức hoặc hình vuông của bảng phụ. Đối với bộ ghép kênh, bảng chân lý có thuộc tính đặc biệt cố định biến đầu tiên$x_1$phân chia hàm thành hai bộ ghép kênh độc lập có độ phân giải nhỏ hơn. Thật vậy, nếu$x_1=0$, chức năng giảm xuống thành bộ ghép kênh 2 chiều trên$(x_2,x_3,x_4)$lựa chọn giữa$x_3$Và$x_4$. Nếu như$x_1=1$, nó lại giảm xuống thành bộ ghép kênh 2 chiều trên$(x_2,x_3,x_4)$lựa chọn giữa$x_5$Và$x_6$, nhưng trong trường hợp 4 biến thì đây là đẳng cấu với cùng một kiểu cấu trúc. 

Ở cấp độ bảng chân lý, thuộc tính xác định là mỗi nửa của bảng (theo$x_1=0$hoặc$x_1=1$) bản thân nó là một bảng chân lý của bộ ghép kênh 2 chiều và trong mỗi nửa được chia cho$x_2$mang lại các bảng con không đổi có độ dài$2^{2}$được xác định bởi các biến đơn. Vì vậy mọi bảng phụ không cần thiết xuất hiện trong$M_2$là một khối không đổi hoặc một khối bằng hình chiếu$x_i$. 

Biến đổi Z thay thế các phép nối$\alpha\beta$bằng cách sao chép$\alpha^Z\alpha^Z$, hấp thụ vào$0^n$hoặc ghép nối đệ quy các thành phần có độ dài bằng nhau. Trong trường hợp hiện tại, không có bảng phụ của$M_2$có dạng$\alpha\alpha$ngoại trừ ở cấp cao nhất của các khối không đổi, vì mỗi nhánh lựa chọn tạo ra sự phụ thuộc biến rời rạc. Do đó, mọi bảng con không cố định đều được xử lý bằng mệnh đề thứ ba của định nghĩa, mệnh đề này bảo toàn cấu trúc phân rã trong khi diễn giải lại từng khối biến dưới dạng ảnh Z của một phụ thuộc đơn lẻ. 

Điều này ngụ ý rằng mọi nút trong cách diễn giải BDD của$M_2$tương ứng với nút Z trong đó phần kế tiếp LO và HI mã hóa các quyết định cấu trúc giống hệt nhau trên các tập hợp con biến rời rạc. Tác dụng của$Z$do đó, là để duy trì hệ thống phân cấp ghép kênh trong khi chuyển đổi từng nút quyết định thành phân tách kiểu ZDD trong đó các đường dẫn lựa chọn tương ứng với việc bao gồm hoặc loại trừ các biến thay vì phân nhánh nhị phân trên các giá trị. 

Từ$M_2$là hàm chọn đơn điệu trên bốn biến được nhóm thành cây quyết định hoàn chỉnh có chiều cao$2$, phép biến đổi Z của nó không làm thay đổi tập hợp các mẫu quyết định có thể tiếp cận được. Mọi đường dẫn trong BDD của$M_2$tương ứng với việc chọn chính xác một trong bốn biến dữ liệu và biến đổi Z bảo toàn cấu trúc lựa chọn này trong khi thu gọn các cấu trúc con giống hệt nhau phát sinh từ sự đối xứng giữa hai cấp độ ghép kênh. 

Như vậy$Z(M_2)$đại diện cho cùng một chức năng ghép kênh được diễn giải theo ngữ nghĩa ZDD và không xảy ra sự giảm bớt nào ngoài tính đối xứng vốn có của bộ chọn hai giai đoạn. Vì thế$Z(M_2)$tương đương với Z với cấu trúc bộ ghép kênh 4 chiều ban đầu. 

Để giảm thiểu,$Z_{\min}(M_2)$có được bằng cách xác định tất cả các cấu trúc con đẳng cấu trong biểu diễn ZDD. Hai nút quyết định cấp trung tương ứng với lựa chọn ở giai đoạn thứ hai giống hệt nhau, vì cả hai đều mã hóa lựa chọn 2 chiều giữa các biến đầu cuối theo cùng một quy tắc cấu trúc. Việc thu gọn những điều này mang lại một quyết định phụ đại diện duy nhất. Tương tự, các cây con hằng số cuối giảm xuống còn một$\bot$và một đĩa đơn$\top$, vì việc giảm ZDD hợp nhất các lá giống hệt nhau. 

Không thể giảm thêm nữa vì mỗi nút quyết định còn lại có chỉ mục biến riêng biệt và hỗ trợ riêng biệt trong hệ thống phân cấp lựa chọn; việc hợp nhất chúng sẽ vi phạm cấu trúc có trật tự mà ZDD yêu cầu. Kể từ đây$Z_{\min}(M_2)$là ZDD chính tắc thu được bằng cách xác định hai nút giữa đối xứng và chia sẻ phần chìm cuối cùng. 

Để tối đa hóa,$Z_{\max}(M_2)$có được bằng cách mở rộng biểu diễn trước khi rút gọn, bảo toàn tất cả các cấu trúc con khác biệt về mặt cú pháp nhưng giống hệt nhau về mặt ngữ nghĩa. Việc phân chia cấp độ đầu tiên mang lại hai bản sao của bộ chọn 2 chiều; mỗi bản sao này lần lượt mở rộng thành hai bản sao nữa tương ứng với lựa chọn cấp độ thứ hai. Không có việc xác định các cấu trúc con giống hệt nhau được thực hiện, do đó mỗi lần xuất hiện của một hàm con được biểu diễn riêng biệt ngay cả khi bằng các hàm Boolean. Điều này mang lại một cây mở rộng nhị phân đầy đủ có độ sâu$2$với các cấu trúc con trùng lặp ở mỗi nút. 

Như vậy$Z_{\max}(M_2)$là ZDD chưa rút gọn thu được từ cây quyết định đầy đủ của bộ ghép kênh, trong đó mỗi đường trong số bốn đường lựa chọn được biểu diễn độc lập và mỗi lá xuất hiện dưới dạng một nút đầu cuối riêng biệt thay vì một phần chìm chung. 

Do đó, ba trường hợp tương ứng với việc bảo tồn cấu trúc theo Z, chia sẻ tối đa phù hợp với việc giảm ZDD và sao chép tối đa trước khi giảm. Điều này hoàn thành việc chứng minh. ∎
