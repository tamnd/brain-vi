---
title: "CF 102948C - Nanh Trắng"
description: "Giả sử hình xuyến 2 × 2 × 3 là tích Descartes của các chu trình có hướng $C2 nhân C2 nhân C3$, với tập đỉnh $V = {(i,j,k) mid i trong {0,1}, j trong {0,1}, k thuộc {0,1,2}}."
date: "2026-07-04T07:26:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102948
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 02-05-21 Div. 2 (Beginner)"
rating: 0
weight: 102948
solve_time_s: 151
verified: false
draft: false
---

[CF 102948C - Nanh Trắng](https://codeforces.com/problemset/problem/102948/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Giả sử hình xuyến 2 × 2 × 3 là tích Descartes của các chu trình có hướng$C_2 \times C_2 \times C_3$, với tập đỉnh$V = \{(i,j,k) \mid i \in \{0,1\},\ j \in \{0,1\},\ k \in \{0,1,2\}\}.$Mối quan hệ bên dưới hình xuyến (như trong ví dụ (69)) kết nối mỗi đỉnh với các lân cận phía trước đơn vị của nó theo từng hướng tọa độ, với modulo bao quanh các kích thước tọa độ. Đối với một đỉnh$x = (i,j,k)$điều này xác định ba bước đi:$(i,j,k) \to (i+1 \bmod 2, j, k), \quad (i,j,k) \to (i, j+1 \bmod 2, k), \quad (i,j,k) \to (i, j, k+1 \bmod 3).$các$\alpha$hàm gán cho một đỉnh tập hợp các ảnh chuyển tiếp của nó theo các bước này và$\beta$hàm gán tập hợp các đỉnh ánh xạ tới nó, tương đương thu được bằng cách đảo ngược từng bước tọa độ. 

Như vậy đối với mỗi$(i,j,k) \in V$,$\alpha(i,j,k) = \{(i+1 \bmod 2, j, k),\ (i, j+1 \bmod 2, k),\ (i, j, k+1 \bmod 3)\},$Và$\beta(i,j,k) = \{(i-1 \bmod 2, j, k),\ (i, j-1 \bmod 2, k),\ (i, j, k-1 \bmod 3)\}.$Để tính toán những điều này một cách rõ ràng, chỉ cần đánh giá các gia số mô-đun trong mỗi tọa độ là đủ. 

Vì$i=0$,$i+1 \bmod 2 = 1$Và$i-1 \bmod 2 = 1$. Vì$i=1$, cả hai$i+1 \bmod 2$Và$i-1 \bmod 2$bình đẳng$0$. Sự đối xứng tương tự xảy ra đối với$j$. Vì$k \in {0,1,2}$, chu kỳ thuận là$0 \to 1 \to 2 \to 0$và chu kỳ ngược là$0 \to 2 \to 1 \to 0$. 

Do đó mọi giá trị của$\alpha$Và$\beta$có thể được liệt kê bằng cách thay thế các chuyển đổi mô-đun này. 

Vì$\alpha$:

Vì$(0,0,0)$,$\alpha(0,0,0) = \{(1,0,0),(0,1,0),(0,0,1)\}.$Vì$(0,0,1)$,$\alpha(0,0,1) = \{(1,0,1),(0,1,1),(0,0,2)\}.$Vì$(0,0,2)$,$\alpha(0,0,2) = \{(1,0,2),(0,1,2),(0,0,0)\}.$Vì$(0,1,0)$,$\alpha(0,1,0) = \{(1,1,0),(0,0,0),(0,1,1)\}.$Vì$(0,1,1)$,$\alpha(0,1,1) = \{(1,1,1),(0,0,1),(0,1,2)\}.$Vì$(0,1,2)$,$\alpha(0,1,2) = \{(1,1,2),(0,0,2),(0,1,0)\}.$Vì$(1,0,0)$,$\alpha(1,0,0) = \{(0,0,0),(1,1,0),(1,0,1)\}.$Vì$(1,0,1)$,$\alpha(1,0,1) = \{(0,0,1),(1,1,1),(1,0,2)\}.$Vì$(1,0,2)$,$\alpha(1,0,2) = \{(0,0,2),(1,1,2),(1,0,0)\}.$Vì$(1,1,0)$,$\alpha(1,1,0) = \{(0,1,0),(1,0,0),(1,1,1)\}.$Vì$(1,1,1)$,$\alpha(1,1,1) = \{(0,1,1),(1,0,1),(1,1,2)\}.$Vì$(1,1,2)$,$\alpha(1,1,2) = \{(0,1,2),(1,0,2),(1,1,0)\}.$các$\beta$chức năng có được bằng cách đảo ngược từng chuyển động trong số ba chuyển động tọa độ. Điều này tạo ra: 

cho$(0,0,0)$,$\beta(0,0,0) = \{(1,0,0),(0,1,0),(0,0,2)\}.$Vì$(0,0,1)$,$\beta(0,0,1) = \{(1,0,1),(0,1,1),(0,0,0)\}.$Vì$(0,0,2)$,$\beta(0,0,2) = \{(1,0,2),(0,1,2),(0,0,1)\}.$Vì$(0,1,0)$,$\beta(0,1,0) = \{(1,1,0),(0,0,0),(0,1,2)\}.$Vì$(0,1,1)$,$\beta(0,1,1) = \{(1,1,1),(0,0,1),(0,1,0)\}.$Vì$(0,1,2)$,$\beta(0,1,2) = \{(1,1,2),(0,0,2),(0,1,1)\}.$Vì$(1,0,0)$,$\beta(1,0,0) = \{(0,0,0),(1,1,0),(1,0,2)\}.$Vì$(1,0,1)$,$\beta(1,0,1) = \{(0,0,1),(1,1,1),(1,0,0)\}.$Vì$(1,0,2)$,$\beta(1,0,2) = \{(0,0,2),(1,1,2),(1,0,1)\}.$Vì$(1,1,0)$,$\beta(1,1,0) = \{(0,1,0),(1,0,0),(1,1,2)\}.$Vì$(1,1,1)$,$\beta(1,1,1) = \{(0,1,1),(1,0,1),(1,1,0)\}.$Vì$(1,1,2)$,$\beta(1,1,2) = \{(0,1,2),(1,0,2),(1,1,1)\}.$Mỗi mục tiếp theo trực tiếp từ việc áp dụng các phép toán tuần hoàn trước và sau theo tọa độ trên$C_2 \times C_2 \times C_3$. Điều này hoàn thành việc tính toán của$\alpha$Và$\beta$các hàm cho hình xuyến 2 × 2 × 3. ∎
