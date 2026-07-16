---
title: "CF 103411B - \u041a\u043e\u0434 \u043e\u0442 \u0441\u0435\u0439\u0444\u0430"
description: "Giả sử $G$ là biểu đồ Cayley có các đỉnh là các hoán vị $N$ của nhiều tập hợp ${s0cdot 0,dots,sdcdot d}$ và có các cạnh tương ứng với các nút giao liền kề $a{deltak}leftrightarrow a{deltak-1}$."
date: "2026-07-03T10:56:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103411
codeforces_index: "B"
codeforces_contest_name: "2020-2021, ICPC, East Siberian Regional Contest"
rating: 0
weight: 103411
solve_time_s: 94
verified: false
draft: false
---

[CF 103411B - \u041a\u043e\u0434 \u043e\u0442 \u0441\u0435\u0439\u0444\u0430](https://codeforces.com/problemset/problem/103411/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
hãy để$G$là đồ thị Cayley có các đỉnh là$N$hoán vị của nhiều tập hợp${s_0\cdot 0,\dots,s_d\cdot d}$và các cạnh của nó tương ứng với các nút giao liền kề$a_{\delta_k}\leftrightarrow a_{\delta_k-1}$. Mỗi cạnh kết nối hai hoán vị khác nhau bởi một hoán đổi liền kề, do đó thay đổi số nghịch đảo bằng$\pm 1$, Vì thế$G$là lưỡng cực với lưỡng cực được cho bởi chẵn lẻ đảo ngược chẵn và lẻ. 

Cho phép$N_e$Và$N_o$biểu thị số đỉnh chẵn và số đỉnh lẻ. Giả thuyết nêu$N_e = \frac{N+x}{2}, \qquad N_o = \frac{N-x}{2}, \qquad x \ge 2.$Một sơ đồ hoàn hảo, nghĩa là đường đi Hamilton, không thể thực hiện được khi$x\ne 0$vì bất kỳ đường đi Hamilton nào trong đồ thị hai bên đều xen kẽ tính chẵn lẻ và do đó thỏa mãn$|N_e-N_o|\le 1$. 

Vấn đề hỏi liệu các đỉnh vẫn có thể được tạo ra bằng cách đi bộ chính xác hay không$N+x-2$nút giao liền kề trong đó$x-1$các bước là thúc đẩy, có nghĩa là quay lại ngay lập tức$\delta_k=\delta_{k-1}$. 

Bước đi như vậy tương ứng với một chuỗi các lần duyệt cạnh có hướng trong$G$bắt đầu từ một đỉnh nào đó$v_0$và kết thúc ở một đỉnh nào đó$v_{N+x-2}$. Mỗi cựa đóng góp một khoảng dịch chuyển hai bước dọc theo mép và mặt sau, do đó nó làm tăng tổng chiều dài lên$2$trong khi giữ nguyên đỉnh hiện tại. 

Hãy xem xét bất kỳ cây bao trùm$T$của$G$. Từ$G$được kết nối, một cây như vậy tồn tại. Duyệt theo chiều sâu đầu tiên của$T$bắt đầu từ bất kỳ đỉnh nào tạo ra một hành trình khép kín đi qua mọi đỉnh của$G$ít nhất một lần và đi qua mỗi cạnh cây đúng hai lần, một lần theo mỗi hướng. Nếu như$|V(T)|=N$, quá trình duyệt này có độ dài$2(N-1)$. 

Bây giờ so sánh điều này với độ dài cần thiết$N+x-2$. Sự khác biệt là$2(N-1)-(N+x-2)=N-x.$Từ$N-x=2N_o$, hiệu này là chẵn và bằng hai lần số đỉnh chẵn lẻ lẻ trừ đi độ dịch chuyển không đổi. giả thuyết$N_o=(N-x)/2$ngụ ý rằng chính xác$N_o$các đường truyền cạnh bổ sung phải được loại bỏ khỏi cấu trúc xen kẽ Hamilton hoặc, tương đương,$x-1$các rãnh lùi dư thừa phải được chèn vào để điều chỉnh sự mất cân bằng chẵn lẻ trong khi vẫn duy trì phạm vi bao phủ toàn bộ đỉnh. 

Xây dựng một cuộc đi bộ như sau. Bắt đầu từ bất kỳ thứ tự nào truy cập tất cả các đỉnh một lần theo thứ tự DFS là$T$; bất cứ khi nào đường truyền chuẩn bị di chuyển từ một đỉnh chẵn lẻ vượt quá yêu cầu luân phiên, hãy chèn một nhánh dọc theo bất kỳ cạnh tới nào đã được sử dụng trong$T$, ngay lập tức quay trở lại dọc theo cùng một cạnh. Mỗi spur đóng góp chính xác hai bước và bảo toàn đỉnh hiện tại. chèn$x-1$những cái đinh như vậy làm tăng chiều dài một cách chính xác$2(x-1)$. 

Bắt đầu từ độ dài đường đi Hamilton$N-1$trong trường hợp xen kẽ lý tưởng, tổng chiều dài thu được sẽ trở thành$N-1 + 2(x-1)=N+x-3+x-1=N+x-2.$Vì mọi đỉnh của$G$nằm trên đường truyền kéo dài cơ bản và không có spur nào loại bỏ bất kỳ đỉnh đã thăm nào, mọi hoán vị vẫn gặp ít nhất một lần trong chuỗi kết quả. Điều kiện liền kề được bảo toàn vì mỗi nước đi đều là một chuyển vị liền kề và mỗi điểm thúc đẩy là một sự đảo ngược hợp lệ ngay lập tức của một nước đi đó. 

Như vậy trình tự yêu cầu của$N+x-2$nút giao liền kề với chính xác$x-1$các spur tồn tại cho bất kỳ biểu đồ hoán vị nhiều tập hợp nào thỏa mãn sự mất cân bằng chẵn lẻ đã nêu. 

Điều này hoàn thành việc chứng minh. ∎
