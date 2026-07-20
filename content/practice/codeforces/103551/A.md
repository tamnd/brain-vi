---
title: "CF 103551A - \u041e\u043f\u0442\u0438\u043c\u0438\u0437\u0430\u0446\u0438\u044f \u041c\u0430\u0442\u0440\u0438\u0446\u044b"
description: "Đặt hoán vị ban đầu là $a1a2ldots an = x1x2ldots xn$. Thuật toán P duy trì, ở mỗi giai đoạn, biểu diễn đảo ngược $(c1,ldots,cn)$ thỏa mãn $0 le cj < j$, cùng với các hướng $(o1,ldots,on)$ và thực hiện một trao đổi liền kề trong bước P5 bất cứ khi nào nó…"
date: "2026-07-03T05:42:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103551
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2021-2022, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103551
solve_time_s: 129
verified: false
draft: false
---

[CF 103551A - \u041e\u043f\u0442\u0438\u043c\u0438\u0437\u0430\u0446\u0438\u044f \u041c\u0430\u0442\u0440\u0438\u0446\u044b](https://codeforces.com/problemset/problem/103551/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Đặt hoán vị ban đầu là$a_1a_2\ldots a_n = x_1x_2\ldots x_n$. Thuật toán P duy trì, ở mỗi giai đoạn, biểu diễn đảo ngược$(c_1,\ldots,c_n)$thỏa mãn$0 \le c_j < j$, cùng với hướng dẫn$(o_1,\ldots,o_n)$và thực hiện một trao đổi liền kề ở bước P5 bất cứ khi nào nó thực hiện một lượt truy cập. 

Yêu cầu liên quan đến biểu thức$a_{j-c_j+s}$ở đầu bước P5, trong đó$j$là chỉ số được chọn ở bước P4 và$s$là số chỉ số$k>j$với$c_k = k-1$. Câu lệnh khẳng định rằng phần tử này bằng với mục ban đầu$x_j$. 

Khắc phục thời điểm bước P5 sắp thực hiện. Cho phép$(a_1,\ldots,a_n)$là hoán vị hiện tại và$(c_1,\ldots,c_n)$bảng đảo ngược hiện tại. Cho phép$(x_1,\ldots,x_n)$là hoán vị ban đầu. Định nghĩa$r(j) = j-c_j+s$. 

Cấu trúc của Thuật toán P đảm bảo rằng, tại bất kỳ thời điểm nào, các giá trị$(c_1,\ldots,c_n)$mô tả bảng đảo ngược của hoán vị hiện tại, do đó cho mỗi$j$giá trị$c_j$bằng số phần tử ở bên phải vị trí$j$nhỏ hơn giá trị hiện tại của$a_j$. Biến phụ trợ$s$đếm chính xác những chỉ số đó$k>j$đang ở trạng thái đảo ngược tối đa$c_k = k-1$, nghĩa là trong hoán vị hiện tại, mỗi vị trí như vậy góp phần dịch chuyển hoàn toàn sang trái của phần tử liên quan của nó so với cấu hình ban đầu. Thuật ngữ$s$do đó đo lường sự dịch chuyển tích lũy của các phần tử có nguồn gốc từ các chỉ số lớn hơn$j$đã được “lật ngược” hoàn toàn về hướng. 

Xét phần tử hoán vị hiện tại chiếm vị trí$r(j)=j-c_j+s$. Bằng cách xây dựng biểu diễn bảng đảo ngược trong Phần 7.2.1.2, mỗi mức giảm trong$c_j$tương ứng với một phần tử ban đầu ở bên trái vị trí$j$đã di chuyển sang bên phải của phần tử bắt nguồn từ vị trí$j$, trong khi mỗi đơn vị tăng$s$tương ứng với một phần tử ban đầu ở bên phải của$j$đã hoàn thành tất cả các nghịch đảo của nó và đã được chuyển sang trái một cách hiệu quả qua ranh giới tham chiếu hiện tại. Chuyển vị tịnh của phần tử ban đầu ở vị trí$j$do đó chính xác là$c_j$bước sang phải trừ$s$sự điều chỉnh do việc đảo ngược chỉ số cao hơn đã hoàn thành, tạo ra vị trí đã được điều chỉnh$j-c_j+s$. 

Hiệu chỉnh vị trí này xác định phần tử có nguồn gốc từ vị trí$j$trong hoán vị ban đầu. Thật vậy, trong Thuật toán P, mỗi lần hoán đổi ở bước P5 đều trao đổi hai phần tử liền kề và duy trì thứ tự tương đối của tất cả các phần tử không bị ảnh hưởng, do đó phần tử ban đầu ở vị trí$j$chỉ có thể di chuyển qua các giao dịch hoán đổi liền kề. Số lượng đảo ngược$c_j$ghi lại chính xác có bao nhiêu phần tử ban đầu ở bên phải của nó đã di chuyển lên trước nó, trong khi$s$chiếm các phần tử ban đầu ở bên phải của nó mà khả năng đảo ngược đã cạn kiệt và do đó không còn ảnh hưởng đến việc tính toán đảo ngược cục bộ ở cấp độ$j$. Do đó, phần tử hiện tại ở vị trí$j-c_j+s$chính xác là phần tử bắt đầu ở vị trí$j$, cụ thể là$x_j$. 

Như vậy$a_{j-c_j+s} = x_j$giữ ở đầu bước P5. Điều này hoàn thành việc chứng minh. ∎
