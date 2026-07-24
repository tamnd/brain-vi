---
title: "CF 103824F - \u6298\u78e8\u738b(phiên bản cứng)"
description: "Đặt $v$ là một nút của BDD có thứ tự rút gọn cho $f$, và đặt $Fv(p)$ biểu thị đa thức độ tin cậy của hàm con được biểu thị tại $v$ dưới chuyên môn hóa $p1=cdots=pn=p$. Giả sử $F'v(p)$ biểu thị đạo hàm của nó đối với $p$."
date: "2026-07-02T08:19:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103824
codeforces_index: "F"
codeforces_contest_name: "2022 Summer Camp of XTU Qualifying Round"
rating: 0
weight: 103824
solve_time_s: 55
verified: false
draft: false
---

[CF 103824F - \u6298\u78e8\u738b(phiên bản cứng)](https://codeforces.com/problemset/problem/103824/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$v$là một nút của BDD có thứ tự giảm cho$f$, và để$F_v(p)$biểu thị đa thức độ tin cậy của hàm con được biểu thị tại$v$thuộc chuyên ngành$p_1=\cdots=p_n=p$. Cho phép$F'_v(p)$biểu thị đạo hàm của nó đối với$p$. 

Đối với các nút chìm, các giá trị được cố định theo định nghĩa của đa thức độ tin cậy. Nếu như$v=\bot$, sau đó$F_v(p)=0$Và$F'_v(p)=0$. Nếu như$v=\top$, sau đó$F_v(p)=1$Và$F'_v(p)=0$. 

Cho phép$v$là một nút nhánh được gắn nhãn bởi biến$x_k$với người kế vị thấp$v_L$và người kế vị cao$v_H$. Đa thức độ tin cậy thỏa mãn sự phân rã được kế thừa từ định nghĩa trong Thuật toán C, vì điều kiện hóa$x_k=0$đóng góp trọng lượng$1-p$và điều hòa$x_k=1$đóng góp trọng lượng$p$. Vì thế$$F_v(p)=(1-p)F_{v_L}(p)+pF_{v_H}(p).$$Phân biệt danh tính này với$p$sản lượng$$F'_v(p)=-(F_{v_L}(p))+(1-p)F'_{v_L}(p)+F_{v_H}(p)+pF'_{v_H}(p).$$Hai lần lặp lại này xác định cả giá trị độ tin cậy và đạo hàm của nó tại mỗi nút khi đã biết giá trị của các nút kế tiếp. 

Quá trình đánh giá trực tiếp được tiến hành theo kiểu duyệt theo thứ tự sau của biểu đồ tuần hoàn có hướng BDD. Mỗi nút được tính một lần vì các BDD có thứ tự giảm xác định các hàm con đẳng cấu, do đó việc chia sẻ đảm bảo rằng cả hai$F_v(p)$Và$F'_v(p)$được xác định nhất quán cho mọi nút đạt được từ gốc. 

Việc tính toán gán cho mỗi nút$v$một cặp số$(F_v, F'_v)$theo sự tái phát. Tại bồn rửa cặp là$(0,0)$hoặc$(1,0)$. Tại các nút nhánh, cặp được hình thành từ các cặp đã được tính toán$v_L$Và$v_H$bằng cách sử dụng các công thức trên. Vì mỗi nút có chính xác hai cung đi ra và đồ thị không theo chu kỳ theo thứ tự thay đổi, nên quá trình truyền từ dưới lên này sẽ đến gốc sau khi tất cả các phụ thuộc được giải quyết. 

giá trị$F(p)$cho hàm Boolean ban đầu$f$là$F_r(p)$tại nút gốc$r$, và đạo hàm của nó là$F'_r(p)$. 

Điều này hoàn thành việc xây dựng Thuật toán C đã sửa đổi để đánh giá đồng thời đa thức độ tin cậy tại$p$và đạo hàm của nó. ∎
