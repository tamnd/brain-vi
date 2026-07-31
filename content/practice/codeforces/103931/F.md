---
title: "CF 103931F - Khu rừng ma thuật"
description: "Một từ mã Morse có độ dài $n$ là một chuỗi trên bảng chữ cái ${cdot, -}$ trong đó mỗi dấu chấm đóng góp trọng lượng $1$ và mỗi dấu gạch ngang đóng góp trọng lượng $2$ và tổng trọng lượng chính xác là $n$."
date: "2026-07-02T07:17:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "F"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 30
verified: false
draft: false
---

[CF 103931F - Khu rừng ma thuật](https://codeforces.com/problemset/problem/103931/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

### Phần (a) 

Một từ mã Morse có độ dài$n$là một dãy trên bảng chữ cái${\cdot, -}$trong đó mỗi dấu chấm đóng góp trọng lượng$1$và mỗi dấu gạch ngang đóng góp trọng lượng$2$, và tổng trọng lượng chính xác là$n$. Điều này tương đương với thành phần của$n$thành nhiều phần$1$Và$2$, trong đó dấu chấm mã hóa$1$và một dấu gạch ngang mã hóa$2$. 

Xác định một đồ thị có các đỉnh đều là những từ có trọng số như vậy$n$. Hai từ liền kề nếu một từ có thể được lấy từ từ kia bằng cách thay dấu gạch ngang bằng hai dấu chấm liên tiếp hoặc thay hai dấu chấm liên tiếp bằng dấu gạch ngang. Mỗi lần di chuyển sẽ bảo toàn tổng trọng lượng, vì$2 \leftrightarrow 1+1$. 

Nhiệm vụ là xây dựng mã Gray trên tập đỉnh này, nghĩa là đường đi Hamilton trong đó các từ liên tiếp khác nhau đúng một thay thế cục bộ như vậy. 

Chúng tôi xây dựng trình tự đệ quy. Cho phép$G(n)$biểu thị thứ tự của tất cả các từ Morse có trọng lượng$n$. 

Vì$n=0$có một từ trống rỗng. Vì$n=1$chỉ có$\cdot$, Vì thế$G(1)=\cdot$. 

Vì$n \ge 2$, từng lời có trọng lượng$n$kết thúc bằng$\cdot$hoặc trong$-$. Điều này tạo ra sự phân chia tập hợp các từ thành hai lớp: 

những từ kết thúc bằng$\cdot$tương ứng với các từ có trọng lượng$n-1$bằng cách xóa dấu chấm cuối cùng và các từ kết thúc bằng$-$tương ứng với các từ có trọng lượng$n-2$bằng cách xóa dấu gạch ngang cuối cùng. 

Điều này mang lại một sự bijection$$\{\text{words of weight } n \text{ ending in } \cdot\} \leftrightarrow G(n-1),$$

$$\{\text{words of weight } n \text{ ending in } -\} \leftrightarrow G(n-2).$$Chúng tôi xác định đệ quy$$G(n) = \bigl(\cdot \, G(n-1)\bigr) \;\; \text{concatenated with} \;\; \bigl(- \, G(n-2)\bigr)^{R},$$Ở đâu$(\cdot,G(n-1))$có nghĩa là tiền tố$\cdot$đến từng từ trong$G(n-1)$, Và$( -,G(n-2))^R$có nghĩa là tiền tố$-$đến từng từ trong$G(n-2)$và sau đó đảo ngược thứ tự của danh sách. 

Điều này tạo ra tất cả các từ có trọng lượng$n$vì mỗi từ rơi vào một trong hai trường hợp duy nhất tùy thuộc vào ký hiệu cuối cùng của nó. 

Chuyển tiếp liên tiếp bên trong$\cdot,G(n-1)$duy trì tính kề cận bằng quy nạp, vì việc loại bỏ dấu chấm cuối cùng sẽ làm giảm cả hai từ thành các phần tử liền kề trong$G(n-1)$và gắn lại dấu chấm sẽ giữ nguyên cấu trúc di chuyển được phép. 

Điều tương tự cũng xảy ra bên trong$-;G(n-2)^R$bằng lập luận quy nạp tương tự được áp dụng cho$G(n-2)$theo thứ tự ngược lại. 

Nó vẫn còn để xác minh điểm nối giữa hai khối. Lời cuối cùng của$\cdot,G(n-1)$là$\cdot$đặt trước từ cuối cùng của$G(n-1)$, thu được bằng cách đệ quy bằng cách xen kẽ các ký hiệu kết thúc theo cấu trúc. Lời đầu tiên của$-;G(n-2)^R$là$-$đặt trước từ cuối cùng của$G(n-2)$. Hai từ này khác nhau chính xác ở vùng cuối trong đó một dấu chấm trong cấu trúc thứ nhất được thay thế bằng dấu gạch ngang trong cấu trúc thứ hai, tương ứng với phép biến đổi được phép$\cdot\cdot \leftrightarrow -$áp dụng tại vị trí ranh giới. Do đó tính kề giữ ở điểm nối. 

Điều này thiết lập mã Gray tạo ra tất cả các từ Morse có độ dài$n$. 

### Phần (b) 

Chuỗi hiển thị,$$q\ q\ q\ q\ q,$$đại diện cho từ bao gồm 15 dấu chấm, vì mỗi dấu chấm$q$tương ứng với một dấu chấm và không có nhóm dấu gạch ngang nào. 

Trong trật tự Gray xây dựng ở phần (a), từ đầu tiên là từ có dấu chấm$\cdot^{15}$và thay đổi tiếp theo xảy ra bằng cách áp dụng phép biến đổi được chấp nhận đầu tiên ở vị trí ngoài cùng bên phải nơi hai dấu chấm liên tiếp có thể được thay thế bằng dấu gạch ngang. 

Cặp dấu chấm ngoài cùng bên phải trong$\cdot^{15}$đang ở vị trí$14$Và$15$, do đó, từ tiếp theo có được bằng cách thay thế hai dấu chấm này bằng dấu gạch ngang. 

Như vậy, chữ kế tiếp là từ gồm 13 dấu chấm, theo sau là một dấu gạch ngang:$$\underbrace{\cdot\cdots\cdots}_{13}\ -.$$### Ghi chú 

Cấu trúc là khối Fibonacci Mã xám ngụy trang: Từ trọng lượng Morse$n$tương ứng với các ô có chiều dài-$n$phân đoạn theo kích thước gạch$1$Và$2$và đệ quy phân chia theo ô cuối cùng. Sự đảo ngược trong khối thứ hai là cơ chế tiêu chuẩn đảm bảo sự thay đổi một bit (ở đây là thay thế cục bộ) khi nối.
