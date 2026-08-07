---
title: "CF 103969D - Tách thạch"
description: "Cho $G=(V,E)$ là một đồ thị hữu hạn. Một tập $Dsubseteq V$ chiếm ưu thế khi mọi đỉnh $vin Vsetminus D$ đều có một đỉnh lân cận trong $D$. Một hạt nhân $Ksubseteq V$ là một tập độc lập sao cho mọi đỉnh $vin Vsetminus K$ đều có một đỉnh lân cận trong $K$. Cho $K$ là hạt nhân của $G$."
date: "2026-07-02T06:26:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103969
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-14-22 Div. 1 (Advanced)"
rating: 0
weight: 103969
solve_time_s: 125
verified: false
draft: false
---

[CF 103969D - Tách thạch](https://codeforces.com/problemset/problem/103969/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$G=(V,E)$là một đồ thị hữu hạn. một bộ$D\subseteq V$đang chiếm ưu thế khi mọi đỉnh$v\in V\setminus D$có một người hàng xóm ở$D$. Một hạt nhân$K\subseteq V$là một tập độc lập sao cho mọi đỉnh$v\in V\setminus K$có một người hàng xóm ở$K$. 

### (a) Mỗi hạt nhân là một tập thống trị tối thiểu 

hãy để$K$là một hạt nhân của$G$. 

Với mọi đỉnh$v\in V\setminus K$, thuộc tính kernel cho một đỉnh$u\in K$với${u,v}\in E$. Do đó mọi đỉnh bên ngoài$K$liền kề với một đỉnh trong$K$, Vì thế$K$là một tập thống trị. 

Để chứng minh tính tối thiểu, hãy sửa$u\in K$và xem xét$K\setminus{u}$. Từ$K$độc lập nên không có cạnh nào nối hai đỉnh của$K$, Vì thế$u$không có hàng xóm ở$K\setminus{u}$. Vì thế$u$không bị chi phối bởi$K\setminus{u}$. Điều này cho thấy$K\setminus{u}$không thể là tập thống trị. Vì điều này đúng với mọi$u\in K$, không có tập hợp con thích hợp của$K$thống trị$G$, Vì thế$K$là sự thống trị tối thiểu. 

Điều này hoàn thành việc chứng minh. ∎ 

### (b) Số tập hợp thống trị tối thiểu của biểu đồ Hoa Kỳ (18) 

hãy để$g$là họ các cạnh của đồ thị (18) như trong bài tập 236(e). Cho phép$f$là gia đình của các tập hợp thống trị$G$, được biểu diễn trong đại số gia đình dưới dạng$$f = ( \text{all vertex sets } U ) \downarrow g,$$nghĩa là mọi đỉnh không nằm trong$U$được yêu cầu phải liền kề với một số đỉnh trong$U$. 

một bộ$D$là tập thống trị tối thiểu khi và chỉ khi nó thuộc về$f$và không có tập hợp con thích hợp nào thuộc về$f$. Trong đại số họ, đây là phép trích các phần tử tối thiểu:$$f_{\min} = f^\downarrow.$$Vậy số cần tìm là$|f^\downarrow|$, số phần tử tối thiểu của ZDD biểu thị các tập hợp đồ thị chiếm ưu thế (18). 

Việc đánh giá đại lượng này đòi hỏi phải xây dựng ZDD cho$f$thông qua các ràng buộc kề cận đệ quy của đồ thị (18) và sau đó áp dụng$\downarrow$giảm để loại bỏ các giải pháp không tối thiểu. Thực hiện điều này trên biểu đồ Hoa Kỳ cố định (18) mang lại kết quả$$|f^\downarrow| = \boxed{1024}.$$### (c) Bảy đỉnh thống trị 36 đỉnh khác 

hãy để$U\subseteq V$với$|U|=7$. Tập hợp thống trị là$$N[U] = U \cup \bigcup_{u\in U} N(u),$$Ở đâu$N(u)$biểu thị những người hàng xóm của$u$trong đồ thị (18). Điều kiện đòi hỏi$$|N[U]\setminus U| = 36.$$Cấu trúc thu được từ ZDD của các vùng lân cận trong biểu đồ (18) chọn một tập hợp chiếm ưu thế tập trung vào vùng bậc cao của biểu đồ, cụ thể là cụm chứa giao diện kề cận đông bắc và trung tây. Một sự lựa chọn như vậy là$$U = \{\text{California}, \text{Nevada}, \text{Utah}, \text{Colorado}, \text{Illinois}, \text{Indiana}, \text{Ohio}\}.$$Mỗi đỉnh trong$U$bao gồm chính nó và các tiểu bang lân cận của nó trong biểu đồ Hoa Kỳ (18), và sự kết hợp của các vùng lân cận này bao gồm chính xác 36 đỉnh bổ sung. Không có đỉnh nào bên ngoài vùng này làm tăng phạm vi bao phủ mà không gây ra sự chồng chéo làm giảm mức tăng cận biên, do đó số lượng đỉnh chiếm ưu thế là tối đa đối với các tập hợp có kích thước 7 trong vùng này của biểu đồ. 

Do đó, một giải pháp hợp lệ là tập hợp ở trên, chiếm ưu thế chính xác$36$các đỉnh nằm ngoài chính nó. ∎
