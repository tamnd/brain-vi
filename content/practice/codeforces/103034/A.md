---
title: "CF 103034A - Pacman và Power Pellet"
description: "Đặt $$Fn(z)=prod{j=0}^{n-1}(1+z+cdots+z^{sj}), qquad left(!binom{S(n)}{k}!right)=[z^k]Fn(z).$$ Sau đó $Fn=F{n-1}(1+z+cdots+z^{s{n-1}})$, do đó việc trích xuất hệ số sẽ cho kết quả $$left(!binom{S(n)}{k}!right) = sum{r=0}^{s{n-1}}left(!binom{S(n-1)}{k-r}!"
date: "2026-07-04T05:21:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103034
codeforces_index: "A"
codeforces_contest_name: "April Fools Contest 2021 Archive (ZS)"
rating: 0
weight: 103034
solve_time_s: 139
verified: false
draft: false
---

[CF 103034A - Pacman và Power Pellet](https://codeforces.com/problemset/problem/103034/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$$F_n(z)=\prod_{j=0}^{n-1}(1+z+\cdots+z^{s_j}),
\qquad 
\left(\!\binom{S(n)}{k}\!\right)=[z^k]F_n(z).$$Sau đó$F_n=F_{n-1}(1+z+\cdots+z^{s_{n-1}})$, do đó việc trích xuất hệ số cho$$\left(\!\binom{S(n)}{k}\!\right)
=
\sum_{r=0}^{s_{n-1}}\left(\!\binom{S(n-1)}{k-r}\!\right),$$với quy ước rằng$\left(!\binom{S(n-1)}{k-r}!\right)=0$khi$k-r<0$. Đây chính xác là sự tương tự của quy tắc Pascal, xuất phát trực tiếp từ việc tích chập các hệ số. 

Sửa chữa$k$. Đối với mỗi$n$, trình tự$\left(!\binom{S(n)}{k}!\right)$đang gia tăng nghiêm trọng trong$n$bất cứ khi nào$k\le \sum_{j=0}^{n-1}s_j$, kể từ khi mở rộng$n$giới thiệu những đóng góp không âm mới vào tích chập ở trên và ít nhất một số hạng trở nên hoàn toàn dương khi$k$là khả thi đối với yếu tố mới. Đặc biệt, với mỗi cố định$k$có một mức tối thiểu duy nhất$n$với$\left(!\binom{S(n)}{k}!\right)>0$. 

### Sự tồn tại của đại diện 

hãy để$N\ge 0$và sửa chữa$t$. Định nghĩa$n_t$là chỉ số lớn nhất của biểu mẫu$s_j\cdot j$như vậy$$\left(\!\binom{S(n_t)}{t}\!\right)\le N.$$Như vậy$n_t$tồn tại bởi vì$\left(!\binom{S(n)}{t}!\right)$cuối cùng vượt quá$N$BẰNG$n$phát triển và nó đang gia tăng trong$n$. 

Bộ$$N^{(t-1)} = N - \left(\!\binom{S(n_t)}{t}\!\right).$$Từ danh tính tích chập,$$\left(\!\binom{S(n_t)}{t}\!\right)
=
\left(\!\binom{S(n_t-1)}{t}\!\right)
+
\left(\!\binom{S(n_t-1)}{t-1}\!\right)
+
\cdots
+
\left(\!\binom{S(n_t-1)}{t-s_{n_t-1}}\!\right),$$thật là trừ$\left(!\binom{S(n_t)}{t}!\right)$loại bỏ tất cả các cấu hình có tọa độ cuối cùng nằm trong$[0,s_{n_t-1}]$. Phần còn lại$N^{(t-1)}$do đó có thể biểu diễn được bằng cách chỉ sử dụng các chỉ số nhỏ hơn$n_t$. 

Việc lặp lại cùng một công trình sẽ tạo ra$n_{t-1}\le n_t$như vậy$$N^{(t-1)}=\left(\!\binom{S(n_{t-1})}{t-1}\!\right)+N^{(t-2)},$$và lợi tức tiếp tục$$N=
\left(\!\binom{S(n_t)}{t}\!\right)+
\left(\!\binom{S(n_{t-1})}{t-1}\!\right)+\cdots+
\left(\!\binom{S(n_1)}{1}\!\right),$$với$n_t\ge n_{t-1}\ge\cdots\ge n_1\ge 0$và mỗi$n_i$rút ra từ tập hợp cho phép${s_0\cdot 0,s_1\cdot 1,\dots}$bởi vì mỗi bước trừ chỉ cho phép các chỉ số tương thích với sự hỗ trợ của việc xác định tích chập$S(\cdot,\cdot)$. 

###Tính độc đáo 

Giả sử tồn tại hai biểu diễn:$$N=\sum_{i=1}^t \left(\!\binom{S(n_i)}{i}\!\right)
=\sum_{i=1}^t \left(\!\binom{S(m_i)}{i}\!\right),
\qquad
n_t\ge\cdots\ge n_1,\; m_t\ge\cdots\ge m_1.$$Cho phép$r$là chỉ số lớn nhất sao cho$n_r\ne m_r$. Không mất tính tổng quát$n_r>m_r$. Khi đó sự đơn điệu trong$n$cho$$\left(\!\binom{S(n_r)}{r}\!\right)\ge \left(\!\binom{S(m_r+1)}{r}\!\right)>\left(\!\binom{S(m_r)}{r}\!\right).$$Tất cả các thuật ngữ có chỉ số cao hơn$i>r$hủy bỏ bằng sự bằng nhau của các tiền tố, do đó vế trái lớn hơn vế phải, mâu thuẫn với đẳng thức của$N$. Lực lượng này$n_r=m_r$cho tất cả$r$, chứng minh tính duy nhất. 

Điều này hoàn thành định lý biểu diễn. ∎ 

### Công thức dành cho$|\partial P_{N_t}|$Trong Hệ quả C, toán tử biên$\partial$hoạt động bằng cách giảm chính xác một tọa độ trong cấu trúc tổ hợp được mã hóa bằng biểu diễn. Mỗi học kỳ$$\left(\!\binom{S(n_i)}{i}\!\right)$$đóng góp chính xác số cách để giảm một trong những$i$các đơn vị được chọn, tương ứng với việc chọn một trong các đơn vị$i$vị trí đóng góp cho thuật ngữ đó. Việc giảm vị trí như vậy sẽ chuyển đổi một khoản đóng góp được tính bằng$S(n_i,i)$thành một được tính bởi$S(n_i,i-1)$. 

Tổng hợp tất cả các cấp mang lại lợi nhuận$$|\partial P_{N_t}|
=
\sum_{i=1}^t \left(\!\binom{S(n_i)}{i-1}\!\right),$$với quy ước$\left(!\binom{S(n_i)}{0}!\right)=1$. 

Ranh giới phân rã duy nhất giữa các cấp độ vì sự biểu diễn của$N$là duy nhất và mỗi mức giảm ảnh hưởng đến chính xác một lệnh triệu tập mà không bị chồng chéo giữa các lệnh khác nhau.$i$. ∎
