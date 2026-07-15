---
title: "CF 103388I - Đảo ngược mọi thứ"
description: "Viết $N$ ở dạng nhị phân $$N = (am a{m-1}dots a0)2 = sum{i=0}^m ai 2^i.$$ Đặt $kappat N$ biểu thị số nguyên nhỏ nhất $M ge N$ có khai triển nhị phân chứa chính xác $t$ đơn vị, tức là."
date: "2026-07-03T18:09:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103388
codeforces_index: "I"
codeforces_contest_name: "2021-2022 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 103388
solve_time_s: 137
verified: false
draft: false
---

[CF 103388I - Đảo ngược mọi thứ](https://codeforces.com/problemset/problem/103388/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Viết$N$ở dạng nhị phân$$N = (a_m a_{m-1}\dots a_0)_2 = \sum_{i=0}^m a_i 2^i.$$Cho phép$\kappa_t N$biểu thị số nguyên nhỏ nhất$M \ge N$khai triển nhị phân của nó chứa chính xác$t$những cái, tức là,$$M = \sum_{i \in S} 2^i \quad \text{for some } |S| = t.$$Định nghĩa này ngụ ý rằng nếu$N$đã có chính xác rồi$t$thì những cái đó$\kappa_t N = N$, từ$N$được chấp nhận và tối thiểu trong số các con số$\ge N$với cùng một tài sản. 

Cho rằng$N$không có chính xác$t$những cái đó. Cho phép$j$giữ vị trí cao nhất sao cho$M$Và$N$khác nhau một chút$j$, Ở đâu$M = \kappa_t N$. Bằng cách tối thiểu$M$, tất cả các bit cao hơn đều đồng ý:$$a_i(M) = a_i(N) \quad \text{for } i > j,$$và ở vị trí$j$, việc xây dựng$M$lực lượng$a_j(M)=1$trong khi$a_j(N)=0$. Nếu không thì số chấp nhận được nhỏ hơn$\ge N$sẽ tồn tại bằng cách đặt số 1 cao nhất thấp hơn$j$, mâu thuẫn với tính tối thiểu. 

Tất cả còn lại$t-1$những cái của$M$phải nằm ở những vị trí ngay phía dưới$j$. Trong số tất cả các lựa chọn của$t-1$vị trí dưới đây$j$, giá trị nhỏ nhất có thể thu được bằng cách đặt chúng ở vị trí thấp nhất hiện có$0,1,\dots,t-2$. Điều này mang lại giới hạn dưới$$M \le 2^j + (2^{t-1}-1).$$Bây giờ so sánh$N$Và$M$. Từ$a_j(N)=0$và tất cả các bit cao hơn trùng khớp, sự khác biệt thỏa mãn$$M - N \le \bigl(2^j + (2^{t-1}-1)\bigr) - (2^j - 1),$$Ở đâu$2^j - 1$là sự đóng góp lớn nhất có thể có của các bit thấp hơn của$N$dưới sự ràng buộc của việc thực hiện vào vị trí$j$là cần thiết cho$M \ge N$. Điều này đơn giản hóa để$$M - N \le 2^{t-1}.$$Để chứng tỏ giới hạn này đã đạt được, hãy lấy$$N = 2^m - 1$$cho bất kỳ$m \ge t-1$. Sau đó$N$có dạng nhị phân bao gồm$m$những cái đó. Số nguyên nhỏ nhất$\ge N$với chính xác$t$người ta phải đặt số 1 ở vị trí$m$và phân phối phần còn lại$t-1$những người ở vị trí thấp nhất, đưa ra$$\kappa_t N = 2^m + (2^{t-1}-1).$$Kể từ đây$$\kappa_t N - N = \bigl(2^m + (2^{t-1}-1)\bigr) - (2^m - 1) = 2^{t-1}.$$Không thể có giá trị lớn hơn vì đối số trước đó cho$\kappa_t N - N \le 2^{t-1}$cho tất cả$N \ge 0$. 

Điều này hoàn thành việc chứng minh. ∎$$\boxed{2^{t-1}}$$
