---
title: "CF 103373C - Bài toán sắp xếp"
description: "Giả sử $C(n,t,m)$ biểu thị đồ thị có các đỉnh đều là $t$-tổ hợp $ctldots c1$ với $$nctcdotsc1ge 0,qquad ct-c1<m,$$ và trong đó hai đỉnh liền kề nhau khi chúng khác nhau trong đúng một mục nhập, nghĩa là, một thay thế $cj leftarrow cj'$ duy trì mức tăng nghiêm ngặt và…"
date: "2026-07-03T12:38:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103373
codeforces_index: "C"
codeforces_contest_name: "2021 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103373
solve_time_s: 154
verified: false
draft: false
---

[CF 103373C - Vấn đề sắp xếp](https://codeforces.com/problemset/problem/103373/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$C(n,t,m)$biểu thị đồ thị có các đỉnh đều$t$-sự kết hợp$c_t\ldots c_1$với$$n>c_t>\cdots>c_1\ge 0,\qquad c_t-c_1<m,$$và trong đó hai đỉnh kề nhau khi chúng khác nhau ở đúng một mục, tức là một thay thế$c_j \leftarrow c_j'$duy trì mức tăng nghiêm ngặt và ràng buộc về khoảng cách. 

Viết$n=s+t$chỉ để thống nhất với ký hiệu Mục 7.2.1.3; đây$t=4$được cố định và$s=n-4$không có vai trò cấu trúc nào ngoại trừ việc giới hạn bảng chữ cái. 

### Cải cách bằng vectơ khoảng cách 

Xác định các biến khoảng cách$$g_1=c_1,\qquad g_2=c_2-c_1,\qquad g_3=c_3-c_2,\qquad g_4=c_4-c_3,$$và sự chùng xuống còn lại$$g_5=(m-1)-(c_4-c_1).$$Những bất đẳng thức chặt chẽ ngụ ý$$g_1\ge 0,\qquad g_2,g_3,g_4\ge 1,$$và điều kiện nhịp tương đương với$$g_5\ge 1.$$Hơn thế nữa,$$g_1+g_2+g_3+g_4+g_5=m-1.$$Do đó mỗi hợp âm hợp lệ tương ứng một cách khách quan với một thành phần của$m-1$thành năm phần với giới hạn dưới$(0,1,1,1,1)$. 

Một sự thay thế duy nhất$c_j\leftarrow c_j\pm 1$thay đổi chính xác một trong những khoảng trống$g_i$qua$+1$và một cái khác bởi$-1$, bảo toàn mọi ràng buộc. Do đó sự kề cận trong$C(n,4,m)$chính xác là sự kề cận trong biểu đồ thành phần bị ràng buộc này. 

### Tính liên thông và rút gọn về đồ thị đơn hình cố định 

hãy để$G(m)$là đồ thị của tất cả các vectơ nguyên$$(g_1,g_2,g_3,g_4,g_5)$$thỏa mãn các ràng buộc trên và tính tổng$m-1$, với sự kề cận được cho bằng cách chuyển$1$giữa các tọa độ trong khi vẫn giữ nguyên giới hạn. 

đồ thị$G(m)$là đồ thị con cảm ứng của đồ thị thành phần chuẩn trên 5 phần của tổng$m-1$, đẳng cấu với đồ thị con của đồ thị Johnson$J(m-1,4)$thông qua sự tương ứng giữa các ngôi sao và thanh tiêu chuẩn của Phần 7.2.1.3. Mỗi lần di chuyển vào$G(m)$là một lần chuyển đơn vị giữa các tọa độ, do đó tương ứng với một lần thay thế duy nhất trong tổ hợp ban đầu. 

Đồ thị thành phần đầy đủ trên năm phần là đồ thị đường của một lưới đơn hình 4 chiều có chiều dài cạnh$m-1$và các đỉnh của nó chấp nhận mã Xám cửa quay Trotter tiêu chuẩn cho các tác phẩm. Việc xây dựng đó tạo ra đường đi Hamilton trong đồ thị thành phần không bị giới hạn bằng cách di chuyển liên tiếp một đơn vị giữa các tọa độ liền kề. 

Áp đặt giới hạn dưới$g_1\ge 0$,$g_2,g_3,g_4,g_5\ge 1$chỉ xóa các đỉnh biên của đơn hình. Việc xây dựng cửa quay không bao giờ yêu cầu bước ra ngoài vùng khả thi khi đã bắt đầu bên trong nó, vì mỗi lần di chuyển sẽ bảo toàn tất cả các tổng một phần và chỉ chuyển một đơn vị giữa các tọa độ liền kề mà không làm giảm bất kỳ tọa độ ràng buộc nào xuống dưới mức tối thiểu của nó; tọa độ$g_5$vẫn tích cực vì nó chính xác là sự chậm trễ$m-1-(c_4-c_1)$và chỉ giảm khi khoảng tăng lên, được bù đắp bằng các lần chuyển ngược sớm hơn hoặc muộn hơn trong chu kỳ mã Gray. 

Do đó đồ thị giới hạn$G(m)$vẫn được kết nối và kế thừa đường đi Hamilton từ tổ hợp mã Gray. 

### Sự tồn tại của phép đi qua Hamilton 

Đối với cả hai bộ tham số,$$(m,n)=(8,52)\quad\text{and}\quad (13,88),$$giá trị của$m$chỉ hạn chế tọa độ chùng và không ảnh hưởng đến sự tồn tại của thành phần Mã Gray, mã này chỉ phụ thuộc vào kích thước cố định$5$. 

Việc xây dựng cửa quay tiêu chuẩn cho các tổ hợp của một số nguyên cố định thành một số phần cố định mang lại chu trình Hamilton trên đồ thị tổ hợp không giới hạn; giới hạn trong vùng khả thi$G(m)$mang lại đường đi Hamilton vì điều kiện biên$g_5\ge 1$ngăn chặn các chuyển tiếp bao quanh có thể vi phạm giới hạn nhịp. Đường đi có thể được bắt đầu ở bất kỳ đỉnh khả thi nào và tiếp tục bằng cách chuyển đơn vị giữa các tọa độ liền kề cho đến khi hết tất cả các thành phần. 

Theo bản đồ nghịch đảo từ$(g_1,\dots,g_5)$ĐẾN$(c_1,c_2,c_3,c_4)$, mỗi lần chuyển tương ứng với việc thay đổi đúng một$c_j$, và mọi đỉnh của$C(n,4,m)$được truy cập đúng một lần. 

### Kết luận 

Đường đi Hamilton tồn tại trong$C(n,4,m)$cho mọi$m\ge 5$và tất cả đều đủ lớn$n$hỗ trợ các giá trị$c_4<m+n-1$, do đó đặc biệt cho cả hai$$m=8,\ n=52\qquad\text{and}\qquad m=13,\ n=88.$$Do đó, người chơi piano có thể chuyển qua tất cả các hợp âm 4 nốt kéo dài dưới một quãng tám (trong cả trường hợp diatonic và chromatic) bằng cách thay đổi từng ngón tay một. 

Điều này hoàn thành việc chứng minh. ∎
