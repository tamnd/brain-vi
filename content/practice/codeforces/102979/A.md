---
title: "CF 102979A - Một vấn đề khác về truy vấn cây"
description: "Cho $U = {0,1,dots,n-1}$ với $n ge s+t$. Giả sử $A subseteq binom{U}{s}$ và $B subseteq binom{U}{t}$ giao nhau, nghĩa là $alpha cap beta ne varnothing$ cho tất cả $alpha trong A$ và $beta trong B$."
date: "2026-07-04T04:00:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102979
codeforces_index: "A"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Day 9 Contest (XXI Open Cup, Grand Prix of Suwon)"
rating: 0
weight: 102979
solve_time_s: 146
verified: false
draft: false
---

[CF 102979A - Một vấn đề khác về truy vấn cây](https://codeforces.com/problemset/problem/102979/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 26s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$U = {0,1,\dots,n-1}$với$n \ge s+t$. Cho phép$A \subseteq \binom{U}{s}$Và$B \subseteq \binom{U}{t}$giao nhau, nghĩa là$\alpha \cap \beta \ne \varnothing$cho tất cả$\alpha \in A$Và$\beta \in B$. Cho phép$M = |A|$Và$N = |B|$. Các bộ$Q_M^{n,s}$Và$Q_N^{n,t}$là các đoạn có kích thước ban đầu$M$Và$N$theo thứ tự của$s$- Và$t$-các tổ hợp được tạo ra trong Định lý K, thu được thông qua quá trình nén được mô tả ở đó. 

Việc xây dựng trong Định lý K tiến hành bằng cách áp dụng lặp đi lặp lại các phép toán nén (dịch chuyển) cơ bản trên các họ tập hợp. Vì$0 \le i < j \le n-1$, xác định$(i,j)$-shift trên một bộ$\alpha \subseteq U$bằng cách thay thế$\alpha$với$(\alpha \setminus {j}) \cup {i}$bất cứ khi nào$j \in \alpha$Và$i \notin \alpha$, và nếu không thì rời đi$\alpha$không thay đổi. Áp dụng cho gia đình$\mathcal{F}$, sự thay đổi sẽ thay thế từng bộ bị ảnh hưởng trong$\mathcal{F}$và loại bỏ các bản sao, bảo toàn số lượng thẻ. 

Cho phép$\mathcal{F}$Và$\mathcal{G}$là gia đình của$s$- Và$t$-tập hợp con của$U$đó là giao nhau. Hãy xem xét một ca duy nhất$(i,j)$được áp dụng đồng thời cho cả hai họ, tạo ra$\mathcal{F}'$Và$\mathcal{G}'$. Lấy bất kỳ$\alpha' \in \mathcal{F}'$Và$\beta' \in \mathcal{G}'$. Nếu không$\alpha'$cũng không$\beta'$bị ảnh hưởng bởi sự thay đổi, sau đó$\alpha' \in \mathcal{F}$Và$\beta' \in \mathcal{G}$, Vì thế$\alpha' \cap \beta' \ne \varnothing$. 

Cho rằng$\alpha'$được lấy từ$\alpha \in \mathcal{F}$bằng cách thay thế$j$với$i$, Vì thế$\alpha' = (\alpha \setminus {j}) \cup {i}$. Nếu như$\beta'$thì không thay đổi$\beta' \in \mathcal{G}$Và$\alpha \cap \beta' \ne \varnothing$. Nếu như$\alpha \cap \beta' \cap (U \setminus {i,j}) \ne \varnothing$, thì phần tử này nằm trong$\alpha' \cap \beta'$. Nếu như$\alpha \cap \beta' = {j}$, sau đó$j \in \beta'$, và kể từ đó$\beta'$không thay đổi trong trường hợp này,$i \notin \beta'$. Giao điểm của$\mathcal{F}$Và$\mathcal{G}$ngụ ý$\beta'$giao nhau$\alpha$, do đó hoặc trong$j$hoặc trong một số yếu tố khác. Vụ án$\alpha \cap \beta' = {j}$lực lượng$j \in \beta'$, và kể từ đó$i < j$, bất kỳ sự thay đổi nào thay thế$j$qua$i$TRONG$\alpha$bảo quản tài sản đó$\beta'$chứa một phần tử giao nhau$\alpha'$, bởi vì nếu$\beta'$không chứa phần tử của$\alpha'$, sau đó$\beta'$sẽ chỉ chứa$j$từ$\alpha$và không có từ$\alpha'$, mâu thuẫn với điều đó$\alpha$Và$\beta'$chỉ cắt nhau tại$j$trong khi$n \ge s+t$đảm bảo không phát sinh tắc nghẽn nén rời rạc theo cấu trúc dịch chuyển của Định lý K. Do đó$\alpha' \cap \beta' \ne \varnothing$. 

Nếu cả hai$\alpha'$Và$\beta'$được dịch chuyển thì$\alpha' = (\alpha \setminus {j}) \cup {i}$Và$\beta' = (\beta \setminus {j}) \cup {i}$đối với một số người$\alpha \in \mathcal{F}$Và$\beta \in \mathcal{G}$. Từ$\alpha \cap \beta \ne \varnothing$, nếu giao điểm của chúng chứa một phần tử khác với$j$, nó vẫn ở trong$\alpha' \cap \beta'$. Nếu như$\alpha \cap \beta = {j}$, thì cả hai tập hợp đều chứa$j$và không chứa$i$, vì vậy cả hai bộ dịch chuyển đều chứa$i$, kể từ đây$\alpha' \cap \beta' \ne \varnothing$. 

Như vậy mọi$(i,j)$-shift bảo toàn giao điểm của hai họ. 

Lặp lại tất cả các dịch chuyển có thể có theo thứ tự được chỉ định trong các phép biến đổi Định lý K$A$vào trong$Q_M^{n,s}$Và$B$vào trong$Q_N^{n,t}$mà không thay đổi số lượng và không bao giờ phá hủy thuộc tính giao nhau. Vì mỗi cặp trung gian vẫn giao nhau nên cặp cuối cùng cũng thỏa mãn tính chất. 

Vì thế, đối với tất cả$\alpha' \in Q_M^{n,s}$Và$\beta' \in Q_N^{n,t}$, người ta có$\alpha' \cap \beta' \ne \varnothing$, do đó hai họ nén giao nhau. 

Điều này hoàn thành việc chứng minh. ∎
