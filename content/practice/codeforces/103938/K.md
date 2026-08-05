---
title: "CF 103938K - Hộp đựng vật phẩm mang phong cách riêng"
description: "Thuật toán C trong phần này đánh giá BDD từ dưới lên bằng cách gán cho mỗi nút $v$ một giá trị chỉ phụ thuộc vào các nút kế thừa LO và HI của nó, với các nút chìm cung cấp các trường hợp cơ sở và mỗi nút bên trong kết hợp các kết quả từ các nút con của nó."
date: "2026-07-02T07:07:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103938
codeforces_index: "K"
codeforces_contest_name: "UTPC Contest 09-30-22 Div. 1 (Advanced)"
rating: 0
weight: 103938
solve_time_s: 127
verified: false
draft: false
---

[CF 103938K - Hộp vật phẩm đặc trưng](https://codeforces.com/problemset/problem/103938/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Thuật toán C trong phần này đánh giá BDD từ dưới lên bằng cách gán cho mỗi nút$v$một giá trị chỉ phụ thuộc vào các nút kế thừa LO và HI của nó, với các nút chìm cung cấp các trường hợp cơ sở và mỗi nút bên trong kết hợp các kết quả từ các nút con của nó. Để tính hàm sinh, sử dụng cùng một phép duyệt, nhưng nửa vành số được thay thế bằng nửa vành đa thức$\mathbb{Z}[z]$với trọng lượng$z$gắn liền với mỗi nhiệm vụ$x_i = 1$. 

Đối với một nút chìm tương ứng với$\bot$, không có phép gán nào thỏa mãn hàm, nên mọi số hạng trong hàm sinh đều biến mất, cho ra$G_{\bot}(z)=0.$Đối với một nút chìm tương ứng với$\top$, chính xác một phép gán đóng góp, cụ thể là phần tiếp theo trống, vì vậy$G_{\top}(z)=1.$Cho phép$v$là một nút nhánh được gắn nhãn bởi biến$x_k$với người kế vị LO$v_0$và người kế nhiệm HI$v_1$. Mỗi nhiệm vụ thỏa mãn kéo dài$v_0$chỉ định$x_k=0$, không góp phần làm tăng số mũ của$z$, trong khi mọi nhiệm vụ thỏa mãn đều mở rộng$v_1$chỉ định$x_k=1$, góp phần bổ sung thêm yếu tố$z$. Các vấn đề phụ tại$v_0$Và$v_1$rời rạc và bảo toàn cấu trúc biến còn lại theo thuộc tính thứ tự của BDD, do đó các đóng góp kết hợp bổ sung. Điều này mang lại sự tái diễn$G_v(z)=G_{v_0}(z)+z\,G_{v_1}(z).$Do đó, Thuật toán C được sửa đổi bằng cách thay thế phép cộng và phép nhân vô hướng trong bước đánh giá của nó bằng phép cộng và phép nhân trong$\mathbb{Z}[z]$, trong khi vẫn giữ nguyên thứ tự duyệt và cấu trúc ghi nhớ. Mỗi nút được đánh giá chính xác một lần như trong thuật toán ban đầu và mỗi lần đánh giá thực hiện một phép cộng đa thức và một phép nhân với$z$áp dụng cho phần đóng góp của chi nhánh HI. 

Giá trị kết quả được lưu trữ tại nút gốc là$G(z)=\sum_{x_1=0}^1 \cdots \sum_{x_n=0}^1 z^{x_1+\cdots+x_n} f(x_1,\ldots,x_n),$vì mỗi root-to-$\top$đường dẫn tương ứng với một phép gán thỏa mãn duy nhất và số mũ tích lũy chính xác số lượng biến được đặt thành$1$dọc theo con đường đó. 

Điều này hoàn thành việc sửa đổi Thuật toán C. ∎
