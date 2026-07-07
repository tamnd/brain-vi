---
title: "CF 102946C - Gà viên"
description: "Giả sử hình xuyến $2 nhân 2 nhân 3$ là tích Descartes $$T = mathbb{Z}2 nhân mathbb{Z}2 nhân mathbb{Z}3,$$ sao cho mỗi phần tử là một bộ ba $(x,y,z)$ với $x,y trong {0,1}$ và $z trong {0,1,2}$, với số học được lấy modulo $2,2,3$ tương ứng. Điều này mang lại cho đỉnh $12$."
date: "2026-07-04T07:31:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102946
codeforces_index: "C"
codeforces_contest_name: "NCTU PCCA Winter Contest 2021"
rating: 0
weight: 102946
solve_time_s: 152
verified: false
draft: false
---

[CF 102946C - Gà viên](https://codeforces.com/problemset/problem/102946/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Hãy để$2\times 2\times 3$hình xuyến là tích Descartes$$T = \mathbb{Z}_2 \times \mathbb{Z}_2 \times \mathbb{Z}_3,$$vậy mỗi phần tử là một bộ ba$(x,y,z)$với$x,y \in {0,1}$Và$z \in {0,1,2}$, với số học lấy modulo$2,2,3$tương ứng. Điều này mang lại$12$đỉnh. 

Cấu trúc hình xuyến trong (69) là đồ thị Cayley của$T$với các bộ tạo tiêu chuẩn tương ứng với số gia đơn vị trong mỗi tọa độ. Đối với một đỉnh$u=(x,y,z)$, xác định ba lân cận phía trước bằng cách tăng một tọa độ modulo độ dài chu kỳ của nó. Điều này tạo ra cấu trúc di chuyển về phía trước cục bộ xác định$\alpha$. Các chuyển động nghịch đảo xác định$\beta$. 

chức năng$\alpha$ánh xạ từng đỉnh vào tập hợp các đỉnh thu được bằng cách áp dụng một trình tạo chuyển tiếp. Như vậy,$$\alpha(x,y,z)
=
\{(x+1 \bmod 2, y, z),\ (x, y+1 \bmod 2, z),\ (x, y, z+1 \bmod 3)\}.$$chức năng$\beta$ánh xạ từng đỉnh vào tập hợp thu được bằng cách áp dụng các bộ tạo nghịch đảo, trừ đi$1$trong mỗi mô đun tọa độ mô đun tương ứng. Như vậy,$$\beta(x,y,z)
=
\{(x-1 \bmod 2, y, z),\ (x, y-1 \bmod 2, z),\ (x, y, z-1 \bmod 3)\}.$$Viết những điều này một cách rõ ràng bằng cách sử dụng đại diện trong${0,1}$Và${0,1,2}$cho$$x-1 \bmod 2 = 1-x,\quad y-1 \bmod 2 = 1-y,\quad z-1 \bmod 3 =
\begin{cases}
2,& z=0\\
0,& z=1\\
1,& z=2.
\end{cases}$$Kể từ đây$$\beta(x,y,z)
=
\{(1-x, y, z),\ (x, 1-y, z),\ (x, y, z-1 \bmod 3)\}.$$Mỗi đỉnh của hình xuyến có đúng ba$\alpha$-hình ảnh và ba$\beta$-preimages, phù hợp với cấu trúc 3 máy phát điện của$2\times 2\times 3$đồ thị sản phẩm tuần hoàn. Cặp đôi$(\alpha,\beta)$hình thành các toán tử kề tiến và lùi của hình xuyến Cayley này, phù hợp với khuôn khổ đối ngẫu thứ tự chéo của các bài tập trước. 

Điều này hoàn thành việc tính toán. ∎
