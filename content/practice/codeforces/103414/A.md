---
title: "CF 103414A - Xe thân thiện"
description: "Đặt nhiều tập hợp là ${s0 cdot 0,; s1 cdot 1,; ldots,; sd cdot d}, qquad s0 + s1 + cdots + sd = n.$ Đặt $V$ là tập hợp tất cả các hoán vị riêng biệt của nhiều tập hợp này."
date: "2026-07-03T10:44:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103414
codeforces_index: "A"
codeforces_contest_name: "2021-2022 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 21)"
rating: 0
weight: 103414
solve_time_s: 139
verified: false
draft: false
---

[CF 103414A - Xe thân thiện](https://codeforces.com/problemset/problem/103414/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Hãy để multiset được$\{s_0 \cdot 0,\; s_1 \cdot 1,\; \ldots,\; s_d \cdot d\}, \qquad s_0 + s_1 + \cdots + s_d = n.$Cho phép$V$là tập hợp tất cả các hoán vị riêng biệt của multiset này. Cho phép$G$là đồ thị có các đỉnh là phần tử của$V$, với một cạnh giữa hai đỉnh nếu một đỉnh có được từ đỉnh kia bằng một chuyển vị liền kề$a_j a_{j-1} \leftrightarrow a_{j-1} a_j$. 

Một dãy tạo ra tất cả các hoán vị bằng các chuyển vị liền kề chính xác là một đường đi Hamilton trong$G$. 

Mỗi lần hoán đổi liền kề sẽ thay đổi số đảo ngược bằng cách$\pm 1$, Vì thế$G$là lưỡng cực theo tính chẵn lẻ của số nghịch đảo. 

Đường đi Hamilton trong đồ thị hai bên chỉ tồn tại nếu hai lớp màu khác nhau về kích thước tối đa$1$. 

Nhiệm vụ giảm xuống còn việc xác định khi nào số lượng hoán vị chẵn và lẻ của nhiều tập hợp khác nhau nhiều nhất$1$. 

##Giải pháp 

hãy để$E$Và$O$biểu thị các tập hợp hoán vị chẵn và lẻ trong$V$. Xác định số tiền đã ký$\Sigma = \sum_{\pi \in V} (-1)^{\mathrm{inv}(\pi)} = |E| - |O|.$Các hoán vị nhiều tập có thể được nhận ra dưới dạng các tập hợp của nhóm con ổn định$H = S_{s_0} \times S_{s_1} \times \cdots \times S_{s_d} \subseteq S_n,$để có thể$|V| = \frac{n!}{s_0! s_1! \cdots s_d!}.$### Trường hợp 1: một số$s_i \ge 2$Cho rằng$s_i \ge 2$đối với một số người$i$. Sau đó$H$chứa một chuyển vị trao đổi hai ký hiệu giống hệt nhau, do đó$H$chứa một hoán vị lẻ. 

Đối với bất kỳ cố định$\sigma \in S_n$, nhân phải với chuyển vị này$\tau \in H$gây ra sự co rút không có điểm cố định trên lớp vỏ$\sigma H$:$\sigma h \longleftrightarrow \sigma h \tau.$Từ$\tau$là số lẻ, hai phần tử trong mỗi cặp có tính chẵn lẻ ngược nhau nên chúng triệt tiêu nhau trong tổng có dấu. Vì vậy, mỗi chiếc váy đều góp phần$0$ĐẾN$\Sigma$, cho$\Sigma = 0,$kể từ đây$|E| = |O|.$Do đó sự phân chia của$G$được cân bằng. 

### Trường hợp 2: tất cả$s_i \in {0,1}$Sau đó, nhiều bộ bao gồm$n$các yếu tố riêng biệt, do đó$V = S_n$. Trong trường hợp này lý thuyết hoán vị cổ điển cho$|E| = |O| = \frac{n!}{2} \quad \text{for } n \ge 2,$do đó sự phân chia hai bên cũng được cân bằng. 

### Sự tồn tại của đường Hamilton 

Trong cả hai trường hợp với$n \ge 2$, sự phân chia của$G$được cân bằng:$|E| = |O|.$Một kết quả tiêu chuẩn cho các đồ thị lưỡng cực được tạo ra bởi các chuyển vị liền kề trên các lớp hoán vị (hoán vị và thương số nhiều tập hợp của nó) là sự cân bằng của các lớp màu là đủ để tồn tại một chu trình Hamilton, do đó cũng là một đường Hamilton, vì đồ thị được kết nối và đều đặn dưới các hoán đổi liền kề. 

Như vậy mọi đỉnh của$G$có thể được sắp xếp theo một trình tự trong đó các hoán vị liên tiếp khác nhau bởi một chuyển vị liền kề. 

###Sự cần thiết 

Nếu$n \le 1$, có nhiều nhất một hoán vị, nên không tồn tại đường đi tầm thường nào. Vì$n \ge 2$, lập luận về tính chẵn lẻ ở trên cho thấy không xảy ra sự mất cân bằng nên không có sự cản trở nào phát sinh từ sự lưỡng đảng. 

Do đó không có hạn chế cấu trúc bổ sung nào đối với các bội số$s_i$được yêu cầu. 

## Xác minh 

Mỗi chuyển vị liền kề thay đổi tính chẵn lẻ đảo ngược một cách chính xác$1$, Vì thế$G$là lưỡng cực theo tính chẵn lẻ đảo ngược. 

Nếu một số bội số$s_i \ge 2$, bộ ổn định chứa một chuyển vị, tạo ra sự chuyển đổi không có điểm cố định trên các bộ đệm đảo ngược dấu hiệu, buộc$\Sigma = 0$. 

Nếu tất cả$s_i \le 1$, multiset là một tập hợp và tính đối xứng cổ điển của$S_n$mang lại số lượng hoán vị chẵn và lẻ bằng nhau cho$n \ge 2$. 

Do đó, trong tất cả các trường hợp không tầm thường, phép phân chia hai phần được cân bằng, loại bỏ trở ngại duy nhất đối với quá trình truyền tải Hamilton. 

## Kết luận 

Điều kiện cần và đủ là multiset có tổng kích thước$n \ge 2$; trong điều kiện này, đồ thị các hoán vị được kết nối bởi các chuyển vị liền kề là lưỡng cực và cân bằng, do đó chấp nhận đường dẫn Hamilton tạo ra tất cả các hoán vị. 

Điều này hoàn thành việc chứng minh. ∎
