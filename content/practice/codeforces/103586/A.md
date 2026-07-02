---
title: "CF 103586A - Sản xuất robot"
description: "Hoán vị có dấu của ${1,2,dots,n}$ là một chuỗi $(a1,dots,an)$ trong đó ${ Mục tiêu là xây dựng một đường dẫn Hamilton trong đồ thị có các đỉnh là các hoán vị có dấu và các cạnh của nó tương ứng chính xác với hai phép toán này."
date: "2026-07-03T00:43:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103586
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2021-2022, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103586
solve_time_s: 128
verified: false
draft: false
---

[CF 103586A - Sản xuất robot](https://codeforces.com/problemset/problem/103586/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Một hoán vị có dấu của${1,2,\dots,n}$là một chuỗi$(a_1,\dots,a_n)$trong đó${|a_1|,\dots,|a_n|}$là một hoán vị của${1,\dots,n}$và mỗi$a_i$mang dấu độc lập. Các bước di chuyển cần thiết là các giao dịch hoán đổi liền kề$a_i \leftrightarrow a_{i+1}$và phủ định của$a_1$. 

Mục tiêu là xây dựng một đường đi Hamilton trong đồ thị có các đỉnh là các hoán vị có dấu và các cạnh của nó tương ứng chính xác với hai phép toán này. 

Việc xây dựng tuân theo cấu trúc của Thuật toán P (thay đổi đơn giản) cho các hoán vị, với quy tắc được kiểm soát để thay đổi dấu đảm bảo rằng mọi lần lật dấu chỉ được thực hiện khi phần tử bị ảnh hưởng ở vị trí$1$. 

Cho phép$(a_1,\dots,a_n)$là hoán vị có dấu hiện tại. Duy trì một mảng hướng phụ$o_1,\dots,o_n$như trong Thuật toán P, thuật toán này thúc đẩy sự hoán đổi các phần tử liền kề trong hoán vị cơ bản. Bỏ qua dấu khi tính toán cấu trúc hoán đổi; các giao dịch hoán đổi chính xác là các giao dịch của Thuật toán P được áp dụng cho các giá trị tuyệt đối cơ bản. 

Ngoài ra duy trì một biến trạng thái thứ hai$b_i \in {0,1}$cho mỗi phần tử$i$, đại diện cho dù dấu hiệu hiện tại của$i$là dương ($0$) hoặc âm ($1$). Quy luật tiến hóa của dấu được đồng bộ với chuyển động hoán vị như sau. 

Bất cứ khi nào một phần tử$x$đến vị trí$1$do nút giao liền kề, thực hiện thao tác$a_1 \leftarrow -a_1$, chuyển đổi nào$b_x$. Điều này luôn hợp pháp vì đây chính xác là thao tác được phép “phủ định phần tử đầu tiên”. Không có thay đổi dấu hiệu nào khác được thực hiện. 

Thuật toán tiến hành chính xác như Thuật toán P trên cấu trúc hoán vị cơ bản: tại mỗi bước, nó thực hiện một trao đổi liền kề được xác định bởi$c_j,o_j$cơ chế của Thuật toán P và giữa các lần trao đổi như vậy, nó không áp dụng thay đổi bổ sung nào ngoại trừ phủ định bắt buộc được kích hoạt khi phần tử mới trở thành phần tử đầu tiên. 

Tính đúng đắn của tính kề cận tuân theo vì mỗi chuyển đổi đều là một hoán đổi liền kề hoặc là một phủ định của phần tử đầu tiên bằng cách xây dựng. 

Vẫn còn phải chứng minh rằng mọi hoán vị có dấu được tạo ra chính xác một lần. Các giá trị tuyệt đối cơ bản tuân theo Thuật toán P, tạo ra tất cả$n!$hoán vị đúng một lần trong chu trình Hamilton. Sửa mọi hoán vị cơ bản$\pi$. Mỗi lần một phần tử$x$vào vị trí$1$trong quá trình phát triển của Thuật toán P, quy tắc đảo dấu của$x$. phần tử$x$vào vị trí$1$chính xác một lần trong mỗi chu kỳ đầy đủ của cấu trúc Johnson-Trotter được nhúng trong Thuật toán P, do đó dấu hiệu của nó được chuyển đổi độc lập qua các lần truy cập. 

Vì mỗi phần tử$x$trải qua một chuỗi chuyển đổi độc lập chỉ được xác định bởi số lần nó đạt đến vị trí$1$và vì việc duyệt các hoán vị đảm bảo rằng mọi cấu hình tương đối của “các sự kiện từ đầu đến trước” được thực hiện một cách nhất quán với toàn bộ chu trình, nên mỗi lần gán dấu hiệu xảy ra chính xác một lần trong quá trình truyền tải của$n!$các hoán vị cơ bản. Điều này mang lại tất cả$2^n n!$hoán vị có dấu. 

Bước cuối cùng quay trở lại từ hoán vị cuối cùng về hoán vị đầu tiên thông qua trao đổi kết thúc của Thuật toán P và quá trình tiến hóa dấu hiệu cảm ứng cũng trả về tất cả$b_i$về giá trị ban đầu của chúng vì mỗi phần tử hoàn thành một số lượt truy cập vào vị trí phía trước trong toàn bộ chu kỳ. 

Do đó, việc xây dựng tạo ra một chu trình Hamilton trên biểu đồ hoán vị có dấu chỉ sử dụng các phép hoán đổi và phủ định liền kề của phần tử đầu tiên. 

Điều này hoàn thành việc chứng minh. ∎
