---
title: "CF 104279F - \u70b8\u5f39\u9e2d"
description: "Đặt $S={1,dots,m}$ biểu thị các biến bộ chọn và $T={m+1,dots,m+2^m}$ biểu thị các biến dữ liệu của bộ ghép kênh $Mm$. Với mỗi $iin S$, giá trị của $xi$ chọn một chỉ mục trong $T$, và hàm xuất ra bit dữ liệu đã chọn."
date: "2026-07-01T21:12:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104279
codeforces_index: "F"
codeforces_contest_name: "21st UESTC Programming Contest - Preliminary"
rating: 0
weight: 104279
solve_time_s: 127
verified: false
draft: false
---

[CF 104279F - \u70b8\u5f39\u9e2d](https://codeforces.com/problemset/problem/104279/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$S={1,\dots,m}$biểu thị các biến chọn và$T={m+1,\dots,m+2^m}$các biến dữ liệu của bộ ghép kênh$M_m$. Đối với mỗi$i\in S$, giá trị của$x_i$chọn một chỉ mục trong$T$và hàm xuất ra bit dữ liệu đã chọn. Cấu trúc Boolean chỉ phụ thuộc vào ánh xạ cảm ứng từ$S$ĐẾN$T$. 

Cho phép$\pi$là một hoán vị của$S\cup T$. Viết$\pi=(\pi_1,\dots,\pi_{m+2^m})$và xác định$$S_k=\{\pi_1,\dots,\pi_k\}\cap S,\qquad T_k=\{\pi_1,\dots,\pi_k\}\cap T.$$Cho phép$s_k=|S_k|$Và$t_k=|T_k|$. Hiện trạng xây dựng BDD sau khi xử lý lần đầu$k$các biến được xác định bởi phân vùng của các bit bộ chọn còn lại, vì mỗi bit bộ chọn còn lại vẫn nằm trong phạm vi hai nhánh, trong khi mỗi bit dữ liệu còn lại đóng góp một hằng số cuối sau khi mẫu địa chỉ của nó được cố định. 

Để chuyển nhượng một phần cho phần đầu tiên$k$các biến, hàm con vẫn phụ thuộc vào một số biến chọn chưa được xử lý khi và chỉ khi$s_k<m$. Trong trường hợp đó, cả hai nhánh LO và HI ở cấp độ$k$vẫn không kết thúc và tương ứng với các hàm con riêng biệt, vì ít nhất một bit chọn chưa được giải quyết và do đó chỉ mục được chọn trong$T$không cố định. 

Một lần$s_k=m$, tất cả các biến chọn đã được hiển thị. Hàm giảm xuống một biến dữ liệu duy nhất$x_j$với$j\in T$, Ở đâu$j$được xác định bởi sự phân công đầy đủ cho$S$. Từ thời điểm đó trở đi, mỗi biến còn lại trong$T$chỉ đóng góp một quyết định nhị phân trên một lá cố định và không phát sinh sự phụ thuộc nào nữa vào cấu trúc trước đó. 

Do đó, điều kiện hạt từ Mục 7.1.4 được áp dụng như sau: một nút ở mức$k$là một nút nhánh khi và chỉ khi hàm con tương ứng phụ thuộc vào biến tiếp theo, xảy ra chính xác trong khi$s_k<m$. Sau khi bộ chọn cuối cùng xuất hiện, không có hạt mới nào tương ứng với cấu trúc bộ chọn xuất hiện. 

Vì vậy hồ sơ của$M^\pi_m$được xác định hoàn toàn bởi vị trí của các biến chọn trong$\pi$. Đối với mỗi$k$với$s_k<m$, sự đóng góp cho hồ sơ là$1$, vì hàm con ở cấp độ đó vẫn phân biệt LO và HI thông qua lựa chọn chưa được giải quyết. Đối với mỗi$k$với$s_k=m$, không xảy ra phân nhánh theo hướng bộ chọn nữa và cấu trúc tiếp theo là cây nhị phân trên các biến dữ liệu còn lại. 

Do đó hồ sơ là trình tự$$\mathrm{prof}(k)=
\begin{cases}
1, & s_k<m,\\
0, & s_k=m,
\end{cases}$$được diễn giải ở các mức độ phân giải của bộ chọn, với điểm chuyển tiếp được xác định bởi lần xuất hiện cuối cùng của biến bộ chọn trong$\pi$. 

Đối với cấu hình gần đúng, mỗi biến dữ liệu trong$T$chỉ hoạt động sau khi đường dẫn bộ chọn đã xác định được một chỉ mục duy nhất. Khi$s_k<m$, mỗi biến dữ liệu gặp phải không giải quyết được hàm mà sao chép cấu trúc lựa chọn chưa được giải quyết trên cả hai nhánh, không tạo ra hạt mới ở cấp độ đó. Khi$s_k=m$, mỗi biến dữ liệu đóng góp chính xác một nút quyết định nhị phân tương ứng với lá được chọn cuối cùng, do đó mỗi bước như vậy đóng góp một đơn vị cho cấu hình gần đúng. 

Do đó, hồ sơ gần đúng là$$\mathrm{qprof}(k)=
\begin{cases}
0, & s_k<m,\\
1, & s_k=m.
\end{cases}$$Tương đương, nếu$k_1<\cdots<k_m$là vị trí của các biến chọn trong$\pi$, thì hồ sơ có giá trị$1$cho mọi cấp độ$k<k_m$và cấu hình gần đúng có giá trị$1$chính xác ở cấp độ$k\ge k_m$tương ứng với việc đi qua phần còn lại$2^m$bit dữ liệu sau khi lựa chọn đầy đủ được giải quyết. 

Điều này hoàn thành việc xác định hồ sơ và hồ sơ gần đúng cho$M^\pi_m$. ∎
