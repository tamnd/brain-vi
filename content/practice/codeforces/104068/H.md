---
title: "CF 104068H - Toxel \u4e0e\u5b9d\u53ef\u68a6\u5bf9\u6218\u7279\u8bad"
description: "Đặt $Gamma = (alpha0,ldots,alpha{t-1})$, $Gamma' = (alpha'0,ldots,alpha'{t'-1})$ và $Gamma'' = (alpha''0,ldots,alpha''{t''-1})$."
date: "2026-07-02T03:05:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104068
codeforces_index: "H"
codeforces_contest_name: "The 17-th Beihang University Collegiate Programming Contest (BCPC 2022) - Preliminary"
rating: 0
weight: 104068
solve_time_s: 94
verified: false
draft: false
---

[CF 104068H - Toxel \u4e0e\u5b9d\u53ef\u68a6\u5bf9\u6218\u7279\u8bad](https://codeforces.com/problemset/problem/104068/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$\Gamma = (\alpha_0,\ldots,\alpha_{t-1})$,$\Gamma' = (\alpha'_0,\ldots,\alpha'_{t'-1})$, Và$\Gamma'' = (\alpha''_0,\ldots,\alpha''_{t''-1})$. Sản phẩm Boustrophedon$\Gamma ,\≀, \Gamma'$tạo thành một chuỗi tất cả các nối$\alpha_i\alpha'_j$Ở đâu$0 \le i < t$Và$0 \le j < t'$, được sắp xếp bằng cách quét$j$từ trái sang phải khi$i$chẵn và từ phải sang trái khi$i$thật kỳ quặc. 

Cấu trúc của sản phẩm giống hệt với cấu trúc đệ quy tiêu chuẩn của các chuỗi Gray trong (5) và quan sát này xác định chiến lược chứng minh. Thực tế quan trọng là đối với mỗi$n \ge 1$, dãy Gray$\Gamma_n$thỏa mãn$$\Gamma_n = (0,1) \,\≀\, \Gamma_{n-1}.$$Danh tính này sửa chữa hành vi của$\Gamma_n$duy nhất, vì (5) xác định đệ quy toàn bộ chuỗi từ$\Gamma_0 = \epsilon$. 

Xác định một hoạt động nhị phân$\star$theo trình tự bởi$\Gamma \star \Gamma' = \Gamma ,\≀, \Gamma'$. Đầu tiên chúng tôi chỉ ra rằng với mọi$m,n \ge 0$, trình tự$\Gamma_m \star \Gamma_n$thỏa mãn đệ quy xác định tương tự như$\Gamma_{m+n}$. 

Vụ án$m=0$sản lượng$\Gamma_0 \star \Gamma_n = (\epsilon) \star \Gamma_n = \Gamma_n$, phù hợp$\Gamma_{0+n}$. Cho rằng$\Gamma_m \star \Gamma_n = \Gamma_{m+n}$giữ cố định$m$. Sử dụng phép đệ quy xác định của mã Gray,$\Gamma_{m+1} = (0,1) \star \Gamma_m$. Vì thế$$\Gamma_{m+1} \star \Gamma_n
= ((0,1) \star \Gamma_m) \star \Gamma_n.$$Nếu hoạt động$\star$là kết hợp, điều này bằng$$(0,1) \star (\Gamma_m \star \Gamma_n)
= (0,1) \star \Gamma_{m+n}
= \Gamma_{m+n+1}.$$Do đó tính kết hợp bao hàm quy tắc thành phần$\Gamma_{m+n} = \Gamma_m \star \Gamma_n$. 

Vẫn còn phải xác minh tính kết hợp trực tiếp từ quy tắc sắp xếp xác định. Mỗi phần tử được tạo ra bởi$\Gamma \star \Gamma'$được xác định duy nhất bởi một cặp$(i,j)$và vị trí của nó trong dãy chỉ phụ thuộc vào việc$i$là chẵn hoặc lẻ. Viết$\Gamma \star \Gamma'$như một chuỗi được lập chỉ mục theo cặp$(i,j)$, thứ tự tương đối của các cặp là từ điển trong$i$, trong khi thứ tự trong mỗi cố định$i$đang tăng hoặc giảm trong$j$theo tính chẵn lẻ của$i$. Nối với$\Gamma''$áp dụng lại quy tắc tương tự, bây giờ cho tập hợp chỉ mục của các cặp$(i,j)$. 

TRONG$(\Gamma \star \Gamma') \star \Gamma''$, mỗi bộ ba$(i,j,k)$được đặt hàng đầu tiên theo cặp$(i,j)$theo quy tắc boustrophedon, và sau đó trong mỗi cặp như vậy bằng$k$tăng hoặc giảm tùy thuộc vào vị trí tương đương của$(i,j)$TRONG$\Gamma \star \Gamma'$. TRONG$\Gamma \star (\Gamma' \star \Gamma'')$, bộ ba giống nhau$(i,j,k)$được đặt hàng đầu tiên bởi$i$, sau đó bởi$(j,k)$bên trong mỗi khối, với sự đảo ngược chỉ được kiểm soát bởi tính chẵn lẻ của$i$Và$j$. 

Điểm cốt yếu là sự ngang bằng về vị trí của$(i,j)$TRONG$\Gamma \star \Gamma'$chỉ phụ thuộc vào$i$Và$j$thông qua một modulo quy tắc tuyến tính cố định$2$, không phụ thuộc vào cách chuỗi được đặt trong ngoặc đơn. Vì vậy, quy tắc xác định liệu$k$-Thứ tự tiến hay lùi chỉ phụ thuộc vào$(i,j)$và không có trong nhóm. Vì cả hai cách xây dựng đều tạo ra thứ tự giống nhau trên tất cả các bộ ba$(i,j,k)$, các chuỗi kết quả trùng nhau. 

Như vậy$$(\Gamma \star \Gamma') \star \Gamma'' = \Gamma \star (\Gamma' \star \Gamma'').$$Điều này hoàn thành việc chứng minh. ∎
