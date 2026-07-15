---
title: "CF 103389H - 4G\u7f51\u7edc"
description: "Đặt $alpha = a1 a2 chấm an$ là một hoán vị của ${1,dots,n}$. Đặt $pi$ biểu thị hoán vị nghịch đảo, do đó $pi(ai)=i$. Bảng đảo ngược từ Mục 7.2.1.2 được xác định bởi $cj = {, i : pi(i) pi(j), i < j }, qquad 1 le j le n,$ so $0 le cj < j$."
date: "2026-07-03T12:15:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103389
codeforces_index: "H"
codeforces_contest_name: "2021\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 103389
solve_time_s: 151
verified: false
draft: false
---

[CF 103389H - 4G\u7f51\u7edc](https://codeforces.com/problemset/problem/103389/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$\alpha = a_1 a_2 \dots a_n$là một hoán vị của${1,\dots,n}$. Cho phép$\pi$biểu thị hoán vị nghịch đảo, vì vậy$\pi(a_i)=i$. Bảng đảo ngược từ Mục 7.2.1.2 được xác định bởi$c_j = \#\{\, i : \pi(i) > \pi(j),\ i < j \}, \qquad 1 \le j \le n,$Vì thế$0 \le c_j < j$. Thứ hạng được định nghĩa là giá trị cơ số hỗn hợp của bảng này trong hệ thống số giai thừa. 

Định nghĩa$r(\alpha) = \sum_{j=1}^n c_j (j-1)!.$Sự ràng buộc$c_j < j$ngụ ý$0 \le r(\alpha) < n!$vì đây là biểu diễn giai thừa tiêu chuẩn với các chữ số$c_n,c_{n-1},\dots,c_1$. 

Để tính toán$r(\alpha)$trong thời gian tuyến tính, phải lấy được bảng đảo ngược mà không cần quét lặp lại tất cả các cặp. Duy trì một mảng$\pi[1..n]$như vậy$\pi[x]$là vị trí của giá trị$x$. Điều này được xây dựng trong một lần:$\pi[a_i] \leftarrow i \quad (1 \le i \le n).$Các giá trị$c_j$sau đó được xác định từ các tập hợp${1,\dots,j-1}$và vị trí của chúng so với$\pi(j)$. Để hỗ trợ việc đếm này một cách hiệu quả, hãy duy trì cấu trúc dữ liệu trên các vị trí lưu trữ, đối với mỗi tập hợp con các giá trị đã được xử lý, số phần tử có vị trí nằm trong một tiền tố nhất định. Với một cây được lập chỉ mục nhị phân trên$1..n$, mỗi truy vấn chèn và tiền tố được thực hiện theo thời gian tỷ lệ với một lần duyệt cây. 

Đang xử lý giá trị$j=1,2,\dots,n$theo thứ tự tăng dần, duy trì một cấu trúc chứa các vị trí$\pi(1),\dots,\pi(j-1)$. Sau đó$c_j = (j-1) - \#\{ i<j : \pi(i) \le \pi(j)\}.$Thuật ngữ thứ hai thu được dưới dạng truy vấn tổng tiền tố tại chỉ mục$\pi(j)$, vậy mỗi$c_j$được tính toán sau một lần cập nhật và một truy vấn trên cấu trúc. Vì mỗi thao tác đi qua một đường dẫn từ gốc tới lá nên tổng công việc trên tất cả$j$là tuyến tính theo nghĩa từ-RAM được sử dụng trong Phần 7.2.1.2. 

Như vậy$r(\alpha)$được tính bằng cách tích lũy$c_j (j-1)!$trong cùng một lần quét, tạo ra$k=r(\alpha)$TRONG$O(n)$các bước. 

Để ánh xạ nghịch đảo, hãy$k$được đưa ra với$0 \le k < n!$. Viết$k$trong biểu diễn giai thừa$k = \sum_{j=1}^n c_j (j-1)!,$với các chữ số thu được tuần tự bằng phép chia:$c_j = k \bmod j,\qquad k \leftarrow \left\lfloor k/j \right\rfloor,\qquad n \ge j \ge 1.$Để xây dựng lại$\alpha$, duy trì một bộ$S$ban đầu chứa${1,\dots,n}$. Vì$j=n,n-1,\dots,1$, chọn$(c_j+1)$-phần tử nhỏ nhất thứ nhất của$S$và nối nó như$a_{n-j+1}$, sau đó xóa nó khỏi$S$. Lựa chọn này được triển khai theo cùng cấu trúc vị trí được sử dụng ở trên, hỗ trợ xóa và thống kê thứ tự trong một lần duyệt cho mỗi thao tác. Mỗi bước sẽ loại bỏ chính xác một phần tử khỏi$S$, vậy tổng số lần cập nhật là$n$và mỗi lựa chọn được thực hiện trong thời gian khấu hao không đổi với cùng một giả định chi phí đơn vị. 

Việc xây dựng mang lại một hoán vị có bảng đảo ngược chính xác$(c_1,\dots,c_n)$, do đó khai triển giai thừa của nó bằng$k$, do đó hoán vị kết quả là$r^{-1}(k)$. 

Cả hai ánh xạ đều sử dụng một lần quét trên các chỉ mục cùng với một bản cập nhật và một truy vấn cho mỗi chỉ mục, mang lại thời gian tuyến tính theo nghĩa của mô hình trong Phần 7.2.1.2. 

Điều này hoàn thành việc chứng minh. ∎
