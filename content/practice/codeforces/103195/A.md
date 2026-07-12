---
title: "CF 103195A - \u041f\u043e\u0435\u0434\u0443 \u0434\u043e\u043c\u043e\u0439"
description: "Đặt $n=s+t$. Bài tập 64 xét tập hợp tất cả các chuỗi có độ dài $n$ chứa chính xác $t$ dấu hoa thị và các chữ số $s$, trong đó mỗi chữ số là $0$ hoặc $1$."
date: "2026-07-03T15:50:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103195
codeforces_index: "A"
codeforces_contest_name: "2020-2021 \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0437\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f, \u0442\u0443\u0440 2"
rating: 0
weight: 103195
solve_time_s: 83
verified: false
draft: false
---

[CF 103195A - \u041f\u043e\u0435\u0434\u0443 \u0434\u043e\u043c\u043e\u0439](https://codeforces.com/problemset/problem/103195/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$n=s+t$. Bài tập 64 xét tập hợp tất cả các chuỗi có độ dài$n$chứa chính xác$t$dấu hoa thị và$s$chữ số, trong đó mỗi chữ số là$0$hoặc$1$. Do đó, tập hợp các đỉnh là tích Descartes của việc chọn một$t$-tập hợp con các vị trí cho dấu hoa thị và gán nhãn nhị phân cho phần còn lại$s$vị trí, do đó tập đỉnh có kích thước$N = 2^s \binom{n}{t}.$Hai đỉnh kề nhau nếu chúng khác nhau bởi một trong các phép biến đổi cho phép$\ast 0 \leftrightarrow 0 \ast$,$\ast 1 \leftrightarrow 1 \ast$, hoặc$0 \leftrightarrow 1$. Đường dẫn genlex Gray là đường dẫn Hamilton trong biểu đồ này tuân theo cấu trúc genlex của bài tập 64 và chu trình genlex Gray là chu trình Hamilton. 

Nhiệm vụ là liệt kê tất cả các đường dẫn genlex Gray và xác định có bao nhiêu trong số đó là các chu trình. 

## Giải pháp 

Mỗi đỉnh bao gồm hai thành phần độc lập. Một thành phần là vị trí của$t$dấu hoa thị, đó là một$(s,t)$-sự kết hợp. Thành phần khác là sự phân công của$0$Và$1$đến phần còn lại$s$các vị trí. 

Những biến đổi$\ast 0 \leftrightarrow 0 \ast$Và$\ast 1 \leftrightarrow 1 \ast$chỉ sửa đổi vị trí của một dấu hoa thị trong khi vẫn giữ nguyên chữ số mang vị trí đó. Điều này gây ra sự kề cận trên đồ thị Johnson của$(s,t)$-sự kết hợp, trong đó mỗi bước di chuyển tương ứng với việc trao đổi một$\ast$với vị trí chữ số liền kề. 

Sự biến đổi$0 \leftrightarrow 1$chỉ sửa đổi một chữ số trong khi vẫn giữ nguyên$\ast$các vị trí cố định. Điều này gây ra sự liền kề trên$s$siêu khối chiều$Q_s$. 

Do đó đồ thị đầy đủ là tích Descartes$G = J(n,t) \,\square\, Q_s,$Ở đâu$J(n,t)$là đồ thị Johnson trên$(s,t)$-sự kết hợp và$Q_s$siêu khối nhị phân trên$s$bit. 

Bài 64 xây dựng chu trình Gray genlex bằng cách đồng bộ chu trình Hamilton trên$J(n,t)$với chu trình Hamilton$Q_s$sao cho mỗi tọa độ thay đổi chính xác khi chu kỳ nhân tố của nó tăng lên. Việc xây dựng mang tính quyết định khi đỉnh ban đầu được cố định, bởi vì ở mỗi bước, quy tắc genlex chọn phép biến đổi duy nhất có thể chấp nhận được để duy trì tính nhất quán từ điển của cả hai thành phần. 

Sửa một đỉnh bắt đầu. Quy tắc genlex xác định một cạnh đi ra duy nhất ở mỗi đỉnh bởi vì phép biến đổi có thể áp dụng đầu tiên về mặt từ điển giữa các cạnh được phép được xác định duy nhất bằng cách xây dựng bài tập 64. Điều này buộc việc truyền tải phải là một chu trình có hướng duy nhất bao trùm tất cả$N$đỉnh. Việc đảo ngược tất cả các lựa chọn sẽ tạo ra quá trình truyền tải ngược, đây lại là một đường dẫn genlex Gray hợp lệ. 

Không còn sự tự do nào khác nữa. Bất kỳ sai lệch nào tại một đỉnh sẽ vi phạm điều kiện từ điển được sử dụng trong cấu trúc, vì cả quy tắc cập nhật kết hợp và quy tắc cập nhật bit đều được xác định duy nhất bởi nguyên tắc “thay đổi ngoài cùng bên phải” nằm trong Thuật toán L trong Phần 7.2.1.3 và sàng lọc chu trình Gray của nó trong bài tập 64. Do đó, mọi đường dẫn Gray genlex trùng khớp với chu trình được xây dựng hoặc đảo ngược của nó. 

Điều này cho ra chính xác hai đường đi Hamilton có hướng trên tập đỉnh đầy đủ, tương ứng với hai hướng của cùng một chu trình. 

Chu trình Hamilton không phụ thuộc vào điểm bắt đầu nên hai đường đi có hướng biểu diễn cùng một chu trình vô hướng. Do đó có đúng một chu trình Gray genlex. 

## Xác minh 

Mỗi đỉnh của$G$có bằng cấp$s+t$, vì chính xác một bước di chuyển có sẵn cho mỗi vị trí chữ số và từng vị trí dấu hoa thị theo các phép biến đổi được phép. Cấu trúc genlex chọn một cạnh đi ra ở mỗi bước, tạo ra một sơ đồ con bao trùm 1 đều, do đó có sự kết hợp rời rạc của các chu trình có hướng. 

Bởi vì việc xây dựng thăm mỗi đỉnh đúng một lần trước khi quay trở lại, đồ thị con bao trùm bao gồm một chu trình duy nhất trên$N$đỉnh. Bất kỳ đường dẫn genlex thay thế nào cũng sẽ yêu cầu một cạnh đi ra khác ở một số đỉnh, mâu thuẫn với tính duy nhất của bước di chuyển được xác định theo từ điển. Do đó không có lựa chọn phân nhánh nào tồn tại trên toàn cầu. 

Việc đảo ngược một chu trình bảo toàn tính kề cận và các phép biến đổi cho phép, do đó tồn tại chính xác hai đường Hamilton có hướng và chúng chỉ khác nhau về hướng. Do đó, số chu kỳ vô hướng là$1$. 

## Trả lời 

Số đường dẫn genlex Gray là$\boxed{2},$và số chu kỳ genlex Gray là$\boxed{1}.$## Ghi chú 

Tính cứng nhắc xuất phát từ sự tương tác giữa hai cấu trúc từ điển “tham lam”: cập nhật kết hợp (Phần 7.2.1.3, Thuật toán L) và cập nhật nhị phân Gray trên$Q_s$. Khi những điều này được đồng bộ hóa như trong bài tập 64, sự lựa chọn cục bộ trở nên bắt buộc trên toàn cầu, thu gọn không gian tìm kiếm của các đường truyền Hamilton thành một chu trình duy nhất cho đến khi đảo ngược.
