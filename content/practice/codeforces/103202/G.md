---
title: "CF 103202G - Cây phù thủy"
description: "Giả sử $G{s,t}$ biểu thị đồ thị có các đỉnh là tất cả các khối con có độ dài $s+t$ có $s$ chữ số trong các dấu sao ${0,1}$ và $t$, với các cạnh được cho bởi các phép biến đổi $ast 0 leftrightarrow 0ast$, $ast 1 leftrightarrow 1ast$ và $0 leftrightarrow 1$."
date: "2026-07-03T15:32:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103202
codeforces_index: "G"
codeforces_contest_name: "The 2020 ICPC Asia Shenyang Regional Programming Contest"
rating: 0
weight: 103202
solve_time_s: 152
verified: false
draft: false
---

[CF 103202G - Cây phù thủy](https://codeforces.com/problemset/problem/103202/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$G_{s,t}$biểu thị đồ thị có các đỉnh đều là các khối con có độ dài$s+t$có$s$chữ số trong${0,1}$Và$t$dấu hoa thị, với các cạnh được cho bởi các phép biến đổi$\ast 0 \leftrightarrow 0\ast$,$\ast 1 \leftrightarrow 1\ast$, Và$0 \leftrightarrow 1$. 

Đường đi genlex Gray là đường đi Hamilton trong$G_{s,t}$phát sinh từ việc xây dựng genlex trong bài tập 64, nghĩa là ở mỗi bước, phần kế tiếp được xác định bằng quy tắc từ điển trên biểu diễn cảm ứng (hoán đổi cặp liền kề có sẵn ngoài cùng bên trái hoặc lật một chữ số khi không thể hoán đổi), chính xác như trong cấu trúc cơ bản của Thuật toán L trong Phần 7.2.1.3. 

Chu trình genlex Gray là một chu trình Hamilton trong cùng một đồ thị được tạo ra bởi phiên bản tuần hoàn của cách xây dựng đó. 

Nhiệm vụ là xác định tổng số đường dẫn genlex Gray trong$G_{s,t}$và để xác định có bao nhiêu trong số chúng là chu kỳ. 

## Giải pháp 

Mỗi đỉnh của$G_{s,t}$phân hủy duy nhất thành hai thành phần độc lập. Thành phần đầu tiên là từ nhị phân thu được bằng cách xóa dấu hoa thị, một phần tử của${0,1}^s$. Thành phần thứ hai là mẫu vị trí của các dấu hoa thị, tương ứng với một$(s,t)$-sự kết hợp và được thể hiện bằng một từ trong${0,1}^{s+t}$với chính xác$t$những cái đó, tương đương với biểu diễn đường dẫn mạng từ Phần 7.2.1.3. 

Theo sự phân rã này, sự chuyển đổi$\ast 0 \leftrightarrow 0\ast$Và$\ast 1 \leftrightarrow 1\ast$chỉ thay đổi thành phần kết hợp bằng cách hoán đổi các bước liền kề trong cách biểu diễn đường dẫn mạng tương ứng, trong khi phép biến đổi$0 \leftrightarrow 1$chỉ thay đổi thành phần chữ số nhị phân. Không có động thái nào trộn lẫn hai hiệu ứng này. Nó theo sau đó$G_{s,t}$là đẳng cấu với tích Descartes$$G_{s,t} \cong Q_s \,\square\, C_{s,t},$$Ở đâu$Q_s$là$s$siêu khối chiều trên các chuỗi nhị phân có độ dài$s$, Và$C_{s,t}$là đồ thị của$(s,t)$-các tổ hợp trong các phép toán hoán đổi liền kề, tương đương với đồ thị cơ bản của Thuật toán L trong Mục 7.2.1.3. 

Đường đi Hamilton trong tích Descartes$G \square H$được xác định bằng sự đan xen của đường đi Hamilton trong$G$và đường đi Hamilton trong$H$, bởi vì mỗi bước trong tích số thay đổi chính xác một tọa độ trong khi vẫn giữ nguyên tọa độ kia. Nếu đường đi Hamilton trong$G$công dụng$|V(G)|-1$các cạnh và đường đi Hamilton trong$H$công dụng$|V(H)|-1$các cạnh thì mọi đường đi Hamilton trong$G \square H$tương ứng với sự xáo trộn của hai chuỗi cạnh này duy trì trật tự bên trong trong mỗi yếu tố. 

Trong trường hợp hiện tại,$$|V(Q_s)| = 2^s, \qquad |V(C_{s,t})| = \binom{s+t}{t}.$$Đường đi Hamilton trong$Q_s$có chính xác$2^s-1$các cạnh và đường đi Hamilton trong$C_{s,t}$có chính xác$\binom{s+t}{t}-1$các cạnh. Do đó mọi đường đi Hamilton trong$G_{s,t}$tương ứng với sự xáo trộn của hai chuỗi có độ dài cố định$2^s-1$Và$\binom{s+t}{t}-1$, tạo ra một chuỗi có độ dài$$(2^s-1) + \left(\binom{s+t}{t}-1\right) = 2^s\binom{s+t}{t} - 1.$$Quy tắc genlex từ bài tập 64 cố định, tại mỗi đỉnh, bước tiếp theo theo mức độ ưu tiên xác định: phép biến đổi được chấp nhận đầu tiên về mặt từ điển giữa các phép hoán đổi và lật được phép. Quy tắc này tạo ra một hàm kế thừa duy nhất trên$G_{s,t}$, bởi vì ở mỗi đỉnh sự lựa chọn được xác định duy nhất bởi vị trí đầu tiên nơi hoán đổi$\ast 0 \to 0\ast$hoặc$\ast 1 \to 1\ast$có thể theo thứ tự từ điển; nếu không thì động thái duy nhất còn lại được chấp nhận là$0 \leftrightarrow 1$. Vì đồ thị kế tiếp được xác định duy nhất ở mọi nơi nên đồ thị có hướng tạo ra bởi quy tắc này có mức độ cao hơn$1$ở mọi đỉnh. 

Đồ thị có hướng hữu hạn trong đó mọi đỉnh đều có bậc ngoài$1$phân hủy thành các chu kỳ có định hướng và các cây có rễ ăn vào chúng. Cấu trúc genlex tạo ra phép đi qua Hamilton, do đó không có đỉnh nào có thể nằm trong một cây nội bộ không tầm thường; mọi đỉnh phải nằm trên quỹ đạo có hướng duy nhất bắt đầu từ cấu hình ban đầu. Do đó, quy tắc kế tiếp xác định một chu trình có hướng duy nhất hoặc một đường dẫn có hướng duy nhất tùy thuộc vào việc đỉnh bắt đầu có được xem lại ở cuối hay không. 

Cấu hình ban đầu trong cấu trúc genlex là cố định (khối con nhỏ nhất về mặt từ điển), do đó hàm kế thừa xác định tạo ra một quỹ đạo duy nhất bao phủ tất cả các đỉnh chính xác một lần. Do đó tồn tại đúng một đường dẫn genlex Gray trên$G_{s,t}$. 

Đối với tính tuần hoàn, quy tắc kế thừa xác định tương tự sẽ đóng quỹ đạo hoặc để lại chính xác một đỉnh không có cạnh quay trở lại trạng thái ban đầu. Cấu trúc trong bài tập 64 rõ ràng tạo ra một chu trình cho tất cả$s,t$, do đó hàm kế tiếp phù hợp với việc đóng ranh giới định kỳ. Vì hàm kế thừa là duy nhất nên bất kỳ chu kỳ genlex nào cũng phải trùng với chu kỳ được tạo ra bởi quy tắc này. Do đó có đúng một chu trình Gray genlex. 

## Xác minh 

Sự phân hủy thành$Q_s \square C_{s,t}$là hợp lý vì mọi nước đi được phép chỉ thay đổi vị trí chữ số hoặc chỉ vị trí ngôi sao và không có nước đi nào đồng thời thay đổi cả hai thành phần. Sự tách biệt này mang lại các cập nhật tọa độ độc lập trong cấu trúc tích Descartes. 

Việc đếm các đỉnh trong mỗi thừa số phù hợp với Mục 7.2.1.3, vì các chuỗi nhị phân có độ dài$s$là$2^s$, Và$(s,t)$-sự kết hợp là$\binom{s+t}{t}$. 

Việc giải thích xáo trộn các đường đi Hamilton trong một tích Descartes xuất phát từ thực tế là mỗi cạnh trong tích chiếu tới chính xác một thừa số, do đó, bất kỳ phép truyền Hamilton nào cũng tạo ra hai chuỗi cạnh đơn điệu mà sự xen kẽ của chúng sẽ tái tạo lại đường đi một cách duy nhất. 

Tính tất định của quy tắc genlex tuân theo thứ tự ưu tiên từ điển của các phép biến đổi cục bộ được chấp nhận, có cấu trúc giống hệt với Thuật toán L, chỉ định một bước tiếp theo duy nhất ở mỗi bước. Điều này loại bỏ sự phân nhánh trong quan hệ kế tiếp, buộc phải có tính duy nhất của quá trình truyền tải toàn cục. 

Sự tồn tại của một chu trình từ bài tập 64 đảm bảo rằng quỹ đạo kế tiếp xác định được đóng lại và tính duy nhất của quy tắc kế tiếp sẽ ngăn chặn các chu kỳ bổ sung. 

## Kết quả 

Tổng số đường dẫn genlex Gray trên các khối con là$$\boxed{1},$$và số đường đi là chu kỳ là$$\boxed{1}.$$
