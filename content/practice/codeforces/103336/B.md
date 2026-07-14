---
title: "CF 103336B - Lễ Hội Quà Tặng"
description: "Cho $rs,dots,r0$ thỏa mãn $$t = rs + cdots + r1 + r0,qquad 0 le rj le mj quad (s ge j ge 0).$$ Viết $$Mj = sum{i=0}^j mi,qquad Tj = t - sum{i=j+1}^s ri,$$ vì vậy $Tj$ là tổng còn lại được phân bổ giữa các chỉ số $0,dots,j$ sau khi sửa $rs,dấu chấm,r{j+1}$."
date: "2026-07-03T14:00:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103336
codeforces_index: "B"
codeforces_contest_name: "OPEI 2021 - Senior"
rating: 0
weight: 103336
solve_time_s: 157
verified: false
draft: false
---

[CF 103336B - Lễ hội quà tặng](https://codeforces.com/problemset/problem/103336/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$r_s,\dots,r_0$thỏa mãn$$t = r_s + \cdots + r_1 + r_0,\qquad 0 \le r_j \le m_j \quad (s \ge j \ge 0).$$Viết$$M_j = \sum_{i=0}^j m_i,\qquad T_j = t - \sum_{i=j+1}^s r_i,$$Vì thế$T_j$là số tiền còn lại được phân phối giữa các chỉ số$0,\dots,j$sau khi sửa$r_s,\dots,r_{j+1}$. 

Tại vị trí$j$, các giá trị của$r_j$bị hạn chế bởi tính khả thi của việc hoàn thành tác phẩm. Sau khi chọn$r_j$, giá trị còn lại$T_{j-1} = T_j - r_j$phải thỏa mãn$$0 \le T_{j-1} \le M_{j-1}.$$Kể từ đây$$T_j - M_{j-1} \le r_j \le T_j,$$cùng với$0 \le r_j \le m_j$. Do đó khoảng cho phép là$$L_j = \max(0,\, T_j - M_{j-1}),\qquad U_j = \min(m_j,\, T_j).$$Thứ tự từ điển trên$(r_s,\dots,r_0)$được chụp cùng với$r_s$quan trọng nhất, vì vậy$r_0$thay đổi nhanh nhất. 

Giải pháp đầu tiên thu được bằng cách chọn từng thành phần ở giá trị khả thi tối thiểu của nó với$T_s = t$:$$r_j = L_j \quad (s \ge j \ge 0).$$### Thuật toán B (Bố cục giới hạn) 

Lính gác$M_{-1} = 0$,$r_{s+1} = 0$được sử dụng để lập chỉ mục thống nhất. 

**B1. [Khởi tạo.]** Đặt$r_j \leftarrow 0$vì$0 \le j \le s$. Bộ$r_{s+1} \leftarrow 0$. Bộ$T \leftarrow t$. Tính toán$M_j = \sum_{i=0}^j m_i$vì$0 \le j \le s$. 

Vì$j$từ$s$xuống$0$, bộ$$r_j \leftarrow \max(0,\, T - M_{j-1}),$$sau đó cập nhật$T \leftarrow T - r_j$. 

**B2. [Chuyến thăm.]** Ghé thăm$(r_s,\dots,r_0)$. 

**B3. [Tìm thấy$j$.]** Bộ$j \leftarrow 0$. Trong khi$j \le s$Và$$r_j = U_j,$$bộ$j \leftarrow j+1$. 

**B4. [Xong?]** Nếu$j > s$, chấm dứt. 

**B5. [Tăng$r_j$.]** Bộ$T \leftarrow T + r_j$. Thay thế$r_j \leftarrow r_j + 1$. Sau đó cho$k = j-1, j-2, \dots, 0$, bộ$$r_k \leftarrow L_k(T),$$Ở đâu$L_k(T) = \max(0,, T - M_{k-1})$được tính với số tiền còn lại hiện tại$T$, và cập nhật$T \leftarrow T - r_k$. Trở lại B2. 

Tính đúng đắn suy ra từ bất biến trên$T_j$và giới hạn khả thi. Tại mỗi bước B3, chỉ số$0,\dots,j-1$đang ở giá trị khả thi tối đa của chúng, do đó, bất kỳ sự gia tăng nào của$r_j$duy trì tính tối thiểu từ điển của hậu tố. Bước B5 khôi phục khả năng hoàn thành tối thiểu khả thi theo tổng còn lại được cập nhật, đảm bảo tạo ra cấu hình từ điển tiếp theo. Sự kiệt sức xảy ra chính xác khi không thể tăng chỉ số nào, tương đương với$r_j = U_j$cho tất cả$j$, do đó tất cả các thành phần giới hạn được tạo một lần và chỉ một lần. 

Điều này hoàn thành việc chứng minh. ∎
