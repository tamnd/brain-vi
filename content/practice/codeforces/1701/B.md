---
title: "CF 1701B - Hoán vị"
description: "Hai cấu trúc được đề xuất để tạo ra một biến ngẫu nhiên $X$ với phân phối không tầm thường trên $[-1,1]$. Ánh xạ đầu tiên của một bộ đồng nhất bị lệch thông qua phép biến đổi lượng giác."
date: "2026-06-09T21:46:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1701
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 131 (Rated for Div. 2)"
rating: 800
weight: 1701
solve_time_s: 60
verified: false
draft: false
---

[CF 1701B - Hoán vị](https://codeforces.com/problemset/problem/1701/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hai công trình được đề xuất để tạo ra một biến ngẫu nhiên$X$với sự phân phối không cần thiết trên$[-1,1]$. Ánh xạ đầu tiên của một bộ đồng nhất bị lệch thông qua phép biến đổi lượng giác. Cách thứ hai sử dụng lấy mẫu từ chối trong đĩa đơn vị và sau đó áp dụng phép biến đổi hợp lý của điểm được chấp nhận. 

Câu hỏi đặt ra là liệu hai thủ tục này có tạo ra sự phân bổ giống nhau cho$X$. 

Cho phép$U$được phân bố đều trên$[0,1]$. Phương pháp 1 xác định$$X = \sin\!\left(\frac{\pi}{2}U\right).$$Từ$U$là đồng nhất, biến$T = \frac{\pi}{2}U$là thống nhất trên$\left[0, \frac{\pi}{2}\right]$. Sự biến đổi$X = \sin T$là đơn điệu tăng lên trong khoảng thời gian này, do đó sự phân bố của$X$được xác định bằng cách đảo ngược ánh xạ. 

Vì$-1 \le x \le 1$, sự kiện$X \le x$tương đương với$$\sin T \le x \quad \Longleftrightarrow \quad T \le \arcsin x,$$từ$\sin T$đang gia tăng. Vì thế$$\Pr(X \le x) = \Pr\!\left(T \le \arcsin x\right) = \frac{2}{\pi}\arcsin x.$$Do đó Phương pháp 1 tạo ra hàm phân phối$$F_1(x) = \frac{2}{\pi}\arcsin x.$$Cách 2 tạo thống nhất độc lập$U, V$TRÊN$[-1,1]$có điều kiện$U^2 + V^2 < 1$. Đây là sự phân bố đồng đều trên đĩa đơn vị. Mật độ khớp không đổi trên đĩa, do đó điểm được chấp nhận là đối xứng hướng tâm. Chúng tôi xác định$$X = \frac{U^2 - V^2}{U^2 + V^2}.$$Giới thiệu tọa độ cực trên đĩa. Cho phép$$U = R\cos\Theta, \quad V = R\sin\Theta,$$Ở đâu$R \in [0,1]$Và$\Theta \in [0,2\pi)$, với$\Theta$thống nhất và độc lập với$R^2$theo nghĩa gây ra bởi thước đo diện tích. Sau đó$$X = \frac{R^2\cos^2\Theta - R^2\sin^2\Theta}{R^2\cos^2\Theta + R^2\sin^2\Theta} = \frac{\cos^2\Theta - \sin^2\Theta}{1} = \cos(2\Theta).$$Biến xuyên tâm bị hủy bỏ hoàn toàn. Sự phân phối của$X$do đó chỉ được xác định bởi$\Theta$, đồng đều trên$[0,2\pi)$. 

Cho phép$T = 2\Theta$, Vì thế$T$là thống nhất trên$[0,4\pi)$. Từ$\cos T$có thời gian$2\pi$, giới hạn trong một khoảng thời gian sẽ đưa ra một biểu diễn tương đương với$T$đồng phục trên$[0,2\pi)$. Do đó chúng ta có thể tính toán phân bố của$X = \cos T$với$T \sim \mathrm{Unif}[0,2\pi)$. 

Vì$-1 \le x \le 1$,$$\Pr(X \le x) = \Pr(\cos T \le x).$$TRÊN$[0,2\pi)$, bộ${\cos T \le x}$gồm hai cung đối xứng có tổng chiều dài là$2\pi - 2\arccos x$. Vì thế$$F_2(x) = 1 - \frac{1}{\pi}\arccos x.$$Sử dụng danh tính$\arccos x = \frac{\pi}{2} - \arcsin x$, chúng tôi có được$$F_2(x) = 1 - \frac{1}{\pi}\left(\frac{\pi}{2} - \arcsin x\right) = \frac{1}{2} + \frac{1}{\pi}\arcsin x.$$Bây giờ hãy so sánh hai hàm phân phối. 

Cách 1 đưa ra$$F_1(x) = \frac{2}{\pi}\arcsin x,$$trong khi Phương pháp 2 cho$$F_2(x) = \frac{1}{2} + \frac{1}{\pi}\arcsin x.$$Các chức năng này không bằng nhau. Ví dụ, tại$x=0$,$$F_1(0) = 0, \qquad F_2(0) = \frac{1}{2}.$$Vì các hàm phân phối khác nhau tại một điểm trong độ hỗ trợ của chúng nên các phân bố cảm ứng sẽ không giống nhau. 

Điều này hoàn thành giải pháp. ∎
