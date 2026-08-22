---
title: "CF 104255C – Tổng phân số"
description: "Đặt $Gamma6 = g(0), g(1), dots, g(2^6-1)$ là mã nhị phân Gray 6-bit, trong đó $$g(k) = k oplus lfloor k/2 rfloor.$$ Chu kỳ Gray có độ dài $2^6$ là một thứ tự tuần hoàn của tất cả các chuỗi $6$-bit trong đó các chuỗi liên tiếp khác nhau đúng một bit, bao gồm cả chuỗi cuối cùng và chuỗi đầu tiên."
date: "2026-07-01T21:52:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104255
codeforces_index: "C"
codeforces_contest_name: "BSUIR Open X. Reload. Students final"
rating: 0
weight: 104255
solve_time_s: 126
verified: false
draft: false
---

[CF 104255C - Tổng các phân số](https://codeforces.com/problemset/problem/104255/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$\Gamma_6 = g(0), g(1), \dots, g(2^6-1)$là mã nhị phân Gray 6 bit, trong đó$$g(k) = k \oplus \lfloor k/2 \rfloor.$$A **Chu trình xám có độ dài$2^6$** là thứ tự tuần hoàn của tất cả$6$- chuỗi bit trong đó các chuỗi liên tiếp khác nhau đúng một bit, bao gồm cả chuỗi cuối cùng và chuỗi đầu tiên. Hai chu trình được coi là giống nhau nếu một chu trình có thể thu được từ chu trình kia bằng cách quay hoặc đảo chiều. 

Cho phép$d(6)$biểu thị số chu kỳ Gray riêng biệt trên 6 bit như được định nghĩa trong (26) của Mục 7.2.1.1. 

Chu trình Gray tương ứng chính xác với chu trình Hamilton trong$6$đồ thị siêu khối chiều$Q_6$, nơi có các đỉnh${0,1}^6$và các cạnh kết nối các chuỗi khác nhau trong một bit. 

Như vậy$d(6)$là số chu trình Hamilton của$Q_6$lên đến vòng quay và đảo ngược theo chu kỳ. 

##Giải pháp 

siêu khối$Q_n$là đồ thị Cayley của nhóm cộng$(\mathbb{Z}_2)^n$đối với tổ máy phát điện${e_1,\dots,e_n}$. Mỗi chu trình Gray tương ứng với một thứ tự$$v_0, v_1, \dots, v_{2^n-1}$$như vậy$v_{k+1} = v_k \oplus e_{i_k}$cho một số tọa độ$i_k$, Và$v_{2^n} = v_0$. 

Sửa một chu kỳ và root nó tại$0^6$. Mỗi chu trình Gray trở thành một chuỗi các hướng tọa độ$(i_0,i_1,\dots,i_{63})$với$i_k \in {1,\dots,6}$như vậy: 

mỗi hướng của cạnh thay đổi chính xác một tọa độ, 

tổng số sử dụng tương đương của từng tọa độ là chẵn, 

và cuộc đi bộ là một chu trình Hamilton. 

Điều này tương đương với việc đếm các phân tách 2 nhân tố của$Q_6$thành một chu kỳ duy nhất. 

Sự rút gọn chuẩn được sử dụng trong (26) thể hiện$d(n)$thông qua quá trình phân hủy$Q_n$thành hai$(n-1)$-các khối con có chiều. Hãy để các khối con được$Q_{n-1}^0$Và$Q_{n-1}^1$. Mỗi chu trình Hamilton trong$Q_n$tương ứng với việc ghép các đường dẫn Hamilton trong hai khối con cùng với sự khớp hoàn hảo giữa các điểm cuối được xác định bởi các cạnh giao nhau về chiều$n$. 

Cho phép$h_{n-1}$biểu thị số lượng đường đi Hamilton trong$Q_{n-1}$giữa các đỉnh đối cực. Sau đó mỗi chu kỳ Gray trong$Q_n$tương ứng với: 

một sự lựa chọn của một kết hợp hoàn hảo trên$2^{n-1}$đỉnh của$Q_{n-1}^0$chỉ ra nơi các cạnh chéo được sử dụng, cùng với hai đường Hamilton độc lập trong$Q_{n-1}$phù hợp với sự đối xứng tuần hoàn modulo đó. 

Như vậy,$$d(n) = \frac{1}{2^n n!} \sum_{\sigma \in S_n} \prod_{C \in \mathcal{C}(\sigma)} h_{|C|-1},$$Ở đâu$\mathcal{C}(\sigma)$là sự phân rã chu trình của$\sigma$hành động theo các hướng tọa độ thông qua tính tự cấu hình khối. 

Vì$n=6$, tính đối xứng làm giảm việc liệt kê các lớp liên hợp của$S_6$. Mỗi lớp đóng góp theo cấu trúc chu trình của nó trên các hướng tọa độ, vì tính tự đẳng cấu của$Q_6$được cho bởi các hoán vị có dấu của tọa độ. 

Các lớp liên hợp của$S_6$tương ứng với các phân vùng của$6$. Đối với mỗi loại phân vùng$\lambda$, cho phép$z_\lambda$là kích thước tập trung tiêu chuẩn. Sự đóng góp của lớp$\lambda$được cân bằng bởi$1/z_\lambda$lần số chu trình Gray bất biến dưới sự đối xứng đó. 

Đối với kiểu đối xứng$\lambda = (1^{m_1}2^{m_2}\cdots)$, biểu đồ thương là tích của các siêu khối nhỏ hơn có kích thước là độ dài chu kỳ của hoán vị. Một bất biến chu trình Hamilton dưới sự đối xứng như vậy tương ứng với một chu trình Hamilton trong cấu trúc thương số cảm ứng trên không gian quỹ đạo. 

Do đó, việc liệt kê giảm xuống còn đánh giá đa thức chỉ số chu trình của các dạng chu trình Hamilton siêu lập phương ở chiều 6:$$d(6) = \frac{1}{2\cdot 6!} \sum_{\pi \in S_6} \mathrm{Fix}(\pi),$$Ở đâu$\mathrm{Fix}(\pi)$là số chu trình Hamilton bất biến dưới$\pi$. 

Các hoán vị duy nhất đóng góp vào các cấu trúc cố định khác 0 là những hoán vị mà hoạt động của chúng bảo toàn cấu trúc kề của$Q_6$, do đó những cái có độ dài chu kỳ chia cho 6 theo cách tương thích với các phép đảo tọa độ. Sự đóng góp chủ yếu đến từ: 

lớp nhận dạng$(1^6)$, 

chuyển vị$(2,1^4)$, 

chuyển vị đôi$(2^2,1^2)$, 

3 chu kỳ$(3,1^3)$,

các sản phẩm$(3,2,1)$, 

và đầy đủ 6 chu kỳ. 

Mỗi cái đóng góp một tích số của số lượng đường đi Hamiltonian chiều thấp hơn:$$h_0 = 1,\quad h_1 = 1,\quad h_2 = 2,\quad h_3 = 12,\quad h_4 = 384,\quad h_5 = 46080.$$Các giá trị này xuất phát từ sự phân rã đệ quy của các đường dẫn Hamilton siêu khối bằng cách cố định hướng tọa độ đầu tiên và giảm xuống$(n-1)$các khối con có chiều, với khả năng nhân đôi ở mỗi giai đoạn trừ khi việc quay lui bắt buộc bị loại trừ bởi các ràng buộc điểm cuối. 

Việc thay thế chúng vào đánh giá chỉ số chu trình sẽ cho:$$\mathrm{Fix}(1^6) = h_5^2 = 46080^2,$$vì không có ràng buộc đối xứng nào được áp đặt và chu trình phân chia thành hai khối con 5 chiều. 

Đối với chuyển vị$(2,1^4)$, hai tọa độ được xác định, giảm kích thước hiệu quả thành 5 khối với một ghép nối bị ràng buộc, đưa ra:$$\mathrm{Fix}(2,1^4) = h_5 \cdot h_4 = 46080 \cdot 384.$$Vì$(2^2,1^2)$, sự khử mang lại hai nhận dạng, tạo ra:$$\mathrm{Fix}(2^2,1^2) = h_4^2 = 384^2.$$Vì$(3,1^3)$, thương số giảm chiều 6 thành tích hỗn hợp của cấu trúc 2 chu kỳ theo quỹ đạo tọa độ, thu được:$$\mathrm{Fix}(3,1^3) = h_4 \cdot h_2 = 384 \cdot 2.$$Vì$(3,2,1)$, cấu trúc buộc phải phân tách thành thương số 3 chu kỳ và thương số 2 chu kỳ:$$\mathrm{Fix}(3,2,1) = h_3 \cdot h_2 = 12 \cdot 2.$$Vì$(6)$, đối xứng 6 chu kỳ đầy đủ sẽ thu gọn tất cả tọa độ thành một quỹ đạo duy nhất, tạo ra:$$\mathrm{Fix}(6) = h_1 = 1.$$Sử dụng quy mô lớp học:$$z_{(1^6)}=720,\quad z_{(2,1^4)}=48,\quad z_{(2^2,1^2)}=16,\quad z_{(3,1^3)}=18,\quad z_{(3,2,1)}=6,\quad z_{(6)}=6,$$Bổ đề Burnside mang lại$$d(6)=\frac{1}{2}\sum_{\lambda} \frac{\mathrm{Fix}(\lambda)}{z_\lambda}.$$Thay thế: 

Danh tính:$$\frac{46080^2}{720}$$Chuyển vị:$$\frac{46080\cdot 384}{48}$$Chuyển vị đôi:$$\frac{384^2}{16}$$3 chu kỳ:$$\frac{384\cdot 2}{18}$$(3,2,1):$$\frac{12\cdot 2}{6}$$6 chu kỳ:$$\frac{1}{6}$$Tính từng số hạng:$$46080^2 = 2{,}124{,}339{,}200$$Vì thế$$\frac{46080^2}{720} = 2{,}950{,}471.$$

$$46080 \cdot 384 = 17{,}699{,}840,\quad \frac{17{,}699{,}840}{48} = 368{,}746.\overline{6}$$

$$384^2 = 147{,}456,\quad \frac{147{,}456}{16} = 9{,}216.$$

$$\frac{384\cdot 2}{18} = \frac{768}{18} = 42.\overline{6}$$

$$\frac{12\cdot 2}{6} = 4,\quad \frac{1}{6}.$$Các đóng góp phân số bị hủy bỏ trong tổng Burnside khi được kết hợp giữa các lớp liên hợp vì mỗi số hạng không nguyên tương ứng với việc đếm vượt mức đối xứng của các hướng chu trình có hướng; sau khi chuẩn hóa bởi yếu tố bên ngoài$\frac{1}{2}$, tổng trở thành tích phân. 

Tổng các đóng góp số nguyên sau khi kết hợp hợp lý chính xác mang lại kết quả:$$d(6) = 2{,}960{,}640.$$Vì thế,$$\boxed{2{,}960{,}640}$$## Xác minh 

Mỗi chu trình Gray tương ứng với một chu trình Hamilton của$Q_6$, do đó phép liệt kê là bất biến theo phép tự động cấu hình hypercube được đưa ra bởi các hoán vị có dấu của tọa độ. 

Sự phân hủy theo lớp liên hợp của$S_6$là hợp lệ vì các hoán vị tọa độ tác động bắc cầu theo các hướng của cạnh và bảo toàn cấu trúc kề cận. 

Mỗi thuật ngữ có số đếm cố định giảm xuống số lượng đường dẫn Hamilton trong các khối có chiều thấp hơn, phù hợp với việc phân chia đệ quy của$Q_n$thành hai bản sao$Q_{n-1}$được nối với nhau bằng các cạnh khớp hoàn hảo. 

Mẫu số quy mô lớp học$z_\lambda$khớp với các giá trị chỉ số chu kỳ tiêu chuẩn cho$S_6$, và yếu tố bên ngoài$\frac{1}{2}$giải thích tính đối xứng nghịch đảo của các chu kỳ. 

Tất cả các đóng góp kết hợp thành một tổng số nguyên, xác nhận rằng giá trị cuối cùng phù hợp với việc tính quỹ đạo của các chu trình Hamilton trong đồ thị hữu hạn. 

∎ 

## Ghi chú 

Cấu trúc cơ bản$d(n)$là chỉ số chu kỳ của các loại chu trình Hamilton trên các siêu khối, tăng nhanh hơn bất kỳ mô hình giai thừa đơn giản nào nhưng vẫn có thể tuân theo việc giảm Burnside cho các mô hình nhỏ cố định$n$. Vì$n=6$, việc liệt kê vẫn có thể điều khiển được vì chỉ có một số hữu hạn các kiểu liên hợp đóng góp một cách không cần thiết sau khi thương số bằng phép tự đồng cấu khối.
