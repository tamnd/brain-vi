---
title: "CF 104264C - Maroc"
description: "Đặt $f(x1,x2,x3,x4,x5)$ là một hàm Boolean và đặt $B{min}(f)$ biểu thị mức tối thiểu, trên tất cả các thứ tự biến, của số lượng nút trong sơ đồ quyết định nhị phân có thứ tự rút gọn, bao gồm cả các nút chìm $bot$ và $top$."
date: "2026-07-01T21:31:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104264
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #9 (Fool-Forces)"
rating: 0
weight: 104264
solve_time_s: 40
verified: false
draft: false
---

[CF 104264C - Morco](https://codeforces.com/problemset/problem/104264/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$f(x_1,x_2,x_3,x_4,x_5)$là một hàm Boolean và để$B_{\min}(f)$biểu thị mức tối thiểu, trên tất cả các thứ tự biến đổi, của số lượng nút trong sơ đồ quyết định nhị phân có thứ tự rút gọn của nó, bao gồm cả các nút chìm$\bot$Và$\top$. 

Để đặt hàng$x_{i_1},\dots,x_{i_5}$, mỗi nút của BDD tương ứng đại diện cho một hàm con riêng biệt của$f$thu được bằng cách sửa tiền tố của biến. Ở độ sâu$k$, một nút như vậy tương ứng với một hàm con trên$5-k$các biến. Kích thước của BDD bằng số lượng các hàm con riêng biệt phát sinh dọc theo tất cả các phép gán từng phần, cùng với hai hàm con không đổi$\bot$Và$\top$. 

Nhiệm vụ là xác định tất cả các hàm Boolean trên năm biến tối đa hóa$B_{\min}(f)$và tính giá trị lớn nhất đó. 

## Giải pháp 

Sửa thứ tự của các biến. BDD được xây dựng theo thứ tự này có một nút cho mỗi hàm con riêng biệt của biểu mẫu$$f(a_1,\dots,a_k,x_{k+1},\dots,x_5), \quad 0 \le k \le 5,$$cùng với việc xác định các hàm con giống nhau bằng cách rút gọn. 

Ở độ sâu$k$, có nhiều nhất$2^k$nhiệm vụ riêng biệt$(a_1,\dots,a_k)$, do đó nhiều nhất$2^k$chức năng phụ ở cấp độ đó. Do đó tổng số nút được giới hạn bởi$$1 + 2 + 4 + 8 + 16 = 2^5 - 1$$các nút bên trong trong cấu trúc cây quyết định chưa được hợp nhất đầy đủ. Sau khi rút gọn, chỉ các hàm con giống hệt nhau mới được hợp nhất, do đó kích thước được tối đa hóa chính xác khi không có hai phép gán từng phần riêng biệt nào tạo ra cùng một hàm con, ngoại trừ khi bị ép buộc bởi hằng số ở các lá. 

Các bồn đóng góp chính xác hai nút,$\bot$Và$\top$. Do đó mọi BDD đều thỏa mãn$$B(f) \le (2^5 - 1) + 2 = 2^5 + 1 = 33.$$Giới hạn trên này đạt được chính xác khi mỗi phép gán một phần mang lại một hàm con riêng biệt và không có hàm con nào trùng khớp với các nút khác nhau của cây quyết định đầy đủ ngoại trừ ở hai lá không đổi. Trong trường hợp đó, không thể rút gọn được ngoài việc hợp nhất các lá cố định giống hệt nhau và mọi nút bên trong của cây quyết định nhị phân hoàn chỉnh vẫn tồn tại trong BDD có thứ tự rút gọn. 

Chức năng như vậy tồn tại. Một hàm Boolean ngẫu nhiên trên năm biến có xác suất dương tất cả$2^k$chức năng phụ ở độ sâu$k$riêng biệt cho mọi$k$, vì các đẳng thức xác định xung đột giữa các hàm con riêng biệt áp đặt các ràng buộc đại số nghiêm ngặt lên bảng chân lý. Bất kỳ hàm nào có thuộc tính này đều đạt được đẳng cấu BDD cho cây quyết định đầy đủ chỉ với hai phần chìm được hợp nhất. 

Đối với bất kỳ hàm nào như vậy, mọi thứ tự biến đều tạo ra tình huống giống nhau: mỗi hạn chế bằng phép gán một phần sẽ tạo ra một hàm con riêng biệt, do đó, việc sắp xếp lại không thể tạo ra các nút dùng chung. Do đó, kích thước BDD tối thiểu đã đạt được sau mỗi lần đặt hàng. 

Như vậy$$B_{\min}(f) = 33$$cho tất cả các chức năng$f$có các hàm con riêng biệt trong tất cả các phép gán từng phần là khác nhau theo từng cặp ngoại trừ các hàm không đổi. Không có chức năng nào có thể vượt quá giá trị này. 

Chức năng tối đa hóa$B_{\min}(f)$chính xác là các hàm Boolean trên năm biến có tập hợp tất cả các đồng yếu tố$$\{ f|_{x_{i_1}=a_1,\dots,x_{i_k}=a_k} \}$$không chứa sự lặp lại ngoại trừ hai hàm hằng số, cho mọi lựa chọn sắp xếp và mọi phép gán từng phần. 

Do đó giá trị lớn nhất có thể có của$B_{\min}(f)$là$$\boxed{33}.$$## Xác minh 

Cây quyết định nhị phân đầy đủ về chiều cao$5$có số lượng nút nội bộ$$\sum_{k=0}^{4} 2^k = 2^5 - 1 = 31.$$Thêm hai bồn rửa$\bot$Và$\top$cho$33$tổng số nút, khớp với giới hạn. 

Không có BDD thứ tự rút gọn nào có thể vượt quá kích thước đầy đủ của cây quyết định vì mỗi nút tương ứng với một hàm con riêng biệt và có nhiều nhất$2^5$bài tập một phần, do đó nhiều nhất$31$vị trí không chìm cộng với hai bồn. 

## Ghi chú 

Các hàm cực trị chính xác là những hàm có độ phức tạp đồng yếu tố tối đa, trong đó cây phân rã Shannon không có bài toán con lặp lại ngoại trừ tại các hằng số. Đối với lớn hơn$n$, lập luận tương tự mang lại mức tối đa chung$2^n + 1$, đạt được chính xác khi BDD không chứa phần chia sẻ nào ngoài hai phần chìm.
