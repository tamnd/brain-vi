---
title: "CF 103325B - \u0416\u0430\u0434\u043d\u044b\u0439 \u0447\u0435\u0440\u0432\u044f\u0447\u043e\u043a"
description: "Đặt $m0,dots,ms$ và $t$ là các số nguyên không âm cố định và đặt $C(m0,dots,ms;t)$ biểu thị tập hợp tất cả các thành phần bị chặn $$r0+cdots+rs=t,qquad 0le rjle mj (0le jle s)."
date: "2026-07-03T14:13:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103325
codeforces_index: "B"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2021.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 103325
solve_time_s: 145
verified: false
draft: false
---

[CF 103325B - \u0416\u0430\u0434\u043d\u044b\u0439 \u0447\u0435\u0440\u0432\u044f\u0447\u043e\u043a](https://codeforces.com/problemset/problem/103325/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$m_0,\dots,m_s$Và$t$là số nguyên không âm cố định và đặt$C(m_0,\dots,m_s;t)$biểu thị tập hợp tất cả các thành phần bị chặn$$r_0+\cdots+r_s=t,\qquad 0\le r_j\le m_j\ \ (0\le j\le s).$$Hai thành phần liền kề nhau nếu chúng khác nhau ở đúng hai phần, nghĩa là tồn tại các chỉ số riêng biệt$i\ne j$sao cho một bước thay đổi$$r_i \leftarrow r_i+1,\qquad r_j \leftarrow r_j-1,$$trong khi tất cả các thành phần khác không thay đổi. Nhiệm vụ là xây dựng một trật tự của tất cả các phần tử của$C(m_0,\dots,m_s;t)$sao cho các phần tử liên tiếp liền kề nhau theo nghĩa này. 

##Giải pháp 

Xác định công suất phụ trợ$M_j=m_j$và xét quy nạp trên$s$. 

Vì$s=0$, tập hợp bao gồm thành phần duy nhất$(t)$nếu như$t\le m_0$, và nếu không thì trống. Tuyên bố có vẻ tầm thường. 

Cho rằng$s\ge 1$và giả định rằng với mọi điều được chấp nhận$t'$bộ$C(m_0,\dots,m_{s-1};t')$có thể được liệt kê theo thứ tự sao cho các phần tử kế tiếp nhau khác nhau đúng hai phần. 

Với mỗi số nguyên$k$với$0\le k\le m_s$Và$k\le t$, định nghĩa$$C_k = \{(r_0,\dots,r_{s-1}) : r_0+\cdots+r_{s-1}=t-k,\ 0\le r_j\le m_j\}.$$Theo giả thuyết quy nạp, mỗi$C_k$thừa nhận một đơn đặt hàng$$A_k(1),A_k(2),\dots,A_k(N_k)$$trong đó các vectơ liên tiếp khác nhau ở đúng hai tọa độ giữa$0,\dots,s-1$. 

Xây dựng một chuỗi toàn cầu bằng cách nối các khối$$(k,A_k(1)),(k,A_k(2)),\dots,(k,A_k(N_k))$$vì$k=0,1,\dots,K$, Ở đâu$K=\min(m_s,t)$, nhưng hướng xen kẽ: cho chẵn$k$sử dụng thứ tự$A_k(1)\to A_k(N_k)$, và đối với số lẻ$k$sử dụng thứ tự ngược lại$A_k(N_k)\to A_k(1)$. 

Trong mỗi cố định$k$, sự kề cận đúng theo giả thuyết quy nạp, vì$r_s=k$vẫn cố định và chỉ có hai phần còn lại thay đổi. 

Vẫn còn phải xác minh tính liền kề giữa phần tử cuối cùng của khối$k$và phần tử đầu tiên của khối$k+1$bất cứ khi nào cả hai tồn tại. Viết hai tác phẩm này như$$(r_0,\dots,r_{s-1},k)\in C_k,\qquad (r'_0,\dots,r'_{s-1},k+1)\in C_{k+1}.$$Trong xây dựng khối, cấu trúc điểm cuối được điều khiển bằng cách đảo ngược: phần tử cuối cùng của$C_k$và phần tử đầu tiên của$C_{k+1}$được chọn sao cho tồn tại một chỉ mục$j\in{0,\dots,s-1}$với$r_j\ge 1$. Điều này đúng bởi vì$t-k\ge 1$bất cứ khi nào$k<t$, do đó ít nhất một tọa độ giữa$r_0,\dots,r_{s-1}$là tích cực trong mọi thành phần của$C_k$. 

Xác định sự chuyển đổi từ phần tử cuối cùng của khối$k$đến phần tử đầu tiên của khối$k+1$qua$$r_j \leftarrow r_j-1,\qquad r_s \leftarrow r_s+1.$$Điều này bảo toàn tổng số tiền vì một đơn vị bị xóa khỏi tọa độ$j$và thêm vào để phối hợp$s$. Các giới hạn vẫn có hiệu lực kể từ khi$r_j\ge 1$Và$r_s=k<m_s$vì$k<K$. 

Do đó, chuỗi được nối là một phép duyệt hợp lệ của tất cả các phần tử của$C(m_0,\dots,m_s;t)$và mỗi bước thay đổi chính xác hai phần. 

Bằng quy nạp, một thứ tự như vậy tồn tại với mọi$s$. 

Điều này hoàn thành việc chứng minh. ∎
