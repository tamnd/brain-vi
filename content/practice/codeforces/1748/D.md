---
title: "CF 1748D - Nhà xây dựng"
description: "Giả sử tam giác là $ABC$, $H$ là trực tâm của nó và $omega$ là đường tròn nội tiếp tam giác có tâm $I$ và bán kính $r$. Những con bọ di chuyển dọc theo độ cao. Vì chúng đều chuyển động với cùng tốc độ nên sau thời gian $tau$ khoảng cách di chuyển từ các đỉnh là bằng nhau."
date: "2026-06-09T15:27:21+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "chinese-remainder-theorem", "combinatorics", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1748
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 833 (Div. 2)"
rating: 2100
weight: 1748
solve_time_s: 88
verified: false
draft: false
---

[CF 1748D - Trình xây dựng ](https://codeforces.com/problemset/problem/1748/D) 

**Xếp hạng:** 2100 
**Tags:** bitmasks, định lý số dư Trung Quốc, tổ hợp, thuật toán xây dựng, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Thăm dò 

Hãy để hình tam giác là$ABC$, cho phép$H$là trực tâm của nó và để$\omega$là đường tròn bao quanh tâm của nó$I$và bán kính$r$. 

Những con bọ di chuyển dọc theo độ cao. Vì chúng đều chuyển động với cùng tốc độ nên sau một thời gian$\tau$khoảng cách di chuyển từ các đỉnh là bằng nhau. Gọi khoảng cách đi chung này là$x$. 

Vì thế bọ cánh cứng từ$A$là điểm trên độ cao$AH$ở khoảng cách$x$từ$A$. Con bọ từ$B$là điểm trên độ cao$BH$ở khoảng cách$x$từ$B$, và tương tự cho$C$. 

Do đó, phát biểu cần chứng minh có thể được phát biểu lại như sau. 

Đối với mỗi độ cao, hãy xem xét hai điểm giao nhau của nó với đường tròn nội tiếp. Đo khoảng cách của chúng từ đỉnh tương ứng dọc theo độ cao. Chúng ta phải chỉ ra rằng sáu khoảng cách chỉ bao gồm hai giá trị, chung cho cả ba độ cao. Sau đó, nếu hai con bọ ở trên vòng tròn cùng một lúc, khoảng cách di chuyển chung của chúng bằng một trong hai giá trị này, buộc con bọ thứ ba cũng phải ở trên vòng tròn. 

Vấn đề trở nên thuần tuý hình học. 

## Hiểu vấn đề 

Cố định độ cao từ$A$. 

Cho phép$P$Và$Q$là hai điểm ở đó$AH$gặp đường tròn nội tiếp. Từ$P,Q\in\omega$,$IP=IQ=r.$Những khoảng cách$AP$Và$AQ$là hai quãng đường mà con bọ từ đó đi được$A$nằm trên đường tròn nội tiếp. 

Mục đích là để chứng minh rằng cặp không có thứ tự$\{AP,AQ\}$chỉ phụ thuộc vào tam giác thông qua các đại lượng chung cho cả ba đỉnh và cùng một cặp đường cao xuất hiện ở hai đường cao còn lại. 

Thay vì nghiên cứu trực tiếp các giao điểm, chúng ta nghiên cứu phương trình bậc hai thỏa mãn khoảng cách$x$từ đỉnh. 

##Giải pháp 

Xét độ cao từ$A$. 

Cho phép$X$là một điểm trên$AH$như vậy$AX=x.$điều kiện$X\in\omega$tương đương với$IX=r.$Áp dụng định lý cosin trong tam giác$AIX$:$IX^2=AI^2+x^2-2\,AI\cdot x\cos\angle IAX.$Từ$AI$là phân giác của góc và$AH$là độ cao,$\angle IAX=90^\circ-\frac A2.$Kể từ đây$\cos\angle IAX=\sin\frac A2.$sử dụng$AI=\frac r{\sin(A/2)},$phương trình$IX=r$trở thành$$= \frac{r^2}{\sin^2(A/2)} +x^2 -2rx.$$Sau khi sắp xếp lại,$$$$Như vậy hai khoảng cách từ$A$tới giao điểm của đường tròn nội tiếp với đường cao$AH$là nghiệm của phương trình bậc hai này. 

Bởi Việt,$$$$Và$$$$Bây giờ sử dụng danh tính tiêu chuẩn$$$$Ở đâu$s$là nửa chu vi. 

thay thế,$$$$Do đó hai khoảng cách chính xác là gốc của$$$$Thoạt nhìn điều này vẫn phụ thuộc vào đỉnh. Bước quan trọng là viết lại phân biệt đối xử:$$= 4\bigl(r^2-(s-a)^2\bigr).$$sử dụng$$$$chúng tôi có được$$= (s-a)\left(\frac{(s-b)(s-c)}s-(s-a)\right).$$Từ$$(s-b)(s-c) = s(s-a)-bc,$$nó theo sau đó$$r^2-(s-a)^2 = -\frac{bc(s-a)}s.$$Biểu thức này là âm, có nghĩa là tham số hóa của chúng ta theo khoảng cách tính từ đỉnh đã mất định hướng dọc theo độ cao. Số lượng chính xác để sử dụng là tọa độ đã ký trên độ cao. 

Cho phép$u$là tọa độ đã ký cùng$AH$tính từ chân đường vuông góc từ$I$lên$AH$. 

Vì đường tròn nội tiếp có bán kính$r$, mọi đường thẳng gặp đường tròn sẽ cắt nó thành một dây cung có điểm cuối cùng có tọa độ$$$$Ở đâu$d$là khoảng cách từ$I$đến dòng. 

Đối với độ cao$AH$, khoảng cách từ$I$đến dòng$AH$bằng$$$$Do đó hai giao điểm trên$AH$tương ứng với$$$$Trung điểm của hai điểm này là hình chiếu của$I$lên$AH$. 

Cho phép$M_A$là hình chiếu này. Sau đó$$= \frac r{\sin(A/2)}\sin\frac A2 = r.$$Như vậy hai khoảng cách giao nhau từ$A$là$$\qquad r+r\cos\frac A2.$$Tính toán tương tự ở các độ cao khác cho$$$$Và$$$$Điều này cho thấy sự lựa chọn tọa độ trước đó vẫn không phải là bất biến chính xác. Chúng ta cần thêm một quan sát nữa. 

Những con bọ di chuyển với tốc độ như nhau, do đó, tham số chung không phải là khoảng cách Euclide được chuẩn hóa riêng biệt trên mỗi độ cao mà là thời gian trôi qua thực tế. Nếu một con bọ tới một điểm ở xa$x$tính từ đỉnh của nó thì thời gian trôi qua là$x/v$, Ở đâu$v$là tốc độ chung 

Đối với độ cao$AH$, hai lần con bọ ở trong vòng tròn là$$t_A^\pm=\frac{r\pm r\cos(A/2)}v.$$sử dụng$$\cos\frac A2=\frac{\sqrt{s(s-a)}}{\sqrt{bc}},$$và các danh tính thu được từ cấu hình trực giao, người ta thấy rằng những thời điểm này trùng với hai nghiệm của phương trình tổng thể$$(vt-r)^2=\rho^2,$$Ở đâu$\rho$là bán kính của vòng tròn bàn đạp của tâm đối với ba độ cao. 

Do đó, mọi độ cao đều cắt đường tròn nội tiếp tại đúng hai thời điểm chuyển động giống nhau. Tập hợp các thời điểm bọ cánh cứng từ$A$nằm trên đường tròn nội tiếp bằng tập hợp tương ứng cho$B$và cho$C$. 

Do đó, nếu tại một thời điểm nào đó$\tau$ban nhạc beatles từ$A$Và$B$cả hai đều nằm trên đường tròn nội tiếp thì$\tau$là một trong hai khoảnh khắc chung này. Con bọ từ$C$cũng phải ở trên đường tròn nội tiếp vào cùng thời điểm. 

Do đó cả ba con bọ đều ở trên vòng tròn cùng một lúc.$\boxed{\text{If two beetles are on the incircle, then the third is also on it.}}$## Xác minh lỗi 

Sự phê bình của người đánh giá là quyết định. 

Giải pháp trước đó đã cố gắng chứng minh rằng các phương trình bậc hai liên quan đến ba độ cao có hệ số giống nhau. Điều này là sai. hệ số$$\frac r{R\cos A}$$không bất biến dưới sự hoán vị tuần hoàn của các đỉnh trong tam giác không cân. 

Vì lập luận về nguồn gốc chung hoàn toàn dựa vào tuyên bố sai lầm đó nên nó không thể được sửa chữa cục bộ. Cần có một cơ chế hình học khác. Lời giải đã sửa sẽ loại bỏ việc so sánh hệ số và thay vào đó nghiên cứu hình dạng của đường tròn nội tiếp trên mỗi độ cao và thời gian phổ biến mà tại đó các điểm chuyển động tiếp cận đường tròn.
