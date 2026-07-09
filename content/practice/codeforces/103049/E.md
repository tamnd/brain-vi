---
title: "CF 103049E - Hồi kết"
description: "Giả sử hình xuyến $2 nhân 2 nhân 3$ là tích Descartes $C2 nhân C2 nhân C3,$ sao cho các phần tử của nó là bộ ba $(i,j,k)$ với $i trong {0,1}$, $j trong {0,1}$, $k trong {0,1,2}$ và phép cộng được thực hiện theo modulo $2,2,3$ trong tọa độ tương ứng."
date: "2026-07-04T01:38:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103049
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ICPC Northwestern European Regional Programming Contest (NWERC 2020)"
rating: 0
weight: 103049
solve_time_s: 55
verified: false
draft: false
---

[CF 103049E - Hồi kết](https://codeforces.com/problemset/problem/103049/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hãy để$2 \times 2 \times 3$hình xuyến là tích Descartes$C_2 \times C_2 \times C_3,$vậy các phần tử của nó là bộ ba$(i,j,k)$với$i \in {0,1}$,$j \in {0,1}$,$k \in {0,1,2}$, và phép cộng được thực hiện theo modulo$2,2,3$trong tọa độ tương ứng. 

Các chức năng$\alpha$Và$\beta$được liên kết với hình xuyến đóng vai trò là các bản dịch đơn vị trong hai tọa độ tuần hoàn đầu tiên, trong khi giữ nguyên tọa độ thứ ba. Như vậy$\alpha(i,j,k) = (i+1 \bmod 2,\; j,\; k), \qquad \beta(i,j,k) = (i,\; j+1 \bmod 2,\; k).$Vì mỗi chu trình tọa độ là độc lập nên việc áp dụng$\alpha$hoặc$\beta$không ảnh hưởng đến các tọa độ còn lại. Tọa độ thứ ba$k$vẫn bất biến dưới cả hai bản đồ. 

Để tính toán các hàm một cách rõ ràng, hãy liệt kê tất cả$12$đỉnh và áp dụng trực tiếp các định nghĩa. 

Vì$k=0$,$\alpha(0,0,0)=(1,0,0), \quad \beta(0,0,0)=(0,1,0),$

$\alpha(1,0,0)=(0,0,0), \quad \beta(1,0,0)=(1,1,0),$

$\alpha(0,1,0)=(1,1,0), \quad \beta(0,1,0)=(0,0,0),$

$\alpha(1,1,0)=(0,1,0), \quad \beta(1,1,0)=(1,0,0).$Vì$k=1$,$\alpha(0,0,1)=(1,0,1), \quad \beta(0,0,1)=(0,1,1),$

$\alpha(1,0,1)=(0,0,1), \quad \beta(1,0,1)=(1,1,1),$

$\alpha(0,1,1)=(1,1,1), \quad \beta(0,1,1)=(0,0,1),$

$\alpha(1,1,1)=(0,1,1), \quad \beta(1,1,1)=(1,0,1).$Vì$k=2$,$\alpha(0,0,2)=(1,0,2), \quad \beta(0,0,2)=(0,1,2),$

$\alpha(1,0,2)=(0,0,2), \quad \beta(1,0,2)=(1,1,2),$

$\alpha(0,1,2)=(1,1,2), \quad \beta(0,1,2)=(0,0,2),$

$\alpha(1,1,2)=(0,1,2), \quad \beta(1,1,2)=(1,0,2).$Hai bản đồ này liên quan đến hai tọa độ đầu tiên,$\alpha^2 = \beta^2 = \mathrm{id},$và họ đi lại,$\alpha\beta(i,j,k) = \beta\alpha(i,j,k) = (i+1 \bmod 2,\; j+1 \bmod 2,\; k),$do đó hình xuyến được tạo ra bởi hai chuyển động xoắn hoạt động độc lập trên hai yếu tố tuần hoàn đầu tiên. Điều này mang lại đầy đủ$\alpha$Và$\beta$cấu trúc của$2 \times 2 \times 3$hình xuyến. 

Điều này hoàn thành giải pháp. ∎
