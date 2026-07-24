---
title: "CF 103821L - ResliPhobia"
description: "Giả sử $w(x1,ldots,xn)$ biểu thị sự đóng góp của một tiểu số $$(1-p1)^{1-x1}p1^{x1}cdots (1-pn)^{1-xn}pn^{xn}.$$ Việc tối đa hóa số lượng này trên tất cả các phép gán thỏa mãn $f(x1,ldots,xn)=1$ tương đương với việc tối đa hóa tích của các yếu tố cục bộ độc lập dọc theo một đường dẫn trong BDD của…"
date: "2026-07-02T08:25:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103821
codeforces_index: "L"
codeforces_contest_name: "(Aleppo + HAIST + SVU + Private) CPC 2022"
rating: 0
weight: 103821
solve_time_s: 127
verified: false
draft: false
---

[CF 103821L - ResliPhobia](https://codeforces.com/problemset/problem/103821/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$w(x_1,\ldots,x_n)$biểu thị sự đóng góp của một minterm$$(1-p_1)^{1-x_1}p_1^{x_1}\cdots (1-p_n)^{1-x_n}p_n^{x_n}.$$Tối đa hóa số lượng này trên tất cả các bài tập thỏa mãn$f(x_1,\ldots,x_n)=1$tương đương với việc tối đa hóa tích của các yếu tố cục bộ độc lập dọc theo đường dẫn trong BDD của$f$, bởi vì mỗi biến$x_i$đóng góp chính xác một yếu tố chỉ phụ thuộc vào việc nhánh LO hay HI được lấy tại nút có nhãn$i$. 

Hãy để mỗi nút BDD$v$được gắn nhãn theo chỉ số biến$i=V(v)$được gán một giá trị$W(v)$được định nghĩa là sự đóng góp tối đa có thể có từ$v$đến nút đầu cuối$\top$. Đối với các nút chìm, các điều kiện biên là$$W(\top)=1,\qquad W(\bot)=0.$$giá trị$1$Tại$\top$tương ứng với sản phẩm trống, và$0$phản ánh rằng bất kỳ con đường nào đạt tới$\bot$không đóng góp gì cho một giải pháp hợp lệ. 

Đối với một nút nhánh$v$dán nhãn$i$, với người kế vị LO$v_0$và người kế nhiệm HI$v_1$, bất kỳ phép gán nào mở rộng một đường đi qua$v$phải chọn chính xác một trong hai cài đặt biến. Sự đóng góp của việc mở rộng qua cạnh LO là$(1-p_i)W(v_0)$và sự đóng góp thông qua cạnh HI là$p_iW(v_1)$. Sự tiếp nối tối ưu từ$v$do đó thỏa mãn$$W(v)=\max\bigl((1-p_i)W(v_0),\; p_iW(v_1)\bigr).$$Bởi vì BDD là một đồ thị tuần hoàn có hướng được sắp xếp theo các chỉ số thay đổi nên các giá trị$W(v)$có thể được tính theo thứ tự tôpô ngược từ điểm chìm về phía gốc. Sự tái diễn này được xác định rõ ràng vì mọi người kế nhiệm của$v$có chỉ số biến lớn hơn. 

Một lần$W(\text{root})$được tính toán, phép gán thỏa mãn tối ưu sẽ đạt được bằng cách tuân theo các lựa chọn đạt được mức tối đa tại mỗi nút. Bắt đầu từ nút gốc$r$, nếu như$r$là một bồn rửa, nhiệm vụ đã được xác định. Nếu như$r$là một nút nhánh được gắn nhãn$i$, thì nếu$$(1-p_i)W(v_0)\ge p_iW(v_1)$$bộ bài tập$x_i=0$và tiến tới$v_0$, nếu không thì nó đặt$x_i=1$và tiến tới$v_1$. Việc lặp lại quá trình này mang lại một con đường nhất thiết phải kết thúc bằng$\top$, vì bất kỳ đường đi nào có trọng số dương đều tương ứng với phép gán thỏa mãn của$f$. 

Tính đúng đắn xuất phát từ thực tế là mọi phép gán thỏa mãn đều tương ứng một cách khách quan với một nghiệm từ gốc tới$\top$đường dẫn trong BDD và sự đóng góp của các hệ số gán nhân lên dọc theo các cạnh chính xác như trong định nghĩa lặp lại$W(v)$. Do đó, đường dẫn được tính toán sẽ tối đa hóa tổng sản phẩm trong số tất cả các bài tập thỏa mãn. 

Điều này hoàn thành việc chứng minh. ∎
