---
title: "CF 103469J - Trò đùa"
description: "Chúng ta được cho một hoán vị cố định $p$ có kích thước $n$, và một hoán vị được chỉ định một phần $q$ có cùng kích thước. Một số vị trí trong $q$ đã biết, một số vị trí khác bằng 0 và phải được điền để chuỗi cuối cùng trở thành một hoán vị hợp lệ từ $1$ đến $n$."
date: "2026-07-03T06:45:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103469
codeforces_index: "J"
codeforces_contest_name: "2021 Summer Petrozavodsk Camp, Day 3: IQ test (XXII Open Cup, Grand Prix of IMO)"
rating: 0
weight: 103469
solve_time_s: 32
verified: false
draft: false
---

[CF 103469J - Trò đùa](https://codeforces.com/problemset/problem/103469/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị cố định$p$kích thước$n$và một hoán vị được chỉ định một phần$q$có cùng kích thước. Một số vị trí trong$q$đã biết, các số khác bằng 0 và phải được điền để dãy cuối cùng trở thành một hoán vị hợp lệ của$1$ĐẾN$n$. 

Với mỗi hoán vị hoàn thành$q$, chúng ta xác định một đại lượng$f(p,q)$. Điều này không được định nghĩa trực tiếp như một công thức mà là một bài toán đếm trên một$2 \times n$ma trận chứa đầy các số$1$bởi vì$2n$, mỗi cái xuất hiện đúng một lần. Ma trận bị ràng buộc sao cho trong mỗi hàng, thứ tự của các giá trị bị ép buộc bởi hoán vị tương ứng: nếu chúng ta sắp xếp các cột bằng cách tăng dần$p_i$, hàng đầu tiên phải tăng nghiêm ngặt và tương tự, hàng thứ hai phải tăng nghiêm ngặt khi các cột được sắp xếp theo thứ tự tăng dần$q_i$. 

Ngoài ra, mỗi cột mang nhãn nhị phân tùy thuộc vào mục nhập trên cùng có nhỏ hơn mục nhập dưới cùng hay không. Chuỗi nhị phân đó không được cố định trước; nó được gây ra bởi ma trận đã chọn. giá trị$f(p,q)$đếm có bao nhiêu ma trận hợp lệ như vậy tồn tại cho một cặp cố định$(p,q)$. Cuối cùng nhiệm vụ là tính tổng$f(p,q)$trên tất cả các lần hoàn thành của hoán vị một phần$q$, modulo$998244353$. 

Những hạn chế$n \le 100$ngụ ý rằng bất kỳ giải pháp nào liên quan đến$O(n^3)$hoặc tệ hơn là tổ hợp trên hoán vị vẫn có thể chấp nhận được, nhưng bất cứ điều gì theo cấp số nhân trong$n$hoặc liệt kê các ma trận một cách rõ ràng là không. Vì ma trận chứa$2n$các giá trị riêng biệt, một cách giải thích ngây thơ đã gợi ý các cấu trúc quy mô giai thừa, vì vậy thách thức thực sự là thu gọn việc đếm thành một thứ chỉ phụ thuộc vào các lựa chọn tổ hợp thô hơn là các phép gán rõ ràng. 

Một trường hợp lỗi tinh vi xuất hiện nếu người ta giả sử chuỗi nhị phân$s$độc lập với phép gán số. Ví dụ, đối với$n=2$, vị trí khác nhau của các giá trị$1,2,3,4$có thể sản xuất tương tự$p,q$các ràng buộc đặt hàng nhưng so sánh cột khác nhau, vì vậy$s$là
