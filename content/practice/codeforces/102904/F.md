---
title: "CF 102904F - Bài tập"
description: "Giả sử $a1 ge cdots ge an ge 0$ và $a'1 ge cdots ge a'n ge 0$ là các phân vùng của $n$, được đệm bằng các số 0 để có độ dài $n$."
date: "2026-07-04T10:15:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102904
codeforces_index: "F"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u041f\u044f\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102904
solve_time_s: 70
verified: false
draft: false
---

[CF 102904F - Bài tập](https://codeforces.com/problemset/problem/102904/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$a_1 \ge \cdots \ge a_n \ge 0$Và$a'_1 \ge \cdots \ge a'_n \ge 0$là phân vùng của$n$, được đệm bằng số 0 theo chiều dài$n$. Liên hợp của chúng được xác định bởi$$b_k = |\{ i \mid a_i \ge k \}|, \qquad b'_k = |\{ i \mid a'_i \ge k \}|.$$Mục đích là để so sánh thứ tự từ điển của$b_1 b_2 \cdots b_n$Và$b'_1 b'_2 \cdots b'_n$với thứ tự từ điển của các chuỗi đảo ngược$a_n \cdots a_1$Và$a'_n \cdots a'_1$. 

Cho phép$r_i = a_{n-i+1}$Và$r'_i = a'_{n-i+1}$là trình tự đảo ngược. So sánh từ điển học$r$Và$r'$được xác định bởi chỉ số nhỏ nhất$i$như vậy$r_i \ne r'_i$, tương đương với chỉ số lớn nhất$j = n-i+1$như vậy$a_j \ne a'_j$. 

Cho phép$j$là chỉ số lớn nhất với$a_j \ne a'_j$. Sau đó$a_{j+1} = a'_{j+1}, \ldots, a_n = a'_n = 0$và các chuỗi đảo ngược đầu tiên khác nhau ở vị trí$i = n-j+1$. 

Cho rằng$a_j < a'_j$; trường hợp ngược lại là đối xứng. Sau đó cho tất cả$k > a'_j$, cả hai$a_j$Và$a'_j$là$< k$, vậy các hàng$1,\ldots,j$không đóng góp vào$b_k$hoặc$b'_k$. Vì$k \le a_j$, cả hai hàng$j$đóng góp, và sự khác biệt giữa$b_k$Và$b'_k$được xác định hoàn toàn bởi chỉ số$1,\ldots,j-1$, Ở đâu$a$Và$a'$trùng khớp. Kể từ đây$b_k = b'_k$cho tất cả$k \le a_j$. 

Tại$k = a_j + 1$, hàng ngang$j$góp phần vào$b'_k$nhưng không phải để$b_k$, từ$a_j < a'_j$. Như vậy$b'_{a_j+1} = b_{a_j+1} + 1$, Vì thế$b < b'$theo thứ tự từ điển. 

Bây giờ chúng ta liên hệ điều này với các trình tự đảo ngược. Từ$j$là chỉ số lớn nhất có chênh lệch, chênh lệch đầu tiên trong$r$Và$r'$xảy ra ở vị trí$n-j+1$, Ở đâu$r_{n-j+1} = a_j$Và$r'_{n-j+1} = a'_j$. Kể từ đây$r < r'$về mặt từ điển. 

Điều này cho thấy nếu$a_n \cdots a_1 < a'_n \cdots a'_1$, sau đó$b_1 \cdots b_n < b'_1 \cdots b'_n$. 

Đối với điều ngược lại, giả sử$b < b'$và để$k$là chỉ số nhỏ nhất với$b_k \ne b'_k$. Sau đó$b_1 = b'_1, \ldots, b_{k-1} = b'_{k-1}$, vì vậy đối với tất cả$i$với$a_i \ge k-1$, số lượng các chỉ số đó đồng ý trong$a$Và$a'$. Sự khác biệt đầu tiên trong cấu trúc hàng phải xảy ra ở chỉ số hàng cao nhất$j$sao cho một trong$a_j, a'_j$vượt qua ngưỡng$k$. Điều này ngụ ý$a_j < a'_j$và tất cả các hàng được lập chỉ mục cao hơn đều bằng nhau. 

Như vậy$j$chính xác là chỉ số lớn nhất nơi$a_j \ne a'_j$và sự khác biệt đầu tiên trong chuỗi đảo ngược xảy ra ở vị trí$n-j+1$với$a_j < a'_j$. Kể từ đây$a_n \cdots a_1 < a'_n \cdots a'_1$. 

Cả hai hàm ý đều đúng nên hai so sánh từ điển học là tương đương nhau. 

Điều này hoàn thành việc chứng minh. ∎
