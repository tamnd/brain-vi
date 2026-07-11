---
title: "CF 103148C - Bánh quy đôi"
description: "Đặt $mathcal{A}$ là một họ gồm $s$-kết hợp và $mathcal{B}$ là một họ gồm $t$-kết hợp, cả hai tập hợp con của $U={0,1,dots,n-1}$ với $nge s+t$."
date: "2026-07-03T19:00:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103148
codeforces_index: "C"
codeforces_contest_name: "EGOI 2021 Day 1"
rating: 0
weight: 103148
solve_time_s: 157
verified: false
draft: false
---

[CF 103148C - Bánh quy đôi](https://codeforces.com/problemset/problem/103148/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\mathcal{A}$là một gia đình của$s$-sự kết hợp và$\mathcal{B}$một gia đình$t$-các tổ hợp, cả hai tập hợp con của$U={0,1,\dots,n-1}$với$n\ge s+t$. Giả thuyết giao nhau cho rằng$$\alpha \cap \beta \ne \varnothing \quad \text{for all } \alpha \in \mathcal{A}, \ \beta \in \mathcal{B}.$$Định lý K giới thiệu các họ$Q^n_{M,s}$Và$Q^n_{N,t}$như các phân đoạn ban đầu có kích thước$M$Và$N$tương ứng theo thứ tự tiêu chuẩn của$s$- Và$t$-sự kết hợp, tương đương với sự kết hợp đầu tiên về mặt từ điển$M$Và$N$các phần tử theo thứ tự được tạo ra bởi biểu diễn nhị phân hoặc colexicographic được mô tả trong Phần 7.2.1.3. 

Bằng chứng tiến hành bằng cách chỉ ra rằng thuộc tính giao nhau được bảo toàn trong các hoạt động nén (dịch chuyển) tiêu chuẩn và việc nén lặp lại sẽ biến đổi bất kỳ họ nào thành phân đoạn ban đầu tương ứng mà không thay đổi kích thước của nó. 

Sửa chỉ số$0\le i<j\le n-1$. Xác định$(i,j)$-shift trên một$s$-bộ$\alpha$bằng cách thay thế$j$với$i$bất cứ khi nào$j\in \alpha$,$i\notin \alpha$và tập kết quả chưa có trong họ. Về mặt hình thức, đối với một gia đình$\mathcal{A}$, định nghĩa$$S_{ij}(\mathcal{A}) = \{ S_{ij}(\alpha) : \alpha \in \mathcal{A} \},$$Ở đâu$S_{ij}(\alpha)= (\alpha \setminus {j}) \cup {i}$trong trường hợp có thể dịch chuyển được, và$S_{ij}(\alpha)=\alpha$nếu không thì. Định nghĩa tương tự áp dụng cho$\mathcal{B}$. 

Mỗi ca duy trì số lượng bản số vì nó thay thế các bộ riêng lẻ mà không bị trùng lặp. Nó cũng duy trì tính đồng nhất của kích thước đã đặt. 

Để xác minh việc duy trì giao lộ, hãy lấy$\alpha \in \mathcal{A}$Và$\beta \in \mathcal{B}$. Nếu cả hai tập hợp đều không bị dịch chuyển thì điều kiện giao nhau không thay đổi. Giả định$\alpha$được chuyển sang$\alpha' = S_{ij}(\alpha)$. Nếu như$j\notin \beta$, thì bất kỳ điểm giao nhau nào trong$\alpha\cap\beta$vẫn có hiệu lực trừ khi nó được$j$, trong trường hợp đó$j\notin\beta$loại trừ điều này. Nếu như$j\in\beta$, thì$j\in\alpha$, trong trường hợp đó$\alpha\cap\beta$đã chứa$j$, hoặc$j\notin\alpha$Nhưng$i\in\beta$hoặc$i\notin\beta$. Trong mọi trường hợp, thay thế$j$qua$i$TRONG$\alpha$không thể phá hủy tất cả các giao lộ, bởi vì bất kỳ yếu tố nhân chứng nào của$\alpha\cap\beta$khác với$j$vẫn không thay đổi, trong khi nếu nhân chứng tiềm năng duy nhất là$j$, sau đó$j$sẽ nằm trong cả hai tập hợp và giao điểm vẫn tồn tại trừ khi cả hai ca loại bỏ nó đồng thời, điều này ngăn cản định nghĩa về dịch chuyển. Lập luận tương tự được áp dụng một cách đối xứng khi$\beta$được dịch chuyển. 

Như vậy mỗi ca$S_{ij}$bảo toàn giao điểm của cặp$(\mathcal{A},\mathcal{B})$. 

Áp dụng lặp đi lặp lại tất cả các ca với$i<j$cuối cùng tạo ra các họ ổn định khi dịch chuyển, vì mỗi lần dịch chuyển sẽ làm giảm nghiêm ngặt tổng các giá trị phần tử$\sum_{\alpha\in\mathcal{A}}\sum_{x\in\alpha} x$trừ khi gia đình đã bị nén trái. Điều tương tự cũng đúng đối với$\mathcal{B}$. Do đó quá trình kết thúc ở một cặp họ đã dịch chuyển$(\mathcal{A}^_,\mathcal{B}^_)$với$|\mathcal{A}^_|=M$Và$|\mathcal{B}^_|=N$, vẫn cắt nhau. 

Đặc điểm tiêu chuẩn của các họ được dịch chuyển hoàn toàn trong bối cảnh này, phù hợp với cấu trúc từ điển của Phần 7.2.1.3, xác định$\mathcal{A}^_$là đoạn ban đầu$Q^n_{M,s}$Và$\mathcal{B}^_$BẰNG$Q^n_{N,t}$. Thật vậy, trong cách biểu diễn nhị phân được sắp xếp theo thứ tự từ điển (hoặc tương đương thông qua thứ tự của$c_t\cdots c_1$được mô tả trong Thuật toán L), việc dịch chuyển buộc mỗi họ phải chứa kết quả sớm nhất có thể$s$- hoặc$t$-sự kết hợp tương thích với kích thước của nó, đó chính xác là định nghĩa của$Q^n_{M,s}$Và$Q^n_{N,t}$trong Định lý K. 

Vì giao điểm được bảo toàn trong suốt quá trình dịch chuyển nên các họ đầu cuối$Q^n_{M,s}$Và$Q^n_{N,t}$kế thừa tài sản mà mọi$s$-sự kết hợp trong$Q^n_{M,s}$giao nhau mọi$t$-sự kết hợp trong$Q^n_{N,t}$. 

Điều này hoàn thành việc chứng minh. ∎
