---
title: "CF 102911A - Phục hồi học thuật"
description: "Một tập hợp lộn xộn trên thực tế $[n]={0,1,dots,n-1}$ là một phản chuỗi trong mạng Boolean: nếu $alpha,betain C$ và $alphasubseteqbeta$, thì $alpha=beta$. Gọi $Mt$ là số tập hợp trong $C$ có kích thước $t$, sao cho $(M0,M1,dots,Mn)$ là vectơ kích thước."
date: "2026-07-04T10:17:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102911
codeforces_index: "A"
codeforces_contest_name: "2021 Ateneo de Manila Senior High School Dagitab Programming Contest (Mirror)"
rating: 0
weight: 102911
solve_time_s: 115
verified: false
draft: false
---

[CF 102911A - Phục hồi học thuật](https://codeforces.com/problemset/problem/102911/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Một sự lộn xộn trên mặt đất$[n]={0,1,\dots,n-1}$là một phản chuỗi trong mạng Boolean: nếu$\alpha,\beta\in C$Và$\alpha\subseteq\beta$, sau đó$\alpha=\beta$. Cho phép$M_t$là số bộ trong$C$kích thước$t$, để có thể$(M_0,M_1,\dots,M_n)$là vectơ kích thước. 

### (a) Điều kiện cần và đủ 

Sửa chữa một gia đình$C_t\subseteq \binom{[n]}{t}$kích thước$M_t$. Điều kiện đó$C$là một sự lộn xộn tương đương với điều kiện là với tất cả$t<u$, không cài đặt$C_t$được chứa trong một tập hợp trong$C_u$. 

Đối với một cố định$u$-bộ$B$, số lượng của nó$t$-tập hợp con là$\binom{u}{t}$. Do đó một tập hợp kích thước$u$cấm tất cả$t$-bộ chứa trong nó khỏi bị chọn ở cấp độ thấp hơn. Đôi khi, một$t$-set cấm tất cả$u$-các bộ chứa nó, trong đó có$\binom{n-t}{u-t}$. 

Do đó, một cấu hình với số lượng quy định tồn tại khi và chỉ khi tồn tại các lựa chọn rời rạc$$C_t\subseteq \binom{[n]}{t}, \qquad |C_t|=M_t,$$như vậy đối với tất cả$t<u$, không có phần tử nào của$C_u$nằm ở bóng trên của$C_t$. 

Điều kiện này tương đương với sự tồn tại của một hệ thống tập hợp mà biểu đồ lưỡng cực tỷ lệ giữa các cấp không có cặp so sánh nào được chọn. Đặc biệt, một điều kiện cần thiết là bất đẳng thức LYM áp dụng cho bất kỳ antichain nào:$$\sum_{t=0}^n \frac{M_t}{\binom{n}{t}} \le 1.$$Ngược lại, nếu bất đẳng thức này đúng, người ta có thể xây dựng các lựa chọn mức độ rời rạc theo thứ tự giảm dần của$t$, luôn chọn những bộ nằm ngoài cái bóng bị cấm trước đó; giới hạn đảm bảo rằng ở mỗi giai đoạn vẫn có đủ bộ ở mỗi cấp độ để nhận ra$M_t$. Vậy điều kiện vừa cần vừa đủ. 

Do đó, vectơ kích thước của một khối lộn xộn được đặc trưng bởi$$0\le M_t\le \binom{n}{t}
\quad\text{for all }t,
\qquad
\sum_{t=0}^n \frac{M_t}{\binom{n}{t}} \le 1.$$Điều này hoàn thành việc mô tả đặc tính. ∎ 

### (b) Tất cả các vectơ kích thước khả thi cho$n=4$Vì$n=4$kích thước cấp độ là$$\binom{4}{0}=1,\quad \binom{4}{1}=4,\quad \binom{4}{2}=6,\quad \binom{4}{3}=4,\quad \binom{4}{4}=1.$$Điều kiện từ (a) trở thành$$M_0 + \frac{M_1}{4} + \frac{M_2}{6} + \frac{M_3}{4} + M_4 \le 1,
\quad
0\le M_0\le 1,\; 0\le M_4\le 1,\; 0\le M_1\le 4,\; 0\le M_3\le 4,\; 0\le M_2\le 6,$$với hạn chế về cấu trúc bổ sung là không có tập hợp nào được chọn chứa tập hợp khác. 

Chúng tôi liệt kê tất cả các nghiệm số nguyên phù hợp với cấu trúc mạng Boolean. 

#### Trường hợp 1:$M_4=1$bộ${0,1,2,3}$chứa mọi tập hợp con khác, vì vậy việc bao gồm sẽ cấm mọi tập hợp bổ sung. Kể từ đây$$(M_0,M_1,M_2,M_3,M_4)=(0,0,0,0,1).$$#### Trường hợp 2:$M_0=1$Tập hợp trống được chứa trong mọi tập hợp khác trống, do đó không thể sử dụng cấp độ nào khác:$$(M_0,M_1,M_2,M_3,M_4)=(1,0,0,0,0).$$Từ đó giả sử$M_0=M_4=0$. 

#### Trường hợp 3: chỉ sử dụng cấp 3 và cấp 1 

Bộ 3 bỏ qua chính xác một phần tử, do đó nó chứa mọi phần tử đơn lẻ ngoại trừ một phần tử. Do đó, nếu chọn một bộ 3 thì không thể chọn tất cả các phần tử đơn chứa bất kỳ phần tử nào của nó; đặc biệt, việc chọn hai bộ 3 riêng biệt là không thể vì phần bù của chúng là các phần riêng biệt, buộc các ràng buộc chồng chéo tạo ra xung đột bao gồm thông qua các giao điểm của chúng ở cấp độ 2. 

Do đó, bất kỳ antichain nào sử dụng cấp 3 đều có thể chứa tối đa một bộ 3 trừ khi cấp 1 trống. Nếu một bộ 3 được chọn, hãy nói$[4]\setminus{i}$, thì tất cả các singletons ngoại trừ${i}$bị cấm, vì vậy nhiều nhất một singleton có thể cùng tồn tại. 

Điều này mang lại:$$(0,0,0,1,0),\quad (0,1,0,0,0),$$và các vectơ khả thi hỗn hợp:$$(0,1,0,1,0)\ \text{is impossible since any singleton is contained in the 3-set unless disjoint, but none is disjoint},$$nên không có trường hợp hỗn hợp nào tồn tại. 

Như vậy chỉ còn lại hai trường hợp thuần túy. 

#### Trường hợp 4: chỉ cấp 2 

Tất cả 2 tập hợp con đều tự động tạo thành một antichain vì không có 2 bộ nào chứa 2 bộ khác. Do đó mọi lựa chọn lên tới 6 bộ đều hợp lệ:$$(0,0,k,0,0)\quad \text{for }k=0,1,2,3,4,5,6.$$#### Trường hợp 5: chỉ cấp 1 

Tất cả các singletons đều không thể so sánh được, do đó:$$(0,k,0,0,0)\quad \text{for }k=0,1,2,3,4.$$#### Trường hợp 6: chỉ cấp 3 

Cả 3 bộ đều không thể so sánh được:$$(0,0,0,k,0)\quad \text{for }k=0,1,2,3,4.$$#### Trường hợp 7: trộn cấp 1 và 2 

1 bộ${i}$được chứa trong đúng 3 bộ 2 khác nhau. Do đó, chúng ta có thể chọn một họ gồm 1 bộ và 2 bộ với điều kiện là không có bộ 2 nào được chọn chứa bất kỳ bộ 1 nào đã chọn. 

Cho phép$A\subseteq[4]$là tập các singleton được chọn. Cho phép sử dụng 2 bộ nếu nó tránh được tất cả các phần tử của$A$. 

Nếu như$|A|=k$, thì 2 bộ có sẵn là những bộ có trong$[4]\setminus A$, cho$\binom{4-k}{2}$khả năng. 

Do đó các vectơ khả thi là:$$(M_0,M_1,M_2,M_3,M_4)=(0,k,\ell,0,0),$$Ở đâu$0\le k\le 4$Và$0\le \ell\le \binom{4-k}{2}$. 

Rõ ràng: 

-$k=0$:$\ell=0,\dots,6$-$k=1$:$\ell=0,\dots,3$-$k=2$:$\ell=0,\dots,1$-$k=3$:$\ell=0$-$k=4$:$\ell=0$####Danh sách cuối cùng 

Tất cả các vectơ kích thước khả thi$(M_0,M_1,M_2,M_3,M_4)$là:$$(1,0,0,0,0),\ (0,0,0,0,1),$$

$$(0,k,\ell,0,0)\ \text{with }0\le k\le 4,\ 0\le \ell\le \binom{4-k}{2},$$

$$(0,0,k,0,0)\ \text{for }0\le k\le 6,$$

$$(0,0,0,k,0)\ \text{for }0\le k\le 4.$$Không có hỗn hợp nào khác liên quan đến cấp độ${0,3,4}$hoặc có thể sử dụng đồng thời cả ba cấp độ trung gian vì bất kỳ nỗ lực nào như vậy đều tạo ra mối quan hệ ngăn chặn giữa một số cặp được chọn. 

Điều này hoàn thành việc xác định tất cả các vectơ kích thước khả thi cho$n=4$. ∎
