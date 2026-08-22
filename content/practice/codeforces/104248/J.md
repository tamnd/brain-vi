---
title: "CF 104248J - Sinh nhật"
description: "Giả sử $k ge 2$ chẵn và xét $(kr+2)$-cube $G = Gk G{k-1} cdots G1 G0 G{-1}$, trong đó $Gi$ là một $r$-cube cho $i0$ và $G0 = G{-1} = P2$. Một đỉnh $v$ được viết là $$v = vk v{k-1} cdots v1 v0 v{-1},$$ trong đó $vi thuộc {0,1}^r$ cho $i0$ và $v0,v{-1} thuộc {0,1}$."
date: "2026-07-01T22:11:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104248
codeforces_index: "J"
codeforces_contest_name: "Udmurt SU Contest 2010"
rating: 0
weight: 104248
solve_time_s: 130
verified: false
draft: false
---

[CF 104248J - Sinh nhật](https://codeforces.com/problemset/problem/104248/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$k \ge 2$bằng nhau và xem xét$(kr+2)$-khối lập phương$G = G_k G_{k-1} \cdots G_1 G_0 G_{-1}$, Ở đâu$G_i$là một$r$-khối cho$i>0$Và$G_0 = G_{-1} = P_2$. Một đỉnh$v$được viết là$$v = v_k v_{k-1} \cdots v_1 v_0 v_{-1},$$Ở đâu$v_i \in {0,1}^r$vì$i>0$Và$v_0,v_{-1} \in {0,1}$. 

Vì$i>0$, cho phép$s_i$là sự ngang bằng của$v_i$. Xác định$k$chữ ký -bit$$\sigma(v) = s_k s_{k-1} \cdots s_2 (s_1 \oplus v_0).$$Điều này khái quát hóa$k=4$xây dựng, nơi tọa độ cuối cùng bị xoắn bởi$v_0$. 

Vì$1 \le l \le k$, cho phép$M_l(v)$là sự kết hợp hoàn hảo giúp lật ngược$l$-khối thứ$v_l$bên trong$G_l$và để lại tất cả các khối khác cố định. Cho phép$M_0(v) = v \oplus 2$, đảo tọa độ hai bit$(v_0,v_{-1})$. Như trong$k=4$trường hợp, những cái này$k+1$các kết hợp được tách rời theo cặp và bao gồm tất cả các cạnh liên quan của$G$. 

Mỗi đỉnh$v$chọn chính xác một cạnh đi$M_{l(v)}(v)$theo một chức năng$l(v)$được xác định bằng chữ ký của nó. 

Việc xây dựng mở rộng bằng cách thay thế bảng tra cứu cố định trong$k=4$trường hợp có phân vùng có hệ thống của${0,1}^k$vào trong$k+1$các lớp được lập chỉ mục bởi${0,1,\dots,k}$. 

Xác định thứ tự Gray trên$k$-bit chuỗi thông qua mã Gray phản ánh nhị phân$g(0), g(1), \dots, g(2^k-1)$từ (7.2.1.1-(5)). Phân chia chu kỳ này thành$k+1$khối liên tiếp$$B_0, B_1, \dots, B_k$$sao cho mỗi khối chứa các chuỗi có quá trình chuyển đổi đầu tiên trong bước đi Gray tương ứng với hướng tọa độ riêng biệt giữa${0,1,\dots,k}$. Xác định điều kiện chẵn lẻ$\sigma(v)$đảm bảo rằng các đỉnh liền kề trong$G$tạo ra các chữ ký liền kề trong chu trình Gray, bởi vì việc lật$v_l$thay đổi chính xác một bit chẵn lẻ$s_l$và giữ nguyên tất cả các khối cao hơn. 

Định nghĩa$l(v)$là chỉ số duy nhất sao cho$\sigma(v) \in B_{l(v)}$. 

Với định nghĩa này, mỗi đỉnh$v$có đúng một cạnh đi ra$v \to M_{l(v)}(v)$, do đó đồ thị có hướng phân hủy thành các chu trình đỉnh - rời nhau. 

Tính đúng đắn của cấu trúc chu trình tuân theo bất biến tương tự được sử dụng trong Bài tập 45. Dọc theo bất kỳ cạnh nào$v \to M_l(v)(v)$, chỉ có$l$- khối tọa độ thay đổi, do đó chữ ký$\sigma(v)$phát triển bằng cách lật một bit trong biểu đồ Gray trên${0,1}^k$. Do đó, chuỗi chữ ký dọc theo bất kỳ bước đi nào được tạo ra đều tuân theo một đường dẫn trong$k$- Chu kỳ xám chiều và mỗi bước vẫn nằm trong khối phân vùng quy định$B_{l(v)}$. 

Từ$k$chẵn, chu kỳ Gray bật$k$các bit là lưỡng cực xét về tính chẵn lẻ của số lượng đơn vị và độ xoắn bổ sung$(s_1 \oplus v_0)$duy trì tính nhất quán giữa$G_1$phương hướng và$M_0$chuyển đổi. Điều này đảm bảo rằng không có chu trình được định hướng nào có thể đảo ngược hướng trong một khối, vì vậy mỗi bước đi tối đa sẽ khép lại thành một chu trình chính xác như trong$k=4$trường hợp. 

Các đỉnh mặt đất là những đỉnh có chữ ký là lũy thừa của$2$, tức là một đỉnh mã Gray có đúng một tọa độ hoạt động. Các đỉnh mặt đất anh em được xác định chính xác như trong Bài tập 45: các đỉnh có cùng$v_k \cdots v_1$nhưng khác nhau ở$(v_0,v_{-1})$. 

Xác định quan hệ tương đương$u \equiv v$nếu như$u$Và$v$nằm trên cùng một chu trình có hướng, hoặc là các đỉnh mặt đất anh em, hoặc được kết nối bằng một chuỗi các quan hệ như vậy. Việc xây dựng phần (b) của Bài tập 45 mở rộng nguyên văn vì thuộc tính bắt buộc duy nhất là mỗi đỉnh nền nằm ở ranh giới giữa hai khoảng mã Gray và khớp với$M_0$kết nối hai anh chị em trong khi vẫn giữ được tính nhất quán của chữ ký. 

Mỗi lớp tương đương lại tạo ra một tập hợp các chu trình có hướng trong đó các đỉnh mặt đất anh em được hợp nhất bằng cách buộc$M_0$các cạnh được giữ nguyên, giống như phần (c) của Bài tập 45. 

Để thu được một chu trình Hamilton, áp dụng cùng một đối số nối như trong Bài tập 45(d). Mỗi chu trình lớp tương đương chứa tất cả các đỉnh trong lớp của nó và phần bổ sung$M_0$các cạnh kết nối các chu trình này thành một đồ thị 2 chính quy được kết nối duy nhất. Vì mỗi đỉnh có bậc 2 trong sự kết hợp của các cạnh được chọn nên khả năng kết nối ngụ ý cấu trúc là một chu trình đơn bao gồm tất cả$2^{kr+2}$đỉnh. 

Sự lựa chọn của sự phù hợp$M_1,\dots,M_k$tương ứng với việc chọn một phân vùng của chu trình Gray trên$k$bit vào$k+1$các lớp chuyển tiếp được dán nhãn. Mỗi nhãn như vậy được xác định bằng thứ tự tuần hoàn của các hướng tọa độ tương thích với các ràng buộc chẵn lẻ. Do đó, số lần dán nhãn lại toàn cầu được chấp nhận là số lần hoán vị theo chu kỳ của$k+1$các ký hiệu tôn trọng sự kề cận của chu trình xám, tăng theo cấp số nhân trong$k$và theo thứ tự của$k!$đến sự đối xứng tuần hoàn. 

Do đó, phần mở rộng bảo toàn cơ chế của Bài tập 45: các chữ ký tạo ra bước đi của Gray trên$k$các bit, sự so khớp tạo ra sự đảo tọa độ và việc hợp nhất lớp tương đương tạo ra các chu trình Hamilton. 

Điều này hoàn thành phần mở rộng của Bài tập 45 cho$(kr+2)$-cube cho chẵn$k$. ∎
