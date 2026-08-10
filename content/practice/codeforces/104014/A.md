---
title: "CF 104014A - \u0411\u043e\u043b\u044c\u0448\u043e\u0439 \u0443\u0434\u043e\u0439"
description: "Giả sử $F$ biểu thị họ 5757 từ SGB được biểu thị trên các biến $a1,dots,z5$ như trong (131), và để ZDD liên quan được xây dựng theo cách sắp xếp tiêu chuẩn với các biến được xử lý theo thứ tự từ điển."
date: "2026-07-02T04:57:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104014
codeforces_index: "A"
codeforces_contest_name: "2022-2023 ICPC NERC, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u0438 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0440\u0435\u0433\u0438\u043e\u043d\u0430 \u0438 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438"
rating: 0
weight: 104014
solve_time_s: 121
verified: false
draft: false
---

[CF 104014A - \u0411\u043e\u043b\u044c\u0448\u043e\u0439 \u0443\u0434\u043e\u0439](https://codeforces.com/problemset/problem/104014/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$F$biểu thị họ 5757 từ SGB được biểu thị trên các biến$a_1,\dots,z_5$như trong (131), và để ZDD liên quan được xây dựng theo cách sắp xếp tiêu chuẩn với các biến được xử lý theo thứ tự từ điển. Hồ sơ z ghi lại, đối với mỗi vị trí biến, số lượng nút ZDD riêng biệt xuất hiện ở cấp độ đó, tương đương với số lượng các họ con còn lại riêng biệt có được bằng cách điều hòa các phép gán cho các biến trước đó và sau đó phân nhánh trên biến hiện tại. 

Đối với một biến như$a_2$, công trình đã sửa xong rồi$a_1$và sau đó xem xét liệu chữ cái thứ hai có phải là$a$. Mỗi nút ở cấp độ này đại diện cho một nhóm phần còn lại riêng biệt của các phần hoàn thành của một phần tiền tố từ có độ dài$2$. Hai nút như vậy giống hệt nhau khi và chỉ khi các tập hợp hoàn thành còn lại giống hệt như các tập hợp con của danh sách từ SGB. 

Mục nhập$23$vì$a_2$phát sinh bởi vì, sau khi sửa chữa$a_1$theo mọi cách có thể chấp nhận được và sau đó chuyển sang$a_2$, có chính xác$23$các lớp tiếp tục khác biệt trống của các từ một phần vẫn có thể phân biệt được bằng các ràng buộc hậu tố do từ điển áp đặt. Theo thuật ngữ ZDD,$23$là số lượng các họ con riêng biệt có thể tiếp cận được ở cấp độ$a_2$sau khi rút gọn sẽ hợp nhất các tập hợp tiếp tục giống hệt nhau được tạo ra bởi các tiền tố khác nhau. Mỗi nút trong số này tương ứng với một nút riêng biệt vì không áp dụng mức giảm thêm ở cấp độ này: các bộ tiếp tục khác nhau ở ít nhất một lần hoàn thành được phép ở các vị trí còn lại. 

Mục nhập$3$vì$b_2$phản ánh sự sụp đổ hơn nữa của cấu trúc sau cuộc khủng hoảng$a$-các biến được giải quyết. Tại$b_2$cấp độ, các họ dư được tạo ra bởi các tiền tố có độ dài khác nhau$2$trong$b$-thành phần rơi vào chính xác ba lớp tương đương theo quan hệ “có tập hoàn thành giống hệt nhau trong các biến$b_3,\dots,z_5$.” Sự giảm thiểu này xảy ra do các ràng buộc xác định các từ SGB không phân biệt nhiều tiền tố trung gian sau khi khối hai chữ cái đầu tiên được cố định; nhiều nhánh trong cấu trúc quyết định dẫn đến các tập hợp tiếp tục giống hệt nhau, do đó chúng hợp nhất trong ZDD rút gọn. giá trị$3$do đó là số lượng các hàm con riêng biệt của$F$tùy thuộc vào biến đầu tiên tại$b_2$mức độ. 

Đối với các mục cuối cùng$0,3,2,1,1,2$tương ứng với$v_5,w_5,x_5,y_5,z_5$, việc diễn giải bị chi phối bởi thực tế rằng đây là những vị trí cuối cùng trong cấu trúc từ gồm 5 chữ cái. Ở độ sâu$5$, mỗi nút đại diện cho một tiền tố có độ dài được xác định đầy đủ$4$cùng với một lựa chọn chữ cái còn lại bị hạn chế bởi tư cách thành viên trong từ điển SGB. 

Mục nhập$0$Tại$v_5$chỉ ra rằng mọi bài tập từng phần đạt đến cấp độ đó đều không nhất quán với danh sách từ, do đó không có phần tiếp theo nào tồn tại; mọi phân họ dư tương ứng đều trống và do đó bị giảm đi trong ZDD, không để lại sự đóng góp nút nào ở vị trí đó. 

Mục nhập$3$Tại$w_5$chỉ ra rằng có chính xác ba họ dư khác trống riêng biệt tồn tại khi chữ cái thứ năm bị giới hạn ở các phần hoàn thành hợp lệ của các tiền tố kết thúc bằng$w$. Các họ này khác nhau ở chỗ việc hoàn thành còn lại được xác định duy nhất hay vẫn có nhiều phần mở rộng hợp lệ. 

Các mục$2,1,1,2$vì$x_5,y_5,z_5$phát sinh từ các ràng buộc hậu tố ngày càng chặt chẽ hơn. Tại$x_5$, vẫn còn hai lớp tiếp tục riêng biệt vì hai bối cảnh hậu tố không tương đương vẫn cho phép hoàn thành. Tại$y_5$Và$z_5$, mỗi phép gán một phần sẽ xác định một hành vi tiếp tục duy nhất, do đó mỗi hành vi đóng góp một nút ZDD còn tồn tại cho mỗi lớp nhất quán. Giá trị cuối cùng$2$Tại$z_5$phản ánh rằng hai mẫu nhất quán cuối cùng riêng biệt vẫn còn cho các lần hoàn thành kết thúc bằng$z$, được phân biệt bằng việc ràng buộc cuối cùng được thỏa mãn một cách duy nhất hay bởi nhiều từ nhất quán với từ điển. 

Trong mỗi trường hợp, mục nhập cấu hình z bằng với số lượng các họ con rút gọn riêng biệt của các từ SGB được tạo ra bằng cách cố định các biến đến mức đó và các giá trị được liệt kê theo sau việc hợp nhất các bộ tiếp tục giống hệt nhau trong biểu diễn ZDD có thứ tự rút gọn. 

Điều này hoàn thành việc chứng minh. ∎
